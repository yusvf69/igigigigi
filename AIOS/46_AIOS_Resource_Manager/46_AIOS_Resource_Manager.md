# AIOS Resource Manager — Engineering Specification

## 1. Purpose

The AIOS Resource Manager provides comprehensive resource management, allocation, accounting, and governance across all compute, memory, storage, network, and GPU resources within the AIOS platform. It tracks resource types including CPU cores and shares, RAM, GPU compute and memory, storage IOPS and capacity, network bandwidth and connections, and specialized hardware accelerators. The system manages allocation per agent, service, team, and department with configurable quotas (hard limits, soft limits, burst allowances), reservations for critical agents with guaranteed resources, and real-time plus historical tracking for accounting and chargeback. It implements oversubscription ratios between 1.2x and 1.5x for efficient resource utilization, pressure handling through OOM killer integration and CPU throttling, resource negotiation between agents and the scheduler, and defragmentation for optimal resource packing. The system exposes comprehensive metrics using the USE methodology (utilization, saturation, errors) across all resource types and provides the foundation for capacity planning, cost allocation, and performance optimization.

## 2. Mission

To deliver a real-time, deterministic, and observable resource management platform that maximizes resource utilization while ensuring fairness, isolation, and predictable performance across all AIOS workloads. The system shall track resource allocation and consumption at sub-second granularity across 10,000+ concurrent agents, enforce hard and soft quotas with under 10ms enforcement latency, maintain oversubscription ratios within configured bounds through dynamic rebalancing, and provide complete resource accounting for chargeback with 99.9% accuracy. The mission includes achieving 80%+ average resource utilization across the fleet while maintaining SLO compliance for critical agents, enabling burst capacity for elastic workloads without impacting guaranteed reservations, and providing predictive resource pressure detection to prevent OOM and throttling events before they occur.

## 3. Responsibilities

### 3.1 Resource Type Management
- **CPU**: Track CPU cores (physical and virtual), CPU shares/weights for fair scheduling, CPU pinning for latency-sensitive workloads, CPU sets for NUMA-aware allocation, and CPU quotas for cgroup-based enforcement. Support hyperthreading awareness and CPU architecture variations (x86, ARM, GPU-integrated).
- **RAM**: Track total memory, available memory, memory allocation per agent/service, memory limits with soft/hard boundaries, swap usage, huge pages, and memory NUMA node affinity. Integrate with OOM killer for enforcement.
- **GPU**: Track GPU compute units (CUDA cores, tensor cores), GPU memory (VRAM), GPU utilization, GPU memory bandwidth, and GPU temperature/power. Support GPU partitioning (MIG, vGPU) and GPU time-slicing.
- **Storage IOPS**: Track storage input/output operations per second, read/write mix, latency percentiles, queue depth, and throughput (MB/s). Support class-based storage allocation (SSD, HDD, NVMe, network storage).
- **Storage Capacity**: Track provisioned capacity, used capacity, snapshot overhead, replication factor overhead, and deduplication/compression ratios. Support thin provisioning and overcommitment.
- **Network Bandwidth**: Track inbound and outbound bandwidth per interface, per connection, and per flow. Track packet rate, connection count, connection establishment rate, and network latency. Support bandwidth QoS and traffic shaping.
- **Specialized Hardware**: Track FPGA utilization, TPU cores, ASIC accelerators, and other domain-specific compute resources. Provide extensible resource type framework for future hardware.

### 3.2 Resource Allocation
- **Per-Agent Allocation**: Allocate resource bundles to individual agent instances based on agent type, configured resource profile, and availability. Support dynamic allocation changes during agent lifecycle (scale up/down).
- **Per-Service Allocation**: Aggregate allocation across all instances of a service. Support service-level resource guarantees and limits. Enable burst allocation for services with elastic demand.
- **Per-Team/Department Allocation**: Organizational resource budgeting with hierarchical allocation. Team-level quotas enforced across all services and agents owned by the team. Department-level caps prevent organizational overconsumption.
- **Allocation Strategies**: Bin-packing (optimize for utilization), spread (optimize for availability), topology-aware (optimize for NUMA/rack locality), and cost-aware (optimize for resource cost).
- **Allocation Lifecycle**: Request, approve, allocate, monitor, reclaim, release. Support preemption for lower-priority allocations when higher-priority workloads require resources.

### 3.3 Quota Management
- **Hard Limits**: Absolute maximum resource consumption. Enforcement via cgroups, resource quotas, and scheduler rejections. Violations result in OOM kill (memory), CPU throttling, IOPS throttling, or request rejection.
- **Soft Limits**: Target resource consumption with allowance for temporary bursts. Exceeding soft limit triggers warning but not enforcement. Used for capacity planning and efficiency monitoring.
- **Burst Allowances**: Configurable burst capacity above soft limits for time-limited spikes. Burst duration and magnitude configurable. Burst credits accumulate during idle periods (similar to CPU burst in AWS).
- **Quota Hierarchy**: Organization > Department > Team > Service > Agent. Child quotas cannot exceed parent quotas. Free (unused) quota can be temporarily borrowed from parent if available.

### 3.4 Resource Reservations
- **Guaranteed Reservations**: Resources reserved for critical agents with hard guarantee. Reserved resources are not available for oversubscription. Used for latency-sensitive, production-critical, and real-time workloads.
- **Reservation Lifecycle**: Create (specify resource profile, duration, priority), approve (validate availability), activate (allocate and pin), modify (adjust resource profile), release (return resources to pool).
- **Reservation Prioritization**: Priority levels (CRITICAL, HIGH, MEDIUM, LOW, BEST_EFFORT). Higher-priority reservations preempt lower-priority allocations when resources are constrained.

### 3.5 Real-Time and Historical Tracking
- **Real-Time Tracking**: Sub-second resource consumption metrics via cgroup stats, /proc filesystem, GPU driver APIs, and network interface counters. Pushed to in-memory time-series buffer for immediate query.
- **Historical Tracking**: Aggregated resource metrics stored in time-series database (Prometheus/TimescaleDB) with configurable retention. Aggregation levels: raw (30 days), 1-minute (90 days), 1-hour (365 days), 1-day (7 years).
- **Accounting Records**: Per-resource, per-owner (agent/service/team) accounting records with start/end time, peak/avg utilization, and cumulative consumption. Used for chargeback and capacity planning.

### 3.6 Chargeback and Showback
- **Resource Costing**: Assign cost per resource unit based on procurement cost, operational overhead, and markup. Cost per CPU core-hour, GB-RAM-hour, GPU-hour, IOPS-GB-month, GB-month, Mbps-month.
- **Usage Accounting**: Accrue resource usage costs in real-time with daily snapshots. Support retroactive cost adjustments. Generate cost allocation reports per organizational unit.
- **Showback Dashboards**: Visibility into resource consumption and imputed cost without financial charge. Department/team-level breakdown with trend analysis and peer comparison.

### 3.7 Oversubscription Management
- **Oversubscription Ratio**: Configurable ratio (default 1.2x - 1.5x) defining how much total allocation can exceed physical capacity. Based on historical utilization patterns and workload profiling.
- **Ratio Calculation**: totalAllocated / totalPhysical. Monitored continuously with alerts if ratio exceeds configured maximum. Auto-reduction of ratio when utilization patterns change.
- **Oversubscription Types**: CPU (commonly 1.5x-3x based on typical utilization), memory (1.2x-1.5x with compress/swap), storage (2x-5x with thin provisioning), network (1x-2x based on traffic patterns).
- **Risk Mitigation**: Oversubscription backed by reclaimable resources (preemptible workloads), swap/compress for memory, and QoS throttling for CPU and network.

### 3.8 Resource Pressure Handling
- **OOM Killer Integration**: Monitor memory pressure indicators (PSI: Pressure Stall Information). Configure OOM priority per agent/container. OOM notification with agent/container metadata for debugging. Post-OOM recovery and automatic restart for non-critical agents.
- **CPU Throttling**: Cgroup CPU quota enforcement with configurable period (default 100ms). Throttling metrics: throttled time, throttled count, throttle ratio. Proportional throttling for soft limit exceedance.
- **IOPS Throttling**: Block IO cgroup controller for IOPS and bandwidth limits. Throttling tiers: guaranteed, burst, and limited. IO priority for latency-sensitive vs. throughput workloads.
- **Network Throttling**: Traffic shaping via tc (traffic control) with rate limits and burst allowances. DSCP marking for QoS differentiation. Per-connection and per-flow rate limiting.
- **Backpressure Signaling**: When resources near exhaustion, signal agents to reduce consumption via backpressure API. Graceful degradation before enforcement kicks in.

### 3.9 Resource Negotiation
- **Agent Request**: Agent specifies resource requirements (CPU, memory, GPU, etc.) with min/desired/max profiles. Includes resource constraints (NUMA affinity, GPU type, storage class).
- **Scheduler Decision**: Scheduler evaluates request against available resources, policies, and priorities. Returns allocation decision with granted resources, placement, and any constraints.
- **Dynamic Negotiation**: Agents can request resource adjustments during runtime. Scale-up/scale-down within configured limits. Prioritized based on agent criticality and current resource pressure.
- **Bid-based Allocation (Optional)**: For advanced scenarios, agents bid for scarce resources with virtual currency. Highest bidder gets allocation within governance limits.

### 3.10 Resource Defragmentation
- **Fragmentation Detection**: Identify resource fragmentation across the cluster (CPU/memory fragmentation, storage fragmentation, NUMA fragmentation). Score fragmentation severity per node and cluster-wide.
- **Active Defragmentation**: Live migration of agents/containers to consolidate resources. Defragmentation scheduling during low-utilization periods. Impact assessment before migration (resource overhead, latency impact).
- **Preventive Measures**: Topology-aware initial placement to minimize fragmentation. Pinning and affinity rules to maintain packing efficiency. Regular reevaluation of placement optimality.

### 3.11 USE Method Metrics
- **Utilization**: Percentage of resource being used over a time interval. Per resource type, per node, per agent. Average, peak, and percentile metrics.
- **Saturation**: Degree of resource contention measured by queue depth or waiting time. Run queue length (CPU), swap usage (memory), IO queue depth (storage), connection backlog (network).
- **Errors**: Count and rate of resource-related errors. OOM events, CPU throttling events, IO errors, network drops and retransmits, GPU errors (XID).

## 4. Non-Responsibilities

- The Resource Manager does NOT manage application-level performance tuning or configuration.
- It does NOT handle resource pricing or financial billing (use AIOS Cost Manager).
- It does NOT manage deployment scheduling or container orchestration (use AIOS Orchestrator).
- It does NOT provide resource forecasting or capacity planning analytics (use AIOS Capacity Planner).
- It does NOT manage power consumption or cooling infrastructure.
- It does NOT replace the operating system kernel resource management (cgroups, scheduler).
- It does NOT manage external cloud provider resource provisioning (use AIOS Cloud Controller).

## 5. Architecture

The AIOS Resource Manager is organized as a distributed resource management platform with a centralized Resource Management Service (RMS) coordinating with per-node Resource Agents. The RMS handles global resource accounting, quota management, reservation management, allocation decisions, and policy enforcement. Per-node Resource Agents collect real-time metrics, enforce local limits via cgroups and kernel interfaces, and report utilization telemetry. The Resource Scheduler makes allocation and placement decisions based on global state, policies, and optimization goals. The Accounting Engine tracks historical usage for chargeback and planning. The Monitoring Pipeline collects, aggregates, and stores USE method metrics for real-time dashboards and alerting.

## 6. Components

### 6.1 Resource Management Service (RMS)
Central coordinator that maintains global resource state, handles allocation requests, enforces quotas, manages reservations, and oversees oversubscription. Provides REST and gRPC APIs for resource management operations. Maintains in-memory resource topology with real-time updates from node agents.

### 6.2 Per-Node Resource Agent
Runs on each compute node, collects resource metrics via cgroups, /proc, GPU drivers, and network interfaces. Enforces local resource limits via cgroup controllers, traffic control, and OOM configuration. Reports utilization, saturation, and error metrics to RMS at configurable intervals (default 1 second).

### 6.3 Resource Scheduler
Makes placement and allocation decisions using global resource state. Supports multiple scheduling strategies (bin-packing, spread, topology-aware, cost-aware). Handles initial placement, dynamic scaling, and defragmentation migrations. Evaluates allocation requests against policies, quotas, and available capacity.

