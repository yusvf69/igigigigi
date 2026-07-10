# AIOS Engineering Specification
## Document 32: AIOS Execution Engine

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

The AIOS Execution Engine is the core runtime component responsible for actually running agent tasks within secure, isolated environments. It receives scheduled tasks from the AIOS Scheduler, creates appropriate execution contexts, manages the lifecycle from launch to completion, and enforces all execution policies including isolation, resource limits, timeout controls, and security constraints. The Execution Engine is the final authority on what runs where, when, and under what conditions.

---

## 2. Mission

To provide a safe, scalable, and deterministic runtime environment where every AIOS agent task executes in full isolation, within guaranteed resource boundaries, with complete observability, and with provable execution trace for audit and debugging purposes.

---

## 3. Responsibilities

### 3.1 Task Execution Management
- Accept scheduled tasks from the Scheduler
- Create execution contexts for each task
- Initialize runtime environments (containers, cgroups, namespaces)
- Launch agent processes with correct configuration and input
- Monitor execution lifetime from start to completion
- Handle execution results (success, failure, timeout, preemption)
- Report completion or failure back to the Scheduler

### 3.2 Execution Mode Support
- Execute tasks sequentially within a single thread
- Execute tasks in parallel using fork-join patterns
- Support conditional execution (if-then-else branches)
- Support iterative execution (loops with bounded iterations)
- Implement fan-out/fan-in for parallel task distribution
- Implement map-reduce for distributed data processing

### 3.3 Resource Management
- Enforce CPU limits using cgroups cpu quota
- Enforce memory limits using cgroups memory subsystem
- Enforce IOPS limits using cgroups blkio
- Enforce network bandwidth limits using tc/rate limiting
- Monitor and report actual resource consumption
- Provide resource accounting per task completion

### 3.4 Execution Safety
- Implement filesystem isolation (chroot, overlayfs, bind mounts)
- Implement network isolation (isolated network namespace)
- Drop unnecessary capabilities (principle of least privilege)
- Apply seccomp filters for system call restriction
- Enforce execution timeout with SIGKILL on expiry
- Implement preemption with state capture and resume

### 3.5 Execution Hooks
- Execute pre-execution hooks before task starts
- Execute post-execution hooks after task completes
- Execute on-interrupt hooks when task is interrupted
- Execute on-timeout hooks when task exceeds timeout
- Support hook chaining and composition

### 3.6 Trace and Replay
- Record full execution trace for every task
- Capture stdout/stderr with ring buffer
- Record execution lifecycle events with timestamps
- Support execution replay for debugging
- Support execution session recording for audit

### 3.7 Lifecycle Management
- Manage task state transitions (PENDING through COMPLETED/FAILED)
- Handle graceful shutdown of tasks
- Support execution suspension and resumption
- Support preemptive termination
- Handle dependent task cascading failures

---

## 4. Non-Responsibilities

- The Execution Engine does NOT schedule tasks; scheduling is handled by the AIOS Scheduler
- The Execution Engine does NOT determine task priority; priority is set by the Scheduler
- The Execution Engine does NOT manage workflow orchestration; workflows are managed by the Workflow Engine
- The Execution Engine does NOT provide data storage or persistence for outputs
- The Execution Engine does NOT perform authentication or identity management
- The Execution Engine does NOT implement business logic for agents
- The Execution Engine does NOT manage task queuing or retry scheduling
- The Execution Engine does NOT provide agent code or agent models
- The Execution Engine does NOT manage network routing or DNS resolution

---

## 5. Architecture

### 5.1 High-Level Architecture

```
                        Execution Engine API Layer
    +----------------+  +----------------+  +-----------------+
    |  Execute API   |  |  Lifecycle API  |  |  Debug/Replay   |
    +--------+-------+  +-------+--------+  +-------+---------+
             |                   |                   |
             v                   v                   v
                     Execution Engine Core Layer
    +------------------------------------------------------+
    |                 Execution Dispatcher                  |
    |   (Accepts tasks, coordinates pipeline, returns)     |
    +------+----------------+------------------+-----------+
           |                |                  |
    +------v------+  +------v------+  +-------v----------+
    | Context Mgr  |  | Mode Router |  | Result Handler   |
    | - Input      |  | - Seq      |  | - Success        |
    | - Env        |  | - Fork-Join|  | - Error          |
    | - Perms      |  | - MapReduce|  | - Partial        |
    | - Identity   |  | - Iterative|  | - Timeout        |
    | - Trace Init |  | - Cond     |  | - Trace Finalize |
    +--------------+  +-------------+  +------------------+
           |                |                  |
    +------v----------------v------------------v-----------+
    |                 Isolation Manager                    |
    | +----------+ +---------+ +-------+ +-----------+    |
    | | Container | |Namespce | |cgroup | | seccomp  |    |
    | | Manager  | | Manager | |Manager| | Manager  |    |
    | +----------+ +---------+ +-------+ +-----------+    |
    | +----------+ +---------+                            |
    | | FS Isole | | Net Isol|                            |
    | +----------+ +---------+                            |
    +------------------------------------------------------+
    |          |                |              |
    +---v------+----+  +------v------+  +----v-----------+
    | Resource Mon  |  | Timeout Mgr |  | Preemption Mgr |
    | - CPU track   |  | - Task TO   |  | - State Capt   |
    | - Mem track   |  | - Wf TO     |  | - Suspend      |
    | - IOPS track  |  | - TO hook   |  | - Resume       |
    | - Enforce     |  | - KILL      |  | - Restore      |
    +---------------+  +-------------+  +----------------+
    |          |                |              |
    +---v------+----+  +------v------+  +----v-----------+
    | Hook Manager   |  | Trace Replay |  | Audit Recorder|
    | - Pre-exec     |  | - Capturing  |  | - Events      |
    | - Post-exec    |  | - Replaying  |  | - Results     |
    | - On-interrupt |  | - Querying   |  | - Security    |
    | - On-timeout   |  | - Exporting  |  |                |
    +----------------+  +-------------+  +----------------+
                    Execution Engine Infrastructure
    +------------------+  +----------------+  +-------------+
    | Container/Virt   |  | Process Mgr   |  | OS Kernel    |
    | (Docker/VM)      |  | (fork/exec)   |  | (cgroups/ns) |
    +------------------+  +----------------+  +-------------+
```

### 5.2 Architecture Principles

1. Defense in Depth: Multiple isolation layers protect the host system
2. Deterministic: Same inputs produce same outputs every time
3. Resource Guarantees: Tasks get exactly their allocated share
4. Complete Observability: Every execution decision is recorded
5. Graceful Degradation: Failures are contained, not cascaded
6. Principle of Least Privilege: Tasks get minimum required permissions
7. Fail Closed: On error, task is terminated, not left running

---

## 6. Internal Components

### 6.1 Execution Dispatcher

The central coordinator for all execution requests:

```
Component: ExecutionDispatcher
Responsibilities:
  - Receive execution requests from the Execute API
  - Validate request completeness and permissions
  - Determine execution mode based on request
  - Coordinate the execution pipeline
  - Manage task lifecycle from PENDING to completion
  - Dispatch results back to caller

Execution Pipeline:
  Step 1: Validate request (format, permissions, resources)
  Step 2: Determine execution mode (sequential, parallel, etc.)
  Step 3: Allocate resources (CPU, memory, IOPS)
  Step 4: Create isolation environment (container/namespace/cgroups)
  Step 5: Set up execution context (input, env, identity, trace)
  Step 6: Attach lifecycle hooks (pre, post, on-interrupt, on-timeout)
  Step 7: Execute task (launch process, monitor lifecycle)
  Step 8: Capture results (output, resource usage, errors)
  Step 9: Execute post-execution hooks
  Step 10: Cleanup resources (destroy isolation, release limits)
  Step 11: Return result to caller

Configuration:
  MaxConcurrentExecutions: 500
  ExecutionRequestTimeoutMs: 10000
  GracefulShutdownMs: 30000
  DefaultIsolationLevel: 2
```

### 6.2 Context Manager

Creates and manages the execution context for each task:

```
Component: ContextManager
Responsibilities:
  - Build execution context from execution request
  - Set up environment variables from task metadata
  - Inject input data into execution environment
  - Configure permissions and identity context
  - Initialize trace context for distributed tracing
  - Create working directory for the task

Context Structure:
  {
    TaskId: string (UUID v4, required)
    WorkflowId: string (optional, for workflow tasks)
    InputData: bytes (serialized input for agent)
    Environment: map<string,string> (environment variables)
    Permissions: PermissionSet (allowed capabilities/resources)
    Identity: AgentIdentity (who/what is executing)
    Trace: TraceContext (traceId, spanId, parentSpanId)
    ExecutionMode: Mode (sequential, parallel, etc.)
    ResourceLimits: ResourceSpec (CPU, memory, IOPS)
    TimeoutMs: int (maximum execution duration)
    Hooks: HookConfig[] (lifecycle hooks to attach)
    Metadata: map<string,string> (additional context)
  }
```

### 6.3 Mode Manager

Implements all supported execution modes:

```
Component: ModeManager
Responsibilities:
  - Determine execution mode from request
  - Implement SEQUENTIAL execution
  - Implement FORK_JOIN parallel execution
  - Implement CONDITIONAL execution
  - Implement ITERATIVE execution (bounded loops)
  - Implement MAP_REDUCE distributed processing
  - Implement FAN_OUT_FAN_IN pattern

Mode Definitions:

SEQUENTIAL:
  - Single agent, single execution
  - Blocking: caller waits for completion
  - Lifecycle: start -> run -> complete
  - Simple deterministic execution

FORK_JOIN:
  - Task creates N parallel subtasks
  - Each subtask executes in independent context
  - Fork: launch all N subtasks concurrently
  - Join: wait for all subtasks to complete
  - Fail-fast: any subtask failure cancels all remaining
  - Fail-tolerant: continue remaining even if some fail (configurable)
  - Results: merged from all subtasks

CONDITIONAL:
  - Precondition expression evaluated before execution
  - If true: execute primary task
  - If false: execute alternative or skip
  - Condition must be side-effect free
  - Condition evaluated in isolated context

ITERATIVE:
  - Agent executes repeatedly in a loop
  - Bounded: max 1000 iterations (hard limit)
  - Each iteration independently traceable
  - Can be parallelized internally (if loop body is stateless)
  - Early exit on accumulator condition

MAP_REDUCE:
  - Map: data split into chunks, each processed in parallel
  - Shuffle: intermediate results re-partitioned
  - Reduce: map results combined into single output
  - Fault tolerant: failed maps retried independently
  - Configurable: map parallelism, reduce factor

FAN_OUT_FAN_IN:
  - Fan-out: single agent spawns N independent subtasks
  - Fan-in: N results aggregated into single output
  - Subtasks are independent (no shared state)
  - Partial failure: configurable tolerance
  - Unlike fork-join: subtasks are independent agents, not subtasks

Configuration:
  MaxParallelism: 100 (max forked subtasks)
  MaxIterations: 1000
  MaxFanOut: 500
  DefaultMode: sequential
```

