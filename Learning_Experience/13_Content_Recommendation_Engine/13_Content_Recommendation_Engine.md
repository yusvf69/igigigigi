# CONTENT RECOMMENDATION ENGINE

## Multi-Stage Personalization Architecture for Islamic Learning Platform

---

| Metadata | Value |
|---|---|
| Document ID | LXP-RECOMMEND-001 |
| Version | 1.0.0 |
| Status | Draft |
| Classification | Enterprise Internal |
| Effective Date | 2026-07-10 |
| Approving Authority | Office of the Chief Architect |
| Custodian | Learning Experience Architecture Team |
| Supersedes | None |
| Core Prerequisites | LXP-MENTOR-001 (Personal AI Mentor), LXP-BIBLE-001 (Learning Experience Bible), LXP-MEMORY-001 (Memory Engine), LXP-ADAPT-001 (Adaptive Learning Engine), LXP-MULTIMODAL-001 (Multi-Modal Learning) |
| Document Dependencies | 00_Personal_AI_Mentor, 01_Learning_Experience_Bible, 02_Memory_Engine, 08_Multi_Modal_Learning, 09_Adaptive_Learning_Engine |

---

## TABLE OF CONTENTS

1. RECOMMENDATION ARCHITECTURE
2. CANDIDATE GENERATION
3. FEATURE ENGINEERING
4. SCORING MODELS
5. RANKING
6. FILTERING
7. REAL-TIME PERSONALIZATION
8. EXPLORATION VS EXPLOITATION
9. COLD START
10. A/B TESTING
11. FEEDBACK LOOP
12. DIVERSITY AND SERENDIPITY
13. SCALABILITY
14. EVALUATION
15. CROSS-DOCUMENT INTEGRATION

---

## 1. RECOMMENDATION ARCHITECTURE

### 1.1 Architectural Philosophy

The Content Recommendation Engine is the decision-making core that determines what content the learner sees, in what order, at what time, through which modality, and at what difficulty. It operates as a multi-stage pipeline processing millions of potential content items down to a single optimal next action. Every decision is informed by the learner's complete cognitive, behavioral, motivational, and contextual state.

The architecture follows a funnel principle: cast a wide net during candidate generation, apply increasingly sophisticated scoring and filtering, and present only the highest-value recommendations.

### 1.2 Multi-Stage Pipeline

```
+-----------------------------------------------------------------------+
|                   CONTENT RECOMMENDATION PIPELINE                       |
|                                                                         |
|  +--------------------------------------------------------------------+|
|  |  STAGE 1: CANDIDATE GENERATION                                     ||
|  |  Content Pool: 500K+ items | Curriculum Candidates: 5K             ||
|  |  | Weakness Candidates: 3K | Interest Candidates: 5K               ||
|  |  | Exploration Candidates: 1K | Social Candidates: 2K              ||
|  |  Total Candidates: ~16K items                                      ||
|  +--------------------------------------------------------------------+|
|                                  |                                      |
|                                  v                                      |
|  +--------------------------------------------------------------------+|
|  |  STAGE 2: FEATURE ENGINEERING                                      ||
|  |  | Learner Features (knowledge state, preferences, history,         ||
|  |  |    behavior, motivation)                                         ||
|  |  | Content Features (type, topic, difficulty, modality, length,     ||
|  |  |    quality, language)                                            ||
|  |  | Context Features (time, device, session state, location)         ||
|  |  | Cross Features (learner-content interactions, learner-context)   ||
|  +--------------------------------------------------------------------+|
|                                  |                                      |
|                                  v                                      |
|  +--------------------------------------------------------------------+|
|  |  STAGE 3: SCORING                                                   ||
|  |  | Collaborative Filtering Score (user-based + item-based + MF)    ||
|  |  | Content-Based Score (similarity to known preferences)           ||
|  |  | Deep Learning Score (two-tower DNN, DCN, transformers)          ||
|  |  | Contextual Bandit Score (exploration bonus)                     ||
|  |  | Hybrid Score (weighted ensemble)                                ||
|  |  | Each candidate receives {score, confidence, source}             ||
|  +--------------------------------------------------------------------+|
|                                  |                                      |
|                                  v                                      |
|  +--------------------------------------------------------------------+|
|  |  STAGE 4: RANKING                                                  ||
|  |  | Multi-Objective Optimization                                    ||
|  |  | Ranking Fusion                                                  ||
|  |  | Diversity Enforcement (MMR, topic spread, modality spread)       ||
|  |  | Freshness Boosting (new content, recent trends)                 ||
|  |  | Serendipity Injection                                           ||
|  |  | Fairness Constraints                                            ||
|  +--------------------------------------------------------------------+|
|                                  |                                      |
|                                  v                                      |
|  +--------------------------------------------------------------------+|
|  |  STAGE 5: FILTERING                                                ||
|  |  | Already-Consumed Filtering                                      ||
|  |  | Difficulty-Appropriate Filtering                                ||
|  |  | Time-Appropriate Filtering                                      ||
|  |  | Content-Type Balance                                            ||
|  |  | Parental Controls                                               ||
|  |  | Madhhab Filtering                                               ||
|  +--------------------------------------------------------------------+|
|                                  |                                      |
|                                  v                                      |
|  +--------------------------------------------------------------------+|
|  |  STAGE 6: PRESENTATION                                             ||
|  |  | Next Action (single best item for immediate consumption)         ||
|  |  | Daily Plan (curated list for the day)                           ||
|  |  | Weekly Focus (thematic bundle for the week)                     ||
|  |  | Home Feed (personalized content stream)                         ||
|  |  | Notification Content (push-worthy recommendations)              ||
|  +--------------------------------------------------------------------+|
+-----------------------------------------------------------------------+
```

### 1.3 Pipeline Stages Detail

| Stage | Input Size | Output Size | Latency Budget | Compute Model |
|---|---|---|---|---|
| Candidate Generation | 500K+ | ~16K candidates | 50ms | Precomputed + Real-time |
| Feature Engineering | ~16K candidates | ~16K enriched | 100ms | Real-time feature store |
| Scoring | ~16K candidates | ~16K scored | 200ms | Model inference (GPU/CPU) |
| Ranking | ~16K scored | ~500 top candidates | 100ms | Heuristic + ML reranker |
| Filtering | ~500 candidates | ~50 finalists | 50ms | Rule-based + ML checks |
| Presentation | ~50 finalists | 1-20 recommendations | 20ms | Template + UX logic |
| **Total** | **500K+** | **1-20** | **< 520ms** | **Hybrid** |

### 1.4 Serving Modes

| Mode | Pipeline Execution | Use Case | SLA |
|---|---|---|---|
| **Real-time** | Full pipeline on demand | Next action, session start | < 500ms p99 |
| **Near-real-time** | Precompute candidates + real-time scoring | Home feed refresh | < 2s |
| **Batch** | Full pipeline offline | Daily plan generation, weekly focus | Nightly cron |
| **Streaming** | Incremental update + lightweight scoring | Notification triggers, adaptation | < 100ms |
| **Precompute + Cache** | Batch scores + cached embeddings + light rerank | High-traffic periods | < 50ms |

### 1.5 Integration with Personal AI Mentor (00_Personal_AI_Mentor)

The Content Recommendation Engine is the primary decision-making component invoked by the Personal AI Mentor. The Mentor maintains the learner model (knowledge state, preferences, goals, history) and uses the Recommendation Engine to determine the optimal next learning action. The integration follows this protocol:

1. **Mentor requests recommendation**: The Mentor calls the Recommendation Engine with the full learner state payload including current goals, known weaknesses, forgetting predictions, and session context.
2. **Engine executes pipeline**: The Recommendation Engine runs its full multi-stage pipeline using the Mentor-provided learner state as the primary input.
3. **Engine returns ranked candidates**: The top-N scored, filtered, and ranked content items are returned with explanations for each recommendation.
4. **Mentor validates and presents**: The Mentor validates the recommendations against the learner's current emotional state and long-term plan, applies any Mentor-specific overrides, and presents the final selection to the learner.
5. **Feedback flows back**: Learner interaction results are captured and fed into both the Mentor's learner model updates and the Recommendation Engine's feedback loop.

