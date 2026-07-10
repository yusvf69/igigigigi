# QUIZ MODE ARCHITECTURE

## Assessment and Knowledge Evaluation System

---

| Document ID | IEP-ARCH-QUIZ-001 |
|---|---|
| Version | 1.0.0 |
| Status | Approved |
| Classification | Enterprise Internal |
| Effective Date | 2026-07-07 |
| Approving Authority | Office of the Chief Architect |
| Custodian | Assessment Content Architecture Team |
| Content Generation Reference | IEP-ARCH-CGEN-001 |
| Knowledge Graph Reference | IEP-ARCH-KG-001 |
| Verification Reference | IEP-ARCH-VRF-001 |

---

## TABLE OF CONTENTS

1. EXECUTIVE OVERVIEW
2. QUIZ MODE PHILOSOPHY
3. QUESTION TYPES
4. QUIZ GENERATION PIPELINE
5. DIFFICULTY AND ADAPTATION
6. ANSWER VERIFICATION
7. EXPLANATION GENERATION
8. SPACED REPETITION INTEGRATION
9. ASSESSMENT ANALYTICS
10. QUIZ VARIANTS AND PERSONALIZATION
11. QUALITY METRICS
12. PERFORMANCE REQUIREMENTS
13. APPENDICES

---

## 1. EXECUTIVE OVERVIEW

### 1.1 Purpose

Quiz Mode assesses learner understanding of Islamic knowledge through structured questions. It moves beyond simple recall to measure comprehension, application, and critical thinking, all while being grounded in verified Knowledge Graph facts.

### 1.2 Scope

This document covers question generation, difficulty calibration, answer verification, explanation generation, spaced repetition scheduling, and assessment analytics.

### 1.3 Core Principle

**Every question must test verifiable knowledge with unambiguous answers grounded in the Knowledge Graph.**

---

## 2. QUIZ MODE PHILOSOPHY

### 2.1 Assessment Levels (Bloom's Taxonomy Adapted)

| Level | Description | Question Type | Knowledge Graph Use |
|---|---|---|---|
| **Recall** | Remembering facts | MCQ, True/False, Fill-in-Blank | Direct fact retrieval |
| **Comprehension** | Understanding meaning | Explanation, matching | Fact + relationship retrieval |
| **Application** | Applying knowledge | Scenario-based, case study | Fact + rule application |
| **Analysis** | Breaking down concepts | Compare/contrast, categorization | Multiple fact relationships |
| **Evaluation** | Making judgments | Ranking, assessment | Multiple facts with criteria |
| **Creation** | Synthesizing knowledge | Essay, open-ended | Fact cluster synthesis |

### 2.2 Islamic Assessment Principles

| Principle | Description |
|---|---|
| **Positive Reinforcement** | Questions encourage learning, not punish ignorance |
| **No Entrapment** | Questions are fair and straightforward, not trick questions |
| **Learning-Oriented** | Every question is a learning opportunity, not just evaluation |
| **Respectful Content** | Questions never trivialize sacred knowledge |
| **Constructive Feedback** | Wrong answers come with explanations that teach |

### 2.3 Quiz Types

| Quiz Type | Purpose | Length | Time Limit |
|---|---|---|---|
| **Knowledge Check** | Quick review after content | 3-5 questions | 2-5 minutes |
| **Topic Quiz** | Assess understanding of a topic | 10-15 questions | 10-15 minutes |
| **Unit Assessment** | End-of-unit evaluation | 20-30 questions | 20-30 minutes |
| **Comprehensive Review** | Cumulative knowledge assessment | 40-50 questions | 40-60 minutes |
| **Daily Challenge** | Daily engagement | 1-3 questions | 2 minutes |
| **Spaced Review** | Reinforce previously learned content | 5-10 questions | 5-10 minutes |

---

## 3. QUESTION TYPES

### 3.1 Supported Question Types

| Type | Description | Auto-Grading | Cognitive Level |
|---|---|---|---|
| **Multiple Choice (MCQ)** | Select correct answer from options | Yes | Recall, Comprehension |
| **True/False** | Determine statement validity | Yes | Recall |
| **Fill-in-the-Blank** | Complete a statement | Yes (exact match) | Recall |
| **Matching** | Match related items | Yes | Comprehension |
| **Ordering** | Arrange items in correct sequence | Yes | Comprehension, Analysis |
| **Multiple Select** | Select all correct options | Yes | Comprehension |
| **Short Answer** | Brief written response | Partial (AI-assisted) | Comprehension, Application |
| **Scenario-Based** | Apply knowledge to a situation | Yes (MCQ format) | Application, Analysis |
| **Compare/Contrast** | Identify similarities and differences | Partial (AI-assisted) | Analysis |
| **Essay** | Extended written response | Manual (AI-assisted) | Evaluation, Creation |

