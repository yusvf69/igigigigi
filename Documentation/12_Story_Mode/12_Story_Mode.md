# STORY MODE ARCHITECTURE

## Narrative Educational Content Generation System

---

| Document ID | IEP-ARCH-STR-001 |
|---|---|
| Version | 1.0.0 |
| Status | Approved |
| Classification | Enterprise Internal |
| Effective Date | 2026-07-07 |
| Approving Authority | Office of the Chief Architect |
| Custodian | Story Content Architecture Team |
| Content Generation Reference | IEP-ARCH-CGEN-001 |
| Knowledge Graph Reference | IEP-ARCH-KG-001 |
| RAG Reference | IEP-ARCH-RAG-001 |

---

## TABLE OF CONTENTS

1. EXECUTIVE OVERVIEW
2. STORY MODE PHILOSOPHY
3. STORY TYPES
4. STORY GENERATION PIPELINE
5. NARRATIVE ARCHITECTURE
6. CHARACTER SYSTEM
7. PLOT GENERATION
8. SETTING AND WORLD-BUILDING
9. MORAL AND LESSON INTEGRATION
10. AGE-APPROPRIATE ADAPTATION
11. ILLUSTRATION AND VISUAL ELEMENTS
12. INTERACTIVE STORIES
13. STORY SERIES AND SEQUENCING
14. QUALITY METRICS
15. PERFORMANCE REQUIREMENTS
16. APPENDICES

---

## 1. EXECUTIVE OVERVIEW

### 1.1 Purpose

Story Mode transforms verified Islamic knowledge into engaging narrative experiences that educate, inspire, and instill Islamic values. Stories are the platform's primary modality for deep learning, leveraging the power of narrative to make abstract concepts concrete, memorable, and emotionally resonant.

### 1.2 Scope

This document covers the story generation system including narrative architecture, character generation, plot construction, moral integration, age adaptation, visual elements, and interactive storytelling features.

### 1.3 Core Principle

**Every story must be factually accurate, narratively engaging, and pedagogically purposeful.**

---

## 2. STORY MODE PHILOSOPHY

### 2.1 Islamic Narrative Traditions

| Tradition | Description | Application |
|---|---|---|
| **Qasas al-Quran** | Quranic storytelling with moral purpose | Primary source material |
| **Hadith Narratives** | Prophetic traditions with narrative elements | Secondary source material |
| **Sirah** | Prophetic biography | Biographical story generation |
| **Amthal** | Parables and analogies | Conceptual story generation |
| **Hikayat** | Classical Islamic tales | Cultural story enrichment |

### 2.2 Pedagogical Storytelling Principles

| Principle | Description |
|---|---|
| **Purposeful Narrative** | Every story has a clear learning objective derived from the Knowledge Graph. |
| **Character-Driven Learning** | Learners connect with protagonists who embody Islamic values. |
| **Conflict and Resolution** | Stories present moral challenges and model Islamically sound resolutions. |
| **Emotional Engagement** | Stories evoke empathy, wonder, and reflection. |
| **Progressive Revelation** | Complex concepts are revealed gradually through the narrative arc. |
| **Applicable Wisdom** | Stories end with clear takeaways applicable to the learner's life. |

### 2.3 Story Modalities

| Modality | Format | Interactivity | Best For |
|---|---|---|---|
| **Read-Along Story** | Text + narration audio | Low | Young children (6-9) |
| **Illustrated Story** | Text + images | Low | All ages |
| **Interactive Story** | Text + choices + branching | High | Older children (10-16) |
| **Animated Story** | Animated scenes + narration | Medium | All ages |
| **Audio Story** | Audio-only narrative | Low | Commuting, multitasking |

---

## 3. STORY TYPES

### 3.1 Story Type Taxonomy

| Type | Description | Fact Sources | Length |
|---|---|---|---|
| **Prophet Story** | Narrative about a prophet's life and mission | Quran, Hadith, Tafsir | 1500-5000 words |
| **Companion Story** | Story about a Sahabi (companion) | Hadith, Sirah, Tarikh | 1000-3000 words |
| **Historical Event** | Key event in Islamic history | Tarikh, scholarly works | 1500-4000 words |
| **Concept Story** | Story explaining an Islamic concept | Quran, Hadith, scholarly works | 500-2000 words |
| **Moral Tale** | Fictional story teaching a moral lesson | Derived from Islamic ethics | 500-1500 words |
| **Quran Story** | Story directly from Quranic narratives | Quran (with ayah references) | 1000-3000 words |
| **Hadith Story** | Story built around a hadith | Hadith (with reference) | 500-1500 words |
| **Parable** | Analogical story explaining complex ideas | Quran, scholarly works | 500-2000 words |
| **Daily Life Story** | Contemporary story applying Islamic principles | Derived from fiqh and akhlaq | 500-1500 words |

