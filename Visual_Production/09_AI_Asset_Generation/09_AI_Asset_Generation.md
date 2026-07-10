# AI Asset Generation Rules

**Document ID:** VP-AIGEN-001
**Version:** 1.0
**Status:** Final
**Classification:** Enterprise AI Production Standard
**Last Updated:** 2026-07-10
**Owner:** AI Production Directorate / Chief Creative Director
**Review Cycle:** Monthly (due to rapid AI evolution)
**Enforcement Level:** Mandatory — No AI-generated asset enters the pipeline without passing these rules.

---

## Cross-Reference Connections

| Connected Document | Relationship | Integration Point |
|---|---|---|
| `Visual_Production/00_Visual_Asset_Audit` | Asset inventory defines what AI must generate | AI generation targets mapped to audit gaps |
| `Visual_Production/01_Visual_Production_Bible` | Visual philosophy governs AI output style | AI must produce assets indistinguishable from human-created work |
| `Visual_Production/02_Illustration_Bible` | Illustration specs constrain AI illustration output | AI follows exact style, palette, composition rules |
| `Visual_Production/03_3D_Asset_Bible` | 3D specs constrain AI 3D output | AI enforces poly budgets, topology, UV standards |
| `Visual_Production/04_Animation_Bible` | Motion specs constrain AI animation output | AI follows physics-based motion, choreography standards |
| `Visual_Production/05_Video_Production_Bible` | Video specs constrain AI video output | AI meets minimum 1080p, 24fps, cultural sensitivity |
| `Visual_Production/06_Audio_Production_Bible` | Audio specs constrain AI audio output | AI enforces 48kHz, 24-bit, -16 LUFS |
| `Visual_Production/07_Asset_Pipeline` | Pipeline governs AI asset ingestion | AI output must conform to pipeline format standards |
| `Visual_Production/08_Quality_Control` | QC governs AI asset verification | AI assets pass identical QC to human-produced assets |
| `Visual_Production/10_Asset_Library` | Library stores AI-generated assets | AI assets tagged with generation metadata |
| `UX_Design/02_Design_System` | Design tokens constrain AI color/style | AI uses exact design system tokens |
| `UX_Design/06_Motion_Design_Bible` | Motion specs inform AI animation rules | AI motion must match motion bible exact specifications |
| `UX_Design/07_Illustration_Bible` | Illustration specs inform AI illustration rules | AI style reference bank sourced from illustration bible |
| `UX_Design/08_Accessibility_Bible` | Accessibility requirements constrain all AI output | AI must meet WCAG AA standards minimum |
| `AIOS/00_AI_Architecture` | AI system architecture enables generation pipeline | Generation governed by AIOS orchestration layer |
| `Documentation/12_Story_Mode` | Story content requirements inform AI generation | AI generates story-consistent visuals |
| `Documentation/15_Podcast` | Audio content requirements inform AI voice | AI narration matches podcast standards |
| `Learning_Experience/02_Memory_Engine` | Memory visuals inform AI generation parameters | AI generates memory-consistent assets |
| `Learning_Experience/03_Gamification_Bible` | Reward visuals inform AI achievement generation | AI generates reward-consistent assets |

---

## Table of Contents

