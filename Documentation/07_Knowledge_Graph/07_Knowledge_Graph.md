# KNOWLEDGE GRAPH ARCHITECTURE

## Islamic Education Platform (IEP)

---

| Document ID | IEP-ARCH-KG-001 |
|---|---|
| Version | 1.0.0 |
| Status | Approved |
| Classification | Enterprise Internal |
| Effective Date | 2026-07-07 |
| Approving Authority | Office of the Chief Architect |
| Custodian | Knowledge Architecture Team |
| Database Architecture Reference | IEP-ARCH-DB-001 |
| Constitution Reference | IEP-CONST-001 (Sections 2, 6, 7) |

---

## TABLE OF CONTENTS

1. EXECUTIVE OVERVIEW
2. KNOWLEDGE GRAPH PHILOSOPHY
3. ONTOLOGY ARCHITECTURE
4. NODE TYPE CATALOG
5. EDGE TYPE CATALOG
6. PROPERTY SCHEMAS
7. KNOWLEDGE DOMAIN ONTOLOGY
8. PROVENANCE ARCHITECTURE
9. VERSIONING AND TEMPORALITY
10. CONFIDENCE AND CERTAINTY
11. CROSS-DOMAIN LINKING
12. QUERY PATTERNS
13. KNOWLEDGE INGESTION FLOW
14. KNOWLEDGE ACCESS PATTERNS
15. GRAPH MAINTENANCE
16. GRAPH ANALYTICS
17. PERFORMANCE OPTIMIZATION
18. APPENDICES

---

## 1. EXECUTIVE OVERVIEW

### 1.1 Purpose

The Knowledge Graph (KG) is the central nervous system of the Islamic Education Platform. It stores all verified knowledge facts, their relationships, their provenance, and their confidence scores in a structured, queryable, and fully auditable format. Every learning mode, every AI agent, and every user interaction draws from this single source of truth.

### 1.2 Core Principles

| Principle | Description |
|---|---|
| Single Source of Truth | Every fact exists in exactly one place in the graph |
| Immutable Provenance | Every fact has a complete, verifiable chain of custody |
| Confidence-Gated | Every fact has a confidence score that determines how it can be used |
| Scholarly Diversity | Multiple scholarly positions coexist with attribution |
| Cross-Domain Connected | Knowledge across all Islamic disciplines is linked |
| Temporality | Facts are versioned; historical states are preserved |

### 1.3 Graph Metrics (Year 5 Projections)

| Metric | Estimate |
|---|---|
| Total Nodes | 500 million |
| Total Edges | 5 billion |
| Node Types | 20+ |
| Edge Types | 50+ |
| Knowledge Domains | 25+ |
| Source Documents | 100,000+ |
| Languages | 30+ |
| Provenance Records | 500 million+ |
| Confidence Updates | 1 billion+ |

---

## 2. KNOWLEDGE GRAPH PHILOSOPHY

### 2.1 Graph Model Choice

The IEP Knowledge Graph uses a **Labeled Property Graph (LPG)** model:

- **Nodes** represent entities, concepts, facts, sources, events, and other knowledge objects.
- **Edges** represent relationships between nodes, with types and properties.
- **Properties** are key-value pairs on both nodes and edges.

Storage: PostgreSQL + Apache AGE (primary), Neo4j (analytics, optional).

### 2.2 Graph Design Principles

| Principle | Implementation |
|---|---|
| **Atomic Facts** | Each fact node represents exactly one atomic assertion |
| **Provenance-Rich** | Every fact node connects to its source via provenance chain |
| **Context-Preserving** | Facts are linked to their context (book, chapter, paragraph) |
| **Multi-Perspective** | Multiple scholarly positions coexist as separate nodes |
| **Confidence-Annotated** | Every node and edge has confidence metadata |
| **Temporally-Aware** | Facts have creation, validation, and expiry timestamps |
| **Cross-Domain** | Facts automatically link to related facts across disciplines |

---

## 3. ONTOLOGY ARCHITECTURE

### 3.1 Upper Ontology

```
    Thing
      |
    +---+---+---+---+---+---+---+---+---+---+
    |   |   |   |   |   |   |   |   |   |   |
  Concept Entity Fact  Source Event Location Time  Scholarly Language  Media
                                                     Position
```

### 3.2 Ontology Design Methodology

1. **Domain Analysis**: Islamic scholars identify key knowledge categories.
2. **Concept Extraction**: Extract concepts from source texts.
3. **Relationship Typing**: Define relationship types between concepts.
4. **Hierarchy Building**: Organize concepts into taxonomies.
5. **Cross-Referencing**: Link related concepts across domains.
6. **Validation**: Scholars validate ontology completeness and accuracy.

### 3.3 Domain Ontologies

