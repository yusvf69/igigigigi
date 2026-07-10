# AIOS Database Manager — Engineering Specification v1.0

## Table of Contents
1. Purpose, Mission, Responsibilities, Non-Responsibilities
2. Architecture, Internal Components, External Components
3. Dependencies, Inputs, Outputs
4. Data Structures, Execution Flow, State Management
5. Communication Protocols, APIs, Events, Queues
6. Caching, Memory, Persistence
7. Validation, Retry Logic, Error Recovery
8. Security, Authentication, Authorization, Permissions
9. Monitoring, Logging, Metrics, Tracing
10. Scalability, Performance, Optimization
11. Testing Strategy, Risk Analysis
12. Failure Scenarios, Recovery Procedures
13. Operational Limits, Maintenance
14. Future Improvements, Examples
15. Acceptance Criteria, Definition of Done

---

## 1. Purpose, Mission, Responsibilities, Non-Responsibilities

### 1.1 Purpose
The AIOS Database Manager is the central data persistence layer for the entire AIOS ecosystem. It provides a unified abstraction over multiple database technologies — including PostgreSQL, TimescaleDB, Redis, and S3-compatible object storage — offering connection management, query routing, health monitoring, schema management, and compliance enforcement. Every AIOS component that requires durable storage, from the Memory System to the Model Registry to the Workflow Engine, relies on the Database Manager for database access.

### 1.2 Mission
To deliver a zero-trust, multi-tenant, multi-engine database access layer that ensures data durability, consistency, isolation, and governance across all AIOS components. The Database Manager abstracts away database-specific connection handling, failover, replication, and migration complexity, providing a simple, secure, and observable database access API.

### 1.3 Responsibilities
- Abstracting access to PostgreSQL, TimescaleDB, Redis, and S3-compatible storage behind a unified API
- Managing database connection pools with configurable min/max connections, connection timeouts, and health checks
- Routing queries based on type: read-write splitting, replica selection with lag awareness
- Optimizing queries through plan analysis and advisory index recommendations
- Managing database schemas with versioned, rolling migrations
- Coordinating distributed transactions across databases (XA or saga patterns)
- Integrating with database-native backup tools and managing backup schedules
- Monitoring database health: query latency, connection count, deadlocks, replication lag, storage usage
- Handling database failover: primary promotion, replica reconfiguration, connection drain
- Enforcing multi-tenant data isolation through schema-per-tenant or row-level security
- Managing data archival policies: tiered storage, retention schedules, purge operations
- Ensuring compliance: data retention enforcement, audit logging, purge policies, GDPR right-to-erasure
- Coordinating with all AIOS subsystems that require database access

### 1.4 Non-Responsibilities
- Does not implement application-level business logic
- Does not manage graph database access (delegated to Knowledge Graph Access)
- Does not manage vector database access (delegated to Vector Memory)
- Does not manage the Memory System's L1-L4 caching tiers
- Does not implement query language parsers (passes through to database drivers)
- Does not manage database user accounts or passwords directly (delegated to platform secrets management)
- Does not perform database software installation or upgrades (delegated to operations/k8s)
- Does not provide a data warehouse or analytics query engine
- Does not implement ETL pipelines or data transformation workflows

---

## 2. Architecture, Internal Components, External Components

### 2.1 Overall Architecture

The Database Manager follows a microservice architecture with component-specific database access layers coordinated by a central configuration and monitoring service.

```
┌──────────────────────────────────────────────────────────────────────────┐
│                        AIOS Database Manager                              │
│                                                                           │
│  ┌──────────────────────────────────────────────────────────────────┐    │
│  │                     Database Access API (gRPC)                    │    │
│  └──────────────────────────────────────────────────────────────────┘    │
│                                    │                                      │
│  ┌──────────────────────────────────────────────────────────────────┐    │
│  │                   Connection Pool Manager                         │    │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌────────────────────┐  │    │
│  │  │PostgreSQL│ │Timescale│ │  Redis   │ │   S3 Object        │  │    │
│  │  │ Pools   │ │  Pools  │ │  Pools   │ │   Storage Pools     │  │    │
│  │  └──────────┘ └──────────┘ └──────────┘ └────────────────────┘  │    │
│  └──────────────────────────────────────────────────────────────────┘    │
│                                    │                                      │
│  ┌──────────────────────────────────────────────────────────────────┐    │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌────────────────────┐  │    │
│  │  │ Query    │ │ Schema   │ │ Backup   │ │ Compliance         │  │    │
│  │  │ Router   │ │ Manager  │ │ Manager  │ │ Manager            │  │    │
│  │  └──────────┘ └──────────┘ └──────────┘ └────────────────────┘  │    │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌────────────────────┐  │    │
│  │  │ Migration│ │ Monitoring│ │ Failover │ │ Archival           │  │    │
│  │  │ Engine   │ │ Collector │ │ Handler  │ │ Manager            │  │    │
│  │  └──────────┘ └──────────┘ └──────────┘ └────────────────────┘  │    │
│  └──────────────────────────────────────────────────────────────────┘    │
└──────────────────────────────────────────────────────────────────────────┘
```

### 2.2 Internal Components

#### 2.2.1 Database Access API
Unified gRPC API providing database-agnostic query execution, transaction management, and schema operations.

#### 2.2.2 Connection Pool Manager
Manages HikariCP (PostgreSQL), Lettuce (Redis), and AWS SDK (S3) connection pools.

Per-database configuration:
- PostgreSQL: HikariCP with automatic pool sizing
- TimescaleDB: HikariCP with time-series-specific optimizations
- Redis: Lettuce connection pool with cluster-awareness
- S3: AWS SDK HTTP connection pool

#### 2.2.3 Query Router
Routes queries to appropriate database instance with read-write splitting.

Subcomponents:
- Query Classifier: Determines read vs write based on SQL parsing
- Replica Selector: Picks least-loaded replica with acceptable lag
- Read-Your-Writes Tracker: Ensures transactions see their own writes
- Query Cost Estimator: Estimates query cost for load-aware routing

#### 2.2.4 Schema Manager
Manages database schemas across all database engines.

Subcomponents:
- Schema Registry: Tracks schema versions per service per database
- Migration Engine: Applies versioned migrations in order
- Migration Validator: Validates migration scripts (syntax, impact analysis)
- Schema Diff Tool: Compares schemas across environments

#### 2.2.5 Migration Engine
Handles schema migrations with forward-only, versioned migrations.

Subcomponents:
- Migration Runner: Executes migration scripts in transactions
- Lock Manager: Prevents concurrent migrations
- Dry-Run Executor: Tests migrations without applying
- Rollback Commander: Applies rollback scripts (for reversible migrations)

#### 2.2.6 Backup Manager
Integrates with database-native backup tools for consistent backups.

Subcomponents:
- Backup Scheduler: Triggers backups per schedule
- Backup Coordinator: Coordinates multi-database backup consistency
- Restore Validator: Tests backup restorability
- Retention Manager: Enforces backup retention policies

