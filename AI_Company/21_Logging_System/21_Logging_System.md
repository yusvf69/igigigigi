# LOGGING SYSTEM

## Enterprise Observability and Audit Architecture for the AI Workforce

---

| Document ID | AI-CORP-LOG-001 |
|---|---|
| Version | 1.0.0 |
| Status | Ratified |
| Classification | Enterprise Internal — Highly Confidential |
| Effective Date | 2026-07-09 |
| Expiry Date | Not Applicable |
| Approving Authority | Office of the Chief Architect |
| Custodian | CIO-AI (Chief Infrastructure AI) / Monitoring Department |
| Last Review Date | 2026-07-09 |
| Overview Reference | AI-CORP-OVW-001 |
| Related Documents | AI-CORP-MON-001 (Monitoring), AI-CORP-PERF-001 (Performance), AI-CORP-COST-001 (Cost), AI-CORP-SEC-001 (Security) |

---

## TABLE OF CONTENTS

1. LOGGING PHILOSOPHY
2. LOG CATEGORIES AND TYPES
3. LOG FORMAT AND SCHEMA
4. LOG GENERATION
5. LOG INGESTION PIPELINE
6. LOG STORAGE ARCHITECTURE
7. LOG QUERYING AND SEARCH
8. LOG RETENTION AND LIFECYCLE
9. LOG PRIVACY AND SECURITY
10. LOG MONITORING AND ALERTING
11. AUDIT LOGGING
12. LOGGING FOR DEBUGGING AND RCA
13. LOGGING COSTS AND OPTIMIZATION
14. LOGGING FOR ML/ANALYTICS

---

## 1. LOGGING PHILOSOPHY

### 1.1 Foundational Principles

The AI Company operates 8,500 autonomous AI agents executing millions of tasks daily to build an Islamic Education Platform. Every action, decision, communication, and state change across this workforce must be recorded with absolute fidelity. The logging system is not merely a technical subsystem; it is the company's institutional memory, its audit backbone, and its primary tool for debugging, optimization, and compliance.

| Principle | Statement | Rationale |
|---|---|---|
| **Every Action Logged** | No agent action occurs without a corresponding log entry | Complete reconstructability of any event chain |
| **Immutability** | Once written, logs are never modified, deleted, or overwritten | Audit integrity, non-repudiation, legal compliance |
| **Append-Only** | All writes are appends to existing log streams; no in-place updates | Sequential write performance, no corruption from partial overwrites |
| **Timestamped** | Every log carries a nanosecond-precision ISO 8601 timestamp | Precise event ordering across distributed agents |
| **Structured** | All logs are structured JSON; no free-text, no ad-hoc formatting | Machine-parsable, queryable, aggregatable |
| **Categorized** | Every log belongs to exactly one category | Efficient routing, storage tiering, and query filtering |
| **Privacy-First** | No PII, secrets, API keys, or sensitive content in logs | GDPR/PDPL compliance, security posture |
| **Tiered Retention** | Retention duration varies by log category | Cost optimization without compliance gaps |

### 1.2 Design Goals

| Goal | Metric | Target |
|---|---|---|
| **Write Throughput** | Logs ingested per second | 500,000+ (peak) |
| **Write Latency** | Time from agent emission to durable storage | < 500ms (p99) |
| **Query Latency (Hot)** | Hot tier query response | < 1s |
| **Query Latency (Warm)** | Warm tier query response | < 10s |
| **Query Latency (Cold)** | Cold tier query response | < 60s |
| **Data Durability** | Probability of log loss | 0% (zero tolerance) |
| **Storage Efficiency** | Compression ratio | 5:1 (text), 10:1 (JSON) |
| **Uptime** | Log pipeline availability | 99.99% |
| **Audit Completeness** | Percentage of auditable actions logged | 100% |

### 1.3 Four Pillars of Logging

The logging system serves four distinct but overlapping purposes:

| Pillar | Purpose | Primary Consumers | Retention |
|---|---|---|---|
| **Operational** | Day-to-day system health, task tracking, performance | Monitoring agents, Operations teams | 7–90 days |
| **Audit** | Compliance, non-repudiation, regulatory evidence | Compliance agents, Board, Regulators | 7 years |
| **Security** | Threat detection, incident investigation, access tracking | Security agents, Incident Response | 7 years |
| **Debug** | Root cause analysis, development, optimization | Engineering agents, RCA teams | 24h–7 days |

### 1.4 Logging Constraints and Boundaries

| Constraint | Policy | Enforcement |
|---|---|---|
| **No PII** | Any field matching PII patterns is redacted before log emission | Agent-side pre-processor |
| **No Secrets** | API keys, tokens, passwords are masked | Automatic detection in log pipeline |
| **No Infinite Logs** | Maximum log size per agent per hour: 100MB | Agent-side circuit breaker |
| **No Blocking** | Log writes never block agent execution | Async-only logging, fire-and-forget |
| **No Payload Bloat** | Maximum log entry size: 64KB | Pipeline-level rejection of oversized entries |
| **No Raw Content** | Never log full user messages, agent output, or Islamic content text | Content hashes referenced, never raw text |

---

## 2. LOG CATEGORIES AND TYPES

### 2.1 Category Hierarchy

Every log belongs to exactly one primary category. Categories are mutually exclusive at the top level but can be cross-referenced through correlation IDs and trace IDs.

```
Log Categories
├── Operational
│   ├── TaskStart
│   ├── TaskEnd
│   ├── StateTransition
│   ├── ResourceUsage
│   └── PerformanceMetric
├── Audit
│   ├── ActionLog
│   ├── ComplianceEvent
│   ├── PolicyEnforcement
│   └── ApprovalEvent
├── Security
│   ├── AuthEvent
│   ├── PermissionChange
│   ├── AccessDenial
│   └── AnomalyFlag
├── Debug
│   ├── ReasoningTrace
│   ├── IntermediateOutput
│   ├── DecisionStep
│   └── ToolCallDetail
├── Error
│   ├── ExceptionDetail
│   ├── StackTrace
│   ├── ErrorContext
│   └── RecoveryAction
├── Business
│   ├── ContentCreated
│   ├── UserInteraction
│   ├── RevenueEvent
│   └── EngagementMetric
├── System
│   ├── InfrastructureEvent
│   ├── ScalingAction
│   ├── DeploymentEvent
│   └── ConfigChange
├── Communication
│   ├── InterAgentMessage
│   ├── HandoverEvent
│   ├── EscalationEvent
│   └── BroadcastMessage
├── Quality
│   ├── VerificationResult
│   ├── QualityScore
│   ├── UserFeedback
│   └── ReviewOutcome
└── Cost
    ├── ResourceConsumption
    ├── APIUsage
    ├── ComputeTime
    └── BudgetEvent
```

### 2.2 Operational Logs

Operational logs track the execution flow of all agent tasks and system processes. These are the highest-volume logs and form the primary basis for monitoring dashboards.

| Log Type | Sub-Type | Trigger | Frequency | Key Fields |
|---|---|---|---|---|
| **TaskStart** | — | Agent begins task execution | Every task start | task_id, workflow_id, priority, queue_wait_time_ms |
| **TaskStart** | ScheduledTask | Scheduled workflow initiated | Per cron trigger | schedule_id, expected_start_time, actual_start_time |
| **TaskStart** | Retry | Retry after failure | Per retry attempt | retry_count, max_retries, previous_error |
| **TaskEnd** | Success | Task completes successfully | Every task end | task_id, duration_ms, result_summary |
| **TaskEnd** | Failure | Task fails | Every task failure | task_id, duration_ms, error_code, failure_reason |
| **StateTransition** | AgentState | Agent changes lifecycle state | Per state change | from_state, to_state, reason, trigger |
| **StateTransition** | WorkflowState | Workflow progresses to next stage | Per stage change | workflow_id, prev_stage, next_stage, decision |
| **ResourceUsage** | CPU | CPU utilization snapshot | Every 30s per agent | utilization_pct, cores_available, throttled |
| **ResourceUsage** | Memory | Memory utilization snapshot | Every 30s per agent | used_bytes, total_bytes, heap_pct |
| **ResourceUsage** | GPU | GPU utilization snapshot | Every 60s per agent | memory_used_mb, utilization_pct, temperature_c |
| **PerformanceMetric** | Latency | API call latency measurement | Per API call | endpoint, method, p50_ms, p95_ms, p99_ms |
| **PerformanceMetric** | Throughput | Tasks per second measurement | Every 60s | tasks_per_sec, active_agents, queue_length |
| **PerformanceMetric** | ErrorRate | Error percentage measurement | Every 60s | total_tasks, error_count, error_pct |

### 2.3 Audit Logs

Audit logs capture every action with compliance, regulatory, or governance significance. They are the most critical logs for integrity and must be tamper-evident.

| Log Type | Description | Trigger | Retention | Tamper-Evidence |
|---|---|---|---|---|
| **ActionLog** | Any action that changes system state | Every state-changing operation | 7 years | Hash chain entry |
| **ComplianceEvent** | Compliance-relevant action (data access, content publishing) | Every compliance-scoped action | 7 years | Digital signature |
| **ApprovalEvent** | Human or system approval of significant action | Every approval grant/denial | 7 years | Signed + hashed |
| **DataAccess** | Access to sensitive data (PII, Islamic content) | Every read of restricted data | 7 years | Aggregated proof |
| **ContentModification** | Creation, update, or deletion of learner-facing content | Every content mutation | 7 years | Version hash chain |
| **PolicyEnforcement** | Application of a policy rule | Every policy evaluation | 7 years | Policy hash + result |
| **ConsentEvent** | User consent grant, withdrawal, or update | Every consent change | 7 years | User verification hash |

### 2.4 Security Logs

Security logs focus on authentication, authorization, access control, and threat detection.

| Log Type | Description | Severity Base | Auto-Alert | Escalation |
|---|---|---|---|---|
| **AuthEvent** | Login, logout, session creation/expiry | INFO | No | No |
| **AuthFailure** | Failed authentication attempt | WARN | > 5/min | Security agent |
| **PermissionChange** | Role assignment, permission grant/revoke | INFO | Yes | Manager |
| **PermissionDenial** | Authenticated user denied access to resource | WARN | > 10/min | Security agent |
| **TokenIssuance** | API key or auth token creation | INFO | No | No |
| **TokenRevocation** | Token invalidation | WARN | Yes | Security agent |
| **AnomalyFlag** | Behavioral anomaly detected by security system | CRITICAL | Yes | Incident Response |
| **ThreatIntel** | External threat indicator matched | CRITICAL | Yes | Incident Response |
| **RateLimitHit** | API rate limit exceeded | WARN | Yes | Security agent |
| **IPBlock** | IP address blocked | WARN | Yes | Security agent |
| **DataExfilAttempt** | Unusual data transfer pattern detected | FATAL | Yes | CE-AI + Incident Response |

### 2.5 Debug Logs

Debug logs contain detailed reasoning traces, decision steps, and intermediate outputs. They are high-volume, low-retention, and sampled in production.

| Log Type | Description | Volume | Sampling | Retention |
|---|---|---|---|---|
| **ReasoningTrace** | Full chain-of-thought from LLM agent | Very high | 1% (stable agents), 100% (probation) | 24 hours |
| **DecisionStep** | Each step in multi-step reasoning | High | 5% | 48 hours |
| **IntermediateOutput** | Output at each processing stage | High | 5% (sampled only) | 24 hours |
| **ToolCallDetail** | Input/output of every tool invocation | Medium | 10% | 48 hours |
| **PromptSnapshot** | Full prompt sent to LLM (redacted) | Medium | 5% | 72 hours |
| **ResponseSnapshot** | Full LLM response (redacted) | Medium | 5% | 72 hours |
| **MemoryAccess** | Agent memory read/write operations | Very high | 0.1% | 12 hours |
| **RetrievalDetail** | Knowledge base retrieval query and results | Medium | 5% | 48 hours |
| **ValidationStep** | Validation rule checks and outcomes | Medium | 10% | 48 hours |

### 2.6 Error Logs

Error logs capture all exception conditions, failures, and unexpected states. Every error must include sufficient context for RCA without requiring cross-referencing.

| Log Type | Severity | Description | Required Fields |
|---|---|---|---|
| **ExceptionDetail** | ERROR | Any caught exception in agent code | agent_id, trace_id, exception_type, message, source_file, line_number |
| **Stacktrace** | ERROR | Full stack trace of exception | stack_trace, root_cause, frames[] |
| **ErrorContext** | ERROR | Context surrounding error (state, inputs, prior actions) | context_data, state_snapshot, relevant_log_ids |
| **FailureAction** | ERROR | What the agent was doing when failure occurred | action, params, stage, progress_pct |
| **TransientError** | WARN | Recoverable/temporary error | retriable, retry_count, backoff_ms |
| **TimeoutError** | ERROR | Operation exceeded time limit | timeout_sec, actual_ms, operation |
| **ValidationError** | WARN | Input/output failed validation | validation_rule, expected, actual, field |
| **FatalError** | FATAL | Non-recoverable error requiring escalation | escalation_level, notified_agents, recovery_plan |

