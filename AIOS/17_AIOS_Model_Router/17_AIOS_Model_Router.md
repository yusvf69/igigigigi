# AIOS Model Router — Engineering Specification v2.0

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
The AIOS Model Router is the intelligent traffic director for all model inference requests flowing through the AIOS ecosystem. It sits at the intersection of the Model Manager, the execution engine, and the external model providers (OpenAI, Anthropic, Google, open-source self-hosted models, and more). The Model Router is responsible for accepting a model inference request — which includes the model identifier, input payload, configuration parameters, and quality-of-service constraints — and routing it to the most appropriate model backend in real time. The routing decision considers a multi-dimensional scoring function that evaluates model capability, current latency, cost budget, availability, throughput, geographic proximity, and user-defined policies. The Model Router implements provider failover, load shedding, request queuing, and concurrency throttling to ensure that AIOS always delivers the best possible inference experience within the constraints of the operating environment.

### 1.2 Mission
To provide a universal, policy-driven, and observable model routing layer that abstracts away the complexity of multi-provider, multi-model AI inference. The Model Router ensures that every inference request is routed to the optimal backend based on a configurable strategy (e.g., lowest latency, lowest cost, highest capability, fallback order), that backends are continuously health-checked and removed from rotation when degraded, that requests are retried transparently on failure, and that all routing decisions are fully auditable. The Model Router aims to achieve sub-millisecond routing overhead, 99.99% routing decision availability, and zero silent failures (every failure is explicitly surfaced with a structured error).

### 1.3 Responsibilities
- Accepting inference requests from the Model Manager, Workflow Engine, Agent Manager, and direct external callers via the API gateway
- Maintaining a real-time registry of all available model backends, including their endpoints, capabilities, authentication credentials, rate limits, current health, and pricing
- Executing a routing strategy selector that chooses a backend based on configurable policies: latency-optimized, cost-optimized, capability-priority, random, round-robin, weighted-distribution, or custom user-defined strategies
- Performing pre-routing validation of requests: model existence, parameter schema compliance, token budget adherence, content safety checks
- Managing a circuit breaker per backend endpoint: health checking with configurable intervals, failure thresholds, cooldown periods, and half-open probe intervals
- Implementing request-level retry logic with configurable retry count, exponential backoff, jitter, and backend exclusion after repeated failures
- Enforcing rate limits at both the global level (total requests per second) and per-backend level (matching provider API limits)
- Executing cost-aware routing that tracks spending per request and per session, with configurable budgets that trigger alerts or block routing
- Managing request queues for overload scenarios: priority queuing with per-tenant scheduling, request timeouts, and queue depth limits
- Producing structured audit logs for every routing decision: input hash, selected backend, decision factors, latency, cost incurred, success/failure
- Supporting request transformation/passthrough: convert incoming request format to the target backend's expected format (OpenAI-compatible, Anthropic, custom protocol)
- Providing a localization-aware routing hint layer: route requests to the geographically closest backend that supports the required model
- Exposing a health check endpoint that reports the status of all backends, queue depths, routing latency percentiles, and error rates

