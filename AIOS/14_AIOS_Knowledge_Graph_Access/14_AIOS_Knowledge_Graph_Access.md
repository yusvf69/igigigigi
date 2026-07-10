# AIOS Knowledge Graph Access — Engineering Specification v1.0

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
The AIOS Knowledge Graph Access subsystem provides a unified, secure, and high-performance interface for agents to query, traverse, and manipulate the AIOS knowledge graph. The knowledge graph encodes structured domain knowledge — including Islamic jurisprudence rulings, entity relationships, scholarly references, and conceptual hierarchies — as a property graph with typed nodes, edges, and attributes.

### 1.2 Mission
To enable AI agents to perform complex graph queries — including node lookups, relationship traversals, path finding, and subgraph extraction — against a multi-tenant knowledge graph with strong consistency guarantees, fine-grained access control, and millisecond-level query latency. The subsystem abstracts the complexity of the underlying graph database, query languages, and optimization strategies behind a simple, secure, and observable API.

### 1.3 Responsibilities
- Managing graph database connection pooling across read replicas and write masters
- Providing a unified graph query interface supporting Cypher, SPARQL, and a custom AIOS graph query language (GQL)
- Implementing query execution: planning, optimization, and result production
- Supporting fundamental query patterns: node lookup by ID or property, relationship traversal, shortest-path and all-paths finding, subgraph extraction, and aggregation queries
- Caching query results with configurable TTL, namespace-aware cache isolation
- Managing graph schema versions and supporting schema evolution
- Executing transactional graph operations with ACID guarantees for write transactions
- Creating and managing graph projections: agent-specific views, materialized subgraphs, and virtual graphs
- Enforcing knowledge graph access permissions: read-only, read-write, and admin per subgraph or node type
- Optimizing graph queries through query planning, index utilization analysis, index recommendations, and fan-out limits
- Monitoring graph health, failover handling, and automatic read-replica promotion
- Coordinating with the Database Manager for persistence and with the Vector Memory for hybrid graph-vector queries
- Supporting full CRUD operations on nodes, edges, and properties

### 1.4 Non-Responsibilities
- Does not define or manage the ontology/schema of the knowledge graph (delegated to domain experts)
- Does not perform graph analytics or graph algorithms (community detection, PageRank, etc.) — these are future enhancements
- Does not generate embeddings for graph elements (delegated to Vector Memory)
- Does not manage relational database operations (delegated to Database Manager)
- Does not provide a triple store or RDF storage (graph is a labeled property graph)
- Does not implement full-text search (delegated to Vector Memory / search engines)
- Does not manage file storage or document storage
- Does not manage user accounts or authentication (delegated to AIOS Security Module)

---

## 2. Architecture, Internal Components, External Components

### 2.1 Overall Architecture

The Knowledge Graph Access subsystem is a query-routing and query-execution layer that sits between AIOS agents and the underlying graph database (Neo4j / Apache TinkerPop-compatible engine). It provides connection management, query transformation, caching, authorization, and observability.

```
┌──────────────────────────────────────────────────────────────────────────┐
│                      AIOS Knowledge Graph Access Service                   │
│                                                                            │
│  ┌──────────────────────────────────────────────────────────────────┐    │
│  │                    Graph API Gateway                               │    │
│  │  (Cypher, SPARQL, AIOS GQL, REST, gRPC)                          │    │
│  └──────────────────────────────────────────────────────────────────┘    │
│                                    │                                      │
│  ┌──────────────────────────────────────────────────────────────────┐    │
│  │              Query Orchestrator                                   │    │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌────────────────────┐  │    │
│  │  │ Query    │ │ Query    │ │ Query    │ │ Query              │  │    │
│  │  │ Parser   │ │ Validator│ │ Optimizer│ │ Executor           │  │    │
│  │  └──────────┘ └──────────┘ └──────────┘ └────────────────────┘  │    │
│  └──────────────────────────────────────────────────────────────────┘    │
│                                    │                                      │
│  ┌──────────────────────────────────────────────────────────────────┐    │
│  │              Connection Management                                │    │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌────────────────────┐  │    │
│  │  │ Pool     │ │ Read     │ │ Write    │ │ Replica            │  │    │
│  │  │ Manager  │ │ Router   │ │ Router   │ │ Monitor            │  │    │
│  │  └──────────┘ └──────────┘ └──────────┘ └────────────────────┘  │    │
│  └──────────────────────────────────────────────────────────────────┘    │
│                                    │                                      │
│  ┌──────────────────────────────────────────────────────────────────┐    │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌────────────────────┐  │    │
│  │  │ Schema   │ │ Project- │ │ Cache    │ │ Permission         │  │    │
│  │  │ Manager  │ │ ion Mgr  │ │ Manager  │ │ Enforcer           │  │    │
│  │  └──────────┘ └──────────┘ └──────────┘ └────────────────────┘  │    │
│  └──────────────────────────────────────────────────────────────────┘    │
│                                    │                                      │
│  ┌──────────────────────────────────────────────────────────────────┐    │
│  │              Backend Abstraction                                  │    │
│  │  ┌──────────────┐  ┌──────────────┐  ┌──────────────────────┐   │    │
│  │  │ Neo4j Adapter│  │ TinkerPop    │  │ AIOS Embedded        │   │    │
│  │  │ (Primary)    │  │ Adapter      │  │ Graph Engine (Lite)   │   │    │
│  │  └──────────────┘  └──────────────┘  └──────────────────────┘   │    │
│  └──────────────────────────────────────────────────────────────────┘    │
└──────────────────────────────────────────────────────────────────────────┘
```

### 2.2 Internal Components

#### 2.2.1 Graph API Gateway
Unified entry point for all graph queries, supporting multiple query languages and protocols.

#### 2.2.2 Query Orchestrator
Parses, validates, optimizes, and executes graph queries against the backend.

Subcomponents:
- Query Parser: Parses Cypher, SPARQL, and AIOS GQL into an intermediate representation (IR)
- Query Validator: Validates queries against schema, permissions, and resource limits
- Query Optimizer: Rewrites query IR for optimal execution (index selection, join ordering, fan-out limits)
- Query Executor: Sends optimized query to backend, manages result streaming and pagination

#### 2.2.3 Connection Manager
Manages connections to the graph database cluster.

Subcomponents:
- Pool Manager: Maintains connection pools for read replicas and write master
- Read Router: Routes read queries to replicas with configurable read preference
- Write Router: Routes write queries to master with quorum awareness
- Replica Monitor: Tracks replica lag, health, and availability for intelligent routing

#### 2.2.4 Schema Manager
Manages graph schema versions and evolution.

Subcomponents:
- Schema Versioner: Tracks schema changes with versioned migrations
- Index Manager: Creates, drops, and recommends indexes based on query patterns
- Constraint Enforcer: Enforces uniqueness and property constraints
- Schema Validator: Validates data operations against current schema