The Mentor's Content Recommendation Engine section (Section 9 of 00_Personal_AI_Mentor) defines the high-level recommendation types. This document provides the full architectural and algorithmic implementation.

### 1.6 Integration with Learning Experience Bible (01_Learning_Experience_Bible)

The Learning Experience Bible defines the user-facing experiences that consume recommendations. Each experience type described in the Bible maps to a specific pipeline configuration:

| Bible Experience Section | Recommendation Type | Pipeline Configuration |
|---|---|---|
| First Lesson (Section 3) | Onboarding Recommendations | Cold-start pipeline, high-engagement bias |
| Daily Experience (Section 9) | Daily Plan | Full pipeline, balanced objectives |
| Weekly Experience (Section 10) | Weekly Focus | Batch pipeline, thematic clustering |
| Long Break Recovery (Section 12) | Re-engagement Recommendations | High-review-weight pipeline |
| Review Timing (Section 22) | Review Schedule | Memory-weighted pipeline |
| Session Design (Section 23) | Session Flow Recommendations | Stream pipeline, real-time adaptation |

### 1.7 Integration with Adaptive Learning Engine (09_Adaptive_Learning_Engine)

The Adaptive Learning Engine determines content difficulty, pacing, and strategy adjustments. The Recommendation Engine uses these adaptive parameters as hard constraints:

1. **Difficulty Constraint**: The adaptive engine provides the learner's current optimal difficulty level. Content too far outside this range is deprioritized.
2. **Strategy Constraint**: The adaptive engine selects a teaching strategy. The recommendation pipeline filters to content modes compatible with the selected strategy.
3. **Pacing Constraint**: Content duration and complexity are weighted accordingly in the scoring stage.
4. **Intervention Mode**: When the adaptive engine detects struggle, boredom, or frustration, the recommendation pipeline shifts to intervention-specific candidate generation.

### 1.8 Integration with Memory Engine (02_Memory_Engine)

The Memory Engine provides forgetting curve data and review scheduling inputs:

1. **Forgetting Risk Scores**: Each fact known by the learner carries a forgetting risk score from the Memory Engine. Content addressing facts with high forgetting risk receives significant score boosts.
2. **Optimal Review Schedule**: The recommendation pipeline respects Memory Engine schedules when generating review candidates.
3. **Ease Factors**: Content related to low-ease-factor facts is prioritized for multi-modal reinforcement.
4. **Interleaving Recommendations**: For advanced learners, the Memory Engine suggests interleaved review patterns.

### 1.9 Integration with Multi-Modal Learning (08_Multi_Modal_Learning)

The Multi-Modal Learning system defines available content modalities and their characteristics:

1. **Modality Affinity Scoring**: Each learner has modality affinity scores based on engagement history.
2. **Modality Rotation**: The pipeline enforces modality diversity across and within sessions.
3. **Multi-Modal Reinforcement**: When content is flagged as struggling, the pipeline presents the same knowledge through different modalities.
4. **Modality Availability**: Content items are filtered based on current device capabilities.

---

## 2. CANDIDATE GENERATION

### 2.1 Content Pool Definition

The content pool is the universe of all available content items. It is organized by multiple taxonomies:

| Content Pool Dimension | Description | Size Estimate | Update Frequency |
|---|---|---|---|
| **Full Content Pool** | All content items across all modes | 500K - 5M items | Continuous |
| **Active Pool** | Items passing quality, language, availability checks | 400K - 4M items | Daily |
| **Learner-Accessible Pool** | Items matching learner's language, age, madhhab, settings | 100K - 1M items | Per learner |
| **Curriculum-Aligned Pool** | Items mapped to enrolled curriculum paths | 10K - 100K items | Per curriculum |
| **Urgent Pool** | Items addressing weaknesses and high-forgetting-risk facts | 100 - 5K items | Real-time |

Content items are indexed by: Content ID, Knowledge Graph fact connections, topic taxonomy (Domain / Subject / Topic / Subtopic), modality (Story, Quiz, Flashcard, Podcast, AI Teacher, Interactive, Video, Text), difficulty level (1-10), quality score (0.0-1.0), freshness timestamp, language, madhhab alignment, age rating, estimated duration, content embedding vector, and prerequisite content IDs.

### 2.2 Curriculum-Aligned Candidates

Curriculum-aligned candidates directly support the learner's enrolled curriculum path.

**Generation Algorithm:**
1. Load learner's current curriculum path(s) and progress within each.
2. Identify the next uncompleted modules, lessons, and topics.
3. For each curriculum node, retrieve all associated content items across all modalities.
4. Score each item by curriculum alignment strength: direct match 1.0, supplementary 0.7, background 0.4.
5. Apply prerequisites check: items with unmet prerequisites are demoted.
6. Select top-K items per curriculum node, balancing modality diversity.

**Curriculum Candidate Sources:**
- Required readings and core lessons
- Supplementary practice materials
- Assessment preparation content
- Extension and enrichment content
- Cross-curricular connecting content

### 2.3 Weakness-Targeted Candidates

Weakness-targeted candidates address identified knowledge gaps, predicted weaknesses, and high-forgetting-risk facts.

**Generation Algorithm:**
1. Query the Weakness Prediction Engine for the learner's current weakness report.
2. Query the Memory Engine for high-forgetting-risk facts and overdue reviews.
3. For each weak fact or concept, find content items that specifically address it:
   - Primary coverage: content directly teaching this fact
   - Secondary coverage: content that includes this fact as part of a broader topic
   - Assessment coverage: content that tests this fact
4. Score candidates by weakness severity: critical 1.0, moderate 0.7, emerging 0.4, high forgetting risk 0.9 with urgency multiplier.
5. Apply modality recommendation per root cause: prerequisite gap to foundational content, memory decay to review content, confusion to comparative content, misconception to corrective content.

### 2.4 Interest-Based Candidates

Interest-based candidates align with the learner's declared and inferred interests.

**Generation Algorithm:**
1. Load declared interests from the Learner Model.
2. Load inferred interests from engagement patterns.
3. Compute interest scores per topic: declared weight from survey, engagement-based weight from time spent, emerging interest from accelerating trend, declining interest from decelerating trend.
4. For each high-interest topic, retrieve top content items.
5. Score items by interest match: exact topic 1.0, parent topic 0.6, related topic 0.3.

### 2.5 Exploration Candidates

Exploration candidates introduce the learner to new topics, modalities, or content not previously encountered.

**Generation Algorithm:**
1. Identify knowledge blind spots: topics never engaged with that are within accessible pool.
2. Compute exploration value per candidate: KG distance from known topics (moderate distance optimal), learner readiness, modality novelty, difficulty appropriateness.
3. Score candidates by exploration potential: novelty score inversely proportional to consumed item similarity, connection score related to known topics, readiness score as probability of successful engagement.
4. Apply serendipity bonus for unexpected but potentially delightful content.
5. Diversify across topics and modalities.

### 2.6 Social Candidates

Social candidates leverage the learning community to surface content.

**Generation Algorithm:**
1. Identify learner's social graph: study groups, peers, teachers, mentors.
2. Collect social signals: content completed by peers, teacher recommendations, high-engagement content in demographic segment, content with high mastery gains for similar learners.
3. Score by social relevance: peer similarity weight, group completion rate, teacher recommendation weight, mentor recommendation weight, demographic popularity weight.
4. Apply collaborative filtering boost for items consumed by similar learners with positive outcomes.
5. Filter for privacy: only surface content peers have opted to share.

### 2.7 Candidate Deduplication and Merging

Before proceeding to feature engineering, the candidate pool is deduplicated and merged.

**Deduplication Rules:**
1. Exact content ID deduplication: keep highest-scoring source.
2. Content equivalence deduplication: same KG facts by different items, keep both but mark related.
3. Series deduplication: keep contiguous series entries.

