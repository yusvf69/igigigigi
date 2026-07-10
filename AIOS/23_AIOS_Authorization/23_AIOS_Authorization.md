# AIOS Authorization System — Engineering Specification
## Document 23 of 30 — Enterprise-Grade Policy-Based Authorization Architecture

---

## 1. Purpose, Mission, Responsibilities, Non-Responsibilities

### 1.1 Purpose
The AIOS Authorization System provides a unified, policy-driven authorization framework that governs every interaction within the AIOS platform. It ensures that every operation is explicitly authorized based on defined policies, supporting multiple authorization models including RBAC, ABAC, and ReBAC through a policy-as-code paradigm.

### 1.2 Mission
To provide consistent, low-latency, and auditable authorization decisions for every action performed within the AIOS platform, ensuring that only authorized entities can perform authorized operations on authorized resources, at all times.

### 1.3 Responsibilities
- Operate as the central Policy Decision Point (PDP) for all authorization decisions
- Manage policy definitions using Rego language (Open Policy Agent) as policy-as-code
- Support multiple authorization models: RBAC, ABAC, ReBAC
- Deploy Policy Enforcement Points (PEP) at every entry point: API gateway, service mesh, kernel
- Enforce authorization at every service call and every API request
- Manage the resource hierarchy: system -> department -> team -> agent
- Cache authorization decisions for performance
- Manage policy versions, testing, and rollout
- Handle authorization failures with proper audit and error responses
- Support dynamic policy updates without service restart
- Integrate with the Authentication System (Document 22) for identity verification
- Support policy evaluation for both synchronous and asynchronous operations

### 1.4 Non-Responsibilities
- Authentication and identity verification (handled by Authentication System, Document 22)
- Permission definitions and permission management (handled by Permission System, Document 24)
- User and agent lifecycle management (handled by Identity Management)
- Application-level authorization logic (should use the central PDP)
- Physical or network access control (handled by Security System, Document 21)
- Policy definition for external systems (only AIOS-internal policies)

---

## 2. Architecture, Internal Components, External Components

### 2.1 Overall Architecture

The Authorization System follows a PDP/PEP architecture with clear separation of concerns between policy decision and policy enforcement:

```
                         ┌──────────────────┐
                         │   Policy Store    │
                         │  (Git + OPA)     │
                         └────────┬─────────┘
                                  │
┌──────────┐   ┌──────────┐   ┌──┴───────────┐   ┌──────────┐
│  Client  │──▶│    PEP   │──▶│     PDP       │   │  Cache   │
│  (Agent, │   │  (Envoy, │   │  (OPA Server) │──▶│ (Redis)  │
│   User)  │   │   SDK)   │   │               │   └──────────┘
└──────────┘   └──────────┘   └───────────────┘
```

### 2.2 Internal Components

#### 2.2.1 Policy Decision Point (PDP)
The PDP is the core authorization engine that evaluates policies and makes decisions.

Key capabilities:
- Policy evaluation using Open Policy Agent (OPA) with Rego language
- REST and gRPC APIs for authorization queries
- Batch authorization for bulk decision-making
- Partial evaluation for pre-computed policies
- Decision caching with configurable TTL
- Structured decision responses including reasons and OPA traces
- Policy module management (load, unload, update)
- Built-in functions for common AIOS authorization patterns
- Connection pooling for high-throughput scenarios

Implementation details:
- Technology: OPA (Open Policy Agent) as embedded library or sidecar
- Language: Go (OPA native) for PDP service
- Scaling: Stateless, horizontally scalable
- Throughput: 10,000+ decisions/second per instance
- Latency: p50 < 2ms, p95 < 8ms, p99 < 20ms

#### 2.2.2 Policy Store
Version-controlled repository for all authorization policies.

Key capabilities:
- Git-backed storage for policy version control
- Policy namespacing for multi-tenant isolation
- Policy module organization by domain
- Schema validation for policy inputs
- Policy compilation and optimization
- Bundle distribution to PDP instances
- Dry-run evaluation support
- Policy testing framework integration
- Rollback to previous policy versions
- Policy metadata management (author, date, description)

Storage:
- Source of truth: Git repository
- Compiled policies: OPA bundle server (HTTP)
- Backup: Git repository backups
- Access: Write via CI/CD pipeline, read via OPA bundle API

#### 2.2.3 Policy Enforcement Points (PEP)
PEPs intercept requests and enforce PDP decisions at various points in the architecture.

Types of PEPs:
1. API Gateway PEP: Envoy external authorization filter
   - Intercepts all external API requests
   - Calls PDP for each request
   - Returns 403 if denied, forwards if allowed
   - Caches decisions locally with short TTL

2. Service Mesh PEP: Istio/Envoy sidecar
   - Intercepts all service-to-service traffic
   - Evaluates authorization policies at mesh level
   - Supports L4 and L7 authorization
   - mTLS integration for identity verification

3. SDK PEP: Client libraries for services
   - Embedded authorization for service code
   - Local policy evaluation for latency-sensitive paths
   - Fallback to remote PDP for complex decisions
   - Automatic retry and circuit breaking

4. Kernel PEP: eBPF-based enforcement
   - System-level authorization for kernel operations
   - File access, network socket operations
   - Process execution authorization
   - Container security enforcement

#### 2.2.4 Policy Compiler
Transforms high-level policies into optimized evaluation plans.

