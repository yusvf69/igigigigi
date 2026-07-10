# AGENTS SPECIFICATION

## Islamic Education Platform (IEP)

---

| Document ID | IEP-AGENTS-001 |
|---|---|
| Version | 1.0.0 |
| Status | Approved |
| Classification | Enterprise Internal |
| Effective Date | 2026-07-07 |
| Approving Authority | Office of the Chief Architect |
| Custodian | Architecture Review Board |
| Constitutional Reference | IEP-CONST-001 (Section 6, 12) |
| Agent Architecture Reference | IEP-ARCH-AGENT-001 |

---

## TABLE OF CONTENTS

1. EXECUTIVE OVERVIEW
2. AGENT SPECIFICATION TEMPLATE
3. EXTRACTION AGENTS
4. VERIFICATION AGENTS
5. CONTENT GENERATION AGENTS (CORE)
6. LEARNING AGENTS
7. QUALITY AGENTS
8. ADMINISTRATIVE AGENTS
9. AGENT INTERACTION MATRIX
10. AGENT DATA FLOW DIAGRAMS
11. AGENT PERFORMANCE BUDGETS
12. AGENT DEPENDENCY MAP
13. APPENDICES

---

## 1. EXECUTIVE OVERVIEW

### 1.1 Purpose

This document provides the complete specifications for every AI Agent in the Islamic Education Platform. Each agent specification follows the standardized 33-section template defined in the Project Constitution, ensuring consistency across all agent implementations. Content generation agents for specific learning modes (Story, Quiz, Flashcard, Podcast, Video, Animation, Gamification) are defined in their respective dedicated documents (folders 12-18) and are referenced here.

### 1.2 Agent Inventory Summary

| Domain | Count | Agent Types |
|---|---|---|
| Extraction | 8 | OCR, Layout, Entity, Relation, Fact, Structure, Verse, Hadith |
| Verification | 7 | Cross-Ref, LLM Check, Contradiction, Source, Scholarly Review, Continuous Verification, Citation |
| Content Generation | 18 | Story, Documentary, Timeline, Map, Character, Quiz, Flashcard, Podcast, Dialogue, Debate, Game, Simulation, Visual, Whiteboard, Video, Translation, TTS, Image |
| Learning | 7 | AI Teacher, Adaptive Path, Assessment, Recommendation, Progress, Spaced Rep, Remediation |
| Quality | 7 | Accuracy, Relevance, Completeness, Style, Bias, Accessibility, Pedagogy |
| Administrative | 4 | Monitoring, Audit, Config, Maintenance |
| **Total** | **51** | |

---

## 2. AGENT SPECIFICATION TEMPLATE

Every agent in the IEP follows this 33-section specification template:

```yaml
agent_specification:
  # ============================================================
  # 1. AGENT NAME
  # ============================================================
  agent_name: "<unique agent identifier>"
  
  # ============================================================
  # 2. MISSION
  # ============================================================
  mission: "<one-sentence mission statement>"
  
  # ============================================================
  # 3. VISION
  # ============================================================
  vision: "<long-term vision for this agent>"
  
  # ============================================================
  # 4. RESPONSIBILITIES
  # ============================================================
  responsibilities: ["<list of specific responsibilities>"]
  
  # ============================================================
  # 5. NON RESPONSIBILITIES
  # ============================================================
  non_responsibilities: ["<explicitly what this agent does NOT do>"]
  
  # ============================================================
  # 6. INPUTS
  # ============================================================
  inputs:
    - name: "<parameter name>"
      type: "<data type>"
      description: "<description>"
      required: true|false
      source: "<where this input comes from>"
      schema: "<reference to schema>"
  
  # ============================================================
  # 7. OUTPUTS
  # ============================================================
  outputs:
    - name: "<output name>"
      type: "<data type>"
      description: "<description>"
      target: "<where output goes>"
      schema: "<reference to schema>"
  
  # ============================================================
  # 8. INTERNAL MEMORY
  # ============================================================
  internal_memory:
    working_memory_size: "<size limit>"
    working_memory_ttl: "<time-to-live>"
    short_term_storage: "<technology>"
    short_term_ttl: "<time-to-live>"
  
  # ============================================================
  # 9. EXTERNAL MEMORY
  # ============================================================
  external_memory:
    long_term_storage: "<technology>"
    knowledge_source: "<which knowledge sources>"
    memory_retrieval: "<retrieval pattern>"
  
  # ============================================================
  # 10. DATABASES USED
  # ============================================================
  databases_used: ["<list of databases with access pattern>"]
  
  # ============================================================
  # 11. KNOWLEDGE SOURCES
  # ============================================================
  knowledge_sources:
    primary: ["<primary knowledge sources>"]
    secondary: ["<fallback knowledge sources>"]
  
  # ============================================================
  # 12. APIS
  # ============================================================
  apis:
    - name: "<API name>"
      protocol: "<gRPC/REST/Kafka>"
      endpoint: "<endpoint>"
      purpose: "<why it's called>"
  
  # ============================================================
  # 13. MODELS
  # ============================================================
  models:
    primary: "<model name>"
    fallback: "<fallback model name>"
    fine_tuned: "<any fine-tuned model>"
    parameters:
      temperature: 0.3
      max_tokens: 4096
  
  # ============================================================
  # 14. PROMPTING STRATEGY
  # ============================================================
  prompting_strategy:
    approach: "<Direct/CoT/ReAct/etc>"
    templates: ["<prompt template IDs>"]
    few_shot_count: <number of examples>
    system_prompt: "<key instructions>"
  
  # ============================================================
  # 15. PLANNING STRATEGY
  # ============================================================
  planning_strategy:
    approach: "<Direct/Chain/DAG/ReAct/etc>"
    max_steps: <maximum steps>
    replanning: "<strategy for replanning>"
  
  # ============================================================
  # 16. REASONING STRATEGY
  # ============================================================
  reasoning_strategy:
    approach: "<Direct/CoT/Reflexion/Consistency/etc>"
    passes: <number of passes for self-consistency>
  
  # ============================================================
  # 17. VALIDATION STRATEGY
  # ============================================================
  validation_strategy:
    layers: ["<validation layers>"]
    rules: ["<validation rules>"]
    auto_approve_threshold: <confidence value>
  
  # ============================================================
  # 18. ERROR RECOVERY
  # ============================================================
  error_recovery:
    retryable_errors: ["<list>"]
    non_retryable_errors: ["<list>"]
    fallback_behavior: "<description>"
    escalation_path: "<who to escalate to>"
  
  # ============================================================
  # 19. RETRY LOGIC
  # ============================================================
  retry_logic:
    max_retries: <number>
    initial_delay_ms: <ms>
    backoff_multiplier: <factor>
    max_delay_ms: <ms>
  
  # ============================================================
  # 20. SELF EVALUATION
  # ============================================================
  self_evaluation:
    method: "<Reflexion/Consistency/etc>"
    dimensions: ["<evaluation dimensions>"]
    feedback_loop: "<how feedback improves agent>"
  
  # ============================================================
  # 21. CONFIDENCE SCORING
  # ============================================================
  confidence_scoring:
    dimensions:
      - name: "<dimension>"
        weight: <weight 0-1>
        method: "<calculation method>"
    overall: "<aggregation method>"
  
  # ============================================================
  # 22. LOGGING
  # ============================================================
  logging:
    log_level: "<INFO/DEBUG/TRACE>"
    log_events: ["<events to log>"]
    retention_days: <number>
  
  # ============================================================
  # 23. MONITORING
  # ============================================================
  monitoring:
    metrics: ["<key metrics>"]
    alerts: ["<alert conditions>"]
    dashboard: "<dashboard reference>"
  
  # ============================================================
  # 24. METRICS
  # ============================================================
  metrics:
    performance: ["<latency, throughput>"]
    quality: ["<accuracy, completeness>"]
    business: ["<cost, utilization>"]
  
  # ============================================================
  # 25. PERFORMANCE
  # ============================================================
  performance:
    p50_latency: "<target>"
    p95_latency: "<target>"
    p99_latency: "<target>"
    throughput_per_instance: "<target>"
  
  # ============================================================
  # 26. SECURITY
  # ============================================================
  security:
    authentication: "<method>"
    authorization: "<level>"
    data_sensitivity: "<classification>"
    encryption: "<encryption standard>"
  
  # ============================================================
  # 27. PRIVACY
  # ============================================================
  privacy:
    data_handling: "<how data is handled>"
    pii_detection: true|false
    retention_policy: "<policy>"
  
  # ============================================================
  # 28. FAILURE CASES
  # ============================================================
  failure_cases: ["<known failure modes and mitigations>"]
  
  # ============================================================
  # 29. HUMAN REVIEW POINTS
  # ============================================================
  human_review_points:
    - condition: "<when human review is triggered>"
      reviewer_type: "<who reviews>"
      escalation_after: "<time before escalation>"
  
  # ============================================================
  # 30. COMPLETE WORKFLOW
  # ============================================================
  complete_workflow:
    steps: ["<step-by-step execution flow>"]
    decision_points: ["<where decisions are made>"]
    state_transitions: ["<state machine>"]
  
  # ============================================================
  # 31. EXAMPLES
  # ============================================================
  examples:
    - input: "<example input>"
      output: "<expected output>"
      explanation: "<why this is correct>"
  
  # ============================================================
  # 32. INTEGRATION WITH ALL OTHER AGENTS
  # ============================================================
  integrations:
    - agent: "<other agent name>"
      pattern: "<communication pattern>"
      protocol: "<protocol>"
      data_exchanged: "<what data>"
  
  # ============================================================
  # 33. FUTURE EXPANSION
  # ============================================================
  future_expansion:
    planned_improvements: ["<list>"]
    potential_new_capabilities: ["<list>"]
    deprecation_path: "<if applicable>"
```

