# DATABASE ARCHITECTURE

## Islamic Education Platform (IEP)

---

| Document ID | IEP-ARCH-DB-001 |
|---|---|
| Version | 1.0.0 |
| Status | Approved |
| Classification | Enterprise Internal |
| Effective Date | 2026-07-07 |
| Approving Authority | Office of the Chief Architect |
| Custodian | Data Architecture Team |
| System Architecture Reference | IEP-ARCH-001 (Section 8) |

---

## TABLE OF CONTENTS

1. EXECUTIVE OVERVIEW
2. DATABASE PHILOSOPHY
3. DATABASE LANDSCAPE
4. POSTGRESQL ARCHITECTURE
5. APACHE AGE GRAPH EXTENSION
6. PGVECTOR EXTENSION
7. TIMESCALEDB EXTENSION
8. REDIS ARCHITECTURE
9. APACHE KAFKA ARCHITECTURE
10. ELASTICSEARCH ARCHITECTURE
11. MINIO OBJECT STORE
12. DATABASE SCHEMA CATALOG
13. DATA MIGRATION STRATEGY
14. BACKUP AND RECOVERY
15. REPLICATION STRATEGY
16. SHARDING STRATEGY
17. CONNECTION MANAGEMENT
18. QUERY OPTIMIZATION
19. DATABASE SECURITY
20. DATABASE MONITORING
21. DATABASE OPERATIONS
22. APPENDICES

---

## 1. EXECUTIVE OVERVIEW

### 1.1 Purpose

This document defines the complete database architecture for the Islamic Education Platform. The IEP employs a **polyglot persistence** approach, using multiple specialized database technologies to handle different data types and access patterns efficiently. The architecture centers on **PostgreSQL** as the primary operational database, extended with **Apache AGE** for graph capabilities and **pgvector** for vector embeddings.

### 1.2 Database Inventory

| Database | Version | Purpose | Data Volume (Year 5) |
|---|---|---|---|
| PostgreSQL 16 | 16.3 | Primary operational database, knowledge graph, vector store | 25 TB |
| Apache AGE | Latest (PG extension) | Graph query capabilities on PostgreSQL | (in PostgreSQL) |
| pgvector | 0.7+ | Vector embeddings storage and similarity search | (in PostgreSQL) |
| TimescaleDB | 2.x | Time-series metrics and analytics | 500 TB |
| Redis 7 | 7.2 | Caching, sessions, rate limiting, real-time state | 1 TB |
| Apache Kafka | 3.x | Event streaming, message bus | 100 TB (7 days) |
| Elasticsearch | 8.x | Full-text search, analytics | 25 TB |
| MinIO | Latest | Object storage for files, media, backups | 10 PB |

### 1.3 Data Distribution Principle

Data is distributed based on access pattern:

| Pattern | Database | Rationale |
|---|---|---|
| Transactional (ACID) | PostgreSQL | Strong consistency for critical data |
| Graph (relationships) | PostgreSQL + AGE | Graph queries on ACID-compliant store |
| Vector (embeddings) | PostgreSQL + pgvector | Single engine for vectors + metadata |
| Time-series | TimescaleDB | Optimized for time-range queries |
| Cache | Redis | Sub-millisecond access |
| Events | Kafka | Durable, replayable event stream |
| Full-text search | Elasticsearch | Advanced text search, scoring |
| Objects (files) | MinIO | Scalable, S3-compatible object store |

---

## 2. DATABASE PHILOSOPHY

### 2.1 Design Principles

| Principle | Description |
|---|---|
| **PostgreSQL First** | PostgreSQL is the default database choice. Only use a different database when PostgreSQL cannot meet the specific requirement. |
| **Polyglot Persistence** | Use the right database for each data type and access pattern. |
| **Data Ownership** | Each domain owns its data exclusively. Cross-domain data access goes through APIs, not shared databases. |
| **Schema First** | All data structures defined as formal schemas (SQL DDL, JSON Schema, Avro, Protobuf). |
| **Immutable Audit** | Critical data changes are append-only. History is never overwritten. |
| **Encryption by Default** | All data encrypted at rest and in transit. |
| **Backward Compatibility** | Schema changes must be backward-compatible for at least 2 versions. |

---

## 3. DATABASE LANDSCAPE

### 3.1 Database Responsibility Matrix

| Data Domain | Primary DB | Cache | Search | Archive |
|---|---|---|---|---|
| Knowledge Graph | PostgreSQL + AGE | Redis | Elasticsearch | PostgreSQL |
| User Profiles | PostgreSQL | Redis | - | PostgreSQL |
| User Progress | PostgreSQL | Redis | - | PostgreSQL |
| Content Metadata | PostgreSQL | Redis | Elasticsearch | PostgreSQL |
| Generated Content | PostgreSQL (meta) + MinIO (files) | CDN | Elasticsearch | MinIO |
| Source Documents | PostgreSQL (meta) + MinIO (files) | - | Elasticsearch | MinIO |
| Book Processing State | PostgreSQL | Redis | - | PostgreSQL |
| Agent State | PostgreSQL | Redis | - | - |
| Workflow State | Temporal (PostgreSQL) | - | - | - |
| Metrics | TimescaleDB | Redis | - | MinIO (cold) |
| Events | Kafka | - | Elasticsearch | MinIO (cold) |
| Audit Logs | PostgreSQL | - | Elasticsearch | MinIO (cold) |
| Configuration | PostgreSQL | Redis | - | - |
| Sessions | - | Redis | - | - |
| Search Index | Elasticsearch | - | - | Elasticsearch snapshots |

