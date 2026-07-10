# AIOS High Availability System — Engineering Specification v1.0

## Document Control
- **Document ID:** AIOS-SPEC-HA-001
- **Version:** 1.0
- **Status:** APPROVED
- **Last Updated:** 2026-07-10
- **Owner:** AIOS Platform Engineering — Site Reliability Team
- **Classification:** CONFIDENTIAL

---

## Table of Contents
1. Purpose
2. Mission
3. Responsibilities
4. Non-Responsibilities
5. Architecture Overview
6. Internal Components
7. External Components
8. Dependencies
9. Inputs
10. Outputs
11. Data Structures
12. Execution Flow
13. State Management
14. Communication Protocols
15. APIs
16. Events
17. Queues
18. Caching
19. Memory
20. Persistence
21. Validation
22. Retry Logic
23. Error Recovery
24. Security
25. Authentication
26. Authorization
27. Permissions
28. Monitoring
29. Logging
30. Metrics
31. Tracing
32. Scalability
33. Performance
34. Optimization
35. Testing Strategy
36. Risk Analysis
37. Failure Scenarios
38. Recovery Procedures
39. Operational Limits
40. Maintenance
41. Future Improvements
42. Examples
43. Acceptance Criteria
44. Definition of Done

---

## 1. Purpose

The AIOS High Availability system ensures that all AIOS components and services remain operational despite infrastructure failures, software crashes, network partitions, and other anomalies. It eliminates single points of failure through redundancy, automated failover, and self-healing mechanisms. The system provides the foundational reliability layer that enables AIOS to meet its availability commitments to agents, workflows, and downstream consumers.

## 2. Mission

To achieve 99.999% availability (five nines) for critical AIOS components and 99.99% for standard components through zero-single-point-of-failure design, automated failure detection and recovery, and proactive health management across all deployment topologies. The system targets sub-15-second failure detection and sub-30-second failover for stateless components and sub-120-second failover for stateful components.

## 3. Responsibilities

- **HA Design Principles:** No single point of failure across any deployment tier; full redundancy at component, node, and AZ levels; automated failover without human intervention; self-healing recovery for common failure patterns.
- **Component Redundancy:** Active-active (all replicas serve traffic) for stateless components, active-passive (standby takes over) for stateful components, N+1 (one extra replica) for critical services.
- **Service HA:** Multiple replicas per component with pod anti-affinity rules ensuring distribution across AZs and nodes.
- **Data HA:** Replication factor 3 for all stateful data stores, synchronous commit for writes, automatic leader failover for Raft/Paxos consensus.
- **Stateful HA:** Raft/Paxos consensus for critical state stores (etcd, CockroachDB), automatic leader election, log replication, and transparent failover.
- **Cross-AZ Deployment:** Minimum 3 availability zones per region, with component replicas distributed across all AZs to survive a single-AZ failure.
- **Connection Draining:** Graceful shutdown with in-flight request completion, connection migration, and session persistence during failover events.
- **Health Monitoring:** Comprehensive liveness, readiness, and startup probes at pod, service, and application levels with configurable intervals and thresholds.
- **Failure Detection:** Multi-layer health checking with configurable intervals (1-60s), failure thresholds (1-10), and success thresholds (1-5) to prevent false positives while maintaining fast detection.
- **Automated Recovery:** Self-healing at the pod level (restart on failure), node level (reschedule on node failure), and application level (leader re-election for stateful services).
- **Chaos Engineering:** Continuous failure injection testing to validate HA mechanisms and identify weaknesses before they cause production incidents.

## 4. Non-Responsibilities

- **Region-Level Disaster Recovery:** Multi-region failover and recovery orchestration is handled by the AIOS Disaster Recovery system.
- **Scaling Decisions:** Determining when to add or remove replicas is handled by the AIOS Scaling System.
- **Data Backup:** Point-in-time backup and restore of data is handled by the AIOS Backup System.
- **Infrastructure Provisioning:** Provisioning of servers, networks, and storage is handled by the AIOS Cluster System.
- **Traffic Load Balancing:** Per-request load balancing and traffic routing is handled by the Ingress/Load Balancer and service mesh.
- **Application-Level Logic:** Component-specific error handling and recovery logic is the responsibility of individual component developers.

## 5. Architecture Overview

### 5.1 HA Design Principles

1. **No Single Point of Failure:** Every component has at least 2 replicas deployed across separate failure domains.
2. **Fault Isolation:** Components are isolated by namespace, node, and AZ to prevent cascading failures.
3. **Graceful Degradation:** Partial failure of a component does not cascade to other components; the system operates in degraded mode.
4. **Automatic Recovery:** Standard failure patterns trigger automated recovery without any human intervention.
5. **Fast Detection:** Health check interval < 5s, failure detection threshold < 15s for critical components.
6. **Self-Healing First:** Always attempt automated recovery before escalating to human operators.

### 5.2 Deployment Topology

