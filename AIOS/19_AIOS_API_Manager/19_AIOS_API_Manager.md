# AIOS API Manager — Engineering Specification v2.0

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
The AIOS API Manager is the unified ingress gateway, request routing layer, protocol adapter, API lifecycle management, developer portal, and API analytics engine for the entire AIOS ecosystem. It provides a single point of entry for all external clients that interact with AIOS — whether they are end-user applications, third-party integrations, internal microservices, or AI agents running in remote environments. The API Manager abstracts away the complexity of internal service topology, protocol mismatches, authentication mechanisms, rate limiting, request validation, response caching, and API versioning. It implements enterprise-grade API management capabilities: API key management, usage plans, developer portals, API documentation generation, request/response transformation, API monetization, and SLA monitoring. The API Manager ensures that every API exposed by the AIOS ecosystem is discoverable, documented, secure, rate-limited, and observable.

### 1.2 Mission
To provide a unified, secure, and developer-friendly API gateway that makes every AIOS capability accessible through well-documented, versioned, and policy-enforced APIs. The API Manager aims to reduce the time to integrate with AIOS from days to minutes by offering auto-generated OpenAPI documentation, SDK generation, interactive API explorers, and self-service API key management. The mission includes enforcing consistent security policies across all APIs, providing real-time monitoring and analytics on API usage, and ensuring 99.99% gateway availability with sub-5ms gateway overhead per request.

### 1.3 Responsibilities
- **API Gateway**: Act as the single ingress point for all external HTTP/gRPC/WebSocket traffic, routing requests to the appropriate internal service
- **API Lifecycle Management**: Manage APIs through their full lifecycle: DESIGN, DEVELOP, TEST, PUBLISH, DEPLOY, MONITOR, DEPRECATE, RETIRE
- **API Versioning**: Support multiple API versions simultaneously with graceful deprecation and migration guides
- **API Documentation**: Auto-generate interactive OpenAPI documentation (Swagger UI) from service annotations
- **Authentication and Authorization**: Validate API keys, JWTs, OAuth2 tokens, and mTLS certificates; enforce per-API, per-user, per-tenant policies
- **Rate Limiting and Throttling**: Apply configurable rate limits per API key, per method, per user, per tenant, with burst allowances and quota tracking
- **Request/Response Transformation**: Transform requests from external formats to internal service formats (e.g., REST to gRPC transcoding)
- **API Key Management**: Generate, rotate, revoke, and audit API keys; support public/private key pairs for signed requests
- **Usage Plans and Monetization**: Define usage tiers (free, pro, enterprise) with per-tier rate limits, feature access, and cost tracking
- **Caching**: Cache API responses at the gateway level for idempotent GET endpoints with configurable TTLs
- **CORS Management**: Apply per-API CORS policies (allowed origins, methods, headers, credentials)
- **Request Validation**: Validate incoming requests against OpenAPI schemas before forwarding to the backend
- **Response Compression**: Compress responses with gzip/brotli based on client Accept-Encoding headers
- **Circuit Breaking**: Detect backend service failures and return cached or degraded responses
- **API Analytics**: Collect and expose API usage data: request count, latency, error rate, user demographics, endpoint popularity
- **Developer Portal**: Provide a self-service portal where developers can discover APIs, read documentation, get API keys, test endpoints, and view usage
- **API Health Monitoring and SLA Tracking**: Monitor API endpoint health and enforce SLAs based on latency and availability

### 1.4 Non-Responsibilities
- The API Manager does NOT implement business logic for any specific API (it routes to internal services that implement the logic)
- The API Manager does NOT store user data (it delegates user management to the Identity service)
- The API Manager does NOT execute AI model inference (it routes to the Model Router)
- The API Manager does NOT manage tool execution (it routes to the Tool Manager)
- The API Manager does NOT implement content moderation (it delegates to the Validation System)
- The API Manager does NOT manage third-party integrations (it provides the gateway, not the integration logic)
- The API Manager does NOT manage internal service discovery (it uses a service registry maintained by the AIOS Registry)
- The API Manager does NOT perform database queries or data storage operations

---

## 2. Architecture, Internal Components, External Components

### 2.1 Overall Architecture
The API Manager is built as a multi-layer Gateway-Engine architecture. The outermost layer is a high-performance proxy (based on a custom Go HTTP server) that handles TLS termination, request parsing, and routing. The inner engine layer handles authentication, rate limiting, transformation, and validation. The management layer handles API lifecycle, documentation, key management, and analytics.

```
+------------------------------------------------------------------+
|                      AIOS API Manager Suite                        |
|                                                                     |
|  +------------------------------------------------------------+   |
|  |                  External API Gateway Layer                  |   |
|  |                                                              |   |
|  |  +--------------+ +--------------+ +--------------+ +------+ |   |
|  |  | TLS           | | Request     | | Rate         | | Auth  | |   |
|  |  | Termination   |->| Router      |->| Limiter      |->| Handler| |   |
|  |  +--------------+ +--------------+ +--------------+ +------+ |   |
|  |  +--------------+ +--------------+ +--------------+ +------+ |   |
|  |  | CORS         | | Request     | | Response    | | Error | |   |
|  |  | Handler      |->| Transformer |->| Transformer |->| Handler| |   |
|  |  +--------------+ +--------------+ +--------------+ +------+ |   |
|  |  +--------------+ +--------------+ +--------------+ +------+ |   |
|  |  | Request      | | Cache       | | Circuit     | | Load  | |   |
|  |  | Validator    |->| Lookup      |->| Breaker      |->| Balancer| |   |
|  |  +--------------+ +--------------+ +--------------+ +------+ |   |
|  +------------------------------------------------------------+   |
|                                |                                    |
|  +------------------------------------------------------------+   |
|  |                 Internal Service Routing Layer               |   |
|  |  +--------------+ +--------------+ +--------------+ +------+ |   |
|  |  | Service      | | Protocol     | | gRPC         | | HTTP  | |   |
|  |  | Registry     |->| Transcoders  |->| Client Pool  |->| Pool   | |   |
|  |  +--------------+ +--------------+ +--------------+ +------+ |   |
|  +------------------------------------------------------------+   |
|                                |                                    |
|  +------------------------------------------------------------+   |
|  |                      Management Layer                        |   |
|  |  +--------------+ +--------------+ +--------------+ +------+ |   |
|  |  | API          | | API          | | Developer    | | Key   | |   |
|  |  | Lifecycle    |->| Documentation|->| Portal        |->| Manager| |   |
|  |  +--------------+ +--------------+ +--------------+ +------+ |   |
|  |  +--------------+ +--------------+ +--------------+ +------+ |   |
|  |  | Usage Plans  | | Analytics    | | SLA Monitor  | |Usage  | |   |
|  |  | and Quotas   |->| Collector    |->| (Prometheus) |->|Export | |   |
|  |  +--------------+ +--------------+ +--------------+ +------+ |   |
|  +------------------------------------------------------------+   |
+------------------------------------------------------------------+
```

### 2.2 Internal Components

#### 2.2.1 TLS Terminator
Terminates TLS 1.3 (minimum) connections. Supports SNI-based multi-domain certificate hosting. Certificates are loaded from the Secrets Vault and hot-reloaded on rotation. Supports mutual TLS (mTLS) for select API routes that require client certificate authentication. All connections are decrypted before reaching the Request Router.