---

## 4. POSTGRESQL ARCHITECTURE

### 4.1 Cluster Topology

```
                    +-----------------------------+
                    |   Connection Pool           |
                    |   (PgBouncer / PgCat)       |
                    +-------------+---------------+
                                  |
              +-------------------+-------------------+
              |                   |                   |
    +---------v----------+ +---------v----------+ +---------v----------+
    | Primary            | | Replica 1          | | Replica N          |
    | (Read/Write)       | | (Read-Only)        | | (Read-Only)        |
    |                    | |                    | |                    |
    | - Knowledge Graph  | | - Content Query    | | - Analytics        |
    |   (write)          | | - User Data (read) | | - Reporting        |
    | - User Registration| | - Agent State(read)| | - Search indexing  |
    | - Content Metadata | | - Progress (read)  | | - Backup           |
    +--------------------+ +--------------------+ +--------------------+
              |                   |                   |
              +-------------------+-------------------+
                                  |
                    +-----------------------------+
                    |   Streaming Replication     |
                    |   (Synchronous for critical) |
                    +-----------------------------+
```

### 4.2 Database Instances

| Instance | Purpose | Nodes | Replication | Storage Type |
|---|---|---|---|---|
| `iep-kg` | Knowledge Graph + AGE | Primary + 2 replicas | Sync (primary->replica1), Async (replica1->replica2) | NVMe SSD |
| `iep-user` | User profiles, auth, subscriptions | Primary + 1 replica | Async | SSD |
| `iep-content` | Content metadata, publishing | Primary + 2 replicas | Async | SSD |
| `iep-learning` | Learning progress, assessments | Primary + 2 replicas | Async | SSD |
| `iep-agent` | Agent state, task state | Primary + 1 replica | Sync | SSD |
| `iep-audit` | Audit logs (append-only) | Primary + 1 replica | Async | HDD (large capacity) |
| `iep-config` | Configuration data | Primary (single) | - | SSD (small) |
| `iep-analytics` | Reporting data warehouse | Primary + 1 replica | Async | SSD |

### 4.3 PostgreSQL Configuration

```ini
# Key PostgreSQL Configuration Parameters
max_connections = 500
shared_buffers = 8GB                    # 25% of RAM
effective_cache_size = 24GB             # 75% of RAM
work_mem = 64MB                         # Per-operation sort memory
maintenance_work_mem = 2GB
wal_level = replica
max_wal_senders = 10
max_replication_slots = 10
wal_keep_size = 4096                    # MB
checkpoint_completion_target = 0.9
random_page_cost = 1.1                  # SSD optimization
effective_io_concurrency = 200          # SSD optimization
max_worker_processes = 16
max_parallel_workers_per_gather = 4
max_parallel_workers = 8
autovacuum_max_workers = 6
autovacuum_naptime = 30s               # Aggressive vacuum for high-write tables
idle_in_transaction_session_timeout = 30s
statement_timeout = 30000               # 30 seconds
log_min_duration_statement = 1000       # Log slow queries > 1s
```

### 4.4 Schema Design Principles

| Principle | Implementation |
|---|---|
| UUID Primary Keys | All tables use UUID v7 (time-ordered) for primary keys |
| Created/Updated Timestamps | Every table has `created_at` and `updated_at` |
| Soft Deletes | Use `deleted_at` timestamp instead of hard deletes for critical tables |
| Row-Level Security | Multi-tenant data isolation via RLS policies |
| JSONB for Flexible Data | Use JSONB for attributes that vary by entity type |
| Array Columns | Use PostgreSQL arrays for ordered lists |
| Exclusion Constraints | Prevent overlapping time ranges, duplicate entries |
| Partial Indexes | Index only active/current rows for performance |
| Covering Indexes | Include columns to avoid heap lookups |

---

## 5. APACHE AGE GRAPH EXTENSION

### 5.1 AGE Architecture

Apache AGE runs as a PostgreSQL extension, providing graph database capabilities within PostgreSQL:

```
    PostgreSQL + Apache AGE
    ========================
    
    SQL + openCypher Queries
              |
    +---------v---------+
    | PostgreSQL Parser  |----> SQL path
    +---------+---------+
              |
    +---------v---------+
    | AGE Hook           |----> Intercepts openCypher queries
    +---------+---------+
              |
    +---------v---------+
    | AGE Query Planner  |----> Converts Cypher to SQL
    +---------+---------+
              |
    +---------v---------+
    | AGE Executor       |----> Executes graph operations
    +---------+---------+
              |
    +---------v---------+
    | PostgreSQL Storage |----> Graph data stored in native PG tables
    +-------------------+
```

### 5.2 Graph Data Model

```sql
-- AGE is installed per database
LOAD 'age';
SET search_path = ag_catalog, "$user", public;

-- Create graph namespace
SELECT create_graph('islamic_knowledge');

-- Graph node labels (vertex labels)
-- Concept, Entity, Fact, Source, Event, Location, Time, ScholarlyPosition

-- Graph edge types (edge labels)
-- is_a, part_of, relates_to, located_in, occurs_at, occurs_in,
-- authored_by, states, supports, contradicts, refines,
-- derived_from, cites, translates_of, prerequisite_for,
-- teaches, applies_to, governs, invalidates, obligates

-- Example: Query graph
-- SELECT * FROM cypher('islamic_knowledge', $$
--     MATCH (s:Concept {name: 'Salah'})-[r]->(t)
--     WHERE r.type = 'has_pillar'
--     RETURN t.name, t.description
-- $$) AS (name text, description text);
```