#### 2.2.7 Monitoring Collector
Collects comprehensive database health metrics.

Subcomponents:
- Query Latency Collector
- Connection Pool Stats Collector
- Deadlock Tracker
- Replication Lag Monitor
- Storage Usage Collector
- Slow Query Log Collector

#### 2.2.8 Failover Handler
Coordinates database failover with minimal downtime.

Subcomponents:
- Health Checker: Probes database health via query + connection tests
- Failover Coordinator: Orchestrates primary promotion
- Connection Drainer: Gracefully drains connections during failover
- Re-connection Manager: Re-establishes connections post-failover

#### 2.2.9 Compliance Manager
Enforces data retention, purging, and auditing policies.

#### 2.2.10 Archival Manager
Manages tiered storage and data archival to S3-compatible object storage.

### 2.3 External Components

| Component | Integration | Protocol |
|-----------|-------------|----------|
| PostgreSQL 15+ | Primary relational database | PostgreSQL wire protocol |
| TimescaleDB 2.10+ | Time-series data | PostgreSQL wire protocol |
| Redis 7.0+ | Caching, session, queues | Redis RESP3 |
| S3-compatible Storage | Object storage, backups | S3 REST API |
| Kubernetes | Orchestration, secrets | k8s API |
| Vault / External Secrets | Database credentials | REST |
| Monitoring Stack | Metrics export | Prometheus |
| Logging System | Structured logging | Log stream |
| Config Service | Dynamic configuration | gRPC |

---

## 3. Dependencies, Inputs, Outputs

### 3.1 Dependencies

#### 3.1.1 Runtime Dependencies
- PostgreSQL 15+ (primary database engine)
- TimescaleDB 2.10+ extension to PostgreSQL
- Redis 7.0+ (ElastiCache or self-hosted cluster)
- S3-compatible object storage (MinIO, AWS S3, GCS)
- AIOS Security Module for credential management
- AIOS Config Service for dynamic configuration

#### 3.1.2 Infrastructure Dependencies
- Kubernetes 1.24+ with StatefulSet support for databases
- SSD/NVMe storage for PostgreSQL WAL and data
- 10GbE+ network for database replication
- PersistentVolume support for database storage

### 3.2 Inputs

| Input | Source | Format | Description |
|-------|--------|--------|-------------|
| ExecuteQuery | Service | Protobuf | SQL query + parameters |
| ExecuteBatch | Service | Protobuf | Batch of queries |
| BeginTransaction | Service | Protobuf | Transaction start with isolation level |
| CommitTransaction | Service | Protobuf | Transaction commit |
| RollbackTransaction | Service | Protobuf | Transaction rollback |
| ApplyMigration | Admin | Protobuf | Migration script |
| CreateBackup | Admin | Protobuf | Backup request |
| RestoreBackup | Admin | Protobuf | Restore request |
| PurgeData | Admin | Protobuf | Compliance data purge |
| ArchiveData | Admin | Protobuf | Archival request |

### 3.3 Outputs

| Output | Destination | Format | Description |
|--------|-------------|--------|-------------|
| QueryResult | Service | Protobuf | Result rows + metadata |
| BatchResult | Service | Protobuf | Per-statement results |
| TransactionStatus | Service | Protobuf | Transaction state |
| MigrationStatus | Admin | JSON | Migration progress/result |
| BackupStatus | Admin | JSON | Backup progress/location |
| HealthStatus | Monitoring | JSON | Database health details |
| Metrics | Monitoring | Prometheus | All database metrics |

---

## 4. Data Structures, Execution Flow, State Management

### 4.1 Data Structures

#### 4.1.1 DatabaseConfig
```
DatabaseConfig {
    db_id: UUID
    engine: DatabaseEngine  // POSTGRESQL, TIMESCALEDB, REDIS, S3
    name: String
    host: String
    port: uint32
    database_name: String
    username_ref: String  // reference to secret
    ssl_mode: SSLMode  // DISABLE, ALLOW, PREFER, REQUIRE, VERIFY_CA, VERIFY_FULL
    pool_config: PoolConfig
    replica_configs: List<ReplicaConfig>
    read_preference: ReadPreference  // PRIMARY, PRIMARY_PREFERRED, SECONDARY, NEAREST
    tags: Map<String, String>
    tenant_id: Option<String>
}
```

#### 4.1.2 PoolConfig
```
PoolConfig {
    min_connections: uint32  // default: 5
    max_connections: uint32  // default: 50
    connection_timeout_ms: uint32  // default: 5000
    idle_timeout_ms: uint32  // default: 600000 (10 min)
    max_lifetime_ms: uint32  // default: 1800000 (30 min)
    keepalive_time_ms: uint32  // default: 30000
    validation_query: String  // default: "SELECT 1"
    validation_interval_ms: uint32  // default: 5000
    leak_detection_threshold_ms: uint32  // default: 60000
}
```

#### 4.1.3 ReplicaConfig
```
ReplicaConfig {
    replica_id: UUID
    host: String
    port: uint32
    read_only: bool
    weight: uint32  // for weighted load balancing
    max_lag_bytes: uint64  // max acceptable replication lag
    max_lag_seconds: uint32  // max acceptable lag in seconds
    tags: Map<String, String>
}
```

#### 4.1.4 MigrationScript
```
MigrationScript {
    migration_id: UUID
    version: uint64  // sequential version number
    description: String
    engine: DatabaseEngine
    database: String
    forward_script: String  // SQL to apply migration
    rollback_script: Option<String>  // SQL to revert migration
    checksum: bytes
    author: String
    created_at: Timestamp
    applied_at: Option<Timestamp>
    applied_by: Option<String>
    status: MigrationStatus  // PENDING, APPLYING, APPLIED, FAILED, ROLLED_BACK
    execution_time_ms: Option<uint64>
    depends_on: Option<uint64>
    batch_size: Option<uint32>  // for large data migrations
}
```

#### 4.1.5 BackupManifest
```
BackupManifest {
    backup_id: UUID
    engine: DatabaseEngine
    database: String
    backup_type: BackupType  // FULL, INCREMENTAL, LOGICAL, SNAPSHOT
    start_time: Timestamp
    end_time: Option<Timestamp>
    size_bytes: uint64
    location: String  // S3 URL
    encryption: EncryptionInfo
    checksum: bytes
    status: BackupStatus  // RUNNING, COMPLETED, FAILED, VERIFIED
    includes_wal: bool
    base_backup: Option<UUID>
    metadata: Map<String, String>
}
```

#### 4.1.6 TransactionContext
```
TransactionContext {
    transaction_id: UUID
    service: String
    database: String
    isolation_level: IsolationLevel
    status: TransactionStatus
    started_at: Timestamp
    last_active: Timestamp
    timeout: Duration
    statements: uint32
    connection_id: String
    xid: Option<uint64>  // database transaction ID
}
```

