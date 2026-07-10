# 09 — Adaptive Learning Engine

## Overview

The Adaptive Learning Engine (ALE) is the central nervous system of the Dien learning platform. It is the real-time decision-making core that continuously monitors every learner interaction, analyzes patterns across multiple dimensions, and dynamically adjusts every aspect of the learning experience — content, difficulty, modality, pace, sequence, support, and goals — to create an optimally personalized learning journey for each individual.

The ALE is not a single algorithm but an orchestrated ecosystem of subsystems: the learner model that maintains a live portrait of knowledge, motivation, and context; the adaptation rules engine that encodes pedagogical principles into executable policies; the machine learning pipeline that discovers patterns invisible to rule-based systems; and the feedback integration loop that closes the gap between intended and actual learning outcomes.

This document connects to the Personal AI Mentor (00), Curriculum Paths (37), Quiz Mode (13), Memory Engine (02), Educational Psychology (04), and Instructional Design (05) subsystems.

---

## 1. Adaptive Engine Architecture

### 1.1 Core Architecture

The Adaptive Learning Engine operates on a continuous observe-analyze-decide-act loop that cycles for every learner interaction, typically within 50–200 milliseconds to ensure real-time responsiveness.

### 1.2 Architectural Layers

**Layer 1 — Data Ingestion Layer:**
This layer captures every learner interaction in real time, normalizes it into a standard event format, and feeds it into the processing pipeline. Events include:
- Content interactions (page views, scroll depth, time on section, click patterns)
- Assessment events (answers submitted, scores earned, hints used, time to answer)
- Navigation events (page transitions, modality switches, search queries)
- Engagement events (logins, session start/end, pause/resume, completion)
- Context events (time of day, device type, location, network quality)
- Bio-behavioral events (response time variance, error clustering, fatigue indicators)

**Layer 2 — Learner Model Layer:**
The learner model is a live, multi-dimensional representation of the learner that is updated after every processed event. It maintains:
- Knowledge state for every concept in the curriculum
- Performance history with granular metrics
- Behavioral patterns and learning analytics
- Motivational and affective state estimates
- Contextual and environmental factors
- Learner identity attributes (goals, preferences, language, madhhab)

**Layer 3 — Adaptation Engine Layer:**
This is the core decision-making layer that queries the learner model and selects optimal adaptations across all adaptation dimensions. It operates through:
- A rules engine that evaluates condition-action pairs
- A machine learning inference pipeline for pattern-based predictions
- A multi-objective optimizer that balances competing goals
- An experimentation framework for A/B testing adaptations

**Layer 4 — Execution Layer:**
This layer implements the adaptation decisions by communicating with downstream systems:
- Content delivery system (select, filter, or generate content)
- Assessment engine (adjust difficulty, question selection)
- Modality selector (switch or adjust modalities)
- Pacing controller (adjust speed, schedule breaks)
- Sequencing engine (reorder learning path)
- Scaffolding manager (adjust hints, supports, feedback)

**Layer 5 — Measurement and Feedback Layer:**
This layer closes the loop by measuring the effectiveness of each adaptation, detecting when adaptations are not working, and feeding this data back into the learner model and rule system to improve future decisions.

### 1.3 The Adaptive Decision Loop

The ALE operates on a continuous decision loop that cycles for each learner event:

```
┌──────────────────────────────────────────────────┐
│                    OBSERVE                        │
│  Capture learner event (answer, click, pause...)  │
└──────────────────────┬───────────────────────────┘
                       ↓
┌──────────────────────────────────────────────────┐
│                   ANALYZE                         │
│  Interpret event → Update learner model          │
│  Detect patterns → Classify situation            │
└──────────────────────┬───────────────────────────┘
                       ↓
┌──────────────────────────────────────────────────┐
│                    DECIDE                         │
│  Evaluate rules → Run ML models                  │
│  Score options → Select best adaptation          │
└──────────────────────┬───────────────────────────┘
                       ↓
┌──────────────────────────────────────────────────┐
│                    ACT                            │
│  Execute adaptation → Deliver modified           │
│  content/difficulty/pace/sequence/support        │
└──────────────────────┬───────────────────────────┘
                       ↓
┌──────────────────────────────────────────────────┐
│                   FEEDBACK                        │
│  Measure outcome → Detect effectiveness          │
│  Update model → Refine rules                     │
└──────────────────────────────────────────────────┘
                       ↓
                    (loop continues)
```

### 1.4 Data Flow Architecture

```
Learner Actions
      ↓
[Event Bus] ←──────────────────────┐
      ↓                             │
[Event Normalizer]                  │
      ↓                             │
[Feature Extractor]                 │
      ↓                             │
┌─────────────────────────────┐    │
│     Learner Model Store     │    │
│  ┌───────────────────────┐  │    │
│  │ Knowledge Graph       │  │    │
│  │ Performance Metrics   │  │    │
│  │ Behavior Patterns     │  │    │
│  │ Motivation State      │  │    │
│  │ Context Profile       │  │    │
│  └───────────────────────┘  │    │
└─────────────────────────────┘    │
      ↓                             │
┌─────────────────────────────┐    │
│   Adaptation Decision Engine│    │
│  ┌───────────────────────┐  │    │
│  │ Rules Engine          │  │    │
│  │ ML Inference          │  │    │
│  │ Multi-Objective Opt   │  │    │
│  │ A/B Evaluator         │  │    │
│  └───────────────────────┘  │    │
└─────────────────────────────┘    │
      ↓                             │
[Adaptation Executor]               │
      ↓                             │
[Content / Difficulty / Modality /  │
 Pace / Sequence / Support / Goal]  │
      ↓                             │
[Learner Experiences Change]        │
      ↓                             │
[Effect Measured] →─────────────────┘
```

### 1.5 Real-Time Decision Loop Specifications

| Parameter | Target | Maximum Tolerable |
|-----------|--------|-------------------|
| Event capture latency | < 10ms | 50ms |
| Learner model update | < 20ms | 100ms |
| Adaptation decision | < 50ms | 200ms |
| Content selection | < 30ms | 150ms |
| Full loop end-to-end | < 150ms | 500ms |
| Sync to server | < 1000ms | 5000ms |

### 1.6 State Management

The ALE maintains several categories of state:

**Persistent State (database-backed):**
- Learner profile attributes (demographics, goals, preferences)
- Knowledge state across all curriculum concepts
- Long-term performance history (weeks/months)
- Completed content and assessments
- Earned credentials and achievements

**Session State (in-memory, session-scoped):**
- Current content position and navigation path
- Session start time, elapsed time, estimated remaining
- Recent interaction history (last 200 events)
- Current difficulty level and adaptation settings
- In-progress assessment state

**Real-Time State (in-memory, ephemeral):**
- Current engagement level (focused, distracted, fatigued)
- Immediate response time patterns (last 5–10 events)
- Emotional state indicators (frustration, flow, boredom)
- Environmental context (noise level, movement, connectivity)

### 1.7 Scalability Architecture

The ALE is designed to scale from a single learner to millions:

- **Horizontal scaling:** The decision engine is stateless; state is maintained in a distributed cache (Redis). Multiple engine instances can process events in parallel for different learners.
- **Sharding:** Learner models are sharded by learner ID, with each shard handled by a dedicated engine instance group.
- **Batch processing:** Non-real-time adaptations (daily plans, weekly reviews) run on a separate batch processing cluster.
- **Content delivery:** Static content is served via CDN; dynamic adaptations are computed at the edge.

### 1.8 Fault Tolerance

- **Graceful degradation:** If the ALE is unavailable, the platform falls back to a default non-adaptive learning path with cached content.
- **Local decision caching:** The client maintains a local copy of the most recent adaptation decisions, allowing 15 minutes of autonomous operation if the server is unreachable.
- **Decoupled components:** If the ML inference engine fails, the rules engine can operate independently. If both fail, default content is served.
- **Rollback capability:** All adaptation decisions are logged; the system can roll back to a previous decision if a negative outcome is detected.

---

## 2. Learner Model Input

### 2.1 Model Dimensions

The learner model is a multi-layered representation of the learner across eight primary dimensions. Each dimension is continuously updated as new data arrives.

### 2.2 Knowledge State

**Concept-level mastery tracking:**
- Every concept in the curriculum graph has a mastery estimate (0.0 to 1.0)
- Mastery is updated using Bayesian Knowledge Tracing (BKT) or Deep Knowledge Tracing (DKT)
- Four knowledge states per concept: Unlearned, Learning, Learned, Mastered
- Forgetting model: Mastery decays over time without reinforcement
- Prerequisite relationships: Mastery of prerequisite concepts is required before new concepts are introduced

**Knowledge Representation:**
- **Knowledge graph:** A directed graph where nodes are concepts and edges represent prerequisite, enrichment, and relationship links
- **Skill vectors:** For ML models, the learner's knowledge is represented as a dense vector embedding
- **Strength metrics:** Per-concept scores for recall, comprehension, application, and synthesis
- **Confidence estimates:** The model's confidence in its own knowledge estimates (uncertain high → more testing needed)

**Update Rules:**
- Correct answer → increase mastery (amount depends on item difficulty and learner ability)
- Incorrect answer → decrease mastery slightly; increase confusion measure
- Hints used → decrease the mastery gain for correct answers
- Response time very short (< 2s) over multiple items → may indicate memorization without understanding
- Response time very long + correct → may indicate successful retrieval but high effort

### 2.3 Performance History

**Metrics Tracked (per concept, per session, per day, per week, cumulative):**
- Accuracy rate (correct / total responses)
- Average response time
- Response time variance
- Hints used per item
- Items attempted
- Mastery progression rate
- Review accuracy (how well previously mastered concepts are retained)
- Learning rate (new concepts mastered per unit time)

**Performance Trends:**
- **Improving:** Accuracy increasing, response time decreasing, hints decreasing
- **Stable:** Consistent accuracy, consistent response time
- **Declining:** Accuracy decreasing, response time increasing, hints increasing
- **Fluctuating:** High variance across similar items — may indicate unstable knowledge
- **Plateaued:** No improvement despite continued effort — may indicate need for different approach

**Error Pattern Analysis:**
- **Random errors:** May indicate lack of knowledge
- **Systematic errors:** Consistent wrong answers on related concepts — may indicate misconception
- **Careless errors:** Fast responses with mistakes — may indicate overconfidence or fatigue
- **Guessing pattern:** Very fast responses, alternating correct/incorrect — may indicate random guessing
- **Retrieval failure:** Slow responses followed by errors — may indicate weak memory trace

### 2.4 Behavioral Patterns

**Engagement Metrics:**
- **Session frequency:** Days studied per week, sessions per day
- **Session duration:** Average and variance of study session lengths
- **Content consumption rate:** Lessons completed, chapters covered per session
- **Interaction depth:** Time on page, scroll depth, annotation activity
- **Navigation patterns:** Linear progression vs. jumping around vs. repeat visits
- **Modality preference:** Which modalities are used, for how long, with what outcomes

**Learning Behavior Classification:**
- **Consistent studier:** Regular sessions, steady progression, predictable patterns
- **Binge learner:** Long intense sessions followed by gaps — may need pacing guidance
- **Explorer:** Jumps between topics, follows curiosity — may need structure
- **Reviewer:** Frequently revisits completed material — may need confidence building
- **Minimalist:** Does the minimum to complete — may need engagement strategies
- **Perfectionist:** Spends excessive time on single items — may need time management

**Behavioral Red Flags:**
- Sudden drop in session frequency (disengagement risk)
- Repeated attempts without improvement (frustration)
- Skipping all optional content (low engagement)
- Extremely fast completion of assessments (cheating or guessing)
- Abandoning sessions mid-way (frustration or time pressure)

### 2.5 Motivational State

**Intrinsic Motivation Indicators:**
- **Curiosity:** Following optional exploration paths, asking questions, exploring tangents
- **Mastery orientation:** Repeating items until mastered, seeking challenging content
- **Autonomy:** Making independent learning choices, customizing experience
- **Purpose:** Engaging with content beyond requirements, connecting learning to life

**Extrinsic Motivation Indicators:**
- **Achievement seeking:** Pursuing badges, points, leaderboard positions
- **Completion focus:** Prioritizing finishing over understanding
- **Social motivation:** Engaging in group activities, discussions

**Motivational State Estimation:**
The ALE infers motivational state from multiple signals:
- **Effort signals:** Time on task, persistence after errors, use of hints
- **Choice signals:** Selection of harder vs. easier options, optional vs. required content
- **Emotional signals:** Response time patterns, error rates, session abandonment
- **Self-report:** Periodic micro-surveys ("How motivated are you right now?")

