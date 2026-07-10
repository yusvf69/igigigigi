# AIOS Runtime Specification
## Document 03: Agent Execution Runtime

---

## Version History
| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2026-07-10 | AIOS Runtime Team | Initial specification |

---

## Table of Contents
1. Purpose and Mission
2. Runtime Architecture Overview
3. Sandbox Architecture
4. Runtime Isolation Levels
5. Runtime Lifecycle
6. Context Management
7. Execution Environment
8. Resource Limits and Quotas
9. Runtime Hook System
10. Runtime Security
11. Performance Optimization
12. Warm Pools and Cold Starts
13. Graceful Shutdown
14. Runtime Metrics and Telemetry
15. Runtime Configuration
16. Runtime Dependencies
17. Data Structures
18. Execution Flow
19. State Management
20. Validation
21. Error Recovery
22. Testing Strategy
23. Risk Analysis
24. Failure Scenarios
25. Operational Limits
26. Future Improvements
27. Acceptance Criteria

---

## 1. Purpose and Mission

### 1.1 Purpose
The AIOS Runtime Manager is responsible for provisioning, managing, and tearing down execution environments for AI agents. It provides secure isolation between agents, enforces resource limits, and optimizes execution performance through sandboxing technologies.

### 1.2 Mission
To provide a secure, isolated, and performant execution environment for every AI agent running on AIOS, with strong guarantees around resource fairness, security isolation, and operational efficiency.

### 1.3 Responsibilities
- Sandbox creation and management for agent execution
- Resource limit enforcement (CPU, memory, IO, network)
- Runtime isolation through cgroups, namespaces, seccomp
- Runtime lifecycle management (init, ready, execution, paused, terminated)
- Warm pool management for fast agent startup
- Cold start optimization
- Context management (identity, permissions, capabilities, session)
- Hook system (pre-exec, post-exec, on-error, on-timeout)
- Environment variable and filesystem management
- Network access control per sandbox

### 1.4 Non-Responsibilities
- Agent business logic execution
- Task scheduling and assignment (Task Manager)
- Inter-agent communication (Communication System)
- Workflow orchestration (Workflow Engine)
- Agent registration and discovery (Agent Manager)

---

## 2. Runtime Architecture Overview

### 2.1 Runtime Components
```
+------------------------------------------------------------------+
|                      RUNTIME MANAGER                              |
|  +------------------+  +------------------+  +---------------+   |
|  | Sandbox Manager  |  | Pool Manager     |  | Context Mgr   |   |
|  | (create/destroy) |  | (warm/cold)      |  | (identity)    |   |
|  +------------------+  +------------------+  +---------------+   |
|  +------------------+  +------------------+  +---------------+   |
|  | Resource Monitor |  | Hook Executor    |  | Security Mgr  |   |
|  | (limits/enforce) |  | (pre/post hooks) |  | (seccomp/cap) |   |
|  +------------------+  +------------------+  +---------------+   |
|  +------------------+  +------------------+  +---------------+   |
|  | Metrics Collector|  | Config Manager   |  | Health Check  |   |
|  +------------------+  +------------------+  +---------------+   |
+------------------------------------------------------------------+
        |                   |                    |
        v                   v                    v
+------------------------------------------------------------------+
|                    SANDBOX LAYER                                  |
|  +------------------+  +------------------+  +---------------+   |
|  | cgroups          |  | Namespaces       |  | seccomp-bpf   |   |
|  | (cpu, mem, io)   |  | (pid,net,uts,ipc)|  | (syscall fltr)|   |
|  +------------------+  +------------------+  +---------------+   |
|  +------------------+  +------------------+  +---------------+   |
|  | Capabilities     |  | Filesystem       |  | Network       |   |
|  | (dropped)        |  | (bind mounts)    |  | (veth pairs)  |   |
|  +------------------+  +------------------+  +---------------+   |
+------------------------------------------------------------------+
```

### 2.2 Runtime Stack
```
+------------------------------------------------------------------+
|  Agent Process (user code)                                       |
+------------------------------------------------------------------+
|  Agent Runtime Library (SDK)                                     |
+------------------------------------------------------------------+
|  Language Runtime (Python/Node/Go/Rust)                          |
+------------------------------------------------------------------+
|  AIOS Runtime Shim (sandbox interface)                            |
+------------------------------------------------------------------+
|  Operating System (minimal)                                       |
+------------------------------------------------------------------+
|  Containers / cgroups / namespaces / seccomp                     |
+------------------------------------------------------------------+
|  AIOS Kernel                                                      |
+------------------------------------------------------------------+
```

---

## 3. Sandbox Architecture

### 3.1 Sandbox Technologies

