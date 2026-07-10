# PODCAST ARCHITECTURE

## Audio Dialogue and Narrative Content Generation System

---

| Document ID | IEP-ARCH-POD-001 |
|---|---|
| Version | 1.0.0 |
| Status | Approved |
| Classification | Enterprise Internal |
| Effective Date | 2026-07-07 |
| Approving Authority | Office of the Chief Architect |
| Custodian | Audio Content Architecture Team |
| Content Generation Reference | IEP-ARCH-CGEN-001 |
| Knowledge Graph Reference | IEP-ARCH-KG-001 |
| RAG Reference | IEP-ARCH-RAG-001 |
| Verification Reference | IEP-ARCH-VRF-001 |

---

## TABLE OF CONTENTS

1. EXECUTIVE OVERVIEW
2. PODCAST PHILOSOPHY
3. PODCAST FORMATS
4. PODCAST GENERATION PIPELINE
5. SCRIPT ARCHITECTURE
6. SPEAKER AND VOICE SYSTEM
7. DIALOGUE GENERATION
8. NARRATION AND STORYTELLING
9. AUDIO PRODUCTION
10. TEXT-TO-SPEECH ENGINE
11. MULTI-LANGUAGE AUDIO
12. PODCAST SEQUENCING AND SERIES
13. QUALITY METRICS
14. PERFORMANCE REQUIREMENTS
15. APPENDICES

---

## 1. EXECUTIVE OVERVIEW

### 1.1 Purpose

Podcast Mode transforms Islamic knowledge into engaging audio content suitable for listening during commutes, household tasks, exercise, or dedicated study. It supports multiple formats including narrative storytelling, scholarly interviews, panel discussions, and lecture-style content, all grounded in verified Knowledge Graph facts.

### 1.2 Scope

This document covers the podcast generation pipeline, script architecture, dialogue generation, speaker and voice management, text-to-speech integration, audio production, and multi-language support.

### 1.3 Core Principle

**Every podcast episode must be factually accurate, engaging to listen to, and pedagogically valuable, with all claims traceable to the Knowledge Graph.**

---

## 2. PODCAST PHILOSOPHY

### 2.1 Design Principles

| Principle | Description |
|---|---|
| **Audio-First Design** | Content is designed specifically for listening, not adapted from text. |
| **Natural Dialogue** | Conversations sound natural, not like reading a script. |
| **Focused Episodes** | Each episode covers a single topic or story comprehensively. |
| **Respectful Tone** | Sacred topics are treated with appropriate reverence. |
| **Accessible Language** | Complex concepts explained in clear, conversational language. |
| **Cultural Authenticity** | Arabic terms pronounced correctly, cultural context preserved. |

### 2.2 Islamic Audio Traditions

| Tradition | Description | Podcast Application |
|---|---|---|
| **Qasas (Storytelling)** | Oral tradition of Islamic storytelling | Narrative podcast episodes |
| **Durus (Lessons)** | Traditional scholarly lectures | Lecture-style episodes |
| **Khutab (Sermons)** | Friday sermon format | Reflective episodes |
| **Halaqat (Study Circles)** | Group discussion format | Panel/interview episodes |
| **Madih (Praise)** | Poetic praise of the Prophet (ص) | Episodic intro/outro |

### 2.3 Listening Contexts

| Context | Typical Duration | Attention Level | Format Preference |
|---|---|---|---|
| **Commute** | 15-30 minutes | Medium | Narrative, Interview |
| **Household Tasks** | 10-20 minutes | Low | Narrative, Lecture |
| **Exercise/Walk** | 20-30 minutes | Medium | Interview, Narrative |
| **Dedicated Study** | 30-60 minutes | High | Lecture, Panel |
| **Before Sleep** | 5-15 minutes | Low | Reflective narrative |
| **Family Listening** | 10-20 minutes | Medium | Family-friendly narrative |

---

## 3. PODCAST FORMATS

### 3.1 Supported Formats

