# PERMISSIONS

## Access Control and Authorization Framework

---

| Document ID | AI-CORP-PERM-001 |
|---|---|
| Version | 1.0.0 |
| Status | Ratified |
| Classification | Enterprise Internal — Confidential |
| Effective Date | 2026-07-09 |
| Approving Authority | Office of the Chief Architect |
| Custodian | Security Department (D-SEC) |
| Responsibilities Reference | AI-CORP-RESP-001 |

---

## TABLE OF CONTENTS

1. EXECUTIVE SUMMARY
2. PERMISSION PHILOSOPHY
3. PERMISSION MODEL
4. ROLE-BASED ACCESS CONTROL
5. RESOURCE TYPES
6. ACTIONS
7. PERMISSION MATRIX
8. PERMISSION INHERITANCE
9. TEMPORARY PERMISSIONS
10. AUDIT TRAIL
11. APPENDICES

---

## 1. EXECUTIVE SUMMARY

### 1.1 Purpose

The Permissions System defines what every AI agent can and cannot do. It implements a strict Role-Based Access Control (RBAC) model with granular permissions at the resource-action level.

### 1.2 Core Principle

**Least privilege: every agent has only the permissions needed to do its job, nothing more.**

---

## 2. PERMISSION PHILOSOPHY

| Principle | Description |
|---|---|
| **Default Deny** | All actions are denied unless explicitly permitted |
| **Least Privilege** | Minimum permissions required for role |
| **Separation of Duties** | No agent can both create and approve the same content |
| **Permission Escalation** | Temporary elevation available with manager approval |
| **Audit Everything** | Every permission check logged |

---

## 3. PERMISSION MODEL

### 3.1 Permission Structure

```
┌─────────────────────────────────────────────────────────────────────────┐
│  Agent ←→ Role ←→ Permission ←→ Resource ←→ Action                    │
│                                                                          │
│  Example:                                                               │
│  Agent "wrk_str_001" has role "Story Writer"                           │
│  Role "Story Writer" has permission "write" on resource "story"        │
│  Result: Agent can create stories                                      │
└─────────────────────────────────────────────────────────────────────────┘
```

### 3.2 Permission Components

| Component | Description | Example |
|---|---|---|
| **Subject** | The agent requesting access | `agent_wrk_ocr_004521` |
| **Resource** | What is being accessed | `kg.fact`, `content.story` |
| **Action** | What is being done | `read`, `write`, `approve` |
| **Condition** | When access is allowed | `dept=kg`, `confidence>95` |
| **Effect** | Allow or Deny | `allow` |

---

## 4. ROLE-BASED ACCESS CONTROL

### 4.1 Role Hierarchy

| Level | Role | Permission Level |
|---|---|---|
| E-1 | CE-AI | Full system access |
| E-2 | Executive | Department-wide full access |
| E-3 | Department Manager | Department-wide operational access |
| M-1 | Team Lead | Team-wide operational access |
| S-1 | Senior Agent | Extended domain access |
| S-2 | Specialist | Specialized domain access |
| W-1 | Worker Agent | Standard task access |
| W-2 | Junior Agent | Limited guided access |
| U-1 | Utility Agent | Support access |
| P-1 | Support Agent | Maintenance access |

### 4.2 Default Permissions by Role Level

| Action | E-1 | E-2 | E-3 | M-1 | S-1 | S-2 | W-1 | W-2 | U-1 | P-1 |
|---|---|---|---|---|---|---|---|---|---|---|
| **Read** | All | All | Dept | Team | Domain | Domain | Task | Guided | Meta | Logs |
| **Write** | All | Dept | Dept | Team | Domain | Domain | Task | Guided | None | None |
| **Approve** | All | Dept | Team | Task | None | None | None | None | None | None |
| **Delete** | All | Dept | Team | None | None | None | None | None | None | None |
| **Manage** | All | Dept | Team | Team | None | None | None | None | None | None |
| **Escalate** | All | All | All | Team | Task | Task | Manager | Manager | Manager | Manager |

---

## 5. RESOURCE TYPES

### 5.1 Resource Catalog

| Resource Category | Resource Type | Examples |
|---|---|---|
| **Content** | `content.story` | Story drafts, published stories |
| **Content** | `content.quiz` | Quiz questions, quiz results |
| **Content** | `content.flashcard` | Flashcards, decks |
| **Content** | `content.podcast` | Scripts, audio files |
| **Content** | `content.image` | Illustrations, diagrams |
| **Knowledge** | `kg.fact` | Knowledge Graph facts |
| **Knowledge** | `kg.entity` | Knowledge Graph entities |
| **Knowledge** | `kg.relationship` | Knowledge Graph relationships |
| **Verification** | `vrf.result` | Verification outcomes |
| **Verification** | `vrf.scholar` | Scholar review data |
| **System** | `sys.config` | System configuration |
| **System** | `sys.deploy` | Deployment artifacts |
| **System** | `sys.metrics` | System metrics |
| **System** | `sys.logs` | System logs |
| **Security** | `sec.permission` | Permission definitions |
| **Security** | `sec.audit` | Audit logs |
| **User** | `user.profile` | Learner profiles |
| **User** | `user.progress` | Learner progress data |
| **Financial** | `fin.budget` | Budget data |
| **Financial** | `fin.cost` | Cost data |

