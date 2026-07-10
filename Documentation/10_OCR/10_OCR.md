# OCR ARCHITECTURE

## Optical Character Recognition Pipeline for Islamic Texts

---

| Document ID | IEP-ARCH-OCR-001 |
|---|---|
| Version | 1.0.0 |
| Status | Approved |
| Classification | Enterprise Internal |
| Effective Date | 2026-07-07 |
| Approving Authority | Office of the Chief Architect |
| Custodian | Document Processing Architecture Team |
| Database Reference | IEP-ARCH-DB-001 |
| Knowledge Graph Reference | IEP-ARCH-KG-001 |
| RAG Reference | IEP-ARCH-RAG-001 |

---

## TABLE OF CONTENTS

1. EXECUTIVE OVERVIEW
2. OCR PHILOSOPHY
3. MULTI-STAGE OCR PIPELINE
4. STAGE 1: DOCUMENT ACQUISITION
5. STAGE 2: IMAGE PREPROCESSING
6. STAGE 3: LAYOUT ANALYSIS
7. STAGE 4: TEXT RECOGNITION
8. STAGE 5: POST-PROCESSING
9. STAGE 6: VERIFICATION
10. ARABIC SCRIPT SPECIALIZATION
11. QURANIC TEXT HANDLING
12. HADITH TEXT HANDLING
13. MULTI-LANGUAGE SUPPORT
14. DIACRITIC RESTORATION
15. CONFIDENCE SCORING
16. ERROR DETECTION AND CORRECTION
17. INTEGRATION WITH KNOWLEDGE GRAPH
18. QUALITY METRICS
19. PERFORMANCE REQUIREMENTS
20. SECURITY AND PRIVACY
21. TESTING AND VALIDATION
22. APPENDICES

---

## 1. EXECUTIVE OVERVIEW

### 1.1 Purpose

The OCR (Optical Character Recognition) Architecture is the primary gateway through which physical Islamic books enter the platform's digital ecosystem. It transforms printed, scanned, and handwritten Islamic texts into machine-readable, structured content that can be verified, stored in the Knowledge Graph, and used for multi-modal content generation.

### 1.2 Scope

This document covers the entire OCR pipeline from document acquisition through text extraction, verification, and integration with downstream systems. It addresses the unique challenges of Islamic text OCR including Arabic script recognition, Quranic diacritic handling, multi-language support (Arabic, Persian, Urdu, Malay, Turkish, English), and specialized layouts found in Islamic publications.

### 1.3 OCR Pipeline Summary

| Stage | Function | Input | Output | Automation |
|---|---|---|---|---|
| Acquisition | Document intake and validation | Physical books, PDFs, images | Validated digital documents | Automated |
| Preprocessing | Image enhancement and normalization | Raw scans | Cleaned, normalized images | Automated |
| Layout Analysis | Structure detection and zoning | Processed images | Structured page regions | Automated |
| Text Recognition | Character and word recognition | Zoned regions | Raw text with confidence scores | Automated |
| Post-Processing | Correction, diacritic restoration, formatting | Raw OCR text | Cleaned, structured text | Automated + Manual |
| Verification | Accuracy validation and quality control | Structured text | Verified digital text | Automated + Manual |

---

## 2. OCR PHILOSOPHY

### 2.1 Core Principles

| Principle | Description |
|---|---|
| **Preservation First** | The digital representation must preserve every detail of the original, including layout, formatting, diacritics, and marginalia. |
| **Confidence-Gated** | Text only enters the Knowledge Graph when OCR confidence exceeds defined thresholds. Below-threshold text requires human review. |
| **Context-Aware Recognition** | Recognition algorithms leverage knowledge of Islamic text conventions (e.g., Quranic verse structure, Hadith chain format) to improve accuracy. |
| **Multi-Pass Refinement** | OCR is not a single pass. Text passes through multiple recognition and verification stages, each improving accuracy. |
| **Human-in-the-Loop** | Critical texts (Quran, authenticated Hadith) require human verification regardless of automated confidence. |
| **Format Agnostic** | The pipeline handles diverse input formats: high-resolution scans, smartphone photos, PDFs, microfilm, and handwritten manuscripts. |

### 2.2 OCR Quality Tiers

| Tier | Accuracy Requirement | Verification | Use Case |
|---|---|---|---|
| **Tier 1: Critical** | > 99.9% character accuracy | Full human verification required | Quran, authenticated Hadith, primary sources |
| **Tier 2: Scholarly** | > 99.5% character accuracy | Automated + spot-check human review | Classical tafsir, fiqh, aqidah texts |
| **Tier 3: Standard** | > 99.0% character accuracy | Automated verification only | Contemporary Islamic books, translations |
| **Tier 4: Reference** | > 98.0% character accuracy | Automated verification, minimal human review | Indexes, bibliographies, reference works |
| **Tier 5: Draft** | > 95.0% character accuracy | Flagged for future review | Draft material, notes, supplementary content |

### 2.3 Knowledge-Truth Separation Application

| Principle | OCR Implementation |
|---|---|
| **Knowledge (Data)** | The raw OCR output text with confidence scores, bounding boxes, and image references. This includes recognition uncertainty. |
| **Truth (Verified Fact)** | The verified text that has passed all OCR verification stages and been confirmed accurate. This is what enters the Knowledge Graph. |
| **Separation Mechanism** | The Verification Agent maintains the distinction. OCR output is always treated as "unverified knowledge" until it passes verification. Verification history includes the original OCR text, the corrected text, and the chain of verification operations. |
| **Provenance Tracking** | Every character in the verified text is traceable to its position in the original source image and the OCR confidence at the time of recognition. |

---

## 3. MULTI-STAGE OCR PIPELINE