### 6.4 Isolation Manager

Provides multi-layered execution isolation:

```
Component: IsolationManager
Responsibilities:
  - Create container environment for each task
  - Configure network namespace
  - Configure filesystem namespace (overlayfs, bind mounts)
  - Configure PID namespace (isolated process tree)
  - Configure IPC namespace (isolated communication)
  - Apply cgroups controls for resource limits
  - Apply seccomp filters for system call restriction
  - Drop non-essential Linux capabilities
  - Clean up isolation resources on completion

Isolation Levels:

Level 0 (NONE):
  - No isolation
  - Run as host process in host namespaces
  - Only for trusted, admin-level agents
  - Requires explicit override

Level 1 (LIGHT):
  - Namespace isolation only (PID, Network)
  - Shared filesystem (read-only view)
  - No capability dropping
  - For: internal system agents

Level 2 (STANDARD):
  - Full namespace isolation (PID, NET, IPC, UTS, Mount)
  - cgroups resource control
  - Capability dropping (remove setuid, dac_override, etc.)
  - Read-only root filesystem with tmpfs for /tmp
  - For: standard agent execution

Level 3 (STRICT):
  - Level 2 plus seccomp filtering
  - Loopback-only network (no external access)
  - Dropped all capabilities
  - For: untrusted or third-party agents

Level 4 (MAXIMUM):
  - Full container isolation (separate container)
  - seccomp with minimal allowlist
  - No network access (loopback only)
  - Read-only root with no writable mounts
  - Capability set: empty
  - For: high-security, high-sensitivity agents

Container Configuration:
  BaseImage: minimal-rootfs (scratch + agent runtime)
  RootFS: read-only (overlayfs)
  TempWritableDirs: /tmp (tmpfs, max 10% of memory limit)
  Network: loopback only (level 3+), NAT outbound (level 2)
  Capabilities: none (level 3+), CAP_NET_BIND_SERVICE (level 2)
  Seccomp: default deny with allowlist

Configuration:
  DefaultIsolationLevel: 2
  MaxIsolationLevel: 4
  IsolationOverrideRequired: true (sysadmin only for level 0)
```

### 6.5 Resource Monitor

Monitors and enforces task resource consumption:

```
Component: ResourceManager
Responsibilities:
  - Allocate CPU quota per task (cgroups cpu.max)
  - Set memory limits with OOM protection (cgroups memory.max)
  - Configure IOPS limits (cgroups io.max)
  - Configure network bandwidth (tc qdisc)
  - Monitor runtime resource consumption (poll every 100ms)
  - Enforce resource limits at cgroup level
  - Detect resource limit violations
  - Report resource accounting at task completion

Resource Limits:
  CPU:
    - CPUQuota: int (microseconds per period, default 100000)
    - CPUs: float (core count, default 1.0)
    - CPUShares: int (relative weight, default 1024)
    - CPUAffinity: string (optional CPU pinning)

  Memory:
    - MemoryLimitMB: int (hard limit, default 512)
    - MemorySwapMB: int (swap limit, default 256)
    - MemoryReservationMB: int (guaranteed minimum)
    - OOMControl: bool (default true, kill on OOM)

  IOPS:
    - ReadIOPS: int (default 1000)
    - WriteIOPS: int (default 500)
    - ReadBPS: int (bytes/sec, default 100MB)
    - WriteBPS: int (bytes/sec, default 50MB)

  Network:
    - BandwidthInKbps: int (default 100000)
    - BandwidthOutKbps: int (default 50000)
    - BurstMultiplier: float (default 2.0)
```

### 6.6 Hook Manager

Manages execution lifecycle hooks:

```
Component: HookManager
Responsibilities:
  - Maintain hook registry for the execution lifecycle
  - Register hooks from system, department, and agent
  - Execute pre-execution hooks (blocking, before task starts)
  - Execute post-execution hooks (non-blocking, after task completes)
  - Execute on-interrupt hooks (blocking, when interrupted)
  - Execute on-timeout hooks (blocking, when timeout)
  - Handle hook failures (fail execution for system hooks, warn for others)

Hook Types:
  PRE_EXECUTION:
    - Purpose: Validate environment, inject secrets, check preconditions
    - Execution: blocking, task waits for hook completion
    - Failure policy: fail execution if system hook, warn if agent hook
    - Order: system -> department -> agent

  POST_EXECUTION:
    - Purpose: Clean up resources, collect metrics, trigger downstream
    - Execution: non-blocking for agent hooks
    - Execution: blocking for system hooks (ensure cleanup)
    - Failure policy: log warning, continue

  ON_INTERRUPT:
    - Purpose: Save intermediate state, clean up locked resources
    - Execution: blocking, max 5 seconds
    - Failure policy: log error, continue (best effort)
    - Available: system only

  ON_TIMEOUT:
    - Purpose: Log timeout details, capture partial output
    - Execution: blocking, max 3 seconds
    - Failure policy: log error, continue
    - Available: system only

Configuration:
  MaxHooksPerLifecycleEvent: 10
  PreHookTimeoutMs: 5000
  PostHookTimeoutMs: 10000
  InterruptHookTimeoutMs: 5000
  TimeoutHookTimeoutMs: 3000
```

### 6.7 Timeout Manager

Enforces execution timeouts:

```
Component: TimeoutManager
Responsibilities:
  - Track task execution duration via monotonic clock
  - Enforce per-task timeout limits
  - Enforce per-workflow timeout limits
  - Trigger on-timeout hooks when timeout exceeded
  - Send SIGTERM for graceful shutdown (5 second grace)
  - Send SIGKILL if graceful shutdown fails
  - Record timeout details in execution trace

Timeout Configuration:
  DefaultTaskTimeoutMs: 300000 (5 minutes)
  MinTaskTimeoutMs: 1000
  MaxTaskTimeoutMs: 86400000 (24 hours)
  GracefulShutdownMs: 5000
  WorkflowTimeoutMs: configurable per workflow

Timeout Hierarchy:
  - Workflow-level timeout checked at workflow boundary
  - Task-level timeout enforced per individual task
  - Sub-task timeout inherits from parent unless explicitly set
  - Hard deadline: task MUST complete before deadline
  - Soft deadline: task SHOULD complete (warning at 80%)
```

### 6.8 Preemption Manager

Handles task preemption for priority scheduling:

```
Component: PreemptionManager
Responsibilities:
  - Handle preemption requests from the Scheduler
  - Capture task execution state for later resumption
  - Suspend task execution gracefully
  - Notify task of incoming preemption (SIGTSTP)
  - Persist suspension state
  - Handle resume requests from the Scheduler
  - Restore task execution state from persistence

Preemption Protocol:
  Step 1: Scheduler sends preemption request
  Step 2: Execution Engine signals task with SIGTSTP
  Step 3: Task captures state (cooperative preemption)
  Step 4: If no state capture in < 2s -> forced suspension
  Step 5: State serialized and stored in State Store
  Step 6: Resources released for preemptor task
  Step 7: When resumption requested:
    a. State deserialized from State Store
    b. Resources reallocated
    c. Task restarted from checkpoint
    d. SIGCONT sent to continue execution

Configuration:
  CooperativePreemptionEnabled: true
  ForcedPreemptionTimeoutMs: 5000
  StateCaptureEnabled: true
  MaxPreemptionsPerTask: 3
  StateStoreTimeoutMs: 2000
```

### 6.9 Result Handler

Collects and formats execution results:

```
Component: ResultHandler
Responsibilities:
  - Collect execution output from completed tasks
  - Classify results: success, partial, error, timeout
  - Format/compress result data
  - Send result to caller via API callback
  - Record result in execution trace
  - Trigger post-execution hooks
  - Handle result routing for workflows

Result Types:
  SUCCESS:
    - Task completed with exit code 0
    - Output data available in result
    - Resource usage recorded
    - Full trace available

  PARTIAL_SUCCESS:
    - Operation partially completed
    - Some subtasks failed or were cancelled
    - Partial output available
    - Warning logged

  ERROR:
    - Task failed with application error
    - Error details captured (type, message, stack trace)
    - No valid output
    - Error code for classification

  TIMEOUT:
    - Task exceeded timeout limit
    - Partial output captured if available
    - Timeout details recorded
    - Execution terminated by engine

  PREEMPTED:
    - Task was preempted (suspended)
    - State saved for resume
    - Not a final state (can be resumed)

Configuration:
  MaxResultSizeBytes: 10485760 (10 MB)
  ResultTruncationEnabled: true
  ResultCompressionEnabled: true
  ResultCompressionAlgorithm: snappy
```

### 6.10 Trace and Replay Recorder

Records execution traces for debugging and replay:

```
Component: TraceRecorder
Responsibilities:
  - Record full execution trace for every task
  - Capture all lifecycle events with timestamps
  - Record input/output hashes for integrity
  - Capture stdout/stderr with ring buffer
  - Record resource usage over time
  - Store trace data for replay and audit
  - Provide trace query and replay interfaces

Trace Record Schema:
  {
    traceId: uuid,
    taskId: uuid,
    workflowId: uuid (optional),
    events: [
      { timestamp: ISO8601, eventType: string, payload: object }
    ],
    inputHash: sha256,
    outputHash: sha256 (null if failed),
    outputTruncated: boolean,
    resourceSnapshots: [
      { timestamp: ISO8601, cpuPercent: float, memoryMB: int, iops: int }
    ],
    errors: [
      { timestamp: ISO8601, errorType: string, message: string }
    ],
    identity: { agent: string, department: string },
    durationMs: int,
    exitCode: int,
    stdoutTail: string (last 64KB),
    stderrTail: string (last 64KB)
  }

Configuration:
  TraceEnabled: true
  MaxTraceEvents: 10000
  MaxStdoutBytes: 65536
  MaxStderrBytes: 65536
  TraceCompressionEnabled: true
  TraceStorageDurationDays: 30
  ReplayDataRetentionDays: 7
  ResourceSnapshotIntervalMs: 1000
```