### 2.7 Business Logs

Business logs capture platform-level business events related to content, users, and revenue.

| Log Type | Description | Who Consumes | Sensitivity |
|---|---|---|---|
| **ContentCreated** | New educational content published | Content managers, Analytics | Medium (no raw content) |
| **ContentUpdated** | Existing content revised | Content managers, Verification | Low |
| **ContentDeleted** | Content removed (soft-delete) | Content managers, Compliance | Medium |
| **UserRegistration** | New user signs up | Analytics | High (PII redacted) |
| **UserSession** | User session start/end/duration | Analytics | High (anonymized) |
| **UserInteraction** | User action within platform (quiz, flashcard, lecture) | Analytics, ML | High (anonymized) |
| **EngagementMetric** | Daily/weekly/monthly engagement aggregates | Analytics | Low (aggregates only) |
| **RevenueEvent** | Subscription, purchase, or payment event | Finance agents | High (PII redacted) |
| **ConversionEvent** | Free-to-paid conversion | Analytics, Finance | Medium |
| **SubscriptionEvent** | Subscription start, cancel, renewal | Finance, Analytics | Low |
| **FeatureUse** | Feature adoption metric | Product analytics | Low |

### 2.8 System Logs

System logs cover infrastructure, deployment, scaling, and configuration events.

| Log Type | Description | Frequency | Owner |
|---|---|---|---|
| **InfrastructureEvent** | Compute, network, storage health events | Every 60s aggregate | Data Infrastructure team |
| **ScalingAction** | Auto-scaling trigger, action, result | Per scaling event | Deployment team |
| **DeploymentEvent** | Build, test, deploy, rollback | Per deployment | CI/CD pipeline |
| **ConfigChange** | Configuration modification | Per change | All teams |
| **NodeEvent** | Node join, leave, health change | Per node state change | Infrastructure team |
| **ClusterEvent** | Cluster membership, leader election | Per cluster change | Infrastructure team |
| **ServiceDiscovery** | Service register, deregister, health check | Per discovery event | Network team |
| **BackupEvent** | Backup start, complete, failure | Per backup | Data Infra team |
| **MaintenanceEvent** | Planned maintenance start/end | Per maintenance window | Infrastructure team |
| **DisasterRecovery** | DR failover, failback | Per DR event | Infrastructure team |

### 2.9 Communication Logs

Communication logs capture every inter-agent message, handover, broadcast, and escalation. They are essential for reconstructing multi-agent workflows.

| Log Type | Protocol | Fields | Retention |
|---|---|---|---|
| **InterAgentMessage** | Message Bus (Kafka/RabbitMQ) | sender_id, receiver_id, message_type, correlation_id, payload_hash | 30 days |
| **HandoverEvent** | Handover protocol | from_agent, to_agent, task_id, handover_reason, context_hash | 90 days |
| **EscalationEvent** | Escalation protocol | from_agent, level, reason, context, acknowledged | 7 years |
| **BroadcastMessage** | Pub/Sub broadcast | sender_id, topic, subscriber_count, payload_hash | 7 days |
| **MessageDeliveryFailure** | Bus system | message_id, failure_reason, retry_count, dead_letter_queue | 90 days |
| **WorkflowCoordination** | Workflow engine | workflow_id, step_id, participating_agents, step_duration | 90 days |
| **AgentResponse** | Synchronous agent call | caller, responder, correlation_id, response_time_ms | 30 days |

### 2.10 Quality Logs

Quality logs track verification results, quality scores, and user feedback for continuous improvement.

| Log Type | Description | Thresholds | Action on Low Score |
|---|---|---|---|
| **VerificationResult** | Fact-checking outcome | match_pct, sources_checked, verifier_agent_id | Escalate, regenerate |
| **QualityScore** | Composite quality metric per content item | overall_score, accuracy, fluency, relevance, alignment | Escalate if < 0.85 |
| **UserFeedback** | Direct user rating, thumbs up/down, comment | rating (1–5), feedback_category | Aggregate for model training |
| **UserFeedbackFlag** | User flags content as incorrect or inappropriate | flag_reason, content_id, user_id | Immediate review escalation |
| **ReviewOutcome** | Human or AI review result | reviewer_type, approved, changes_requested, review_notes | Track review efficacy |
| **AutoReviewOutcome** | Automated quality check result | check_type, passed, score, dimension_scores | Pass/fail for pipeline |
| **RegressionCheck** | Regression test result | test_id, result, expected, actual, diff | Block deployment if fail |
| **ScholarReview** | Islamic scholar review outcome | scholar_id, status, fatwa_reference, authentication_hash | Mandatory for Q1 content |

### 2.11 Cost Logs

Cost logs track all resource consumption, API usage, and compute time at a per-task and per-agent granularity.

| Log Type | Description | Fields | Purpose |
|---|---|---|---|
| **ResourceConsumption** | CPU, memory, GPU used by agent | agent_id, task_id, cpu_seconds, memory_mb_seconds, gpu_seconds | Cost allocation, chargeback |
| **APIUsage** | External API calls made during task | provider, endpoint, calls, cost_per_call, total_cost | API cost tracking |
| **LLMUsage** | LLM inference calls | model, prompt_tokens, completion_tokens, total_tokens, cost | LLM cost tracking |
| **ComputeTime** | Wall clock and compute time per task | task_id, wall_clock_ms, compute_ms, idle_ms, wait_ms | Efficiency analysis |
| **StorageUsage** | Storage consumed per agent/workflow | storage_type, bytes_stored, days_stored, cost | Storage cost allocation |
| **NetworkCost** | Data transfer costs | direction, bytes, region, cost_per_gb, total_cost | Network cost allocation |
| **BudgetEvent** | Budget check or cap enforcement | budget_id, limit, current_spend, action_taken | Budget control |
| **CostAnomaly** | Unusual cost pattern detected | baseline, deviation_pct, suspected_cause, recommendation | Cost optimization |
| **TokenBudget** | Per-agent token allocation and usage | agent_id, allocated_tokens, used_tokens, remaining, reset_time | Token budget enforcement |

---

## 3. LOG FORMAT AND SCHEMA

### 3.1 Universal Log Schema

Every log in the system follows a strict JSON schema. No deviation is permitted. Fields are divided into required and optional, and every field has a defined type, format, and validation rule.