#### 4.1.7 QueryResult
```
QueryResult {
    columns: List<ColumnInfo>
    rows: List<Row>
    row_count: uint32
    truncated: bool
    execution_time_ms: float64
    database: String
    replica_used: Option<String>
    query_id: UUID
    warnings: List<String>
}
```

#### 4.1.8 RetentionPolicy
```
RetentionPolicy {
    policy_id: UUID
    database: String
    table: String
    field: String  // timestamp or date field
    retention_duration: Duration
    purge_batch_size: uint32  // default: 10000
    purge_interval: Duration
    archive_before_purge: bool
    archive_location: Option<String>
    enabled: bool
    created_at: Timestamp
}
```

#### 4.1.9 ArchiveManifest
```
ArchiveManifest {
    archive_id: UUID
    database: String
    table: String
    field: String
    range_start: Timestamp
    range_end: Timestamp
    row_count: uint64
    size_bytes: uint64
    format: String  // PARQUET, CSV, JSON
    location: String  // S3 URL
    compression: String
    checksum: bytes
    created_at: Timestamp
    status: ArchiveStatus  // CREATING, COMPLETE, FAILED, RESTORING
}
```

### 4.2 Execution Flow

#### 4.2.1 Query Execution Flow
1. Service sends ExecuteQuery to Database Manager
2. Database Manager authenticates service identity
3. Connection Pool Manager selects or creates connection:
   a. For read queries: select replica based on read preference + lag
   b. For write queries: use primary connection
   c. For transactional queries: use existing transaction connection
4. Query Router validates query:
   a. Parameterized query check (SQL injection prevention)
   b. Statement type classification (SELECT, INSERT, UPDATE, DELETE, DDL, etc.)
   c. Permission check (service authorized for table/operation?)
   d. Resource limit check (timeout, max rows)
5. Connection acquired from pool with timeout
6. Query executed on database:
   a. Statement prepared (if prepared statement caching enabled)
   b. Parameters bound
   c. Query executed with configured timeout
   d. Statement.close() in finally block
7. Result fetched and transformed into QueryResult protobuf
8. Connection returned to pool
9. Metrics recorded (query latency, rows returned, connection used)
10. Response returned to service

#### 4.2.2 Migration Execution Flow
1. Admin or CI/CD pipeline submits migration scripts
2. Migration Engine validates scripts (syntax check, dry run)
3. Migration Engine acquires migration lock (advisory lock or database lock)
4. Engine reads current schema version from migration tracking table
5. Engine identifies pending migrations (sorted by version)
6. Each migration applied in order:
   a. Version recorded with APPLYING status
   b. Forward script executed in transaction
   c. On success: version → APPLIED, execution time recorded
   d. On failure: version → FAILED, transaction rolled back
   e. If rollback script exists: execute rollback
7. If all migrations applied: release lock, update schema cache
8. If any migration failed: alert admin, preserve state for investigation
9. Cache invalidated for affected database(s)

#### 4.2.3 Backup Execution Flow
1. Backup schedule or manual request triggers backup
2. Backup Manager determines backup type:
   a. PostgreSQL: pg_dump for logical, pg_basebackup for physical
   b. TimescaleDB: pg_dump with timescaledb-specific flags
   c. Redis: SAVE or BGSAVE (configurable)
   d. S3: no backup needed (native replication)
3. Backup lock acquired (prevents conflicting operations):
   a. PostgreSQL: no lock (online backup with WAL archiving)
   b. Redis: BGSAVE (non-blocking)
4. Backup executed:
   a. Source writes backup to temporary location
   b. Backup Manager compresses and encrypts backup
   c. Upload to S3 with manifest metadata
   d. Verify checksum post-upload
5. Cleanup temporary files
6. Retention policy applied (delete old backups)
7. Backup status recorded with full manifest
8. Restore test: optionally validate backup integrity with test restore

#### 4.2.4 Failover Flow
1. Health Checker detects primary unavailability (3 consecutive failures)
2. Alert triggered: primary down
3. Failover Handler begins coordinated failover:
   a. Verify replica is caught up (lag < threshold)
   b. Promote replica: pg_ctl promote or Patroni failover
   c. Update DNS or service discovery with new primary address
   d. Reconfigure replicas to follow new primary
4. Connection Pool Manager:
   a. Mark old primary connections as invalid
   b. Drain in-flight transactions from old primary
   c. Create new connection pools pointing to new primary
5. Read replicas continue serving (transparent to services)
6. New primary verified healthy (test queries)
7. Failover event logged with full timeline
8. Old primary repaired and added as replica

#### 4.2.5 Data Purge Flow
1. Compliance Manager evaluates retention policies
2. For each policy:
   a. Compute purge threshold date (now - retention_duration)
   b. Select rows to delete in batches (purge_batch_size)
   c. If archive_before_purge: archive rows to S3 first
   d. Execute batch DELETE with LIMIT
   e. VACUUM affected table (or schedule for off-peak)
   f. Log purged row count and duration
3. If purge fails mid-way: log error, resume from last checkpoint

### 4.3 State Management

#### 4.3.1 Connection State Machine
```
CREATED → VALIDATED (health check passes)
CREATED → FAILED (health check fails)
VALIDATED → IN_USE (assigned to query)
VALIDATED → LEAK_DETECTED (not returned within threshold)
VALIDATED → EVICTED (idle timeout or max lifetime)
IN_USE → VALIDATED (returned to pool)
IN_USE → FAILED (query error, connection broken)
LEAK_DETECTED → EVICTED (forced close)
FAILED → CLOSED (released)
```

#### 4.3.2 Migration State Machine
```
PENDING → APPLYING (lock acquired, script starts)
APPLYING → APPLIED (success, transaction committed)
APPLYING → FAILED (error, transaction rolled back)
FAILED → APPLYING (retry after fix)
FAILED → ROLLED_BACK (rollback applied)
ROLLED_BACK → APPLYING (re-apply after fix)
APPLIED → SUPERSEDED (newer migration on same schema)
```

#### 4.3.3 Database State
```
ONLINE → DEGRADED (high latency, connection issues)
ONLINE → READ_ONLY (primary down, replicas serving reads)
DEGRADED → ONLINE (recovered)
DEGRADED → FAILED (unrecoverable)
READ_ONLY → ONLINE (primary restored)
READ_ONLY → FAILED (all replicas lost)
FAILED → RECOVERING (restore in progress)
RECOVERING → ONLINE (restore complete)
```

#### 4.3.4 Transaction State Management
- Each transaction tracked with timeout and heartbeat
- Idle-in-transaction detection: rollback if idle > threshold
- Long-running transaction alerts
- Deadlock detection via PostgreSQL logs, auto-rollback notified

---

## 5. Communication Protocols, APIs, Events, Queues

### 5.1 Communication Protocols