#### 2.2.2 Request Router
The primary routing engine. It examines the incoming request's method, path, headers (Host, Content-Type, Accept, X-API-Version), and query parameters to determine which internal service and endpoint should handle the request. The router maintains a trie-based routing table for O(1) path matching. It supports path parameters, wildcards, priority-based route ordering, and default routes. The routing table is periodically synced from the AIOS Configuration system.

#### 2.2.3 Authentication Handler
Validates the authentication credentials of the incoming request. Supports multiple authentication methods:
- **API Key**: The key is extracted from the Authorization Bearer header, hashed with bcrypt, and compared against the stored hash. The key may have scoped permissions.
- **JWT**: The JWT is validated for signature, expiry (exp claim), issuer (iss), audience (aud), and scope. The JWK set is fetched from the Identity Service and cached.
- **OAuth2**: The bearer token is introspected against the OAuth2 provider's introspection endpoint.
- **mTLS**: The client certificate is validated against the internal CA, and the CN is used as the identity.
The handler returns a structured identity context including user_id, tenant_id, roles, permissions, and api_key_id.

#### 2.2.4 Rate Limiter
A multi-dimensional rate limiter that checks and enforces:
- Per-API-key rate limit (requests per second/minute/hour)
- Per-user rate limit
- Per-tenant aggregate rate limit
- Per-API-method rate limit
- Burst allowances (configurable, default 2x the sustained rate)
Uses sliding window counters stored in Redis. Each counter is a Redis sorted set with score = timestamp. The window is slid by removing entries older than the window duration. Tokens are consumed atomically with Lua scripts.

#### 2.2.5 Request Validator
Validates incoming requests against the API's OpenAPI 3.0 schema definition. Checks required parameters, parameter types and patterns, request body schema conformance, header constraints, and query parameter constraints. Returns a structured 400 error with field-level detail on schema violations.

#### 2.2.6 Request/Response Transformer
Handles protocol and format transformations:
- REST to gRPC transcoding (using the gRPC-HTTP transcoding spec)
- Content negotiation between JSON, MessagePack, Protocol Buffers, and YAML
- Header injection and removal based on API configuration
- Response rewriting that strips backend internal fields from public responses

#### 2.2.7 Cache Layer
Caches responses for idempotent requests (GET, HEAD, and POST if marked idempotent). Cache key is composed of method, path, query, and tenant hash. TTL is configurable per API route (default 60s, max 300s). Storage is Redis. Cache invalidation occurs on backend mutations to the same resource path, TTL expiry, or explicit invalidation via API. Respects Cache-Control headers from backend responses.

#### 2.2.8 Circuit Breaker
Each upstream service has a circuit breaker that tracks failure rate in a 60-second sliding window. Threshold is 50% failure rate with at least 10 requests. Open state duration is 30 seconds. In half-open state, one probe request is allowed. In open state, the manager returns a cached response (if available) or a 503 with Retry-After header.

#### 2.2.9 Load Balancer
Distributes requests to upstream service instances using weighted round-robin with least-connections awareness. Health-aware routing forwards only to HEALTHY instances. On backend 5xx, retries up to 2 times with a different instance. Supports optional sticky sessions based on a stickiness header or cookie.

#### 2.2.10 API Lifecycle Manager
Manages the complete lifecycle of every API: CREATE (register a new API definition), PUBLISH (release to API Portal), DEPRECATE (mark as deprecated with Deprecation headers), RETIRE (remove from gateway, return 410 Gone), ARCHIVE (move to cold storage). Each lifecycle transition emits an api.lifecycle event to the Event System.

#### 2.2.11 Developer Portal
A web application that serves as the developer experience entry point. Includes an API browser with search and categorization, an interactive API explorer (Swagger UI), API key management (generate, list, revoke, view usage), usage analytics (request counts, error rates, latency percentiles, cost estimates), and auto-generated documentation with code snippets in Python, JavaScript, Go, Rust, and cURL.

#### 2.2.12 Analytics Collector
Collects and aggregates API usage data. For every API call, a request log entry is collected and aggregated into 1-minute, 5-minute, 1-hour, and 1-day buckets. Data includes unique users, unique API keys, request count, error count, P50/P95/P99 latency, response size, and cost incurred. Exposes data via the monitoring API and the Developer Portal.

#### 2.2.13 API Key Manager
Handles the full lifecycle of API keys. Generation creates a new API key with a key ID (prefix) and a secret key (full key). The key ID is returned to the user; the secret key is hashed with bcrypt and stored. Rotation generates a new secret for the same key ID with a 5-minute grace period for the old key. Revocation immediately invalidates a key. Keys can have optional TTL and scopes (specific API methods, IP ranges, referer domains).

### 2.3 External Components
- Authentication System: Validates JWT/OAuth2 tokens and provides identity context
- Authorization System: Checks permissions on API resources
- Secrets Vault: Stores TLS certificates and API key hash salt
- Configuration System: Stores API definitions, routing tables, rate limits, CORS configs
- Redis Cluster: Rate limiter state, cache, session stickiness
- Identity Service (IDP): Provides JWKS, OAuth2, and user management
- Monitoring System (Prometheus): Receives metrics
- Logging System (Loki/ELK): Receives audit logs
- Analytics Database (ClickHouse): Stores aggregated usage analytics
- Internal Services: Model Router, Tool Manager, Workflow Engine, Agent Manager, Memory System

---

## 3. Dependencies, Inputs, Outputs

### 3.1 Internal Dependencies
- Authentication System (identity verification)
- Authorization System (permission evaluation)
- Secrets Vault (certificates, key salt)
- Configuration System (API definitions, policies)
- Event System (lifecycle events, analytics events)

### 3.2 External Dependencies
- Redis Cluster (rate limiter, cache, session state)
- Identity Provider (OAuth2, JWT verification)
- Developer Portal Database (API key metadata, usage data)
- Analytics Database (ClickHouse for long-term usage storage)

### 3.3 Inputs

#### 3.3.1 Incoming Request
- Method: GET/POST/PUT/DELETE/PATCH
- Path: /api/v1/{resource}
- Headers: Authorization, Content-Type, X-API-Version, User-Agent
- Body: JSON, MessagePack, Protocol Buffers, or YAML as indicated by Content-Type

#### 3.3.2 API Definition (Management)
```json
{
  "name": "chat-completions",
  "version": "2024-01-01",
  "visibility": "public",
  "routes": [
    {
      "method": "POST",
      "path": "/api/v1/chat/completions",
      "target_service": "model-router",
      "target_method": "RouteChatCompletion"
    }
  ],
  "rate_limits": {"per_second": 100, "per_minute": 6000},
  "authentication": "required",
  "cors": {
    "allowed_origins": ["*"],
    "allowed_methods": ["GET", "POST"],
    "allowed_headers": ["Authorization", "Content-Type"]
  }
}
```

### 3.4 Outputs

#### 3.4.1 Success Response
- Status: 200 OK
- Headers: Content-Type, X-Request-Id, X-RateLimit-Remaining, X-RateLimit-Reset
- Body: JSON response from the internal service

#### 3.4.2 Error Response
```json
{
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "API rate limit exceeded. Retry after 15 seconds.",
    "status": 429,
    "retry_after_seconds": 15,
    "request_id": "uuid"
  }
}
```