#### 2.2.5 Projection Manager
Creates and manages agent-specific or department-specific graph views.

Subcomponents:
- View Creator: Defines virtual graphs or materialized subgraphs
- View Refresh: Refreshes materialized views on schedule or trigger
- View Security: Enforces view-specific access controls

#### 2.2.6 Cache Manager
Multi-level caching for query results, schema metadata, and frequent traversals.

#### 2.2.7 Permission Enforcer
Evaluates and enforces graph-level and subgraph-level permissions on every operation.

### 2.3 External Components

| Component | Integration | Protocol |
|-----------|-------------|----------|
| Neo4j 5.x | Primary graph database | Bolt protocol |
| Apache TinkerPop | Alternative graph backend | Gremlin WebSocket |
| Database Manager | Cross-DB transactions | Internal API |
| Vector Memory | Hybrid graph-vector search | gRPC |
| Memory System | Query result caching (L2/L3) | Internal API |
| Security Module | Authentication, authorization | Internal API |
| Config Service | Dynamic configuration | gRPC |
| Monitoring Stack | Metrics, logging, tracing | Prometheus / OpenTelemetry |

---

## 3. Dependencies, Inputs, Outputs

### 3.1 Dependencies

#### 3.1.1 Runtime Dependencies
- Neo4j 5.15+ (Enterprise Edition for production, Community for dev/test)
- AIOS Security Module for JWT validation and permission resolution
- AIOS Config Service for dynamic configuration
- AIOS Database Manager for cross-component transaction coordination

#### 3.1.2 Infrastructure Dependencies
- Kubernetes 1.24+ for orchestration
- SSD storage for graph database (high IOPS required)
- 10GbE+ network for graph query latency
- Neo4j clustering (primary + read replicas) for high availability

### 3.2 Inputs

| Input | Source | Format | Description |
|-------|--------|--------|-------------|
| GraphQueryRequest | Agent | Protobuf | Query string + language + parameters |
| NodeCreateRequest | Agent | Protobuf | Node labels, properties |
| EdgeCreateRequest | Agent | Protobuf | Source/target nodes, type, properties |
| NodeUpdateRequest | Agent | Protobuf | Node ID, property updates |
| NodeDeleteRequest | Agent | Protobuf | Node ID or match pattern |
| SubgraphExtractRequest | Agent | Protobuf | Start nodes, depth, edge types |
| PathFindRequest | Agent | Protobuf | Start node, end node, max depth |
| TransactionRequest | Agent | Protobuf | Begin/commit/rollback |
| SchemaMigrationRequest | Admin | JSON | Schema changes, versioning |
| ProjectionCreateRequest | Admin | JSON | View definition |

### 3.3 Outputs

| Output | Destination | Format | Description |
|--------|-------------|--------|-------------|
| GraphQueryResponse | Agent | Protobuf | Query results as list of records |
| NodeInfo | Agent | Protobuf | Node with labels, properties, metadata |
| EdgeInfo | Agent | Protobuf | Edge with type, properties, endpoints |
| SubgraphResponse | Agent | Protobuf | Subgraph as node+edge list |
| PathResponse | Agent | Protobuf | Ordered list of nodes+edges |
| TransactionStatus | Agent | Protobuf | Transaction ID, status |
| Metrics | Monitoring | Prometheus | Query latency, cache hit, error rate |
| SchemaInfo | Admin | JSON | Current schema with versions |

---

## 4. Data Structures, Execution Flow, State Management

### 4.1 Data Structures

#### 4.1.1 GraphNode
```
GraphNode {
    id: UUID  // internal graph ID
    external_id: Option<String>  // domain-specific ID (e.g., "ruling-0427")
    labels: List<String>  // e.g., ["Ruling", "Fiqh", "Prayer"]
    properties: Map<String, Value>
    created_at: Timestamp
    updated_at: Timestamp
    version: uint64
    tenant_id: String
    subgraph_id: Option<String>
    permissions: Map<String, List<String>>  // action → roles
}
```

#### 4.1.2 GraphEdge
```
GraphEdge {
    id: UUID
    type: String  // e.g., "REFERENCES", "DERIVES_FROM", "CONTRADICTS"
    source_node_id: UUID
    target_node_id: UUID
    properties: Map<String, Value>
    directed: bool
    weight: Option<float64>
    created_at: Timestamp
    updated_at: Timestamp
    version: uint64
    tenant_id: String
}
```

#### 4.1.3 SubgraphDefinition
```
SubgraphDefinition {
    subgraph_id: UUID
    name: String
    description: String
    owner_tenant: String
    type: SubgraphType  // VIRTUAL, MATERIALIZED, PROJECTION
    base_query: String  // Cypher query defining the subgraph
    nodes: List<NodePattern>
    edges: List<EdgePattern>
    refresh_policy: Option<RefreshPolicy>
    permissions: SubgraphPermissions
    created_at: Timestamp
    ttl: Option<Duration>
    status: SubgraphStatus  // ACTIVE, REFRESHING, STALE, ERROR
    size_bytes: Option<uint64>
}
```

#### 4.1.4 QueryPlan
```
QueryPlan {
    plan_id: UUID
    query_hash: String
    original_query: String
    optimized_query: String
    query_type: QueryType  // READ, WRITE, ADMIN
    estimated_cost: QueryCost
    estimated_rows: uint64
    index_usage: List<IndexInfo>
    fan_out_estimate: uint32
    execution_strategy: ExecutionStrategy  // DIRECT, PARALLEL, PIPELINED
    cache_policy: CachePolicy  // CACHEABLE, NON_CACHEABLE
}
```

#### 4.1.5 QueryCost
```
QueryCost {
    estimated_cpu_ms: uint64
    estimated_io_ms: uint64
    estimated_network_ms: uint64
    estimated_total_ms: uint64
    risk_level: RiskLevel  // LOW, MEDIUM, HIGH, CRITICAL
}
```

#### 4.1.6 IndexInfo
```
IndexInfo {
    index_name: String
    index_type: IndexType  // BTREE, RANGE, TEXT, FULLTEXT, LOOKUP, POINT
    node_label: Option<String>
    edge_type: Option<String>
    properties: List<String>
    cardinality: uint64
    selectivity: float64  // 0.0 - 1.0
    last_used: Timestamp
    usage_count: uint64
}
```

#### 4.1.7 SchemaVersion
```
SchemaVersion {
    version_id: UUID
    version_number: uint32
    status: SchemaVersionStatus  // PENDING, APPLYING, APPLIED, FAILED, ROLLED_BACK
    changes: List<SchemaChange>
    migration_script: String
    checksum: bytes
    applied_at: Option<Timestamp>
    applied_by: Option<String>
    rollback_script: Option<String>
    depends_on: Option<UUID>
}
```