| Domain | Root Concept | Specialized Ontology |
|---|---|---|
| Quran | Quran | Surah, Ayah, Juz, Hizb, Ruku, Sajdah, Revelation Type |
| Hadith | Hadith | Collection, Book, Chapter, Hadith, Narrator, Chain Grade |
| Fiqh | Fiqh | Ibadah, Muamalat, Family Law, Criminal Law, Inheritance |
| Aqeedah | Aqeedah | Tawheed, Prophethood, Angels, Books, Last Day, Qadr |
| Seerah | Seerah | Biography, Event, Battle, Treaty, Migration, Year |
| History | Islamic History | Dynasty, Caliphate, Period, Region, Civilization |
| Arabic | Arabic Language | Grammar, Morphology, Rhetoric, Lexicon |
| Tazkiyah | Tazkiyah | Spiritual States, Virtues, Vices, Purification |
| Fiqh Methods | Usul al-Fiqh | Sources, Principles, Ijtihad, Ijma, Qiyas |
| Hadith Sciences | Mustalah al-Hadith | Classification, Narrator Criticism, Chain Analysis |

---

## 4. NODE TYPE CATALOG

### 4.1 Primary Node Types

#### 4.1.1 Concept (Abstract Ideas)

```json
{
  "node_type": "Concept",
  "label": "Salah",
  "description": "The Islamic act of worship, prayer performed five times daily",
  "domain": "Fiqh",
  "subdomain": "Ibadah",
  "alternative_names": ["Prayer", "Salat", "Namaz"],
  "parent_concept": "Ibadah",
  "child_concepts": ["Fajr", "Dhuhr", "Asr", "Maghrib", "Isha"],
  "related_concepts": ["Wudu", "Taharah", "Qibla", "Adhan"],
  "definition_sources": ["fact-001", "fact-002"],
  "confidence": 0.99
}
```

#### 4.1.2 Entity (Specific Things)

```json
{
  "node_type": "Entity",
  "entity_type": "Person",
  "sub_type": "Scholar",
  "label": "Imam Abu Hanifa",
  "full_name_ar": "أبو حنيفة النعمان بن ثابت",
  "full_name_en": "Abu Hanifa al-Nu'man ibn Thabit",
  "birth_year": 699,
  "death_year": 767,
  "birth_place": "Kufa",
  "madhab": "Hanafi",
  "known_for": ["Founder of Hanafi school", "Fiqh methodology"],
  "teachers": ["Hammad ibn Abi Sulayman"],
  "students": ["Abu Yusuf", "Muhammad al-Shaybani"],
  "major_works": ["Al-Fiqh al-Akbar"],
  "confidence": 0.99
}
```

#### 4.1.3 Fact (Atomic Assertion)

```json
{
  "node_type": "Fact",
  "fact_type": "Ruling",
  "domain": "Fiqh",
  "statement": "Salah is obligatory (fard) upon every adult Muslim",
  "subject": "Salah",
  "predicate": "is_obligatory_for",
  "object": "Adult_Muslim",
  "conditions": ["sane", "adult", "Muslim"],
  "qualifiers": "Except for menstruating women for specific prayers",
  "evidence": ["Quran 2:43", "Hadith Bukhari 1"],
  "scholarly_consensus": true,
  "confidence": 0.99
}
```

#### 4.1.4 Source (Document Reference)

```json
{
  "node_type": "Source",
  "source_type": "Book",
  "title": "Sahih al-Bukhari",
  "author": "Muhammad al-Bukhari",
  "language": "ar",
  "publication_date": 846,
  "publisher": "Various",
  "collection": "Kutub al-Sittah",
  "volume_count": 9,
  "hadith_count": 7563,
  "digital_identifier": "isbn-xxx",
  "checksum_sha256": "abc...",
  "confidence": 1.0
}
```

#### 4.1.5 Event (Historical Occurrence)

```json
{
  "node_type": "Event",
  "event_type": "Battle",
  "label": "Battle of Badr",
  "date": "624-03-13 (17 Ramadan 2 AH)",
  "location": "Badr, Hejaz",
  "participants": ["Prophet Muhammad", "Abu Jahl"],
  "outcome": "Muslim victory",
  "significance": "First major battle in Islamic history",
  "confidence": 0.98
}
```

#### 4.1.6 Location (Geographic Place)

```json
{
  "node_type": "Location",
  "location_type": "City",
  "label": "Mecca",
  "label_ar": "مكة المكرمة",
  "country": "Saudi Arabia",
  "region": "Hejaz",
  "coordinates": {"lat": 21.4225, "lng": 39.8262},
  "significance": ["Birthplace of Prophet Muhammad", "Holiest city in Islam"],
  "historical_names": ["Bakka"],
  "confidence": 1.0
}
```

### 4.2 Complete Node Type Catalog