---

## 3. EXTRACTION AGENTS

### 3.1 OCR Agent (A-EXT-001)

---

**1. AGENT NAME**: `ocr-agent`

**2. MISSION**: Extract clean, accurate, machine-readable text from scanned books and documents across all supported languages, with specialized optimization for Arabic script and classical Islamic manuscripts.

**3. VISION**: To become the world's most accurate OCR engine for Islamic manuscripts and Arabic-script documents, achieving >99.5% character accuracy for classical texts and >99.9% for modern publications.

**4. RESPONSIBILITIES**:
- Accept uploaded book files in various formats (PDF, TIFF, JPG, PNG).
- Preprocess images (deskew, denoise, binarize, despeckle).
- Detect script type, language, and writing style.
- Perform OCR with language-specific models.
- Detect and correct common OCR errors (Arabic ligature handling, diacritic restoration).
- Output structured text with bounding box coordinates and confidence scores.
- Detect page layout (columns, headers, footnotes, marginalia).
- Report OCR quality metrics and flag low-confidence regions for human review.

**5. NON RESPONSIBILITIES**:
- Does NOT interpret or understand the text content.
- Does NOT extract semantic meaning or entities.
- Does NOT verify factual accuracy of the text.
- Does NOT handle handwritten manuscripts (future scope).

**6. INPUTS**:
| Name | Type | Required | Source | Schema |
|---|---|---|---|---|
| book_id | UUID | Yes | Book Ingestion Service | book_ingestion_schema |
| file_url | URL | Yes | Object Store | s3_url |
| file_format | Enum | Yes | Upload metadata | - |
| languages | String[] | Yes | Book metadata | ISO 639-1 codes |
| pages | Integer[] | No | Selected page range | - |
| quality_threshold | Float | No (default: 0.95) | Configuration | 0.0-1.0 |
| preprocessing_params | Object | No | Configuration | ocr_preprocessing_schema |

**7. OUTPUTS**:
| Name | Type | Target | Schema |
|---|---|---|---|
| extracted_text | String | Kafka topic: knowledge.ocr.completed | ocr_result_schema |
| page_metadata | PageMeta[] | Kafka topic | page_metadata_schema |
| confidence_scores | ConfidenceMap | Kafka topic | - |
| layout_blocks | LayoutBlock[] | Kafka topic | layout_block_schema |
| quality_report | OCRQualityReport | Kafka topic | ocr_quality_schema |

**8. INTERNAL MEMORY**:
- Working Memory: Current book pages in batch (500 pages max).
- Short-Term Memory: Book-level processing context (session duration).

**9. EXTERNAL MEMORY**:
- Long-Term Storage: PostgreSQL for OCR job metadata.
- Knowledge Source: Language-specific OCR models stored in model registry.

**10. DATABASES USED**:
- PostgreSQL (job metadata, quality metrics).
- MinIO (intermediate OCR results, processed images).

**11. KNOWLEDGE SOURCES**:
- Primary: Fine-tuned OCR models (custom Arabic Nafez/CAMeL + Tesseract).
- Secondary: Cloud OCR APIs (Google Cloud Vision, Azure OCR).
- Reference: Unicode character database, Arabic script grammar rules.

**12. APIS**:
| Name | Protocol | Endpoint | Purpose |
|---|---|---|---|
| Book Ingestion Service | gRPC | book-ingestion:9090 | Fetch book metadata |
| Object Store | S3 API | minio:9000 | Read/write files |
| Model Registry | gRPC | model-registry:9090 | Fetch OCR model |
| Preprocessing Service | gRPC | image-preprocessing:9090 | Image enhancement |

**13. MODELS**:
| Role | Model | Version | Notes |
|---|---|---|---|
| Primary (Arabic) | Custom Fine-tuned CRNN + Transformer | v2.1 | Fine-tuned on 50K+ Islamic manuscript pages |
| Primary (English/Latin) | Tesseract LSTM | 5.x | Default for non-Arabic |
| Fallback | Google Cloud Vision OCR | - | When primary fails |
| Layout Analysis | LayoutLMv3 | Fine-tuned | Document layout detection |
| Language Detection | FastText | Pretrained | Language identification |

**14. PROMPTING STRATEGY**: Not applicable (OCR is a specialized ML model, not LLM-based). Quality assessment uses few-shot prompting for error detection.

**15. PLANNING STRATEGY**:
- Approach: Pipeline (Preprocess -> Layout Detect -> OCR -> Postprocess -> Quality Check).
- Max Steps: 5 (with page-level parallelism).
- Replanning: If quality < threshold, retry with alternative preprocessing.

**16. REASONING STRATEGY**: Not applicable. Deterministic ML pipeline with confidence-based decision gates.

**17. VALIDATION STRATEGY**:
- Layers: [[Format, Confidence, Consistency, Completeness]].
- Rules:
  - Minimum average confidence > 0.95.
  - Character error rate < 1% (estimated).
  - No empty pages for non-empty source images.
  - Layout block completeness > 90%.
- Auto-approve threshold: Confidence > 0.95.

**18. ERROR RECOVERY**:
| Error | Retryable | Fallback |
|---|---|---|
| Image corrupt | No | Skip page, report |
| OCR model unavailable | Yes (3x) | Switch to fallback model |
| Quality below threshold | Yes (2x, different preprocessing) | Flag for human review |
| Timeout | Yes (2x) | Split batch, retry |

**19. RETRY LOGIC**:
- Max retries: 3.
- Initial delay: 5s.
- Backoff multiplier: 2.0.
- Max delay: 60s.

**20. SELF EVALUATION**: Confidence scoring per character, per page, per document. Quality metrics compared against historical baseline. Pages below threshold retried or flagged.

**21. CONFIDENCE SCORING**:
| Dimension | Weight | Method |
|---|---|---|
| Per-character confidence mean | 0.4 | Model logits average |
| Layout detection accuracy | 0.2 | Block boundary precision |
| Language consistency | 0.2 | N-gram language model score |
| Diacritic restoration accuracy | 0.1 | Reference comparison |
| Ligature handling correctness | 0.1 | Rule-based check |

**22. LOGGING**:
- Level: INFO (per page), DEBUG (per character).
- Events: [[job_start, page_complete, quality_check, job_complete, error]].
- Retention: 90 days (raw), 30 days (debug).

**23. MONITORING**:
- Metrics: [[pages_per_minute, avg_confidence, error_rate, quality_distribution]].
- Alerts: [[avg_confidence < 0.90 for 10 consecutive pages, error_rate > 5%]].
- Dashboard: "OCR Agent Performance" (page throughput, quality trends, error breakdown).

**24. METRICS**:
| Category | Metrics |
|---|---|
| Performance | Pages/minute, avg page latency, batch throughput |
| Quality | Mean confidence, pages below threshold, layout accuracy |
| Business | Pages processed, jobs completed, cost per page |

