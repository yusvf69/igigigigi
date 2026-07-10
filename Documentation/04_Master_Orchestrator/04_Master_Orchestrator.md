# MASTER ORCHESTRATOR

## Islamic Education Platform (IEP)

---

| Document ID | IEP-ARCH-ORCH-001 |
|---|---|
| Version | 1.0.0 |
| Status | Approved |
| Classification | Enterprise Internal |
| Effective Date | 2026-07-07 |
| Approving Authority | Office of the Chief Architect |
| Custodian | Architecture Review Board |
| Constitutional Reference | IEP-CONST-001 (Section 6) |
| Agent Architecture Reference | IEP-ARCH-AGENT-001 |

---

## TABLE OF CONTENTS

1. EXECUTIVE OVERVIEW
2. ORCHESTRATOR ARCHITECTURE PHILOSOPHY
3. ORCHESTRATOR CORE ARCHITECTURE
4. WORKFLOW ENGINE
5. WORKFLOW CATALOG
6. TASK DISPATCH AND SCHEDULING
7. AGENT REGISTRY AND DISCOVERY
8. STATE MANAGEMENT
9. RESULT COLLECTION AND ASSEMBLY
10. ERROR HANDLING AND COMPENSATION
11. HUMAN-IN-THE-LOOP MANAGEMENT
12. PRIORITY AND QUEUE MANAGEMENT
13. OBSERVABILITY AND MONITORING
14. SECURITY AND ACCESS CONTROL
15. SCALABILITY AND PERFORMANCE
16. RESILIENCE AND RECOVERY
17. ORCHESTRATOR INTEGRATION POINTS
18. WORKFLOW DESIGNER INTERFACE
19. TESTING AND VALIDATION
20. DEPLOYMENT AND OPERATIONS
21. FUTURE EVOLUTION
22. APPENDICES

---

## 1. EXECUTIVE OVERVIEW

### 1.1 Purpose

The Master Orchestrator is the central coordination hub of the entire Islamic Education Platform. It is responsible for receiving high-level goals from users, administrators, or scheduled triggers, decomposing them into workflows, dispatching tasks to appropriate agents, collecting and assembling results, handling errors and exceptions, and managing the flow of work across the entire multi-agent system.

### 1.2 Role in the Architecture

```
    User/Admin/Scheduler
           |
           v
    +============================================+
    |                                         |
    |         MASTER ORCHESTRATOR              |
    |                                         |
    |  +----------+  +---------+  +---------+ |
    |  | Workflow |  | Task    |  | State   | |
    |  | Engine   |  | Dispatcher|  | Manager| |
    |  +----------+  +---------+  +---------+ |
    |  +----------+  +---------+  +---------+ |
    |  | Result   |  | Error   |  | Human   | |
    |  | Collector|  | Handler |  | in Loop | |
    |  +----------+  +---------+  +---------+ |
    |                                         |
    +============================================+
           |           |           |
    +------v--+  +-----v----+  +--v---------+
    |Extract  |  |Verify    |  |Content Gen |
    |Agents   |  |Agents    |  |Agents      |
    +---------+  +----------+  +------------+
    +------v--+  +-----v----+
    |Learning |  |Quality   |
    |Agents   |  |Agents    |
    +---------+  +----------+
```

### 1.3 Core Responsibilities

1. **Workflow Management**: Define, execute, and monitor complex multi-step workflows.
2. **Task Dispatch**: Assign tasks to appropriate agents based on capability, availability, and priority.
3. **State Tracking**: Maintain the complete state of all active workflows and tasks.
4. **Result Collection**: Gather results from multiple agents and assemble final outputs.
5. **Error Handling**: Detect failures, execute retry strategies, and manage compensation.
6. **Human-in-the-Loop**: Route tasks requiring human judgment to reviewers and incorporate feedback.
7. **Scheduling**: Trigger workflows on schedules, events, or demand.
8. **Observability**: Expose workflow status, metrics, and logs for monitoring.

### 1.4 Non-Responsibilities

The Orchestrator does NOT:
- Perform domain-specific reasoning (agent responsibility).
- Store knowledge (Knowledge Graph responsibility).
- Generate content (Content Agent responsibility).
- Manage user accounts (User Service responsibility).
- Serve API requests to end-users (API Gateway responsibility).

---

## 2. ORCHESTRATOR ARCHITECTURE PHILOSOPHY

### 2.1 Design Principles

| Principle | Description |
|---|---|
| **Deterministic Execution** | Given the same input and state, the orchestrator always produces the same workflow execution. Non-determinism is isolated to AI agents. |
| **Exactly-Once Workflow** | Workflows execute exactly once. Duplicate execution prevented through idempotency keys and deduplication. |
| **Compensating Transactions** | Every workflow step has a defined compensation action for rollback on failure. |
| **Observability by Default** | Every workflow state change, task dispatch, and result is logged and traceable. |
| **Graceful Degradation** | When agents fail, the orchestrator degrades gracefully (partial results, fallback modes) rather than failing entirely. |
| **Cost-Bounded Execution** | Every workflow has a maximum cost budget. The orchestrator tracks spending and stops if budget exceeded. |

### 2.2 Key Architectural Decisions

| Decision | Choice | Rationale |
|---|---|---|
| Workflow Engine | Temporal.io | Industry leader for durable workflow execution, built-in retry, timeout, state persistence |
| Task Queue | Kafka (per agent type) | Durable, scalable, supports exactly-once semantics |
| State Store | PostgreSQL + Redis | PG for durable state, Redis for hot state access |
| Workflow Definition | YAML + Custom DSL | Human-readable, version-controlled, supports A/B testing |
| Scheduling | Temporal Cron + Event Triggers | Flexible scheduling and event-driven triggers |

---

## 3. ORCHESTRATOR CORE ARCHITECTURE

### 3.1 High-Level Architecture

```
    +===================================================================+
    |                      MASTER ORCHESTRATOR                           |
    |                                                                     |
    |  +--------------------------------------------------------------+  |
    |  |                   WORKFLOW API LAYER                          |  |
    |  |                                                               |  |
    |  |  /orchestrator/v1/workflows  - CRUD workflow definitions      |  |
    |  |  /orchestrator/v1/executions - Start, query, cancel           |  |
    |  |  /orchestrator/v1/tasks      - Task status, retry, escalate   |  |
    |  |  /orchestrator/v1/agents     - Agent registry management      |  |
    |  +--------------------------------------------------------------+  |
    |                                                                     |
    |  +--------------------------------------------------------------+  |
    |  |                   WORKFLOW ENGINE (Temporal)                  |  |
    |  |                                                               |  |
    |  |  +------------------+  +-----------------+  +---------------+  |  |
    |  |  | Workflow         |  | Activity        |  | Child Workflow|  |  |
    |  |  | Definitions      |  | Executors       |  | (Sub-workflow)|  |  |
    |  |  +------------------+  +-----------------+  +---------------+  |  |
    |  |                                                               |  |
    |  |  Features: Durable execution, retry, timeout, saga, signal    |  |
    |  +--------------------------------------------------------------+  |
    |                                                                     |
    |  +--------------------------------------------------------------+  |
    |  |                   CORE SERVICES                               |  |
    |  |                                                               |  |
    |  |  +------------------+  +-----------------+  +---------------+  |  |
    |  |  | Task Dispatcher  |  | State Manager   |  | Result        |  |  |
    |  |  | (Agent Selection,|  | (Workflow State,|  | Collector     |  |  |
    |  |  |  Queue Routing)  |  |  Checkpoints)   |  | (Aggregation, |  |  |
    |  |  +------------------+  +-----------------+  |  Assembly)    |  |  |
    |  |                                              +---------------+  |  |
    |  |  +------------------+  +-----------------+  +---------------+  |  |
    |  |  | Error Handler    |  | Human-in-Loop   |  | Scheduler     |  |  |
    |  |  | (Retry,          |  | (Review Queue,  |  | (Time-based,  |  |  |
    |  |  |  Compensation,   |  |  Escalation)    |  |  Event-based) |  |  |
    |  |  |  Escalation)     |  +-----------------+  +---------------+  |  |
    |  |  +------------------+                                            |  |
    |  +--------------------------------------------------------------+  |
    |                                                                     |
    |  +--------------------------------------------------------------+  |
    |  |                   DATA STORES                                |  |
    |  |                                                               |  |
    |  |  +------------------+  +-----------------+  +---------------+  |  |
    |  |  | PostgreSQL       |  | Redis           |  | Temporal      |  |  |
    |  |  | (Durable State,  |  | (Hot State,     |  | (Workflow     |  |  |
    |  |  |  Workflow Defs,  |  |  Locks,         |  |  Engine State)|  |  |
    |  |  |  Audit Log)      |  |  Rate Limits)   |  |               |  |  |
    |  |  +------------------+  +-----------------+  +---------------+  |  |
    |  +--------------------------------------------------------------+  |
    +===================================================================+
```