### 3.1 Pipeline Architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         DOCUMENT ACQUISITION                            │
│  Physical Book → Scan → Validate → Classify → Route                    │
└──────────────────────────────┬──────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                         IMAGE PREPROCESSING                             │
│  Deskew → Denoise → Binarize → Enhance → Normalize                    │
└──────────────────────────────┬──────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                          LAYOUT ANALYSIS                                │
│  Page Segmentation → Region Detection → Reading Order → Structure      │
└──────────────────────────────┬──────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                          TEXT RECOGNITION                               │
│  Line Detection → Word Segmentation → Character Recognition → Decoding │
└──────────────────────────────┬──────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                          POST-PROCESSING                                │
│  Spell Check → Diacritic Restoration → Formatting → Structure Rebuild  │
└──────────────────────────────┬──────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                          VERIFICATION                                   │
│  Automated Checks → Cross-Reference → Confidence Scoring → Human Review │
└──────────────────────────────┬──────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                    KNOWLEDGE GRAPH INTEGRATION                          │
│  Entity Extraction → Relationship Mapping → Fact Insertion            │
└─────────────────────────────────────────────────────────────────────────┘
```

### 3.2 Pipeline Orchestration

The OCR pipeline is orchestrated by the Master Orchestrator (IEP-ARCH-ORCH-001) which manages:

| Orchestration Function | Description |
|---|---|
| **Document Queue Management** | Priority-based document queuing based on content tier and urgency |
| **Worker Allocation** | Dynamic allocation of OCR processing workers based on document complexity |
| **Stage Transition** | Manages progression through pipeline stages with quality gates |
| **Error Handling** | Detects failures at each stage and triggers retry or human escalation |
| **Progress Tracking** | Maintains real-time status of every document in the pipeline |
| **Resource Management** | Manages GPU allocation for deep learning models, storage for intermediate artifacts |

### 3.3 Parallel Processing Architecture

| Processing Mode | Description | Use Case |
|---|---|---|
| **Sequential (Page-by-Page)** | Pages processed one at a time | Simple, modern books with uniform layout |
| **Parallel (Batch)** | Multiple pages processed simultaneously | Large books with consistent formatting |
| **Distributed** | Pages distributed across multiple OCR servers | High-volume processing of standard texts |
| **Priority (Single-Thread)** | Dedicated processing with maximum accuracy | Critical texts (Quran, authenticated Hadith) |

---

## 4. STAGE 1: DOCUMENT ACQUISITION

### 4.1 Input Sources

| Source | Format | Resolution Requirement | Special Considerations |
|---|---|---|---|
| Professional Scanner | TIFF, 300-600 DPI | Minimum 300 DPI, 24-bit color | Best quality, recommended for critical texts |
| Smartphone Camera | JPEG, PNG | Minimum 8 MP | Perspective correction, lighting normalization required |
| Digital PDF (Born-Digital) | PDF | N/A | Direct text extraction may be possible, OCR for verification |
| Scanned PDF | PDF (image-based) | Varies, minimum 200 DPI | Extract images for processing |
| Microfilm Scan | TIFF, JPEG2000 | Minimum 400 DPI | Dust, scratches, and film grain need specialized processing |
| Handwritten Manuscript | JPEG, TIFF | Minimum 600 DPI | Specialized handwriting recognition models required |

### 4.2 Document Validation

On intake, every document undergoes validation:

| Validation Check | Description | Pass/Fail Criteria |
|---|---|---|
| **Image Quality** | Blur detection, brightness, contrast, resolution | Minimum sharpness score, resolution ≥ 200 DPI |
| **Completeness** | All pages present, correct page order | Page count within ±5% of expected |
| **Format Support** | File format is in supported list | Must be TIFF, JPEG, PNG, PDF, or JPEG2000 |
| **Corruption Check** | File integrity verification | No corruption in file header or data |
| **Duplicate Detection** | Content hash comparison against existing documents | No exact duplicate (unless re-scan requested) |
| **Language Detection** | Automatic script and language identification | Must match expected language(s) for this document |

### 4.3 Document Classification

After validation, documents are classified:

| Classification Attribute | Options | Impact on Processing |
|---|---|---|
| **Content Tier** | Critical, Scholarly, Standard, Reference, Draft | Determines verification rigor and review requirements |
| **Language** | Arabic, Persian, Urdu, Malay, Turkish, English, Multi | Selects language-specific models and dictionaries |
| **Script Type** | Naskh, Ruqah, Diwani, Thuluth, Nastaliq, Kufic, Printed | Selects appropriate recognition models |
| **Book Type** | Quran, Hadith, Tafsir, Fiqh, Aqidah, Seerah, Language, General | Activates domain-specific post-processing rules |
| **Page Complexity** | Simple (text-only), Moderate (text + headers), Complex (multi-column, tables, diagrams) | Determines layout analysis strategy |
| **Age/Condition** | Modern, Aged, Damaged, Manuscript | Determines preprocessing requirements |

### 4.4 Document Queue Management

| Queue | Priority | Processing SLA | Description |
|---|---|---|---|
| **Critical Queue** | P0 | < 24 hours | Quran, authenticated Hadith, primary sources |
| **Scholarly Queue** | P1 | < 72 hours | Classical texts, academic works |
| **Standard Queue** | P2 | < 1 week | Contemporary books, general content |
| **Reference Queue** | P3 | < 2 weeks | Supplementary material, indexes |
| **Bulk Queue** | P4 | Best effort | Mass digitization projects, archives |

---

## 5. STAGE 2: IMAGE PREPROCESSING

### 5.1 Preprocessing Pipeline

```
Raw Image
    │
    ▼
┌─────────────────────┐
│ 1. Deskewing        │  Correct page rotation (critical for Arabic script)
└─────────────────────┘
    │
    ▼
┌─────────────────────┐
│ 2. Denoising        │  Remove salt-and-pepper noise, background artifacts
└─────────────────────┘
    │
    ▼
┌─────────────────────┐
│ 3. Binarization     │  Convert to binary (black/white) using adaptive thresholding
└─────────────────────┘
    │
    ▼
┌─────────────────────┐
│ 4. Enhancement      │  Contrast adjustment, edge sharpening, stroke normalization
└─────────────────────┘
    │
    ▼
┌─────────────────────┐
│ 5. Normalization    │  Standardize to uniform resolution, orientation, size
└─────────────────────┘
    │
    ▼
Processed Image
```

### 5.2 Deskewing

| Parameter | Specification |
|---|---|
| **Detection Method** | Hough Transform + Projection Profile Analysis |
| **Correction Range** | ±15 degrees |
| **Target Accuracy** | < 0.1 degree residual skew |
| **Arabic-Specific** | Preserves diacritic positions during rotation (bilinear interpolation) |
| **Failure Mode** | If skew > 15 degrees, flag for manual orientation correction |

### 5.3 Denoising

| Technique | Application | Parameters |
|---|---|---|
| **Median Filter** | Salt-and-pepper noise removal | Kernel size: 3x3 |
| **Non-Local Means** | Background texture removal | h = 10, templateWindowSize = 7, searchWindowSize = 21 |
| **Morphological Operations** | Remove small artifacts | Opening operation, kernel: 2x2 |
| **Dust/Scratch Removal** | Microfilm and aged document artifacts | Adaptive inpainting |
| **Background Subtraction** | Uniform background removal | Otsu thresholding + morphological smoothing |

### 5.4 Binarization

| Method | Application | Parameters |
|---|---|---|
| **Adaptive Gaussian Thresholding** | Variable lighting documents | Block size: 33, C: 5 |
| **Otsu's Thresholding** | Uniform lighting, good contrast | Global threshold |
| **Sauvola's Method** | Aged documents, varying backgrounds | Window size: 45, k: 0.2 |
| **Phase Preservation** | Documents with faint diacritics | Preserves phase information for small marks |

**Critical Requirement**: Binarization must preserve diacritics (tashkeel), dots (i'jam), and small marks (sukoon, shaddah) that are essential for Arabic text accuracy.

### 5.5 Enhancement

| Operation | Purpose | Method |
|---|---|---|
| **Contrast Limited Adaptive Histogram Equalization (CLAHE)** | Improve local contrast | Clip limit: 2.0, tile size: 8x8 |
| **Stroke Width Normalization** | Uniform stroke thickness | Morphological thinning/thickening |
| **Edge Enhancement** | Sharpen character boundaries | Unsharp masking, kernel: 5x5 |
| **Shadow Removal** | Documents with page curvature shadows | Surface fitting and subtraction |
| **Perspective Correction** | Camera-captured documents | Homography estimation from page corners |

### 5.6 Normalization

| Parameter | Target | Method |
|---|---|---|
| **Resolution** | 300 DPI standard, 600 DPI for manuscripts | Linear interpolation |
| **Orientation** | Top-left origin, text flowing right-to-left | Automatic orientation detection |
| **Color Depth** | 1-bit (binary) for OCR, 8-bit grayscale for analysis | Thresholding or grayscale conversion |
| **Page Size** | Standardized to A4 dimensions (with aspect ratio preservation) | Resize with padding |
| **Margin Removal** | Remove excessive borders | Content-based cropping |

---

## 6. STAGE 3: LAYOUT ANALYSIS

### 6.1 Layout Analysis Pipeline

```
Processed Image
    │
    ▼
┌──────────────────────────┐
│ 1. Page Segmentation     │  Identify text regions, images, tables, margins
└──────────────────────────┘
    │
    ▼
