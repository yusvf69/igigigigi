# VERIFICATION ARCHITECTURE

## Multi-Layer Fact Verification System

---

| Document ID | IEP-ARCH-VRF-001 |
|---|---|
| Version | 1.0.0 |
| Status | Approved |
| Classification | Enterprise Internal |
| Effective Date | 2026-07-07 |
| Approving Authority | Office of the Chief Architect |
| Custodian | Verification Architecture Team |
| Constitution Reference | IEP-CONST-001 (Section 6.5) |
| Knowledge Graph Reference | IEP-ARCH-KG-001 |

---

## TABLE OF CONTENTS

1. EXECUTIVE OVERVIEW
2. VERIFICATION PHILOSOPHY
3. FIVE-LAYER VERIFICATION ARCHITECTURE
4. LAYER 1: SOURCE VERIFICATION
5. LAYER 2: CROSS-REFERENCE VERIFICATION
6. LAYER 3: LLM CONSISTENCY CHECK
7. LAYER 4: CONTRADICTION DETECTION
8. LAYER 5: SCHOLARLY REVIEW
9. VERIFICATION WORKFLOW
10. CONFIDENCE AGGREGATION
11. CONTINUOUS VERIFICATION
12. HUMAN REVIEW MANAGEMENT
13. QUALITY METRICS
14. PERFORMANCE
15. SECURITY
16. APPENDICES

---

## 1. EXECUTIVE OVERVIEW

### 1.1 Purpose

The Verification Architecture is the platform's quality assurance backbone. It ensures that every fact in the Knowledge Graph has been rigorously checked through multiple independent verification methods before it can be used for content generation. This multi-layer approach minimizes the risk of factual errors, hallucination propagation, and misinformation reaching learners.

### 1.2 Verification Philosophy

| Principle | Description |
|---|---|
| **No Single Point of Truth** | No fact is trusted based on a single source or method. |
| **Independent Layers** | Each verification layer uses a different methodology, so a failure in one does not compromise the others. |
| **Confidence-Gated** | Facts only become available for content generation after passing defined confidence thresholds. |
| **Always Verifiable** | Every fact retains its complete verification history. |
| **Continuous Process** | Verification is not a one-time event but continues throughout the fact's lifetime. |
| **Human Oversight** | Critical and sensitive knowledge requires human scholarly review, not just automated checks. |

### 1.3 Verification Layers Summary

| Layer | Method | Automation | Time | Confidence Contribution |
|---|---|---|---|---|
| L1: Source | Document authenticity, OCR quality, structure validation | Fully automated | Minutes | 10% |
| L2: Cross-Reference | Compare against multiple trusted sources | Fully automated | Hours | 35% |
| L3: LLM Consistency | AI-based plausibility and consistency check | Fully automated | Minutes | 20% |
| L4: Contradiction | Detect conflicts across the Knowledge Graph | Fully automated | Hours | 15% |
| L5: Scholarly | Human expert review and approval | Human-in-the-loop | Days | 20% |

---

## 2. VERIFICATION PHILOSOPHY

### 2.1 Verification Trust Model

```
    Raw Extraction (0% trusted)
         |
    L1: Source Verification (10% trusted)
         |
    L2: Cross-Reference (45% trusted)
         |
    L3: LLM Consistency (65% trusted)
         |
    L4: Contradiction Check (80% trusted)
         |
    L5: Scholarly Review (100% trusted for sensitive)
         |
    VERIFIED FACT
```

### 2.2 Verification Timing

| Scenario | Verification Trigger | Max Time to Verify |
|---|---|---|
| New book ingestion | Immediate (automated pipeline) | 48 hours (full), 4 hours (automated) |
| Fact update | Immediate | 24 hours |
| New authoritative source added | Event-driven re-verification | 7 days |
| Scheduled re-verification | Cron (based on domain importance) | - |
| User report | Immediate priority queue | 24 hours for review |
| Content generation quality failure | Immediate flag | 48 hours |

---

## 3. FIVE-LAYER VERIFICATION ARCHITECTURE

### 3.1 Architecture Overview