### 6.4 Quota Manager
Manages quota definitions, enforcement, and hierarchy. Evaluates quota consumption on each allocation request. Generates quota warning and violation alerts. Supports quota borrowing and quota transfer between teams.

### 6.5 Reservation Manager
Manages resource reservation lifecycle. Guarantees resource availability for reserved workloads. Coordinates with scheduler to reserve capacity and prevent oversubscription of reserved resources. Handles reservation expiration and renewal.

### 6.6 Accounting Engine
Tracks resource usage for chargeback and capacity planning. Accumulates per-owner, per-resource consumption counters. Generates daily usage snapshots and monthly billing reports. Provides cost allocation data to AIOS Cost Manager.

### 6.7 Defragmentation Controller
Monitors resource fragmentation across the cluster. Triggers defragmentation when fragmentation score exceeds threshold (default 0.3). Coordinates live migrations to consolidate resources. Assesses migration impact and schedules during low-utilization periods.

### 6.8 Pressure Monitor
Monitors resource pressure signals (PSI, OOM events, throttling). Generates pressure alerts and triggers backpressure signaling. Coordinates with scheduler to preempt low-priority workloads under severe pressure.

### 6.9 Monitoring Pipeline
Collects metrics from all node agents, aggregates, and stores in time-series database. Provides query API for real-time and historical resource data. Generates USE method dashboards and alerting rules.

## 7. Dependencies

### Internal Dependencies
- AIOS Configuration System for resource policies, quotas, and scheduling strategies.
- AIOS Service Discovery for node registration and health tracking.
- AIOS Metrics System for resource metric storage and dashboarding.
- AIOS Logging System for structured operational and audit logging.
- AIOS Tracing System for resource allocation trace spans.
- AIOS Cost Manager for resource cost data and chargeback integration.
- AIOS Orchestrator for agent/container lifecycle and migration coordination.

### External Dependencies
- Linux cgroups v2 for CPU, memory, IOPS resource enforcement.
- Linux traffic control (tc) for network bandwidth enforcement.
- NVIDIA GPU driver and NVML for GPU monitoring and management.
- Prometheus Node Exporter for node-level hardware metrics.
- Time-series database (Prometheus, VictoriaMetrics, TimescaleDB) for historical metrics.
- Kernel PSI (Pressure Stall Information) for resource pressure monitoring.

## 8. Inputs/Outputs

### Inputs
- Resource Allocation Request: agent/service ID, resource profile (CPU, RAM, GPU, etc.), constraints, priority.
- Resource Release Notification: agent/service ID, resources to release.
- Quota Definition: scope (team/service/agent), resource type, hard/soft limits, burst configuration.
- Reservation Request: resource profile, duration, priority, owner.
- Resource Profile Update: new resource requirements for running agent/service.
- Scheduling Strategy Configuration: strategy type, weights, constraints.
- Oversubscription Ratio Configuration: per-resource-type ratio settings.
- Defragmentation Trigger: manual or automatic threshold-based trigger.

### Outputs
- Allocation Decision: granted resources, placement node, enforced limits.
- Quota Status: current consumption, limit, remaining, violation status.
- Resource Reservation: reservation ID, guaranteed resources, expiration.
- Resource Utilization Report: per-owner, per-resource utilization with trends.
- Pressure Alert: resource type, severity, affected entities, suggested actions.
- Defragmentation Plan: migrations list with impact assessment and schedule.
- USE Metrics: utilization, saturation, errors per resource type per node.

## 9. Data Structures

### ResourceProfile
CPU cores and shares, RAM amount, GPU compute units and memory, storage IOPS and capacity, network bandwidth and connections. Min/desired/max values for elastic workloads. NUMA affinity, GPU type preference, storage class requirement.

### Allocation
Allocation ID, owner (agent/service/team), resource profile, granted resources, placement node, cgroup paths, enforcement limits, start time, expected duration, priority, status (PENDING, ACTIVE, PREEMPTED, RELEASED).

### Quota
Quota ID, scope (department/team/service/agent), resource type, hard limit, soft limit, burst allowance, burst duration, current consumption, peak consumption, last reset timestamp, hierarchy level.

### Reservation
Reservation ID, owner, resource profile, start time, end time, guarantee level, priority, status (PENDING, ACTIVE, EXPIRED, RELEASED), cost (if applicable).

### OversubscriptionConfig
Resource type, target ratio, current ratio, max ratio, reclaim strategy (PREEMPT, COMPRESS, SWAP, THROTTLE), utilization baseline window.

### PressureEvent
Event ID, resource type, pressure type (OOM, THROTTLE, PSI), severity (LOW, MEDIUM, HIGH, CRITICAL), affected entity, timestamp, duration, action taken (NONE, PREEMPT, KILL, THROTTLE, NOTIFY).

### FragmentationMetrics
Node ID, resource type, fragmentation score (0-1), fragment count, largest free block, recommended action.

### USE_Metrics
Node ID, resource type, utilization (avg, peak, P95), saturation (queue depth, wait time), errors (count, rate), timestamp.

### ResourceTopology
Nodes with CPU topology (sockets, cores, threads, NUMA nodes), memory topology (NUMA node memory size and type), GPU topology (GPU index, NVLink connectivity), storage topology (device type, mount points, IOPS capacity), network topology (interfaces, bandwidth, latency).

## 10. Execution Flow

### 10.1 Resource Allocation Flow
1. Agent/service requests allocation via RMS API with resource profile and constraints.
2. Scheduler evaluates request against global resource state, available capacity, quotas, and reservations.
3. Quota Manager checks owner quota: if hard limit exceeded, reject with QuotaExceededError.
4. Scheduler selects target node using configured strategy (bin-packing, spread, topology-aware).
5. Node Resource Agent validates local capacity and configures cgroups with resource limits.
6. RMS records allocation in global state with status ACTIVE.
7. Accounting Engine starts tracking resource consumption for this allocation.
8. Response returned with allocation ID, node, and resource limits.

### 10.2 Quota Enforcement Flow
1. Quota Manager monitors consumption for all quota scopes in real-time.
2. On each allocation request, evaluate hard limit: if exceeded, reject.
3. On each allocation request, evaluate soft limit: if exceeded, emit warning and allow burst.
4. Burst tracking: consume burst credits, emit BurstExhaustedAlert when depleted.
5. Periodic reconciliation: verify quota consumption against actual resource usage.
6. Quota violation events emitted with scope, resource type, and current consumption.

### 10.3 Resource Pressure Response Flow
1. Pressure Monitor detects pressure signal (PSI > threshold, OOM event, throttling).
2. Evaluate pressure severity based on resource type and pressure level.
3. If backpressure enabled, signal consuming agents to reduce usage via backpressure API.
4. If pressure exceeds backpressure window, escalate to scheduler for preemption.
5. Scheduler selects lowest-priority allocations on the affected node for preemption.
6. Preempted allocation notified, resources reclaimed, allocated to higher-priority workloads.
7. Pressure event logged with severity, affected entities, and actions taken.

### 10.4 Defragmentation Flow
1. Defragmentation Controller evaluates cluster fragmentation score on schedule (default every 5 minutes).
2. If score exceeds threshold (default 0.3), generate defragmentation plan.
3. Plan identifies source and destination nodes for migrations to consolidate resources.
4. Impact assessment: estimate migration cost (network transfer, downtime, resource overhead).
5. Schedule defragmentation during low-utilization window or coordinate with scheduler.
6. Execute migrations: live migrate agents/containers from source to destination nodes.
7. Validate post-migration: confirm resource release on source and allocation on destination.
8. Update fragmentation score and continue monitoring.

## 11. State Management

### 11.1 Global Resource State (RMS In-Memory)
Current allocations, available capacity, reservations, quotas, node health, and resource topology. Replicated across RMS instances for high availability. State recovery from persistent store on restart.

### 11.2 Per-Node State (Node Agent)
Local allocations, cgroup configurations, resource utilization counters, and enforcement status. Reported to RMS on configurable interval (1 second) and on state changes.

### 11.3 Persistent State (PostgreSQL)
Allocation records, quota definitions and consumption history, reservation records, accounting records, and configuration. Provides durable storage for restart recovery and historical queries.

### 11.4 Time-Series State (Prometheus/VictoriaMetrics)
Real-time and historical USE metrics with configurable retention. Used for dashboards, alerting, and capacity planning queries.

## 12. Communication

### 12.1 APIs

#### Resource Allocation API (gRPC)
```
service ResourceAllocationService {
  rpc Allocate(AllocateRequest) returns (AllocateResponse);
  rpc Release(ReleaseRequest) returns (ReleaseResponse);
  rpc UpdateAllocation(UpdateAllocationRequest) returns (UpdateAllocationResponse);
  rpc GetAllocation(GetAllocationRequest) returns (Allocation);
  rpc ListAllocations(ListAllocationsRequest) returns (ListAllocationsResponse);
  rpc QueryResources(QueryResourcesRequest) returns (ResourceAvailability);
}
```

#### Quota API (RESTful)
```
POST /v1/quotas (create quota)
GET /v1/quotas/{id}
PUT /v1/quotas/{id} (update quota)
DELETE /v1/quotas/{id}
GET /v1/quotas/{id}/consumption
GET /v1/quotas?scope=team:engineering
POST /v1/quotas/{id}/borrow
```

#### Reservation API (RESTful)
```
POST /v1/reservations (create reservation)
GET /v1/reservations/{id}
PUT /v1/reservations/{id} (update)
DELETE /v1/reservations/{id}
GET /v1/reservations/availability
```

#### Metrics API (RESTful)
```
GET /v1/metrics/use?node=all&resource=CPU
GET /v1/metrics/topology?node=node-1
GET /v1/metrics/pressure?severity=HIGH
GET /v1/metrics/fragmentation
```

### 12.2 Events Emitted

| Event | Payload | Emitter | Consumer |
|-------|---------|---------|----------|
| resource.allocation.created | allocationId, owner, resources, node | RMS | Accounting, Monitoring |
| resource.allocation.released | allocationId, owner, resources, node | RMS | Accounting, Monitoring |
| resource.quota.violation | quotaId, scope, resourceType, current, limit | Quota Manager | Alerting, Notification |
| resource.quota.warning | quotaId, scope, resourceType, current, limit (soft) | Quota Manager | Notification |
| resource.pressure.high | nodeId, resourceType, pressureLevel, affectedAllocations | Pressure Monitor | Scheduler, Alerting |
| resource.pressure.oom | nodeId, killedAllocation, reason | Pressure Monitor | Incident, Alerting |
| resource.preemption.started | allocationId, priority, reason | Scheduler | Agent, Notification |
| resource.preemption.completed | allocationId, success, resourcesReclaimed | Scheduler | RMS, Accounting |
| resource.defragmentation.plan | planId, migrations, impact | Defrag Controller | Dashboard |
| resource.node.down | nodeId, lastHeartbeat, allocatedCount | RMS | Alerting, Scheduler |

## 13. Memory

### 13.1 RMS In-Memory State
Global resource topology and allocation state: ~1GB for 10,000 nodes with 100,000 allocations. Resource topology cache: ~100MB. Quota state: ~50MB for 1,000 quotas. Reservation state: ~50MB for 10,000 reservations.

### 13.2 Node Agent Memory
Cgroup stats and metric buffers: ~50MB per node. Metric reporting buffers: ~25MB. OOM and pressure event buffers: ~10MB.

### 13.3 Time-Series Database
Raw metrics retention: 30 days at 15-second scrape interval, ~10GB per 1000 nodes. Aggregated metrics retention: 1 hour, 1 day, 1 week resolutions for extended retention.

## 14. Persistence

### 14.1 PostgreSQL Schema