┌──────────────────────────┐
│ 2. Region Classification │  Label regions: body text, header, footnote, etc.
└──────────────────────────┘
    │
    ▼
┌──────────────────────────┐
│ 3. Reading Order         │  Determine logical reading sequence for RTL text
└──────────────────────────┘
    │
    ▼
┌──────────────────────────┐
│ 4. Structure Detection   │  Identify chapters, verses, hadith chains, etc.
└──────────────────────────┘
    │
    ▼
Structured Page
```

### 6.2 Page Segmentation Methods

| Method | Approach | Best For | Accuracy |
|---|---|---|---|
| **Deep Learning (LayoutLMv3)** | Transformer-based region detection | Complex layouts, mixed content | > 95% |
| **Connected Component Analysis** | Physical region detection | Simple text-only pages | > 98% |
| **Projection Profile** | Histogram-based column/line detection | Single-column, regular text | > 97% |
| **Hybrid (DL + Rule)** | Deep learning with Islamic layout rules | Specialized Islamic book layouts | > 96% |

### 6.3 Region Types

| Region Type | Description | Processing Requirements |
|---|---|---|
| **Body Text** | Main content text | Standard OCR processing |
| **Header** | Page header (title, chapter name) | Store as metadata, not body text |
| **Footer** | Page number, publisher info | Store as metadata |
| **Footnote** | Marginal notes, citations | Link to anchor text in body |
| **Margin Note** | Scholar annotations in margins | Preserve as separate text block |
| **Table** | Structured tabular data | Table-specific OCR and structure preservation |
| **Image/Diagram** | Non-text visual content | Store reference, do not OCR (describe in metadata) |
| **Quranic Verse** | Special Quranic layout with ayah markers | Specialized Quranic OCR (see Section 11) |
| **Hadith Chain** | Sanad (chain of narration) format | Specialized Hadith OCR (see Section 12) |
| **Matn** | Hadith text body | Standard OCR with specialized verification |
| **Poetry** | Poetic verse layout | Line-by-line preservation with meter analysis |

### 6.4 Reading Order Determination

For Arabic (RTL) text with potential mixed LTR content:

| Rule | Implementation |
|---|---|
| **Primary Direction** | Right-to-left for Arabic body text |
| **Number Handling** | Digits may be LTR (Western numerals) or RTL (Hindi numerals) |
| **Mixed Content** | Embedded LTR content (English terms, numbers) detected and handled per Unicode Bidirectional Algorithm |
| **Multi-Column** | Top-to-bottom, right-to-left column progression |
| **Footnotes** | Read after main body, linked by anchor markers |
| **Table Reading** | Right-to-left within rows, top-to-bottom across rows |

### 6.5 Structure Detection Rules

| Book Type | Structural Elements | Detection Method |
|---|---|---|
| **Quran** | Surah, Ayah, Juz, Hizb, Ruku, Sajdah markers | Pattern matching + known position database |
| **Hadith** | Book, Chapter, Hadith number, Sanad, Matn | Pattern matching + known collection structure |
| **Tafsir** | Surah, Ayah range, commentator attribution | Hierarchical structure detection |
| **Fiqh** | Book, Chapter, Section, Sub-section | Hierarchical pattern matching |
| **General Book** | Chapter, Section, Paragraph, Footnote | Generic structure detection |

---

## 7. STAGE 4: TEXT RECOGNITION

### 7.1 Recognition Architecture

```
Zoned Region Image
    │
    ▼
┌──────────────────────────┐
│ 1. Line Segmentation     │  Detect and extract individual text lines
└──────────────────────────┘
    │
    ▼
┌──────────────────────────┐
│ 2. Word Segmentation     │  Segment lines into individual words
└──────────────────────────┘
    │
    ▼
┌──────────────────────────┐
│ 3. Character Recognition │  Recognize characters using deep learning models
└──────────────────────────┘
    │
    ▼
┌──────────────────────────┐
│ 4. Decoding              │  Convert recognition output to text with confidence
└──────────────────────────┘
    │
    ▼
Raw Text with Confidence
```

### 7.2 Recognition Models

| Model | Architecture | Training Data | Best For | CER |
|---|---|---|---|---|
| **Arabic-CRNN** | CNN + BiLSTM + CTC | 10M+ Arabic text lines | Printed Arabic (Naskh, printed fonts) | < 0.5% |
| **Arabic-VisionTransformer** | ViT + Seq2Seq | 5M+ Arabic text lines | Complex Arabic scripts | < 1.0% |
| **Quran-OCR** | Custom CNN + Transformer | 2M+ Quranic text lines | Quran with diacritics | < 0.1% |
| **Hadith-OCR** | CNN + BiLSTM + Attention | 3M+ Hadith text lines | Hadith text with chains | < 0.3% |
| **Nastaliq-OCR** | Custom CNN + BiLSTM | 1M+ Nastaliq lines | Urdu, Persian Nastaliq script | < 2.0% |
| **Multi-Lingual** | Transformer (mT5-based) | 20M+ lines, 10 languages | Mixed-language content | < 1.5% |
| **Handwriting** | CNN + Transformer | 500K+ handwritten lines | Manuscripts, handwritten notes | < 5.0% |

### 7.3 Line Segmentation

| Method | Description | Accuracy |
|---|---|---|
| **Projection Profile** | Horizontal histogram analysis for line gaps | > 97% for regular text |
| **Deep Learning (UNet)** | Pixel-level line segmentation | > 99% for complex layouts |
| **Connected Component** | Group connected components into lines | > 95% for well-spaced text |
| **Hybrid** | DL segmentation + projection refinement | > 99.5% |

### 7.4 Word Segmentation

For Arabic script, word segmentation is complex due to:
- Cursive nature (letters connect within words)
- Space characters are word boundaries
- Some particles (و, ف, ب, ل, ك) are prefixes attached to words

| Method | Description | Accuracy |
|---|---|---|
| **Space Detection** | Detect inter-word vs intra-character spaces | > 98% |
| **Ligature Analysis** | Identify common Arabic ligatures (lam-alif, etc.) | > 99% |
| **Prefix Handling** | Recognize and separate common prefixes | > 97% |
| **Post-Segmentation Verification** | Lexicon-based word validation | > 99% |

### 7.5 Character Set

The OCR system recognizes the full Unicode range for Islamic texts:

| Character Set | Unicode Range | Examples |
|---|---|---|
| **Arabic** | U+0600–U+06FF | ا ب ت ث ج ح خ د ذ ر ز س ش ص ض ط ظ ع غ ف ق ك ل م ن ه و ي |
| **Arabic Supplement** | U+0750–U+077F | Extended Arabic characters for Quranic and regional usage |
| **Arabic Extended-A** | U+08A0–U+08FF | Additional marks for Quranic notation |
| **Arabic Presentation Forms** | U+FB50–U+FDFF, U+FE70–U+FEFF | Ligature forms |
| **Quranic** | U+08E0–U+08FF | Quranic annotation signs |
| **Diacritics** | U+0610–U+061A, U+064B–U+065F | Fatha, Damma, Kasra, Sukoon, Shaddah, Madd, etc. |
| **Persian** | U+FB00+ | گ چ پ ژ (additional Persian characters) |
| **Urdu** | U+FB00+ | ڑ ٹ ڈ (additional Urdu characters) |
| **Indic Numerals** | U+0660–U+0669 | Arabic-Indic digits (٠١٢٣٤٥٦٧٨٩) |
| **Western Numerals** | U+0030–U+0039 | 0-9 (used in mixed contexts) |

### 7.6 Recognition Confidence

Each recognized character has associated confidence metadata:

| Confidence Level | Score Range | Action |
|---|---|---|
| **High** | 95-100% | Accept without review |
| **Medium** | 80-94% | Flag for automated post-processing review |
| **Low** | 50-79% | Flag for human review |
| **Critical** | < 50% | Require re-scan or manual transcription |

Word-level confidence is calculated as the weighted average of character confidences, with diacritics and key distinguishing characters (dots) weighted more heavily.

---

## 8. STAGE 5: POST-PROCESSING

### 8.1 Post-Processing Pipeline

```
Raw OCR Text
    │
    ▼