```
    +===================================================================+
    |                   VERIFICATION ARCHITECTURE                         |
    |                                                                     |
    |  Fact from Extraction Pipeline                                      |
    |       |                                                             |
    |  +----v---------------------------------------------------------+  |
    |  |           VERIFICATION ORCHESTRATOR                           |  |
    |  |  - Receives facts from extraction pipeline                    |  |
    |  |  - Coordinates verification layers                            |  |
    |  |  - Manages verification state and progress                    |  |
    |  |  - Aggregates confidence scores                               |  |
    |  |  - Routes to human review when needed                         |  |
    |  +----+-----------+-----------+----------+----------+------------+  |
    |       |           |           |          |          |               |
    |  +----v----+ +----v----+ +---v----+ +---v----+ +--v-----------+   |
    |  | L1:     | | L2:     | | L3:    | | L4:    | | L5:         |   |
    |  | Source  | | Cross-  | | LLM    | | Contra-| | Scholarly   |   |
    |  | Verify  | | Ref     | | Check  | | diction| | Review      |   |
    |  +----+----+ +----+----+ +---+----+ +---+----+ +------+------+   |
    |       |           |           |        |               |          |
    |  +----v-----------v-----------v--------v---------------v------+   |
    |  |              CONFIDENCE AGGREGATOR                         |   |
    |  |  Combines results from all layers into overall score       |   |
    |  +---------------------------+--------------------------------+   |
    |                              |                                    |
    |  +---------------------------v--------------------------------+   |
    |  |              KNOWLEDGE GRAPH (Verified Facts)               |   |
    |  +------------------------------------------------------------+   |
    +===================================================================+
```

### 3.2 Verification Orchestrator

The Verification Orchestrator manages the entire verification workflow:

```yaml
verification_orchestrator:
  responsibilities:
    - Receive facts from extraction pipeline
    - Initialize verification state for each fact
    - Dispatch to verification layers in sequence
    - Track verification progress per fact
    - Handle layer failures and retries
    - Aggregate confidence scores
    - Decide: verified, flagged, or rejected
    - Update Knowledge Graph with verification status
    - Trigger human review when needed
    - Log all verification events
  
  workflow:
    - receive_fact
    - run_source_verification
    - run_cross_reference
    - run_llm_check
    - run_contradiction_detection
    - if sensitive_domain: run_scholarly_review
    - aggregate_confidence
    - update_knowledge_graph
    - log_verification_complete
```

---

## 4. LAYER 1: SOURCE VERIFICATION

### 4.1 Purpose

Verify that the source document is authentic, the OCR extraction is accurate, and the document structure is correctly parsed.

### 4.2 Verification Checks

| Check | Method | Failure Action |
|---|---|---|
| Document hash integrity | Compare SHA-256 hash against publisher-provided hash | Flag document as tampered |
| OCR quality score | Average character confidence across all pages | <95%: manual transcription review |
| Language consistency | Language model perplexity check | Mismatch: flag for language review |
| Structure completeness | Expected chapters present | Missing structure: manual review |
| Page order integrity | Page number sequence validation | Out of order: reorder or flag |
| Metadata consistency | Author, title, publisher match known catalogs | Mismatch: investigate |

### 4.3 Source Verification Output

```json
{
  "source_id": "book-001",
  "verification_results": {
    "hash_integrity": {"status": "passed", "hash": "sha256-..."},
    "ocr_quality": {"status": "passed", "score": 0.97, "pages_below_threshold": 3},
    "language_consistency": {"status": "passed", "detected_language": "ar", "confidence": 0.99},
    "structure_completeness": {"status": "passed", "expected_chapters": 15, "found_chapters": 15},
    "metadata_verification": {"status": "passed", "matches_catalog": true},
    "overall_status": "verified",
    "overall_confidence": 0.97
  }
}
```

---

## 5. LAYER 2: CROSS-REFERENCE VERIFICATION

### 5.1 Purpose

Verify each extracted fact by comparing it against multiple trusted sources in the Knowledge Graph. A fact is stronger if it appears in multiple independent sources.

### 5.2 Cross-Reference Process

