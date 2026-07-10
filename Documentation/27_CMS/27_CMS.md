# CONTENT MANAGEMENT SYSTEM ARCHITECTURE

## Knowledge Curation and Administration Platform

---

| Document ID | IEP-ARCH-CMS-001 |
|---|---|
| Version | 1.0.0 |
| Status | Approved |
| Classification | Enterprise Internal |
| Effective Date | 2026-07-09 |
| Approving Authority | Office of the Chief Architect |
| Custodian | CMS Architecture Team |

---

## TABLE OF CONTENTS

1. EXECUTIVE OVERVIEW
2. CMS PHILOSOPHY
3. USER ROLES
4. CONTENT WORKFLOW
5. CONTENT TYPES
6. SCHOLAR REVIEW SYSTEM
7. VERSIONING
8. MEDIA MANAGEMENT
9. QUALITY METRICS
10. APPENDICES

---

## 1. EXECUTIVE OVERVIEW

### 1.1 Purpose

The CMS provides administrators, scholars, and content creators with the tools to curate, review, approve, and manage all Islamic knowledge on the platform. It ensures content quality, authenticity, and proper versioning throughout the content lifecycle.

### 1.2 Core Principle

**Every piece of content on the platform passes through a verifiable chain of creation, review, and approval before reaching learners.**

---

## 2. CMS PHILOSOPHY

| Principle | Description |
|---|---|
| **Workflow-Driven** | Content moves through defined stages with clear handoffs |
| **Scholar-First** | Tools designed for efficiency of scholar review |
| **Audit Trails** | Every action logged with who, what, when |
| **Separation of Concerns** | Content creators cannot approve their own content |
| **Rollback Safe** | All changes reversible |

---

## 3. USER ROLES

| Role | Permissions | Responsibilities |
|---|---|---|
| **Content Creator** | Create/edit drafts, upload media | Draft content |
| **Content Reviewer** | Review, suggest changes | Quality and style review |
| **Scholar** | Verify Islamic accuracy, approve | Scholarly verification |
| **Editor** | Final approval, publish | Editorial oversight |
| **Translator** | Create/edit translations | Localization |
| **Admin** | Full access, user management | Platform administration |

---

## 4. CONTENT WORKFLOW

### 4.1 Standard Workflow

```
Draft → Review → Scholar Verify → Edit → Approve → Publish
  │         │           │           │        │         │
  ▼         ▼           ▼           ▼        ▼         ▼
Creator  Reviewer    Scholar     Editor   Editor   Live
```

### 4.2 Workflow States

| State | Description | Assigned To |
|---|---|---|
| **Draft** | Initial creation, not yet submitted | Content Creator |
| **In Review** | Submitted for review | Content Reviewer |
| **Scholar Review** | Under scholarly verification | Scholar |
| **Changes Requested** | Needs revisions | Content Creator |
| **Approved** | Ready for publication | Editor |
| **Published** | Live on platform | System |
| **Archived** | Removed from active use | System |

---

## 5. CONTENT TYPES

| Type | CMS Features | Verification Requirements |
|---|---|---|
| **Story** | Rich text editor, image embedding, audio attachment | Scholar review for facts |
| **Quiz Question** | Question builder, answer options, explanation editor | Automated fact check + spot check |
| **Flashcard** | Front/back editor, cloze deletion builder | Automated fact check |
| **Podcast Script** | Script editor, speaker assignment, TTS preview | Full scholar review |
| **Knowledge Graph Fact** | Structured fact editor, relationship builder | Full scholar verification |
| **Article** | Rich text editor, reference manager | Scholar review |
| **Video Lesson** | Video upload, chapters, description | Scholar review |

---

## 6. SCHOLAR REVIEW SYSTEM

| Feature | Description |
|---|---|
| **Inline Annotation** | Scholars annotate directly on content |
| **Source Linking** | Link claims to Quran, Hadith, scholarly works |
| **Reference Library** | Integrated access to digital reference works |
| **Scholarly Consensus View** | Show majority and minority opinions |
| **Review Dashboard** | Prioritized queue of content needing review |
| **Fatwa/Citation Check** | Verify citations against original sources |

---

## 7. VERSIONING

| Feature | Implementation |
|---|---|
| **Content Versioning** | Every save creates a version (deltas stored) |
| **Published Version** | Currently live version independently tracked |
| **Rollback** | One-click rollback to any previous version |
| **Diff View** | Side-by-side comparison of any two versions |
| **Version History** | Complete audit trail of who changed what and when |

---

## 8. MEDIA MANAGEMENT

| Media Type | Formats | Max Size | Storage |
|---|---|---|---|
| **Images** | JPEG, PNG, WebP | 10 MB | S3 + CDN |
| **Audio** | MP3, AAC, OGG | 50 MB | S3 + CDN |
| **Video** | MP4, WebM | 200 MB | S3 + CDN + Transcoding |
| **Documents** | PDF | 50 MB | S3 |

---

## 9. QUALITY METRICS

| Metric | Target |
|---|---|
| **Content Review Time** | < 48 hours for standard content |
| **Scholar Review Time** | < 1 week |
| **Publishing Accuracy** | 100% (no errors found after publish) |
| **Rollback Rate** | < 1% of published content |
| **Workflow Cycle Time** | Draft → Publish in < 2 weeks |

---

## 10. APPENDICES

### 10.1 Related Documents

| Document | Title | Relationship |
|---|---|---|
| IEP-ARCH-AGENT-001 | Agent Architecture | Verification Agent integration |
| IEP-ARCH-VRF-001 | Verification Architecture | Content verification |
| IEP-ARCH-KG-001 | Knowledge Graph | Fact storage |

### 10.2 Revision History

| Version | Date | Author | Changes |
|---|---|---|---|
| 1.0.0 | 2026-07-09 | Office of the Chief Architect | Initial approved release |
