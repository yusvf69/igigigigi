# QURAN ENGINE ARCHITECTURE

## Comprehensive Quranic Content Processing System

---

| Document ID | IEP-ARCH-QURAN-001 |
|---|---|
| Version | 1.0.0 |
| Status | Approved |
| Classification | Enterprise Internal |
| Effective Date | 2026-07-09 |
| Approving Authority | Office of the Chief Architect |
| Custodian | Quran Content Architecture Team |
| Knowledge Graph Reference | IEP-ARCH-KG-001 |
| OCR Reference | IEP-ARCH-OCR-001 |

---

## TABLE OF CONTENTS

1. EXECUTIVE OVERVIEW
2. QURAN ENGINE PHILOSOPHY
3. QURANIC DATA MODEL
4. RECITATION SYSTEMS (QIRA'AT)
5. RASM AL-UTHMANI HANDLING
6. AYAT INDEXING AND RETRIEVAL
7. TAJWEED RULES ENGINE
8. TAFSIR INTEGRATION
9. HIFDH (MEMORIZATION) TOOLS
10. QURAN SEARCH
11. QUALITY METRICS
12. APPENDICES

---

## 1. EXECUTIVE OVERVIEW

### 1.1 Purpose

The Quran Engine is a specialized subsystem that handles all Quranic content with the highest possible accuracy and reverence. It manages ayat text across all qira'at, rasm al-Uthmani orthography, tajweed rules, tafsir integration, hifdh tools, and search — treating the Quran as the platform's most sacred asset.

### 1.2 Core Principle

**The Quran is the literal word of Allah. Every character, diacritic, and recitation rule must be preserved with absolute fidelity.**

---

## 2. QURAN ENGINE PHILOSOPHY

| Principle | Description |
|---|---|
| **Complete Preservation** | Every qira'ah, riwayah, and rasm variant is preserved without preference |
| **Multi-Layer Validation** | Quranic text validated against multiple verified mushafs before being accepted |
| **Citation Precision** | Every ayah reference includes surah, ayah number, qira'ah, and riwayah |
| **Reverent Presentation** | Quranic text always displayed with proper formatting, never in isolation from context |
| **Tajweed Integration** | Recitation rules embedded in the text model, not added as an afterthought |

---

## 3. QURANIC DATA MODEL

### 3.1 Hierarchical Structure

```
Quran
├── Surah (114)
│   ├── Revelation Type (Makki/Madani)
│   ├── Verse Count
│   ├── Ayah (1-286 per surah)
│   │   ├── Text (per Qira'ah)
│   │   ├── Diacritics
│   │   ├── Tajweed Marks
│   │   ├── Sajdah Marker (if applicable)
│   │   ├── Hizb/Juz/Ruku Position
│   │   └── Tafsir Links
│   └── Hizb (60)
│       └── Juz (30)
│           └── Quarter (240)
│               └── Ruku (558)
├── Sajdah Signs (14 obligatory + ikhtilafi)
├── Manzil (7)
└── Para/Juz (30)
```

### 3.2 Core Tables

| Table | Description | Records |
|---|---|---|
| **Surah** | Surah metadata | 114 |
| **Ayah** | Ayah text per canonical qira'ah | ~6,236 (text) × 10 (qira'at) |
| **Word** | Individual word with morphology | ~77,430 per qira'ah |
| **Diacritic** | Tashkeel per character/word | ~500,000+ |
| **Tajweed Rule** | Applied tajweed rules per letter | ~1.5M+ rules |
| **Tafsir** | Tafsir text linked to ayat | By tafsir work |
| **Qira'ah** | Canonical recitation variants | 10 mutawatir qira'at |

### 3.3 Ayah Reference System

```
Standard: Surah:ayah → 2:255 (Al-Baqarah, Ayat al-Kursi)
Full:     Surah:ayah:qira'ah:riwayah → 2:255:Hafs:Asim
Extended: Surah:ayah:qira'ah:riwayah:rasm → 2:255:Hafs:Asim:Uthmani
```

---

## 4. RECITATION SYSTEMS (QIRA'AT)

### 4.1 Supported Qira'at

| Qira'ah | Imam | Narrators | Status |
|---|---|---|---|
| **Nafi'** | Nafi' al-Madani | Qalun, Warsh | Supported |
| **Ibn Kathir** | Ibn Kathir al-Makki | Al-Bazzi, Qunbul | Supported |
| **Abu Amr** | Abu Amr al-Basri | Al-Duri, Al-Susi | Supported |
| **Ibn Amir** | Ibn Amir al-Shami | Hisham, Ibn Dhakwan | Supported |
| **Asim** | Asim al-Kufi | Shu'bah, Hafs | Supported (Hafs = default) |
| **Hamzah** | Hamzah al-Kufi | Khalaf, Khallad | Supported |
| **Al-Kisa'i** | Al-Kisa'i al-Kufi | Al-Layth, Al-Duri | Supported |
| **Abu Ja'far** | Abu Ja'far al-Madani | Isa ibn Wardan, Ibn Jummaz | Supported |
| **Ya'qub** | Ya'qub al-Hadrami | Ruways, Rawh | Supported |
| **Khalaf** | Khalaf al-'Ashir | Ishaq, Idris | Supported |

### 4.2 Qira'ah Comparison Engine

| Feature | Description |
|---|---|
| **Text Diff** | Highlight differences between any two qira'at for a given ayah |
| **Pronunciation Guide** | Audio comparison of recitation differences |
| **Rule Explanation** | Scholarly explanation of why each qira'ah differs |
| **Chain of Narration** | Complete sanad for each riwayah |