```
Per Region (Minimum 3 AZs):

   AZ1                    AZ2                    AZ3
┌─────────┐             ┌─────────┐             ┌─────────┐
│ Kernel  │             │ Kernel  │             │ Kernel  │
│ (3 pods)│             │ (3 pods)│             │ (3 pods)│
│         │             │         │             │         │
│ API GW  │             │ API GW  │             │ API GW  │
│ (2 pods)│             │ (2 pods)│             │ (2 pods)│
│         │             │         │             │         │
│ Runtime │             │ Runtime │             │ Runtime │
│ (5 pods)│             │ (5 pods)│             │ (5 pods)│
│         │             │         │             │         │
│ Broker  │             │ Broker  │             │ Broker  │
│ (1 pod) │             │ (1 pod) │             │ (1 pod) │
└─────────┘             └─────────┘             └─────────┘

Stateful Components (3 replicas across AZs):
┌──────────────────────────────────────────────┐
│  etcd: Leader(az1), Follower(az2), Follower(az3) │
│  CockroachDB: 9 nodes (3 per AZ)            │
│  Redis: 6 shards × 3 replicas across AZs    │
│  Kafka: 12 partitions × 3 replicas across AZs│
└──────────────────────────────────────────────┘
```

### 5.3 HA Architecture Components

```
┌─────────────────────────────────────────────────────────────┐
│                  HA ORCHESTRATOR                              │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐       │
│  │ Health Check │  │ Failover     │  │ Connection   │       │
│  │ Manager      │  │ Orchestrator │  │ Drain Manager│       │
│  └──────────────┘  └──────────────┘  └──────────────┘       │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐       │
│  │ Stateful HA  │  │ Health       │  │ Chaos Test   │       │
│  │ Manager      │  │ Aggregator   │  │ Controller   │       │
│  └──────────────┘  └──────────────┘  └──────────────┘       │
└─────────────────────────────────────────────────────────────────────┘
```

## 6. Internal Components

### 6.1 Health Check Manager

**Component ID:** HA-HCM-001
**Language:** Go
**Runtime:** AIOS Kernel Operator
**Replicas:** 3-5

**Supported Check Types:**
| Type | Description | Default Interval | Default Timeout | Failure Threshold | Success Threshold |
|------|------------|-----------------|--------------|-------------------|-----------------|
| HTTP | GET endpoint, check status code 200-399 | 5s | 3s | 3 | 2 |
| gRPC | gRPC health check protocol | 5s | 3s | 3 | 2 |
| TCP | TCP port probe | 5s | 3s | 3 | 2 |
| Exec | Run command inside container | 10s | 5s | 3 | 2 |
| Custom | Component-specific custom check | 10s | 5s | 2 | 1 |

**Health Check Configuration (Per Component):**
```yaml
health_checks:
  - component: aios-kernel
    liveness:
      httpGet: { path: /healthz, port: 8080 }
      initialDelaySeconds: 30
      periodSeconds: 5
      failureThreshold: 3
      successThreshold: 1
    readiness:
      httpGet: { path: /readyz, port: 8080 }
      initialDelaySeconds: 5
      periodSeconds: 5
      failureThreshold: 3
      successThreshold: 1
    startup:
      httpGet: { path: /startupz, port: 8080 }
      initialDelaySeconds: 0
      periodSeconds: 2
      failureThreshold: 60  # 2 min startup allowed
```

### 6.2 Failover Orchestrator

**Component ID:** HA-FO-001
**Language:** Go
**Runtime:** AIOS Kernel Operator
**Replicas:** 3-5 (active-active)

**Capabilities:**
- Monitors health events and triggers failover/recovery actions
- Orchestrates pod-level restart: terminate → schedule → restart → verify
- Orchestrates service-level failover: change endpoint routing via service mesh
- Handles leader re-election for Raft/Paxos based stateful services
- Implements circuit breaker to prevent repeated failover attempts on unstable components
- Tracks failover history to detect persistent failure patterns

**Failover Actions:**
| Failure Type | Action | Target Time |
|-------------|--------|-------------|
| Pod crash | Pod restart | < 10s |
| Pod crash loop | Pod reschedule to new node | < 60s |
| Node failure | Reschedule all pods to remaining nodes | < 120s |
| AZ failure | Distribute pods to remaining AZs | < 300s |
| Leader failure | Leader re-election | < 30s |
| Service unhealthy | Traffic re-routing | < 5s |

### 6.3 Connection Drain Manager

**Component ID:** HA-CDM-001
**Language:** Go + Rust
**Runtime:** Sidecar per pod

**Drain Phases:**
1. PreStop Hook (SIGTERM):
   - Mark pod as NotReady (remove from service endpoints)
   - Wait for in-flight request completion (max 30s)
   - Close active connections gracefully
   - For stateful pods: flush buffers, checkpoint state
2. Termination Grace Period (max 60s):
   - If not completed after 30s: force close remaining connections
   - Send SIGKILL after grace period
3. Post-Termination:
   - Verify pod terminated
   - Update registry (remove from service discovery)

**Connection Draining Per Protocol:**
| Protocol | Drain Strategy | Timeout |
|----------|-------------|---------|
| HTTP/1.1 | Complete in-flight, reject new | 30s |
| HTTP/2 | GOAWAY frame, drain streams | 30s |
| gRPC | Graceful GOAWAY, drain streams | 30s |
| TCP | Complete pending reads/writes | 10s |
| WebSocket | Close frame, drain messages | 15s |
| Database | Commit open transactions | 60s |

