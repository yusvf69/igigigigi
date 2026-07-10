# PERSONAL AI MENTOR

## Intelligent Lifelong Learning Companion

---

| Metadata | Value |
|---|---|
| Document ID | LXP-MENTOR-001 |
| Version | 1.0.0 |
| Status | Draft |
| Classification | Enterprise Internal |
| Effective Date | 2026-07-10 |
| Approving Authority | Office of the Chief Architect |
| Custodian | Learning Experience Architecture Team |
| Supersedes | None |
| Core Prerequisites | IEP-ARCH-ATEACH-001 (AI Teacher), IEP-ARCH-PROG-001 (Progress Tracking), IEP-ARCH-CURR-001 (Curriculum Paths) |

---

## TABLE OF CONTENTS

1. EXECUTIVE OVERVIEW
2. MENTOR PHILOSOPHY
3. MENTOR ARCHITECTURE
4. LEARNER MODEL
5. KNOWLEDGE STATE TRACKING
6. WEAKNESS PREDICTION ENGINE
7. FORGETTING PREDICTION ENGINE
8. STRATEGY SELECTION ENGINE
9. CONTENT RECOMMENDATION ENGINE
10. DAILY MENTORSHIP CYCLE
11. WEEKLY MENTORSHIP CYCLE
12. MONTHLY MENTORSHIP CYCLE
13. LONG-TERM MENTORSHIP
14. MENTOR-LEARNER RELATIONSHIP
15. MENTOR-AI TEACHER INTEGRATION
16. MENTOR-CONTENT INTEGRATION
17. MENTOR-GAMIFICATION INTEGRATION
18. MENTOR-CURRICULUM INTEGRATION
19. MENTOR-MEMORY ENGINE INTEGRATION
20. MENTOR-SOCIAL INTEGRATION
21. COMMUNICATION PROTOCOLS
22. DATA STRUCTURES
23. EXECUTION FLOW
24. STATE MANAGEMENT
25. CACHING AND MEMORY
26. PERSISTENCE
27. VALIDATION AND RETRY LOGIC
28. ERROR RECOVERY
29. SECURITY AND PRIVACY
30. MONITORING AND METRICS
31. SCALABILITY AND PERFORMANCE
32. TESTING STRATEGY
33. RISK ANALYSIS
34. FAILURE SCENARIOS
35. OPERATIONAL LIMITS
36. FUTURE IMPROVEMENTS
37. ACCEPTANCE CRITERIA
38. DEFINITION OF DONE

---

## 1. EXECUTIVE OVERVIEW

### 1.1 Purpose

The Personal AI Mentor is the learner's intelligent lifelong companion — an AI system that understands each learner as an individual, tracks their knowledge state across all domains, predicts what they will forget and struggle with, selects optimal learning strategies in real time, and orchestrates their entire educational journey across every content mode, assessment type, and social interaction on the platform.

It is **not** a chatbot. The AI Teacher handles conversations. The Mentor is the **strategic intelligence** behind every learner's experience.

### 1.2 Core Principle

**The Mentor knows the learner better than the learner knows themselves — and uses that knowledge to guide, not control.**

### 1.3 Scope

The Mentor is responsible for:
- Building and maintaining a deep learner model across cognitive, behavioral, and motivational dimensions
- Predicting forgetting curves for every known fact per learner
- Predicting weakness areas before they manifest in assessments
- Selecting teaching strategies per interaction based on learner state
- Recommending the right content type, topic, and difficulty at the right time
- Generating daily, weekly, and monthly learning plans
- Orchestrating the complete revision schedule
- Detecting learner disengagement, frustration, boredom, or confusion
- Adapting goals, pace, and challenge levels dynamically
- Integrating with every content mode, assessment system, and social feature

The Mentor explicitly does **not**:
- Generate content (delegated to Content Generation agents)
- Conduct conversations (delegated to AI Teacher)
- Assign gamification rewards (delegated to Gamification System)
- Grade assessments (delegated to Quiz/Assessment System)
- Store the Knowledge Graph (delegated to KG System)

---

## 2. MENTOR PHILOSOPHY

### 2.1 Foundational Beliefs

| Belief | Implication |
|---|---|
| Every learner is unique | The Mentor builds an individual model — no one-size-fits-all |
| Learning is non-linear | The Mentor supports exploration, not just lockstep progression |
| Understanding > Memorization | The Mentor prioritizes depth over speed |
| Mistakes are learning data | The Mentor never penalizes — it learns from errors |
| Consistency beats intensity | The Mentor optimizes for sustainable daily habits |
| Learners know themselves too | The Mentor incorporates learner self-assessment and preferences |
| Knowledge decays naturally | The Mentor's primary job is to combat forgetting |
| Motivation fluctuates | The Mentor adapts to the learner's emotional state |

### 2.2 Design Principles

| Principle | Description |
|---|---|
| **Radical Personalization** | Every recommendation, strategy, and plan is tailored to one specific learner |
| **Predictive Before Reactive** | Predict problems before they occur, don't just respond to them |
| **Minimum Effective Intervention** | Guide with the lightest touch necessary — don't overwhelm |
| **Transparent Reasoning** | When asked, the Mentor explains why it recommended something |
| **Continuous Learning** | The Mentor improves its model with every learner interaction |
| **Respectful Autonomy** | The learner can override any recommendation |
| **Privacy By Design** | The learner model belongs to the learner, not the platform |
| **Progressive Trust** | The Mentor earns more autonomy as it demonstrates good recommendations |

### 2.3 Mentor Persona

| Attribute | Description |
|---|---|
| **Name** | Personalizable by learner (default: "Mentor") |
| **Tone** | Encouraging, patient, wise, respectful, honest |
| **Communication Style** | Warm but professional, uses learner's language |
| **Knowledge Boundary** | Knows only what the KG contains + what it learns about the learner |
| **Relationship** | Coach + guide + accountability partner |
| **Availability** | 24/7 asynchronous; synchronous check-ins at learner's preferred times |
| **Memory** | Remembers every interaction, every recommendation, every outcome |

---

## 3. MENTOR ARCHITECTURE