**Affective State Detection:**
- **Boredom:** Fast responses, low accuracy, skipping content, distracted navigation
- **Frustration:** Repeated errors, increasing response time, hint overuse, negative feedback
- **Flow:** Optimal challenge balance, steady progress, deep focus, time distortion
- **Confusion:** Hesitation, backtracking, seeking help, error recovery attempts
- **Curiosity:** Exploring beyond requirements, following tangents, asking questions
- **Anxiety:** Very fast or very slow responses, test avoidance, self-deprecating comments

### 2.6 Contextual Factors

**Environmental Context:**
- **Time of day:** Morning, afternoon, evening, night
- **Day of week:** Weekday vs. weekend
- **Location:** Home, commute, work, mosque, public space
- **Device:** Desktop, tablet, phone, smartwatch
- **Connectivity:** Online, offline, low bandwidth, high bandwidth
- **Social context:** Alone, with family, with study group, supervised

**Temporal Context:**
- **Time since last session:** Minutes, hours, days (affects retrieval strength)
- **Time since last sleep:** Hours awake (affects cognitive capacity)
- **Current session duration:** Minutes elapsed (fatigue increases with duration)
- **Upcoming events:** Exam, deadline, event (affects learning goals and priority)

**Cultural Context:**
- **Language preference:** Primary language, secondary language(s)
- **Madhhab:** Hanafi, Maliki, Shafi'i, Hanbali (affects fiqh content)
- **Scholarly tradition:** Regional scholarly preferences
- **Islamic calendar:** Ramadan, Dhul Hijjah, Muharram — affecting content relevance
- **Local customs:** Culturally appropriate examples and analogies

### 2.7 Real-Time Signals

**Immediate Behavioral Signals:**
- **Response time:** Milliseconds since item presentation
- **Response time deviation:** Difference from learner's baseline
- **Cursor/touch patterns:** Hovering, hesitation, rapid movement, precision
- **Scroll behavior:** Fast scanning vs. careful reading vs. skipping
- **Pause behavior:** Where and how long learners pause
- **Re-reading behavior:** Returning to previously read sections
- **Annotation activity:** Highlighting, note-taking during content

**Physiological Signals (where available):**
- **Eye tracking:** Fixation duration, saccade patterns, pupil dilation
- **Heart rate:** Changes indicating stress, engagement, or relaxation
- **Facial expression:** Detected emotional states
- **Voice analysis:** Tone, pace, pitch changes during voice interactions
- **Posture/activity:** Device motion, typing patterns, touch pressure

**Derived Real-Time Metrics:**
- **Momentary Engagement Index:** Composite of scroll, click, annotation, and time-on-task signals
- **Cognitive Load Estimate:** Response time + error rate + pause frequency
- **Confusion Probability:** Based on hesitation + backtracking + help-seeking
- **Fatigue Index:** Session duration + error rate trend + response time slowing
- **Flow Probability:** Optimal difficulty + steady pace + deep focus signals

### 2.8 Model Update Frequency

| Signal Type | Update Frequency | Persistence |
|-------------|-----------------|-------------|
| Knowledge state (concept mastery) | After each assessment | Persistent (database) |
| Knowledge state (inferred) | Every 5 interactions | Persistent (database) |
| Performance metrics | After each assessment | Persistent (database) |
| Behavioral patterns | Every session | Persistent (database) |
| Motivational state | Every 10 minutes | Session + persistent |
| Contextual factors | On change | Session |
| Real-time signals | Every event (50-200ms) | Ephemeral (in-memory) |

---

## 3. Adaptation Dimensions

### 3.1 The Seven Adaptation Axes

The ALE adapts learning across seven primary dimensions:

1. **Content (What):** What specific content is presented
2. **Difficulty (How Hard):** The challenge level of content and assessments
3. **Modality (How):** The sensory channel through which content is delivered
4. **Pace (How Fast):** The speed at which content is covered
5. **Sequence (What Order):** The order in which content is encountered
6. **Support (How Much Help):** The level of scaffolding and assistance
7. **Goal (What For):** The learning objectives and standards applied

### 3.2 Content Adaptation

**Selection Criteria:**
- Content aligned with current learning objectives
- Appropriate for learner's knowledge state (prerequisites met)
- Matched to learner's interests and goals
- Culturally and madhhab-appropriate
- Modality-compatible with current context

**Content Customization:**
- **Example selection:** Use examples from domains the learner is familiar with
- **Analogy personalization:** Choose analogies that resonate with the learner's background
- **Explanation depth:** Adjust level of detail — overview, standard, or deep dive
- **Language complexity:** Simplify or enrich language based on learner's proficiency
- **Contextualization:** Frame content in contexts relevant to the learner's life

**Content Filtering:**
- Exclude content the learner has already mastered
- Exclude content for which prerequisites are not met
- Prioritize content aligned with learner's current goals
- Filter by madhhab, scholarly preference, or regional tradition

### 3.3 Difficulty Adaptation

**Dynamic Difficulty Adjustment (DDA):**
- Continuously adjusts difficulty to maintain optimal challenge
- Difficulty calibrated per content type (concept, procedure, recall, application)
- Assessment difficulty adapts: easier questions for weak areas, harder for strong areas

**Optimal Challenge Window:**
- Target accuracy: 70–85% correct (research-optimal learning zone)
- Below 70%: Too difficult → simplify, provide scaffolding, offer easier alternatives
- Above 85%: Too easy → increase difficulty, accelerate, add enrichment
- 70–85%: Optimal → maintain current difficulty, gradual progression

**Flow Corridor Maintenance:**
- If accuracy > 85% for 5+ consecutive items → increase difficulty
- If accuracy < 60% for 3+ consecutive items → decrease difficulty
- If response time < 30% of baseline for 5+ items → increase difficulty (too easy)
- If response time > 200% of baseline for 3+ items → decrease difficulty (too hard)
- If hints used > 50% of items → decrease difficulty

**Difficulty Adjustment Mechanisms:**
- **Content difficulty:** Choose more/less complex texts, simpler/richer examples
- **Assessment difficulty:** Adjust question complexity, distractor quality, answer options
- **Task demands:** From recognition (easiest) → recall → comprehension → application → analysis → synthesis → evaluation (hardest)
- **Support level:** More worked examples, hints, and scaffolding for harder content
- **Response requirements:** Multiple choice (easiest) → short answer → essay → oral explanation → demonstration (hardest)

### 3.4 Modality Adaptation

**Modality Selection Logic:**
- Primary modality selected based on content type (see document 08)
- Secondary modality added for reinforcement when needed
- Modality switched when learner engagement or performance declines
- Multi-modal combinations optimized for cognitive load

**Modality Switching Triggers:**
- Learner manually switches modality
- Engagement drops below threshold in current modality
- Performance below expected in current modality
- Context changes (location, device, time available)
- Fatigue detected (shift to passive modality)
- Content type changes (different modality affinity)

**Modality Effectiveness Tracking:**
- Per-learner, per-content-type modality effectiveness scores
- Scores updated after each learning interaction
- Primary modality: highest effectiveness score determines default
- Switching recommendation: secondary modality with highest effectiveness score

### 3.5 Pacing Adaptation

**Speed Adjustment:**
- **Auto-pacing:** System adjusts content delivery speed based on learner's processing speed
- **Reading speed:** Adjusts text display speed based on reading rate
- **Audio speed:** Sets default playback speed based on comprehension and preference
- **Interaction timing:** Adjusts time limits based on learner's typical response time

**Time Allocation:**
- More time allocated to challenging concepts
- Less time to already-mastered content (skip or fast-track)
- Time budgeting per session based on learner's available time and goals
- Break frequency adjusted based on fatigue indicators

**Session Length Optimization:**
- Optimal session length determined by learner's historical engagement curves
- Session ends at natural break points when engagement begins to decline
- Recommended session length communicated before session starts
- Micro-sessions (5–10 min) for low-availability periods
- Extended sessions (30–60 min) for high-availability periods

**Break Scheduling:**
- Breaks suggested when fatigue indicators reach threshold
- Break duration suggested based on session intensity and duration
- Break activities suggested (stretching, walking, hydration, dhikr)
- Optional break override for learners who prefer to continue

**Content Density Control:**
- High density: More content per unit time, faster transitions — for high-ability, high-engagement learners
- Medium density: Standard content volume — default
- Low density: Less content, more reflection time, more repetition — for struggling or fatigued learners
- Content density adjusted by removing non-essential examples, shortening explanations, or reducing enrichment content

### 3.6 Sequencing Adaptation

**Path Personalization:**
- Default path: Curriculum as designed by instructional designers
- Accelerated path: Skip prerequisites already mastered, move faster
- Remedial path: Insert remedial content when mastery gaps are detected
- Enrichment path: Add depth, breadth, and related content for interested learners

**Prerequisite Adjustment:**
- If learner shows mastery of prerequisites through prior assessment, skip prerequisite review
- If learner struggles with current content despite prerequisites being marked complete, re-test prerequisite mastery
- Dynamic prerequisite insertion: If learner encounters a concept that requires unmastered prior knowledge, auto-insert the prerequisite content

**Skipping and Enrichment:**
- **Mastery skip:** Learner can test out of content they already know
- **Interest skip:** Learner can skip content they are not interested in (with awareness that later content may require it)
- **Enrichment insertion:** Automatically insert supplementary content when learner shows high interest or rapid mastery
- **Detour allowance:** Permit learner-driven exploration of related topics with automatic return path

**Remediation Insertion:**
- When assessment reveals gaps, remediation content is inserted before proceeding
- Remediation is targeted to specific gaps, not a full repeat of the content
- Different remediation strategies tried if first attempt does not resolve the gap
- Remediation can be in a different modality than the original presentation

### 3.7 Support Adaptation

**Scaffolding Fading:**
- **Full support:** Worked examples, step-by-step guidance, explicit instructions
- **Partial support:** Completion problems (half-solved), guided questions, hint sequences
- **Minimal support:** Independent problem-solving with error-only feedback
- **No support:** Open-ended application, synthesis, evaluation tasks

**Fading Triggers:**
- Learner demonstrates consistent success at current support level
- Learner requests less support
- Learner's accuracy exceeds 85% at current support level for 5+ items
- Learner's response time decreases while maintaining accuracy

**Scaffolding Increase Triggers:**
- Learner demonstrates consistent difficulty at current support level
- Learner requests more support
- Learner's accuracy falls below 60% at current support level
- Learner's frustration indicators increase

**Hint Progression:**
- **Hint 1:** Minimal clue — direction of thinking, relevant concept name
- **Hint 2:** Moderate clue — narrowed options, partial solution
- **Hint 3:** Strong clue — almost the answer, key insight
- **Hint 4 (if enabled):** Show answer with explanation

**Hint Effectiveness Tracking:**
- How many hints needed before correct answer (fewer = better)
- Performance on similar items after hint use (hint effectiveness)
- Learner's preference for hint style (conceptual vs. procedural vs. answer)

**Worked Example Fading:**
- **Full worked example:** Problem + complete solution + explanation
- **Completion example:** Problem + partial solution + learner completes
- **Practice problem:** Problem only + learner solves independently
- **Application task:** Real-world scenario + learner applies concepts

**Prompt Reduction:**
- Initial learning: Detailed prompts at each step
- Intermediate: Reduced prompts, key reminders only
- Advanced: No prompts, learner expected to initiate all steps independently
- Expert: Learner teaches concepts to others (peer instruction)

### 3.8 Feedback Adaptation

**Feedback Timing:**
- **Immediate feedback:** After each response — for procedural skills, factual recall, and error correction
- **Delayed feedback:** At the end of a set or session — for conceptual understanding and problem-solving
- **Adaptive timing:** Immediate for errors (corrective), delayed for correct answers (to strengthen retrieval)
- **Self-requested feedback:** Learner controls when they receive feedback

**Feedback Type:**
- **Corrective feedback:** Indicates error and shows correct answer
- **Explanatory feedback:** Explains why the answer is correct/incorrect
- **Hints and prompts:** Guides learner toward correct answer without giving it
- **Metacognitive feedback:** Prompts self-reflection ("How confident are you?" "What strategy did you use?")
- **Attributional feedback:** Attributes success to effort and strategy ("You solved this by carefully applying the rule")

**Feedback Depth:**
- **Minimal:** Correct/incorrect indicator only
- **Standard:** Correct answer shown with brief explanation
- **Detailed:** Full explanation with references, related concepts, and common misconceptions
- **Adaptive:** Depth matches learner's knowledge state (more detail for struggling learners, less for advanced)

**Feedback Tone:**
- **Encouraging:** For struggling learners, emphasizes effort and progress
- **Neutral:** For most learners, focuses on information
- **Direct:** For advanced learners or repeated errors, straightforward correction
- **Socratic:** For reflective learners, questions that guide discovery