| Technology | Purpose | Configuration |
|-----------|---------|--------------|
| cgroups v2 | Resource limits (CPU, memory, IO, PID, network) | Subtree controllers |
| Namespaces | Isolation (PID, network, mount, UTS, IPC, user, cgroup) | Clone flags |
| seccomp-bpf | Syscall filtering | BPF program |
| capabilities | Privilege dropping | Capability sets |
| bind mounts | Filesystem access control | Read-only/private |
| veth pairs | Network isolation | Virtual Ethernet |

### 3.2 Sandbox Structure
```yaml
sandbox:
  id: string (UUID v7)
  agent_id: string
  status: [CREATING, ACTIVE, PAUSED, TERMINATED, FAILED]
  
  cgroups:
    cpu:
      cpuset_cpus: string
      cpu_max: string (quota period)
      cpu_weight: u64
    memory:
      memory_max: u64 (bytes)
      memory_swap_max: u64
      memory_high: u64 (soft limit)
    io:
      io_max: string (read/write limits)
    pids:
      pids_max: u32
  
  namespaces:
    pid: bool
    net: bool
    mnt: bool
    uts: bool
    ipc: bool
    user: bool
    cgroup: bool
  
  seccomp:
    profile: string (default/custom)
    allow_syscalls: [u32]
    deny_syscalls: [u32]
    default_action: string (allow/deny/kill)
  
  capabilities:
    bounding: [string]
    effective: [string]
    inheritable: [string]
    permitted: [string]
    ambient: [string]
  
  filesystem:
    mounts:
      - source: string
        target: string
        type: string (bind/tmpfs/proc/sys)
        options: [string]
    rootfs: string (path to root filesystem)
    writable: bool
  
  network:
    interfaces:
      - name: string
        type: veth
        mtu: u32
    firewall:
      ingress_rules: [string]
      egress_rules: [string]
  
  environment:
    variables: map<string, string>
    secrets: [string] (references to Secret Manager)
```

---

## 4. Runtime Isolation Levels

### 4.1 Isolation Profiles
| Profile | Namespaces | cgroups | seccomp | Use Case |
|---------|-----------|---------|---------|----------|
| NONE | None | None | None | Trusted system agents |
| LIGHT | PID, UTS | CPU, memory | Default deny | Monitoring agents |
| STANDARD | PID, NET, UTS, IPC | All | Custom allow | Most agents |
| STRICT | All | All with low limits | Minimal allow | Untrusted agents |
| AIR_GAP | All + no network | All + minimal | Minimal allow | Security-critical |

### 4.2 Isolation Level Selection
An agent's isolation level is determined by:
1. Agent trust score (computed from reputation, audit history)
2. Agent type (coordination agents get less isolation)
3. Tenant security requirements
4. Data sensitivity of the task
5. Compliance requirements (PCI, HIPAA, SOC2)

---

## 5. Runtime Lifecycle

### 5.1 Lifecycle States
```
INIT -> READY -> EXECUTING -> PAUSED -> EXECUTING (cycle)
EXECUTING -> COMPLETED
EXECUTING/READY -> TERMINATED
INIT -> FAILED (error during setup)
```

### 5.2 State Definitions
| State | Description | Actions |
|-------|-------------|---------|
| INIT | Sandbox being created | Allocate resources, setup cgroups/namespaces |
| READY | Sandbox ready for agent | Waiting for agent process |
| EXECUTING | Agent running | Monitor resources, enforce limits |
| PAUSED | Agent suspended (SIGSTOP) | Freeze cgroup, save state |
| COMPLETED | Agent finished normally | Collect results, cleanup |
| FAILED | Runtime error occurred | Log error, cleanup |
| TERMINATED | Force killed | Immediate cleanup |

### 5.3 State Transitions
```python
transitions = {
    'INIT': ['READY', 'FAILED'],
    'READY': ['EXECUTING', 'TERMINATED'],
    'EXECUTING': ['PAUSED', 'COMPLETED', 'FAILED', 'TERMINATED'],
    'PAUSED': ['EXECUTING', 'TERMINATED'],
    'COMPLETED': [],  # Terminal state
    'FAILED': [],     # Terminal state
    'TERMINATED': [], # Terminal state
}
```

---

## 6. Context Management

### 6.1 Context Structure
```yaml
runtime_context:
  identity:
    agent_id: string
    agent_type: string
    tenant_id: string
    user_id: string (optional)
    service_account: string
  
  authentication:
    token: string (JWT, short-lived)
    capabilities: [string]
    permissions: [string]
  
  session:
    session_id: string
    created_at: u64
    expires_at: u64
    metadata: map<string, string>
  
  task:
    task_id: string
    workflow_id: string (optional)
    workflow_step: string (optional)
    priority: u8
    deadline: u64
  
  environment:
    variables: map<string, string>
    filesystem:
      temp_dir: string
      data_dir: string
      scratch_dir: string
    network:
      allowed_domains: [string]
      allowed_ips: [string]
      rate_limits:
        requests_per_second: u32
        bandwidth_per_second: u64
```

