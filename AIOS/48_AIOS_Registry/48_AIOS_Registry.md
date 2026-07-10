# AIOS Registry System — Engineering Specification v1.0

## Document Control
- **Document ID:** AIOS-SPEC-REGISTRY-001
- **Version:** 1.0
- **Status:** APPROVED
- **Last Updated:** 2026-07-10
- **Owner:** AIOS Platform Engineering — Registry & Discovery Team
- **Classification:** CONFIDENTIAL

---

## Table of Contents
1. Purpose
2. Mission
3. Responsibilities
4. Non-Responsibilities
5. Architecture Overview
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

The AIOS Registry System is the single source of truth for all entity metadata across the AIOS platform, providing centralized discovery, management, and versioning of agents, models, tools, plugins, services, workflows, and configuration. It serves as the backbone for all component discovery, metadata management, and cross-component integration. Every AIOS component uses the registry to discover other components, register capabilities, and publish metadata.

## 2. Mission

To provide a highly available, scalable, and consistent registry that serves as the backbone for all AIOS component discovery, metadata management, and cross-component integration, with sub-millisecond query latency, 99.999% uptime, and the ability to handle 10M+ entity registrations with cross-region eventual consistency under 10 seconds.

## 3. Responsibilities

- **Registry Types:** Agent registry (all active AI agents, capabilities, status), model registry (model versions, metadata, artifact locations), tool registry (tool definitions, schemas, versions), plugin registry (plugin metadata, versions, dependencies), service registry (all AIOS services, endpoints, health), workflow registry (workflow definitions, DAGs, versions), configuration registry (config schemas, values, versions).
- **Central Registry:** Single source of truth for all entity metadata with strong consistency for writes and tunable consistency for reads.
- **Registry Data Model:** Entity type, ID, name, version, status, metadata (flexible schema), tags, dependencies, ownership.
- **Registry API:** Full CRUD operations, search (exact and fuzzy), query (by type, status, tags), watch (subscribe to changes via streaming).
- **Registry Caching:** Local in-memory cache on each registry service pod with server-sent invalidation on write events.
- **Registry Replication:** Cross-region async replication with Change Data Capture (CDC) to Kafka, with <1s lag within region and <10s cross-region.
- **Registry Security:** Authentication (mTLS, JWT), fine-grained access control (entity-level permissions), audit logging for all mutations.
- **Registry Versioning:** Every change is tracked and versioned with full audit trail. Supports rollback to previous version.
- **Registry Health:** Integrity checks (entity counts, checksum comparisons, orphan reference detection) run periodically.
- **Registry Scalability:** Sharding for large registries (by entity type), read replicas for high query throughput.
- **Registry Integration:** All AIOS components consume/publish to registry for service discovery and metadata management.
- **Registry GC:** Automated garbage collection of expired/stale entities, soft delete with configurable retention.
- **Registry Import/Export:** Bulk import from JSON/YAML, export for backup and migration.

## 4. Non-Responsibilities

- Runtime state management of agents (handled by AIOS Memory System)
- Workflow execution state (handled by Workflow Engine)
- Agent execution (handled by AIOS Runtime)
- Configuration value management (handled by Configuration System)
- Secrets management (handled by Security System)
- Live health checking (handled by HA System, registry stores health metadata)

## 5. Architecture Overview

### 5.1 Registry Types

```
┌────────────────────────────────────────────────────────────────────┐
│                        AIOS REGISTRY                                │
│                                                                     │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐             │
│  │ Agent        │  │ Model        │  │ Tool         │             │
│  │ Registry     │  │ Registry     │  │ Registry     │             │
│  │              │  │              │  │              │             │
│  │ 10M+ agents  │  │ 100K+ models │  │ 50K+ tools   │             │
│  └──────────────┘  └──────────────┘  └──────────────┘             │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐             │
│  │ Plugin       │  │ Service     │  │ Workflow     │             │
│  │ Registry     │  │ Registry     │  │ Registry     │             │
│  │              │  │              │  │              │             │
│  │ 5K+ plugins  │  │ 500+ services │  │ 10K+ workflow │             │
│  └──────────────┘  └──────────────┘  └──────────────┘             │
└────────────────────────────────────────────────────────────────────┘
```

### 5.2 Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────────┐
│                     REGISTRY SERVICE (gRPC/REST)                     │
│                                                                     │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌──────────┐  │
│  │ Agent       │  │ Model       │  │ Tool        │  │ Cache    │  │
│  │ Shard 1     │  │ Shard 1     │  │ Shard 1     │  │ Layer    │  │
│  ├─────────────┤  ├─────────────┤  ├─────────────┤  │ (Redis)  │  │
│  │ Agent       │  │ Model       │  │ Tool        │  │          │  │
│  │ Shard 2     │  │ Shard 2     │  │ Shard 2     │  └──────────┘  │
│  ├─────────────┤  ├─────────────┤  ├─────────────┤               │
│  │ Agent       │  │ Model       │  │ Plugin      │               │
│  │ Shard N     │  │ Shard M     │  │ Shard 1     │               │
│  └─────────────┘  └─────────────┘  └─────────────┘               │
└─────────────────────────┬───────────────────────────────────────────┘
                          │
          ┌───────────────┼───────────────┐
          │               │               │
          v               v               v