| Field | Type | Required | Format/Constraints | Description |
|---|---|---|---|---|
| **log_schema_version** | string | Yes | Semantic version, e.g., `1.0.0` | Version of the log schema used |
| **timestamp** | string | Yes | ISO 8601 with nanoseconds: `2026-07-09T14:30:00.123456789Z` | Exact time of log generation |
| **level** | string | Yes | Enum: `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `FATAL` | Log severity level |
| **logger_name** | string | Yes | `organization.department.team.agent_role` e.g. `ai_company.learning.story.writer_agent` | Source component identifier |
| **agent_id** | string | Yes | UUID v7 format, e.g. `01J3F2A0B1C2D3E4F5G6H7I8J9` | Unique identifier of the agent generating the log |
| **trace_id** | string | Yes | UUID v7, e.g. `01J3F2A0B1C2D3E4F5G6H7I8J9` | End-to-end trace identifier for request/transaction |
| **span_id** | string | Yes | UUID v7 | Specific span within a trace |
| **category** | string | Yes | Enum: `operational`, `audit`, `security`, `debug`, `error`, `business`, `system`, `communication`, `quality`, `cost` | Primary log category |
| **log_type** | string | Yes | Enum as defined in section 2, e.g. `TaskStart`, `AuthEvent`, `ReasoningTrace` | Specific type within category |
| **message** | string | Yes | Max 1024 chars, no PII, no secrets | Human-readable log summary |
| **data** | object | Yes | Arbitrary JSON, max 64KB, validated at pipeline | Structured payload specific to log type |
| **source** | object | Yes | See source sub-fields below | Where the log originated (region, host, process) |
| **environment** | string | Yes | Enum: `production`, `staging`, `development`, `test`, `shadow` | Deployment environment |

### 3.2 Source Sub-Fields (Required)

| Field | Type | Required | Format | Description |
|---|---|---|---|---|
| source.region | string | Yes | e.g. `us-east-1`, `me-south-1` | AWS region of origin |
| source.host_id | string | Yes | e.g. `ip-10-0-1-234.ec2.internal` | Host or container ID |
| source.process_id | int | Yes | e.g. `42391` | PID of generating process |
| source.thread_id | int | Yes | e.g. `23` | Thread ID |
| source.service | string | Yes | e.g. `agent-runtime`, `message-bus` | Service name |
| source.version | string | Yes | e.g. `2.3.1` | Service version |

### 3.3 Optional Fields

| Field | Type | Max Length/Format | Description |
|---|---|---|---|
| team_id | string | UUID v7 | Team that owns the agent |
| dept_id | string | e.g. `D-STR` | Department identifier |
| workflow_id | string | UUID v7 | Workflow instance identifier |
| task_id | string | UUID v7 | Specific task identifier |
| parent_span_id | string | UUID v7 | Parent span in distributed trace |
| correlation_id | string | UUID v7 | Cross-system correlation identifier |
| user_id | string | Hashed UUID (SHA-256) | Hashed user identifier (never raw) |
| content_id | string | UUID v7 | Content item identifier |
| session_id | string | UUID v7 | User session identifier |
| duration_ms | int | > 0 | Duration of measured operation |
| tokens_used | int | > 0 | Token count for LLM operations |
| model_version | string | e.g. `gpt-4o-2026-05-15` | AI model version used |
| parent_trace_id | string | UUID v7 | Parent trace for merged workflows |
| retry_count | int | >= 0 | Retry attempt number |
| priority | int | 1-10 | Priority level of task |
| tags | string[] | Array of strings, max 10 tags | Arbitrary tags for filtering |
| error_code | string | e.g. `ERR_LLM_TIMEOUT`, `ERR_VALIDATION_FAILED` | Machine-readable error code |
| performance_budget_ms | int | > 0 | Expected duration budget |

### 3.4 Log Levels

Each log level has specific semantics and routing behavior.

| Level | Numeric Value | Semantics | Storage Tier | Auto-Alert |
|---|---|---|---|---|
| TRACE | 1000 | Step-by-step execution details | Hot only (24h) | Never |
| DEBUG | 500 | Detailed diagnostic information | Hot only (7d) | Never |
| INFO | 300 | Normal operational events | Hot (30d) | Never |
| WARN | 200 | Something unexpected but recoverable | Hot (30d) → Warm (90d) | Configurable |
| ERROR | 100 | Operation failed, system partially degraded | Hot (7d) → Warm (90d) → Cold (7yr) | Always |
| FATAL | 0 | Non-recoverable, system impact imminent | Hot (7d) → Warm (90d) → Cold (7yr) | Always (PagerDuty) |

### 3.5 Complete Log Example

```json
{
  "log_schema_version": "1.0.0",
  "timestamp": "2026-07-09T10:30:00.123456789Z",
  "level": "INFO",
  "logger_name": "ai_company.learning.story.writing_agent",
  "agent_id": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
  "trace_id": "trace_8f7e6d5c-4b3a-2109-fedc-ba9876543210",
  "span_id": "span_12345678-90ab-cdef-1234-567890abcdef",
  "category": "operational",
  "log_type": "TaskStart",
  "message": "Story generation task started for content_id c78901d2-e3f4-5678-9abc-def012345678",
  "data": {
    "task_id": "task_98765432-10fe-dcba-0987-6543210fedcb",
    "workflow_id": "wf_12345678-90ab-cdef-1234-567890abcdef",
    "story_parameters": {
      "age_group": "8-12",
      "language": "ar",
      "topic": "prophet_stories_ibrahim",
      "style": "narrative_with_moral",
      "target_word_count": 1500,
      "complexity_level": "intermediate"
    },
    "source_content_hash": "sha256:c1d2e3f4a5b6c7d8e9f0a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c1d",
    "queue_wait_time_ms": 234,
    "assigned_by": "workflow_allocator_agent_v3",
    "deadline_epoch": 20260709T110000Z
  },
  "source": {
    "region": "us-east-1",
    "host_id": "ip-10-243-56-78.ec2.internal",
    "container_id": "container_a1b2c3d4e5f6",
    "pod_name": "story-agent-pool-7-x9y8z7",
    "process_id": 31289,
    "thread_id": 14,
    "service": "story-production-service",
    "version": "2.4.1"
  },
  "environment": "production",
  "team_id": "T-STR-WRITE-03",
  "dept_id": "D-STR",
  "correlation_id": "corr_12345678-90ab-cdef-1234-567890abcdef",
  "tags": ["story", "arabic", "prophet-stories", "age-8-12", "high-priority"],
  "error_code": null,
  "duration_ms": null,
  "tokens_used": null,
  "model_version": "gpt-4o-2026-05-15"
}
```

### 3.6 Schema Versioning

| Log Schema Version | Release Date | Changes |
|---|---|---|
| 1.0.0 | 2026-07-09 | Initial ratified schema |
| 2.0.0 | TBD | Planned: field-level encryption indicator, nested tracing support |

Schema version is validated at the ingestion pipeline. Logs with unsupported schema versions are rejected and sent to a dead-letter queue for manual review.

### 3.7 Schema Validation Rules

| Rule | Validation | Action on Violation |
|---|---|---|
| Required fields present | All required fields non-null | Reject log, log to audit dead-letter |
| Field types match schema | Type check on all fields | Coerce if possible, reject if impossible |
| Field lengths within limits | String lengths, array sizes, object depths | Truncate with warning, reject if exceeding hard limit |
| No PII patterns | Regex check for email, phone, SSN, credit card patterns | Redact field, log to security quarantine |
| No secret patterns | Regex check for API keys, tokens, passwords | Redact, log to security quarantine |
| Timestamp in reasonable range | Within 5 minutes of pipeline time | If future by > 5min → reject; if past > 24hr → reject |
| Data JSON is valid | Parse-check data field | Reject entry |
| Log level valid | Enum check | Default to INFO |
| Agent ID exists in registry | Cross-reference agent_id with agent registry | Allow but flag warning |

---

## 4. LOG GENERATION

### 4.1 Agent Auto-Logging Behavior

Every agent in the AI Company has built-in auto-logging at key lifecycle points. This is not configurable per agent; it is enforced by the agent runtime.

| Lifecycle Point | Log Behavior | Level | Category | Automatic Fields |
|---|---|---|---|---|
| **Agent Start** | Log on initialization | INFO | operational | agent_id, start_reason, configuration_hash |
| **Agent End** | Log on termination | INFO | operational | agent_id, runtime_seconds, tasks_completed, end_reason |
| **Task Entry** | Log on receiving task | INFO | operational | task_id, workflow_id, priority, queue_depth |
| **Task Exit** | Log on completing task | INFO | operational | result, duration_ms, task_id |
| **Task Failure** | Log on task failure | ERROR | error | error_code, failure_reason, retry_count, context |
| **Decision Point** | Log on key branching decisions | DEBUG | debug | decision_type, options, chosen_option, rationale |
| **LLM Call** | Log before/after LLM invocation | DEBUG | cost | model, prompt_tokens, completion_tokens, latency_ms |
| **Tool Call** | Log on external tool invocation | DEBUG | operational | tool_name, input_hash, output_hash, duration |
| **Memory Access** | Log on agent memory read/write | TRACE | debug | access_type, memory_key, value_size |
| **State Change** | Log on any state change | INFO | operational | from_state, to_state, trigger, transition_time |
| **API Call** | Log on external API call | INFO | operational | endpoint, method, status_code, duration_ms |
| **Error Caught** | Log on exception caught | ERROR/INFO | error | exception_type, message, stack_hash, handled |
| **Escalation** | Log on initiating escalation | WARN | communication | escalation_level, reason, target_agent |
| **Handover** | Log on task handover | INFO | communication | target_agent, handover_reason, context_hash |
| **Quality Check** | Log on internal quality validation | INFO | quality | check_type, passed, score, details |

### 4.2 Log Generation Rate Estimates

| Agent Tier | Agents | Avg Logs/s per Agent | Peak Logs/s per Agent | Total Peak Logs/s |
|---|---|---|---|---|
| **Executive (E)** | 37 | 5 | 20 | 740 |
| **Management (M)** | 150 | 10 | 50 | 7,500 |
| **Senior (S)** | 850 | 30 | 150 | 127,500 |
| **Worker (W)** | 3,400 | 50 | 250 | 850,000 |
| **Utility (U)** | 2,550 | 60 | 300 | 765,000 |
| **Support (P)** | 1,513 | 15 | 60 | 90,780 |
| **Probationary (T)** | ~50 | 100 | 500 | 25,000 |
| **Total** | **8,500** | **~500 (avg)** | **240 (avg/peak)** | **~1,866,780 (all peak)** |

### 4.3 Steady-State Throughput

| Metric | Value |
|---|---|
| Average logs/second (steady state) | ~425,000 |
| Peak logs/second (burst) | ~1,870,000 |
| Average log size | 512 bytes |
| Peak log size (99th pct) | 4 KB |
| Average throughput (steady) | ~212 MB/s (~18 TB/day) |
| Peak throughput (sustained) | ~750 MB/s (~65 TB/day) |

### 4.4 Sampling Rules

Sampling is applied at the agent side before emission. Sampling must never drop TRACE logs during active debugging sessions or for probationary agents.

| Condition | Log Levels | Sampling Rate | Mechanism |
|---|---|---|---|
| Stable agent (in production >7 days) | DEBUG | 1% | Random uniform sampling |
| Stable agent (in production >7 days) | TRACE | 0.1% | Random uniform sampling |
| Stable agent (in production >7 days) | INFO | 100% | Always log |
| Stable agent (in production >7 days) | WARN+ | 100% | Always log |
| Probationary agent (<100 tasks) | DEBUG | 100% | Always log |
| Probationary agent (<100 tasks) | TRACE | 50% | Random uniform sampling |
| Agent under active debugging | DEBUG | 100% | Debug flag overrides sampling |
| Agent in training environment | ALL | 100% | Full logging in dev/test |
| ERROR/FATAL (all agents) | ERROR/FATAL | 100% | Never sampled |
| AUDIT category (all agents) | ALL | 100% | Never sampled |
| SECURITY category (all agents) | ALL | 100% | Never sampled |
| COST category (all agents) | ALL | 100% | Never sampled |

### 4.5 Batched Emission

Agents do not emit logs as individual network calls. Logs are buffered and emitted in batches for performance.

| Parameter | Value | Rationale |
|---|---|---|
| **Buffer interval** | 500ms | Balance between latency and throughput |
| **Buffer size (soft)** | 100 KB | Where batch is flushed even if interval not met |
| **Buffer size (hard)** | 1 MB | Maximum before force-flush to prevent OOM |
| **Max entries per batch** | 10,000 | Limit for single network write |
| **Batch compression** | gzip (level 6) | ~80% compression on JSON |
| **Retry on failure** | 3 attempts, exponential backoff | Survive transient network issues |
| **Backpressure reaction** | Drop DEBUG first, then TRACE | Graceful degradation |

### 4.6 Async Logging Guarantees

| Property | Guarantee | Mechanism |
|---|---|---|
| **Non-blocking** | Agent execution never waits for log write | Fire-and-forget, dedicated log writer thread |
| **Ordering** | Logs within a batch maintain order | Batches are ordered; cross-batch ordering not guaranteed |
| **Durability** | Logs survive agent crash | Local 1GB buffer on disk before network send |
| **Latency impact** | < 1ms added to agent execution | Dedicated logging thread, zero-copy serialization |
| **Memory impact** | < 200MB per agent thread | Fixed-size ring buffer, oldest dropped at capacity |

### 4.7 Log Generation Edge Cases

| Scenario | Behavior | Rationale |
|---|---|---|
| **Agent crashes before buffer flush** | Logs in memory buffer are lost; disk buffer logs survive | Acceptable tradeoff for performance; critical logs are immediately flushed |
| **Agent overloaded (backlog > 10K logs)** | Oldest 10% of buffer dropped, WARN+ preserved | Graceful degradation under extreme load |
| **Log size exceeds 64KB** | Truncated to 64KB, warning field added | Pipeline hard limit enforced |
| **Circular log reference** | Maximum nesting depth of 10 levels, then truncated | Prevent infinite recursion in serialization |
| **Binary data in logs** | Base64-encoded, size limit applies | Binary never logged raw |
| **Agent emits 100% ERROR logs** | Agent auto-quarantined, alert sent to manager | Error storm detection |

---

## 5. LOG INGESTION PIPELINE

### 5.1 Pipeline Architecture Overview

```
Agent Runtime ──> Sidecar ──> Local Buffer ──> Log Shipper ──> Kafka ──> Processing ──> Storage
                                                                                        │
                                                                                        ▼
                                                                                  Alerting System