```
    Fact: "Salah is obligatory upon every adult Muslim"
         |
    [Query Generation] -> Generate multiple search queries
         |
    [Multi-Source Search]
    - Search Knowledge Graph for similar/related facts
    - Search source document corpus
    - Search trusted reference databases
         |
    [Source Authority Scoring]
    - Quran: weight 1.0 (highest authority)
    - Mutawatir Hadith: weight 0.95
    - Authentic Hadith (Sahih): weight 0.90
    - Scholarly Consensus (Ijma): weight 0.95
    - Well-known Hadith (Hasan): weight 0.80
    - Major Scholar Work: weight 0.70
    - Minor Scholar Work: weight 0.50
    - Contemporary Work: weight 0.40
         |
    [Match Analysis]
    - Exact match: same wording in multiple sources
    - Semantic match: same meaning, different wording
    - Partial match: overlapping but not identical
    - No match: no related source found
         |
    [Corroboration Score Calculation]
    - Number of supporting sources
    - Authority weights of supporting sources
    - Agreement level
```

### 5.3 Cross-Reference Scoring

```python
def calculate_corroboration(matches: list[Match]) -> float:
    """
    Calculate corroboration score from multiple source matches.
    """
    if not matches:
        return 0.0
    
    total_weight = 0
    weighted_score = 0
    
    for match in matches:
        authority_weight = SOURCE_AUTHORITY_WEIGHTS.get(match.source_type, 0.3)
        match_weight = {
            "exact": 1.0,
            "semantic": 0.8,
            "partial": 0.5,
            "contradictory": -1.0
        }.get(match.match_type, 0.0)
        
        score = authority_weight * match_weight
        total_weight += authority_weight
        weighted_score += score
    
    # Normalize and apply diminishing returns for many sources
    base_score = weighted_score / total_weight if total_weight > 0 else 0
    
    # Bonus for multiple independent sources
    diversity_bonus = min(len(set(m.source_type for m in matches)) * 0.05, 0.2)
    
    final_score = base_score + diversity_bonus
    return max(0.0, min(1.0, final_score))
```

### 5.4 Cross-Reference Output

```json
{
  "fact_id": "fact-salah-obligation-001",
  "cross_reference_result": {
    "status": "confirmed",
    "corroboration_score": 0.96,
    "supporting_sources": 8,
    "source_breakdown": {
      "quran": {"matches": 3, "highest_authority": "Quran 2:43"},
      "hadith": {"matches": 4, "highest_authority": "Sahih Bukhari 1"},
      "ijma": {"matches": 1, "description": "Scholarly consensus"},
      "scholar_works": {"matches": 5, "highest_authority": "Al-Mughni, Ibn Qudamah"}
    },
    "match_types": {
      "exact": 5,
      "semantic": 6,
      "partial": 2
    },
    "contradictory_sources": 0,
    "confidence": 0.96
  }
}
```

---

## 6. LAYER 3: LLM CONSISTENCY CHECK

### 6.1 Purpose

Use an LLM to evaluate the fact for internal consistency, logical coherence, and consistency with established Islamic scholarly methodology.

### 6.2 LLM Check Process

```
    Fact + Supporting Sources
         |
    [Prompt Construction]
    - System prompt: Expert Islamic scholar role
    - Context: Related facts from KG
    - Task: Evaluate fact consistency
         |
    [Evaluation Dimensions]
    1. Internal consistency: Does the fact contradict itself?
    2. Scholarly consistency: Does it align with established positions?
    3. Methodological soundness: Is the reasoning valid?
    4. Contextual accuracy: Is context properly preserved?
    5. Completeness: Are important qualifiers included?
         |
    [LLM Generation] -> Structured output with scores
         |
    [Multi-Pass Consistency]
    - Run 3 times with different temperatures
    - Check agreement across runs
    - Flag if significant disagreement
         |
    [Result]
    - Overall consistency score
    - Dimension scores
    - Identified issues
    - Suggested improvements
```

### 6.3 LLM Check Prompt