### 6.4 Stateful HA Manager

**Component ID:** HA-SHA-001
**Language:** Rust
**Runtime:** AIOS Kernel Service

**Consensus Protocol Configuration:**
| Store | Protocol | Nodes | Replica Factor | Commit Strategy |
|-------|----------|-------|---------------|-----------------|
| etcd | Raft | 3-5 | 3 | Majority write |
| CockroachDB | Raft | 9 | 3 | Majority write |
| Kafka | Raft | 3 | 3 | ISR + min.insync.replicas=2 |
| Redis | Sentinel | 3 | 3 | Quorum: 2 |
| Raft-based stores | Raft | 3 | 3 | Majority write |

**Leader Election:**
- Failure detection: Heartbeat timeout (default: 150ms for etcd, 500ms for CockroachDB)
- Request vote: After timeout, candidate requests votes from all other members
- Majority vote wins: Requires (n/2 + 1) votes to become leader
- Term increment: Each election cycle increments the term number
- Pre-Vote: Prevent disruptions from stale candidates (raft PreVote enabled)

### 6.5 Health Aggregator

**Component ID:** HA-HA-001
**Language:** Go
**Replicas:** 3

**Aggregation Hierarchy:**
```
Pod Health (liveness + readiness)
    ↓
Component Health (all pods of a component)
    ↓
Service Health (all components that form a service)
    ↓
Region Health (all services in a region)
    ↓
Fleet Health (all regions)
```

**Health Scoring Algorithm:**
```
Component Health Score = (healthy_pods / total_pods) × 1.0
  If any pod CrashLoopBackOff → -0.2 per pod
  If any pod NotReady → -0.1 per pod
  
Service Health Score = min(all component health scores) × weight
  Weighted by component criticality:
  - Critical (kernel, registry): 5.0
  - High (scheduler, runtime): 3.0
  - Normal (all others): 1.0

Region Health Score = min(service health scores) × region_bonus
  Region bonus: 1.0 (healthy), 0.9 (1 AZ down), 0.5 (2 AZ down)
```

### 6.6 Chaos Test Controller

**Component ID:** HA-CTC-001
**Language:** Python
**Runtime:** AIOS Chaos Engineering Pod

**Failure Injection Types:**
| Type | Tool | Parameters |
|------|------|-----------|
| Pod kill | kubectl delete pod | Count: 1-10, interval: 5-60s |
| Node failure | Stop node instance | Count: 1 per AZ |
| Network latency | Istio fault injection | Latency: 100-5000ms, ratio: 1-100% |
| Packet loss | Istio fault injection | Loss: 1-50%, ratio: 1-100% |
| CPU stress | stress-ng | CPU: 80-100%, duration: 30-300s |
| Memory stress | stress-ng | Memory: 80-95%, duration: 30-300s |
| Disk full | dd fill | Disk: 80-95%, file size: 1-10GB |
| Network partition | iptables | Drop: all egress, all ingress, targeted |

**Chaos Test Schedule:**
- Daily: Random pod kill (1 pod per component)
- Weekly: AZ failure simulation (drain 1 AZ, verify failover)
- Monthly: Full chaos day (all failure types + simultaneous failures)
- Quarterly: Region failure simulation (full region failover + failback)

## 7. External Components

| Component | Role | Data Flow |
|-----------|------|-----------|
| AIOS Kernel | Managed component | Health checks, failover actions |
| AIOS Runtime | Managed component | Health checks, failover actions |
| AIOS Message Broker | Managed component | Health checks, partition rebalancing |
| AIOS Database | Managed component | Health checks, failover |
| AIOS Cache | Managed component | Health checks, shard failover |
| AIOS API Gateway | Managed component | Health checks, connection draining |
| AIOS Scheduler | Managed component | Health checks, failover |
| AIOS Registry | Managed component | Health checks, failover |
| AIOS Monitoring | Observability | Health metrics, failover events |
| AIOS Disaster Recovery | Escalation | Region-level failure escalation |
| Istio | Service mesh | Traffic routing, mTLS, fault injection |
| Kubernetes | Orchestration | Pod lifecycle, node management |

## 8. Dependencies

**Build:** Go 1.22+, Rust 1.80+, Python 3.12+, Protocol Buffers 3.x, Istio Client-Go 1.22+

**Runtime:** Kubernetes 1.30+, Istio 1.22+, etcd 3.5+, CockroachDB 24.1+, Redis 7.2+, Kafka 3.7+, RabbitMQ 4.0+, Envoy 1.30+, Prometheus 2.48+

## 9. Inputs

**Health Check Inputs:**
- Liveness probe results (per pod): {pod_id, healthy, latency, error}
- Readiness probe results (per pod): {pod_id, ready, connections, latency}
- Startup probe results (per pod): {pod_id, started, duration}
- Node conditions: {node_id, Ready, MemoryPressure, DiskPressure, PIDPressure}
- Pod status events: {pod_id, status: CrashLoopBackOff|Running|Pending|Error}
- Resource metrics: CPU, memory, disk, network per pod per node