### 6.2 Context Propagation
```
External Request -> API Gateway (add JWT)
    |
    v
Task Manager (enrich context with task info)
    |
    v
Agent Manager (add agent identity, capabilities)
    |
    v
Runtime Manager (create sandbox with context)
    |
    v
Agent Process (received context via environment/IPC)
```

---

## 7. Execution Environment

### 7.1 Environment Variables
```bash
# Always set
AIOS_AGENT_ID=<agent_id>
AIOS_AGENT_TYPE=<agent_type>
AIOS_TENANT_ID=<tenant_id>
AIOS_TASK_ID=<task_id>
AIOS_SESSION_ID=<session_id>
AIOS_SANDBOX_ID=<sandbox_id>
AIOS_RUNTIME_ID=<runtime_id>

# Conditional
AIOS_WORKFLOW_ID=<workflow_id>
AIOS_WORKFLOW_STEP=<step_id>
AIOS_DEADLINE=<deadline_epoch>
AIOS_MAX_CPU=<cpu_limit>
AIOS_MAX_MEMORY=<memory_limit>
AIOS_MAX_DURATION=<duration_limit>
AIOS_TEMP_DIR=/tmp/aios/<sandbox_id>
AIOS_DATA_DIR=/data/aios/<sandbox_id>

# Tracing
AIOS_TRACE_ID=<trace_id>
AIOS_SPAN_ID=<span_id>
OTEL_EXPORTER_OTLP_ENDPOINT=http://otel-collector:4318
OTEL_SERVICE_NAME=<agent_id>
```

### 7.2 Filesystem Layout
```
/ (read-only, minimal rootfs)
  /bin/         - Essential binaries
  /lib/         - Shared libraries
  /usr/         - User utilities
  
/tmp/           - Temp space (tmpfs, size limited)
  /aios/        - AIOS temp directory
    /<sandbox>/ - Per-sandbox temp
  
/var/           - Variable data
  /aios/        - AIOS data directory
    /<sandbox>/ - Per-sandbox data (writable)
  
/aios/          - AIOS interface
  /config/      - Runtime configuration
  /secrets/     - Mounted secrets (read-only)
  /ipc/         - IPC socket directory
```

### 7.3 Network Access Control
```yaml
network_rules:
  default_policy: deny         # Default deny egress
  
  allow_rules:
    - domains: [api.aios.io, *.aios.internal]
      protocols: [tcp, tls]
      ports: [443, 8443]
      rate_limit: 1000/s
    
    - cidrs: [10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16]
      protocols: [tcp]
      ports: [8080, 9090, 4318]
      purpose: service_mesh
    
    - cidrs: [169.254.169.254]       # Metadata service
      protocols: [http]
      ports: [80]
      purpose: cloud_metadata
  
  deny_rules:
    - cidrs: [0.0.0.0/0]             # Block all other traffic
      reason: default_deny
  
  dns:
    allowed_domains: [*.aios.io, *.aios.internal]
    cache_ttl: 60s
    block_porn: true
    block_malware: true
```

---

## 8. Resource Limits and Quotas

### 8.1 CPU Management
```yaml
cpu_limits:
  quota_us: 100000             # CPU time per period (100ms)
  period_us: 100000            # Period (100ms) = 1 CPU core
  weight: 100                  # CFS weight (relative share)
  cpuset_cpus: "0-3"           # CPU affinity
  max: "4"                     # Max CPU cores
  rt_runtime_us: 0             # Real-time runtime (0 = disabled)
  rt_period_us: 1000000        # Real-time period
```

### 8.2 Memory Management
```yaml
memory_limits:
  max: 4294967296              # 4GB hard limit
  high: 3221225472             # 3GB soft limit (throttle at)
  swap_max: 1073741824         # 1GB swap max
  oom_control: true            # Enable OOM killer
  oom_score_adj: 500           # OOM score adjustment
  kmem_limit: 536870912        # 512MB kernel memory
  num_pages: 1048576           # Max page count
```

### 8.3 I/O Management
```yaml
io_limits:
  classes:
    - name: readonly
      rbps_max: 104857600      # 100MB/s read
      wbps_max: 0              # No write
      riops_max: 10000         # 10K read IOPS
      wiops_max: 0             # No write IOPS
  
    - name: readwrite
      rbps_max: 524288000      # 500MB/s read
      wbps_max: 262144000      # 250MB/s write
      riops_max: 50000         # 50K read IOPS
      wiops_max: 25000         # 25K write IOPS
```

### 8.4 PID Management
```yaml
pid_limits:
  max: 1024                    # Max processes/threads
  event: true                  # Notify on PIDs approaching limit
```

### 8.5 Quota Enforcement
```yaml
quotas:
  per_tenant:
    total_cpu: 1000            # Total CPU cores across all agents
    total_memory: 4096         # 4TB total memory
    total_agents: 500          # Max concurrent agents
    total_storage: 1048576     # 1PB total storage
  
  per_agent:
    max_cpu: 64                # Max CPU cores per agent
    max_memory: 549755813888   # 512GB per agent
    max_duration: 86400        # 24 hours max execution
    max_egress: 10737418240    # 10GB egress per execution
```