### 3.2 Component Responsibilities

| Component | Primary Responsibility | Dependencies |
|---|---|---|
| Workflow API Layer | Accept workflow execution requests, return status | Temporal, Auth Service |
| Workflow Engine (Temporal) | Execute durable workflows with retry, timeout, saga | Temporal Server (PostgreSQL + Kafka) |
| Task Dispatcher | Route tasks to appropriate agent queues | Agent Registry, Kafka |
| State Manager | Track workflow/task state, provide checkpoint/restore | PostgreSQL, Redis |
| Result Collector | Aggregate and assemble results from multiple agents | Kafka, Object Store |
| Error Handler | Execute retry strategies, compensation actions, escalation | Temporal, Notification Service |
| Human-in-Loop Manager | Manage review queue, route to reviewers, collect decisions | Review Queue, Notification Service |
| Scheduler | Trigger workflows on schedule or events | Temporal Cron, Kafka Events |
| Agent Registry | Maintain catalog of available agents, their capabilities and status | PostgreSQL, Health Checks |

---

## 4. WORKFLOW ENGINE

### 4.1 Workflow Execution Model

The Master Orchestrator uses **Temporal.io** for durable workflow execution:

```
    +------------------+     +------------------+     +------------------+
    | Client           |     | Temporal Server  |     | Worker (Go/Rust)  |
    | (API Layer)      |     |                  |     |                   |
    |                  |     | +--------------+ |     | +---------------+ |
    | StartWorkflow()  |---->| | Workflow      | |---->| | Workflow      | |
    |                  |     | | Execution     | |     | | Definition    | |
    |                  |     | | (Durable)     | |     | | Execution     | |
    |                  |     | +--------------+ |     | +---------------+ |
    |                  |     | +--------------+ |     | +---------------+ |
    |                  |     | | Activity     | |---->| | Activity      | |
    |                  |     | | Execution    | |     | | Execution     | |
    |                  |     | +--------------+ |     | +---------------+ |
    +------------------+     +------------------+     +------------------+
                                  |        |
                          +-------v-+  +--v--------+
                          | Temporal|  | Temporal  |
                          | State   |  | Queue     |
                          | (PG)    |  | (Kafka)   |
                          +---------+  +-----------+
```

### 4.2 Workflow Definition

Workflows are defined using a combination of:
- **Temporal Workflow Code**: Core execution logic in Go or Rust.
- **YAML Workflow Definitions**: High-level workflow structure, parameters, and agent assignments.
- **Task Templates**: Parameterized task definitions for common patterns.

#### 4.2.1 Workflow Definition Structure

```yaml
workflow:
  id: "book-ingestion-pipeline"
  name: "Book Ingestion Pipeline"
  version: "2.1.0"
  description: "Complete pipeline: upload -> OCR -> extract -> verify -> store"
  
  input_schema:
    book_id: { type: string, required: true }
    priority: { type: integer, default: 5, min: 1, max: 10 }
    languages: { type: array, items: string }
  
  max_execution_time: "48h"
  max_cost_usd: 10.00
  
  steps:
    - id: validate_input
      type: activity
      activity: validate_book_input
      retry: { max_attempts: 3 }
      timeout: "5m"
      
    - id: ocr_processing
      type: agent_task
      agent_type: ocr-agent
      input:
        book_id: "{input.book_id}"
        languages: "{input.languages}"
        quality_threshold: 0.95
      depends_on: [validate_input]
      timeout: "4h"
      retry: { max_attempts: 2 }
      compensation: delete_ocr_results
      
    - id: layout_analysis
      type: agent_task
      agent_type: layout-agent
      input:
        book_id: "{input.book_id}"
        ocr_result: "{steps.ocr_processing.result}"
      depends_on: [ocr_processing]
      timeout: "30m"
      retry: { max_attempts: 2 }
      
    - id: named_entity_recognition
      type: agent_task
      agent_type: entity-agent
      input:
        book_id: "{input.book_id}"
        text: "{steps.ocr_processing.result.text}"
        layout: "{steps.layout_analysis.result}"
      depends_on: [layout_analysis]
      timeout: "1h"
      retry: { max_attempts: 2 }
      
    - id: fact_extraction
      type: agent_task
      agent_type: fact-agent
      input:
        book_id: "{input.book_id}"
        entities: "{steps.named_entity_recognition.result}"
        text: "{steps.ocr_processing.result.text}"
      depends_on: [named_entity_recognition]
      timeout: "2h"
      retry: { max_attempts: 2 }
      
    - id: verification
      type: sub_workflow
      workflow: fact-verification-pipeline
      input:
        book_id: "{input.book_id}"
        facts: "{steps.fact_extraction.result}"
      depends_on: [fact_extraction]
      timeout: "12h"
      
    - id: store_knowledge
      type: activity
      activity: store_in_knowledge_graph
      input:
        book_id: "{input.book_id}"
        verified_facts: "{steps.verification.result}"
      depends_on: [verification]
      timeout: "30m"
      
    - id: trigger_content_generation
      type: signal
      signal: content_generation_triggered
      input:
        book_id: "{input.book_id}"
      depends_on: [store_knowledge]
      
    - id: notify_completion
      type: activity
      activity: send_notification
      input:
        type: book_ingestion_complete
        book_id: "{input.book_id}"
        status: success
      depends_on: [store_knowledge]
  
  compensation_steps:
    - on_failure: [any]
      actions:
        - activity: mark_book_failed
          params: { book_id: "{input.book_id}", reason: "{failure.reason}" }
        - activity: notify_admin
          params: { type: book_ingestion_failed, book_id: "{input.book_id}" }
```

### 4.3 Workflow Execution Guarantees

| Guarantee | Implementation |
|---|---|
| **Durable Execution** | Temporal persists workflow state. Workflow survives worker crashes and restarts. |
| **Exactly-Once Execution** | Idempotency keys prevent duplicate workflow starts. |
| **At-Least-Once Activities** | Temporal retries activities until success or max attempts. |
| **Deterministic Replay** | Workflow code is deterministic. Non-determinism isolated to activities. |
| **Timeout Enforcement** | Schedule-to-close, start-to-close, and heartbeat timeouts enforced. |
| **Retry with Backoff** | Exponential backoff with jitter for all activities and agent tasks. |
| **Saga Compensation** | On failure, compensation actions are executed in reverse order. |