**Failover Inputs:**
- Health state transitions: healthy → unhealthy
- Node failure events: node NotReady or terminated
- Leader election events: leader lost, new leader elected
- Manual failover commands
- Chaos test triggers

**Operator Commands:**
- Trigger pod restart
- Trigger service failover
- View health status
- Override health check thresholds
- Enable/disable failover for component

## 10. Outputs

**Events:**
| Event | Description | Priority |
|-------|-----------|----------|
| pod.unhealthy | Pod health check failed | HIGH |
| pod.restarted | Pod restarted (cause, duration) | INFO |
| pod.crashloop | Pod entering crash loop (retries) | CRITICAL |
| service.failover | Service failover initiated/completed | HIGH |
| node.unhealthy | Node condition unhealthy | CRITICAL |
| az.failure | Availability zone impacted | CRITICAL |
| leader.changed | Raft leader election completed | INFO |
| drain.completed | Connection drain completed | INFO |
| drain.failed | Connection drain timed out | WARNING |
| health.aggregated | Health score computed | DEBUG |

**Metrics:**
- pod_uptime_seconds{component} — Gauge per pod
- failover_count_total{component, type, result} — Counter
- failover_duration_seconds{component, type} — Histogram
- availability_percent{component} — Gauge (rolling 30d)
- health_score{component, tier} — Gauge (0.0-1.0)
- leader_election_duration_seconds — Histogram
- drain_duration_seconds{protocol} — Histogram
- circuit_breaker_state{component} — Gauge (0=closed,1=open,2=half-open)

## 11. Data Structures

### 11.1 ComponentAvailability (In-Memory + etcd)

```rust
struct ComponentAvailability {
    component_id: String,
    target_availability: f64,  // 0.99999 (five nines)
    current_availability: f64,     // Rolling 30-day availability
    uptime_seconds: u64,
    last_failover: Option<DateTime<Utc>>,
    last_downtime: Option<DateTime<Utc>>,
    total_failovers: u32,
    successful_failovers: u32,
    failed_failovers: u32,
    total_pods: u32,
    healthy_pods: u32,
    degraded_pods: u32,
    health_score: f64,
    az_distribution: Vec<String>,
    replication_factor: u8,
    circuit_breaker_state: String,  // CLOSED, OPEN, HALF_OPEN
    last_state_change: DateTime<Utc>,
}
```

### 11.2 FailoverRecord (Persistent — PostgreSQL)

```sql
CREATE TABLE failover_history (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    component_id VARCHAR(128) NOT NULL,
    failover_type VARCHAR(32) NOT NULL,
    -- POD_RESTART, POD_RESCHEDULE, SERVICE_FAILOVER, LEADER_ELECTION
    cause VARCHAR(256) NOT NULL,
    -- HEALTH_CHECK_FAILURE, NODE_FAILURE, OOM_KILL, CRASH_LOOP, MANUAL
    source_instance VARCHAR(128),
    target_instance VARCHAR(128),
    source_node VARCHAR(128),
    target_node VARCHAR(128),
    from_az VARCHAR(32),
    to_az VARCHAR(32),
    detection_time TIMESTAMPTZ NOT NULL,
    failover_start TIMESTAMPTZ NOT NULL,
    failover_end TIMESTAMPTZ,
    duration_ms INTEGER,
    success BOOLEAN NOT NULL,
    data_loss_seconds INTEGER DEFAULT 0,
    error_message TEXT,
    automated BOOLEAN DEFAULT true,
    triggered_by VARCHAR(128),  -- Operator ID or 'SYSTEM'
    created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_failover_component ON failover_history(component_id, created_at DESC);
CREATE INDEX idx_failover_time ON failover_history(detection_time DESC);
CREATE INDEX idx_failover_type ON failover_history(failover_type);
```

### 11.3 HealthSnapshot

```rust
struct HealthSnapshot {
    timestamp: DateTime<Utc>,
    aggregate_level: String,  // POD, COMPONENT, SERVICE, REGION
    entity_id: String,
    health_score: f64,
    status: String,  // HEALTHY, DEGRADED, UNHEALTHY, DOWN
    contributed_check_results: Vec<CheckDetail>,
    failing_checks: Vec<CheckDetail>,
    components: Vec<SubHealthSnapshot>,
    // For aggregate levels
    active_incidents: Vec<String>,
    last_updated: DateTime<Utc>,
}

struct CheckDetail {
    check_type: String,
    target: String,
    result: bool,
    latency_ms: u32,
    error: Option<String>,
    consecutive_failures: u32,
}
```

### 11.4 CircuitBreakerState (In-Memory + etcd)

```rust
struct CircuitBreakerState {
    component_id: String,
    failure_type: String,
    state: CircuitState,  // CLOSED, OPEN, HALF_OPEN
    failure_count: u32,
    last_failure: Option<DateTime<Utc>>,
    open_since: Option<DateTime<Utc>>,
    half_open_time: Option<DateTime<Utc>>,
    recovery_attempts: u32,
    threshold_failure_count: u32,
    threshold_time_window: Duration,
    timeout_open: Duration,   // Time before transitioning to half-open
    timeout_half_open: Duration,  // Time before recovering
}
```