---

## 5. RASM AL-UTHMANI HANDLING

| Feature | Description |
|---|---|
| **Rasm Types** | Uthmani (standard), Imla'i (modern spelling), Tajweed Mushaf |
| **Character Mapping** | Uthmani-specific characters (alif maqsurah, ta marboutah, etc.) |
| **Orthography Rules** | Deletion (hadhf), addition (ziyadah), substitution (ibdal) rules encoded |
| **Page Matching** | Standard page numbering for all major mushafs (604-page, Madinah, Indo-Pak) |
| **Rendering** | Specialized font rendering for Uthmani script with proper ligatures |

---

## 6. AYAT INDEXING AND RETRIEVAL

| Index | Method | Use Case |
|---|---|---|
| **Surah:Ayah** | B-tree | Direct reference lookup |
| **Topic** | Knowledge Graph entity links | Search by theme/subject |
| **Keyword** | Arabic root-based full-text search | Word/phrase search |
| **Semantic** | Vector embeddings | Meaning-based retrieval |
| **Tajweed Property** | Character-level tags | Find by recitation rule |
| **Tafsir Reference** | Cross-reference to tafsir works | Scholarly deep-dive |

---

## 7. TAJWEED RULES ENGINE

### 7.1 Rule Categories

| Category | Rules | Letters Affected |
|---|---|---|
| **Makharij** | Points of articulation | All letters |
| **Sifat** | Letter characteristics | All letters |
| **Noon Sakinah** | Izhar, Idgham, Iqlab, Ikhfa | ن + tanween |
| **Mim Sakinah** | Idgham Shafawi, Ikhfa Shafawi, Izhar Shafawi | م |
| **Madd** | 6 types of elongation | Alif, waw, ya |
| **Qalqalah** | Echoing letters | ق ط ب ج د |
| **Ghunnah** | Nasalization | ن, م (mushaddad) |
| **Rulings of Raa** | Tafkhim and Tarqiq of ر | ر |
| **Rulings of Laam** | Tafkhim of لا in Allah's name | ل |

### 7.2 Rule Application

```
Raw Text → Character Analysis → Rule Detection → Rule Application → Tajweed-Marked Text
    │              │                    │                 │
    ▼              ▼                    ▼                 ▼
"بسم"    → [ب, س, م] → Noon? No   → MMN rule on م   → "بِسْمِ" (no ghunnah)
```

---

## 8. TAFSIR INTEGRATION

| Feature | Description |
|---|---|
| **Tafsir Works** | Ibn Kathir, Al-Tabari, Al-Qurtubi, Al-Razi, Al-Sa'di, others |
| **Verse Links** | Each ayah linked to relevant tafsir passages |
| **Multi-Tafsir View** | Compare multiple tafsir works for the same ayah |
| **Tafsir Search** | Search within tafsir works |
| **Sabab al-Nuzul** | Context of revelation linked to each ayah |
| **Scholar Attribution** | Each tafsir clearly attributed to its author |

---

## 9. HIFDH (MEMORIZATION) TOOLS

| Tool | Description |
|---|---|
| **Ayah-by-Ayah Memorization** | Listen → Repeat → Record → Compare |
| **Mushafah (Page) Memorization** | Memorize by standard Mushaf page |
| **Surah Goal Tracking** | Progress tracking per surah |
| **Weakness Detection** | Identify frequently missed words |
| **Verse Recitation Check** | AI-based recitation accuracy assessment |
| **Tajweed Feedback** | Specific tajweed correction on recorded recitation |
| **Manzil Review** | Review a full manzil (1/7 of Quran) |
| **Peer Review** | Recite to a peer for verification |

---

## 10. QURAN SEARCH

| Search Type | Method | Example |
|---|---|---|
| **Exact Phrase** | Full-text index | "الرَّحْمَٰنِ الرَّحِيمِ" |
| **Root-Based** | Arabic triliteral root | search رحم → find all derivatives |
| **Semantic** | Vector embedding | "God's mercy" → finds رحمة |
| **Topic** | Knowledge Graph | "Prophets in the Quran" |
| **Tajweed Pattern** | Character-level index | "Find all idgham bighunnah" |

---

## 11. QUALITY METRICS

| Metric | Target |
|---|---|
| **Text Accuracy (Hafs)** | 100% (zero errors) |
| **Text Accuracy (All Qira'at)** | > 99.99% |
| **Diacritic Accuracy** | 100% |
| **Tajweed Rule Accuracy** | > 99.9% |
| **Tafsir Linking** | 100% of ayat linked to at least one tafsir |
| **Quran Search P50** | < 50ms |
| **Audio Sync Accuracy** | < 100ms offset |

---

## 12. APPENDICES

### 12.1 Related Documents

| Document | Title | Relationship |
|---|---|---|
| IEP-ARCH-KG-001 | Knowledge Graph | Quran domain in KG |
| IEP-ARCH-OCR-001 | OCR Architecture | Quranic OCR specialization |
| IEP-ARCH-RAG-001 | RAG Architecture | Quranic RAG retrieval |
| IEP-ARCH-SRCH-001 | Search Architecture | Quran search |

### 12.2 Revision History

| Version | Date | Author | Changes |
|---|---|---|---|
| 1.0.0 | 2026-07-09 | Office of the Chief Architect | Initial approved release |