```

Every agent instance runs on a compute node (container/pod/VM). Each node hosts a log sidecar process that is responsible for collecting, buffering, and shipping logs to the central pipeline.

### 5.2 Agent Sidecar

| Component | Specification | Purpose |
|---|---|---|
| **Collection** | Reads logs from agent stdout (JSON line format) | Captures all agent-generated log lines |
| **Structured parsing** | JSON deserializer, validation against schema | Ensures only valid structured logs proceed |
| **Enrichment** | Adds source fields (region, host, container, process) | Context enrichment without agent coordination |
| **Rate limiting** | Token bucket: 1000 logs/s per agent, burst 5000 | Prevents agent from flooding pipeline |
| **Health endpoint** | HTTP /health returns buffer stats, log count, error count | Sidecar health monitoring |
| **Resource usage** | < 100MB RAM, < 5% CPU per sidecar | Minimal overhead on agent node |

### 5.3 Local Buffer

| Parameter | Value | Description |
|---|---|---|
| **Buffer type** | Memory-mapped file | Survives process restart |
| **Soft limit** | 1 GB (disk) | Triggers rotation to new buffer |
| **Hard limit** | 2 GB (disk) | Oldest logs overwritten (ring buffer semantics) |
| **Flush trigger** | Every 5 seconds OR 100MB accumulated | Data sent to central pipeline |
| **Survival** | Survives agent restart within 30 minutes | If agent crashes and restarts, buffer intact |
| **Failure retention** | Without central pipeline: holds ~1 hour | At 425K logs/s, 1GB buffer holds ~2 million logs (~30 min at peak) |
| **Disk type** | SSD (NVMe) | Low latency, high throughput |

### 5.4 Log Shipper

| Parameter | Value | Description |
|---|---|---|
| **Shipper software** | Fluentd / Logstash / Custom agent | Industry-standard log shipping |
| **Send interval** | Every 5 seconds | Batch window |
| **Batch size** | 100 KB (uncompressed) | Compressed at transport layer |
| **Transport** | gRPC over TLS 1.3 | Low-latency encrypted transport |
| **Protocol** | Custom protobuf over gRPC | Smaller payload, faster serialization |
| **Compression** | snappy (streaming) | 5:1 compression ratio |
| **Retry** | Exponential backoff: 1s, 2s, 4s, 8s, max 60s | 5 retries before dead-letter |
| **Dead-letter queue** | Local file, max 100MB | Manually recoverable |
| **Authentication** | mTLS certificate | Node-level authentication |
| **Load balancing** | Round-robin across 5 Kafka brokers | Simple, no session affinity needed |

### 5.5 Ingestion Tier (Kafka)

| Parameter | Value | Rationale |
|---|---|---|
| **Broker count** | 10 | High availability, throughput |
| **Partitions** | 100 per log category topic | High parallelism for consumption |
| **Replication factor** | 3 | Durability, broker failure tolerance |
| **Retention** | 24 hours | Window for reprocessing failed batches |
| **Topic per category** | 10 topics (operational, audit, security, debug, error, business, system, communication, quality, cost) | Domain isolation, independent scaling |
| **Tiered storage** | Kafka + remote storage (S3) | Extended retention for slow consumers |
| **Throughput** | 500 MB/s per cluster | Headroom for 2x peak |
| **Cluster size** | 10 brokers (r6i.4xlarge) | Compute-optimized for compression |
| **Auto-scaling** | MSK auto-scaling | No manual intervention |
| **Min ISR** | 2 | Availability over consistency for log data |

### 5.6 Processing Pipeline

After Kafka, logs pass through a processing pipeline that parses, validates, enriches, and routes each log.

| Stage | Description | Throughput | Latency |
|---|---|---|---|
| **Parser** | Parse protobuf/gRPC payload → structured JSON | 1M logs/s | < 1ms |
| **Validator** | Run schema validation rules (section 3.7) | 500K logs/s | < 5ms |
| **Redactor** | Apply PII redaction, secret masking (section 9) | 500K logs/s | < 3ms |
| **Enricher** | Add geo-location, agent metadata, department lookup | 500K logs/s | < 2ms |
| **Router** | Route to appropriate storage tier based on category + level | 500K logs/s | < 1ms |
| **Indexer** | Prepare for Elasticsearch bulk indexing | 500K logs/s | < 10ms |
| **AlertMatcher** | Check against alert rules (section 10) | 500K logs/s | < 5ms |
| **DeadLetter** | Rejected logs with rejection reason | N/A (lower volume) | 10s batch |
| **Total pipeline** | End-to-end from Kafka to storage | 500K logs/s | < 100ms |

### 5.7 Failover and Resilience

| Failure Scenario | Behavior | Recovery |
|---|---|---|
| **Kafka broker down** | Producers retry with backoff; remaining brokers absorb load | Auto-rebalance within 30s |
| **Kafka cluster down** | Local buffer holds up to 1 hour of logs | When cluster recovers, flush buffer |
| **Sidecar crash** | Agent continues logging to stdout; new sidecar picks up on restart | < 10s recovery |
| **Processing pipeline down** | Kafka retains logs for 24 hours; consumers resume when restored | Exactly-once semantics via offset tracking |
| **Storage tier unavailable** | Logs buffered in Kafka; backpressure to shippers after 60s | Auto-retry every 30s |
| **Network partition** | Local buffer accumulates; logs capped at 2GB ring buffer | Flush on reconnection |
| **Node failure** | Logs in local buffer lost (non-critical) | Critical logs are immediately sent; replayable |

### 5.8 Backpressure Mechanism

When the pipeline approaches saturation, a graduated backpressure policy engages:

| Saturation Level | Threshold | Action |
|---|---|---|
| **Level 0** | < 70% capacity | Normal operation |
| **Level 1** | 70–85% capacity | Reduce DEBUG emission sampling to 0.5%, TRACE to 0.05% |
| **Level 2** | 85–95% capacity | Drop all DEBUG logs, drop 90% of TRACE logs |
| **Level 3** | 95–100% capacity | Drop DEBUG, TRACE, and 50% of INFO logs |
| **Level 4** | > 100% capacity (track overflow) | Drop all non-critical; keep only AUDIT, SECURITY, ERROR, FATAL, WARN |

Backpressure decisions are made per-agent at the sidecar level based on pipeline health indicator broadcast from the monitoring system.

---

## 6. LOG STORAGE ARCHITECTURE

### 6.1 Storage Tier Overview

| Tier | Retention | Engine | Size (Compressed) | Nodes | Query Speed |
|---|---|---|---|---|---|
| **Hot** | 7 days | Elasticsearch (OpenSearch) 3x replication | ~126 TB | 15 data nodes | < 1s simple, < 5s complex |
| **Warm** | 90 days | Elasticsearch (OpenSearch) 2x replication + compression | ~1.6 PB | 30 data nodes | < 10s average |
| **Cold** | 7 years | S3/Parquet → Athena/Presto/Trino | ~46 PB | N/A (serverless query) | < 60s |
| **Archive** | Permanent | S3 Glacier + annual snapshot | ~575 PB (over lifetime) | N/A | < 24h retrieval |

### 6.2 Hot Tier (Elasticsearch)

| Parameter | Value | Rationale |
|---|---|---|
| **Engine** | OpenSearch 2.x | Open source, robust log analytics |
| **Data nodes** | 15 | r6g.8xlarge (32 vCPU, 256 GB RAM) |
| **Master nodes** | 3 | Dedicated, c6g.2xlarge |
| **Shards** | 5 primary, 2 replicas per index | Balance parallelism and data safety |
| **Index pattern** | `logs-{category}-{YYYY-MM-DD}` | Daily indices for easy management |
| **Index lifecycle** | ILM: 7 days hot → warm/delete | Automated by Elasticsearch ILM |
| **Refresh interval** | 30 seconds | Balance between near-real-time and indexing throughput |
| **Replication** | 3x | 1 primary + 2 replicas across AZs |
| **Storage per node** | 2TB NVMe SSD × 15 = 30 TB raw | ~126 TB after 3x replication (but 5:1 compression) |
| **Effective capacity** | 30 TB raw × 5:1 compression = 150 TB effective | > 126 TB needed for 7 days |
| **Rollover** | When index > 500GB or per 24 hours | Prevents oversized indices |

### 6.3 Warm Tier (Elasticsearch)

| Parameter | Value | Rationale |
|---|---|---|
| **Engine** | OpenSearch (warm tier) | With frozen indices, slower but cheaper |
| **Data nodes** | 30 | r6g.4xlarge (16 vCPU, 128 GB RAM) |
| **Storage per node** | 4 TB HDD + NVMe cache | HDD for bulk, NVMe for hot search block cache |
| **Compression** | LPS (low-precision storage) | 10:1 compression, 5:1 with best_compression |
| **Index pattern** | `logs-warm-{category}-{YYYY-MM}` | Monthly indices for warm data |
| **Replication** | 2x | Lower redundancy for warm data is acceptable |
| **Total effective capacity** | 30 nodes × 4 TB × 2x rep × 5:1 = 300 TB effective | Warm retention requires ~1.6 PB → need to scale |
| **Scaling** | Auto-scale to 60 nodes for warm | Elasticsearch auto-scaling policy |
| **Search throttling** | Throttle to max 10 concurrent heavy queries | Protect cluster stability |

### 6.4 Cold Tier (S3 + Athena/Trino)

| Parameter | Value | Rationale |
|---|---|---|
| **Storage** | AWS S3 Standard-IA | Low-cost, accessible storage, 30-day min |
| **Format** | Parquet (columnar) | Columnar format for efficient query |
| **Compression** | ZSTD (level 19) | Very high compression ratio |
| **Partition** | By year/month/day/category | Partition pruning for query efficiency |
| **ETL** | Daily Spark job: read from Kafka → transform → S3 | Batch processing for cold path |
| **Query engine** | Trino (Presto SQL) on EMR Serverless | Serverless SQL querying |
| **Query speed** | < 60s for most queries | Trino engine optimization |
| **Retention** | 7 years (2,555 days) | Compliance requirement |
| **Storage estimate** | ~18 TB/day × 365 × 5:1 compression × 5:1 parquet = 1.3 TB/day → ~26 PB for 7 years | Actually 18TB/day → S3 IA cost-optimized |
| **Refined estimate** | 18 TB/day raw → 3.6 TB (gzip) → 0.72 TB (parquet zstd at 5:1) → 2.6 PB for 7 years | Cold tier practical sizing |

### 6.5 Archive Tier (Glacier)

| Parameter | Value | Rationale |
|---|---|---|
| **Storage** | AWS S3 Glacier Flexible Retrieval | Durable, extremely low cost |
| **Frequency** | Yearly snapshot | Annual archive |
| **Format** | Parquet + manifest files | Standard format, future-compatible |
| **Encryption** | AES-256 at rest | Compliance requirement |
| **Retrieval time** | 12–24 hours (standard retrieval) | Only for legal/compliance requests |
| **Retrieval cost** | $0.01/GB standard retrieval | Very low |
| **Retention** | Permanent | Legal hold may extend forever |
| **Lifecycle** | Cold tier → Glacier after 7 years full retention | Automated S3 lifecycle |
| **Size** | ~1.3 PB/year × 7 years → ~9 PB in Glacier | Cumulative 7 years of archive |

### 6.6 Storage Cost Estimation

| Tier | Monthly Data | Unit Cost | Monthly Cost | Annual Cost |
|---|---|---|---|---|
| **Hot** | 126 TB (3x replication) | $0.125/GB/month (gp3 SSD) | $15,750 | $189,000 |
| **Warm** | 288 TB (2x replication) | $0.08/GB/month (HDD) | $23,040 | $276,480 |
| **Cold** | ~45 TB/month written | S3 Standard-IA: $0.0125/GB/month | $562 | $6,750 |
| **Archive** | ~7 PB cumulative | Glacier: $0.004/GB/month | $28,000 | $336,000 (steady state year 7+) |
| **Query compute** | Variable | OpenSearch + Athena compute | ~$5,000/month | $60,000 |
| **Total (Year 1)** | | | ~$72,352/month | $868,230 |
| **Total (Year 7+)** | | | ~$100,000+/month | $1.2M+ |

### 6.7 Compression Details

| Storage Stage | Raw Size | Compressed Size | Ratio | Method |
|---|---|---|---|---|
| **In-transit (Kafka → Shippers)** | 212 MB/s | 42 MB/s | 5:1 | Snappy |
| **Hot tier (Elasticsearch)** | 126 TB | 25 TB | 5:1 | best_compression (ES) |
| **Warm tier (Elasticsearch)** | 1.6 PB | 160 TB | 10:1 | best_compression + LPS |
| **Cold tier (Parquet)** | 46 PB | 2.6 PB | 18:1 | ZSTD level 19 |
| **Archive (Glacier)** | 46 PB | 2.6 PB | 18:1 | ZSTD (pre-compressed) |

---

## 7. LOG QUERYING AND SEARCH

### 7.1 Query Interface

| Interface | Tier | Protocol | Target Audience |
|---|---|---|---|
| **Kibana / OpenSearch Dashboards** | Hot, Warm | HTTP, REST | Analysts, engineers, managers |
| **Trino CLI / JDBC** | Cold | JDBC | Data engineers, analysts |
| **Athena Console** | Cold (S3) | SQL | Analytics team |
| **Custom API Gateway** | All | GraphQL (internal) | Agent-to-agent log queries |
| **Grafana Explore** | Hot | PromQL (metrics) + Loki (logs) | All technical users |

### 7.2 Query Language

#### Hot/Warm Tier: Lucene Syntax (OpenSearch)

```
# Find all ERROR logs for a specific agent in last hour
level:ERROR AND agent_id:"a1b2c3d4-..." AND timestamp:[now-1h TO now]

# Find all logs for a specific trace across all agents
trace_id:"trace_8f7e6d5c-..." 

# Find all auth failures in last 24 hours
category:security AND log_type:AuthFailure AND timestamp:[now-24h TO now]

# Find all cost logs for GPT-4o usage yesterday
category:cost AND data.model_version:"gpt-4o*" AND timestamp:[2026-07-08 TO 2026-07-09]

# Complex: find all timeouts where retry count > 3
log_type:Retry AND data.retry_count:[3 TO *] AND timestamp:[now-1h TO now]

# Aggregation: count errors by agent, top 10
`level:ERROR` | top_k=10, metric=count, field=agent_id
```

#### Cold Tier: SQL (Trino on Parquet)

```sql
-- Find all ERROR logs for a specific trace
SELECT timestamp, agent_id, message, data
FROM logs_cold.logs_all
WHERE trace_id = 'trace_8f7e6d5c-4b3a-2109-fedc-ba9876543210'
  AND level = 'ERROR'
  AND day >= '2026-01-01'
ORDER BY timestamp ASC
LIMIT 1000;

-- Find average task duration per agent by month
SELECT 
  agent_id,
  month,
  AVG(data.duration_ms) as avg_duration,
  COUNT(*) as task_count
FROM logs_cold.logs_all
WHERE category = 'operational'
  AND log_type = 'TaskEnd'
  AND day >= '2026-01-01'
GROUP BY agent_id, month
ORDER BY avg_duration DESC;

-- Find users with most access denials in 2025
SELECT 
  data.user_id,
  COUNT(*) as denial_count
FROM logs_cold.logs_all
WHERE category = 'security'
  AND log_type = 'AccessDenial'
  AND year = 2025
GROUP BY data.user_id
ORDER BY denial_count DESC
LIMIT 100;

-- Monthly cost report by model
SELECT 
  month,
  data.model_version,
  SUM(data.tokens_used) as total_tokens,
  SUM(data.tokens_used * 0.0001) as estimated_cost
FROM logs_cold.logs_all
WHERE category = 'cost'
  AND log_type = 'LLMUsage'
  AND year = 2026