┌──────────────┐ ┌──────────────┐ ┌──────────────┐
│ PostgreSQL   │ │ CockroachDB │ │ Kafka (CDC)  │
│ (Primary)   │ │ (Multi-reg)  │ │ (Replication)│
└──────────────┘ └──────────────┘ └──────────────┘
```

### 5.3 Sharding Architecture

- Sharding strategy: hash-based (murmur3) on entity ID modulo shard count
- Each registry type has its own set of shards
- Initial shards: 8-16 per registry type
- Shard rebalancing: background process, no downtime, 1-2 hours to complete
- Max entities per shard: 2 million

## 6. Internal Components

### 6.1 Registry Service (Stateless — gRPC)

**Component ID:** REG-SVC-001
**Language:** Go
**Replicas:** 12-24 (active-active)

**Request Routing:**
- Reads: Check local cache → if miss, route to appropriate shard → cache result → return
- Writes: Route to primary shard → write to DB → publish change event → invalidate cache → return
- Searches: Query index → aggregate results from relevant shards → return

### 6.2 Registry Cache

**Component ID:** REG-CACHE-001
**Language:** Go (local), Redis (shared)
**Memory:** 384MB local, 2GB Redis

**TTL by Entity Type:**
| Type | Local Cache TTL | Redis Cache TTL |
|------|----------------|-----------------|
| Configuration | 300s | 600s |
| Service endpoints | 10s | 30s |
| Agents | 30s | 60s |
| Models | 60s | 120s |
| Tools | 60s | 120s |
| Workflows | 120s | 300s |

**Invalidation:** Server-sent invalidation on write event from registry:
- Write occurs → change event published → all service instances receive invalidation → local cache entry removed

### 6.3 Registry Shard Manager

**Component ID:** REG-SHARD-001
**Language:** Go
**Replicas:** 3

**Shard Management:**
- Hash ring: consistent hashing with virtual nodes (256 virtual nodes per shard)
- Shard key: hash(entity_type + entity_id) % total_virtual_nodes
- Rebalance: when adding/removing shards, move only (1/total_shards) of data

**Shard Health:**
- Shard leader failure: promote replica to leader
- Shard congestion: read-only mode, increase replica count
- Shard skew detection: re-allocate hash range

### 6.4 Registry Replicator

**Component ID:** REG-REPL-001
**Language:** Rust
**Replicas:** 3-5

**Cross-Region Replication:**
```
Primary Region → CDC (Debezium) → Kafka MirrorMaker → Target Region → Apply to CockroachDB