| Node Type | Description | Attributes Count | Estimated Count |
|---|---|---|---|
| Concept | Abstract Islamic concept | 15 | 10M |
| Entity: Person | Individual person (scholar, narrator, etc.) | 25 | 1M |
| Entity: Group | Tribe, school, dynasty, organization | 12 | 100K |
| Entity: Book | Book or document | 20 | 500K |
| Fact: Ruling | Legal/religious ruling | 20 | 50M |
| Fact: Definition | Definition of a term | 12 | 5M |
| Fact: Historical | Historical fact | 15 | 10M |
| Fact: Biographical | Biographical fact | 15 | 5M |
| Fact: Linguistic | Linguistic fact | 12 | 10M |
| Fact: Scientific | Scientific/medical fact from Islamic civilization | 12 | 1M |
| Source: Book | Full book | 20 | 100K |
| Source: Chapter | Book chapter | 10 | 1M |
| Source: Page | Book page | 8 | 100M |
| Source: Paragraph | Paragraph text | 8 | 1B |
| Event | Historical event | 15 | 500K |
| Location | Geographic location | 15 | 100K |
| Time | Time period or date | 10 | 100K |
| ScholarlyPosition | A scholar's position on a specific issue | 12 | 10M |
| Language: Word | Arabic word or term | 10 | 1M |
| Language: Root | Arabic root (3-4 letters) | 6 | 10K |
| Language: Grammar Rule | Arabic grammar rule | 8 | 10K |
| Media | Image, audio, video reference | 10 | 1M |

---

## 5. EDGE TYPE CATALOG

### 5.1 Hierarchical Edges

| Edge Type | Source | Target | Description | Properties |
|---|---|---|---|---|
| is_a | Concept | Concept | Subclass relationship | hierarchy_level |
| part_of | Entity | Entity | Mereological relationship | role |
| member_of | Person | Group | Group membership | role, period |
| chapter_of | Chapter | Book | Chapter belongs to book | chapter_number |
| page_of | Page | Chapter | Page belongs to chapter | page_number |

### 5.2 Semantic Edges

| Edge Type | Source | Target | Description | Properties |
|---|---|---|---|---|
| relates_to | Node | Node | Generic relationship | relationship_type, strength |
| synonymous | Concept | Concept | Synonym relationship | language |
| translates_to | Concept | Concept | Translation | source_lang, target_lang |
| prerequisite_for | Concept | Concept | Learning prerequisite | importance |
| teaches | Concept | Concept | One concept teaches another | pedagogical_order |
| governs | Rule | Concept | Rule governs concept | rule_type |
| invalidates | Condition | Action | Condition invalidates action | condition_type |

### 5.3 Factual Edges

| Edge Type | Source | Target | Description | Properties |
|---|---|---|---|---|
| states | Person | Fact | Person asserts a fact | source_ref |
| supports | Fact | Fact | One fact supports another | strength |
| contradicts | Fact | Fact | Facts contradict | resolution (if resolved) |
| refines | Fact | Fact | One fact refines another | refinement_type |
| cites | Fact | Source | Fact cites source | citation_text, page |

### 5.4 Provenance Edges

| Edge Type | Source | Target | Description | Properties |
|---|---|---|---|---|
| derived_from | Fact | Extraction | Fact from extraction process | method, date |
| extracted_by | Fact | Agent | Agent that extracted fact | agent_id, model_version |
| verified_by | Fact | Verification | Verification step | verifier_type, result |
| approved_by | Fact | Person | Human approval | reviewer_id, date |

### 5.5 Temporal Edges

| Edge Type | Source | Target | Description | Properties |
|---|---|---|---|---|
| occurs_at | Event | Location | Where event occurred | - |
| occurs_in | Event | Time | When event occurred | calendar_type |
| born_in | Person | Location | Birthplace | - |
| died_in | Person | Location | Deathplace | - |
| lived_during | Person | Time | Lifetime | - |
| active_during | Person | Time | Period of activity | - |

### 5.6 Complete Edge Type Catalog

| Edge Type | Part of Ontology | Count Estimate |
|---|---|---|
| is_a | General | 50M |
| part_of | General | 100M |
| relates_to | General | 500M |
| synonymous | Language | 1M |
| translates_to | Language | 10M |
| prerequisite_for | Learning | 5M |
| teaches | Learning | 5M |
| governs | Fiqh | 10M |
| states | All | 50M |
| supports | All | 100M |
| contradicts | All | 5M |
| refines | All | 10M |
| cites | All | 500M |
| derived_from | Provenance | 500M |
| verified_by | Provenance | 500M |
| approved_by | Provenance | 10M |
| occurs_at | History | 500K |
| occurs_in | History | 500K |
| born_in | Biography | 1M |
| died_in | Biography | 1M |
| studied_under | Biography | 5M |
| taught | Biography | 5M |
| narrated_from | Hadith | 10M |
| authored_by | Bibliography | 500K |

---