| Format | Description | Speakers | Duration | Best For |
|---|---|---|---|---|
| **Narrative Story** | Single narrator tells a story | 1 narrator | 10-30 min | Stories, biographies |
| **Hosted Narrative** | Host introduces and comments on a story | 1 host + narrator | 15-30 min | Educational stories |
| **Scholar Interview** | Host interviews a subject-matter expert | 1 host + 1 guest | 20-45 min | Deep dives |
| **Panel Discussion** | Multiple perspectives on a topic | 1 host + 2-3 guests | 30-60 min | Complex topics |
| **Lecture** | Single speaker presenting content | 1 lecturer | 15-45 min | Structured lessons |
| **Conversational** | Two hosts discussing a topic | 2 hosts | 15-30 min | Light learning |
| **Reflective** | Meditative content with Quran/du'a | 1 narrator | 5-15 min | Spiritual reflection |
| **Q&A** | Answering common questions | 1 host | 10-20 min | FAQ, common misconceptions |

### 3.2 Format Selection

```
Knowledge Graph Topic
    │
    ▼
┌──────────────────────────────┐
│ Format Selector              │
│ • Topic type                 │
│ • Target audience            │
│ • Intended listening context │
│ • Duration preference        │
└──────────────────────────────┘
    │
    ├── Biographical → Narrative Story / Hosted Narrative
    ├── Conceptual → Lecture / Conversational
    ├── Controversial/Debated → Panel Discussion
    ├── Practical → Q&A / Interview
    ├── Spiritual → Reflective
    └── Historical Event → Narrative Story / Hosted Narrative
```

---

## 4. PODCAST GENERATION PIPELINE

### 4.1 Pipeline Stages

```
┌─────────────────────────────────────────────────────────────────────────┐
│                     1. TOPIC SELECTION                                  │
│  • Query Knowledge Graph for topic cluster                             │
│  • Define learning objectives                                          │
│  • Select podcast format                                               │
└──────────────────────────────┬──────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                     2. SCRIPT PLANNING                                  │
│  • Outline episode structure                                           │
│  • Define speaker roles and voices                                     │
│  • Plan segment timing                                                 │
│  • Identify key facts to cover                                         │
└──────────────────────────────┬──────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                     3. SCRIPT GENERATION                                │
│  • Generate dialogue/narration                                         │
│  • Apply format-specific templates                                     │
│  • Integrate natural speech patterns                                   │
│  • Add verbal citations and references                                 │
└──────────────────────────────┬──────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                     4. VERIFICATION                                     │
│  • Factual accuracy check                                              │
│  • Script coherence review                                             │
│  • Tone and appropriateness check                                      │
│  • Timing validation                                                   │
└──────────────────────────────┬──────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                     5. AUDIO PRODUCTION                                 │
│  • TTS generation for each speaker                                     │
│  • Add sound effects and music                                         │
│  • Mix and master audio                                                │
│  • Apply pacing and emphasis                                           │
└──────────────────────────────┬──────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                     6. DELIVERY                                         │
│  • Generate show notes and metadata                                    │
│  • Create transcript                                                   │
│  • Publish to podcast feed                                             │
│  • Notify subscribers                                                  │
└─────────────────────────────────────────────────────────────────────────┘
```

### 4.2 Script Planning

| Planning Element | Description | Output |
|---|---|---|
| **Episode Title** | Catchy, descriptive title | "The Story of Prophet Musa: Faith Against Pharaoh" |
| **Episode Description** | Brief summary for show notes | "In this episode, we explore the story of Prophet Musa..." |
| **Learning Objectives** | What listeners should understand | Understand tawheed in the context of Pharaoh's tyranny |
| **Target Duration** | Planned length | 22 minutes |
| **Segment Plan** | Timing for each section | Intro: 2min, Story: 15min, Analysis: 3min, Outro: 2min |
| **Speaker Assignments** | Who speaks each part | Narrator: Story, Host: Intro/Analysis/Outro |
| **Key Facts** | Facts from Knowledge Graph | KG-FACT-004521, KG-FACT-004522, KG-FACT-004523 |
| **Quran/Hadith Quotes** | Direct quotes to include | Quran 20:9-98 |