#### 4.1.8 TransactionContext
```
TransactionContext {
    transaction_id: UUID
    agent_id: AgentID
    tenant_id: String
    status: TransactionStatus  // ACTIVE, COMMITTING, COMMITTED, ROLLING_BACK, ROLLED_BACK
    started_at: Timestamp
    last_active: Timestamp
    timeout: Duration
    operations: uint32
    query_count: uint32
    read_connection: Option<ConnectionID>
    write_connection: Option<ConnectionID>
    isolation_level: IsolationLevel  // READ_COMMITTED, REPEATABLE_READ, SERIALIZABLE
    lock_status: LockStatus
}
```

#### 4.1.9 PathResult
```
PathResult {
    nodes: List<GraphNode>
    edges: List<GraphEdge>
    total_length: uint32
    total_weight: Option<float64>
    path_type: PathType  // SHORTEST, ALL, SIMPLE, WEIGHTED
    hops: List<PathHop>
}
```

#### 4.1.10 PathHop
```
PathHop {
    hop_number: uint32
    source_node: GraphNode
    edge: GraphEdge
    target_node: GraphNode
    edge_type: String
    direction: String  // OUTGOING, INCOMING, BOTH
    weight: Option<float64>
}
```

### 4.2 Execution Flow

#### 4.2.1 Graph Query Execution Flow
1. Agent sends GraphQueryRequest via gRPC or REST
2. Graph API Gateway authenticates request (JWT validation)
3. Query Orchestrator parses query into intermediate representation:
   a. SQL parser for Cypher → AST
   b. SPARQL parser → algebra expression
   c. AIOS GQL parser → internal IR
4. Query Validator checks:
   a. Schema compliance (node labels, edge types, property types)
   b. Permission: does agent have access to queried graph elements?
   c. Resource limits: query complexity within bounds
5. Query Optimizer rewrites IR:
   a. Apply predicate pushdown (filter as early as possible)
   b. Apply index selection (use most selective index)
   c. Limit fan-out (prevent excessive intermediate results)
   d. Apply query parameterization for cache reuse
6. Query Cache check:
   a. Compute cache key from query hash + params + agent permissions
   b. If cache hit: return cached result (within TTL)
   c. If cache miss: proceed to execution
7. Query Executor selects backend connection:
   a. Read queries: route to read replica (prefer low-lag replica)
   b. Write queries: route to write master
   c. Transactional queries: use existing transaction connection
8. Executor sends query to backend via Bolt protocol (Neo4j)
9. Backend executes query, returns results as records
10. Query Executor transforms results:
    a. Removes unauthorized properties
    b. Truncates results to page size
    c. Formats into response protobuf
11. Results cached (if cacheable)
12. Metrics recorded: query latency, rows returned, cache status
13. Response returned to agent

#### 4.2.2 Path Finding Flow
1. Agent sends PathFindRequest with start_id, end_id, max_depth, edge_types
2. Permission check: agent must have read access on start and end nodes and all traversable edge types
3. Query Optimizer selects algorithm:
   a. max_depth <= 3: BFS bidirectional search
   b. max_depth <= 10: BFS with pruning + A* heuristic
   c. max_depth > 10: Restricted BFS with max_paths limit
4. Optimizer adds index hints: use index on node(id), edge(source), edge(target)
5. Path query executed via Cypher: `MATCH p = shortestPath((start)-[*1..max_depth]->(end))`
6. Results processed:
   a. Remove cycles (if simple_path mode)
   b. Sort by path length or weight
   c. Limit to max_paths (default 10)
7. Return PathResult with ordered nodes and edges

#### 4.2.3 Subgraph Extraction Flow
1. Agent sends SubgraphExtractRequest with start_nodes, depth, edge_types, direction
2. Permission check per node and edge type
3. Query Planner determines extraction strategy:
   a. depth <= 2: Single Cypher query with variable-length traversal
   b. depth > 2: Multi-step BFS with visited set (avoid explosion)
4. Extraction executed:
   a. Start from each seed node
   b. Traverse edges of specified types to specified depth
   c. Collect all visited nodes and edges
   d. Deduplicate (graph may have multiple paths to same node)
5. Result prepared as node list + edge list
6. Materialized view option: if subgraph is for repeated use, create projection
7. Return SubgraphResponse

#### 4.2.4 Schema Migration Flow
1. Admin creates SchemaMigrationRequest with migration script
2. Schema Manager validates migration:
   a. Forward migration script
   b. Rollback script
   c. Checksum verification
3. Migration lock acquired (prevent concurrent migrations)
4. Schema version recorded as APPLYING
5. Migration executed in transaction:
   a. Create/drop indexes
   b. Create/drop constraints
   c. Add/remove node labels
   d. Add/remove edge types
   e. Add/remove properties
6. On success:
   a. Version → APPLIED
   b. Schema cache invalidated
   c. Query cache cleared (schema-dependent queries)
7. On failure:
   a. Migration → FAILED
   b. Automatic rollback if enabled
   c. Manual intervention if rollback fails

### 4.3 State Management

#### 4.3.1 Query State Machine
```
RECEIVED → AUTHENTICATING → AUTHENTICATED → PARSING → VALIDATING → 
VALIDATED → OPTIMIZING → OPTIMIZED → CACHE_CHECK → EXECUTING → 
TRANSFORMING → RESPONDING → COMPLETED
```

#### 4.3.2 Transaction State Machine
```
CREATED → ACTIVE → COMMITTING → COMMITTED
CREATED → ACTIVE → ROLLING_BACK → ROLLED_BACK
ACTIVE → TIMEOUT → ROLLING_BACK → ROLLED_BACK
ACTIVE → ERROR → ROLLING_BACK → ROLLED_BACK
```

#### 4.3.3 Consistency Model
- Read queries: Read-committed (default), configurable to repeatable-read
- Write queries: Read-committed with write locks
- Transactional: Serializable (configured by transaction isolation level)
- Read replicas: Eventually consistent (configurable acceptable lag)

#### 4.3.4 Cache Consistency
- Write-through: On write to graph, invalidate related cache entries
- TTL-based: Cache entries expire after configurable lifetime
- Version-based: Cache entries tagged with schema version; new schema → all cache invalidated

---

## 5. Communication Protocols, APIs, Events, Queues

### 5.1 Communication Protocols

| Protocol | Usage | Transport | Serialization |
|----------|-------|-----------|---------------|
| gRPC | Agent ↔ KG Access Service | HTTP/2 | Protobuf |
| REST | Admin operations | HTTPS | JSON |
| Neo4j Bolt | KG Access ↔ Neo4j | TCP | PackStream |
| Gremlin WebSocket | KG Access ↔ TinkerPop | WebSocket | JSON/GraphSON |
| Internal RPC | Cross-component | Unix Socket | Protobuf |

