# 22. Deployment Model

**Document Version:** 1.0
**Last Updated:** 1448 AH / 2026 CE
**Owner:** Deployment Operations (Dept-22)
**Classification:** Internal — Infrastructure
**Status:** Final

---

## Table of Contents

1. [Deployment Philosophy](#1-deployment-philosophy)
2. [Agent Packaging](#2-agent-packaging)
3. [Deployment Environments](#3-deployment-environments)
4. [Deployment Strategies](#4-deployment-strategies)
5. [Deployment Pipeline (CI/CD)](#5-deployment-pipeline-cicd)
6. [Deployment of Agent Groups](#6-deployment-of-agent-groups)
7. [Configuration Management](#7-configuration-management)
8. [Database and State Deployment](#8-database-and-state-deployment)
9. [Model Deployment](#9-model-deployment)
10. [Rollback Procedures](#10-rollback-procedures)
11. [Health Checks and Readiness](#11-health-checks-and-readiness)
12. [Multi-Region Deployment](#12-multi-region-deployment)
13. [Deployment Monitoring and Verification](#13-deployment-monitoring-and-verification)
14. [Deployment Governance](#14-deployment-governance)
15. [Special Deployment Cases](#15-special-deployment-cases)

---

## 1. Deployment Philosophy

### 1.1 Core Principles

The deployment philosophy of the Autonomous AI Company is built on the foundation that all 8,500 AI agents must be deployable as a single coordinated system, not as isolated components. Every deployment event, whether it involves one agent or one hundred, is treated as a system-wide operation that must preserve the integrity, availability, and correctness of the entire platform.

**Ten Foundational Principles:**

| # | Principle | Description |
|---|---|---|
| 1 | Immutable Infrastructure | Every deployment creates brand-new instances. No in-place updates, no hot-patching of running containers, no configuration drift. Once an instance is deployed, it is never modified; replacement is the only mutation. |
| 2 | Zero-Downtime | No user-facing agent or internal service shall experience downtime during deployment. This applies to all 8,500 agents across all 33 departments. |
| 3 | Reversibility First | Every deployment must be reversible within 60 seconds for standard agents. The rollback path must be designed before the deployment begins. |
| 4 | Graduated Risk | Higher-risk deployments follow stricter validation paths. Risk is assessed per agent type, with canary, blue-green, and rolling strategies applied proportionally. |
| 5 | Observability | Every deployment is fully observable. Metrics, logs, and traces flow in real-time. Deployment success or failure is determined by data, not intuition. |
| 6 | Repeatability | Deployments are automated, scripted, and version-controlled. No manual steps in production. The same pipeline runs in dev, staging, pre-prod, and production. |
| 7 | Least Privilege | Deployment permissions follow the principle of least privilege. No individual can deploy to production without approval. Automated gates prevent unauthorized changes. |
| 8 | Data Integrity | Stateful deployments preserve data integrity at all times. Schema migrations are backward-compatible. No deployment shall cause data loss or corruption. |
| 9 | Environment Parity | All environments are as similar to production as possible. Configuration differs, infrastructure does not. What works in staging will work in production. |
| 10 | Continuous Improvement | Post-deployment reviews are mandatory. Every incident, near-miss, or delay generates an improvement item for the deployment pipeline itself. |

### 1.2 Immutable Infrastructure Model

All agent deployments use immutable infrastructure. When a new version of an agent is required, the following occurs:

1. A new container image is built from scratch with all dependencies included
2. The image is signed, scanned, and pushed to the private registry
3. New instances are provisioned using the new image
4. Old instances are drained of traffic and terminated
5. No configuration is ever modified on a running instance

This eliminates configuration drift, ensures reproducibility, and guarantees that the deployed artifact is exactly what was tested. The build artifact (container image) is the single source of truth — what was tested in staging is byte-for-byte identical to what runs in production.

### 1.3 Zero-Downtime Guarantee

Zero-downtime deployment is non-negotiable for all agent types. The following mechanisms ensure this:

- **Stateless Agents (6,800 agents):** Rolling updates with health checks. New pods start before old pods are terminated. The Kubernetes deployment controller ensures at least the desired number of replicas are always running.
- **Stateful Agents (1,200 agents):** Blue-green deployments with persistent volume claims. The entire new stack (including databases if applicable) is provisioned alongside the old. Traffic switches only after the new stack passes all health checks.
- **Model-Serving Agents (500 agents):** Shadow deployments where the new model version receives traffic copies before serving live traffic. Model loading is pre-warmed; cold-start latency is eliminated.
- **Coordinators and Orchestrators (all 130 team leads' coordinator agents):** Blue-green with canary. The coordinator agent is the brain of each team; it is deployed with extra caution using a two-phase canary process.

### 1.4 Rollback Capability

Every deployment must have a rollback path that can be executed within 60 seconds for standard agents. This is achieved through:

- **Blue-green deployments:** Traffic is switched back to the previous stack by updating the load balancer selector (sub-second rollback).
- **Rolling deployments:** `kubectl rollout undo` reverts to the previous ReplicaSet (typically < 30 seconds).
- **Canary deployments:** The canary is simply scaled to zero; remaining traffic continues to the stable version (instantaneous).
- **Stateful deployments:** A pre-deployment snapshot is taken. Rollback restores the snapshot and switches traffic back (target: < 5 minutes).

The rollback time is measured from the moment the rollback trigger fires to the moment 100% of traffic is served by the previous version.

### 1.5 Risk Classification for Deployments

Every agent and every deployment is classified according to risk:

| Risk Level | Agent Types | Rollback Target | Deployment Strategy | Approval Required |
|---|---|---|---|---|
| **Critical** | Executive agents (CE-AI, 7 EVPs), Payment agents, Auth agents, Data persistence agents | < 30 seconds | Blue-green + canary | CAB + Dept Manager |
| **High** | Department heads, Team leads, Curriculum agents, Assessment agents | < 60 seconds | Blue-green or canary | Dept Manager |
| **Medium** | Standard workflow agents, Content generation agents, Analysis agents | < 120 seconds | Rolling or canary | Team Lead |
| **Low** | Logging agents, Monitoring agents, Reporting agents, Internal tools | < 5 minutes | Rolling | Auto-deploy |

---

## 2. Agent Packaging

### 2.1 Container Image Architecture

Every AI agent in the organization is packaged as an OCI-compliant container image. The image contains everything the agent needs to run, with no external dependencies except for runtime services (databases, message queues, model servers).

**Image Layers (bottom to top):**

```
Layer 7: Agent Application Code          (ephemeral, changes per build)
Layer 6: Runtime Configuration           (environment-specific, injected at deploy)
Layer 5: Agent Dependencies (pip/npm)    (changes with requirements updates)
Layer 4: Model Weights / Model Reference (changes with model updates)
Layer 3: Agent Runtime (Python/Node/Go)  (changes with version upgrades)
Layer 2: System Libraries                (changes with OS updates)
Layer 1: Base Image (hardened minimal)   (changes with security patches)
```

**Base Image Specifications:**

| Component | Specification |
|---|---|
| Base OS | Alpine Linux (hardened) or distroless image |
| Size | < 50 MB for base |
| CVE Patches | Automated weekly rebuild |
| Shell | Removed in production images (distroless) |
| Package Manager | Removed in production images |
| Non-Root User | Enforced, UID 1001 (appuser) |
| Read-only RootFS | Enforced via security context |

### 2.2 Image Components Detail

**Standard Agent Image (< 2 GB):**

| Component | Typical Size | Description |
|---|---|---|
| Base image (distroless) | 20 MB | Google distroless base, minimal attack surface |
| Python runtime (3.11) | 150 MB | Slim Python with only needed stdlib modules |
| Agent SDK and libraries | 200 MB | Internal agent framework, gRPC, protobuf, message queue client |
| Application code | 5-50 MB | Compiled bytecode or bundled JS |
| Model reference (TF-IDF, small BERT) | 500 MB - 1.5 GB | Quantized or distilled model weights |
| Configuration bundle | < 1 MB | Default config, schema files |
| **Total** | **~900 MB - 1.8 GB** | Well within 2 GB limit |

**Large Model Agent Image (< 10 GB):**

| Component | Typical Size | Description |
|---|---|---|
| Base image (minimal Ubuntu) | 100 MB | Heavier base needed for CUDA/cuDNN |
| CUDA runtime, cuDNN, TensorRT | 2.5 GB | GPU compute dependencies |
| Python runtime + ML framework (PyTorch) | 800 MB | Optimized PyTorch build |
| Model weights (LLaMA, Falcon, etc.) | 4-7 GB | 4-bit or 8-bit quantized |
| Agent application code | 20 MB | Thin wrapper around model |
| Model server (vLLM, TGI, Triton) | 300 MB | Optimized inference server |
| **Total** | **~7.5 - 9.5 GB** | Within 10 GB limit |

### 2.3 Image Versioning

Images follow strict semantic versioning with additional build metadata:

```
<org>/<agent-name>:<major>.<minor>.<patch>-<build-metadata>

Examples:
  aic/quran-translator:2.1.3-b20260709.1430
  aic/curriculum-designer:1.0.1-b20260708.2155
  aic/student-assessment:3.2.0-b20260707.0812
```

**Versioning Rules:**

| Component | Increment When | Example |
|---|---|---|
| **Major** | Breaking changes to agent API, behavior, or model architecture | 1.0.0 → 2.0.0 |
| **Minor** | New features, new models, new capabilities (backward-compatible) | 1.0.0 → 1.1.0 |
| **Patch** | Bug fixes, security patches, performance improvements | 1.0.0 → 1.0.1 |
| **Build metadata** | Build date + sequential number | b20260709.1430 |

Every image tag is immutable. Once pushed to the registry, a tag cannot be overwritten. New builds always produce new tags.

### 2.4 Image Registry

**Private Registry Architecture:**

```
Private Container Registry
  - Harbor (Registry)
  - Clair (Scanner)
  - Notary (Signing)
  - Trivy (CVE)
  - Cosign (Sig)
```

**Registry Specifications:**

| Feature | Specification |
|---|---|
| Platform | Harbor (self-hosted, air-gapped capable) |
| Vulnerability Scanner | Clair + Trivy (dual scan for coverage) |
| Image Signing | Cosign (Sigstore) |
| Authentication | OIDC with Vault, robot accounts for CI |
| Retention Policy | Keep last 50 tags per agent, last 30 days |
| Geo-Replication | Active-active in 3 regions (US, EU, ME) |
| Storage | S3-compatible object store (MinIO) |
| Rate Limiting | 1,000 pulls/min per robot account |
| Audit Log | All push/pull operations logged to SIEM |

### 2.5 Image Size Optimization

**Standard Agents (< 2 GB) — Optimization Techniques:**

| Technique | Impact | Description |
|---|---|---|
| Multi-stage builds | 40-60% size reduction | Builder stage with full SDK, runtime stage with only artifacts |
| Distroless base | 80% reduction vs Ubuntu | No shell, no package manager, no unnecessary binaries |
| Layer squashing | 15-25% reduction | Combine layers to reduce metadata overhead |
| Dependency pruning | 30-50% reduction | `pip install --no-cache-dir`, remove tests and docs |
| Model quantization | 60-80% reduction for models | INT8/FP16 quantization, knowledge distillation |
| Model on shared volume | Variable | Reference model from shared PVC, not inside image |

**Large Model Agents (< 10 GB) — Optimization Techniques:**

| Technique | Impact | Description |
|---|---|---|
| 4-bit quantization | 75% reduction | NF4 or GPTQ quantization for LLMs |
| Model sharding | Removes duplication | Shard across multiple GPUs, load only needed shards |
| Shared model cache | Variable | Pre-downloaded models on node-local SSD |
| Sparse model loading | 50% reduction | Load only active parameters |
| Model decompression at startup | 2x compression | gzip/bzip2 model files, decompress on first start |

### 2.6 Image Signing and Verification

All images must be signed before deployment. Deployment fails if signature verification fails.

**Signing Process:**

1. CI pipeline builds image and pushes to staging registry
2. CI pipeline signs the image digest using Cosign with the build key
3. Image digest, signature, and attestation are stored in the registry
4. Deployment pipeline verifies the signature before pulling the image
5. Verification checks: (a) signature matches trusted public key, (b) attestation matches build metadata, (c) image digest matches what was tested

**Key Management:**

| Key Type | Purpose | Rotation |
|---|---|---|
| Build Key (short-lived) | Sign images during CI | Per-build (ephemeral) |
| Deployment Key | Verify images during deploy | Monthly |
| Root Key | Sign deployment keys | Yearly, offline |
| Recovery Key | Emergency verification | Offline, vault |

**Verification Enforcement:**

- **Dev environment:** Warning on invalid signature, deployment proceeds
- **Staging environment:** Block deployment if signature invalid
- **Pre-prod environment:** Block deployment if signature invalid
- **Production environment:** Block deployment if ANY check fails

---

## 3. Deployment Environments

### 3.1 Environment Overview

The organization maintains four distinct deployment environments, each with specific purposes, configurations, and access controls. Every environment mirrors production as closely as possible, with differences only in scale, data, and configuration.

| Property | Development | Staging | Pre-Production | Production |
|---|---|---|---|---|
| **Purpose** | Rapid iteration, feature testing | Integration testing, performance testing | Final validation, canary | Live user traffic |
| **Scale** | 5% of prod | 10% of prod | 100% (same as prod) | 100% |
| **Agent Count** | ~425 agents | ~850 agents | ~8,500 agents | ~8,500 agents |
| **Clusters** | 3 shared clusters | 1 dedicated cluster | 2 dedicated clusters | 8 clusters (multi-region) |
| **Data** | Synthetic + anonymized | Anonymized subset | Full anonymized | Real user data |
| **SLA** | None | 99.5% | 99.9% | 99.99% |
| **Deploy Frequency** | 50+ times/day | 20+ times/day | 5-10 times/day | 3-5 times/day |
| **Approval** | None | Team Lead | Dept Manager | CAB + Dept Manager |

### 3.2 Development Environment (Dev)

**Purpose:** Allow individual teams and agents to develop, test, and iterate rapidly without impacting other teams.

**Architecture:**
- Shared Kubernetes cluster with 100 nodes (10 dedicated per department, remaining shared)
- Each of the 33 departments has a dedicated namespace with resource quotas
- Each of the 130 teams has a sub-namespace within their department
- A single team may deploy up to 20 agents simultaneously
- Total namespace quota: 32 CPU cores, 128 GB RAM, 1 TB storage per team

**Data Isolation:**
- Synthetic data generated by the Data Engineering department
- Anonymized snapshots of production data (refreshed weekly, no PII)
- Each team gets a separate database instance (PostgreSQL schema per team)
- No cross-team data access without explicit permissions

**Access Control:**
| Role | Permissions |
|---|---|
| Developer | Deploy to own team namespace, view logs, exec into containers |
| Team Lead | Deploy to team namespace, configure resources, view metrics |
| Dept Manager | Deploy to department namespace, override resource limits |
| DevOps | Cluster-level admin, namespace creation, infrastructure changes |

**Deployment Behavior:**
- Auto-deployed on every PR merge to `develop` branch
- PR must pass: unit tests, linting, container build, vulnerability scan (critical only)
- Deployment completes within 5 minutes
- Failed deployments auto-revert; developer receives notification
- No health check gating (agent must start, but may fail readiness)

### 3.3 Staging Environment (Staging)

**Purpose:** Validate that agents work correctly together at scale, identify integration issues, and measure baseline performance.

**Architecture:**
- Dedicated Kubernetes cluster with 200 nodes
- 1:1 replica of production topology (same service mesh, same message queues, same databases)
- All 33 departments and 130 teams present at 10% capacity
- Service mesh (Istio) fully configured with same policies as production
- Monitoring stack identical to production (Prometheus, Grafana, Loki, Tempo)

**Data Isolation:**
- Full copy of production data with PII anonymized (tokenization + masking)
- Data refreshed from production every 24 hours
- All external APIs connected to sandbox endpoints
- Payment gateways in test mode
- Email/SMS notifications suppressed (logged only)

**Access Control:**
| Role | Permissions |
|---|---|
| Developer | Read-only logs and metrics |
| Team Lead | Deploy own team's agents, view all logs |
| Dept Manager | Deploy department agents, configure tests |
| QA Team | Execute test suites, view results |
| DevOps | Full access |

**Integration Testing:**
- All 130 teams' agents must pass integration tests before staging deployment is considered successful
- Cross-team interaction tests (e.g., Curriculum → Assessment → Reporting flow)
- Message queue load testing (Kafka throughput at 10% production load)
- Database connection pooling validation

**Performance Baseline:**
- Every staging deployment captures: P50/P95/P99 latency, throughput, error rate, resource usage
- Results compared to previous deployment: regression > 10% blocks promotion to pre-prod
- Baseline stored in InfluxDB and accessible via Grafana dashboard

### 3.4 Pre-Production Environment (Pre-Prod)

**Purpose:** Final validation environment where deployments are verified under production-equivalent conditions before reaching live users.

**Architecture:**
- Two dedicated clusters (active-active, same region as production primary)
- 1:1 scale with production: 8,500 agent instances, 2,000 nodes
- Same hardware specifications as production (same CPU model, same GPU SKU)
- Same network topology, same firewall rules, same load balancers
- Same external service endpoints (but pointing to sandboxes)

**Data Isolation:**
- Full production dataset with all PII anonymized
- Realistic data volume (30M+ user records, 500M+ learning interactions)
- External API calls use sandbox/test endpoints
- No external user traffic — only synthetic and QA traffic
- Database write operations preserved (not rolled back) for data integrity testing

**Access Control:**
| Role | Permissions |
|---|---|
| Team Lead | Trigger deployment, view dashboards |
| Dept Manager | Approve deployment, view all |
| QA Manager | Execute final validation |
| DevOps | Full access |
| Executive | Read-only dashboards |

**Canary Validation:**
1. Deploy to 1% of pre-prod agents (5 minutes observation)
2. If pass, deploy to 20% (5 minutes observation)
3. If pass, deploy to 100% (10 minutes observation)
4. Full regression suite runs against 100% deployment
5. Only after all steps pass is deployment approved for production

**Promotion Gating:**

| Gate | Check | Fail Action |
|---|---|---|
| Build Verification | Image signature valid, CVE scan passed (no critical/high) | Block |
| Unit Tests | 100% test suite pass rate | Block |
| Integration Tests | All cross-agent flows pass | Block |
| Performance | No regression > 10% in P95 latency or throughput | Warn, require override |
| Security Scan | SAST/DAST scan passed | Block |
| Model Evaluation | Model accuracy within 1% of baseline | Block if > 2% |
| Load Test | 2x expected load sustained for 15 minutes | Block |

### 3.5 Production Environment (Production)

**Purpose:** Serve live user traffic with maximum reliability, availability, and performance.

**Architecture:**
- 8 Kubernetes clusters across 3 regions:
  - **Primary Region (Middle East):** 3 clusters, 1,500 nodes, active for all workloads
  - **Secondary Region (Europe):** 3 clusters, 1,200 nodes, active for read-only, warm standby for write
  - **DR Region (Southeast Asia):** 2 clusters, 800 nodes, cold standby
- Total: ~8,500 agent instances across 3,500 nodes
- Multi-AZ within each region (3 availability zones minimum)

**Data:**
- Real user data: 30M+ registered users, 50M+ Islamic education content items
- Full transaction history, payment records, learning progress
- PII encrypted at rest and in transit
- Data residency compliance: user data stored in region of origin

**SLA Targets:**
| Metric | Target |
|---|---|
| Uptime | 99.99% (52.56 minutes downtime/year) |
| P95 Response Time | < 200 ms for standard agents |
| P95 Response Time | < 2 seconds for model inference agents |
| Error Rate | < 0.1% |
| Throughput | 100,000 requests/second (aggregate) |
| Deployment Success Rate | > 99.5% |

**Access Control (Strictest):**
| Role | Permissions |
|---|---|
| Team Lead | View-only dashboards |
| Dept Manager | Approve deployments, view logs (no exec) |
| CAB Member | Approve production changes |
| DevOps On-Call | Emergency access (logged, audited) |
| CE-AI | Override for emergency (logged, audited) |

### 3.6 Environment Promotion Flow

```
                    ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
                    │    Dev    │───>│  Staging  │───>│ Pre-Prod │───>│Production│
                    │           │    │           │    │          │    │          │
                    │ Auto on   │    │ Auto on   │    │ Manual   │    │ CAB      │
                    │ PR merge  │    │ staging   │    │ approval │    │ approval │
                    │           │    │ success   │    │ + gates  │    │ + gates  │
                    └──────────┘    └──────────┘    └──────────┘    └──────────┘
                         │               │               │               │
                         ▼               ▼               ▼               ▼
                      5 min            10 min          15 min          30 min
                      (build+test)     (integration)    (validation)    (canary+full)
```

**Promotion Rules:**

| Step | From | To | Trigger | Duration | Gates |
|---|---|---|---|---|---|
| 1 | PR Merge | Dev | Auto on merge | 5 min | Build passes, unit tests pass |
| 2 | Dev | Staging | Auto on dev success | 10 min | All integration tests pass |
| 3 | Staging | Pre-Prod | Manual by Team Lead | 15 min | Perf baseline OK, security scan OK |
| 4 | Pre-Prod | Production | Manual by Dept Manager + CAB | 30 min | All gates pass, approvals obtained |

**Hotfix/Expedited Path (Emergency Only):**

```
PR Merge → Dev (auto) → Pre-Prod (manual, CE-AI approval) → Production (CE-AI approval)
```

This path bypasses staging. Used only for critical security patches or platform outages. Must be authorized by CE-AI and two EVPs.

---

## 4. Deployment Strategies

### 4.1 Strategy Overview

The organization employs six distinct deployment strategies, selected based on the risk level, agent type, impact scope, and required rollback speed of each deployment.

### 4.2 Rolling Update

**Description:** Replace agent instances gradually, batch by batch, with health checks between each batch.

**Applicable To:** Low-risk agents, stateless agents, internal tooling agents.

**Parameters:**
| Parameter | Default | Range |
|---|---|---|
| Batch size | 10% of total instances | 5% - 25% |
| Batch interval | 30 seconds | 10 - 120 seconds |
| Max surge | 25% | 10% - 50% |
| Max unavailable | 0% | 0% - 10% |
| Health check wait | 30 seconds | 10 - 60 seconds |

**Process:**
1. New ReplicaSet created with 1 replica (surge)
2. Wait for new pod to pass readiness probe
3. Wait health check wait duration (30s default)
4. If healthy: scale down old ReplicaSet by 1 batch
5. Wait for old pod termination to complete
6. Scale up new ReplicaSet by 1 batch
7. Repeat steps 2-6 until all instances replaced
8. Old ReplicaSet retained for rollback (scaled to 0)

**Rollback:**
- `kubectl rollout undo` immediately scales up old ReplicaSet and scales down new
- Rollback time: < 30 seconds for typical deployments
- If more than 3 batches fail health checks, rollout automatically pauses

### 4.3 Blue-Green Deployment

**Description:** Maintain two full production stacks (blue = current, green = new). Switch traffic from blue to green when green is ready.

**Applicable To:** High-risk agents, stateful agents, critical infrastructure.

**Architecture:**
```
                      ┌──────────────┐
                      │  Load Balancer │
                      │  (Traffic Switch)│
                      └──────┬───────┘
                             │
              ┌──────────────┼──────────────┐
              │              │              │
              ▼              │              ▼
     ┌────────────────┐      │     ┌────────────────┐
     │   Blue Stack    │      │     │  Green Stack    │
     │  (Current)      │      │     │  (New Version)  │
     │  8,500 agents   │      │     │  8,500 agents   │
     │  Serving traffic │     │     │  Idle / Testing  │
     └────────────────┘      │     └────────────────┘
              │              │              │
              ▼              │              ▼
     ┌────────────────┐      │     ┌────────────────┐
     │   Blue DB       │      │     │  Green DB       │
     │  (Active)       │      │     │  (Replica, sync)│
     └────────────────┘      │     └────────────────┘
```

**Parameters:**
| Parameter | Default | Range |
|---|---|---|
| Parallel stacks | 2 | 2 (blue + green) |
| Traffic switch | Instant (DNS/LB update) | Instant or gradual |
| Warm-up period | 5 minutes | 2 - 30 minutes |
| Validation period | 10 minutes | 5 - 60 minutes |
| Resource overhead | 2x production | 2x - 3x (during switch) |

**Process:**
1. Green stack is fully provisioned alongside blue
2. Green stack runs health checks and smoke tests (5 minutes)
3. Green stack begins receiving a copy of traffic (shadow mode, 5 minutes)
4. Metrics compared between blue and green (error rate, latency, throughput)
5. If green passes: traffic switched from blue to green via load balancer update
6. Blue retained for 30 minutes (rollback window)
7. After 30 minutes: blue stack destroyed, green becomes new blue

**Rollback:**
- If green fails during step 3-4: green destroyed, blue continues (instant, no user impact)
- If green fails in step 5 (after traffic switch): update load balancer back to blue (sub-second)
- Full rollback time: < 5 seconds for traffic switch
### 4.4 Canary Deployment

**Description:** Gradually shift traffic from old version to new version in controlled percentages, with automated promotion based on metrics.

**Applicable To:** High and medium-risk agents, model updates, new feature rollouts.

**Traffic Progression:**
```
Time:    T0        T+5m      T+10m     T+15m     T+20m     T+25m
Old:    100%      99%       95%       80%       50%       0%
New:     0%        1%        5%       20%       50%      100%
                    │         │         │          │         │
                    ▼         ▼         ▼          ▼         ▼
                Metrics: Metrics:  Metrics:   Metrics:  Full
                Compare  Compare   Compare    Compare   Rollout
                OK       OK        OK         OK
```

**Phases:**
| Phase | Traffic to New | Duration | Evaluation Criteria |
|---|---|---|---|
| 1 — Shadow 1 | 1% | 5 minutes | Error rate < 0.5%, no crash loops |
| 2 — Small Canary | 5% | 10 minutes | Error rate < 0.1%, P95 latency within 10% of baseline |
| 3 — Medium Canary | 20% | 15 minutes | Error rate < 0.1%, P99 latency within 20%, throughput stable |
| 4 — Large Canary | 50% | 15 minutes | All metrics within tolerance, no user complaints |
| 5 — Full Rollout | 100% | N/A | Deployment complete |

**Automated Rollback Conditions (at any phase):**
| Metric | Threshold | Action |
|---|---|---|
| Error rate | > 0.5% (new vs old baseline) | Immediate rollback |
| P95 latency | > 2x baseline | Immediate rollback |
| P99 latency | > 3x baseline | Immediate rollback |
| Health check failures | > 5% of new instances | Immediate rollback |
| 5xx responses | > 1% of new instance responses | Immediate rollback |
| Model accuracy | > 2% degradation | Immediate rollback (model deploys) |

### 4.5 A/B Testing Deployment

**Description:** Two versions of an agent run simultaneously for an extended period, with traffic split by experiment parameters (user cohort, region, device type, etc.).

**Applicable To:** Feature experimentation, UX changes, model comparison, algorithm changes.

**Traffic Split Dimensions:**
| Dimension | Example | Use Case |
|---|---|---|
| User ID hash | users with ID ending in 0-4 vs 5-9 | 50/50 random split |
| Region | ME region vs EU region | Geographic rollback testing |
| User tier | Free tier vs Premium tier | Feature availability testing |
| Device type | Mobile vs Desktop | UI/UX experimentation |
| Language | Arabic vs English | Language-specific model testing |
| Cohort | New users vs Returning users | Onboarding flow testing |

**Configuration Example (logical structure):**
```
Experiment ID: ab-20260709-content-quality
Agent: Content Recommendation Agent
Duration: 7 days
Split:
  - Version: v2.1.3 (control, 50% of users)
  - Version: v2.2.0 (treatment, 50% of users)
Dimensions: user_id_hash % 2
Metrics:
  primary: click_through_rate
  secondary: [session_duration, user_satisfaction_score, content_completion_rate]
Promotion Criteria:
  - Treatment must exceed control by >= 5% in primary metric (statistically significant)
  - Treatment must not degrade any secondary metric by > 2%
  - Treatment error rate must be within 0.1% of control
```

### 4.6 Feature Flags (Feature Toggles)

**Description:** Deploy new code with features disabled via toggles. Enable features gradually as confidence increases, without redeployment.

**Applicable To:** All deployments as a supplement to other strategies, especially for non-structural changes.

**Flag Types:**
| Type | Lifetime | Example | Description |
|---|---|---|---|
| Release Toggle | Days-Weeks | new-curriculum-engine | Hide incomplete feature, enable when ready |
| Experiment Toggle | Days-Months | recommendation-model-v2 | A/B testing experiment flag |
| Ops Toggle | Hours-Days | disable-heavy-analytics | Kill switch for performance issues |
| Permission Toggle | Permanent | premium-features-flag | Feature access based on user tier |
| Migration Toggle | Days-Weeks | use-new-db-schema | Gradual database migration |

**Gradual Rollout via Flags:**
```
Day 1:  Feature flag enabled for internal team only (0.1% of users)
Day 2:  Enabled for 1% of users, monitoring
Day 3:  Enabled for 5% of users, monitoring
Day 4:  Enabled for 20% of users, monitoring
Day 5:  Enabled for 50% of users, monitoring
Day 6:  Enabled for 100% of users, flag removed from code in next release
```

**Kill Switch Protocol:**
- Every feature flag can act as a kill switch
- If a feature causes issues, the flag is turned off (no redeployment needed)
- Kill switch activation time: < 60 seconds (flag propagation delay)
- All critical features have kill switches

### 4.7 Shadow Deployment

**Description:** Deploy a new version of an agent that receives a copy of live traffic but does not serve responses to users. Responses are compared to the current version for validation.

**Applicable To:** High-risk model updates, critical infrastructure changes, new agent types.

**Process:**
1. Shadow agent deployed with same input routing as current agent
2. Load balancer duplicates requests (or service mesh mirrors traffic) to shadow
3. Current agent serves user; shadow agent processes but discards response
4. Comparison engine compares: response quality, completion time, error patterns
5. If shadow outperforms current across 10,000+ requests: shadow promoted to production
6. If shadow underperforms: results analyzed, shadow either improved or discarded

**Metrics Comparison:**
| Metric | Current | Shadow | Threshold for Promotion |
|---|---|---|---|
| Response accuracy | 94.2% | 96.1% | Shadow > Current + 1% |
| P95 latency | 180ms | 195ms | Shadow < Current + 20% |
| Error rate | 0.08% | 0.05% | Shadow < Current |
| Content relevance score | 8.4/10 | 8.7/10 | Shadow > Current |

### 4.8 Strategy Selection Matrix

| Agent Type / Risk Level | Recommended Strategy | Alternative | Rollback Speed | Resource Cost |
|---|---|---|---|---|
| **CE-AI Agent** (Critical) | Blue-Green + Canary (1%) | Blue-Green only | < 5 seconds | 2x during deploy |
| **EVP Agents** (7) (Critical) | Blue-Green + Canary (5%) | Canary only | < 5 seconds | 2x during deploy |
| **Payment Agents** (Critical) | Blue-Green | Shadow + Canary | < 5 seconds | 2x during deploy |
| **Auth Agents** (Critical) | Blue-Green | N/A | < 5 seconds | 2x during deploy |
| **Department Head Agents** (High) | Canary (5% → 20% → 100%) | Blue-Green | < 60 seconds | 1.2x - 2x |
| **Team Lead Agents** (High) | Canary (5% → 100%) | Rolling | < 60 seconds | 1.2x - 1.5x |
| **Curriculum Agents** (High) | Canary (1% → 10% → 100%) | Shadow + Canary | < 60 seconds | 1.2x - 1.5x |
| **Assessment Agents** (High) | Canary (5% → 20% → 100%) | Blue-Green | < 60 seconds | 1.2x - 2x |
| **Content Agents** (Medium) | Rolling (batch 10%) | Canary (1% → 100%) | < 120 seconds | 1.1x |
| **Analysis Agents** (Medium) | Rolling (batch 10%) | Canary | < 120 seconds | 1.1x |
| **Notification Agents** (Low) | Rolling (batch 25%) | Recreate | < 5 minutes | 1.0x |
| **Logging Agents** (Low) | Rolling (batch 25%) | Recreate | < 5 minutes | 1.0x |
| **Model Update** (High-Critical) | Shadow + Canary | A/B Testing | < 5 seconds | 2x - 3x |
| **Feature Flag Change** (All) | No deployment needed | Config change | Instant | 0x |
| **New Department Launch** (Critical) | Blue-Green, phased by team | Canary per team | < 5 minutes | 2x during launch |
| **Emergency Patch** (Critical) | Blue-Green (expedited) | Rolling (hotfix) | < 30 seconds | 2x |

---

## 5. Deployment Pipeline (CI/CD)

### 5.1 Pipeline Architecture

The deployment pipeline is a fully automated, multi-stage CI/CD system that takes code from commit to production deployment with automated gates, approvals, and rollback capabilities at every stage.

### 5.2 Stage 1: Source

**Trigger:** Git push to any branch

**Actions:**
1. Webhook event triggers pipeline
2. Branch determination: `develop` → dev deploy, `staging/*` → staging deploy, `pre-prod/*` → pre-prod deploy, `main` → prod deploy
3. Commit SHA recorded as build identifier
4. Changed files analyzed to determine which agents need rebuilding (monorepo with per-agent change detection)

**Branch Mapping:**
| Branch Pattern | Target Environment | Deployment Permission | Gate |
|---|---|---|---|
| `develop` | Dev | Auto | PR must pass checks |
| `staging/*` | Staging | Team Lead | Dev deploy must have passed |
| `pre-prod/*` | Pre-Prod | Dept Manager | Staging deploy must have passed |
| `main` | Production | CAB + Dept Manager | Pre-prod deploy must have passed |
| `hotfix/*` | Pre-Prod (expedited) | CE-AI approval | Emergency path |

### 5.3 Stage 2: Build (CI)

**Trigger:** Source analysis complete

**Duration:** 5 minutes (standard), 15 minutes (large model agents)

**Pipeline Steps:**
1. Checkout code (30s)
2. Determine changed agents/modules (10s)
3. Pull base images from cache (30s)
4. Build container image(s) — multi-stage (3-12 min)
5. Run unit tests inside build container (1-3 min)
6. Tag image with version + commit SHA (5s)
7. Push image to private registry (30s)
8. Record image digest in build metadata (5s)

**Output Artifacts:**
| Artifact | Location | Description |
|---|---|---|
| Container Image | Private Registry (Harbor) | `registry.aic.internal/<agent>:<version>-<sha>` |
| Build Metadata | Build Artifacts (S3) | JSON file with commit SHA, image digest, build timestamp |
| Test Results | Build Artifacts (S3) | JUnit XML format |
| SBOM (Software Bill of Materials) | Build Artifacts (S3) | CycloneDX format |

### 5.4 Stage 3: Vulnerability Scan

**Trigger:** Image pushed to registry

**Duration:** 3 minutes

**Pipeline Steps:**
1. Clair scan of image layers (1 min)
2. Trivy scan of OS packages + application dependencies (1 min)
3. Compare CVEs against policy (30s)
4. Generate SBOM (30s)
5. Sign image with Cosign (30s)

**CVE Policy (enforced at this stage):**
| Severity | Dev | Staging | Pre-Prod | Production |
|---|---|---|---|---|
| Critical | Block | Block | Block | Block |
| High | Warn | Block | Block | Block |
| Medium | Warn | Warn | Block | Block |
| Low | Allow | Warn | Warn | Block (cumulative > 10) |

### 5.5 Stage 4: Test

**Trigger:** Build and scan pass

**Duration:** 10 minutes (standard), 30 minutes (model agents)

**Test Categories:**
| Test Type | Duration | Coverage | Fail Action |
|---|---|---|---|
| Unit Tests | 3 min | All agent functions | Block |
| Integration Tests | 5 min | Agent ↔ agent interaction | Block |
| API Contract Tests | 2 min | All gRPC/REST endpoints | Block |
| Model Evaluation (if applicable) | 15 min | Accuracy, bias, performance | Warn (require override for > 2% degradation) |
| Security Scan (SAST) | 5 min | Code vulnerabilities | Block |
| Dependency Check | 2 min | Known vulnerable dependencies | Block |

### 5.6 Stage 5: Package

**Trigger:** All tests pass

**Duration:** 2 minutes

**Pipeline Steps:**
1. Apply final tags (version, latest, git-sha) (15s)
2. Sign image with Cosign using deployment key (10s)
3. Generate and sign attestation (in-toto format) (15s)
4. Push final image to immutable tag location (30s)
5. Update Helm chart / Kustomize manifests with new image tag (20s)
6. Push updated manifests to Git (deployment config repo) (10s)
7. Update deployment metadata in database (10s)

**Package Output:**
- Immutable image tag: `registry.aic.internal/<agent>:<major>.<minor>.<patch>-b<builddate>.<buildnum>`
- Deployable manifest: Helm chart with values updated for each environment
- Attestation: signed document proving build provenance

### 5.7 Stage 6: Deploy to Dev

**Trigger:** Automatic upon successful package stage

**Duration:** 5 minutes

**Deployment Process:**
1. Pull deployment manifests from Git (5s)
2. Verify image signature (5s)
3. Apply manifests to dev cluster (30s)
4. Wait for rollout to complete (2 min)
5. Run smoke tests (1 min)
6. Run readiness verification (30s)
7. Report status (10s)

**Success Criteria:**
- All pods in Running state
- All readiness probes passing
- Smoke tests: 100% pass rate
- No errors in agent logs (WARNING level or above)

### 5.8 Stage 7: Deploy to Staging

**Trigger:** Automatic upon successful dev deployment

**Duration:** 10 minutes

**Deployment Process:**
1. Verify dev deployment passed (all checks green) (5s)
2. Pull staging manifests from Git (5s)
3. Verify image signature (5s)
4. Apply manifests to staging cluster (30s)
5. Wait for rollout to complete (3 min)
6. Run integration tests (5 min)
7. Run performance baseline tests (2 min)
8. Run data integrity tests (2 min)
9. Report status (10s)

**Integration Tests:**
- Full end-to-end flows: Student → Curriculum → Assessment → Progress Tracking
- Cross-department flows: Content Production → Quality Assurance → Publishing
- Model inference pipelines: Request → Model Server → Response → Feedback
- Message queue throughput: Kafka produce/consume at staging load levels

### 5.9 Stage 8: Deploy to Pre-Prod

**Trigger:** Manual approval by Team Lead (after staging deployment passes)

**Duration:** 15 minutes

**Gating Checklist (automated validation before deployment):**
| Gate | Source | Details |
|---|---|---|
| Staging success | CI system | All staging tests passed |
| Performance regression | Performance DB | No regression > 10% in P95 or throughput |
| Security scan | Vulnerability DB | No unaddressed critical/high CVEs |
| Model evaluation | Model Registry | Model accuracy within 2% of baseline |
| Change approval | Approval system | Team Lead has approved |
| Deployment window | Calendar | Current time within allowed window |
| Freeze period check | Calendar | Not in deployment freeze |
| Resource availability | Cluster metrics | Sufficient capacity in pre-prod cluster |

### 5.10 Stage 9: Deploy to Production

**Trigger:** Manual approval by Department Manager + Change Advisory Board (CAB)

**Duration:** 30 minutes

**Production Gates (same as pre-prod plus additional):**
| Additional Production Gates | Source | Details |
|---|---|---|
| CAB approval | Approval system | CAB member approved |
| Dept Manager approval | Approval system | Department Manager approved |
| Pre-prod success | CI system | Pre-prod deployment passed all checks |
| Load test passed | Performance DB | 2x load sustained for 15 min in pre-prod |
| Business hours check | Calendar | Deployment within 9 AM - 5 PM ME time |
| CE-AI override (if needed) | Approval system | For emergency/outside-window deployments |

**Production Deployment Process:**
1. Validate all gates (30s)
2. Pull production manifests from Git (5s)
3. Verify image signature (5s)
4. Apply manifests to production cluster (region A first) (1 min)
5. Canary: 1% of traffic in region A (5 min observation)
6. If pass: 5% canary (5 min observation)
7. If pass: 20% canary (5 min observation)
8. If pass: 50% canary (5 min observation)
9. If pass: 100% rollout in region A (2 min)
10. Wait 15 min for region A stabilization
11. Deploy to region B (same canary process, 15 min)
12. Deploy to region C (DR) if applicable (10 min)
13. Full verification suite (5 min)
14. Report status to CAB and stakeholders (10s)

### 5.11 Pipeline Metrics

| Metric | Target | Measurement Point |
|---|---|---|
| Dev deploy success rate | > 98% | CI system |
| Staging deploy success rate | > 95% | CI system |
| Pre-prod deploy success rate | > 90% | CI system |
| Prod deploy success rate | > 99.5% | CI system |
| Time to detect deployment failure | < 30 seconds | Monitoring system |
| Time to rollback (standard) | < 30 seconds | Automated rollback |
| Time to rollback (stateful) | < 5 minutes | Automated rollback |
| Pipeline duration (dev-to-prod, standard) | < 60 minutes | CI system |
| Pipeline duration (dev-to-prod, large model) | < 120 minutes | CI system |
| Human approval time (pre-prod) | < 60 minutes | Approval system |
| Human approval time (prod) | < 4 hours | CAB meeting schedule |

### 5.12 Pipeline Failure Handling

| Failure Point | Automated Action | Notification | Escalation |
|---|---|---|---|
| Build failure | Retry once after 1 min | Developer | Team Lead |
| Vulnerability scan failure | Block pipeline | Developer + Security | Security Lead |
| Test failure | Block pipeline | Developer + QA | Team Lead |
| Dev deploy failure | Auto-rollback | Developer | Team Lead |
| Staging deploy failure | Auto-rollback | Developer + Team Lead | Dept Manager |
| Pre-prod deploy failure | Auto-rollback (canary phase) | Team Lead + Dept Manager | EVP |
| Production deploy failure | Auto-rollback | Dept Manager + CAB + CE-AI | CE-AI |
| Approval timeout | Expire request | Requester | Dept Manager |

---

## 6. Deployment of Agent Groups

### 6.1 Grouping Strategy

Agents are never deployed individually. They are deployed in logical groups that correspond to organizational structure, functional dependencies, and deployment efficiency.

**Group Hierarchy:**
```
Level 1: Platform-Wide Deployment (all 8,500 agents)
    ↓
Level 2: Department Deployment (33 departments, ~250 agents each)
    ↓
Level 3: Team Deployment (130 teams, ~65 agents each)
    ↓
Level 4: Functional Group (5-15 agents with shared dependencies)
    ↓
Level 5: Individual Agent (1 agent)
```

**Grouping by Team:**
Each of the 130 teams has a deployment group containing all agents owned by that team. This is the primary unit of deployment.

Example — Curriculum Development Team (Team ID: CURR-07):
- Curriculum Generator Agent (1 instance)
- Curriculum Validator Agent (1 instance)
- Curriculum Versioning Agent (1 instance)
- Curriculum Localization Agent (5 instances, per language)
- Curriculum QA Agent (1 instance)
- Metadata Tagging Agent (2 instances)
- Total: 11 agents per team deployment group

**Grouping by Function:**
Example — Assessment Pipeline (Functional Group):
- Question Generator Agent
- Answer Validator Agent
- Scoring Engine Agent
- Grade Calculator Agent
- Feedback Generator Agent
- Progress Tracker Agent
- All 6 deployed together to maintain data consistency

### 6.2 Startup Order

**Dependency Resolution Algorithm:**
1. Parse agent deployment manifests for all agents in the group
2. Extract dependency declarations from each agent's configuration
3. Build a directed acyclic graph (DAG) of dependencies
4. Topological sort to determine startup order
5. Verify no circular dependencies exist (block deployment if found)
6. Assign each agent to a startup phase

**Example Startup Phases for a Department:**
| Phase | Agents | Dependencies | Expected Duration |
|---|---|---|---|
| 0 | Database agents, Message queue agents, Config server agents | None | 2 min |
| 1 | Auth agents, Logging agents, Monitoring agents | Phase 0 | 2 min |
| 2 | Model server agents, Cache agents | Phase 0-1 | 5 min (model loading) |
| 3 | Coordinator agents, Department head agents | Phase 0-2 | 2 min |
| 4 | Workflow agent group A (independent) | Phase 0-3 | 3 min |
| 5 | Workflow agent group B (depends on A) | Phase 0-4 | 3 min |
| 6 | User-facing agents, API gateway agents | Phase 0-5 | 2 min |
| 7 | Reporting agents, Analytics agents | Phase 0-6 | 3 min |

**Total deployment time for a department: ~22 minutes**

### 6.3 Global Deployment Coordinator

A dedicated **Global Deployment Coordinator Agent** (Agent ID: DEPLOY-COORD-001) manages all multi-agent deployments.

**Responsibilities:**
1. Receive deployment request from CI/CD pipeline
2. Calculate dependency graph for all agents in the deployment
3. Determine startup order across all agent groups
4. Coordinate with Kubernetes API to deploy in correct order
5. Monitor deployment progress and health at each phase
6. Pause, resume, or rollback based on phase-level health checks
7. Report deployment status to the Deployment Dashboard
8. Manage batch deployments (500 agents at a time)

### 6.4 Batch Deployment

To avoid overwhelming the cluster control plane and to provide manageable rollback scopes, agents are deployed in batches of 500.

**Batch Process:**
```
Total agents to deploy: 2,000
Batch size: 500
Number of batches: 4

Batch 1: Deploy agents 1-500
         Wait for all 500 to pass health checks (5 min timeout)
         If fail: rollback batch 1 (all 500)
         If pass: proceed to batch 2

Batch 2: Deploy agents 501-1000
         Same process...

Batch 3: Deploy agents 1001-1500
         Same process...

Batch 4: Deploy agents 1501-2000
         Same process...
```

**Batch Timeouts:**
| Agent Type | Per-Batch Timeout | Per-Agent Timeout |
|---|---|---|
| Standard agents | 10 minutes | 2 minutes |
| Large model agents | 20 minutes | 10 minutes |
| Stateful agents | 15 minutes | 5 minutes |
| Mixed batch | 20 minutes | Per-agent max |

### 6.5 Large Model Agent Deployment

Large model agents (those requiring GPU and model weights > 2 GB) require special handling during deployment.

**Pre-Warming Process:**
1. Deploy model server instance (start container)
2. Load model weights from shared volume or download from registry (30s to 5 min)
3. Compile model for inference (JIT compilation, 1-5 min)
4. Run warmup inference (10 sample requests, 30s)
5. Agent readiness probe passes only after pre-warming complete
6. Traffic is routed to the instance only after readiness

**Model Cache Strategy:**
- Node-local SSD cache for frequently used models
- Model shards pre-fetched before deployment
- Cache hit rate target: > 95%
- Cache miss penalty: 5 minutes extra startup time

---

## 7. Configuration Management

### 7.1 Configuration Philosophy

Configuration is fully externalized from container images. No agent image contains environment-specific configuration. All configuration is injected at deployment time from external sources.

**Configuration Sources:**
- Environment Variables
- Config Server (Consul)
- Feature Flag Service
- Secret Store (Vault)
- Kubernetes ConfigMaps
- Kubernetes Secrets

### 7.2 Configuration Hierarchy

Configuration is resolved with a hierarchical override system:

```
                    ┌─────────────────────────────┐
                    │      Default Config           │
                    │  (image defaults, lowest)     │
                    └──────────┬──────────────────┘
                               │ overridden by
                               ▼
                    ┌─────────────────────────────┐
                    │   Environment Config          │
                    │  (dev/staging/pre-prod/prod)  │
                    └──────────┬──────────────────┘
                               │ overridden by
                               ▼
                    ┌─────────────────────────────┐
                    │   Department Config           │
                    │  (all agents in department)   │
                    └──────────┬──────────────────┘
                               │ overridden by
                               ▼
                    ┌─────────────────────────────┐
                    │    Team Config                │
                    │  (all agents in team)         │
                    └──────────┬──────────────────┘
                               │ overridden by
                               ▼
                    ┌─────────────────────────────┐
                    │   Agent-Specific Config       │
                    │  (single agent instance)      │
                    └─────────────────────────────┘
```

**Resolution Order (highest priority wins):**
1. Agent-specific Config (most specific)
2. Team Config
3. Department Config
4. Environment Config
5. Default Config (lowest priority)

### 7.3 Configuration Sources Detail

**Environment Variables:**
- Used for: deployment-specific values (pod name, namespace, service endpoints)
- Set by: Kubernetes deployment manifests
- Updated by: redeployment only
- Examples: `POD_NAME`, `NAMESPACE`, `SERVICE_HOST`, `SERVICE_PORT`

**Config Server (Consul / etcd):**
- Used for: shared configuration across agents, dynamic settings
- Set by: Config management tool, CI/CD pipeline
- Updated by: API call (no redeployment needed)
- Watch mechanism: agents watch for changes every 30 seconds
- Examples: feature flags, model selection, global timeouts, rate limits

**Feature Flag Service (LaunchDarkly / Flagsmith):**
- Used for: gradual rollouts, A/B testing, kill switches
- Set by: Product managers, engineers via web UI
- Updated by: Real-time via SDK
- Examples: new feature toggles, experiment assignments

**Secret Store (HashiCorp Vault):**
- Used for: secrets, credentials, API keys, certificates
- Set by: Security team, automated rotation
- Updated by: Vault API (auto-rotation)
- Access: per-agent service account tokens with limited scope
- Examples: database passwords, API keys for external services, TLS certificates

### 7.4 Dynamic Configuration

**Scope of Dynamic Changes:**
| Config Type | Can Change Dynamically? | Mechanism | Propagation Time |
|---|---|---|---|
| Feature flags | Yes | Feature Flag SDK | < 30 seconds |
| Model selection (which model to use) | Yes | Config Server watch | < 30 seconds |
| Temperature, top_p, inference params | Yes | Config Server watch | < 30 seconds |
| Timeout values | Yes | Config Server watch | < 30 seconds |
| Log levels | Yes | Config Server watch | < 10 seconds |
| Rate limits | Yes | Config Server watch | < 30 seconds |
| External service endpoints | No | Requires redeployment | N/A |
| Database connection strings | No | Requires redeployment | N/A |
| Agent dependencies (which agents to call) | No | Requires redeployment | N/A |
| Resource limits (CPU/memory) | No | Requires redeployment | N/A |

**Dynamic Config Change Process:**
1. Engineer updates config via Config Server API or UI
2. Config Server validates the change against schema
3. Config Server versions the change (increment version number)
4. Config Server broadcasts change notification to subscribed agents
5. Agents receive notification, fetch updated config
6. Agents apply config change (if applicable)
7. Agents log the config change (audit trail)
8. Agents continue operating with new config

### 7.5 Configuration Versioning

Every configuration change is logged and versioned:

```
Config Version: 47
Agent: curriculum-generator
Environment: production
Changed by: user@aic.internal (OIDC: eng-Sarah)
Change time: 2026-07-09T14:30:00+03:00
Previous version: 46
Change summary: Updated temperature from 0.7 to 0.8 for more creative outputs
Audit trail: https://config.aic.internal/versions/47
```

### 7.6 Configuration Validation

All configuration changes are validated before application:
1. Schema validation: Does the config match the expected schema? (JSON Schema or Protobuf schema per agent type)
2. Type validation: Are all values of the correct type?
3. Range validation: Are all values within acceptable ranges? (e.g., temperature 0.0-2.0, timeout 1-300 seconds)
4. Dependency validation: Do all referenced resources exist?
5. Security validation: Does config expose secrets? Scan for hardcoded secrets, reject if found
6. Impact analysis: What is the blast radius of this change?

### 7.7 Secret Management

**Vault Architecture:**
- HashiCorp Vault with 3 nodes in HA mode, auto-unseal
- Agent secrets, database credentials, external API keys, service auth tokens
- Each agent has a Vault policy granting access only to its required secrets
- Access authenticated via Kubernetes service account token (Vault K8s auth)
- All secret access is logged (who, what, when, which agent)

**Secret Auto-Rotation:**
| Secret Type | Rotation Frequency | Method |
|---|---|---|
| Database passwords | 30 days | Vault dynamic secrets (auto) |
| API keys | 90 days | Vault + scheduled rotation |
| TLS certificates | 180 days | cert-manager + Let's Encrypt |
| Service tokens | 7 days | Vault Kubernetes auth |
| JWT signing keys | 30 days | Vault transit engine |

### 7.8 Example Configuration Catalog

| Config Key | Type | Agent | Default | Production Value | Dynamic? |
|---|---|---|---|---|---|
| `model.name` | string | curriculum-generator | `gpt-4` | `gpt-4-turbo` | Yes |
| `model.temperature` | float | curriculum-generator | `0.7` | `0.8` | Yes |
| `model.max_tokens` | int | curriculum-generator | `4096` | `4096` | Yes |
| `timeout.request` | int (seconds) | all agents | `30` | `60` | Yes |
| `timeout.idle` | int (seconds) | all agents | `300` | `600` | Yes |
| `logging.level` | string | all agents | `INFO` | `WARN` | Yes |
| `logging.sampling` | float | all agents | `1.0` | `0.1` | Yes |
| `rate_limit.rpm` | int | all agents | `1000` | `5000` | Yes |
| `cache.ttl` | int (seconds) | content agents | `3600` | `7200` | Yes |
| `retry.max_attempts` | int | all agents | `3` | `5` | No |
| `db.pool_size` | int | db agents | `10` | `50` | No |
| `feature.new_curriculum` | bool | curriculum | `false` | `true` | Yes (flag) |

---

## 8. Database and State Deployment

### 8.1 Schema Migration Strategy

All database schema changes follow the **expand → migrate → contract** pattern to ensure zero-downtime migrations.

**The Expand-Migrate-Contract Pattern:**
```
Phase 1: EXPAND
  Add new columns, tables, indexes (non-breaking)
  Old code continues to work (ignores new columns)
  New code can read/write to both old and new structures

Phase 2: MIGRATE
  Backfill data from old structure to new structure
  Run dual-writes: write to both old and new
  Monitor data consistency
  Verify all reads return correct data

Phase 3: CONTRACT
  Remove old columns, tables, indexes
  This happens in a SUBSEQUENT deployment (next version)
  Old code must be fully retired before contract phase
```

**Backward Compatibility Commitment:**
- Schema changes must be backward-compatible for at least 2 major versions
- Version N+1 schema can be read/written by version N agents
- Contract phase happens in version N+2, after version N agents are fully retired

### 8.2 Zero-Downtime Migration Rules

| Migration Type | Zero-Downtime Safe? | Method |
|---|---|---|
| Add column (nullable) | Yes | Expand phase only |
| Add column (non-nullable, default) | Yes | Expand + backfill default |
| Add table | Yes | Expand phase only |
| Add index | Yes | CONCURRENTLY keyword |
| Remove column | No (2 version lag) | Contract phase (in v+2) |
| Remove table | No (2 version lag) | Contract phase (in v+2) |
| Rename column | No | Add new + backfill + remove old (3 phases, 2 versions) |
| Change column type | No | Add new typed column + migrate + remove old |
| Add foreign key | Yes | NOT VALID + VALIDATE (no lock) |
| Remove foreign key | Yes | Contract phase |

**Lock Avoidance:**
- All DDL operations use `LOCK_TIMEOUT = 100ms`
- If lock cannot be acquired, migration fails gracefully
- No `ALTER TABLE ... ALTER COLUMN` operations that require exclusive locks on live tables

### 8.3 Stateful Agent Deployment

Stateful agents (agents that maintain persistent state) require special deployment handling.

**Stateful Agent Types:**
- Knowledge Graph agents (maintain graph database)
- User Profile agents (maintain user state)
- Session Management agents (maintain session state)
- Progress Tracking agents (maintain learning progress)
- Content Cache agents (maintain content caches)

**Deployment Process for Stateful Agents:**
1. Pre-deployment snapshot of persistent data
2. Run expand-phase schema migrations (if applicable)
3. Blue-green deployment (not rolling for stateful)
4. New stack with new Persistent Volume Claims (PVCs)
5. Data synchronized from old PVC to new PVC (if schema unchanged)
6. Traffic switched to new stack
7. Old stack retained for rollback (60 minute window)

**Persistent Volume Claims (PVC) Strategy:**
| Stateful Agent Type | Storage Class | Volume Size | Backup Frequency | Snapshot Before Deploy |
|---|---|---|---|---|
| Knowledge Graph | SSD-backed, HA | 500 GB | Hourly | Yes |
| User Profile | SSD-backed | 200 GB | Real-time replication | Yes |
| Session Store | Memory-backed (Redis) | 50 GB | 5 min RDB snapshots | Yes |
| Progress Tracking | SSD-backed | 100 GB | 15 min | Yes |
| Content Cache | SSD-backed | 1 TB | 6 hours | No (rebuildable) |

### 8.4 Knowledge Graph Deployment

**Partitioning Strategy:**
- Domain partition: Each Islamic studies domain has its own partition (Quran, Hadith, Fiqh, Sirah, Arabic Language, Tafsir, Aqeedah, Islamic History)
- Shard by content ID: Within each partition, data is sharded by content ID (consistent hashing)
- Replica factor: 3 replicas per shard (for HA and read scalability)

**Deployment Process:**
1. New partition instances created alongside existing (no routing yet)
2. New instances subscribe to change data capture (CDC) stream
3. Catch up with current state (1-5 minutes)
4. New instances begin serving read traffic (shadow mode)
5. Route 10% of Knowledge Graph reads to new instances
6. Increase by 20% increments every 5 minutes
7. Full rollout after 30 minutes if all checks pass
8. Old instances kept for 60 minutes (rollback window)

### 8.5 Cache Warming

After deployment, caches are empty (cold). A cache warming process pre-populates caches to avoid performance degradation.

**Cache Types and Warming Strategy:**
| Cache Type | Warming Method | Warming Time | Target Hit Rate |
|---|---|---|---|
| Model inference cache | Pre-compute top 1,000 most common inference patterns | 2-5 min | 60% immediately, 90% after 1 hour |
| Content cache | Load most popular 10,000 content items | 1-2 min | 70% immediately |
| Session cache | Not warmed (empty is fine, loads on demand) | N/A | N/A |
| User profile cache | Pre-load top 5% most active user profiles | 3-5 min | 80% for active users |
| API response cache | Warm with recent frequent API calls from logs | 2-3 min | 50% immediately |

**Cache Warming Process:**
1. Extract list of top-K items from production logs (last 24 hours)
2. For each item, generate cache key
3. Generate pre-warming requests in batches of 100
4. Send requests to new agent instances (no user traffic yet)
5. Monitor: warming progress, error rate, memory usage
6. Cache hit rate reported in deployment dashboard
7. When hit rate exceeds target (or timeout reached): declare warm
8. Route user traffic to instances only after warm

---

## 9. Model Deployment

### 9.1 Model Serving Architecture

Models are served from dedicated inference endpoints, separate from agent containers. This allows independent scaling, versioning, and deployment of models.

```
                          ┌──────────────────┐
                          │   Agent Container  │
                          │  (business logic)  │
                          └────────┬─────────┘
                                   │ gRPC / HTTP inference request
                                   ▼
                    ┌──────────────────────────────┐
                    │     Model Inference Server     │
                    │  (vLLM / TGI / Triton / TorchServe)  │
                    ├──────────────────────────────┤
                    │  Model A (v1.2)  │  Model B (v2.0)  │
                    │  (GPU: 1)       │  (GPU: 2)        │
                    └──────────────────────────────┘
                                   │
                                   ▼
                    ┌──────────────────────────────┐
                    │     Model Registry             │
                    │  (MLflow / S3 + Model DB)     │
                    └──────────────────────────────┘
```

**Model Server Specifications:**
| Feature | Specification |
|---|---|
| Server Software | vLLM (primary), TGI (fallback), Triton (ensemble) |
| GPU Requirement | NVIDIA A100 80GB or H100 (1-8 GPUs per instance) |
| Model Format | SafeTensors (primary), ONNX (fallback) |
| Quantization | FP16 (default), INT8/NF4 (for throughput) |
| Batching | Dynamic batching, max batch 128 |
| Max Input Tokens | 8,192 (standard), 32,768 (for long-form content) |
| Max Output Tokens | 4,096 (standard), 8,192 (for content generation) |

### 9.2 Model Versioning

Every model is versioned independently of the agent that uses it.

**Model Versioning Scheme:**
```
Model Name: quran-translation-model
Version: v2.1.3
  - Major (2): Architecture change (new model family)
  - Minor (1): Training data update, fine-tuning
  - Patch (3): Hyperparameter optimization, bug fix

Model Artifacts in Registry:
  quran-translation-model/v2.1.3/
    ├── model.safetensors
    ├── config.json
    ├── tokenizer.json
    ├── metrics.json   (accuracy, BLEU, etc.)
    ├── license.txt
    └── README.md
```

**Concurrent Model Versions:**
- Multiple model versions can run simultaneously
- Previous version (N-1) always kept running for 7 days after new version (N) rollout
- Older versions (N-2+) are automatically scaled down after grace period
- A/B testing: two versions run concurrently for comparison

### 9.3 Model Rollout Process

**Shadow → Canary → Full Production:**

```
Phase 1: SHADOW (Duration: 6-24 hours)
  - New model deployed as shadow alongside current production model
  - Shadow receives copy of ALL inference requests
  - Shadow produces responses but does NOT serve them to users
  - Comparison engine compares shadow vs production outputs
  - Metrics: output quality score, latency, error rate
  - Decision: proceed to canary if shadow matches or exceeds production

Phase 2: CANARY (Duration: 2-8 hours)
  - New model receives 1% of live traffic (expandable to 5% then 20%)
  - Users are randomly selected (consistent hashing for session consistency)
  - Real user metrics collected: satisfaction, engagement, completion rate
  - Monitoring: error rate < 0.1%, latency within 10% of production
  - Rollback: instant (remove canary traffic, route to previous)
  - Progression: 1% → 5% → 20% → 50% (automated gates between each)

Phase 3: FULL PRODUCTION (Duration: indefinite)
  - New model serves 100% of traffic
  - Old model kept in hot standby for 7 days
  - Continuous monitoring for regression
  - Automated rollback possible within 7 days
```

### 9.4 Model A/B Testing

Longer-term comparison of two model versions for specific metrics.

```
Experiment: AB-20260709-translation-quality
Model A: quran-translation-model v2.1.2 (current production)
Model B: quran-translation-model v2.2.0 (proposed upgrade)
Duration: 7 days
Traffic Split: 50% Model A, 50% Model B (random by user ID hash)
Primary Metric: Translation accuracy (human-rated, sampled)
Secondary Metrics:
  - Generation speed (tokens/second)
  - User satisfaction rating (1-5)
  - Content completion rate
  - Error rate
Promotion Rule:
  - Model B must exceed Model A in primary metric by >= 5% (p < 0.05)
  - Model B must not regress in any secondary metric by > 2%
  - Model B error rate must be within 0.1% of Model A
```

### 9.5 Model Rollback

**Instant Rollback:**
- Model server supports loading multiple model versions
- Agent configuration points to active model version
- To rollback: update config to point to previous version (no redeployment)
- Rollback time: < 10 seconds (next inference request uses old model)
- Config change is dynamic, no agent restart needed

**Hot-Swap (Zero-Downtime Model Update):**
1. Agent sends inference request to model server
2. Model server receives request
3. Model server routes to currently active model version
4. Meanwhile, model server loads new version in separate memory space
5. New version loading complete (5-60 seconds depending on model size)
6. Model server updates active version pointer atomically
7. Next inference request uses new model version
8. Old model version kept in memory for 5 minutes (fast rollback)
9. After 5 minutes: old model unloaded, memory freed

### 9.6 Model Caching

**LRU Cache for Inference Results:**
| Cache Parameter | Value |
|---|---|
| Cache Algorithm | LRU (Least Recently Used) |
| Max Cache Size | 100,000 entries per model server |
| Cache TTL | 24 hours (or until model version changes) |
| Cache Key | Input text hash + model version + parameters hash |
| Cache Hit Rate Target | > 60% for frequently accessed content |
| Eviction Policy | LRU, with pinned entries for top 100 queries |

---

## 10. Rollback Procedures

### 10.1 Rollback Philosophy

Every deployment must have a rollback path. Rollback is not a failure — it is a normal part of the deployment process. The system is designed to make rollback fast, reliable, and safe.

**Key Principles:**
1. **Design for rollback:** The rollback path is defined before the deployment begins
2. **Automated detection:** Metrics-based triggers detect deployment issues automatically
3. **Speed:** Standard agents rollback in < 60 seconds; stateful in < 5 minutes
4. **Safety:** Data integrity is preserved during rollback
5. **Auditability:** Every rollback is logged, analyzed, and reviewed

### 10.2 Automated Rollback Triggers

| Trigger | Threshold | Evaluation Window | Agents Affected | Action |
|---|---|---|---|---|
| Error rate spike | > 5% error rate (from baseline < 0.5%) | 1 minute | All | Immediate rollback |
| Latency spike | > 2x baseline P95 latency | 2 minutes | All | Immediate rollback |
| Health check failure | > 10% of new instances failing health checks | 30 seconds | Deployed agents | Pause + investigate → rollback |
| Crash loop | Any pod in CrashLoopBackOff > 3 times | 1 minute | Single agent | Remove from traffic + rollback |
| Model accuracy drop | > 5% degradation in model quality score | 5 minutes | Model agents | Rollback model to previous version |
| Throughput drop | > 30% reduction in requests/second | 2 minutes | All | Rollback |
| Saturation | CPU > 90% or memory > 90% across all new pods | 2 minutes | Affected agents | Rollback or scale out |
| User complaints | > 10 user reports within 5 minutes (via monitoring) | 5 minutes | Suspected agents | Manual investigation → possible rollback |

### 10.3 Rollback Time Targets

| Agent Type | Rollback Time Target | Mechanism | Notes |
|---|---|---|---|
| Stateless agents (standard) | < 30 seconds | Rolling revert / Blue-green switch | Most common scenario |
| Stateless agents (large model) | < 2 minutes | Rolling revert (model loading) | GPU memory release |
| Stateful agents | < 5 minutes | Blue-green switch + data restore | PVC snapshot restore |
| Knowledge Graph | < 10 minutes | Blue-green + CDC catchup | Consistency verification |
| Model server | < 10 seconds | Config change (switch model version) | No restart needed |
| Database schema | < 15 minutes | Previous schema restore | Expand-phase only (safe) |
| Full platform (all agents) | < 30 minutes | Automated multi-group rollback | Coordinated by DEPLOY-COORD |

### 10.4 Rollback Mechanisms by Strategy

**Blue-Green Rollback:**
- Traffic instantly reverts to Blue by updating load balancer selector
- Rollback time: < 5 seconds
- User impact: None (instant switch, no dropped connections)

**Rolling Update Rollback:**
- `kubectl rollout undo deployment/<name>` scales up old ReplicaSet
- Old instances replace new instances batch by batch
- Rollback time: < 60 seconds

**Canary Rollback:**
- Set canary traffic weight to 0%
- All traffic routes to stable version
- Rollback time: < 5 seconds (traffic routing update)

### 10.5 Data Rollback

| Scenario | Rollback Method | Time | Risk |
|---|---|---|---|
| Expand-phase migration (add column) | No rollback needed (backward compatible) | 0 | None |
| New data written in new column | Truncate new column data (if migration failed) | 5 min | Low |
| Dual-write (old + new) | Stop writing to new, continue with old | 1 min | None |
| Schema contract phase | Restore from pre-deployment backup | 15 min | Medium |
| Data transformation | Restore pre-transformation data from backup | 15 min | Medium |

**Pre-Deployment Snapshot Process:**
1. Before any stateful deployment: Database pg_dump/mongodump, PVC Volume snapshot via CSI driver, Knowledge Graph export to JSON
2. Snapshot stored in backup storage (S3/object store)
3. Snapshot verified with checksum
4. Snapshot retention: 7 days minimum

### 10.6 Rollback Testing

| Test | Frequency | Environment | Success Criteria |
|---|---|---|---|
| Automated rollback trigger | Every deployment | Dev | Triggers fire correctly on simulated failure |
| Rollback execution | Every staging deployment | Staging | Full rollback completes within timeout |
| Data rollback | Weekly | Staging | Data restored with full integrity |
| Blue-green switch | Every blue-green deploy | Pre-prod | Traffic switches in < 5 seconds |
| Canary rollback | Every canary deploy | Pre-prod | Canary weight goes to 0% instantly |
| Emergency rollback | Monthly | Production (off-peak) | Full platform rollback in < 30 min |

### 10.7 Post-Rollback Process

After every rollback, an automated process runs:
1. Rollback detected and timestamped
2. Automated notifications sent to: Deployment team, Affected team, Dept Manager (Slack, PagerDuty, Email)
3. Full details logged to Deployment Database
4. Root Cause Analysis (RCA) initiated with automated data collection
5. RCA ticket created in JIRA with 2-hour preliminary SLA, 24-hour full RCA SLA
6. Rollback review meeting scheduled for next business day
7. Action items tracked in JIRA with due dates

---

## 11. Health Checks and Readiness

### 11.1 Health Check Architecture

Every agent instance has three types of health probes that Kubernetes uses to manage traffic routing and instance lifecycle:
- `/health` (Liveness) — Called every 10 seconds, returns 200 OK if alive
- `/ready` (Readiness) — Called every 5 seconds, returns 200 OK if ready to accept work
- `/startup` (Startup Probe) — Called every 10 seconds, max 30 attempts (5 min)
- `/metrics` (Prometheus) — Scraped every 15 seconds for monitoring

### 11.2 Liveness Check (/health)

**Purpose:** Determine if the agent process is alive and responding.

**Check Mechanism:**
- Kubernetes Liveness Probe, HTTP GET to `/health` endpoint
- Expected response: HTTP 200 OK with `{"status": "alive"}`
- Called every 10 seconds, initial delay: 5 seconds after container start

**Failure Actions:**
| Consecutive Failures | Action |
|---|---|
| 1 | Log warning |
| 2 | Log error |
| 3 (failureThreshold) | Kubernetes restarts the container |

### 11.3 Readiness Check (/ready)

**Purpose:** Determine if the agent is ready to accept work.

**Check Mechanism:**
- Kubernetes Readiness Probe, HTTP GET to `/ready` endpoint
- Expected response: HTTP 200 OK with `{"status": "ready"}`
- Called every 5 seconds, initial delay varies by agent type

**Readiness Criteria (agent-specific):**
| Agent Type | Readiness Checks | Typical Time to Ready |
|---|---|---|
| Standard agent | Config loaded, DB connection OK | 5-15 seconds |
| Model-dependent agent | Model loaded, model server reachable | 30 seconds - 5 minutes |
| Stateful agent | State synced, PVC mounted | 10-30 seconds |
| Knowledge Graph agent | Partition loaded, peers reachable | 30-60 seconds |
| API Gateway agent | Upstream services reachable | 10-20 seconds |

**Failure Actions:**
| Consecutive Failures | Action |
|---|---|
| 1 | Remove from service mesh (no new traffic) |
| 2 | Log error |
| 3 (failureThreshold) | Container removed from endpoints (15 seconds total) |

### 11.4 Startup Probe (/startup)

**Purpose:** Give slow-starting agents (large model loads) sufficient time to initialize before liveness probes start killing them.

**Startup Duration by Agent Type:**
| Agent Type | P50 Startup Time | P95 Startup Time | Max (failureThreshold) |
|---|---|---|---|
| Standard agent | 3 seconds | 8 seconds | 30 seconds |
| Large model agent (CUDA) | 45 seconds | 120 seconds | 5 minutes |
| Large model agent (model download) | 2 minutes | 4 minutes | 5 minutes |
| Stateful agent (with data sync) | 15 seconds | 60 seconds | 3 minutes |
| Knowledge Graph agent | 30 seconds | 90 seconds | 5 minutes |

### 11.5 Metrics Endpoint (/metrics)

**Purpose:** Expose Prometheus-formatted metrics for monitoring and alerting.

**Standard Metrics Exposed:**
```
# Agent health
agent_up 1
agent_startup_time_seconds 145
agent_ready 1

# Request metrics
agent_requests_total{status="success"} 15234
agent_requests_total{status="error"} 23
agent_request_duration_seconds{quantile="0.5"} 0.045
agent_request_duration_seconds{quantile="0.95"} 0.182
agent_request_duration_seconds{quantile="0.99"} 0.453

# Resource metrics
agent_memory_usage_bytes 234567890
agent_cpu_usage_cores 0.45
agent_gpu_memory_usage_bytes 4567890123

# Business metrics (agent-specific)
agent_tasks_completed_total 892
agent_tasks_failed_total 3
agent_model_inference_latency_ms 156
agent_cache_hit_ratio 0.87
```

### 11.6 Degraded Mode

An agent can be in "degraded" mode — alive and responding but not fully functional.

**Degraded Mode Conditions:**
| Condition | Degraded Behavior | Routes Non-Critical Work? |
|---|---|---|
| Database connection slow | Use local cache, queue writes | No (block all writes) |
| Model server overloaded | Use simpler/fallback model | Yes (route non-critical) |
| Upstream service unavailable | Return cached response | Yes (route non-critical) |
| High memory usage (> 85%) | Reduce batch size, flush caches | Yes (route non-critical) |
| GPU temperature high | Reduce inference batch size | Yes (route non-critical) |

**Degraded Mode Response:**
```
/ready endpoint returns:
{
  "status": "degraded",
  "reason": "upstream_model_server_timeout",
  "since_seconds": 45,
  "can_accept_non_critical": true
}
```

---

## 12. Multi-Region Deployment

### 12.1 Regional Architecture

The platform operates across three geographic regions to provide high availability, low latency, and disaster recovery.

### 12.2 Region Specifications

| Property | Region A (Primary) | Region B (Secondary) | Region C (DR) |
|---|---|---|---|
| **Location** | Saudi Arabia (Riyadh) | Germany (Frankfurt) | Singapore |
| **Status** | Active for all workloads | Active for read-only | Cold standby |
| **Nodes** | 1,500 | 1,200 | 800 |
| **Agents** | 8,500 (full) | 4,250 (read-only) | 0 (scaled to 0) |
| **Latency (ME users)** | < 20ms | < 100ms | < 150ms |
| **Latency (EU users)** | < 100ms | < 20ms | < 150ms |
| **Latency (ASIA users)** | < 150ms | < 150ms | < 30ms |
| **Data** | Full production | Read replica | Async replica |
| **AZ Count** | 3 | 3 | 2 |
| **SLA** | 99.99% | 99.9% | N/A (standby) |
| **Failover Priority** | 1 (primary) | 2 (takes over if A fails) | 3 (if A and B fail) |

**Replication Lag SLAs:**
| Data Store | Sync/Async | Max Acceptable Lag | Monitoring |
|---|---|---|---|
| PostgreSQL (A→B) | Synchronous | 0 (committed same time) | Replication status |
| PostgreSQL (A→C) | Asynchronous | 5 minutes | Lag in bytes |
| MongoDB (A→B) | Asynchronous | 10 seconds | Oplog lag |
| Knowledge Graph | Synchronous | 0 (distributed consensus) | Raft status |
| Redis | Asynchronous | 2 seconds | Replication offset |

### 12.3 Traffic Routing

**Geo-DNS Routing:**
- User in Middle East: DNS resolves to Region A (primary) — latency < 20ms
- User in Europe: DNS resolves to Region B (secondary) — latency < 20ms for reads, writes proxied to Region A
- User in Southeast Asia: DNS resolves to Region C (DR if active, else proxy to A)

**Traffic Routing Table:**
| User Region | Normal Routing | Region A Down | Region B Down | Region C (activated) |
|---|---|---|---|---|
| Middle East | A | B | A | C |
| Europe | B (reads), A (writes) | B (all) | A (all) | C |
| Southeast Asia | A (proxy) | B (proxy) | A (proxy) | C (direct) |
| Other | A (proxy) | B (proxy) | A (proxy) | C (proxy) |

### 12.4 Regional Deployment Process

Deployments are rolled out across regions in a staggered fashion with observation periods.

```
Region A ───► Deploy + Observe (15 min) ───► Pass
                    │
                    ▼
Region B ───► Deploy + Observe (15 min) ───► Pass
                    │
                    ▼
Region C ───► Deploy + Observe (if activated) ───► Pass
                    │
                    ▼
          Deployment Complete
```

**Region-Specific Configuration:**
| Configuration | Region A | Region B | Region C |
|---|---|---|---|
| Content language priority | Arabic | English + Arabic | English + Malay |
| Preferred model | Full model (best accuracy) | Quantized model (lower latency) | Quantized model |
| Rate limits (user) | 100 req/min | 200 req/min (lower load) | 200 req/min |
| Data retention | Full (ME laws) | Full (GDPR) | Full (SG laws) |
| Content filter | Local regulations | EU regulations | Local regulations |
| Payment methods | Mada, Visa, STC Pay | PayPal, Visa, Mastercard | GrabPay, Visa, Mastercard |

### 12.5 Disaster Recovery Activation

**DR Activation Triggers:**
| Trigger | Condition | Action |
|---|---|---|
| Region A health score < 50% | Automated monitoring | Auto-activate Region B as primary |
| Region A complete outage | No connectivity > 5 min | Auto-activate DR procedure |
| Manual decision | CE-AI or EVP-Infrastructure | Manual DR activation |
| Scheduled DR test | Quarterly exercise | Planned activation |

**DR Activation Process (15 minute target):**
```
T+0:  DR triggered
T+1:  Region C cluster scaled up (nodes power on)
T+2:  Region C agents deployed (from latest production images)
T+3:  Region C databases restored from latest async replica
T+4:  Region C data consistency verified
T+5:  Region C model servers loaded with latest models
T+6:  Region C starts accepting health checks
T+7:  Region C readiness verified (all agents ready)
T+8:  Geo-DNS updated to route traffic to Region C
T+9:  Traffic ramping up (10% → 50% → 100% over 3 minutes)
T+10: Full production load served from Region C
T+11: Verification suite runs
T+12: Monitoring confirms stable operations
T+13: Notification sent: DR activation complete
T+14: Post-DR review initiated
T+15: FULL DR ACTIVATION COMPLETE
```

---

## 13. Deployment Monitoring and Verification

### 13.1 Post-Deployment Verification

Every deployment triggers an automated verification suite immediately after deployment completes.

**Verification Suite:**
| Test Type | Duration | What It Tests | Success Criteria |
|---|---|---|---|
| Smoke Tests | 2 min | All agents respond to /health and /ready | 100% pass |
| Functional Tests | 3 min | Key agent workflows (happy path) | 100% pass |
| Integration Tests | 5 min | Cross-agent communication | 100% pass |
| Data Integrity Tests | 3 min | Data consistency, no corruption | 100% pass |
| Performance Baseline | 5 min | Latency, throughput, error rate | Within 10% of baseline |
| Model Accuracy | 5 min | Model quality metrics (if model changed) | Within 2% of baseline |

**Verification Duration by Deployment Type:**
| Deployment Type | Verification Duration | Fail Action |
|---|---|---|
| Standard agent (rolling) | 5 minutes | Auto-rollback |
| Large model agent | 10 minutes | Auto-rollback |
| Stateful agent | 10 minutes | Auto-rollback |
| Model update | 15 minutes | Auto-rollback |
| Platform-wide | 20 minutes | Coordinated rollback |
| Emergency patch | 3 minutes | Notify if fail (manual decision) |

### 13.2 Success Criteria

A deployment is declared successful ONLY when ALL of the following criteria are met:

**Hard Criteria (must pass):**
| Metric | Standard Deploy | Critical Deploy | Measurement |
|---|---|---|---|
| Health checks | 100% of agents pass | 100% of agents pass | /health and /ready endpoints |
| Error rate | <= 0.5% | <= 0.1% | Agent metrics (5 min window) |
| P95 latency | <= 1.5x baseline | <= 1.2x baseline | Request duration metrics |
| P99 latency | <= 2x baseline | <= 1.5x baseline | Request duration metrics |
| Throughput | >= 80% of baseline | >= 90% of baseline | Requests/second |
| CPU/Memory | <= 85% of limit | <= 80% of limit | Resource metrics |
| Model accuracy (if applicable) | Within 5% of baseline | Within 2% of baseline | Model evaluation metrics |

### 13.3 Deployment Dashboard

A real-time dashboard displays the status of all ongoing and historical deployments.

**Dashboard Components:**
- Current deployment status (active, pending, succeeded, failed, rolling back)
- Deployment timeline (last 24 hours) with success/failure/rollback indicators
- Active rollbacks list with progress
- Deployment statistics (total, success rate, avg duration)
- Per-agent deployment history

### 13.4 Alerts

| Level | Color | Response Time | Channel | Example |
|---|---|---|---|---|
| Critical | Red | Immediate (< 5 min) | PagerDuty + Slack + SMS | Production deployment failed |
| High | Orange | < 15 minutes | Slack + Email | Deployment blocked at gate |
| Medium | Yellow | < 1 hour | Slack | Test suite failure |
| Low | Blue | < 24 hours | Email digest | Performance regression > 10% |

### 13.5 Deployment Audit Log

Every deployment action is logged to an immutable audit trail.

**Audit Log Entry Fields:**
- deployment_id, timestamp, agent, version_from, version_to, environment, strategy
- trigger, initiated_by, approved_by (with names, roles, timestamps)
- pipeline_stages (each stage with status and duration in seconds)
- health_checks_post_deploy (agents_total, agents_healthy, error_rate, p95_latency_ms, throughput_rps)
- rollback (null or rollback details)
- duration_total_min, result (success/failure/rollback)

**Audit Log Retention:**
- Active deployments: Indefinite (live in database)
- Completed deployments: 2 years in hot storage (Elasticsearch)
- Completed deployments: 7 years in cold storage (S3 Glacier)

---

## 14. Deployment Governance

### 14.1 Deployment Approval Matrix

| Environment | Trigger | Approver | Method | Timeline |
|---|---|---|---|---|
| Development | PR merge to `develop` | None (auto-deploy) | Automated | Instant |
| Staging | Auto after dev passes | Team Lead | Click-to-approve in CI | < 1 hour expected |
| Pre-Production | Manual trigger by Team Lead | Department Manager | Click-to-approve in CI | < 2 hours expected |
| Production (Standard) | Manual trigger by Dept Manager | CAB + Department Manager | CAB meeting + approval | < 4 hours (CAB schedule) |
| Production (Emergency) | Manual trigger by EVP/CE-AI | CE-AI (sole approval) | Emergency authorization | < 15 minutes |
| Production (Zero-risk) | Pre-defined low-risk changes | None (auto-deploy with monitoring) | Automated | < 5 minutes |

### 14.2 Approval Roles and Responsibilities

| Role | Can Approve For | Cannot Approve For | Back-up |
|---|---|---|---|
| Team Lead | Staging deployment, team config changes, low-risk feature flags | Pre-prod, production, cross-team changes | Senior Engineer |
| Department Manager | Pre-prod deployment, department-wide config, mid-risk flags | Production (without CAB) | EVP of Department |
| CAB Member (rotating) | Production deployment (standard), emergency deployment (with CE-AI) | Financial/payment changes without security review | CE-AI |
| EVP | Production deployment for their department (with CAB) | Platform-wide deployment (needs CE-AI) | CE-AI |
| CE-AI | Emergency deployment (any environment), platform-wide deployment | N/A (ultimate authority) | Senior EVP |

### 14.3 Change Advisory Board (CAB)

**CAB Composition:**
- 7 rotating members from senior technical leadership
- Chair: EVP of Infrastructure (permanent)
- Members: 2 Dept Managers, 2 Team Leads, 1 Security Lead, 1 QA Lead
- Quorum: 4 members required, Term: 2 weeks per rotation

**CAB Meeting Schedule:**
- Regular: Daily at 10:00 AM and 3:00 PM (ME time)
- Emergency: On-demand, summoned within 30 minutes
- Duration: 30 minutes per meeting

**CAB Review Criteria:**
- Change ticket completeness (risk assessment, rollback plan, test results)
- Test results from dev, staging, and pre-prod environments
- Performance baseline comparison (no regression > 10%)
- Security scan results (no critical/high CVEs unaddressed)
- Rollback plan feasibility (time target, data integrity)
- Deployment window compliance (business hours, no freeze)
- Monitoring dashboard link (for post-deploy observation)
- Communication plan (who to notify)

### 14.4 Deployment Freeze Periods

| Freeze Period | Duration | Scope | Reason |
|---|---|---|---|
| Ramadan (3 days before) | 3 days | No production deployments | Preparation for high traffic |
| Ramadan (entire month) | 29-30 days | No production deployments | Peak usage period (traffic 5x normal) |
| Ramadan (3 days after) | 3 days | No production deployments | Post-peak stabilization |
| Eid al-Fitr | 3 days | No production deployments | Holiday, reduced staffing |
| Eid al-Adha | 3 days | No production deployments | Holiday, reduced staffing |
| Islamic New Year | 1 day | No production deployments | Holiday |
| National Holidays (all regions) | 1 day each | No production deployments | Holiday, reduced staffing |
| Platform-Wide Maintenance | 2 days quarterly | All deployments | Scheduled infrastructure upgrades |

**Freeze Period Exceptions:**
- Emergency security patches (CVE with CVSS > 9.0): CE-AI approval required
- Critical production outage fix: CE-AI approval required
- Freeze exception process: Submit request → CE-AI review → EVP-Infrastructure concurrence

### 14.5 Deployment Windows

| Window Type | Days | Hours | Approval Required |
|---|---|---|---|
| Standard (Normal) | Sunday - Thursday | 9:00 AM - 5:00 PM ME time | Standard approval chain |
| Standard (Extended) | Sunday - Thursday | 7:00 AM - 7:00 PM ME time | Dept Manager + CAB |
| Off-Hours (Urgent) | Any day | Outside standard hours | CE-AI approval |
| Emergency (Any time) | Any day | Any time | CE-AI approval only |

### 14.6 Change Management Process

**Change Ticket Requirements:**
| Field | Standard Deploy | Emergency Deploy |
|---|---|---|
| Change ID | Auto-generated | Auto-generated |
| Agent/System | Required | Required |
| Version (from → to) | Required | Required |
| Change description | Required (detailed) | Required (brief) |
| Risk assessment | Required (low/med/high/critical) | Required (high/critical) |
| Rollback plan | Required | Required (can be brief) |
| Test results | Required (all envs) | Required (unit + smoke minimum) |
| Performance data | Required | Optional |
| Security scan | Required | Required |
| CAB approval | Required | Not required (CE-AI replaces) |
| Dept Manager approval | Required | Not required (CE-AI replaces) |
| Monitoring dashboard | Required | Required |
| Communication plan | Required | Required (expedited) |

---

## 15. Special Deployment Cases

### 15.1 New Department Launch

Launching a new department requires coordinated deployment of 20-50 new agents across multiple teams.

**Process:**
1. **Pre-Launch (2 weeks before):** Create infrastructure (namespaces, service accounts, Vault policies, database schemas, message queue topics)
2. **Development (1 week before):** Deploy all new agents to dev environment; run integration tests with existing departments
3. **Staging (3 days before):** Full staging deployment; test cross-department interactions; performance baseline
4. **Pre-Prod (2 days before):** Pre-prod deployment with canary validation; load testing at 2x expected load
5. **Production Launch Day:**
   - 8:00 AM: Deploy infrastructure (message queues, databases, configs) — no agents yet
   - 9:00 AM: Deploy department head agents and coordinator agents (Phase 0)
   - 9:30 AM: Deploy workflow agents (Phase 1)
   - 10:00 AM: Deploy user-facing agents (Phase 2)
   - 10:30 AM: Begin routing 10% of relevant traffic to new department
   - 11:00 AM: If successful, route 50% of traffic
   - 11:30 AM: Route 100% of traffic
   - 12:00 PM: Declare launch successful; monitor throughout day
6. **Post-Launch (1 week):** Intensive monitoring; performance tuning; bug fixes

### 15.2 Agent Retraining

Deploy an updated model without disrupting current work.

**Process:**
1. Train new model version offline (separate infrastructure)
2. Evaluate model against test set (accuracy, bias, performance)
3. Register model in Model Registry (new version)
4. Deploy model as shadow alongside current production model
5. Shadow period: 48 hours minimum, compare output quality on 50,000+ requests
6. If shadow outperforms current: canary rollout to 5% of users
7. Canary period: 24 hours, monitor real user satisfaction
8. If canary passes: gradual rollout to 100% over 4 hours
9. Old model kept in hot standby for 7 days
10. After 7 days: old model scaled down (kept in registry for reference)

### 15.3 Emergency Patch

Accelerated deployment for critical security patches or platform outages.

**Trigger Conditions:**
- Critical vulnerability (CVSS >= 9.0) affecting production agents
- Production outage affecting > 10% of users
- Data loss or corruption detected
- Active security breach

**Expedited Pipeline:**
```
1. Developer commits fix to `hotfix/<ticket-number>` branch
2. CI builds image (5 min, parallel scanning)
3. Vulnerability scan (3 min, critical+high only)
4. Unit tests (3 min, critical path tests only)
5. Package (1 min)
6. Deploy to dev (3 min, smoke tests only)
7. **SKIP STAGING** (bypass with CE-AI approval)
8. Deploy to pre-prod (5 min, canary 1% → 100%)
9. Manual approval: CE-AI (5 min max)
10. Deploy to production (blue-green, 10 min)
11. Verify (3 min)

Total emergency pipeline time: ~35 minutes maximum
```

**Emergency Approval:**
- CE-AI sole approval authority for emergency deployments
- EVP-Infrastructure must be notified within 5 minutes of emergency deployment
- Post-deployment review mandatory within 2 hours
- Standard approval chain must be retroactively completed within 24 hours

### 15.4 Scale-Up Event

Surge deployment of additional agent instances to handle increased load.

**Trigger Conditions:**
- Traffic exceeds 80% of current capacity for > 5 minutes
- Anticipated high-traffic event (e.g., Ramadan, Eid, new course launch)
- DDoS or traffic spike (legitimate or attack)

**Scale-Up Process:**
1. Horizontal Pod Autoscaler (HPA) detects increased load
2. HPA scales agent replicas (max 3x normal within 5 minutes)
3. Cluster Autoscaler provisions additional nodes (2-5 minutes)
4. Model server instances scale (if GPU-bound, adds GPU nodes)
5. Cache clusters scale (add Redis shards)
6. Database connection pool increases
7. Load balancer targets updated automatically
8. New instances warm up and become ready (30s-5min depending on type)
9. Traffic distributes across all instances
10. Monitoring: verify new instances are handling load correctly

**Scale-Down (Post-Event):**
1. Load decreases below threshold for 15 minutes
2. HPA scales down gradually (1 replica per 2 minutes)
3. Cluster Autoscaler de-provisions nodes
4. Cache clusters consolidate
5. Database connection pool reduces
6. Scale-down complete within 30 minutes of load decrease

**Pre-Scaling (Known Events):**
- For predictable events (Ramadan, Eid): pre-scale 2 hours before expected surge
- Deploy additional instances ahead of time (warm pool)
- Test capacity with synthetic load before event
- Monitor and adjust during event

### 15.5 Agent Retirement

Graceful removal of an agent from the platform.

**Retirement Process:**
```
Phase 1: ANNOUNCEMENT (2 weeks before)
  1. Announce retirement plan to all dependent agents
  2. Notify all teams that consume this agent's output
  3. Update dependency graph to flag agent as "retiring"
  4. Begin migration of consumers to alternative agents

Phase 2: DRAINING (1 week before)
  1. Set agent in "draining" mode — no new work assigned
  2. Complete all in-progress tasks (with deadline)
  3. Unfinished tasks reassigned to successor agents
  4. Monitor for any dependencies that still require this agent

Phase 3: GRACEFUL SHUTDOWN (Retirement Day)
  1. Verify: zero active tasks, zero pending messages
  2. Remove agent from service mesh (no traffic routing)
  3. Verify: no traffic for 15 minutes
  4. Scale down agent to 0 replicas
  5. Archive agent logs and metrics
  6. Create final backup of agent state (if any)
  7. Update dependency graph: agent marked as "retired"

Phase 4: CLEANUP (1 week after retirement)
  1. Remove agent from deployment manifests
  2. Remove agent configuration from Config Server
  3. Revoke Vault access for agent service account
  4. Clean up PVCs (after backup verified)
  5. Remove agent container image (keep last 3 versions)
  6. Update documentation and deployment records
```

### 15.6 Platform-Wide Deployment

Coordinate deployment across all departments, staggered over 48 hours.

**Schedule:**
```
Day 1, 8:00 AM:  Deployment Coordinator activates
Day 1, 9:00 AM:  Infrastructure agents (message queues, databases, config server)
Day 1, 10:00 AM: Department 01-05 (Core Infrastructure, Security, Platform)
Day 1, 12:00 PM: Department 06-11 (Curriculum, Assessment, Content Production)
Day 1, 2:00 PM:  Department 12-17 (Student Services, Progress Tracking, Analytics)
Day 1, 4:00 PM:  Observation period — all deployed agents monitored
Day 1, 5:00 PM:  Day 1 deployment window closes

Day 2, 8:00 AM:  Day 1 verification review
Day 2, 9:00 AM:  Department 18-23 (Quality Assurance, Publishing, Distribution)
Day 2, 11:00 AM: Department 24-29 (User Management, Communications, Support)
Day 2, 1:00 PM:  Department 30-33 (Executive, Reporting, Compliance)
Day 2, 3:00 PM:  Observation period — full platform deployed
Day 2, 5:00 PM:  Platform-wide deployment declared complete

Total: ~16 active deployment hours over 2 days
```

**Coordination:**
- Global Deployment Coordinator Agent manages sequencing
- Each department deployment is verified before next department begins
- If any department fails: platform-wide deployment pauses, rollback that department
- Communication channel: dedicated Slack channel `#platform-deploy`
- War room: active during entire deployment window (EVP-Infrastructure + Dept Managers + DevOps)
- Go/no-go check: conducted at start of each day

**Rollback Scope:**
- If critical failure detected: full platform rollback (target < 30 minutes)
- If department-level failure: rollback only that department (target < 5 minutes)
- If individual agent failure: rollback only that agent (target < 60 seconds)
- Rollback decision: CE-AI or EVP-Infrastructure for platform-level; Dept Manager for department-level

---

## Document Control

| Version | Date | Author | Changes |
|---|---|---|---|
| 1.0 | 2026-07-09 | Deployment Operations (Dept-22) | Initial release |

**Review Cycle:** This document is reviewed quarterly by the Deployment Operations department and the Change Advisory Board. Updates are published to the internal documentation portal at `https://docs.aic.internal/infrastructure/deployment-model`.

**Appendices:**
- Appendix A: Deployment Runbook (linked separately)
- Appendix B: Emergency Response Playbook (linked separately)
- Appendix C: CAB Standard Operating Procedures (linked separately)
- Appendix D: Environment Configuration Reference (linked separately)
- Appendix E: Rollback Decision Tree (linked separately)

---

*End of Document — 22. Deployment Model*