```sql
CREATE TABLE allocations (
  id UUID PRIMARY KEY,
  owner_type VARCHAR(64) NOT NULL,
  owner_id VARCHAR(256) NOT NULL,
  resource_profile JSONB NOT NULL,
  granted_resources JSONB NOT NULL,
  node_id VARCHAR(256) NOT NULL,
  cgroup_path TEXT,
  priority INT NOT NULL DEFAULT 0,
  status VARCHAR(32) NOT NULL DEFAULT 'ACTIVE',
  started_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  expected_duration INTERVAL,
  released_at TIMESTAMPTZ,
  metadata JSONB
);

CREATE TABLE quotas (
  id UUID PRIMARY KEY,
  scope_type VARCHAR(64) NOT NULL,
  scope_id VARCHAR(256) NOT NULL,
  resource_type VARCHAR(64) NOT NULL,
  hard_limit DOUBLE PRECISION NOT NULL,
  soft_limit DOUBLE PRECISION,
  burst_allowance DOUBLE PRECISION,
  burst_duration INTERVAL,
  consumption DOUBLE PRECISION NOT NULL DEFAULT 0,
  peak_consumption DOUBLE PRECISION NOT NULL DEFAULT 0,
  created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  updated_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  UNIQUE(scope_type, scope_id, resource_type)
);

CREATE TABLE reservations (
  id UUID PRIMARY KEY,
  owner_id VARCHAR(256) NOT NULL,
  resource_profile JSONB NOT NULL,
  start_time TIMESTAMPTZ NOT NULL,
  end_time TIMESTAMPTZ NOT NULL,
  guarantee_level VARCHAR(32) NOT NULL DEFAULT 'HARD',
  priority INT NOT NULL DEFAULT 0,
  status VARCHAR(32) NOT NULL DEFAULT 'ACTIVE',
  created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  allocated_resources JSONB
);

CREATE TABLE resource_accounting (
  id BIGSERIAL PRIMARY KEY,
  owner_type VARCHAR(64) NOT NULL,
  owner_id VARCHAR(256) NOT NULL,
  resource_type VARCHAR(64) NOT NULL,
  resource_unit VARCHAR(64) NOT NULL,
  consumption DOUBLE PRECISION NOT NULL,
  start_time TIMESTAMPTZ NOT NULL,
  end_time TIMESTAMPTZ NOT NULL,
  cost DOUBLE PRECISION,
  allocation_id UUID REFERENCES allocations(id)
);
```

## 15. Validation

### 15.1 Input Validation
Resource profiles validated for completeness (all required resource types present) and reasonableness (min <= desired <= max). Resource constraints validated for compatibility with available node types. Quota limits validated as positive numbers with hard >= soft. Burst allowance validated as positive duration.

### 15.2 Allocation Validation
Requested resources must not exceed quota (hard limit check). Requested resources must be available on at least one node. Resource constraints must be satisfiable (e.g., NUMA affinity matches available topology). Priority must be within defined range (0-100).

### 15.3 Runtime Validation
Actual resource usage validated against allocation limits (enforcement action on violation). Quota consumption reconciled with actual usage periodically. Reservation availability validated against current allocations before commitment.

## 16. Security

### 16.1 Authentication
API authentication via service-to-service JWT tokens. Node Agent authentication via TLS client certificates. Administrative operations require elevated authentication.

### 16.2 Authorization
Role-based access control: resource.admin (full access), resource.allocator (request and manage allocations), resource.viewer (read-only), quota.manager (manage quotas), quota.viewer (view quotas). Resource visibility scoped to owner's organization tree unless cross-team access granted.

### 16.3 Audit Trail
All allocation changes logged with caller identity and timestamp. All quota modifications logged with before/after values. All reservation lifecycle events logged. All resource enforcement actions (preemption, OOM, throttling) logged.

## 17. Monitoring and Observability

### 17.1 Key Metrics

| Metric | Type | Labels | Description |
|--------|------|--------|-------------|
| resource_utilization | Gauge | node, resource_type | Current resource utilization percent |
| resource_saturation | Gauge | node, resource_type | Resource saturation indicator |
| resource_errors_total | Counter | node, resource_type, error_type | Resource error count |
| resource_allocation_count | Gauge | status | Active allocations count |
| resource_quota_consumption | Gauge | quota_id, resource_type | Current quota usage |
| resource_oversubscription_ratio | Gauge | resource_type, node | Current oversubscription ratio |
| resource_pressure_events_total | Counter | type, severity | Resource pressure event count |
| resource_preemption_count | Counter | priority, reason | Preemption event count |
| resource_defragmentation_score | Gauge | node | Current fragmentation score |
| resource_allocation_latency_ms | Histogram | — | Allocation request processing time |

### 17.2 Logging
Structured JSON logging with correlation ID. Log levels: DEBUG for allocation details (sampled), INFO for allocations and releases, WARN for quota warnings and pressure events, ERROR for enforcement failures and node failures.

### 17.3 Distributed Tracing
Allocation requests traced through RMS and Node Agent. Span attributes include resource type, amount, node, and decision result. Trace sampling: 100% for rejections and errors, 10% for successful allocations.

### 17.4 Health Checks
RMS health: global state consistency, backend connectivity, replication status. Node Agent health: cgroup availability, metric reporting, enforcement capability. Scheduler health: decision latency, allocation queue depth.

## 18. Scalability

### 18.1 Horizontal Scaling
RMS horizontally scalable with partitioned node ownership. Node Agents stateless and independently scalable. Scheduler can be sharded by resource pool or organization.

### 18.2 Vertical Scaling
RMS memory scales with cluster size (target 10,000 nodes per RMS instance). Node Agent resource overhead minimal (< 100MB per node). Time-series database scales with metric cardinality and retention.

### 18.3 Performance Targets
Allocation decision latency: < 50ms P99 for standard requests, < 200ms P99 for complex constraints. Metric reporting latency: < 1s from collection to dashboard. Quota enforcement latency: < 10ms from allocation request to enforcement decision.

## 19. Testing

### 19.1 Unit Tests
Allocation logic for all scheduling strategies. Quota enforcement logic with hard/soft/burst limits. Resource profile validation. Oversubscription ratio calculation and enforcement.

### 19.2 Integration Tests
End-to-end allocation lifecycle: request, approve, allocate, monitor, release. Quota enforcement with actual resource consumption. Node Agent metric collection and reporting. Reservation guarantee and preemption behavior.

### 19.3 Chaos Tests
Node failure with allocation failover. Resource pressure injection with OOM and throttling verification. Oversubscription saturation test with preemption of lowest-priority workloads. Network partition between RMS and Node Agents.

## 20. Risk Analysis

| Risk | Probability | Impact | Mitigation |
|------|------------|--------|------------|
| Oversubscription causing resource contention | Medium | High | Conservative ratios, pressure monitoring, preemption |
| RMS state inconsistency | Low | Critical | State replication, reconciliation, failover |
| OOM killer killing critical agent | Low | High | OOM priority configuration, guaranteed reservations |
| Resource accounting drift | Medium | Medium | Periodic reconciliation, consumption audit |
| Scheduling fragmentation | Medium | Medium | Defragmentation, topology-aware placement |
| Quota enforcement bypass | Low | High | Kernel-level enforcement, audit, reconciliation |
| Node agent failure causing blind spot | Low | Medium | RMS heartbeat detection, automatic node draining |

## 21. Limits

| Limit | Value | Rationale |
|-------|-------|-----------|
| Max nodes per RMS instance | 10,000 | State memory and update bandwidth |
| Max allocations per node | 1000 | Cgroup and kernel overhead |
| Max oversubscription ratio (CPU) | 3.0 | Practical utilization patterns |
| Max oversubscription ratio (RAM) | 1.5 | OOM risk mitigation |
| Max quota hierarchy depth | 5 | Complexity management |
| Min allocation granularity | 0.1 CPU, 1MB RAM | Kernel scheduling granularity |
| Max allocation duration | Unlimited | System design |
| Max metric scrape interval | 60 seconds | Acceptable staleness |
| Min metric scrape interval | 1 second | Collection overhead |
| Max concurrent allocation requests | 10,000/sec | RMS processing capacity |

## 22. Maintenance

### 22.1 Routine Tasks
Review oversubscription ratios and adjust targets weekly. Validate quota consumption trends and adjust limits monthly. Verify resource accounting accuracy by reconciling with actual usage weekly. Review fragmentation scores and schedule defragmentation as needed. Rotate node agent certificates quarterly.

### 22.2 Capacity Planning
Monitor resource utilization trends and growth rates. Project resource exhaustion dates based on current trends. Plan capacity additions when utilization exceeds 70% of total capacity for 7 consecutive days. Review oversubscription ratios and adjust based on actual contention patterns.

## 23. Future Improvements

- **ML-Based Oversubscription Tuning**: ML model that predicts optimal oversubscription ratios based on workload patterns, time-of-day, and historical contention.
- **Autonomous Resource Negotiation**: Agents negotiate resources with each other using market-based mechanisms for optimal allocation.
- **Cross-Cluster Resource Federation**: Unified resource management across multiple clusters and cloud providers with transparent workload migration.
- **Resource-Aware Scheduling for ML Training**: GPU topology-aware scheduling that optimizes for NVLink topology, GPU memory bandwidth, and model parallelism.
- **Predictive Resource Pressure Prevention**: ML-based prediction of resource pressure events (OOM, throttling) with proactive mitigation.
- **Carbon-Aware Resource Allocation**: Resource allocation that considers carbon intensity of different regions and times for environmentally optimal placement.

## 24. Acceptance Criteria

1. Resource allocation requests processed within 50ms P99 under 1000 concurrent requests.
2. Hard quota enforcement prevents allocation exceeding limits with < 10ms enforcement delay.
3. Oversubscription ratio maintained within configured bounds across the cluster.
4. OOM killer correctly identifies and terminates lowest-priority allocation under memory pressure.
5. Resource defragmentation improves fragmentation score by 50%+ after execution.
6. USE metrics collected and available for query within 1 second of resource state change.
7. Resource accounting tracks 100% of resource consumption with < 0.1% drift.
8. Node Agent failure detected within 3 heartbeat intervals and allocations handled.
9. Reservation guarantees honored with zero preemption for HARD guaranteed allocations.
10. All resource enforcement actions logged with complete audit trail.

## 25. Definition of Done (DoD)

1. All acceptance criteria verified through automated testing.
2. Unit test coverage above 85% for allocation, quota, and scheduling logic.
3. Integration tests pass for all resource types and enforcement mechanisms.
4. Performance benchmarks meet all latency and throughput targets.
5. Chaos tests pass with 48-hour soak including node failures, pressure events, and partitions.
6. Monitoring dashboards created for resource overview, per-node USE metrics, quota status, and fragmentation.
7. Alert rules configured for quota violations, pressure events, node failures, and oversubscription threshold breaches.
8. Security review completed covering authentication, authorization, and audit.
9. Documentation complete: API reference, configuration guide, operational runbook.
10. Runbook reviewed by operations team with demonstrated enforcement and recovery procedures.
11. Defragmentation and preemption procedures tested and documented.
12. Capacity planning reports established with growth projections and recommended actions.

## 26. Detailed Resource Scheduling Strategies

### 26.1 Bin-Packing Strategy
Bin-packing scheduling places allocations onto nodes to maximize resource utilization. The scheduler evaluates each node's available resources and selects the node that results in the highest utilization after placement (best-fit decreasing). CPU and memory are the primary dimensions for bin-packing, with GPU and other resources as secondary dimensions. The bin-packing algorithm uses first-fit decreasing where allocations are sorted by resource demand (largest first) and placed on the first node with sufficient capacity. This strategy minimizes the number of active nodes required, reducing infrastructure costs. It is suitable for batch processing workloads, non-production environments, and cost-optimized deployments. Risk: increased resource contention and reduced performance isolation during load spikes.

### 26.2 Spread Strategy
Spread scheduling places allocations across nodes to maximize availability and fault tolerance. The scheduler distributes allocations across different nodes, racks, availability zones, and failure domains to minimize the impact of node failures. Anti-affinity rules prevent placing multiple replicas of the same service on the same node. The spread strategy uses worst-fit placement where each allocation is placed on the node with the most available resources, ensuring maximum headroom per node. This strategy is optimal for production-critical services, stateful workloads, and latency-sensitive applications. Trade-off: lower overall utilization (typically 40-60%) compared to bin-packing (70-85%).

### 26.3 Topology-Aware Strategy
Topology-aware scheduling considers the physical and logical topology of the cluster for placement decisions. The scheduler accounts for NUMA node boundaries to minimize remote memory access latency, GPU NVLink topology for optimal GPU-to-GPU communication bandwidth, rack and switch topology for network latency optimization, and storage locality for data-intensive workloads. Topology-aware scheduling uses a cost function that assigns weights to different topology factors. Default weights: NUMA affinity 40%, GPU connectivity 30%, rack locality 20%, storage locality 10%. Weights are configurable per workload type. This strategy is essential for high-performance computing, ML training, and real-time inference workloads where topology directly impacts performance.

### 26.4 Cost-Aware Strategy
Cost-aware scheduling minimizes infrastructure cost while meeting performance requirements. The scheduler considers resource costs per node type (different instance types have different cost-to-performance ratios), reserved vs. on-demand pricing, spot instance availability, and regional pricing differences. The cost optimization function: minimize ∑(resourceCost × resourceUsage) subject to performanceConstraints(nodeLatency, nodeThroughput, nodeCapability). This strategy is suitable for cost-sensitive workloads, batch processing, and development/test environments. Trade-off: potential higher latency and lower throughput when using lower-cost resources. The cost-aware strategy integrates with the AIOS Cost Manager for real-time cost data.