### 5.2 APIs

#### 5.2.1 Public gRPC API

```
service KnowledgeGraphService {
    // Query execution
    rpc ExecuteQuery(QueryRequest) returns (QueryResponse);
    rpc ExecuteExplain(ExplainRequest) returns (ExplainResponse);
    rpc ExecuteProfile(ProfileRequest) returns (ProfileResponse);
    
    // Node CRUD
    rpc CreateNode(CreateNodeRequest) returns (CreateNodeResponse);
    rpc GetNode(GetNodeRequest) returns (GetNodeResponse);
    rpc UpdateNode(UpdateNodeRequest) returns (UpdateNodeResponse);
    rpc DeleteNode(DeleteNodeRequest) returns (DeleteNodeResponse);
    rpc BatchCreateNodes(BatchCreateNodesRequest) returns (BatchCreateNodesResponse);
    
    // Edge CRUD
    rpc CreateEdge(CreateEdgeRequest) returns (CreateEdgeResponse);
    rpc GetEdge(GetEdgeRequest) returns (GetEdgeResponse);
    rpc UpdateEdge(UpdateEdgeRequest) returns (UpdateEdgeResponse);
    rpc DeleteEdge(DeleteEdgeRequest) returns (DeleteEdgeResponse);
    rpc BatchCreateEdges(BatchCreateEdgesRequest) returns (BatchCreateEdgesResponse);
    
    // Graph traversal
    rpc GetNeighbors(GetNeighborsRequest) returns (GetNeighborsResponse);
    rpc FindPath(FindPathRequest) returns (FindPathResponse);
    rpc FindAllPaths(FindAllPathsRequest) returns (FindAllPathsResponse);
    rpc ExtractSubgraph(ExtractSubgraphRequest) returns (ExtractSubgraphResponse);
    
    // Transaction management
    rpc BeginTransaction(BeginTransactionRequest) returns (BeginTransactionResponse);
    rpc CommitTransaction(CommitTransactionRequest) returns (CommitTransactionResponse);
    rpc RollbackTransaction(RollbackTransactionRequest) returns (RollbackTransactionResponse);
    
    // Subgraph/projection management
    rpc CreateProjection(CreateProjectionRequest) returns (CreateProjectionResponse);
    rpc GetProjection(GetProjectionRequest) returns (GetProjectionResponse);
    rpc DeleteProjection(DeleteProjectionRequest) returns (DeleteProjectionResponse);
    rpc RefreshProjection(RefreshProjectionRequest) returns (RefreshProjectionResponse);
    rpc ListProjections(ListProjectionsRequest) returns (ListProjectionsResponse);
    
    // Schema management (admin)
    rpc GetSchema(GetSchemaRequest) returns (GetSchemaResponse);
    rpc ApplyMigration(ApplyMigrationRequest) returns (ApplyMigrationResponse);
    rpc ListMigrations(ListMigrationsRequest) returns (ListMigrationsResponse);
    rpc GetIndexRecommendations(IndexRecommendationsRequest) returns (IndexRecommendationsResponse);
    
    // Admin
    rpc GetMetrics(GetKGKMetricsRequest) returns (GetKGKMetricsResponse);
    rpc GetQueryStats(GetQueryStatsRequest) returns (GetQueryStatsResponse);
    rpc ClearCache(ClearCacheRequest) returns (ClearCacheResponse);
    rpc GetGraphStats(GraphStatsRequest) returns (GraphStatsResponse);
}
```

#### 5.2.2 REST Admin API

```
GET    /api/v1/kg/schema                  - Get graph schema
POST   /api/v1/kg/schema/migrate          - Apply schema migration
GET    /api/v1/kg/schema/migrations       - List migrations
GET    /api/v1/kg/schema/indexes          - List indexes
POST   /api/v1/kg/schema/indexes          - Create index
DELETE /api/v1/kg/schema/indexes/{name}   - Drop index
GET    /api/v1/kg/projections             - List projections
POST   /api/v1/kg/projections             - Create projection
GET    /api/v1/kg/projections/{id}        - Get projection
DELETE /api/v1/kg/projections/{id}        - Delete projection
POST   /api/v1/kg/projections/{id}/refresh - Refresh projection
GET    /api/v1/kg/stats                   - Graph statistics
GET    /api/v1/kg/query-stats             - Query statistics
POST   /api/v1/kg/cache/clear             - Clear cache
GET    /api/v1/kg/health                  - Health check
GET    /api/v1/kg/index-recommendations   - Index recommendations
```

### 5.3 Events

```
GraphNodeCreatedEvent {
    event_id: UUID
    event_type: "graph.node.created"
    timestamp: Timestamp
    node_id: UUID
    labels: List<String>
    tenant_id: String
    created_by: AgentID
}

GraphNodeUpdatedEvent {
    event_id: UUID
    event_type: "graph.node.updated"
    timestamp: Timestamp
    node_id: UUID
    changed_properties: List<String>
    old_version: uint64
    new_version: uint64
}

GraphEdgeCreatedEvent {
    event_id: UUID
    event_type: "graph.edge.created"
    timestamp: Timestamp
    edge_id: UUID
    edge_type: String
    source_node_id: UUID
    target_node_id: UUID
    tenant_id: String
}

GraphQueryExecutedEvent {
    event_id: UUID
    event_type: "graph.query.executed"
    timestamp: Timestamp
    query_hash: String
    query_language: QueryLanguage
    execution_time_ms: float64
    rows_returned: uint32
    cache_hit: bool
    query_cost: QueryCost
    agent_id: AgentID
    tenant_id: String
}

GraphSchemaChangedEvent {
    event_id: UUID
    event_type: "graph.schema.changed"
    timestamp: Timestamp
    migration_id: UUID
    version_number: uint32
    change_summary: String
    applied_by: String
}
```

### 5.4 Queues

| Queue | Purpose | Backend |
|-------|---------|---------|
| graph.query.async | Async query execution | Kafka |
| graph.node.batch | Batch node creation | Kafka |
| graph.edge.batch | Batch edge creation | Kafka |
| graph.projection.refresh | Projection refresh jobs | NATS |
| graph.schema.migration | Schema migration queue | NATS |
| graph.cache.invalidate | Cache invalidation events | NATS |
| graph.metrics | Metrics aggregation | Kafka |
| graph.replication.monitor | Replica lag monitoring | Internal |

---

## 6. Caching, Memory, Persistence

### 6.1 Caching Strategy

#### 6.1.1 Query Result Cache
- Key: hash(query_string + parameters + agent_permissions + schema_version)
- Value: serialized query result
- TTL: Configurable per query type (default: node lookup 300s, traversal 30s, path 60s)
- Size: Configurable (default 100,000 entries)
- Eviction: LRU with query priority override
- Invalidation: On write to affected nodes/edges/types
- Cache warming: Pre-cache frequent queries on service start