Lag Targets: Within-region: < 1s, Cross-region: < 10s
```

**Replication Conflicts:**
- Last-writer-wins with timestamp comparison
- Conflict log stored for audit
- Manual resolution possible for rare cases

### 6.5 Registry Health Checker

**Component ID:** REG-HC-001
**Language:** Go + SQL
**Replicas:** 2

**Integrity Checks:**
1. Entity Count Integrity:
   - SELECT COUNT(*) FROM registry_entities vs aggregated cache count
   - Tolerance: < 0.1% deviation

2. Checksum Integrity:
   - Compute table-level checksum using pg_checksumtable
   - Compare to previous known checksum
   - Run every hour for critical registries

3. Orphaned Reference Detection:
   - Check all dependency references exist
   - Check all foreign keys exist
   - Report orphans for cleanup

4. Index Integrity:
   - REINDEX if fragmentation > 30%
   - Monitor index size growth

### 6.6 Registry Metadata Indexer

**Component ID:** REG-INDEX-001
**Language:** Go + Elasticsearch
**Replicas:** 3-5

**Search Indexing:**
- Type: Elasticsearch (for full-text search)
- Index per entity type
- Fields indexed: name, description, tags, metadata (text fields)
- Sync: Change Data Capture from PostgreSQL → Elasticsearch
- Index refresh: 1s interval for near-real-time search

## 7. External Components

| Component | Type | Integration |
|-----------|------|-------------|
| AIOS Kernel | Consumer | Registry for service discovery |
| AIOS Runtime | Producer/Consumer | Agent registration, discovery |
| AIOS Workflow | Producer | Workflow definition registration |
| AIOS Agent Manager | Producer/Consumer | Agent instance registration |
| AIOS Model Manager | Producer | Model version registration |
| AIOS Tool Manager | Producer | Tool definition registration |
| AIOS Plugin System | Producer | Plugin registration |
| AIOS Configuration System | Producer/Consumer | Config schema registration |
| AIOS Security System | Producer/Consumer | Permission-based access control |
| AIOS Monitoring System | Consumer | Registry events for metrics |
| AIOS Backup System | Consumer | Registry backup data |
| CockroachDB | Storage | Primary database |
| PostgreSQL | Storage | Secondary/legacy database |
| Kafka | Event Bus | CDC, events for replication |
| Elasticsearch | Search | Full-text search indexing |
| Redis | Cache | Distributed cache layer |

## 8. Dependencies

**Build:** Go 1.22+, Rust 1.80+, Protocol Buffers 3.x, CockroachDB driver 24.1+, Kafka Client 3.7+, Elasticsearch Client 8.11+, Redis Client 7.2+

**Runtime:** CockroachDB 24.1+ (or PostgreSQL 16+), Redis 7.2+ (optional cache layer), Kafka 3.7+, Elasticsearch 8.12+, Debezium 2.5+ (CDC)

## 9. Inputs

**Create Requests:**
- Entity type (agent, model, tool, plugin, service, workflow, config)
- Entity metadata (name, description, version, tags, properties)
- Dependencies and references
- Permissions and ownership
- Request authentication/authorization context

**Update Requests:**
- Entity ID and fields to update (partial updates supported)
- New version information (if version increment required)

**Read Requests:**
- Entity ID lookup (primary key query)
- Filtered queries (by type, status, tags, owner)
- Search queries (full-text, fuzzy)
- Watch requests (subscribe to changes on entity or entity type)

**System Inputs:**
- Health check pings from agents/services (heartbeat, status update)
- GC trigger events (periodic, on-demand)
- Rebalance events (shard add/remove)
- Replication stream events (cross-region)

## 10. Outputs

**Read Results:**
- Entity metadata (full or partial, based on requested fields)
- Entity version history
- Search results (pageinated, sorted)
- Change events (for watch subscribers)

**Events:**
| Event | Description | Payload |
|-------|-----------|---------|
| registry.entity.created | New entity registered | {type, id, version, name} |
| registry.entity.updated | Entity metadata changed | {type, id, version, changed_fields} |
| registry.entity.deleted | Entity soft/hard deleted | {type, id, reason} |
| registry.entity.status.changed | Entity status change | {type, id, old_status, new_status} |
| registry.entity.heartbeat | Health heartbeat update | {type, id, status, timestamp} |
| registry.shard.rebalanced | Shard redistribution | {type, old_count, new_count} |
| registry.health.check | Integrity check result | {status, issues, duration} |

**Metrics:**
- Query latency: P50 2ms, P95 5ms, P99 15ms
- Write latency: P50 10ms, P95 25ms, P99 50ms
- Cache hit rate: > 95%
- Events per second: 100K+ (peak)

## 11. Data Structures

### 11.1 RegistryEntity (Persistent — CockroachDB/PostgreSQL)

```yaml
RegistryEntity:
  type: "agent | model | tool | plugin | service | workflow | config"
  id: "uuid v7 (time-ordered)"
  name: "string"
  display_name: "string" (optional)
  description: "text" (optional)
  version: "1.2.3" (semver)
  status: "active | inactive | deprecated | archived | deleted"
  type_data:
    # Flexible schema per type
    agent: { capabilities: [...], runtime: "python3.12", memory_limit: 512MB, ... }
    model: { base_model: "gpt-4", parameters: "70B", quantization: "int8", ... }
    tool: { schema: {...}, runtime: "js", version: "1.0", ... }
    service: { endpoint: "grpc://kernel.aios.internal:8080", protocol: "grpc" }
    workflow: { dag: {...}, steps: 5, triggers: [...], ... }
    config: { schema: "json_schema", default_values: {...} }
  tags:
    - "production"
    - "critical"
    - "team-platform"
  dependencies:
    - entity_type: "service"
      entity_id: "uuid-123"
      version_constraint: ">=2.0.0"
    - entity_type: "model"
      entity_id: "uuid-456"
      version_constraint: "3.x"
  owner:
    team: "platform-engineering"
    contact: "platform@aios.io"
  permissions:
    read_roles: ["viewer", "operator", "admin"]
    write_roles: ["operator", "admin"]
    admin_roles: ["admin"]
  metadata:
    # Arbitrary key-value pairs (up to 1MB total)
    key1: "value1"
    key2: "value2"
  created_at: "2026-01-01T00:00:00Z"
  updated_at: "2026-07-10T14:00:00Z"
  version_created_at: "2026-07-10T14:00:00Z"
  version_updated_by: "system:agent-manager"
  heartbeat:  # For services/agents
    last_heartbeat: "2026-07-10T14:30:00Z"
    heartbeat_interval: 30
    status: "online"
