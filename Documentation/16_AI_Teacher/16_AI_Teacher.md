# AI TEACHER ARCHITECTURE

## Conversational Islamic Education Tutor

---

| Document ID | IEP-ARCH-ATEACH-001 |
|---|---|
| Version | 1.0.0 |
| Status | Approved |
| Classification | Enterprise Internal |
| Effective Date | 2026-07-07 |
| Approving Authority | Office of the Chief Architect |
| Custodian | Conversational AI Architecture Team |
| Content Generation Reference | IEP-ARCH-CGEN-001 |
| Knowledge Graph Reference | IEP-ARCH-KG-001 |
| RAG Reference | IEP-ARCH-RAG-001 |
| Verification Reference | IEP-ARCH-VRF-001 |

---

## TABLE OF CONTENTS

1. EXECUTIVE OVERVIEW
2. AI TEACHER PHILOSOPHY
3. CONVERSATION ARCHITECTURE
4. DIALOGUE MANAGEMENT
5. KNOWLEDGE GROUNDING
6. TEACHING STRATEGIES
7. QUESTION HANDLING
8. MULTI-TURN CONTEXT MANAGEMENT
9. PERSONALIZATION
10. SAFETY AND GUARDRAILS
11. QUALITY METRICS
12. PERFORMANCE REQUIREMENTS
13. APPENDICES

---

## 1. EXECUTIVE OVERVIEW

### 1.1 Purpose

The AI Teacher is the platform's conversational tutor that provides personalized, interactive Islamic education through natural language dialogue. It answers questions, explains concepts, guides learning paths, and adapts its teaching style to each learner's level, all while remaining strictly grounded in verified Knowledge Graph facts.

### 1.2 Core Principle

**The AI Teacher is a tutor, not an authority. It facilitates learning by guiding learners to verified knowledge, never by offering personal opinions or interpretations.**

---

## 2. AI TEACHER PHILOSOPHY

### 2.1 Role Definition

| Aspect | Role |
|---|---|
| **Primary Function** | Guide learners to understand verified Islamic knowledge |
| **Tone** | Patient, encouraging, respectful, knowledgeable |
| **Knowledge Boundary** | Strictly limited to Knowledge Graph verified facts |
| **Teaching Approach** | Socratic — guides learners to discover answers themselves |
| **Authority Limitation** | Never issues fatwas, never offers personal religious opinions |
| **Error Handling** | "I don't have enough verified information to answer that" rather than speculation |

### 2.2 Design Principles

| Principle | Description |
|---|---|
| **Knowledge-Gated** | Every response is grounded in Knowledge Graph facts with citations |
| **Learner-Centric** | Adapts to learner's level, interests, and learning style |
| **Progressive Disclosure** | Answers at appropriate depth for the learner's level |
| **Encouraging Curiosity** | Rewards questions and exploration |
| **Honest Uncertainty** | Clearly states when information is outside its knowledge boundary |
| **Respectful Always** | Maintains Islamic adab (etiquette) in all interactions |

---

## 3. CONVERSATION ARCHITECTURE

### 3.1 Conversation Pipeline

```
User Message
    │
    ▼
┌───────────────────────────────────────────────┐
│ 1. INPUT PROCESSING                            │
│ • Language detection                           │
│ • Intent classification                        │
│ • Entity extraction                            │
│ • Sentiment analysis                           │
│ • Safety check                                 │
└───────────────────────────────────────────────┘
    │
    ▼
┌───────────────────────────────────────────────┐
│ 2. KNOWLEDGE RETRIEVAL                         │
│ • Query Knowledge Graph                        │
│ • RAG context assembly                         │
│ • Fact selection and ranking                   │
│ • Context window management                    │
└───────────────────────────────────────────────┘
    │
    ▼
┌───────────────────────────────────────────────┐
│ 3. RESPONSE PLANNING                           │
│ • Teaching strategy selection                  │
│ • Response structure planning                  │
│ • Depth calibration                            │
│ • Follow-up question planning                  │
└───────────────────────────────────────────────┘
    │
    ▼
┌───────────────────────────────────────────────┐
│ 4. RESPONSE GENERATION                         │
│ • LLM response generation                      │
│ • Fact grounding enforcement                   │
│ • Citation injection                           │
│ • Tone and style application                   │
└───────────────────────────────────────────────┘
    │
    ▼
┌───────────────────────────────────────────────┐
│ 5. OUTPUT VERIFICATION                         │
│ • Factual consistency check                    │
│ • Safety check                                 │
│ • Citation validation                          │
│ • Quality scoring                              │
└───────────────────────────────────────────────┘
    │
    ▼
Response to User
```