#### 6.1.2 Node Cache (Hot Node Cache)
- Individual node objects cached in memory
- Write-through: updates to node invalidate cache entry
- TTL: Configurable (default 300 seconds)
- Size: Configurable (default 50,000 nodes)
- Used for: frequent node lookups by ID

#### 6.1.3 Schema Cache
- Graph schema (labels, edge types, properties, indexes, constraints)
- Invalidated on schema migration
- Stale-while-revalidate: Serve stale schema during revalidation
- TTL: 60 seconds (stale), immediate revalidation on write

#### 6.1.4 Metadata Cache
- Index statistics and cardinality estimates
- Used for query optimization decisions
- Refresh interval: 5 minutes or on explicit trigger

### 6.2 Persistence

#### 6.2.1 Graph Database Persistence
- Neo4j stores nodes, edges, properties, indexes, and constraints
- Transaction log for crash recovery
- Periodic store snapshots (checkpoints)
- Logical backup via Neo4j dump

#### 6.2.2 Cache Persistence
- Query cache: in-memory only (L2/L3 Memory System for larger caches)
- Node cache: in-memory with optional spill to L3 shared memory
- Schema cache: in-memory, reloaded from DB on service start

#### 6.2.3 Backup Strategy
- Daily: Neo4j full database dump
- Hourly: Transaction log backup
- Continuous: WAL streaming to backup location
- Snapshots: To L5 archival storage (S3-compatible)
- Retention: 30 daily, 12 monthly, 7 yearly

---

## 7. Validation, Retry Logic, Error Recovery

### 7.1 Validation

#### 7.1.1 Query Validation
- Syntax validation for all supported query languages
- Schema validation: referenced labels, types, properties must exist
- Permission validation: agent has access to queried graph elements
- Resource validation: query complexity within allowed limits
- Parameter validation: parameter types match schema definitions
- Injection prevention: query sanitization, parameterized queries only

#### 7.1.2 Resource Limits
```
Max query length: 10000 characters
Max query parameters: 100
Max result size: 10000 rows (default, configurable)
Max traversal depth: 20 hops
Max path results: 100
Max subgraph nodes: 100000
Max concurrent queries per agent: 10
Max query execution time: 30 seconds
```
```

### 7.2 Retry Logic

#### 7.2.1 Retry Configuration
```
GraphRetryConfig {
    read_retry: RetryPolicy {
        max_attempts: 3
        base_delay_ms: 50
        max_delay_ms: 1000
        multiplier: 2.0
        retryable_errors: [READ_REPLICA_LAG, CONNECTION_TIMEOUT, READ_TIMEOUT, NODE_NOT_FOUND]
    },
    write_retry: RetryPolicy {
        max_attempts: 2
        base_delay_ms: 200
        max_delay_ms: 2000
        multiplier: 2.0
        retryable_errors: [DEADLOCK_DETECTED, WRITE_CONFLICT, MASTER_NOT_AVAILABLE]
    },
    transaction_retry: RetryPolicy {
        max_attempts: 3
        base_delay_ms: 500
        max_delay_ms: 5000
        multiplier: 2.0
        retryable_errors: [SERIALIZATION_FAILURE, DEADLOCK_DETECTED]
    }
}
```

### 7.3 Error Recovery

#### 7.3.1 Neo4j Connection Failure
1. Connection pool detects connection failure
2. Mark connection as BROKEN, remove from pool
3. Attempt reconnection with exponential backoff
4. Read requests reassigned to other replicas
5. Write requests queued if master is unavailable
6. When connection restored: add back to pool, verify health
7. Drain write queue to recovered connection

#### 7.3.2 Neo4j Primary Failure
1. Detected via connection pool health checks
2. Promote healthy replica to primary (via Neo4j clustering)
3. Update write router with new primary address
4. Repoint write connections to new primary
5. Verify new primary has caught up with replication log
6. Continue normal operations
7. Repair old primary when recovered, add as replica

#### 7.3.3 Deadlock Recovery
1. Neo4j detects deadlock, selects victim transaction
2. Victim transaction rolled back with DEADLOCK_DETECTED error
3. KG Access Service catches deadlock error
4. Retry transaction with exponential backoff + random jitter
5. If retry succeeds: commit, return success
6. If retry fails after max attempts: return error to agent
7. Agent may retry at application level

#### 7.3.4 Replica Lag Recovery
1. Replica Monitor detects excessive lag (> threshold)
2. Remove replica from read routing pool
3. Flag replica for catch-up monitoring
4. Read traffic redistributed to healthy replicas
5. When replica lag drops below threshold:
   a. Warm replica with cache data
   b. Add back to routing pool
   c. Resume traffic distribution

---

## 8. Security, Authentication, Authorization, Permissions

### 8.1 Security Architecture

#### 8.1.1 Network Security
- mTLS for all gRPC communications
- Neo4j Bolt over TLS
- Admin API over HTTPS with client certificates
- Network policies restrict graph DB access to KG Access Service only

#### 8.1.2 Data Security
- Node/edge properties marked as SENSITIVE are encrypted at rest
- Encryption via Neo4j native encryption or application-level
- Access to encrypted properties requires explicit permission

#### 8.1.3 Audit Trail
- All graph mutations (node/edge create, update, delete) are audited
- Query execution logged for sensitive subgraphs
- Schema changes fully audited with before/after state

### 8.2 Authentication
- Agent JWT validation on every request
- Admin API requires multi-factor authentication
- Service-to-service mTLS with SPIFFE workload identities

### 8.3 Authorization

#### 8.3.1 Permission Levels
```
kg:node:read:{label_pattern}:{property_pattern}
kg:node:write:{label_pattern}
kg:node:delete:{label_pattern}
kg:edge:read:{type_pattern}
kg:edge:write:{type_pattern}
kg:edge:delete:{type_pattern}
kg:subgraph:read:{subgraph_id}
kg:subgraph:write:{subgraph_id}
kg:schema:read
kg:schema:write
kg:admin
```

#### 8.3.2 RBAC Roles
- KG_READER: Read nodes and edges in assigned subgraphs/labels
- KG_WRITER: Write nodes and edges in assigned subgraphs/labels
- KG_ADMIN: Schema management, full graph access
- KG_AUDITOR: Read-only access to audit logs and query history

#### 8.3.3 Fine-Grained Permissions
- Per-label read/write/delete
- Per-edge-type read/write/delete
- Per-property access (some properties may be restricted)
- Per-subgraph access
- Row-level security via property filters

### 8.4 Permissions

#### 8.4.1 Property-Level Access
```
Node type: "Ruling"
Properties:
  - text: PUBLIC (any KG_READER can read)
  - source_reference: PUBLIC
  - internal_grade: CONFIDENTIAL (only KG_ADMIN can read)
  - author_notes: PRIVATE (only owner can read)