---

## 9. Runtime Hook System

### 9.1 Hook Types
| Hook | Timing | Purpose | Cancel Execution |
|------|--------|---------|-----------------|
| pre_exec | Before agent starts | Setup, validation, injection | Yes (return error) |
| post_exec | After agent completes | Result processing, cleanup | N/A |
| on_error | On runtime error | Error handling, logging | N/A |
| on_timeout | On execution timeout | Graceful shutdown | N/A |
| on_pause | On pause request | Save state | N/A |
| on_resume | On resume request | Restore state | N/A |
| on_checkpoint | Periodic | State checkpointing | N/A |
| on_heartbeat | Every heartbeat | Health check | Yes (force stop) |

### 9.2 Hook Definitions
```yaml
hooks:
  pre_exec:
    - name: validate_environment
      timeout_ms: 5000
      retry: 2
      on_failure: abort
    
    - name: setup_network_policy
      timeout_ms: 3000
      retry: 0
      on_failure: abort
    
    - name: mount_secrets
      timeout_ms: 10000
      retry: 3
      on_failure: abort
  
  post_exec:
    - name: collect_artifacts
      timeout_ms: 30000
      retry: 3
      on_failure: warn
    
    - name: cleanup_temp_files
      timeout_ms: 15000
      retry: 0
      on_failure: warn
    
    - name: emit_completion_metrics
      timeout_ms: 5000
      retry: 0
      on_failure: log
  
  on_error:
    - name: capture_crash_dump
      timeout_ms: 30000
      retry: 0
      on_failure: log
    
    - name: notify_workflow_engine
      timeout_ms: 5000
      retry: 3
      on_failure: log
  
  on_timeout:
    - name: save_partial_results
      timeout_ms: 15000
      retry: 0
      on_failure: log
    
    - name: force_terminate
      timeout_ms: 10000
      retry: 0
      on_failure: escalate
```

### 9.3 Hook Execution Engine
```rust
struct HookEngine {
    hooks: HashMap<HookPoint, Vec<Hook>>,
    config: HookConfig,
    stats: HookStats,
}

impl HookEngine {
    fn execute(&self, point: HookPoint, ctx: &Context) -> Result<(), Error> {
        let hooks = self.hooks.get(&point).ok_or(Error::NoHooks)?;
        for hook in hooks {
            let result = self.run_hook(hook, ctx);
            match result {
                Ok(_) => continue,
                Err(e) if hook.on_failure == Abort => return Err(e),
                Err(e) => warn!("Hook {} failed: {}", hook.name, e),
            }
        }
        Ok(())
    }
    
    fn run_hook(&self, hook: &Hook, ctx: &Context) -> Result<(), Error> {
        let deadline = Instant::now() + Duration::from_millis(hook.timeout_ms);
        let mut last_error = None;
        
        for attempt in 0..=hook.retry {
            match self.execute_with_timeout(hook, ctx, deadline) {
                Ok(_) => return Ok(()),
                Err(e) => {
                    last_error = Some(e);
                    if attempt < hook.retry {
                        sleep(backoff(attempt));
                    }
                }
            }
        }
        Err(last_error.unwrap())
    }
}
```

---

## 10. Runtime Security

### 10.1 Seccomp Profiles
```json
{
  "defaultAction": "SCMP_ACT_ERRNO",
  "architectures": ["SCMP_ARCH_X86_64"],
  "syscalls": [
    {
      "names": ["accept", "bind", "clock_gettime", "close", "connect",
        "dup", "dup2", "epoll_create1", "epoll_ctl", "epoll_wait",
        "exit", "exit_group", "fstat", "ftruncate", "futex",
        "getdents64", "getpeername", "getsockname", "getsockopt",
        "listen", "lseek", "mkdir", "mmap", "mprotect", "munmap",
        "nanosleep", "newfstatat", "openat", "pipe2", "poll",
        "ppoll", "pread64", "pwrite64", "read", "readlinkat",
        "recvfrom", "recvmsg", "rt_sigaction", "rt_sigprocmask",
        "sendmmsg", "sendmsg", "sendto", "setsockopt", "shutdown",
        "sigaltstack", "socket", "statx", "tgkill", "write",
        "writev"],
      "action": "SCMP_ACT_ALLOW"
    },
    {
      "names": ["clone", "fork", "vfork"],
      "action": "SCMP_ACT_ALLOW",
      "args": [
        {
          "index": 0,
          "value": 2114060288,
          "valueTwo": 0,
          "op": "SCMP_CMP_MASKED_EQ"
        }
      ]
    }
  ]
}
```