**25. PERFORMANCE**:
| Metric | Target |
|---|---|
| P50 latency per page | 30s |
| P95 latency per page | 120s |
| P99 latency per page | 300s |
| Throughput (per instance) | 10 pages/min |
| Max batch size | 500 pages |

**26. SECURITY**:
- Authentication: mTLS between services.
- Authorization: Service-level RBAC.
- Data sensitivity: Internal (book content).
- Encryption: TLS 1.3 in transit, AES-256 at rest.

**27. PRIVACY**:
- Data handling: Book content processed in-memory, not retained longer than needed.
- PII detection: Yes (email, phone, address detection in general books).
- Retention: Processed images deleted after quality check.

**28. FAILURE CASES**:
| Failure | Mitigation |
|---|---|
| Poor quality scan | Preprocessing enhancement, human flag |
| Mixed language pages | Language segmentation before OCR |
| Complex layouts (marginalia, tables) | Layout-aware postprocessing |
| Very low confidence across book | Escalate to human transcription |
| Model out of memory | Page batching, memory limits |

**29. HUMAN REVIEW POINTS**:
| Condition | Reviewer | Escalation |
|---|---|---|
| Page confidence < 0.80 | OCR Quality Reviewer | 24 hours |
| Book avg confidence < 0.85 | OCR Quality Reviewer | 24 hours |
| Layout detection failure | Document Specialist | 48 hours |
| Inconsistent text across editions | Content Specialist | 48 hours |

**30. COMPLETE WORKFLOW**:
```
1. Receive task from Orchestrator (book_id, file_url, languages)
2. Validate input (book exists, file accessible, format supported)
3. Download book from Object Store
4. For each page (parallel batch of 50):
   a. Load image
   b. Preprocess (deskew, denoise, binarize)
   c. Detect layout (text blocks, columns, headers, footnotes)
   d. Detect language per block
   e. Run OCR with appropriate model
   f. Postprocess (ligature correction, diacritic restoration)
   g. Calculate confidence scores
   h. Store intermediate result
5. Aggregate page results
6. Calculate overall quality score
7. If quality >= threshold:
   a. Publish extracted text and metadata to Kafka
   b. Report completion to Orchestrator
8. If quality < threshold:
   a. Flag for human review
   b. Report with quality warning
9. Cleanup intermediate files
```

**31. EXAMPLES**:
```json
// Input
{
  "book_id": "book-001",
  "file_url": "s3://books/ar/kitab-al-tawheed.pdf",
  "file_format": "pdf",
  "languages": ["ar"],
  "quality_threshold": 0.95
}

// Output (simplified)
{
  "book_id": "book-001",
  "total_pages": 250,
  "processed_pages": 250,
  "avg_confidence": 0.972,
  "pages_flagged": 3,
  "text": {
    "page_001": {
      "blocks": [
        {
          "type": "header",
          "text": "كتاب التوحيد",
          "confidence": 0.99,
          "bbox": {"x1": 100, "y1": 50, "x2": 500, "y2": 80}
        },
        {
          "type": "body",
          "text": "بسم الله الرحمن الرحيم\nالحمد لله الذي...",
          "confidence": 0.97,
          "bbox": {"x1": 80, "y1": 100, "x2": 520, "y2": 700}
        }
      ]
    }
  },
  "quality_report": {
    "overall_score": 0.97,
    "dimensions": {
      "character_accuracy": 0.98,
      "layout_accuracy": 0.96,
      "language_consistency": 0.99,
      "diacritic_accuracy": 0.94
    },
    "flagged_pages": [
      {"page": 47, "confidence": 0.72, "reason": "low_quality_scan"},
      {"page": 123, "confidence": 0.78, "reason": "mixed_language"},
      {"page": 200, "confidence": 0.65, "reason": "damaged_page"}
    ]
  }
}
```

**32. INTEGRATION WITH ALL OTHER AGENTS**:
| Agent | Pattern | Protocol | Data |
|---|---|---|---|
| Layout Agent | Sequential (pipeline) | gRPC | OCR text + metadata |
| Entity Agent | Sequential (pipeline) | Kafka | Extracted text |
| Structure Agent | Sequential (pipeline) | gRPC | Layout blocks |
| Verse Agent | Sequential (pipeline) | Kafka | Extracted text |
| Hadith Agent | Sequential (pipeline) | Kafka | Extracted text |
| Verification Orchestrator | Event (pub) | Kafka | Completion event |

**33. FUTURE EXPANSION**:
- Handwriting recognition for classical manuscript marginalia.
- Multi-script detection (Ottoman Turkish, Persian, Urdu, Pashto, Sindhi).
- End-to-end Arabic text recognition without character segmentation.
- Real-time OCR for live document capture.
- Automatic script/style classification for dating manuscripts.

---

### 3.2 Layout Agent (A-EXT-002)

---

**1. AGENT NAME**: `layout-agent`

**2. MISSION**: Analyze extracted document text and image data to detect, classify, and structure the logical layout of book pages including chapters, sections, subsections, paragraphs, headers, footnotes, marginalia, tables, diagrams, and special text blocks.

**3. VISION**: To reconstruct the complete logical document structure of any Islamic text, preserving the hierarchical organization that scholars rely on for navigation and reference.

**4. RESPONSIBILITIES**:
- Detect chapter/section boundaries from OCR output.
- Classify text blocks (header, body, footnote, marginalia, caption, table).
- Build hierarchical table of contents.
- Detect paragraph boundaries and reading order.
- Identify special elements (Quranic verses, Hadith, poetry, formulas).
- Detect cross-references and internal links.
- Output structured document model.
- Handle multi-column layouts and complex page geometries.

**5. NON RESPONSIBILITIES**:
- Does NOT extract semantic meaning or entities.
- Does NOT verify content accuracy.
- Does NOT OCR (receives OCR output).
- Does NOT generate summaries or abstracts.

**6. INPUTS**: OCR text with bounding boxes, page images, book metadata.

**7. OUTPUTS**: Hierarchical document structure, TOC, block classifications, reading order, special element annotations.

**8-33**: ([Following the same complete template structure as OCR Agent, adapted for layout analysis])

---

### 3.3 Entity Agent (A-EXT-003) through 3.8 Hadith Agent (A-EXT-008)

(Each follows the identical 33-section template structure. Due to the volume, I will provide the key differentiating specifications for each.)

---

**Entity Agent (A-EXT-003)** - Named Entity Recognition for Islamic text.

- **Responsibility**: Extract named entities: People (scholars, narrators, historical figures), Places (cities, regions, battle locations), Concepts (Islamic terms, Fiqh concepts), Books (titles, authors), Events (battles, treaties, significant dates), Groups (tribes, schools of thought, dynasties).
- **Models**: Fine-tuned Arabic BERT-based NER (custom Islamic NER dataset of 100K+ annotated entities), spaCy for fallback.
- **Knowledge Sources**: Historical gazetteer of Islamic places, scholar biographical database (Tabaqat), book title authority list, concept ontology.
- **Entity Types**: PERSON, LOCATION, CONCEPT, BOOK, EVENT, GROUP, DATE_PERIOD, MADHAB, TERM_ARABIC, TERM_ENGLISH.
- **Confidence**: Per-entity confidence based on NER model probability + post-processing rules.

**Relation Agent (A-EXT-004)** - Extract relationships between entities.

- **Responsibility**: Extract semantic relationships between extracted entities (e.g., "authored_by", "studied_under", "located_in", "part_of", "states", "supports", "contradicts").
- **Models**: Fine-tuned relation extraction model (based on Arabic RoBERTa + custom relation classifier).
- **Relation Types**: authored_by, taught, studied_under, narrated_from, located_in, part_of, states, supports, contradicts, refutes, cites, based_on, translation_of, commentary_on.
- **Confidence**: Pairwise relation confidence + cross-validation with known relationships from knowledge base.

**Fact Agent (A-EXT-005)** - Extract atomic facts as triples.

- **Responsibility**: Extract atomic facts (Subject-Predicate-Object triples) from document text, grounded in extracted entities and relations.
- **Models**: GPT-4o for fact extraction with structured output constraints.
- **Prompt Strategy**: Chain-of-Thought with explicit instruction to only extract facts explicitly stated in text, never infer or add information.
- **Fact Types**: [Concept]-[Relation]-[Value], [Entity]-[Attribute]-[Value], [Event]-[Property]-[Value].
- **Validation**: All facts must have provenance (document ID, page, position). Cross-check with existing Knowledge Graph for duplicates.
- **Confidence**: Based on extraction model confidence + source text clarity + relation consistency.