**Candidate Budget Allocation:**
| Candidate Source | Min Budget | Max Budget |
|---|---|---|
| Curriculum | 2,000 | 5,000 |
| Weakness | 1,000 | 3,000 |
| Interest | 2,000 | 5,000 |
| Exploration | 500 | 1,000 |
| Social | 500 | 2,000 |
| Total | 6,000 | 16,000 |

---

## 3. FEATURE ENGINEERING

### 3.1 Feature Categories

Features are organized into learner-side, content-side, context, and cross-features. Every feature is versioned in a feature registry.

### 3.2 Learner Features

#### 3.2.1 Knowledge State Features

| Feature | Description | Type | Update Frequency | Source |
|---|---|---|---|---|
| learner_overall_mastery | Aggregate mastery across all domains | Float [0,1] | Every event | Learner Model |
| learner_domain_mastery | Per-domain mastery vector | Float [0,1]^D | Every event | Learner Model |
| learner_topic_mastery | Per-topic mastery vector | Float [0,1]^T | Every event | Learner Model |
| learner_fact_mastery | Per-fact mastery vector | Float [0,1]^F | Every event | Learner Model |
| learner_weakest_domain | Domain with lowest mastery | Categorical | Every event | Learner Model |
| learner_weakest_topic | Topic with lowest mastery | Categorical | Every event | Learner Model |
| learner_weakness_count | Number of critical weaknesses | Integer | Every event | Weakness Predictor |
| learner_forgetting_risk_avg | Average forgetting risk across all facts | Float [0,1] | Every event | Memory Engine |
| learner_forgetting_risk_max | Maximum forgetting risk for any fact | Float [0,1] | Every event | Memory Engine |
| learner_overdue_review_count | Number of past-due reviews | Integer | Every event | Memory Engine |
| learner_review_queue_depth | Total reviews due this period | Integer | Daily | Memory Engine |
| learner_knowledge_completeness | Curriculum knowledge percentage | Float [0,1] | Daily | Curriculum Paths |
| learner_prerequisite_gaps | Number of unmet prerequisites | Integer | Daily | Knowledge Graph |

#### 3.2.2 Preference Features

| Feature | Description | Type | Update Frequency |
|---|---|---|---|
| learner_preferred_modality | Top-rated content modality | Categorical | Weekly |
| learner_modality_affinity | Affinity score per modality | Float [0,1]^M | Weekly |
| learner_preferred_difficulty | Self-reported preferred difficulty | Integer [1,10] | Weekly |
| learner_preferred_session_length | Target session duration | Integer | Weekly |
| learner_preferred_learning_time | Time of day for learning | Categorical | Per session |
| learner_content_depth_preference | Quick / Balanced / Deep | Categorical | Weekly |
| learner_new_vs_review_ratio | Preference for new vs review content | Float [0,1] | Weekly |
| learner_topic_interest_declared | Explicit interest vector | Float [0,1]^T | Rarely |
| learner_topic_interest_inferred | Behavioral interest vector | Float [0,1]^T | Daily |
| learner_autonomy_level | Guided / Semi-autonomous / Autonomous | Categorical | Monthly |
| learner_madhhab | Jurisprudential school | Categorical | On registration |

#### 3.2.3 History Features

| Feature | Description | Type | Update Frequency |
|---|---|---|---|
| learner_recent_content_ids | Last 100 content items consumed | Array[Categorical] | Every event |
| learner_recent_modalities | Last 20 modality types used | Array[Categorical] | Every event |
| learner_recent_topics | Last 50 topics engaged with | Array[Categorical] | Every event |
| learner_recent_difficulties | Last 50 difficulty levels experienced | Array[Integer] | Every event |
| learner_consumed_content_ids | Set of all consumed content IDs | Set[Categorical] | Every event |
| learner_skipped_content_ids | Set of all skipped content IDs | Set[Categorical] | Every event |
| learner_repeated_content_ids | Set of repeatedly consumed content IDs | Set[Categorical] | Every event |
| learner_total_sessions | Total learning sessions | Integer | Daily |
| learner_total_learning_time | Cumulative learning time in minutes | Integer | Daily |
| learner_session_count_7d | Sessions in last 7 days | Integer | Daily |
| learner_session_count_30d | Sessions in last 30 days | Integer | Daily |
| learner_learning_time_7d | Learning minutes in last 7 days | Integer | Daily |
| learner_learning_time_30d | Learning minutes in last 30 days | Integer | Daily |

#### 3.2.4 Behavior Features

| Feature | Description | Type | Update Frequency | Source |
|---|---|---|---|---|
| learner_session_completion_rate | Percentage of sessions completed | Float [0,1] | Daily | Engagement Analysis |
| learner_content_completion_rate | Percentage of started content completed | Float [0,1] | Daily | Engagement Analysis |
| learner_quiz_accuracy_7d | Quiz accuracy over last 7 days | Float [0,1] | Daily | Quiz System |
| learner_quiz_accuracy_30d | Quiz accuracy over last 30 days | Float [0,1] | Daily | Quiz System |
| learner_avg_session_duration | Mean session length in minutes | Float | Daily | Engagement Analysis |
| learner_avg_content_duration | Mean content consumption time | Float | Daily | Engagement Analysis |
| learner_attention_span_est | Estimated attention span | Integer | Daily | Adaptive Engine |
| learner_fatigue_probability | Predicted current fatigue level | Float [0,1] | Real-time | Adaptive Engine |
| learner_boredom_probability | Predicted current boredom level | Float [0,1] | Real-time | Adaptive Engine |
| learner_confusion_probability | Predicted current confusion level | Float [0,1] | Real-time | Adaptive Engine |
| learner_streak_days | Current streak length | Integer | Daily | Gamification |
| learner_longest_streak | Longest streak ever achieved | Integer | Daily | Gamification |
| learner_click_through_rate | Historical CTR on recommendations | Float [0,1] | Weekly | Feedback Loop |
| learner_skip_rate | Historical skip rate on recommendations | Float [0,1] | Weekly | Feedback Loop |
| learner_session_time_of_day | Current session hour | Integer [0,23] | Real-time | Session Context |
| learner_session_day_of_week | Current day of week | Integer [0,6] | Real-time | Session Context |
| learner_session_number | Sequential session number | Integer | Real-time | Session Context |

#### 3.2.5 Motivation Features

| Feature | Description | Type | Update Frequency |
|---|---|---|---|
| learner_motivation_level | Current motivation (low/medium/high/very high) | Ordinal | Daily |
| learner_motivation_trend_7d | 7-day motivation trend | Categorical | Daily |
| learner_intrinsic_motivation_score | Intrinsic drive measure | Float [0,1] | Weekly |
| learner_extrinsic_motivation_score | Extrinsic drive measure | Float [0,1] | Weekly |
| learner_goal_progress | Progress toward primary goal | Float [0,1] | Daily |
| learner_goal_confidence | Self-efficacy belief | Float [0,1] | Weekly |
| learner_goal_clarity | Understanding of goal path | Float [0,1] | Weekly |
| learner_frustration_level | Current frustration estimate | Float [0,1] | Real-time |
| learner_engagement_score | Composite engagement metric | Float [0,1] | Real-time |
| learner_interest_diversity | Breadth of topics engaged with | Float [0,1] | Weekly |

### 3.3 Content Features

#### 3.3.1 Static Content Features

| Feature | Description | Type | Source |
|---|---|---|---|
| content_id | Unique identifier | Categorical | CMS |
| content_type | Story, Quiz, Flashcard, Podcast, etc. | Categorical | CMS |
| content_topics | Array of topic tags | Array[Categorical] | CMS |
| content_domain | Primary domain | Categorical | CMS |
| content_difficulty | Difficulty level 1-10 | Integer | Expert Rating |
| content_length_minutes | Estimated duration | Float | CMS |
| content_word_count | Total word count | Integer | CMS |
| content_language | Primary language | Categorical | CMS |
| content_madhhab | Madhhab alignment or Universal | Categorical | CMS |
| content_age_rating | Minimum age recommendation | Categorical | CMS |
| content_prerequisites | Required prior content IDs | Array[Categorical] | KG |
| content_kg_facts | KG fact IDs covered | Array[Categorical] | KG |
| content_learning_objectives | Learning outcomes | Array[String] | CMS |
| content_series_id | Series membership | Categorical | CMS |
| content_series_position | Position within series | Integer | CMS |
| content_creation_date | When content was created | Datetime | CMS |
| content_update_date | Last content update | Datetime | CMS |
| content_embedding | Dense embedding vector | Float[256] | Embedding Model |