### 10.2 Capability Dropping
```bash
# Dropped capabilities (agents run without these):
# CAP_SYS_ADMIN: System administration
# CAP_SYS_MODULE: Module loading
# CAP_SYS_RAWIO: Raw I/O
# CAP_SYS_BOOT: Reboot
# CAP_SYS_PTRACE: Process tracing
# CAP_SYSLOG: Kernel log access
# CAP_NET_ADMIN: Network administration
# CAP_MKNOD: Device node creation
# CAP_SETUID: UID changes
# CAP_SETGID: GID changes
# CAP_LINUX_IMMUTABLE: File attribute changes

# Retained capabilities (minimum required):
# CAP_NET_BIND_SERVICE: Bind to ports > 1024
# CAP_AUDIT_WRITE: Write audit log
# CAP_KILL: Send signals
# CAP_CHOWN: Change file ownership (if needed)
# CAP_DAC_OVERRIDE: Bypass file permission checks (if needed)
```

### 10.3 Filesystem Security
```yaml
filesystem_security:
  rootfs: read-only overlay
  mounts:
    /proc: read-only, hide PID 1
    /sys: read-only, limited visibility
    /tmp: tmpfs, noexec, nosuid
    /var/aios: writable, size-limited
  symlink: protected (symlink restrictions)
  hardlink: protected (ownership check)
  ptrace: disabled
  dmesg: restricted
  kallsyms: restricted
```

---

## 11. Performance Optimization

### 11.1 JIT Compilation Support
- Detect JIT-friendly agent workloads
- Pre-JIT compile agent bytecode in warm pool
- Cache compiled code across executions
- Profile-guided optimization for repeated agents

### 11.2 Context Caching
```yaml
context_caching:
  cache_type: LRU
  max_entries: 10000
  ttl_seconds: 300
  
  cached_items:
    - agent_binary: true
    - dependencies: true
    - compiled_code: true
    - model_weights: true (with version pinning)
    - environment_setup: true
    
  warm_pool:
    preloaded_contexts:
      - agent_type: cognitive
        count: 100
      - agent_type: generation
        count: 50
      - agent_type: retrieval
        count: 200
    refresh_interval: 60
    refresh_strategy: rolling
```

### 11.3 Memory Optimization
- Page cache sharing between same-type agents (KSM)
- Huge pages for memory-intensive agents
- Memory reclaim thresholds per sandbox
- Swap priority: agents with lower priority swap first
- OOM protection for critical agents (oom_score_adj = -500)

---

## 12. Warm Pools and Cold Starts

### 12.1 Warm Pool Architecture
```yaml
warm_pool:
  enabled: true
  size: 500         # Total warm sandboxes
  types:
    cognitive:
      min: 50
      max: 200
      preload_agent: true
      preload_timeout: 30s
    
    generation:
      min: 20
      max: 100
      preload_agent: true
      preload_timeout: 60s
    
    retrieval:
      min: 100
      max: 300
      preload_agent: false
      preload_timeout: 10s
    
    classification:
      min: 50
      max: 200
      preload_agent: true
      preload_timeout: 20s
  
  eviction_policy: LRU
  eviction_idle_timeout: 300        # 5 min idle = evict
  refresh_interval: 30              # Check every 30s
```

### 12.2 Cold Start Sequence
```python
cold_start_steps = {
    'total_target': '< 2s for standard, < 5s for GPU',
    'steps': [
        ('allocate_sandbox', 100),        # 100ms
        ('create_cgroups', 50),           # 50ms
        ('setup_namespaces', 30),         # 30ms
        ('mount_filesystem', 200),        # 200ms
        ('configure_network', 100),       # 100ms
        ('load_seccomp_profile', 10),     # 10ms
        ('drop_capabilities', 5),         # 5ms
        ('setup_environment', 50),        # 50ms
        ('load_agent_binary', 500),       # 500ms
        ('initialize_agent_runtime', 500),# 500ms
        ('load_dependencies', 300),       # 300ms
        ('connect_services', 100),        # 100ms
        ('ready_signal', 5),              # 5ms
    ],
    'optimizations': [
        'Parallel setup (cgroups + namespaces + mounts)',
        'Pre-loaded seccomp profiles',
        'Copy-on-write filesystem (overlayfs)',
        'Pre-heated network namespaces',
        'Cached agent binaries (page cache)',
    ]
}
```

### 12.3 Cold Start Optimization Strategies
1. **Sandbox snapshotting**: Save and restore sandbox state
2. **Overlay filesystem**: Share base rootfs across sandboxes
3. **Pre-warmed network**: Pre-allocate veth pairs and IPs
4. **Agent binary caching**: Keep binaries in page cache
5. **Lazy loading**: Defer dependency loading until needed
6. **Predictive warm-up**: ML-based prediction of agent demand

---

## 13. Graceful Shutdown