**Structure Agent (A-EXT-006)** - Extract book structure.

- **Responsibility**: Extract and normalize the book's structural organization (front matter, chapters, sections, subsections, appendices, indexes, bibliography).
- **Models**: LayoutLMv3 for visual structure + GPT-4o-mini for logical structure normalization.
- **Output**: Normalized TOC in standard IEP format with chapter/section IDs, titles, page ranges, and hierarchical relationships.

**Verse Agent (A-EXT-007)** - Extract Quranic verse citations.

- **Responsibility**: Detect, extract, and verify all Quranic verse citations (ayat) within the text. Normalize to standard Surah:Ayah format. Verify against the master Quran text.
- **Models**: Custom regex + fine-tuned Arabic BERT for verse boundary detection + exact text matching against verified Quran text database.
- **Verification**: Every extracted verse is matched against the official verified Quran text. Mismatches are flagged.
- **Output**: Surah number, Ayah number, extracted text, matched text, confidence, position in document.

**Hadith Agent (A-EXT-008)** - Extract Hadith citations.

- **Responsibility**: Detect and extract Hadith citations, including the chain (sanad), text (matn), grading (hukm), and source collection reference.
- **Models**: Custom Hadith detection model + rule-based pattern matching for known Hadith collections.
- **Verification**: Cross-reference against verified Hadith database (Sahih Bukhari, Sahih Muslim, etc.). Detect fabrications and misattributions.
- **Output**: Source collection, book number, Hadith number, narrators chain, text, grading, confidence.

---

## 4. VERIFICATION AGENTS

### 4.1 Cross-Reference Agent (A-VRF-001)

---

**1. AGENT NAME**: `cross-reference-agent`

**2. MISSION**: Verify extracted facts by cross-referencing them against multiple trusted sources in the knowledge base, detecting corroboration, contradiction, or lack of evidence for each asserted fact.

**3. VISION**: To create the most comprehensive cross-referencing system for Islamic knowledge, where every fact is verified against all available authoritative sources.

**4. RESPONSIBILITIES**:
- Receive facts from Fact Extraction Agent.
- For each fact, identify potential matching facts in the existing Knowledge Graph.
- Compare facts against multiple trusted source documents.
- Calculate corroboration score based on number and authority of supporting sources.
- Detect contradictions between the new fact and existing knowledge.
- Flag facts that cannot be corroborated by any existing source.
- Generate verification evidence with supporting quotes from sources.
- Output verification result with confidence score.

**5. NON RESPONSIBILITIES**:
- Does NOT generate new facts or interpretations.
- Does NOT make judgments about scholarly correctness (that is Scholarly Review Agent).
- Does NOT modify the Knowledge Graph directly.

**6. INPUTS**: Facts list (with provenance), Knowledge Graph query capability.

**7. OUTPUTS**: Per-fact verification results with corroboration scores.

**8-33**: ([Complete template adapted for cross-reference verification, including:])
- **Models**: Embedding model for semantic fact matching + GPT-4o-mini for fact comparison.
- **Retrieval Strategy**: Hybrid (vector similarity + keyword + graph traversal) across all relevant source documents.
- **Confidence Dimensions**: Source count (weight 0.3), source authority (weight 0.3), consistency (weight 0.2), semantic match quality (weight 0.2).
- **Verification Levels**: CONFIRMED (3+ independent high-authority sources), SUPPORTED (1-2 sources), UNVERIFIED (no matching source), CONTRADICTED (conflicting sources), UNCERTAIN (ambiguous evidence).
- **Human Review**: CONTRADICTED and UNCERTAIN facts sent to Scholarly Review Agent.

---

### 4.2 LLM Check Agent (A-VRF-002) through 4.7 Citation Agent (A-VRF-007)

(Following the same 33-section template structure.)

**LLM Check Agent (A-VRF-002)**: Uses LLM reasoning to assess factual plausibility and consistency.
- **Model**: Claude Opus (primary), GPT-4o (fallback) - highest capability models for critical reasoning.
- **Prompt Strategy**: Reflexion (generate assessment, self-critique, refine).
- **Assessment Dimensions**: Factual consistency with provided sources, internal consistency, plausibility, completeness, clarity.
- **Output**: Plausibility score, consistency score, identified issues, suggested corrections.
- **Threshold**: Below 0.70 triggers human review.

**Contradiction Agent (A-VRF-003)**: Detects contradictions across the entire Knowledge Graph.
- **Execution**: Scheduled continuous (daily full scan + event-driven on new facts).
- **Model**: Fine-tuned NLI (Natural Language Inference) model for Arabic + GPT-4o for complex contradictions.
- **Process**: Scan all facts for pairs with overlapping subjects/predicates but differing objects. Classify as CONTRADICTORY, COMPLEMENTARY, or INDEPENDENT.
- **Output**: Contradiction report with fact pairs, contradiction type, severity, and suggested resolution.
- **Human Review**: All confirmed contradictions require human scholarly review.

**Source Agent (A-VRF-004)**: Verify source document authenticity and chain of custody.
- **Verification**: Cryptographic hash verification, metadata consistency, publisher verification, chain of custody audit.
- **Output**: Source authenticity score, provenance chain, tamper evidence.

**Scholarly Review Agent (A-VRF-005)**: Manage human scholarly review workflow.
- **Role**: Routing agent that manages the human review queue for verification tasks.
- **Function**: Assign to appropriate scholar based on expertise, track deadlines, manage escalation, incorporate feedback.
- **Integration**: Works with the Human-in-Loop system in the Orchestrator.

**Continuous Verification Agent (A-VRF-006)**: Scheduled re-verification of stored knowledge.
- **Schedule**: Varies by knowledge domain (critical: daily, standard: weekly, archival: monthly).
- **Process**: Re-extract facts from source documents, compare with stored facts, report discrepancies.
- **Trigger**: Also triggered when new authoritative sources are added that may affect existing knowledge.

**Citation Agent (A-VRF-007)**: Verify and normalize all citations in generated content.
- **Function**: Ensure every generated content piece has proper citations to source facts.
- **Verification**: Check each citation references an existing fact, the fact supports the claim, the provenance is accurate.
- **Output**: Citation completeness score, missing citations, incorrect citations.

---

## 5. CONTENT GENERATION AGENTS (CORE)

### 5.1 Content Generation Orchestrator (A-CNT-001)

---

**1. AGENT NAME**: `content-generation-orchestrator`

**2. MISSION**: Coordinate all content generation agents to produce high-quality, multi-modal educational content from verified knowledge, ensuring consistency, accuracy, and pedagogical effectiveness across all learning modes.

**3. VISION**: To be the central intelligence that transforms verified knowledge into the richest possible set of learning experiences, orchestrating multiple specialized agents to create a complete educational ecosystem from any knowledge input.

**4. RESPONSIBILITIES**:
- Receive content generation requests (book-based, topic-based, or mode-specific).
- Determine which content agents to invoke based on requested modes.
- Fan-out tasks to multiple content generation agents in parallel.
- Collect and validate results from all agents.
- Ensure cross-mode consistency (same facts in story as in quiz).
- Manage content quality gates before publishing.
- Handle partial failures (one mode fails, others succeed).

**5. NON RESPONSIBILITIES**:
- Does NOT generate content directly.
- Does NOT modify the Knowledge Graph.
- Does NOT deliver content to end users.

**6-33**: (Full template following the standard structure, adapted for orchestration role.)

---

### 5.2 Translation Agent (A-CNT-002)

---

**1. AGENT NAME**: `translation-agent`

**2. MISSION**: Translate knowledge facts and generated content between all supported languages while preserving Islamic terminology accuracy, scholarly precision, and contextual meaning.

**3. VISION**: To break down all language barriers in Islamic education, enabling any learner to access authentic knowledge in their native language with the same depth and accuracy as the original.

**4. RESPONSIBILITIES**:
- Translate Knowledge Graph facts between languages.
- Translate generated content (stories, quizzes, etc.) to target languages.
- Preserve Islamic terminology with proper transliteration and explanation.
- Handle Arabic-specific constructs (Quranic references, Hadith chains, scholarly titles).
- Maintain accuracy of religious concepts across languages.
- Flag ambiguous or culturally sensitive translations for human review.
- Support RTL and LTR mixed text.