┌──────────────────────────┐
│ 1. Spell Checking        │  Lexicon-based and statistical correction
└──────────────────────────┘
    │
    ▼
┌──────────────────────────┐
│ 2. Diacritic Restoration │  Reconstruct missing or incorrect tashkeel
└──────────────────────────┘
    │
    ▼
┌──────────────────────────┐
│ 3. Formatting            │  Paragraph reconstruction, whitespace normalization
└──────────────────────────┘
    │
    ▼
┌──────────────────────────┐
│ 4. Structure Rebuild     │  Reconstruct document hierarchy
└──────────────────────────┘
    │
    ▼
┌──────────────────────────┐
│ 5. Metadata Extraction   │  Extract bibliographic and structural metadata
└──────────────────────────┘
    │
    ▼
Structured Text
```

### 8.2 Spell Checking

| Component | Description | Accuracy |
|---|---|---|
| **Arabic Lexicon** | 500K+ Arabic word dictionary with morphological forms | > 99% coverage of standard Arabic |
| **Domain Lexicon** | Specialized Islamic terminology lexicon (200K+ terms) | > 99% coverage of Islamic texts |
| **Quranic Lexicon** | Complete Quranic vocabulary with all diacritized forms | 100% coverage |
| **Hadith Lexicon** | Common Hadith terminology, narrator names | > 98% coverage |
| **Statistical Language Model** | n-gram language model for Arabic | Context-aware correction |
| **Confusion Set** | Character confusion pairs (e.g., س/ش, ح/ج, ص/ض) | Targeted correction |

### 8.3 Spell Correction Strategy

| Error Type | Example | Correction Method | Priority |
|---|---|---|---|
| **Single Character Substitution** | س instead of ش | Edit distance + confusion set | High |
| **Missing Character** | كتب instead of كتاب | Lexicon lookup + morphological analysis | High |
| **Extra Character** | كتبت instead of كتب | Lexicon lookup | Medium |
| **Transposition** | كتب instead of كتب | Edit distance | Low |
| **Word Boundary Error** | الكتاب instead of الكتاب | Word segmentation reanalysis | High |
| **Diacritic Error** | فتح instead of فتح | Diacritic restoration (see Section 14) | Critical for Quran |

### 8.4 Formatting Reconstruction

| Operation | Description |
|---|---|
| **Paragraph Detection** | Indentation, line spacing, and content analysis to detect paragraph boundaries |
| **Line Wrapping** | Reconstruct original line breaks from justified text |
| **Whitespace Normalization** | Standardize spaces, remove excessive whitespace |
| **Punctuation Restoration** | Add proper Arabic punctuation (، . ؟ !) where missing |
| **Number Normalization** | Standardize numeral style (Hindi vs. Western) to match document convention |
| **Quote Detection** | Detect and preserve quoted text, distinguishing from body text |

### 8.5 Structure Rebuild

| Document Element | Detection Method | Preservation |
|---|---|---|
| **Title** | Font size, position, and style analysis | Store as title metadata |
| **Chapter Headings** | Numbering pattern + position + font analysis | Store with hierarchical level |
| **Section Headings** | Sub-heading pattern | Store with hierarchical level |
| **Paragraphs** | Indentation + spacing | Preserve paragraph structure |
| **Lists** | Bullet/number detection | Preserve list structure |
| **Tables** | Grid line detection + cell analysis | Preserve tabular structure |
| **Footnotes** | Superscript markers + page-bottom text | Link to anchor positions |
| **Page Numbers** | Fixed position detection | Store as page metadata |

### 8.6 Metadata Extraction

| Metadata Field | Extraction Method | Example |
|---|---|---|
| **Book Title** | Title page OCR + cover analysis | "البداية والنهاية" |
| **Author** | Title page + known author database | "ابن كثير" |
| **Publisher** | Copyright page + publisher database | "دار السلام" |
| **Publication Year** | Date pattern matching | "1420 هـ / 2000 م" |
| **Edition** | Edition number detection | "الطبعة الثالثة" |
| **Volume** | Volume/page number detection | "المجلد الأول" |
| **Language** | Script + content analysis | "العربية" |
| **Page Count** | Page number range | "856 صفحة" |
| **ISBN** | Barcode + number pattern | "978-9960-xxx-xx-x" |

---

## 9. STAGE 6: VERIFICATION

### 9.1 Verification Pipeline

```
Structured Text
    │
    ▼
┌─────────────────────────────────────┐
│ 1. Automated Quality Checks         │
│    • Character accuracy validation  │
│    • Word-level lexicon validation  │
│    • Structural consistency check   │
└─────────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────────┐
│ 2. Cross-Reference Verification     │
│    • Compare against digital sources │
│    • Verify quotes and citations    │
│    • Check numerical consistency    │
└─────────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────────┐
│ 3. Domain-Specific Verification     │
│    • Quran: Compare to verified text │
│    • Hadith: Check chain integrity  │
│    • Poetry: Meter and rhyme check  │
└─────────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────────┐
│ 4. Confidence Scoring              │
│    • Aggregate all verification results │
│    • Calculate overall confidence   │
│    • Determine verification tier    │
└─────────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────────┐
│ 5. Human Review (if required)      │
│    • Present flagged content       │
│    • Scholar/corrector review      │
│    • Final approval or rejection   │
└─────────────────────────────────────┘
    │
    ▼