### 5.3 Graph Query Patterns

| Pattern | Cypher | Use Case |
|---|---|---|
| Direct relationship | `MATCH (a)-[r]->(b) WHERE a.name = 'Salah' RETURN b` | Find components of a concept |
| Path traversal | `MATCH path = (a)-[*1..3]->(b) WHERE ... RETURN path` | Explore knowledge connections |
| Shortest path | `MATCH p = shortestPath((a)-[*..10]-(b)) WHERE ... RETURN p` | Find connections between scholars |
| Aggregation | `MATCH (c:Concept)-[r]->(f:Fact) RETURN c.name, count(f)` | Count facts per concept |
| Recommendation | `MATCH (u:User)-[:studied]->(c:Concept)-[:related]->(rec)` | Content recommendation |
| Contradiction | `MATCH (f1:Fact)-[:contradicts]->(f2:Fact) RETURN f1, f2` | Find conflicting facts |
| Provenance chain | `MATCH (f:Fact)-[:derived_from*]->(s:Source) RETURN f, s` | Complete provenance |

---

## 6. PGVECTOR EXTENSION

### 6.1 Vector Store Architecture

pgvector provides vector similarity search within PostgreSQL:

```sql
-- Enable extension
CREATE EXTENSION vector;

-- Create tables with vector columns
CREATE TABLE fact_embeddings (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    fact_id UUID NOT NULL REFERENCES facts(id),
    embedding vector(1536),        -- OpenAI embedding dimension
    model_version TEXT NOT NULL,
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Create indexes for similarity search
CREATE INDEX ON fact_embeddings 
    USING ivfflat (embedding vector_cosine_ops)
    WITH (lists = 100);

-- Alternative: HNSW index for better performance
CREATE INDEX ON fact_embeddings 
    USING hnsw (embedding vector_cosine_ops);
```

### 6.2 Vector Collections

| Collection | Dimensions | Model | Size (Year 5) | Use Case |
|---|---|---|---|---|
| fact_embeddings | 1536 | text-embedding-3-small | 500 GB | Fact similarity search |
| concept_embeddings | 1536 | text-embedding-3-small | 100 GB | Concept matching |
| content_embeddings | 1536 | text-embedding-3-small | 1 TB | Content similarity |
| user_query_embeddings | 1536 | text-embedding-3-small | 50 GB | User intent matching |
| scholar_embeddings | 1536 | text-embedding-3-small | 10 GB | Scholar expertise matching |

---

## 7. TIMESCALEDB EXTENSION

### 7.1 Time-Series Architecture

TimescaleDB handles all time-series data for monitoring and analytics:

```sql
-- Enable extension
CREATE EXTENSION timescaledb;

-- Create hypertable for metrics
CREATE TABLE metrics (
    time TIMESTAMPTZ NOT NULL,
    service_name TEXT NOT NULL,
    metric_name TEXT NOT NULL,
    value DOUBLE PRECISION,
    labels JSONB
);

-- Convert to hypertable
SELECT create_hypertable('metrics', 'time');

-- Create continuous aggregate for hourly rollup
CREATE MATERIALIZED VIEW metrics_hourly
WITH (timescaledb.continuous) AS
SELECT 
    time_bucket('1 hour', time) AS bucket,
    service_name,
    metric_name,
    AVG(value) AS avg_value,
    MAX(value) AS max_value,
    MIN(value) AS min_value,
    COUNT(*) AS sample_count
FROM metrics
GROUP BY bucket, service_name, metric_name;
```

### 7.2 Retention Policies

| Dataset | Retention | Rollup Interval | Compression |
|---|---|---|---|
| Raw metrics | 7 days | - | None |
| Hourly rollup | 90 days | 1 hour | Native compression |
| Daily rollup | 2 years | 1 day | Native compression |
| Monthly rollup | 10 years | 1 month | Native compression |
| Raw events | 30 days | - | None |
| Aggregated events | 2 years | 1 day | Native compression |

---

## 8. REDIS ARCHITECTURE

### 8.1 Redis Cluster Topology

```
    +--------------------------------------------------------+
    |                    REDIS CLUSTER                         |
    |                                                         |
    |  +------------------+  +------------------+              |
    |  | Node 1 (Master)  |  | Node 4 (Replica) |              |
    |  | Slots 0-5461     |  | Slots 0-5461     |              |
    |  +------------------+  +------------------+              |
    |                                                         |
    |  +------------------+  +------------------+              |
    |  | Node 2 (Master)  |  | Node 5 (Replica) |              |
    |  | Slots 5462-10923 |  | Slots 5462-10923 |              |
    |  +------------------+  +------------------+              |
    |                                                         |
    |  +------------------+  +------------------+              |
    |  | Node 3 (Master)  |  | Node 6 (Replica) |              |
    |  | Slots 10924-16383|  | Slots 10924-16383|              |
    |  +------------------+  +------------------+              |
    |                                                         |
    |  Sentinel Management:                                    |
    |  3 Sentinel nodes for automatic failover                |
    +--------------------------------------------------------+
```

### 8.2 Redis Usage Patterns

