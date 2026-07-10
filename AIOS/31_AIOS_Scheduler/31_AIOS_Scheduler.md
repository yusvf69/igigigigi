# AIOS Engineering Specification
## Document 31: AIOS Scheduler

### Version: 1.0.0
### Status: DRAFT
### Last Updated: 2026-07-10

---

## TABLE OF CONTENTS

1. Purpose
2. Mission
3. Responsibilities
4. Non-Responsibilities
5. Architecture
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

The AIOS Scheduler is the central orchestration component responsible for deciding which agent executes which task at which time on which compute resource. It ensures that all tasks across the AIOS ecosystem are scheduled according to policy, priority, resource availability, and temporal constraints. The scheduler is the intelligence layer between task submission and task execution, optimizing for throughput, latency, fairness, and reliability across the entire operating system.

## 2. Mission

To provide a deterministic, policy-driven scheduling substrate that guarantees timely execution of critical agent tasks while maximizing cluster-wide resource utilization, maintaining fairness across tenants and departments, and supporting real-time deadlines for time-sensitive operations.

---

## 3. Responsibilities

### 3.1 Core Scheduling
- Accept task scheduling requests from all AIOS components
- Maintain per-priority scheduling queues with strict ordering
- Dispatch ready tasks to the Execution Engine based on policy
- Track task lifecycle from submission to completion or failure

### 3.2 Policy Enforcement
- Apply FIFO scheduling for non-prioritized workloads
- Enforce priority-based scheduling with P0 through P4 levels
- Support round-robin scheduling across equal-priority agents
- Implement weighted-fair scheduling for multi-tenant fairness
- Provide deadline-aware scheduling using EDF algorithm
- Support dynamic policy switching based on cluster state

### 3.3 Priority Management
- Define and maintain five distinct priority tiers
- Implement priority inheritance for dependency chains
- Detect and prevent priority inversion scenarios
- Support dynamic priority adjustment (aging)
- Provide priority escalation paths for critical chains

### 3.4 Queue Management
- Maintain per-priority FIFO queues
- Support per-agent-type scheduling queues
- Provide per-department queue isolation
- Implement gang scheduling queues for workflow groups
- Manage delay and dead-letter queues for failed scheduling

### 3.5 Load Management
- Monitor compute node utilization across the cluster
- Schedule tasks on least-loaded nodes when policy permits
- Enforce per-node and per-tenant concurrency limits
- Implement backpressure signaling when nodes are saturated
- Provide load shedding under extreme conditions

### 3.6 Deadline Management
- Track task submission deadlines and execution duration estimates
- Implement earliest-deadline-first ordering for time-sensitive tasks
- Monitor deadline miss rates and trigger escalation procedures
- Provide deadline-aware admission control
- Support soft deadlines (warning) and hard deadlines (failure)

### 3.7 Affinity Management
- Support data-locality-aware scheduling (schedule near data)
- Implement NUMA-aware agent placement
- Support cache-affinity scheduling for hot agents
- Provide node-affinity constraints for specialized hardware
- Implement anti-affinity for failure domain separation

### 3.8 Coordination
- Coordinate with Execution Engine for task dispatching
- Interface with Queue System for reliable message delivery
- Provide scheduling feedback to workflow engine
- Synchronize with cluster state management for node health
- Coordinate with load balancer for optimal distribution

### 3.9 Metrics and Observability
- Track scheduling latency, throughput, and queue depth
- Monitor scheduling decision quality
- Report deadline miss rates and scheduling failures
- Expose scheduler state for debugging and visualization
- Provide scheduling analytics for capacity planning

---

## 4. Non-Responsibilities

- The Scheduler does NOT execute tasks; execution is delegated to the Execution Engine
- The Scheduler does NOT manage compute resources directly; resource management is handled by the kernel
- The Scheduler does NOT provide permanent storage; persistence is delegated to the Persistence Layer
- The Scheduler does NOT perform authentication; identity is verified before scheduling requests
- The Scheduler does NOT implement business logic; it only schedules tasks according to policy
- The Scheduler does NOT manage network routing; network concerns are handled separately
- The Scheduler does NOT provide data replication; data is managed by the data layer
- The Scheduler does NOT handle agent registration; agents register through the Agent Registry

---

## 5. Architecture

### 5.1 High-Level Architecture

The AIOS Scheduler follows a three-tier architecture:

```
┌─────────────────────────────────────────────────────────────┐
│                     Scheduler API Layer                      │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────┐      │
│  │ Schedule API │  │  Policy API  │  │  Monitoring API │      │
│  └──────┬───────┘  └──────┬──────┘  └────────┬────────┘      │
└─────────┼──────────────────┼───────────────────┼──────────────┘
          │                  │                   │
┌─────────┼──────────────────┼───────────────────┼──────────────┐
│         ▼                  ▼                   ▼               │
│                     Scheduler Core Layer                       │
│  ┌─────────────────────────────────────────────────────┐    │
│  │              Scheduler Dispatcher                     │    │
│  └──────────┬────────────────────────────────┬────────┘    │
│            │                                    │              │
│  ┌─────────▼─────────┐          ┌─────────────▼───────────┐  │
│  │   Policy Engine    │          │   Queue Manager          │  │
│  │  - FIFO Policy    │          │  - Priority Queues      │  │
│  │  - Priority Policy│          │  - Gang Queues          │  │
│  │  - Round Robin    │          │  - Affinity Queues      │  │
│  │  - Weighted Fair   │          │  - Delay/Dead Letter    │  │
│  │  - EDF Scheduler  │          │  - Backpressure Ctrl    │  │
│  └───────────────────┘          └─────────────────────────┘  │
│                                                              │
│  ┌───────────────────┐          ┌─────────────────────────┐  │
│  │  Load Balancer    │          │  Affinity Manager        │  │
│  │  - Node Monitor   │          │  - Data Locality        │  │
│  │  - Load Scoring   │          │  - NUMA Placement       │  │
│  │  - Least-Loaded   │          │  - Cache Affinity       │  │
│  │  - Backpressure   │          │  - Anti-Affinity        │  │
│  └───────────────────┘          └─────────────────────────┘  │
│                                                              │
│  ┌───────────────────┐          ┌─────────────────────────┐  │
│  │  Priority Manager  │          │  Deadline Manager        │  │
│  │  - Inheritance    │          │  - EDF Ordering          │  │
│  │  - Inversion Prev │          │  - Deadline Tracking     │  │
│  │  - Aging           │          │  - Miss Detection        │  │
│  │  - Escalation     │          │  - Admission Control     │  │
│  └───────────────────┘          └─────────────────────────┘  │
└────────────────────────────────────────────────────────────┘
          │                  │                   │
┌─────────┼──────────────────┼───────────────────┼──────────────┐
│         ▼                  ▼                   ▼               │
│                   Scheduler Infrastructure Layer                │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────┐  │
│  │ State Store  │  │ Event Bus   │  │ Metrics Reporter    │  │
│  └─────────────┘  └─────────────┘  └─────────────────────┘  │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────┐  │
│  │ Lock Manager│  │ Trace Exporter│  │ Audit Recorder    │  │
│  └─────────────┘  └─────────────┘  └─────────────────────┘  │
└──────────────────────────────────────────────────────────────┘
```

### 5.2 Architecture Principles

1. **Stateless Decision**: Scheduling decisions can be recomputed from state
2. **Deterministic**: Same input always produces same scheduling decision
3. **Isolated Policy**: Each scheduling policy is independently pluggable
4. **Observable by Design**: Every decision is logged and measurable
5. **Fail-Closed**: On uncertainty, do not schedule; escalate to human
6. **Resource Aware**: All decisions consider current cluster state

---

## 6. Internal Components

### 6.1 Scheduler Dispatcher

The central routing component that receives scheduling requests and coordinates the scheduling pipeline:

```
Component: SchedulerDispatcher
Responsibilities:
  - Receive scheduling requests from API layer
  - Validate request completeness and permissions
  - Route to appropriate policy engine based on request metadata
  - Coordinate the scheduling pipeline stages
  - Return scheduling decisions to callers
  - Handle scheduling failures and escalations

Lifecycle:
  - Init: Register with API layer, load policies, initialize queues
  - Run: Accept requests, dispatch through pipeline, return decisions
  - Stop: Drain pending requests, persist state, unregister
  - Error: Log failure, attempt recovery or escalation

Configuration:
  - MaxConcurrentRequests: 1000
  - RequestTimeoutMs: 5000
  - PolicyEvaluationTimeoutMs: 1000
  - MaxScheduleAttempts: 3
```

### 6.2 Policy Engine

The policy engine evaluates which scheduling policy applies and executes it:

```
Component: Policy
Responsibilities:
  - Maintain registry of all available scheduling policies
  - Select appropriate policy based on task and cluster state
  - Execute policy algorithm to select target node
  - Support dynamic policy switching during operation
  - Validate policy configuration for correctness

Policies:
  - FIFO: Strict first-in-first-out ordering
  - Priority: Order by priority level, then by submission time
  - RoundRobin: Distribute evenly across ready nodes
  - WeightedFair: Proportional allocation based on weights
  - EDF: Earliest deadline first for time-sensitive tasks

Lifecycle:
  - Init: Load policy configurations, validate policies
  - Select: Determine active policy for this request
  - Execute: Run policy decision algorithm
  - Record: Log policy decision and reasoning

Configuration:
  - DefaultPolicy: "priority"
  - DynamicSwitchingEnabled: true
  - PolicyReevaluationPeriodMs: 60000
  - PolicyWeightRefreshPeriodMs: 300000
```

### 6.3 Queue Manager

Manages all scheduling queues and their lifecycles:

```
Component: QueueManager
Responsibilities:
  - Maintain per-priority FIFO queues (5 levels: P0-P4)
  - Maintain per-agent-type queues for affinity scheduling
  - Maintain per-department queues for tenant isolation
  - Maintain gang queues for workflow agent groups
  - Handle queue backpressure and load shedding
  - Manage delay queues for deferred scheduling
  - Cooperate with external Queue System for persistence

Queue Types:
  - PriorityQueue: Ordered by priority, then FIFO within level
  - GangQueue: Group of agents that must be scheduled together
  - AffinityQueue: Tasks with data locality constraints
  - DepartmentQueue: Isolated queue per department
  - DelayQueue: Tasks deferred to future time

Data Structures:
  - PriorityQueue: Max-heap per priority level
  - GangQueue: Set of task groups awaiting bundling
  - AffinityQueue: Map<NodeID, Queue<Task>>
  - DepartmentQueue: Map<DeptID, Queue<Task>>

Configuration:
  - MaxDepthPerQueue: 100000
  - BackpressureThresholdPercent: 80
  - LoadSheddingThresholdPercent: 95
  - QueueStalenessTimeoutMs: 3600000
```

### 6.4 Load Balancer

Monitors cluster load and scores nodes for scheduling:

```
Component: LoadBalancer
Responsibilities:
  - Maintain real-time load metrics for all compute nodes
  - Compute load scores for each node
  - Implement least-loaded selection strategy
  - Support weighted selection for heterogeneous clusters
  - Enforce per-node and per-tenant concurrency limits
  - Signal backpressure when nodes are saturated
  - Detect and exclude unhealthy nodes

Load Metrics:
  - CPU utilization (percentage, 1min/5min/15min avg)
  - Memory utilization (percentage, available bytes)
  - IOPS utilization (reads/writes per second)
  - Network utilization (bandwidth in/out)
  - Queue depth (pending tasks)
  - Active context switches per second
  - GC pressure (for managed runtime nodes)

Configuration:
  - LoadBalancingAlgorithm: least-loaded
  - NodeHealthCheckMs: 5000
  - NodeExcludeThresholdPercent: 90
  - ConcurrencyLimitDefault: 50
  - ConcurrencyLimitPerDept: 20
```

### 6.5 Affinity Manager

Manages scheduling affinity and data locality:

```
Component: AffinityManager
Responsibilities:
  - Track data location across the cluster
  - Compute data locality scores for node-task pairs
  - Support NUMA-aware placement decisions
  - Maintain cache affinity hints for hot agents
  - Enforce anti-affinity for failure domain separation
  - Provide soft affinity (preference) and hard affinity (requirement)

Affinity Types:
  - DataLocality: Schedule agent on node containing required data
  - NUMA: Schedule within same NUMA domain for memory locality
  - CacheAffinity: Schedule on node with warm caches
  - HardwareAffinity: Schedule on node with specific hardware (GPU, TPU)
  - AntiAffinity: Avoid scheduling on same node/rack/zone

Configuration:
  - DataLocalityWeight: 0.8
  - CacheAffinityWeight: 0.3
  - AntiAffinityEnforcement: "hard"
  - DataLocationCacheSize: 10000
  - DataLocationCacheTTLMs: 300000
```

### 6.6 Priority Manager

Manages priority levels, inheritance, inversion prevention, and aging:

```
Component: PriorityManager
Responsibilities:
  - Define priority levels P0 through P4
  - Track current priority of each task
  - Implement priority inheritance protocol
  - Detect priority inversion scenarios
  - Execute priority inversion prevention
  - Implement priority aging mechanism
  - Support dynamic priority adjustment

Priority Levels:
  - P0 (Critical): Real-time system operations, health checks, safety
  - P1 (High): User-facing requests, payment processing, SLA-bound tasks
  - P2 (Medium): Standard workflows, report generation
  - P3 (Low): Background processing, batch analytics
  - P4 (Background): Maintenance, garbage collection, cleanup

Priority Inheritance:
  - When high-priority task blocks on low-priority task:
    1. Detect dependency chain: T1(high) → resource → T2(low)
    2. Temporarily elevate T2 to T1's priority
    3. T2 executes at elevated priority until lock released
    4. After release, T2 returns to original priority
  - Inheritance depth: max 3 levels
  - Inheritance timeout: 30 seconds max elevation

Aging Mechanism:
  - Tasks waiting > 60s: priority increased by 1 level every 60s
  - Maximum age escalation: P4 → P0 in 5 steps
  - Aging resets after task begins execution
  - Aging recorded in task metadata

Configuration:
  - AgingIntervalMs: 60000
  - AgingStepLevels: 1
  - InheritanceMaxDepth: 3
  - InheritanceMaxDurationMs: 30000
  - PriorityBoostMax: 1 (from base priority)
```

### 6.7 Deadline Manager

Manages deadline-aware scheduling using EDF:

```
Component: DeadlineManager
Responsibilities:
  - Track submitted deadlines for all time-sensitive tasks
  - Maintain EDF ordering among deadline-bound tasks
  - Compute scheduling urgency scores
  - Detect approaching deadline misses
  - Escalate imminent misses to operations
  - Enforce hard deadline violations with task termination
  - Support soft deadlines (warning) and hard deadlines (failure)

EDF Algorithm:
  - Order tasks by earliest absolute deadline
  - Preempt running task if new task has earlier deadline
  - Preemption overhead accounted in scheduling decision
  - Soft deadline: warning at 80% of deadline duration
  - Hard deadline: termination at 100% of deadline duration

Configuration:
  - DeadlineTrackingEnabled: true
  - SoftDeadlineThreshold: 0.8
  - HardDeadlineThreshold: 1.0
  - PreemptionEnabled: true
  - PreemptionOverheadMs: 50
  - EscalationThresholdMs: 300000
```

---

## 7. External Components

### 7.1 Execution Engine
- Receives scheduled tasks for execution
- Reports task completion and failures
- Provides resource usage feedback
- Signals preemption events

### 7.2 Queue System
- Provides persistent queue infrastructure
- Supports delay, retry, and dead-letter queues
- Guarantees at-least-once delivery