GROUP BY month, data.model_version
ORDER BY month, estimated_cost DESC;
```

### 7.3 Common Query Templates

| Query Name | Description | Category Filter | Time Range | Frequency |
|---|---|---|---|---|
| **Agent Health** | All ERROR/WARN from specific agent last hour | error, operational | Last 1 hour | Continuous (dashboard) |
| **Error Rate by Department** | ERROR count per department, grouped | error | Last 24 hours | Every 5 min (cached) |
| **Performance Regression** | Task duration p50/p95/p99 by agent type | operational | Last 7 days | Hourly |
| **Auth Failure Analysis** | Auth failures by IP, user, region | security | Last 7 days | On demand |
| **Trace Investigation** | All logs for a specific trace_id | All | Varies | Incident-driven |
| **Cost by Department** | LLM cost per department for billing | cost | Last 30 days | Daily |
| **Deployment Health** | Deployment events with failure | system | Last 3 days | Post-deployment |
| **Compliance Report** | All audit logs for user content changes | audit | Last 90 days | Daily (automated) |
| **Quality Trend** | Quality scores over time per content type | quality | Last 30 days | Hourly |
| **Error Hotspots** | Top 10 error codes with count and trend | error | Last 24 hours | Every 15 min |
| **Security Incidents** | All FATAL/CRITICAL security events | security | Last 30 days | On demand |
| **SLI Compliance** | Logs showing performance against SLOs | operational | Last 7 days | Continuous |

### 7.4 Search Performance and SLAs

| Query Type | Tier | Complexity | P50 | P95 | P99 |
|---|---|---|---|---|---|
| **Filter on timestamp + agent_id** | Hot | Simple | 80ms | 300ms | 1s |
| **Full-text search on message** | Hot | Complex | 500ms | 2s | 5s |
| **Cross-trace aggregation** | Hot | Complex | 2s | 5s | 10s |
| **Filter on timestamp + category (warm)** | Warm | Simple | 2s | 5s | 10s |
| **Date range + aggregation (warm)** | Warm | Complex | 5s | 15s | 30s |
| **Multi-condition cold query** | Cold | Simple | 10s | 30s | 60s |
| **Large cold scan (month+) in S3** | Cold | Complex | 30s | 60s | 120s |
| **Archive retrieval** | Archive | Retrieval | 12h | 24h | 48h |

### 7.5 Rate Limiting and Query Governance

| Policy | Limit | Enforcement |
|---|---|---|
| **Concurrent queries per user** | Hot: 5, Warm: 3, Cold: 2 | Reject additional queries with 429 |
| **Concurrent complex queries** | Hot: 3, Warm: 1, Cold: 1 | Queue for sequential execution |
| **Query time range limit** | Hot: 7 days, Warm: 90 days, Cold: unlimited | Truncate beyond tier retention |
| **Result size limit** | Hot/Warm: 10,000 rows, Cold: 100,000 rows | Return truncated results with warning |
| **No full table scan on hot** | Must have at least timestamp filter | Reject open-ended queries |
| **API query rate** | 100 query/min per agent, 1000/min per user | 429 + exponential backoff |
| **Cost conscious** | Complex queries on cold > $5 estimated cost warned | Approval required for > $50 query |

### 7.6 Saved Queries and Dashboards

| Dashboard Name | Category | Refresh Rate | Queries | Audience |
|---|---|---|---|---|
| **Executive Log Health** | Operational | 60s | Total log volume, error rate, top errors, ingestion lag | CE-AI, Executives |
| **Security Operations** | Security | 30s | Auth failure rate, permission changes, anomaly flags | Security team |
| **Quality Dashboard** | Quality | 60s | Quality score trends, verification pass rate, user flags | QA team |
| **Cost Explorer** | Cost | 300s | Cost by model, department, daily trend | Finance, managers |
| **Error Monitor** | Error | 30s | ERROR rate by agent, top error codes, real-time alerts | All |
| **Audit Trail Viewer** | Audit | Manual | Time range chooser, specific actor investigation | Compliance |
| **Debug Console** | Debug | 10s | Agent-specific debug trace viewer | Agent developers |
| **Communication Flow** | Communication | 60s | Handover count, escalation rate, message volume | Operations |
| **Infrastructure Health** | System | 30s | Deployment failures, scaling events, node health | Infrastructure |

---

## 8. LOG RETENTION AND LIFECYCLE

### 8.1 Retention Policies by Log Level and Category

| Category | Levels | Hot Tier (Full Text) | Warm Tier (Compressed) | Cold Tier (Parquet) | Archive (Glacier) | Total |
|---|---|---|---|---|---|---|
| **Operational** | INFO, WARN, ERROR | 7 days (all), 30d (WARN+) | 30d (INFO), 90d (WARN+) | 7 years (ERROR only) | Permanent (ERROR only) | 7yr max |
| **Operational** | DEBUG, TRACE | 7 days | 30 days | None | None | 30 days |
| **Audit** | ALL | 7 days | 90 days | 7 years | Permanent | Permanent |
| **Security** | ALL | 7 days | 90 days | 7 years | Permanent | Permanent |
| **Debug** | DEBUG, TRACE | 24h (TRACE), 7d (DEBUG) | 30d (DEBUG only) | None | None | 30 days |
| **Debug** | INFO, WARN, ERROR | 7 days | 30 days | 7 years (ERROR) | Permanent (ERROR) | 7yr max |
| **Error** | ALL | 7 days | 90 days | 7 years | Permanent | Permanent |
| **Business** | ALL | 30 days | 90 days | 7 years (ERROR+) | Permanent (ERROR+) | 7yr max |
| **System** | ALL | 7 days | 30 days | 1 year | Permanent (ERROR) | 7yr max |
| **Communication** | ALL | 30 days | 90 days | 7 years (ERROR) | Permanent (ERROR) | 7yr max |
| **Quality** | ALL | 30 days | 90 days | 2 years | Optional | 2–7 years |
| **Cost** | ALL | 30 days | 90 days | 7 years | Permanent | Permanent |

### 8.2 Lifecycle Flow

```
Agent generates log
        │
        ▼
    ┌────────────────┐
    │  Sidecar Buffer │  (seconds to minutes)
    └────────┬───────┘
             │
             ▼
    ┌────────────────┐
    │   Kafka         │  (24 hours retention, re-processable)
    └────────┬───────┘
             │
             ├───────────────────────────────────────────┐
             ▼                                           ▼
    ┌─────────────────────┐                   ┌─────────────────────┐
    │   Hot Tier (ES)     │  Auto-ILM (7d)    │   Cold Pipeline     │
    │   Full text search  │ ────────────→   │   (Spark → Parquet)  │
    │   7–30 days          │                   │                     │
    └─────────────────────┘                   └─────────────────────┘
             │                                           │
             │ Auto-ILM (30–90d)                       │ Daily batch
             ▼                                           ▼
    ┌─────────────────────┐                   ┌─────────────────────┐
    │   Warm Tier (ES)    │                    │   Cold (S3 Parquet)   │
    │   Compressed search  │                    │   SQL queryable     │
    │   30–90 days        │                    │   7 years            │
    └─────────────────────┘                    └─────────────────────┘
             │                                           │
             │ Delete after retention                    │ S3 Lifecycle Policy
             ▼                                           ▼
    ┌─────────────────────┐                   ┌─────────────────────┐
    │   Delete (Secure)    │                    │   Glacier Archive   │
    │   GDPR-compliant wipe │                    │   Permanent          │
    └─────────────────────┘                    └─────────────────────┘