Verified Text → Knowledge Graph
```

### 9.2 Automated Quality Checks

| Check | Method | Threshold |
|---|---|---|
| **Character Accuracy** | Statistical character distribution vs. expected Arabic distribution | > 95% expected character distribution match |
| **Word Validity** | Percentage of recognized words found in the lexicon | > 98% for Tier 1, > 95% for Tier 2 |
| **Diacritic Accuracy** | Diacritic presence and placement against expected patterns | > 99% for Quranic text |
| **Line Consistency** | Line length variation within expected bounds | Standard deviation < 20% of mean line length |
| **Structure Integrity** | Missing or extra structural elements | Zero missing required elements |
| **Page Number Sequence** | Sequential page number validation | No gaps or duplicates |

### 9.3 Cross-Reference Verification

| Reference Source | Use Case | Method |
|---|---|---|
| **Verified Digital Quran** | Quran OCR verification | Exact character-by-character comparison |
| **Digital Hadith Collections** | Hadith verification | Match with authenticated digital hadith databases |
| **Published Scholar Editions** | Classical text verification | Cross-reference with known critical editions |
| **Library Catalogs** | Metadata verification | Match title, author, edition against library records |
| **ISBN Database** | Book identification | Verify ISBN and bibliographic details |

### 9.4 Human Review Interface

| Feature | Description |
|---|---|
| **Side-by-Side View** | Original scan next to OCR text |
| **Confidence Highlighting** | Color-coded confidence levels per character/word |
| **Direct Correction** | Click-to-correct interface for errors |
| **Batch Approval** | Approve all high-confidence segments at once |
| **Review History** | Complete record of all corrections and approvals |
| **Escalation Path** | Route difficult cases to senior reviewers or scholars |

---

## 10. ARABIC SCRIPT SPECIALIZATION

### 10.1 Arabic Script Challenges

| Challenge | Description | Mitigation Strategy |
|---|---|---|
| **Cursive Writing** | Letters connect within words | Character bounding box + language model decoding |
| **Contextual Forms** | Letters have initial, medial, final, and isolated forms | Unicode normalization + shape-aware recognition |
| **Diacritics** | Small marks above/below letters that change meaning | High-resolution preservation, specialized diacritic models |
| **Dot Placement** | Dots distinguish many letters (ب/ت/ث, ج/ح/خ, etc.) | High-resolution capture, dot-sensitive recognition |
| **Ligatures** | Mandatory and stylistic letter combinations | Comprehensive ligature dictionary + recognition |
| **Justification** | Kashida (tatweel) elongation for text justification | Kashida detection and removal before recognition |
| **Complex Fonts** | Decorative and calligraphic styles | Calligraphy-specific recognition models |
| **Mixed Script** | Arabic + English + numbers in same text | Script detection + multi-model recognition |

### 10.2 Arabic Font Categories

| Category | Examples | Recognition Model | Expected CER |
|---|---|---|---|
| **Printed Naskh** | Standard book font | Arabic-CRNN | < 0.3% |
| **Modern Simplified** | Simplified Arabic, Times New Roman | Arabic-CRNN | < 0.2% |
| **Traditional Naskh** | Traditional printing press | Arabic-CRNN | < 0.5% |
| **Kufic** | Quranic chapter headings | Quran-OCR | < 1.0% |
| **Thuluth** | Decorative headings | Arabic-VisionTransformer | < 2.0% |
| **Diwani** | Ottoman/Official script | Arabic-VisionTransformer | < 3.0% |
| **Nastaliq** | Urdu, Persian, some Arabic | Nastaliq-OCR | < 2.0% |
| **Ruqah** | Handwritten-style, notes | Arabic-VisionTransformer | < 2.5% |

### 10.3 Arabic Language Model

The Arabic language model supports OCR post-processing:

| Component | Description | Coverage |
|---|---|---|
| **Morphological Analyzer** | Root-pattern analysis for Arabic word structure | All Arabic roots |
| **Part-of-Speech Tagger** | Grammatical role identification | Full Arabic POS tagset |
| **Syntactic Parser** | Sentence structure analysis | Arabic grammar rules |
| **Named Entity Recognizer** | Person, place, book title identification | Islamic NER (scholars, books, places) |
| **Semantic Analyzer** | Word sense disambiguation | Context-aware meaning selection |

---

## 11. QURANIC TEXT HANDLING

### 11.1 Quranic OCR Requirements

| Requirement | Specification | Rationale |
|---|---|---|
| **Diacritic Preservation** | 100% of all tashkeel marks | Diacritics change meaning in Quranic Arabic |
| **Quranic Orthography** | Preserve Uthmani script conventions | Standard for printed Mus'haf |
| **Verse Boundary Accuracy** | 100% correct ayah boundaries | Essential for reference and citation |
| **Surah Identification** | Automatic surah recognition | Based on basmalah pattern and surah names |
| **Page Matching** | Match to standard Mus'haf page number | Standard page numbering (604-page Mus'haf) |
| **Hizb/Ruku Markers** | Preserve division markers | For recitation and study tracking |
| **Sajdah Markers** | Identify prostration verses | Critical for recitation rules |
| **Stop/Waqf Marks** | Preserve recitation pause marks | Essential for proper recitation |

### 11.2 Quranic Verification

| Verification Step | Method | Standard |
|---|---|---|
| **Surah Match** | OCR output compared to verified Surah text | King Fahd Complex Mushaf |
| **Verse-by-Verse** | Each ayah verified independently | Al-Azhar approved text |
| **Word-by-Word** | Each word matched to known Quranic word | Quranic lexicon |
| **Diacritic Check** | Every diacritic verified | Complete tashkeel coverage |
| **Orthography Check** | Verify Uthmani script conventions | Standard Uthmani orthography |
| **Basmalah Detection** | Verify basmalah at start of each surah (except Surah 9) | Known pattern |

### 11.3 Quranic OCR Workflow

```
Quran Page Image
    │
    ▼
┌─────────────────────────────────┐
│ 1. Surah & Page Identification  │  Match page pattern to known Mus'haf
└─────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────┐
│ 2. Verse Segmentation           │  Identify ayah markers and boundaries
└─────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────┐
│ 3. Word Recognition             │  High-accuracy word-level recognition
└─────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────┐
│ 4. Diacritic Recognition        │  Specialized diacritic detection model
└─────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────┐
│ 5. Verse Assembly               │  Reconstruct verses from recognized words
└─────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────┐
│ 6. Verification                 │  Compare against verified digital Mushaf
└─────────────────────────────────┘
    │
    ▼
