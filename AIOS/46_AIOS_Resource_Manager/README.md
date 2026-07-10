# AIOS Resource Manager — Engineering Specification v1.0

## Document Control
- **Document ID:** AIOS-SPEC-RESOURCE-001
- **Version:** 1.0
- **Status:** APPROVED
- **Last Updated:** 2026-07-10
- **Owner:** AIOS Platform Engineering — Resource Management Team
- **Classification:** CONFIDENTIAL

---

## 1. Purpose

The AIOS Resource Manager governs the allocation, tracking, accounting, and optimization of all computational resources across the AIOS platform, ensuring fair distribution, preventing resource abuse, and enabling cost-effective operations.

## 2. Mission

To provide transparent, fair, and efficient resource management that ensures every AIOS agent, team, and department receives the resources they need to operate effectively, while preventing any single consumer from degrading platform stability and enabling accurate cost allocation.

## 3. Responsibilities

- Resource types: CPU (cores, shares), Memory (RAM, swap), GPU (model-specific, VRAM), Storage (IOPS, capacity), Network (bandwidth, connections).
- Resource allocation: per agent, per team, per department.
- Resource quotas: hard limit (cannot exceed), soft limit (exceed for burst), burst allowance (short-term peak).
- Resource reservations: guaranteed resources for critical agents.
- Resource limits: prevent runaway consumption via cgroups, pod limits, and network throttles.
- Resource tracking: real-time usage, historical trends.
- Resource accounting: chargeback per agent/department.
- Oversubscription: safe ratio 1.2x-1.5x (memory) and 2x-4x (compute).
- Pressure handling: OOM killer for memory, CPU throttling for compute.
- Resource negotiation: agents request resources, scheduler allocates within quotas.
- Defragmentation: periodic compaction of fragmented resource pools.
- USE method metrics: utilization, saturation, errors.

## 4. Non-Responsibilities

- Infrastructure provisioning (Cluster System)
- Scaling decisions (Scaling System)
- Application-level optimization (Performance Manager)
- FinOps billing (Finance team)

## 5. Architecture

### 5.1 Resource Hierarchy

```
Global Pool -> Regional Pool -> Cluster Pool -> Node Pool -> Pod/Agent
```

### 5.2 Components

```
Resource Manager
  Resource Allocator: quota enforcement, reservation
  Resource Monitor: usage tracking, pressure detection
  Resource Accountant: charge-back
  Resource Defragmenter: compaction
  Resource Negotiator: agent request handling
```

## 6. Internal Components

### Resource Allocator
Enforces quotas (hard, soft, burst). Handles reservations. Oversubscription ratios: CPU 4:1, memory 1.5:1, storage 2:1, GPU 1:1.

### Resource Monitor
Collects usage metrics via K8s Metrics Server, cgroups, GPU metrics. Tracks utilization, saturation, errors (USE).

### Resource Accountant
Computes chargeback: CPU-hr, GPU-hr, GB-hr, IOPS-hr, MBps-hr. Supports showback and chargeback models.

### Resource Defragmenter
Manages compaction of GPU memory, memory fragmentation reduction, CPU core affinity rebalancing.

## 7. External Components

Kubernetes, cgroups, K8s Metrics Server, Prometheus, NVIDIA GPU metrics, AIOS Scaling System.

## 8. Dependencies

Kubernetes 1.30+, cgroups v2, Prometheus 2.48+, NVIDIA GPU Operator, AIOS Kernel.

## 9. Data Structures

### ResourceQuota (Persistent)
```yaml
ResourceQuota:
  scope: {type: (agent|team|dept), id: string}
  limits:
    cpu: 4 cores
    memory: 16GB
    gpu: {type: "A100", count: 2}
    storage: {capacity: "500GB", iops: 10000}
    network: {bandwidth: "1Gbps", connections: 10000}
  reservations:
    cpu: 2 cores (guaranteed)
    memory: 8GB (guaranteed)
  burst:
    enabled: true
    cpu_overage: 50%
    memory_overage: 20%
```

## 10. Execution Flow

Agent registration: agent requests resources -> negotiation with quota -> allocation within limits -> reservation if critical -> burst allowed for peak -> tracking begins.

Pressure handling: memory pressure detected -> OOM killer for lowest priority -> CPU throttling applied -> notification sent -> escalation if persistent.

Defragmentation: fragmentation detected -> migration of workloads -> compaction -> verification. Duration: 10-30 min.

## 11-44. Additional Sections

**Key Limits:**
- Global CPU limit: 100000 cores
- Global memory limit: 256TB
- Per-agent CPU max: 32 cores
- Per-agent memory max: 128GB
- Per-team CPU max: 10000 cores
- Oversubscription ratios: CPU 4:1, Memory 1.5:1
- Max concurrent agents: 100K (current), 500K (year 2)
- Reservation pool: 20% of total capacity

**Acceptance Criteria:**
Agent cannot exceed hard quota. Critical agent gets guaranteed resources. Oversubscription safely managed (no OOM for non-violators). Chargeback accurate +/- 1%. Pressure handling prevents platform crash. Defragmentation reduces fragmentation by 50%.