### 3.2 Question Component Structure

Each question has:

```
┌─────────────────────────────────────────────────────────────────────────┐
│ QUESTION                                                                │
│ "What is the first pillar of Islam?"                                   │
│                                                                          │
│ OPTIONS (for MCQ):                                                      │
│ A. Salah (Prayer)                     ✗                                 │
│ B. Shahada (Declaration of Faith)     ✓  ← Correct Answer               │
│ C. Sawm (Fasting)                     ✗                                 │
│ D. Zakat (Charity)                    ✗                                 │
│                                                                          │
│ EXPLANATION:                                                             │
│ The first pillar of Islam is the Shahada, the declaration that          │
│ "There is no god but Allah, and Muhammad is the Messenger of Allah."    │
│ This is based on the hadith of Jibril (Sahih Muslim, Book 1, Hadith 1).│
│                                                                          │
│ METADATA:                                                                │
│ Fact IDs: [KG-FACT-001234, KG-FACT-001235]                             │
│ Difficulty: Easy                                                         │
│ Bloom's Level: Recall                                                    │
│ Knowledge Domain: Aqidah                                                 │
│ Learning Objective: Identify the five pillars of Islam                  │
│ Age Group: 9-12                                                          │
└─────────────────────────────────────────────────────────────────────────┘
```

### 3.3 Distractor Generation (for MCQ)

| Strategy | Description | Example |
|---|---|---|
| **Plausible Fact** | A fact from a related but different concept | "Salah" instead of "Shahada" (both are pillars) |
| **Common Misconception** | A widely held incorrect belief | "Fasting" is the first pillar (confuses order) |
| **Partial Truth** | A statement that is correct but not the answer to this question | "There is no god but Allah" (part of Shahada but incomplete) |
| **Semantic Similarity** | Word-level similarity that tests careful reading | "Shahada" vs. "Shahid" (similar-sounding but different) |

### 3.4 Distractor Quality Rules

| Rule | Description |
|---|---|
| **No Obviously Wrong Answers** | All distractors should be plausible to someone who doesn't know the answer |
| **No Trick Distractors** | Avoid "all of the above" / "none of the above" or other trick formats |
| **Length Matching** | All options should be similar in length to avoid pattern recognition |
| **No Repetition** | No duplicate or overlapping options |
| **Randomized Order** | Correct answer position randomized across questions |
| **Cultural Sensitivity** | No options that mock or trivialize Islamic concepts |

---

## 4. QUIZ GENERATION PIPELINE

### 4.1 Pipeline Stages

```
┌─────────────────────────────────────────────────────────────────────────┐
│                     1. REQUIREMENT ANALYSIS                             │
│  • Determine quiz type and purpose                                     │
│  • Identify knowledge domain and topic                                 │
│  • Define difficulty and age parameters                                │
└──────────────────────────────┬──────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                     2. FACT SELECTION                                   │
│  • Query Knowledge Graph for relevant facts                            │
│  • Select facts appropriate for assessment                             │
│  • Balance across cognitive levels                                     │
└──────────────────────────────┬──────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                     3. QUESTION GENERATION                              │
│  • Generate question stem                                              │
│  • Generate correct answer                                             │
│  • Generate distractors (for MCQ)                                      │
│  • Generate explanation                                                │
└──────────────────────────────┬──────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                     4. VERIFICATION                                     │
│  • Answer correctness verification                                     │
│  • Distractor validity check                                           │
│  • Question clarity review                                             │
│  • No ambiguity check                                                  │
└──────────────────────────────┬──────────────────────────────────────────┘
                               │
                               ▼
┌──────────────────────────────┐
│                     5. ASSEMBLY                                        │
│  • Combine into quiz format                                           │
│  • Order questions (easy→hard)                                        │
│  • Add instructions and metadata                                      │
└─────────────────────────────────────────────────────────────────────────┘
```

### 4.2 Fact-to-Question Mapping