#### 3.3.2 Dynamic Content Features

| Feature | Description | Type | Update Frequency |
|---|---|---|---|
| content_quality_score | Expert + user quality rating | Float [0,1] | Weekly |
| content_popularity_7d | Views in last 7 days | Integer | Daily |
| content_popularity_30d | Views in last 30 days | Integer | Daily |
| content_completion_rate_global | Global completion rate | Float [0,1] | Daily |
| content_avg_rating | Average user rating | Float [0,5] | Daily |
| content_effectiveness_score | Avg mastery gain per consumption | Float | Weekly |
| content_engagement_score | Avg engagement during consumption | Float [0,1] | Weekly |
| content_avg_time_spent | Avg time spent consuming | Float (minutes) | Daily |
| content_skip_rate | Percentage of users who skip | Float [0,1] | Daily |
| content_repeat_consumption_rate | Users who consume multiple times | Float [0,1] | Weekly |
| content_recommendation_ctr | Avg CTR when recommended | Float [0,1] | Weekly |
| content_mastery_impact | Avg mastery delta after consumption | Float | Weekly |
| content_recency_weight | Time-decayed freshness weight | Float [0,1] | Real-time |
| content_trending_score | Velocity of recent engagement | Float | Real-time |

### 3.4 Context Features

#### 3.4.1 Temporal Context

| Feature | Description | Type |
|---|---|---|
| current_hour | Hour of day 0-23 | Integer |
| current_day_of_week | Day 0-6 (Monday=0) | Integer |
| current_month | Month 1-12 | Integer |
| is_weekend | Whether today is weekend | Boolean |
| is_holiday | Whether today is an Islamic holiday | Boolean |
| is_ramadan | Whether currently in Ramadan | Boolean |
| season | Current season | Categorical |
| time_since_last_session | Days since last session | Integer |
| remaining_session_budget | Remaining time in session budget | Integer |

#### 3.4.2 Device Context

| Feature | Description | Type |
|---|---|---|
| device_type | Phone / Tablet / Desktop / Other | Categorical |
| device_orientation | Portrait / Landscape | Categorical |
| os_type | iOS / Android / Web | Categorical |
| network_type | WiFi / Cellular / Offline | Categorical |
| audio_available | Whether audio output is available | Boolean |
| battery_level | Current battery percentage | Float [0,100] |
| charging_status | Whether device is charging | Boolean |
| notification_permission | Whether notifications are enabled | Boolean |

#### 3.4.3 Session State

| Feature | Description | Type |
|---|---|---|
| session_id | Unique session identifier | Categorical |
| session_goal | Learner's stated session goal | Categorical |
| session_mode_so_far | Modalities used this session | Array[Categorical] |
| session_topics_covered | Topics covered this session | Array[Categorical] |
| session_items_consumed | Content items consumed this session | Integer |
| session_frustration_count | Number of frustration signals | Integer |
| session_help_request_count | Number of help requests | Integer |
| session_engagement_current | Real-time engagement estimate | Float [0,1] |
| session_last_action_type | Type of last learner action | Categorical |
| session_adaptive_difficulty | Current adaptive difficulty setting | Integer [1,10] |
| session_teaching_strategy | Current active teaching strategy | Categorical |

### 3.5 Cross Features

Cross features combine learner, content, and context features:

| Feature | Description | Formula |
|---|---|---|
| content_difficulty_gap | Difference between content and adaptive difficulty | `abs(content_difficulty - learner_adaptive_difficulty)` |
| topic_mastery_by_content | Learner's mastery of this content's topic | `learner_topic_mastery[content_topic]` |
| fact_overlap_count | KG facts shared between content and weaknesses | `count(intersect(content_kg_facts, learner_weak_facts))` |
| fact_novelty_count | Content KG facts learner hasn't encountered | `count(difference(content_kg_facts, learner_known_facts))` |
| modality_recent_usage | Times modality used in last N sessions | `count(learner_recent_modalities == content_type)` |
| modality_affinity_score | Learner's affinity for this modality | `learner_modality_affinity[content_type]` |
| curriculum_alignment | How directly content supports curriculum | `curriculum_alignment_map[content_id]` |
| weakness_relevance | Sum of weakness severities addressed | `sum(weakness_map[fact] for fact in content_kg_facts)` |
| prerequisite_satisfaction | Whether all prerequisites satisfied | `all(prereq in learner_consumed)` |
| attention_fit | Content length fits remaining attention | `content_length <= remaining_attention` |
| session_budget_fit | Content fits remaining session time | `content_length <= remaining_session_minutes` |
| freshness_weight | Time-decayed weight for new content | `decay(content_age, half_life=30_days)` |
| social_proof_weight | Peer/teacher recommendations count | `count(social_recommendations[content_id])` |
| madhhab_match | Content madhhab matches learner's | `content_madhhab in [learner_madhhab, universal]` |
| age_appropriateness | Content meets age rating constraints | `content_age_rating <= learner_age_group` |
| fatigue_penalty | Penalty for heavy content when fatigued | `if fatigue > 0.7: content_difficulty * -0.1` |
| boredom_correction | Boost for novel content when bored | `if boredom > 0.7: novelty_score * 0.2` |

### 3.6 Feature Store Architecture

Features are served through a distributed feature store with three tiers:

**Online Feature Store (Redis + Memory):**
- Real-time learner features updated every event
- Session context features updated every action
- Device context features updated per request
- Latency: < 5ms per batch lookup

**Near-Line Feature Store (DynamoDB/Cassandra):**
- Per-learner aggregates (daily updated)
- Per-content aggregates (hourly updated)
- Medium-term behavior patterns (7-30 day windows)
- Latency: < 20ms per lookup

**Offline Feature Store (S3 + Parquet + Spark):**
- Full historical feature snapshots
- Training data for model updates
- Long-term patterns and trends
- Latency: Minutes to hours

---

## 4. SCORING MODELS

### 4.1 Scoring Architecture Overview

The scoring stage applies multiple models to each candidate, producing a multi-dimensional score vector combined in the ranking stage.

```
+-----------------------------------------------------------------------+
|                        SCORING MODEL ENSEMBLE                          |
|                                                                        |
|  +----------------+  +----------------+  +----------------+            |
|  | Collaborative  |  | Content-Based  |  | Deep Learning  |            |
|  | Filtering      |  | Filtering      |  | Models         |            |
|  |                |  |                |  |                |            |
|  | - User-Based   |  | - Similarity   |  | - Two-Tower DNN|            |
|  | - Item-Based   |  | - Metadata     |  | - DCN          |            |
|  | - Matrix Fac.  |  | - KG-Based     |  | - Transformer  |            |
|  +----------------+  +----------------+  +----------------+            |
|          |                   |                   |                     |
|          +-------------------+-------------------+                     |
|                              v                                        |
|  +------------------------------------------------------------------+ |
|  |                    HYBRID SCORE FUSION                             | |
|  |  ensemble_score = S(weight_i x score_i) / S(weight_i)             | |
|  +------------------------------------------------------------------+ |
|                              v                                        |
|  +------------------------------------------------------------------+ |
|  |                    CONTEXTUAL BANDIT LAYER                         | |
|  |  final_score = ensemble_score + lambda x exploration_bonus        | |
|  +------------------------------------------------------------------+ |
+-----------------------------------------------------------------------+
```

### 4.2 Collaborative Filtering

#### 4.2.1 User-Based Collaborative Filtering

Finds learners similar to the target and recommends content those similar learners engaged with positively.

**Similarity Computation:**
```
learner_similarity(a, b) = cosine_similarity(interaction_vector(a), interaction_vector(b))
interaction_vector: sparse vector of (content_id -> weighted_interaction)
weighted_interaction: completion(1.0) + rating(0.5) + engagement(0.3)
```

