# AIOS Engineering Specification
## Document 33: AIOS Queue System

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

The AIOS Queue System is the central message queuing and buffering infrastructure that provides reliable, asynchronous communication between AIOS components. It decouples task producers (scheduler, workflow engine, agents) from task consumers (execution engine, agents) by providing a durable, ordered, and fault-tolerant message transport layer. The queue system is the backbone of all asynchronous operations within the AIOS ecosystem, ensuring no messages are lost during production, transmission, or consumption. It serves as the primary mechanism for load leveling, allowing the system to handle bursts of task submissions without overwhelming downstream consumers. The queue system also provides the foundation for task prioritization, delayed execution, scheduled processing, and dead-letter management. Without the queue system, the AIOS platform would lack the resilience and decoupling necessary for reliable distributed operation across a multi-node cluster. The queue system is designed to operate at internet scale, supporting millions of messages per day across hundreds of queues, with strict ordering guarantees where required and best-effort delivery where performance is prioritized over consistency.

---

## 2. Mission

To provide a reliable, persistent, and scalable message queuing substrate that guarantees at-least-once delivery, maintains strict ordering within queues, supports multiple queue types and hierarchies, and provides the operational observability needed to manage message flow across the entire AIOS cluster. The queue system aims to achieve 99.999% durability for persisted messages, zero message loss during normal operation, sub-millisecond enqueue latency for in-memory queues, and the ability to scale horizontally to handle increasing message volumes without architectural changes. The mission extends to providing comprehensive monitoring and alerting capabilities that enable operators to detect and resolve queue-related issues before they impact end users.

---

## 3. Responsibilities

3.1 Queue Management: Create, maintain, and destroy queues across the AIOS cluster. Support multiple queue types: FIFO, priority, delay, scheduled, dead-letter. Implement queue hierarchy: system, department, agent-type, workflow. Enforce queue naming conventions and organizational structure. Manage queue lifecycle from creation to deletion. Support queue pausing and draining for maintenance operations. Implement queue configuration versioning and rollback.

3.2 Message Lifecycle: Accept messages from authorized producers. Validate message format, size, and content against configurable schemas. Persist messages with configurable durability levels. Deliver messages to authorized consumers. Track message lifecycle through all states: CREATED, QUEUED, DELIVERED, ACKNOWLEDGED, FAILED, DEAD_LETTERED, EXPIRED, CANCELLED. Implement acknowledged message removal with configurable retention for audit purposes. Handle unacknowledged messages through requeue or dead-letter routing. Support message-level TTL with automatic expiration and cleanup.

3.3 Delivery Guarantees: Provide at-least-once delivery for all queues as the default guarantee. Support exactly-once delivery for critical queues via idempotency keys and consumer-side deduplication. Maintain FIFO ordering within single-producer queues with strict sequence tracking. Provide order-guaranteed delivery for priority queues based on priority level and insertion order. Implement consumer acknowledgment protocol with configurable acknowledgment timeouts. Handle consumer failures by requeueing in-flight messages after timeout expiration.

3.4 Consumer Management: Support pull-based consumption where the consumer polls for messages at its own pace. Support push-based consumption where the queue pushes messages to the consumer endpoint. Manage consumer concurrency limits and prefetch thresholds. Implement backpressure mechanisms for slow consumers to prevent system overload. Detect and manage consumer failures through health checking and automatic consumer deregistration. Support consumer grouping for load-balanced consumption across multiple consumer instances.

3.5 Queue Monitoring: Track queue depth in real-time with sub-second granularity. Monitor message age and identify stale messages that exceed processing SLAs. Measure throughput rates for both enqueue and dequeue operations. Detect consumer lag and implement backpressure indicators. Report queue health and capacity status through structured health check endpoints. Generate alerts for queue anomalies including depth spikes, consumer disconnections, and high error rates.

3.6 Poison Message Handling: Detect poison messages through configurable maximum delivery attempt thresholds. Quarantine poison messages in dead-letter queues with full metadata preservation. Analyze poison messages for root cause categorization and pattern identification. Prevent poisoned queues from blocking normal operation through circuit breaker patterns on message consumption. Implement dead-letter queue retention policies with configurable TTL and manual purge capabilities.

3.7 Priority Management: Implement priority queue ordering with support for P0 through P4 priority levels. Prevent priority inversion through proper queue-level isolation. Support message priority escalation through the aging mechanism. Handle starvation of low-priority messages via automatic priority promotion based on queue wait time. Provide configurable aging policies per queue with promotion thresholds and timing windows.

3.8 Persistence and Durability: Support in-memory queues for ephemeral messages where speed is paramount. Support disk-backed queues for durable messages using RocksDB storage engine. Support replicated queues using Raft consensus protocol for high availability across multiple nodes. Survive system restarts with persistent queues through crash-recovery mechanisms including write-ahead logging. Provide message recovery procedures for queue node failures including automated leader election and data replication. Support configurable persistence levels per queue: EPHEMERAL (memory only), DURABLE (local disk), REPLICATED (multi-node consensus).

3.9 Queue Scheduling: Support delayed message delivery where messages remain invisible until a delay period expires. Support scheduled message delivery where messages are released at or after a specified timestamp. Implement a timer wheel for efficient delay and scheduling management. Provide calendar-based scheduling for recurring message patterns. Support timezone-aware scheduling for geographically distributed deployments.

3.10 Queue Administration: Provide comprehensive administrative capabilities for queue configuration and management. Support queue creation with full configuration specification, including queue type, max depth, max message size, TTL, persistence level, dead-letter queue mapping, consumer limits, and backpressure configuration. Provide queue migration tools for moving queues between nodes without data loss. Support queue import and export for environment synchronization and disaster recovery.

3.11 Queue Statistics: Track comprehensive statistics for each queue including total messages enqueued, dequeued, acknowledged, failed, and dead-lettered. Maintain per-second throughput averages, peaks, and percentiles. Track message size distribution. Monitor consumer performance metrics by consumer ID. Provide history for queue depth and throughput for capacity planning. Support time-based aggregation for hourly, daily, and weekly reporting.

3.12 Queue Backup and Restore: Implement automated backup schedules for durable queues with configurable intervals. Support full and incremental backup strategies. Provide point-in-time restore capability. Ensure backup integrity through checksum verification. Support backup to remote storage for disaster recovery scenarios.

---

## 4. Non-Responsibilities

The Queue System does NOT execute tasks. The Queue System does NOT schedule tasks. The Queue System does NOT determine message routing beyond queue assignment. The Queue System does NOT perform message transformation or modification. The Queue System does NOT implement business logic. The Queue System does NOT authenticate users (but validates permissions). The Queue System does NOT provide service discovery. The Queue System does NOT manage connections or network routing. The Queue System does NOT implement agent logic. The Queue System does NOT replace the need for a dedicated event bus. The Queue System does NOT provide long-term data storage. The Queue System does NOT implement workflow orchestration. The Queue System does NOT manage consumer lifecycle beyond registration and health tracking. The Queue System does NOT implement rate limiting for task execution (only for message enqueue/dequeue operations). The Queue System does NOT provide pub/sub semantics for broadcast scenarios.

---

## 5. Architecture

5.1 High-Level Architecture: The Queue System follows a three-layer architecture: API Layer, Core Layer, and Storage Layer. The API Layer provides gRPC interfaces for producer and consumer operations and REST interfaces for administrative operations. The Core Layer contains the Queue Manager, Routing Manager, Consumer Manager, Backpressure Manager, Poison Handler, Priority Manager, and Queue Cleaner. The Storage Layer supports multiple backends including in-memory stores, disk-backed RocksDB stores, replicated Raft stores, dead-letter stores, and delay timer stores.

The three layers communicate through defined internal interfaces. The API Layer receives requests and forwards them to the Core Layer after basic validation. The Core Layer performs business logic, validation, and orchestration, then delegates to the Storage Layer for persistence. The Storage Layer provides a uniform interface that abstracts over the various storage backends. This layered architecture allows each layer to be independently scaled, tested, and modified. New storage backends can be added by implementing the Storage Layer interface without modifying the Core or API layers.

5.2 Architecture Principles:
- Durable by Default: Messages are persisted to disk unless explicitly configured as ephemeral.
- Order Preserving: Within a single queue, FIFO order is maintained for messages at the same priority level.
- Consumer Decoupled: Producers and consumers are completely decoupled and unaware of each other.
- Self-Healing: Queues automatically recover from partitions, node failures, and storage errors.
- Observable: Every queue operation is measured, logged, and available for monitoring.
- Elastic: Queues scale horizontally across nodes with automatic sharding and rebalancing.
- Secure: All queue operations require proper authentication and authorization.
- Fault-Tolerant: No single point of failure exists in the queue system architecture.
- Performant: Enqueue and dequeue operations are designed for single-digit millisecond latency.
- Configurable: Every aspect of queue behavior is configurable through runtime configuration.

5.3 Deployment Architecture: The Queue System is deployed as a distributed service across multiple nodes in the AIOS cluster. Each node hosts a queue server instance that communicates with peers via gRPC for replication and coordination. Queue data is partitioned across nodes based on queue name hashing with configurable replication factor. Leader election for replicated queues is managed through the Raft consensus protocol. Clients connect to any node but may be redirected to the leader node for consistent queue operations. The queue system supports multi-datacenter deployment with cross-datacenter replication for disaster recovery.

5.4 Network Architecture: The Queue System uses gRPC for all internal communications between queue nodes and between the queue system and its clients. gRPC supports bidirectional streaming, which is leveraged for push-based consumer delivery and metrics streaming. The admin REST API uses HTTPS with TLS (mTLS for internal endpoints). All replication traffic between nodes is encrypted in transit using TLS. The queue system maintains persistent connections to clients for push-based delivery and establishes new connections for pull-based operations.

5.5 Storage Architecture: The storage layer provides a uniform interface over multiple storage backends. Each backend implements the same interface and provides consistent semantics for message creation, retrieval, acknowledgment, and deletion. The in-memory backend is optimized for speed and is suitable for ephemeral messages where loss on restart is acceptable. The RocksDB backend provides persistent storage on local disk with write-ahead logging for crash recovery. The Raft backend provides replicated storage across multiple nodes with strong consistency guarantees. The storage backend can be selected per queue at creation time and cannot be changed after creation.

---

## 6. Internal Components

6.1 QueueManager: Central component that maintains the registry of all active queues. The QueueManager routes enqueue and dequeue requests to the correct queue instance based on queue name. It manages the full queue lifecycle including creation, activation, pausing, resuming, draining, and deletion. It handles configuration changes and queues that need to be reloaded when configuration updates. The QueueManager also coordinates cross-queue operations such as moving expired delayed messages to their target queues. It maintains the queue registry in memory with periodic persistence to the storage backend. The QueueManager polls on a configurable interval (default 100ms) to check for delayed and scheduled messages that need to be released to their target queues.

6.2 RoutingManager: Validates that producers are authorized to write to the target queue and consumers are authorized to read from the target queue. Applies rate limiting policies at the producer, consumer, queue, and cluster levels. Routes the message to the appropriate queue instance and storage backend. Handles routing failures including queue-not-found and permission-denied errors. Implements multiple routing strategies: direct routing (message goes to named queue), pattern routing (queue name pattern matching), header routing (routing based on message headers), and topic routing (routing based on topic subscription). The RoutingManager caches routing decisions to minimize latency for repeated routing requests.