### 13.1 Shutdown Sequence
```python
graceful_shutdown = {
    'timeout': '30s',
    'steps': [
        ('SIGTERM', '10s'),              # Signal agent to stop
        ('wait_for_exit', '5s'),          # Wait for clean exit
        ('run_on_shutdown_hooks', '10s'), # Execute hooks
        ('save_checkpoint', '3s'),        # Save state if possible
        ('flush_logs', '2s'),             # Flush buffered logs
        ('close_ipc_channels', '1s'),     # Close IPC connections
        ('unmount_filesystems', '1s'),    # Unmount sandbox FS
        ('release_cgroups', '0.5s'),      # Release control groups
        ('cleanup_temps', '1s'),          # Remove temp files
    ],
    'force_timeout': {
        'trigger': '15s after SIGTERM',
        'action': 'SIGKILL',
    }
}
```

### 13.2 Signal Handling
```yaml
signal_handling:
  SIGTERM:
    action: graceful_shutdown
    timeout: 10s
    escalate: SIGKILL
  
  SIGINT:
    action: graceful_shutdown
    timeout: 5s
    escalate: SIGKILL
  
  SIGQUIT:
    action: crash_dump + shutdown
    timeout: 5s
  
  SIGUSR1:
    action: save_checkpoint
    description: "Trigger checkpoint save"
  
  SIGUSR2:
    action: dump_stack_trace
    description: "Debug signal"
  
  SIGSTOP:
    action: freeze_process
    description: "Pause agent"
  
  SIGCONT:
    action: unfreeze_process
    description: "Resume agent"
```

---

## 14. Runtime Metrics and Telemetry

### 14.1 Runtime Metrics
```yaml
metrics:
  sandbox:
    - runtime_sandbox_created_total
    - runtime_sandbox_destroyed_total
    - runtime_sandbox_active
    - runtime_sandbox_creating
    - runtime_sandbox_failed
    - runtime_sandbox_create_duration_ms
    - runtime_sandbox_destroy_duration_ms
  
  warm_pool:
    - runtime_warm_pool_size
    - runtime_warm_pool_available
    - runtime_warm_pool_in_use
    - runtime_warm_pool_hit_total
    - runtime_warm_pool_miss_total
    - runtime_warm_pool_eviction_total
  
  cold_start:
    - runtime_cold_start_total
    - runtime_cold_start_duration_ms
    - runtime_cold_start_p50_ms
    - runtime_cold_start_p99_ms
  
  resource_usage:
    - runtime_cpu_usage_percent
    - runtime_memory_usage_bytes
    - runtime_memory_swap_bytes
    - runtime_io_read_bytes_total
    - runtime_io_write_bytes_total
    - runtime_network_rx_bytes_total
    - runtime_network_tx_bytes_total
  
  lifecycle:
    - runtime_execution_duration_ms
    - runtime_lifecycle_transition_total
    - runtime_graceful_shutdown_total
    - runtime_force_terminate_total
    - runtime_pause_total
    - runtime_resume_total
```

### 14.2 Runtime Logging
```yaml
logging:
  events:
    - runtime.sandbox.created
    - runtime.sandbox.destroyed
    - runtime.sandbox.cold_start
    - runtime.sandbox.warm_pool_hit
    - runtime.sandbox.resource_exceeded
    - runtime.sandbox.oom_killed
    - runtime.sandbox.timeout
    - runtime.sandbox.error
    - runtime.lifecycle.started
    - runtime.lifecycle.completed
    - runtime.lifecycle.paused
    - runtime.lifecycle.resumed
    - runtime.lifecycle.terminated
  
  agent_logs:
    collection: stdout/stderr
    format: json
    fields: [timestamp, level, agent_id, task_id, message]
    rate_limit: 1000/s per agent
    max_size: 100MB per execution
```

---

## 15. Runtime Configuration

### 15.1 Global Configuration
```yaml
runtime:
  global:
    default_isolation: STANDARD
    max_sandboxes_per_node: 500
    max_agents_per_node: 250
    
  sandbox_defaults:
    cpu_limit: 2
    memory_limit: 4294967296     # 4GB
    duration_limit: 1800         # 30min
    network_egress: true
    
  warm_pool:
    enabled: true
    default_size: 100
    refresh_interval: 30
    
  security:
    seccomp_default: default.json
    cap_drop_all: true
    rootfs_readonly: true
    network_default_deny: true
    
  hooks:
    pre_exec_timeout: 30000
    post_exec_timeout: 30000
    max_hooks_per_point: 10
```

---

## 16. Runtime Dependencies

| Component | Dependencies | Protocol |
|-----------|-------------|----------|
| Runtime Manager | Process Manager, Agent Manager | gRPC |
| Sandbox Manager | cgroups, namespaces, seccomp | Kernel syscalls |
| Pool Manager | Config Store, Metrics | Internal API |
| Context Manager | Auth Service, Secret Manager | gRPC |
| Resource Monitor | Kernel (sysfs, cgroupfs) | File system |
| Hook Executor | Config Store | gRPC |
| Security Manager | Policy Engine | gRPC |

---

## 17. Data Structures