```

#### 8.4.2 Subgraph Permissions
- Agents granted access to specific named subgraphs
- Subgraph access does not imply access to underlying full graph
- Materialized views may expose subset of properties
- Permission inheritance: agent → team → department → subgraph

---

## 9. Monitoring, Logging, Metrics, Tracing

### 9.1 Metrics

#### 9.1.1 Core Metrics
```
# Query metrics
aios_kg_queries_total{language="cypher", status="success"} 350000
aios_kg_queries_total{language="cypher", status="error"} 1200
aios_kg_query_duration_seconds{type="node_lookup", quantile="0.5"} 0.002
aios_kg_query_duration_seconds{type="node_lookup", quantile="0.99"} 0.010
aios_kg_query_duration_seconds{type="traversal", quantile="0.5"} 0.015
aios_kg_query_duration_seconds{type="traversal", quantile="0.99"} 0.200
aios_kg_query_duration_seconds{type="path_find", quantile="0.5"} 0.050
aios_kg_query_duration_seconds{type="path_find", quantile="0.99"} 0.500
aios_kg_query_rows_returned_total 15000000

# Cache metrics
aios_kg_cache_hit_rate{cache_type="query"} 0.55
aios_kg_cache_hit_rate{cache_type="node"} 0.78
aios_kg_cache_size{cache_type="query"} 25000
aios_kg_cache_eviction_total{cache_type="query"} 5000

# Connection pool metrics
aios_kg_connection_pool_size{type="read"} 20
aios_kg_connection_pool_size{type="write"} 5
aios_kg_connection_active{type="read"} 12
aios_kg_connection_waiting{type="read"} 0
aios_kg_connection_timeout_total 3

# Schema metrics
aios_kg_schema_version 42
aios_kg_index_count 85
aios_kg_unused_index_count 3
aios_kg_node_count{label="Ruling"} 50000
aios_kg_node_count{label="Scholar"} 1200
aios_kg_edge_count{type="REFERENCES"} 150000
aios_kg_edge_count{type="DERIVES_FROM"} 45000

# Replica metrics
aios_kg_replica_lag_seconds{replica="r1"} 0.5
aios_kg_replica_lag_seconds{replica="r2"} 1.2
aios_kg_replica_available 3
aios_kg_replica_behind_master 0