### 3.2 Story Type Selection Logic

```
Knowledge Graph Facts
    │
    ▼
┌──────────────────────────────┐
│ Story Type Classifier        │
│ • Analyze fact types         │
│ • Match to story type        │
│ • Consider learner profile   │
└──────────────────────────────┘
    │
    ├── Biographical Facts → Prophet Story / Companion Story
    ├── Event Facts → Historical Event
    ├── Conceptual Facts → Concept Story / Parable
    ├── Ethical Facts → Moral Tale / Daily Life Story
    ├── Quranic Facts → Quran Story
    └── Hadith Facts → Hadith Story
```

---

## 4. STORY GENERATION PIPELINE

### 4.1 Pipeline Stages

```
┌─────────────────────────────────────────────────────────────────────────┐
│                     1. FACT SELECTION                                   │
│  Select fact cluster from Knowledge Graph                              │
│  Determine story type and learning objective                           │
└──────────────────────────────┬──────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                     2. NARRATIVE PLANNING                               │
│  Define story structure (3-act, 5-act, etc.)                          │
│  Identify characters, setting, conflict, resolution                    │
└──────────────────────────────┬──────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                     3. CONTENT GENERATION                               │
│  Generate narrative following plan                                     │
│  Integrate facts, dialogue, description                                │
│  Maintain character consistency and narrative flow                     │
└──────────────────────────────┬──────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                     4. VERIFICATION                                     │
│  Factual accuracy check                                                │
│  Narrative coherence evaluation                                        │
│  Age-appropriateness verification                                      │
│  Islamic values alignment check                                        │
└──────────────────────────────┬──────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                     5. ENRICHMENT                                       │
│  Generate illustrations                                               │
│  Add vocabulary notes                                                  │
│  Create discussion questions                                           │
│  Generate narration audio                                              │
└──────────────────────────────┬──────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                     6. DELIVERY                                         │
│  Format for target modality                                            │
│  Cache for reuse                                                       │
│  Push to user feed                                                     │
└─────────────────────────────────────────────────────────────────────────┘
```

### 4.2 Narrative Planning

| Plan Component | Description | Generated By |
|---|---|---|
| **Story Type** | Classification of the story | Classifier model |
| **Learning Objective** | What the learner should take away | Knowledge Graph query |
| **Target Audience** | Age group and reading level | User profile |
| **Narrative Structure** | Story arc template | Template selector |
| **Character Set** | Protagonist, antagonist, supporting | Character generator |
| **Setting** | Time period, location, context | World-building engine |
| **Plot Points** | Key events in sequence | Plot generator |
| **Moral/Lesson** | The story's teaching point | Knowledge Graph extraction |

### 4.3 Generation Parameters

| Parameter | Values | Default | Impact |
|---|---|---|---|
| **Length** | short (500w), medium (1500w), long (3000w) | medium | Story depth and detail |
| **Complexity** | simple, moderate, complex | moderate | Vocabulary, sentence structure, concepts |
| **Narrative Style** | descriptive, dialogue-heavy, action-focused | descriptive | Storytelling approach |
| **Moral Emphasis** | subtle, moderate, explicit | moderate | How directly the lesson is stated |
| **Historical Accuracy** | strict, narrative-flexible | narrative-flexible | Adherence to historical details |
| **Imagination Scope** | fact-bound, creatively-enhanced | fact-bound | Room for creative elements |

---

## 5. NARRATIVE ARCHITECTURE

### 5.1 Three-Act Structure (Default)

```
Act 1: Setup                          Act 2: Confrontation              Act 3: Resolution
┌──────────────────────────┐          ┌──────────────────────────┐      ┌──────────────────────────┐
│ Exposition               │          │ Rising Action            │      │ Climax                   │
│ • Introduce setting      │    ──►   │ • Character faces        │ ──►  │ • Key moral decision      │
│ • Introduce protagonist  │          │   challenges             │      │ • Islamic values tested   │
│ • Present normal world   │          │ • Learning occurs        │      │                          │
│                          │          │ • Tension builds         │      │ Falling Action           │
│ Inciting Incident        │          │                          │      │ • Consequences shown      │
│ • Call to adventure      │          │                          │      │ • Lessons learned         │
│ • Moral challenge        │          │                          │      │                          │
│                          │          │                          │      │ Resolution               │
│                          │          │                          │      │ • New understanding       │
│                          │          │                          │      │ • Application to life     │
└──────────────────────────┘          └──────────────────────────┘      └──────────────────────────┘
```

