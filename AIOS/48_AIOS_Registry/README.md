# AIOS Registry System — Engineering Specification v1.0

## Document Control
- **Document ID:** AIOS-SPEC-REGISTRY-001
- **Version:** 1.0
- **Status:** APPROVED
- **Last Updated:** 2026-07-10
- **Owner:** AIOS Platform Engineering — Registry & Discovery Team
- **Classification:** CONFIDENTIAL

---

## 1. Purpose

The AIOS Registry System is the single source of truth for all entity metadata across the AIOS platform, providing centralized discovery, management, and versioning of agents, models, tools, plugins, services, workflows, and configuration.

## 2. Mission

To provide a highly available, scalable, and consistent registry that serves as the backbone for all AIOS component discovery, metadata management, and cross-component integration, with sub-millisecond query latency and 99.999% uptime.

## 3. Responsibilities

- Registry types: agent registry, model registry, tool registry, plugin registry, service registry, workflow registry, configuration registry.
- Central registry: single source of truth for entity metadata.
- Registry data model: entity type, ID, name, version, status, metadata, tags, dependencies.
- Registry API: CRUD, search, query, watch (subscribe to changes).
- Registry caching: local cache with server-sent invalidation.
- Registry replication: across regions with eventual consistency.
- Registry security: authentication, fine-grained access control.
- Registry versioning: every change tracked and versioned.
- Registry health: integrity checks, consistency verification.
- Registry scalability: sharding for large registries.
- Registry integration: all AIOS components use registry for discovery.
- Registry metrics: query latency, cache hit rate, entity count.

## 4. Non-Responsibilities

- Runtime state management (Memory System)
- Workflow execution (Workflow Engine)
- Agent execution (AIOS Runtime)
- Configuration management (Configuration System)

## 5. Architecture

### 5.1 Registry Types

```
Agent Registry: all active AI agents, their capabilities, status
Model Registry: model versions, metadata, artifacts location
Tool Registry: tool definitions, schemas, versions
Plugin Registry: plugin metadata, versions, dependencies
Service Registry: all AIOS services, endpoints, health
Workflow Registry: workflow definitions, DAGs, versions
Configuration Registry: config schemas, values, versions
```

### 5.2 Architecture Diagram

```
  Registry API (gRPC/REST)
  ┌─────────────────────────────────┐
  │      Registry Service           │
  │  ┌────────┐ ┌────────┐ ┌───┐  │
  │  │ Agent  │ │ Model  │ │...│  │
  │  │ Shard 1│ │ Shard 1│ │   │  │
  │  └────────┘ └────────┘ └───┘  │
  │  ┌────────┐ ┌────────┐         │
  │  │ Agent  │ │ Model  │         │
  │  │ Shard 2│ │ Shard 2│         │
  │  └────────┘ └────────┘         │
  │        Local Cache              │
  └─────────────────────────────────┘
  │
  ├── Database (PostgreSQL/CockroachDB)
  ├── Replication to secondary regions
  └── Event Bus for change notifications
```

## 6. Internal Components

### Registry Service
Stateless gRPC service. Routes reads to local cache or database. Routes writes to primary databae. Publishes change events on write.

### Registry Cache
Local in-memory cache (384MB). Maps entity_type + ID + fields to cached value. TTL: variable (configs: 300s, agents: 30s, services: 10s). Invalidation: on write event from registry.

### Registry Shard Manager
Manages sharding of large registries. Hash-based sharding (murmur3). Supports rebalancing (add/remove shards). Monitor shard skew.

### Registry Replicator
Cross-region async replication. Change Data Capture (CDC) to Kafka -> stream to secondary regions -> apply to local DB. Lag target: < 1s (within region), < 10s (cross region).

### Registry Health Checker
Periodic integrity checks: entity count = expected, no orphaned references, checksum comparisons for critical registries.

## 7. External Components

AIOS Kernel, AIOS Cache, AIOS Event Bus, all AIOS components (consumers); AIOS Kernel, AIOS Workflow, AIOS Agent Runtime (producers).

## 8. Dependencies

CockroachDB 24.1+ or PostgreSQL 16+, Redis (optional cache layer), Kafka 3.7+ for cross-region replication.

## 9. Data Structures

### RegistryEntity
```yaml
RegistryEntity:
  type: "agent | model | tool | plugin | service | workflow | config"
  id: "uuid"
  name: "string"
  version: "semver"
  status: "active | inactive | deprecated | archived"
  metadata: {key: value} (flexible schema)
  tags: ["string"]
  dependencies: [{entity_type, entity_id, version_constraint}]
  owner: "team_name"
  created_at: timestamp
  updated_at: timestamp
  checksum: "string"
```

### RegistrySearchIndex
```sql
CREATE INDEX idx_registry_type ON registry_entities(type);
CREATE INDEX idx_registry_name ON registry_entities(name);
CREATE INDEX idx_registry_status ON registry_entities(status);
CREATE INDEX idx_registry_tags ON registry_entities USING GIN(tags);
CREATE INDEX idx_registry_version ON registry_entities(type, name, version);
```

## 10. Execution Flow

Entity registration: create metadata -> validate -> version increment -> store -> publish event -> cache invalidate -> replicate.

Entity query: query API -> check cache (hit: return) -> query database -> cache result -> return.

Entity watch: client subscribes -> registry watches for changes -> sends delta events.

## 11-44. Additional Sections

**Registry Metrics:**
- Entity count: 10M+ agents, 100K+ models, 50K+ tools
- Query latency: P50 2ms, P95 5ms, P99 15ms
- Write latency: P50 10ms, P95 25ms, P99 50ms
- Cache hit rate: > 95%
- Events per second: 100K+ (peak)

**Scaling:**
- 8-16 shards per registry type
- 3-6 replicas per shard
- 2-5X replication factor for reads
- Add shards: no downtime, background rebalance in 1-2 hours
- Max entities per shard: 2M per shard (balanced)

**Acceptance Criteria:**
Entity registration completes in < 50ms. Query returns in < 15ms P99. Cache hit > 95%. Cross-region replication lag < 10s. Registry surviving shard failure. Search indexing for tags/full-text. All components discoverable via registry.