| Fact Type | Suitable Question Types | Example |
|---|---|---|
| **Definition** | MCQ, Fill-in-Blank, True/False | "Tawheed means..." |
| **Classification** | Matching, Categorization | "Match each pillar to its description" |
| **Sequence** | Ordering, Timeline | "Arrange the prophets in chronological order" |
| **Comparison** | Compare/Contrast, MCQ | "What is the difference between...?" |
| **Cause-Effect** | Scenario-based, MCQ | "Why is zakat important in Islam?" |
| **Person Fact** | MCQ, Fill-in-Blank | "Who was the first caliph of Islam?" |
| **Event Fact** | Ordering, MCQ, Timeline | "In which year did the Battle of Badr occur?" |
| **Rule (Fiqh)** | Scenario-based, Application | "What should a Muslim do if...?" |
| **Quote/Verse** | Fill-in-Blank, MCQ | "Complete the ayah: '...' " |

---

## 5. DIFFICULTY AND ADAPTATION

### 5.1 Difficulty Dimensions

| Dimension | Easy | Medium | Hard | Expert |
|---|---|---|---|---|
| **Fact Obscurity** | Common knowledge | Less common | Specialized | Advanced scholarly |
| **Cognitive Level** | Recall | Comprehension | Application | Analysis/Evaluation |
| **Distractor Plausibility** | Clearly wrong | Somewhat plausible | Very plausible | Expert-level confusion |
| **Question Complexity** | Single fact | Two facts linked | Multiple facts | Chain of reasoning |
| **Language Complexity** | Simple vocabulary | Standard vocabulary | Advanced vocabulary | Scholarly terminology |

### 5.2 Dynamic Difficulty Adjustment

```
Learner answers question
    │
    ▼
┌──────────────────────────────┐
│ Performance Assessment       │
│ • Answer correct/incorrect   │
│ • Response time              │
│ • Confidence level           │
└──────────────────────────────┘
    │
    ▼
┌──────────────────────────────┐
│ Difficulty Adjustment        │
│ • Consecutive correct → harder│
│ • Consecutive wrong → easier │
│ • Maintain 70-80% accuracy  │
└──────────────────────────────┘
    │
    ▼
┌──────────────────────────────┐
│ Next Question Selection      │
│ • Adjusted difficulty        │
│ • Target weak areas          │
│ • Maintain topic coverage    │
└──────────────────────────────┘
```

### 5.3 Adaptive Parameters

| Parameter | Adjustment Rule | Range |
|---|---|---|
| **Fact Obscurity** | +1 level after 3 correct, -1 level after 2 wrong | 1-5 |
| **Cognitive Level** | Increase when recall mastered | Recall → Comprehension → Application → Analysis |
| **Distractor Count** | More distractors at higher difficulty | 3-6 options |
| **Time Limit** | Tighter limits for higher difficulty | 30-120 seconds per question |
| **Question Format** | Simpler formats at lower difficulty | True/False → MCQ → Scenario |

---

## 6. ANSWER VERIFICATION

### 6.1 Automated Grading

| Question Type | Grading Method | Accuracy |
|---|---|---|
| **MCQ** | Option selection match | 100% |
| **True/False** | Boolean match | 100% |
| **Fill-in-Blank** | Exact string match + fuzzy match for diacritic variants | > 99% |
| **Matching** | Pair comparison | 100% |
| **Ordering** | Sequence comparison | 100% |
| **Multiple Select** | Set comparison | 100% |
| **Short Answer** | AI-assisted semantic comparison with acceptable answer set | > 95% |
| **Essay** | Rubric-based AI evaluation + human review for high-stakes | > 90% |

### 6.2 Short Answer Grading

| Step | Method | Description |
|---|---|---|
| **Normalization** | Text preprocessing | Remove diacritics, normalize whitespace, handle Arabic variants |
| **Keyword Extraction** | NLP | Extract key terms from answer |
| **Semantic Comparison** | Embedding similarity | Compare answer meaning to expected answer set |
| **Acceptable Answer Set** | Pre-generated variants | "Allah", "الله", "God" → all accepted |
| **Confidence Scoring** | Match quality | 0-100% confidence in correctness |
| **Human Review Trigger** | Low confidence | < 90% confidence → flag for human review |

### 6.3 Partial Credit Rules