### 7.3 Agent Registry
- Provides agent type metadata
- Reports agent health status
- Supplies agent capability information

### 7.4 Cluster State Manager
- Provides cluster topology information
- Reports node health and availability
- Manages node membership

### 7.5 Workflow Engine
- Submits workflow agent tasks for scheduling
- Provides workflow-level deadlines
- Requests gang scheduling for workflow agents

### 7.6 Monitoring Service
- Collects scheduling metrics
- Tracks scheduler health
- Generates scheduling alerts

### 7.7 Authentication Service
- Verifies scheduling request authenticity
- Provides identity context for authorization

---

## 8. Dependencies

| Dependency | Type | Criticality | Description |
|---|---|---|---|
| Cluster State Store | Internal | CRITICAL | Node health and topology |
| Queue System | Internal | CRITICAL | Task queue persistence |
| Execution Engine | Internal | CRITICAL | Task execution |
| Event Bus | Internal | HIGH | Async event delivery |
| Codebase | Internal | HIGH | Static policy definitions |
| Agent Registry | Internal | HIGH | Agent metadata |
| Authentication Service | External | HIGH | Request authentication |
| Monitoring System | Internal | HIGH | Metrics collection |
| Workflow | Internal | MEDIUM | Workflow task scheduling |
| Secret Store | External | LOW | Nothing needed |

---

## 9. Inputs

### 9.1 Scheduling Request
```yaml
Input: ScheduleTaskRequest
Source: API Layer (from any authenticated component)
Schema:
  TaskID: string (UUID v4, required)
  AgentType: string (required)
  Priority: enum [P0, P1, P2, P3, P4] (required)
  Department: string (required)
  Deadline: timestamp (optional)
  EstimatedDurationMs: integer (recommended)
  DataLocationHints: string[] (optional)
  AffinityConstraints: object (optional)
  GangGroupID: string (optional)
  WorkflowID: string (optional)
  Metadata: map<string,string> (optional)
```

### 9.2 Policy Configuration
```
Schema: PolicyConfig
Source: Configuration System
Fields:
  ActivePolicy: string
  PolicyParameters: map<string,any>
  PriorityWeights: map<string,float>
  DepartmentWeights: map<string,float>
  QueueLimits: map<string,int>
  AgingEnabled: boolean
```

### 9.3 Cluster State
```
Schema: ClusterStateSnapshot
Source: Cluster State Manager
Fields:
  Nodes: [NodeStatus]
    NodeID: string
    LoadPercentage: float
    AvailableMemory: int
    AvailableCPU: int
    AvailableIOPS: int
    ActiveTasks: int
    IsHealthy: bool
  NodeTopology: map<string, NodeInfo>
  DataLocations: map<string, string[]>
```

---

## 10. Outputs

### 10.1 Scheduling Decision
```
{
  TaskId: "uuid-1234",
  Decision: "SCHEDULED" | "DEFERRED" | "REJECTED" | "FAILED",
  ScheduledNode: "node-42",
  ScheduledTime: 1720612345000,
  EstimatedStartTime: 1720612346000,
  EstimatedCompletionTime: 1720612349000,
  Reason: "Least-loaded node with data locality",
  PolicyUsed: "priority",
  PriorityAssigned: "P1",
  QueuePosition: 3,
  DeadlineRisk: "LOW"
}
```

### 10.2 Scheduling Rejection
```
{
  TaskId: "uuid-1234",
  Status: "REJECTED",
  Reason: "All nodes saturated",
  ProposedRetryAtMs: 1720613000000,
  AlternativeSuggestion: "Switch to batch priority for expedited processing"
}
```

---

## 11. Data Structures

### 11.1 Task Scheduling Record
```typescript
interface TaskSchedule {
  taskId: string;
  workflowId?: string;
  agentType: string;
  priority: Priority;
  basePriority: Priority;
  departmentId: string;
  status: TaskScheduleStatus;
  submitTime: number;
  scheduledTime?: number;
  startTime?: number;
  completionTime?: number;
  deadline?: number;
  deadlineType: 'SOFT' | 'HARD';
  nodeId?: string;
  state: TaskState;
  resourceDemand: ResourceDemand;
  affinityHints: AffinityHint[];
  gangGroupId?: string;
  parentTaskId?: string;
  dependencyChain: string[];
  ageSteps: number;
  isInheritedPriority: boolean;
  inheritedFromTaskId?: string;
  schedulingAttempt: number;
  lastSchedulingError?: string;
}

enum TaskScheduleStatus {
  PENDING = 'PENDING',
  QUEUED = 'QUEUED',
  SCHEDULED = 'SCHEDULED',
  RUNNING = 'RUNNING',
  COMPLETED = 'COMPLETED',
  FAILED = 'FAILED',
  CANCELLED = 'CANCELLED',
  DEFERRED = 'DEFERRED',
}

enum Priority {
  P0 = 0,
  P1 = 1,
  P2 = 2,
  P3 = 3,
  P4 = 4,
}
```

### 11.2 Node Score
```typescript
interface NodeScore {
  nodeId: string;
  loadScore: number;
  dataLocalityScore: number;
  affinityScore: number;
  totalScore: number;
  reason: string;
  details: {
    cpuUtilization: number;
    memoryUtilization: number;
    iopsUtilization: number;
    networkUtilization: number;
    activeTaskCount: number;
    dataDistance: number;
    cacheTemperature: number;
  };
}
```

### 11.3 Schedule Policy
```typescript
interface SchedulePolicy {
  name: string;
  type: PolicyType;
  isActive: boolean;
  weight: number;
  config: Record<string, any>;
  selector: (tasks: TaskScheduling[], nodes: NodeScore[]) => NodeScore[];
}

enum PolicyType {
  FIFO = 'FIFO',
  PRIORITY = 'PRIORITY',
  ROUND_ROBIN = 'ROUND_ROBIN',
  WEIGHTED_FAIR = 'WEIGHTED_FAIR',
  EDF = 'EDF',
  HYBRID = 'HYBRID',
}
```

### 11.4 Scheduling Queue
```typescript
interface SchedulingQueue {
  queueId: string;
  queueType: QueueType;
  priorityLevel: Priority;
  owner?: string; // departmentId or agentType or gangGroupId
  tasks: TaskRecord[];
  depth: number;
  maxDepth: number;
  backpressureActive: boolean;
  metrics: QueueMetrics;
}

enum QueueType {
  PER_PRIORITY = 'PER_PRIORITY',
  PER_AGENT = 'PER_AGENT',
  PER_DEPARTMENT = 'PER_DEPARTMENT',
  GANG = 'GANG',
  DELAY = 'DELAY',
  DEAD_LETTER = 'DEAD_LETTER',
}
```

### 11.5 Gang Scheduling Group
```typescript
interface GangGroup {
  gangId: string;
  workflowId: string;
  memberTasks: string[];
  allMembersReady: boolean;
  requiredNode: string;
  status: GangStatus;
  formationDeadline: number;
  dispatchTimeout: number;
}

enum GangStatus {
  FORMING = 'FORMING',
  READY = 'READY',
  DISPATCHED = 'DISPATCHED',
  PARTIAL_FAILURE = 'PARTIAL_FAILURE',
  TIMEOUT = 'TIMEOUT',
}
```

---

## 12. Execution Flow

### 12.1 Primary Scheduling Flow

