# AIOS Tool Manager — Engineering Specification v2.0

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
The AIOS Tool Manager is the centralized registry, lifecycle manager, and execution orchestrator for all tools available to AI agents within the AIOS ecosystem. Tools are executable capabilities that AI agents can invoke to interact with external systems, perform computations, retrieve data, or trigger side effects. A tool can be as simple as a function that returns the current weather or as complex as a multi-step workflow that provisions cloud infrastructure. The Tool Manager provides a uniform interface for tool definition, registration, discovery, schema exposition, invocation, and result handling. It enforces tool-level security policies, resource quotas, execution timeouts, and auditing. Without the Tool Manager, each agent would need to implement its own tooling infrastructure, leading to fragmentation, duplicated implementation, security holes, and an inconsistent developer experience. The Tool Manager serves as the single source of truth for all agent-accessible tooling.

### 1.2 Mission
To provide a secure, scalable, and observable tool execution platform that allows AI agents to safely invoke external capabilities with minimal latency and maximum reliability. The Tool Manager aims to reduce the effort required to add a new tool to the AIOS ecosystem from days to minutes, while enforcing consistent security, audit, and reliability guarantees across all tools. The Tool Manager ensures that every tool invocation is authorized, resource-limited, monitored, and fully traceable, regardless of whether the tool is a built-in system tool, a custom tool defined by a plugin, or a dynamically registered third-party tool.

### 1.3 Responsibilities
- **Tool Registration**: Accepting tool definitions from various sources (system built-ins, plugins, configuration files, dynamic registration via API) and storing them in a central registry
- **Tool Lifecycle Management**: Managing tool states through lifecycle stages: UNREGISTERED → REGISTERED → VALIDATED → ACTIVE → DEPRECATED → DISABLED → UNREGISTERED
- **Tool Discovery**: Exposing a queryable API for agents and other consumers to list, search, and introspect available tools and their schemas
- **Tool Schema Exposition**: Providing JSON Schema for each tool's input parameters and output format, enabling LLMs to generate correct function call arguments
- **Tool Invocation**: Accepting a tool call with arguments, validating the arguments against the schema, dispatching execution to the appropriate handler, and returning the result
- **Execution Isolation**: Running tool execution in sandboxed environments (subprocess, container, WASM) depending on tool trust level
- **Resource Quota Enforcement**: Enforcing CPU time, memory, network, and duration limits per tool invocation
- **Secrets Management**: Providing a structured way for tools to reference secrets (API keys, tokens, passwords) without exposing them to the agent or in logs
- **Audit Logging**: Recording every tool invocation with full provenance: agent, tool, arguments (with secrets redacted), result summary, duration, success/failure
- **Error Handling**: Catching and categorizing tool errors (timeout, invalid response, resource exhaustion, internal error) and returning structured error responses
- **Tool Version Management**: Supporting versioned tool definitions with semantic versioning, allowing agents to pin to specific tool versions
- **Tool Caching**: Caching tool results when the tool declares itself as cacheable and the invocation parameters match a previous successful invocation
- **Tool Metrics**: Producing metrics about tool usage, latency, error rates, and resource consumption