| Protocol | Usage | Transport | Serialization |
|----------|-------|-----------|---------------|
| gRPC | Service ←→ Database Manager | HTTP/2 | Protobuf |
| PostgreSQL Wire | DB Manager ←→ PostgreSQL | TCP | Binary |
| Redis RESP3 | DB Manager ←→ Redis | TCP | Binary |
| S3 REST | DB Manager ←→ S3 | HTTPS | XML/JSON |
| Internal RPC | Internal components | Unix Socket | Protobuf |

### 5.2 APIs

#### 5.2.1 Public gRPC API

```
service DatabaseManagerService {
    // Query execution
    rpc Execute(ExecuteRequest) returns (ExecuteResponse);
    rpc ExecuteBatch(ExecuteBatchRequest) returns (ExecuteBatchResponse);
    rpc ExecuteStream(ExecuteStreamRequest) returns (stream Row);
    
    // Prepared statements
    rpc Prepare(PrepareRequest) returns (PrepareResponse);
    rpc ExecutePrepared(ExecutePreparedRequest) returns (ExecutePreparedResponse);
    rpc ClosePrepared(ClosePreparedRequest) returns (ClosePreparedResponse);
    
    // Transactions
    rpc Begin(BeginRequest) returns (BeginResponse);
    rpc Commit(CommitRequest) returns (CommitResponse);
    rpc Rollback(RollbackRequest) returns (RollbackResponse);
    rpc Savepoint(SavepointRequest) returns (SavepointResponse);
    rpc RollbackToSavepoint(RollbackToSavepointRequest) returns (RollbackToSavepointResponse);
    
    // Schema management (admin)
    rpc GetSchemaVersion(GetSchemaVersionRequest) returns (GetSchemaVersionResponse);
    rpc ApplyMigration(ApplyMigrationRequest) returns (ApplyMigrationResponse);
    rpc RollbackMigration(RollbackMigrationRequest) returns (RollbackMigrationResponse);
    rpc ListMigrations(ListMigrationsRequest) returns (ListMigrationsResponse);
    rpc ValidateMigration(ValidateMigrationRequest) returns (ValidateMigrationResponse);
    rpc DryRunMigration(DryRunMigrationRequest) returns (DryRunMigrationResponse);
    
    // Backup and restore (admin)
    rpc CreateBackup(CreateBackupRequest) returns (CreateBackupResponse);
    rpc GetBackupStatus(GetBackupStatusRequest) returns (GetBackupStatusResponse);
    rpc ListBackups(ListBackupsRequest) returns (ListBackupsResponse);
    rpc RestoreBackup(RestoreBackupRequest) returns (RestoreBackupResponse);
    rpc ValidateBackup(ValidateBackupRequest) returns (ValidateBackupResponse);
    rpc DeleteBackup(DeleteBackupRequest) returns (DeleteBackupResponse);
    
    // Compliance (admin)
    rpc SetRetentionPolicy(SetRetentionPolicyRequest) returns (SetRetentionPolicyResponse);
    rpc GetRetentionPolicy(GetRetentionPolicyRequest) returns (GetRetentionPolicyResponse);
    rpc ListRetentionPolicies(ListRetentionPoliciesRequest) returns (ListRetentionPoliciesResponse);
    rpc TriggerPurge(TriggerPurgeRequest) returns (TriggerPurgeResponse);
    rpc GetPurgeStatus(GetPurgeStatusRequest) returns (GetPurgeStatusResponse);
    
    // Archival (admin)
    rpc ArchiveData(ArchiveDataRequest) returns (ArchiveDataResponse);
    rpc RestoreArchivedData(RestoreArchivedDataRequest) returns (RestoreArchivedDataResponse);
    rpc ListArchives(ListArchivesRequest) returns (ListArchivesResponse);
    rpc GetArchiveStatus(GetArchiveStatusRequest) returns (GetArchiveStatusResponse);
    
    // Monitoring
    rpc GetDatabaseHealth(GetDatabaseHealthRequest) returns (GetDatabaseHealthResponse);
    rpc GetPoolStats(GetPoolStatsRequest) returns (GetPoolStatsResponse);
    rpc GetSlowQueries(GetSlowQueriesRequest) returns (GetSlowQueriesResponse);
    rpc GetReplicationStatus(GetReplicationStatusRequest) returns (GetReplicationStatusResponse);
    rpc GetMetrics(GetDBMetricsRequest) returns (GetDBMetricsResponse);
    rpc GetIndexRecommendations(IndexRecommendationsRequest) returns (IndexRecommendationsResponse);
}
```

#### 5.2.2 REST Admin API

```
GET    /api/v1/db/databases                    - List databases
GET    /api/v1/db/databases/{id}               - Get database config
PUT    /api/v1/db/databases/{id}               - Update database config
POST   /api/v1/db/databases/{id}/test          - Test connection
GET    /api/v1/db/databases/{id}/health        - Database health
GET    /api/v1/db/pools                        - Pool stats
GET    /api/v1/db/migrations                   - List migrations
POST   /api/v1/db/migrations                   - Apply migration
POST   /api/v1/db/migrations/{id}/rollback     - Rollback migration
POST   /api/v1/db/migrations/validate          - Validate migration
POST   /api/v1/db/backups                      - Create backup
GET    /api/v1/db/backups                      - List backups
GET    /api/v1/db/backups/{id}                 - Backup status
POST   /api/v1/db/backups/{id}/restore         - Restore backup
DELETE /api/v1/db/backups/{id}                 - Delete backup
POST   /api/v1/db/retention                    - Set retention policy
POST   /api/v1/db/purge                        - Trigger purge
GET    /api/v1/db/archives                     - List archives
POST   /api/v1/db/archives                     - Archive data
POST   /api/v1/db/archives/{id}/restore        - Restore archive
GET    /api/v1/db/slow-queries                 - Slow query log
GET    /api/v1/db/replication                  - Replication status
GET    /api/v1/db/index-recommendations        - Index recommendations
```

### 5.3 Events

```
DatabaseConnectionEvent {
    event_id: UUID
    event_type: "db.connection"
    timestamp: Timestamp
    database: String
    operation: ConnectionOperation  // POOL_CREATED, CONNECTION_OPENED, CONNECTION_CLOSED, POOL_EXHAUSTED
    pool_active: uint32
    pool_idle: uint32
    pool_pending: uint32
}

DatabaseQueryEvent {
    event_id: UUID
    event_type: "db.query"
    timestamp: Timestamp
    database: String
    query_hash: String
    execution_time_ms: float64
    rows_returned: uint32
    success: bool
    replica_used: Option<String>
    query_id: UUID
}

DatabaseFailoverEvent {
    event_id: UUID
    event_type: "db.failover"
    timestamp: Timestamp
    database: String
    old_primary: String
    new_primary: String
    failover_duration_ms: uint64
    data_loss_risk: DataLossRisk
    trigger: FailoverTrigger  // HEALTH_CHECK, MANUAL, AUTOMATIC
}

DatabaseMigrationEvent {
    event_id: UUID
    event_type: "db.migration"
    timestamp: Timestamp
    database: String
    version: uint64
    description: String
    status: MigrationStatus
    execution_time_ms: Option<uint64>
    error: Option<String>
}

DatabasePurgeEvent {
    event_id: UUID
    event_type: "db.purge"
    timestamp: Timestamp
    database: String
    table: String
    purged_rows: uint64
    retention_duration: Duration
    archived: bool
}
```