### 5.2 Alternative Narrative Structures

| Structure | Best For | Description |
|---|---|---|
| **Three-Act** | Most stories | Setup → Confrontation → Resolution |
| **Five-Act** | Complex historical events | Exposition → Rising Action → Climax → Falling Action → Denouement |
| **Hero's Journey** | Prophet and companion stories | Call → Trials → Transformation → Return |
| **Framed Narrative** | Hadith and Quran stories | Story within a story |
| **Parallel Narrative** | Comparative concepts | Two storylines that converge |
| **Circular Narrative** | Reflective moral tales | Ends where it begins, with new understanding |
| **Episodic** | Long-form series | Connected but self-contained episodes |

### 5.3 Narrative Elements

| Element | Description | Implementation |
|---|---|---|
| **Hook** | Opening that captures attention | First paragraph generated to intrigue |
| **Inciting Incident** | Event that begins the story's action | Derived from fact cluster |
| **Conflict** | Central challenge or problem | Moral dilemma or external challenge |
| **Rising Action** | Escalating series of events | Progressive revelation of facts |
| **Climax** | Peak moment of decision or action | Key moral/faith moment |
| **Falling Action** | Consequences and reflection | Lessons internalized |
| **Resolution** | Conclusion and new state | Application to learner's life |
| **Theme** | Central idea or message | Learning objective |

---

## 6. CHARACTER SYSTEM

### 6.1 Character Types

| Type | Source | Examples | Generation Method |
|---|---|---|---|
| **Historical Figure** | Knowledge Graph (person entity) | Prophet Muhammad (ص), Abu Bakr (ر), Salahuddin | Direct from KG with authenticated biography |
| **Composite Character** | Aggregated from multiple sources | "A scholar in Baghdad" | Composite from authentic traits |
| **Fictional Protagonist** | Generated for moral tales | "A young boy learning about honesty" | Template-based with age-appropriate attributes |
| **Antagonist/Challenge** | Generated or historical | Temptation, difficult situation, historical adversary | Balanced, respectful portrayal |
| **Supporting Character** | Generated or historical | Family, teachers, friends | Complementary to protagonist |
| **Narrator** | Voice of the story | Third-person omniscient, first-person child | Consistent narrative voice |

### 6.2 Character Generation Rules

| Rule | Description | Enforcement |
|---|---|---|
| **Historical Accuracy** | Historical characters' words, actions, and traits match authenticated sources | Strict: only KG-verified attributes |
| **Respectful Portrayal** | Prophets and companions portrayed with utmost respect | Style guidelines, honorifics required |
| **No Speculation on Intent** | Never speculate about a historical figure's internal thoughts unless recorded | Verification check |
| **Balanced Villains** | Antagonists are not caricatures; their motivations are explained | Ethical portrayal guidelines |
| **Relatable Protagonists** | Fictional protagonists reflect the target age group | Age-matched characteristics |
| **Positive Representation** | Muslim characters are portrayed positively and authentically | Values alignment check |

### 6.3 Character Attributes

Each character in a story has:

| Attribute | Description | Example |
|---|---|---|
| **Name** | Character's name | "خالد بن الوليد" / "Khalid" |
| **Role** | Function in the story | Protagonist, mentor, antagonist |
| **Age** | Character's age | 12 years old |
| **Traits** | Personality characteristics | Brave, curious, honest |
| **Knowledge** | What the character knows | Knows about salah, learning about patience |
| **Motivation** | What drives the character | To learn the truth, to help others |
| **Arc** | How the character changes | From impatient to patient |

---

## 7. PLOT GENERATION

### 7.1 Plot Construction

| Stage | Input | Output | Method |
|---|---|---|---|
| **Plot Seed** | Fact cluster + story type | Core story premise | LLM + template |
| **Plot Points** | Core premise + narrative structure | Sequence of 5-10 key events | Plot generation algorithm |
| **Scene Breakdown** | Plot points + character set | Individual scene descriptions | Scene generation engine |
| **Full Narrative** | Scenes + characters + setting | Complete story text | LLM with guided generation |