Responsibilities:
- Parse Rego policy files
- Compile policies into optimized bytecode
- Partial evaluation for static attributes
- Policy optimization (rule reordering, indexing)
- Schema validation of policy inputs
- Generate policy bundles for distribution
- Policy analysis (dead code detection, conflict detection)
- Performance benchmarking of compiled policies

#### 2.2.5 Authorization Cache
Distributed cache for authorization decisions to reduce PDP load.

Implementation:
- Cache store: Redis Cluster
- Cache key: hash(subject + action + resource + context)
- Cache value: decision + reason + TTL
- Cache TTL: configurable per policy (default 5 minutes)
- Negative caching: denied decisions cached for 1 minute
- Eviction: LRU with per-policy priority
- Invalidation: cache invalidation on policy change
- Size limit: 10GB per cluster

#### 2.2.6 Policy Test Framework
Automated testing for policy changes before deployment.

Capabilities:
- Unit testing: test individual rules with mock inputs
- Integration testing: test complete policies with realistic scenarios
- Coverage analysis: identify untested policy paths
- Regression testing: compare results against baseline
- Performance testing: measure evaluation time
- Conflict detection: identify contradictory rules
- Fuzz testing: test with random inputs
- Compliance testing: verify policy meets requirements

#### 2.2.7 Audit Logger for Authorization
Records all authorization decisions for compliance and debugging.

Features:
- Structured logging of all decisions
- Both allowed and denied decisions logged
- Trace ID correlation for distributed tracing
- Policy version at time of decision
- Decision reason and matched rules
- OPA trace output (when enabled)
- Real-time streaming to security event pipeline
- Tamper-evident log entries

### 2.3 External Components

#### 2.3.1 Open Policy Agent (OPA)
- Policy evaluation engine
- Rego language runtime
- Bundle distribution mechanism
- REST/gRPC API server
- Status API for monitoring

#### 2.3.2 Envoy Proxy
- External authorization filter (ext_authz)
- Rate limit service integration
- mTLS termination
- Access logging

#### 2.3.3 Istio Service Mesh
- AuthorizationPolicy CRD
- PeerAuthentication for mTLS
- RequestAuthentication for JWT validation
- Telemetry integration

#### 2.3.4 Redis Cluster
- Authorization decision cache
- Rate limit counter storage
- Session cache for authorization metadata

#### 2.3.5 Git Repository
- Policy source of truth
- Version history
- Code review platform integration
- CI/CD pipeline triggers

#### 2.3.6 CI/CD Platform
- Policy testing on pull requests
- Policy deployment to staging
- Canary policy rollout
- Automatic rollback on failure

---

## 3. Dependencies, Inputs, Outputs

### 3.1 Dependencies

| Dependency | Type | Version | Criticality | Purpose |
|---|---|---|---|---|
| Open Policy Agent | Runtime | 0.55+ | Critical | Policy evaluation engine |
| Envoy Proxy | Proxy | 1.28+ | High | API gateway PEP |
| Istio | Mesh | 1.20+ | Medium | Service mesh PEP |
| Redis Cluster | Cache | 7.0+ | High | Decision cache |
| Git | Storage | 2.40+ | High | Policy version control |
| Prometheus | Monitoring | 2.45+ | Medium | Metrics |
| Kafka | Events | 3.5+ | Medium | Audit event streaming |
| Vault | Secrets | 1.15+ | Medium | Policy signing keys |

### 3.2 Inputs

- Authorization requests containing:
  - Subject: identity information (ID, type, roles, attributes)
  - Action: operation being performed (create, read, update, delete, execute, admin)
  - Resource: target resource type, path, attributes
  - Context: time, location, request method, source IP, risk score
- Policy definitions written in Rego
- Policy bundle updates from policy store
- Resource hierarchy definitions
- Role definitions and assignments
- Attribute definitions for ABAC policies
- Relationship definitions for ReBAC policies
- Policy test cases and expected results
- Cache invalidation events

### 3.3 Outputs

- Authorization decisions:
  - Allow: operation permitted
  - Deny: operation denied with reason
  - Indeterminate: policy evaluation error
  - NotApplicable: no matching policy
- Decision metadata:
  - Matched rules
  - Policy version
  - Evaluation time
  - OPA trace (debug mode)
- Audit log entries for all decisions
- Authorization metrics:
  - Decision count by result
  - Evaluation latency
  - Cache hit/miss rate
- Policy status information
- Cache entries for repeated decisions

---

## 4. Data Structures, Execution Flow, State Management

### 4.1 Key Data Structures

#### 4.1.1 Authorization Request
```json
{
  "apiVersion": "authz.aios.local/v1",
  "kind": "AuthorizationRequest",
  "requestId": "uuid-v4",
  "timestamp": "RFC3339Nano",
  "subject": {
    "identity_id": "uuid",
    "identity_type": "agent|service|user|plugin",
    "identity_name": "string",
    "roles": ["role1", "role2"],
    "attributes": {
      "department": "engineering",
      "clearance_level": 3,
      "agent_type": "content_generation",
      "team": "content-team"
    },
    "authentication": {
      "method": "mTLS|jwt|api_key|oauth2",
      "mfa_verified": true,
      "auth_time": "RFC3339Nano"
    }
  },
  "action": {
    "operation": "create|read|update|delete|execute|admin",
    "verb": "POST|GET|PUT|DELETE|PATCH",
    "is_write": true,
    "is_destructive": false
  },
  "resource": {
    "type": "model|memory|tool|api|queue|workflow|data|config",
    "id": "resource-uuid",
    "name": "resource-name",
    "path": "/aios/v1/models/generation/gpt-4",
    "namespace": "production",
    "attributes": {
      "classification": "internal|restricted|critical",
      "owner": "team-uuid",
      "department": "engineering"
    },
    "parent": {
      "type": "project",
      "id": "project-uuid"
    }
  },
  "context": {
    "time": "RFC3339Nano",
    "day_of_week": "Monday",
    "hour": 14,
    "source_ip": "10.0.1.50",
    "network_zone": "internal|restricted|critical",
    "request_method": "POST",
    "user_agent": "AIOS-Agent/1.0",
    "geolocation": {
      "country": "US",
      "region": "us-east-1"
    },
    "risk_score": 0.1,
    "threat_level": "normal|elevated|high|critical"
  }
}
```