# Error metrics
aios_kg_query_errors_total{error_type="syntax"} 45
aios_kg_query_errors_total{error_type="timeout"} 12
aios_kg_query_errors_total{error_type="permission"} 8
aios_kg_query_errors_total{error_type="deadlock"} 2
```

### 9.2 Logging

#### 9.2.1 Log Levels
- ERROR: Query failures, connection failures, schema migration failures, permission violations
- WARN: Replica lag, high query latency, cache thrashing, index recommendations
- INFO: Query execution (sampled), schema changes, connection pool events
- DEBUG: Individual query plans, cache operations, connection acquisition
- TRACE: Full query text (off by default, enable for debugging)

#### 9.2.2 Audit Events
- Node/edge creation, modification, deletion (who, what, timestamp)
- Schema migrations (before/after state)
- Permission changes
- Subgraph/projection creation and deletion
- Cross-subgraph access attempts

### 9.3 Tracing

#### 9.3.1 Trace Spans
```
GraphQuery → Parse → Validate → Optimize → CacheCheck → BackendExecute → Transform → Respond
NodeCreate → Validate → Permission→ BackendWrite → IndexUpdate → CacheInvalidate → Respond
PathFind   → Validate → Optimize → BackendExecute → ProcessPaths → Filter → Respond
Migration  → Validate → LockAcquire → Backup → Execute → Verify → LockRelease → Respond
```

---

## 10. Scalability, Performance, Optimization

### 10.1 Scalability

#### 10.1.1 Horizontal Scaling
- KG Access Service: Stateless, scales behind load balancer
- Neo4j: Cluster with read replicas, leader election
- Connection pool management distributes across replicas
- Query cache per KG Access Service instance (distributed cache for larger deployments)

#### 10.1.2 Performance Targets
```
Query Type           Dataset        P50 Latency    P99 Latency    Throughput (per node)
Node lookup by ID    50M nodes      2 ms           10 ms          10,000 QPS
Node lookup by property 50M nodes  10 ms          100 ms         2,000 QPS
1-hop traversal      50M nodes      5 ms           30 ms          5,000 QPS
2-hop traversal      50M nodes      15 ms          100 ms         2,000 QPS
3-hop traversal      50M nodes      50 ms          500 ms         500 QPS
Shortest path        50M nodes      100 ms         2,000 ms       100 QPS
Subgraph extract (depth 2) 50M     200 ms         1,000 ms       50 QPS
```

### 10.2 Optimization Techniques

#### 10.2.1 Query Optimization
- Predicate pushdown: evaluate filters as early as possible in traversal
- Index selection: automatically select the most selective index for each predicate
- Join ordering: reorder graph patterns to minimize intermediate results
- Fan-out limiting: cap the number of traversals from any node (default 1000)
- Early termination: stop traversal when result limit reached
- Result streaming: return partial results as they become available

#### 10.2.2 Index Strategy
- Index all node IDs and external IDs
- Index frequently filtered properties (by query pattern analysis)
- Composite indexes for multi-property lookups
- Full-text indexes for text search on content properties
- Range indexes for numeric/date range queries
- Point indexes for geospatial queries
- Unused index detection: drop indexes with no usage in 30 days

#### 10.2.3 Query Plan Cache
- Executed query plans cached for reuse (plan cache separate from result cache)
- Plan cache key: query shape (parameterized) + schema version
- Eviction: LRU, max 10,000 plans
- Invalidation: on schema change or statistics update

#### 10.2.4 Read/Write Splitting
- Read queries: distributed across replicas with load balancing
- Write queries: directed to master only
- Transactional read-your-writes: read from master within transaction boundary
- Stale read tolerance: configurable per query (agents can specify acceptable lag)

---

## 11. Testing Strategy, Risk Analysis

### 11.1 Testing Strategy

#### 11.1.1 Unit Testing
- Query parser for Cypher, SPARQL, and AIOS GQL
- Query optimizer: predicate pushdown, index selection, fan-out limiting
- Permission evaluator: role resolution, permission matching
- Cache key generation and lookup
- Schema validation logic
- Connection pool selection and routing

#### 11.1.2 Integration Testing
- Full query lifecycle: parse → validate → optimize → execute → respond
- Node/edge CRUD operations with permission validation
- Path finding with correct shortest path results
- Subgraph extraction with correct node/edge counts
- Transaction commit and rollback
- Schema migration forward and rollback
- Cache invalidation on writes

#### 11.1.3 Performance Testing
- Query latency at various dataset sizes (1K, 10K, 100K, 1M, 10M, 50M nodes)
- Concurrent query throughput (10, 50, 200, 1000 concurrent agents)
- Index build performance at scale
- Path finding performance with varying depth and graph density
- Cache hit rate under production-like access patterns

#### 11.1.4 Chaos Testing
- Neo4j primary failure during write operations
- Read replica failure during read load
- Connection pool exhaustion
- Slow query injection (simulate complex graph traversal)
- Network latency between KG Access Service and Neo4j

### 11.2 Risk Analysis

| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|------------|
| Complex query causing Neo4j OOM | Medium | High | Query complexity limits, circuit breaker, query timeout |
| Read replica serving stale data | Medium | Medium | Lag monitoring, read-your-writes for critical queries |
| Schema migration corruption | Low | Critical | Backup before migration, rollback script, test in staging |
| Index bloat slowing writes | Medium | Medium | Index usage monitoring, unused index cleanup |
| Permission bypass via query injection | Low | Critical | Parameterized queries only, query whitelist for dangerous patterns |
| Hot node causing contention | Medium | Medium | Lock analysis, suggest schema changes |
| Cache poisoning from stale data | Low | Medium | TTL-based invalidation, write-through updates |
| Full graph traversal DoS | Low | High | Fan-out limits, query cost estimation, rate limiting |

---

## 12. Failure Scenarios, Recovery Procedures

### 12.1 Failure Scenarios

#### 12.1.1 Neo4j Primary Crash
**Scenario**: Neo4j primary node fails during peak load.

**Symptoms**: Write operations fail, read replicas return stale data.

**Impact**: Write unavailability for 30-120 seconds during failover.

**Recovery**:
1. Neo4j cluster detects primary failure (heartbeat timeout)
2. Cluster triggers leader election (typically 7-15 seconds for a 3-node cluster)
3. New primary selected from available replicas
4. New primary replays any unapplied transaction log entries
5. KG Access Service detects connection failure to old primary
6. Write router updates to new primary address
7. Write connections re-established to new primary
8. Pending write queue drained to new primary
9. Normal operations resume

#### 12.1.2 Query Timeout (Runaway Query)
**Scenario**: A complex traversal query takes too long, consuming resources.

**Symptoms**: High CPU on Neo4j, other queries slow or timing out.

**Impact**: Performance degradation for all users of that Neo4j instance.

**Recovery**:
1. Query exceeded execution timeout (30 seconds default)
2. Neo4j kills the query, releases resources
3. KG Access Service returns timeout error to agent
4. Query logged for analysis
5. If recurrent: add query to blocklist, notify admin
6. Query optimizer may suggest index to prevent recurrence

#### 12.1.3 Connection Pool Exhaustion
**Scenario**: All connections in pool are in use, new requests queue.

**Symptoms**: Query latency increases, eventually requests timeout.

**Impact**: Service degradation for all agents.

**Recovery**:
1. Connection pool monitor alerts at 80% utilization
2. Auto-scale KG Access Service instances (horizontal pod autoscaling)
3. Increase connection pool size (if Neo4j can handle more connections)
4. Reduce connection timeout to fail fast on stuck connections
5. If pool fully exhausted: return RETRY_AFTER response to clients

#### 12.1.4 Schema Migration Failure
**Scenario**: Schema migration script fails mid-execution.

**Symptoms**: Inconsistent schema state, queries may fail.

**Impact**: Graph queries may return errors or unexpected results.

**Recovery**:
1. Migration marked as FAILED in schema version table
2. Automatic rollback triggered (if rollback script exists)
3. If rollback succeeds: database returns to previous schema version
4. If rollback fails: manual intervention required
5. Notify admin with full diagnostics
6. Cache fully invalidated regardless of outcome

### 12.2 Recovery Procedures

#### 12.2.1 Full Graph Restore
1. Identify latest backup (Neo4j dump) and transaction logs
2. Restore graph DB from dump file
3. Replay transaction logs from backup time to latest checkpoint
4. Verify: node count, edge count, latest migration version
5. Run integrity constraints
6. Warm query cache with frequent queries
7. Return service to active

#### 12.2.2 Subgraph Recovery
1. Identify affected subgraph (corrupted or stale)
2. If materialized: rebuild from base query
3. If virtual: check permissions and base graph health
4. If base graph issue: restore base graph first
5. Verify subgraph consistency
6. Clear subgraph-related cache entries

---

## 13. Operational Limits, Maintenance

### 13.1 Operational Limits

| Parameter | Limit | Action on Exceed |
|-----------|-------|------------------|
| Max nodes in graph | 500 million | Plan database sharding or archive |
| Max edges in graph | 2 billion | Plan database sharding or archive |
| Max properties per node | 500 | Reject writes exceeding limit |
| Max properties per edge | 100 | Reject writes exceeding limit |
| Max label count per node | 20 | Reject writes exceeding limit |
| Max query execution time | 30 seconds | Kill query, return timeout |
| Max query result rows | 100,000 | Truncate result |
| Max traversal depth | 20 hops | Reject query |
| Max concurrent queries per agent | 10 | Queue or throttle |
| Max concurrent transactions per agent | 5 | Block new transaction start |
| Max transaction timeout | 300 seconds | Rollback transaction |
| Max connection pool size | 100 per service instance | Alert, auto-scale |

### 13.2 Maintenance Procedures

#### 13.2.1 Routine Maintenance
- Hourly: Replica lag check, query performance review
- Daily: Index usage analysis, drop unused indexes
- Weekly: Query statistics review, identify optimization opportunities
- Monthly: Schema review, backup test restore
- Quarterly: Performance benchmark, capacity planning
- Yearly: Full disaster recovery drill

#### 13.2.2 Index Maintenance
1. Review query patterns and top slow queries
2. Analyze index usage statistics
3. Drop indexes with < 10 uses in 30 days
4. Create indexes for frequently filtered properties
5. Monitor index creation impact on write performance
6. Schedule index creation during low-traffic periods

#### 13.2.3 Data Archival
1. Identify old/inactive subgraphs (no queries in 90 days)
2. Create archival snapshot to L5
3. Remove subgraph from active graph database
4. Update schema if archival affects labels/types
5. Maintain read-only access for archived data via L5

---

## 14. Future Improvements, Examples

### 14.1 Future Improvements

#### 14.1.1 Planned Enhancements
- Native graph algorithms library (PageRank, community detection, centrality)
- Graph neural network integration for entity resolution and link prediction
- Temporal graph support (query graph state at specific point in time)
- Streaming graph updates with real-time projection refresh
- Natural language to graph query translation (LLM-powered)
- Automated schema inference from new data
- Graph visualization service for agent-facing graph exploration
- Cross-graph federation (query multiple AIOS KG instances)
- Graph versioning and branching (experimental subgraphs)
- Query cost estimation before execution

#### 14.1.2 Research Areas
- Property graph → RDF bridge for interop
- Distributed graph processing (Apache Spark GraphX integration)
- Incremental graph view maintenance
- Learned query optimizers
- Graph data fabric across heterogeneous sources

### 14.2 Examples

#### 14.2.1 Agent Querying "Rulings About Prayer Times" Subgraph

```
Scenario: An Islamic jurisprudence research agent needs all rulings about prayer times.