## 6. PROPERTY SCHEMAS

### 6.1 Common Properties (All Nodes)

```json
{
  "id": "UUID v7",
  "created_at": "ISO 8601 timestamp",
  "updated_at": "ISO 8601 timestamp",
  "confidence": 0.0-1.0,
  "tenant_id": "UUID",
  "status": "draft|pending_review|verified|deprecated|archived",
  "version": 1,
  "labels": ["tag1", "tag2"]
}
```

### 6.2 Common Properties (All Edges)

```json
{
  "id": "UUID v7",
  "created_at": "ISO 8601 timestamp",
  "confidence": 0.0-1.0,
  "source_provenance": "extraction|manual|ai_generated|imported",
  "weight": 1.0
}
```

### 6.3 Confidence Property Definition

| Field | Type | Description |
|---|---|---|
| overall | float | Aggregate confidence (0-1) |
| extraction_confidence | float | Quality of extraction from source |
| verification_confidence | float | Cross-reference verification score |
| scholarly_approval | float | Human scholarly review (1.0=approved) |
| consistency_score | float | Consistency with related facts |
| temporal_reliability | float | How well the fact holds over time |
| method | string | How confidence was calculated |

---

## 7. KNOWLEDGE DOMAIN ONTOLOGY

### 7.1 Domain Architecture

```
    Root: Islamic Knowledge
      |
    +- Quran
    |   +- Revelation
    |   +- Recitation (Qira'at)
    |   +- Exegesis (Tafsir)
    |   +- Sciences (Ulum al-Quran)
    |
    +- Hadith
    |   +- Collections
    |   +- Classification
    |   +- Narrators (Rijal)
    |   +- Sciences (Mustalah)
    |
    +- Fiqh
    |   +- Ibadah (Worship)
    |   +- Muamalat (Transactions)
    |   +- Family Law
    |   +- Criminal Law
    |   +- Inheritance
    |   +- Usul al-Fiqh (Methodology)
    |
    +- Aqeedah
    |   +- Tawheed
    |   +- Prophethood
    |   +- Unseen
    |   +- Qadr
    |
    +- Seerah & History
    |   +- Prophetic Biography
    |   +- Caliphates
    |   +- Dynasties
    |   +- Civilizations
    |
    +- Spirituality
    |   +- Tazkiyah
    |   +- Adhkar
    |   +- Repentance
    |
    +- Arabic Language
    |   +- Grammar (Nahw)
    |   +- Morphology (Sarf)
    |   +- Rhetoric (Balaghah)
    |   +- Lexicon
    |
    +- Comparative
        +- Other Religions
        +- Sects
        +- Contemporary Issues
```

### 7.2 Cross-Domain Linking Strategy

| Source Domain | Target Domain | Link Type | Example |
|---|---|---|---|
| Fiqh (Salah) | Quran (2:43) | cites | "Salah is obligatory" cites "Establish prayer" |
| Fiqh (Salah) | Hadith (Bukhari 1) | cites | "Pray as you saw me pray" |
| Fiqh (Salah) | Aqeedah | is_based_on | Rulings based on theological foundations |
| Fiqh (Salah) | Seerah | context | When was Salah prescribed? |
| Fiqh (Salah) | Arabic | term | Definition of "Salah" linguistically |
| Fiqh (Salah) | History | evolution | How Salah was practiced historically |
| Fiqh (Salah) | Tazkiyah | spiritual | Spiritual significance of Salah |
| Fiqh (Salah) | Biography | teaches | Scholars who taught about Salah |

---

## 8. PROVENANCE ARCHITECTURE

### 8.1 Provenance Chain

```
    Source Document (Book, Page, Paragraph)
         |
    [OCR Agent] - extraction_1
         |
    [Entity Agent] - extraction_2
         |
    [Fact Agent] - extraction_3
         |
    Fact Node
         |
    [Cross-Ref Agent] - verification_1
         |
    [LLM Check Agent] - verification_2
         |
    [Contradiction Agent] - verification_3
         |
    [Scholarly Review] - verification_4 (human)
         |
    VERIFIED FACT (usable by content agents)
```

### 8.2 Provenance Record Schema

