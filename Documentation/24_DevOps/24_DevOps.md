# DEVOPS ARCHITECTURE

## Infrastructure, CI/CD, and Platform Operations

---

| Document ID | IEP-ARCH-DEVOPS-001 |
|---|---|
| Version | 1.0.0 |
| Status | Approved |
| Classification | Enterprise Internal |
| Effective Date | 2026-07-07 |
| Approving Authority | Office of the Chief Architect |
| Custodian | DevOps Architecture Team |
| Security Reference | IEP-ARCH-SEC-001 |
| API Reference | IEP-ARCH-API-001 |

---

## TABLE OF CONTENTS

1. EXECUTIVE OVERVIEW
2. DEVOPS PHILOSOPHY
3. INFRASTRUCTURE
4. CI/CD PIPELINE
5. CONTAINERIZATION
6. ORCHESTRATION
7. MONITORING AND OBSERVABILITY
8. DISASTER RECOVERY
9. COST MANAGEMENT
10. ENVIRONMENTS
11. APPENDICES

---

## 1. EXECUTIVE OVERVIEW

### 1.1 Purpose

The DevOps Architecture defines the infrastructure, deployment, monitoring, and operational practices that ensure the Islamic Education Platform is reliable, scalable, secure, and cost-efficient.

### 1.2 Core Principle

**Infrastructure as Code, everything automated, everything monitored.**

---

## 2. DEVOPS PHILOSOPHY

| Principle | Description |
|---|---|
| **IaC Everything** | All infrastructure defined in code (Terraform) |
| **Immutable Deployments** | No manual changes to running infrastructure |
| **Shift Left** | Security and testing earlier in the pipeline |
| **Observability** | Every component produces metrics, logs, traces |
| **Chaos Engineering** | Proactively test failure scenarios |
| **Cost Awareness** | Every deployment decision considers cost |

---

## 3. INFRASTRUCTURE

### 3.1 Cloud Provider

| Provider | Service | Purpose |
|---|---|---|
| **AWS (Primary)** | EKS, RDS, ElastiCache, OpenSearch, S3 | Core infrastructure |
| **Vercel (Web)** | Edge Functions, ISR, CDN | Web application hosting |
| **Cloudflare** | DNS, DDoS protection, CDN | Global edge network |

### 3.2 Infrastructure Components

| Component | Service | Specification |
|---|---|---|
| **Kubernetes** | AWS EKS | Multi-AZ, auto-scaling, spot instances |
| **Database** | AWS RDS PostgreSQL | Multi-AZ, automated backups, 500GB - 5TB |
| **Cache** | AWS ElastiCache Redis | Cluster mode, Multi-AZ, 50GB+ |
| **Search** | AWS OpenSearch | 3-node cluster, 2TB |
| **Object Storage** | AWS S3 + MinIO (internal) | S3 Standard + Glacier for archival |
| **CDN** | Cloudflare / AWS CloudFront | Global edge caching |
| **Message Queue** | AWS MQ (RabbitMQ) / MSK (Kafka) | HA cluster |

### 3.3 Networking

| Component | Configuration |
|---|---|
| **VPC** | /16 with public, private, and database subnets |
| **Load Balancing** | ALB (HTTP), NLB (gRPC/WebSocket) |
| **Service Mesh** | Istio for microservice communication |
| **DNS** | Route53 with health checks and failover |
| **NAT** | NAT Gateways per AZ for private subnets |

---

## 4. CI/CD PIPELINE

### 4.1 Pipeline Stages

```
Code Push
    │
    ▼
┌──────────────────────────────┐
│ 1. Lint & Type Check         │
│ • ESLint, Prettier, tsc      │
• 5 min
└──────────────────────────────┘
    │
    ▼
┌──────────────────────────────┐
│ 2. Unit Tests                │
│ • Vitest / Jest / Pytest     │
• 10 min
└──────────────────────────────┘
    │
    ▼
┌──────────────────────────────┐
│ 3. Build & Scan              │
│ • Docker build               │
│ • Trivy vulnerability scan   │
│ • Snyk dependency scan       │
• 15 min
└──────────────────────────────┘
    │
    ▼
┌──────────────────────────────┐
│ 4. Deploy to Staging         │
│ • Helm chart deployment      │
│ • Smoke tests                │
• 10 min
└──────────────────────────────┘
    │
    ▼
┌──────────────────────────────┐
│ 5. Integration Tests         │
│ • API contract tests         │
│ • E2E tests (Playwright)     │
│ • Performance tests (k6)     │
• 20 min
└──────────────────────────────┘
    │
    ▼
┌──────────────────────────────┐
│ 6. Deploy to Production      │
│ • Blue-green deployment      │
│ • Health check monitoring    │
│ • Gradual rollout (10/30/100%)│
• 30 min
└──────────────────────────────┘
```

### 4.2 Deployment Strategies