---

## 5. WORKFLOW CATALOG

### 5.1 Complete Workflow Inventory

| ID | Workflow Name | Domain | Priority | Est. Duration | Agents Involved |
|---|---|---|---|---|---|
| WF-001 | book-ingestion-pipeline | Knowledge | P0 | 2-48 hours | OCR, Layout, Entity, Fact, Cross-Ref, Verification |
| WF-002 | fact-verification-pipeline | Verification | P0 | 1-12 hours | Cross-Ref, LLM Check, Contradiction, Scholarly Review |
| WF-003 | content-generation-for-book | Content | P0 | 1-4 hours | All Content Agents |
| WF-004 | single-mode-generation | Content | P0 | 5-30 minutes | One Content Agent |
| WF-005 | multi-mode-generation | Content | P0 | 30-120 minutes | Multiple Content Agents |
| WF-006 | ai-teacher-conversation | Learning | P0 | Real-time | AI Teacher, RAG |
| WF-007 | adaptive-learning-path | Learning | P0 | Real-time | Adaptive Path, Progress |
| WF-008 | quiz-assessment | Learning | P0 | 1-5 minutes | Assessment |
| WF-009 | quality-check-content | Quality | P0 | 5-30 minutes | All Quality Agents |
| WF-010 | continuous-reverification | Verification | P1 | 24-72 hours | Continuous Verification |
| WF-011 | batch-translation | Content | P1 | 1-24 hours | Translation Agent |
| WF-012 | scheduled-content-refresh | Content | P1 | 1-4 hours | Content Agents |
| WF-013 | user-onboarding | User | P0 | Real-time | Recommendation, Progress |
| WF-014 | content-publishing | Content | P0 | 1-10 minutes | Publishing Service |
| WF-015 | report-generation | Analytics | P1 | 5-30 minutes | Analytics Agents |

### 5.2 Workflow Dependency Graph

```
    WF-001 (Book Ingestion)
      |
      +-- WF-002 (Fact Verification)
      |     |
      |     +-- WF-003 (Content Generation for Book)
      |           |
      |           +-- WF-009 (Quality Check Content)
      |                 |
      |                 +-- WF-014 (Content Publishing)
      |
      +-- (triggers) WF-012 (Scheduled Content Refresh)

    WF-004 (Single Mode Generation)
      |
      +-- WF-009 (Quality Check Content)
            |
            +-- WF-014 (Content Publishing)

    WF-005 (Multi-Mode Generation)
      |
      +-- (Fan-out to multiple WF-004 instances)
      |
      +-- WF-009 (Quality Check All)
            |
            +-- WF-014 (Content Publishing)
```

---

## 6. TASK DISPATCH AND SCHEDULING

### 6.1 Task Dispatch Flow

```
    Workflow Engine
         |
         | Activity: DispatchAgentTask
         v
    +--------------------+
    | Task Dispatcher     |
    |                     |
    | 1. Validate task    |
    | 2. Lookup agent type|
    | 3. Check agent      |
    |    availability     |
    | 4. Check rate limits|
    | 5. Build task       |
    |    message          |
    | 6. Enqueue to Kafka |
    +--------------------+
         |
         v
    +--------------------+     +--------------------+
    | Kafka Topic:       |---->| Agent Consumer     |
    | agent.quiz-agent   |     | (Quiz Agent Pool)  |
    +--------------------+     +--------------------+
         |
         | (Task enqueued with key: task_id)
         v
    +--------------------+
    | Wait for Result     |
    | (Kafka Result Topic)|
    +--------------------+
```

### 6.2 Agent Selection Strategy

```yaml
agent_selection:
  strategy: "capability_and_load"
  
  criteria:
    - agent_type matches task type (mandatory)
    - agent_version compatible (mandatory)
    - agent has required capabilities (mandatory)
    - agent has capacity (queue depth < threshold)
    - agent in correct region (preferred)
  
  routing:
    default: "round_robin"
    high_priority: "least_loaded"
    bulk: "batch_queue"
    realtime: "nearest_available"
```

### 6.3 Priority Queue Management

| Priority Level | Label | Max Queue Depth | Preemption | Examples |
|---|---|---|---|---|
| 0 | Critical | 10 | Preempts all | System health, legal compliance |
| 1 | High | 50 | Preempts 2-4 | User-facing content generation |
| 2 | Normal | 200 | None | Standard book ingestion |
| 3 | Low | 1000 | None | Batch translation, analytics |
| 4 | Background | 5000 | None | Scheduled re-verification |

### 6.4 Task Scheduling Patterns

| Pattern | Trigger | Mechanism |
|---|---|---|
| **On-Demand** | User or API request | Immediate dispatch |
| **Event-Driven** | Kafka event | Event handler triggers workflow |
| **Scheduled** | Cron expression | Temporal Cron Schedule |
| **Chained** | Previous workflow completes | Workflow signal |
| **Batched** | Accumulate tasks over time | Temporal buffered signal |
| **Periodic** | Fixed interval | Temporal Cron |

---

## 7. AGENT REGISTRY AND DISCOVERY

### 7.1 Agent Registry

```json
{
  "agent_registry": {
    "agent_types": [
      {
        "agent_type": "quiz-agent",
        "display_name": "Quiz Generation Agent",
        "version": "2.3.1",
        "status": "active",
        "capabilities": [
          "multiple_choice_generation",
          "true_false_generation",
          "matching_generation",
          "fill_blank_generation",
          "adaptive_difficulty",
          "explanation_generation"
        ],
        "supported_input_types": ["fact_list", "categorized_facts", "knowledge_refs"],
        "supported_output_types": ["quiz_json", "quiz_markdown", "quiz_xml"],
        "languages": ["ar", "en", "ur", "id", "fr", "tr", "bn", "sw"],
        "models": {
          "primary": "gpt-4o",
          "fallback": "gpt-4o-mini"
        },
        "performance_profile": {
          "p50_latency_ms": 5000,
          "p95_latency_ms": 15000,
          "p99_latency_ms": 30000,
          "max_concurrent": 20,
          "cost_per_task_avg": 0.05
        },
        "rate_limits": {
          "max_per_minute": 100,
          "max_per_hour": 2000,
          "max_per_day": 40000
        },
        "requirements": {
          "cpu": 2,
          "memory_gb": 4,
          "gpu": false
        },
        "health": {
          "status": "healthy",
          "last_heartbeat": "2026-07-07T10:00:00Z",
          "current_queue_depth": 15,
          "running_instances": 8
        }
      }
    ]
  }
}
```

### 7.2 Agent Health Monitoring

| Check | Frequency | Action on Failure |
|---|---|---|
| Heartbeat | 30 seconds | Mark agent as unhealthy, route to fallback |
| Queue Depth | 10 seconds | Scale up if depth > threshold |
| Task Success Rate | 5 minutes | Alert if rate < 90%, pause agent if < 70% |
| Resource Utilization | 1 minute | Scale resources or throttle tasks |
| Latency | 5 minutes | Alert if P95 > threshold |
| Cost Rate | 1 minute | Alert if cost exceeds budget |

---

## 8. STATE MANAGEMENT

### 8.1 State Store Architecture