---

## 7. External Components

### 7.1 AIOS Scheduler
- Dispatches tasks for execution
- Receives execution completion status
- Sends preemption requests
- Receives preemption acknowledgment

### 7.2 Agent Registry
- Provides agent metadata (binary path, version, capabilities)
- Verifies agent integrity (SHA256 checksum)
- Reports agent deprecation/update

### 7.3 Container Runtime
- Creates and manages execution containers
- Provides container lifecycle
- Handles container networking

### 7.4 Linux Kernel
- Provides cgroups for resource control
- Provides namespaces for isolation
- Provides seccomp for system call filtering
- Provides capabilities for privilege control

### 7.5 State Store
- Stores preemption state for later resumption
- Stores active task assignments

### 7.6 Configuration System
- Provides execution engine configuration
- Provides security policies
- Provides isolation level defaults

### 7.7 Monitoring Service
- Collects execution metrics
- Generates execution alerts
- Visualizes execution dashboards

---

## 8. Dependencies

| Dependency | Type | Criticality | Description |
|---|---|---|---|
| AIOS Scheduler | Internal | CRITICAL | Task dispatch, preemption |
| Agent Registry | Internal | CRITICAL | Agent binary retrieval |
| Container Runtime | Infra | CRITICAL | Execution isolation |
| Linux Kernel (cgroups) | OS | CRITICAL | Resource control |
| Linux Kernel (namespaces) | OS | CRITICAL | Process isolation |
| seccomp | OS | HIGH | Syscall filtering |
| Linux Capabilities | OS | HIGH | Privilege control |
| Network Control (tc) | OS | HIGH | Bandwidth management |
| Filesystem (overlayfs) | OS | HIGH | Filesystem isolation |
| State Store (etcd/Consul) | Internal | HIGH | Preemption state |
| Trace System | Internal | HIGH | Execution tracing |
| Config Server | Internal | MEDIUM | Engine configuration |
| Secret Store | External | MEDIUM | Secret injection |

---

## 9. Inputs

### 9.1 ExecuteRequest
```
Schema: ExecuteRequest
Source: AIOS Scheduler (via gRPC)
Fields:
  TaskId: string (UUID v4, required)
  WorkflowId: string (optional)
  AgentType: string (required, registered in Agent Registry)
  AgentVersion: string (optional, default: latest)
  InputData: bytes (serialized input for the agent)
  Environment: map<string, string> (optional env vars)
  Mode: string [SEQUENTIAL, PARALLEL, CONDITIONAL, ITERATIVE, MAP_REDUCE, FAN_OUT_IN]
  ResourceLimits: ResourceSpec (optional, uses defaults if not provided)
  TimeoutMs: int (optional, default from configuration)
  IsolationLevel: int [0-4] (optional, default from configuration)
  Permissions: string[] (permission set for this execution)
  Priority: enum [P0..P4] (from scheduler, for preemption decision)
  CallerIdentity: string (JWT or service account identity)
  DepartmentId: string (department scope)
  Hooks: HookConfig[] (optional lifecycle hooks)
  TraceId: string (optional, for distributed tracing)
  Metadata: map[string, string] (optional additional data)
```

### 9.2 Configuration Input

```
Schema: ExecutionEngineConfig
Source: Configuration System
Fields:
  DefaultIsolationLevel: int (0-4)
  DefaultTimeoutMs: int
  DefaultResourceLimits: ResourceSpec
  MaxConcurrentExecutions: int
  TraceEnabled: boolean
  PreemptionEnabled: boolean
  AvailableModes: string[]
  MaxResultSizeBytes: int
  MaxStdoutCaptureBytes: int
  HookTimeoutMs: map[string, int]
  AllowedIsolationOverrides: string[]
```

---

## 10. Outputs

### 10.1 ExecuteResponse

```
Schema: ExecuteResponse
Fields:
  TaskId: string
  Status: enum [SUCCESS, PARTIAL_SUCCESS, ERROR, TIMEOUT, PREEMPTED, CANCELLED]
  OutputData: bytes (optional, truncated at MaxResultSizeBytes)
  OutputLocation: string (optional, if output stored externally)
  Error: {
    Type: string (error classification),
    Message: string (human-readable error message),
    StackTrace: string (optional, for debugging),
    ErrorCode: int (machine-readable error code)
  }
  ResourceUsage: {
    CPUPercentUsed: float (average CPU utilization),
    PeakMemoryMB: int (peak memory),
    PeakIOPS: int (peak IOPS),
    DurationMs: int (wall clock execution time),
    TotalCPUTimeMs: int (total CPU time consumed),
    TotalBytesRead: int,
    TotalBytesWritten: int,
    TotalNetworkInBytes: int,
    TotalNetworkOutBytes: int
  }
  StartTime: timestamp (ISO8601)
  EndTime: timestamp (ISO8601)
  TraceId: string
  Preemptions: int (number of times preempted)
  IsolationLevel: int (actual isolation level used)
```

### 10.2 Stream Events (for streaming results)

```
Event: TaskStarted
  Payload: { taskId, startTime, nodeId, mode }

Event: TaskProgress
  Payload: { taskId, progressPercent, substatus, timestamp }

Event: TaskCheckpoint
  Payload: { taskId, checkpointId, progressPercent, timestamp }

Event: TaskCompleted
  Payload: { taskId, status, outputHash, durationMs, usage }

Event: TaskFailed
  Payload: { taskId, errorType, errorMessage, exitCode, durationMs }

Event: TaskTimedOut
  Payload: { taskId, timeoutMs, durationMs, partialOutputAvailable }

Event: TaskPreempted
  Payload: { taskId, reason, suspendedBy, stateHash }

Event: TaskResumed
  Payload: { taskId, checkpointId, resumptionTime }
```

---

## 11. Data Structures

### 11.1 TaskExecutionRecord

```
interface TaskExecutionRecord {
  taskId: string
  workflowId?: string
  agentType: string
  agentVersion: string
  status: TaskStatus
  mode: ExecutionMode
  isolationLevel: number
  context: ExecutionContext
  resourceAllocation: ResourceAllocation
  startTime?: number
  endTime?: number
  durationMs?: number
  exitCode?: number
  preemptions: number
  suspensionCount: number
  resumeCount: number
  hooks: HookExecutionRecord[]
  events: TaskEvent[]
  result?: ExecutionResult
}

enum TaskStatus {
  PENDING = 'PENDING',
  INITIALIZING = 'INITIALIZING',
  ISOLATING = 'ISOLATING',
  RUNNING = 'RUNNING',
  SUSPENDED = 'SUSPENDED',
  RESUMING = 'RESUMING',
  COMPLETED = 'COMPLETED',
  FAILED = 'FAILED',
  TIMEOUT = 'TIMEOUT',
  CANCELLED = 'CANCELLED',
  PREEMPTED = 'PREEMPTED'
}
```

### 11.2 ExecutionContext

```
interface ExecutionContext {
  taskId: string
  workflowId?: string
  traceId: string
  spanId: string
  parentSpanId?: string
  agentType: string
  agentVersion: string
  input: Buffer
  environment: Record<string, string>
  permissions: Set<string>
  identities: AgentIdentity
  resourceLimits: ResourceSpec
  timeoutMs: number
  mode: ExecutionMode
  hooks: HookConfig[]
  metadata: Record<string, string>
  workingDirectory: string
  containerId?: string
  pid?: number
}
```

### 11.3 ResourceSpec and ResourceAllocation

```
interface ResourceSpec {
  cpuCores: number
  memoryMB: number
  maxMemoryMB: number
  iops: IOPSLimit
  network: NetworkLimit
  diskMB: number
  ephemeralStorageMB: number
}

interface ResourceAllocation {
  cgroupPath: string
  cpuQuota: number
  cpuPeriod: number
  memoryLimit: number
  memorySwapLimit: number
  networkClassId: number
  allocatedMemoryMB: number
  allocatedCPU: number
}

interface IOPSLimit {
  readIOPS: number
  writeIOPS: number
  readBPS: number
  writeBPS: number
}

interface NetworkLimit {
  bandwidthInKbps: number
  bandwidthOutKbps: number
  connectionsPerSecond: number
  burstMultiplier: number
}
```

### 11.4 Hook Configuration

```
interface HookConfig {
  name: string
  type: HookType [SYSTEM, DEPARTMENT, AGENT]
  phase: HookPhase [PRE_EXECUTION, POST_EXECUTION, ON_INTERRUPT, ON_TIMEOUT]
  script: string (hook script path or inline)
  timeoutMs: number
  blocking: boolean
  retry: RetryConfig
  metadata: Record<string, string>
}
```

### 11.5 Preemption State

```
interface PreemptionState {
  taskId: string
  nodeId: string
  originalPriority: string
  preemptedByTaskId: string
  preemptedByPriority: string
  suspensionTime: number
  stateStoragePath: string
  stateChecksum: string
  resumeAttempt: number
  resumeDeadline: number
}
```

### 11.6 ForkJoinSubtasks

```
interface ForkJoinTask {
  parentTaskId: string
  subtaskId: string
  subtaskIndex: number
  totalSubtasks: number
  input: Buffer
  mode: ExecutionMode
  parallelism: number
  failFast: boolean
  status: SubTaskStatus
  result?: ExecutionResult
}
```

---

## 12. Execution Flow

### 12.1 Primary Sequential Execution Flow