6.3 ConsumerManager: Registers consumers and maintains their queue subscriptions. Supports both pull-based consumption where the consumer calls dequeue and push-based consumption where the queue pushes messages to a consumer endpoint. Manages consumer concurrency limits to prevent consumer overload. Tracks prefetch limits to control how many messages are delivered before acknowledgment. Monitors consumer health through periodic heartbeats and reconnection attempts. Implements flow control for slow consumers through prefetch reduction and eventual message redirection. Detects consumer failure through acknowledgment timeout monitoring and automatically requeues messages that were in-flight at the time of failure.

6.4 PersistenceManager: Provides a unified interface over multiple storage backends. The in-memory store keeps all data in RAM for fastest access with the trade-off of data loss on restart. The disk-backed store uses RocksDB for persistent storage on local NVMe or SSD storage with write-ahead logging for crash recovery. The replicated store uses the Raft consensus algorithm across 3 or more nodes for strong consistency and high availability. The PersistenceManager automatically selects the appropriate backend based on the queue's persistence level configuration. It manages storage connections, connection pooling, and retry logic for storage operations.

6.5 BackpressureManager: Monitors for slow consumers by tracking queue depth trends and consumer lag metrics. Defines four backpressure levels: WARNING (queue depth exceeds 60% of maximum), ELEVATED (depth exceeds 75%), CRITICAL (depth exceeds 90%), and SHEDDING (depth exceeds 95%). At each level, progressively more aggressive actions are taken: reducing prefetch at WARNING, stopping new prefetches at ELEVATED, stopping all push delivery at CRITICAL, and rejecting new producer messages at SHEDDING. Recovery occurs when the dequeue rate consistently exceeds the enqueue rate for a configurable recovery window (default 30 seconds). The BackpressureManager broadcasts its state to all connected queue nodes for coordinated backpressure decisions.

6.6 PoisonMessageHandler: Detects poison messages after a configurable maximum number of delivery attempts (default 5). When a message exceeds the maximum delivery attempts, it is quarantined to the dead-letter queue associated with the source queue. The PoisonMessageHandler records full delivery history including timestamps, consumer identifiers, and error messages for each failed delivery attempt. It analyzes quarantined messages to categorize the failure pattern (transient, persistent, configuration error) and surfaces patterns that require operational attention. For frequently recurring poison patterns, it can automatically pause delivery on the affected queue until the issue is investigated.

6.7 PriorityManager: Maintains ordering within priority queues by ensuring that higher-priority messages are dequeued before lower-priority messages. Within each priority level, FIFO ordering is maintained. The PriorityManager implements an aging mechanism to prevent starvation of low priority messages. The default aging policy promotes P4 to P3 after 5 minutes, P3 to P2 after 10 minutes, P2 to P1 after 30 minutes, and P1 to P0 after 60 minutes. Aging promotion times are configurable per queue to accommodate different workload requirements. The PriorityManager ensures that priority inversion does not occur by using per-priority-level ordering with no cross-level blocking.

6.8 QueueCleaner: Performs periodic cleanup of expired and acknowledged messages. Removes messages that have exceeded their TTL (time-to-live) without being consumed. Removes acknowledged messages after a configurable retention period (default 1 hour). Purges old dead-letter messages that have exceeded their DLQ retention period. Compacts storage backend when the ratio of deleted to live data exceeds a configurable threshold (default 20%). Runs on a configurable cleanup interval (default 60 seconds). Reports cleanup statistics including number of messages removed, bytes freed, and compaction status.

6.9 DelayTimer: Manages delayed and scheduled message delivery using a hierarchical timing wheel structure. For delayed messages, the DelayTimer holds the message until the delay period expires, then releases it to the target queue. For scheduled messages, the message is held until the scheduled timestamp is reached. The timer wheel supports timing resolutions at multiple levels: milliseconds, seconds, minutes, and hours. Messages with delays or schedules longer than the timer wheel range are persisted to the storage backend until they are within the wheel's range. The DelayTimer periodically checks for messages that are ready to be released and hands them to the QueueManager for redistribution to their target queues.

6.10 QueueShardCoordinator: Coordinates queue sharding across multiple nodes. Handles shard allocation when new nodes are added to the cluster. Rebalances shards when nodes are removed or when the cluster topology changes. Maintains consistent hashing for shard-to-node mapping. Handles shard-leader election for replicated queue shards. Coordinates shard migration for data transfer between nodes without data loss.

6.11 DeadLetterManager: Manages dead-letter queues. When a message is dead-lettered, the DeadLetterManager creates a copy in the associated DLQ, preserving all original message metadata and adding poison-related metadata. It maintains indexes on the DLQ for efficient querying by original queue, poison reason, delivery count, and time. Provides administrative APIs for reviewing, releasing, replaying, and purging dead-lettered messages. Supports batch operations for bulk dead-letter management.

---

## 7. External Components

7.1 AIOS Scheduler: The scheduler sends scheduled tasks as messages to execution queues. It receives task status messages from execution completion through the queue system. The scheduler manages task retry and dead-letter handling for execution tasks by monitoring queue consumer feedback. It uses the queue system to decouple task scheduling from task execution, allowing each to scale independently.

7.2 AIOS Execution Engine: The execution engine consumes task execution messages from queues. It sends execution status messages back through the queue system. It reports task progress and completion status via queue messages. The execution engine uses the queue system as its primary input source, polling or receiving pushed messages for tasks to execute.

7.3 AIOS Workflow Engine: The workflow engine sends workflow step triggers through the queue system. It receives workflow status updates via queue messages. The workflow engine manages workflow-level delays and scheduled messages using the queue system's delayed message capabilities. It uses the queue system to orchestrate complex multi-step workflows with branching and parallel execution.

7.4 AIOS Monitoring Service: The monitoring service consumes queue metrics for building monitoring dashboards and generating alerts. It provides queue visualization including real-time depth charts, throughput graphs, and consumer lag heat maps. The monitoring service uses queue metrics to generate operations alerts when queue health thresholds are violated.

7.5 AIOS Audit Service: The audit service consumes queue audit events for recording message lifecycle events in the audit log. It tracks all queue operations including queue creation, deletion, configuration changes, and significant message events. The audit service provides the compliance foundation for queue operations.

7.6 AIOS Admin Console: The admin console provides a human interface for queue management and monitoring. It allows operations teams to create, configure, and delete queues. It provides monitoring dashboards with real-time and historical views. It offers debugging tools for investigating queue issues including message inspection, consumer health, and dead-letter analysis.

7.7 AIOS Configuration System: The configuration system provides queue configuration storage and distribution. It manages configuration versions and supports rollback of configuration changes. The Queue System subscribes to configuration changes and applies them to active queues.

7.8 AIOS Authentication and Authorization Services: These services provide the security foundation for the queue system. The authentication service validates credentials and produces session tokens. The authorization service evaluates access control policies for queue operations.

7.9 AIOS State Manager: The state manager provides distributed coordination for the queue system. It manages leader election for replicated queues. It provides distributed locking for cross-node queue operations. It stores cluster state for queue shard allocations.

---

## 8. Dependencies

| Dependency | Type | Criticality | Description |
|---|---|---|---|
| RocksDB | Infrastructure | CRITICAL | Local message persistence for durable queues |
| Raft Consensus | Internal | HIGH | Replicated queue consistency across nodes |
| Event Bus | Internal | MEDIUM | Publishing queue lifecycle events |
| Auth Service | Internal | HIGH | Producer and consumer authentication |
| Config Service | Internal | HIGH | Queue configuration management |
| Monitoring Service | Internal | MEDIUM | Metrics collection and alerting |
| NTP/Clock Sync | Infrastructure | HIGH | Message timing and TTL enforcement |
| Disk Storage | Infrastructure | CRITICAL | Durable message storage |
| Network | Infrastructure | CRITICAL | Inter-node replication and client communication |
| Memory | Infrastructure | HIGH | In-memory queue storage and caching |
| State Manager | Internal | HIGH | Queue shard coordination and leader election |
| Audit Service | Internal | LOW | Message lifecycle audit logging |
| TLS Infrastructure | Infrastructure | HIGH | Encryption for inter-node communication |
| Kubernetes/Orchestrator | Infrastructure | MEDIUM | Node management and scaling |

---

## 9. Inputs

9.1 EnqueueRequest: QueueName (required, string, max 255 chars), MessageBody (required, bytes, max 1 MB), MessageId (optional, UUIDv4, auto-generated if not provided), Priority (optional, enum: P0-P4, default P2), DelayMs (optional, int64, 0-86400000 ms, 24 hours max), ScheduledAt (optional, Unix timestamp, up to 1 year in future), TTLMs (optional, int64, 0-2592000000 ms, 30 days max), IdempotencyKey (optional, string, max 128 chars, for deduplication), Headers (optional, map, max 20 entries, key max 256 chars, value max 1024 chars), ContentType (optional, string, valid MIME type).

9.2 DequeueRequest: QueueName (required, string), ConsumerId (required, string, registered consumer), MaxMessages (optional, int32, 1-100, default 1), WaitTimeoutMs (optional, int32, 0-30000 ms for long poll), AutoAck (optional, boolean, default false), VisibilityTimeout (optional, int32, 30000-43200000 ms, default 30000).

9.3 AckRequest: QueueName (required), MessageId (required), ConsumerId (required), Result (required, enum: ACK/NACK), Error (optional, string, for NACK with error details), Requeue (optional, boolean, for NACK, default true), RequeueDelayMs (optional, int32, requeue delay).

9.4 BatchEnqueueRequest: Messages (required, array of EnqueueRequest, max 100), QueueName (required, applied to all messages if not specified per message), Atomic (optional, boolean, default false: if true, all or none succeed).

9.5 BatchDequeueRequest: QueueName (required), ConsumerId (required), MaxMessages (1-1000), WaitTimeoutMs (0-30000), AutoAck (boolean).

9.6 PurgeRequest: QueueName (required), ConfirmationToken (required, string, prevents accidental purge), PurgeType (enum: ALL/ACKNOWLEDGED/EXPIRED/DEAD_LETTER).

9.7 QueueCreateRequest: QueueName (required, string), QueueType (required, enum: FIFO/PRIORITY/DELAY/SCHEDULED/DEAD_LETTER), PersistenceLevel (required, enum: EPHEMERAL/DURABLE/REPLICATED), MaxDepth (optional, int64, default 1000000), MaxMessageSize (optional, int32, default 1048576), DefaultTTLMs (optional, int64, default 86400000), DeadLetterQueue (optional, string), BackpressureEnabled (optional, boolean, default true), PriorityLevels (optional, array of enum: P0-P4), OwnerDepartment (optional, string).

9.8 QueueDeleteRequest: QueueName (required, string), Force (optional, boolean, default false: drain before delete if false), ConfirmationToken (required, string).

9.9 QueuePauseRequest: QueueName (required, string), Reason (optional, string), ExpectedResumeAt (optional, timestamp).

---

## 10. Outputs

10.1 EnqueueResponse: Status (required, enum: ENQUEUED/REJECTED/DUPLICATE/ERROR), MessageId (returned if generated), QueuePosition (optional, int64), EstimatedDelayMs (optional, int32), Error (optional, error code and message on failure), Timestamp (int64, Unix millisecond time of enqueue).