```
    +===================================================================+
    |                      STATE MANAGEMENT                              |
    |                                                                     |
    |  +------------------+  +-----------------+  +------------------+   |
    |  | WORKFLOW STATE   |  | TASK STATE      |  | LOCK STATE       |   |
    |  | (Temporal)       |  | (PostgreSQL)    |  | (Redis)          |   |
    |  |                  |  |                 |  |                  |   |
    |  | - Workflow       |  | - Task ID       |  | - Distributed    |   |
    |  |   Execution      |  | - Status        |  |   Locks          |   |
    |  | - History        |  | - Agent         |  | - Mutex          |   |
    |  | - Checkpoints    |  | - Input/Output  |  | - Leases         |   |
    |  | - Pending        |  | - Retry Count   |  |                  |   |
    |  |   Activities     |  | - Error         |  |                  |   |
    |  | - Signals        |  | - Duration      |  |                  |   |
    |  |                  |  | - Cost          |  |                  |   |
    |  +------------------+  +-----------------+  +------------------+   |
    |                                                                     |
    |  +------------------+  +-----------------+  +------------------+   |
    |  | AUDIT STATE      |  | CACHED STATE    |  | COMPENSATION     |   |
    |  | (PostgreSQL)     |  | (Redis)         |  | STATE            |   |
    |  |                  |  |                 |  | (Temporal)       |   |
    |  | - All state      |  | - Hot workflow  |  |                  |   |
    |  |   Changes        |  |   state for     |  | - Compensation   |   |
    |  | - Timestamps     |  |   fast query    |  |   actions taken  |   |
    |  | - Actor          |  | - Active task   |  | - Pending        |   |
    |  | - Previous State |  |   snapshots     |  |   compensations  |   |
    |  +------------------+  +-----------------+  +------------------+   |
    +===================================================================+
```

### 8.2 State Transition Model

```yaml
workflow_states:
  PENDING:          { initial: true }
  RUNNING:          {}
  PAUSED:           { pause_reasons: [human_review, wait_dependency] }
  COMPLETED:        { terminal: true, success: true }
  FAILED:           { terminal: true, success: false }
  COMPENSATED:      { terminal: true, success: false, description: "All compensations executed" }
  TIMED_OUT:        { terminal: true, success: false }
  CANCELLED:        { terminal: true, success: false }

task_states:
  PENDING:          { initial: true }
  QUEUED:           {}
  DISPATCHED:       {}
  RUNNING:          {}
  BLOCKED:          { block_reasons: [human_review, dependency] }
  RETRYING:         {}
  COMPLETED:        { terminal: true, success: true }
  FAILED:           { terminal: true, success: false }
  TIMED_OUT:        { terminal: true, success: false }
  CANCELLED:        { terminal: true, success: false }
```

### 8.3 State Query API

```json
// GET /orchestrator/v1/executions/{workflow_id}/state
{
  "workflow_id": "wf-book-ingestion-123",
  "status": "RUNNING",
  "progress": {
    "total_steps": 7,
    "completed_steps": 4,
    "failed_steps": 0,
    "current_step": "verification",
    "percent_complete": 57
  },
  "current_task": {
    "task_id": "task-verification-123",
    "agent_type": "verification-orchestrator",
    "status": "RUNNING",
    "started_at": "2026-07-07T09:30:00Z",
    "elapsed_seconds": 450
  },
  "completed_tasks": [
    {
      "task_id": "task-ocr-123",
      "agent_type": "ocr-agent",
      "status": "COMPLETED",
      "duration_seconds": 120,
      "confidence": 0.96
    }
  ],
  "timeline": [
    { "timestamp": "2026-07-07T09:00:00Z", "event": "workflow_started" },
    { "timestamp": "2026-07-07T09:02:00Z", "event": "task_completed", "task": "validate_input" },
    { "timestamp": "2026-07-07T09:02:05Z", "event": "task_dispatched", "task": "ocr_processing" },
    { "timestamp": "2026-07-07T09:04:05Z", "event": "task_completed", "task": "ocr_processing" },
    { "timestamp": "2026-07-07T09:04:10Z", "event": "task_dispatched", "task": "layout_analysis" },
    { "timestamp": "2026-07-07T09:04:40Z", "event": "task_completed", "task": "layout_analysis" },
    { "timestamp": "2026-07-07T09:04:45Z", "event": "task_dispatched", "task": "named_entity_recognition" },
    { "timestamp": "2026-07-07T09:22:00Z", "event": "task_dispatched", "task": "verification" }
  ],
  "estimated_remaining": "4h 30m",
  "errors": [],
  "warnings": [
    "OCR quality: 0.89 (below threshold 0.95). Proceeding with reduced confidence."
  ]
}
```

---

## 9. RESULT COLLECTION AND ASSEMBLY

### 9.1 Result Collection Patterns

| Pattern | Description | Use Case |
|---|---|---|
| **Single Result** | One agent returns one result | Simple task |
| **Sequential Assembly** | Each step passes result to next | Pipeline workflows |
| **Parallel Merge** | Multiple parallel results merged | Content generation for all modes |
| **Best-of-N** | Multiple agents run, best selected | Quiz generation (generate 3, pick best) |
| **Voting** | Multiple agents vote, majority wins | Fact verification |
| **Streaming** | Continuous result stream | AI Teacher conversation |

### 9.2 Result Assembly

```
    Workflow: Multi-Mode Content Generation
    =======================================
    
    Knowledge: Book_123 (verified facts about Seerah)
    
    Step 1: Fan-Out to Content Agents (parallel)
    +-----------+         +------------------+
    | Story     |-------->| Story: "The Life |  Assembly
    | Agent     |         | of the Prophet"  |-------+
    +-----------+         +------------------+       |
    +-----------+         +------------------+       |
    | Quiz      |-------->| Quiz: 20         |       |
    | Agent     |         | Questions        |       |
    +-----------+         +------------------+       |
    +-----------+         +------------------+       |
    | Timeline  |-------->| Timeline: 50     |       |
    | Agent     |         | Events           |       |
    +-----------+         +------------------+       |
    +-----------+         +------------------+       |
    | Podcast   |-------->| Script: 15 min   |       |
    | Agent     |         | Audio: 15 min    |       |
    +-----------+         +------------------+       |
    +-----------+         +------------------+       |
    | Maps      |-------->| Interactive Map  |       |
    | Agent     |         | with 20 locations|       |
    +-----------+         +------------------+       |
                                                     v
                                            +==================+
                                            | Content Package  |
                                            |                  |
                                            | - Book ID: 123   |
                                            | - Story: ✓       |
                                            | - Quiz: ✓        |
                                            | - Timeline: ✓    |
                                            | - Podcast: ✓     |
                                            | - Maps: ✓        |
                                            |                  |
                                            | Status: COMPLETED |
                                            +==================+
```

### 9.3 Result Schema