```
STEP 1: RECEIVE REQUEST
  - Valid request submitted via Schedule API
  - Validate request: permissions, format, completeness
  - Record in audit log
  → If invalid: Return REJECTED with error details

STEP 2: DETERMINE POLICY
  - Check task metadata for policy hints
  - Check active system policy
  - For deadline tasks: override to EDF
  - For P0 tasks: override to priority preemptive
  → Policy selected: FIFO | Priority | RR | WFair | EDF

STEP 3: ENQUEUE TASK
  - Determine appropriate queue:
    - Per priority level → PriorityQueue[priority]
    - Gang scheduling → GangQueue[gangId]
    - Department isolation → DeptQueue[departmentId]
    - Deadline task → EDFQueue (ordered by deadline)
  - If queue at capacity → Backpressure or Shedding
  - Assign queue position and estimated wait time
  → Task inserted into scheduling queue

STEP 4: SCHEDULING (DEQUEUE LOOP)
  - Dequeue from: Priority order → Wait time → Deadline order
  - For gang scheduling: Wait until all gang members ready
  - For delay scheduling: Wait until delay time expires

STEP 5: NODE SELECTION
  - Get cluster state from Cluster State Manager
  - Apply load balancing: score all available nodes
  - Apply affinity constraints: filter/score by affinity
  - Apply anti-affinity constraints: exclude forbidden nodes
  - Select best candidate: highest total score
  - If no candidate → DEFER or REJECT

STEP 6: SCHEDULING DECISION
  - Assign task to selected node
  - Update task state: QUEUED → SCHEDULED
  - Record scheduling metadata
  - Emit TaskScheduleEvent
  - Return decision to caller

STEP 7: DISPATCH
  - Send task to Execution Engine on selected node
  - Track task execution
  - Monitor for preemption or completion
  - On completion: record metrics, update state

STEP 8: POST-SCHEDULING
  - Update per-node and per-priority load counters
  - Adjust queue positions of remaining tasks
  - Check for aging promotions
  - Report metrics
```

### 12.2 Preemption Flow

```
STEP 1: PREEMPTION TRIGGER
  - New P0 task arrives and all P0 slots busy
  - EDF task with earlier deadline than running task
  - Priority inheritance triggers escalation

STEP 2: VICTIM SELECTION
  - Select running task with lowest priority
  - Check preemption safety: is task in critical section?
  - If in critical section: wait for exit or timeout

STEP 3: SUSPEND VICTIM
  - Signal Execution Engine to suspend victim task
  - Capture task state for later resume
  - Set victim status: RUNNING → SUSPENDED
  - Record preemption reason

STEP 4: SCHEDULE PREEMPTOR
  - Dispatch high-priority task to freed node
  - Start execution immediately
  - Set status: SCHEDULED → RUNNING
  - Record preemption in audit

STEP 5: RESUME VICTIM
  - When preemptor completes or blocks:
  - Restore victim task state
  - Re-enqueue victim at head of priority queue
  - Set status: SUSPENDED → QUEUED (head)
```

### 12.3 Gang Scheduling Flow

```
STEP 1: GANG FORMATION
  - First gang member arrives → create GangGroup
  - Each subsequent member joins GangGroup
  - Track all required members for this workflow step
  - Set deadline for gang formation

STEP 2: GANG READINESS CHECK
  - When all members present OR timeout approaching:
    - Check cluster capacity for gang dispatch
    - Need enough resources on single node (or coordinated nodes)

STEP 3: GANG DISPATCH
  - Select node with capacity for all gang members
  - If no single node: gang is distributed with latency penalty
  - All members dispatched together
  - Gang enters RUNNING state

STEP 4: GANG COORDINATION
  - All members execute concurrently
  - Synchronization barriers between gang members
  - If any member fails → gang is PARTIAL_FAILURE

STEP 5: GANG COMPLETION
  - All members complete → gang COMPLETED
  - If partial failure → retry or escalate
  - Trigger next workflow step
```

### 12.4 Priority Inheritance Flow

```
STEP 1: LOCK CONTENTION DETECTION
  - P0 task T1 requests lock held by P4 task T2
  - Scheduler detects: T1 → waiting → T2 → running
  - Inversion scenario confirmed

STEP 2: INHERITANCE ACTIVATION
  - Temporarily elevate T2 priority from P4 to P1 (T1's level)
  - T2 preempts any other P4 tasks on its node
  - T2 gets priority boost in scheduler queues

STEP 3: LOCK RELEASE MONITORING
  - Monitor T2 for lock release
  - Timeout: max 30s inheritance duration

STEP 4: RESTORATION
  - On lock release: T2 priority returns to P4
  - T1 acquires lock and continues
  - Inheritance recorded in audit log
```

---

## 13. State Management

### 13.1 Scheduler State

The scheduler maintains the following state, persisted in the State Store:

```
State Schema:
  SchedulingState:
    Queues: map<string, SchedulingQueue>
    ActiveTasks: map<string, TaskRecord>
    PendingGangs: map<string, GangGroup>
    PeakBalance: map<string, NodeLoadRecord>
    PolicyCache: map<string, any>
    
  Persistence:
    All state written to distributed State Store (etcd/Consul)
    State snapshots taken every 60s for recovery
    State versioned with monotonic counter
    Partial updates: update affected queue only
```

### 13.2 State Transitions

```
Task State Machine:
  PENDING → QUEUED → SCHEDULED → RUNNING → COMPLETED
                              → SUSPENDED → QUEUED
                              → RUNNING → FAILED
  PENDING → REJECTED
  QUEUED → DEFERRED → QUEUED (after delay)
  QUEUED → CANCEL → CANCELED
  RUNNING → CANCEL → CANCELED

Gang State Machine:
  FORMING → READY → DISPATCHED → COMPLETED
  FORMING → TIMEOUT → DEGRADED
  DISPATCHED → PARTIAL_FAILURE
```

---

## 14. Communication Protocols

### 14.1 Internal Communication

| Protocol | Transport | Format | Use Case |
|---|---|---|---|
| Scheduler Internal | Unix Domain Socket | Protobuf | Component-to-component calls |
| State Store | gRPC | Protobuf | State persistence and queries |
| Events | Async pub/sub | JSON | Schedule events |
| Metrics | UDP | StatsD format | Performance metrics |

### 14.2 External Communication

| Protocol | Transport | Format | Use Case |
|---|---|---|---|
| Schedule API | gRPC | Protobuf | External scheduling requests |
| Admin API | REST/HTTP | JSON | Configuration and queries |
| Monitoring API | gRPC stream | Protobuf | Real-time scheduler state |
| WebSocket | WSS | JSON | Real-time scheduler dashboard |

---

## 15. APIs

### 15.1 Schedule API (gRPC)

```
service SchedulerService {
  rpc ScheduleTask(ScheduleTaskRequest) returns (ScheduleTaskResponse);
  rpc CancelTask(CancelTaskRequest) returns (CancelTaskResponse);
  rpc GetTaskStatus(GetTaskStatusRequest) returns (GetTaskStatusResponse);
  rpc ListScheduledTasks(ListScheduledTasksRequest) returns (ListScheduledTasksResponse);
  rpc RescheduleTask(RescheduleTaskRequest) returns (RescheduleTaskResponse);
  rpc SetNodeAffinity(SetNodeAffinityRequest) returns (SetNodeAffinityResponse);
  rpc RegisterGangGroup(RegisterGangGroupRequest) returns (RegisterGangGroupResponse);
}
```

### 15.2 Admin API (REST)

```
GET    /api/v1/scheduler/status           - Scheduler health and state
GET    /api/v1/scheduler/config           - Active configuration
PUT    /api/v1/scheduler/config           - Update configuration
GET    /api/v1/scheduler/queues          - Queue depths and metrics
GET    /api/v1/scheduler/queues/:id      - Queue detail
DELETE /api/v1/scheduler/queues/:id/tasks/:taskId - Remove from queue
GET    /api/v1/scheduler/nodes           - Node load and scores
GET    /api/v1/scheduler/policies         - Active policies
PUT    /api/v1/scheduler/policies        - Switch active policy
GET    /api/v1/scheduler/metrics         - Performance metrics
POST   /api/v1/scheduler/rebalance       - Trigger manual rebalance
```

### 15.3 Monitoring API (gRPC Stream)

