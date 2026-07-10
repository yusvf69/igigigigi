# AIOS Configuration System — Engineering Specification

**Document ID:** 40_AIOS_Configuration_System  
**Version:** 1.0.0  
**Status:** Draft  
**Classification:** Enterprise Internal  
**Owner:** AIOS Platform Engineering  
**Last Updated:** 2026-07-10  

---

## 1. Purpose

The AIOS Configuration System is the authoritative, centralized configuration management subsystem of the Artificial Intelligence Operating System. It provides a unified, declarative, and secure mechanism for defining, storing, distributing, validating, and observing configuration across every component, agent, deployment, and tenant within the AIOS ecosystem. The system is designed to support zero-downtime changes, dynamic hot-reloading, full version history, cryptographic security for secrets, and GitOps-driven configuration-as-code workflows at enterprise scale.

All AIOS subsystems — including the Agent Runtime, Workflow Engine, Scheduler, Resource Manager, Telemetry Pipeline, Security Layer, and I/O Layer — depend on the Configuration System for their operational parameters, feature flags, connection strings, tuning parameters, and behavioral policies. Without this subsystem, no component can initialize, authenticate, or communicate. It is the first subsystem to boot and the last to shut down.

---

## 2. Mission

Deliver a configuration platform that is:

- **Deterministic:** Given the same configuration inputs, every instance produces identical effective configuration.
- **Dynamic:** Configuration changes take effect without restarting or re-deploying any component.
- **Secure:** Secrets never touch disk unencrypted; access is audited and authorized at every layer.
- **Auditable:** Every configuration change is recorded with actor, timestamp, before/after diff, and approval chain.
- **Resilient:** The system tolerates failures of upstream config sources, network partitions, and corrupt payloads.
- **Extensible:** New configuration sources, formats, and validation rules can be added via plugin without modification to the core engine.
- **Scalable:** The system supports millions of configuration keys across thousands of services and tens of thousands of agents.

---

## 3. Responsibilities

### 3.1 Core Responsibilities

- **Configuration Source Ingestion:** Read configuration from files (JSON, YAML, TOML), environment variables, CLI arguments, remote config servers (etcd, Consul, ZooKeeper), and secrets vaults (HashiCorp Vault, AWS Secrets Manager, GCP Secret Manager).
- **Hierarchical Merge:** Apply a strict precedence hierarchy (listed below) and produce a single consolidated configuration object for every consumer.
- **Schema Validation:** Validate every configuration value against its declared JSON Schema or Protobuf schema before it is ever applied.
- **Dynamic Hot Reload:** Watch for configuration changes from every source and propagate changes live to subscribed components without restart.
- **Configuration Versioning:** Assign a monotonically increasing version number to every effective configuration change.
- **Config-as-Code Integration:** Expose a GitOps-compatible interface: config changes are made via pull requests, validated in CI/CD, and applied automatically.
- **Secrets Management:** Transparently resolve secret references (e.g., `vault://path/to/secret#key`) by fetching from the configured secrets vault, caching securely, and rotating automatically.
- **Encryption:** Encrypt sensitive configuration values using AES-256-GCM with envelope encryption (master key wrapping data keys).
- **Access Control:** Enforce read/write/approve permissions on every configuration path, backed by RBAC and ABAC policies.
- **Drift Detection:** Continuously compare the actual running configuration against the expected declared configuration and alert on any divergence.
- **Export & Import:** Support full backup, restore, and migration of configuration between environments.
- **Feature Flags:** Manage boolean, percentage-based, and segment-based feature flags with gradual rollout, canary deployment, and A/B test configuration.

### 3.2 Non-Responsibilities

- The Configuration System does **not** execute agent logic or orchestrate agent workflows.
- The Configuration System does **not** store agent state, session data, or agent memory.
- The Configuration System does **not** handle inter-agent messaging or event routing (handled by the Event Bus).
- The Configuration System does **not** perform secret rotation itself; it triggers rotation via integration with the Secrets Vault agent.
- The Configuration System does **not** provide a general-purpose key-value store; it is configuration-focussed only.
- The Configuration System does **not** compile or transpile code; configuration values are data only.
- The Configuration System does **not** enforce runtime SLA or SLO guarantees for agent execution.
- The Configuration System does **not** replace the AIOS Resource Manager for compute resource allocation.

---

## 4. Architecture

### 4.1 High-Level Architecture

The AIOS Configuration System follows a layered, mediator-based architecture with pluggable source adapters, a central merge and validation engine, and a distribution layer that pushes configuration to all subscribing components.

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           Configuration Consumers                          │
│  ┌─────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────────┐ │
│  │Agents   │  │Workflows │  │Scheduler │  │Resource  │  │Telemetry     │ │
│  │         │  │Engine   │  │          │  │Manager   │  │Pipeline      │ │
│  └────▲────┘  └────▲─────┘  └────▲─────┘  └────▲─────┘  └──────▲───────┘ │
│       │             │             │             │              │          │
└───────┼─────────────┼─────────────┼─────────────┼──────────────┼──────────┘
        │             │             │             │              │