10.2 DequeueResponse: Status (required, enum: MESSAGES_FOUND/EMPTY/ERROR), Messages (array of: MessageId, Body, Headers, Priority, ContentType, DeliveryCount, EnqueuedAt, ExpiresAt, FirstDeliveredAt), ConsumerLag (int64, number of messages behind), Error (on ERROR status).

10.3 AckResponse: Status (required, enum: ACKNOWLEDGED/NOT_FOUND/DUPLICATE_ACK/ERROR), MessageId (string), Error (on ERROR status).

10.4 BatchEnqueueResponse: Status (required, enum: ALL_ENQUEUED/PARTIAL/REJECTED/ERROR), Responses (array of per-message EnqueueResponse), AtomicOperationOutcome (for atomic batches: ALL_ENQUEUED/ALL_REJECTED).

10.5 QueueInfoResponse: QueueName, QueueType, PersistenceLevel, Status (ACTIVE/PAUSED/DRAINING/INACTIVE), Depth (int64), MaxDepth (int64), CurrentConsumerCount, MessageConsumed (int64, lifetime total), MessageEnqueued (int64, lifetime total), CreatedAt, UpdatedAt, Configuration (queue configuration).

10.6 QueueMetricsResponse: QueueName, Depth, EnqueueRate (per second), DequeueRate (per second), AckRate (per second), AverageLatencyMs, P50LatencyMs, P95LatencyMs, P99LatencyMs, ConsumerLag, DeadLetterCount, PoisonMessageCount, BackpressureLevel (NORMAL/WARNING/ELEVATED/CRITICAL/SHEDDING), ErrorRate, StorageSize (bytes), StorageUsedBytes, Timestamp.

10.7 DeadLetterListResponse: Messages (array of dead-letter records with original metadata, poison metadata, delivery history), TotalCount, Page, PageSize.

10.8 HealthCheckResponse: Status (HEALTHY/DEGRADED/UNHEALTHY), NodeCount, ActiveShards, StorageStatus, LastReplicationTimestamp, QueueCount, MessageCount, MemoryUsage, DiskUsage.

---

## 11. Data Structures

11.1 QueueDefinition: queueName (string, max 255 chars), queueType (enum: FIFO/PRIORITY/DELAY/SCHEDULED/DEAD_LETTER), persistenceLevel (enum: EPHEMERAL/DURABLE/REPLICATED), hierarchy (enum: SYSTEM/DEPARTMENT/AGENT_TYPE/WORKFLOW), maxDepth (int64, default 1,000,000), maxMessageSize (int32, default 1 MB), defaultTTLMs (int64, default 24 hours), deadLetterQueue (string, optional, reference to DLQ), backpressureEnabled (boolean, default true), priorityLevels (array of string from P0-P4), ownerDepartment (string), ownerAgentType (string), ownerWorkflowId (string), createdAt (int64, timestamp), updatedAt (int64, timestamp), status (enum: CREATING/ACTIVE/PAUSED/DRAINING/INACTIVE/DELETED), configVersion (int64).

11.2 MessageRecord: messageId (string, UUIDv4), queueName (string), body (string, up to 1 MB), headers (map, max 20 entries), contentType (string, MIME type), priority (enum: P0-P4), ttlMs (int64, remaining TTL), delayMs (int64, remaining delay), scheduledAt (int64, Unix timestamp), status (enum: CREATED/QUEUED/DELIVERED/ACKNOWLEDGED/FAILED/DEAD_LETTERED/EXPIRED/CANCELLED), producerId (string), idempotencyKey (string, optional), corrrelationId (string), traceId (string), spanId (string), createdAt (int64, timestamp), enqueuedAt (int64, timestamp), acknowledgedAt (int64, timestamp, nullable), deliveryCount (int32), maxDeliveryCount (int32), lastDeliveredAt (int64, timestamp, nullable), lastDeliveredTo (string, consumerId, nullable), lastError (string, nullable), metadata (map).

11.3 ConsumerRecord: consumerId (string), queueName (string), consumerType (enum: PULL/PUSH), pushEndpoint (string, for push consumers), prefetchCount (int32, default 10), ackTimeoutMs (int32, default 30000), concurrency (int32, default 1), backpressureLevel (enum: NORMAL/WARNING/ELEVATED/CRITICAL/SHEDDING), isActive (boolean), lastSeenAt (int64), lastAckedAt (int64), totalAcknowledged (int64), totalFailed (int64), totalRequeued (int64), lag (int64), registeredAt (int64), department (string), heartbeatIntervalMs (int32, default 5000), failureDetected (boolean, default false).

11.4 QueueMetrics: queueName (string), depth (int64, current message count), enqueueRate (float64, per second), dequeueRate (float64, per second), ackRate (float64, per second), nackRate (float64, per second), averageLatencyMs (float64), p50LatencyMs (float64), p95LatencyMs (float64), p99LatencyMs (float64), consumerLag (int64), deadLetterCount (int64), poisonMessageCount (int64), backpressureLevel (enum), errorRate (float64), storageSize (int64, bytes), storageUsedBytes (int64), activeConsumers (int32), metricTimestamp (int64).

11.5 PoisonMessageRecord: originalQueueName (string), messageId (string), messageBody (string), deliveryAttempts (int32), maxAttempts (int32), previousErrors (array of: consumerId, timestamp, errorMessage, errorCategory), lastError (string), lastConsumerId (string), firstDetectedAt (int64), quarantinedAt (int64), analysisResult (string), analysisCategory (enum: TRANSIENT/PERSISTENT/CONFIG_ERROR/UNKNOWN), resolved (boolean), resolvedAt (int64), resolutionAction (string), metadata (map).

11.6 BackpressureState: queueName (string), level (enum: NORMAL/WARNING/ELEVATED/CRITICAL/SHEDDING), affectedConsumers (array of consumerId), startedAt (int64), depthOnTrigger (int64), currentDepth (int64), maxDepth (int64), enqueueRate (float64), dequeueRate (float64), consumerLag (int64), durationMs (int64), recoveryProgress (float64, 0-1).

11.7 RedisConsumerRecord: queueName (string), consumerId (string), messagesInFlight (array of messageId), lastHeartbeat (int64), prefetchCount (int32), totalMessagesReceived (int64), totalMessagesAcked (int64), totalMessagesNacked (int64), created (int64), expired (int64), heartbeatInterval (int32).

11.8 QueueConfig: queueName (string), queueType (enum), persistenceLevel (enum), maxDepth (int64), maxMessageSize (int32), defaultTTLMs (int64), deadLetterQueue (string), backpressureEnabled (boolean), backpressureThresholds (map of level to percentage threshold), agingEnabled (boolean), agingPolicy (map of source to target priority with promotion timeout), consumerTimeoutMs (int32), maxDeliveryAttempts (int32), retentionOnAckMs (int64), retentionOnExpiryMs (int64), allowedProducers (array of identity patterns), allowedConsumers (array of identity patterns), encrypted (boolean), encryptionKeyRef (string), auditEnabled (boolean).

11.9 HealthCheckResult: status (enum: HEALTHY/DEGRADED/UNHEALTHY), component (string), message (string), lastCheckTime (int64), latencyMs (int64), details (map[string]string).

---

## 12. Execution Flow

12.1 Message Enqueue Flow:
1. Producer sends EnqueueRequest via gRPC to any queue node
2. API Layer validates the request structure
3. RoutingManager validates producer authorization for target queue
4. BackpressureManager checks queue is not in SHEDDING state
5. RoutingManager applies rate limiting check for the producer
6. RoutingManager applies rate limiting check for the queue
7. Producer quota check against queue level limits
8. QueueManager looks up queue definition in registry
9. Validate message against queue constraints: size, format, content type
10. Check idempotency key if provided, return DUPLICATE if already processed
11. If delay or scheduled type: store message in DelayTimer, not in queue
12. Generate MessageId if not provided (UUIDv4)
13. Create MessageRecord with status QUEUED and current timestamp
14. Persist message to storage backend (in-memory, RocksDB, or Raft)
15. If queue is a priority queue: insert in priority order (highest priority first, FIFO within priority)
16. If queue is a FIFO queue: append to end of queue
17. Set message status to QUEUED, store enqueuedAt timestamp
18. Increment queue depth counter
19. Emit QueueMessageEnqueued event via Event System
20. If push consumers registered: notify consumer manager of new message
21. If pull consumers waiting in long poll: wake waiting consumers
22. Update queue metrics: enqueue count, queue depth, latency
23. Return EnqueueResponse to producer with status ENQUEUED and MessageId

12.2 Message Dequeue Flow (Pull):
1. Consumer sends DequeueRequest via gRPC
2. API Layer validates request structure
3. RoutingManager validates consumer authorization for target queue
4. ConsumerManager validates consumer is registered and active
5. Check backpressure level and reduce prefetch if backpressured
6. Lock queue for read operation (per-shard lock)
7. Read next batch of messages: priority order first, then FIFO
8. For each dequeued message: set status to DELIVERED, set deliveryCount+1
9. Set lastDeliveredAt, lastDeliveredTo with current timestamp and consumerId
10. Record message in consumer's flight tracking
11. If queue is empty and WaitTimeoutMs > 0: enter long poll wait
12. If long poll timeout expires without message: return EMPTY status
13. If new message arrives during long poll: return immediately
14. Return DequeueResponse with batch of messages and consumer lag info
15. Start acknowledgment timer for each message (VisibilityTimeout)
16. Update metrics: dequeue count, latency, consumer lag

12.3 Push Consumption Flow:
1. Consumer registers for push delivery with push endpoint and consumer config
2. ConsumerManager stores push consumer registration
3. When message arrives or consumer signals ready: queue pushes message
4. Push via gRPC stream or HTTP endpoint based on consumer configuration
5. Consumer receives message and processes it
6. Consumer sends acknowledgment (ACK or NACK) via Ack API
7. If ACK received within timeout: message marked as acknowledged
8. If NACK received: check delivery count, requeue or DLQ
9. If no response within acknowledgment timeout: retry with backoff
10. After MaxAckTimeoutFailures (default 3): mark consumer as unhealthy
11. Stop push delivery to unhealthy consumer
12. Redistribute messages to other consumers

12.4 Ack/Nack Flow:
1. Consumer sends AckRequest (ACK or NACK)
2. If ACK: set message status to ACKNOWLEDGED
3. Remove message from active storage (or mark for cleanup)
4. Record acknowledgment in consumer ack log
5. Update consumer metrics (totalAcknowledged)
6. If NACK: check deliveryCount vs maxAttempts
7. If deliveryCount < maxAttempts and requeue=true: set status to QUEUED
8. Apply requeue delay if specified (for immediate or non-immediate retry)
9. If deliveryCount >= maxAttempts or requeue=false: set status to DEAD_LETTERED
10. Copy message to dead-letter queue with poison metadata
11. Record poison message details including delivery history
12. Analyze failure pattern for eventual categorization
13. Remove message from consumer's in-flight tracking
14. Update metrics