**5. NON RESPONSIBILITIES**:
- Does NOT generate original content.
- Does NOT interpret or explain meaning (pure translation).
- Does NOT verify factual accuracy of translated content.

**6. INPUTS**: Source text, source language, target language, content type, domain context, terminology glossary.

**7. OUTPUTS**: Translated text, translation confidence, flagged terms, alternative translations.

**8-33**: Full template with:
- **Models**: Gemini 1.5 Pro (primary for translation), GPT-4o (fallback), custom Islamic terminology model.
- **Knowledge Sources**: Islamic terminology database (50K+ terms with translations), Quran translation corpus, Hadith translation corpus, scholar name authority list.
- **Validation**: Round-trip verification (translate to target, back to source, compare), terminology consistency check, human review for sensitive domains.
- **Confidence Dimensions**: Semantic preservation (0.4), terminology accuracy (0.3), fluency (0.2), completeness (0.1).

---

### 5.3 Text-to-Speech Agent (A-CNT-017)

**1. AGENT NAME**: `tts-agent`

**2. MISSION**: Generate natural, emotionally appropriate, and pedagogically effective audio narration from text content across all supported languages, with special attention to Quranic recitation and Arabic pronunciation.

**3. VISION**: To create the most authentic and engaging audio learning experience, where every learner can listen to Islamic content in a voice that is clear, natural, and respectful.

**4. RESPONSIBILITIES**:
- Convert text to natural speech in supported languages.
- Support multiple voice profiles (male, female, different ages).
- Handle Arabic diacritics, Tajweed rules, and Quranic recitation styles.
- Generate SSML with proper pauses, emphasis, and prosody for educational content.
- Adjust speaking rate and tone based on content type (story vs. lecture).
- Support voice cloning for known educators (with permission).

**5. NON RESPONSIBILITIES**:
- Does NOT generate text content.
- Does NOT translate.
- Does NOT provide Quranic memorization feedback.

**6-33**: Full template with models including ElevenLabs (primary), Amazon Polly Neural (fallback), custom Tajweed TTS model.

---

### 5.4 Image Generation Agent (A-CNT-018)

**1. AGENT NAME**: `image-generation-agent`

**2. MISSION**: Generate appropriate, accurate, and respectful visual illustrations for educational content, adhering to Islamic guidelines on representation and ensuring visual accuracy of historical/cultural depictions.

**3. VISION**: To visually enrich Islamic education with illustrations that are both beautiful and accurate, enhancing comprehension without compromising Islamic principles.

**4. RESPONSIBILITIES**:
- Generate illustrations for educational content (maps, diagrams, infographics).
- Create visual summaries of concepts.
- Generate character portraits respecting Islamic guidelines.
- Create historical scene illustrations based on factual descriptions.
- Generate diagrams for processes (Wudu, Salah positions, inheritance calculations).
- Ensure visual accuracy (clothing, architecture, geography, time period).
- Apply Islamic artistic traditions where appropriate.

**5. NON RESPONSIBILITIES**:
- Does NOT depict prophets or companions (respecting scholarly differences).
- Does NOT generate inappropriate or disrespectful imagery.
- Does NOT verify factual accuracy of visual content.
- Does NOT generate animated content (handled by Animation Agent).

**6-33**: Full template with models including DALL-E 3 (primary), Stable Diffusion (fallback), and custom Islamic art style LoRA models.

---

## 6. LEARNING AGENTS

### 6.1 AI Teacher Agent (A-LRN-001)

---

**1. AGENT NAME**: `ai-teacher-agent`

**2. MISSION**: Provide personalized, accurate, and engaging conversational Islamic education to learners, answering questions, explaining concepts, presenting multiple scholarly perspectives, and guiding learners through their educational journey with patience and wisdom.

**3. VISION**: To be the world's most trusted AI teacher for Islamic education, available 24/7 to any learner, in any language, on any topic, always accurate, always respectful, always patient.

**4. RESPONSIBILITIES**:
- Accept natural language questions from learners on any Islamic topic.
- Retrieve accurate answers from the verified Knowledge Graph via RAG.
- Present multiple scholarly positions where ikhtilaf exists.
- Provide citations and source references for all information.
- Adapt language complexity and depth to learner level.
- Maintain conversation context across multi-turn interactions.
- Detect when a question is outside the agent's verified knowledge.
- Provide follow-up questions to deepen understanding.
- Flag inappropriate questions or attempts to solicit unverified information.
- Escalate to human scholars when questions exceed scope.

**5. NON RESPONSIBILITIES**:
- Does NOT issue fatwas or religious rulings.
- Does NOT invent knowledge not in the Knowledge Graph.
- Does NOT engage in theological debate beyond presenting established positions.
- Does NOT provide psychological or medical advice.
- Does NOT collect personal information beyond what is needed for education.

**6. INPUTS**: User question (text), conversation history, user profile (level, language, interests), session context.

**7. OUTPUTS**: Answer text, citations, confidence score, suggested follow-ups, learning path recommendations.

**8. INTERNAL MEMORY**: Current conversation (100 turns max), user session context.

**9. EXTERNAL MEMORY**: Long-term conversation history (90 days), user learning profile, preferred topics and scholars.

**10. DATABASES USED**: Knowledge Graph (read-only via RAG), Vector Store (conversation embeddings), Redis (session state), PostgreSQL (user profile).

**11. KNOWLEDGE SOURCES**: Knowledge Graph (primary), RAG pipeline (verified facts), authenticated source documents, scholarly positions database.

**12. APIS**:
| Name | Protocol | Purpose |
|---|---|---|
| RAG Service | gRPC | Retrieve context for answers |
| Knowledge Graph | gRPC | Query specific facts |
| User Service | gRPC | Get/update user profile |
| Progress Service | gRPC | Track learning progress |
| Model Gateway | gRPC | LLM inference |

**13. MODELS**:
| Role | Model | Notes |
|---|---|---|
| Primary | GPT-4o-mini (streaming) | Fast, cost-effective for conversation |
| Complex reasoning | GPT-4o | For deep theological questions |
| Multi-turn | Claude 3.5 Sonnet | Better long-context handling |
| Embedding | text-embedding-3-small | RAG retrieval |

**14. PROMPTING STRATEGY**:
- Approach: ReAct (retrieve context, reason, respond).
- System prompt: "You are an AI Islamic teacher. You answer questions based ONLY on the verified facts provided in the context. You never invent or assume information. When scholarly differences exist, you present multiple positions with attribution. You cite your sources. You acknowledge uncertainty. You maintain respectful tone."
- Guardrails: Do not issue fatwas, do not speculate, do not provide medical/legal advice, do not engage with inappropriate content.
- Context window: Last 10 conversation turns + retrieved facts.

**15. PLANNING STRATEGY**:
- Approach: Direct (single step for simple questions), Chain (multi-step for complex questions).
- Steps: Understand query -> Classify intent -> Retrieve context -> Generate answer -> Validate -> Return.
- Replanning: If confidence < threshold, retrieve more context.

**16. REASONING STRATEGY**: Chain-of-Thought for complex theological questions, Direct for factual questions.

**17. VALIDATION STRATEGY**:
- Factual grounding: Every sentence must be traceable to retrieved context.
- Safety: Output passes through content safety filters.
- Confidence: If overall < 0.80, include uncertainty disclaimer.

**18. ERROR RECOVERY**:
| Error | Response |
|---|---|
| RAG returns no results | "I don't have verified information on this topic. Let me connect you with a human scholar." |
| LLM refuses (safety) | Provide alternative helpful response. |
| Low confidence | "I'm not entirely certain. Here's what I know: [answer]. I recommend consulting a scholar for a definitive answer." |
| User persists on unverified topic | "This is beyond the scope of what I can answer. I'll note your question for our scholarly team." |

**19. RETRY LOGIC**: Max 2 retries for RAG retrieval, max 1 retry for LLM generation.

**20. SELF EVALUATION**:
- Method: Reflexion (evaluate each response before sending).
- Dimensions: Factual grounding (every claim has source), completeness, clarity, safety, user satisfaction.

**21. CONFIDENCE SCORING**:
| Dimension | Weight | Method |
|---|---|---|
| Factual grounding | 0.5 | % of claims with source citations |
| RAG relevance | 0.3 | Retrieval score |
| Response coherence | 0.1 | Self-evaluation |
| User satisfaction | 0.1 | Post-response feedback (optional) |

