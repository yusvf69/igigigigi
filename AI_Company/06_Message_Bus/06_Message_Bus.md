# MESSAGE BUS ARCHITECTURE

## Inter-Agent Message Infrastructure

---

| Document ID | AI-CORP-BUS-001 |
|---|---|
| Version | 1.0.0 |
| Status | Ratified |
| Classification | Enterprise Internal |
| Effective Date | 2026-07-09 |
| Approving Authority | Office of the Chief Architect |
| Custodian | AI Company Operations Board |
| Communication Reference | AI-CORP-COMM-001 |

---

## TABLE OF CONTENTS

1. EXECUTIVE SUMMARY
2. MESSAGE BUS ARCHITECTURE
3. TOPIC STRUCTURE
4. PRODUCERS AND CONSUMERS
5. DELIVERY GUARANTEES
6. DEAD LETTER QUEUE
7. MONITORING
8. APPENDICES

---

## 1. EXECUTIVE SUMMARY

### 1.1 Purpose

The Message Bus is the central nervous system of the AI company. All asynchronous inter-agent communication flows through this infrastructure, enabling decoupled, scalable, and reliable message delivery between thousands of agents.

### 1.2 Architecture Overview

```
┌─────────────────────────────────────────────────────────────────────────┐
│                      MESSAGE BUS LAYER                                  │
│                                                                          │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │                    APACHE KAFKA CLUSTER                          │   │
│  │  Topics: 500+    Partitions: 3,000+    Brokers: 12             │   │
│  │  Retention: 7 days  Throughput: 1M+ msgs/sec                   │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│                                                                          │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │                    RABBITMQ CLUSTER (RPC)                       │   │
│  │  Queues: 1,000+   Direct exchanges  Throughput: 100K msgs/sec  │   │
│  └─────────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## 2. MESSAGE BUS ARCHITECTURE

### 2.1 Technology Stack

| Component | Technology | Purpose |
|---|---|---|
| **Event Bus** | Apache Kafka 3.5+ | Durable event streaming, pub-sub |
| **RPC Queue** | RabbitMQ 3.12+ | Request-response patterns |
| **Schema Registry** | Confluent Schema Registry | Message schema validation |
| **Stream Processing** | Kafka Streams / Flink | Real-time message processing |

### 2.2 Kafka Cluster Specification

| Parameter | Value |
|---|---|
| Broker Count | 12 (3 per AZ × 4 AZs) |
| Partitions per Topic | 6 (default, configurable) |
| Replication Factor | 3 |
| Retention Period | 7 days |
| Message Size Max | 10 MB |
| Compression | Zstandard (level 3) |

### 2.3 Topic Naming Convention

```
{domain}.{department}.{action}.{version}

Examples:
kg.verification.fact-checked.v1
content.story.generated.v2
learning.quiz.completed.v1
infra.monitoring.alert.v1
```

---

## 3. TOPIC STRUCTURE

### 3.1 Core Topics

| Topic | Producers | Consumers | Volume/Day |
|---|---|---|---|
| `ocr.page.completed` | D-OCR | D-KG, D-VRF | 2M |
| `kg.fact.verified` | D-VRF | D-KG, D-CGEN | 1M |
| `content.generated` | D-CGEN | D-STR, D-QUIZ, D-FC, D-POD, D-IMG | 500K |
| `learning.progress.updated` | D-QUIZ, D-FC | D-SMOD, D-REC | 10M |
| `ai.teacher.response` | D-AIT | D-SMOD | 5M |
| `infra.alert.triggered` | D-MON | D-SEC, D-OPS | 1K |
| `system.deploy.completed` | D-DPL | All departments | 50 |

### 3.2 Topic Partitions

| Partition Strategy | Description | Example |
|---|---|---|
| **By Agent ID** | Hash partition on agent_id | `task.assigned` |
| **By Department** | Partition per department | `kg.fact.*` |
| **By Region** | Partition per geographic region | `user.request` |
| **By Priority** | Partition per priority level | `alert.*` |

---

## 4. PRODUCERS AND CONSUMERS

### 4.1 Producer Rules

| Rule | Description |
|---|---|
| **Schema Required** | Every message must conform to registered schema |
| **Idempotency Key** | Producer must include idempotency key |
| **ACK Level** | All (wait for all replicas) |
| **Retry** | 3 retries with exponential backoff |
| **Error Topic** | Failed messages published to `error.{topic}` |

### 4.2 Consumer Rules

| Rule | Description |
|---|---|
| **Consumer Group** | One group per logical consumer |
| **Auto-Commit** | Disabled (manual commit after processing) |
| **Max Poll Records** | 500 |
| **Max Poll Interval** | 5 minutes |
| **Rebalance Strategy** | Cooperative sticky |

---

## 5. DELIVERY GUARANTEES

| Guarantee | Implementation | Use Case |
|---|---|---|
| **At-Least-Once** | Kafka default + manual commit | Standard tasks |
| **Exactly-Once** | Idempotent producers + transactional API | Financial/critical operations |
| **At-Most-Once** | Auto-commit + fire-and-forget | Monitoring metrics |
| **Ordered** | Single partition per key | Sequential task processing |

---

## 6. DEAD LETTER QUEUE

| Parameter | Value |
|---|---|
| **DLQ Topic** | `dlq.{original_topic}` |
| **Max Retries** | 3 |
| **Retry Backoff** | 1s, 10s, 60s |
| **DLQ Retention** | 30 days |
| **DLQ Consumer** | D-OPS Queue Management Team |

---

## 7. MONITORING

| Metric | Collection | Alert |
|---|---|---|
| **Messages/sec** | Per broker | > 80% capacity |
| **Consumer Lag** | Per consumer group | > 10,000 |
| **Failed Messages** | Per topic | > 0.1% |
| **DLQ Size** | Per DLQ topic | > 1,000 |
| **Broker Disk Usage** | Per broker | > 80% |

---

## 8. APPENDICES

### 8.1 Related Documents

| Document ID | Title |
|---|---|
| AI-CORP-COMM-001 | Communication System |
| AI-CORP-EVT-001 | Event System |
| AI-CORP-MON-001 | Monitoring System |

### 8.2 Revision History

| Version | Date | Author | Changes |
|---|---|---|---|
| 1.0.0 | 2026-07-09 | Office of the Chief Architect | Initial ratified release |