12.5 Dead-Letter Flow:
1. Message exceeds max delivery attempts
2. PoisonMessageHandler creates PoisonMessageRecord with full delivery history
3. Copy message body and all metadata to corresponding DLQ
4. Add poison metadata to message: reason, last error, delivery count
5. Create DLQ message record with original queue reference
6. Remove message from source queue
7. Analyze error pattern across all failed delivery attempts
8. Categorize poison type: transient (network, timeout), persistent (auth, permission), config (invalid data, format), unknown
9. Update PoisonMessageRecord with analysis results
10. If persistent pattern detected: alert operations team
11. DLQ consumers can: analyze message, manually retry, discard, replay
12. Set DLQ retention period (default 30 days)
13. Periodically purge DLQ messages that exceed retention

12.6 Backpressure Flow:
1. BackpressureManager monitors queue depth and consumer lag on continuous basis
2. Compare depth against queue's maximum depth
3. Calculate depth percentage: (currentDepth / maxDepth) * 100
4. If depth >= 60%: WARNING level, reduce all consumer prefetch by 50%
5. If depth >= 75%: ELEVATED level, stop all new prefetch assignments
6. If depth >= 90%: CRITICAL level, stop push-based delivery to consumers
7. If depth >= 95%: SHEDDING level, reject new enqueue requests with 503
8. If backpressure level persists at WARNING or above for 5 minutes: alert operations
9. Continuous monitoring: compare dequeueRate against enqueueRate
10. When dequeueRate >= enqueueRate for 30 consecutive seconds: begin recovery
11. Reduce backpressure level gradually: SHEDDING->CRITICAL->ELEVATED->WARNING->NORMAL
12. At each level reduction, restore corresponding consumer operations
13. Full recovery: resume normal operation, reset prefetch to original values
14. Log backpressure lifecycle: start, each level change, and full recovery

12.7 Timer Wheel Flow:
1. Producer sends message with DelayMs or ScheduledAt
2. DelayTimer calculates release time: currentTime + DelayMs (or ScheduledAt)
3. Insert message into appropriate timer wheel slot based on release time
4. Timer wheel advances each tick (default 1ms precision)
5. When tick reaches slot with messages: identify messages due for release
6. Remove messages from timer slot
7. For each due message: enqueue to target queue
8. Update message status from DELAYED to QUEUED
9. If target queue is paused: hold in prestaging area
10. Notify queue consumers of new messages if applicable
11. Update delay timer metrics
12. Handle timer wheel overflow if message delay exceeds wheel range

---

## 13. State Management

13.1 Message State Machine:
- CREATED: Message initially created but not yet enqueued
- QUEUED: Message is in the queue, available for consumption
- DELIVERED: Message has been delivered to a consumer but not yet acknowledged
- ACKNOWLEDGED: Message has been successfully processed by consumer (terminal state)
- FAILED: Message processing failed, awaiting retry decision
- DEAD_LETTERED: Message delivery failed after max attempts (terminal state)
- EXPIRED: Message TTL has expired without acknowledgment (terminal state)
- CANCELLED: Message was explicitly cancelled before consumption (terminal state)

Transitions:
- CREATED -> QUEUED (message enqueued)
- QUEUED -> DELIVERED (message dequeued by consumer)
- QUEUED -> EXPIRED (TTL exceeded)
- QUEUED -> CANCELLED (explicit cancellation)
- DELIVERED -> ACKNOWLEDGED (consumer ACK)
- DELIVERED -> FAILED -> QUEUED (consumer NACK with requeue)
- DELIVERED -> FAILED -> DEAD_LETTERED (max attempts reached)
- DELIVERED -> QUEUED (auto-requeue on consumer failure/timeout)

13.2 Queue State Machine:
- CREATING: Queue being initialized in registry, not yet accepting messages
- ACTIVE: Queue is running, accepting enqueue and dequeue operations
- PAUSED: Queue pauses, messages accumulate but consumers cannot dequeue
- DRAINING: Queue is being drained, consumers can dequeue but no new messages accepted
- INACTIVE: Queue is inactive but not deleted, data preserved
- DELETED: Queue and all messages permanently removed

Transitions:
- CREATING -> ACTIVE (initialization complete)
- ACTIVE -> PAUSED (admin pause or backpressure)
- ACTIVE -> DRAINING (admin operation for deletion preparation)
- DRAINING -> INACTIVE (all messages consumed, ready for deletion)
- PAUSED -> ACTIVE (resume from pause)
- DRAINING -> ACTIVE (cancel drain operation)
- INACTIVE -> DELETED (admin confirms deletion)
- INACTIVE -> ACTIVE (reactivate old queue)

13.3 Consumer State Machine:
- REGISTERING: Consumer registration in progress
- ACTIVE: Consumer is registered and active for message consumption
- DISCONNECTED: Consumer connection interrupted, attempting reconnection
- SLOW: Consumer is slow, backpressure applied
- INACTIVE: Consumer is inactive (deregistered or failure detected)
- BLOCKED: Consumer is blocked by backpressure

Transitions:
- REGISTERING -> ACTIVE (registration complete)
- ACTIVE -> DISCONNECTED (heartbeat timeout)
- ACTIVE -> SLOW (backpressure applied)
- ACTIVE -> INACTIVE (consumer deregistration or failure)
- DISCONNECTED -> ACTIVE (successful reconnection)
- DISCONNECTED -> INACTIVE (reconnection failure after max attempts)
- SLOW -> ACTIVE (backpressure resolved)
- BLOCKED -> ACTIVE (backpressure resolved or admin override)

13.4 Shard State Machine:
- INITIALIZING: Shard is being created
- ACTIVE: Shard is active and processing
- ACTIVE -> LEADER: Shard is the leader for replicated queues
- ACTIVE -> FOLLOWER: Shard is a follower in Raft group
- LEADER -> CANDIDATE: Leader election in progress
- MIGRATING: Shard is being migrated to another node
- RECOVERING: Shard is recovering from failure
- OFFLINE: Shard is offline

---

## 14. Communication Protocols

14.1 Internal Communication Protocols:
- Producer to Queue: gRPC over HTTP/2 with Protobuf serialization
- Queue to Consumer (push): gRPC bidirectional streaming with Protobuf
- Consumer to Queue (pull): gRPC unary calls with Protobuf
- Peer-to-peer replication: gRPC with Raft consensus protocol
- Queue to RocksDB: Internal C++ API via RocksDB library
- Queue to Event Bus: Internal pub/sub interface with JSON event payloads
- Metrics to Monitoring: StatsD protocol over UDP for low-latency metrics
- Queue to Redis (if used): Redis RESP protocol for consumer tracking

14.2 External Communication Protocols:
- Admin REST API: HTTPS (TLS 1.3) with JSON payloads
- Monitoring: gRPC streaming for metrics export
- Cross-Datacenter Replication: gRPC with Protobuf over dedicated links

14.3 Wire Format:
- Primary serialization: Protocol Buffers (proto3) for gRPC
- Admin API: JSON (RFC 7159) for REST endpoints
- Event payloads: JSON for Event Bus publishing
- Storage serialization: RocksDB uses its native serialization internally

14.4 Protocol Versioning:
- gRPC services are versioned in the Protobuf package:
  - v1: initial queue service (current)
  - v2: under development with enhanced features
- All Protobuf messages use field numbers and wire format for backward compatibility
- Clients should send API version in request metadata
- Server can reject unsupported API versions

14.5 Security for Protocols:
- All gRPC communications use TLS 1.3 for encryption
- mTLS used for inter-node replication
- REST admin APIs require TLS and JWT authentication
- Connection pooling with configurable pool sizes
- Connection timeout: 10s for establishment, 30s for operations

---

## 15. APIs

15.1 gRPC Queue Service:

    service QueueService {
        // Queue Management
        rpc CreateQueue (CreateQueueRequest) returns (CreateQueueResponse);
        rpc DeleteQueue (DeleteQueueRequest) returns (DeleteQueueResponse);
        rpc GetQueueInfo (GetQueueInfoRequest) returns (GetQueueInfoResponse);
        rpc ListQueues (ListQueuesRequest) returns (ListQueuesResponse);
        rpc PurgeQueue (PurgeQueueRequest) returns (PurgeQueueResponse);
        rpc PauseQueue (PauseQueueRequest) returns (PauseQueueResponse);
        rpc BypassBackpressureForQueue (BypassBackpressureRequest) returns (BypassBackpressureResponse);

        // Message Operations
        rpc Enqueue (EnqueueRequest) returns (EnqueueResponse);
        rpc Dequeue (DequeueRequest) returns (DequeueResponse);
        rpc Ack (AckRequest) returns (AckResponse);
        rpc Nack (NackRequest) returns (NackResponse);
        rpc Peek (PeekRequest) returns (PeekResponse);
        rpc ReserveMessage (ReserveRequest) returns (ReserveResponse);
        rpc Heartbeat (HeartbeatRequest) returns (HeartbeatResponse);

        // Batch Operations
        rpc BatchEnqueue (BatchEnqueueRequest) returns (BatchEnqueueResponse);
        rpc BatchDequeue (BatchDequeueRequest) returns (BatchDequeueResponse);
        rpc BatchAck (BatchAckRequest) returns (BatchAckResponse);

        // Consumer Management
        rpc RegisterConsumer (RegisterConsumerRequest) returns (RegisterConsumerResponse);
        rpc UnregisterConsumer (UnregisterConsumerRequest) returns (UnregisterConsumerResponse);
        rpc RegisterPushConsumer (RegisterPushConsumerRequest) returns (stream PushMessage);
        rpc ListConsumers (ListConsumersRequest) returns (ListConsumersResponse);
        rpc GetConsumerInfo (GetConsumerInfoRequest) returns (GetConsumerInfoResponse);
        rpc UpdateConsumer (UpdateConsumerRequest) returns (UpdateConsumerResponse);
        rpc AcknowledgePush (AcknowledgePushRequest) returns (AcknowledgePushResponse);

        // DLQ/Dead Letter Management
        rpc ListDeadLetterMessages (ListDeadLetterMessagesRequest) returns (ListDeadLetterMessagesResponse);
        rpc ReplayDeadLetterMessage (ReplayDeadLetterMessageRequest) returns (ReplayDeadLetterMessageResponse);
        rpc DeleteDeadLetterMessage (DeleteDeadLetterMessageRequest) returns (DeleteDeadLetterMessageResponse);
        rpc GetDeadLetterMetrics (GetDeadLetterMetricsRequest) returns (GetDeadLetterMetricsResponse);

        // Health and Metrics
        rpc GetQueueMetrics (GetQueueMetricsRequest) returns (GetQueueMetricsResponse);
        rpc GetBackpressureState (GetBackpressureStateRequest) returns (GetBackpressureStateResponse);
        rpc GetHealth (GetHealthRequest) returns (GetHealthResponse);
        rpc GetStats (GetStatsRequest) returns (GetStatsResponse);
    }

