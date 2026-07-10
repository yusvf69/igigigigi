# AIOS Process Manager Specification
## Document 04: Process Management

---

## Version History
| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2026-07-10 | AIOS Process Team | Initial specification |

---

## Table of Contents
1. Purpose and Mission
2. Process Model Overview
3. Agent as OS Process Model
4. Process States
5. Process Control Block (PCB)
6. Process Creation Sequence
7. Process Teardown Sequence
8. Process Hierarchy
9. Process Groups and Sessions
10. Orphan Process Handling
11. Zombie Process Prevention
12. Process Priority and Niceness
13. Process Resource Tracking
14. Process Statistics
15. Process Listing API
16. Bulk Process Operations
17. Process Health Monitoring
18. Process Configuration
19. Data Structures
20. Execution Flow
21. State Management
22. APIs and Communication
23. Validation
24. Error Recovery
25. Testing Strategy
26. Risk Analysis
27. Failure Scenarios
28. Operational Limits
29. Future Improvements
30. Acceptance Criteria

---

## 1. Purpose and Mission

### 1.1 Purpose
The AIOS Process Manager is the service responsible for managing the lifecycle of AI agent processes. It bridges the gap between the high-level agent abstraction and the low-level kernel process management, providing process creation, monitoring, and control capabilities.

### 1.2 Mission
To provide reliable, efficient, and observable process management for all AI agents running on AIOS, ensuring that every agent process is properly created, monitored, and cleaned up.

### 1.3 Responsibilities
- Agent process creation and termination
- Process state management and tracking
- Process hierarchy and parent-child relationships
- Process resource tracking (CPU, memory, IO)
- Process health monitoring with heartbeat
- Orphan process adoption
- Zombie process prevention and reaping
- Process priority and niceness management
- Process listing and introspection
- Bulk process operations (suspend, resume, kill)
- Process statistics and metrics collection

### 1.4 Non-Responsibilities
- Agent business logic execution (handled by agent processes)
- Task scheduling and assignment (Task Manager)
- Runtime sandbox management (Runtime Manager)
- High-level orchestration (Workflow Engine)

---

## 2. Process Model Overview

### 2.1 Agent as OS Process
In AIOS, each agent runs as a dedicated operating system process (or process tree). This provides:
- Strong isolation between agents (separate address spaces)
- OS-level resource accounting
- Standard process management tools
- Familiar debugging and monitoring

### 2.2 Process Types
| Process Type | Description | Parent | Lifetime |
|-------------|-------------|--------|----------|
| Agent process | Main agent execution | Process Manager | Task duration |
| Helper process | Sub-processes (tools, plugins) | Agent process | Sub-task duration |
| Worker thread | Thread within agent process | Agent process | Task step duration |
| Sidecar process | Supporting services | Process Manager | Agent lifetime |
| Supervisor | Monitoring/respawn | Process Manager | Permanent |

### 2.3 Process Model Diagram
```
+------------------------------------------------------------------+
|  Process Manager (service)                                        |
|  +------------------------------------------------------------+  |
|  |  Process Table                                                |  |
|  |  PID | Agent | State | PPID | PGroup | Session | Resources  |  |
|  |  -----+-------+-------+------+--------+---------+-----------  |  |
|  |  100  | cog-1 | RUN   | 1    | 100    | S1      | {cpu, mem} |  |
|  |  101  | cog-2 | BLK   | 1    | 101    | S1      | {cpu, mem} |  |
|  |  102  | gen-1 | RUN   | 1    | 102    | S1      | {cpu, mem} |  |
|  |  103  | gen-1 | SLEEP | 102  | 102    | S1      | {cpu, mem} |  |
|  |  104  | ret-1 | RUN   | 1    | 104    | S2      | {cpu, mem} |  |
|  +------------------------------------------------------------+  |
+------------------------------------------------------------------+
        |                        |                        |
        v                        v                        v
  +----------+             +----------+             +----------+
  | Agent    |             | Agent    |             | Agent    |
  | Process  |             | Process  |             | Process  |
  | (PID 100)|             | (PID 102)|             | (PID 104)|
  |  +-------+             |  +-------+             +----------+
  |  | Helper|             |  | Helper|
  |  |PID 103|             |  |PID 105|
  +---+------+             +---+------+
```

---

## 3. Agent as OS Process Model