| Pattern | Data Type | Key Pattern | TTL | Memory |
|---|---|---|---|---|
| Session Store | Hash | `session:{session_id}` | 24h | 10 GB |
| Cache (API) | String | `cache:api:{path}:{params_hash}` | 1-10m | 5 GB |
| Cache (Query) | String | `cache:query:{query_hash}` | 5m | 10 GB |
| Rate Limiting | Sorted Set | `ratelimit:{user_id}:{endpoint}` | 1m | 1 GB |
| Lock Manager | String | `lock:{resource_id}` | 30s | 100 MB |
| Queue (Agent) | List | `queue:agent:{agent_type}` | - | 5 GB |
| Hot State | Hash | `state:{workflow_id}` | 24h | 5 GB |
| Leaderboard | Sorted Set | `leaderboard:{period}` | 1d-30d | 1 GB |
| Global Counters | HyperLogLog | `counter:{metric}:{date}` | 30d | 500 MB |
| Geospatial | Geo | `geo:{entity_type}` | permanent | 1 GB |

---

## 9. APACHE KAFKA ARCHITECTURE

### 9.1 Kafka Cluster Topology

```
    +--------------------------------------------------------+
    |                    KAFKA CLUSTER                         |
    |                                                         |
    |  +------------------+  +------------------+              |
    |  | Broker 1         |  | Broker 2         |              |
    |  | (Controller)     |  |                  |              |
    |  +------------------+  +------------------+              |
    |                                                         |
    |  +------------------+  +------------------+              |
    |  | Broker 3         |  | Broker 4         |              |
    |  |                  |  |                  |              |
    |  +------------------+  +------------------+              |
    |                                                         |
    |  +------------------+  +------------------+              |
    |  | Broker 5         |  | Broker 6         |              |
    |  | (Mirror Maker    |  | (Mirror Maker    |              |
    |  |  to DR region)   |  |  from DR region) |              |
    |  +------------------+  +------------------+              |
    |                                                         |
    |  Zookeeper / KRaft Ensemble (3 nodes)                   |
    |  Schema Registry, KafkConnect, KSQLDB                    |
    +--------------------------------------------------------+
```

### 9.2 Topic Catalog

| Topic | Partitions | Replication | Retention | Throughput | Message Size |
|---|---|---|---|---|---|
| `book.uploaded` | 10 | 3 | 7d | 100 msg/s | 1 KB |
| `book.ocr.completed` | 10 | 3 | 7d | 100 msg/s | 1 MB |
| `book.entities.extracted` | 20 | 3 | 7d | 500 msg/s | 10 KB |
| `book.facts.extracted` | 20 | 3 | 7d | 500 msg/s | 50 KB |
| `book.verification.completed` | 20 | 3 | 30d | 500 msg/s | 10 KB |
| `book.knowledge.stored` | 10 | 3 | 30d | 100 msg/s | 1 KB |
| `content.generation.requested` | 20 | 3 | 7d | 200 msg/s | 10 KB |
| `content.generation.completed` | 20 | 3 | 7d | 200 msg/s | 100 KB |
| `content.published` | 10 | 3 | 30d | 100 msg/s | 1 KB |
| `user.activity` | 50 | 3 | 7d | 10000 msg/s | 500 B |
| `user.progress.updated` | 20 | 3 | 7d | 1000 msg/s | 1 KB |
| `agent.task.{agent_type}` | 5-20 | 3 | 7d | 500 msg/s | 100 KB |
| `agent.result.{agent_type}` | 5-20 | 3 | 7d | 500 msg/s | 500 KB |
| `agent.heartbeat` | 5 | 3 | 1h (compact) | 100 msg/s | 100 B |
| `system.metrics` | 10 | 3 | 3d | 10000 msg/s | 500 B |
| `system.audit` | 10 | 3 | 30d | 500 msg/s | 1 KB |
| `system.error` | 5 | 3 | 30d | 50 msg/s | 10 KB |
| `orchestrator.workflow.state` | 20 | 3 | 7d | 500 msg/s | 5 KB |

---

## 10. ELASTICSEARCH ARCHITECTURE

### 10.1 Cluster Topology

```
    +--------------------------------------------------------+
    |                 ELASTICSEARCH CLUSTER                    |
    |                                                         |
    |  +------------------+  +------------------+              |
    |  | Master Nodes (3) |  | Master Nodes (3) |              |
    |  | (Dedicated,      |  | (Dedicated,      |              |
    |  |  No data)        |  |  No data)        |              |
    |  +------------------+  +------------------+              |
    |                                                         |
    |  +------------------+  +------------------+              |
    |  | Data Node 1      |  | Data Node 2      |              |
    |  | (Hot - SSD)      |  | (Hot - SSD)      |              |
    |  +------------------+  +------------------+              |
    |                                                         |
    |  +------------------+  +------------------+              |
    |  | Data Node 3      |  | Data Node 4      |              |
    |  | (Warm - HDD)     |  | (Warm - HDD)     |              |
    |  +------------------+  +------------------+              |
    |                                                         |
    |  +------------------+  +------------------+              |
    |  | Ingest Node 1    |  | Ingest Node 2    |              |
    |  | (Pipeline)       |  | (Pipeline)       |              |
    |  +------------------+  +------------------+              |
    +--------------------------------------------------------+
```

### 10.2 Index Catalog

