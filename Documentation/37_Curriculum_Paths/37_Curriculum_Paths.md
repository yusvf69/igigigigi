# CURRICULUM AND LEARNING PATHS ARCHITECTURE

## Structured Educational Journey Design

---

| Document ID | IEP-ARCH-CURR-001 |
|---|---|
| Version | 1.0.0 |
| Status | Approved |
| Classification | Enterprise Internal |
| Effective Date | 2026-07-09 |
| Approving Authority | Office of the Chief Architect |
| Custodian | Curriculum Design Team |
| Knowledge Graph Reference | IEP-ARCH-KG-001 |
| Progress Tracking Reference | IEP-ARCH-PROG-001 |

---

## TABLE OF CONTENTS

1. EXECUTIVE OVERVIEW
2. CURRICULUM PHILOSOPHY
3. CURRICULUM DATA MODEL
4. LEARNING PATH TYPES
5. PREREQUISITE AND PROGRESSION
6. LEARNING OUTCOMES
7. ASSESSMENT INTEGRATION
8. ADAPTIVE PATH ADJUSTMENT
9. CURRICULUM AUTHORING
10. QUALITY METRICS
11. APPENDICES

---

## 1. EXECUTIVE OVERVIEW

### 1.1 Purpose

The Curriculum and Learning Paths Architecture structures Islamic knowledge into coherent educational journeys. It defines how facts from the Knowledge Graph are sequenced, leveled, and connected to create meaningful learning progressions from beginner to advanced.

### 1.2 Core Principle

**Knowledge is not a flat collection of facts — it is a structured tree of understanding. The curriculum guides the learner from roots to branches.**

---

## 2. CURRICULUM PHILOSOPHY

| Principle | Description |
|---|---|
| **Spiral Learning** | Core concepts revisited at increasing depth across levels |
| **Knowledge Before Action** | Foundational beliefs (aqidah) before practices (fiqh) |
| **Gradual Complexity** | Simple → Complex: introduce concepts, then details, then nuances |
| **Integrated Domains** | Connect aqidah, fiqh, seerah, and akhlaq in coherent themes |
| **Learner-Relevant** | Age-appropriate content that connects to the learner's life |
| **Scholarly Grounded** | Curriculum structure reviewed and approved by Islamic scholars |

---

## 3. CURRICULUM DATA MODEL

### 3.1 Core Entities

| Entity | Description | Example |
|---|---|---|
| **Curriculum** | Top-level educational program | "Foundations of Islam" |
| **Level** | Grade or stage within curriculum | "Level 2: Ages 9-12" |
| **Unit** | Thematic grouping of topics | "Unit 3: The Prophets" |
| **Module** | Single topic within a unit | "Module 2: Prophet Musa (AS)" |
| **Lesson** | Single learning session | "Lesson 1: Musa's Birth" |
| **Learning Objective** | Specific, measurable outcome | "Describe the story of Musa's birth" |
| **Knowledge Fact** | Fact from Knowledge Graph | KG-FACT-004521 |

### 3.2 Curriculum Structure

```
Curriculum: "Islamic Studies"
├── Level 1 (Ages 6-8): Foundations
│   ├── Unit 1: Who is Allah?
│   │   ├── Module 1: Allah is One
│   │   │   ├── Lesson 1: Tawheed
│   │   │   │   ├── Objective: Define tawheed
│   │   │   │   └── Objective: Give one example
│   │   │   └── Lesson 2: Allah is the Creator
│   │   └── Module 2: Allah's Names
│   ├── Unit 2: Our Prophet Muhammad ﷺ
│   └── Unit 3: The Five Pillars
│
├── Level 2 (Ages 9-12): Understanding
│   ├── Unit 1: Tawheed (Advanced)
│   ├── Unit 2: Stories of the Prophets
│   └── Unit 3: Salah and Wudu
│
└── Level 3 (Ages 13-15): Application
    └── ...
```

---

## 4. LEARNING PATH TYPES

### 4.1 Path Categories

| Path Type | Description | Duration | Flexibility |
|---|---|---|---|
| **Structured Curriculum** | Fixed progression through levels | Multi-year | Low |
| **Topic Mastery Path** | Deep dive into one knowledge domain | 4-12 weeks | Medium |
| **Interest-Based Path** | Auto-generated from learner interests | Variable | High |
| **Remedial Path** | Target identified knowledge gaps | Per gap | Medium |
| **Revision Path** | Comprehensive review of a level | 2-4 weeks | Low |
| **Spontaneous Path** | AI-generated: answer a learner's question → expand | Per session | Very High |

### 4.2 Path Generation