### 3.1 Process Structure
Each agent process has:
- **PID**: Unique process identifier
- **PPID**: Parent process ID (usually Process Manager)
- **PGID**: Process group ID (all processes in agent's group)
- **SID**: Session ID
- **UID/GID**: User and group for access control
- **Address space**: Virtual memory (code, data, heap, stack)
- **File descriptors**: stdin, stdout, stderr, IPC channels
- **Signal handlers**: SIGTERM, SIGKILL, SIGUSR1, SIGUSR2
- **Resource limits**: CPU, memory, file size, processes

### 3.2 Agent Process Environment
```bash
# Process environment
AIOS_PID=<pid>
AIOS_PPID=<ppid>
AIOS_AGENT_ID=<agent_id>
AIOS_TASK_ID=<task_id>
AIOS_TENANT_ID=<tenant_id>
AIOS_SANDBOX_ID=<sandbox_id>
AIOS_TRACE_ID=<trace_id>
AIOS_SPAN_ID=<span_id>
PATH=/aios/bin:/usr/local/bin:/usr/bin:/bin
HOME=/tmp/aios/<sandbox_id>
```

### 3.3 Process Creation Method
- **fork() + exec()**: Standard UNIX process creation
- **clone()**: For threads and namespaces
- **spawn()**: For container-based processes
- **vfork()**: For minimal-overhead execution

---

## 4. Process States

### 4.1 State Definitions
| State | Label | Description | CPU Allocated |
|-------|-------|-------------|---------------|
| PENDING | P | Process being created | No |
| READY | R | Runnable, waiting for CPU | No |
| RUNNING | RUN | Currently executing | Yes |
| BLOCKED | S | Sleeping/interruptible wait | No |
| UNINTERRUPTIBLE | D | Uninterruptible sleep (I/O) | No |
| STOPPED | T | Stopped (SIGSTOP/SIGTSTP) | No |
| TRACED | t | Being traced by ptrace | No |
| ZOMBIE | Z | Terminated, waiting for parent | No |
| COMPLETED | C | Exited normally, reaped | No |
| FAILED | F | Exited with error, reaped | No |
| TERMINATED | K | Killed by signal, reaped | No |

### 4.2 State Transition Diagram
```
             +-----------+
             |  PENDING  |
             +-----+-----+
                   |
                   v
             +-----------+
        +--->|   READY   |<---+
        |    +-----+-----+    |
        |          |          |
        |          v          |
        |    +-----------+    |
        |    |  RUNNING  |    |
        |    +-----+-----+    |
        |     /    |    \     |
        |    v     v     v    |
   +---------+ +------+ +--+ |
   | BLOCKED | |STOPPED| |Z | |
   +----+----+ +------+ +--+ |
        |                |    |
        v                |    |
   +---------+           |    |
   |  READY  |-----------+----+
   +---------+
   
   RUNNING -> COMPLETED (exit(0))
   RUNNING -> FAILED (exit(non-zero))
   RUNNING -> TERMINATED (SIGKILL)
   any -> ZOMBIE -> COMPLETED/FAILED (after wait)
```

---

## 5. Process Control Block (PCB)

### 5.1 PCB Structure (User-Space View)
```rust
struct ProcessControlBlock {
    // Identity
    pid: u32,
    ppid: u32,
    pgid: u32,
    sid: u32,
    
    // Agent info
    agent_id: String,
    agent_type: AgentType,
    task_id: Option<String>,
    workflow_id: Option<String>,
    tenant_id: String,
    sandbox_id: String,
    
    // State
    state: ProcessState,
    sub_state: String,        // Wait channel, block reason
    exit_code: Option<i32>,
    exit_signal: Option<u32>,
    
    // Priority
    priority: i32,             // -20 to 19
    nice: i32,                 // -20 to 19
    rt_priority: u32,          // 0-99 (0 = not RT)
    
    // Credentials
    uid: u32,
    gid: u32,
    euid: u32,
    egid: u32,
    
    // Resource usage (from kernel)
    resource_usage: ResourceUsage,
    
    // Timestamps
    timestamps: ProcessTimestamps,
    
    // Statistics
    stats: ProcessStats,
    
    // Relationships
    children: Vec<u32>,
    siblings: Vec<u32>,
    
    // File descriptors
    open_fds: u32,
    max_fds: u32,
    
    // Capabilities
    capabilities: Vec<String>,
    
    // Health
    last_heartbeat: Option<u64>,
    heartbeat_failures: u32,
}
```

### 5.2 Resource Usage Structure
```rust
struct ResourceUsage {
    // CPU
    user_cpu_us: u64,
    system_cpu_us: u64,
    voluntary_ctx_switches: u64,
    involuntary_ctx_switches: u64,
    
    // Memory
    rss: u64,                   // Resident set size (bytes)
    vms: u64,                   // Virtual memory size (bytes)
    shared: u64,                // Shared memory (bytes)
    data: u64,                  // Data segment (bytes)
    stack: u64,                 // Stack size (bytes)
    code: u64,                  // Code segment (bytes)
    
    // I/O
    io_read_bytes: u64,
    io_write_bytes: u64,
    io_read_ops: u64,
    io_write_ops: u64,
    
    // Network
    network_rx_bytes: u64,
    network_tx_bytes: u64,
    network_rx_packets: u64,
    network_tx_packets: u64,
    
    // Signals
    signals_received: u64,
    signals_pending: u32,
    
    // Page faults
    minor_faults: u64,
    major_faults: u64,
    
    // IPC
    ipc_messages_sent: u64,
    ipc_messages_recv: u64,
}
```

### 5.3 Process Timestamps
```rust
struct ProcessTimestamps {
    created_at: u64,            // Epoch ns (process creation)
    ready_at: Option<u64>,      // First time READY
    started_at: Option<u64>,    // First time RUNNING
    blocked_at: Option<u64>,    // Last time BLOCKED
    stopped_at: Option<u64>,    // Time of STOP signal
    terminated_at: Option<u64>, // Time of termination
    user_time: u64,             // CPU time in user mode (ns)
    system_time: u64,           // CPU time in kernel mode (ns)
    wall_time: u64,             // Real elapsed time (ns)
    last_stat_refresh: u64,     // Last time stats were read
}
```

---

## 6. Process Creation Sequence

### 6.1 Creation Steps
```
1. Process Manager receives CreateProcess request
2. Validate request (agent_id, resource limits, etc.)
3. Allocate PID from process table
4. Initialize PCB with process metadata
5. Set up process credentials (UID, GID, capabilities)
6. Set up process environment variables
7. Open standard file descriptors (stdin, stdout, stderr)
8. Set up IPC channels for agent communication
9. Call kernel sys_fork() or sys_exec()
10. Wait for kernel to confirm process creation
11. Set initial process state to READY
12. Register process in process groups and sessions
13. Publish process.created event
14. Return PID to caller
```

### 6.2 Creation Request
```json
{
  "agent_id": "cog-001",
  "agent_type": "cognitive",
  "tenant_id": "tenant-abc",
  "task_id": "task-456",
  "workflow_id": "wf-789",
  "sandbox_id": "sand-012",
  "binary_path": "/aios/agents/cognitive-agent",
  "arguments": ["--task-id", "task-456", "--config", "standard"],
  "environment": {
    "AIOS_MODE": "production",
    "LOG_LEVEL": "info"
  },
  "resource_limits": {
    "cpu_quota": 100000,
    "memory_max": 4294967296,
    "pids_max": 256
  },
  "credentials": {
    "uid": 1001,
    "gid": 2001,
    "capabilities": ["CAP_NET_BIND_SERVICE", "CAP_KILL"]
  },
  "priority": 10,
  "nice": 0
}
```

---

## 7. Process Teardown Sequence

### 7.1 Normal Termination
```
1. Agent process calls exit() or returns from main()
2. Kernel collects exit code
3. Kernel sends SIGCHLD to parent (Process Manager)
4. Process state changes to ZOMBIE
5. Process Manager catches SIGCHLD
6. Process Manager calls waitpid(pid, &status, WNOHANG)
7. Kernel releases process resources:
   - Close file descriptors
   - Unmap memory regions
   - Release IPC channels
   - Release kernel stack
   - Update process accounting
8. Process Manager reads final resource usage
9. Process Manager removes PID from process table
10. Publish process.completed event
11. Reap zombie (state -> COMPLETED)
```

### 7.2 Force Termination
```
1. Process Manager receives KillProcess(pid, signal) request
2. Validate process exists and authorization
3. Log termination request
4. Call kernel sys_kill(pid, signal)
   - SIGTERM: grace period (10s default)
   - SIGKILL: immediate termination
5. If SIGTERM and process still alive after grace:
   - Send SIGKILL
6. Follow normal termination from step 4+
7. Publish process.terminated event
```

### 7.3 Teardown Request
```json
{
  "pid": 1234,
  "action": "terminate",
  "signal": "SIGTERM",
  "grace_period_ms": 10000,
  "force": false,
  "reason": "Task timeout exceeded",
  "collect_dump": true
}
```

---

## 8. Process Hierarchy

### 8.1 Hierarchy Rules
- Every process (except PID 1) has a parent
- Children are reparented to init (PID 1) if parent dies
- Process groups contain one or more processes
- Sessions contain one or more process groups
- Session leader is the first process in the session

### 8.2 Hierarchy Example
```
PID 1 (init/process-manager)
  |
  +-- PID 100 (cognitive-agent, pgid=100, sid=1)
  |     |
  |     +-- PID 101 (helper-tool, pgid=100, sid=1)
  |     +-- PID 102 (helper-tool, pgid=100, sid=1)
  |
  +-- PID 200 (generation-agent, pgid=200, sid=1)
  |     |
  |     +-- PID 201 (model-inference, pgid=200, sid=1)
  |
  +-- PID 300 (retrieval-agent, pgid=300, sid=2)
  |
  +-- PID 400 (monitoring-agent, pgid=400, sid=2)
```

### 8.3 Parent-Child API
```rust
impl ProcessManager {
    fn get_children(pid: u32) -> Vec<ProcessInfo>;
    fn get_parent(pid: u32) -> Option<ProcessInfo>;
    fn get_process_tree(pid: u32) -> ProcessTree;
    fn get_group_processes(pgid: u32) -> Vec<ProcessInfo>;
    fn get_session_processes(sid: u32) -> Vec<ProcessInfo>;
    fn reparent_children(pid: u32, new_parent: u32);
}
```

---

## 9. Process Groups and Sessions

### 9.1 Process Groups
- A process group is a collection of related processes
- All processes in a group receive the same signal
- Used for job control and bulk operations
- Each process group has a leader (first process)
- PGID equals PID of group leader

### 9.2 Sessions
- A session is a collection of process groups
- All processes in a session share a controlling terminal
- Session leader is the first process in the session
- Used for managing user login sessions
- SID equals PID of session leader

### 9.3 Operations
```rust
impl ProcessGroup {
    fn create_group(leader_pid: u32) -> u32;     // Returns PGID
    fn join_group(pid: u32, pgid: u32);
    fn exit_group(pgid: u32);
    fn signal_group(pgid: u32, signal: Signal);
    fn suspend_group(pgid: u32);                   // SIGSTOP
    fn resume_group(pgid: u32);                    // SIGCONT
}

impl Session {
    fn create_session(leader_pid: u32) -> u32;     // Returns SID
    fn signal_session(sid: u32, signal: Signal);
    fn get_foreground_group(sid: u32) -> u32;
    fn set_foreground_group(sid: u32, pgid: u32);
}
```

---

## 10. Orphan Process Handling

### 10.1 Orphan Detection
A process becomes orphaned when its parent terminates before it does. AIOS handles this through reparenting.

### 10.2 Reparenting Rules
1. When a process terminates, its children are reparented
2. New parent depends on process type:
   - Normal agents -> Process Manager (if still alive)
   - System processes -> PID 1 (init)
3. The reparented process continues execution normally
4. New parent handles wait() calls for reaping

### 10.3 Orphan Prevention
- Process Manager acts as immediate parent for all agents
- Agents are NOT direct children of each other
- Helper processes are children of agent, reparented to PM
- Orphan detection is part of health monitoring

---

## 11. Zombie Process Prevention

### 11.1 Zombie Creation
A zombie process is created when:
1. A child process terminates
2. The parent has not yet called wait()
3. The kernel keeps the exit status until wait()

### 11.2 Zombie Prevention Strategy
```rust
impl ZombiePrevention {
    // Strategy 1: SIGCHLD handler with waitpid/WNOHANG
    fn handle_sigchld() {
        loop {
            match waitpid(-1, &mut status, WNOHANG) {
                Ok(0) => break,           // No more children
                Ok(pid) => self.reap(pid), // Reap child
                Err(_) => break,
            }
        }
    }
    
    // Strategy 2: Double-fork technique
    fn create_process(): u32 {
        let child = fork();
        if child == 0 {
            // Grandchild
            if fork() == 0 {
                // Actual agent process
                exec(agent_path);
            }
            exit(0);  // Child exits immediately
        }
        waitpid(child);  // Reap child immediately
        // Grandchild is reparented to init
    }
    
    // Strategy 3: Process Manager as immediate parent
    // All agents are direct children of PM
    // PM has SIGCHLD handler that reaps immediately
    
    // Strategy 4: Periodic zombie reaper
    fn reap_zombies_periodically() {
        for pid in self.get_zombie_processes() {
            waitpid(pid);
        }
    }
}
```

### 11.3 Zombie Monitoring
```yaml
zombie_monitoring:
  check_interval: 10s
  max_zombie_threshold: 100
  auto_reap: true
  alert_on_high_zombie: true
  
  actions:
    - zombie_count > 100: Warning alert
    - zombie_count > 1000: Critical alert + force reaping
    - zombie_age > 300s: Log + attempt waitpid
    - zombie from dead parent: Reparent + waitpid
```

---

## 12. Process Priority and Niceness

### 12.1 Priority Overview
| Priority Range | Class | Description |
|---------------|-------|-------------|
| -20 to -11 | Real-time | Latency-critical agents |
| -10 to 0 | High priority | Interactive agents |
| 1 to 10 | Normal priority | Standard agents |
| 11 to 19 | Low priority | Background agents |
| 20 | Idle | Only runs when nothing else |

### 12.2 Priority Management
```rust
impl PriorityManager {
    // Set process nice value (-20 to 19)
    fn set_nice(pid: u32, nice: i32);
    
    // Get current process priority
    fn get_priority(pid: u32) -> ProcessPriority;
    
    // Calculate dynamic priority based on behavior
    fn calculate_dynamic_priority(pid: u32) -> i32 {
        let base = CURRENT_PCB[pid].nice;
        let bonus = self.get_interactive_bonus(pid);
        let penalty = self.get_cpu_penalty(pid);
        clamp(base + bonus - penalty, -20, 19)
    }
    
    // Interactive bonus: short sleepers get higher priority
    fn get_interactive_bonus(pid: u32) -> i32 {
        let sleep_avg = CURRENT_PCB[pid].stats.sleep_avg;
        (sleep_avg / 1000) as i32  // 0-10 bonus
    }
    
    // CPU penalty: CPU hogs get lower priority
    fn get_cpu_penalty(pid: u32) -> i32 {
        let cpu_usage = CURRENT_PCB[pid].resource_usage.user_cpu_us;
        (cpu_usage / 1000000) as i32  // 0-5 penalty
    }
}
```

### 12.3 Priority Inheritance
- Child processes inherit parent's priority
- Priority can be changed dynamically
- Priority aging: long-waiting processes get boosted
- Priority decay: CPU-intensive processes get reduced

---

## 13. Process Resource Tracking

### 13.1 Resources Tracked
| Resource | Collection Method | Unit | Granularity |
|----------|------------------|------|-------------|
| CPU time | /proc/[pid]/stat | microseconds | Real-time |
| Memory RSS | /proc/[pid]/status | bytes | 1s interval |
| Virtual memory | /proc/[pid]/status | bytes | 1s interval |
| I/O bytes | /proc/[pid]/io | bytes | 5s interval |
| I/O operations | /proc/[pid]/io | count | 5s interval |
| Context switches | /proc/[pid]/status | count | 1s interval |
| Open files | /proc/[pid]/fd | count | On signal |
| Network I/O | cgroup net_prio | bytes | 10s interval |

### 13.2 Resource Accounting
```rust
struct ResourceAccounting {
    limits: ResourceLimits,
    current: ResourceUsage,
    peak: ResourceUsage,
    total: ResourceUsage,
    
    // Tracking
    measurement_count: u64,
    last_measurement: u64,
    
    // Alerts
    exceeded_limits: Vec<LimitEvent>,
    
    // Charging
    cost: ResourceCost,
}

impl ResourceAccounting {
    fn measure(pid: u32) -> ResourceUsage;
    fn check_limits(&self) -> Vec<LimitCheck>;
    fn calculate_cost(&self) -> ResourceCost;
    fn generate_report(&self) -> ResourceReport;
}
```

---

## 14. Process Statistics

### 14.1 Exposed Statistics
```yaml
process_statistics:
  per_process:
    - pid, ppid, pgid, sid
    - state, priority, nice
    - cpu_user, cpu_system, cpu_total
    - memory_rss, memory_vms, memory_swap
    - io_read_bytes, io_write_bytes
    - io_read_ops, io_write_ops
    - ctx_switches_voluntary, ctx_switches_involuntary
    - page_faults_minor, page_faults_major
    - signals_pending, signals_received
    - open_fds, open_fds_limit
    - threads_count
    - age_seconds
    - start_time

  aggregated:
    - total_processes
    - running_processes
    - blocked_processes
    - zombie_processes
    - stopped_processes
    - total_cpu_percent
    - total_memory_percent
    - total_io_read_bytes
    - total_io_write_bytes
    - process_creation_rate
    - process_termination_rate
    - avg_process_lifetime
    - max_process_lifetime
```

### 14.2 Statistics Collection
```rust
impl StatsCollector {
    fn collect(pid: u32) -> ProcessStats {
        let stat = read_proc_stat(pid);
        let status = read_proc_status(pid);
        let io = read_proc_io(pid);
        
        ProcessStats {
            pid,
            state: parse_state(stat.state),
            cpu_user: stat.utime * TICK_NS,
            cpu_system: stat.stime * TICK_NS,
            memory_rss: status.vm_rss * PAGE_SIZE,
            memory_vms: status.vm_size,
            io_read_bytes: io.read_bytes,
            io_write_bytes: io.write_bytes,
            ctx_switches_vol: status.nivcsw,
            ctx_switches_invol: status.nvcsw,
            page_faults_minor: stat.minflt,
            page_faults_major: stat.majflt,
            open_fds: count_fds(pid),
            threads: status.threads,
            age: now() - self.pcb.created_at,
        }
    }
    
    fn collect_aggregated() -> AggregatedStats {
        let processes = self.process_table.iter();
        AggregatedStats {
            total: processes.len(),
            running: processes.filter(|p| p.state == RUNNING).count(),
            blocked: processes.filter(|p| p.state == BLOCKED).count(),
            zombie: processes.filter(|p| p.state == ZOMBIE).count(),
            // ...
        }
    }
}
```

---

## 15. Process Listing API

### 15.1 API Endpoints
```yaml
process_api:
  list:
    path: /api/v1/processes
    method: GET
    params:
      - name: state
        type: string
        description: Filter by state (RUNNING, BLOCKED, etc.)
      - name: agent_type
        type: string
        description: Filter by agent type
      - name: tenant_id
        type: string
        description: Filter by tenant
      - name: sort
        type: string
        description: Sort field (cpu, memory, age)
      - name: limit
        type: integer
        default: 100
      - name: offset
        type: integer
        default: 0
  
  get:
    path: /api/v1/processes/{pid}
    method: GET
    response: ProcessInfo (detailed PCB + stats)
  
  get_tree:
    path: /api/v1/processes/{pid}/tree
    method: GET
    response: ProcessTree (hierarchical view)
  
  get_group:
    path: /api/v1/processes/{pid}/group
    method: GET
    response: [ProcessInfo] (all processes in group)
```

### 15.2 API Response Format
```json
{
  "processes": [
    {
      "pid": 1234,
      "ppid": 1,
      "pgid": 1234,
      "sid": 1,
      "agent_id": "cog-001",
      "agent_type": "cognitive",
      "task_id": "task-456",
      "state": "RUNNING",
      "priority": 10,
      "nice": 0,
      "cpu_percent": 45.2,
      "memory_bytes": 2147483648,
      "open_fds": 24,
      "threads": 8,
      "age_seconds": 3600,
      "started_at": "2026-07-10T12:00:00Z",
      "resource_usage": {
        "cpu_user_us": 500000000,
        "cpu_system_us": 120000000,
        "ctx_switches_vol": 15000,
        "ctx_switches_invol": 5000,
        "io_read_bytes": 1073741824,
        "io_write_bytes": 536870912
      }
    }
  ],
  "total": 1,
  "limit": 100,
  "offset": 0
}
```

---

## 16. Bulk Process Operations

### 16.1 Supported Operations
| Operation | Action | Signal | Graceful |
|-----------|--------|--------|----------|
| suspend | Stop process | SIGSTOP | Yes |
| resume | Continue process | SIGCONT | N/A |
| terminate | Kill process | SIGTERM | Yes (grace period) |
| force_kill | Force kill | SIGKILL | No |
| restart | Restart | SIGTERM + exec | Partial |
| set_priority | Change priority | N/A | N/A |

### 16.2 Bulk Operation Request
```json
{
  "operation": "terminate",
  "filters": {
    "agent_type": "cognitive",
    "tenant_id": "tenant-abc",
    "state": "RUNNING",
    "older_than_ms": 3600000
  },
  "signal": "SIGTERM",
  "grace_period_ms": 30000,
  "force_after_ms": 30000,
  "max_processes": 100,
  "reason": "Scheduled maintenance"
}
```

### 16.3 Bulk Operation Execution
```rust
impl BulkOperations {
    fn execute_bulk(request: BulkRequest) -> BulkResponse {
        let processes = self.find_processes(request.filters);
        let mut results = Vec::new();
        
        for process in processes.take(request.max_processes) {
            match request.operation {
                "terminate" => {
                    let result = self.graceful_terminate(process.pid, 
                        request.grace_period_ms, request.force_after_ms);
                    results.push(result);
                }
                "suspend" => {
                    sys_kill(process.pid, SIGSTOP);
                    results.push(OperationResult::success(process.pid));
                }
                // ...
            }
        }
        
        BulkResponse {
            operation: request.operation,
            total_requested: processes.len(),
            successful: results.iter().filter(|r| r.success).count(),
            failed: results.iter().filter(|r| !r.success).count(),
            results,
        }
    }
}
```

---

## 17. Process Health Monitoring

### 17.1 Health Check Mechanism
```yaml
health_monitoring:
  heartbeat:
    interval: 15s
    timeout: 30s
    mechanism: 
      - Process sends heartbeat via IPC channel
      - If no heartbeat for 30s -> probe liveness
      - If probe fails -> mark UNHEALTHY
      - If UNHEALTHY for 60s -> auto-terminate + restart
  
  liveness_probe:
    type: signal (SIGUSR2)
    response_expected: signal (SIGUSR2 to parent)
    timeout: 5s
    retries: 3
  
  readiness_probe:
    type: IPC message
    response_expected: "ready"
    timeout: 5s
  
  process_state_check:
    interval: 30s
    checks:
      - Is process in kernel process table?
      - Is process in expected state?
      - Is process consuming expected resources?
      - Is process within resource limits?
```

### 17.2 Health States
| State | Definition | Action |
|-------|------------|--------|
| HEALTHY | Heartbeat received recently | None |
| DEGRADED | Heartbeat delayed > 2x interval | Log warning |
| UNHEALTHY | Heartbeat missed > timeout | Send probe |
| STALLED | Probe failed, no response | Attempt recovery |
| DEAD | Process table entry missing | Clean up PCB |
| ZOMBIE | Process is zombie | Reap immediately |

### 17.3 Health Monitoring Implementation
```rust
impl HealthMonitor {
    fn check_heartbeat(sandbox_id: &str) -> HealthStatus {
        let pcb = self.process_table.get_by_sandbox(sandbox_id);
        let now = current_time_ns();
        let since_heartbeat = now - pcb.last_heartbeat;
        
        if since_heartbeat < HEARTBEAT_INTERVAL * 2 {
            HEALTHY
        } else if since_heartbeat < HEARTBEAT_TIMEOUT {
            DEGRADED
        } else {
            // Send liveness probe
            match self.liveness_probe(pcb.pid) {
                Ok(_) => DEGRADED,
                Err(_) => UNHEALTHY,
            }
        }
    }
    
    fn liveness_probe(pid: u32) -> Result<(), Error> {
        // Send SIGUSR2, expect SIGUSR2 back within timeout
        let result = with_timeout(5_000_000_000, || {
            sys_kill(pid, SIGUSR2);
            wait_for_signal(SIGUSR2, pid)
        });
        result
    }
}
```

---

## 18. Process Configuration

### 18.1 Global Configuration
```yaml
process_manager:
  config:
    max_processes: 1000000
    max_processes_per_tenant: 10000
    max_processes_per_agent: 100
    default_priority: 10
    default_nice: 0
    
    heartbeat:
      interval_seconds: 15
      timeout_seconds: 30
      unhealthy_threshold: 2
      stall_threshold: 4
      dead_threshold: 10
    
    zombie_prevention:
      enabled: true
      reap_interval: 10
      max_zombie_threshold: 100
    
    resource_tracking:
      enabled: true
      cpu_interval: 1
      memory_interval: 1
      io_interval: 5
      network_interval: 10
    
    orphan_handling:
      reparent_to_init: false
      reparent_to_pm: true
      auto_reap_orphans: true
    
    cleanup:
      completed_pcb_retention: 3600
      failed_pcb_retention: 86400
      terminated_pcb_retention: 3600
      cleanup_interval: 300
```

---

## 19. Data Structures

### 19.1 Process Table
```rust
struct ProcessTable {
    processes: HashMap<u32, ProcessControlBlock>,
    
    // Indexes
    by_agent_id: HashMap<String, u32>,
    by_task_id: HashMap<String, u32>,
    by_sandbox_id: HashMap<String, u32>,
    by_tenant: HashMap<String, Vec<u32>>,
    by_state: EnumMap<ProcessState, Vec<u32>>,
    by_agent_type: EnumMap<AgentType, Vec<u32>>,
    
    // Tree structure
    children: HashMap<u32, Vec<u32>>,
    
    // Statistics
    stats: TableStats,
    
    // Lock
    lock: RwLock,
}

struct TableStats {
    total_created: u64,
    total_terminated: u64,
    current_count: u32,
    peak_count: u32,
    zombie_count: u32,
    orphan_count: u32,
}
```

### 19.2 ProcessInfo (API Response)
```rust
#[derive(Serialize)]
struct ProcessInfo {
    pid: u32,
    ppid: u32,
    pgid: u32,
    sid: u32,
    agent_id: String,
    agent_type: AgentType,
    task_id: Option<String>,
    tenant_id: String,
    state: ProcessState,
    priority: i32,
    nice: i32,
    cpu_percent: f64,
    memory_bytes: u64,
    open_fds: u32,
    threads: u32,
    age_seconds: u64,
    started_at: DateTime<Utc>,
    resource_usage: ResourceUsageSummary,
}
```

---

## 20. Execution Flow

### 20.1 Process Manager Service Flow
```
Startup:
  1. Initialize process table
  2. Register SIGCHLD handler
  3. Connect to event system
  4. Start heartbeat checker
  5. Start zombie reaper
  6. Start stats collector
  7. Signal ready to orchestrator

Main loop:
  1. Wait for requests (gRPC/HTTP)
  2. Process creation/deletion requests
  3. Handle SIGCHLD (reap zombies)
  4. Periodic health checks
  5. Periodic stats collection
  6. Periodic cleanup of old PCB entries
  7. Respond to monitoring queries
```

---

## 21. State Management

| State | Storage | Persistence | Recovery |
|-------|---------|-------------|----------|
| Process table | In-memory + database | Recoverable | Rebuild from kernel + DB |
| PCB | In-memory | Ephemeral | Rebuild from /proc |
| Stats | In-memory + metrics | Metrics | Recalculated |
| Health state | In-memory | Ephemeral | Re-detected |
| Configuration | Database | Persistent | Ready from DB |

---

## 22. APIs and Communication

### 22.1 Internal gRPC API
```protobuf
service ProcessManager {
    rpc CreateProcess(CreateProcessRequest) returns (CreateProcessResponse);
    rpc GetProcess(GetProcessRequest) returns (ProcessInfo);
    rpc ListProcesses(ListProcessesRequest) returns (ListProcessesResponse);
    rpc KillProcess(KillProcessRequest) returns (KillProcessResponse);
    rpc SetPriority(SetPriorityRequest) returns (SetPriorityResponse);
    rpc BulkOperation(BulkOperationRequest) returns (BulkOperationResponse);
    rpc GetProcessTree(GetProcessTreeRequest) returns (ProcessTree);
    rpc GetProcessStats(GetProcessStatsRequest) returns (ProcessStats);
    rpc WatchProcess(WatchProcessRequest) returns (stream ProcessEvent);
}
```

### 22.2 Events Published
```yaml
events:
  process.created:
    payload: {pid, agent_id, tenant_id, task_id}
  
  process.completed:
    payload: {pid, exit_code, resource_usage}
  
  process.failed:
    payload: {pid, exit_code, error_message}
  
  process.terminated:
    payload: {pid, signal, reason}
  
  process.orphaned:
    payload: {pid, new_parent_pid}
  
  process.zombie:
    payload: {pid, zombie_duration}
  
  process.health_changed:
    payload: {pid, old_state, new_state}
  
  process.resource_exceeded:
    payload: {pid, resource, limit, current}
```

---

## 23. Validation

| Check | Input | Action on Failure |
|-------|-------|-------------------|
| PID exists | Any pid parameter | Return 404 |
| Agent ID exists | CreateProcess | Return 400 |
| Tenant quota | CreateProcess | Return 429 |
| Process ownership | Kill/SetPriority | Return 403 |
| Process state | Kill (SIGTERM already dead) | Return 409 |
| Priority range | SetPriority | Clamp to valid range |
| Resource limits | CreateProcess | Return 422 |
| Authorization | All operations | Return 403 |

---

## 24. Error Recovery

| Error | Detection | Recovery |
|-------|-----------|----------|
| Process crash | SIGCHLD | Capture exit code, notify task manager |
| Stale PCB | Missing from /proc | Remove from table, emit event |
| Zombie accumulation | Count > threshold | Force reap all zombies |
| Orphaned process | Parent missing | Reparent to Process Manager |
| Heartbeat failure | No heartbeat | Liveness probe, terminate if dead |
| Process table full | Allocation failure | Reject new processes, notify auto-scaler |
| Resource abuse | Kernel OOM | Terminate process, emit alert |

---

## 25. Testing Strategy

| Test | Scope | Method |
|------|-------|--------|
| Unit | PCB, state transitions, priority management | Go test |
| Integration | Process creation/teardown with kernel | Integration harness |
| Hierarchy | Parent-child, orphan, zombie scenarios | Scenario testing |
| Load | 100K concurrent processes | Load generator |
| Resilience | Crash recovery, reconnection | Chaos testing |
| Migration | Process state persistence and recovery | Migration tests |

---

## 26. Risk Analysis

| Risk | Probability | Impact | Mitigation |
|------|------------|--------|------------|
| Process table corruption | Low | Critical | Lock protection, hash validation |
| Zombie DoS | Low | Medium | Reaper, threshold alerts |
| Orphan leak | Low | Medium | Reparenting, periodic cleanup |
| PID exhaustion | Low | High | PID reuse, monitoring |
| State inconsistency | Low | High | Reconciliation with /proc |
| Resource tracking drift | Medium | Low | Periodic recalibration |
| Health check storm | Low | Medium | Rate limiting, exponential backoff |

---

## 27. Failure Scenarios

### 27.1 Process Manager Crash
- **Symptoms**: No process management, no health checks
- **Detection**: Service mesh health check fails
- **Action**: 
  1. Failover to standby Process Manager
  2. New instance reconciles state from /proc
  3. Replay events from event store
  4. Resume health checks
- **Recovery time**: < 30s
- **Impact**: No new processes during failover, existing processes continue

### 27.2 PID Exhaustion
- **Symptoms**: Cannot create new processes
- **Detection**: PID allocation returns error
- **Action**:
  1. Check for zombies to reap
  2. Check for long-completed processes to clean
  3. If still exhausted, scale to new node
- **Mitigation**: Configure pid_max appropriately

---

## 28. Operational Limits

| Resource | Soft Limit | Hard Limit |
|----------|-----------|-----------|
| Processes per PM instance | 100,000 | 1,000,000 |
| Process creation rate | 1,000/s | 5,000/s |
| PID range | 32,768-4,194,304 | 4,194,304 |
| Zombie threshold | 100 | 1,000 |
| Health check interval | 15s | 5s |
| PCB retention (completed) | 1h | 24h |
| Bulk operation size | 100 | 1,000 |
| Process tree depth | 10 | 100 |

---

## 29. Future Improvements

### Short Term (6 months)
- eBPF-based process monitoring
- Predictive PID exhaustion prevention
- Process checkpoint/restore
- Enhanced zombie visualization

### Medium Term (12 months)
- Cross-node process migration
- Process dependency analyzer
- Automatic resource optimization
- Process behavior anomaly detection

### Long Term (24 months)
- AI-driven process optimization
- Deterministic process replay
- Namespace-level process grouping
- Global process view across clusters

---

## 30. Acceptance Criteria

### 30.1 Functional
- [ ] Process creation with proper PCB initialization
- [ ] All state transitions correct
- [ ] Parent-child hierarchy management
- [ ] Zombie prevention and reaping
- [ ] Orphan process reparenting
- [ ] Process groups and sessions
- [ ] Priority and niceness control
- [ ] Resource tracking and accounting
- [ ] Health monitoring with heartbeat
- [ ] Bulk operations (suspend, resume, kill)
- [ ] Process listing and introspection API

### 30.2 Non-Functional
- [ ] Supports 100K+ concurrent processes per instance
- [ ] Process creation < 10ms P50, < 50ms P99
- [ ] Process query < 5ms P50, < 20ms P99
- [ ] Zero zombie processes under normal operation
- [ ] Heartbeat check < 1ms overhead per process
- [ ] PCB memory < 1KB per process
- [ ] Process table scan < 100ms for 100K entries

### 30.3 Definition of Done
- [ ] All acceptance criteria met
- [ ] Integration tests with kernel process management
- [ ] Stress test with 100K+ processes
- [ ] Failure scenario tests (PM crash, zombie attack)
- [ ] Performance targets validated
- [ ] Monitoring dashboards implemented
- [ ] Alerting configured for all failure modes
- [ ] Documentation complete