| Index | Shards | Replicas | Documents (Year 5) | Size | Refresh |
|---|---|---|---|---|---|
| `books` | 5 | 2 | 100K | 10 GB | Daily |
| `knowledge_facts` | 20 | 2 | 1B | 500 GB | Real-time |
| `knowledge_concepts` | 10 | 2 | 10M | 50 GB | Real-time |
| `content_text` | 30 | 2 | 100M | 1 TB | Real-time |
| `hadith` | 5 | 2 | 100K | 50 GB | Static |
| `quran_verses` | 3 | 2 | 6K translations | 5 GB | Static |
| `scholars` | 3 | 2 | 100K | 1 GB | Weekly |
| `users` (admin) | 5 | 2 | 50M | 50 GB | Real-time |
| `audit_logs` | 10 | 2 | 1B | 500 GB | Real-time |

---

## 11. MINIO OBJECT STORE

### 11.1 Bucket Structure

| Bucket | Access | Encryption | Retention | Size (Year 5) |
|---|---|---|---|---|
| `source-books` | Private (services only) | AES-256 | Permanent | 100 TB |
| `ocr-intermediate` | Private (OCR pipeline) | AES-256 | 30 days | 10 TB |
| `generated-media` | CDN | AES-256 | Permanent | 5 PB |
| `knowledge-exports` | Private (admin) | AES-256 | 90 days | 10 TB |
| `user-uploads` | Private (user auth) | AES-256 | 30 days | 1 TB |
| `system-backups` | Private (ops only) | AES-256 + Vault key | 90 days (daily) | 500 TB |
| `logs-archive` | Private (ops only) | AES-256 | 7 years | 500 TB |
| `ml-models` | Private (ML platform) | AES-256 | Versioned | 10 TB |

### 11.2 MinIO Configuration

```yaml
minio:
  mode: distributed
  nodes: 8
  drives_per_node: 4
  erasure_code_parity: 4  # 8+4 EC, can lose 4 drives per set
  tiering:
    hot: nvme-ssd  (first 7 days)
    warm: hdd      (7-90 days)
    cold: s3-glacier (90+ days)
  versioning: enabled for critical buckets
  locking: enabled for compliance buckets
  encryption: SSE-S3 with automatic key rotation
```

---

## 12. DATABASE SCHEMA CATALOG

### 12.1 Core Schema Tables (iep-kg database)

```sql
-- Knowledge Graph: Node types
CREATE TABLE graph_nodes (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    node_type TEXT NOT NULL,  -- concept, entity, fact, source, event, location, time, scholarly_position
    name TEXT NOT NULL,
    properties JSONB DEFAULT '{}',
    confidence DOUBLE PRECISION DEFAULT 1.0,
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW(),
    deleted_at TIMESTAMPTZ,
    CHECK (confidence >= 0 AND confidence <= 1)
);

-- Knowledge Graph: Edge types
CREATE TABLE graph_edges (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    source_node_id UUID NOT NULL REFERENCES graph_nodes(id),
    target_node_id UUID NOT NULL REFERENCES graph_nodes(id),
    edge_type TEXT NOT NULL,  -- is_a, part_of, states, contradicts, etc.
    properties JSONB DEFAULT '{}',
    confidence DOUBLE PRECISION DEFAULT 1.0,
    created_at TIMESTAMPTZ DEFAULT NOW(),
    deleted_at TIMESTAMPTZ,
    CHECK (confidence >= 0 AND confidence <= 1)
);

-- Provenance records
CREATE TABLE provenance_records (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    fact_id UUID NOT NULL REFERENCES graph_nodes(id),
    source_document_id UUID NOT NULL,
    page_number INTEGER,
    paragraph_index INTEGER,
    char_offset_start INTEGER,
    char_offset_end INTEGER,
    extraction_method TEXT NOT NULL,  -- ocr, manual, api
    extracted_by_agent TEXT NOT NULL,
    verification_status TEXT NOT NULL DEFAULT 'unverified',
    verification_confidence DOUBLE PRECISION,
    source_hash TEXT NOT NULL,  -- SHA-256 of source content
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Verification chain
CREATE TABLE verification_chain (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    fact_id UUID NOT NULL REFERENCES graph_nodes(id),
    verification_layer TEXT NOT NULL,  -- cross_ref, llm_check, contradiction, scholarly
    verifier_id TEXT NOT NULL,  -- agent_id or human_reviewer_id
    result TEXT NOT NULL,  -- confirmed, supported, unverified, contradicted, uncertain
    confidence DOUBLE PRECISION,
    evidence JSONB,
    created_at TIMESTAMPTZ DEFAULT NOW()
);
```

### 12.2 User Schema (iep-user database)

```sql
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email TEXT UNIQUE,
    phone TEXT,
    password_hash TEXT,
    auth_provider TEXT,  -- email, google, apple, etc.
    auth_provider_id TEXT,
    name TEXT NOT NULL,
    preferred_language TEXT DEFAULT 'en',
    roles TEXT[] DEFAULT '{user}',
    subscription_tier TEXT DEFAULT 'free',
    tenant_id UUID NOT NULL,
    is_active BOOLEAN DEFAULT true,
    email_verified BOOLEAN DEFAULT false,
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW(),
    deleted_at TIMESTAMPTZ
);

CREATE TABLE user_profiles (
    user_id UUID PRIMARY KEY REFERENCES users(id),
    display_name TEXT,
    bio TEXT,
    avatar_url TEXT,
    date_of_birth DATE,
    gender TEXT,
    country TEXT,
    timezone TEXT DEFAULT 'UTC',
    learning_preferences JSONB DEFAULT '{}',
    accessibility_preferences JSONB DEFAULT '{}',
    notification_preferences JSONB DEFAULT '{}',
    updated_at TIMESTAMPTZ DEFAULT NOW()
);
```