15.2 REST Admin API:

    # Queue CRUD
    GET /api/v1/admin/queues - List all queues
    POST /api/v1/admin/queues - Create a queue
    GET /api/v1/admin/queues/{queue_name} - Get queue details
    DELETE /api/v1/admin/queues/{queue_name} - Delete a queue
    PUT /api/v1/admin/queues/{queue_name} - Update queue configuration

    # Queue Operations
    POST /api/v1/admin/queues/{queue_name}/purge - Purge queue messages
    POST /api/v1/admin/queues/{queue_name}/pause - Pause queue
    POST /api/v1/admin/queues/{queue_name}/resume - Resume queue
    POST /api/v1/admin/queues/{queue_name}/drain - Drain queue

    # Queue Monitoring
    GET /api/v1/admin/queues/{queue_name}/metrics - Get queue metrics
    GET /api/v1/admin/queues/{queue_name}/consumers - List consumers
    GET /api/v1/admin/queues/{queue_name}/dead-letter - List dead letters
    GET /api/v1/admin/queues/{queue_name}/poison - List poison messages

    # Admin Dashboard
    GET /api/v1/admin/queues/dashboard - Queue dashboard data
    GET /api/v1/admin/backpressure - Backpressure states
    GET /api/v1/admin/replication - Replication status
    GET /api/v1/admin/metrics - System-wide metrics

    # Dead Letter Management
    POST /api/v1/admin/dead-letter/{id}/replay - Replay dead letter to original queue
    POST /api/v1/admin/dead-letter/{id}/discard - Discard dead letter permanently
    POST /api/v1/admin/dead-letter/bulk-replay - Replay multiple dead letters

    # Health
    GET /api/v1/admin/health - Full health check
    GET /api/v1/admin/health/liveness - Liveness probe
    GET /api/v1/admin/health/readiness - Readiness probe

---

## 16. Events

16.1 Published Events Event System:
- QueueCreated: { queueName, queueType, persistenceLevel, createdAt, createdBy }
- QueueDeleted: { queueName, deletedAt, deletedBy, messagesLost (count) }
- QueuePaused: { queueName, pausedAt, pausedBy, reason }
- QueueResumed: { queueName, resumedAt, resumedBy }
- QueueActive: { queueName, depth, timestamp }
- MessageEnqueued: { queueName, messageId, contentType, size, producerId, hasDelay, priority }
- MessageAcknowledged: { queueName, messageId, consumerId, processingLatencyMs, deliveryCount }
- MessageFailed: { queueName, messageId, consumerId, error, deliveryCount }
- MessageDeadLettered: { queueName, messageId, deliveryCount, lastError, dlqName }
- MessageExpired: { queueName, messageId, createdAt, expiredAt, ttlMs }
- MessageRequeued: { queueName, messageId, deliveryCount, requeueDelayMs }
- QueueDepthWarning: { queueName, depth, maxDepth, level (WARNING/ELEVATED) }
- QueueDepthCritical: { queueName, depth, maxDepth, level (CRITICAL/SHEDDING) }
- BackpressureEmitted: { queueName, level, depthOnTrigger, consumerLag, affectedConsumers }
- BackpressureRecovered: { queueName, level, recoveryTimeMs, messagesDuringBackpressure }
- ConsumerDisconnected: { queueName, consumerId, messagesInFlight, lastHeartbeat }
- ConsumerLagged: { queueName, consumerId, lag, lagThreshold }
- ConsumerRegistered: { queueName, consumerId, consumerType, prefetch }
- PoisonMessagePatternDetected: { patternId, queueName, errorPattern, messageCount, firstDetectedAt }
- QueueShardMoved: { queueName, shardId, sourceNode, targetNode }
- QueueStorageLow: { nodeId, storageUsageBytes, storageLimitBytes, queueCount }

16.2 Subscribed Events:
- ConfigChanged: { configVersion, configChanges[] } -> reload configuration
- NodeShutdown: { nodeId, gracefulTimeout } -> drain queues and consumer redistribution
- SystemHealthCheck: { requestId } -> report node health status
- QueueConfigUpdated: { queueName, configChanges } -> apply runtime queue config changes
- NodeAdded: { nodeId, nodeAddress } -> rebalance shards
- NodeRemoved: { nodeId } -> migrate shards from removed node

---

## 17. Queues

17.1 Queue Types:
- FIFO: Strict first-in-first-out ordering. Messages are delivered in the exact order they were enqueued. Suitable for sequential processing tasks where order matters.
- Priority: Messages are delivered in priority order (highest priority first). Within the same priority level, FIFO ordering applies. Supports priority levels P0 (highest) through P4 (lowest).
- Delay: Messages are held for a minimum delay period after enqueue before becoming available for consumption. The delay period is configurable per message.
- Scheduled: Messages are held until a specific timestamp or absolute time before becoming available for consumption. Supports one-time and recurring schedules.
- Dead-Letter: Special queue used to store messages that have exceeded their maximum delivery attempts. Dead-letter queues are automatically associated with source queues.

17.2 Queue Hierarchy:
- System Queues (system.*): Reserved for AIOS core components. Include system.scheduler.tasks, system.execution.status, system.workflow.events, and system.monitoring.alerts.
- Department Queues (dept.<department>.*): Isolated queues per department or team.
- Agent Type Queues (agent.<agent_type>.tasks): Queues for specific agent types, allowing per-agent-type routing and scaling.
- Workflow Queues (wfl.<workflow_id>.*): Temporary queues created for specific workflow executions, used for workflow step coordination.
- Dead-Letter Queues (dlq.*): Queues for storing poison messages, automatically managed.

17.3 Queue Naming Convention:
- Pattern: <scope>.<category>.<subcategory>.<variant>
- Examples:
  - system.scheduler.tasks (scheduler task queue)
  - system.execution.status (execution status queue)
  - dept.engineering.tasks (engineering department tasks)
  - dept.data-science.output (data science results)
  - agent.data-validator.tasks (data validator agent specific queue)
  - agent.code-reviewer.tasks (code reviewer agent specific queue)
  - wfl.wf-12345.steps (workflow 12345 step queue)
  - wfl.wf-67890.status (workflow 67890 status queue)
  - dlq.agent.data-validator.dead (DLQ for data validator agent)
  - dlq.system.scheduler.dead (DLQ for scheduler)
- Reserved prefixes: system., dlq.

17.4 Queue Configuration:
- maxDepth: maximum number of messages in queue (default 1,000,000)
- maxMessageSize: maximum size per message in bytes (default 1 MB, max 10 MB)
- defaultTTLMs: default TTL if message does not specify (default 24 hours)
- retentionOnAckMs: how long after ack before message fully deleted (default 1 hour)
- maxDeliveryAttempts: maximum times a message can be delivered (default 5, max 15)
- deliveryBackoffMs: backoff for redelivery attempts (default 1,000ms)
- deadLetterQueue: associated DLQ name (auto-generated if not specified)
- backpressureEnabled: enable backpressure mechanism (default true)
- priorityLevels: which priority levels are supported (default P0-P4)
- agingEnabled: automatic priority promotion for old messages (default true)
- encryptionEnabled: encrypt messages at rest (default false per queue)
- auditEnabled: log all queue operations to audit log (default true)

---

## 18. Caching

18.1 QueueDefinitionCache: Caches queue definitions to avoid repeated database lookups. Per-node cache with configurable capacity (default 1000 entries) and TTL (default 60 seconds). Cache invalidation upon queue configuration change or deletion. Eviction policy: LRU. Cache key: queueName.

18.2 QueueMetricsCache: Caches real-time queue metrics to reduce computational overhead. Per-node cache with capacity for all active queues (no TTL-based eviction, updated every 1 second). Cache key: queueName.

18.3 ConsumerRegistryCache: Caches consumer registrations for fast routing decisions. Per-node cache with configurable capacity (default 10000 entries) and TTL (default 30 seconds). Eviction: LRU. Cache key: consumerId.

18.4 ProducerRateLimitCache: Caches producer rate limit state for fast rate limiting decisions. Per-node cache with configurable capacity (default 10000 entries) and TTL (default 1 second, rate limit windows reset on TTL). Cache key: producerId.

18.5 IdempotencyKeyCache: Caches processed idempotency keys to detect duplicates. Per-node cache with configurable capacity (default 100000 entries). TTL set to message TTL plus 1 hour to ensure deduplication window. Eviction: LRU. Cache key: idempotencyKey.

18.6 BackpressureStateCache: Caches backpressure state for all queues. Per-node cache with near-real-time updates. Cache key: queueName.

18.7 SecurityTokenCache: Caches validated security tokens. Per-node cache with configurable capacity (default 5000 entries) and TTL set to token expiration time. Eviction: LRU.

18.8 Cache Consistency Strategy:
- Local caches (per-node) are sufficient because queue definitions, metrics, and consumer registrations are updated infrequently or tolerate eventual consistency
- Idempotency key cache must be replicated for exactly-once semantics: distributed cache (Redis) or internal gossip protocol
- Cache warmup: on node startup, preload queue definitions, consumer registrations, and security tokens
- Cache coherency: cache invalidation events published on Event Bus for distributed cache refresh

---

## 19. Memory

19.1 Memory Footprint Estimates:
- Queue definitions: approximately 1 MB per 1000 queues (not including messages)
- Message index: approximately 100 bytes per message for metadata tracking
- In-memory queue storage: up to 4 GB per node for in-memory queues
- Message body storage: variable, based on message size and count
- Caches: approximately 1 GB for all cache types
- Consumer state tracking: approximately 500 KB per 1000 consumers
- Metrics buffer: approximately 200 MB for unflushed metrics
- Connection buffers: approximately 100 MB for gRPC connections
- Monitor framework overhead: 500 MB baseline

19.2 Memory Configuration:
- Max heap memory for queue system: configurable (default 8 GB)
- In-memory queue storage limit: configurable (default 4 GB)
- Cache memory limit: configurable (default 1 GB)
- Warning threshold: 80% of configured memory
- Critical threshold: 95% of configured memory (triggers emergency actions)
- GC configuration: concurrent garbage collection for Go, G1GC for Java

19.3 Memory Management:
- Memory-mapped files for RocksDB storage (leverages OS page cache)
- Pool allocation for message objects to reduce GC pressure
- Direct memory access for large message buffers
- Circuit breaker pattern: if memory usage exceeds critical threshold, reject new messages
- Emergency memory reclamation: force cache eviction and compaction
- Memory metrics: usage percentage, allocation rate, GC pause time, heap composition

19.4 Memory Pressure Handling:
- Level 1 (Warning, >80%): emit alert, reduce prefetch sizes, reduce cache TTLs
- Level 2 (Critical, >90%): reject non-critical requests, force GC, reduce message max size
- Level 3 (Emergency, >95%): reject all new messages except administrative operations, flush caches

---

## 20. Persistence

20.1 Persistence Levels:
- EPHEMERAL: Messages stored only in memory. No persistence to disk. Fastest access speed. Message loss on process restart. Suitable for transient event notifications, non-critical status updates, metrics streams.
- DURABLE: Messages persisted to local disk using RocksDB storage engine. Messages survive process restart. Write-ahead logging (WAL) enabled for crash recovery. Appropriate for standard task queues, workflow step queues, department queues.
- QUORUM: Messages replicated across multiple nodes using Raft consensus. Strong consistency guarantees. Survives node failure (up to f failures with 2f+1 nodes). Highest durability. Required for system-critical queues, workflow orchestration queues, and any queue where message loss is unacceptable.

20.2 RocksDB Storage:
- Storage engine: RocksDB (embeddable, persistent key-value store)
- Compression: ZSTD compression for message bodies (~3x compression ratio)
- WAL: enabled for crash recovery
- Compaction: level-based with periodic manual compaction
- Memory budget: 512 MB block cache, 256 MB write buffer
- Performance: up to 100K write operations per second per node
- Data directory: configurable path (default /var/lib/aios/queue/rocksdb)
- Backup: periodic RocksDB snapshots to backup location

