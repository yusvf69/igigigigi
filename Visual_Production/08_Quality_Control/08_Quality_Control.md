# Quality Control Bible

**Document ID:** QC-BIBLE-001
**Version:** 1.0.0
**Last Updated:** 2026-07-10
**Classification:** Internal — Quality Control
**Owner:** Quality Control Lead
**Approved By:** Director of Visual Production

---

## Table of Contents

1. [QC Philosophy](#1-qc-philosophy)
2. [Educational Accuracy](#2-educational-accuracy)
3. [Historical Accuracy](#3-historical-accuracy)
4. [Source Verification](#4-source-verification)
5. [Visual Consistency](#5-visual-consistency)
6. [Accessibility](#6-accessibility)
7. [Brand Consistency](#7-brand-consistency)
8. [Performance](#8-performance)
9. [Optimization](#9-optimization)
10. [File Naming](#10-file-naming)
11. [Versioning](#11-versioning)
12. [Metadata](#12-metadata)
13. [Localization](#13-localization)
14. [QC Checklists](#14-qc-checklists)
15. [QC Automation](#15-qc-automation)
16. [QC Process](#16-qc-process)

---

## Foreword

This Quality Control Bible establishes the definitive quality standards, processes, and verification frameworks for all visual assets produced within the Visual Production pipeline. It is the single source of truth for quality-related decisions and serves as the contractual quality agreement between all production roles, reviewers, and stakeholders.

Adherence to this Bible is mandatory for every individual who creates, reviews, approves, or manages visual assets. Non-compliance with any standard defined herein must be documented, justified, and approved through the established exceptions process. The standards in this document are not guidelines but requirements.

### How to Use This Document
This document is organized into sixteen comprehensive sections. Each section addresses a distinct dimension of quality control. All team members must be familiar with the full document. Specialists should have deep knowledge of their specific areas. The QC checklists in Section 14 serve as the primary daily reference for hands-on quality verification.

### Key Terminology
| Term | Definition |
|------|------------|
| Asset | Any visual or audio file produced within the Visual Production pipeline |
| Asset Set | A group of related assets designed to work together (e.g., a character + all textures) |
| Blocker | A severity level indicating the asset cannot proceed to the next pipeline stage |
| Critical | A severity level indicating a significant issue requiring resolution before final approval |
| Pipeline | The sequential production workflow from concept to final asset delivery |
| QC Gate | A quality check point in the pipeline that assets must pass through |
| Waiver | Formal documented approval to accept a known non-critical issue |
| Re-verification | The process of confirming that previously identified issues have been resolved |

## 1. QC Philosophy

### 1 Foundational Principles

Quality is the responsibility of every individual contributor, reviewer, and stakeholder in the Visual Production pipeline. No asset reaches the platform without passing through the QC framework defined in this document. The QC philosophy rests on five pillars:

1. **Prevention Over Inspection** — Quality must be designed and built into every asset from the earliest conceptual stage. Rework at the end of the pipeline is exponentially more expensive than getting it right at the start. Each production stage includes built-in quality gates that catch issues before they propagate downstream.

2. **Educational Excellence** — Every asset must serve the platform's core mission: delivering world-class educational content. Visual assets are not merely decorative; they are pedagogical tools. Each asset must demonstrably support specific learning objectives and enhance the educational experience.

3. **Visual Beauty** — The platform competes with the visual standards of premium educational media. Every asset must exhibit craftsmanship, attention to detail, and aesthetic coherence. Beauty is not superficial; it signals care and builds trust with learners.

4. **Historical and Cultural Accuracy** — The platform serves a global audience with diverse cultural and historical backgrounds. Assets depicting historical periods, cultural practices, geographic regions, or religious contexts must be meticulously researched and verified.

5. **Technical Performance** — Educational impact is nullified if assets fail to load, render poorly, or perform inconsistently across devices. Every asset must meet defined technical budgets for file size, memory, draw calls, and frame rate across all target platforms.

### 2 Quality Definition

For the purposes of this document, a quality asset is defined as an asset that is educationally accurate and age-appropriate; historically and culturally accurate where applicable; visually consistent with the style guide; technically performant within defined budgets; fully accessible per WCAG 2.1 AA standards; correctly named, versioned, and documented; properly localized or localization-ready; and free of defects, artifacts, or errors.

### 3 Quality Maturity Model

| Level | Name | Description |
|-------|------|-------------|
| 0 | Ad Hoc | No formal QC; quality varies by artist |
| 1 | Awareness | Basic checklist exists; applied inconsistently |
| 2 | Defined | This QC Bible; standardized process with defined roles |
| 3 | Managed | Metrics collected; process measured and improved |
| 4 | Optimized | Continuous improvement; automated QC fully integrated |

### 4 Cost of Quality

| Quality Dimension | Cost if Caught Early | Cost if Caught Late | Multiplier |
|-------------------|---------------------|---------------------|------------|
| Conceptual error | 1x (concept stage) | 20x (post-production) | 20x |
| Historical inaccuracy | 1x (research stage) | 50x (published asset) | 50x |
| Visual inconsistency | 1x (rough draft) | 15x (final render) | 15x |
| Performance issue | 1x (prototype) | 30x (shipped asset) | 30x |
| Accessibility failure | 1x (wireframe) | 40x (audit finding) | 40x |
| Cultural insensitivity | 1x (concept review) | 100x (public backlash) | 100x |
| Educational error | 1x (content review) | 60x (curriculum impact) | 60x |

### 5 Zero-Defect Mentality

The QC team operates with a zero-defect mentality for educational, historical, and accessibility issues. Non-critical visual or performance issues may be accepted with documented justification and Lead approval. Blocker and Critical severity issues must be resolved before any asset proceeds to the next pipeline stage.

### 6 Quality Metrics and KPIs

| Metric | Target | Measurement Method |
|--------|--------|--------------------|
| First-pass yield | >= 85% | Automated QC + peer review pass rate |
| Defect density | <= 2 per asset | Issues found per QC review cycle |
| Blocker defects | 0 at final approval | Final QC gate verification |
| Re-review rate | <= 10% | Assets requiring >1 review cycle |
| Turnaround time per stage | <= 48 hours | Time from submission to sign-off |
| Customer-reported defects | <= 1 per 1000 assets | Post-release tracking system |
| QC coverage | 100% of assets | All assets pass through full QC workflow |

### 7 Relationship to Other Documents

This Quality Control Bible is the authoritative QC reference for Visual Production. It connects to:

- **00_Visual_Asset_Audit** — Defines the audit framework for assets requiring QC review
- **01_Visual_Production_Bible** — Overall production standards and workflow
- **02_Illustration_Bible** — Style and technical standards for illustrations
- **03_3D_Asset_Bible** — Style and technical standards for 3D assets
- **04_Animation_Bible** — Style and technical standards for animations
- **05_Video_Production_Bible** — Style and technical standards for video
- **06_Audio_Production_Bible** — Style and technical standards for audio
- **07_Asset_Pipeline** — The pipeline into which QC gates are integrated
- **09_AI_Asset_Generation** — QC for AI-generated assets
- **10_Asset_Library** — Destination for QC-approved assets

Where conflicts exist, this QC Bible takes precedence for quality-related decisions.

### 8 QC Team Structure

| Role | Responsibility | Reports To |
|------|---------------|------------|
| QC Lead | Overall QC process ownership, final sign-off, exceptions | Director of Visual Production |
| QC Specialist | Detailed asset review per checklists, re-verification | QC Lead |
| Educational Reviewer | Educational accuracy, curriculum alignment | QC Lead |
| Historical Reviewer | Historical accuracy, anachronism detection | QC Lead |
| Religious Reviewer | Religious content accuracy, scriptural alignment | QC Lead |
| Accessibility Reviewer | WCAG compliance, inclusive representation | QC Lead |
| Automation Engineer | QC tool development, CI/CD pipeline integration | QC Lead |
| Peer Reviewer | Visual quality, style consistency, composition | QC Specialist |

### 9 Quality Culture

Building a quality culture requires commitment at every level of the organization:
1. Leadership demonstrates commitment by allocating resources, recognizing quality achievements, and treating quality as a strategic priority
2. Production managers build quality into schedules, not as an afterthought but as an integral production phase with adequate time allocation
3. Artists take ownership of quality from concept through delivery and view QC feedback as learning opportunities, not criticism
4. QC reviewers serve as partners in quality, providing constructive actionable feedback with clear references to standards
5. Every team member has the authority and responsibility to raise quality concerns without fear of negative consequences

Quality recognition program: monthly QC champion award for the team member who demonstrates exceptional quality commitment. Quarterly quality showcase featuring outstanding assets and the practices that produced them.

### 10 Continuous Improvement

The QC process is continuously improved through:
1. Quarterly QC process review meetings with all team members
2. Defect pattern analysis to identify systemic pipeline issues
3. Root cause analysis for all Blocker and Critical defects
4. Annual QC Bible revision cycle incorporating lessons learned
5. Benchmarking against industry standards in educational content
6. Feedback collection from artists and producers on process efficiency
7. Automation opportunity identification in each quarterly review

## 2. Educational Accuracy

### 1 Educational Accuracy Framework

Every visual asset on the platform is an educational tool. Even purely decorative assets must not contradict or undermine educational content. This section defines the framework for verifying educational accuracy across all asset types.

The framework operates on four principles:
1. Truthfulness — All content must be factually correct and verifiable against authoritative sources
2. Appropriateness — Content must match the cognitive and developmental level of the target learner
3. Clarity — Educational content must be communicated clearly without ambiguity or misleading simplification
4. Completeness — Assets must present sufficient context for accurate understanding

### 2 Learning Objective Mapping

| Field | Description | Required |
|-------|-------------|----------|
| Learning Objective ID | Unique identifier from the curriculum database | Yes |
| Learning Objective Text | Full text of the educational objective | Yes |
| Subject Domain | Mathematics, Science, History, Islamic Studies, Geography, Art | Yes |
| Grade Level | KG through Grade 12, or Adult Learner | Yes |
| Cognitive Level | Bloom's Taxonomy: Remember, Understand, Apply, Analyze, Evaluate, Create | Yes |
| Asset Contribution Type | How the asset serves the objective: explain, illustrate, demonstrate | Yes |

Every asset must be mapped to at least one learning objective. Unmappable assets require Educational Content Lead review.

### 3 Bloom's Taxonomy Alignment

| Level | Description | Example Assets |
|-------|-------------|----------------|
| Remember | Recall facts | Labeled diagrams, flashcards, timelines |
| Understand | Explain ideas | Explanatory illustrations, concept maps |
| Apply | Use in new situations | Procedural animations, interactive diagrams |
| Analyze | Draw connections | Comparison charts, cause-effect diagrams |
| Evaluate | Justify decisions | Evidence-based infographics, pro-con visuals |
| Create | Produce original work | Synthesis diagrams, compositional assets |

### 4 Age-Appropriateness Validation

| Age Range | Content Complexity | Visual Complexity | Abstraction Level |
|-----------|-------------------|-------------------|-------------------|
| 5-7 years | Single concept per asset | Simple shapes, bright primary colors | Concrete only |
| 8-10 years | 1-2 related concepts | Moderate detail, richer palette | Mostly concrete; simple analogies |
| 11-13 years | 2-3 interconnected concepts | Detailed illustrations, nuanced colors | Some abstraction; metaphors OK |
| 14-16 years | Complex multi-step concepts | Full detail, sophisticated lighting | Abstract concepts accepted |
| 17+ years | Advanced nuanced concepts | Professional-grade fidelity | Full abstraction |

### 5 Cognitive Load Assessment

| Load Type | Definition | Design Guidelines |
|-----------|------------|------------------|
| Intrinsic Load | Inherent complexity of the subject | Break into segments; one concept per asset; simple to complex sequence |
| Extraneous Load | Poor design causing extra effort | Eliminate decorative distractions; consistent conventions; avoid split attention |
| Germane Load | Productive effort building schemas | Use signaling; worked examples; scaffolded complexity; varied practice |

Techniques: segment complex information, use signaling, eliminate redundancy, place text near visuals, use conversational language, allow learner control over pace, pre-train prerequisites.

### 6 Misinformation Prevention

| Check | Description | Severity |
|-------|-------------|----------|
| Factual accuracy | All claims verified against authoritative sources | Blocker |
| Outdated info | Content current per latest research | Critical |
| Misleading framing | Presentation implying incorrect conclusions | Blocker |
| Oversimplification | Simplified appropriately while maintaining truth | Major |
| False equivalency | Unequal views presented as equal | Blocker |
| Cherry-picked data | Selective evidence distorting truth | Critical |
| Conspiracy content | Promotion of debunked theories | Blocker |
| Pseudoscience | Non-scientific claims as science | Blocker |

### 7 Knowledge Node Accuracy

1. Verify asset's knowledge node ID matches the intended concept
2. Confirm prerequisite knowledge is correctly assumed
3. Check connections to related knowledge nodes are accurate
4. Validate no incorrect relationships between concepts
5. Ensure correct position in the learning sequence
6. Verify asset does not contradict other assets for the same node

### 8 Citation Format Compliance

| Citation Element | Requirement |
|-----------------|-------------|
| Author name(s) | Full names as they appear in the source |
| Publication title | Complete title with subtitle |
| Publisher | Full publisher name |
| Publication date | Year; month/day for periodicals |
| Page numbers | Specific pages for direct claims |
| DOI or ISBN | Where available |
| URL and access date | For online sources |
| Edition/volume | For multi-edition works |

Styles: Academic = APA 7th; Historical = Chicago Notes-Bibliography; Religious = specialized per tradition.

### 9 Curriculum Alignment Check

| Dimension | Verification Method |
|-----------|---------------------|
| National curriculum standards | Cross-reference with published curriculum documents |
| Learning outcome mapping | Verify asset supports published outcomes |
| Scope and sequence position | Confirm correct point in learning sequence |
| Cross-curricular connections | Verify connections to other subject domains |
| Assessment alignment | Asset supports assessment of the objective |

### 10 Fact-Checking Protocol

Level 1: Self Fact-Check (Artist) — Source every claim, document sources, flag uncertainties.
Level 2: Peer Fact-Check (Peer Reviewer) — Verify sources, spot-check claims against sources.
Level 3: Expert Fact-Check (Subject Matter Expert) — Review accuracy, nuance, source authority.
Level 4: Automated Fact-Check (Where Available) — Run automated tools, verify numbers, flag for review.

### 11 Controversial Content Protocol

1. Present consensus as the primary position
2. Acknowledge minority views where educationally relevant
3. Frame disagreements as scholarly interpretation, not opinion
4. Avoid fringe theories as equivalent to established scholarship
5. Indicate when a view is contested and by whom
6. Provide references for all presented positions
7. Review by multiple subject matter experts

### 12 Expert Educational Review

| Asset Type | Reviewer Credentials |
|------------|---------------------|
| Core curriculum illustrations | Licensed curriculum specialist |
| Scientific diagrams | PhD in relevant discipline |
| Historical scene illustrations | MA or PhD in history (specialized) |
| Mathematical visualizations | MA or PhD in mathematics education |
| Islamic studies content | Accredited Islamic scholar with ijazah |
| Language learning visuals | Licensed language educator |

## 3. Historical Accuracy

### 1 Historical Accuracy Framework

Assets depicting historical content must be meticulously researched. Historical inaccuracies undermine credibility and cause reputational damage.

### 2 Period Identification

| Field | Description | Format |
|-------|-------------|--------|
| Century | Primary century of depiction | 7th century CE |
| Specific Year | Exact year where known | 622 CE |
| Era | Broader classification | Early Islamic, Abbasid, Ottoman |
| Dynasty/Ruling Power | Governing authority | Umayyad, Abbasid, Fatimid, Mamluk |
| Calendar System | Primary calendar | Hijri, Gregorian, or dual |

### 3 Geographic Accuracy

| Element | Verification | Sources |
|---------|-------------|---------|
| Region boundaries | Historical borders, not modern | Historical atlases |
| Place names | Historical names with modern equivalent | Geography references |
| Topography | Accurate terrain features | Physical geography data |
| Political divisions | Administrative divisions for era | Administrative history |
| Important locations | Key sites correctly positioned | Archaeological surveys |
| Trade routes | Accurate positioning | Economic history references |

### 4 Architecture Style per Period and Region

| Period | Region | Key Features | Materials |
|--------|--------|-------------|-----------|
| Pre-Islamic Arabia | Peninsula | Mud-brick, stone buildings, tents | Mud brick, stone, palm |
| Early Islamic 7th-8th c. | Arabia/Levant | Hypostyle halls, simple minarets | Mud brick, stone, wood |
| Umayyad 661-750 | Syria/Spain | Mosaics, horseshoe arches, palaces | Stone, marble, glass mosaic |
| Abbasid 750-1258 | Iraq/Persia | Muqarnas, iwans, Round City | Brick, stucco, tile |
| Fatimid 909-1171 | Egypt | Keel arches, monumental gates | Stone, stucco, wood |
| Mamluk 1250-1517 | Egypt/Syria | Stone domes, ablaq stonework | Stone, marble, stained glass |
| Ottoman 1299-1922 | Turkey/Balkans | Central domes, pencil minarets | Stone, marble, Iznik tile |
| Mughal 1526-1857 | India/Pakistan | Bulbous domes, charbagh gardens | Sandstone, marble, inlay |
| Safavid 1501-1736 | Iran | Blue tilework, pointed arches | Brick, ceramic tile |
| West African Medieval | Mali/Songhai | Sudano-Sahelian adobe, wood beams | Adobe, wood |
| Swahili Coast Medieval | E. Africa | Coral stone, carved doors | Coral stone, mangrove |

### 5 Clothing Accuracy

| Element | Verification Points | Sources |
|---------|---------------------|---------|
| Silhouette | Matches period imagery and archaeology | Period art, museum collections |
| Fabrics | Materials available: linen, wool, cotton, silk | Textile archaeology |
| Colors | Natural dyes; no synthetic before 1856 | Dye history |
| Headwear | Turbans, veils, caps match period/status | Period art, written descriptions |
| Footwear | Region and period appropriate | Archaeological finds |
| Jewelry | Materials and styles available | Museum collections |
| Social class markers | Distinctions between all classes | Sumptuary laws, records |
| Occupational clothing | Trade-specific garments | Guild records, art |

### 6 Object Accuracy

| Category | Verification | Sources |
|----------|-------------|---------|
| Tools/implements | Correct design, materials per period | Archaeology, museums |
| Weapons/armor | Style, materials period-appropriate | Arms collections, period art |
| Books/writing | Codex vs scroll; materials, script | Codicology references |
| Furniture | Available designs, materials | Period art, archaeology |
| Lighting | Oil lamps, candles; fuel types | Lamps finds, records |
| Musical instruments | Types available per period/region | Music archaeology, art |
| Coins/currency | Correct designs, metals, denominations | Numismatic collections |
| Transport | Camels, horses, ships — period tack | Transport history |
| Scientific instruments | Astrolabes, quadrants, globes | History of science, museums |

### 7 Anachronism Checklist

Check every historically-themed asset for:
- [ ] No modern materials: plastic, synthetic fabrics, modern steel
- [ ] No modern infrastructure: paved roads, electrical lines, streetlights
- [ ] No modern vehicles: cars, trucks, bicycles, aircraft
- [ ] No modern communication: phones, radios, televisions
- [ ] No buildings from wrong historical periods
- [ ] No modern writing instruments: ballpoint pens, markers
- [ ] No modern typography or fonts in scene elements
- [ ] No modern hairstyles or grooming
- [ ] No anachronistic color combinations from synthetic dyes
- [ ] No post-Columbian Exchange items before 1492

### 8 Expert Historical Review

| Depth | Reviewer | Required |
|-------|----------|----------|
| Background setting | In-house historian | Yes |
| Specific event/figure | External specialist | Yes |
| Complex period depiction | External historian | Yes |
| Religious historical content | Scholar + historian | Yes |
| Controversial interpretation | Multiple experts (2+) | Yes |

### 9 Source Citation for Historical Claims

| Claim Type | Minimum Sources | Format |
|------------|----------------|--------|
| Well-established fact | 2 authoritative sources | Standard citation |
| Date or event | 1 primary + 1 scholarly secondary | Chicago NB |
| Interpretation | 1 scholarly secondary | APA or Chicago |
| Scholarly consensus | 1 review article | Standard citation |
| Minority view | Source + context note | Attribution format |

## 4. Source Verification

### 1 Source Credibility Assessment CRAAP Test

| Criterion | Questions | Score |
|-----------|-----------|-------|
| Currency | When published? Updated? Current? | 1-5 |
| Relevance | Directly addresses topic? Audience? | 1-5 |
| Authority | Author credentials? Institution? | 1-5 |
| Accuracy | Evidence? Verifiable? Peer-reviewed? | 1-5 |
| Purpose | Intent? Bias? Objective? | 1-5 |

Scores below 15/25 require verification; below 10/25 prohibited.

### 2 Source Hierarchy

| Tier | Types | Acceptability |
|------|-------|---------------|
| 1 | Peer-reviewed journals, reference works, primary sources, curriculum docs | Always accepted |
| 2 | Scholarly monographs, university press, museum catalogs, government pubs | Always accepted |
| 3 | Trade publications, quality journalism, expert documentaries | With verification |
| 4 | Popular media, general books, educational websites | General context only |
| 5 | Social media, blogs, forums, non-specialist | Prohibited |

### 3 Primary vs Secondary Sources

Primary = direct evidence from the period (manuscripts, artifacts, coins, inscriptions, contemporary chronicles, official documents, architectural remains). Secondary = analysis of primary sources (journal articles, history books, encyclopedias).

Historical visual details must trace to primary sources. Secondary sources must cite their provenance. Conflicts: closest contemporary primary takes precedence.

### 4 Academic vs Popular Sources

| Dimension | Academic | Popular |
|-----------|----------|---------|
| Authorship | Scholars, institutional affiliation | Journalists, writers |
| Review | Peer-reviewed | Editorial review |
| Citations | Extensive footnotes | Minimal or none |
| Language | Technical terminology | General audience |
| Publisher | University press, scholarly assoc. | Commercial publisher |
| Purpose | Advance scholarly knowledge | Inform/entertain |

### 5 Scholarly Consensus Check

Before finalizing historically-themed assets:
1. Consult review articles from last 10 years
2. Check multiple academic sources for convergence
3. Identify active scholarly debates relevant to the depiction
4. Determine if consensus shifted since previous version
5. Document consensus status in asset metadata
6. For contested topics, consult minimum 3 recent scholarly works

### 6 Minority Scholarly Views

1. Present consensus as primary position
2. Present minority views as alternatives, not equals
3. Explain why minority view is not consensus
4. Attribute views to specific scholars or schools
5. No platform for fringe views without academic support
6. Review by multiple experts for balanced treatment

### 7 Bibliography Standards

1. List all sources consulted, not only cited ones
2. Organize by type: primary, secondary, reference
3. Separate academic and popular sections
4. Include original language for non-English works
5. Note edition for frequently updated works
6. Provide access notes for paywalled sources

### 8 Timestamp for Evolving Knowledge

| Frequency | Topics | Fields |
|-----------|--------|--------|
| Quarterly | Emerging tech, current events | last_reviewed, next_review |
| Annually | Science, medicine, geography | last_reviewed, next_review |
| Per cycle | Established history, math, language | last_reviewed |
| On event | Current events, discoveries | last_reviewed, status flag |

Overdue review assets are flagged and queued for re-verification.

## 5. Visual Consistency

### 1 Style Guide Compliance

- [ ] Art style matches designated style (flat, semi-realistic, 3D, etc.)
- [ ] Line weight/style consistent with guide
- [ ] Shading style matches (flat, cel, gradient, PBR)
- [ ] Texture style matches (none, painted, photoreal)
- [ ] LOD matches importance tier specification
- [ ] Rendering style consistent (vector, raster, 3D)
- [ ] Artistic liberties documented and approved

### 2 Color Palette Adherence

| Check | Description | Method |
|-------|-------------|--------|
| Primary palette | Approved brand colors only | Color picker sampling |
| Secondary palette | Per style guide specification | Color picker |
| Accent colors | Sparingly and per guidelines | Visual review |
| Skin tones | Approved range for figures | Color picker |
| Nature colors | Approved ranges for terrain, sky, water | Color picker |
| Cultural meaning | Appropriate per audience region | Cultural reference |
| Color harmony | Color theory principles | Peer review |
| Saturation/brightness | Within limits defined in guide | Measurement |

### 3 Typography Standards

| Check | Severity |
|-------|----------|
| Font family: approved typefaces, licensed | Critical |
| Font weight: only defined weights | Major |
| Font size: matches typographic scale | Major |
| Line height: follows vertical rhythm | Major |
| Letter spacing: tracking per specs | Minor |
| Text alignment: follows reading direction (LTR/RTL) | Critical |
| Text hierarchy: correct heading/body/caption | Major |
| Text contrast: WCAG 2.1 AA | Critical |
| Language support: all characters | Critical |
| RTL rendering: right aligned, correct glyphs | Critical |
| Text overflow: no cutoff or overlap | Major |

### 4 Character Proportion Consistency

| Metric | Tolerance |
|--------|-----------|
| Head-to-body ratio | +/- 0.25 heads |
| Shoulder width | +/- 0.1 body width |
| Eye position | +/- 5% head height |
| Eye spacing | +/- 1 eye width |
| Limb length ratio | +/- 5% |
| Hand size vs face | +/- 10% |
| Foot size vs body | +/- 10% |

### 5 Perspective and Composition

| Check | Requirement |
|-------|-------------|
| Vanishing points | Correct convergence |
| Horizon line | Consistent across scene |
| Foreshortening | Correctly applied |
| Object scaling | Proportional to distance |
| Overlap | Correct depth layering |
| Atmospheric perspective | Correct color/value shift |
| Camera angle | Consistent with brief |
| Rule of thirds | Guideline, not rule |
| Leading lines | Direct eye flow |
| Negative space | No awkward cropping |

### 6 Lighting Direction Consistency

| Check | Description |
|-------|-------------|
| Primary light source | Single consistent direction |
| Secondary light | Consistent fill/rim |
| Shadow direction | All same direction |
| Shadow type | Consistent hard/soft |
| Highlights | Consistent with source |
| Ambient light | Consistent level/color |
| Color temperature | Warm, cool, or neutral consistent |
| Reflections | Consistent environment reflection |

### 7 Level of Detail Consistency

| Importance | LOD Standard | Tolerance |
|------------|-------------|-----------|
| Hero characters | High: full detail, textures, shadows | +/- 0.5 LOD |
| Supporting characters | Medium-high: simplified textures | +/- 0.5 LOD |
| Background characters | Low: silhouette, minimal detail | +/- 0.5 LOD |
| Hero props | High: full texture, all details | +/- 0.5 LOD |
| Environmental props | Medium: correct shape, simple texture | +/- 0.5 LOD |
| Background elements | Low: color-blocked shapes | +/- 0.5 LOD |
| UI elements | Ultra-consistent across all screens | +/- 0.25 LOD |

### 8 Consistency Verification Process

1. Reference set creation from approved assets
2. Side-by-side comparison on calibrated monitor
3. Grid overlay for alignment and proportion
4. Color sampling across all assets in set
5. Overlay comparison on reference poses
6. Export settings verification (resolution, format, color space)
7. Lighting diagram for multi-asset scenes

## 6. Accessibility

### 1 WCAG 2.1 AA Compliance by Asset Type

| Type | Criteria | Priority |
|------|----------|----------|
| Illustrations | 1.1.1, 1.4.1, 1.4.3, 1.4.11 | High |
| 3D Models | 1.1.1, 1.3.1, 4.1.2 | High |
| Animations | 2.2.2, 2.3.1 | Critical |
| Video | 1.2.2, 1.2.4, 1.2.5 | Critical |
| Audio | 1.1.1, 1.2.1 | High |
| Icons | 1.1.1, 1.4.1, 1.4.11 | High |
| UI Assets | 1.4.3, 1.4.11, 2.5.5, 1.3.4 | Critical |
| Infographics | 1.1.1, 1.3.1, 1.4.3 | High |

### 2 Color Contrast Minimums

| Requirement | Ratio | Applies To |
|-------------|-------|------------|
| Normal text | 4.5:1 | Body, labels, captions < 18pt |
| Large text | 3:1 | Headings, text >= 18pt or 14pt bold |
| UI components | 3:1 | Buttons, forms, focus indicators |
| Graphical objects | 3:1 | Icons, charts, infographic elements |
| Text on gradients | 4.5:1 | Worst-case contrast point |
| Text on images | 4.5:1 | Average and minimum regions |

### 3 Alt Text Quality

| Level | Description | Acceptable |
|-------|-------------|------------|
| Excellent | Concise, contextually appropriate, educational | Yes |
| Good | Accurate, slightly verbose but complete | Yes |
| Minimal | Present but lacks educational context | Minor |
| Generic | Just 'Image' or 'Picture' — no description | Blocker |
| Missing | No alt attribute on non-decorative | Blocker |
| Incorrect | Describes inaccurately | Blocker |
| AI unverified | From AI without human review | Must be reviewed |

### 4 Screen Reader Compatibility

| Check | Severity |
|-------|----------|
| Heading hierarchy (no skips) | Critical |
| ARIA labels on interactive elements | Critical |
| Tab order follows visual order | Critical |
| Visible focus indicators | Major |
| Form labels associated | Critical |
| Errors programmatically identifiable | Major |
| Keyboard navigation | Critical |
| Skip navigation link | Major |

### 5 Color Blind Safety

| Type | Affected | Concern |
|------|----------|---------|
| Deuteranopia | ~6% males | Red/green confusion |
| Protanopia | ~2% males | Red/green; red looks darker |
| Tritanopia | <1% | Blue/yellow confusion |
| Achromatopsia | <0.01% | No color perception |

Guidelines: don't rely on color alone, avoid red/green combos for critical info, ensure luminance contrast, test with simulators, use color blind-friendly palettes.

### 6 Motion Reduction

| Criterion | Implementation |
|-----------|----------------|
| Pause/stop/hide | Play/pause controls, stop on interaction |
| Three flashes | No content flashes > 3x per second |
| prefers-reduced-motion | Respect OS accessibility setting |
| Static alternatives | Provide for all animations |

### 7 Inclusive Representation

| Dimension | Guidelines |
|-----------|------------|
| Gender | Balanced, avoid stereotypes |
| Skin tone | Diverse range |
| Ability | Include disabilities naturally |
| Age | Diverse age groups |
| Body type | Range of body types |
| Cultural attire | Accurate and respectful |
| Socioeconomic | Various backgrounds |

## 7. Brand Consistency

### 1 Brand Identity Compliance

Every asset must be consistent with the platform's brand identity. Brand consistency builds trust and recognition.

### 2 Logo Usage Rules

| Rule | Specification |
|------|--------------|
| Clear space | Height of logo on all sides |
| Minimum size | Full: 24px height; icon: 16px |
| Background contrast | Sufficient contrast |
| Aspect ratio | Locked; no stretching |
| Color variant | Correct for background |
| Prohibited | No effects, shadows, gradients, outlines, rotation |

### 3 Color System Adherence

| Role | Tolerance |
|------|-----------|
| Primary brand | Exact match required |
| Secondary brand | Exact match required |
| Neutrals | +/- 2% luminance |
| Functional | Exact match for semantic meaning |
| Accent | Within approved accent palette |

### 4 Design Token Compliance

All values from design token system: color (hex/RGB), spacing (4/8/12/16/24/32/40/48/64/80/96px), typography (family/weight/size/line-height/letter-spacing), shadow (elevation 0-5), radius (none/sm/md/lg/full), animation (duration/easing), opacity (disabled/hint/overlay/solid).

### 5 Brand Personality Reflection

Trustworthy: accurate, consistent. Educational: clear, pedagogical. Innovative: modern, engaging. Respectful: sensitive, inclusive. Beautiful: aesthetic, detailed. Clear: uncluttered, logical.

## 8. Performance

### 1 File Size Budgets

| Type | Target | Maximum |
|------|--------|---------|
| Illustration hero | 200 KB | 500 KB |
| Illustration supporting | 100 KB | 250 KB |
| Illustration thumbnail | 25 KB | 50 KB |
| 3D model hero | 2 MB | 5 MB |
| 3D model supporting | 500 KB | 1.5 MB |
| 3D model simple | 100 KB | 300 KB |
| 2D animation 30s | 500 KB | 1 MB |
| 3D animation 30s | 2 MB | 5 MB |
| Video HD 60s | 5 MB | 10 MB |
| Video 4K 60s | 15 MB | 25 MB |
| Audio speech 60s | 500 KB | 1 MB |
| Audio music 60s | 1 MB | 2 MB |
| Icon | 2 KB | 5 KB |
| UI asset | 10 KB | 30 KB |
| Sprite sheet | 500 KB | 1 MB |

### 2 Load Time Targets

| Type | Target | Max (3G) |
|------|--------|----------|
| Illustration | < 500ms | < 2s |
| 3D model | < 2s | < 5s |
| Animation | < 1s | < 3s |
| Video start | < 3s | < 5s |
| Audio start | < 1s | < 3s |
| Icon | < 100ms | < 500ms |
| Full page | < 3s | < 5s |

### 3 Draw Call Budgets

| Platform | Per Frame | Per Scene |
|----------|-----------|-----------|
| Low-end mobile | 50 | 100 |
| Mid-range mobile | 100 | 200 |
| High-end mobile | 150 | 300 |
| Tablet | 150 | 300 |
| Desktop standard | 200 | 400 |
| Desktop high-end | 300 | 600 |

### 4 Texture Resolution

| Importance | Max | Mobile Max |
|------------|-----|------------|
| Hero character | 2048x2048 | 1024x1024 |
| Supporting character | 1024x1024 | 512x512 |
| Background character | 512x512 | 256x256 |
| Hero prop | 2048x2048 | 1024x1024 |
| Environmental prop | 1024x1024 | 512x512 |
| Terrain | 2048x2048 | 1024x1024 |
| UI texture | 512x512 | 256x256 |

### 5 Animation Performance

| Metric | Target | Max |
|--------|--------|-----|
| Frame rate | 60 fps | 30 fps |
| Frame time | 16.67ms | 33.33ms |
| Simultaneous animations | 10 | 20 |
| Bones per character | 30 | 50 |
| Blend shapes per mesh | 20 | 50 |
| Keyframes per animation | 100 | 300 |

### 6 Mobile-Specific Checks

- GPU overdraw < 3x
- Peak memory < 200 MB on low-end
- Background loading, non-blocking UI
- Progressive loading: critical first
- Network resilience with retry logic
- Low-power mode compatibility

## 9. Optimization

### 1 Compression Ratio Targets

| Type | Format | Target Ratio | Max Ratio |
|------|--------|-------------|-----------|
| Photo | WebP | 20:1 | 30:1 |
| Photo | AVIF | 25:1 | 35:1 |
| Flat illustration | WebP lossless | 30:1 | 50:1 |
| Gradient illustration | WebP lossy | 15:1 | 25:1 |
| Vector | SVG | 100:1+ | — |
| 3D + textures | glTF+KTX2 | 5:1 | 10:1 |
| 3D no textures | glTF Draco | 10:1 | 20:1 |
| Video | H.265 | 50:1 | 100:1 |
| Vector anim | Lottie | 10:1 | 20:1 |
| Audio speech | Opus | 10:1 | 15:1 |
| Audio music | AAC | 8:1 | 12:1 |

### 2 Format Selection

| Content | Primary | Alt | Fallback |
|---------|---------|-----|----------|
| Photo web | AVIF | WebP | JPEG |
| Photo print | TIFF | PNG | — |
| Flat illustration | SVG | WebP | PNG |
| Complex illustration | WebP | PNG | JPEG |
| 3D web | glTF GLB | glTF JSON | — |
| 2D vector anim | Lottie | WebP anim | GIF |
| 2D raster anim | WebP anim | MP4 H.265 | MP4 H.264 |
| Audio speech | Opus | MP3 | AAC |
| Audio music | AAC | Opus | MP3 |

### 3 3D Poly Count Compliance

| Type | Target Triangles | Max |
|------|-----------------|-----|
| Hero character | 5,000-15,000 | 25,000 |
| Supporting character | 2,000-5,000 | 10,000 |
| Background character | 500-1,500 | 2,500 |
| Hero prop | 1,000-5,000 | 10,000 |
| Environmental prop | 200-1,000 | 2,000 |
| Hero building | 5,000-15,000 | 25,000 |
| Background building | 500-2,000 | 5,000 |
| Terrain tile | 500-2,000 | 5,000 |
| Hero vegetation | 500-2,000 | 3,000 |
| Background vegetation | 50-200 | 500 |

### 4 Audio Bitrate Optimization

| Type | Format | Target | Min | Sample Rate |
|------|--------|--------|-----|-------------|
| Speech narration | Opus | 64 kbps | 48 kbps | 48 kHz |
| Speech dialogue | Opus | 96 kbps | 64 kbps | 48 kHz |
| Background music | AAC | 128 kbps | 96 kbps | 44.1 kHz |
| Featured music | AAC | 192 kbps | 128 kbps | 44.1 kHz |
| Sound effects | Opus | 64 kbps | 48 kbps | 48 kHz |
| Quranic recitation | AAC | 192 kbps | 128 kbps | 48 kHz |

## 10. File Naming

### 1 Naming Convention Format

[Asset Type Prefix]_[Category]_[Subcategory]_[Variant]_[Language]_v[Version]_[Resolution]_[Format]

Example: ILL_SCENE_MOSQUE_INTERIOR_DAY_AR_v2.1.3_3840x2160_WEBP

### 2 Asset Type Prefixes

ILL=Illustration, 3DM=3D Model, ANIM=Animation, VID=Video, AUD=Audio, ICN=Icon, UI=UI Asset, SPR=Sprite, TEX=Texture, MAT=Material, FNT=Font, TPL=Template, DOC=Document, RIG=Rig, SKN=Skin/Mesh

### 3 Category and Subcategory

CHAR=Character, SCENE=Scene/Background, PROP=Prop, UI=User Interface, VFX=Visual Effect, TXT=Text, MAP=Map, DIA=Diagram, INF=Infographic, DEC=Decorative, ANM=Animation, MSH=3D Mesh

### 4 Language Codes

AR=Arabic, EN=English, AR_EN=Bilingual, FR=French, UR=Urdu, TR=Turkish, ID=Indonesian, FA=Farsi, NEU=Language-neutral, MLT=Multiple languages

### 5 Version Format

vMAJOR.MINOR.PATCH — e.g. v1.0.0, v2.5.3
- MAJOR: conceptual change, redesign, educational change
- MINOR: detail revision, addition, refinement
- PATCH: optimization, bug fix, format change

### 6 Resolution Format

WIDTHxHEIGHT for raster (e.g., 3840x2160), VECTOR for SVG, NA for non-visual

### 7 Automated Naming Verification

1. Correct segment count
2. Valid prefix from approved list
3. Valid category/subcategory
4. Valid language code
5. Version matches X.Y.Z format
6. Resolution matches WxH for raster
7. Format label matches actual file format (magic byte check)
8. No spaces or special characters (except underscore, hyphen, dot)
9. No consecutive underscores
10. Max 255 characters total path length

### 8 Naming Exceptions

Only for: legacy assets, third-party assets, temporary working files. Requires QC Lead approval and exception registry logging.

## 11. Versioning

### 1 Version Numbering: X.Y.Z

vMAJOR.MINOR.PATCH where each component is a non-negative integer.

### 2 Major Version Triggers

Complete redesign, educational content change, major historical correction, style guide change, technology shift, accessibility overhaul. Requires full QC re-review through all pipeline stages.

### 3 Minor Version Triggers

Detail revision, element addition/removal, style refinement, content expansion, new language variants, performance improvements. Requires targeted QC review of affected areas.

### 4 Patch Version Triggers

File size reduction, artifact correction, re-export, metadata update, accessibility fix, minor technical fix. Requires automated QC + visual spot-check only.

### 5 Changelog Requirements

Each entry: version, date, author, change type (MAJOR/MINOR/PATCH), change description, reason, files affected, QC approval, related references.

### 6 Version in File and Metadata

Version must appear in both filename (vX.Y.Z) and metadata (version: X.Y.Z). Must match exactly. Mismatch = Critical severity.

### 7 Version Lifecycle

v0.1.0 (concept) → v0.5.0 (draft) → v0.9.0 (pre-release) → v1.0.0 (first release) → v1.1.0 (minor) → v1.1.1 (patch) → v2.0.0 (major revision)

## 12. Metadata

### 1 Required Fields

asset_id, name, type, category, subcategory, description, keywords_ar, keywords_en, version, author, date_created, date_modified, source, license, educational_level, learning_objective, language, accessibility_notes, file_format, resolution, file_size_bytes, color_space, usage_rights, qc_status, qc_approver, qc_date, review_history

### 2 Asset ID Format

[PREFIX]-[YYYYMMDD]-[SEQUENCE]-[CHECKSUM]
Example: ILL-20260710-0001-A3

### 3 Metadata Format (YAML)

Embedded YAML or companion .meta file with all required fields. Automated validation checks field presence, types, enum values, date formats, version format, asset ID checksum, description length, and keyword count.

### 4 Automated Metadata Extraction

file_format (magic bytes), file_size_bytes (system stat), resolution (image header), color_space (ICC profile), duration (media header), frame_rate (media header), bitrate (media header), poly_count (3D model header)

### 5 Metadata Examples per Asset Type

**Illustration metadata example:**
```yaml
asset_id: ILL-20260710-0001-A3
name: Interior of a 7th Century Mosque
type: illustration
category: SCENE
subcategory: MOSQUE
description: Detailed interior of a simple 7th-century mosque in Medina featuring palm trunk columns and worshippers
keywords_ar: [مسجد, منبر, محراب, صلاة, عمارة إسلامية]
keywords_en: [mosque, minbar, mihrab, prayer, interior, islamic architecture]
version: 1.2.0
author: { name: Youssef Mohamed, role: Senior Illustrator, email: y.mohamed@platform.edu }
date_created: 2026-06-01
date_modified: 2026-07-10
sources: [{ type: primary, reference: Archaeological surveys, citation: SCTNH 2020 }]
license: CC-BY-NC-SA 4.0
educational_level: grade_7
language: AR
resolution: 3840x2160
file_size_bytes: 245760
color_space: sRGB
qc_status: approved
```

**3D model metadata example:**
```yaml
asset_id: 3DM-20260710-0002-B7
name: Abbasid Era Merchant Character
type: 3d_model
category: CHAR
subcategory: MERCHANT
version: 1.0.0
poly_count: 12500
lod_info: { lod0: 12500, lod1: 7500, lod2: 3750, lod3: 1250 }
texture_count: 4
texture_resolution_max: 2048
rig_type: skeletal
bone_count: 42
```

**Audio metadata example:**
```yaml
asset_id: AUD-20260710-0003-C2
name: History Lecture - Abbasid Caliphate Introduction
type: audio
category: TXT
subcategory: NARRATION
version: 1.0.0
duration_seconds: 1845
bitrate_kbps: 64
sample_rate_khz: 48
channels: 1
format: OPUS
loudness_lufs: -16
transcript: [path to transcript file]
```

### 6 Metadata Validation Rules

| Rule | Severity |
|------|----------|
| Required fields present with non-null values | Blocker |
| Field types match their defined schema | Critical |
| Enum values from approved lists only | Critical |
| Date format YYYY-MM-DD valid and plausible | Major |
| Version matches X.Y.Z pattern | Critical |
| Asset ID unique across the entire library | Critical |
| Asset ID checksum correctly calculated | Major |
| Description length 50 to 500 characters | Minor |
| Minimum 3 Arabic and 3 English keywords | Major |
| At least one source for historical/educational assets | Critical |
| Complete review history chain documented | Major |
| Accessibility notes include alt text for visual assets | Critical |
| Learning objective ID exists in curriculum database | Critical |
| File size bytes matches actual file size | Major |

### 7 Search and Discovery

Search dimensions: subject (keywords), educational level, type, historical period, region, language, author, date, QC status, license, usage rights.

### 6 Validation Rules

Required fields present = Blocker. Correct field types = Critical. Valid enum values = Critical. Valid date format = Major. Valid version format = Critical. Unique asset ID = Critical. Valid checksum = Major. Description 50-500 chars = Minor. Minimum 3 keywords per language = Major. Source cited for historical/educational = Critical.

## 13. Localization

### 1 Localization Readiness

- [ ] No embedded unextractable text
- [ ] Text layers separated and named for localization
- [ ] No text baked into raster images
- [ ] Text containers expand/contract for language length
- [ ] RTL support for Arabic/Farsi/Urdu
- [ ] Culturally appropriate colors and symbols
- [ ] Language-neutral identifiers for localizable elements

### 2 Text-Free Assets (Preferred)

| Type | Text-Free | Acceptable Exception |
|------|-----------|---------------------|
| Illustration | Preferred | Labels in separate layers |
| 3D Model | Required | No text on model |
| Animation | Preferred | Overlay layers only |
| Video | Overlay only | Lower thirds, captions |
| Icon | Required | No text at all |
| UI Asset | Overlay only | Rendered by code |

### 3 RTL Mirroring Requirements

- Reading direction right-to-left
- UI layout mirrored
- Directional icons mirrored
- Timeline flows right-to-left
- Text right-aligned, numbers left-aligned
- Confirm/primary actions on left in RTL
- Characters face reading direction

### 4 Cultural Sensitivity per Region

| Region | Sensitivity Dimensions |
|--------|----------------------|
| Saudi Arabia/Gulf | Islamic values, modesty, conservative norms |
| Levant | Religious diversity, political sensitivity |
| North Africa | Islamic values, Berber/Amazigh recognition |
| Turkey | Secular + Islamic, Ottoman heritage |
| Indonesia/Malaysia | SE Asian Islamic traditions, ethnic diversity |
| Pakistan/India | Religious diversity, regional languages |
| West Africa | Diverse Islamic traditions |

### 5 Color Meaning per Culture

Green: Islam (all regions), nature, prosperity. Red: caution, danger (Gulf), passion (Levant), patriotism (Turkey). White: purity, peace. Black: mourning, Abbasid heritage. Blue: protection (nazar), spirituality. Gold: wealth, quality. Purple: luxury, royalty (historic).

### 6 Symbol Meaning per Culture

Crescent+star: Islam everywhere. Open hand: protection (Fatima's hand). Eye: protection from evil. Olive branch: peace, Palestine. Book: knowledge, Quran. Number 7: perfection, heavens. Number 40: completion, age milestone.

### 7 Prohibited Imagery per Region

All regions: nudity, tobacco, alcohol, drugs, gambling, pig imagery, sexual content, excessive violence, disrespectful religious content.
Saudi/Gulf: cross display (non-Muslim), idol veneration, LGBTQ+, political party symbols.
Indonesia: inappropriate religious mixing, pork imagery, Communist symbols.
Pakistan: blasphemous content, politically charged India-Pakistan imagery.
Turkey: PKK symbols, insulting Ataturk, denigrating Turkish identity.

### 8 Language-Specific Font Requirements

Arabic: full Arabic script, Quranic marks, diacritics (Noto Naskh Arabic, Amiri). Urdu: Nastaliq style (Noto Nastaliq Urdu). Farsi: Persian-specific glyph forms. Pashto, Kurdish Sorani, Jawi: extended Arabic support. Turkish: ı, ş, ç, ğ, ö, ü characters. French: extended Latin accents.

### 9 Localized Asset Variants

Language variant (locale: AR, EN, TR), cultural variant (region: SA, ID, PK), censorship variant (variant: CENSORED), mirrored composition (variant: MIRRORED). Variants share asset ID root with variant suffix.

## 14. QC Checklists

### 1 Severity Levels

| Severity | Definition | Action | Response |
|----------|------------|--------|----------|
| Blocker | Asset cannot proceed | Fix immediately | Immediate |
| Critical | Significant quality/accuracy issue | Fix before approval | 24 hours |
| Major | Notable consistency/performance issue | Fix or waive with Lead OK | 48 hours |
| Minor | Small impact | Fix when convenient | Next iteration |
| Suggestion | Improvement opportunity | Consider for future | N/A |

### 2 Illustration QC Checklist

Blocker:
- [ ] ILL-001: No metadata or missing required fields
- [ ] ILL-002: File name does not follow naming convention
- [ ] ILL-003: Contains educational factual error
- [ ] ILL-004: Contains historical inaccuracy
- [ ] ILL-005: Contains culturally inappropriate content
- [ ] ILL-006: Text embedded in raster (not editable)
- [ ] ILL-007: No alt text for non-decorative image
- [ ] ILL-008: File is corrupt or cannot be opened
- [ ] ILL-009: Format is not approved
- [ ] ILL-010: Violates platform content policies

Critical:
- [ ] ILL-011: Color palette does not match style guide
- [ ] ILL-012: Resolution below minimum requirements
- [ ] ILL-013: Color contrast below WCAG 2.1 AA (text < 4.5:1)
- [ ] ILL-014: File size exceeds maximum budget
- [ ] ILL-015: Style inconsistent with series/collection
- [ ] ILL-016: Typography does not match approved system
- [ ] ILL-017: Color space incorrect (not sRGB)
- [ ] ILL-018: Character proportions inconsistent in set
- [ ] ILL-019: Perspective errors in composition
- [ ] ILL-020: Light source direction inconsistent

Major:
- [ ] ILL-021: Line weight inconsistent with style guide
- [ ] ILL-022: Shading style inconsistent with series
- [ ] ILL-023: LOD inconsistent with importance tier
- [ ] ILL-024: Alt text lacks educational context
- [ ] ILL-025: Non-text contrast below 3:1
- [ ] ILL-026: Unused layers or hidden elements
- [ ] ILL-027: Background/foreground LOD conflict
- [ ] ILL-028: Visible compression artifacts
- [ ] ILL-029: Version filename vs metadata mismatch
- [ ] ILL-030: Changelog missing or incomplete

Minor:
- [ ] ILL-031: Layer naming not standard
- [ ] ILL-032: Keywords insufficient (<3 per language)
- [ ] ILL-033: Description too short (<50 chars)
- [ ] ILL-034: Slight color drift (within tolerance)
- [ ] ILL-035: Anti-aliasing issues on edges
- [ ] ILL-036: Wrong format extension case
- [ ] ILL-037: Author field incomplete
- [ ] ILL-038: Citation format not perfect
- [ ] ILL-039: Description missing Arabic or English
- [ ] ILL-040: Learning objective not documented

### 3 3D Model QC Checklist

Blocker:
- [ ] 3DM-001: No metadata or missing required fields
- [ ] 3DM-002: File name does not follow naming convention
- [ ] 3DM-003: Educational factual error in text/labels
- [ ] 3DM-004: Historical inaccuracy
- [ ] 3DM-005: Culturally inappropriate content
- [ ] 3DM-006: Inverted normals on visible faces
- [ ] 3DM-007: File is corrupt or cannot be opened
- [ ] 3DM-008: Format is not approved
- [ ] 3DM-009: No texture maps when required
- [ ] 3DM-010: Violates platform content policies

Critical:
- [ ] 3DM-011: Poly count exceeds maximum budget
- [ ] 3DM-012: Texture resolution exceeds budget
- [ ] 3DM-013: UV mapping has significant stretching
- [ ] 3DM-014: Model scale incorrect for scene
- [ ] 3DM-015: Rig has broken constraints
- [ ] 3DM-016: LOD system missing or incorrect
- [ ] 3DM-017: File size exceeds maximum budget
- [ ] 3DM-018: No collision mesh (interactive objects)
- [ ] 3DM-019: Missing/incorrect material assignments
- [ ] 3DM-020: Texture color space incorrect

Major:
- [ ] 3DM-021: Minor UV stretching in less visible areas
- [ ] 3DM-022: Suboptimal texture packing
- [ ] 3DM-023: Smoothing groups not correctly set
- [ ] 3DM-024: Vertex count reducible without quality loss
- [ ] 3DM-025: N-gons in visible areas
- [ ] 3DM-026: Draw call count reducible via batching
- [ ] 3DM-027: Normal map baking artifacts
- [ ] 3DM-028: LOD distances not per-platform
- [ ] 3DM-029: Animation not exported with model
- [ ] 3DM-030: Version filename vs metadata mismatch

### 4 Animation QC Checklist

Blocker:
- [ ] ANIM-001: No metadata or missing required fields
- [ ] ANIM-002: File name does not follow convention
- [ ] ANIM-003: Educational factual error
- [ ] ANIM-004: Historical inaccuracy
- [ ] ANIM-005: Culturally inappropriate content
- [ ] ANIM-006: Violates platform content policies
- [ ] ANIM-007: Flashes >3 times per second
- [ ] ANIM-008: File corrupt or cannot be opened
- [ ] ANIM-009: Fails at target frame rate
- [ ] ANIM-010: No accessible alternative (text description)

Critical:
- [ ] ANIM-011: File size exceeds maximum budget
- [ ] ANIM-012: Frame rate below target (60fps)
- [ ] ANIM-013: Duration does not match spec
- [ ] ANIM-014: Visual popping/snapping at keyframes
- [ ] ANIM-015: Cannot be paused (no control)
- [ ] ANIM-016: Lip sync significantly off
- [ ] ANIM-017: Motion does not match intended physics
- [ ] ANIM-018: Joint deformation issues
- [ ] ANIM-019: Incorrect easing curves
- [ ] ANIM-020: Color space incorrect

Major:
- [ ] ANIM-021: Timing slightly off from storyboard
- [ ] ANIM-022: Minor popping at loop points
- [ ] ANIM-023: Suboptimal keyframe count
- [ ] ANIM-024: Anticipation/follow-through missing
- [ ] ANIM-025: No prefers-reduced-motion support
- [ ] ANIM-026: Background/foreground layer issues
- [ ] ANIM-027: Version filename vs metadata mismatch
- [ ] ANIM-028: Changelog missing or incomplete
- [ ] ANIM-029: Text not localization-ready
- [ ] ANIM-030: Alt text/animation description missing

### 5 Video QC Checklist

Blocker:
- [ ] VID-001: No metadata or missing required fields
- [ ] VID-002: File name does not follow convention
- [ ] VID-003: Educational factual error
- [ ] VID-004: Historical/cultural inappropriateness
- [ ] VID-005: Violates platform policies
- [ ] VID-006: No captions (prerecorded)
- [ ] VID-007: File corrupt or cannot play
- [ ] VID-008: Flashes >3 times per second
- [ ] VID-009: Audio missing or completely out of sync
- [ ] VID-010: Format not supported by platforms

Critical:
- [ ] VID-011: Resolution below minimum spec
- [ ] VID-012: File size exceeds maximum budget
- [ ] VID-013: Audio sync off >100ms
- [ ] VID-014: No audio description track
- [ ] VID-015: Captions inaccurate or >1s out of sync
- [ ] VID-016: Bitrate too low (visible artifacts)
- [ ] VID-017: Color grading inconsistent with guide
- [ ] VID-018: No chapter markers (long-form)
- [ ] VID-019: Background music not licensed
- [ ] VID-020: Aspect ratio incorrect

Major:
- [ ] VID-021: Minor audio sync (<100ms)
- [ ] VID-022: Caption formatting not standard
- [ ] VID-023: Transitions inconsistent
- [ ] VID-024: Lower thirds not standardized
- [ ] VID-025: Visual noise above threshold
- [ ] VID-026: Frame rate mismatch (judder)
- [ ] VID-027: Version filename vs metadata mismatch
- [ ] VID-028: Changelog missing or incomplete
- [ ] VID-029: Thumbnail/poster not provided
- [ ] VID-030: CC file format incorrect

### 6 Audio QC Checklist

Blocker:
- [ ] AUD-001: No metadata or missing required fields
- [ ] AUD-002: File name does not follow convention
- [ ] AUD-003: Content contains factual error
- [ ] AUD-004: Culturally inappropriate content
- [ ] AUD-005: Violates platform policies
- [ ] AUD-006: No transcript for spoken content
- [ ] AUD-007: File corrupt or cannot play
- [ ] AUD-008: Format not supported by platforms
- [ ] AUD-009: Offensive or inappropriate language
- [ ] AUD-010: Quranic audio has incorrect recitation

Critical:
- [ ] AUD-011: Bitrate below minimum spec
- [ ] AUD-012: File size exceeds maximum budget
- [ ] AUD-013: Background noise above -60dB
- [ ] AUD-014: Clipping or distortion present
- [ ] AUD-015: Sample rate does not match spec
- [ ] AUD-016: Loudness not normalized
- [ ] AUD-017: Stereo/mono configuration incorrect
- [ ] AUD-018: No silent sections trimmed
- [ ] AUD-019: Music/SFX not licensed
- [ ] AUD-020: Transcript accuracy <99%

### 7 Icon QC Checklist

Blocker:
- [ ] ICN-001: No metadata or missing required fields
- [ ] ICN-002: File name does not follow convention
- [ ] ICN-003: Culturally inappropriate content
- [ ] ICN-004: Violates platform policies
- [ ] ICN-005: Not vector (SVG) when possible
- [ ] ICN-006: Has embedded text
- [ ] ICN-007: No accessible name/label
- [ ] ICN-008: File corrupt or cannot open
- [ ] ICN-009: Style inconsistent within set
- [ ] ICN-010: Overlapping paths cause artifacts

Critical:
- [ ] ICN-011: Stroke width not per icon system
- [ ] ICN-012: Size not per icon grid
- [ ] ICN-013: Contrast below 3:1
- [ ] ICN-014: Meaning not clear/recognizable
- [ ] ICN-015: File size exceeds budget
- [ ] ICN-016: Padding not per grid spec
- [ ] ICN-017: Missing required variants
- [ ] ICN-018: Color-only meaning (no secondary indicator)
- [ ] ICN-019: SVG code unnecessarily complex
- [ ] ICN-020: Export viewBox incorrect

### 8 UI Asset QC Checklist

Blocker:
- [ ] UI-001: No metadata or missing required fields
- [ ] UI-002: File name does not follow convention
- [ ] UI-003: Violates platform policies
- [ ] UI-004: Does not match design system
- [ ] UI-005: No accessible states (focus/hover/active/disabled)
- [ ] UI-006: Not keyboard accessible
- [ ] UI-007: Touch target < 48x48px
- [ ] UI-008: No RTL variant when required
- [ ] UI-009: File corrupt or cannot open
- [ ] UI-010: Embedded text in raster format

Critical:
- [ ] UI-011: Contrast below WCAG for text/components
- [ ] UI-012: Component states not provided
- [ ] UI-013: File size exceeds budget
- [ ] UI-014: Resolution does not match target screens
- [ ] UI-015: Spacing not per design tokens
- [ ] UI-016: Typography not per design tokens
- [ ] UI-017: Missing responsive variants
- [ ] UI-018: Radius not per design tokens
- [ ] UI-019: Elevation not per design tokens
- [ ] UI-020: Format not suitable (prefer SVG)

## 15. QC Automation

### 1 File Format Validation

Magic byte verification for all formats: PNG (89504E47), JPEG (FFD8FF), WebP (52494646...57454250), AVIF (0000001C6674797061766966), SVG (3C737667), GIF (47494638), glTF Binary (676C5446), MP4 (0000001866747970), WebM (1A45DFA3), MP3 (494433/FFFB), AAC (FFF1/FFF9), Opus (4F70757348656164), WAV (52494646...57415645).

### 2 Automated Checks

Naming convention checker: segment count, prefix validity, category, language code, version format, resolution format, format code, no spaces, no special chars, no double underscores, max length.
Resolution/DPI checker: width, height, DPI, aspect ratio, min/max resolution per type.
Color space validator: ICC profile present, sRGB for web, bit depth >= 8, wide gamut flag.
File size budget checker: raw size, compressed size, size vs resolution ratio, size vs duration.
Metadata validator: required fields present, field types correct, enum valid, dates valid, version valid, asset ID checksum valid, description length, keyword count.
Version checker: filename vs metadata match, version format, version increment, changelog entry.

### 3 Contrast Ratio Calculator

Uses WCAG formula: L1 = 0.2126*R + 0.7152*G + 0.0722*B (linearized), ratio = (L1+0.05)/(L2+0.05). Tests text contrast (>4.5:1 normal, >3:1 large), non-text contrast (>3:1), focus indicator contrast (>3:1), gradient text (worst case), text on image (average/minimum).

### 4 CI/CD Pipeline Integration

| Stage | Automation | Action |
|-------|-----------|--------|
| Submission | Format, naming, metadata, version | Block on corruption/convention failure |
| Build | Size budget, resolution, color space | Block on budget/space violation |
| Pre-release | Full automated QC suite | Block on any failure |
| Release | Performance monitoring | Warn on regression |

## 16. QC Process

### 1 QC Workflow Overview

Artist Self-Review → Peer Review → Automated QC → QC Specialist Review → Educational Review → Religious Review (if applicable) → Accessibility Review → Final Approval

Each stage must pass before proceeding. Failed stages return the asset to the appropriate earlier stage with documented issues.

### 2 Stage 1: Artist Self-Review

Who: The artist/creator. When: Before submission. Goal: Catch obvious issues. Process: Review against full checklist, fix all Blocker/Critical issues, verify naming and metadata, run basic automated checks, submit with completed self-review documentation. Turnaround: Before submission.

### 3 Stage 2: Peer Review

Who: Another artist/designer. When: After self-review. Goal: Catch missed issues, ensure visual consistency. Focus: Visual consistency, composition, color, proportions, LOD, overall quality. Turnaround: 24 hours. Escalation: QC Specialist for disputes.

### 4 Stage 3: Automated QC

What: Fully automated checks. Includes: file format validation, naming convention, metadata completeness, version matching, file size budget, resolution check, color space, contrast ratio calculation. Turnaround: Instant. Blocking failures stop the pipeline.

### 5 Stage 4: QC Specialist Review

Who: Dedicated QC Specialist. Scope: Full checklist review per asset type, detailed visual inspection, technical specification verification, issue documentation. Turnaround: 48 hours. Output: QC report with pass/fail per severity level.

### 6 Stage 5: Educational Review

Who: Educational Reviewer or Subject Matter Expert. Scope: Learning objective mapping verification, content accuracy, cognitive load assessment, age-appropriateness, curriculum alignment, fact-checking protocol. Turnaround: 48 hours. Sign-off required for all educational assets.

### 7 Stage 6: Religious Review

Who: Religious Reviewer (accredited scholar). When: For assets with religious content. Scope: Scriptural accuracy, theological correctness, cultural respect, Islamic content verification. Turnaround: 72 hours. Sign-off mandatory for religious content assets.

### 8 Stage 7: Accessibility Review

Who: Accessibility Reviewer. Scope: WCAG 2.1 AA compliance, color contrast verification, alt text quality, screen reader testing, color blind simulation, motion reduction check, inclusive representation. Turnaround: 48 hours. Sign-off mandatory for all assets.

### 9 Stage 8: Final Approval

Who: QC Lead. Scope: Verify all prior reviews completed, confirm no Blocker/Critical issues, review waivers for Major issues, verify metadata completeness, sign off on asset. Turnaround: 24 hours. Output: Final QC sign-off recorded in metadata.

### 10 Blocker Management

Blocker issues halt the asset pipeline immediately. The asset is returned to the artist with detailed documentation. The QC Lead is notified of all Blocker findings. The asset cannot proceed until all Blocker issues are resolved and re-verified.

### 11 Escalation Path

Artist → Peer Reviewer → QC Specialist → QC Lead → Director of Visual Production. Disputed findings are escalated to the next level. QC Lead decisions are final unless overridden by the Director.

### 12 QC Sign-Off Requirements

All eight QC stages must have documented sign-off in the asset metadata. Sign-off includes: reviewer name, role, date, and decision (pass/pass with notes/fail). Assets with any fail status at any stage cannot proceed. Assets with pass with notes must have documented resolution timelines.

### 13 Turnaround Time Targets

| Stage | Target | Maximum |
|-------|--------|---------|
| Artist Self-Review | Before submission | N/A |
| Peer Review | 24 hours | 48 hours |
| Automated QC | Instant | 5 minutes |
| QC Specialist Review | 48 hours | 72 hours |
| Educational Review | 48 hours | 72 hours |
| Religious Review | 72 hours | 96 hours |
| Accessibility Review | 48 hours | 72 hours |
| Final Approval | 24 hours | 48 hours |

### 14 QC Documentation Requirements

Every QC review must produce: checklist with pass/fail per item, list of issues found with severity, re-verification results, sign-off documentation. All documentation stored in asset metadata and QC tracking system.

### 15 QC Process Diagrams and Flows

The QC process follows a strict sequential flow. Each gate must be passed before the asset moves to the next stage. The process is designed as a series of quality filters, each catching specific categories of issues before they propagate downstream.

**Gate Entry Criteria:**
- Artist self-review: Asset must be complete, all layers named and organized, metadata populated, naming convention verified
- Peer review: Self-review must be documented and signed off in metadata
- Automated QC: Peer review must be complete with no Blocker issues outstanding
- QC Specialist: Automated QC must pass all checks with no failures
- Educational review: QC Specialist must sign off on technical and visual quality
- Religious review: Educational review must sign off (for religious content assets)
- Accessibility review: All prior reviews must be complete and signed off
- Final approval: All prior reviews must be fully signed off with no open Blocker or Critical issues

**Gate Exit Criteria:**
- Artist self-review: Self-review checklist complete, asset ready for peer review
- Peer review: Peer reviewer sign-off, all Blocker issues resolved
- Automated QC: All automated checks passing, report generated and attached to metadata
- QC Specialist: Full QC report with pass/fail per item, all Blocker/Critical resolved
- Educational review: Educational accuracy confirmed, learning objective verified
- Religious review: Religious content accuracy confirmed, cultural sensitivity verified
- Accessibility review: WCAG compliance confirmed, inclusive representation verified
- Final approval: All gates passed, final sign-off in metadata, asset released to library

**Issue Flow:**
When an issue is found at any gate, the asset is returned to the appropriate preceding stage:
- Educational issue → Return to educational preparation / research stage
- Historical issue → Return to research / source verification stage
- Visual quality issue → Return to artist for revision
- Technical performance issue → Return to artist or technical artist
- Accessibility issue → Return to artist with specific WCAG guidance
- Naming/metadata issue → Return to artist for correction
- Cultural sensitivity issue → Return through all review stages after cultural adaptation

**Re-review Process:**
After issues are addressed, the asset enters a shortened re-review cycle:
1. Artist addresses all documented issues
2. Artist documents the fix and updates version number
3. The specific gate that found the issue re-verifies the fix
4. If the fix is satisfactory, the asset proceeds; if not, it cycles back
5. After three re-review cycles for the same issue, the issue is escalated to QC Lead

### 16 Roles and Responsibilities Detail

| Role | Specific Responsibilities | Authority |
|------|-------------------------|-----------|
| Artist | Produce assets per specifications; self-review; fix issues; maintain metadata | None to sign off; must fix all issues |
| Peer Reviewer | Check visual quality, consistency, style guide compliance; provide constructive feedback | Can reject assets for visual quality issues |
| QC Specialist | Full checklist review; technical verification; performance validation | Can reject assets; can waive Minor issues |
| Educational Reviewer | Verify learning objectives; check content accuracy; assess age-appropriateness | Can reject for educational inaccuracies |
| Historical Reviewer | Verify historical details; check sources; identify anachronisms | Can reject for historical inaccuracies |
| Religious Reviewer | Verify religious accuracy; check scriptural alignment; assess cultural respect | Can reject for religious inaccuracies |
| Accessibility Reviewer | Check WCAG compliance; test with assistive tech; verify inclusive representation | Can reject for accessibility failures |
| QC Lead | Final sign-off authority; waiver approval; escalation resolution; process oversight | Can override any review decision; sole authority for waiving Major issues |
| Director | Strategic quality direction; resource allocation; final dispute resolution | Can override QC Lead decisions |

### 17 QC Metrics and Reporting

| Metric | Collection Method | Reporting Frequency | Target |
|--------|-------------------|---------------------|--------|
| First-pass yield | Automated tracking of pass/fail at each gate | Weekly | >= 85% |
| Defect density | Issues found per asset per review cycle | Weekly | <= 2/asset |
| Blocker count | Count of Blocker issues by type | Daily | 0 at final approval |
| Re-review rate | Assets requiring >1 cycle per gate | Weekly | <= 10% |
| Gate turnaround | Time from submission to sign-off per gate | Weekly | <= 48 hours |
| Backlog size | Assets awaiting review | Daily | <= 50 assets |
| Aging assets | Assets in review > 72 hours | Weekly | 0 |
| Waiver rate | Assets with waivers granted | Monthly | <= 5% |
| Customer defects | Post-release defects reported | Monthly | <= 1/1000 |

### 18 Continuous Process Improvement

The QC process is never static. The following mechanisms drive continuous improvement:

**Quarterly Reviews:** Every quarter, the QC team conducts a comprehensive review of the previous quarter's QC metrics, defect patterns, and process effectiveness. The review identifies:
- Most common defect types and their root causes
- Gates that are most effective at catching defects versus those that could be automated
- Bottlenecks in the QC workflow that slow down production
- Training needs identified through recurring defect patterns
- Automation opportunities that could reduce manual review burden

**Root Cause Analysis:** All Blocker and Critical severity defects undergo formal root cause analysis using the 5 Whys method. The analysis is documented and shared with the production team to prevent recurrence. Systemic issues are escalated to the production process improvement program.

**Lessons Learned Database:** All QC reviews produce lessons learned that are captured in a searchable database. Team members can query the database for similar issues encountered in past projects. The database is reviewed during project kickoffs to proactively address known risk areas.

**Annual QC Bible Revision:** The Quality Control Bible undergoes an annual revision cycle each July. The revision incorporates lessons learned from the past year, updates to standards and technologies, changes in regulatory or accessibility requirements, and feedback from all QC team members and production stakeholders.

### 19 Connecting to Adjacent Production Documents

This Quality Control Bible is the central quality reference within the Visual Production documentation ecosystem. The following connections ensure consistency across all production documents:

**00_Visual_Asset_Audit Connection:** The Audit identifies assets that require QC review based on their criticality, age, and known issue history. The QC Bible defines the standards against which those assets are audited. Audit findings feed into QC process improvement and training priorities.

**01_Visual_Production_Bible Connection:** The Production Bible defines the overall production workflow, team structure, and project management framework. The QC Bible integrates quality gates into that workflow and assigns quality responsibilities to the roles defined in the Production Bible.

**02-06 Production Bibles Connection:** Each asset-type-specific production bible (Illustration, 3D, Animation, Video, Audio) defines the technical and creative standards for that asset type. The QC Bible references those standards and provides the checklists and verification processes to validate compliance. Artists reference their specific production bible during creation; QC reviewers reference the QC Bible during verification.

**07_Asset_Pipeline Connection:** The Asset Pipeline defines the sequence of production stages from concept to delivery. The QC Bible inserts quality gates at each pipeline stage and defines the entry and exit criteria for moving between stages. Pipeline automation triggers QC processes at defined stages.

**09_AI_Asset_Generation Connection:** AI-generated assets require additional QC scrutiny for issues unique to AI generation including hallucination, bias, inconsistent quality, and licensing ambiguity. The QC Bible's standards apply equally but with enhanced verification for AI-generated content.

**10_Asset_Library Connection:** The Asset Library is the final destination for all QC-approved assets. Library metadata is populated from the asset's QC metadata. Search and discovery in the library rely on the metadata standards defined in this QC Bible. Library access controls enforce usage rights verified during QC.

---

## Document Control

| Field | Value |
|-------|-------|
| Document ID | QC-BIBLE-001 |
| Version | 1.0.0 |
| Effective Date | 2026-07-10 |
| Next Review Date | 2027-07-10 |
| Owner | Quality Control Lead |
| Approver | Director of Visual Production |
| Classification | Internal — Quality Control |
| Storage Location | Visual_Production/08_Quality_Control/ |

## Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0.0 | 2026-07-10 | QC Lead | Initial release — comprehensive QC Bible |

---

*End of Quality Control Bible*