---

## 4. Content Adaptation

### 4.1 Content Selection Engine

The content selection engine matches content items to the current learning situation using a multi-factor scoring system:

**Scoring Factors:**
- **Pedagogical relevance:** How directly does this content address the current learning objective?
- **Prerequisite alignment:** Have all prerequisites been met?
- **Knowledge state fit:** Is the content at the right level for the learner's current mastery?
- **Interest alignment:** Does this content match the learner's stated or inferred interests?
- **Goal alignment:** Does this content serve the learner's current learning goals?
- **Modality compatibility:** Can this content be effectively delivered in the current modality?
- **Context appropriateness:** Is this content suitable for the current context (time, place, device)?
- **Freshness:** Has this content been seen recently? (avoid repetition unless for review)

**Selection Algorithm:**
```
Score(item) = w1 × PedagogicalScore + w2 × PrerequisiteScore 
              + w3 × KnowledgeFit + w4 × InterestScore 
              + w5 × GoalScore + w6 × ModalityFit 
              + w7 × ContextScore + w8 × FreshnessBonus
```

Where weights vary by situation:
- Review sessions: w8 (freshness) weighted higher
- New material: w3 (knowledge fit) weighted higher
- Assessment: w2 (prerequisite) and w1 (pedagogical) weighted higher
- Exploration: w4 (interest) weighted higher

### 4.2 Complexity Adjustment

Content complexity is adjusted along multiple dimensions:

**Vocabulary Complexity:**
- **Basic:** Common vocabulary, short sentences, simple structures (for beginners, children, language learners)
- **Standard:** Academic vocabulary, moderate sentence length (for intermediate learners)
- **Advanced:** Specialized terminology, complex structures, nuanced expression (for advanced learners)

**Conceptual Complexity:**
- **Concrete:** Tangible examples, visible relationships, single-step reasoning
- **Abstract:** Conceptual explanations, relationships among ideas, multi-step reasoning
- **Meta-conceptual:** Comparative analysis, critical evaluation, synthesis of multiple perspectives

**Structural Complexity:**
- **Simple:** One idea at a time, clear organization, explicit transitions
- **Moderate:** Multiple related ideas, clear but implicit organization
- **Complex:** Interconnected ideas, network structure, implicit relationships

**Example Selection by Complexity:**
- **Simple examples:** Common, familiar, single-variable, from everyday life
- **Moderate examples:** Somewhat unfamiliar, multi-variable, from related domains
- **Complex examples:** Unfamiliar, many variables, from specialized domains, requiring prior knowledge

### 4.3 Explanation Customization

**Explanation Style:**
- **Rule-first:** State the principle, then provide examples and applications
- **Example-first:** Present examples, then derive the principle inductively
- **Analogy-based:** Use analogy to build understanding, then formalize
- **Socratic:** Guide discovery through questioning sequence
- **Narrative:** Embed explanation within a story or scenario

**Explanation Personalization:**
- Match explanation style to learner's preferred learning approach
- Use learner's background knowledge as anchor for new explanations
- Reference previously learned concepts to build connections
- Address known misconceptions proactively

**Comparison and Contrast:**
- Compare new concept to previously learned concepts
- Contrast with common misconceptions
- Show similarities and differences across madhahib or scholarly opinions

### 4.4 Analogy Personalization

**Analogy Source Domains:**
- **Agriculture:** For learners from agricultural backgrounds
- **Business/Finance:** For professionals and business students
- **Technology:** For tech-savvy learners
- **Sports:** For athletes and sports enthusiasts
- **Family/Home:** For those with family responsibilities
- **Nature/Science:** For scientifically inclined learners

**Analogy Effectiveness Tracking:**
- Does the analogy improve comprehension? (assessed via follow-up questions)
- Does the learner reference the analogy in later explanations?
- Does the analogy cause any misconceptions? (compare with non-analogy group)

### 4.5 Content Localization

**Cultural Localization:**
- Examples use culturally familiar names, places, and scenarios
- Visual content reflects the learner's cultural context (clothing, architecture, settings)
- Scholarly references prioritize scholars from the learner's tradition
- Culturally appropriate humor and expression

**Madhhab-Specific Content:**
- Fiqh content filtered by the learner's madhhab (or presented comparatively if desired)
- Preferred scholarly authorities highlighted
- Regional practice variations acknowledged

**Language Localization:**
- Full translation into the learner's language
- Key Arabic terms preserved with translation
- Language-level adjustment for Arabic proficiency

---

## 5. Difficulty Adaptation

### 5.1 Dynamic Difficulty Adjustment System

The DDA system continuously monitors learner performance and adjusts difficulty in real time to maintain the optimal challenge zone where learning is most efficient.

### 5.2 The Optimal Challenge Window

Research in educational psychology consistently shows that learning is most efficient when the learner maintains approximately 70–85% accuracy. This zone provides enough challenge to promote growth while avoiding the demotivation of frequent failure.

**Zone Definitions:**
- **Frustration zone (accuracy < 60%):** Too difficult; learner likely to disengage, give up, or develop negative associations
- **Growth zone (accuracy 60–70%):** Challenging but manageable; high learning potential with appropriate support
- **Optimal zone (accuracy 70–85%):** Ideal balance of challenge and success; maximum learning efficiency
- **Comfort zone (accuracy 85–95%):** Easy but still engaging; review and consolidation
- **Boredom zone (accuracy > 95%):** Too easy; learner likely to disengage, lose focus

**Zone Transition Logic:**
- Learner in optimal zone for sustained period → maintain, gradually increase difficulty
- Learner approaching frustration zone → intervene with support or easier content
- Learner in boredom zone for 5+ items → increase difficulty
- Learner in frustration zone for 3+ items → decrease difficulty immediately

### 5.3 Difficulty Calibration

**Initial Calibration:**
- Diagnostic assessment determines starting difficulty
- Multi-level assessment (easy, medium, hard items) establishes baseline
- Starting difficulty set to produce ~80% accuracy on initial content

**Continuous Calibration:**
- Difficulty adjusted after each interaction (real-time)
- Moving window of last 10 responses determines current accuracy
- Accuracy trends (not single responses) drive adjustment decisions
- Difficulty changes are gradual — significant jumps are avoided

**Calibration per Concept:**
- Different concepts may have different optimal difficulty levels
- Learner's relative strengths and weaknesses affect per-concept calibration
- Concept difficulty calibration is independent — mastery of one does not change difficulty of another

### 5.4 Flow Corridor Maintenance

Flow state — the optimal state of engagement where challenge matches skill — is maintained through continuous corridor monitoring:

**Flow Signals:**
- **Loss of flow (too hard):** Errors increase, response time increases, frustration signals (hesitation, backtracking), help-seeking increases, session abandonment risk
- **Loss of flow (too easy):** Errors near zero, response time very fast, pausing/distraction behaviors, rushing through content, skipping optional material
- **In flow:** Steady accuracy (70–85%), consistent response time, natural pauses for reflection, occasional help-seeking (appropriate), session continuation past natural break points

**Flow Recovery Interventions:**
- Too hard → decrease difficulty, increase support, switch modality, insert reminder/review
- Too easy → increase difficulty, reduce support, add enrichment, challenge with synthesis tasks
- In flow → maintain current trajectory, small gradual difficulty increases

### 5.5 Frustration Detection and Management

**Frustration Signals:**
- Rapid error rate increase (3+ consecutive errors, or 80%+ errors in last 5 items)
- Response time increases significantly (> 200% of baseline)
- Cursor/mouse agitation patterns (hovering, rapid random movement)
- Repeated hint requests without attempting between hints
- Negative feedback actions (backing out, closing, switching away)
- Session abandonment after difficult item

**Frustration Interventions:**
- Immediate difficulty reduction (1–2 levels)
- Support increase (more hints, worked examples)
- Modality switch (to easier or more engaging modality)
- Encouragement message with attributional feedback
- Offer a break or switch to easier content
- Present a "safety net" item (very easy success experience)
- Direct intervention: "It seems like this is challenging. Would you like to try a different approach?"

### 5.6 Difficulty Scale

The Dien platform uses a 10-level difficulty scale:

| Level | Label | Description | Accuracy Target |
|-------|-------|-------------|-----------------|
| 1 | Foundational | Basic recognition, core vocabulary, simple definitions | 90-95% |
| 2 | Introductory | Simple concepts, guided examples, structured practice | 85-90% |
| 3 | Elementary | Core concepts, standard examples, moderate guidance | 80-85% |
| 4 | Developing | Extended concepts, multiple examples, less guidance | 75-80% |
| 5 | Intermediate | Complex concepts, few examples, minimal guidance | 70-75% |
| 6 | Proficient | Advanced concepts, application required, no guidance | 65-70% |
| 7 | Advanced | Multi-step reasoning, synthesis, evaluation | 60-65% |
| 8 | Expert | Original analysis, critical evaluation, scholarly comparison | 55-60% |
| 9 | Master | Independent research, novel application, teaching others | 50-55% |
| 10 | Scholar | Contribution to field, original synthesis across disciplines | Below 50% |

---

## 6. Pacing Adaptation

### 6.1 Speed Adjustment

The ALE adjusts the pace of content delivery to match the learner's cognitive processing speed, ensuring that content is neither rushed (causing incomplete processing) nor dragged (causing boredom).

**Reading Speed Calibration:**
- Initial reading speed estimated from diagnostic (timed reading of sample text)
- Speed adjusted based on comprehension checks
- Text density (amount per screen) adjusted to match comfortable reading chunk size
- Option for rapid serial visual presentation (RSVP) for fast readers

**Audio Speed Calibration:**
- Default audio speed set to 1.0x for new learners
- Learner can adjust manually (0.5x–2.0x)
- System observes comprehension at different speeds and may recommend optimal speed
- Speed changes preserve pitch to avoid comprehension degradation

**Processing Time Allocation:**
- Content segments assigned minimum and maximum time based on complexity
- Learner-paced segments (no time limit) available for difficult content
- System-paced segments with adjustable time limits for time-efficient learning
- Auto-advance vs. manual advance based on learner preference and context

### 6.2 Time Allocation

**Per-Session Time Budgeting:**
- Learner specifies available time at session start (or system infers from schedule)
- ALE allocates time across planned activities: review, new content, practice, reflection
- Prioritization: essential content gets time first, enrichment gets remaining time
- Time warnings at 5-minute and 1-minute before session end

**Per-Concept Time Allocation:**
- More time to concepts the learner finds challenging
- Less time to concepts the learner masters quickly
- Time allocation adjusted dynamically based on in-session performance
- Minimum time enforced for essential content (no skipping)

**Interleaved vs. Blocked Time:**
- Blocked practice: All time allocated to one topic — for initial learning
- Interleaved practice: Time split across multiple topics — for discrimination and retention
- Adaptive interleaving: System determines optimal interleaving pattern based on learning objectives

### 6.3 Session Length Optimization

**Optimal Session Duration:**
- Determined by analyzing learner's historical engagement curves
- Typical optimal range: 20–50 minutes for focused learning
- Extended sessions (60–90 min) possible with breaks
- Micro-sessions (5–15 min) for spaced practice and review

**Session Length Recommendations:**
- Based on current learning load, fatigue level, and available time
- Shorter sessions recommended for difficult new content
- Longer sessions for review and practice of familiar content
- Flexibility: learner can always extend or shorten

**Session Termination:**
- Natural termination: At a logical break point (topic completion, assessment end)
- Time-based termination: When allocated time expires
- Engagement-based termination: When engagement signals indicate fatigue
- Learner-initiated termination: At any point with progress saved

### 6.4 Break Scheduling

**Break Timing:**
- Breaks suggested every 25–45 minutes based on cognitive load research
- Break timing personalized: Some learners benefit from shorter, more frequent breaks
- Break triggered by fatigue indicators even if within normal session duration
- Post-break content selected to ease re-entry (review before new content)

**Break Activities:**
- Physical: Stretching, walking, wudu (if applicable)
- Mental: Short dhikr, du'a, deep breathing
- Environmental: Change of scene, hydration, eye rest
- Optional: No activity, just rest

**Break Discipline:**
- System enforces minimum break duration (3 minutes)
- System allows break extension if learner chooses
- During breaks, learning activity is blocked to ensure genuine rest
- Post-break learning readiness check (optional)

### 6.5 Content Density Control

Content density — the amount of information presented per unit time — is adjusted to match the learner's cognitive capacity:

**Density Levels:**
| Level | Content per 10 min | Time per Concept | Modality Intensity |
|-------|-------------------|------------------|-------------------|
| Sparse | 1 concept, 2–3 examples | 8–12 min | Single modality |
| Standard | 1–2 concepts, 3–4 examples | 5–8 min | 1–2 modalities |
| Dense | 2–3 concepts, 4–5 examples | 3–5 min | 2–3 modalities |
| Intensive | 3+ concepts, 5+ examples | 2–3 min | Multiple modalities |