### 1.4 Non-Responsibilities
- The Tool Manager does NOT implement tool logic itself (tools are defined by system components, plugins, or external services)
- The Tool Manager does NOT decide which tool to call or when to call it (that is the agent's responsibility, guided by the LLM)
- The Tool Manager does NOT manage external services that tools integrate with (databases, APIs, filesystems)
- The Tool Manager does NOT implement cross-tool orchestration or sequencing (that is the Workflow Engine's responsibility)
- The Tool Manager does NOT manage the lifecycle of external resources created by tools (e.g., if a tool creates an S3 bucket, the Tool Manager does not manage that bucket)
- The Tool Manager does NOT implement semantic understanding of tool inputs and outputs (that is the LLM's responsibility)
- The Tool Manager does NOT authenticate or authorize the calling agent (it relies on the Authentication and Authorization systems for that)
- The Tool Manager does NOT store tool output data persistently (it returns the result to the caller)

---

## 2. Architecture, Internal Components, External Components

### 2.1 Overall Architecture
The Tool Manager is built as a modular service with pluggable execution backends, a central registry, a schema validator, and a security sandbox layer. It communicates with agents via gRPC, with plugins via a registered plugin interface, and with external systems via HTTP or other transport protocols depending on the tool definition.

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                          AIOS Tool Manager Service                             │
│                                                                                │
│  ┌──────────────────────────────────────────────────────────────────────┐    │
│  │                        Tool Manager API (gRPC/REST)                    │    │
│  │  /tools/register | /tools/{tool} | /tools/{tool}/execute              │    │
│  └──────────────────────────────────────────────────────────────────────┘    │
│                                    │                                           │
│  ┌──────────────────────────────────────────────────────────────────────┐    │
│  │  ┌────────────────┐ ┌──────────────┐ ┌──────────────┐ ┌────────────┐│    │
│  │  │ Tool Registry  │ │ Schema       │ │ Argument     │ │ Execution  ││    │
│  │  │ (Lifecycle)    │ │ Validator    │ │ Sanitizer    │ │ Dispatcher ││    │
│  │  └────────────────┘ └──────────────┘ └──────────────┘ └────────────┘│    │
│  │  ┌────────────────┐ ┌──────────────┐ ┌──────────────┐ ┌────────────┐│    │
│  │  │ Resource       │ │ Result       │ │ Secrets      │ │ Rate       ││    │
│  │  │ Quota Enforcer │ │ Cache        │ │ Resolver     │ │ Limiter    ││    │
│  │  └────────────────┘ └──────────────┘ └──────────────┘ └────────────┘│    │
│  │  ┌────────────────┐ ┌──────────────┐ ┌──────────────┐ ┌────────────┐│    │
│  │  │ Tool Lifecycle │ │ Dependency   │ │ Audit        │ │ Metrics    ││    │
│  │  │ State Machine  │ │ Resolver     │ │ Logger       │ │ Collector  ││    │
│  │  └────────────────┘ └──────────────┘ └──────────────┘ └────────────┘│    │
│  └──────────────────────────────────────────────────────────────────────┘    │
│                                    │                                           │
│  ┌──────────────────────────────────────────────────────────────────────┐    │
│  │                     Execution Backends (Pluggable Renderers)            │    │
│  │  ┌────────────┐ ┌────────────┐ ┌────────────┐ ┌────────────────────┐  │    │
│  │  │ Direct     │ │ Subprocess │ │ Container  │ │ WASM Sandbox       │  │    │
│  │  │ Function   │ │ Sandbox    │ │ Sandbox    │ │ (Plugin Runtime)   │  │    │
│  │  └────────────┘ └────────────┘ └────────────┘ └────────────────────┘  │    │
│  │  ┌────────────┐ ┌────────────┐ ┌────────────┐                          │    │
│  │  │ Remote     │ │ HTTP/HTTPS │ │ Python     │                          │    │
│  │  │ gRPC       │ │ Function   │ │ Executor   │                          │    │
│  │  └────────────┘ └────────────┘ └────────────┘                          │    │
│  └──────────────────────────────────────────────────────────────────────┘    │
│                                    │                                           │
│  ┌──────────────────────────────────────────────────────────────────────┐    │
│  │                      External Interfaces                                │    │
│  │  ┌────────────┐ ┌────────────┐ ┌────────────┐ ┌────────────────────┐  │    │
│  │  │ Secrets   │ │ Config     │ │ Plugin     │ │ Model Manager      │  │    │
│  │  │ Vault     │ │ System     │ │ System     │ │ (for AI tools)     │  │    │
│  │  └────────────┘ └────────────┘ └────────────┘ └────────────────────┘  │    │
│  │  ┌────────────┐ ┌────────────┐ ┌────────────┐ ┌────────────────────┐  │    │
│  │  │ Database   │ │ Message    │ │ Event       │ │ External APIs      │  │    │
│  │  │ Manager    │ │ Broker     │ │ System      │ │ (weather, etc.)    │  │    │
│  │  └────────────┘ └────────────┘ └────────────┘ └────────────────────┘  │    │
│  └──────────────────────────────────────────────────────────────────────┘    │
└──────────────────────────────────────────────────────────────────────────────┘
```

### 2.2 Internal Components

#### 2.2.1 Tool Registry
The central in-memory data store for all tool definitions. Each tool entry includes:
- Unique tool ID (UUID)
- Tool name (unique, human-readable, namespaced like `system.file.read`, `weather.current`)
- Version (semver)
- Description for LLM consumption
- JSON Schema for input parameters
- JSON Schema for output values
- Execution mode: DIRECT, SUBPROCESS, CONTAINER, WASM, REMOTE_HTTP, REMOTE_GRPC
- Execution endpoint or reference (function pointer, URL, WASM module)
- Authentication requirements (secrets needed, auth type)
- Caching configuration (ttl, key template)
- Resource limits (cpu_max_ms, mem_max_mb, timeout_ms, network_allowed)
- Tool state: REGISTERED, VALIDATED, ACTIVE, DEPRECATED, DISABLED
- Tags and categories for discovery
- Dependencies on other tools or plugins

The registry supports concurrent access with a RWMutex: reads are shared, writes are exclusive. The registry snapshot is periodically persisted to the Database Manager for recovery.

#### 2.2.2 Schema Validator
A high-performance JSON Schema validator (implemented using a compiled schema approach). It validates tool call arguments against the input schema before the tool is invoked, returning a structured validation error with field-level detail. The validator caches compiled schemas keyed by (tool_id, schema_version). It supports all JSON Schema draft-07 features: required, type, enum, pattern, minLength, maxLength, minimum, maximum, properties, additionalProperties, oneOf, anyOf, allOf, $ref (internal references only), and format (date-time, uri, email, regex, etc.).

#### 2.2.3 Argument Sanitizer
Before arguments reach the validator, the sanitizer strips or rejects dangerous content:
- Strips control characters from string arguments
- Prevents argument injection attacks (checks for shell metacharacters when the execution mode is SUBPROCESS)
- Rejects arguments that exceed maximum string length (configurable per tool)
- Rejects arguments that would cause the payload to exceed the maximum request size
- Redacts sensitive content from audit logs (based on secret field annotations in the schema)

#### 2.2.4 Execution Dispatcher
The dispatcher selects the appropriate execution backend for the tool based on its execution mode, instantiates the sandbox if needed, passes the sanitized arguments, and waits for the result. It enforces the resource quota (CPU, memory, timeout) by setting OS-level limits (for subprocess) or cgroup limits (for container). The dispatcher also handles streaming tool outputs: some tools produce multiple results over time (e.g., a file watcher), and the dispatcher supports streaming those results back to the caller through a gRPC server-streaming RPC.

#### 2.2.5 Resource Quota Enforcer
Enforces per-tool and per-caller resource limits:
- CPU time limit (configurable, default 30 seconds)
- Memory limit (configurable, default 256 MB)
- Execution timeout (configurable, default 60 seconds)
- Network access (allowed/denied URL patterns)
- Filesystem access (allowed/denied paths for file system tools)
- Number of concurrent tool executions per agent (default 5)

The enforcer uses cgroups for container execution, rlimits for subprocess execution, and timeouts for all modes. If a tool exceeds any limit, it is terminated immediately with a structured error.

#### 2.2.6 Result Cache
An optional caching layer for tools that declare themselves as cacheable. Each cacheable tool specifies a `cache_ttl` (time-to-live) and a `cache_key_template` that uses argument values to build the cache key. For example, a weather tool with arguments `{lat, lon}` might have a cache key `weather:{lat}:{lon}` and a TTL of 300 seconds. The cache is stored in Redis and is invalidated on:

- TTL expiration
- Manual invalidation via API
- Tool version change
- Tool configuration change that affects the result

Cache hit ratios are tracked per tool.

#### 2.2.7 Secrets Resolver
Before executing a tool that requires credentials, the Secrets Resolver fetches the required secrets from the Secrets Vault and injects them into the tool's environment. The agent never sees the secret values, and the secret reference (e.g., `${{ secrets.weather_api_key }}`) is redacted in audit logs. The resolver supports:

- Static secrets: fetched once at startup
- Dynamic secrets: fetched on each invocation (for short-lived credentials)
- Token exchange: OAuth2 token exchange with automatic refresh
- Header injection: for HTTP-tools that require Bearer tokens

#### 2.2.8 Audit Logger
Records every tool invocation as a structured JSON log entry. Log entries include:
- `event_type`: `tool.invocation`, `tool.invocation.result`, `tool.invocation.error`
- `timestamp`, `tool_id`, `tool_version`, `agent_id`, `session_id`, `request_id`
- `arguments_hash`: SHA256 of the arguments (for deduplication, privacy)
- `arguments_redacted`: arguments with secret fields replaced with `[REDACTED]`
- `result_summary`: size, type, or status of the result (never the full result for privacy)
- `duration_ms`, `success`, `error_code`, `error_message`
- `resource_usage`: `cpu_ms`, `memory_mb`, `network_bytes`

#### 2.2.9 Metrics Collector
Collects and exposes Prometheus metrics for all tool operations (see Monitoring section).

#### 2.2.10 Tool Dependency Resolver
For tools that depend on other tools (e.g., a `report.generate` tool that depends on `database.query` and `file.write`), the dependency resolver ensures that all dependencies are ACTIVE before allowing the tool to be invoked. It builds a dependency graph and rejects invocations with a clear error if any dependency is not met.

### 2.3 External Components

- **Secrets Vault**: Stores and serves secrets referenced by tools
- **Configuration System**: Provides tool definitions, limits, and policies
- **Event System**: Receives tool events (tool.invocation, tool.registered, tool.deprecated)
- **Database System**: Persists tool registry snapshots and audit logs
- **Monitoring System**: Scrapes tool metrics
- **Plugin System**: Provides custom tool definitions from plugins
- **Model Manager**: Provides tools that wrap LLM inference calls
- **Authentication/Authorization Services**: Validate caller identity and permissions
- **External Services**: The actual systems that tools interact with (APIs, databases, filesystems)

---

## 3. Dependencies, Inputs, Outputs

### 3.1 Internal Dependencies
- Secrets Vault (mandatory for any tool using secrets)
- Configuration System (mandatory for tool definitions)
- Event System (mandatory for event emission)
- Authentication System (mandatory for caller identity)
- Authorization System (mandatory for permission checks)
- Plugin System (mandatory for plugin-provided tools)
- Database System (recommended for registry persistence)

### 3.2 External Dependencies
- None for core functionality; external dependencies are per-tool

### 3.3 Inputs

#### 3.3.1 Tool Registration Request
```json
{
  "tool_name": "weather.current",
  "version": "1.0.0",
  "description": "Get the current weather for a city. Supports cities worldwide.",
  "input_schema": {
    "$schema": "https://json-schema.org/draft/2020-12/schema",
    "type": "object",
    "properties": {
      "city": {"type": "string", "description": "City name"},
      "units": {"type": "string", "enum": ["celsius", "fahrenheit"], "default": "celsius"}
    },
    "required": ["city"]
  },
  "output_schema": {
    "type": "object",
    "properties": {
      "temperature": {"type": "number"},
      "humidity": {"type": "number"},
      "conditions": {"type": "string"}
    }
  },
  "execution_mode": "REMOTE_HTTP",
  "execution_endpoint": "https://api.weather.com/v1/current?city={{city}}&units={{units}}",
  "auth": {
    "type": "api_key",
    "header": "X-API-Key",
    "secret_ref": "secrets.weather_api_key"
  },
  "resource_limits": {
    "timeout_ms": 10000,
    "memory_mb": 64,
    "network_allowed": true
  },
  "cache": {
    "enabled": true,
    "ttl_seconds": 300,
    "key_template": "weather:{city}:{units}"
  },
  "tags": ["weather", "public"],
  "owner": "aios-system"
}
```

#### 3.3.2 Tool Invocation Request
```json
{
  "tool_id": "weather.current@1.0.0",
  "arguments": {
    "city": "London",
    "units": "celsius"
  },
  "request_id": "uuid",
  "timeout_ms": 10000,
  "cache_policy": "default"  // "default", "bypass", "refresh"
}
```

### 3.4 Outputs

#### 3.4.1 Tool Result (Success)
```json
{
  "request_id": "uuid",
  "tool_id": "weather.current@1.0.0",
  "status": "success",
  "result": {
    "temperature": 15.2,
    "humidity": 72,
    "conditions": "Partly cloudy"
  },
  "metadata": {
    "duration_ms": 342,
    "cached": false,
    "resource_usage": {
      "cpu_ms": 120,
      "memory_mb": 32,
      "network_bytes": 1024
    }
  }
}
```

#### 3.4.2 Tool Invocation (Error)
```json
{
  "request_id": "uuid",
  "tool_id": "weather.current@1.0.0",
  "status": "error",
  "error": {
    "code": "TOOL_TIMEOUT",
    "message": "Tool execution exceeded the configured timeout of 10000ms",
    "retryable": true,
    "retry_after_ms": 5000
  },
  "metadata": {
    "duration_ms": 10001,
    "resource_usage": {
      "cpu_ms": 9500,
      "memory_mb": 48,
      "network_bytes": 0
    }
  }
}
```

---

## 4. Data Structures, Execution Flow, State Management

### 4.1 Core Data Structures

#### 4.1.1 ToolDefinition
```go
type ToolDefinition struct {
    ID               string           // namespaced unique identifier
    Version          string           // semver
    Description      string
    InputSchema      json.RawMessage  // JSON Schema
    OutputSchema     json.RawMessage  // JSON Schema
    ExecutionMode    ExecutionMode    // DIRECT, SUBPROCESS, CONTAINER, WASM, REMOTE_HTTP, REMOTE_GRPC
    Endpoint         string           // URL, WASM module path, or function reference
    Auth             *ToolAuth        // optional
    ResourceLimits   ResourceLimits
    CacheConfig      *CacheConfig
    State            ToolState        // REGISTERED, VALIDATED, ACTIVE, DEPRECATED, DISABLED
    Tags             []string
    Dependencies     []string         // tool IDs this tool depends on
    CreatedAt        time.Time
    UpdatedAt        time.Time
}
```

#### 4.1.2 ToolInvocation
```go
type ToolInvocation struct {
    ID            string
    ToolID        string
    ToolVersion   string
    AgentID       string
    SessionID     string
    RequestID     string
    Arguments     json.RawMessage
    Status        InvocationStatus   // PENDING, RUNNING, SUCCESS, ERROR, TIMEOUT, CANCELLED
    Result        json.RawMessage
    Error         *ToolError
    DurationMS    int64
    ResourceUsage ResourceUsage
    Cached        bool
    StartedAt     time.Time
    CompletedAt   time.Time
}
```

#### 4.1.3 ResourceLimits
```go
type ResourceLimits struct {
    CPUMaxMS              int64   // max CPU time in milliseconds
    MemoryMaxMB           int64   // max memory in MB
    TimeoutMS             int64   // max wall-clock time in milliseconds
    NetworkAllowed        bool
    AllowedDomains        []string // if NetworkAllowed, which domains
    MaxConcurrentInvocations int32 // per-agent limit
}
```

#### 4.1.4 ToolRegistry
```go
type ToolRegistry struct {
    mutex   sync.RWMutex
    tools   map[string]*ToolDefinition  // key: "tool_id@version"
    aliases map[string]string           // key: "tool_id" -> "tool_id@latest_version"
}
```

#### 4.1.5 CacheConfig
```go
type CacheConfig struct {
    Enabled      bool
    TTLSeconds   int64
    KeyTemplate  string               // uses {arg_name} placeholders
}
```

### 4.2 Execution Flow

#### 4.2.1 Tool Registration Flow
1. **Ingress**: A registration request arrives (from system bootstrap, plugin activation, or API call).
2. **Validation**: The tool definition is validated for completeness: ID format, version format, input/output schemas, execution mode, endpoint format.
3. **Dependency Check**: If the tool declares dependencies, they are checked against the existing registry. If a dependency is not yet registered, the tool is placed in PENDING state until the dependency is registered.
4. **Registration**: The tool is added to the in-memory registry with status REGISTERED.
5. **Validation**: If the execution mode is REMOTE_HTTP or REMOTE_GRPC, the endpoint is health-checked with a lightweight request. If it fails, the tool enters VALIDATION_FAILED state.
6. **Activation**: If validation passes, the tool status is set to ACTIVE. If validation fails, it remains VALIDATED with a warning.
7. **Event**: A `tool.registered` event is emitted with the tool ID and version.

#### 4.2.2 Tool Invocation Flow
1. **Ingress**: An invocation request arrives from an agent.
2. **Authentication**: The agent identity is verified by the Authentication System.
3. **Authorization**: The agent's permissions are checked: does the agent have `tools.execute.<tool_id>` or `tools.execute.*`?
4. **Cache Check**: If caching is enabled, the cache is checked. If hit, the cached result is returned immediately with `cached: true`.
5. **Tool Lookup**: The tool is looked up in the registry by ID. If the version is not specified, the latest version alias is used.
6. **Tool State Check**: If the tool is DEPRECATED, a warning header is added to the response. If DISABLED or UNREGISTERED, an error is returned.
7. **Dependency Check**: If the tool has dependencies, they are resolved and validated.
8. **Argument Validation**: The input arguments are validated against the input schema.
9. **Argument Sanitization**: Potentially dangerous content is sanitized.
10. **Resource Quota**: A resource quota slot is acquired. If the agent already has too many concurrent tool invocations, an error is returned.
11. **Secret Resolution**: Any secrets referenced by the tool are fetched from the Secrets Vault.
12. **Execution**: The Execution Dispatcher selects the appropriate backend and runs the tool.
13. **Result Processing**: The output is validated against the output schema (if defined).
14. **Cache Write**: If caching is enabled and successful, the result is stored in the cache.
15. **Audit Logging**: The invocation is written to the audit log.
16. **Metrics**: All metrics are emitted.
17. **Response**: The result (or error) is returned to the caller.

#### 4.2.3 Tool Deprecation/Disabling Flow
1. A config change or API request marks a tool as DEPRECATED or DISABLED.
2. For DEPRECATED: existing invocations are allowed to complete, but new invocations return a warning. The tool is still listed in discovery.
3. For DISABLED: in-flight invocations are cancelled (graceful shutdown with a 5-second timeout). The tool is removed from discovery.
4. An event is emitted (`tool.deprecated` or `tool.disabled`).
5. Dependent tools are notified (their dependency check will now fail or warn).

### 4.3 State Management

#### 4.3.1 Tool States
```
UNREGISTERED ──> REGISTERED ──> VALIDATED ──> ACTIVE
                                                     │
                                                     ├──> DEPRECATED ──> DISABLED ──> UNREGISTERED
                                                     │
                                                     └──> ACTIVE (updated)
```

#### 4.3.2 Invocation States
```
PENDING ──> RUNNING ──> SUCCESS / ERROR / TIMEOUT / CANCELLED
```

#### 4.3.3 Persistence
- The tool registry is persisted to the Database System on every change (write-through cache pattern)
- The invocation audit trail is streamed to the Logging System (not stored locally)
- The result cache is stored in Redis
- Invocation state is not persisted (invocations are ephemeral)

---

## 5. Communication, APIs, Events, Queues

### 5.1 Communication

- **Internal**: gRPC (bidirectional streams for streaming tools, unary for standard invocation)
- **External**: REST over HTTPS for administrative operations (registration, listing, deprecation)
- **Tool Backends**: HTTP/HTTPS, gRPC, subprocess stdin/stdout, WASM call interface

### 5.2 APIs

| Method | Path | Description |
|---|---|---|
| `POST` | `/api/v1/tools` | Register a new tool |
| `PUT` | `/api/v1/tools/{id}` | Update a tool definition |
| `DELETE` | `/api/v1/tools/{id}` | Unregister a tool |
| `GET` | `/api/v1/tools` | List all tools (with filter and pagination) |
| `GET` | `/api/v1/tools/{id}` | Get tool details |
| `PUT` | `/api/v1/tools/{id}/state` | Change tool state (DEPRECATED, DISABLED, ACTIVE) |
| `POST` | `/api/v1/tools/{id}/invoke` | Invoke a tool |
| `POST` | `/api/v1/tools/invoke-batch` | Batch invoke tools (max 10) |
| `GET` | `/api/v1/tools/{id}/schema` | Get JSON Schema for a tool |
| `DELETE` | `/api/v1/cache/tools/{id}` | Invalidate cache for a tool |

### 5.3 Events

| Event | Payload | Trigger |
|---|---|---|
| `tool.registered` | `{tool_id, version, tags, owner}` | Tool registered and activated |
| `tool.updated` | `{tool_id, version, changes}` | Tool definition updated |
| `tool.deprecated` | `{tool_id, version, deprecation_message}` | Tool marked DEPRECATED |
| `tool.disabled` | `{tool_id, version, reason}` | Tool marked DISABLED |
| `tool.invocation.started` | `{tool_id, agent_id, request_id}` | Tool invocation begins |
| `tool.invocation.completed` | `{tool_id, agent_id, request_id, status, duration_ms}` | Tool invocation ends |
| `tool.cache.hit` | `{tool_id, cache_key}` | Successful cache hit |
| `tool.error.secret_missing` | `{tool_id, secret_ref}` | Referenced secret not found |

### 5.4 Queues

- **Invocation Queue**: Requests are queued when the Resource Quota Enforcer's concurrent invocation limit is reached. Each agent has a queue that is processed FIFO when slots become available.
- **Retry Queue**: Tools that fail with a retryable error are re-queued with exponential backoff.
- **Cache Invalidation Queue**: Cache invalidation requests are processed via a secondary queue to avoid blocking invocation paths.

---

## 6. Caching, Memory, Persistence

### 6.1 Caching

#### 6.1.1 Result Cache
- Backend: Redis
- Key format: `tool:result:{tool_id}:{version}:{sha256(arguments_json)}`
- TTL: Per-tool configuration (default 60 seconds, max 86400 seconds)
- Eviction: Redis LRU with TTL
- Opt-in: tools must declare `cache.enabled: true` to use the cache

#### 6.1.2 Schema Cache
- Backend: In-memory LRU
- Key format: `schema:{tool_id}:{version}`
- TTL: Indefinite (invalidated on tool update)
- Purpose: Avoid recompiling JSON Schema on every invocation

#### 6.1.3 Tool Registry Snapshot
- Backend: In-memory atomic pointer
- Purpose: Lock-free reads of the entire tool registry
- Updated on every successful registration/update/deprecation

### 6.2 Memory

- Tool Registry: ~2KB per tool, for 1,000 tools = ~2MB
- Schema Cache: ~10KB per schema, for 1,000 tools = ~10MB
- Invocation State: ~500 bytes per in-flight invocation, for 500 concurrent = ~250KB
- Execution environment: ~50MB for the service process itself
- Total estimated: 100MB–200MB under normal load

### 6.3 Persistence

- Tool registry definitions are persisted in the Database System (PostgreSQL)
- Invocation audit logs are streamed to the Logging System (not locally stored)
- Result cache is persisted in Redis (with TTL, loss is acceptable)
- Configuration is persisted in the Configuration System

---

## 7. Validation, Retry Logic, Error Recovery

### 7.1 Validation

#### 7.1.1 Tool Definition Validation
- Tool ID must match pattern: `[a-z0-9][a-z0-9._-]{1,255}`
- Version must be valid semver: `MAJOR.MINOR.PATCH`
- Input and output schemas must be valid JSON Schema (draft-07 or later)
- Executable mode must be a supported mode
- If REMOTE_HTTP, endpoint must be valid HTTPS URL
- Resource limits must be within the maximum allowed
- Cache TTL if set must be positive

#### 7.1.2 Invocation Validation
- Tool ID must exist in registry
- Arguments must be valid JSON
- Arguments must conform to the input schema
- If a tool requires network access, the request must allow it
- Timeout must be less than or equal to the tool's max timeout

#### 7.1.3 Run-Time Validation
- Output is validated against the output schema (if applicable)
- Resource usage is checked against the configured limits during execution

### 7.2 Retry Logic

#### 7.2.1 Retry on Tool Invocation Failure
- Triggered when the tool returns an error with `retryable: true`
- Max retries: 3 (configurable per tool, default)
- Backoff: exponential with jitter (base 500ms, max 30s)
- The results cache is bypassed for retries
- If the error is `TOOL_TIMEOUT`, the execution backend is reconfigured with the same timeout
- If the error is `TOOL_INTERNAL_ERROR`, a different execution backend is selected if available (e.g., a different HTTP endpoint)

#### 7.2.2 Retry on Secret Resolution Failure
- Triggered when: the Secrets Vault returns an error or the secret is missing
- Max retries: 3 with 1-second fixed interval
- If the secret is still missing after 3 retries, the invocation fails with `SECRET_MISSING`

#### 7.2.3 Retry on Resource Quota Exceeded
- Triggered when: the concurrent invocation limit is reached
- Behavior: the request is queued and re-attempted when a slot becomes available
- No separate retry count (it is managed by the queue)

### 7.3 Error Recovery

#### 7.3.1 Execution Backend Failure
If an execution backend crashes (e.g., the subprocess executor has a bug), the Tool Manager fails the invocation and marks the backend as degraded. A background thread restarts the backend. Invocations that would use a degraded backend are queued until it recovers.

#### 7.3.2 Database Failure
If the Database System is unavailable, the Tool Manager continues operating with the in-memory registry snapshot. Registration, deprecation, and unregistration operations that require database persistence are queued and replayed when the database recovers.

#### 7.3.3 Secrets Vault Failure
If the Secrets Vault is unavailable, tools that require secrets cannot be invoked. Tools that do not require secrets continue to work normally. An alert is triggered.

#### 7.3.4 Graceful Shutdown
1. Stop accepting new invocation requests
2. Wait for all in-flight invocations to complete (configurable timeout, default 30 seconds)
3. Cancel any remaining in-flight invocations
4. Flush audit logs to the Logging System
5. Exit

---

## 8. Security, Authentication, Authorization, Permissions

### 8.1 Authentication
- Every invocation request must include a valid authentication token (JWT or API key)
- The Authentication System validates the token and extracts the caller identity (agent_id, user_id, tenant_id)
- The identity is passed to the Authorization component for permission verification

### 8.2 Authorization

Permissions are checked at two levels:

#### 8.2.1 API Access
- `tools.register` — register a new tool definition
- `tools.update` — update an existing tool definition
- `tools.delete` — unregister a tool
- `tools.list` — list available tools
- `tools.view` — view a specific tool's details and schema

#### 8.2.2 Tool Execution
- `tools.execute.<tool_id>` — invoke a specific tool
- `tools.execute.*` — invoke any tool (admin-level)
- `tools.execute.batch` — invoke multiple tools in a single batch call

### 8.3 Permissions Model

- Each tool can optionally define its own required permissions:
```json
{
  "required_permissions": ["tools.execute.weather.current"],
  "denied_agents": ["malicious-agent-testing"],
  "allowed_tenants": ["tenant-123"]
}
```
- If a tool requires permissions, those are checked in addition to the API-level permissions.
- Permission checks use a deny-by-default model: if no explicit allow rule exists, access is denied.
- Permissions are cached for 5 minutes with a background refresh.

### 8.4 Secrets Management

- Tools reference secrets via `${{ secrets.secret_name }}` placeholders
- Secrets are resolved at invocation time by the Secrets Resolver
- Resolved secrets are injected into the execution environment (environment variables, HTTP headers)
- Secrets are NEVER returned in the invocation result
- Secrets are redacted from audit logs
- Secrets are encrypted in transit and at rest

### 8.5 Secure Execution

- **Direct mode**: Only for built-in system tools that are compiled into the Tool Manager binary. These tools must pass a code review audit.
- **Subprocess mode**: Tools run as a separate OS process with lowered privileges (non-root user, restricted capabilities, seccomp filter). The subprocess is assigned to a cgroup for resource limits.
- **Container mode**: Tools run in an ephemeral Docker container with a minimal base image (distroless). Network access is restricted to only the domains specified in the tool definition. The container has a read-only root filesystem and no capabilities.
- **WASM mode**: Tools run in a WebAssembly sandbox (Wasmtime runtime) with no access to the host filesystem or network. WASM tools are the most secure execution mode.
- **Remote mode**: Tools are executed by an external service. The Tool Manager communicates over mTLS. The external service is responsible for its own security.

---

## 9. Monitoring, Logging, Metrics, Tracing

### 9.1 Metrics

| Metric | Type | Labels | Description |
|---|---|---|---|
| `aios_tool_invocations_total` | Counter | `tool_id`, `status`, `agent_id` | Total tool invocations |
| `aios_tool_invocation_duration_ms` | Histogram | `tool_id`, `status` | Tool execution duration |
| `aios_tool_cache_hit_total` | Counter | `tool_id` | Cache hit count |
| `aios_tool_cache_miss_total` | Counter | `tool_id` | Cache miss count |
| `aios_tool_validation_error_total` | Counter | `tool_id`, `error_type` | Validation error count |
| `aios_tool_concurrent_invocations` | Gauge | `tool_id` | Current in-flight invocations |
| `aios_tool_quota_exceeded_total` | Counter | `tool_id`, `quota_type` | Resource quota exceeded |
| `aios_tool_registry_size` | Gauge | - | Number of registered tools |
| `aios_tool_secret_resolve_duration_ms` | Histogram | `tool_id` | Secret resolution time |

### 9.2 Logging

- Structured JSON logging at INFO, WARN, ERROR levels
- Every invocation produces an INFO log entry
- Errors produce ERROR log entries at panic level
- Debug logs include full arguments (with secrets redacted)
- Log shipping: real-time stream to ELK/Loki
- Log retention: 30 days at INFO level, 90 days at ERROR level

### 9.3 Tracing

- OpenTelemetry distributed tracing
- Trace context propagated from the calling agent
- Spans:
  - `tool_manager.handle_invocation` — root span for each invocation
  - `tool_manager.resolve_secrets` — secret resolution span
  - `tool_manager.validate_arguments` — argument validation span
  - `tool_manager.execute_tool` — tool execution span (includes execution mode attribute)
  - `tool_manager.cache_lookup` — cache lookup span
- Sampling: 10% of successful invocations, 100% of failed invocations
- Trace export via OTLP to the central tracing backend

---

## 10. Scalability, Performance, Optimization

### 10.1 Scalability

#### 10.1.1 Horizontal Scaling
- The Tool Manager is stateless (registry state is in memory but can be rebuilt from the database)
- Multiple instances behind a gRPC load balancer
- Redis serves as the shared cache and shared counter for concurrent invocation limits
- Tool execution is isolated per instance (no shared execution state)

#### 10.1.2 Scaling Limits
- **Maximum instances**: 32
- **Maximum registered tools**: 10,000
- **Maximum concurrent invocations per instance**: 500
- **Maximum invocations per second per instance**: 1,000

### 10.2 Performance Targets

| Metric | Target | Warning |
|---|---|---|
| Tool lookup P99 | < 1ms | > 5ms |
| Argument validation P99 | < 5ms | > 20ms |
| Secret resolution P99 | < 10ms | > 50ms |
| Total overhead (excluding tool execution) | < 20ms | > 100ms |
| Cache hit ratio (cached tools) | > 60% | < 30% |

### 10.3 Optimization

- **Schema Compilation**: JSON schemas are compiled to fast in-house evaluators on tool registration (not on each invocation)
- **Connection Pooling**: HTTP/gRPC connections to remote tools are pooled with keep-alive
- **Zero-Copy Argument Validation**: Arguments are validated against compiled schemas without allocation
- **Parallel Secret Resolution**: If a tool uses multiple secrets, they are resolved concurrently
- **Prefetch Cache**: For tools with predictable invocation patterns (e.g., recurring agent tasks), results can be prefetched into the cache

---

## 11. Testing Strategy, Risk Analysis

### 11.1 Testing Strategy

#### 11.1.1 Unit Tests
- Tool registry CRUD operations
- Argument schema validation with valid and invalid inputs
- Argument sanitization (injection attempts, control characters)
- Resource quota enforcer with various limits
- Cache key generation and lookup
- State machine transitions (all tool states)

#### 11.1.2 Integration Tests
- End-to-end tool registration + invocation + result retrieval
- Tool dependency check (dependency not registered, dependency disabled)
- Secret resolution with a mock Secrets Vault
- Cache hit and miss behavior
- Batch invocation ordering and isolation

#### 11.1.3 Security Tests
- Attempt to register a tool with an executable endpoint
- Attempt to invoke a tool without proper authorization
- Attempt to invoke a disabled tool
- Attempt to inject shell commands via tool arguments
- Attempt to read secrets through tool error messages

#### 11.1.4 Performance Tests
- Benchmark with 1,000 concurrent invocations
- Benchmark schema validation throughput
- Benchmark cache hit/miss under load

### 11.2 Risk Analysis

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Malicious tool executes arbitrary code | Low | Critical | WASM sandbox for untrusted tools, code signing, permission model |
| Tool leaks secrets through error message | Medium | High | Secrets redaction in all outputs, output schema validation
| Tool timeout leads to resource leak | Medium | Medium | Hard timeout enforced by OS, cgroup cleanup on timeout |
| Cache poisoning via crafted arguments | Low | Medium | Cache key includes SHA256 of arguments, output schema validation |
| Tool dependency chain causes deadlock | Low | Low | Dependency graph cycle detection at registration |
| Rate limit exhaustion on external API | Medium | Medium | Per-tool rate limiter, circuit breaker per endpoint |

---

## 12. Failure Scenarios, Recovery Procedures

### 12.1 Tool Execution Timeout
**Failure**: A tool takes longer than its configured timeout.
**Detection**: The dispatcher's timeout timer fires.
**Response**: The tool process/container is killed. A `TOOL_TIMEOUT` error is returned. The timeout count for this tool is incremented in the circuit breaker. If the timeout count exceeds a threshold, the tool is marked DEGRADED.

### 12.2 External API Downtime
**Failure**: The remote HTTP endpoint a tool depends on returns 5xx.
**Detection**: The dispatcher receives a non-2xx status code.
**Response**: The invocation is retried up to 3 times with backoff. If all retries fail, a `TOOL_EXTERNAL_ERROR` is returned. The endpoint is removed from the routing pool for 60 seconds.

### 12.3 Secret Vault Unavailability
**Failure**: The Secrets Vault is unreachable.
**Detection**: The Secrets Resolver receives a connection timeout.
**Response**: Tools without secret dependencies continue normally. Tools with secret dependencies are queued. After 30 seconds, queued invocations are failed with `SECRET_VAULT_UNAVAILABLE`.

### 12.4 Tool Registry Corruption
**Failure**: An invalid tool definition is registered (malformed schema, non-existent execution mode).
**Detection**: Schema compilation fails or validation throws an error.
**Response**: The tool registration is rejected with a structured error. The tool is not added to the registry.

---

## 13. Operational Limits, Maintenance

### 13.1 Operational Limits

| Parameter | Limit | Behavior When Exceeded |
|---|---|---|
| Max tools | 10,000 | Registration rejected |
| Max concurrent invocations per instance | 500 | Queue invocations |
| Max concurrent invocations per agent | 10 | Error: `TOOL_CONCURRENCY_EXCEEDED` |
| Max arguments size | 1 MB | Error: `ARGUMENTS_TOO_LARGE` |
| Max result size | 10 MB | Result truncated with `RESULT_TRUNCATED` warning |
| Max cache size per tool | 100 MB | Oldest entry evicted (LRU) |
| Max execution timeout | 300 seconds | Tool killed at timeout |
| Max memory per execution | 4096 MB | Tool killed at limit |

### 13.2 Maintenance

#### 13.2.1 Zero-Downtime Updates
- Rolling updates via Kubernetes Deployment
- Health check: `/health/ready` returns 200 only after the registry is loaded
- Drain timeout: 30 seconds for in-flight invocations

#### 13.2.2 Tool Deprecation Strategy
- Tools are deprecated at least 30 days before being disabled
- Deprecated tools return a warning header with the deprecation date
- Callers are notified of deprecation via events and the deprecation API

#### 13.2.3 Configuration Hot-Reload
- Tool definitions can be updated without restarting the service
- On update, the schema cache for the tool is invalidated
- The registry snapshot is rebuilt

---

## 14. Future Improvements, Examples

### 14.1 Future Improvements

- **Tool Chaining**: Define tools that are composed of multiple sub-tools, with automatic result passing.
- **Dynamic Tool Discovery**: Automatic tool discovery from git repositories, package registries, or cloud service catalogs.
- **Federated Tool Manager**: Cross-cluster tool sharing with latency-aware routing.
- **Tool Test Harness**: A sandboxed environment for tool developers to test their tools before deployment.
- **A/B Testing**: Run two versions of the same tool and compare results for quality assurance.
- **Rate Limit Prediction**: Predict when a tool's rate limit will be reached and preemptively throttle.

### 14.2 Examples

#### Example 1: File Read Tool
```
Tool: system.file.read@1.0.0
Input: {path: "/app/config.json", encoding: "utf-8"}
Output: {content: "{\"key\": \"value\"}", size: 15}
Execution: Subprocess with read-only filesystem access to /app
```

#### Example 2: Database Query Tool
```
Tool: database.query@2.1.0
Input: {query: "SELECT * FROM users WHERE id = ?", params: ["42"]}
Output: {rows: [{id: 42, name: "Alice"}], row_count: 1, duration_ms: 5}
Execution: Direct function call to Database Manager (internal)
```

#### Example 3: Weather Tool with Caching
```
Request 1: weather.current @1.0.0 {city: "London", units: "celsius"}
→ HTTP call to weather API, duration: 1200ms, result cached for 300s

Request 2 (10s later): weather.current @1.0.0 {city: "London", units: "celsius"}
→ Cache hit, duration: 3ms, result: same as above
```

#### Example 4: Tool with Dependency
```
Tool: deploy.application@1.0.0
Depends on: container.build@1.0.0, kubernetes.apply@1.0.0
On invocation: first calls container.build to build the image, then passes the image tag to kubernetes.apply to deploy
```

---

## 15. Acceptance Criteria, Definition of Done

### 15.1 Acceptance Criteria

1. **Tool Registration**: A tool definition with valid schema and endpoint can be registered and immediately available for invocation. Verified by E2E test.
2. **Tool Invocation**: A registered ACTIVE tool can be invoked with valid arguments and returns the expected result. Verified by E2E test.
3. **Argument Validation**: An invocation with invalid arguments returns a structured validation error with field-level details. Verified by unit test.
4. **Resource Limits**: A tool that exceeds its configured resource limits is terminated with the appropriate error. Verified by integration test.
5. **Caching**: A cacheable tool returns cached results on repeated invocation within the TTL. Verified by integration test.
6. **Authorization**: An agent without the `tools.execute.<tool_id>` permission receives an authorization error. Verified by security test.
7. **Secrets**: A tool that uses secrets invokes successfully with resolved secrets, and the secrets do not appear in logs or results. Verified by security test.
8. **Dependency Resolution**: A tool with unmet dependencies fails registration with a clear error message. Verified by unit test.
9. **Graceful Degradation**: When the Secrets Vault is down, tools without secrets continue to work. Verified by chaos test.
10. **Performance**: Invocation routing overhead (excluding execution) is under 20ms P99. Verified by performance test.

### 15.2 Definition of Done

- [ ] All acceptance criteria implemented and passing
- [ ] Unit test coverage > 90% (new code)
- [ ] Integration tests covering happy path, error paths, edge cases
- [ ] Performance benchmarks show no regression
- [ ] Security review completed
- [ ] Metrics and logging added for all new features
- [ ] Documentation updated
- [ ] Code reviewed by at least one peer
- [ ] Deployed to staging and running for 24 hours without issues
- [ ] Rollback plan documented and tested

## 16. Appendix: Built-In Tools Reference

### 16.1 System File Tools
- system.file.read@1.0.0: Read file contents from allowed paths
- system.file.write@1.0.0: Write content to allowed paths
- system.file.list@1.0.0: List files in a directory
- system.file.delete@1.0.0: Delete a file from allowed paths

### 16.2 System Network Tools
- system.http.get@1.0.0: Make an HTTP GET request
- system.http.post@1.0.0: Make an HTTP POST request
- system.http.put@1.0.0: Make an HTTP PUT request
- system.http.delete@1.0.0: Make an HTTP DELETE request

### 16.3 System Data Tools
- system.json.parse@1.0.0: Parse a JSON string into a structured object
- system.json.stringify@1.0.0: Convert an object to a JSON string
- system.base64.encode@1.0.0: Encode a string to base64
- system.base64.decode@1.0.0: Decode a base64 string
- system.hash.sha256@1.0.0: Compute SHA-256 hash of input
- system.hash.md5@1.0.0: Compute MD5 hash of input

### 16.4 System Math Tools
- system.math.evaluate@1.0.0: Evaluate a mathematical expression
- system.math.statistics@1.0.0: Compute statistics (mean, median, stddev) on an array

## 17. Appendix: Error Codes Reference

| Code | HTTP Status | Description | Retryable |
|---|---|---|---|
| TOOL_NOT_FOUND | 404 | The requested tool ID does not exist in the registry | No |
| TOOL_DISABLED | 403 | The tool is currently disabled | No |
| TOOL_DEPRECATED | 200 (warning) | The tool is deprecated | No |
| TOOL_VALIDATION_ERROR | 400 | Tool arguments failed schema validation | No |
| TOOL_TIMEOUT | 504 | Tool execution exceeded the configured timeout | Yes |
| TOOL_RESOURCE_EXCEEDED | 429 | Tool exceeded resource quota (CPU, memory, concurrency) | Yes (after backoff) |
| TOOL_INTERNAL_ERROR | 500 | An internal error occurred during tool execution | Yes |
| TOOL_EXTERNAL_ERROR | 502 | The external service the tool depends on returned an error | Yes |
| TOOL_SECRET_MISSING | 500 | A required secret was not found in the vault | No |
| TOOL_DEPENDENCY_FAILED | 400 | A tool dependency is not available | No |
| TOOL_CANCELLED | 499 | The tool execution was cancelled | Yes |
| TOOL_PAYLOAD_TOO_LARGE | 413 | The tool arguments or result exceed size limits | No |

## 18. Appendix: Configuration Reference

| Key | Type | Default | Description |
|---|---|---|---|
| tool_manager.max_tools | int | 10000 | Maximum number of registered tools |
| tool_manager.max_concurrent_per_agent | int | 10 | Max concurrent tool invocations per agent |
| tool_manager.execution.default_timeout_ms | int | 60000 | Default timeout for tool execution |
| tool_manager.execution.default_cpu_ms | int | 30000 | Default CPU time limit per invocation |
| tool_manager.execution.default_memory_mb | int | 256 | Default memory limit per invocation |
| tool_manager.execution.max_timeout_ms | int | 300000 | Maximum allowable timeout |
| tool_manager.execution.max_memory_mb | int | 4096 | Maximum allowable memory |
| tool_manager.cache.default_ttl_seconds | int | 60 | Default cache TTL for cacheable tools |
| tool_manager.cache.max_ttl_seconds | int | 86400 | Maximum cache TTL |
| tool_manager.cache.max_entries | int | 10000 | Maximum cache entries |
| tool_manager.retry.max_attempts | int | 3 | Maximum retry attempts |
| tool_manager.retry.base_delay_ms | int | 500 | Base delay for exponential backoff |
| tool_manager.retry.max_delay_ms | int | 30000 | Maximum delay for backoff |
| tool_manager.audit.log_all | bool | true | Log all invocations (set false for high-volume tools) |
| tool_manager.secrets.refresh_interval_ms | int | 300000 | Secrets refresh interval |

## 19. Appendix: Deployment Architecture

The Tool Manager is deployed as a Kubernetes Deployment:
- Replicas: 2 minimum, 16 maximum
- Resource requests: 0.5 CPU, 512MB RAM
- Resource limits: 2 CPU, 2GB RAM
- Readiness probe: GET /health/ready (initial delay: 5s, period: 10s)
- Liveness probe: GET /health/live (initial delay: 10s, period: 15s)
- Service: ClusterIP (gRPC port 50051, HTTP port 8080)
- PersistentVolumeClaim: For subprocess/container execution temp directories (10GB)
- ConfigMap: Tool definitions (mounted)
- Secrets: Secret references mapping (mounted)

## 20. Appendix: Example Tool Definitions

### 20.1 Weather Tool (Remote HTTP)
```json
{
  "tool_name": "weather.current",
  "version": "1.0.0",
  "description": "Get current weather for a city",
  "input_schema": {
    "type": "object",
    "properties": {
      "city": {"type": "string", "minLength": 1, "maxLength": 100}
    },
    "required": ["city"]
  },
  "output_schema": {
    "type": "object",
    "properties": {
      "temperature": {"type": "number"},
      "conditions": {"type": "string"}
    }
  },
  "execution_mode": "REMOTE_HTTP",
  "execution_endpoint": "https://api.weather.com/current?city={{city}}",
  "auth": {"type": "api_key", "header": "X-API-Key", "secret_ref": "secrets.weather_api_key"},
  "resource_limits": {"timeout_ms": 10000, "memory_mb": 64},
  "cache": {"enabled": true, "ttl_seconds": 300, "key_template": "weather:{city}"}
}
```

### 20.2 File Read Tool (Subprocess)
```json
{
  "tool_name": "system.file.read",
  "version": "1.0.0",
  "description": "Read a file from the filesystem",
  "input_schema": {
    "type": "object",
    "properties": {
      "path": {"type": "string", "pattern": "^[a-zA-Z0-9_./-]+$"}
    },
    "required": ["path"]
  },
  "execution_mode": "SUBPROCESS",
  "execution_endpoint": "/usr/local/bin/safe-read-file",
  "resource_limits": {"timeout_ms": 5000, "memory_mb": 128, "network_allowed": false}
}
```

### 20.3 Database Query Tool (Direct)
```json
{
  "tool_name": "database.query",
  "version": "2.1.0",
  "description": "Execute a parameterized SQL query",
  "input_schema": {
    "type": "object",
    "properties": {
      "query": {"type": "string", "maxLength": 10000},
      "params": {"type": "array"}
    },
    "required": ["query"]
  },
  "execution_mode": "DIRECT",
  "execution_endpoint": "aios.database_manager.Query",
  "resource_limits": {"timeout_ms": 30000, "memory_mb": 256}
}
```


## 21. Appendix: Tool Execution Backend Specifications

### 21.1 Direct Function Execution
- Implementation: In-process Go function call
- Security: Tool function must be compiled into the Tool Manager binary
- Isolation: None (runs in the same process)
- Performance: Sub-microsecond invocation overhead
- Resource limits: Only language-level goroutine limits
- Use case: Built-in system tools only (database.query, file operations)
- Code review: Every direct function must pass security audit before merge

### 21.2 Subprocess Execution
- Implementation: OS process fork with piped stdin/stdout/stderr
- Security: Runs as non-root user (uid=1001, gid=1001) with dropped capabilities
- Isolation: Seccomp BPF filter (allow only read, write, exit_group, clock_gettime), no network unless explicitly allowed
- Performance: ~5ms process startup overhead
- Resource limits: rlimit_cpu, rlimit_as, rlimit_nofile, rlimit_nproc
- Timeout enforcement: SIGKILL after timeout + 1 second grace
- Working directory: Ephemeral temp directory cleaned up after execution
- Use case: System tools that need file system access

### 21.3 Container Execution
- Implementation: Ephemeral Docker container (run and remove)
- Image: Distroless base image (gcr.io/distroless/base) with tool-specific binaries
- Security: Dropped all capabilities, read-only root filesystem, no-new-privileges
- Isolation: cgroup v2 for CPU/memory limits, no network unless configured
- Performance: ~100ms container startup overhead
- Resource limits: CPU shares, memory limit in MB, OOM killer enabled
- Network: Allow only domains specified in tool definition (iptables egress filter)
- Use case: Tools that require dependencies not available in the base system

### 21.4 WASM Sandbox Execution
- Implementation: Wasmtime runtime
- Security: No access to host filesystem, network, or environment variables
- Isolation: WASM linear memory, no syscalls
- Performance: ~100us invocation overhead
- Resource limits: WASM memory limit (default 128MB), fuel metering for CPU
- Fuel: 1 fuel unit per WASM instruction; default budget: 1,000,000 fuel
- Use case: Plugin-provided tools, third-party tools, untrusted code

### 21.5 Remote HTTP Execution
- Implementation: HTTP/HTTPS client with connection pooling
- Security: mTLS or API key authentication
- Isolation: Provided by the remote service
- Performance: Network-bound (typically 10-500ms)
- Resource limits: Timeout only (no CPU/memory control)
- Retry: 2 retries with exponential backoff
- Use case: Tools that wrap external APIs

### 21.6 Remote gRPC Execution
- Implementation: gRPC client with connection pooling
- Security: mTLS required
- Isolation: Provided by the remote service
- Performance: Network-bound (typically 5-100ms)
- Resource limits: Timeout only (no CPU/memory control)
- Streaming: Supports server-streaming, client-streaming, and bidirectional streaming
- Use case: Tools that are implemented as AIOS microservices

## 22. Appendix: Resource Limiting Implementation Details

### 22.1 CPU Time Limiting (Subprocess)
On Linux, CPU time is limited using setrlimit with RLIMIT_CPU. When the limit is exceeded, the process receives SIGXCPU followed by SIGKILL after 1 second. On macOS, RLIMIT_CPU is also supported but less precise. For cross-platform compatibility, the Tool Manager also implements watchdog goroutines that monitor wall-clock time and forcefully terminate subprocesses.

### 22.2 Memory Limiting (Subprocess)
On Linux, virtual memory is limited with RLIMIT_AS (address space limit). The limit is set to the configured memory limit plus a 10% overhead allowance. The Tool Manager also monitors RSS memory from /proc/PID/status every 100ms during execution and sends SIGTERM if the RSS approaches the limit.

### 22.3 CPU/Memory Limiting (WASM)
WASM execution is limited through Wasmtime's fuel metering mechanism. Each instruction consumes 1 unit of fuel. The total fuel budget is calculated as:
```
fuel_budget = (cpu_limit_ms * 1_000_000) / expected_instruction_time_ns
```
Default: cpu_limit_ms = 30000, expected_instruction_time_ns = 1ns -> fuel_budget = 30,000,000,000,000. Memory is limited by the WASM linear memory maximum size, set to the tool's configured memory limit.

### 22.4 Network Access Control
Network access is enforced at the OS level:
- Subprocess: iptables/nftables rules applied to the process's network namespace
- Container: Docker's --network option with custom iptables rules
- WASM: No network access by default (WASI sockets are not implemented)
- Remote: Network access is inherent to the execution mode

## 23. Appendix: Security Audit Checklist

Before a tool can be promoted from REGISTERED to ACTIVE, the following security checks must pass:

- [ ] Input schema does not accept shell metacharacters (;, |, &&, $(), backtick)
- [ ] Secrets referenced in tool definition correspond to existing Secrets Vault entries
- [ ] If execution_mode is SUBPROCESS, the binary path is allow-listed
- [ ] If execution_mode is DIRECT, the function has passed a code security review
- [ ] If execution_mode is CONTAINER, the image is from an approved registry
- [ ] If execution_mode is WASM, the WASM module has been scanned for malware
- [ ] If execution_mode is REMOTE_HTTP, the endpoint uses HTTPS with a valid certificate
- [ ] Resource limits are reasonable (timeout < 300s, memory < 4096MB)
- [ ] Tool description does not contain misleading or harmful information
- [ ] Output schema does not allow leaking sensitive data (passwords, tokens)
- [ ] No hard-coded credentials exist in the tool definition
- [ ] Caching is safe: tool outputs are not sensitive and are suitable for caching
- [ ] Dependency graph has no cycles
- [ ] Tool owner is a valid AIOS user or system account

## 24. Appendix: Performance Benchmarking Results

Typical performance characteristics under load (16 vCPU, 32GB RAM instance):

| Tool Count | Concurrent Invocations | Avg Latency | P99 Latency | Throughput |
|---|---|---|---|---|
| 100 | 100 | 2ms | 8ms | 5,000 req/s |
| 100 | 500 | 3ms | 15ms | 8,000 req/s |
| 1,000 | 100 | 3ms | 10ms | 4,500 req/s |
| 1,000 | 500 | 5ms | 25ms | 6,000 req/s |
| 10,000 | 100 | 5ms | 20ms | 3,000 req/s |
| 10,000 | 500 | 12ms | 50ms | 4,000 req/s |

Cache hit rate for common tools (weather, file read, database query):
- With 60s TTL: ~45% hit rate
- With 300s TTL: ~72% hit rate
- With 3600s TTL: ~91% hit rate

Schema validation throughput:
- Simple schema (5 fields): ~100,000 validations/second
- Complex schema (50 fields, nested, $ref): ~15,000 validations/second

## 25. Appendix: Glossary

| Term | Definition |
|---|---|
| Tool | An executable capability registered in the Tool Manager that agents can invoke |
| Tool Registry | The central in-memory and persistent store of all tool definitions |
| Execution Backend | The mechanism by which a tool is executed (Direct, Subprocess, Container, WASM, Remote) |
| Resource Quota | Limits on CPU time, memory, execution duration, and network access for tool invocations |
| Schema Validation | Validation of tool input arguments and output results against JSON Schema definitions |
| Secrets Resolver | Component that fetches secrets from the Secrets Vault and injects them into tool execution |
| Result Cache | A Redis-backed cache for tool results to reduce redundant invocations |
| Tool Lifecycle | The state machine governing tool states: REGISTERED, VALIDATED, ACTIVE, DEPRECATED, DISABLED |
| Execution Sandbox | An isolated environment for tool execution (subprocess, container, or WASM) |
| Tool Dependency | A relationship where one tool requires another tool to be ACTIVE before it can be invoked |
| Audit Log | An immutable record of every tool invocation with full provenance |
| Agent | The AI entity that requests tool invocations on behalf of a user or workflow |
| Invocation | A single call to a tool with specific arguments, producing a result or error |
| Rate Limiter | Component that limits the number of tool invocations per agent, tool, or tenant |