```
Step 1: RECEIVE EXECUTION REQUEST
  - ExecuteRequest received from Scheduler via Execute API
  - Validate request format, permissions, resource availability
  - Create TaskExecutionRecord with PENDING status
  - Generate trace span for this execution

Step 2: INITIALIZE EXECUTION CONTEXT
  - Call ContextManager.create()
  - Set input data into context
  - Set environment variables from request + defaults
  - Set permissions and identity
  - Initialize trace context (traceId, spanId)
  - Set status to INITIALIZING

Step 3: SET UP ISOLATION ENVIRONMENT
  - Call IsolationManager.setup()
  - Create cgroup for this task (cpu, memory, io, net)
  - Create namespace bundle (PID, NET, IPC, UTS, Mount)
  - Apply seccomp filter (if isolation >= 3)
  - Drop unnecessary capabilities
  - Mount read-only root filesystem via overlayfs
  - Mount tmpfs for /tmp
  - Create working directory
  - Set status to ISOLATING

Step 4: EXECUTE PRE-EXECUTION HOOKS
  - Call HookManager.executePreHooks()
  - Execute system pre-hooks first
  - Execute department pre-hooks second
  - Execute agent pre-hooks last
  - Each hook is blocking (task waits)
  - Hook failure policy: system hook failure aborts execution
  - Agent hook failure logs warning, continues
  - Record hook execution results

Step 5: LAUNCH PROCESS
  - Fork or create container for the agent
  - Apply cgroup assignments to the process
  - Set process in the correct namespaces
  - Pipe stdin from input data (or mount input file)
  - Pipe stdout and stderr to trace capture
  - Start timeout timer (TimeoutManager.start())
  - Set status to RUNNING
  - Emit TaskStarted event

Step 6: MONITOR EXECUTION
  - Resource Monitor polls every 100ms:
    - CPU used (cgroups cpuacct)
    - Memory used (cgroups memory.current)
    - IOPS (cgroups io.stat)
  - Trace Recorder captures:
    - stdout/stderr ring buffer (last 64KB each)
    - Resource snapshots every 1s
    - Significant lifecycle events
  - Check preemption signal (non-blocking)
  - Check timeout (TimeManager.isExpired())

Step 7: HANDLE COMPLETION
  Process exits normally (exit code 0):
    - Collect full output from stdout
    - Collect full stderr for logging
    - ResourceManager.finalize() - collect final usage
    - TraceRecorder.finalize() - finalize trace record
    - Set status to COMPLETED
    - Emit TaskCompleted event

  Process exits with error (non-zero exit code):
    - Capture stderr as error message
    - Capture exit code
    - ResourceManager.finalize()
    - Set status to FAILED
    - Emit TaskFailed event

  Timeout fires:
    - TimeoutManager.trigger()
    - Execute on-timeout hooks (capture partial output)
    - Send SIGTERM (5 second grace period)
    - If process still alive: send SIGKILL
    - Set status to TIMEOUT
    - Emit TaskTimedOut event

Step 8: EXECUTE POST-EXECUTION HOOKS
  - Call HookManager.executePostHooks()
  - System hooks for cleanup
  - Capture post-hook results (for audit)
  - Post-hook failure: log warning only

Step 9: FORMAT AND RETURN RESULT
  - Build ExecuteResponse from execution data
  - Include status, output (or error), resource usage
  - Send to caller via gRPC response
  - Record result in audit log

Step 10: CLEANUP RESOURCES
  - Destroy isolation environment
  - Clean up cgroups
  - Clean up namespaces
  - Remove temporary files
  - Close file descriptors
  - Release resource allocations
  - Log task completion summary
```

### 12.2 Fork-Join Parallel Execution Flow

```
Step 1: RECEIVE FORK_JOIN REQUEST
  - Mode = FORK_JOIN
  - Input contains list of subtask data chunks
  - Determine parallelism: min(requested, maxParallelism, available resources)

Step 2: CREATE SUBTASKS
  - For each data chunk: create SubTask record
  - Each subtask assigned independent context
  - Each subtask gets independent isolation environment
  - Set subtask total = N, subtask index = 0..N-1

Step 3: FORK SUBTASKS
  - Launch all N subtasks concurrently (limited by parallelism)
  - Each subtask runs like sequential execution (steps 4-10)
  - Subtasks communicate progress to parent

Step 4: MONITOR ALL SUBTASKS
  - Track each subtask's lifecycle
  - Handle partial failures:
    - Fail-fast: on any failure, send cancel to all remaining subtasks
    - Fail-tolerant: continue remaining subtasks, track failures
  - Collect results as subtasks complete

Step 5: JOIN RESULTS
  - Wait for all subtasks to complete (or fail)
  - Aggregate all subtask results into combined output
  - Merge resource usage (total CPU, peak memory)
  - Determine final status:
    - All succeeded: SUCCESS
    - Some failed (fail-safe configured): PARTIAL_SUCCESS
    - Some failed (fail-fast): FAILED
    - Any timeout: TIMEOUT

Step 6: RETURN COMBINED RESULT
  - Return combined result with per-subtask breakdown
  - Include subtask status in result metadata
  - Proceed to post-execution hooks and cleanup
```

### 12.3 Preemption Flow (Suspend and Resume)

```
PREEMPTION (SUSPEND):

Step 1: RECEIVE PREEMPT REQUEST
  - Scheduler sends PreemptRequest(taskId)
  - Task must be in RUNNING state
  - Verify task is preemptible (not in critical section)

Step 2: INITIATE SUSPENSION
  - Set status: RUNNING -> SUSPENDING
  - Send SIGTSTP to task process
  - Start cooperative suspension timeout timer (2 seconds)
  - Execute on-interrupt hooks (save intermediate state)

Step 3: CAPTURE STATE
  - If task is cooperative (checkpoint received):
    - Save checkpoint data
    - Capture current memory state
    - Save to State Store
  - If task is non-cooperative (timeout):
    - Capture last-known state (from trace)
    - Mark as forced preemption
    - State may be incomplete

Step 4: SUSPEND TASK
  - Suspend all task threads
  - Release CPU allocation (memory may be retained)
  - Close network connections
  - Set status: SUSPENDING -> SUSPENDED
  - Emit TaskPreempted event
  - Notify Scheduler: preemption complete

RESUMPTION:

Step 5: RECEIVE RESUME REQUEST
  - Scheduler sends ResumeRequest(taskId)
  - Task must be in SUSPENDED state

Step 6: RESTORE STATE
  - Load state from State Store
  - Verify state integrity (checksum)
  - Reallocate resources (CPU, memory)

Step 7: RESUME EXECUTION
  - Restore memory and process state
  - Restore network connections
  - Send SIGCONT to resumed process
  - Set status: SUSPENDED -> RESUMING -> RUNNING
  - Emit TaskResumed event

Step 8: CONTINUE MONITORING
  - Task continues from where it was suspended
  - Normal monitoring resumes
```

### 12.4 Execution Replay Flow

```
Step 1: RECEIVE REPLAY REQUEST
  - ReplayRequest(traceId) or ReplayRequest(taskId)
  - Load trace data from TraceRecorder storage
  - Verify trace integrity (hash checks)

Step 2: REPLAY TIMELINE
  - Replay events in chronological order
  - For each event:
    - Show timestamp
    - Show event type
    - Show event payload (truncated if large)
  - Show resource usage graph (CPU, memory over time)
  - Show stdout/stderr output aligned to timeline

Step 3: ANALYZE REPLAY
  - Compare original execution to replayed timeline
  - Highlight differences (if trace is corrupted or modified)
  - Annotate key decision points
  - Show errors with stack traces at relevant timeline points

Step 4: GENERATE REPLAY REPORT
  - Summary of execution
  - Timeline with all events
  - Resource usage graphs
  - Error annotations
  - Output at each stage
```

---

## 13. State Management

### 13.1 Task State Machine

```
                +-----------+
                |  PENDING  |
                +-----+-----+
                      |
                      v
                +-------------+
                | INITIALIZING |
                +------+------+
                       |
                       v
                +-------------+
                |  ISOLATING  |
                +------+------+
                       |
                       v
           +-----------+-----------+
           |                       |
     +-----v------+         +------v------+
     |   RUNNING   |         |  SUSPENDED  |
     +-----+------+         +------+------+
           |                       |
           |                       v
           |                +------v------+
           |                |  RESUMING   |
           |                +------+------+
           |                       |
           +----+--------+---------+
                |        |
        +-------v--+  +--v--------+
        | COMPLETED |  |  FAILED   |
        +-----------+  +-----------+

 Alternative paths:
   RUNNING -> TIMEOUT
   RUNNING -> PREEMPTED (terminal for this execution)
   RUNNING -> CANCELLED
   PENDING -> CANCELLED
```

### 13.2 State Persistence

```
State is persisted at different levels:

  Active Tasks (in-memory):
    - TaskExecutionRecord for currently running tasks
    - Rebuilt on engine restart via reconciliation

  Completed/Error Tasks (Result Store):
    - Task results and error details
    - Resource usage data
    - Trace data

  Preemption State (State Store):
    - PreemptionState for suspended tasks
    - Checkpoint snapshots for resumed tasks
    - Survives engine restart
    - Automatically cleaned when task completes or resume fails

  Trace Records (Trace Store):
    - Full execution traces
    - Stdout/stderr ring buffer captures
    - Resource usage time series
    - Retention: 30 days default
```

---

## 14. Communication Protocols

### 14.1 Internal Protocols

| Protocol | Transport | Format | Use |
|---|---|---|---|
| Execute API | gRPC | Protobuf | Task execution RPC |
| Lifecycle API | gRPC | Protobuf | Preempt, resume, cancel |
| Debug API | gRPC stream | Protobuf | Traces and replay |
| IPC | Unix Domain Socket | Protobuf | Agent-to-Engine communication |
| Events | Event Bus (pub/sub) | JSON | Execution lifecycle events |
| Metrics | StatsD/UDP | String format | Performance metrics |

### 14.2 External Protocols

| Protocol | Transport | Format | Use |
|---|---|---|---|
| Container Mgmt | Container Runtime API | gRPC | Create/manage containers |
| Resource Control | cgroups fs | File read/write | CPU, memory, IOPS limits |
| Network Control | netlink | Netlink messages | Network namespace setup |
| Filesystem | mount syscall | Syscall | Overlayfs/chroot setup |
| Process Control | clone/fork/execve | Syscall | Process lifecycle |

---

## 15. APIs

### 15.1 Execution API (gRPC)