Verified Quranic Text
```

---

## 12. HADITH TEXT HANDLING

### 12.1 Hadith Structure Recognition

A typical Hadith entry has:

| Component | Description | OCR Challenge |
|---|---|---|
| **Hadith Number** | Unique identifier within the collection | Simple number recognition |
| **Sanad (Chain)** | Chain of narrators from the Prophet to the collector | Complex names, lineage terms, honorifics |
| **Matn (Text)** | The content of the hadith | May include Quran quotes, poetry within |
| **Takhrij** | Cross-reference to other collections | Complex citation patterns |
| **Grade** | Authentication grade (Sahih, Hasan, Da'if, etc.) | May be abbreviated or color-coded |
| **Commentary** | Editor's notes and explanations | Mixed with main text |

### 12.2 Supported Hadith Collections

| Collection | Language | Special Features |
|---|---|---|
| Sahih al-Bukhari | Arabic | Complete chain, repeated hadith with different chains |
| Sahih Muslim | Arabic | Chapter structure, narrator groupings |
| Sunan Abi Dawud | Arabic | Chapter headings, abwab structure |
| Sunan al-Tirmidhi | Arabic | Hadith grading inline |
| Sunan al-Nasai | Arabic | Multiple chains for key hadith |
| Sunan Ibn Majah | Arabic | Brief chains |
| Musnad Ahmad | Arabic | Long chains, many narrators |
| Muwatta Malik | Arabic | Mixed hadith and scholar opinions |
| Forty Hadith (Nawawi) | Arabic + Translations | Multi-language parallel text |

### 12.3 Hadith Chain Recognition

| Element | Pattern | Recognition Method |
|---|---|---|
| **حدثنا/أخبرنا** | Transmission phrase | Pattern matching |
| **عن** | "From" (chain link) | Pattern matching |
| **Narrator Name** | Person name with lineage | Named entity recognition |
| **Honorific** | رضي الله عنه, عليه السلام, etc. | Pattern matching, normalization |
| **Chain Continuity** | Connected vs. disconnected | Chain structure analysis |
| **Narrator Death Date** | Year of death for identification | Number pattern matching |

### 12.4 Hadith-Specific Verification

| Verification | Method | Reference |
|---|---|---|
| **Chain Verification** | Match narrators against known hadith chains | Hadith narrator database |
| **Text Comparison** | Compare matn against authenticated editions | Digital hadith library |
| **Number Validation** | Verify hadith number matches collection numbering | Collection index |
| **Grade Verification** | Cross-reference hadith grade with major scholars | Grade database |
| **Duplicate Detection** | Identify hadith repeated with different chains | Content similarity analysis |

---

## 13. MULTI-LANGUAGE SUPPORT

### 13.1 Supported Languages

| Language | Script | OCR Model | Language Model |
|---|---|---|---|
| **Arabic** | Arabic | Arabic-CRNN | Arabic LM + Islamic Domain LM |
| **Persian (Farsi)** | Arabic (Persian) | Nastaliq-OCR / Arabic-CRNN | Persian LM |
| **Urdu** | Arabic (Urdu) | Nastaliq-OCR | Urdu LM |
| **Malay (Jawi)** | Arabic (Jawi) | Arabic-CRNN | Malay LM + Jawi lexicon |
| **Turkish (Ottoman)** | Arabic (Ottoman Turkish) | Arabic-VisionTransformer | Ottoman Turkish LM |
| **Turkish (Modern)** | Latin | Multi-Lingual | Turkish LM |
| **English** | Latin | Multi-Lingual | English LM |
| **French** | Latin | Multi-Lingual | French LM |
| **German** | Latin | Multi-Lingual | German LM |
| **Indonesian** | Latin | Multi-Lingual | Indonesian LM |
| **Swahili** | Latin | Multi-Lingual | Swahili LM |
| **Kurdish** | Arabic / Latin | Mixed model | Kurdish LM |

### 13.2 Mixed-Language Document Handling

| Scenario | Detection Method | Processing Strategy |
|---|---|---|
| **Arabic + English** | Script detection at word level | Separate recognition per script, then merge in reading order |
| **Quran + Translation** | Page layout analysis (parallel text) | Recognize each column separately with language tag |
| **Arabic + Urdu** | Character set detection | Single model with dual lexicon |
| **Arabic + Persian** | Character set detection | Single model with Persian character extensions |
| **Classical + Modern** | Lexicon-based age classification | Dual language model selection |
| **Scholarly Transliteration** | Latin script with diacritics | Latin model with extended character set |

### 13.3 Arabic-Script Dialect Handling

| Dialect/Variant | Key Differences | OCR Adaptation |
|---|---|---|
| **Classical Arabic** | Quranic orthography, no modern punctuation | Quranic OCR + Classical Arabic LM |
| **Modern Standard Arabic** | Standard orthography, modern punctuation | Standard Arabic LM |
| **Persian** | Additional characters (گ, چ, پ, ژ) | Extended character set |
| **Urdu** | Additional characters (ٹ, ڈ, ڑ, etc.) | Extended character set + Nastaliq support |
| **Jawi (Malay)** | Modified Arabic alphabet for Malay | Jawi-specific lexicon |
| **Ottoman Turkish** | Persian-influenced orthography | Ottoman Turkish lexicon |

---

## 14. DIACRITIC RESTORATION

### 14.1 Diacritic Types

| Diacritic | Unicode | Name | Function |
|---|---|---|---|
| َ | U+064E | Fatha | /a/ vowel above letter |
| ُ | U+064F | Damma | /u/ vowel above letter |
| ِ | U+0650 | Kasra | /i/ vowel below letter |
| ً | U+064B | Fathatan | /an/ nunation above |
| ٌ | U+064C | Dammatan | /un/ nunation above |
| ٍ | U+064D | Kasratan | /in/ nunation below |
| ْ | U+0652 | Sukoon | No vowel (above) |
| ّ | U+0651 | Shaddah | Gemination (doubling) |
 | U+0653 | Maddah | Elongation (over alif) |
| ٰ | U+0670 | Superscript Alif | Short alif |
| ٖ | U+0656 | Subscript Alif | Rare alif below |

### 14.2 Diacritic Restoration Pipeline

| Stage | Method | Accuracy |
|---|---|---|
| **Detection** | High-resolution diacritic position analysis on original image | > 99% |
| **Classification** | CNN-based diacritic type classification | > 98% |
| **Placement Verification** | Validate diacritic positions relative to base letters | > 99% |
| **Contextual Correction** | Linguistic rule-based correction of impossible diacritic combinations | 100% rule enforcement |
| **Quranic Validation** | Compare against known Quranic diacritization | 100% for Quran |

### 14.3 Diacritic Restoration Models

| Model | Training Data | Best For | Accuracy |
|---|---|---|---|
| **Quranic Diacritic Model** | 100% diacritized Quran text (all qira'at) | Quran OCR | > 99.9% |
| **Classical Arabic Diacritic Model** | 10M+ diacritized classical Arabic words | Classical Islamic texts | > 95% |
| **Modern Arabic Diacritic Model** | 5M+ diacritized modern Arabic words | Contemporary books | > 93% |
| **Statistical Diacritic Model** | n-gram + morphological analysis | Unseen text, general purpose | > 90% |

### 14.4 Diacritic Confidence

Each diacritic carries a confidence score:

| Confidence | Meaning | Action |
|---|---|---|
| > 99% | Almost certain | Accept automatically |
| 95-99% | Very likely | Flag for automated review |
| 80-94% | Likely with alternatives | Flag for human review |
| < 80% | Uncertain | Mark as undiacritized in output |

---

## 15. CONFIDENCE SCORING

### 15.1 Multi-Level Confidence

| Level | Scope | Calculation |
|---|---|---|
| **Character** | Individual character | Direct model output softmax probability |
| **Word** | Single word | Weighted average of character confidences |
| **Line** | Single line of text | Average of word confidences |
| **Region** | Zoned region (paragraph, etc.) | Average of line confidences |
| **Page** | Entire page | Weighted average of region confidences |
| **Document** | Full document | Weighted average of page confidences |

### 15.2 Confidence Weighting Factors

| Factor | Weight | Rationale |
|---|---|---|
| **Recognition Score** | 0.40 | Direct model confidence |
| **Lexicon Match** | 0.15 | Word found in dictionary |
| **Language Model Score** | 0.15 | Contextual probability |
| **Diacritic Accuracy** | 0.10 | Diacritic verification score |
| **Structural Consistency** | 0.10 | Layout and structure match |
| **Cross-Reference Match** | 0.10 | Match against verified sources |

### 15.3 Confidence Thresholds

| Tier | Minimum Confidence | Verification Required |
|---|---|---|
| **Tier 1: Critical** | 99.5% | Full human verification |
| **Tier 2: Scholarly** | 98.0% | Automated + spot-check human |
| **Tier 3: Standard** | 95.0% | Automated only |
| **Tier 4: Reference** | 90.0% | Automated only |
| **Tier 5: Draft** | 80.0% | Flagged for future review |

### 15.4 Confidence Decay

OCR confidence naturally decays over time:

| Factor | Decay Rate | Mitigation |
|---|---|---|
| **Source Deterioration** | Minimal (digital copies don't degrade) | N/A |
| **Model Improvement** | Confidence may increase with model updates | Re-OCR when model improves by > 0.5% CER |
| **Verification Discovery** | New verification sources may reveal errors | Continuous re-verification |
| **Version Tracking** | Each OCR version tracked | Maintain full version history |

---

## 16. ERROR DETECTION AND CORRECTION

### 16.1 Error Taxonomy

| Error Type | Description | Frequency (Standard Text) | Frequency (Complex Text) |
|---|---|---|---|
| **Character Substitution** | Wrong character recognized | 0.3% | 2.0% |
| **Character Insertion** | Extra character added | 0.1% | 0.5% |
| **Character Deletion** | Character missed | 0.2% | 1.5% |
| **Word Boundary** | Incorrect word split/join | 0.5% | 3.0% |
| **Diacritic Error** | Missing/incorrect diacritic | 1.0% | 5.0% |
| **Line Merge/Split** | Lines incorrectly combined/separated | 0.1% | 2.0% |
| **Region Misclassification** | Wrong region type assigned | 0.5% | 3.0% |
| **Reading Order** | Incorrect text sequence | 0.1% | 1.0% |

### 16.2 Automatic Correction Methods

| Method | Application | Success Rate |
|---|---|---|
| **Edit Distance** | Minor character-level errors | > 90% |
| **Lexicon Lookup** | Word not in dictionary → find nearest match | > 85% |
| **Language Model** | Context-based correction | > 80% |
| **Confusion Set** | Common character confusions (س→ش, etc.) | > 95% |
| **Pattern Matching** | Known patterns (dates, numbers, names) | > 98% |
| **Quranic Database** | Exact match against digital Mushaf | 100% |

### 16.3 Error Logging and Analysis

| Logged Field | Description | Use |
|---|---|---|
| **Error Type** | Classification of the error | Trend analysis |
| **Original Text** | OCR output before correction | Debugging |
| **Corrected Text** | Text after correction | Audit trail |
| **Correction Method** | How the error was fixed | Method effectiveness analysis |
| **Confidence Before/After** | Confidence scores | Impact measurement |
| **Source Image Region** | Bounding box of error region | Model retraining data |
| **Timestamp** | When the correction was made | Temporal analysis |

---

## 17. INTEGRATION WITH KNOWLEDGE GRAPH

### 17.1 OCR-to-Knowledge Graph Pipeline

```
OCR Verified Text
    │
    ▼