---

## 4. Data Structures, Execution Flow, State Management

### 4.1 Core Data Structures

#### 4.1.1 APIDefinition
- ID, Name, Version (date-based), Visibility (PUBLIC, INTERNAL, PRIVATE)
- Routes array: method, path, target_service, target_method, middlewares, timeout_ms
- OpenAPISpec, RateLimits, AuthConfig, CORSConfig, CacheConfig, CircuitBreaker
- State: DESIGN, ACTIVE, DEPRECATED, RETIRED

#### 4.1.2 APIKey
- ID (key prefix like "aks_abc123"), KeyHash (bcrypt), OwnerID, TenantID
- Scopes, IPWhitelist, ReferrerWhitelist, ExpiresAt, CreatedAt, LastUsedAt, Revoked

#### 4.1.3 RateLimitState
- Key: composite "apikey:{id}:{window}"
- Count, WindowStart, WindowEnd

#### 4.1.4 IdentityContext
- UserID, TenantID, Roles, Permissions, APIKeyID, AuthMethod, AuthenticatedAt

### 4.2 Execution Flow

#### 4.2.1 Request Processing Flow
1. **TLS Termination**: Decrypt request, pass plaintext HTTP to gateway
2. **Connection Context**: Create context with request_id, client_ip, protocol
3. **Route Lookup**: Match request to route using trie-based routing table; 404 if no match
4. **API Definition Lookup**: Fetch API definition from in-memory cache
5. **CORS Check**: For OPTIONS preflight, check Origin against allowed origins
6. **Authentication**: Extract and validate credentials; 401 if missing or invalid
7. **Authorization**: Check permissions for API method; 403 if unauthorized
8. **Rate Limiting**: Check all applicable limits; 429 if exceeded with Retry-After
9. **Request Validation**: Validate against OpenAPI schema; 400 if invalid
10. **Cache Lookup**: For cacheable requests, check cache; return cached response if hit
11. **Request Transformation**: Transform request format if needed (REST to gRPC, content negotiation)
12. **Load Balancing**: Select healthy upstream instance
13. **Circuit Breaker Check**: If circuit is open for the upstream, return cached or 503
14. **Forward Request**: Send transformed request to upstream service
15. **Response Transformation**: Transform response back to external format
16. **Cache Write**: If cacheable, store response in cache
17. **Audit Log**: Record request and response metadata in audit log
18. **Analytics**: Update usage counters and metrics
19. **Response**: Return response to client

### 4.3 State Management

- **Gateway State**: In-memory routing table (trie) updated from Configuration System via event subscription. Atomic pointer swap for lock-free reads.
- **Rate Limiter State**: Redis sorted sets with TTL. Sliding window purges old entries on each read/write.
- **Circuit Breaker State**: In-memory per-upstream counters with periodic flushing to Redis for crash recovery.
- **Cache State**: Redis with TTL. No local in-memory cache (all instances share the same Redis cache).
- **API Definition State**: In-memory copy of all active API definitions, refreshed on configuration change.

---

## 5. Communication, APIs, Events, Queues

### 5.1 Communication Protocols
- **External**: HTTPS (TLS 1.3), gRPC (TLS 1.3), WebSocket (WSS)
- **Internal**: gRPC (mTLS), HTTP/2 (mTLS)
- **Management**: REST over HTTPS (admin API)

### 5.2 Exposed APIs

| Method | Path | Description |
|---|---|---|
| Any | /api/v1/{service}/{resource} | Public API routing endpoints |
| GET | /api/v1/health | Health check |
| GET | /api/v1/metrics | Prometheus metrics |
| GET | /api/v1/openapi.json | Combined OpenAPI spec |
| POST | /admin/v1/apis | Register new API |
| PUT | /admin/v1/apis/{id} | Update API definition |
| DELETE | /admin/v1/apis/{id} | Retire API |
| GET | /admin/v1/apis | List APIs |
| POST | /admin/v1/api-keys | Generate API key |
| DELETE | /admin/v1/api-keys/{id} | Revoke API key |
| GET | /admin/v1/api-keys | List API keys |
| GET | /admin/v1/analytics | Get usage analytics |
| POST | /admin/v1/cache/invalidate | Invalidate cache |

### 5.3 Events

| Event | Payload | Trigger |
|---|---|---|
| api.lifecycle.created | {api_id, name, version} | API registered |
| api.lifecycle.published | {api_id, version} | API published |
| api.lifecycle.deprecated | {api_id, version, deprecation_date} | API deprecated |
| api.lifecycle.retired | {api_id, version} | API retired |
| api.rate_limit.exceeded | {api_key_id, method, path} | Rate limit hit |
| api.auth.failed | {method, reason, client_ip} | Authentication failure |
| api.circuit_breaker.open | {target_service} | Circuit breaker opens |
| api.circuit_breaker.closed | {target_service} | Circuit breaker recovers |

### 5.4 Queues

- **Request Queue**: Internal buffering when upstream services are at capacity. FIFO with per-tenant isolation.
- **Analytics Queue**: Asynchronous processing of usage data. Batched and written to ClickHouse every 5 seconds.
- **Cache Invalidation Queue**: Cache invalidation requests are processed asynchronously to avoid blocking the request path.

---

## 6. Caching, Memory, Persistence

### 6.1 Caching

#### 6.1.1 Response Cache
- Backend: Redis
- Key: {method}:{path}:{query}:{tenant_hash}
- TTL: 60s default, configurable per route (1s to 300s)
- Invalidation: On backend mutation to the same resource path
- Cache-Control: Respects no-cache, max-age, must-revalidate from backends

#### 6.1.2 OpenAPI Schema Cache
- Backend: In-memory
- Key: {api_id}:{version}
- TTL: Indefinite (invalidated on API definition update)
- Purpose: Avoids loading and parsing schemas on every validation

#### 6.1.3 Auth Token Cache
- Backend: In-memory TTL cache
- TTL: 80% of token expiry
- Purpose: Caches decoded JWTs and JWK sets to avoid repeated verification

#### 6.1.4 Routing Table Cache
- Backend: In-memory trie data structure
- TTL: Event-driven (updated on configuration change)
- Purpose: O(1) path matching for every request

### 6.2 Memory
- Routing table: ~500KB for 1,000 routes
- Schema cache: ~50KB per API, for 200 APIs = ~10MB
- Auth token cache: ~2KB per cached token, up to ~10MB
- Total estimated: 50MB–150MB per instance

### 6.3 Persistence
- API definitions are stored in the Configuration System (etcd, Postgres)
- API key metadata is stored in the Developer Portal database (Postgres)
- Usage analytics are stored in ClickHouse
- Cache data is stored in Redis (ephemeral)
- Rate limiter state is in Redis (ephemeral)
- No local disk storage is required

---

## 7. Validation, Retry Logic, Error Recovery

### 7.1 Validation

#### 7.1.1 Request Validation
- OpenAPI schema validation for all requests
- Type checking: integer bounds, string patterns, enum values
- Required header presence validation
- Content-Type validation against allowed types per route
- Request size validation (max 10MB per request)

#### 7.1.2 API Definition Validation
- Route paths must be valid URI templates
- Target services must exist in the Service Registry
- Rate limit values must be positive integers
- CORS origins must be valid URLs or wildcards
- API version format must be YYYY-MM-DD or semver
- OpenAPI spec must be valid JSON Schema

