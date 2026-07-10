# AIOS Retry System — Engineering Specification

## 1. Purpose

The AIOS Retry System provides a robust, configurable, and observable mechanism for handling transient failures across all agent interactions, API calls, database operations, and distributed service communications within the AIOS platform. It ensures system resilience by automatically recovering from recoverable errors while preventing cascading failures through circuit breakers, bulkheads, and retry budget enforcement. The system guarantees exactly-once or at-most-once execution semantics through idempotency keys, maintains retry state across process restarts, and exposes rich telemetry for operational insight. It is designed to handle diverse failure modes including network timeouts, rate limiting, service unavailability, database deadlocks, and infrastructure throttling without compromising system stability or exhausting resources.

## 2. Mission

To deliver a production-grade retry and resilience framework that maximizes successful operation completion rates while strictly bounding resource consumption, latency impact, and failure blast radius. The system shall achieve at least 99.9% transient failure recovery without manual intervention, maintain sub-millisecond overhead for retry decision logic, support pluggable retry policies per operation type, and provide real-time visibility into retry health through comprehensive metrics, events, and distributed tracing. The mission includes enabling safe degradation patterns so that localized failures never escalate to system-wide outages.

## 3. Responsibilities

### 3.1 Retry Trigger Identification
- Detect and classify transient errors including network timeouts (connect/read/write), HTTP 429 (rate limited), HTTP 503 (service unavailable), HTTP 502 (bad gateway), HTTP 504 (gateway timeout), database deadlocks (40001), connection pool exhaustion, temporary DNS failures, TLS handshake failures, and gRPC status codes Unavailable, DeadlineExceeded, and ResourceExhausted.
- Maintain an extensible registry of error classifiers with configurable severity levels and retry eligibility flags.
- Support custom error classification via plugin interface for agent-specific transient errors.

### 3.2 Retry Policy Management
- Implement multiple retry policies: fixed interval, exponential backoff, jittered backoff (full/equal/decorrelated), incremental backoff, and linear backoff.
- Support policy composition with per-policy max retries, max duration, and backoff multiplier configuration.
- Allow operation-level policy override via annotations, configuration files, or runtime API.
- Provide policy templates for common patterns (default, aggressive, conservative, no-retry).

### 3.3 Retry Budget Enforcement
- Enforce maximum retry attempts per operation (configurable default 3, range 0-10).
- Enforce maximum total retry duration (configurable default 30s, range 0-300s).
- Track global retry budget as rate per second/minute with configurable cap to prevent retry storms.
- Implement budget depletion behavior: once budget exhausted, new operations fail fast without retry.

### 3.4 Exponential Backoff Calculation
- Base formula: delay = baseDelay × multiplier^(attempt - 1)
- Apply configurable base delay (default 100ms), multiplier (default 2.0), and max delay cap (default 30s).
- Support logarithmic backoff variant: delay = baseDelay × ln(e + multiplier × (attempt - 1)).
- Support fibonacci backoff variant: delay = baseDelay × fib(attempt + 1) where fib(0)=0, fib(1)=1.

### 3.5 Jitter Strategies
- **Full Jitter**: delay = random(0, calculatedBackoff) — uniform distribution from 0 to backoff value.
- **Equal Jitter**: delay = calculatedBackoff/2 + random(0, calculatedBackoff/2) — reduces thundering herd.
- **Decorrelated Jitter**: delay = min(cap, random(baseDelay, previousDelay × 3)) — best for high concurrency.
- All jitter strategies must be deterministic when a seed is provided for replayability.
- Configurable jitter enable/disable per policy with strategy selection.

### 3.6 Idempotency Keys
- Generate unique idempotency keys (UUIDv4 or ULID) for each operation at creation time.
- Store idempotency key → response mapping with configurable TTL (default 24h, max 7d).
- On retry, present the same idempotency key; the downstream service returns the original response.
- Support client-supplied idempotency keys via header/field for external callers.
- Implement idempotency key collision detection with conflict error response.

### 3.7 Retry State Persistence
- Persist retry state (attempt count, last error, next retry time, idempotency key) to durable storage.
- Support backends: Redis (primary), PostgreSQL, or embedded BoltDB/LMDB.
- On restart, recover in-flight retries from persistent state and resume retry schedule.
- TTL-based cleanup for completed retry state after configurable retention period.

### 3.8 Circuit Breaker
- Implement three-state circuit breaker: CLOSED (normal operation), OPEN (failing fast), HALF-OPEN (probing recovery).
- **CLOSED → OPEN**: Transition when failure count exceeds threshold (default 5) within a sliding window (default 60s).
- **OPEN → HALF-OPEN**: Transition after timeout elapses (default 30s, configurable per breaker).
- **HALF-OPEN → CLOSED**: Transition when probe request succeeds.
- **HALF-OPEN → OPEN**: Transition when probe request fails.
- Configurable failure threshold, success threshold (for half-open), sliding window size, and open timeout.
- Support per-operation-type and per-dependency circuit breakers.

### 3.9 Bulkhead Pattern
- Isolate retry execution into separate thread pools or semaphore partitions per dependency/operation type.
- Configurable maximum concurrent retry calls per bulkhead (default 10).
- Configurable queue capacity for waiting retries per bulkhead (default 20).
- When bulkhead is saturated, new retries fail immediately with BulkheadFullError.
- Support bulkhead metrics: active count, queue depth, max active, rejection count.

### 3.10 Retry Metrics and Telemetry
- **Count**: Total retry attempts per operation type, per dependency, per policy.
- **Success After Retry**: Count of operations that succeeded after one or more retries.
- **Failure After Retries**: Count of operations that ultimately failed despite retries.
- **Circuit Breaker State**: Current state gauge (0=closed, 1=half-open, 2=open) per breaker.
- **Breaker Transition Count**: Rate of state transitions (closed→open, open→half-open, half-open→closed).
- **MTTR (Mean Time To Recover)**: Average time from first failure to successful retry.
- **Retry Budget Utilization**: Current utilization as percentage of budget.
- **Bulkhead Metrics**: Active threads, queue depth, rejection rate.
- **Idempotency Cache**: Hit rate, miss rate, size, eviction count.
- **Persistence Latency**: P50/P95/P99 for state read/write operations.

## 4. Non-Responsibilities

- The retry system does NOT handle permanent/business errors (400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found, 409 Conflict, 422 Unprocessable Entity).
- It does NOT retry operations that have side effects without idempotency guarantees.
- It does NOT replace application-level error handling or compensating transactions for business logic failures.
- It does NOT provide distributed transaction coordination or two-phase commit.
- It does NOT manage connection pools or transport-level retry (handled by network layer).
- It does NOT perform automatic schema migration or version compatibility resolution.

