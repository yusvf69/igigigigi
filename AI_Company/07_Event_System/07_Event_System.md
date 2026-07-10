# EVENT SYSTEM

## Event-Driven Architecture for Agent Coordination

---

| Document ID | AI-CORP-EVT-001 |
|---|---|
| Version | 1.0.0 |
| Status | Ratified |
| Classification | Enterprise Internal |
| Effective Date | 2026-07-09 |
| Approving Authority | Office of the Chief Architect |
| Custodian | AI Company Operations Board |
| Message Bus Reference | AI-CORP-BUS-001 |

---

## TABLE OF CONTENTS

1. EXECUTIVE SUMMARY
2. EVENT PHILOSOPHY
3. EVENT TAXONOMY
4. EVENT LIFECYCLE
5. EVENT HANDLERS
6. EVENT CHAINING
7. EVENT STORE
8. APPENDICES

---

## 1. EXECUTIVE SUMMARY

### 1.1 Purpose

The Event System defines how state changes propagate through the AI company. Events trigger workflows, notify agents, update dashboards, and maintain system coherence.

### 1.2 Core Principle

**Every state change produces an event. Every event has a consumer. No action happens in isolation.**

---

## 2. EVENT PHILOSOPHY

| Principle | Description |
|---|---|
| **Event Sourcing** | All state changes recorded as events in append-only log |
| **Single Source of Truth** | Event store is authoritative record of what happened |
| **Reactive Triggers** | Agents react to events, not polls |
| **Immutable** | Events are never modified, only corrected by new events |
| **Traceable** | Every event has a cause (parent event) and effect (child events) |

---

## 3. EVENT TAXONOMY

### 3.1 Event Categories

| Category | Examples | Volume/Day |
|---|---|---|
| **Task Events** | task.created, task.assigned, task.started, task.completed, task.failed | 5M |
| **State Events** | agent.online, agent.offline, agent.busy, agent.idle | 500K |
| **Content Events** | fact.verified, story.generated, quiz.created, podcast.published | 2M |
| **System Events** | deploy.started, deploy.completed, alert.triggered, alert.resolved | 10K |
| **Business Events** | user.signed_up, subscription.started, milestone.reached | 100K |

### 3.2 Event Schema

```json
{
  "event_id": "evt_20260709_001234_001",
  "event_type": "content.story.generated",
  "version": "1",
  "timestamp": "2026-07-09T14:30:00.000Z",
  "source": {
    "agent_id": "agent_sen_str_001234",
    "department": "D-STR"
  },
  "correlation_id": "task_20260709_008900",
  "parent_event_id": "evt_20260709_001234_000",
  "payload": {
    "story_id": "str_004521",
    "fact_ids": ["KG-FACT-001", "KG-FACT-002"],
    "language": "ar",
    "age_group": "9-12",
    "quality_score": 92.5
  },
  "metadata": {
    "priority": "normal",
    "ttl_ms": 3600000,
    "retry_count": 0
  }
}
```

### 3.3 Event Naming Convention

```
{domain}.{entity}.{action}
Examples:
task.completed
fact.verified
story.generated
agent.online
deploy.completed
```

---

## 4. EVENT LIFECYCLE

```
Created → Validated → Published → Routed → Consumed → Acknowledged
    │          │           │         │         │            │
    ▼          ▼           ▼         ▼         ▼            ▼
Record   Schema OK?   On topic   To queues  Handler    Mark done
in log               Retry if   with key   processes
                     fail       for order
```

### 4.1 Event Status Flow

| Status | Description | Next |
|---|---|---|
| `created` | Event object initialized | → validated |
| `validated` | Schema and permission check passed | → published |
| `published` | Written to event bus | → routed |
| `routed` | Delivered to consumer queue | → consumed |
| `consumed` | Received by handler | → processing |
| `processing` | Being handled | → completed / failed |
| `completed` | Successfully processed | → terminal |
| `failed` | Error during processing | → retry / dead_letter |
| `dead_letter` | Max retries exceeded | → terminal |

---

## 5. EVENT HANDLERS

### 5.1 Handler Registration

| Handler | Subscribes To | Department | Action |
|---|---|---|---|
| `OCRCompletedHandler` | `ocr.page.completed` | D-KG | Extract entities from OCR'd page |
| `FactVerifiedHandler` | `kg.fact.verified` | D-CGEN | Trigger content generation |
| `StoryGeneratedHandler` | `content.story.generated` | D-IMG | Generate story illustrations |
| `QuizCompletedHandler` | `learning.quiz.completed` | D-SMOD | Update learner mastery model |
| `AlertTriggeredHandler` | `infra.alert.triggered` | D-SEC | Execute security response |

### 5.2 Handler Execution Rules

| Rule | Description |
|---|---|
| **Idempotency** | Handlers must handle duplicate events safely |
| **Timeout** | Handler must complete within 30 seconds |
| **Error Handling** | Catch all exceptions, publish failure event |
| **Retry** | Automatic retry on transient failure (3x) |
| **Dead Letter** | After max retries, publish to dead letter queue |

---

## 6. EVENT CHAINING

### 6.1 Event Chain Example: Book → Story

```
book.imported
    → ocr.page.completed (× N pages)
        → kg.facts.extracted
            → kg.fact.verified (× M facts)
                → content.generation.requested
                    → content.story.generated
                        → media.illustration.requested
                            → media.illustration.completed
                                → qa.story.reviewed
                                    → content.story.published
```

### 6.2 Chain Rules

| Rule | Description |
|---|---|
| **Fan-Out** | One event triggers multiple child events |
| **Fan-In** | Multiple events must complete before parent proceeds |
| **Sequential** | Events within a chain must process in order |
| **Parallel** | Independent branches process concurrently |
| **Timeout** | Chain must complete within defined SLA or escalate |

---

## 7. EVENT STORE

| Property | Specification |
|---|---|
| **Storage** | Apache Kafka (hot) + S3 (cold) |
| **Retention (Hot)** | 7 days |
| **Retention (Cold)** | 7 years |
| **Query** | Event type, timestamp, correlation_id, source |
| **Replay** | Full event replay supported for recovery |

---

## 8. APPENDICES

### 8.1 Related Documents

| Document ID | Title | Relationship |
|---|---|---|
| AI-CORP-BUS-001 | Message Bus Architecture | Event transport |
| AI-CORP-COMM-001 | Communication System | Message protocols |
| AI-CORP-WORK-001 | Workflows | Event-driven workflows |

### 8.2 Revision History

| Version | Date | Author | Changes |
|---|---|---|---|
| 1.0.0 | 2026-07-09 | Office of the Chief Architect | Initial ratified release |