```json
{
  "workflow_id": "wf-content-gen-456",
  "status": "COMPLETED",
  "result": {
    "book_id": "book-123",
    "content_package": {
      "modes": {
        "story": {
          "status": "COMPLETED",
          "content_id": "content-story-789",
          "confidence": 0.94,
          "dimensions": { "audio": false, "video": false, "interactive": false },
          "metadata": { "word_count": 2500, "generation_time_s": 18 }
        },
        "quiz": {
          "status": "COMPLETED",
          "content_id": "content-quiz-790",
          "confidence": 0.96,
          "num_questions": 20,
          "metadata": { "questions_by_type": { "mcq": 12, "true_false": 5, "matching": 3 } }
        },
        "timeline": {
          "status": "COMPLETED",
          "content_id": "content-timeline-791",
          "confidence": 0.92,
          "metadata": { "event_count": 50, "date_range": "570-632 CE" }
        },
        "podcast": {
          "status": "COMPLETED",
          "content_id": "content-podcast-792",
          "confidence": 0.88,
          "metadata": { "duration_minutes": 15, "audio_url": "...", "script_word_count": 3500 }
        },
        "maps": {
          "status": "COMPLETED",
          "content_id": "content-maps-793",
          "confidence": 0.90,
          "metadata": { "locations": 20, "routes": 5 }
        }
      }
    },
    "metrics": {
      "total_duration_seconds": 240,
      "total_cost_usd": 1.45,
      "agents_involved": 5,
      "total_tokens": 45000
    }
  }
}
```

---

## 10. ERROR HANDLING AND COMPENSATION

### 10.1 Error Handling Strategy

```
    Task Failure Detected
         |
    +----v----+
    | Retry?  |
    +----+----+
         |
    +----+--------+
    |             |
    YES           NO
    |             |
    v             v
    Retry     +---+----+
    (backoff) | Compensate? |
    |         +---+----+
    |             |
    |        +----+----+
    |        |         |
    |       YES       NO
    |        |         |
    |        v         v
    |    Execute     Mark Failed
    |    Compensa-   Notify
    |    tion        Orchestrator
    |    Actions
    |
    +--> If all retries exhausted
         |
         v
    (Same as NO path)
```

### 10.2 Retry Strategy Configuration

```yaml
retry_strategy:
  default:
    max_attempts: 3
    initial_interval: "5s"
    backoff_coefficient: 2.0
    maximum_interval: "5m"
    non_retryable_errors:
      - "INVALID_INPUT"
      - "PERMISSION_DENIED"
      - "DATA_INTEGRITY_ERROR"
  
  agent_tasks:
    max_attempts: 3
    initial_interval: "10s"
    backoff_coefficient: 2.0
    maximum_interval: "10m"
    non_retryable_errors:
      - "AGENT_NOT_FOUND"
      - "AGENT_CAPABILITY_MISMATCH"
      - "HUMAN_REVIEW_REJECTED"
  
  verification:
    max_attempts: 5
    initial_interval: "30s"
    backoff_coefficient: 3.0
    maximum_interval: "30m"
```

### 10.3 Compensation Actions

| Workflow | Failure Step | Compensation Action |
|---|---|---|
| Book Ingestion | OCR Failed | Mark book as failed, notify admin, no cleanup needed |
| Book Ingestion | Verification Failed | Mark facts as unverified, store with low confidence |
| Book Ingestion | Knowledge Graph Storage Failed | Retry storage, if fails: keep facts in staging area |
| Content Generation | Generation Failed | Return partial package with warning, retry failed mode |
| Content Generation | Quality Check Failed | Flag content as unverified, do not publish |
| Content Publishing | Publish Failed | Rollback content status to draft |
| Translation | Translation Failed | Fall back to source language with note |

### 10.4 Saga Pattern Implementation

```python
async def book_ingestion_saga(ctx: WorkflowContext, input: BookIngestionInput):
    """
    Saga pattern for book ingestion with compensation.
    If any step fails, all previous steps are compensated.
    """
    compensation_actions = []
    
    try:
        # Step 1: Validate
        validation = await ctx.activity(validate_book, input)
        compensation_actions.append(lambda: None)  # No compensation needed
        
        # Step 2: OCR
        ocr_result = await ctx.activity(ocr_process, validation)
        compensation_actions.insert(0, lambda: delete_ocr_results(ocr_result))
        
        # Step 3: Extract entities
        entities = await ctx.activity(extract_entities, ocr_result)
        compensation_actions.insert(0, lambda: delete_entities(entities))
        
        # Step 4: Extract facts
        facts = await ctx.activity(extract_facts, entities)
        compensation_actions.insert(0, lambda: delete_facts(facts))
        
        # Step 5: Store in knowledge graph
        stored = await ctx.activity(store_in_kg, facts)
        compensation_actions.insert(0, lambda: remove_from_kg(stored))
        
        # All steps successful
        return {"status": "completed", "book_id": input.book_id}
        
    except Exception as e:
        # Compensate: execute compensation actions in order
        ctx.log.error(f"Workflow failed at step. Running {len(compensation_actions)} compensations.")
        for compensation in compensation_actions:
            try:
                await compensation()
            except Exception as ce:
                ctx.log.error(f"Compensation failed: {ce}")
                # Log compensation failure for manual intervention
        
        raise WorkflowFailureError(f"Book ingestion failed: {e}. Compensations executed.")
```

---

## 11. HUMAN-IN-THE-LOOP MANAGEMENT

### 11.1 Human Review Workflow

```
    Agent Task
         |
    +----v----+
    | Needs   |
    | Review? |----> No ----> Continue
    +----+----+
         |
         | Yes
         v
    +--------------------+
    | Create Review Task  |
    |  - Content          |
    |  - Context          |
    |  - Questions        |
    |  - Priority         |
    +--------------------+
         |
         v
    +--------------------+
    | Assign to Reviewer  |
    | (Round-robin /      |
    |  Expertise-based)   |
    +--------------------+
         |
         v
    +--------------------+
    | Await Decision     |
    |  - Approved         |
    |  - Rejected         |
    |  - Needs Changes    |
    |  - Escalate         |
    +--------------------+
         |
    +----+----+----+----+
    |    |    |    |    |
    v    v    v    v    v
  Approve  Reject  Change  Escalate  Timeout
    |      |       |       |         |
    v      v       v       v         v
  Continue  Fail  Retry   Senior    Auto-
           Task  with    Reviewer   Approve
                Changes  Assign     (if low
                                    risk)
```

### 11.2 Human Review Queue

```json
{
  "review_queue": {
    "queue_id": "review-queue-scholarly",
    "reviewer_type": "islamic_scholar",
    "items": [
      {
        "review_id": "review-789",
        "task_id": "task-verification-456",
        "workflow_id": "wf-book-123",
        "priority": "high",
        "created_at": "2026-07-07T09:00:00Z",
        "content_type": "fiqh_ruling",
        "content": {
          "ruling": "Zakat is obligatory on retirement savings",
          "source_facts": ["fact-123", "fact-124", "fact-125"],
          "scholarly_disagreement": true,
          "positions": [
            {"scholar": "Imam Abu Hanifa", "position": "Obligatory"},
            {"scholar": "Imam Malik", "position": "Not obligatory"}
          ],
          "ai_confidence": 0.72,
          "verification_results": {
            "cross_reference": "partial_match",
            "llm_check": "consistent",
            "contradiction_detected": false
          }
        },
        "decision_required": {
          "type": "single_select",
          "options": [
            {"value": "approve", "label": "Approve as presented"},
            {"value": "approve_with_note", "label": "Approve with note"},
            {"value": "reject", "label": "Reject - incorrect"},
            {"value": "needs_revision", "label": "Needs revision"}
          ]
        },
        "assigned_reviewer": "scholar-ahmed-001",
        "status": "pending",
        "deadline": "2026-07-08T09:00:00Z",
        "escalation": {
          "escalate_after": "2026-07-08T09:00:00Z",
          "escalate_to": "senior-scholar-board"
        }
      }
    ]
  }
}
```

### 11.3 Escalation Policy