```
You are an expert Islamic scholar and fact-checker. Your role is to evaluate the 
consistency and accuracy of extracted Islamic knowledge facts.

Fact to evaluate: {fact.statement}

Context (related facts from verified sources):
{context}

Source: {fact.source}
Confidence of extraction: {fact.extraction_confidence}

Evaluate this fact on the following dimensions (score 0-1):

1. INTERNAL CONSISTENCY: Does the fact contain any internal contradictions?
2. SCHOLARLY CONSISTENCY: Is this consistent with established Islamic scholarship?
3. METHODOLOGICAL SOUNDNESS: Does the reasoning follow valid scholarly methodology?
4. CONTEXTUAL ACCURACY: Is the context properly preserved without distortion?
5. COMPLETENESS: Are all necessary qualifiers and conditions included?

For each dimension:
- Score (0.0 to 1.0)
- Explanation
- Specific issues (if any)

Also identify:
- Any potential misinterpretations
- Any missing qualifiers or conditions that could change the meaning
- Any alternative scholarly positions that should be noted

Output as JSON.
```

### 6.4 LLM Check Output

```json
{
  "fact_id": "fact-salah-obligation-001",
  "llm_check_result": {
    "overall_score": 0.94,
    "dimensions": {
      "internal_consistency": {"score": 1.0, "notes": "No internal contradictions"},
      "scholarly_consistency": {"score": 0.95, "notes": "Consistent with all four schools"},
      "methodological_soundness": {"score": 0.90, "notes": "Well-supported by Quran and Sunnah"},
      "contextual_accuracy": {"score": 0.92, "notes": "Context is accurate, but mention of menstruation exception adds important context"},
      "completeness": {"score": 0.88, "notes": "Could mention the exception for those unable to pray due to valid excuse"}
    },
    "issues": [
      {
        "type": "completeness",
        "severity": "minor",
        "description": "Fact does not mention the exception for menstruating women",
        "suggestion": "Add qualifier: 'Salah is obligatory upon every adult, sane Muslim who is not menstruating or in post-partum bleeding'"
      }
    ],
    "multi_pass_agreement": 0.93,
    "passes": [0.94, 0.95, 0.91],
    "model": "claude-3.5-opus",
    "confidence": 0.94
  }
}
```

---

## 7. LAYER 4: CONTRADICTION DETECTION

### 7.1 Purpose

Detect if the new fact contradicts any existing facts in the Knowledge Graph. Contradictions must be resolved before the fact can be published.

### 7.2 Contradiction Detection Process

```
    New Fact
         |
    [Find Related Facts]
    - Find facts with same or related subject
    - Find facts with same predicate
    - Semantic similarity search for close facts
         |
    [Contradiction Classification]
    - DIRECT_CONTRADICTION: "A is fard" vs "A is not fard"
    - CONDITIONAL_DIFFERENCE: "A is fard" vs "A is fard under conditions B"
    - SCHOLARLY_DISAGREEMENT: "School X says A" vs "School Y says B"
    - TEMPORAL_DIFFERENCE: Ruling changed over time
    - CONTEXTUAL_DIFFERENCE: Different contexts, not truly contradictory
         |
    [Contradiction Type Determination]
    - Use NLI (Natural Language Inference) model
    - Classify relationship: entails, contradicts, neutral
         |
    [If Contradiction Found]
    - Record contradiction edge between facts
    - Assess severity (major/minor)
    - Flag for scholarly review
    - Do not publish new fact until resolved
```

### 7.3 Contradiction Resolution

| Resolution Type | Description | Timeframe |
|---|---|---|
| **False alarm** | Facts are not actually contradictory (different context) | Automated, immediate |
| **Qualification** | Add qualifiers to reconcile differences | Scholarly review, 7 days |
| **Complementary** | Both facts are valid in different contexts | Add context linking |
| **One incorrect** | One fact is wrong; deprecate it | Scholarly review, 7 days |
| **Legitimate ikhtilaf** | Genuine scholarly difference of opinion | Present as diversity of opinion |
| **Needs research** | Requires further scholarly investigation | Escalate to advisory board |

### 7.4 Contradiction Output

```json
{
  "fact_id": "fact-hand-position-hanafi",
  "contradiction_result": {
    "contradictions_found": 1,
    "contradictions": [
      {
        "existing_fact_id": "fact-hand-position-shafii",
        "existing_fact": "Hands should be placed on the chest during Salah",
        "new_fact": "Hands should be placed below the navel during Salah",
        "classification": "scholarly_disagreement",
        "nli_result": "contradicts",
        "severity": "minor",
        "resolution": "legitimate_ikhtilaf",
        "explanation": "This is a well-known scholarly difference between the Hanafi and Shafi'i schools. Both positions are valid."
      }
    ],
    "non_contradictory_relations": 12,
    "confidence": 0.98
  }
}
```