┌──────────────────────────────────┐
│ 1. Entity Extraction             │
│    • Named entities (people,     │
│      places, books, concepts)    │
│    • Quran references            │
│    • Hadith references           │
│    • Dates and events            │
└──────────────────────────────────┘
    │
    ▼
┌──────────────────────────────────┐
│ 2. Relationship Mapping          │
│    • Author → Book               │
│    • Book → Subject              │
│    • Quote → Source              │
│    • Concept → Definition        │
└──────────────────────────────────┘
    │
    ▼
┌──────────────────────────────────┐
│ 3. Fact Formulation              │
│    • Structured fact extraction  │
│    • Context preservation        │
│    • Confidence propagation      │
└──────────────────────────────────┘
    │
    ▼
┌──────────────────────────────────┐
│ 4. Knowledge Graph Insertion     │
│    • Node creation               │
│    • Edge creation               │
│    • Provenance attachment       │
│    • Verification status set     │
└──────────────────────────────────┘
```

### 17.2 Entity Extraction Mapping

| Entity Type | Examples | Extraction Method |
|---|---|---|
| **Person (Scholar)** | ابن كثير, البخاري, مسلم | NER + Islamic scholar database |
| **Book Title** | صحيح البخاري, البداية والنهاية | Pattern + publication database |
| **Location** | مكة, المدينة, دمشق, بغداد | NER + geographic database |
| **Quran Verse** | سورة البقرة:٢٥٥ | Pattern matching (سورة X:Y) |
| **Hadith Reference** | رواه البخاري (رقم: ١) | Pattern matching |
| **Concept** | التوحيد, الإيمان, الفقه | Domain-specific terminology |
| **Date** | ٧٥٠ هـ, ١٣٤٥ م | Calendar pattern matching |
| **School of Thought** | حنفي, مالكي, شافعي, حنبلي | Named entity recognition |

### 17.3 Provenance Data

Each fact inserted into the Knowledge Graph from OCR carries:

| Provenance Field | Description | Example |
|---|---|---|
| **Source Document** | Original book identifier | IEP-BK-001234 |
| **Source Page** | Page number in original | ص ١٤٢ |
| **Line Number** | Line position on page | السطر ١٢ |
| **Region Type** | Text region classification | Body Text |
| **OCR Date** | When OCR was performed | 2026-07-07T14:30:00Z |
| **OCR Model Version** | Recognition model used | Arabic-CRNN v2.3 |
| **Confidence Score** | Overall confidence for this fact | 98.7% |
| **Verification History** | Links to verification records | IEP-VRF-008945 |

---

## 18. QUALITY METRICS

### 18.1 Key Performance Indicators

| Metric | Description | Target | Measurement Method |
|---|---|---|---|
| **Character Error Rate (CER)** | Percentage of incorrect characters | < 0.5% (Tier 1) | Comparison with ground truth |
| **Word Error Rate (WER)** | Percentage of incorrect words | < 1.0% (Tier 1) | Comparison with ground truth |
| **Diacritic Accuracy** | Percentage of correctly recognized/restored diacritics | > 99.5% (Quran) | Comparison with verified text |
| **Layout Accuracy** | Correct region detection and classification | > 98% | Manual audit |
| **Reading Order Accuracy** | Correct text sequence | > 99% | Manual audit |
| **Metadata Accuracy** | Correct book metadata extraction | > 97% | Cross-reference with catalog |
| **Structure Accuracy** | Correct identification of chapters, sections, etc. | > 98% | Manual audit |
| **Throughput** | Pages processed per hour | 1000 pages/hour (standard) | Pipeline monitoring |
| **First-Pass Accuracy** | Accuracy before post-processing correction | > 97% | Automated verification |
| **Human Review Rate** | Percentage of text requiring human review | < 5% (Tier 3) | Workflow monitoring |

### 18.2 Quality Gates

| Gate | Stage | Criteria | Action on Failure |
|---|---|---|---|
| **Gate 1** | After Acquisition | Valid format, sufficient resolution, no corruption | Reject or request re-scan |
| **Gate 2** | After Preprocessing | Image quality score > 80% | Re-process with different parameters |
| **Gate 3** | After Layout Analysis | Region detection confidence > 90% | Flag for manual layout correction |
| **Gate 4** | After Recognition | Document CER < 5% | Re-OCR with different model |
| **Gate 5** | After Post-Processing | Word-level lexicon match > 90% | Flag for human review |
| **Gate 6** | After Verification | Meets tier-specific confidence threshold | Route for additional review |
| **Gate 7** | Final Approval | All verification steps passed | Release to Knowledge Graph |

### 18.3 Continuous Monitoring

| Monitor | Frequency | Action on Degradation |
|---|---|---|
| **OCR Accuracy Per Model** | Daily | Retrain or rollback model |
| **Throughput Rate** | Hourly | Scale worker allocation |
| **Queue Length** | Real-time | Increase or decrease processing capacity |
| **Error Distribution** | Weekly | Focus retraining on problematic character classes |
| **Human Review Volume** | Daily | Adjust automated verification thresholds |
| **Diacritic Accuracy** | Weekly | Enhance diacritic models |

---

## 19. PERFORMANCE REQUIREMENTS

### 19.1 Processing Performance

| Metric | Standard | Critical | Manuscript |
|---|---|---|---|
| **Pages per Hour (per GPU)** | 1000 | 500 (higher accuracy) | 100 (specialized models) |
| **Time per Page (pipeline)** | 3.6 seconds | 7.2 seconds | 36 seconds |
| **GPU Memory Required** | 8 GB | 16 GB | 24 GB |
| **Storage per Page (processed)** | 5 MB (preprocessed image + metadata) | 10 MB (higher resolution) | 20 MB |
| **Storage per Page (raw)** | 25 MB (300 DPI TIFF) | 50 MB (600 DPI TIFF) | 100 MB (600 DPI) |

### 19.2 Infrastructure Requirements

| Component | Specification | Quantity |
|---|---|---|
| **GPU (Standard OCR)** | NVIDIA A10G or equivalent | 4 per processing cluster |
| **GPU (Critical OCR)** | NVIDIA A100 or equivalent | 2 dedicated |
| **CPU** | 16-core, 32-thread | 8 per cluster |
| **RAM** | 64 GB | 8 per cluster |
| **Storage (Hot)** | 10 TB NVMe SSD | OCR processing and queued documents |
| **Storage (Cold)** | 100 TB | Raw scan archive |
| **Network** | 10 GbE | Interconnect between workers |

### 19.3 Scalability

| Scale Level | Pages/Day | GPU Count | Throughput |
|---|---|---|---|
| **Development** | 1,000 | 1 | ~40 pages/hour |
| **Standard Production** | 25,000 | 4 | ~1,000 pages/hour |
| **High Volume** | 100,000 | 16 | ~4,000 pages/hour |
| **Mass Digitization** | 500,000 | 80 | ~20,000 pages/hour |

---

## 20. SECURITY AND PRIVACY

### 20.1 Data Security

| Security Measure | Implementation |
|---|---|
| **Encryption at Rest** | AES-256 for raw scans and processed text |
| **Encryption in Transit** | TLS 1.3 for all OCR pipeline communication |
| **Access Control** | Role-based access for OCR queues, results, and raw images |
| **Audit Logging** | All OCR operations logged with user, timestamp, and action |
| **Data Retention** | Raw scans retained for 5 years, processed text indefinitely |
| **Secure Deletion** | Cryptographic erasure for deleted documents |

### 20.2 Content Sensitivity

| Sensitivity Level | Examples | Handling Requirements |
|---|---|---|
| **Public** | Published books, general Islamic content | Standard OCR processing |
| **Internal** | Pre-publication manuscripts, draft translations | Restricted OCR queue, no external processing |
| **Sensitive** | Rare manuscripts, personal collections | Dedicated secure processing, NDAs for reviewers |
| **Highly Sensitive** | Unreleased scholarly editions | Air-gapped processing, named reviewers only |

### 20.3 DRM and Copyright

| Concern | Policy | Implementation |
|---|---|---|
| **Copyright Compliance** | Only process documents with verified rights | Copyright check before OCR |
| **Watermarking** | Digital watermark on processed text | Steganographic watermark for tracking |
| **Access Limitation** | Full text only available to authorized users | Document-level access controls |
| **Fair Use** | Quotes and excerpts for educational purposes | Usage tracking and attribution |

---

## 21. TESTING AND VALIDATION

### 21.1 OCR Test Suite

| Test Category | Description | Pass Criteria |
|---|---|---|
| **Standard Arabic** | 1,000 pages of varied printed Arabic texts | CER < 0.5% |
| **Quranic Text** | 100 pages of Quran from different Mus'haf prints | CER < 0.1%, 100% diacritic accuracy |
| **Hadith Text** | 500 pages from different Hadith collections | CER < 0.3%, chain structure 100% accurate |
| **Multi-Column Layout** | 200 pages of complex multi-column layouts | Region detection > 98% |
| **Mixed Language** | 200 pages of Arabic-English mixed content | Script detection > 99% |
| **Aged/Damaged** | 100 pages of aged or damaged documents | CER < 3% |
| **Handwritten** | 50 pages of handwritten manuscripts | CER < 5% |
| **Nastaliq Script** | 100 pages of Nastaliq text | CER < 2% |
| **Low Quality** | 100 pages of low-resolution or camera-captured text | CER < 3% |

### 21.2 Continuous Improvement Pipeline

```
OCR Production Data
    │
    ▼
