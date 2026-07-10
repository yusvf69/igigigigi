# ANALYTICS AND BI ARCHITECTURE

## Data-Driven Insights and Business Intelligence

---

| Document ID | IEP-ARCH-ANALYTICS-001 |
|---|---|
| Version | 1.0.0 |
| Status | Approved |
| Classification | Enterprise Internal |
| Effective Date | 2026-07-09 |
| Approving Authority | Office of the Chief Architect |
| Custodian | Data Analytics Team |

---

## TABLE OF CONTENTS

1. EXECUTIVE OVERVIEW
2. ANALYTICS PHILOSOPHY
3. DATA PIPELINE
4. EVENT TRACKING
5. DASHBOARDS
6. REPORTS
7. ML AND PREDICTIVE ANALYTICS
8. PRIVACY AND COMPLIANCE
9. APPENDICES

---

## 1. EXECUTIVE OVERVIEW

### 1.1 Purpose

The Analytics and BI Architecture transforms raw platform data into actionable insights for product improvement, content optimization, learner success, and business decision-making — while maintaining strict privacy standards.

### 1.2 Core Principle

**Measure what matters for learner success. Data serves learning, not surveillance.**

---

## 2. ANALYTICS PHILOSOPHY

| Principle | Description |
|---|---|
| **Privacy-First** | All analytics are anonymized by default; no individual tracking without consent |
| **Actionable Insights** | Every metric has a clear decision associated with it |
| **Product-Led** | Analytics drive product improvements, not the reverse |
| **Real-Time Where Needed** | Operational metrics in real-time; business metrics batch |
| **Democratized Access** | Dashboards available to all team members (with appropriate data access) |

---

## 3. DATA PIPELINE

```
┌─────────────────────────────────────────────────────────────────────────┐
│                      DATA SOURCES                                       │
│  Apps → API Gateway → Services → Databases → CDP                      │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                      DATA LAKE (S3 + Iceberg)                           │
│  Raw events → Partitioned → Compressed → Cataloged                     │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
                    ┌───────────────┴───────────────┐
                    ▼                               ▼
┌──────────────────────────────┐ ┌──────────────────────────────┐
│ Stream Processing (Kafka)    │ │ Batch Processing (Spark)     │
│ • Real-time dashboards       │ │ • Daily aggregations         │
│ • Alerts                     │ │ • ML training data           │
│ • Personalization            │ │ • Business reports           │
└──────────────────────────────┘ └──────────────────────────────┘
                    │                               │
                    └───────────────┬───────────────┘
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                      DATA WAREHOUSE                                     │
│  Redshift / Snowflake → Fact tables → Dimension tables → Views        │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                      EXPOSURE LAYER                                     │
│  Metabase / Preset → Dashboards → Scheduled Reports → Data API        │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## 4. EVENT TRACKING

### 4.1 Event Categories

| Category | Example Events | Volume/Day |
|---|---|---|
| **User** | signup, login, profile_update, settings_change | 500K |
| **Content** | story_view, quiz_start, flashcard_review, podcast_listen | 5M |
| **Learning** | answer_submit, card_rate, lesson_complete, quiz_finish | 10M |
| **Social** | post_create, comment, group_join, friend_add | 200K |
| **Progress** | level_up, badge_earn, streak_update, mastery_change | 100K |
| **System** | error, api_call, performance_metric | 1M |

### 4.2 Event Schema

```json
{
  "event": "quiz_question_answered",
  "user_id": "anon_xyz789",
  "session_id": "ses_001234",
  "timestamp": "2026-07-09T14:30:00Z",
  "properties": {
    "question_id": "q_00452",
    "topic": "Tawheed",
    "correct": true,
    "attempt_number": 1,
    "response_time_ms": 4500,
    "difficulty": "medium"
  },
  "context": {
    "platform": "web",
    "version": "2.1.0",
    "locale": "ar",
    "timezone": "Asia/Riyadh"
  }
}
```

---

## 5. DASHBOARDS

| Dashboard | Audience | Refresh | Key Metrics |
|---|---|---|---|
| **Executive Overview** | Leadership | Daily | DAU, MAU, retention, revenue, content growth |
| **Learning Analytics** | Product | Real-time | Quiz accuracy, completion rates, time spent |
| **Content Performance** | Content team | Daily | Popular content, engagement, completion |
| **AI Quality** | AI team | Real-time | Hallucination rate, accuracy, user satisfaction |
| **Social Health** | Community | Daily | Active groups, posts, sentiment |
| **Engineering** | Engineering | Real-time | Latency, errors, uptime, deployments |
| **Business** | Management | Weekly | Subscribers, LTV, churn, CAC |

---

## 6. REPORTS

| Report | Frequency | Content |
|---|---|---|
| **Weekly Product Report** | Weekly | Key metrics, trends, anomalies, recommendations |
| **Monthly Business Review** | Monthly | Revenue, growth, content milestones, roadmap progress |
| **Quarterly Impact Report** | Quarterly | Learner outcomes, content expansion, platform improvements |
| **Content Quality Audit** | Monthly | Accuracy rates, scholar review status, error rates |
| **A/B Test Results** | Per experiment | Statistical significance, winner determination |

---

## 7. ML AND PREDICTIVE ANALYTICS

| Model | Purpose | Features | Update Frequency |
|---|---|---|---|
| **Content Recommender** | Suggest next content | Past activity, mastery, interests | Real-time |
| **Churn Predictor** | Identify at-risk users | Engagement drop, streak loss, inactivity | Daily |
| **Difficulty Calibrator** | Adjust content difficulty | Accuracy, response time, mastery | Per session |
| **Engagement Forecaster** | Predict DAU/MAU | Historical trends, seasonal factors | Weekly |
| **Content Quality Predictor** | Estimate content quality pre-publish | Metadata, creator history, topic | On creation |

---

## 8. PRIVACY AND COMPLIANCE

| Requirement | Implementation |
|---|---|
| **Anonymization** | User IDs hashed after 90 days for long-term analysis |
| **Aggregation** | All reports use aggregated data (min 10 users per bucket) |
| **Opt-Out** | Users can opt out of analytics tracking |
| **Data Retention** | Raw events: 90 days; Aggregated: indefinite |
| **GDPR Compliance** | Right to access, export, delete all analytics data |
| **No Third-Party Sharing** | Analytics data never shared with third parties |

---

## 9. APPENDICES

### 9.1 Related Documents

| Document | Title | Relationship |
|---|---|---|
| IEP-ARCH-PROG-001 | Progress Tracking | Learner analytics source |
| IEP-ARCH-DB-001 | Database Architecture | Data storage |
| IEP-ARCH-SEC-001 | Security Architecture | Data privacy |

### 9.2 Revision History

| Version | Date | Author | Changes |
|---|---|---|---|
| 1.0.0 | 2026-07-09 | Office of the Chief Architect | Initial approved release |