---

## 8. LAYER 5: SCHOLARLY REVIEW

### 8.1 Purpose

Human expert review for high-sensitivity facts, facts with low automated confidence, facts involving scholarly disagreement, and any fact related to core Islamic beliefs and practices.

### 8.2 When Scholarly Review is Required

| Condition | Domain Examples | Priority |
|---|---|---|
| Automated confidence < 0.70 | All domains | High |
| L4 contradiction detected | All domains | High |
| Fact involves scholarly disagreement | Fiqh, Aqeedah | High |
| Fact is a Quranic ruling | Tafsir, Fiqh | Critical |
| Fact is a Hadith-based ruling | Hadith, Fiqh | High |
| Fact about core beliefs | Aqeedah | Critical |
| New fact contradicts established knowledge | All domains | Critical |
| User or scholar reports error | All domains | Varies |
| First fact from a new source | All domains | Medium |

### 8.3 Scholarly Review Interface

```
    +--------------------------------------------------------+
    |             SCHOLARLY REVIEW INTERFACE                    |
    |                                                          |
    |  Fact: "Salah is obligatory (fard) upon every adult      |
    |         Muslim"                                          |
    |                                                          |
    |  Source: Sahih al-Bukhari, Book 8, Hadith 1              |
    |  Confidence: 0.96 (Automated)                            |
    |                                                          |
    |  Cross-Reference: Confirmed (8 sources)                  |
    |  LLM Check: Consistent (0.94)                            |
    |  Contradictions: None                                    |
    |                                                          |
    |  [Approve] [Approve with Note] [Reject] [Edit]           |
    |                                                          |
    |  Notes:                                                   |
    |  +------------------------------------------------------+|
    |  | This is correct but should mention the exception     ||
    |  | for those with valid excuses.                        ||
    |  +------------------------------------------------------+|
    +--------------------------------------------------------+
```

### 8.4 Scholarly Review Output

```json
{
  "fact_id": "fact-salah-obligation-001",
  "scholarly_review_result": {
    "reviewer_id": "scholar-ahmed-001",
    "reviewer_name": "Dr. Ahmed al-Tayyeb",
    "review_date": "2026-07-07",
    "decision": "approved_with_note",
    "note": "Fact is correct. I have added a qualifier about exceptions for those with valid excuses.",
    "modified_fact": "Salah is obligatory (fard 'ayn) upon every adult, sane Muslim who has reached puberty, except those with valid Shar'i excuses such as menstruation, post-partum bleeding, or severe illness.",
    "confidence_assignment": 1.0,
    "review_time_minutes": 5
  }
}
```

---

## 9. VERIFICATION WORKFLOW

### 9.1 Complete Verification Workflow

```
    Fact Extracted
         |
    v
    [L1: Source Verification]
    |
    +-- Pass: Continue
    +-- Fail: Reject fact, flag source
    |
    v
    [L2: Cross-Reference]
    |
    +-- Confirmed (score >= 0.90): Continue
    +-- Supported (score >= 0.70): Continue, flag for review
    +-- Unverified (score < 0.70): Hold for scholarly review
    +-- Contradicted: Flag contradiction, stop
    |
    v
    [L3: LLM Consistency Check]
    |
    +-- Consistent (score >= 0.85): Continue
    +-- Questionable (score >= 0.60): Flag for review
    +-- Inconsistent (score < 0.60): Hold for human review
    |
    v
    [L4: Contradiction Detection]
    |
    +-- No contradiction: Continue
    +-- Scholarly disagreement: Flag as ikhtilaf, continue
    +-- Direct contradiction: Hold for resolution
    |
    v
    [Is this a sensitive domain?]
    |
    +-- Yes: Route to L5 Scholarly Review
    +-- No: Skip to confidence aggregation
    |
    v
    [L5: Scholarly Review] (if required)
    |
    +-- Approved: Continue
    +-- Approved with note: Accept modified fact
    +-- Rejected: Fact not published
    +-- Needs revision: Return for modification
    |
    v
    [Confidence Aggregation]
    |
    v
    [Knowledge Graph Update]
    |
    +-- Confident (score >= 0.85): Publish as verified
    +-- Moderate (score >= 0.70): Publish with confidence note
    +-- Low (score < 0.70): Store but do not publish for content generation
    |
    v
    [Provenance Recording]
```