## 5. Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    AIOS Retry System                         │
│                                                             │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐   │
│  │  Retry   │  │ Circuit  │  │Bulkhead  │  │Idempo-  │   │
│  │ Decision │  │ Breaker  │  │ Executor │  │tency    │   │
│  │  Engine  │  │ Manager  │  │  Pool    │  │Manager  │   │
│  └────┬─────┘  └────┬─────┘  └────┬─────┘  └────┬─────┘   │
│       │             │             │             │          │
│  ┌────┴─────────────┴─────────────┴─────────────┴────┐     │
│  │                Policy Registry                      │     │
│  └────────────────────────┬───────────────────────────┘     │
│                           │                                 │
│  ┌────────────────────────┴───────────────────────────┐     │
│  │             State Persistence Layer                   │     │
│  │  (Redis / PostgreSQL / Embedded DB)                  │     │
│  └────────────────────────┬───────────────────────────┘     │
│                           │                                 │
│  ┌────────────────────────┴───────────────────────────┐     │
│  │           Metrics & Observability Layer               │     │
│  └─────────────────────────────────────────────────────┘     │
└─────────────────────────────────────────────────────────────┘
```

## 6. Components

### 6.1 RetryDecisionEngine
Central orchestrator that evaluates whether an operation should be retried based on error classification, retry policy, circuit breaker state, bulkhead availability, and retry budget. Produces retry decisions with computed delay, jitter, and idempotency key. Interfaces with all downstream components to gather state before rendering a decision.

### 6.2 PolicyRegistry
Manages the lifecycle of retry policies including registration, lookup, composition, and validation. Stores policies in a thread-safe map with O(1) lookup by operation type and priority. Supports hierarchical policy resolution (global → service → operation). Provides policy validation on registration to catch misconfiguration.

### 6.3 CircuitBreakerManager
Owns all circuit breaker instances organized by dependency/resource identifier. Each breaker maintains its internal state machine with counters, timers, and sliding windows. Exposes thread-safe operations for recording success/failure, querying state, and resetting. Periodically transitions half-open breakers to attempt recovery probes.

### 6.4 BulkheadExecutorPool
Manages thread pools and semaphore partitions for isolated retry execution. Each bulkhead is configured with core pool size, max pool size, queue capacity, and keep-alive time. Implements rejection handling with BulkheadFullException propagation. Monitors thread utilization and adjusts pool sizes based on configured elasticity.

### 6.5 IdempotencyManager
Generates, validates, and stores idempotency keys. Maintains an LRU cache of key→response mappings with TTL-based expiration. Handles key collision scenarios with configurable conflict resolution (reject, deduplicate, overwrite). Coordinates with persistence layer for durable key storage.

### 6.6 StatePersistence
Provides CRUD operations for retry state across configurable backends. Implements repository pattern with pluggable storage adapters for Redis, PostgreSQL, and embedded databases. Handles serialization/deserialization of retry state to Protobuf or JSON. Manages TTL-based cleanup of stale state entries.

### 6.7 MetricsCollector
Aggregates retry-related metrics from all components and exports them to the platform's metrics infrastructure (Prometheus, StatsD, or OpenTelemetry). Maintains counters, histograms, gauges, and rate aggregations with pre-configured bucketing and labeling.

### 6.8 ErrorClassifier
Maintains a registry of error type → classification mappings. Each classification includes retryable flag, severity level, recommended policy, and optional delay override. Supports dynamic classification via plugin interface for agent-specific errors. Provides bulk classification for error batches.

## 7. Dependencies

### Internal Dependencies
- AIOS Configuration System — for policy and threshold configuration.
- AIOS Metrics System — for metric emission and dashboard integration.
- AIOS Logging System — for structured log output with correlation IDs.
- AIOS Tracing System — for distributed trace context propagation and span creation.
- AIOS State Manager — for persistent state storage of retry metadata and circuit breaker state.
- AIOS Secret Manager — for storage of any credentials needed for downstream retry calls.

### External Dependencies
- Redis 7.x — primary backend for idempotency cache, retry state, and circuit breaker state.
- PostgreSQL 15+ — optional secondary backend for durable retry state persistence.
- Protocol Buffers 3.x — serialization format for retry state across services.
- OpenTelemetry SDK 1.x — for metrics, tracing, and logging export.
- Prometheus client library — for metric exposition endpoint.

## 8. Inputs/Outputs

### Inputs
- **Operation Request**: Includes operation type, operation ID, target service, payload, and context.
- **Error Signal**: Error object from failed operation attempt with type, message, status code, and metadata.
- **Policy Configuration**: Retry policy definition including algorithm, parameters, and thresholds.
- **Breaker Configuration**: Circuit breaker thresholds, timeouts, and window settings.
- **Budget Configuration**: Maximum retry count, duration, and rate limits.
- **Classification Rules**: Error classification mappings and severity assignments.
- **State Recovery Request**: Bootstrap request on system startup to reload in-flight retries.

### Outputs
- **Retry Decision**: Decision object with shouldRetry (bool), delay (duration), jitteredDelay (duration), and reason (string).
- **Retry Response**: Success response or final failure with attempt history and error summary.
- **Circuit Breaker State Change**: Events for state transitions to monitoring and alerting systems.
- **Bulkhead Rejection**: BulkheadFullException when capacity exceeded.
- **Idempotency Match**: Cached response from prior successful execution.
- **Metrics Events**: Counters, histograms, and gauges for observability pipeline.

## 9. Data Structures

### RetryDecision
```json
{
  "operationId": "string (UUIDv4)",
  "shouldRetry": "boolean",
  "attempt": "int32",
  "maxRetries": "int32",
  "delay": "duration (ms)",
  "jitteredDelay": "duration (ms)",
  "nextRetryAt": "timestamp (RFC3339)",
  "reason": "string",
  "policyName": "string",
  "breakerState": "string (CLOSED|OPEN|HALF_OPEN)",
  "idempotencyKey": "string (UUIDv4)"
}
```

### RetryPolicy
```json
{
  "name": "string",
  "algorithm": "string (FIXED|EXPONENTIAL|JITTERED|INCREMENTAL|LINEAR)",
  "baseDelayMs": "int32",
  "maxDelayMs": "int32",
  "multiplier": "float64",
  "maxRetries": "int32",
  "maxDurationMs": "int64",
  "jitterType": "string (NONE|FULL|EQUAL|DECORRELATED)",
  "jitterSeed": "int64 (optional)",
  "retryableStatusCodes": ["int32"],
  "retryableErrorTypes": ["string"]
}
```

### CircuitBreakerState
```json
{
  "name": "string",
  "state": "string (CLOSED|OPEN|HALF_OPEN)",
  "failureCount": "int64",
  "successCount": "int64",
  "failureThreshold": "int32",
  "successThreshold": "int32",
  "slidingWindowSizeMs": "int64",
  "openTimeoutMs": "int64",
  "lastFailureAt": "timestamp",
  "lastSuccessAt": "timestamp",
  "lastStateChangeAt": "timestamp",
  "halfOpenProbeCount": "int32"
}
```

### BulkheadConfiguration
```json
{
  "name": "string",
  "maxConcurrentCalls": "int32",
  "maxQueueCapacity": "int32",
  "queueTimeoutMs": "int64",
  "keepAliveMs": "int64",
  "allowCoreThreadTimeout": "boolean"
}
```

### IdempotencyEntry
```json
{
  "idempotencyKey": "string",
  "operationType": "string",
  "responsePayload": "bytes (Protobuf)",
  "responseStatusCode": "int32",
  "createdAt": "timestamp",
  "expiresAt": "timestamp",
  "attemptCount": "int32"
}
```

### RetryState
```json
{
  "operationId": "string",
  "idempotencyKey": "string",
  "operationType": "string",
  "targetService": "string",
  "attemptCount": "int32",
  "maxRetries": "int32",
  "lastError": "string",
  "lastErrorAt": "timestamp",
  "nextRetryAt": "timestamp",
  "createdAt": "timestamp",
  "updatedAt": "timestamp",
  "state": "string (PENDING|IN_PROGRESS|SUCCEEDED|FAILED|CANCELLED)",
  "policyName": "string"
}
```

### ErrorClassification
```json
{
  "errorType": "string",
  "isRetryable": "boolean",
  "severity": "string (LOW|MEDIUM|HIGH|CRITICAL)",
  "recommendedPolicy": "string",
  "delayOverrideMs": "int32 (optional)",
  "statusCodes": ["int32"]
}
```

### RetryBudget
```json
{
  "maxRetriesPerOperation": "int32",
  "maxDurationMsPerOperation": "int64",
  "maxRetryRatePerSecond": "int32",
  "currentRatePerSecond": "float64",
  "budgetExhausted": "boolean",
  "resetAt": "timestamp"
}
```

## 10. Execution Flow

### 10.1 Standard Retry Flow
1. Operation initiated with operation ID and payload.
2. RetryDecisionEngine receives the operation request.
3. Engine queries ErrorClassifier to determine if error is retryable (on first call, error is nil — proceed).
4. Engine queries PolicyRegistry for the operation's retry policy.
5. Engine checks CircuitBreakerManager for target dependency state:
   - If OPEN → immediately fail with CircuitBreakerOpenError, no retry.
   - If HALF_OPEN → allow one probe request, proceed with caution.
6. Engine checks BulkheadExecutorPool for capacity:
   - If saturated → fail with BulkheadFullError.
7. Engine generates or retrieves idempotency key from IdempotencyManager.
8. Operation is dispatched to target service.
9. On success:
   a. Record success in CircuitBreakerManager.
   b. Cache response in IdempotencyManager with key→response mapping.
   c. Emit success metrics.
   d. Return response to caller.
10. On failure:
    a. Classify error via ErrorClassifier.
    b. If not retryable → return final error.
    c. If retryable:
       i.   Record failure in CircuitBreakerManager.
       ii.  Increment attempt counter.
       iii. Check if max retries exceeded → if yes, return final error.
       iv.  Check if max duration exceeded → if yes, return final error.
       v.   Check retry budget → if exhausted, fail fast.
       vi.  Compute backoff delay with jitter.
       vii. Persist retry state via StatePersistence.
       viii. Schedule retry after computed delay.
       ix.  Emit retry metrics.
       x.   On retry attempt, restore idempotency key and loop to step 4.

### 10.2 Circuit Breaker Flow
1. Breaker starts in CLOSED state.
2. On each failure, failureCount increments within sliding window.
3. When failureCount >= failureThreshold → transition to OPEN.
4. Start open timeout timer.
5. All requests to OPEN breaker fail immediately.
6. After open timeout → transition to HALF_OPEN.
7. Allow single probe request through.
8. If probe succeeds → transition to CLOSED, reset failureCount.
9. If probe fails → transition back to OPEN, restart timeout.

### 10.3 State Recovery on Restart
1. On startup, RetrySystem loads all PENDING and IN_PROGRESS states from persistence.
2. For each recovered state, evaluate if nextRetryAt has passed:
   - If yes → execute retry immediately.
   - If no → schedule retry for future execution.
3. Recover circuit breaker states from persistence (if persisted).
4. Rehydrate idempotency cache from persistence.
5. Emit recovery metrics: count of recovered operations.

## 11. State Management

### 11.1 Transient State
- In-memory LRU cache for idempotency key→response mappings (max 100k entries, 24h TTL).
- In-memory circuit breaker state machines with sliding window counters.
- In-memory retry budget counters with 1-second granularity windows.

### 11.2 Persistent State
- Retry state persisted to Redis (key: `retry:state:{operationId}`, TTL: 7 days).
- Idempotency cache persisted to Redis (key: `idempotency:{key}`, TTL: configurable 24h).
- Circuit breaker state optionally persisted to Redis (key: `breaker:{name}`, TTL: 7 days).
- PostgreSQL used as secondary durable store for critical retry state (configurable).

### 11.3 State Consistency
- Idempotency writes use Redis SETNX to prevent race conditions on concurrent retries of same key.
- Retry state updates use optimistic locking with version numbers.
- Circuit breaker transitions are atomic via Redis Lua scripts or distributed locks.
- Periodic reconciliation job detects and corrects state drift between memory and persistence.

## 12. Communication

### 12.1 APIs

#### RetryDecisionAPI (Internal gRPC)
```
service RetryDecisionService {
  rpc ShouldRetry(ShouldRetryRequest) returns (ShouldRetryResponse);
  rpc RecordSuccess(RecordSuccessRequest) returns (Empty);
  rpc RecordFailure(RecordFailureRequest) returns (Empty);
  rpc GetRetryState(GetRetryStateRequest) returns (RetryState);
  rpc CancelRetry(CancelRetryRequest) returns (Empty);
  rpc ResetCircuitBreaker(ResetCircuitBreakerRequest) returns (Empty);
  rpc GetCircuitBreakerState(GetBreakerStateRequest) returns (CircuitBreakerState);
}
```

#### PolicyManagementAPI (Internal gRPC)
```
service PolicyManagementService {
  rpc RegisterPolicy(RegisterPolicyRequest) returns (PolicySummary);
  rpc UpdatePolicy(UpdatePolicyRequest) returns (PolicySummary);
  rpc DeletePolicy(DeletePolicyRequest) returns (Empty);
  rpc GetPolicy(GetPolicyRequest) returns (RetryPolicy);
  rpc ListPolicies(ListPoliciesRequest) returns (ListPoliciesResponse);
  rpc ValidatePolicy(ValidatePolicyRequest) returns (ValidationResult);
}
```

### 12.2 Events Emitted

| Event | Payload | Emitter | Consumer |
|-------|---------|---------|----------|
| `retry.attempt` | operationId, attempt, delay, policy | DecisionEngine | Metrics, Logging |
| `retry.success_after_retry` | operationId, totalAttempts, totalDuration | DecisionEngine | Metrics, Alerting |
| `retry.failure_after_retries` | operationId, totalAttempts, lastError | DecisionEngine | Alerting, Incident |
| `retry.budget_exhausted` | budgetType, currentRate, limit | BudgetManager | Alerting, AutoScaling |
| `circuit_breaker.state_change` | breakerName, fromState, toState, reason | BreakerManager | Alerting, Dashboard |
| `circuit_breaker.half_open_probe` | breakerName, result | BreakerManager | Metrics |
| `bulkhead.rejection` | bulkheadName, activeCount, queueDepth | ExecutorPool | Alerting, AutoScale |
| `idempotency.collision` | key, existingOperationId, newOperationId | IdempotencyMgr | Audit, Logging |

## 13. Caching

### 13.1 Idempotency Cache
- L1: In-memory LRU cache (100k entries, 24h TTL, 1ms access).
- L2: Redis cache (1M entries, 24h TTL, 5ms access).
- Cache-aside pattern: check L1 then L2, populate on miss.
- Write-through on success: write to L1 + L2 synchronously.

### 13.2 Policy Cache
- In-memory cache of active policies with near-infinite TTL (invalidated on policy change).
- 10k entry capacity, LRU eviction.
- Sub-millisecond lookup for retry decision path.

### 13.3 Circuit Breaker State Cache
- In-memory only (no remote caching needed for state machines).
- State transitions invalidate local state immediately.

## 14. Memory

### 14.1 Heap Usage
- Idempotency cache: ~500MB at 100k entries (5KB avg per entry).
- Retry state in-memory tracking: ~50MB at 10k concurrent in-flight retries.
- Circuit breaker state: ~1MB per 1000 breakers.
- Policy cache: ~2MB at 10k policies.
- Total estimated heap: ~600MB under normal load, 1.2GB under peak.

### 14.2 Off-Heap
- Thread pool stacks: ~10MB per 100 threads in bulkhead pools.
- Direct memory for network buffers: ~50MB.

### 14.3 Garbage Collection
- Generational GC with young generation tuning for high-throughput retry decisions.
- Idempotency cache entries use weak references where feasible.
- Retry state objects pool to reduce allocation pressure.
- Target: GC pause < 10ms, collection interval > 5 seconds under load.

## 15. Persistence

### 15.1 Redis Schema
```
retry:state:{operationId} → Hash { attempt, lastError, nextRetryAt, state, ... } TTL 7d
retry:state:index:nextRetry → Sorted Set { score: nextRetryAt, member: operationId }
retry:state:index:pending → Set { operationId }  [for recovery on restart]
idempotency:{key} → String (JSON/Protobuf) TTL 24h-7d
breaker:{name} → Hash { state, failureCount, lastFailureAt, ... } TTL 7d
breaker:index → Set { breakerName }  [for listing]
```

### 15.2 PostgreSQL Schema (Optional)
```sql
CREATE TABLE retry_state (
  operation_id UUID PRIMARY KEY,
  idempotency_key UUID NOT NULL UNIQUE,
  operation_type VARCHAR(128) NOT NULL,
  target_service VARCHAR(256) NOT NULL,
  attempt_count INT NOT NULL DEFAULT 0,
  max_retries INT NOT NULL,
  last_error TEXT,
  last_error_at TIMESTAMPTZ,
  next_retry_at TIMESTAMPTZ,
  created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  updated_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  state VARCHAR(32) NOT NULL DEFAULT 'PENDING',
  policy_name VARCHAR(128)
);