```
service ExecutionService {
  rpc ExecuteTask(ExecuteRequest) returns (ExecuteResponse)
  rpc ExecuteTaskStream(stream ExecuteRequest) returns (stream ExecuteResponse)
  rpc PreemptTask(PreemptRequest) returns (PreemptResponse)
  rpc ResumeTask(ResumeRequest) returns (ResumeResponse)
  rpc CancelTask(CancelRequest) returns (CancelResponse)
  rpc GetTaskStatus(GetStatusRequest) returns (TaskStatusResponse)
  rpc GetTaskResult(GetResultRequest) returns (ExecuteResponse)
  rpc GetTaskTrace(TraceRequest) returns (TraceResponse)
}

message ExecuteRequest {
  string task_id = 1
  string workflow_id = 2
  string agent_type = 3
  bytes input = 4
  map<string,string> environment = 5
  ExecutionMode mode = 6
  ResourceSpec resources = 7
  int32 timeout_ms = 8
  int32 isolation_level = 9
  repeated HookConfig hooks = 10
  string caller_identity = 11
  string department_id = 12
  string trace_id = 13
}
```

### 15.2 Admin API (REST)

```
GET  /api/v1/engine/status              - Engine health and capacity
GET  /api/v1/engine/tasks               - Running tasks list
GET  /api/v1/engine/tasks/:taskId       - Task details
POST /api/v1/engine/tasks/:taskId/cancel   - Cancel a task
POST /api/v1/engine/tasks/:taskId/preempt  - Preempt a task
POST /api/v1/engine/tasks/:taskId/resume   - Resume a task
GET  /api/v1/engine/metrics             - Engine performance metrics
GET  /api/v1/engine/config              - Current configuration
PUT  /api/v1/engine/config              - Update configuration
GET  /api/v1/engine/capacity            - Available capacity
```

### 15.3 Debug API (gRPC Stream)

```
service ExecutionDebugService {
  rpc StreamExecutionLog(LogFilter) returns (stream LogEntry)
  rpc GetFullTrace(TraceRequest) returns (TraceResponse)
  rpc ReplayExecution(ReplayRequest) returns (stream ReplayEvent)
  rpc AttachToTask(AttachRequest) returns (stream TaskOutput)
}
```

---

## 16. Events

### 16.1 Published Events (from Execution Engine)

| Event Name | Payload | Consumers |
|---|---|---|
| TaskStarted | taskId, timestamp, nodeId, mode, isolationLevel | Scheduler, Monitoring, Audit |
| TaskProgress | taskId, progressPercent, substatus | Workflow Engine, Monitoring |
| TaskCheckpoint | taskId, checkpointId, progress | Trace System, Workflow |
| TaskCompleted | taskId, status, outputHash, durationMs, resourceUsage | Scheduler, Workflow, Audit |
| TaskFailed | taskId, errorType, errorMessage, exitCode, durationMs | Scheduler, Retry, Audit |
| TaskTimedOut | taskId, timeoutMs, durationMs, partialOutput | Scheduler, Monitoring |
| TaskPreempted | taskId, reason, suspendedBy, stateHash | Scheduler, Audit |
| TaskResumed | taskId, checkpointId, resumptionTime | Scheduler |
| TaskCancelled | taskId, reason, cancelledBy | Scheduler, Workflow |
| ResourceExceeded | taskId, resourceType, current, limit | Monitoring, Ops |

### 16.2 Subscribed Events (consumed by Execution Engine)

| Event Name | Source | Handler Action |
|---|---|---|
| TaskScheduled | Scheduler | Prepare execution context |
| PreemptRequest | Scheduler | Initiate preemption flow |
| ResumeRequest | Scheduler | Initiate resume flow |
| CancelRequest | Operator/Workflow | Initiate cancellation flow |
| ConfigChanged | Configuration System | Reload execution config |
| NodeShutdown | Cluster Manager | Drain and complete tasks |
| AgentUpdated | Agent Registry | Invalidate agent binary cache |

---

## 17. Queues

The Execution Engine does not maintain persistent task queues. Tasks are received directly from the Scheduler and executed immediately, subject to concurrency limits. When the concurrency limit is reached, tasks are rejected with capacity exceeded, and the Scheduler handles retry/requeue logic.

The Execution Engine maintains an internal, in-memory active task queue only:

```
Internal Active Tasks (in-memory):
  - ActiveTaskQueue: ordered by task start time
  - Purpose: iteration for resource monitoring
  - Not persisted, rebuilt on startup
  
  - Preemption Priority Queue (in-memory):
  - Ordered by priority (P0 highest)
  - Used for preemption victim selection
```

---

## 18. Caching

### 18.1 Cache Strategy

| Cache | Scope | TTL | Size Limit | Eviction Policy |
|---|---|---|---|---|
| Agent Binary Cache | Node | 1 hour | 1 GB | LRU |
| Container Image Cache | Node | 24 hours | 10 GB | LRU |
| Environment Config Cache | Per-engine | 5 minutes | 10 MB | TTL-based |
| Seccomp Filter Cache | Global | 1 hour | 10 MB | LRU |
| Hook Script Cache | Global | 1 hour | 10 MB | LRU |
| Resource Config Cache | Per-engine | 30 seconds | 1 MB | TTL-based |

### 18.2 Image Cache Management

```
Agent Image Caching:
  - On first execution of agent type: pull image, cache locally
  - SHA256 verification before cache insertion
  - Pre-pull for high-frequency agent types
  - Cache miss: pull from registry (timeout: 5 min)
  - Cache eviction: oldest unused when exceeding size limit
  - Stale image cleanup: every 6 hours

Agent Binary Caching:
  - Version-keyed: agent-type + version as cache key
  - Integrity verification (SHA256 hash)
  - Pre-load for high-frequency agent types
  - Fallback to agent registry on cache miss
  - Clear on agent version update
```

---

## 19. Memory

### 19.1 Memory Allocation

| Component | Memory | Notes |
|---|---|---|
| Execution Contexts | 2 MB per active task | Context struct + input buffer |
| Task State Records | 512 bytes per task | Metadata only |
| Output Buffers | 10 MB per task max | Truncated at limit |
| Container Overhead | 50 MB per task | Container runtime + namespaces |
| Trace Buffers | 128 KB per task | Ring buffers for std{out,err} |
| Caches | 2 GB total | Agent images, binaries |
| Monitoring Data | 500 MB total | Resource snapshots, aggregations |
| Reserved Pool | 500 MB | Pre-allocated for new tasks |

Per-task (typical isolated task): approx 62 MB
Per-node limit (500 concurrent tasks): 31 GB (approximate)

### 19.2 Memory Management Policies

- Per-task memory limit enforced via cgroups memory subsystem
- OOM killer disabled for agent processes; engine-level OOM handling
- Output buffer truncated at MaxResultSizeBytes
- Ring buffer for stdout: tail-only (loses beginning on overflow)
- Ring buffer for stderr: tail-only (captures last errors)
- Off-heap memory used for large input data
- G1-style GC for managed memory regions
- Memory pressure signals trigger cache eviction

---

## 20. Persistence

### 20.1 Required Persistence

| Data Type | Storage Backend | Retention | Recovery Strategy |
|---|---|---|---|
| Execution Traces | Filesystem + Trace DB | 30 days | Load on replay request |
| Preemption State | State Store (etcd/Consul) | Until task completes | Restore on resume |
| Execution Config | Config Server | Permanent | Auto-reload on restart |
| Resource Usage Data | Time-series DB | 90 days | For capacity planning |
| Audit Log | Append-only Log | 7 years | Permanent record |
| Container Images | Container Registry | Permanent | Pull on execution |

### 20.2 Output Persistence

- Task outputs stored in Result Store
- Default retention: 7 days (non-critical)
- Extended retention: 30 days (audited/regulated)
- Large outputs (>10MB): stored in object store (S3-compatible)
- Outputs may be compressed (snappy) for storage efficiency

---

## 21. Validation

### 21.1 Input Validation

```
ExecuteRequest Validation Rules:

TaskId:
  - Must be valid UUID v4
  - Must be unique (no duplicates currently executing)
  - Maximum length: 64 characters

AgentType:
  - Must be registered in Agent Registry
  - Must be active (not deprecated)
  - Must have deployable binary/image
  - Agent must be available (no maintenance hold)

ResourceLimits (if specified):
  - CPU: 0.1 to MaxCPUCores (default: 64)
  - Memory: 64 to MaxMemoryMB (default: 65536)
  - Timeout: MinTimeoutMs (1000) to MaxTimeoutMs (86400000)
  - IOPS: 0 to MaxIOPS (100000)
  - Disk: 0 to MaxDiskMB (102400)

IsolationLevel:
  - Must be 0-4
  - Level 0 requires admin override
  - Must not exceed department's maximum allowed

ExecutionMode:
  - Must be one of supported modes
  - Must be compatible with agent capabilities

Permissions:
  - Must be subset of caller's permissions
  - Scoped explicitly (no wildcards)
  - Validated against allowed permissions

Validation Status Codes:
  200: Valid, proceeding to execution
  400: VALIDATION_ERROR (invalid format)
  403: PERMISSION_DENIED (insufficient permissions)
  429: RATE_LIMIT_EXCEEDED (too many requests)
  503: RESOURCE_UNAVAILABLE (capacity exhausted)
```

### 21.2 Runtime Validation

```
Resource Limit Validation:
  - Before execution: check available capacity
  - During execution: enforce limits at cgroup level
  - Threshold warnings: at 80% of limit -> log warning
  - Limit exceeded: at 100% -> apply enforcement

Isolation Integrity Validation:
  - Verify seccomp filter loaded correctly
  - Verify cgroup settings applied
  - Verify namespace membership (PID in correct PID ns)
  - Verify read-only mount is actually read-only

Output Validation:
  - Output size check against MaxResultSizeBytes
  - Output format validation (if MIME type specified)
  - Output content safety scan (if configured)
```

---

## 22. Retry Logic

The Execution Engine does not implement retry logic directly. Retries are handled by the AIOS Retry System. The engine's role is to accurately report failure status so the Retry System can make appropriate retry decisions.

### 22.1 Reporting Errors for Retry

```
When reporting errors, the engine includes:
  - Error type (transient, persistent, critical)
  - IsRetryable boolean (engine's best guess)
  - Suggested action for retry (increase resources, different node)

Retry-Determining Fields in Error Output:
  - transient: true if infrastructure error (container fail, OOM)
  - resourceExceeded: true if task exceeded limits
  - applicationError: true if agent returned error (not retryable)
  - systemFault: true if engine caused error (retryable)
```