### 3.1 High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│                        PERSONAL AI MENTOR                                │
│                                                                          │
│  ┌──────────────────────────────────────────────────────────────────┐   │
│  │                    MENTOR CORE                                     │   │
│  │  ┌────────────┐  ┌────────────┐  ┌────────────┐  ┌────────────┐  │   │
│  │  │  Learner   │  │  Forgetting│  │  Weakness  │  │  Strategy  │  │   │
│  │  │  Model     │  │  Predictor │  │  Predictor │  │  Selector  │  │   │
│  │  └────────────┘  └────────────┘  └────────────┘  └────────────┘  │   │
│  │                                                                      │   │
│  │  ┌────────────┐  ┌────────────┐  ┌────────────┐  ┌────────────┐  │   │
│  │  │  Content   │  │   Daily    │  │  Session   │  │   Long     │  │   │
│  │  │  Recommender│  │   Planner  │  │  Optimizer │  │   Term     │  │   │
│  │  └────────────┘  └────────────┘  └────────────┘  └────────────┘  │   │
│  └──────────────────────────────────────────────────────────────────┘   │
│                                    │                                    │
│  ┌──────────────────────────────────────────────────────────────────┐   │
│  │                    INTEGRATION LAYER                               │   │
│  │  ┌────────┐  ┌────────┐  ┌────────┐  ┌────────┐  ┌────────┐     │   │
│  │  │ AI     │  │Memory  │  │Content │  │Gamifica│  │Curriculum│    │   │
│  │  │Teacher │  │Engine  │  │Modes   │  │tion    │  │Paths    │    │   │
│  │  └────────┘  └────────┘  └────────┘  └────────┘  └────────┘     │   │
│  └──────────────────────────────────────────────────────────────────┘   │
│                                    │                                    │
│  ┌──────────────────────────────────────────────────────────────────┐   │
│  │                    AIOS INTERFACE                                  │   │
│  │  Agent Manager │ Memory System │ Event Bus │ Scheduler │ Model   │   │
│  └──────────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────────┘
```

### 3.2 Internal Components

| Component | Primary Function | Runs As |
|---|---|---|
| **Learner Model** | Maintains comprehensive learner state | Persistent agent (AIOS) |
| **Forgetting Predictor** | Predicts memory decay per fact per learner | Scheduled job + real-time |
| **Weakness Predictor** | Predicts knowledge gaps before they surface | ML inference pipeline |
| **Strategy Selector** | Chooses teaching approach per interaction | Real-time inference |
| **Content Recommender** | Selects what content to serve next | Real-time inference + batch |
| **Daily Planner** | Generates daily learning schedule | Cron (daily) |
| **Session Optimizer** | Adjusts in real time during a learning session | Real-time |
| **Long-Term Planner** | Quarterly/yearly goal setting and tracking | Cron (quarterly) |
| **Engagement Detector** | Detects boredom, frustration, confusion | Real-time stream analysis |
| **Reflection Engine** | Generates insights and summaries for learner | Weekly/monthly batch |

### 3.3 External Components

| Component | Interface | Data Exchanged |
|---|---|---|
| AI Teacher | gRPC + Events | Learner queries, Mentor instructions, session context |
| Memory Engine | gRPC | Review schedule, ease factors, retention predictions |
| Quiz System | Events | Answers, accuracy, response time, confidence |
| Story Mode | Events | Stories read, comprehension checks |
| Flashcard System | Events | Reviews, ease factors, recall success |
| Podcast System | Events | Podcasts listened, engagement metrics |
| Gamification System | Events | XP earned, badges, streaks, level |
| Curriculum Paths | gRPC | Path progress, completed modules, prerequisites |
| Progress Tracking | Events + DB | All learning events |
| Social Learning | Events | Study group activity, peer interactions |
| KG System | gRPC (read) | Fact relationships for difficulty estimation |

---

## 4. LEARNER MODEL

### 4.1 Model Dimensions

The learner model is multidimensional, capturing not just what the learner knows but who they are as a learner:

| Dimension | Sub-Components | Update Frequency | Persistence |
|---|---|---|---|
| **Identity** | Age, language, location, madhhab (optional), education level | On registration / profile change | Permanent |
| **Cognitive** | Knowledge state per fact, per domain, per concept | Every learning event | Permanent |
| **Behavioral** | Learning time patterns, session duration, content preferences, device usage | Every session | 90 days rolling |
| **Motivational** | Engagement score, streak health, goal alignment, topic interest levels | Daily | Permanent |
| **Metacognitive** | Self-assessment accuracy, help-seeking behavior, reflection quality | Weekly | Permanent |
| **Social** | Group participation, peer teaching, discussion engagement | Daily | Permanent |
| **Physiological** | Optimal learning times, attention span patterns, fatigue indicators | Per session | 30 days rolling |
| **Historical** | Past curricula, completed levels, mastered domains, growth trajectory | Permanent | Permanent |

### 4.2 Identity Model

```json
{
  "learner_id": "usr_a1b2c3d4",
  "profile": {
    "display_name": "Ahmed",
    "age_group": "9-12",
    "primary_language": "ar",
    "secondary_languages": ["en"],
    "madhhab": "hanafi",
    "country": "SA",
    "education_level": "intermediate"
  },
  "preferences": {
    "preferred_content_types": ["story", "interactive"],
    "preferred_learning_time": "evening",
    "session_duration_target_min": 20,
    "session_duration_max": 45,
    "notification_frequency": "moderate",
    "review_reminder_time": "19:00",
    "mentor_tone": "encouraging",
    "autonomy_level": "guided" // guided, semi-autonomous, autonomous
  },
  "goals": {
    "primary": {
      "type": "curriculum",
      "id": "curr_foundations_001",
      "target_completion": "2027-06-01"
    },
    "secondary": [
      {"type": "topic", "id": "topic_tawheed_001", "priority": "high"},
      {"type": "skill", "id": "skill_arabic_reading", "priority": "medium"}
    ],
    "personal": [
      {"text": "Read Quran with understanding", "set_by": "learner", "date": "2026-01-15"}
    ]
  }
}
```

### 4.3 Cognitive Model (Per-Fact State)

For every Knowledge Graph fact, the Mentor maintains:

```json
{
  "learner_id": "usr_a1b2c3d4",
  "fact_id": "KG-FACT-004521",
  "domain": "Aqidah",
  "topic": "Tawheed",
  "mastery": {
    "level": 3,                    // 0-5 scale
    "confidence": 0.82,            // 0.0-1.0
    "stability": 0.74,             // 0.0-1.0 (resistance to forgetting)
    "last_reviewed": "2026-07-08T14:30:00Z",
    "next_review": "2026-07-15T14:30:00Z",
    "times_correct": 12,
    "times_incorrect": 3,
    "avg_response_time_ms": 4200,
    "ease_factor": 2.3,
    "interval_days": 7,
    "learning_history": [
      {"event": "first_exposure", "source": "story", "date": "2026-06-01"},
      {"event": "quiz_correct", "source": "topic_quiz", "date": "2026-06-03"},
      {"event": "flashcard_correct", "source": "spaced_review", "date": "2026-06-05"},
      {"event": "quiz_incorrect", "source": "comprehensive", "date": "2026-06-20"},
      {"event": "ai_teacher_discussion", "source": "conversation", "date": "2026-06-22"},
      {"event": "flashcard_correct", "source": "spaced_review", "date": "2026-07-01"},
      {"event": "flashcard_correct", "source": "spaced_review", "date": "2026-07-08"}
    ]
  },
  "relationships": {
    "prerequisite_for": ["KG-FACT-004522", "KG-FACT-004523"],
    "builds_upon": ["KG-FACT-004520"],
    "related": ["KG-FACT-004530", "KG-FACT-004531"]
  }
}
```

### 4.4 Cognitive Model (Aggregate Per Domain)

```json
{
  "learner_id": "usr_a1b2c3d4",
  "domain": "Aqidah",
  "overall_mastery": 0.74,
  "fact_count": 48,
  "facts_mastered": 22,
  "facts_learning": 18,
  "facts_exposed": 8,
  "facts_not_started": 0,
  "weakest_topics": [
    {"topic": "Qadr", "mastery": 0.45, "fact_count": 6},
    {"topic": "Asma wa Sifat", "mastery": 0.58, "fact_count": 12}
  ],
  "strongest_topics": [
    {"topic": "Tawheed", "mastery": 0.92, "fact_count": 8},
    {"topic": "Risalah", "mastery": 0.87, "fact_count": 5}
  ],
  "forgetting_risk": 0.23,       // overall domain forgetting risk
  "weekly_growth": 0.05,
  "monthly_growth": 0.18,
  "assessment_history": {
    "quizzes_taken": 18,
    "avg_accuracy": 0.79,
    "avg_response_time_ms": 8500
  }
}
```

### 4.5 Behavioral Model

```json
{
  "learner_id": "usr_a1b2c3d4",
  "session_patterns": {
    "typical_start_hour": 19,       // 7 PM local time
    "typical_end_hour": 20,         // 8 PM
    "typical_duration_min": 25,
    "preferred_days": ["sunday", "monday", "tuesday", "wednesday", "thursday"],
    "session_frequency_per_week": 5.2
  },
  "engagement": {
    "current_streak_days": 14,
    "longest_streak_days": 45,
    "weekly_active_days_avg": 5.2,
    "session_completion_rate": 0.88,
    "content_completion_rate": 0.82,
    "quiz_attempt_rate": 0.7
  },
  "attention": {
    "estimated_attention_span_min": 22,
    "fatigue_threshold_min": 35,
    "optimal_break_frequency_min": 25
  },
  "content_affinity": {
    "most_engaged": "story",
    "least_engaged": "podcast",
    "diversity_score": 0.65        // 0.0 (one mode only) - 1.0 (all modes)
  },
  "learning_velocity": {
    "new_facts_per_week_avg": 12,
    "facts_to_review_per_week_avg": 35,
    "mastery_growth_per_week_avg": 0.04
  }
}
```

### 4.6 Motivational Model

```json
{
  "learner_id": "usr_a1b2c3d4",
  "motivation": {
    "current_level": "moderate",    // low, moderate, high, very_high
    "trend_7d": "increasing",
    "trend_30d": "stable",
    "intrinsic_score": 0.72,
    "extrinsic_score": 0.55
  },
  "interests": {
    "declared": ["stories_of_prophets", "islamic_history"],
    "inferred": ["fiqh_of_salah", "namaaz"],
    "emerging": ["arabic_language"],
    "declining": ["general_aqidah"]
  },
  "frustration_indicators": {
    "repeated_failures": false,
    "session_abandonment_rate": 0.08,
    "help_requests_rate": 0.12,
    "negative_sentiment_in_teacher_dialogue": 0.03
  },
  "goals": {
    "progress_toward_primary": 0.35,
    "goal_confidence": 0.7,        // belief they can achieve their goal
    "goal_clarity": 0.8            // how well they understand their goal path
  }
}
```

---

## 5. KNOWLEDGE STATE TRACKING

### 5.1 Mastery Levels (Extended)

| Level | Label | Criteria | Confidence Range | Stability Range |
|---|---|---|---|---|
| 0 | Not Started | Fact never encountered | 0.0 | 0.0 |
| 1 | Exposed | Seen fact once, no recall attempt | 0.0-0.1 | 0.0-0.1 |
| 2 | Learning | Active study phase, inconsistent recall | 0.1-0.4 | 0.1-0.3 |
| 3 | Familiar | Can recall with effort, knows where to find it | 0.4-0.7 | 0.3-0.6 |
| 4 | Mastered | Consistent correct recall under normal conditions | 0.7-0.9 | 0.6-0.85 |
| 5 | Automatic | Instant recall, can teach others | 0.9-1.0 | 0.85-1.0 |

### 5.2 Mastery Update Rules

| Event | Mastery Impact | Confidence Impact | Stability Impact |
|---|---|---|---|
| Correct quiz answer (first attempt) | +0.15 | +0.10 | +0.05 |
| Correct quiz answer (review) | +0.05 | +0.05 | +0.08 |
| Incorrect quiz answer | -0.10 | -0.15 | -0.05 |
| Correct flashcard recall | +0.03 | +0.03 | +0.06 |
| Incorrect flashcard recall | -0.05 | -0.08 | -0.03 |
| AI Teacher discussion on fact | +0.05 | +0.05 | +0.02 |
| Story reading (fact present) | +0.02 | +0.02 | +0.01 |
| Podcast listening (fact referenced) | +0.01 | +0.01 | +0.01 |
| Spaced repetition review (correct) | +0.04 | +0.04 | +0.10 |
| Spaced repetition review (incorrect) | -0.08 | -0.10 | -0.06 |

### 5.3 Mastery Decay (Enhanced)

The decay function is per-fact and considers multiple factors:

```
decay_rate = base_decay × (1 - stability) × recency_weight × difficulty_weight

Where:
- base_decay: 0.05 (default), varies by fact type (definition: 0.03, concept: 0.05, procedure: 0.07)
- stability: current stability score (0.0-1.0)
- recency_weight: 1.0 for first week, decreases by 0.1 each week
- difficulty_weight: easy(0.8), medium(1.0), hard(1.2), expert(1.5)

effective_mastery = mastery × (1 - decay_rate × days_since_review)
```

### 5.4 Knowledge Graph Overlay

The Mentor maintains a personalized overlay on the Knowledge Graph:

```
         ┌──────────────┐
         │   Tawheed    │ ← Mastery: 0.92, Stability: 0.88, Next Review: +14d
         └──────────────┘
                │
     ┌──────────┼──────────┐
     ▼          ▼          ▼
┌──────────┐ ┌──────────┐ ┌──────────┐
│Rububiyyah│ │Uluhiyyah │ │Asma Sifat│ ← WEAKNESS: Mastery 0.58, Stability 0.42
│Mastery:  │ │Mastery:  │ │Mastery:  │    Next Review: +2d (URGENT)
│0.95      │ │0.88      │ │0.58      │
│Stable: 0.│ │Stable:0.8│ │Stable:0.4│
│92       │ │2         │ │2         │
└──────────┘ └──────────┘ └──────────┘
                              │
                    ┌─────────┼─────────┐
                    ▼         ▼         ▼
               ┌────────┐ ┌────────┐ ┌────────┐
               │Names   │ │Attribs │ │Negation│
               │0.62    │ │0.55    │ │0.45    │ ← CRITICAL WEAKNESS
               └────────┘ └────────┘ └────────┘