### 7.2 Plot Templates

| Template | Structure | Best For |
|---|---|---|
| **Quest** | Character seeks knowledge/wisdom, faces trials, achieves understanding | Learning journeys |
| **Overcoming the Monster** | Character faces a challenge (external or internal) and overcomes through faith | Moral tales |
| **Voyage and Return** | Character experiences something new and returns transformed | Concept introduction |
| **Rebirth** | Character undergoes transformation through learning | Personal growth stories |
| **Tragedy** | Consequences of ignoring Islamic guidance | Cautionary tales |
| **Comedy** | Misunderstandings resolved through knowledge | Lighthearted lessons |

### 7.3 Pacing Guidelines

| Story Length | Total Scenes | Words per Scene | Reading Time |
|---|---|---|---|
| **Short (500w)** | 3-4 | 125-165 | 3-5 minutes |
| **Medium (1500w)** | 5-7 | 215-300 | 10-15 minutes |
| **Long (3000w)** | 8-12 | 250-375 | 20-30 minutes |

---

## 8. SETTING AND WORLD-BUILDING

### 8.1 Setting Types

| Setting Type | Source | Examples |
|---|---|---|
| **Historical** | Knowledge Graph (location + time) | Mecca 610 CE, Baghdad 800 CE, Andalusia 1200 CE |
| **Contemporary** | Modern world | A city, a school, a home today |
| **Abstract** | Conceptual space | "The Garden of Knowledge," "The Path of Life" |
| **Quranic** | Quranic descriptions | Jannah, Jahannam, natural phenomena described in Quran |
| **Imaginative** | Generated for parables | A village where honesty is currency |

### 8.2 World-Building Rules

| Rule | Description |
|---|---|
| **Historical Accuracy** | Historical settings must match verified historical facts |
| **Cultural Authenticity** | Cultural practices, dress, architecture match the period and region |
| **Sensory Details** | Settings described through sight, sound, smell, touch |
| **Purposeful Description** | Setting details serve the story and learning objective |
| **Age-Appropriate Detail** | Complexity of setting description matches age group |
| **No Anachronisms** | Modern elements not introduced into historical settings |

---

## 9. MORAL AND LESSON INTEGRATION

### 9.1 Lesson Integration Approaches

| Approach | Description | Best For |
|---|---|---|
| **Explicit Conclusion** | Story ends with stated moral/lesson | Young children (6-9) |
| **Character Reflection** | Character articulates what they learned | Older children (10-14) |
| **Socratic Ending** | Story ends with a question for the reader | Teens (15+) |
| **Embedded Wisdom** | Lesson woven throughout without explicit statement | All ages, sophisticated readers |
| **Discussion Prompt** | Story ends with conversation starters | Group learning, family reading |

### 9.2 Lesson Types

| Lesson Category | Examples | Knowledge Graph Source |
|---|---|---|
| **Aqidah (Faith)** | Tawheed, belief in prophets, Qadr | Aqidah knowledge domain |
| **Akhlaq (Character)** | Honesty, patience, gratitude, courage | Akhlaq knowledge domain |
| **Fiqh (Practice)** | Importance of salah, zakat, fasting | Fiqh knowledge domain |
| **Sirah (History)** | Lessons from prophetic biography | Sirah knowledge domain |
| **Tazkiyah (Purification)** | Self-reflection, repentance, mindfulness | Tazkiyah knowledge domain |

---

## 10. AGE-APPROPRIATE ADAPTATION

### 10.1 Age Group Parameters

| Age Group | Vocabulary | Sentence Length | Concept Depth | Story Length | Moral Explicitness |
|---|---|---|---|---|---|
| **6-8** | Basic (500 words) | 5-10 words | Concrete, simple | 500-800 words | Very explicit |
| **9-12** | Intermediate (2000 words) | 8-15 words | Concrete with some abstraction | 1000-2000 words | Moderately explicit |
| **13-15** | Advanced (5000 words) | 10-20 words | Abstract concepts | 1500-3000 words | Subtle + reflective |
| **16+** | Full adult vocabulary | Variable | Complex theological/philosophical | 2000-5000 words | Implicit + discussion |

### 10.2 Content Adaptation Rules