---

## 23. Error Recovery

### 23.1 Error Classification

| Error | Category | Severity | Recovery Action |
|---|---|---|---|
| Container runtime error | Infrastructure | HIGH | Retry on new container |
| cgroup creation failure | Infrastructure | HIGH | Fallback to process-only mode |
| Namespace creation failure | Infrastructure | HIGH | Fallback to level 0 isolation |
| Filesystem mount failure | Infrastructure | MEDIUM | Use in-memory temp storage |
| Agent binary not found | Configuration | HIGH | Fail with BINARY_NOT_FOUND |
| Agent execution error (runtime) | Transient | MEDIUM | Capture error, report to Scheduler |
| Resource limit violation | Persistent | MEDIUM | Report violation, terminate |
| Timeout enforcement | Persistent | MEDIUM | Capture partial, terminate |
| Hook script failure | Configuration | LOW | Skip hook, log warning (agent) |
| Trace recording failure | Non-critical | LOW | Continue without trace |

### 23.2 Graceful Degradation

```
Isolation Failure:
  Step 1: Attempt level 2 isolation (standard container)
  Step 2: If fails: attempt level 1 isolation (namespaces only)
  Step 3: If still fails: attempt level 0 (process only)
  Step 4: Log each degradation with warning
  Step 5: If level 0 also fails: return ISOLATION_FAILED error

Resource Limit Violation:
  Step 1: Detect resource approaching limit (at 80%)
  Step 2: Apply pressure (throttle IO, reduce CPU quota)
  Step 3: At 100% for 5+ seconds: send SIGXFSZ or XCPU
  Step 4: At 100% for 10s: terminate with TIMEOUT
  Step 5: Report which resource was violated

Container Runtime Down:
  Step 1: Fallback to direct cgroups + namespace management
  Step 2: Run agent as isolated process (without container runtime)
  Step 3: Isolation level automatically downgraded
  Step 4: Notify operations of runtime failure
```

### 23.3 Orphaned Task Recovery

```
On Engine Crash/Restart:
  Step 1: On start, scan for surviving processes
  Step 2: Query cgroups for existing tasks
  Step 3: Check PID files / state files on disk
  Step 4: For each surviving task:
    a. Query agent state (is it running?)
    b. If running: reattach monitoring, reclassify as RUNNING
    c. If crashed: capture exit code, report status
    d. If unknown: send SIGTERM, capture what we can
  Step 5: Reconcile with Scheduler:
    a. Report all running tasks
    b. Report all found terminations
    c. Accept updated state from Scheduler
  Step 6: Resume accepting new execution requests

Task Not Found Locally But Known to Scheduler:
  - Treat as orphaned task (state lost)
  - Attempt to find any surviving process
  - If none: report as failed, Scheduler requeues
```

---

## 24. Security

### 24.1 Security Architecture

```
Defense in Depth - Isolation Stack:
  Layer 1: Host Operating System (kernel-hardened)
  Layer 2: Container Runtime (containerd/Docker)
  Layer 3: cgroups (CPU, memory, IOPS limits)
  Layer 4: Namespaces (PID, NET, IPC, UTS, Mount, User)
  Layer 5: seccomp (system call allowlist)
  Layer 6: Capability Dropping (remove privileged capabilities)
  Layer 7: Read-Only Root Filesystem
  Layer 8: AppArmor/SELinux (if available on host)
  Layer 9: Network Policies (network isolation, proxy, allowlist)
```

### 24.2 Seccomp Filtering

```
Default Deny Policy:
  - Block ALL system calls by default
  - Allowlist only necessary syscalls:
    read, write, close, mmap, munmap, mprotect, brk, sbrk,
    fstat, stat, lstat, newfstatat, openat, readlink, pread64,
    pwrite64, clock_gettime, getrandom, exit_group, exit,
    sched_yield, nanosleep, time_gettime64, futex, clone,
    set_robust_list, set_tid_address, getpid, getppid, gettid,
    tkill, tgkill, rt_sigaction, rt_sigprocmask, rt_sigreturn,
    sigaltstack, ioctl, poll, ppoll, epoll_create, epoll_ctl,
    epoll_wait, eventfd2, pipe2, dup, dup2, dup3, fcntl,
    getdents64, readv, writev, preadv, pwritev,
    connect (to allowlisted destinations only)

  All others: blocked with SIGSYS
```

### 24.3 Capability Dropping

```
Default (Level 2+): Drop all capabilities:
  CAP_SETUID, CAP_SETGID, CAP_NET_ADMIN, CAP_SYS_ADMIN,
  CAP_DAC_OVERRIDE, CAP_DAC_READ_SEARCH, CAP_SYS_PTRACE,
  CAP_SYS_BOOT, CAP_SYS_MODULE, CAP_KILL (if not needed),
  CAP_NET_RAW, CAP_NET_BROADCAST, CAP_IPC_LOCK,
  CAP_LINUX_IMMUTABLE, CAP_MKNOD, CAP_LEASE, CAP_SETFCAP,
  CAP_SYS_CHROOT, CAP_SYS_NICE, CAP_SYS_RESOURCE, CAP_SYS_TIME,
  CAP_WAKE_ALARM, CAP_BLOCK_SUSPEND, CAP_AUDIT_WRITE,
  CAP_AUDIT_CONTROL, CAP_SYSLOG, CAP_SYS_TTY_CONFIG

  Allowed (Level 2 only): CAP_NET_BIND_SERVICE (if agent needs network)
```

### 24.4 Filesystem Security

```
Read-Only Root (Level 2+):
  - Root filesystem: read-only (overlayfs with read-only lower)
  - /tmp: tmpfs, writable, size-limited (10% of memory limit)
  - /var/tmp: tmpfs, writable, size-limited
  - No writable bind mounts to host paths
  - No /proc or /sys write access
  - No /dev access except /dev/null, /dev/zero, /dev/random
```

---

## 25. Authentication

### 25.1 Execution Authentication

- All execution requests must include a valid identity token (JWT)
- Token validated by Authentication Service before execution forwarding
- Token contains: Agent ID, Department, Roles, Permissions
- Token scope limits what the execution can access
- Token lifetime: max 1 hour (tasks must complete within token TTL)
- If token expires during execution: task is terminated
- Token must be presented at each API call (not cached for auth)

---

## 26. Authorization

### 26.1 Execution Authorization

| Permission | Description | Required For |
|---|---|---|
| execute:task | Execute any agent task | All callers |
| execute:preempt | Preempt running tasks | Scheduler only |
| execute:resume | Resume suspended tasks | Scheduler only |
| execute:cancel | Cancel tasks | Scheduler, Operators |
| execute:debug | Debug/attach to tasks | Developers, Operators |
| execute:admin | Full administrative access | Sysadmins |

---

## 27. Permissions

### 27.1 Permission Scopes

```
execute.task.normal: Execute with standard isolation
execute.task.high_isolation: Execute with level 4 isolation
execute.task.no_isolation: Execute with level 0 (requires admin role)
execute.preempt: Preempt any task
execute.resume: Resume any suspended task
execute.cancel: Cancel any running task
execute.admin: View/change engine configuration
execute.monitor: View engine metrics and tasks
execute.debug: Attach to running tasks, view traces
```

### 27.2 Permission Validation Flow

```
1. Permission set extracted from JWT token in request
2. Permission set validated against required permissions:
   - execute:task required for all ExecuteTask calls
   - execute:preempt required for PreemptTask calls
3. If any required permission missing: return 403
4. Permission scope checked (e.g., department scope)
5. If cross-department access: require 'execute:admin' role
6. All permission checks logged in audit trail
```

---

## 28. Monitoring

### 28.1 Key Metrics

| Metric | Type | Description | Alert Threshold |
|---|---|---|---|
| engine.active_tasks | Gauge | Currently executing tasks | > 80% capacity |
| engine.task_throughput | Counter | Tasks completed per second | < 50% expected |
| engine.execution_latency | Histogram | Dispatch to completion time | > 30s p99 |
| engine.cpu_utilization | Gauge | Engine CPU usage | > 90% |
| engine.memory_usage | Gauge | Engine memory usage | > 90% |
| engine.failure_rate | Counter | Failed / total tasks | > 5% |
| engine.timeout_rate | Counter | Timed out / total tasks | > 2% |
| engine.preemption_rate | Counter | Preempted / total tasks | > 10 |
| engine.container_setup_time | Histogram | Container creation time | > 2s p99 |
| engine.isolation_failures | Counter | Isolation setup failures | > 0 |

### 28.2 Dashboards

```
Execution Engine Dashboard:
  - Active tasks: gauge per node (last 5 min sparkline)
  - Task throughput: rate chart (tasks/second)
  - Execution latency: p50/p95/p99/p99.9 distribution
  - Resource utilization: per-node CPU and memory heatmap
  - Failure rate: time series with error breakdown
  - Preemption events: timeline with cause
  - Isolation level distribution: pie chart
  - Top error types: bar chart (last hour)
  - Container setup times: distribution
  - Capacity utilization: current vs max
```

---

## 29. Logging

### 29.1 Log Levels

| Level | Usage | Examples |
|---|---|---|
| DEBUG | Detailed lifecycle | "Context created", "cgroup applied" |
| INFO | Normal operations | "Task started", "Task completed in 5.2s" |
| WARN | Anomalies | "Resource at 80%", "Hook failure (agent)" |
| ERROR | Failures | "Container creation failed", "Task error" |
| CRITICAL | System failure | "Engine state corrupted", "cgroup OOM" |

### 29.2 Log Structure

```
All log entries include:
  - timestamp: ISO8601
  - taskId: if applicable
  - workflowId: if available
  - nodeId: node name
  - agentType: which agent
  - level: LOG_LEVEL
  - correlationId: trace ID
  - message: human-readable message
  - durationMs: if timing-related
  - error: structured error data (if error)
```

---

## 30. Metrics