```
Learner Profile + Goals
    │
    ▼
┌──────────────────────────────┐
│ Path Generator               │
│ • Curriculum templates       │
│ • Learner level detection    │
│ • Goal mapping               │
│ • Gap analysis               │
└──────────────────────────────┘
    │
    ▼
┌──────────────────────────────┐
│ Module Sequencing            │
│ • Prerequisite check         │
│ • Optimal ordering           │
│ • Time estimation            │
└──────────────────────────────┘
    │
    ▼
┌──────────────────────────────┐
│ Content Assignment           │
│ • Stories for introduction   │
│ • Flashcards for facts       │
│ • Quizzes for assessment     │
│ • Podcasts for depth         │
└──────────────────────────────┘
    │
    ▼
Learning Path
```

---

## 5. PREREQUISITE AND PROGRESSION

### 5.1 Prerequisite Types

| Type | Description | Example |
|---|---|---|
| **Conceptual** | Must understand A before B | "Must complete Tawheed before Shirk" |
| **Sequential** | Must study A before B | "Must complete Level 1 before Level 2" |
| **Mastery-Based** | Must master A before B | "Must score 80% on Tawheed quiz to proceed" |
| **Age-Based** | Minimum age for content | "Must be 13+ for advanced Fiqh" |

### 5.2 Progression Rules

| Rule | Description |
|---|---|
| **Horizontal** | Complete all modules in a unit before moving to next unit |
| **Vertical** | Complete all units in a level before next level |
| **Remedial Loop** | If assessment < 60%, automatically assign remedial path |
| **Acceleration** | If assessment > 95%, skip to enrichment content |
| **Prerequisite Lock** | Cannot access a module until prerequisites are met |

---

## 6. LEARNING OUTCOMES

### 6.1 Outcome Taxonomy

| Level | Outcome Type | Verb Examples | Assessment |
|---|---|---|---|
| **1** | Recall | List, define, identify | MCQs, flashcards |
| **2** | Understand | Explain, describe, summarize | Short answer, quiz |
| **3** | Apply | Demonstrate, use, implement | Scenario-based |
| **4** | Analyze | Compare, contrast, categorize | Essay, discussion |
| **5** | Evaluate | Assess, justify, critique | Essay, debate |
| **6** | Create | Design, develop, compose | Project, presentation |

### 6.2 Outcome Mapping

Each learning objective maps to:
- One or more Knowledge Graph facts
- A cognitive level (Bloom's)
- An assessment method
- A content modality (story, quiz, flashcard, podcast)

---

## 7. ASSESSMENT INTEGRATION

| Assessment Point | Purpose | Method | Weight |
|---|---|---|---|
| **Pre-Assessment** | Diagnose prior knowledge | Short quiz | 0% (diagnostic) |
| **Formative** | Check understanding during module | Embedded questions | 10% |
| **Summative** | End-of-module assessment | Full quiz | 30% |
| **Unit Test** | End-of-unit comprehensive | Extended quiz | 30% |
| **Level Exam** | End-of-level final | Comprehensive | 30% |
| **Mastery Check** | Long-term retention | Spaced repetition data | Continuous |

---

## 8. ADAPTIVE PATH ADJUSTMENT

| Learner Signal | Adjustment |
|---|---|
| **Consistently high quiz scores** | Accelerate, offer enrichment |
| **Struggling with a topic** | Remidial loop, alternative explanations |
| **Rapidly answering flashcards** | Mark facts as mastered, reduce reviews |
| **Long gaps between sessions** | Review path, consolidation modules |
| **High interest in a topic** | Offer deeper learning path extension |
| **Boredom indicators (low time, skipping)** | Switch modality, offer choice |

---

## 9. CURRICULUM AUTHORING

| Feature | Description |
|---|---|
| **Curriculum Builder** | Drag-and-drop curriculum construction interface |
| **Template Library** | Pre-built curriculum templates by age group and level |
| **Learning Objective Designer** | Define outcomes with Bloom's taxonomy selector |
| **Prerequisite Mapper** | Visual relationship mapping between concepts |
| **Content Requirement Generator** | Auto-detect what content is needed for a curriculum |
| **Curriculum Export** | PDF/scholarly format for review and approval |

---

## 10. QUALITY METRICS

| Metric | Target |
|---|---|
| **Level Completion Rate** | > 70% |
| **Module Completion Rate** | > 80% |
| **Prerequisite Pass Rate** | > 85% at first attempt |
| **Learning Objective Achievement** | > 90% |
| **Curriculum Satisfaction** | > 4.5/5 learner rating |
| **Age-Appropriateness Score** | 100% (scholar-verified) |

---

## 11. APPENDICES

### 11.1 Related Documents

| Document | Title | Relationship |
|---|---|---|
| IEP-ARCH-KG-001 | Knowledge Graph | Fact source for curriculum |
| IEP-ARCH-PROG-001 | Progress Tracking | Learner progress data |
| IEP-ARCH-CGEN-001 | Content Generation | Content aligned to curriculum |
| IEP-ARCH-QUIZ-001 | Quiz Mode | Assessment integration |

### 11.2 Revision History

| Version | Date | Author | Changes |
|---|---|---|---|
| 1.0.0 | 2026-07-09 | Office of the Chief Architect | Initial approved release |