#### 7.1.3 Run-Time Validation
- Upstream service responses are validated for schema conformance (optional, for debugging)
- Response size is validated (max 50MB per response)
- Response content type is validated against client Accept header

### 7.2 Retry Logic

#### 7.2.1 Retry on Upstream Failure
- Triggered when: upstream returns 5xx or connection error
- Max retries: 2 (configurable per route)
- Backoff: 100ms fixed delay
- The failed instance is excluded from retry selection
- If the circuit breaker is open, the request is not retried (cached or 503 instead)

#### 7.2.2 Retry on Rate Limit
- Not retried (the client receives 429 with Retry-After)
- The client is expected to respect the Retry-After header

#### 7.2.3 Retry on Timeout
- Triggered when: upstream does not respond within the configured timeout
- Max retries: 1 (to avoid overwhelming a struggling upstream)
- Different instance is selected for the retry

### 7.3 Error Recovery

#### 7.3.1 Gateway Internal Error
- If the API Manager itself encounters an internal error (panic, OOM, nil pointer), the error is caught by a recovery middleware
- A 500 error with a generic message is returned (no internal details exposed)
- The error is logged with a full stack trace
- The request_id is included in the response to allow log correlation

#### 7.3.2 Upstream Service Unreachable
- If all upstream instances are unreachable, return 503 Service Unavailable
- Include estimated retry_after_seconds (default 30)
- Emit circuit_breaker.open event
- Return cached response if available (with Age header indicating staleness)

#### 7.3.3 Configuration Error
- If the configuration system returns invalid data, the last known valid configuration is preserved
- An alert is sent to the operations team
- The invalid configuration is rejected and logged

---

## 8. Security, Authentication, Authorization, Permissions

### 8.1 Authentication

#### 8.1.1 API Key Authentication
- Keys are 64-character random hex strings with a prefix (e.g., "aks_abc123...")
- Keys are hashed with bcrypt (cost factor 12) before storage
- The full key is shown only once at creation time
- Keys can be scoped to specific API methods and IP ranges
- Revoked keys are checked on every request (with a 1-minute cache of revoked key IDs)

#### 8.1.2 JWT Authentication
- JWTs are validated for signature (RS256/ES256) using the JWKS endpoint
- Standard claims: exp, nbf, iss, aud, sub, scope
- JWT is cached until its exp minus 10% to avoid repeated verification
- JWT refresh is handled by the client (the API Manager does not issue tokens)

#### 8.1.3 OAuth2 Authentication
- Bearer tokens are introspected against the configured OAuth2 introspection endpoint
- The introspection result is cached for 5 minutes (configurable)
- Supports any OAuth2-compliant provider (Auth0, Okta, Keycloak, custom)

#### 8.1.4 mTLS Authentication
- Client certificates are validated against the AIOS internal CA
- The certificate CN is used as the identity
- Certificate revocation lists (CRLs) are checked on each connection
- Only supported for internal service-to-service communication

### 8.2 Authorization

#### 8.2.1 Scope-Based Authorization
- Each API key has a list of scopes (e.g., "chat-completions:write", "models:read")
- Each API route requires specific scopes
- Wildcards are supported: "chat-completions:*" grants access to all methods on that API
- Deny rules take precedence over allow rules

#### 8.2.2 Tenant Isolation
- Each API key is scoped to a single tenant
- Tenant A cannot access resources belonging to Tenant B
- Rate limits are enforced per tenant
- Analytics are reported per tenant

#### 8.2.3 IP-Based Access Control
- API keys can be restricted to specific source IPs or CIDR ranges
- Requests from non-whitelisted IPs are rejected with 403
- Also supports geo-IP blocking (allow/deny by country code)

### 8.3 Permissions

- `api.gateway.access` — basic access to the API gateway
- `api.gateway.admin` — administrative access (manage APIs, keys, view analytics)
- `api.key.create` — create new API keys
- `api.key.revoke` — revoke existing API keys
- `api.{name}.{method}` — access a specific API method (e.g., api.chat-completions.POST)
- `api.metrics.view` — view API metrics and analytics

### 8.4 TLS and Encryption
- All external traffic uses TLS 1.3 minimum
- All internal traffic uses mTLS (mutual TLS 1.3)
- Certificates are automatically rotated by the Secrets Vault integration
- Cipher suites: TLS_AES_128_GCM_SHA256, TLS_AES_256_GCM_SHA384
- HSTS is enabled with max-age=31536000

---

## 9. Monitoring, Logging, Metrics, Tracing

### 9.1 Metrics

| Metric | Type | Labels | Description |
|---|---|---|---|
| aios_api_requests_total | Counter | method, path, status_code, tenant | Total API requests |
| aios_api_request_duration_ms | Histogram | method, path, status_code | Request latency |
| aios_api_rate_limit_exceeded_total | Counter | path, tenant | Rate limit exceeded |
| aios_api_auth_failed_total | Counter | method, reason | Auth failures |
| aios_api_cache_hit_total | Counter | path | Cache hit count |
| aios_api_cache_miss_total | Counter | path | Cache miss count |
| aios_api_upstream_duration_ms | Histogram | target_service | Upstream service latency |
| aios_api_upstream_errors_total | Counter | target_service, status_code | Upstream errors |
| aios_api_circuit_breaker_state | Gauge | target_service | 0=CLOSED, 1=HALF_OPEN, 2=OPEN |
| aios_api_active_connections | Gauge | - | Active connections |
| aios_api_request_size_bytes | Histogram | method | Request payload size |

### 9.2 Logging
- Structured JSON logging at INFO, WARN, ERROR levels
- Every API request generates an INFO log entry at completion
- Authentication failures generate WARN log entries
- Internal errors generate ERROR log entries with stack traces
- Log fields: timestamp, level, request_id, method, path, status_code, duration_ms, tenant_id, user_id, client_ip, user_agent
- PII redaction: credit card numbers, email addresses, and API keys are redacted with *** before logging
- Log shipping: real-time streaming to Loki via Promtail
- Log retention: 30 days (INFO), 90 days (ERROR), 7 days (DEBUG)

### 9.3 Tracing
- OpenTelemetry distributed tracing
- Trace context propagated from client via W3C Trace Context headers
- Spans:
  - api_gateway.handle_request (root)
  - api_gateway.authenticate
  - api_gateway.rate_limit
  - api_gateway.validate
  - api_gateway.proxy_upstream
  - api_gateway.transform_response
- All spans include method, path, status_code, tenant_id attributes
- Sampling: 100% for errors, 10% for success
- Export: OTLP to Jaeger or Tempo

### 9.4 Health Checks
- Liveness: /health/live — returns 200 if the process is running
- Readiness: /health/ready — returns 200 if all downstream services are reachable and the routing table is loaded
- Detailed: /health/detail — per-upstream health status, cache hit rates, rate limiter status

---

## 10. Scalability, Performance, Optimization

### 10.1 Scalability

#### 10.1.1 Horizontal Scaling
- The API Gateway is stateless; multiple instances can be deployed behind a load balancer
- Shared state (rate limiter, cache, circuit breaker) is stored in Redis
- Configuration changes propagate to all instances via the event system
- Instances can be added or removed without downtime