```

---

## 6. WEAKNESS PREDICTION ENGINE

### 6.1 Prediction Model

The Weakness Predictor identifies knowledge gaps before they surface in assessments:

| Input Feature | Source | Weight |
|---|---|---|
| Current mastery level | Learner Model | 0.25 |
| Days since last review | Learner Model | 0.20 |
| Prerequisite mastery | KG relationships | 0.15 |
| Related concept mastery | KG relationships | 0.10 |
| Number of exposures | Learner Model | 0.10 |
| Average correctness | Learner Model | 0.08 |
| Response time trend | Learner Model | 0.07 |
| Age group norm comparison | Global analytics | 0.05 |

### 6.2 Prediction Output

```json
{
  "learner_id": "usr_a1b2c3d4",
  "generated_at": "2026-07-10T00:00:00Z",
  "weakness_report": {
    "critical": [
      {
        "fact_id": "KG-FACT-004530",
        "topic": "Asma wa Sifat - Negation",
        "current_mastery": 0.45,
        "predicted_mastery_in_7d": 0.32,
        "probability_of_incorrect": 0.72,
        "root_cause": "prerequisite_gap",
        "prerequisite_fact_id": "KG-FACT-004529",
        "prerequisite_mastery": 0.38,
        "recommended_action": "review_prerequisite_then_retach"
      },
      {
        "fact_id": "KG-FACT-006712",
        "topic": "Salah - Conditions",
        "current_mastery": 0.50,
        "predicted_mastery_in_7d": 0.38,
        "probability_of_incorrect": 0.65,
        "root_cause": "infrequent_review",
        "days_since_review": 21,
        "recommended_action": "urgent_review"
      }
    ],
    "moderate": [
      {
        "fact_id": "KG-FACT-008901",
        "topic": "Zakat - Nisab",
        "current_mastery": 0.60,
        "predicted_mastery_in_7d": 0.50,
        "root_cause": "single_exposure",
        "exposure_count": 1,
        "recommended_action": "reinforce"
      }
    ],
    "emerging": [
      {
        "fact_id": "KG-FACT-009102",
        "topic": "Hajj - Pillars",
        "current_mastery": 0.70,
        "predicted_mastery_in_7d": 0.62,
        "root_cause": "declining_trend",
        "recommended_action": "monitor"
      }
    ]
  }
}
```

### 6.3 Root Cause Classification

| Root Cause | Description | Intervention |
|---|---|---|
| **prerequisite_gap** | Learner lacks prerequisite knowledge | Teach prerequisite first |
| **infrequent_review** | Too long since last exposure | Urgent review session |
| **single_exposure** | Only encountered once | Additional exposures in different modes |
| **declining_trend** | Mastery decreasing despite reviews | Change teaching strategy |
| **confusion_with_similar** | Learner confuses with related concept | Comparative teaching |
| **misconception** | Learner has formed incorrect understanding | Corrective teaching with clear evidence |
| **overconfidence** | Learner thinks they know but doesn't | Challenge with harder questions |
| **underconfidence** | Learner knows more than they think | Confidence-building exercises |
| **attention_related** | Errors correlate with fatigue/distraction | Change timing or break |
| **language_barrier** | Errors correlate with language complexity | Simplify language or offer translation |

---

## 7. FORGETTING PREDICTION ENGINE

### 7.1 Forgetting Model

The forgetting predictor uses an enhanced Ebbinghaus-inspired model calibrated per learner:

```
retention_probability = e^(-t / S)

Where:
- t = time since last successful recall (in days)
- S = memory strength (stability score × 30 days max)

Retention Thresholds:
- Critical: retention_probability < 0.5 → immediate review needed
- Warning: retention_probability 0.5-0.7 → review within 48 hours
- Healthy: retention_probability 0.7-0.9 → review within scheduled interval
- Strong: retention_probability > 0.9 → no immediate action
```

### 7.2 Individual Forgetting Curve

Each learner has a calibrated forgetting curve per fact type:

```
Learner Profile: "usr_a1b2c3d4"

Forgetting Rate by Fact Type:
  Definitions:     ████░░░░░░ 0.42 (faster forgetting)
  Concepts:        ███░░░░░░░ 0.35
  Procedures:      █████░░░░░ 0.52 (fastest forgetting)
  Narratives:      ██░░░░░░░░ 0.22 (slowest forgetting)
  Verses (Quran):  ███████░░░ 0.68 (unique challenge)

Learner's Personal Coefficient: 1.15 (15% faster forgetting than average)
```

### 7.3 Forgetting Heat Map

The Mentor generates a heat map of forgetting risk across all domains:

```
Knowledge Domains - Forgetting Risk Heat Map (today: 2026-07-10)

Aqidah:
  Tawheed           ████████████████░░░░░░░░░░░░░░░  HIGH RISK (0.62)
  Risalah           ██████████████████████░░░░░░░░░  MEDIUM (0.48)
  Qadr              ██████████████████████████░░░░░  LOW (0.32)

Fiqh:
  Salah             ██████████████████████████████  VERY LOW (0.18) [reviewed yesterday]
  Zakat             ████████████████░░░░░░░░░░░░░░░  HIGH RISK (0.58)
  Sawm              ████████████████████████░░░░░░░  MEDIUM (0.42)
  Hajj              ████████████████░░░░░░░░░░░░░░░  HIGH RISK (0.65) [not started]

Seerah:
  Makkan Period     ██████████████████████░░░░░░░░░  MEDIUM (0.45)
  Madinan Period    ████████████████████░░░░░░░░░░░  MEDIUM (0.52)
```

### 7.4 Optimal Review Schedule

The Mentor computes the optimal review schedule balancing all facts:

```json
{
  "learner_id": "usr_a1b2c3d4",
  "schedule_date": "2026-07-10",
  "review_budget_minutes": 15,
  "overdue_facts": 3,
  "due_today": 8,
  "due_this_week": 35,
  "schedule": [
    {"fact_id": "KG-FACT-004530", "priority": "critical", "suggested_mode": "flashcard"},
    {"fact_id": "KG-FACT-006712", "priority": "critical", "suggested_mode": "quiz"},
    {"fact_id": "KG-FACT-004529", "priority": "high", "suggested_mode": "ai_teacher"},
    {"fact_id": "KG-FACT-009102", "priority": "medium", "suggested_mode": "story"},
    {"fact_id": "KG-FACT-008901", "priority": "medium", "suggested_mode": "flashcard"},
    {"fact_id": "KG-FACT-004531", "priority": "low", "suggested_mode": "podcast"},
    {"fact_id": "KG-FACT-004532", "priority": "low", "suggested_mode": "flashcard"},
    {"fact_id": "KG-FACT-004533", "priority": "low", "suggested_mode": "ai_teacher"}
  ],
  "estimated_time_minutes": {
    "critical": 4,
    "high": 3,
    "medium": 5,
    "low": 3,
    "total": 15
  }
}
```

---

## 8. STRATEGY SELECTION ENGINE

### 8.1 Teaching Strategy Library

| Strategy ID | Name | Description | Best When |
|---|---|---|---|
| S-01 | Direct Instruction | Clear explanation with examples | Learner is new to topic, needs foundation |
| S-02 | Socratic Dialogue | Guided questions to promote discovery | Learner has some knowledge, needs depth |
| S-03 | Analogy | Connect to known concept | Abstract or unfamiliar concept |
| S-04 | Storytelling | Narrative-based teaching | Historical, moral, or emotional content |
| S-05 | Scenario-Based | Real-world application | Practical knowledge, fiqh, akhlaq |
| S-06 | Comparison | Compare/contrast with similar concepts | Learner confuses similar concepts |
| S-07 | Chunking | Break complex topic into small pieces | Complex topic, overwhelmed learner |
| S-08 | Spiral Learning | Revisit at increasing depth | Foundational concepts needing deep understanding |
| S-09 | Corrective Teaching | Clear correction with evidence | Learner has misconception |
| S-10 | Confidence Building | Easy wins + positive reinforcement | Underconfident or discouraged learner |
| S-11 | Challenge Extending | Push beyond current level | Bored, high-performing learner |
| S-12 | Multi-Modal Reinforcement | Same content, different modes | Struggling learner needs alternative access |
| S-13 | Peer Learning | Study with or teach others | Social learner, or mastery level (teach to learn) |
| S-14 | Reflective Practice | Self-explanation, summarization | Consolidation phase |
| S-15 | Interleaving | Mix different topics in one session | Advanced learner needing deeper integration |

### 8.2 Strategy Selection Algorithm

```
Input: Learner State + Target Fact(s) + Current Context
                          │
                          ▼
┌──────────────────────────────────────────────────────┐
│ 1. Filter by Learner Profile                          │
│    • Remove strategies learner has not responded to   │
│    • Remove strategies inappropriate for age          │
│    • Keep strategies matching learning style          │
└──────────────────────────────────────────────────────┘
                          │
                          ▼
┌──────────────────────────────────────────────────────┐
│ 2. Score by Context                                   │
│    • Learner confusion level → corrective strategies  │
│    • Learner boredom level → extending strategies     │
│    • Knowledge gap type → targeted strategies         │
│    • Fact type → natural affinity strategies          │
│    • Available time → depth-appropriate strategies    │
└──────────────────────────────────────────────────────┘
                          │
                          ▼
┌──────────────────────────────────────────────────────┐
│ 3. Score by History                                   │
│    • Strategies that worked before: +weight           │
│    • Strategies tried recently: -weight               │
│    • Strategies never tried: exploration bonus        │
└──────────────────────────────────────────────────────┘
                          │
                          ▼
┌──────────────────────────────────────────────────────┐
│ 4. Select Best Strategy                                │
│    • Highest composite score                          │
│    • With exploration noise (ε-greedy: 0.1)           │
└──────────────────────────────────────────────────────┘
                          │
                          ▼
                   Strategy + Confidence Score
```

### 8.3 Strategy Effectiveness Tracking

The Mentor tracks how effective each strategy is for this specific learner:

```json
{
  "learner_id": "usr_a1b2c3d4",
  "strategy_effectiveness": {
    "S-01_direct_instruction": {
      "times_used": 24,
      "avg_mastery_gain": 0.08,
      "avg_engagement_during": 0.75,
      "effectiveness_score": 0.72,
      "last_used": "2026-07-05"
    },
    "S-02_socratic": {
      "times_used": 18,
      "avg_mastery_gain": 0.12,
      "avg_engagement_during": 0.88,
      "effectiveness_score": 0.85,
      "last_used": "2026-07-08"
    },
    "S-04_storytelling": {
      "times_used": 31,
      "avg_mastery_gain": 0.10,
      "avg_engagement_during": 0.92,
      "effectiveness_score": 0.91,
      "last_used": "2026-07-09"
    }
  }
}
```

---

## 9. CONTENT RECOMMENDATION ENGINE

### 9.1 Recommendation Types

| Type | Description | Trigger | Frequency |
|---|---|---|---|
| **Next Action** | Single best action right now | Session start, task completion | Real-time |
| **Daily Plan** | Full day's learning schedule | Daily (learner's preferred time) | Daily |
| **Weekly Focus** | Theme and goals for the week | Weekly (Sunday) | Weekly |
| **Weakness Intervention** | Targeted content for weak areas | Weakness detected | As needed |
| **Review Session** | Consolidated review plan | Forgetting risk detected | Per schedule |
| **Exploration Suggestion** | New topic to explore | Interest detected or boredom | Weekly |
| **Challenge Recommendation** | Quiz/quest recommendation | Readiness detected | As needed |
| **Social Suggestion** | Study group or peer matching | Social engagement goal | Weekly |
| **Break Suggestion** | Take a break, reflect | Fatigue or frustration detected | Real-time |
| **Long-Term Path Adjustment** | Curriculum modification | Progress data review | Monthly |

### 9.2 Content Scoring Algorithm

```
score(content, learner) =
    relevance_weight × relevance_score(content, learner.knowledge_state)
    + freshness_weight × (1 - recency_bonus(content, learner.recent_content))
    + modality_weight × modality_affinity(content.type, learner.preferences)
    + difficulty_weight × difficulty_match(content.difficulty, learner.optimal_difficulty)
    + weakness_weight × weakness_coverage(content, learner.weaknesses)
    + variety_weight × mode_diversity(content, learner.recent_modes)
    + engagement_weight × predicted_engagement(content, learner.history)
    + novelty_weight × exploration_bonus(content, learner)
