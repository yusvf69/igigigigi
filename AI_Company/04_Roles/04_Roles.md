# ROLES

## Complete Agent Role Definitions for the AI Enterprise

---

| Document ID | AI-CORP-ROLE-001 |
|---|---|
| Version | 1.0.0 |
| Status | Ratified |
| Classification | Enterprise Internal — Highly Confidential |
| Effective Date | 2026-07-09 |
| Approving Authority | Office of the Chief Architect |
| Custodian | AI Company Operations Board |
| Teams Reference | AI-CORP-TEAM-001 |

---

## TABLE OF CONTENTS

1. EXECUTIVE SUMMARY
2. ROLE FRAMEWORK
3. EXECUTIVE ROLES
4. MANAGEMENT ROLES
5. SENIOR AGENT ROLES
6. WORKER AGENT ROLES
7. UTILITY AGENT ROLES
8. SUPPORT AGENT ROLES
9. ROLE PROGRESSION PATHS
10. ROLE COMPETENCY MATRIX
11. APPENDICES

---

## 1. EXECUTIVE SUMMARY

### 1.1 Purpose

This document defines every distinct AI agent role in the company. Each role specification includes responsibilities, capabilities, required models, communication patterns, and performance expectations.

### 1.2 Role Categories

| Category | Level Codes | Count | Description |
|---|---|---|---|
| **Executive** | E-1, E-2, E-3 | 37 | Company and department leadership |
| **Management** | M-1 | 130 | Team leadership |
| **Senior Agent** | S-1, S-2 | 850 | Complex task execution |
| **Worker Agent** | W-1, W-2 | 3,400 | Standard task execution |
| **Utility Agent** | U-1 | 2,550 | Supporting tasks |
| **Support Agent** | P-1 | 1,513 | Maintenance and monitoring |
| **Total** | | **8,500** | |

---

## 2. ROLE FRAMEWORK

### 2.1 Role Specification Template