### 9.2 Verification SLA

| Step | Max Time | SLA Target |
|---|---|---|
| L1: Source Verification | 15 minutes | 99% |
| L2: Cross-Reference | 2 hours | 95% |
| L3: LLM Check | 5 minutes | 99% |
| L4: Contradiction | 1 hour | 95% |
| L5: Scholarly Review | 48 hours (high), 7 days (normal) | 90% |
| **Total (automated)** | **4 hours** | **90%** |
| **Total (with human review)** | **7 days** | **95%** |

---

## 10. CONFIDENCE AGGREGATION

### 10.1 Aggregation Formula

```
Overall Confidence = L1 * 0.10 + L2 * 0.35 + L3 * 0.20 + L4 * 0.15 + (L5 * 0.20 if reviewed)

Where:
L1 = source_verification_confidence
L2 = cross_reference_corroboration_score
L3 = llm_check_consistency_score
L4 = 1.0 if no contradiction, 0.5 if scholarly disagreement, 0.0 if direct contradiction
L5 = 1.0 if approved, 0.8 if approved_with_note, 0.0 if rejected

If L5 not performed:
  Overall = (L1 * 0.10 + L2 * 0.35 + L3 * 0.20 + L4 * 0.15) / 0.80
```

### 10.2 Final Verification Decision

| Overall Score | Status | Action |
|---|---|---|
| >= 0.95 | VERIFIED | Available for all content generation |
| 0.85 - 0.94 | VERIFIED_MEDIUM | Available with confidence note |
| 0.70 - 0.84 | FLAGGED | Available for admin/scholar review only |
| 0.50 - 0.69 | UNCERTAIN | Stored but not available for content |
| < 0.50 | REJECTED | Not stored, extraction pipeline notified |

### 10.3 Domain-Specific Thresholds

| Domain | Min for Content Gen | Min for Storage | Min for AI Teacher |
|---|---|---|---|
| Quran (verses, rulings) | 0.99 | 0.95 | 0.99 |
| Hadith (text, grading) | 0.95 | 0.90 | 0.95 |
| Fiqh (rulings) | 0.90 | 0.80 | 0.90 |
| Aqeedah (beliefs) | 0.95 | 0.90 | 0.95 |
| Seerah (events) | 0.85 | 0.75 | 0.85 |
| History (general) | 0.80 | 0.70 | 0.80 |
| Biography | 0.85 | 0.75 | 0.85 |
| Arabic Language | 0.90 | 0.80 | 0.90 |
| Contemporary Issues | 0.85 | 0.75 | 0.85 |

---

## 11. CONTINUOUS VERIFICATION

### 11.1 Re-Verification Triggers

| Trigger | Scope | Frequency |
|---|---|---|
| New authoritative source added | Related facts | Event-driven |
| Scheduled domain review | Entire domain | Weekly (critical), Monthly (standard) |
| Error report received | Specific fact | Immediate |
| Content generation failure | Related facts | Immediate |
| User report of inaccuracy | Specific fact | Immediate |
| Model upgrade (verification) | All processed facts | Quarterly |
| New contradictory evidence | Contradicted facts | Event-driven |

### 11.2 Re-Verification Process

```
    Trigger Event
         |
    [Identify Affected Facts]
    - By domain, source, or specific fact
         |
    [Queue for Re-Verification]
    - Priority based on impact
         |
    [Run L2-L4 (automated)]
    - Compare against updated knowledge base
    - Detect changes in corroboration
    - Detect new contradictions
         |
    [Compare with Original Verification]
    - Score change > 0.10: Flag for review
    - New contradiction: Flag for resolution
         |
    [Update Knowledge Graph]
    - Update confidence scores
    - Add new verification chain entry
    - Mark as re-verified with timestamp
         |
    [Notify Impacted Content]
    - Identify content using this fact
    - Flag for content refresh
```