Request:
```
POST /api/v1/kg/query
{
    "language": "cypher",
    "query": "MATCH (r:Ruling)-[:ABOUT]->(t:Topic {name: 'Prayer Times'}) 
              WHERE r.madhhab = 'Hanafi' 
              RETURN r.text, r.source_reference, r.grade
              ORDER BY r.priority DESC
              LIMIT 20",
    "parameters": {}
}
```

Flow:
1. Agent authenticated, JWT contains permissions for "Ruling" node read access
2. Query parsed and validated against schema
3. Optimizer identifies:
   - Index on Topic(name) → selective (only 50 "Prayer Times" topics)
   - Index on Ruling(madhhab) → moderately selective
   - Query plan: Index lookup Topic(name), traverse ABOUT edges, filter madhhab
4. Query executed against Neo4j read replica (read-only query)
5. Results returned: 20 rulings with text, source, grade
6. Results cached for 300 seconds (TTL for topic-specific queries)
7. Metrics: 5ms query time, cache miss, 20 rows returned

Result sample:
```
[
  {
    "text": "The time for Fajr prayer begins at true dawn (al-fajr al-sadiq)...",
    "source_reference": "al-Hidayah, Kitab al-Salah, Chapter 1",
    "grade": "MU'TAMAD (Relied upon)"
  },
  {
    "text": "Asr prayer time begins when the shadow of an object equals its height...",
    "source_reference": "Radd al-Muhtar, Vol. 2, p. 45",
    "grade": "MU'TAMAD (Relied upon)"
  },
  ...
]
```

#### 14.2.2 Multi-Hop Traversal for Source Verification

```
Scenario: Verify the chain of transmission for a ruling.

Query:
```
MATCH (r:Ruling {id: "ruling-0427"})-[:DERIVES_FROM*1..5]->(s:Source)
RETURN r.text, collect(DISTINCT s.title) AS source_chain
```

Flow:
1. Variable-length path traversal (1-5 hops)
2. Optimizer uses index on Ruling(id) for starting node
3. Fan-out limit of 1000 per hop prevents explosion
4. BFS traversal with visited set
5. Results: ruling text and all sources in chain
6. Cache TTL: 600 seconds (source chains rarely change)

#### 14.2.3 Graph Projection for Department-Specific View

```
Scenario: Hanafi fiqh department only needs Hanafi rulings.

Projection definition:
```
POST /api/v1/kg/projections
{
    "name": "hanafi_rulings_only",
    "base_query": "MATCH (r:Ruling) WHERE r.madhhab IN ['Hanafi', 'Hanbali'] 
                   OPTIONAL MATCH (r)-[e]-(n) 
                   RETURN r, e, n",
    "refresh_policy": {
        "type": "SCHEDULED",
        "interval_hours": 24
    },
    "permissions": {
        "read_roles": ["DEPARTMENT_HANAFI"],
        "write_roles": ["DEPARTMENT_HANAFI_ADMIN"]
    }
}
```

Usage:
- Department agents only see Hanafi/Hanbali rulings via this projection
- Query performance improved (smaller graph to search)
- Write isolation: changes in projection may or may not propagate to base graph
- Refresh: daily or on demand (for real-time updates, use virtual projection)

#### 14.2.4 Cross-KG and Vector Hybrid Search

```
Scenario: Find rulings about prayer times AND semantically similar content.

Flow:
1. Graph query: finds structural knowledge graph results (prayer time rulings)
2. Vector search: finds semantically similar content in vector memory
3. Results fused and ranked

Graph query returns:
- Exact topic matches (rulings about "Prayer Times")
- Related rulings via IS_A edge (rulings about "Fajr", "Dhuhr", etc.)
- Authoritative source references

Vector search returns:
- Semantically similar rulings (different wording, same meaning)
- Related content that may not be explicitly linked in KG
- Contextual explanations

Combined result:
- High confidence matches from both sources
- Cross-referenced for verification
- Presented to agent with source annotation
```

---

## 15. Acceptance Criteria, Definition of Done

### 15.1 Acceptance Criteria

#### 15.1.1 Functional Acceptance Criteria
1. Execute Cypher queries and return correct results
2. Execute SPARQL queries and return correct results
3. Execute AIOS GQL queries and return correct results
4. Node CRUD operations work correctly with permission enforcement
5. Edge CRUD operations work correctly with permission enforcement
6. Path finding returns correct shortest path between two nodes
7. Subgraph extraction correctly extracts connected subgraph
8. Transactions commit and rollback correctly
9. Schema migration applies forward and rolls back correctly
10. Query cache serves valid cached queries within TTL

#### 15.1.2 Non-Functional Acceptance Criteria
1. Node lookup p99 latency < 10ms (50M node dataset)
2. 1-hop traversal p99 latency < 30ms (50M node dataset)
3. Path finding (5 hops) p99 latency < 2 seconds (50M node dataset)
4. Write throughput > 1000 nodes/second
5. Read replica lag < 2 seconds average
6. Query cache hit rate > 50%
7. Permission enforcement: zero bypasses in penetration testing
8. Connection pool utilization < 80% under normal load
9. Zero data loss on single Neo4j node failure
10. Schema migration rollback completes in < 5 minutes

### 15.2 Definition of Done

Knowledge Graph Access feature or component is "Done" when:

1. **Specification**: All sections addressed
2. **Implementation**: All APIs, query languages, and graph operations implemented
3. **Unit Tests**: > 90% line coverage for query parsing, optimization, permissions
4. **Integration Tests**: Full query lifecycle tested with all supported query languages
5. **Performance Tests**: Latency and throughput meet specified targets at scale
6. **Security Review**: Permission model penetration tested, no bypasses found
7. **Documentation**: API reference, query language guide, admin runbook complete
8. **Monitoring**: All metrics, logs, and traces implemented
9. **Alerting**: Critical conditions (high latency, replica lag, migration failure) alerted
10. **Chaos Tests**: Survive Neo4j primary failure without data loss
11. **Code Review**: All changes reviewed and approved
12. **Backward Compatibility**: API changes are backward compatible
13. **Deployment**: Kubernetes manifests and deployment procedures documented
14. **Rollback**: Rollback procedures documented and tested
15. **SLA**: Meets defined availability (99.9%) and performance targets

---

*End of AIOS Knowledge Graph Access Specification Document v1.0*