```

### 9.3 Recommended Content Types Per Goal

| Learner Goal | Best Content Type | Secondary | Avoid When |
|---|---|---|---|
| **New concept introduction** | Story | AI Teacher dialogue | Quiz (too early) |
| **Fact memorization** | Flashcard | Quiz (recall type) | Podcast (passive) |
| **Deep understanding** | AI Teacher (Socratic) | Interactive story | Flashcard (too shallow) |
| **Practical application** | Scenario-based quiz | Interactive story | Lecture-style content |
| **Review/consolidation** | Quiz | Flashcard | New content |
| **Interest exploration** | Documentary | Podcast | Assessment |
| **Mastery demonstration** | Challenge | Teaching others | Passive content |
| **Confidence building** | Easy quiz (guaranteed success) | Familiar story | Hard challenges |
| **Remedial/corrective** | AI Teacher (corrective) | Different-modality content | Same approach that failed |

### 9.4 Daily Plan Generation

The Mentor generates a daily plan that balances all learner needs:

```
Learner: Ahmed (usr_a1b2c3d4)
Date: Saturday, July 11, 2026
Preferred Time: 7:00 PM - 7:30 PM (30 min session)

━━ DAILY PLAN ━━

📌 REVIEW (12 min) — Priority: Critical
  • [Critical] Asma wa Sifat - Negation (flashcard, 3 min)
    ⚠ Last reviewed 21 days ago, mastery dropped to 0.45
  • [Critical] Salah - Conditions (mini-quiz, 3 min)
    ⚠ 72% probability of incorrect answer
  • [Review] Conditions of Zakat (flashcard, 2 min)
  • [Review] Hajj Pillars (quick quiz, 2 min)
  • [Review] Tawheed al-Rububiyyah (1 min — already strong)

📚 NEW LEARNING (10 min)
  • Topic: Asma wa Sifat — Foundation (Story Mode, 5 min)
    📖 Story: "The Beautiful Names — Understanding Allah's Attributes"
    📍 Why: Prerequisite for the weak area identified
  • AI Teacher check-in (5 min)
    🎯 Focus: Verify understanding of the story's key concepts

🎯 CHALLENGE (5 min)
  • Daily Challenge: 3 questions on "Tawheed & Its Categories"
    🏆 Bonus: Streak-protected if completed

📊 SESSION SUMMARY
  • Total time: ~27 min
  • Facts to review: 5
  • New facts to learn: 3-5
  • Predicted mastery gain: +0.03 to +0.05
  • Forgetting risk reduction: -0.15 in targeted areas
```

---

## 10. DAILY MENTORSHIP CYCLE

### 10.1 Cycle Phases

```
┌─────────────────────────────────────────────────────────────────────────┐
│                     DAILY MENTORSHIP CYCLE                               │
│                                                                          │
│  ┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐              │
│  │ PREPARE  │───►│ NOTIFY  │───►│ GUIDE   │───►│ ADAPT   │              │
│  │(00:00AM) │    │(6:00PM) │    │(7:00PM) │    │(During) │              │
│  └─────────┘    └─────────┘    └─────────┘    └─────────┘              │
│       │              │              │              │                     │
│       ▼              ▼              ▼              ▼                     │
│  • Update model    • Send daily   • Learner     • Monitor real-time    │
│  • Run predictions   plan push     starts session  • Adjust difficulty │
│  • Generate plan   • Learner        • Follow plan  • Switch strategies │
│  • Queue content     reviews plan  • Mentor prods  • Detect issues     │
│                     • Confirms or  • Offers help  • End-of-session     │
│                       adjusts                     reflection           │
│                                                                          │
│  ┌──────────────────────────────────────────────────────────────────┐   │
│  │                      REFLECT (Post-session)                      │   │
│  │  • Analyze session outcomes                                      │   │
│  │  • Update strategy effectiveness                                 │   │
│  │  • Update learner model                                          │   │
│  │  • Adjust next day's plan based on today's results               │   │
│  └──────────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────────┘
```

### 10.2 Phase Details

**PREPARE (Midnight Batch):**
- Process all learning events from the previous day
- Update learner model (mastery, stability, confidence per fact)
- Run forgetting predictor for all known facts
- Run weakness predictor for all domains
- Generate tomorrow's daily plan
- Queue content generation requests (stories, quizzes, flashcards)
- Calculate daily review budget based on learner's history

**NOTIFY (Learner's Preferred Time - 1 hour):**
- Send daily plan notification with summary
- Highlight critical items and estimated duration
- Learner reviews, can request changes (shorter/longer, focus change)
- Mentor adjusts plan if requested
- Confirm plan and remind at start time

**GUIDE (During Session):**
- Present first recommended action
- After each action completion, update state and recommend next
- Offer AI Teacher for deeper questions
- Provide encouragement and progress feedback
- Detect early signs of fatigue, boredom, or frustration
- Proactively suggest breaks or mode changes

**ADAPT (Real-time During Session):**
- If learner struggles: lower difficulty, switch strategy
- If learner excels: increase difficulty, offer extension
- If learner bored: switch content type, add challenge
- If learner distracted: gentle redirect, break suggestion
- If learner confused: offer AI Teacher, alternative explanation
- Track pacing: adjust remaining plan to fit time budget

**REFLECT (Post-Session):**
- Calculate session effectiveness (mastery gained, facts reviewed)
- Update strategy effectiveness scores
- Update content affinity scores
- Update attention span estimates
- Update forgetting model calibration
- Queue tomorrow's preparation with new data

---

## 11. WEEKLY MENTORSHIP CYCLE

### 11.1 Weekly Review (Sunday)

The Mentor generates a weekly reflection and plan:

```
━━ WEEK IN REVIEW ━━
July 6-12, 2026 | Learner: Ahmed

📊 THIS WEEK'S STATS
  • Sessions completed: 5 of 7 target (71%)
  • Total learning time: 2h 15m (avg 27m/session)
  • New facts learned: 14
  • Facts reviewed: 42
  • Quiz accuracy: 82% (+3% vs last week)
  • Streak: 17 days (🔥 on fire!)
  • Badges earned: Tawheed Apprentice 🏅

📈 MASTERY GROWTH
  • Aqidah: 0.74 → 0.78 (+0.04) ✅
  • Fiqh: 0.62 → 0.63 (+0.01) ⚠️ Slow growth
  • Seerah: 0.55 → 0.60 (+0.05) ✅
  • Arabic: Not started ❌

🔍 WEAKNESS SPOTLIGHT
  • Fiqh of Zakat: Mastery dropped to 0.50 — needs attention
  • Arabic alphabet: You haven't started yet! Ready to begin?

🎯 NEXT WEEK'S FOCUS
  • Theme: "Worship and Practice" — deepen Fiqh understanding
  • Daily target: 25 min sessions (up from 20)
  • Milestone: Complete Zakat module
  • Optional: Start Arabic basics (5 min/day)

💡 MENTOR NOTE
  "You've made great progress in Aqidah this month.
  Your consistency is excellent. Let's channel that
  same energy into Fiqh next week. Remember: knowledge
  of worship enhances worship. 🇮🇩"
```

### 11.2 Weekly Plan Generation

| Step | Action | Data Used |
|---|---|---|
| 1 | Calculate weekly mastery growth per domain | Learner Model |
| 2 | Identify top weakness trends | Weakness Predictor (7-day window) |
| 3 | Calculate forgetting risk per domain | Forgetting Predictor |
| 4 | Set weekly theme and primary goal | Curriculum Path + Weaknesses |
| 5 | Allocate time budget per domain | Learner availability + priorities |
| 6 | Select weekly challenge/quest | Gamification System |
| 7 | Generate weekly content queue | Content Recommendation Engine |
| 8 | Calculate predicted outcomes | Historical effectiveness rates |

---

## 12. MONTHLY MENTORSHIP CYCLE

### 12.1 Monthly Review

```
━━ MONTH IN REVIEW ━━
June 10 - July 10, 2026 | Learner: Ahmed

📈 30-DAY GROWTH
  • Overall mastery: 0.58 → 0.67 (+0.09)
  • Facts mastered: 14 → 22 (+8)
  • Facts learning: 25 → 18 (-7 — progress!)
  • New domains entered: 2 (Fiqh, Seerah)

🏆 ACHIEVEMENTS
  • Completed: Foundations of Aqidah (Level 1)
  • Unlocked: Fiqh level 2 content
  • Streak record: 17 days (current)
  • Quiz accuracy: 78% → 82% (+4%)

🎯 GOAL TRACKING
  • Primary goal: "Complete Foundations of Islam" — 35% complete
  • On track? Yes (estimated completion: Dec 2026)
  • Adjust? Increase Fiqh time allocation

🔮 NEXT MONTH PREDICTION
  • Expected mastery gain: +0.08 to +0.12
  • Expected facts to master: 8-12 new facts
  • Risk: Arabic language unit may slow progress
  • Recommendation: Start Arabic at 5min/day in addition to current plan