| Level | Reviewer | Review Time | Escalation Condition |
|---|---|---|---|
| L1 | General Reviewer | 24 hours | Timeout or uncertain |
| L2 | Domain Specialist | 12 hours | L1 escalates or high priority |
| L3 | Senior Scholar | 6 hours | L2 escalates or critical priority |
| L4 | Advisory Board | 24 hours | Novel or precedent-setting content |

---

## 12. PRIORITY AND QUEUE MANAGEMENT

### 12.1 Queue Architecture

```
    +------------------------------------------------------------------+
    |                      AGENT QUEUES (Kafka Topics)                   |
    |                                                                   |
    |  Topic: agent.ocr-agent                                          |
    |  Partitions: 10                                                   |
    |  +-- Partition 0: Priority 0 (Critical)                          |
    |  +-- Partition 1: Priority 1 (High)                              |
    |  +-- Partition 2-9: Priority 2-4 (Normal-Low)                    |
    |                                                                   |
    |  Topic: agent.quiz-agent                                         |
    |  Partitions: 5                                                    |
    |  +-- Partition 0: Priority 0-1                                   |
    |  +-- Partition 1-4: Priority 2-4                                 |
    |                                                                   |
    |  Topic: agent.ai-teacher                                         |
    |  Partitions: 20 (high throughput)                                 |
    |  +-- Partition 0-19: User sessions (keyed by user_id)            |
    |                                                                   |
    |  Dead Letter Queue: agent.dlq                                    |
    |  +-- All failed messages (after retry exhausted)                 |
    +------------------------------------------------------------------+
```

### 12.2 Fair Scheduling

```yaml
fair_scheduling:
  enabled: true
  algorithm: "weighted_fair_queuing"
  
  weights:
    tenant_public: 0.4
    tenant_institution: 0.3
    tenant_enterprise: 0.2
    tenant_internal: 0.1
  
  max_concurrent:
    per_tenant: 50
    per_agent_type: 20
    per_user: 5
```

---

## 13. OBSERVABILITY AND MONITORING

### 13.1 Orchestrator Metrics

| Metric | Type | Labels | Description |
|---|---|---|---|
| `orchestrator.workflows.started` | Counter | workflow_type, priority | Total workflows started |
| `orchestrator.workflows.completed` | Counter | workflow_type, status | Workflows completed |
| `orchestrator.workflows.failed` | Counter | workflow_type, error_type | Workflows that failed |
| `orchestrator.workflows.duration` | Histogram | workflow_type | Workflow execution duration |
| `orchestrator.tasks.dispatched` | Counter | agent_type, priority | Tasks dispatched to agents |
| `orchestrator.tasks.completed` | Counter | agent_type | Tasks completed by agents |
| `orchestrator.tasks.failed` | Counter | agent_type, error_type | Tasks that failed |
| `orchestrator.tasks.retried` | Counter | agent_type | Tasks that were retried |
| `orchestrator.tasks.pending` | Gauge | agent_type | Current pending task count |
| `orchestrator.tasks.latency` | Histogram | agent_type | Task dispatch-to-completion latency |
| `orchestrator.human.reviews_requested` | Counter | reviewer_type | Human reviews requested |
| `orchestrator.human.review_time` | Histogram | reviewer_type | Time to complete human review |
| `orchestrator.cost.total` | Counter | workflow_type | Total cost accrued |
| `orchestrator.cost.by_agent` | Counter | agent_type | Cost by agent type |

### 13.2 Dashboard Panels

| Panel | Metric | Visualization | Refresh |
|---|---|---|---|
| Active Workflows | Current RUNNING count | Number card | 10s |
| Workflow Success Rate | (Completed / Started) * 100 | Gauge | 1m |
| Workflow Duration (P50/P95/P99) | workflow_duration histogram | Heatmap | 5m |
| Failed Workflows | Top 5 failure types | Bar chart | 1m |
| Agent Queue Depth | Current pending by agent type | Stacked area | 10s |
| Agent Latency (P95) | task_latency by agent type | Bar chart | 1m |
| Agent Error Rate | (Failed / Dispatched) * 100 | Line chart | 1m |
| Human Review Queue | Pending count by priority | Stacked bar | 30s |
| Cost Accrued Today | Total cost | Number card | 1m |
| Cost by Workflow Type | Cost breakdown | Pie chart | 1h |

### 13.3 Critical Alerts

| Alert | Condition | Severity | Channel |
|---|---|---|---|
| Workflow Failure Rate > 10% | Rolling 1 hour | SEV2 | Slack + PagerDuty |
| Workflow Failure Rate > 25% | Rolling 30 minutes | SEV1 | PagerDuty + SMS |
| Agent Queue Depth > Threshold | 5 consecutive checks | SEV3 | Slack |
| Agent Error Rate > 20% | Rolling 15 minutes | SEV2 | Slack + PagerDuty |
| Human Review Queue > 100 | Pending for > 4 hours | SEV3 | Slack |
| Cost Rate > Budget | Daily budget exceeded 80% | SEV3 | Slack + Email |
| Orchestrator Down | No heartbeat for 30s | SEV0 | Phone + SMS + Slack |

---

## 14. SECURITY AND ACCESS CONTROL

### 14.1 Orchestrator Security

| Security Layer | Implementation |
|---|---|
| **API Authentication** | OAuth 2.1 + JWT for all orchestrator API requests |
| **Workflow Authorization** | Only authorized services can start workflows |
| **Task Authorization** | Tasks assigned only to authorized agent types |
| **Data Access Control** | Orchestrator accesses data stores with minimal privileges |
| **Audit Logging** | All orchestrator actions logged immutably |
| **Rate Limiting** | Per-user, per-tenant, per-workflow-type rate limits |

### 14.2 Permission Model

```json
{
  "orchestrator_permissions": {
    "roles": {
      "admin": {
        "can": ["start_workflow", "cancel_workflow", "modify_workflow_def", "view_all"]
      },
      "operator": {
        "can": ["start_workflow", "cancel_workflow", "view_active", "view_logs"]
      },
      "service": {
        "can": ["start_workflow", "query_workflow"]
      },
      "viewer": {
        "can": ["query_workflow", "view_logs"]
      }
    },
    "workflow_permissions": {
      "book-ingestion-pipeline": {
        "allowed_roles": ["admin", "operator", "service"],
        "allowed_tenants": ["internal"]
      },
      "content-generation-for-book": {
        "allowed_roles": ["admin", "operator", "service"],
        "allowed_tenants": ["*"]
      }
    }
  }
}
```

---

## 15. SCALABILITY AND PERFORMANCE

### 15.1 Scaling Targets

| Metric | Target | Measurement |
|---|---|---|
| Concurrent Workflows | 10,000 | Active workflow instances |
| Task Dispatch Rate | 100,000 tasks/minute | Tasks dispatched per minute |
| Total Agent Types | 50 | Registered agent types |
| State Queries | 10,000/second | State query API calls |
| Human Review Items | 10,000 pending | Items in review queue |
| Workflow Definitions | 500 | Defined workflow templates |

### 15.2 Scaling Strategy

| Component | Scaling Strategy | Mechanism |
|---|---|---|
| Temporal Server | Horizontal (add workers) | Temporal worker pool |
| Task Dispatcher | Horizontal (add instances) | Kafka consumer group rebalance |
| State Store (Read) | Read replicas | PostgreSQL read replicas |
| State Store (Write) | Write master | PostgreSQL primary |
| Hot State (Redis) | Redis Cluster | Cluster mode with sharding |
| Human Review Queue | Horizontal (add reviewers) | Reviewer pool management |
| API Layer | Horizontal (add instances) | Load balancer |