### 12.3 Content Schema (iep-content database)

```sql
CREATE TABLE content_items (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    book_id UUID,
    title TEXT NOT NULL,
    content_type TEXT NOT NULL,  -- story, quiz, flashcard, podcast, timeline, etc.
    status TEXT NOT NULL DEFAULT 'draft',  -- draft, generating, ready, published, archived
    language TEXT NOT NULL DEFAULT 'en',
    source_fact_ids UUID[],
    quality_score DOUBLE PRECISION,
    confidence_score DOUBLE PRECISION,
    metadata JSONB DEFAULT '{}',
    version INTEGER DEFAULT 1,
    created_by_agent TEXT,
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW(),
    published_at TIMESTAMPTZ,
    deleted_at TIMESTAMPTZ
);

CREATE TABLE content_media (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    content_item_id UUID NOT NULL REFERENCES content_items(id),
    media_type TEXT NOT NULL,  -- audio, video, image, document, interactive
    format TEXT NOT NULL,  -- mp3, mp4, webp, pdf, json
    url TEXT NOT NULL,
    size_bytes BIGINT,
    duration_seconds INTEGER,
    checksum TEXT,  -- SHA-256
    created_at TIMESTAMPTZ DEFAULT NOW()
);
```

### 12.4 Learning Schema (iep-learning database)

```sql
CREATE TABLE user_learning_progress (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID NOT NULL,
    content_item_id UUID NOT NULL,
    status TEXT NOT NULL DEFAULT 'not_started',  -- not_started, in_progress, completed, reviewed
    progress_percent DOUBLE PRECISION DEFAULT 0,
    score DOUBLE PRECISION,
    time_spent_seconds INTEGER DEFAULT 0,
    attempts INTEGER DEFAULT 0,
    last_accessed_at TIMESTAMPTZ,
    completed_at TIMESTAMPTZ,
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW(),
    UNIQUE(user_id, content_item_id)
);

CREATE TABLE user_knowledge_state (
    user_id UUID NOT NULL,
    concept_id UUID NOT NULL,
    mastery_level DOUBLE PRECISION DEFAULT 0,  -- 0 to 1
    confidence DOUBLE PRECISION DEFAULT 0,
    last_assessed_at TIMESTAMPTZ,
    next_review_at TIMESTAMPTZ,
    review_count INTEGER DEFAULT 0,
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW(),
    PRIMARY KEY (user_id, concept_id)
);
```

### 12.5 Agent State Schema (iep-agent database)

```sql
CREATE TABLE agent_tasks (
    task_id UUID PRIMARY KEY,
    agent_type TEXT NOT NULL,
    workflow_id UUID NOT NULL,
    status TEXT NOT NULL DEFAULT 'pending',  -- pending, queued, dispatched, running, completed, failed, timed_out
    priority INTEGER DEFAULT 5,
    input JSONB,
    output JSONB,
    confidence DOUBLE PRECISION,
    error TEXT,
    retry_count INTEGER DEFAULT 0,
    max_retries INTEGER DEFAULT 3,
    cost_usd DOUBLE PRECISION,
    tokens_used INTEGER,
    queue_duration_ms INTEGER,
    execution_duration_ms INTEGER,
    assigned_agent_instance TEXT,
    created_at TIMESTAMPTZ DEFAULT NOW(),
    started_at TIMESTAMPTZ,
    completed_at TIMESTAMPTZ
);

CREATE INDEX idx_agent_tasks_status ON agent_tasks(status);
CREATE INDEX idx_agent_tasks_agent_type ON agent_tasks(agent_type);
```

---

## 13. DATA MIGRATION STRATEGY

### 13.1 Migration Principles

| Principle | Implementation |
|---|---|
| **Backward Compatible** | Migrations must not break running services |
| **Zero Downtime** | Migrations run without taking databases offline |
| **Expand-Contract** | Add new schema, migrate data, remove old schema |
| **Versioned** | All migrations are versioned and reversible |
| **Automated** | Migrations run as part of CI/CD pipeline |

### 13.2 Migration Tools

| Tool | Purpose | Usage |
|---|---|---|
| Sqitch | PostgreSQL schema migrations | All domain databases |
| Flyway | Schema versioning | Alternative for specific teams |
| Custom scripts | Data migrations | Complex data transformations |
| Kafka Connect | Zero-downtime data sync | Between databases |

---

## 14. BACKUP AND RECOVERY

### 14.1 Backup Schedule

| Database | Type | Frequency | Retention | Storage |
|---|---|---|---|---|
| PostgreSQL (all) | pg_dump (logical) | Daily | 90 daily, 12 monthly, 7 yearly | MinIO |
| PostgreSQL (all) | WAL archiving | Continuous | 30 days (for PITR) | MinIO |
| TimescaleDB | pg_dump | Daily | 30 days | MinIO |
| Elasticsearch | Snapshot | Daily | 30 days | MinIO |
| Redis | RDB snapshot | Hourly | 7 days | MinIO |
| MinIO | Cross-region replication | Continuous | 30 days version history | DR region |
| Kafka | Log retention + backup | Continuous | 7 days (topic) + 30 days (backup) | MinIO |

### 14.2 Recovery Time Objectives

| Scenario | RTO | RPO |
|---|---|---|
| PostgreSQL single node failure | 5 minutes | 0 (sync replica) |
| PostgreSQL complete failure | 30 minutes | 5 minutes (WAL) |
| Elasticsearch failure | 30 minutes | 0 (replica) |
| Redis failure | 5 minutes | 0 (replica) |
| Kafka failure | 15 minutes | 0 (replication) |
| MinIO failure | 15 minutes | 0 (erasure coding) |
| Full region failure | 4 hours | 15 minutes |