#### 4.1.2 Authorization Decision
```json
{
  "apiVersion": "authz.aios.local/v1",
  "kind": "AuthorizationDecision",
  "requestId": "uuid-v4",
  "decisionId": "uuid-v4",
  "timestamp": "RFC3339Nano",
  "decision": "allow|deny|indeterminate|not_applicable",
  "reason": "Policy matched: content-gen-can-call-gen-models",
  "matchedPolicies": [
    {
      "id": "policy-uuid",
      "name": "content-generation-agent-policy",
      "version": "1.2.3",
      "rule": "allow_content_gen_models",
      "namespace": "production/agents"
    }
  ],
  "denyReason": "|User does not have required clearance level",
  "evaluationTimeMs": 2.5,
  "opaTrace": "|detailed trace (debug mode only)",
  "ttl": 300,
  "cacheable": true,
  "metadata": {
    "evaluatedBy": "pdp-instance-01",
    "policyBundleVersion": "2024-01-15-1030",
    "schemaVersion": "v1"
  }
}
```

#### 4.1.3 Policy Definition
```rego
package aios.authz.agent_models

# Default deny - all requests denied unless explicitly allowed
default allow := false

# Allow content generation agents to call generation models
allow {
    input.subject.identity_type == "agent"
    input.subject.attributes.agent_type == "content_generation"
    input.resource.type == "model"
    input.resource.attributes.model_type == "generation"
    input.action.operation == "execute"
}

# Allow agents with admin role to call any model
allow {
    "admin" in input.subject.roles
    input.resource.type == "model"
    input.action.operation == "execute"
}

# Deny access to restricted models outside business hours
deny[{"reason": "Restricted models can only be accessed during business hours"}] {
    input.resource.attributes.classification == "restricted"
    input.context.hour < 9
    input.context.hour > 17
}
```

#### 4.1.4 Policy Bundle Manifest
```yaml
apiVersion: authz.aios.local/v1
kind: PolicyBundle
metadata:
  name: production-agent-policies
  version: 2024-01-15-1030
spec:
  policies:
    - name: agent-model-access
      path: policies/agents/model_access.rego
      namespace: production/agents
      enabled: true
    - name: agent-memory-access
      path: policies/agents/memory_access.rego
      namespace: production/agents
      enabled: true
    - name: agent-tool-access
      path: policies/agents/tool_access.rego
      namespace: production/agents
      enabled: false
  data:
    - name: agent-types
      path: data/agents/types.json
    - name: model-classifications
      path: data/models/classifications.json
  discovery:
    name: aios/authz/discovery
  decisionLogs:
    enabled: true
    reportingPeriod: 60
  defaultDecision: deny
  tracing:
    enabled: false
    samplingRate: 0.01
```

### 4.2 Execution Flows

#### 4.2.1 Basic Authorization Flow
```
1. Client sends request to Service
   - Request includes authentication token (JWT, mTLS cert, API key)

2. Policy Enforcement Point (PEP) intercepts request
   - Location: API Gateway (Envoy), Service Mesh sidecar, or SDK
   - PEP extracts identity from request context

3. PEP constructs authorization request
   - Subject: from token/context (identity_id, type, roles, attributes)
   - Action: from request method and path (create, read, etc.)
   - Resource: from request target (path, type, attributes)
   - Context: from request metadata (time, IP, etc.)

4. PEP checks local decision cache
   - If cache hit and TTL valid: use cached decision, skip to step 8
   - If cache miss: continue to step 5

5. PEP sends authorization request to PDP
   - Protocol: gRPC (preferred) or REST
   - Timeout: 100ms
   - Retry: 3 attempts with 50ms backoff

6. PDP evaluates authorization request
   a. Load applicable policies from policy bundle
   b. Execute Rego policy evaluation with request as input
   c. Apply default decision (deny) if no policy matches
   d. Record evaluation metrics
   e. Log decision to audit log

7. PDP returns decision to PEP
   - Decision: allow/deny/indeterminate
   - Reason and matched policies
   - Cache TTL for this decision

8. PEP enforces decision
   - Allow: forward request to target service
   - Deny: return 403 Forbidden with reason
   - Indeterminate: return 500 or fail open/closed based on config

9. PEP caches decision (if cacheable)
   - Store with TTL from PDP
   - Include decision and reason

10. PEP logs enforcement action
    - Decision, reason, timing, identity, resource
    - Send to audit event stream
```