```
service SchedulerMonitor {
  rpc SubscribeScheduleEvents(ScheduleEventFilter) returns (stream ScheduleEvent);
  rpc SubscribeNodeLoadUpdates(Empty) returns (stream NodeLoadUpdate);
  rpc SubscribePriorityChanges(Empty) returns (stream PriorityChange);
}
```

---

## 16. Events

### 16.1 Published Events

| Event | Publisher | Payload | Consumers |
|---|---|---|---|
| TaskScheduled | Scheduler | TaskId, NodeId, Timestamp | Execution, Monitoring, Audit |
| TaskDeferred | Scheduler | TaskId, Delay, Reason | Queue, Monitoring |
| TaskCancelled | Scheduler | TaskId, Reason | Execution, Workflow |
| TaskPreempted | Scheduler | TaskId, PreemptorId, Reason | Execution, Monitoring |
| PriorityInherited | Scheduler | TaskId, OldPriority, NewPriority | Audit, Monitoring |
| PriorityAged | Scheduler | TaskId, OldPriority, NewPriority | Audit, Monitoring |
| DeadlineMissWarning | Scheduler | TaskId, Deadline, CurrentTime | Monitoring, Ops |
| NodeOverloaded | Scheduler | NodeId, LoadPercentage | Load Balancer, Ops |
| GangFormed | Scheduler | GangId, MemberCount | Workflow, Monitoring |
| RebalanceTriggered | Scheduler | Reason, Nodes[] | Load Balancer |

### 16.2 Subscribed Events

| Event | Publisher | Handler |
|---|---|---|
| TaskCompleted | Execution Engine | Mark task done, update metrics |
| TaskFailed | Execution Engine | Queue retry or dead letter |
| NodeDown | Cluster State | Drain node, reschedule tasks |
| NodeHealthy | Cluster State | Re-enable node for scheduling |
| PolicyChanged | Configuration | Reload policy engine |
| TaskRateLimitHit | Queue System | Apply backpressure |

---

## 17. Queues

### 17.1 Internal Scheduler Queues

| Queue Name | Type | Depth Limit | Persistence | Priority |
|---|---|---|---|---|
| priority.p0 | Per-priority FIFO | 100000 | In-memory + snapshot | P0 |
| priority.p1 | Per-priority FIFO | 100000 | In-memory + snapshot | P1 |
| priority.p2 | Per-priority FIFO | 100000 | In-memory + snapshot | P2 |
| priority.p3 | Per-priority FIFO | 100000 | In-memory + snapshot | P3 |
| priority.p4 | Per-priority FIFO | 100000 | In-memory + snapshot | P4 |
| gang.* | Per-gang group | 1000 | In-memory | Dynamic |
| department.* | Per-department | 50000 | In-memory + snapshot | Dynamic |
| agent.* | Per-agent-type | 50000 | In-memory + snapshot | Dynamic |
| delay | Time-based | 100000 | Persistent | N/A |
| dead-letter | Failure | 1000 | Persistent | N/A |

### 17.2 Queue Operation

```
Queue Operations:
  Enqueue(task, queueId) -> position, estimatedDelay
  Dequeue(queueId) -> task (or null if empty)
  Peek(queueId) -> task (non-destructive)
  Remove(taskId, queueId) -> boolean
  Reorder(queueId, taskId, newPosition) -> boolean
  GetDepth(queueId) -> integer
  GetMetrics(queueId) -> QueueMetrics
```

---

## 18. Caching

### 18.1 Cache Strategy

| Cache Name | Scope | TTL | Size | Eviction |
|---|---|---|---|---|
| Schedule Policy Cache | Per-scheduler | 5 min | 100 entries | LRU |
| Node Score Cache | Per-scheduler | 1s (stale=acceptable) | All nodes | Time-based |
| Data Location Cache | Per-scheduler | 5 min | 10000 entries | LRU |
| Affinity Cache | Per-scheduler | 10 min | 1000 entries | LRU |
| Task State Cache | Per-scheduler | Task lifetime | All active tasks | Time-based |
| Policy Decision Cache | Per-scheduler | 1 min | 500 entries | LRU |

### 18.2 Cache Invalidation

```
Rules:
  - Node score cache invalidated every 1s (accepting stale reads)
  - Data location cache invalidated on node movement events
  - Policy cache invalidated on policy change event
  - Task state cache is authoritative (no invalidation needed)
  - Affinity cache invalidated on agent restart
```

---

## 19. Memory

### 19.1 Memory Budget

| Component | Max Memory | Allocation | Notes |
|---|---|---|---|
| Queue State | 2 GB | 1GB P0, 500MB each P1-P4 | Configurable |
| Task Records | 1 GB | ~500 bytes per active task | ~2M tasks |
| Cache Data | 500 MB | Across all caches | Adjustable |
| Node State | 200 MB | ~2KB per node | For 100K nodes |
| Metadata & Index | 300 MB | Queues, policies, config | Fixed |
| **Total** | **2.5 GB** | | |

### 19.2 Memory Allocation Strategy

- Priority queues get guaranteed memory allocation
- P0 queue: 40% of queue memory budget
- P1 queue: 25% of queue memory budget
- P2 queue: 15% of queue memory budget
- P3 queue: 10% of queue memory budget
- P4 queue: 5% of queue memory budget
- Overflow: 5% reserved for overflow
- Cache memory is shared and elastic

---

## 20. Persistence

### 20.1 Persistence Strategy

| Data Type | Store | Durability | Recovery |
|---|---|---|---|
| Scheduling State | State Store (etcd/Consul) | Strong | Automatic reload on startup |
| Queue State | State Store + local snapshot | Weak (in-memory OK) | Rebuild from tasks |
| Audit Log | Database | Strong | Append-only |
| Metrics | Time-series DB | Configurable | Not recovered |
| Config | Config Server | Strong | Automatic reload |

### 20.2 State Snapshots

```
Snapshot Policy:
  - Full snapshot: every 60s
  - Incremental snapshot: every 10s
  - Stored: State Store + local disk
  - Recovery: Load from State Store
  - Max snapshot restore time: 5 seconds
```

---

## 21. Validation

### 21.1 Input Validation

```
ScheduleTaskRequest Validation:
  TaskId:
    - Must be valid UUID v4
    - Must be unique (no duplicate task IDs)
    - Max length: 64 characters
  Priority:
    - Must be one of: P0, P1, P2, P3, P4
    - Must match caller authorized priority range
  Deadline:
    - Must be in future (within 5s tolerance)
    - Must not exceed max deadline (30 days)
    - Required if EDF policy active
  Department:
    - Must match caller's department
    - Department must be active in system
  ResourceDemand:
    - CPU: 0.1 to 64 cores
    - Memory: 64MB to 256GB
    - Network: 0 to 10Gbps

Validation Responses:
  - 200: Valid request
  - 400: Invalid format or missing required field
  - 403: Insufficient permissions
  - 413: Request size exceeds limit (1MB max)
  - 429: Too many requests (rate limit exceeded)
  - 503: Scheduler not ready
```

### 21.2 Policy Validation

```
Before activation, each policy must pass validation:
  - Policy configuration complete and well-formed
  - All referenced nodes exist and are healthy
  - Queue definitions do not conflict
  - Weight sum equals 1.0 for weighted policies
  - Deadline parameters within acceptable ranges
  - Policy change will not cause scheduling instability
```

---

## 22. Retry Logic

### 22.1 Scheduling Retries

```
Retry Policy for Scheduling Attempts:
  - Max attempts: 3
  - Initial backoff: 100ms
  - Backoff multiplier: 2x
  - Max backoff: 1000ms
  - Jitter: +/- 50%

Retry Triggers:
  - Node not found / node offline
  - Cluster state stale (retry with fresh state)
  - Transient queue write error
  - Temporary resource scarcity

After Max Retries:
  - Task goes to DEFERRED queue with 30s delay
  - Notification sent to monitoring
  - Deferred tasks retried automatically
  - If deferred 3 times → moved to dead letter
```