| Question Type | Partial Credit Logic |
|---|---|
| **Multiple Select** | Points = (correct selected - incorrect selected) / total correct, minimum 0 |
| **Matching** | Points = correct matches / total pairs |
| **Ordering** | Points depend on position accuracy (weighted by distance from correct position) |
| **Fill-in-Blank (multi-part)** | Points = correct blanks / total blanks |
| **Short Answer** | AI-assigned partial credit based on completeness and accuracy |

---

## 7. EXPLANATION GENERATION

### 7.1 Explanation Structure

Each explanation includes:

| Component | Description | Source |
|---|---|---|
| **Correct Answer Statement** | Clear statement of the correct answer | Knowledge Graph fact |
| **Rationale** | Why this is the correct answer | Knowledge Graph relationships |
| **Source Reference** | Quran verse, hadith, or scholarly work | Knowledge Graph citation |
| **Common Misconception** | Why someone might pick a wrong answer | Distractor analysis |
| **Related Facts** | Additional context for deeper understanding | Knowledge Graph expansion |
| **Application** | How this knowledge applies to daily life | Pedagogical extension |

### 7.2 Explanation Depth Levels

| Level | Content | Best For |
|---|---|---|
| **Brief** | Correct answer + single sentence rationale | Quick reviews, daily challenges |
| **Standard** | Correct answer + rationale + source | Standard quizzes |
| **Detailed** | Full explanation with sources, misconceptions, and related facts | Comprehensive assessments |
| **Scholarly** | In-depth analysis with multiple sources and scholarly opinions | Advanced learners |

---

## 8. SPACED REPETITION INTEGRATION

### 8.1 Spaced Repetition Model

| Parameter | Description | Default |
|---|---|---|
| **Initial Interval** | Days until first review | 1 day |
| **Ease Factor** | Multiplier for interval growth | 2.5 (default) |
| **Interval Cap** | Maximum days between reviews | 365 days |
| **Minimum Interval** | Minimum days between reviews | 1 day |
| **Retention Target** | Desired probability of recall | 90% |

### 8.2 Fact-Question Mapping for Spaced Repetition

Each Knowledge Graph fact can generate multiple questions:

```
KG Fact: "The five pillars of Islam are Shahada, Salah, Zakat, Sawm, Hajj."
    │
    ├── Question 1: "How many pillars of Islam are there?" (Easy, Recall)
    ├── Question 2: "Name the five pillars of Islam." (Medium, Recall)
    ├── Question 3: "Which pillar involves fasting during Ramadan?" (Easy, Comprehension)
    ├── Question 4: "What is the relationship between the five pillars?" (Hard, Analysis)
    └── Question 5: "Explain how the five pillars support a Muslim's faith." (Expert, Evaluation)
```

### 8.3 Scheduling Algorithm

| Event | Action |
|---|---|
| **Correct answer** | Increase interval, increase ease factor |
| **Wrong answer** | Reset interval, decrease ease factor |
| **Quick correct (< 5s)** | Increase ease factor more (may indicate too easy) |
| **Slow correct (> 30s)** | Treat as partial recall, slight interval increase |
| **Repeated wrong** | Reduce interval to 1 day, flag concept for review |

---

## 9. ASSESSMENT ANALYTICS

### 9.1 Per-Question Metrics

| Metric | Description | Use |
|---|---|---|
| **Accuracy Rate** | % of learners who answer correctly | Question quality assessment |
| **Average Response Time** | Time to answer | Difficulty calibration |
| **Distractor Selection Rate** | % per distractor | Distractor quality evaluation |
| **Discrimination Index** | How well question separates high/low performers | Question quality |
| **Difficulty Index** | % correct (p-value) | Difficulty calibration |

### 9.2 Learner Analytics

| Metric | Description | Use |
|---|---|---|
| **Overall Accuracy** | Average accuracy across all quizzes | Progress tracking |
| **Domain Accuracy** | Accuracy per knowledge domain | Strength/weakness identification |
| **Cognitive Level Accuracy** | Accuracy per Bloom's level | Depth of learning assessment |
| **Learning Curve** | Accuracy over time for specific topics | Mastery tracking |
| **Forgetting Curve** | Accuracy decline over time without review | Spaced repetition optimization |
| **Engagement** | Quiz completion rate, time spent | Motivation assessment |

### 9.3 Analytics-Driven Interventions

| Trigger | Intervention |
|---|---|
| **Low accuracy in a domain** | Recommend review content + targeted questions |
| **Steep forgetting curve** | Increase spaced repetition frequency |
| **Consistently fast + correct** | Increase difficulty |
| **Consistently slow + correct** | Review material, more practice |
| **High distractor selection** | Improve distractor quality or question clarity |