---

## 12. HUMAN REVIEW MANAGEMENT

### 12.1 Review Queue

```yaml
review_queue:
  priority_levels:
    critical:
      max_queue_time: 4 hours
      reviewers: ["senior_scholar"]
      notification: "immediate"
    
    high:
      max_queue_time: 24 hours
      reviewers: ["domain_specialist", "scholar"]
      notification: "within_1_hour"
    
    normal:
      max_queue_time: 7 days
      reviewers: ["scholar"]
      notification: "daily_digest"
    
    low:
      max_queue_time: 30 days
      reviewers: ["scholar", "research_assistant"]
      notification: "weekly_digest"
  
  reviewer_assignment:
    strategy: "expertise_based"
    fallback: "round_robin"
    max_pending_per_reviewer: 20
    escalation_after: "2x max_queue_time"
```

### 12.2 Reviewer Performance Metrics

| Metric | Target | Review Cycle |
|---|---|---|
| Reviews completed per week | 50+ (full-time scholar) | Weekly |
| Average review time | <15 minutes (standard), <60 min (complex) | Monthly |
| Accuracy rate | >98% agreement with senior review | Quarterly |
| Response time (critical) | <4 hours | Monthly |
| Response time (high) | <24 hours | Monthly |

---

## 13. QUALITY METRICS

### 13.1 Verification Quality Dashboard

| Metric | Target | Measurement | Alert |
|---|---|---|---|
| Verification coverage | 100% of extracted facts | All facts have verification chain | <100% |
| Overall accuracy rate | >99.97% | Random audit of published facts | <99.9% |
| False positive rate (errors published) | <0.03% | User reports + automated detection | >0.1% |
| False negative rate (correct facts rejected) | <0.5% | Audit of rejected facts | >1% |
| L1-L4 pass rate | >85% first attempt | Facts passing without human review | <70% |
| Human review turnaround (critical) | <4 hours | Queue time | >8 hours |
| Human review turnaround (normal) | <7 days | Queue time | >14 days |
| Contradiction detection rate | >95% of contradictions found | Audit of known contradictions | <90% |
| Re-verification timeliness | <24 hours for critical trigger | Time from trigger to completion | >48 hours |

---

## 14. PERFORMANCE

### 14.1 Verification Throughput

| Layer | Facts/Hour (Single Instance) | Instances Needed (Year 5) |
|---|---|---|
| L1: Source | 10,000 | 2 |
| L2: Cross-Reference | 5,000 | 8 |
| L3: LLM Check | 1,000 | 20 |
| L4: Contradiction | 10,000 (per scan) | 2 |
| L5: Scholarly (human) | 10 | 20 scholars |

### 14.2 Cost per Fact

| Layer | Cost per Fact (USD) |
|---|---|
| L1: Source Verification | $0.0001 |
| L2: Cross-Reference | $0.005 |
| L3: LLM Check | $0.02 |
| L4: Contradiction | $0.001 |
| L5: Scholarly Review | $2.00 (per fact) |
| **Total (automated only)** | **$0.026** |
| **Total (with scholarly review)** | **$2.03** |

---

## 15. SECURITY

### 15.1 Verification System Security

| Concern | Mitigation |
|---|---|
| **Tampering with verification results** | Immutable audit trail, cryptographic verification chain |
| **False positives (suppressing correct facts)** | Multiple independent verifiers, human appeal process |
| **False negatives (accepting incorrect facts)** | Multi-layer system reduces single-point failure |
| **Scholar bias** | Multiple scholars, diverse madhab representation |
| **Verification process manipulation** | All verification actions logged and auditable |

### 15.2 Verification Data Integrity

```sql
-- Append-only verification log
CREATE TABLE verification_chain (
    id UUID PRIMARY KEY,
    fact_id UUID NOT NULL,
    layer TEXT NOT NULL,
    result JSONB NOT NULL,
    previous_hash TEXT NOT NULL,
    current_hash TEXT NOT NULL,
    created_at TIMESTAMPTZ DEFAULT NOW(),
    
    CONSTRAINT fk_previous 
        FOREIGN KEY (previous_hash) 
        REFERENCES verification_chain(current_hash)
);

-- Verify chain integrity
SELECT COUNT(*) FROM verification_chain vc1
LEFT JOIN verification_chain vc2 
    ON vc1.previous_hash = vc2.current_hash
WHERE vc2.id IS NULL AND vc1.previous_hash != 'GENESIS';
```