**Configuration:**
- Similarity metric: cosine
- Neighborhood size: 100
- Min similarity: 0.1
- Interaction weighting: completed 1.0, high rating 0.8, medium rating 0.5, low rating 0.1, skipped -0.3
- Update frequency: daily batch

#### 4.2.2 Item-Based Collaborative Filtering

Computes similarity between content items based on co-consumption patterns.

**Advantages Over User-Based CF:**
- More stable: item similarities change slowly
- More interpretable: "Because you liked X, you might like Y"
- Better for large-scale: item-item matrix computed offline

**Configuration:**
- Similarity metric: cosine
- Max neighbors: 50
- Min similarity: 0.05
- Update frequency: every 6 hours

#### 4.2.3 Matrix Factorization

Learns latent representations of learners and content in a shared embedding space.

**Model:** R = U x V^T where R is the learner-content interaction matrix, U is learner latent factors (N x k), V is content latent factors (M x k), k = 64.

**Training Objective (Weighted ALS):**
```
min S w_ij x (R_ij - U_i . V_j)^2 + lambda(||U||^2 + ||V||^2)
```
Where w_ij = confidence weight based on interaction strength.

**Configuration:**
- Latent dimensions: 64
- Regularization: 0.01
- Confidence weighting: linear (1.0 to 10.0)
- Optimizer: ALS weighted
- Iterations: 20
- Update frequency: daily

### 4.3 Content-Based Filtering

Recommends items similar to what the learner has engaged with previously.

#### 4.3.1 Similarity-Based Filtering

Content embeddings are computed from: text embeddings (multilingual sentence transformer, weight 0.5), Knowledge Graph embeddings from KG fact connections (weight 0.3), metadata embeddings (topic, domain, difficulty, modality, weight 0.2). Combined as 1024-dimensional vectors.

**Configuration:**
- Embedding model: multilingual_e5_large
- Embedding dimension: 1024
- Similarity metric: cosine
- History window: 90 days
- Decay half-life: 30 days

#### 4.3.2 Metadata-Based Filtering

Scores content by explicit metadata match to learner preferences including topic overlap, modality match, difficulty compatibility, language match, and madhhab alignment.

#### 4.3.3 Knowledge Graph-Based Filtering

Scores content by KG relatedness between content facts and learner's known facts, weighted by learner mastery. Relatedness decays exponentially with graph distance.

### 4.4 Hybrid Approaches

#### 4.4.1 Weighted Hybrid

```
hybrid_score = w_cf x cf_score + w_cb x content_based_score + w_mf x mf_score
               + w_kg x kg_score + w_pop x popularity_score + w_rec x recency_score
```

Weights are functions of learner history length, content age, context mode, and cold start status.

#### 4.4.2 Cascading Hybrid

Stage 1: CF model narrows 16K to 5K (high recall). Stage 2: Content-based narrows 5K to 1K (high precision). Stage 3: Deep learning scores top 1K. Stage 4: Contextual bandit adds exploration bonus.

#### 4.4.3 Feature-Augmented Hybrid

Matrix factorization augmented with content and context features:
```
score = U[learner] . V[content] + theta . content_features + phi . context_features
```

This handles cold start naturally through content features.

### 4.5 Deep Learning Models

#### 4.5.1 Two-Tower Neural Network

Separate towers for learners and content, computing relevance as dot product.

**Learner Tower:** Input sparse + dense features, embedding layer, MLP [256, 128, 64] with ReLU + BatchNorm, L2 normalization.

**Content Tower:** Same architecture structure with content features.

**Output:** Dot product of normalized embeddings, passed through sigmoid for p(engagement).

**Training:** Binary cross-entropy loss, 5:1 negative sampling, Adam optimizer (lr 0.001), batch size 1024, 10 epochs.

#### 4.5.2 Deep Cross Network (DCN)

Explicitly models feature interactions using cross layers.

**Architecture:** Input features fed into both a Cross Network (4 cross layers with bounded-degree feature interactions) and a DNN path (MLP [256, 128, 64]). Combined via concatenation and linear layer to sigmoid output.

**Cross Layer Operation:** x_{l+1} = x_0 x (W_l x x_l + b_l) + x_l

#### 4.5.3 Transformer-Based Model

For session-aware sequential recommendation capturing learner behavior sequences.

**Architecture:** Input sequence of up to 50 actions (content embedding + context embedding), positional encoding, 3 transformer blocks (4-head self-attention, hidden 512, dropout 0.1). Candidate scoring via MLP on transformer output concatenated with candidate embedding.

### 4.6 Contextual Bandits

Adds exploration-aware scoring layer balancing known-good recommendations with exploration.

**Algorithms:**
- LinUCB: Upper confidence bound for new content exploration
- LinTS (Thompson Sampling): Posterior sampling for preference exploration
- Neural-LinUCB: Neural feature encoding with UCB
- Bootstrapped DQN: Ensemble variance as uncertainty

**Integration:** base_score + lambda_ucb x ucb_bonus + lambda_ts x thompson_sample + lambda_novelty x novelty_bonus. Lambda values depend on context (new learner gets high UCB, established learner gets low exploration).

---

## 5. RANKING

### 5.1 Multi-Objective Optimization

Ranking optimizes multiple competing objectives simultaneously:

| Objective | Description | Weight Range |
|---|---|---|
| Relevance | How relevant to learner's current needs | 0.3 - 0.5 |
| Engagement | How likely learner will engage deeply | 0.2 - 0.4 |
| Learning Impact | How much content improves knowledge | 0.2 - 0.4 |
| Diversity | Variety across topics, modalities, difficulty | 0.1 - 0.2 |
| Freshness | Recency bias toward new content | 0.05 - 0.15 |
| Serendipity | Unexpected but valuable discoveries | 0.0 - 0.1 |
| Fairness | Equal opportunity across content types | 0.05 - 0.1 |

**Multi-Objective Scoring:**
```
final_rank_score(c) = w_rel x relevance + w_eng x engagement + w_learn x learning_impact
                      + w_div x diversity_bonus + w_fresh x freshness
                      + w_serendip x serendipity + w_fair x fairness
```

Weights are personalized by learner segment, dynamic based on context, and adjustable via A/B testing.

### 5.2 Ranking Fusion

#### 5.2.1 Linear Fusion

Weighted linear combination of scores from each model. Weights calibrated via grid search, Bayesian optimization, or online bandit learning.

#### 5.2.2 Learning-to-Rank (LambdaMART)

Gradient boosted decision trees trained to predict optimal ranking directly. Features include all scoring model outputs, content features, cross features, context features, and ranking signals. Targets are relevance grades (0-4) derived from completion, accuracy improvement, rating, and engagement.

**Parameters:** 500 trees, learning rate 0.1, max leaves 64, min data in leaf 50, NDCG-based pairwise loss.

#### 5.2.3 Position-Aware Fusion

Content position influences score through position bias: position_bias(position) = a / (a + position - 1). Debiased fusion applies inverse propensity weighting to correct for position bias.

### 5.3 Diversity Enforcement

#### 5.3.1 Maximum Marginal Relevance (MMR)

Selects items that are both relevant and dissimilar to already-selected items:
```
MMR = argmax[lambda x relevance(c) - (1-lambda) x max similarity(c, selected)]
```
Lambda controls relevance-diversity trade-off (default: 0.5-0.7).

#### 5.3.2 Topic Diversity

Ensures recommendations span multiple topics with Jaccard similarity penalty for topic overlap.

#### 5.3.3 Modality Diversity

Ensures recommendations span multiple modalities. Penalty applied when same modality exceeds max per session (start: 2, middle: 2, end: 2, review: 3, weekly: 5).

#### 5.3.4 Difficulty Diversity

Ensures appropriate difficulty variety within a session. Content demoted if all selected items are significantly harder or easier.

### 5.4 Freshness Boosting

**Content Freshness:** New or updated content receives boost = 1 + alpha x exp(-days/14). Applies to content less than 90 days old.