20.3 Raft Consensus:
- Protocol: Raft consensus algorithm
- Group size: 3 nodes (minimum), 5 nodes (recommended for production)
- Leader election timeout: 150-300ms (randomized)
- Replication: asynchronous, pipeline-style
- Log compaction: snapshot-based
- Consistency: strong (linearizable reads)
- Performance: under 10ms write latency within same datacenter
- Recovery: automated leader election for up to f failures

20.4 Data Retention:
- Standard messages: retained until ack + retentionOnAck (default 1 hour)
- Dead-letter queue messages: 30 days retention (configurable)
- Expired messages: cleaned up on next cleanup cycle (60s interval)
- Audit logs: 90 days retention (managed by external audit service)
- Queue definitions: retained until queue deletion confirmed

20.5 Data Backup:
- Full backup: daily RocksDB snapshot for all durable queues
- Incremental backup: hourly WAL file backup
- Backup destination: configurable (local or remote storage)
- Backup verification: checksum verification on each backup
- Restore procedure: restore from latest full backup + WAL replay

20.6 Storage Error Handling:
- Write failure: retry 3 times, then report UNAVAILABLE
- Read failure: retry once, then fail to cache fallback
- Corruption detection: RocksDB checksum verification on each read
- Corruption recovery: restore from replica or backup

---

## 21. Validation

21.1 Queue Name Validation:
- Max length: 255 characters
- Allowed characters: lowercase a-z, digits 0-9, dots, underscores, hyphens
- Must start with one of: system., dept., agent., wfl., dlq.
- Must be unique within the AIOS cluster
- Reserved patterns: system.* (system use only), dlq.* (dead letter queues)

21.2 Message Validation:
- Max body size: 1 MB (configurable to 10 MB)
- Max headers: 20 entries per message
- Header key max length: 256 bytes
- Header value max length: 1024 bytes
- Content-Type: must be valid MIME type
- TTL: 0 to 30 days (2592000000 ms)
- Delay: 0 to 24 hours (86400000 ms)
- Schedule: up to 1 year in the future
- Priority: must be P0, P1, P2, P3, or P4
- Priority order: P0 (highest), P1, P2 (default), P3, P4 (lowest)
- IdempotencyKey: max 128 characters, alphanumeric with hyphens

21.3 Producer Validation:
- Must be authenticated (valid JWT token)
- Must have write permission for target queue
- Must be within rate limit (default 1000 messages per second)
- Must be within producer quota per queue (default 10000 messages per hour)

21.4 Consumer Validation:
- Must be authenticated (valid JWT token)
- Must have read permission for target queue
- Consumer must be registered in ACTIVE state
- Prefetch count: 1-100 (validated)
- Acknowledgment timeout: 1 second to 5 minutes (validated)
- Concurrency: 1-10 (validated)

21.5 Queue Operation Validation:
- Create: validate name, type, persistenceLevel, maxDepth, etc.
- Delete: validation confirmationToken, queue must exist
- Purge: validation confirmationToken to prevent accidental purge
- Pause: queue must be in ACTIVE state
- Resume: queue must be in PAUSED state

21.6 Business Validation:
- Cannot create DLQ manually (created automatically by system)
- Cannot rename a queue after creation
- Cannot modify queue persistence level after creation
- Cannot delete a queue with messages unless force=true

21.7 Security Codes:
- 200: OK (success)
- 400: BAD_REQUEST (validation error, invalid parameters)
- 401: UNAUTHORIZED (missing or invalid authentication)
- 403: FORBIDDEN (no permission, authorization denied)
- 404: NOT_FOUND (queue or consumer not found)
- 409: CONFLICT (duplicate queue creation, duplicate message via idempotency)
- 413: PAYLOAD_TOO_LARGE (message exceeds max size)
- 422: UNPROCESSABLE_ENTITY (queue is full, cannot accept more messages)
- 429: TOO_MANY_REQUESTS (rate limit exceeded, producer or queue level)
- 503: SERVICE_UNAVAILABLE (backpressure shedding, node recovery)
- 504: TIMEOUT (operation exceeded timeout)

---

## 22. Retry Logic

22.1 Message Delivery Retry:
- Maximum attempts: 3 (configurable per queue, max 15)
- Backoff strategy: exponential backoff with jitter
- Base delay: 1 second
- Multiplier: 2x
- Maximum delay: 60 seconds
- Jitter: +/- 50% (randomized within range)
- Backoff sequence (example): 1s, 2s, 4s, 8s, 16s, 32s, 60s(capped)
- After maximum attempts: message routed to dead-letter queue
- Retry only for transient errors: consumer timeout, network error, temporary unavailability

22.2 Storage Operation Retry:
- Write retry max: 3 attempts for RocksDB
- Read retry: once, then fallback to cache if available
- Raft operations: built-in retry in Raft protocol (log replication)
- Retry delay: 50ms for storage operations
- After max retries: return STORAGE_ERROR to caller

22.3 Consumer Push Delivery Retry:
- On push failure: retry up to 3 times with 1s backoff
- After max retries: mark consumer unhealthy
- Redistribute undelivered messages to other consumers
- When consumer reconnects: resume normal delivery

22.4 Replication Retry:
- Raft log replication: automatic in protocol with exponential backoff
- Node unreachable: retry with increasing delay (1s, 2s, 4s, 8s)
- After 60 seconds of continuous failure: mark node as failed

---

## 23. Error Recovery

23.1 Storage Failure Recovery:
- RocksDB write failure: retry with WAL replay
- RocksDB corruption: restore from latest snapshot
- RocksDB disk full: move to alternative storage path or reject writes
- Raft leader failure: automated leader election (150-300ms)
- Raft log inconsistency: snapshot sync from leader
- Storage backend fallback: DURABLE falls back to EPHEMERAL if disk full

23.2 Consumer Failure Recovery:
- Consumer heartbeat timeout: attempt reconnection 3 times
- If reconnection fails: deregister consumer, requeue in-flight messages
- In-flight messages: set back to QUEUED status after consumer timeout
- Redistribute to other registered consumers
- Notify admin of consumer failure

23.3 Node Failure Recovery:
- Queue node crash: remaining nodes detect via heartbeat
- Shard rebalancing: reassign shards from dead node to live nodes
- Raft follower failure: leader continues with remaining follower(s)
- Raft leader failure: remaining followers hold election
- Data recovery: replicas serve remaining data, dead node data restored when node rejoins

23.4 Cluster Partition Recovery:
- Network partition: both sides continue independently
- Split-brain prevention: Raft quorum-based leader election (only majority side serves writes)
- Partition healed: rejoining node syncs state from current leader
- Data reconciliation: last-writer-wins for conflicting writes
- Manual intervention required for irreconcilable conflicts

23.5 Poison Message Recovery:
- DLQ message replay: selective or batch replay to original queue
- DLQ message routing: manually route to specific consumer for analysis
- DLQ message discard: remove permanently after analysis
- Automated replay: replay with modified parameters if error pattern was transient

---

## 24. Security

24.1 Access Control:
- Queue access requires authentication (JWT token from authentication service)
- Authorization checks per operation: read (dequeue, peek), write (enqueue), admin (create, delete, configure)
- Queue isolation between departments: dept.* queues visible only to authorized department members
- System queue protection: system.* queues accessible only to core AIOS components
- Dead-letter queue access: restricted to operations and admin roles

24.2 Data Security:
- Message-level encryption (AES-256-GCM) for queues with encryption enabled
- Encryption at rest: transparent encryption for persistent storage
- Encryption in transit: TLS 1.3 for all gRPC and REST communications
- Keys managed by AIOS Secrets Manager: key rotation, access auditing

24.3 Audit Trail:
- All queue operations logged: create, delete, modify, enqueue, dequeue, ack
- Each audit record includes: operation, timestamp, user/component, queue, result
- Audit data stored in append-only mode for tamper evidence
- Audit data retention: 90 days minimum (configurable)

24.4 Rate Limiting:
- Producer rate limits: per producer ID, default 1000 messages/second
- Queue rate limits: default 10000 messages/second per queue
- Consumer rate limits: default 1000 dequeue operations/second per consumer
- Cluster-wide rate limits: aggregated across all nodes
- Rate limit violation: 429 Too Many Requests, retry-after header

24.5 Network Security:
- gRPC with TLS 1.3 for end-to-end encryption
- mTLS for inter-node communication (mutual certificate verification)
- Admin REST API: HTTPS only
- Client certificates for component-to-queue communication
- Network policies restrict access to queue nodes

---

## 25. Authentication

25.1 Authentication Method:
- JWT tokens (JSON Web Tokens) for all queue API access
- Tokens issued by AIOS Authentication Service
- Token validation: signature, expiration, issuer, audience
- Token refresh: configurable (default 24 hours token lifetime)
- Supported token formats: JWT with RS256 or ES256 signatures

25.2 Token Requirements:
- Token must include: subject (component or user identity), roles (permission set), issued_at, expires_at
- Token must be present in gRPC metadata (authorization header)
- Token lifetime: 1-24 hours (configurable per identity type)
- Short-lived tokens for agents (1 hour), longer for admin users (24 hours)
- Token revocation: immediate on logout or permission change

25.3 Component Authentication:
- Components authenticate using service accounts with client certificates
- mTLS for inter-component communication
- Service account identity mapped to queue permissions
- Service tokens automatically rotated every hour

---

## 26. Authorization

26.1 Authorization Model:
- Role-based access control (RBAC) for queue operations
- Three roles: reader (dequeue, peek), writer (enqueue), admin (full administrative control)
- Resource-based permissions: per queue or queue naming pattern (e.g., dept.*)
- Authorization evaluated at the beginning of each operation

26.2 Permission Matrix:
- Queue list: admin role
- Queue create: admin role
- Queue delete: admin role
- Queue configure: admin role
- Queue info: reader, writer, admin
- Enqueue: writer, admin
- Dequeue: reader, admin
- Peek: reader, admin
- Ack/Nack: reader (can only ack own messages), admin
- Purge: admin
- Dead-letter review: admin
- Dead-letter replay: admin
- Backpressure bypass: admin
- Metrics: reader, writer, admin

26.3 Permission Evaluation:
- Cache permission decisions for 30 seconds to reduce auth service load
- Permissions evaluated against queue name pattern (wildcards supported)
- Department isolation enforced for dept.* queues
- System queue write restriction: only core system components

---

## 27. Permissions

27.1 Permission Types:
- queue:read: ability to dequeue and peek messages from a queue
- queue:write: ability to enqueue messages to a queue
- queue:admin: ability to create, delete, configure, and manage queues
- queue:purge: ability to purge queue messages
- queue:monitor: ability to view queue metrics monitoring
- queue:dlq:manage: ability to manage dead-letter queue messages
- queue:system: special role for system queue management

27.2 Permission Assignment:
- Permissions assigned to roles: reader (queue:read), writer (queue:write), admin (all)
- Roles assigned to users/components via group membership
- Role inheritance: department roles apply to all dept.* queues
- Permission template: { "role": "writer", "queues": ["agent.data-validator.tasks"] }
- Permission for all queues: { "role": "admin", "queues": ["*"] }