```json
{
  "provenance_id": "UUID v7",
  "fact_id": "UUID of the fact node",
  "chain": [
    {
      "step": 1,
      "agent": "ocr-agent",
      "agent_version": "2.1.0",
      "action": "extracted_text",
      "input": {"source_id": "book-123", "page": 45},
      "output": {"text_length": 500},
      "confidence": 0.97,
      "timestamp": "2026-07-07T10:00:00Z",
      "checksum": "sha256 of extracted text"
    },
    {
      "step": 2,
      "agent": "entity-agent",
      "agent_version": "1.3.0",
      "action": "extracted_entities",
      "input": {"text": "..."},
      "output": {"entities": ["Salah", "Wudu"]},
      "confidence": 0.95,
      "timestamp": "2026-07-07T10:01:00Z"
    },
    {
      "step": 3,
      "agent": "fact-agent",
      "agent_version": "2.0.1",
      "action": "extracted_fact",
      "input": {"entities": ["Salah", "Wudu"]},
      "output": {"fact": "Wudu is a condition for Salah"},
      "confidence": 0.93,
      "timestamp": "2026-07-07T10:02:00Z"
    }
  ],
  "source_reference": {
    "source_id": "source-bukhari-book1",
    "document_title": "Sahih al-Bukhari",
    "page": 45,
    "paragraph": 3,
    "char_offset": {"start": 100, "end": 250},
    "text_snippet": "...لا يقبل الله صلاة أحدكم إذا أحدث حتى يتوضأ..."
  },
  "current_verification_status": "verified",
  "verification_chain": [
    {"layer": "cross_ref", "result": "confirmed", "confidence": 0.96},
    {"layer": "llm_check", "result": "consistent", "confidence": 0.94},
    {"layer": "contradiction", "result": "no_contradiction", "confidence": 0.99},
    {"layer": "scholarly", "result": "approved", "reviewer": "scholar-001", "confidence": 1.0}
  ]
}
```

### 8.3 Provenance Immutability

| Property | Implementation |
|---|---|
| Append-only | Provenance records are never modified, only appended |
| Cryptographic linking | Each record includes hash of previous record |
| Timestamp authority | All records have trusted timestamps |
| Access control | Readable by all, writable only by verification pipeline |
| Retention | Permanent (never deleted) |

---

## 9. VERSIONING AND TEMPORALITY

### 9.1 Fact Versioning

Every fact node maintains a version chain:

```json
{
  "fact_id": "fact-salah-obligation-001",
  "current_version": 3,
  "versions": [
    {
      "version": 1,
      "statement": "Salah is obligatory upon every Muslim",
      "confidence": 0.90,
      "created_at": "2026-01-01",
      "status": "superseded"
    },
    {
      "version": 2,
      "statement": "Salah is obligatory (fard) upon every adult, sane Muslim",
      "confidence": 0.95,
      "created_at": "2026-03-15",
      "status": "superseded"
    },
    {
      "version": 3,
      "statement": "Salah is obligatory (fard 'ayn) upon every adult, sane Muslim who has reached the age of puberty",
      "confidence": 0.99,
      "created_at": "2026-06-01",
      "status": "active"
    }
  ],
  "version_history": [
    {"version": 1, "changed_by": "fact-agent-v1", "reason": "initial_extraction"},
    {"version": 2, "changed_by": "scholarly-review", "reason": "added_qualifiers"},
    {"version": 3, "changed_by": "cross-ref-agent", "reason": "added_specificity"}
  ]
}
```

### 9.2 Temporal Query Patterns

```cypher
-- Get fact as of a specific date
MATCH (f:Fact {id: 'fact-001'})
WHERE f.version_created_at <= datetime('2026-03-01')
RETURN f ORDER BY f.version DESC LIMIT 1

-- Get fact history
MATCH (f:Fact {id: 'fact-001'})
MATCH (f)-[:has_version]->(v:FactVersion)
RETURN v.version, v.statement, v.status, v.created_at
ORDER BY v.version DESC

-- Get facts valid during a time period
MATCH (f:Fact)
WHERE f.version_created_at <= datetime('2026-12-31')
  AND (f.version_deprecated_at IS NULL 
       OR f.version_deprecated_at > datetime('2026-01-01'))
RETURN f
```

---

## 10. CONFIDENCE AND CERTAINTY

### 10.1 Confidence Levels

| Level | Range | Meaning | Usage |
|---|---|---|---|
| CERTAIN | 0.99-1.00 | Absolutely verified (Quran, Mutawatir Hadith) | Auto-publish, no review |
| HIGH | 0.90-0.98 | Strongly verified (multiple sources, scholarly consensus) | Auto-publish with citation |
| MEDIUM | 0.70-0.89 | Supported but limited evidence | Published with confidence note |
| LOW | 0.50-0.69 | Weak evidence, single source | Requires human review before use |
| UNCERTAIN | <0.50 | Insufficient evidence | Not published, stored for research |

### 10.2 Confidence Aggregation

```
Fact Confidence = f(extraction_confidence, verification_confidence, scholarly_approval, consistency_score)

extraction_confidence: quality of source OCR and fact extraction
verification_confidence: cross-reference match quality
scholarly_approval: 1.0 if approved by scholar, otherwise 0
consistency_score: agreement with related facts in the graph

Formula:
  base = extraction_confidence * 0.3 + verification_confidence * 0.3
  if scholarly_approval:
    base = base * 0.6 + 0.4 * scholarly_approval
  else:
    base = base * 0.8
  final = base * (1 - (1 - consistency_score) * 0.2)
  final = clamp(final, 0, 1)
```