```

### 12.2 Monthly Adjustment

| Decision | Criteria | Adjustment |
|---|---|---|
| **Accelerate** | 2+ months ahead of pace | Increase difficulty, add topics |
| **Decelerate** | 2+ months behind | Reduce scope, extend timeline |
| **Course Correct** | New interests or goals | Add/remove topics, adjust focus |
| **Intervention** | Motivation dropped 2+ months | Change approach, set new goals |
| **Celebrate** | Major milestone reached | Recognition, new challenge unlocked |

---

## 13. LONG-TERM MENTORSHIP

### 13.1 Mentor Relationship Evolution

| Phase | Duration | Mentor Role | Learner Autonomy |
|---|---|---|---|
| **Onboarding** | Week 1 | Director — tells learner what to do | Very Low |
| **Foundation** | Months 1-3 | Guide — explains why, offers choices | Low |
| **Growth** | Months 3-12 | Coach — suggests, learner decides | Medium |
| **Independence** | Year 2+ | Advisor — available when asked | High |
| **Mastery** | Year 3+ | Partner — collaborates on goals | Very High |
| **Teaching** | Year 4+ | Facilitator — learner teaches others | Full |

### 13.2 Quarterly Goal Setting

Every quarter (or upon request), the Mentor conducts a goal-setting session:

```
━━ QUARTERLY GOAL SETTING ━━
Q3 2026 (July - September) | Learner: Ahmed

📊 PROGRESS REVIEW
  • Q2 Goal: Complete Aqidah Foundations — 100% complete ✅
  • Q2 Goal: Start Fiqh — 60% complete ⏳
  • Q2 Goal: 90-day streak — Missed (reached 45 days) ⚠️

🎯 Q3 GOALS (Suggested by Mentor)
  1. Primary: Complete Fiqh of Worship (Salah, Zakat, Sawm)
     - Measurable: Score > 80% on Fiqh comprehensive quiz
     - Timeline: September 30
  2. Secondary: Learn 50 new Arabic vocabulary words
     - Measurable: 90% accuracy on Arabic review
     - Timeline: September 30
  3. Stretch: Earn the "Fiqh Scholar" badge
     - Measurable: Complete all Fiqh challenges at Hard difficulty
  4. Habit: Maintain > 5 sessions/week average
     - Measurable: Weekly activity report

Learner can: Accept, Modify, Replace, or Decline each goal.
The Mentor remembers the decision and adjusts future recommendations.
```

---

## 14. MENTOR-LEARNER RELATIONSHIP

### 14.1 Communication Channels

| Channel | Purpose | Frequency | Format |
|---|---|---|---|
| **Daily Plan Push** | Deliver daily schedule | Daily | Notification + in-app card |
| **Session Check-in** | Start session greeting | Each session | In-app message |
| **Progress Nudge** | Encouragement or redirect | As needed | Push notification |
| **Weekly Reflection** | Weekly review | Weekly | In-app report |
| **Monthly Review** | Monthly deep dive | Monthly | In-app report + email |
| **Milestone Celebration** | Achievement recognition | Per milestone | Notification + animation |
| **Goal Setting** | Quarterly goal session | Quarterly | Interactive in-app |
| **Intervention Alert** | Concern detection | As needed | Notification + optional AI Teacher |
| **Learner Query** | Learner asks Mentor | On demand | Chat-like interface |
| **Mentor Insights** | Mentor shares observation | Weekly | In-app card |

### 14.2 Mentor Tone Adaptation

| Learner State | Tone | Language Pattern |
|---|---|---|
| **Confident / Successful** | Celebratory | "Excellent work! Your understanding is growing beautifully." |
| **Struggling** | Encouraging | "This is a challenging concept. Let's try a different approach." |
| **Frustrated** | Patient | "It's okay to find this difficult. Every scholar started where you are." |
| **Bored** | Challenging | "Ready for something more interesting? I have just the challenge." |
| **Fatigued** | Gentle | "You've done well today. Rest is part of learning too." |
| **Consistent** | Appreciative | "Your consistency is inspiring. This is how knowledge grows." |
| **Returning after break** | Welcoming | "Welcome back! Your knowledge is waiting for you. Let's ease in gently." |

### 14.3 Learner Override Rights

The learner can at any time:

| Override | Effect | Mentor Response |
|---|---|---|
| **Skip recommendation** | Remove from today's plan | "No problem. We'll reschedule for tomorrow." |
| **Change focus topic** | Shift daily plan to prefered topic | "Great choice! Let me adjust the plan." |
| **Request harder/easier** | Adjust difficulty parameters | "Adjusting now. Tell me if this feels right." |
| **Change content type** | Switch to prefered mode | "Switching to story mode. Enjoy!" |
| **Extend/shorten session** | Adjust time budget | "I'll adjust the plan to fit your time." |
| **Pause tracking** | Stop predictions for a set period | "Tracking paused until [date]. I'll be here when you return." |
| **Request explanation** | Ask why a recommendation was made | "I recommended this because..." (transparent reasoning) |
| **Permanently disable** | Turn off Mentor features | "Mentor features disabled. You can re-enable anytime in settings." |

---

## 15. MENTOR-AI TEACHER INTEGRATION

### 15.1 Role Separation

| Aspect | Mentor | AI Teacher |
|---|---|---|
| **Primary Function** | Strategic guidance, planning, prediction | Tactical conversation, explanation, dialogue |
| **Scope** | All domains, all timeframes | Current topic, current session |
| **Initiative** | Proactive (suggests, plans, predicts) | Reactive (responds to learner queries) |
| **Memory** | Complete learner history | Session context only |
| **Decision** | What to learn, when, how, why | How to explain this specific question |
| **Output** | Plans, recommendations, insights | Dialogue turns, explanations |
| **Relationship** | Coach, guide, strategist | Tutor, explainer, conversationalist |

### 15.2 Integration Protocol

```
Learner asks AI Teacher a question
          │
          ▼
┌─────────────────────────────────────┐
│ AI Teacher handles conversation     │
│ • Answers question                  │
│ • Generates follow-ups              │
│ • Emits TEACHER_INTERACTION event   │
└─────────────────────────────────────┘
          │
          ▼
┌─────────────────────────────────────┐
│ Mentor receives event               │
│ • Updates learner model             │
│   - Fact mentioned → exposure       │
│   - Learner response → mastery hint │
│ • Checks prediction accuracy        │
│   - Did we predict this question?   │
│   - Update weakness model           │
│ • Generates post-session insight    │
└─────────────────────────────────────┘
          │
          ▼
┌─────────────────────────────────────┐
│ Mentor may suggest follow-up        │
│ • "Based on your question, you     │
│   might enjoy this story on..."     │
│ • "Let's add this to your review   │
│   schedule for next week."          │
└─────────────────────────────────────┘
```

### 15.3 Mentor Instructions to AI Teacher

When the Mentor identifies a need for conversation:

```json
{
  "instruction_id": "mentor_inst_007",
  "learner_id": "usr_a1b2c3d4",
  "instruction_type": "teaching_session",
  "topic": {
    "fact_id": "KG-FACT-004530",
    "focus": "Asma wa Sifat - Negation"
  },
  "learner_context": {
    "current_mastery": 0.45,
    "known_related_facts": ["KG-FACT-004528", "KG-FACT-004529"],
    "misconception_suspected": true,
    "misconception_description": "Learner may think negation of attributes means absence of attributes"
  },
  "teaching_parameters": {
    "strategy": "corrective_teaching",
    "tone": "patient",
    "depth_level": 3,
    "age_group": "9-12",
    "language": "ar"
  },
  "follow_up_goal": "Verify the learner can correctly identify which attributes are negated vs affirmed"
}
```

---

## 16. MENTOR-CONTENT INTEGRATION

### 16.1 Content Type Selection Matrix

| Learner Need | Story | Quiz | Flashcard | Podcast | AI Teacher | Interactive |
|---|---|---|---|---|---|---|
| First exposure | ★★★★★ | ★☆☆☆☆ | ★☆☆☆☆ | ★★☆☆☆ | ★★★☆☆ | ★★☆☆☆ |
| Deep understanding | ★★★★☆ | ★★★☆☆ | ★★☆☆☆ | ★★★☆☆ | ★★★★★ | ★★★★☆ |
| Memorization | ★★☆☆☆ | ★★★★☆ | ★★★★★ | ★☆☆☆☆ | ★★★☆☆ | ★★☆☆☆ |
| Review | ★★☆☆☆ | ★★★★☆ | ★★★★★ | ★★☆☆☆ | ★★★☆☆ | ★★★☆☆ |
| Application | ★★★☆☆ | ★★★★★ | ★☆☆☆☆ | ★★☆☆☆ | ★★★★☆ | ★★★★★ |
| Interest exploration | ★★★★☆ | ★★☆☆☆ | ★☆☆☆☆ | ★★★★★ | ★★★☆☆ | ★★★☆☆ |
| Quick session | ★☆☆☆☆ | ★★★★☆ | ★★★★★ | ★★☆☆☆ | ★★★☆☆ | ★★☆☆☆ |
| Deep session | ★★★★★ | ★★★★☆ | ★★☆☆☆ | ★★★★☆ | ★★★★★ | ★★★★★ |
| Confidence building | ★★★★★ | ★★★☆☆ | ★★★★☆ | ★★★☆☆ | ★★★★☆ | ★★☆☆☆ |
| Corrective | ★★★☆☆ | ★★★☆☆ | ★★★☆☆ | ★☆☆☆☆ | ★★★★★ | ★★★★☆ |

### 16.2 Content Request Protocol

When the Mentor determines new content is needed:

```json
{
  "request_id": "mentor_content_req_0042",
  "learner_id": "usr_a1b2c3d4",
  "priority": "high",
  "purpose": "weakness_intervention",
  "content_spec": {
    "type": "story",
    "fact_ids": ["KG-FACT-004530", "KG-FACT-004529"],
    "learning_objective": "Understand that Allah's attributes are perfect and negation means absence of imperfection",
    "target_age_group": "9-12",
    "complexity": "moderate",
    "length": "medium",
    "narrative_angle": "concept_story",
    "moral_focus": "tawheed"
  },
  "learner_context_for_generation": {
    "known_facts": ["KG-FACT-004528"],
    "current_mastery": 0.45,
    "preferred_style": "descriptive",
    "language": "ar"
  },
  "deadline": "2026-07-11T18:00:00Z"
}
```

---

## 17. MENTOR-GAMIFICATION INTEGRATION

### 17.1 Gamification Decisions by Mentor

| Decision | Mentor's Role | Gamification System's Role |
|---|---|---|
| **Challenge difficulty** | Sets based on learner's readiness | Executes challenge |
| **Reward timing** | Determines when reward is most motivating | Issues reward |
| **Streak protection** | Activates based on learner's motivation state | Tracks streak |
| **Goal difficulty** | Adjusts to maintain optimal challenge | Tracks goal progress |
| **Badge eligibility** | Identifies readiness for badge | Issues badge |
| **Difficulty progression** | Controls pace of difficulty increase | Updates difficulty |
| **Break suggestions** | Recommends breaks when appropriate | No role |
| **Competition matching** | Matches learners of similar level | Runs competition |

### 17.2 Gamification Adaptation Rules

| Learner Signal | Gamification Response |
|---|---|
| Motivation low | Increase extrinsic rewards, add streak protection, offer easier wins |
| Motivation high | Reduce extrinsic rewards, add challenges, emphasize intrinsic value |
| Consistent performer | Add variety, new challenge types, mastery recognition |
| Returning after break | Reduce challenge, emphasize welcome rewards, protect from leaderboard drop |
| Underconfident | Guaranteed early wins, positive reinforcement, private achievements |
| Overconfident | Increase challenge difficulty, introduce harder comparisons |

---

## 18. MENTOR-CURRICULUM INTEGRATION

### 18.1 Curriculum Awareness

The Mentor knows the learner's position in every curriculum path:

```
Curriculum: "Foundations of Islam"
  Level 1: Foundations of Aqidah ─── 100% COMPLETE ✅
  Level 2: Understanding Islam ──── 35% complete
    Unit 1: Tawheed (Advanced) ──── 80% complete
      Module 1: Tawheed al-Rububiyyah ──── 100% 🏆
      Module 2: Tawheed al-Uluhiyyah ───── 90% ✅
      Module 3: Tawheed al-Asma wa Sifat ── 45% ⏳ ← Current
    Unit 2: Risalah & Prophethood ── 10% complete
    Unit 3: Angels & Divine Books ── 0% not started