| Metric | Type | Tags | Description |
|---|---|---|---|
| engine.active.count | Gauge | node, department | Active tasks count |
| engine.started.total | Counter | mode, isolation, agent_type | Started task count |
| engine.completed.total | Counter | mode, status, agent_type | Completed task count |
| engine.failed.total | Counter | error_type, agent_type | Failed task count |
| engine.timeout.total | Counter | isolation_level, agent_type | Timeout count |
| engine.duration.ms | Histogram | mode, status | Execution duration |
| engine.cpu.usage | Gauge | node, task_id | CPU usage |
| engine.memory.usage.mb | Gauge | node, task_id | Memory usage |
| engine.iops.current | Gauge | node, task_id | Current IOPS |
| engine.resource.throttled | Counter | resource_type | Throttled task count |
| engine.isolation.setup.ms | Histogram | isolation_level | Isolation setup time |
| engine.hook.execution.ms | Histogram | hook_type, hook_phase | Hook execution time |
| engine.preemption.count | Counter | reason | Preemption count |
| engine.task.latency.ms | Histogram | status | Task lifecycle latency |

---

## 31. Tracing

### 31.1 Trace Spans

| Span Name | Start Event | End Event | Parent Span |
|---|---|---|---|
| engine.execute | ExecuteAPI called | Response sent | scheduler.dispatch |
| engine.initialize | Context creation | Context ready | engine.execute |
| engine.isolate | Isolation setup start | Isolation ready | engine.execute |
| engine.launch | Process start | Process confirmed | engine.execute |
| engine.monitor | Process monitoring start | Process exit | engine.execute |
| engine.hook.pre | Pre-hook start | Pre-hook done | engine.execute |
| engine.hook.post | Post-hook start | Post-hook done | engine.execute |
| engine.preempt | Preempt request received | Suspension done | scheduler.preempt |
| engine.resume | Resume request received | Process continued | scheduler.resume |
| engine.result | Result collection start | Result sent | engine.execute |

### 31.2 Trace Sampling

- P0 and P1 tasks: 100% trace sampling
- P2 tasks: 10% trace sampling
- P3 and P4 tasks: 1% trace sampling
- Failure events: always captured regardless of sample rate

---

## 32. Scalability

### 32.1 Horizontal Scaling

- Execution Engine scales by adding more compute nodes
- Each node runs its own Execution Engine instance
- No centralized execution state (node-local)
- Scheduler handles task distribution across nodes
- Capacity: linear scaling with number of nodes

### 32.2 Vertical Scaling

Per-Node Limits:
  - Max concurrent tasks: 500 (default, configurable up to 2000)
  - Max memory for task execution: 32 GB (configurable)
  - Task throughput: 100 tasks/second per node (typical)
  - Isolation setup throughput: 5 new tasks/second (container creation)

Scaling Strategy:
  - Primarily horizontal (add nodes)
  - Vertical scaling for capacity bursts
  - Auto-scaling based on queue depth and task latency

---

## 33. Performance

### 33.1 Performance Targets

| Operation | Target (p50) | Acceptable (p99) | Degraded | 
|---|---|---|---|
| Container/namespace setup | < 1s | < 3s | > 5s |
| Process launch | < 50ms | < 200ms | > 500ms |
| Resource monitoring tick | < 1ms | < 5ms | > 10ms |
| Hook execution (non-blocking) | < 500ms | < 2s | > 3s |
| Result collection | < 5ms | < 20ms | > 50ms |
| Trace recording per event | < 1ms | < 5ms | > 10ms |
| Full lifecycle (small agent) | < 2s | < 5s | > 10s |

### 33.2 Performance Optimizations

```
1. Image Pre-pull: Pre-pull common container images before scheduled execution
2. Namespace Reuse: Reuse PID and User namespaces when isolation allows
3. Container Pool: Maintain warm container pool for frequent agent types
4. Context Pool: Object pool for execution context structs to reduce GC
5. Lazy Mount: Mount filesystems on first access, not at process start
6. Batch Monitoring: Aggregate resource samples, batch writes to storage
7. Async Hooks: Non-blocking hooks executed in separate goroutine
8. Zero-copy Input: Share immutable input data between tasks (same input)
9. Compressed Traces: Background compression of trace data
10. CPU Pinning: CPU pinning for P0 tasks reduces context switch overhead
```

---

## 34. Optimization

### 34.1 Resource Optimization

```
CPU Optimization:
  - Dynamic CPU quota based on actual usage (overprovision idle)
  - Overcommit CPU for batch tasks (P3, P4) - 2x overcommit
  - Priority-based CPU scheduling: P0 tasks get CPU pinning
  - Idle detection and quota reduction

Memory Optimization:
  - Overcommit memory with oom_score adjustment (agents score higher)
  - Pages sharing between same-agent-type containers via KSM
  - Swap limited: small swap (256MB) for memory pressure relief
  - Compressed cache for infrequently accessed agent data

IOPS Optimization:
  - Write-back cache for agent outputs (compress+write in batch)
  - Asynchronous I/O for file operations
  - Block-level deduplication for identical agent outputs
```

### 34.2 Cold Start Optimization

```
Cold Start Threshold: < 500ms (target)
  - Pre-warm: launch placeholder containers for high-frequency agents
  - Snapshot: use CRIU checkpoint/restore for fast container start
  - Image optimization: minimize base image (< 50MB target)
  - Lazy loading: load only what's needed for first execution
  - Container reuse: reuse OS-level processes when isolation allows

Techniques:
  1. Container Snapshot: Save container state after initialization
  2. Reuse: Restore from snapshot for subsequent executions
  3. Time savings: 3s -> 200ms for container creation
  4. Image layers: Pre-cache common layers (base image + runtime)
```

---

## 35. Testing Strategy

### 35.1 Unit Tests

Coverage Targets:
  - Context initialization: 100% branch coverage
  - Isolation setup: 95% branch coverage
  - Timeout enforcement: 100% branch coverage
  - Hook execution: 90% branch coverage
  - Result handling: 95% branch coverage
  - Trace recording: 90% branch coverage

Test Categories:
  1. Sequential execution lifecycle (PENDING through COMPLETED)
  2. Fork-join with all subtasks succeeding
  3. Fork-join with one subtask failing (fail-fast)
  4. Fork-join with partial failure (fail-tolerant)
  5. Conditional execution (condition true and false)
  6. Iterative execution (bounded loop, early termination)
  7. Timeout enforcement (process termination)
  8. Timeout grace period (SIGTERM then SIGKILL)
  9. Preemption state capture (cooperative)
  10. Preemption forced suspension (non-cooperative)
  11. Resume from checkpoint
  12. Container/namespace setup failure recovery
  13. Hook execution and failure handling
  14. Resource limit enforcement (CPU, memory, IOPS)

### 35.2 Integration Tests

Scenarios:
  1. Scheduler -> Execution -> Result cycle
  2. Fork-join across multiple engine nodes
  3. Preemption -> Resume -> Completion
  4. Isolation level verification (each level 0-4)
  5. Resource limit enforcement for different resource types
  6. Trace recording and replay
  7. Hook execution within full task lifecycle
  8. Graceful shutdown and drain

### 35.3 Chaos Tests

Scenarios:
  1. Random SIGKILL during execution
  2. Node failure during task execution
  3. Disk full during output collection
  4. System OOM (near memory limit)
  5. Container runtime crash
  6. Clock skew during timeout enforcement
  7. Massive fork-join (500+ subtasks)
  8. Simultaneous preemption of multiple tasks
  9. Network partition during execution

---

## 36. Risk Analysis

### 36.1 Security Risks

| Risk | Impact | Likelihood | Mitigation |
|---|---|---|---|
| Escalation from container to host | Critical | Very Low | seccomp + capability drop + RW FS |
| Agent binary tampering | High | Low | SHA256 verification before launch |
| Namespace escape | Critical | Very Low | Multiple layers, regular updates |
| Side-channel attack (timing, cache) | Medium | Low | No shared CPU cache with sensitive tasks |
| Denial of service via task count | Medium | Medium | Concurrency limit, rate limiting, cgroups |
| Data exfiltration via network | High | Low | Network isolation, allowlist, proxy |

### 36.2 Operational Risks

| Risk | Impact | Likelihood | Mitigation |
|---|---|---|---|
| Orphaned tasks on crash | Medium | Low | Recovery procedure, Scheduler reconciliation |
| State inconsistency after restart | Medium | Low | Reconciliation with Scheduler |
| OOM killer taking engine process | Critical | Low | oom_score_adj = -1000 (protected) |
| Slow container setup for P0 | High | Medium | Container pre-pull, warm pool |
| Agent image pull timeout | Medium | Medium | Retry with backoff, cached images |
| cgroup accounting drift | Low | Medium | Periodic sync with cgroup counters |

---

## 37. Failure Scenarios

### 37.1 Failure Analysis

```
Scenario A: Container Runtime Failure
  Symptom: Container creation fails
  Impact: Task cannot execute
  Detection: Container create returns error
  Recovery:
    1. Retry container creation (max 2 attempts)
    2. If still fails: fallback to direct cgroups + namespace
    3. If fallback also fails: return RUNTIME_UNAVAILABLE
    4. Notify operations of runtime failure

Scenario B: Agent Process Dies Unexpectedly
  Symptom: Process exits with non-zero code
  Impact: Task fails to complete, may have partial output
  Detection: Process manager detects exit
  Recovery:
    1. Capture exit code, stderr output
    2. Capture any stdout (partial output)
    3. Record in trace
    4. Return FAILED with exit code and stderr
    5. Scheduler may retry based on exit code

Scenario C: Preemption State Lost
  Symptom: Cannot save preemption state
  Impact: Task may not be resumable
  Detection: State store write returns error
  Recovery:
    1. Retry state save (max 3 attempts)
    2. If all fails: abort preemption, keep task running
    3. Notify Scheduler: preemption failed
    4. Task continues execution

Scenario D: Trace Storage Failure
  Symptom: Cannot write trace data
  Impact: Trace data incomplete, but execution continues
  Detection: Trace write returns error
  Recovery: Continue without trace, log warning
  Post-recovery: Attempt to write trace summary at completion

Scenario E: Hook Script Failure
  Symptom: Hook returns non-zero exit
  Impact: Hook action not performed
  Detection: Hook execution result is failure
  Recovery:
    1. System hook failure: fail the task
    2. Agent hook failure: log warning, continue
    3. Post-hook failure: log warning, continue

Scenario F: Node OOM (Out of Memory)
  Symptom: System OOM killer activates
  Impact: Tasks killed, possibly engine
  Detection: OOM notification from kernel
  Recovery:
    1. Engine's oom_score_adj = -1000 (last to be killed)
    2. Agent tasks have normal oom_score (first to be killed)
    3. Killed tasks return OOM_KILLED error
    4. Reduce active task count, reduce memory oversubscription
```