```

### 11.2 RegistryVersion (Persistent — Audit Table)

```yaml
RegistryVersion:
  version_id: "uuid"
  entity_type: "agent"
  entity_id: "uuid-original"
  version_number: 42
  changed_at: "2026-07-10T14:00:00Z"
  changed_by: "auth0|operator-123"
  change_type: "UPDATE | CREATE | DELETE"
  changed_fields: ["name", "status", "tags"]
  previous_version_summary: { name: "agent-old-name", status: "active" }
  current_value: { name: "agent-new-name", status: "active", tags: [...], ... }
  diff: { name: { from: "old", to: "new" }, status: { from: "inactive", to: "active" } }
```

### 11.3 RegistrySearchIndex (Elasticsearch)

```sql
-- Index created in Elasticsearch, matching fields:
-- name (text, English analyzer)
-- description (text, English analyzer)
-- tags (keyword)
-- type (keyword)
-- status (keyword)
-- owner.team (keyword)
-- metadata.* (flattened text for generic search)

-- Equivalent SQL indices for on-database search backups:
CREATE INDEX idx_registry_type ON registry_entities(type);
CREATE INDEX idx_registry_name ON registry_entities(name);
CREATE INDEX idx_registry_status ON registry_entities(status);
CREATE INDEX idx_registry_tags ON registry_entities USING GIN(tags);
CREATE INDEX idx_registry_version ON registry_entities(type, name, version);
CREATE INDEX idx_registry_owner ON registry_entities((metadata->>'owner'));
CREATE INDEX idx_registry_updated ON registry_entities(updated_at DESC);
```

### 11.4 RegistryWatch (In-Memory — per subscription)

```rust
struct RegistryWatch {
    watch_id: Uuid,
    subscriber_id: String,
    watch_filter: WatchFilter,
    // { entity_type: "agent", entity_id: Option<String>, status_filter: ["active"] }
    create_cursor: String,
    // Stream position for starting events
    subscriber_endpoint: String,
    // Where to deliver events (gRPC stream ID)
    event_types: Vec<String>,
    // created, updated, deleted, status_change
    created_at: DateTime<Utc>,
    last_event_at: DateTime<Utc>,
    event_count: u64,
    active: bool,
}
```

## 12. Execution Flow

### 12.1 Entity Registration

```
1. Request: POST /v1/entities { type: "agent", name: "payment-analyzer", ... }
2. Authentication: Validate mTLS/JWT
3. Authorization: Check caller has write permission for agent registry
4. Validation:
   - Name uniqueness: query database for existing agent with same name
   - Schema validation: validate metadata against type schema
   - Dependency validation: all referenced entities exist
   - Version format: validate semver
5. Create entity:
   - Generate UUID (version 4, time-ordered)
   - Set version to 0.1.0 (initial)
   - Set status to "active"
6. Store in database:
   - INSERT into registry_entities
   - INSERT into registry_versions (version 1)
7. Index:
   - Update Elasticsearch index (async, via CDC)
8. Cache invalidation:
   - Publish change event to registry.events (Kafka)
   - All registry service instances receive invalidation
   - Local cache removed for entity
9. Response:
   - Return created entity with ID and version
10. Event: Publish registry.entity.created event
```

### 12.2 Entity Query

```
1. Request: GET /v1/entities/{type}/{id}
2. Authorization: Check caller has read permission for entity type
3. Cache lookup:
   - Generate cache key: registry:{type}:{id}
   - Check local cache (TTL based on entity type)
   - If hit: return cached entity (with staleness header)
4. If cache miss:
   - Route to shard: hash(type + id) % total_shards
   - Query shard leader: SELECT * FROM registry_entities WHERE type = ? AND id = ?
5. Cache result:
   - Update local cache with TTL based on type
   - Future reads from cache
6. Response:
   - Return entity with cache headers (age, max-age)
```

### 12.3 Entity Watch (Subscription)

```
1. Request: Client opens gRPC stream WatchEntities(filter)
2. Validate filters: entity_type required, others optional
3. Create watch: create RegistryWatch record, set cursor to current event
4. Stream loop:
   a. Consume events from registry.events topic (Kafka)
   b. Filter events against watch filter (type, entity_id, event_type)
   c. If match found: send event over gRPC stream
   d. If no events for 30s: send heartbeat to keep stream alive
5. Stream termination:
   a. Client closes stream
   b. Remove RegistryWatch
   c. Log: watcher disconnected
6. Error handling:
   a. If stream broken: client must reconnect
   b. On reconnect: send events from last cursor (acknowledged event)
```

### 12.4 Heartbeat Update (Service/Agent Health)

```
1. Service sends heartbeat: { entity_id, status, timestamp }
2. Registry receives heartbeat:
   a. Look up entity in database
   b. If not found: return error (entity not registered)
3. Update heartbeat timestamp:
   UPDATE registry_entities
   SET metadata = jsonb_set(metadata, '{last_heartbeat}', to_jsonb(now()))
   WHERE id = ? AND type = 'service' OR type = 'agent'
4. Check heartbeat freshness:
   - If last heartbeat > interval * 3: entity marked as offline
   - Publish registry.entity.status.changed event