## 12. Execution Flow

### 12.1 Pod Crash Recovery (Single Pod Failure)

```
Phase 1 — DETECTION:
1. Health Check Manager runs liveness probe on pod
2. Liveness probe fails 3 consecutive times
3. Consecutive failure count: 3/3 → status: UNHEALTHY
4. Health Check Manager publishes pod.unhealthy event

Phase 2 — DECISION:
1. Failover Orchestrator receives health event
2. Evaluates: single pod failure, not node failure
3. Decision: let K8s handle (liveness probe restart)
   - Or: if custom restart needed, trigger FailoverOrchestrator

Phase 3 — RECOVERY (K8s Liveness Probe):
1. Kubelet detects liveness failure
2. Kubelet sends SIGTERM to container
3. Container PreStop hook: connection draining (max 30s)
4. SIGKILL after grace period (60s)
5. Kubelet restarts container (backoff: 10s, 20s, 40s, max 300s)
6. Container starts, startup probe passes
7. Liveness probe passes (1 success)
8. Readiness probe passes → pod back in service

Phase 4 — VERIFICATION:
1. Pod Running and Ready
2. No new errors
3. Service has sufficient replicas
4. Metrics: failover duration logged
```

### 12.2 Crash Loop Backoff Recovery (Repeated Pod Failure)

```
Phase 1 — DETECTION:
1. Same pod crashes 3 times within 10 minutes
2. K8s enters CrashLoopBackOff state
3. Failover Orchestrator detects CrashLoop
4. Health Aggregator receives degraded pod event

Phase 2 — DECISION:
1. Evaluate: pod crashes repeatedly, likely not transient
2. Decision: reschedule to a different node
3. Check: anti-affinity to ensure different AZ if possible

Phase 3 — RESCHEDULE:
1. Delete pod (kubectl delete pod)
2. K8s scheduler places pod on different node (anti-affinity)
3. New pod starts with startup check
4. If new node healthy: proceed to verification
5. If CrashLoop continues: try different AZ
6. If all AZs exhausted: manually investigate

Phase 4 — VERIFICATION:
1. New pod Running and Ready
2. No crash recurrence
3. Log error details for investigation
```

### 12.3 Node Failure Recovery

```
Phase 1 — DETECTION:
1. Node conditions: Readiness check fails
2. kubelet does not respond for 1 minute
3. Node controller marks node as NotReady
4. Node lease check: if expired > 1 min, mark node as Dead

Phase 2 — DECISION:
1. Node Unhealthy event published
2. Failover Orchestrator:
   - Check node's pods status
   - Identify pods that need rescheduling
   - Check remaining capacity in other nodes

Phase 3 — RECOVERY:
1. Pods from failed node are evicted
2. If node unrecoverable:
   - kubectl delete node (force removal)
   - Scheduled: pods rescheduled to healthy nodes (or pending)
   - CA: if pending pods, cluster scales up
3. If node transient:
   - Wait for node recovery (max 5 min)
   - If recovered: pod restart on same node
   - If not: force reschedule

Phase 4 — VERIFICATION:
1. All pods rescheduled and Running
2. Remaining nodes have sufficient capacity
3. Node removed from cluster (if replaced)
```

### 12.4 Stateful HA — Raft Leader Re-election

```
Phase 1 — DETECTION:
1. Leader node becomes unresponsive (network partition or crash)
2. Follower nodes detect heartbeat timeout (150ms default)
3. Follower converts to CANDIDATE state

Phase 2 — ELECTION:
1. Candidate increments term number
2. Candidate sends RequestVote RPC to all peers
3. Peers vote for the first candidate that meets criteria:
   - Candidates' log is at least as up-to-date as the peer's
   - Peer has not voted for another candidate in this term
4. Candidate receives majority votes (> n/2)
5. Candidate becomes LEADER

Phase 3 — LEADER DUTIES:
1. New leader starts accepting client requests
2. Leader establishes heartbeat with all followers
3. Leader replicates any pending log entries
4. Leader continues normal operations

Phase 4 — RECOVERY (old leader):
1. Old leader recovers from failure
2. Old leader contacts cluster as FOLLOWER
3. Old leader learns of new term and leader
4. Old leader catches up on missed log entries
5. Follower state achieved, ready for future transitions

Phase 5 — VERIFICATION:
1. New leader established
2. Writes accepted normally
3. Followers synced within expected lag
4. Total election duration logged (< 5s target)
```

### 12.5 Service Degradation Handling

```
1. Degradation Detection:
   - Component Health Score drops below 0.8
   - P99 latency increases > 50% above baseline
   - Error rate > 1% for 5 consecutive minutes

2. Assessment:
   - Which pods are affected?
   - What type of degradation?
   - Is traffic routing causing the issue?

3. Automatic Actions:
   - If latency increase: redistribute traffic (remove slow pods from service)
   - If error rate increase: isolate erroring pods (remove from service)
   - If P99 > 500ms: add more replicas (HA system coordinates with scaling)

4. Degraded Mode:
   - Non-critical operations disabled (analytics, logging)
   - Read-only mode for critical operations
   - Circuit breakers for downstream dependencies

5. Recovery:
   - Once component Health Score > 0.95 for 10 minutes: exit degraded mode
   - Re-enable all features
   - Verify system fully recovered
```