### 5.4 Queues

| Queue | Purpose | Backend |
|-------|---------|---------|
| db.query.async | Async query execution | Kafka |
| db.migration | Migration job queue | NATS |
| db.backup | Backup job queue | NATS |
| db.restore | Restore job queue | NATS |
| db.purge | Data purge queue | NATS |
| db.archive | Data archival queue | NATS |
| db.events | Database events stream | Kafka |
| db.metrics | Metrics aggregation | Kafka |

---

## 6. Caching, Memory, Persistence

### 6.1 Caching Strategy

#### 6.1.1 Query Result Cache (Database Manager)
- Optional query result cache for repeated read queries
- Cache key: hash(normalized_query + parameters + database)
- TTL: Configurable per query pattern (default 300 seconds for reference data, 0 for transactional)
- Size: Configurable (default 10,000 entries)
- Invalidation: On write to same table(s) referenced in query
- Location: L2 session cache (Redis) for low latency

#### 6.1.2 Prepared Statement Cache
- Prepared statements cached per connection pool
- LRU eviction when pool max prepared statements reached
- Key: SQL query text
- Value: PreparedStatement handle
- Max per pool: 256 statements

#### 6.1.3 Connection Pool Health Cache
- Replica health and lag data cached for 1 second
- Avoids querying pg_stat_replication on every request
- Updated by background monitoring goroutine

### 6.2 Persistence

#### 6.2.1 Database Persistence
- PostgreSQL: Full ACID compliance, WAL for crash recovery
- TimescaleDB: Time-series data with automatic partitioning
- Redis: AOF + RDB persistence for recovery
- S3: 11x9 durability with cross-region replication

#### 6.2.2 Migration Tracking
- Migration state stored in dedicated table: `_aios_schema_migrations`
- Contains version, checksum, applied_at, status
- Never modified except by Migration Engine

#### 6.2.3 Backup Persistence
- All backups stored in S3-compatible object storage
- Organized by: /backups/{engine}/{database}/{type}/{timestamp}/
- Metadata in backup manifest (PostgreSQL-style)
- Transaction log / WAL archive in separate bucket

---

## 7. Validation, Retry Logic, Error Recovery

### 7.1 Validation

#### 7.1.1 Query Validation
- Parameterized queries only (SQL injection prevention)
- DDL statement validation (DANGEROUS operations require admin role)
- Query length limit: 100,000 characters
- Max parameters: 10,000 per query
- Max batch size: 1,000 statements
- Statement type classification and permission check
- Read-only validation: ensure read-only transactions don't write

#### 7.1.2 Connection Validation
- Connection health check on borrow from pool
- Connection validation interval: every 5 seconds
- SSL/TLS certificate validation
- Credential rotation detection (reject connections with rotated credentials)

#### 7.1.3 Migration Validation
- Syntax check on migration scripts
- Dry-run execution against test database
- Impact analysis (which tables/columns affected)
- Checksum verification (detect script tampering)
- Dependency ordering validation
- Rollback script requirement for PRODUCTION migrations

### 7.2 Retry Logic

#### 7.2.1 Retry Configuration
```
DBRetryConfig {
    transaction_retry: RetryPolicy {
        max_attempts: 3
        base_delay_ms: 100
        max_delay_ms: 1000
        multiplier: 2.0
        jitter_ms: 20
        retryable_errors: [
            SERIALIZATION_FAILURE, DEADLOCK_DETECTED,
            CONNECTION_RESET, CONNECTION_TIMEOUT, 
            REPLICA_LAG, READ_ONLY_MODE
        ]
    },
    connection_retry: RetryPolicy {
        max_attempts: 5
        base_delay_ms: 500
        max_delay_ms: 10000
        multiplier: 2.0
        jitter_ms: 100
        retryable_errors: [
            CONNECTION_REFUSED, HOST_UNREACHABLE,
            AUTHENTICATION_FAILED (after credential refresh)
        ]
    },
    migration_retry: RetryPolicy {
        max_attempts: 2
        base_delay_ms: 1000
        max_delay_ms: 5000
        multiplier: 2.0
        retryable_errors: [LOCK_TIMEOUT, DEADLOCK_DETECTED]
    }
}
```

### 7.3 Error Recovery

#### 7.3.1 Connection Pool Exhaustion Recovery
1. Pool Exhaustion Detected (all connections active, queue full)
2. Auto-scale: increase max_connections (if within DB limits)
3. If cannot increase: return SERVER_BUSY with Retry-After header
4. Log pool exhaustion with caller details
5. Identify connection leaks via stack traces (leak detection)
6. Force-close leaked connections
7. Alert: potential connection leak or traffic spike
8. If persistent: trigger auto-scaling of service instances

#### 7.3.2 Replication Lag Recovery
1. Monitor detects replica lag > threshold
2. Remove lagging replica from read routing pool
3. Investigate cause (long-running query on replica, network issue, WAL accumulation)
4. If long-running query: cancel query on replica
5. If network: check connectivity, restart replication
6. When lag resolved: warm replica cache, return to pool

#### 7.3.3 Data Corruption Recovery
1. Detect corruption (CHECKSUM failure, index corruption, unexpected error)
2. Isolate affected database or table
3. Mark as DEGRADED (read-only if safe, otherwise offline)
4. Restore from latest backup:
   a. Restore to separate environment
   b. Replay WAL to point before corruption
   c. Verify integrity
5. If corruption limited: restore affected table from backup
6. If widespread: full database restore
7. Validate post-restore integrity
8. Return to service

#### 7.3.4 Migration Failure Recovery
1. Migration fails (syntax error, constraint violation, timeout)
2. Transaction automatically rolled back
3. Migration status → FAILED in tracking table
4. If forward migration partially applied (non-transactional DDL):
   a. Apply rollback script if available
   b. Or manually revert changes
5. Notify admin with error details and migration context
6. After fix: restart migration (retries from failed version)
7. If unresolvable: manual DBA intervention required

---

## 8. Security, Authentication, Authorization, Permissions

### 8.1 Security Architecture

#### 8.1.1 Network Security
- mTLS for all gRPC communication with Database Manager
- PostgreSQL connections over SSL/TLS with certificate verification
- Redis connections over TLS (Stunnel or native TLS)
- S3 over HTTPS with TLS 1.2 minimum
- Network policies restrict database access to Database Manager only

#### 8.1.2 Credential Management
- Database credentials stored in external secrets management (Vault)
- Automatic credential rotation (30-day rotation policy)
- Never log credentials or connection strings
- Per-service database accounts (not shared)
- Connection string assembled at runtime from secret references