---

## 16. RESILIENCE AND RECOVERY

### 16.1 Failure Scenarios

| Scenario | Impact | Recovery |
|---|---|---|
| Orchestrator Worker Crash | In-progress workflows pause | Temporal auto-restarts from last checkpoint |
| Temporal Server Failure | All workflows affected | Temporal High Availability (multi-node) |
| Kafka Broker Failure | Task dispatch delayed | Kafka replication + controller re-election |
| Database Failure | State queries fail | Read replica failover |
| Redis Failure | Hot state lost | Rebuild from PostgreSQL |
| Network Partition | Agent communication fails | Circuit breaker + retry queue |

### 16.2 Recovery Procedures

```yaml
recovery_procedures:
  worker_crash:
    detection: Temporal server detects heartbeat timeout
    recovery: Auto-restart workflow from last checkpoint
    duration: < 30 seconds
    
  temporal_server_failure:
    detection: Temporal client connection timeout
    recovery: 
      - Wait for Temporal leader election (30s)
      - Workflows resume automatically
    duration: < 2 minutes
    
  kafka_failure:
    detection: Produce/consume errors
    recovery:
      - Messages persisted in Kafka for 7 days
      - Consumer resumes from last committed offset
    duration: < 1 minute
    
  database_failure:
    detection: Connection errors
    recovery:
      - Read replicas promoted if primary fails
      - Connection pooling handles failover
    duration: < 1 minute
    
  full_orchestrator_failure:
    detection: All health checks fail
    recovery:
      - Kubernetes reschedules pods
      - Temporal replays workflow state
      - In-flight tasks resume or timeout
    duration: < 5 minutes
```

---

## 17. ORCHESTRATOR INTEGRATION POINTS

### 17.1 Integration Catalog

| Integration | Direction | Protocol | Purpose |
|---|---|---|---|
| API Gateway | Inbound | gRPC/REST | Accept workflow execution requests |
| Agent Kafka Topics | Outbound | Kafka | Dispatch tasks to agents |
| Agent Result Topics | Inbound | Kafka | Receive agent results |
| Temporal Server | Internal | gRPC | Workflow engine operations |
| PostgreSQL | Internal | SQL | State persistence |
| Redis | Internal | Redis | Hot state, locks |
| Knowledge Graph Service | Outbound | gRPC | Store verified knowledge |
| Notification Service | Outbound | gRPC | Send notifications |
| User Service | Outbound | gRPC | User info for context |
| Analytics System | Outbound | Kafka | Emit analytics events |

### 17.2 Integration Sequence Diagrams

#### 17.2.1 Book Ingestion Workflow

```
    User/Admin          Orchestrator        OCR Agent          Entity Agent     Fact Agent    Verify Agent    KG Service
       |                     |                  |                   |              |             |               |
       |-- Start Workflow -->|                  |                   |              |             |               |
       |                     |-- Dispatch ---->|                   |              |             |               |
       |                     |  (OCR Task)     |                   |              |             |               |
       |                     |                  |-- Process ------>|              |             |               |
       |                     |<----- Result ---|                   |              |             |               |
       |                     |                                     |              |             |               |
       |                     |-- Dispatch ----------------------->|              |             |               |
       |                     |  (Entity Task)                      |-- Process -->|             |               |
       |                     |<------------------ Result ---------|              |             |               |
       |                     |                                                      |             |               |
       |                     |-- Dispatch ---------------------------------------->|             |               |
       |                     |  (Fact Task)                                        |-- Process-->|               |
       |                     |<----------------------- Result ---------------------|             |               |
       |                     |                                                                      |               |
       |                     |-- Dispatch -------------------------------------------------------->|               |
       |                     |  (Verify Task)                                                        |-- Process-->|
       |                     |<---------------------------- Result --------------------------------|               |
       |                     |                                                                                   |
       |                     |-- Store to KG ------------------------------------------------------------------>|
       |                     |                                                                                   |
       |                     |-- Generate Content (parallel fan-out to all content agents)                       |
       |                     |                                                                                   |
       |<---- Workflow Done -|                                                                                   |
```

#### 17.2.2 AI Teacher Interaction

```
    User              API Gateway         AI Teacher Agent    Orchestrator    RAG/KG
     |                     |                     |                 |            |
     |-- Question -------->|                     |                 |            |
     |                     |-- Route to Agent -->|                 |            |
     |                     |                     |                 |            |
     |                     |                     |-- Retrieve ---->|            |
     |                     |                     |  Context        |            |
     |                     |                     |                               |
     |                     |                     |<--- Facts + Context ---------|
     |                     |                     |                 |            |
     |                     |                     |-- Generate      |            |
     |                     |                     |  Response       |            |
     |                     |                     |  (LLM)          |            |
     |                     |                     |                 |            |
     |                     |                     |-- Log           |            |
     |                     |                     |  Interaction    |            |
     |                     |                     |                 |            |
     |<-- Streaming Response ------------------|                 |            |
```

---

## 18. WORKFLOW DESIGNER INTERFACE

### 18.1 Admin Interface

The Master Orchestrator provides an administrative interface for:

1. **Workflow Management**
   - View all active, completed, and failed workflows
   - Filter by type, status, priority, time range
   - Cancel or pause workflows
   - Re-run failed workflows with modifications

2. **Workflow Definition Editor**
   - Visual workflow designer (DAG editor)
   - YAML/JSON editor for advanced users
   - Version management and rollback
   - A/B testing configuration

3. **Agent Monitoring**
   - Agent health status dashboard
   - Queue depth visualization
   - Latency and error rate charts
   - Resource utilization metrics

4. **Human Review Management**
   - Review queue dashboard
   - Reviewer assignment configuration
   - Escalation management
   - Review statistics and quality metrics

5. **Cost Management**
   - Cost dashboard by workflow/agent/tenant
   - Budget configuration
   - Cost anomaly detection
   - Optimization recommendations

---

## 19. TESTING AND VALIDATION

### 19.1 Testing Strategy

| Test Type | What is Tested | How | Automation |
|---|---|---|---|
| **Unit** | Individual orchestrator components | Mock dependencies, test each component | Fully automated |
| **Integration** | Workflow execution with mocked agents | Temporal test framework | Fully automated |
| **Workflow** | Complete workflow definitions | In-memory Temporal server | Fully automated |
| **Resilience** | Failure recovery, compensation | Inject failures, verify recovery | Semi-automated |
| **Performance** | Throughput, latency under load | Load testing with simulated agents | Fully automated |
| **Chaos** | Random failures, network issues | Chaos engineering | Semi-automated |

### 19.2 Workflow Test Cases

```yaml
test_book_ingestion:
  description: "Test complete book ingestion workflow"
  
  test_cases:
    - id: happy_path
      input: { book_id: "test-001", priority: 5 }
      expected: { status: "COMPLETED", steps_completed: 7 }
    
    - id: ocr_fails_then_recovers
      input: { book_id: "test-002", priority: 5 }
      inject_failures:
        - step: ocr_processing
          failure: "AgentTimeout"
          after_attempts: 2
      expected: { status: "COMPLETED", retries: 1 }
    
    - id: ocr_fails_permanently
      input: { book_id: "test-003", priority: 5 }
      inject_failures:
        - step: ocr_processing
          failure: "PermanentFailure"
      expected: { status: "FAILED", compensations_executed: true }
    
    - id: human_review_required
      input: { book_id: "test-004", priority: 5 }
      inject:
        - step: verification
          behavior: "request_human_review"
      expected: { status: "BLOCKED", human_review_required: true }
```