## 13. State Management

### Pod Health State Machine

```
RUNNING ───────────► UNHEALTHY (liveness failure)
   ▲                       │
   │                       ▼
   │                   BACKOFF (kubelet restart delay: 10s-300s)
   │                       │
   │                       ▼
   ├─────────────────── CRASHLOOP (3+ fails)
   │                       │
   │                       ▼
   │                 RESCHEDULING (new node/AZ)
   │                       │
   │                       ▼
   └─────────────────── RUNNING
```

### Raft Consensus State Machine

```
FOLLOWER ──► CANDIDATE (heartbeat timeout)
  ▲               │
  │               ▼
  │              REQUESTING_VOTES
  │               │
  │        ┌──────┴───────┐
  │        │              │
  │        ▼              ▼
  │    LEADER        FOLLOWER (lost election)
  │        │              │
  │        │              └──────► (back to FOLLOWER)
  │        ▼
  │   LEADER (normal operations)
  │        │
  └────────┘ (leader failure → new election)
```

### Circuit Breaker State Machine

```
CLOSED (normal, failures < threshold)
  │
  ├── Failure count >= threshold in time window
  │
  ▼
OPEN (failures return immediately)
  │
  ├── timeout_duration passed
  │
  ▼
HALF_OPEN (test request allowed)
  │
  ├── Test request succeeds ──► CLOSED
  ├── Test request fails ──► OPEN (reset timeout)
```

## 14. Communication Protocols

### Internal AIOS Components

**Health Checks:**
- gRPC: Health/Check RPC (standard gRPC health check protocol)
- HTTP: GET /healthz, /readyz, /startupz endpoints

**Failover Orchestration:**
- gRPC: FailoverService { GetStatus, InitiateFailover, CancelFailover }
- Topic: ha.events (Kafka)

### Stateful Consensus

- **Raft:** etcd client protocol (gRPC)
- **Kafka:** Kafka protocol (TCP, binary)
- **Redis:** Redis Sentinel protocol (TCP)
- **CockroachDB:** PostgreSQL protocol + internal Raft

### Service Mesh

**Istio Control Plane:** xDS (Discovery Service) via gRPC stream
**Envoy:** xDS API (static or aggregated)

### Infrastructure

- **Kubernetes API:** HTTPS with TLS 1.3
- **Kubelet API:** HTTPS with TLS 1.3
- **Node Exporter:** Protocol buffers via gRPC

## 15. APIs

### gRPC Service Definitions

```protobuf
service HealthService {
    rpc GetHealth(HealthRequest) returns (HealthResponse);
    rpc GetComponentHealth(ComponentHealthRequest) returns (ComponentHealthResponse);
    rpc SetComponentHealth(SetHealthRequest) returns (SetHealthResponse);
    rpc WatchHealth(HealthWatchRequest) returns (stream HealthEvent);
}

service FailoverService {
    rpc GetFailoverStatus(FailoverStatusRequest) returns (FailoverStatusResponse);
    rpc InitiateFailover(FailoverRequest) returns (FailoverResponse);
    rpc CancelFailover(CancelFailoverRequest) returns (CancelFailoverResponse);
    rpc SetCircuitBreaker(CircuitBreakerRequest) returns (CircuitBreakerResponse);
}
```

### REST API

```
GET    /v1/ha/status                   — Overall HA status
GET    /v1/ha/components               — All component health
GET    /v1/ha/components/:id        — Single component health
POST   /v1/ha/failover              — Manual failover
GET    /v1/ha/failover/history       — Failover history
GET    /v1/ha/failover/active       — Active failover
POST   /v1/ha/circuitbreaker/:id    — Update circuit breaker
GET    /v1/ha/metrics                — HA metrics
```

## 16. Events

| Event | Payload |
|-------|---------|
| ha.pod.health.changed | {pod_id, component, old_status, new_status} |
| ha.service.health.changed | {service_id, old_score, new_score, threshold} |
| ha.failover.started | {failover_id, component, type, cause} |
| ha.failover.completed | {failover_id, duration, success, data_loss} |
| ha.failover.failed | {failover_id, error, rollback_needed} |
| ha.leader.elected | {cluster_id, leader_id, term, duration} |
| ha.drain.completed | {pod_id, protocol, active_connections, duration} |
| ha.drain.failed | {pod_id, reason, remaining_connections} |
| ha.circuit_breaker.changed | {component, state: OPEN|CLOSED|HALF_OPEN} |

## 17. Queues

**Kafka Topics:**
- ha.events (8 partitions, 7d retention): General HA events
- ha.events.critical (4 partitions, 30d retention): Failover events
- ha.health.snapshots (4 partitions, 24h retention): Periodic health snapshots
- ha.leader.elections (4 partitions, 30d retention): Leader election events

## 18. Caching