| Element | 6-8 | 9-12 | 13-15 | 16+ |
|---|---|---|---|---|
| **Death/Violence** | Not depicted, implied only | Mentioned but not detailed | May be described contextually | Handled with historical accuracy |
| **Complex Theology** | Simplified analogies | Basic explanations | Detailed exposition | Full theological depth |
| **Moral Dilemmas** | Simple right/wrong | Shades of gray introduced | Complex ethical reasoning | Nuanced discussion |
| **Historical Detail** | Key events only | Key events + context | Full narrative with details | Complete historical analysis |

---

## 11. ILLUSTRATION AND VISUAL ELEMENTS

### 11.1 Illustration Types

| Type | Description | Generation Method |
|---|---|---|
| **Scene Illustration** | Key moment from the story | Text-to-image (DALL-E, Midjourney) |
| **Character Portrait** | Main character visualization | Text-to-image with style guide |
| **Setting Illustration** | Location visualization | Text-to-image with historical research |
| **Concept Diagram** | Visual explanation of a concept | Diagram generation engine |
| **Map** | Geographic context of the story | Map generation from geographic data |
| **Timeline** | Chronological visualization | Timeline generation engine |

### 11.2 Image Generation Guidelines

| Guideline | Description |
|---|---|
| **Prophet Depiction** | No visual representation of Prophet Muhammad (ص); use symbols, light, or calligraphy |
| **Companion Depiction** | Faces respectfully obscured or depicted in approved historical illustration style |
| **Art Style** | Consistent style across all illustrations in a story |
| **Cultural Authenticity** | Architecture, clothing, and objects match historical period |
| **Age-Appropriateness** | No frightening or inappropriate imagery |
| **Quality Standard** | Minimum 1024x1024, photorealistic or high-quality illustration style |

### 11.3 Illustration Integration

| Integration Point | Description |
|---|---|
| **Cover Image** | Story title page illustration |
| **Scene Break** | Illustration at key narrative moments |
| **Character Introduction** | Portrait when character first appears |
| **Concept Visualization** | Diagram at point of teaching |
| **Chapter/Section Header** | Decorative element |
| **End Page** | Closing illustration with moral summary |

---

## 12. INTERACTIVE STORIES

### 12.1 Interactivity Types

| Type | Description | Complexity |
|---|---|---|
| **Choice Points** | Reader chooses what happens next | Low |
| **Branching Paths** | Different choices lead to different story branches | Medium |
| **Knowledge Checks** | Embedded questions that affect story progression | Medium |
| **Exploration** | Reader explores different aspects of a setting | High |
| **Role-Playing** | Reader takes on the role of a character | High |

### 12.2 Branching Story Architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│                        STORY START                                      │
└────────────────────────────────┬────────────────────────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                     SCENE 1: INCITING INCIDENT                          │
│  Include 2-3 choices at key decision point                              │
└────────────────────────────────┬────────────────────────────────────────┘
                                 │
            ┌────────────────────┼────────────────────┐
            ▼                    ▼                    ▼
┌─────────────────────┐ ┌─────────────────────┐ ┌─────────────────────┐
│ BRANCH A            │ │ BRANCH B            │ │ BRANCH C            │
│ Good choice         │ │ Better choice       │ │ Poor choice         │
│ Positive outcome    │ │ Best outcome        │ │ Learning opportunity│
│ Lesson reinforced   │ │ Lesson mastered     │ │ Try again option    │
└─────────────────────┘ └─────────────────────┘ └─────────────────────┘
            │                    │                    │
            └────────────────────┼────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                     CONVERGENCE POINT                                   │
