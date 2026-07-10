# LOCALIZATION ARCHITECTURE

## Internationalization and Multi-Language System

---

| Document ID | IEP-ARCH-LOC-001 |
|---|---|
| Version | 1.0.0 |
| Status | Approved |
| Classification | Enterprise Internal |
| Effective Date | 2026-07-07 |
| Approving Authority | Office of the Chief Architect |
| Custodian | Localization Architecture Team |

---

## TABLE OF CONTENTS

1. EXECUTIVE OVERVIEW
2. LOCALIZATION PHILOSOPHY
3. LANGUAGE STRATEGY
4. TRANSLATION WORKFLOW
5. STRING MANAGEMENT
6. RTL/LTR SUPPORT
7. CONTENT LOCALIZATION
8. CULTURAL ADAPTATION
9. QUALITY METRICS
10. APPENDICES

---

## 1. EXECUTIVE OVERVIEW

### 1.1 Purpose

The Localization Architecture enables the platform to serve learners in their native languages while preserving the accuracy and authenticity of Islamic Arabic content. It manages translation workflows, RTL layout, and cultural adaptation across all content and interface elements.

### 1.2 Core Principle

**Arabic is the source language for Islamic content; translations serve learning without replacing authenticity.**

---

## 2. LOCALIZATION PHILOSOPHY

| Principle | Description |
|---|---|
| **Arabic Source of Truth** | All Islamic content originates in Arabic; translations are referenced to Arabic |
| **Terminology Preservation** | Key Islamic terms (tawheed, salah, etc.) preserved in Arabic with explanation |
| **Cultural Context** | Translations account for cultural differences in Muslim communities worldwide |
| **Gradual Expansion** | Languages added based on user base size and scholarly resource availability |
| **Community Contributors** | Native speakers and Islamic scholars review translations |

---

## 3. LANGUAGE STRATEGY

### 3.1 Language Tiers

| Tier | Languages | Content Coverage | Timeline |
|---|---|---|---|
| **Tier 1** | Arabic, English | 100% | Launch |
| **Tier 2** | Urdu, Malay, Turkish | 100% | Q2 |
| **Tier 3** | French, German, Indonesian, Swahili | 80%+ | Q3 |
| **Tier 4** | Bengali, Russian, Chinese, Spanish, Somali | 60%+ | Q4 |

### 3.2 Language-Specific Considerations

| Language | Script | RTL | Special Handling |
|---|---|---|---|
| Arabic | Arabic | Yes | Quranic text, diacritics, tajweed |
| English | Latin | No | Transliteration of Arabic terms |
| Urdu | Arabic (Nastaliq) | Yes | Extended character set |
| Malay | Latin / Jawi | No/Yes | Jawi script support |
| Turkish | Latin | No | Modern Turkish orthography |
| French | Latin | No | French grammar rules for Arabic terms |
| Indonesian | Latin | No | Arabic loanword handling |

---

## 4. TRANSLATION WORKFLOW

### 4.1 Pipeline

```
Source Content (Arabic)
    │
    ▼
┌──────────────────────────────┐
│ 1. String Extraction         │
│ • Separate UI strings        │
│ • Separate content strings   │
│ • Identify Islamic terms     │
└──────────────────────────────┘
    │
    ▼
┌──────────────────────────────┐
│ 2. Machine Translation       │
│ • LLM-based translation      │
│ • Terminology enforcement    │
│ • Preserve Islamic terms     │
└──────────────────────────────┘
    │
    ▼
┌──────────────────────────────┐
│ 3. Human Review              │
│ • Native speaker review      │
│ • Scholar review (Islamic)   │
│ • Consistency check          │
└──────────────────────────────┘
    │
    ▼
┌──────────────────────────────┐
│ 4. Validation                │
│ • Placeholder check          │
│ • Length check               │
│ • RTL rendering test         │
└──────────────────────────────┘
    │
    ▼
Translation Ready
```

### 4.2 Islamic Term Preservation