```

### 18.2 Pacing Decisions

| Situation | Mentor Action |
|---|---|
| Learner ahead of curriculum pace | Enrich with deeper content, cross-domain connections |
| Learner behind pace | Reduce non-essential content, focus on core objectives |
| Learner struggling with module | Insert remedial loop, alternative content, more time |
| Learner excelling in module | Accelerate, offer extension, skip redundant practice |
| Learner disengaged from curriculum | Offer choice, interest-based detour, then reconnect |
| Curriculum updated | Identify gaps, plan catch-up, adjust timeline |

---

## 19. MENTOR-MEMORY ENGINE INTEGRATION

### 19.1 Data Exchange

The Mentor and Memory Engine have a bidirectional relationship:

```
Memory Engine → Mentor:
  • Review schedule for every fact
  • Ease factors per fact per learner
  • Retention probability predictions
  • Overdue fact counts
  • Memory health score (overall)

Mentor → Memory Engine:
  • Priority adjustments (weakness → higher priority)
  • Content mode preferences per fact type
  • Learner availability (review budget)
  • Forgetting curve calibration feedback
```

### 19.2 Review Priority Override

The Mentor can override the Memory Engine's default priority:

| Condition | Override | Rationale |
|---|---|---|
| Weakness detected | Increase priority 2x | Need to address gaps urgently |
| Upcoming curriculum topic | Pre-teach related facts | Prepare for upcoming content |
| Learner interested in topic | Increase priority 1.5x | Capitalize on motivation |
| Learner fatigued | Decrease priority for hard facts | Avoid frustration during tiredness |
| Assessment tomorrow | Priority for assessed facts | Peak before evaluation |
| Holiday break approaching | Schedule reviews before break | Prevent forgetting during hiatus |

---

## 20. MENTOR-SOCIAL INTEGRATION

### 20.1 Social Recommendations

| Condition | Recommendation |
|---|---|
| Learner has high mastery in a topic | Suggest teaching/helping others in that topic |
| Learner struggling with a concept | Suggest finding a study buddy at similar level |
| Learner has consistent habits | Suggest joining a study group for accountability |
| Learner shows interest in discussion | Suggest relevant forum thread or Q&A |
| Learner is competitive | Suggest challenges, friendly competition (opt-in) |
| Learner is advanced | Suggest mentorship program (teach beginners) |
| Learner is isolated | Suggest community events, group quests |

### 20.2 Group Dynamics Monitoring

The Mentor monitors the learner's social interactions for:
- Healthy participation rate vs. lurking
- Positive vs. negative sentiment in interactions
- Help received vs. help given balance
- Study group progress alignment
- Peer review quality and constructiveness

---

## 21. COMMUNICATION PROTOCOLS

### 21.1 Events Emitted

| Event | Trigger | Payload | Consumers |
|---|---|---|---|
| `mentor.daily_plan.ready` | Daily plan generated | plan_id, learner_id, summary | Notification System |
| `mentor.recommendation.made` | Content recommended | learner_id, content_type, content_id, reason | Content Delivery |
| `mentor.weakness.detected` | Critical weakness found | learner_id, fact_ids, severity, action | AI Teacher, Content Gen |
| `mentor.forgetting.alert` | Forgetting risk critical | learner_id, fact_ids, probability | Notification System |
| `mentor.strategy.selected` | Teaching strategy chosen | learner_id, strategy_id, context | AI Teacher |
| `mentor.insight.generated` | Weekly/monthly insight | learner_id, insight_type, content | Notification System |
| `mentor.goal.updated` | Goal added/modified | learner_id, goal_id, changes | Progress Tracking |
| `mentor.intervention.triggered` | Learner distress detected | learner_id, signal_type, severity | AI Teacher, Support |
| `mentor.session.complete` | Learning session ended | learner_id, session_summary | Progress Tracking, Analytics |

### 21.2 Events Consumed

| Event | Source | Payload | Mentor Action |
|---|---|---|---|
| `learning.event.recorded` | Progress Tracking | event_type, learner_id, metadata | Update learner model |
| `quiz.answered` | Quiz System | learner_id, question_id, correct, response_time | Update mastery, check predictions |
| `flashcard.reviewed` | Flashcard System | learner_id, fact_id, ease, correct | Update stability, interval |
| `story.completed` | Story Mode | learner_id, story_id, comprehension_score | Update exposures, mastery |
| `teacher.interaction` | AI Teacher | learner_id, topic, sentiment, duration | Update interest, check confusion |
| `session.started` | Client | learner_id, timestamp | Track session start |
| `session.ended` | Client | learner_id, timestamp, duration | Calculate session effectiveness |
| `gamification.achievement` | Gamification | learner_id, achievement_type | Update motivation model |
| `social.interaction` | Social Learning | learner_id, interaction_type | Update social model |

---

## 22. DATA STRUCTURES

### 22.1 Core Entities

```
LearnerModel {
  learner_id: String (PK)
  identity: IdentityProfile
  preferences: LearnerPreferences
  goals: LearnerGoals
  cognitive_state: Map<FactID, FactState>  // per-fact mastery
  domain_summaries: Map<DomainID, DomainSummary>
  behavioral_profile: BehavioralProfile
  motivational_state: MotivationalState
  social_profile: SocialProfile
  strategy_history: Map<StrategyID, StrategyEffectiveness>
  created_at: Timestamp
  updated_at: Timestamp
  version: Integer
}

FactState {
  fact_id: String (PK)
  mastery_level: Integer (0-5)
  confidence: Float (0.0-1.0)
  stability: Float (0.0-1.0)
  last_reviewed: Timestamp
  next_review: Timestamp
  interval_days: Float
  ease_factor: Float
  times_correct: Integer
  times_incorrect: Integer
  avg_response_time_ms: Float
  exposure_count: Integer
  exposure_modalities: List<String>
  learning_events: List<LearningEventSummary>
}

LearningEventSummary {
  event_type: String
  modality: String
  timestamp: Timestamp
  success: Boolean
  response_time_ms: Integer
}

DailyPlan {
  plan_id: String (PK)
  learner_id: String (FK)
  date: Date
  estimated_duration_min: Integer
  items: List<PlanItem>
  total_reviews: Integer
  total_new_items: Integer
  predicted_mastery_gain: Float
  status: PENDING | CONFIRMED | IN_PROGRESS | COMPLETED | PARTIALLY_COMPLETED
  learner_feedback: PlanFeedback
  generated_at: Timestamp
}

PlanItem {
  item_id: String (PK)
  type: REVIEW | NEW_LEARNING | CHALLENGE | REFLECTION | BREAK
  priority: CRITICAL | HIGH | MEDIUM | LOW
  fact_ids: List<String>
  content_type: STORY | QUIZ | FLASHCARD | PODCAST | AI_TEACHER | INTERACTIVE
  content_id: String (optional)
  estimated_minutes: Integer
  status: PENDING | SKIPPED | COMPLETED | PARTIALLY_COMPLETED
  reason: String  // why this item was included
}

WeaknessReport {
  learner_id: String (PK)
  generated_at: Timestamp
  critical: List<WeaknessItem>
  moderate: List<WeaknessItem>
  emerging: List<WeaknessItem>
}

WeaknessItem {
  fact_id: String
  topic: String
  current_mastery: Float
  predicted_mastery_7d: Float
  probability_incorrect: Float
  root_cause: String
  recommended_action: String
  prerequisite_fact_id: String (optional)
}
```

---

## 23. EXECUTION FLOW

### 23.1 Mentor Service Lifecycle

```
System Boot
    │
    ▼
┌──────────────────────────────────────┐
│ 1. Load Learner Models                │
│    • Load from persistent store       │
│    • Validate model integrity         │
│    • Initialize in-memory cache       │
└──────────────────────────────────────┘
    │
    ▼
┌──────────────────────────────────────┐
│ 2. Register Event Consumers           │
│    • Subscribe to learning events     │
│    • Subscribe to system events       │
│    • Initialize stream processors     │
└──────────────────────────────────────┘
    │
    ▼
┌──────────────────────────────────────┐
│ 3. Start Scheduled Jobs               │
│    • Daily plan generation (midnight) │
│    • Weakness prediction (hourly)     │
│    • Forgetting prediction (hourly)   │
│    • Weekly review (Sunday)           │
│    • Monthly review (1st of month)    │
│    • Model cleanup (daily)            │
└──────────────────────────────────────┘
    │
    ▼
┌──────────────────────────────────────┐
│ 4. Start Real-Time Services           │
│    • Session optimizer                │
│    • Engagement detector              │
│    • Real-time recommendation API     │
└──────────────────────────────────────┘
    │
    ▼
Mentor Service Ready
```

### 23.2 Real-Time Recommendation Flow

```
Learner opens platform / completes activity
                │
                ▼
┌─────────────────────────────────────────────┐
│ 1. Get Learner Context                        │
│    • Current learner state (from cache)      │
│    • Current session state                    │
│    • Recently completed activity              │
└─────────────────────────────────────────────┘
                │
                ▼