---

## 16. APPENDICES

### Appendix A: Verification State Machine

```yaml
verification_states:
  PENDING:              { description: "Awaiting verification", initial: true }
  IN_SOURCE_CHECK:      { description: "Running L1 source verification" }
  IN_CROSS_REF:         { description: "Running L2 cross-reference" }
  IN_LLM_CHECK:         { description: "Running L3 LLM consistency check" }
  IN_CONTRADICTION:     { description: "Running L4 contradiction detection" }
  IN_SCHOLARLY_REVIEW:  { description: "Awaiting L5 scholarly review" }
  VERIFIED:             { description: "Fully verified and published" }
  VERIFIED_LOW:         { description: "Verified with moderate confidence" }
  FLAGGED:              { description: "Flagged for admin review" }
  CONTROVERSIAL:        { description: "Scholarly disagreement recorded" }
  REJECTED:             { description: "Failed verification" }
  DEPRECATED:           { description: "Superseded by new evidence" }
  
  transitions:
    - from: PENDING
      to: [IN_SOURCE_CHECK, REJECTED]
    - from: IN_SOURCE_CHECK
      to: [IN_CROSS_REF, REJECTED]
    - from: IN_CROSS_REF
      to: [IN_LLM_CHECK, IN_SCHOLARLY_REVIEW, REJECTED]
    - from: IN_LLM_CHECK
      to: [IN_CONTRADICTION, IN_SCHOLARLY_REVIEW, REJECTED]
    - from: IN_CONTRADICTION
      to: [IN_SCHOLARLY_REVIEW, VERIFIED, VERIFIED_LOW, REJECTED]
    - from: IN_SCHOLARLY_REVIEW
      to: [VERIFIED, VERIFIED_LOW, CONTROVERSIAL, REJECTED]
    - from: [VERIFIED, VERIFIED_LOW]
      to: [DEPRECATED, CONTROVERSIAL]
    - from: FLAGGED
      to: [VERIFIED, VERIFIED_LOW, REJECTED]
```

### Appendix B: Verification Rules by Domain

| Domain | L1 Source Min | L2 Cross-Ref Min | L3 LLM Min | L5 Required | Auto-Publish Threshold |
|---|---|---|---|---|---|
| Quran | 0.99 | 0.99 | 0.99 | Always | 0.99 |
| Hadith | 0.95 | 0.95 | 0.95 | If hasan or lower | 0.95 |
| Fiqh (Ibadah) | 0.95 | 0.90 | 0.90 | If disagreement exists | 0.90 |
| Fiqh (Muamalat) | 0.90 | 0.85 | 0.85 | If novel ruling | 0.85 |
| Aqeedah | 0.99 | 0.95 | 0.95 | Always | 0.95 |
| Seerah | 0.90 | 0.85 | 0.85 | If controversial | 0.85 |
| History | 0.85 | 0.80 | 0.80 | If disputed | 0.80 |
| Biography | 0.85 | 0.80 | 0.80 | If living person | 0.80 |
| Language | 0.90 | 0.85 | 0.85 | Never | 0.85 |
| Contemporary | 0.85 | 0.80 | 0.85 | If sensitive | 0.80 |

### Appendix C: Related Documents

| Document ID | Title | Relationship |
|---|---|---|
| IEP-CONST-001 | Project Constitution | Constitutional basis for verification |
| IEP-ARCH-KG-001 | Knowledge Graph Architecture | Verified facts stored here |
| IEP-ARCH-RAG-001 | RAG Architecture | Uses verified facts |
| IEP-ARCH-AGENT-001 | Agent Architecture | Verification agents |
| IEP-AGENTS-001 | Agents Specification | L1-L4 agents, L5 human workflow |
| IEP-SEC-001 | Security Architecture | Verification data integrity |

---

**END OF VERIFICATION ARCHITECTURE**