#### 10.1.2 Scaling Limits
- Maximum instances: 64
- Maximum routes per instance: 10,000
- Maximum concurrent connections per instance: 50,000
- Maximum APIs: 1,000
- Maximum API keys: 100,000

#### 10.1.3 Auto-Scaling
- Metric: CPU utilization and active connections
- Scale up when: CPU > 70% or active connections > 30,000 for 60 seconds
- Scale down when: CPU < 30% and active connections < 10,000 for 5 minutes
- Minimum instances: 2 (HA)
- Maximum instances: 64

### 10.2 Performance Targets

| Metric | Target | Degradation Alarm |
|---|---|---|
| Gateway overhead P50 | < 2ms | > 5ms |
| Gateway overhead P99 | < 5ms | > 20ms |
| Rate limit check P99 | < 1ms | > 5ms |
| Auth validation P99 | < 10ms | > 50ms |
| Request validation P99 | < 5ms | > 20ms |
| Throughput per instance | 10,000 req/s | - |

### 10.3 Optimization Techniques

- **Connection Pooling**: HTTP and gRPC connections to upstream services are pooled with keepalive. Max idle: 256 per upstream.
- **Zero-Copy Request Handling**: Request bodies are read directly from the socket buffer without intermediate copies in the gateway path.
- **Trie-Based Routing**: O(1) path matching regardless of route table size.
- **Precompiled OpenAPI Validators**: Schemas are compiled to fast evaluators on API registration.
- **Response Compression**: Dynamic gzip/brotli compression based on client Accept-Encoding.
- **HTTP/2 Multiplexing**: Upstream connections use HTTP/2 for multiplexing multiple requests over a single connection.
- **Brotli Static Dictionary**: Common response patterns (error messages, API metadata) use a shared brotli dictionary for better compression.

### 10.4 Load Shedding

When the system is under extreme load:
1. Non-critical API routes (rate-limited, low priority) are degraded first
2. Cached responses are served for all cacheable requests
3. Authentication token verification is degraded (use cached JWK sets only, skip introspection)
4. Request validation is skipped for trusted tenants
5. New connections are accepted with a longer timeout to naturally slow down clients

---

## 11. Testing Strategy, Risk Analysis

### 11.1 Testing Strategy

#### 11.1.1 Unit Tests
- Test each middleware in isolation (CORS, auth, rate limiter, validator, transformer)
- Test routing table construction and path matching
- Test cache key generation and invalidation logic
- Test circuit breaker state transitions
- Coverage target: > 90%

#### 11.1.2 Integration Tests
- End-to-end request flow with a mock upstream service
- Test authentication with valid and invalid API keys, JWTs, OAuth2 tokens
- Test rate limiter with exact boundary conditions (at limit, over limit, burst)
- Test CORS preflight and actual request handling
- Test cache hit and miss scenarios
- Test request transformation (REST to gRPC transcoding)
- Coverage target: > 80%

#### 11.1.3 Performance Tests
- Benchmark gateway overhead with 10,000 concurrent requests
- Benchmark rate limiter throughput (should handle 100,000 checks/sec)
- Benchmark schema validation throughput (should handle 10,000 validations/sec)
- Target: P99 gateway overhead < 5ms

#### 11.1.4 Chaos Tests
- Simulate Redis failure: verify rate limiter degrades to local counting
- Simulate upstream outage: verify circuit breaker behavior
- Simulate configuration change storm: verify no routing table corruption
- Simulate TLS renegotiation flood: verify connection handling

#### 11.1.5 Security Tests
- API key brute force attempts
- JWT forgery attempts
- SQL injection via query parameters (if any)
- Request smuggling via malformed headers
- Path traversal attacks in URL

### 11.2 Risk Analysis

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Rate limiter state corruption | Low | Medium | Redis replication, Lua atomicity |
| Auth bypass via malformed JWT | Low | Critical | Strict JWT validation library, JWK pinning |
| TLS certificate expiry | Medium | High | Auto-rotation via Secrets Vault, monitoring alert 30 days before expiry |
| Upstream slow loris attack | Low | Medium | Request timeout, connection limits per IP |
| API key leak | Medium | High | Key rotation support, IP whitelisting, audit logging |
| Configuration system unavailable | Low | Medium | Last known good configuration cached locally |

---

## 12. Failure Scenarios, Recovery Procedures

### 12.1 Redis Failure
**Scenario**: The Redis cluster is unavailable (network partition, crash, maintenance).
**Detection**: Rate limiter and cache operations return connection errors.
**Response**:
- Rate limiter: Degrades to local in-memory token bucket with per-instance counting. Over-counting is accepted (some tenants may exceed limits briefly).
- Cache: All cache reads return MISS. All cache writes are dropped.
- Circuit breaker: Degrades to local in-memory state.
- Session stickiness: Lost; requests may be routed to any instance.
**Recovery**: When Redis is restored, all in-memory states are flushed and Redis-backed state takes over immediately.

### 12.2 Upstream Service Outage
**Scenario**: An internal service (e.g., Model Router) is unavailable.
**Detection**: Proxied request returns connection refused or timeout.
**Response**:
- Circuit breaker opens for the affected service.
- Cached responses are served for GET requests where available.
- Non-cacheable requests receive 503 Service Unavailable.
- Health check endpoint shows the upstream as UNHEALTHY.
**Recovery**: On successful health check probe, the circuit breaker closes and traffic resumes.

### 12.3 TLS Certificate Expiry
**Scenario**: The gateway's TLS certificate has expired or is about to expire.
**Detection**: Monitoring alert fires 30 days before expiry; clients report TLS errors.
**Response**:
- If the certificate has already expired, clients receive TLS handshake errors.
- The API Manager auto-renews certificates from the Secrets Vault on a 24-hour check cycle.
- If auto-renewal fails, an alert is sent to the operations team.
**Recovery**: Manual certificate renewal via the Secrets Vault API or replacement of the certificate file.

### 12.4 Configuration Corruption
**Scenario**: A configuration update introduces invalid routes or API definitions.
**Detection**: Configuration validation fails during the update.
**Response**:
- The update is rejected and the previous configuration remains active.
- An alert with the validation errors is sent to the operations team.
- The invalid configuration is logged for debugging.

---

## 13. Operational Limits, Maintenance

### 13.1 Operational Limits

| Parameter | Limit | Behavior When Exceeded |
|---|---|---|
| Max request body size | 10 MB | Return 413 Payload Too Large |
| Max response body size | 50 MB | Truncate response with warning header |
| Max concurrent connections | 50,000 | Reject new connections |
| Max rate limit per API key | 10,000 req/s | Key cannot be configured higher |
| Max headers per request | 100 | Return 400 Bad Request |
| Max header size | 32 KB | Return 431 Header Fields Too Large |
| Max URL length | 8 KB | Return 414 URI Too Long |
| Max API definition size | 1 MB | Reject registration |
| Max cache entry size | 1 MB | Do not cache oversized responses |
| Rate limit window min | 1 second | - |
| Rate limit window max | 1 day | - |

### 13.2 Maintenance

#### 13.2.1 Zero-Downtime Updates
- Rolling updates via Kubernetes Deployment with pod disruption budget
- Health check gates ensure only ready instances receive traffic
- Active connections are drained over 30 seconds before an instance is terminated
- Configuration updates are hot-reloaded without restart