┌─────────────────────────────────────────────┐
│ 2. Check for Alerts                          │
│    • Critical weakness requiring action?     │
│    • Forgetting risk critical?               │
│    • Engagement drop detected?               │
│    • Session fatigue risk?                   │
└─────────────────────────────────────────────┘
                │
                ▼
┌─────────────────────────────────────────────┐
│ 3. Score All Possible Next Actions           │
│    • Pending reviews (prioritized)           │
│    • Weakness interventions                  │
│    • New content aligned to curriculum       │
│    • Daily plan remaining items              │
│    • Exploration suggestions                 │
│    • Break suggestions (if fatigued)         │
└─────────────────────────────────────────────┘
                │
                ▼
┌─────────────────────────────────────────────┐
│ 4. Select Best Next Action                   │
│    • Highest composite score                 │
│    • Context-appropriate                     │
│    • Within time budget                      │
└─────────────────────────────────────────────┘
                │
                ▼
┌─────────────────────────────────────────────┐
│ 5. Present Recommendation                    │
│    • Show to learner with reason             │
│    • Offer alternative if appropriate        │
│    • Execute on learner confirmation         │
└─────────────────────────────────────────────┘
```

### 23.3 Daily Plan Generation Flow

```
Midnight Trigger
    │
    ▼
┌─────────────────────────────────────────────┐
│ 1. Process Yesterday's Results               │
│    • All learning events processed           │
│    • Model updated                           │
│    • Strategy effectiveness recalculated     │
└─────────────────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────────────────┐
│ 2. Run Predictions                           │
│    • Forgetting predictor (all known facts)  │
│    • Weakness predictor (all active domains) │
│    • Engagement predictor (motivation trend) │
└─────────────────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────────────────┐
│ 3. Compute Review Budget                     │
│    • Available time per learner preference   │
│    • Critical/overdue items count            │
│    • New items to introduce                  │
│    • Balance across domains                  │
└─────────────────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────────────────┐
│ 4. Generate Plan Items                       │
│    • Priority 1: Critical reviews            │
│    • Priority 2: Weakness interventions      │
│    • Priority 3: Curriculum progression      │
│    • Priority 4: Exploration/enrichment      │
│    • Priority 5: Challenge/quest             │
│    • Priority 6: Reflection/review           │
└─────────────────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────────────────┐
│ 5. Validate & Optimize                       │
│    • Total duration within budget?           │
│    • Appropriate mix of content types?       │
│    • Difficulty properly calibrated?         │
│    • Adjust if needed (trim/expand)          │
└─────────────────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────────────────┐
│ 6. Queue Content Generation                  │
│    • Request any needed new content          │
│    • Pre-cache planned content               │
└─────────────────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────────────────┐
│ 7. Store Plan & Schedule Notification        │
│    • Persist plan for learner's access       │
│    • Schedule push notification              │
└─────────────────────────────────────────────┘
```

---

## 24. STATE MANAGEMENT

### 24.1 Mentor Agent States

```
┌──────────┐
│ BOOTING  │ Loading models and connecting to services
└────┬─────┘
     │
┌────▼─────┐
│ INITIALIZED │ Models loaded, services connected
└────┬─────┘
     │
┌────▼─────┐
│ ACTIVE   │ Processing events, generating recommendations
└────┬─────┘
     │
┌────▼─────┐
│ DEGRADED │ Some predictions unavailable, operating with cached data
└────┬─────┘
     │
┌────▼─────┐
│ RECOVER  │ Restoring from checkpoint after failure
└────┬─────┘
     │
┌────▼─────┐
│ SHUTDOWN │ Graceful shutdown, saving state
└──────────┘
```

### 24.2 Learner Session States

```
┌──────────┐
│ IDLE     │ Learner not currently active
└────┬─────┘
     │
┌────▼─────┐
│ ENGAGED  │ Learner actively learning
└────┬─────┘
     │
┌────▼─────┐
│ STUCK    │ Learner struggling, not progressing
└────┬─────┘
     │
┌────▼─────┐
│ FATIGUED │ Learner showing signs of fatigue
└────┬─────┘
     │
┌────▼─────┐
│ DISTRACTED │ Learner not responding, likely distracted
└────┬─────┘
     │
┌────▼─────┐
│ COMPLETED │ Session ended naturally
└──────────┘
```

---

## 25. CACHING AND MEMORY

### 25.1 Cache Strategy

| Cache Layer | Content | TTL | Size Target |
|---|---|---|---|
| **L1: In-Memory** | Active learner models (recent 1hr) | 1 hour | 10,000 learners/node |
| **L2: Redis** | Learner models (24hr), daily plans, predictions | 24 hours | 1M learners |
| **L3: Database** | Full learner history, all models | Permanent | Unlimited |

### 25.2 Memory Architecture

| Memory Type | Content | Storage | Access Pattern |
|---|---|---|---|
| **Working Memory** | Current session context, recent 50 events | In-memory | Real-time read/write |
| **Short-Term Memory** | Last 24h of activity, recent recommendations | Redis | Frequent read/write |
| **Long-Term Memory** | Complete learner model, all history | PostgreSQL | Persistent |
| **Prediction Cache** | Current predictions, next review schedule | Redis | Frequent read, periodic write |
| **Content Cache** | Recently recommended content IDs | Redis | Frequent read |

---

## 26. PERSISTENCE

### 26.1 Data Stores

| Store | Data | Technology | Replication |
|---|---|---|---|
| **Learner Model Store** | All learner models, cognitive state | PostgreSQL (partitioned by learner_id) | Primary + Read replicas |
| **Event Store** | All learning events (time-series) | TimescaleDB | 3x replication |
| **Prediction Store** | Weakness/forgetting predictions | PostgreSQL | Primary + Read replicas |
| **Plan Store** | Daily/weekly/monthly plans | PostgreSQL | Primary |
| **Cache** | Active models, recommendations | Redis Cluster | 3x replication |
| **Analytics Store** | Aggregate metrics, trends | ClickHouse | 2x replication |

### 26.2 Retention Policies

| Data | Hot (Fast Access) | Warm (Queryable) | Cold (Archive) | Deletion |
|---|---|---|---|---|
| Learner Model | Current version | Previous 30 versions | Annual snapshots | 7yr or account deletion |
| Learning Events | 7 days | 90 days | 7 years | Account deletion |
| Daily Plans | 30 days | 1 year | 7 years | Account deletion |
| Predictions | 7 days | Not stored | Not stored | N/A |
| Session Logs | 1 day | 30 days | 1 year | Anonymized after 1yr |

---

## 27. VALIDATION AND RETRY LOGIC

### 27.1 Input Validation

| Input | Validation | Action on Failure |
|---|---|---|
| Learning event | Schema validation, required fields, valid learner_id | Drop event, log error |
| Learner preference update | Range checks, allowed values | Reject with error message |
| Recommendation request | Learner_id exists, valid context | Return fallback recommendation |
| Content generation request | Valid fact_ids, valid content type | Reject with error details |

### 27.2 Retry Policies

| Operation | Max Retries | Backoff | On Failure |
|---|---|---|---|
| Model update | 3 | Exponential (100ms, 500ms, 2s) | Log and continue with stale model |
| Prediction computation | 2 | Fixed (1s) | Return previous prediction |
| Content generation request | 3 | Exponential (1s, 4s, 9s) | Skip content, use alternative |
| Event processing | 5 | Exponential (100ms, 400ms, 1.6s, 6.4s, 25.6s) | Dead letter queue |
| Database write | 3 | Exponential (100ms, 500ms, 2s) | Circuit breaker, alert |

---

## 28. ERROR RECOVERY

### 28.1 Error Classification

| Error Type | Severity | Recovery | Impact |
|---|---|---|---|
| Model load failure | HIGH | Retry from backup, rebuild | Mentor unavailable for affected learners |
| Prediction failure | MEDIUM | Use cached predictions | Stale recommendations |
| Event processing failure | LOW | Dead letter queue, reprocess | Delayed model updates |
| Database connection loss | CRITICAL | Failover to replica, circuit breaker | Read-only mode, no updates |
| Cache failure | MEDIUM | Bypass cache, read from DB | Increased latency |
| Content generation failure | LOW | Skip content, use alternative | Learner gets different content |

### 28.2 Graceful Degradation

| Missing Component | Mentor Behavior |
|---|---|
| Forgetting Predictor | Use default SM-2 algorithm |
| Weakness Predictor | Recommend based on lowest mastery scores |
| Content Generation | Recommend existing content only |
| AI Teacher | Skip conversation recommendations |
| Learner Model (partial) | Use available data, mark predictions as low-confidence |
| KG Access | Use cached fact metadata |

---

## 29. SECURITY AND PRIVACY

### 29.1 Data Privacy

| Principle | Implementation |
|---|---|
| **Learner Data Ownership** | All learner model data belongs to the learner |
| **Data Encryption** | AES-256 at rest, TLS 1.3 in transit |
| **Access Control** | Only Mentor service and learner can access full model |
| **Anonymization** | Analytics data anonymized after 90 days |
| **Export** | Full learner model export available in JSON |
| **Deletion** | Complete model deletion on account closure |
| **Minimum Data** | Only collect data needed for mentorship |
| **Transparency** | Learner can view their complete model anytime |

### 29.2 Model Access Permissions

| Role | Learner Model Access | Prediction Access | Plan Access |
|---|---|---|---|
| Learner | Full | Full | Full |
| Parent (if child account) | Progress summary only | No | Daily plan view |
| AI Teacher | Current session context only | No | No |
| Mentor Service | Full | Full | Full |
| Administrator | Anonymized aggregates only | No | No |
| Analytics | Anonymized, aggregated only | No | No |

---

## 30. MONITORING AND METRICS

### 30.1 Key Performance Indicators

| Metric | Target | Measurement |
|---|---|---|
| **Recommendation Acceptance Rate** | > 80% | Learner follows Mentor recommendation |
| **Prediction Accuracy (Weakness)** | > 85% | Weakness predicted before quiz failure |
| **Prediction Accuracy (Forgetting)** | > 80% | Forgetting predicted before recall failure |
| **Mastery Growth per Session** | > 0.03 avg | Average mastery gain per learning session |
| **Learner Satisfaction** | > 4.5/5 | Monthly learner survey |
| **Session Completion Rate** | > 80% | Sessions completed vs started |
| **Daily Plan Completion Rate** | > 70% | Planned items completed |
| **Long-Term Retention** | > 80% at 30 days | Mastery retention after 30d without review |
| **Engagement Lift** | +20% vs non-mentored | Comparison with control group |
| **Model Update Latency** | < 5s P95 | Time from event to model update |

### 30.2 Monitoring Dashboards

| Dashboard | Audience | Content |
|---|---|---|
| **Mentor Health** | Engineering | Service health, prediction latency, model freshness |
| **Prediction Accuracy** | Data Science | Weakness/forgetting prediction accuracy trends |
| **Learner Outcomes** | Product | Mastery growth, retention, engagement by cohort |
| **Recommendation Effectiveness** | Product | Acceptance rates by recommendation type |
| **Model Quality** | Data Science | Drift detection, data quality, coverage |
| **Cost & Performance** | Operations | Compute cost per learner, response times |

### 30.3 Alerting Rules

| Alert | Condition | Severity | Channel |
|---|---|---|---|
| **Model Update Lag** | Events unprocessed > 5min | P2 | Slack, Dashboard |
| **Prediction Accuracy Drop** | Accuracy drops below 70% | P1 | Slack, SMS |
| **Recommendation Acceptance Drop** | Rate drops below 50% | P2 | Slack |
| **Service Down** | No heartbeat for 30s | P0 | SMS, Phone |
| **Model Corruption** | Integrity check fails | P0 | SMS, Phone |
| **Data Pipeline Failure** | Events not flowing | P1 | Slack, SMS |

---

## 31. SCALABILITY AND PERFORMANCE

### 31.1 Scalability Targets

| Metric | Target | Scaling Strategy |
|---|---|---|
| **Concurrent Learners** | 100,000 | Horizontal sharding by learner_id |
| **Events Processed/Second** | 50,000 | Stream processing cluster |
| **Model Updates/Second** | 10,000 | Batch updates with eventual consistency |
| **Recommendation Latency** | < 100ms P95 | Cached models + pre-computed scores |
| **Daily Plan Generation** | 1M plans/hour | Distributed batch processing |
| **Prediction Computation** | 10M facts/minute | Distributed map-reduce |
| **Storage Growth** | 1TB/month | Partitioning + archival |

### 31.2 Performance Budgets

| Operation | Budget | Measurement |
|---|---|---|
| Real-time recommendation | 100ms | P95 from request to response |
| Model update after event | 1s | P95 from event receipt to model write |
| Daily plan generation | 5s per learner | P95 |
| Weakness prediction (full scan) | 10s per domain per learner | P95 |
| Weekly report generation | 30s per learner | P95 |
| Model load on startup | 500ms per learner | P95 |

### 31.3 Sharding Strategy

```
Shard Key: learner_id (hash-based)