## 27. Resource Manager Security Configuration

### 27.1 cgroup Configuration for Container Isolation
```
# CPU quota for an agent container
echo "echo 50000 > /sys/fs/cgroup/cpu/agent-{id}/cpu.cfs_quota_us"
echo "echo 100000 > /sys/fs/cgroup/cpu/agent-{id}/cpu.cfs_period_us"

# Memory limit for an agent container
echo "echo 2147483648 > /sys/fs/cgroup/memory/agent-{id}/memory.limit_in_bytes"
echo "echo 1610612736 > /sys/fs/cgroup/memory/agent-{id}/memory.soft_limit_in_bytes"

# IOPS limit for an agent container
echo "echo \"8:0 10000\" > /sys/fs/cgroup/blkio/agent-{id}/blkio.throttle.read_iops_device"
echo "echo \"8:0 20000\" > /sys/fs/cgroup/blkio/agent-{id}/blkio.throttle.write_iops_device"
```

### 27.2 Linux Capability Restrictions
Agent containers are run with minimal Linux capabilities to prevent resource manipulation:
- Dropped capabilities: CAP_SYS_ADMIN, CAP_SYS_RESOURCE, CAP_NET_ADMIN, CAP_SYS_MODULE, CAP_SYS_PTRACE, CAP_SYS_RAWIO, CAP_DAC_OVERRIDE, CAP_CHOWN, CAP_FOWNER, CAP_SETUID, CAP_SETGID.
- Retained capabilities: CAP_NET_BIND_SERVICE (if binding to privileged ports).
Capability restrictions prevent agents from modifying cgroup settings, changing resource limits, accessing raw devices, or performing other operations that could circumvent resource enforcement.

## 28. Resource Manager Monitoring and Alerting Details

### 28.1 USE Method Monitoring Configuration
Each resource type has specific USE metrics collected at configurable intervals:
- CPU: Utilization (system + user + iowait), Saturation (run queue length / load average), Errors (segfaults, machine checks). Collection interval: 15 seconds.
- Memory: Utilization (active + inactive + cache + buffers), Saturation (swap usage / OOM score / PSI memory pressure), Errors (OOM events, ECC memory errors). Collection interval: 15 seconds.
- Storage: Utilization (IOPS / bandwidth vs. max capacity), Saturation (IO queue depth / await time), Errors (read/write errors, timeout, disk failures). Collection interval: 30 seconds.
- Network: Utilization (bandwidth vs. link capacity), Saturation (packet drops / backlog / retransmits), Errors (interface errors, CRC errors, collisions). Collection interval: 30 seconds.
- GPU: Utilization (compute / memory vs. max), Saturation (kernel launch queue depth, memory queue), Errors (XID errors, ECC errors, TDR events). Collection interval: 10 seconds.

### 28.2 Alert Rules Configuration
```yaml
alerts:
  - name: NodeCPUOvercommit
    condition: avg(node_cpu_utilization) > 0.85 for 5m
    severity: WARNING
    action: "Node CPU utilization exceeds 85%. Review allocations."
    
  - name: NodeMemoryPressure
    condition: node_memory_available_bytes / node_memory_total_bytes < 0.1
    severity: CRITICAL
    action: "Node memory pressure critical. Immediate preemption or scale."
    
  - name: OOMKillEvent
    condition: increase(node_oom_kills_total[5m]) > 0
    severity: CRITICAL
    action: "OOM killer triggered. Investigate memory allocation and pressure."
    
  - name: OversubscriptionRatioExceeded
    condition: oversubscription_ratio > max_oversubscription_ratio
    severity: WARNING
    action: "Oversubscription ratio exceeds maximum. Adjust quotas or scale."
    
  - name: AllocationFailureRate
    condition: rate(resource_allocation_failures_total[5m]) > 0.01
    severity: WARNING
    action: "Allocation failure rate above 1%. Check resource availability."
    
  - name: NodeResourceSaturation
    condition: node_cpu_saturation > 10 or node_memory_saturation > 0.5
    severity: WARNING
    action: "Node saturation detected. Review workload placement."
```

## 29. Resource Manager Capacity Planning Models

### 29.1 Linear Growth Model
The linear growth model assumes constant resource growth over time: resource(t) = base + rate * t. This model is suitable for stable workloads with predictable growth patterns. The model is fitted using least squares regression on historical data. Forecast confidence intervals widen linearly with forecast horizon. The linear model is the default for services with less than 6 months of historical data.

### 29.2 Exponential Growth Model
The exponential growth model assumes multiplicative growth: resource(t) = base * growthRate^t. This model is suitable for rapidly growing services, new product launches, or viral adoption patterns. The model is fitted using log-linear regression. Growth rates above 10% per month trigger capacity planning escalation. The exponential model requires at least 3 months of data for reliable fitting.

### 29.3 Logistic Growth Model
The logistic growth model assumes S-curve growth with a saturation point: resource(t) = capacity / (1 + exp(-k * (t - t0))). This model is suitable for services approaching market saturation or physical capacity limits. The saturation point (capacity) is estimated from infrastructure limits or market data. The logistic model requires at least 12 months of data for reliable fitting and is used for long-term capacity planning only.

### 29.4 Seasonal Growth Model
The seasonal growth model incorporates daily, weekly, and yearly patterns: resource(t) = trend(t) + seasonality_weekly(t) + seasonality_daily(t) + noise. This model is suitable for services with strong usage patterns (e.g., business hours, weekday/weekend, holiday seasons). Seasonal decomposition uses STL (Seasonal-Trend decomposition using Loess) with configurable seasonal periods. The seasonal model requires at least 4 weeks of data for daily patterns and 12 months for yearly patterns.

## 30. Resource Manager Scalability Limits

### 30.1 RMS Instance Limits
- Maximum nodes managed: 10,000 per RMS instance.
- Maximum allocations tracked: 1,000,000 per RMS instance.
- Maximum concurrent allocation requests: 10,000 per second.
- State update rate: 100,000 node metric reports per second.
- State memory: 1GB baseline + 200 bytes per allocation + 100 bytes per node.

### 30.2 Scheduler Limits
- Maximum scheduling decisions per second: 1,000.
- Maximum constraints per request: 20.
- Maximum topology depth: 5 levels (region, AZ, rack, node, NUMA).
- Scheduling decision latency: 10ms P50, 50ms P99 for standard requests.

### 30.3 Node Agent Limits
- Maximum allocations per node: 1,000.
- Maximum cgroups per node: 1,000.
- Metric collection overhead: < 1% CPU, < 50MB memory per node.
- Metric reporting rate: maximum 10 reports per second.

### 30.4 Enforcement Limits
- cgroup write latency: < 1ms per cgroup operation.
- OOM notification delay: < 100ms from OOM to notification.
- Throttling reaction time: < 100ms from enforcement decision to limit application.
- Network QoS propagation delay: < 50ms for tc filter updates.

## 31. Resource Manager Integration with Container Orchestration

### 31.1 Kubernetes Integration
The AIOS Resource Manager integrates with Kubernetes for container orchestration and resource management. It acts as a custom scheduler or scheduler extender that makes resource allocation decisions based on global resource state, policies, and optimization goals. The integration includes: reading Kubernetes node resources and pod resource requests/limits via the Kubernetes API, writing resource allocation decisions as pod scheduling decisions, enforcing resource quotas through Kubernetes ResourceQuota and LimitRange objects, and monitoring resource usage through Kubernetes Metrics Server and cAdvisor. The Resource Manager also manages oversubscription by setting appropriate resource request-to-limit ratios and managing burst capacity through Kubernetes pod priority and preemption.

### 31.2 Docker/Container Integration
At the container level, the Resource Manager enforces resource limits through Docker container resource constraints: CPU shares and quota via --cpu-shares and --cpu-quota, memory limits via --memory and --memory-reservation, IOPS limits via --device-read-iops and --device-write-iops, and blkio weight for proportional IO distribution. Container resource profiles are translated from the agent's resource profile at container creation time. Resource updates during agent runtime trigger container resource limit updates via Docker API update calls. The integration ensures that resource enforcement at the container level matches the Resource Manager's allocation decisions.

### 31.3 cgroup v2 Integration
All resource enforcement ultimately uses Linux cgroup v2 controllers: CPU controller for CPU quota, period, and weight. Memory controller for memory min, low, high, and max limits. IO controller for IO weight and IO max limits (IOPS and bandwidth). PIDs controller for maximum number of processes/threads. cgroup v2 provides a unified hierarchy with improved pressure stall information (PSI) for resource pressure monitoring. The Resource Manager creates cgroup directories for each allocation and writes limit values to the appropriate cgroup control files. cgroup statistics are read periodically for resource usage tracking and enforcement verification.

## 32. Resource Manager Cost Analysis

### 32.1 Infrastructure Cost Modeling
The Resource Manager supports cost modeling for resource allocation decisions: CPU cost per core-hour based on instance type and purchase option (on-demand, reserved, spot). Memory cost per GB-hour based on instance type. GPU cost per GPU-hour based on GPU type and purchase option. Storage cost per IOPS-GB-month based on storage type (SSD, HDD, NVMe). Network cost per GB-transferred based on region and provider. Cost models are configurable and updated from AIOS Cost Manager. Resource allocation decisions can include cost optimization as a factor in the scheduling strategy.

### 32.2 Resource Efficiency Metrics
Resource efficiency is measured using the following metrics: CPU utilization percentage (average over time window). Memory utilization percentage (average over time window). Resource allocation efficiency: (actual usage / allocated resources) averaged over time. Cluster packing efficiency: (total allocated resources / total cluster capacity). Oversubscription efficiency: (total allocated / total physical) without performance degradation. Fragmentation cost: percentage of resources lost to fragmentation. Efficiency targets: CPU utilization > 65%, Memory utilization > 75%, Allocation efficiency > 50%, Cluster packing > 75%, Oversubscription efficiency = target ratio - 0.1 (buffer).

### 32.3 Chargeback Data Export
The Resource Manager exports chargeback data to AIOS Cost Manager in the following format: allocation records with start/end time, resource quantities (CPU core-seconds, GB-seconds, GPU-seconds, etc.), resource type and class, allocation owner (agent/service/team), and cost calculation metadata (purchase option, region, provider). Data is exported in batches every hour and also streamed in real-time for near-real-time cost tracking. Chargeback accuracy: within 1% of actual infrastructure cost, validated by monthly reconciliation against cloud provider billing data.

## 33. Resource Manager Governance Policies

### 33.1 Resource Consumption Policies
Resource consumption policies define acceptable resource usage patterns: maximum resource consumption per agent per resource type, maximum burst duration and magnitude above soft limits, minimum resource efficiency (actual usage / allocated resources) before resource reclamation, resource consumption growth rate limits (maximum month-over-month increase), and resource consumption during off-peak hours (right-sizing requirements). Policy violations generate warnings initially, followed by automated actions (resource limit reduction, preemption, or quarantine) if violations continue after warning period.

### 33.2 Resource Approval Workflows
Resource allocation requests that exceed approval thresholds require workflow approval: allocations exceeding department quota require department head approval, allocations exceeding team quota require engineering manager approval, allocations of specialized resources (GPU, FPGA) require resource committee approval, reservations for critical priority require SRE director approval. Approval workflows integrate with the AIOS notification system and support approval via collaboration tools (Slack, email). Approval decisions are cached for 24 hours for efficiency.

### 33.3 Resource Audit and Compliance
All resource allocation, modification, and release operations are logged for audit. Audit logs include: operation type (ALLOCATE, MODIFY, RELEASE, PREEMPT), resource quantities before and after operation, requesting entity and authenticated user, approval reference (if applicable), and timestamp (microsecond precision). Audit logs are retained for 7 years for compliance purposes and are exportable to SIEM systems. Compliance reports are generated quarterly showing resource allocation patterns, approval rates, and policy violation history.

## 34. Resource Manager Future Architecture Extensions

### 34.1 Cross-Cluster Resource Federation
Future versions of the Resource Manager will support cross-cluster resource federation where resources from multiple Kubernetes clusters, cloud providers, and on-premise data centers are managed as a single resource pool. Federation uses a hierarchical Resource Manager topology with global and local instances. Global RMS manages cross-cluster resource allocation, quota enforcement across clusters, and workload placement optimization across clusters. Local RMS instances manage within-cluster allocation and enforcement. Cross-cluster resource transfer uses workload migration (container live migration or stateful workload transfer).