| Category | Preserved in Arabic | Translated |
|---|---|---|
| **Allah's Names** | Yes (with translation in parentheses) | Never |
| **Quranic Terms** | Yes (tawheed, shirk, etc.) | Explanation only |
| **Prayer Terms** | Yes (salah, ruku, sujud) | Explanation only |
| **Scholarly Titles** | Yes (Imam, Hafiz, etc.) | Context-dependent |
| **General Concepts** | Yes with transliteration | Translation provided |
| **Instructions** | No | Fully translated |

---

## 5. STRING MANAGEMENT

### 5.1 i18n Infrastructure

| Component | Technology |
|---|---|
| **Framework** | i18next (JS) / GNU gettext (Python) |
| **Storage** | Lokalise / Crowdin (cloud), Git-based (locale files) |
| **Format** | JSON (web), YAML (backend), PO (Python) |
| **Key Convention** | `namespace.section.key` → `content.quiz.title` |

### 5.2 String Types

| Type | Example | Translation Method |
|---|---|---|
| **UI String** | "Save Changes" | Standard translation |
| **Content String** | "The five pillars of Islam are..." | Human-reviewed translation |
| **Islamic Term** | "tawheed" | Preserved + glossary link |
| **Dynamic String** | "{count} questions" | ICU message format |
| **Plural Form** | "{count} card(s)" | ICU plural rules |
| **Date/Time** | "July 7, 2026" | Locale-specific formatting |

---

## 6. RTL/LTR SUPPORT

### 6.1 Layout Strategy

| Element | RTL Behavior |
|---|---|
| **Text Alignment** | Right-aligned for RTL, left-aligned for LTR |
| **Navigation** | Sidebar on right for RTL |
| **Reading Order** | Character direction per Unicode Bidi Algorithm |
| **Mixed Text** | Arabic terms in LTR context use dir="auto" |
| **Icons** | Mirrored for RTL where meaningful (arrows, back) |
| **Forms** | Label → Input → Error (flipped for RTL) |

### 6.2 CSS Implementation

```css
/* Using logical properties instead of directional */
.element {
  padding-inline-start: 16px;
  margin-inline-end: 8px;
  border-inline-start: 2px solid;
}
```

---

## 7. CONTENT LOCALIZATION

### 7.1 Content Types

| Content | Localization Approach |
|---|---|
| **Stories** | Full translation + cultural adaptation of examples |
| **Quizzes** | Translated questions, localized answer options |
| **Flashcards** | Front/back in target language, Arabic terms preserved |
| **Podcasts** | Full script translation + new TTS in target language |
| **AI Teacher** | Conversational in target language, Arabic terms code-switched |

### 7.2 Cultural Adaptation

| Element | Adaptation |
|---|---|
| **Examples** | Replace with culturally relevant equivalents |
| **Names** | Use familiar names for the target culture |
| **Currency** | Local currency for zakat/charity examples |
| **Geography** | Local context for historical geography references |
| **Food/Clothing** | Culturally appropriate examples |

---

## 8. QUALITY METRICS

| Metric | Target |
|---|---|
| **Translation Coverage** | 100% of UI, > 80% of content |
| **Translation Accuracy** | > 99% (human-reviewed) |
| **Terminology Consistency** | 100% of Islamic terms preserved |
| **RTL Rendering** | Zero layout breakage |
| **Placeholder Accuracy** | 100% of variables interpolated correctly |
| **String Length Overflow** | < 1% cause UI breakage |

---

## 9. APPENDICES

### 9.1 Related Documents

| Document | Title | Relationship |
|---|---|---|
| IEP-ARCH-KG-001 | Knowledge Graph | Multi-lingual fact storage |
| IEP-ARCH-RAG-001 | RAG Architecture | Cross-lingual retrieval |

### 9.2 Revision History

| Version | Date | Author | Changes |
|---|---|---|---|
| 1.0.0 | 2026-07-09 | Office of the Chief Architect | Initial approved release |
