# 03 — Dataset Bible

**Knowledge Factory — Dataset Management System**

| Field | Value |
|---|---|
| Document ID | 03-DATASET-BIBLE |
| Version | 1.0.0 |
| Status | Published |
| Last Modified | 2026-07-10 |
| Classification | Enterprise Internal |
| Domain | Data Engineering / Islamic Knowledge |
| Language | English (primary), Arabic (secondary) |
| License | Proprietary — Knowledge Factory Internal |
| Owner | Dataset Stewardship Committee |
| Approval Required | Chief Data Officer |

---

## Table of Contents

1. [Dataset Philosophy](#1-dataset-philosophy)
2. [Dataset Inventory](#2-dataset-inventory)
3. [Dataset Quality](#3-dataset-quality)
4. [Dataset Cleaning](#4-dataset-cleaning)
5. [Dataset Versioning](#5-dataset-versioning)
6. [Metadata](#6-metadata)
7. [Licensing](#7-licensing)
8. [Storage](#8-storage)
9. [Validation](#9-validation)
10. [Backup](#10-backup)
11. [Evolution](#11-evolution)

---

## 1. Dataset Philosophy

### 1.1 Foundational Principles

Data is the foundation upon which the Knowledge Factory is built. Every inference, every retrieval, every generated response, and every knowledge graph query derives its authority from the underlying datasets. If the data is corrupt, the system fails. If the data is biased, the system propagates bias. If the data is incomplete, the system is unreliable. Therefore, the Knowledge Factory adheres to an uncompromising dataset philosophy rooted in seven core principles.

### 1.2 The Seven Pillars of Dataset Stewardship

#### 1.2.1 Pillar One — High Quality

Every dataset ingested into the Knowledge Factory must meet or exceed measurable quality thresholds before it is promoted to production. Quality is not a one-time gate but a continuous attribute that is measured, monitored, and improved over the entire lifecycle of the dataset. No dataset is too valuable to reject if it fails quality standards. The cost of bad data propagating through the system is orders of magnitude higher than the cost of rejecting or fixing data at the point of ingestion.

Quality is measured across six dimensions: completeness, accuracy, consistency, timeliness, uniqueness, and validity. Each dimension is scored on a 0.0 to 1.0 scale. A composite quality score is derived as the weighted harmonic mean of all six dimensions. The minimum acceptable composite score for production datasets is 0.95. Datasets scoring below 0.80 are quarantined and flagged for remediation. Datasets scoring between 0.80 and 0.95 are admitted to staging but require a remediation plan with a defined timeline for reaching the 0.95 threshold.

Critical datasets — those containing primary religious source material such as Quran mushaf text, tafsir corpora, and hadith collections — require a composite quality score of 0.99 or higher. These datasets undergo additional human review by certified scholars before any quality score is finalized.

#### 1.2.2 Pillar Two — Ethically Sourced

The Knowledge Factory sources data exclusively through ethical channels. This means:

All data acquisition respects the intellectual property rights of original creators, publishers, and custodians. No dataset is acquired through web scraping that violates terms of service, copyright law, or data protection regulations. Where data is obtained from third-party providers, a formal data acquisition agreement is executed that specifies permitted uses, restrictions, and attribution requirements.

Data sourced from Islamic institutions, universities, and research centers is acquired through formal partnerships or memoranda of understanding. These agreements specify the scope of use, duration, and any restrictions on redistribution or commercial exploitation.

Data derived from user interactions is collected only with informed consent, in compliance with applicable privacy regulations including GDPR, CCPA, and similar frameworks. Users are informed of how their data will be used, what specific purpose it serves, and how long it will be retained.

No dataset contains personally identifiable information unless specifically authorized and anonymized according to industry best practices. Any dataset that inadvertently contains PII is immediately quarantined and subjected to PII redaction before being admitted to the pipeline.

#### 1.2.3 Pillar Three — Properly Licensed

Every dataset in the Knowledge Factory has an explicitly documented license. No dataset is used without a clear understanding of the permissions, restrictions, and obligations that govern its use. The license is recorded in the dataset metadata and is checked programmatically before any dataset is used in a processing pipeline or made accessible to agents.

Datasets with incompatible licenses are never combined in the same processing pipeline without a legal review. Dataset license compatibility is checked at pipeline creation time, at dataset update time, and at inference time when datasets are combined in agent contexts.

The licensing section of this document provides a complete compatibility matrix, detailed interpretations of each license type, and the procedures for handling license ambiguity or change.

#### 1.2.4 Pillar Four — Versioned

All datasets are versioned using a strict semantic versioning scheme. Every change to a dataset — addition of records, correction of errors, schema modifications, format changes — produces a new version. The version history is preserved indefinitely. Any agent or process that relied on a specific version of a dataset can always access that version, either through hot storage for recent versions or through cold storage archives for older versions.

Versioning is not optional. A dataset without a version identifier is not a dataset in the Knowledge Factory — it is a draft, a work-in-progress, or an artifact that has not yet been formally admitted to the system. Only versioned datasets are accessible through the Knowledge Factory API, the Agent Runtime, and the Knowledge Graph integration layer.

#### 1.2.5 Pillar Five — Documented

Every dataset is accompanied by comprehensive documentation. Documentation is not an afterthought but is created concurrently with the dataset and is updated whenever the dataset changes. Documentation covers all aspects of the dataset: its purpose, source, schema, quality metrics, known limitations, usage examples, and citation format.

Documentation is bilingual where appropriate, with at least English documentation for all datasets and Arabic documentation for datasets related to Islamic studies, Arabic language, and regional content.

The minimum documentation requirements are defined in the Metadata section of this document. Additional documentation, such as usage guides, integration examples, and dataset-specific data dictionaries, is created for complex datasets.

#### 1.2.6 Pillar Six — Accessible

Datasets are accessible to authorized agents and users through standardized interfaces. The Knowledge Factory provides a unified data access layer that abstracts away storage format, location, and retrieval mechanism. Agents interact with datasets through a consistent API that handles authentication, authorization, rate limiting, and version resolution.

Accessibility does not mean unrestricted access. The principle of least privilege applies to all dataset access. Agents are granted access only to the datasets they need for their specific function. Access is logged and audited. Unauthorized access attempts are detected, logged, and escalated.

Accessibility also means interoperability. Datasets are stored in open, well-documented formats wherever possible. Proprietary formats are used only when the source data cannot be obtained in an open format, and in such cases, a conversion pathway to an open format is documented and maintained.

#### 1.2.7 Pillar Seven — Responsible

The Knowledge Factory acknowledges its responsibility in handling data related to Islamic knowledge. The Quran is not merely data — it is the revealed word of Allah, and its handling requires the utmost care, respect, and accuracy. Hadith collections represent the prophetic tradition, and errors in hadith data could lead to incorrect religious rulings. The Knowledge Factory treats this responsibility with the seriousness it deserves.

Responsible data stewardship includes:

- Ensuring that Quranic text is verified by multiple independent checks (Qaris, huffaz, and automated comparison with established mushaf standards).
- Preserving complete isnad (chain of narration) data for hadith so that scholars can independently verify authenticity gradings.
- Maintaining provenance metadata that traces every record back to its original source so that the chain of custody is always transparent.
- Implementing access controls that prevent misuse of sensitive religious content, such as Quranic text in inappropriate contexts.
- Providing clear attribution for scholarly opinions in fiqh and tafsir datasets so that users can evaluate the authority behind each opinion.

The responsibility extends to the present and future. As the Knowledge Factory evolves, new datasets will be integrated, and existing datasets will be updated. The Dataset Stewardship Committee, composed of data engineers, Islamic scholars, legal experts, and ethicists, oversees the dataset lifecycle and ensures that these seven pillars are upheld.

### 1.3 Dataset Lifecycle Overview

Every dataset in the Knowledge Factory follows a defined lifecycle:

1. **Discovery** — Potential datasets are identified through research, partnerships, or user feedback.
2. **Assessment** — The dataset is evaluated for quality, licensing, ethical considerations, and alignment with Knowledge Factory goals.
3. **Acquisition** — The dataset is obtained through appropriate channels with proper licensing and attribution.
4. **Ingestion** — The dataset is loaded into the Knowledge Factory infrastructure, validated, and cleaned.
5. **Staging** — The dataset is available in a staging environment for testing and evaluation.
6. **Production** — The dataset passes quality gates and is promoted to production.
7. **Monitoring** — The dataset is continuously monitored for quality, usage, and relevance.
8. **Maintenance** — The dataset is updated, corrected, and enhanced according to the maintenance schedule.
9. **Deprecation** — When a dataset is superseded or becomes obsolete, it is formally deprecated.
10. **Archival** — Deprecated datasets are archived with full metadata and remain accessible for historical reference.

The lifecycle is not necessarily linear. A dataset may cycle between Maintenance and Production multiple times. A dataset that fails quality checks may return to Ingestion or Staging for remediation. A deprecated dataset may be reactivated if a new version becomes available or if the reasons for deprecation are resolved.


---

## 2. Dataset Inventory

### 2.1 Inventory Overview

The Knowledge Factory maintains a comprehensive inventory of all datasets used across the platform. This inventory is itself a dataset — the Dataset Registry — which is the authoritative source of truth for what datasets exist, what versions are available, where they are stored, who is responsible for them, and what their quality status is.

The Dataset Registry is implemented as a versioned, queryable metadata store that is accessible to all authorized agents and processes. Every dataset in the following inventory is registered in the Dataset Registry with its complete metadata as specified in the Metadata section of this document.

The inventory is organized into the following categories:

- Quran Datasets
- Hadith Datasets
- Scholarly Works Datasets
- Historical Datasets
- Arabic Language Datasets
- Educational Datasets
- Knowledge Graph Datasets
- Training Data Datasets

### 2.2 Quran Datasets

#### 2.2.1 DATASET-QR-001: Mushaf Al-Madinah (Hafs)

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-QR-001 |
| Dataset Name | Mushaf Al-Madinah — Hafs 'an Asim |
| Version | 2.1.0 |
| Source | King Fahd Glorious Quran Printing Complex |
| License | Public Domain (Saudi Government Publication) |
| Format | Plain text (Uthmani script), JSON, XML |
| Records | 114 surahs, 6,236 ayahs, ~77,430 words |
| Update Frequency | As needed (scholar corrections) |
| Quality Score | 0.9997 |
| Schema Version | 1.2 |
| Responsible Team | Quran Data Team |
| Curator | Senior Qari Review Board |
| Last Verified | 2026-06-15 |
| Storage Tier | Hot (CDN-replicated) |
| Backup Classification | Critical (Tier 1) |
| Languages | Arabic (Uthmani script) |
| Tags | quran, mushaf, hafs, asim, uthmani |
| Temporal Coverage | Revealed 610–632 CE |

#### 2.2.2 DATASET-QR-002: Mushaf Multiple Riwayat

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-QR-002 |
| Dataset Name | Mushaf Text — Multiple Qira'at (Riwayat) |
| Version | 1.3.1 |
| Source | Various (licensed from Islamic universities) |
| License | CC BY-NC-SA 4.0 |
| Format | JSON, Parquet |
| Records | 10 qira'at x 6,236 ayahs = 62,360 ayah records |
| Update Frequency | Quarterly |
| Quality Score | 0.994 |
| Schema Version | 1.1 |
| Responsible Team | Quran Data Team |
| Curator | Qira'at Research Center |
| Riwayat Included | Hafs, Warsh, Qalun, Al-Duri, Al-Susi, Khallaf, Shu'bah, Qunbul, Al-Bazzi, Ishaq |
| Storage Tier | Hot |
| Backup Classification | Critical (Tier 1) |
| Languages | Arabic |
| Tags | quran, qiraat, riwayat, hafs, warsh, qalun, duri, comparison |
| Temporal Coverage | Revealed 610–632 CE |

#### 2.2.3 DATASET-QR-003: Mushaf Tajweed Rules

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-QR-003 |
| Dataset Name | Tajweed Rules Annotated Mushaf |
| Version | 2.1.0 |
| Source | Derived from DATASET-QR-001 with scholarly annotation |
| License | CC BY-NC-SA 4.0 |
| Format | JSON (nested annotations), XML |
| Records | 6,236 ayahs with full tajweed markup |
| Update Frequency | Biannual |
| Quality Score | 0.996 |
| Schema Version | 1.3 |
| Responsible Team | Quran Data Team |
| Curator | Tajweed Scholars Committee |
| Tajweed Rules Covered | 62 rules including idgham, iqlab, ikhfa, ghunnah, madd, qalqalah, etc. |
| Storage Tier | Hot |
| Backup Classification | Critical (Tier 1) |
| Languages | Arabic with English rule labels |
| Tags | quran, tajweed, recitation, madd, ghunnah, idgham |

#### 2.2.4 DATASET-QR-004: Quranic Arabic Corpus

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-QR-004 |
| Dataset Name | Quranic Arabic Corpus — Morphological and Syntactic Analysis |
| Version | 1.7.2 |
| Source | Quranic Arabic Corpus project (University of Leeds), augmented by internal scholars |
| License | CC BY-NC-SA 4.0 |
| Format | XML, JSON, CSV |
| Records | ~77,430 words with full morphological analysis |
| Update Frequency | Semi-annual |
| Quality Score | 0.989 |
| Schema Version | 2.0 |
| Responsible Team | Arabic NLP Team |
| Curator | Arabic Linguistics Research Group |
| Annotation Depth | Root, lemma, POS, morphology, syntax, dependency relations |
| Storage Tier | Hot |
| Backup Classification | Critical (Tier 1) |
| Languages | Arabic, English (glosses) |
| Tags | quran, corpus, morphology, syntax, arabic, grammar, dependency, irab |

#### 2.2.5 DATASET-QR-005: Tafsir Al-Tabari

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-QR-005 |
| Dataset Name | Tafsir al-Tabari — Jami' al-Bayan 'an Ta'wil Ay al-Quran |
| Version | 1.0.0 |
| Source | Digitized from Dar al-Ma'arif printed edition (Cairo), verified against 3 manuscripts |
| License | CC BY-NC-ND 4.0 |
| Format | JSON, XML, plain text |
| Records | 30 juz, 114 surahs, 6,236 ayahs with full commentary |
| Volume | ~15 million words |
| Update Frequency | As needed (correction cycles) |
| Quality Score | 0.984 |
| Schema Version | 1.0 |
| Responsible Team | Tafsir Data Team |
| Curator | Senior Mufassir Review Board |
| Sections per Ayah | Up to 40 (multiple narrations and opinions per ayah) |
| Storage Tier | Hot |
| Backup Classification | Critical (Tier 1) |
| Languages | Arabic |
| Tags | tafsir, tabari, classical, narration, quran, commentary |
| Notes | Ibn Jarir al-Tabari (839–923 CE). Includes full asaneed. |

#### 2.2.6 DATASET-QR-006: Tafsir Ibn Kathir

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-QR-006 |
| Dataset Name | Tafsir Ibn Kathir |
| Version | 1.1.1 |
| Source | Digitized from Dar al-Salam print edition (Riyadh) |
| License | CC BY-NC-ND 4.0 |
| Format | JSON, plain text |
| Records | 6,236 ayahs with commentary |
| Volume | ~8 million words |
| Update Frequency | As needed (correction cycles) |
| Quality Score | 0.988 |
| Schema Version | 1.1 |
| Responsible Team | Tafsir Data Team |
| Curator | Senior Mufassir Review Board |
| Storage Tier | Hot |
| Backup Classification | Critical (Tier 1) |
| Languages | Arabic |
| Tags | tafsir, ibn-kathir, classical, narration, quran, commentary |
| Notes | Ismail ibn Kathir (1301–1373 CE) |

#### 2.2.7 DATASET-QR-007: Tafsir Al-Qurtubi

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-QR-007 |
| Dataset Name | Tafsir al-Qurtubi — Al-Jami' li-Ahkam al-Quran |
| Version | 1.0.0 |
| Source | Digitized from Dar al-Kutub al-Ilmiyyah edition (Beirut), verified against 2 manuscripts |
| License | CC BY-NC-ND 4.0 |
| Format | JSON, plain text |
| Records | 6,236 ayahs with commentary |
| Volume | ~12 million words |
| Update Frequency | As needed |
| Quality Score | 0.976 |
| Schema Version | 1.0 |
| Responsible Team | Tafsir Data Team |
| Curator | Senior Mufassir Review Board |
| Storage Tier | Warm |
| Backup Classification | Critical (Tier 1) |
| Languages | Arabic |
| Tags | tafsir, qurtubi, maliki, fiqh-ahkam, quran, commentary |
| Notes | Muhammad ibn Ahmad al-Qurtubi (1214–1273 CE). Known for fiqh-oriented commentary. |

#### 2.2.8 DATASET-QR-008: Tafsir Al-Razi (Mafatih al-Ghayb)

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-QR-008 |
| Dataset Name | Tafsir al-Razi — Mafatih al-Ghayb |
| Version | 0.9.0 |
| Source | Digitized from Dar al-Kutub al-Ilmiyyah edition (Beirut) |
| License | CC BY-NC-ND 4.0 |
| Format | JSON, plain text |
| Records | 6,236 ayahs with commentary |
| Volume | ~18 million words |
| Update Frequency | As needed |
| Quality Score | 0.959 |
| Schema Version | 1.0 |
| Responsible Team | Tafsir Data Team |
| Curator | Senior Mufassir Review Board |
| Status | Staging (quality below 0.99 threshold for critical dataset) |
| Storage Tier | Warm |
| Backup Classification | Critical (Tier 1) |
| Languages | Arabic |
| Tags | tafsir, razi, philosophical, kalam, quran, commentary |
| Notes | Fakhr al-Din al-Razi (1150–1210 CE). Known for philosophical depth. Remediation plan in progress. |

#### 2.2.9 DATASET-QR-009: Tafsir Al-Sa'di

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-QR-009 |
| Dataset Name | Tafsir al-Sa'di — Tayseer al-Kareem al-Rahman |
| Version | 1.0.0 |
| Source | Digitized from Dar al-Salam print edition (Riyadh) |
| License | CC BY-NC-ND 4.0 |
| Format | JSON, plain text |
| Records | 6,236 ayahs with commentary |
| Volume | ~4 million words |
| Update Frequency | As needed |
| Quality Score | 0.991 |
| Schema Version | 1.0 |
| Responsible Team | Tafsir Data Team |
| Curator | Senior Mufassir Review Board |
| Storage Tier | Hot |
| Backup Classification | Critical (Tier 1) |
| Languages | Arabic |
| Tags | tafsir, saadi, contemporary, quran, commentary |
| Notes | Abd al-Rahman al-Sa'di (1889–1957 CE). Contemporary tafsir, widely used. |

#### 2.2.10 DATASET-QR-010: Tafsir Ma'arif al-Quran

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-QR-010 |
| Dataset Name | Ma'arif al-Quran (Mufti Muhammad Shafi) |
| Version | 1.0.0 |
| Source | Digitized from Dar al-Ishaat publication (Karachi) |
| License | CC BY-NC-ND 4.0 |
| Format | JSON, plain text |
| Records | 6,236 ayahs with commentary |
| Volume | ~6 million words |
| Update Frequency | As needed |
| Quality Score | 0.983 |
| Schema Version | 1.0 |
| Responsible Team | Tafsir Data Team |
| Curator | Urdu/English Tafsir Review Panel |
| Languages | Urdu (primary), with English translation |
| Storage Tier | Warm |
| Backup Classification | Critical (Tier 1) |
| Tags | tafsir, marif, shafi, urdu, quran, commentary, hanafi |
| Notes | Mufti Muhammad Shafi Deobandi (1897–1976 CE). |

#### 2.2.11 DATASET-QR-011: Tafsir Jalalayn

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-QR-011 |
| Dataset Name | Tafsir al-Jalalayn |
| Version | 1.2.0 |
| Source | Multiple digitized editions verified against printed Egyptian Standard Edition |
| License | Public Domain (classical text, standard edition) |
| Format | JSON, plain text |
| Records | 6,236 ayahs with concise commentary |
| Volume | ~1.5 million words |
| Update Frequency | As needed |
| Quality Score | 0.995 |
| Schema Version | 1.1 |
| Responsible Team | Tafsir Data Team |
| Curator | Classical Texts Verification Unit |
| Storage Tier | Hot |
| Backup Classification | Critical (Tier 1) |
| Languages | Arabic |
| Tags | tafsir, jalalayn, classical, concise, quran, commentary |
| Notes | Jalal al-Din al-Mahalli (1389–1459 CE) and Jalal al-Din al-Suyuti (1445–1505 CE). |

#### 2.2.12 DATASET-QR-012: Tafsir Al-Munir

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-QR-012 |
| Dataset Name | Tafsir al-Munir (Wahbah al-Zuhayli) |
| Version | 1.0.0 |
| Source | Digitized from Dar al-Fikr publication (Damascus), author-approved edition |
| License | CC BY-NC-ND 4.0 (courtesy of Dar al-Fikr) |
| Format | JSON, plain text |
| Records | 6,236 ayahs with commentary |
| Volume | ~10 million words |
| Update Frequency | As needed |
| Quality Score | 0.981 |
| Schema Version | 1.0 |
| Responsible Team | Tafsir Data Team |
| Curator | Contemporary Tafsir Review Panel |
| Storage Tier | Warm |
| Backup Classification | Critical (Tier 1) |
| Languages | Arabic |
| Tags | tafsir, munir, zuhayli, contemporary, comparative, quran, commentary |
| Notes | Wahbah al-Zuhayli (1932–2015 CE). Contemporary comparative tafsir. |

#### 2.2.13 DATASET-QR-013: Tafsir Comprehensive Collection

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-QR-013 |
| Dataset Name | Comprehensive Tafsir Collection — Multi-Tafsir Comparison |
| Version | 1.0.0 |
| Source | Derived from DATASET-QR-005 through DATASET-QR-012 plus additional minor tafsirs |
| License | Mixed (individual tafsir licenses apply; aggregated under CC BY-NC-SA 4.0 for derived data) |
| Format | JSON (nested multi-commentary format), Parquet |
| Records | 6,236 ayahs x up to 20 tafsir commentaries |
| Update Frequency | Quarterly |
| Quality Score | 0.987 |
| Schema Version | 1.0 |
| Responsible Team | Tafsir Data Team |
| Curator | Tafsir Integration Working Group |
| Tafsirs Included | Tabari, Ibn Kathir, Qurtubi, Razi, Sa'di, Jalalayn, Munir, Ma'arif, Baghawi, Zamakhshari, Nasafi, Alusi, Shawkani, Ibn Ashur, Tantawi, Qasimi, Al-Azhar, Al-Maraghi, Al-Amin, Al-Mawdudi |
| Storage Tier | Hot |
| Backup Classification | Critical (Tier 1) |
| Languages | Arabic, selected tafsirs in English/Urdu |
| Tags | tafsir, comprehensive, multi-tafsir, comparison, quran, commentary |

#### 2.2.14 DATASET-QR-014: Quran Translations

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-QR-014 |
| Dataset Name | Quran Translations — Multilingual Collection |
| Version | 2.3.0 |
| Source | Various (licensed from publishers and open-source projects) |
| License | Mixed (see individual translation licenses) |
| Format | JSON, Parquet |
| Records | 6,236 ayahs in 45 languages |
| Update Frequency | Quarterly |
| Quality Score | 0.976 |
| Schema Version | 1.4 |
| Responsible Team | Quran Data Team |
| Curator | Translation Review Network |
| Languages | English (9 versions: Yusuf Ali, Pickthall, Sahih International, Hilali-Khan, Muhsin Khan, Arberry, Asad, Abdel Haleem, Ghazali), Urdu (6), French (4), Spanish (3), German (3), Indonesian (4), Malay (2), Turkish (5), Farsi (3), Russian (2), Chinese (2), Swahili (2), Bengali (2), Japanese (1), Korean (1), Tamil (1), and 18 additional languages |
| Storage Tier | Hot |
| Backup Classification | Critical (Tier 1) |
| Tags | quran, translation, multilingual, english, urdu, comparative |

#### 2.2.15 DATASET-QR-015: Quran Concordance

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-QR-015 |
| Dataset Name | Quranic Concordance — Complete Word Index |
| Version | 1.1.0 |
| Source | Derived from DATASET-QR-001 and DATASET-QR-004 |
| License | CC BY-SA 4.0 |
| Format | JSON, Parquet |
| Records | ~77,430 word occurrences indexed by root, lemma, and surface form |
| Update Frequency | Semi-annual |
| Quality Score | 0.994 |
| Schema Version | 1.2 |
| Responsible Team | Arabic NLP Team |
| Curator | Lexicography Research Unit |
| Storage Tier | Hot |
| Backup Classification | Standard (Tier 2) |
| Languages | Arabic, English |
| Tags | quran, concordance, index, word, root, lemma, search |

#### 2.2.16 DATASET-QR-016: Quran Recitation Metadata

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-QR-016 |
| Dataset Name | Quran Recitation Metadata — Audio Alignment |
| Version | 1.0.0 |
| Source | Multiple reciters, time-aligned internally |
| License | CC BY-NC-SA 4.0 |
| Format | JSON (timestamp alignments), XML |
| Records | 114 surahs x 10 reciters = 1,140 recitation alignments |
| Update Frequency | Quarterly (new reciters added) |
| Quality Score | 0.972 |
| Schema Version | 1.0 |
| Responsible Team | Quran Data Team |
| Curator | Recitation Alignment Project |
| Reciters | Sudais, Shuraym, Abdul Basit (murattal & mujawwad), Al-Minshawi, Al-Husary, Al-Afasi, Ghazali, Juhany, Al-Ajmi, and 15 more |
| Storage Tier | Warm |
| Backup Classification | Standard (Tier 2) |
| Languages | N/A (metadata only; audio stored separately in media storage) |
| Tags | quran, recitation, audio, timestamp, alignment, sudais, minshawi |

### 2.3 Hadith Datasets

#### 2.3.1 DATASET-HT-001: Sahih al-Bukhari

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-HT-001 |
| Dataset Name | Sahih al-Bukhari — Full Text with Grading and Isnad |
| Version | 2.2.0 |
| Source | Digitized from Dar al-Taqwa publication (Cairo), verified against 3 manuscript editions |
| License | CC BY-NC-ND 4.0 |
| Format | JSON, XML, Parquet |
| Records | 7,563 hadith (including repeats), ~2,602 unique hadith |
| Update Frequency | As needed |
| Quality Score | 0.997 |
| Schema Version | 1.4 |
| Responsible Team | Hadith Data Team |
| Curator | Hadith Verification Board |
| Grade Included | Sahih (original grading by al-Bukhari). Internal verification grade also recorded. |
| Chapters | 97 kitab, 3,450 bab |
| Isnad Depth | Full chain from Bukhari to Prophet Muhammad (s) |
| Storage Tier | Hot (CDN-replicated) |
| Backup Classification | Critical (Tier 1) |
| Languages | Arabic (text), English (translation from Dar al-Salam edition) |
| Tags | hadith, bukhari, sahih, sunni, canonical |

#### 2.3.2 DATASET-HT-002: Sahih Muslim

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-HT-002 |
| Dataset Name | Sahih Muslim — Full Text with Grading and Isnad |
| Version | 2.1.0 |
| Source | Digitized from Dar al-Taqwa publication (Cairo), verified against 2 manuscript editions |
| License | CC BY-NC-ND 4.0 |
| Format | JSON, XML, Parquet |
| Records | 7,490 hadith (including repeats), ~2,200 unique hadith |
| Update Frequency | As needed |
| Quality Score | 0.996 |
| Schema Version | 1.4 |
| Responsible Team | Hadith Data Team |
| Curator | Hadith Verification Board |
| Grade Included | Sahih (original grading by Muslim). Internal verification grade also recorded. |
| Chapters | 56 kitab, 1,362 bab |
| Isnad Depth | Full chain from Muslim to Prophet Muhammad (s) |
| Storage Tier | Hot (CDN-replicated) |
| Backup Classification | Critical (Tier 1) |
| Languages | Arabic (text), English (translation) |
| Tags | hadith, muslim, sahih, sunni, canonical |

#### 2.3.3 DATASET-HT-003: Sunan Abi Dawud

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-HT-003 |
| Dataset Name | Sunan Abi Dawud — Full Text with Grading and Isnad |
| Version | 2.1.0 |
| Source | Digitized from Dar al-Taqwa publication (Cairo) |
| License | CC BY-NC-ND 4.0 |
| Format | JSON, XML, Parquet |
| Records | 5,274 hadith |
| Update Frequency | As needed |
| Quality Score | 0.994 |
| Schema Version | 1.4 |
| Responsible Team | Hadith Data Team |
| Curator | Hadith Verification Board |
| Grade Included | Original grading by Abu Dawud (mentioned gradings). Internal verification grade also recorded. |
| Chapters | 46 kitab |
| Isnad Depth | Full chain |
| Storage Tier | Hot |
| Backup Classification | Critical (Tier 1) |
| Languages | Arabic (text), English (translation) |
| Tags | hadith, abu-dawud, sunan, sunni, canonical |

#### 2.3.4 DATASET-HT-004: Jami' al-Tirmidhi

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-HT-004 |
| Dataset Name | Jami' al-Tirmidhi — Full Text with Grading and Isnad |
| Version | 2.1.0 |
| Source | Digitized from Dar al-Taqwa publication (Cairo) |
| License | CC BY-NC-ND 4.0 |
| Format | JSON, XML, Parquet |
| Records | 3,956 hadith |
| Update Frequency | As needed |
| Quality Score | 0.995 |
| Schema Version | 1.4 |
| Responsible Team | Hadith Data Team |
| Curator | Hadith Verification Board |
| Grade Included | Original grading by al-Tirmidhi (hasan, sahih, gharib, etc.). Internal verification grade also recorded. |
| Chapters | 50 kitab |
| Isnad Depth | Full chain |
| Storage Tier | Hot |
| Backup Classification | Critical (Tier 1) |
| Languages | Arabic (text), English (translation) |
| Tags | hadith, tirmidhi, jami, discussion, canonical |

#### 2.3.5 DATASET-HT-005: Sunan al-Nasai (Al-Mujtaba)

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-HT-005 |
| Dataset Name | Sunan al-Nasai — Al-Mujtaba (full text with grading and isnad) |
| Version | 2.1.0 |
| Source | Digitized from Dar al-Taqwa publication (Cairo) |
| License | CC BY-NC-ND 4.0 |
| Format | JSON, XML, Parquet |
| Records | 5,761 hadith |
| Update Frequency | As needed |
| Quality Score | 0.994 |
| Schema Version | 1.4 |
| Responsible Team | Hadith Data Team |
| Curator | Hadith Verification Board |
| Grade Included | Original grading by al-Nasai. Internal verification grade also recorded. |
| Chapters | 51 kitab |
| Isnad Depth | Full chain |
| Storage Tier | Hot |
| Backup Classification | Critical (Tier 1) |
| Languages | Arabic (text), English (translation) |
| Tags | hadith, nasai, sunan, canonical |

#### 2.3.6 DATASET-HT-006: Sunan Ibn Majah

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-HT-006 |
| Dataset Name | Sunan Ibn Majah — Full Text with Grading and Isnad |
| Version | 2.1.0 |
| Source | Digitized from Dar al-Taqwa publication (Cairo) |
| License | CC BY-NC-ND 4.0 |
| Format | JSON, XML, Parquet |
| Records | 4,341 hadith |
| Update Frequency | As needed |
| Quality Score | 0.993 |
| Schema Version | 1.4 |
| Responsible Team | Hadith Data Team |
| Curator | Hadith Verification Board |
| Grade Included | Internal verification grade (Ibn Majah did not systematically grade). |
| Chapters | 37 kitab |
| Isnad Depth | Full chain |
| Storage Tier | Hot |
| Backup Classification | Critical (Tier 1) |
| Languages | Arabic (text), English (translation) |
| Tags | hadith, ibn-majah, sunan, canonical |

#### 2.3.7 DATASET-HT-007: Musnad Ahmad ibn Hanbal

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-HT-007 |
| Dataset Name | Musnad Ahmad ibn Hanbal — Full Text with Grading and Isnad |
| Version | 2.0.1 |
| Source | Digitized from Dar al-Hadith publication (Cairo) |
| License | CC BY-NC-ND 4.0 |
| Format | JSON, XML, Parquet |
| Records | ~27,000 hadith (including repeats) |
| Update Frequency | As needed |
| Quality Score | 0.991 |
| Schema Version | 1.3 |
| Responsible Team | Hadith Data Team |
| Curator | Hadith Verification Board |
| Grade Included | Internal verification grade by contemporary scholars. |
| Chapters | Arranged by companion (musnad format) |
| Isnad Depth | Full chain |
| Storage Tier | Hot |
| Backup Classification | Critical (Tier 1) |
| Languages | Arabic (text), English (translation in progress) |
| Tags | hadith, ahmad, musnad, hanbali, major-collection |

#### 2.3.8 DATASET-HT-008: Muwatta Malik

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-HT-008 |
| Dataset Name | Muwatta Malik ibn Anas — Full Text with Grading and Isnad |
| Version | 1.2.0 |
| Source | Digitized from Dar al-Gharb al-Islami edition (Beirut) |
| License | CC BY-NC-ND 4.0 |
| Format | JSON, XML |
| Records | ~1,720 hadith (depending on riwaya) |
| Update Frequency | As needed |
| Quality Score | 0.993 |
| Schema Version | 1.2 |
| Responsible Team | Hadith Data Team |
| Curator | Classical Texts Verification Unit |
| Riwaya Included | Yahya al-Laythi (primary), Muhammad al-Shaybani (appendix) |
| Chapters | 61 kitab |
| Isnad Depth | Full chain |
| Storage Tier | Hot |
| Backup Classification | Critical (Tier 1) |
| Languages | Arabic (text), English (translation) |
| Tags | hadith, malik, muwatta, maliki, classical |

#### 2.3.9 DATASET-HT-009: Hadith Comprehensive Collection

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-HT-009 |
| Dataset Name | Hadith Comprehensive Collection — All Major Collections |
| Version | 2.0.0 |
| Source | Derived from DATASET-HT-001 through DATASET-HT-008 plus additional collections |
| License | CC BY-NC-SA 4.0 (derived dataset) |
| Format | JSON, Parquet |
| Records | ~250,000 hadith across all collections |
| Update Frequency | Quarterly |
| Quality Score | 0.992 |
| Schema Version | 2.0 |
| Responsible Team | Hadith Data Team |
| Curator | Hadith Integration Working Group |
| Collections Included | Bukhari, Muslim, Abu Dawud, Tirmidhi, Nasai, Ibn Majah, Musnad Ahmad, Muwatta Malik, Sunan al-Darimi, Sahih Ibn Hibban, Sahih Ibn Khuzaymah, Sunan al-Bayhaqi, Musannaf Abd al-Razzaq, Musannaf Ibn Abi Shaybah, Al-Adab al-Mufrad, Sharh al-Sunnah al-Baghawi, Mishkat al-Masabih, Riyad al-Salihin, Bulugh al-Maram, Al-Mundhiri's al-Targhib, Al-Nawawi's 40 Hadith, and 15 additional collections |
| Storage Tier | Hot |
| Backup Classification | Critical (Tier 1) |
| Languages | Arabic (text), English (translation for primary collections) |
| Tags | hadith, comprehensive, sunni, canonical, all-collections |

#### 2.3.10 DATASET-HT-010: Hadith Grading Database

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-HT-010 |
| Dataset Name | Hadith Grading Database — Multi-Scholar Grading |
| Version | 1.4.0 |
| Source | Compiled from published scholarly works and fatwa collections |
| License | CC BY-NC-SA 4.0 |
| Format | JSON, Parquet |
| Records | ~100,000 grading entries covering ~40,000 unique hadith |
| Update Frequency | Semi-annual |
| Quality Score | 0.989 |
| Schema Version | 1.2 |
| Responsible Team | Hadith Data Team |
| Curator | Hadith Verification Board |
| Scholars Included | Bukhari, Muslim, Abu Dawud, Tirmidhi, Ibn al-Jawzi, Ibn Taymiyyah, Ibn al-Qayyim, al-Dhahabi, Ibn Hajar, al-San'ani, al-Shawkani, al-Albani, Ibn Baz, Ibn Uthaymin, al-Arna'ut, al-Wadi'i, al-Ghamidi, and 25 additional scholars |
| Storage Tier | Hot |
| Backup Classification | Critical (Tier 1) |
| Languages | Arabic |
| Tags | hadith, grading, authentication, sahih, daif, scholarly-opinion |

#### 2.3.11 DATASET-HT-011: Hadith Narrator Database (Rijal)

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-HT-011 |
| Dataset Name | Rijal al-Hadith — Narrator Biographies and Jarh wa Ta'dil |
| Version | 1.3.0 |
| Source | Compiled from Tahdhib al-Kamal (al-Mizzi), Tahdhib al-Tahdhib (Ibn Hajar), Mizan al-I'tidal (al-Dhahabi) and 10 additional primary sources |
| License | CC BY-NC-SA 4.0 |
| Format | JSON, CSV |
| Records | ~40,000 narrators with biographical data |
| Update Frequency | Semi-annual |
| Quality Score | 0.986 |
| Schema Version | 1.3 |
| Responsible Team | Hadith Data Team |
| Curator | Rijal Research Unit |
| Data Per Narrator | Full name, kunya, nisbah, tribe, birthplace, death date, teachers, students, jarh (criticism), ta'dil (praise), reliability grade(s) with source citations |
| Storage Tier | Hot |
| Backup Classification | Standard (Tier 2) |
| Languages | Arabic |
| Tags | hadith, rijal, narrators, biographies, tahdhib, jarh-tadil |

#### 2.3.12 DATASET-HT-012: Hadith Topics Classification

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-HT-012 |
| Dataset Name | Hadith Topic Classification — Multi-Level Thematic Tags |
| Version | 1.2.0 |
| Source | Derived from DATASET-HT-009 with scholarly annotation |
| License | CC BY-NC-SA 4.0 |
| Format | JSON, Parquet |
| Records | ~250,000 hadith tagged with hierarchical topics |
| Update Frequency | Semi-annual |
| Quality Score | 0.978 |
| Schema Version | 1.1 |
| Responsible Team | Hadith Data Team |
| Curator | Hadith Classification Working Group |
| Topic Hierarchy | 15 top-level categories, 150 second-level, 650 third-level |
| Storage Tier | Warm |
| Backup Classification | Standard (Tier 2) |
| Languages | Arabic, English |
| Tags | hadith, topics, classification, thematic, search |

### 2.4 Scholarly Works Datasets

#### 2.4.1 DATASET-SW-001: Fiqh al-Madhahib al-Arba'ah

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-SW-001 |
| Dataset Name | Fiqh al-Madhahib al-Arba'ah — Four Schools Jurisprudence |
| Version | 1.2.0 |
| Source | Compiled from major fiqh works of the four schools |
| License | CC BY-NC-SA 4.0 |
| Format | JSON, Parquet |
| Records | ~100,000 fiqh rulings classified by school, topic, and evidence |
| Update Frequency | Semi-annual |
| Quality Score | 0.984 |
| Schema Version | 1.3 |
| Responsible Team | Fiqh Data Team |
| Curator | Fiqh Codification Committee |
| Schools Covered | Hanafi (al-Hidayah, al-Ikhtiyar, Radd al-Muhtar), Maliki (al-Mudawwanah, al-Kafi, Bidayat al-Mujtahid), Shafi'i (al-Umm, al-Majmu', al-Minhaj, Fath al-Qarib), Hanbali (al-Mughni, al-Muqni', al-Insaf, Sharh al-Muntaha), Zahiri (al-Muhalla) |
| Storage Tier | Hot |
| Backup Classification | Critical (Tier 1) |
| Languages | Arabic (primary), English (selected rulings) |
| Tags | fiqh, jurisprudence, hanafi, maliki, shafii, hanbali, comparative |

#### 2.4.2 DATASET-SW-002: Fiqh Fatawa Collection

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-SW-002 |
| Dataset Name | Fatawa Collection — Contemporary and Classical |
| Version | 1.1.0 |
| Source | Compiled from published fatwa collections of major scholars and institutions |
| License | CC BY-NC-ND 4.0 |
| Format | JSON, Parquet |
| Records | ~200,000 fatawa |
| Update Frequency | Quarterly |
| Quality Score | 0.979 |
| Schema Version | 1.2 |
| Responsible Team | Fiqh Data Team |
| Curator | Fatawa Review Board |
| Scholars/Institutions Included | Ibn Taymiyyah, Ibn al-Qayyim, al-Shawkani, Ibn Baz, Ibn Uthaymin, al-Albani, al-Lajna al-Daimah, al-Azhar, Dar al-Ifta al-Misriyyah, al-Majma' al-Fiqhi, Mufti Taqi Uthmani, Yusuf al-Qaradawi, and 30 additional scholars |
| Storage Tier | Hot |
| Backup Classification | Critical (Tier 1) |
| Languages | Arabic (primary), Urdu, English |
| Tags | fiqh, fatawa, contemporary, classical, rulings, ifta |

#### 2.4.3 DATASET-SW-003: Usul al-Fiqh Corpus

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-SW-003 |
| Dataset Name | Usul al-Fiqh — Principles of Jurisprudence Corpus |
| Version | 1.0.0 |
| Source | Digitized from major usul works |
| License | CC BY-NC-SA 4.0 |
| Format | JSON, plain text |
| Records | 15 major usul works, fully digitized |
| Update Frequency | As needed |
| Quality Score | 0.987 |
| Schema Version | 1.0 |
| Responsible Team | Foundational Texts Team |
| Curator | Usul Research Unit |
| Works Included | al-Risalah (al-Shafi'i), al-Mustasfa (al-Ghazali), al-Ihkam (al-Amidi), Muntaha al-Su'l (al-Baydawi), al-Taqrir (Ibn Amir Haj), Irshad al-Fuhul (al-Shawkani), al-Muwafaqat (al-Shatibi), al-Mizan (al-Shirazi), al-Mustasfa (al-Ghazali) |
| Storage Tier | Warm |
| Backup Classification | Standard (Tier 2) |
| Languages | Arabic |
| Tags | usul, fiqh, principles, jurisprudence, methodology |

#### 2.4.4 DATASET-SW-004: Aqeedah Compendium

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-SW-004 |
| Dataset Name | Aqeedah Compendium — Islamic Creed Works |
| Version | 1.0.0 |
| Source | Digitized from authoritative creedal works |
| License | CC BY-NC-SA 4.0 |
| Format | JSON, plain text |
| Records | 25 major aqeedah works |
| Update Frequency | As needed |
| Quality Score | 0.991 |
| Schema Version | 1.0 |
| Responsible Team | Foundational Texts Team |
| Curator | Aqeedah Research Unit |
| Works Included | al-Aqidah al-Tahawiyyah, al-Aqidah al-Wasitiyyah, Sharh al-Aqidah al-Tahawiyyah (Ibn Abi al-Izz), al-Hamawiyyah, al-Tadmuriyyah, Kitab al-Iman, al-Ibannah, al-Sharh wa al-Ibanah, al-Iqtisad fi al-I'tiqad, al-Ghunya fi al-Usul |
| Storage Tier | Hot |
| Backup Classification | Critical (Tier 1) |
| Languages | Arabic (primary), English (selected works) |
| Tags | aqeedah, creed, theology, faith, islamic-doctrine |

#### 2.4.5 DATASET-SW-005: Sirah al-Nabawiyyah

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-SW-005 |
| Dataset Name | Sirah al-Nabawiyyah — Prophetic Biography Comprehensive |
| Version | 1.1.0 |
| Source | Digitized from major sirah works |
| License | CC BY-NC-SA 4.0 |
| Format | JSON, plain text |
| Records | 12 major sirah works |
| Update Frequency | As needed |
| Quality Score | 0.987 |
| Schema Version | 1.2 |
| Responsible Team | Historical Texts Team |
| Curator | Sirah Research Unit |
| Works Included | al-Sirah al-Nabawiyyah (Ibn Hisham), al-Tabaqat al-Kubra (Ibn Sa'd), al-Sirah al-Nabawiyyah (Ibn Kathir), al-Sirah al-Nabawiyyah (al-Salihi), al-Shifa (Qadi Iyad), Zad al-Ma'ad (Ibn al-Qayyim), Mukhtasar Sirat al-Rasul (al-Dhahabi), al-Mawahib al-Laduniyyah (al-Qastallani), al-Rawd al-Unuf (al-Suhayli), al-Sirah al-Halabiyyah, Fiqh al-Sirah (al-Ghazali), al-Rahiq al-Makhtum (al-Mubarakfuri) |
| Storage Tier | Hot |
| Backup Classification | Critical (Tier 1) |
| Languages | Arabic (primary), English (selected) |
| Tags | sirah, prophetic-biography, life-of-muhammad, prophetic-history |

#### 2.4.6 DATASET-SW-006: Islamic History Chronological

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-SW-006 |
| Dataset Name | Islamic History — Comprehensive Chronological Record |
| Version | 1.0.0 |
| Source | Compiled from major historical works |
| License | CC BY-NC-SA 4.0 |
| Format | JSON, Parquet |
| Records | ~50,000 dated events from 610 CE to present |
| Update Frequency | Semi-annual |
| Quality Score | 0.975 |
| Schema Version | 1.1 |
| Responsible Team | Historical Texts Team |
| Curator | Islamic History Research Unit |
| Sources | Tarikh al-Tabari, Tarikh al-Ya'qubi, Tarikh al-Mas'udi, al-Bidayah wa al-Nihayah, Tarikh al-Islam (al-Dhahabi), al-'Ibar (Ibn Khaldun), Tarikh al-Khulafa (al-Suyuti), and 20 additional primary sources |
| Storage Tier | Warm |
| Backup Classification | Standard (Tier 2) |
| Languages | Arabic (primary), English (summary) |
| Tags | history, islamic, timeline, chronology, events |

### 2.5 Arabic Language Datasets

#### 2.5.1 DATASET-AR-001: Classical Arabic Corpus

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-AR-001 |
| Dataset Name | Classical Arabic Corpus — Pre-Islamic through 19th Century |
| Version | 1.3.0 |
| Source | Compiled from digitized classical texts, public domain libraries |
| License | CC BY-SA 4.0 |
| Format | JSON, Parquet |
| Records | ~500 million words from ~10,000 classical texts |
| Update Frequency | Semi-annual |
| Quality Score | 0.982 |
| Schema Version | 1.4 |
| Responsible Team | Arabic NLP Team |
| Curator | Classical Arabic Corpus Project |
| Periods | Pre-Islamic (500–610 CE), Early Islamic (610–750 CE), Abbasid Golden (750–1258 CE), Post-Abbasid (1258–1798 CE), Early Modern (1798–1920 CE) |
| Genres | Poetry (pre-Islamic, Umayyad, Abbasid, Andalusian), Prose (adab, sermons, letters), Scientific (medicine, astronomy, mathematics), Philosophical, Historical, Juridical, Theological, Lexicographical |
| Storage Tier | Warm |
| Backup Classification | Standard (Tier 2) |
| Languages | Arabic |
| Tags | arabic, classical, corpus, pre-islamic, abbasid, golden-age, poetry, prose |

#### 2.5.2 DATASET-AR-002: Modern Arabic Corpus

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-AR-002 |
| Dataset Name | Modern Arabic Corpus — Contemporary Arabic Text |
| Version | 2.1.0 |
| Source | Compiled from news sources, books, academic publications, web (licensed) |
| License | CC BY-NC-SA 4.0 |
| Format | JSON, Parquet |
| Records | ~2 billion words |
| Update Frequency | Quarterly |
| Quality Score | 0.969 |
| Schema Version | 1.5 |
| Responsible Team | Arabic NLP Team |
| Curator | Modern Arabic Corpus Project |
| Sources | News (30+ Arabic news agencies), Academic journals (1,200+ journals), Books (50,000+ titles), Web (licensed crawl data) |
| Storage Tier | Warm |
| Backup Classification | Standard (Tier 2) |
| Languages | Arabic (Modern Standard) |
| Tags | arabic, modern, contemporary, corpus, news, academic, msa |

#### 2.5.3 DATASET-AR-003: Arabic Morphological Analyzer Models

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-AR-003 |
| Dataset Name | Arabic Morphological Analyzer — Pre-trained Models and Lexicons |
| Version | 2.2.1 |
| Source | Derived from DATASET-AR-001, DATASET-AR-002, augmented with specialized lexicons |
| License | CC BY-NC-SA 4.0 |
| Format | TensorFlow SavedModel, ONNX, JSON (lexicon) |
| Records | Lexicon: ~500,000 lemmas with full morphological paradigm |
| Update Frequency | Semi-annual |
| Quality Score | 0.972 |
| Schema Version | 2.0 |
| Responsible Team | Arabic NLP Team |
| Curator | Arabic Morphology Research Group |
| Features | Root extraction, pattern identification, POS tagging, diacritization, lemma selection, stem identification, morphological segmentation |
| Storage Tier | Hot (models), Warm (lexicon) |
| Backup Classification | Standard (Tier 2) |
| Languages | Arabic |
| Tags | arabic, morphology, analyzer, model, lexicon, root, pattern, pos |

#### 2.5.4 DATASET-AR-004: Arabic Treebank

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-AR-004 |
| Dataset Name | Arabic Syntactic Treebank — Dependency Parsing |
| Version | 1.2.0 |
| Source | Derived from DATASET-AR-001, DATASET-AR-002, augmented with manual annotation |
| License | CC BY-NC-SA 4.0 |
| Format | CoNLL-U, JSON |
| Records | ~1.5 million annotated sentences |
| Update Frequency | Semi-annual |
| Quality Score | 0.979 |
| Schema Version | 1.2 |
| Responsible Team | Arabic NLP Team |
| Curator | Arabic Syntax Research Group |
| Annotation | Dependency relations, phrase structure for selected portions |
| Storage Tier | Warm |
| Backup Classification | Standard (Tier 2) |
| Languages | Arabic |
| Tags | arabic, treebank, syntax, dependency, grammar, parsing |

#### 2.5.5 DATASET-AR-005: Arabic Lexicons

| Attribute | Value |
|---|---|
| Dataset ID | DATASET-AR-005 |
| Dataset Name | Arabic Lexicons — Classical and Modern Dictionary Collection |
| Version | 1.4.0 |
| Source | Digitized from major Arabic lexicons |
| License | CC BY-NC-SA 4.0 |
| Format | JSON, Parquet |
| Records | ~250,000 headwords with full lexical entries |
| Update Frequency | As needed |
| Quality Score | 0.991 |
| Schema Version | 1.3 |
| Responsible Team | Arabic NLP Team |
| Curator | Lexicography Research Unit |
| Lexicons Included | Lisan al-Arab (Ibn Manzur), al-Qamus al-Muhit (al-Fayruzabadi), Taj al-Arus (al-Zabidi), al-Mu'jam al-Wasit, al-Munjid, al-Mufradat fi Gharib al-Quran (al-Raghib), Mukhtar al-Sihah, al-Misbah al-Munir |
| Storage Tier | Hot |
| Backup Classification | Standard (Tier 2) |
| Languages | Arabic, English (selected entries) |
| Tags | arabic, lexicon, dictionary, lisan-al-arab, qamus, classical |

