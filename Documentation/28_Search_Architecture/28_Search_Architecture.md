# SEARCH ARCHITECTURE

## Full-Text and Semantic Search System

---

| Document ID | IEP-ARCH-SRCH-001 |
|---|---|
| Version | 1.0.0 |
| Status | Approved |
| Classification | Enterprise Internal |
| Effective Date | 2026-07-09 |
| Approving Authority | Office of the Chief Architect |
| Custodian | Search Architecture Team |

---

## TABLE OF CONTENTS

1. EXECUTIVE OVERVIEW
2. SEARCH PHILOSOPHY
3. SEARCH TYPES
4. INDEXING ARCHITECTURE
5. QUERY PIPELINE
6. ARABIC SEARCH SPECIALIZATION
7. RANKING AND RELEVANCE
8. FACETS AND FILTERS
9. QUALITY METRICS
10. APPENDICES

---

## 1. EXECUTIVE OVERVIEW

### 1.1 Purpose

The Search Architecture provides fast, accurate, and relevant search across all platform content — stories, quizzes, flashcards, podcasts, Knowledge Graph facts, and community discussions — with specialized support for Arabic morphology and Islamic terminology.

### 1.2 Core Principle

**Every learner query finds the most relevant, verified content in milliseconds.**

---

## 2. SEARCH PHILOSOPHY

| Principle | Description |
|---|---|
| **Hybrid Search** | Keyword + semantic search combined for best results |
| **Arabic-First** | Arabic morphology, stemming, and diacritic-aware search |
| **Knowledge-Grounded** | Search results prioritize verified Knowledge Graph content |
| **Personalized** | Results consider learner's level, interests, history |
| **Fast** | Sub-second response for all queries |

---

## 3. SEARCH TYPES

| Type | Method | Use Case |
|---|---|---|
| **Full-Text** | Elasticsearch BM25 | Keyword search for text content |
| **Semantic** | Vector embeddings (pgvector) | Meaning-based search |
| **Hybrid** | Combined BM25 + vector | General search (default) |
| **Knowledge Graph** | Cypher/graph traversal | Relationship-based discovery |
| **Autocomplete** | Edge N-gram + suggesters | Search-as-you-type |
| **Spell Correct** | Levenshtein + phonetic | Correct misspelled queries |
| **Cross-Lingual** | Multi-lingual embeddings | Search in one language, find in another |

---

## 4. INDEXING ARCHITECTURE

### 4.1 Index Structure

```
┌─────────────────────────────────────────────────────────────────────────┐
│                      ELASTICSEARCH CLUSTER                              │
│                                                                          │
│  Index: content_v1          Index: knowledge_v1       Index: social_v1 │
│  • stories                   • facts                  • discussions     │
│  • quizzes                   • entities               • comments        │
│  • flashcards                • relationships          • groups          │
│  • podcasts                                                     │
│  • lessons                                                        │
│                                                                          │
│  Index: suggestions_v1       Index: logs_v1                             │
│  • autocomplete              • search_analytics                          │
│  • popular_searches          • click_tracking                            │
└─────────────────────────────────────────────────────────────────────────┘
```

### 4.2 Vector Store

| Component | Technology | Dimensions |
|---|---|---|
| **Content Embeddings** | pgvector (PostgreSQL) | 1536 (text-embedding-3) |
| **Arabic Specialized** | pgvector | 768 (Arabic-specific model) |
| **Cross-Lingual** | pgvector | 1024 (multilingual model) |

---

## 5. QUERY PIPELINE

```
User Query
    │
    ▼
┌──────────────────────────────┐
│ 1. Query Understanding       │
│ • Language detection         │
│ • Intent classification      │
│ • Entity extraction          │
│ • Spell correction           │
└──────────────────────────────┘
    │
    ▼
┌──────────────────────────────┐
│ 2. Query Expansion           │
│ • Synonyms (Islamic terms)   │
│ • Stemming (Arabic)          │
│ • Transliteration expansion  │
└──────────────────────────────┘
    │
    ▼
┌──────────────────────────────┐
│ 3. Multi-Strategy Search     │
│ • Full-text (BM25)           │
│ • Semantic (vector)          │
│ • Knowledge Graph            │
└──────────────────────────────┘
    │
    ▼
┌──────────────────────────────┐
│ 4. Fusion & Ranking          │
│ • Reciprocal Rank Fusion     │
│ • Personalization boost      │
│ • Freshness boost            │
│ • Quality score boost        │
└──────────────────────────────┘
    │
    ▼
┌──────────────────────────────┐
│ 5. Result Assembly           │
│ • Highlight generation       │
│ • Facet extraction           │
│ • Pagination                 │
└──────────────────────────────┘
    │
    ▼
Search Results
```

---

## 6. ARABIC SEARCH SPECIALIZATION

| Feature | Implementation |
|---|---|
| **Arabic Stemmer** | Custom Arabic light stemmer (remove prefixes/suffixes) |
| **Root-Based Search** | Index by triliteral root for classical Arabic |
| **Diacritic Insensitive** | Strip tashkeel for matching, preserve in display |
| **Alef Normalization** | Normalize أ إ آ to ا for search |
| **Ta Marbouta** | Normalize ة to ه |
| **Kashida Removal** | Strip tatweel characters |
| **Transliteration** | Romanized → Arabic script matching |

---

## 7. RANKING AND RELEVANCE

| Factor | Weight | Description |
|---|---|---|
| **BM25 Score** | 0.35 | Keyword relevance |
| **Vector Similarity** | 0.30 | Semantic relevance |
| **Content Quality** | 0.15 | Verification tier, scholar-approved |
| **Learner Match** | 0.10 | Matching learner level and interests |
| **Freshness** | 0.05 | Newer content boosted slightly |
| **Popularity** | 0.05 | Historical click and engagement rate |

---

## 8. FACETS AND FILTERS

| Facet | Example Values |
|---|---|
| **Knowledge Domain** | Aqidah, Fiqh, Hadith, Quran, Seerah, Tarikh |
| **Content Type** | Story, Quiz, Flashcard, Podcast, Article |
| **Difficulty** | Beginner, Intermediate, Advanced, Expert |
| **Age Group** | 6-8, 9-12, 13-15, 16+ |
| **Language** | Arabic, English, Urdu, Malay |
| **Scholar** | Ibn Kathir, Al-Bukhari, etc. |
| **Verification Status** | Verified, Scholar-Approved, Auto-Verified |

---

## 9. QUALITY METRICS

| Metric | Target |
|---|---|
| **P50 Latency** | < 100ms |
| **P95 Latency** | < 300ms |
| **P99 Latency** | < 1 second |
| **Click-Through Rate (Top 3)** | > 60% |
| **Zero Results Rate** | < 2% |
| **Spell Correction Accuracy** | > 95% |
| **Index Freshness** | < 5 minutes |

---

## 10. APPENDICES

### 10.1 Related Documents

| Document | Title | Relationship |
|---|---|---|
| IEP-ARCH-DB-001 | Database Architecture | pgvector + Elasticsearch cluster |
| IEP-ARCH-RAG-001 | RAG Architecture | Search for context retrieval |

### 10.2 Revision History

| Version | Date | Author | Changes |
|---|---|---|---|
| 1.0.0 | 2026-07-09 | Office of the Chief Architect | Initial approved release |