#### 8.1.3 Data-at-Rest Encryption
- PostgreSQL: Transparent Data Encryption (TDE) or pg_tde extension
- Redis: AOF encryption, RDB encryption
- S3: Server-side encryption with AES-256
- Backup files: Client-side AES-256-GCM encryption

#### 8.1.4 Audit Logging
- All DDL operations (CREATE, ALTER, DROP) audited
- All schema migrations audited
- All backup/restore operations audited
- All purge/compliance operations audited
- Failed login attempts audited

### 8.2 Authentication
- Service-to-service mTLS authentication
- JWT-based authentication for admin operations
- Database authentication via service-specific credentials
- Secrets rotated every 30 days

### 8.3 Authorization

#### 8.3.1 Permission Model
```
db:query:execute:{database}:{table_pattern}:{operation}
db:schema:read:{database}
db:schema:write:{database}
db:admin:{database}
db:backup:create
db:backup:restore
db:migration:apply
db:compliance:purge
```

#### 8.3.2 RBAC Roles
- DB_READER: Execute SELECT on assigned tables
- DB_WRITER: Execute INSERT/UPDATE/DELETE on assigned tables
- DB_DDL: Execute DDL on assigned tables (schema changes)
- DB_ADMIN: Full database administration
- DB_BACKUP_ADMIN: Backup and restore operations

### 8.4 Permissions

#### 8.4.1 Table-Level Access
- Each service assigned specific tables for read/write
- Cross-service table access requires explicit grant
- System tables (_aios_*): DB_ADMIN only

#### 8.4.2 Tenant Isolation
- Schema-per-tenant: Each tenant has separate schema in shared database
- Row-level security: PostgreSQL RLS policies enforce tenant isolation
- Connection tagging: connections tagged with tenant_id for audit

---

## 9. Monitoring, Logging, Metrics, Tracing

### 9.1 Metrics

#### 9.1.1 Core Metrics
```
# Connection pool metrics
aios_db_pool_active{db="postgres-main", pool="primary"} 15
aios_db_pool_idle{db="postgres-main", pool="primary"} 35
aios_db_pool_pending{db="postgres-main", pool="primary"} 0
aios_db_pool_max{db="postgres-main", pool="primary"} 50
aios_db_pool_timeout_total{db="postgres-main"} 2
aios_db_pool_connection_create_seconds{db="postgres-main"} 0.005

# Query metrics
aios_db_queries_total{db="postgres-main", type="select"} 2500000
aios_db_queries_total{db="postgres-main", type="write"} 500000
aios_db_query_duration_seconds{db="postgres-main", quantile="0.5"} 0.002
aios_db_query_duration_seconds{db="postgres-main", quantile="0.99"} 0.500
aios_db_query_errors_total{db="postgres-main", error="syntax"} 15
aios_db_query_errors_total{db="postgres-main", error="constraint"} 42

# Replication metrics
aios_db_replication_lag_bytes{db="postgres-main", replica="r1"} 5242880
aios_db_replication_lag_seconds{db="postgres-main", replica="r1"} 0.5
aios_db_replication_status{db="postgres-main", replica="r1"} 1
aios_db_replication_count{db="postgres-main"} 3

# Storage metrics
aios_db_storage_used_bytes{db="postgres-main"} 107374182400
aios_db_storage_total_bytes{db="postgres-main"} 1099511627776
aios_db_storage_usage_ratio{db="postgres-main"} 0.098
aios_db_wal_size_bytes{db="postgres-main"} 1073741824

# Migration metrics
aios_db_migration_total 342
aios_db_migration_duration_seconds{version="341"} 2.5
aios_db_migration_failure_total 1
aios_db_schema_version{db="postgres-main"} 342

# Backup metrics
aios_db_backup_duration_seconds 1800
aios_db_backup_size_bytes 53687091200
aios_db_backup_total 150
aios_db_backup_failure_total 0

# Deadlock metrics
aios_db_deadlock_total{db="postgres-main"} 5
aios_db_long_running_transactions{db="postgres-main"} 0

# Slow query metrics
aios_db_slow_queries_total{db="postgres-main", threshold="1s"} 120
aios_db_slow_queries_total{db="postgres-main", threshold="10s"} 5
```

### 9.2 Logging

#### 9.2.1 Log Levels
- ERROR: Failed queries, connection failures, migration failures, backup failures
- WARN: Slow queries, connection pool near exhaustion, replica lag, replication issues
- INFO: Normal operations, migrations applied, backups created, pool events
- DEBUG: Query execution details, connection lifecycle
- TRACE: Full SQL text (off by default, security-restricted)

#### 9.2.2 Audit Logging
- All schema changes
- All data purges
- All backup/restore operations
- All migration operations
- All configuration changes
- Access to sensitive tables/data

### 9.3 Tracing

#### 9.3.1 Trace Spans
```
DBExecute → AcquireConnection → PrepareStatement → ExecuteQuery → FetchResults → ReturnConnection
DBTransaction → AcquireConnection → Begin → Execute → Execute → Commit/ → ReturnConnection
                                                     Rollback
DBMigration → Validate → LockAcquire → Backup → ExecuteForward → Verify → Record → LockRelease
DBBackup   → LockAcquire → ExecuteBackup → Compress → Encrypt → Upload → Verify → LockRelease
```

---

## 10. Scalability, Performance, Optimization

### 10.1 Scalability

#### 10.1.1 Horizontal Scaling
- Database Manager: Stateless service, scales horizontally
- PostgreSQL: Read replicas for read scaling, primary for writes
- Redis: Cluster mode for horizontal scaling
- S3: Infinite scalability (no scaling concerns)

#### 10.1.2 Performance Targets
```
Operation                    Target Latency (p50)    Target Latency (p99)
Simple SELECT (1 row)        1 ms                    5 ms
Multi-row SELECT (100 rows)  3 ms                    20 ms
INSERT (1 row)               2 ms                    10 ms
UPDATE (1 row)               3 ms                    15 ms
DELETE (1 row)               2 ms                    10 ms
Batch INSERT (100 rows)      10 ms                   50 ms
Transaction (3 statements)   10 ms                   100 ms
Connection acquire           1 ms                    5 ms
Schema migration             5 seconds               30 seconds
Backup (100GB database)      30 minutes              60 minutes
```

### 10.2 Optimization Techniques

#### 10.2.1 Connection Pool Optimization
- Pool sizing: `connections = (core_count * 2) + effective_spindle_count`
- Dynamic pool sizing based on request rate
- Connection creation batching
- Keepalive to prevent firewall drops

#### 10.2.2 Query Optimization
- Automatic query parameterization for plan reuse
- Prepared statement caching per connection
- Batch statement execution (reduce round-trips)
- Streaming results for large result sets
- Query timeout enforcement

#### 10.2.3 Read/Write Splitting
- All SELECT queries routed to replicas (unless inside write transaction)
- Read-your-writes consistency: track last write time per service
- Stale read tolerance configurable per query
- Replica selection weighted by current load