### 34.2 Edge Resource Management
Edge resource management extends the Resource Manager to edge computing nodes with constrained resources. Edge nodes have limited CPU, memory, and storage compared to cloud nodes. Edge Resource Manager operates in offline-capable mode with local decision making when disconnected from centralized RMS. Edge resource profiles include bandwidth limitations and intermittent connectivity patterns. Resource allocation on edge nodes prioritizes latency-critical workloads over resource-intensive workloads.

### 34.3 Quantum Resource Management (Future)
As quantum computing resources become available, the Resource Manager will extend to manage quantum resources: qubit count and quality, quantum volume, coherence time, gate fidelity, and quantum-classical hybrid resource coordination. Quantum resource allocation will consider qubit availability, error rates, and workload quantum resource requirements. Quantum resource scheduling will integrate with classical resource scheduling for hybrid quantum-classical workloads.

## 35. Resource Manager Detailed Metrics Reference

### 35.1 CPU Metrics Reference
CPU metrics collected by the Resource Manager: cpu_utilization_percent (total CPU utilization as percentage, range 0-100). cpu_user_percent (user space CPU time percentage). cpu_system_percent (kernel CPU time percentage). cpu_iowait_percent (CPU waiting for IO completion percentage). cpu_steal_percent (CPU stolen by hypervisor for other VMs). cpu_load_1m (system load average for 1 minute). cpu_load_5m (system load average for 5 minutes). cpu_load_15m (system load average for 15 minutes). cpu_cfs_throttled_seconds_total (total time CPU throttled by cgroup). cpu_cfs_throttled_count (number of throttling periods). cpu_run_queue_length (number of tasks waiting for CPU). cpu_pressure (PSI pressure stall information for CPU, 10s/60s/300s averages). Each metric is available per node, per container/cgroup, and per allocation.

### 35.2 Memory Metrics Reference
Memory metrics collected by the Resource Manager: memory_total_bytes (total physical memory). memory_available_bytes (available memory for new allocations). memory_used_bytes (total memory in use). memory_active_bytes (actively used memory, not reclaimable). memory_inactive_bytes (inactive memory, reclaimable under pressure). memory_cached_bytes (page cache and tmpfs). memory_buffers_bytes (buffer cache). memory_swap_total_bytes (total swap space). memory_swap_used_bytes (swap in use). memory_hugepages_total (total huge pages). memory_hugepages_free (free huge pages). memory_oom_kills_total (total OOM killer invocations). memory_pressure (PSI pressure stall information for memory, 10s/60s/300s averages). memory_fragmentation_index (memory fragmentation level, 0-1). Each metric is available per node, per NUMA node, per container/cgroup, and per allocation.

### 35.3 Storage Metrics Reference
Storage metrics collected by the Resource Manager: disk_reads_total (total read operations). disk_writes_total (total write operations). disk_read_bytes_total (total bytes read). disk_write_bytes_total (total bytes written). disk_read_latency_ms (average read latency in milliseconds). disk_write_latency_ms (average write latency in milliseconds). disk_iops_read (current read IOPS). disk_iops_write (current write IOPS). disk_io_queue_depth (current IO queue depth). disk_io_await_ms (average IO operation time including queue time). disk_io_svctm_ms (average IO service time excluding queue time). disk_pressure (PSI pressure stall information for IO, 10s/60s/300s averages). disk_errors_total (total disk errors). Storage metrics are available per device, per mount point, per container/cgroup, and per allocation.

### 35.4 Network Metrics Reference
Network metrics collected by the Resource Manager: network_bytes_received_total (total bytes received). network_bytes_transmitted_total (total bytes transmitted). network_packets_received_total (total packets received). network_packets_transmitted_total (total packets transmitted). network_errors_received_total (total receive errors). network_errors_transmitted_total (total transmit errors). network_packets_dropped_received_total (total receive drops). network_packets_dropped_transmitted_total (total transmit drops). network_retransmits_total (total TCP retransmissions). network_connections_active (current active TCP connections). network_connections_established (established TCP connections). network_bandwidth_received_bps (current receive bandwidth in bps). network_bandwidth_transmitted_bps (current transmit bandwidth in bps). Network metrics are available per interface, per container, and per allocation.

### 35.5 GPU Metrics Reference
GPU metrics collected by the Resource Manager: gpu_utilization_percent (GPU compute utilization). gpu_memory_utilization_percent (GPU memory utilization). gpu_memory_total_bytes (total GPU memory). gpu_memory_used_bytes (used GPU memory). gpu_memory_free_bytes (free GPU memory). gpu_temperature_celsius (GPU temperature). gpu_power_watts (GPU power consumption). gpu_power_limit_watts (GPU power limit). gpu_pcie_throughput_rx_bps (PCIe receive throughput). gpu_pcie_throughput_tx_bps (PCIe transmit throughput). gpu_nvlink_throughput_rx_bps (NVLink receive throughput). gpu_nvlink_throughput_tx_bps (NVLink transmit throughput). gpu_encoder_utilization_percent (video encoder utilization). gpu_decoder_utilization_percent (video decoder utilization). gpu_xid_errors_total (total XID error events). GPU metrics are available per GPU device, per MIG partition, per container, and per allocation.

## 36. Resource Manager Alert Configuration Templates

### 36.1 Capacity Alert Templates
NodeCPUHigh: avg(node_cpu_utilization) > 85% for 10 minutes, severity WARNING, notify team. NodeMemoryHigh: avg(node_memory_used_bytes / node_memory_total_bytes) > 90% for 5 minutes, severity WARNING, notify team. NodeDiskFull: predict_linear(node_disk_free_bytes[1h], 3600) < 0 for 1 hour, severity CRITICAL, notify SRE on-call. ClusterCPUHigh: avg(cluster_cpu_utilization) > 75% for 30 minutes, severity WARNING, notify capacity planning. ClusterMemoryHigh: avg(cluster_memory_utilization) > 80% for 30 minutes, severity WARNING, notify capacity planning. GPUUtilizationHigh: avg(gpu_utilization) > 95% for 15 minutes, severity WARNING, notify ML platform team.

### 36.2 Saturation Alert Templates
CPUSaturation: avg(node_cpu_saturation) > 5 for 5 minutes, severity WARNING, notify team. MemoryPressure: avg(node_memory_pressure_10s) > 0.5 for 5 minutes, severity CRITICAL, notify SRE on-call. IOPressure: avg(node_io_pressure_10s) > 0.5 for 5 minutes, severity WARNING, notify team. CPUPressure: avg(node_cpu_pressure_10s) > 0.5 for 5 minutes, severity WARNING, notify team. DiskQueueDepth: avg(node_disk_io_queue_depth) > 10 for 5 minutes, severity WARNING, notify team.

### 36.3 Error Alert Templates
OOMKill: increase(node_oom_kills_total[5m]) > 0, severity CRITICAL, notify SRE on-call. DiskError: increase(node_disk_errors_total[5m]) > 0, severity CRITICAL, notify SRE on-call. NetworkError: increase(node_network_errors_total[5m]) > 0, severity WARNING, notify network team. GPUError: increase(gpu_xid_errors_total[5m]) > 0, severity CRITICAL, notify ML platform team. AllocationFailure: rate(resource_allocation_failures_total[5m]) > 0.05, severity WARNING, notify team.

### 36.4 Quota and Policy Alert Templates
QuotaExceeded: increase(resource_quota_violations_total[5m]) > 0, severity WARNING, notify quota owner. OversubscriptionBreached: avg(oversubscription_ratio) > max_oversubscription_ratio for 5 minutes, severity CRITICAL, notify capacity planning. ReservationExpiring: resource_reservation_end_time - now() < 7 days, severity INFO, notify reservation owner. ResourceHogging: agent(avg(resource_utilization) > 90% for 1 hour), severity INFO, notify agent owner.

## 37. Resource Manager Troubleshooting Guide

### 37.1 Allocation Failures
Problem: Allocation request returns failure. Common causes: insufficient cluster capacity, quota exceeded, resource constraints unsatisfiable, node agent unreachable. Diagnostic steps: Check cluster resource availability (free CPU, free memory, etc.). Verify quota consumption (current vs. limit for requester scope). Verify resource constraints in allocation request (CPU/RAM/GPU type compatibility). Check node agent health (heartbeat status, last contact time). Check scheduler logs for specific rejection reason. Resolution: Increase cluster capacity (add nodes). Request quota increase or release other allocations. Adjust resource constraints to match available node types. Restart or replace failed node agent.

### 37.2 Resource Enforcement Failures
Problem: Agent consuming more resources than allocated limit. Common causes: cgroup configuration not applied correctly, kernel cgroup version mismatch, agent bypassing container limits (privileged container), resource limit update not propagated. Diagnostic steps: Verify cgroup configuration for allocation (check control files). Check if cgroup v2 is available and enabled (required for IO and PSI). Verify container is running with appropriate security context (non-privileged). Check node agent logs for resource update failures. Resolution: Re-apply cgroup configuration. Re-create container with correct security context. Upgrade kernel to support cgroup v2 for required features. Restart node agent to reload resource configurations.

### 37.3 Resource Accounting Drift
Problem: Resource accounting shows different consumption than actual usage. Common causes: Allocation not properly released, cgroup stats read failure, node agent downtime led to missed accounting, time zone or clock skew between nodes. Diagnostic steps: Compare allocation consumption with node-level resource usage for same agent/container. Check allocation release logs for missing release events. Verify node agent was running continuously during accounting period. Check clock synchronization (NTP status) on all nodes. Resolution: Manually release orphaned allocations. Trigger reconciliation job to correct accounting records. Fix node agent issues and backfill missing accounting data. Ensure NTP is properly configured on all nodes.

## 38. Resource Manager Testing Specifications

### 38.1 Unit Test Requirements
Resource manager unit tests must cover: Allocation logic for all scheduling strategies (bin-packing, spread, topology-aware, cost-aware) with correct node selection and resource verification. Quota enforcement for hard limits, soft limits, and burst allowances with correct acceptance and rejection behavior. Resource profile validation for completeness, reasonableness, and constraint compatibility. Oversubscription ratio calculation and enforcement with correct ratio tracking and breach detection. Reservation management with correct lifecycle transitions and conflict detection. Resource negotiation with correct request evaluation and decision logic. Fragmentation score calculation with correct aggregation and threshold comparison. Minimum unit test coverage: 90% for core resource management logic, 85% for scheduling algorithms, 80% for quota enforcement.

### 38.2 Integration Test Requirements
Integration tests must verify: End-to-end allocation lifecycle: request submission, quota check, scheduling decision, resource allocation on node, cgroup configuration, resource monitoring, and release. Quota enforcement with actual cgroup-based resource limits: verify agent cannot exceed hard limits (CPU throttling, OOM kill, IOPS throttling). Resource pressure response: inject memory pressure, verify OOM killer correctly targets lowest-priority allocation. Oversubscription enforcement: configure oversubscription ratio, allocate beyond physical capacity, verify enforcement actions (throttling, preemption). Reservation guarantee: create reservation, allocate resources up to cluster capacity, verify reserved resources remain available. Defragmentation: create fragmented allocation pattern, trigger defragmentation, verify improved fragmentation score. Accounting accuracy: run workload with known resource consumption, verify accounting records match within 1%.

### 38.3 Performance Test Requirements
Performance tests must verify: Allocation request throughput: 1,000 requests/second sustained with P99 latency < 50ms. Scheduling decision throughput: 100 decisions/second with P99 latency < 200ms for complex constraints. Metric collection throughput: 10,000 nodes reporting metrics at 15-second intervals without backlog. Quota enforcement latency: < 10ms from allocation request to enforcement decision. Node agent overhead: < 1% CPU and < 50MB memory per node for metric collection and enforcement. Performance tests must run for minimum 30 minutes with steady-state verification.

### 38.4 Chaos Test Requirements
Chaos tests must verify: Node failure: running allocations on failed node are detected and re-allocated to healthy nodes within 60 seconds. Network partition: RMS continues operating with available nodes, automatic reconnection after partition heals. Resource pressure injection: simulate memory pressure, verify preemption of lowest-priority workloads. Oversubscription saturation: allocate beyond oversubscription ratio, verify enforcement actions prevent actual resource exhaustion. RMS failover: primary RMS failure, standby RMS takes over within 30 seconds with complete state. Chaos tests must be automated and run weekly in a staging environment.