**Trend Freshness:** Content gaining momentum (views_7d - views_prior_7d) / views_prior_7d receives additional boost.

**Seasonal Freshness:** Content relevant to Islamic calendar events receives boost: direct match 1.0, adjacent 0.6, preparatory 0.4.

### 5.5 Serendipity Injection

Serendipitous recommendations are unexpected yet relevant.

**Serendipity Scoring:** unexpectedness x relevance where unexpectedness = 1 - max_similarity to history and relevance = predicted engagement.

**Serendipity Sources:** Topic neighbors, modality newcomers, community hidden gems, cross-domain connections, time-based surprises, similar-learner discoveries.

**Serendipity Budget:** 10% of normal session recommendations, 30% of exploration sessions, 0% of review sessions.

### 5.6 Fairness Constraints

**Demographic Fairness:** Ensures recommendations do not systematically disadvantage any group based on language, geographic region, madhhab, education level, age group, or gender.

**Content Provider Fairness:** Ensures diverse content creators are represented.

**Disparate Impact Mitigation:** Maintains disparate impact ratio >= 0.8 across all protected groups.

---

## 6. FILTERING

### 6.1 Filtering Pipeline

After ranking, candidates pass through hard filters (remove) and soft filters (demote) to produce the final list.

### 6.2 Already-Consumed Filtering

Prevents recommending already-consumed content. Non-replayable content is removed. Replayable content has minimum intervals: stories 90 days, quizzes 30 days, flashcards immediate, podcasts 60 days, interactive 120 days. Cross-mode deduplication: same KG facts covered in last 7 days incurs demotion.

### 6.3 Difficulty-Appropriate Filtering

Hard filter: content outside [learner_level - 2, learner_level + 2] is demoted or removed. Soft ZPD scoring: ideal challenge at learner_level + 1 (1.0), comfortable at learner_level (0.8), stretch at +2 (0.5), review at -1 (0.6), too easy at -2 (0.3).

Dynamic difficulty adjustment: accuracy > 80% over last 5 quizzes increases optimal difficulty; accuracy < 50% decreases it. Max 2 levels per 24 hours.

### 6.4 Time-Appropriate Filtering

Content exceeding remaining session time is removed unless modular (then demoted). Content exceeding remaining attention span is demoted. Time-of-day appropriateness: early morning favors light content, post-lunch penalizes high-concentration content, peak hours have no penalty.

### 6.5 Content-Type Balance

Ensures balanced modality mix. Weekly targets: Story 15-25%, Quiz 15-25%, Flashcard 20-30%, Podcast 5-15%, AI Teacher 10-20%, Interactive 5-15%, Video 0-10%, Text 5-15%. Learner preferences shift within ranges. New modalities are gently introduced over time.

### 6.6 Parental Controls

Filters based on parent/guardian settings: maximum content rating, blocked topics, blocked modalities, allowed learning times, maximum daily screen time, topic sensitivity filter. Hard removal for violations.

### 6.7 Madhhab Filtering

Jurisprudential school alignment: direct match boosted, universal no change, multiple madhahib no change, mismatch with singular view removed, mismatch with comparative view allowed, mismatch otherwise demoted.

---

## 7. REAL-TIME PERSONALIZATION

### 7.1 Session-Based Recommendations

Real-time personalization adapts recommendations within a single session based on immediate learner behavior. A session state machine transitions between greeting, active learning, struggle/excel/bored substates, and session end.

**Real-Time Adaptation Signals:**
- Struggle: accuracy < 40%, response time > 2x baseline, help requests -> lower difficulty, switch modality
- Excellence: accuracy = 100%, response time < 0.5x baseline, seeks harder content -> increase difficulty, offer extensions
- Boredom: content abandonment, long pauses, skipping -> switch topic, inject novelty
- Fatigue: multiple pauses, declining quality, exceeding typical duration -> suggest break or light content

### 7.2 Real-Time Feature Updates

Features update per action: session items consumed, modalities used, engagement trend, fatigue estimate, difficulty performance. Feature freshness requirements: fatigue estimate 10s max stale, engagement trend 15s, confusion level 10s, session duration 1s, recent content immediate.

### 7.3 Bandit Model per Session

Lightweight bandit model runs in real-time with arms representing content categories. Updated per action with Bayesian posterior. Reset at session start.

### 7.4 Online Learning

Continuous model updates without full retraining: Matrix factorization via SGD every 100 events, two-tower via mini-batch every 1000 events, bandit per action, content embeddings incremental. Staleness detection monitors feature distribution drift, prediction error increase, and engagement metric drop to trigger retraining.

---

## 8. EXPLORATION VS EXPLOITATION

### 8.1 Exploration-Exploitation Framework

Balances exploiting known-good recommendations with exploring new options. Exploration rate varies by context: new learner 40%, established 15%, low engagement 30%, high engagement 10%, exploration mode 60%, review mode 5%.

### 8.2 Epsilon-Greedy

With probability epsilon, choose random content; otherwise choose highest-scored item. Default epsilon 0.1, decay rate 0.95 per 1000 recommendations, minimum 0.02. Limitation: explores uniformly without learning from outcomes.

### 8.3 Upper Confidence Bound (UCB)

Selects items with highest potential considering both estimated reward and uncertainty. Score = estimated_reward + exploration_parameter x sqrt(2 x log(t) / n_content). LinUCB variant uses per-content weight vectors with covariance matrices.

### 8.4 Thompson Sampling

Selects items according to probability of being optimal. Samples from posterior distribution of estimated reward. Beta-Bernoulli version for binary rewards: alpha = prior_alpha + positive_responses, beta = prior_beta + total - positive.

**Advantages:** Natural balance of exploration and exploitation, handles uncertainty asymmetrically, converges to optimal policy asymptotically.

### 8.5 Bayesian Bandits

Full Bayesian treatment: each content item has Beta(alpha_prior, beta_prior) reward distribution. Posterior updates after each recommendation. Exploration automatically decreases as posteriors concentrate with more data.

### 8.6 Explore-Then-Commit (ETC)

Pure exploration for first E rounds, then commit to best performer. Exploration rounds: 50 for new learner, 30 for new content. Re-exploration triggered by engagement drop below 0.5.

### 8.7 Context-Dependent Exploration

Exploration rate dynamically adjusts: base 10%, multiplied by history factor (new learner 2.0, established 1.0), engagement factor (declining 1.8, increasing 0.8), session phase (review 0.3, exploration 3.0), time pressure (remaining < 5 min: 0.3). Clamped to [0.02, 0.6].

**Exploration Budget Allocation:** Topic exploration 30%, modality exploration 20%, difficulty exploration 15%, social exploration 20%, seasonal exploration 15%.

---

## 9. COLD START

### 9.1 Cold Start Dimensions

Pure cold start (new learner + new content), learner cold start (new learner + existing content), content cold start (existing learner + new content), warm state (existing learner + existing content).

### 9.2 New Learner Cold Start

#### 9.2.1 Survey-Based Initialization

During onboarding, initial recommendations use interest vector (10 domain ratings), content preferences, goal embedding, learning style vector, session preferences, prior knowledge, demographic data. Rule-based mapping: introductory content for high-interest domains, preferred modalities prioritized, baseline difficulty computed from self-assessment.

#### 9.2.2 Demographic-Based Recommendation

Finds demographic cohort matching learner's age, language, region. Recommends the cohort's most engaged-with content, filtered by age-appropriateness, language, and introductory bias. Cohorts defined by age group x language x region with size-based weighting.

#### 9.2.3 Popularity-Based Recommendation

Global top content by completion rate x engagement, diversified across modalities, topics, and difficulties. Prioritizes universally appealing and introductory content.

### 9.3 New Content Cold Start

#### 9.3.1 Metadata-Based Recommendation

Extracts content features, finds similar existing content via embedding similarity, identifies learners who engaged with similar content, scores learners by similarity to content features, targets top-N learners for initial promotion.

#### 9.3.2 Content-Based Feature Prediction