### 22.2 Recovery Retries

```
Scheduler Recovery Retry:
  - On crash recovery: retry any in-flight tasks
  - Tasks in SCHEDULED but not yet RUNNING → re-queue
  - Tasks in RUNNING → verify with Execution Engine
  - Tasks in QUEUED → maintain position
```

---

## 23. Error Recovery

### 23.1 Error Classification

| Error Type | Severity | Recovery Strategy |
|---|---|---|
| Queue full | HIGH | Backpressure, then reject |
| Node unavailable | MEDIUM | Reschedule to another node |
| Policy engine error | CRITICAL | Fallback to FIFO, alert ops |
| State store unavailable | CRITICAL | Stop scheduling, retry |
| Task validation failure | LOW | Return error to caller |
| Gang formation timeout | MEDIUM | Partial gang dispatch |
| Priority inheritance cycle | CRITICAL | Break cycle, force resolution |

### 23.2 Recovery Procedures

```
State Store Failure:
  1. Detect: Read/write errors to state store
  2. Isolate: Stop new scheduling requests
  3. Drain: Complete in-flight tasks
  4. Retry: Attempt reconnection with backoff
  5. Fallback: Use local state only (degraded mode)
  6. Restore: When state store recovers, sync state
  7. Resume: Accept scheduling requests

Policy Engine Failure:
  1. Detect: Policy evaluation exception
  2. Fallback: Use FIFO policy (simplest, always available)
  3. Alert: Notify operations immediately
  4. Recover: Reload policy configuration
  5. Verify: Run policy test cases
  6. Resume: Switch back to configured policy

Node Overload:
  1. Detect: Node load exceeds threshold
  2. Exclude: Remove node from scheduling pool
  3. Reschedule: Move affected tasks to other nodes
  4. Monitor: Watch node load trend
  5. Reinclude: When node load drops, add back to pool
```

---

## 24. Security

### 24.1 Scheduling Security Model

- All scheduling requests must be authenticated
- Requests without valid credentials are REJECTED
- Each request carries identity context
- Scheduling decisions are tamper-evident (signed)
- Queue contents are encrypted at rest and in transit

### 24.2 Secure Scheduling

- **Tenant Isolation**: Departments cannot see or affect other departments' scheduling
- **Priority Isolation**: Lower priority tasks cannot preempt higher priority tasks
- **Resource Isolation**: Scheduling quotas enforced per tenant
- **Audit Trail**: All scheduling decisions recorded permanently

---

## 25. Authentication

### 25.1 Authentication Requirements

- All API calls must include valid bearer token
- Tokens validated against Authentication Service
- Token carries: identity, roles, department, permissions
- Token caching: 5 minute cache with revocation support
- Failed authentication: 401 with no additional information

---

## 26. Authorization

### 26.1 Authorization Policies

| Permission | Description | Required Role |
|---|---|---|
| schedule:submit | Submit tasks for scheduling | agent, user, service |
| schedule:cancel | Cancel scheduled tasks | agent, admin |
| schedule:priority | Set task priority | team-lead+ |
| schedule:policy | Change scheduling policy | sysadmin |
| schedule:rebalance | Trigger rebalance | sysadmin |
| schedule:admin | Admin operations | sysadmin |

---

## 27. Permissions

### 27.1 Permission Matrix

```
Section A: Core Permissions
  schedule.task.create     - Create scheduling entries
  schedule.task.read      - Read scheduling state
  schedule.task.update    - Update scheduling state
  schedule.task.delete    - Remove from scheduling

Section B: Admin Permissions
  schedule.policy.read    - Read policy configuration
  schedule.policy.write   - Modify policy configuration
  schedule.config.read   - Read scheduler configuration
  schedule.config.write  - Modify scheduler configuration

Section C: Monitoring Permissions
  schedule.metrics.read   - Read scheduling metrics
  schedule.audit.read     - Read scheduling audit log
```

---

## 28. Monitoring

### 28.1 Monitoring Dashboard

```
Scheduler Dashboard:
  Live panel:
    - Current scheduling rate (tasks/sec)
    - Queue depth per priority level
    - Active tasks across cluster
    - Node utilization heatmap
    - Scheduling latency (p50, p95, p99, p99.9)
    - Deadline miss count (last hour)

  Alerting:
    - Queue depth > 80%: warning
    - Queue depth > 95%: critical
    - Scheduling latency > 100ms p95: warning
    - Scheduling latency > 500ms p95: critical
    - Deadline miss rate > 1%: critical
    - Any P0 task wait time > 1s: critical
    - Node excluded > 10% nodes: warning
```

---

## 29. Logging

### 29.1 Log Categories

| Category | Level | Detail |
|---|---|---|
| schedule.decision | INFO | Every scheduling decision |
| schedule.queue | DEBUG | Queue operations |
| schedule.policy | INFO | Policy engine execution |
| schedule.preempt | WARN | Preemption events |
| schedule.error | ERROR | Scheduling errors |
| schedule.security | WARN | Authorization failures |
| schedule.audit | INFO | Audit-relevant events |
| schedule.state | DEBUG | State transitions |

### 29.2 Log format

```
[2026-07-10T14:30:00.123Z] [INFO] [schedule.decision] taskId=uuid-1234
  status=SCHEDULED node=node-42 policy=priority priority=P1
  waitTimeMs=43 estimatedStartTime=... estimatedCompletionTime=...
  callerDept=engineering
```

---

## 30. Metrics

| Metric | Type | Tags | Description |
|---|---|---|---|
| scheduler.requests.total | Counter | policy, status | Total scheduling requests |
| scheduler.schedule.duration_ms | Histogram | policy | Scheduling decision time |
| scheduler.queue.depth | Gauge | queue_type, priority | Current queue depth |
| scheduler.queue.wait_ms | Histogram | queue_type, priority | Time tasks wait in queue |
| scheduler.tasks.scheduled | Counter | node, priority | Scheduled tasks per node |
| scheduler.tasks.completed | Counter | node, priority | Completed tasks per node |
| scheduler.tasks.failed | Counter | node, priority | Failed tasks per node |
| scheduler.tasks.preempted | Counter | reason | Preempted task count |
| scheduler.priority.inherited | Counter | from, to | Inheritance events |
| scheduler.gang.formed | Counter | status | Gang formation events |
| scheduler.nodes.active | Gauge | zone | Active scheduling nodes |
| scheduler.nodes.excluded | Gauge | zone | Excluded nodes |
| scheduler.deadline.miss | Counter | priority | Deadline miss events |
| scheduler.backpressure.events | Counter | trigger | Backpressure activations |
| scheduler.policies.active | Gauge | policy | Active policy count |
| scheduler.errors | Counter | type, severity | Scheduler errors |

---

## 31. Tracing

### 31.1 Trace Spans

| Span Name | Start Event | End Event | Parent |
|---|---|---|---|
| schedule-task | Request received | Response sent | HTTP/gRPC span |
| policy-evaluate | Policy eval start | Policy decision | schedule-task |
| queue-enqueue | Enqueue start | Enqueue complete | schedule-task |
| queue-dequeue | Dequeue start | Dequeue complete | schedule-task |
| node-select | Node selection start | Node decision | schedule-task |
| dispatch-task | Dispatch start | Dispatch ack | schedule-task |
| preempt-task | Preemption triggered | Suspension done | schedule-task |
| inherit-priority | Inheritance start | Inheritance end | schedule-task |

### 31.2 Trace Context Propagation

- Trace ID passed through: API request → gRPC metadata → Event headers
- All external calls include trace context
- All event emissions include trace context
- Cross-component traces correlated via shared trace ID

---

## 32. Scalability

### 32.1 Horizontal Scaling

- Scheduler instances: 3-7 replicas for HA
- Leader election via State Store (etcd lease)
- Active scheduler: Leader handles all scheduling
- Standby schedulers: Hot standby, ready to take over
- State replication: State store provides consistency