**Density Adjustment Signals:**
- Accuracy dropping → reduce density
- Response time increasing → reduce density
- Session engagement high → maintain or increase density
- Learner requests more depth → increase density of examples and explanations
- Context: commute/low-attention → lower density; dedicated study → higher density

---

## 7. Sequencing Adaptation

### 7.1 Path Personalization

The adaptive learning engine does not treat the curriculum as a fixed linear sequence. Instead, it views the curriculum as a rich graph of interconnected concepts, each with multiple possible learning paths. The ALE selects and dynamically adjusts the path that best suits each learner.

**Curriculum Graph Structure:**
- **Nodes:** Individual concepts, topics, skills, or learning objectives
- **Edges:** Prerequisite relationships (must know A before B), enrichment relationships (B is enriched by knowing C), and correlation relationships (A and B are often learned better together)
- **Entry points:** Multiple possible starting points depending on prior knowledge
- **Exit points:** Mastery of terminal learning objectives

**Path Types:**
- **Prerequisite-ordered path:** Follows prerequisite relationships strictly — safest path, ensures no gaps
- **Spiral path:** Revisits concepts at increasing depth — good for retention and deepening understanding
- **Interest-ordered path:** Follows learner's interest, with prerequisites inserted as needed
- **Mastery path:** Moves quickly through known content, slows for new content
- **Review path:** Revisits content based on forgetting curve predictions

### 7.2 Prerequisite Adjustment

**Prerequisite Verification:**
- Before introducing a new concept, verify mastery of prerequisites
- Prerequisite mastery can be demonstrated through: prior assessment, prior completion, transfer task
- If prerequisite mastery is uncertain, administer short diagnostic

**Prerequisite Remediation:**
- If prerequisite gap is detected, insert prerequisite content
- Remediation is targeted and efficient — only the specific prerequisite knowledge needed
- Remediation modality may differ from original presentation for variety

**Prerequisite Skipping:**
- If learner demonstrates mastery of advanced concept without formal prerequisites (via transfer from other domains), prerequisites may be skipped
- Prerequisites are not skipped for learners who show incomplete understanding

### 7.3 Skipping and Enrichment

**Mastery Skip:**
- Learner can demonstrate mastery of a topic and skip its instruction
- Mastery demonstrated via: short assessment, transfer task, or verified prior completion
- Skipped content still appears in review rotation

**Interest Skip:**
- Learner can skip topics that are not of interest
- System warns if skipped content is prerequisite for desired future content
- Skipped topics remain available for later if interest changes

**Enrichment Paths:**
- Learners who master core content quickly are offered enrichment
- Enrichment includes: deeper exploration, related topics, scholarly debates, practical application
- Enrichment is optional but encouraged for mastery-oriented learners

**Interest-Based Detours:**
- When learner shows heightened interest (dwell time, follow-up questions, exploration), offer detour into related content
- Detour is bounded: maximum depth and duration specified
- Return path: clear route from detour back to main path

### 7.4 Remediation Insertion

**Remediation Strategy:**
- **Diagnostic:** Identify specific knowledge gaps (not just "didn't get it")
- **Targeted:** Remediate only the specific gap, avoid full content repetition
- **Multi-modal:** Remediation in a different modality than original instruction
- **Alternative approach:** Teach the same concept using a different method or analogy
- **Success-oriented:** Remediation items designed to build confidence

**Remediation Types:**
- **Re-explain:** Same explanation, different words
- **Re-exemplify:** Same concept, different examples
- **Re-present:** Same content, different modality
- **Break-down:** Decompose into smaller sub-skills
- **Build-up:** Start from prerequisite and rebuild

**Remediation Loop:**
1. Assessment reveals gap
2. First remediation approach attempted
3. Re-assessment on the specific gap
4. If gap persists, try different approach
5. After 3 failed approaches, flag for human teacher intervention
6. After successful remediation, return to main path

### 7.5 Sequencing for Retention

The ALE uses principles from the science of memory to optimize content sequencing:

**Spacing Effect:**
- Content is spaced across multiple sessions rather than massed
- Optimal spacing intervals determined by the forgetting curve
- Review items are interleaved with new content at increasing intervals

**Interleaving:**
- Related concepts are interleaved rather than blocked
- Interleaving improves discrimination between similar concepts
- Interleaving ratio (new:review) adapts based on retention

**Desirable Difficulties:**
- Introduce challenges at optimal difficulty (see section 5)
- Vary practice conditions (different contexts, different modalities)
- Test before teaching (generation effect) for some content

**Contextual Variety:**
- Same content practiced in different contexts
- Contextual variety improves transfer
- Contexts drawn from learner's real-life situations

---

## 8. Scaffolding Adaptation

### 8.1 The Scaffolding Fading Model

Scaffolding follows a structured fading model where support is gradually removed as the learner gains competence:

**Scaffolding Phases:**

**Phase 1 — Full Scaffolding:**
- Content: Fully worked examples with detailed annotations
- Practice: Step-by-step guided practice with immediate feedback
- Instructions: Explicit, detailed, with rationale
- Hints: Available for every step
- Feedback: Immediate, explanatory, identifies specific errors
- Learner role: Follow-along, observe, practice with guidance

**Phase 2 — Medium Scaffolding:**
- Content: Partially worked examples with some steps for learner to complete
- Practice: Guided practice with reduced step-by-step support
- Instructions: Brief, with key points only
- Hints: Available but progressively less detailed
- Feedback: Delayed slightly, focuses on process rather than product
- Learner role: Active participation, partial independence

**Phase 3 — Light Scaffolding:**
- Content: Problems with solution verification (check your work)
- Practice: Independent practice with periodic check-ins
- Instructions: Minimal, scenario-based
- Hints: Limited, only on request
- Feedback: End-of-practice summary, focuses on patterns
- Learner role: Independent practitioner

**Phase 4 — No Scaffolding:**
- Content: Application scenarios, real-world problems
- Practice: Fully independent practice
- Instructions: Goal-oriented only
- Hints: Not available (or significantly restricted)
- Feedback: Self-verification guidelines provided
- Learner role: Autonomous learner

### 8.2 Fading Triggers

The transition from each phase to the next is triggered by demonstrated readiness:

**Forward Fading Triggers (move to less support):**
- Sustained accuracy > 85% at current phase
- Decreasing hint usage across items
- Decreasing response time while maintaining accuracy
- Successful transfer to novel contexts
- Learner self-assessment of readiness (if accurate)

**Backward Fading Triggers (return to more support):**
- Sustained accuracy < 65% at current phase
- Increasing hint usage across items
- Increasing response time with decreasing accuracy
- Failure on transfer tasks
- Learner request for more support

### 8.3 Hint Progression System

The hint system provides a structured progression from minimal to maximal support:

**Hint Levels:**
| Level | Type | Content | Example |
|-------|------|---------|---------|
| 1 | Orient | Directs attention, orients thinking | "Consider the concept of ihsan" |
| 2 | Recall | Activates relevant knowledge | "Remember that ihsan means to worship Allah as if you see Him" |
| 3 | Rule | States the relevant principle | "The principle is: the higher the level of awareness, the greater the reward" |
| 4 | Guide | Partial solution or direction | "Think about how this applies to the three levels: Islam, Iman, Ihsan" |
| 5 | Almost | Near-complete solution | "The answer relates to Ihsan being the highest level because..." |
| 6 | Answer | Complete solution with explanation | Shows the complete answer and why it is correct |

**Hint Adaptation:**
- Initial hint level depends on learner's knowledge state (novices start at Level 1, experts may start at Level 3)
- If learner repeatedly needs Level 5+ hints, system increases scaffolding in next content
- If learner consistently answers after Level 1–2 hints, system may skip lower levels next time

### 8.4 Worked Example Fading

Worked example fading moves from completed examples to independent problem-solving:

**Fading Sequence:**
1. **Study:** Learner studies a fully worked example
2. **Explain:** Learner explains the worked example (self-explanation)
3. **Complete:** Learner completes a partial example (50% steps provided)
4. **Practice:** Learner solves parallel problem independently
5. **Transfer:** Learner applies concept to novel situation

**Example Types:**
- **Full example:** Problem → Step 1 → Step 2 → Step 3 → Solution
- **Completion 1:** Problem → Step 1 → Step 2 → _____ → Learner completes Step 3
- **Completion 2:** Problem → Step 1 → _____ → _____ → Learner completes Steps 2–3
- **Independent:** Problem → Learner completes all steps
- **Transfer:** Novel problem → Learner recognizes applicability and solves

### 8.5 Autonomy Graduation

Beyond immediate content mastery, scaffolding fading serves a broader purpose: graduating the learner to autonomous learning.

**Autonomy Development Stages:**
| Stage | Learner Behavior | System Role |
|-------|-----------------|-------------|
| Dependent | Follows provided path, uses available support | Direct instruction, full scaffolding |
| Supported | Makes choices within options, uses support selectively | Guided path, moderate scaffolding |
| Independent | Sets own path, manages own learning | Resource provider, light monitoring |
| Self-directed | Identifies learning needs, seeks resources autonomously | Consultant, reference |
| Mentor | Teaches others, contributes to learning community | Platform, amplifier |

**Autonomy Indicators:**
- Frequency of independent learning choices
- Accuracy of self-assessment (calibration)
- Effectiveness of self-directed learning
- Quality of explanations given to others
- Initiation of learning outside prescribed path

---

## 9. Feedback Adaptation

### 9.1 Real-Time Feedback Decisions

The ALE makes feedback decisions for each learner response, determining when to give feedback, what type of feedback to give, how detailed to be, and what tone to use.

### 9.2 Feedback Timing Models

**Immediate Feedback:**
- Best for: Procedural skills, factual recall, automated processes
- Effect: Prevents error rehearsal, provides immediate correction
- When: Early learning, error-prone content, struggling learners
- Implementation: After each response, before next item

**Delayed Feedback:**
- Best for: Conceptual understanding, problem-solving, transfer
- Effect: Promotes error detection and correction, enhances retention
- When: Later learning, confident learners, complex tasks
- Implementation: End of set, end of session, or next session

**Adaptive Timing:**
- Correct responses: Delay feedback (strengthens retrieval practice effect)
- Incorrect responses: Immediate feedback (prevents error rehearsal)
- Uncertain responses: Prompt self-assessment before feedback
- Very fast incorrect: Immediate feedback with attention check
- Very slow correct: Positive feedback with efficiency encouragement

### 9.3 Feedback Type Selection

**Directive Feedback:**
- Tells the learner what to do differently
- Best for: Procedural tasks, clear right/wrong answers, novices
- Example: "The correct answer is [X]. Here is why..."

**Facilitative Feedback:**
- Guides the learner to discover the correct approach
- Best for: Conceptual understanding, problem-solving, advanced learners
- Example: "Consider how this relates to the principle of [Y]. What changes?"

**Verification Feedback:**
- Simply indicates correct/incorrect
- Best for: Confidence checks, quick practice, self-assessment
- Example: "✓ Correct" or "✗ Incorrect"

**Elaborated Feedback:**
- Provides explanation, related concepts, common errors
- Best for: Deep learning, error analysis, knowledge construction
- Example: "Your answer was [X]. The correct answer is [Y] because..."

**Metacognitive Feedback:**
- Prompts reflection on learning process
- Best for: Developing self-regulated learning, advanced learners
- Example: "How confident are you in your answer? What strategy did you use?"

### 9.4 Feedback Depth Selection

| Level | Information Provided | When Used |
|-------|---------------------|-----------|
| Level 0 | No feedback | Assessment conditions, self-check |
| Level 1 | Correct/Incorrect only | Quick practice, warm-up |
| Level 2 | Correct answer shown | Busy context, review mode |
| Level 3 | Correct answer + brief explanation | Standard learning |
| Level 4 | Correct answer + detailed explanation | Difficult concepts, errors |
| Level 5 | Full diagnostic: what, why, how, related concepts, connections | Deep learning, error analysis |
| Level 6 | Comparative feedback: your approach vs. expert approach | Expert coaching |

**Depth Adaptation:**
- Learner struggling → deeper feedback (Level 4–5)
- Learner succeeding → shallower feedback (Level 1–2)
- Complex concept → deeper feedback
- Simple recall → shallow feedback
- Error related to known misconception → targeted explanation
- Random error (not systematic) → shallow feedback

### 9.5 Feedback Tone Adaptation