#### 4.2.2 Batch Authorization Flow
```
1. Service needs to authorize multiple operations
   - Example: Agent needs to check permissions for 100 resources

2. Service sends batch authorization request
   - POST /api/v1/authz/batch
   - Array of authorization requests (up to 1000 per batch)
   - Single identity context for all requests

3. PEP/PDP processes batch:
   a. Check cache for each request
   b. For cache misses, evaluate policies in batch
   c. OPA supports bulk evaluation efficiently
   d. Return all decisions in single response

4. Service receives batch response
   - Array of decisions matching input order
   - Each decision with allow/deny and reason

Batch Request:
{
  "subject": { "identity_id": "...", "type": "agent", ... },
  "context": { "time": "...", "source_ip": "...", ... },
  "requests": [
    { "action": { "operation": "read" }, "resource": { "type": "model", "id": "model-1" } },
    { "action": { "operation": "read" }, "resource": { "type": "model", "id": "model-2" } },
    { "action": { "operation": "execute" }, "resource": { "type": "model", "id": "model-3" } }
  ]
}
```

#### 4.2.3 Policy Update Flow
```
1. Policy Author creates/updates policy
   - Writes Rego policy in Git repository
   - Creates pull request with policy changes

2. CI/CD pipeline triggered
   a. Syntax validation: check Rego syntax
   b. Unit tests: run policy test cases
   c. Integration tests: test with real scenarios
   d. Coverage analysis: ensure tests cover new rules
   e. Performance benchmark: measure evaluation time
   f. Conflict detection: check for contradictory rules

3. Code review and approval
   - Security team reviews policy changes
   - Approve or request changes

4. Policy deployment
   a. Merge to main branch
   b. CI/CD compiles and bundles policies
   c. Deploy to staging PDP
   d. Run smoke tests against staging
   e. Deploy to production PDP
   f. Gradually roll out (canary: 10% -> 50% -> 100%)

5. Policy activation
   a. PDP loads new policy bundle
   b. New requests use updated policies
   c. Cache invalidated for affected resources
   d. Monitor for error rate changes
   e. Automatic rollback if error rate increases
```

#### 4.2.4 Authorization Decision Caching Flow
```
1. PDP makes authorization decision
2. PDP checks if decision is cacheable:
   - Policies without time-based conditions: cacheable
   - Policies with time conditions: cacheable with short TTL
   - Policies with dynamic data: not cacheable
3. PDP calculates cache TTL:
   - Default: 5 minutes
   - High-security resources: 1 minute
   - Low-risk operations: 15 minutes
4. PDP returns decision with cache TTL
5. PEP stores decision in cache:
   - Key: hash(subject_id + action + resource_path + context_hash)
   - Value: decision + reason + expiry
6. PEP serves subsequent requests from cache
7. Cache invalidation:
   - On policy update: broadcast invalidation by policy ID
   - On resource attribute change: invalidate by resource path
   - On role change: invalidate by subject_id
   - TTL expiry: automatic invalidation
```

### 4.3 State Management

#### 4.3.1 Policy State
- **Source of Truth**: Git repository
- **Working State**: OPA in-memory policy store
- **Distribution**: OPA bundle server with HTTP API
- **Persistence**: Git history for versioning
- **Consistency**: Strong consistency for policy updates

#### 4.3.2 Decision State
- **Transient**: Individual authorization decisions
- **Cached**: In Redis with TTL
- **Persisted**: Audit log in Elasticsearch
- **Consistency**: Eventual for cache, strong for audit

#### 4.3.3 PDP Instance State
- **Stateless**: PDP instances are stateless
- **Shared State**: Policy bundles in OPA, decisions in cache
- **Scale-out**: No session affinity needed

#### 4.3.4 State Backup and Recovery
- Policy store: Git repository backup (replicated)
- Decision cache: Recoverable from PDP (cold start)
- Audit log: Elasticsearch backup (snapshot daily)
- RTO: 5 minutes for PDP, 15 minutes for full recovery
- RPO: 0 for policy store (Git), 1 minute for audit log

---

## 5. Communication Protocols, APIs, Events, Queues

### 5.1 Communication Protocols

| Protocol | Usage | Port | Authentication | Encryption |
|---|---|---|---|---|
| gRPC (TLS) | PDP API (primary) | 8444 | mTLS (SPIFFE) | TLS 1.3 |
| HTTP/2 (TLS) | PDP API (fallback) | 8445 | mTLS (SPIFFE) | TLS 1.3 |
| HTTPS | PEP external check | 443 | JWT | TLS 1.3 |
| HTTP | OPA bundle distribution | 8181 | Token | TLS 1.3 |
| gRPC | Envoy ext_authz | 9001 | mTLS | TLS 1.3 |

### 5.2 APIs

#### 5.2.1 PDP API (gRPC - Primary)
```
rpc Authorize (AuthorizeRequest) returns (AuthorizeResponse)
rpc AuthorizeBatch (stream AuthorizeRequest) returns (stream AuthorizeResponse)
rpc GetAllowedActions (GetAllowedActionsRequest) returns (AllowedActionsResponse)
rpc EvaluatePolicy (EvaluatePolicyRequest) returns (EvaluatePolicyResponse)
rpc HealthCheck (HealthCheckRequest) returns (HealthCheckResponse)
```

#### 5.2.2 PDP API (REST - Secondary)
```
POST /api/v1/authz/authorize           - Single authorization decision
POST /api/v1/authz/authorize/batch     - Batch authorization decisions
POST /api/v1/authz/allowed-actions     - Get allowed actions for resource
POST /api/v1/authz/evaluate            - Evaluate policy with custom input
GET  /api/v1/authz/health              - Health check
GET  /api/v1/authz/metrics             - PDP metrics
GET  /api/v1/authz/policies            - List loaded policies
GET  /api/v1/authz/policies/{id}       - Get policy details
```