┌───────┼─────────────┼─────────────┼─────────────┼──────────────┐
│       │             │   Distribution Layer            │              │  │
│       │             │   (Pub/Sub, gRPC streaming,    │              │  │
│       │             │    WebSocket push)              │              │  │
│       └─────────────┼─────────────┼──────────────────┘              │  │
│                     │             │                                 │  │
│               ┌─────▼─────────────▼──────┐                        │  │
│               │   Configuration Cache      │                        │  │
│               │   (In-memory + Redis)      │                        │  │
│               └─────▲────────────────▲───┘                        │  │
│                     │                 │                              │  │
│               ┌─────┴─────────────────┴───┐                        │  │
│               │     Configuration Engine   │                        │  │
│               │  ┌───────────┐ ┌────────┐  │                        │  │
│               │  │ Merge &   │ │ Validate │  │                        │  │
│               │  │ Resolve   │ │ Engine  │  │                        │  │
│               │  └───────────┘ └────────┘  │                        │  │
│               │  ┌───────────┐ ┌────────┐  │                        │  │
│               │  │ Version   │ │ Diff    │  │                        │  │
│               │  │ Manager   │ │ Engine  │  │                        │  │
│               │  └───────────┘ └────────┘  │                        │  │
│               │  ┌───────────┐ ┌────────┐  │                        │  │
│               │  │ Drift     │ │ Access  │  │                        │  │
│               │  │ Detector  │ │ Control │  │                        │  │
│               │  └───────────┘ └────────┘  │                        │  │
│               └─────────▲───────────────────┘                        │  │
│                         │                                            │  │
│               ┌─────────┴───────────────────┐                               │  │
│               │   Source Abstraction   │                               │  │
│               │   Layer                │                               │  │
│               └──┬─────┬──────┬──────┬──┘                               │  │
│                  │     │      │      │                                  │  │
│        ┌─────────┘ ┌───┘ ┌────┘ ┌───┴──────────┐                     │  │
│        ▼         ▼     ▼      ▼                  ▼                     │  │
│  ┌─────────┐ ┌─────┐ ┌────┐ ┌───────────┐ ┌──────────┐              │  │
│  │File     │ │Env  │ │CLI │ │etcd/Consul│ │Vault     │              │  │
│  │Watchers │ │Vars │ │Args │ │Watcher    │ │Watcher   │              │  │
│  └─────────┘ └─────┘ └────┘ └───────────┘ └──────────┘              │  │
└─────────────────────────────────────────────────────────────────────────┘
```

### 4.2 Internal Components

#### 4.2.1 Configuration Engine

The central processing unit. It is responsible for:

- Orchestrating the merge process across all sources in precedence order.
- Calling the validation engine before any configuration is finalized.
- Versioning every finalized configuration snapshot.
- Dispatching configuration deltas to the distribution layer.
- Managing the configuration lifecycle: init → loading → merging → validating → versioning → distributing → monitoring.

The engine operates as a single long-lived goroutine (or equivalent in other languages) with an event-driven internal state machine. It exposes a gRPC API for synchronous queries and an asynchronous watch API for streaming updates.

#### 4.2.2 Merge & Resolve Engine

Applies the configuration hierarchy with the following precedence order (1 = highest priority):

| Priority | Source              | Override Scope       | Notes                                  |
|---------|---------------------|----------------------|----------------------------------------|
| 1       | Instance override   | Single instance      | CLI flag, instance-level config file   |
| 2       | Service override    | Entire service       | Service-level configuration file       |
| 3       | Region override     | Geographic region    | Region-specific overrides              |
| 4       | Environment         | dev/staging/prod     | Environment variable group             |
| 5       | Defaults            | Global fallback      | Built-in defaults or base config file  |

The merge strategy is configurable per key path:

- **Deep Merge:** For object/map values. Recursively merge nested fields. Arrays are appended unless the `replace` directive is set.
- **Shallow Merge:** For flat objects. Top-level keys from higher-precedence sources completely replace top-level keys from lower-precedence sources.
- **Replace:** The higher-precedence value completely replaces the lower-precedence value, regardless of type. This is the default for scalar values.
- **Omit:** The key is not merged at all from this source; the lower-precedence value (or default) is retained.

The Merge & Resolve Engine also performs **variable substitution**. Configuration values may contain references to other configuration keys or environment variables using the syntax `${path.to.key}` or `${ENV_VAR}`. Substitution is performed after merge and before validation.

#### 4.2.3 Validation Engine

The Validation Engine enforces that every configuration value conforms to its schema before it is accepted. It supports three schema formats:

- **JSON Schema (draft-07+):** Used for JSON and YAML based configurations. Supports complex constructs like `oneOf`, `allOf`, `if/then/else`, `patternProperties`, and `contentEncoding`.
- **Protobuf:** Used for high-performance, strongly-typed configurations in the gRPC path. The `.proto` file defines the schema; validation is done via `protoc-gen-validate` rules.
- **Custom Validation Functions:** Plugins can register Go-style validation functions for arbitrary logic (e.g., "this port must be between 1024 and 65535 AND not conflict with a known system port").

Validation is performed in two phases:

1. **Static/Syntactic Validation:** Before the config is stored or applied. Checks types, ranges, patterns, required fields, enum values. Failures are reported immediately and prevent the configuration from being accepted.
2. **Dynamic/Sanity Validation:** After the config is merged but before it is applied. Checks cross-field dependencies, resource availability, port conflicts, required service availability. Failures result in a rollback to the previous valid configuration.

The validation engine maintains a registry of all schemas indexed by configuration path prefix. Each schema entry includes:

- `schema_id`: Globally unique identifier for the schema version.
- `schema_format`: json_schema, protobuf, or custom.
- `schema_content`: The schema definition itself.
- `compatible_versions`: List of previous schema versions that remain compatible (backward compatibility).
- `migration_function`: Optional. A function that migrates configuration from older schema versions to this one.
- `validate_on_read`: Boolean. If true, re-validate every read from cache (default: false for performance; true for secrets and security-critical config).

#### 4.2.4 Version Manager

Assigns every committed configuration snapshot a monotonically increasing, globally unique version number.

- Versions are **epoch-based**: `{epoch}.{sequence}`, where `epoch` is a unix timestamp in seconds (allowing human-friendly ordering) and `sequence` is a local counter per epoch.
- The version is committed to a version history store (backed by a durable log, e.g., a write-ahead log on disk, plus Redis).
- Version history is queryable. Users can fetch: the current version, a specific version by ID, a list of versions within a time range, or the latest version at a given point in time.
- Each version record includes:
  - `version_id`: The version string.
  - `timestamp`: ISO 8601 UTC timestamp.
  - `actor`: The authenticated user or system that initiated the change.
  - `source`: The configuration source type that triggered this version.
  - `change_summary`: A human-readable summary of the change.
  - `checksum`: SHA-256 of the entire effective configuration object at this version.
  - `parent_version`: The version that this change was applied on top of.
  - `metadata`: Arbitrary key-value pairs for custom annotations.

#### 4.2.5 Diff Engine

Produces human-readable and machine-parseable diffs between any two configuration versions.

- **Structural Diff:** Side-by-side comparison of two configuration trees. Shows added, removed, and changed keys. For changed values, shows the old and new values.
- **Semantic Diff:** Beyond structural changes, identifies functional differences: "This change moves the agent timeout from 30s to 60s, affecting reliability but also latency." Semantic diffs use rule-based heuristics and domain knowledge encoded in the configuration schema annotations.
- **Impact Analysis:** Maps configuration changes to the components and agents that would be affected. "This change touches 'scheduling.policy', affecting 12 agents of type 'worker' in region 'us-east-1'."
- **Output Formats:** JSON (machine), Markdown (UI display), HTML (email notifications), Unified Diff Text (Git-style).

#### 4.2.6 Drift Detector

Runs continuously (configurable interval, default 30s) to compare the **actual running configuration** against the **expected declared configuration**.

- **Actual:** Captured by polling the in-memory configuration state of each component via its health/status endpoint.
- **Expected:** The latest committed configuration version from the version manager.
- **Drift Detection Logic:** For each configuration key, compare the value. Ignore keys that are explicitly excluded from drift detection (configurable via annotation `x-aios-drift-exempt: true` in the schema).
- **Drift Reporting:** When drift is detected, the drift detector:
  1. Emits a `config.drift_detected` event.
  2. Logs a WARN-level entry with the drift details: key, expected value, actual value, first-detected timestamp.
  3. Increments a Prometheus counter metric `aios_config_drift_total`.
  4. If escalation is configured (e.g., drift > 5 keys or drift persists > 10 minutes), sends an alert via the Alerting subsystem.
- **Auto-Remediation:** Optionally (controlled by feature flag `config.drift.auto_remediate`), the drift detector can push the expected configuration back to the drifted component via the Distribution Layer.

#### 4.2.7 Access Control Engine

Enforces fine-grained access control on configuration paths.

- **Permission Model:** Combines Role-Based Access Control (RBAC) and Attribute-Based Access Control (ABAC).
- **Permission Dimensions:**
  - `read`: Can read the configuration value.
  - `write`: Can create or update the configuration value.
  - `delete`: Can delete the configuration key.
  - `approve`: Can approve a pending configuration change in the approval workflow.
  - `rollback`: Can initiate a rollback to a previous version.
  - `export`: Can export or back up configuration data.
  - `admin`: Full control including permission management.
- **Policy Resolution:**
  1. ReBAC (Relationship-Based Access Control): Check if the subject has an explicit relationship to the resource (e.g., "is owner of this configuration path").
  2. RBAC: Check if the subject's roles grant the required permission on the resource.
  3. ABAC: Evaluate attribute-based rules (e.g., "allow only if request comes from within corporate network AND during business hours AND for region == same as subject's region").
  4. Deny: If no rule explicitly allows, the request is denied.
- **Approval Workflows:** Changes to certain high-risk configuration paths (e.g., `security.*`, `database.connections.*`, `feature_flags.production.*`) require approval. The workflow is:
  1. Author submits a change request (CR).
  2. CR is routed to the appropriate approvers based on the path and risk level.
  3. Approvers are notified (slack/email/pager).
  4. Approver approves or rejects. Rejection includes a reason.
  5. If approved, the change is automatically applied after a cooling-off period (configurable, default 60s).
  6. If not approved within the TTL (configurable, default 24h), the CR expires and is rejected.
  7. Audit log records every step of the workflow.
- **Change Requests (CRs):** Each CR includes:
  - `cr_id`: UUID.
  - `author`: Authenticated user.
  - `created_at`: Timestamp.
  - `config_path`: The configuration path being modified.
  - `old_value`: The value before the change (serialized).
  - `new_value`: The value after the change (serialized).
  - `diff`: The diff (generated by the Diff Engine).
  - `reason`: Author-provided justification.
  - `risk_level`: low / medium / high / critical.
  - `approvers`: List of users notified for approval.
  - `status`: pending / approved / rejected / expired / applied.
  - `approval_trail`: Ordered list of { approver, action, timestamp, reason }.

#### 4.2.8 Template Engine

Enables dynamic configuration using Go templates (or equivalent).

- **Template Syntax:** Uses `{{ .Path.To.Key }}` for configuration references, `{{ env "ENV_VAR" }}` for environment variables, `{{ secret "vault://path#key" }}` for secret references.
- **Function Library:** Built-in functions include:
  - `default value key`: Return `key` if it exists and is non-zero, else return `value`.
  - `contains string substring`: Check if string contains substring.
  - `merge dict1 dict2`: Merge dictionaries.
  - `toUpper`, `toLower`, `trim`, `split`, `replace`.
  - `now`: Current timestamp.
  - `uuid`: Generate a random UUID.
  - `encrypt value`: Encrypt the value with the system key (for inline encryption in config).
  - `base64encode`, `base64decode`.
- **Template Execution Order:** Templates are evaluated after merge but before validation. Circular references (template A references template B which references template A) are detected and result in a validation error.
- **Template Caching:** Rendered template results are cached by configuration version. If the underlying configuration does not change, the cached result is reused.

#### 4.2.9 Feature Flag Manager

Manages the lifecycle of feature flags: create, read, update, delete, enable, disable, rollout.

- **Flag Definition:**
  ```json
  {
    "name": "new_scheduler_algorithm",
    "description": "Use the new load-aware scheduling algorithm",
    "owner": "scheduler-team",
    "created_at": "2026-01-15T10:00:00Z",
    "type": "boolean",
    "default_value": false,
    "rollout": {
      "percentage": 50,
      "segments": [
        {"type": "agent_type", "value": "worker", "percentage": 100},
        {"type": "tenant", "value": "acme-corp", "percentage": 100},
        {"type": "region", "value": "us-east-1", "percentage": 30}
      ],
      "gradual_increment": 5,
      "gradual_interval_seconds": 3600,
      "cooldown_seconds": 300
    },
    "kill_switch": false,
    "depends_on_flags": ["enable_scheduler_v2"],
    "expires_at": "2026-06-01T00:00:00Z"
  }
  ```

- **Rollout Strategies:**
  - **Gradual Rollout:** Increase percentage by `gradual_increment` every `gradual_interval_seconds`. Pause or rollback at any time via kill switch.
  - **Segmented Rollout:** Enable for specific tenant IDs, agent types, regions, or deployment IDs.
  - **A/B Test:** Randomly assign users/agents to variant A (control) or variant B (treatment). Expose the assignment to the telemetry pipeline for analysis.
  - **Canary:** Enable for a single instance or small group first, then expand.
- **Kill Switch:** An emergency stop that instantly disables a feature flag across all deployments, overriding all rollout percentages. Used when a bad flag causes production issues.
- **Flag Evaluation:** Feature flags are evaluated client-side by the AIOS agent SDK. The Configuration System pushes the full flag configuration (flag name, enabled status, rollout rules, kill switch status) to every agent. The agent evaluates locally for performance. The client SDK handles:
  1. Check kill switch — if true, return default value.
  2. Check if flag is expired — if expired, return default value.
  3. Check segment match — if the agent matches a segment override, use that segment's value.
  4. Check rollout percentage — generate a consistent hash of (agent_id + flag_name) and mod 100. If hash < percentage, return the flag's value; otherwise return default.

#### 4.2.10 Config-as-Code (GitOps) Integration

Exposes a GitOps interface where configuration changes flow through a Git repository.

- **Repository Structure:**
  ```
  config-gitops/
  ├── defaults/
  │   ├── base.yaml
  │   └── agent_defaults.yaml
  ├── environments/
  │   ├── dev/
  │   │   ├── override.yaml
  │   │   └── secrets.yaml (encrypted)
  │   ├── staging/
  │   │   ├── override.yaml
  │   │   └── secrets.yaml (encrypted)
  │   └── prod/
  │       ├── override.yaml
  │       ├── secrets.yaml (encrypted)
  │       └── regions/
  │           ├── us-east-1.yaml
  │           └── eu-west-1.yaml
  ├── services/
  │   ├── scheduler.yaml
  │   └── telemetry.yaml
  ├── feature_flags/
  │   ├── flags.yaml
  │   └── experiments.yaml
  └── schemas/
      ├── scheduler.schema.json
      └── telemetry.schema.json
  ```

- **Workflow:**
  1. Developer creates a pull request modifying configuration files in the GitOps repo.
  2. CI/CD pipeline runs:
     - Schema validation against schemas in the `schemas/` directory.
     - Template rendering and validation.
     - Mounting validation (check that the config can be mounted on the target environment).
     - Drift simulation: What would this change do? Show the diff.
     - Access control validation: Does the PR author have permission to modify these paths?
     - Automatic approval for low-risk changes; manual approval for high-risk changes.
  3. PR is merged to the main branch.
  4. GitOps operator (an AIOS subsystem) detects the merge, fetches the new config, applies it via the Configuration Engine.
  5. Post-apply validation confirms the config is active.
  6. If post-apply validation fails, the operator automatically reverts to the previous commit.

### 4.3 External Components

#### 4.3.1 File Watcher

Monitors configuration files on disk for changes and triggers reload.

- **Watched Formats:** `.yaml`, `.yml`, `.json`, `.toml`, `.hcl`.
- **Watch Mechanism:** Uses OS-native file system notification (FSEvents on macOS, inotify on Linux, ReadDirectoryChanges on Windows) with a fallback to polling (default interval: 10s) if native watchers are unavailable.
- **Debouncing:** File change events are debounced (configurable, default 500ms) to avoid processing intermediate states during write operations.
- **Atomicity:** Watcher detects atomic moves and symlink replacements. It checks that the file has a valid checksum before loading.
- **Watch Directories:** The watcher can watch entire directories. New files added to a watched directory are automatically picked up. This supports Kubernetes ConfigMap updates (where the Volume mounts update symlinks).
- **File Change Event:** When a change is detected, the File Watcher emits a `source.file.changed` event with the file path, new content, and old checksum.

#### 4.3.2 Environment Variable Provider

Reads configuration from environment variables.

- **Mapping Convention:** Environment variables are mapped to configuration keys using a configurable prefix (default: `AIOS_`). For example:
  - `AIOS_SCHEDULER_TIMEOUT=60` maps to `scheduler.timeout`.
  - `AIOS_AGENT_DEFAULT_TTL=300` maps to `agent.default.ttl`.
- **Nested Key Delimiter:** Uses underscore (`_`) as delimiter for nested keys. To support keys with underscores, the user can use a double underscore (`__`) to represent a literal underscore. Example: `AIOS_DB__CONNECTION_STRING` maps to `db_connection.string` (the `__` is an escaped underscore).
- **Type Coercion:** Environment variables are strings. The provider attempts to coerce them to the expected type based on the schema:
  - `"true"` / `"false"` → boolean.
  - Numeric strings → int or float.
  - Quoted strings → strings (with quotes stripped).
  - JSON strings (starting with `[` or `{`) → parsed as JSON arrays/objects.
- **Hot Reload:** Environment variables are read once at startup unless `config.env.watch` is enabled (which polls `/proc/self/environ` or uses the file watcher on the environment file).

#### 4.3.3 CLI Args Provider

Reads command-line arguments at process start.

- **Flag Format:** `--config-key=value` or `--config.key.subkey=value`. Short flags are not supported (to avoid conflicts).
- **Priority:** CLI args have the highest priority in the hierarchy (Instance override level).
- **Restrictions:** CLI args cannot be used for dynamic configuration — they are read once on startup and cannot be changed without restarting the process. This is by design: CLI args are for initial bootstrapping only.
- **Usage:** Typically used for:
  - Pointing to a different config source (`--config.source=etcd://localhost:2379`).
  - Overriding specific keys for debugging or one-off runs.
  - Running in migration mode (`--migrate-config`).

#### 4.3.4 Config Server Watcher (etcd / Consul)

Watches remote key-value stores for configuration changes.

- **Supported Backends:**
  - **etcd:** Uses the etcd v3 gRPC API. Watches keys recursively via `Watch` with a prefix.
  - **Consul:** Uses the Consul KV API. Watches keys using Consul's blocking query mechanism (index-based long polling).
  - **ZooKeeper:** Uses ZooKeeper's `getData` with a watch flag.

- **Connection Management:**
  - Establishes a long-lived gRPC/HTTP connection to the config server.
  - Uses exponential backoff with jitter for reconnection (initial: 100ms, max: 30s, jitter: 20%).
  - Health checks every 10s (TCP ping or API health endpoint).
  - Multi-endpoint support: if one endpoint fails, fail over to the next in the list.
  - TLS required for production. mTLS supported for mutual authentication.

- **Key Prefix Convention:** Config server keys follow a path convention:
  ```
  /aios/config/{env}/{region}/{service}/{instance}/{key}
  ```
  The watcher watches all keys under `/aios/config/{environment}/` and applies the hierarchy.

- **Value Format:** Values in the config server can be:
  - Raw text (for simple key-value pairs).
  - JSON-encoded objects.
  - YAML-encoded blocks (for complex configuration).
  - Schema references (e.g., the value is `ref://schema/category` and the actual value is fetched from a different path).

- **Change Detection:** The watcher maintains a local revision index. When the server reports a new revision, the watcher fetches all keys changed since the last known revision and emits individual key-change events.

#### 4.3.5 Secrets Vault Watcher

Integrates with secrets management systems to resolve secret references.

- **Backends:**
  - **HashiCorp Vault:** Using the official Go client. Supports KV v1, KV v2, and dynamic secrets (database credentials, AWS IAM, etc.). Uses Vault Agent for auto-auth (token renewal).
  - **AWS Secrets Manager:** Using the AWS SDK. Supports automatic rotation of RDS credentials.
  - **GCP Secret Manager:** Using the GCP SDK. Supports secret versions.
  - **Azure Key Vault:** Using the Azure SDK.

- **Secret Reference Syntax:**
  ```
  vault://path/to/secret#field_name
  aws://secret_name#field_name
  gcp://projects/{project}/secrets/{secret}/versions/{version}#field_name
  azure://{vault_name}/{secret_name}#field_name
  ```
  The system resolves these references transparently: the path `database.password` might have the value `vault://kv/data/database#password`, and the agent sees only the resolved plaintext (for a brief moment before it re-encrypts it).

- **Caching and Renewal:**
  - Secrets are cached in the configuration cache in encrypted form (re-encrypted with the data key before caching).
  - Cache TTL: configurable (default: 3600s for static secrets, 1800s for dynamic secrets).
  - Automatic renewal: Before the TTL expires, the vault watcher re-fetches the secret and updates the cache.
  - If the vault is unreachable at renewal time, the cached value is retained until the vault is reachable again (or until a configurable `staleness_grace_period` expires, default 2x TTL).
  - After the grace period, the system raises a CRITICAL alert and the configuration key is marked as `stale`.

- **Leasing and Rotation:**
  - For dynamic secrets (e.g., database credentials that expire), the vault watcher handles leasing.
  - It renews leases automatically before they expire.
  - If a secret is rotated, the watcher detects the new version and updates the cache.
  - Rotation events emit `config.secret.rotated` events.

#### 4.3.6 Distribution Layer

The Distribution Layer is responsible for delivering configuration updates to every running component, agent, and service in the AIOS cluster.

- **Delivery Methods:**
  1. **gRPC Server-Side Streaming:** The configuration consumer opens a long-lived gRPC stream (e.g., `WatchConfig(WatchConfigRequest) stream WatchConfigResponse`) and receives configuration deltas as they happen. This is the primary delivery mechanism for AIOS internal components. The stream stays open; the server pushes updates without the client polling.
  2. **Pub/Sub (Message Queue):** The Configuration System publishes configuration change events to a message queue (Kafka, RabbitMQ, or NATS). Subscribers consume and apply the deltas. This is used for components that are not gRPC-capable or that require asynchronous delivery.
  3. **WebSocket Push:** For agent SDKs running in web or mobile environments, configuration is pushed over a WebSocket connection. There is a WebSocket server per region that handles agent subscriptions.
  4. **Polling Fallback:** If streaming/PubSub is unavailable, components fall back to polling. Polling interval is configurable (default 60s). Polling is rate-limited per component to avoid thundering herd.

- **Delivery Guarantees:**
  - At-least-once delivery for configuration updates.
  - Each delivery includes a monotonically increasing sequence number.
  - Consumers deduplicate based on `(config_version, sequence_number)`.
  - Delivery is acknowledged by the consumer. Unacknowledged updates are retried.

- **Initial Synchronization (Initial Configuration Load):**
  1. Consumer starts up and connects to the Configuration System.
  2. The consumer sends its identity (component type, agent ID, version, region, deployment ID).
  3. The Configuration System looks up the effective configuration for that consumer.
  4. The full configuration snapshot is sent as the first message on the stream.
  5. Subsequent messages are only deltas (changes).

- **Configuration Scoping in Distribution:**
  The distribution layer uses consumer identity to send scoped configuration:
  - A scheduler agent in us-east-1 receives only `scheduler.*` and `global.*` config.
  - A worker agent for tenant "acme" receives only `agent.worker.*`, `tenant.acme.*`, and `global.*`.
  - This scoping reduces payload size, bandwidth usage, and processing load on each consumer.

---

## 5. Dependencies

### 5.1 Internal Dependencies

| Dependency          | Type         | Description                                                       |
|---------------------|--------------|-------------------------------------------------------------------|
| AIOS Security Layer | Required     | For authentication of consumers and authorization of config reads. |
| AIOS Event Bus      | Required     | For publishing config change events.                               |
| AIOS Telemetry Layer | Required    | For metrics, logs, and traces.                                    |
| AIOS Metadata Store | Required     | For storing schema registrations and access policies.             |
| AIOS Audit System  | Required     | For recording every config change with actor, timestamp, diff.    |

### 5.2 External Dependencies

| Dependency              | Type           | Justification                                        |
|-------------------------|----------------|--------------------------------------------------------|
| etcd / Consul           | Runtime        | Remote config server backend. At least one required.   |
| HashiCorp Vault         | Runtime        | Secrets management. Recommended.                      |
| Redis (or Memcached)    | Runtime        | Distributed cache for configuration snapshots.          |
| PostgreSQL              | Runtime        | Persistence for version history, CRs, and drift alerts.|
| Apache Kafka / NATS      | Runtime        | Pub/sub for config distribution and event streaming.    |
| File System             | Runtime        | For file watcher and local config file reads.           |
| Git (for GitOps)        | CI/CD          | Configuration as code repository management.            |
| AWS S3 / GCS / Azure Blob | Operational | For config export/import backups.                       |
| Prometheus              | Monitoring     | Metrics exposition.                                     |

### 5.3 Optional Dependencies

| Dependency              | Use Case                            |
|-------------------------|--------------------------------------|
| ZooKeeper              | Alternative config server backend.   |
| AWS Secrets Manager    | Alternative secrets backend.         |
| GCP Secret Manager     | Alternative secrets backend.         |
| Azure Key Vault        | Alternative secrets backend.         |
| RabbitMQ               | Alternative pub/sub.                 |
| Apache ZooKeeper       | Alternative config server.           |
| OpenPolicy Agent (OPA) | External policy evaluation for ABAC. |
| GitHub / GitLab API    | GitOps PR status checks.             |

---

## 6. Inputs

### 6.1 Primary Inputs (Configuration Sources)

| Input Source    | Format(s)                       | Protocol / Interface              | Frequency    |
|-----------------|---------------------------------|-----------------------------------|--------------|
| Config Files    | .yaml, .json, .toml, .hcl, .conf | File system read + FSNotify watch | On start + on change |
| Environment Vars | KEY=VALUE                      | os.Environ() / polling            | On start (poll optional) |
| CLI Args        | --key=value                     | os.Args() parsing                 | On start only |
| Config Server   | JSON (key-value)                | gRPC / HTTP API                   | On start + watch stream |
| Secrets Vault   | JSON (key-value)                | Vault API / AWS SDK / GCP SDK    | On start + TTL renewal |

### 6.2 Configuration Schema Definitions

| Input Source | Format                | Protocol            |
|--------------|-----------------------|---------------------|
| Schema files | .schema.json (JSON Schema), .proto (Protobuf) | File system read |
| Schema registry | gRPC API         | gRPC               |

### 6.3 Access Control Policies

| Input Source | Format               | Protocol         |
|--------------|----------------------|------------------|
| Policy files  | .rego (OPA), .yaml   | File system read |
| Policy API   | gRPC API             | gRPC             |

### 6.4 Management Commands

| Input | Description                  | Protocol |
|-------|------------------------------|----------|
| Apply config | Apply a new configuration   | gRPC / REST API / CLI |
| Rollback | Rollback to a previous version | gRPC / REST API / CLI |
| Get config | Read the current config     | gRPC / REST API / CLI |
| Diff config | Diff two versions          | gRPC / REST API / CLI |
| Export config | Export all config        | gRPC / REST API / CLI |
| List versions | List version history      | gRPC / REST API / CLI |
| Create CR | Create a change request      | gRPC / REST API / CLI |
| Approve CR | Approve a change request     | gRPC / REST API / CLI |
| Create flag | Create a feature flag      | gRPC / REST API / CLI |
| Toggle flag | Toggle a feature flag       | gRPC / REST API / CLI |

---

## 7. Outputs

### 7.1 Outputs to Internal Consumers

| Output | Consumer                   | Format                   | Protocol              | Frequency            |
|--------|----------------------------|--------------------------|-----------------------|----------------------|
| Effective Config | All AIOS components   | JSON / Protobuf          | gRPC streaming        | On change            |
| Config Deltas | All AIOS components      | JSON / Protobuf (diff)   | gRPC streaming / Pub/Sub | On change        |
| Feature Flag Evaluation | Agent SDKs | Protobuf (flag definitions) | gRPC streaming / WebSocket | On change    |
| Version ID | All components         | String (version)        | Included in config     | On change            |

### 7.2 Outputs to External Systems

| Output | Destination | Format          | Protocol          |
|--------|-------------|-----------------|-------------------|
| Audit Events | Audit Log DB | JSON (CloudEvents) | Kafka / gRPC     |
| Metrics | Prometheus | OpenMetrics     | HTTP (scrape endpoint) |
| Alerts | Alerting Service | JSON          | Webhook / Kafka    |
| Drift Reports | Ops Dashboard | JSON (DriftReport) | REST API          |
| Git Status | GitOps Repo | Git commit      | Git                |

### 7.3 Outputs to Human Operators

| Output | Format | Medium |
|--------|--------|--------|
| Config change notification | Slack message | Webhook |
| CR approval request | Slack message / Email | Webhook / SMTP |
| Drift alert | PagerDuty / OpsGenie | API |
| Config diff display | HTML / Markdown | Dashboard / CLI |
| Backup file | .yaml / .json (archive) | S3 / GCS / filesystem |

---

## 8. Data Structures

### 8.1 Configuration Object

The fundamental unit of configuration. It is an immutable, versioned snapshot of all configuration values for a given scope at a given point in time.

```json
{
  "version_id": "1738598400.42",
  "parent_version_id": "1738598300.41",
  "created_at": "2026-02-03T12:00:00.000Z",
  "actor": "user:jdoe@acme.com",
  "source": "gitops",
  "checksum": "sha256:abc123def456...",
  "status": "active",
  "config": {
    "global": {
      "cluster_id": "aios-prod-1",
      "environment": "production",
      "region": "us-east-1"
    },
    "scheduler": {
      "policy": "weighted_round_robin",
      "timeout_seconds": 30,
      "max_retries": 3,
      "queue_depth": 10000
    },
    "agent": {
      "default": {
        "ttl_seconds": 3600,
        "max_concurrent_tasks": 5
      },
      "worker": {
        "ttl_seconds": 7200,
        "max_concurrent_tasks": 10
      }
    },
    "telemetry": {
      "metrics_export_interval_seconds": 15,
      "trace_sample_rate": 0.1
    },
    "feature_flags": {
      "new_scheduler_algorithm": {
        "enabled": true,
        "rollout_percentage": 50
      }
    },
    "security": {
      "tls_enabled": true,
      "tls_min_version": "1.3"
    }
  },
  "metadata": {
    "change_summary": "Enable new scheduler algorithm for 50% rollout.",
    "risk_level": "medium",
    "cr_id": "cr-20260203-a1b2c3",
    "tags": {
      "team": "scheduler-team",
      "ticket": "SCHED-1234"
    }
  }
}
```

### 8.2 ConfigKey

The atomic unit of configuration. A path-value pair with metadata.

```json
{
  "key": "scheduler.timeout_seconds",
  "value": 30,
  "type": "integer",
  "source": "file:config.yaml",
  "source_priority": 4,
  "sensitive": false,
  "schema_id": "aios.scheduler.config-v1",
  "version_introduced": 42,
  "version_last_modified": 42,
  "schema_valid": true,
  "checksum": "sha256:foo"
}
```

### 8.3 ChangeRequest

Represents a proposed configuration change that requires approval.

```json
{
  "cr_id": "cr-20260203-a1b2c3d4",
  "author": "user:jane@acme.com",
  "created_at": "2026-02-03T11:00:00Z",
  "expires_at": "2026-02-03T12:00:00Z",
  "target_path": "scheduler.timeout_seconds",
  "old_value": 20,
  "new_value": 30,
  "diff": {
    "type": "change",
    "path": "scheduler.timeout_seconds",
    "old": 20,
    "new": 30,
    "impact": "Changes agent timeout from 20s to 30s. Affects 15 worker agents in us-east-1.",
    "tokens_changed": ["scheduler", "timeout"]
  },
  "reason": "Increase timeout to accommodate slower model inference times.",
  "risk_level": "medium",
  "required_approvals": 2,
  "approvals": [
    {
      "approver": "user:alice@acme.com",
      "action": "approved",
      "at": "2026-02-03T11:30:00Z",
      "reason": "Approved after reviewing perf test results."
    }
  ],
  "status": "partially_approved",
  "escalation": {
    "escalated_to": null,
    "escalation_level": 0,
    "queue_time_seconds": 1800
  }
}
```

### 8.4 DriftReport

Reported when the actual running configuration differs from the expected.

```json
{
  "report_id": "drift-20260203-001",
  "detected_at": "2026-02-03T11:05:00Z",
  "consumer_id": "scheduler:us-east-1:instance-5",
  "expected_version": "1738598400.42",
  "actual_config_checksum": "sha256:oldchecksum",
  "drifted_keys": [
    {
      "key": "scheduler.timeout_seconds",
      "expected": 30,
      "actual": 20,
      "drift_type": "value_mismatch",
      "first_detected_at": "2026-02-03T11:00:00Z",
      "duration_seconds": 300,
      "auto_remediated": false
    }
  ],
  "severity": "warning",
  "acknowledged": false
}
```

### 8.6 FeatureFlagDefinition

Complete definition of a feature flag.

```json
{
  "name": "new_scheduler_algorithm",
  "description": "Use the new Scheduler v2 algorithm",
  "owner": "scheduler-team",
  "type": "boolean",
  "default_value": false,
  "state": "released",
  "created_at": "2026-01-15T00:00:00Z",
  "updated_at": "2026-02-03T12:00:00Z",
  "version": 5,
  "rollout": {
    "enabled": true,
    "percentage": 50,
    "segments": [
      {"type": "agent_type", "values": ["worker"], "percentage": 100},
      {"type": "tenant_id", "values": ["acme", "globex"], "percentage": 100}
    ],
    "gradual_increment": 5,
    "gradual_interval_seconds": 3600
  },
  "kill_switch": false,
  "experiment_id": "exp-2026-bfae4",
  "depends_on": ["enable_scheduler_v2"],
  "expires_at": "2026-06-01T00:00:00Z",
  "audit_trail": [
    {"ts": "2026-01-15T00:00:00Z", "actor": "user:dev", "action": "created", "value": false},
    {"ts": "2026-01-20T00:00:00Z", "actor": "user:qa", "action": "enabled", "value": true, "rollout_percentage": 10},
    {"ts": "2026-02-03T12:00:00Z", "actor": "user:ops", "action": "rollout", "value": true, "rollout_percentage": 50}
  ]
}
```

### 8.7 SchemaDefinition

Registered schema for a configuration path prefix.

```json
{
  "schema_id": "aios.scheduler-v2",
  "path_prefix": "scheduler",
  "format": "json_schema",
  "content": {
    "$schema": "https://json-schema.org/draft/07/schema#",
    "type": "object",
    "properties": {
      "timeout_seconds": {
      "type": "integer",
        "minimum": 1,
        "maximum": 300,
        "default": 30
      },
      "policy": {
        "type": "string",
        "enum": ["round_robin", "weighted_round_robin", "least_busy"]
      }
    },
    "required": ["timeout_seconds", "policy"],
    "additionalProperties": false
  },
  "compatible_versions": ["aios.scheduler-v1"],
  "migration_function_id": "migrate_scheduler_v1_to_v2",
  "validate_on_read": false,
  "tags": ["scheduler", "agent-config"]
}
```

### 8.8 ConfigWatchRequest

Sent by a consumer to subscribe to configuration updates for a specific scope.

```json
{
  "consumer_id": "agent:worker:instance-5",
  "consumer_type": "agent",
  "agent_type": "worker",
  "service": "agent-runtime",
  "deployment_id": "deploy-us-east-1-v42",
  "region": "us-east-1",
  "environment": "production",
  "tenant_id": "acme-corp",
  "scoped_paths": ["global.*", "agent.worker.*", "tenant.acme.*"],
  "last_known_version": "1738598400.41",
  "capabilities": {
    "supports_delta": true,
    "supports_websocket": false,
    "max_payload_bytes": 1048576
  }
}
```

### 8.9 ConfigWatchResponse

Sent by the Configuration System to subscribed consumers.

```json
{
  "version_id": "1738598400.42",
  "sequence_number": 99,
  "full_snapshot": false,
  "delta": [
    {
      "action": "upsert",
      "path": "scheduler.timeout_seconds",
      "new_value": 30,
      "value_type": "integer"
    },
    {
      "action": "delete",
      "path": "scheduler.legacy_field"
    }
  ],
  "checksum": "sha256:newchecksum",
  "pushed_at": "2026-02-03T12:00:01.000Z",
  "ttl_seconds": 30
}
```

---

## 9. Execution Flow

### 9.1 Configuration Loading and Initialization Flow

```
[System Start]
    │
    ▼
(1) Bootstrap Phase
    ├── Load minimal bootstrap config (from a well-known path: /etc/aios/bootstrap.yaml or compiled-in defaults)
    ├── Initialize the Log system (can't log without config — bootstrap config is used)
    ├── Initialize Metrics registry
    │
    ▼
(2) Source Discovery Phase
    ├── Discover all configured sources from bootstrap config
    │   (Order: CLI → Env Vars → Config Server → Files)
    ├── Connect to Config Server (etcd/Consul):
    │   ├── Establish connection
    │   ├── Authenticate
    │   └── If fails: fall back to local files only + log warning
    ├── Connect to Vault:
    │   ├── Establish connection
    │   ├── Authenticate (kubernetes auth, token, or IAM role)
    │   └── If fails: continue with degraded mode (secret references will fail)
    ├── Initialize File System Watchers
    │   └── Register file paths for FSNotify
    │
    ▼
(3) Config Loading Phase
    ├── Read all sources in parallel:
    │   ├── CLI args → List of (key, value)
    │   ├── Env Vars → List of (key, value)
    │   ├── Config Server → Namespace tree of config values
    │   └── File System → Parse all config files
    ├── For each value, record its source priority
    │
    ▼
(4) Template Resolution Phase
    ├── Execute template substitutions in all values
    ├── Resolve secret references by fetching from Vault
    ├── Resolve cross-key references (${path.to.key})
    ├── If any template fails to parse: report error + fail startup (config is invalid)
    │
    ▼
(5) Merge Phase
    ├── Start with the defaults layer (worst priority: 0)
    ├── Apply environment layer (priority: 1)
    ├── Apply region layer (priority: 2)
    ├── Apply service layer (priority: 3)
    ├── Apply instance/CLI layer (priority: 4)
    ├── Merge Strategy per key:
    │   ├── Scalar: highest priority wins
    │   ├── Nested Object: deep merge
    │   ├── Array: append by default, but replace if annotated
    │   └── null value: remove key (effectively)
    │
    ▼
(6) Validation Phase
    ├── Pull schema for each configuration path prefix
    ├── Validate merged config against schema:
    │   ├── Type check
    │   ├── Range check (min, max, enum)
    │   ├── Required field check
    │   ├── Conditional validation (if/then/else in JSON Schema)
    │   ├── Cross-field consistency checks
    │   └── Sanity checks (port not in use, path exists, etc.)
    ├── If validation fails:
    │   ├── Log error with detailed validation failure messages
    │   ├── Emit metrics (config.validation.error)
    │   ├── Fail startup: do not accept the configuration
    │   └── Provide clear error report to operator
    │
    ▼
(7) Versioning Phase
    ├── Generate new version ID
    ├── Compute checksum of full config tree
    ├── Store version record in PostgreSQL:
    ├── Set version as "active"
    │
    ▼
(8) Cache Phase
    ├── Store full configuration snapshot in:
    │   ├── Local in-memory cache (fast, primary)
    │   └── Redis distributed cache (backup, cross-instance)
    ├── Set TTL on cache entries (configurable)
    │
    ▼
(9) Distribution Phase
    ├── Notify all gRPC streaming subscribers:
    │   ├── Send full snapshot to new subscribers
    │   └── Send delta to existing subscribers
    ├── Publish config.change event to Kafka
    ├── Update WebSocket subscribers
    │
    ▼
(10) Monitoring Phase
    ├── Start drift detection loop
    ├── Start config server watch loop
    ├── Start file watch loop
    └── System ready: "AIOS Configuration System initialized"
```

### 9.2 Dynamic Configuration Update Flow

```
[Config Change Detected]
    ├── From File Watcher: file changed
    ├── From Config Server Watch: key changed
    ├── From Vault: secret rotated
    ├── From Management API: manual update
    └── From GitOps: new commit detected
    │
    ▼
(1) Change Ingestion
    ├── Receive the change as a set of (key, new_value, metadata)
    ├── Record the source of the change
    ├── Perform preliminary validation (type, format)
    ├── If preliminary validation fails:
    │   └── Reject the change, log error, return error to caller
    │
    ▼
(2) Access Control Check
    ├── Resolve the actor (user or system) making the change
    ├── Fetch permissions for the actor on the affected config path
    ├── Evaluate RBAC and ABAC policies
    ├── If actor has insufficient permissions:
    │   └── Reject the change, audit log failure, return error
    │
    ▼
(3) Approval Check
    ├── Check if the config path requires approval
    ├── If required:
    │   ├── Create a ChangeRequest (CR) record
    │   ├── Route CR to configured approvers
    │   ├── Wait for approval (async, polling)
    │   └── If rejected or expired: stop here
    │
    ▼
(4) Re-merge and Re-validate
    ├── Re-merge the configuration with the new value(s)
    ├── Re-run schema validation on the entire affected subtree
    ├── Re-run sanity checks (cross-field, cross-service)
    ├── If validation fails:
    │   └── Reject the change, restore previous value
    │
    ▼
(5) Version and Commit
    ├── Generate new version ID
    ├── Compute diff against the previous version
    ├── Store the new version in the version history
    ├── Update the checksum
    │
    ▼
(6) Cache Update
    ├── Update local in-memory cache with new values
    ├── Update Redis cache (publish/subscribe pattern or direct write)
    │
    ▼
(7) Distribution
    ├── Push config change deltas to all subscribers:
    │   ├── gRPC streams: send delta message
    │   ├── Kafka: publish change event
    │   ├── WebSocket: push update
    │   └── Polling subscribers: update on next poll
    ├── Wait for acknowledgments (configurable timeout: default 5s)
    │
    ▼
(8) Post-Apply Validation
    ├── Wait for drift detection cycle to confirm the change was applied
    ├── Run a health check on affected components
    ├── If the change causes issues:
    │   └── Automatic rollback to previous version
    │
    ▼
(9) Audit and Monitor
    ├── Record the change in the audit log
    ├── Increment metrics:
    │   ├── aios_config_reload_total (with label status=success/failure)
    │   ├── aios_config_validation_errors
    │   └── aios_config_staleness_seconds
    ├── Notify operators (optional, based on risk level)
    └── Done
```

### 9.3 Configuration Rollback Flow

```
[Rollback Request]
    │
    ▼
(1) Authentication and Authorization
    ├── Verify the requesting actor has "rollback" permission
    │   on the affected config path
    │
    ▼
(2) Fetch the target version
    ├── Check that the target version exists in the version history
    ├── Verify that the target version is a valid, non-corrupt snapshot
    │
    ▼
(3) Compute the rollback change
    ├── Diff the current (version N) vs target (version M)
    ├── The "rollback" is essentially applying the inverse change
    │
    ▼
(4) Same as a standard config update (steps 2-9 of Hot Update Flow)
    ├── Access control, approval, re-merge, re-validate, version,
    │   cache update, distribution, post-apply validation, audit
    │
    ▼
(5) Quick Rollback for Emergencies
    ├── If the rollback is marked as "emergency":
    │   └── Bypass approval workflow (but still audit log it)
    │   └── Skip post-apply wait time
    │   └── Push update immediately to all subscribers
    │
    ▼
(6) Record
    ├── The rollback itself creates a new version (N+1)
    ├── Version N+1 is a clone of version M
    ├── Metadata includes: rollback: true, rolled_back_from: N
    │
    ▼
(7) Alert operators:
    ├── "Rollback to version 1738598300.15 completed. Previous version was 1738598400.42."
    └── Done
```

### 9.4 Configuration Export/Import/Migration Flow

```
[Export Configuration]
    │
    ▼
(1) User calls Export API with:
    ├── scope (environment, region, service, or full)
    ├── format (yaml, json, or protobuf)
    └── version (optional: export a specific version)
    │
    ▼
(2) Configuration Engine fetches the requested scope from the version store
    │
    ▼
(3) Sensitive values are:
    ├── Replaced with placeholder values or encrypted with backup key
    └── Annotated with `[ENCRYPTED]` metadata marker
    │
    ▼
(4) Output file is generated:
    ├── File header: AIOS Config Export, date, source, version
    ├── Full config tree
    ├── Metadata (version, actor, timestamps)
    └── Checksum of the export payload
    │
    ▼
(5) File is returned to user or uploaded to S3/GCS/Blob storage

[Import Configuration]
    │
    ▼
(1) User provides an import file + target scope
    │
    ▼
(2) Validate the import file:
    ├── Checksum validation
    ├── Schema validation
    ├── Sanity checks (are the values valid?)
    │
    ▼
(3) If the import includes encrypted values, decrypt with backup key
    │
    ▼
(4) Apply as a normal configuration update (steps 1-9 of Hot Update Flow)
    ├── Note: this may create many ChangeRequests if the diff is large
    └── Warning: large imports should be done during maintenance windows
    │
    ▼
(5) Record in audit log: "Configuration imported from file X by user Y"
    └── Done

[Migration Between Environments]
    │
    ▼
(1) Export config from the source environment (e.g., staging)
    │
    ▼
(2) Transform the export:
    ├── Replace environment-specific values (e.g., URLs, ports)
    ├── Replace secret references (e.g., update vault paths)
    └── Anonymize or remove sensitive data for lower environments
    │
    ▼
(3) Validate the transformed config against the target environment's schemas
    │
    ▼
(4) Import into the target environment (e.g., production)
    └── Done
```

---

## 10. State Management

### 10.1 Engine States

The Configuration Engine runs as a deterministic state machine:

```
[INIT] ──→ [BOOTSTRAPPING] ──→ [LOADING] ──→ [MERGING] ──→ [VALIDATING]
                              │                                │
                              │                                ▼
                              │                          [VALIDATION_FAILED]
                              │                                │
                              │                                ▼
                              │                            [SHUTDOWN] (critical failure = OS dies)
                              │
                              ▼
                        [INIT_FAILED] ──→ [SHUTDOWN]
                        
[VALIDATING] ──→ (passed) ──→ [VERSIONING] ──→ [CACHING] ──→ [ACTIVE]
                                                                  │
                                                                  ├──→ (config change detected) → [RELOADING]
                                                                  ├── (secret rotation) → [SECRET_REFRESH]
                                                                  ├── (drift detected) → [DRIFT_ALERT]
                                                                  └── (operator command) → [MANAGEMENT]

[ACTIVE] ──→ [HEALTHY] / [DEGRADED] / [UNHEALTHY]
             │               │               │
             │               ▼               │
             │         [DEGRADED]            │
             │          Dependencies         │
             │          partially down      │
             │               │               │
             │               ▼               │
             │          [AUTO_RECOVER]           │
             │               │               │
             └───────────────┴───────────────┘
                                           │
                                           ▼
                                      [SHUTDOWN]
```

### 10.2 State Descriptions

| State | Description |
|-------|-------------|
| INIT | Engine is initialized but no config has been loaded. No consumers can connect. |
| BOOTSTRAPPING | Engine is reading the bootstrap config to determine sources. |
| LOADING | Engine is actively fetching config from all discovered sources. |
| MERGING | Engine is merging configuration from all sources per hierarchy. |
| VALIDATING | Engine is validating the merged config against all applicable schemas. |
| VALIDATION_FAILED | Validation errors were found. Engine cannot transition to ACTIVE. |
| VERSIONING | Engine is writing the version record for the new config. |
| CACHING | Engine is populating the config cache. |
| ACTIVE | Engine is running, serving configs, watching for changes, and distributing updates. |
| RELOADING | A config change was detected; engine is re-running load/merge/validate/version. |
| SECRET_REFRESH | Engine is refreshing cached secrets from the vault. |
| DRIFT_ALERT | Drift was detected; engine is logging and alerting. |
| DEGRADED | Engine is running but one or more upstream dependencies are unavailable (e.g., etcd down). |
| AUTO_RECOVER | Engine is attempting to reconnect to lost dependencies. |
| SHUTDOWN | Engine is performing graceful shutdown. |

### 10.3 Session / Watcher State

For each gRPC streaming watcher connection:

| State | Description |
|-------|-------------|
| CONNECTING | Initial handshake. |
| SUBSCRIBING | Processing the initial ConfigWatchRequest. |
| SUBSCRIBED | Stream is open, receiving deltas. |
| STALE | Consumer has not acknowledged updates for > 60s. |
| DISCONNECTED | Consumer dropped the connection. Stream will be cleaned up after timeout. |
| RECONNECTING | Consumer reconnecting with last_known_version. |

### 10.4 Secret Cache State

| State | Description |
|-------|-------------|
| FRESH | Secret is valid and within TTL. |
| RENEWING | Engine is renewing the secret from the vault. |
| STALE | Secret is past TTL but within the grace period. |
| EXPIRED | Secret is past the grace period. Returned as error for any read. |

---

## 11. Communication Protocols

### 11.1 gRPC (Primary API)

- **Protocol:** gRPC with Protocol Buffers (proto3).
- **Transport:** HTTP/2 with TLS (mTLS required for production).
- **Service Definition:**
  ```protobuf
  service ConfigurationService {
    // Get the current effective configuration for the caller's scope.
    rpc GetConfig(GetConfigRequest) returns (GetConfigResponse);
    
    // Watch for configuration changes (server-side streaming).
    rpc WatchConfig(WatchConfigRequest) returns (stream ConfigEvent);
    
    // Apply a configuration change.
    rpc ApplyConfig(ApplyConfigRequest) returns (ApplyConfigResponse);
    
    // Rollback to a previous version.
    rpc RollbackConfig(RollbackConfigRequest) returns (RollbackConfigResponse);
    
    // Get the diff between two versions.
    rpc DiffConfig(DiffConfigRequest) returns (DiffConfigResponse);
    
    // Get version history.
    rpc ListVersions(ListVersionsRequest) returns (ListVersionsResponse);
    
    // Get a specific version's full config snapshot.
    rpc GetVersion(GetVersionRequest) returns (GetVersionResponse);
    
    // Create a change request.
    rpc CreateChangeRequest(CreateChangeRequestReq) returns (ChangeRequest);
    
    // Approve or reject a change request.
    rpc ApproveChangeRequest(ApproveChangeRequestReq) returns (ChangeRequest);
    
    // Manage feature flags.
    rpc CreateFeatureFlag(CreateFeatureFlagReq) returns (FeatureFlag);
    rpc UpdateFeatureFlag(UpdateFeatureFlagReq) returns (FeatureFlag);
    rpc DeleteFeatureFlag(DeleteFeatureFlagReq) returns (Empty);
    rpc ListFeatureFlags(ListFeatureFlagsReq) returns (ListFeatureFlagsResp);
    rpc EvaluateFeatureFlag(EvaluateFeatureFlagReq) returns (EvaluateFeatureFlagResp);
    
    // Export configuration.
    rpc ExportConfig(ExportConfigReq) returns (ExportConfigResp);
    
    // Import configuration.
    rpc ImportConfig(ImportConfigReq) returns (ImportConfigResp);
    
    // Health check.
    rpc Health(HealthReq) returns (HealthResp);
  }
  ```

- **Authentication:** Each gRPC request includes a JWT in the `Authorization` metadata header (bearer token). The JWT includes the caller's identity (user ID or service account ID), roles, and expiration.
- **Timeouts:** 
  - Unary calls: 30s default.
  - Streaming calls: No server-side timeout; client is expected to keep the stream alive.
- **Interceptors:**
  - AuthN/AuthZ interceptor (validates JWT, checks permissions).
  - Logging interceptor (logs every request).
  - Metrics interceptor (records latency, request count, error count per RPC).
  - Rate-limiting interceptor (per-caller token bucket).

### 11.2 REST API (Secondary / Management API)

- **Base Path:** `/api/v1/config/`
- **Endpoints:**
  - `GET /api/v1/config/{scope}` — Get configuration (JSON output).
  - `PUT /api/v1/config/{scope}` — Apply configuration change.
  - `POST /api/v1/config/{scope}/rollback` — Rollback to version.
  - `GET /api/v1/config/versions` — List versions.
  - `GET /api/v1/config/versions/{version_id}` — Get version snapshot.
  - `POST /api/v1/config/export` — Export config (download as YAML/JSON).
  - `POST /api/v1/config/import` — Import config.
  - `GET /api/v1/config/diff` — Diff two versions.
  - `GET /api/v1/config/change-requests` — List change requests.
  - `POST /api/v1/config/change-requests` — Create change request.
  - `PUT /api/v1/config/change-requests/{cr_id}/approve` — Approve.
  - `PUT /api/v1/config/change-requests/{cr_id}/reject` — Reject.
  - `GET /api/v1/config/feature-flags` — List flags.
  - `POST /api/v1/config/feature-flags` — Create flag.
  - `PUT /api/v1/config/feature-flags/{flag_name}` — Update flag.
  - `DELETE /api/v1/config/feature-flags/{flag_name}` — Delete flag.
  - `POST /api/v1/config/feature-flags/{flag_name}/kill` — Kill switch.
  - `GET /api/v1/config/drift` — List active drifts.
  - `GET /api/v1/config/metrics` — Config system metrics (Prometheus format).

### 11.3 CLI (Operator Interface)

- **Interface:** AIOS CLI binary.
- **Commands:**
  ```
  aios config get [--scope <scope>] [--path <path>] [--format yaml|json]
  aios config set <key=value> [--scope <scope>] [--reason <reason>] [--ttl <seconds>]
  aios config apply <file.yaml> [--scope <scope>]
  aios config rollback --version <version_id> [--emergency]
  aios config versions [--limit <n>] [--since <timestamp>] [--path <filter>]
  aios config diff --v1 <version_id> --v2 <version_id> [--format unified|json|html]
  aios config export [--scope <scope>] [--version <version>] [--output <file>] [--format yaml|json|proto]
  aios config import <file> [--scope <scope>] [--dry-run]
  aios config cr create <change.yaml>
  aios config cr list
  aios config cr approve <cr_id> --reason "..."
  aios config cr reject <cr_id> --reason "..."
  aios config flag create <name> --default <value> --type boolean|int|string
  aios config flag enable <name> [--percentage <n>]
  aios config flag disable <name>
  aios config flag kill-switch <name>
  aios config drift list
  aios config drift acknowledge <drift_id>
  aios config health
  ```

### 11.4 Events (Event Bus)

The Configuration System publishes the following events to the AIOS Event Bus:

| Event Name | Payload | Trigger |
|-----------|---------|---------|
| `config.loading` | `{source, scope}` | Config loading started from a source. |
| `config.loaded` | `{source, keys_count, checksum}` | Config loading completed from a source. |
| `config.changed` | `{version_id, actor, diff_summary}` | A new configuration version was committed. |
| `config.applied` | `{consumer_id, version_id, status}` | A consumer successfully applied the change. |
| `config.apply_failed` | `{consumer_id, version_id, error}` | A consumer failed to apply the change. |
| `config.validation.failed` | `{version_id, errors[]}` | Validation failed for a config change. |
| `config.secret.rotated` | `{secret_path, vault_backend}` | A secret was rotated in the vault. |
| `config.secret.stale` | `{secret_path, staleness_seconds}` | A secret is stale (past TTL). |
| `config.drift.detected` | `{report_id, consumer_id, keys_count}` | Drift was detected for a consumer. |
| `config.drift.remediated` | `{report_id, consumer_id, success}` | Drift was auto-remediated. |
| `config.rollback.performed` | `{from_version, to_version, actor}` | A rollback was performed. |
| `config.cr.created` | `{cr_id, target_path, author}` | A change request was created. |
| `config.cr.approved` | `{cr_id, approver}` | A change request was approved. |
| `config.cr.rejected` | `{cr_id, approver}` | A change request was rejected. |
| `config.cr.expired` | `{cr_id}` | A change request expired. |
| `config.cr.applied` | `{cr_id, version_id}` | An approved CR was applied. |
| `config.source.connected` | `{source_type, endpoint}` | A config source connection established. |
| `config.source.disconnected` | `{source_type, endpoint, reason}` | A config source connection lost. |
| `config.health.changed` | `{status}` | The config system's health status changed. |

### 11.5 Queues

The Configuration System uses the following queues:

| Queue Name | Backend | Purpose | Consumer |
|-----------|---------|---------|----------|
| `aios.config.apply` | Kafka / NATS | Apply config changes asynchronously | Configuration Engine workers |
| `aios.config.validation` | Kafka / NATS | Offload validation to a pool of validators | Validator workers |
| `aios.config.distribution` | Kafka / NATS | Distribute config updates to non-streaming consumers | Distribution workers |
| `aios.config.drift-check` | Internal in-memory queue | Process drift detection results | Drift Detector |
| `aios.config.secret-refresh` | Internal in-memory queue | Process secret refresh/renewal | Secret Refresh workers |
| `aios.config.audit` | Kafka | Forward audit events to the audit log | Audit log consumer |
| `aios.config.alert` | Kafka | Forward alert events to the alerting system | Alert consumer |

---

## 12. Caching

### 12.1 Cache Layers

The Configuration System uses a two-layer cache:

#### Layer 1: Local In-Memory Cache

- **Type:** Concurrent map (e.g., Go's `sync.Map` + copy-on-write tree).
- **Scope:** Per-instance. The full configuration tree is stored locally for zero-latency reads.
- **Update Strategy:** On config change, a new copy of the config tree is built, and the pointer to the "current" config is atomically swapped. This ensures that readers always see a consistent snapshot without locking.
- **TTL:** No TTL (cache lives as long as the process lives; cache is invalidated via the atomic swap).
- **Eviction:** Not needed; the cache is replaced in toto on each config version change.
- **Size Limit:** Configurable (default: 256 MB). If the config tree exceeds this, a warning is logged and the cache is still populated (but the operator should optimize).

#### Layer 2: Distributed Cache (Redis / Memcached)

- **Type:** Redis Cluster or Memcached.
- **Purpose:** Allows multiple instances or services to share a cached copy of the config. If a new instance starts up, it can fetch the config from Redis instead of re-loading from all sources.
- **Key Format:** `aios:config:{scope_hash}:{version_id}`
- **Value:** The full configuration snapshot, serialized as JSON or Protobuf bytes.
- **TTL:** Configurable (default: 3600s). Refreshed on each config version change.
- **Eviction:** Redis eviction policy: `volatile-lru`. The config is set with a TTL; if memory pressure is high, older config versions may be evicted. The current version is always refreshed.
- **Read Strategy:** Read-through. On a cache miss, load from the version history store in PostgreSQL, populate the cache, and return.

### 12.2 Cache Invalidation Patterns

| Event | Invalidation Action |
|-------|-------------------|
| New config version applied | Atomic swap of local cache. Write new version to Redis. TTL is extended. |
| Secret TTL expires | Invalidate the specific secret key from both caches. Fetch new value from vault. Populate both caches. |
| Drift detected (config mismatched between cache and expected) | Invalidate local cache for the affected consumer. Force re-read from Redis or store. |
| Redis key evicted by LRU | Next read will miss cache → load from PostgreSQL → repopulate Redis. |
| Instance shutdown | Caches are lost (expected). Next startup will load from sources. |
| Network partition between instances | Redis remains; local cache is unaffected. When partition resolves, Redis stays as the source of truth. |

---

## 13. Memory

### 13.1 Memory Model

The configuration tree is stored as an immutable, hierarchical map. Once built, it is never modified; a new version creates a new tree.

- **Node Types:**
  - `MapNode`: A map of string → ConfigNode (for objects).
  - `SliceNode`: An ordered list of ConfigNode (for arrays).
  - `ScalarNode`: A scalar value (string, int, float, bool, null).
  - `SecretNode`: A scalar value flagged as sensitive (redacted in logs).

- **Memory Budget:**
  - The full config snapshot for a large deployment is budgeted to 128 MB uncompressed (but rarely exceeds 10 MB in practice).
  - Each version snapshot stored temporarily during merge phase may add up to 50 MB extra.
  - Secret cache: 16 MB budget (secrets are usually small).
  - Total process memory: 512 MB max for the Configuration System process.

- **Memory Optimization:**
  - Shared structure between versions: Versions are stored as diffs, not full copies. However, the cache stores full snapshots for performance.
  - String interning: Configuration keys are interned to avoid duplicate strings.
  - Value deduplication: If the same value appears in multiple keys (rare), only one copy is stored.
  - Memory-mapped files for large YAML/JSON config files: Use a memory-mapped file reader for large files to avoid loading the entire file into the heap.

### 13.2 Memory Leak Prevention

- All goroutines that watch config sources must have a context with a cancel function. On shutdown or source failure, the context is canceled, and the goroutine must exit.
- All gRPC stream goroutines must be tracked in a WaitGroup. On shutdown, the system waits for all streams to close gracefully.
- The in-memory config cache must release old versions when a new version is applied (old version pointer is released; GC will collect it).
- Redis cache keys have TTLs that are enforced by Redis itself.

---

## 14. Persistence

### 14.1 Database: PostgreSQL

| Table | Purpose |
|-------|---------|
| `config_versions` | Stores every version of the effective configuration. |
| `config_versions_diff` | Stores the diff from one version to the next. |
| `config_change_requests` | Stores all change requests. |
| `config_change_request_approvals` | Stores approval/rejection actions. |
| `config_feature_flags` | Stores feature flag definitions. |
| `config_feature_flag_audit` | Stores audit trail for feature flag changes. |
| `config_drift_reports` | Stores drift detection reports. |
| `config_secret_cache` | Stores metadata about cached secrets. |
| `config_validation_errors` | Stores validation error history. |
| `config_consumer_registrations` | Stores registered consumers (for gRPC watchers). |

### 14.2 Write-Ahead Log (WAL)

In addition to PostgreSQL, the Configuration Engine maintains a local WAL on disk:

- **Location:** `/var/lib/aios/config/wal/`
- **Format:** Append-only, each record is a Protobuf-encoded `WalEntry`:
  ```
  message WalEntry {
    int64 timestamp = 1;
    string version_id = 2;
    bytes diff_payload = 3;   // Protobuf-encoded diff
    string checksum = 4;      // SHA-256 of the full config
    string actor = 5;
  }
  ```
- **Purpose:** The WAL provides durability. Even if PostgreSQL is down, config changes applied to the in-memory cache are logged to disk. When PostgreSQL recovers, the WAL is replayed to catch up.
- **Rotation:** WAL files are rotated when they reach 512 MB or after 1 hour, whichever comes first.
- **Retention:** WAL files are retained for 7 days (or longer for compliance).

### 14.3 Snapshot Store (S3/GCS)

- **Frequency:** Full configuration snapshots are written to object storage every hour, or on each version change (if the change is "critical" or "high" risk).
- **Format:** Single `.json.gz` or `.yaml.gz` file.
- **Path:** `s3://aios-config-backups/{environment}/{cluster_id}/{date}/{version_id}.yaml.gz`
- **Retention:** Hourly snapshots for 30 days, daily snapshots for 1 year, monthly snapshots for 7 years.

---

## 15. Validation

### 15.1 Schema Validation Deep Dive

Validation is performed by the Schema Validator component. It supports three formats:

1. **JSON Schema (Draft-07):** Validates JSON and YAML configs. The schema is referenced from a schema registry. The validator supports:
   - `type`, `properties`, `required`, `additionalProperties`
   - `minimum`, `maximum`, `exclusiveMinimum`, `exclusiveMaximum`
   - `pattern`, `patternProperties`, `format` (`date-time`, `uri`, `email`, `ipv4`, `ipv6`)
   - `enum`, `const`
   - `oneOf`, `anyOf`, `allOf`, `not`
   - `if`, `then`, `else`
   - `$ref` (external references resolved from the schema registry)
   - Custom annotations: `x-aios-sensitive` (marks key as sensitive), `x-aios-drift-exclude`, `x-aios-requires-approval`

2. **Protobuf Validation (protoc-gen-validate):**
   - Uses the `buf` toolchain to generate validation code from `.proto` files.
   - Supports: `int32`, `int64`, `uint32`, `uint64`, `float`, `double`, `bool`, `string`, `bytes`, `enum`, `message`, `repeated`, `map`.
   - Well-known rules from `validate.proto`: `(validate.rules).string.min_len`, `(validate.rules).int32.gte`, `(validate.rules).message.required`, etc.

3. **Custom Validation Functions:**
   - Registered via a plugin mechanism. Each function receives the (path, value) and returns an error if validation fails.
   - Examples:
     - `port_available`: Checks that a TCP port is not already in use.
     - `file_exists`: checks that a file path exists on disk.
     - `url_reachable`: checks that a URL returns a 200 (HTTP).
     - `dependency_available`: checks that a required upstream service is healthy (via the service registry).
   - Custom functions are declared in the schema metadata: `x-aios-custom-validation: ["port_available"]`.

### 15.2 Validation Pipelines

Validation is applied in multiple phases:

| Phase | Timing | Scope | Action on Failure |
|-------|--------|-------|-------------------|
| Schema syntax validation | On config source read | Individual source | Reject source, log error, continue with other sources |
| Schema semantic validation | After merge | Full merged config | Reject config change, log error |
| Sanity check | After merge | Full merged config | Reject config change, log error, attempt fallback |
| Cross-field validation | After merge | Multiple related fields | Reject config change, log detailed explanation |
| Dependency check | After merge | External dependencies | Warning (not fatal); system continues with degraded config |
| Post-apply validation | After distribution | Consumer acknowledgment | Auto-rollback if configured |
| Periodic validation | Every 300s (configurable) | Current active config | Log warning if stale |

### 15.3 Validation Error Report

When validation fails, a detailed error report is generated:

```json
{
  "validation_id": "val-20260203-001",
  "timestamp": "2026-02-03T12:00:05Z",
  "config_version": "1738598400.42",
  "status": "failed",
  "errors": [
    {
      "path": "scheduler.timeout_seconds",
      "severity": "error",
      "code": "SCHEMA_VIOLATION",
      "message": "Value 500 exceeds maximum of 300.",
      "expected": {"maximum": 300},
      "actual": 500
    },
    {
      "path": "scheduler.policy",
      "severity": "error",
      "code": "NOT_IN_ENUM",
      "message": "Value 'random' is not one of the allowed values.",
      "expected": {"enum": ["round_robin", "weighted_round_robin", "least_busy"]},
      "actual": "random"
    }
  ],
  "warnings": [
    {
      "path": "agent.worker.ttl_seconds",
      "severity": "warning",
      "code": "DEVALUE_DEPRECATED",
      "message": "This config key is deprecated. Use 'agent.default.ttl_seconds' instead.",
      "deprecated_since": "v1.2.0"
    }
  ],
  "failed_components": ["scheduler"],
  "recommended_action": "Set 'scheduler.timeout_seconds' to 300 or less and 'scheduler.policy' to one of: round_robin, weighted_round_robin, least_busy."
}
```

---

## 16. Retry Logic

### 16.1 Retry Strategies

| Operation | Strategy | Initial Delay | Max Delay | Max Retries | Backoff Factor |
|-----------|----------|--------------|-----------|-------------|----------------|
| Config server connection | Exponential backoff + jitter | 100ms | 30s | Infinite (until success) | 2.0 |
| Vault connection | Exponential backoff + jitter | 200ms | 60s | Infinite (until success) | 2.0 |
| Secret fetch | Exponential backoff + jitter | 100ms | 10s | 5 | 2.0 |
| Config distribution (per consumer) | Exponential backoff + jitter | 500ms | 60s | 3 per change | 2.0 |
| gRPC call to consumer for health check | Linear backoff | 1s | 5s | 3 | N/A |
| PostgreSQL write (version store) | Exponential backoff + jitter | 100ms | 5s | 5 | 2.0 |
| Redis cache write | Exponential backoff + jitter | 50ms | 2s | 3 | 2.0 |
| GitOps operator pull | Exponential backoff + jitter | 10s | 300s | Infinite (until success) | 1.5 |
| Config export to S3/GCS | Exponential backoff + jitter | 1s | 30s | 5 | 2.0 |

### 16.2 Jitter

- Random jitter of +/- 20% of the current delay is added to every retry to prevent thundering herd.
- Formula: `delay = min(max_delay, base * (backoff_factor ^ attempt)) * (0.8 + 0.4 * random())`

### 16.3 Circuit Breakers

Each upstream connection (config server, vault, Redis, PostgreSQL) has a circuit breaker:

- **Closed:** Normal operation. Requests pass through.
- **Open:** After 5 consecutive failures within 60 seconds. Requests fail fast without attempting. Circuit stays open for 30s.
- **Half-Open:** After 30s, allow 1 request. If it succeeds → close. If it fails → open again for 60s.

---

## 17. Error Recovery

### 17.1 Error Classification

| Error Class | Example | Severity | Recovery |
|-------------|---------|----------|----------|
| Configuration validation error | Schema violation in new config | High | Block the change; keep previous config. |
| Config source connection loss | etcd connection dropped | High | Degraded: use cached config; attempt reconnection. |
| Vault connection loss | Vault unreachable | Critical | Degraded: use cached secrets with grace period; if expired, fail config reads. |
| Invalid config file | YAML syntax error | Medium | Reject that file; continue with other sources. |
| Secrets cache staleness | Secret past TTL | High | Block reads for that secret; alert immediately. |
| Distribution failure | gRPC push to consumer failed | Low | Retry; if persistent after 3 retries, mark consumer as stale. |
| PostgreSQL failure | Write to version store fails | Critical | Fall back to WAL; retry after reconnection. |
| Redis cache failure | Write to Redis fails | Low | Continue with only local cache; warn. |
| Drift detection | Config mismatch between cache and consumer | Medium | Alert; optionally auto-remediate. |
| Permission error | Unauthorized read attempt | Low (from system's perspective) | Deny access; audit log. |

### 17.2 Recovery Procedures

| Failure | Recovery Procedure |
|---------|--------------------|
| Config source connection lost | 1. Open circuit breaker. 2. Continue serving config from cache. 3. Log warning. 4. Attempt to reconnect: the retry strategy (exponential backoff + jitter, max 30s delay). 5. On reconnection, flush cache and re-read config. 6. Close circuit breaker. |
| Vault connection lost | 1. Continue serving cached secrets. 2. Start grace period timer (configurable, default: 2x secret TTL). 3. Reconnect with retry strategy. 4. If grace period expires before reconnection: mark secrets as expired; fail all secret reads; alert at CRITICAL level to on-call. 5. On reconnection, re-fetch all secrets, repopulate cache, clear expired state. |
| Config file change detected but invalid | 1. Validate the file. 2. If invalid: log error; revert to the previous valid config for that file; emit `config.validation.failed` event; do NOT apply the invalid change; wait for the next valid update. |
| Distribution failure to a consumer | 1. Wait for consumer to reconnect (or retry). 2. On reconnection, send the current full snapshot (not just the delta since it might have missed many changes). 3. If the consumer does not reconnect within 300s, mark as disconnected for cleanup. |
| Drift detected | 1. If `config.drift.auto_remediate` is enabled, push the expected config to the consumer. 2. If auto-remediation fails, escalate to operator with the drift report. 3. Drift is cleared when the consumer's config matches the expected config again. |
| Graceful shutdown | 1. Close all gRPC streams to consumers, notifying them of the shutdown. 2. Flush the WAL to disk. 3. Wait for all in-flight writes to complete. 4. Disconnect from upstreams. 5. Log "Config system shutting down gracefully." 6. Exit. |
| Crash recovery | 1. On restart, check the WAL for uncommitted entries. 2. Replay the WAL into the version store. 3. Determine the last committed version. 4. Load config as normal (standard init flow). 5. On "ACTIVE", publish `config.recovered` event. |

---

## 18. Security

### 18.1 Authentication

| Interface | Method | Details |
|-----------|--------|---------|
| gRPC API | JWT (Bearer token) | Token issued by AIOS Identity Service. Includes: `sub` (user ID or service account), `roles`, `tenant_id`, `exp` (expiry), `iat` (issued at). Tokens expire every 1 hour; refreshes handled by the client. |
| REST API | JWT (Bearer token) | Same as gRPC. |
| CLI | OAuth 2.0 Device Authorization Grant | Operator logs in via browser; CLI receives a refresh token. |
| GitOps Webhook | mTLS + Shared Secret | The Git push webhook includes a client certificate and an HMAC-signed payload. |
| Config Server (etcd/Consul) | mTLS | client and server certificates are verified. |
| Vault | Vault token / Kubernetes auth | Token is periodically renewed. For K8s: uses service account JWT to authenticate. |
| Between internal components | mTLS | All internal gRPC communication is over mTLS. |

### 18.2 Authorization

- **RBAC Roles:**
  - `config.admin`: Full access to all config operations, including deleting keys, managing schemas, and managing access policies.
  - `config.writer`: Can create and update configuration, create feature flags, approve change requests.
  - `config.reader`: Can read configuration values (including sensitive ones).
  - `config.operator`: Can read non-sensitive config and perform rollbacks (emergency only).
  - `config.auditor`: Can read version history, change requests, and audit logs (read-only).

- **ABAC Attributes:**
  - `request.ip`: The originating IP address.
  - `request.time`: The time of the request (allows time-based restrictions).
  - `request.method`: The HTTP/gRPC method.
  - `resource.path`: The config path being accessed.
  - `resource.sensitive`: Whether the resource is marked as sensitive.
  - `resource.risk_level`: The risk level of the resource.
  - `subject.team`: The team the actor belongs to.
  - `subject.tenant`: The tenant the actor belongs to.
  - `environment`: The deployment environment (dev/staging/prod).

- **Policy Evaluation Example (OPA Rego):**
  ```rego
  package config.authz
  
  # Deny by default
  default allow = false
  
  # Admins can do anything
  allow {
      input.subject.roles[_] == "config_admin"
  }
  
  # Writers can write non-sensitive config during business hours
  allow {
      input.subject.roles[_] == "config_writer"
      input.resource.risk_level != "critical"
      time.clock(input.request.time, [9, 0, 0])  # After 9 AM
      time.clock(input.request.time, [17, 0, 0])  # Before 5 PM
  }
  
  # Writers can write sensitive config only if approved
  allow {
      input.subject.roles[_] == "config_writer"
      input.resource.risk_level == "critical"
      input.request.approved == true
  }
  
  # Operators can rollback in emergencies
  allow {
      input.subject.roles[_] == "config_operator"
      input.request.action == "rollback"
      input.request.emergency == true
  }
  ```

### 18.3 Encryption

- **At Rest (Configuration Data):**
  - Sensitive keys, as annotated by `x-aios-sensitive` in the schema, are encrypted when written to the version store (PostgreSQL).
  - Algorithm: AES-256-GCM.
  - Envelope Encryption: Each config version has a unique data key (DEK) that encrypts its sensitive values. The DEK itself is encrypted by a master key (KEK) stored in the vault.
  - DEK format: 256-bit random key.
  - KEK format: 256-bit AES key stored in Vault's transit engine.

- **At Rest (Secrets Cache):**
  - Secrets fetched from the vault are re-encrypted with the data key before being stored in the cache (in memory).
  - The plaintext secret exists in memory only during the fetch-and-encrypt window (sub-millisecond).
  - The cache stores: `{key_path}: {encrypted_value (AES-256-GCM)}, {nonce}, {auth_tag}`.

- **At Rest (Config Files):**
  - Config files on disk can be encrypted with `sops` or `age`. The Configuration System detects encrypted files by the `.enc` suffix and decrypts them using a vault-stored key.
  - Format: SOPs format: YAML/JSON file with encrypted values; non-encrypted keys remain in plaintext.

- **In Transit:**
  - All gRPC, HTTP, and Kafka communication: TLS 1.3 required. mTLS for internal service-to-service.
  - Config server (etcd) communication: TLS 1.3.
  - Vault communication: TLS 1.3.
  - Redis communication: TLS 1.3 (with ACL + password).
  - PostgreSQL communication: TLS 1.3.

- **Key Rotation:**
  - Data keys: rotated every 24 hours (or on each data key compromise signal).
  - Master key (KEK): rotated manually via vault admin. Each rotation creates a new version of the key. Older versions remain usable for decryption of older DEK.
  - Root key: the key that encrypts the master key. Stored in an HSM (Hardware Security Module) with automatic rotation every 30 days.

### 18.4 Audit Logging

Every security-relevant action is audited:

| Action | Details logged |
|--------|----------------|
| Config read (sensitive key) | Actor, timestamp, key path, action (read), allowed (boolean) |
| Config read (non-sensitive) | Actor, timestamp, action (read), count of keys |
| Config write | Actor, timestamp, key path, old checksum, new checksum, old value (if non-secret), action (write) |
| Config delete | Actor, timestamp, key path, action (delete) |
| Config read (denied) | Actor, timestamp, key path, permission evaluated, reason for denial |
| Rollback | Actor, timestamp, from version, to version, action (rollback) |
| CR created | Actor, timestamp, CR ID, target path, action (cr.create) |
| CR approved | Actor, timestamp, CR ID, action (cr.approve) |
| CR rejected | Actor, timestamp, CR ID, reason, action (cr.reject) |
| Feature flag change | Actor, timestamp, flag name, old_value, new_value, action (flag.update) |
| Feature flag kill switch | Actor, timestamp, flag name, action (flag.kill_switch) |
| Encryption key rotation | Actor, timestamp, key type (DEK/KEK), old key version, new key version, action (key_rotation) |
| Login / Auth failure | Actor (if available), timestamp, IP address, method, action (auth.failure) |
| Source connection change | Source type, endpoint, old state, new state, action (source) |

Audit logs are sent to the AIOS Audit Log subsystem over Kafka and stored in PostgreSQL (for querying) and S3 (for long-term archival).

---

## 19. Monitoring

### 19.1 Key Metrics

All metrics are exposed via Prometheus at `/metrics` on a dedicated HTTP port (default 9101).

| Metric Name | Type | Labels | Description |
|------------|------|--------|-------------|
| `aios_config_current_version` | Gauge | `env`, `region`, `cluster` | The current version ID (as a gauge). |
| `aios_config_reload_total` | Counter | `source`, `status` (success/failure) | Total config reloads. |
| `aios_config_reload_duration_seconds` | Histogram | `source` | Duration of a full config reload. |
| `aios_config_validation_errors_total` | Counter | `schema_id`, `severity` (error/warning) | Validation errors. |
| `aios_config_validation_duration_seconds` | Histogram | `schema_id` | Duration of a single validation run. |
| `aios_config_staleness_seconds` | Gauge | `consumer_id`, `key_path` | How stale a config consumer's config is (0 = up to date). |
| `aios_config_access_total` | Counter | `action` (read/write/delete/rollback), `status` (allowed/denied) | Access attempts to config. |
| `aios_config_access_denied_total` | Counter | `action`, `reason` | Denied access attempts. |
| `aios_config_drift_total` | Counter | `consumer_type`, `severity` | Drift detections. |
| `aios_config_drift_active` | Gauge | `consumer_type` | Current active drifts. |
| `aios_config_change_request_total` | Counter | `status` (created/approved/rejected/expired/applied) | Change requests. |
| `aios_config_secret_rotations_total` | Counter | `vault_backend` | Secret rotations. |
| `aios_config_secret_stale_total` | Counter | `vault_backend` | Secrets that have become stale. |
| `aios_config_secret_fetch_duration_seconds` | Histogram | `vault_backend` | Duration of vault fetch. |
| `aios_config_cache_hits_total` | Counter | `cache_layer` (local/redis) | Cache hits. |
| `aios_config_cache_misses_total` | Counter | `cache_layer` (local/redis) | Cache misses. |
| `aios_config_cache_size_bytes` | Gauge | `cache_layer` | Cache size in bytes. |
| `aios_config_distribution_sent_total` | Counter | `protocol` (grpc/kafka/websocket/poll), `consumer_type` | Config updates sent. |
| `aios_config_distribution_ack_total` | Counter | `protocol`, `consumer_type`, `status` (success/failure) | Config update acknowledgments. |
| `aios_config_distribution_duration_seconds` | Histogram | `protocol`, `consumer_type` | Distribution latency. |
| `aios_config_watchers_active` | Gauge | `consumer_type` | Number of active watchers (streams). |
| `aios_config_upstream_connection_status` | Gauge | `target` (etcd/vault/redis/pgsql) | 1 = connected, 0 = disconnected. |

### 19.2 Logging

| Log Level | When | What to log |
|-----------|------|-------------|
| DEBUG | Config source loaded, watcher connected, metrics scraped | Source names, config keys count (not values), connection states. |
| INFO | Config version applied, CR created/approved/expired, distribution sent | Version ID, CR ID, consumer count, change summary. |
| WARN | Config source disconnected, config validation warning, drift detected, circuit breaker opened | Source type, validation path, drift details, circuit breaker target. |
| ERROR | Config change rejected (validation error), distribution failure, vault fetch failure, auth failure, PostgreSQL write failure | Path, value (redacted for secrets), consumer ID, error message, stack trace. |
| CRITICAL | All config sources down, vault unreachable after grace period, stale secrets with no renewal, cluster-wide config corruption | Full details, operator escalation required. |

### 19.3 Tracing

- **Tracing System:** OpenTelemetry (distributed tracing).
- **Traced Operations:**
  - Config change lifecycle: change detection → merge → validate → version → cache → distribute.
  - gRPC call: GetConfig, WatchConfig, ApplyConfig.
  - Vault fetch: resolve secret reference.
  - Config server watch: etcd/Consul event handling.
  - Validation: schema parse, validation execution.
  - Drift detection: comparison of running vs expected.
- **Span Attributes:**
  - `aios.config.version_id`: Version ID.
  - `aios.config.operation`: Config operation name.
  - `aios.config.source`: Source type.
  - `aios.config.consumer_id`: Consumer ID (if applicable).
  - `aios.config.duration_ms`: Operation duration.
  - `aios.config.result`: Result status.

### 19.4 Health Check

| Endpoint | Response | Checks |
|----------|----------|--------|
| `/healthz` (gRPC: `Health`) | `{"status": "healthy"}` | Process alive, all source connections OK, no stale secrets. |
| `/healthz` | `{"status": "degraded", "reasons": ["etcd disconnected"]}` | At least one source is functional; degraded sources listed. |
| `/healthz` | `{"status": "unhealthy", "reasons": ["all sources unreachable", "vault expired"]}` | No functional config sources or secrets unreachable. |
| `/readyz` (gRPC: `Ready`) | `{"status": "ready"}` | Config has been loaded and is being served (post-init). |
| `/livez` (gRPC: `Live`) | `{"status": "alive"}` | Process is running (does not reflect config state). |

### 19.5 Alerting

| Alert Name | Condition | Severity | Notification |
|-----------|-----------|----------|-------------|
| **ConfigSystemDown** | `up{job="aios-config"} == 0` | CRITICAL | PagerDuty, Slack, Email |
| **AllConfigSourcesUnreachable** | `aios_config_upstream_connection_status == 0` for all source targets | CRITICAL | PagerDuty, Slack, Email |
| **ConfigStaleTooLong** | `aios_config_staleness_seconds > 600` | HIGH | Slack, Email |
| **ConfigDriftPersistent** | `aios_config_drift_active > 5` for > 15min | HIGH | Slack, Email |
| **SecretsAboutToExpire** | `aios_config_secret_stale_total > 0` for > 5min | HIGH | Slack, Email |
| **HighValidationErrors** | `rate(aios_config_validation_errors_total[5m]) > 0.1` | WARNING | Slack |
| **HighConfigDistributionFails** | `rate(aios_config_distribution_ack_total{status="failure"}[5m]) > 0.05` | WARNING | Slack |
| **CacheMissRate** | `rate(aios_config_cache_misses_total[5m]) / rate(aios_config_cache_hits_total[5m]) > 0.2` | WARNING | Slack |
| **ConfigPermissionDenied** | `rate(aios_config_access_denied_total[5m]) > 0.01` | WARNING | Slack |
| **SecretsStaleCritical** | `aios_config_secret_stale_total{status="expired"} > 0` | CRITICAL | PagerDuty, Slack |

---

## 20. Scalability

### 20.1 Horizontal Scaling

The Configuration System is horizontally scalable: multiple instances of the config system can run simultaneously, with one elected leader.

- **Leader Election:** Uses etcd (or Consul) for leader election. The leader is responsible for:
  - Accepting config changes (ApplyConfig, CR operations).
  - Versioning and committing changes.
  - Distributing changes to subscribed consumers.
  - Drift detection and auto-remediation.
  - GitOps operator (pulling from Git repo).
- **Follower Role:** Follower instances:
  - Serve read-only config requests (GetConfig, GetVersion, DiffConfig, ListVersions, ListFeatureFlags).
  - Maintain their own in-memory cache of the config.
  - Subscribe to the leader's change events (via the event bus) to keep their caches up to date.
  - Can serve watcher streams (watch streams are sharded across all instances).
  - If the leader fails, one follower is automatically promoted.

### 20.2 Sharding

Config watchers are sharded across configuration instances:

- **Shard Key:** `hash(consumer_type + consumer_id) % num_instances`
- **Re-sharding:** When a new instance joins or leaves, active watcher connections are rebalanced. Rebalancing uses a consistent hash ring to minimize the number of connections that need to be moved.
- **Watcher Capacity per Instance:** Configurable (default: 10,000 concurrent gRPC streams per instance).

### 20.3 Performance Budgets

| Metric | Target | Degradation threshold |
|--------|--------|-----------------------|
| Config read latency (GetConfig) | < 5ms p99 | > 20ms |
| Config change application latency | < 100ms p99 (median) | > 1s |
| Config distribution latency (to one consumer) | < 200ms p99 | > 1s |
| Config distribution latency (to all consumers) | < 5s p99 | > 30s |
| Config watcher connection setup | < 100ms p99 | > 500ms |
| Secret fetch from vault | < 200ms p99 | > 1s |
| Config reload (full) | < 2s p99 | > 10s |
| Config version store write | < 50ms p99 | > 200ms |
| Drift detection cycle (per consumer group) | < 5s p99 | > 30s |

### 20.4 Resource Budgets

| Resource | Per Instance | Cluster Total |
|----------|--------------|---------------|
| CPU | 2 cores (burst to 4) | 8 cores |
| Memory | 1 GB (512 MB reserved) | 4 GB |
| Disk (WAL) | 10 GB | 40 GB |
| Network bandwidth | 100 Mbps | 400 Mbps |
| Max gRPC watchers | 10,000 | 40,000 |
| Max config versions stored | 1,000,000 | 4,000,000 |
| Max config sources | 10 | 40 |

---

## 21. Performance

### 21.1 Optimization Techniques

| Technique | Application | Impact |
|-----------|-------------|--------|
| **Immutable Config Tree** | Cache reads via atomic pointer swap | Zero-lock reads, only one lock for writes. |
| **Copy-on-Write Merge** | Merge phase creates a new config tree rather than mutating the old one | Safe concurrent reads during merge. |
| **Delta Distribution** | Only send changed keys, not the full snapshot | Reduces bandwidth ~100x. |
| **Selective Scope** | Only send config scoped to a consumer's interests | Reduces bandwidth per consumer 10-100x. |
| **Protobuf Serialization** | Use protobuf instead of JSON for internal distribution | 10x faster serialization, 5x smaller payloads. |
| **Cache Indexing** | Index config keys by path for O(1 lookups | Faster than iteration. |
| **Connection Pooling** | gRPC connections to etcd, vault, PostgreSQL, Redis | Reduced connection setup overhead. |
| **Batched Redis Writes** | Batch cache writes in the distribution layer when updating many keys | Increased throughput to Redis. |
| **Schema Pre-compilation** | Pre-compile JSON Schema into an evaluator at schema registration time | Faster validation at runtime. |
| **Lazy Secret Resolution** | Secrets are only fetched from vault when first read, not at config load | Faster startup. |

### 21.2 Benchmarks

| Operation | Payload Size | Concurrency | Avg Latency | p99 Latency | Throughput |
|-----------|-------------|-------------|-------------|-------------|------------|
| GetConfig | 50 KB (full config) | 100 | 5 ms | 15 ms | 10,000 req/s |
| GetConfig | 2 KB (scoped) | 1000 | 2 ms | 8 ms | 50,000 req/s |
| WatchConfig (connect + full snapshot) | 50 KB | 100 simultaneous | 10 ms initial | 30 ms | 100 connects/s |
| WatchConfig (delta) | 200 B | 100,000 watchers | 20 ms | 100 ms | 50,000 updates/s |
| ApplyConfig (single key) | 100 bytes | 100 | 30 ms | 80 ms | 500 ops/s |
| ApplyConfig (full config) | 50 KB | 10 | 500 ms | 800 ms | 10 ops/s |
| Validate (full config against schema) | 50 KB, 200 fields | 10 | 20 ms | 50 ms | 500 ops/s |
| Secret fetch (from vault) | 1 KB | 100 | 100 ms | 300 ms | 200 req/s |

---

## 22. Testing Strategy

### 22.1 Unit Tests

- **Scope:** All internal components individually.
- **Frameworks:** Go's `testing` package, `testify/assert`, `testify/mock`.
- **Coverage Target:** 90%+ lines, 85%+ branches.
- **Key Test Cases:**
  - Merge engine with all combinations of deep/shallow/replace/omit.
  - Template engine with all built-in functions and edge cases.
  - Schema validation: valid config passes, invalid config fails with correct error.
  - Access control: all RBAC/ABAC/ReBAC scenarios.
  - Feature flag evaluation: hash-based rollout, segmentation, kill switch.
  - Diff engine: structural and semantic diffs.
  - Caching: cache hit/miss, TTL expiration, invalidation.
  - State machine: all state transitions.
  - Version manager: version generation, version ordering, version conflict detection.

### 22.2 Integration Tests

- **Scope:** Multi-component interactions.
- **Mock Dependencies:** etcd (embedded), Vault (Dev server), PostgreSQL (test container), Redis (test container).
- **Key Test Cases:**
  - Full config lifecycle: bootstrap → load → merge → validate → version → cache → distribute.
  - Config hot reload from all sources (file, env var, etcd, vault).
  - Config rollback and version history retrieval.
  - Config change approval: CR creation → approval workflow → apply.
  - Config drift: detection, reporting, and auto-remediation.
  - Feature flag rollout: gradual rollout, segmentation, kill switch.
  - Export and import: round-trip export → import → validate equivalence.
  - GitOps workflow: PR merge → GitOps operator applies change.
  - Secret resolution and automatic rotation.
  - Config encryption at rest and in transit tests.

### 22.3 E2E Tests

- **Scope:** Full AIOS deployment with actual config system + agents.
- **Orchestration:** Start a test cluster with 2 config instances, 1 etcd, 1 vault, 1 consul, PostgreSQL, Redis, and 5 simulated agents.
- **Key Test Cases:**
  - Change config in etcd → agents pick up the change without restart.
  - Rotate a secret in vault → config system picks up the new secret → agents use the new secret.
  - Disconnect etcd → config system degrades → reconnect etcd → config system recovers.
  - Apply a config that fails validation → config system rejects it and keeps the old config.
  - Rollback to a previous version → agents revert to the old config.
  - Create a change request → approve → config change applied → audit log has the record.
  - Simulate leader election: kill leader → follower becomes leader → no disruption to agents.
  - Export config → delete config → import config → system recovers.

### 22.4 Performance Tests

- **Scope:** Load testing the config system under heavy concurrency.
- **Tools:** k6 (for API), ghz (for gRPC), custom agent simulators.
- **Key Test Cases:**
  - 10,000 concurrent gRPC watchers receiving config updates.
  - 100,000 config keys across all sources.
  - 100 Hz config change rate.
  - 1 GB config export.
  - 1000 concurrent vault secret fetches.
  - 1000 concurrent schema validation calls.
  - Config server watch reconnection under heavy load (1000 etcd watch events/s).

### 22.5 Chaos Tests

- **Scope:** Resilience and fault tolerance.
- **Tools:** AWS Fault Injection Simulator, Gremlin, Litmus.
- **Key Test Cases:**
  - Kill etcd process → config system degrades → etcd restarts → system recovers.
  - Kill vault process → secrets become stale → vault restarts → secrets refreshed.
  - Kill PostgreSQL → config system uses WAL → PGSQL restarts → WAL replayed.
  - Kill Redis → cache misses → PGSQL fallback → Redis restarts → cache repopulated.
  - Network partition between config instances → leader re-elected.
  - Network partition between config and all agents → agents use cached config.
  - Power failure simulation (kill config process) → restart → WAL recovery.
  - High latency injection (100ms delay on all external calls) → deployment degrades but does not crash.

### 22.6 Security Tests

- **Scope:** Config system security.
- **Tools:** OWASP ZAP, Burp Suite, custom fuzzers.
- **Key Test Cases:**
  - JWT bypass: attempt to access API with expired, malformed, or missing JWT.
  - RBAC bypass: try to write config with a reader role.
  - Secret leakage: ensure secrets are never in logs, stack traces, error messages.
  - Schema injection: attempt to inject fields not in schema.
  - Path traversal: attempt to read config paths outside of access scope.
  - Rate limiting: ensure rate limiting prevents abuse.
  - TLS downgrade: ensure the system rejects non-TLS connections.
  - Audit log completeness: run a series of operations and confirm audit logs are complete.

---

## 23. Risk Analysis

| Risk ID | Risk Description | Likelihood | Impact | Mitigation |
|---------|-----------------|------------|--------|------------|
| R01 | Config source (etcd/Consul) goes down | Medium | High (no new config changes, but cached config continues to serve) | Multiple config sources (files + etcd); circuit breaker; degraded mode; alerts. |
| R02 | Vault goes down | Medium | Critical (secrets become stale, agents may fail) | Grace period for stale secrets; vault HA with multiple replicas; alert immediately. |
| R03 | PostgreSQL goes down | Low | High (version history unavailable, CR workflow impacted) | WAL replay on restart; degraded mode: cache-only (no new versions). |
| R04 | Redis goes down | Low | Medium (cache misses cause increased PGSQL load) | Fallback to local cache; alerts when Redis connection drops. |
| R05 | Corrupt config file pushed to GitOps repo | Medium | Medium (bad config validated and rejected; no production impact if validation works) | Schema validation in CI/CD prevents invalid config from being applied. |
| R06 | Feature flag kill switch used incorrectly | Low | High (turning off a critical flag unexpectedly) | Two-person approval for kill switch; audit log; emergency override only in CRITICAL cases. |
| R07 | Accidental rollback to the wrong version | Low | Medium (agents get old config) | Rollback preview: diff shown before confirm; confirm dialog; rollback itself versioned. |
| R08 | Denial of Service against config API | Medium | Medium (legitimate config changes delayed) | Rate limiting; IP-based blocking; WAF in front of REST API. |
| R09 | Secret leaked via log or error message | Low | Critical | All secret references are redacted in logs; WAL does not log secret values. |
| R10 | Config change applied before approval workflow completes (race condition) | Low | Critical | CR-based workflow is async; CR must be in approved state before apply is queued. |
| R11 | Config distribution takes too long (config inconsistency across agents) | Medium | Medium (agents running different configs for a short time) | Add a "wait for all ACKs" timeout; after timeout, alert; eventually consistent design accepted. |
| R12 | Schema incompatible with previous version (schema migration failure) | Low | High (some components cannot read the new config) | Schema compatibility checks; migration function required; alert if migration fails. |
| R13 | GitOps repository inadvertently deleted | Low | High | Git repository is backed up; GitOps operator can be repointed to a new repo. |
| R14 | Configuration change creates a deadlock (mutual dependency) | Low | High (no config can be applied after the deadlock) | Dependency graphs: detect cycles before applying; reject changes that introduce cycles. |

---

## 24. Failure Scenarios

### 24.1 Scenario: etcd Cluster Failure

**Symptoms:**
- Config server watcher metrics show `aios_config_upstream_connection_status{target="etcd"} == 0`.
- `Config source disconnected` event emitted.
- WARN log: "etcd connection lost, will retry."

**Impact:**
- Dynamic config changes from etcd are paused.
- File-based config (if configured) and local cache continue to work.
- Secrets vault integration (separate from etcd) continues.

**Resolution:**
1. Operator investigates etcd cluster health.
2. etcd cluster restored.
3. Config watcher reconnects (exponential backoff).
4. Config watcher does a full re-sync of `/aios/config/` namespace.
5. Config system picks up any changes that were missed.
6. Cache is updated with new data.
7. System returns to healthy state.

**Post-Mortem Action:**
- Consider adding a secondary config server (e.g., Consul) for redundancy.
- Shorten reconnection backoff max delay to 15s.

### 24.2 Scenario: Vault Unreachable

**Symptoms:**
- `aios_config_upstream_connection_status{target="vault"} == 0`.
- CRITICAL alert if grace period expires.

**Impact:**
- Cached secrets continue to work within grace period (2x TTL).
- After grace period expires, all config reads that require a secret fail.
- Agents that depend on those secrets cannot start or connect.

**Resolution:**
1. Operator investigates vault cluster.
2. Vault restored.
3. Config system reconnects.
4. All secrets are re-fetched.
5. Cache is repopulated.
6. "Stale" state is cleared.

**Fallback:**
- If the grace period is about to expire and vault is still unreachable, an emergency operator can manually push static secrets via the config API (with elevated permissions and audit log).

### 24.3 Scenario 3: Config Change Causes an Outage

**Symptoms:**
- After applying a config change, multiple components fail their health checks.
- Alerts: `ConfigStaleTooLong`, `ConfigDriftPersistent`, agent failures.

**Detection:**
- Post-apply validation detects that components are not healthy within 30s of the change.

**Immediate Action:**
1. Automatic rollback to the previous version (emergency rollback).
2. Distribution layer pushes the rollback to all consumers.

**Post-Rollback Actions:**
1. Operator investigates the root cause of the failed change.
2. The change is analyzed: Was it a validation gap? Was the schema insufficient?
3. A fix is applied to the schema or validation logic to prevent this change from passing validation in the future.
4. The change can be retried after the fix.

### 24.4 Scenario: Config Drift on an Agent

**Symptoms:**
- The agent is running a config version that doesn't match the expected version.
- Metric: `aios_config_drift_total` incremented.

**Cause:**
- The agent missed a config update (network issue, agent restarted, agent crashed).
- The agent's local cache was corrupted.
- The agent overrode a config value locally (not allowed but possible).

**Auto-Remediation:**
1. If `config.drift.auto_remediate` is enabled, the config system pushes the expected config to the agent via the gRPC stream.
2. If the auto-remediation succeeds, the drift clears.
3. If the auto-remediation fails (e.g., agent not reachable), the drift persists and an alert is sent.

**Operator Remediation:**
1. Operator reviews the drift report to understand the difference.
2. Operator can manually push the config to the agent via the CLI (`aios config set` with the agent's scope).
3. Operator can restart the agent (last resort).

### 24.5 Scenario 5: Config Source Data Corruption

**Symptoms:**
- The config file is corrupted (binary data, truncated, or malformed YAML/JSON).
- YAML/JSON parsing fails.
- Validation fails.

**Impact:**
- The config file is rejected.
- If this is the only source, the system falls back to defaults (or fails to start).
- If this is one of multiple sources, the other sources take over.

**Recovery:**
1. File watcher logs error: "Unable to parse config file X. Skipping file."
2. Config system continues with the last known good state from that file (the previous version is retained in cache).
3. Operator is alerted.
4. Operator restores the correct file (from GitOps repo or backup).
5. File watcher detects the new valid file, loads it, and applies it.

---

## 25. Operational Limits

### 25.1 Hard Limits

| Limit | Value | Consequence if Exceeded |
|-------|-------|------------------------|
| Max config keys per deployment | 500,000 | Performance degradation (slow merge/distribution). |
| Max config key length | 2048 characters | Truncation; rejected by validation. |
| Max config value size | 1 MB per key | Rejected by validation (unless overridden for binary configs). |
| Max config file size | 50 MB | Rejected; must use external config source. |
| Max change request TTL | 72 hours | CR expires. |
| Max change request approvers | 10 | Remaining approvers are not required (the required_approvals field caps it). |
| Max gRPC watchers per instance | 10,000 | New watcher connections are rejected. |
| Max config versions stored | 1,000,000 | Old versions are archived to S3. |
| Max config change rate | 100 Hz | Changes beyond 100 Hz are queued with bounded queue (throttled). |
| Max template recursion depth | 20 | Template evaluation fails with a circular reference error. |
| Max rollout percentage | 100% | Cannot exceed 100%. |
| Max feature flags per deployment | 10,000 | Performance degradation in flag evaluation. |
| Max segments per feature flag | 100 | Additional segments are ignored. |

### 25.2 Recommended Operating Ranges

| Metric | Recommended Range |
|--------|-------------------|
| Config keys per deployment | < 50,000 |
| Config value size | < 64 KB |
| Config change rate | < 10 per second |
| gRPC watchers per instance | < 5,000 |
| Feature flags per deployment | < 1,000 |
| Concurrent change requests | < 100 |

---

## 26. Maintenance

### 26.1 Routine Operations

| Operation | Frequency | Description |
|-----------|-----------|-------------|
| Version archive | Weekly | Archive versions older than 30 days to S3. |
| WAL rotation | Hourly | Force WAL rotation if > 512 MB. |
| Secret key rotation | Every 90 days | Rotate KEK and DEK. |
| PostgreSQL vacuum | Weekly | `VACUUM ANALYZE` on config tables. |
| Docker image update | Monthly | Apply security updates to the config system container. |
| Schema audit | Monthly | Review schemas for required updates. |
| Permission audit | Monthly | Review access control policies. |
| Config backup | Daily | Export config to S3/GCS. |
| Drift report review | Weekly | Review drift reports, investigate patterns. |

### 25.3 Upgrades

- **Rolling Upgrade:** Config system instances are upgraded one at a time.
- **Backward Compatibility:** Each new version of the config system remains compatible with existing config files, schemas, and APIs for at least 2 minor versions.
- **Migration Path:** If a config schema version changes, a migration function is provided. The config engine automatically applies the migration when loading the config.
- **Upgrade Procedure:**
  1. Deploy new config system image to staging environment.
  2. Run smoke tests in staging (config change, rollback, drift detection).
  3. Deploy to one production instance (followers first, then leader).
  4. Monitor metrics for 10 minutes.
  5. Continue rolling out to remaining production instances.
  6. If issues are detected: roll back to the previous version.

---

## 27. Future Improvements

| Improvement | Description | Priority | Target Version |
|-------------|-------------|----------|----------------|
| AI-powered config suggestions | Use AI to analyze config changes and suggest optimal values based on history and patterns. | Medium | 2.0 |
| Config dependency graph visualization | Generate a visual graph of config dependencies to help operators understand the blast radius of changes. | Medium | 2.0 |
| Config change prediction | Predict the impact of a config change on system performance before it is applied. | High | 2.1 |
| Automatic config optimization | Allow the system to automatically tune configuration based on runtime metrics (e.g., adjust timeouts based on observed latency). | High | 2.1 |
| Multi-cluster config management | Manage configuration across multiple AIOS clusters from a single pane of glass. | Medium | 2.2 |
| Config conflict detection | Detect when two config changes conflict (e.g., same key changed by different actors at the same time). | High | 2.0 |
| Rollback quarantine | After a rollback, prevent the same config from being applied for a configurable quarantine period. | Low | 2.0 |
| Config A/B testing UI | A web UI for creating and monitoring A/B tests of config values. | Medium | 2.2 |
| Natural language config change | Allow users to describe the desired change in natural language and have it converted to a config change. | Low | 3.0 |
| Config rollback automation | Automatically roll back if the system detects performance degradation after a config change. | High | 2.1 |

---

## 28. Examples

### Example 1: Enabling a Feature Flag via Config

**Operator CLI:**
```bash
aios config flag create new_scheduler --default false --description "New scheduling algorithm"
aios config flag enable new_scheduler --percentage 10
```

**What happens internally:**
1. CLI validates the flag name is unique.
2. The flag is stored in `config_feature_flags` and `config_feature_flag_audit`.
3. The Configuration Engine triggers a config change for the `feature_flags` path.
4. The merge engine picks up the new flag and validates it (no schema violation).
5. A new config version is created.
6. The distribution layer pushes the updated `feature_flags.new_scheduler` to all subscribed agents.
7. Each agent evaluates the flag locally: `hash(agent_id, "new_scheduler") % 100 < 10` — if true, the new algorithm is used.

### Example 2: Resolving a Secret Reference

**Config file:**
```yaml
database:
  host: db.acme.com
  port: 5432
  password: vault://kv/data/database#password
```

**What happens internally:**
1. The file watcher picks up the config file.
2. The merge engine includes this value in the merged config.
3. The template engine detects the `vault://` prefix and calls the Vault Watcher.
4. The Vault Watcher authenticates to the vault and fetches `kv/data/database`.
5. The data is decrypted by the vault, and the `password` field is returned.
6. The template engine stores the result in the config tree, replacing the `vault://` reference with the actual password.
7. The config tree has `database.password` = actual password (plaintext in memory for < 1ms).
8. Immediately after merge, the config tree is encrypted: the `database.password` value is encrypted with the data key and stored in the config cache as `AES-256-GCM(actual_password)`.
9. The encrypted value is decrypted on the fly when an authorized consumer reads it.

### Example 3: Change Request with Approval

**Operator (Alice):**
```bash
aios config set security.tls_min_version=1.3 --reason "Upgrade TLS minimum to 1.3 for PCI compliance" --risk-level critical
```

**What happens:**
1. The change is captured as a ChangeRequest (CR).
2. The access control system checks: Alice has `config.writer` role. The path `security.*` is high risk. The change requires approval.
3. A CR is created: target path = `security.tls_min_version`, new value = `1.3`, risk = `critical`.
4. Approvers are selected based on policy (for `security.*` paths, the approver is the security team, Bob).
5. Bob receives a Slack notification: "Alice requests to change `security.tls_min_version` to `1.3` (critical). Approve or review."
6. Bob runs `aios config diff` to see the exact change.
7. Bob approves: `aios config cr approve <cr_id> --reason "Agreed. TLS 1.3 is req'd"`
8. The CR is marked as "approved".
9. The config change is now applied: the merge, validate, version, cache, distribute cycle runs.
10. Alice and Bob receive a confirmation: "Config change applied. Version: 1738598400.43."

---

## 29. Acceptance Criteria

| ID | Criterion | Verification Method |
|----|----------|-------------------|
| AC-01 | The Configuration Engine shall start, load configuration from at least one source (file, env, CLI, or config server), and make it available to consumers within 5 seconds. | Unit test, integration test, E2E test. |
| AC-02 | The configuration hierarchy shall be enforced: instance overrides service, service overrides region, region overrides environment, environment overrides defaults. | Unit test (merge engine). |
| AC-03 | The system shall validate configuration values against their JSON Schema or Protobuf schema before applying. Invalid configurations shall be rejected with a clear error message. | Unit test, integration test. |
| AC-04 | Dynamic configuration changes shall be applied without restarting the config system or any consumer. | E2E test: change config, observe change in agents without restart. |
| AC-05 | All configuration changes shall be versioned. Version numbers shall be monotonically increasing and globally unique. | Integration test. |
| AC-06 | Rollback to any previous version shall be possible within 5 seconds. | E2E test. |
| AC-07 | Secret references in configuration (e.g., `vault://path#key`) shall be resolved transparently. Secrets shall never appear in logs. | Integration test + audit. |
| AC-08 | Sensitive configuration values shall be encrypted at rest with AES-256-GCM. | Unit test (encryption layer). |
| AC-09 | RBAC and ABAC shall be enforced on all configuration operations. An actor without the appropriate role cannot perform the operation. | Integration test (access control). |
| AC-10 | Change requests requiring approval shall follow the approval workflow: create, notify, approve/reject, apply. None of these steps shall be bypassed for high-risk changes. | E2E test. |
| AC-11 | The system shall detect configuration drift within 60 seconds of occurrence and report it. | Integration test. |
| AC-12 | Feature flags shall support gradual rollout (percentage-based), segmented rollout (by agent type, tenant, region), and kill switch. | Unit test, integration test. |
| AC-13 | The system shall export a full configuration snapshot as YAML or JSON and import it back without data loss. | E2E test: export → import → verify checksum match. |
| AC-14 | The system shall support hot reload of configuration from file changes, etcd/Consul key changes, and vault secret rotations. | E2E test. |
| AC-15 | The system shall tolerate the failure of one config source (e.g., etcd) without losing the ability to serve configuration to consumers (degraded mode). | Chaos test. |
| AC-16 | The system shall tolerate the failure of vault for at least the grace period (2x secret TTL) without failing config reads. | Chaos test. |
| AC-17 | The system shall expose all required metrics (see Section 19.1) in Prometheus format. | Integration test + manual verification. |
| AC-18 | The gRPC API, REST API, and CLI shall all work as documented. | E2E test. |
| AC-19 | The config change commit shall be recorded in the audit log. | Integration test. |
| AC-20 | The system shall support at least 10,000 concurrent gRPC watchers without degradation. | Performance test. |

---

## 30. Definition of Done

A feature or improvement for the AIOS Configuration System is considered **Done** when all of the following conditions are met:

1. **Specification Compliance:** The feature matches the requirements in this document and any associated feature request.
2. **Code Complete:** All code is written, reviewed (at least one approval), and merged into the main branch.
3. **Unit Tests:** All unit tests pass. Coverage for the changed code is >= 90%.
4. **Integration Tests:** All integration tests pass (including any new tests for the feature).
5. **E2E Tests:** All E2E tests pass.
6. **Performance Tests:** Performance benchmarks meet the targets defined in Section 20.3 and Section 21.2.
7. **Security Review:** The security team has reviewed the feature and confirmed no security vulnerabilities. Secrets are handled correctly. Encryption is used where needed. Access controls are enforced.
8. **Documentation:** The feature is documented in the AIOS Configuration System documentation. This includes:
   - User-facing documentation: "How to use this feature."
   - Developer documentation: "How this feature works internally" (if relevant).
   - API documentation: gRPC / REST API updated.
9. **Metrics & Logging:** The feature exposes necessary metrics (Prometheus), logs (structured, appropriate level), and traces (OpenTelemetry spans).
10. **Monitoring & Alerting:** Relevant alerts and dashboards are created or updated.
11. **Backward Compatibility:** The feature does not break any existing API, configuration format, or behavior. If it does change behavior, migration documentation is provided.
12. **Rollback Plan:** Any new behavior can be rolled back (via a feature flag or config change) without data loss.
13. **Operational Review:** The operations team has reviewed the feature and confirmed it is maintainable.
14. **Risk Assessment:** Any new risks are documented in the Risk Analysis section of this document.
15. **Feature Flag:** (Optional, for high-risk features) The feature is gated by a feature flag that defaults to off and is enabled via config.

---

*End of Engineering Specification Document — AIOS Configuration System*

---

**Revision History:**

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0.0 | 2026-07-10 | AIOS Platform Engineering | Initial version. |