### 37.2 Failure Propagation

```
Error Propagation Rules:
  1. Execution Engine errors bubble up to the Scheduler
  2. Scheduler determines if task should be retried
  3. Workflow-level errors bubble up to Workflow Engine
  4. Infrastructure errors (container, node) are handled locally
  5. Security/fencing errors cause immediate task termination
  6. Cascade prevention: each component handles its own recovery
  7. Error containment: engine errors do not affect other engines
```

---

## 38. Recovery Procedures

### 38.1 Post-Crash Recovery

```
Engine Crash Recovery:
  Step 1: Engine restarts automatically
  Step 2: Load configuration from Config Server
  Step 3: Connect to Scheduler
  Step 4: Report engine startup, request reconciliation
  Step 5: For each task known to Scheduler:
    a. Check if process is still alive
    b. If alive: reattach monitoring, continue execution
    c. If dead: capture exit, report termination
  Step 6: Clean up orphaned cgroups and namespaces
  Step 7: Resume accepting new execution requests

Orphaned Process Cleanup:
  1. Scan all surviving child processes
  2. Identify which agent each belongs to
  3. If not tracked: terminate process
  4. Capture any remaining output
  5. Log orphan cleanup in audit
```

### 38.2 Graceful Node Draining

```
Step 1: Receive shutdown signal (SIGTERM from orchestrator)
Step 2: Stop accepting new execution requests
Step 3: Calculate drain timeout (max 60 seconds)
Step 4: For each running task:
   a. P0 tasks: migrate immediately (checkpoint and migrate)
   b. P1 tasks: request migration from Scheduler
   c. P2-P4 tasks: wait for completion or timeout
Step 5: For each pending migration:
   a. Save checkpoint if available
   b. Notify Scheduler for rescheduling
Step 6: After drain timeout: force terminate remaining tasks
Step 7: Clean up all cgroups, namespaces, containers
Step 8: Notify Scheduler: node is offline
Step 9: Engine process exits
```

---

## 39. Operational Limits

### 39.1 Hard Limits

| Parameter | Limit | Behavior at Limit |
|---|---|---|
| Max concurrent tasks per node | 500 | Reject new tasks |
| Max memory per node for agents | 32 GB | cgroup enforcement, OOM risk |
| Max CPU cores per node for agents | 64 | CFS quota enforcement |
| Max task duration | 86400000ms (24h) | Hard timeout, SIGKILL |
| Max isolation setup time | 30000ms | Fail isolation, return error |
| Max preemptions per task | 3 | After 3, task is non-preemptible |
| Max containers per node | 1000 | Depends on node resources |
| Max hooks per lifecycle event | 10 | Additional hooks ignored |
| Max output size | 10 MB | Truncated with warning |
| Max trace events per task | 10000 | Stop recording new events |
| Max agent binary size | 500 MB | Reject as too large |

### 39.2 Soft Limits (Warning)

| Parameter | Warning Level | Action |
|---|---|---|
| Concurrent tasks > 80% capacity | Warning | Reduce new task rate |
| Memory usage > 80% | Warning | Limit new task memory allocation |
| CPU usage > 80% | Warning | CFS throttling may occur |
| Container setup > 2s | Warning | Use cached images |
| Preemption rate > 10/min | Warning | Check scheduler priority fairness |
| Failure rate > 5% | Critical | Alert operations |

---

## 40. Maintenance

### 40.1 Routine Tasks

| Task | Frequency | Impact | Duration |
|---|---|---|---|
| Image cache cleanup | Every 6 hours | None | < 1 second |
| Agent binary cache cleanup | Every 1 hour | None | < 100 ms |
| Old trace data purge | Every 1 hour | None (async) | < 5 seconds |
| Config reload check | Every 60 seconds | None | < 10 ms |
| Metrics flush | Every 10 seconds | Negligible | < 5 ms |
| Connection health check | Every 30 seconds | None | < 10 ms |
| Container runtime health | Every 10 seconds | None | < 100 ms |
| cgroups integrity check | Every 5 minutes | None | < 50 ms |
| Namespace leak detection | Every 15 minutes | None | < 100 ms |

### 40.2 Agent Binary Updates

```
Procedure:
  1. New agent version published to Agent Registry
  2. Execution Engine notified of version update
  3. Cache invalidation: remove old cached binary
  4. Fresh execution: fetch new version from registry
  5. Running tasks: continue with current binary version
  6. After all old-tasks complete: old version cache slot freed
  7. Rollback: re-publish old version to registry, clear cache

Version Compatibility:
  - Always fetch specific version when specified
  - Default to latest when version not specified
  - Version conflicts: use exact version from request
```

---

## 41. Future Improvements

### 41.1 Roadmap

```
Phase 2 (Near-term):
  - GPU and TPU support for ML agent execution
  - Firecracker microVM isolation for level 4
  - Meltdown/Spectre-safe isolation via KVM
  - Pre-baked execution environments per agent type
  - Predictive container pre-warming (ML-based)
  - WebAssembly (Wasm) execution as alternative to containers

Phase 3 (Mid-term):
  - eBPF-based resource monitoring (lower overhead)
  - Profile-based seccomp allowlist (generate from execution trace)
  - Rust-based agent execution sandbox for performance-critical agents
  - Multi-node task execution (distributed agents across nodes)
  - Dynamic resource rebalancing (move resources between tasks)
  - Execution cost accounting (monetary cost tracking per task)

Phase 4 (Long-term):
  - Confidential computing (TEE/Trusted Execution Environment)
  - Global execution: agents move between data centers
  - Execution pre-compilation (NVIDIA CUDA pre-compile)
  - Quantum execution environment (future quantum systems)
  - Auto-scaling execution environments for demand spikes
```

---

## 42. Examples

### 42.1 Standard Sequential Execution

```
Request:
  taskId: "val-task-001"
  agentType: "data-validator"
  input: "csv validation data"
  mode: SEQUENTIAL
  timeoutMs: 30000
  resources: { cpuCores: 1, memoryMB: 512 }
  isolationLevel: 2

Process:
  1. Validation passed
  2. Context created with input data
  3. Namespace isolation set up (PID, NET, IPC, Mount)
  4. cgroups: memory.max = 512M, cpu.max = 100000
  5. seccomp: default allowlist
  6. Capabilities: all dropped
  7. Process launched: data-validator
  8. Monitoring: CPU 0.8 cores, memory 120MB
  9. Duration: 5 seconds
  10. Exit code: 0

Response:
  Status: SUCCESS
  Output: { validated: true, issues: [] }
  ResourceUsage: { cpuPercent: 80, peakMemoryMB: 120, durationMs: 5000 }
  TraceId: "trace-abc-123"
```

### 42.2 Fork-Join with Failure

```
Request:
  taskId: "map-task-001"
  mode: FORK_JOIN
  input: [ chunk1, chunk2, chunk3, chunk4, chunk5 ]
  parallelism: 5
  failFast: true

Process:
  1. 5 subtasks created, each with independent isolation
  2. All 5 subtasks launch concurrently
  3. Subtask chunk3 fails (data corruption)
  4. failFast = true: cancel all other 4 subtasks
  5. Partial results from chunk1, chunk2, chunk4, chunk5 collected

Response:
  Status: FAILED (fail-fast)
  Error: {
    type: "SUBtask_FAILED",
    message: "chunk3: data corruption at offset 1024"
  }
  SubtaskResults: [
    { index: 0, status: SUCCESS, result: "..." },
    { index: 1, status: SUCCESS, result: "..." },
    { index: 2, status: FAILED, error: "data corruption" },
    { index: 3, status: CANCELLED, result: null },
    { index: 4, status: CANCELLED, result: null }
  ]
```

---

## 43. Acceptance Criteria

| # | Criteria | Verification Method |
|---|---|---|
| AC-1 | Task executes to completion, returns result | Integration test |
| AC-2 | Sequential execution returns correct output | Unit test |
| AC-3 | Fork-join returns all subtask results combined | Unit test |
| AC-4 | Fork-join with fail-fast cancels remaining tasks | Unit test |
| AC-5 | Conditional execution handles true and false paths | Unit test |
| AC-6 | Iterative execution bounded by iteration limit | Unit test |
| AC-7 | Timeout enforcement sends SIGTERM then SIGKILL | Integration test |
| AC-8 | Timeout captures partial output | Integration test |
| AC-9 | Preemption captures state and saves to store | Integration test |
| AC-10 | Preemption resumes from checkpoint | Integration test |
| AC-11 | Resource limits enforced via cgroups | Integration test |
| AC-12 | seccomp filter blocks unauthorized syscalls | Integration test |
| AC-13 | Namespace isolation prevents cross-task visibility | Integration test |
| AC-14 | Execution trace recorded for every task | Integration test |
| AC-15 | Trace replay reproduces execution timeline | Integration test |
| AC-16 | Engine can handle 500 concurrent tasks | Performance test |
| AC-17 | Engine recovers from crash without data loss | Chaos test |

---

## 44. Definition of Done

| # | Criteria | Owner |
|---|---|---|
| DoD-1 | All acceptance criteria pass | QA |
| DoD-2 | 95% unit test branch coverage | Engineering |
| DoD-3 | Integration tests pass for all execution modes | QA |
| DoD-4 | Performance targets met (500 concurrent, < 3s setup) | Performance |
| DoD-5 | Isolation security tests pass (no escape, no privilege) | Security |
| DoD-6 | Chaos tests pass: crash, OOM, network partition | QA |
| DoD-7 | Monitoring dashboards configured and verified | Ops |
| DoD-8 | Recovery procedures documented | Ops |
| DoD-9 | API documentation complete | Engineering |
| DoD-10 | Operational limits defined and enforced | Engineering |
| DoD-11 | Runbook created for failure scenarios | Ops |
| DoD-12 | Security audit completed | Security |

---

## End of Document 32: AIOS Execution Engine