1. [AI Asset Generation Philosophy](#1-ai-asset-generation-philosophy)
2. [AI Ethics & Governance](#2-ai-ethics--governance)
3. [Illustration Generation](#3-illustration-generation)
4. [Icon Generation](#4-icon-generation)
5. [Background Generation](#5-background-generation)
6. [Infographic Generation](#6-infographic-generation)
7. [Map Generation](#7-map-generation)
8. [Animation Generation](#8-animation-generation)
9. [Video Generation](#9-video-generation)
10. [3D Model Generation](#10-3d-model-generation)
11. [Sound Effects Generation](#11-sound-effects-generation)
12. [Narration Generation](#12-narration-generation)
13. [Historical Accuracy in AI](#13-historical-accuracy-in-ai)
14. [Educational Integrity in AI](#14-educational-integrity-in-ai)
15. [Style Consistency](#15-style-consistency)
16. [Prompt Engineering Guide](#16-prompt-engineering-guide)
17. [Human Oversight Requirements](#17-human-oversight-requirements)
18. [AI Review Checklist](#18-ai-review-checklist)

---

## 1. AI Asset Generation Philosophy

### 1.1 Core Philosophy Statement

AI is a powerful tool, not a replacement for human creativity and judgment. The platform embraces AI as an accelerant for visual production, enabling the creation of thousands of high-quality assets at a speed and scale impossible for human artists alone. However, AI generation is always governed by the principle that human oversight, creative direction, and quality control remain the ultimate arbiters of what ships to users.

AI accelerates production but never bypasses quality, accuracy, or sensitivity standards. Human oversight is required at every critical stage. The goal is to produce assets that are indistinguishable from human-created work in quality, while scaling production beyond human capacity.

### 1.2 The AI Production Triangle

AI asset generation operates within three interdependent constraints:

**Speed:** AI must deliver assets faster than traditional production. A full illustration concept that would take an artist 8 hours should take AI 30 minutes to draft. A 3D model that would take 40 hours should take AI 4 hours to produce base geometry. Speed is the primary value proposition of AI in the pipeline.

**Quality:** AI output must meet the same quality bar as human-created assets. No quality discounts are granted because an asset was AI-generated. If an AI asset does not meet the quality standard, it is rejected, same as a human asset. The quality bar is defined by the Visual Production Bible, the Illustration Bible, the 3D Asset Bible, and all other asset-specific specification documents.

**Scale:** AI must enable production at scales impossible for human teams. Generating 500 icons for a complete icon set. Producing 200 background variations for different learning modes. Creating 1000 illustration variants for an asset library. Scale is the secondary value proposition of AI integration.

### 1.3 AI as Force Multiplier

AI functions as a force multiplier for the creative team, not a replacement. The creative team remains the same size or grows slowly, but output multiplies by 5x to 10x. This multiplication is achieved through:

**Draft Generation:** AI produces first-pass drafts from prompt specifications. Artists receive a close-to-final asset that requires refinement rather than creation from scratch. This reduces per-asset creation time by 60-80%.

**Variant Production:** AI generates multiple variants of approved concepts. Color variations, composition variations, style variations. The human selects the best variant and provides feedback for refinement.

**Asset Series Production:** AI produces series of related assets from a single approved template. A prompt template approved for one historical city illustration can generate 20 city illustrations with consistent quality.

**Repetitive Asset Production:** AI handles high-volume, low-complexity asset types: icons, patterns, textures, simple backgrounds. Human artists focus on hero assets, complex compositions, and creative direction.

### 1.4 Human-AI Collaboration Model

The platform uses a structured human-AI collaboration model with clear role boundaries:

**Human Directs:** Humans define creative direction, select style references, write prompts, approve concepts, and make all creative decisions. AI does not make creative decisions. It executes creative directions within defined constraints.

**AI Produces:** AI generates draft assets, variants, and series based on human direction. AI follows strict prompt parameters, style references, and technical specifications. AI does not deviate from established constraints.

**Human Refines:** Humans review AI output, provide feedback, make corrections, and approve final assets. AI output is never used directly without human review. Every AI-generated asset passes through at least one human review stage.

**Human Approves:** No asset enters the production pipeline without human approval. The approval workflow is identical for human-created and AI-created assets. AI generation metadata is attached to all AI assets for traceability.

### 1.5 When AI Is Used and When It Is Not

AI is used for assets that fall into specific categories with clear boundaries:

**AI-Primary (AI generates, human reviews):**
- Background environments (mode-appropriate)
- Icons (simple and complex)
- Patterns and textures
- Simple illustration assets (objects, plants, tools)
- Graphic elements and decorative assets
- Initial concept drafts and variants
- LOD generation for 3D assets
- Format conversion and asset optimization

**AI-Assisted (Human creates, AI enhances):**
- Complex illustrations and story scenes
- Character illustrations and expressions
- Hero 3D assets and environments
- Animations and motion sequences
- Video production and editing
- Infographics and data visualizations
- Maps and geographic renderings

**AI-Prohibited (Human-only creation):**
- Content requiring original artistic vision
- Sacred text rendering and calligraphy
- Sensitive cultural or religious depictions
- Content requiring subjective aesthetic judgment
- Final human character expressions and emotions
- Any asset requiring novel creative interpretation

### 1.6 Quality Parity Principle

The Quality Parity Principle states: AI-generated assets must meet the same quality standards as human-created assets. There is no separate, lower quality standard for AI-generated content. This principle applies to:

**Visual Quality:** Composition, color, lighting, detail, and style must match human-created work. An AI illustration must be as beautiful and well-crafted as a human illustration.

**Technical Quality:** Resolution, format, file size, and technical specifications must match human-created work. An AI 3D model must meet poly budgets and topology standards.

**Accuracy Quality:** Historical, educational, and cultural accuracy must match human-created work. An AI historical scene must be as accurate as a human-researched scene.

**Sensitivity Quality:** Cultural, religious, and accessibility sensitivity must match human-created work. An AI asset must pass the same sensitivity reviews as a human asset.

Any AI asset that fails quality parity is rejected or sent for human rework. There is no "good enough for AI" standard. The parity bar is absolute.

### 1.7 AI Asset Lifecycle

Every AI-generated asset follows a defined lifecycle with documented checkpoints:

**Phase 1: Brief Definition.** Human defines asset requirements: type, style, parameters, constraints, references. The brief is documented and approved before any generation begins.

**Phase 2: Prompt Engineering.** Human writes or selects prompts based on the brief. Negative prompts are added. Style references are attached. Parameters are configured.

**Phase 3: Initial Generation.** AI generates initial output based on prompts. Multiple variants are produced (minimum 3 for most asset types).

**Phase 4: Human Review — Draft.** Human reviews initial variants. Selects best direction. Provides feedback for refinement. Rejects failed variants with documented reasons.

**Phase 5: Refinement Generation.** AI regenerates based on feedback. Addresses specific issues identified in review. Produces refined variants.

**Phase 6: Human Review — Final.** Human reviews refined output. Checks against brief requirements. Verifies quality parity. Approves or sends for further refinement.

**Phase 7: Quality Control.** QC team applies standard quality control checklist. Verifies technical specifications. Checks for artifacts, errors, or issues.

**Phase 8: Metadata Tagging.** AI generation metadata is attached: model used, prompt used, date generated, human reviewer, approval status.

**Phase 9: Pipeline Ingestion.** Approved asset enters the asset pipeline. Goes through format conversion, optimization, and library storage.

**Phase 10: Usage Monitoring.** Asset usage is tracked. Performance is monitored. If issues arise (style drift, quality degradation, user complaints), asset is flagged for review.

---

## 2. AI Ethics & Governance

### 2.1 Ethical Foundation

The platform's AI usage is governed by seven ethical principles that apply to all AI-generated content. These principles are non-negotiable and apply regardless of the AI model, tool, or technique used.

**Transparency:** AI-generated content is clearly labeled as AI-generated. Users have the right to know when they are viewing AI-created content. Transparency builds trust and prevents deception.

**Accountability:** Humans are responsible for all published content, regardless of how it was produced. The human who approves an AI-generated asset is accountable for its quality, accuracy, and appropriateness. AI is never blamed for errors, the human who approved the AI output is responsible.

**Fairness:** AI does not perpetuate bias. All prompts, training data, and model selection are vetted for fairness. AI output is reviewed for unintended bias in representation, language, and imagery.

**Privacy:** AI is not trained on user data without explicit consent. No user data, learning progress, personal information, or behavioral data is used to train or fine-tune AI models. User data privacy is absolute.

**Cultural Respect:** AI is guided to respect all cultures, traditions, and beliefs. AI output is reviewed for cultural sensitivity. AI never generates content that mocks, stereotypes, or disrespects any culture.

**Religious Respect:** AI never generates disrespectful content about any religion. Religious figures, texts, symbols, and practices are treated with respect. AI is specifically constrained to avoid generating content that could be considered blasphemous, sacrilegious, or disrespectful.

**Quality:** AI assets meet the same quality bar as human assets. No quality exceptions are made for AI-generated content.

### 2.2 AI Transparency Labeling

Every AI-generated asset must carry transparency metadata attached at the file level and displayed in appropriate contexts:

**File Metadata:**
- `ai_generated: true`
- `ai_model: {model_name_version}`
- `generation_date: {ISO_date}`
- `human_reviewer: {reviewer_id}`
- `approval_status: approved|rejected|pending`
- `prompt_id: {reference_to_prompt_library}`
- `generation_parameters: {key_parameters}`

**User-Facing Labels:** AI-generated content in educational contexts may include a subtle indicator: a small "AI-generated" badge, a footnote, or a tooltip. The label is informative, not alarming. It tells the user: "This visual was created with AI assistance and reviewed by our team."

**Internal Labels:** AI assets in the production pipeline are clearly marked in asset management systems. The production team knows at every stage which assets are AI-generated and which are human-created.

### 2.3 Human Accountability Chain

The chain of accountability for AI-generated content is clear and documented with defined roles:

**Prompt Engineer:** Accountable for writing accurate, appropriate, and safe prompts. Errors in prompts that lead to bad AI output are traced to the prompt engineer.

**Reviewer:** Accountable for reviewing AI output against specifications. Approval by the reviewer constitutes acceptance of responsibility for the asset's quality, accuracy, and appropriateness.

**Art Director:** Accountable for overall creative direction and quality standards. The art director ensures AI output meets the platform's visual standards.

**Content Lead:** Accountable for educational and factual accuracy. The content lead ensures AI output contains correct information.

**Cultural Advisor:** Accountable for cultural and religious sensitivity. The cultural advisor signs off on assets that touch on sensitive topics.

**Executive Producer:** Ultimately accountable for all published content. The executive producer ensures the accountability chain is followed and documented.

### 2.4 Bias Prevention Framework

AI bias is prevented through a multi-layered framework that operates at every stage of generation:

**Prompt Bias Review:** All prompts are reviewed for bias before generation. Prompts that could produce biased output are flagged and revised. Prompt engineers are trained in bias awareness.

**Model Selection:** AI models are evaluated for bias before adoption. Models known to produce biased output are rejected. Models must pass a bias audit before being approved for production use.

**Output Bias Review:** AI output is reviewed for bias as part of the standard review workflow. Reviewers check for: balanced representation, appropriate diversity, absence of stereotypes, fair treatment of all subjects.

**Representation Guidelines:**
- Historical content: represent the actual diversity of the historical period being depicted
- Modern content: represent the diversity of the Muslim world and broader global community
- Characters: avoid stereotypes in appearance, dress, and behavior
- Gender: represent both men and women appropriately in historical and modern contexts
- Age: represent different age groups with dignity
- Ability: include representations of people with disabilities when appropriate

**Bias Correction Workflow:**
1. Bias detected (by reviewer, automated tool, or user report)
2. Asset is flagged and quarantined
3. Bias is documented: what bias, where it appears, likely cause
4. Prompt is revised to prevent bias recurrence
5. Asset is regenerated or corrected
6. Corrected asset goes through standard review
7. Bias incident is documented in the bias log

### 2.5 Cultural Sensitivity Governance

AI cultural sensitivity is governed by specific rules and review protocols:

**Cultural Consultation:** Any AI generation involving specific cultures, traditions, or practices requires consultation with cultural advisors. Advisors review prompts, reference materials, and output.

**Cultural Reference Library:** AI is provided with a curated library of culturally appropriate references. This library is vetted by cultural advisors and updated regularly. AI must use approved references only.

**Cultural Sensitivity Prompts:** Prompts for culturally sensitive content include specific sensitivity instructions. These instructions constrain AI to generate content that respects cultural norms and practices.

**Automatic Flagging:** AI output is automatically flagged for review when it involves: religious content, cultural practices, traditional dress, sacred sites, ethnic representations, or any content with cultural sensitivity markers.

**Forbidden Content:** AI never generates: caricatures of cultural groups, misrepresentations of cultural practices, culturally inappropriate combinations, disrespectful depictions of cultural symbols, or any content that cultural advisors have identified as potentially harmful.

### 2.6 Religious Respect Governance

Religious sensitivity is treated with the highest priority across all AI generation:

**Sacred Text Handling:** AI never generates, modifies, or renders Quranic text. Quranic text is human-created and human-verified only. AI may not be used to write, complete, or interpret Quranic verses.

**Religious Figure Depiction:** AI follows the platform's guidelines on depicting prophets, companions, and religious figures. These guidelines are conservative and respect the diversity of Islamic traditions. When in doubt, AI does not depict.

**Religious Symbol Handling:** Religious symbols (crescent, calligraphy, architectural elements) are generated with respect and accuracy. AI never creates parodies, modifications, or disrespectful versions of religious symbols.

**Context Sensitivity:** AI considers the religious context of content. A mosque background is treated differently from a market background. Religious settings require higher sensitivity review.

**Automated Religious Content Flagging:** Any AI output detected to contain religious content is automatically flagged for human review. No AI-generated religious content passes without human approval.

### 2.7 Privacy and Data Governance

AI privacy governance ensures user data is never compromised through generation pipelines:

**No User Data Training:** AI models used by the platform are never trained on user data. User learning data, personal information, behavioral data, and communication data are excluded from any training or fine-tuning.

**No User Data in Prompts:** Prompts never contain user-identifiable information. No user names, locations, personal details, or learning histories are included in prompts.

**Prompt Anonymization:** Any prompt that may reference user-related data is anonymized before processing. User identifiers are stripped. Personal information is removed.

**Output Privacy Review:** AI output is reviewed to ensure it does not contain or reveal user data. If user data is detected, the output is quarantined and the incident is investigated.

**Model Data Governance:** All AI models used for generation are vetted for data handling practices. Models that train on user data without explicit consent are rejected. Models must have documented data governance policies.

### 2.8 Ethical Incident Response

When an AI ethics incident occurs (biased output, privacy breach, cultural insensitivity, religious disrespect), the following five-tier response protocol is activated:

**Tier 1 — Immediate Containment:**
1. Flag and quarantine the offending asset
2. Remove asset from all user-facing contexts
3. Notify the AI Production Director
4. Document the incident with screenshots and context

**Tier 2 — Investigation:**
1. Identify root cause: prompt error, model issue, review failure, process gap
2. Determine scope: how many assets affected, how long undetected, how many users impacted
3. Interview relevant team members
4. Review similar assets for the same issue

**Tier 3 — Correction:**
1. Fix the root cause (revise prompt, update model, improve review process, add guardrail)
2. Re-review all potentially affected assets
3. Regenerate or correct problematic assets
4. Update documentation to prevent recurrence

**Tier 4 — Communication:**
1. If users were affected, prepare user communication
2. If the issue was significant, prepare public communication
3. Document lessons learned in the ethics incident log
4. Present findings to the AI Ethics Board

**Tier 5 — Prevention:**
1. Update prompt engineering guidelines
2. Add automated detection for the issue type
3. Train team members on the incident
4. Update review checklists

### 2.9 AI Ethics Board

The AI Ethics Board governs all AI usage on the platform with defined membership and responsibilities:

**Membership:**
- Chief Creative Director (Chair)
- AI Production Director
- Head of Content
- Senior Cultural Advisor
- Senior Religious Advisor
- Legal Counsel
- User Research Lead
- External Ethics Consultant

**Responsibilities:**
- Review and approve all AI models used for generation
- Approve AI use cases and define boundaries
- Review ethics incidents and approve corrective actions
- Update ethical guidelines as AI capabilities evolve
- Approve new AI generation tools and techniques
- Conduct quarterly ethics audits of AI output

**Meeting Cadence:**
- Monthly standing meetings
- Emergency meetings convened for incidents
- Quarterly ethics audits
- Annual ethics guideline review

---

## 3. Illustration Generation

### 3.1 Style Enforcement

AI illustration generation must match the platform's unified illustration style exactly. The style is defined as "Flat-with-Depth" as specified in the Visual Production Bible and Illustration Bible.

**Style Reference Images:**
- A curated library of 100+ approved illustration style references is provided to the AI model
- References cover: characters, environments, objects, scenes, patterns
- References are categorized by: content type, period, region, mood
- References are updated when the style evolves
- AI must be prompted with style references for every generation

**Style Parameters:**
- Base shapes: flat, no complex shading
- Depth creation: through layering, not rendering
- Layer color values: distinct, separated by at least 15% value difference
- Shadows: simple, geometric, falling lower-right (consistent light source upper-left)
- Highlights: clean, minimal, upper-left placement
- Line work: clean, consistent weight (2px for standard illustrations, 1px for detail)
- Overall effect: dimensional but not photorealistic

**Style Constraint Prompts:**
Every illustration prompt must include style constraint language:
- "Style: flat illustration with clean geometric shapes, warm color palette, layered depth, simple shadows falling lower-right, clean minimal highlights upper-left"
- "Reference style: [attached style reference image ID]"
- "No gradients, no photorealistic rendering, no complex shading, no 3D effects"

### 3.2 Composition Guidelines

AI proposes 3 compositions for each illustration request. The human selects the best direction:

**Composition Generation Rules:**
- All 3 compositions must follow the brief parameters
- Compositions must differ in: focal point placement, depth layering, or perspective
- Each composition is submitted as a concept sketch (not full render)
- Compositions include: rough layout, value distribution, focal point indication
- Composition 1: rule of thirds placement, standard perspective
- Composition 2: centered or symmetrical placement, alternative perspective
- Composition 3: dynamic or unconventional placement, experiment perspective

**Composition Review Criteria:**
Human reviewers evaluate compositions on:
- Narrative clarity: does the composition tell the right story?
- Focal point: is the most important element emphasized?
- Depth: are there clear foreground, midground, background layers?
- Balance: is visual weight distributed appropriately?
- Flow: does the eye move naturally through the composition?
- Negative space: is space used effectively?
- Educational clarity: does the composition support learning?

**Composition Constraints:**
- Rule of thirds for primary subject placement
- Leading lines toward focal point
- Foreground/midground/background layering (minimum 3 layers)
- Clear focal point within 2 seconds of viewing
- Negative space used to direct attention
- Golden ratio positioning for primary emotional beats
- Frame-within-frame using architectural elements when appropriate

### 3.3 Color Palette Constraints

AI illustration color must be constrained to the platform palette:

**Palette Source:**
- Primary palette: defined in UX_Design/02_Design_System
- Warm palette: the default for all illustrations
- Period-specific palettes: defined in the Visual Production Bible section 7.3
- Mode-specific palettes: defined per learning mode
- Context-specific palettes: defined per asset type

**Color Constraint Prompts:**
- "Color palette: warm earth tones [burnt sienna, ochre, umber] with cool accents [teal, steel blue]"
- "No colors outside the approved palette"
- "Saturation level: moderate with strategic saturated accents"
- "Color harmony: complementary or analogous as specified in brief"

**Color Review:**
- AI output is checked against palette specifications
- Out-of-palette colors are flagged
- Saturation levels are verified
- Color harmony is evaluated

### 3.4 Character Illustration Generation

AI character illustrations must maintain consistent proportions, expressions, and clothing across all appearances:

**Character Proportions:**
- Stylized 4-5 head proportion
- Consistent head-to-body ratio across all characters
- Consistent facial feature placement
- Consistent limb proportions
- Character proportion reference sheet provided to AI for each character type

**Expression Generation:**
- AI generates expressions from an approved expression library
- Expressions must be consistent with the character's base design
- Emotion range: happiness, sadness, contemplation, surprise, concern, determination
- Expression intensity: subtle for background characters, expressive for primary characters
- Expression reference sheet provided to AI for each character

**Clothing Consistency:**
- Historical characters: period-appropriate, region-appropriate clothing
- Modern characters: contemporary, culturally appropriate clothing
- Scholar characters: scholarly attributes (books, pens, instruments)
- Clothing colors: within platform palette
- Clothing details: accurate to period and region
- Clothing reference library provided to AI

**Character Generation Constraints:**
- No photorealistic characters
- No caricature or exaggeration
- No disrespectful depictions
- No anachronistic clothing
- No inconsistent proportions across the same character in different scenes

### 3.5 Environment Illustration Generation

AI environment illustrations must be period-accurate and region-appropriate:

**Environment Reference Library:**
- Period-specific architecture references
- Region-specific landscape references
- Cultural-specific decoration references
- Lighting reference by time of day and period
- Color palette reference by period and region

**Historical Environment Constraints:**
- Architecture must match the period and region
- Decorative elements must be culturally and historically appropriate
- Plants and landscapes must be native to the region
- Lighting must match period-appropriate light sources
- No anachronistic elements

**Atmosphere and Mood:**
- Mood must match the educational context
- Lighting creates atmosphere
- Color temperature supports mood
- Atmospheric effects (fog, dust, light rays) are period-appropriate
- Mood is consistent across the scene

### 3.6 Historical Illustration Constraints

AI illustrations of historical content must pass historical accuracy review before approval:

**Pre-Generation Research:**
- AI is provided with verified historical references before generation
- References include: period, region, event, key figures, setting details
- AI is constrained to generate only from provided references
- AI does not invent historical details

**Historical Accuracy Prompt Requirements:**
- Specific year or date range in the prompt
- Specific region and location
- Specific event or context
- Specific figures involved (with reference)
- Period-appropriate detail parameters

**Anachronism Prevention Prompts:**
- Include negative prompts for common historical anachronisms:
  - "No modern architecture, no modern clothing, no modern technology"
  - "No electric lighting, no modern materials, no modern objects"
  - "No anachronistic weapons, tools, or instruments"
  - "No modern hairstyles, no modern facial hair styles"
  - "No modern racial or ethnic anachronisms"

### 3.7 Prompt Engineering for Consistent Illustration Style

**Standard Illustration Prompt Template:**

Generate a [type] illustration in the platform's flat-with-depth style.

Subject: [description of subject]
Setting: [period, region, location]
Composition: [focal point, perspective, layout]
Mood: [emotion or atmosphere]
Color palette: [specific palette reference]
Lighting: [light source, time of day, quality]

Style constraints:
- Flat illustration with clean geometric shapes
- Warm color palette from approved palette
- Layered depth (foreground, midground, background)
- Simple shadows falling lower-right
- Clean minimal highlights upper-left
- 2px consistent line weight
- No gradients, no photorealistic rendering
- No 3D effects, no complex shading

[Specific constraints based on asset type]
[Negative prompts based on asset type]
[Style reference: reference_image_id]

**Prompt Refinement Workflow:**
1. Write initial prompt using template
2. Generate first batch (3-4 variants)
3. Review: identify issues (composition, color, accuracy, style)
4. Refine prompt: address specific issues, add constraints
5. Generate second batch
6. Review: verify issues resolved
7. Approve or iterate further

### 3.8 Negative Prompt Library for Illustrations

**Common Negative Prompts (applicable across all illustration types):**
- "No photorealistic rendering, no 3D rendering, no CGI style"
- "No gradients, no complex shading, no soft shadows"
- "No modern elements, no anachronisms"
- "No cluttered composition, no unclear focal point"
- "No distorted anatomy, no incorrect proportions"
- "No inconsistent lighting, no multiple light sources"
- "No oversaturated colors, no neon colors, no colors outside palette"
- "No text in the illustration (text is added separately)"
- "No watermarks, no signatures, no artist marks"

**Character Illustration Negative Prompts:**
- "No photorealistic faces, no uncanny valley"
- "No distorted facial features, no asymmetrical features"
- "No incorrect number of fingers, no distorted hands"
- "No floating elements, no disconnected limbs"
- "No inconsistent character proportions"
- "No inappropriate expressions for context"

**Historical Illustration Negative Prompts:**
- "No modern clothing, no modern hairstyles"
- "No modern buildings, no modern architecture"
- "No electric lights, no modern technology"
- "No anachronistic objects, no out-of-period elements"
- "No incorrect historical uniforms or armor"
- "No modern political boundaries on maps"

**Environment Illustration Negative Prompts:**
- "No empty or barren environments (unless historically accurate)"
- "No inconsistent scale between elements"
- "No floating objects"
- "No incorrect plant species for region"
- "No generic Middle Eastern aesthetic (be specific to period/region)"

---

## 4. Icon Generation

### 4.1 Unified Icon Style

All AI-generated icons follow a strict unified style with precise grid and stroke specifications:

**Grid System:**
- All icons designed on a 24x24 pixel grid
- Grid alignment: pixel-perfect, snapped to grid
- Padding: 2px internal padding on all sides
- Active area: 20x20 pixel drawing area within the 24x24 grid

**Stroke Specifications:**
- Stroke width: 2px uniform stroke
- Stroke caps: rounded (1px radius)
- Stroke joins: rounded (1px radius)
- Stroke color: consistent with icon category color
- No variable stroke widths within an icon

**Fill Specifications:**
- Default: outlined (stroke only, no fill)
- Active state: filled with category color
- Disabled state: 40% opacity of default state
- No gradients, no patterns, no textures in icons

**Corner Radius:**
- Standard corners: 2px radius
- Sharp corners: 0px radius (for specific geometric icons)
- Rounded elements: 1px minimum, 4px maximum

### 4.2 Semantic Meaning Preservation

Icons must clearly communicate their meaning without ambiguity:

**Meaning Clarity:**
- Icon meaning must be recognizable within 1 second
- Use established icon conventions (magnifying glass = search, house = home)
- No abstract representations for concrete concepts
- Cultural adaptation: some icons have different meanings in different cultures
- RTL awareness: icons with directional meaning (arrows, back, forward) must be mirrored for RTL

**Meaning Verification:**
- Every icon is tested with users for meaning recognition
- Icons below 80% recognition threshold are redesigned
- Meaning is verified for all supported cultures and regions

### 4.3 RTL Mirror Awareness

Icons with directional meaning require RTL versions for right-to-left language support:

**Directional Icons:**
- Arrows (left, right, back, forward, up, down)
- Navigation icons (next, previous, return)
- Media icons (play, rewind, fast-forward)
- Communication icons (reply, forward, send)
- Layout icons (sort, filter, organize)

**RTL Rules:**
- Directional icons are horizontally mirrored for RTL layouts
- Mirroring uses the vertical axis center
- Icon meaning must be preserved in mirrored version
- Non-directional icons are not mirrored

### 4.4 Consistent Visual Weight

All icons in a set must have consistent visual weight for balanced appearance:

**Visual Weight Parameters:**
- Stroke width: uniform 2px across entire set
- Density: similar amount of detail across icons
- Complexity: similar level of detail across icons
- Size: icon content fills similar proportion of the grid
- Spacing: similar internal spacing across icons

**Visual Weight Verification:**
- Icons are compared side-by-side for visual balance
- Automated tools check stroke width consistency
- Visual weight audit conducted for every icon set

### 4.5 State Variations

Every icon has defined states with specific visual treatments:

**Default State:**
- Normal appearance
- Category color (or neutral color for monochrome sets)
- Standard opacity (100%)

**Active State:**
- Filled version of the icon
- Category color
- Standard opacity (100%)
- May include subtle background indicator

**Disabled State:**
- Same as default or active
- Reduced opacity (40%)
- No color change (gray is not used, reduced opacity preserves meaning while indicating unavailability)
- No interaction feedback

**Hover/Focus State:**
- Subtle scale increase (105%)
- Subtle color shift (10% lightness increase)
- Cursor change to pointer

### 4.6 Icon Generation Prompt Templates

**Standard Icon Prompt Template:**

Generate a [icon_name] icon in the platform's unified icon style.

Grid: 24x24 pixel grid
Style: outlined, 2px stroke, rounded caps, rounded joins
Color: [category color hex]
State: [default/active/disabled]

Semantic requirements:
- Meaning: [clear description of what the icon represents]
- Convention: [standard convention to follow if applicable]
- RTL: [check if directional - yes/no]

Quality constraints:
- Pixel-perfect alignment
- Consistent 2px stroke throughout
- Rounded caps and joins (1px radius)
- 2px internal padding from grid edge
- No fills for default state
- No gradients, no patterns, no textures
- No text in the icon

Cultural constraints:
- [Any cultural meaning considerations]
- [RTL mirror requirement if applicable]

Negative prompts:
- No thin strokes, no variable stroke widths
- No complex details that won't read at 24px
- No text characters
- No gradients or shadows
- No photorealistic elements

### 4.7 Icon Accessibility Requirements

Icons must be distinguishable by shape, not just by color:

**Shape Distinction:**
- Every icon has a unique silhouette
- No two icons in the same set have identical or near-identical shapes
- Shape difference must be recognizable at 16px size

**Color Independence:**
- Icon meaning is conveyed through shape, not color
- Color may reinforce meaning but must not be the only differentiator
- Monochrome version of each icon conveys the same meaning as colored version

**Size Versatility:**
- Icons must be legible at 16px (minimum UI size)
- Icons are designed at 24px and tested at all required sizes (16px, 20px, 24px, 32px, 48px, 64px)
- Detail level is optimized for the smallest expected size
- No critical details smaller than 2px

**Alternative Text:**
- Every icon has an alt text description
- Description conveys the icon's meaning, not its appearance
- Example: "Search" not "Magnifying glass"
- Alt text is provided for every icon at generation time

### 4.8 AI Icon Review Criteria

- [ ] Icon designed on 24x24 grid
- [ ] 2px stroke consistently applied
- [ ] Rounded caps and joins
- [ ] Pixel-perfect alignment
- [ ] Internal padding maintained (2px)
- [ ] Color matches category specification
- [ ] RTL version exists for directional icons
- [ ] All states generated (default, active, disabled)
- [ ] Visual weight consistent with icon set
- [ ] Meaning is clear and recognizable
- [ ] Shape distinguishable from other icons in set
- [ ] Legible at 16px minimum
- [ ] Alt text provided
- [ ] Cultural meaning verified

---

## 5. Background Generation

### 5.1 Mode-Appropriate Backgrounds

AI generates background environments for each of the platform's 8 learning modes with distinct specifications:

**Mode Background Specifications:**

| Learning Mode | Background Style | Color Dominance | Mood | Atmosphere |
|---|---|---|---|---|
| Story Mode | Cinematic environment | Warm golds, deep blues | Immersive, narrative | Atmospheric depth, period lighting |
| Documentary Mode | Realistic/authentic environment | Warm earth tones, muted | Authoritative, grounded | Natural light, documentary clarity |
| Knowledge Tree | Abstract knowledge space | Deep indigos, warm golds | Exploratory, deep | Ethereal glow, particle atmosphere |
| Memory Garden | Natural/organic environment | Emerald greens, warm sunlight | Peaceful, serene | Soft diffusion, gentle light |
| AI/Mentor Mode | Modern/sophisticated environment | Cool blues, warm accents | Intelligent, warm | Subtle glow, clean atmosphere |
| Achievement Mode | Celebratory environment | Golds, warm corals, rich jewel tones | Joyful, triumphant | Warm radiance, sparkle atmosphere |
| Community Mode | Warm/social environment | Warm neutrals, soft corals | Welcoming, connective | Soft, intimate atmosphere |
| Quiz/Assessment Mode | Focused/clean environment | Clean whites, warm accents | Clear, focused | Minimal, distraction-free |

### 5.2 Seamless Tiling for Patterns

AI-generated patterns must tile seamlessly for continuous background coverage:

**Tiling Requirements:**
- Pattern dimensions: power of 2 (256x256, 512x512, 1024x1024)
- Seamless on all edges (top-to-bottom, left-to-right)
- No visible seam lines at any scale
- Pattern repeats without visible boundaries
- Pattern works at multiple scales (50%, 100%, 200%)

**Pattern Generation Prompts:**
- Include tiling-specific instructions:
  - "Generate a seamlessly tiling pattern"
  - "Pattern edges must match perfectly for infinite tiling"
  - "No visible seams at tile boundaries"
  - "Pattern works at [specified scale] resolution"

**Pattern Verification:**
- AI output is tested for seamless tiling
- Test method: tile 4x4 grid and inspect for seams
- Verification at multiple zoom levels
- Color and density consistency across tiles

### 5.3 Glassmorphism Considerations

AI-generated glass effect elements follow the Visual Production Bible section 15 specifications:

**Glass Effect Parameters:**
- Background blur: 20-40px blur radius
- Background opacity: 60-80%
- Border accent: 0.5-1px at 20-30% white opacity
- Background color influenced by behind-content, modified by blur and opacity

**AI Glass Generation Rules:**
- Glass effect is applied to specific elements only (cards, sheets, navigation)
- Glass effect is never applied to text-heavy areas
- Glass effect is never the primary background treatment
- Glass overlay maintains readability of content beneath

### 5.4 Performance Optimization

AI-generated backgrounds are performance-optimized for target platforms:

**Resolution Standards:**
- Mobile background: 1125x2436px (iPhone resolution)
- Tablet background: 2048x2732px (iPad resolution)
- Desktop background: 2560x1440px (standard desktop)
- Web background: 1920x1080px (standard web)
- File size: maximum 500KB per background asset

**Detail Constraints:**
- Complex detail is concentrated in focal areas (center, edges)
- Peripheral areas have reduced detail (gradient or simple patterns)
- No high-frequency patterns that cause moire on screen
- No excessive detail that increases file size
- GPU-friendly: no complex shaders in background elements

**Format Standards:**
- Delivery format: WebP (web), JPEG (fallback), PNG (when transparency needed)
- Quality: 80% JPEG quality unless higher needed
- Color space: sRGB

### 5.5 Atmosphere and Mood

AI background generation includes atmospheric elements for depth and immersion:

**Atmospheric Elements:**
- Lighting quality (time of day, direction, warmth)
- Weather effects (clear, overcast, fog, atmospheric haze)
- Particulates (dust, mist, particles appropriate to environment)
- Color temperature (warm for inviting, cool for contemplative)
- Depth atmosphere (foreground clarity, midground softening, background fade)

**Mood Consistency:**
- Mood matches the learning mode (see mode table above)
- Mood is consistent across background and foreground elements
- Mood supports the educational context
- Mood does not distract from content

### 5.6 Educational Context Appropriateness

Backgrounds must be appropriate for the educational context and never distract from content:

**Context Rules:**
- Backgrounds do not distract from learning content
- Backgrounds support content visibility (sufficient contrast)
- Backgrounds are culturally appropriate for the content
- Backgrounds are period-appropriate for historical content
- Backgrounds do not contain competing visual elements

**Educational Background Types:**
- Reading backgrounds: warm, calm, minimal distraction
- Video backgrounds: dark or neutral to frame video content
- Quiz backgrounds: clean, focused, no visual competition
- Interactive backgrounds: engaging but not distracting
- Achievement backgrounds: celebratory, visually rewarding

---

## 6. Infographic Generation

### 6.1 Data Accuracy Preservation

AI-generated infographics must preserve data accuracy above all other considerations:

**Data Integrity Rules:**
- All data points in the infographic must be verified before generation
- AI is provided with structured data (not prose) for accuracy
- Data is provided in machine-readable format (JSON, CSV)
- AI does not infer, approximate, or round data without explicit instruction
- All data visualizations must accurately represent source data

**Data Verification Workflow:**
1. Source data is verified by content team
2. Verified data is structured for AI input
3. AI generates infographic from structured data
4. Data values in infographic are compared to source data
5. Any discrepancy causes automatic rejection
6. Human reviewer confirms data accuracy

### 6.2 Information Hierarchy

Infographics must have a clear hierarchy of information for immediate comprehension:

**Hierarchy Structure:**
- Title: largest, most prominent, immediately visible
- Main statistic or finding: second most prominent
- Supporting data points: clear but subordinate
- Labels and annotations: legible but minimal
- Source citation: present but unobtrusive

**Visual Hierarchy Rules:**
- Size indicates importance (larger = more important)
- Color indicates relationships (same category = same color)
- Position indicates sequence (top-to-bottom or left-to-right)
- Contrast indicates emphasis (higher contrast = more emphasis)
- Whitespace indicates separation (more space = different category)

### 6.3 Style and Color Adherence

Infographics follow platform style and color specifications for visual consistency:

**Style Rules:**
- Flat design style consistent with platform illustration style
- Clean geometric shapes for charts and diagrams
- Consistent icon style for visual elements
- Typography: platform typefaces only
- Line weights: consistent throughout

**Color Rules:**
- Platform palette colors only
- Color used consistently (same data category = same color throughout)
- Maximum 5 colors per infographic (plus neutral backgrounds)
- Data visualization colors from platform data visualization palette
- No colors that could confuse data interpretation

### 6.4 Text Legibility

All text in AI-generated infographics must be legible at all intended display sizes:

**Text Standards:**
- Minimum font size: 12px for labels, 16px for body, 24px for headers
- Contrast ratio: minimum 4.5:1 for body text, 3:1 for large text
- No text on complex backgrounds (use shielding or contrast enhancement)
- No text at angles that reduce readability (0 or 90 degrees only)
- Text not used as decorative element

**Typography Constraints:**
- Platform typefaces only
- Maximum 2 typefaces per infographic
- Typeface usage consistent (header font vs body font)
- No stretched, compressed, or distorted text
- No text effects (shadows, gradients, outlines) unless accessibility requires

### 6.5 Accessibility Requirements

Infographics must be accessible to all users including those with visual impairments:

**Color Blind Safety:**
- Color is not the only means of conveying information
- Patterns and shapes complement color coding
- Data visualization uses color blind safe palettes
- Contrast is sufficient regardless of color vision deficiency

**Screen Reader Compatibility:**
- Data table equivalent provided for all infographic visualizations
- Alt text describes the infographic's key findings
- All text is selectable and readable by screen readers

**WCAG Compliance:**
- WCAG AA minimum for all infographic content
- WCAG AAA preferred for educational content
- Keyboard navigable for interactive infographic elements

### 6.6 Educational Clarity Priority

Educational clarity is prioritized over aesthetic complexity in all infographic generation:

**Clarity Rules:**
- Simple visualizations are preferred over complex ones
- Bar charts over radar charts. Line charts over 3D charts.
- Pie charts limited to 5 segments maximum
- Scatter plots limited to clear patterns, not noise
- Data-to-ink ratio maximized: remove non-data ink

**Clarity Test:**
- Can the key finding be understood within 5 seconds?
- Can the data be compared without confusion?
- Is the hierarchy of information immediately clear?
- Would a learner understand this without additional explanation?

### 6.7 Data Source Citation

Every AI-generated infographic includes source citations for all data points:

**Citation Format:**
- Source name
- Publication date
- URL or document reference
- Accessed date
- Citation placed at bottom of infographic

**Citation Rules:**
- Citations are mandatory for all data points
- Citations are legible (minimum 10px)
- Citations are unobtrusive but present
- Multiple sources listed separately
- AI-generated data (e.g., from AI analysis) is labeled as such

### 6.8 Infographic Generation Prompt Templates

**Standard Infographic Prompt Template:**

Generate an infographic in the platform's flat illustration style.

Title: [infographic title]
Key finding: [primary data point to communicate]
Supporting data: [JSON or structured data]
Data type: [comparison/trend/distribution/relationship/process]

Layout constraints:
- Orientation: [portrait/landscape/square]
- Maximum elements: [number of data points]
- Read direction: [LTR/RTL]

Color palette:
- Primary data color: [hex]
- Secondary data color: [hex]
- Background: [hex]
- Text: [hex]

Style constraints:
- Flat design, clean shapes
- Consistent line weights
- Platform typefaces only
- Maximum 5 colors
- No decorative elements that don't serve data communication

Accessibility constraints:
- Color blind safe palette
- Minimum 12px text, 4.5:1 contrast
- Patterns or shapes alongside color coding
- Source citation at bottom

Cultural constraints:
- [RTL layout if needed]
- [Color meaning considerations for region]

Negative prompts:
- No 3D chart effects
- No decorative elements that distract from data
- No text on complex backgrounds
- No gradients in data visualization
- No overlapping labels

---

## 7. Map Generation

### 7.1 Geographic Accuracy

AI-generated maps must use verified base data for all geographic features:

**Base Data Requirements:**
- Geographic base data from verified sources only
- Approved base data sources: OpenStreetMap verified data, government survey data, academic geographic databases
- AI does not generate geographic features from training data alone
- Coastlines, borders, rivers, and terrain must match verified geographic data

**Accuracy Verification:**
- Generated maps are compared against verified base data
- Coordinate accuracy checked for labeled locations
- Border accuracy checked for historical and modern boundaries
- Terrain accuracy verified against elevation data
- Any geographic error causes automatic rejection

### 7.2 Historical Map Styles

AI-generated historical maps use period-appropriate cartographic styles:

**Period-Specific Styles:**

| Period | Map Style | Coloring | Borders | Labels |
|---|---|---|---|---|
| Pre-Islamic | Hellenistic/Roman cartography | Parchment tones, limited pigment | Simple province boundaries | Latin or Greek place names |
| Early Islamic (7th-9th c.) | Arabic cartographic tradition | Earth tones, gold accents | Regional divisions | Arabic script, Kufic style |
| Golden Age (9th-13th c.) | Islamic geographic tradition | Rich blues, greens, golds | Detailed regional borders | Arabic Naskh, geographic terminology |
| Later Islamic (13th-17th c.) | Ottoman/Persian/Mughal cartography | Varied by region, elaborate coloring | Provincial boundaries | Regional scripts |
| Modern | Contemporary cartographic standards | Platform color palette | Current boundaries | Modern typography |

**Historical Map Constraints:**
- Map projection must be appropriate for the period
- Place names must reflect period-appropriate naming conventions
- Borders must reflect period-appropriate boundaries (not modern)
- Cartographic decorations must be period-appropriate
- No anachronistic cartographic elements

### 7.3 Route and Boundary Animation Generation

AI generates animations of routes and boundary changes for timeline and educational content:

**Route Animation Standards:**
- Route path: smooth, follows actual geography
- Animation speed: 2-5 seconds per route segment
- Path visualization: dotted or solid line, consistent style
- Waypoints: marked at key locations
- Arrival indication: subtle pulse or marker highlight

**Boundary Change Animation Standards:**
- Time period selection: clear date range indication
- Boundary transition: smooth morph between states
- Color change: gradual transition for affected areas
- Label update: synchronized with boundary change
- Pacing: 3-5 seconds per major change

**Animation Technical Specifications:**
- Frame rate: 30fps for map animations
- Resolution: match map resolution (minimum 1920x1080)
- Format: Lottie (for UI integration) or MP4 (for video integration)
- File size: maximum 2MB for Lottie, 10MB for MP4

### 7.4 Terrain Accuracy

AI-generated terrain must be geographically accurate and visually appropriate:

**Terrain Sources:**
- Elevation data from verified sources (SRTM, ASTER GDEM)
- Terrain classification from verified land cover data
- Water body locations from verified hydrographic data
- Vegetation zones from verified climate/ecology data

**Terrain Visualization:**
- Elevation coloring: hypsometric tinting with platform palette
- Contour lines: optional, standard intervals
- Shaded relief: consistent light direction (upper-left)
- Terrain exaggeration: 1.5-2x for visibility (not misleading)
- Resolution appropriate to map scale

### 7.5 City Placement Accuracy

AI-generated maps must place cities accurately according to verified geographic data:

**City Placement Rules:**
- Coordinates from verified geographic data
- City size indicator proportional to historical or modern population
- City symbol consistent with map style and city importance
- City labels legible at all intended zoom levels

**Historical City Placement:**
- Use historical population data for city size indication
- Use historical place names
- Indicate whether city exists today, exists in ruins, or no longer exists
- Show historical trade routes, waterways, and connections

### 7.6 Label Readability

Labels on AI-generated maps must be legible at all zoom levels without overlapping:

**Label Standards:**
- Minimum font size: 10px at intended zoom level
- Font: platform sans-serif for readability
- Color: high contrast against map background
- No label overlap with other labels or map features
- Labels aligned to feature (curved for rivers, horizontal for cities)

**Dynamic Labeling:**
- Labels scale with zoom level
- Important features labeled at all zoom levels
- Secondary features labeled on zoom-in
- Label density controlled to prevent clutter

---

## 8. Animation Generation

### 8.1 Motion Design Compliance

AI-generated animations must follow the Motion Design Bible specifications exactly:

**Motion Source Reference:**
- All AI animation must reference UX_Design/06_Motion_Design_Bible
- Motion principles from Motion Design Bible are enforced in AI generation
- Easing curves match Motion Design Bible specifications
- Duration guidelines match Motion Design Bible specifications
- Choreography rules match Motion Design Bible specifications

**Motion Timing Standards:**
- UI animations: 200-400ms
- Micro-interactions: 100-200ms
- Narrative animations: 1-10 seconds (content-dependent)
- Celebratory animations: 500-2000ms
- Ambient animations: looping, continuous
- Loading animations: 2-5 seconds per cycle

### 8.2 Physics-Based Motion

AI animation must follow physics principles for realistic and natural movement:

**Weight and Momentum:**
- Heavier objects: slower acceleration, slower deceleration, more overshoot
- Lighter objects: faster acceleration, faster deceleration, less overshoot
- Mass-consistent motion throughout an object's animation
- No unnatural instant stops or starts

**Physics Parameters:**
- Gravity: consistent throughout scene
- Friction: appropriate to surface/material
- Bounce: energy-consistent with material properties
- Air resistance: appropriate to object size and speed
- Collision: physically plausible responses

**Physics Constraint Prompts:**
- "Physics: realistic weight and momentum"
- "Motion follows physics: gravity 9.8m/s squared, friction appropriate to surface"
- "No floating, no instant stops, no unnatural acceleration"

### 8.3 Character Animation Standards

AI character animation must be natural and consistent with rigging specifications:

**Natural Movement:**
- Walk cycles: consistent stride, arm swing, weight shift
- Idle animation: subtle breathing, micro-movements, natural sway
- Gesture animation: purposeful, conversational, culturally appropriate
- Facial animation: natural expression transitions, no uncanny valley

**Rigging Consistency:**
- Animations must match the character's rigging specifications
- Joint limitations respected (no hyperextension)
- Deformation consistent with skinning
- No clipping, no intersecting geometry

**Character Animation Types:**
- Idle: subtle movement, breathing, micro-expressions
- Walk: standard locomotion, variant speeds
- Gesture: specific communicative movements
- Expression: emotion-driven facial animation
- Interaction: object manipulation, tool use

### 8.4 Environmental Animation

AI-generated environmental animation is subtle, looping, and atmospheric:

**Atmospheric Animation:**
- Light: subtle shifts, cloud shadows, time-of-day transitions
- Weather: gentle rain, drifting fog, moving clouds
- Vegetation: wind through leaves, grass movement, flower opening
- Water: gentle flow, ripple, reflection movement

**Looping Requirements:**
- All environmental animations loop seamlessly
- No visible loop point
- Minimum loop duration: 5 seconds (shorter loops are noticeable)
- Loop variation: random parameters for longer cycles

**Performance Constraints:**
- Environmental animations are GPU-optimized
- Particle count limited to system performance specifications
- Animation resolution matches display resolution
- No animation that degrades interaction performance

### 8.5 Particle Effects

AI-generated particle effects must stay within performance budgets while being visually effective:

**Particle System Specifications:**
- Maximum particles per system: 200 (mobile), 500 (desktop)
- Particle lifetime: 1-5 seconds
- Particle size: 2-20px
- Particle speed: appropriate to effect type
- Particle color: platform palette

**Effect Types:**
- Ambient: gentle floating particles, dust motes, light sparkles
- Celebratory: burst particles, confetti, sparkle trails
- Achievement: glow particles, rise effect, golden sparkles
- Atmospheric: fog particles, mist, dust
- Interactive: ripple particles, touch effects, hover particles

**Performance Budgeting:**
- Particle effects are performance-tested on target devices
- Effects that cause frame drops are simplified or removed
- Reduced motion setting disables all particle effects

### 8.6 Style Consistency with Animation Library

AI-generated animations must match the existing animation library for visual coherence:

**Animation Library Standards:**
- Animation style reference library maintained for AI training
- Keyframe structure consistent with library animations
- Easing curves match library standards
- Timing matches library standards
- Visual treatment matches library standards

**Animation Types in Library:**
- Transition animations (screen-to-screen)
- Micro-interaction animations (button, toggle, scroll)
- Loading animations (progress, spinner, skeleton)
- Achievement animations (badge, reward, level-up)
- Ambient animations (background, environment, particle)

---

## 9. Video Generation

### 9.1 Quality Standards

AI-generated video must meet minimum quality requirements for all educational content:

**Resolution and Frame Rate:**
- Minimum resolution: 1080p (1920x1080)
- Standard resolution: 4K (3840x2160) for hero content
- Frame rate: 24fps minimum, 30fps standard, 60fps for motion-intensive content
- Bitrate: 15Mbps minimum for 1080p, 45Mbps for 4K
- Color depth: 8-bit minimum, 10-bit preferred

**Video Technical Specifications:**
- Codec: H.264 (delivery), H.265/HEVC (archive), ProRes (master)
- Color space: Rec. 709 (SDR), Rec. 2020 (HDR)
- Audio: AAC, 48kHz, 16-bit minimum
- File format: MP4 (delivery), MOV (master)

### 9.2 Style Consistency

AI-generated video must be consistent with the platform's art direction and visual identity:

**Visual Style:**
- Color grading matches platform palette
- Lighting follows platform lighting philosophy
- Composition follows platform composition rules
- Typography uses platform typefaces
- Graphics and overlays match design system

**Style Reference:**
- Style reference videos provided to AI for each content type
- Color grade LUT provided for consistent color treatment
- Lower third templates provided for consistent graphics
- Transition style references provided for consistent editing

### 9.3 Historical Reconstruction Accuracy

AI-generated historical reconstructions must be accurate and research-based:

**Accuracy Requirements:**
- Architecture: period-appropriate, research-based
- Clothing: period-appropriate, region-appropriate, social-class-appropriate
- Objects: historically accurate, verified against references
- Lighting: period-appropriate light sources
- Sound: period-appropriate ambient sounds

**Research Verification:**
- All historical reconstructions based on verified references
- References documented and attached to generation request
- Historical consultant reviews before approval
- Accuracy score: minimum 90% on historical accuracy checklist

### 9.4 Text Rendering

AI-generated video must not contain garbled or incorrect text within frames:

**Text Rendering Rules:**
- Text is rendered as separate overlay, not embedded in AI video generation
- AI does not generate text within video frames
- All text is added in post-production using platform typography
- This prevents garbled text, misspellings, and incorrect characters common in AI video

**Post-Production Text:**
- Lower thirds: consistent template, correct typography
- Titles and headers: proper typography, correct Arabic rendering
- Subtitles: properly formatted, timed correctly, accessibility compliant
- Callouts and annotations: consistent style, correct positioning

### 9.5 Lip Sync Accuracy

AI character dialogue must have accurate lip synchronization with audio:

**Lip Sync Standards:**
- Phoneme accuracy: 90% minimum
- Timing synchronization: within 2 frames of audio
- Expression synchronization: appropriate emotion matching dialogue tone
- No visible mismatch between audio and lip movement

**Language-Specific Lip Sync:**
- Arabic: phoneme mapping for Arabic speech sounds
- Arabic: emphasis on accurate consonant articulation (Arabic has more consonants than most languages)
- Other languages: phoneme mapping per language
- Multi-language: character's lip sync language matches dialogue language

### 9.6 Cultural Sensitivity in Video

AI-generated video scenes must pass cultural sensitivity review before approval:

**Sensitivity Requirements:**
- All scenes reviewed for cultural appropriateness
- Religious contexts treated with respect
- Historical events depicted with sensitivity to modern sensibilities
- No stereotypical or reductive depictions of cultures
- Gender representations appropriate to cultural and historical context

**Sensitive Content Flagging:**
- AI automatically flags scenes with: religious content, cultural practices, historical trauma, political content
- Flagged content requires mandatory human review
- Cultural advisor review required for flagged content

### 9.7 Quality Control Equivalence

AI-generated video must pass the same QC standards as produced video:

**QC Requirements Identical to Produced Video:**
- Visual quality
- Audio quality
- Content accuracy
- Cultural sensitivity
- Technical specifications
- Accessibility compliance

---

## 10. 3D Model Generation

### 10.1 Poly Count Budget Enforcement

AI-generated 3D models must adhere to poly count budgets based on asset tier:

**Tier-Based Budgets:**

| Tier | Classification | Max Triangles | LODs |
|---|---|---|---|
| Tier 1 | Hero assets | 50,000 tris | 4 LODs |
| Tier 2 | Important assets | 20,000 tris | 4 LODs |
| Tier 3 | Standard assets | 8,000 tris | 3 LODs |
| Tier 4 | Background assets | 3,000 tris | 2 LODs |
| Tier 5 | Distant assets | 500 tris | 1 LOD |

**Budget Enforcement:**
- AI model generation reports triangle count
- Automatic rejection if triangle count exceeds tier budget by more than 10%
- Optimization required if within 10% over budget
- LOD budgets enforced separately

### 10.2 Clean Topology Requirements

AI-generated 3D models must have clean, production-ready topology:

**Topology Standards:**
- Quad-based topology (triangles converted for runtime)
- No n-gons (polygons with more than 4 sides)
- Edge flow follows form: loops around features (eyes, mouth, joints)
- No poles with more than 5 edges
- No degenerate faces (zero-area faces)
- No duplicate vertices or overlapping geometry

**Modeling Guidelines:**
- Clean edge loops for deformation areas (joints, facial expressions)
- Consistent polygon density: higher detail in focal areas, lower in hidden areas
- Support loops at hard edges (bevels, transitions)
- No internal faces
- Watertight mesh for 3D printing preparation

### 10.3 UV Mapping Standards

AI-generated 3D models must have proper UV mapping for texture application:

**UV Requirements:**
- Non-overlapping UVs (no overlapping islands)
- UV island packing: maximize texture space usage (80% minimum)
- Consistent texel density across all islands
- Seam placement in hidden or less visible areas
- UV padding: 2px minimum between islands

**UV Resolution Matching:**
- UV island size proportional to texture importance
- Face features: larger UV space
- Hidden areas: smaller UV space
- Texel density consistent across connected assets

### 10.4 Texture Resolution Compliance

AI-generated 3D model textures must meet resolution standards by tier:

**Texture Resolution by Tier:**

| Tier | Albedo | Normal | ORM | Max Memory |
|---|---|---|---|---|
| Tier 1 | 2048x2048 | 2048x2048 | 2048x2048 | 32MB |
| Tier 2 | 1024x1024 | 1024x1024 | 1024x1024 | 8MB |
| Tier 3 | 1024x1024 | 512x512 | 512x512 | 4MB |
| Tier 4 | 512x512 | 256x256 | 256x256 | 1MB |
| Tier 5 | 256x256 | None | None | 0.25MB |

**Texture Parameters:**
- Power-of-two dimensions required
- sRGB for albedo and emission
- Linear space for normal and ORM
- No visible compression artifacts
- Consistent quality across all maps

### 10.5 Material Assignment Consistency

AI-generated 3D models must have consistent material assignments following PBR workflows:

**Material Standards:**
- PBR material workflow (base color, normal, ORM)
- Consistent material naming convention
- No duplicate materials (merge identical materials)
- Material instances preferred over unique materials
- Maximum 3 materials per standard asset, 5 per hero asset

**Material Properties:**
- Base color: platform palette values
- Roughness: physically appropriate for material type
- Metalness: correct metal/non-metal assignment
- Normal: appropriate surface detail intensity

### 10.6 LOD Generation

AI generates LOD (Level of Detail) variants for all Tier 1-4 assets:

**LOD Reduction Rules:**
- LOD1: 50% of base triangle count
- LOD2: 20% of base triangle count
- LOD3: 5% of base triangle count
- LOD4 (Tier 1-2 only): Billboard impostor

**LOD Quality:**
- LOD0 and LOD1: no visible quality difference at normal viewing distance
- LOD2: minor simplification acceptable
- LOD3: flat shading with baked detail acceptable
- LOD4: billboard with rendered impostor texture

### 10.7 File Format Compliance

AI-generated 3D models must be delivered in correct formats for the target pipeline:

**Format Specifications:**
- Authoring: FBX 2020+ (binary, embedded media)
- Runtime (Unity): .asset/.prefab (Unity 2022 LTS+)
- Runtime (Web): .glb/.gltf (glTF 2.0 with Draco compression)
- Runtime (AR): .usdz (USDZ 0.8+)
- Source: .blend (Blender 3.6+) or .mb (Maya 2024+)

**Export Settings:**
- Forward axis: -Z
- Up axis: +Y
- Units: meters
- Scale: 1.0
- Transforms: baked before export
- No vertex color data (unless specifically required)

### 10.8 Integration Testing

AI-generated 3D assets require integration testing before they enter the production pipeline:

**Integration Test Requirements:**
- Load in target engine (Unity for mobile, Three.js for web)
- Verify material rendering
- Verify animation playback
- Verify collision detection
- Verify LOD transitions
- Performance test on target devices
- Memory usage verification

**Test Documentation:**
- Test results documented per asset
- Failed tests require fix and retest
- Integration approved by technical artist

---

## 11. Sound Effects Generation

### 11.1 Technical Specifications

AI-generated sound effects must meet strict technical specifications for audio quality:

**Sample Rate and Bit Depth:**
- Sample rate: 48kHz (standard)
- Sample rate: 96kHz (for high-fidelity ambient sounds)
- Bit depth: 24-bit
- No sample rate conversion artifacts

**Loudness Standard:**
- Target loudness: -16 LUFS (integrated)
- Loudness range: 10-15 LU (appropriate for content type)
- True peak: -2dBTP maximum
- No clipping, no limiting artifacts

**Format Specifications:**
- Master format: WAV (48kHz, 24-bit, uncompressed)
- Delivery format: AAC (256kbps, 48kHz, 24-bit)
- Archive format: WAV (96kHz, 24-bit if available)
- Metadata: embedded BWF (Broadcast Wave Format) with generation metadata

### 11.2 Sound Quality Requirements

AI-generated sound effects must be artifact-free and production-ready:

**Quality Standards:**
- No audible artifacts (clicks, pops, distortion, noise)
- Clean transients: attack is clear and natural
- No unnatural resonances or filtering artifacts
- No audible loop points (for looping sounds)
- Frequency response: full range (20Hz-20kHz) appropriate to content

**Quality Verification:**
- Visual inspection of waveform (check for DC offset, clipping, noise floor)
- Spectral analysis (check for artifacts, unwanted frequencies)
- Critical listening on studio monitors and headphones
- A/B comparison with reference sounds

### 11.3 Cultural Appropriateness

AI-generated sound effects must be culturally appropriate for their context:

**Sound Selection Rules:**
- Sounds match the cultural context of the content
- No sounds that could be culturally insensitive
- Instrument sounds appropriate to region and period
- Ambient sounds appropriate to location and culture

**Cultural Sound Library:**
- Curated library of culturally appropriate sounds for AI reference
- Each sound tagged with cultural context metadata
- AI constrained to use approved cultural sound references
- Cultural advisor review of sound selections

### 11.4 UI Sound Specifications

AI-generated UI sounds must meet interaction design requirements for feedback:

**UI Sound Parameters:**
- Duration: less than 1 second (most under 500ms)
- Informative: conveys state change or feedback
- Pleasant: non-jarring, harmonious frequencies
- Consistent: same sound family across related interactions
- Non-repetitive: variations in micro-adjustments

**UI Sound Types:**
- Tap/click: short, tactile, confirming
- Toggle: two-state indication (on/off)
- Success: positive, ascending pitch
- Error: gentle, informative, not alarming
- Notification: attention-getting but not disruptive
- Transition: smooth, continuous, directional

### 11.5 Ambient Sound Specifications

AI-generated ambient sounds must be suitable for continuous background playback:

**Looping Requirements:**
- Seamless loop: no audible loop point
- Minimum loop duration: 10 seconds (longer for nature ambients)
- Loop variation: subtle randomization for natural feel
- Phase cancellation check: no frequency cancellation at loop point

**Non-Distracting Requirement:**
- Ambient sounds do not compete with narration or primary audio
- Frequency spectrum avoids narration frequency range (300Hz-3kHz)
- Volume: -20dB relative to narration (-36 LUFS ambient level)
- Dynamic range: limited (2-6dB) for consistent background

---

## 12. Narration Generation

### 12.1 Voice Quality Standards

AI-generated narration must meet natural speech quality standards for educational content:

**Natural Prosody:**
- Sentence-level intonation: appropriate pitch variation
- Phrase grouping: natural breath groups and pauses
- Emphasis: correct word stress for meaning
- Pacing: appropriate speaking rate (150-170 words per minute for Arabic, 160-180 for English)
- No monotone delivery, no robotic quality

**Emotion Modeling:**
- Tone matches content type (authoritative for educational, warm for storytelling, encouraging for learning feedback)
- Emotional range: appropriate expression without melodrama
- Consistent emotional tone within a single narration

**Voice Consistency:**
- Same voice across all content for the same character/narrator
- Consistent tone, pacing, and character across sessions
- Voice parameters (pitch, speed, timbre) locked per character

### 12.2 Arabic Pronunciation Standards

AI Arabic narration must follow strict pronunciation guidelines for educational accuracy:

**Tajweed Standards for Quranic Content:**
- AI does not generate Quranic recitation (human reciters only)
- AI may generate non-Quranic Arabic using correct MSA (Modern Standard Arabic) pronunciation
- Makharij (points of articulation): correct for all Arabic letters
- Sifaat (characteristics of letters): correct application
- Emphasis on difficult letters: ain, ghain, qaf, dad, zha, haa

**Classical Arabic Pronunciation:**
- Case endings (i'rab): properly pronounced in classical contexts
- Tanween: correct nunation
- Shadda: correct gemination
- Madd: correct vowel lengthening (2, 4, or 6 counts as appropriate)

**Modern Standard Arabic:**
- Clear, educated MSA pronunciation
- Correct stress patterns
- No dialectal interference (Egyptian, Levantine, Gulf dialects avoided for formal content)
- Pausal forms correctly applied

### 12.3 Arabic Term Pronunciation

AI must correctly pronounce all Arabic terms and names from a curated library:

**Name Pronunciation Library:**
- Curated pronunciation library for historical figures, places, and terms
- Each entry includes: written form, transliteration, IPA pronunciation guide
- AI references this library for all name and term pronunciations

**Common Pronunciation Challenges:**
- Differentiated pronunciation of zha vs dad vs zay
- Correct pronunciation of taa marboota in different contexts
- Hamzat al-wasl vs hamzat al-qat'
- Sun and moon letters: correct assimilation of al-
- Definite article pronunciation: clear al- not slurred

### 12.4 Voice Consistency

AI voices must be consistent across all content for the same character:

**Character Voice Profiles:**
- Each character/narrator has a locked voice profile
- Profile includes: base pitch, pitch range, speaking rate, timbre, emotional baseline
- Profile is applied to all generations for that character
- Profile changes require production director approval

**Voice Matching:**
- New AI voices matched to existing voice profiles
- Voice characteristics verified against profile
- A/B comparison with previous recordings
- Consistency check across multiple sessions

### 12.5 Emotional Range

AI narration must convey appropriate emotion for the content type:

**Content-Type Emotional Mapping:**

| Content Type | Primary Emotional Tone | Secondary Tone | Avoid |
|---|---|---|---|
| Historical narrative | Authoritative, reverent | Warm, engaged | Casual, flippant |
| Educational explanation | Clear, patient | Encouraging | Condescending, rushed |
| Storytelling (narrative) | Expressive, warm | Dramatic (appropriate) | Monotone, flat |
| Learning feedback | Encouraging, positive | Warm, supportive | Robotic, indifferent |
| Achievement celebration | Joyful, celebratory | Warm, proud | Subdued, flat |
| Guided meditation | Calm, soothing | Gentle, warm | Energetic, rushed |
| Quiz/assessment | Neutral, clear | Encouraging | Leading, suggestive |

### 12.6 AI Voice Distinguishability

AI voices must be clearly distinguishable from human voices through labeling:

**Labeling Requirement:**
- AI narration is labeled as AI-generated in content metadata
- Users are informed when listening to AI-generated narration
- AI voices are not marketed as or implied to be human voices

**Voice Style Distinction:**
- AI voices may have a slight but pleasant distinctiveness from human voices
- AI voices prioritize clarity over perfect human mimicry
- The goal is excellent communication, not deception

### 12.7 Multi-Language Support

AI narration supports all platform languages with native-quality accent:

**Language Support Rules:**
- Each language uses native speakers for voice modeling
- Accent: standard educated accent for the language (MSA for Arabic, Received Pronunciation or General American for English)
- No foreign-accented delivery in any language
- Language switching within content: smooth, appropriate pause

**Language-Specific Requirements:**
- Arabic: MSA with correct regional variations only when content requires
- English: consistent accent throughout (RP or General American, not mixed)
- Other languages: native-quality delivery verified by native speaker

---

## 13. Historical Accuracy in AI

### 13.1 Constraint to Verified Data

AI must be constrained to historically verified data for all historical content:

**Data Sources:**
- AI generation uses only verified historical data sources
- Approved sources: academic publications, museum catalogs, archaeological reports, primary source documents
- No generation from unverified internet sources
- Source verification status recorded with each generation

**Constraint Mechanism:**
- Historical content prompts include specific source references
- AI generates only from provided reference materials
- No historical interpolation or invention
- Gap indication: if reference data is insufficient, AI flags the gap rather than inventing

### 13.2 Prompt Engineering for Accuracy

Historical prompts must include specific accuracy parameters for verified output:

**Prompt Requirements:**
- Specific year or year range
- Specific region with geographic coordinates
- Specific dynasty or ruling period
- Specific cultural context
- Verified reference images attached

**Accuracy Prompt Template:**

Generate a historically accurate [scene type] from [specific year/period].

Historical parameters:
- Period: [specific dynasty/caliphate/era]
- Year: [specific year or range]
- Region: [specific region, modern coordinates if applicable]
- Location: [specific city/site]
- Event: [specific event if applicable]

Accuracy constraints:
- Architecture: [period-specific references attached]
- Clothing: [period and region-specific references attached]
- Objects: [period-appropriate references attached]
- Color palette: [period-appropriate palette reference]
- Lighting: [period-appropriate light sources only]

Reference sources:
- [Source 1: academic publication]
- [Source 2: museum catalog]
- [Source 3: archaeological reference]

Negative prompts:
- No anachronistic elements from other periods
- No generically "Islamic" or "Middle Eastern" styling (be specific)
- No modern construction techniques or materials
- No modern lighting

### 13.3 Cross-Reference with Historical Database

AI-generated historical content must cross-reference against the platform's historical database:

**Cross-Reference Requirements:**
- Dates verified against historical database
- Locations verified against historical geography
- Figures verified against historical records
- Events verified against multiple sources
- Any discrepancy flagged for human review

**Database Integration:**
- AI generation system connected to historical database
- Automatic cross-referencing during generation
- Confidence score per element
- Low-confidence elements flagged for human verification

### 13.4 Anachronism Detection

AI must flag potential anachronisms for human review and correction:

**Automated Anachronism Detection:**
- Object detection: flag objects that don't match period inventory
- Architecture detection: flag architectural styles from wrong periods
- Clothing detection: flag clothing from wrong periods or regions
- Technology detection: flag technology that didn't exist in the period
- Material detection: flag materials not available in the period

**Detection Workflow:**
1. AI generates asset with accuracy constraints
2. Automated anachronism detection scans the asset
3. Potential anachronisms are flagged with bounding boxes
4. Human reviewer assesses each flag
5. Confirmed anachronisms are corrected
6. False positives are logged for system improvement

### 13.5 Period-Specific Detail Enforcement

AI must enforce period-specific details for architecture, clothing, and objects:

**Architecture Enforcement:**
- Building types: only types that existed in the period
- Architectural styles: period-appropriate
- Construction materials: historically accurate
- Decorative elements: period-appropriate patterns and motifs
- Scale and proportion: historically accurate

**Clothing Enforcement:**
- Garment types: period and region appropriate
- Fabrics and materials: historically accurate
- Colors: derived from period-available dyes
- Social class differentiation: appropriate to depicted status
- Occasion-appropriate attire (battle, court, daily life, ceremony)

**Object Enforcement:**
- Object types: only those existing in the period
- Materials: historically accurate
- Craftsmanship: period-appropriate techniques
- Usage: correct functional context
- No objects from later periods

### 13.6 Prohibition on Invention

AI is not allowed to invent historical details when reference data is insufficient:

**Invention Prevention:**
- AI must use provided references for all historical details
- If a detail is not in the reference, AI must not generate it
- AI must flag missing details rather than inventing them
- "Best guess" generation is prohibited

**Handling Information Gaps:**
- If reference material is insufficient for a complete scene:
  1. AI generates only the elements with sufficient reference
  2. Remaining area uses appropriate period-appropriate background treatment
  3. Human artist fills in missing details based on additional research
  4. Gap is documented in the asset metadata

---

## 14. Educational Integrity in AI

### 14.1 Learning Objective Alignment

AI-generated content must support defined learning objectives:

**Objective Integration:**
- Every asset has a documented learning objective
- AI generation parameters include the learning objective
- Generated assets evaluated against learning objective
- Any asset that doesn't support the objective is rejected

**Learning Objective Types:**
- Knowledge: recall facts, concepts, terminology
- Comprehension: explain ideas, interpret information
- Application: use information in new situations
- Analysis: draw connections between ideas
- Synthesis: combine elements into a new pattern
- Evaluation: justify decisions, critique information

### 14.2 Factual Accuracy Enforcement

AI factual accuracy is enforced through knowledge graph integration:

**Knowledge Graph Integration:**
- Platform knowledge graph serves as the factual reference
- AI must cross-reference all factual claims against the knowledge graph
- Knowledge graph is curated and maintained by the content team
- Any fact not in the knowledge graph requires verification

**Accuracy Verification:**
- Automated fact-checking against knowledge graph
- Human verification of all factual claims
- Source citation attached to each claim
- Accuracy confidence score per asset

### 14.3 Age-Appropriate Content Filtering

AI-generated content must be filtered for age-appropriateness:

**Age Level Parameters:**
- Content complexity: matched to learner's age and level
- Vocabulary: age-appropriate language
- Visual detail: appropriate for cognitive development
- Emotional content: appropriate for age group
- Abstract concepts: introduced at developmentally appropriate stages

**Age Group Specifications:**

| Age Group | Content Characteristics | Visual Style | Complexity Level |
|---|---|---|---|
| 6-8 years | Simple concepts, concrete examples | Bright, playful, high visual interest | Basic |
| 9-12 years | Broader concepts, some abstraction | Rich but clear illustration | Intermediate |
| 13-15 years | Abstract concepts, connections | Sophisticated visualization | Advanced |
| 16-18 years | Complex analysis, evaluation | Professional-level visuals | Expert |

### 14.4 Complexity Level Matching

AI content complexity must match the learner's level:

**Level Adaptation:**
- Content is generated at the learner's current level
- Complexity increases as the learner progresses
- Multiple complexity versions may be generated for the same concept
- AI tailors explanations and visuals to the appropriate level

**Content Differentiation:**
- Same concept, different levels:
  - Basic: simple explanation, concrete example, friendly illustration
  - Intermediate: detailed explanation, real-world application, informative diagram
  - Advanced: comprehensive analysis, research connections, expert visualization

### 14.5 No Misleading Content

AI must never generate misleading or oversimplified content:

**Accuracy Principles:**
- Complexity is acknowledged, not hidden
- Uncertainty is stated, not glossed over
- Multiple perspectives are presented where appropriate
- Simplifications are labeled as simplifications
- No false certainty

**Simplification Guidelines:**
- Simplifications for educational purposes are acceptable when:
  - Labeled as simplified
  - The core concept remains accurate
  - More complex explanation is available for deeper learning
  - The simplification does not create misconceptions

### 14.6 Educational Review

All AI-generated educational content requires educational review:

**Review Requirements:**
- Educational content expert reviews all AI-generated learning materials
- Review covers: factual accuracy, age-appropriateness, learning objective alignment
- Review is documented with pass/fail and recommendations
- Approved educational content is tagged with reviewer ID

---

## 15. Style Consistency

### 15.1 Style Reference Database

AI must maintain consistent style through a curated reference database:

**Database Structure:**
- Style reference images organized by: asset type, content type, period, region, mood
- Each reference tagged with: style parameters, palette, lighting setup, composition notes
- References updated when style guidelines evolve
- Database version-tracked for consistency

**Reference Usage Rules:**
- Every AI generation must reference at least one approved style image
- The reference ID must be included in the generation prompt
- Multiple references can be combined for complex assets
- References constrain AI output to the established style

### 15.2 Style Drift Detection

Automated style drift detection protects visual consistency across all AI output:

**Detection Methods:**
- Automated comparison of AI output against style reference database
- Color palette analysis: check colors against approved palette
- Composition analysis: check composition rules adherence
- Detail level analysis: check detail consistency
- Style embedding comparison: vector-based style similarity

**Drift Thresholds:**
- Color drift: >10% of pixels outside palette = flag
- Composition drift: deviation from composition rules = flag
- Style similarity: <85% similarity to reference = flag
- Detail drift: inconsistent detail level = flag

**Drift Correction:**
- Flagged asset is reviewed by art director
- Prompt is refined to correct style drift
- Model may need fine-tuning if drift is systematic
- Drift incidents logged for trend analysis

### 15.3 Regular Style Audits

AI output undergoes regular style audits to maintain consistency:

**Audit Schedule:**
- Weekly: automated style audit of all AI-generated assets
- Monthly: human style audit of random sample (10%)
- Quarterly: comprehensive style audit of all AI assets
- Post-model-update: full style audit after any model change

**Audit Documentation:**
- Audit results recorded per asset
- Audit findings compiled into style consistency report
- Trends identified and addressed
- Audit history maintained for reference

### 15.4 AI Retraining and Fine-Tuning

AI models are retrained or fine-tuned when style evolves:

**Retraining Triggers:**
- Style guide update: major visual identity changes
- Drift accumulation: systematic drift detected in audits
- New asset types: new categories not well represented in current model
- Quality plateau: generation quality stagnating
- Model improvement: new model versions with significant improvements

**Retraining Process:**
1. Style update documented and approved
2. New reference library compiled
3. Training data curated from approved assets
4. Model fine-tuned on new data
5. Rigorous testing against style checklist
6. A/B comparison with previous model
7. Approval required before deployment
8. Post-deployment monitoring for 2 weeks

### 15.5 Platform-Specific LoRA and Checkpoint Usage

Different asset types may use platform-specific LoRA or checkpoint models:

**LoRA Usage:**
- Character style LoRA: ensures consistent character appearance
- Environment style LoRA: ensures consistent environment rendering
- Period-specific LoRA: ensures historical accuracy per period
- Pattern style LoRA: ensures consistent decorative pattern generation

**Checkpoint Management:**
- Approved checkpoints documented and version-controlled
- Checkpoint selection based on asset type requirements
- Checkpoint performance tracked and reviewed
- Outdated checkpoints retired and replaced
- New checkpoints tested against full style checklist before approval

---

## 16. Prompt Engineering Guide

### 16.1 General Prompt Engineering Principles

All AI asset generation follows these foundational prompt engineering principles:

**Specificity:**
- Be specific about what you want
- Include concrete parameters (colors, dimensions, styles)
- Reference specific style images
- Avoid vague descriptors

**Structure:**
- Use structured prompt templates for consistency
- Separate sections with clear headers
- Place constraints in logical order
- End with negative prompts

**Iteration:**
- Start broad, refine specific
- Generate, review, refine, regenerate
- Document what works and what doesn't
- Build a prompt library from successful prompts

**Constraints:**
- Always include style constraints
- Always include technical constraints
- Always include negative prompts
- Always include cultural/accuracy constraints

### 16.2 Asset Type Prompt Templates

**Illustration Prompt Template:**

Generate a [type] illustration in the platform's flat-with-depth style.

Subject: [description]
Setting: [period, region, location]
Composition: [focal point, perspective]
Mood: [emotion or atmosphere]
Color palette: [palette reference]
Lighting: [light source and quality]

Style constraints: [flat geometric shapes, warm palette, layered depth, simple shadows, clean highlights, 2px line weight]

Negative prompts: [no photorealistic, no gradients, no complex shading, no anachronisms, no text]

Style reference: [reference image ID]

**Icon Prompt Template:**

Generate a [icon_name] icon in the unified icon style.

Grid: 24x24, Stroke: 2px, Caps: rounded, Joins: rounded
Color: [hex], State: [default/active/disabled]
Semantic meaning: [description]

Constraints: [pixel-perfect, consistent stroke, no fills (default), no text]

RTL: [yes/no, provide mirrored version if yes]

Negative prompts: [no thin strokes, no complex details, no text]

**Background Prompt Template:**

Generate a [mode] background environment.

Mode: [story/documentary/knowledge tree/memory garden/AI/achievement/community/quiz]
Mood: [specified mood]
Dominant colors: [palette reference]
Atmosphere: [lighting, weather, particulates]

Constraints: [performance optimized, no competing elements, appropriate for educational context, seamless tiling if pattern]

**Infographic Prompt Template:**

Generate an infographic in flat illustration style.

Title: [title]
Key finding: [main data point]
Data: [structured data]
Layout: [orientation, read direction]

Style: [flat design, clean shapes, platform typefaces, max 5 colors]
Accessibility: [color blind safe, minimum 12px text, 4.5:1 contrast, source citation]

Negative prompts: [no 3D charts, no decorative distractions, no overlapping labels]

**Map Prompt Template:**

Generate a [period/style] map of [region/location].

Period: [specific era]
Accuracy: [verified base data required]
Projection: [period-appropriate or modern]
Features: [cities, borders, terrain, routes, labels]

Style: [period-appropriate cartographic style, platform palette]
Labels: [legible at all zoom levels, period-appropriate place names]

Negative prompts: [no anachronistic borders, no modern place names (for historical), no generic styling]

**Animation Prompt Template:**

Generate a [animation type] animation.

Duration: [timing specification]
Easing: [easing curve reference]
Motion: [physics-based, weight specification]
Style: [consistent with animation library]

Constraints: [looping for ambient, performance optimized, reduced motion alternative]

**3D Model Prompt Template:**

Generate a [tier] [asset type] 3D model.

Poly budget: [triangle count limit]
Topology: [clean quads, edge flow]
UVs: [non-overlapping, consistent texel density]
Textures: [resolution per map type]
Materials: [PBR workflow, platform palette]

LODs: [generate LOD variants]
Format: [FBX, glTF, or USDZ as required]

Negative prompts: [no n-gons, no overlapping UVs, no degenerate faces]

**Sound Effect Prompt Template:**

Generate a [sound type] sound effect.

Duration: [timing]
Sample rate: 48kHz, Bit depth: 24-bit
Loudness: -16 LUFS, True peak: -2dBTP max
Format: WAV master, AAC delivery

Quality: [no artifacts, clean transients, no audible loop points]

Cultural: [culturally appropriate, region-specific if applicable]

**Narration Prompt Template:**

Generate narration for [content type] in [language].

Voice: [character/narrator voice profile]
Tone: [content-appropriate emotional tone]
Pacing: [appropriate speaking rate]
Pronunciation: [correct MSA for Arabic, standard accent for language]

Constraints: [natural prosody, correct term pronunciation, voice consistency, no robotic quality]

### 16.3 Negative Prompt Library

**Universal Negative Prompts (all asset types):**
- No watermarks, signatures, or artist marks
- No text (added separately in post-production)
- No distorted or unnatural elements
- No artifacts, noise, or compression errors
- No inconsistent style elements
- No elements outside the approved palette

**Illustration Negative Prompts:**
- No photorealistic rendering
- No 3D or CGI style
- No complex shading or soft shadows
- No gradients (flat color only)
- No cluttered composition
- No unclear focal point
- No distorted anatomy or proportions
- No inconsistent lighting or multiple light sources
- No oversaturated or neon colors

**Historical Asset Negative Prompts:**
- No modern clothing, hairstyles, or accessories
- No modern buildings or architecture
- No electric lighting or modern technology
- No anachronistic objects from wrong periods
- No incorrect historical uniforms or armor
- No modern political boundaries
- No generic cultural styling (be specific to period and region)

**Icon Negative Prompts:**
- No thin or variable stroke widths
- No complex details illegible at 24px
- No text characters within the icon
- No gradients, shadows, or 3D effects
- No photorealistic elements

**3D Model Negative Prompts:**
- No n-gons or degenerate geometry
- No overlapping UVs or texture seams in visible areas
- No duplicate vertices or floating geometry
- No incorrect material assignments
- No poly count exceeding tier budget

**Audio Negative Prompts:**
- No audible artifacts, clicks, or pops
- No distortion or clipping
- No unnatural resonances
- No audible loop points
- No hiss, hum, or background noise

### 16.4 Style Reference Inclusion

Every prompt must include at least one approved style reference:

**Reference Types:**
- Style reference image: exact visual style to match
- Color palette reference: approved color combination
- Lighting reference: approved lighting setup
- Composition reference: approved layout approach
- Character reference: approved character design

**Reference Format:**
- Reference image file name or ID
- Brief description of what the reference shows
- Which aspect to match (style, color, lighting, composition)
- Any deviations from the reference (with approval)

**Reference Management:**
- References stored in centralized style reference database
- References version-controlled and audited
- Expired references retired and replaced
- New references added through approval workflow

### 16.5 Color Palette Constraints

Color prompts must reference the platform's design system palette:

**Palette Integration:**
- Use design system token names in prompts
- Include hex values for specific colors
- Reference period-specific palettes for historical content
- Reference mode-specific palettes for backgrounds

**Color Constraint Language:**
- "Use only colors from the approved platform palette"
- "Primary color: [token or hex], Secondary: [token or hex], Accent: [token or hex]"
- "Maximum [number] colors in the composition"
- "No colors outside the specified palette"

### 16.6 Composition Guidelines for Prompts

Composition instructions must be included in all illustration prompts:

**Composition Parameters:**
- Focal point position (rule of thirds, center, dynamic)
- Perspective (eye-level, bird's-eye, three-quarter)
- Depth layers (foreground, midground, background)
- Leading lines (architectural, natural, directional)
- Balance (symmetrical, asymmetrical, radial)

**Composition Constraint Language:**
- "Primary subject at rule of thirds intersection [top-left, top-right, bottom-left, bottom-right]"
- "Leading lines toward the focal point from [direction]"
- "Minimum 3 depth layers: foreground elements, midground subject, background environment"
- "Negative space [amount] around focal point"
- "Frame-within-frame using [architectural elements] if appropriate"

### 16.7 Cultural Sensitivity Prompts

Cultural sensitivity prompts are mandatory for culturally specific content:

**Sensitivity Language:**
- "Culturally appropriate representation of [culture/region/period]"
- "Respectful depiction of [religious/cultural elements]"
- "Period-appropriate social norms and practices"
- "No stereotyping or caricature"
- "Accurate cultural details from verified references"

**Culture-Specific Constraints:**
- "Clothing appropriate to [region] and [period]"
- "Architecture consistent with [region] building traditions"
- "Decorative elements from [culture] artistic tradition"
- "Colors from [region/period] traditional palette"
- "No elements from other cultures or periods"

### 16.8 Accessibility Requirements in Prompts

Accessibility requirements must be included in all generation prompts:

**Accessibility Language:**
- "Sufficient contrast between all elements (minimum 4.5:1)"
- "Information conveyed through shape and position, not color alone"
- "Text at minimum [size] with proper contrast"
- "No critical information below [size] pixels"
- "Clear visual hierarchy with distinct levels"

### 16.9 Quality Descriptors

Quality descriptors set the expected quality level for AI output:

**Quality Language:**
- "Production quality, suitable for educational content"
- "Clean, polished, professional finish"
- "Consistent with National Geographic/BBC documentary visual standards"
- "No rough edges, no unfinished areas, no visible artifacts"
- "Cinematic quality lighting and composition"

### 16.10 Iterative Refinement Process

The refinement process follows a strict generate-review-refine-regenerate cycle:

**Refinement Cycle:**
1. Generate initial batch (3-5 variants)
2. Review against checklist
3. Identify specific issues
4. Refine prompt addressing each issue
5. Document prompt changes
6. Regenerate
7. Review again
8. Repeat until approved or rejected

**Issue Documentation:**
- Each issue documented with: description, location in output, severity
- Prompt change documented with: what changed, why it changed
- Refinement history maintained per asset

**Cycle Limits:**
- Maximum 5 refinement cycles per asset
- If not approved after 5 cycles, asset is referred to human creation
- Cycle count tracked in asset metadata

---

## 17. Human Oversight Requirements

### 17.1 Mandatory Human Review Stages

Human review is required at these mandatory stages for every AI-generated asset:

**Stage 1: Concept Approval**
- Human approves the creative brief before generation
- Verifies: asset type, style, purpose, constraints
- Signs off on: reference materials, prompt parameters, quality expectations

**Stage 2: Composition Approval**
- Human reviews initial composition variants (for illustrations, scenes, layouts)
- Selects best composition direction
- Provides feedback for refinement

**Stage 3: Historical Accuracy Review**
- Required for all historical content
- Reviewed by historical consultant or content expert
- Verifies: period accuracy, regional accuracy, anachronism absence

**Stage 4: Educational Accuracy Review**
- Required for all educational content
- Reviewed by educational content expert
- Verifies: factual accuracy, learning objective alignment, age-appropriateness

**Stage 5: Cultural Sensitivity Review**
- Required for all culturally specific content
- Reviewed by cultural advisor
- Verifies: cultural appropriateness, respectfulness, accuracy

**Stage 6: Final Quality Review**
- Required for all assets before pipeline ingestion
- Reviewed by art director or senior designer
- Verifies: style match, technical quality, overall quality

### 17.2 AI Autonomous Operation Scope

What AI can do autonomously without human review:

**Autonomous Operations:**
- Initial draft generation (always reviewed before use)
- Variant generation from approved brief
- Background environment generation (first pass)
- Simple icon generation (first pass)
- Pattern generation and tiling
- Texture generation for 3D models
- LOD generation (geometry simplification)
- Format conversion and technical optimization
- Asset resizing and resolution adjustment
- Color palette application within approved constraints

### 17.3 Human Review Required

Human review is required for these asset types and stages:

**Mandatory Human Review:**
- Character illustrations and expressions
- Story scenes and narrative illustrations
- 3D hero assets (Tier 1)
- All educational content
- Animations (all types)
- Video (all types)
- Narration (all languages)
- Any asset containing text
- Historical reconstructions
- Sensitive cultural or religious content
- Final approval of any asset before pipeline ingestion

### 17.4 Reviewer Qualifications

Reviewers must have specific qualifications for each review type:

**Art Director / Senior Designer:**
- Minimum 5 years visual design experience
- Platform style mastery
- Quality evaluation certification

**Historical Consultant:**
- Advanced degree in relevant historical field
- Platform historical accuracy training
- Familiarity with platform's historical database

**Educational Content Expert:**
- Teaching or curriculum development experience
- Subject matter expertise in relevant domain
- Age-appropriateness assessment training

**Cultural Advisor:**
- Deep knowledge of relevant culture(s)
- Cultural sensitivity training certification
- Platform cultural guidelines training

### 17.5 Review Documentation

All reviews must be documented with:

**Documentation Requirements:**
- Reviewer identity and qualifications
- Date and time of review
- Review stage (draft, refinement, final)
- Pass/fail/pending status
- Specific issues identified
- Recommendations for improvement
- Approval signature or digital equivalent

**Documentation Storage:**
- Review records attached to asset metadata
- Stored in asset management system
- Auditable for compliance verification
- Retained for minimum 2 years

---

## 18. AI Review Checklist

### 18.1 Standard Review Checklist

For each AI-generated asset, the following checklist items must be completed:

**Style Match (pass/fail):**
- Does the asset match the platform's unified illustration style?
- Are the style parameters (flat shapes, layered depth, geometric precision) correctly applied?
- Is the asset visually consistent with the approved style reference?
- Is there any style drift or deviation from established guidelines?

**Color Adherence (pass/fail):**
- Are all colors within the approved platform palette?
- Are period-specific or mode-specific palette constraints followed?
- Is color saturation appropriate for the content type?
- Is color harmony maintained throughout the asset?

**Composition Quality (1-5):**
- Is the focal point clear and properly positioned?
- Are depth layers present and well-executed?
- Does the eye flow naturally through the composition?
- Is negative space used effectively?
- Does the composition support the educational purpose?

**Historical Accuracy (pass/fail/review):**
- Is the content historically accurate based on provided references?
- Are period-specific details (architecture, clothing, objects) correct?
- Are there any anachronisms present?
- Does the asset need historical consultant review? (If yes, route to review)

**Educational Accuracy (pass/fail/review):**
- Does the asset support the defined learning objective?
- Is the factual content accurate?
- Is the content age-appropriate for the target audience?
- Does the asset need educational expert review? (If yes, route to review)

**Cultural Sensitivity (pass/fail):**
- Is the asset culturally appropriate for its context?
- Are cultural elements accurately and respectfully represented?
- Are there any stereotypical or reductive depictions?
- Does the asset pass cultural sensitivity guidelines?

**Technical Quality (pass/fail):**
- Does the asset meet technical specifications (resolution, format, file size)?
- Are there any visible artifacts, errors, or quality issues?
- Does the asset perform within performance budgets?
- Are file formats and naming conventions correct?

**Accessibility (pass/fail):**
- Does the asset meet WCAG AA standards?
- Is there sufficient contrast (minimum 4.5:1)?
- Is information conveyed through multiple channels (not color alone)?
- Are text elements legible at all intended sizes?
- Are alternative text descriptions provided?

**Overall Quality (1-5):**
- 5: Exceptional — exceeds quality expectations, no revisions needed
- 4: Excellent — meets all standards, minor optional improvements
- 3: Good — meets standards, some improvements recommended
- 2: Below standard — significant improvements required
- 1: Unacceptable — does not meet minimum standards

**Recommendations for Revision:**
- Specific changes needed (if any)
- Prompt refinements required
- Additional references needed
- Resubmission required (yes/no)

### 18.2 Asset Type Specific Checklist Additions

**Illustration Checklist Additions:**
- [ ] Style matches flat-with-depth specifications exactly
- [ ] Composition has minimum 3 depth layers
- [ ] Character proportions are consistent
- [ ] Historical details are period-accurate
- [ ] No anachronistic elements present
- [ ] Negative prompts were effective

**Icon Checklist Additions:**
- [ ] Designed on 24x24 grid with 2px internal padding
- [ ] 2px uniform stroke with rounded caps and joins
- [ ] All states generated (default, active, disabled)
- [ ] RTL version exists for directional icons
- [ ] Visual weight consistent with set
- [ ] Meaning is clear and recognizable
- [ ] Alt text provided

**Background Checklist Additions:**
- [ ] Mode-appropriate style and colors
- [ ] Seamless tiling verified (if pattern)
- [ ] Performance optimized (file size, resolution)
- [ ] Mood matches educational context
- [ ] No distracting elements

**Infographic Checklist Additions:**
- [ ] Data accuracy verified against source
- [ ] Information hierarchy is clear
- [ ] Color blind safe palette used
- [ ] Text meets minimum size and contrast requirements
- [ ] Source citation included
- [ ] Educational clarity prioritized

**Map Checklist Additions:**
- [ ] Geographic accuracy verified against base data
- [ ] Period-appropriate cartographic style
- [ ] Place names historically accurate
- [ ] Borders historically accurate
- [ ] Labels legible at all zoom levels
- [ ] Terrain and city placement verified

**Animation Checklist Additions:**
- [ ] Follows Motion Design Bible specifications
- [ ] Physics-based motion (weight, momentum)
- [ ] Timing matches duration standards
- [ ] Loops seamlessly (if looping animation)
- [ ] Performance within budget
- [ ] Reduced motion alternative available

**Video Checklist Additions:**
- [ ] Minimum 1080p, 24fps quality
- [ ] Style consistent with art direction
- [ ] No garbled text in frames
- [ ] Lip sync accuracy verified (if dialogue)
- [ ] Cultural sensitivity review passed
- [ ] QC standards met (identical to produced video)

**3D Model Checklist Additions:**
- [ ] Poly count within tier budget
- [ ] Clean topology (quads, no n-gons)
- [ ] UV mapping non-overlapping
- [ ] Texture resolution compliant
- [ ] Material assignments correct
- [ ] LOD variants generated
- [ ] File format compliant
- [ ] Integration test passed

**Sound Effect Checklist Additions:**
- [ ] Sample rate: 48kHz, Bit depth: 24-bit
- [ ] Loudness: -16 LUFS, True peak: -2dBTP
- [ ] No audible artifacts
- [ ] Clean transients
- [ ] Culturally appropriate
- [ ] Loops seamlessly (if looping)
- [ ] WAV master + AAC delivery

**Narration Checklist Additions:**
- [ ] Natural prosody (no robotic quality)
- [ ] Correct pronunciation (Arabic: MSA standards)
- [ ] Voice consistent with character profile
- [ ] Appropriate emotional tone for content
- [ ] AI voice labeled as AI-generated
- [ ] Multi-language: native-quality accent

---

## Appendix A: AI Model Governance

### A.1 Approved Model List

Only models approved by the AI Ethics Board may be used for asset generation:

| Model | Purpose | Approval Date | Review Date | Status |
|---|---|---|---|---|
| [Model Name v1.0] | Illustration generation | 2026-01-15 | 2026-07-15 | Active |
| [Model Name v2.0] | 3D model generation | 2026-03-01 | 2026-09-01 | Active |
| [Model Name v1.0] | Voice generation | 2026-02-01 | 2026-08-01 | Active |

### A.2 Model Evaluation Criteria

Models are evaluated on these criteria before approval:
- Output quality and consistency
- Style adherence
- Cultural sensitivity
- Bias assessment
- Technical performance
- Privacy compliance
- Cost efficiency

### A.3 Model Retirement

Models are retired when:
- Superseded by significantly better model
- Quality degradation detected
- Security or privacy concerns identified
- Style guide evolution makes model obsolete

---

## Appendix B: Prompt Library Management

### B.1 Prompt Library Structure

Prompts are stored in a centralized, version-controlled library:
- Organized by asset type
- Tagged with parameters (style, period, region, mood)
- Version-tracked with change history
- Success rate tracked per prompt

### B.2 Prompt Approval Workflow

1. Prompt written by prompt engineer
2. Reviewed by art director
3. Tested with 3 generations
4. Approved for production use
5. Added to prompt library
6. Success rate monitored

### B.3 Prompt Retirement

Prompts are retired when:
- Style guide changes make them obsolete
- Success rate drops below 70%
- Better prompt alternatives exist
- Asset type requirements change

---

## Appendix C: AI Generation Logging

All AI generations are logged with:
- Generation timestamp
- Model and version used
- Full prompt text
- Generation parameters
- Output asset ID
- Human reviewer
- Review outcome
- Refinement history

Logs are retained for minimum 1 year for audit and analysis purposes.

---

## Appendix D: Quick Reference — Review Responsibilities

| Review Stage | Required For | Review By | Can Skip? |
|---|---|---|---|
| Concept Approval | All assets | Art Director | No |
| Composition Approval | Illustrations, Scenes | Art Director | No |
| Historical Accuracy | Historical content | Historical Consultant | No |
| Educational Accuracy | Educational content | Content Expert | No |
| Cultural Sensitivity | Cultural content | Cultural Advisor | No |
| Final Quality Review | All assets | Senior Designer | No |

---

## Document Metadata

**Document ID:** VP-AIGEN-001
**Version:** 1.0
**Author:** AI Production Directorate
**Last Updated:** 2026-07-10
**Review Cycle:** Monthly
**Classification:** Enterprise AI Production Standard

**Connecting Documents:**
- Visual_Production/00 through 08 (all preceding documents)
- Visual_Production/10_Asset_Library (successor document)
- UX_Design/02_Design_System (design tokens)
- UX_Design/06_Motion_Design_Bible (motion specifications)
- UX_Design/07_Illustration_Bible (illustration specifications)
- UX_Design/08_Accessibility_Bible (accessibility requirements)
- AIOS/00_AI_Architecture (AI system architecture)

**Enforcement:** This document is mandatory for all AI asset generation. Non-compliance results in asset rejection and process review.