#### 13.2.2 API Deprecation Policy
- APIs are deprecated for at least 90 days before retirement
- Deprecated APIs return Deprecation and Sunset HTTP headers
- API consumers are notified via the Developer Portal and email
- Migration guides are provided for breaking changes

#### 13.2.3 Rate Limit Review
- Rate limits are reviewed monthly based on analytics data
- Automatic rate limit adjustment can be enabled for trusted tenants (within configurable bounds)
- Rate limit overrides are logged and require admin approval

---

## 14. Future Improvements, Examples

### 14.1 Future Improvements

#### 14.1.1 GraphQL Federation
Support GraphQL as a first-class protocol: route GraphQL queries to appropriate backend services and aggregate responses.

#### 14.1.2 WebSocket Management
Native WebSocket lifecycle management: connection pooling, message routing, backpressure handling, and automatic reconnection.

#### 14.1.3 AI-Assisted API Security
Use AI models to detect anomalous API usage patterns (credential stuffing, DDoS, data scraping) and automatically adjust rate limits or block IPs.

#### 14.1.4 SDK Generation
Auto-generate client SDKs in popular languages from OpenAPI specs, including authentication handling and retry logic.

#### 14.1.5 Canary Deployments
Route a percentage of traffic to a new version of an API for A/B testing and gradual rollout with automatic rollback.

#### 14.1.6 Multi-Protocol Webhooks
Support incoming webhooks in multiple protocols (HTTP, gRPC, message queue) with automatic retry, deduplication, and delivery guarantees.

### 14.2 Examples

#### Example 1: Simple API Proxy
```
Client: POST /api/v1/chat/completions (Authorization: Bearer aks_abc...)
Gateway: Validates API key -> Checks rate limit -> Forwards to Model Router
Model Router: Processes request -> Returns response
Gateway: Transforms response -> Returns to client
```

#### Example 2: Cache Hit
```
Client: GET /api/v1/models (Authorization: Bearer aks_abc...)
Gateway: Validates key -> Checks cache -> Cache HIT
Response: Returns cached model list (X-Cache: HIT, Age: 45)
```

#### Example 3: Rate Limit Exceeded
```
Client: POST /api/v1/chat/completions (100th request in 1 minute)
Gateway: Validates key -> Rate limiter returns EXCEEDED
Response: 429 with X-RateLimit-Remaining: 0, Retry-After: 45
```

#### Example 4: Circuit Breaker Active
```
Client: POST /api/v1/chat/completions
Gateway: Forwards to Model Router -> Connection timeout -> Circuit Opens
Response: 503 Service Unavailable (Retry-After: 30)
```

#### Example 5: API Deprecation
```
Client: POST /api/v0/chat/completions
Gateway: Routes request -> Adds headers
Response: 200 OK with Deprecation: true, Sunset: Sat, 01 Jan 2025 00:00:00 GMT
```

---

## 15. Acceptance Criteria, Definition of Done

### 15.1 Acceptance Criteria

1. **Request Routing**: Every valid API request is correctly routed to the appropriate internal service based on the method and path. Verified by integration test.

2. **Authentication**: API requests without valid credentials are rejected with 401. API requests with valid credentials are allowed. Verified by integration test.

3. **Rate Limiting**: A client that exceeds the configured rate limit is rejected with 429 and a Retry-After header. The rate limit window resets correctly. Verified by integration test.

4. **Caching**: A GET request to a cacheable endpoint returns the cached response on subsequent calls within the TTL. The response includes X-Cache: HIT. Verified by integration test.

5. **Circuit Breaker**: After 50% of requests to an upstream fail, the circuit breaker opens and subsequent requests return 503. After the cooldown period, probe requests are allowed. Verified by chaos test.

6. **API Lifecycle**: A published API is accessible. A deprecated API returns Deprecation headers. A retired API returns 410. Verified by integration test.

7. **CORS**: Cross-origin requests are handled according to the API's CORS configuration. Preflight (OPTIONS) requests return the correct headers. Verified by integration test.

8. **Performance**: Gateway overhead is under 5ms P99 under 1,000 requests per second. Verified by performance test.

9. **Security**: API keys are never returned in responses or logs. All traffic is encrypted with TLS 1.3. Verified by security scan.

10. **Graceful Degradation**: When Redis is unavailable, the gateway continues to operate with degraded rate limiting and no caching. Verified by chaos test.

### 15.2 Definition of Done

- [ ] All acceptance criteria implemented and passing
- [ ] Unit test coverage > 90% for new code
- [ ] Integration tests covering happy path, error paths, edge cases
- [ ] Performance benchmarks show no regression
- [ ] Security review completed (no secrets in logs, all traffic encrypted, auth enforced)
- [ ] Monitoring metrics, logs, and traces added for all new features
- [ ] All API routes have OpenAPI documentation
- [ ] Developer portal reflects new APIs
- [ ] API lifecycle management processes are documented
- [ ] Rollback plan exists: previous version can be redeployed in under 5 minutes


## 16. Appendix: Rate Limiting Algorithms

### 16.1 Sliding Window Counter
The API Manager uses a sliding window algorithm for rate limiting. For each rate limit dimension, a Redis sorted set stores entry timestamps:
```
Key: rate_limit:{dimension}:{identifier}:{window_size}
Score/Member: timestamp
```
On each request:
1. Remove entries older than `now - window_size` (ZREMRANGEBYSCORE)
2. Count remaining entries (ZCARD)
3. If count >= limit, reject with 429
4. Otherwise, add current timestamp (ZADD) and allow

The window slides continuously, providing smooth rate limiting without the sharp reset boundaries of fixed-window algorithms.

### 16.2 Burst Allowance
Burst allowances are implemented using a token bucket that tops up at the sustained rate but can accumulate up to the burst size:
```
tokens = min(burst_size, tokens + (now - last_refill) * sustained_rate)
if tokens >= 1:
    tokens -= 1
    allow
else:
    reject
```
The token bucket state is stored in Redis with Lua-based atomic operations.

### 16.3 Concurrent Request Limit
In addition to rate-per-time limits, the API Manager enforces a concurrent request limit per API key. This is tracked using a Redis counter that is incremented on request start and decremented on request completion:
```
Key: concurrent:{api_key_id}
INCR -> if result > limit: DECR and reject
On completion: DECR
```
The concurrent counter has a TTL of 60 seconds to handle cleanup of abandoned requests.

## 17. Appendix: OpenAPI Schema Validation Details

The Request Validator uses a compiled OpenAPI 3.0 validator that supports:
- Schema compilation to fast evaluator at API registration time
- Deep validation of nested objects (up to 20 levels)
- Pattern validation (regex)
- Enum validation
- String format validation (date-time, email, uri, uuid, ipv4, ipv6)
- Integer/Number bounds (minimum, maximum, exclusiveMinimum, exclusiveMaximum)
- Array validation (minItems, maxItems, uniqueItems)
- Object validation (required, additionalProperties, minProperties, maxProperties)
- Composition (allOf, anyOf, oneOf, not) — up to 10 combiners
- $ref resolution (internal references only, up to 5 levels deep)