### 1.4 Non-Responsibilities
- The Model Router does NOT perform model inference itself; it routes to backends that perform inference
- The Model Router does NOT manage model lifecycle (deployment, loading, unloading) — that is the Model Manager's responsibility
- The Model Router does NOT manage model fine-tuning, training, or evaluation
- The Model Router does NOT persist inference results (that is the caller's responsibility)
- The Model Router does NOT authenticate users or tenants (that is handled by the Authentication system)
- The Model Router does NOT implement business logic for agent decision-making
- The Model Router does NOT manage API keys or secrets for backends (it reads them from the Secrets Vault)
- The Model Router does NOT handle prompt engineering, template rendering, or system message construction
- The Model Router does NOT implement content filtering or moderation policies (those are handled by the Validation system)

---

## 2. Architecture, Internal Components, External Components

### 2.1 Overall Architecture
The Model Router follows a layered pipeline architecture with a central route evaluation engine, a backend registry, a circuit breaker layer, a rate limiter, and a queue manager. All components are designed for concurrent access with lock-free reads and fine-grained write locking.

```
┌──────────────────────────────────────────────────────────────────────────────────────┐
│                              AIOS Model Router Service                                │
│                                                                                        │
│  ┌──────────────────────────────────────────────────────────────────────────┐          │
│  │                         Router API Layer (gRPC + REST)                    │          │
│  │  /api/v1/models/{model}/route  |  /api/v1/route  |  /api/v1/backends     │          │
│  └──────────────────────────────────────────────────────────────────────────┘          │
│                                      │                                                   │
│  ┌──────────────────────────────────────────────────────────────────────────┐          │
│  │          ┌─────────────────┐    ┌──────────────────┐    ┌──────────────┐ │          │
│  │          │ Request Parser  │    │ Route Evaluator  │    │ Strategy     │ │          │
│  │          │ & Validator     │───▶│ (Scoring Engine) │───▶│ Selector     │ │          │
│  │          └─────────────────┘    └──────────────────┘    └──────────────┘ │          │
│  │                                                                              │          │
│  │  ┌─────────────────┐    ┌──────────────────┐    ┌────────────────────────┐ │          │
│  │  │ Backend         │    │ Circuit Breaker  │    │ Cost Manager          │ │          │
│  │  │ Registry (live) │◀───│ Manager           │◀───│ (Budget Tracker)      │ │          │
│  │  └─────────────────┘    └──────────────────┘    └────────────────────────┘ │          │
│  │                                                                              │          │
│  │  ┌─────────────────┐    ┌──────────────────┐    ┌────────────────────────┐ │          │
│  │  │ Rate Limiter    │    │ Queue Manager    │    │ Request Transformer   │ │          │
│  │  │ (Token Bucket)  │───▶│ (Priority Queue) │───▶│ (Format Adapter)      │ │          │
│  │  └─────────────────┘    └──────────────────┘    └────────────────────────┘ │          │
│  └──────────────────────────────────────────────────────────────────────────┘          │
│                                    │                                                   │
│  ┌──────────────────────────────────────────────────────────────────────────┐          │
│  │                     Backend Proxy Layer                                   │          │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌─────────────┐  │          │
│  │  │ OpenAI   │ │ Anthropic│ │ Google   │ │ Self-    │ │ Other       │  │          │
│  │  │ Adapter  │ │ Adapter  │ │ Adapter  │ │ Hosted   │ │ Providers   │  │          │
│  │  └──────────┘ └──────────┘ └──────────┘ └──────────┘ └─────────────┘  │          │
│  └──────────────────────────────────────────────────────────────────────────┘          │
│                                    │                                                   │
│  ┌──────────────────────────────────────────────────────────────────────────┐          │
│  │                     Backend Pool (gRPC/HTTPS)                            │          │
│  │  [Provider A] [Provider B] [Self-Hosted GPU Cluster] [Edge Nodes]       │          │
│  └──────────────────────────────────────────────────────────────────────────┘          │
└──────────────────────────────────────────────────────────────────────────────────────┘
```

### 2.2 Internal Components

#### 2.2.1 Route Evaluator (Scoring Engine)
The core decision-making component. It receives a deserialized routing request and evaluates every eligible backend against a configurable scoring function. The evaluator maintains an in-memory scoring cache keyed by (model, strategy) to avoid recomputing scores when backends have not changed. On each evaluation cycle, it pulls the latest backend metrics from the Backend Registry and computes a composite score. The winning backend (highest score) is selected and passed downstream. The evaluator supports strategy plug-ins: new strategies can be registered at startup. Built-in strategies include weighted-random, lowest-latency, lowest-cost, capability-max, and failover-priority.

#### 2.2.2 Route Validator
Responsible for checking the incoming request for structural correctness before any routing computation begins. It validates model name existence in the registry, parameter schema compliance (temperature range, max_tokens bounds, top_p, etc.), required fields presence (e.g., messages array for chat, prompt for completion), content length checks against model context window, and authorization checks (does the caller have permission to use the requested model?). Returns structured error responses in case of validation failure without touching the routing engine.

#### 2.2.3 Backend Registry
A live in-memory data structure that holds the current state of all known backends. Each backend entry includes its endpoint URL, authentication method and credential reference, model compatibility list, current health state (HEALTHY, DEGRADED, UNHEALTHY), current latency P50 and P99, current error rate, last checked timestamp, rate limit tier, cost per token, capacity (max concurrent requests), and geo-tags (region, data center). The registry is seeded from the AIOS Configuration system on startup and updated via the Health Checker component. Writes require a write lock; reads are lock-free via atomic pointer swaps on the entire registry snapshot.

#### 2.2.4 Circuit Breaker Manager
Each backend has an associated circuit breaker with three states: CLOSED (normal operation), OPEN (requests are immediately rejected), HALF_OPEN (probing to see if the backend has recovered). The circuit breaker tracks a sliding window of failures. When the failure count exceeds a configurable threshold within the window, the breaker trips to OPEN. After a configurable cooldown period, it transitions to HALF_OPEN and allows a single probe request through. If the probe succeeds, the breaker resets to CLOSED; if it fails, it returns to OPEN with a longer cooldown. This pattern prevents cascading failures and allows automatic recovery. The breaker state is persisted to Redis for crash recovery.

#### 2.2.5 Rate Limiter
Implements a token bucket algorithm with per-backend and per-tenant dimensions. Each backend declares its rate limit capacity (tokens per second, burst size). Each tenant (or API key) has a configurable global rate limit and per-model sub-limits. The Rate Limiter checks before consuming a token and fails fast if the bucket is empty. Token replenishment runs on a background ticker. The rate limiter also supports "soft limits" that trigger a warning header without rejecting the request, allowing clients to self-throttle. All rate limit decisions are logged for auditing.

#### 2.2.6 Queue Manager
When all eligible backends are at capacity (rate limited) or the circuit breakers are open, the Queue Manager buffers the request in a priority queue. Each queue entry has a deadline (timeout). The Queue Manager runs a consumption loop that pops the highest-priority, earliest-deadline request and re-submits it to the Route Evaluator. Queue depth, average wait time, and drop rate are tracked as metrics. The queue is backed by a Redis list for persistence across restarts.

#### 2.2.7 Health Checker
A background component that periodically pings all registered backends. Each backend is checked at a configurable interval (default 10 seconds for cloud providers, 5 seconds for self-hosted). The health check sends a lightweight request (e.g., a minimal prompt or a dedicated health endpoint) and measures latency. Results update the Backend Registry and the Circuit Breaker Manager. If a backend fails three consecutive health checks, it is marked UNHEALTHY and removed from routing consideration. A succeeding health check in OPEN state triggers the HALF_OPEN probe.

#### 2.2.8 Request Transformer
Translates the internal AIOS canonical request format into the specific format expected by the selected backend. For example, an internal request with a `messages` array and `model` field is transformed into the OpenAI Chat Completions JSON format, or the Anthropic Messages format, or the Google Gemini format, or a raw completion prompt format for self-hosted models. The transformer also adapts response formats back to a canonical internal response format that downstream consumers expect.

#### 2.2.9 Cost Tracker
Tracks the cost incurred per request by multiplying the token count (input + output) of the response by the backend's per-token pricing. The cost tracker maintains running totals per model, per tenant, per session, and per time window (daily, monthly). It enforces cost budgets: if a tenant or session exceeds its budget, subsequent routing decisions either avoid expensive backends or reject the request entirely depending on policy. The cost tracker also logs every cost event for billing and reporting.

### 2.3 External Components
- **Model Manager**: Provides the list of available models and their lifecycle state
- **AIOS Configuration System**: Provides the Model Router's configuration (backends, strategies, thresholds)
- **Secrets Vault**: Provides API keys and authentication credentials for backend providers
- **Redis Cluster**: Used for rate limiter buckets, circuit breaker state, queue persistence, health check state
- **External Model Providers**: OpenAI, Anthropic, Google, AWS Bedrock, Azure OpenAI, self-hosted vLLM/TGI endpoints
- **Monitoring System (Prometheus/Grafana)**: Scrapes routing metrics
- **Logging System (ELK/Loki)**: Receives structured audit logs
- **Cost Management/Billing System**: Receives cost events for invoicing
- **Event System**: Emits routing decision events for downstream consumers (Analytics, Workflow)

---

## 3. Dependencies, Inputs, Outputs

### 3.1 Internal Dependencies
- Model Manager (model metadata, capability inference)
- Configuration System (runtime config, strategy definitions, backend definitions)
- Secrets Vault (credential retrieval)
- Event System (event emission)
- Monitoring System (metric emission)
- Logging System (audit logging)
- Cache System (scoring cache, backend metadata cache)

### 3.2 External Dependencies
- Redis Cluster (rate limiter state, circuit breaker state, queue persistence)
- DNS and HTTP resolution (backend connectivity)
- Time synchronization service (for accurate cost and latency metrics)
- Provider APIs (network connectivity, TLS)

### 3.3 Inputs

#### 3.3.1 Route Request
```json
{
  "request_id": "uuid",
  "tenant_id": "string",
  "user_id": "string",
  "model": "string (e.g., gpt-4, claude-3-opus)",
  "parameters": {
    "messages": [{"role": "user", "content": "..."}],
    "temperature": 0.7,
    "max_tokens": 1024,
    "top_p": 1.0,
    "frequency_penalty": 0.0,
    "presence_penalty": 0.0,
    "stop": ["\n"],
    "stream": false
  },
  "routing_policy": {
    "strategy": "latency|cost|capability|failover|custom",
    "allowed_backends": ["openai", "anthropic"],
    "excluded_backends": [],
    "max_cost_per_request": 0.05,
    "max_latency_ms": 5000,
    "priority": 1
  },
  "context": {
    "session_id": "uuid",
    "workflow_id": "uuid",
    "agent_id": "uuid",
    "retry_count": 0
  }
}
```

#### 3.3.2 Configuration Input
- Backend definitions (endpoints, auth, models, pricing, rate limits, region)
- Strategy definitions (scoring weights, priority ordering)
- Health check intervals and failure thresholds
- Rate limit configurations (global, per-tenant, per-backend)
- Budget definitions (per-tenant, per-model, per-time-window)

#### 3.3.3 Health Check Responses
- Lightweight model response or HTTP 200 from health endpoint
- Measured latency in milliseconds

### 3.4 Outputs

#### 3.4.1 Routing Decision
```json
{
  "request_id": "uuid",
  "selected_backend": "openai-eastus-01",
  "routing_decision": {
    "strategy_used": "lowest-latency",
    "score": 0.92,
    "candidate_backends": [
      {"id": "openai-eastus-01", "score": 0.92, "reason": "lowest latency"},
      {"id": "anthropic-westus-01", "score": 0.85, "reason": "second lowest cost"}
    ],
    "circuit_breaker_state": "CLOSED",
    "selected_capacity": 12
  },
  "request_metadata": {
    "routing_latency_us": 342,
    "queue_wait_ms": 0,
    "cost_estimate": 0.0032
  }
}
```

#### 3.3.3 Error Responses
```json
{
  "request_id": "uuid",
  "error": {
    "code": "ROUTER_NO_BACKEND_AVAILABLE",
    "message": "No backend is currently available for model 'gpt-4'. All 2 backends have open circuit breakers.",
    "retryable": true,
    "retry_after_ms": 5000,
    "details": {
      "total_backends": 2,
      "healthy_backends": 0,
      "circuit_breaker_open": ["openai-eastus-01", "openai-westus-01"]
    }
  }
}
```

#### 3.3.4 Monitoring Outputs
- Metrics (latency percentiles, error rates, queue depth, cost rate)
- Audit logs (every routing decision with full provenance)
- Events (ROUTING_DECISION_MADE, BACKEND_UNAVAILABLE, BACKEND_RECOVERED, COST_THRESHOLD_EXCEEDED, RATE_LIMIT_EXCEEDED)

---

## 4. Data Structures, Execution Flow, State Management

### 4.1 Core Data Structures

#### 4.1.1 BackendNode
```go
type BackendNode struct {
    ID               string
    Endpoint         string
    AuthMethod       AuthMethodType  // API_KEY, OAUTH2, MTLS, NONE
    AuthRef          string          // reference to Secrets Vault key
    Models           []string
    Capabilities     map[string]bool // streaming, vision, function_calling, etc.
    Region           string
    DataCenter       string
    LatencyP50       time.Duration
    LatencyP99       time.Duration
    ErrorRate        float64
    CurrentState     BackendState    // HEALTHY, DEGRADED, UNHEALTHY
    CircuitBreaker   CircuitBreakerState // CLOSED, OPEN, HALF_OPEN
    Capacity         int32           // max concurrent requests
    InFlightRequests int32
    CostPerInputToken     float64
    CostPerOutputToken    float64
    RateLimitPerSecond    int32
    BurstSize             int32
    LastChecked           time.Time
    Tags                  map[string]string
}
```

#### 4.1.2 RoutingRequest
```go
type RoutingRequest struct {
    RequestID     string
    TenantID      string
    UserID        string
    Model         string
    Parameters    ModelParameters
    RoutingPolicy RoutingPolicy
    Context       RequestContext
    ReceivedAt    time.Time
    Deadline      time.Time
    Priority      int
    RetryCount    int
}
```

#### 4.1.3 RoutingPolicy
```go
type RoutingPolicy struct {
    Strategy         StrategyType  // LATENCY, COST, CAPABILITY, FAILOVER, CUSTOM
    AllowedBackends  []string      // empty means all
    ExcludedBackends []string
    MaxCost          float64       // max cost per request in cents
    MaxLatency       time.Duration
    Weights          map[string]float64  // for weighted strategies
}
```

#### 4.1.4 RouteScore
```go
type RouteScore struct {
    BackendID      string
    Score          float64
    LatencyScore   float64
    CostScore      float64
    CapabilityScore float64
    AvailabilityScore float64
    Reason         string
}
```

#### 4.1.5 PriorityQueue
A min-heap ordered by (priority, deadline). Lower priority number = higher priority. Earlier deadline = dequeued first within same priority. Implemented as a channel-based queue with configurable depth.

#### 4.1.6 TokenBucket
```go
type TokenBucket struct {
    Tokens         int64
    MaxTokens      int64
    RefillRate     int64  // tokens per second
    LastRefillTime time.Time
    Mutex          sync.Mutex
}
```

### 4.2 Execution Flow

#### 4.2.1 Route Request Flow
1. **Ingress**: The gRPC/REST API layer receives a route request. A request ID is generated if not provided. The request is timestamped and passed to the Request Parser.
2. **Validation**: The Request Parser validates the request structure against the schema. If invalid, a structured error is returned immediately with code `ROUTE_INVALID_REQUEST`.
3. **Policy Resolution**: The routing policy is resolved. If the request does not specify a strategy, the default strategy from the configuration is applied. Allowed/Excluded backends are resolved and validated against the registry.
4. **Backend Enumeration**: A list of candidate backends is built from the Backend Registry, filtered by model support, region affinity (if applicable), and tenant permissions.
5. **Filtering**: The candidate list is further filtered by removing excluded backends, backends with OPEN circuit breakers, backends that exceed the max cost threshold for any possible completion length (estimated), and backends whose current latency P99 exceeds the configured max latency.
6. **Scoring**: If more than one candidate remains, the Route Evaluator computes a score for each candidate using the selected strategy. If zero candidates remain, the flow short-circuits to Queue or Fail.
7. **Selection**: The backend with the highest score is selected. In case of a tie, a random weighted selection is performed based on the top tier of scores.
8. **Proxy**: The Request Transformer converts the internal request into the backend-specific format. The proxy layer calls the backend with the transformed request.
9. **Response**: The backend response (or error) is received, transformed back into the canonical format, cost is calculated, and the response is returned to the caller. Success and failure are recorded in the audit log.
10. **Post-Routing**: The circuit breaker state is updated (success = reset failure count, failure = increment failure count), cost is recorded in the Cost Tracker, and all metrics are emitted.

#### 4.2.2 Health Check Flow
1. The Health Checker ticker fires (every N seconds per backend).
2. For each backend due for a check, a lightweight request is dispatched.
3. If the backend responds with HTTP 2xx within the timeout, latency is recorded, the backend state transitions to HEALTHY (or remains HEALTHY), and any OPEN circuit breaker on this backend transitions to HALF_OPEN and sends a single probe request.
4. If the backend fails (timeout, 5xx, connection error), the failure counter for the associated circuit breaker is incremented.
5. If the failure counter exceeds the threshold, the circuit breaker transitions to OPEN and the backend state is set to DEGRADED.
6. After 3 consecutive failed health checks, the backend state is set to UNHEALTHY and it is removed from the routing pool.
7. Metrics are emitted for each health check result.

### 4.3 State Management

#### 4.3.1 In-Memory State
The Backend Registry is held in memory for sub-microsecond routing decisions. It is a copy-on-write data structure: reads use an atomic load of a pointer to the latest snapshot; writes create a new snapshot and atomically swap the pointer. This ensures reads are never blocked by writes. The snapshot is rebuilt every time a backend is added, removed, or updated (via health check or configuration change).

#### 4.3.2 Redis-Backed State
Rate limiter token buckets, circuit breaker states, and queue entries are persisted in Redis for resilience. On startup, the Model Router loads circuit breaker states from Redis to avoid starting with a fully CLOSED state after a crash. Queue entries are stored as Redis sorted sets by deadline for reliable priority reinsertion.

#### 4.3.3 State Recovery
On crash recovery:
1. Load configuration from the Configuration System.
2. Restore circuit breaker states from Redis (CLOSED if not found, OPEN with remaining cooldown if found).
3. Restore queue entries from Redis and reinsert into the in-memory priority queue.
4. Rebuild the Backend Registry by issuing health checks to all known backends (no Redis state for registry; it is always derived from live data).
5. Resume normal routing operations.

---

## 5. Communication Protocols, APIs, Events, Queues

### 5.1 Communication Protocols

The Model Router exposes two primary API protocols:

#### 5.1.1 gRPC (Internal)
- Service definition in `router.proto`
- Bidirectional streaming for streaming model responses
- Unary RPCs for routing decisions
- Header-based tenant identification
- mTLS required for all internal calls

#### 5.1.2 REST (External)
- OpenAPI 3.0 specification
- JSON request/response bodies
- Rate limiting via `X-RateLimit-*` headers
- Authentication via `Bearer` token (API key)

### 5.2 APIs

#### 5.2.1 Route API
```
POST /api/v1/route
Body: RoutingRequest
Response: RoutingDecision or Error
Status: 200 (success), 429 (rate limited), 503 (no backends available), 400 (validation failure)
```

#### 5.2.2 Batch Route API
```
POST /api/v1/route/batch
Body: []RoutingRequest
Response: []RoutingDecision
Batch size limit: 100
```

#### 5.2.3 Backend Status API
```
GET /api/v1/backends
Response: []BackendNode (full status of all backends)
GET /api/v1/backends/{backend_id}
Response: BackendNode
```

#### 5.2.4 Health Check API
```
GET /api/v1/health
Response: {"status": "healthy", "backends": {"healthy": 12, "degraded": 1, "unhealthy": 0}}
```

#### 5.2.5 Metrics API
```
GET /api/v1/metrics
Response: Prometheus-formatted metrics
GET /api/v1/metrics/routing
Response: Routing-specific metrics (latency percentiles, strategy distribution, etc.)
```

#### 5.2.6 Configuration API
```
PUT /api/v1/config/backends
Body: BackendConfiguration
Response: 200 OK
```

### 5.3 Events

The Model Router emits the following events to the AIOS Event System:

| Event Type | Payload | Trigger | Consumer |
|---|---|---|---|
| `router.decision.made` | request_id, model, selected_backend, score, latency_ms | Every successful routing | Analytics, Audit Trail |
| `router.decision.failed` | request_id, model, error_code, reason | When no backend available | Monitoring, Auto-remediation |
| `router.backend.unavailable` | backend_id, reason, duration_seconds | Circuit breaker opens | Operations Dashboard |
| `router.backend.recovered` | backend_id, latency_ms | Circuit breaker closes | Operations Dashboard |
| `router.rate_limit.exceeded` | tenant_id, backend_id, reset_after_ms | Rate limit bucket empty | Monitoring, Cost Control |
| `router.cost.threshold.exceeded` | tenant_id, model, current_cost, threshold | Budget exceeded | Billing, Alerting |
| `router.queue.depth.warning` | queue_depth, wait_time_ms | Queue depth > 80% of max | Auto-scaling trigger |
| `router.health.check.failed` | backend_id, consecutive_failures | Health check failure | Operations Dashboard |

### 5.4 Queues

#### 5.4.1 Incoming Request Queue
- Type: Priority channel with configurable depth
- Default depth: 10,000 entries
- Overflow policy: Reject oldest lowest-priority request
- Used for: Buffering requests when all backends are at capacity

#### 5.4.2 Retry Queue
- Type: Delayed queue (executed after backoff expires)
- Backoff algorithm: `min(base_delay * 2^retry_count + jitter, max_delay)`
- Default base delay: 100ms
- Default max delay: 30 seconds
- Max retries: 3 (configurable per tenant)

#### 5.4.3 Health Check Queue
- Type: Time-wheel based scheduler
- Each backend has its own interval
- Backends are checked asynchronously to avoid blocking

---

## 6. Caching, Memory, Persistence

### 6.1 Caching

#### 6.1.1 Scoring Cache
- Cache key: `score:{strategy}:{model}:{tenant}:{region_hint}`
- TTL: 5 seconds (low TTL because backend metrics change frequently)
- Storage: In-memory LRU cache
- Max entries: 100,000
- Eviction policy: LRU with TTL expiration

#### 6.1.2 Backend Registry Snapshot Cache
- Cache key: N/A (singleton)
- TTL: Event-driven (updated on health check or config change)
- Storage: In-memory atomic pointer
- Purpose: Avoids lookup overhead on every routing request

#### 6.1.3 Auth Token Cache
- Cache key: `auth:{backend_id}:{token_hash}`
- TTL: 80% of token expiry
- Storage: In-memory TTL cache with background refresh
- Purpose: Reuses OAUTH2 bearer tokens to reduce authentication overhead

#### 6.1.4 Health Check Result Cache
- Cache key: `health:{backend_id}`
- TTL: Health check interval (configurable per backend)
- Storage: In-memory with atomic update
- Purpose: Prevents redundant health checks

### 6.2 Memory

#### 6.2.1 In-Memory Data
- Backend Registry snapshot: ~1KB per backend, typically < 10KB total
- Scoring Cache: ~500 bytes per entry, up to ~50MB for 100,000 entries
- Priority Queue: variable, up to 10,000 entries at ~2KB each = ~20MB
- Auth Token Cache: ~2KB per token, up to ~2MB
- Total estimated memory footprint: 50MB–200MB depending on workload

#### 6.2.2 Memory Limits
- Configurable max heap limit via GOGC
- Graceful degradation: when memory approaches the limit, non-essential caches are purged (auth token cache first, then scoring cache)
- Memory monitoring via `process_resident_memory_bytes` metric

### 6.3 Persistence

#### 6.3.1 Redis Persistence
- Circuit breaker states: `breaker:{backend_id}` → `{state, remaining_cooldown_ms, failure_count}`
- Queue entries: `queue:{priority}` → sorted set of `{request_id: deadline_timestamp}`
- Rate limiter state: `rate_limit:{backend_id}:{tenant_id}` → `{tokens, last_refill}`
- All Redis keys have TTL for automatic cleanup

#### 6.3.2 File/NoSQL Persistence
- Configuration is persisted in the Configuration System (etcd or file)
- Audit logs are streamed to the Logging System (not stored locally)
- Cost metrics are stored in the Cost System (PostgreSQL or TimescaleDB)

#### 6.3.3 No Local Persistence
- The Model Router does NOT write to local disk
- All state is ephemeral (in-memory) or backed by Redis
- This enables stateless horizontal scaling: any Model Router instance can handle any request

---

## 7. Validation, Retry Logic, Error Recovery

### 7.1 Validation

#### 7.1.1 Request Validation
- Model name exists in registry: `model` must match exactly or be a known alias
- Parameters comply with schema: temperature ∈ [0,2], top_p ∈ (0,1], max_tokens ∈ [1, 409600], presence_penalty ∈ [-2,2], frequency_penalty ∈ [-2,2]
- Required fields present: `messages` (for chat) or `prompt` (for completion), `model`
- Content length: total prompt tokens must be ≤ model's context window minus configured minimum output tokens
- Streaming: if stream=true, backend must support streaming; if caller does not support streaming, fall back to non-streaming
- Priority bounds: priority ∈ [0, 1000], lower number = higher priority

#### 7.1.2 Backend Validation
- Endpoint URL is valid and reachable (verified at registration)
- Authentication credentials are present and valid (verified on registration and periodically)
- Model-to-backend mapping is consistent: the backend must actually serve the claimed model
- Rate limit configuration is non-zero

#### 7.1.3 Policy Validation
- Strategy must be one of: `latency`, `cost`, `capability`, `failover`, `round-robin`, `weighted`, `custom:{name}`
- Allowed/Excluded backends must reference existing backend IDs
- Weights must sum to 1.0 for weighted strategies
- Budget values must be non-negative

### 7.2 Retry Logic

#### 7.2.1 Retry on Backend Failure
- Triggered when: backend returns HTTP 5xx, connection timeout, TLS error, circuit breaker is OPEN for the selected backend during proxying
- Max retries: 3 (configurable per tenant, per model)
- Backoff algorithm: `delay = min(base_delay * 2^retry_count + jitter(0, 100ms), max_delay)`
  - base_delay = 100ms
  - max_delay = 30s
- Jitter: random uniform between 0 and 100ms added to each delay
- Retry behavior: the failed backend is excluded from the candidate list for subsequent retry attempts
- On each retry, the routing score is recomputed (since backend metrics may have changed)
- After all retries: final error is returned with `retryable: false`

#### 7.2.2 Retry on Rate Limit
- Triggered when: backend returns HTTP 429 or `X-RateLimit-Remaining: 0`
- Behavior: request is queued for `Retry-After` duration (from response header) or a default of 1 second
- The request is resubmitted to the routing engine with an excluded list containing the rate-limited backend
- If all backends are rate-limited, the request is queued with the shortest retry-after

#### 7.2.3 Retry on Transient Error
- Triggered when: DNS resolution failure, temporary network partition, upstream load balancer timeout
- Same backoff and jitter as backend failure retry
- Backend is NOT excluded from the candidate pool (the error may be transient for all backends)

### 7.3 Error Recovery

#### 7.3.1 Circuit Breaker Recovery
- When in OPEN state, the circuit breaker allows a single probe request after cooldown (default 30 seconds, configurable per backend)
- The probe uses a minimal request (e.g., "Hello" completion to the model) to minimize cost
- If the probe succeeds (HTTP 2xx, response within timeout), the breaker transitions to CLOSED
- If the probe fails, the breaker returns to OPEN with an increased cooldown (exponential backoff, max 300 seconds)
- Successful probe restores the backend to the candidate pool

#### 7.3.2 Startup Recovery
- On boot, the Model Router loads the last known backend configuration and immediately issues health checks to ALL backends
- During the health check window (expected: 1-2 seconds), all routing requests are queued
- After the initial health check completes, queued requests are processed
- If the initial health check takes longer than 5 seconds, requests are rejected with a `503 Service Unavailable` and `retry_after: 5`

#### 7.3.3 Backend Deregistration
- If a backend is manually removed from configuration, all in-flight requests to that backend are allowed to complete (with a hard timeout of 60 seconds)
- After 60 seconds, any pending requests to the removed backend are failed with a `BACKEND_DEREGISTERED` error
- The backend is immediately removed from the candidate pool

#### 7.3.4 Graceful Shutdown
- On SIGTERM, the Model Router stops accepting new routing requests
- In-flight requests are allowed to complete within a configurable drain timeout (default 30 seconds)
- After the drain timeout, any remaining in-flight requests are failed with `ROUTER_SHUTTING_DOWN`
- All queue entries are flushed to Redis
- The process exits cleanly

---

## 8. Security, Authentication, Authorization, Permissions

### 8.1 Authentication

#### 8.1.1 Internal (gRPC)
- mTLS is mandatory: every internal caller presents a client certificate signed by the AIOS internal CA
- The Model Router validates the certificate CN against an allow-list of authorized services
- No anonymous access is permitted over gRPC

#### 8.1.2 External (REST)
- API keys (random 64-character hex strings) issued to tenants and agents
- API keys are hashed with bcrypt and stored in the Secrets Vault
- The Model Router validates the key against the vault on each request (with a 5-minute cache)
- JWT tokens are also supported for machine-to-machine calls

### 8.2 Authorization

#### 8.2.1 Model Access Control
Each model has an access control list (ACLs) defining which tenants, users, and agents can use it:
- `allow_tenants`: list of tenant IDs
- `allow_users`: list of user IDs
- `allow_agents`: list of agent IDs
- `deny_patterns`: reject requests matching patterns (e.g., certain content types)

If no ACL is defined for a model, the model uses the tenant's default access level.

#### 8.2.2 Tenant Isolation
- Tenant A cannot see, use, or affect backend availability for Tenant B
- Rate limits are enforced per-tenant and per-backend
- Cost budgets are enforced per-tenant
- Operations on one tenant must not degrade or impact other tenants

### 8.3 Permissions

A user/subject needs these permissions to use the Model Router:
- `models.route` — allows routing a request to a model
- `models.route.<specific_model>` — allows routing to a specific model
- `models.view_backends` — allows viewing backend status
- `models.update_config` — allows updating routing configuration (admin only)

### 8.4 Secrets Management

- API keys and OAUTH2 tokens for backend providers are stored in the AIOS Secrets Vault
- The Model Router requests credentials at boot time and caches them with periodic refresh
- Credentials are NEVER logged, even in audit logs (the credential reference is logged instead)
- The Model Router supports automatic credential rotation: if a backend returns HTTP 401, the router invalidates its cached credential and fetches a fresh one

### 8.5 Request Security

- All requests and responses are encrypted in transit (TLS 1.3 minimum)
- All internal traffic over gRPC is encrypted with mTLS
- Request payloads that contain PII or sensitive data are logged only with field-level hashing
- The Model Router supports request content filtering: if the request matches a pattern in the deny list (e.g., prompt injection patterns), the request is rejected with code `CONTENT_BLOCKED`

### 8.6 Auditing

- Every routing decision is logged with a non-repudiable audit trail
- Audit log entries include: request_id, user_id, tenant_id, timestamp, model, selected_backend, all candidate backends and their scores, decision reason, success/failure, and latency
- Audit logs are write-only (append-only) and stored in the central Logging System
- Audit log retention: 90 days (configurable)

---

## 9. Monitoring, Logging, Metrics, Tracing

### 9.1 Metrics

The Model Router exposes Prometheus metrics on `/api/v1/metrics`:

| Metric Name | Type | Labels | Description |
|---|---|---|---|
| `aios_router_requests_total` | Counter | `model`, `backend`, `status`, `tenant` | Total routing requests processed |
| `aios_router_routing_duration_ms` | Histogram | `model`, `strategy` | Time spent on routing decision (excluding backend inference) |
| `aios_router_inference_duration_ms` | Histogram | `model`, `backend` | Total end-to-end inference time |
| `aios_router_queue_depth` | Gauge | `priority` | Current queue depth |
| `aios_router_queue_wait_ms` | Histogram | `priority` | Time requests spent in queue |
| `aios_router_backend_state` | Gauge | `backend_id` | 0=UNHEALTHY, 1=DEGRADED, 2=HEALTHY |
| `aios_router_circuit_breaker_state` | Gauge | `backend_id` | 0=CLOSED, 1=HALF_OPEN, 2=OPEN |
| `aios_router_rate_limit_exceeded_total` | Counter | `backend_id`, `tenant_id` | Rate limit exceeded count |
| `aios_router_cost_total_cents` | Counter | `model`, `backend`, `tenant` | Cumulative cost in cents |
| `aios_router_backend_latency_p50_ms` | Gauge | `backend_id` | P50 latency for a backend |
| `aios_router_backend_latency_p99_ms` | Gauge | `backend_id` | P99 latency for a backend |
| `aios_router_retry_count_total` | Counter | `model`, `reason` | Total retry attempts |
| `aios_router_fallback_total` | Counter | `model`, `from_backend`, `to_backend` | Fallback trigger count |
| `aios_router_in_flight_requests` | Gauge | - | Current in-flight routing requests |
| `aios_router_scoring_cache_hit_ratio` | Gauge | - | Cache hit ratio for scoring |

### 9.2 Logging

- **Log level**: INFO for routing decisions, ERROR for failures, DEBUG for detailed routing evaluation (off by default)
- **Log format**: Structured JSON
- **Log fields (every entry)**: `timestamp`, `level`, `service` (model-router), `request_id`, `tenant_id`, `model`, `backend_id`, `decision`, `latency_ms`, `error_code`
- **Log shipping**: All logs are streamed to the central Logging System (ELK/Loki)
- **Log sample rate**: 100% for ERROR, 10% for INFO, 0.1% for DEBUG (configurable)
- **PII redaction**: Fields matching patterns for email, phone, SSN, API keys are redacted with `***` before logging

### 9.3 Tracing

- Distributed tracing with OpenTelemetry
- Trace propagation via W3C Trace Context headers
- Span creation:
  - `router.handle_request` — root span for each routing request
  - `router.validate_request` — validation sub-span
  - `router.score_backends` — scoring sub-span
  - `router.select_backend` — selection sub-span
  - `router.proxy_request` — backend proxy sub-span (with `peer.service` attribute)
- All spans include `model`, `backend`, `tenant_id`, `request_id` attributes
- Traces are sampled: 100% for ERROR, 10% for success (configurable rate)
- Trace export: OTLP to the central tracing backend (Jaeger or Tempo)

### 9.4 Health Checks

- Liveness: `/health/live` — returns 200 if the service is running
- Readiness: `/health/ready` — returns 200 if the router has completed initialization, backends are being checked, and the queue is accepting requests
- Detailed: `/health/detail` — returns JSON with per-backend health status

### 9.5 Alerting Rules (Prometheus/AlertManager)

| Rule | Condition | Severity |
|---|---|---|
| `HighErrorRate` | `aios_router_requests_total{status="error"} / aios_router_requests_total > 0.05` for 5 minutes | Critical |
| `AllBackendsUnhealthy` | `aios_router_backend_state == 0 for all backend_id` | Critical |
| `QueueDepthCritical` | `aios_router_queue_depth > 5000` | Warning |
| `CircuitBreakerFrequent` | `aios_router_circuit_breaker_state{backend_id="..."} == 2 for > 300 seconds` | Warning |
| `LatencyHighP99` | `aios_router_backend_latency_p99_ms > 10000` | Warning |
| `CostRateHigh` | `rate(aios_router_cost_total_cents[5m]) > threshold` | Warning |

---

## 10. Scalability, Performance, Optimization

### 10.1 Scalability

#### 10.1.1 Horizontal Scaling
- The Model Router is stateless from a request-processing perspective
- Multiple instances can be deployed behind a round-robin load balancer
- Redis serves as the shared state layer:
  - Circuit breaker states
  - Rate limiter buckets
  - Queue entries
- Cache invalidation is not required because the Backend Registry is updated via health checks, which are distributed across instances (each instance runs its own health checks and writes to Redis)

#### 10.1.2 Scaling Limits
- **Maximum instances**: 32 (limited by Redis connection pool)
- **Maximum backends per instance**: 5,000
- **Maximum concurrent requests per instance**: 10,000
- **Maximum queue depth per instance**: 50,000

#### 10.1.3 Auto-scaling
- Metric: `aios_router_queue_depth` across all priority levels
- Scale up when: average queue depth > 2000 for 30 seconds
- Scale down when: queue depth < 100 for 5 minutes
- Minimum instances: 2 (for HA)
- Maximum instances: 32

### 10.2 Performance Objectives

| Metric | Target | Degradation Threshold |
|---|---|---|
| Routing decision P50 | < 500µs | > 2ms |
| Routing decision P99 | < 2ms | > 10ms |
| Queue wait time P99 | < 100ms | > 500ms |
| End-to-end overhead P50 | < 2ms | > 5ms |
| Throughput per instance | 5,000 req/s | - |
| Maximum response time | 60s per request | - |
| Scoring cache hit ratio | > 80% | < 50% |

### 10.3 Optimization Techniques

#### 10.3.1 Lock-Free Reads
- The Backend Registry uses atomic pointer swap for snapshot isolation
- Reads do not acquire any lock
- Writes create a new snapshot and atomically publish it

#### 10.3.2 Connection Pooling
- Backend HTTP connections are pooled with keep-alive
- Maximum idle connections per backend: 100
- Connection timeout: 5 seconds
- Pool is per-instance (each instance has its own connection pool to each backend)

#### 10.3.3 Scoring Cache
- The scoring result for (model, strategy, backends_snapshot_hash) is cached for 5 seconds
- If backends have not changed, the cache serves the same result
- The cache is invalidated on any Backend Registry update

#### 10.3.4 Request Batching
- Requests to the same backend with compatible parameters can be batched (disabled by default)
- Batching reduces per-request overhead at the cost of increased latency for batched requests
- Configurable batch window: 10ms (requests arriving within 10ms of each other for the same backend may be batched)

#### 10.3.5 Zero-Copy Request Transformation
- The Request Transformer uses a buffer pool to avoid allocations
- Response streaming uses a pass-through transform (no buffering of the full response)

### 10.4 Load Shedding

When the system is overloaded:
1. **Non-critical requests are dropped first**: requests with `priority` > 500 (lowest priority) are rejected
2. **Sampling is reduced**: metrics detailed logging is reduced
3. **Retries are disabled**: retries are disabled to avoid adding load
4. **Fallback strategy is disabled**: force the request to fail fast rather than retrying on different backends
5. **Health check interval is increased**: less frequent health checks to reduce overhead

---

## 11. Testing Strategy, Risk Analysis

### 11.1 Testing Strategy

#### 11.1.1 Unit Tests
- Test each routing strategy (latency, cost, capability, failover, random, weighted) with mock backend registries
- Test validation logic for all parameter bounds
- Test circuit breaker state transitions
- Test token bucket rate limiter with various refill rates
- Test priority queue ordering and deadline handling
- Coverage target: > 90%

#### 11.1.2 Integration Tests
- Test end-to-end routing flow with a mock backend server
- Test health checker with a backend that alternates between healthy and unhealthy
- Test cost tracking across multiple requests
- Test request transformer for each supported provider format
- Test retry logic with a backend that fails N times then succeeds
- Coverage target: > 80%

#### 11.1.3 Performance Tests
- Benchmark routing decision latency: measure P50, P99, P99.9 for 100,000 requests
- Benchmark concurrent request handling: measure throughput with 50 concurrent clients
- Benchmark scoring cache hit ratio at different cache TTLs
- Benchmark circuit breaker overhead under failure conditions
- Target: routing decision P50 < 500µs, P99 < 2ms

#### 11.1.4 Chaos Tests
- Simulate backend failures (random backend goes down)
- Simulate Redis failure: verify the router degrades gracefully (circuit breakers and rate limiters lose accuracy but routing continues based on last known state)
- Simulate network latency spikes: verify retry and queue behavior
- Simulate configuration change while requests are in-flight: verify no lost requests

#### 11.1.5 Configuration Tests
- Test every configuration option with valid and invalid values
- Test configuration hot-reload without downtime
- Test configuration persistence and recovery

### 11.2 Risk Analysis

| Risk | Probability | Impact | Mitigation |
|---|---|---|---|
| All backends become unavailable | Low | High | Queue requests, retry with backoff, emit urgent alert |
| Routing misconfiguration causes highest-cost backend selection | Medium | Medium | Validate configuration on load, require dual-approval for config changes, implement cost cap per request |
| Scoring cache returns stale data leading to suboptimal routing | Medium | Low | Short TTL (5 seconds), invalidate cache on backend change, add staleness metric |
| Redis becomes unavailable | Low | Medium | Degrade to local in-memory state, circuit breakers reset to CLOSED, rate limiters are disabled, queue capacity drops |
| Backend credential rotation fails | Low | High | Multiple credential refresh attempts, alert if refresh fails after 3 attempts |
| Malicious tenant performs resource exhaustion attack | Medium | High | Per-tenant rate limits, per-tenant circuit breakers, per-tenant cost budgets, request-level monitoring |
| Infinite retry loop causes cost spiral | Low | High | Hard limit of 3 retries, circuit breaker prevents retries to the same backend, alert on high retry rate |

---

## 12. Failure Scenarios, Recovery Procedures

### 12.1 Backend Unreachable

**Scenario**: An external model provider API is unreachable (e.g., network partition, DNS failure, provider outage).

**Detection**: Health check timeout, connection error on route request.

**Response**:
1. Circuit breaker opens for the affected backend.
2. The backend is excluded from the candidate pool.
3. Subsequent requests for that model are routed to the next best candidate.
4. If no other candidate exists, the request is queued or failed with a `503` and `retryable: true`.
5. The operations team is alerted.

**Recovery**:
1. The health checker continues to probe on the configured interval.
2. When the backend becomes reachable again, the probe succeeds.
3. The circuit breaker transitions to CLOSED.
4. The backend is added back to the candidate pool.

### 12.2 All Backends Degraded

**Scenario**: All registered backends for a model are either unhealthy or running with OPEN circuit breakers.

**Detection**: `.Route` returns zero candidates after filtering.

**Response**:
1. All routing requests for that model are queued with escalating priority.
2. An alert is triggered: `AllBackendsUnhealthy`.
3. A background goroutine retries routing every N seconds (configurable, default 5).
4. If the queue depth exceeds 80% of max, a notification is sent to auto-scaling.

**Recovery**:
1. Backends recover via health checker probes.
2. Once at least one backend is HEALTHY, queued requests are processed in priority order.
3. Normal routing resumes.

### 12.3 Rate Limiter Bombardment

**Scenario**: A tenant or a specific tenant's workload exceeds the rate limit on all backends simultaneously.

**Detection**: Rate limiter returns bucket empty for all backends for a tenant.

**Response**:
1. The request is queued with a `Retry-After` of 1 second.
2. If the queue depth for this tenant exceeds a threshold, lower-priority requests are dropped.
3. An event `rate_limit.exceeded` is emitted with tenant_id and backend_id.
4. Rate limit metric is incremented.

### 12.4 Cost Budget Exceeded

**Scenario**: A tenant's daily or monthly cost budget is exceeded.

**Detection**: Cost tracker returns `budget_exceeded` for the tenant.

**Response**:
1. Routing decisions for that tenant skip high-cost backends.
2. If no low-cost backends are available, the request is failed with `BUDGET_EXCEEDED`.
3. An alert is sent to the tenant administrator.
4. Hard budget enforcement: requests are rejected with `403 BUDGET_EXCEEDED`.

### 12.5 Configuration Corruption or Invalid Configuration

**Scenario**: A configuration update introduces invalid backend definitions or removes all backends for a model.

**Detection**: Configuration validation fails on load.

**Response**:
1. The configuration update is rejected and the previous configuration remains active.
2. An alert is triggered: `InvalidConfiguration`.
3. The operator is notified with a detailed diff of the invalid changes.

### 12.6 In-Memory State Corruption

**Scenario**: A software bug corrupts the in-memory Backend Registry or scoring cache.

**Detection**: Metrics show panics, invalid scores, or negative values.

**Response**:
1. The corrupted component is rebuilt from Redis state and health checks.
2. The service continues operation with minimal disruption.
3. A detailed error log is emitted with the output of the corrupted state.

---

## 13. Operational Limits, Maintenance

### 13.1 Operational Limits

| Parameter | Limit | Behavior When Exceeded |
|---|---|---|
| Maximum number of backends | 5,000 | Reject registration of new backends |
| Maximum queue depth | 10,000 per instance | Reject new requests with `QUEUE_FULL` |
| Maximum concurrent requests per instance | 10,000 | Reject new requests with `TOO_MANY_CONCURRENT_REQUESTS` |
| Maximum request size | 1 MB | Reject with `PAYLOAD_TOO_LARGE` |
| Maximum batch size | 100 requests | Reject with `BATCH_TOO_LARGE` |
| Maximum model name length | 256 characters | Reject with `INVALID_MODEL_NAME` |
| Maximum timeout per request | 300 seconds | Timeout the request with `REQUEST_TIMEOUT` |
| Number of retries per request | 3 | Fail without retry after 3 attempts |

### 13.2 Maintenance

#### 13.2.1 Zero-Downtime Updates
- The Model Router supports rolling updates via Kubernetes Deployment strategy
- A load balancer health check ensures traffic is only sent to ready instances
- Drain timeout: 30 seconds (in-flight requests complete, new requests are blocked)
- Configuration updates are applied without restart: the Configuration Watcher detects changes and hot-reloads

#### 13.2.2 Configuration Hot-Reload
- The AIOS Configuration System supports live configuration updates
- The Model Router subscribes to configuration change events
- Backend definitions, strategies, rate limits, and budgets can all be updated without restart
- On configuration change, the registry snapshot is rebuilt, and the scoring cache is invalidated

#### 13.2.3 Versioning
- The Model Router API is versioned via URL prefix: `/api/v1/`
- Backend definitions include a version field for schema evolution
- Configuration schema has a version field to detect drift
- Breaking changes require a new API version

#### 13.2.4 Backup and Restore
- Configuration is backed up automatically by the AIOS Configuration System
- Circuit breaker and rate limiter state in Redis is ephemeral and does not require backup
- Restore: deploy a new instance and point it at the same Redis and Configuration System

---

## 14. Future Improvements, Examples

### 14.1 Future Improvements

#### 14.1.1 Predictive Routing
Use historical latency and cost data to predict backend performance for specific request patterns (e.g., requests with long prompts are slower on some backends). Pre-warm the scoring cache with predicted scores.

#### 14.1.2 Multi-Model Composite Routing
Route parts of the same request to different models: e.g., use a fast cheap model for initial processing and a premium model for verification. Combine responses into a single coherent output.

#### 14.1.3 Semantic Model Selection
Instead of requiring the caller to specify a model name, allow the caller to specify a capability profile (e.g., "high quality, fast, vision-capable") and let the router select the model that best matches the profile.

#### 14.1.4 Dynamic Pricing Negotiation
For self-hosted GPU clusters with dynamic pricing (based on GPU availability), the router could negotiate prices in real time and select the cheapest available.

#### 14.1.5 Cross-Region Request Steering
Route requests not just across providers but across regions, selecting the region with the lowest current load and closest proximity to the caller.

#### 14.1.6 Rate Limit Prediction
Predict rate limit exhaustion based on token consumption trends and pre-emptively throttle before the limit is hit.

#### 14.1.7 A/B Testing Support
Ability to route a percentage of traffic to a new model/provider for evaluation and comparison against existing backends.

### 14.2 Examples

#### Example 1: Simple Routing (Default Strategy)
```
Request: model="gpt-4", strategy="latency"
Backends: openai-eastus (latency=800ms), openai-westus (latency=1200ms)
Decision: route to openai-eastus (lowest latency)
```

#### Example 2: Cost-Optimized Routing
```
Request: model="claude-3-haiku", strategy="cost", max_cost=0.01
Backends: anthropic-eastus ($0.0005/token, latency=600ms), google-cloud ($0.0008/token, latency=400ms)
Decision: route to anthropic-eastus (lowest cost within budget)
```

#### Example 3: Fallover Routing with Circuit Breaker
```
Request: model="gpt-4", strategy="failover"
Backends: openai-primary (CIRCUIT OPEN), openai-secondary (HEALTHY)
Decision: route to openai-secondary (primary is open)
Event emitted: router.decision.made with decision_reason="failover:primary_circuit_open"
```

#### Example 4: Queue and Retry
```
Request: model="gpt-4", all backends at capacity
Response: queued (queue depth=342)
After 2 seconds: backend capacity available, request routed to openai-eastus
Total wait time: 2100ms
```

#### Example 5: Request Transformation
```
Request (internal): {model="gpt-4", messages: [{role:"user", content:"Hello"}]}
Target backend: OpenAI API
Transformed request: {model:"gpt-4", messages: [{role:"user", content:"Hello"}], temperature:0.7}
Response from OpenAI: {id:"chatcmpl-xxx", choices:[{...}]}
Transformed response (internal): {content:"Hello! How can I help you today?", usage:{prompt_tokens:10, completion_tokens:9}}
```

---

## 15. Acceptance Criteria, Definition of Done

### 15.1 Acceptance Criteria

1. **Routing correctness**: Given a routing request with a valid model and at least one healthy backend, the router MUST return a non-error response with a selected backend that could serve the request. Verified by unit test battery.

2. **Health check mechanism**: A backend that returns an error for N consecutive health checks MUST be marked UNHEALTHY and excluded from the routing pool. Verified by integration test.

3. **Circuit breaker**: A backend that fails M times within a sliding window MUST be protected by an OPEN circuit breaker. M and the window are configurable. Verified by integration test.

4. **Rate limiting**: If the rate limit for a backend or tenant is exceeded, the request MUST be rejected with HTTP 429 or queued with a `Retry-After` header. Verified by performance test.

5. **Retry behavior**: A failed request (receiving a 5xx from the backend) MUST be retried up to N times with exponential backoff. The failed backend MUST be excluded from the retry candidate pool. Verified by integration test.

6. **Scoring correctness**: The routing strategy MUST correctly order backends by the chosen criterion (latency, cost, capability, or failover priority). Verified by unit test.

7. **Audit trail**: Every routing decision MUST produce a structured audit log entry with request_id, model, selected_backend, decision reason, latency, and success/failure. Verified by integration test.

8. **Performance target**: Routing decision P50 < 500µs and P99 < 2ms under 1,000 requests per second. Verified by performance test.

9. **Graceful degradation**: When Redis is unavailable, the router must continue to operate with best-effort routing (no rate limiting, circuit breakers reset to CLOSED). Verified by chaos test.

10. **Security**: All API calls require authentication. Credentials are never exposed in logs. mTLS is enforced for gRPC. Verified by security scan and unit test.

### 15.2 Definition of Done

A feature or fix for the Model Router is considered DONE when:

- [ ] All acceptance criteria for the feature/fix are satisfied
- [ ] Unit test coverage for new code is at least 90%
- [ ] Integration tests cover the happy path, error paths, and edge cases
- [ ] Performance benchmark shows no regression beyond the acceptable threshold
- [ ] All new configuration is validated with positive and negative tests
- [ ] Security review has been completed (no new credentials in logs, no broken auth, no privilege escalation)
- [ ] Monitoring metrics and logs have been added for the feature
- [ ] Documentation has been updated (including the engineering spec if behavior changed)
- [ ] All changes are reviewed by at least one other engineer
- [ ] The feature is deployed to staging and passes canary testing for 24 hours before production rollout
- [ ] Rollback plan exists: the previous version of the Model Router can be redeployed in under 5 minutes

## 16. Appendix: Detailed Routing Strategy Specifications

### 16.1 Latency-Optimized Strategy
The latency-optimized strategy selects the backend with the lowest current P50 latency. The scoring function is:
```
score = 1.0 / (1.0 + current_latency_p50_seconds)
```
Backends with latency P50 above the configured max_latency are excluded from scoring. If two backends have identical P50 latency, P99 latency is used as a tiebreaker.

### 16.2 Cost-Optimized Strategy
The cost-optimized strategy selects the backend with the lowest estimated cost. The estimated cost is computed as:
```
estimated_cost = (input_tokens * cost_per_input_token) + (expected_output_tokens * cost_per_output_token)
```
If max_cost_per_request is set, backends with estimated_cost > max_cost_per_request are excluded. The scoring function is:
```
score = 1.0 / (1.0 + estimated_cost_cents)
```

### 16.3 Capability-Priority Strategy
Each backend declares a capability score (0.0 to 1.0) for each model it serves. The capability score represents the quality, feature completeness, and reliability of that model on this backend. The scoring function uses the declared capability score directly. This is useful when a self-hosted model with lower latency and cost should still be deprioritized in favor of a cloud provider with higher quality outputs.

### 16.4 Failover Strategy
Backends are ordered by a configured priority list. The first backend in the priority list that is HEALTHY and has a CLOSED circuit breaker is selected. If a higher-priority backend recovers while a lower-priority one is in use, subsequent requests are NOT re-routed (no connection draining). Only new requests use the recovered higher-priority backend.

### 16.5 Weighted Random Strategy
Each backend is assigned a weight (0.0 to 1.0). The probability of selecting a backend is weight / sum_of_all_weights. This is useful for A/B testing or gradual traffic migration. Weights are validated to ensure they sum to 1.0; if they do not, they are normalized.

### 16.6 Custom Strategy
Users can define custom strategies as a Lua script that receives the candidate backends and returns a selected backend ID. The script has access to the full RoutingRequest and BackendNode objects. Custom strategies are sandboxed and have a 10ms execution budget.

## 17. Appendix: Error Codes Reference

| Code | HTTP Status | Description | Retryable |
|---|---|---|---|
| ROUTER_NO_BACKEND_AVAILABLE | 503 | No backend is currently available for the requested model | Yes |
| ROUTER_INVALID_REQUEST | 400 | Request failed schema validation | No |
| ROUTER_RATE_LIMIT_EXCEEDED | 429 | Rate limit exceeded for the tenant or backend | Yes (after delay) |
| ROUTER_BUDGET_EXCEEDED | 403 | Cost budget has been exceeded for the tenant | No |
| ROUTER_BACKEND_ERROR | 502 | The selected backend returned an error | Yes |
| ROUTER_TIMEOUT | 504 | The backend did not respond within the timeout | Yes |
| ROUTER_QUEUE_FULL | 503 | The request queue is full | Yes |
| ROUTER_SHUTTING_DOWN | 503 | The router is in the process of shutting down | Yes |
| ROUTER_UNAUTHORIZED | 401 | The caller is not authenticated | No |
| ROUTER_FORBIDDEN | 403 | The caller does not have permission to use the model | No |
| ROUTER_MODEL_NOT_FOUND | 404 | The requested model is not registered | No |

## 18. Appendix: Configuration Reference

| Key | Type | Default | Description |
|---|---|---|---|
| router.default_strategy | string | latency | Default routing strategy |
| router.health_check.interval | duration | 10s | Interval between health checks |
| router.health_check.timeout | duration | 5s | Timeout for health check requests |
| router.health_check.failure_threshold | int | 3 | Consecutive failures before marking UNHEALTHY |
| router.circuit_breaker.failure_threshold | int | 5 | Failures in window before opening |
| router.circuit_breaker.window | duration | 60s | Sliding window for failure tracking |
| router.circuit_breaker.cooldown | duration | 30s | Time in OPEN state before HALF_OPEN |
| router.rate_limiter.default_limit | int | 100 | Default requests per second per tenant |
| router.rate_limiter.default_burst | int | 200 | Default burst allowance |
| router.queue.max_depth | int | 10000 | Maximum request queue depth |
| router.queue.default_timeout | duration | 30s | Default request timeout |
| router.cache.scoring_ttl | duration | 5s | TTL for scoring cache entries |
| router.cache.max_scoring_entries | int | 100000 | Maximum scoring cache entries |

## 19. Appendix: Deployment Architecture

The Model Router is deployed as a Kubernetes Deployment with the following characteristics:
- Replicas: 2 minimum, 32 maximum (auto-scaled based on queue depth)
- Resource requests: 0.5 CPU, 512MB RAM
- Resource limits: 2 CPU, 2GB RAM
- Readiness probe: GET /health/ready (initial delay: 5s, period: 10s)
- Liveness probe: GET /health/live (initial delay: 10s, period: 15s)
- Service: ClusterIP with gRPC and HTTP ports
- Service Mesh: Istio sidecar for mTLS and traffic management
- ConfigMap: Routing configuration (mounted and watched for changes)
- Secrets: Backend credentials (mounted from Vault via CSI driver)

## 20. Appendix: Quick Start Examples

### 20.1 Registering a Backend
```json
POST /admin/v1/backends
{
  "id": "openai-eastus-01",
  "endpoint": "https://api.openai.com/v1",
  "auth_method": "api_key",
  "auth_ref": "secrets.openai_api_key",
  "models": ["gpt-4", "gpt-4-turbo", "gpt-3.5-turbo"],
  "region": "eastus",
  "cost_per_input_token": 0.00003,
  "cost_per_output_token": 0.00006,
  "rate_limit_per_second": 100,
  "burst_size": 200,
  "capacity": 100
}
```

### 20.2 Routing a Request
```json
POST /api/v1/route
{
  "model": "gpt-4",
  "parameters": {"messages": [{"role": "user", "content": "Hello"}], "max_tokens": 100},
  "routing_policy": {"strategy": "latency", "max_cost_per_request": 0.01}
}
```

### 20.3 Setting a Budget
```json
PUT /admin/v1/budgets/tenant-123
{
  "daily_budget_cents": 1000,
  "monthly_budget_cents": 30000,
  "hard_cap": true,
  "notification_threshold": 0.8
}
```