**22. LOGGING**: [[question, response, confidence, sources_used, latency, user_feedback]].

**23. MONITORING**: [[questions_per_session, avg_confidence, user_satisfaction, safety_flag_rate, cost_per_conversation]].

**24. METRICS**:
| Category | Metrics |
|---|---|
| Quality | Answer accuracy, citation rate, user satisfaction score |
| Safety | Flag rate, inappropriate content detection rate |
| Performance | Response latency (P50 < 2s, P95 < 5s) |
| Business | Cost per conversation, conversations per day |

**25. PERFORMANCE**:
| Metric | Target |
|---|---|
| P50 response latency | 1.5s |
| P95 response latency | 4s |
| P99 response latency | 8s |
| Concurrent conversations per instance | 100 |
| Average conversation turns | 5-10 |

**26. SECURITY**: Input sanitization for prompt injection, output safety filters, rate limiting per user.

**27. PRIVACY**: Conversation history anonymized after 90 days. No PII stored in conversation logs.

**28. FAILURE CASES**:
| Failure | Mitigation |
|---|---|
| Prompt injection attempt | Input classification + sanitization |
| Off-topic persistence | Polite redirection, session timeout |
| User argumentative | Maintain respectful tone, offer to escalate |
| Technical timeout | Graceful error message, offer to retry |

**29. HUMAN REVIEW POINTS**:
| Condition | Action |
|---|---|
| User explicitly requests human scholar | Escalate to scholar queue |
| Question has avg confidence < 0.70 | Flag for review, offer escalation |
| Content safety filter triggered | Log, review by safety team |
| Repeated off-topic behavior | Flag for admin review |

**30. COMPLETE WORKFLOW**:
```
1. Receive question from user via WebSocket
2. Validate input (sanitize, check length, classify intent)
3. Retrieve conversation history from session memory
4. Retrieve contextual facts from RAG pipeline
5. Build prompt with system instructions + conversation history + facts
6. Generate response via LLM (streaming)
7. Validate response (factual grounding, safety, confidence)
8. If valid:
   a. Stream response to user
   b. Update conversation history
   c. Log interaction
   d. Update user progress (implicit)
9. If invalid:
   a. If low confidence: add disclaimer, proceed
   b. If safety violation: rewrite, recheck
   c. If RAG empty: inform user, offer escalation
10. Return final response
```

---

### 6.2 Adaptive Path Agent (A-LRN-002) through 6.7 Remediation Agent (A-LRN-007)

(Each follows the 33-section template. Key summaries:)

**Adaptive Path Agent (A-LRN-002)**: Dynamically adjusts learner's educational path based on performance, preferences, and goals.
- **Model**: Reinforcement learning (custom MDP) + GPT-4o-mini for path explanation.
- **Input**: User profile, learning history, assessment results, goal.
- **Output**: Next recommended learning activities, adjusted difficulty, suggested focus areas.
- **Algorithm**: Bayesian Knowledge Tracing + Thompson Sampling for exploration/exploitation.
- **Confidence**: Path optimality score based on user progress vs. predicted progress.

**Assessment Agent (A-LRN-003)**: Generate, administer, and grade assessments.
- **Models**: GPT-4o (question generation), fine-tuned BERT (short answer grading).
- **Method**: Adaptive testing using Item Response Theory (IRT).
- **Question Types**: MCQ, True/False, Matching, Fill-in-Blank, Short Answer, Essay.
- **Output**: Score per question, overall score, knowledge state estimate, misconceptions identified.
- **Validation**: Psychometric validation (item difficulty, discrimination, reliability).

**Recommendation Agent (A-LRN-004)**: Recommend content and learning paths.
- **Algorithm**: Hybrid (collaborative filtering + content-based + knowledge graph embeddings).
- **Input**: User profile, behavior history, knowledge state, peer group patterns.
- **Output**: Ranked list of recommended content with relevance scores.
- **Models**: LightFM (collaborative), GraphSAGE (KG embeddings), GPT-4o-mini (explanation generation).

**Progress Agent (A-LRN-005)**: Track and report learning progress.
- **Function**: Consume learning events, update progress models, generate progress reports.
- **Models**: Learning curve estimation, mastery threshold calculation.
- **Output**: Knowledge mastery by topic, time spent, strengths/weaknesses, pace analysis.

**Spaced Repetition Agent (A-LRN-006)**: Schedule optimal review intervals.
- **Algorithm**: SM-5 (SuperMemo) + FSRS (Free Spaced Repetition Scheduler) with custom parameters.
- **Input**: Item difficulty rating, user recall history, review timestamps.
- **Output**: Next review date for each item, items due for review.
- **Storage**: PostgreSQL for scheduling data, Redis for daily due items.

**Remediation Agent (A-LRN-007)**: Identify and address learning gaps.
- **Function**: Analyze assessment results to identify specific misconceptions or gaps.
- **Approach**: Prerequisite chain analysis (if concept B depends on concept A and B is low, check A).
- **Output**: Gap analysis report, recommended remediation activities, prerequisite review suggestions.

---

## 7. QUALITY AGENTS

### 7.1 Accuracy Agent (A-QAL-001)

---

**1. AGENT NAME**: `accuracy-agent`

**2. MISSION**: Verify that every generated content piece accurately reflects the source facts from which it was derived, detecting any hallucination, distortion, omission, or addition of information.

**3. VISION**: To be the final quality gate that ensures no learner ever encounters a factual error, no matter which learning mode they use or how the content was generated.

**4. RESPONSIBILITIES**:
- Extract claims from generated content.
- Match each claim to source facts from the Knowledge Graph.
- Detect claims not supported by any source fact (hallucination).
- Detect claims that contradict source facts (distortion).
- Detect claims that omit critical context.
- Calculate overall accuracy score.
- Flag inaccurate content for revision.

**5-33**: Full template with:
- **Models**: Fine-tuned NLI model for claim-fact matching + GPT-4o for complex evaluation.
- **Method**: Decompose content into atomic claims, match each claim to source facts using semantic similarity + exact matching.
- **Accuracy Levels**: EXACT_MATCH, SUPPORTED (implied by facts), NOT_SUPPORTED (no source), CONTRADICTED (conflicts with source), CONTEXT_MISSING (technically true but misleading).
- **Threshold**: Below 99% accuracy triggers content revision.

---

### 7.2 Relevance Agent (A-QAL-002) through 7.7 Pedagogy Agent (A-QAL-007)

(Following the 33-section template. Key summaries:)

**Relevance Agent (A-QAL-002)**: Check content relevance to stated learning objectives.
- **Method**: Embedding similarity between content and learning objectives + topical coverage analysis.
- **Threshold**: Below 0.70 relevance triggers revision.

**Completeness Agent (A-QAL-003)**: Check that generated content covers all required knowledge.
- **Method**: Compare content topics against required topic list for the knowledge domain.
- **Output**: Coverage percentage, missing topics, under-covered topics.

**Style Agent (A-QAL-004)**: Check writing style, tone, and readability.
- **Method**: Readability formulas (Flesch, Arabic OSMAN), style guide compliance, tone appropriateness.
- **Dimensions**: Readability, consistency, engagement, tone (respectful, educational, appropriate for age).

**Bias Agent (A-QAL-005)**: Detect unintended bias in content.
- **Method**: Check for unbalanced presentation of scholarly views, gender bias in examples, cultural bias, regional bias.
- **Output**: Bias score, specific examples of potential bias, correction suggestions.

**Accessibility Agent (A-QAL-006)**: Check WCAG compliance.
- **Method**: Automated checks for alt text, contrast, heading structure, descriptive links, ARIA labels.
- **Standard**: WCAG 2.1 AA minimum.

