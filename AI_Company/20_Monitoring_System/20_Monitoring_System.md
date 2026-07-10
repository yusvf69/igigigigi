# 20 — Monitoring System

> **Enterprise Monitoring Architecture for an Autonomous AI Company of 8,500 Agents**
> *Part of the "Autonomous AI Company: Agents as Employees" Design Series*

---

| Metadata | Value |
|---|---|
| Document ID | 20-MON-v2.4 |
| Domain | Infrastructure & Operations |
| Applicable To | All 8,500 Agents, 130 Teams, 33 Departments, 7 Executives, CE-AI |
| Platform Context | AI-Powered Islamic Education Platform |
| Document Status | Final |
| Last Updated | 2026-07-09 |

---

## Table of Contents

1. [Monitoring Philosophy](#1-monitoring-philosophy)
2. [Monitoring Architecture](#2-monitoring-architecture)
3. [Agent Health Monitoring](#3-agent-health-monitoring)
4. [Infrastructure Monitoring](#4-infrastructure-monitoring)
5. [Workflow Monitoring](#5-workflow-monitoring)
6. [Quality Monitoring](#6-quality-monitoring)
7. [Business Monitoring](#7-business-monitoring)
8. [Security Monitoring](#8-security-monitoring)
9. [Alerting System](#9-alerting-system)
10. [Dashboards](#10-dashboards)
11. [Tracing and Observability](#11-tracing-and-observability)
12. [SLAs and SLOs](#12-slas-and-slos)
13. [Monitoring System Itself (Watchdog)](#13-monitoring-system-itself-watchdog)

---

## 1. Monitoring Philosophy

### 1.1 Core Principles

The Monitoring System is the nervous system of the Autonomous AI Company. It provides real-time visibility into every agent, every pipeline, every transaction, and every piece of infrastructure across the 8,500-agent ecosystem. The philosophy is rooted in **total observability**: nothing is exempt from monitoring, and every signal is captured, stored, and actionable.

**The Ten Commandments of Monitoring:**

1. **Monitor Everything** — Every agent, service, database, queue, cache, API gateway, GPU node, and storage volume must emit telemetry. No blind spots.
2. **Measure What Matters** — Not all metrics are equal. Distinguish between signals (actionable) and noise (informational). Focus monitoring investment on metrics that drive decisions and alerts.
3. **Proactive Over Reactive** — Detect anomalies before they become incidents. Alert on leading indicators (e.g., memory trending up) not just lagging indicators (e.g., OOM kill).
4. **Observability Over Monitoring** — Monitoring tells you *what* is broken. Observability lets you ask *why*. Invest in traces, structured logging, and high-cardinality metrics.
5. **Single Pane of Glass** — A unified monitoring platform (Grafana) that aggregates all data sources. No switching between tools to understand an incident.
6. **Context is King** — Every alert, metric, and trace must include contextual metadata: agent_id, team_id, department_id, pipeline_id, workflow_instance_id, region, version.
7. **Monitor the Monitors** — The monitoring system itself must be watched. A watchdog daemon runs independently to detect monitoring system failure.
8. **Error Budgets Drive Culture** — SLOs and error budgets define the acceptable failure rate. Teams manage their own error budgets. Running out of error budget means stop shipping.
9. **Cost-Aware Monitoring** — Monitoring has a cost (storage, compute, network). Sampling strategies, retention policies, and aggregation tiers balance visibility with cost.
10. **Security is a Monitoring Concern** — Security monitoring is not a separate function; it is embedded into the monitoring philosophy. Every metric is a potential security signal.

### 1.2 Proactive vs Reactive Detection

| Aspect | Proactive Detection | Reactive Detection |
|---|---|---|
| **Detection Timing** | Before failure occurs | After failure occurs |
| **Signals** | Trends, rate of change, forecasted exhaustion | Error spikes, downtime, crash |
| **Alerts** | Warning-level (P3-P4) | Critical-level (P0-P1) |
| **Example** | Memory usage trending up over 4 hours predicting OOM in 30 min | Agent crash P0 alert |
| **Action** | Auto-remediation, scaling, preemptive restart | Incident response, root cause analysis |
| **Target Ratio** | 70% of alerts should be proactive | 30% of alerts reactive |

**Proactive Detection Mechanisms:**
- **Trend Analysis:** Moving averages over 5min, 15min, 60min. Non-linear regression to predict when a threshold will be breached.
- **Anomaly Detection:** ML models trained on historical metric behavior. Seasonal decomposition (daily, weekly patterns) to detect deviations.
- **Predictive Scaling:** Based on historical load patterns predict when resource exhaustion will occur and trigger scaling preemptively.
- **Health Score Degradation:** Composite health scores degrade gradually as leading indicators worsen. Alert when score drops below thresholds.

**Reactive Detection Mechanisms:**
- **Threshold Breach:** Hard limits (e.g., CPU > 95% for 5min).
- **Error Rate Spike:** Sudden increase in error rate > 5x baseline triggers P0.
- **Silence Detection:** Agent stops emitting heartbeat for > 30 seconds.
- **Dead Man Switch:** Monitoring system goes silent for > 60 seconds triggers external watchdog.

### 1.3 The Four Pillars of Observability

| Pillar | Description | Storage System | Query Interface |
|---|---|---|---|
| **Logs** | Immutable, timestamped records of discrete events (JSON) | Elasticsearch cluster (3 nodes, 48TB) | Kibana, ES DSL |
| **Metrics** | Numeric measurements at regular intervals, high cardinality | Prometheus (Thanos for long-term) | PromQL, Grafana |
| **Traces** | End-to-end record of a single request across distributed services | Jaeger (Cassandra backend) | Jaeger UI |
| **Events** | Business-significant occurrences (deployments, scaling, config changes) | Event Store (PostgreSQL) | Custom dashboard |

**Retention Policies:**

| Data Type | Hot Storage | Cool Storage | Cold Storage | Total Retention |
|---|---|---|---|---|
| Metrics (1s resolution) | 7 days | 30 days (5s rollup) | 1 year (1min rollup) | Indefinite (archived) |
| Logs (structured) | 30 days | 90 days | 1 year | 7 years (compliance) |
| Traces (100% sampled) | 7 days | — | — | 7 days (1% sampled) |
| Events | 90 days | 1 year | — | 7 years |
| Heartbeats | 30 days | 90 days | — | 90 days |

**Storage Cost Optimization:**
- **Metrics:** Roll up older data (5s to 1min to 15min to 1hr) reducing storage 300x over a year.
- **Logs:** Remove PII after 90 days. Compress (gzip) before archival. Index lifecycle management (ILM) in Elasticsearch.
- **Traces:** 100% sampled traces kept 7 days. Head-based sampling; tail-based sampling retains all error traces.
- **Events:** Single row per event; cheap to retain. Purge after 7 years.

### 1.4 Single Pane of Glass

Grafana configured as the single entry point for all monitoring data:

| Data Source | Plugin | Query Language | Use Case |
|---|---|---|---|
| Prometheus/Thanos | Built-in Prometheus | PromQL | Metrics dashboards, alerting |
| Elasticsearch | Built-in ES | Lucene/ES DSL | Logs search, log-based metrics |
| Jaeger | Jaeger plugin | TraceQL | Trace waterfall visualization |
| PostgreSQL | Built-in | SQL | Business metrics, events, config |
| AWS CloudWatch | CloudWatch plugin | CloudWatch Metrics | AWS infrastructure metrics |
| PagerDuty | PagerDuty plugin | — | Incident timeline on dashboards |
| GitHub/GitLab | Git plugin | — | Deployment events, change tracking |

**Grafana Organization:**
- Organizations: One per department (33) — each sees only its own dashboards
- Folders: One per department, sub-folders for teams (130)
- Teams: Mapped to company teams with RBAC (Viewer/Editor/Admin)
- Annotations: Auto-deployment, scaling, config change, alert firing annotations

### 1.5 Cost of Monitoring

| Cost Category | Monthly Estimate | % of Total Infra Cost |
|---|---|---|
| Prometheus + Thanos storage | $8,400 | 2.5% |
| Elasticsearch cluster (3 nodes) | $6,200 | 1.8% |
| Jaeger + Cassandra | $3,100 | 0.9% |
| Grafana (hosted, 5 replicas) | $1,800 | 0.5% |
| Monitoring agent/sidecar overhead | $4,500 | 1.3% |
| Data transfer (metrics/logs/traces) | $2,100 | 0.6% |
| **Total** | **$26,100** | **7.6%** |

**Target:** Keep monitoring costs below 10% of total infrastructure cost. Re-evaluate retention policies quarterly.

---


## 2. Monitoring Architecture

### 2.1 High-Level Architecture Diagram

```
+-----------------------------------------------------------------------------+
|                         AGENT ECOSYSTEM                                       |
|  +----------+  +----------+  +----------+  +----------+                       |
|  | Agent    |  | Agent    |  | Agent    |  | Agent    |  ...8500             |
|  | Sidecar  |  | Sidecar  |  | Sidecar  |  | Sidecar  |                       |
|  +----+-----+  +----+-----+  +----+-----+  +----+-----+                       |
|       |              |              |              |                            |
|       |  Metrics     |  Logs        |  Traces     |  Events                    |
|       |  (5s push)   |  (stream)    |  (span)     |  (event)                   |
+-------+--------------+--------------+--------------+----------------------------+
        |              |              |              |
        v              v              v              v
+-----------------------------------------------------------------------------+
|                       AGGREGATION TIER                                        |
|  +----------------+ +------------+ +------------+ +--------------+            |
|  | Prometheus     | | Logstash   | | Jaeger     | | Event Bus    |            |
|  | Server (x3)    | | (x2)       | | Collector  | | (Kafka)      |            |
|  | Service        | | Aggregator | | (x2)       | |              |            |
|  | Discovery      | | & Parser   | | Processor  | | Stream       |            |
|  +-------+--------+ +-----+------+ +------+-----+ +------+-------+            |
+-----------+----------------+-----------------+----------------+----------------+
            |                |                 |                |
            v                v                 v                v
+-----------------------------------------------------------------------------+
|                         STORAGE TIER                                           |
|  +--------------+  +--------------+  +--------------+  +---------+           |
|  | Thanos       |  | Elasticsearch|  | Jaeger       |  | Postgres|           |
|  | Store        |  | Cluster      |  | Cassandra   |  | Event   |           |
|  | (ObjectStore)|  | (3 data +   |  | Backend      |  | Store   |           |
|  |              |  |  3 master)   |  |              |  |         |           |
|  +------+-------+  +------+-------+  +------+-------+  +----+----+           |
+---------+-----------------+------------------+---------------+----------------+
          |                 |                  |               |
          v                 v                  v               v
+-----------------------------------------------------------------------------+
|                  ALERTING & VISUALIZATION TIER                                 |
|  +---------------+  +---------------+  +---------------+                      |
|  | Grafana       |  | Alertmanager  |  | PagerDuty     |                      |
|  | (Dashboards)  |  | (Rule Engine) |  | (On-call)     |                      |
|  +---------------+  +-------+-------+  +-------+-------+                      |
|                             |                  |                               |
|                             v                  v                               |
|                      +--------------+  +--------------+                        |
|                      | Slack Bot   |  | SMS Gateway  |                        |
|                      | (ops channel)|  | (Twilio)     |                        |
|                      +--------------+  +--------------+                        |
+-----------------------------------------------------------------------------+
```

### 2.2 Data Collection Layer

#### 2.2.1 Agent Sidecar

Every agent runs a sidecar process (co-located in the same pod/container) responsible for telemetry emission. This is non-negotiable — no agent runs without a sidecar.

**Sidecar Responsibilities:**
- Health state aggregation (heartbeat, resource usage, task state)
- Metric formatting and batching
- Log forwarding (stdout/stderr to structured JSON to Logstash)
- Trace span collection and propagation
- Event emission for lifecycle changes (startup, shutdown, scaling)

**Sidecar Configuration:**

```yaml
monitoring_sidecar:
  heartbeat_interval: 5s
  metrics_interval: 5s
  log_interval: 1s
  trace_sampling:
    default: 0.01
    high_priority: 0.10
    critical: 1.00
  metrics_batch_size: 1000
  log_batch_size: 500
  retry:
    max_retries: 3
    backoff: exponential
  resource_limits:
    cpu: 0.1 cores
    memory: 128MB
```

**Data Emission Endpoints:**

| Signal | Protocol | Endpoint | Port |
|---|---|---|---|
| Metrics | HTTP (Prometheus pushgateway mode) | prometheus.telemetry.svc.cluster.local:9091 | 9091 |
| Logs | TCP (Lumberjack/Beats protocol) | logstash.telemetry.svc.cluster.local:5044 | 5044 |
| Traces | gRPC (Jaeger protocol) | jaeger-collector.telemetry.svc.cluster.local:14250 | 14250 |
| Events | HTTP (REST) | event-bus.telemetry.svc.cluster.local:8080 | 8080 |
| Heartbeat | HTTP (REST) | heartbeat-receiver.telemetry.svc.cluster.local:8081 | 8081 |

#### 2.2.2 Infrastructure Telemetry

| Component | Exporter | Port | Metrics Count |
|---|---|---|---|
| Kubernetes (kube-state-metrics) | Prometheus operator | 8080 | 1,200+ |
| Node exporter (per host) | Node exporter | 9100 | 750+ |
| GPU exporter (NVIDIA) | DCGM exporter | 9400 | 150+ |
| PostgreSQL exporter | postgres_exporter | 9187 | 200+ |
| Redis exporter | redis_exporter | 9121 | 100+ |
| Kafka exporter | kafka_exporter | 9308 | 300+ |
| Nginx/API Gateway | nginx_exporter | 9113 | 80+ |
| AWS EBS/EC2 | CloudWatch exporter | — | 400+ |

**Total distinct metrics collected per minute: approximately 5 million**

### 2.3 Aggregation Tier

#### 2.3.1 Prometheus Servers

Three Prometheus servers run in a highly available configuration:

| Instance | Role | Retention | Storage |
|---|---|---|---|
| prometheus-0 | Primary writer | 7 days | 2TB SSD |
| prometheus-1 | Primary writer | 7 days | 2TB SSD |
| prometheus-2 | Sidecar receiver (backup) | 7 days | 2TB SSD |

**Configuration:**
- Scrape interval: 15s (default), 5s for critical services
- Evaluation interval: 15s for rules, 5s for critical rules
- External labels: cluster=prod, region=us-east-1
- Alertmanager endpoint: alertmanager.telemetry.svc.cluster.local:9093

#### 2.3.2 Thanos (Long-Term Metrics Storage)

Thanos extends Prometheus for global querying and unlimited retention:

| Component | Role | Replicas |
|---|---|---|
| Thanos Sidecar | Attached to each Prometheus | 3 |
| Thanos Store Gateway | Object storage reader | 2 |
| Thanos Compactor | Downsample and compact blocks | 1 |
| Thanos Querier | Global query endpoint | 2 |
| Thanos Ruler | Rule evaluation against historical data | 2 |
| Object Store (S3-compatible) | Long-term metric storage | — |

**Downsampling Rules:**
- Raw data (5s resolution): Keep 7 days
- 5-minute resolution rollup: Keep 30 days
- 1-hour resolution rollup: Keep 365 days
- 1-day rollup: Keep indefinitely

#### 2.3.3 Elasticsearch Cluster

Three-node Elasticsearch cluster with high availability:

| Node | Role | RAM | Storage | CPU |
|---|---|---|---|---|
| es-master-0 | Master + Data | 32GB | 16TB SSD | 8 cores |
| es-master-1 | Master + Data | 32GB | 16TB SSD | 8 cores |
| es-master-2 | Master + Data | 32GB | 16TB SSD | 8 cores |

**Indexing:**
- Index per day: logs-2026-07-09, traces-2026-07-09
- Shards: 3 primary shards, 1 replica
- Refresh interval: 30 seconds
- ILM policy: Hot (7 days) to Warm (30 days) to Cold (365 days) to Delete

#### 2.3.4 Jaeger Collector + Cassandra

| Component | Replicas | Storage |
|---|---|---|
| Jaeger Collector | 2 | Stateless |
| Jaeger Query | 2 | Stateless |
| Cassandra Backend | 3 | 2TB each, SSD |
| Spark (for analytics) | 1 | Stateless |

**Sampling Strategies:**
- **Head sampling:** probabilistic at collector level. Default 5% for production.
- **Tail sampling:** retain all traces with errors regardless of head sampling decision.
- **Rate limiting:** max 10,000 spans/second total across all collectors.
- **Per-agent sampling override:** dynamically adjustable through configuration API.

### 2.4 Alerting Tier

#### 2.4.1 Alertmanager

Three Alertmanager replicas running in a mesh configuration:

| Feature | Configuration |
|---|---|
| Grouping | Group by alertname, severity, team |
| Group wait | 30 seconds |
| Group interval | 5 minutes |
| Repeat interval | P0: 5min, P1: 15min, P2: 30min, P3: 1hr, P4: 4hr |
| Inhibition | If P0 fires, inhibit related P1/P2 for same service |

#### 2.4.2 Notification Service

| Channel | Priority | Template | Limits |
|---|---|---|---|
| SMS (Twilio) | P0 only | [P0] {alertname} on {agent} - {summary} | 50/day per recipient |
| Phone (Twilio voice) | P0 only | TTS: Critical alert: {alertname}. Please acknowledge. | 10/day per recipient |
| Slack Ops Channel | P0, P1 | Rich card with metric graph, runbook link, acknowledge button | Unlimited |
| Slack Team Channel | P2, P3 | Brief notification with dashboard link | Unlimited |
| Email | P4 | Daily/weekly digest | Once per digest |
| Dashboard surge | P4 | Highlight on Grafana dashboard | N/A |

#### 2.4.3 Alert Rules Engine

Alert rules are defined in Prometheus rules format:

```
alert: HighAgentErrorRate
expr: rate(agent_errors_total[5m]) / rate(agent_requests_total[5m]) > 0.05
for: 2m
labels:
  severity: p1
  team: content
  department: curriculum
annotations:
  summary: "Agent {{ $labels.agent_id }} error rate is {{ $value | humanizePercentage }}"
  description: "Agent {{ $labels.agent_name }} has {{ $value | humanizePercentage }} error rate over 5 minutes, exceeding 5% threshold."
  runbook: "https://runbooks.internal/agents/high-error-rate"
  dashboard: "https://grafana.internal/d/agent-detail?agent={{ $labels.agent_id }}"
```

**Rule Categories:**
- **Red rules:** Breach of hard SLO (100% condition). P0/P1.
- **Yellow rules:** Approaching SLO breach. Warning only. P2/P3.
- **Blue rules:** Informational. No immediate action. P4.
- **Green rules:** Auto-remediation attempt before alert. No human notification if successful.

### 2.5 Visualization Tier

#### 2.5.1 Grafana Deployment

| Component | Replicas | Resources |
|---|---|---|
| Grafana server | 5 (behind LB) | 4 vCPU, 8GB RAM each |
| Grafana DB (PostgreSQL) | 1 (HA via replication) | 2 vCPU, 4GB RAM, 100GB SSD |
| Grafana Image Renderer | 2 | 2 vCPU, 4GB RAM |

**Grafana Configuration:**
- Auth: OAuth2 via Keycloak (SSO with company LDAP)
- Anonymous access: Disabled
- Data sources: 7 (Prometheus, ES, Jaeger, Postgres, CloudWatch, Loki, InfluxDB)
- Alerting: Grafana alerts enabled (secondary alerting channel)
- Plugins: Jaeger, PagerDuty, Business Analytics, Zabbix (legacy)
- Provisioning: All dashboards, data sources, and alerts provisioned via YAML files in Git

---


## 3. Agent Health Monitoring

### 3.1 Heartbeat System

Every agent (8,500 total) must send a heartbeat every 5 seconds. The heartbeat is a lightweight HTTP POST to the heartbeat receiver service containing minimal metadata.

**Heartbeat Payload:**

```json
{
  "agent_id": "agent-0428-7f3a",
  "agent_name": "story-writer-quran-level3",
  "agent_type": "content-generator",
  "team_id": "team-017-curriculum-content",
  "department_id": "dept-04-curriculum",
  "hostname": "ip-10-0-42-7.ec2.internal",
  "pod_name": "agent-story-writer-quran-l3-7f3a-x9k2m",
  "cluster": "prod-us-east-1",
  "version": "2.4.1",
  "state": "processing",
  "uptime_seconds": 847201,
  "heartbeat_sequence": 169443,
  "timestamp": "2026-07-09T14:32:17.042Z"
}
```

**Heartbeat SLA:**

| Missed Heartbeats | Status | Action |
|---|---|---|
| 0 | Healthy | Normal operations |
| 1 | Healthy | No action |
| 2 | Healthy | Internal flag set |
| 3 | Suspected | P3 alert raised |
| 4 | Suspected | Escalated to P2, auto-diagnostics initiated |
| 5 | Confirmed Down | P1 alert |
| >10 | Declared Dead | P0 alert, auto-restart/recreate |

**Heartbeat Redundancy:**
- Primary heartbeat receiver: 3 replicas behind a load balancer
- Secondary heartbeat receiver: 2 replicas on separate infrastructure (different AZ)
- If primary unreachable agent sidecar automatically switches to secondary endpoints
- Heartbeat data written to both Redis (real-time status) and Prometheus (historical)

**Real-Time Agent Status:**
- 8,420 Healthy (99.06%)
- 42 Degraded
- 18 Suspected
- 12 Confirmed Down
- 8 Declared Dead

### 3.2 Resource Monitoring

Each agent sidecar collects resource usage metrics every 5 seconds and pushes to Prometheus.

**Resource Metrics Collected:**

| Metric Name | Type | Unit | Description | Alert Threshold |
|---|---|---|---|---|
| agent_cpu_usage_percent | Gauge | % of CPU limit | Current CPU usage | > 90% for 5min P2 |
| agent_cpu_throttled_seconds | Counter | seconds | CPU throttled time | > 30s in 5min P2 |
| agent_memory_usage_bytes | Gauge | bytes | Current memory usage | > 90% of limit P2 |
| agent_memory_working_set_bytes | Gauge | bytes | RSS + cache | > 80% of limit P3 |
| agent_gpu_utilization_percent | Gauge | % | GPU compute utilization | > 95% for 2min P2 |
| agent_gpu_memory_usage_bytes | Gauge | bytes | GPU memory used | > 90% of limit P2 |
| agent_gpu_temperature_celsius | Gauge | degrees C | GPU temperature | > 80C P2 > 85C P1 |
| agent_network_receive_bytes | Counter | bytes/s | Inbound network | > 100MB/s for 1min P2 |
| agent_network_transmit_bytes | Counter | bytes/s | Outbound network | > 100MB/s for 1min P2 |
| agent_disk_read_bytes | Counter | bytes/s | Disk read throughput | > 50MB/s sustained P2 |
| agent_disk_write_bytes | Counter | bytes/s | Disk write throughput | > 50MB/s sustained P2 |
| agent_disk_usage_percent | Gauge | % | Disk usage of agent volume | > 85% P3 > 95% P2 |

**Resource Baseline Profiles (7-day rolling window):**

| Agent Type | CPU Baseline | Memory Baseline | GPU Baseline | Network Baseline |
|---|---|---|---|---|
| Content Generator (LLM-based) | 65% +/- 15% | 4.2GB +/- 0.8GB | 72% +/- 12% | 2.1MB/s +/- 0.5MB/s |
| Knowledge Graph Builder | 45% +/- 10% | 8.1GB +/- 1.2GB | 12% +/- 5% | 5.4MB/s +/- 1.2MB/s |
| Quiz Generator | 55% +/- 12% | 3.0GB +/- 0.5GB | 45% +/- 15% | 1.0MB/s +/- 0.3MB/s |
| AI Teacher (real-time) | 40% +/- 10% | 2.5GB +/- 0.4GB | 0% | 0.8MB/s +/- 0.2MB/s |
| Verification Agent | 70% +/- 10% | 6.0GB +/- 1.0GB | 60% +/- 15% | 3.0MB/s +/- 1.0MB/s |

**Auto-Remediation Based on Resource Usage:**

| Condition | Action |
|---|---|
| CPU > 90% for 10 minutes | Scale agent horizontally (increase replicas) |
| Memory > 90% for 5 minutes | Restart agent (memory leak recovery) |
| GPU temperature > 85C | Throttle agent content generation rate by 50% |
| Disk usage > 90% | Run log rotation then garbage collection; if still >90% expand volume |
| Network > 80% of pod limit | Increase pod network bandwidth allocation |
| Any resource > 95% for 1 min | Kill and restart agent |

### 3.3 Latency Monitoring

Every agent tracks per-task/request latency and exposes histograms.

**Latency Metrics (Prometheus Histogram):**

| Metric | Buckets (seconds) | Export |
|---|---|---|
| agent_request_duration_seconds | [0.01, 0.05, 0.1, 0.25, 0.5, 1, 2, 5, 10, 30, 60, 120, 300] | All agents |
| agent_task_duration_seconds | [0.1, 0.5, 1, 2, 5, 10, 30, 60, 300, 600, 1800, 3600] | Content generation agents only |

**Latency Quantile Targets:**

| Agent Type | P50 Target | P95 Target | P99 Target | Alert on P95 |
|---|---|---|---|---|
| AI Teacher | < 200ms | < 1s | < 3s | P0 |
| Content Generator (short) | < 5s | < 15s | < 30s | P1 |
| Content Generator (long) | < 30s | < 60s | < 120s | P1 |
| Knowledge Graph Builder | < 10s | < 30s | < 60s | P2 |
| Quiz Generator | < 5s | < 15s | < 30s | P1 |
| Verification Agent | < 10s | < 30s | < 60s | P2 |
| Recommendation Agent | < 100ms | < 500ms | < 1s | P2 |
| Search Agent | < 200ms | < 1s | < 2s | P1 |

**Latency Anomaly Detection:**
- Moving average latency over 5min 15min 60min windows with deviation alerts
- Latency decomposition: queue wait time vs processing time vs post-processing time
- External dependency latency: if upstream API latency spikes alert on upstream failure
- Seasonal latency patterns: if latency always higher at certain times exclude from anomaly detection

### 3.4 Error Rate Monitoring

Error rate is the ratio of failed tasks to total tasks measured per minute.

**Error Rate Formula:**
error_rate = count(agent_errors_total[1m]) / count(agent_tasks_total[1m])

**Error Rate Thresholds:**

| Error Rate (5min) | Severity | Action |
|---|---|---|
| < 1% | P4 (info) | Log only |
| 1% to 3% | P3 (warning) | Investigate during working hours |
| 3% to 10% | P2 (medium) | Immediate investigation |
| 10% to 25% | P1 (high) | Page on-call engineer, auto-disable agent if sustained |
| > 25% | P0 (critical) | All-hands incident, auto-disable agent after 1 minute |

**Error Categories:**

| Error Category | Description | Auto-Remediation |
|---|---|---|
| timeout | Task exceeded max execution time | Restart agent, increase timeout |
| oom_killed | Agent killed by OOM killer | Increase memory limit, restart |
| llm_failure | LLM API error (rate limit, bad request, server error) | Retry with backoff, switch to fallback model |
| validation_failure | Output failed internal validation | Decrease generation temperature, regenerate |
| dependency_failure | Downstream service unavailable | Retry with backoff, alert dependency owner |
| config_error | Invalid configuration at runtime | Auto-fix known patterns, notify team |
| network_error | Network connectivity issues | Retry on different host, alert network team |
| data_error | Input data corrupted malformed or missing | Require human intervention |

### 3.5 Throughput Monitoring

**Throughput Metrics:**

| Metric | Type | Description |
|---|---|---|
| agent_tasks_total | Counter | Total tasks completed (ever) |
| agent_tasks_active | Gauge | Currently active in-flight tasks |
| agent_tasks_queued | Gauge | Tasks waiting in input queue |
| agent_tasks_rate_per_minute | Gauge (calculated) | Tasks completed per minute |
| agent_input_queue_depth | Gauge | Depth of message queue |

**Throughput Baselines:**

| Agent Type | Expected Throughput | Peak Throughput | Alert if Below |
|---|---|---|---|
| Content Generator | 12 tasks/min | 30 tasks/min | < 6 tasks/min for 5min |
| Knowledge Graph Builder | 8 tasks/min | 20 tasks/min | < 4 tasks/min for 5min |
| Quiz Generator | 15 tasks/min | 40 tasks/min | < 7 tasks/min for 5min |
| AI Teacher | 60 tasks/min | 200 tasks/min | < 30 tasks/min for 5min |
| Verification Agent | 10 tasks/min | 25 tasks/min | < 5 tasks/min for 5min |

**Throughput Anomaly Detection:**
- Sudden drop (> 50% in 1 minute): Potential deadlock or dependency failure
- Sudden spike (> 200% of normal): Potential runaway agent or misconfiguration
- Slow decline (over hours): Memory leak or gradual performance degradation
- Zero throughput for > 2 minutes: Agent stuck in error loop

### 3.6 Agent State Monitoring

Each agent maintains an internal state machine:

```
Starting -> Idle <-> Processing -> Complete Shutdown
               ^           |
               |           v
            Paused       Error -> Starting (restart)
```

**State Duration Tracking:**

| State | Typical Duration | Alert Threshold |
|---|---|---|
| Starting | 2-10 seconds | > 60s P2 |
| Idle | 0-30 seconds | > 300s P3 |
| Processing | 0.1-120 seconds | > 600s P1 |
| Paused | 0-3600 seconds | > 7200s P3 |
| Error | 0-10 seconds (auto-recovery) | > 60s P2 |
| Shutdown | 5-15 seconds | > 60s P2 |

**State Distribution Monitoring:**
- Large fraction in Error (> 5% of team): Team lead alerted
- Large fraction in Idle (> 40% of team): Workload distribution issue
- Agents cycling Processing -> Error -> Starting -> Processing: Auto-remediation failing

### 3.7 Confidence Monitoring

Each agent emits a confidence score with every task output (0.0 to 1.0).

**Confidence Metrics:**

| Metric | Type | Description |
|---|---|---|
| agent_confidence_score | Histogram | Distribution per agent (0.0 to 1.0) |
| agent_confidence_avg | Gauge | Rolling average over last 100 tasks |
| agent_confidence_variance | Gauge | Variance over last 100 tasks |
| agent_confidence_low_ratio | Gauge | % of tasks with confidence < 0.5 |

**Confidence Thresholds:**

| Level | Meaning | Action if Below Normal |
|---|---|---|
| 0.9 to 1.0 | Highly confident | Normal operations |
| 0.7 to 0.9 | Confident | Normal operations |
| 0.5 to 0.7 | Uncertain | Flag for human review, log for retraining |
| 0.3 to 0.5 | Low confidence | Route to senior agent, auto-flag output |
| 0.0 to 0.3 | Very low confidence | Reject output, escalate to human, retrain agent |

### 3.8 Drift Monitoring

**Types of Drift Monitored:**

| Drift Type | Detection Method | Alert Threshold |
|---|---|---|
| Output Distribution Drift | Jensen-Shannon divergence 7-day vs baseline | JS divergence > 0.10 |
| Input Data Drift | Kolmogorov-Smirnov test | p < 0.01 |
| Concept Drift | CUSUM (cumulative sum) of error rate | Crossing CUSUM threshold |
| Label Drift | Shift in output label distribution | > 10% change in any label |
| Model Performance Drift | Rolling accuracy vs held-out test set | Accuracy drop > 5% |

**Drift Detection Pipeline:**
1. Raw outputs aggregated per agent per day
2. Distribution builder computes 7-day rolling window
3. Baseline vs current comparison (JS divergence / KS test / CUSUM)
4. Within threshold: log no action
5. Breach threshold: P3 alert - auto-investigate (model version change, input data change, upstream behavior change)
6. If drift persists > 7 days: P2 alert, escalate, retrain agent, human review

**Drift Monitoring Schedule:**
- Daily drift check for all agents (computational cost: ~2 CPU-hours/day)
- Weekly drift report for department heads
- Monthly drift analysis for executive review

---


## 4. Infrastructure Monitoring

### 4.1 Compute Cluster Monitoring

The platform runs on a Kubernetes cluster of 250 nodes across 3 availability zones.

**Node-Level Metrics:**

| Metric | Source | Collection Interval | Alert Threshold |
|---|---|---|---|
| Node CPU utilization | node_exporter | 15s | > 85% average across cluster P3 |
| Node memory utilization | node_exporter | 15s | > 85% P2 > 95% P1 |
| Node disk utilization | node_exporter | 15s | > 80% P2 > 90% P1 |
| Node disk IOPS | node_exporter | 15s | > 80% of provisioned IOPS P2 |
| Node network throughput | node_exporter | 15s | > 80% of provisioned bandwidth P2 |
| Node CPU steal time | node_exporter | 15s | > 10% P2 (cloud contention) |
| Node OOM count | kube-state-metrics | 30s | Any OOM P1 |
| Node disk pressure | kube-state-metrics | 30s | True P2 |
| Node PID pressure | kube-state-metrics | 30s | True P2 |

**Pod-Level Metrics:**

| Metric | Source | Description | Alert Threshold |
|---|---|---|---|
| Pod restart count | kube-state-metrics | Restarts in last 24h | > 3 restarts P2 |
| Pod status (Pending) | kube-state-metrics | Pods stuck in Pending | Any P2 |
| Pod status (CrashLoopBackOff) | kube-state-metrics | Pods in crash loop | Any P1 |
| Pod status (ImagePullBackOff) | kube-state-metrics | Failing to pull image | Any P2 |
| Pod OOM killed count | kube-state-metrics | OOM kills per pod | > 1 P2 |
| Pod eviction count | kube-state-metrics | Evicted pods | > 1 P2 |

**Scaling Events:**

| Metric | Description | Alert |
|---|---|---|
| Scale-up events per hour | HPA scale-up count | > 50/hour P3 (thrashing) |
| Scale-down events per hour | HPA scale-down count | > 50/hour P3 |
| Max replica count reached | Hit max HPA replicas | P2 (capacity planning needed) |
| HPA unable to calculate | HPA missing metrics | P2 |

**Cluster Capacity:**

| Resource | Used | Available | % Used | Forecast (30 days) |
|---|---|---|---|---|
| vCPU | 1,850 | 3,200 | 57.8% | +12% |
| Memory | 6.2 TB | 12.8 TB | 48.4% | +15% |
| GPU (A100) | 120 | 240 | 50.0% | +25% |
| GPU (H100) | 48 | 64 | 75.0% | +20% ALERT |
| Disk (SSD) | 72 TB | 192 TB | 37.5% | +18% |
| Network bandwidth | 4.2 Gbps | 10 Gbps | 42.0% | +20% |

GPU (H100) alert: 75% used, projected 90% in 30 days. Provisioning request needed.

### 4.2 Storage Systems

#### 4.2.1 Object Storage (S3-compatible)

| Metric | Description | Threshold |
|---|---|---|
| Total bucket size | Aggregated storage across all buckets | Track for cost |
| 4xx error rate | Access denied, not found | > 1% P2 |
| 5xx error rate | Internal server errors | > 0.1% P1 |
| GET latency (P95) | Time to retrieve 1KB object | > 50ms P2 |
| PUT latency (P95) | Time to write 1KB object | > 100ms P2 |
| Throttling events | Requests throttled per minute | > 0 P2 |

#### 4.2.2 Block Storage (EBS)

| Metric | Description | Threshold |
|---|---|---|
| Volume queue length | I/O requests waiting | > 100 P2 |
| Read latency | Average read time | > 10ms P2 |
| Write latency | Average write time | > 20ms P2 |
| Burst balance | % of burst credits remaining | < 20% P2 |

#### 4.2.3 File Storage (EFS/FSx)

| Metric | Description | Threshold |
|---|---|---|
| Throughput | Read/write throughput | > 80% of provisioned P2 |
| IOPS | Operations per second | > 80% of provisioned P2 |
| Burst credit balance | Remaining burst credits | < 10% P2 |

### 4.3 Database Health

#### 4.3.1 PostgreSQL

| Metric | Description | Warning | Critical |
|---|---|---|---|
| Connection count | Active connections | > 80% of max | > 95% P1 |
| Connection wait time | Time waiting for connection | > 200ms avg | > 500ms P1 |
| Query latency (P95) | SELECT response time | > 200ms | > 1s P2 |
| Write latency (P95) | INSERT/UPDATE response time | > 100ms | > 500ms P1 |
| Replication lag | Standby behind primary | > 30s | > 60s P1 |
| Replication slot lag | WAL retention lag | > 1GB | > 5GB P1 |
| Deadlock count | Deadlocks per minute | > 0 | > 1 P0 |
| Transaction age | Longest running transaction | > 15min | > 30min P1 |
| Table bloat | Bloat ratio per table | > 20% | > 40% P2 |
| Cache hit ratio | Shared buffer hit rate | < 95% | < 90% P2 |
| Vacuum age | Time since last vacuum | > 2 days | > 7 days P2 |
| Table size growth | Growth rate per day | > 10%/day | Track for capacity |
| WAL generation rate | WAL data per minute | > 1GB/min | > 5GB/min P1 |

**Replication Status:**
- Primary: pg-primary-0 (us-east-1a) Healthy
- Standby-1: us-east-1b lag 2.3s Healthy
- Standby-2: us-east-1c lag 1.8s Healthy
- Standby-3: us-west-2a lag 47.8s WARNING (exceeds 30s threshold)
- HA Status: Primary healthy, automatic failover configured, quorum-based

### 4.4 Cache Health (Redis)

| Metric | Description | Warning | Critical |
|---|---|---|---|
| Hit rate | Read requests served from cache | < 85% | < 70% P2 |
| Memory usage | % of maxmemory | > 75% | > 90% P2 |
| Eviction rate | Keys evicted per minute | > 100/min | > 1000/min P2 |
| Connected clients | Active client connections | > 80% of max | > 95% P2 |
| Command latency (P99) | Slowest commands | > 100ms | > 500ms P2 |
| Replication lag | Replica lag | > 5s | > 30s P1 |
| CPU usage | Redis process CPU | > 80% | > 95% P2 |
| Memory fragmentation | RSS / used_memory | > 1.5 or < 1.0 | > 2.0 P2 |

### 4.5 Network Monitoring

| Metric | Description | Threshold |
|---|---|---|
| Bandwidth usage (per node) | % of provisioned bandwidth | > 80% P2 > 95% P1 |
| Packet loss rate | TCP packet loss | > 0.1% P2 > 1% P1 |
| Round-trip time (P99) | Inter-node latency | > 10ms same AZ > 50ms cross-AZ > 200ms cross-region |
| Connection count | Active TCP connections per node | > 95% of limit P2 |
| Retransmit rate | TCP retransmission rate | > 2% P2 > 5% P1 |
| DNS resolution time | Time to resolve internal DNS | > 100ms P2 |
| TLS handshake time | Time to complete TLS | > 500ms P2 |
| Error rate | Network errors per hour | > 0 P2 |

### 4.6 Message Bus (Kafka)

| Metric | Description | Threshold |
|---|---|---|
| Consumer group lag | Lag per consumer per partition | > 10,000 P2 > 100,000 P1 |
| Message rate | Messages per second per topic | Trending |
| Dead letter queue size | Messages in DLQ | > 0 P3 |
| Partition count | Partitions per topic | Tracked |
| Leader election rate | Leader elections per minute | > 1 P2 |
| Request handler avg time | Handler response time | > 50ms P2 |
| Network throughput | Bytes in/out per broker | > 80% of limit P2 |
| Disk usage | Per broker disk | > 75% P2 > 90% P1 |

### 4.7 API Gateways

| Metric | Description | Threshold |
|---|---|---|
| Request rate | Requests per second | > 2x normal P2 |
| 4xx error rate | Client errors | > 5% P2 > 10% P1 |
| 5xx error rate | Server errors | > 1% P2 > 2% P1 |
| Latency (P50) | Median response time | > 200ms P2 > 500ms P1 |
| Latency (P95) | 95th percentile | > 1s P1 > 2s P0 |
| Latency (P99) | 99th percentile | > 3s P0 |
| Rate limiting count | Requests rate-limited | > 0 P3 |
| Upstream failure rate | Failed upstream connection | > 1% P2 |
| Connection pool usage | Pool for upstream connections | > 80% P2 |

---


## 5. Workflow Monitoring

### 5.1 Pipeline Status Monitoring

Every business workflow is a pipeline of agent-to-agent interactions. Each pipeline instance is tracked through its lifecycle.

**Pipeline Status Matrix:**

| Pipeline Type | Active | Paused | Failed | Completed | Total | SLA% |
|---|---|---|---|---|---|---|
| Book to KG | 42 | 2 | 1 | 1,247 | 1,292 | 96.7% |
| KG to Story | 178 | 5 | 4 | 4,892 | 5,079 | 91.2% |
| KG to Quiz | 67 | 1 | 2 | 4,234 | 4,304 | 98.3% |
| KG to Lesson Plan | 34 | 0 | 0 | 1,234 | 1,268 | 100% |
| Story to Translation | 128 | 3 | 5 | 2,845 | 2,981 | 87.4% |
| Story to Audio Generation | 56 | 1 | 2 | 1,894 | 1,953 | 92.1% |
| Content to Review | 89 | 223 | 12 | 3,456 | 3,780 | 99.2% |
| Content to Publish | 5 | 1 | 0 | 892 | 898 | 100% |
| User to AI Teacher | 2,845 | 12 | 4 | 98,456 | 101,317 | 99.6% |

**Total:** 2,663 Active / 231 Paused / 30 Failed / 119,128 Completed / 122,052 Total

**Pipeline Status Definitions:**

| Status | Definition | Duration Target | Escalation |
|---|---|---|---|
| Active | Pipeline running normally | Varies by pipeline | Track latency |
| Paused | Pipeline halted (awaiting human approval) | < 24h | > 24h P3 |
| Failed | Unrecoverable error | N/A | Immediate P1 |
| Completed | Successfully completed | Within SLA | Done |
| Queued | Waiting for resources | < 5min | > 30min P2 |
| Retrying | Auto retry loop | < 5 retries | > 10 retries P2 |

### 5.2 Pipeline Metrics

| Metric | Type | Description |
|---|---|---|
| pipeline_items_per_hour | Gauge | Items processed per hour per pipeline |
| pipeline_duration_seconds | Histogram | P50/P95/P99 completion time per pipeline |
| pipeline_completion_time_seconds | Gauge | Raw completion time per instance |
| pipeline_avg_stage_duration | Gauge | Average time per stage |
| pipeline_queue_depth | Gauge | Items waiting to enter pipeline |
| pipeline_sla_breach_count_total | Counter | SLA breaches |
| pipeline_failure_count_total | Counter | Pipeline failures |
| pipeline_retry_count_total | Counter | Individual task retries |

### 5.3 Bottleneck Detection

**Bottleneck Algorithm:**
For each pipeline stage:
1. Compute stage_duration (time spent in stage per item)
2. Compute stage_queue (items waiting at this stage)
3. Compute stage_utilization (active workers / total workers)
4. If stage_queue > 5 AND stage_utilization > 85% AND stage_duration > avg * 2:
   Flag as BOTTLENECK

**Bottleneck Example:**

Pipeline: Knowledge Graph to Story

| Stage | Duration | Queue | Workers | Status |
|---|---|---|---|---|
| Topic Extraction Agent | 2.1s | 3 | 6/8 | OK |
| Story Outline Agent | 4.7s | 12 | 8/8 | BOTTLENECK |
| Story Drafting Agent | 42.3s | 7 | 12/12 | BOTTLENECK |
| Story Verification Agent | 8.2s | 2 | 4/4 | OK |
| Style Adjustment Agent | 3.4s | 1 | 4/4 | OK |
| Theological Review Agent | 12.1s | 0 | 2/2 | OK |

Action: Scale Story Outline Agent (8 to 12 replicas), Story Drafting Agent (12 to 16 replicas)

### 5.4 SLA Tracking per Pipeline

| Pipeline | SLA Target | P95 Actual | % Within SLA | Burn Rate |
|---|---|---|---|---|
| Book to KG | < 120s | 98s | 99.2% | 0.8% |
| KG to Story | < 180s | 142s | 97.1% | 2.9% |
| KG to Quiz | < 60s | 45s | 99.8% | 0.2% |
| KG to Lesson Plan | < 90s | 72s | 100% | 0% |
| Story to Translation | < 300s | 420s | 87.4% | 12.6% ALERT |
| Story to Audio | < 180s | 165s | 92.1% | 7.9% |
| Content to Publish | < 30s | 22s | 100% | 0% |
| User to AI Teacher | < 3s | 1.8s | 99.2% | 0.8% |

**SLA Burn Rate Monitoring:**
- Burn rate = 100% - (% within SLA)
- Burn rate > 10% for 7 days: P2 alert - pipeline consistently missing SLA
- Burn rate > 20% for 1 day: P1 alert - pipeline in critical state
- Each department has error budget of 5% burn rate per month

### 5.5 Pipeline Failure Analysis

```
FAILURE ANALYSIS - Last 7 Days

Pipeline: KG to Storybook (4 failures)
  - Stage: Story Drafting Agent failed (3 failures)
    -- LLM timeout (2): story-drafter-l3 and story-drafter-l2 timed out
    -- Output validation (1): duplicate content from duplicate KG node
  - Stage: Theological Review Agent failed (2 failures)
    -- Model query error (2): HTTP 500 from model server
    -- Resolution: Increased model server replicas from 2 to 4

Pipeline: Story to Translation (5 failures)
  - Stage: Translation Agent failed (4 failures)
    -- Language detection failure (3): detected language mismatch
    -- Model output too long (1): exceeded max token limit
  - Stage: Translation Verification failed (1 failure)
    -- Expert review flagged critical translation error
```

### 5.6 Retry Rate Monitoring

| Pipeline | Retry Rate | Tasks Retried | Common Retry Reason |
|---|---|---|---|
| Book to KG | 2.1% | 1,247 | LLM timeout, KG node collision |
| KG to Story | 4.8% | 4,892 | LLM timeout, output validation failure |
| KG to Quiz | 1.2% | 4,234 | Question generation too similar |
| Story to Translation | 8.7% | 2,845 | Translation model rate limit |
| Story to Audio | 3.4% | 1,892 | TTS service unavailable |
| Content to Publish | 0.2% | 892 | CMS API timeout |

**Escalation for Retry Rate:**
- Retry rate > 5%: P3 alert to team lead
- Retry rate > 10%: P2 alert, immediate investigation
- Retry rate > 25%: P1 alert, auto-disable pipeline

---


## 6. Quality Monitoring

### 6.1 Content Quality Scores

Every piece of content receives a quality score upon creation generated by verification agents and validated by human reviewers on a sample basis.

**Quality Scoring Rubric:**

| Dimension | Weight | Scoring Method | Score Range |
|---|---|---|---|
| Factual Accuracy | 0.30 | Cross-reference with verified KG, reference texts | 0-100 |
| Grammar & Language | 0.15 | NLP-based grammar check + human validation | 0-100 |
| Stylistic Quality | 0.10 | Style guide adherence (age-appropriate, narrative flow) | 0-100 |
| Theological Accuracy | 0.25 | Senior theological review agent + human scholar | 0-100 |
| Pedagogical Value | 0.15 | Educational effectiveness score (engagement prediction) | 0-100 |
| Formatting & Structure | 0.05 | Template adherence, markdown quality | 0-100 |

Composite Score = weighted average of all dimensions.

**Quality Score Trends:**

| Content Type | Current | 7-day Avg | 30-day Avg | Change | Goal |
|---|---|---|---|---|---|
| Stories | 89.4 | 87.2 | 86.5 | +2.9 | >=85 |
| Quizzes | 91.2 | 90.1 | 89.8 | +1.4 | >=90 |
| KG Entries | 94.7 | 94.2 | 93.8 | +0.9 | >=93 |
| Lesson Plans | 88.9 | 88.1 | 87.4 | +1.5 | >=85 |
| Translations | 83.6 | 82.4 | 83.1 | +0.5 | >=80 |
| Audio Narration | 87.3 | 86.8 | 86.0 | +1.3 | >=85 |
| Islamic Rulings | 95.2 | 95.0 | 94.6 | +0.6 | >=95 |

**Quality Alert Thresholds:**

| Threshold | Severity | Action |
|---|---|---|
| Any content type drops > 5 points in 7 days | P3 | Investigate, notify content team lead |
| Any content type drops > 10 points in 7 days | P2 | Immediate investigation, pause generation for that type |
| Any content type drops below goal | P2 | Review pipeline, agent, and model configuration |
| More than 5% of content below quality threshold (< 70) | P2 | Increase human review sampling rate |
| More than 10% of content below quality threshold (< 70) | P1 | Stop content generation pipeline, force human review for all |

### 6.2 User Satisfaction Scores

| Metric | Description | Source | Target |
|---|---|---|---|
| NPS (Net Promoter Score) | User satisfaction survey (monthly) | In-app survey | > 50 |
| Lesson Rating | Average rating of lessons (1-5 stars) | In-lesson rating | > 4.2 |
| Teacher Rating | Average rating of AI teacher | Post-session rating | > 4.0 |
| Content Feedback | Thumbs up/down per piece of content | In-content feedback | > 85% positive |
| Support Satisfaction | Post-support survey score | Support ticket closure | > 90% satisfied |
| Course Completion Rate | % of users completing a course | Analytics | > 60% |

### 6.3 Error Distribution by Category

| Error Category | Description | Rate (per 1000 items) | Trend | Alert |
|---|---|---|---|---|
| Factual Error | Incorrect information, date, name, location | 2.4 | -12% | P2 if > 10 |
| Grammatical Error | Spelling, grammar, punctuation mistakes | 8.1 | -8% | P2 if > 25 |
| Stylistic Error | Improper tone, overly complex language | 4.7 | +5% | P3 if > 15 |
| Theological Error | Incorrect Islamic ruling, misattributed hadith | 0.8 | -20% | P0 if > 2 |
| Formatting Error | Broken markdown, missing sections | 3.2 | -15% | P3 if > 10 |

**Error Reduction Targets:**
- Reduce theological errors to 0 (zero tolerance) by Q4 2026
- Reduce factual errors by 50% by Q4 2026 (target: 1.2 per 1000)
- Reduce grammatical errors by 30% by Q4 2026 (target: 5.6 per 1000)

### 6.4 Accuracy Trends per Model

| Model | Avg Accuracy | Tasks | P50 Latency | P95 Latency |
|---|---|---|---|---|
| gpt-4-omni | 97.2% | 12,834 | 2.1s | 4.7s |
| claude-3.5-opus | 96.8% | 8,231 | 2.4s | 5.8s |
| mistral-3.0-large | 93.4% | 4,872 | 1.2s | 2.9s |
| llama-3.3-70b | 92.1% | 7,456 | 1.8s | 4.1s |
| custom-islamic-qa-v2 | 95.6% | 2,145 | 0.8s | 1.9s |
| quran-tafsir-expert | 98.7% | 1,234 | 3.2s | 7.4s |
| hadith-authenticator | 94.2% | 892 | 4.1s | 9.8s |

Note: llama-3.3-70b shows -1.0% accuracy in 7 days. Potentially drifted. Investigate.

### 6.5 False Positive/Negative Rates for Verification Agents

| Verification Agent | False Positive Rate | False Negative Rate | Precision | Recall | F1 Score |
|---|---|---|---|---|---|
| Factual Accuracy Verifier | 3.2% | 2.1% | 96.8% | 97.9% | 97.3% |
| Grammar Verifier | 4.1% | 5.8% | 95.9% | 94.2% | 95.0% |
| Theological Accuracy Verifier | 0.8% | 0.5% | 99.2% | 99.5% | 99.3% |
| Stylistic Verifier | 5.5% | 4.2% | 94.5% | 95.8% | 95.1% |
| Translation Verifier | 3.8% | 2.9% | 96.2% | 97.1% | 96.6% |
| Audio Quality Verifier | 2.0% | 1.5% | 98.0% | 98.5% | 98.2% |

**Verifier Quality Alerts:**
- F1 < 95%: P3 alert, schedule retraining
- F1 < 90%: P2 alert, immediate retraining, pause dependent pipelines
- False positive rate > 10%: P2 alert (verifier blocking good content)
- False negative rate > 10%: P1 alert (verifier missing real errors)

### 6.6 A/B Test Monitoring

| Test | Feature | Variant A | Variant B | Duration | Sample | Significance |
|---|---|---|---|---|---|---|
| AB-042 | AI Teacher greeting style | Formal | Friendly | 7 days | 4,200 users | p = 0.03 |
| AB-048 | Quiz difficulty algorithm | Fixed | Adaptive | 14 days | 8,900 users | p = 0.11 |
| AB-051 | Audio narration speed | 1.0x default | 1.25x | 5 days | 2,100 users | p = 0.01 |

**Guardrail Metrics During A/B Tests:**
- Overall user satisfaction (must not drop > 2%)
- Task completion rate (must not drop > 5%)
- Error rate (must not increase > 10%)
- Latency P95 (must not increase > 100ms)
- Any guardrail breach: auto-stop A/B test, roll all users to safe variant

### 6.7 Drift Detection (Content Level)

| Metric | Description | Detection Method | Threshold |
|---|---|---|---|
| Content Length Drift | Average word count | Moving average vs baseline | +/- 15% |
| Readability Drift | Flesch-Kincaid grade level | Moving average vs baseline | +/- 2 grade levels |
| Sentiment Drift | Overall sentiment | Distribution shift (JS divergence) | > 0.15 |
| Topic Drift | Topic distribution | Word2Vec topic distribution | JS divergence > 0.10 |
| Vocabulary Drift | Type-token ratio | TTR shift | +/- 10% |
| Instruction Following | Non-compliance rate | Human-reviewed sample (5%) | Non-compliance > 3% |

**Drift Example:**
Topic distribution shift detected: Stories about Prophets dropped from 35% to 30%; Stories about Companions rose from 22% to 28%. Root cause: New content planner agent version v2.4.0 optimizes for engagement score which favors Companion stories. Action: Adjust content planner weighting to balance output distribution.

---


## 7. Business Monitoring

### 7.1 User Metrics

| Metric | Value | vs Yesterday | vs Last Week | Target |
|---|---|---|---|---|
| Daily Active Users (DAU) | 142,857 | +3.2% | +12.1% | 150,000 |
| Weekly Active Users (WAU) | 612,340 | — | +8.4% | 650,000 |
| Monthly Active Users (MAU) | 1,892,450 | — | +5.7% | 2,000,000 |
| New Signups (24h) | 4,832 | +2.1% | +8.9% | 5,000/day |
| Total Registered Users | 3,847,290 | +0.2% | +1.1% | N/A |
| DAU/MAU Ratio | 10.5% | -0.3% | -0.1% | > 15% |
| Stickiness (7-day) | 42.3% | +0.5% | +1.2% | > 40% |

**Retention Cohorts (Weekly):**

| Cohort | Week 0 | Week 1 | Week 2 | Week 4 | Week 8 | Week 12 |
|---|---|---|---|---|---|---|
| 2026-06-21 | 100% | 58.2% | 45.1% | 38.4% | 32.1% | — |
| 2026-06-14 | 100% | 60.1% | 47.3% | 40.2% | 33.8% | 28.2% |
| 2026-06-07 | 100% | 57.8% | 44.9% | 38.1% | 31.5% | 26.8% |
| 2026-05-31 | 100% | 59.5% | 46.2% | 39.7% | — | — |

Average Retention: W1: 59.4% W2: 46.3% W4: 39.4% W8: 32.5% W12: 27.5%
Goal: W1 > 60% W2 > 48% W4 > 42% W8 > 35% W12 > 30%

### 7.2 Engagement Metrics

| Metric | Value | Target | Status |
|---|---|---|---|
| Avg Session Duration | 24.7 min | > 20 min | Above target |
| Lessons Completed (24h) | 84,234 | 80,000 | Above target |
| Lessons Completed per User | 1.7 / day | > 1.5 / day | Above target |
| Quizzes Taken (24h) | 32,847 | 30,000 | Above target |
| AI Teacher Sessions (24h) | 12,347 | 12,000 | Above target |
| Content Items Bookmarked | 4,578 | 4,000 | Above target |
| Feature Adoption Rate | 68.3% | > 65% | Above target |
| Course Enrollment Rate | 38.2% | > 35% | Above target |

**Feature Adoption Rates:**

| Feature | Adoption % | Change vs Last Month |
|---|---|---|
| Content Browsing | 92.3% | +0.5% |
| Content Reading | 78.1% | +2.1% |
| Quiz Taking | 62.4% | +3.4% |
| AI Teacher | 58.2% | +4.7% |
| Bookmarking | 45.1% | +1.2% |
| Progress Tracking | 38.7% | +2.8% |
| Study Groups | 22.3% | +5.8% Fastest growing |
| Personalized Paths | 18.4% | +2.1% |
| Social Sharing | 12.1% | -0.8% Declining |
| Offline Download | 8.7% | +1.2% |

### 7.3 Revenue Metrics

| Metric | Value | MoM Change | Target | Status |
|---|---|---|---|---|
| MRR (Monthly Recurring Revenue) | $847,350 | +8.2% | $900,000 | On track |
| ARPU (Average Revenue Per User) | $14.20 | +2.1% | $15.00 | Improving |
| Free to Paid Conversion Rate | 8.7% | +0.3% | 10% | Improving |
| Monthly Churn Rate | 5.8% | -0.4% | < 5% | Improving |
| Paid User Count | 59,700 | +6.2% | 65,000 | On track |
| Days to Payback (CAC) | 45 days | — | < 60 days | Below target |
| Net Dollar Retention | 108% | +3% | > 105% | Above target |
| New Paid Subscriptions (24h) | 492 | +8.6% | 500 | Near target |

**Revenue Alerting:**
- MRR drops > 5% in a week: P2 alert to finance team
- Churn rate increases > 2% MoM: P2 alert, investigate churn reasons
- Conversion rate drops > 20% in a day: P1 alert (potential issue with subscription flow)
- Any payment processing failure rate > 1%: P1 alert to engineering

### 7.4 Content Metrics

| Metric | Value | Target | Status |
|---|---|---|---|
| Total Published Content | 847,234 | 1,000,000 | On track |
| Content Created per Day | 4,234 | 4,000 | Above target |
| Content in Review | 32,847 | < 50,000 | Within limits |
| Content Pending Human Review | 12,847 | < 10,000 | Over target |
| Content Published per Day | 3,845 | 3,500 | Above target |
| Content Archived Rate | 2.1% | < 5% | Low |
| Content Quality Score (Avg) | 87.2 | > 85 | Above target |
| Content Rejection Rate | 3.8% | < 5% | Within limits |

### 7.5 Platform Health

| Metric | Current | Target | Status |
|---|---|---|---|
| Uptime (30-day rolling) | 99.98% | 99.99% | Below target (5.8 min downtime) |
| Uptime (365-day rolling) | 99.992% | 99.99% | Exceeds target |
| Error Budget Consumed (MTD) | 4.8 min | 4.3 min | 1.5 min over (0.5 min remaining) |
| API Availability | 99.995% | 99.99% | Above target |
| API Latency P95 | 247ms | < 500ms | Below target |
| AI Teacher Availability | 99.97% | 99.99% | Below target |
| AI Teacher Latency P95 | 1.8s | < 2s | Below target |

**Error Budget Detail:**
SLO: 99.99% uptime = 4.32 minutes downtime allowed per month
- Jul 01-04: 0 min consumed
- Jul 05: 1m 42s (DB failover) - 1.70 min consumed
- Jul 07: 3m 12s (API gateway) - 3.10 min consumed
- Remaining: 0.48 min (28.8 seconds)
- Burn rate: 0.54 min/day = 3.7x SLO
- BURN RATE ALERT: Freeze all deployments until burn rate decreases

### 7.6 Growth Metrics

| Metric | Value | Trend | Target |
|---|---|---|---|
| Viral Coefficient (K-factor) | 0.82 | +0.05 MoM | > 1.0 |
| Invites Sent per User | 2.4 | +0.3 MoM | > 3.0 |
| Invite Conversion Rate | 22.3% | +0.5% MoM | > 25% |
| Organic Acquisition % | 48.2% | +2.1% MoM | > 50% |
| Paid Acquisition % | 51.8% | -2.1% MoM | < 50% |
| Cost Per Acquisition (CPA) | $8.40 | -$0.50 MoM | < $10 |
| Customer Acquisition Cost | $12.10 | -$0.80 MoM | < $15 |

**Growth Alerting:**
- K-factor drops below 0.5: P2 alert (viral loop broken)
- CPA increases > 20% in a week: P2 alert (marketing efficiency declining)
- Organic acquisition drops > 10%: P2 alert (SEO issues, brand awareness decline)
- Paid acquisition > 70% of total: P2 alert (over-dependence on paid channels)

---


## 8. Security Monitoring

### 8.1 Intrusion Detection

| Metric | Description | Source | Alert Threshold |
|---|---|---|---|
| Unauthorized access attempts | 401/403 on restricted endpoints | API Gateway logs, WAF | > 100/min P1 |
| Brute force attempts | Multiple logins from same IP | Auth service logs | > 10/min per IP P1 |
| SQL injection attempts | Attack signature patterns | WAF logs, ES query logs | > 0 P1 |
| XSS attempts | Cross-site scripting signatures | WAF logs | > 0 P1 |
| Path traversal attempts | ../ in URL patterns | WAF logs, API GW logs | > 0 P1 |
| Port scanning probes | Multiple connection attempts on closed ports | Network logs, HIDS | Any P2 |
| Zero-day exploit attempts | Behavior-based anomaly detection | Behavioral analytic system | > 0 P0 |

### 8.2 Data Exfiltration Monitoring

| Metric | Description | Source | Alert Threshold |
|---|---|---|---|
| Large outbound transfers | Single response > 100MB | API gateway, network monitoring | > 100MB P1 |
| Anomalous download patterns | User downloads > 100 items in 5 min | Analytics, API logs | > 100/5min P1 |
| Sensitive data pattern | SSN, credit cards in response | DLP system | Any P0 |
| Unusual destination IPs | Data sent to unknown external IPs | Network monitoring | Any P2 |
| Egress traffic spike | Total egress > 200% of normal | Network monitoring | > 200% in 1hr P1 |
| API data scraping | Same endpoint repeated calls | API GW logs, rate limiter | > 1000 calls/user/min P2 |

### 8.3 Agent Behavior Anomaly Detection

Each agent's behavior is modeled over time. Deviations from the baseline can indicate compromise or misconfiguration.

| Agent Behavior Metric | Baseline | Anomaly Threshold | Alert |
|---|---|---|---|
| Average response length (tokens) | 1,200 +/- 200 | > 2,000 or < 500 | P3 |
| Average confidence score | 0.91 +/- 0.04 | < 0.75 | P3 |
| API call rate (LLM calls/min) | 12 +/- 3 | > 20 or < 5 | P2 |
| Network egress (bytes/day) | 2.1 GB +/- 0.5 GB | > 5 GB | P2 |
| Database query rate (queries/min) | 4.5 +/- 1.0 | > 15 | P2 |
| External host connections (unique/day) | 3 +/- 2 | > 10 | P3 |
| Average processing time per task | 4.2s +/- 1.0s | > 10s or < 2s | P2 |

**Anomaly Example:**
Agent content-generator-7823:
- Anomaly Score: 92.4/100 (threshold: 75)
- API call rate: 47/min (baseline: 12/min, +292%)
- Network egress: 800MB in 5min (baseline: 0.5MB/5min)
- Response length: 0 tokens (baseline: 1,200 tokens)
- Risk Level: HIGH - Agent appears to be exfiltrating or compromised
- Auto-Action: Agent disabled, network access revoked, API keys rotated, quarantine container, incident opened

**Agent Isolation Protocol:**

| Trigger | Isolation Level | Action |
|---|---|---|
| Single anomaly (score > 75) | Soft isolation | Internal API calls only |
| Two+ anomalies (score > 80) | Medium isolation | Network restricted, no external calls |
| Critical anomaly (score > 90) | Hard isolation | Agent quarantined, API keys rotated, process killed |
| Confirmed compromise | Total isolation | Container destroyed, credentials revoked, forensic image taken |

### 8.4 Permission Violations

| Metric | Description | Alert Threshold |
|---|---|---|
| Access denied events | User/agent accessing unauthorized resource | > 50/hour P2 |
| Escalation attempts | Attempts to elevate privileges without authorization | > 0 P1 |
| Cross-tenant access | User accessing other user's data | > 0 P1 |
| Permission escalation success | Unauthorized privilege escalation | > 0 P0 |
| Token reuse | JWT used from different IP/location | > 0 P2 |
| Token expiry bypass | Attempting to use expired token | > 10/hour P2 |
| Admin action audit | All admin-level actions | No threshold, always logged |

**Audit Trail Example (24 hours):**

| Timestamp | Agent/User | Action | Status |
|---|---|---|---|
| 09:42:17 | ce-ai | Granted admin access to department-17 dataset | ALLOW |
| 11:23:04 | agent-story-0427 | Attempted access to user-pii-database | DENY |
| 13:01:55 | user-8472332 | Attempted role escalation (student to teacher) | DENY |
| 14:12:43 | agt-verify-3891 | Attempted access to deployment-prod namespace | DENY |
| 17:34:22 | agent-report-gen | Accessed 42,000 user records (anomalous) | FLAGGED |
| 19:02:11 | unknown-agent | Created pod in kube-system | DENY |
| 23:58:04 | external-IP | Brute force attempt (847 requests in 2 min) | BLOCKED |

### 8.5 Rate Limiting Violations

| Metric | Description | Count | Alert |
|---|---|---|---|
| API rate limit hits (per user) | User exceeding API call limits | 8,234/day | P3 if > 100/user/day |
| API rate limit hits (per IP) | IP exceeding API call limits | 234/day | P2 if > 500/IP/day |
| Agent rate limit hits | Agent exceeding LLM call rate | 12/day | P2 if > 10/day |
| Login rate limit hits | Too many logins from same IP | 847/day | P1 if > 100/IP/day |
| Request throttling events | Gateway throttling | 42/day | P3 if > 1000/day |

### 8.6 Authentication Failures

| Metric | Description | Count | Alert |
|---|---|---|---|
| Failed logins (total) | All failed login attempts | 12,847/day | P2 if > 20,000/day |
| Successful logins | Valid login completions | 84,234/day | P2 if success rate < 80% |
| MFA failures | Failed MFA challenge attempts | 234/day | P2 if > 1,000/day |
| Token refresh failures | Failed JWT refresh | 42/day | P3 if > 500/day |
| Session creation failures | New session failure | 12/day | P2 if > 100/day |

### 8.7 Security Dashboard

```
SECURITY DASHBOARD
Current Threat Level: YELLOW (ELEVATED)

ACTIVE INCIDENTS:
  ID      Severity  Type          Agent           Duration  Status
  SEC-72  CRITICAL  Exfiltration  content-gen-42  12 min    Quarantined
  SEC-73  HIGH      Brute force   n/a             2 min     Mitigated
  SEC-74  MEDIUM    Anomaly       verifier-891    34 min    Resolved

RESOLVED INCIDENTS (Today):
  ID      Severity  Type          Duration  Resolution Time
  SEC-70  MEDIUM    Auth failure  18 min    4 min
  SEC-71  LOW       Rate limit    2 min     Auto-resolved

KEY METRICS:
  Unauthorized access attempts (24h): 3,847
  Blocked IPs (24h): 847
  Agent anomalies (24h): 4
  Permission violations (24h): 42
  DLP alerts (24h): 2
  Auth failures (24h): 12,847

MTTR: 8.3 min (target: < 15 min)
```

---


## 9. Alerting System

### 9.1 Alert Severity Classification

| Severity | Code | Response SLA | Description | Example |
|---|---|---|---|---|
| P0 Critical | sev-critical | 1min acknowledge, continuous work | System-wide outage, data loss, security breach, revenue impact | Database down, security breach, payment failure |
| P1 High | sev-high | 5min acknowledge, continuous work | Major feature unavailable, significant error spike, SLO breach risk | Content pipeline down, AI Teacher unavailable, error rate > 10% |
| P2 Medium | sev-medium | 15min respond, resolve within 4h | Partial degradation, warning trend, approaching P1 threshold | Memory > 90%, latency P95 above threshold |
| P3 Low | sev-low | 1h respond, resolve within 24h | Non-blocking issue, minor anomaly, informational | Drift detected, single agent misconfiguration |
| P4 Info | sev-info | 24h (next business day) | Informational, tracked for post-mortem | A/B test completed, model accuracy trend change |

### 9.2 Alert Channels Matrix

|  | SMS | Phone | Slack-Ops | Slack-Team | Email | Dashboard | Ticket |
|---|---|---|---|---|---|---|---|
| P0 | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| P1 | — | — | Yes | Yes | Yes | Yes | Yes |
| P2 | — | — | — | Yes | Yes | Yes | — |
| P3 | — | — | — | Optional | Yes | Yes | — |
| P4 | — | — | — | — | Yes | Yes | — |

- SMS Provider: Twilio (2-way SMS for acknowledgment)
- Phone Provider: Twilio Voice (TTS + DTMF acknowledgment: Press 1 to ack, 2 to escalate)
- Slack Ops Channel: #ops-alerts (P0-P1 only, 24/7)
- Slack Team Channels: #team-{name}-alerts (P2-P3)
- Email: alerting@ai-company.internal
- Ticket: ServiceNow ITSM (auto-creates for P0-P2)

### 9.3 Alert Fatigue Prevention

#### 9.3.1 Deduplication

Alertmanager groups similar alerts:
- Group by agent_id + alertname: Multiple HighMemoryUsage from same agent grouped into one notification
- Group by severity + service: All P1 alerts from content-generation service summarized
- Group by team + alertname: All P3 alerts for team-017 sent as daily digest

#### 9.3.2 Suppression

Suppression rules during known conditions:
1. prometheus_target_down AND known_maintenance_window: suppress for duration
2. agent_restart detected AND expected_recovery < 30s: suppress P0-P2 for 60s
3. error_rate_spike AND autoscaler_active AND scaling_in_progress: suppress P2-P3 for 5min
4. database_failover AND failover_expected AND estimated_time < 120s: suppress P0 for 120s
5. bulk_deployment AND deployment_id matches known_rollout: suppress related alerts for 10min

#### 9.3.3 Silencing

Manual silencing for known issues:
- Creator: on-call SRE
- Max duration: 24h (4h needs SRE lead approval)
- All silences logged to Audit DB with reason
- Common reasons: known issue under investigation, config change in progress, non-production issue, false positive, third-party degradation

#### 9.3.4 Alert Grouping Parameters

| Parameter | P0 | P1 | P2 | P3 | P4 |
|---|---|---|---|---|---|
| Group Wait | 10s | 20s | 30s | 60s | 5min |
| Group Interval | 1min | 2min | 5min | 15min | 1hr |
| Repeat Interval | 5min | 15min | 30min | 1hr | 4hr |

### 9.4 Auto-Remediation Rules

| Alert Condition | Action | Success Rate | Escalation if Failed |
|---|---|---|---|
| Agent memory > 85% for 5 min | Restart agent gracefully | 78% | P2 - investigate memory leak |
| Agent not responding (3 missed heartbeats) | Force restart | 92% | P1 - suspected deeper issue |
| Agent error rate > 10% for 2 min | Throttle input queue by 50% | 85% | P1 - diagnose error pattern |
| Queue depth > 10,000 | Scale consumer group 2x | 95% | P2 - check if scaling enough |
| DB connection pool > 80% | Increase pool by 20% | 90% | P1 - if max pool reached |
| Disk usage > 85% | Run log rotation and GC | 88% | P1 - if > 90%, expand volume |
| GPU temp > 82C | Reduce batch size 50% | 76% | P1 - if > 85C, terminate job |
| API latency P95 > threshold | Reroute traffic from slow nodes | 92% | P1 - if all nodes affected |
| Rate limiting triggered | Increase rate limit temporarily | 85% | P2 - if persistent, investigate |

**Auto-Remediation Audit:**
- All actions logged to audit trail
- If same remediation runs > 3 times in 24h: P2 alert (action ineffective)
- Validation step required after each auto-remediation
- Any remediation that worsens the problem: P0 alert, human override

### 9.5 Escalation Paths

**P0 Escalation:**
```
Level 0: On-call SRE (primary) — SLA: 1min to acknowledge
Level 1: On-call SRE (secondary/backup) — if no ack in 1min, SLA: 2min
Level 2: SRE Team Lead — if no ack in 2min, SLA: 3min
Level 3: Head of Infrastructure — if no ack in 3min, SLA: 5min
Level 4: CTO — automatic informational
```

**P1 Escalation:**
```
Level 0: On-call SRE (primary) — SLA: 5min to acknowledge
Level 1: On-call SRE (secondary) — if no ack in 5min, SLA: 5min
Level 2: SRE Team Lead — if no ack in 5min, SLA: 10min
Level 3: Head of Infrastructure — informational only
```

**P2 Escalation:**
```
Level 0: On-call team member — SLA: 15min to acknowledge
Level 1: Team Lead — if no ack in 15min, SLA: 15min
Level 2: Department Head — informational only
```

**P3 Escalation:**
```
Level 0: Team member (daytime hours only) — SLA: 1hr to acknowledge
Level 1: Team Lead (next business day) — if no ack in 1hr
```

### 9.6 On-Call Schedules

Follow-the-sun schedule covering 3 regions:

| Region | Time (UTC) | Primary SRE | Backup SRE |
|---|---|---|---|
| APAC | 00:00 - 08:00 UTC | sre-1 (SG) | sre-2 (SG) |
| EMEA | 08:00 - 16:00 UTC | sre-3 (UK) | sre-4 (DE) |
| US | 16:00 - 24:00 UTC | sre-5 (US-EAST) | sre-6 (US-WEST) |

Handoff Overlaps: 30 minutes each transition
Rotation: Weekly (Mon 00:00 UTC)
Backup Rotation: Monthly

**On-call Responsibilities:**
- Acknowledge P0/P1 alerts within SLA
- Coordinate incident response
- Update status page (status.ai-company.com)
- Document incident in post-mortem
- Escalate if needed

**On-call Compensation:**
- $200 on-call stipend per week
- Time-and-a-half for incident work > 1 hour
- Comp time: next business day off if paged 22:00-06:00 local

### 9.7 Maintenance Windows

| Type | Max Duration | Approval Needed | Suppression Scope |
|---|---|---|---|
| Emergency fix | 30 min | SRE lead | P2-P4 only |
| Standard deploy | 2 hours | Team lead | P2-P4 only |
| Infrastructure upgrade | 4 hours | Head of Infrastructure | P0-P4 (all) |
| Database migration | 8 hours | Head of Infrastructure + CTO | P0-P4 (all) |

**Maintenance Window Process:**
1. Submit via internal portal (minimum 4h in advance)
2. Include: affected services, start/end time, description, risk assessment
3. Auto-creates silence rules in Alertmanager
4. Post-maintenance: auto-remove silence rules, send summary
5. If maintenance overruns: approval needed for extension
6. If maintenance causes impact: incident review within 24h

---


## 10. Dashboards

### 10.1 Executive Dashboard

**Purpose:** CEO, CE-AI, and executives view of entire business and platform health.

**Refresh Rate:** 30 seconds
**Audience:** CE-AI, 7 Executives, Board of Directors
**Data Sources:** Prometheus, PostgreSQL, Elasticsearch, Event Store

**Layout:**
- Company Health Score: 91.7% (+0.3% yesterday, +2.1% this month)
- Status: All Systems Operational (Green: 32/33 depts, Yellow: Content Quality)
- KPI Cards: DAU 142,857 / MAU 1,847,450 / MRR $847,450 / Uptime 99.98%
- Weekly Trends: DAU, Revenue, Quality, Uptime (30-day sparklines)
- Active Incidents: P0: 1 / P1: 0 / P2: 3 / P3: 8
- Error Budget: 78% consumed, 0.48 min remaining
- Agents by Status: Healthy 8,420 / Degraded 42 / Down 12 / Idle 26
- Quality by Department, Revenue Breakdown

### 10.2 Department Dashboards

**Purpose:** Each department head (33) views KPIs specific to their domain.

**Refresh Rate:** 15 seconds
**Audience:** 33 Department Heads, Team Leads
**Data Sources:** Department-specific Prometheus metrics, department events

**Example: Content Department Dashboard**
- Department Health: 87.4% (YELLOW - quality score below 90% target)
- Content Created/24h: 4,847 (target: 4,000)
- Content Published/24h: 3,845 (target: 3,500)
- Content In Review: 32,847 (target: < 50,000)
- Quality by Content Type: Stories 89.4, Quizzes 91.2, KGs 94.8, Lesson 88.1, Audio 87.3
- Agents by Status, Pipeline Status, Active Incidents

### 10.3 Agent Dashboard

**Purpose:** View detailed metrics for any of the 8,500 agents.

**Refresh Rate:** 5 seconds
**Audience:** Engineering staff, Agent supervisors, Team Leads
**Data Sources:** Agent-specific Prometheus metrics, heartbeat, logs

**Layout:**
- Agent Identity: ID, name, type, team, department, version, uptime, pod, host
- Status: Healthy/State: Processing/Heartbeat: 0.04s ago
- Resource Usage (1h chart): CPU 68%, Memory 4.1GB/8GB, GPU 35%, Net 1.2MB/s
- Performance: Tasks/min 12, Latency P50 4.2s, Error Rate 1.2%, Confidence 0.91
- Error History (24h): Timestamp, Type, Count, Resolution
- Drift Status, Recent Tasks, Conf Trend (14 days), Recent Traces

### 10.4 Real-Time Operations Dashboard

**Purpose:** NOC wall monitors for on-call SREs.

**Refresh Rate:** 1 second (auto-refresh)
**Audience:** On-call SREs, Operations Team, NOC
**Data Sources:** All data sources

**Layout:**
- Active Incidents: P0/P1/P2 counts, MTTR 8.3 min
- System Status: API / DB / KG / AI all NORMAL
- Alert Rate (5min): 28/min (today: 847 alerts)
- Agent Health Map: 8,420 Healthy / 42 Degraded / 18 Suspected / 12 Down / 8 Dead
- Network Status: Bandwidth 4.2 Gbps, Packet loss 0.01%, Latency 12ms P95
- Pipeline Throughput: 845 items/hr total
- Error Rates: Content 0.9%, Platform 2.4%, Security 0.1%
- Alert Timeline (6h)

### 10.5 Pipeline Dashboards

**Purpose:** Track specific pipeline health, latency, throughput, failures.

**Refresh Rate:** 10 seconds
**Audience:** Pipeline owners, Team Leads, Pipeline engineers
**Data Sources:** Pipeline metrics from Prometheus, Traces from Jaeger, Logs from ES

**Layout:**
- Pipeline Identity: Name, owner, status, SLA target vs current
- Pipeline Stages Visual Flow with stage durations and bottlenecks
- Latency: P50/P95/P99 with trend
- Failure Breakdown: LLM timeout 62%, Validation 18%, Dependency 12%, Network 8%
- Trace Explorer waterfall charts (Jaeger integration)
- Recent Incidents

### 10.6 Cost Dashboards

**Purpose:** Track infrastructure and operating costs per department, team, and agent.

**Refresh Rate:** 1 hour
**Audience:** CE-AI, Finance team, Department Heads, Executives
**Data Sources:** AWS CUR, Prometheus resource usage, Kubernetes resource allocation

**Layout:**
- Total Monthly: $347,200 (budget: $380,000, $32,800 under)
- Cost per User: $0.12/user/month (trending down)
- Cost per Agent: $0.82/agent/day (trending down)
- Cost by Department: Content 27.1%, Infrastructure 19.5%, AI Teacher 15.0%, KG 9.9%
- Cost by Resource Type: Compute 48%, LLM API 26%, Storage 9%, Network 5%
- Cost Trend (6 months): $298K to $347K

### 10.7 Security Dashboard

(Detailed in Section 8.7)

**Purpose:** Real-time and aggregated security posture.

**Refresh Rate:** 10 seconds
**Audience:** Security Team, SOC Analysts, CISO, Executives
**Data Sources:** WAF logs, Auth logs, Network monitoring, DLP system, PagerDuty

### 10.8 Grafana Provisioning Architecture

All dashboards provisioned via GitOps in dedicated repository:

```
Repository: git@github.com:ai-company/grafana-dashboards.git
Branch: main (production) / staging (changes before deploy)

Directory Structure:
grafana-dashboards/
  dashboards/
    executive/executive-dashboard.json
    department/ (33 department dashboards)
    operations/ (realtime-ops, pipeline-overview, security)
    agents/agent-detail.json
    cost/cost-dashboard.json
    custom/ (user-created, reviewed before provisioning)
  datasources/ (prometheus, elasticsearch, jaeger, postgres)
  alerts/ (agent-alerts, infrastructure-alerts)
  provisioning.yaml
```

Deployment: CI/CD pipeline - merges to main auto-deploys via Grafana API
Rollback: git revert + re-deploy (full history preserved)

---


## 11. Tracing and Observability

### 11.1 Distributed Tracing Architecture

Every task, request, and operation within the AI Company must be traceable end-to-end across all agents and services.

**Trace ID Format:**
```
trace_id: "ae8f3c9d1f4a7280be4c923d1e4f832"  (128-bit, hex encoded, unique)
span_id:  "7f3a8c1e4b2d9f0a"                   (64-bit, hex encoded)
parent_span_id: "a1b2c3d4e5f6a7b8"            (64-bit, null for root span)
```

**Trace Context Propagation:**
- HTTP Headers: X-Trace-ID, X-Span-ID, X-Parent-Span-ID
- gRPC Metadata: trace-id, span-id
- Kafka Message Headers: trace_id, parent_span_id
- W3C Trace Context: traceparent and tracestate headers

### 11.2 Span Types

| Span Type | Description | Typical Duration | Always Sampled? |
|---|---|---|---|
| agent-processing | Agent performing primary function | 0.1s - 300s | P0 agents: Yes |
| llm-call | Call to an LLM model | 0.5s - 30s | Yes (P0, P1 agents) |
| database-query | PostgreSQL query execution | 0.1ms - 5s | No (1% sample) |
| cache-get | Redis cache read | 0.1ms - 50ms | No (0.1% sample) |
| cache-set | Redis write | 0.5ms - 100ms | No (0.1% sample) |
| api-call | HTTP/gRPC call to service | 10ms - 10s | Yes (P0 endpoints) |
| message-publish | Publish to Kafka topic | 1ms - 100ms | No (1% sample) |
| message-consume | Consume from Kafka topic | 0.1ms - 1s | No (1% sample) |
| file-io | S3 or EBS read/write | 10ms - 5s | No (1% sample) |
| verification-step | Verification agent review | 1s - 60s | Yes (P0, P1 agents) |
| pipeline-stage | Multi-agent pipeline stage | 1s - 300s | Yes (all pipelines) |
| human-review | Waiting for human review | 1min - 24h | Yes (100%) |

### 11.3 Span Attributes

Every span carriers standardized attributes:

**Required Attributes:**
- trace_id, span_id, parent_span_id
- service.name (agent type or service name)
- service.version (agent version)
- span.kind (server/client/producer/consumer/internal)
- agent.id (if it involves an agent)
- task.id (if it involves a task)
- pipeline.id (if it involves a pipeline)
- user.id (if it involves a user request)

**Optional Attributes:**
- model.name (for LLM calls)
- model.provider (OpenAI, Anthropic, etc.)
- cache.hit (true/false for cache operations)
- db.statement (sanitized SQL for database queries)
- http.method, http.url, http.status_code (for HTTP calls)
- messaging.destination (Kafka topic)
- error.type, error.message (if the span has an error)
- input.size, output.size (tokens for LLM, bytes for files)
- queue.wait_time (time spent in queue before processing)

### 11.4 Trace Sampling Strategy

| Agent Priority | Sampling Rate | Rationale |
|---|---|---|
| P0 agents (critical path) | 100% | Every action must be traceable for security and audit |
| P1 agents (high importance) | 10% | Sufficient for debugging common issues |
| P2 agents (standard) | 1% | Enough for statistical analysis |
| P3 agents (background) | 0.1% | Minimal sampling for cost efficiency |
| Error traces (all agents) | 100% (tail-based) | Always retain traces with errors |
| Slow traces (P95+) | 100% (tail-based) | Always retain slow traces for debugging |

**Head Sampling (at sidecar):** Probabilistic sampling before sending.
**Tail Sampling (at collector):** Override head sampling decision - retain all error traces regardless.
**Rate Limiting:** Max 10,000 spans/second total across all collectors.

### 11.5 Trace Visualization

**Waterfall Charts (Jaeger UI):**
```
[Root Span: pipeline: KG to Story (12.4s)]
  +-- [Span: topic-extraction (0.8s)]
  |     +-- [Span: llm-call (0.6s)]
  |     +-- [Span: database-query (0.1s)]
  +-- [Span: story-outline (4.2s)]  ★ Slowest span
  |     +-- [Span: llm-call (3.8s)]
  +-- [Span: story-drafting (6.1s)]
  |     +-- [Span: llm-call (5.5s)]
  |     +-- [Span: cache-get (0.01s)]
  +-- [Span: verification (1.2s)]
```

**Service Maps:** Generated by Jaeger showing:
- Nodes: Each agent type and service
- Edges: Communication between services (client-server relationships)
- Edge labels: Request rate, error rate, average latency
- Node color: Health status (green/yellow/red based on error rate)
- Node size: Relative throughput

**Dependency Graphs:** Generated from trace data showing:
- Which agents call which other agents
- Which databases and caches are accessed
- Which LLM models are used by which agents
- Traffic volume between components

### 11.6 Trace Querying

**Query Dimensions:**
- By trace_id: Direct lookup of specific trace
- By agent_id: All traces involving a specific agent
- By workflow_id: All traces for a specific workflow instance
- By user_id: All traces for a specific user request
- By pipeline_type: All KG to Story traces
- By error: All traces with any error span
- By latency: All traces longer than X seconds
- By time range: All traces in a specific window

**Trace Query Examples:**
```
Query: Find all traces where agent "story-writer-l3" had errors in the last 24h
Result: 87 traces, avg duration 42.3s, error types: [llm_timeout: 62, validation_failure: 25]

Query: Show all traces for user "user-8472332" AI Teacher session at 14:32
Result: 1 trace, 8 spans, total duration 2.4s, AI Teacher response received

Query: Find all slow traces (> 60s) in pipeline "KG to Story" in the last 7 days
Result: 347 traces, trend: increasing (42 last week vs 89 this week)
```

### 11.7 Trace-Storage Correlation

Logs, metrics, and traces are correlated through common IDs:

- **trace_id** appears in logs as log.trace_id for correlation
- **metric labels** include trace_id for linking metrics to traces
- **Grafana Explore** allows jumping from metric spike to related traces in one click
- **Logs to Traces:** From a log line with trace_id, click to open corresponding trace
- **Traces to Logs:** From a span, click to see all logs emitted during that span

---


## 12. SLAs and SLOs

### 12.1 System SLOs

| SLO | Target | Measurement Period | Measurement Method |
|---|---|---|---|
| Agent Availability | 99.99% | Monthly | % of agents sending heartbeat on time (allows 4.3 min downtime/month) |
| API Response Time (P95) | < 500ms | Rolling 28 days | Prometheus histogram agent_request_duration_seconds |
| Content Generation Time | < 60s per item | Rolling 7 days | Pipeline duration histogram |
| AI Teacher Response | < 2s | Rolling 7 days | AI Teacher span duration |
| Pipeline Completion | Within 2x estimated time | Per pipeline instance | Pipeline SLA breach counter |
| Data Durability | 99.9999999% (11 nines) | Annual | Zero data loss events |
| Dashboard Availability | 99.9% | Monthly | Grafana probe |
| Search Availability | 99.9% | Monthly | Search endpoint probe |
| Payment Processing | 99.9% | Monthly | Payment endpoint probe |

### 12.2 Service-Level Indicators (SLIs)

**Availability SLI:**
```
availability = (total_time - downtime) / total_time
where downtime = time when > 5% of agents in a department fail heartbeat
```

**Latency SLI:**
```
latency_sli = count(requests with latency < threshold) / total_requests
P95 latency target: 95% of requests complete in < 500ms
```

**Throughput SLI:**
```
throughput_sli = actual_throughput / expected_throughput
Expected throughput defined per pipeline (updated weekly)
```

**Quality SLI:**
```
quality_sli = percentage of content items with quality score >= threshold
Threshold: 80/100 for acceptable quality, 90/100 for good quality
```

**Error Rate SLI:**
```
error_sli = 1 - (error_count / total_requests)
Target: error rate < 1% for all agent types
```

### 12.3 Error Budgets

Each SLO has an associated error budget - the maximum allowable failure over the measurement period.

| SLO | Error Budget (Monthly) | Error Budget (Daily) |
|---|---|---|
| Agent Availability 99.99% | 4.32 minutes | 8.64 seconds |
| API Latency (P95) < 500ms | 5% of requests may exceed | ~1,440 min of 28,800 min |
| Content Generation < 60s | 5% of items may exceed | Tracked per pipeline |
| AI Teacher < 2s | 5% of requests may exceed | Tracked hourly |

**Error Budget Policy:**
- Error budget consumed by any downtime or SLO breach
- Error budget resets on the 1st of each month
- When error budget reaches 50% consumed: P3 alert to team
- When error budget reaches 75% consumed: P2 alert to department head
- When error budget reaches 100% consumed: Feature freeze, all deployments stopped, incident review required
- When error budget is exhausted: Emergency meeting with CE-AI and department heads

### 12.4 Burn Rate Alerts

Burn rate measures how fast the error budget is being consumed relative to the SLO period.

| Burn Rate | Duration | Alert | Action |
|---|---|---|---|
| > 2x (consuming budget 2x faster than allowed) | 1 hour | P2 | Investigate, prepare incident response |
| > 5x | 30 minutes | P1 | Immediate incident response |
| > 10x | 10 minutes | P0 | All-hands incident |
| > 20x | 5 minutes | P0-CRITICAL | Auto-rollback last deployment, freeze all changes |

**Burn Rate Example:**
```
SLO: 99.99% = 4.32 min/month error budget
If we have 2 minutes of downtime in the first 3 days of the month:
  Burn rate = (2 min / 3 days) / (4.32 min / 30 days) = 0.667 / 0.144 = 4.63x
  Alert: P1 - Burn rate 4.63x sustained
  Action: Immediate incident response, investigate cause, prepare mitigation
```

### 12.5 SLO Monitoring Dashboard

```
SLO STATUS - Month to Date (July 2026)

SLO                       Target    Current    Status    Budget Remaining
─────────────────────────────────────────────────────────────────────
Agent Availability        99.99%    99.98%     VIOLATED  0.48 min
API Latency P95 < 500ms   95.0%     98.2%      OK        87% remaining
Content Gen < 60s         95.0%     96.4%      OK        72% remaining
AI Teacher < 2s           95.0%     97.8%      OK        64% remaining
Data Durability 11x9s     100%      100%       OK        100% remaining
Dashboard Uptime 99.9%    99.99%    99.99%     OK        100% remaining
Payment Proc. 99.9%       99.999%   99.999%    OK        100% remaining

BURN RATE TRACKING:
  Agent Availability: 3.7x (ALERT - P1)
  API Latency: 0.3x (normal)
  Content Gen: 0.8x (normal)
  AI Teacher: 0.6x (normal)

OVERALL ERROR BUDGET CONSUMPTION: 78%
Status: YELLOW - Error budget nearly exhausted for Availability SLO.
Action: Feature freeze recommended until availability improves.
```

### 12.6 SLO Attainment Reporting

**Monthly SLO Report (provided to CE-AI and executives):**

| SLO | Target | July MTD | June | May | QoQ Trend |
|---|---|---|---|---|---|
| Agent Availability | 99.99% | 99.982% | 99.991% | 99.993% | -0.011% Declining |
| API Latency (P95) | < 500ms | 247ms | 312ms | 389ms | Improving |
| Content Gen < 60s | 95% | 96.4% | 95.8% | 94.7% | Improving |
| AI Teacher < 2s | 95% | 97.8% | 97.1% | 96.2% | Improving |
| Data Durability | 11x9s | 100% | 100% | 100% | Stable |

**Quarterly Business Review Metrics:**
- Total downtime: 8.4 min (Q2), 12.1 min (Q1)
- Total incidents: 47 (Q2), 62 (Q1)
- Mean Time to Detect (MTTD): 2.3 min (Q2), 3.1 min (Q1)
- Mean Time to Acknowledge (MTTA): 1.2 min (Q2), 1.8 min (Q1)
- Mean Time to Resolve (MTTR): 8.4 min (Q2), 12.7 min (Q1)
- Mean Time Between Failures (MTBF): 44.6 hours (Q2), 34.3 hours (Q1)

---


## 13. Monitoring System Itself (Watchdog)

### 13.1 Philosophy of Meta-Monitoring

The monitoring system is the most critical component of the entire platform. If monitoring fails, the entire company is flying blind. Therefore, the monitoring system is designed with the same (or higher) reliability standards as the systems it monitors. Every component of the monitoring stack is itself monitored, and the monitoring of the monitors is itself monitored.

This creates a chain of monitoring that terminates at a **simple, independent, external watchdog** that requires no dependencies on the monitoring stack to function.

### 13.2 Monitoring Service Health Checks

Each monitoring component performs self-checks and exposes a health endpoint:

| Component | Health Check | Interval | Failure Action |
|---|---|---|---|
| Prometheus | Self-test: can scrape metrics, evaluate rules | 10s | Restart Prometheus, failover to secondary |
| Alertmanager | Self-test: can receive alerts, route to receivers | 15s | Restart Alertmanager, mesh rebalances |
| Grafana | Self-test: can query data sources, render dashboards | 10s | Restart Grafana, load balancer reroutes |
| Elasticsearch | Cluster health API: green status, all shards active | 15s | Rebalance shards, replace failed node |
| Jaeger Collector | gRPC health check: can receive spans | 10s | Restart collector, traffic reroutes |
| Thanos Store | Self-test: can query object store | 30s | Restart Thanos, querier retries |
| Heartbeat Receiver | Self-test: can accept heartbeats | 5s | Failover to secondary receiver |
| Kafka Connect | Self-test: consumer group healthy, no lag | 15s | Restart connector, rebalance group |

**Health Check Endpoint Standard:**
```
GET /health
Response:
{
  "status": "healthy" | "degraded" | "unhealthy",
  "timestamp": "2026-07-09T14:32:17.042Z",
  "version": "2.4.1",
  "checks": {
    "self": { "status": "ok", "latency_ms": 2 },
    "upstream_dependency": { "status": "ok", "latency_ms": 45 },
    "storage": { "status": "ok", "used_percent": 42.3 }
  },
  "uptime_seconds": 847201,
  "last_error": null
}
```

### 13.3 Self-Check Daemon

A dedicated self-check daemon runs alongside the monitoring stack, performing comprehensive checks every 10 seconds:

**Self-Check Categories:**

**Data Completeness Checks:**
- All 8,500 agents sent heartbeat in last 10s interval (gap detection)
- All 5 million distinct metrics received in last minute (coverage check)
- No duplicate heartbeats from same agent in same interval (duplicate detection)
- No gap in metric collection > 30 seconds for any critical component
- Log indexing rate matches expected log generation rate
- Span ingestion rate matches expected trace generation rate

**Performance Checks:**
- Prometheus query latency (P99) < 1s for standard queries
- Elasticsearch query latency (P99) < 500ms for recent indices
- Grafana dashboard load time < 3s for standard dashboards
- Alert evaluation time < 5s for all rules
- Notification delivery time < 10s (SLA: 1min for P0)
- Kafka consumer lag < 1,000 for all monitoring topics

**Alerting System Checks:**
- Can Alertmanager receive and route a test alert
- Can notification service reach all channels (Slack, SMS, Phone, Email)
- Can PagerDuty create and acknowledge a test incident
- Test alert routing to on-call engineer succeeds (synthetic alert generated every hour)
- Auto-remediation rules can execute a dry-run action

**Storage System Checks:**
- Prometheus TSDB: no corruption, no write errors, compaction working
- Elasticsearch: no red shards, no unassigned shards, no write rejections
- Cassandra (Jaeger backend): no hinted handoff failures, compaction working
- Thanos object store: read/write test succeeds
- PostgreSQL (event store): connection pool healthy, replication lag < 5s

### 13.4 Independent External Watchdog

The external watchdog is a **completely independent** monitoring system that requires no dependencies on the main monitoring stack.

**Watchdog Requirements:**
- Runs on separate physical infrastructure (different cloud provider or bare metal)
- Requires no network access to the main monitoring stack to function
- Has its own independent power, network, and compute
- Can send alerts through a completely separate channel (dedicated phone line, separate Slack workspace)
- Cannot be affected by any failure in the main infrastructure

**Watchdog Implementation:**

```
+------------------------------------------------------------------+
|                    EXTERNAL WATCHDOG SYSTEM                        |
|                  (Separate Cloud: us-west-2)                       |
|                                                                     |
|  +-------------------+                                             |
|  | Watchdog Daemon   |  Checks:                                     |
|  | (Runs on EC2,     |   - Prometheus /health endpoint every 10s   |
|  |  separate AWS acct)|   - Grafana /health endpoint every 10s     |
|  |                   |   - Elasticsearch /health every 15s         |
|  | with independent  |   - Heartbeat receiver /health every 5s     |
|  |  monitoring stack)|   - Alertmanager /health every 15s          |
|  +--------+----------+   - Canary agent heartbeat (external)       |
|           |               - API gateway /health every 10s           |
|           |               - Can create test alert every 60min       |
|           v                                                         |
|  +-------------------+                                             |
|  | Watchdog Alert    |  - If any health check fails > 3 times      |
|  |  Manager          |  - If canary agent heartbeat missing > 30s  |
|  +-------------------+  - If test alert not delivered in 90s       |
|           |              - If self-check daemon unresponsive        |
|           v                                                         |
|  +-------------------+                                             |
|  | Watchdog          |  - Separate PagerDuty service               |
|  | Notification      |  - Separate Slack workspace (#watchdog)     |
|  | (Independent)     |  - Separate SMS provider (AWS SNS)          |
|  +-------------------+  - Phone call via separate Twilio account    |
|                                                                     |
+------------------------------------------------------------------+
```

**Watchdog Alert Thresholds:**

| Watchdog Check | Threshold | Action |
|---|---|---|
| Prometheus health endpoint fails | 3 consecutive failures (30s) | Watchdog alert P1 |
| Grafana health endpoint fails | 3 consecutive failures (30s) | Watchdog alert P1 |
| Elasticsearch health != green | 2 consecutive checks (30s) | Watchdog alert P1 |
| Heartbeat receiver health fails | 3 consecutive failures (15s) | Watchdog alert P0 |
| Alertmanager health fails | 2 consecutive checks (30s) | Watchdog alert P1 |
| Canary agent heartbeat missing | > 30s since last heartbeat | Watchdog alert P0 |
| Test alert not delivered | > 90s since test alert sent | Watchdog alert P2 |
| Self-check daemon unresponsive | > 60s since last self-check | Watchdog alert P2 |
| Monitoring data gap detected | > 60s gap in any critical metric | Watchdog alert P1 |

### 13.5 Failover Architecture

The monitoring system has full redundancy with automatic failover.

**Monitoring Failover Levels:**

| Level | Failure Scenario | Failover Action | RTO | RPO |
|---|---|---|---|---|
| L1 | One Prometheus instance fails | Prometheus HA: querier uses remaining instances, failed instance auto-restarts | 0 (no impact) | 0 |
| L2 | Both Prometheus instances fail | Thanos querier reads from object store (historical data) + secondary Prometheus pair activates | 30s | 5min |
| L3 | Elasticsearch cluster degrades | Read from replica shards, degraded search experience, auto-rebalance | 0 (degraded) | 0 |
| L4 | Alertmanager fails | Remaining Alertmanager instances absorb the load | 0 (no impact) | 0 |
| L5 | Full monitoring stack failure (all components) | External watchdog alerts, manual failover to secondary monitoring stack | 5min | 10min |
| L6 | Primary region failure (us-east-1) | Failover to secondary region (us-west-2) with full monitoring stack | 10min | 15min |

**Monitoring Stack Failover Steps (L5):**
1. External watchdog detects complete monitoring stack failure (> 60s)
2. Watchdog alerts on-call SRE via independent channel
3. SRE activates secondary monitoring stack via runbook
4. Secondary stack (pre-provisioned in separate AWS account) comes online
5. DNS switch: monitoring.ai-company.com points to secondary stack
6. Agent sidecars detect new monitoring endpoints via config reload
7. Data backfill from agent-sidecar buffers (1-hour buffer in sidecar)
8. Post-recovery: Analyze root cause of primary monitoring failure

### 13.6 Monitoring Data Integrity

**Integrity Checks:**

| Check | Frequency | Description | Action on Failure |
|---|---|---|---|
| Gap detection | Every 5 minutes | Scan metric store for gaps > 30s in any time series | Log gap, alert P2 if gap > 60s |
| Duplicate detection | Every 5 minutes | Check for duplicate data points (same metric, same timestamp) | Log duplicates, alert P3 if > 0.1% |
| Heartbeat sequence check | Every minute | Verify heartbeat sequence numbers are monotonic per agent | Alert P2 if sequence gap detected |
| Cross-source consistency | Every hour | Compare agent counts from Prometheus vs heartbeat receiver vs Kubernetes | Alert P2 if mismatch > 1% |
| Log-metric correlation | Every hour | Verify log volume correlates with metric volume (should be proportional) | Alert P3 if deviating > 20% |
| Trace-metric correlation | Every hour | Verify trace count correlates with request count | Alert P3 if deviating > 20% |
| Storage integrity | Daily | Checksum verification of stored metric blocks | Alert P1 if corruption detected |
| Data retention compliance | Weekly | Verify data older than retention policy is purged | Alert P2 if retention exceeded |

### 13.7 Watchdog Dashboard

```
WATCHDOG STATUS — Monitoring System Health

Monitoring Stack: OK (all components healthy)
  Prometheus: OK (3/3 instances, 2.4M series)
  Elasticsearch: OK (3/3 nodes, green status)
  Grafana: OK (5/5 replicas, avg load 42%)
  Alertmanager: OK (3/3 replicas, mesh healthy)
  Heartbeat Receiver: OK (3/3 replicas, 8,420 agents heartbeating)
  Thanos: OK (compaction running, no errors)

Self-Check Daemon: OK (last check: 2s ago, 47/47 checks passed)
External Watchdog: OK (last heartbeat: 1s ago, independent channel healthy)

ALERTS (Monitoring System):
  P0: 0  P1: 0  P2: 1 (Elasticsearch node es-data-1 has 70% disk usage)
  P3: 2 (Grafana plugin update available, Thanos compactor slow)
  P4: 1 (Daily integrity check completed: no gaps, no duplicates, 0 errors)

CANARY AGENT: OK (heartbeat received, latency 1.2ms, no errors in 24h)

TEST ALERT: Sent 47s ago. Status: DELIVERED (to Slack #ops-alerts, SMS, PagerDuty)
  Ack time: 12s (SLA: 60s) - 8s faster than SLA

MONITORING COST: $26,100/month (7.6% of total infra cost) - Within budget
```

### 13.8 Operational Runbooks for Monitoring Failures

**Runbook M-001: Prometheus Down**
```
Symptoms:
- Prometheus health endpoint returning 503
- Grafana panels showing "No data" errors
- Alertmanager receiving "PrometheusDown" alert

Immediate Actions:
1. Check Prometheus pod status: kubectl get pods -n monitoring
2. Check Prometheus logs: kubectl logs prometheus-0 -n monitoring
3. If OOM killed: increase memory limit, restart
4. If disk full: increase PVC size, run compaction
5. If config error: rollback last config change
6. If all else fails: restore from Thanos object store, restart from backup

Expected Resolution: < 5 minutes
Escalation: If not resolved in 5 min, page SRE lead
```

**Runbook M-002: Elasticsearch Red Status**
```
Symptoms:
- Elasticsearch cluster health = red (one or more primary shards unassigned)
- Kibana returning errors
- Log search returning incomplete results

Immediate Actions:
1. Check cluster health: GET _cluster/health
2. Identify unassigned shards: GET _cat/shards?v&h=index,shard,prirep,state,node,unassigned.reason
3. If node failed: let cluster auto-reallocate shards to remaining nodes (typically < 60s)
4. If disk full on node: increase disk space, rebalance shards
5. If all else fails: restore from snapshot

Expected Resolution: < 10 minutes
Escalation: If not resolved in 15 min, page infrastructure lead
```

**Runbook M-003: Complete Monitoring Stack Failure**
```
Symptoms:
- All monitoring health endpoints unreachable
- External watchdog alert triggered
- No monitoring data visible on any dashboard

Immediate Actions:
1. Acknowledge watchdog alert
2. Open secondary monitoring stack (pre-provisioned)
3. Update DNS to point to secondary stack
4. Verify secondary stack is receiving data
5. Notify all engineering via Slack #ops that monitoring has failed over
6. Investigate primary stack failure root cause
7. Alert all teams to retain local agent sidecar buffer data

Expected Resolution: < 15 minutes for failover
Full Recovery: < 2 hours to restore primary stack
Escalation: P0 all-hands incident declared
```

---

## Document References

| Reference | Document | Description |
|---|---|---|
| [REF-01] | 05_Agent_Architecture.md | Agent lifecycle, states, and configuration |
| [REF-02] | 07_Agent_Communication.md | Message bus, inter-agent pipelines, workflow orchestration |
| [REF-03] | 12_Quality_Assurance.md | Quality metrics, verification agents, human review process |
| [REF-04] | 17_Security_Architecture.md | Security model, authentication, authorization, DLP |
| [REF-05] | 21_Incident_Response.md | Incident severity, response procedures, post-mortem process |
| [REF-06] | 22_Disaster_Recovery.md | Backup strategies, failover procedures, RTO/RPO definitions |
| [REF-07] | 23_Cost_Management.md | Cost allocation, budgeting, resource optimization |

---

## Appendices

### Appendix A: Metric Naming Conventions

```
Format: {domain}_{subdomain}_{measurement}_{unit}
Examples:
  agent_cpu_usage_percent
  pipeline_duration_seconds
  content_quality_score
  user_session_duration_minutes
  security_auth_failure_count
```

### Appendix B: Logging Standards

```
Format: JSON structured logging
Required fields: timestamp, level, logger, message, trace_id, span_id, agent_id, service
Levels: DEBUG, INFO, WARN, ERROR, FATAL
Log retention: 30 days hot, 90 days warm, 1 year cold, 7 years archived
```

### Appendix C: Dashboard Naming Conventions

```
{audience}_{domain}_{focus}
Examples:
  executive_company_overview
  department_content_quality
  agent_detail_performance
  operations_realtime_status
  cost_department_breakdown
```

---

*Document Version: 2.4*
*Last Updated: 2026-07-09*
*Prepared by: Infrastructure & Operations Division*
*Approved by: CE-AI, CTO, Head of Infrastructure*

---