Predicts engagement, completion, and effectiveness from content features. Predicts target audience segments. New content uses blended score: content relevance 0.4, metadata match 0.3, exploration bonus 0.2, predicted performance 0.1.

### 9.4 Hybrid Cold Start

Combines survey-based, demographic-based, and popularity-based strategies with weights adjusted by data availability. Phases: pure cold (100% hybrid), early learning 1-20 interactions (70% hybrid + 30% CF), emerging 21-100 interactions (40% hybrid + 60% CF), established 100+ interactions (10% hybrid + 90% full pipeline).

---

## 10. A/B TESTING

### 10.1 Experiment Framework

Systematic evaluation of recommendation algorithm changes through lifecycle: Ideate, Design, Deploy, Analyze, Decide.

### 10.2 Hypothesis Formation

Every experiment requires a clear falsifiable hypothesis specifying the independent variable change, expected effect, primary metric, guardrail metrics, and learner segment.

### 10.3 Metric Selection

**Primary Metrics:** CTR (15-25% baseline), Completion Rate (60-80%), Session Duration (15-30 min), Retention D7 (40-60%), Mastery Gain (0.03-0.10/week), Engagement Score (0.5-0.8), Diversity Score (0.4-0.7), User Satisfaction (3.5-4.5/5).

**Guardrail Metrics:** Session completion rate non-inferior (< 2% degradation), skip rate non-inferior (< 3%), frustration score non-inferior (< 5%), quiz accuracy non-inferior (< 2%), system latency p99 < 600ms, crash rate < 0.1%.

### 10.4 Randomization

Default randomization unit: learner (each learner in one variant). Alternative: session or request. Learner assignment via consistent hashing with stratification by tenure, engagement, and age group. Minimum sample size: 10,000 learners per variant for 80% power at 5% significance.

### 10.5 Statistical Significance

**Frequentist Approach:** Chi-square test for binary metrics, t-test or Mann-Whitney for continuous metrics. 95% confidence intervals. Multiple testing correction: Bonferroni for <= 10 metrics, Benjamini-Hochberg FDR for > 10.

**Bayesian Approach:** Beta-Bernoulli conjugate priors. Monte Carlo computation of probability treatment > control. 95% credible intervals for lift. Decision based on prob_treatment_better > 0.95 for significance.

**Decision Framework:** Both significant and positive -> Promote. One significant with moderate Bayesian confidence -> Promote with monitoring. Neither significant -> Continue or inconclusive. Negative -> Rollback. Guardrail breach -> Rollback immediately.

### 10.6 Automated Promotion

Gradual rollout in phases: canary 1% (1 day), internal 5% (2 days), limited 25% (5 days), majority 75% (7 days), full 100% (indefinite). Each phase has monitoring criteria and rollback conditions.

---

## 11. FEEDBACK LOOP

### 11.1 Feedback Loop Architecture

Captures learner interactions and incorporates them into the pipeline through real-time updates (bandit, session features, immediate feedback) and batch processing (model retraining, feature recomputation, A/B analysis, long-term trends).

### 11.2 Implicit Feedback

**Click Signals:** Captures impression timestamp, position, source; click timestamp and time-to-click; dwell time. Computes position bias, context weight, dwell weight for overall click weight.

**Time Spent Signals:** Absolute time, completion ratio, fully/partially consumed or abandoned. Positive weight proportional to completion ratio.

**Completion Rate Signals:** Per-modality completion tracking: quiz (questions answered, accuracy, first-attempt), story (segments read, comprehension, reflection), flashcard (cards reviewed, recall accuracy, ease factor), podcast (listened minutes, segments completed).

**Repeat Consumption Signals:** Repeat count, intervals, voluntary vs recommended repeat, loyalty score. Strong positive weight for voluntary repeats.

### 11.3 Explicit Feedback

**Ratings (1-5):** Normalized to 0.0-1.0. Positive (>=4), negative (<=2), neutral. Confidence weighted by consumption completeness and time since consumption.

**Like/Dislike:** Dislikes weighted 1.5x (stronger signal). Category propagation: dislike a story reduces preference for similar stories.

**Save/Bookmark:** Strong positive intent signal (weight 1.2). Intention inferred: "want to read later", "reference material", "favorite".

**Share Signals:** Endorsement weighted by target: peer 1.3, study group 1.5, teacher 0.8, social media 0.6. Content score increases for the sharing learner.

### 11.4 Feedback Incorporation

**Immediate (Real-time):** Session features update, bandit model update, learner embedding projection, rerank trigger for significant signals, adaptive engine updates.

**Batch (Scheduled):** Hourly: aggregate implicit signals, update feature store, refresh popularity. Every 6 hours: item similarity updates, trending scores. Daily: collaborative filtering retraining, learner similarity recomputation, cold start updates. Weekly: deep learning retraining, content embedding updates, bandit prior recalibration. Monthly: full model retraining, feature importance analysis, experiment finalization.

### 11.5 Delayed Feedback Handling

Feedback delays range from seconds (clicks) to months (repeat consumption, long-term mastery). Attribution modeling uses inverse propensity weighting by delay with confounding correction for intervening sessions.

**Delayed Positive (Mastery Gain):** Compares mastery before and 30 days after consumption. Significant gains update content effectiveness and boost similar content.

**Delayed Negative (Churn):** Analyzes recommendations preceding churn events. Reduces scores for categories associated with negative engagement patterns.

---

## 12. DIVERSITY AND SERENDIPITY

### 12.1 Diversity Metrics

**Topic Diversity:** Normalized entropy of topic distribution in recommendation set.

**Modality Diversity:** Entropy of modality distribution.

**Difficulty Diversity:** Range and variance of difficulty levels.

**Provider Diversity:** Number of unique content providers represented.

### 12.2 Topic Diversity

Target: recommendation set should span multiple topics with no single topic exceeding 30% of recommendations. Achieved through MMR with topic-overlap penalty.

### 12.3 Modality Diversity

Target: recommendation set should include at least 2 modalities per session, 4 per week. Enforced through modality violation penalties.

### 12.4 Difficulty Diversity

Target: recommendations should include varied difficulties within ZPD range. Pure review items may cluster at same difficulty.

### 12.5 Serendipity Scoring

Serendipity = unexpectedness x relevance. Unexpectedness measured by novelty of topic, modality, or content embedding. Relevance ensures content is still valuable.

### 12.6 Discovery Promotion

Dedicated discovery mechanisms: "Explore" mode with increased exploration rate, "Random Discovery" feature surfacing hidden gems, "Trending for You" showing what similar learners are engaging with, seasonal discovery collections.

---

## 13. SCALABILITY

### 13.1 Offline Computation

**Batch Scoring:** Full pipeline run nightly for daily plan generation. All candidates scored, ranked, and filtered in batch. Results cached for low-latency serving.

**Precomputed Embeddings:** Content embeddings precomputed during content ingestion (or update). Learner embeddings refreshed daily via batch projection. Item similarity matrices precomputed every 6 hours.

**Feature Precomputation:** Aggregate features (popularity, completion rates, effectiveness scores) recomputed on scheduled intervals. Knowledge state aggregates precomputed nightly.

### 13.2 Online Computation

**Real-Time Features:** Lightweight session features computed per action without heavy dependencies. Learner-agnostic content features served from fast cache.

**Lightweight Models:** Two-tower model inference reduces to embedding lookup + dot product. Bandit models maintain per-content parameters (alpha, beta) for fast sampling.

**Caching Strategy:**
- Learner embeddings: cache with TTL 1 hour or invalidate on significant profile change
- Content embeddings: permanent cache, updated on content changes
- Feature store: Redis with TTL-based eviction
- Recommendation lists: cache for 5-15 minutes depending on list type
- Model predictions: cache frequently requested (learner, content) pairs

### 13.3 Hybrid Serving

Batch pipeline precomputes daily plans and caches results. Online pipeline handles real-time next-action recommendations. Streaming pipeline handles lightweight session adaptations. Traffic routed based on request type and latency requirements.

### 13.4 Index Structures