---

## 15. REPLICATION STRATEGY

### 15.1 Replication Topology

```
    Region A (Primary)                    Region B (DR)
    ================                      ================
    
    PostgreSQL Primary <----sync----> PostgreSQL Replica
         |                                      |
    Streaming Replication              Streaming Replication
         |                                      |
    PostgreSQL Replica 1            PostgreSQL Replica
         |                                      |
    Async Replication                           |
         |                                      |
    PostgreSQL Replica 2                        |
         |                                      |
    TimescaleDB Primary ----------> TimescaleDB Replica
         |                                      |
    Kafka (Primary) ---------------> Kafka (Mirror)
         |                                      |
    MinIO (Primary) ---------------> MinIO (Replica)
```

### 15.2 Replication Decisions

| Database | Replication Method | Sync/Async | Automatic Failover |
|---|---|---|---|
| PostgreSQL (KG) | Streaming | Sync (1 replica) | Yes (Patroni) |
| PostgreSQL (User) | Streaming | Async | Yes (Patroni) |
| PostgreSQL (Content) | Streaming | Async | Yes (Patroni) |
| PostgreSQL (Learning) | Streaming | Async | Yes (Patroni) |
| TimescaleDB | Streaming | Async | Yes (Patroni) |
| Redis | Cluster replication | Async | Yes (Sentinel) |
| Kafka | MirrorMaker 2 | Async | Yes (manual promotion) |
| Elasticsearch | Cross-cluster | Async | Yes (manual) |
| MinIO | Bucket replication | Async | Yes (DNS redirect) |

---

## 16. SHARDING STRATEGY

### 16.1 PostgreSQL Sharding (Citus)

Sharding is implemented using **Citus** for tables that exceed single-node capacity:

```sql
-- Citus distributed table example
SELECT create_distributed_table('graph_nodes', 'node_type');
SELECT create_distributed_table('graph_edges', 'source_node_id');
SELECT create_distributed_table('provenance_records', 'fact_id');
```

| Shard Column | Distribution | Workers | Rebalance Strategy |
|---|---|---|---|
| `node_type` | Hash | 8 | Automatic (Citus) |
| `user_id` (mod) | Hash | 8 | Manual (maintenance window) |
| `content_type` | Hash | 4 | Automatic (Citus) |
| `fact_id` | Hash | 8 | Automatic (Citus) |

### 16.2 Shard Key Design

| Table | Shard Key | Reason |
|---|---|---|
| graph_nodes | node_type | Even distribution across types |
| graph_edges | source_node_id | Co-locate edges with source nodes |
| provenance_records | fact_id | Co-locate provenance with facts |
| user_learning_progress | user_id | All user data on same shard |
| content_items | content_type | Co-locate by content type |

---

## 17. CONNECTION MANAGEMENT

### 17.1 Connection Pool Architecture

```
    Application Service
         |
    PgBouncer (Transactional pool)
         |
    +------+------+
    |      |      |
    v      v      v
  Primary  Replica1  Replica2
```

### 17.2 Connection Limits

| Service | Max Connections | Pool Mode | Timeout |
|---|---|---|---|
| API Gateway | 100 | Transaction | 30s |
| Knowledge Services | 200 | Transaction | 30s |
| Content Services | 150 | Session | 60s |
| Content Gen Agents | 100 | Transaction | 120s |
| Learning Services | 200 | Transaction | 30s |
| Analytics | 50 | Statement | 300s |
| Admin | 20 | Session | 60s |
| Migration Jobs | 10 | Session | 3600s |

---

## 18. QUERY OPTIMIZATION

### 18.1 Index Strategy

| Index Type | Use Case | Frequency of Use |
|---|---|---|
| B-tree | Primary keys, unique constraints, equality queries | Continuous |
| Hash | Equality on large text columns | Frequent |
| GiST | Full-text search, geometric data | Moderate |
| GIN | JSONB queries, array contains | Moderate |
| BRIN | Time-ordered data (logs, events) | Continuous |
| Partial | Filtered indexes on active rows | Continuous |
| Covering | Include columns to avoid heap lookups | Optimization |
| Vector (IVFFlat) | Approximate nearest neighbor | Frequent |
| Vector (HNSW) | High-precision nearest neighbor | Moderate |

### 18.2 Slow Query Detection

```sql
-- Log queries exceeding threshold
SET log_min_duration_statement = 1000; -- 1 second

-- Monitor
SELECT * FROM pg_stat_statements 
ORDER BY total_time DESC 
LIMIT 20;

-- Missing index detection
SELECT * FROM pg_stat_user_tables 
WHERE seq_scan > 1000 
  AND seq_scan > idx_scan;
```

### 18.3 Query Optimization Guidelines

| Issue | Solution |
|---|---|
| Sequential scans on large tables | Add appropriate indexes |
| High buffer cache miss ratio | Increase shared_buffers |
| Slow aggregations | Materialized views with refresh |
| Frequent similar queries | Prepared statements |
| N+1 queries in ORM | Batch loading, join optimization |
| Large IN lists | Temporary table join |

---

## 19. DATABASE SECURITY

### 19.1 Access Control