#### 10.2.4 Batch Operations
- Batch INSERT: use PostgreSQL COPY protocol for bulk loads
- Batch UPDATE: use unnest + array parameters
- Batch SELECT: use IN clauses (with limits)
- Batch DELETE: use CTE-based batch deletes

---

## 11. Testing Strategy, Risk Analysis

### 11.1 Testing Strategy

#### 11.1.1 Unit Testing
- Connection pool acquisition and release
- Query router logic (read-write splitting, replica selection)
- Query classifier (SELECT vs INSERT/UPDATE/DELETE)
- Migration version ordering and dependency resolution
- Retention policy evaluation logic
- Backup manifest generation and validation
- Permission evaluation

#### 11.1.2 Integration Testing
- Full query lifecycle with PostgreSQL
- Transaction commit and rollback
- Concurrent query execution
- Connection pool exhaustion and recovery
- Migration forward and rollback
- Backup creation and restore
- Data purge with verification
- Archive and restore flow

#### 11.1.3 Performance Testing
- Query latency at various concurrency levels
- Connection pool performance with burst traffic
- Migration execution time at scale
- Backup throughput
- Replica lag under write load

#### 11.1.4 Chaos Testing
- PostgreSQL primary failure
- Read replica failure during read load
- Network partition between Database Manager and PostgreSQL
- Disk full simulation
- Connection pool saturation
- Deadlock injection

### 11.2 Risk Analysis

| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|------------|
| Connection pool exhaustion from leak | Medium | High | Leak detection, force close, auto-scaling |
| Migration causing data loss | Low | Critical | Backup before migration, rollback script, staging test |
| Primary failure during write load | Medium | High | Read replicas, automatic failover, write queue |
| Replication lag causing stale reads | Medium | Medium | Lag monitoring, read-your-writes tracking |
| Query timeout cascading failure | Medium | Medium | Query timeout enforcement, circuit breaker |
| Data corruption from storage failure | Low | Critical | WAL archiving, backup verification, replica diversity |
| Deadlock reducing throughput | Medium | Low | Deadlock detection, retry logic, lock analysis |
| Compliance purge deleting wrong data | Low | Critical | Soft delete first, audit trail, approval workflow |

---

## 12. Failure Scenarios, Recovery Procedures

### 12.1 Failure Scenarios

#### 12.1.1 PostgreSQL Primary Failure
**Scenario**: Primary PostgreSQL node crashes (OOM, hardware failure, kernel panic).

**Symptoms**: All write queries fail, read replicas continue serving.

**Impact**: Write operations unavailable for 30-120 seconds.

**Recovery**:
1. Health Checker detects failure (3 failed probes)
2. Failover Handler triggered:
   a. Identify best candidate replica (lowest lag)
   b. Promote to primary: `pg_ctl promote` or Patroni failover
   c. Update service discovery/DNS
3. Read replicas reconfigured to follow new primary
4. Connection pools updated (mark old primary connections as invalid)
5. Write connections drained to new primary
6. Replica lag caught up
7. Old primary, when recovered, added as replica
8. Return to normal operation

#### 12.1.2 Connection Pool Exhaustion
**Scenario**: Traffic spike saturates all pool connections.

**Symptoms**: New queries wait in queue, latency increases, eventual timeouts.

**Impact**: Service degradation, cascading timeouts.

**Recovery**:
1. Pool exhaustion detected (pending queue > 0)
2. Auto-scale: increase max_connections (if DB can handle)
3. If cannot increase: return SERVER_BUSY, suggest retry
4. Identify source of spike (which service, which query)
5. If connection leak: force-close leaked connections
6. If legitimate spike: trigger service auto-scaling
7. Rate-limit offending service if abusive

#### 12.1.3 Data Corruption
**Scenario**: Storage subsystem corrupts PostgreSQL data file.

**Symptoms**: Query errors, CHECKSUM failures, crash on affected data.

**Impact**: Data loss (limited or widespread depending on corruption).

**Recovery**:
1. Detect corruption via CHECKSUM failure or error
2. Isolate: set affected database/table to READ_ONLY or OFFLINE
3. Restore from latest backup:
   a. Restore database to temporary location
   b. Replay WAL to maximize data recovery
   c. Verify integrity of recovered data
4. Replace corrupted database with restored version
5. Validate post-restore: row counts, integrity constraints, sample queries
6. Resume service

#### 12.1.4 Replication Lag Spike
**Scenario**: A long-running reporting query on a replica causes replication lag.

**Symptoms**: Read queries routed to lagging replica return stale data.

**Impact**: Data freshness concerns for read operations.

**Recovery**:
1. Replica lag exceeds threshold (> threshold seconds)
2. Remove lagging replica from read routing pool
3. Identify cause (long-running query, WAL accumulation)
4. If long-running query: cancel it (if safe)
5. If WAL accumulation: increase WAL receiver/apply parameters
6. Monitor lag catch-up progress
7. When lag resolved: verify, warm cache, return to pool

### 12.2 Recovery Procedures

#### 12.2.1 Full Database Restore
1. Identify target backup (based on point-in-time)
2. Set database to OFFLINE
3. Provision new database instance (if replacing)
4. Restore backup to new instance
5. Configure replication if needed
6. Update Database Manager configuration with new connection details
7. Run integrity validation
8. Warm connection pools
9. Set database to ONLINE
10. Notify affected services of restore completion

#### 12.2.2 Emergency Schema Rollback
1. Identify problematic migration version
2. Prepare rollback script (should exist for production migrations)
3. Set database to READ_ONLY (prevent writes during rollback)
4. Dry-run rollback in staging
5. Apply rollback in production
6. Verify schema version reverted correctly
7. Validate data integrity (rollback may have deleted columns)
8. Set database to READ_WRITE

---

## 13. Operational Limits, Maintenance

### 13.1 Operational Limits

| Parameter | Limit | Action on Exceed |
|-----------|-------|------------------|
| Max connections per PostgreSQL | Configurable (default 500) | Block new connections |
| Max connections per pool | 100 per service instance | Pool exhausted error |
| Max query execution time | 30 seconds | Cancel query |
| Max query result rows | 100,000 | Truncate result |
| Max batch statements | 1,000 | Truncate batch |
| Max prepared statements | 256 per connection | Evict oldest |
| Max databases managed | 100 | Alert, plan consolidation |
| Max concurrent migrations | 1 per database | Queue migrations |
| Max backup duration | 24 hours | Alert, investigate |
| Max replication lag | 60 seconds | Remove from pool |
| Max storage usage | 85% of total | Alert, plan expansion |
| Max WAL accumulation | 100 GB | Alert, investigate replication |

### 13.2 Maintenance Procedures

#### 13.2.1 Routine Maintenance
- Hourly: Replica lag check, slow query review
- Daily: Connection pool health, storage usage review
- Weekly: Index usage analysis, unused index cleanup
- Monthly: Backup restoration test, migration review
- Quarterly: Performance benchmark, capacity planning
- Yearly: Full DR drill, compliance audit