┌──────────────────────────────┐
│ 1. Error Collection          │  Aggregate all detected and corrected errors
└──────────────────────────────┘
    │
    ▼
┌──────────────────────────────┐
│ 2. Error Classification      │  Categorize by type, font, page condition
└──────────────────────────────┘
    │
    ▼
┌──────────────────────────────┐
│ 3. Ground Truth Generation   │  Create verified training data from corrections
└──────────────────────────────┘
    │
    ▼
┌──────────────────────────────┐
│ 4. Model Fine-Tuning         │  Retrain models with new ground truth
└──────────────────────────────┘
    │
    ▼
┌──────────────────────────────┐
│ 5. A/B Testing               │  Compare new vs. existing model performance
└──────────────────────────────┘
    │
    ▼
┌──────────────────────────────┐
│ 6. Production Deployment     │  Roll out improved model
└──────────────────────────────┘
```

---

## 22. APPENDICES

### 22.1 Supporting Technology Stack

| Component | Technology | Purpose |
|---|---|---|
| **OCR Engine** | Tesseract 5.x + Custom DL Models | Base OCR processing |
| **Deep Learning Framework** | PyTorch 2.x | Model training and inference |
| **Image Processing** | OpenCV 4.x, PIL | Preprocessing and enhancement |
| **Layout Analysis** | LayoutLMv3, Detectron2 | Page structure detection |
| **Language Model** | Custom Arabic BERT, morphological analyzers | Post-processing and correction |
| **Queue Management** | RabbitMQ / Apache Kafka | Document queue orchestration |
| **Storage** | MinIO (S3-compatible) | Raw and processed document storage |
| **Database** | PostgreSQL | OCR metadata and status tracking |
| **Orchestration** | Kubernetes | Worker cluster management |

### 22.2 Document Processing Workflow Summary

| Step | Automated | Human | Time (Standard Page) |
|---|---|---|---|
| Document Acquisition | ✓ | | 0.1s |
| Image Preprocessing | ✓ | | 0.5s |
| Layout Analysis | ✓ | | 0.3s |
| Text Recognition | ✓ | | 1.5s |
| Post-Processing | ✓ | | 0.5s |
| Automated Verification | ✓ | | 0.3s |
| Human Review (if needed) | | ✓ | 30-60s |
| Knowledge Graph Integration | ✓ | | 0.2s |
| **Total (no human review)** | | | **3.4s** |
| **Total (with human review)** | | | **33.4-63.4s** |

### 22.3 Glossary

| Term | Definition |
|---|---|
| **CER** | Character Error Rate — percentage of incorrectly recognized characters |
| **WER** | Word Error Rate — percentage of incorrectly recognized words |
| **DPI** | Dots Per Inch — measure of scan resolution |
| **Tashkeel** | Arabic diacritical marks indicating short vowels |
| **I'jam** | Dots distinguishing Arabic letters (e.g., ب vs. ت) |
| **Sanad** | Chain of narrators in a Hadith |
| **Matn** | The text content of a Hadith |
| **Naskh** | Common Arabic calligraphic style used in printing |
| **Nastaliq** | Cursive Arabic script style used in Persian and Urdu |
| **Kashida** | Tatweel — elongation stroke in Arabic justification |
| **Uthmani Script** | Standard Quranic orthography based on the Uthmanic codex |
| **Qira'at** | Canonical methods of Quranic recitation |
| **Bounding Box** | Coordinates defining the position of detected text in an image |

### 22.4 Related Documents

| Document ID | Title | Relationship |
|---|---|---|
| IEP-CONST-001 | Project Constitution | Constitutional authority for OCR quality requirements |
| IEP-ARCH-DB-001 | Database Architecture | Storage for OCR metadata and processed text |
| IEP-ARCH-KG-001 | Knowledge Graph Architecture | Target for OCR output integration |
| IEP-ARCH-RAG-001 | RAG Architecture | Consumer of OCR-verified text |
| IEP-ARCH-VRF-001 | Verification Architecture | OCR verification layer integration |
| IEP-ARCH-AGENT-001 | Agent Architecture | Verification Agent roles in OCR pipeline |
| IEP-ARCH-ORCH-001 | Master Orchestrator | Pipeline orchestration and queue management |

### 22.5 Revision History

| Version | Date | Author | Changes |
|---|---|---|---|
| 1.0.0 | 2026-07-07 | Office of the Chief Architect | Initial approved release |