### 32.2 Vertical Scaling

```
Scaling Limits:
  - Maximum tasks per scheduler instance: 2,000,000
  - Maximum nodes managed: 100,000
  - Maximum departments: 10,000
  - Maximum throughput: 50,000 tasks/second (single instance)
  - Maximum throughput: 200,000 tasks/second (with sharding)

Sharding Strategy:
  - Shard by Department ID: each shard handles subset
  - Shard by Node Group: each scheduler manages node group
  - Shard by Priority Level: separate scheduler per priority
  - State store handles cross-shard coordination
```

### 32.3 Throughput Optimization

```
Batch Processing:
  - Scheduling requests batched for processing
  - Maximum batch size: 100
  - Batch timeout: 10ms
  - Policy evaluation: per batch, not per task

Pipeline:
  - Request validation pipeline: parallel
  - Queue operations: batched writes
  - Node selection: parallel scoring
  - Decision recording: async write behind

Rate Limiting:
  - Per caller: 1000 requests/second
  - Burst: 2000 requests for up to 5 seconds
  - Backpressure at 1500 requests/second sustained
```

---

## 33. Performance

### 33.1 Performance Targets

| Metric | Target | Degraded | Unacceptable |
|---|---|---|---|
| Scheduling decision latency (p50) | < 5ms | < 15ms | > 50ms |
| Scheduling decision latency (p99) | < 20ms | < 60ms | > 200ms |
| Queue enqueue latency (p99) | < 2ms | < 5ms | > 20ms |
| Node scoring latency (p99) | < 10ms | < 30ms | > 100ms |
| Throughput (single instance) | 10K req/s | 5K req/s | < 5K req/s |
| State recovery time | < 5s | < 15s | > 30s |
| Queue depth check | < 1ms | < 3ms | > 10ms |

### 33.2 Performance Considerations

- All hot paths are lock-free where possible
- Queue operations use lock-free CAS semantics
- Node scoring uses immutable snapshots
- Policy evaluation uses cached pre-computed data
- Event publication is asynchronous
- Metrics collection does not block decision path

---

## 34. Optimization

### 34.1 Scheduling Optimization

```
Optimization Techniques:
  1. Lookaside Scheduling: Pre-compute schedule for common patterns
  2. Speculative Scheduling: Schedule on multiple candidate nodes
  3. Co-Scheduling: Bundle compatible tasks for batch processing
  4. Deferred Correction: Allow suboptimal decision now, correct later
  5. Weighted Scoring: Multi-factor scoring with tunable weights

Optimization Parameters:
  - ScheduleQualityWeight: 0.7
  - DecisionSpeedWeight: 0.2
  - ResourceUtilizationWeight: 0.1
```

### 34.2 Memory Optimization

```
Memory Management:
  - Object pooling for TaskRecord objects
  - Flyweight pattern for repeated metadata
  - Lazy evaluation of node scores
  - Compressed queue state in main memory
  - Frequent GC with generational collection
  - Off-heap storage for large queues (> 100K)
```

---

## 35. Testing Strategy

### 35.1 Unit Testing

```
Test Coverage Targets:
  - Policy engine: 100% branch coverage
  - Queue operations: 100% branch coverage
  - Node scoring: 100% branch coverage
  - Priority management: 95% branch coverage
  - Affinity resolution: 90% branch coverage

Test Categories:
  1. Policy selection tests
  2. FIFO ordering tests
  3. Priority ordering tests
  4. Round-robin fairness tests
  5. Weighted fair allocation tests
  6. EDF deadline ordering tests
  7. Priority inheritance tests
  8. Inversion detection tests
  9. Aging mechanism tests
  10. Gang formation tests
  11. Load scoring tests
  12. Affinity scoring tests
```

### 35.2 Integration Testing

```
Integration Test Scenarios:
  1. Schedule → Execute → Complete cycle
  2. Schedule with priority inheritance flow
  3. Gang formation → dispatch → completion
  4. Preemption → suspend → resume cycle
  5. Deadline miss → escalation
  6. Backpressure → load shedding
  7. Multi-department isolation
  8. Cross-scheduler failover
```

### 35.3 Chaos Testing

```
Chaos Scenarios:
  1. Random node failures during scheduling
  2. State store outages
  3. Network partitions
  4. Extreme load (10x normal)
  5. Priority flooding (all P0)
  6. Starvation attacks
  7. Gang formation deadlocks
  8. Simultaneous policy changes
```

---

## 36. Risk Analysis

### 36.1 Identified Risks

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Priority inversion causes deadlock | Low | Critical | Inheritance protocol, timeout |
| Gang scheduling deadlock | Low | High | Formation timeout, partial dispatch |
| Scheduling policy misconfiguration | Medium | High | Validation, automatic rollback |
| State store partition | Medium | Critical | Leader election, hot standby |
| Starvation of low-priority tasks | Medium | Medium | Aging mechanism, starvation alerts |
| Deadline misses for critical tasks | Low | Critical | Preemption, escalation |
| Queue overflow due to load spike | Medium | Medium | Backpressure, load shedding |

### 36.2 Risk Mitigation

```
Priority Inversion Prevention:
  - Mandatory priority inheritance for all shared resources
  - Inheritance timeout with automatic break
  - Inversion detection and alerting

Starvation Prevention:
  - Mandatory aging for all queues
  - Guaranteed minimum service per department
  - Starvation detection: P4 wait time > 300s

Gang Formation Deadlocks:
  - Formation timeout: 30s max wait
  - Partial dispatch with notification
  - Deadlock detection cycle check
```

---

## 37. Failure Scenarios

### 37.1 Failure Mode Analysis

```
Scenario A: State Store Unavailable
  Symptom: All scheduling operations fail with connection error
  Impact: No scheduling possible, cluster idle
  Detection: Health check failure, error rate spike
  Recovery: Fallback to local state, degraded mode
  
Scenario B: Policy Engine Crash
  Symptom: Scheduling decisions fail or return defaults
  Impact: Suboptimal scheduling, policy not enforced
  Detection: Policy eval timeout, default policy activation
  Recovery: Policy cache fallback, live reload
  
Scenario C: Priority Inversion Cycle
  Symptom: High-priority tasks blocked, throughput drops
  Impact: Critical tasks waiting indefinitely
  Detection: P0 wait time > 5s, inheritance depth exceeded
  Recovery: Force break at max depth (3), escalate to admin

Scenario D: Queue Overflow
  Symptom: New tasks rejected with "queue full"
  Impact: Tasks not accepted, backlog grows
  Detection: Queue depth > 95% threshold
  Recovery: Backpressure → load shedding → admin intervention

Scenario E: Scheduler Instance Failure
  Symptom: Scheduling requests timeout
  Impact: No scheduling for ~5s during leader election
  Detection: Leader lease expires
  Recovery: New leader elected, state restored from store
```

---

## 38. Recovery Procedures

### 38.1 Startup Recovery

```
Scheduler Startup:
  1. Load configuration from Config Server
  2. Connect to State Store
  3. Restore scheduling state from latest snapshot
  4. Wait for all queues to be reconstructed
  5. Connect to Event Bus and subscribe to events
  6. Verify state consistency (periodic checks)
  7. Register as available scheduler
  8. Enter leader election
  9. If elected leader: activate scheduling
  10. Begin accepting scheduling requests
```

### 38.2 Full State Recovery

```
Recovery from Total State Loss:
  1. Start with empty state
  2. Rebuild active tasks from Execution Engine
  3. Rebuild queue state from Queue System
  4. Reconstruct priority levels from agent metadata
  5. Verify all pending workflows
  6. Consistency check: no duplicates, no orphaned tasks
  7. Enter degraded mode: reduced throughput
  8. Monitor: verify scheduling quality metrics met
  9. Exit degraded mode
```

---

## 39. Operational Limits

### 39.1 Hard Limits