CREATE TABLE circuit_breaker_state (
  name VARCHAR(256) PRIMARY KEY,
  state VARCHAR(32) NOT NULL DEFAULT 'CLOSED',
  failure_count BIGINT NOT NULL DEFAULT 0,
  success_count BIGINT NOT NULL DEFAULT 0,
  failure_threshold INT NOT NULL DEFAULT 5,
  success_threshold INT NOT NULL DEFAULT 2,
  sliding_window_size_ms BIGINT NOT NULL DEFAULT 60000,
  open_timeout_ms BIGINT NOT NULL DEFAULT 30000,
  last_failure_at TIMESTAMPTZ,
  last_success_at TIMESTAMPTZ,
  last_state_change_at TIMESTAMPTZ,
  metadata JSONB
);

CREATE TABLE idempotency_cache (
  idempotency_key UUID PRIMARY KEY,
  operation_type VARCHAR(128) NOT NULL,
  response_payload BYTEA NOT NULL,
  response_status_code INT NOT NULL,
  created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  expires_at TIMESTAMPTZ NOT NULL,
  attempt_count INT NOT NULL DEFAULT 1
);
```

## 16. Validation

### 16.1 Configuration Validation
- All retry policies validated at registration time.
- Base delay must be >= 10ms, max delay must be >= base delay.
- Max retries must be in range [0, 10]; 0 means no retries.
- Max duration must be in range [0ms, 300000ms]; 0 means unlimited.
- Multiplier must be >= 1.0 for exponential algorithms.
- Failure threshold must be >= 1, success threshold must be >= 1.
- Bulkhead max concurrent calls must be >= 1, queue capacity >= 0.

### 16.2 Runtime Validation
- Idempotency keys validated as UUIDv4 format.
- Operation IDs validated as non-empty.
- Target service names validated against service registry.
- Error objects validated for required fields (type, message).
- Retry state loaded from persistence validated for schema compatibility.

### 16.3 Input Sanitization
- All string inputs trimmed and escaped for injection prevention.
- All numeric inputs clamped to allowable ranges.
- All timestamps validated as RFC3339 format.
- Payload size limited to 1MB for idempotency cache entries.

## 17. Retry Logic

### 17.1 Backoff Algorithm Implementations

**Exponential Backoff:**
```
delay = min(maxDelay, baseDelay × multiplier^(attempt - 1))
attempt=1: 100ms, attempt=2: 200ms, attempt=3: 400ms, attempt=4: 800ms...
```

**Full Jitter:**
```
delay = random(0, min(maxDelay, baseDelay × multiplier^(attempt - 1)))
attempt=1: 0-100ms, attempt=2: 0-200ms, attempt=3: 0-400ms...
```

**Equal Jitter:**
```
half = min(maxDelay, baseDelay × multiplier^(attempt - 1)) / 2
delay = half + random(0, half)
attempt=1: 50-100ms, attempt=2: 100-200ms, attempt=3: 200-400ms...
```

**Decorrelated Jitter:**
```
delay = min(maxDelay, random(baseDelay, previousDelay × 3))
prev=100ms: 100-300ms, prev=300ms: 100-900ms, prev=900ms: 100-1000ms(maxDelay)...
```

**Incremental Backoff:**
```
delay = min(maxDelay, baseDelay + increment × (attempt - 1))
attempt=1: 100ms, attempt=2: 200ms, attempt=3: 300ms...
```

### 17.2 Policy Templates
```json
{
  "default": { "algorithm": "EXPONENTIAL", "baseDelayMs": 100, "maxDelayMs": 30000, "multiplier": 2.0, "maxRetries": 3, "maxDurationMs": 30000, "jitterType": "DECORRELATED" },
  "aggressive": { "algorithm": "JITTERED", "baseDelayMs": 50, "maxDelayMs": 5000, "multiplier": 1.5, "maxRetries": 5, "maxDurationMs": 15000, "jitterType": "FULL" },
  "conservative": { "algorithm": "EXPONENTIAL", "baseDelayMs": 500, "maxDelayMs": 60000, "multiplier": 3.0, "maxRetries": 2, "maxDurationMs": 60000, "jitterType": "EQUAL" },
  "no-retry": { "algorithm": "FIXED", "baseDelayMs": 0, "maxRetries": 0, "maxDurationMs": 0 },
  "streaming": { "algorithm": "LINEAR", "baseDelayMs": 200, "maxDelayMs": 5000, "maxRetries": 8, "maxDurationMs": 40000, "jitterType": "FULL" }
}
```

## 18. Error Recovery

### 18.1 Retry State Corruption
- On detecting corrupted state (invalid timestamps, negative counts), log warning and reset to default.
- Corrupted entries are quarantined for manual inspection with operationId reference.
- Automatic cleanup of corrupted entries after 7 days.

### 18.2 Circuit Breaker State Loss
- On restart, if persisted breaker state is unavailable, start all breakers in CLOSED state.
- Gradual traffic ramp-up to avoid overwhelming recovering dependencies.
- Breakers auto-calibrate within one sliding window period.

### 18.3 Idempotency Cache Loss
- On cache miss (e.g., Redis flush), downstream services must handle potential duplicate operations.
- Idempotency Manager generates new idempotency keys for operations whose cache entries are lost.
- Operations that require strict exactly-once semantics must be revalidated by downstream services.

### 18.4 Bulkhead Exhaustion
- When all bulkhead threads are saturated, new retries fail immediately.
- Callers should implement their own fallback or queueing strategy.
- Alert triggered when bulkhead utilization exceeds 80% for 60 seconds.

### 18.5 Persistence Backend Failure
- If primary Redis is unavailable, fall back to PostgreSQL if configured.
- If both backends unavailable, operate in memory-only degraded mode with reduced capacity.
- In degraded mode, max retries reduced to 1, max retry rate halved.
- Automatic recovery when persistence backend becomes available.

## 19. Security

### 19.1 Authentication
- All retry system APIs require valid service-to-service JWT tokens with audience claim matching the retry system.
- Token validation includes signature verification, expiry check, and audience verification.
- Idempotency key operations require write scope for set operations, read scope for get operations.

### 19.2 Authorization
- Role-Based Access Control (RBAC) for policy management operations:
  - `retry.policy.admin` — full CRUD on all policies.
  - `retry.policy.reader` — read-only policy access.
  - `retry.breaker.reset` — permission to reset circuit breakers.
  - `retry.operation.cancel` — permission to cancel in-flight retries.

### 19.3 Encryption
- All retry state stored in Redis/PostgreSQL encrypted at rest using AES-256-GCM.
- Idempotency cache entries encrypted at rest.
- Network communication between retry system components encrypted with TLS 1.3.
- Idempotency keys treated as sensitive — hashed before storage in audit logs.

### 19.4 Audit Trail
- All policy changes logged with before/after state, user identity, and timestamp.
- All circuit breaker manual resets logged.
- All idempotency key collisions logged for security review.
- Audit logs retained for minimum 90 days, exportable to SIEM systems.

## 20. Monitoring and Observability

### 20.1 Key Metrics

| Metric | Type | Labels | Description |
|--------|------|--------|-------------|
| `retry_attempts_total` | Counter | operation_type, policy, target | Total retry attempts |
| `retry_success_after_retry_total` | Counter | operation_type, target | Operations succeeding after at least one retry |
| `retry_failure_after_retries_total` | Counter | operation_type, target | Operations failing after exhausting retries |
| `retry_budget_utilization_ratio` | Gauge | budget_type | Current retry budget utilization (0-1) |
| `circuit_breaker_state` | Gauge | breaker_name | Current state (0=closed, 1=half-open, 2=open) |
| `circuit_breaker_transitions_total` | Counter | breaker_name, from_state, to_state | State transition count |
| `bulkhead_active_threads` | Gauge | bulkhead_name | Currently active retry threads |
| `bulkhead_queue_depth` | Gauge | bulkhead_name | Waiting retry count |
| `bulkhead_rejections_total` | Counter | bulkhead_name, reason | Rejected retry count |
| `idempotency_cache_hit_ratio` | Gauge | tier | Idempotency cache hit rate |
| `idempotency_cache_size` | Gauge | tier | Current cache entry count |
| `retry_mttr_seconds` | Histogram | operation_type | Mean time to recover via retry |
| `retry_delay_seconds` | Histogram | policy | Applied retry delay distribution |
| `retry_persistence_read_latency_ms` | Histogram | backend | State persistence read latency |
| `retry_persistence_write_latency_ms` | Histogram | backend | State persistence write latency |
| `retry_decision_latency_ms` | Histogram | — | Retry decision computation time |

### 20.2 Logging
- Structured JSON logging with operation ID, trace ID, and span ID correlation.
- Log levels: DEBUG (retry decision details), INFO (state transitions), WARN (budget approaching limit, bulkhead high utilization), ERROR (persistence failures, state corruption).
- Log sampling: 100% for ERROR and WARN, 10% for INFO, 1% for DEBUG under normal operation.
- Rate-limited logging for high-frequency events (per-second retry decision logging capped at 100 logs/second).

### 20.3 Distributed Tracing
- Each retry attempt creates a child span under the original operation trace.
- Span attributes: operation_id, attempt, delay_ms, policy_name, breaker_state.
- Trace sampling: 100% for operations with retries, 1% for first-attempt successes.
- Full trace context propagation through gRPC metadata headers.

### 20.4 Health Check
- Readiness probe: retry system is ready when persistence backend is connected and state recovery is complete (max 30s after startup).
- Liveness probe: retry system is live if retry decision engine responds within 1 second and error rate < 10%.
- Component-level health: circuit breaker manager, bulkhead pools, idempotency manager each report health status.

## 21. Scalability

### 21.1 Horizontal Scaling
- Retry system is stateless for decision making (state is externalized to Redis/PostgreSQL).
- Multiple retry system instances can run behind a load balancer.
- Each instance independently manages its own in-memory state but shares persistent state via Redis.
- Circuit breaker state synchronization via Redis pub/sub state change events.

### 21.2 Vertical Scaling
- Bulkhead thread pool sizes configurable per instance based on CPU core count.
- Idempotency cache size scales with available heap memory.
- Connection pools to Redis and PostgreSQL auto-tuned based on available file descriptors.

### 21.3 Performance Targets
- Retry decision latency: < 500µs P50, < 2ms P95, < 10ms P99.
- Idempotency cache lookup: < 100µs L1, < 5ms L2.
- State persistence write: < 5ms P50, < 20ms P95 (Redis).
- Circuit breaker state transition: < 1ms.
- Concurrent retry capacity: 10,000+ in-flight retries per instance.

## 22. Testing

### 22.1 Unit Tests
- Backoff algorithm computations verified against known values for all five algorithms.
- Circuit breaker state machine transitions verified for all paths (CLOSED→OPEN→HALF_OPEN→CLOSED/OPEN).
- Idempotency key generation verified for uniqueness and format compliance.
- Policy validation logic verified for all constraint violations.
- Error classification verified for all built-in error types.

### 22.2 Integration Tests
- Redis persistence adapter: CRUD operations, TTL expiration, connection recovery.
- PostgreSQL persistence adapter: CRUD operations, connection pool exhaustion, query timeout.
- End-to-end retry flow with mock service returning configurable errors.
- Idempotency guarantee verification: same request with same key returns same response.

### 22.3 Chaos Tests
- Random service failures injected to verify retry recovery.
- Redis failover tests to verify persistence backend switching.
- Bulkhead saturation tests to verify rejection behavior.
- Circuit breaker slow recovery tests (half-open probes timing).
- Network partition tests to verify state consistency after reconnection.

### 22.4 Performance Tests
- Throughput test: 10,000 concurrent operations with 50% retry rate.
- Latency test: retry decision latency under load P50/P95/P99 measurement.
- Memory test: idempotency cache at capacity, measure heap usage and GC pressure.
- Storm test: instant retry storm of 1000 operations, verify budget enforcement.

### 22.5 Resilience Tests
- Repeated failure to verify circuit breaker opens correctly.
- Gradual recovery to verify half-open probe success transitions back to closed.
- Persistence backend outage to verify degraded mode operation.
- Process restart to verify state recovery.

## 23. Risk Analysis

| Risk | Probability | Impact | Mitigation |
|------|------------|--------|------------|
| Retry storm cascading failure | Medium | Critical | Retry budget, circuit breaker, bulkhead |
| Infinite retry loop | Low | High | Max retries cap, max duration cap |
| Idempotency key collision | Low | Medium | UUIDv4 generation, collision detection, rejection |
| Circuit breaker never recovering | Low | Medium | Half-open probe with adaptive timeout |
| Persistence backend overload | Medium | High | Connection pooling, fallback, degraded mode |
| Memory exhaustion from cache | Medium | Medium | LRU eviction, max capacity, size limits |
| Clock skew affecting delay | Low | Medium | NTP synchronization, monotonic clock usage |
| Thundering herd on recovery | Medium | High | Decorrelated jitter, gradual recovery, bulkhead |

## 24. Failure Scenarios

### 24.1 Downstream Service Unavailable (HTTP 503)
- Classification: retryable, severity HIGH.
- Policy: default exponential backoff with decorrelated jitter.
- Circuit breaker opens after 5 failures in 60s window.
- After 30s in OPEN → HALF_OPEN probe.
- On recovery, traffic gradually restored.

### 24.2 Rate Limiting (HTTP 429)
- Classification: retryable, severity MEDIUM.
- Policy: use Retry-After header value if present, else incremental backoff.
- No circuit breaker opening for 429 (considered normal throttling).
- Bulkhead isolation to prevent one consumer starving others.

### 24.3 Database Deadlock (40001)
- Classification: retryable, severity MEDIUM.
- Policy: short exponential backoff with full jitter.
- Max retries: 3, max duration: 10s.
- Idempotency key required for transaction semantics.

### 24.4 Connection Timeout
- Classification: retryable, severity HIGH.
- Policy: aggressive backoff with full jitter.
- Circuit breaker opens after 3 failures in 30s window.
- Combined with connection pool health checking.

### 24.5 Temporary DNS Failure
- Classification: retryable, severity MEDIUM.
- Policy: fixed 1s delay, max 3 retries.
- If all retries fail → error propagated to caller.
- DNS caching at platform level to reduce occurrence.

### 24.6 TLS Handshake Failure
- Classification: retryable, severity LOW (if intermittent).
- Policy: fixed 5s delay, max 2 retries.
- If consistent failure → classified as non-retryable, escalate to security.

## 25. Recovery Procedures

### 25.1 Automatic Recovery
- Circuit breaker half-open probes automatically detect service recovery.
- Retry state automatically resumes after persistence backend reconnection.
- Bulkhead pools automatically reinitialize after configuration change.
- Idempotency cache automatically warms from persisted state.

### 25.2 Manual Recovery
- Force-close circuit breaker via API (requires `retry.breaker.reset` permission).
- Cancel all in-flight retries for a specific operation type.
- Clear idempotency cache (for selective keys or entire cache).
- Reset retry budget counters.
- Purge stuck retry states from persistence.

### 25.3 Incident Response Playbooks
- **Retry Storm**: Immediately reduce retry budget to 10% of normal, open circuit breakers proactively, analyze root cause.
- **Circuit Breaker Cascade**: Verify downstream service health, force-close breakers in dependency order (upstream first).
- **Idempotency Cache Overflow**: Increase cache capacity, reduce TTL, verify no memory pressure, add Redis cluster shard.
- **Bulkhead Starvation**: Increase thread pool size, verify no deadlocked operations, analyze queue depth trends.

## 26. Limits

| Limit | Value | Rationale |
|-------|-------|-----------|
| Max retries per operation | 10 | Prevent infinite loops, bound latency |
| Max retry duration | 300s | Upper bound on delayed response |
| Max retry rate per second | 10,000 | Platform-wide retry storm prevention |
| Min base delay | 10ms | Prevent tight busy loops |
| Max base delay | 60,000ms | Upper bound for initial delay |
| Max idempotency cache entries | 1,000,000 | Memory budget constraint |
| Idempotency key max TTL | 7 days | Storage cost vs. utility tradeoff |
| Max concurrent bulkhead calls | 1,000 | Thread/FD resource constraint |
| Max queue depth per bulkhead | 10,000 | Memory bound for waiting operations |
| Circuit breaker max breakers | 10,000 | Memory constraint for state machines |
| Retry state payload max size | 64 KB | Storage and serialization efficiency |
| Max policy count | 1,000 | Administration overhead |

## 27. Maintenance

### 27.1 Routine Tasks
- Review and trim stale retry states (weekly).
- Verify circuit breaker thresholds against actual failure patterns (bi-weekly).
- Update error classification rules for new error types (as needed).
- Review retry budget utilization trends (monthly).
- Rotate idempotency cache encryption keys (quarterly).

### 27.2 Upgrades
- Backward-compatible policy schema changes: rolling update with no downtime.
- Breaking policy schema changes: flag-based feature toggle, gradual rollout.
- Persistence backend upgrades: blue-green deployment with dual-write during transition.
- Protocol buffer schema evolution: backward-compatible field additions only.

### 27.3 Deprecation
- Deprecated policies: grace period of 30 days with warning logs.
- Deprecated APIs: maintain backward compatibility for 2 minor versions.
- Deprecated error classifiers: redirect to new classifiers at runtime.

## 28. Future Improvements

- **Adaptive Retry Policies**: ML-based policy selection where the system learns optimal backoff parameters from historical success patterns per operation type and time of day.
- **Global Retry Budget Across Regions**: Cross-region retry budget coordination to prevent cascading failures across geographically distributed deployments.
- **Retry Chain Visualization**: Real-time DAG visualization of retry chains showing dependencies, breaker states, and retry progress for complex multi-step operations.
- **Automated Circuit Breaker Tuning**: Self-tuning circuit breaker thresholds based on observed failure patterns, latency distributions, and recovery times.
- **Retry Simulation Engine**: Offline simulation tool that replays historical failure patterns against proposed policy configurations to predict effectiveness.
- **Cross-Service Retry Coordination**: Distributed retry state propagation where upstream services are informed of downstream retry activities to coordinate backpressure.
- **Advanced Cost-Based Retry Decisions**: Retry decisions informed by operation cost (monetary, latency budget, resource consumption) to avoid expensive retries for low-value operations.

## 29. Acceptance Criteria

1. All backoff algorithms produce correct delay values within 1% tolerance of mathematical specification.
2. Circuit breaker correctly transitions through CLOSED→OPEN→HALF_OPEN→CLOSED state machine for all scenarios.
3. Idempotency guarantees: repeated execution with same key returns identical response.
4. Retry state survives process restart: recovered retries resume within 30 seconds of startup.
5. Bulkhead isolation: saturation of one bulkhead does not affect other bulkheads.
6. Retry budget enforcement: operations fail fast when budget is exhausted (failure < 10ms).
7. Metrics accuracy: all counters, histograms, and gauges match actual system behavior within 0.1% tolerance.
8. Degraded mode: system continues operating (with reduced capacity) when persistence backend is unavailable.
9. Concurrency safety: no race conditions or state corruption under 1,000 concurrent retry operations.
10. Configuration validation: all invalid configurations rejected at registration time with descriptive error messages.

## 30. Definition of Done (DoD)

1. All acceptance criteria verified through automated tests.
2. Unit test coverage >= 90% for all backoff algorithms, circuit breaker state machine, and policy validation.
3. Integration tests pass for all persistence backends (Redis, PostgreSQL, embedded).
4. Chaos tests pass: 72-hour soak test with fault injection no unrecovered failures.
5. Performance benchmarks meet all latency and throughput targets.
6. Documentation complete: API reference, configuration guide, operational runbook.
7. Monitoring dashboards created: retry overview, circuit breaker health, bulkhead utilization, idempotency cache performance.
8. Alert rules configured: budget exhaustion, breaker cascade, bulkhead saturation, state persistence failure.
9. Security review completed: authentication, authorization, encryption, audit trail verified.
10. Runbook reviewed by operations team with demonstrated recovery procedures.
11. Configuration schema versioned and backward-compatible.
12. Feature flag exists for disabling retry system if issues discovered in production.

## 31. Detailed Retry Policy Examples

### 31.1 Fixed Interval Policy
The fixed interval policy applies a constant delay between each retry attempt regardless of the attempt number. This is suitable for scenarios where the downstream service needs consistent spacing between retries, such as when polling for asynchronous job completion. Configuration: baseDelayMs=1000, maxRetries=5. Attempt 1 delay 1000ms, attempt 2 delay 1000ms, attempt 3 delay 1000ms, attempt 4 delay 1000ms, attempt 5 delay 1000ms. Total worst-case duration: 5000ms. This policy is simple and predictable but can cause thundering herd when multiple clients retry simultaneously, as all clients will retry at the same fixed intervals.

### 31.2 Exponential Backoff Policy
The exponential backoff policy multiplies the delay by a configurable factor after each attempt, providing progressively longer waits between retries. This is the most common retry strategy for transient failures as it gives the downstream service increasing time to recover. Configuration: baseDelayMs=100, multiplier=2.0, maxDelayMs=10000, maxRetries=6. Attempt 1 delay 100ms, attempt 2 delay 200ms, attempt 3 delay 400ms, attempt 4 delay 800ms, attempt 5 delay 1600ms, attempt 6 delay 3200ms (capped at 10000ms). Total worst-case duration: 6300ms. The exponential growth quickly reduces retry frequency, giving the system time to recover while still attempting multiple retries.

### 31.3 Jittered Exponential Backoff Policy
The jittered exponential backoff policy adds randomization to the exponential backoff delay to prevent thundering herd problems where multiple clients synchronize their retry attempts. Configuration: baseDelayMs=100, multiplier=2.0, maxDelayMs=10000, maxRetries=5, jitterType=DECORRELATED. Using decorrelated jitter, the delay is computed as min(maxDelay, random(baseDelay, previousDelay * 3)). Starting with previousDelay=100ms, attempt 1: random(100, 300) = 247ms, attempt 2: random(100, 741) = 512ms, attempt 3: random(100, 1536) = 823ms, attempt 4: random(100, 2469) = 1567ms, attempt 5: random(100, 4701) = 2893ms. The decorrelated jitter provides the best distribution for high-concurrency scenarios as it naturally spreads retry timings across a wide range.

### 31.4 Incremental Backoff Policy
The incremental backoff policy adds a fixed increment to the delay after each attempt, providing a linear increase in wait time. This is suitable for rate-limited scenarios where the rate limit window is known and fixed. Configuration: baseDelayMs=1000, incrementMs=2000, maxDelayMs=10000, maxRetries=5. Attempt 1 delay 1000ms, attempt 2 delay 3000ms, attempt 3 delay 5000ms, attempt 4 delay 7000ms, attempt 5 delay 9000ms. Total worst-case duration: 25000ms. This policy is useful when the rate limit resets at known intervals, allowing retries to align with reset boundaries.

### 31.5 Fibonacci Backoff Policy
The Fibonacci backoff policy uses Fibonacci numbers as multipliers for the base delay, providing a more gradual increase than exponential in early attempts but faster growth in later attempts. Configuration: baseDelayMs=100, maxDelayMs=10000, maxRetries=7. Attempt 1 delay 100ms (fib 1), attempt 2 delay 100ms (fib 1), attempt 3 delay 200ms (fib 2), attempt 4 delay 300ms (fib 3), attempt 5 delay 500ms (fib 5), attempt 6 delay 800ms (fib 8), attempt 7 delay 1300ms (fib 13). Total worst-case duration: 3300ms. Fibonacci backoff provides a good balance between quick retries for intermittent blips and increasing spacing for sustained outages.

## 32. Advanced Retry Scenarios

### 32.1 Retry with Streaming Operations
For streaming gRPC or HTTP operations, retry logic must account for partial data delivery. The retry system implements stream-aware retry where only the portion of the stream that was not successfully acknowledged is retried. Stream position tracking via client-side acknowledgment markers enables resume from last successful position. The retry policy for streaming operations uses shorter timeouts and fewer retries to avoid excessive data duplication. Configuration: maxRetries=2, baseDelayMs=500, jitterType=FULL. Stream retry creates a new stream connection and resumes from the last acknowledged position.

### 32.2 Retry with Transactional Operations
For operations that modify multiple resources, retry must account for partial completion. The retry system integrates with the AIOS State Manager saga pattern to execute compensating actions for partially completed operations before retrying. Each retry attempt checks the actual state of each resource before re-applying operations. Idempotency keys are critical for transactional retries to prevent duplicate application of completed operations. The retry policy for transactional operations uses idempotency tokens with per-resource tracking and compensating rollback on persistent failure.

### 32.3 Retry with Long-Running Operations
For operations that take more than 30 seconds to complete, the retry system uses an asynchronous retry pattern where the initial request returns a tracking ID, and retries poll for completion status. The retry state for long-running operations includes the polling interval, last known status, and completion timeout. If the operation is still in progress, the retry logic waits and polls again rather than re-initiating the operation. Configuration: pollIntervalMs=5000, completionTimeoutMs=300000, maxRetries=0 (no operation retry, only poll retry).

## 33. Circuit Breaker Advanced Configuration

### 33.1 Per-Operation Circuit Breaker Groups
Circuit breakers can be grouped by operation type, downstream dependency, or client identity. Each group has independent thresholds, windows, and timeouts. For example, a single downstream database may have separate circuit breakers for read operations (failure threshold 5) and write operations (failure threshold 3) since write failures are more impactful. Grouping enables granular failure isolation while sharing the underlying dependency.

### 33.2 Adaptive Circuit Breaker Thresholds
The circuit breaker can automatically adjust thresholds based on observed failure patterns and recovery times. If the system observes that failures are recovering faster, the half-open timeout can be reduced dynamically. If failures are taking longer to resolve, the timeout can be extended. Adaptive thresholds use a sliding window of recovery time observations to calculate the optimal timeout. Minimum timeout: 5 seconds, maximum timeout: 300 seconds. Threshold adjustment is gradual (max 10% change per evaluation) to prevent oscillation.

### 33.3 Circuit Breaker Metrics Collection
Each circuit breaker instance collects detailed metrics for analysis and tuning:
- Request volume: total requests through the breaker over time.
- Failure rate: rolling failure rate over the sliding window.
- Mean time between failures (MTBF): average time between state transitions to OPEN.
- Mean time to recover (MTTR): average time spent in OPEN state before recovery.
- Concurrency: number of in-flight requests through the breaker.
- Rejection rate: requests rejected due to OPEN state.
These metrics are exported to the monitoring system for dashboard visualization and alerting.

## 34. Bulkhead Advanced Patterns

### 34.1 Dynamic Bulkhead Sizing
Bulkhead thread pools can be dynamically resized based on observed demand and system capacity. The sizing algorithm monitors queue depth, rejection rate, and thread utilization to compute optimal pool size. When rejection rate exceeds 5% over a 60-second window, the pool size is increased by 20% (up to configured maximum). When thread utilization is below 30% for 300 seconds, the pool size is decreased by 10% (down to configured minimum). Pool size changes are logged and emitted as metrics events.

### 34.2 Bulkhead with Priority Queuing
Each bulkhead supports priority queuing where retry operations are assigned a priority based on the original operation criticality. Higher priority retries bypass lower priority retries in the queue. Priority levels: CRITICAL (P0), HIGH (P1), MEDIUM (P2), LOW (P3), BEST_EFFORT (P4). Within each priority level, retries are processed in FIFO order. Priority inversion prevention ensures that lower priority operations are not starved by higher priority operations through aging mechanisms.

### 34.3 Bulkhead with Timeout Per Operation
Each retry operation within a bulkhead has a configurable timeout. If the retry operation (including the downstream call and any processing) exceeds the timeout, the operation is cancelled and the thread is released back to the pool. Dead retry detection: retry operations that have been executing for more than 5x the expected operation duration are forcefully interrupted. Deadline propagation ensures that the retry timeout is respected by downstream calls through context deadline propagation.

## 35. Retry System Performance Characteristics

### 35.1 Baseline Performance (Single Instance)
- Retry decision (no retry needed): 50 microseconds P50, 150 microseconds P99.
- Retry decision (with retry needed): 200 microseconds P50, 500 microseconds P99.
- Idempotency key generation: 5 microseconds P50, 20 microseconds P99.
- Idempotency cache lookup (L1 hit): 10 microseconds P50, 50 microseconds P99.
- Idempotency cache lookup (L2 hit): 500 microseconds P50, 5 milliseconds P99.
- Circuit breaker state check: 2 microseconds P50, 10 microseconds P99.
- Bulkhead acquire: 5 microseconds P50, 50 microseconds P99.
- State persistence write (Redis): 1 millisecond P50, 5 milliseconds P99.

### 35.2 Throughput
- Maximum retry decisions per second: 100,000 (single instance).
- Maximum concurrent in-flight retries: 10,000 (default bulkhead configuration).
- Maximum idempotency cache writes per second: 50,000 (Redis cluster).
- Maximum circuit breaker state transitions per second: 10,000.

### 35.3 Resource Consumption
- CPU: 0.1 core per 10,000 retry decisions per second.
- Memory: 500MB baseline + 50KB per active in-flight retry.
- Network: 100 bytes per retry decision (metrics emission).
- Redis connections: 10-50 connections per instance.

## 36. Retry Protocol Specification

### 36.1 gRPC Retry Interceptor Specification
The gRPC retry interceptor wraps all outgoing gRPC calls with retry logic. It intercepts the call at the client interceptor level, before the call reaches the network layer. On receiving a response or error, the interceptor evaluates retry eligibility based on gRPC status code. Codes retried: Unavailable (14), DeadlineExceeded (4), ResourceExhausted (8) for rate limiting. Codes not retried: OK (0), InvalidArgument (3), NotFound (5), PermissionDenied (7), Unauthenticated (16). The interceptor reads retry policy from the client call options, falling back to the default policy for the service. It injects the idempotency key into gRPC metadata under the key "idempotency-key". The interceptor records per-attempt timing and propagates deadline-adjusted context to each retry attempt.

### 36.2 HTTP Retry Interceptor Specification
The HTTP retry interceptor wraps outgoing HTTP requests with retry logic. It evaluates HTTP status codes for retry eligibility. Status codes retried: 408 (Request Timeout), 425 (Too Early), 429 (Too Many Requests), 500 (Internal Server Error), 502 (Bad Gateway), 503 (Service Unavailable), 504 (Gateway Timeout). Status codes not retried: all 2xx, 3xx, 400, 401, 403, 404, 405, 406, 409, 410, 411, 413, 422. The interceptor reads the Retry-After header from HTTP 429 and 503 responses and uses it as the delay for the next retry attempt (if present). It injects the idempotency key as the HTTP header "Idempotency-Key". The interceptor respects the request context deadline and cancels retries when the deadline is exceeded.

### 36.3 Retry Policy Propagation
Retry policies are propagated through distributed systems via context propagation. The calling service's retry policy is included in the request context as a serialized RetryPolicy message. Downstream services read the propagated policy and use it for their own retry decisions. If a downstream service needs to override the propagated policy (e.g., due to local constraints), it can do so but must log the override. Retry budget consumption is tracked across the call chain, with each service decrementing the remaining budget. This prevents retry amplification where each service in a chain independently retries and multiplies the total retry count.

## 37. Retry System Integration Points

### 37.1 Integration with AIOS API Gateway
The AIOS API Gateway integrates with the retry system for handling upstream client request retries. The gateway extracts idempotency keys from incoming requests and uses them for deduplication. If the gateway receives a duplicate request with the same idempotency key and the original request is still in-flight, it waits for the original response and returns it to the duplicate caller. This prevents duplicate processing of the same client request. The gateway also applies retry policies for downstream service calls made during request processing, ensuring that internal retries are transparent to the client.

### 37.2 Integration with AIOS Message Queue
The AIOS Message Queue (Kafka/RabbitMQ) integrates with the retry system for message processing retries. When a consumer fails to process a message, it can either retry immediately (synchronous retry) or send the message to a retry queue with a delivery delay (asynchronous retry). The retry queue has configurable delay and max delivery count. After exhausting retries, the message is sent to a dead letter queue for manual inspection. The retry system provides the retry policy configuration (max deliveries, delay algorithm, dead letter routing) for each consumer group.

### 37.3 Integration with AIOS Database Connector
The AIOS Database Connector integrates with the retry system for handling database operation retries. Transient database errors (serialization failures, deadlocks, connection timeouts) are automatically retried. The retry policy for database operations uses incremental backoff with max 3 retries. Transactional operations that fail are retried at the transaction level (the entire transaction is retried, not individual statements). The retry system tracks which statements within a transaction succeeded before failure and logs this information for debugging.

## 38. Retry System Testing Framework

### 38.1 Unit Test Coverage Requirements
The retry system mandates the following minimum unit test coverage: backoff algorithm implementations: 100% coverage of mathematical computation for all algorithms. Circuit breaker state machine: 100% coverage of all state transitions and edge cases. Policy validation: 100% coverage of all validation rules and constraint checks. Error classification: 100% coverage of all built-in error type classifications. Idempotency key generation: 100% coverage of format validation and uniqueness guarantees. Bulkhead configuration: 100% coverage of thread pool configuration and rejection behavior.

### 38.2 Integration Test Scenarios
Integration test scenarios that must pass before release: Redis state persistence: CRUD operations, TTL expiration, connection loss and recovery. PostgreSQL state persistence: transaction commit and rollback, schema migration. End-to-end retry: configure mock service to return specific errors and verify retry behavior for each error type. Idempotency verification: send same request twice with same idempotency key, verify identical response. Circuit breaker integration: verify that OPEN breaker correctly blocks requests and transitions to HALF_OPEN after timeout.

### 38.3 Chaos Engineering Test Scenarios
Regular chaos experiments validate retry system resilience: Random service failure injection: 50% of downstream calls fail for 60 seconds, verify retry system maintains correct behavior. Redis cluster failover: force Redis master failover, verify retry state persistence survives. Network latency injection: add 500ms latency to 25% of requests, verify retry budgets prevent cascading. CPU pressure: consume 80% CPU on retry system node, verify decision latency stays within P99 target. Bulkhead saturation: trigger maximum concurrent retries, verify new operations fail with BulkheadFullError.

## 39. Retry System Deployment Topology

### 39.1 Single Region Deployment
In a single-region deployment, the retry system operates with a single Redis cluster for state persistence and circuit breaker state. Multiple retry system instances run behind a load balancer for high availability. Redis Sentinel provides automatic failover for the Redis cluster. Each retry system instance maintains its own in-memory caches (idempotency, policy, breaker state) and synchronizes state changes via Redis. This topology supports up to 10,000 retry decisions per second with 99.99% availability.

### 39.2 Multi-Region Active-Active Deployment
In a multi-region active-active deployment, each region has its own retry system instances and Redis cluster. Cross-region retry state replication is performed via Redis replication or Kafka mirroring. Idempotency keys are prefixed with region identifier to prevent cross-region collisions. Circuit breaker state is region-local (a downstream service failure in one region does not affect other regions). This topology supports up to 50,000 retry decisions per second globally with 99.999% availability and RPO of 1 second for cross-region state replication.

### 39.3 Multi-Region Active-Passive Deployment
In a multi-region active-passive deployment, one region is primary and handles all retry decisions. The secondary region maintains a warm standby with replicated retry state. On primary region failure, DNS or load balancer routing switches traffic to the secondary region within 60 seconds. Retry state from the primary is available in the secondary with RPO of 5 seconds (asynchronous replication). In-flight retries that were in the primary at the time of failure are recovered from the replicated state and resumed in the secondary. This topology provides cost-effective disaster recovery with RTO of 60 seconds and RPO of 5 seconds.

## 40. Retry System Cost Analysis

### 40.1 Infrastructure Cost
The retry system infrastructure cost includes: Redis cluster nodes (3 primary + 3 replica = 6 nodes at $200/month each = $1,200/month for small deployment). Retry system instances (2 instances at $100/month each = $200/month). PostgreSQL database for optional durable state (1 instance at $300/month). Total infrastructure cost: $1,700/month for a production deployment serving up to 10,000 decisions/second.

### 40.2 Operational Cost Savings
The retry system reduces operational costs through: Reduced manual incident response: automatic retry recovery reduces incident count by estimated 40%. Reduced developer debugging time: structured retry logs and metrics reduce MTTR by estimated 30%. Reduced infrastructure over-provisioning: circuit breakers prevent cascading failures, reducing the need for over-provisioned capacity. Estimated operational cost savings: $5,000-$15,000/month for a medium-scale deployment (10 services, 100 agents).

### 40.3 Business Impact
The retry system improves business metrics by: Increased operation success rate: 99.9% transient failure recovery vs. 95% without retry system. Reduced end-user impact: transient failures that would result in user-facing errors are automatically recovered. Improved developer productivity: developers focus on business logic instead of retry implementation. Estimated business value: $20,000-$50,000/month in reduced error-related revenue loss and improved developer velocity.