#### 5.2.3 Policy Management API (Internal - REST)
```
POST   /api/v1/policies                - Create new policy
GET    /api/v1/policies                - List policies
GET    /api/v1/policies/{id}           - Get policy details
PUT    /api/v1/policies/{id}           - Update policy
DELETE /api/v1/policies/{id}           - Delete policy
POST   /api/v1/policies/{id}/validate  - Validate policy syntax
POST   /api/v1/policies/{id}/test      - Run policy tests
POST   /api/v1/policies/{id}/deploy    - Deploy policy to PDPs
POST   /api/v1/policies/{id}/rollback  - Rollback to previous version
GET    /api/v1/policies/{id}/versions  - List policy versions
```

#### 5.2.4 Cache Management API (Internal - REST)
```
POST   /api/v1/cache/invalidate        - Invalidate cache entries
POST   /api/v1/cache/invalidate/subject - Invalidate by subject
POST   /api/v1/cache/invalidate/resource - Invalidate by resource
POST   /api/v1/cache/warm              - Pre-warm cache entries
GET    /api/v1/cache/stats             - Cache statistics
DELETE /api/v1/cache                   - Clear entire cache
```

### 5.3 Events

#### 5.3.1 Authorization Events
```
authz.decision.allowed       - Authorization granted
authz.decision.denied        - Authorization denied
authz.decision.indeterminate - Policy evaluation error
authz.policy.created         - New policy created
authz.policy.updated         - Policy updated
authz.policy.deleted         - Policy deleted
authz.policy.deployed        - Policy deployed to PDP
authz.policy.rolled_back     - Policy rolled back
authz.policy.error           - Policy evaluation error
authz.cache.invalidated      - Cache invalidation event
authz.cache.warming          - Cache warming event
```

#### 5.3.2 Audit Event Schema
```json
{
  "event_type": "authz.decision.allowed|denied|indeterminate",
  "timestamp": "RFC3339Nano",
  "request_id": "uuid",
  "decision_id": "uuid",
  "subject": {
    "identity_id": "uuid",
    "identity_type": "agent|service|user",
    "identity_name": "string"
  },
  "action": {
    "operation": "read|write|execute"
  },
  "resource": {
    "type": "model|memory|tool",
    "path": "/resource/path"
  },
  "decision": "allow|deny",
  "reason": "string",
  "matched_policies": ["policy-name-1"],
  "evaluation_time_ms": 2.5,
  "trace_id": "distributed-trace-id",
  "pdp_instance": "pdp-01",
  "policy_bundle_version": "2024-01-15-1030"
}
```

### 5.4 Queues

| Queue | Content | Consumer | Priority | Retention |
|---|---|---|---|---|
| authz.decisions | All authorization decisions | Audit logger | Normal | 7 days |
| authz.denied | Denied authorization attempts | Security monitor | High | 14 days |
| authz.policy.updates | Policy change notifications | PDP instances | High | 1 hour |
| authz.cache.invalidation | Cache invalidation commands | PEP instances | High | 1 hour |
| authz.audit | Authorization audit log entries | Compliance store | Normal | 30 days |

---

## 6. Caching, Memory, Persistence

### 6.1 Caching Strategy

#### 6.1.1 Decision Cache (L1 - Local)
- **Store**: Local memory (each PDP/PEP instance)
- **Key**: hash(subject_id + action + resource_path + context_checksum)
- **TTL**: 5 minutes (configurable)
- **Size**: 100,000 entries per instance
- **Eviction**: LRU
- **Hit Rate Target**: > 80% for repeated decisions
- **Invalidation**: Remote invalidation via Redis pub/sub

#### 6.1.2 Decision Cache (L2 - Distributed)
- **Store**: Redis Cluster
- **Key**: authz:decision:{hash}
- **TTL**: 5 minutes
- **Size**: 10M entries per cluster
- **Replication**: 3 replicas per shard
- **Consistency**: Eventual (acceptable for authorization cache)

#### 6.1.3 Policy Bundle Cache
- **Store**: Local filesystem (each PDP)
- **Content**: Compiled Rego policies
- **Update**: Pull from bundle server on change notification
- **Version Tracking**: Bundle version + last updated timestamp

#### 6.1.4 Role/Attribute Cache
- **Store**: Redis
- **Key**: authz:roles:{subject_id}, authz:attributes:{subject_id}
- **TTL**: 10 minutes
- **Invalidation**: On role change or attribute update

### 6.2 Memory Requirements

| Component | Min Memory | Recommended | Max Memory | Notes |
|---|---|---|---|---|
| PDP (OPA) | 512MB | 2GB | 8GB | Scales with policy complexity |
| PEP (Envoy) | 256MB | 1GB | 4GB | Inline to proxy |
| Policy Store | 256MB | 1GB | 4GB | Git + bundle server |
| Cache Layer | 1GB | 4GB | 16GB | Redis cluster |

### 6.3 Persistence

#### 6.3.1 Policy Store
- **Primary**: Git repository (scalable, distributed)
- **Backup**: Git replicas, cloud storage backup
- **Versioning**: Full Git history
- **Access**: HTTPS or SSH for Git operations

#### 6.3.2 Decision Audit Log
- **Primary**: Elasticsearch
- **Index**: `authz-log-{YYYY.MM.DD}`
- **Retention**: Hot 7 days, Warm 90 days, Cold 7 years
- **Backup**: Snapshot to S3 daily