## 39. Resource Manager Deployment Architecture

### 39.1 Single Cluster Deployment
Components: RMS instance (2 replicas for HA), Node Agent on each compute node (1 per node), Scheduler (embedded in RMS or separate), Quota Manager (embedded in RMS), Accounting Engine (separate service, 2 replicas). Databases: PostgreSQL for persistent state, Prometheus/VictoriaMetrics for metrics storage. Networking: RMS accessible via internal load balancer, Node Agents connect to RMS via gRPC. Security: mTLS between all components, RBAC for API access. Scalability: supports up to 10,000 nodes, up to 100,000 allocations. High availability: RMS replicas behind load balancer, PostgreSQL with streaming replication, Node Agents reconnect on RMS failure.

### 39.2 Multi-Cluster Federation Deployment
Components: Global RMS (manages cross-cluster allocations), Local RMS per cluster (manages within-cluster allocations), Federation Gateway (routes allocation requests to appropriate cluster). Data flow: allocation requests evaluated by Global RMS for cross-cluster placement, forwarded to Local RMS for within-cluster allocation. Global RMS maintains aggregate resource view across clusters. Local RMS maintains detailed resource view within cluster. Federation uses asynchronous state replication with eventual consistency for cross-cluster resource tracking. Scalability: supports up to 100 clusters, unlimited nodes. High availability: Global RMS with active-passive failover, Local RMS independent per cluster.

### 39.3 Edge Deployment
Components: Edge Node Agent (lightweight, operates offline), Edge RMS (local decision making, syncs with central RMS when connected). Edge capabilities: local allocation decisions during network disconnection, metric buffering for deferred reporting, quota enforcement with local state. Synchronization: periodic batch sync with central RMS, conflict resolution for allocation state (last-writer-wins). Edge profiles: reduced resource types (CPU, memory only, no GPU), simplified scheduling (bin-packing only), minimal persistence (local SQLite for state). Scalability: supports up to 1,000 edge nodes per edge RMS.

## 40. Resource Manager API Reference

### 40.1 Allocation API
Create allocation: POST /v1/allocations with body containing owner ID, resource profile, constraints, priority, duration. Returns allocation ID, status, assigned node, and resource limits. Get allocation: GET /v1/allocations/{id} returns allocation details including resource consumption, node, and status. Update allocation: PUT /v1/allocations/{id} with body containing updated resource profile (scale up/down within configured limits). Returns updated allocation details. Release allocation: DELETE /v1/allocations/{id} releases resources and updates accounting. List allocations: GET /v1/allocations with query parameters for owner, node, status, resource type. Returns paginated list of allocations.

### 40.2 Quota API
Create quota: POST /v1/quotas with body containing scope, resource type, hard limit, soft limit, burst configuration. Returns quota ID and current consumption. Get quota: GET /v1/quotas/{id} returns quota details and current consumption. Update quota: PUT /v1/quotas/{id} with body containing updated limits. Delete quota: DELETE /v1/quotas/{id} removes quota (fails if consumption > 0). List quotas: GET /v1/quotas with query parameters for scope type and ID. Returns paginated list of quotas with consumption. Get quota consumption: GET /v1/quotas/{id}/consumption returns current consumption, peak consumption, and utilization percentage.

### 40.3 Metrics API
Get node metrics: GET /v1/metrics/nodes/{nodeId} with query parameters for resource type, metrics list, time range, and aggregation interval. Returns time-series metrics. Get allocation metrics: GET /v1/metrics/allocations/{allocationId} with same parameters as node metrics. Returns per-allocation time-series metrics. Get cluster metrics: GET /v1/metrics/cluster with query parameters for resource type and aggregation. Returns cluster-level aggregate metrics. Get USE metrics: GET /v1/metrics/use?node={nodeId}&resource={resourceType} returns utilization, saturation, and error metrics. Get resource topology: GET /v1/metrics/topology returns hierarchical resource topology (region, AZ, rack, node, NUMA).

### 40.4 Reservation API
Create reservation: POST /v1/reservations with body containing resource profile, start and end time, guarantee level, priority. Returns reservation ID and confirmation of availability. Get reservation: GET /v1/reservations/{id} returns reservation details. Update reservation: PUT /v1/reservations/{id} with body containing updated resource profile or schedule. Delete reservation: DELETE /v1/reservations/{id} releases reservation. List reservations: GET /v1/reservations with query parameters for owner, status, time range. Returns paginated list. Check availability: GET /v1/reservations/availability with query parameters for resource profile and time range. Returns availability status and suggested alternatives if unavailable.

## 41. Resource Manager Troubleshooting Guide

### 41.1 Allocation Failures
Problem: Allocation request returns failure. Common causes: insufficient cluster capacity, quota exceeded, resource constraints unsatisfiable, node agent unreachable, scheduler configuration error. Diagnostic steps: Check cluster resource availability (free CPU, free memory, free GPU) across nodes. Verify quota consumption (current vs. limit for requester scope). Verify resource constraints in allocation request (CPU type compatibility, GPU model requirement, NUMA affinity). Check all node agents' health status (heartbeat, last contact). Verify scheduler configuration (strategy, weights, policies). Resolution: Scale cluster (add nodes) or release unused allocations. Request quota increase or optimize existing allocations. Relax resource constraints (remove GPU requirement, allow any NUMA node). Restart or replace failed node agent. Review and correct scheduler configuration.

### 41.2 Quota Enforcement Failures
Problem: Agent consuming resources beyond allocated quota without enforcement. Common causes: cgroup enforcement not configured correctly, kernel version not supporting cgroup v2, agent running as privileged container bypassing limits, enforcement delay (resource limit not applied before agent started). Diagnostic steps: Verify cgroup configuration for allocation (check control files exist and have correct values). Check kernel version (cgroup v2 required for full enforcement). Verify container security context (non-privileged, no CAP_SYS_RESOURCE). Check timing of resource limit application vs. agent start. Resolution: Re-apply cgroup configuration with correct paths and values. Upgrade kernel to version supporting cgroup v2. Recreate container with non-privileged security context. Ensure resource limits are applied before agent start (synchronize allocation and container creation).

### 41.3 Resource Accounting Drift
Problem: Resource accounting shows different consumption than actual usage. Common causes: Allocation not properly released (orphaned allocation), cgroup stats read failure during accounting period, node agent downtime causing missed accounting, clock skew between nodes causing timing misalignment, allocation modification not reflected in accounting. Diagnostic steps: Compare allocation consumption with node-level resource usage for same agent/container. Check allocation release logs for missing release events (agent crash without cleanup). Verify node agent was running continuously during accounting period. Check clock synchronization status (NTP) on all nodes. Verify allocation modification history (scale events, resource adjustments). Resolution: Manually release orphaned allocations via API. Trigger reconciliation job to correct accounting records (compares actual usage vs. recorded usage). Fix node agent issues (restart, update) and backfill missing accounting data. Configure NTP on all nodes with consistent time source. Update accounting records for allocation modifications.

### 41.4 Scheduling Performance Degradation
Problem: Scheduling decisions taking longer than expected (> 200ms P99). Common causes: Large number of pending allocation requests, high cluster complexity (many nodes, many existing allocations), resource constraint evaluation overhead, scheduling strategy computational complexity (topology-aware > spread > bin-packing). Diagnostic steps: Check scheduler queue depth and processing rate. Check cluster size (nodes, allocations, reservations). Review constraint complexity (number and type of constraints per request). Check scheduling strategy type and configuration. Resolution: Scale scheduler horizontally (add more scheduler instances). Optimize cluster by reducing fragmentation and consolidating allocations. Simplify resource constraints (reduce constraint count, use broader constraint types). Use less computationally intensive strategy (bin-packing instead of topology-aware for non-critical workloads).

## 42. Resource Manager Configuration Templates

### 42.1 Production Configuration Template
```yaml
resource-manager:
  scheduling:
    default_strategy: spread
    strategies:
      production-critical: topology-aware
      batch-processing: bin-packing
    constraints:
      max_per_node: 50 allocations
      anti_affinity: true for replicated services
  oversubscription:
    cpu: 1.5 (target), 2.0 (maximum)
    memory: 1.2 (target), 1.5 (maximum)
    storage: 2.0 (target), 4.0 (maximum)
    network: 1.0 (target), 1.5 (maximum)
  quotas:
    enforcement: HARD for CPU, memory; SOFT for storage, network
    burst:
      enabled: true
      default_duration: 300 seconds
    hierarchy: department > team > service > agent
  reservations:
    min_guarantee: 10% of cluster capacity for CRITICAL workloads
    max_reservation: 50% of cluster capacity
  accounting:
    collection_interval: 60 seconds
    retention: 90 days raw, 365 days aggregated
    chargeback: enabled
```

### 42.2 Development Configuration Template
```yaml
resource-manager:
  scheduling:
    default_strategy: bin-packing
    constraints:
      max_per_node: 100 allocations (allow denser packing)
  oversubscription:
    cpu: 2.0 (target), 3.0 (maximum)
    memory: 1.5 (target), 2.0 (maximum)
  quotas:
    enforcement: SOFT for all resources (warn only)
    burst:
      enabled: true
      default_duration: 600 seconds
  reservations:
    enabled: false (no reservations in dev)
  accounting:
    collection_interval: 300 seconds (less granular)
    retention: 30 days raw
    chargeback: disabled
```

## 43. Resource Manager Version History

### 43.1 Version 1.0.0 (Initial Release)
Core features: Resource tracking for CPU, memory, and storage. CRUD allocation lifecycle. Hard and soft quota enforcement. Bin-packing and spread scheduling strategies. Real-time resource utilization metrics (1-second granularity). Per-node cgroup enforcement. Resource accounting for basic chargeback. REST API for allocation and quota management.

### 43.2 Version 1.1.0
Added features: GPU resource tracking and allocation. Topology-aware scheduling (NUMA, GPU NVLink). Oversubscription management with configurable ratios. Resource pressure monitoring with PSI metrics. Burst allowances for soft limit exceedance. Resource reservation system for critical workloads. Defragmentation controller for optimal resource packing. Prometheus metrics export for USE method monitoring.

### 43.3 Version 1.2.0
Added features: Cost-aware scheduling with integration to AIOS Cost Manager. Cross-cluster resource federation. Network bandwidth tracking and QoS enforcement. Multi-dimensional resource constraints (combine CPU, memory, GPU in single request). Reservation priority and preemption. Automated defragmentation scheduling. Enhanced accounting with team/department chargeback. etcd-backed state for high availability. Performance optimization: 50ms P99 allocation latency.

## 44. Resource Manager Glossary

Resource: A consumable entity required for workload execution (CPU, memory, GPU, storage, network). Allocation: The assignment of resources to a specific agent, service, or workload. Quota: A limit on resource consumption for a specific scope (hard limit = absolute maximum, soft limit = target with burst). Reservation: A guaranteed allocation of resources for future use by a specific workload. Oversubscription: The practice of allocating more resources than physically available, based on typical utilization patterns. Preemption: The forced release of resources from a lower-priority allocation to satisfy a higher-priority request. Defragmentation: The process of reorganizing allocations to reduce fragmentation and improve packing efficiency. USE method: A methodology for monitoring resources: Utilization (percent busy), Saturation (queue depth), Errors (error count). NUMA: Non-Uniform Memory Access - a memory architecture where memory access time depends on the physical location relative to the processor. cgroup: Linux control group - a kernel feature for limiting, accounting for, and isolating resource usage. PSI: Pressure Stall Information - a Linux kernel feature that measures resource pressure (time tasks wait for resources). IOPS: Input/Output Operations Per Second - a measure of storage performance. QoS: Quality of Service - the ability to prioritize different workloads for resource access. Chargeback: The practice of charging internal teams for their resource consumption based on usage accounting.

## 45. Resource Manager Compliance and Audit

### 45.1 Resource Allocation Audit Trail
Every resource allocation, modification, and release operation is logged with: operation timestamp (microsecond precision), operation type (ALLOCATE, MODIFY, RELEASE, PREEMPT), requester identity (service, user, or automation), allocation ID and owner, resource quantities before and after operation, approval reference (if applicable), and originating request trace ID. Audit logs are stored in an immutable append-only format with cryptographic hash chaining for tamper evidence. Audit logs are retained for 7 years in compliance with financial audit requirements. Audit log query API supports filtering by time range, operation type, requester, and allocation owner.