---

## 6. ACTIONS

| Action | Description | Examples |
|---|---|---|
| **create** | Create a new resource | Create a story |
| **read** | View existing resource | Read a fact |
| **update** | Modify existing resource | Edit a quiz question |
| **delete** | Remove a resource | Delete an illustration |
| **approve** | Approve a pending resource | Approve a fact |
| **reject** | Reject a pending resource | Reject a story |
| **publish** | Make resource public | Publish a podcast |
| **archive** | Move to archive | Archive old content |
| **execute** | Run a process | Execute OCR pipeline |
| **configure** | Change configuration | Modify system settings |
| **delegate** | Assign to another agent | Delegate verification |
| **escalate** | Raise to higher level | Escalate an issue |

---

## 7. PERMISSION MATRIX

### 7.1 Content Department Permissions

| Resource \ Action | create | read | update | delete | approve | publish |
|---|---|---|---|---|---|---|
| `content.story` | W-1 | W-1 | W-1 | M-1 | M-1 | E-3 |
| `content.quiz` | W-1 | W-1 | W-1 | M-1 | M-1 | E-3 |
| `content.flashcard` | W-1 | W-1 | W-1 | M-1 | M-1 | E-3 |
| `content.podcast` | W-1 | W-1 | W-1 | M-1 | M-1 | E-3 |
| `content.image` | W-1 | W-1 | W-1 | M-1 | M-1 | E-3 |

### 7.2 Knowledge Department Permissions

| Resource \ Action | create | read | update | delete | approve |
|---|---|---|---|---|---|
| `kg.fact` | W-1 | W-1 | W-1 | M-1 | S-1 |
| `kg.entity` | W-1 | W-1 | W-1 | M-1 | S-1 |
| `kg.relationship` | W-1 | W-1 | W-1 | M-1 | S-1 |
| `vrf.result` | S-1 | W-1 | S-1 | M-1 | S-1 |
| `vrf.scholar` | S-1 | M-1 | E-3 | E-3 | E-3 |

### 7.3 System Department Permissions

| Resource \ Action | create | read | update | delete | configure |
|---|---|---|---|---|---|
| `sys.config` | E-3 | M-1 | E-3 | E-3 | E-3 |
| `sys.deploy` | DPL agents | M-1 | M-1 | E-3 | E-3 |
| `sys.metrics` | MON agents | W-1 | MON | E-3 | E-3 |
| `sys.logs` | All | All | None | E-3 | None |

### 7.4 Security Permissions

| Resource \ Action | read | modify | audit | manage |
|---|---|---|---|---|
| `sec.permission` | E-3+ | E-3+ | E-2+ | E-1 |
| `sec.audit` | E-2+ | None | E-2+ | E-1 |

---

## 8. PERMISSION INHERITANCE

### 8.1 Inheritance Rules

| Rule | Description | Example |
|---|---|---|
| **Hierarchical** | Manager inherits team permissions | Team Lead can do everything their team can |
| **Departmental** | Department-wide permissions extend to all teams | KG Manager can access all KG teams' data |
| **Escalation** | Escalated tasks grant temporary permissions | Senior Agent covering for absent lead |
| **Emergency** | Emergency mode grants elevated permissions | Security incident temporary access |

### 8.2 Inheritance Chain

```
CE-AI: Full system access
  └── Executive: Full department access
       └── Department Manager: Full team access
            └── Team Lead: All team member access
                 └── Senior Agent: Domain access
                      └── Worker Agent: Task access
```

---

## 9. TEMPORARY PERMISSIONS

| Type | Duration | Approval | Use Case |
|---|---|---|---|
| **Task Escalation** | Task duration | Manager auto-approve | Agent needs broader access for specific task |
| **Emergency Override** | 15 minutes | Executive approval | Security incident response |
| **Shadow Mode** | Until disabled | Manager approval | New agent observing |
| **Cross-Dept Transfer** | 7 days | Both managers | Agent covering another department |

---

## 10. AUDIT TRAIL

| Event | Logged Data | Retention |
|---|---|---|
| **Permission Check** | Agent, resource, action, result, timestamp | 90 days |
| **Permission Change** | Changed by, target, old value, new value | 7 years |
| **Access Denial** | Agent, resource, action, reason | 90 days |
| **Temporary Elevation** | Agent, duration, approver, reason | 1 year |

---

## 11. APPENDICES

### 11.1 Related Documents

| Document ID | Title |
|---|---|
| AI-CORP-RESP-001 | Responsibilities |
| AI-CORP-ESC-001 | Escalation Rules |
| AI-CORP-SEC-001 | Security Architecture |

### 11.2 Revision History

| Version | Date | Author | Changes |
|---|---|---|---|
| 1.0.0 | 2026-07-09 | Office of the Chief Architect | Initial ratified release |