| Parameter | Limit | Behavior at Limit |
|---|---|---|
| Max queue depth | 100,000 per queue | Reject with queue-full error |
| Max active tasks | 500,000 cluster-wide | Slow scheduling until tasks complete |
| Max nodes managed | 100,000 | Increase sampling interval |
| Max scheduling rate | 100,000 req/s | Reject, return backpressure |
| Max task wait time | 1 hour | Age to high priority automatically |
| Max gangs in formation | 10,000 | Reject new gang requests |
| Max gangs per workflow | 500 | Break into sub-workflows |
| Max priority levels | 5 (P0-P4) | Defined by spec, not configurable |
| Max inheritance depth | 3 | Force break, alert admin |
| Max inheritance duration | 30s | Force break, revert priority |

### 39.2 Soft Limits (Warning Thresholds)

| Parameter | Warning | Elevation | Critical |
|---|---|---|---|
| Queue depth | > 80% capacity | > 90% capacity | > 95% capacity |
| Scheduling latency | > 10ms p95 | > 25ms p95 | > 50ms p95 |
| Deadline miss rate | > 0.1% | > 0.5% | > 1% |
| Node exclusion | > 5% | > 10% | > 20% |
| Priority inheritance rate | > 100/s | > 500/s | > 1000/s |

---

## 40. Maintenance

### 40.1 Routine Maintenance

| Operation | Frequency | Impact | Duration |
|---|---|---|---|
| Queue depth cleanup | Daily | None | < 1s |
| Stale task removal | Hourly | None | < 1s |
| Node state sync | Every 5s | None | Continuous |
| Policy cache refresh | Every 5 min | None | < 100ms |
| State snapshot | Every 60s | Minor | < 500ms |
| Metrics aggregation | Every 10s | None | Continuous |
| Config reload check | Every 30s | None | < 10ms |

### 40.2 Preventive Maintenance

```
Weekly:
  - Review scheduling metrics trends
  - Analyze deadline miss patterns
  - Check for priority imbalance
  - Review aging statistics

Monthly:
  - Performance benchmark execution
  - Capacity planning review
  - Policy effectiveness review
  - Queue depth trend analysis

Quarterly:
  - Full scheduler stress test
  - Recovery procedure drill
  - Security audit review
  - Dependency version upgrade check
```

---

## 41. Future Improvements

### 41.1 Roadmap

```
Phase 1: Current implementation
  - Priority-based scheduling with 5 levels
  - FIFO fallback for non-prioritized
  - Per-department queue isolation
  - Basic node load scoring
  - Priority inheritance

Phase 2 (Next):
  - EDF deadline-aware scheduling
  - Gang scheduling support
  - Aging mechanism for starvation prevention
  - Affinity scheduling with data locality

Phase 3 (Future):
  - ML-based scheduling optimization
  - Predictive scheduling (pre-schedule predicted tasks)
  - Multi-cluster scheduling
  - Energy-aware scheduling
  - Cost-aware scheduling

Phase 4 (Advanced):
  - Supercomputing scheduling (MPI-aware)
  - Quantum-inspired scheduling algorithms
  - Self-tuning scheduler
  - Federated scheduling across organizational boundaries
```

### 41.2 Known Limitations

- No real-time I/O scheduling (disk, network)
- No GPU topology-aware scheduling
- No energy consumption scheduling
- No inter-cluster scheduling (single cluster)
- No cross-region scheduling
- No multi-cloud scheduling

---

## 42. Examples

### 42.1 Basic Scheduling Example

```
Request:
  TaskId: "prod-payment-workflow-001-step-3"
  AgentType: "payment-processor"
  Priority: P1
  Department: "payments"
  Deadline: 1728100000000
  InputDurationMs: 2000

Processing:
  1. Validate: All fields valid, authorized
  2. Policy: Priority (default for P1 tasks)
  3. Queue: enqueued to priority.p1 at position 42
  4. Wait time: ~350ms
  5. Node selection: Scoring 3 candidate nodes
     - node-03: CPU=45%, Mem=60%, affinity=0.8 → score=0.71
     - node-07: CPU=30%, Mem=40%, affinity=0.3 → score=0.49
     - node-12: CPU=70%, Mem=80%, affinity=0.9 → score=0.56
     → Selected: node-03 (highest score)
  6. Decision: SCHEDULED on node-03
  7. Dispatch: Sent to Execution Engine

Result:
  Status: SCHEDULED
  Node: node-03
  WaitTimeMs: 350
  EstimatedCompilation: 1728099650000
```

### 42.2 Gang Scheduling Example

```
Workflow: "data-pipeline-42" has 3 parallel agents:
  - agent "data-fetcher" (A1)
  - agent "data-validator" (A2)
  - agent "data-enricher" (A3)
  All must run simultaneously on same node.

Process:
  1. A1 arrives → GangGroup created: {gang-42}
  2. A2 arrives → GangGroup: {gang-42, members: [A1, A2]}
  3. A3 arrives → GangGroup: {gang-42, members: [A1, A2, A3], status: READY}
  4. Cluster capacity check: node-07 has capacity for 3 agents
  5. Gang dispatched as group to node-07
  6. All 3 agents start simultaneously
  7. All complete → gang COMPLETED
```

### 42.3 Priority Inversion Prevention Example

```
Scenario:
  - Agent T1 (P0, safety-critical) needs lock L1
  - Agent T2 (P4, background) holds lock L1
  - Agent T3 (P2) currently running on same node

Detection:
  - T1 blocked on L1 → T2 holds L1
  - Priority inversion: P0 blocked on P4
  - Inheritance activated

Action:
  - T2 priority temporarily elevated to P1
  - T2 preempts T3 (P2 → held while T2 runs)
  - T2 releases lock L1
  - T2 priority returns to P4
  - T3 resumes
  - T1 acquires lock L1 and continues
```

---

## 43. Acceptance Criteria

| # | Criteria | Verification Method |
|---|---|---|
| AC1 | Scheduler consistently makes scheduling decisions within 20ms p99 | Automated performance test |
| AC2 | Priority ordering: P0 always scheduled before P1, etc. | Integration test |
| AC3 | FIFO ordering maintained within same priority level | Unit test |
| AC4 | Priority inheritance activates within 10ms of inversion detection | Integration test |
| AC5 | Aging promotes P4 tasks after 300s wait time | Integration test |
| AC6 | Gang scheduling dispatches all members simultaneously | Integration test |
| AC7 | EDF scheduling schedules earlier deadlines first | Unit test |
| AC8 | Load-aware scheduling selects least-loaded nodes | Integration test |
| AC9 | Affinity scheduling correctly schedules near data | Integration test |
| AC10 | Queue backpressure activates at 80% depth | Unit test |
| AC11 | State recovery completes within 5 seconds | Chaos test |
| AC12 | Scheduler handles 10,000 tasks/second sustained throughput | Performance test |
| AC13 | No task experiences starvation beyond 1 hour | Integration test |
| AC14 | Deadline miss rate < 0.1% for non-overloaded clusters | Performance test |

---

## 44. Definition of Done

| # | Criteria | Owner |
|---|---|---|
| DoD-1 | All acceptance criteria met | QA |
| DoD-2 | 95% unit test branch coverage | Engineering |
| DoD-3 | Integration tests pass for all scheduling flows | QA |
| DoD-4 | Performance targets met under 10K tasks/second | Performance |
| DoD-5 | Chaos tests pass: node failures, state store outages | QA |
| DoD-6 | Security audit: auth, authz, audit trail verified | Security |
| DoD-7 | Monitoring dashboards and alerts configured | Ops |
| DoD-8 | Recovery procedures documented and tested | Ops |
| DoD-9 | Operational limits documented | Engineering |
| DoD-10 | API documentation complete and published | Engineering |
| DoD-11 | Migration plan from existing scheduler documented | Engineering |
| DoD-12 | Runbook created for all known failure scenarios | Ops |

---

## End of Document 31: AIOS Scheduler