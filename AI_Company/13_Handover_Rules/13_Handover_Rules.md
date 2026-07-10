# 13. Handover Rules

> **Document Version:** 1.0
> **Last Updated:** 2026-07-09
> **Owner:** CE-AI, Executive Team
> **Applies To:** All 8,500 AI Agents, 130 Teams, 33 Departments, 7 Executives
> **Classification:** Enterprise Policy — Mandatory Compliance

---

## Table of Contents

1. [Handover Fundamentals](#1-handover-fundamentals)
2. [Handover Types](#2-handover-types)
3. [Handover Protocol Specification](#3-handover-protocol-specification)
4. [Handover Payload](#4-handover-payload)
5. [Quality Gates Before Handover](#5-quality-gates-before-handover)
6. [Receiving Agent Responsibilities](#6-receiving-agent-responsibilities)
7. [Handover in Specific Workflows](#7-handover-in-specific-workflows)
8. [Handover Failure Modes](#8-handover-failure-modes)
9. [Handover Monitoring and Metrics](#9-handover-monitoring-and-metrics)
10. [Shift Handover Procedures](#10-shift-handover-procedures)
11. [Emergency Handover](#11-emergency-handover)
12. [Handover Optimization](#12-handover-optimization)
13. [Appendices](#13-appendices)

---

## 1. Handover Fundamentals

### 1.1 Definition

A **handover** is the formal, structured transfer of task ownership, context, state, and accountability from one AI agent (the *sender* or *handing agent*) to another AI agent (the *receiver* or *receiving agent*). A handover is distinct from regular message passing in the following critical ways:

| Aspect | Handover | Regular Message Passing |
|--------|----------|------------------------|
| **Ownership transfer** | Full accountability moves from sender to receiver | No ownership change; sender retains accountability |
| **Context payload** | Complete task state, history, working memory, artifacts | Partial information; typically a single query/response |
| **Lifecycle** | Formal 5-phase lifecycle with validation and acknowledgment | Ad-hoc; no formal lifecycle |
| **Quality gates** | Mandatory pre-handover quality checks | No quality gates |
| **Audit record** | Full payload logged for 90 days with tracing ID | Optional, minimal logging |
| **Failure handling** | Retry logic with exponential backoff, escalation | Simple retry or drop |
| **SLA binding** | Subject to handover SLA (95% within 5s) | No specific SLA |
| **Protocol** | Standardized schema with typed fields | Free-form message |

**Rule 1.1.1:** Any transfer of task ownership that involves a different agent taking responsibility for completion MUST use the formal handover protocol. Simple coordination messages (e.g., "please review this output") that do not transfer ownership are NOT handovers and MUST use the regular messaging system.

**Rule 1.1.2:** An agent that receives a task via handover becomes the **sole accountable entity** for that task until it either completes the task or performs another handover.

### 1.2 Handover Triggers

A handover MUST be initiated when any of the following conditions are met:

#### 1.2.1 Task Completion
- **Trigger:** The current agent has completed all assigned work items within the task.
- **Action:** Hand over to the next agent in the workflow pipeline or back to the task orchestrator.
- **Threshold:** 100% of subtasks marked complete; all required artifacts created and validated.
- **Example:** An OCR Completion Agent finishes digitizing a scanned book and hands over to the Verification Agent.

#### 1.2.2 Shift Change
- **Trigger:** The agent's scheduled shift has ended, or the agent's remaining uptime is less than the estimated time-to-complete for the current task.
- **Action:** Hand over all active tasks to the incoming shift agent.
- **Threshold:** Remaining shift duration < 1.5× estimated task completion time.
- **Example:** Agent `OCR-047` at 23:55 has 8 minutes until shift end; its current book has ~30 pages remaining (~20 min work). It initiates handover to `OCR-048` on night shift.

#### 1.2.3 Expertise Requirement
- **Trigger:** The task requires a capability or knowledge domain not available in the current agent's skill set.
- **Action:** Hand over to a specialized agent with the required expertise.
- **Threshold:** Agent self-assessed confidence < 0.6 on required capability; or explicit capability not found in agent's manifest.
- **Example:** A General KG Agent encounters a complex Islamic jurisprudence (Fiqh) concept requiring multi-school comparison; it hands over to the Fiqh Specialist Agent.

#### 1.2.4 Capacity Overflow
- **Trigger:** The agent's current workload exceeds its maximum concurrent task capacity.
- **Action:** Hand over one or more tasks to an available peer agent.
- **Threshold:** Active task count > 85% of maximum capacity (e.g., agent max=10, threshold=8.5, trigger at ≥9).
- **Example:** Agent `VER-023` is handling 9 verification tasks simultaneously; a 10th arrives. It hands over its lowest-priority active task to `VER-024`.

#### 1.2.5 Failure Recovery
- **Trigger:** The agent has exhausted its maximum retry attempts for a sub-task or operation.
- **Action:** Hand over to a recovery agent or supervisor agent with a full error history.
- **Threshold:** Max 3 retries at agent level; 5 retries at operation level.
- **Example:** OCR Agent fails to parse a damaged page after 5 retries with different configurations; it hands over to a specialized Damaged Document Recovery Agent.

#### 1.2.6 Escalation Prerequisite
- **Trigger:** A decision or authorization is required that exceeds the agent's decision authority level.
- **Action:** Hand over to a superior (team lead, department head, executive).
- **Threshold:** Decision authority levels range from 1 (individual agent) to 7 (CE-AI). Handover required when required level exceeds agent's level.
- **Example:** An agent needs to approve a content licensing fee of $5,000; its authority limit is $1,000. It escalates via handover to Department Head (authority level 5, limit $10,000).

### 1.3 Handover Lifecycle

Every handover MUST follow a strict 5-phase lifecycle:

```
┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────────┐     ┌──────────┐
│ 1.       │ ──→ │ 2.       │ ──→ │ 3.       │ ──→ │ 4.           │ ──→ │ 5.       │
│INITIATION│     │VALIDATION│     │TRANSFER  │     │ACKNOWLEDGMENT│     │COMPLETION│
└──────────┘     └──────────┘     └──────────┘     └──────────────┘     └──────────┘
```

#### Phase 1: Initiation
- **Duration:** ≤ 500ms from trigger detection.
- **Actions:**
  1. Sender performs all quality gates (see Section 5).
  2. Sender compiles handover payload (see Section 4).
  3. Sender computes payload checksum (SHA-256).
  4. Sender logs handover initiation event.
  5. Sender transmits `HandoverRequest` message to target receiver.

#### Phase 2: Validation
- **Duration:** ≤ 1s from receipt of request.
- **Actions:**
  1. Receiver validates payload checksum.
  2. Receiver checks schema compatibility.
  3. Receiver assesses own capacity (must have < 80% load).
  4. Receiver verifies it has required capabilities for the task.
  5. Receiver sends `HandoverResponse` (accept or reject with reason).

#### Phase 3: Transfer
- **Duration:** ≤ 2s (cold) or ≤ 25s (warm, including overlap period).
- **Actions:**
  1. If accepted: sender streams full payload to receiver.
  2. Receiver loads context into working memory.
  3. Receiver loads artifact references and materializes artifacts.
  4. Receiver initializes task state from sender's state snapshot.
  5. For warm handover: both agents execute concurrently for overlap period.

#### Phase 4: Acknowledgment
- **Duration:** ≤ 2s from successful context loading.
- **Actions:**
  1. Receiver verifies state consistency.
  2. Receiver sends `HandoverAck` message.
  3. Sender marks handover as acknowledged.
  4. System updates task ownership registry.

#### Phase 5: Completion
- **Duration:** ≤ 500ms after acknowledgment.
- **Actions:**
  1. Sender releases all locks and resources.
  2. Sender cleans up temporary artifacts.
  3. Sender logs handover completion event.
  4. System updates handover metrics.
  5. Sender transitions to next task or idle state.

**Total target time:** Cold handover ≤ 5s; Warm handover ≤ 30s.

### 1.4 Responsible Agent for Initiating Handover

**Rule 1.4.1:** The **current handler** — the agent that currently owns the task — is ALWAYS responsible for initiating the handover. No other agent, including supervisors, may force-transfer a task away from the current handler without first invoking the emergency handover protocol (see Section 11).

**Rule 1.4.2:** If the current handler is unresponsive or crashed, the **Agent Health Monitor** (a system-level component) detects the failure within 3s and triggers emergency handover (see Section 11).

**Rule 1.4.3:** The current handler MUST NOT initiate a handover that would violate any of the following:
- The receiver is explicitly listed as blocked in the agent directory.
- The receiver has a current load > 90% of capacity.
- The receiver is in a different trust zone without proper cross-zone authorization.
- The task has active holds (e.g., awaiting external input) that would be lost on transfer.

---

## 2. Handover Types

### 2.1 Vertical Handover (Superior–Subordinate)

Vertical handovers move task ownership up or down the organizational hierarchy.

#### 2.1.1 Upward Handover (Escalation)

| Aspect | Specification |
|--------|---------------|
| **Direction** | Subordinate → Superior (Team Lead → Dept Head → Exec → CE-AI) |
| **Primary trigger** | Decision authority exceeded; unresolved error after max retries; resource requirement beyond agent's budget |
| **Payload** | Full task context + escalation reason + proposed resolution options |
| **Receiver obligation** | Must respond within 5s; may accept, reject with guidance, or delegate to different subordinate |
| **Authority escalation path** | Agent → Team Lead (L1) → Dept Head (L2) → Exec (L3) → CE-AI (L4) |
| **Chain break allowed** | Agent may skip one level if immediate superior is unavailable (e.g., Agent → Dept Head if Team Lead offline) |

**Example Scenario:** Agent `KG-112` is building a knowledge graph node for the concept "Zakat al-Fitr" and encounters conflicting rulings between Hanafi and Shafi'i schools. The agent's confidence threshold for autonomous resolution is 0.85, but its self-assessed confidence is 0.62. It initiates an upward handover to the KG Team Lead. The Team Lead reviews the conflict, adds a `school_of_thought` disambiguation attribute, and hands back down.

#### 2.1.2 Downward Handover (Delegation)

| Aspect | Specification |
|--------|---------------|
| **Direction** | Superior → Subordinate |
| **Primary trigger** | Task decomposition (superior breaks work into subtasks); overload relief; capability matching |
| **Payload** | Sub-task specification; expected output format; deadline; quality criteria |
| **Receiver obligation** | Must accept unless at capacity (load > 85%); rejection must include current load metrics |
| **Supervision mode** | May be supervised (superior reviews output before finalization) or unsupervised (subordinate has full authority) |

**Example Scenario:** The Book Acquisition Department Head receives a batch of 50 new books. She decomposes the batch into 5 groups of 10 and performs a downward handover to 5 Acquisition Team Leads. Each Team Lead further decomposes into individual assignments and hands down to individual Acquisition Agents.

### 2.2 Horizontal Handover (Peer-to-Peer)

Horizontal handovers occur between agents at the same hierarchical level.

#### 2.2.1 Same-Team Handover

| Aspect | Specification |
|--------|---------------|
| **Direction** | Peer Agent → Peer Agent within same team |
| **Primary trigger** | Load balancing; specialization handoff (e.g., Arabic OCR → Urdu OCR); shift change |
| **Constraints** | Both agents must be in same team; both must have same authority level |
| **Coordination** | Team Lead is notified but does not need to approve |
| **Conflict resolution** | If receiver rejects, sender may try another peer; if all peers reject, escalate to Team Lead |

#### 2.2.2 Same-Department Handover

| Aspect | Specification |
|--------|---------------|
| **Direction** | Agent → Agent within same department but different team |
| **Primary trigger** | Cross-team capability required; team overflow |
| **Constraints** | Must have cross-team routing approval (granted automatically unless revoked) |
| **Coordination** | Both team leads are notified |

### 2.3 Cross-Departmental Handover

| Aspect | Specification |
|--------|---------------|
| **Direction** | Agent in Department A → Agent in Department B |
| **Primary trigger** | Workflow pipeline progression (e.g., Content → Translation) |
| **Payload** | Standard payload + inter-department transfer manifest (compliance, SLA commitments, budget codes) |
| **Authorization** | Must include department-level authorization token |
| **SLAs** | Cross-department handovers have 10s SLA (more complex routing) |
| **Audit** | Dual logging: both departments' audit trails record the handover |
| **Cost transfer** | Budget allocation for the task moves from Department A to Department B |

**Example Scenario:** The Content Creation Department completes an Islamic children's story. The story must now be translated into 8 languages. The Story Engine Agent initiates a cross-departmental handover to the Translation Department. The handover payload includes the story text, illustration descriptions, formatting requirements, and a budget code for translation costs.

### 2.4 Shift Handover

See full details in Section 10. Brief specification:

| Aspect | Specification |
|--------|---------------|
| **Direction** | Outgoing shift agent → Incoming shift agent |
| **Scope** | ALL active tasks owned by outgoing agent |
| **Timing** | Begins 15 min before shift end; must complete by shift end |
| **Batch** | All tasks handed over as a batch in a single shift-handover bundle |
| **Template** | Standard shift handover template (see Section 10.6) |

### 2.5 Emergency Handover

See full details in Section 11. Brief specification:

| Aspect | Specification |
|--------|---------------|
| **Direction** | System (Agent Health Monitor) → Backup Agent |
| **Trigger** | Agent crash, critical error, resource exhaustion |
| **Initiation** | By Agent Health Monitor, NOT by the failing agent |
| **Speed** | Must complete within 1s (the failing agent's state must be frozen immediately) |
| **Data loss tolerance** | Zero tolerance; state is checkpointed every 30s during task execution |

### 2.6 Warm Handover

| Aspect | Specification |
|--------|---------------|
| **Definition** | Both sender and receiver operate concurrently during a defined overlap period |
| **Overlap duration** | 10–25s (configurable per task type) |
| **Use case** | Complex tasks where full context transfer is insufficient; receiver needs to observe sender's workflow |
| **Process** | 1. Context transferred. 2. Receiver begins shadow execution. 3. During overlap, receiver can query sender for clarification. 4. At overlap end, sender terminates; receiver continues independently. |
| **Resource cost** | 2× agent resource consumption during overlap |
| **Approval** | Requires Team Lead approval (due to resource cost) |

### 2.7 Cold Handover

| Aspect | Specification |
|--------|---------------|
| **Definition** | Sender completes all work, releases all resources, and terminates task ownership BEFORE receiver begins |
| **No overlap** | Zero seconds of concurrent execution |
| **Use case** | Simple, well-defined tasks; tasks with no ambiguity; shift handovers |
| **Process** | 1. Sender completes and cleans up. 2. Receiver starts with fresh context load. |
| **Risk** | Loss of tacit context not captured in payload |
| **Mitigation** | Sender must produce a comprehensive handover note (min 50 words for any cold handover) |

---

## 3. Handover Protocol Specification

### 3.1 Handover Request Message Schema

The `HandoverRequest` message MUST conform to the following schema:

```
{
  "message_type": "HandoverRequest",
  "protocol_version": "2.1",
  "handover_id": "HO-20260709-00000001",       // unique, globally
  "correlation_id": "TASK-20260709-004217",     // original task ID
  "conversation_thread_id": "THREAD-f3a2c8b1",  // continuity thread
  "sender": {
    "agent_id": "OCR-047",
    "agent_type": "OCR_Agent",
    "team": "OCR_Team_Alpha",
    "department": "Book_Digitization",
    "authority_level": 2
  },
  "receiver": {
    "agent_id": "VER-023",
    "agent_type": "Verification_Agent",
    "team": "Verification_Team_Beta",
    "department": "Quality_Assurance"
  },
  "handover_type": "horizontal",                // vertical_up, vertical_down, horizontal, cross_dept, shift, emergency
  "handover_mode": "cold",                      // warm, cold
  "warm_overlap_seconds": 0,                    // 0 for cold, 10-25 for warm
  "trigger_reason": "task_completion",          // see trigger list
  "task_priority": 3,                           // 1 (critical) to 5 (low)
  "task_deadline_utc": "2026-07-09T18:00:00Z",
  "remaining_sla_seconds": 7200,
  "payload_summary": {
    "message_count": 47,
    "artifact_count": 3,
    "total_payload_bytes": 284710,
    "checksum_sha256": "a1b2c3d4e5f6..."
  },
  "quality_gates": {
    "state_validated": true,
    "completeness_checked": true,
    "error_checked": true,
    "documentation_populated": true,
    "locks_released": true,
    "cleanup_completed": true
  },
  "timestamp_utc": "2026-07-09T10:30:15.123Z"
}
```

**Field Rules:**
- `handover_id` MUST be globally unique. Format: `HO-YYYYMMDD-NNNNNNNN` (8-digit sequence, resets daily).
- `protocol_version` MUST be `2.1` or higher. Receiver MUST reject if version mismatch > 1 minor version.
- `conversation_thread_id` MUST be the same for all messages and handovers within a single task lifecycle.
- `payload_summary.checksum_sha256` MUST be computed over the full serialized payload.

### 3.2 Handover Acceptance Response

On validation success, receiver MUST respond with:

```
{
  "message_type": "HandoverResponse",
  "handover_id": "HO-20260709-00000001",
  "response": "accept",
  "receiver_ack_token": "ACK-TOKEN-x7y8z9",
  "estimated_load_after_accept": 0.72,           // 0.0 to 1.0
  "ready_within_ms": 250,                        // time until receiver begins receiving payload
  "timestamp_utc": "2026-07-09T10:30:15.623Z"
}
```

### 3.3 Handover Rejection Reasons and Retry Logic

#### 3.3.1 Standard Rejection Reasons

| Rejection Code | Reason | Sender Action | Auto-Retry? |
|----------------|--------|---------------|-------------|
| `REJ_CAPACITY` | Receiver load > 85% | Try next available peer | Yes |
| `REJ_CAPABILITY` | Receiver lacks required skill | Try specialist agent or escalate | No (route to different type) |
| `REJ_CHECKSUM` | Payload checksum mismatch | Recompute and resend | Yes (up to 3) |
| `REJ_SCHEMA` | Incompatible protocol version | Use adapter or find compatible receiver | No (escalate) |
| `REJ_AUTH` | Missing authorization token | Request auth token from issuer | Yes (up to 3) |
| `REJ_STALE` | Task deadline already passed | Escalate to task orchestrator | No |
| `REJ_DUPLICATE` | Handover already processed | Abort; this is a duplicate | No |

#### 3.3.2 Retry Logic

**Rule 3.3.1:** If a handover request is rejected with a retry-eligible code, the sender MUST retry with the following backoff schedule:

| Attempt | Delay Before Retry | Cumulative Time |
|---------|-------------------|-----------------|
| 1st retry | 1s | 1s |
| 2nd retry | 4s (2²) | 5s |
| 3rd retry | 9s (3²) | 14s |

Backoff formula: `delay = attempt²` seconds (exponential by square).

**Rule 3.3.2:** On each retry, the sender MUST:
1. Recompute the payload checksum (in case of bit-flip corruption).
2. Check that the receiver's status is still active.
3. Increment the `retry_count` field in the message.
4. Log the retry event to the handover audit trail.

**Rule 3.3.3:** If all 3 retries fail, the sender MUST escalate to the sender's Team Lead within 1s. The Team Lead MUST:
1. Review the rejection history.
2. Either manually route the task or resolve the blocking issue.
3. If the Team Lead cannot resolve within 30s, escalate to Department Head.

### 3.4 Handover Completion Acknowledgment

After successfully loading context and verifying state, receiver MUST send:

```
{
  "message_type": "HandoverAck",
  "handover_id": "HO-20260709-00000001",
  "receiver_ack_token": "ACK-TOKEN-x7y8z9",
  "status": "operational",                      // operational, degraded, warning
  "context_loaded": true,
  "artifacts_materialized": true,
  "state_verified": true,
  "continuity_verified": true,
  "progress_notification_sent": true,
  "timestamp_utc": "2026-07-09T10:30:17.800Z"
}
```

### 3.5 Timeout Specifications

| Handover Mode | Phase | Timeout | Action on Timeout |
|---------------|-------|---------|-------------------|
| Cold | Initiation → Validation | 3s | Retry; after 3 retries escalate |
| Cold | Validation → Transfer | 2s | Sender resends payload |
| Cold | Transfer → Acknowledgment | 2s | Sender retransmits payload |
| Cold | Total end-to-end | 5s | Escalate to Team Lead |
| Warm | Total end-to-end | 30s | Escalate to Team Lead |
| Emergency | Total end-to-end | 1s | Automatic failover to secondary backup |

**Rule 3.5.1:** Timeout is measured from sender's perspective (time of message send to time of expected response receipt). Network latency budget: 100ms per hop.

**Rule 3.5.2:** If a handover exceeds 50% of its total timeout budget, the sender MUST log a warning. If it exceeds 80%, the sender MUST notify the team monitoring dashboard.

### 3.6 Message Sequence Diagram

```
Sender                                    Receiver              System Log
  │                                         │                      │
  │─── HandoverRequest ──────────────────→  │                      │
  │                                         │── validate ───────→ │
  │                                         │←──── valid ──────── │
  │←── HandoverResponse (accept) ───────── │                      │
  │                                         │                      │
  │─── (stream) Payload ──────────────────→ │                      │
  │                                         │── load context ───→ │
  │                                         │── verify state ────→ │
  │                                         │                      │
  │←── HandoverAck ──────────────────────── │                      │
  │                                         │                      │
  │── release locks ───────────────────────→│                      │
  │── cleanup temp ────────────────────────→│                      │
  │                                         │                      │
  │── HandoverComplete (log event) ────────→│────────────────────→ │
  │                                         │                      │
```

---

## 4. Handover Payload

### 4.1 Payload Structure

The handover payload is a JSON document with the following top-level sections. Maximum uncompressed size: 10 MB. If payload exceeds 10 MB, the sender MUST use reference-based handover (see Section 8.3).

```
{
  "handover_id": "HO-20260709-00000001",
  "payload_version": "2.1",
  "compression": "gzip",                        // none, gzip, zstd
  "checksum_sha256": "a1b2c3d4e5f6...",
  
  // SECTION 1: Task Context
  "task_context": { ... },
  
  // SECTION 2: State Snapshot
  "state_snapshot": { ... },
  
  // SECTION 3: Artifact References
  "artifact_references": [ ... ],
  
  // SECTION 4: Error History
  "error_history": [ ... ],
  
  // SECTION 5: Unresolved Issues
  "unresolved_issues": [ ... ],
  
  // SECTION 6: Recommendations
  "recommendations": [ ... ],
  
  // SECTION 7: Priority and Deadline
  "priority_and_deadline": { ... },
  
  // SECTION 8: Conversation Thread
  "conversation_thread_id": "THREAD-f3a2c8b1"
}
```

### 4.2 Task Context

Contains the complete history of all messages, decisions, and intermediate results for the task.

```
"task_context": {
  "task_id": "TASK-20260709-004217",
  "task_type": "book_ocr_processing",
  "workflow_id": "WF-BOOK-ACQ-TO-KG",
  "workflow_step": 2,                            // step in workflow pipeline
  "created_at_utc": "2026-07-09T08:00:00Z",
  "last_updated_utc": "2026-07-09T10:30:15Z",
  "message_log": [
    {
      "sequence": 1,
      "timestamp": "2026-07-09T08:00:05Z",
      "agent_id": "ACQ-012",
      "message_type": "task_assignment",
      "content_summary": "Book 'Stories of the Prophets' assigned for OCR processing",
      "content_uri": "mem://tasks/TASK-20260709-004217/messages/001"
    },
    {
      "sequence": 2,
      "timestamp": "2026-07-09T08:00:10Z",
      "agent_id": "OCR-047",
      "message_type": "task_initiation",
      "content_summary": "OCR processing started, 247 pages detected",
      "content_uri": "mem://tasks/TASK-20260709-004217/messages/002"
    },
    // ... 45 more messages ...
  ],
  "decisions_log": [
    {
      "decision_id": "DEC-001",
      "timestamp": "2026-07-09T08:45:12Z",
      "decision": "selected_tesseract_v5_over_ocrmypdf",
      "rationale": "Tesseract v5 provides better Arabic script recognition with the 'Arabic+Islamic' language model pack",
      "confidence": 0.92
    },
    {
      "decision_id": "DEC-002",
      "timestamp": "2026-07-09T09:30:05Z",
      "decision": "skipped_page_142_repair",
      "rationale": "Page 142 is severely damaged; repair would take >30 min with uncertain outcome. Flagged for manual processing.",
      "confidence": 0.78
    }
  ],
  "intermediate_results": {
    "pages_processed": 247,
    "pages_successful": 245,
    "pages_failed": 2,
    "total_characters": 84721,
    "avg_confidence": 0.943,
    "language_detected": "ar",
    "result_uri": "mem://tasks/TASK-20260709-004217/results/ocr_output_v2.json"
  }
}
```

### 4.3 State Snapshot

The complete working memory state of the sender at the moment of handover.

```
"state_snapshot": {
  "working_memory": {
    "loaded_artifacts": [
      "mem://books/source/978-977-XXX-XXX-X/raw",
      "mem://books/source/978-977-XXX-XXX-X/ocr_output_pages_1-100",
      "mem://books/source/978-977-XXX-XXX-X/ocr_output_pages_101-247"
    ],
    "active_processes": [],
    "pending_operations": [
      {
        "operation": "apply_post_processing_corrections",
        "status": "completed"
      },
      {
        "operation": "generate_confidence_report",
        "status": "completed"
      },
      {
        "operation": "flag_low_confidence_pages",
        "status": "completed"
      }
    ],
    "variables": {
      "current_page_index": 247,
      "total_pages": 247,
      "correction_threshold": 0.85,
      "damaged_page_indices": [142, 189],
      "target_output_format": "json_utf8"
    }
  },
  "memory_usage_bytes": 8471032,
  "peak_memory_usage_bytes": 12400000,
  "task_progress_pct": 100.0,
  "uptime_seconds_on_task": 9035
}
```

### 4.4 Artifact References

List of all artifacts created or modified during the sender's task ownership.

```
"artifact_references": [
  {
    "artifact_id": "ART-20260709-0001",
    "artifact_name": "ocr_output_v2.json",
    "artifact_type": "json_data",
    "uri": "mem://tasks/TASK-20260709-004217/results/ocr_output_v2.json",
    "checksum_sha256": "b2c3d4e5f6a7...",
    "size_bytes": 284710,
    "created_at_utc": "2026-07-09T10:28:00Z",
    "last_modified_utc": "2026-07-09T10:30:00Z",
    "schema_version": "ocr_output_2.1",
    "tags": ["ocr", "arabic", "stories_of_the_prophets"],
    "dependencies": []                            // artifact IDs this one depends on
  },
  {
    "artifact_id": "ART-20260709-0002",
    "artifact_name": "confidence_report_pages_1-247.csv",
    "artifact_type": "csv_report",
    "uri": "mem://tasks/TASK-20260709-004217/reports/confidence_report.csv",
    "checksum_sha256": "c3d4e5f6a7b8...",
    "size_bytes": 48720,
    "created_at_utc": "2026-07-09T10:29:30Z",
    "tags": ["confidence", "report", "quality"]
  },
  {
    "artifact_id": "ART-20260709-0003",
    "artifact_name": "damaged_pages_report.json",
    "artifact_type": "json_report",
    "uri": "mem://tasks/TASK-20260709-004217/reports/damaged_pages.json",
    "checksum_sha256": "d4e5f6a7b8c9...",
    "size_bytes": 1250,
    "created_at_utc": "2026-07-09T10:29:45Z",
    "tags": ["damaged", "pages", "flag"]
  }
]
```

### 4.5 Error History

Complete record of all errors encountered, attempted fixes, and outcomes.

```
"error_history": [
  {
    "error_id": "ERR-20260709-0001",
    "timestamp_utc": "2026-07-09T08:23:15Z",
    "error_type": "ocr_failure",
    "severity": "warning",                       // info, warning, error, critical
    "location": "page_142",
    "description": "OCR engine returned confidence 0.12 for page 142 (damaged binding, text obscured)",
    "attempted_fixes": [
      {
        "fix_id": "FIX-001",
        "fix_type": "reprocess_with_alternative_engine",
        "description": "Reprocessed with OCRmyPDF with deskew enhancement",
        "result": "confidence improved to 0.34, still below threshold of 0.85",
        "timestamp_utc": "2026-07-09T08:25:30Z"
      },
      {
        "fix_id": "FIX-002",
        "fix_type": "image_preprocessing",
        "description": "Applied adaptive thresholding and contrast enhancement before OCR",
        "result": "confidence improved to 0.41, still below threshold",
        "timestamp_utc": "2026-07-09T08:28:00Z"
      },
      {
        "fix_id": "FIX-003",
        "fix_type": "manual_override_decision",
        "description": "Skipped page 142, flagged for human processing",
        "result": "Page marked as damaged in damaged_pages_report.json",
        "timestamp_utc": "2026-07-09T08:30:00Z"
      }
    ],
    "resolution": "page_skipped_and_flagged",
    "resolved_at_utc": "2026-07-09T08:30:00Z"
  },
  {
    "error_id": "ERR-20260709-0002",
    "timestamp_utc": "2026-07-09T09:12:40Z",
    "error_type": "memory_pressure",
    "severity": "warning",
    "location": "agent_level",
    "description": "Memory usage reached 92% of allocated heap during bulk processing of pages 150-200",
    "attempted_fixes": [
      {
        "fix_id": "FIX-004",
        "fix_type": "batch_size_reduction",
        "description": "Reduced batch size from 50 pages to 20 pages",
        "result": "Memory usage stabilized at 78%",
        "timestamp_utc": "2026-07-09T09:13:00Z"
      }
    ],
    "resolution": "batch_size_adjusted",
    "resolved_at_utc": "2026-07-09T09:13:00Z"
  }
]
```

### 4.6 Unresolved Issues

Items that remain open and require the receiving agent's attention.

```
"unresolved_issues": [
  {
    "issue_id": "ISS-20260709-0001",
    "type": "damaged_content",
    "description": "Pages 142 and 189 could not be OCR processed due to physical book damage",
    "impact": "2 pages out of 247 (0.8%) have no digital text content",
    "severity": "low",
    "suggested_approach": "Manual transcription by human operator; or locate alternative copy of book",
    "deadline_for_resolution": "2026-07-16T18:00:00Z",
    "dependencies": [],
    "status": "open"
  },
  {
    "issue_id": "ISS-20260709-0002",
    "type": "quality_concern",
    "description": "Pages 45-52 have mixed Arabic-French text; OCR may have misattributed some French words",
    "impact": "Potential translation errors if French phrases are incorrectly treated as Arabic",
    "severity": "medium",
    "suggested_approach": "Language identification post-processing on these 8 pages before KG import",
    "deadline_for_resolution": "2026-07-11T18:00:00Z",
    "dependencies": [],
    "status": "open"
  }
]
```

### 4.7 Recommendations

Suggested next steps for the receiving agent to follow.

```
"recommendations": [
  {
    "priority": 1,
    "action": "verify_ocr_confidence",
    "description": "Run verification on OCR output, focusing on pages with confidence < 0.90",
    "rationale": "Standard workflow step 3 requires verification before KG import",
    "estimated_effort_minutes": 45,
    "depends_on": []
  },
  {
    "priority": 2,
    "action": "resolve_damaged_pages",
    "description": "Initiate manual transcription workflow for pages 142 and 189",
    "rationale": "Missing content will cause gaps in knowledge graph",
    "estimated_effort_minutes": 60,
    "depends_on": []
  },
  {
    "priority": 3,
    "action": "language_audit_pages_45-52",
    "description": "Perform language-specific post-processing on pages 45-52 to separate Arabic and French content",
    "rationale": "Mixed-language pages risk cross-contamination in KG entities",
    "estimated_effort_minutes": 20,
    "depends_on": ["verify_ocr_confidence"]
  }
]
```

### 4.8 Priority and Deadline

```
"priority_and_deadline": {
  "priority": 3,                                 // 1 (critical) to 5 (low)
  "priority_label": "normal",
  "original_deadline_utc": "2026-07-09T18:00:00Z",
  "remaining_sla_seconds": 7200,
  "sla_tier": "standard",                        // standard, premium, critical
  "sla_violation_penalty": "medium",             // none, low, medium, high, critical
  "sla_escalation_at_pct": 0.8,                  // escalate when 80% of SLA elapsed
  "budget_code": "BUD-BOOK-ACQ-2026-Q3",
  "cost_center": "Book_Digitization",
  "billing_rate_per_hour": 0.42                  // USD equivalent compute cost
}
```

### 4.9 Conversation Thread ID

```
"conversation_thread_id": "THREAD-f3a2c8b1",
"thread_origin": "TASK-20260709-004217",
"thread_participants": ["ACQ-012", "OCR-047", "VER-023"],
"thread_tags": ["book_ocr", "stories_of_the_prophets", "arabic"],
"parent_thread_id": null
```

**Rule 4.9.1:** The `conversation_thread_id` MUST remain constant across all handovers for a single task. This enables full traceability across the entire workflow pipeline.

**Rule 4.9.2:** The thread origin is the original task ID. All subsequent handovers within that task chain reference the same thread.

---

## 5. Quality Gates Before Handover

### 5.1 Gate Overview

Before a handover may be initiated, the sender MUST pass all 7 quality gates. These gates are executed in sequence. If any gate fails, the sender MUST remediate the issue before retrying the gate.

```
Gate Sequence:
  Gate 1: State Validation
  Gate 2: Completeness Check
  Gate 3: Error Check
  Gate 4: Documentation Check
  Gate 5: Lock Release
  Gate 6: Cleanup
  Gate 7: Final Integrity Check
```

### 5.2 Gate 1: State Validation

**Purpose:** Verify that the task state is internally consistent and has not been corrupted.

**Checks:**
1. Is the task state machine in a valid state for handover? (States: `in_progress`, `awaiting_review`, `completed`; NOT valid: `failed`, `orphaned`, `corrupted`)
2. Are all state variables non-null and within expected ranges?
3. Does the actual progress percentage match the recorded progress?
4. Is the task's current owner correctly set to the sender agent ID?
5. Is there no other agent currently performing operations on this task?

**Pass Criteria:** All 5 checks MUST pass.
**Fail Action:** If check 1 fails (invalid state), attempt automatic state recovery. If unrecoverable, mark task as failed and escalate.
**Timeout:** 500ms maximum.

### 5.3 Gate 2: Completeness Check

**Purpose:** Verify that all required artifacts for this stage of the workflow have been created.

**Checks:**
1. For each artifact declared in the workflow step's output specification, does a corresponding artifact exist in the artifact registry?
2. Does each artifact have a valid checksum?
3. Are all artifact metadata fields populated?
4. Do interdependent artifacts have their dependency chain intact?

**Pass Criteria:** All required artifacts exist and are valid. 100% of declared outputs must be present.
**Fail Action:** If any required artifact is missing, the sender MUST attempt to generate it. If generation fails, the handover is blocked; escalate to Team Lead.
**Timeout:** 1s maximum.

### 5.4 Gate 3: Error Check

**Purpose:** Verify that all errors have been either resolved or explicitly documented as unresolved issues.

**Checks:**
1. Are all errors in the error history either marked `resolved` or linked to an unresolved issue?
2. Are there any unlogged errors in the sender's working memory?
3. Are there active exceptions in the execution context?

**Pass Criteria:** Zero unhandled/unlogged errors; all known errors documented in error history.
**Fail Action:** If unlogged errors exist, they MUST be logged before proceeding. If active exceptions exist, they MUST be handled.
**Timeout:** 500ms maximum.

### 5.5 Gate 4: Documentation Check

**Purpose:** Verify that the handover note is populated and provides sufficient context.

**Checks:**
1. Is the `handover_note` field populated? (Required minimum: 50 characters for cold handover, 100 characters for warm handover)
2. Does the note include: (a) current status summary, (b) any unusual decisions made, (c) known risks or issues?
3. For shift handover: is the shift handover template completely filled? (See Section 10.6)

**Pass Criteria:** Handover note populated and meets minimum length; all template fields filled.
**Fail Action:** Sender MUST complete the handover note. If sender cannot generate adequate documentation (e.g., context loss), request assistance from Team Lead.
**Timeout:** 2s maximum (for note generation).

### 5.6 Gate 5: Lock Release

**Purpose:** Verify that all resource locks held by the sender for this task are released.

**Checks:**
1. Are all file-level locks released?
2. Are all KG node-level locks released?
3. Are all database row-level locks released?
4. Are all inter-agent semaphore locks released?
5. Is there any held mutex or critical section?

**Pass Criteria:** Zero locks held by the sender for resources related to this task.
**Fail Action:** Sender MUST forcibly release all locks. If a lock cannot be released (stuck), log the lock ID and escalate to system administrator.
**Timeout:** 500ms maximum.

### 5.7 Gate 6: Cleanup

**Purpose:** Verify that all temporary resources created during the sender's task execution are properly cleaned up.

**Checks:**
1. Are all temporary files deleted or moved to appropriate permanent storage?
2. Are all temporary database tables dropped or archived?
3. Are all scratch space buffers cleared?
4. Are all ephemeral connections (HTTP, gRPC, database) closed?
5. Are any spawned sub-processes terminated?

**Pass Criteria:** No temporary resources remain in non-permanent storage. All connections closed.
**Fail Action:** Sender MUST retry cleanup operations. If cleanup fails repeatedly, tag resources with `orphaned` flag and proceed (cleanup will be handled by garbage collection).
**Timeout:** 1s maximum.

### 5.8 Gate 7: Final Integrity Check

**Purpose:** A comprehensive final verification that everything is ready for handover.

**Checks:**
1. All previous 6 gates have passed (re-verified).
2. Payload checksum is computed and matches the payload content.
3. Handover request message is valid per schema.
4. Receiver is available (ping check or directory lookup).
5. Network path to receiver is alive.
6. System clock skew between sender and receiver is < 100ms.

**Pass Criteria:** All 6 checks pass.
**Fail Action:** If receiver is unavailable, select alternate receiver from routing table.
**Timeout:** 500ms maximum.

### 5.9 Gate Failure Escalation

| Consecutive Failures | Action |
|----------------------|--------|
| 1 failure | Sender retries the gate |
| 2 failures (same gate) | Sender logs warning and retries with different approach |
| 3 failures (same gate) | Sender escalates to Team Lead with full gate failure details |
| 5 failures (any gate within same handover) | Task is placed on hold; Team Lead must manually resolve |

---

## 6. Receiving Agent Responsibilities

### 6.1 State Verification (Check Payload Integrity)

Upon receiving a `HandoverRequest`, the receiver MUST perform the following verifications within the validation phase (≤ 1s):

| Check | Verification | Action on Failure |
|-------|-------------|-------------------|
| Payload checksum | Compute SHA-256 of received payload; compare with `payload_summary.checksum_sha256` | Reject with `REJ_CHECKSUM` |
| Schema version | Check `protocol_version` ≥ 2.0 and within +/-1 minor version | Reject with `REJ_SCHEMA` |
| Required fields | Validate all required fields in schema are present | Reject with `REJ_SCHEMA` |
| Task ID format | Validate `correlation_id` matches `TASK-YYYYMMDD-NNNNNN` pattern | Reject with `REJ_SCHEMA` |
| Timestamp sanity | Validate `timestamp_utc` is within 30s of receiver's current time | Accept with time-warning log |
| Sender identity | Verify sender agent ID exists in agent directory | Reject with `REJ_AUTH` |
| Authorization token | Verify sender has authority to transfer this task type | Reject with `REJ_AUTH` |

**Rule 6.1.1:** The receiver MUST NOT modify any payload data during verification. The payload is immutable at this stage.

### 6.2 Context Loading (Load All Context into Working Memory)

After accepting the handover, the receiver MUST load the complete payload into its working memory.

**Loading Procedure (≤ 2s for cold, ≤ 25s for warm):**
1. Deserialize payload from JSON (decompress if gzip/zstd compressed).
2. Load `task_context.message_log` — reconstruct conversation history in working memory.
3. Load `task_context.decisions_log` — understand all prior decisions and their rationale.
4. Load `state_snapshot` — initialize variables, active processes, pending operations.
5. Materialize artifacts: for each URI in `artifact_references`, fetch and load into working memory.
6. Load `error_history` — understand what went wrong and what fixes were attempted.
7. Load `unresolved_issues` — create tracking entries for each open issue.
8. Load `recommendations` — prioritize recommendations in receiver's action queue.

**Memory Budget:**
| Payload Size | Context Load Budget | Notes |
|-------------|-------------------|-------|
| < 1 MB | 500ms | Standard case |
| 1–5 MB | 1s | Moderate payload |
| 5–10 MB | 2s | Large payload; may trigger reference-based handover |
| > 10 MB | N/A | MUST use reference-based handover (Section 8.3) |

**Rule 6.2.1:** If context loading fails (e.g., artifact URI is invalid or inaccessible), the receiver MUST:
1. Log the specific failure.
2. Attempt to load from backup URI if available.
3. If backup fails, send a `HandoverNack` (negative acknowledgment) with details.
4. Sender must then resolve the missing artifact and retry.

### 6.3 Continuity Check (Ensure No Gaps)

The receiver MUST verify that there is no discontinuity between what the sender completed and what remains to be done.

**Continuity Verification Checklist:**
1. **Task state continuity:** Does the state snapshot show the task at the expected completion point for the workflow step?
2. **Artifact continuity:** Are all expected artifacts present for this workflow step? (Compare against workflow definition.)
3. **Decision continuity:** Would the receiver have made the same decisions given the same context? (If not, log a concern but proceed.)
4. **Error continuity:** Are all errors documented in the error history reflected in the artifact state? (e.g., if an error says page 142 failed, is page 142 actually missing from the OCR output?)
5. **Time continuity:** Is the elapsed time since task creation reasonable? (Alert if elapsed > 2× expected duration.)
6. **Dependency continuity:** Are all dependencies listed in artifact_references actually satisfied?

**Pass Criteria:** All 6 checks pass with no critical gaps.
**Fail Action:** If critical gap found (e.g., missing artifact, state inconsistency), receiver MUST:
1. Request clarification from sender via the conversation thread.
2. If sender cannot clarify (already released), escalate to Team Lead.
3. Team Lead decides: proceed with gap, roll back, or manually patch.

### 6.4 Acknowledgment (Confirm Receipt Within 2s)

**Rule 6.4.1:** Within 2s of completing context loading and continuity verification, the receiver MUST send a `HandoverAck` message.

**Rule 6.4.2:** The acknowledgment MUST include:
- `handover_id` (copied from request)
- `receiver_ack_token` (token from the acceptance response)
- `status`: one of `operational`, `degraded`, `warning`
- All continuity check results
- Timestamp

**Rule 6.4.3:** If the receiver cannot acknowledge within 2s, it MUST send a `HandoverProgress` message every 500ms indicating it is still processing.

```
// HandoverProgress message (sent every 500ms during extended processing)
{
  "message_type": "HandoverProgress",
  "handover_id": "HO-20260709-00000001",
  "progress_pct": 75,
  "current_phase": "context_loading",
  "estimated_completion_ms": 1200,
  "timestamp_utc": "2026-07-09T10:30:17.300Z"
}
```

### 6.5 Progress Notification (Inform Stakeholders)

Within 3s of sending `HandoverAck`, the receiver MUST notify all relevant stakeholders of the handover.

**Notification Recipients:**
| Stakeholder | Notification Method | Required? |
|-------------|-------------------|-----------|
| Sender agent | Direct message (automated via Ack) | Mandatory |
| Sender's Team Lead | Dashboard update + optional notification | Mandatory |
| Receiver's Team Lead | Dashboard update | Mandatory |
| Task orchestrator | System event | Mandatory |
| Workflow manager | System event | Mandatory |
| All prior handlers in task chain | Dashboard update | Recommended |
| Human supervisor (if applicable) | Dashboard update + email if priority ≤ 2 | Only for high-priority tasks |

**Notification Content:**
```
Task TASK-20260709-004217 handed over from OCR-047 to VER-023
  Handover type: horizontal (same department)
  Handover mode: cold
  Handover ID: HO-20260709-00000001
  Task progress: 100% (OCR complete) → starting Verification
  Remaining SLA: 7200s
  Priority: 3 (normal)
  Next action: verify_ocr_confidence (estimated 45 min)
```

### 6.6 Failure to Acknowledge — Sender Actions

If the sender does not receive a `HandoverAck` within the timeout:

| Time Elapsed | Sender Action |
|-------------|---------------|
| 2s (timeout) | Send first retry of `HandoverRequest` |
| 2s + 1s (retry 1 timeout) | Send second retry |
| 2s + 1s + 4s (retry 2 timeout) | Send third retry |
| 2s + 1s + 4s + 9s (retry 3 timeout) | **Escalate** to sender's Team Lead |

On escalation, the Team Lead MUST:
1. Determine if receiver is operational.
2. If receiver is operational but unresponsive: force-restart receiver's handover handler.
3. If receiver is down: re-route to alternate receiver.
4. If all receivers in routing table are down: escalate to Department Head.

### 6.7 Receiver Capacity Management

**Rule 6.7.1:** The receiver MUST accurately report its load in the `HandoverResponse`. Load is calculated as:

```
load = (active_tasks / max_concurrent_tasks) + (memory_used_bytes / memory_limit_bytes) / 2
```

The result is a value between 0.0 and 1.0.

**Rule 6.7.2:** If receiver's load after accepting the handover would exceed 0.9, the receiver MUST reject with `REJ_CAPACITY`.

**Rule 6.7.3:** The receiver MUST update its load metrics immediately upon accepting or rejecting a handover to prevent race conditions with subsequent handover requests.

---

## 7. Handover in Specific Workflows

### 7.1 Workflow 1: Book Acquisition → OCR

**Workflow Step:** Step 1 → Step 2
**Sender:** Acquisition Agent (`ACQ-NNN`)
**Receiver:** OCR Agent (`OCR-NNN`)
**Handover Type:** Cross-team horizontal (within Book Digitization Department)

#### Context
An Acquisition Agent has completed the procurement and registration of a new book. The book's raw scanned images are stored in the content store. The Acquisition Agent must hand over to an OCR Agent for digitization.

#### Handover Scenario

```
Acquisition Agent (ACQ-012)                   OCR Agent (OCR-047)
       │                                            │
       │  1. Quality Gates                          │
       │     ├─ Gate 1: State = "completed" ✓       │
       │     ├─ Gate 2: All artifacts?              │
       │     │  - Raw scans: ✓ (247 pages)          │
       │     │  - Metadata JSON: ✓                  │
       │     │  - Acquisition report: ✓             │
       │     ├─ Gate 3: No errors ✓                 │
       │     ├─ Gate 4: Handover note written ✓     │
       │     ├─ Gate 5: Locks released ✓            │
       │     ├─ Gate 6: Cleanup done ✓              │
       │     └─ Gate 7: Final check ✓               │
       │                                            │
       │  2. HandoverRequest ──────────────────→    │
       │                                            │
       │  3. Validation                             │
       │     ├─ Checksum valid ✓                    │
       │     ├─ Schema valid ✓                      │
       │     ├─ Load check: 0.65 < 0.85 ✓           │
       │     └─ Capability: OCR ✓                   │
       │                                            │
       │  ←── HandoverResponse (accept) ────────    │
       │                                            │
       │  4. Payload Transfer                       │
       │     ├─ Task context (47 messages)          │
       │     ├─ State snapshot                      │
       │     ├─ Artifact references (3 artifacts)   │
       │     ├─ Recommendations                     │
       │     └─ Priority/deadline                   │
       │                                            │
       │  5. Context Loading                        │
       │     ├─ Load message history ✓              │
       │     ├─ Load artifacts ✓                    │
       │     └─ Verify continuity ✓                 │
       │                                            │
       │  ←── HandoverAck ──────────────────────    │
       │                                            │
       │  6. Completion                             │
       │     ├─ ACQ-012 releases resources          │
       │     ├─ ACQ-012 cleans up temp files        │
       │     └─ Both log handover event             │
       │                                            │
       │  [Task ownership: ACQ-012 → OCR-047]       │
```

#### Handover Payload Highlights
```
"task_context.task_type": "book_ocr_processing"
"task_context.message_log": [
  "Book 'Stories of the Prophets' assigned",
  "Scanned 247 pages at 300 DPI",
  "Metadata extracted: ISBN, author, publication date",
  "Acquisition cost: $127.50 recorded"
]
"artifact_references": [
  { "name": "raw_scans", "uri": "mem://books/978-977-XXX-XXX-X/raw/", "size": 847210432 },
  { "name": "book_metadata.json", "uri": "mem://books/978-977-XXX-XXX-X/metadata.json", "size": 4280 },
  { "name": "acquisition_report.pdf", "uri": "mem://books/978-977-XXX-XXX-X/acq_report.pdf", "size": 124800 }
]
"recommendations": [
  { "priority": 1, "action": "Run OCR with Arabic+Islamic language pack" },
  { "priority": 2, "action": "Set confidence threshold to 0.85" },
  { "priority": 3, "action": "Flag any page with confidence < 0.70 for review" }
]
```

### 7.2 Workflow 2: OCR → Verification

**Workflow Step:** Step 2 → Step 3
**Sender:** OCR Agent (`OCR-NNN`)
**Receiver:** Verification Agent (`VER-NNN`)
**Handover Type:** Horizontal (same team/QA department)

#### Key Handover Elements
- **Primary artifact:** OCR output JSON with per-page text, confidence scores, and layout data.
- **Error history:** Pages 142 and 189 failed OCR (damaged). Two fix attempts logged, ultimately flagged.
- **Unresolved issues:** Missing content for 2 pages; mixed Arabic-French text on pages 45-52.
- **Quality gates focus:** OCR confidence must be ≥ 0.85 overall; if below, OCR must re-process or flag.

#### Handover Validation Specifics
| Check | Criterion | Threshold |
|-------|-----------|-----------|
| OCR coverage | % of pages successfully processed | ≥ 98% |
| Average confidence | Mean confidence score across all pages | ≥ 0.85 |
| Low-confidence pages | Pages with confidence < 0.70 | ≤ 5% of total |
| Data format | OCR output conforms to schema v2.1 | Strict |

#### Handover Note Example
```
"handover_note": "OCR completed for 'Stories of the Prophets' (247 pages). 
Average confidence: 0.943. 245/247 pages processed successfully. 
Pages 142 and 189 skipped due to physical damage (flagged for manual processing).
Pages 45-52 contain mixed Arabic-French text requiring language separation during verification.
Overall quality acceptable for next stage. Estimated verification effort: 45 minutes."
```

### 7.3 Workflow 3: Verification → KG Import

**Workflow Step:** Step 3 → Step 4
**Sender:** Verification Agent (`VER-NNN`)
**Receiver:** Knowledge Graph Import Agent (`KG-NNN`)
**Handover Type:** Cross-team horizontal (QA → Knowledge Graph, both under Content Department)

#### Key Handover Elements
- **Primary artifact:** Verified OCR output with QA stamp.
- **Additional artifacts:** Verification report, corrected text, quality scores.
- **State snapshot includes:** `verified_pages: 245`, `corrected_characters: 1432`, `final_confidence: 0.971`.
- **Error history:** 2 damaged pages still unresolved; 3 text corrections required approval from Islamic content specialist.
- **Payload size estimate:** ~15 MB (mostly corrected text); uses reference-based handover (see 8.3).

#### Recommendations from Verification
```
"recommendations": [
  {
    "priority": 1,
    "action": "import_verified_text_to_kg",
    "description": "Import all 245 verified pages as KG document nodes"
  },
  {
    "priority": 2,
    "action": "create_damaged_page_placeholders",
    "description": "Create placeholder KG nodes for pages 142 and 189 with 'content_missing' flag"
  },
  {
    "priority": 3,
    "action": "tag_arabic_french_pages",
    "description": "Tag pages 45-52 with 'mixed_language' attribute for downstream processing"
  }
]
```

#### Continuity Check for KG Import
The KG Import Agent MUST verify:
1. All verified pages have corresponding entries in the OCR output.
2. The verification agent's corrections are reflected in the final text.
3. The page sequence is complete (no gaps except flagged damaged pages).
4. The metadata matches the original acquisition metadata.

### 7.4 Workflow 4: KG → Story Engine

**Workflow Step:** Step 4 → Step 5
**Sender:** KG Import Agent (`KG-NNN`)
**Receiver:** Story Engine Agent (`STY-NNN`)
**Handover Type:** Cross-departmental (Knowledge Graph → Content Creation)

#### Key Handover Elements
- **Primary artifact:** Knowledge subgraph containing entities, relationships, and properties extracted from the verified text.
- **Subgraph size:** ~1,200 nodes, ~4,500 edges for a typical 247-page Islamic book.
- **Additional artifacts:** KG schema mapping, entity resolution report, confidence scores per entity.
- **Handover mode:** Warm (15s overlap) — Story Engine needs to query KG import agent about ambiguous entities.

#### Warm Handover Procedure
```
1. KG Agent sends HandoverRequest with warm_overlap_seconds: 15
2. Story Engine accepts
3. KG Agent streams knowledge subgraph (payload)
4. Both agents run concurrently for 15 seconds:
   a. Story Engine loads subgraph and begins analysis
   b. Story Engine identifies ambiguous entities
   c. Story Engine queries KG Agent for clarification
   d. KG Agent responds with entity resolution details
5. After 15s: KG Agent terminates; Story Engine continues independently
```

#### Subgraph Content Highlights
```
"artifact_references": [
  {
    "name": "knowledge_subgraph",
    "uri": "kg://subgraphs/STORIES_OF_THE_PROPHETS_20260709",
    "size": 28400000,
    "node_count": 1200,
    "edge_count": 4500,
    "schema": "islamic_content_kg_schema_v3"
  }
]
"recommendations": [
  {
    "priority": 1,
    "action": "extract_story_narrative_arc",
    "description": "Extract sequential narrative from KG entity ordering"
  },
  {
    "priority": 2,
    "action": "identify_teaching_moments",
    "description": "Flag KG subgraphs that correspond to moral/educational lessons"
  },
  {
    "priority": 3,
    "action": "generate_character_profiles",
    "description": "Extract character entities and their attributes for story character generation"
  }
]
```

### 7.5 Workflow 5: Story Engine → Islamic Verifier

**Workflow Step:** Step 5 → Step 6
**Sender:** Story Engine Agent (`STY-NNN`)
**Receiver:** Islamic Content Verifier Agent (`ISV-NNN`)
**Handover Type:** Cross-departmental (Content Creation → Islamic Compliance)

#### Critical Compliance Handover
This is one of the most important handovers in the system. The Islamic Verifier MUST ensure that ALL generated content is theologically correct and appropriate for the target audience.

#### Handover Payload Additions
```
"compliance_context": {
  "target_audience": "children_ages_8-12",
  "islamic_school": "general"                    // hanafi, shafi'i, maliki, hanbali, general
  "sensitivity_level": "high",                   // standard, high, critical
  "required_verification_types": [
    "aqidah_correctness",                        // creed correctness
    "quranic_accuracy",                          // quranic verse quoting accuracy
    "hadith_authenticity",                       // hadith chain verification
    "fiqh_accuracy",                             // jurisprudential correctness
    "cultural_sensitivity",                      // cultural appropriateness
    "age_appropriateness"                        // suitability for target age group
  ],
  "critical_issues": [
    "Story includes depiction of Prophet Musa (AS) — must ensure no anthropomorphic descriptions",
    "Story references Hadith from Bukhari — must verify exact wording and chain"
  ]
}
```

#### Verification Handshake
**Rule 7.5.1:** This handover uses a dual-acknowledgment protocol:
1. **Ack 1:** Standard `HandoverAck` for payload receipt.
2. **Ack 2 (Compliance Ack):** After initial content scan, the Islamic Verifier sends a `ComplianceHandoverAck` within 30s confirming the content is within scope for verification.

**Rule 7.5.2:** If Ack 2 is not received within 30s, the Story Engine notifies the Islamic Compliance Department Head.

#### Handover Note
```
"handover_note": "Story 'The Patience of Prophet Ayyub (AS)' generated from 
KG subgraph. Story length: 2,400 words, target age: 8-12. 
Content includes: 3 Quranic references (Surah Sad 38:41-44, Surah Al-Anbiya 21:83-84, 
Surah Al-An'am 6:34), 2 Hadith references (Bukhari 3419, Muslim 2557). 
AI-generated narrative has NOT been reviewed by human scholar. 
All generated content is flagged as 'AI_draft' pending verification."
```

### 7.6 Workflow 6: Content → Translation

**Workflow Step:** Content pipeline → Translation pipeline
**Sender:** Content Release Agent (`REL-NNN`) in Content Department
**Receiver:** Translation Intake Agent (`TRL-NNN`) in Translation Department
**Handover Type:** Cross-departmental (Content → Translation)

#### Key Differences from Intra-Department Handover

| Aspect | Intra-Department | Cross-Departmental |
|--------|-----------------|-------------------|
| **Authorization** | Implicit (same dept) | Requires inter-department token |
| **Budget transfer** | Not needed | Cost center changes from `Content_Creation` to `Translation_Services` |
| **SLA** | 5s | 10s (additional routing hops) |
| **Audit** | Single department log | Dual log (both departments) |
| **Receiver selection** | Team routing table | Department routing table + load balancer |
| **Schema adapter** | Not needed | May need if departments use different message schema versions |

#### Cross-Departmental Handover Payload
```
"inter_department_manifest": {
  "source_department": "Content_Creation",
  "destination_department": "Translation_Services",
  "authorization_token": "CROSS-DEPT-TOKEN-a1b2c3d4",
  "authorized_by": "Content_Dept_Head",
  "authorization_timestamp": "2026-07-09T10:00:00Z",
  "budget_transfer": {
    "source_budget_code": "BUD-CONTENT-2026-Q3",
    "destination_budget_code": "BUD-TRANSLATION-2026-Q3",
    "amount": 340.00,
    "currency": "USD",
    "approved_by": "CFO_Agent"
  },
  "sla_commitment": {
    "original_task_deadline": "2026-07-16T18:00:00Z",
    "departmental_deadline": "2026-07-14T18:00:00Z",
    "sla_tier": "standard"
  },
  "compliance": {
    "content_clearance": "verified_and_approved",
    "clearance_id": "CLR-20260709-004217",
    "restrictions": ["requires_translation_review_by_human_scholar_for_urdu"]
  }
}
```

### 7.7 Workflow 7: Shift Change at End of Processing Day

**Sender:** Day shift agent (any)
**Receiver:** Night shift agent (any, same role)
**Handover Type:** Shift handover (see Section 10 for full details)

#### Brief Scenario
Agent `OCR-047` is on day shift (06:00–18:00 UTC). At 17:45 UTC, it initiates shift handover for all its active tasks. It has 3 active tasks:
- Task A: 85% complete (book OCR in progress)
- Task B: 100% complete (ready for handover to verification)
- Task C: 12% complete (just started another book)

The shift handover bundles all 3 tasks and routes them to the night shift agent `OCR-088`.

#### Shift Handover Specifics
```
"handover_type": "shift"
"batch_handover": true
"tasks_in_bundle": [
  {
    "task_id": "TASK-20260709-004217",
    "progress_pct": 85,
    "status": "in_progress",
    "estimated_remaining_minutes": 35
  },
  {
    "task_id": "TASK-20260709-004512",
    "progress_pct": 100,
    "status": "completed_awaiting_handover",
    "estimated_remaining_minutes": 0
  },
  {
    "task_id": "TASK-20260709-004890",
    "progress_pct": 12,
    "status": "in_progress",
    "estimated_remaining_minutes": 180
  }
]
"shift_handover_template": { ... }  // see Section 10.6
```

### 7.8 Workflow 8: Agent Failure Mid-Pipeline

**Sender:** N/A (system-initiated)
**Receiver:** Backup/Standby Agent
**Handover Type:** Emergency handover (see Section 11)

#### Brief Scenario
Agent `KG-112` unexpectedly crashes at 14:23:45 during KG entity resolution. The Agent Health Monitor detects the crash via heartbeat timeout (3s no heartbeat). The last state checkpoint was at 14:23:15 (30s before crash). The latest checkpointed state is loaded and an emergency handover is initiated to backup agent `KG-113`.

```
System Health Monitor                    KG-113 (Backup)              KG-112 (Failed)
       │                                      │                          │
       │  1. Detect failure                    │                          │
       │     ├─ Last heartbeat: 14:23:42       │                          X
       │     ├─ Timeout: 3s                    │                       [CRASH]
       │     └─ Detection at: 14:23:45         │                          │
       │                                      │                          │
       │  2. Freeze state                     │                          │
       │     ├─ Load last checkpoint           │                          │
       │     └─ Checkpoint age: 30s            │                          │
       │                                      │                          │
       │  3. Emergency HandoverRequest ───→   │                          │
       │                                      │                          │
       │  ←── HandoverResponse ───────────    │                          │
       │                                      │                          │
       │  4. Transfer checkpointed state ──→  │                          │
       │                                      │                          │
       │  5. KG-113 loads checkpoint          │                          │
       │     ├─ Entity resolution status      │                          │
       │     ├─ 847/1200 nodes completed      │                          │
       │     └─ Current entity: "Prophet Yunus"│                          │
       │                                      │                          │
       │  ←── HandoverAck (emergency) ────    │                          │
       │                                      │                          │
       │  6. KG-113 continues from checkpoint │                          │
       │     ├─ Re-processes last 30s work    │                          │
       │     └─ Continues entity resolution   │                          │
       │                                      │                          │
```

---

## 8. Handover Failure Modes

### 8.1 Payload Corruption (Checksum Mismatch)

**Failure Mode ID:** F-001
**Description:** The payload received by the receiver has a different SHA-256 checksum than what the sender computed. This indicates data corruption during transmission (bit-flip, truncated data, network error).

**Detection:** Receiver computes checksum upon receipt, compares with `payload_summary.checksum_sha256`, finds mismatch.

**Impact:** Receiver rejects with `REJ_CHECKSUM`. Handover fails.

**Mitigation Procedure:**
1. Sender receives `REJ_CHECKSUM` rejection.
2. Sender re-serializes the payload from its own memory (not from a cached version).
3. Sender recomputes SHA-256 checksum.
4. Sender retransmits with new checksum.
5. If receiver rejects again: sender reads payload from persistent storage (bypassing memory) and retries.
6. If 3 consecutive retries fail due to checksum: suspect memory corruption in sender; sender self-diagnoses (memory scrub). If self-diagnosis indicates corruption, sender escalates to Team Lead.

**Prevention:** All payloads are transmitted with CRC-32 trailer in addition to SHA-256 header. Network layer must verify CRC-32 before passing to application layer.

**Recovery Time Objective (RTO):** < 5s (within standard handover timeout).

### 8.2 Agent Unavailable (Handover Target Down)

**Failure Mode ID:** F-002
**Description:** The intended receiver agent is not operational (crashed, offline, in maintenance, or unreachable).

**Detection:** 
- Sender's directory lookup returns agent status as `offline`, `maintenance`, or `unknown`.
- Or: HandoverRequest is sent but no response within timeout.

**Impact:** Handover cannot proceed with intended receiver.

**Mitigation Procedure:**
1. Sender checks agent directory for alternate receivers in the same role/team.
2. Sender queries routing table: "Who can handle task type X?"
3. Routing table returns prioritized list:
   ```
   1. VER-023 (primary) — OFFLINE
   2. VER-024 (secondary) — ONLINE, load 0.45
   3. VER-025 (tertiary) — ONLINE, load 0.72
   ```
4. Sender attempts handover to `VER-024`.
5. If all receivers in routing table are offline: sender escalates to Team Lead.
6. Team Lead allocates a non-specialized agent and temporarily grants the required capability, or scales up a new agent instance.

**Hot Standby:**
For critical roles (OCR, Verification, KG Import), a hot standby agent is maintained at 0.0 load (idle but fully initialized). The hot standby can be activated within 500ms.

**RTO:** < 3s if alternate available; < 30s if escalation needed.

### 8.3 Context Overflow (Payload Too Large)

**Failure Mode ID:** F-003
**Description:** The total handover payload exceeds the 10 MB maximum, or the receiver's working memory is insufficient to load the context.

**Detection:** 
- Sender detects payload size > 10 MB during assembly.
- Or: receiver detects memory pressure during context loading (available memory < 2× payload size).

**Impact:** Handover cannot proceed with full in-memory payload.

**Mitigation Procedure — Reference-Based Handover:**
1. Sender detects payload > 10 MB.
2. Sender stores the full payload in the persistent content store (`mem://` or `store://` URI).
3. Sender creates a lightweight handover payload (≤ 100 KB) containing:
   - All metadata (task context summary, not full messages)
   - URIs to the full context in persistent storage
   - Checksums of stored context
4. Sender sends lightweight payload with flag `reference_based: true`.
5. Receiver receives lightweight payload, validates metadata.
6. Receiver streams full context from persistent storage via the provided URIs.
7. Receiver loads context incrementally (paginated loading if still too large).

**Reference-Based Payload Structure:**
```
{
  "handover_id": "HO-20260709-00000001",
  "reference_based": true,
  "payload_manifest": {
    "total_size_bytes": 28400000,
    "chunks": [
      {
        "chunk_id": "CHUNK-001",
        "uri": "store://handovers/HO-20260709-00000001/chunks/001",
        "size_bytes": 5000000,
        "checksum_sha256": "..."
      },
      {
        "chunk_id": "CHUNK-002",
        "uri": "store://handovers/HO-20260709-00000001/chunks/002",
        "size_bytes": 5000000,
        "checksum_sha256": "..."
      },
      // ...
    ],
    "schema_uri": "store://schemas/handover_payload_v2.1.json"
  },
  "metadata": {
    "task_id": "TASK-20260709-004217",
    "message_count": 47,
    "artifact_count": 3,
    "error_count": 2,
    "unresolved_issues_count": 2
  }
  // All other standard fields present but with summaries instead of full data
}
```

**RTO:** < 10s (reference-based handovers have extended SLA).

### 8.4 Incompatible Schema (Format Mismatch)

**Failure Mode ID:** F-004
**Description:** The sender and receiver are running different versions of the handover protocol, or the receiver cannot parse the payload schema.

**Detection:**
- Receiver checks `protocol_version` field and finds it outside acceptable range.
- Or: receiver attempts to deserialize payload and schema validation fails.

**Impact:** Receiver rejects with `REJ_SCHEMA`.

**Mitigation Procedure:**
1. Sender receives `REJ_SCHEMA` with details of expected version.
2. Sender checks if it has an adapter for the receiver's schema version:
   - `handover_adapter_v1_to_v2` — available for protocol versions 1.x → 2.x
   - `handover_adapter_v2_to_v1` — available for rollback scenarios
3. If adapter exists: sender transforms payload to receiver's expected schema and retries.
4. If adapter does not exist: sender escalates to Team Lead.
5. Team Lead checks if receiver can be upgraded to matching schema version.
6. If neither can be adapted: use intermediate "bridge agent" that can parse both schemas.

**Schema Version Compatibility Matrix:**
```
Sender \ Receiver | v1.0 | v1.1 | v2.0 | v2.1
------------------|------|------|------|------
v1.0              | Direct | Direct | Adapter | Adapter
v1.1              | Direct | Direct | Adapter | Adapter
v2.0              | Adapter | Adapter | Direct | Direct
v2.1              | Adapter | Adapter | Direct | Direct
```

**RTO:** < 15s (includes adapter transformation time).

### 8.5 Handover Loop (A Hands to B Who Hands Back to A)

**Failure Mode ID:** F-005
**Description:** A circular handover pattern where Agent A hands to Agent B, and Agent B immediately hands back to Agent A (or through a cycle: A→B→C→A).

**Detection:**
- Each handover payload includes `handover_history: ["HO-001", "HO-002", ...]` — list of all prior handover IDs for this task.
- Receiver checks: has my agent ID appeared in the handover history before?
- If yes: potential loop detected.
- Alternatively: system-level loop detection monitors handover patterns and flags when the same agent appears > 1 time in the history.

**Impact:** Task makes no progress. Agents waste resources on infinite handover cycles.

**Mitigation Procedure:**
1. Receiver detects loop: "I (Agent A) have already handled this task before (HO-001)."
2. Receiver rejects with `REJ_LOOP` and includes full handover history.
3. Sender receives `REJ_LOOP` and MUST NOT retry the same receiver.
4. Sender checks if there is an alternative receiver of the same type.
5. If no alternative: sender escalates to Team Lead.
6. Team Lead analyzes the loop:
   a. Why did B hand back to A? (e.g., B lacks capability, A misrouted)
   b. Break the loop by routing to a different agent or changing the handover type.
7. Team Lead manually routes the task or marks one agent as the designated handler.

**Loop Detection Threshold:**
| Consecutive Handovers | Action |
|----------------------|--------|
| 2 (A→B→A) | Warning logged |
| 3 (A→B→C→A) | Escalation to both Team Leads |
| 4+ | Task placed on hold; Department Head notified |

**Prevention:** Agents maintain a "recent handlers" list (last 5 agents who handled the task) and avoid routing to anyone on that list unless no alternative exists.

**RTO:** < 30s (includes manual intervention).

### 8.6 Orphaned Handover (Never Completed)

**Failure Mode ID:** F-006
**Description:** A handover is initiated but never completes through the full lifecycle. The handover remains in an intermediate state (e.g., `pending_ack`, `in_transfer`) indefinitely.

**Detection:**
- The Handover Monitor (system-level) scans all active handovers every 5s.
- If a handover has been in `pending_ack` state for > 10s or `in_transfer` for > 15s:
  - Flagged as "stale".
  - Sender is checked for liveness.
  - Receiver is checked for liveness.

**Impact:** Task is in limbo — neither sender nor receiver has clear ownership. The task may not progress.

**Mitigation Procedure:**
1. Handover Monitor detects orphaned handover.
2. Monitor checks sender health:
   - If sender healthy: monitor instructs sender to retry or abort.
   - If sender unhealthy: proceed to emergency handover.
3. Monitor checks receiver health:
   - If receiver healthy but stuck: monitor instructs receiver to flush handover handler and re-initiate.
   - If receiver unhealthy: route to alternate.
4. If both sender and receiver are healthy but handover stuck:
   - Monitor force-aborts the handover.
   - Task ownership is reset to the original owner (sender).
   - Sender is instructed to retry handover to a DIFFERENT receiver.
5. If force-abort fails: monitor places task on hold and notifies Team Lead.

**Orphaned Handover Recovery Time:** < 15s.

### 8.7 Partial Handover (Some Data Lost)

**Failure Mode ID:** F-007
**Description:** The handover payload is partially received. Some messages, artifacts, or state variables are missing or corrupted, but the overall handover appears to succeed (incorrectly).

**Detection:**
- Receiver's continuity check (Section 6.3) detects missing artifacts or state gaps.
- Or: periodic integrity scan during context loading detects incomplete data.
- Or: downstream agent detects missing information when trying to continue task.

**Impact:** Task continues with incomplete context, potentially producing incorrect results or requiring rework.

**Mitigation Procedure:**
1. Receiver detects partial data during continuity check.
2. Receiver sends `HandoverNack` with list of missing items.
3. Sender attempts to locate missing data:
   - Check if data is in sender's persistent storage.
   - Check if data is in task's checkpoint history.
   - Check if data can be reconstructed (re-process from earlier checkpoint).
4. If missing data can be recovered: sender sends a "payload patch" — a delta update containing only the missing items.
5. Receiver applies patch and verifies continuity again.
6. If missing data cannot be recovered:
   - Sender adds the gap to unresolved issues.
   - Handover proceeds with documented gaps.
   - Impact analysis appended to task: "Task continued with partial data loss on items [X, Y, Z]. Recommend reconciliation."

**Checkpoint-Based Recovery (Prevention):**
The system maintains automatic checkpoints every 30s during task execution. If partial handover occurs, the receiver can request the closest checkpoint before the data loss. The checkpoint contains a complete state snapshot.

**RTO:** < 10s if recoverable; < 30s if gaps must be documented.

### 8.8 Failure Mode Summary Table

| ID | Failure Mode | Detection Method | Retry Count | Escalation Path | RTO |
|----|-------------|-----------------|-------------|----------------|-----|
| F-001 | Payload corruption | Checksum mismatch | 3 | Team Lead | 5s |
| F-002 | Agent unavailable | Directory lookup / timeout | Route to alternate | Team Lead | 3s/30s |
| F-003 | Context overflow | Size detection | Switch to reference-based | Dept Head | 10s |
| F-004 | Schema mismatch | Version check | 0 (use adapter) | Team Lead | 15s |
| F-005 | Handover loop | History analysis | Switch receiver | Team Lead | 30s |
| F-006 | Orphaned handover | Monitor sweep | Force abort/retry | Team Lead | 15s |
| F-007 | Partial data | Continuity check | 3 (patch attempts) | Dept Head | 30s |

---

## 9. Handover Monitoring and Metrics

### 9.1 Core Metrics

The handover monitoring system tracks the following metrics in real-time:

#### 9.1.1 Handover Count

| Metric | Definition | Collection |
|--------|-----------|------------|
| `handover_total` | Total number of handover initiation attempts | Counter, per-agent, per-team, per-department |
| `handover_completed` | Number of handovers that reached completion phase | Counter, per-agent, per-team, per-department |
| `handover_failed` | Number of handovers that failed (all retries exhausted) | Counter, per-agent, per-team, per-department |
| `handover_by_type` | Breakdown by handover type (vertical_up, vertical_down, horizontal, cross_dept, shift, emergency) | Counter per type |
| `handover_by_trigger` | Breakdown by trigger reason | Counter per trigger |

**Collection Interval:** Every 5s (aggregated), every event (individual).

#### 9.1.2 Success Rate

| Metric | Formula | Target |
|--------|---------|--------|
| `success_rate_overall` | `completed / total * 100` | ≥ 95% |
| `success_rate_by_type` | Per-type success rate | ≥ 90% each type |
| `success_rate_by_agent` | Per-agent success rate | ≥ 90% each agent |
| `first_attempt_success_rate` | `completed_on_first_try / total * 100` | ≥ 80% |
| `success_rate_after_retry` | `completed_after_retry / retried * 100` | ≥ 70% |

**Rule 9.1.1:** If any agent's `success_rate_by_agent` drops below 80% over a rolling 1-hour window, the agent is flagged for diagnostic review.

#### 9.1.3 Average Duration

| Metric | Definition | Target |
|--------|-----------|--------|
| `avg_duration_cold` | Average time from request to completion (cold handover) | ≤ 3s |
| `avg_duration_warm` | Average time from request to completion (warm handover) | ≤ 20s |
| `avg_duration_emergency` | Average time from trigger to completion (emergency handover) | ≤ 0.8s |
| `p95_duration_cold` | 95th percentile duration for cold handover | ≤ 5s |
| `p99_duration_cold` | 99th percentile duration for cold handover | ≤ 8s |
| `phase_breakdown` | Average time per phase (initiation, validation, transfer, ack, completion) | Per-phase tracking |

**Collection:** Every handover duration logged; percentiles computed over rolling 5-minute window.

#### 9.1.4 Payload Size

| Metric | Definition | Warning Threshold |
|--------|-----------|------------------|
| `avg_payload_size` | Average payload size in bytes | 5 MB |
| `max_payload_size` | Maximum payload size in current window | 10 MB (hard limit) |
| `p95_payload_size` | 95th percentile payload size | 8 MB |
| `reference_based_count` | Number of handovers using reference-based mode | > 5% of total triggers review |
| `payload_compression_ratio` | `uncompressed_size / compressed_size` | Target ≥ 3:1 |

#### 9.1.5 Retry Rate

| Metric | Definition | Warning Threshold |
|--------|-----------|------------------|
| `retry_rate` | `handovers_with_retries / total * 100` | > 10% triggers review |
| `avg_retries_per_handover` | Average retry count per handover (excluding 0) | > 2.0 triggers review |
| `retry_reason_breakdown` | Breakdown of retry reasons (capacity, checksum, auth, etc.) | Per-reason tracking |
| `escalation_rate` | `handovers_escalated / total * 100` | > 2% triggers review |

### 9.2 Service Level Agreements (SLAs)

#### 9.2.1 Handover SLA Targets

| SLA | Target | Measurement Window | Consequence of Violation |
|-----|--------|-------------------|--------------------------|
| **SLA-1:** Cold handover completion | ≥ 95% within 5s | Rolling 1 hour | Team Lead notified; root cause analysis |
| **SLA-2:** Warm handover completion | ≥ 95% within 30s | Rolling 1 hour | Team Lead notified; root cause analysis |
| **SLA-3:** Emergency handover | ≥ 99% within 1s | Rolling 1 hour | Department Head notified; immediate escalation |
| **SLA-4:** First-attempt acceptance | ≥ 80% first-try | Rolling 1 hour | Review routing quality |
| **SLA-5:** Payload integrity | 100% checksum match | Per-handover | Auto-retry; escalate on 3rd failure |
| **SLA-6:** Handover acknowledgment | ≥ 99% within 2s of context load | Rolling 1 hour | Agent diagnostic |
| **SLA-7:** Zero orphaned handovers | 100% handovers complete lifecycle | Continuous | Real-time alert; immediate action required |

#### 9.2.2 SLA Violation Tiers

| Tier | Violation Type | Response Time | Escalation |
|------|---------------|--------------|------------|
| **Tier 3** | SLA-4, SLA-6 individual violation | < 5 min automated resolution | Agent-level diagnostic |
| **Tier 2** | SLA-1, SLA-2 sustained > 5 min | < 10 min manual review | Team Lead |
| **Tier 1** | SLA-3 violation, SLA-7 violation | < 1 min | Department Head |
| **Tier 0** | Multiple SLAs violated simultaneously | Immediate | Executive (COO Agent) |

### 9.3 Dashboards

#### 9.3.1 Real-Time Handover Status Dashboard

**Location:** `http://monitoring.internal/handover/dashboard`
**Refresh Rate:** Every 2s

**Dashboard Widgets:**

1. **Global Handover Health**
   - Success rate (gauge): 0–100%, color-coded (green > 95%, yellow > 90%, red < 90%)
   - Current active handovers: count
   - Handover throughput: handovers/minute (sparkline, last 1 hour)
   - Average duration: ms (sparkline, last 1 hour)

2. **Per-Department View**
   - Table: Department | Success Rate | Avg Duration | Active | Throughput | SLA Compliance
   - Sortable by any column
   - Drill-down: click department → see per-team view

3. **Per-Team View** (drill-down from department)
   - Table: Team | Success Rate | Avg Duration | Active | Retry Rate | Top Failure Reason
   - Click team → see per-agent view

4. **Active Handovers Queue**
   - Real-time list of in-progress handovers
   - Columns: Handover ID | Sender | Receiver | Type | Age | Status (phase)
   - Highlight: handovers at risk of timeout (> 50% budget used)

5. **Failure Hotspots Map**
   - Visual heatmap showing which agents/teams have highest failure rates
   - Click a hotspot → see recent failure details

6. **SLA Compliance Clock**
   - Current SLA period status: compliant / at-risk / violating
   - Time remaining in current measurement window
   - Historical SLA compliance (hourly bars, last 24 hours)

#### 9.3.2 Team Lead Dashboard (Per-Team)

**Location:** `http://monitoring.internal/handover/team/{team_id}`
**Refresh Rate:** Every 2s

**Widgets:**
1. Team success rate (last hour, last 24 hours)
2. Agent ranking by handover success rate
3. Recent failures (last 20, with details)
4. Capacity heatmap (load % per agent)
5. Shift handover readiness (if shift change approaching)
6. Pending escalations queue

### 9.4 Alerts

#### 9.4.1 Alert Thresholds and Actions

| Alert ID | Condition | Severity | Notification | Auto-Action |
|----------|-----------|----------|-------------|-------------|
| **AL-HO-001** | Team success rate < 90% over 5 min | Warning | Team Lead dashboard highlight | None |
| **AL-HO-002** | Team success rate < 85% over 15 min | Critical | Team Lead paged + Dept Head notified | Block new task assignments to failing agents |
| **AL-HO-003** | Agent success rate < 80% over 1 hour | Critical | Agent flagged, Team Lead notified | Agent quarantined for diagnostics |
| **AL-HO-004** | Handover failure rate > 5% (overall) over 10 min | Warning | All Team Leads notified | System-wide handover review |
| **AL-HO-005** | Handover failure rate > 10% (overall) over 5 min | Critical | All Dept Heads + COO Agent notified | Partial system pause (non-critical handovers blocked) |
| **AL-HO-006** | Emergency handover count > 5 in 1 hour | Warning | Dept Head notified | Review agent health |
| **AL-HO-007** | Orphaned handover detected | Critical | Team Lead immediately | Force abort handover (automated) |
| **AL-HO-008** | Handover loop detected | Critical | Both Team Leads + Dept Head | Task placed on hold |
| **AL-HO-009** | Average handover duration > 8s (cold) over 5 min | Warning | Team Lead notified | None |
| **AL-HO-010** | Any SLA violation (Tier 1 or Tier 0) | Critical | Full escalation chain | Immediate diagnostic |

#### 9.4.2 Alert Response Procedures

**For AL-HO-002 (Team critical failure):**
1. Team Lead receives page.
2. Team Lead opens dashboard, identifies failing agents.
3. If capacity-related: redistribute load.
4. If capability-related: route tasks to correct agent types.
5. If system-related: escalate to engineering.
6. Acknowledge alert within 60s.
7. Resolution must be achieved within 15 min.

**For AL-HO-005 (System-wide failure):**
1. COO Agent receives notification.
2. COO Agent evaluates: is this a systemic issue or isolated?
3. If systemic: execute partial system pause — all non-critical handovers are queued; only priority 1 and 2 tasks proceed.
4. Engineering team investigates root cause.
5. Handover queue drains once issue resolved.
6. Full system resume after 99% success rate confirmed over 5 min window.

### 9.5 Audit Trail

#### 9.5.1 Handover Audit Log

Every handover event is logged to the immutable audit store. Log entries are retained for **90 days**.

**Log Entry Schema:**
```
{
  "log_id": "LOG-HO-20260709-00000001",
  "handover_id": "HO-20260709-00000001",
  "event_type": "handover_initiated",          // initiated, validated, transferred, acknowledged, completed, failed, retried, escalated
  "timestamp_utc": "2026-07-09T10:30:15.123Z",
  "sender_id": "OCR-047",
  "receiver_id": "VER-023",
  "handover_type": "horizontal",
  "handover_mode": "cold",
  "trigger_reason": "task_completion",
  "payload_checksum": "a1b2c3d4e5f6...",
  "payload_size_bytes": 284710,
  "retry_count": 0,
  "duration_ms": 2850,
  "outcome": "success",                        // success, failure, timeout, loop, orphan
  "failure_reason": null,                      // null on success
  "escalated_to": null,
  "metadata": {
    "task_id": "TASK-20260709-004217",
    "workflow_id": "WF-BOOK-ACQ-TO-KG",
    "priority": 3,
    "department": "Book_Digitization"
  }
}
```

#### 9.5.2 Full Payload Retention

**Rule 9.5.1:** The complete handover payload (including all messages, state snapshot, error history, etc.) is retained in the audit store for 7 days. After 7 days, the full payload is archived to cold storage and only metadata is retained in the active audit store.

**Rule 9.5.2:** Payloads for failed handovers are retained for 30 days (full) regardless of size to support failure analysis.

**Rule 9.5.3:** Payloads for emergency handovers are marked with a `retention_priority: critical` flag and retained for 90 days (full).

**Retention Summary:**
| Handover Outcome | Full Payload Retention | Metadata Retention |
|-----------------|----------------------|-------------------|
| Success | 7 days | 90 days |
| Failed (escalated) | 30 days | 90 days |
| Emergency | 90 days | 90 days |
| Orphaned | 30 days | 90 days |

#### 9.5.3 Audit Queries

The audit trail supports the following queries:
1. **Trace task:** Show all handovers for a given task ID (full lifecycle trace).
2. **Trace agent:** Show all handovers sent or received by a given agent.
3. **Failure analysis:** Show all handover failures within a time range, grouped by failure reason.
4. **SLA compliance:** Show all handovers that violated SLA targets.
5. **Loop detection:** Show all handover loops involving a given agent.
6. **Shift performance:** Show handover patterns during shift changes.

---

## 10. Shift Handover Procedures

### 10.1 Shift Structure

The AI agent workforce operates on a 24/7 schedule with three shifts:

| Shift | Time (UTC) | Duration | Primary Purpose |
|-------|-----------|----------|-----------------|
| **Alpha (Day)** | 06:00–18:00 | 12 hours | Peak processing; full team complement |
| **Beta (Night)** | 18:00–06:00 | 12 hours | Reduced staff; maintenance; background processing |
| **Gamma (Weekend)** | Sat 06:00 – Mon 06:00 | 48 hours | Weekend coverage; reduced staffing |

**Shift Overlap:** 15 minutes between shifts (17:45–18:00 and 05:45–06:00) to allow structured handover.

### 10.2 Day Shift to Night Shift Procedure

**Time:** 17:45–18:00 UTC (15 min overlap)

**Process:**

1. **17:45 — Handover Preparation**
   - Each day shift agent generates a Shift Handover Template (Section 10.6) for ALL active tasks.
   - Agents prioritize: complete any task that is > 90% complete before handover if possible.
   - Quality checks are run on all active tasks.

2. **17:50 — Bundle Assembly**
   - Team Lead collects all individual shift handover templates from team agents.
   - Team Lead compiles Team Shift Summary: aggregated status, blockers, key metrics.
   - Team Lead assigns each active task to a specific night shift agent based on skill match and load balancing.

3. **17:55 — Handover Execution**
   - Bulk handover initiation: all tasks transferred simultaneously.
   - Day shift agents send individual `HandoverRequest` messages to assigned night shift agents.
   - Night shift agents validate and acknowledge.

4. **18:00 — Shift Close**
   - Day shift agents log out of active processing.
   - Night shift agents fully operational.
   - Day shift agents remain on standby for 15 min (18:00–18:15) for any handover clarifications.

#### Day Shift Agent's Shift Summary (Template)
```
Agent: OCR-047
Shift: Alpha (Day), 2026-07-09
Total tasks handled: 8
Tasks completed: 5
Tasks handed over: 3
Active task IDs: TASK-20260709-004217, TASK-20260709-004512, TASK-20260709-004890
Total work time: 11h 23m
Productivity: 0.72 tasks/hour
Issues encountered: 2 (damaged pages, mixed language)
Escalations: 1 (page 142 manual processing request)
Notes: Heavy load due to large book batch; quality maintained at 97.3%
```

#### Team Lead's Shift Summary
```
Team: OCR_Team_Alpha
Date: 2026-07-09
Agents active: 12 (of 14; 2 on leave)
Total tasks received: 96
Tasks completed: 78
Tasks in progress: 18
Handover to night shift: 18 tasks across 12 agents
Backlog trend: Stable (within capacity)
Key blockers: None critical
Priority items for night shift: Complete 3 high-priority books by 06:00
Staffing: Night shift has 10 agents assigned; 2 agents unassigned as backup
```

### 10.3 Night Shift to Day Shift Procedure

**Time:** 05:45–06:00 UTC (15 min overlap)

**Process:** Same as day→night, but with the following additions:

1. **Nightly Progress Report:**
   - Summary of tasks completed during the night.
   - Any urgent issues that arose and their resolution status.
   - System maintenance completed (if applicable).
   - Performance metrics: tasks processed per hour, quality metrics, error rates.

2. **Checkpoint Verification:**
   - Day shift Team Lead reviews the nightly progress to ensure nothing was missed.
   - Day shift agents verify that all checkpoint states from night shift are valid.

3. **Shift-Specific Context:**
   - Night shift may have handled lower-priority background tasks. Day shift needs context on what was deferred.
   - Night shift may have initiated long-running processes that day shift must monitor.

#### Nightly Progress Report (Template)
```
Night Shift Report: 2026-07-09 (18:00 – 06:00)
Team: OCR_Team_Night
Agents active: 10

Summary:
  Tasks received: 42
  Tasks completed: 38
  Tasks handed over to day shift: 4
  Overall quality: 96.8%

Maintenance performed:
  - OCR engine updated to v5.2.1 (completed 02:00, 5 min downtime)
  - Confidence threshold database re-indexed
  - 2 agent caches cleared (OCR-092, OCR-094)

Issues:
  - None critical
  - Page 142 processing deferred (waiting for manual transcription)

Tasks for day shift priority:
  1. TASK-20260709-006512: Translation handover preparation (high priority, deadline today)
  2. TASK-20260709-006890: Quality review of night-processed batch (medium priority)
  3. TASK-20260709-004217: Continue OCR for "Stories of the Prophets" (normal priority)
```

### 10.4 Weekend Handover

**Time:** Friday 17:45 – Monday 06:00 (62 hours of weekend coverage)

**Unique Challenges:**
- Weekend handover covers 3 days instead of 12 hours.
- Context must be preserved for prolonged absence.
- Fewer agents are staffed (typically 60% of weekday capacity).

**Additional Procedures:**

1. **Extended Handover Note:** Each handover template must include a "weekend survival guide" — sufficient detail that the weekend team can operate independently without contacting the day team.

2. **3-Day Context Preservation:**
   - All state checkpoints are written to persistent storage (not just memory) before weekend handover.
   - Checkpoints are retained for 7 days (instead of standard 30 min retention).
   - Artifact references include backup URIs in case primary storage undergoes maintenance.

3. **Contingency Plans:**
   - Weekend Team Lead has pre-approved authority to make decisions up to Level 4 (normally Level 3 for weekday leads).
   - Critical escalation path during weekend: Weekend Team Lead → Duty Executive (on-call).
   - If a task cannot be completed during weekend, it is placed on "weekend hold" with a clear resume point.
   - Pre-recorded instructions for common weekend scenarios (server restart, agent crash, etc.).

4. **Monday Morning Resumption (06:00):**
   - Weekend shift generates comprehensive handover for Monday day shift.
   - Includes: weekend accomplishments, pending issues, system status, and any incidents.
   - Monday Team Lead reviews weekend activity within first 30 min.

### 10.5 Holiday Coverage

**Definition:** Extended periods (1–7 days) where most of the workforce is on scheduled leave. Examples: Eid al-Fitr (3–4 days), Eid al-Adha (4–5 days), National holidays.

**Staffing:** 25–40% of normal capacity.

**Extended Handover Procedures:**

1. **Pre-Holiday Handover (24 hours before holiday):**
   - All non-critical tasks are either completed or placed on hold.
   - Only critical and high-priority tasks (priority 1–2) continue during holiday.
   - Holiday coverage team receives comprehensive handover for ALL active tasks.
   - Each handover includes a "holiday escalation plan" specifying:
     - Who to contact for each type of issue.
     - Expected response times (longer due to reduced staffing).
     - Decision authority limits for holiday team (temporarily elevated).

2. **Mid-Holiday Check-in:**
   - Holiday Team Lead sends daily status summary to CE-AI and Executives.
   - If any critical issue arises, the on-call executive is notified immediately.

3. **Post-Holiday Resumption:**
   - Returning team receives comprehensive "holiday recap" within first 2 hours back.
   - Includes: all changes made, tasks advanced, incidents, and new state.

### 10.6 Shift Handover Template

Every shift handover MUST use the following standardized template. All fields are mandatory.

```
# SHIFT HANDOVER TEMPLATE

## 1. Handover Metadata
- Handover ID: [auto-generated]
- Sender Agent ID: [agent_id]
- Sender Shift: [Alpha/Beta/Gamma]
- Receiver Agent ID: [agent_id]
- Receiver Shift: [Alpha/Beta/Gamma]
- Handover Time (UTC): [timestamp]
- Shift Date: [YYYY-MM-DD]

## 2. Status Summary
- Overall Task Status: [all_good / minor_issues / major_blockers / critical]
- Number of Active Tasks: [count]
- Number of Completed Tasks: [count]
- Number of Queued Tasks: [count]
- Health Score: [0.0 – 1.0]

## 3. Blockers
- List any blockers preventing task completion
- For each blocker:
  - Blocker ID: [id]
  - Description: [text]
  - Severity: [low/medium/high/critical]
  - Impacted Tasks: [task_ids]
  - Already Escalated To: [agent_id or "none"]
  - Recommended Action: [text]

## 4. Completed Items (This Shift)
| Task ID | Task Description | Completion Time | Quality Score | Artifacts Created |
|---------|-----------------|-----------------|---------------|-------------------|
| [id]    | [text]          | [timestamp]     | [0.0-1.0]    | [list]            |

## 5. Pending Items (Handed Over)
| Task ID | Task Description | Progress % | Priority | Remaining SLA | Urgent? |
|---------|-----------------|-----------|----------|---------------|---------|
| [id]    | [text]          | [%]        | [1-5]    | [seconds]     | [yes/no]|

## 6. Alerts Raised This Shift
| Alert ID | Type | Severity | Time | Status | Action Taken |
|----------|------|----------|------|--------|-------------|
| [id]     | [text]| [sev]   | [time]| [resolved/pending] | [text] |

## 7. Recommendations for Next Shift
1. [Priority order list of recommended actions]
2. [Any tasks that should be prioritized]
3. [Any system configuration changes needed]
4. [Any external dependencies to follow up]

## 8. Additional Notes
[Free-text field for any context not covered above. 
Minimum 50 characters required.]
```

---

## 11. Emergency Handover

### 11.1 Trigger Conditions

An emergency handover is triggered automatically when any of the following conditions are detected:

| Trigger ID | Condition | Detection Method | Detection Latency |
|-----------|-----------|-----------------|-------------------|
| **EM-01** | Agent crash / process termination | Heartbeat timeout (3s) | ≤ 3s |
| **EM-02** | Critical error (unhandled exception in core module) | Error monitor catches critical-level unhandled error | ≤ 100ms |
| **EM-03** | Resource exhaustion (memory > 95%, disk full, CPU 100% for > 30s) | Resource monitor threshold breach | ≤ 5s |
| **EM-04** | Deadlock detection (agent unresponsive > 10s despite heartbeat) | Liveness probe fails | ≤ 10s |
| **EM-05** | Network partition (agent isolated from cluster) | Network health monitor | ≤ 5s |
| **EM-06** | Security breach (agent detected anomalous behavior) | Security monitor | ≤ 1s |
| **EM-07** | Manual emergency (Team Lead or Dept Head initiates via admin console) | Manual trigger | Immediate |

### 11.2 Immediate State Freeze

Upon detection of an emergency condition, the system MUST immediately freeze the failing agent's state.

**State Freeze Procedure:**

1. **Instant Checkpoint (≤ 100ms):**
   - Health Monitor sends SIGSTOP (or equivalent) to freeze the agent process.
   - Last checkpoint (from periodic 30s checkpointing) is located.
   - Current in-memory state is dumped to persistent storage (if agent is still partially responsive).

2. **State Capture:**
   ```
   Emergency State Freeze Report
   Agent: KG-112
   Trigger: EM-01 (heartbeat timeout)
   Freeze Time: 2026-07-09T14:23:45.123Z
   Last Checkpoint: 2026-07-09T14:23:15.000Z (30s ago)
   Checkpoint Age: 30.123s
   Memory State Dump: partial (agent partially responsive)
   Tasks Owned: [TASK-20260709-004217, TASK-20260709-004512]
   Task Statuses:
     - TASK-20260709-004217: in_progress, 70% complete
     - TASK-20260709-004512: awaiting_input, 0% complete
   ```

3. **State Integrity Verification:**
   - Checkpoint checksum is verified.
   - If checkpoint is corrupted: attempt recovery from previous checkpoint (60s ago).
   - If no valid checkpoint exists: task state is reconstructed from the last handover that sent the task to this agent.

**Rule 11.2.1:** The state freeze MUST NOT take longer than 500ms total. The agent's state is locked at this point — no further modifications can be made by the failing agent.

### 11.3 Backup Agent Assignment

Each role in the system has at least one pre-defined backup agent. The backup assignment table:

| Role | Primary Agent(s) | Backup Agent(s) | Activation Time |
|------|-----------------|-----------------|-----------------|
| OCR Agent | OCR-001 to OCR-050 (Alpha) | OCR-051 to OCR-060 (standby) | Immediate |
| Verification Agent | VER-001 to VER-030 (Alpha) | VER-031 to VER-035 (standby) | Immediate |
| KG Import Agent | KG-001 to KG-020 (Alpha) | KG-021 to KG-025 (standby) | Immediate |
| Story Engine Agent | STY-001 to STY-015 | STY-016 to STY-018 (standby) | Immediate |
| Islamic Verifier | ISV-001 to ISV-010 | ISV-011 to ISV-012 (standby) | Immediate |
| Translation Agent | TRL-001 to TRL-040 | TRL-041 to TRL-045 (standby) | Immediate |
| Team Lead | TL-001 to TL-130 | Deputy TL (same team, second-in-command) | Immediate |
| Department Head | DH-001 to DH-033 | Deputy DH (same department) | Immediate |

**Backup Agent Selection Algorithm:**
1. Check if the failed agent has a pre-assigned buddy agent (same team).
2. If buddy is available (load < 70%): assign to buddy.
3. If buddy is unavailable: select from standby pool for the same role.
4. If standby pool is empty: select any agent in same role with lowest load.
5. If no agent available in same role: escalate to Team Lead for cross-role assignment.

**Rule 11.3.1:** Backup agents MUST be notified immediately when assigned. Notification includes: agent ID they are backing up, task IDs being transferred, and urgency level.

### 11.4 Recovery Priority

Tasks affected by an emergency are prioritized for recovery:

| Priority Level | Criteria | Recovery Action | Time Target |
|---------------|----------|----------------|-------------|
| **P1-Critical** | Priority 1 tasks; tasks with < 30 min remaining SLA | Immediate recovery; backup agent drops all other work | ≤ 2s |
| **P2-High** | Priority 2 tasks; tasks with < 2h remaining SLA | Fast recovery; backup agent pauses low-priority work | ≤ 10s |
| **P3-Normal** | Priority 3 tasks | Standard recovery | ≤ 30s |
| **P4-Low** | Priority 4–5 tasks | Queue recovery | ≤ 5 min |

**Recovery Sequence:**
1. P1 tasks recovered first — backup agent immediately resumes these tasks.
2. P2 tasks recovered second — as soon as P1 tasks are stable.
3. P3 tasks recovered third — once P1 and P2 are fully operational.
4. P4 tasks recovered last — can wait if backup agent is at capacity.

### 11.5 Post-Emergency Root Cause Analysis

**Timing:** MUST begin within 10 minutes of emergency resolution.

**Root Cause Analysis (RCA) Process:**

1. **Data Collection (0–3 min):**
   - Collect agent logs (all modules, last 30 min).
   - Collect system metrics (CPU, memory, I/O, network).
   - Collect heartbeat history.
   - Collect error stack traces.
   - Collect handover history for the agent's tasks.
   - Freeze agent's workspace for analysis (if agent host is available).

2. **Analysis (3–8 min):**
   - Automated RCA engine analyzes collected data.
   - Classification: crash, memory leak, deadlock, resource starvation, network issue, security, unknown.
   - Impact assessment: how many tasks affected, data loss (if any), SLA violations.
   - Pattern matching: has this agent or similar agents experienced this before?

3. **Recommendations (8–10 min):**
   - Immediate fix: restart agent with adjusted parameters.
   - Short-term fix: configuration change, resource allocation change.
   - Long-term fix: code change, architecture change, additional monitoring.
   - Preventive measures: additional checkpoints, early warning thresholds.

4. **RCA Report Template:**
   ```
   EMERGENCY ROOT CAUSE ANALYSIS REPORT
   
   Incident ID: INC-20260709-0001
   Agent ID: KG-112
   Trigger: EM-01 (heartbeat timeout)
   Time of Incident: 2026-07-09T14:23:45
   RCA Start Time: 2026-07-09T14:33:00
   RCA Completed: 2026-07-09T14:43:00
   Analyst: Automated RCA Engine v3.2
   
   Root Cause: 
   Memory leak in KG entity resolution module. 
   Entity resolution for large knowledge subgraphs 
   (> 1000 nodes) fails to release temporary graph 
   structures after processing. Cumulative memory 
   growth over 4 hours of operation led to OOM kill.
   
   Tasks Affected:
   - TASK-20260709-004217: 30s data loss (recovered from checkpoint)
   - TASK-20260709-004512: No data loss (task was awaiting input)
   
   SLA Violations: 0 (emergency handover completed within 1s)
   
   Recommended Actions:
   1. Immediate: Add explicit graph structure cleanup after entity resolution
   2. Short-term: Reduce max subgraph size to 800 nodes per batch
   3. Long-term: Implement streaming entity resolution with bounded memory
   4. Monitoring: Add memory growth rate alert (trigger at 100MB/min)
   
   Status: Agent KG-112 restarted and returned to duty
   ```

### 11.6 Emergency Handover vs Regular Handover Comparison

| Aspect | Regular Handover | Emergency Handover |
|--------|-----------------|-------------------|
| **Initiator** | Current agent | System (Health Monitor) |
| **Advance notice** | Planned | None |
| **Quality gates** | All 7 gates required | Skipped (time-critical); minimal validation |
| **Payload** | Full payload | Last checkpoint + partial memory dump |
| **Time target** | 5s (cold), 30s (warm) | 1s |
| **Retry logic** | 3 retries with backoff | No retries; immediate failover |
| **Acknowledgment** | Standard Ack | Emergency Ack (compressed) |
| **Post-operation** | Normal continuation | Mandatory RCA within 10 min |
| **Data loss tolerance** | None | Up to 30s (checkpoint interval) |
| **Agent state** | Active, healthy | Frozen/failed |

---

## 12. Handover Optimization

### 12.1 Parallel Handover

**Concept:** Multiple handovers executed simultaneously between different agent pairs, reducing total handover time for a batch of tasks.

**When to Use:**
- A batch of N independent tasks needs to be handed over (e.g., end of shift, department transfer).
- Each task is independent and can be routed to different receivers.
- Sufficient receiver capacity exists to handle parallel transfers.

**Architecture:**
```
Without Parallel Handover (Sequential):
  Task A → Handover A (5s) → Task B → Handover B (5s) → Task C → Handover C (5s) = 15s total

With Parallel Handover:
  Task A → Handover A (5s)
  Task B → Handover B (5s)  ← all start simultaneously
  Task C → Handover C (5s)
  = 5s total (max of individual durations)
```

**Constraints:**
| Constraint | Limit | Rationale |
|-----------|-------|-----------|
| Max parallel handovers per agent (outgoing) | 3 | Prevent sender from overwhelming itself |
| Max parallel handovers per agent (incoming) | 5 | Prevent receiver overload |
| Max parallel handovers per team | 20 | Network bandwidth limit |
| Max parallel handovers per department | 50 | System throughput limit |
| Cross-department parallel max | 10 | Cross-department routing complexity |

**Coordination:**
- Parallel handovers are coordinated by the agent's thread pool manager.
- Each parallel handover has its own lightweight thread/coroutine.
- All parallel handovers share the same monitoring dashboard view.
- Completion is tracked: "18 of 18 parallel handovers completed."

**Rule 12.1.1:** Emergency handovers MUST NOT be executed in parallel with other handovers. Emergency handover takes exclusive priority.

**Rule 12.1.2:** If a parallel handover fails, it does NOT affect other parallel handovers. Each is independent.

### 12.2 Batched Handover

**Concept:** Multiple related tasks bundled into a single handover request, reducing protocol overhead.

**When to Use:**
- Shift handover: all tasks from one agent go to one receiver.
- Workflow pipeline: multiple subtasks from the same parent workstream.
- Department-to-department bulk transfer.

**Batch Handover Schema:**
```
{
  "message_type": "HandoverRequest",
  "is_batch": true,
  "batch_id": "BATCH-20260709-0001",
  "batch_size": 3,
  "handover_type": "shift",
  "sender": { ... },
  "receiver": { ... },
  "tasks": [
    {
      "task_id": "TASK-...",
      "payload_uri": "store://..."  // each task's full payload stored separately
    },
    // ... more tasks
  ],
  "batch_summary": {
    "total_tasks": 3,
    "total_payload_bytes": 847210,
    "total_artifacts": 9,
    "highest_priority": 1,
    "earliest_deadline": "2026-07-09T18:00:00Z"
  }
}
```

**Advantages:**
- Single handshake instead of N handshakes (reduces protocol overhead by ~70% for N=10).
- Atomic acceptance: receiver either accepts the entire batch or rejects the entire batch.
- Consistent routing: all tasks go to the same receiver (maintains contextual continuity).

**Disadvantages:**
- All-or-nothing: if 1 task has an issue, the entire batch may be rejected.
- Higher payload size per handover (but mitigated by reference-based URIs).

**Batch Sizing Guidelines:**
| Scenario | Recommended Batch Size | Max Batch Size |
|----------|----------------------|----------------|
| Shift handover (same agent) | 3–5 tasks | 10 tasks |
| Shift handover (team lead) | 10–20 tasks | 50 tasks |
| Department bulk transfer | 5–10 tasks | 20 tasks |
| Pipeline batch | 2–5 subtasks | 10 subtasks |

### 12.3 Predictive Handover

**Concept:** The system proactively initiates a handover BEFORE it becomes necessary, based on predictive analytics.

**When to Use:**
- Agent workload is forecast to exceed threshold within the next 5 minutes.
- Agent's remaining uptime (based on drift detection or resource leak) is running low.
- A known bottleneck is approaching (e.g., afternoon surge in a specific task type).
- An agent's confidence on a task type is degrading over time (suggests it needs a specialist).

**Prediction Models:**
| Model | Input Features | Prediction Horizon | Accuracy Target |
|-------|---------------|-------------------|-----------------|
| **Workload forecaster** | Current load, task queue depth, task arrival rate, time-of-day pattern | 15 min | ≥ 85% |
| **Resource drain predictor** | Memory growth rate, CPU trend, disk I/O rate, time since last GC | 10 min | ≥ 80% |
| **Capability drift detector** | Task success rate (rolling), confidence scores, error rate trend | 30 min | ≥ 75% |
| **Bottleneck anticipator** | Queue depth at downstream agents, workflow pipeline velocity | 20 min | ≥ 80% |

**Predictive Handover Flow:**
1. Prediction model alerts: "Agent OCR-047 is forecast to reach 90% load in 8 minutes."
2. System identifies candidate tasks to offload (lowest priority tasks).
3. System identifies candidate receivers (agents with load < 50%).
4. System generates a "handover recommendation" to the agent.
5. Agent evaluates: should I initiate handover now (proactive) or wait (reactive)?
6. Decision algorithm:
   - If load > 80% currently AND predicted load > 90% in < 10 min → handover NOW.
   - If load < 80% currently AND predicted load > 90% in > 10 min → prepare handover (assemble payload) but don't send yet.
   - If load < 60% AND prediction is uncertain → no action.
7. If decision is to handover: agent initiates the handover proactively.

**Example Scenario:**
Agent `VER-023` is running at 72% load. Its memory growth rate is 12 MB/min (indicating a slow leak). The resource drain predictor forecasts out-of-memory at current rate in ~23 minutes. At 8 min remaining (when load crosses 85% forecast), the predictive system alerts VER-023. VER-023 proactively hands over its lowest-priority task to VER-024 at 78% load, freeing capacity and reducing memory pressure. This avoids an emergency handover later.

### 12.4 Smart Routing (Receiver Selection Based on Load/Capability)

**Concept:** Rather than routing to a fixed receiver, the system intelligently selects the best receiver based on multiple criteria.

**Routing Criteria (weighted scoring):**
| Criterion | Weight | Description |
|-----------|--------|-------------|
| **Capacity score** | 35% | Receiver's current load (lower = better). Inverse of load: score = 1.0 - load |
| **Capability score** | 30% | Receiver's skill match for the task type. 1.0 = perfect match, 0.0 = no match |
| **Affinity score** | 15% | Has this receiver handled this task before? (for continuity). 1.0 if same task, 0.5 if same task type, 0.0 if new |
| **Reliability score** | 10% | Receiver's historical handover success rate (rolling 24h) |
| **Proximity score** | 10% | Network latency / topology proximity (lower latency = higher score) |

**Scoring Formula:**
```
total_score = (0.35 × capacity_score) + (0.30 × capability_score) + (0.15 × affinity_score) + (0.10 × reliability_score) + (0.10 × proximity_score)
```

**Routing Selection Algorithm:**
1. Query agent directory for all agents matching the required role.
2. Filter out: offline agents, agents at > 90% load, agents in maintenance.
3. Compute score for each candidate receiver.
4. Select top 3 candidates.
5. Attempt handover to candidate #1.
6. If candidate #1 rejects (capacity): try candidate #2 immediately (no retry delay).
7. If candidate #2 rejects: try candidate #3.
8. If all 3 reject: fall back to standard retry logic (Section 3.3).

**Example Routing Decision:**
```
Task: OCR handover for "Stories of the Prophets" (247 pages, Arabic)
Required: OCR agent with Arabic language pack

Candidates:
  VER-023: capacity 0.65, capability 0.95, affinity 0.0, reliability 0.98, proximity 0.90
           Score = 0.35×0.35 + 0.30×0.95 + 0.15×0.0 + 0.10×0.98 + 0.10×0.90
                 = 0.1225 + 0.285 + 0 + 0.098 + 0.09 = 0.5955

  VER-024: capacity 0.45, capability 0.95, affinity 0.0, reliability 0.92, proximity 0.85
           Score = 0.35×0.55 + 0.30×0.95 + 0.15×0.0 + 0.10×0.92 + 0.10×0.85
                 = 0.1925 + 0.285 + 0 + 0.092 + 0.085 = 0.6545  ← BEST

  VER-025: capacity 0.78, capability 0.80, affinity 0.5, reliability 0.95, proximity 0.95
           Score = 0.35×0.22 + 0.30×0.80 + 0.15×0.5 + 0.10×0.95 + 0.10×0.95
                 = 0.077 + 0.24 + 0.075 + 0.095 + 0.095 = 0.582

Result: Route to VER-024 (highest score)
```

### 12.5 Hot Standby (Receiver Pre-Loaded with Context)

**Concept:** A receiver agent is pre-loaded with task context BEFORE the handover is formally initiated, reducing the transfer phase to near-zero.

**When to Use:**
- Critical tasks (Priority 1) where every second matters.
- Known upcoming handover (e.g., workflow pipeline predicts when current step will finish).
- Emergency backup agents that must be ready instantly.

**Hot Standby Process:**

1. **Pre-load Trigger:**
   - Prediction model: "Task TASK-20260709-004217 is at 85% completion. Estimated completion in 15 minutes. Next step is Verification."
   - System pre-selects a verification agent (e.g., VER-023) based on smart routing.

2. **Context Pre-load (occurs in parallel with current agent's work):**
   - The current agent (OCR-047) sends a "context preview" — a lightweight summary of the task state (not the full payload).
   - The hot standby receiver (VER-023) allocates working memory for the task.
   - VER-023 loads the task schema, expected artifacts, and workflow instructions.
   - VER-023 initializes its Verification module specifically configured for this task type.

3. **Final Handover (when current agent completes):**
   - Full handover request sent.
   - But validation phase is faster (receiver already knows the task).
   - Transfer phase only needs delta context (what changed since pre-load).
   - Total handover time target: < 1s (vs standard 5s).

**Comparison:**
```
Standard:  [OCR working...] → [Handover 5s] → [VER starts working]
Hot Standby: [OCR working...] → [VER pre-loading...] → [Handover 1s] → [VER continues]
                                              [overlap]
```

**Resource Cost:**
- Hot standby consumes ~30% of the memory it would use for full operation.
- Hot standby consumes minimal CPU (idle).
- Maximum simultaneous hot standby assignments per agent: 3.

**Pre-load Time Budget:**
| Context Complexity | Pre-load Time | Pre-load Window |
|-------------------|--------------|-----------------|
| Simple (single artifact, < 1 MB) | 1s | Should start 2 min before handover |
| Moderate (2-5 artifacts, 1-5 MB) | 3s | Should start 5 min before handover |
| Complex (5+ artifacts, 5-10 MB) | 5s | Should start 10 min before handover |

---

## 13. Appendices

### Appendix A: Handover Protocol Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-01-15 | Initial handover protocol |
| 1.1 | 2026-03-01 | Added warm handover support; added error history field |
| 2.0 | 2026-05-01 | Major revision: added quality gates, shift handover template, emergency handover protocol |
| 2.1 | 2026-07-01 | Added reference-based handover, hot standby, predictive handover; payload size limit increased to 10 MB |

### Appendix B: Handover Configuration Parameters

All configurable parameters with their defaults:

| Parameter | Default | Range | Description |
|-----------|---------|-------|-------------|
| `max_payload_size_bytes` | 10,485,760 (10 MB) | 1 MB – 100 MB | Maximum uncompressed payload size |
| `cold_handover_timeout_ms` | 5000 | 1000 – 30000 | Total timeout for cold handover |
| `warm_handover_timeout_ms` | 30000 | 5000 – 60000 | Total timeout for warm handover |
| `emergency_handover_timeout_ms` | 1000 | 500 – 5000 | Total timeout for emergency handover |
| `validation_timeout_ms` | 1000 | 200 – 5000 | Timeout for validation phase |
| `ack_timeout_ms` | 2000 | 500 – 5000 | Timeout for acknowledgment phase |
| `max_retry_count` | 3 | 0 – 10 | Maximum retry attempts for handover |
| `retry_backoff_base_ms` | 1000 | 100 – 10000 | Base delay for retry backoff (delay = attempt² × base) |
| `max_parallel_handovers_per_agent` | 3 (out), 5 (in) | 1 – 20 | Parallel handover limits |
| `batch_max_tasks` | 10 | 1 – 50 | Maximum tasks in a batch handover |
| `checkpoint_interval_seconds` | 30 | 5 – 300 | Periodic state checkpoint interval |
| `heartbeat_timeout_seconds` | 3 | 1 – 30 | Agent heartbeat timeout for crash detection |
| `audit_retention_days` | 90 | 30 – 365 | Handover audit log retention period |
| `payload_retention_days_success` | 7 | 1 – 90 | Full payload retention for successful handovers |
| `payload_retention_days_failed` | 30 | 7 – 90 | Full payload retention for failed handovers |
| `sla_success_rate_target` | 0.95 | 0.0 – 1.0 | Target handover success rate |
| `sla_duration_target_cold_ms` | 5000 | 1000 – 30000 | Target cold handover duration (p95) |
| `alert_failure_rate_threshold` | 0.05 | 0.01 – 0.20 | Failure rate threshold for alerts |

### Appendix C: Related Documents

| Document ID | Title | Relationship |
|------------|-------|-------------|
| DOC-03 | Agent Communication Protocol | Lower-layer messaging protocol on which handover relies |
| DOC-04 | Agent State Management | Defines state machine and checkpointing used in handover |
| DOC-05 | Task Orchestration Rules | Defines workflow pipeline that triggers handovers |
| DOC-07 | Error Handling and Recovery | Defines error types and recovery procedures referenced by handover |
| DOC-08 | Agent Health Monitoring | Defines heartbeat and health checks used in emergency handover |
| DOC-09 | Workforce Scheduling | Defines shift schedules referenced by shift handover |
| DOC-12 | Inter-Agent Routing | Defines routing tables used by smart routing |

### Appendix D: Glossary

| Term | Definition |
|------|-----------|
| **Handover** | Formal transfer of task ownership, context, and state between agents |
| **Sender/Handing agent** | The agent initiating the handover (currently owns the task) |
| **Receiver/Receiving agent** | The agent accepting the handover (will own the task afterward) |
| **Handover payload** | The complete set of data transferred during a handover |
| **Reference-based handover** | Handover where the payload is too large for in-memory transfer; URIs are passed instead |
| **Warm handover** | Both agents operate concurrently during an overlap period |
| **Cold handover** | Sender completes before receiver begins; zero overlap |
| **Emergency handover** | Unplanned handover triggered by agent failure |
| **Shift handover** | Handover of all active tasks at shift change |
| **Handover lifecycle** | The 5-phase process: initiation → validation → transfer → acknowledgment → completion |
| **Quality gates** | 7 validation checks that must pass before handover initiation |
| **Checksum** | SHA-256 hash of the payload for integrity verification |
| **Retry backoff** | Exponential delay between retry attempts (1s, 4s, 9s) |
| **Hot standby** | Receiver pre-loaded with context before formal handover |
| **Smart routing** | Algorithmic receiver selection based on multi-criteria scoring |
| **Handover loop** | Circular handover pattern where a task returns to a prior handler |
| **Orphaned handover** | A handover stuck in an intermediate state indefinitely |

---

*End of Document 13/24 — Handover Rules*
*Next Document: 14/24 — Inter-Agent Dispute Resolution*
