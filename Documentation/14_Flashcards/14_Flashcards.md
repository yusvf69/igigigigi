# FLASHCARD ARCHITECTURE

## Fact Memorization and Rapid Recall System

---

| Document ID | IEP-ARCH-FC-001 |
|---|---|
| Version | 1.0.0 |
| Status | Approved |
| Classification | Enterprise Internal |
| Effective Date | 2026-07-07 |
| Approving Authority | Office of the Chief Architect |
| Custodian | Memorization Content Architecture Team |
| Content Generation Reference | IEP-ARCH-CGEN-001 |
| Knowledge Graph Reference | IEP-ARCH-KG-001 |
| Verification Reference | IEP-ARCH-VRF-001 |

---

## TABLE OF CONTENTS

1. EXECUTIVE OVERVIEW
2. FLASHCARD PHILOSOPHY
3. FLASHCARD TYPES
4. FLASHCARD GENERATION PIPELINE
5. CARD STRUCTURE AND FORMAT
6. SPACED REPETITION ENGINE
7. LEARNING PROGRESSION
8. MULTI-MEDIA CARDS
9. DECK MANAGEMENT
10. STUDY SESSION MANAGEMENT
11. SYNCHRONIZATION AND OFFLINE
12. QUALITY METRICS
13. PERFORMANCE REQUIREMENTS
14. APPENDICES

---

## 1. EXECUTIVE OVERVIEW

### 1.1 Purpose

Flashcards provide a rapid, efficient mechanism for memorizing Islamic knowledge. They distill complex facts into digestible question-answer pairs optimized for spaced repetition, enabling learners to build long-term retention of essential Islamic concepts, verses, hadith, and terminology.

### 1.2 Scope

This document covers the flashcard generation system, card types, spaced repetition algorithm, deck management, study session mechanics, and multi-media card support.

### 1.3 Core Principle

**Every flashcard must contain exactly one atomic fact from the Knowledge Graph, presented for optimal memorization and recall.**

---

## 2. FLASHCARD PHILOSOPHY

### 2.1 Design Principles

| Principle | Description |
|---|---|
| **Atomic Facts** | Each card teaches exactly one fact from the Knowledge Graph. |
| **Active Recall** | Cards require the learner to produce the answer, not recognize it. |
| **Minimum Information** | Cards contain the minimum information needed for the fact. |
| **Progressive Building** | Complex concepts are broken into multiple cards that build on each other. |
| **Contextual Learning** | Cards include context (source, category) but keep the core fact atomic. |
| **Multiple Formats** | The same fact can appear in different card formats for different learning styles. |

### 2.2 Islamic Memorization Tradition

The flashcard system draws from traditional Islamic memorization methods:

| Traditional Method | Flashcard Equivalent | Application |
|---|---|---|
| **Tikrar (Repetition)** | Spaced repetition scheduling | Repeated review at optimal intervals |
| **Tarseel (Gradual)** | Progressive card difficulty | Start easy, gradually increase complexity |
| **Ard (Presentation)** | Active recall testing | Learner must produce answer from memory |
| **Taffahum (Understanding)** | Explanation cards | Cards that test comprehension, not just rote |
| **Tasheel (Simplification)** | Atomic fact decomposition | Break complex knowledge into simple cards |
| **Muraja'ah (Review)** | Deck review sessions | Systematic review of learned cards |

### 2.3 Learning Objectives

| Objective | Card Focus | Example |
|---|---|---|
| **Memorization** | Exact fact recall | "What are the five pillars of Islam?" |
| **Comprehension** | Understanding meaning | "Explain what tawheed means." |
| **Association** | Linking related concepts | "Which prophet is associated with the event of the splitting of the sea?" |
| **Application** | Using knowledge | "What should a Muslim say when sneezing?" |
| **Recognition** | Identifying patterns | "Identify the type of hadith based on its chain." |

---

## 3. FLASHCARD TYPES

### 3.1 Card Format Types

| Type | Front | Back | Best For |
|---|---|---|---|
| **Basic** | Question | Answer | Fact recall |
| **Cloze Deletion** | Sentence with blank | Completed sentence | Terminology, verses, definitions |
| **Q&A** | Question | Answer + Explanation | Comprehensive learning |
| **Reverse** | Answer (as prompt) | Question (as recall) | Bidirectional association |
| **Image** | Image prompt | Description/fact | Visual learners |
| **Audio** | Audio clip | Text/translation | Pronunciation, Quran memorization |
| **List** | Single item prompt | Position in list | Sequential facts |
| **Comparison** | Concept pair | Similarities/differences | Comparative learning |