### 3.2 Intent Classification

| Intent Category | Examples | Response Strategy |
|---|---|---|
| **Factual Question** | "What is tawheed?" | Direct factual answer with citation |
| **Conceptual Question** | "How is tawheed related to shirk?" | Explanation with examples |
| **Procedural Question** | "How do I perform wudu?" | Step-by-step guide |
| **Clarification Request** | "Can you explain that differently?" | Rephrase with simpler language |
| **Deep Dive Request** | "Tell me more about..." | Expand with additional related facts |
| **Personal Application** | "How can I apply this in my life?" | Practical guidance with examples |
| **Challenge/Test** | "What do you know about..." | Comprehensive knowledge display |
| **Off-Topic** | "What's the weather like?" | Polite redirect |
| **Sensitive Topic** | Questions about controversial issues | Escalate to scholarly sources |
| **Meta Question** | "How do you work?" | Explain AI Teacher capabilities |

---

## 4. DIALOGUE MANAGEMENT

### 4.1 Conversation State

| State Variable | Description | Example |
|---|---|---|
| **Topic** | Current Knowledge Graph topic | "Tawheed" |
| **Sub-Topic** | Current sub-topic | "Tawheed al-Uluhiyyah" |
| **Depth Level** | Current explanation depth (1-5) | 3 |
| **Learner Level** | Learner's knowledge level | Intermediate |
| **Interaction Count** | Messages in current session | 12 |
| **Follow-up Pending** | Awaiting learner response to follow-up | True |
| **Cited Facts** | Facts referenced in this session | [KG-FACT-001, KG-FACT-002] |
| **Teaching Strategy** | Current pedagogical approach | Socratic |

### 4.2 Dialogue Flow

```
┌─────────────────────────────────────────────────────────────────────────┐
│                      OPENING                                            │
│ Teacher: "Assalamu alaykum! What would you like to learn about today?" │
│ Learner: "Tell me about tawheed."                                      │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                      EXPLORATION                                        │
│ Teacher: "Tawheed is the foundation of our faith. It means the oneness │
│           of Allah. There are three categories. Do you know what they   │
│           are?"                                                         │
│ Learner: "No, tell me."                                                │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                      TEACHING                                           │
│ Teacher: "The three categories are: (1) Tawheed al-Rububiyyah — oneness │
│           of Lordship, (2) Tawheed al-Uluhiyyah — oneness of worship,   │
│           and (3) Tawheed al-Asma wa al-Sifat — oneness of names and    │
│           attributes. Would you like me to explain each one?"           │
│ Learner: "Yes, please."                                                │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                      DEEPENING                                          │
│ Teacher: "Let's start with Tawheed al-Rububiyyah. This means..."       │
└─────────────────────────────────────────────────────────────────────────┘
```

### 4.3 Context Window Management

| Component | Size Limit | Strategy |
|---|---|---|
| **Current Topic Context** | 4K tokens | Recent conversation turns |
| **Learner Profile** | 1K tokens | Level, interests, history summary |
| **RAG Context** | 8K tokens | Selected Knowledge Graph facts |
| **Teaching Strategy** | 500 tokens | Current pedagogical approach |
| **Total Context** | 16K tokens | Managed by sliding window |

---

## 5. KNOWLEDGE GROUNDING

### 5.1 Grounding Enforcement