**Tone Dimensions:**
- **Supportive ↔ Directive:** How much guidance vs. how much autonomy
- **Warm ↔ Formal:** Personal connection vs. professional distance
- **Encouraging ↔ Challenging:** Building confidence vs. raising standards
- **Simple ↔ Elaborate:** Concise feedback vs. detailed explanation

**Tone Selection by Learner State:**
| Learner State | Recommended Tone | Example |
|--------------|-----------------|---------|
| Frustrated | Warm, encouraging, supportive | "I can see this is challenging. You've learned similar concepts well before. Let's try a different approach." |
| Bored | Challenging, concise, direct | "Ready for something more interesting? Try solving this using a different method." |
| Confident | Direct, brief, challenging | "Correct. Now, can you apply this to a more complex situation?" |
| Anxious | Warm, supportive, structuring | "There's no pressure here. Let's work through this step by step together." |
| Curious | Elaborative, engaging, open | "Great question! This actually connects to a fascinating topic. Would you like to explore it?" |
| Overconfident | Challenging, metacognitive | "Are you sure? Test your understanding by explaining the reasoning behind your answer." |

---

## 10. Recommendation Adaptation

### 10.1 Real-Time Content Scoring

Every content item available to the learner is scored in real time based on multiple criteria:

**Scoring Criteria:**
- **Utility:** How much this content will advance the learner toward their current goal
- **Readiness:** Whether prerequisites are met and the learner is ready for this content
- **Urgency:** How time-sensitive this content is (e.g., Ramadan-specific content during Ramadan)
- **Novelty:** Whether this content is new or a review
- **Receptivity:** Whether the learner is in a good state to receive this content
- **Diversity:** How this content diversifies the learner's recent experience

**Scoring Formula:**
```
RecommendationScore = α × UtilityScore + β × ReadinessScore 
                      + γ × UrgencyScore + δ × NoveltyScore 
                      + ε × ReceptivityScore + ζ × DiversityBonus
```

### 10.2 Multi-Objective Optimization

The ALE balances competing objectives when making recommendations:

**Primary Objectives:**
- Learning efficiency (maximize knowledge gain per unit time)
- Retention (maximize long-term memory)
- Engagement (maximize sustained participation)
- Satisfaction (maximize learner enjoyment and confidence)

**Secondary Objectives:**
- Coverage (ensure all required content is eventually covered)
- Review (maintain previously learned content)
- Exploration (encourage discovery of new interests)
- Autonomy (respect learner choices and preferences)

**Objective Weighting:**
| Phase | Primary Objective | Weight |
|-------|------------------|--------|
| New material | Learning efficiency | 0.5 |
| Review session | Retention | 0.6 |
| Remediation | Learning efficiency | 0.7 |
| Exploration | Engagement | 0.5 |
| Pre-exam preparation | Retention + Coverage | 0.4 each |

### 10.3 Exploration-Exploitation Balance

The ALE must balance recommending content that is known to be effective for the learner (exploitation) with trying new approaches that may be even better (exploration):

**Exploitation:** Recommend content and approaches that have worked well for this learner in the past
**Exploration:** Try new content, modalities, or approaches that may prove more effective

**Balance Mechanism — Epsilon-Greedy:**
- 80% of recommendations: Exploit (choose known-effective option)
- 20% of recommendations: Explore (try new options)
- Epsilon decreases as learner data accumulates (less need for exploration over time)
- Epsilon increases when learner performance plateaus (need new approach)

**Context-Sensitive Exploration:**
- New learner: Higher exploration rate (40%) to build profile
- Established learner: Lower exploration rate (10–15%)
- Struggling learner: Higher exploration (try anything that might help)
- Thriving learner: Lower exploration (don't fix what's working)
- New content type: Higher exploration to discover modality/approach fit

### 10.4 Diversity Enforcement

The ALE actively maintains diversity in recommendations to prevent monotony and ensure broad coverage:

**Diversity Dimensions:**
- **Content type:** Mix of concepts, procedures, stories, practices
- **Topic:** Different topics within the curriculum
- **Difficulty:** Mix of easy, medium, and challenging content
- **Modality:** Different sensory channels
- **Activity type:** Mix of reading, listening, watching, doing
- **Task demand:** Mix of recall, comprehension, application, analysis

**Diversity Rules:**
- No more than 3 consecutive items from the same content type
- No more than 2 consecutive items at the same difficulty level
- At least 1 review item for every 3 new items
- At least 1 different modality every 5 interactions
- All major topics reviewed within each week

### 10.5 Session Composition

Each session's content is composed from multiple sources to create a balanced learning experience:

**Session Components:**
- **Review (20%):** Previously learned content due for review (spaced repetition)
- **New content (60%):** Primary learning objectives for the session
- **Practice (10%):** Application and assessment of learned content
- **Reflection (5%):** Metacognitive prompts, journaling, self-assessment
- **Enrichment (5%):** Optional additional content, interesting connections

**Session Flow:**
1. **Warm-up (2–3 min):** Quick review of previous session's content
2. **Main session (15–40 min):** New content delivery and practice
3. **Checkpoint (3–5 min):** Brief assessment of session content
4. **Cooldown (2–3 min):** Summary, reflection, preview of next session

---

## 11. Adaptation Triggers

### 11.1 Trigger Classification

Adaptation triggers are classified events or conditions that cause the ALE to reassess and potentially change its adaptive settings. Triggers range from individual learner actions to complex pattern detections.

### 11.2 Correct/Incorrect Triggers

**Single Correct Response:**
- Effect: Slight increase in concept mastery, slight increase in difficulty for next item
- Threshold: Each individual correct answer triggers this
- Actions: Update knowledge state, check if next item should be harder

**Correct Streak (3+):**
- Effect: Significant increase in mastery, difficulty increase likely
- Threshold: 3 consecutive correct answers
- Actions: Increase difficulty by 1 level, reduce hint availability, consider enrichment

**Correct Streak (5+):**
- Effect: Consider mastery acceleration
- Threshold: 5 consecutive correct answers
- Actions: Offer skip-ahead to more advanced content, increase interleaving

**Single Incorrect Response:**
- Effect: Slight decrease in mastery, increase confusion measure
- Threshold: Each individual incorrect answer triggers this
- Actions: Provide corrective feedback, check for systematic error pattern

**Incorrect Streak (2):**
- Effect: Moderate concern, support increase
- Threshold: 2 consecutive incorrect
- Actions: Decrease difficulty by 1 level, increase hint availability, add scaffolding

**Incorrect Streak (3+):**
- Effect: Significant concern, intervention required
- Threshold: 3 consecutive incorrect
- Actions: Decrease difficulty significantly, switch modality, insert remediation, offer break

### 11.3 Response Time Triggers

**Fast Correct Answer (< 30% of baseline):**
- Interpretation: May indicate rote memorization or lucky guess
- Effect: Concept marked as "recalled" but not necessarily "understood"
- Actions: Follow up with application-level question on same concept

**Fast Incorrect Answer (< 30% of baseline):**
- Interpretation: Careless error, impulsivity, or guessing
- Effect: Reduced confidence in knowledge estimate
- Actions: Attention cue, encouragement to slow down, retry with same difficulty

**Slow Correct Answer (> 200% of baseline):**
- Interpretation: Effortful retrieval, knowledge present but weak
- Effect: Concept marked as "learning" (not "learned")
- Actions: Schedule review with shorter interval, provide fluency practice

**Slow Incorrect Answer (> 200% of baseline):**
- Interpretation: Knowledge gap or confusion, learner trying hard but lacking understanding
- Effect: Significant gap indicator
- Actions: Decrease difficulty, provide worked example, consider prerequisite gap

**Sudden Response Time Change (40%+ from trend):**
- Interpretation: State change — fatigue, distraction, or emotional response
- Effect: Context state updated
- Actions: Check for overall pattern, consider pace or modality change

### 11.4 Confidence Level Triggers

If the system includes self-assessed confidence (e.g., "How confident are you in your answer?")

**High Confidence + Correct:**
- Interpretation: Knowledge is strong and learner is aware of it
- Effect: Accelerate mastery progression
- Actions: Increase difficulty, move to application

**High Confidence + Incorrect:**
- Interpretation: Misconception or overconfidence
- Effect: Flag for misconception analysis
- Actions: Targeted correction with explanation of the misconception, calibration feedback

**Low Confidence + Correct:**
- Interpretation: Knowledge present but learner lacks confidence
- Effect: Need confidence building
- Actions: Positive attributional feedback, easier items for success, reduce difficulty temporarily

**Low Confidence + Incorrect:**
- Interpretation: Knowledge gap, learner is aware of uncertainty
- Effect: Standard learning gap
- Actions: Teach, provide scaffolding, build from known to unknown

### 11.5 Engagement Signal Triggers

**Dwelling (extended pause on content):**
- Interpretation: Deep processing or confusion
- Distinction: Deep processing (followed by correct response) vs. confusion (followed by error)
- Actions: Check for understanding, provide option for help or clarification

**Skipping (rapidly advancing through content):**
- Interpretation: Boredom (known content) or rushing (time pressure)
- Distinction: Boredom (skipping familiar content) vs. rushing (skipping all content)
- Actions: Bored — increase difficulty; Rushing — suggest time management, adjust session length

**Rereading (returning to previous content):**
- Interpretation: Confusion or review seeking
- Actions: Check understanding, offer summary, suggest bookmarking for later review

**Annotation Activity:**
- Interpretation: Active engagement, deep processing
- Effect: Positive engagement signal
- Actions: Maintain current trajectory, optionally suggest related content based on annotation topic

**External Navigation (leaving the learning platform):**
- Interpretation: Distraction, break, or disengagement
- Effect: Session context update
- Actions: Save progress, upon return, provide re-entry summary

### 11.6 Session Context Triggers

**Session Start:**
- Effect: Initialize session context, set learning goals for session
- Actions: Present daily plan, review previous session, set session objectives

**Session End:**
- Effect: Finalize session data, update long-term models
- Actions: Save progress, generate summary, preview next session, schedule review

**Session Resume (after break):**
- Effect: Reactivate prior context
- Actions: Brief review of pre-break content, check retention of pre-break learning

**Content Completion:**
- Effect: Update curriculum progress, check for new available content
- Actions: Assess mastery, provide consolidation, recommend next content

**Topic Transition:**
- Effect: Shift learning objective, possibly shift modality
- Actions: Summarize completed topic, preview next topic, assess readiness

### 11.7 Time of Day and Fatigue Triggers

**Time-Based Fatigue Detection:**
- Late night (after typical bedtime): Reduce cognitive load requirements
- Post-meal: Reduce interactive/kinesthetic demands, favor passive modalities
- Early morning: Higher cognitive capacity, favor challenging content

**Performance-Based Fatigue Detection:**
- Accuracy decline > 15% from session baseline
- Response time increase > 50% from session baseline
- Error rate increasing in later part of session
- Hints usage increasing
- Session duration exceeding typical maximum (adjust per learner)

**Fatigue Interventions:**
- Suggest break (3–15 minutes)
- Reduce content density
- Switch to easier modality (passive audio or visual)
- Offer review content instead of new learning
- Recommend ending session if fatigue is significant

### 11.8 Trigger Priority and Conflict Resolution

When multiple triggers fire simultaneously, the ALE prioritizes:

| Priority | Trigger Category | Rationale |
|----------|------------------|-----------|
| 1 (Highest) | Learner safety/wellbeing | Fatigue, frustration, distress |
| 2 | Error remediation | Incorrect streak, knowledge gaps |
| 3 | Engagement crisis | Boredom, disengagement, skipping |
| 4 | Performance optimization | Adjusting difficulty for flow |
| 5 | Learning efficiency | Adaptive recommendations |
| 6 | Personalization | Preference-based adjustments |
| 7 (Lowest) | Enhancement | Enrichment, exploration offers |

---

## 12. Batch Adaptation — Daily

### 12.1 Daily Plan Generation