**Pedagogy Agent (A-QAL-007)**: Check pedagogical soundness.
- **Method**: Educational quality framework assessing alignment with learning objectives, appropriate cognitive level (Bloom's), active learning elements, assessment alignment, prerequisite sequencing.
- **Output**: Pedagogical quality score, improvement suggestions.

---

## 8. ADMINISTRATIVE AGENTS

(Each follows the 33-section template. Brief summaries:)

**Monitoring Agent (A-ADM-001)**: System health monitoring and alerting.
- **Function**: Monitor all agent health, queue depths, error rates, resource utilization.
- **Integration**: Prometheus metrics, Grafana dashboards, PagerDuty alerts.

**Audit Agent (A-ADM-002)**: Immutable audit logging for compliance.
- **Function**: Record all configuration changes, content modifications, user data access, system events.
- **Storage**: Append-only log, stored for 7 years.
- **Access**: Read-only, authorized audit personnel only.

**Config Agent (A-ADM-003)**: Centralized configuration management.
- **Function**: Manage and distribute configuration to all agents and services.
- **Source**: Git-based with CI/CD pipeline for validation and deployment.

**Maintenance Agent (A-ADM-004)**: Scheduled system maintenance.
- **Function**: Execute cleanup, reindexing, backup verification, health checks, certificate renewal.

---

## 9. AGENT INTERACTION MATRIX

```
                    OCR Layout Entity Relation Fact Struct Verse Hadith XRef LMCk Contr Source SchR ContV Cite Trans TTS Image AITch Adapt Assess Reco Prog SpRep Remed Accur Relev Compl Style Bias Access Ped
    OCR              -   Seq    Seq    -      -    Seq   Seq   Seq    -    -     -     -     -    -    -    -    -    -    -    -      -    -    -    -     -    -     -     -    -    -     -    -
    Layout           -    -     Seq    -      -    Seq   -     -     -    -     -     -     -    -    -    -    -    -    -    -      -    -    -    -     -    -     -     -    -    -     -    -
    Entity           -    -      -    Seq    Seq    -    -     -     -    -     -     -     -    -    -    -    -    -    -    -      -    -    -    -     -    -     -     -    -    -     -    -
    Relation         -    -     Seq    -     Seq    -    -     -     -    -     -     -     -    -    -    -    -    -    -    -      -    -    -    -     -    -     -     -    -    -     -    -
    Fact             -    -     Seq   Seq     -     -    -     -    Seq   Seq   Seq   Seq   Seq  Seq   -    -    -    -    -    -      -    -    -    -     -    -     -     -    -    -     -    -
    Structure        -    -     -      -      -     -    -     -     -    -     -     -     -    -    -    -    -    -    -    -      -    -    -    -     -    -     -     -    -    -     -    -
    Verse            -    -     -      -     Seq    -    -     -    Seq   -     -     -     -    -    -    -    -    -    -    -      -    -    -    -     -    -     -     -    -    -     -    -
    Hadith           -    -     -      -     Seq    -    -     -    Seq   -     -     -     -    -    -    -    -    -    -    -      -    -    -    -     -    -     -     -    -    -     -    -
    Cross-Ref        -    -     -      -     Seq    -    -     -     -    -     -    Seq    -    -    -    -    -    -    -    -      -    -    -    -     -    -     -     -    -    -     -    -
    LLM Check        -    -     -      -     Seq    -    -     -     -    -     Seq   Seq    -    -    -    -    -    -    -    -      -    -    -    -     -    -     -     -    -    -     -    -
    Contradiction    -    -     -      -     Seq    -    -     -    Seq   Seq    -    Seq    -    -    -    -    -    -    -    -      -    -    -    -     -    -     -     -    -    -     -    -
    Scholarly Review -    -     -      -     Seq    -    -     -    Seq   Seq   Seq    -    Seq   -    -    -    -    -    -    -      -    -    -    -     -    -     -     -    -    -     -    -
    Cont. Verify     -    -     -      -     Seq    -    -     -    Seq   Seq   Seq    -     -    -    -    -    -    -    -    -      -    -    -    -     -    -     -     -    -    -     -    -
    Citation         -    -     -      -      -     -    -     -     -    -     -     -     -    -    -    -    -    -    -    -      -    -    -    -    Seq   -     Seq   Seq   -    -     -    -
    Translation      -    -     -      -      -     -    -     -     -    -     -     -     -    -    -    -    -    -    -    -      -    -    -    -     -    -     -     -    -    -     -    -
    TTS              -    -     -      -      -     -    -     -     -    -     -     -     -    -   Seq    -    -    -    -    -      -    -    -    -     -    -     -     -    -    -     -    -
    Image            -    -     -      -      -     -    -     -     -    -     -     -     -    -    -     -    -    -    -    -      -    -    -    -     -    -     -     -    -    -     -    -
    AI Teacher       -    -     -      -      -     -    -     -    Seq   Seq    -     -     -   RAG   -    -    -    -    -    -      -    -   RAG   -     -    -     -     -    -    -     -    -
    Adaptive Path    -    -     -      -      -     -    -     -     -    -     -     -     -    -    -    -    -   Seq    -    -      -    -    -   Seq   -    -     -     -    -    -     -    -
    Assessment       -    -     -      -      -     -    -     -     -    -     -     -     -    -    -    -    -    -     -    -      -    -    -    -     -    -     -     -    -    -     -    -
    Recommendation   -    -     -      -      -     -    -     -     -    -     -     -     -    -    -    -    -   Seq   Seq   -      -    -    -    -     -    -     -     -    -    -     -    -
    Progress         -    -     -      -      -     -    -     -     -    -     -     -     -    -    -    -    -   Seq    -    Seq    -    -    -    -     -    -     -     -    -    -     -    -
    Spaced Rep       -    -     -      -      -     -    -     -     -    -     -     -     -    -    -    -    -    -     -     -    Seq   -    -    -     -    -     -     -    -    -     -    -
    Remediation      -    -     -      -      -     -    -     -     -    -     -     -     -    -    -    -    -   Seq   Seq   Seq    -    -    -    -     -    -     -     -    -    -     -    -
    Accuracy         -    -     -      -      -     -    -     -     -    -     -     -     -    -    -    -    -    -     -     -    RAG   -    -    -     -    -     -     -    -    -     -    -
    Relevance        -    -     -      -      -     -    -     -     -    -     -     -     -    -    -    -    -    -     -     -     -    -    -    -     -    -     -     -    -    -     -    -
    Completeness     -    -     -      -      -     -    -     -     -    -     -     -     -    -    -    -    -    -     -     -     -    -    -    -     -    -     -     -    -    -     -    -
    Style            -    -     -      -      -     -    -     -     -    -     -     -     -    -    -    -    -    -     -     -     -    -    -    -     -    -     -     -    -    -     -    -
    Bias             -    -     -      -      -     -    -     -     -    -     -     -     -    -    -    -    -    -     -     -     -    -    -    -     -    -     -     -    -    -     -    -
    Accessibility    -    -     -      -      -     -    -     -     -    -     -     -     -    -    -    -    -    -     -     -     -    -    -    -     -    -     -     -    -    -     -    -
    Pedagogy         -    -     -      -      -     -    -     -     -    -     -     -     -    -    -    -    -    -     -     -     -    -    -    -     -    -     -     -    -    -     -    -

Key: Seq = Sequential (pipeline), RAG = Retrieval query, - = No direct interaction
```

---

## 10. AGENT DATA FLOW DIAGRAMS

### 10.1 End-to-End Book Processing Flow

```
    Upload -> [OCR Agent] -> [Layout Agent] -> [Entity Agent] -> [Relation Agent] -> [Fact Agent]
                                                                                            |
                                                                                            v
    [Verse Agent] -> [Hadith Agent]                                                     [Knowledge Graph]
         |                |                                                                    |
         v                v                                                                    |
    [Cross-Ref Agent] <- [Source Verif] ------------------------------------------------------+
         |
         v
    [LLM Check Agent] -> [Contradiction Agent] -> [Scholarly Review Agent] -> [Confidence Agent]
                                                                                            |
                                                                                            v
                                                                                    [Knowledge Graph]
                                                                                     (verified facts)
                                                                                            |
                                                                                +-----------+-----------+
                                                                                |                       |
                                                                        [Content Gen Orch]    [Continuous Verification]
                                                                                |                       |
                                                                        +-------+-------+               |
                                                                        |       |       |               |
                                                                        v       v       v               v
                                                                  [Story] [Quiz] [Podcast] ...    [Re-verify]
                                                                        |       |       |
                                                                        +-------+-------+               |
                                                                                |                       |
                                                                        [Quality Agents]               |
                                                                        (Accuracy, Relevance, ...)     |
                                                                                |                       |
                                                                        [Publishing]                   |
                                                                                |                       |
                                                                        [User Delivery] <--------[Flag if changed]
```

### 10.2 User Learning Flow

```
    User Login -> [Progress Agent] -> [Adaptive Path Agent] -> [Recommendation Agent]
                                                                        |
                                                                        v
    User Selects Content -> [Content Delivery] -> [AI Teacher Agent] <-> [RAG Pipeline]
                                                          |
                                                          v
    User Interacts -> [Assessment Agent] -> [Progress Agent] -> [Adaptive Path Agent] (update path)
                          |
                          v
    [Spaced Rep Agent] -> Schedules review -> [Remediation Agent] (if gaps detected)
```

---

## 11. AGENT PERFORMANCE BUDGETS

| Agent | P50 Latency | P95 Latency | P99 Latency | Throughput/Instance | Max Concurrent | Cost/Task |
|---|---|---|---|---|---|---|
| OCR | 30s/page | 120s/page | 300s/page | 10 pages/min | 10 | $0.05/page |
| Layout | 5s/page | 20s/page | 60s/page | 50 pages/min | 20 | $0.002/page |
| Entity | 500ms | 2s | 5s | 1000 pages/min | 20 | $0.001/page |
| Relation | 1s | 4s | 10s | 500 pages/min | 20 | $0.002/page |
| Fact | 2s | 8s | 20s | 200 pages/min | 20 | $0.005/page |
| Verse | 500ms | 2s | 5s | 2000 pages/min | 30 | $0.001/page |
| Hadith | 1s | 5s | 15s | 500 pages/min | 20 | $0.003/page |
| Cross-Ref | 3s | 15s | 45s | 100 facts/min | 30 | $0.01/fact |
| LLM Check | 5s | 20s | 60s | 50 facts/min | 20 | $0.03/fact |
| Contradiction | 30s (full scan) | 5min | 15min | 1M facts/scan | 5 | $0.50/scan |
| Scholarly Review | 12h | 48h | 7d | N/A (human) | N/A | N/A |
| Cont. Verification | 1h | 4h | 12h | 100K facts/cycle | 3 | $1.00/cycle |
| Citation | 2s | 8s | 20s | 500 checks/min | 20 | $0.002/check |
| Translation | 10s/page | 30s/page | 60s/page | 50 pages/min | 15 | $0.01/page |
| TTS | 2s/min audio | 5s/min audio | 10s/min audio | 30 min audio/min | 20 | $0.02/min |
| Image | 10s/image | 30s/image | 60s/image | 6 images/min | 10 | $0.04/image |
| AI Teacher | 1.5s | 4s | 8s | 100 conversations | 100+ | $0.005/msg |
| Adaptive Path | 100ms | 500ms | 2s | 1000 req/s | 50 | $0.0001/req |
| Assessment | 5s (gen) / 500ms (grade) | 15s / 2s | 30s / 5s | 50/min gen, 1000/min grade | 20 | $0.03/gen |
| Recommendation | 200ms | 1s | 3s | 500 req/s | 30 | $0.0005/req |
| Progress | 50ms | 200ms | 500ms | 2000 req/s | 20 | $0.00005/req |
| Spaced Rep | 10ms | 50ms | 200ms | 5000 req/s | 10 | $0.00001/req |
| Remediation | 500ms | 2s | 5s | 500 req/s | 10 | $0.001/req |
| Accuracy | 5s | 20s | 60s | 50 checks/min | 20 | $0.01/check |
| Relevance | 3s | 10s | 30s | 80 checks/min | 15 | $0.005/check |
| Completeness | 5s | 20s | 60s | 50 checks/min | 15 | $0.01/check |
| Style | 2s | 8s | 20s | 100 checks/min | 15 | $0.003/check |
| Bias | 5s | 15s | 45s | 60 checks/min | 10 | $0.01/check |
| Accessibility | 3s | 10s | 30s | 80 checks/min | 10 | $0.002/check |
| Pedagogy | 5s | 20s | 60s | 50 checks/min | 10 | $0.01/check |

---

## 12. AGENT DEPENDENCY MAP

```
    Agent                    Depends On                        Depended On By
    =====================    ==============================    ==============================
    OCR Agent                Book Ingestion Service            Layout, Entity, Verse, Hadith
    Layout Agent             OCR Agent                         Entity, Fact, Structure
    Entity Agent             OCR Agent, Layout Agent           Relation, Fact
    Relation Agent           Entity Agent                      Fact
    Fact Agent               Entity Agent, Relation Agent      Cross-Ref, LLM Check, Verse (enhanced)
    Structure Agent          Layout Agent                      Content Gen (TOC)
    Verse Agent              OCR Agent, Fact Agent (enhanced)  Verification (priority)
    Hadith Agent             OCR Agent, Fact Agent (enhanced)  Verification (priority)
    Cross-Ref Agent          Fact Agent, Knowledge Graph       Verification Orchestrator
    LLM Check Agent          Fact Agent, Knowledge Graph       Verification Orchestrator
    Contradiction Agent      Fact Agent, Knowledge Graph       Verification Orchestrator
    Source Agent             Book Ingestion Service            Verification Orchestrator
    Scholarly Review Agent   Cross-Ref, LLM Check, Contradict  Knowledge Graph (approved facts)
    Cont. Verification       Knowledge Graph                   Knowledge Graph (updates)
    Citation Agent           Content Gen Agents                Content Publishing
    Translation Agent        Content Gen Agents                Content Assembly
    TTS Agent                Podcast Script                    Media Assembly
    Image Agent              Visual Content Script             Media Assembly
    AI Teacher Agent         Knowledge Graph (RAG)             User Delivery
    All Content Gen Agents   Knowledge Graph                   Content Quality Agents
    All Quality Agents       Content Gen Agents                Content Publishing
```

---

## 13. APPENDICES

### Appendix A: Agent Status Summary

| Status | Count | Agents |
|---|---|---|
| P0 - Critical Path | 18 | OCR, Layout, Entity, Relation, Fact, Verse, Hadith, Cross-Ref, LLM Check, Contradiction, Source, Scholarly Review, Content Gen Orch, AI Teacher, Adaptive Path, Assessment, Progress, Accuracy |
| P1 - Phase 2 | 9 | Structure, Continuous Verification, Citation, Translation, TTS, Image, Recommendation, Spaced Rep, Relevance, Completeness, Style, Bias, Pedagogy |
| P2 - Phase 3 | 6 | Dialogue, Debate, Game, Simulation, Video, Remediation, Accessibility |
| P3 - Future | 2 | Advanced Simulation, VR Agents |

### Appendix B: Agent Communication Requirements

| Requirement | Specification |
|---|---|
| Messages/second peak | 50,000 |
| Max message size | 10 MB (OCR results may be larger) |
| Latency (internal) | < 10ms p99 between agents |
| Durability | At-least-once delivery |
| Ordering | Per-entity ordering via Kafka key |
| Dead letter queue | All agent types |

### Appendix C: Agent Resource Requirements (Total)

| Resource | Total Requirement | Notes |
|---|---|---|
| CPU (production) | 500+ cores | Distributed across agent types |
| Memory (production) | 2+ TB RAM | VRAM for GPU agents |
| GPU (production) | 50+ GPUs | LLM inference, OCR, TTS, Image gen |
| Storage (production) | 10+ TB | Model storage, intermediate results |
| Kafka Throughput | 100+ MB/s | Event streaming between agents |

### Appendix D: Related Documents

| Document ID | Title | Relationship |
|---|---|---|
| IEP-CONST-001 | Project Constitution | Governing principles |
| IEP-ARCH-AGENT-001 | Agent Architecture | Agent framework and patterns |
| IEP-ARCH-ORCH-001 | Master Orchestrator | Workflow coordination |
| IEP-ARCH-VRF-001 | Verification Architecture | Verification agent details |
| IEP-ARCH-KG-001 | Knowledge Graph Architecture | Knowledge access for agents |
| IEP-ARCH-RAG-001 | RAG Architecture | RAG integration for agents |
| IEP-12 to IEP-18 | Mode-Specific Agents | Story, Quiz, Flashcard, etc. |

---

**END OF AGENTS SPECIFICATION**

---

*This document provides the complete specifications for all 51 AI agents in the Islamic Education Platform. Each agent follows the standardized 33-section template. Mode-specific content generation agents (Story, Quiz, Flashcard, Podcast, etc.) have expanded specifications in their respective dedicated documents. Questions regarding this document should be directed to the Office of the Chief Architect.*