27.3 Permission Caching:
- Permission evaluations cached for 30 seconds
- Cache invalidation on permission change events
- Grace period: cached permissions continue to apply for up to 30 seconds after revocation

---

## 28. Monitoring

28.1 Queue Monitoring:
- Queue depth: number of messages currently in queue (real-time)
- Queue depth history: time-series for trending
- Enqueue rate: messages per second (1 minute, 5 minute, 15 minute averages)
- Dequeue rate: messages per second
- Ack rate: message acknowledgments per second
- Consumer lag: messages behind consumer's current position
- Backpressure state: current level and duration

28.2 Consumer Monitoring:
- Consumer health: active, idle, slow, disconnected, timed out
- Consumer performance: messages/sec, latency, ack rate
- Consumer lag: depth of unprocessed messages
- Consumer prefetch utilization: how much of prefetch buffer is in use

28.3 Storage Monitoring:
- Disk usage per node: percentage, growth rate
- RocksDB statistics: compaction status, write amplification, cache hit rate
- Replication status: leader state, replication lag, replication volume

28.4 Health Check Endpoints:
- Liveness probe: is the queue node server process alive?
- Readiness probe: can the node accept requests (storage ready)?
- Health endpoint: detailed health report of all components

---

## 29. Logging

29.1 Log Categories:
- SYSTEM: System events (startup, shutdown, configuration changes)
- OPERATION: Queue operations (create, delete, configure)
- MESSAGE: Message lifecycle (enqueue, dequeue, ack, nack, dead-letter)
- ERROR: All error events
- AUDIT: Audit-related logs
- METRICS: Periodic metric snapshots

29.2 Log Format:
- Structured JSON log format
- Fields: timestamp, level, logger, message, queueName, messageId, producerId, consumerId, duration, result, error, correlationId
- Log levels: TRACE, DEBUG, INFO, WARN, ERROR, FATAL

29.3 Log Sampling:
- Message lifecycle logs: sample at 1% rate for high-volume queues (unless ERROR)
- Error logs: always recorded
- Audit logs: always recorded
- Security logs: always recorded

29.4 Log Management:
- Log rotation: daily or 1GB, whichever comes first
- Log retention: 30 days on local disk
- Log shipping: to centralized logging system (Elasticsearch/Loki)
- Log search: by queue name, message ID, consumer ID, time range

---

## 30. Metrics

30.1 Queue Metrics:
- queue.active.count (gauge): number of active queues per node
- queue.depth (gauge): current message count per queue
- queue.enqueue.rate (counter): messages enqueued per second
- queue.dequeue.rate (counter): messages dequeued per second
- queue.ack.rate (counter): messages acknowledged per second
- queue.latency.ms (histogram): end-to-end latency from enqueue to ack (p50, p95, p99)
- queue.delivery.latency.ms (histogram): time from enqueue to first delivery
- queue.consumer.lag (gauge): per consumer lag count
- queue.backpressure.level (gauge): current backpressure level (0-4)
- queue.dead.letter.count (counter): messages routed to DLQ
- queue.poison.detected (counter): poison messages identified
- queue.storage.size (gauge): storage space used per queue
- queue.error.rate (counter): error operations per second
- queue.enqueue.total (counter): lifetime total enqueued
- queue.dequeue.total (counter): lifetime total dequeued

30.2 Consumer Metrics:
- consumer.active.count (gauge): active consumers per queue
- consumer.ack.time (histogram): time for consumer to ack message
- consumer.prefetch.utilization (gauge): how full is prefetch buffer

30.3 System Metrics:
- node.heap.usage (gauge): JVM/process heap usage
- node.cpu.usage (gauge): CPU utilization percentage
- node.disk.usage (gauge): disk space used by queue storage
- node.network.io (counter): network bytes in/out per second
- node.grpc.connections (gauge): open gRPC connections

30.4 Metric Export:
- Metrics exported via StatsD protocol (UDP) to monitoring service
- Export interval: every 10 seconds
- Format: StatsD line protocol (tags for queue name, node ID)

---

## 31. Tracing

31.1 Trace Context:
- All queue operations propagate trace context using OpenTelemetry
- Trace context includes: traceId, spanId, parentSpanId
- Context propagated via gRPC metadata
- Enrich message records with traceIds for end-to-end tracing

31.2 Spans:
- Enqueue span: from request reception to response
- Dequeue span: from request to message retrieval
- Ack span: from ack reception to completion
- Storage span: any storage operation (RocksDB/Raft)
- Replication span: Raft replication round-trip

31.3 Trace Sampling:
- Default: 1% for standard messages (configurable)
- ERROR traces: 100% sampling
- Admin operations: 100% sampling
- Custom: configurable sampling rate per queue type

---

## 32. Scalability

32.1 Horizontal Scaling:
- Queue sharding: messages distributed by queue name hash across nodes
- Consistent hashing for minimal rebalancing on node changes
- Number of shards: configurable (default 64 per node, total depends on cluster size)
- Shard auto-rebalancing on node addition/removal
- Leader-follower per shard for Raft replicated queues
- Add nodes to increase throughput and reduce per-node message volume

32.2 Vertical Scaling:
- Increase memory for in-memory queues (RAM limit x4 GB)
- Faster storage (NVMe) for durable queues
- More CPU cores for message processing (gRPC handling)
- Read replica nodes for monitoring queries (offload metrics queries from primary)

32.3 Load Distribution:
- Producer requests routed to nearest node with queue shard
- Reader requests load-balanced across replica nodes for replicated queues
- Consumer push distribution: round-robin among available consumers
- Weighted distribution when consumers have different capacities

32.4 Scale Limits:
- Max queues per node: 5,000
- Max messages per node: 10,000,000
- Max enqueue per second per node: 100,000
- Max dequeue per second per node: 100,000
- Cluster max nodes: 100

---

## 33. Performance

33.1 Performance Targets:
- Enqueue latency (in-memory): p50 < 100us, p99 < 500us
- Enqueue latency (durable/RocksDB): p50 < 1ms, p99 < 5ms
- Enqueue latency (replicated/Raft): p50 < 5ms, p99 < 20ms
- Dequeue latency: p50 < 200us, p99 < 1ms
- Ack latency: p50 < 100us, p99 < 500us
- Throughput (in-memory): 100K enqueue/sec per node
- Throughput (durable): 50K enqueue/sec per node
- Throughput (replicated): 10K enqueue/sec per node

33.2 Performance Optimization:
- Zero-copy message serialization using Protobuf
- Direct memory buffers for large messages
- Read-ahead and write-behind caching for storage
- Pre-allocated connection pools for gRPC
- Batched writes for RocksDB
- Asynchronous disk I/O with io_uring on Linux
- Lock-free data structures for hot paths

33.3 Performance Testing:
- Regular performance testing with production-like load patterns
- Baseline benchmarks for each storage backend
- Performance regression testing in CI pipeline
- Profiling for hot-spot bottlenecks

---

## 34. Optimization

34.1 Message Batching:
- Producers can batch multiple messages in a single enqueue request (max 100)
- Consumers can request multiple messages in a single dequeue request (max 100)
- Batch partial failure handling: atomic by default, non-atomic optional
- Batch reduction in network overhead: up to 10x throughput improvement

34.2 Connection Pooling:
- Pre-configured gRPC connection pools between queue nodes
- Pool size: 10-100 connections per node pair
- Connection reuse for reduced establishment overhead
- Health checking for connections in pool

34.3 Memory Optimization:
- Object pooling for message data structures
- Direct byte buffers for large message bodies
- Off-heap memory allocation when possible
- Lazy message deserialization (headers first, body on access)

34.4 Query Optimization:
- Indexed fields on message records for fast retrieval: messageId, status, priority, timestamps
- No full table/range scans on queue data
- Periodic compaction to optimize storage layout
- Caching for frequent query patterns

---

## 35. Testing Strategy

35.1 Unit Testing:
- Test each component in isolation with mocks
- Test state machines: message, queue, consumer state transitions
- Test validation logic: queue name, message size, permission checks
- Test backpressure algorithm: depth calculation, level transitions, recovery
- Test priority sorting: P0-P4 ordering, aging promotion
- Coverage: 95% unit test branch coverage

35.2 Integration Testing:
- Test enqueue-dequeue flow end-to-end
- Test ack/nack flow and dead letter detection
- Test with each storage backend (in-memory, RocksDB)
- Test push consumption: registration, delivery, ack
- Test batch operations: batch enqueue, batch dequeue
- Test backpressure end-to-end: depth buildup and recovery
- Test timer wheel: delay messages, scheduled messages

35.3 Chaos Testing:
- Storage failure: kill RocksDB, verify recovery
- Consumer failure: kill consumer, verify message requeue
- Node failure: kill queue node, verify shard recovery
- Network partition: partition nodes, verify split-brain prevention
- Disk full: simulate disk full, verify backpressure

35.4 Performance Testing:
- Message throughput test per storage backend
- Latency distribution measurement (p50, p95, p99, p999)
- Concurrent consumer test with varying prefetch
- Backpressure behavior under sustained load
- Timer wheel accuracy under high delay message rate

---

## 36. Risk Analysis

| Risk | Impact | Likelihood | Mitigation |
|---|---|---|---|
| Queue data corruption | Data loss, task loss | Low (5%) | RocksDB WAL, Raft replication, checksum verification, regular backups |
| Storage full | Service unavailability | Medium (20%) | Disk monitoring, retention policies, backpressure, alert before full |
| Consumer runaway (infinite loop) | Message loss | Low (10%) | Prefetch limits, ack timeout, delivery count limit, dead-letter |
| Producer flood | System overload | Medium (30%) | Rate limiting per producer, quota per queue, backpressure shedding |
| Raft partition | No writes to partitioned queues | Low (5%) | Leader election with pre-vote, automated barrier removal |
| Poison message flood | DLQ growth, obscure real issues | Low (15%) | DLQ isolation, pattern analysis, alerting for poison rate |
| Message ordering violation | Incorrect processing | Low (5%) | Strict locking for FIFO, sequence number tracking |
| Single point node failure | Queue availability | Medium (10%) | Raft replication, automatic failover, cross-datacenter |
| Memory leak | Node crash | Low (5%) | Mem limit, GC monitoring, leak detection in CI |
| Long poll timeout explosion | Connection exhaustion | Low (5%) | Max long pollers per queue, connection pool limits |

---

## 37. Failure Scenarios

37.1 Catastrophic Node Failure: Complete loss of a queue node due to hardware failure or process crash. For durable queues with Raft replication: leader election within 150-300ms, remaining nodes continue serving, no data loss. For durable queues without replication: queue becomes unavailable until node recovers. Recovery: restore queue from RocksDB backup.

37.2 Storage Full: Disk containing queue storage reaches 100% capacity. Writes immediately fail. Reads may succeed from cache. Backpressure prevents further enqueue. Recovery: admin manually cleans old data, increases disk, or moves storage to another mount point.

37.3 Consumer Deadlock: Consumer acquires messages but crashes without acking them. After visibility timeout expiration, messages are requeued. If repeated delivery to same dead consumer, the delivery count increments until max attempts triggers dead-letter routing.

37.4 Network Partition Between Queue and Consumer: Consumer cannot reach queue or deliver results. Queue holds in-flight messages until acknowledgment timeout. After timeout, messages are requeued. If partition persists, messages cycle through retry until dead-lettered.