### 3.2 Content-Based Card Types

| Type | Front | Back | Source |
|---|---|---|---|
| **Verse (Ayah)** | "Complete the verse: {partial}" | Full verse + surah/ayah reference | Quran Knowledge Graph |
| **Hadith** | "Complete the hadith: {partial}" | Full hadith + reference | Hadith Knowledge Graph |
| **Term (Mustalah)** | "Define: {term}" | Definition + example | Terminology Knowledge Graph |
| **Person (Science)** | "Who is {name}?" | Biography + key contributions | Biography Knowledge Graph |
| **Event (Tarikh)** | "What happened in {year}?" | Event description + significance | History Knowledge Graph |
| **Rule (Hukm)** | "What is the ruling on {action}?" | Ruling + evidence | Fiqh Knowledge Graph |
| **Concept (Mafhum)** | "Explain: {concept}" | Definition + evidence | Aqidah Knowledge Graph |
| **Prayer (Du'a)** | "What is the du'a for {occasion}?" | Du'a text + source | Du'a Knowledge Graph |

### 3.3 Card Format Selection

```
Knowledge Graph Fact
    │
    ▼
┌──────────────────────────────┐
│ Card Type Classifier         │
│ • Analyze fact structure     │
│ • Match to optimal format    │
│ • Consider learner preference│
└──────────────────────────────┘
    │
    ├── Definition fact → Basic / Cloze
    ├── Sequential fact → List / Ordered
    ├── Comparative fact → Comparison
    ├── Text-heavy fact → Cloze Deletion
    ├── Visual fact → Image Card
    ├── Audio fact (Quran, Du'a) → Audio Card
    └── Bidirectional fact → Reverse Card
```

---

## 4. FLASHCARD GENERATION PIPELINE

### 4.1 Pipeline Stages

```
┌─────────────────────────────────────────────────────────────────────────┐
│                     1. FACT SELECTION                                   │
│  • Query Knowledge Graph for atomic facts                              │
│  • Verify fact suitability for flashcard format                        │
│  • Ensure fact atomicity (single fact per card)                        │
└──────────────────────────────┬──────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                     2. CARD TYPE DETERMINATION                          │
│  • Select optimal card format                                          │
│  • Determine on/back content split                                     │
│  • Identify cloze deletion points (if applicable)                      │
└──────────────────────────────┬──────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                     3. CONTENT GENERATION                               │
│  • Generate front content (cue)                                        │
│  • Generate back content (answer)                                      │
│  • Add supplementary information (source, category, tags)              │
└──────────────────────────────┬──────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                     4. VERIFICATION                                     │
│  • Fact accuracy check                                                 │
│  • Card clarity review                                                 │
│  • Atomicity verification (one fact per card)                          │
│  • Duplicate detection                                                 │
└──────────────────────────────┬──────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                     5. ENRICHMENT                                       │
│  • Add audio (TTS for Arabic text)                                     │
│  • Add images if applicable                                            │
│  • Generate mnemonic hints                                             │
│  • Link to related cards                                               │
└─────────────────────────────────────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                     6. DECK ASSIGNMENT                                  │
│  • Assign to appropriate deck(s)                                       │
│  • Set initial spaced repetition parameters                            │
│  • Index for search and review                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

### 4.2 Atomic Fact Decomposition

Complex facts are decomposed into atomic flashcard units:

```
KG Fact: "The five pillars of Islam are Shahada, Salah, Zakat, Sawm, and Hajj."
    │
    ├── Card 1: "What are the five pillars of Islam?" → "Shahada, Salah, Zakat, Sawm, Hajj"
    ├── Card 2: "What is the first pillar of Islam?" → "Shahada (Declaration of Faith)"
    ├── Card 3: "What is the second pillar of Islam?" → "Salah (Prayer)"
    ├── Card 4: "What is the third pillar of Islam?" → "Zakat (Charity)"
    ├── Card 5: "What is the fourth pillar of Islam?" → "Sawm (Fasting during Ramadan)"
    ├── Card 6: "What is the fifth pillar of Islam?" → "Hajj (Pilgrimage to Mecca)"
    ├── Card 7: "What does Shahada mean?" → "The declaration that there is no god but Allah..."
    └── Card 8 (Cloze): "The _____ pillars of Islam are..." → "five"
```

---

## 5. CARD STRUCTURE AND FORMAT

### 5.1 Card Data Model

| Field | Type | Description | Required |
|---|---|---|---|
| `card_id` | UUID | Unique identifier | Yes |
| `fact_ids` | UUID[] | Source Knowledge Graph fact IDs | Yes |
| `deck_id` | UUID | Parent deck identifier | Yes |
| `card_type` | Enum | Basic, Cloze, Q&A, Reverse, Image, Audio | Yes |
| `front` | Text | Front content (cue) | Yes |
| `back` | Text | Back content (answer) | Yes |
| `front_language` | String | Language of front content | Yes |
| `back_language` | String | Language of back content | Yes |
| `metadata` | JSON | Source, category, tags, difficulty | Yes |
| `media` | JSON[] | Associated media (images, audio) | No |
| `hints` | String[] | Mnemonic hints | No |
| `related_card_ids` | UUID[] | Links to related cards | No |
| `created_at` | Timestamp | Generation time | Yes |
| `updated_at` | Timestamp | Last modification | Yes |

### 5.2 Card Display Format

```
┌─────────────────────────────────────────────┐
│  FRONT                                       │
│  ─────────────────────────────────────────── │
│  What is the first pillar of Islam?          │
│                                              │
│  [Tap to reveal answer]                      │
│                                              │
│  Category: Aqidah    Difficulty: Easy        │
│  Source: Hadith of Jibril (Muslim)           │
└─────────────────────────────────────────────┘
              │ (tap)
              ▼
┌─────────────────────────────────────────────┐
│  BACK                                        │
│  ─────────────────────────────────────────── │
│  Shahada (Declaration of Faith)              │
│                                              │
│  "There is no god but Allah, and Muhammad    │
│  is the Messenger of Allah."                 │
│                                              │
│  ─────────────────────────────────────────── │
│  Rate your recall:                           │
│  [Again] [Hard] [Good] [Easy]               │
└─────────────────────────────────────────────┘
```

### 5.3 Cloze Deletion Format

```
┌─────────────────────────────────────────────┐
│  FRONT                                       │
│  ─────────────────────────────────────────── │
│  The first pillar of Islam is {_____}.       │
│                                              │
│  Category: Aqidah    Difficulty: Easy        │
└─────────────────────────────────────────────┘
              │ (tap)
              ▼
┌─────────────────────────────────────────────┐
│  BACK                                        │
│  ─────────────────────────────────────────── │
│  The first pillar of Islam is Shahada.       │
│                                              │
│  Shahada: Declaration of Faith               │
│  "There is no god but Allah, and Muhammad    │
│  is the Messenger of Allah."                 │
│                                              │
│  Rate your recall:                           │
│  [Again] [Hard] [Good] [Easy]               │
└─────────────────────────────────────────────┘
```

---

## 6. SPACED REPETITION ENGINE

### 6.1 Algorithm (SM-2 Based with Islamic Learning Adaptations)

| Parameter | Description | Default Value |
|---|---|---|
| **Ease Factor (EF)** | Multiplier for interval growth | 2.5 |
| **Interval (I)** | Days until next review | 1, 6, 15, 40, 100, 250, etc. |
| **Repetitions (n)** | Consecutive correct answers count | 0 |
| **Quality (q)** | Self-assessed recall quality | 0-5 |

### 6.2 Scheduling Logic

| Quality | Rating | Meaning | Interval Adjustment | EF Change |
|---|---|---|---|---|
| 5 | Easy | Perfect recall | I' = I × EF | EF += 0.15 |
| 4 | Good | Correct with some effort | I' = I × EF | EF += 0.10 |
| 3 | Hard | Correct but difficult | I' = I × EF × 0.8 | EF -= 0.05 |
| 2 | Again (with hint) | Recalled after hint | I' = max(I × 0.5, 1) | EF -= 0.10 |
| 1 | Again (wrong, close) | Incorrect but had idea | I' = 1 (reset) | EF -= 0.15 |
| 0 | Again (wrong, no idea) | Complete forget | I' = 1 (reset) | EF -= 0.20 |

### 6.3 Islamic Learning Adaptations

| Adaptation | Rationale | Implementation |
|---|---|---|
| **Blessed Times** | Learning before Fajr, after Asr is traditionally encouraged | Prioritize review scheduling during these times |
| **Day of Jumu'ah** | Friday is a special day for review | Schedule weekly reviews on Fridays |
| **Ramadan Adjustment** | Increased worship may reduce study time | Automatically reduce new card count during Ramadan |
| **Holiday Preservation** | Eid days are for family, not study | Skip reviews on Eid al-Fitr and Eid al-Adha |
| **Barakah in Consistency** | Small daily amounts are better than large sporadic sessions | Default to small daily card limits (10-20 new cards/day) |

### 6.4 Review Queue Priority

| Priority | Card Type | Scheduling |
|---|---|---|
| **P0** | Overdue cards (past due date) | Immediate review |
| **P1** | Due today | Review today |
| **P2** | Due tomorrow | Pre-load for next session |
| **P3** | New cards (within daily limit) | Learn after review queue |
| **P4** | Future cards | Not yet scheduled |

---

## 7. LEARNING PROGRESSION

### 7.1 Card Lifecycle

```
New Card
    │
    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                      LEARNING PHASE                                     │
│  First exposure to the card                                             │
│  • See front → attempt recall → reveal back                            │
│  • Repeat until rated ≥ 3 (Hard)                                       │
│  • Maximum 6 repetitions per session                                   │
└─────────────────────────────────────────────────────────────────────────┘
    │ (rated ≥ 3)
    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                      REVIEW PHASE                                       │
│  Card enters spaced repetition schedule                                │
│  • Reviewed at increasing intervals                                    │
│  • Each review strengthens memory                                       │
│  • Correct → longer interval                                           │
│  • Incorrect → back to learning                                        │
└─────────────────────────────────────────────────────────────────────────┘
    │ (interval reaches 365 days)
    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                      MASTERED PHASE                                     │
│  Card is considered permanently retained                               │
│  • Annual review only                                                   │
│  • Can be demoted if missed in annual review                            │
└─────────────────────────────────────────────────────────────────────────┘
```

### 7.2 Learning Steps

| Step | Action | Duration |
|---|---|---|
| **Step 1** | See card front, attempt recall | 10 seconds max |
| **Step 2** | If correct, next step. If wrong, see answer and try again in 1 minute | 1 minute |
| **Step 3** | If correct, next step. If wrong, back to Step 1 | 10 minutes |
| **Step 4** | If correct, card graduates to review phase | — |

---

## 8. MULTI-MEDIA CARDS

### 8.1 Supported Media Types

| Media Type | Format | Max Size | Use Case |
|---|---|---|---|
| **Audio** | MP3, AAC | 2 MB | Quran recitation, du'a pronunciation, vocabulary audio |
| **Image** | JPEG, PNG, WebP | 500 KB | Visual aids, maps, diagrams, calligraphy |
| **SVG** | SVG | 100 KB | Diagrams, charts, Arabic script |
| **Video** | MP4 (short clips) | 5 MB | Demonstration, context (limited use) |

### 8.2 Audio Generation

| Content Type | Audio Source | Generation |
|---|---|---|
| **Quran Verses** | High-quality recitation database | Pre-recorded by renowned qurra' |
| **Du'a/Supplications** | TTS or recording | TTS with Arabic voice model |
| **Arabic Vocabulary** | TTS | Neural TTS with proper tajweed |
| **Hadith Text** | TTS | Neural TTS |
| **Translations** | TTS | Language-specific neural TTS |

### 8.3 Image Generation

| Image Type | Source | Description |
|---|---|---|
| **Calligraphy** | Generated or licensed | Beautiful Arabic calligraphy of verses/terms |
| **Diagram** | Generated | Concept maps, relationships, hierarchies |
| **Map** | Geographic data | Historical locations, routes |
| **Icon/Symbol** | Icon library | Visual representation of concepts |
| **Photograph** | Licensed or generated | Historical sites, artifacts |

---

## 9. DECK MANAGEMENT

### 9.1 Deck Hierarchy

```
Islamic Knowledge (Root Deck)
    │
    ├── Aqidah (Faith)
    │       ├── Tawheed
    │       ├── Angels
    │       ├── Books
    │       ├── Prophets
    │       └── Qadr
    │
    ├── Fiqh (Jurisprudence)
    │       ├── Taharah (Purification)
    │       ├── Salah (Prayer)
    │       ├── Zakat (Charity)
    │       ├── Sawm (Fasting)
    │       └── Hajj (Pilgrimage)
    │
    ├── Hadith
    │       ├── Hadith Terminology
    │       ├── Sahih al-Bukhari
    │       ├── Sahih Muslim
    │       └── Forty Hadith of Nawawi
    │
    ├── Quran
    │       ├── Surah Names
    │       ├── Juz Division
    │       ├── Key Verses
    │       └── Tajweed Rules
    │
    ├── Seerah (Prophetic Biography)
    │       ├── Meccan Period
    │       ├── Medinan Period
    │       └── Battles and Expeditions
    │
    ├── Islamic History
    │       ├── Rightly Guided Caliphs
    │       ├── Umayyad Period
    │       ├── Abbasid Period
    │       └── Andalus
    │
    └── Du'a and Adhkar
            ├── Morning Adhkar
            ├── Evening Adhkar
            └── Occasional Du'as
```

### 9.2 Auto-Deck Assignment

When a flashcard is generated, it is automatically assigned:

| Knowledge Graph Domain | Primary Deck | Sub-Deck |
|---|---|---|
| Aqidah | Aqidah | Based on sub-category |
| Fiqh (Ibadat) | Fiqh | Specific worship category |
| Fiqh (Mu'amalat) | Fiqh | Transactions sub-deck |
| Hadith | Hadith | Collection-based sub-deck |
| Quran (Surah) | Quran | Surah names or key verses |
| Seerah | Seerah | Period-based sub-deck |
| Tarikh | Islamic History | Period-based sub-deck |
| Du'a | Du'a and Adhkar | Occasion-based sub-deck |

### 9.3 Custom Deck Features

| Feature | Description |
|---|---|
| **Manual Card Creation** | Learners can create their own cards from any content |
| **Deck Sharing** | Curated decks shared among learners (with permissions) |
| **Deck Merging** | Combine related decks for comprehensive review |
| **Smart Filters** | Filter cards by difficulty, tags, due date, performance |
| **Progress Dashboard** | Per-deck statistics: cards learned, retention rate, time spent |

---

## 10. STUDY SESSION MANAGEMENT

### 10.1 Session Types

| Session Type | Duration | Contents | Trigger |
|---|---|---|---|
| **Quick Review** | 5 minutes | Overdue cards only | Learner-initiated |
| **Standard Study** | 15-20 minutes | Overdue + due today + new cards | Scheduled or initiated |
| **Deep Study** | 30-60 minutes | All queue cards | Scheduled |
| **Cram Session** | Variable | Specific deck, all cards | Before a quiz |
| **Auto-Review** | 2-5 minutes | Daily notification review | Notification tap |

### 10.2 Session Flow

```
Session Start
    │
    ▼
┌──────────────────────────────┐
│ Review Queue (due cards)     │
│ • Process all overdue first  │
│ • Then due today cards       │
│ • Each card: see → recall →  │
│   reveal → rate              │
└──────────────────────────────┘
    │
    ▼
┌──────────────────────────────┐
│ New Cards (if within limit) │
│ • Learn new cards one by one│
│ • Each card: learn → recall │
│   → rate (learning steps)   │
└──────────────────────────────┘
    │
    ▼
┌──────────────────────────────┐
│ Session End                  │
│ • Summary: cards reviewed,   │
│   accuracy, time spent       │
│ • Next session recommended   │
│ • Sync progress              │
└──────────────────────────────┘
```

### 10.3 Daily Limits

| Limit Type | Default | Maximum | Rationale |
|---|---|---|---|
| **New Cards per Day** | 20 | 50 | Avoid overwhelming learners |
| **Review Cards per Day** | Unlimited | — | All due cards should be reviewed |
| **Session Time** | 20 minutes | 60 minutes | Optimal focused study duration |
| **Card Timeout** | 60 seconds | 120 seconds | Prevent overthinking |
| **Learning Step Retries** | 6 | 10 | Prevent endless repetition |

---

## 11. SYNCHRONIZATION AND OFFLINE

### 11.1 Sync Architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│                              SERVER                                     │
│  • Master card database                                                 │
│  • Spaced repetition state                                              │
│  • Deck definitions                                                     │
│  • Progress analytics                                                   │
└─────────────────────────────────────────────────────────────────────────┘
        ▲                     │
        │ sync                │ sync
        ▼                     ▼
┌──────────────────┐ ┌──────────────────┐
│  Client Device 1  │ │  Client Device 2  │
│  • Local cache    │ │  • Local cache    │
│  • Offline queue  │ │  • Offline queue  │
│  • Local SM-2     │ │  • Local SM-2     │
└──────────────────┘ └──────────────────┘
```

### 11.2 Offline Capabilities

| Feature | Online | Offline | Sync on Reconnect |
|---|---|---|---|
| **Review Cards** | ✓ | ✓ (cached) | Sync review results |
| **Learn New Cards** | ✓ | ✓ (pre-downloaded) | Sync learning progress |
| **Create Custom Cards** | ✓ | ✓ (local) | Sync on reconnect |
| **Media (Audio/Images)** | ✓ | ✓ (pre-cached) | Cache management |
| **Progress Dashboard** | ✓ | ✓ (local) | Sync stats |
| **Deck Updates** | ✓ | ✗ | Download on reconnect |

---

## 12. QUALITY METRICS

### 12.1 Card Quality Metrics

| Metric | Target | Measurement |
|---|---|---|
| **Factual Accuracy** | 100% | Automated Knowledge Graph verification |
| **Atomicity** | 100% | One fact per card verification |
| **Clarity** | > 95% | Learner comprehension survey |
| **Appropriate Difficulty** | 70-85% recall rate | Statistical analysis |
| **No Ambiguity** | 100% | Single unambiguous answer |
| **Source Citation** | 100% | All cards cite Knowledge Graph source |

### 12.2 Learner Performance Metrics

| Metric | Target | Use |
|---|---|---|
| **Retention Rate** | > 90% at 30 days | Spaced repetition effectiveness |
| **Cards Learned per Day** | 20-50 | Learning pace |
| **Review Completion Rate** | > 80% | Engagement |
| **Average Ease Factor** | 2.2-2.8 | Card difficulty calibration |
| **Time per Card** | 5-15 seconds | Card efficiency |

---

## 13. PERFORMANCE REQUIREMENTS

### 13.1 Generation Performance

| Operation | Target | Scale |
|---|---|---|
| **Single Card Generation** | < 1 second | Per fact |
| **Batch Deck Generation (100 cards)** | < 30 seconds | Per topic |
| **Bulk Generation (10,000 cards)** | < 1 hour | Full curriculum |

### 13.2 Runtime Performance

| Operation | Target |
|---|---|
| **Card Load** | < 100ms |
| **Review Submission** | < 50ms |
| **Sync (100 reviews)** | < 1 second |
| **Deck List Load** | < 200ms |
| **Search** | < 500ms |
| **Offline Card Cache** | < 1000 cards |

---

## 14. APPENDICES

### 14.1 Flashcard Templates

| Template ID | Type | Parameters |
|---|---|---|
| `basic_v2` | Basic | question, answer, source |
| `cloze_v1` | Cloze | sentence, blanks[], answers[] |
| `qa_v3` | Q&A | question, answer, explanation, source |
| `reverse_v1` | Reverse | prompt, recall, source |
| `image_v2` | Image | image_url, prompt, answer |
| `audio_v1` | Audio | audio_url, text, translation |

### 14.2 Related Documents

| Document ID | Title | Relationship |
|---|---|---|
| IEP-ARCH-CGEN-001 | Content Generation Architecture | Shared generation infrastructure |
| IEP-ARCH-KG-001 | Knowledge Graph Architecture | Fact source for cards |
| IEP-ARCH-VRF-001 | Verification Architecture | Card content verification |
| IEP-ARCH-QUIZ-001 | Quiz Mode Architecture | Companion assessment modality |
| IEP-ARCH-STR-001 | Story Mode Architecture | Story→Flashcard learning path |

### 14.3 Revision History

| Version | Date | Author | Changes |
|---|---|---|---|
| 1.0.0 | 2026-07-07 | Office of the Chief Architect | Initial approved release |