**Approximate Nearest Neighbor (ANN) Index:** For content-based similarity search using HNSW or IVF-PQ. Enables fast nearest-neighbor lookups among 500K+ items without exhaustive comparison.

**Inverted Indexes:** Topic-content mapping, difficulty-content mapping, modality-content mapping, curriculum-content mapping. Built offline, updated incrementally.

**Feature Index:** Learner feature store indexed by learner ID for O(1) lookup. Content feature store indexed by content ID.

**Embedding Index:** FAISS or ScaNN index for content embeddings. Partitioned by language for reduced search space.

### 13.5 Serving Architecture

**Request Flow:**
1. Request arrives at load balancer
2. Feature service fetches learner and context features (5ms)
3. Candidate generator queries precomputed indexes (20ms)
4. Scoring service runs ensemble inference (100ms)
5. Ranking service applies reranker (50ms)
6. Filtering service applies constraints (20ms)
7. Response assembled and returned (5ms)

**Horizontal Scaling:** Each pipeline stage independently scalable. Feature service scales with QPS. Candidate generation scales with content pool size. Scoring scales with model complexity. Target: 10K requests/second per model instance.

---

## 14. EVALUATION

### 14.1 Offline Metrics

#### 14.1.1 Accuracy Metrics

| Metric | Definition | Target | When to Use |
|---|---|---|---|
| Precision@K | Fraction of relevant items in top-K | 0.4-0.7 | Assess recommendation precision |
| Recall@K | Fraction of relevant items retrieved in top-K | 0.3-0.6 | Assess recommendation recall |
| NDCG@K | Normalized discounted cumulative gain | 0.5-0.8 | Assess ranking quality with graded relevance |
| MRR | Mean reciprocal rank of first relevant item | 0.3-0.6 | When single relevant item exists |
| Hit Rate@K | Whether at least one relevant item in top-K | 0.6-0.9 | Practical measure of success |
| MAP | Mean average precision across queries | 0.3-0.6 | Overall ranking quality |

#### 14.1.2 Coverage Metrics

| Metric | Definition | Target |
|---|---|---|
| Catalog Coverage | % of content pool recommended at least once | > 40% |
| User Coverage | % of learners receiving recommendations | > 99% |
| Cold Start Coverage | % of new items receiving recommendations | > 70% within 7 days |
| Long Tail Coverage | % of recommendations from less popular items | > 20% |

#### 14.1.3 Diversity Metrics

| Metric | Definition | Target |
|---|---|---|
| Topic Entropy | Entropy of topic distribution in recommendations | > 0.7 (normalized) |
| Modality Entropy | Entropy of modality distribution | > 0.6 (normalized) |
| Intra-List Similarity | Avg pairwise similarity within recommendation list | < 0.5 |
| Serendipity Rate | % of recommendations with high serendipity score | > 10% |

#### 14.1.4 Novelty and Freshness Metrics

| Metric | Definition | Target |
|---|---|---|
| Mean Popularity Rank | Avg popularity of recommended items | Moderate |
| Freshness Ratio | % of recommendations < 30 days old | > 15% |
| Exploration Rate | % of recommendations from new category | 10-20% |

### 14.2 Online Metrics

#### 14.2.1 Engagement Metrics

| Metric | Definition | Target |
|---|---|---|
| Click-Through Rate (CTR) | Clicks / Impressions | 15-25% |
| Completion Rate | Completed / Started | 60-80% |
| Session Duration | Minutes per session | 15-30 min |
| Sessions per Week | Weekly session count | 4-7 |
| Repeat Usage Rate | Learners returning within 7 days | > 50% |
| Time to First Click | Seconds from impression to click | < 5s |

#### 14.2.2 Effectiveness Metrics

| Metric | Definition | Target |
|---|---|---|
| Mastery Gain | Weekly per-learner knowledge increase | > 0.03 |
| Quiz Accuracy | Correct / Total questions answered | 65-85% |
| Weakness Reduction | % decrease in identified weaknesses | > 20% monthly |
| Retention Rate | Active at Day 7 / Day 30 / Day 90 | > 40% / > 25% / > 15% |
| Learning Velocity | New facts mastered per week | 5-15 |

#### 14.2.3 Satisfaction Metrics

| Metric | Definition | Target |
|---|---|---|
| User Satisfaction Score | Post-session rating (1-5) | > 4.0 |
| Net Promoter Score (NPS) | Likelihood to recommend | > 30 |
| Frustration Rate | % of sessions with frustration signals | < 10% |
| Skip Rate | % of recommendations skipped | < 20% |
| Feedback Submission Rate | % of learners providing explicit feedback | > 15% |

### 14.3 A/B Testing Metrics

| Metric Category | Metric | Minimum Detectable Effect | Minimum Sample |
|---|---|---|---|
| Primary | CTR, Completion, Retention | 5% relative | 10,000/variant |
| Guardrail | Latency, Error Rate, Skip Rate | 2% absolute | 5,000/variant |
| Segment | Per-segment variant of primary | 10% relative | 3,000/segment |
| Counter | Short-term trade-off metrics | 10% relative | 5,000/variant |

### 14.4 Counterfactual Evaluation

**Off-Policy Evaluation (OPE):** Uses inverse propensity scoring (IPS) to estimate online metrics from logged data without running new A/B tests. Self-normalized IPS (SNIPS) reduces variance. Importance weight clipping prevents extreme weights.

**Replay Evaluation:** Uses historical logged data to replay recommendations as if a new policy was in place. Handles position bias through propensity weighting.

**Interleaved Evaluation:** Presents interleaved results from two policies and measures user preference without separate test groups.

### 14.5 Evaluation Pipeline

**Offline Evaluation (pre-deployment):**
1. Split historical data into train/validation/test (70/15/15)
2. Compute offline metrics on test set
3. Compare against baseline model
4. Statistical significance test (paired t-test or bootstrap)
5. Check for overfitting (train vs test metric gap)
6. Segment analysis (by learner type, content type, etc.)
7. Coverage and diversity checks
8. Fairness audit across demographic groups

**Online Evaluation (in production):**
1. A/B test with hypothesis, metrics, and sample size
2. Real-time monitoring dashboard
3. Automated significance testing at scheduled intervals
4. Guardrail metric alerts
5. Long-term effect analysis (weekly/monthly)
6. Segment breakdown analysis
7. Qualitative feedback collection

---

## 15. CROSS-DOCUMENT INTEGRATION

### 15.1 Integration with 00_Personal_AI_Mentor

The Recommendation Engine is invoked by the Mentor's Content Recommendation Engine component (Section 9 of the Mentor document). The Mentor provides the learner model state, weakness predictions, forgetting predictions, and session context. The Recommendation Engine returns scored and ranked content candidates. The Mentor validates, potentially overrides, and presents recommendations to the learner. Feedback flows back to both systems.

### 15.2 Integration with 01_Learning_Experience_Bible

Each user-facing experience described in the Bible maps to specific pipeline configurations: First Lesson uses cold-start pipeline, Daily Experience uses full pipeline, Weekly Experience uses batch with thematic clustering, Long Break Recovery uses high review weight, Review Timing uses memory-weighted pipeline, Session Design uses stream pipeline with real-time adaptation.

### 15.3 Integration with 02_Memory_Engine

Forgetting risk scores, optimal review schedules, ease factors, and interleaving suggestions from the Memory Engine serve as primary inputs to the candidate generation and scoring stages. Content addressing high-forgetting-risk facts receives significant score boosts.

### 15.4 Integration with 08_Multi_Modal_Learning

Modality definitions, pedagogical characteristics, and modality-specific engagement patterns inform modality affinity scoring, modality rotation enforcement, and multi-modal reinforcement strategies within the recommendation pipeline.

### 15.5 Integration with 09_Adaptive_Learning_Engine

Difficulty constraints, strategy constraints, pacing constraints, and intervention mode signals from the Adaptive Learning Engine serve as hard constraints and scoring weights within the recommendation pipeline. The adaptive engine's real-time state determines dynamic weight adjustments.

---

*Document Version: 1.0.0 | Last Updated: 2026-07-10 | Custodian: Learning Experience Architecture Team*