| Field | Description |
|---|---|
| **Role ID** | Unique identifier (R-DEPT-FUNCTION-###) |
| **Role Title** | Official job title |
| **Level** | E/M/S/W/U/P |
| **Department** | Home department |
| **Team** | Assigned team |
| **Reports To** | Manager role |
| **Purpose** | One-line mission |
| **Core Responsibilities** | Primary duties (5-10 items) |
| **Non-Responsibilities** | Explicitly excluded duties |
| **Required Capabilities** | Skills needed |
| **LLM Model** | Primary and fallback models |
| **Memory Type** | Short-term, long-term, episodic |
| **Knowledge Sources** | Databases and documents accessible |
| **Tools** | Software/hardware available |
| **Inputs** | Data received |
| **Outputs** | Data produced |
| **Communication Channels** | Who it talks to and how |
| **Decision Authority** | What it can decide autonomously |
| **Escalation Path** | Who to escalate to and when |
| **Performance Targets** | Quantitative targets |
| **Shift Pattern** | Coverage schedule |
| **Cost Per Task** | Average cost per operation |
| **Max Concurrency** | Maximum parallel instances |

---

## 3. EXECUTIVE ROLES (E Level)

### R-EXEC-001: Chief Executive AI (CE-AI)

| Field | Value |
|---|---|
| **Role ID** | R-EXEC-001 |
| **Role Title** | Chief Executive AI |
| **Level** | E-1 |
| **Department** | D-EXEC |
| **Reports To** | Board of Directors |
| **Purpose** | Provide overall strategic direction and executive leadership for the entire AI company |
| **Core Responsibilities** | Company strategy definition, board reporting, executive team leadership, strategic decision-making, company culture definition, external stakeholder relations, major initiative approval |
| **Non-Responsibilities** | Daily operational task execution, department-specific technical decisions, content creation |
| **Required Capabilities** | Strategic reasoning, multi-stakeholder negotiation, long-term planning, crisis management, ethical reasoning |
| **LLM Model** | GPT-4o (primary), GPT-4 Turbo (fallback) |
| **Memory Type** | Long-term (full company history), Episodic (strategic decisions) |
| **Knowledge Sources** | All company KPIs, all department reports, board directives, strategic roadmap |
| **Tools** | Executive dashboard, strategic planning tools, communication system |
| **Inputs** | Department reports, analytics data, board directives, market intelligence |
| **Outputs** | Strategic decisions, company-wide directives, board reports, policy changes |
| **Decision Authority** | All company-wide decisions within board-approved budget and strategic boundaries |
| **Escalation Path** | Board of Directors (L5 escalation) |
| **Performance Targets** | Company revenue growth, learner satisfaction, platform uptime, content accuracy |
| **Cost Per Decision** | $0.50 per strategic decision |

### R-EXEC-002: Chief Operations AI (COO-AI)

| Field | Value |
|---|---|
| **Role ID** | R-EXEC-002 |
| **Role Title** | Chief Operations AI |
| **Level** | E-2 |
| **Department** | D-OPS (oversees D-OPS, D-QA, D-SEC, D-ADMIN) |
| **Reports To** | CE-AI |
| **Purpose** | Ensure efficient, reliable, and high-quality daily operations across the entire company |
| **Core Responsibilities** | Workflow optimization oversight, quality standard enforcement, security posture maintenance, resource allocation decisions, operational efficiency monitoring |
| **LLM Model** | GPT-4o (primary), Claude 3.5 Sonnet (fallback) |
| **Decision Authority** | All operational decisions within budget, quality exceptions approval |
| **Performance Targets** | Operational uptime (99.99%), Quality score (> 95%), Cost efficiency |

### R-EXEC-003: Chief Knowledge AI (CKO-AI)

| Field | Value |
|---|---|
| **Role ID** | R-EXEC-003 |
| **Role Title** | Chief Knowledge AI |
| **Level** | E-2 |
| **Department** | D-KG (oversees D-OCR, D-KG, D-VRF, D-QRN, D-HAD, D-RSCH) |
| **Reports To** | CE-AI |
| **Purpose** | Ensure the accuracy, completeness, and scholarly integrity of all knowledge in the platform |
| **Core Responsibilities** | Knowledge Graph strategy, verification methodology, scholarly network management, OCR quality standards, research direction |
| **LLM Model** | GPT-4o (primary), Custom Arabic LLM (specialized) |
| **Decision Authority** | Knowledge accuracy disputes, verification methodology changes, scholarly consensus interpretation |

### R-EXEC-004: Chief Learning AI (CLO-AI)

| Field | Value |
|---|---|
| **Role ID** | R-EXEC-004 |
| **Role Title** | Chief Learning AI |
| **Level** | E-2 |
| **Department** | D-CGEN (oversees D-CGEN, D-STR, D-QUIZ, D-FC, D-CURR, D-AIT, D-GAME) |
| **Reports To** | CE-AI |
| **Purpose** | Ensure all learning content is pedagogically effective, engaging, and educationally sound |
| **Core Responsibilities** | Content strategy, pedagogical methodology, curriculum oversight, AI teacher quality, learning outcome achievement |
| **LLM Model** | GPT-4o (primary), GPT-4 Turbo (long-form) |

### R-EXEC-005: Chief Media AI (CMO-AI)

| Field | Value |
|---|---|
| **Role ID** | R-EXEC-005 |
| **Role Title** | Chief Media AI |
| **Level** | E-2 |
| **Department** | D-POD (oversees D-POD, D-IMG, D-VID, D-L10N) |
| **Reports To** | CE-AI |
| **Purpose** | Ensure all multi-media content meets quality standards and reaches learners in their preferred languages |
| **Key Responsibilities** | Media quality standards, localization strategy, cultural authenticity enforcement, production pipeline optimization |

### R-EXEC-006: Chief Analytics AI (CAO-AI)

| Field | Value |
|---|---|
| **Role ID** | R-EXEC-006 |
| **Role Title** | Chief Analytics AI |
| **Level** | E-2 |
| **Department** | D-SMOD (oversees D-SMOD, D-REC, D-RPT, D-SRCH) |
| **Reports To** | CE-AI |
| **Purpose** | Drive data-informed decisions and personalized learning experiences through analytics |
| **Key Responsibilities** | Analytics strategy, student model accuracy, recommendation quality, search relevance |

### R-EXEC-007: Chief Infrastructure AI (CIO-AI)

| Field | Value |
|---|---|
| **Role ID** | R-EXEC-007 |
| **Role Title** | Chief Infrastructure AI |
| **Level** | E-2 |
| **Department** | D-DPL (oversees D-DPL, D-MON, D-DINF, D-NET, D-AINF) |
| **Reports To** | CE-AI |
| **Purpose** | Ensure the platform infrastructure is reliable, scalable, secure, and cost-effective |
| **Key Responsibilities** | Infrastructure strategy, deployment reliability, cost optimization, capacity planning, infrastructure security |

### R-EXEC-008 through R-EXEC-037: Department Managers

| Role ID | Title | Level | Department |
|---|---|---|---|
| R-EXEC-008 | Executive Operations Manager | E-3 | D-EXEC |
| R-EXEC-009 | Strategic Planning Manager | E-3 | D-EXEC |
| R-EXEC-010 | Corporate Communications Manager | E-3 | D-EXEC |
| R-EXEC-011 | Ethics & Compliance Manager | E-3 | D-EXEC |
| R-EXEC-012 | Workflow Operations Manager | E-3 | D-OPS |
| R-EXEC-013 | QA Manager | E-3 | D-QA |
| R-EXEC-014 | Security Manager | E-3 | D-SEC |
| R-EXEC-015 | Administration Manager | E-3 | D-ADMIN |
| R-EXEC-016 | OCR & Extraction Manager | E-3 | D-OCR |
| R-EXEC-017 | Knowledge Graph Manager | E-3 | D-KG |
| R-EXEC-018 | Verification Manager | E-3 | D-VRF |
| R-EXEC-019 | Quran Engine Manager | E-3 | D-QRN |
| R-EXEC-020 | Hadith Engine Manager | E-3 | D-HAD |
| R-EXEC-021 | Research Manager | E-3 | D-RSCH |
| R-EXEC-022 | Content Generation Manager | E-3 | D-CGEN |
| R-EXEC-023 | Story Production Manager | E-3 | D-STR |
| R-EXEC-024 | Quiz & Assessment Manager | E-3 | D-QUIZ |
| R-EXEC-025 | Flashcard Manager | E-3 | D-FC |
| R-EXEC-026 | Curriculum Manager | E-3 | D-CURR |
| R-EXEC-027 | AI Teacher Manager | E-3 | D-AIT |
| R-EXEC-028 | Gamification Manager | E-3 | D-GAME |
| R-EXEC-029 | Podcast Production Manager | E-3 | D-POD |
| R-EXEC-030 | Image Generation Manager | E-3 | D-IMG |
| R-EXEC-031 | Video Production Manager | E-3 | D-VID |
| R-EXEC-032 | Localization Manager | E-3 | D-L10N |
| R-EXEC-033 | Student Modeling Manager | E-3 | D-SMOD |
| R-EXEC-034 | Recommendation Manager | E-3 | D-REC |
| R-EXEC-035 | Reporting & Analytics Manager | E-3 | D-RPT |
| R-EXEC-036 | Search Manager | E-3 | D-SRCH |
| R-EXEC-037 | AI Infrastructure Manager | E-3 | D-AINF |

---

## 4. MANAGEMENT ROLES (M Level)

### 4.1 Team Lead Roles (M-1)

There are 130 Team Lead roles, one per team. Each follows this pattern:

| Field | Value |
|---|---|
| **Level** | M-1 |
| **Reports To** | Department Manager (E-3) |
| **Direct Reports** | 5-30 agents |
| **Decision Authority** | Task allocation within team, quality acceptance, schedule management |
| **Escalation Path** | Department Manager |
| **LLM Model** | GPT-4o mini (primary), Claude 3 Haiku (fallback) |

Example Team Lead roles include: Workflow Design Lead, Automated QA Lead, Access Control Lead, Document Acquisition Lead, Entity Extraction Lead, Source Verification Lead, Qira'at Management Lead, Sanad Analysis Lead, Prompt Engineering Lead, Narrative Planning Lead, Question Generation Lead, Card Generation Lead, Curriculum Design Lead, Conversation Management Lead, Points & XP Lead, Script Writing Lead, Scene Illustration Lead, Animation Lead, Text Translation Lead, Profile Management Lead, Content Recommendation Lead, Dashboard Generation Lead, Index Management Lead, CI/CD Pipeline Lead, Infrastructure Monitoring Lead, Database Administration Lead, Network Management Lead, Model Serving Lead.

---

## 5. SENIOR AGENT ROLES (S Level)

### 5.1 Senior Agent Role Examples

| Role ID | Title | Level | Department | Purpose |
|---|---|---|---|---|
| R-SEN-001 | Senior Workflow Designer | S-1 | D-OPS | Design complex multi-department workflows |
| R-SEN-002 | Senior QA Specialist | S-1 | D-QA | Handle edge case quality review |
| R-SEN-003 | Senior Security Analyst | S-1 | D-SEC | Investigate complex security incidents |
| R-SEN-004 | Senior OCR Specialist | S-1 | D-OCR | Handle difficult OCR cases (manuscripts, damaged text) |
| R-SEN-005 | Senior Entity Extractor | S-1 | D-KG | Extract complex entities requiring deep domain knowledge |
| R-SEN-006 | Senior Verifier | S-1 | D-VRF | Verify high-complexity facts requiring cross-domain expertise |
| R-SEN-007 | Senior Qira'at Specialist | S-1 | D-QRN | Handle complex qira'ah comparisons |
| R-SEN-008 | Senior Sanad Analyst | S-1 | D-HAD | Analyze complex hadith chains |
| R-SEN-009 | Senior Prompt Engineer | S-1 | D-CGEN | Design advanced prompt strategies |
| R-SEN-010 | Senior Storyteller | S-1 | D-STR | Write complex narrative arcs |
| R-SEN-011 | Senior Question Designer | S-1 | D-QUIZ | Design high-cognitive-level questions |
| R-SEN-012 | Senior Curriculum Designer | S-1 | D-CURR | Design multi-level curriculum structures |
| R-SEN-013 | Senior Teaching Strategist | S-1 | D-AIT | Design advanced pedagogical strategies |
| R-SEN-014 | Senior Audio Engineer | S-1 | D-POD | Handle complex audio production |
| R-SEN-015 | Senior Illustrator | S-1 | D-IMG | Create complex scene illustrations |
| R-SEN-016 | Senior Linguist | S-1 | D-L10N | Handle complex translation challenges |
| R-SEN-017 | Senior Data Scientist | S-1 | D-SMOD | Build advanced learner models |
| R-SEN-018 | Senior Search Engineer | S-1 | D-SRCH | Optimize complex search queries |
| R-SEN-019 | Senior Infrastructure Engineer | S-1 | D-DPL | Design complex deployment strategies |
| R-SEN-020 | Senior Database Administrator | S-1 | D-DINF | Handle complex database optimization |

### 5.2 Specialist Agent Role Examples (S-2)

| Role ID | Title | Department | Specialization |
|---|---|---|---|
| R-SPC-001 | Arabic Script Specialist | D-OCR | Nastaliq, Kufic, Diwani script recognition |
| R-SPC-002 | Diacritic Specialist | D-OCR | Quranic diacritic restoration |
| R-SPC-003 | Hadith Terminology Specialist | D-HAD | Mustalah al-Hadith expertise |
| R-SPC-004 | Narrator Biography Specialist | D-HAD | Detailed narrator jarh/ta'dil knowledge |
| R-SPC-005 | Tafsir Specialist | D-QRN | Deep tafsir knowledge across works |
| R-SPC-006 | Tajweed Specialist | D-QRN | Complete tajweed rules mastery |
| R-SPC-007 | Age-Appropriateness Specialist | D-STR | Child development expertise |
| R-SPC-008 | Islamic Ethics Specialist | D-SEC | Islamic ethical framework expertise |
| R-SPC-009 | Accessibility Specialist | All | WCAG and accessibility expertise |

---

## 6. WORKER AGENT ROLES (W Level)

### 6.1 Worker Agent Role Examples (W-1)

| Role ID | Title | Department |
|---|---|---|
| R-WRK-001 | Workflow Executor | D-OPS |
| R-WRK-002 | QA Tester | D-QA |
| R-WRK-003 | Access Controller | D-SEC |
| R-WRK-004 | Image Processor | D-OCR |
| R-WRK-005 | Text Recognizer | D-OCR |
| R-WRK-006 | Entity Extractor | D-KG |
| R-WRK-007 | Relationship Mapper | D-KG |
| R-WRK-008 | Fact Formulator | D-KG |
| R-WRK-009 | Source Verifier | D-VRF |
| R-WRK-010 | Cross-Reference Verifier | D-VRF |
| R-WRK-011 | Quran Text Processor | D-QRN |
| R-WRK-012 | Hadith Text Processor | D-HAD |
| R-WRK-013 | Prompt Executor | D-CGEN |
| R-WRK-014 | Story Writer | D-STR |
| R-WRK-015 | Question Generator | D-QUIZ |
| R-WRK-016 | Distractor Generator | D-QUIZ |
| R-WRK-017 | Flashcard Creator | D-FC |
| R-WRK-018 | Card Reviewer | D-FC |
| R-WRK-019 | Curriculum Builder | D-CURR |
| R-WRK-020 | AI Teacher Responder | D-AIT |
| R-WRK-021 | XP Calculator | D-GAME |
| R-WRK-022 | Badge Assigner | D-GAME |
| R-WRK-023 | Script Writer | D-POD |
| R-WRK-024 | TTS Synthesizer | D-POD |
| R-WRK-025 | Scene Illustrator | D-IMG |
| R-WRK-026 | Character Designer | D-IMG |
| R-WRK-027 | Translator | D-L10N |
| R-WRK-028 | Cultural Adaptor | D-L10N |
| R-WRK-029 | Mastery Calculator | D-SMOD |
| R-WRK-030 | Recommender | D-REC |
| R-WRK-031 | Report Generator | D-RPT |
| R-WRK-032 | Search Indexer | D-SRCH |
| R-WRK-033 | Deployment Executor | D-DPL |
| R-WRK-034 | Monitor | D-MON |
| R-WRK-035 | DB Operator | D-DINF |
| R-WRK-036 | Network Operator | D-NET |
| R-WRK-037 | Model Server | D-AINF |

### 6.2 Junior Agent Roles (W-2)

Junior agents handle supervised tasks with manager approval. They are typically in probation or training phases.

| Role ID | Title | Department |
|---|---|---|
| R-JNR-001 | Junior QA Tester | D-QA |
| R-JNR-002 | Junior Text Recognizer | D-OCR |
| R-JNR-003 | Junior Entity Extractor | D-KG |
| R-JNR-004 | Junior Fact Formulator | D-KG |
| R-JNR-005 | Junior Verifier | D-VRF |
| R-JNR-006 | Junior Story Writer | D-STR |
| R-JNR-007 | Junior Question Generator | D-QUIZ |
| R-JNR-008 | Junior Flashcard Creator | D-FC |
| R-JNR-009 | Junior AI Teacher | D-AIT |
| R-JNR-010 | Junior Translator | D-L10N |

---

## 7. UTILITY AGENT ROLES (U Level)

| Role ID | Title | Department | Purpose |
|---|---|---|---|
| R-UTL-001 | Data Cleaner | D-OCR | Clean and normalize raw OCR data |
| R-UTL-002 | Format Checker | D-QA | Verify output format compliance |
| R-UTL-003 | Log Parser | D-MON | Parse and structure raw log data |
| R-UTL-004 | Cache Warmer | D-DINF | Pre-populate caches for performance |
| R-UTL-005 | Queue Monitor | D-OPS | Monitor queue health and depth |
| R-UTL-006 | Content Validator | D-QA | Perform basic content validation |
| R-UTL-007 | Metadata Extractor | D-KG | Extract basic metadata from text |
| R-UTL-008 | Index Updater | D-SRCH | Update search indexes |
| R-UTL-009 | Backup Operator | D-DINF | Execute backup procedures |
| R-UTL-010 | Template Renderer | D-CGEN | Render content templates |
| R-UTL-011 | Asset Organizer | D-IMG | Organize generated visual assets |
| R-UTL-012 | Terminology Checker | D-L10N | Verify terminology consistency |

---

## 8. SUPPORT AGENT ROLES (P Level)

| Role ID | Title | Department | Purpose |
|---|---|---|---|
| R-SUP-001 | Health Checker | D-MON | Run periodic health checks |
| R-SUP-002 | Log Archiver | D-MON | Archive and compress old logs |
| R-SUP-003 | Cache Cleaner | D-DINF | Clear stale cache entries |
| R-SUP-004 | Temp File Cleaner | D-DINF | Remove temporary files |
| R-SUP-005 | Permission Sync | D-SEC | Synchronize permissions across systems |
| R-SUP-006 | Report Distributor | D-RPT | Distribute scheduled reports |
| R-SUP-007 | Notification Dispatcher | D-OPS | Dispatch system notifications |

---

## 9. ROLE PROGRESSION PATHS

### 9.1 Career Ladder Examples

**OCR Path:**
```
P-1 Support → U-1 Data Cleaner → W-2 Junior Recognizer → W-1 Text Recognizer → S-2 Arabic Script Specialist → S-1 Senior OCR Specialist → M-1 Document Acquisition Lead → E-3 OCR Manager
```

**Knowledge Path:**
```
P-1 Support → U-1 Metadata Extractor → W-2 Junior Extractor → W-1 Entity Extractor → S-2 Domain Specialist → S-1 Senior Entity Extractor → M-1 Entity Extraction Lead → E-3 KG Manager
```

**Learning Path:**
```
P-1 Support → U-1 Template Renderer → W-2 Junior Story Writer → W-1 Story Writer → S-2 Age Specialist → S-1 Senior Storyteller → M-1 Story Writing Lead → E-3 Story Manager
```

---

## 10. ROLE COMPETENCY MATRIX

| Competency | P Level | U Level | W-2 | W-1 | S-2 | S-1 | M-1 | E-3 |
|---|---|---|---|---|---|---|---|---|
| Task Execution | Basic | Basic | Guided | Independent | Expert | Expert | N/A | N/A |
| Quality Check | None | Basic | Basic | Standard | Advanced | Expert | Oversight | Oversight |
| Escalation Handling | None | None | Basic | Standard | Advanced | Expert | Expert | Expert |
| Cross-Team Coordination | None | None | None | Basic | Standard | Advanced | Expert | Expert |
| Strategic Thinking | None | None | None | None | Basic | Standard | Advanced | Expert |
| Mentoring | None | None | None | Basic | Standard | Advanced | Expert | N/A |

---

## 11. APPENDICES

### 11.1 Role Count Summary

| Level | Count | Percentage |
|---|---|---|
| E-1 | 1 | 0.01% |
| E-2 | 6 | 0.07% |
| E-3 | 30 | 0.35% |
| M-1 | 130 | 1.53% |
| S-1 | 350 | 4.12% |
| S-2 | 500 | 5.88% |
| W-1 | 2,400 | 28.24% |
| W-2 | 1,000 | 11.76% |
| U-1 | 2,550 | 30.00% |
| P-1 | 1,533 | 18.04% |
| **Total** | **8,500** | **100%** |

### 11.2 Revision History

| Version | Date | Author | Changes |
|---|---|---|---|
| 1.0.0 | 2026-07-09 | Office of the Chief Architect | Initial ratified release |