Shard 0: learner_id hash 00-1F
  └─ Mentor instance 0
     ├─ Learner models (PostgreSQL partition)
     ├─ Event processor
     └─ Prediction engine

Shard 1: learner_id hash 20-3F
  └─ Mentor instance 1
     ├─ ...

...
Up to 256 shards for 100K+ concurrent learners
```

---

## 32. TESTING STRATEGY

### 32.1 Testing Levels

| Level | Focus | Methods | Frequency |
|---|---|---|---|
| **Unit** | Individual predictors, recommenders, planners | Isolation tests with mock data | Every commit |
| **Integration** | Component interactions, event processing | Service-level tests with test containers | Every PR |
| **System** | Full mentor workflow | E2E tests with simulated learners | Every deployment |
| **Performance** | Latency, throughput, scalability | Load testing, stress testing | Weekly |
| **A/B Testing** | Mentor vs. control, strategy variants | Production experimentation | Continuous |
| **Chaos** | Failure scenarios, degradation | Fault injection | Monthly |

### 32.2 Test Scenarios

| Scenario | Type | Expected Outcome |
|---|---|---|
| Learner completes quiz → model updated → next recommendations adjusted | Integration | Mastery updated, recommendations reflect new state |
| Learner model corrupted → recovery from backup | System | Model restored, predictions resume |
| 10,000 learners start sessions simultaneously | Performance | All recommendations within 100ms P95 |
| Forgetting predictor offline → fallback to SM-2 | Chaos | Recommendations continue with lower quality |
| New learner with no history → onboarding recommendations | Integration | Appropriate beginner content recommended |
| Learner ignores 5 recommendations → engagement intervention | Integration | Mentor detects pattern, adjusts approach |

---

## 33. RISK ANALYSIS

| Risk | Probability | Impact | Mitigation |
|---|---|---|---|
| **Prediction inaccuracy leads to bad recommendations** | Medium | High | Continuous accuracy monitoring, fallback strategies, learner override |
| **Learner model grows too large** | Medium | Medium | Archival strategy, compression, partition by time |
| **Bias in recommendations** | Low | High | Regular bias audits, diverse content inclusion, transparent reasoning |
| **Learner data privacy breach** | Low | Critical | Encryption, access controls, regular security audits |
| **Mentor becomes annoying (over-engagement)** | Medium | Medium | Learner controls frequency, easy to reduce/dismiss |
| **Learner becomes dependent on Mentor** | Low | Medium | Progressive autonomy, goal to reduce guidance over time |
| **Model staleness during downtime** | Medium | Medium | Graceful degradation, cached predictions |
| **Gaming the system (learners optimizing for metrics)** | Low | Medium | Multiple metrics, human review, anomaly detection |

---

## 34. FAILURE SCENARIOS

| Scenario | Symptom | Recovery | RTO | RPO |
|---|---|---|---|---|
| Mentor service crash | Recommendations stop | Auto-restart, load from checkpoint | 30s | 5s |
| Database failure | Model updates fail | Failover to read replica, queue writes | 60s | 0 (sync commit) |
| Prediction model corrupted | Predictions unreliable | Rollback to previous model version | 5min | N/A |
| Event pipeline backlog | Model increasingly stale | Prioritize recent events, skip old | 10min | 1hr |
| Cache failure | Increased latency | Direct DB reads, rebuild cache | 5min | N/A |
| KG unavailable | Content recommendations blind | Use cached fact data | Continue degraded | N/A |

---

## 35. OPERATIONAL LIMITS

| Limit | Value | Consequence if Exceeded |
|---|---|---|
| Max learner model size | 1MB per learner | Archive old data, compress |
| Max concurrent sessions per shard | 5,000 | Increase shard count |
| Max prediction compute per hour | 10M fact-learner pairs | Reduce frequency, batch predictions |
| Max daily plan generation rate | 10,000/min | Queue generation, extend window |
| Max events per second per shard | 5,000 | Partition event stream |
| Max learner model versions retained | 30 | Prune old versions |
| Max recommendation alternatives | 5 | Reduced learner choice |

---

## 36. FUTURE IMPROVEMENTS

| Improvement | Description | Priority | Dependencies |
|---|---|---|---|
| **Multi-Modal Learning Style Detection** | Automatically detect optimal modality per topic | High | Behavioral model maturity |
| **Emotion-Aware Mentoring** | Detect emotions from text/speech/behavior for adaptation | High | Sentiment analysis model |
| **Peer Mentor Network** | Mentors share anonymized insights for better predictions | Medium | Privacy framework |
| **Cross-Learner Pattern Recognition** | Identify common learning patterns across similar learners | Medium | Scale, anonymization |
| **Meta-Learning Mentor** | Mentor that learns how to mentor better over time | Low | Long-term data collection |
| **Voice Mentor** | Voice-based mentor interactions | Low | Voice AI maturity |
| **AR/VR Mentor Integration** | Mentor guides in immersive environments | Low | Platform maturity |
| **Family Mentor** | Single mentor for family accounts with multiple learners | Low | Account model changes |
| **Lifelong Learning Path** | 10+ year curriculum planning with career integration | Low | Content coverage |
| **Mentor-to-Mentor Handover** | Seamless transfer between mentor instances | Medium | Distributed systems |

---

## 37. ACCEPTANCE CRITERIA

- [ ] Learner model captures identity, cognitive, behavioral, motivational, metacognitive, and social dimensions
- [ ] Per-fact mastery tracking with confidence, stability, and decay
- [ ] Forgetting predictor generates per-fact retention probability with > 80% accuracy
- [ ] Weakness predictor identifies gaps before they surface in assessments with > 85% accuracy
- [ ] Strategy selector chooses from 15+ teaching strategies based on learner state
- [ ] Content recommender scores and selects from all content types
- [ ] Daily plan generates automatically with review, new learning, and challenge items
- [ ] Weekly review summarizes progress, identifies trends, sets focus
- [ ] Monthly and quarterly reviews enable long-term goal tracking
- [ ] Mentor-AI Teacher integration: Mentor instructs Teacher on strategy
- [ ] Mentor-Content integration: Mentor requests content as needed
- [ ] Mentor-Gamification integration: Mentor adjusts reward difficulty
- [ ] Mentor-Curriculum integration: Mentor tracks and paces curriculum progress
- [ ] Mentor-Memory Engine integration: Mentor overrides review priorities
- [ ] Mentor-Social integration: Mentor recommends peer interactions
- [ ] Learner can override any recommendation
- [ ] Mentor tone adapts to learner's emotional state
- [ ] All predictions and recommendations can be explained to the learner
- [ ] Recommendation acceptance rate > 80%
- [ ] Session completion rate > 80%
- [ ] Learner satisfaction > 4.5/5
- [ ] Model update latency < 5s P95
- [ ] Recommendation latency < 100ms P95
- [ ] Graceful degradation under component failure

---

## 38. DEFINITION OF DONE

- [ ] All 20 internal components specified with inputs, outputs, and responsibilities
- [ ] Learner model covers all 8 dimensions with full data structures
- [ ] Forgetting and weakness prediction engines specified with algorithms
- [ ] Strategy selection engine covers 15+ strategies with selection logic
- [ ] Content recommendation engine defined with scoring algorithm
- [ ] All four mentorship cycles (daily, weekly, monthly, long-term) documented
- [ ] Integration with AI Teacher, Content, Gamification, Curriculum, Memory Engine, Social specified
- [ ] All communication protocols (events emitted/consumed) defined
- [ ] All data structures specified with JSON schemas
- [ ] Execution flows documented for real-time and batch operations
- [ ] State management defined for both mentor and learner sessions
- [ ] Caching, memory, and persistence strategies specified
- [ ] Validation, retry, and error recovery logic documented
- [ ] Security and privacy controls defined
- [ ] Monitoring metrics, dashboards, and alerts specified
- [ ] Scalability targets and performance budgets documented
- [ ] Testing strategy with scenarios defined
- [ ] Risk analysis with mitigations completed
- [ ] Failure scenarios with recovery procedures documented
- [ ] Operational limits specified
- [ ] Future improvements cataloged
- [ ] Acceptance criteria met
- [ ] No source code generated
- [ ] Connects to all referenced existing documents
- [ ] Ready for architectural review

---

*End of Document LXP-MENTOR-001*