### 10.3 Confidence Decay

Facts can lose confidence over time if new contradictory evidence emerges:

```json
{
  "fact_id": "fact-xyz",
  "original_confidence": 0.95,
  "current_confidence": 0.82,
  "confidence_decay": [
    {
      "event": "new_evidence_from_book_456",
      "date": "2026-06-15",
      "old_confidence": 0.95,
      "new_confidence": 0.85,
      "reason": "Contradictory evidence found in newly ingested source"
    },
    {
      "event": "scholarly_review_updated",
      "date": "2026-07-01",
      "old_confidence": 0.85,
      "new_confidence": 0.82,
      "reason": "Scholar raised concern about incomplete context"
    }
  ]
}
```

---

## 11. CROSS-DOMAIN LINKING

### 11.1 Automatic Linking Rules

| Rule | Source | Target | Condition |
|---|---|---|---|
| Entity-Action | Person | Fact | Entity is subject of fact |
| Source-Content | Source | Fact | Fact cites source |
| Concept-Concept | Concept | Concept | Related by domain ontology |
| Location-Event | Location | Event | Event occurs at location |
| Time-Event | Time | Event | Event occurs at time |
| Scholar-Position | Person | ScholarlyPosition | Scholar states position |
| Position-Disagreement | ScholarlyPosition | ScholarlyPosition | Different positions on same issue |
| Term-Definition | Language:Word | Fact:Definition | Word defined by fact |

### 11.2 Link Weight Calculation

```
Link Weight = f(direct_reference, semantic_similarity, scholarly_consensus)

direct_reference: explicit citation or mention (weight 0.6)
semantic_similarity: embedding cosine similarity (weight 0.2)
scholarly_consensus: how many scholars agree on this link (weight 0.2)
```

### 11.3 Cross-Domain Navigation Example

```
User explores: "Salah (Prayer)"
  --> Fiqh: Rulings, conditions, pillars, invalidators
  --> Quran: Verses mentioning Salah (2:43, 4:103, etc.)
  --> Hadith: Narrations about Salah (Bukhari, Muslim)
  --> Seerah: When was Salah prescribed? (Isra and Mi'raj)
  --> Biography: Scholars who wrote about Salah
  --> History: Evolution of Salah practices
  --> Arabic: Linguistic meaning of "Salah"
  --> Tazkiyah: Spiritual significance
  --> Contemporary: Rulings on missed prayers, traveling
```

---

## 12. QUERY PATTERNS

### 12.1 Common Query Templates

```cypher
-- Get all facts about a concept
MATCH (c:Concept {name: 'Salah'})-[r]-(f:Fact)
RETURN f.statement, type(r) as relationship, f.confidence
ORDER BY f.confidence DESC

-- Get scholarly positions on a topic
MATCH (sp:ScholarlyPosition)-[:about]->(c:Concept {name: 'Salah_hand_position'})
MATCH (p:Person)-[:states]->(sp)
RETURN p.name, sp.position, sp.evidence, sp.confidence

-- Get provenance chain for a fact
MATCH (f:Fact {id: 'fact-001'})
MATCH path = (f)-[:derived_from*]->(s:Source)
RETURN path

-- Find contradictions in knowledge graph
MATCH (f1:Fact)-[:contradicts]->(f2:Fact)
RETURN f1.statement, f2.statement, f1.domain, f1.confidence, f2.confidence
ORDER BY f1.domain

-- Find connected concepts across domains
MATCH (c1:Concept {name: 'Salah'})-[r*1..3]-(c2:Concept)
WHERE c2.domain <> 'Fiqh'
RETURN DISTINCT c2.name, c2.domain, c2.confidence

-- Get prerequisite learning path
MATCH path = (c1:Concept {name: 'Advanced_Tafsir'})<-[:prerequisite_for*]-(c2:Concept)
RETURN [n IN nodes(path) | n.name] AS prerequisites

-- Recommend related content for user
MATCH (u:User {id: 'user-001'})-[:studied]->(c:Concept)
MATCH (c)-[:relates_to]-(rec:Concept)
WHERE NOT (u)-[:studied]->(rec)
RETURN rec.name, count(*) AS relevance
ORDER BY relevance DESC
LIMIT 10
```

### 12.2 Query Performance Targets

| Query Type | P50 | P95 | P99 |
|---|---|---|---|
| Direct neighbor lookup | 5ms | 20ms | 50ms |
| 2-hop traversal | 15ms | 50ms | 150ms |
| 3-hop traversal | 50ms | 200ms | 500ms |
| Full-text search on properties | 10ms | 50ms | 100ms |
| Path finding (shortest) | 50ms | 200ms | 1s |
| Aggregation query | 100ms | 500ms | 2s |
| Pattern matching | 200ms | 1s | 5s |
| Full provenance chain | 50ms | 200ms | 500ms |