### 45.2 Quota Modification Audit Trail
Every quota modification is logged with: operation timestamp, modified quota ID and scope, previous limits (hard, soft, burst), new limits (hard, soft, burst), modifier identity, approval reference (if applicable), and reason for modification. Quota modification history is retained for the lifetime of the quota plus 3 years. Quota modification reports are generated monthly for review by resource governance team. Unauthorized quota modification attempts (permission denied) are logged and alerted to security team.

### 45.3 Resource Consumption Compliance
Resource consumption compliance is monitored and reported monthly: overallocation detection (agents consuming more than allocated resources), quota violation tracking (agents exceeding hard limits with enforcement action taken), idle resource detection (allocated resources with < 5% utilization for 7+ days), and preemption tracking (lower-priority workloads preempted for higher-priority requests). Compliance reports are distributed to service owners, team leads, and resource governance committee. Non-compliance remediation plans are tracked with target resolution dates.

## 46. Resource Manager Upgrade and Migration Procedures

### 46.1 RMS Version Upgrade
Upgrade Resource Management Service version with zero downtime: Deploy new RMS version in parallel with existing version. New RMS instances connect to same backend databases and read existing state. Gradually shift traffic to new RMS instances (10% increments every 10 minutes). Monitor allocation success rate, latency, and error rate during rollout. If issues detected, rollback traffic to previous version. After full rollout and 24-hour monitoring, decommission old RMS instances.

### 46.2 Node Agent Version Upgrade
Upgrade Node Agent version on compute nodes: Deploy new Node Agent binary on each compute node. Stop old Node Agent, start new Node Agent (agent restart does not affect running allocations - cgroup configuration persists). Verify new agent can read existing cgroup state and continue enforcement. Monitor agent health after upgrade (heartbeat, metric reporting). Upgrade nodes in batches (10% at a time) to limit blast radius. Rollback by redeploying previous agent version on affected nodes.

### 46.3 Database Schema Migration
Migrate PostgreSQL database schema for Resource Manager: Create new schema version alongside existing schema (dual-write). Write new data to both old and new schema. Backfill existing data from old schema to new schema (batch process). Verify data consistency between old and new schema. Cut over reads to new schema. Stop dual-writes to old schema. Drop old schema after verification period. Rollback: switch reads back to old schema, stop writes to new schema.

## 47. Resource Manager Performance Optimization

### 47.1 RMS Performance Optimization
RMS performance optimization techniques: In-memory state caching: cache resource topology and allocation state in local memory with 1-second refresh. Connection pooling: maintain persistent connections to node agents and databases to avoid connection setup overhead. Request batching: batch allocation state updates from multiple node agents into single database transactions. Asynchronous processing: perform non-critical operations (accounting, metrics storage) asynchronously via message queue. Query optimization: use indexed queries for allocation lookups, avoid table scans. Thread pool tuning: size thread pool for allocation request processing based on expected concurrency (target 2x CPU core count).

### 47.2 Scheduler Performance Optimization
Scheduler performance optimization techniques: Pre-computation: maintain pre-computed resource availability per node, updated on each allocation change. Filter-first: apply simple constraint filters (resource type, capacity) before complex constraint evaluation (topology, affinity). Incremental evaluation: for allocation updates (scale), only re-evaluate affected constraints rather than full re-evaluation. Parallel evaluation: evaluate multiple candidate nodes in parallel for allocation decisions. Scoring function optimization: optimize score computation with pre-computed weights and pre-aggregated topology information. Cached results: cache scheduling results for identical allocation requests within configurable window (default 1 second).

### 47.3 Node Agent Performance Optimization
Node Agent performance optimization techniques: Efficient metric collection: read cgroup stats from memory-mapped files, batch read operations. Metric delta computation: compute utilization deltas from cumulative counters rather than full state reads. Reporting compression: compress metric reports before sending to reduce network bandwidth. Adaptive reporting interval: increase reporting interval during low activity (5 seconds), decrease during high activity (1 second). Local buffering: buffer metric reports during network interruptions, batch-send on reconnection. Profile-guided optimization: focus optimization effort on most frequently executed code paths based on profiling data.

## 48. Resource Manager Disaster Recovery Scenarios

### 48.1 Complete RMS Failure
Scenario: All RMS instances become unavailable due to software bug, configuration error, or infrastructure failure. Impact: New allocation requests cannot be processed. Existing allocations continue running (node agents independently enforce limits). Metric collection continues (node agents buffer metrics locally). Detection: RMS health check failures, allocation API errors. Recovery: Deploy new RMS instances from backup configuration. Load allocation state from database (last known state). Reconnect to all node agents and reconcile state. Verify allocation state consistency. Resume allocation processing.

### 48.2 Database State Corruption
Scenario: Resource Manager database is corrupted due to hardware failure, software bug, or data corruption. Impact: Allocation history lost, quota state lost, reservation state lost. Existing allocations continue running (node agents independent). Detection: Database query errors, state inconsistency between RMS and node agents. Recovery: Restore database from latest backup. Reconcile allocation state with node agents (current actual allocations). Rebuild quota state from latest backup and reconcile with current allocations. Rebuild reservation state from latest backup. Verify all state consistency through reconciliation. Resume normal operations.

### 48.3 Cluster-Wide Resource Exhaustion
Scenario: Resource exhaustion across the entire cluster prevents any new allocations. Impact: All allocation requests fail with insufficient capacity. Existing allocations continue running. New agents and services cannot start. Detection: 100% allocation failure rate, scheduling queue backup. Recovery: Identify idle or underutilized allocations and release them. Trigger defragmentation to consolidate resources. If possible, scale cluster by adding new compute nodes. Implement emergency resource reclamation (preempt best-effort workloads). Reevaluate oversubscription ratios and adjust if safe. Communicate capacity constraints to stakeholders.

## 49. Resource Manager Automation and Scripting

### 49.1 Resource Reclamation Automation
Automated resource reclamation: Identify allocations with utilization < 5% for 7 consecutive days. Send warning notification to allocation owner with 48-hour response window. If no response within window, automatically reduce allocation to minimum profile. If utilization remains low for additional 7 days, release allocation and archive data. Reclamation actions are logged to audit trail with owner notification history. Exclusions: allocations tagged with critical-priority are exempt from automatic reclamation.

### 49.2 Capacity Scaling Automation
Automated capacity scaling for cluster nodes: Monitor cluster-level resource utilization. When CPU or memory utilization exceeds 80% for 1 hour, automatically trigger capacity scaling. For cloud deployments: add compute nodes via auto-scaling group (5-minute provisioning time). For on-premise deployments: generate capacity request ticket with resource specifications. Scale-down: when utilization drops below 40% for 24 hours, remove underutilized nodes (drain then terminate). Scale-down excluded for nodes hosting CRITICAL-priority allocations. Scaling actions are logged and notifications sent to infrastructure team.

### 49.3 Quota Management Automation
Automated quota management: Quarterly quota review: automatically generate quota consumption reports and send to quota owners. Quota increase requests within 20% of current limit and within free capacity are auto-approved. Quota increase requests exceeding 20% require manual approval. Unused quota (consumption < 50% for 3 consecutive months) is automatically reduced to 80% of actual consumption. Quota reduction is notified to quota owner with 7-day objection window. Emergency quota increase (critical workloads) is auto-approved with post-approval notification.

## 50. Resource Manager Training Materials

### 50.1 Operator Training Module
Module 1: Resource Manager Architecture and Components (4 hours). Module 2: Allocation Lifecycle Management (4 hours). Module 3: Quota and Reservation Configuration (4 hours). Module 4: Monitoring and Alerting (4 hours). Module 5: Troubleshooting Common Issues (4 hours). Module 6: Backup and Recovery Procedures (4 hours). Module 7: Capacity Planning (4 hours). Module 8: Security and Compliance (4 hours). Total: 32 hours. Assessment: Practical exam requiring trainee to manage a simulated resource manager deployment through normal operations and recovery scenarios.

### 50.2 Developer Training Module
Module 1: Resource Model and Allocation API (2 hours). Module 2: Requesting Resources in Application Code (2 hours). Module 3: Understanding Quotas and Limits (2 hours). Module 4: Resource Optimization Best Practices (2 hours). Module 5: Monitoring Application Resource Usage (2 hours). Module 6: Responding to Resource Events (2 hours). Total: 12 hours. Assessment: Developer must instrument an application with resource requests and demonstrate proper handling of quota errors and resource pressure events.

## 51. Resource Manager Executive Summary

### 51.1 Business Value Proposition
The AIOS Resource Manager delivers significant business value: Resource efficiency: oversubscription ratios of 1.2x-1.5x increase resource utilization from typical 40-50% to 70-85%, reducing infrastructure costs by 30-40%. Automated defragmentation and bin-packing optimize resource packing for maximum utilization. Cost visibility: per-agent, per-service, per-team resource accounting enables accurate chargeback and cost allocation. Integration with AIOS Cost Manager provides complete cost visibility from resource consumption to financial chargeback. Operational reliability: resource pressure handling prevents cascading failures by preempting low-priority workloads before OOM or CPU starvation affects critical services. Guaranteed reservations ensure critical workloads always have resources. Developer productivity: simple allocation API abstracts complex resource management (cgroup configuration, NUMA topology, GPU partitioning) from developers. Automatic enforcement prevents resource abuse without developer intervention.

### 51.2 Key Performance Indicators
Resource manager KPIs tracked for business reporting: Resource utilization: average CPU utilization across cluster (target > 65%), average memory utilization (target > 75%), average GPU utilization (target > 80%). Oversubscription efficiency: achieved oversubscription ratio vs. target ratio. Allocation success rate: percentage of allocation requests that succeed. Resource pressure events: number of OOM, throttling, and preemption events per week. Resource accounting accuracy: variance between recorded consumption and actual usage (target < 1%). Cost savings: infrastructure cost savings from improved utilization and oversubscription.

### 51.3 Strategic Roadmap
Resource manager strategic roadmap for next 12-18 months: Phase 1 (months 1-3): CPU and memory resource tracking and allocation. Hard and soft quota enforcement with cgroup v2. Bin-packing and spread scheduling strategies. Basic resource metrics collection with USE methodology. Phase 2 (months 4-6): GPU resource tracking and allocation. Topology-aware scheduling with NUMA and GPU NVLink awareness. Oversubscription management with configurable ratios. Resource pressure monitoring and alerting. Phase 3 (months 7-9): Cost-aware scheduling with AIOS Cost Manager integration. Cross-cluster resource federation. Network bandwidth tracking and QoS enforcement. Defragmentation controller with automated scheduling. Phase 4 (months 10-12): Reservation system with priority and preemption. Automated resource reclamation for idle allocations. Predictive resource pressure detection using ML. Enhanced accounting with chargeback integration. Phase 5 (months 13-18): Autonomous resource management with self-tuning oversubscription ratios. ML-based workload placement optimization. Edge resource management for distributed deployments. Cross-cloud resource federation for hybrid and multi-cloud deployments.

## 52. Resource Manager References

### 52.1 Internal References
AIOS Configuration System: for resource policy and allocation configuration. AIOS Metrics System: for resource metric emission and monitoring integration. AIOS Logging System: for structured resource management logging. AIOS Tracing System: for allocation operation trace spans. AIOS Cost Manager: for resource cost tracking and chargeback integration. AIOS Orchestrator: for container lifecycle and migration coordination. AIOS Service Discovery: for compute node discovery and health tracking. AIOS Secret Manager: for node authentication credentials.

### 52.2 External References
Linux cgroups v2 Documentation: https://docs.kernel.org/admin-guide/cgroup-v2.html - Linux control groups v2 documentation for CPU, memory, IO resource enforcement. Linux Traffic Control Documentation: https://tldp.org/HOWTO/Traffic-Control-HOWTO/ - Linux traffic control documentation for network bandwidth management. NVIDIA NVML Documentation: https://developer.nvidia.com/nvidia-management-library-nvml - NVIDIA Management Library for GPU monitoring and management. Prometheus Documentation: https://prometheus.io/docs/ - Prometheus time-series database documentation for metric collection and alerting. Kubernetes Resource Management: https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/ - Kubernetes resource management concepts for container resource allocation. USE Method: https://www.brendangregg.com/usemethod.html - Brendan Gregg's USE methodology for resource monitoring. cgroup v2 Pressure Stall Information: https://docs.kernel.org/accounting/psi.html - Linux PSI documentation for resource pressure monitoring.