**Health Cache (Redis):**
- All component health status
- TTL: 10s, Capacity: 100MB
- Key: health:{component_id}

**Pod Status Cache (Local):**
- Per-node pod status map
- TTL: 5s, Capacity: 50MB per node

## 19. Memory Budget

| Component | Memory | Notes |
|-----------|--------|-------|
| Health Manager | 256MB | Pod health states, check results |
| Failover Orchestrator | 512MB | Failover state, active failovers |
| Connection Drain Manager | 128MB per pod | Active connection tracking |
| Stateful HA Manager | 256MB | Raft state, cluster config |
| Health Aggregator | 256MB | Aggregated health data |
| Chaos Controller | 512MB | Test definitions, history |

## 20. Persistence

**PostgreSQL:** Failover history, availability metrics, HA configuration, health check history
**etcd:** Circuit breaker states, leader election state, health configuration

## 21. Validation

**Health Check Validation:**
- Check type must be valid (HTTP, gRPC, TCP, exec, custom)
- Interval must be >= timeout + network latency (avoid false positives)
- Thresholds must be >= 1
- Startup check must complete before liveness/readiness

**Failover Validation:**
- Component must exist in registry
- Failover not already in progress for component
- Sufficient capacity exists in target nodes/AZs
- Circuit breaker state allows failover

## 22. Retry Logic

| Operation | Max Retries | Backoff | Max Delay |
|-----------|-------------|---------|-----------|
| Pod restart | 3 | Exponential | 300s |
| Pod reschedule | 3 per AZ | Linear | 60s |
| Leader election | 5 (across terms) | Linear | 50ms |
| Connection drain | 2 | Fixed | 15s |
| Health check (failed) | Until recovery | N/A (periodic) | N/A |

## 23. Error Recovery

**Pod Down (single):** K8s liveness probe restart → backoff (10s-300s) → up to 3 attempts → then CrashLoopBackoff → reschedule

**Node Down:** Pods evicted → rescheduled to remaining nodes → if insufficient capacity → cluster autoscaling

**etcd Leader Failure:** Follower detects heartbeat timeout → election → new leader within 5s → old leader rejoins as follower

**Network Partition:** Split brain detection → minority partition stops serving writes → majority partition continues → when partition heals, minority catches up

## 24. Security

**Communication:** All health check and failover communication uses mTLS via Istio
**AuthZ:** Only operators with 'ha.operator' role can trigger manual failover
**Audit:** All failover actions (manual and automatic) logged with timestamps and operator ID

## 25. Authentication

**Internal:** mTLS with SPIFFE/SPIRE, service accounts
**Operator:** OIDC with MFA for manual failover commands

## 26. Authorization

| Role | Permissions |
|------|------------|
| ha.admin | All HA operations including manual failover |
| ha.operator | View health, approve auto-failover |
| ha.viewer | Read-only: health status, failover history |

## 27. Permissions

ha:status:read, ha:failover:initiate, ha:failover:cancel, ha:health:configure, ha:circuit_breaker:update

## 28. Monitoring

**SLIs:**
| SLI | Target | Method |
|-----|--------|--------|
| Health Check Latency | P99 < 5s | Check duration |
| Failure Detection Time | P99 < 15s | Health check to detection |
| Failover Duration (stateless) | P99 < 30s | Event timing |
| Failover Duration (stateful) | P99 < 120s | Event timing |
| Leader Election Duration | P99 < 5s | Raft metric |
| Drain Success Rate | > 99.5% | Drain events |
| Availability (critical) | 99.999% | Uptime monitor |

**Dashboards:**
1. Component Health: All components health score, failing checks, active incidents
2. Failover Activity: Recent failover events, duration, cause distribution
3. Circuit Breakers: Active breaker states, failure rates
4. Raft Consensus: Leader term, election count, log replication lag

## 29. Logging

Structured JSON: {timestamp, level, component, ha_type, event, target, duration_ms, result}
ERROR: Failover failure, circuit breaker open, leader loss
WARN: Health check threshold, drain timeout, crash loop detection
INFO: Failover completion, leader elected, health change

## 30. Metrics

**Counters:** failover_total{type,result}, health_check_total{result}, drain_total{protocol,result}, circuit_breaker_open_total
**Histograms:** failover_duration_seconds{type}, health_check_latency_seconds{type}, leader_election_duration_seconds, drain_duration_seconds{protocol}
**Gauges:** health_score{component}, availability_percent{component}, active_failovers, circuit_breaker_state{component}

## 31. Tracing

OpenTelemetry with 100% sampling for failover events, 10% for health checks.
Spans: health.check.execute, failover.initiate, failover.execute, drain.connections

## 32. Scalability

| Component | Strategy | Capacity |
|----------|----------|---------|
| Health Manager | Horizontal (3-5) | 1000+ components |
| Failover Orchestrator | Active-active (3-5) | 100 failovers/min |
| Connection Drain Manager | Sidecar per pod | Per connection limits |
| Stateful HA Manager | Per consensus group | 1000+ groups |
| Health Aggregator | Horizontal (3) | 10000+ pods |

## 33. Performance