#### 13.2.2 PostgreSQL Vacuuming
- Auto-vacuum configured for all tables
- Manual vacuum for high-update tables during low traffic
- Analyze after bulk operations
- Monitor vacuum progress and table bloat

#### 13.2.3 Index Maintenance
- Review index usage statistics weekly
- Drop unused indexes (> 30 days no usage)
- Rebuild bloated indexes (REINDEX CONCURRENTLY)
- Create indexes based on slow query analysis

#### 13.2.4 Storage Expansion
1. Monitor storage usage trend
2. Project when 85% threshold reached
3. Plan expansion: increase PVC size or add new volume
4. Schedule during maintenance window
5. Expand storage, verify online
6. Verify query performance post-expansion

---

## 14. Future Improvements, Examples

### 14.1 Future Improvements

#### 14.1.1 Planned Enhancements
- Connection pooling for additional database engines (MySQL, MongoDB)
- Distributed query execution across shards
- Automatic index recommendations with impact analysis
- Query performance baseline and regression detection
- Multi-region database replication
- Database cost optimization (right-sizing, reserved instances)
- Self-healing connection pools (automatic recovery)
- Intelligent query routing with ML-based replica selection
- Data masking for non-production environments
- Automated schema diff and synchronization

#### 14.1.2 Research Areas
- Edge database replication for distributed AIOS instances
- Database-agnostic query language (cross-engine query)
- Serverless database scaling (Aurora, Neon)
- Blockchain-based data integrity verification
- AI-driven query optimization

### 14.2 Examples

#### 14.2.1 Multi-Service Transaction
```
Scenario: Agent memory write involves L4 persistent storage across multiple tables.

Request:
{
    "service": "memory-system",
    "transaction": {
        "isolation_level": "READ_COMMITTED",
        "statements": [
            "INSERT INTO agent_memory (agent_id, key, value, tier, ttl) VALUES ($1, $2, $3, 'L4', $4)",
            "INSERT INTO memory_audit (agent_id, operation, key, size_bytes) VALUES ($1, 'WRITE', $2, $5)",
            "UPDATE agent_quota_usage SET current_bytes = current_bytes + $5 WHERE agent_id = $1"
        ],
        "parameters": [
            ["agent-0427", "session/ctx-993", "{\"data\":\"...\"}", "86400s", "524288"],
            ["agent-0427", "WRITE", "session/ctx-993", "524288"],
            ["agent-0427", "524288"]
        ]
    }
}

Flow:
1. Database Manager receives transaction request
2. Connection acquired from primary pool (transaction requires primary)
3. BEGIN with READ_COMMITTED isolation
4. Execute three parameterized statements
5. On success: COMMIT
6. On any failure: ROLLBACK
7. Connection returned to pool
8. Metrics recorded per statement

Outcome:
- All three inserts/update succeed atomically
- Agent memory persisted, audit logged, quota updated
- If any statement fails, no partial state
```

#### 14.2.2 Schema Migration Example
```
Migration V042: Add 'metadata' JSONB column to agent_memory table

Forward Script:
```sql
ALTER TABLE agent_memory ADD COLUMN metadata JSONB DEFAULT '{}'::jsonb;
CREATE INDEX idx_agent_memory_metadata ON agent_memory USING GIN (metadata);
COMMENT ON COLUMN agent_memory.metadata IS 'Flexible metadata for memory blocks';

-- Backfill existing rows with default empty metadata
UPDATE agent_memory SET metadata = '{}'::jsonb WHERE metadata IS NULL;
```

Rollback Script:
```sql
DROP INDEX IF EXISTS idx_agent_memory_metadata;
ALTER TABLE agent_memory DROP COLUMN IF EXISTS metadata;
```

Execution:
1. Migration validated and dry-run in staging
2. Applied in production: 2.5 seconds
3. Index creation: 5 seconds (concurrent, no table lock)
4. Backfill: 30 seconds (100K rows)
5. Status: APPLIED, version 43
```

#### 14.2.3 Backup and Restore
```
Daily Backup Schedule:
- 01:00 UTC: PostgreSQL full backup (pg_dump -Fc)
- 03:00 UTC: WAL archival checkpoint
- Hourly: WAL archive to S3

Restore Scenario: Database corruption at 14:30

Recovery:
1. Stop application writes
2. Restore latest full backup (from 01:00 UTC)
3. Replay WAL archives from 01:00 to 14:30
4. Verify database consistency
5. Point-in-time recovery complete: 0 data loss
6. Resume service

Metrics:
- Full backup: 15 min (100GB database)
- WAL replay: 10 min (13.5 hours of WAL)
- Total recovery time: ~30 minutes
```

---

## 15. Acceptance Criteria, Definition of Done

### 15.1 Acceptance Criteria

#### 15.1.1 Functional Acceptance Criteria
1. Execute SELECT, INSERT, UPDATE, DELETE queries on PostgreSQL
2. Execute batch queries with correct per-statement results
3. Begin, commit, and rollback transactions correctly
4. Read-write splitting: SELECTs go to replicas, writes go to primary
5. Connection pool correctly manages min/max/active/idle connections
6. Schema migration applies forward scripts and records version
7. Schema migration rolls back via rollback script
8. Backup creation and restore work correctly
9. Retention policy purges data older than threshold
10. Data archive exports to S3 and restores correctly

#### 15.1.2 Non-Functional Acceptance Criteria
1. Simple query p99 latency < 5ms
2. Write query p99 latency < 15ms
3. Connection pool acquire p99 < 5ms
4. Connection pool utilization < 70% under normal load
5. Read-write split routes 100% of SELECT statements to replicas
6. Replica lag < 5 seconds at p99
7. Migration failure: zero data loss
8. Backup restore: 100% data integrity
9. Failover: < 120 seconds total downtime
10. No SQL injection vulnerabilities (parameterized queries only)

### 15.2 Definition of Done

A Database Manager feature or component is "Done" when:

1. **Specification**: All sections addressed
2. **Implementation**: All database types, query types, and management features implemented
3. **Unit Tests**: > 90% line coverage
4. **Integration Tests**: Full query lifecycle, transaction, migration, backup, restore tested
5. **Performance Tests**: Latency and throughput meet specified targets
6. **Security Review**: SQL injection prevention, credential management, encryption verified
7. **Documentation**: API reference, migration guide, operations runbook complete
8. **Monitoring**: All metrics, logs, traces implemented
9. **Alerting**: Critical conditions (primary down, pool exhausted, replication lag) alerted
10. **Chaos Tests**: Survive primary failure, replica failure, pool exhaustion, network partition
11. **Code Review**: All changes reviewed and approved
12. **Backward Compatibility**: API changes are backward compatible
13. **Deployment**: Kubernetes manifests, migration scripts, deployment procedures documented
14. **Rollback**: Schema rollback, backup restore, full DR procedures documented
15. **SLA**: Meets defined availability (99.95%) and performance targets

---

*End of AIOS Database Manager Specification Document v1.0*