### 52.3 Standards and Specifications
CPU allocation: Linux CFS (Completely Fair Scheduler) shares and quota as defined in Linux kernel documentation. Memory allocation: Linux memory controller cgroup v2 min/low/high/max limits as defined in Linux kernel documentation. IO allocation: Linux IO controller cgroup v2 IO weight and IO max as defined in Linux kernel documentation. Network QoS: Linux traffic control (tc) HTB (Hierarchical Token Bucket) and TBF (Token Bucket Filter) as defined in Linux networking documentation. GPU monitoring: NVIDIA Management Library (NVML) as defined in NVIDIA developer documentation. Resource accounting: IEEE standard for financial chargeback and showback for IT services.

## 53. Resource Manager Appendix A: cgroup Configuration Reference

### 53.1 CPU cgroup Configuration
CPU quota configuration: /sys/fs/cgroup/cpu/{group}/cpu.cfs_quota_us: CPU time in microseconds per period (e.g., 50000 = 0.5 core). /sys/fs/cgroup/cpu/{group}/cpu.cfs_period_us: Period in microseconds (default 100000 = 100ms). /sys/fs/cgroup/cpu/{group}/cpu.shares: CPU weight for proportional distribution (default 1024). CPU scheduling configuration: Set quota for hard CPU limit: echo 50000 > cpu.cfs_quota_us (limits to 0.5 core). Set shares for soft CPU limit: echo 2048 > cpu.shares (double weight for proportional allocation). Set cpuset cpus for CPU pinning: echo 0-3 > /sys/fs/cgroup/cpuset/{group}/cpuset.cpus. Set cpuset mems for NUMA binding: echo 0 > /sys/fs/cgroup/cpuset/{group}/cpuset.mems. Read CPU usage: cat cpuacct.usage (total CPU time consumed in nanoseconds). Read CPU stats: cat cpu.stat (throttled time, number of periods, number of throttled periods).

### 53.2 Memory cgroup Configuration
Memory limit configuration: /sys/fs/cgroup/memory/{group}/memory.limit_in_bytes: Hard memory limit in bytes. /sys/fs/cgroup/memory/{group}/memory.soft_limit_in_bytes: Soft memory limit in bytes (best-effort enforcement). /sys/fs/cgroup/memory/{group}/memory.memsw.limit_in_bytes: Memory + swap hard limit. /sys/fs/cgroup/memory/{group}/memory.oom_control: OOM killer control (0 = enabled, 1 = disabled, task will be frozen). Memory pressure configuration: Set hard memory limit: echo 2147483648 > memory.limit_in_bytes (2GB hard limit). Set soft memory limit: echo 1610612736 > memory.soft_limit_in_bytes (1.5GB soft limit). Disable swap: echo 2147483648 > memory.memsw.limit_in_bytes (same as memory limit to disable swap). Enable OOM killer: echo 0 > memory.oom_control. Read memory usage: cat memory.usage_in_bytes (current memory usage). Read memory max usage: cat memory.max_usage_in_bytes (peak memory usage). Read memory stats: cat memory.stat (detailed memory breakdown by type).

### 53.3 IO cgroup Configuration
IO limit configuration (cgroup v2): /sys/fs/cgroup/{group}/io.max: IO limits in format (major:minor) rbps=value wbps=value riops=value wiops=value. /sys/fs/cgroup/{group}/io.weight: IO weight for proportional distribution (1-10000). IO limit setup: Set read IOPS limit: echo "8:0 riops=10000" > io.max (10000 read IOPS on device 8:0). Set write IOPS limit: echo "8:0 wiops=20000" > io.max (20000 write IOPS). Set read bandwidth limit: echo "8:0 rbps=104857600" > io.max (100 MB/s read). Set write bandwidth limit: echo "8:0 wbps=209715200" > io.max (200 MB/s write). Set IO weight: echo 500 > io.weight (default is 100). Read IO stats: cat io.stat (IO usage statistics by device).

### 53.4 Network Traffic Control Configuration
Network bandwidth limit setup: Create HTB qdisc: tc qdisc add dev eth0 root handle 1: htb default 30. Create class with rate limit: tc class add dev eth0 parent 1: classid 1:1 htb rate 100mbit ceil 200mbit burst 15k. Create cgroup filter: tc filter add dev eth0 parent 1: protocol ip handle 1: cgroup. Assign task to class: echo 0x10001 > /sys/fs/cgroup/net_prio/{group}/net_prio.ifpriomap. Read network stats: tc -s class show dev eth0 (class statistics). Monitor network usage: tc -s qdisc show dev eth0 (qdisc statistics).

### 53.5 GPU Resource Isolation
NVIDIA GPU isolation via MIG (Multi-Instance GPU): Create MIG partition: nvidia-smi mig -cgi 19,19,19 -C (create 3 MIG instances with 2GB each on A100). Assign GPU to container: --gpus '"device=0:0"' (assign specific MIG instance). Set GPU memory limit via NVIDIA container runtime: NVIDIA_VISIBLE_DEVICES=0:0 NVIDIA_MIG_CONFIG_DEVICES=all. GPU monitoring: nvidia-smi dmon -s pucvmet (GPU monitoring). GPU process tracking: nvidia-smi pmon -s u (process-level GPU usage). GPU XID error monitoring: nvidia-smi -q -d PFA (XID error monitoring).

## 54. Resource Manager Appendix B: Monitoring and Alerting Configuration

### 54.1 Prometheus Metric Definitions
Resource manager Prometheus metrics: resource_manager_allocations_total (counter, labels: status, resource_type): Total allocation operations. resource_manager_allocations_current (gauge, labels: status): Current allocation count by status. resource_manager_allocation_latency_ms (histogram, labels: strategy): Allocation decision latency. resource_manager_quota_consumption (gauge, labels: scope, resource_type): Current quota consumption. resource_manager_quota_limit (gauge, labels: scope, resource_type): Quota hard limit. resource_manager_reservation_count (gauge, labels: status): Reservation count by status. resource_manager_node_cpu_utilization (gauge, labels: node): Node CPU utilization. resource_manager_node_memory_utilization (gauge, labels: node): Node memory utilization. resource_manager_node_gpu_utilization (gauge, labels: node): Node GPU utilization. resource_manager_oversubscription_ratio (gauge, labels: node, resource_type): Current oversubscription ratio. resource_manager_fragmentation_score (gauge, labels: node): Resource fragmentation score. resource_manager_pressure_events_total (counter, labels: type, severity): Resource pressure event count. resource_manager_preemption_count (counter, labels: priority): Preemption event count.

### 54.2 Grafana Dashboard Definitions
Resource Manager main dashboard panels: Cluster overview: total nodes, total allocations, resource utilization by type (CPU, memory, GPU, storage, network). Node detail: per-node resource utilization, allocation count, fragmentation score, pressure events. Allocation tracking: active allocations by service/team/department, allocation growth trend, allocation latency distribution. Quota management: quota consumption by scope, quota violations timeline, quota modification history. Oversubscription tracking: oversubscription ratio by resource type, oversubscription breach events. Reservation tracking: active reservations by priority, reservation utilization, reservation expiration timeline. Resource accounting: cost by team/department, resource consumption trends, chargeback summary.

### 54.3 Alert Manager Configuration
Alert manager rules for Resource Manager: Critical alerts: OOM kill event (pager duty, immediate). Node CPU pressure > 0.5 for 5 minutes (pager duty). Node memory pressure > 0.5 for 5 minutes (pager duty). Oversubscription ratio > max_ratio for 5 minutes (pager duty). Major alerts: Node CPU utilization > 85% for 10 minutes (email + slack). Node memory utilization > 90% for 5 minutes (email + slack). Allocation failure rate > 5% for 5 minutes (email + slack). Reservation utilization < 50% for 7 days (email). Warning alerts: Quota consumption > 80% (email). Fragmentation score > 0.4 (email, weekly digest). Resource reclaim recommendation available (email, weekly digest). Capacity planning forecast shows exhaustion within 90 days (email).

## 55. Resource Manager Appendix C: Disaster Recovery Checklists

### 55.1 Complete RMS Failure Recovery Checklist
Prepare: Verify database backup availability (PostgreSQL allocation state). Verify node agent connectivity (agents are independent and still enforcing). Notify stakeholders of allocation service degradation. Restore: Deploy new RMS instances from backup configuration. Connect RMS to existing PostgreSQL database. Load allocation state from database. Reconcile: Query all node agents for current allocation state. Compare agent-reported state with database state. Resolve discrepancies (prefer agent-reported state for existing allocations). Verify: Test allocation API (GET allocations, check quota consumption). Test allocation requests (create test allocation, verify enforcement). Monitor for 30 minutes to confirm stability. Resume: Re-enable full allocation processing. Notify stakeholders of recovery completion.

### 55.2 Resource Exhaustion Emergency Procedure
Detect: Resource exhaustion alert (allocation failures > 90% for 5 minutes). Assess: Determine which resources are exhausted (CPU, memory, GPU, storage). Identify affected allocation requests (service, priority, team). Mitigate: Preempt best-effort and low-priority allocations to free resources. Reduce oversubscription ratio to 1.0 (disable overcommitment). Temporarily reduce resource profiles for non-critical services. Scale: Add compute nodes if possible (cloud auto-scaling, on-premise commissioning). Request emergency capacity from infrastructure team. Recover: Gradually restore preempted workloads (highest priority first). Restore oversubscription ratios to normal values. Validate that resource utilization returns to sustainable levels. Prevent: Implement earlier capacity planning warnings. Add predictive resource exhaustion detection. Establish emergency capacity reserve.

### 55.3 Accounting Data Loss Recovery Checklist
Detect: Accounting reconciliation alert (variance > 1% between recorded and actual usage). Assess: Determine scope of data loss (time range, resource types, affected allocations). Identify root cause (database failure, collection pipeline failure, configuration error). Recover: Restore accounting data from backup if available. Recompute accounting data from node agent historical metrics (if available). For periods without data, estimate consumption based on allocation profile and typical utilization patterns. Verify: Compare restored/recomputed accounting data with known usage patterns. Validate chargeback calculations with affected teams. Prevent: Improve accounting data backup frequency. Add real-time accounting data replication. Implement accounting data validation checks. Enhance monitoring for accounting pipeline health.

## 56. Resource Manager Document Control
Document version: 1.0.0. Last updated: 2026-07-10. Author: AIOS Platform Engineering. Approver: Chief Architect. Document status: APPROVED. Review cadence: Quarterly. Next review date: 2026-10-10. Document location: AIOS documentation repository. Distribution: Internal AIOS engineering teams only. References: AIOS Cost Manager Specification, AIOS Orchestrator Specification, AIOS Configuration System Specification. Related documents: AIOS Capacity Planning Guide, AIOS Resource Governance Policy, AIOS Node Deployment Procedure.

## 57. End of Document
This concludes the AIOS Resource Manager Engineering Specification version 1.0.0.
Compliance with all specifications is mandatory for production deployments.
Resource administration requires AIOS platform engineering team approval for all configuration changes to production resource manager deployments. All allocation, quota, and reservation changes are logged and audited. Compliance with resource governance policies is mandatory for all teams using the AIOS platform.

Resource manager specification version 1.0.0 approved by AIOS Architecture Review Board.

Resource Manager Appendix D: Capacity Planning Worksheets
CPU capacity: total_cores = sum(node_available_cores), reserved_cores = sum(reservation_cores),
available_cores = total_cores - reserved_cores, usable_cores = available_cores * oversubscription_ratio.
Memory capacity: total_memory = sum(node_available_memory), reserved_memory = sum(reservation_memory),
available_memory = total_memory - reserved_memory, usable_memory = available_memory * oversubscription_ratio.
GPU capacity: total_gpus = sum(node_available_gpus), reserved_gpus = sum(reservation_gpus),
available_gpus = total_gpus - reserved_gpus (GPU oversubscription not recommended for production).
Storage capacity: total_iops = sum(node_available_iops), total_capacity = sum(node_available_capacity_gb).
Network capacity: total_bandwidth = sum(node_available_bandwidth_bps).
Growth projections: monthly_growth_rate = avg(increase_over_last_3_months).
Exhaustion date: projected_date = today + (capacity_remaining / monthly_growth_rate).
End of Resource Manager Appendix D.
These worksheets provide the foundation for capacity planning and infrastructure budgeting across the AIOS platform.

All capacity planning decisions must be reviewed and approved by the AIOS capacity planning committee.

Capacity planning worksheets updated quarterly based on actual usage trends and business growth forecasts.