---

## 5. SCRIPT ARCHITECTURE

### 5.1 Script Structure

```
┌─────────────────────────────────────────────────────────────────────────┐
│                      INTRODUCTION (1-3 minutes)                         │
│  • Opening hook or question                                             │
│  • Topic introduction                                                   │
│  • Speaker introductions                                                │
│  • Context setting                                                      │
└─────────────────────────────────────────────────────────────────────────┘
│
┌─────────────────────────────────────────────────────────────────────────┐
│                      MAIN CONTENT (80% of duration)                     │
│  Segment 1: Core narrative/exposition                                   │
│  Segment 2: Deeper analysis or related content                          │
│  Segment 3: Real-world application or reflection                        │
│  • Natural transitions between segments                                 │
│  • Quran verses and hadith quoted with context                          │
│  • Scholarly perspectives included                                      │
└─────────────────────────────────────────────────────────────────────────┘
│
┌─────────────────────────────────────────────────────────────────────────┐
│                      CONCLUSION (1-3 minutes)                            │
│  • Key takeaways summarized                                             │
│  • Reflection question for listeners                                    │
│  • Teaser for next episode                                              │
│  • Closing remarks and credits                                          │
└─────────────────────────────────────────────────────────────────────────┘
```

### 5.2 Segment Design

| Segment Type | Purpose | Duration | Content |
|---|---|---|---|
| **Hook** | Capture attention | 15-30s | Question, surprising fact, sound bite |
| **Context** | Set the stage | 1-2 min | Historical/cultural background |
| **Narrative** | Tell the story | 5-15 min | Story with dialogue and description |
| **Analysis** | Explain meaning | 2-5 min | Scholarly interpretation, lessons |
| **Application** | Make relevant | 1-3 min | How listener can apply this |
| **Reflection** | Deepen understanding | 1-2 min | Thought-provoking question |
| **Wrap-up** | Close the episode | 30-60s | Summary, preview, call-to-action |

### 5.3 Script Markup

The script includes markup for audio production:

| Markup | Meaning | Example |
|---|---|---|
| `[NARRATOR]` | Speaker identifier | `[NARRATOR] In the seventh century...` |
| `(pause)` | Brief pause | `(pause) And that is when everything changed.` |
| `(slow)` | Slow down delivery | `(slow) This is a profound lesson for all of us.` |
| `(emphasis)` | Emphasize the word | `(emphasis) Tawheed` is the foundation of our faith.` |
| `[SFX: ...]` | Sound effect | `[SFX: gentle wind sound]` |
| `[MUSIC: ...]` | Background music | `[MUSIC: reflective oud theme]` |
| `(QURAN: 20:9)` | Quran verse to be recited | `(QURAN: 20:9) And has there come to you the story of Musa?` |

---

## 6. SPEAKER AND VOICE SYSTEM

### 6.1 Speaker Archetypes

| Archetype | Voice Characteristics | Role |
|---|---|---|
| **Narrator** | Warm, authoritative, clear | Tells stories, provides descriptions |
| **Host** | Engaging, conversational, knowledgeable | Introduces topics, asks questions, provides analysis |
| **Scholar** | Learned, thoughtful, precise | Explains concepts, provides scholarly perspectives |
| **Student/Seeker** | Curious, earnest, relatable | Asks questions, represents the learner |
| **Historical Character** | Context-appropriate (dramatized) | Dialogue in narrative segments |

### 6.2 Voice Management

| Voice Property | Options | Selection Strategy |
|---|---|---|
| **Gender** | Male, Female | Based on speaker archetype and cultural appropriateness |
| **Age** | Young, Middle-aged, Mature | Match speaker role |
| **Accent** | Various Arabic dialects, regional English | Match content and target audience |
| **Tone** | Warm, Formal, Friendly, Authoritative | Match format and topic |
| **Speed** | Normal (150 wpm), Slow (120 wpm), Fast (170 wpm) | Based on content complexity |
| **Pitch** | Low, Medium, High | Natural variation for natural dialogue |

### 6.3 Voice Consistency

| Rule | Description |
|---|---|
| **Series Consistency** | Same voice(s) used across episodes in a series |
| **Speaker Distinction** | Each speaker archetype has clearly distinct voice |
| **Cultural Authenticity** | Arabic content uses voices with authentic Arabic pronunciation |
| **Tone Matching** | Voice tone matches content gravity (somber for serious topics, warm for inspirational) |
| **No Exaggeration** | Voices are natural and respectful, never cartoonish or overdramatized |

---

## 7. DIALOGUE GENERATION

### 7.1 Dialogue Principles

| Principle | Description |
|---|---|
| **Natural Speech Patterns** | Use contractions, filler words, incomplete sentences as natural speech does |
| **Turn-Taking** | Speakers alternate naturally, not in rigid Q&A format |
| **Active Listening Cues** | "I see," "That's interesting," "Can you explain more?" |
| **Reciprocal Questions** | Speakers ask each other questions |
| **Disagreement (Respectful)** | Scholars may differ; differences presented respectfully |
| **Humor (Appropriate)** | Gentle, respectful humor for appropriate topics |
| **Emotional Range** | Wonder, awe, reflection, concern — matching content |

### 7.2 Dialogue Generation Strategies

| Strategy | Description | Example |
|---|---|---|
| **Host-Expert** | Host asks questions, expert explains | "So what exactly does the term 'tawheed' mean?" |
| **Peer Discussion** | Two learners discuss and discover | "I always thought it meant something else." |
| **Narrator-Reflection** | Narrator tells, then reflects | "And that brings us to an important question..." |
| **Socratic Dialogue** | Questions lead to deeper understanding | "But why would Allah test the prophets so severely?" |
| **Debate (Respectful)** | Different viewpoints presented | "Some scholars take the view that..., while others argue..." |

### 7.3 Dialogue Pacing

| Element | Text | Audio | Purpose |
|---|---|---|---|
| **Question** | "What do you think?" | Rising intonation | Engage listener |
| **Explanation** | "Tawheed means..." | Steady, clear | Convey information |
| **Pause** | (pause) | Silence 1-2s | Let information sink in |
| **Emphasis** | "This is (emphasis) crucial" | Slower, stronger | Highlight importance |
| **Transition** | "Now, let's move to..." | Slightly faster | Move between segments |
| **Summary** | "So to recap..." | Clear, deliberate | Reinforce key points |

---

## 8. NARRATION AND STORYTELLING

### 8.1 Narrative Techniques for Audio

| Technique | Description | Application |
|---|---|---|
| **Sensory Description** | Describe sounds, smells, sights | "Imagine the heat of the desert sun..." |
| **Present Tense (Historical)** | Make history feel present | "Musa stands before Pharaoh..." |
| **Direct Address** | Speak directly to listener | "Picture yourself in that moment..." |
| **Rhetorical Questions** | Engage listener's mind | "What would you have done?" |
| **Suspense Building** | Create anticipation | "But what happened next would change everything..." |
| **Emotional Pacing** | Vary emotional intensity | Calm → tense → resolution → reflection |

### 8.2 Quran and Hadith Integration

| Element | Integration Method | Example |
|---|---|---|
| **Quran Verse Recitation** | Pre-recorded recitation by renowned qari | (recitation of Surah Al-Fatihah) |
| **Verse Translation** | Narrator reads translation after recitation | "This means: In the name of Allah..." |
| **Hadith Narration** | Narrator reads in Arabic then translates | "The Prophet (ص) said: 'Actions are judged by intentions.'" |
| **Context Explanation** | Scholar explains context of revelation | "This verse was revealed when..." |
| **Tafsir Insight** | Scholarly interpretation shared | "Ibn Kathir explains that this verse..." |

### 8.3 Storytelling Templates

| Template | Structure | Best For |
|---|---|---|
| **Linear Narrative** | Beginning → Middle → End | Biographies, historical events |
| **In Medias Res** | Start at climax → flashback → resolution | Dramatic stories |
| **Frame Story** | Present-day framing → historical narrative → return | Moral lessons with contemporary relevance |
| **Parallel Stories** | Two stories told in alternation → convergence | Comparative topics |
| **Question-Driven** | Start with a question → explore through story | Educational content |

---

## 9. AUDIO PRODUCTION

### 9.1 Audio Elements

| Element | Description | Source |
|---|---|---|
| **Speech** | Main content audio | TTS generation |
| **Quran Recitation** | Pre-recorded Quran verses | Licensed recitation database |
| **Background Music** | Ambient instrumental | Licensed Islamic instrumental library |
| **Sound Effects** | Environmental sounds | SFX library |
| **Intro/Outro Music** | Episode theme music | Custom composed or licensed |
| **Transition Sounds** | Segment change indicators | Short audio cues |

### 9.2 Music Guidelines

| Guideline | Description |
|---|---|
| **Instruments Only** | No vocals in background music (maintain focus on speech) |
| **Cultural Authenticity** | Preference for traditional Islamic instruments (oud, ney, qanun) |
| **Volume Level** | Background music at -20dB relative to speech |
| **No Competing Content** | Music fades during important speech, rises during transitions |
| **Mood Matching** | Reflective for spiritual content, uplifting for stories, calm for lectures |
| **No Distraction** | Music supports, never dominates the content |

### 9.3 Audio Mixing Parameters

| Parameter | Speech | Music | SFX | Quran |
|---|---|---|---|---|
| **Volume (dB)** | -3 to -6 | -20 to -25 | -15 to -20 | -3 to -6 |
| **EQ** | Clear mid-range (300-3000 Hz) | Roll off below 80 Hz | Varies | Full range |
| **Compression** | Light (2:1 ratio) | Moderate (3:1) | None | Light |
| **Reverb** | None (dry) | Light room reverb | Context-dependent | Hall reverb |
| **Stereo Position** | Center | Wide (L/R) | Context-dependent | Center |

### 9.4 Output Formats

| Format | Bitrate | Sample Rate | Use Case |
|---|---|---|---|
| **MP3** | 128 kbps | 44.1 kHz | Standard streaming |
| **AAC** | 96 kbps | 44.1 kHz | Apple Podcasts |
| **OGG** | 96 kbps | 44.1 kHz | Open-source platforms |
| **FLAC** | Lossless | 44.1 kHz | Archival, high-quality download |

---

## 10. TEXT-TO-SPEECH ENGINE

### 10.1 TTS Requirements

| Requirement | Specification |
|---|---|
| **Voice Quality** | Neural TTS (human-like) |
| **Arabic Support** | Modern Standard Arabic + Quranic recitation mode |
| **Language Support** | All platform-supported languages |
| **Emotional Range** | Neutral, warm, serious, inspirational |
| **Speed Control** | 0.5x to 2.0x |
| **Pitch Control** | ±20% |
| **SSML Support** | Full SSML for prosody control |

### 10.2 Arabic TTS Specialization

| Feature | Implementation |
|---|---|
| **Tajweed Rules** | Proper Quranic recitation rules applied automatically |
| **Diacritic-Aware** | Correct pronunciation based on tashkeel |
| **Madd (Elongation)** | Proper elongation of recitation vowels |
| **Ghunnah (Nasalization)** | Correct nasalization for noon and meem |
| **Ikhfa (Concealment)** | Proper tajweed rules for Quranic recitation |
| **Honorific Handling** | Correct pause and phrasing for ﷺ, صلى الله عليه وسلم, etc. |

### 10.3 SSML Markup

| Tag | Function | Example |
|---|---|---|
| `<prosody rate="slow">` | Control speaking speed | For important concepts |
| `<prosody pitch="high">` | Control pitch | For questions |
| `<break time="2s"/>` | Insert pause | For reflection |
| `<emphasis level="strong">` | Emphasize word | For key terms |
| `<say-as interpret-as="date">` | Format dates | For Islamic calendar dates |
| `<phoneme alphabet="ipa" ph="...">` | Custom pronunciation | For difficult Arabic terms |

---

## 11. MULTI-LANGUAGE AUDIO

### 11.1 Language Options

| Language | TTS Model | Arabic Preservation |
|---|---|---|
| **Arabic** | Neural Arabic TTS | Native (primary language) |
| **English** | Neural English TTS | Arabic terms pronounced correctly within English |
| **Urdu** | Neural Urdu TTS | Arabic script preserved |
| **Malay** | Neural Malay TTS | Arabic with Malay pronunciation |
| **Turkish** | Neural Turkish TTS | Arabic with Turkish pronunciation |
| **French** | Neural French TTS | Arabic with French pronunciation |
| **German** | Neural German TTS | Arabic with German pronunciation |

### 11.2 Code-Switching

When an episode includes Arabic terms within another language:

| Strategy | Implementation | Example (English) |
|---|---|---|
| **Full Arabic Pronunciation** | Arabic TTS segment for Arabic words | "The concept of توحيد (tawheed) is fundamental." |
| **Transliterated Pronunciation** | Phonetic approximation | "The concept of tawheed is fundamental." |
| **Dual Pronunciation** | Arabic first, then phonetic | "توحيد (tawheed) literally means..." |
| **Contextual Translation** | Translate, then use term | "Tawheed, the oneness of Allah, means..." |

---

## 12. PODCAST SEQUENCING AND SERIES

### 12.1 Series Types

| Series Type | Structure | Example |
|---|---|---|
| **Thematic Series** | Episodes around a theme | "Stories of the Prophets" |
| **Chronological Series** | Historical progression | "The Life of Prophet Muhammad (ص)" |
| **Concept Series** | Build understanding of a concept | "Understanding Tawheed" |
| **Book Study** | Chapter-by-chapter study | "Studying the Forty Hadith of Imam Nawawi" |
| **Seasonal Series** | Tied to Islamic calendar | "Ramadan Reflections" |

### 12.2 Episode Sequencing

| Episode | Content | Duration |
|---|---|---|
| **Episode 1: Introduction** | Why this topic matters | 10 min |
| **Episode 2: Foundation** | Core concepts and background | 20 min |
| **Episode 3: Deep Dive** | Detailed exploration | 30 min |
| **Episode 4: Application** | How to apply this knowledge | 20 min |
| **Episode 5: Reflection** | Review and reflection | 15 min |
| **Episode 6: Q&A** | Answering common questions | 25 min |

---

## 13. QUALITY METRICS

### 13.1 Content Quality Metrics

| Metric | Target | Measurement |
|---|---|---|
| **Factual Accuracy** | 100% | Knowledge Graph verification |
| **Script Coherence** | > 90% | Automated coherence scoring |
| **Natural Speech Flow** | > 85% | Listener satisfaction survey |
| **Audio Quality** | > 4.5/5 | Technical audio analysis |
| **Appropriate Pacing** | > 85% | Listener engagement metrics |
| **Pronunciation Accuracy** | > 95% | Arabic language expert review |

### 13.2 Listener Engagement Metrics

| Metric | Target | Use |
|---|---|---|
| **Episode Completion Rate** | > 70% | Content engagement |
| **Series Retention Rate** | > 50% across series | Series quality |
| **Average Listening Duration** | > 80% of episode | Content holding power |
| **Return Listener Rate** | > 40% | Overall show quality |
| **Share Rate** | > 5% of listeners | Content value perception |

---

## 14. PERFORMANCE REQUIREMENTS

### 14.1 Generation Performance

| Episode Duration | Script Generation | Audio Production | Total |
|---|---|---|---|
| **10 minutes** | < 2 minutes | < 5 minutes | < 7 minutes |
| **20 minutes** | < 4 minutes | < 8 minutes | < 12 minutes |
| **30 minutes** | < 6 minutes | < 12 minutes | < 18 minutes |
| **45 minutes** | < 10 minutes | < 18 minutes | < 28 minutes |

### 14.2 Storage Requirements

| Component | Storage per 20 min Episode |
|---|---|
| **Script** | 10-20 KB |
| **Audio (MP3, 128 kbps)** | ~20 MB |
| **Audio (AAC, 96 kbps)** | ~15 MB |
| **Show Notes + Metadata** | 5-10 KB |
| **Transcript** | 20-30 KB |

### 14.3 Streaming Performance

| Metric | Target |
|---|---|
| **Audio Start Time** | < 2 seconds |
| **Buffering** | < 1% of listening time |
| **Concurrent Streams** | 10,000+ |
| **Download Speed** | Full episode in < 30s on 5 Mbps |

---

## 15. APPENDICES

### 15.1 Podcast Templates

| Template ID | Format | Key Parameters |
|---|---|---|
| `narrative_story_v2` | Narrative Story | narrator_voice, topic_structure, duration, tone |
| `hosted_narrative_v1` | Hosted Narrative | host_voice, narrator_voice, analysis_depth |
| `scholar_interview_v2` | Scholar Interview | host_voice, scholar_voice, question_count |
| `panel_discussion_v1` | Panel Discussion | host_voice, guest_voices[], format_type |
| `lecture_v3` | Lecture | lecturer_voice, structure_type, interactivity_level |
| `conversational_v2` | Conversational | host1_voice, host2_voice, style |
| `reflective_v1` | Reflective | narrator_voice, includes_quran, duration |

### 15.2 Script Outline Template

```json
{
  "episode_id": "IEP-POD-004521",
  "title": "The Story of Prophet Musa: Faith Against Pharaoh",
  "format": "hosted_narrative",
  "duration_minutes": 22,
  "segments": [
    {
      "type": "intro",
      "duration": "2:00",
      "speaker": "host",
      "content": "Opening hook and topic introduction"
    },
    {
      "type": "narrative",
      "duration": "14:00",
      "speaker": "narrator",
      "content": "Main story with dialogue"
    },
    {
      "type": "analysis",
      "duration": "4:00",
      "speaker": "host",
      "content": "Key lessons and scholarly insights"
    },
    {
      "type": "outro",
      "duration": "2:00",
      "speaker": "host",
      "content": "Summary and next episode preview"
    }
  ],
  "voices": {
    "host": "voice_host_v2",
    "narrator": "voice_narrator_v3"
  },
  "facts": ["KG-FACT-004521", "KG-FACT-004522"],
  "quran_verses": ["20:9-98"],
  "hadith_references": ["Sahih Bukhari 1234"]
}
```

### 15.3 Related Documents

| Document ID | Title | Relationship |
|---|---|---|
| IEP-ARCH-CGEN-001 | Content Generation Architecture | Shared generation infrastructure |
| IEP-ARCH-KG-001 | Knowledge Graph Architecture | Fact source for episodes |
| IEP-ARCH-RAG-001 | RAG Architecture | Context assembly |
| IEP-ARCH-VRF-001 | Verification Architecture | Script verification |
| IEP-ARCH-STR-001 | Story Mode Architecture | Shared narrative techniques |
| IEP-ARCH-ATEACH-001 | AI Teacher Architecture | Conversational AI integration |

### 15.4 Revision History

| Version | Date | Author | Changes |
|---|---|---|---|
| 1.0.0 | 2026-07-07 | Office of the Chief Architect | Initial approved release |