| Level | Mechanism | Scope |
|---|---|---|
| Network | VPC, private subnets, security groups | All databases |
| Connection | TLS 1.3 + mTLS | All databases |
| Authentication | SCRAM-SHA-256, Vault dynamic credentials | PostgreSQL |
| Authorization | GRANT + RLS | Row-level |
| Audit | pgaudit extension | All DDL, DML, access |

### 19.2 Row-Level Security Policies

```sql
-- Multi-tenant isolation
CREATE POLICY tenant_isolation ON graph_nodes 
    USING (tenant_id = current_setting('app.current_tenant_id') 
           OR tenant_id = '00000000-0000-0000-0000-000000000000');

-- Content access by publication status
CREATE POLICY content_access ON content_items 
    USING (status = 'published' 
           OR current_user = 'admin' 
           OR created_by = current_user_id());
```

### 19.3 Encryption

| Layer | Method | Key Management |
|---|---|---|
| In Transit | TLS 1.3 | Auto-rotated certificates |
| At Rest (disk) | LUKS / Cloud KMS | Cloud HSM |
| At Rest (database) | pg_tde (transparent) | Vault |
| Column Level | pgcrypto + app key | Vault transit engine |
| Backup | GPG / Vault | Offline key storage |

---

## 20. DATABASE MONITORING

### 20.1 Key Metrics

| Metric | Source | Alert Threshold |
|---|---|---|
| Connection count | pg_stat_activity | > 80% of max |
| Replication lag | pg_stat_replication | > 10s (sync), > 60s (async) |
| Transaction age | pg_stat_activity | > 5 minutes |
| Cache hit ratio | pg_statio_user_tables | < 95% |
| Index scan ratio | pg_stat_user_tables | < 50% sequential scans |
| Long queries | pg_stat_activity | > 30 seconds |
| Deadlocks | pg_stat_database | > 0 |
| WAL generation rate | WAL file count | > 500 GB/hour |
| Disk usage | df / mount | > 80% |
| Table bloat | pgstattuple | > 20% bloat |

---

## 21. DATABASE OPERATIONS

### 21.1 Routine Maintenance

| Task | Frequency | Window | Automation |
|---|---|---|---|
| VACUUM (auto) | Continuous | - | Auto-vacuum daemon |
| VACUUM (manual) | Weekly | Low-traffic period | Cron job |
| REINDEX | Monthly | Maintenance window | Cron job |
| ANALYZE | Continuous | - | Auto-analyze daemon |
| pg_stat_reset() | Never | - | - |
| Backup verification | Weekly | During backup | Automated script |
| Query performance review | Monthly | - | DBA review |

### 21.2 Health Checks

```yaml
health_checks:
  postgresql:
    - check: "SELECT 1"
      interval: 10s
    - check: "replication_lag < 10s"
      interval: 30s
    - check: "connections < 80% max"
      interval: 60s
  
  redis:
    - check: "PING"
      interval: 10s
    - check: "memory_usage < 80%"
      interval: 60s
  
  kafka:
    - check: "broker_health"
      interval: 30s
    - check: "consumer_lag < threshold"
      interval: 60s
```

---

## 22. APPENDICES

### Appendix A: Connection Strings Reference

| Database | Host | Port | Database Name | Pool |
|---|---|---|---|---|
| Knowledge Graph | postgres-kg-primary | 5432 | iep_kg | pgbouncer-kg:6432 |
| User | postgres-user-primary | 5432 | iep_user | pgbouncer-user:6432 |
| Content | postgres-content-primary | 5432 | iep_content | pgbouncer-content:6432 |
| Learning | postgres-learning-primary | 5432 | iep_learning | pgbouncer-learning:6432 |
| Agent | postgres-agent-primary | 5432 | iep_agent | pgbouncer-agent:6432 |
| Audit | postgres-audit-primary | 5432 | iep_audit | pgbouncer-audit:6432 |
| Metrics | timescaledb-primary | 5432 | iep_metrics | pgbouncer-metrics:6432 |
| Redis | redis-cluster | 6379 | 0-15 | redis-cluster |
| Kafka | kafka-broker:9092 | 9092 | - | kafka-broker:9092 |
| Elasticsearch | elasticsearch:9200 | 9200 | - | elasticsearch:9200 |
| MinIO | minio:9000 | 9000 | - | minio:9000 |

### Appendix B: Capacity Planning

| Resource | Current (Year 1) | Year 3 | Year 5 |
|---|---|---|---|
| PostgreSQL Storage | 2 TB | 10 TB | 25 TB |
| TimescaleDB Storage | 10 TB | 100 TB | 500 TB |
| Redis Memory | 64 GB | 256 GB | 1 TB |
| Kafka Storage | 10 TB | 50 TB | 100 TB |
| Elasticsearch Storage | 1 TB | 10 TB | 25 TB |
| MinIO Storage | 10 TB | 1 PB | 10 PB |
| PostgreSQL Connections | 500 | 2000 | 5000 |
| Redis Operations/sec | 100K | 1M | 5M |
| Kafka Messages/sec | 10K | 100K | 500K |

### Appendix C: Related Documents

| Document ID | Title | Relationship |
|---|---|---|
| IEP-ARCH-001 | System Architecture | Overall architecture |
| IEP-ARCH-KG-001 | Knowledge Graph Architecture | Graph schema and queries |
| IEP-ARCH-RAG-001 | RAG Architecture | Vector store usage |
| IEP-SEC-001 | Security Architecture | Database security |
| IEP-DEPLOY-001 | Deployment Architecture | Database deployment |

---

**END OF DATABASE ARCHITECTURE**