| Strategy | Use Case | Downtime | Risk |
|---|---|---|---|
| **Blue-Green** | Web services, APIs | Zero | Instant rollback |
| **Rolling Update** | Stateless workers | Zero | Gradual |
| **Canary** | High-risk changes | Zero | 10% → 30% → 100% |
| **Feature Flags** | Feature toggles | Zero | Instant disable |

---

## 5. CONTAINERIZATION

| Component | Base Image | Size Limit | Registry |
|---|---|---|---|
| **Node.js Services** | node:20-alpine | < 200 MB | ECR |
| **Python Services** | python:3.12-slim | < 300 MB | ECR |
| **Next.js Web** | node:20-alpine (builder) → nginx:alpine (runner) | < 100 MB | ECR |
| **Sidecars** | Various | < 50 MB | ECR |

---

## 6. ORCHESTRATION

### 6.1 Kubernetes Configuration

| Component | Configuration |
|---|---|
| **Cluster** | EKS 1.29+, Multi-AZ (3 AZs), managed node groups |
| **Node Types** | On-demand (base), Spot (workers), GPU (AI inference) |
| **Autoscaling** | Cluster Autoscaler + HPA/VPA |
| **Storage** | EBS CSI driver, EFS for shared storage |
| **Ingress** | ALB Ingress Controller + Istio Gateway |
| **Secrets** | External Secrets Operator + AWS Secrets Manager |

### 6.2 Resource Allocation

| Service | CPU | Memory | Replicas (min-max) |
|---|---|---|---|
| **API Gateway** | 2 | 4 GB | 3-20 |
| **Content Service** | 2 | 4 GB | 3-15 |
| **AI Service** | 4 | 16 GB | 2-10 |
| **Web App** | 2 | 4 GB | 3-20 |
| **Worker (Async)** | 1 | 2 GB | 2-50 |

---

## 7. MONITORING AND OBSERVABILITY

### 7.1 Three Pillars

| Pillar | Tool | Retention |
|---|---|---|
| **Metrics** | Prometheus + Grafana | 30 days (high-res), 1 year (aggregated) |
| **Logs** | Loki + Grafana | 30 days |
| **Traces** | Tempo / Jaeger | 14 days |

### 7.2 Key Dashboards

| Dashboard | Content |
|---|---|
| **Platform Overview** | Request rate, error rate, latency, active users |
| **Service Health** | Per-service CPU, memory, request count, P50/P95/P99 |
| **Database** | Connection count, query performance, slow queries |
| **AI Service** | Token usage, generation time, cache hit rate |
| **Cost** | Per-service cost breakdown, trend |
| **SLA/SLO** | Uptime, error budget, SLO attainment |

### 7.3 Alerting

| Severity | Response Time | Channels |
|---|---|---|
| **Critical** | < 5 min | PagerDuty, SMS, Slack |
| **Warning** | < 15 min | Slack, email |
| **Info** | < 1 hour | Slack |

---

## 8. DISASTER RECOVERY

| Scenario | RTO | RPO | Strategy |
|---|---|---|---|
| **AZ Failure** | < 5 min | < 1 min | Multi-AZ deployment, automatic failover |
| **Region Failure** | < 1 hour | < 15 min | Cross-region replication (passive standby) |
| **Data Corruption** | < 4 hours | < 1 hour | Point-in-time recovery (PITR) |
| **Accidental Deletion** | < 1 hour | < 5 min | Deletion protection, backup |
| **Full Disaster** | < 4 hours | < 1 hour | Warm standby in second region |

---

## 9. COST MANAGEMENT

| Strategy | Implementation | Estimated Savings |
|---|---|---|
| **Spot Instances** | Spot for worker nodes | 60-70% |
| **Right-Sizing** | Regular resource review | 20-30% |
| **Auto-Scaling** | Scale down to 0 during low usage | 40-50% for non-critical |
| **Reserved Instances** | RI for baseline capacity | 30-40% |
| **S3 Lifecycle** | Move cold data to Glacier | 70% |
| **Data Transfer** | CloudFront for egress | Variable |

---

## 10. ENVIRONMENTS

| Environment | Purpose | Size | Deploy Trigger |
|---|---|---|---|
| **Development** | Developer testing | Minimal (shared) | Branch push |
| **Staging** | Pre-production validation | 50% of prod | PR merge to main |
| **Production** | Live platform | Full | Manual approval + Canary |
| **DR** | Disaster recovery | Minimum + scale on failover | Manual activation |

---

## 11. APPENDICES

### 11.1 Related Documents

| Document ID | Title | Relationship |
|---|---|---|
| IEP-ARCH-SEC-001 | Security Architecture | Infrastructure security |
| IEP-ARCH-API-001 | API Integration | Service deployment |
| IEP-ARCH-TEST-001 | Testing Architecture | CI/CD test integration |

### 11.2 Revision History

| Version | Date | Author | Changes |
|---|---|---|---|
| 1.0.0 | 2026-07-07 | Office of the Chief Architect | Initial approved release |