---

## 13. KNOWLEDGE INGESTION FLOW

### 13.1 Ingestion Pipeline

```
    Source Document
         |
    [Preprocessing] -> Language detection, format normalization
         |
    [OCR Text Extraction]
         |
    [Structure Analysis] -> TOC, chapters, sections
         |
    [Entity Extraction]
         |
    [Relation Extraction]
         |
    [Fact Extraction]
         |
    [Cross-Reference]
         |
    [LLM Consistency Check]
         |
    [Contradiction Detection]
         |
    [Scholarly Review] (for sensitive domains)
         |
    [Knowledge Graph Storage]
         |
    [Provenance Recording]
         |
    [Confidence Scoring]
         |
    VERIFIED KNOWLEDGE
```

### 13.2 Ingestion Rate Targets

| Metric | Target |
|---|---|
| Books per day (OCR pipeline) | 50 |
| Books per day (digital) | 200 |
| Facts extracted per day | 5M |
| Facts verified per day | 3M |
| Time from upload to verified (book) | <48 hours |
| Time from upload to verified (digital) | <4 hours |

---

## 14. KNOWLEDGE ACCESS PATTERNS

### 14.1 Access Patterns by Consumer

| Consumer | Access Pattern | Frequency | Query Type |
|---|---|---|---|
| Content Gen Agent | Retrieve facts for a topic | Per generation | Concept -> Fact subtree |
| AI Teacher Agent | Retrieve context for question | Per user question | Semantic search + neighbor lookup |
| Quiz Agent | Retrieve facts by category | Per quiz | Concept -> Fact by type |
| User (via API) | Explore knowledge graph | Per user action | Graph traversal |
| Recommendation | Find related concepts | Per recommendation | Collaborative + graph |
| Contradiction Agent | Scan for contradictions | Continuous | Pattern matching |
| Scholar | Review and verify | On-demand | Direct node access |

### 14.2 Caching Strategy

| Cache | Content | TTL | Invalidation |
|---|---|---|---|
| Hot fact cache (Redis) | Frequently accessed facts | 1 hour | Event: fact updated |
| Query result cache (Redis) | Common query patterns | 5 minutes | Event: domain updated |
| Embedding cache (Redis) | Fact embeddings | 24 hours | Event: model updated |
| Subgraph cache (Redis) | Common subgraphs | 1 hour | Event: subgraph changed |

---

## 15. GRAPH MAINTENANCE

### 15.1 Maintenance Tasks

| Task | Frequency | Description |
|---|---|---|
| Consistency check | Daily | Detect orphaned nodes, broken edges |
| Duplicate detection | Weekly | Find and merge duplicate fact nodes |
| Stale fact review | Weekly | Flag facts below confidence threshold |
| Schema validation | Daily | Verify all nodes conform to schema |
| Index maintenance | Weekly | Rebuild indexes as needed |
| Provenance audit | Monthly | Verify provenance chain integrity |

### 15.2 Graph Health Metrics

| Metric | Healthy | Warning | Critical |
|---|---|---|---|
| Orphaned nodes | <0.1% | 0.1-0.5% | >0.5% |
| Duplicate facts | <0.01% | 0.01-0.05% | >0.05% |
| Broken edges | <0.1% | 0.1-0.5% | >0.5% |
| Facts below threshold | <5% | 5-10% | >10% |
| Query latency P99 | <200ms | 200-500ms | >500ms |
| Index usage rate | >95% | 80-95% | <80% |

---

## 16. GRAPH ANALYTICS

### 16.1 Analytics Queries

```cypher
-- Most connected concepts (hubs in the graph)
MATCH (c:Concept)-[r]-()
RETURN c.name, count(r) AS connection_count
ORDER BY connection_count DESC
LIMIT 100

-- Knowledge gaps (concepts with few facts)
MATCH (c:Concept)
WHERE NOT (c)-[:has_fact]->()
RETURN c.name, c.domain

-- Most cited sources
MATCH (s:Source)<-[:cites]-(f:Fact)
RETURN s.title, count(f) AS citation_count
ORDER BY citation_count DESC

-- Scholar influence (how many students, how often cited)
MATCH (p:Person {entity_type: 'Scholar'})
OPTIONAL MATCH (p)-[:taught]->(student:Person)
OPTIONAL MATCH (:Fact)-[:states]->()-[:attributed_to]->(p)
RETURN p.name, count(DISTINCT student) AS students, count(*) AS influence_score
ORDER BY influence_score DESC

-- Cross-domain density
MATCH (c1:Concept)-[r]-(c2:Concept)
WHERE c1.domain <> c2.domain
RETURN c1.domain, c2.domain, count(r) AS cross_links
ORDER BY cross_links DESC
```