Validation errors are returned as:
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Request failed schema validation",
    "details": [
      {"path": "$.messages[0].content", "reason": "maxLength exceeded (2000 > 1024)"},
      {"path": "$.temperature", "reason": "value must be >= 0 and <= 2"}
    ]
  }
}
```

## 18. Appendix: Error Codes Reference

| Code | HTTP Status | Description | Retryable |
|---|---|---|---|
| AUTHENTICATION_REQUIRED | 401 | Authentication is required but no credentials provided | No |
| AUTHENTICATION_FAILED | 401 | The provided credentials are invalid or expired | No |
| AUTHORIZATION_FAILED | 403 | The authenticated identity lacks required permissions | No |
| RATE_LIMIT_EXCEEDED | 429 | Rate limit exceeded for this API key or tenant | Yes (after Retry-After) |
| VALIDATION_ERROR | 400 | Request failed schema validation | No |
| NOT_FOUND | 404 | The requested resource or route was not found | No |
| METHOD_NOT_ALLOWED | 405 | The HTTP method is not allowed for this route | No |
| UPSTREAM_ERROR | 502 | The upstream service returned an error | Yes |
| UPSTREAM_TIMEOUT | 504 | The upstream service did not respond in time | Yes |
| SERVICE_UNAVAILABLE | 503 | The API Manager or upstream service is unavailable | Yes |
| CORS_ORIGIN_DENIED | 403 | The request origin is not allowed by CORS policy | No |
| PAYLOAD_TOO_LARGE | 413 | The request body exceeds the maximum size | No |
| API_RETIRED | 410 | The requested API version has been retired | No |
| GATEWAY_INTERNAL_ERROR | 500 | An unexpected error occurred in the API gateway | No |

## 19. Appendix: Configuration Reference

| Key | Type | Default | Description |
|---|---|---|---|
| gateway.tls.min_version | string | 1.3 | Minimum TLS version |
| gateway.tls.cert_ref | string | secrets.gateway_tls_cert | TLS certificate reference |
| gateway.tls.key_ref | string | secrets.gateway_tls_key | TLS key reference |
| gateway.max_request_size_mb | int | 10 | Maximum request body size |
| gateway.max_response_size_mb | int | 50 | Maximum response body size |
| gateway.max_connections | int | 50000 | Maximum concurrent connections |
| gateway.max_headers | int | 100 | Maximum headers per request |
| gateway.max_header_size_kb | int | 32 | Maximum header size |
| gateway.request_timeout_s | int | 60 | Default request timeout |
| gateway.read_timeout_s | int | 30 | Socket read timeout |
| gateway.write_timeout_s | int | 60 | Socket write timeout |
| gateway.idle_timeout_s | int | 120 | Keepalive timeout |
| rate_limiter.default_per_second | int | 100 | Default rate limit per API key |
| rate_limiter.default_burst | int | 200 | Default burst allowance |
| rate_limiter.sliding_window_s | int | 60 | Rate limit window in seconds |
| rate_limiter.redis_key_ttl_s | int | 3600 | TTL for rate limiter Redis keys |
| cache.default_ttl_s | int | 60 | Default cache TTL |
| cache.max_ttl_s | int | 300 | Maximum cache TTL |
| cache.max_entry_size_mb | int | 1 | Maximum cache entry size |
| circuit_breaker.failure_threshold | float | 0.5 | Failure rate threshold (0.0 to 1.0) |
| circuit_breaker.min_requests | int | 10 | Min requests before circuit breaker activates |
| circuit_breaker.window_s | int | 60 | Failure tracking window |
| circuit_breaker.cooldown_s | int | 30 | Open state duration |
| cors.default_allowed_origins | string | * | Default CORS allowed origins |
| cors.max_age_s | int | 86400 | CORS preflight max age |

## 20. Appendix: Developer Portal API

The Developer Portal exposes its own API for programmatic access:

| Method | Path | Description |
|---|---|---|
| GET | /portal/v1/apis | List published APIs |
| GET | /portal/v1/apis/{id} | Get API details and documentation |
| GET | /portal/v1/apis/{id}/openapi | Get OpenAPI spec |
| POST | /portal/v1/keys | Generate a new API key |
| GET | /portal/v1/keys | List own API keys |
| DELETE | /portal/v1/keys/{id} | Revoke an API key |
| GET | /portal/v1/usage | View own usage analytics |
| POST | /portal/v1/keys/{id}/rotate | Rotate API key secret |

All Developer Portal API calls require authentication (API key or JWT). The portal uses the same Authentication system as the gateway.

## 21. Appendix: Deployment Architecture

The API Manager is deployed as a Kubernetes Deployment:
- Replicas: 2 minimum, 64 maximum (auto-scaled based on CPU and connections)
- Resource requests: 1 CPU, 1GB RAM
- Resource limits: 4 CPU, 4GB RAM
- Readiness probe: GET /health/ready (initial delay: 10s, period: 10s)
- Liveness probe: GET /health/live (initial delay: 15s, period: 20s)
- Service: LoadBalancer (external: ports 443/HTTPS and 80/HTTP redirect)
- Service: ClusterIP (internal: gRPC port 50051, admin port 8080)
- Ingress: For developer portal (portal.api.aios.io)
- HPA: CPU target 70%, memory target 80%, custom metric: active_connections > 30000
- PodDisruptionBudget: minAvailable 2
- ConfigMap: Gateway configuration (mounted and hot-reloaded)
- Secrets: TLS certificates (mounted from Vault via CSI driver)

## 22. Appendix: Request Flow Example (Full Trace)

A complete request flow through the API Manager:

1. Client establishes TCP connection to aios-api-gateway.aios.io:443
2. TLS handshake: Server presents certificate for *.aios.io, client verifies
3. Client sends: POST /api/v1/chat/completions with Authorization header
4. Gateway: TLS terminates, extracts HTTP request
5. Gateway: Generates request_id "req_a1b2c3d4"
6. Gateway: Route lookup matches POST /api/v1/chat/completions -> target: model-router
7. Gateway: CORS check (Origin matches allowed origins)
8. Gateway: Authentication extracts Bearer token, validates as API key, returns identity: {user: "usr_abc", tenant: "tnt_123", permissions: ["chat-completions:write"]}
9. Gateway: Authorization checks permission -> Allowed
10. Gateway: Rate limiter checks api_key:aks_abc:60s -> count = 42, limit = 100 -> Allowed (58 remaining)
11. Gateway: Request validator checks against OpenAPI spec -> Valid
12. Gateway: Cache lookup for GET (not applicable, this is POST) -> Skip
13. Gateway: Request transformer converts REST to gRPC: POST /api/v1/chat/completions -> model-router.RouteChatCompletion
14. Gateway: Circuit breaker check for model-router -> CLOSED
15. Gateway: Load balancer selects model-router-instance-3 (healthy, least connections)
16. Gateway: Forwards gRPC request to model-router-instance-3:50051
17. Model Router processes request, returns gRPC response
18. Gateway: Receives gRPC response, transforms back to REST JSON
19. Gateway: Writes audit log entry
20. Gateway: Updates analytics counters
21. Gateway: Returns HTTP 200 with JSON body, X-Request-Id, X-RateLimit-Remaining headers
22. Client receives response
Total gateway overhead: 3.2ms


## 23. Appendix: TLS Certificate Management

### 23.1 Certificate Sources
The API Manager supports multiple TLS certificate sources:
- **File**: PEM files mounted from Kubernetes secrets
- **Vault**: HashiCorp Vault PKI engine with automatic renewal
- **ACME**: Let's Encrypt automatic certificate provisioning (with DNS-01 challenge)
- **Custom CA**: For internal-only deployments

### 23.2 Automatic Renewal
For Vault-sourced certificates:
1. A background goroutine checks certificate expiry every 6 hours
2. If the certificate expires within 30 days, a renewal is triggered
3. The new certificate is fetched from Vault and stored in memory
4. Existing TLS connections continue with the old certificate (graceful transition)
5. New TLS connections use the new certificate immediately

### 23.3 Certificate Rotation
For forced rotation (compromise, policy change):
1. Operator triggers rotation via the admin API
2. All existing TLS connections are drained with a 60-second grace period
3. New certificates are loaded
4. New connections use the new certificates
5. Old connections are terminated after the grace period

## 24. Appendix: Rate Limiting Configuration Examples

### 24.1 Standard API Key Rate Limit
```json
{
  "api_key_id": "aks_abc123",
  "limits": {
    "per_second": 10,
    "per_minute": 600,
    "per_hour": 36000,
    "concurrent": 5
  },
  "burst_multiplier": 2
}
```

### 24.2 Enterprise Tier Rate Limit
```json
{
  "tenant_id": "tnt_enterprise_001",
  "limits": {
    "per_second": 1000,
    "per_minute": 60000,
    "per_hour": 3600000,
    "concurrent": 100
  },
  "burst_multiplier": 3,
  "enforcement": "soft"  // soft = warning headers, no rejections
}
```

### 24.3 Per-Endpoint Rate Limit
```json
{
  "route": "POST /api/v1/chat/completions",
  "limits": {
    "per_second": 500
  },
  "overrides": [
    {"tenant_id": "tnt_free", "per_second": 10},
    {"tenant_id": "tnt_pro", "per_second": 100},
    {"tenant_id": "tnt_enterprise", "per_second": 1000}
  ]
}
```

## 25. Appendix: CORS Configuration Examples

### 25.1 Open API (Public)
```json
{
  "allowed_origins": ["*"],
  "allowed_methods": ["GET", "POST", "PUT", "DELETE", "PATCH"],
  "allowed_headers": ["Authorization", "Content-Type", "X-Request-Id"],
  "exposed_headers": ["X-Request-Id", "X-RateLimit-Remaining"],
  "allow_credentials": true,
  "max_age_seconds": 86400
}
```

### 25.2 Restricted API (Internal)
```json
{
  "allowed_origins": ["https://dashboard.aios.io", "https://admin.aios.io"],
  "allowed_methods": ["GET", "POST"],
  "allowed_headers": ["Authorization", "Content-Type", "X-CSRF-Token"],
  "exposed_headers": ["X-Request-Id"],
  "allow_credentials": true,
  "max_age_seconds": 3600
}
```

## 26. Appendix: Monitoring Dashboards

### 26.1 Key Dashboard Panels
- Total requests per second (line chart, split by API)
- Request latency P50/P95/P99 (heatmap over time)
- Error rate by status code (stacked area chart)
- Rate limit exceeded count (per API key, per tenant)
- Cache hit ratio (gauge per API route)
- Circuit breaker state (per upstream service)
- Active connections (gauge)
- Top 10 API keys by request count (table)
- Top 10 API keys by error rate (table)
- Upstream service health (table with status indicators)

### 26.2 Alert Annotations
Alerts are annotated on the dashboard:
- High error rate (red annotation on the timeline)
- Rate limit threshold near (yellow annotation)
- Circuit breaker state change (purple annotation)
- Configuration change (blue annotation)
- Deployment event (green annotation)

## 27. Appendix: API Lifecycle Example

### Stage 1: Design
API spec is created and reviewed. Version is set (e.g., 2024-07-01). No traffic is served.

### Stage 2: Development
Backend service is implemented. Internal testing via direct gRPC calls to the service. API Manager routes are not yet configured.

### Stage 3: Test
API is registered in the API Manager with visibility=PRIVATE. Only test tenants and internal testing accounts can access it. Automated integration tests run against the API.

### Stage 4: Preview
API visibility is set to BETA. A subset of early adopter tenants is given access. Feedback is collected. Rate limits are conservative (10 req/s).

### Stage 5: Publish
API visibility is set to PUBLIC. The API appears in the Developer Portal. Documentation is available. Rate limits are set to the production configuration. SLA monitoring begins.

### Stage 6: Deprecate
A newer version is published. The old version is marked with Deprecation: true and Sunset headers. A 90-day transition period begins. Documentation is updated with migration guides.

### Stage 7: Retire
After the 90-day deprecation period, the API version is retired. All requests return 410 Gone. The API definition is archived. The route is removed from the routing table.

## 28. Appendix: Performance Benchmarking Results

Gateway overhead benchmarks (16 vCPU, 32GB RAM instance, no upstream):

| Scenario | P50 | P95 | P99 | Max |
|---|---|---|---|---|
| No auth, no rate limit, no cache | 0.3ms | 0.8ms | 1.2ms | 3ms |
| API key auth only | 0.5ms | 1.5ms | 2.5ms | 5ms |
| API key + rate limit | 0.8ms | 2.0ms | 3.5ms | 8ms |
| API key + rate limit + validation | 1.2ms | 3.0ms | 5.0ms | 12ms |
| API key + rate limit + validation + cache (miss) | 1.5ms | 3.5ms | 6.0ms | 15ms |
| API key + rate limit + validation + cache (hit) | 1.0ms | 2.5ms | 4.0ms | 10ms |
| Full pipeline (all middlewares) | 2.0ms | 4.5ms | 7.0ms | 20ms |

Throughput: 15,000 requests/second with full pipeline before CPU saturation on a single instance.

## 29. Appendix: Glossary

| Term | Definition |
|---|---|
| API Manager | The unified ingress gateway for all external API traffic in the AIOS ecosystem |
| API Definition | A registered API including its routes, schemas, policies, and lifecycle state |
| API Route | A mapping from an external HTTP method and path to an internal service endpoint |
| API Lifecycle | The state machine governing API states: DESIGN, ACTIVE, DEPRECATED, RETIRED |
| API Key | A credential used for authenticating external API requests |
| Rate Limiter | Component that enforces maximum request rates per API key, user, or tenant |
| Circuit Breaker | A protection mechanism that prevents requests from reaching unhealthy upstream services |
| Request Transformer | Component that converts requests between protocols and formats (REST to gRPC, etc.) |
| Request Validator | Component that validates incoming requests against OpenAPI schemas |
| CORS | Cross-Origin Resource Sharing policy for web browser-based API access |
| Developer Portal | A web application for API discovery, documentation, key management, and usage analytics |
| Upstream Service | An internal AIOS service that handles the actual business logic for an API route |
| Response Cache | A Redis-backed cache for idempotent API responses to reduce latency and upstream load |
| Analytics Collector | Component that aggregates API usage data for monitoring and reporting |
| SLA | Service Level Agreement: targets for API availability, latency, and error rate |
| Usage Plan | A tiered pricing and rate-limiting scheme for API access (free, pro, enterprise) |
| Transcoding | Converting between protocols (e.g., REST HTTP to gRPC) |
| Gateway Overhead | The additional latency introduced by the API Manager beyond the upstream service response time |