#### 6.3.3 Policy Testing Data
- **Primary**: Git repository (test files alongside policies)
- **Storage**: Test cases, fixtures, expected results
- **Format**: JSON test files with Rego test framework

---

## 7. Validation, Retry Logic, Error Recovery

### 7.1 Validation Rules

#### 7.1.1 Policy Validation
- Syntax validation: valid Rego syntax required
- Semantic validation:
  - No undefined references
  - No infinite recursion
  - Type checking against input schema
  - No unreachable rules (dead code)
- Conflict detection:
  - Both allow and deny for same input
  - Contradictory conditions
  - Overlapping rule scope
- Performance validation:
  - Evaluation time < 10ms for simple policies
  - Evaluation time < 50ms for complex policies
  - No O(n^2) or worse complexity patterns

#### 7.1.2 Authorization Request Validation
- Subject identity: must be a valid, non-expired identity
- Action: must be a recognized operation
- Resource type: must be a known resource type
- Context: timestamp must be within reasonable bounds
- Size limits: request < 100KB, batch < 1000 requests

#### 7.1.3 Input Schema Validation
All authorization requests validated against JSON Schema:
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "required": ["subject", "action", "resource"],
  "properties": {
    "subject": {
      "type": "object",
      "required": ["identity_id", "identity_type"],
      "properties": {
        "identity_id": {"type": "string", "format": "uuid"},
        "identity_type": {"type": "string", "enum": ["agent", "service", "user", "plugin"]},
        "roles": {"type": "array", "items": {"type": "string"}}
      }
    },
    "action": {
      "type": "object",
      "required": ["operation"],
      "properties": {
        "operation": {"type": "string", "enum": ["create", "read", "update", "delete", "execute", "admin"]}
      }
    },
    "resource": {
      "type": "object",
      "required": ["type"],
      "properties": {
        "type": {"type": "string"},
        "path": {"type": "string", "maxLength": 1024}
      }
    }
  }
}
```

### 7.2 Retry Logic

#### 7.2.1 PDP Unavailable Retry
- **Strategy**: Exponential backoff with jitter
- **Initial Delay**: 50ms
- **Max Delay**: 2 seconds
- **Max Retries**: 3
- **Backoff Formula**: `50 * 2^attempt + random(0, 25) ms`
- **Retry Conditions**: Connection refused, timeout, 503
- **Fail-Open Policy**: Configurable per PEP (default: fail closed)

#### 7.2.2 Cache Read Retry
- **Strategy**: Immediate retry once
- **Delay**: 10ms
- **Retry Condition**: Connection error to Redis
- **Fallback**: Skip cache, call PDP directly

#### 7.2.3 Policy Bundle Download Retry
- **Strategy**: Exponential backoff
- **Initial Delay**: 1 second
- **Max Delay**: 30 seconds
- **Max Retries**: 10
- **Retry Conditions**: Connection error, HTTP 5xx
- **Fallback**: Continue with current bundle

### 7.3 Error Recovery

#### 7.3.1 PDP Failure
```
Detection: Health check fails, >1% error rate
Impact: Authorization decisions unavailable
Recovery:
  1. PEP switches to local cache (existing decisions)
  2. New PDP instances auto-scaled
  3. Failed PDP restarted
  4. Cache warmed before accepting traffic
  5. Gradual traffic shift to recovered PDP
```

#### 7.3.2 Policy Evaluation Error
```
Detection: Indeterminate decision count > threshold
Impact: Requests denied or failed open
Recovery:
  1. Log full OPA trace for debugging
  2. If policy error: rollback to previous version
  3. If data error: correct data, reload bundle
  4. Verify fix with policy tests
```

#### 7.3.3 Cache Corruption
```
Detection: Cache hit rate drops, decisions inconsistent
Impact: Increased PDP load, stale decisions
Recovery:
  1. Invalidate entire cache
  2. Warm cache with common decisions
  3. Monitor for decision consistency
  4. Verify cache integrity
```

---

## 8. Security, Authentication, Authorization, Permissions

### 8.1 Authorization System Security

#### 8.1.1 PDP Security
- PDP API requires mTLS with valid SPIFFE identity
- Policy bundle distribution over TLS
- Signed policy bundles to prevent tampering
- Audit logging of all PDP API calls
- Rate limiting on PDP API
- Input validation to prevent Rego injection

#### 8.1.2 Policy Security
- Policies stored in signed Git repository
- Policy changes require code review
- Policy deployment requires approval
- Policy evaluation in sandboxed environment
- No network access from OPA policies (by default)
- Resource limits on policy evaluation (CPU, memory, time)

#### 8.1.3 PEP Security
- PEP runs in same security context as proxy/service
- PEP configuration protected from unauthorized changes
- PEP-to-PDP communication over mTLS
- PEP local cache encrypted at rest

### 8.2 Authorization Models

#### 8.2.1 RBAC (Role-Based Access Control)
```
Users/Agents assigned to roles
Roles have permissions
Permissions granted to resources

Example:
  Role: content_editor
  Permissions: model:execute, storage:write
  Assigned to: content-gen-agent-01, content-gen-agent-02
```

#### 8.2.2 ABAC (Attribute-Based Access Control)
```
Policies evaluate subject, resource, and environment attributes

Example:
  Allow if:
    subject.clearance_level >= resource.required_clearance
    AND subject.department == resource.owning_department
    AND context.time between 09:00 and 17:00
```

#### 8.2.3 ReBAC (Relationship-Based Access Control)
```
Access based on relationships between subjects and resources