| Mechanism | Description |
|---|---|
| **Fact-Only Context** | LLM receives only Knowledge Graph facts as context |
| **Citation Requirement** | Every factual claim must cite a Knowledge Graph fact ID |
| **Post-Generation Verification** | Response checked against source facts |
| **Boundary Enforcement** | "I only know what's in my Knowledge Graph" |
| **Uncertainty Expression** | "Based on the sources I have access to..." |

### 5.2 Citation Formats

| Context | Citation Format | Example |
|---|---|---|
| **Inline** | [Source: Fact ID] | "Tawheed means the oneness of Allah [KG-FACT-001]." |
| **Verbal** | "According to verified sources..." | "According to verified sources from the Quran, tawheed is..." |
| **Footnote** | Numbered references | "Tawheed has three categories¹. The first is..." |

---

## 6. TEACHING STRATEGIES

### 6.1 Strategy Library

| Strategy | Description | Best For |
|---|---|---|
| **Socratic** | Guide with questions to promote discovery | Conceptual understanding |
| **Direct Instruction** | Clear explanation with examples | Factual knowledge |
| **Analogy** | Use familiar concepts to explain new ones | Abstract concepts |
| **Storytelling** | Narrative-based teaching | Historical/religious narratives |
| **Scenario-Based** | Real-world application scenarios | Practical knowledge |
| **Comparison** | Compare/contrast with known concepts | Differentiation |
| **Chunking** | Break complex topics into small pieces | Complex subjects |
| **Spiral** | Revisit topics at increasing depth | Long-term retention |

### 6.2 Strategy Selection

```
Learner Question + Profile
    │
    ▼
┌──────────────────────────────┐
│ Strategy Selector            │
│ • Question type              │
│ • Learner level              │
│ • Topic characteristics      │
│ • Previous interactions      │
└──────────────────────────────┘
    │
    ├── "What is..." → Direct Instruction
    ├── "How is...related to..." → Comparison
    ├── "Why did..." → Storytelling or Socratic
    ├── "How do I..." → Scenario-Based
    ├── Abstract concept → Analogy
    └── Complex topic → Chunking → Spiral
```

---

## 7. QUESTION HANDLING

### 7.1 Question Types and Responses

| Question Type | Response Format | Depth |
|---|---|---|
| **Definition** | Concise definition + example + source | Level 1-2 |
| **Explanation** | Structured explanation with breakdown | Level 2-4 |
| **Comparison** | Side-by-side comparison table | Level 3-4 |
| **Application** | Scenario + guidance + principles | Level 3-5 |
| **Evidence** | Primary sources (Quran, Hadith) + scholarly consensus | Level 4-5 |
| **Counterargument** | Present multiple scholarly views fairly | Level 5 |

### 7.2 Handling Unknown Questions

| Scenario | Response |
|---|---|
| **Fact not in Knowledge Graph** | "I don't have verified information about that. I recommend consulting a qualified scholar." |
| **Ambiguous question** | "Could you clarify what you mean by...? I want to give you the most accurate answer." |
| **Beyond scope** | "That's an excellent question that requires deeper scholarly expertise. Let me connect you with our scholarly resources." |
| **Potential misconception** | "That's a common question. Let me share what our verified sources say about this." |

---

## 8. MULTI-TURN CONTEXT MANAGEMENT

### 8.1 Conversation Memory

| Memory Type | Duration | Content | Storage |
|---|---|---|---|
| **Session Memory** | Session lifetime | Current topic, recent turns, cited facts | In-memory |
| **Short-Term Memory** | 24 hours | Recent sessions, topics explored | Redis |
| **Long-Term Memory** | Permanent | Learner level, mastered topics, interests | PostgreSQL |
| **Fact Memory** | Permanent | Facts previously cited to this learner | PostgreSQL |

### 8.2 Follow-Up Generation

After answering, the AI Teacher generates follow-ups:

| Follow-up Type | Purpose | Example |
|---|---|---|
| **Check Understanding** | Verify comprehension | "Does that make sense?" |
| **Prompt Deeper Question** | Encourage exploration | "Would you like to learn about the opposite — shirk?" |
| **Suggest Application** | Practical connection | "How do you see this in daily life?" |
| **Link to Related Topic** | Broaden knowledge | "This connects to the concept of ihsan. Shall I explain?" |
| **Offer Quiz** | Test knowledge | "Would you like a quick quiz on what we just discussed?" |

---

## 9. PERSONALIZATION

### 9.1 Learner Model

| Attribute | Source | Use |
|---|---|---|
| **Knowledge Level** | Quiz results, AI Teacher interactions | Depth calibration |
| **Learning Style** | User preferences, interaction patterns | Strategy selection |
| **Interests** | Topics explored, questions asked | Topic recommendation |
| **Language Preference** | User setting | Language selection |
| **Age Group** | User profile | Age-appropriate language |
| **Madhhab (optional)** | User setting | School-specific details |
| **Learning Pace** | Speed of responses, review needs | Pacing adjustment |

### 9.2 Adaptation Rules

| Learner Signal | Adaptation |
|---|---|
| **Asks "why" frequently** | Use Socratic method, deeper explanations |
| **Struggles with a concept** | Simplify, use analogies, offer more examples |
| **Rapidly answers correctly** | Increase depth, introduce related concepts |
| **Prefers examples** | Lead with practical examples before theory |
| **Repeats questions** | Re-explain using different approach |

---

## 10. SAFETY AND GUARDRAILS

### 10.1 Content Safety Filters

| Filter | Description | Action |
|---|---|---|
| **Blasphemy** | Irreverent content about Allah, prophets, Islam | Block, polite warning |
| **Hate Speech** | Content attacking any group | Block, report |
| **Fatwa Requests** | Requests for religious rulings | Redirect to qualified scholars |
| **Medical/Legal Advice** | Requests outside Islamic education scope | Redirect to professionals |
| **Extremism** | Content promoting extremism | Block, report, escalate |
| **Inappropriate for Age** | Content above learner's age rating | Block, redirect to appropriate content |

### 10.2 Ethical Boundaries

| Boundary | Policy |
|---|---|
| **No Personal Opinion** | AI Teacher does not express personal religious opinions |
| **No Speculation** | Does not speculate on matters not in Knowledge Graph |
| **No Partiality** | Presents all mainstream scholarly views fairly |
| **Respectful Disagreement** | "Scholars have different views on this. According to the Hanafi school..., while the Shafi'i school holds..." |
| **Encourage Scholar Consultation** | For complex matters, encourages consulting human scholars |

---

## 11. QUALITY METRICS

| Metric | Target | Measurement |
|---|---|---|
| **Factual Accuracy** | 100% | Post-generation verification |
| **Response Relevance** | > 95% | Semantic similarity to query |
| **User Satisfaction** | > 4.5/5 | Post-interaction rating |
| **Conversation Continuation** | > 60% | Follow-up questions asked |
| **First Response Time** | < 2 seconds | Latency monitoring |
| **Safety Violations** | 0 | Automated monitoring |

---

## 12. PERFORMANCE REQUIREMENTS

| Metric | Target |
|---|---|
| **First Response Time** | < 2 seconds |
| **Follow-up Response Time** | < 1 second |
| **Concurrent Sessions** | 5,000+ |
| **Session Duration (avg)** | 10-15 minutes |
| **Context Window** | 16K tokens |
| **Knowledge Retrieval** | < 500ms |

---

## 13. APPENDICES

### 13.1 Related Documents

| Document ID | Title | Relationship |
|---|---|---|
| IEP-ARCH-CGEN-001 | Content Generation Architecture | Shared generation infrastructure |
| IEP-ARCH-KG-001 | Knowledge Graph Architecture | Fact source |
| IEP-ARCH-RAG-001 | RAG Architecture | Context assembly |
| IEP-ARCH-VRF-001 | Verification Architecture | Response verification |

### 13.2 Revision History

| Version | Date | Author | Changes |
|---|---|---|---|
| 1.0.0 | 2026-07-07 | Office of the Chief Architect | Initial approved release |