| Operation | Target |
|-----------|--------|
| Health check interval | 5s |
| Failure detection | < 15s |
| Pod restart (stateless) | < 10s |
| Pod reschedule (stateful) | < 120s |
| Leader election | < 5s |
| Connection drain (HTTP) | < 30s |
| Circuit breaker recovery | 5-60s |

## 34. Optimization

**Health Check Optimization:**
- Cache health results locally for 5s
- Batch health queries per component
- Timeout ensure no long-lived probes

**Failover Optimization:**
- Parallel pod reschedule per AZ
- Pre-warm standby nodes for critical components
- Leader pre-vote to avoid unnecessary elections

## 35. Testing Strategy

**Unit:** 95%+ for state machines, decision logic, health scoring
**Integration:** Pod crash recovery, leader election, drain testing
**E2E:** Complete failover scenarios, AZ failure, multi-component cascade
**Chaos:** Daily pod kill, weekly AZ failure, monthly full chaos day, quarterly region failover
**Performance:** 1000 simultaneous pod failures, 100 leader elections/min

## 36. Risk Analysis

| Risk | Probability | Impact | Mitigation |
|------|-----------|--------|-----------|
| False positive health check | Medium | Medium | Hysteresis, multiple probes, success threshold |
| Split brain (Raft) | Low | Critical | PreVote, majority decision |
| Cascading failover | Low | High | Circuit breaker, rate limit failover attempts |
| Slow failover detection | Low | Medium | Reduced health check intervals, faster probes |
| Drain failure | Medium | Low | Force kill after timeout, logging for investigation |
| Circuit breaker open too long | Low | Medium | Configurable timeout, monitoring, alert |

## 37. Failure Scenarios

**Scenario 1: Pod Crash Loop**
Detection: 3 crashes in 10 min → CrashLoopBackOff
Action: Reschedule to new node, if continues → different AZ, if exhausted → escalate

**Scenario 2: Node Failure**
Detection: Node NotReady for 1 min → kubelet unreachable
Action: Evict pods, reschedule, if stuck → force delete node, replace

**Scenario 3: AZ Failure**
Detection: all nodes in 1 AZ unhealthy for > 2 min
Action: Distribute pods from failed AZ to remaining AZs, if capacity insufficient → scaling trigger

**Scenario 4: Raft Leader Loss**
Detection: Heartbeat timeout 150ms → election starts
Action: New leader elected, catch up logs, clients reconnect

**Scenario 5: Network Partition**
Detection: Partition between 2 etcd members
Impact: Minority partition (1 node) stops writes
Recovery: When partition heals, minority catches up, full functionality restored

## 38. Recovery Procedures

**Post-Failover Verification:**
1. Check all affected pods Running and Ready
2. Verify health scores back to normal
3. Verify traffic flowing correctly to all replicas
4. Check for any error rate increase
5. If failover-related: verify data consistency
6. Document failover details

**Rollback Manual Failover:**
1. Revert to original configuration
2. Re-enable original pods/instances
3. Verify health returns to pre-failover state
4. Continue monitoring for 30 min

## 39. Operational Limits

- Max failover attempts per component per hour: 10
- Max concurrent failover operations: 5 per cluster
- Health check timeout: max 30s
- Drain timeout: max 120s
- Circuit breaker open timeout: min 30s, max 300s
- Leader election timeout: min 50ms, max 500ms

## 40. Maintenance

**Daily:** Review health check logs, verify failover history
**Weekly:** Circuit breaker state review, health check threshold tuning
**Monthly:** Failover simulation testing, drain policy review
**Quarterly:** Full chaos engineering day, availability target review, HA runbook update

## 41. Future Improvements

- AI-driven health anomaly detection (predictive failure detection)
- Automated recovery optimization (RL-optimized failover strategies)
- Multi-region active-active deployment with consistent HA model
- Self-tuning health check thresholds based on historical patterns

## 42. Examples

**Pod Crash Recovery:** Pod aios-kernel-3 becomes unhealthy → kubelet restart → backoff 10s → pod restarts, Ready in 25s → total downtime 35s

**Leader Election:** etcd-1 loses network → etcd-2 detects 150ms timeout → etcd-2 requests votes → becomes leader in 3s → old leader rejoins as follower

**Node Failure:** i-abc123 becomes NotReady → pods evicted (5 pods) → rescheduled to 3 nodes → total from detection to all pods Running: 90s

## 43. Acceptance Criteria

1. Single pod failure recovers within 30s (stateless) or 120s (stateful)
2. Node failure recovers within 5 min
3. AZ failure does not cause data loss
4. Raft leader election completes within 5s
5. Connection draining completes within 30s for HTTP/gRPC
6. Circuit breaker prevents cascading failures
7. No single component failure causes full platform outage
8. Chaos testing passes without data loss
9. Health score accurately reflects system state
10. Failover operations leave system in consistent state

## 44. Definition of Done

1. All ACs verified through testing
2. HA design review completed
3. Health checks configured for all components
4. Failover procedures documented and tested
5. Circuit breaker thresholds defined
6. Monitoring dashboards deployed
7. Alert rules configured
8. Runbook published
9. Chaos test schedule established
10. Security review completed