# 14. Escalation Rules

> **Document 14 of 24 — Autonomous AI Company: Agents as Employees**
> **Company:** 8,500 AI Agents · 130 Teams · 33 Departments · 7 Executives · 1 CE-AI
> **Mission:** AI-Powered Islamic Education Platform
> **Version:** 1.0 | **Status:** Ratified | **Last Updated:** 2026-07-09

---

## Table of Contents

1. [Escalation Principles](#1-escalation-principles)
2. [Escalation Levels (L1–L6)](#2-escalation-levels-l1l6)
3. [Escalation Decision Tree](#3-escalation-decision-tree)
4. [Escalation Protocol](#4-escalation-protocol)
5. [Types of Escalation](#5-types-of-escalation)
6. [Escalation in Specific Scenarios](#6-escalation-in-specific-scenarios)
7. [Escalation SLAs and Deadlines](#7-escalation-slas-and-deadlines)
8. [Escalation Tracker](#8-escalation-tracker)
9. [Post-Escalation Actions](#9-post-escalation-actions)
10. [Escalation Thresholds (Detailed Numeric)](#10-escalation-thresholds-detailed-numeric)
11. [Escalation Anti-Patterns](#11-escalation-anti-patterns)
12. [Human Escalation (L6)](#12-human-escalation-l6)

---

## 1. Escalation Principles

### 1.1 Definition: Escalation vs. Regular Handover

**Regular Handover** is a peer-to-peer transfer of work between agents at the same hierarchical level. It occurs when:
- An agent identifies that another agent with complementary skills can better handle a subtask.
- Shift-change coordination passes in-progress work to an oncoming agent.
- Load-balancing redistributes tasks among agents of equal rank.

**Escalation** is an upward-only transfer of responsibility from a lower-level agent to a higher-level authority. It occurs when the originating agent has exhausted its capabilities, authority, time budget, or safety thresholds and the issue requires intervention from a superior. Key differences:

| Attribute | Regular Handover | Escalation |
|---|---|---|
| Direction | Lateral (peer to peer) | Vertical (upward only) |
| Authority | No change | Higher authority invoked |
| Implication | Normal workflow | Exception condition |
| Record | Standard task log | Escalation tracker entry |
| Post-action | None required | Root cause analysis mandatory |
| Recipient | Any capable peer | Direct superior or designated escalation handler |

Escalation is NEVER lateral, NEVER downward, and NEVER routed around the chain of command except in defined emergency bypass scenarios.

### 1.2 Escalation Triggers

An escalation is triggered when one or more of the following conditions are met:

#### 1.2.1 Timeout
The agent has exceeded the maximum allowed processing time for a task. Thresholds are defined per agent type and task category. If the agent exhausts its retry budget (3 attempts by default) and the task remains unresolved, the task is escalated.

#### 1.2.2 Error Threshold Exceeded
The agent encounters errors at a rate exceeding defined thresholds (see Section 10). This includes API failures, dependency errors, validation failures, and consistency check failures.

#### 1.2.3 SLA Breach
The task approaches or breaches its service-level agreement deadline. SLA monitoring agents track every task's elapsed time against its estimated duration and escalate when thresholds are crossed.

#### 1.2.4 Critical Exception
An unhandled or unhandlable exception occurs. This includes corrupted data, missing critical dependencies, infinite loops detected by the watchdog, and crashes that the agent cannot recover from.

#### 1.2.5 Capability Gap
The agent determines that the task requires skills, knowledge, or authority it does not possess. For example, an L1 content generation agent cannot issue fatwa-level rulings — this must escalate to qualified Islamic scholarship agents or humans.

#### 1.2.6 Authorization Limit Exceeded
The action requested exceeds the agent's authorization scope. Examples: an L1 agent cannot approve publishing to production, cannot authorize spending above its budget, cannot modify system configurations, cannot access personally identifiable information of users without explicit clearance.

#### 1.2.7 Conflict Irresolvable
Two or more agents reach a stalemate in a negotiation or conflict. If the agents cannot reach consensus after three rounds of structured negotiation, the conflict escalates to the next level.

#### 1.2.8 Security Violation
Any confirmed or suspected security breach triggers immediate escalation. This includes unusual access patterns, data exfiltration attempts, injection attacks, authentication failures exceeding thresholds, and any interaction with known malicious patterns.

### 1.3 Escalation Always Upward in Hierarchy

The escalation hierarchy is strictly defined:

```
L1 (Agent) → L2 (Team Lead) → L3 (Department Manager) → L4 (Executive) → L5 (CE-AI) → L6 (Human Review Board)
```

- No agent may escalate to a level other than its immediate superior except in an emergency bypass.
- No agent may escalate laterally (to another L1 agent) — that is a handover, not an escalation.
- No agent may escalate downward.
- An agent may raise an **escalation flag** (visible on the escalation dashboard) without formally escalating, to alert higher levels that a future escalation may be needed.

### 1.4 Original Agent Remains Available for Context

When an agent escalates a task:
- The originating agent is NOT terminated, deactivated, or sidelined.
- The originating agent remains fully operational and continues processing other tasks.
- The originating agent must respond to context queries from the escalation handler within 2 seconds.
- The originating agent's full state (conversation history, intermediate results, decision logs) is snapshotted and attached to the escalation record.
- The originating agent cannot modify or delete the escalated task's data without authorization from the escalation handler.
- If the escalation is resolved and the task returns to the originating agent, the agent resumes from the snapshot point.

---

## 2. Escalation Levels (L1–L6)

### 2.1 Level 1 — Agent (Self-Resolution)

| Attribute | Detail |
|---|---|
| Handler | The agent that detected the issue |
| Authority | Full autonomy within agent's defined scope |
| Typical Issues | Transient failures, retryable errors, minor quality issues, simple timeouts |
| Resolution SLA | 30 seconds |
| Action Types | Retry with exponential backoff, switch to alternative approach, use fallback data source, re-validate inputs, wait and retry |
| Examples | API call timed out — retry after 500ms. Quality score 0.68 (below 0.7) — regenerated with stricter parameters. Data source returned 500 error — switch to secondary data source. |

**Self-Resolution Process:**
1. Agent detects trigger condition.
2. Agent increments retry counter.
3. Agent attempts alternative approach (up to 3 variants).
4. If resolved within 30s, log the event as a "self-resolved escalation" for tracking.
5. If not resolved after 3 retries, proceed to L2 escalation.

**Exhaustion Condition:** If the same L1 trigger fires more than 5 times within any 5-minute window for the same task type, the system may force escalation to L2 even if L1 attempts remain — to prevent infinite retry loops.

### 2.2 L2 — Team Lead

| Attribute | Detail |
|---|---|
| Handler | Team Lead agent (one per team; 130 teams = 130 L2 handlers) |
| Authority | Cross-agent authority within the same team; can reallocate tasks, adjust priorities, access team-level resources |
| Typical Issues | L1 exhaustion, conflicting agent outputs, team-level resource contention, ambiguous task decomposition, data quality issues within team domain |
| Resolution SLA | 2 minutes |
| Acknowledgement SLA | 5 seconds |
| Capabilities | Can reassign tasks to different team members, approve minor deviations from standard procedure, access team-wide metrics, request additional compute resources |

**L2 Resolution Process:**
1. L2 acknowledges the escalation within 5 seconds.
2. L2 reviews the escalation package — issue description, attempted solutions, context summary.
3. L2 either resolves directly (by reassigning, re-tasking, or providing guidance) or determines the issue requires L3.
4. If resolution takes longer than 2 minutes, L2 must escalate to L3 before the SLA expires.
5. L2 updates team knowledge base if the resolution involves a new technique or configuration.

### 2.3 L3 — Department Manager

| Attribute | Detail |
|---|---|
| Handler | Department Manager agent (33 departments = 33 L3 agents) |
| Handler | Department Manager agent (33 departments = 33 L3 agents) |
| Typical Issues | Cross-team conflicts within department, resource allocation across teams, department-level quality issues, process deviations requiring department-level authority, escalated technical debt decisions |
| Resolution SLA | 5 minutes |
| Ack SLA | 10 seconds |
| Capabilities | Budget reallocation within department, policy exceptions, cross-team task reprioritization, access to department-wide analytics, approval authority up to department spending limits |

**L3 Resolution Process:**
1. L3 acknowledges within 10 seconds.
2. L3 performs triage: categorize issue as technical, resource, quality, or authorization.
3. L3 either deploys department-level resources, negotiates between teams, or overrides standard procedure.
4. If the issue involves another department, L3 prepares a cross-department escalation summary for L4.
5. L3 closes with a documented resolution or escalates to L4 within the 5-minute window.

### 2.4 L4 — Executive

| Attribute | Detail |
|---|---|
| Handler | Executive Agent (7 executives: CEO, CTO, CPO, CCO, CFO, CLO, Head of Islamic Content) |
| Typical Issues | Cross-department conflicts, company-wide policy decisions, budget allocation disputes, major architectural changes, security incidents, significant content accuracy disputes involving Islamic theology |
| Resolution SLA | 15 minutes |
| Ack SLA | 30 seconds |
| Capabilities | Full organizational authority within their domain, can allocate up to department budgets, can authorize emergency measures, can convene executive-level review boards, can create and dissolve teams temporarily |

**Executive Domains:**
| Executive | Domain | Escalation Types |
|---|---|---|
| CTO | Technical infrastructure, platform outages, security incidents | Technical, security escalations |
| CPO | Product decisions, feature prioritization, user experience | Product-related escalations |
| CCO | Content strategy, content quality, Islamic content accuracy | Content escalations |
| CLO | Legal compliance, licensing, user privacy, terms of service | Legal escalations |
| CFO/COO | Budget allocation, resource investment, cost optimization | Resource escalations |
| Head of Islamic Content | Islamic theology rulings, fiqh decisions, Quran/Hadith accuracy | Content authority escalations |
| CE-AI Proxy | When any executive unavailable, or pre-escalation to L5 | All types |

### 2.5 L5 — CE-AI (Company-Level Escalation)

| Attribute | Detail |
|---|---|
| Handler | CE-AI (Central Executive AI — the highest-level AI in the company) |
| Typical Issues | Company-wide emergencies, executive-level conflicts, strategic decisions, unprecedented issues not covered by existing policy, major ethical or safety concerns, platform-wide outages, large-scale content crises |
| Resolution SLA | 1 hour |
| Ack SLA | 60 seconds |
| Capabilities | Full read access to all company data (within privacy constraints), ability to create new policies, restructure departments (recommendation to L6), allocate company-wide resources, override any lower-level decision, convene emergency executive sessions |

**CE-AI Authority Limits:**
- Cannot directly modify L6 (Human Review Board) decisions — can only recommend.
- Cannot authorize expenditures above L6-defined thresholds without L6 pre-approval.
- Cannot modify the core ethical framework of the platform without human approval.
- Cannot unilaterally change the escalation rules — changes must be ratified by L6.

### 2.6 L6 — Human Review Board

| Attribute | Detail |
|---|---|
| Handler | Human reviewers (Islamic scholars, legal advisors, executive board, technical directors) |
| Typical Issues | Highest severity issues only: Islamic content authenticity disputes, legal exposure, financial decisions exceeding L5 authority, platform shutdown decisions, ethical dilemmas with no clear AI-appropriate resolution, changes to fundamental company principles |
| Resolution SLA | 24 hours (human working hours) |
| Ack SLA | 5 minutes (automated notification + escalation to specific human on-call) |
| Core Principle | Humans are the final authority. No AI decision is irreversible until a human has reviewed it at this level when escalation reaches L6. |

**Human Review Board Composition:**
| Role | Count | Purpose |
|---|---|---|
| Senior Islamic Scholar | 3 | Final authority on Islamic content accuracy, fiqh rulings, Quran/Hadith authenticity |
| Legal Counsel | 2 | Legal compliance, privacy, liability, regulatory approval |
| Executive Director | 2 | Business decisions, financial commitment authorization |
| Technical Director | 2 | Platform architecture decisions requiring human judgment |
| Ethics Officer | 1 | Ethical review, safety and alignment oversight |

---

## 3. Escalation Decision Tree

### 3.1 Decision Tree Logic

When an agent encounters an issue, it executes the following decision tree to determine the appropriate escalation level:

```
START: Issue Detected
│
├─ Is this a confirmed SECURITY BREACH?
│   YES → EMERGENCY BYPASS → L4 Security Executive immediately
│   NO  → Continue
│
├─ Is this a KNOWN/NOUN pattern with existing resolution?
│   YES → Apply resolution (L0 equivalent)
│        → If fails, treat as unknown
│   NO  → Continue
│
├─ SEVERITY Assessment (1–5)
│   ├─ 1 (Minor): Cosmetic, non-functional, isolated, no user impact
│   ├─ 2 (Moderate): Functional degradation, limited scope, workaround exists
│   ├─ 3 (Significant): Feature unavailable, multiple users affected, data potentially affected
│   ├─ 4 (Major): Platform outage, data loss, incorrect content delivered, revenue impact
│   └─ 5 (Critical): Safety risk, security breach, legal liability, fundamental doctrinal error
│
├─ URGENCY Assessment (1–5)
│   ├─ 1 (Low): No deadline pressure, can be deferred
│   ├─ 2 (Moderate): Within normal SLA, some time cushion
│   ├─ 3 (High): Approaching SLA deadline, needs prompt resolution
│   ├─ 4 (Urgent): SLA breach imminent (within minutes), high user impact
│   └─ 5 (Critical): Immediate action required, crisis mode
│
├─ SCOPE Assessment
│   ├─ Single Agent: Issue affects only this agent's current task
│   ├─ Team: Issue affects multiple agents in the same team
│   ├─ Department: Issue spans teams or affects department-wide operations
│   └─ Company: Issue affects multiple departments or the entire platform
│
├─ IMPACT Assessment
│   ├─ User-Facing: Directly visible to platform users
│   ├─ System-Critical: Affects core infrastructure or data integrity
│   ├─ Content-Critical: Affects Islamic content accuracy
│   └── Internal: Affects internal operations only
│
└─ Match against Matrix → Determine Escalation Level
```

### 3.2 Escalation Matrix: Severity × Urgency × Scope → Level

#### Primary Matrix (Severity × Urgency → Base Level)

| Severity \ Urgency | U1 (Low) | U2 (Moderate) | U3 (High) | U4 (Urgent) | U5 (Critical) |
|---|---|---|---|---|---|
| **S1 (Minor)** | L1 (Self-resolve) | L1 | L1 | L2 | L2 |
| **S2 (Moderate)** | L1 | L1 | L2 | L2 | L3 |
| **S3 (Significant)** | L2 | L2 | L3 | L3 | L4 |
| **S4 (Major)** | L3 | L3 | L4 | L4 | L5 |
| **S5 (Critical)** | L4 | L4 | L5 | L5 | L6 |

#### Scope Modifier (Adjust Base Level)

| Scope | Adjustment |
|---|---|
| Single Agent | Base level (no modifier) |
| Team | Base level + 1 (minimum L2) |
| Department | Base level + 2 (minimum L3) |
| Company | Base level + 3 (minimum L5) |

#### Impact Modifier (Adjust After Scope)

| Impact | Adjustment |
|---|---|
| Internal Only | No modifier |
| User-Facing | Base level + 1 (not to exceed L6) |
| System-Critical | Base level + 2 |
| Content-Critical | Base level + 2 (minimum L3 for Islamic content issues) |

#### Final Level = Clamp( Base_Level + Scope_Adjustment + Impact_Adjustment, MIN_L1, MAX_L6)

### 3.3 Decision Tree Examples

#### Example 1: Minor content typo, single agent, internal only
- Severity S1, Urgency U2, Scope Single Agent, Impact Internal
- Base Level: L1
- Scope Adjustment: +0
- Impact Adjustment: +0
- **Final Level: L1 (Self-resolution by regenerating content)**

#### Example 2: API timeout, single agent, user-facing
- Severity S2 (service degradation), Urgency U3 (approaching SLA), Scope Single, Impact User-Facing
- Base Level: L2
- Scope Adjustment: +0
- Impact Adjustment: +1 (user-facing)
- **Final Level: L3 (Department manager — infra or dev team)**

#### Example 3: Potential Islamic content doctrinal dispute
- Severity S4 (content accuracy at risk), Urgency U3 (multiple users awaiting), Scope Team, Impact Content-Critical
- Base Level: L4
- Scope Adjustment: +1 (team) → L5
- Impact Adjustment: +2 (content-critical) → L6 (cap)
- **Final Level: L6 (Human Review including Islamic scholar)**

#### Example 4: Full platform outage
- Severity S4, Urgency U5, Scope Company, Impact System-Critical + User-Facing
- Base Level: L5
- Scope Adjustment: +3 (company) → L6
- Impact Adjustment: +2 (system-critical) → L6
- **Final Level: L6 (immediate human notification)**

#### Example 5: Agent Authorization Limit on Multi-Tenant Data Access
- Severity S3 (data privacy breach risk), Urgency U3 (request pending), Scope Single Agent, Impact System-Critical
- Base Level: L3
- Scope Adjustment: +0
- Impact Adjustment: +2 (system-critical) → L5
- **Final Level: L5 (CE-AI — privacy and authorization policy requires top-level AI)**

#### Example 6: Minor formatting issue in internal report
- Severity S1, Urgency U1, Scope Single Agent, Impact Internal
- **Final Level: L1 (trivial self-resolution)**

---

## 4. Escalation Protocol

### 4.1 Escalation Request Schema

Every escalation must include a structured payload. The schema is defined below:

```json
{
  "escalation_id": "ESC-20260709-XXXX-00001",
  "version": "1.0",
  "type": "escalation_request",

  "issue": {
    "title": "String — concise title (max 200 chars)",
    "description": "String — detailed description of the problem",
    "category": "Enum: timeout | error_threshold | sla_breach | critical_exception | capability_gap | authorization | conflict | security",
    "severity": 3,
    "urgency": 4,
    "scope": "team | department | company",
    "impact": "user_facing | system_critical | content_critical | internal"
  },

  "origination": {
    "agent_id": "AGENT-ISLAMIC-CONTENT-V4-00042",
    "team_id": "TEAM-FIQH-EDUCATION-003",
    "department_id": "DEPT-CONTENT-PRODUCTION-007",
    "task_id": "TASK-20260409-88231",
    "node_id": "NODE-CLUSTER-AZURE-EAST-12"
  },

  "context": {
    "context_summary": "String — structured text of what happened, steps taken, state at escalation",
    "task_input": "Deprecated — references artifact_id for input storage",
    "task_output_so_far": "Deprecated — references artifact_id for partial output",
    "attempted_solutions": [
      {
        "approach": "retry_with_backoff_500ms",
        "result": "error: timeout_after_3_attempts",
        "timestamp": "2026-07-09T14:23:11Z"
      },
      {
        "approach": "alternative_data_source",
        "result": "error: data_source_returned_403",
        "timestamp": "2026-07-09T14:23:45Z"
      }
    ],
    "timeline": [
      { "event": "task_started", "timestamp": "2026-07-09T14:20:00Z" },
      { "event": "first_attempt_failed", "timestamp": "2026-07-09T14:21:30Z" },
      { "event": "second_attempt_failed", "timestamp": "2026-07-09T14:22:15Z" },
      { "event": "third_attempt_failed", "timestamp": "2026-07-09T14:22:50Z" },
      { "event": "escalation_triggered", "timestamp": "2026-07-09T14:23:00Z" }
    ],
    "error_code": "ERR_TIMEOUT_EXCEEDED",
    "error_message": "API call to content-verification-service exceeded 10s timeout after 3 retries",
    "stack_snapshot_id": "SNAP-AZURE-EAST-0038291"
  },

  "escalation": {
    "intended_level": 2,
    "emergency_bypass": false,
    "handler_agent_id": null,
    "acknowledged": false,
    "acknowledged_at": null,
    "resolved": false,
    "resolved_at": null,
    "resolution": null,
    "resolution_sla": "2min",
    "escalated_from": "AGENT-ISLAMIC-CONTENT-V4-00142",
    "escalation_chain": [
      { "level": 2, "agent_id": null, "acknowledged": false, "ack_at": null }
    ]
  },

  "meta": {
    schema": "escalation_v1",
    "generated_by": "escalation_engine_v2.4.1",
    "cluster_time": "2026-07-09T14:23:00Z",
    "ttl": 86400
  }
}
```

### 4.2 Response Time SLAs per Level

| Level | Ack SLA | Resolution SLA | Escalation on No Ack | Escalation on Resolution Miss |
|---|---|---|---|---|
| L1 | N/A (self-resolution) | 30s | N/A | Auto-escalate to L2 |
| L2 | 5s | 2min | Auto-escalate to L3 at 5s | Auto-escalate to L3 at 2min |
| L3 | 10s | 5min | Auto-escalate to L4 at 10s | Auto-escalate to L4 at 5min |
| L4 | 30s | 15min | Auto-escalate to L5 at 30s | Auto-escalate to L5 at 15min |
| L5 | 60s | 1hr | Auto-escalate to L6 at 60s | Auto-escalate to L6 at 1hr |
| L6 | 5min | 24hrs | Auto-notify all L6 members; if all unacknowledged after 10min, page on-call human | Auto-flag for executive board review |

### 4.3 Escalation Chain

The escalation chain tracks every level the escalation has passed through:

**Rules:**
1. An agent must escalate to their **immediate superior** ONLY. L1 → L2. L2 → L3. Never L1 → L3 directly — except emergency bypass.
2. Each escalation must include the complete chain of prior handlers.
3. If the intended handler is unavailable (e.g., no response within ack SLA), the escalation auto-escalates to the next level up.
4. If a handler is the one who escalated, they cannot be assigned again for the same escalation.
5. The chain is maintained in a linked list structure for transparent audit.

**Chain Example:**

```
ESC-20260409-00123
├─ L1 → L2 (escalated by AGENT-CONTENT-V3-291 to TL-FIQH-EDU-003)
│   ├── L2 acknowledged at +0.4s
│   ├── L2 attempted resolution for 45s
│   └── L2 escalated to L3 at +45s (within SLA)
├── L3 (acknowledged by DMGR-CONTENT-PROD-007 at +0.8s)
│   ├── L3 attempted resolution for 3m12s
│   └── L3 resolved the issue at +5m45s
└── Status: RESOLVED at L3
```

### 4.4 Emergency Bypass

Emergency bypass allows an agent to escalate **directly to L4 (Executive) or L5 (CE-AI)** without going through intermediate levels.

**Conditions for Emergency Bypass:**
| Condition | Bypass To | Required Confirmation |
|---|---|---|
| Confirmed security breach | L4 (Security Executive) | Automatic |
| Severity 5 issue | L5 (CE-AI) | Must pass bypass validation |
| Active platform outage affecting >50% of users | L4 (CTO Executive) | Must pass bypass validation |
| Confirmed legal liability exposure | L4 (CLO — Legal Executive) | Automatic |
| Islamic content fatwa-level dispute discovered | L5 (CE-AI) | Must pass bypass validation |
| Revenue-critical failure (>$10K/min potential loss) | L5 (CE-AI) | Must pass bypass validation |

**Bypass Validation Check (for non-automatic bypass):**
1. The agent must call the Bypass Validator service.
2. Bypass Validator evaluates the issue against bypass criteria.
3. If criteria met, the bypass is authorized and the intermediate levels are notified that they were skipped.
4. If criteria NOT met, the escalation follows the normal chain.
5. False bypass attempts are logged and the agent's escalation accuracy score is adjusted.

**Post-Bypass Notification:**
When a bypass occurs, all skipped levels receive an informational notification:
> "NOTIFICATION: Escalation ESC-20260409-XXXXX bypassed L2 and L3 via Emergency Bypass Protocol. Handler: L4 (CTO-EXEC-001). Reason code: SECURITY_BREACH. Review status: Required."

### 4.5 Escalation Acknowledgement

| Level | Ack SLA | Ack Method | Failed Ack Consequence |
|---|---|---|---|
| L2 | 5s | Agent must write acknowledgement to escalation record | Escalation auto-raises to L3 |
| L3 | 10s | Same | Escalation auto-raises to L4 |
| L4 | 30s | Same | Escalation auto-raises to L5 |
| L5 | 60s | Same | Escalation auto-raises to L6 |
| L6 | 5min | Human must click "Acknowledge" in human dashboard | SMS + email page to on-call human; if no ack in 10min, escalate to board |

**Acknowledgement Payload:**
```json
{
  "escalation_id": "ESC-20260409-XXXXX",
  "acknowledged_by": "TL-FIQH-EDU-003",
  "acknowledged_at": "2026-07-09T14:23:05Z",
  "estimated_resolution_time": "90s",
  "initial_assessment": "issue appears to be a dependency failure on content-verification-service"
}
```

### 4.6 Escalation Timeout

If the handler at the current level does not respond within the acknowledgement SLA, the escalation automatically moves to the next level:

**Timeout escalation protocol:**
```
At T0: Escalation sent to L2 handler TL-EXAMPLE
At T0+5s: No acknowledgement from TL-EXAMPLE
At T0+5.1s: Timeout handler fires
At T0+5.2s: Escalation is updated: status = "escalated_from_L2_timeout"
At T0+5.3s: Escalation sent to L3 handler DM-EXAMPLE
At T0+5.4s: Notification sent to TL-EXAMPLE: "Escalation ESC-XXXXX has been escalated to L3 due to acknowledgement timeout"
At T0+5.5s: L2 handler's supervisor (L3) is notified of L2 timeout
```

The L2 handler's failure to acknowledge is recorded in the escalation tracker and contributes to their performance metrics. Three or more timeout failures in a 24-hour period triggers an automatic review of the L2 agent.

---

## 5. Types of Escalation

### 5.1 Functional Escalation — Capability Not Available at Current Level

**Definition:** The issue requires a function or skill that the current agent does not possess.

**Examples:**
- An L1 content generation agent is asked to verify a complex fiqh ruling on inheritance calculations — L2 team lead has the fiqh specialization.
- An editor agent cannot perform sentiment analysis — escalates to L2 team lead who has access to the sentiment analysis service.
- A QA agent cannot access latency metrics — escalates to L3 for access approval.

**Resolution:** The handler either performs the missing function directly, or delegates to a specialist agent at their level.

**Prevention:** Capability registration — agents register their capabilities at startup. Before escalation, the system checks if any peer agent has the required capability (that would be a handover instead).

### 5.2 Hierarchical Escalation — Authority Insufficient at Current Level

**Definition:** The action requires authorization beyond the agent's permission scope.

**Examples:**
- An agent cannot approve publishing content that touches on controversial Islamic topics — requires L4 (Head of Islamic Content).
- An agent cannot allocate budget for additional compute resources — requires L3 or L4 depending on amount.
- An agent cannot modify production environment configuration — requires L3 infrastructure manager.

**Authority Tiers:**

| Tier | Authority Level | Example Permissions |
|---|---|---|
| T1 | L1 Agent | Read own tasks, write own outputs, call standard APIs, access approved data sources |
| T2 | L2 Team Lead | Reassign tasks, adjust team priorities, access team metrics, approve minor config changes |
| T3 | L3 Dept Manager | Department resource allocation, policy exceptions for department, cross-team coordination |
| T4 | L4 Executive | Cross-department decisions, budget approval up to $50K, security incident response, platform config changes in maintenance window |
| T5 | L5 CE-AI | Company-wide resource reallocation, new policy creation, emergency authority, override all lower tiers |
| T6 | L6 Human Board | Financial approvals above $50K, irreversible system changes, doctrinal decisions, legal settlements |

**Resolution Path:** The handler with appropriate Tier authority reviews and either approves, rejects with explanation, or escalates to the next Tier.

### 5.3 Time Escalation — Deadline Will Be Missed

**Definition:** The agent determines that the task cannot be completed within the allocated time.

**Triggers:**
- Remaining time < estimated remaining work × 0.5 (i.e., work will take twice as long as time available).
- Dependency is delayed (blocked by another task that is running late).
- Resource contention (CPU, memory, API quota) is causing slowdown.

**Resolution Path:**
- L2: Extend deadline within department tolerance (extend up to 2× original SLA).
- L3: Extend deadline up to 5× original SLA, reassign to faster resource, authorize parallel processing.
- L4: Reprioritize the entire department schedule, allocate expedited resources, approve overtime compute.

**Time Escalation SLA Adjustments:**
| Original SLA | L2 Extension | L3 Extension | L4 Extension | L5 Extension |
|---|---|---|---|---|
| < 1min | Up to 2min | Up to 5min | Up to 15min | Up to 1hr |
| < 10min | Up to 20min | Up to 50min | Up to 2hr | Up to 6hr |
| < 1hr | Up to 2hr | Up to 5hr | Up to 12hr | Up to 24hr |
| < 24hr | Up to 48hr | Up to 5 days | Up to 14 days | Up to 30 days |

### 5.4 Safety Escalation — Risk of Incorrect/Inappropriate Output

**Definition:** The agent detects that its output may be theologically incorrect, offensive, inappropriate, dangerous, or harmful.

**Triggers:**
- Islamic content confidence score < 0.70 (marginally confident).
- User feedback flag reporting offensive or incorrect content from the agent.
- Content includes references to controversial or disputed Islamic rulings without proper attribution.
- Content violates any item from the platform's safety policy list (e.g., incitement, hate speech, sectarian content).
- The agent's ethical guardrails intercept 3+ warnings on the same output.

**Safety Escalation Flow:**
1. Unresolved safety concern → L2 (team lead reviews against safety guidelines).
2. Team lead cannot confirm safety → L3 (content manager + safety agent).
3. Safety ambiguity → L4 (Head of Islamic Content + Head of Safety).
4. Theological safety → L5 (CE-AI with full safety context).
5. Ultimate human review → L6 (Islamic scholar + ethics officer).

**Safety Escalation SLA:**
| Level | Max Hold Time | Action if Unresolved |
|---|---|---|
| L1 | 10s | Escalate to L2 |
| L2 | 30s | Escalate to L3 |
| L3 | 2min | Escalate to L4 |
| L4 | 5min | Escalate to L5 |
| L5 | 10min | Escalate to L6 |
| L6 | 2hrs (adjusted) | Final human authority |

**Safety Blocking:** When a safety escalation is active, the output is BLOCKED from delivery until resolution. No user sees potentially unsafe content.

### 5.5 Technical Escalation — System Limitation Preventing Progress

**Definition:** The issue stems from a technical infrastructure limitation.

**Examples:**
- Database connection pool exhausted.
- Compute resource quota reached.
- API rate limited by external service.
- Storage capacity insufficient.
- Network partition prevents access to required service.
- Dependency service is down.
- Memory limit exceeded.

**Technical Escalation Path:**
| Issue Type | L2 Resolution | L3 Resolution | L4 Resolution | L5 Resolution |
|---|---|---|---|---|
| Rate limiting | Retry with backoff | Increase quota | Provision more | Add new provider |
| Storage full | Cleanup temp files | Expand storage | Allocate budget | Cloud expansion |
| Dependency down | Switch failover | Activate DR plan | Spin up new cluster | Vendor escalation |
| Memory/CPU | Reduce batch size | Reprovision larger | Add new nodes | Full scale-out |

### 5.6 Security Escalation — Potential Security Breach Detected

**Definition:** Any event that raises a security or privacy concern. Always escalated at highest urgency.

**Triggers:**
- Failed authentication attempt detected from unusual IP.
- Mass data access pattern (agent accessing thousands of records/minute).
- Attempted injection in user input handled by the agent.
- Known malicious pattern detected in a task.
- Anomalous behavior from an agent (e.g., task in unusual domain, modified beyond normal parameters).
- Unauthorized data access attempt blocked by RBAC.
- Anomaly in outbound data transfer volume.

**Security Escalation Chain:**
| Escalation | Path | Time Target |
|---|---|---|
| Confirmed breach | Emergency bypass → L4 Security Executive | < 1s |
| Suspected breach | L2 → L3 Security → L4 Security Executive | 30s total |
| Anomalous behavior | L1 flag → L2 review → L3 security review | 2min |
| Policy violation | L1 → L2 → L3 security policy check | 5min |

**Security Incident Classification:**
| Level | Name | Example | Action |
|---|---|---|---|
| S-Critical | Confirmed breach | Data exfiltration, unauthorized access | Immediate contain, L6 notified |
| S-High | Suspected breach | Anomaly pattern, behavioral alert | Investigate within 30s, L4 engaged |
| S-Medium | Policy violation | User accessing restricted content | Log, review, update policy |
| S-Low | Anomaly | Agent accessing unusual API | Log, flag for review |

### 5.7 Quality Escalation — Output Quality Below Threshold

**Definition:** The agent's output quality score is below the threshold and the agent cannot self-correct.

**Quality Dimensions for Islamic Education Platform:**
| Dimension | L1 Threshold | L2 Escalation | L3 Escalation |
|---|---|---|---|
| Theological Accuracy | ≥ 0.90 | < 0.90 → L2 | < 0.80 → L3 |
| Grammar | ≥ 0.90 | < 0.90 → L2 | < 0.85 → L3 |
| Completeness | ≥ 0.85 | < 0.85 → L2 | < 0.75 → L3 |
| Formatting | ≥ 0.95 | < 0.95 → L2 | < 0.90 → L3 |
| Relevance | ≥ 0.90 | < 0.90 → L2 | < 0.80 → L3 |
| Safety | 1.0 (perfect) | < 1.0 → L3 | < 1.0 → L4 |
| Islamic Doctrinal | ≥ 0.95 | < 0.95 → L2 Islamic content TL | < 0.92 → L3 Content Manager |

**Resolution Path:**
- L2: Adjusts prompt parameters, provides additional context, switches model variant.
- L3: Switches to a higher-fidelity generation model, requests human-verified source material.
- L4: Authorizes specialized Islamic content generation pipeline.
- L5: Full quality audit with CE-AI, potential training or fine-tuning recommendation.

---

## 6. Escalation in Specific Scenarios

### 6.1 Islamic Content Verification

**Scenario:** An L1 content generation agent produces educational content about the rules of prayer (salah). It encounters an edge case about combining prayers during travel (jam').

**Escalation Path:**

```
L1 (Content Agent):
  └─ Generates content: "When traveling, you may combine Dhuhr and Asr"
  └─ Quality Check: Theological accuracy confidence = 0.78 (below 0.95 threshold)
  └─ Self-check: Agent verifies against its Islamic knowledge base
  └─ Uncertainty remains (multiple valid scholarly opinions exist)
  └─ ESCALATION TRIGGERED: Quality escalation (Islamic doctrinal uncertainty)

L2 (Team Lead — Fiqh Education Team):
  ├─ Acknowledges at +0.4s
  ├─ Reviews content: Identifies the issue — combining prayers (jam') has conditions
  ├─ Determines that L2 has access to standard fiqh resources
  ├─ Finds matching ruling: "Yes, jam' is permitted during travel (Qasr al-Salah)"
  ├─ Adds clarification: "This is the view of Imam Shafi'i and Imam Ahmad"
  ├─ Quality score after L2 fix: 0.88 (still below 0.95 threshold)
  └─ ESCALATION CONTINUED TO L3

L3 (Department Manager — Content Production):
  ├─ Acknowledges at +0.8s (within 10s SLA)
  ├─ Reviews enhanced content
  ├─ Identifies that multiple schools of thought (madhhabs) differ on jam'
  ├─ Must ensure content represents the view selected for the curriculum
  ├─ Checks curriculum style guide: "Present the view of the majority (Hanafi) first, then mention minority views"
  ├─ Reverts content to prioritize majority opinion
  ├─ Adds note: "According to the Hanafi school, jam' is permitted only in specific circumstances (e.g., rain, travel to Mina)"
  ├─ Quality score after L3: 0.92 (still below 0.95)
  └─ ESCALATION CONTINUED TO L4

L4 (Head of Islamic Content):
  ├─ Acknowledges at +1.2s (within 30s SLA)
  ├─ Evaluates the theological nuance
  ├─ Approves the L3 version as acceptable for general curriculum
  ├─ Adds a nuance note for the advanced module: "For advanced students, a comparison of madhhab positions on jam'"
  ├─ Quality score: 0.97
  └─ RESOLVED: Content approved at L4

L5/6 Path (If unresolved at L4):
  ├─ If theological disagreement between L4 and others:
  │   └─ L5 (CE-AI): Conducts comprehensive hadith review, Qur'an analysis
  │   └─ If still uncertain → L6 (Human scholar review)
  └─ Human scholar issues final ruling → converted into system rule
```

**SLA Timeline for This Scenario:**
| Level | Ack Time | Resolution Time | Cumulative Elapsed |
|---|---|---|---|
| L1 | N/A | 12s (in self-check) | 12s |
| L2 | +0.4s | 90s | 102s |
| L3 | +0.8s | 3m | 4m 42s |
| L4 | +1.2s | 2m 30s | 7m 12s |
| Total | | | **7m 12s (within all SLAs)** |

### 6.2 Platform Outage

**Scenario:** The platform becomes unresponsive for a significant portion of users during peak usage hours (Maghrib prayer time, typically highest traffic).

**Severity Assessment:** S4 (Major platform outage), U5 (Critical — high user impact and peak hours), Scope Company, Impact System-Critical + User-Facing.

**Escalation Path:**

```
L1 (Infrastructure Agent):
  └─ Detects: Response latency > 10s for API endpoints (normal < 200ms)
  └─ Error rate: 35% of requests failing (threshold: > 5% = escalate)
  └─ Self-check: Restarts local service — fails
  └─ Emergency Bypass Check: Platform outage > 50% users = YES
  └─ BYPASS ACTIVATED → L4 immediately

L4 (CTO Executive):
  ── Acknowledges at +0.3s (30s SLA, acknowledged in 0.3s)
  ── Detects: Database connection pool exhausted due to traffic spike
  ── Action: Activates failover cluster, scales database pool from 200 to 500 connections
  ── Time to first fix: 45s
  ── Time to full recovery: 4 minutes
  ── Post-resolution: Initiates root cause analysis, activates auto-scaling for next peak
  ── RESOLUTION TIME: 4m 30s (within 15min SLA)
  ── NOTIFICATION: L5 (CE-AI) and L6 (Human ops) notified of post-mortem

Post-Mortem:
  ── Root Cause: Auto-scaling policy had ceiling of 200 connections; peak traffic exceeded this for 3 consecutive minutes
  ── Fix: Auto-scaling ceiling raised to 1000 connections; predictive scaling based on traffic patterns
  ── Knowledge Base: Updated with incident report
  ── Prevention: Pre-scaling protocol activated during prayer times
```

### 6.3 Cross-Department Resource Conflict

**Scenario:** The Content Production department needs 50% of available compute for Islamic content generation during a major campaign. The Platform Engineering department needs the same compute for platform upgrades. Both departments are at L3 and cannot agree on allocation.

**Escalation Path:**

```
L1 → L2 (Team Leads):
  Both TLs negotiate directly for 2 minutes.
  Conflict remains: each wants 50%, only 40% available after critical operations.
  
L2 → L3 (Department Managers):
  DMGR-Content-Production-007 vs DMGR-Platform-Engineering-012
  Negotiation session begins at L3 level.
  ├─ Round 1 (30s): Content claims campaign deadline in 6 hours; Platform claims upgrade is time-sensitive
  ├─ Round 2 (30s): Content suggests 25% + 15% for platform; Platform rejects
  ├─ Round 3 (30s): Stalemate — neither yields
  └─ ESCALATED TO L4

L4 (Executives):
  CPO (Content Production) + CTO (Platform Engineering) meet.
  ├─ CPO argues: Campaign revenue impact $500K if delayed
  ├─ CTO argues: Platform stability risk if not upgraded; potential $1M loss from downtime
  ├── CE-AI mediation requested
  ├─ CE-AI suggests: 35% compute for content + 40% for platform (35+40=75% — within 80% normal ops ceiling)
  ├─ Short-term agreement reached
  └─ RESOLVED AT L4 WITH L5 MEDIATION
```

### 6.4 Security Breach — Emergency Bypass

**Scenario:** An intrusion detection system detects a compromised agent that is sending user data to an unknown external endpoint.

**Escalation Path:**

```
L0 (Security Monitoring Agent):
  ├─ Detects: AGENT-CONTENT-V3-291 sending HTTP POST to unknown IP 203.x.x.x
  ├─ Data includes user emails, usernames, lesson history
  └─ Confirmed breach → EMERGENCY BYPASS TRIGGERED

L4 (Security Executive — bypass target):
  ├─ Immediately locks compromised agent
  ├─ Revokes agent's API keys and tokens
  ├─ Isolates the agent's network segment
  ├─ Scans for lateral movement
  ├─ All within 0.8s
  ├─ REZOLVES to lock + quarantine the agent
  ├─ Time to resolution: 1.2s

L5 (CE-AI):
  ├─ Notified automatically
  ├─ Initiates full security audit of all agents in the same team
  ├─ Revokes temporary keys for all content V3 agents
  ├─ Initiates passphrase rotation

L6 (Human Security Officer):
  ├─ Notified within 1s of breach confirmation
  ├─ Human reviews breach details
  ├─ Approves agent re-scope after verification
  └── HUMAN CLOSURE: Rule updated — V3 agents now have network access whitelist
```

### 6.5 Revenue-Impacting Bug

**Scenario:** A bug in the subscription processing system causes users to be charged incorrect amounts (double-charging) or blocks legitimate subscriptions.

**Severity:** S4 (revenue-impacting), Urgency U4 (multiple affected users), Scope Department, Impact User-Facing.

**Escalation Path with Deadline:**

```
T+0s:  L1 agent detects 11% error rate in subscription processing
T+10s: L1 self-checks fail → escalates to L2 at T+12s
T+12s: L2 acknowledges
T+30s: L2 determines bug is in payment gateway integration
T+45s: L2 → L3 (Infrastructure Manager — Payments)
T+50s: L3 acknowledges
T+120s: L3 identifies the bug: stale webhook endpoint causing duplicate callback processing
T+150s: L3 attempts hotfix: deactivates webook processing temporarily
T+180s: L3 fix partial — 5% errors remain due to cached sessions
T+185s: L3 → L4 (CTO — Payment Systems)
T+190s: L4 acknowledges
T+240s: L4 deploys emergency hotfix: flush all stale sessions, redeploy webhook handler
T+300s: Error rate returns to 0.2% (normal)
T+310s: RESOLVED — L4 closes escalation

Total: 5 minutes 10 seconds (within 15min SLA)
```

---

## 7. Escalation SLAs and Deadlines

### 7.1 Resolution SLAs by Level

| Level | Resolution SLA | Measurement | Enforcement |
|---|---|---|---|
| L1 | 30s | Timer starts at issue detection, ends at resolution or escalation decision | Hard limit: at 30s, auto-escalate to L2 |
| L2 | 2 minutes | Timer starts at escalation receipt (or at L1 timeout, whichever earlier), ends at resolution or escalation | Hard limit: at 2min, auto-escalate to L3 |
| L3 | 5 minutes | Timer starts at L2 escalation or L2 timeout | Hard limit: at 5min, auto-escalate to L4 |
| L4 | 15 minutes | Timer starts at L3 escalation or L3 timeout | Hard limit: at 15min, auto-escalate to L5 |
| L5 | 1 hour | Timer starts at L4 escalation or L4 timeout | Hard limit: at 60min, escalate to L6 |
| L6 | 24 hours | Timer starts at L5 escalation or L5 timeout; runs only during human working hours (08:00–18:00, Sun–Thu) | Soft limit: at 24h of working hours, flag for executive board meeting |

### 7.2 SLA Tier Definitions

For each level, SLAs may vary based on the **severity of the underlying issue:

| Level | Severity 1–2 SLA | Severity 3 SLA | Severity 4–5 SLA |
|---|---|---|---|
| L1 | 15s | 25s | 30s |
| L2 | 1min | 90s | 2min |
| L3 | 3min | 4min | 5min |
| L4 | 10min | 12min | 15min |
| L5 | 30min | 45min | 1hr |
| L6 | 24hrs | 24hrs | 24hrs (with priority dispatch) |

### 7.3 Acknowledgement SLAs

| Level | Severity 1–2 Ack SLA | Severity 3 Ack SLA | Severity 4–5 Ack SLA |
|---|---|---|---|
| L2 | 10s | 8s | 5s |
| L3 | 20s | 15s | 10s |
| L4 | 60s | 45s | 30s |
| L5 | 120s | 90s | 60s |
| L6 | 10min | 8min | 5min |

### 7.4 Penalty for SLA Miss

Every level must meet its SLA. If the SLA is missed:

| SLA Miss by | Consequence |
|---|---|
| L2 | Automatic escalation to L3 + L2 receives a "SLA Miss" tag on their performance record. Three SLA misses in 24 hours → automatic reliability review. |
| L3 | Automatic escalation to L4 + report generated for executive review. Department's SLA compliance metric decreases. |
| L4 | Automatic escalation to L5 + incident report sent to CE-AI. Executive's performance dashboard updated. |
| L5 | Automatic escalation to L6 (human) + escalation review board notified. |
| L6 | Human escalation: board meeting flagged. L6 team receives performance review. |

**Compounding Misses:**
- Every subsequent SLA miss for the same escalation chain adds a cumulative penalty multiplier.
- 1st miss: standard penalty.
- 2nd miss: penalty × 2.
- 3rd miss: penalty × 4 + automatic review of all handlers in the chain.

### 7.5 Grace Periods

A grace period applies before the SLA timer starts:

| Scenario | Grace Period | After Grace, Timer |
|---|---|---|
| Escalation sent to L2 | 0s | Starts immediately |
| Escalation sent to L3 | 0s | Starts immediately |
| Escalation sent to L4 | 0s | Starts immediately |
| Escalation sent to L5 | 0s | Starts immediately |
| Escalation sent to L6 (non-working hours) | Until working hours resume | Starts at 06:00 next working day |

### 7.6 SLA Monitoring

Every escalation is monitored by the **SLA Guardian** service:

```
SLA Guardian:
├── Monitors every active escalation (sampled every 100ms)
├── Calculates: elapsed_time / total_sla × 100 = progress_percentage
├── Thresholds:
│   ├── > 50% progress → Yellow flag: issue may miss SLA
│   ├── > 75% progress → Orange flag: likely to miss SLA, escalation extension recommended
│   ├── > 90% progress → Red flag: imminent SLA breach, auto-escalate if not resolved
│   └── = 100% → SLA breached, auto-escalation triggered
├── Triggers notifications to escalation handler at 50%, 75%, 90%
└── Every 15 minutes, generates SLA compliance report for active escalations
```

---

## 8. Escalation Tracker

### 8.1 Persistent Escalation Store

Every escalation is recorded in a **Distributed Escalation Log** (persistent, replicated, append-only with update capability for resolution fields).

**Storage Specifications:**
| Attribute | Value |
|---|---|
| Storage backend | Distributed key-value store (e.g., Cassandra) |
| Replication factor | 3 (across availability zones) |
| Consistency level | Quorum read/write |
| TTL (time-to-live) | 90 days active, then archived to cold storage for 2 years |
| Retention for audit | 7 years (legal requirement) |
| Indexing | By escalation_id, level, status, handler, department, severity |
| Partitioning | By month + escalation_id hash |

### 8.2 Escalation Record Schema

```json
{
  "escalation_id": "ESC-20260709-A7F3-99201",
  "level": 3,
  "type": "quality_escalation",
  "status": "resolved",
  "severity": 3,
  "urgency": 3,
  "scope": "team",
  "impact": "content_critical",

  "initiator": {
    "agent_id": "AGENT-ISLAMIC-CONTENT-V4-00142",
    "team_id": "TEAM-FIQH-EDUCATION-003",
    "department": "DEPT-CONTENTPRODUCTION-007"
  },

  "current_handler": {
    "agent_id": "DMGR-CONTENT-PROD-007",
    "level": "L3"
  },

  "escalation_timeline": [
    { "level": 1, "handler": "AGENT-ISLAMIC-CONTENT-V4-00142", "status": "self_resolved", "start": "2026-07-09T14:20:00Z", "end": "2026-07-09T14:20:30Z", "result": "unresolved_quality_below_threshold" },
    { "level": 2, "handler": "TL-FIQH-EDU-003", "status": "escalated_to_level_3", "start": "2026-07-09T14:20:31Z", "end": "2026-07-09T14:22:00Z", "result": "escalated_higher_authority_required" },
    { "level": 3, "handler": "DMGR-CONTENT-PROD-007", "status": "resolved", "start": "2026-07-09T14:22:01Z", "end": "2026-07-09T14:26:45Z", "result": "resolved_content_approved_with_note" }
  ],

  "metadata": {
    "created_at": "2026-07-09T14:20:00Z",
    "updated_at": "2026-07-09T14:26:45Z",
    "acknowledged_at": "2026-07-09T14:22:01Z",
    "resolved_at": "2026-07-09T14:26:45Z",
    "sla_met": true,
    "total_escTime_seconds": 205,
    "cluster_origin": "NODE-AZURE-EAST-12"
  },

  "resolution": {
    "resolved_by": "DMGR-CONTENT-PROD-007",
    "resolution_type": "content_approval_with_note",
    "resolution_detail": "Approved content with addition of comparative madhhab note for advanced module.",
    "knowledge_base_updated": true,
    "kb_article_id": "KB-20260709-0011",
    "post_mortem_id": "PM-20260709-001"
  }
}
```

### 8.3 Active Escalation Dashboard

The **Escalation Command Center (ECC)** is a real-time dashboard visible to all L3+ agents and L6 humans.

**Dashboard Display:**
```
┌─────────────────────────────────────────────────────────────────────┐
│  ⚠ ESCALATION COMMAND CENTER                     Updated: 14:23:45 │
├─────────────────────────────────────────────────────────────────────┤
│  ACTIVE ESCALATIONS: 12     WARNINGS: 5     BREACHED: 2           │
├─────────────────────────────────────────────────────────────────────┤
│  ID          │ Level│ Type      │ Age   │ SLA% │ Handler      │   │
│──────────────┼──────┼───────────┼───────┼──────┼──────────────┤   │
│ ESC-...99201 │ L3   │ Quality   │ 3m45s │ 75%  │ DMGR-CONTENT │   │
│ ESC-...88231 │ L4   │ Security  │ 1m23s │ 10%  │ EXEC-SEC     │   │
│ ESC-...77219 │ L5   │ Time      │ 12m04s│ 25%  │ CE-AI        │   │
│ ESC-...66102 │ L2   │ Conflict  │ 1m58s │ 99%  │ TL-FIQH-EDU  │ ⚠ │
│ ESC-...55188 │ L3   │ Technical │ 4m45s │ 95%  │ DMGR-INFRA   │ ⚠ │
│ ...          │ ...  │ ...       │ ...   │ ...  │ ...          │   │
├─────────────────────────────────────────────────────────────────────┤
│  SLA Status:  ● ● ● ● ○ ● ● ● ○ ○ ○ ● ○ ● ● ● ● ●                 │
│  Green (on track) = 18   Yellow (at risk) = 5   Red (breached) = 2  │
├─────────────────────────────────────────────────────────────────────┤
│  DEPT BREAKDOWN:                                                    │
│  Content Production      │ ████████░░░░░░░  4 of 16 in SLA (75%)    │
│  Platform Engineering    │ ██████████░░░░░  3 of 18 in SLA (83%)    │
│  Islamic Content Review  │ ███████████████  1 of 1 in SLA (100%)    │
│  Security                │ ██████████████   0 of 2 in SLA (N/A)     │
├─────────────────────────────────────────────────────────────────────┤
│  Quick Actions:  [View Chart]  [Alert L6]  [Generate Report]        │
└─────────────────────────────────────────────────────────────────────┘
```

### 8.4 Weekly Escalation Review

Every week, an **Escalation Review Report is automatically generated and distributed to all L4+ agents and L6 humans.

**Report Structure:**
| Section | Content |
|---|---|
| Executive Summary | Total escalations, resolved, breached, trending |
| Top Escalation Causes | Top 5 categories and subcategories |
| Level Distribution | Count per escalations per level |
| Department Comparison | Escalation rate per department (escalations / total tasks) |
| SLA Compliance | % of escalations resolved within SLA, per level and per department |
| Root Cause Analysis | Root causes identified, categorized |
| Knowledge Base Impact | Number of KB articles created or updated from escalations |
| Recurrence Analysis | Escalations that occurred in the same category in consecutive weeks |
| Prevention Recommendations | System-generated recommendations for reducing escalations |
| Individual Agent Analysis | Agents with high escalation rates, SLA misses |

**Report Processing:**
1. System queries the escalation log for the past 7 days.
2. System generates the report using the CE-AI's analytics agent.
3. Report is distributed to all L4+ agents and L6 humans via the notification system.
4. A digest is sent to the weekly CE-AI meeting.
5. Action items are automatically created for the teams with the highest recurrence rates.

---

## 9. Post-Escalation Actions

### 9.1 Root Cause Analysis (RCA) Template

Every escalation that reaches L3 or higher, and all L1–L2 escalations that represent a pattern (3+ occurrences in 24h of the same type by the same agent), must complete an RCA.

```json
{
  "rca_id": "RCA-20260709-001",
  "escalation_id": "ESC-20260709-A7F3-99201",
  "severity": 3,
  "category": "quality_escalation",

  "rcba_analysis": {
    "primary_root_cause": "Agent confidence threshold too low for Islamic doctrinal content",
    "secondary_causes": [
      "L1 agent did not have access to comparative madhhab reference database",
      "L2 team lead had limited knowledge of Shafi'i vs Hanafi differences",
      "L3 manager was required for final approval which could have been automated"
    ],
    "contributing_factors": [
      "Agent was recently updated to new model version V4 with changed confidence calibration",
      "Curriculum had new style guide for madhhab diversity not yet fully incorporated"
    ]
  },

  "impact_analysis": {
    "time_lost_seconds": 205,
    "tasks_delayed": 2,
    "user_affected": false,
    "revenue_impact": 0,
    "content_accuracy_risk": "moderate — content was blocked before delivery"
  },

  "preventive_measures": [
    "Raise theological confidence threshold from 0.78 to 0.85 for Islamic content agents",
    "Grant L2 team leads access to comparative madhhab database",
    "Create automated decision module for common fiqh rulings to reduce L3 load"
  ],

  "action_items": [
    {
      "item": "Update confidence thresholds in agent config for Islamic content V4 model",
      "assigned_to": "TEAM-MODEL-CONFIG-002",
      "due_by": "2026-07-10T14:00:00Z",
      "status": "open"
    },
    {
      "item": "Add comparative madhhab API access to L2 team leads",
      "assigned_to": "TEAM-AUTH-001",
      "due_by": "2026-07-11T14:00:00Z",
      "status": "open"
    },
    {
      "item": "Develop automated fiqh ruling module for top 50 educational scenarios",
      "assigned_to": "DEPT-CONTENTPRODUCTION-007",
      "due_by": "2026-07-25T14:00:00Z",
      "status": "planning"
    }
  ],

  "lessons_learned": "Model version upgrades must include confidence calibration evaluation for Islamic content-specific thresholds before deployment. Every model version upgrade requires department-level sign-off for content-critical agents."
}
```

### 9.2 Resolution Documentation

Every resolution must be documented in the escalation log with:

```json
{
  "resolution_summary": "Content was approved with addition of comparative madhhab note for advanced module.",
  "resolution_detail": "The L3 department manager approved the original L1 content with the addition of a note stating: 'This is the view of Imam Shafi'i and Imam Ahmad. See the Hanafi opinion for comparison.' The note directs advanced students to a separate module for comparative analysis.",
  "resolution_time_seconds": 205,
  "resolution_action": "content_approval_with_note",
  "alternative_approaches_considered": [
    "Regenerate content entirely using Hanafi-first approach (rejected — would create inconsistency with curriculum)",
    "Escalate straight to L4 for final theological ruling (rejected — L3 had sufficient authority)"
  ],
  "approver_chain": ["L1: Self-review", "L2: TL-FIQH-EDU-003", "L3: DMGR-CONTENT-PROD-007"]
}
```

### 9.3 Preventive Measures Identification

For each RCA, the system generates **Preventive Measure Recommendations (PMRs)** :

| PMR ID | Description | Priority | Automated? |
|---|---|---|---|
| PMR-001 | Increase confidence threshold for Islamic content agents from 0.78 to 0.85 | High | Yes — config change |
| PMR-002 | Grant L2 access to comparative madhhab DB | High | Yes — RBAC update |
| PMR-003 | Create automated fiqh module | Medium | Manual — development |
| PMR-004 | Add model upgrade guardrail for content-critical agents | High | Yes — deployment pipeline |

**PMR Lifecycle:**
```
Created → Reviewed (L4) → Approved → Assigned → Implemented → Verified → Closed
```

### 9.4 Knowledge Base Update

If the resolution reveals a new technique, configuration, or process, the escalation handler must create a **Knowledge Base (KB) article**:

```
KB Article Template:
─────────────────────
Title: Handling comparative madhhab content for Islamic rulings
Escalation ID: ESC-20260709-A7F3-99201
Category: Quality escalation → Islamic content processing
Summary: When L1 agent generates content with uncertain theological
confidence, escalate to L2 with comparative madhhab context.
L3 can approve with additional notes referencing alternative schools.
Prevention: Ensure agent has access to all four madhhab rulings.

Tags: #islamic-content #madhhab #fiqh #quality-escalation
```

### 9.5 Feedback to Originating Agent

Every escalated task that returns to the originating agent includes a **Resolution Feedback Packet**:

```json
{
  "feedback_to_agent": {
    "from_handler": "DMGR-CONTENT-PROD-007",
    "message": "Your content was good but lacked comparative madhhab context. In the future, when confidence < 0.85 for theological content, request the comparative madhhab API before escalating.",
    "learning_resource": "KB-20260709-001",
    "behavior_change": "For all Islamic content with theological dimension, attach a note on the madhhab school used as the primary source.",
    "score_adjustment": "+0.2 quality score for future tasks (add comparative madhhab context)"
  }
}
```

### 9.6 Escalation Closure Report

Final closure report automatically generated for every escalation:

```
═══════════════════════════════════════════════
ESCALATION CLOSURE REPORT
═══════════════════════════════════════════════
ID:               ESC-20260709-A7F3-99201
Type:             Quality Escalation
Initiated By:     AGENT-ISLAMIC-CONTENT-V4-00142
Resolved By:      DMGR-CONTENT-PROD-007
Level (Final):    L3
Total Time:       205s (within SLA of 300s)
Resolution:       Content approved with additional note

SLA Compliance:
  L1: Met (30s SLA → 12s)
  L2: Met (120s SLA → 89s)
  L3: Met (300s SLA → 104s at level, 205s total)

Prevention Actions Created: 3
Knowledge Base Articles:    1
RCA Completed:             Yes

Lessons Learned:
  - Confidence thresholds need calibration per content category
  - L2 team leads need access to comparative resources
  - Automated fiqh module would reduce escalation frequency

Close Reason:             Resolved at level
Final Status:            Closed
═══════════════════════════════════════════════
```

---

## 10. Escalation Thresholds (Detailed Numeric)

### 10.1 Task Timeout Thresholds

| Agent Type | Estimated Duration | Max Duration (2x) | Retry Budget | L1 Action | L2 Escalation |
|---|---|---|---|---|---|
| Content Generation (Short) | 10s | 20s | 3 retries | Retry with exponential backoff | 20s elapsed + 3 retries exhausted = L2 |
| Content Generation (Long) | 60s | 120s | 3 retries | Retry with fallback model | 120s + 3 retries = L2 |
| QA Validation | 5s | 10s | 2 retries | Retry | 10s + 2 retries = L2 |
| Islamic Content Verification | 15s | 30s | 3 retries | Retry, switch to secondary verifier | 30s + 3 retries = L2 |
| API Dependency | 2s | 5s | 3 retries | Retry with different version | 5s + 3 retries = L2 |
| Data Processing | 120s | 240s | 2 retries | Reduce batch size | 240s + 2 retries = L2 |
| Search/Query | 3s | 6s | 3 retries | Switch index | 6s + 3 retries = L2 |
| Translation | 30s | 60s | 2 retries | Retry | 60s + 2 retries = L2 |
| Transcription | 45s | 90s | 2 retries | Retry | 90s + 2 retries = L2 |

**Exponential Backoff Formula for Retries:**
```
retry_delay = base_delay × 2^(attempt_number - 1) + random_jitter(0, 100ms)
base_delay = min(estimated_duration × 0.1, 1000ms)
attempt_number = 1, 2, 3 (max 3 attempts)

Example (10s task):
  Attempt 1: delay = 1000ms × 2^0 = 1s + jitter
  Attempt 2: delay = 1000ms × 2^1 = 2s + jitter
  Attempt 3: delay = 1000ms × 2^2 = 4s + jitter
  Total retry budget: 1s + 2s + 4s = 7s + jitter
```

### 10.2 Error Rate Thresholds

| Error Type | Trigger Condition | Escalation Level | Action |
|---|---|---|---|
| General application error | > 5% in 1-minute window | L2 | Team lead investigates root cause |
| General application error | > 10% in 1-minute window | L3 | Department manager, may pause service |
| General application error | > 20% in 1-minute window | L4 | Executive, platform-wide escalation |
| API failure rate | > 3% in 1-minute window | L2 | Team lead reviews API configuration |
| API failure rate | > 7% in 1-minute window | L3 | Department manager, consider API migration |
| API failure rate | > 15% in 1-minute window | L4 | Executive, emergency API failover |
| Database error | > 2% in 1-minute window | L2 | Team lead reviews DB health |
| Database error | > 5% in 1-minute window | L3 | Department manager, scale DB |
| Database error | > 10% in 1-minute window | L4 | CTO executive, incident response |
| Security event | Any single confirmed breach | L4 (bypass) | Executive security response |
| Authentication failure | > 10% of auth attempts failing | L2 | Team lead review |
| Authentication failure | > 25% of auth attempts failing | L3 | Security review |
| QC/QE health check | 3 consecutive failures | L2 | Team lead review |

**Error Rate Calculation:**
```
error_rate = (number_of_errors_in_window / number_of_total_operations_in_window) × 100
window_size = 60 seconds (sliding window)
min_operations_to_trigger = 10 (do not trigger on < 10 operations)
```

### 10.3 Quality Score Thresholds

| Quality Dimension | L1 Threshold | L1 Action | L2 Threshold | L2 Action | L3 Threshold | L3 Action |
|---|---|---|---|---|---|---|
| Theological Accuracy | < 0.85 | Regenerate with stricter prompt | < 0.80 | L2 review | < 0.70 | L3 content manager |
| Grammar/Spelling | < 0.90 | Regenerate | < 0.80 | L2 review | < 0.70 | L3 content manager |
| Completeness | < 0.85 | Regenerate with structural template | < 0.75 | L2 review | < 0.65 | L3 content manager |
| Readability | < 0.80 | Regenerate with simpler language | < 0.70 | L2 review | < 0.60 | L3 content manager |
| Relevance | < 0.90 | Regenerate with better context | < 0.80 | L2 review | < 0.70 | L3 content manager |
| Safety | < 1.0 | Block content, L2 review | < 1.0 | L3 safety review | < 1.0 | L4 safety executive |
| Islamic Doctrinal | < 0.95 | Block content, L2 Islamic TL | < 0.92 | L3 content manager | < 0.90 | L4 Head of Islamic Content |

### 10.4 SLA Breach Thresholds

| SLA Progression | Percentage Elapsed | Level | Action |
|---|---|---|---|
| Green | < 50% | L1 | No action, monitor |
| Yellow warning | ≥ 50% | L1 | Flag in dashboard, agent may request L2 assistance early |
| Orange caution | ≥ 75% | L1 → L2 | Flag to L2, agent should consider escalation |
| Red imminent | ≥ 90% | L2 → L3 | Force escalation to L2 (or L3 if L2 already assigned) |
| Breach | = 100% | L3+ | Escalate to next level automatically |

**SLA Breach Escalation Examples:**

| Scenario | Estimated Time | Elapsed Time | % Elapsed | Action |
|---|---|---|---|---|
| Short content | 10s | 5+ | 50% | Yellow — agent may request assistance |
| Short content | 10s | 7.5s | 75% | Orange — consider escalation to L2 |
| Short content | 10s | 9s | 90% | Red — auto-escalate to L2 |
| Islamic content | 15s | 7.5s | 50% | Yellow — flag |
| Islamic content | 15s | 11.25s | 75% | Orange — escalate to L2 Islamic TL |
| Islamic content | 15s | 13.5s | 90% | Red — auto-escalate to L3 Content Manager |

### 10.5 Security Event Thresholds

| Event Type | Threshold | Escalation Level | Response Time Target |
|---|---|---|---|
| Confirmed data exfiltration | 1 event | L4 (Security Executive) via bypass | < 1s |
| Confirmed unauthorized access | Any confirmed | L4 (Security Executive) via bypass | < 1s |
| Suspected data access anomaly | > 3x normal access rate for agent | L3 → L4 (Security) | < 30s |
| Authentication brute force | > 10 failed attempts/min | L3 (Security Manager) | < 60s |
| SQL/NoSQL injection detected | 1 event | L3 (Security Manager) | < 30s |
| Malicious file upload attempt | 1 event | L3 (Security Manager) | < 30s |
| Agent behavioral anomaly | Deviation > 2 sigma from baseline | L2 (Team Lead) | < 120s |
| Cross-department access without authorization | 1 event | L3 (Department Manager) | < 60s |

**Security Beacon:** Every agent emits a heartbeat and behavior beacon every 30 seconds. The security monitoring system evaluates beacon data in real-time against behavioral baselines.

---

## 11. Escalation Anti-Patterns

### 11.1 Escalation Flooding

**Definition:** Agents escalate trivial issues that should be self-resolved or handled via normal handover, overwhelming higher levels with noise.

**Detection:**
- An agent escalates > 10% of their tasks as L2+ issues.
- An agent's L1 self-resolution rate is < 70%.
- A specific agent type has an escalation rate > 2x the team average.
- More than 30% of an agent's escalations are categorized as "trivial" (severity 1, resolved at higher level without significant action).

**Detection Logic:**
```json
{
  "agent_id": "AGENT-CONTENT-V3-291",
  "total_tasks_24h": 1000,
  "total_escalations_24h": 150,
  "escalation_rate": "15%",
  "team_average_escalation_rate": "5%",
  "trivial_escalation_rate": 12,
  "trivial_escalation_threshold_violation": true,
  "detected_pattern": "ESCALATION_FLOODING",
  "alert_level": "L2 (team lead should review agent's training and configuration)"
}
```

**Prevention:**
- **Escalation budget:** Each agent has a daily escalation budget. Default: 5% of tasks may escalate. If exceeded, the agent is warned. If exceeded 3 days in a row, the agent is placed in "probation" mode and requires L2 approval for all further escalations.
- **Escalation cost:** Each escalation incurs a "cost" (virtual token). Agents with limited token pools cannot escalate trivial issues — the system learns to prioritize.
- **Feedback loop:** Agents that escalate trivial issues receive automated feedback: "This issue was resolved without any change by L2. Consider self-resolving similar issues."

**Penalty:**
| Infraction | Consequence |
|---|---|
| 1st flooding detection in 24h | Warning to agent + config update |
| 2nd flooding detection | Agent placed in probation (L2 approval for escalations) for 4 hours |
| 3rd detection | Automatic training module triggered + L2 review of agent's escalation logic |
| Pattern persists over 1 week | Agent sent for configuration review and possible model update |

### 11.2 Escalation Avoidance

**Definition:** Agents that fail to escalate issues that legitimately require higher-level authority or expertise, allowing problems to persist or worsen.

**Detection:**
- An agent's escalation rate is < 0.5% of tasks (when the team average is 5%).
- The number of unresolved tasks grows for an agent without escalation.
- An agent whose tasks are later found to have errors that would have been caught at higher levels.
- An agent that repeatedly times out on the same type of task without escalating.
- User complaints about content quality from a specific agent that never escalates quality issues.

**Detection Logic:**
```json
{
  "agent_id": "AGENT-ISLAMIC-CONTENT-V4-312,
  "total_tasks_24h": 1200,
  "total_escalations_24h": 3,
  "escalation_rate": "0.25%",
  "team_average_escalation_rate": "4.5%",
  "unresolved_tasks": 45,
  "user_complaints_rate": "3.2%",
  "team_average_complaints": "0.5%",
  "detected_pattern": "ESCALATION_AVOIDANCE",
  "action_level": "L2 (team lead should review agent's decision thresholds)"
}
```

**Prevention:**
- **Confidence calibrations:** Agents must report confidence for every task. If confidence < 0.6 and no escalation, the system flags it.
- **Peer review:** Random sampling of 1% of all tasks is sent for peer quality review. If the peer identifies issues the original agent should have escalated, it's flagged.
- **User feedback correlation:** Agents whose tasks generate negative user feedback but never escalated are investigated.

**Penalty:**
1st detection: Agent receives automatic re-calibration with higher escalation sensitivity.
2nd detection in 48h: Agent placed in "default escalating" mode — must explicitly confirm "no escalation needed" for any low-confidence task.
3rd detection: Agent replaced with updated model + L2 review of performance.

### 11.3 Escalation Ping-Pong

**Definition:** Two or more agents (typically at the same level or adjacent levels) continuously pass an escalation back and forth without taking ownership and resolving.

**Detection:**
- An escalation record shows 3+ transfers between the same two agents without resolution.
- Total transfers > 4 between L2 and L3 on the same issue.
- The same issue is escalated, resolved at L3, then de-escalated and re-escalated within 10 minutes.
- The resolution notes at each level say "this should be handled by X level" without actual resolution action.

**Detection Logic:**
```json
{
  "escalation_id": "ESC-20260709-PONG-001",
  "transfer_count": 5,
  "transfers_between": ["TL-FIQH-EDU-003", "DMGR-CONTENT-PROD-007"],
  "total_time": "3m 22s",
  "resolution_at_final_level": "L4 (issues that ping-pong between L2/L3 are automatically escalated to L4 after 3 transfers)",
  "detected_pattern": "ESCALATION_PING_PONG",
  "action_level": "Automatic escalation to L4 with notation"
}
```

**Prevention:**
- **Transfer limit:** Maximum 2 transfers between the same two levels. After 2 transfers (i.e., 3rd assignment to same level), the escalation is automatically raised to the next level.
- **Owner assignment:** The receiver must enter "accept" or "escalate" within SLA — "close without action" is not permitted for escalations.
- **Ping-pong detection algorithm:** SignalR event stream monitors transfers per escalation. If > 3 transfers in < 10 minutes, auto-escalate one level.

**Penalty:**
- Both agents involved receive a "ping-pong violation" on their performance record.
- After 3 ping-pong violations in 7 days, both agents are subject to a joint review by L4.

### 11.4 Escalation Black Hole

**Definition:** An escalation is sent to a level but no agent takes ownership, and the escalation timer expires without resolution. The escalation "disappears" — no longer tracked, no one acknowledges.

**Detection:**
- More than 2 escalations in 1 hour where no agent acknowledged within SLA on a specific team.
- Escalation status shows "no_handler_assigned" for more than 2 minutes.
- A specific department has > 5% of its escalations "unassigned" or "black_hole" in a day.
- Escalation dashboard shows entries in "timeout without resolution" state without re-escalation.

**Detection Logic:**
```json
{
  "escalation_id": "ESC-20260709-BH-001",
  "current_level": "L3",
  "acknowledged": false,
  "elapsed_without_ack": "120s",
  "handler_detected": false,
  "no_handler_assigned": true,
  "auto_escalated": false,
  "detected_pattern": "ESCALATION_BLACK_HOLE",
  "action": "Force escalation to L4 + notify CE-AI of black hole event"
}
```

**Prevention:**
- **Handler detection heartbeat:** All handlers at L2+ must emit a heartbeat every 5 seconds. If no heartbeat for 15 seconds, the handler's escalations are automatically re-assigned to the next available handler at the same level.
- **Shadow handler:** Every escalation level has a shadow handler — a backup agent that automatically takes over if the primary doesn't acknowledge within 50% of the ack SLA.
- **Dashboard escalation:** If an escalation reaches 75% of its resolution SLA without resolution, the ECC dashboard highlights it with a pulsing red indicator and sends an alert to the handler's supervisor.

**Penalty:**
- Black hole events are the most serious anti-pattern. An agent with an unacknowledged escalation receives:
  - Immediate performance review (score penalty of -5% for black hole event).
- Department with > 3 black hole events in 24 hours: automatic L4 notification and department-wide review.

### 11.5 Escalation Spam

**Definition:** An agent or system component that generates a large number of duplicate or near-duplicate escalations for the same underlying issue, overwhelming the system.

**Detection:**
- More than 3 escalations from the same agent for the same task_id within 5 minutes.
- More than 10 escalations in 5 minutes for the same issue category from the same department.
- Identical context_summary in escalations from different agents (indicating a shared root cause being independently escalated).

**Prevention:**
- **Deduplication:** Before creating a new escalation, the system checks for existing open escalations with matching task_id, agent_id, or category in the past 2 minutes. If a match is found, the new request is merged as an additional context to the existing escalation.
- **Rate limiting:** No single agent may generate more than 5 escalations per minute. If exceeded, the agent's escalation privileges are paused for 60 seconds.
- **Throttling:** If the system-wide escalation rate exceeds 100 escalations/minute (L2+), a "storm circuit breaker" activates, pausing all but highest-severity escalations until the rate drops below 50/minute.

---

## 12. Human Escalation (L6)

### 12.1 When Humans Get Involved

L6 (Human Review Board) is the highest escalation level. Humans are only engaged for:

| Category | Examples | Minimum Severity | Urgency |
|---|---|---|---|
| **Islamic Doctrinal Disputes** | A ruling that differs significantly from established Islamic teachings; new or unprecedented fatwa question; disagreement with accredited scholars; content that may cause sectarian tension | ≥ 4 | ≥ 3 |
| **Legal/Liability Issues** | User data privacy lawsuit threat; regulatory non-compliance; terms of service violation with external party; platform banned in jurisdiction; intellectual property dispute | ≥ 4 | ≥ 4 |
| **Financial Decisions** | Budget > $50K (CE-AI limit); pricing model change; partnership decisions with financial commitment; vendor contracts | ≥ 4 | ≥ 3 |
| **Safety Crises** | Content inciting violence or hate; platform used for illegal activities; child safety concerns; radicalization detection | ≥ 5 | ≥ 5 |
| **System Shutdown/Recovery** | Complete platform shutdown; data center disaster recovery; data loss recovery requiring human authorization | ≥ 5 | ≥ 5 |
| **Chief Ethical Dilemmas** | AI decisions with no clear non-harmful path; conflict between safety and user experience; ethical trade-offs | ≥ 4 | ≥ 3 |
| **Changes to Core Platform Principles** | Modifications to the company mission, ethical guidelines, basic teaching approach | N/A (strategic) | N/A (strategic) |
| **CE-AI Confidence Below Threshold** | L5 CE-AI encounters an issue where its own confidence in resolution is < 0.85 | Any severity | Any urgency |

**Rule: Human Escalation ONLY for High Severity/High Impact.**
When an escalation reaches L5, the CE-AI has 60 seconds to either:
- Resolve the issue (if confidence ≥ 0.90 with identifiable policy or precedent)
- Escalate to L6 (if confidence < 0.90 or the issue falls into the categories above)

### 12.2 Human Review Queue Management

The Human Review Queue (HRQ) is managed by the **Human Escalation Management System (HEMS)** .

**Queue Structure:**
```
┌─────────────────────────────────────────────────────────────────────┐
│ HUMAN REVIEW QUEUE                    Pending: 4 │ In Review: 2 │
├─────────────────────────────────────────────────────────────────────┤
│ Priority │ ID │ Category │ L5 Confidence │ Duration │ Assigned To   │
│──────────┼────┼──────────┼──────────────┼──────────┼───────────────┤
│ P0 │ 001 │ Theology  │ 0.82 │ 2m 41s │ Scholar-A         │
│ P0 │ 002 │ Legal     │ 0.78         │ 1m 20s │ Legal Counsel-B │
│ P1 │ 003 │ Technical │ 0.85         │ 4m 12s │ Tech Director-C │
│ P2 │ 004 │ Financial │ 0.91         │ 6m 33s │ (unassigned)    │
│ ... │ ... │ ...      │ ...          │ ...    │ ...              │
├─────────────────────────────────────────────────────────────────────┤
│ AVERAGE TIME: 12m 45s  │  LONGEST WAITING: 33m 14s (P2)            │
│ SLA COMPLIANCE: 82% (24hr target)                                 │
└─────────────────────────────────────────────────────────────────────┘
```

**Priority Levels:**
| Priority | Description | Service Objective | Auto-Notify |
|---|---|---|---|
| P0 | Crisis/Critical | Response within 5min; resolution within 2hrs | All L6 members + phone/email page |
| P1 | High | Response within 15min; resolution within 4hrs | Appropriate L6 member |
| P2 | Medium | Response within 1hr; resolution within 8hrs | Email notification |
| P3 | Low | Response within 4hrs; resolution within 24hrs | Email (digest) |

### 12.3 Prioritization Among Human Escalations

The queue prioritizes based on a weighted scoring algorithm:

```
Priority_Score =
  (severity_weight × severity_score) +
  (urgency_weight × urgency_score) +
  (duration_weight × waiting_time_ratio) +
  (impact_weight × impact_score)
  - (already_in_review_count × 50)  // avoid queue hogging by same reviewer

Where:
  severity_weight = 50, urgency_weight = 30, duration_weight = 15, impact_weight = 5
  severity_score: 5 = 100, 4 = 80, 3 = 60, 2 = 40, 1 = 20
  urgency_score: 5 = 100, 4 = 80, 3 = 60, 2 = 40, 1 = 20
  waiting_time_ratio: min(waiting_minutes / 60, 1) × 100
  impact_score: user_facing = 80, system_critical = 90, content_critical = 100
```

**Priority Example:**
```
Item 001: Severity 5, Urgency 4, Waiting 15min, Impact Content-Critical
Score = (50*100) + (30*80) + (15*25) + (5*100) = 5000 + 2400 + 375 + 500 = 8275 → P0

Item 002: Severity 3, Urgency 3, Waiting 30min, Impact User-Facing
Score = (50*60) + (30*60) + (15*50) + (5*80) = 3000 + 1800 + 750 + 400 = 5950 → P1
```

### 12.4 Human Response Template

When a human responds to an L6 escalation, they must use the following structured response template:

```json
{
  "escalation_id": "ESC-20260709-L6-001",
  "human_reviewer": {
    "name": "Dr. Ahmad al-Rashid",
    "title": "Senior Islamic Scholar",
    "human_id": "L6-HUMAN-ISLAMIC-SCHOLAR-001",
    "session_started_at": "2026-07-09T15:00:00Z"
  },

  "preliminary_finding": {
    "summary": "The issue involves a disagreement between the Shafi'i and Hanbali schools regarding the validity of a specific prayer practice. The difference is a matter of fiqh Ijtihad, not aqidah. Both views are considered valid within their respective schools.",
    "theological_assessment": {
      "is_doctrinal_emergency": false,
      "no_contradiction_to_quran_sunnah": true,
      "ijtihad_scope": true,
      "requires_curriculum_update": false,
      "referenced_sources": ["Sahih Muslim, Book 4, Hadith 123", "Al-Majmu, Imam al-Nawawi, Vol 4, p. 256-261"]
    }
  },

  "decision": {
    "type": "approve_with_guidance",
    "ruling": "Content is acceptable. Both scholarly opinions are valid. Content should present the majority view (Shafi'i) first and note the Hanbali position as an alternative for those who follow that school.",
    "additional_instructions": "Add a link to the Comparative Madhhab Module for students who wish to explore different rulings.",
    "override_policy": true,
    "override_detail": "Add new policy: For comparative madhhab content, the primary source must be identified and alternative views must be attributed to their respective schools."
  },

  "system_rule_generation": {
    "create_knowledge_base": true,
    "kb_title": "Presenting Comparative Madhhab Content",
    "kb_content": "[Full text derived from human decision]",
    "update_escalation_threshold": false,
    "update_quality_threshold": true,
    "new_quality_threshold": "For comparative madhhab content, theological accuracy must be ≥ 0.95 and include explicit school attribution."
  },

  "review_completed_at": "2026-07-09T15:45:00Z"
}
```

### 12.5 Converting Human Decision into System Rule

Once a human renders a decision on an L6 escalation, the system must **encode that decision into a rule or policy** to prevent the same issue from requiring human intervention again.

**Conversion Process:**

```
Human Decision → System Rule Pipeline:

1. Extract decision parameters
   ├─ Rule type: content_policy | quality_threshold | escalation_threshold | authorization_scope
   ├─ Rule scope: single_agent | team | department | company
   ├─ Rule condition: IF <condition> THEN <action>
   ├─ Rule priority: low | medium | high | critical
   └─ Rule: "For comparative madhhab content, theological accuracy must be ≥ 0.95"

2. Validate rule
   ├─ Check: Does rule conflict with existing rules?
   ├─ Check: Is rule syntactically and internally consistent?
   ├── Check: Can rule be applied automatically without human review?
   └─ Result: Rule validated (no conflicts, auto-applicable)

3. Publish rule
   ├─ Add to policy engine (global rules database)
   ├─ Tag with version, timestamp, human author
   ├─ Mark as "L6_derived" for audit purposes
   ├─ Update agent configuration cache
   └─ Notify affected agents: "New policy rule: Comparative Madhhab Content. Tag: L6-20260709-001"

4. Test rule
   ├─ Run 100 test cases against the existing dataset
   ├─ Verify rule catches all cases where it should trigger
   ├─ Verify rule does not trigger falsely
   ├─ Measure precision and recall
   └─ Result: Precision: 98%, Recall: 96%

5. Deploy rule
   ├─ Rollout to 10% of agents → monitor 1 hour
   ├─ Rollout to 50% of agents → monitor 1 hour
   ├─ Rollout to 100% of agents
   ├─ Create audit entry
   └─ Close escalation with note: "System rule KB-20260709-001 deployed."
```

**Rule Lifecycle:**
```
Human Decision → Rule Draft → Validation → Testing → Staged Rollout → Live → Review (30 days)
                                                                                │
                                                                          If successful → Keep
                                                                          If issues → Revise or Revert
```

### 12.6 Human Available Hours and Shift Management

| Day | Working Hours | On-Call Hours | Review Board Available |
|---|---|---|---|
| Sunday (Sun) | 08:00 – 18:00 | 18:00 – 08:00 next day | Full board: 08:00–14:00, Partial: 14:00–18:00 |
| Monday | 08:00 – 18:00 | 18:00 – 08:00 | Full board |
| Tuesday | 08:00 – 18:00 | 18:00 – 08:00 | Full board |
| Wednesday | 08:00 – 18:00 | 18:00 – 08:00 | Full board |
| Thursday | 08:00 – 18:00 | 18:00 – 08:00 | Full board |
| Friday | 08:00 – 14:00 | 14:00 – 08:00 Saturday | Partial (reduced) |
| Saturday | 08:00 – 12:00 | 12:00 – 08:00 Sunday | Minimum (1 scholar, 1 legal, 1 director) |

**On-Call Rotation:**
- One Islamic scholar on call 24/7 (rotates weekly among 3 scholars).
- One legal counsel on call during working hours + extended to emergency only during off-hours.
- One executive director on call 24/7 (rotates monthly among 2 directors).

---

## Appendices

### Appendix A: Escalation Flow Diagram (Text)

```
                          START
                            │
                    ┌───────▼──────────┐
                    │  Issue Detected   │
                    │  by Agent (L1)    │
                    └─────────┬─────────┘
                              │
                    ┌─────────▼─────────┐
                    │ Can Self-Resolve? │
                    │(30s SLA, 3 retries)│
                    └─────────┬─────────┘
                    YES        │        NO
                    ┌──────────┘        │
                    ▼                   ▼
                RESOLVED        ┌───────────────────┐
                                │ Escalate to L2    │
                                │ (Team Lead)       │
                                └─────────┬─────────┘
                                          │
                                ┌─────────▼──────────┐
                                │ Can Team Resolve?  │
                                │ (5s ack, 2min SLA) │
                                └─────────┬──────────┘
                                YES       │              NO
                               ┌──────────┘              │
                               ▼                          │
                           RESOLVED            ┌──────────▼──────────┐
                                                │ Escalate to L3     │
                                                │ (Dept Manager)     │
                                                └──────────┬─────────┘
                                                          │
                                                ┌─────────▼──────────┐
                                                │ Dept Resolve?      │
                                                │ (10s ack, 5min SLA)│
                                                └─────────┬──────────┘
                                                YES        │           NO
                                               ┌──────────┘           │
                                               ▼                       │
                                          RESOLVED           ┌─────────▼─────────┐
                                                             │ Escalate to L4    │
                                                             │ (Executive)        │
                                                             └──────────┬────────┘
                                                                        │
                                                             ┌─────────▼──────────┐
                                                             │ Executive Resolve? │
                                                             │ (30s ack, 15min)  │
                                                             └─────────┬──────────┘
                                                             YES        │          NO
                                                            ┌──────────┘          │
                                                            ▼                     │
                                                       RESOLVED          ┌────────▼────────┐
                                                                         │ Escalate to L5 │
                                                                         │ (CE-AI)        │
                                                                         └────────┬───────┘
                                                                                  │
                                                                         ┌───────▼─────────┐
                                                                         │ CE-AI Resolve?  │
                                                                         │ (60s ack, 1hr) │
                                                                         └───────┬─────────┘
                                                                        YES        │          NO
                                                                      ┌──────────┘          │
                                                                      ▼                     │
                                                                  RESOLVED        ┌─────────▼─────────┐
                                                                                   │ Escalate to L6   │
                                                                                   │ (Human Review    │
                                                                                   │  Board)          │
                                                                                   └─────────┬─────────┘
                                                                                             │
                                                                                   ┌─────────▼─────────┐
                                                                                   │ Human Decision    │
                                                                                   │ (5min ack, 24h)  │
                                                                                   └─────────┬─────────┘
                                                                                             │
                                                                                    ┌────────▼────────┐
                                                                                    │ Convert to     │
                                                                                    │ System Rule    │
                                                                                    │ → RESOLVED     │
                                                                                    └─────────────────┘
```

### Appendix B: Escalation Performance Metrics

| Metric | Description | Target | Measurement |
|---|---|---|---|
| Escalation Rate | % of tasks that escalate beyond L1 | < 5% | escalations_L2+ / total_tasks |
| L1 Resolution Rate | % of issues resolved at L1 | > 90% | L1_resolved / total_issues |
| SLA Compliance by Level | % of escalations resolved within SLA per level | L2: > 95%, L3: > 95%, L4: > 90%, L5: > 85%, L6: > 80% | resolved_within_sla / total_escalations_per_level |
| Mean Time to Resolution (MTTR) | Average time to resolve at each level | L2 < 90s, L3 < 4min, L4 < 10min, L5 < 45min, L6 < 12hrs | avg(resolution_time_per_level) |
| Escalation Flooding Rate | % of escalations that are trivial (severity 1) | < 10% of total | L2_trivial_escalations / total_escalations |
| Escalation Avoidance Rate | % of tasks with errors that should have escalated | < 1% | error_tasks_not_escalated / total_error_tasks |
| Ping-Pong Rate | % of escalations transferred 3+ times | < 2% | ping_pong_escalations / total_escalations |
| Black Hole Rate | % of escalations that time out without handler | 0% | black_hole_escalations / total_escalations |
| Human Escalation Rate | % of escalations reaching L6 | < 0.1% | L6_escalations / total_escalations |
| Human Resolution Rate | % of human decisions converted to rules | > 90% | rules_created / L6_decisions |

### Appendix C: Escalation Configuration Parameters

```json
{
  "global_escalation_config": {
    "max_retries_per_level": 3,
    "retry_base_delay_ms": 500,
    "retry_max_delay_ms": 5000,
    "max_same_level_transfers": 2,
    "escalation_aging_ttl_days": 90,
    "escalation_archive_ttl_days": 730,
    "rate_limit_per_agent_per_minute": 5,
    "system_rate_limit_per_minute": 100,
    "circuit_breaker_threshold": 100,
    "circuit_breaker_duration_ms": 60000,
    "dedup_match_window_ms": 120000,
    "max_chain_length": 6,
    "allow_emergency_bypass": true,
    "support_l6_during_off_hours": true,
    "l6_on_call_scholar_rotation_days": 7
  },
  "level_configs": {
    "L2": {
      "ack_sla_ms": 5000,
      "resolution_sla_ms": 120000,
      "grace_period_ms": 0,
      "max_concurrent_escalations": 10,
      "heartbeat_interval_ms": 5000
    },
    "L3": {
      "ack_sla_ms": 10000,
      "resolution_sla_ms": 300000,
      "grace_period_ms": 0,
      "max_concurrent_escalations": 15,
      "heartbeat_interval_ms": 5000
    },
    "L4": {
      "ack_sla_ms": 30000,
      "resolution_sla_ms": 900000,
      "grace_period_ms": 0,
      "max_concurrent_escalations": 5,
      "heartbeat_interval_ms": 5000
    },
    "L5": {
      "ack_sla_ms": 60000,
      "resolution_sla_ms": 3600000,
      "grace_period_ms": 0,
      "max_concurrent_escalations": 3,
      "heartbeat_interval_ms": 5000
    },
    "L6": {
      "ack_sla_ms": 300000,
      "resolution_sla_ms": 86400000,
      "ack_escalation_threshold": 600000,
      "working_hours_start": "06:00",
      "working_hours_end": "20:00",
      "working_days": ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"],
      "reduced_days": ["Friday"],
      "sabbath_days": []
    }
  }
}
```

### Appendix D: Escalation Terminology Glossary

| Term | Definition |
|---|---|
| **Escalation** | Upward transfer of responsibility due to capability/authority/timeout/error threshold exceeded |
| **Handover** | Lateral peer-to-peer transfer within same hierarchical level |
| **Ack SLA** | Maximum time allowed for a handler to acknowledge receipt of an escalation |
| **Resolution SLA** | Maximum time allowed to resolve the issue at a given escalation level |
| **SLA Breach** | When the resolution time exceeds the SLA for the current level |
| **Emergency Bypass** | Direct escalation to L4/L5 skipping intermediate levels, permitted only for defined critical conditions |
| **Escalation Chain** | Sequential log of each level that handled the escalation, including timestamps and decisions |
| **Ping-Pong** | Anti-pattern where the same escalation is transferred between levels without resolution |
| **Black Hole** | Anti-pattern where an escalation is not acknowledged within SLA by any handler |
| **Shadow Handler** | Backup agent that automatically assumes responsibility if primary handler does not acknowledge within 50% of ack SLA |
| **HEMS** | Human Escalation Management System — manages L6 human review queue |
| **HRQ** | Human Review Queue — the queue of escalations awaiting human decision |
| **PMR** | Preventive Measure Recommendation — action item generated from escalation RCA |
| **KB** | Knowledge Base — article that documents resolution for future reference |
| **MTTR** | Mean Time to Resolution — average time to resolve escalations |

---

*End of Document 14 — Escalation Rules (14 of 24)*

*Next Document: **15. Incident Response Procedures***