### 17.1 Sandbox Structure
```rust
struct Sandbox {
    id: String,
    agent_id: String,
    status: SandboxStatus,
    isolation_level: IsolationLevel,
    
    // Resource limits
    cpu_quota: u64,
    cpu_period: u64,
    memory_max: u64,
    memory_high: u64,
    io_rbps_max: u64,
    io_wbps_max: u64,
    pids_max: u32,
    
    // Namespace FDs
    pid_ns_fd: i32,
    net_ns_fd: i32,
    mnt_ns_fd: i32,
    uts_ns_fd: i32,
    ipc_ns_fd: i32,
    user_ns_fd: i32,
    cgroup_ns_fd: i32,
    
    // Cgroup paths
    cgroup_path: String,
    
    // Process info
    pid: u32,
    child_pid: u32,
    
    // Metrics
    created_at: u64,
    started_at: Option<u64>,
    completed_at: Option<u64>,
    cpu_usage: f64,
    memory_usage: u64,
    
    // Context
    context: RuntimeContext,
    
    // Hooks
    hooks: Vec<Hook>,
}
```

### 17.2 Pool Structure
```rust
struct WarmPool {
    pool_type: AgentType,
    max_size: u32,
    min_size: u32,
    current_size: u32,
    
    // Pre-warmed sandboxes
    available: VecDeque<Sandbox>,
    in_use: HashMap<String, Sandbox>,
    
    // Configuration
    preload_agent: bool,
    preload_timeout: Duration,
    eviction_idle_timeout: Duration,
    
    // Statistics
    hits: u64,
    misses: u64,
    evictions: u64,
    created: u64,
}
```

---

## 18. Execution Flow

### 18.1 Agent Start Flow
```
Request to start agent
    |
    v
Runtime Manager.receive_request()
    |
    v
Runtime Manager.select_sandbox()
    |
    +---> Check warm pool for available sandbox
    |        |
    |        +---> Hit: Mark in_use, return sandbox
    |        |
    |        +---> Miss: Create new sandbox (cold start)
    |                |
    |                v
    |           Sandbox Manager.create()
    |                |
    |                v
    |           +-> Create cgroups
    |           +-> Create namespaces
    |           +-> Setup filesystem (overlay mount)
    |           +-> Configure network (veth pair)
    |           +-> Load seccomp profile
    |           +-> Drop capabilities
    |           +-> Set environment variables
    |           +-> Mount secrets
    |                |
    |                v
    |           Sandbox (READY state)
    |
    v
Runtime Manager.start_agent(sandbox, context)
    |
    v
Hook Engine.execute(pre_exec_hooks)
    |
    v
Process Manager.create_in_sandbox(sandbox, agent_path, args)
    |
    v
Sandbox state -> EXECUTING
    |
    v
Resource Monitor.start_tracking(sandbox)
    |
    v
Return (agent is running)
```

### 18.2 Agent Completion Flow
```
Agent process exits
    |
    v
Runtime Manager.handle_exit(pid, exit_code)
    |
    v
Hook Engine.execute(post_exec_hooks)
    |
    v
Collect exit code and resource usage
    |
    v
Sandbox state -> COMPLETED or FAILED
    |
    v
Resource Monitor.stop_tracking(sandbox)
    |
    v
Collect and emit metrics
    |
    v
Event System.publish(runtime.agent.completed)
    |
    v
Return sandbox to warm pool (if eligible)
    |
    OR
    |
    v
Sandbox Manager.destroy(sandbox) -> cleanup
```

---

## 19. State Management

| State | Storage | Location | Persistence |
|-------|---------|----------|-------------|
| Sandbox state | In-memory + database | Runtime Manager | Recoverable |
| Resource usage | In-memory (streaming) | Runtime Manager | Ephemeral |
| Warm pool state | In-memory | Runtime Manager | Ephemeral |
| Agent context | In-memory (passed) | Runtime + Agent | Ephemeral |
| Checkpoints | S3/GCS | External | Persistent |

---

## 20. Validation

### 20.1 Sandbox Request Validation
- Agent ID exists and is active
- Resource requests within tenant quotas
- Isolation level is valid for agent trust score
- Required capabilities are available
- Network rules are valid
- Hook configurations are valid

### 20.2 Runtime Environment Validation
- Root filesystem integrity check
- Seccomp profile validity
- Namespace creation success
- Cgroup creation success
- Network interface creation success
- Secret mount success
- Environment variable sanitization

---

## 21. Error Recovery

| Error | Detection | Recovery |
|-------|-----------|----------|
| Sandbox creation failure | Error return | Retry with backoff, fallback to cold start |
| Resource exhaustion | Kernel notification | OOM kill non-critical agents, notify auto-scaler |
| Cgroup failure | Write error | Escalate to node agent |
| Network setup failure | Timeout | Retry with new veth pair |
| Hook failure | Timeout/error | Log + continue or abort (per hook config) |
| Agent unresponsive | Heartbeat miss | Watchdog -> SIGTERM -> SIGKILL |
| Filesystem corruption | I/O error | Destroy sandbox, notify admin |