Example:
  Allow if:
    subject == resource.owner
    OR subject in resource.team_members
    OR subject.manager_of(resource.owner)
```

### 8.3 Policy Decision Points

```
Every entry point has a PEP:

1. External API Gateway:
   - PEP checks every external API call
   - Identity from JWT/OAuth2 token
   - Resource from API path

2. Service Mesh (Sidecar):
   - PEP checks every service-to-service call
   - Identity from mTLS SPIFFE ID
   - Resource from HTTP header/gRPC metadata

3. SDK (In-Process):
   - PEP checks internal function calls
   - Identity from current execution context
   - Resource from function parameters

4. Kernel (eBPF):
   - PEP checks system calls
   - Identity from process credentials
   - Resource from file/network operation targets
```

---

## 9. Monitoring, Logging, Metrics, Tracing

### 9.1 Key Metrics

| Metric | Type | Description | Warning | Critical |
|---|---|---|---|---|
| authz.decisions.total | Counter | Total authorization decisions | - | - |
| authz.decisions.allowed | Counter | Allowed decisions | - | - |
| authz.decisions.denied | Counter | Denied decisions | > 1000/min | > 10000/min |
| authz.decisions.indeterminate | Counter | Error decisions | > 1/min | > 10/min |
| authz.evaluation.latency | Histogram | Policy evaluation time | p99 > 10ms | p99 > 50ms |
| authz.cache.hit_rate | Gauge | Decision cache hit rate | < 70% | < 50% |
| authz.cache.size | Gauge | Cache entry count | > 80% | > 95% |
| authz.policy.count | Gauge | Number of loaded policies | - | - |
| authz.pdp.cpu | Gauge | PDP CPU utilization | > 70% | > 85% |
| authz.pdp.memory | Gauge | PDP memory utilization | > 75% | > 90% |
| authz.policy.eval_time | Histogram | Per-policy evaluation time | p99 > 5ms | p99 > 20ms |
| authz.denied.by_resource | Counter | Denied by resource type | Monitor | Investigate |

### 9.2 Dashboards (Grafana)

#### Dashboard 1: Authorization Overview
- Decision rate (allow vs deny over time)
- Authorization latency (p50/p95/p99)
- Cache hit rate
- Top denied resources
- Top denied subjects
- Policy error rate

#### Dashboard 2: Policy Performance
- Per-policy evaluation time
- Per-policy invocation count
- Policy bundle version timeline
- Policy compilation time
- Most expensive policies (by eval time)

#### Dashboard 3: Security Monitoring
- Denied authorization alerts over time
- Indeterminate decisions (policy errors)
- Authorization attempts by zone
- Suspicious authorization patterns
- Rate limit triggers

### 9.3 Logging

- All authorization decisions logged (allow and deny)
- Log format: structured JSON
- Log fields: request_id, subject, action, resource, decision, reason, matched_policies, latency, trace_id
- Log levels: INFO for decisions, WARN for denied, ERROR for indeterminate
- Audit logging: immutable, tamper-evident store
- Sampling: 100% of denied/error, 10% of allowed (configurable)

### 9.4 Tracing

- Distributed tracing with OpenTelemetry
- Spans: authz.authorize, authz.evaluate_policy, authz.check_cache, authz.enforce
- Trace context propagated from authenticated request
- Always sample for denied and error decisions
- Variable sampling for allowed decisions (1-10%)

---

## 10. Scalability, Performance, Optimization

### 10.1 Scalability

#### 10.1.1 PDP Horizontal Scaling
- Stateless PDP instances behind load balancer
- Auto-scaling based on CPU utilization (target 60%)
- No session affinity required
- Up to 100 instances per region
- Estimated 10,000 decisions/second per instance

#### 10.1.2 Cache Scaling
- Redis Cluster with data sharding
- Add shards as cache size grows
- Read replicas for high-read scenarios
- Estimated 1M decisions cached per 1GB

#### 10.1.3 PEP Scaling
- PEP scales with parent component (Envoy, Sidecar)
- No additional scaling concerns
- Local cache reduces PDP load

### 10.2 Performance Targets

| Operation | Target p50 | Target p95 | Target p99 | Max |
|---|---|---|---|---|
| Simple authorization (cached) | 0.5ms | 2ms | 5ms | 10ms |
| Simple authorization (uncached) | 2ms | 8ms | 20ms | 50ms |
| Complex authorization (cached) | 1ms | 4ms | 10ms | 20ms |
| Complex authorization (uncached) | 5ms | 20ms | 50ms | 100ms |
| Batch authorization (100 items) | 10ms | 30ms | 80ms | 200ms |
| Policy compilation | 100ms | 500ms | 2s | 5s |

### 10.3 Optimization Techniques

1. **Policy Optimization**:
   - Pre-compile policies to bytecode
   - Partial evaluation for static inputs
   - Rule indexing for faster matching
   - Avoid expensive operations (regex, recursion) in hot paths

2. **Decision Caching**:
   - Cache decisions aggressively
   - Negative caching (cache denied as well)
   - Cache warming on deployment
   - Hierarchical cache: local > Redis > PDP

3. **Batch Processing**:
   - Batch authorization requests where possible
   - OPA partial evaluation for repeated queries
   - Pipeline multiple decisions through single evaluation

4. **Connection Optimization**:
   - Connection pooling to Redis and PDP
   - gRPC streaming for batch operations
   - Keep-alive connections
   - Protocol Buffers for serialization

---

## 11. Testing Strategy, Risk Analysis

### 11.1 Testing Strategy

#### 11.1.1 Policy Testing
- Unit tests for each policy rule
- Integration tests for complete policies
- Fuzz testing with random inputs
- Negative testing: unexpected inputs, edge cases
- Performance regression testing
- Conflict detection testing

#### 11.1.2 PDP Testing
- Load testing at 2x expected peak
- Latency testing under various loads
- Failover testing (PDP instance failure)
- Cache behavior testing (hit/miss, invalidation)
- Policy update testing (hot reload, rollback)

#### 11.1.3 Integration Testing
- PEP + PDP integration
- Cache + PDP integration
- Policy store + PDP integration
- Authentication + Authorization integration
- End-to-end authorization flow

### 11.2 Risk Analysis

| Risk | Probability | Impact | Score | Mitigation |
|---|---|---|---|---|
| Policy evaluation too slow | Medium | High | 12 | Performance testing, optimization |
| Incorrect policy | Medium | Critical | 15 | Testing, review, canary deploy |
| PDP overload | Medium | High | 12 | Auto-scaling, caching, rate limiting |
| Cache inconsistency | Low | Medium | 6 | Short TTL, invalidation |
| Rego injection | Low | Critical | 9 | Input validation, sandboxing |
| Policy bypass | Low | Critical | 9 | Multiple PEP layers, audit |
| Authorization flood | High | Medium | 10 | Rate limiting, auto-scaling |

---

## 12. Failure Scenarios, Recovery Procedures

### 12.1 PDP Complete Failure
```
Symptoms: All PDP instances unreachable
Impact: No new authorization decisions
Recovery:
  1. PEPs use local cache for existing decisions
  2. New requests fail closed (denied) or open based on config
  3. Auto-scaling launches new PDP instances
  4. Policy bundles loaded from bundle server
  5. Cache warmed with high-frequency decisions
  6. Gradual traffic shift to new PDPs
