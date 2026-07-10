# HADITH ENGINE ARCHITECTURE

## Comprehensive Hadith Content Processing System

---

| Document ID | IEP-ARCH-HADITH-001 |
|---|---|
| Version | 1.0.0 |
| Status | Approved |
| Classification | Enterprise Internal |
| Effective Date | 2026-07-09 |
| Approving Authority | Office of the Chief Architect |
| Custodian | Hadith Content Architecture Team |
| Knowledge Graph Reference | IEP-ARCH-KG-001 |
| OCR Reference | IEP-ARCH-OCR-001 |

---

## TABLE OF CONTENTS

1. EXECUTIVE OVERVIEW
2. HADITH ENGINE PHILOSOPHY
3. HADITH DATA MODEL
4. COLLECTION SUPPORT
5. CHAIN OF NARRATION (SANAD) SYSTEM
6. HADITH GRADING
7. NARRATOR CRITICISM (AL-JARH WA AL-TA'DIL)
8. TAKHRIJ (CROSS-REFERENCING)
9. HADITH SEARCH
10. COMMENTARY AND EXPLANATION
11. QUALITY METRICS
12. APPENDICES

---

## 1. EXECUTIVE OVERVIEW

### 1.1 Purpose

The Hadith Engine is a specialized subsystem for processing, authenticating, cross-referencing, and presenting hadith literature. It manages chain of narration analysis, grading, narrator biographies, takhrij, and scholarly commentary with the rigor required for authoritative Islamic secondary sources.

### 1.2 Core Principle

**Every hadith presented includes its chain, grade, and source variations. The learner sees not just the text but its scholarly context.**

---

## 2. HADITH ENGINE PHILOSOPHY

| Principle | Description |
|---|---|
| **Full Chain Transparency** | No hadith presented without its sanad and grade |
| **Multi-Source Comparison** | Same hadith across collections shown side-by-side |
| **Grading Attribution** | Grades attributed to the scholar who gave them |
| **Narrator Integrity** | Complete narrator profiles with jarh/ta'dil |
| **Context Preservation** | Full chapter, book, and collection context |

---

## 3. HADITH DATA MODEL

```
Hadith
├── Sanad (Chain)
│   ├── Narrator 1 (from the Prophet ﷺ)
│   ├── Narrator 2
│   ├── ...
│   └── Narrator N (to the compiler)
│
├── Matn (Text)
│   ├── Arabic text (diacritized)
│   ├── Translation(s)
│   └── Variant readings
│
├── Collection Metadata
│   ├── Book
│   ├── Chapter
│   └── Hadith Number
│
├── Grading
│   ├── Per collection scholar
│   └── Per contemporary scholar
│
├── Narrator Links
│   └── Biography references
│
├── Takhrij
│   └── Cross-references to other collections
│
└── Commentary
    └── Sharh works
```

---

## 4. COLLECTION SUPPORT

### 4.1 Primary Collections

| Collection | Ahadith | Compiler | Grade Coverage |
|---|---|---|---|
| Sahih al-Bukhari | 7,275 | Imam al-Bukhari | Full (muttasil only) |
| Sahih Muslim | 5,500 | Imam Muslim | Full |
| Sunan Abi Dawud | 5,274 | Abu Dawud | Partial (implicit) |
| Sunan al-Tirmidhi | 4,400 | Al-Tirmidhi | Explicit per hadith |
| Sunan al-Nasai | 5,761 | Al-Nasai | Partial |
| Sunan Ibn Majah | 4,341 | Ibn Majah | Minimal |
| Musnad Ahmad | 27,000+ | Imam Ahmad | Minimal |
| Muwatta Malik | 1,720 | Imam Malik | No explicit grades |
| Sahih Ibn Khuzaymah | 3,000+ | Ibn Khuzaymah | Implicit |
| Sahih Ibn Hibban | 7,500+ | Ibn Hibban | Implicit |
| Mustadrak al-Hakim | 8,800+ | Al-Hakim | Implicit |
| Sunan al-Darimi | 3,500+ | Al-Darimi | Minimal |

### 4.2 Secondary Collections

| Collection | Description |
|---|---|
| Riyad al-Salihin | Compiled by Imam al-Nawawi |
| Bulugh al-Maram | Fiqh-oriented by Ibn Hajar |
| Al-Adab al-Mufrad | Manners by Imam al-Bukhari |
| Al-Arba'in al-Nawawiyyah | 40 hadith by Imam al-Nawawi |
| Mishkat al-Masabih | Wali al-Din al-Tabrizi |

---

## 5. CHAIN OF NARRATION (SANAD) SYSTEM

### 5.1 Chain Structure Analysis

```
Sanad Element                 Analysis
───────────────────────────────────────────────────────
حدثنا فلان                    Mode of transmission (sampling)
قال حدثنا فلان                Mode (reading/reciting)
عن فلان                       Mode (from)
عن فلان                       Mode (from)
عن النبي صلى الله عليه وسلم   Final authority
قال رسول الله صلى الله عليه وسلم
"النص الحديثي"               Matn (content)
```

### 5.2 Chain Analysis Engine

| Feature | Description |
|---|---|
| **Chain Continuity Check** | Detect missing narrators (inqita'), disconnected chains (munqati', mu'allaq, mu'dal) |
| **Narrator Overlap** | Identify multiple chains converging at a narrator |
| **Grade Prediction** | Predict grade based on chain quality metrics |
| **Chain Visualization** | Graphical chain representation |
| **Narrator Frequency** | How often each narrator appears in chains |
| **Parallel Chains** | Show all known chains for a hadith content |

---

## 6. HADITH GRADING

### 6.1 Classification Taxonomy

| Category | Sub-Categories | Description |
|---|---|---|
| **Sahih** | Sahih li-dhatihi, Sahih li-ghayrihi | Authentic chain + text |
| **Hasan** | Hasan li-dhatihi, Hasan li-ghayrihi | Good chain, minor weakness |
| **Da'if** | Da'if, Da'if jiddan | Weak chain |
| **Mawdu'** | Fabricated | Forged hadith |

### 6.2 Grading Sources

| Source | Type | Coverage |
|---|---|---|
| **Original Compiler** | Historical | Per hadith |
| **Al-Albani** | Contemporary | Extensive |
| **Ibn Hajar** | Classical | Fath al-Bari |
| **Al-Dhahabi** | Classical | Talkhis |
| **Al-Arna'ut** | Contemporary | Tahqiq of Musnad Ahmad |
| **Ibn Baz** | Contemporary | Limited |
| **Al-Albani School** | Contemporary | Various |

---

## 7. NARRATOR CRITICISM (AL-JARH WA AL-TA'DIL)

### 7.1 Narrator Data Model

| Field | Description | Example |
|---|---|---|
| **Full Name** | Complete name with lineage | أحمد بن حنبل الشيباني |
| **Kunyah** | Patronymic | أبو عبد الله |
| **Laqab** | Honorific | إمام أهل السنة |
| **Birth/Death** | AH dates | 164-241 AH |
| **Teachers** | List of narrators they narrated from | ~100+ |
| **Students** | List who narrated from them | ~200+ |
| **Ta'dil** | Positive criticism | ثقة، حجة، إمام |
| **Jarh** | Negative criticism | لا بأس به، صدوق |
| **Critics** | Who evaluated them | Ibn Hajar, Al-Dhahabi |
| **Consensus** | Level of scholarly agreement on their status | متفق على ثقته |

### 7.2 Jarh/Ta'dil Classification

| Level | Ta'dil | Jarh |
|---|---|---|
| **1** | ثقة/ثبت/حجة | كذاب/وضاع |
| **2** | صدوق/لا بأس به | متهم بالكذب |
| **3** | صدوق يخطئ/محله الصدق | متروك/ساقط |
| **4** | مقبول/صويلح | ضعيف/منكر الحديث |
| **5** | لين/فيه مقال | ليس بالقوي |
| **6** | Unknown (majhul) | Unknown severity |

---

## 8. TAKHRIJ (CROSS-REFERENCING)

| Feature | Description |
|---|---|
| **Matn Matching** | Find all instances of a hadith across collections |
| **Partial Match** | Find similar hadith with minor text variations |
| **Chain Cross-Reference** | Find hadith that share narrators |
| **Sahihain Coverage** | Identify Bukhari-Muslim agreement |
| **Shawahid/Mutaba'at** | Supporting chains and parallel narrations |
| **Takhrij Export** | Generate formal takhrij in scholarly format |

---

## 9. HADITH SEARCH

| Search Type | Method | Example |
|---|---|---|
| **Text** | Full-text Arabic + translation | "إنما الأعمال بالنيات" |
| **Topic** | Knowledge Graph topic links | "Sincerity (ikhlas)" |
| **Narrator** | Narrator name lookup | "البخاري" |
| **Grade** | Grade filter | "Sahih only" |
| **Collection** | Collection + hadith number | "Bukhari 1" |
| **Chain Analysis** | Search by chain structure | "Malik → Nafi' → Ibn Umar" |

---

## 10. COMMENTARY AND EXPLANATION

| Feature | Description |
|---|---|
| **Sharh Works** | Fath al-Bari, al-Minhaj, 'Awn al-Ma'bud, etc. |
| **Language** | Arabic commentary + translations |
| **Fiqh Extraction** | Juristic rulings derived from the hadith |
| **Lesson Summary** | Key takeaways in bullet points |
| **Contemporary Application** | Relevance to modern life |

---

## 11. QUALITY METRICS

| Metric | Target |
|---|---|
| **Text Accuracy** | 100% (against verified digital editions) |
| **Chain Accuracy** | 100% (narrator names and order) |
| **Grade Accuracy** | 100% (attributed to correct scholar) |
| **Collection Coverage** | All primary collections |
| **Narrator Coverage** | > 99% of narrators in primary chains |
| **Search P50** | < 100ms |

---

## 12. APPENDICES

### 12.1 Related Documents

| Document | Title | Relationship |
|---|---|---|
| IEP-ARCH-KG-001 | Knowledge Graph | Hadith domain in KG |
| IEP-ARCH-OCR-001 | OCR Architecture | Hadith OCR specialization |
| IEP-ARCH-SRCH-001 | Search Architecture | Hadith search |
| IEP-ARCH-VRF-001 | Verification | Hadith verification |

### 12.2 Revision History

| Version | Date | Author | Changes |
|---|---|---|---|
| 1.0.0 | 2026-07-09 | Office of the Chief Architect | Initial approved release |