---

## 22. Testing Strategy

| Test Type | Scope | Tooling |
|-----------|-------|---------|
| Unit | Individual runtime components | Go test |
| Integration | Runtime + Process Manager | Integration test |
| Sandbox | cgroups/namespaces/seccomp validation | Custom tests |
| Isolation | Security isolation verification | Pen testing tools |
| Performance | Cold start, warm pool hit rate | k6, custom bench |
| Stress | Max sandboxes per node | stress-ng |
| Chaos | Node failures, resource limits | Chaos engineering |
| Fuzzing | Configuration parsing | go-fuzz |

---

## 23. Risk Analysis

| Risk | Probability | Impact | Mitigation |
|------|------------|--------|------------|
| Sandbox escape | Low | Critical | Defense in depth, seccomp, capabilities |
| Resource starvation | Medium | High | Hard limits, OOM protection, QoS |
| Warm pool exhaustion | Medium | Medium | Auto-scaling, predictive warm-up |
| Cold start latency | Medium | Medium | Pre-warming, binary caching |
| Hook timeout | Low | Medium | Configurable timeout, retry |
| Secret leak | Low | Critical | Ephemeral mounts, memory locking |
| Namespace leak | Low | Medium | Reference counting, cleanup verification |
| Cgroup leak | Low | Medium | Hierarchical cleanup |

---

## 24. Failure Scenarios

### 24.1 Sandbox Creation Failure
- **Symptoms**: Sandbox creation error, agent not starting
- **Detection**: Error return from Sandbox Manager
- **Action**: Retry 3 times with exponential backoff (100ms, 200ms, 400ms)
- **Fallback**: Create in different node or request new node
- **Recovery**: Log error, create incident, notify agent manager

### 24.2 Resource Limit Exceeded
- **Symptoms**: Agent OOM killed, CPU throttled, I/O blocked
- **Detection**: cgroup event notifications, kernel OOM message
- **Action**: Log resource exceeded event
- **Mitigation**: Request more resources, scale down processing
- **Recovery**: On retry, allocate more resources if available

### 24.3 Warm Pool Exhaustion
- **Symptoms**: All warm sandboxes in use, cold starts increase
- **Detection**: Pool miss rate exceeds threshold (10%)
- **Action**: Increase warm pool size, trigger batch cold start
- **Recovery**: Predictive scaling based on queue depth

---

## 25. Operational Limits

| Resource | Hard Limit | Soft Limit | Notes |
|----------|-----------|-----------|-------|
| Sandboxes per node | 500 | 250 default | CPU/memory dependent |
| Sandbox creation time | 5s | 1s | Excludes agent init |
| Cold start time | 10s | 2s | With optimizations |
| Warm pool hit ratio | - | > 90% | Target |
| Memory per sandbox | 512GB | 16GB | Configurable |
| CPU per sandbox | 64 cores | 4 cores | Configurable |
| Execution duration | 7 days | 30min | Configurable per tenant |
| Hooks per sandbox | 20 | 5 | Per hook point |
| Concurrent sandboxes | 10000 | 1000 | Per cluster |

---

## 26. Future Improvements

### Short Term (6 months)
- Container-native runtime (OCI-compatible)
- Checkpoint/restore (CRIU integration)
- GPU isolation and sharing
- Improved warm pool prediction
- Sidecar injection for agent services

### Medium Term (12 months)
- WebAssembly runtime support
- eBPF-based security monitoring
- VMM-based isolation (Firecracker/KVM)
- Cross-node sandbox migration
- Auto-tuning of resource limits

### Long Term (24 months)
- Confidential computing (TEE integration)
- Heterogeneous compute support (FPGA, TPU)
- Deterministic replay for debugging
- AI-optimized scheduler for agent workloads
- Self-healing runtime environments

---

## 27. Acceptance Criteria

### 27.1 Functional
- [ ] Sandbox creation with cgroups/namespaces/seccomp
- [ ] All isolation levels implemented (NONE through AIR_GAP)
- [ ] Runtime lifecycle management complete
- [ ] Resource limits enforced correctly
- [ ] Hook system functional (pre, post, on_error, on_timeout)
- [ ] Warm pool management working
- [ ] Graceful shutdown sequence implemented
- [ ] Context propagation complete

### 27.2 Non-Functional
- [ ] Cold start < 2s (standard), < 5s (GPU)
- [ ] Warm pool hit rate > 90%
- [ ] Memory overhead per sandbox < 50MB
- [ ] CPU overhead per sandbox < 1%
- [ ] Support 250+ concurrent agents per node
- [ ] No sandbox escape vulnerabilities in audit
- [ ] Hook timeout configurable and enforced
- [ ] Resource limit enforcement within 1% of target