5. Response: OK
6. Periodic cleanup: heartbeats older than 5 minutes are considered stale
```

## 13. State Management

### Entity State Machine

```
PENDING (initial creation, not yet active)
  │
  ▼
ACTIVE (registered and available)
  │
  ├──► INACTIVE (manually deactivated, not discoverable)
  ├──► DEPRECATED (still available but marked for removal)
  └──► ARCHIVED (historical record only, not discoverable)
        │
        ▼
      DELETED (soft-delete, recoverable within retention period)
        │
        ▼
      PURGED (hard-delete after retention period)
```

### Shard State Machine

```
INITIALIZING → ACTIVE → REBALANCING → ACTIVE
  │              │          │
  ▼              ▼          ▼
FAILED        READ_ONLY    FAILED
  │                         │
  ▼                         ▼
RECOVERING               RECOVERING
```

## 14. Communication Protocols

**Primary — gRPC:**
```protobuf
service RegistryService {
    // Entity CRUD
    rpc CreateEntity(CreateEntityRequest) returns (Entity);
    rpc GetEntity(GetEntityRequest) returns (Entity);
    rpc UpdateEntity(UpdateEntityRequest) returns (Entity);
    rpc DeleteEntity(DeleteEntityRequest) returns (DeleteResponse);
    
    // Batch Operations
    rpc BatchGet(BatchGetRequest) returns (BatchGetResponse);
    rpc BatchCreate(BatchCreateRequest) returns (BatchCreateResponse);
    
    // Search
    rpc SearchEntities(SearchRequest) returns (SearchResponse);
    
    // Watch
    rpc WatchEntities(WatchRequest) returns (stream RegistryEvent);
    
    // Version Management
    rpc ListVersions(ListVersionsRequest) returns (ListVersionsResponse);
    rpc GetVersion(GetVersionRequest) returns (RegistryVersion);
    rpc RollbackToVersion(RollbackRequest) returns (Entity);
    
    // Heartbeat
    rpc Heartbeat(HeartbeatRequest) returns (HeartbeatResponse);
    
    // Admin
    rpc HealthCheck(HealthCheckRequest) returns (HealthCheckResponse);
    rpc GetStats(GetStatsRequest) returns (RegistryStats);
    rpc RebalanceShards(RebalanceRequest) returns (RebalanceResponse);
}
```

**Secondary — REST API:**
```
POST   /v1/entities              — Create entity
GET    /v1/entities/:type/:id     — Get entity
PUT    /v1/entities/:type/:id    — Update entity
DELETE /v1/entities/:type/:id    — Soft-delete entity
POST   /v1/entities/search       — Search entities (POST body has query)
GET    /v1/entities/:type       — List by type
GET    /v1/entities/:type/:id/versions — List versions
POST   /v1/entities/:type/:id/rollback/:version — Rollback to version
POST   /v1/entities/heartbeat   — Send heartbeat
GET    /v1/health               — Health check
GET    /v1/stats                — Registry statistics
```

## 15. APIs

### gRPC Message Details

```protobuf
message CreateEntityRequest {
    string type = 1;        // agent, model, tool, etc.
    string name = 2;
    string display_name = 3;
    string description = 4;
    string version = 5;     // initial version (default 0.1.0)
    string status = 6;      // default: active
    google.protobuf.Struct metadata = 7;
    repeated string tags = 8;
    repeated Dependency dependencies = 9;
    string owner_team = 10;
    string owner_contact = 11;
    RegistryPermissions permissions = 12;
}

message GetEntityRequest {
    string type = 1;
    string id = 2;
    repeated string fields = 3;  // which fields to return (empty = all)
}

message SearchRequest {
    string query = 1;
    string type = 2;  // optional: filter by type
    repeated string status = 3;  // filter by status
    repeated string tags = 4;  // filter by tag
    string sort_by = 5;
    string sort_order = 6;  // ASC, DESC
    uint32 page_size = 7 [default = 20];
    uint32 page = 8 [default = 1];
}
```

## 16. Events

| Event | Partition Key | Schema |
|-------|-------------|--------|
| registry.entity.created | entity_type | {type, id, version, name, changed_by} |
| registry.entity.updated | entity_id | {type, id, version, changed_fields, changed_by} |
| registry.entity.deleted | entity_id | {type, id, reason, changed_by} |
| registry.entity.status.changed | entity_id | {type, id, old_status, new_status} |
| registry.entity.heartbeat | entity_id | {type, id, status, timestamp} |
| registry.shard.rebalanced | registry_type | {type, old_shard_count, new_shard_count} |
| registry.cache.invalidate | entity_type | {type, entity_id} (broadcast to all instances) |

**Kafka Topics:**
- registry.events (32 partitions, 30d retention): All entity life cycle events
- registry.replication (16 partitions, 7d retention): Cross-region CDC stream
- registry.cache.invalidation (8 partitions, 24h retention): Cache invalidation commands

## 17. Queues

**Kafka Configuration:**
```yaml
topics:
  registry.events:
    partitions: 32
    replication_factor: 3
    retention.ms: 2592000000  # 30 days
    cleanup.policy: [compact, delete]
  registry.replication:
    partitions: 16
    replication_factor: 3
    retention.ms: 604800000  # 7 days
    cleanup.policy: [delete]
  registry.cache.invalidation:
    partitions: 8
    replication_factor: 3
    retention.ms: 86400000  # 1 day
    cleanup.policy: [delete]