37.5 Replication Lag (Raft): Follower nodes fall behind leader due to high load or network issues. Leader continues serving writes. Remotely isolated follower may be expelled from quorum. When follower recovers, it syncs via snapshot from leader.

37.6 Timer Wheel Drift: System time changes significantly (NTP adjustment, DST). Scheduled messages may be delayed or released early. Use monotonic clock for timer wheel, NTP slew mode.

37.7 Idempotency Key Collision: Two different messages accidentally have the same idempotency key. The second enqueue returns DUPLICATE. Mitigation: use UUIDv4 with sufficient randomness, key includes producer ID prefix.

---

## 38. Recovery Procedures

38.1 Queue Node Recovery:
1. Detect node failure via heartbeat timeout (300ms)
2. For replicated queues: remaining nodes elect new leader (150-300ms)
3. For non-replicated queues: place queue as UNAVAILABLE, alert admin
4. When node restarts: RocksDB recovery from WAL
5. Node re-joins cluster: sync state from leader
6. Reshard if node detections indicate permanent removal
7. Verify queue node health before resuming full operations

38.2 Dead-Letter Queue Recovery:
1. Identify poison message pattern via DLQ analysis
2. Determine root cause: transient failure, bug, configuration error
3. For transient patterns: replay messages to original queue
4. For config issues: fix configuration, then replay messages
5. For persistent bugs: fix bug and deploy, then replay messages
6. Replay options: single message, batch, or all matching pattern
7. After replay: monitor re-consumption in normal queue

38.3 Storage Corruption:
1. Detect corruption via checksum mismatch or RocksDB error
2. Stop writes to affected queue
3. Restore from latest valid snapshot (RocksDB backup)
4. Replay WAL for operations since snapshot
5. If snapshots also corrupt: restore from replica (Raft)
6. If all replicas corrupt: last resort restore from full cluster backup
7. Verify data integrity after restore

38.4 Consumer Reconnection:
1. Consumer disconnects: queue adds consumer to reconnection list
2. Reconnection attempts: every 5 seconds, max 3 attempts
3. On successful reconnection: restore consumer to ACTIVE state
4. On failed reconnection after max: mark consumer as INACTIVE
5. Requeue any in-flight messages for failed consumer
6. Notify admin of failed consumer

38.5 Full Cluster Restart:
1. Stop all non-system enqueue operations
2. Wait for in-flight messages to ack or timeout (configurable, default 30 seconds)
3. For each node: flush WAL, take consistent RocksDB snapshot
4. Restart nodes one at a time in correct order (system queues first)
5. On each restart: verify Raft leader election, check for data consistency
6. After all nodes restarted: resume operations
7. Verify cluster health across all shards

---

## 39. Operational Limits

| Parameter | Maximum | Behavior When Exceeded |
|---|---|---|
| Max queue depth | 1,000,000 messages | Reject new enqueue requests (422) |
| Max message size | 1MB (10MB configurable) | Reject message (413) |
| Max queues per node | 5,000 | Reject queue creation (422) |
| Max consumers per queue | 1,000 | Reject consumer registration (422) |
| Max enqueue rate per producer | 1,000/second | 429 Too Many Requests |
| Max enqueue rate per queue | 10,000/second | Backpressure, potentially 429 |
| Max delivery attempts | 15 | Dead-letter after max attempts |
| Max TTL | 30 days | Cap TTL at 30 days |
| Max delay | 24 hours | Cap delay at 24 hours |
| Max schedule future | 1 year | Cap schedule at 1 year |
| Max headers per message | 20 | Reject additional headers |
| Max message body per batch | 100 | Reject batch with more than 100 |
| Max prefetch per consumer | 100 | Apply default (10) if more |
| Max ack timeout | 5 minutes | Cap timeout at 5 minutes |
| Max network timeout | 30 seconds | Cap timeout at 30 seconds |

---

## 40. Maintenance

40.1 Routine Maintenance:
- Compaction: weekly RocksDB compaction to optimize storage
- Backup: daily full backup for all durable queues
- Log rotation: rotate and compress queue system logs (daily or 1GB)
- Monitor disk usage: ensure < 80% utilization for queue storage
- Performance review: monthly performance metrics review

40.2 Queue Management:
- Orphaned queue detection: identify queues with no consumers for > 7 days
- Dead-letter review: weekly review of dead-letter queue patterns
- Configuration audit: monthly review of queue configuration for consistency
- Cleanup unused queues: move inactive queues to DRAINING state

40.3 Routine Upgrades:
- Rolling upgrades with zero downtime for replicated queues
- Upgrade order: system queues first, then service queues
- Backward compatibility: one version backward for gRPC API
- Migration scripts provided for breaking changes

---

## 41. Future Improvements

41.1 Queue Replication: Implement cross-datacenter queue replication for disaster recovery. Active-passive model where secondary datacenter can take over if primary fails. Asynchronous replication for performance, with RPO of 5 seconds.

41.2 Queue Analytics: Add built-in analytics for queue performance. Traffic analysis, trend prediction, and capacity planning recommendations. Automated suggestions for queue configuration optimization.

41.3 Multi-Region Support: Extend queue infrastructure for multi-region deployment with regional isolation. Provide awareness for data sovereignty and compliance. Support geo-fencing for message routing.

41.4 Streaming Queue Support: Add support for continuously streaming queues for real-time processing. Support for consumer groups and offset management. Integration with event streaming platforms.

41.5 Interactive Queue Simulation: Build a queue system simulator for capacity planning and what-if analysis. Model different load patterns and configurations. Provide recommendations for optimal configuration.

41.6 Message Priority Queuing: Enhanced priority implementation with sub-priorities within levels. Dynamic priority adjustment based on message age and SLA urgency. SLA-based priority escalation.

41.7 Self-Tuning Backpressure: ML-based backpressure tuning that learns optimal thresholds from historical patterns. Adaptive algorithms for backpressure level transitions. Automated recovery prediction.

---

## 42. Examples

42.1 Basic Enqueue and Dequeue:
Producer enqueues a task to the "agent.data-validator.tasks" queue. The message contains a request for data validation. A consumer agent polls for messages from the queue using long polling. The consumer receives the message, processes it, and sends an acknowledgment (ACK). The system considers the message successfully processed and removes it from the queue.

42.2 Delayed Message Enqueue:
A producer enqueues a reminder message with a delay of 86400000ms (24 hours). The message is stored in the DelayTimer component. After 24 hours, the timer expires and the message is automatically moved to the target queue. A consumer then dequeues and processes the reminder.

42.3 Poison Message Dead-Letter:
A message is consistently rejected by consumers due to invalid data format. Each consumer returns NACK with an error. After 5 delivery attempts (default max), the message is moved to the dead-letter queue. An admin reviews the DLQ, identifies the data format issue, and either replays a corrected version or discards the message.

42.4 Priority Queue Starvation Prevention:
A queue receives low-priority messages (P4) during a period of high-priority messages (P0). The priority manager ages low-priority messages over time. After 5 minutes, P4 messages promote to P3. After 10 minutes to P2, and after 30 minutes to P1, ensuring they eventually get processed.

42.5 Consumer Backpressure:
A consumer falls behind due to slow processing. Queue depth grows to 60% resulting in WARNING backpressure. The consumer's prefetch is reduced to help the consumer catch up. If the consumer continues to fall behind and depth reaches 95%, the queue enters SHEDDING level and new producer enqueues are rejected.

---

## 43. Acceptance Criteria

| AC | Description | Verification Method |
|---|---|---|
| AC-1 | FIFO order preserved within a single queue for messages at the same priority | Integration test with 1000 messages, verify delivery order |
| AC-2 | Priority queue returns highest priority messages first | Unit test with messages at all 5 priority levels |
| AC-3 | At-least-once delivery guaranteed for durable mode | Integration test: crash consumer after dequeue, verify message requeued |
| AC-4 | Exactly-once delivery via idempotency key | Integration test: enqueue with same key twice, verify only processed once |
| AC-5 | Backpressure activates when depth exceeds threshold | Unit test: set depth to 61%, verify WARNING level |
| AC-6 | Backpressure recovers when dequeue catches up | Integration test: simulate slow consumer, then fast consumer, verify recovery |
| AC-7 | Poison message moves to DLQ after max delivery attempts | Integration test: nack message max times, verify in DLQ |
| AC-8 | Queue survives process restart (DURABLE mode) | Chaos test: kill queue process, verify messages persist after restart |
| AC-9 | Raft replicated queue survives single node failure | Chaos test: kill 1 of 3 Raft nodes, verify no data loss |
| AC-10 | Long polling returns message within WaitTimeoutMs | Integration test: dequeue with 10s timeout, enqueue after 5s, verify message delivered |
| AC-11 | Delayed messages are invisible until after delay expires | Unit test: enqueue with 5s delay, try dequeue immediately, verify empty |
| AC-12 | Scheduled message released at or after scheduled timestamp | Unit test: schedule message 10s in future, verify released after 10s |
| AC-13 | Consumer disconnect triggers requeue of in-flight messages | Integration test: kill consumer with messages in flight, verify messages requeued |
| AC-14 | Cross-department queue isolation enforced | Integration test: attempt cross-department access, verify denied |
| AC-15 | Queue storage usage tracked and reported | Unit test: create queue, enqueue messages, verify storage metrics |
| AC-16 | Aging mechanism promotes low priority messages | Integration test: promote P4 to P3 after 5 minute aging timeout |
| AC-17 | Enqueue handles 10K msg/sec sustained (in-memory) | Performance test: sustained rate for 1 minute, verify no errors |
| AC-18 | Dequeue handles 10K msg/sec sustained | Performance test: sustained rate for 1 minute, verify throughput |
| AC-19 | Batch enqueue processes 100 messages atomically | Integration test: atomic batch of 100, verify all or nothing |
| AC-20 | Queue healthy endpoint reflects storage status | Unit test: verify healthy with storage up, degraded with storage down |

---

## 44. Definition of Done

| DoD | Criteria | Owner |
|---|---|---|
| DoD-1 | All acceptance criteria pass | QA |
| DoD-2 | 95% unit test branch coverage for core components | Engineering |
| DoD-3 | Integration tests for all queue types (FIFO, Priority, Delay, Scheduled, DLQ) | QA |
| DoD-4 | Integration tests for all storage backends (in-memory, RocksDB, Raft) | QA |
| DoD-5 | Performance targets met: 10K msg/sec in-memory, 50K durable, 10K replicated | Performance |
| DoD-6 | Security audit: authentication, authorization, encryption, audit trail | Security |
| DoD-7 | Monitoring dashboards and alerts configured | Ops |
| DoD-8 | Recovery procedures documented for all failure scenarios | Ops |
| DoD-9 | API documentation is complete including prototypes and examples | Engineering |
| DoD-10 | Operational limits defined, documented, and enforced | Engineering |
| DoD-11 | Detailed runbook for queue failure scenarios | Ops |
| DoD-12 | Performance baselines established and documented | Performance |
| DoD-13 | Chaos tests for storage failure, consumer failure, node failure | QA |
| DoD-14 | Tracing spans implemented for all operations | Engineering |
| DoD-15 | Capacity planning guide documented | Ops |

---

## End of Document 33: AIOS Queue System