At the start of each day (or at the learner's first session of the day), the ALE generates a personalized daily learning plan:

**Plan Components:**
- **Review items:** Content due for spaced repetition review
- **New content:** Primary learning objectives for the day
- **Practice:** Scheduled practice activities
- **Assessment:** Any scheduled quizzes or assessments
- **Reflection:** Time allocated for reflection and journaling
- **Flex time:** Unallocated time for learner-directed learning

**Plan Generation Algorithm:**
```
DailyPlan = Optimize(
    ReviewItems: items due based on forgetting curve
    + NewContent: next content in priority order
    + PracticeItems: items needing more practice
    + AssessmentItems: scheduled assessments
    Subject to:
        TotalTime <= DailyTimeBudget
        ContentDiversity >= MinimumDiversity
        DifficultyVariation := AppropriateMixture
        LearnerPreferences := Respect
)
```

**Plan Presentation:**
- Structured timeline with time estimates for each activity
- Priority indicators (essential vs. optional)
- Progress tracking (check off completed items)
- Flexibility for learner-driven modifications

### 12.2 Review Scheduling

The daily review schedule is determined by the Memory Engine:

**Spaced Repetition Algorithm:**
- Each mastered concept has an optimal review interval
- Intervals follow the forgetting curve (expanding intervals)
- Review items are prioritized by: closest to forgetting point, highest difficulty, longest since review

**Daily Review Load:**
- Target: 15–25% of session time spent on review
- Review items drawn from all previously learned content
- Maximum review items per session: 10–15 (to prevent overload)

**Review Format Diversity:**
- Not all reviews are identical — format varies:
  - Quick recall (flashcard-style)
  - Application questions
  - Explanation prompts
  - Connection tasks (relate to other concepts)

### 12.3 Goal Adjustment

Daily goals are adjusted based on progress toward weekly and long-term goals:

**Goal Monitoring:**
- Compare actual progress vs. planned progress
- Identify off-track areas
- Adjust daily targets accordingly

**Goal Adjustment Rules:**
- If ahead of schedule → add enrichment content
- If behind schedule → prioritize essential content, delay optional content
- If consistently off → reassess goal feasibility and adjust

### 12.4 Curriculum Pacing

Daily pacing ensures the curriculum is covered at a sustainable rate:

**Pacing Calculation:**
- Remaining content ÷ available days (with buffer for review and reflection)
- Adjust pace for difficulty: harder topics get more days
- Adjust for learner's demonstrated learning rate

**Pacing Adjustments:**
- Learner ahead → slow down for deepening OR accelerate to enrichment
- Learner behind → speed up by reducing enrichment, focusing on essentials
- Learner on track → maintain current pace

### 12.5 Content Pre-Generation

To ensure smooth offline and low-latency experiences, the ALE pre-generates content for the expected daily plan:

**Pre-Generation Scope:**
- Next day's content (text, audio, visual assets)
- Review items and their varied formats
- Assessment items for planned quizzes
- Alternative modality versions

**Pre-Generation Trigger:**
- End of previous day's session
- During background processing (when device is charging and connected to WiFi)
- When next day's plan is finalized (or predicted with high confidence)

**Pre-Generation Caching:**
- Generated content cached locally on device
- Cache refreshed when plan changes significantly
- Fallback to server-streamed content if cache is insufficient

---

## 13. Long-Term Adaptation — Weekly and Monthly

### 13.1 Weekly Review Cycle

Every week, the ALE conducts a comprehensive review of the learner's progress and adjusts the learning strategy:

**Weekly Analysis:**
- Knowledge growth rate (concepts mastered per week)
- Retention rate (review accuracy)
- Engagement patterns (study consistency, session quality)
- Difficulty calibration accuracy
- Modality effectiveness
- Time utilization efficiency

**Weekly Adjustments:**
- Recalibrate difficulty baselines
- Update modality preference profiles
- Adjust daily time budgets
- Modify weekly goal targets
- Recommend schedule adjustments

### 13.2 Goal Reassessment

Long-term goals are reassessed at regular intervals:

**Monthly Goal Review:**
- Progress toward quarterly and annual goals
- Goal relevance (are these still the right goals?)
- Goal feasibility (can these goals be achieved in the remaining time?)
- Goal adjustment recommendations

**Goal Types and Review Frequency:**
| Goal Type | Example | Review Frequency | Adjustment Method |
|-----------|---------|------------------|-------------------|
| Daily habit | Study 30 min daily | Weekly | Incremental adjustment |
| Course completion | Complete Tafseer course | Monthly | Pacing adjustment |
| Memorization target | Memorize Surah al-Baqarah | Monthly | Strategy change |
| Mastery goal | Achieve fiqh proficiency | Quarterly | Path change |
| Life goal | Become a community teacher | Annually | Major reassessment |

**Goal Calibration:**
- Compare self-assessed progress with system-assessed progress
- Identify calibration gaps (over- or under-estimation)
- Provide calibration feedback to improve self-assessment accuracy

### 13.3 Learning Path Adjustment

Long-term learning paths are adjusted based on accumulated data:

**Path Evaluation:**
- Is the current path producing expected learning gains?
- Are there persistent difficulties or blocks?
- Has the learner's interest or focus shifted?
- Are there new available paths or resources?

**Path Adjustment Types:**
- **Acceleration:** Learner is ready for more advanced content sooner
- **Deepening:** Learner benefits from deeper exploration of current topics
- **Realignment:** Learner's interests have shifted — adjust path accordingly
- **Remediation:** Persistent gaps require path modification to include remediation
- **Enrichment:** Learner seeks additional depth beyond standard path

### 13.4 Strategy Effectiveness Update

The ALE tracks which learning strategies are most effective for each learner and updates strategy weights:

**Tracked Strategies:**
- Blocked vs. interleaved practice
- Massed vs. spaced study
- Test-first vs. study-first
- Worked example vs. problem-solving
- Single modality vs. multi-modal
- Immediate vs. delayed feedback

**Effectiveness Measurement:**
- Learning rate (concepts mastered per hour) under each strategy
- Retention rate (accuracy on delayed reviews) under each strategy
- Transfer success (application to novel contexts) under each strategy
- Learner satisfaction rating under each strategy

**Strategy Weight Updates:**
- Strategies with higher learning rates get higher weight in future planning
- Strategies with low effectiveness are reduced or eliminated
- New strategies are periodically introduced for exploration

### 13.5 Model Recalibration

Over time, the learner model may drift from the learner's actual state. Regular recalibration ensures accuracy:

**Recalibration Triggers:**
- Weekly: Full model review
- Monthly: Comprehensive recalibration
- After major milestone (exam completion, course completion)
- When prediction accuracy falls below threshold (predicted vs. actual performance mismatch > 15%)

**Recalibration Methods:**
- **Re-baseline assessment:** Brief comprehensive assessment to verify knowledge state
- **Model parameter update:** Adjust BKT/DKT model parameters to better fit recent data
- **Profile update:** Update preference and behavioral profiles
- **New learner identity check:** Detect if learning patterns have shifted significantly

**Calibration Assessment Design:**
- Brief (5–10 minutes) to minimize disruption
- Covers representative sample of all recent content
- Includes both recall and application items
- Calibrated to learner's expected level

---

## 14. Group Adaptation

### 14.1 Group Learning Dynamics

The ALE extends adaptation to group learning contexts, where multiple learners study together with shared or coordinated experiences.

### 14.2 Study Group Recommendation

The ALE recommends optimal study group compositions:

**Group Formation Criteria:**
- **Compatible knowledge levels:** Similar enough to work together, different enough to benefit from peer teaching
- **Complementary strengths:** Each member strong in different areas
- **Compatible schedules:** Available at overlapping times
- **Compatible goals:** Pursuing similar or complementary learning objectives
- **Social compatibility:** Low conflict risk, comfortable interaction

**Group Types:**
- **Peer learning pairs (2 learners):** Best for focused study and accountability
- **Study circles (3–5 learners):** Traditional Islamic learning group, good for discussion and shared learning
- **Tutorial groups (1 teacher + 3–8 learners):** Guided group learning with a teacher or advanced peer
- **Discussion forums (10+):** Asynchronous discussion on specific topics

### 14.3 Peer Matching Algorithm

```
MatchScore(learnerA, learnerB) = w1 × KnowledgeComplementarity
                                 + w2 × ScheduleOverlap
                                 + w3 × GoalAlignment
                                 + w4 × ModalityCompatibility
                                 + w5 × PersonalityCompatibility
                                 + w6 × LanguageCompatibility
                                 - w7 × ConflictHistory
```

High match scores result in peer learning recommendations.

### 14.4 Collaborative Difficulty

Group activities have their own difficulty adaptation:

**Group Difficulty Factors:**
- Individual member abilities (weakest member often determines baseline)
- Group dynamics (cooperative groups can handle harder content)
- Task type (discussion vs. collaborative problem-solving vs. peer teaching)

**Group Difficulty Adjustment:**
- Content difficulty set to challenge the median member
- Scaffolding provided for weaker members within group context
- Extension tasks for stronger members
- Group difficulty can be dynamically adjusted based on group performance

### 14.5 Group Pacing

Group learning requires synchronized pacing:

**Group Pacing Mechanisms:**
- **Leader-paced:** Most advanced or designated leader sets pace
- **Consensus-paced:** Group votes or discusses pace
- **Adaptive group pacing:** System monitors group progress and adjusts group pace
- **Individual-within-group:** Core content at group pace, individual enrichment/remediation at individual pace

**Group Pace Challenges:**
- **Too fast for some:** Offer individual catch-up support
- **Too slow for some:** Offer individual enrichment alongside group content
- **Optimal pace:** Group of similar-ability learners working together

### 14.6 Social Learning Adaptation

The ALE tracks social learning interactions and adapts recommendations:

**Tracked Social Signals:**
- Peer help requests (giving and receiving)
- Peer explanations (quality, accuracy, helpfulness)
- Discussion participation (frequency, quality)
- Group problem-solving effectiveness
- Social loafing indicators (low participation)

**Social Learning Interventions:**
- Encourage peer teaching when one member clearly understands
- Suggest discussion prompts when group is not engaging
- Rotate roles within study groups (leader, scribe, questioner, summarizer)
- Recommend group composition changes if dynamics are problematic

---

## 15. A/B Testing Framework

### 15.1 Experimental Design

The ALE includes a built-in A/B testing framework that allows continuous experimentation to improve adaptation strategies:

**Experiment Types:**
- **Algorithm tests:** Compare different adaptation algorithms
- **Parameter tests:** Compare different parameter values (e.g., different difficulty thresholds)
- **Content tests:** Compare different content versions
- **Interface tests:** Compare different UI/UX designs
- **Modality tests:** Compare different modality configurations

**Experiment Lifecycle:**
1. **Hypothesis formulation:** Define what is being tested and expected outcome
2. **Experiment design:** Set up conditions, randomization, metrics
3. **Power analysis:** Determine required sample size
4. **Experiment execution:** Run for predetermined duration
5. **Analysis:** Statistical testing and interpretation
6. **Decision:** Promote, iterate, or discard
7. **Documentation:** Record results and insights

**Example Hypothesis:**
"If we increase the target accuracy window from 70–85% to 75–90%, learners will show higher engagement scores while maintaining equivalent learning rates."

### 15.2 Randomization

**Randomization Units:**
- **Learner-level:** Each learner is assigned to one condition for the experiment duration
- **Session-level:** Different sessions for the same learner may use different conditions
- **Interaction-level:** Each individual interaction may be randomized (for high-frequency experiments)

**Stratified Randomization:**
- Ensure balanced representation across key learner characteristics
- Stratification factors: knowledge level, age, language, madhhab, device type
- Unequal randomization (e.g., 80% control, 20% treatment) for risky experiments

**Blinding:**
- Learners are not informed of experimental conditions
- Content authors and designers are blinded when possible
- Analysis is automated to minimize bias

### 15.3 Metrics Tracking

**Primary Metrics (determine experiment outcome):**
- Learning rate (concepts mastered per unit time)
- Retention rate (accuracy on delayed review)
- Engagement (session frequency, duration, completion rate)
- Satisfaction (learner ratings, NPS)

**Secondary Metrics (provide context and safety monitoring):**
- Error rate
- Time on task
- Modality switching frequency
- Support request rate
- Session abandonment rate
- Negative feedback rate

**Guardrail Metrics (must not degrade):**
- Overall satisfaction (must not decrease)
- Accessibility compliance (must not decrease)
- Performance (must not increase latency beyond threshold)
- Content accuracy (must not introduce errors)

### 15.4 Statistical Significance

**Analysis Framework:**
- Default significance level: α = 0.05
- Multiple comparison correction (Bonferroni or Benjamini-Hochberg)
- Minimum detectable effect: 5% relative change in primary metric
- Bayesian analysis for complex experiments

**Stopping Rules:**
- Fixed duration (pre-determined, not data-dependent)
- Early stopping only for: clear harm (guardrail metric violation), overwhelming evidence of effectiveness (p < 0.001)

**Subgroup Analysis:**
- Pre-registered subgroups: beginners vs. advanced, by language, by device
- Exploratory subgroup analysis flagged as such

### 15.5 Automated Promotion

When an experiment shows statistically significant and practically meaningful improvement, the winning condition is automatically promoted to production:

**Promotion Criteria:**
1. Statistically significant improvement (p < 0.05) on primary metric
2. No degradation on any guardrail metric
3. Effect size above minimum threshold
4. Results consistent across subgroups
5. Experiment duration adequate (minimum 1 week, or full learning cycle)

**Promotion Process:**
1. Winning condition identified by automated analysis
2. Code/config change created and tested
3. Gradual rollout (10% → 25% → 50% → 100% over 1 week)
4. Continued monitoring for 2 weeks post-promotion

**Rollback Triggers:**
- Negative impact on guardrail metrics
- Unexpected user complaints or support tickets
- Performance degradation
- Seasonal or contextual effects (promotion may be reversed after context changes)

---

## 16. Adaptation Rules Engine

### 16.1 Rules Engine Architecture

The adaptation rules engine is a production-rule system that encodes pedagogical principles as executable condition-action pairs. It operates alongside ML models, providing deterministic, explainable adaptations that complement statistical predictions.

### 16.2 Rule Definition Format

```
RULE: <rule_name>
WHEN: <condition_expression>
THEN: <action_expression>
PRIORITY: <integer>
EFFECTIVE: <date_range | permanent>
CONFLICT_GROUP: <group_name>
DESCRIPTION: "<human-readable explanation>"
```

**Example Rules:**

```
RULE: fatigue_intervention
WHEN: learner.fatigue_index > 0.7 AND session.duration_minutes > 20
THEN: SUPPORT INCREASE TO level_3; MODE SWITCH TO passive
PRIORITY: 100
CONFLICT_GROUP: engagement
DESCRIPTION: "When fatigue is high and learner has been studying for over 20 minutes, increase support and switch to passive mode"

RULE: incorrect_streak_intervention
WHEN: learner.consecutive_errors >= 3
THEN: DIFFICULTY DECREASE BY 1; SCAFFOLDING INCREASE TO level_4
PRIORITY: 90
CONFLICT_GROUP: difficulty
DESCRIPTION: "After 3 consecutive errors, decrease difficulty and increase scaffolding"

RULE: mastery_acceleration
WHEN: learner.accuracy_last_10 >= 0.9 AND learner.knowledge_state.current_concept >= 0.8
THEN: DIFFICULTY INCREASE BY 1; CONTENT ADVANCE
PRIORITY: 50
CONFLICT_GROUP: difficulty
DESCRIPTION: "When learner shows sustained high accuracy and near-mastery, increase difficulty and advance content"

RULE: modality_switch_on_disengagement
WHEN: learner.engagement_index < 0.4 AND modality_current_type != learner.preferred_modality
THEN: MODE SWITCH TO learner.preferred_modality
PRIORITY: 70
CONFLICT_GROUP: modality
DESCRIPTION: "When engagement is low and current modality is not preferred, switch to preferred modality"

RULE: morning_challenge
WHEN: context.time_of_day == "morning" AND learner.knowledge_state.average_mastery >= 0.5
THEN: DIFFICULTY INCREASE BY 0.5; CONTENT_TYPE "challenge"
PRIORITY: 30
CONFLICT_GROUP: time_context
DESCRIPTION: "In morning hours for intermediate+ learners, offer challenging content"
```

### 16.3 Condition Evaluation

Conditions are evaluated against the current learner model state, session context, and environmental factors:

**Condition Operators:**
- Comparison: `==`, `!=`, `<`, `>`, `<=`, `>=`
- Logical: `AND`, `OR`, `NOT`
- Set membership: `IN`, `NOT_IN`
- Temporal: `BEFORE`, `AFTER`, `BETWEEN`
- Statistical: `TRENDING_UP`, `TRENDING_DOWN`, `STABLE`, `VOLATILE`

**Condition Evaluation Order:**
1. Rules are sorted by priority (highest first)
2. All applicable rules are identified (conditions met)
3. Highest-priority rule in each conflict group fires
4. Fired rules' actions are executed in priority order

### 16.4 Action Execution

Actions are commands that adjust the learning environment:

**Action Categories:**
- **Content actions:** `CONTENT_SELECT`, `CONTENT_REORDER`, `CONTENT_SKIP`, `CONTENT_INSERT`
- **Difficulty actions:** `DIFFICULTY_INCREASE`, `DIFFICULTY_DECREASE`, `DIFFICULTY_SET`
- **Modality actions:** `MODE_SWITCH`, `MODALITY_ADD`, `MODALITY_REMOVE`
- **Pacing actions:** `PACE_INCREASE`, `PACE_DECREASE`, `BREAK_SUGGEST`
- **Support actions:** `SCAFFOLDING_INCREASE`, `SCAFFOLDING_DECREASE`, `HINT_ADD`, `HINT_REMOVE`
- **Feedback actions:** `FEEDBACK_TYPE_SET`, `FEEDBACK_DEPTH_SET`, `FEEDBACK_TONE_SET`

**Action Parameters:**
- Numeric increments/decrements (e.g., `DIFFICULTY INCREASE BY 1`)
- Absolute values (e.g., `DIFFICULTY SET TO 5`)
- Relative adjustments (e.g., `SUPPORT INCREASE TO level_4`)
- Mode switches (e.g., `MODE SWITCH TO visual`)

### 16.5 Rule Priority

Priority determines which rule fires when multiple rules are applicable:

**Priority Scale:**
- 100–90: Safety and wellbeing (fatigue, frustration, distress)
- 89–70: Learning crisis (error streaks, knowledge gaps, confusion)
- 69–50: Engagement (boredom, disengagement, motivation)
- 49–30: Optimization (difficulty calibration, pacing, flow)
- 29–10: Personalization (preferences, modality, style)
- 9–1: Enhancement (enrichment, exploration, variety)

### 16.6 Conflict Resolution

When multiple rules in the same conflict group fire simultaneously:

**Conflict Groups:**
- `difficulty`: Only one difficulty-changing rule fires (highest priority)
- `modality`: Only one modality-changing rule fires
- `pacing`: Only one pacing-changing rule fires
- `support`: Only one support-changing rule fires
- `engagement`: Multiple engagement rules may fire if they address different dimensions

**Resolution Strategy:**
1. Within conflict group, only highest-priority rule fires
2. Different conflict groups' rules fire independently (can adjust difficulty AND modality simultaneously)
3. If two rules in same group have equal priority, the one with more specific condition wins
4. If truly tied, use a pre-determined tiebreaker (first rule in alphabetical order)

### 16.7 Rule Lifecycle

**Rule Creation:**
- Created by instructional designers and learning scientists
- Parameterized for flexibility (thresholds, increments)
- Tested in simulation before deployment

**Rule Testing:**
- Simulate with historical learner data
- Check for conflicts and unintended interactions
- Validate rule behavior across learner types

**Rule Deployment:**
- Graduated rollout: 10% → 25% → 100%
- A/B testing of new rules vs. current rules
- Monitoring dashboard for rule performance

**Rule Monitoring:**
- How often does each rule fire?
- What is the outcome when the rule fires?
- Are there learner characteristics associated with rule firing?
- Is the rule producing the intended effect?

**Rule Retirement:**
- Rules that rarely fire or have no measurable effect are retired
- Rules that are consistently superseded by higher-priority rules are reviewed
- Rules that create negative outcomes are immediately retired
- Retired rules are archived for future reference

### 16.8 Rule Chaining

Rules can trigger other rules through a chaining mechanism:

**Chaining Types:**
- **Sequential:** Rule A's action creates conditions for Rule B to fire
- **Conditional:** Rule A adjusts a parameter that changes Rule B's evaluation
- **Hierarchical:** Higher-level rule activates a set of lower-level rules for the session

**Chaining Safety:**
- Maximum chain depth: 5 (prevents infinite loops)
- Chain termination if same rule fires twice in a chain (loop detection)
- All chaining is logged for debugging

---

## 17. Machine Learning Adaptation

### 17.1 ML Architecture

The ALE employs multiple ML approaches depending on the adaptation task:

| Task | ML Approach | Input | Output |
|------|-------------|-------|--------|
| Knowledge state estimation | Bayesian Knowledge Tracing (BKT) / Deep Knowledge Tracing (DKT) | Response sequence, item characteristics | Mastery probability per concept |
| Difficulty prediction | Item Response Theory (IRT) / Neural IRT | Learner ability, item parameters | Probability of correct response |
| Next content recommendation | Collaborative Filtering + Content-Based Filtering | Learner history, content features | Ranked content list |
| Engagement prediction | Sequence models (LSTM/Transformer) | Recent interaction sequence | Engagement score |
| Modality effectiveness | Bandit algorithms | Historical modality performance | Optimal modality selection |

### 17.2 Collaborative Filtering

**Use Case:** Content recommendation and peer matching

**Approach:** Matrix factorization on learner-content interaction matrix

**Implementation:**
- Learner embedding: Dense vector representing learner's content preferences
- Content embedding: Dense vector representing content characteristics
- Score = dot product of learner and content embeddings
- Cold start: New learners use demographic/personalization data for initial embeddings

**Training:**
- Implicit feedback (views, completions, time spent) and explicit feedback (ratings)
- Regularized matrix factorization with bias terms
- Periodic retraining (weekly) with incremental updates

### 17.3 Content-Based Filtering

**Use Case:** Content recommendation based on content attributes

**Approach:** Feature-based similarity matching

**Features:**
- Content domain, topic, subtopic
- Content type (concept, procedure, narrative, etc.)
- Difficulty level
- Modality compatibility
- Scholar/source attribution
- Language and script

**Recommendation:**
- Compute content similarity to learner's positively engaged content
- Recommend content with high similarity but not yet consumed
- Balance similarity with diversity (avoid filter bubbles)

### 17.4 Reinforcement Learning

**Use Case:** Sequential adaptation decisions (what to do next)

**Approach:** Deep Q-Network (DQN) or Proximal Policy Optimization (PPO)

**State Space:**
- Learner knowledge state vector
- Recent interaction history embedding
- Session context features
- Available content pool

**Action Space:**
- Which content to present next
- At what difficulty level
- In which modality
- With what support level

**Reward Function:**
- Primary: Knowledge gain (pre- vs. post-assessment)
- Secondary: Engagement sustainment, time efficiency
- Shaped rewards for intermediate actions (correct answer, hint avoidance)
- Penalty for negative outcomes (frustration, disengagement)

**Training:**
- Offline training on historical interaction data
- Online fine-tuning with exploration (epsilon-greedy)
- Target network for stable training
- Experience replay buffer

### 17.5 Bandit Algorithms

**Use Case:** Modality selection, feedback type selection, content variation selection

**Approach:** Contextual Multi-Armed Bandits (LinUCB, Thompson Sampling)

**Implementation:**
- Arms: Different options (modalities, feedback types, content variations)
- Context: Learner and situation features
- Reward: Engagement or learning outcome
- Algorithm: Thompson Sampling for probabilistic selection

**Advantages:**
- Efficient exploration (fewer bad experiences)
- Rapid adaptation to changing preferences
- Well-suited for online learning with immediate feedback

### 17.6 Sequence Models

**Use Case:** Next-action prediction, dropout prediction, engagement forecasting

**Approach:** LSTM or Transformer models

**Features:**
- Sequence of learner actions with timestamps
- Item and context features for each action
- Time intervals between actions

**Outputs:**
- Next likely action or content
- Probability of session continuation
- Predicted engagement for next 10 minutes
- Risk of dropout or disengagement

**Architecture:**
- Embedding layer for categorical features
- Multi-layer LSTM or Transformer encoder
- Task-specific output heads
- Attention mechanism for interpretability (what past actions influenced prediction)

### 17.7 Model Training Pipeline

**Data Collection:**
- All learner interactions logged to event stream
- Data warehouse for historical analysis
- Feature store for reusable feature computation

**Feature Engineering:**
- Automated feature extraction from event stream
- Feature selection based on mutual information and feature importance
- Handling missing data and outliers
- Temporal feature computation (rolling windows, trends)

**Model Training:**
- Automated training pipeline (Airflow or similar)
- Hyperparameter optimization (Bayesian search)
- Cross-validation with time-series awareness
- Model versioning and artifact storage

**Model Evaluation:**
- Holdout test set (temporal split)
- Offline metrics (AUC, RMSE, NDCG) depending on task
- Online metrics (A/B test results)
- Fairness evaluation across learner subgroups

**Model Deployment:**
- Real-time inference service (low-latency, high-throughput)
- Batch inference for non-real-time tasks
- Canary deployment for new models
- Automatic rollback on performance degradation

**Model Monitoring:**
- Prediction drift detection
- Feature drift detection
- Performance metric dashboards
- Alerting on significant changes

---

## 18. Islamic Adaptation

### 18.1 Madhhab-Aware Content

The ALE adapts fiqh content based on the learner's madhhab (school of jurisprudence):

**Madhhab Profiles:**
- Each learner can specify their madhhab: Hanafi, Maliki, Shafi'i, Hanbali, or Non-madhhabi
- Learners can also choose comparative fiqh (all madhahib presented)
- Madhhab can be changed at any time

**Madhhab Adaptation:**
- Fiqh content filtered by madhhab
- Rulings presented according to the learner's madhhab as primary position
- Other madhahib positions shown as secondary (comparative) information
- Scholarly references prioritized from the learner's tradition

**Madhhab-Specific Features:**
- Madhhab-specific prayer timings calculation methods
- Madhhab-specific fiqh calculators (zakat, inheritance)
- References to authoritative texts within the madhhab tradition
- Links to major scholars of the madhhab

**Comparative Mode:**
- Side-by-side comparison of madhahib rulings
- Exploration of evidence used by each madhhab
- Understanding of principles (usul) behind differences
- Respectful presentation of differences as richness of tradition

### 18.2 Language-Preference Adaptation

The ALE adapts content based on the learner's language profile:

**Language Profiles:**
- Primary language (for instruction and UI)
- Secondary languages (for additional resources)
- Arabic proficiency level (none, basic, intermediate, advanced, fluent)
- Preference for original Arabic alongside translation

**Language Adaptation:**
- Content delivered in the learner's primary language
- Arabic original preserved alongside translation for Quranic/hadith content
- Transliteration available for Arabic text
- Language complexity adjusted to proficiency level

**Arabic Learning Integration:**
- For learners studying Arabic, content integrates language learning
- Vocabulary lists extracted from current content
- Grammar explanations linked to encountered structures
- Progressive Arabic immersion as proficiency grows

### 18.3 Cultural Sensitivity

The ALE adapts content presentation for cultural sensitivity:

**Cultural Considerations:**
- Visual depictions: Respectful treatment of prophets (no facial depiction), appropriate clothing in illustrations
- Gender considerations: Appropriate content framing, gender-separate options where culturally needed
- Regional customs: Recognition of valid regional variations in practice
- Sensitivity to converts: Appropriate framing for new Muslims (avoiding assumptions of prior knowledge)

**Cultural Content Adaptation:**
- Examples and analogies drawn from culturally familiar contexts
- Regional scholarly traditions acknowledged and respected
- Culturally appropriate greetings and expression
- Avoidance of culturally insensitive or controversial content

### 18.4 Regional Scholarly Tradition

The ALE recognizes and adapts to regional scholarly traditions:

**Regional Profiles:**
- **South Asian:** Emphasis on Deobandi, Barelvi, and Aligarh traditions
- **Middle Eastern:** Al-Azhar, Qum, and other traditional centers
- **Southeast Asian:** Malay-Indonesian scholarly tradition
- **African:** North African Maliki tradition, Sub-Saharan traditions
- **Western:** Contemporary Western scholarship and fatwa institutions

**Regional Adaptation:**
- Preferred scholars and institutions from the learner's region
- Regional fiqh practices (e.g., moon sighting differences)
- Local language resources and translations
- Connection to local Islamic educational institutions

### 18.5 Age-Appropriate Islamic Content

The ALE adapts content for different age groups:

**Children (5–12):**
- Simple language, concrete examples, visual storytelling
- Focus on basic concepts: love of Allah, love of Prophet, basic manners (adab)
- Stories of prophets presented in age-appropriate detail
- Interactive activities and games for engagement
- Memorization of short surahs and basic du'as

**Teens (13–17):**
- Deeper exploration of aqidah and fiqh with real-world relevance
- Addressing teenage-specific challenges through Islamic lens
- Critical thinking about contemporary issues
- Peer learning and discussion opportunities
- Identity formation in Islamic context

**Young Adults (18–30):**
- Advanced fiqh for daily life (marriage, finance, career)
- Intellectual engagement with Islamic scholarship
- Contemporary fiqh and fatwa exploration
- Leadership development and community engagement

**Adults (31+):**
- Comprehensive study of classical texts
- Practical application in family and community contexts
- Deepening spirituality (tazkiyah) and reflective practice
- Teaching and mentoring preparation

**Seniors (60+):**
- Emphasis on ibadah, dhikr, and spiritual preparation
- Accessible content (larger text, clear audio, simple navigation)
- Legacy and family-related content
- Reflection and tazkiyah emphasis

### 18.6 Islamic Calendar Adaptation

The ALE adapts content based on the Islamic calendar:

**Calendar-Aware Content:**
- **Ramadan:** Fasting fiqh, spiritual preparation, Quran completion, laylat al-qadr focus
- **Dhul Hijjah:** Hajj content, sacrifice fiqh, first 10 days emphasis
- **Muharram:** Ashura, year reflection, hijra significance
- **Rabi al-Awwal:** Sirah focus, Prophet's character study
- **Shaban:** Preparation for Ramadan, mid-Shaban practices
- **Rajiab:** Sacred months, isra wal mi'raj

**Seasonal Content Adjustment:**
- Content related to current Islamic month promoted
- Reminders for upcoming Islamic events and practices
- Preparation content before significant periods (e.g., Ramadan preparation in Shaban)
- Reflection content after significant periods

---

## Cross-Module Connections

**Connection to 00_Personal_AI_Mentor:**
The Personal AI Mentor is the primary consumer and driver of the Adaptive Learning Engine. The Mentor interprets the ALE's adaptation decisions and interacts with the learner, explaining why certain adaptations are made and offering the learner choices. The Mentor's personality, tone, and pedagogical approach are themselves adaptive — the Mentor learns how to best mentor each individual learner. The ALE provides the Mentor with decision support; the Mentor provides the ALE with rich data about the learner's reception and response to adaptations.

**Connection to 37_Curriculum_Paths:**
The curriculum paths defined in document 37 are the tracks along which the ALE navigates. The curriculum graph — with its nodes (concepts), edges (prerequisites), and multiple possible paths — is the terrain over which the ALE charts each learner's individual journey. The ALE selects which path to follow, adjusts the path based on learner data, and can create deviations (detours, enrichment, remediation) while maintaining connection to the overall curriculum structure.

**Connection to 13_Quiz_Mode:**
Quiz mode provides the assessment data that is a primary input to the ALE's learner model. Quiz performance updates knowledge state estimates, difficulty calibration, and mastery tracking. The ALE, in turn, adapts quiz difficulty, question types, feedback timing, and content selection. The two systems form a closed loop: adaptive learning → assessment → knowledge update → further adaptation.

**Connection to 02_Memory_Engine:**
The Memory Engine determines when content needs to be reviewed based on the forgetting curve and spacing effect. The ALE uses this information to schedule review items within sessions, prioritize content that is at risk of being forgotten, and determine the optimal interleaving of new and review content. The Memory Engine provides the retention dimension of the ALE's optimization; the ALE provides the Memory Engine with the learning schedule that determines when memory traces are formed.

**Connection to 04_Educational_Psychology:**
The educational psychology foundation (document 04) provides the theoretical basis for all adaptation decisions. Cognitive load theory informs difficulty and modality adaptation. Motivation theories inform engagement and goal adaptation. Memory research informs spacing and review scheduling. Self-regulated learning theory informs scaffolding fading and autonomy graduation. The ALE operationalizes these psychological principles into executable adaptation policies.

**Connection to 05_Instructional_Design:**
The instructional design framework (document 05) defines the pedagogical strategies that the ALE selects among. Different instructional approaches (direct instruction, inquiry-based learning, problem-based learning, mastery learning) are encoded as adaptation strategies within the ALE's rules and ML models. The ALE selects the instructional approach most appropriate for the learner, content, and context, and adjusts the intensity and purity of the approach based on learner response.

---

## Appendices

### Appendix A: Adaptation Decision Time Frames

| Time Frame | Scope | Decision Examples | Update Frequency |
|------------|-------|-------------------|-----------------|
| Real-time (ms) | Single interaction | Difficulty of next item, hint offer | After each learner action |
| Within-session (min) | Current session | Session pacing, break timing, modality | Every 5–10 actions |
| Between-session (hours) | Current day | Daily plan, review schedule | At session boundaries |
| Daily | Current day | Day's content, time allocation | Once per day |
| Weekly | Past week | Goal adjustment, strategy update | Once per week |
| Monthly | Past month | Path adjustment, model recalibration | Once per month |
| Quarterly | Past 3 months | Major goal reassessment | Once per quarter |
| Annually | Past year | Life goal alignment, long-term planning | Once per year |

### Appendix B: Learner Model Schema

```json
{
  "learner_id": "string",
  "profile": {
    "demographics": { "age_group", "language", "region", "madhhab" },
    "goals": { "short_term": [], "medium_term": [], "long_term": [] },
    "preferences": { "modality": {}, "pace": {}, "feedback": {} }
  },
  "knowledge_state": {
    "concepts": {
      "<concept_id>": {
        "mastery": 0.0..1.0,
        "stage": "unlearned|learning|learned|mastered",
        "last_practiced": "timestamp",
        "review_interval": "days",
        "forgetting_curve": { "params": {} },
        "confusion_level": 0.0..1.0
      }
    },
    "knowledge_graph": { "edges": [] }
  },
  "performance_history": {
    "overall": { "accuracy", "avg_response_time", "total_items", "learning_rate" },
    "by_concept": {},
    "by_session": {},
    "by_day": {},
    "trends": { "improving", "stable", "declining" }
  },
  "behavioral_patterns": {
    "study_habits": { "consistency", "session_length_avg", "preferred_times" },
    "interaction_style": { "explorer|focused|minimalist|perfectionist" },
    "engagement_curve": { "per_session": [], "per_day": [] }
  },
  "motivational_state": {
    "intrinsic_motivation": 0.0..1.0,
    "extrinsic_motivation": 0.0..1.0,
    "current_affective_state": "bored|frustrated|flow|confused|curious|anxious",
    "self_efficacy": 0.0..1.0
  },
  "context": {
    "current": { "time", "device", "location", "social_context" },
    "historical": { "typical_times", "typical_locations" }
  },
  "adaptation_history": {
    "difficulty_changes": [],
    "modality_switches": [],
    "path_deviations": [],
    "interventions": []
  }
}
```

### Appendix C: Adaptation Rules Priority Matrix

| Priority | Category | Example Rule | Response Time |
|----------|----------|-------------|---------------|
| 100 | Learner safety | Fatigue > threshold, session > max duration | Immediate |
| 95 | Emotional distress | Frustration signals > threshold | Immediate |
| 90 | Academic crisis | 3+ consecutive errors | Immediate |
| 85 | Remediation | Knowledge gap detected | Immediate |
| 80 | Engagement rescue | Engagement index < 0.3 | Immediate |
| 75 | Modality misfit | Modality switching multiple times | < 1 min |
| 70 | Difficulty mismatch | Accuracy outside optimal range | Immediate |
| 65 | Scaffolding adjustment | Hint usage pattern change | Immediate |
| 60 | Pacing adjustment | Response time trend change | < 2 min |
| 55 | Content recommendation | Current content near completion | < 1 min |
| 50 | Break scheduling | Session duration approaching limit | < 5 min |
| 45 | Goal progress check | Milestone approaching | End of session |
| 40 | Review scheduling | Content due for review | Session start |
| 35 | Modality optimization | Performance better in alt modality | < 3 min |
| 30 | Preference update | Consistent behavior pattern | End of session |
| 25 | Enrichment offer | Mastery rate > expected | < 2 min |
| 20 | Exploration suggestion | No new content explored recently | End of session |
| 15 | Social recommendation | Group learning opportunity | End of session |
| 10 | Achievement notice | Milestone reached | End of session |
| 5 | Casual suggestion | Related content of possible interest | Low priority |

### Appendix D: Key Adaptation Metrics

| Metric | What It Measures | Target | Action If Below Target |
|--------|-----------------|--------|----------------------|
| Learning rate | Concepts mastered per hour | ≥ 2 per hour | Review difficulty calibration, check prerequisites |
| Retention rate | Accuracy on delayed reviews | ≥ 75% | Increase review frequency, improve encoding strength |
| Engagement score | Session frequency, duration, depth | ≥ 0.7 (scale 0–1) | Check modality fit, interest alignment, goal relevance |
| Satisfaction score | Learner ratings | ≥ 4.0 / 5.0 | Review difficulty balance, support level, content quality |
| Optimal challenge % | Time spent in 70-85% accuracy zone | ≥ 60% of session | Improve difficulty calibration |
| Session completion rate | Sessions completed vs. started | ≥ 80% | Address frustration, time management, engagement |
| Modality satisfaction | Per-modality learner ratings | ≥ 3.5 / 5.0 | Improve content quality, switch frequency, modality fit |
| Adaptation accuracy | Predicted vs. actual outcomes | ≥ 80% agreement | Recalibrate learner model, update rules |

---

*End of Document 09 — Adaptive Learning Engine*