---

## 17. PERFORMANCE OPTIMIZATION

### 17.1 Indexing Strategy

```sql
-- Primary indexes for AGE graph
SELECT * FROM cypher('islamic_knowledge', $$
    CREATE INDEX ON :Concept(name)
$$) AS (result text);

SELECT * FROM cypher('islamic_knowledge', $$
    CREATE INDEX ON :Fact(domain)
$$) AS (result text);

SELECT * FROM cypher('islamic_knowledge', $$
    CREATE INDEX ON :Fact(confidence)
$$) AS (result text);

SELECT * FROM cypher('islamic_knowledge', $$
    CREATE INDEX ON :Source(title)
$$) AS (result text);

-- Composite indexes for common queries
SELECT * FROM cypher('islamic_knowledge', $$
    CREATE INDEX ON :Concept(domain, name)
$$) AS (result text);

SELECT * FROM cypher('islamic_knowledge', $$
    CREATE INDEX ON :Fact(domain, confidence)
$$) AS (result text);
```

### 17.2 Query Optimization Guidelines

| Issue | Solution |
|---|---|
| Full graph scans | Add appropriate labels and indexes |
| Deep traversals (>5 hops) | Limit depth, use shortestPath |
| Large property JSONB | Extract frequently queried properties to columns |
| High-degree nodes (hub concepts) | Paginate or limit results |
| Frequent pattern matches | Materialized views for common patterns |
| Text searches | Use Elasticsearch, not graph text matching |

---

## 18. APPENDICES

### Appendix A: Ontology Validation Rules

| Rule | Description | Enforcement |
|---|---|---|
| Every fact has at least one source | Provenance completeness | Mandatory |
| Every edge has a type | Type consistency | Mandatory |
| Every node has a domain | Domain classification | Mandatory |
| Every fact has a subject, predicate, object | Triple completeness | Mandatory |
| Confidence is between 0 and 1 | Range validation | Mandatory |
| No duplicate edges of same type between same nodes | Uniqueness | Mandatory |
| Temporary facts have expiry dates | Temporality | Conditional |

### Appendix B: Sample Knowledge Graph Subgraph

```
(Salah:Concept {domain: 'Fiqh'})
  -[:is_a]-> (Ibadah:Concept)
  -[:has_pillar]-> (Takbir:Concept)
  -[:has_pillar]-> (Qiyam:Concept)
  -[:has_pillar]-> (Ruku:Concept)
  -[:has_pillar]-> (Sujud:Concept)
  -[:has_condition]-> (Wudu:Concept)
  -[:has_condition]-> (Taharah:Concept)
  -[:has_condition]-> (Qibla:Concept)
  -[:has_condition]-> (Niyyah:Concept)
  -[:cites]-> (Quran_2_43:Fact {statement: "Establish prayer..."})
  -[:cites]-> (Hadith_Bukhari_1:Fact {statement: "Pray as you saw me pray"})
  -[:obligatory_for]-> (Adult_Muslim:Concept)
  -[:scholarly_disagreement]-> (Ruling_on_combining_prayers:ScholarlyPosition)

(Imam_Abu_Hanifa:Person {entity_type: 'Scholar'})
  -[:states]-> (Hanafi_Position_Takbir:ScholarlyPosition)
  -[:states]-> (Hanafi_Position_Hands:ScholarlyPosition)

(Imam_Shafii:Person {entity_type: 'Scholar'})
  -[:states]-> (Shafii_Position_Takbir:ScholarlyPosition)
  -[:states]-> (Shafii_Position_Hands:ScholarlyPosition)

(Hanafi_Position_Hands:ScholarlyPosition)
  -[:differs_from]-> (Shafii_Position_Hands:ScholarlyPosition)
  -[:on]-> (Hand_position_in_Salah:Concept)

(Battle_of_Badr:Event)
  -[:occurs_at]-> (Badr:Location)
  -[:occurs_in]-> (Year_2_AH:Time)
  -[:involved]-> (Prophet_Muhammad:Person)
  -[:involved]-> (Abu_Jahl:Person)
  -[:described_in]-> (Quran_3_123:Fact)
```

### Appendix C: Related Documents

| Document ID | Title | Relationship |
|---|---|---|
| IEP-CONST-001 | Project Constitution | Governing principles |
| IEP-ARCH-001 | System Architecture | System context |
| IEP-ARCH-DB-001 | Database Architecture | Storage implementation |
| IEP-ARCH-RAG-001 | RAG Architecture | Graph-based retrieval |
| IEP-ARCH-VRF-001 | Verification Architecture | Fact verification |
| IEP-AGENTS-001 | Agents Specification | Agents that read/write graph |
| IEP-ARCH-AGENT-001 | Agent Architecture | Knowledge access patterns |

---

**END OF KNOWLEDGE GRAPH ARCHITECTURE**