---

## 20. DEPLOYMENT AND OPERATIONS

### 20.1 Deployment Architecture

```
    +--------------------------------------------------------+
    |                  KUBERNETES CLUSTER                      |
    |                                                         |
    |  +--------------------------------------------------+  |
    |  | orchestrator-system Namespace                     |  |
    |  |                                                  |  |
    |  | orchestrator-api (3+ replicas)                   |  |
    |  | orchestrator-worker (10+ replicas)               |  |
    |  | orchestrator-scheduler (2 replicas)              |  |
    |  +--------------------------------------------------+  |
    |                                                         |
    |  +--------------------------------------------------+  |
    |  | temporal-system Namespace                          |  |
    |  |                                                  |  |
    |  | temporal-server (3 replicas)                     |  |
    |  | temporal-admin (1 replica)                       |  |
    |  | temporal-web (1 replica)                         |  |
    |  +--------------------------------------------------+  |
    |                                                         |
    |  +--------------------------------------------------+  |
    |  | data Namespace                                    |  |
    |  |                                                  |  |
    |  | postgresql-orchestrator (primary + 2 replicas)   |  |
    |  | redis-orchestrator (cluster mode)                |  |
    |  +--------------------------------------------------+  |
    +--------------------------------------------------------+
```

### 20.2 Operational Runbooks

| Scenario | Runbook |
|---|---|
| Worker Pod Crash | "Automatic: Kubernetes restarts pod. Temporal replays workflow from last checkpoint. Monitor: verify workflows resume within 30s." |
| High Queue Depth | "1. Check agent health. 2. Scale agent pool. 3. If agents healthy, increase consumer parallelism. 4. If persistent, pause low-priority workflows." |
| Failed Workflow | "1. Check error details in orchestrator logs. 2. Determine if transient or permanent. 3. If transient: retry. 4. If permanent: check compensation executed, notify relevant team." |
| Cost Spike | "1. Check cost dashboard for abnormal patterns. 2. Identify workflow/agent causing spike. 3. Check for loops, excessive retries. 4. Apply budget limits if needed. 5. Root cause analysis." |

---

## 21. FUTURE EVOLUTION

### 21.1 Planned Enhancements

| Enhancement | Timeline | Description |
|---|---|---|
| **Dynamic Workflow Optimization** | Year 2 | Orchestrator learns optimal agent selection and workflow patterns from historical execution data |
| **Cross-Region Orchestration** | Year 2 | Workflows that span multiple geographic regions with data locality awareness |
| **Automatic Workflow Generation** | Year 3 | AI-powered workflow creation from natural language descriptions |
| **Predictive Scaling** | Year 3 | ML-based prediction of workflow volume for proactive resource scaling |
| **Self-Healing Workflows** | Year 3 | Orchestrator automatically detects and fixes common workflow failures |
| **Multi-Tenant Isolation** | Year 2 | Tenant-level resource quotas and performance isolation |

### 21.2 Architecture Evolution Path

```
    Year 1: Basic Temporal Workflows
    - Manual workflow definitions
    - Simple agent dispatch
    - Basic error handling
    
    Year 2: Intelligent Orchestration
    - Dynamic agent selection
    - Predictive scaling
    - Multi-tenant isolation
    
    Year 3: Autonomous Orchestration
    - Self-healing workflows
    - AI-generated workflows
    - Cross-region coordination
```

---

## 22. APPENDICES

### Appendix A: Workflow Definition Template

```yaml
workflow:
  id: "unique-workflow-id"
  name: "Human-Readable Name"
  version: "1.0.0"
  description: "Clear description of what this workflow does"
  
  input_schema:
    param1: { type: string, required: true, description: "..." }
    param2: { type: integer, default: 10, description: "..." }
  
  max_execution_time: "24h"
  max_cost_usd: 100.00
  priority: 5  # 1 (critical) to 10 (background)
  
  steps:
    - id: step_1
      type: activity  # or agent_task or sub_workflow or signal
      activity: activity_name
      agent_type: agent-type-name  # if agent_task
      input:
        param1: "{input.param1}"
        param2: "{steps.step_1.result}"
      depends_on: []
      timeout: "1h"
      retry:
        max_attempts: 3
        initial_interval: "5s"
        backoff_coefficient: 2.0
      compensation: compensation_action_name
      
  compensation_steps:
    - on_failure: [any]
      actions:
        - activity: cleanup_action
          params: { ... }
```

### Appendix B: Orchestrator Configuration

```yaml
orchestrator:
  server:
    host: "0.0.0.0"
    port: 8080
    grpc_port: 9090
    
  temporal:
    host: "temporal-server.temporal-system:7233"
    namespace: "iep-production"
    task_queue: "orchestrator-tasks"
    
  kafka:
    brokers: ["kafka-0:9092", "kafka-1:9092", "kafka-2:9092"]
    producer:
      acks: "all"
      compression: "snappy"
      linger_ms: 5
    consumer:
      group_id: "orchestrator-consumer"
      auto_offset_reset: "earliest"
      enable_auto_commit: false
      
  state:
    postgresql:
      host: "postgresql-orchestrator.data"
      port: 5432
      database: "orchestrator_state"
      pool_size: 50
    redis:
      host: "redis-orchestrator.data"
      port: 6379
      cluster_mode: true
      
  limits:
    max_concurrent_workflows: 10000
    max_concurrent_tasks: 50000
    max_queue_depth_per_agent: 5000
    max_cost_per_workflow: 100.00
    
  monitoring:
    metrics_port: 9090
    tracing_endpoint: "jaeger-collector.monitoring:4317"
    
  human_review:
    default_deadline: "24h"
    escalation_levels:
      - level: 1
        deadline: "24h"
      - level: 2
        deadline: "12h"
      - level: 3
        deadline: "6h"
```

### Appendix C: Glossary

| Term | Definition |
|---|---|
| Orchestrator | Central coordinator that manages workflows and dispatches tasks to agents |
| Workflow | A defined sequence of steps executed to accomplish a goal |
| Activity | A single unit of work executed by the workflow engine |
| Agent Task | A task dispatched to an AI agent for execution |
| Saga | A sequence of local transactions with compensating actions |
| Compensation | An action that reverses the effects of a previous failed step |
| Human-in-Loop | A workflow step that requires human judgment |
| Temporal | The workflow engine used for durable execution |
| Fan-Out | Dispatching multiple tasks in parallel |
| Dead Letter Queue | Queue for messages that failed processing |

### Appendix D: Related Documents

| Document ID | Title | Relationship |
|---|---|---|
| IEP-CONST-001 | Project Constitution | Governing principles |
| IEP-ARCH-001 | System Architecture | Platform architecture context |
| IEP-ARCH-AGENT-001 | Agent Architecture | Agent design and lifecycle |
| IEP-DEPLOY-001 | Deployment Architecture | Orchestrator deployment |
| IEP-MONITOR-001 | Monitoring Architecture | Orchestrator monitoring |
| IEP-SECURITY-001 | Security Architecture | Orchestrator security |

---

**END OF MASTER ORCHESTRATOR**

---

*This document defines the Master Orchestrator architecture for the Islamic Education Platform. All workflow definitions and agent interactions must conform to this architecture. Questions regarding this document should be directed to the Office of the Chief Architect.*