│  Story continues regardless of branch                                 │
│  All branches teach the same core lesson                              │
└─────────────────────────────────────────────────────────────────────────┘
```

### 12.3 Choice Design Principles

| Principle | Description |
|---|---|
| **Meaningful Choices** | Each choice has visible consequences |
| **No Wrong Answers** | All branches teach the lesson; some paths are just more effective |
| **Reflective Feedback** | After a "poor" choice, explain why the alternative was better |
| **Age-Appropriate Complexity** | Number of choices matches age group (2 for young, 3-4 for older) |
| **Islamic Values Guidance** | Choices reflect realistic Islamic decision-making |

---

## 13. STORY SERIES AND SEQUENCING

### 13.1 Story Series Types

| Series Type | Description | Example |
|---|---|---|
| **Chronological** | Stories in historical order | Lives of the prophets in order |
| **Thematic** | Stories around a theme | Stories about patience |
| **Character** | Following one character | Life of Prophet Muhammad (ص) |
| **Concept-Building** | Progressive concept introduction | Iman → Islam → Ihsan series |
| **Curriculum-Aligned** | Matching educational curriculum | Grade 3 Islamic Studies stories |

### 13.2 Story Sequencing Rules

| Rule | Description |
|---|---|
| **Prerequisite Knowledge** | Earlier stories provide context for later ones |
| **Increasing Complexity** | Later stories assume knowledge from earlier ones |
| **Thematic Coherence** | Related stories are grouped and cross-referenced |
| **Pacing** | Mix of short and long stories to maintain engagement |
| **Review Elements** | Periodic stories that revisit and reinforce previous lessons |

---

## 14. QUALITY METRICS

### 14.1 Story Quality Dimensions

| Dimension | Weight | Metrics | Target |
|---|---|---|---|
| **Factual Accuracy** | 0.35 | All claims match Knowledge Graph facts | 100% |
| **Narrative Quality** | 0.20 | Coherence, pacing, character development, dialogue | Score ≥ 85 |
| **Pedagogical Value** | 0.20 | Learning objective achieved, lesson clarity | Score ≥ 85 |
| **Age Appropriateness** | 0.10 | Vocabulary, complexity, content suitability | 100% match |
| **Engagement** | 0.10 | Readability, emotional resonance, hook effectiveness | Score ≥ 80 |
| **Islamic Values** | 0.05 | Values alignment, respectful portrayals | 100% pass |

### 14.2 Story Review Criteria

| Criterion | Description | Pass/Fail |
|---|---|---|
| **Fact Check** | Every factual claim traceable to Knowledge Graph | All claims must pass |
| **Prophet Portrayal** | Respectful, accurate, honorifics included | Must pass |
| **No Speculation** | No invented details about historical figures | Must pass |
| **Moral Clarity** | Lesson is clear and Islamically sound | Must pass |
| **Age Appropriate** | Content, language, and themes suitable for target age | Must pass |
| **No Harmful Content** | Violence, fear, or negative content within acceptable bounds | Must pass |

---

## 15. PERFORMANCE REQUIREMENTS

### 15.1 Generation Performance

| Story Length | Generation Time | Cache Hit Rate Target | Cost per Story |
|---|---|---|---|
| **Short (500w)** | < 10 seconds | > 70% | $0.01-0.02 |
| **Medium (1500w)** | < 30 seconds | > 60% | $0.03-0.05 |
| **Long (3000w)** | < 2 minutes | > 50% | $0.06-0.10 |
| **Interactive** | < 5 minutes (pre-generated) | > 90% (component reuse) | $0.15-0.30 |

### 15.2 Storage Requirements

| Content Type | Storage per Story | Examples |
|---|---|---|
| **Text** | 5-30 KB | Story content |
| **Illustrations** | 500 KB - 2 MB | 3-10 images per story |
| **Audio Narration** | 5-20 MB | TTS narration |
| **Metadata** | 1-5 KB | Tags, facts, references |

---

## 16. APPENDICES

### 16.1 Story Template Examples

| Template ID | Type | Key Parameters |
|---|---|---|
| `prophet_story_v2` | Prophet Story | prophet_name, period, key_event, moral_focus |
| `companion_story_v1` | Companion Story | companion_name, relationship_to_prophet, key_contribution |
| `concept_story_v3` | Concept Story | concept_name, age_group, analogy_type |
| `moral_tale_v2` | Moral Tale | virtue, scenario, protagonist_age |
| `quran_story_v1` | Quran Story | surah, ayah_range, narrative_angle |
| `hadith_story_v2` | Hadith Story | hadith_reference, context, application |

### 16.2 Related Documents

| Document ID | Title | Relationship |
|---|---|---|
| IEP-ARCH-CGEN-001 | Content Generation Architecture | Shared generation infrastructure |
| IEP-ARCH-KG-001 | Knowledge Graph Architecture | Fact source for stories |
| IEP-ARCH-RAG-001 | RAG Architecture | Context assembly for story generation |
| IEP-ARCH-VRF-001 | Verification Architecture | Story content verification |
| IEP-ARCH-QUIZ-001 | Quiz Mode Architecture | Companion assessment modality |
| IEP-ARCH-FC-001 | Flashcard Architecture | Companion review modality |

### 16.3 Revision History

| Version | Date | Author | Changes |
|---|---|---|---|
| 1.0.0 | 2026-07-07 | Office of the Chief Architect | Initial approved release |