```

## 18. Caching

**Local Cache (per service pod):**
- Storage: In-memory Go map
- Size: 384MB (configurable)
- TTL: variable per type (10-300s)
- Invalidation: server-sent event on write from any instance
- Key format: registry:{type}:{id}
- Value: serialized entity JSON

**Redis Cache (shared):**
- Storage: Redis Cluster (3 nodes, 6 shards)
- Size: 2GB
- TTL: 60-600s (configured per type)
- Key format: registry_cache:{type}:{id}
- Access pattern: read-through, write-through on entity update
- Eviction: allkeys-lru when memory full

**Search Cache:**
- Elasticsearch query cache
- Size: 1GB
- TTL: 30s for searches, 60s for aggregations

## 19. Memory Budget

| Component | Memory | Breakdown |
|-----------|--------|-----------|
| Registry Service | 2GB per pod | Cache 384MB, Request buffers 256MB, Connection state 512MB, Other 512MB |
| Registry Shard Manager | 512MB | Shard map 128MB, Health 128MB, State 256MB |
| Registry Replicator | 512MB | Replication state 256MB, Buffers 256MB |
| Registry Health Checker | 256MB | Check results 128MB, History 128MB |
| Registry Metadata Indexer | 1GB | Index buffers 512MB, Queue 256MB, State 128MB |
| Redis Cache | 2GB | Entity cache 1.5GB, Metadata 256MB, Overhead 256MB |

## 20. Persistence

**CockroachDB (Primary):**
- Tables: registry_entities, registry_versions, registry_watches
- Capacity: 100GB initial, 500GB projected (2 years)
- Replication: 3 nodes, synchronous commit
- Backup: Integrated with AIOS Backup System

**PostgreSQL (Secondary — Read-only replicas):**
- Same schema as CockroachDB
- Used for read-heavy workloads (query offload)
- Replication: streaming (sub-1s lag)

**Elasticsearch:**
- Indexes: one per entity type
- Search capacity: 10 million documents
- Retention: 365 days (then archived to S3)

**S3 (Cold Storage):**
- Registry snapshots for backup: daily full, incremental hourly
- Data format: JSONL (one entity per line)
- Retention: 7 years (compliance)

## 21. Validation

**Create Validation:**
- Entity type must be one of the defined types
- Name must be unique within type
- Name must match pattern: ^[a-zA-Z0-9_-]{3,128}$
- Version must be valid semver
- Metadata size must be < 1MB
- Tags must be valid (^[a-z0-9_-]{1,64}$)
- Dependencies must reference existing entities
- Owner team must be registered team

**Update Validation:**
- Entity must exist
- Must be in ACTIVE or INACTIVE status (not DELETED/PURGED)
- Cannot change entity type

**Search Validation:**
- Query must be >= 3 characters
- Page size max 100
- Filter combinations must be consistent (e.g., type required for tags without type)

## 22. Retry Logic

| Operation | Max Retries | Backoff | Notes |
|-----------|-------------|---------|-------|
| Write to DB | 3 | Exponential (10ms, 100ms) | Only for transient errors |
| Write to Kafka | 3 | Exponential (10ms, 50ms) | Async, retryable |
| Replication read | 2 | Fixed (100ms) | Fallback to secondary |
| Cache update | 2 | Fixed (10ms) | Non-critical |
| Search index update | 3 | Exponential (10ms, 100ms) | Near-real-time |

## 23. Error Recovery

**Database Failure:**
- Primary DB down: switch to read-only mode, use secondary replicas
- Writes queued for replay on DB recovery
- After recovery: replay queued writes, reconcile with CDC

**Cache Failure:**
- Redis down: all requests go to database
- Local cache still operational
- On recovery: cache warmed from database queries

**Replication Failure:**
- Kafka broker down: CDC logs continue locally, replay when broker recovers
- Long outage: manual snapshot transfer between regions

**Shard Failure:**
- Shard leader down: promote replica (automatic)
- Both leader and replica: shard goes read-only, alert operators

## 24. Security

**Communication Security:**
- All gRPC: mTLS with SPIFFE/SPIRE
- REST API: TLS 1.3
- Internal DB: TLS 1.3 with client certificates
- Kafka: TLS 1.3 with SASL/SCRAM

**Data Security:**
- Registry data at rest: encrypted via CockroachDB/PostgreSQL encryption
- Audit log: append-only, encrypted

**Access Control:**
- Entity-level permissions (read_roles, write_roles, admin_roles)
- Default deny: no entity accessible without explicit permission
- ACL caching: evaluate once per session

## 25. Authentication

**Internal:** mTLS with SPIFFE/SPIRE (spiffe://aios.internal/registry/{component})
**External:** OAuth 2.0 + OIDC (operators), API Key (automation, format: aiosrk_xxxxxxxx...)

## 26. Authorization

| Role | READ | WRITE | ADMIN |
|------|------|-------|-------|
| registry.admin | All entities | All entities (including system) | All operations |
| registry.operator | All entities | Own entities + assigned teams | Modify existing entities |
| registry.viewer | Assigned entities | None | None |
| system.component | Own type entities | Own type entities (create only) | None |

## 27. Permissions

```
registry:entity:{create,read,update,delete}(type: [agent, model, tool, ...])
registry:entity:search
registry:entity:watch
registry:version:{list,read,rollback}
registry:admin:{rebalance,check,audit}
```

## 28. Monitoring

**SLIs:**
| SLI | Target |
|-----|--------|
| Query latency (cache hit) | P50 < 1ms, P99 < 5ms |
| Query latency (cache miss) | P50 < 5ms, P99 < 20ms |
| Write latency | P50 < 10ms, P99 < 50ms |
| Search latency | P50 < 20ms, P99 < 100ms |
| Cache hit rate | > 95% |
| Entity count | 10M+ (current), 50M+ (projected) |
| Events per second | 100K+ (peak) |

**Dashboards:**
1. Registry Health: QPS, latency, error rate, cache hit rate
2. Entity Distribution: count by type, status, growth trend
3. Cache Performance: hit rate by type, size, eviction rate
4. Replication Lag: cross-region replication delay
5. Shard Distribution: entity count per shard, skew detection

## 29. Logging

JSON: {timestamp, level, component, request_type, entity_type, entity_id, duration, status, caller}
ERROR: Write failure, DB connection failure, replication failure
WARN: Cache miss rate high, slow query, shard skew
INFO: Entity created/updated/deleted, shard rebalance, watch created
DEBUG: All requests, cache operations, DB operations

## 30. Metrics

Counter: registry_queries_total{type, result}, registry_writes_total{type, status}, registry_events_total{event_type}
Histogram: registry_query_latency_seconds{type, cache_hit}, registry_write_latency_seconds{type}
Gauge: registry_entity_count{entity_type, status}, registry_cache_hit_ratio{layer}, registry_replication_lag_seconds{region}, registry_shard_count{type}, registry_shard_skew_ratio{type}

## 31. Tracing

OpenTelemetry with 100% sampling for writes, 10% for reads.
Spans: entity.create, entity.get, entity.update, entity.search, entity.watch

## 32. Scalability

| Dimension | Current | Year 2 | Year 3 |
|-----------|---------|--------|--------|
| Entities | 10M | 50M | 500M |
| Writes/sec | 1000 | 5000 | 50000 |
| Reads/sec | 50000 | 250000 | 2.5M |
| Events/sec | 100K | 500K | 5M |
| Registry Services | 12-24 | 50-100 | 200-500 |
| Shards per type | 8-16 | 64-128 | 256-512 |

## 33. Performance

| Operation | P50 | P95 | P99 |
|-----------|-----|-----|-----|
| Get Entity (cache hit) | 0.5ms | 2ms | 5ms |
| Get Entity (cache miss) | 3ms | 10ms | 20ms |
| Create Entity | 8ms | 20ms | 45ms |
| Update Entity | 10ms | 25ms | 50ms |
| Search (simple query) | 5ms | 15ms | 30ms |
| Search (complex query) | 20ms | 50ms | 100ms |
| Watch event delivery | 2ms | 5ms | 10ms |
| Heartbeat update | 2ms | 5ms | 15ms |

## 34. Optimization

**Read Path Optimization:**
- Cache-aside pattern: always read from cache, update on miss
- Batch reads: multiple entity reads in single DB query
- Result projection: only fetch requested fields
- Connection pooling: reuse gRPC connections

**Write Path Optimization:**
- Partial updates: only send changed fields (minimize data transfer)
- Async indexing: write to DB synchronously, index asynchronously via CDC
- Debezium for reliable CDC: low latency, exactly-once semantics

**Cache Optimization:**
- Adaptive TTL: increase TTL for stable entities, decrease for frequently changing entities
- Write-through: invalidate cache immediately on write
- Batched invalidation: batch within 100ms window to reduce Kafka overhead

## 35. Testing

**Unit:** 95%+ coverage, all state machines, validation, permission evaluation
**Integration:** Full CRUD cycle, cache invalidation, search indexing
**E2E:** Entity lifecycle (create → search → update → watch → delete), cross-region replication, shard rebalancing
**Chaos:** DB failure, Kafka failure, shard failure, cache failure, network partition
**Performance:** 10M+ entities, 10000 QPS, 100K events/sec, 5M entity search

## 36. Risk Analysis

| Risk | Probability | Impact | Mitigation |
|------|-----------|--------|-----------|
| DB corruption | Low | Critical | Regular backup, checksum integrity, hot standby |
| Shard hot spot | Low | High | Consistent hashing, rebalancing, shard split |
| Cache stampede | Low | High | Single-flight pattern, request coalescing |
| Replication lag too high | Medium | Medium | Lag monitoring, alerting, CDC tuning |
| Entity name collision | Low | Medium | Unique constraint, validation |

## 37. Failure Scenarios

**Database Failure:**
- Impact: Registry writes blocked, reads fall back to cache
- Recovery: Failover to replica, once primary restored → sync
- Max downtime: 60s for reads, database recovery time for writes

**Cache Stampede:**
- Impact: When cache entry expires, all concurrent requests hit DB
- Recovery: Single-flying (only one request hits DB, rest wait)
- Prevention: cache TTL jitter (+ 10%) to prevent synchronized expiry

**Shard Hot Spot:**
- Impact: One shard overloaded (skewed entity distribution)
- Recovery: Rebalance shard (split hash range for hot shard)
- Detection: Shard skew ratio > 2.0 for > 5 min

**Replication Lag Crisis:**
- Impact: Cross-region reads returning stale data
- Recovery: Route reads to primary region until lag normalizes
- Prevention: Lag alerting at 5s, 10s, 30s thresholds

## 38. Recovery Procedures

**Full Registry Recovery from Backup:**
1. Restore CockroachDB from latest backup
2. Rebuild Elasticsearch indexes from DB dump
3. Rewarm Redis cache
4. Re-establish CDC streams for replication
5. Verify entity counts match backup manifest
6. Run integrity checks (checksum, orphan detection)
7. Enable writes after verification

**Shard Recovery from Failure:**
1. Promote read replica to leader
2. Rebalance hash range distribution
3. Verify query latency returns to normal
4. Remove failed shard from registry

## 39. Operational Limits

- Max entity size: 1 MB (metadata + tags + dependencies)
- Max entities per type: Unlimited (10M+ current, 500M+ capacity)
- Max shards per type: 512
- Max entities per shard: 2 million (balanced)
- Max concurrent watch connections: 100000
- Max watch filters per connection: 10
- Query timeout: 5s (gRPC timeout)
- Write timeout: 15s (gRPC timeout)
- Max batch size: 100 entities
- TTL range: 10s (service) to 600s (config)

## 40. Maintenance

**Daily:** Monitor query latency, write latency, cache hit rate, event rate
**Weekly:** Review shard distribution for skew, validate replication lag, check index fragmentation
**Monthly:** Integrity check all registries, review obsolete entities, update ACLs
**Quarterly:** Full DR test (restore from backup), shard rebalance dry run, capacity planning review

## 41. Future Improvements

- Multi-master writes (CRDT-based conflict resolution)
- Registry graph traversal API (find paths between entities)
- ML-based entity metadata quality scoring
- Automatic entity deprecation based on usage metrics
- Cross-organizational registry federation

## 42. Examples

**Agent Registration:**
POST /v1/entities { type: "agent", name: "payment-validator", version: "1.0.0", metadata: { runtime: "python3.12", capabilities: ["payment.validation"] }, tags: ["production", "critical"] }
Response: { id: "550e8400-e29b-41d4-a716-446655440000", ... }

**Service Discovery:**
GET /v1/services/aios-kernel
Response: { type: "service", id: "...", name: "aios-kernel", metadata: { endpoint: "grpc://kernel.aios.internal:8080", status: "active" } }

**Entity Watch:**
WatchRequest { filter: { entity_type: "agent", status: ["active"] } }
Stream receives: registry.entity.created, registry.entity.updated, registry.entity.status.changed for all agents

## 43. Acceptance Criteria

1. Entity registration returns within 50ms (P99)
2. Entity query returns within 5ms (P99, cache hit) or 20ms (cache miss)
3. Cache hit rate > 95%
4. Cross-region replication lag < 10s
5. Registry survives single shard failure without data loss
6. Search returns relevant results within 100ms (P99)
7. Watch streams deliver events within 10ms of write commit
8. Entity-level permissions enforced correctly
9. Full registry backup and restore completes within recovery SLA
10. Registry handles 10M+ entities with 100K+ events/second

## 44. Definition of Done

1. All ACs satisfied and verified
2. API contracts defined and published
3. Caching layer optimized (> 95% hit rate)
4. Sharding configured for all entity types
5. Cross-region replication operational
6. Search indexing operational
7. Watch subscriptions implemented
8. Monitoring dashboards deployed for all SLIs
9. Alert rules configured for key metrics
10. Integrity checks enabled and running
11. Security review completed no critical findings
12. Runbook published for registry operations