```

### 8.3 Hot-to-Warm Transition

| Transition | Trigger | Action | Duration |
|---|---|---|---|
| **Operational DEBUG/TRACE** | 7 days elapsed | Delete from hot; no warm/cold | Instant (ILM) |
| **Operational INFO** | 30 days elapsed | Move from hot to warm, delete after warm | ILM rollover |
| **Operational WARN+** | 30 days (hot full text) → 90 days (warm compressed) | Roll from hot to warm to delete | ILM phase transitions |
| **Audit all levels** | 7 days hot → 90 days warm → 7 years cold → permanent archive | Full lifecycle through all tiers | Automated pipeline |
| **Debug TRACE** | 24 hours elapsed | Delete | Instant (ILM) |
| **Error all levels** | 7 days hot → 90 days warm → 7 years cold → permanent archive | Full lifecycle | Automated |

### 8.4 Archival Process

Archival is the process of taking cold-tier data and creating permanent, encrypted, immutable snapshots for compliance.

| Step | Description | Frequency | Owner |
|---|---|---|---|
| **Snapshot creation** | Daily Parquet files on S3 are organized into yearly snapshot manifests | Daily (append), Annual (snapshot) | Data Infra team |
| **Compression** | ZSTD level 19 re-compression for archive efficiency | Annual | Batch job |
| **Encryption** | KMS AES-256 re-encryption with archive-specific key | Annual | Security team |
| **Checksum** | SHA-256 checksum of every file, stored in manifest | Annual | Data Infra team |
| **Upload** | Transfer to Glacier with vault lock policy | Annual | Automated |
| **Verification** | Random sample 0.1% of archived logs retrieved and validated | Annual | Compliance team |
| **Manifest** | Create searchable manifest of what is archived (date range, categories, volume) | Annual | Batch job |
| **Lock** | Apply Glacier Vault Lock policy (WORM: Write Once Read Many) | Annual | Compliance team |

### 8.5 Deletion and Secure Erase

| Deletion Type | Method | Timeline | Verification |
|---|---|---|---|
| **Hot tier delete** | Delete Elasticsearch index (ILM) | Instant | Confirmed by ILM status |
| **Warm tier delete** | Delete Elasticsearch index (ILM) | Instant | Confirmed by ILM status |
| **Cold tier delete** | S3 object delete with version purge | Immediate | S3 delete marker |
| **Archive deletion** | Glacier object delete (if not under legal hold) | Immediate | Confirmed deletion |
| **GDPR secure erase** | Overwrite with zeros, then delete (S3: delete with MFA) | Immediate | Verification scan |
| **Legal hold** | Object lock until hold released | Indefinite | Glacier legal hold tag |
| **Retention period check** | Automated daily compliance scanning | Daily | Report generated |

### 8.6 Legal Hold

| Aspect | Policy |
|---|---|
| **Activation** | Legal agent or compliance team applies hold tag to specific time range/agent/trace |
| **Mechanism** | S3 Object Lock with governance mode; ES index write-block |
| **Duration** | Until hold is explicitly released by legal authority |
| **Scope** | Agent-ID, trace-ID, time range, or category + level combination |
| **Verification** | Daily legal hold compliance report |
| **Alert** | Any attempted deletion of held log triggers security alert |
| **Release** | Signed digital release order required, logged to audit trail |

---

## 9. LOG PRIVACY AND SECURITY

### 9.1 PII Detection and Redaction

PII detection runs at two stages: at the agent sidecar (prevention) and at the pipeline (catch-all). Both stages must match before a log is stored.

| PII Type | Pattern | Action | Replacement |
|---|---|---|---|
| **Email address** | `^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$` | Redact | `REDACTED_EMAIL` |
| **Phone number** | International: `\+?[1-9]\d{1,14}` (E.164 format) | Redact | `REDACTED_PHONE` |
| **Credit card** | Luhn-valid: `\b(?:4[0-9]{12}|(?:5[1-5][0-9]{2}|222[1-9]|22[3-9][0-9]|2[3-6][0-9]{2}|27[01][0-9]|2720)[0-9]{12})\b` | Redact | `REDACTED_CC` |
| **IP address** | Private: `\b(10\.|172\.(1[6-9]|2[0-9]|3[01])\.|192\.168\.)` | Redact | `REDACTED_IP_PRIVATE` |
| **IP address** | Public: `\b(?:[0-9]{1,3}\.){3}[0-9]{1,3}\b` | Mask | `MASKED_IP_XXX.XXX.XXX.0` |
| **National ID** | Saudi: `\b[1-2]\d{9}\b` / UAE: `\b784-\d{4}-\d{7}-\d{1}\b` | Redact | `REDACTED_NATIONAL_ID` |
| **Passport** | Generic: `\b[A-Z]{1,2}\d{6,9}\b` | Redact | `REDACTED_PASSPORT` |
| **Date of birth** | `\b\d{4}-\d{2}-\d{2}\b` | Redact | `REDACTED_DOB` |
| **Postal code** | Generic: `\b\d{5}(-\d{4})?\b` | Redact | `REDACTED_POSTAL_CODE` |
| **Full name** | NLP detection + pattern `[A-Z][a-z]+ [A-Z][a-z]+` | Redact | `REDACTED_NAME` |

### 9.2 Secrets and API Key Masking

| Secret Type | Detection | Action | Result |
|---|---|---|---|
| **API Key** | `[A-Za-z0-9_-]{20,}` or known provider prefix (sk-, pk_, AKIA, etc.) | Mask | Show first 4 + last 4 chars only |
| **Bearer token** | `Bearer\s+[A-Za-z0-9_-]{20,}` | Mask | `Bearer REDACTED_TOKEN` |
| **JWT** | `eyJ[A-Za-z0-9_-]+\.eyJ[A-Za-z0-9_-]+\.[A-Za-z0-9_-]+` | Mask | `JWT_REDACTED` |
| **Password** | `password[\s":]+\S+` (case-insensitive) | Mask | `REDACTED_PASSWORD` |
| **Private key** | `-----BEGIN.*PRIVATE KEY-----` | Mask | `PRIVATE_KEY_REDACTED` |
| **Database URL** | `(postgres|mysql|mongodb)://[^:]+:[^@]+@` | Mask | Connection string without credentials |
| **Session token** | `session[_\s]?[Tt]oken[\s":]+\S+` | Mask | `SESSION_TOKEN_REDACTED` |
| **OpenAI key** | `sk-[A-Za-z0-9]{32,}` | Mask | `sk-xxxx...xxxx` (first 4 + last 4) |
| **Auth header** | `Authorization:\s*\S+` | Mask | `Authorization: REDACTED` |

### 9.3 Encryption Standards

| Aspect | Standard | Implementation |
|---|---|---|
| **At rest (hot)** | AES-256 | Elasticsearch encryption at rest (KMS) |
| **At rest (warm)** | AES-256 | Elasticsearch encryption at rest (KMS) |
| **At rest (cold)** | AES-256 | S3 Server-Side Encryption with KMS |
| **At rest (archive)** | AES-256 | Glacier encryption with dedicated KMS key |
| **In transit (agent → sidecar)** | TLS 1.3 | localhost only, but encrypted anyway |
| **In transit (sidecar → shipper)** | TLS 1.3 | mTLS certificate authentication |
| **In transit (shipper → Kafka)** | TLS 1.3 | mTLS |
| **In transit (Kafka → pipeline)** | TLS 1.3 | Internal |
| **In transit (pipeline → storage)** | TLS 1.3 | Internal |
| **Key rotation** | KMS automatic key rotation | 1 year rotation for data keys |
| **Key management** | AWS KMS + CloudHSM for root keys | FIPS 140-2 Level 3 |

### 9.4 Access Control Model

| Role | Scope | Permissions | Authentication | Example Users |
|---|---|---|---|---|
| **Log Viewer** | Assigned departments only, current day only | READ hot tier, no full-text search on message, no data field | IAM credentials + SSO | On-call engineers |
| **Log Analyst** | Department scope, last 90 days | READ hot + warm, full field access, aggregation queries | IAM + MFA | Senior engineers, managers |
| **Log Administrator** | All logs, all tiers | READ all tiers, manage indices, retention policy changes | IAM + MFA + approval | Logging team, Infrastructure |
| **Log Auditor** | All audit + security logs | READ all tiers, no modification | IAM + MFA + signed audit trail of queries | Compliance, Security, Board |
| **Debug Analyst** | Agent-level, assignment required | READ debug logs for assigned agents | IAM + MFA + assignment token | Debugging engineers |
| **Log Viewer (Agent)** | Own logs only, current task | READ own logs, specific trace_id | Agent identity token | Any agent (self-debug) |

### 9.5 Audit of Log Access

Every query against the logging system is itself logged as an audit event.

| Event | Fields | Retention | Action |
|---|---|---|---|
| **Query Executed** | user_id, query_text, tier, execution_time_ms, results_count, query_timestamp | 7 years | Stored in audit log |
| **Query Failed** | user_id, query_text, error, reason, timestamp | 7 years | Alert if > 5/min/user |
| **Access Denied** | user_id, requested_resource, permission_missing, timestamp | 7 years | Security alert |
| **Export** | user_id, export_type (CSV, Parquet), row_count, filter_criteria, timestamp | 7 years | Auto-approved threshold: 10K rows |
| **Download** | user_id, file_name, size, timestamp | 7 years | Logged, no auto-approve if > 100MB |

### 9.6 Sensitive Islamic Content Restriction

| Content Sensitivity Level | Logging Allowed | Log Storage Tier | Additional Controls |
|---|---|---|---|
| **Quran text** | Log only content hash (SHA-256), never raw text | Audit tier only | Access restricted to Quran engine agents only |
| **Hadith text** | Log only content hash, reference ID | Audit tier only | Access restricted to Hadith engine agents |
| **Fatwa** | Log only fatwa ID and metadata | Audit tier only | Scholar-level + fatwa committee access only |
| **Islamic scholarly opinion** | Log only research ID, never text | Audit tier only | Verification department only |
| **User religious queries** | Log only anonymized (topic category, language, timestamp) | Business (restricted) | First-party data policy; user consent required |
| **Religious lessons** | Log only content ID, never transcript | Operational (restricted) | Content team only |
| **Tafsir** | Log only reference (surah, ayah range) | Audit (restricted) | Restricted to authenticated scholars |

---

## 10. LOG MONITORING AND ALERTING

### 10.1 Log-Based Alerting Framework

Log-based alerts are rules that trigger when log patterns match defined conditions. They are managed within the Monitoring Department and executed as part of the log processing pipeline.

| Alert Component | Description | Configuration |
|---|---|---|
| **Rule** | Conditions to evaluate on log stream | YAML-defined with test cases |
| **Window** | Time window for evaluation | Fixed: 1m, 5m, 15m, 1h, 24h |
| **Threshold** | Trigger condition | Count, rate, percentage, absence |
| **Condition** | Filter + aggregation | Query DSL for hot tier |
| **Action** | What happens when triggered | Alert, notification, incident, escalation |
| **Cooldown** | Minimum time between alerts | Prevent alert storm |
| **Severity** | Impact level | INFO, WARN, CRITICAL, PAGER |

### 10.2 Pre-Defined Alert Rules

| Alert ID | Name | Category | Condition | Window | Threshold | Severity | Action |
|---|---|---|---|---|---|---|---|
| ALERT-LOG-001 | High Error Rate | Error | `level:ERROR AND category:operational` | 5m | > 5% of total logs in window | CRITICAL | PagerDuty → On-call agent |
| ALERT-LOG-002 | Fatal Event | Error | `level:FATAL` | 1m | Any single FATAL | PAGER | PagerDuty → CE-AI + Security |
| ALERT-LOG-003 | Agent Silence | Operational | `category:operational AND agent_id:X` | 60s | No logs from agent_id in 60s | WARN | Notify agent manager |
| ALERT-LOG-004 | Error Storm | Error | `level:ERROR AND error_code:X` | 5m | > 50 occurrences | CRITICAL | Incident + agent quarantine |
| ALERT-LOG-005 | Auth Failure Spike | Security | `log_type:AuthFailure` | 5m | > 20 from same user_id | WARN | Security agent notification |
| ALERT-LOG-006 | Pipeline Lag | System | Pipeline processing lag | 1m | > 60 seconds lag | CRITICAL | Infrastructure alert |
| ALERT-LOG-006b | Pipeline Lag (warning) | System | Pipeline processing lag | 1m | > 30 seconds lag | WARN | Infrastructure agent |
| ALERT-LOG-007 | Storage Warning | System | Storage usage by tier | 5m | Hot > 80%, Warm > 75% | WARN | Infrastructure agent |
| ALERT-LOG-008 | Storage Critical | System | Storage usage by tier | 5m | Hot > 90%, Warm > 85% | CRITICAL | PagerDuty → Data Infra |
| ALERT-LOG-009 | Schema Violation | System | `schema_validation:failed` | 5m | > 100/min | WARN | Logging team |
| ALERT-LOG-010 | Trace Broken | Operational | Agent task start without corresponding end | 30m | > 10% orphaned starts | WARN | Manager notification |
| ALERT-LOG-011 | Anomaly Flag | Security | `log_type:AnomalyFlag` | Immediate | Any anomaly | CRITICAL | Incident Response team |
| ALERT-LOG-012 | Permission Spam | Security | `log_type:PermissionChange` | 5m | > 10 changes in same role | WARN | Security team |
| ALERT-LOG-013 | Debug Volume Spike | Debug | `category:debug` | 5m | Volume > 10x baseline | WARN | Debugging team |
| ALERT-LOG-014 | Cost Anomaly | Cost | `log_type:CostAnomaly` | Immediate | Anomaly detected | WARN | Finance agent |
| ALERT-LOG-015 | Quality Degradation | Quality | `log_type:QualityScore AND data.score < 0.7` | 15m | > 5 items below threshold | WARN | QA team lead |
| ALERT-LOG-016 | Escalation Storm | Communication | `log_type:EscalationEvent AND data.level >= 3` | 5m | > 3 level 3+ escalations | CRITICAL | Executives |
| ALERT-LOG-017 | Deployment Failure | System | `log_type:DeploymentEvent AND result:failed` | 30m | Any failure | CRITICAL | Infra + Deployment teams |
| ALERT-LOG-018 | Budget Cap Hit | Cost | `log_type:BudgetEvent AND action:cap_enforced` | Immediate | Any cap enforcement | WARN | Finance + Agent team |
| ALERT-LOG-019 | Log Pipeline Offline | System | No ingestion for any category | 5m | 0 logs ingested | PAGER | Incident Response Team |
| ALERT-LOG-020 | Data Exfil | Security | `log_type:DataExfil` | Immediate | Any | PAGER | CE-AI + Security |

### 10.3 Real-Time Log Anomaly Detection

Beyond threshold-based alerts, machine learning models analyze log patterns in real time for anomaly detection.

| Anomaly Type | Detection Method | Model Type | Response |
|---|---|---|---|
| **Error pattern change** | Compare error code distribution to 7-day rolling baseline | Statistical (KL divergence) | Alert if divergence > threshold |
| **Log volume spike** | Volume per agent/category compared to 1-hour rolling average | Holt-Winters forecasting | Alert if > 5 sigma above baseline |
| **Log volume drop** | Missing logs from key agent or service | Absence detection | Alert if agent silent > 60s |
| **New error code** | Error code never seen in last 7 days | Count-Min Sketch | Auto-create ticket |
| **Correlated failures** | Multiple agents failing with same pattern | Association rule mining | Alert + suggest RCA |
| **Rate of change** | Log velocity (logs/s) suddenly changes | Statistical (CUSUM) | Alert for investigation |
| **Geographic anomaly** | Logs from unexpected region or IP | Baseline variance | Security alert |
| **Time pattern anomaly** | Activity at unusual time for agent type | Seasonal decomposition | WARN flag |

### 10.4 Pipeline Health Monitoring

The logging pipeline is a critical path and must be monitored continuously.

| Metric | Collection Frequency | Warning Threshold | Critical Threshold | Dashboard |
|---|---|---|---|---|
| **Ingestion rate (logs/s)** | Every 10s | < 80% of expected | < 50% of expected | Pipeline Health |
| **Processing lag (seconds)** | Every 10s | > 30s | > 60s | Pipeline Health |
| **Backpressure level** | Every 10s | Level 2 | Level 3+ | Pipeline Health |
| **Kafka consumer lag** | Every 10s | > 100K messages | > 500K messages | Kafka Dashboard |
| **Dead-letter queue size** | Every 30s | > 1000 | > 5000 | Pipeline Health |
| **Storage usage per tier** | Every 60s | > 75% | > 90% | Storage |
| **Validation failure rate** | Every 60s | > 1% | > 5% | Quality |
| **Redaction rate** | Every 60s | > 0.1% of logs require redaction | > 1% | Security Exposure |
| **Pipeline error rate** | Every 30s | > 1% of pipeline processing steps | > 5% | Pipeline Health |
| **TLS certificate expiry** | Daily | < 30 days | < 7 days | Security |

### 10.5 Correlation with Metrics and Traces

| Source | Integration | Correlation Key | Purpose |
|---|---|---|---|
| **Metrics (Prometheus)** | Grafana | Timestamp + agent_id | Overlay error rate on resource usage charts |
| **Traces (OpenTelemetry)** | Jaeger, Grafana Tempo | trace_id | Click from log → trace → span detail |
| **Alerts (AlertManager)** | Unified on-call | incident_id | Link logs to alert context |
| **Cost data (Chargeback system)** | Cost dashboard | agent_id + task_id | Log-driven cost allocation |
| **Quality metrics** | Quality dashboard | content_id | Log-driven quality computation |
| **Deployments (CI/CD)** | Change dashboard | deployment_id | Correlate deployment events with log anomalies |

---

## 11. AUDIT LOGGING

### 11.1 Audit Log Requirements

Audit logs are the most critical subset of the logging system. They must satisfy legal, regulatory, and compliance requirements for the Islamic Education Platform.

| Requirement | Detail | Enforcement |
|---|---|---|
| **Non-repudiation** | Agent cannot deny performing a logged action | Digital signature on audit entries |
| **Integrity** | Audit logs cannot be modified after creation | Hash chain + immutable storage |
| **Chain of Custody** | Every access and modification of audit logs is tracked | Separate audit-of-audit log stream |
| **Complete** | Every auditable action is logged before execution completes | Pre-commit logging pattern |
| **Accurate** | Timestamps are synchronized to NTP, cannot be faked | NTP sync enforced, timestamp signed |
| **Readily Accessible** | Audit logs available for search within 60 seconds | Real-time pipeline |
| **Retention** | 7 years minimum, permanent for critical items | Lifecycle policy + Glacier archive |
| **Exportable** | Standard format (CSV, Parquet) for regulatory requests | Self-service export portal |

### 11.2 Audit Log Schema (Enhanced Fields)

In addition to the standard log schema, audit logs carry these mandatory fields:

| Field | Type | Required | Description |
|---|---|---|---|
| **audit.action** | string | Yes | The action performed, e.g. `content.create`, `user.role.change`, `data.access` |
| **audit.actor** | object | Yes | `{ agent_id, role, department }` who performed action |
| **audit.target** | object | Yes | `{ resource_type, resource_id, current_owner }` what was acted upon |
| **audit.result** | string | Yes | Enum: `approved`, `denied`, `failed`, `pending` |
| **audit.reason** | string | Yes | Why the action was taken (policy reference, justification) |
| **audit.hash_chain** | object | Yes | `{ prev_hash, current_hash, nonce }` cryptographic chain link |
| **audit.policy_reference** | string | No | e.g. `POLICY-DATA-ACCESS-V2` policy that governed the action |
| **audit.verification** | object | Yes | `{ verified_by, verification_method, timestamp }` |
| **audit.approval** | object | Conditional | `{ approved_by, approval_level, approval_timestamp }` if approval required |

### 11.3 Tamper-Evident Audit Logs

Each audit log entry is linked cryptographically to the previous entry in a hash chain. This guarantees immutability and allows efficient verification of the entire chain.

| Component | Implementation | Purpose |
|---|---|---|
| **Hash algorithm** | SHA-256 | Collision-resistant hash function |
| **Current hash** | `SHA-256(prev_hash + timestamp + action + actor + target + result + nonce)` | Link to previous entry |
| **Prev hash** | Hash of the immediately preceding audit log entry | Chain continuity |
| **Nonce** | Random 16-byte value | Prevent hash collision attacks |
| **Chain root** | Hash of the very first audit log entry (trusted initialization) | Starting point for verification |
| **Periodic anchor** | Every 10,000 entries, hash anchored to external blockchain or distributed ledger | External verification |
| **Verification interval** | Every audit batch commit (every 500ms), chain verified | Continuous integrity check |
| **Breach detection** | If chain is broken (hash mismatch), immediate security incident | Alert + Investigation |

**Hash Chain Algorithm:**

```
Let H be SHA-256 hash function
Let || be concatenation

For the first audit log (entry 0):
  hash_0 = H("GENESIS" || timestamp_0 || action_0 || actor_0 || target_0 || result_0 || nonce_0)

For entry n (n > 0):
  hash_n = H(prev_hash_{n-1} || timestamp_n || action_n || actor_n || target_n || result_n || nonce_n)

Verify entry N:
  computed_hash = H(prev_hash_{N-1} || timestamp_N || ... || nonce_N)
  assert computed_hash == stored_hash_N
```

### 11.4 Audit Log Verification (Self-Healing)

| Event | Detection | Recovery |
|---|---|---|
| **Hash mismatch** | Automated verification at batch commit | Quarantine the broken segment, alert security, reconstruct from evidence |
| **Missing entry** | Sequence gap detected in hash chain | Flag gap, attempt to recover from Kafka replay, escalate if irrecoverable |
| **Chain root compromised** | Root hash does not match externally anchored copy | Full audit investigation, manual verification, chain restart with notarization |
| **Timestamp jump** | Timestamp jumps backwards or > 5 seconds forward | Clock skew investigation, flag all entries with inaccurate timestamp |
| **Duplicate entry** | Same hash chain ID detected twice | Investigate source (replay? retry?), remove duplicate with justification |

### 11.5 Daily Compliance Checks

| Check | Scope | Method | Frequency | Report |
|---|---|---|---|---|
| **Audit completeness** | All agents have audit logs for last 24h | Count of audit logs per agent | Daily | Missing agents list |
| **Chain integrity** | Hash chain valid for all audit logs | Batch verification of last 100K entries | Every hour | Integrity report |
| **Retention compliance** | Logs not purged before retention period ends | Cross-reference retention policy with log ages | Daily | Violation report |
| **Access compliance** | All audit log queries matched to authorized users | Audit-of-audit query log analysis | Daily | Unauthorized access report |
| **GDPR/PDPL** | User deletion requests have log entries logged | Verify delete logs match request logs | Daily | Compliance check report |
| **Tamper check** | Random sample of 100 audit log entries verified | Manual (automated) alidation of 100 random hashes | Daily | Validation certificate |

### 11.6 Audit Reports

| Report Name | Content | Frequency | Audience |
|---|---|---|---|
| **User Access Report** | Who accessed what, when, and result | Daily | Security, Compliance |
| **Permission Change Report** | All role/permission modifications, who approved | Daily | Security, Department managers |
| **Content Modification Report** | All content creation, update, deletion events | Daily | Content team, QA |
| **Approval Report** | All actions requiring approval, approver, decision | Daily | Compliance, Department managers |
| **Data Access Report** | All read operations on sensitive data | Daily | Security, Privacy officer |
| **Financial Audit Trail** | All revenue, billing, and cost events | Weekly | Finance, Board (summary) |
| **Chain Integrity Report** | Hash chain verification results | Daily | Compliance, Security |
| **Retention Compliance Report** | Deletion timing vs policy | Weekly | Legal, Compliance |
| **Quarterly Audit Summary** | All audit events, exceptions, compliance status | Quarterly | Board of Directors, CE-AI |

---

## 12. LOGGING FOR DEBUGGING AND RCA

### 12.1 Debug Log Standards

Debug logs must be detailed enough for a developer or debugging agent to reconstruct the full execution context of any task. The following fields must always be present in debug logs:

| Field | Required | Description | Example |
|---|---|---|---|
| **step_name** | Yes | Name of the step being executed | `"validate_input_schema"` |
| **step_number** | Yes | Step index within the overall workflow | `3` |
| **total_steps** | Yes | Total number of steps in the workflow | `12` |
| **input_snapshot** | Conditional | Input passed to this step (truncated, no PII) | `{ "content_id": "abc", "type": "story" }` |
| **output_snapshot** | Conditional | Output produced (truncated, no PII) | `{ "valid": true, "errors": null }` |
| **decision_reasoning** | Conditional | For decision steps: reasoning process | `"Chose GPT-4o over GPT-3.5 because complexity > 0.7"` |
| **alternative_considered** | Conditional | Alternatives considered at decision point | `["reject", "escalate", "retry_simple"]` |
| **tool_input** | Conditional | Input to tool/function call | `{ "tool": "verify_fact", "params": {"fact_id": "f123"}}` |
| **tool_output** | Conditional | Tool output | `{ "confidence": 0.95, "verdict": "verified" }` |
| **duration_this_step_ms** | Yes | How long this step took | `234` |
| **cumulative_duration_ms** | Yes | Total duration so far | `1456` |
| **memory_snapshot** | Conditional | Relevant memory key/values accessed during step | `{ "accessed_keys": ["user_profile", "story_template"] }` |

### 12.2 Effective Debug Logging Guidelines

| Guideline | Rationale | Implementation |
|---|---|---|
| **Log at every branch** | Capture why a decision path was taken | Every `if`, `switch`, `case` must have a corresponding log |
| **Log input and output** | Restore state at any point in execution | Log input to each function, output from each function |
| **Log failures in detail** | Understand exactly what went wrong | Log the exact failure, the state, and the expected state |
| **Log retries with context** | Understand retry progression | Log retry count, backoff duration, incremental state changes |
| **Log external calls with timing** | Track latency bottlenecks | Log call start, call end, duration |
| **Log context transitions** | Track context switches (handover, escalation) | Log from_agent, to_agent, context summary nonces |
| **Avoid log spam** | Critical information drowns in noise | Log at appropriate level; DEBUG for detail, INFO for summary |
| **Log correlation IDs everywhere** | Never lose the chain | Every log in a trace has trace_id, span_id, correlation_id |
| **Avoid logging full documents** | Reduce volume, protect content | Log content hash + metadata, not content body |
| **Log parameter values (not just references)** | Self-contained debugging | Parameters necessary to reproduce must be included |

### 12.3 RCA Log Analysis Process

When a failure occurs, the Root Cause Analysis (RCA) process uses logs to trace the complete chain of events.

| RCA Step | Logs Required | Action | Expected Outcome |
|---|---|---|---|
| **1. Identify the failing trace** | All logs for trace_id | Search for ERROR/FATAL in trace, find initial failure | Failure location identified |
| **2. Trace backwards** | All logs in trace, reverse chronological | Walk back from failure to find root cause | Root cause identified |
| **3. Examine decision points** | Debug logs at decision steps | Logs showing decision inputs and reasoning | Determine if decision was correct given state |
| **4. Check preconditions** | Operational logs before the trace | Validate that inputs/outputs/handovers were correct | Validate upstream correctness |
| **5. Cross-reference with metrics** | Performance metrics, resource usage | Check CPU, memory, latency at time of failure | Identify resource exhaustion |
| **6. Cross-reference with communication logs** | Handover and escalation logs | Check if messages were dropped or delayed | Identify communication failure |
| **7. Validate against quality logs** | Verification results before failure | Check if quality checks passed before failure | Identify quality gap |
| **8. Formulate conclusion** | All gathered logs | Synthesize root cause and contributing factors | Documented RCA |

### 12.4 Full Task Reconstruction

The logging system must support reconstructing the complete execution of any task from start to finish. This is crucial for debugging, compliance, and ML training.

| Reconstruction Aspect | Data Required | Fields | Source |
|---|---|---|---|
| **Task acceptance** | Task acceptance log | task_id, agent_id, assignment_time, priority | Operational: TaskStart |
| **Input data** | Task input (hash reference) | content_hash, source_reference, parameter_snapshot | Operational: TaskStart.data |
| **Each execution step** | Step-by-step debug logs | step_name, step_input, step_output, step_duration | Debug: DecisionStep, ToolCallDetail |
| **Decision points** | Decision logs | branch, choice, rationale, alternatives | Debug: DecisionStep |
| **External calls** | API/LM call logs | endpoint, input_hash, output_hash, latency, cost | Operational/Cost: APICall, LLMUsage |
| **State changes** | State transition logs | from_state, to_state, trigger | Operational: StateTransition |
| **Inter-agent communication** | Communication logs | sender, receiver, message_type, payload_hash | Communication: InterAgentMessage |
| **Handovers** | Handover logs | from_agent, to_agent, reason, context_hash | Communication: HandoverEvent |
| **Escalations** | Escalation logs | level, reason, target, resolution | Communication: EscalationEvent |
| **Quality checks** | Verification logs | check_type, passes, score, reviewer | Quality: VerificationResult |
| **Task completion** | Task result log | result (success/failure), duration, output_summary | Operational: TaskEnd |
| **User feedback** | Feedback logs | rating, comments (anonymized), feedback_type | Quality: UserFeedback |

### 12.5 Correlation ID System

| ID Type | Scope | Generation | Propagation | Example |
|---|---|---|---|---|
| **trace_id** | Single end-to-end request | Generated at entry point (API, event, schedule) | All logs, all services, all agents | `trace_a1b2c3d4-e5f6-7890-abcd-ef12345678` |
| **span_id** | Single service or agent within trace | Generated at each service/agent entry | All logs from that service/agent | `span_b2c3d4e5-f6a7-8901-2bcd-ef23456789` |
| **parent_span_id** | Parent of current span | Set by caller service | Logs in child service link to parent | `parent_a1b2c3d4-e5f6-7890-abcd-ef12345678` |
| **correlation_id** | Cross-system correlation | Generated at first external boundary | Shared with external systems (user ID, payment ID) | `corr_c3d4e5f6-a7b8-9012-3cde-f3456789ab` |
| **task_id** | Within single task execution | Generated by work allocation engine | All logs for that task | `task_d4e5f6a7-b8c9-0123-4def-a4567890bc` |
| **session_id** | User session across requests | Generated at user login | User-facing services, limited scope | `session_e5f6a7b8-c9d0-1234-5efg-b5678901cd` |

### 12.6 Log Context for Isolation

Every log must contain sufficient context to be independently useful for debugging, without requiring cross-referencing to other logs.

| Context Element | Included In | Purpose |
|---|---|---|
| **Agent identity** | agent_id, logger_name | Which agent generated the log |
| **Task context** | task_id, workflow_id | What work was being done |
| **Execution context** | trace_id, span_id, correlation_id | How this fits into the bigger picture |
| **Environment** | environment, region, host, source | Where this was running |
| **Version info** | source.version, model_version | What version of code/model was running |
| **Time context** | timestamp | Exactly when it happened |
| **State context** | data.state_snapshot or data.current_state | What the state was at this moment |
| **Temporal context** | duration_ms | How long things took |
| **Decision context** | data.step_name, data.step_number, data.total_steps | Where in the process flow we are |
| **Error context** | error_code, data.exception_type, data.failure_action | For errors: what and where |

---

## 13. LOGGING COSTS AND OPTIMIZATION

### 13.1 Cost Breakdown by Component

| Component | Monthly Cost (Year 1) | Percentage | Year 7 (Steady State) |
|---|---|---|---|
| **Hot tier storage (Elasticsearch)** | $15,750 | 19.7% | $31,500 (growth) |
| **Warm tier storage (Elasticsearch)** | $23,040 | 28.8% | $46,080 |
| **Cold tier storage (S3)** | $562 | 0.7% | $1,124 |
| **Archive storage (Glacier)** | $0 (Year 1, building) | 0% | $28,000 |
| **Kafka cluster** | $8,400 | 10.5% | $12,600 |
| **Processing pipeline (compute)** | $12,000 | 15.0% | $18,000 |
| **Elasticsearch compute nodes** | $9,600 | 12.0% | $14,400 |
| **Network transfer** | $3,500 | 4.4% | $5,250 |
| **Cold tier query compute (Athena/Trino)** | $1,500 | 1.9% | $3,000 |
| **Sidecar + shipper compute overhead** | $2,500 | 3.1% | $3,750 |
| **Storage compression overhead** | $1,000 | 1.2% | $1,500 |
| **Backup and redundancy** | $2,000 | 2.5% | $3,000 |
| **Total** | **~$80,000** | **100%** | **~$168,000** |

### 13.2 Optimization Strategies

| Strategy | Category | Impact | Implementation | Savings Estimate |
|---|---|---|---|---|
| **Log level filtering** | Volume reduction | Reduce DEBUG/TRACE from stable agents by 99% | Sampling rules (Section 4.4) | 45% storage reduction |
| **Aggregation of repetitive logs** | Volume reduction | Combine 100s of micro-logs into one aggregate per minute | Agent-side aggregation buffer | 15–25% volume reduction |
| **Differential logging** | Volume reduction | Log only changes from last log, not full state | Emit state diff, not full snapshot | 10% volume reduction |
| **Smart compression** | Storage optimization | Best_compression in ES + ZSTD in cold | Tier-specific compression policies | 20% storage cost reduction |
| **Cold tier optimization** | Storage cost | Parquet + ZSTD + column pruning | Spark ETL with optimal schema | 80% cold cost reduction compared to raw text |
| **Retention tuning** | Storage cost | Right-size retention per level; don't over-retain | Automated policy (Section 8.1) | 30% storage reduction |
| **Hot tier overprovision** | Compute cost | Don't over-provision ES nodes; use cold for everything beyond 7 days | Automated ILM + sizing formula | 15% compute reduction |
| **Query cost awareness** | Query cost | Expensive queries go to warm, not hot; cold queries warned | Cost governance (Section 7.5) | 20% query compute reduction |
| **Sampling high-volume agents** | Volume reduction | Utility agents don't need full DEBUG logging | Role-based sampling policies | 15% volume reduction from Utility agents |
| **Deduplication** | Volume reduction | Identical logs within 60s deduplicated | Pipeline dedup filter | 3% volume reduction |

### 13.3 Cost per GB

| Tier | Ingested Raw Cost | Compressed Cost | Query Cost | Monthly Cost/TB Raw |
|---|---|---|---|---|
| **Hot (ES)** | $0.112/GB/month | $0.032/GB/month (compressed storage) | $2.50/TB queried | $112/TB/month |
| **Warm (ES)** | $0.064/GB/month | $0.006/GB/month (compressed storage) | $2.00/TB queried | $64/TB/month |
| **Cold (S3 Parquet)** | $0.001/GB/month | $0.0001/GB/month (Parquet) | $5.00/TB scanned (Athena) | $1/TB/month |
| **Archive (Glacier)** | $0.0004/GB/month | $0.00002/GB/month | $10/TB retrieval (Glacier) | $0.40/TB/month |

### 13.4 Logging Budget

| Budget Item | Percentage of Total Infra Budget | Dollar Value (Monthly) |
|---|---|---|
| **Infrastructure budget** | 100% (infrastructure) | $800,000 |
| **Logging (target)** | 10% of infrastructure budget | $80,000 |
| **Logging (ceiling)** | 15% of infrastructure budget | $120,000 |
| **Current logging cost** | ~10% | ~$80,000 |

### 13.5 Cost Optimization Review Cycle

| Review | Frequency | Participants | Process |
|---|---|---|---|
| **Volume review** | Weekly | Logging team, SRE | Review log volumes per agent; identify spikes; enforce sampling |
| **Retention review** | Monthly | Compliance, Data Infra | Review retention is meeting business needs; adjust policies |
| **Cost review** | Monthly | Finance, Infrastructure | Review actual spending vs budget; optimize storage |
| **Query cost audit** | Monthly | Analytics, Reporting | Review expensive queries; educate users |
| **Quarterly optimization** | Quarterly | All stakeholders | Full optimization review; implement major changes |
| **Annual TCO review** | Annually | Infrastructure, Finance | Total cost of ownership analysis; plan for growth |

---

## 14. LOGGING FOR ML/ANALYTICS

### 14.1 Logs as Training Data

The logging system generates a continuous stream of behavioral data about every agent. This data is a primary source for training and improving agents.

| Training Use Case | Log Data Used | Data Volume | Frequency | Purpose |
|---|---|---|---|---|
| **Behavioral cloning** | ReasoningTrace, DecisionStep logs | 100 TB/month | Batch monthly | Train agents to mimic successful behaviors |
| **Error pattern learning** | Error logs with context | 500 GB/month | Batch weekly | Train agents to avoid common errors |
| **Success pattern mining** | TaskEnd (success) logs with debug traces | 2 TB/month | Batch weekly | Identify success patterns for positive reinforcement |
| **Decision optimization** | DecisionStep logs with outcomes | 500 TB/month (sampled) | Batch monthly | Train agents to make better decisions |
| **Quality score prediction** | Quality logs + task parameters | 100 GB/month | Batch daily | Predict quality score before task completion |
| **Cost optimization** | Cost logs + model usage | 200 GB/month | Batch weekly | Train cost-aware decision making |
| **Anomaly detection** | Security, error logs | 5 TB/month | Batch weekly | Train security anomaly detection models |
| **Recovery recommendation** | Error + recovery logs | 300 GB/month | Batch monthly | Train agents to auto-recover from failures |

### 14.2 Log Mining Pipeline

The log mining pipeline extracts, transforms, and loads log data into the feature store and ML training pipeline.

```
Log Pipeline (Kafka)
       │
       ▼
┌──────────────────┐
│  Log ETL (Spark)  │  Daily batch processing
│  - Filter         │  - Extract relevant fields
│  - Aggregate      │  - Transform to training format
│  - Feature engine │  - Remove PII, hash sensitive IDs
│  - Write to       │  - Feature store (Redis, Feast)
│    Feature Store  │  - Training dataset (S3 Parquet)
└──────────────────┘
       │
       ├────────────────────────────────────┐
       ▼                                    ▼
┌──────────────────┐              ┌──────────────────┐
│ Feature Store     │              │ Training Datasets │
│ (Feast + Redis)   │              │ (S3 Parquet)      │
│ - Real-time       │              │ - Historical      │
│ - Agent context   │              │ - Labeled         │
│ - Behavioral      │              │ - Benchmark       │
│ - Environmental   │              │                   │
└──────────────────┘              └──────────────────┘
       │                                    │
       ▼                                    ▼
┌──────────────────┐              ┌──────────────────┐
│ Online Inference  │              │ Model Training   │
│ - Agent behavior  │              │ - Reinforcement  │
│ - Decision assist │              │ - Supervised     │
│ - Cost prediction │              │ - Unsupervised   │
└──────────────────┘              └──────────────────┘
```

### 14.3 Log-Based Agent Improvement

| Improvement Type | Source Logs | Process | Example |
|---|---|---|---|
| **Error avoidance** | Error logs (error_code, failure_reason, context) | Group errors by code; identify top 10; update agent prompts/training for those patterns | Agent always fails on "fact_not_in_knowledge_graph" → agent trained to verify first |
| **Decision improvement** | DecisionStep logs (alternatives, outcome) | Extract successful decision patterns; bootstrap RL training | Agent consistently fails when choosing "high_risk" branch → train to prefer "verified" path |
| **Quality improvement** | Quality logs (score, dimensions, feedback) | Identify low-scoring patterns; adjust agent training data | Agent generates stories scoring low on "age_appropriateness" → fine-tune on age-appropriate data |
| **Cost efficiency** | Cost logs (model, token count, task cost) | Identify high-cost patterns; train cost-optimization strategies | Agent selects expensive model for simple tasks → train cost-aware model selection |
| **Communication improvement** | Communication logs (handovers, escalations) | Identify handover/escalation patterns that lead to failures | Agent | |

### 14.4 Feature Engineering from Logs

| Feature | Source Log Type | Aggregation | Use Case |
|---|---|---|---|
| `avg_task_duration_7d` | TaskEnd | 7-day rolling average per agent | Performance prediction, workload balancing |
| `error_count_1h` | Error logs | 1-hour sliding window count | Anomaly detection, agent health |
| `token_cost_per_task` | Cost logs | Per-task sum | Cost prediction |
| `decision_success_rate` | DecisionStep + TaskEnd | Ratio of successful decisions to total decisions | Agent maturity scoring |
| `handover_frequency` | HandoverEvent | Count per workflow/agent | Collaboration optimization |
| `escalation_rate` | EscalationEvent | Count per agent per day | Agent autonomy scoring |
| `quality_score_running` | QualityScore | Running average of last 50 scores | Agent quality tracking |
| `memory_access_pattern` | MemoryAccess | TF-IDF of accessed keys | Memory optimization |
| `tool_success_rate` | ToolCallDetail | Success/failure ratio per tool | Tool reliability scoring |
| `user_feedback_rolling` | UserFeedback | Rating rolling average per content type | Content quality tracking |
| `latency_p99_agent` | PerformanceMetric (Latency) | P99 of task duration per agent | Performance alerting |
| `model_cost_efficiency` | Cost logs (LLMUsage) | Tokens per dollar per model | Model selection optimization |

### 14.5 Feedback Loop Integration

The log analysis results feed back into the system for continuous improvement.

```
Log Pipeline → Log Mining → Analysis → Recommendations
                                           │
                                           ├──→ Agent prompt update
                                           ├──→ Workflow optimization
                                           ├──→ Resource allocation change
                                           ├──→ Sampling rule adjustment
                                           ├──→ Quality threshold calibration
                                           └──→ Agent retraining trigger
```

| Feedback Type | Trigger | Action | Update Cycle |
|---|---|---|---|
| **Frequency** | Logged | What changes | Frequency |
|---|---|---|---|
| **Agent prompt update** | > 10% error rate for error code | Update agent system prompt to address pattern | Every 24 hours |
| **Workflow optimization** | Workflow takes longer than expected | Optimize workflow steps, add parallelism | Every 7 days |
| **Resource allocation** | Agent resource usage exceeds budget | Reduce agent model size, lower concurrency | Real-time (budget check) |
| **Sampling rule update** | Agent is stable for 7 days | Reduce DEBUG sampling from 10% to 1% | Weekly |
| **Quality threshold calibration** | False positive rate on quality check > 5% | Adjust quality score threshold | Monthly |
| **Agent retraining** | Quality score drops > 20% in last 500 tasks | Trigger model fine-tuning job | On detection |
| **Alert rule tuning** | Alert fatigue (too many false positives) | Adjust alert thresholds | Weekly |

---

## APPENDICES

### A. Log Schema Version History

| Version | Date | Changes |
|---|---|---|
| 1.0.0 | 2026-07-09 | Initial ratified release |

### B. Related Documents

| Document ID | Title | Relationship |
|---|---|---|
| AI-CORP-OVW-001 | AI Company Overview | Company context, workforce size, platform |
| AI-CORP-ORG-001 | Organization Structure | Department IDs, team structure, agent roles |
| AI-CORP-MON-001 | Monitoring System | Alert rules, dashboard definitions, SLAs |
| AI-CORP-PERF-001 | Performance System | Performance metrics referenced in logs |
| AI-CORP-COST-001 | Cost System | Cost tracking, budget allocation |
| AI-CORP-SEC-001 | Security Framework | Security event classification |
| AI-CORP-COMM-001 | Communication System | Inter-agent message types, handover protocols |
| AI-CORP-WORK-001 | Workflows | Workflow definitions, step types |
| AI-CORP-KPI-001 | KPI System | KPI definitions referenced in logs |

### C. Glossary

| Term | Definition |
|---|---|
| **Agent** | An AI employee that executes tasks within the company |
| **Agent ID** | UUID v7 that uniquely identifies an agent instance |
| **Audit Log** | Immutable, tamper-evident log of compliance-relevant actions |
| **Cold Tier** | Parquet files on S3, queried via SQL (7 years) |
| **Correlation ID** | UUID linking a log entry to external systems |
| **Debug Log** | Detailed execution trace for development and RCA |
| **Hot Tier** | Elasticsearch, fast search, short retention (7-30 days) |
| **ISO 8601** | International standard for date/time format |
| **Log Shipper** | Process that sends buffered logs from node to central pipeline |
| **PII** | Personally Identifiable Information |
| **RCA** | Root Cause Analysis |
| **Span ID** | UUID identifying a specific segment within a distributed trace |
| **Trace ID** | UUID identifying an end-to-end request or transaction |
| **Warm Tier** | Elasticsearch, compressed, medium retention (30-90 days) |

### D. Revision History

| Version | Date | Author | Changes |
|---|---|---|---|
| 1.0.0 | 2026-07-09 | Office of the Chief Architect | Initial ratified release |