```

### 12.2 Incorrect Policy Deployment
```
Symptoms: Unexpected denials or allows after policy update
Detection: Monitoring alerts on authorization pattern change
Recovery:
  1. Automatic rollback to previous policy version
  2. Invalidate all cached decisions
  3. Verify authorization behavior returns to normal
  4. Investigate policy change in staging
  5. Re-deploy with fixes after testing
```

### 12.3 Cache Stampede
```
Symptoms: Cache miss storm, PDP overload after cache invalidate
Detection: Sudden PDP CPU spike, increased latency
Recovery:
  1. Cache regeneration with rate limiting
  2. Thundering herd protection (single flyweight pattern)
  3. Gradual TTL increase during recovery
  4. Pre-warm cache for critical decisions
```

---

## 13. Operational Limits, Maintenance

### 13.1 Operational Limits

| Parameter | Soft Limit | Hard Limit | Action |
|---|---|---|---|
| Policies per PDP | 1000 | 5000 | Review and consolidate |
| Policy size | 100KB | 1MB | Optimize or split |
| Decision cache entries | 1M | 10M | Increase cache size |
| Cache TTL | 5 min | 30 min | Security review |
| Authorization request size | 10KB | 100KB | Reject larger |
| Batch request count | 100 | 1000 | Reject larger |
| PDP instances | 10 | 100 | Review architecture |
| Policy compilation time | 1s | 10s | Optimize policies |

### 13.2 Maintenance

- Weekly: Review denied authorization patterns
- Monthly: Policy performance review, cache hit rate analysis
- Quarterly: Full policy audit, testing, optimization
- Annually: Authorization architecture review, threat model update

---

## 14. Future Improvements, Examples

### 14.1 Future Improvements

- AI-assisted policy generation from access patterns
- Real-time policy recommendation engine
- Automated policy optimization
- Cross-cluster policy synchronization
- Policy visualization and debugging tools
- Natural language policy authoring
- Predictive authorization (pre-authorize based on patterns)
- Zero-trust network policies integration

### 14.2 Example Policies

#### Example: Content Generation Agent Policy
```rego
package aios.authz.content_gen

default allow := false

allow {
    input.subject.attributes.agent_type == "content_generation"
    input.action.operation == "execute"
    input.resource.type == "model"
    input.resource.attributes.category == "generation"
}

allow {
    input.subject.attributes.agent_type == "content_generation"
    input.action.operation == "read"
    input.resource.type == "memory"
    input.resource.attributes.memory_type == "context"
}

deny[{"reason": "ContentGen agents cannot call analysis models"}] {
    input.subject.attributes.agent_type == "content_generation"
    input.resource.type == "model"
    input.resource.attributes.category == "analysis"
}
```

---

## 15. Acceptance Criteria, Definition of Done

### 15.1 Acceptance Criteria
- [ ] PDP evaluates policies and returns decisions within 10ms p99
- [ ] PEP enforces decisions at all entry points (gateway, mesh, SDK, kernel)
- [ ] Caching reduces PDP load by > 80%
- [ ] Policy updates take effect within 30 seconds
- [ ] All authorization decisions audited
- [ ] RBAC, ABAC, ReBAC models supported
- [ ] Policy testing framework operational
- [ ] Rollback capability tested

### 15.2 Definition of Done
1. PDP/PEP architecture fully specified
2. All authorization models documented with examples
3. Policy lifecycle (author -> test -> deploy -> monitor) defined
4. Performance targets specified and achievable
5. All failure scenarios documented with recovery
6. Monitoring, metrics, and dashboards defined
7. Security controls for authz system documented
8. Integration with AuthN and Permission systems specified