---

## 10. QUIZ VARIANTS AND PERSONALIZATION

### 10.1 Quiz Variants

| Variant | Description | Generation Strategy |
|---|---|---|
| **Standard** | Default quiz format | Generate from fact cluster |
| **Review** | Focus on weak areas | Select from incorrect history |
| **Challenge** | Higher difficulty, timed | Increase difficulty parameters |
| **Quick** | Minimal questions, fast | Select high-discrimination questions |
| **Comprehensive** | Full topic coverage | Sample from all sub-topics evenly |

### 10.2 Personalization Rules

| Rule | Description | Example |
|---|---|---|
| **Knowledge-Based** | Skip mastered concepts | "You've mastered the five pillars, let's test your zakah knowledge" |
| **Weakness-Focused** | Emphasize struggling areas | "You've struggled with prophets' stories. Let's practice more." |
| **Interest-Linked** | Connect to preferred topics | "Since you enjoy seerah, here are questions about the Prophet's life." |
| **Pacing-Aligned** | Match learning speed | "You're learning quickly, let's try harder questions." |

---

## 11. QUALITY METRICS

### 11.1 Question Quality Metrics

| Metric | Target | Measurement |
|---|---|---|
| **Factual Accuracy** | 100% | Automated verification against Knowledge Graph |
| **Answer Unambiguity** | 100% | Single correct answer for objective types |
| **Distractor Plausibility** | > 80% | Expert review of distractor quality |
| **Question Clarity** | > 95% | Learner understanding survey |
| **Appropriate Difficulty** | 60-80% accuracy rate | Statistical analysis |
| **No Cultural Bias** | 100% | Review against bias checklist |

### 11.2 Quiz Quality Metrics

| Metric | Target |
|---|---|
| **Topic Coverage** | All key facts in the topic represented |
| **Cognitive Level Distribution** | 40% Recall, 30% Comprehension, 20% Application, 10% Higher |
| **Difficulty Distribution** | 30% Easy, 40% Medium, 20% Hard, 10% Expert |
| **Explanatory Quality** | 100% of questions have explanations |
| **Question Uniqueness** | < 10% overlap with other quizzes |

---

## 12. PERFORMANCE REQUIREMENTS

### 12.1 Generation Performance

| Content | Generation Time | Cache Rate |
|---|---|---|
| **Single MCQ** | < 2 seconds | > 80% |
| **5-Question Quiz** | < 10 seconds | > 70% |
| **10-Question Quiz** | < 20 seconds | > 60% |
| **30-Question Assessment** | < 1 minute | > 50% |

### 12.2 Runtime Performance

| Metric | Target |
|---|---|
| **Question Load Time** | < 500ms |
| **Answer Submission** | < 200ms |
| **Result Calculation** | < 1 second |
| **Concurrent Quiz Takers** | 10,000+ |
| **Quiz History Access** | < 1 second |

---

## 13. APPENDICES

### 13.1 Question Type Templates

| Template ID | Type | Parameters |
|---|---|---|
| `mcq_recall_v2` | Multiple Choice | fact, distractors(3-5), difficulty |
| `t_f_v1` | True/False | statement, correct_boolean, explanation |
| `fill_blank_v1` | Fill-in-Blank | sentence, blank_position, acceptable_answers[] |
| `matching_v2` | Matching | left_items[], right_items[], pairs[] |
| `ordering_v1` | Ordering | items[], correct_order[] |
| `scenario_v3` | Scenario-Based | scenario_text, fact_applied, options[] |

### 13.2 Related Documents

| Document ID | Title | Relationship |
|---|---|---|
| IEP-ARCH-CGEN-001 | Content Generation Architecture | Shared generation infrastructure |
| IEP-ARCH-KG-001 | Knowledge Graph Architecture | Fact source for questions |
| IEP-ARCH-VRF-001 | Verification Architecture | Answer verification |
| IEP-ARCH-STR-001 | Story Mode Architecture | Story→Quiz learning path |
| IEP-ARCH-FC-001 | Flashcard Architecture | Companion review modality |

### 13.3 Revision History

| Version | Date | Author | Changes |
|---|---|---|---|
| 1.0.0 | 2026-07-07 | Office of the Chief Architect | Initial approved release |
