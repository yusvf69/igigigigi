# 16 — Human Review System

> **Document 16 of 24 — Autonomous AI Company: Agents as Employees**
> *Company Scale: 8,500 AI Agents | 130 Teams | 33 Departments | 7 Executives | 1 CE-AI*
> *Mission: AI-Powered Islamic Education Platform*

---

## Table of Contents

1. [Purpose of Human Review](#1-purpose-of-human-review)
2. [Human Review Board Structure](#2-human-review-board-structure)
3. [Triggering Human Review](#3-triggering-human-review)
4. [Human Review Workflow](#4-human-review-workflow)
5. [Review Types and Depth](#5-review-types-and-depth)
6. [Review Decision Outcomes](#6-review-decision-outcomes)
7. [Reviewer Tools and Dashboard](#7-reviewer-tools-and-dashboard)
8. [Human Feedback Integration](#8-human-feedback-integration)
9. [Quality Assurance of Human Reviews](#9-quality-assurance-of-human-reviews)
10. [Contentious and Edge Cases](#10-contentious-and-edge-cases)
11. [Human-in-the-Loop Learning](#11-human-in-the-loop-learning)
12. [Scaling Human Review](#12-scaling-human-review)
13. [Ethics and Compliance](#13-ethics-and-compliance)

---

## 1. Purpose of Human Review

### 1.1 Why Humans Remain Essential

The Autonomous AI Company operates 8,500 AI agents generating Islamic educational content at massive scale. While AI achieves high accuracy on routine tasks, humans remain indispensable for several critical domains:

**Islamic Content Accuracy**

The Quran, Hadith, and Fiqh represent divine revelation and centuries of scholarly tradition. AI models, even when fine-tuned on Islamic corpora, exhibit failure modes:

| AI Weakness | Example | Consequence of Error |
|---|---|---|
| Fabricated hadith (hadith mawdu') | AI generates a saying attributed to the Prophet that does not exist in any canonical collection | Spreading false religious attribution — a major sin (kabirah) |
| Incorrect chain of narration (sanad) | AI assigns hasan grade to a da'if hadith | Misleading students about reliability of Prophetic traditions |
| Misapplied Quranic tafseer | AI applies a modern interpretation where classical consensus (ijma') exists on a different meaning | Teaching incorrect tafseer, undermining scholarly tradition |
| Outdated fiqh ruling | AI issues a ruling based on an older school position superseded by contemporary fatwa councils | Providing non-operative Islamic guidance |
| Weak analogy (qiyas) | AI draws parallel between two cases where fundamental difference exists | Invalid legal reasoning propagated as Islamic law |
| Language nuance error | AI misreads Arabic grammatical structure (i'rab) changing meaning of an ayah | Incorrect understanding of Divine text |
| Cross-madhhab confusion | AI blends positions from different schools without proper methodology | Creating a hybrid ruling no scholar endorses |
| Anthropomorphic error | AI presents metaphorical Quranic verses literally | Teaching an incorrect aqidah (creed) |

**Legal Compliance**

The platform operates across multiple jurisdictions. Each country's laws regarding religious content, hate speech, child protection, and financial transactions must be respected.

| Jurisdiction | Legal Requirement | AI Risk | Human Role |
|---|---|---|---|
| Saudi Arabia | Content must align with official Islamic syllabus | AI may introduce non-standard interpretations | Confirm alignment with Saudi curriculum standards |
| UAE | Anti-discrimination law prohibits sectarian content | AI may inadvertently favor one madhhab over another | Verify balanced representation across recognized schools |
| Malaysia | State-level Islamic authorities regulate online religious content | AI unaware of specific state fatwas | Validate content against state religious department requirements |
| Indonesia | MUI (Majelis Ulama Indonesia) fatwa compliance | AI may contradict published MUI positions | Cross-reference with official MUI fatwa database |
| UK | Charity Commission rules for Islamic educational content | AI may inadvertently contain fundraising content | Review for compliance with secular charity regulations |
| US | First Amendment considerations, anti-terrorism financing laws | AI may discuss sensitive political topics | Screen for prohibited content under US law |

**Ethical Boundaries**

AI agents lack genuine moral reasoning. They simulate ethical decision-making through pattern matching. When novel ethical situations arise — situations not represented in training data — humans must decide.

| Scenario | Why AI Cannot Decide Alone | Human Ethical Judgment Required |
|---|---|---|
| User asks AI to justify interest-based mortgage (riba) for medical emergency | AI may mechanically say "riba is haram" without considering necessity (darurah) exceptions | Scholar weighs darurah principle against riba prohibition, considers extent of need |
| Student asks about Islamic view on newly emerging reproductive technology | No training data exists on this specific technology | Scholar applies maqasid al-shariah (higher objectives of Islamic law) methodology |
| User discloses intention to harm themselves | AI lacks moral agency to balance confidentiality vs. duty of care | Human review team coordinates with authorities |
| Cross-madhhab marriage question (Hanafi wife + Maliki husband) | AI may apply only one madhhab's rules | Scholar considers both positions, advises on practical application |
| AI generates content about non-Islamic religious practices for comparative religion | AI may inadvertently show disrespect or misrepresent other traditions | Human reviewer ensures respectful, accurate comparative presentation |

**Novel Situations**

| Novel Situation | Why AI Fails | Human Solution |
|---|---|---|
| New Islamic educational game format | AI has never seen this format in training | Human instructional designer reviews Islamic accuracy of game mechanics |
| Integration with VR Hajj simulation | AI cannot evaluate sensory fidelity against ritual requirements | Scholar reviews whether VR suffices for educational vs. ritual purposes |
| AI-generated Islamic children's song with modern instruments | No precedent in classical sources | Scholar evaluates permissibility based on general principles |
| User-submitted question about cryptocurrency zakat | Novel financial instrument, no classical ruling | Senior scholar leads panel to derive new ruling using usul al-fiqh |

**Edge Cases**

Edge cases — scenarios at the boundary of the AI's knowledge distribution — routinely surface in Islamic content due to:

- **Rare Quranic recitation variants (qira'at)** — the 10 canonical recitation styles have numerous sub-variants; AI may mix readings
- **Conflicting hadith grades** — two scholars may grade the same hadith differently; AI lacks the scholarly depth to weigh methodologies
- **Extremely niche fiqh questions** — e.g., "Water used in wudu that a cat previously drank from — is it pure?" requires deep knowledge of najasah categories
- **Historical disagreements among Companions** — AI may present one Companion's opinion as definitive when scholarly disagreement exists
- **Very new Muslim revert with limited Arabic** — AI may recommend classical texts the user cannot handle

### 1.2 What Humans Do Not Do

The Human Review Board operates under strict scope limitation.

| Excluded Task | Rationale | Which System Handles It |
|---|---|---|
| High-volume content generation | 8,500 agents produce 500,000+ pieces of content daily; humans cannot physically review all | Agent pipeline with automated quality gates |
| Repetitive data retrieval | Looking up Quranic verse numbers, hadith narration links, fiqh ruling text | Knowledge Graph agents |
| Simple factual verification of unambiguous sources | "Does Bukhari 1234 exist?" — binary factual question | Database integrity checks, knowledge graph verifiers |
| Formatting and presentation | Arabic diacritics (tashkeel), font selection, layout | Presentation agents |
| Initial moderation of clearly offensive user input | Profanity, hate speech, spam | Automated moderation filter |
| Routine LMS operations | Enrollment, progress tracking, certificate generation | Operations agents |
| Performance analytics | Content popularity, user engagement metrics | Analytics agents |
| Translation of standard content | Arabic-to-English of well-understood terms | Translation agents |

### 1.3 Review Domains

**Domain 1: Content Accuracy (Primary Domain)**

- Quranic text correctness (ayah boundaries, surah identification, word accuracy)
- Hadith verification (narration text, chain, grading authenticity)
- Fiqh ruling accuracy (madhhab-specific positions, conditions, nullifiers)
- Tafseer correctness (authenticity of narration, attribution to correct scholar)
- Seerah (biography) factual accuracy (dates, names, locations, events)
- Aqidah (creed) alignment with authentic Islamic theology
- Arabic language precision (nahw, sarf, balaghah)

**Domain 2: Platform Safety**

- User data protection compliance
- Content suitability by age group (children vs. adult learners)
- Anti-radicalization markers — content that may promote extremism
- Sectarian content that could incite hatred
- Psychological impact of content (balance of hope and fear — khawf wa raja')
- Child protection: ensuring gamification and interaction appropriate for minors
- Financial safety: ensuring payment systems, donations, and e-commerce comply with Islamic finance principles

**Domain 3: Financial Decisions**

- Pricing of premium content (courses, certifications, tutoring)
- Subscription model Islamic compliance (subscriptions = buyu' al-murakkabah analysis)
- Donation distribution rules (zakat distribution, sadaqah allocation)
- Sponsor content assessment (endorsements by Islamic organizations)
- Platform revenue from haram sources (advertising for non-halal products)
- Freemium model permissibility (free basic content, paid advanced)

**Domain 4: Legal Compliance**

- Educational accreditation requirements across jurisdictions
- Data protection and privacy law compliance (GDPR, PDPL, etc.)
- Charity registration requirements for donation features
- Intellectual property — plagiarism checking for Islamic content
- Defamation — content about scholars or Islamic institutions
- Terms of Service compliance with applicable law

**Domain 5: Strategic Direction**

- Content curriculum changes (new courses, updated methodologies)
- Platform-wide content policies (e.g., "no content promoting only one madhhab")
- New geographic expansion content adaptations
- Partnership content quality (external content from scholars/universities)
- Crisis response content (e.g., pandemic-related Islamic guidance)

**Domain 6: Novel Content Types**

- First-time AI-generated media format (podcasts, video scripts, infographics)
- AI-generated fatwa for novel contemporary questions
- Integration of non-Islamic sources (e.g., reference to modern psychology in Tazkiyah)
- Content for users with special needs (learning disabilities, deaf/blind accessibility)
- Gamified religious content (points for Quran memorization, leaderboards)
- AI-generated interactive content (quizzes with Islamic content, chatbots for fiqh)

---

## 2. Human Review Board Structure

### 2.1 Board Composition

The Human Review Board (HRB) comprises 15-25 human reviewers at any given time, scalable to 50+ during high-volume seasons.

#### 2.1.1 Head of Islamic Review

| Attribute | Detail |
|---|---|
| Title | Head of Islamic Review & Content Authority |
| Required qualification | Mufti-level certification from recognized Islamic university (Al-Azhar, QMUL Imaam, IIUI, etc.) |
| Minimum experience | 15+ years in Islamic scholarship and teaching |
| Additional requirements | Fluency in Arabic (native level), English (professional); familiarity with AI/technology |
| Role | Final authority on all content decisions, responsible for Board quality, training, and external representation |
| Reporting to | CE-AI and Board of Advisors |
| Remuneration | Executive-level salary equivalent to $150k-$200k/year + housing + annual umrah/hajj sponsorship |
| Deputy | Deputy Head of Islamic Review (trained to Mufti level) |

#### 2.1.2 Content Reviewers — Islamic Scholars

| Level | Count | Minimum Qualification | Specialization Options | Authority |
|---|---|---|---|---|
| Senior Scholar (L3) | 3-5 | Ijazah in specialized field + 10+ years | Tafseer, Hadith, Fiqh, Usul al-Fiqh, Arabic | Comprehensive review authority, override L2 decisions |
| Scholar (L2) | 5-8 | Bachelor/Master in Islamic Studies + 3+ years experience | Specific madhhab expert (Hanafi, Shafi'i, Maliki, Hanbali) or thematic expert | Standard & deep review authority |
| Junior Scholar (L1) | 5-10 | 2+ years Islamic studies (Alimiyyah or equivalent) + passing certification | General Islamic knowledge | Quick & standard review authority |

#### 2.1.3 Platform Safety Officers

| Role | Count | Responsibilities |
|---|---|---|
| Lead Safety Officer | 1 | Oversee content safety decisions, develop safety policies, liaise with legal team |
| Safety Reviewers | 3-5 | Review flagged content for safety concerns: child safety, hate speech, radicalization, data privacy |
| Child Protection Specialist | 1 | Review content for child audiences (under 18), age-rating decisions, child psychology input |
| Crisis Response Officer | 1 | Handle urgent platform-wide safety issues, emergency content removal |

#### 2.1.4 Legal Advisors

| Role | Count | Jurisdiction Expertise |
|---|---|---|
| Senior Islamic Legal Advisor | 1 | Saudi Arabia, UAE, Egypt — Shariah law and Islamic regulatory authorities |
| International Legal Counsel | 1 | UK, EU, USA — secular law, charity law, education law |
| Data Protection Specialist | 1 | Global privacy regulations (GDPR, PDPL, CCPA, etc.) |

#### 2.1.5 Subject Matter Experts (SMEs)

| SME Type | Count | Rate | Activation |
|---|---|---|---|
| Tajweed Expert | 2 | $150/hr | Quranic recitation content review |
| Qira'at Expert | 2 | $200/hr | Rare qira'at verification |
| Hadith Sciences Specialist | 2 | $175/hr | Complex chain analysis, jarh wa ta'dil |
| Fiqh of Minorities Expert | 1 | $200/hr | Muslim minority issues in non-Muslim countries |
| Comparative Religion Scholar | 1 | $150/hr | Content about other religions |
| Islamic History Professor | 1 | $100/hr | Historical accuracy of seerah, Islamic history content |
| Arabic Linguistics Expert | 1 | $150/hr | Complex Arabic grammatical questions |
| Fatwa Council Liaison | 1 | $250/hr | Liaison with official fatwa bodies |
| Child Psychologist (Islamic perspective) | 1 | $180/hr | Islamic education for children, age-appropriate teaching |

### 2.2 Hierarchical Review Structure

```
                                    +---------------------+
                                    |  L4: External Board |
                                    |  (Renowned scholars, |
                                    |   Institutional reps)|
                                    +----------+----------+
                                               |
                                    +----------+----------+
                                    |  Head of Islamic     |
                                    |  Review (Mufti-level)|
                                    |  Has final authority |
                                    +----------+----------+
                                               |
          +------------------------------------+------------------------------------+
          |                                    |                                    |
+---------+---------+              +-----------+-----------+            +-----------+----------+
|  L3: Senior        |              |  Lead Safety         |            |  Legal Advisors      |
|  Scholars (3-5)    |              |  Officer + 4 Safety   |            |  (3 legal experts)   |
|  Tafseer/Hadith/   |              |  Reviewers            |            |                      |
|  Fiqh/Usul/Arabic  |              |                       |            |                      |
+---------+----------+              +-----------------------+            +----------------------+
          |
+---------+----------+
|  L2: Specialized     |
|  Islamic Scholars    |
|  (5-8 scholars)      |
+---------+----------+
          |
+---------+----------+
|  L1: Trained Content |
|  Moderators          |
|  (5-10 moderators)   |
+--------------------+
```

### 2.3 On-Call Rotation

| Time Slot | L1 Coverage | L2 Coverage | L3 Coverage | L4 (SME) Coverage |
|---|---|---|---|---|
| Mon-Fri 08:00-18:00 Mecca | 4 online | 3 online | 1 online | Available by appointment |
| Mon-Fri 18:00-08:00 | 1 on-call (1hr) | 1 on-call (4hr) | 1 on-call (4hr) | On-demand, 24hr |
| Sat-Sun | 1 on-call (2hr) | 1 on-call (4hr) | 1 on-call (8hr) | On-demand, 48hr |
| Ramadan (month) | 8 online (14hr shifts) | 5 online (12hr shifts) | 2 online (10hr shifts) | Full availability, 4hr |
| Hajj (week) | 6 online | 4 online | 1 on-call | SME scheduled |
| Eid holidays | 1 on-call (4hr) | 1 on-call (8hr) | Head of Islamic Review on-call | Emergency only, 24hr |

**On-Call Compensation**: Standard on-call: base salary + 15% shift differential. Night/weekend: +25%. Ramadan/Hajj: +50%. Emergency activation: +$500 flat fee + overtime. L4 SME activation: +$250/hr.

**Urgency Escalation Tree for Emergencies**:

```
Content Flagged as Emergency
  |
  +-- L1 Reviewer assigned
  |     +-- Can handle in 15 min -> resolve
  |     +-- Cannot handle -> escalate to L2
  |
  +-- L2 Scholar assigned
  |     +-- Can handle in 30 min -> resolve (notify L3)
  |     +-- Cannot handle -> escalate to L3
  |
  +-- L3 Senior Scholar assigned
  |     +-- Can handle in 1 hr -> resolve (notify Head)
  |     +-- Cannot handle -> escalate to Head
  |
  +-- Head of Islamic Review
        +-- Can handle -> final decision
        +-- Cannot -> escalate to External Board (L4)
```

### 2.4 Qualifications, Training, and Certification

#### 2.4.1 L1 Reviewer Qualification

| Requirement | Detail |
|---|---|
| Minimum education | 2+ years structured Islamic studies (Alimiyyah course, Islamic university) OR 4+ years self-study with certification from recognized Islamic institute |
| Language | Arabic (reading fluency), English (professional), platform language (Urdu/Bahasa/Malay/etc.) |
| Test | 3-hour exam: Quranic ayah identification (50 ayahs), hadith authenticity grading (20), fiqh questions (30), policy questions (20) |
| Technical | Comfort with web-based tools, data entry accuracy (> 95%), dashboard proficiency |
| Soft skills | Attention to detail, patience, ability to explain decisions, team collaboration |
| Background check | Clean criminal record, no extremist links, no history of hate speech or sectarianism |
| Probation | 90 days, all reviews peer-reviewed by L2, maximum 200 reviews |

#### 2.4.2 L2 Scholar Qualification

| Requirement | Detail |
|---|---|
| Education | Bachelor/Master in Islamic Studies from recognized university (Al-Azhar, Islamic University of Madinah, Qalam, IIUI, Zaytuna) OR equivalent traditional study with ijazah |
| Specialization | At least one: Quranic Sciences, Hadith Sciences, Fiqh & Usul al-Fiqh, Aqidah |
| Ijazah | Must hold formal ijazah (license to teach) in at least one specialization |
| Experience | 3+ years post-graduation in Islamic teaching, research, or content creation |
| Language | Arabic (native/near-native), English (advanced, TOEFL 105+ or equivalent) |
| Certification | Pass L2 Certification Exam |

**L2 Certification Exam Structure**:

| Section | Duration | Score Range | Minimum Pass | Topics |
|---|---|---|---|---|
| Quranic Sciences | 60 min | 0-100 | 80 | Asbab al-nuzul, nasikh wa mansukh, muhkam wa mutashabih, ulum al-Quran |
| Hadith Sciences | 60 min | 0-100 | 80 | Mustalah al-hadith, jarh wa ta'dil, comparative grading |
| Fiqh | 90 min | 0-100 | 80 | Fiqh rules, usul applied to 10 case studies |
| Platform Review Simulation | 90 min | 0-100 | 85 | Review 10 AI-generated pieces; match L3 consensus on 8/10 |
| Ethics & Compliance | 30 min | 0-100 | 95 | Platform policies, legal boundaries, ethical scenarios |
| Arabic Language | 60 min | 0-100 | 80 | Classical Arabic grammar, rhetoric (balaghah), semantics |

#### 2.4.3 L3 Senior Scholar Qualification

| Requirement | Detail |
|---|---|
| Education | PhD/Doctorate in Islamic Sciences or Mufti-level certification |
| Ijazah | Multiple ijazah — Quran (with sanad chain), Hadith (with sanad), Fiqh (authorization to issue fatwa) |
| Experience | 5+ years post-doctorate or 10+ years in teaching and fatwa issuance |
| Publication | Minimum 3 peer-reviewed publications in reputable Islamic studies journals |
| Language | Arabic (native), English (academic), ideally one additional language (Persian, Urdu) |
| Technology | Understand AI model behavior, prompt engineering concepts, knowledge graph principles |
| Appointment | Unanimous vote of current L3 members + Head of Islamic Review + Executive approval |

**L3 Continuing Education**: Annual 40 hours professional development in Islamic studies, 20 hours technology/AI training, attend 2 international Islamic conferences, submit 1 published paper every 2 years, mentor 1 L2 scholar towards L3.

#### 2.4.4 L4 External Advisory Board

| Attribute | Detail |
|---|---|
| Composition | 7-11 members |
| Members | Representatives from: Al-Azhar University, Islamic Fiqh Academy (OIC), International Institute of Islamic Thought, ISESCO, one Grand Mufti (rotating), two recognized Islamic scholars at large, leader of an accredited Islamic educational platform |
| Term | 3 years, renewable twice |
| Meeting frequency | Quarterly (virtual), annual (in-person) |
| Compensation | $20,000/year retainer + expenses + $500/hour for ad-hoc consultation |
| Authority | Advisory only — no veto power. Head of Islamic Review follows recommendations or documents reasons for divergence |

### 2.5 Reviewer Roles and Responsibility Matrix

| Responsibility | L1 | L2 | L3 | L4 | Head of Islamic Review | Safety Officer | Legal Advisor |
|---|---|---|---|---|---|---|---|
| Quick review (standard) | Primary | QA (5%) | -- | -- | -- | -- | -- |
| Standard review (most content) | Primary | QA (5%) | -- | -- | -- | -- | -- |
| Deep review (complex) | -- | Primary | Secondary | -- | Final approval | -- | -- |
| Comprehensive (novel) | -- | Assist | Primary | Advisory | Final | Review if needed | Review if needed |
| Panel review (major) | -- | -- | Chair | Chair | Final signoff | Review | Review |
| External (institutions) | -- | -- | Liaise | Lead | Final acceptance | -- | Contract |
| Safety flagged | -- | -- | -- | -- | -- | Primary | Legal referral |
| Legal compliance | -- | -- | -- | -- | -- | Consulted | Primary |
| Emergency content removal | -- | -- | -- | Head + Safety jointly | -- | Lead | Consulted |
| User complaint | Triage | Standard | Escalated | -- | Final appeal | If safety | If legal |

### 2.6 Compensation Structure

| Level | Base Salary (USD) | FTE | On-Call | Bonus | Total Est. Annual |
|---|---|---|---|---|---|
| L1 Reviewer | $45k-$60k | 1.0 | +15-25% | 10% | $57k-$83k |
| L2 Scholar | $70k-$90k | 1.0 | +15-25% | 15% | $92k-$129k |
| L3 Senior Scholar | $120k-$150k | 1.0 | +15-25% | 20% | $166k-$225k |
| L4 Advisory | $20k retainer | 0.1 | -- | -- | $20k + $500/hr |
| Head of Islamic Review | $180k-$250k | 1.0 | +25% | 25% | $248k-$359k |
| Lead Safety Officer | $80k-$110k | 1.0 | +15-20% | 15% | $106k-$152k |
| Safety Reviewer | $50k-$65k | 1.0 | +15-20% | 10% | $63k-$86k |
| Legal Advisor | $90k-$140k | 1.0 | -- | 15% | $104k-$161k |

---

## 3. Triggering Human Review

### 3.1 Automatic Triggers

#### 3.1.1 Agent Confidence Thresholds

Every agent produces a confidence score (0.0 to 1.0) based on: model logit confidence, knowledge graph match strength, citation completeness, consistency with past outputs, and cross-check score from 3+ independent verifier agents.

| Confidence Range | Action | Human Review Triggered? |
|---|---|---|
| 0.95 - 1.0 | Auto-approve | Random sample 1% |
| 0.80 - 0.949 | Auto-approve, monitor | Only if downward trend |
| 0.70 - 0.799 | Standard auto-approve | Random 5% for new categories |
| 0.60 - 0.699 | **Standard Human Review** | Yes — 100% |
| 0.40 - 0.599 | **Deep Human Review (L2)** | Yes — 100% |
| 0.20 - 0.399 | **Comprehensive Review (L3)** | Yes — 100% |
| 0.00 - 0.199 | Blocked, auto-sent for analysis | Yes — 100% priority |

**Real Example**: An agent generates hadith content with confidence 0.65. The hadith's sanad chain is not well matched to Knowledge Graph. Content is automatically funneled to L1 Standard Review.

#### 3.1.2 Escalation from Lower Tiers (L0-L6)

| Tier | Description | Action | Human Review? |
|---|---|---|---|
| L0 | Confidence > 0.95, no conflicts | Auto-execute | No |
| L1 | 0.60-0.94, minor uncertainty | Cross-verify from 2 peer agents | No |
| L2 | 0.40-0.59, moderate uncertainty | 5 peer agents for consensus | No |
| L3 | > 0.20, high uncertainty or conflict | Cross-team verification | No |
| L4 | Persistent uncertainty, contradicting sources | Knowledge Graph deep search | No |
| L5 | Still unresolved, novel/complex | **Human Review (L1)** | **Yes** |
| L6 | Human reviewer cannot resolve | **Escalate to L2/L3** | **Yes** |

**Real Scenario**: Agent generates content on fiqh of "organic halal meat certification." Conflicting sources exist — some scholars say "halal" is sufficient, others require "tayyib" (wholesome) criteria. Agent reaches L5, triggering L1 review. L1 reviewer lacks fiqh depth on modern food certification, escalating to L6 -> L2 review.

#### 3.1.3 Novel Quranic Interpretation

When an agent generates Quranic tafseer that: does not match top 10 classical tafseer works, proposes a new meaning not found in recognized tafseer, relies on non-standard qira'at, or interprets mutashabih verses with novel meaning -> **Automatic Deep Review (L2)** with alert to Tafseer specialist + Head of Islamic Review. Content blocked pending review.

| Quranic Content Type | Deviation Allowed | Trigger |
|---|---|---|
| Muhkam (clear) verses | 0% | Any divergence -> immediate review |
| Mutashabih (ambiguous) | <= 10% from classical consensus | Novel interpretation -> L3 review |
| Linguistic tafseer (lughawi) | <= 20% from Arabic norms | Creative grammatical reading -> review |
| Scientific tafseer | 0% — must reference classical source | Independent modern connection -> review |
| Juridical tafseer (ahkam) | 0% — must follow usul | Any deviation -> review |

#### 3.1.4 New Fatwa / Islamic Ruling Request

When a user submits a classified "fatwa request":

| Fatwa Type | Review Level | Max Response | Reviewer Requirement |
|---|---|---|---|
| Simple (clear ruling from Quran/Sunnah consensus) | L1 -> Optional L2 | 4hrs | L1 with Islamic studies background |
| Moderate (ijtihad, well-known sources) | L2 | 24hrs | Scholar specialized in relevant domain |
| Complex (new issue, multiple valid opinions) | L3 | 72hrs | Senior scholar + notify Head |
| Cross-madhhab (schools differ) | L3 | 72hrs | Scholars from each relevant madhhab |
| High-impact (financial, marital, medical) | L3 + Legal | 7 days | Panel of 3 L3 + Legal advisor |
| International consultation | L4 | 14-30 days | External advisory board |

**Fatwa Workflow**:
1. User submits question through platform
2. Content Analysis Agent classifies as "fatwa request" (type, madhhab, difficulty)
3. Generation agent produces initial response with citations
4. Agent confidence assessed; if below thresholds -> **Human Review**
5. If confidence > 0.8 and validated, still marked "Pending Scholar Verification" — queued for Standard Review within 24 hours
6. User sees response with banner: "This response awaits verification by our Islamic scholarship team within 24 hours."

#### 3.1.5 User Complaint About Content Accuracy

| Complaint Type | Automatic Action | Human Review | SLA |
|---|---|---|---|
| General (unspecified) | Flagged, sent to L1 | L1 Standard Review | 24hrs |
| Specific citation error (with evidence) | Content blocked, sent to L2 | L2 Standard Review | 4hrs |
| Multiple complaints from different users | Content blocked, sent to L3 | L3 Comprehensive Review | 24hrs |
| Complaint from verified scholar account | Content blocked, L3 + Head | L3 Review | 4hrs |
| Repeated complaint about same category | Statistical flag raised | Category-wide audit | 7 days |

**Complaint Rate Limits**: Standard users: max 5 complaints/hour, 20/day. Verified scholars: max 20/hour, 100/day. New accounts (< 30 days): max 2/day.

#### 3.1.6 Cross-Madhhab Conflict

| Scenario | Trigger | Human Review Required |
|---|---|---|
| Agent presents only one madhhab as definitive | Detected by Cross-Madhhab Check Agent | L2 Standard Review |
| Agent presents all positions correctly | No action needed | -- |
| Agent inaccurately represents a madhhab | Confidence < 0.6 | L2 Deep Review with specialist |
| Agent creates hybrid ruling not endorsed by any madhhab | Detected by comparative agent | L3 Comprehensive Review |
| Content has value judgment on madhhabs | Platform policy violation | L2 Standard Review |

**Cross-Madhhab Display Protocol**: Content must display: "Scholarly Note: This topic has differing opinions among scholars. Presented below are the positions of the four major Sunni schools of thought." Each position attributed to its school. Preferred position per contemporary fatwa councils noted. Human reviewer verifies all cross-madhhab content before publication.

#### 3.1.7 Content Flagged by 3+ Independent Agents

Verificatory agents deployed for every content piece: Quranic Check Agent, Hadith Check Agent, Fiqh Consistency Agent, Language Agent, Source Authenticity Agent, Safety Agent, Madhhab Balance Agent, User Suitability Agent.

- 1 flag -> flagged but normal flow
- 2 flags -> confidence decreased, slower publication
- **3+ flags -> Automatic Standard Human Review**
- **4+ flags -> Automatic Deep Review (L2)**
- **5+ flags -> Automatic Comprehensive Review (L3)**

**Real Scenario**: Agent generates content on "Signs of the Day of Judgment." Qur'anic Check Agent flags misattributed ayah. Hadith Check Agent flags weak hadith used as evidence. Fiqh Consistency Agent flags irregularity. Three flags -> content sent to Standard Review.

#### 3.1.8 Revenue or Legal Impact Thresholds

| Impact Level | Threshold | Human Review Required |
|---|---|---|
| Free public domain | No direct revenue | Random sample 1% |
| Behind paywall | > $0.10 expected per user | Standard Review |
| Premium certification | > $50 per user | L2 Deep Review |
| Advertising paid course | > $100 per conversion | L2 + Safety |
| Zakat/sadaqah calculations | > $1M annually | L3 Comprehensive + Legal |
| Halal/haram business content | Legal liability possible | Legal Advisor Review |
| Regulated jurisdiction content | Direct legal compliance | Legal Advisor Review |

#### 3.1.9 A/B Testing Requiring Human Judgment

| Test Type | Human Review Required | Depth |
|---|---|---|
| Content presentation format | No | -- |
| Content tone (level of academic detail) | Sample 10% | Standard (L1) |
| Theological approach (different scholarly positions) | 100% of variants | L2 Deep Review |
| Content language (new dialect) | First 100 pieces | L2 Standard Review |
| New audience demographic | First 50 pieces | L2 + Safety Review |
| AI-generated visuals for Islamic concepts | First 20 visuals | L2 + SME |

#### 3.1.10 Random Sampling for Quality Audit

1% of all auto-approved content randomly selected for human review.

| Sampling Parameter | Value |
|---|---|
| Rate | 1% of auto-approved content |
| Strategy | Stratified by content type |
| Strata | Quranic (0.5%), Hadith (1.5%), Fiqh (1.2%), Aqidah (1.0%), Seerah (0.8%), Arabic (0.5%), General (1.0%), Fatwa (2.0%) |
| Review depth | Standard Review (L1) |
| Failure handling | Correct content in place, retrain agent, audit flag, 100% of that agent's output reviewed for 24 hours |

**Audit Statistics**: 500,000 items/day -> 5,000 sampled/day. 99% expected pass rate -> 50 issues discovered/day from random audit.

### 3.2 Human-Initiated Review

Any human reviewer can pull any content item for review at any time. Reviewer enters content ID in dashboard, system displays full content + confidence scores + related content, reviewer initiates review with reason. Metrics track pull-to-review time, decision, and accuracy.

**Pull Reasons**: Quality assurance, random inspection, follow-up on partially resolved issues, check consistency across related content, investigate user complaint in same category.

---

## 4. Human Review Workflow

### 4.1 Core Review Workflow

```
Content enters Human Review Queue (automatic trigger, complaint, or pull)
  |
  +-- 1. Review Request Creation: Content ID, type, priority, trigger source,
  |       confidence score, agent metadata, domain tags, language, madhhab
  |       -> SLA timer begins
  |
  +-- 2. Queue Assignment: Auto-assignment by availability, SLA urgency,
  |       domain match (specialization), load balance, language match
  |
  +-- 3. Reviewer Picks Up: Dashboard notification, reads content,
  |       sees all context: sources, citations, AI reasoning chain,
  |       related previous reviews
  |
  +-- 4. Review: Apply appropriate depth, check sources, accuracy,
  |       citations, tone; record findings, select outcome
  |
  +-- 5. Decision: Approve / Approve with modifications / Reject /
  |       Reject with guidance / Send for deeper review / Request context
  |
  +-- 6. Feedback to System: Content action (publish/block/edit),
  |       Agent feedback (structured), Knowledge Graph update,
  |       Rule refinement, Training dataset flagging
  |
  +-- 7. Learning Integration: Agent model fine-tuning pipeline,
        Prompt refinement, Constraint updates, Review metrics recorded
```

### 4.2 SLA by Review Type

| Review Type | SLA Start | SLA Deadline | 50% Notice | 80% Notice | Breach Policy |
|---|---|---|---|---|---|
| Quick Review | Queued | 2 hours | None | Auto-notify | Reassign to L1 |
| Standard Review | Queued | 24 hours | 12hr | 19hr | Escalate to L2 lead |
| Deep Review | Queued | 48 hours | 24hr | 38hr | Escalate to L3 + Head notified |
| Comprehensive Review | Queued | 72 hours | 36hr | 58hr | Head of Islamic Review assigned |
| Panel Review | Scheduled | 7 days | 3 days | 6 days | Head reviews + extends |
| External Review | Scheduled | 14-30 days | 7 days | 14/3 days | Head contacts external party |
| Urgent Correction | Queued | 4 hours | 2hr | 3hr | Head alerted, auto-assign backup |
| Emergency (platform-wide) | Queued | 1 hour | 30min | 45min | Head + CE-AI alerted, immediate action |

**SLA Rules**: Clock starts from queue entry. Pauses while awaiting additional information. Pauses during reviewer illness (max 1hr). Clock expires on decision, not system update. Emergency SLA runs regardless of time.

### 4.3 Queue Prioritization

**Priority Score** = (0.40 x Urgency) + (0.25 x SLA Proximity) + (0.20 x Impact) + (0.10 x Reviewer Match) + (0.05 x Queue Time)

Each factor scored 1-5:

| Factor | Scoring Details |
|---|---|
| Urgency | 5=Emergency, 4=Urgent, 3=Panel, 2=Standard, 1=Quick/Sample |
| SLA Proximity | 5 when < 10% remaining, 1 when > 90% remaining |
| Impact | 5=Platform-wide, 4=>10k users, 3=>1k, 2=>100, 1=<100 |
| Reviewer Match | 5=Exact specialization match, 3=Partial, 1=General |
| Queue Time | 5=>4hrs, 4=2-4hrs, 3=1-2hrs, 2=30-60min, 1=<30min |

**Queue Filters**: Domain (Quran/Hadith/Fiqh/Aqidah/Seerah/Arabic/General/Fatwa/Safety/Legal), Urgency (Emergency/Urgent/Normal/Standard/Quick), Language, Madhhab, Time in Queue, Trigger Source.

### 4.4 Auto-Assignment Algorithm

**Assignment Score** = (0.35 x Specialization Match) + (0.25 x Language Match) + (0.20 x Availability) + (0.10 x Current Load) + (0.10 x SLA Feasibility)

All factors scored 0-100. Items assigned to highest-scoring reviewer above 75. Emergency items auto-assigned regardless of load. Items must be assigned within 5 minutes or appear in manager queue.

**Reviewer Daily Max**: L1: 80 reviews (120 emergency). L2: 30 deep, 10 comprehensive. L3: 10 comprehensive, 5 panels/week.

### 4.5 Review State Machine

```
IN QUEUE -> ASSIGNED <-> IN REVIEW -> AWAITING DECISION
              |                            |
              +-- SLA Breach -> Reassign   +--> AWAITING ADDITIONAL INFO (user/agent)
                                           +--> AWAITING SECONDARY REVIEW (L2)
                                           +--> NEEDS MORE INFO -> back to IN REVIEW
                                           |
                                           +--> DECIDED
                                           +--> CLOSED (abandoned/cancelled/duplicate)
```

---

## 5. Review Types and Depth

### 5.1 Quick Review (L1)

| Attribute | Detail |
|---|---|
| Duration | 1-2 minutes |
| Reviewer | L1 content moderator |
| Eligible Content | Standard explanations, commonly asked questions, general educational content |
| Checks | Surface: correct content type, basic source attribution, no policy violations, age-appropriate language |
| Does NOT Check | Deep source verification, chain of narration, grammatical nuance, cross-madhhab consistency, theological implications |
| Max/Reviewer/Day | 120 |

**Checklist**:
- [ ] Content category matches claim
- [ ] No obvious factual errors (e.g., "Zakat is 5%")
- [ ] Source citation exists and matches domain
- [ ] No profanity, hate speech, sectarian language
- [ ] Age-appropriate for claimed audience
- [ ] Language level matches user level (basic/intermediate/advanced)
- [ ] No contradictions with platform content on same topic
- [ ] If Arabic: tashkeel present, basic spelling check
- [ ] If fatwa-like: must escalate to Standard (no quick review)

**Real Scenario**: User asks "When should I pray Fajr?" Agent generates correct time range. L1 reviewer checks: (a) correct prayer, (b) correct time, (c) Arabic terms used, (d) no policy violations. Approve in 90 seconds.

### 5.2 Standard Review (L1-L2)

| Attribute | Detail |
|---|---|
| Duration | 5-10 minutes |
| Reviewer | L1 (majority) or L2 (random sample) |
| Eligible Content | Hadith explanations, average-complexity fiqh, tafseer of clear verses, seerah, course scripts |
| Checks | Full check: source accuracy, citation verification, correct Islamic terminology, acceptable tafseer/hadith interpretation, proper madhhab attribution, no policy violations |
| Does NOT Check | Deep source chain, novel scholarly analysis, cross-madhhab comparison, legal/financial implications |
| Approval | Single L1; random 5% L2 quality check |
| Max/Reviewer | 30/day |

**Checklist**:
- [ ] Quranic verses: verify surah, ayah, exact text
- [ ] Hadith: check text against major collections, verify authenticity grade
- [ ] Fiqh ruling: check against knowledge graph, matches established positions
- [ ] Scholar opinions: verify actual stated position (avoid misattribution)
- [ ] Cross-madhhab: at least 1 ikhtilaf reference if applicable
- [ ] Citations complete: source, book, volume/page, URL
- [ ] Language: tashkeel on Quranic text, correct spelling
- [ ] Tone: respectful (adab), not overly harsh
- [ ] Audience: vocabulary level correct
- [ ] Platform policy: no prohibited topics, no extremism

### 5.3 Deep Review (L2)

| Attribute | Detail |
|---|---|
| Duration | 15-30 minutes |
| Reviewer | L2 specialized scholar |
| Eligible Content | Complex fiqh rulings, tafseer of mutashabih verses, hadith with weak chains, controversial topics, bid'ah content, contemporary issues |
| Checks | Full scholarly: source chain verification, multiple tafseer comparison, fiqh usul analysis, cross-madhhab positions, difficult Arabic grammar, implication analysis |
| Does NOT Check | Novel ijtihad, new fatwa on unprecedented issue, significant financial/legal impact content |
| Approval Process | Single L2, can be appealed to L3 |
| Max/Reviewer | 10/day |

**Deep Review Checklist**:
- [ ] Full hadith verification: isnad against main canonical source
- [ ] Multi-tafseer comparison: 3+ major tafseer
- [ ] Fiqh reasoning analysis: Is usul correct? Qiyas valid? Conditions correct?
- [ ] Cross-madhhab positions cited if ikhtilaf exists
- [ ] Arabic grammatical analysis: i'rab correct?
- [ ] Scholarly consensus: does content contradict ijma?
- [ ] Contemporary relevance: does ruling account for modern context?
- [ ] Safety: Could content be misinterpreted or misused?
- [ ] Knowledge Graph consistency: alignment with existing content

**Real Example**: Agent generates content on "Cryptocurrency in Islam." L2 scholar: (1) Checks specific fatwa from OIC or Al-Azhar; (2) Analyzes whether cryptocurrency qualifies as mal (wealth); (2) Checks gharar involvement; (4) Confirms majority view (haram) with ikhtilaf noted; (5) Approves with modification: add "Scholars differ — ask your local scholar." 23 minutes.

### 5.4 Comprehensive Review (L3)

| Attribute | Detail |
|---|---|
| Duration | 1-2 hours |
| Reviewer | L3 senior scholar |
| Eligible Content | Novel Quranic interpretation, new fatwa on unprecedented issue, major theological implications, content changing platform position, major social/financial/personal fatwa |
| Checks | Multi-source verification, extensive citations, usul al-fiqh, maqasid al-shariah, established scholarly works, safety, legal, platform impact |
| Approval Process | Single L3 + Head of Islamic Review notification (Head can override within 24hrs) |
| Max/Reviewer | 5/day |

**Checklist**:
- [ ] 5+ tafseer works consulted for Quranic analysis
- [ ] Hadith: full chain verification, jarh wa ta'dil analysis
- [ ] Usul al-fiqh: sources used, order, exceptions (istihsan, masalih mursalah)
- [ ] Maqasid al-shariah: how does this serve higher objectives?
- [ ] Scholarly consensus: verify claimed ijma, check for historical ikhtilaf
- [ ] Contemporary fatwa council positions: OIC Fiqh Academy, Al-Azhar, Dar al-Ifta'
- [ ] Legal review: if financial/social implications
- [ ] Safety: anticipate misuse, extremism potential
- [ ] Platform impact: does this contradict existing content?
- [ ] Audience analysis: implications for different demographics?
- [ ] Bias check: balanced across madhahib?
- [ ] Arabic: full linguistic analysis of Quranic Arabic
- [ ] Risk assessment: categorize (low/medium/high/very high)

**Real Example**: "Is the egg-shaped Earth reference in Quran (al-Anbiyah') related to modern science?" L3 reviews 7 tafseer, analyzes Arabic root "dahaha" (ostrich egg), checks classical tafseer (Tabari, Ibn Kathir, Shawkani), finds 3 scholarly opinions: literal vs. allegorical vs. flattened egg. Conclusion: "Present as metaphorical, not definitive scientific proof. Most classical scholars saw it as reference to inhabitable Earth." 1 hour 45 minutes.

### 5.5 Panel Review (L3-L4)

| Attribute | Detail |
|---|---|
| Duration | 4-8 hours (synchronous, 1 day) |
| Reviewer | L3 panel (3-5 senior scholars) + possibly L4 advisors |
| Eligible Content | Platform-wide content policies, major fatwa affecting thousands, significant controversy, highly sensitive topics, new curriculum design |
| Process | Independent review -> synchronous discussion -> vote -> majority position with minority report |
| Approval Process | Majority vote; Head of Islamic Review can delay for further consideration |
| Max per Month | 2-4 |

**Panel Review Process**:
1. Panel chair selected from L3
2. Chair selects 2-4 additional members (diverse specializations)
3. Materials distributed 48 hours before panel
4. Independent review (2-4 hours each)
5. Synchronous video discussion (2-4 hours)
6. Each member states position with evidence
7. Vote taken
8. Majority position + minority report documented
9. Head of Islamic Reviews final decision (24 hours)

**Panel Decision Format**:
```
Panel Review #: PR-2026-0042
Content: "Islamic ruling on AI-generated Quranic recitation for Tajweed teaching"
Panel Chair: Dr. Abdullah Al-Shaikh (L3 -- Fiqh of Technology)
Panel Members: Sheikh Omar Mahmoud (L3 -- Fiqh, Al-Azhar),
               Professor Khadija Hassan (L3 -- Hadith Sciences),
               Dr. Hamza Idris (L3 -- Usul al-Fiqh),
               Sheikh Salman Qureshi (L2 -- AI specialist, ad-hoc)

Decision: REJECT (4/5)

Majority (4): AI-generated recitation permissible (ja'iz) only if:
  1. Verified by qualified Qari before publication
  2. Explicitly labeled "AI-generated"
  3. Users encouraged to verify with human Qari
  4. Not promoted as substitute for human instruction

Minority (1): Not permitted at any level -- Tajweed requires human-to-human
  transmission (talaqqi), can cause incorrect leaning without detection.

Panel Chair Summary: Majority stands. Minority view documented.
```

### 5.6 External Review (L4)

| Attribute | Detail |
|---|---|
| Duration | 7-30 days |
| Reviewer | L4 External Advisory Board or designated institution |
| Eligible Content | Major global fatwa, platform-wide creedal statements, partnership with external bodies, content requiring institutional scholarly consensus |
| Process | Formal submission -> review -> written decision -> acceptance/rejection by Head |
| Occurrence | 2-4 per year |

**Triggers**: Platform changes stance on major theological issue, new partnership with Al-Azhar/OIC/government department, fatwa about global issue (war, pandemic, global finance), content affecting platform reputation with major Muslim organizations.

**Process**: Head drafts formal request -> L4 chair assigns reviewers -> Board reviews within timeline -> written opinion -> Head determines acceptance. If accepted: published with attribution "Reviewed by [External Board]." If rejected: Head documents reasons for divergence.

---

## 6. Review Decision Outcomes

### 6.1 Approve

| Aspect | Detail |
|---|---|
| Definition | Content accepted for publication as-is |
| When Used | Content meets all accuracy, quality, and policy standards |
| Action | Content immediately published |
| System Update | Confidence updated to 1.0 |
| Agent Feedback | Positive reinforcement: "Approved -- no corrections needed" |
| Knowledge Graph | No update (content already matches) |
| Training | Flagged as positive example |
| Metrics | Decision, reviewer ID, time taken, content type, confidence before/after |

### 6.2 Approve with Modifications

| Aspect | Detail |
|---|---|
| Definition | Content essentially correct but requires specific changes |
| When Used | Missing citation, imprecise wording, incorrect diacritics, tone adjustment |
| Action | Reviewer specifies exact changes; agent implements automatically |
| Modification Types | Text edit (add/remove/change wording), Source add, Citation correction, Tone adjustment, Disclaimer addition |
| System | Agent modifies content per specifications, re-publishes |
| Metrics | Changes count, changes type, implementation time |

**Modification Specification Format**:
```
Modification #1:
  Type: TEXT EDIT
  Location: Paragraph 3, Sentence 2
  Current: "The prophet said 'pray as you see me pray' referring all prayers"
  Proposed: "The Prophet said, 'Pray as you see me pray' (Bukhari 631), referring to the manner of prayer"
  Reason: Hadith citation missing -- added

Modification #2:
  Type: TONE ADJUSTMENT
  Location: Paragraph 5
  Current: "You must pray exactly like this"
  Proposed: "The recommended way to pray is as follows"
  Reason: Less prescriptive, matches platform tone
```

### 6.3 Reject

| Aspect | Detail |
|---|---|
| Definition | Content discarded -- cannot be published |
| When Used | Major errors: false hadith, incorrect ruling, fabricated source, harmful content |
| Action | Content blocked; not saved to knowledge graph |
| System | Agent negative feedback; confidence decreased for similar content |
| Training | Flagged as negative example |
| Agent | Model weights adjusted to penalize error patterns |

**Rejection Categories**:

| Error Category | Severity | Action |
|---|---|---|
| Fabricated hadith | Critical | Agent flagged, model adjustment, review all content from same agent |
| Incorrect Quranic quote | Critical | Agent flagged, immediate correction, review all Quranic content from agent |
| Fatwa contradicts established ijma | Major | Agent penalized, constraint added, review all fiqh content from agent |
| Misattribution to scholar | Major | Content discarded, citation check tightened |
| Incomplete ruling (missing condition) | Moderate | Discarded, agent prompted to add condition |
| Tone inappropriate | Minor | Tone parameter adjusted |

### 6.4 Reject with Guidance

| Aspect | Detail |
|---|---|
| Definition | Content rejected but detailed guidance provided for learning |
| When Used | Content has correctable errors; agent shows learning potential |
| Action | Content discarded; agent training updated with structured guidance |
| System | Rule/constraint added to prevent recurrence |
| Knowledge Graph | Potential update if rejection revealed missing knowledge |

**Guided Rejection Format**:
```
Rejection ID: R-2026-45231
Decision: REJECT WITH GUIDANCE
Error Type: Incorrect Fiqh Ruling (partial)
Guidance for Agent:
  1. Error: Stated "Zakat is due on gold at 2.5% of total gold savings"
  2. Correction: Zakat on gold is 2.5% of gold >= 85 grams (nisab),
     held for one lunar year (haul)
  3. Missing condition: You omitted nisab and haul conditions
  4. Knowledge Graph Update: Verify[Zakat, Gold, Nisab] = 85g
```

### 6.5 Send for Deeper Review

| Aspect | Detail |
|---|---|
| Definition | Current reviewer lacks expertise to make final decision |
| When Used | Unfamiliar content, complex cross-specialization, recognized limitation |
| Action | Content labeled "insufficient expertise," assigned to next level |
| System | Priority increased (based on time already spent) |

**Escalation Paths**: L1 cannot decide -> L2. L2 cannot decide -> L3. L3 cannot decide -> Head -> External Board (L4).

### 6.6 Request Additional Context

| Aspect | Detail |
|---|---|
| Definition | More information needed before making decision |
| When Used | Ambiguous event, missing source, contradictory sources |
| Action | Content held, auto-request sent to user or agent |
| System | State changed to AWAITING_INFO, SLA paused |

**Common Requests**: Source clarification, user clarification ("intentional eating?"), content reference needed, madhhab specification, audience level clarification.

### 6.7 Decision Complete Record

Every decision generates a complete audit record:

```
REVIEW DECISION RECORD: RDR-2026-89323

METADATA:
  Content ID:           C-2026-48931
  Agent ID:             A-S24-C-0072 (Fiqh Team, Agent 72)
  Trigger:              Auto (confidence = 0.68)
  Queue Time:           00:12:34
  Reviewer:             L2-007 (Dr. Ahmad Al-Tabari)
  Review Type:          Deep Review
  Decision:             Approve with Modifications (2 mods)
  Review Duration:      00:21:47

METRICS:
  Quality Score:        95 (of 100)
  Corrections:          2
  Agent Penalty:        Yes (confidence reduced for missed conditions)
  Knowledge Graph:      Add condition record for Qurbani
  Training Data:        Yes (partial correct, partial error)

TIMELINE:
  08:23:45 : Review request created
  08:25:12 : Assigned
  08:27:44 : Picked up
  08:49:31 : Decision submitted
  08:49:47 : Content published (modified version)

REVIEWER NOTES:
  Content correctly identified qurbani as wajib. Missing minimum
  animal age (1 yr for goats, 2 yrs for cows). Hadith correctly
  cited. Two modifications applied.

AGENT FEEDBACK CONFIRMATION:
  Modifications applied: 2/2
  Confidence updated: 0.68 -> 1.0
```

### 6.8 Confidence After Review

| Outcome | Confidence After |
|---|---|
| Approve | 1.0 |
| Approve with modifications | 1.0 |
| Reject | 0.0 |
| Reject with guidance | 0.0 |
| Send for deeper review | Unchanged |
| Request additional context | Unchanged |

---

## 7. Reviewer Tools and Dashboard

### 7.1 Review Interface

The review interface is a purpose-built web application optimized for rapid, accurate review of Islamic content.

**Main Review Screen Layout**:

```
+------------------------------------------------------------------------+
| [Content ID] [Type: Fiqh - Zakat] [Lang: English] [Priority: **]       |
| Created: 08:23 | SLA ends: tomorrow 08:23 | Remaining: 20:45           |
+------------------------------------------+-----------------------------+
| Content Review Pane                      | Reference Panel             |
|                                          |                             |
| Title: "Understanding Zakat"             | Source Comparison View      |
|                                          | - Quran (2:267): [OK]       |
| Body: "Zakat is the third pillar..."     | - Hadith (Bukhari): [OK]    |
|                                          | - Hadith (Muslim): [OK]     |
| [Highlighted text] -> Agent includes     | - Fiqh ruling: [OK] Hanafi |
| hadith from Bukhari Section on           | - Citation: [OK]            |
| Zakat al-Fitr...                         |                             |
|                                          | Agent Confidence: 0.68      |
| [Reviewer Notes Area]                     | Quran: 0.95, Hadith: 0.62   |
|                                          | Fiqh: 0.55, Source: 0.72     |
| The content is generally correct.        | Tone: 0.81, Safety: 0.64    |
| Zakat al-Fitr needs specific madhhab     |                             |
| reference. Also missing nisab for silver.| Citation Checker:           |
|                                          | 4/5 citations verified       |
| [Templates] [Decision] [Submit]         | Missing: Zakat al-Fitr amount |
+------------------------------------------+-----------------------------+
| Similar Content History                                                   |
| - Zakat Guide v1 (approve, 2025-09-12)                                    |
| - Zakat for Freelancers (approve with mods, 2025-11-04)                  |
| - Zakat al-Fitr (reject - wrong rate, 2025-12-20)                        |
+------------------------------------------------------------------------+
```

**Source Comparison View**: Left column = AI content, Right column = verified source from knowledge graph. Color coding: green (match), yellow (partial), red (mismatch), gray (missing). Click for side-by-side. "View Chain" for hadith isnad visualization.

**Agent Confidence Display**: Gauge (0-1.0). Overall confidence in large number. Breakdown by domain. Trending sparkline. "Agent Scorecard" link.

**Citation Checker**: Lists each citation, validated against database. Status icons: checkmark (verified), warning (partial -- correct source but wrong volume), x (not found). Manual check for complex citations.

**Similar Content History**: Top 5 similar pieces, each with decision, linked to full record. "Consistency Check" shows if current content agrees/disagrees with pre-approved content.

**Reviewer Notes**: Free text, auto-saves every 30 seconds. Supports Arabic/English script. Paste source text from Quran/hadith apps. Markdown formatting.

**Template System**: Pre-written templates for common scenarios:

- Approve with modifications: "Add Arabic text," "Specify madhhab," "Age restriction note," "Tone adjustment"
- Reject with guidance: "This hadith is fabricated", "Scholar attribution incorrect", "Ruling contradicts ijma"
- Custom templates per reviewer, team-wide templates from Head of Islamic Review

### 7.2 Dashboard KPIs

#### 7.2.1 Individual Reviewer KPIs

| KPI | Target | How Displayed |
|---|---|---|
| Reviews Completed | L1: 30-50, L2: 10-20, L3: 3-8 | Number + daily progress bar |
| Avg Review Time | Quick < 3 min, Standard < 8 min, Deep < 22 min, Comprehensive < 90 min | Number + color (green/yellow/red) |
| Approval Rate | 70-90% | Percentage + trend arrow |
| Escalation Rate | < 10% | Percentage + range indicator |
| Utilization Rate | 80-90% | Percentage bar |
| Abandoned Reviews | < 5% | Number |
| Error Rate (peer) | < 2% | Percentage + red if > 2% |
| SLA Compliance | > 95% | Percentage |

**Individual Reviewer View**:
```
Welcome, Dr. Omar Al-Tabasi [L2 Scholar]
Shift: 08:00-16:00 Mecca | Reviews today: 15 | Active: 2

KPI                    | Value     | Target     | Status
Avg Review Time        | 14.2 min  | 15 min     | On target
Approval Rate          | 82%       | 70-90%     | In range
Escalation Rate        | 8%        | <10%       | Acceptable
Utilization            | 78%       | 80-90%     | Below optimal
SLA Compliance         | 100%      | >95%       | Excellent
Error Rate (peer)      | 1.2%      | <2%        | Acceptable
```

#### 7.2.2 Management Dashboard KPIs

| KPI | Target |
|---|---|
| Total Reviews Completed | 500-800/day (all levels) |
| Avg Queue Wait Time | < 30 min standard, < 15 min urgent |
| Backlog Size | < 200 items, < 50 overdue |
| SLA Breach Rate | < 5% of all reviews |
| Reviewer Accuracy | > 95% (peer review audits) |
| Inter-Rater Reliability (Cohen's Kappa) | > 0.85 |
| Random Sample Pass Rate | > 99% |
| Rejection vs Escalation Ratio | 80:20 |
| Cost per Review | $0.50-$5.00 depending on depth |
| Emergency Response Time | < 60 min |

**Management Dashboard View**:
```
[HUMAN REVIEW BOARD] Dashboard Manager

TODAY'S PERFORMANCE:
Reviews Completed: 523 (L1: 412, L2: 89, L3: 22)
vs Yesterday: +12% | vs Last Week: +5% | vs Ramadan avg: -30%

SLA Status              | Current       | Target       | Performance
Quick                   | Avg: 45 min   | < 2 hrs      | Excellent
Standard                | Avg: 12.3 hrs | < 24 hrs     | Excellent
Deep                    | Avg: 32 hrs   | < 48 hrs     | Excellent
Comprehensive           | Avg: 55 hrs   | < 72 hrs     | Excellent
Emergency (1hr)         | Avg: 0.86 hrs | < 1 hr       | Excellent
Urgent (4hrs)           | Avg: 2.1 hrs  | < 4 hrs      | Excellent

Queue Status            | Active Items | Avg Wait
Quick                   | 12           | 45 min
Standard                | 89           | 3.2 hrs
Deep                    | 12           | 2.5 hrs
Comprehensive          | 4            | 0.75 hrs
Emergency               | 0            | N/A

Reviewer Status         | Online | On-Call | Offline | Capacity
L1 (10 total)           | 8      | 1       | 1       | 80%
L2 (8 total)            | 5      | 2       | 1       | 88%
L3 (4 total)            | 2      | 1       | 1       | 75%
Legal (3 total)         | 2      | 1       | 0       | 100%
Safety (4 total)        | 3      | 1       | 0       | 100%
```

### 7.3 Notification System

| Notification Type | Channel | Trigger | Priority |
|---|---|---|---|
| New assignment | In-app toast + SMS | Item assigned | Medium |
| SLA 50% | In-app notice | Time = 50% SLA | Low |
| SLA 75% | In-app + push | Time = 75% SLA | High |
| SLA breach imminent | Urgent in-app + push | Time = 90% | Critical |
| SLA breach | Phone call to manager | SLA expired | Critical |
| High-priority reassignment | In-app + push | Emergency override | Critical |
| Shift reminder | Push (30 min before) | Shift start | Low |
| Fatigue warning | In-app dialog | Reviews > 120% max | High |

**SLA Warning Levels**: Green (> 50% remaining) -> Yellow (25-50%) -> Orange (10-25%) -> Red (< 10%) -> Breached (auto-escalate).

**Batch Completion Notifications**: "Completed 10 reviews. Avg 3.2 min. Accuracy 100%. Take a break." If accuracy drops: "Accuracy dropped to 92% in this batch. Consider longer break."

---

## 8. Human Feedback Integration

### 8.1 Immediate Feedback (Template-Based)

| Category | Template ID | Trigger | Example Text |
|---|---|---|---|
| Missing Arabic text | FB-MA-001 | Content lacks original Arabic for Quranic verse | "Include Arabic text of verse before translation: [Arabic][Translation][Surah:Ayah]" |
| Incorrect hadith grade | FB-HG-002 | Grade differs from reviewer's assessment | "This hadith is hasan, not sahih. Albani grades it hasan." |
| Missing madhhab reference | FB-MR-001 | Fiqh ruling without madhhab | "Specify school: 'According to the Hanafi school:' before the ruling." |
| Tone too prescriptive | FB-TP-001 | "Must" for recommended acts | "Change to recommendation language as there is ikhtilaf on this." |
| Missing citation | FB-MC-001 | Content without source | "All rulings need citations — at minimum Quran or hadith reference." |
| Age inappropriateness | FB-AI-001 | Content too advanced/too basic | "This content is for intermediate learners. Current level is advanced." |
| Sectarian language | FB-SL-001 | Language favoring one madhhab negatively | "Remove phrase 'only the Hanafi view is correct'. Present ikhtilaf neutrally." |
| Insufficient disclaimer | FB-ID-001 | Omits scholarly difference | "Add: 'Some scholars hold a different view; please consult your local scholar.'" |

### 8.2 Structured Feedback Format

When an agent receives feedback, it comes in a standardized JSON format:

```json
{
  "review_id": "R-2026-45231",
  "agent_id": "A-S24-C-0072",
  "decision": "APPROVE_WITH_MODIFICATIONS",
  "feedback_items": [
    {
      "type": "MISSING_CONDITION",
      "severity": "major",
      "element": "zakat_al_fitr_amount",
      "location": "paragraph_4",
      "error": "Missing specific zakat al-fitr amount per madhhab",
      "correction": "Zakat al-Fitr amount: 2.25kg wheat or equivalent (Hanafi)",
      "source": "Kitaab al-Zakat, al-Hidayah",
      "learning_impact": "ADD_TO_CHECKLIST"
    },
    {
      "type": "MISSING_ARABIC",
      "severity": "minor",
      "location": "paragraph_1",
      "error": "Hadith text not in original Arabic",
      "correction": "Add Arabic text for 'Buniya al-Islamu ala khams'",
      "source": "",
      "learning_impact": "TEMPLATE_UPDATE"
    }
  ],
  "overall_quality_score": 82,
  "reviewer_notes": "Good structure and sources. Add conditions for completeness."
}
```

### 8.3 Feedback Channels

| Channel | Description | Used For | Latency |
|---|---|---|---|
| Template-based | Pre-approved correction templates | Common issues | Real-time |
| Structured JSON | Machine-readable correction format | All reviews | Real-time |
| Training data pipeline | Flagged content sent to training dataset | Content flagged as training example | Daily batch |
| Rule creation | Human decision converted to system rule | Repetitive error patterns | Near real-time |
| Knowledge Graph update | New verified information added to KG | Missing or corrected facts | Real-time |
| Agent model fine-tuning | Corrections aggregated into training batches | Performance improvement | Weekly |
| Prompt refinement | Agent prompts updated based on feedback patterns | Systematic errors | Bi-weekly |
| Constraint addition | Hard rules added to agent behavior | Repeated critical errors | Immediate |

### 8.4 Feedback Flow Architecture

```
Human Decision
  |
  +-- Template match? -> Immediate template feedback to agent (real-time)
  |
  +-- Structured feedback generation (every review)
  |     |
  |     +-- Agent notification (real-time, in-app)
  |     +-- Agent confidence update (same session)
  |     +-- Knowledge Graph check (auto-update if new fact)
  |
  +-- Training data pipeline (daily batch)
  |     |
  |     +-- Classification: positive/negative/instructive example
  |     +-- Sent to training data lake
  |     +-- Flagged for model fine-tuning queue
  |
  +-- Rule/constraint evaluation
  |     |
  |     +-- Is this a new pattern? Yes -> Create system rule
  |     +-- Is this related to existing rule? -> Refine rule
  |     +-- Rule immediately applied to all agents
  |
  +-- Aggregation pipeline (weekly)
        |
        +-- Head of Islamic Review report
        +-- Departmental feedback trends
        +-- Agent-specific improvement recommendations
```

---

## 9. Quality Assurance of Human Reviews

### 9.1 Peer Review System

5% of all L1 reviews are randomly selected for re-review by L2 scholars. This serves as a quality check on reviewer accuracy.

| Parameter | Value |
|---|---|
| Peer review rate | 5% of all L1 reviews |
| Reviewer | L2 scholar (randomly assigned, not same as original reviewer's team) |
| Selection | Stratified random: 5% across all review types, weighted toward higher-complexity items |
| Action if approved | Counted as quality confirmation |
| Action if disagreed | Recorded as reviewer error, sent to reviewer's manager for coaching |
| Reviewer error tracking | Running 30-day error rate per reviewer |

**Peer Review Process**:
1. System selects random L1-reviewed item
2. Item appears in L2 queue as "Quality Check - Blind Review"
3. L2 reviews item without seeing L1 decision
4. L2 makes independent decision
5. System compares L1 and L2 decisions
6. If agreement: recorded (strengthens reviewer's accuracy score)
7. If disagreement: escalated to L3 for tiebreaker
8. L3's decision is final, recorded as reviewer error or reviewer accuracy confirmation

### 9.2 Audit Trail

Every review decision is logged with full metadata:

| Field | Example |
|---|---|
| Review ID | R-2026-89323 |
| Reviewer ID | L1-042 |
| Content ID | C-2026-48931 |
| Decision | APPROVE |
| Timestamp | 2026-01-15 08:49:31 UTC |
| Review Duration | 00:07:23 |
| Reviewer Notes | "Correct content, accurate sources, no issues." |
| Peer Review Status | In progress |
| Peer Review Decision | MATCH (if completed) |
| Audit Version | 1 |
| Modified By | (empty if no override) |
| Override Reason | (empty if no override) |

### 9.3 Reviewer Accuracy Tracking

**Accuracy Score** (0-100%) calculated as:

- Base accuracy: % of peer-reviewed decisions that match L2/L3 consensus
- Adjusted for complexity: weighted by CRT (content review type) difficulty factor
- Weighted for recency: last 30 days weighted 70%, prior months 30%

**Accuracy Tiers**:

| Tier | Score | Status | Consequence |
|---|---|---|---|
| Excellent | > 95% | Green | No action |
| Good | 90-95% | Green | Standard monitoring |
| Acceptable | 85-89% | Yellow | Increased peer review rate to 10% |
| Needs improvement | 80-84% | Orange | Manager conversation, coaching, reduced review count |
| Poor | < 80% | Red | Probation, mandatory retraining, reassignment if no improvement |

**Accuracy Report (Monthly per Reviewer)**:

```
Reviewer: L1-042 (Fatima Al-Khalid)
Period: Dec 2025 - Jan 2026
Review Count: 1,247
Accuracy Score: 94.3% (Excellent)

Peer Reviews This Month: 62
Agreement: 58 (93.5%)
Disagreement: 4 (6.5%)
  - 3 overridden by L2 -> Reviewer correct (disagreement resolved)
  - 1 overridden by L3 -> Reviewer error

Error Breakdown:
  - Missed citation (1): Added citation that was incorrect
  - Incorrect tone assessment (1): Approved content with borderline tone

Trend: Stable (94-96% over 3 months)
Action: None required - satisfactory performance.
```

### 9.4 Calibration Sessions

Monthly sessions where all reviewers meet (virtual) to align on review standards.

| Session Type | Frequency | Duration | Participants | Purpose |
|---|---|---|---|---|
| All-reviewer calibration | Monthly | 2 hours | All L1, L2, L3 | Review 10 pre-selected items, discuss disagreements, align criteria |
| L2-L3 alignment | Bi-weekly | 1 hour | L2 + L3 scholars | Discuss complex rulings, emerging fiqh issues, novel interpretations |
| Cross-madhhab training | Monthly | 1 hour | Required for all L1+ | Education on madhhab positions, reducing sectarian bias |
| New fatwa updates | As needed | 1 hour | L2+ | Review new fatwa from OIC, Al-Azhar, ISESCO |
| Language precision | Quarterly | 2 hours | All reviewers | Arabic grammar refresher, new terminology |
| Safety recalibration | Monthly | 1 hour | All reviewers | Updated safety guidelines, emerging threats |

**Calibration Session Format**:
1. 10 pre-selected items distributed 48 hours prior
2. Each reviewer makes independent decision
3. Session reveals all decisions, identifies disagreements
4. Group discussion of each disagreement: why did reviewers disagree?
5. Consensus criteria established for future items
6. Updated guidelines distributed

### 9.5 Blind Re-review

Random re-submission of pre-reviewed content to reviewers to test consistency.

| Parameter | Value |
|---|---|
| Rate | 1% of all approved content |
| Method | Content removed from "reviewed" state, re-sent to different reviewer as new item |
| Minimum gap | 30 days after original review |
| Comparison | Compare new decision to original decision |
| Measurement | Intra-rater reliability (same reviewer, different time) and inter-rater reliability (different reviewer) |

### 9.6 Inter-Rater Reliability (Cohen's Kappa)

| Kappa Value | Interpretation |
|---|---|
| > 0.90 | Excellent agreement |
| 0.85 - 0.90 | Good (acceptable) |
| 0.80 - 0.84 | Moderate (needs improvement) |
| 0.75 - 0.79 | Fair (concern - calibration needed) |
| < 0.75 | Poor (systemic issue) |

Target **> 0.85** for reviewers at same level. Monthly calculation.

**Kappa Calculation**: On double-reviewed items (peer reviews), Cohen's Kappa measures the agreement between reviewers beyond chance. Value of 0.85 means 85% agreement above what would be expected by random chance.

---

## 10. Contentious and Edge Cases

### 10.1 Disagreement Between Reviewers

| Scenario | Resolution Process | Time to Resolve | Final Authority |
|---|---|---|---|
| L1 vs L1 | Escalate to L2 for judgment | Within 24 hours | L2 scholar |
| L1 vs L2 | Escalate to L3 for final decision | Within 48 hours | L3 senior scholar |
| L2 vs L2 | Panel vote (3 L2 scholars) | Within 72 hours | Majority vote |
| L2 vs L3 | L3 decision stands | Immediate | L3 senior scholar |
| L3 vs L3 | Panel review (Head of Islamic Review chairs) | Within 5 days | Head of Islamic Review |
| L3 vs L4 | Extensive panel discussion | Within 14 days | Head + L4 external opinion |
| Review vs System | System model disagrees with human | Human always overrides | Human decision stands |

### 10.2 Reviewer Unavailable

| Scenario | Procedure |
|---|---|
| Reviewer falls ill mid-review | Auto-save draft, reassign to next available reviewer at same level |
| Reviewer leaves shift mid-item | Same as above, SLA clock continues |
| Reviewer does not pick up assignment within 30 min | Auto-reassign, flag to manager |
| Reviewer unavailable for extended period | Manager assigns temporary replacement, shift coverage adjusted |
| Reviewer quits/moved | Content re-assigned, new reviewer notified of transition, SLA extended by 50% |

**SLA Extensions for Unavailability**:
- Illness: SLA extended by actual time of illness (max 4hrs)
- Emergency personal leave: SLA extended by 2 hours
- Technical issue (platform down): SLA paused, resumed when system back

### 10.3 Reviewer Bias Detection

| Bias Type | Detection Method | Algorithmic Signal | Action |
|---|---|---|---|
| Favoring specific madhhab | Statistical comparison of approval rates by madhhab | If approval rate for madhhab A is > 10% higher than madhhab B | Weekly report to manager, calibration session, content distribution review |
| Harsher on certain content types | Comparison of rejection rates across content types | Rejection rate for topic X is > 20% higher than average | Discuss in calibration, assess if justified or biased |
| Leniency bias | Comparison with peer review | Subject review score is significantly more favorable than others | Increase peer review rate to 20%, mandatory calibration |
| Stringency bias | Comparison with peer review | Score significantly less favorable | Same as leniency |
| Reviewer fatigue | Degradation of accuracy over shift | Error rate in last 2 hours > 2x first 2 hours | Auto-pause reviews, enforced break, adjust shift schedule |
| Cultural bias | Differential scoring for same content in different languages | Arabic vs English approval rate gap > 10% | Training on linguistic bias, review of translation quality |
| Sectarian bias | Approval rate for content from specific sect | Higher approval for one sect over others | Immediate suspension pending investigation |

**Bias Detection Dashboard**:
```
BIAS ALERTS
Reviewer: L1-042 | Period: Last 30 days | Reviews: 1,247

Madhhab Balance:
  Hanafi approval: 84% (avg across reviewers: 82%) -- Within normal range
  Shafi'i approval: 86% (avg: 81%) -- Slightly elevated, monitor
  Maliki approval: 79% (avg: 80%) -- Within normal range
  Hanbali approval: 81% (avg: 82%) -- Within normal range
  Ja'fari approval: 72% (avg: 70%) -- Within normal range
  ALERT: None

Content Type Bias:
  Fiqh approval: 82% (avg: 78%) -- Elevated
  Hadith approval: 79% (avg: 80%) -- Normal
  ALERT: Fiqh content approved 4% above average. Check if content genuinely
  deserves higher approval or if reviewer is lenient on fiqh.

Shift Fatigue Analysis:
  Reviews 8-10am: 96% accuracy
  Reviews 10-12pm: 95% accuracy
  Reviews 12-2pm: 94% accuracy
  Reviews 2-4pm: 91% accuracy [DETECTED: >2% drop per 2hr block]
  ALERT: Significant accuracy decline in final 2 hours of shift.
  Action: Remove from queue at 2pm for 30-min mandatory break.
```

### 10.4 Fatigue Detection and Mitigation

**Fatigue Indicators**:

| Metric | Normal Range | Fatigue Signal | Action |
|---|---|---|---|
| Review time | Quick 1-3 min, Standard 5-10 min | Quick > 5 min, Standard > 15 min | Offer break, check for burnout |
| Accuracy (peer review) | > 95% | < 90% in last hour | Immediate break, reduce shift tomorrow |
| Skipped reviews | < 2% of assigned | > 5% of assigned in last hour | Fatigue + disengagement, reassign items |
| Approval rate shift | +/- 5% of personal baseline | > 10% shift (+/-) | Possible fatigue affecting judgment |
| Typing errors | < 3 per 100 words | > 8 per 100 words in notes | Cognitive fatigue, suggest break |
| Decision reversal | < 1% of reviews | > 3% "change mind" before final | Decision fatigue, reassign to easier items |

**Fatigue Mitigation Protocol**:
1. Yellow alert: System shows break reminder in-app, optional
2. Orange alert: System forces 5-minute break (reviews locked)
3. Red alert: System removes all items from queue, assigns to others, 30-minute mandatory break
4. If red triggers twice in one week -> manager notified for shift adjustment
5. Permanent issue: reduced review limit per day

**Shift Design for Fatigue Prevention**:
- L1 reviewers: max 6 hours of active reviewing per 8-hour shift
- L2 scholars: max 5 hours per shift
- L3 scholars: max 4 hours per shift
- Mandatory 10-minute break every 2 hours
- Mandatory 30-minute lunch break
- No more than 5 consecutive days of reviews
- Variety of review types encouraged (don't assign same type all shift)

### 10.5 External Manipulation Attempts

**Coordinated Content Flagging Attacks**:

| Attack Type | Detection Signal | Mitigation |
|---|---|---|
| Bot accounts flagging content | Flag rate > 50/hour from single IP, multiple accounts | IP block, user account suspension, flag count deducted |
| Targeted flagging of specific scholar's content | Multiple flags from different users on same scholar account | Investigate, rate-limit complaints, contact scholar if suspicious |
| Fake verified scholar accounts flagging | Verified account behavior pattern mismatch (e.g., scholar from Egypt flagging fiqh in Indonesia) | Suspicious behavior flag, manual verification, account review |
| Flagging to remove content the attacker doesn't like | High flagging rate from users who never report other content | Rate-limit, require evidence for each flag, escalate to safety team |
| DDoS of review queue | Abnormally high review request volume (100x normal) | Queue rate limit, cap emergency items, notify Head |

**Rate Limiting Rules**:

| User Action | Standard Limit | Verified Scholar Limit | New Account Limit (<30 days) |
|---|---|---|---|
| Content flags/hour | 5/hr | 50/hr | 1/hr |
| Content flags/day | 20/day | 200/day | 2/day |
| Review requests (user-pull) | 10/day | 50/day | 2/day |
| Appeals | 3/day | 10/day | 0 |

**Suspicious Activity Protocol**:
1. System detects unusual flagging pattern
2. Automatic rate limiting applied
3. Manager receives alert
4. Manager investigates: user history, IP, flagging behavior
5. If confirmed manipulation: user banned, flags invalidated
6. If not confirmed: limit removed, monitoring continues

---

## 11. Human-in-the-Loop Learning

### 11.1 Learning Loop Architecture

Every human review is a learning opportunity for the AI system. The loop works as follows:

```
Human Review Decision
  |
  +---> Corrective Action (immediate fix)
  |       |
  |       +--- Content published/blocked/edited
  |
  +---> Agent Feedback (same session)
  |       |
  |       +--- Structured correction sent to agent
  |       +--- Agent updates internal state
  |       +--- Confidence adjustment for similar future content
  |
  +---> Knowledge Graph Update (real-time)
  |       |
  |       +--- New facts added
  |       +--- Incorrect facts flagged for removal
  |       +--- Relationships updated
  |
  +---> Training Data Pipeline (daily)
  |       |
  |       +--- Content flagged as positive/negative/instructive
  |       +--- Added to training dataset
  |       +--- Sent to model fine-tuning queue
  |
  +---> Rule/Constraint Engine (near real-time)
  |       |
  |       +--- New rule created (system constraint)
  |       +--- Existing rule refined
  |       +--- Rule propagated to all agents
  |
  +---> Aggregation & Analysis (weekly)
          |
          +--- Trends identified
          +--- Department reports
          +--- Head of Islamic Review insights
```

### 11.2 Feedback Categories

| Category | Code | Description | Example |
|---|---|---|---|
| Factual correction | FF-001 | Content contains incorrect Islamic fact | "Zakat is 2.5%, not 5%" |
| Source error | FF-002 | Source is incorrect or fabricated | "Hadith not found in any canonical collection" |
| Missing context | FF-003 | Content lacks necessary qualification | "Not specifying which madhhab the ruling applies to" |
| Incomplete | FF-004 | Content missing conditions or exceptions | "Zakat rule without nisab requirement" |
| Better explanation | FF-005 | Meaning is correct but explanation is unclear | "Could explain 'taqwa' with more examples" |
| Inappropriate tone | FF-006 | Tone is too harsh, prescriptive, or condescending | "Use 'recommended' instead of 'must'" |
| Insufficient citations | FF-007 | Ruling stated without supporting evidence | "Claim without Quran or hadith reference" |
| Error in reasoning | FF-008 | Qiyas (analogical reasoning) is flawed | "Qiyas between wine and kombucha is in a pplicable" |
| Language error | FF-009 | Arabic grammar or spelling mistake | "Incorrect tashkeel on Quranic text" |
| Age inappropriateness | FF-010 | Content not suitable for target age group | "Advanced fiqh discussion for beginners" |
| Safety concern | FF-011 | Content could harm user or platform | "Might encourage unsafe behavior" |
| Bias detected | FF-012 | Content shows preference for one madhhab | "Only presenting Hanafi view as correct" |

**Weekly Feedback Aggregation Report** (to Head of Islamic Content):

```
FEEDBACK AGGREGATION REPORT (Week 03, Jan 2026)

Total Reviews This Week: 3,841
Total Feedback Items: 7,823 (avg 2.04 per review)

Top Feedback Categories:
  Missing context        1,842 (25.5%)  [Observations: Conditions still missed]
  Source error           1,123 (15.5%)  [Main issue: hadith grade misstatements]
  Wrong tone               892 (12.3%)  [Maint: overprescriptive language]
  Better explanation       721 (10.0%)  [Use more examples for key concepts]
  Other (7 categories)   2,656 (36.7%)  [See breakdown]

Most Improved Agent Category: A-S24 group (Hadith agents)
  Error reduction: 22% week-over-week
  Main improvement: hadith grade accuracy

Most Problematic Agent: A-S22-C-014 (Fiqh rulings agent)
  Error rate: 18% (avg across fiqh agents: 9%)
  Frequent errors: missing conditions (45%), wrong madhhab reference (30%)
  Recommended action: Model retraining, prompt refinement

Trend: Downward trend in source errors (good), stable tone errors (needs focus)

Head of Islamic Content Notes:
  "Continue focus on tone refinement. Missing conditions remain a top issue
  across fiqh agents. Recommend additional training data from condition-rich
  examples. Hadith agents improving well -- specific prompt engineering
  working as intended."
```

### 11.3 Agent Behavior Adjustment

Based on aggregated human feedback, several types of adjustments are made:

| Adjustment Type | Trigger | Frequency | Responsibility | Example |
|---|---|---|---|---|
| Model retraining | Cumulative feedback indicates pattern error across many agents | Weekly (or critical: immediate) | AI Training Team | "All fiqh agents fine-tuned with 1,000 examples of condition-complete rulings" |
| Prompt refinement | Specific agent consistently makes same error | Bi-weekly | Prompt Engineering Team | "Add to fiqh agent prompt: 'Always include: ruling + conditions + exceptions + scholarly difference'" |
| Constraint addition | New rule from human review that must be enforced | Immediate | Knowledge Graph Team | "Add constraint: 'Zakat content must include nisab threshold for both gold and silver'" |
| Knowledge graph correction | Systematic source error detected | Daily | Knowledge Graph Team, L3 Scholar approval | "Correction: hadith XYZ grade updated from sahih to hasan in knowledge graph" |
| Agent assignment limit | Agent error rate > threshold | Monthly | Operations Team | "Agent A-S22-C-014 error rate 18% > 10% threshold. Removed from all fiqh content generation, placed in retraining" |
| Agent deactivation | Agent produces dangerous content | Immediate | Head of Islamic Review, CE-AI | "Agent A-S09-C-003 generated fabricated hadith -> immediately suspended, investigation" |

**Agent Retraining Escalation Levels**:

| Level | Error Rate (rolling 30-day) | Action |
|---|---|---|
| Green | < 5% | Standard monitoring |
| Yellow | 5-10% | Increase random sampling to 10%, targeted prompt refinement |
| Orange | 10-15% | Agent removed from production, retraining cycle, additional constraints |
| Red | > 15% or any critical error | Agent deactivated, full root cause analysis, potential new training approach |

**Agent Scorecard (Weekly)**:

```
Agent Scorecard: A-S22-C-014 (Fiqh Rulings - Zakat Specialist)
Week: 1-7 Jan 2026

Accuracy: 82.3% (Average: 92.1%)
Error rate: 17.7% (Critical: 0, Major: 3, Moderate: 8, Minor: 12)

Top Errors:
  - Missing nisab conditions: 8 occurrences (38% of errors)
  - Wrong madhhab attribution: 5 occurrences (24%)
  - Incorrect rate: 3 occurrences (14%)
  - Missing haul condition: 2 occurrences (10%)

Status: ORANGE - Agent removed from production. Retraining in progress.
Expected return: 14 Jan 2026.
```

---

## 12. Scaling Human Review

### 12.1 Review Volume Prediction

Historical data and content pipeline throughput are used to predict review volumes.

| Content Type | Daily Production | Auto-approve Rate | Human Review Rate | Daily Reviews |
|---|---|---|---|---|
| Quranic content | 50,000 items | 97% | 3% | 1,500 |
| Hadith content | 80,000 items | 93% | 7% | 5,600 |
| Fiqh rulings | 120,000 items | 90% | 10% | 12,000 |
| Aqidah content | 30,000 items | 95% | 5% | 1,500 |
| Seerah/biography | 40,000 items | 96% | 4% | 1,600 |
| Arabic language | 60,000 items | 98% | 2% | 1,200 |
| General Islamic Ed | 100,000 items | 92% | 8% | 8,000 |
| Fatwa responses | 20,000 items | 70% | 30% | 6,000 |
| **Total** | **500,000** | -- | -- | **37,300** |

**Monthly Volume**: 37,300 x 30 = ~1.12 million reviews/month

**Peak Volume Scenarios**:
- Ramadan: 300% increase in content queries -> ~112,000 reviews/day
- Hajj: 200% increase in content -> ~75,000 reviews/day
- Product launch (new course): 50% increase -> 56,000 reviews/day

### 12.2 Reviewer Capacity Planning

**Current Capacity** (standard season):

| Level | # Reviewers | Reviews/Reviewer/Day | Total Reviews/Day | Hours Reviewing |
|---|---|---|---|---|
| L1 | 10 | 35 (avg) | 350 | 6 hrs/day |
| L2 | 8 | 15 (avg) | 120 | 5 hrs/day |
| L3 | 4 | 5 (avg) | 20 | 4 hrs/day |
| Total | 22 | -- | 490 | -- |

**Capacity Gap**: 37,400 reviews/day required - 490 capacity/day = 36,910 gap

**Why the gap exists and how it is resolved**: The vast majority of reviews are handled by the automated quality assessment pipeline. Human reviewers only see a fraction of all content (those that trigger review or are flagged). The 37,400 reviews/day reflects only the queue that reaches humans.

**Actual Queue Volume After Filtration**:

| Review Source | Volume | % of Total Generated | Human Review Required |
|---|---|---|---|
| Low confidence (< 0.6) | 25,000 | 5% | Yes (100% trigger) |
| Agent escalation (L5+) | 500 | 0.1% | Yes |
| User complaints | 1,000 | 0.2% | Yes |
| Random sample (1%) | 5,000 | 1% | Yes |
| Human initiated | 200 | 0.04% | Yes |
| Cross-madhhab flagged | 1,000 | 0.2% | Yes |
| Safety flagged | 500 | 0.1% | Yes |
| **Total Human Review** | **~33,300** | **~6.66%** | **--** |

**Of these 33,300**:
- Quick/Standard (L1): ~28,000 (84%)
- Deep (L2): ~4,000 (12%)
- Comprehensive (L3): ~1,000 (3%)
- Panel (L3-L4): ~100 (0.3%)
- External (L4): ~200 (0.6%) [annualized]

### 12.3 Capacity vs Demand

| Level | Demand/day | Current Capacity | FTE Gap |
|---|---|---|---|
| L1 | 28,000 | 350 (10 reviewers) | Huge gap = automate majority |
| L2 | 4,000 | 120 (8 scholars) | Gap = increase to 20 L2 |
| L3 | 1,000 | 20 (4 seniors) | Gap = increase to 10 L3 |

**Actual Strategy**: This document specifies the theoretical full-scale operation. In practice, the platform uses AI agents to dramatically filter the volume reaching humans:

**AI Pre-filtering**:
- Automated agents handle 93.34% of all content without human touch (500k produced, 33.3k sent to human)
- AI agents grow more accurate over time, reducing human review percentage
- 6.66% human review is initial target; goal is 3% within 2 years

**For the 6.66% that reaches humans**:
- L1 reviewers (trained content moderators) handle 84% of volume
- L2 scholars handle 12%
- L3 senior scholars handle 3%
- L4 external board handles < 0.5%

**Full FTE Plan**:

| Level | Current | Goal (6 months) | Goal (1 year) | Goal (2 years) |
|---|---|---|---|---|
| L1 | 10 | 30 | 50 | 30 (automation reduces) |
| L2 | 8 | 12 | 20 | 15 |
| L3 | 4 | 6 | 10 | 8 |
| L4 | 7 | 9 | 11 | 11 |
| Safety | 5 | 8 | 12 | 10 |
| Legal | 3 | 4 | 5 | 5 |
| Total | 37 | 78 | 118 | 79 |

### 12.4 Seasonal Adjustments

| Season | Volume Multiplier | L1 Required | L2 Required | L3 Required | Duration |
|---|---|---|---|---|---|
| Normal | 1x | 10 | 8 | 4 | Ongoing |
| Ramadan | 3x | 30 (hire temps + full activation of on-call) | 15 (full time + all on-call) | 6 (full time) | 30 days |
| Hajj | 2x | 20 (full activation) | 12 | 5 | 14 days |
| New academic year | 1.5x | 15 (activate part-time) | 10 (activate on-call) | 4 | 60 days |
| Exam period | 1.3x | 13 | 9 | 4 | 30 days |
| Eid | 0.5x | 5 (minimum coverage) | 3 (on-call) | 2 (on-call) | 5 days |

**Seasonal Staffing Strategy**:
1. Retain a pool of 20 part-time L1 reviewers (flexible scheduling, quarterly availability)
2. Build a list of 10 on-call L2 scholars (for madhhab-specific surges)
3. Partner with 5 Islamic universities for seasonal talent
4. Pre-record calibration sessions for seasonal hires
5. During Ramadan: split shifts into 4-hour blocks (14hrs/day coverage) to prevent fatigue

### 12.5 Cost Optimization

| Review Level | Volume Share | Cost per Review | Annual Cost |
|---|---|---|---|
| L1 (Quick/Standard) | 80% | $0.50-$1.00 | $5.1M-$10.2M |
| L2 (Deep) | 15% | $5.00-$10.00 | $7.3M-$14.6M |
| L3 (Comprehensive) | 4% | $15.00-$25.00 | $2.2M-$3.6M |
| L4 (Panel/External) | 1% | $100-$500 | $36,500-$182,500 |
| **Total** | **100%** | -- | **$14.6M-$28.6M/year** |

**Cost Reduction Strategies**:

1. **AI Confidence Improvement**: Each 1% increase in auto-approval rate saves $730K-$1.4M/year (calculated on 500k items/day x 1% x average cost savings)

2. **L1 Automation**: As per-madhhab agents improve, reduce L1 quick reviews (target: replace 50% of L1 quick reviews with automated validation within 18 months)

3. **Review Batching**: L1 reviewers trained to batch-scan 5 items at once, reducing per-item time by 30%

4. **Template Acceleration**: Pre-approved templates reduce L1 decision time from 3 min to 1.5 min

5. **Tiered Compensation**: Most reviews handled by lower-cost L1; expensive L3 and L4 reserved for true edge cases only

6. **Geographic Arbitrage**: L1 reviewers can be hired in lower-cost regions (SE Asia, South Asia) for $30k-$40k/year vs $45k-$60k in Middle East

### 12.6 Automation of L1 Reviews

**Roadmap for AI Trust Improvement**:

| Phase | Timeline | Human Review Rate | AI Confidence Target | Automation Goal |
|---|---|---|---|---|
| Launch | Month 1-6 | 6.66% (33k/day) | > 0.8 for 70% of content | 30% of L1 reviews automated |
| Growth | Month 7-18 | 5% (25k/day) | > 0.85 for 80% of content | 50% of L1 reviewed automated |
| Optimization | Month 19-30 | 3.5% (17.5k/day) | > 0.9 for 85% of content | 70% of L1 automated |
| Maturity | Month 31+ | 2% (10k/day) | > 0.95 for 90% of content | 85% of L1 automated |

---

## 13. Ethics and Compliance

### 13.1 Reviewer Independence

**Independence Principles**:
- Reviewers must not have any personal or financial stake in the content they review
- Reviewers cannot review content generated by agents they trained or helped develop
- Reviewers cannot review content in areas where they have expressed a rigid personal opinion publicly (recusal process)
- Reviewers must disclose any relationship with content creators, scholars cited, or institutions referenced

**Conflict of Interest Policy**:

| Scenario | Is it a Conflict? | Action |
|---|---|---|
| Reviewer review content citing their own teacher | Conflict (if reviewer is biased towards their teacher) | Reviewer must disclose, content reassigned |
| Reviewer review content about their own madhhab | Potential conflict (may favor their school) | If madhhab-specific content: must recuse or include disclosure "Reviewer is a Hanafi scholar" |
| Reviewer is shown content they helped research | Conflict (cannot review own work) | Immediate recusal |
| Reviewer has published academic paper on the topic | Potential conflict (may defend their position) | Recuse, reassign to scholar with no publication on topic |
| Reviewer receives gift from platform user or content creator | Conflict (bribery risk) | Gifts policy: max $50 value annually, all gifts disclosed. Breach triggers investigation |
| Reviewer is related to a platform executive | Conflict (potential bias/deference) | Recuse from all reviews related to that executive's department |

**Disclosure Form** (completed by each reviewer quarterly):

```
CONFLICT OF INTEREST DISCLOSURE

Reviewer Name: Dr. Khadija Hassan
Quarter: Q1 2026

1. Primary madhhab affiliation: Hanafi
2. Ijazah held in: Tafseer, Hadith
3. Teachers/mentors: Sheikh Muhammad Al-Yamani, Dr. Aisha Al-Qurtubi
4. Published works this year: "Understanding Ta'weel in Modern Context" (Journal of Islamic Studies)
5. Platform-related investments: None
6. Relationship with platform employees: None
7. Content topics where I have publicly expressed strong opinions:
   - Ijtihad in the modern age (published 2 papers)
   - The role of women in Islamic scholarship (public lecture, 2025)
8. I confirm that if assigned content on these topics, I will recuse myself: Yes / No

Signature: ___________________ Date: __________
```

### 13.2 Conflict of Interest Resolution Process

1. Reviewer identifies potential conflict (self-reporting expected)
2. Reviewer requests reassignment via dashboard ("Conflict - recuse")
3. System logs the recusal (for bias tracking)
4. Content reassigned to unbiased reviewer
5. If non-disclosure discovered: investigation, potential termination
6. Systemic pattern of conflict disclosures triggers HR review

### 13.3 Confidentiality

All human-reviewed content is confidential:

| Rule | Detail | Penalty for Breach |
|---|---|---|
| No content may be shared externally | All reviews are within platform ecosystem only | Immediate termination |
| No screenshots of dashboard or interface | Review process is proprietary | Written warning (first), termination (second) |
| No discussion of specific reviews on social media | Even without identifying details | Investigation, potential termination |
| Content identities (user who asked question) are protected | PII of users must not be exposed | Termination, legal liability |
| Review decisions are confidential during review period | Until content published or rejected | Written warning |
| Agent performance data is internal | Accuracy by agent is proprietary | Investigation |

**Data Access Controls**:
- Reviewers can only see content they are assigned to review (no random browsing)
- Dashboard access requires 2FA (TOTP app + password)
- All access logged (audit trail: who, when, what, why)
- Content access revoked when reviewer leaves shift
- Historical access requires manager approval and reason

### 13.4 Escalation of Ethical Concerns

**Ethical Concern Categories**:

| Category | Examples | Escalation Path |
|---|---|---|
| Reviewer misconduct | Bias, discrimination, harassment | L2 reviewer's manager -> Head of Islamic Review -> HR |
| Content ethics violation | Content promoting extremism, hate speech | Safety Officer -> Head of Islamic Review -> Legal -> CE-AI |
| Platform ethics | Data privacy breach, financial misconduct | Legal Advisor -> Head of Content -> CE-AI -> Board |
| Systemic bias | Pattern of anti-madhhab approach across multiple reviewers | Head of Islamic Review -> External Advisory Board |
| External pressure | Government or organizational pressure to approve content | Legal Advisor -> Head of Islamic Review -> CE-AI |
| AI ethics concerns | Agent behavior seen as unethical (deception, manipulation) | Head of Islamic Review -> AI Ethics Team -> CE-AI |

**Escalation Process**:

```
Ethical Concern Identified
  |
  +-- Immediate: Notify immediate supervisor (within 1 hour)
  |
  +-- Supervisor evaluates (within 4 hours)
  |     |
  |     +-- Can resolve at this level -> Document and close
  |     +-- Requires escalation -> Send to Head of Islamic Review
  |
  +-- Head of Islamic Review (within 24 hours)
  |     |
  |     +-- Can resolve -> Document and close
  |     +-- Requires escalation -> Chief Ethics Officer (if exists), CE-AI
  |
  +-- CE-AI (within 48 hours)
        |
        +-- Unilateral decision or Board involvement
        +-- Decision documented, action taken, all parties notified
```

**Whistleblower Protection**:
- Any reviewer reporting an ethical concern in good faith is protected from retaliation
- Anonymous reporting channel available (via encrypted form)
- Retaliation against whistleblower is grounds for immediate termination
- Retaliation complaints investigated by external third party

### 13.5 Compliance with Local Islamic Authorities

**Regulatory Compliance Matrix**:

| Jurisdiction | Regulatory Body | Requirements | Compliance Action | Responsible Party |
|---|---|---|---|---|
| Saudi Arabia | Ministry of Islamic Affairs | All Islamic content must not contradict official Saudi curriculum | Content submitted for pre-approval if > 10k users in SA | Legal + Head of Islamic Review |
| UAE | General Authority of Islamic Affairs | Content reviewed by authority if covering specified sensitive topics | Quarterly content audit submission | Compliance Officer |
| Malaysia | Department of Islamic Development (JAKIM) | Content must not contradict state fatwas, must be in Malay accessible | Malay-language content pre-audit by JAKIM | Legal + Content Team |
| Indonesia | MUI (Majelis Ulama Indonesia) | Content must comply with MUI fatwas, especially for mass distribution | Monthly compliance report, MUI liaison | Head of Islamic Review + Fatwa Liaison |
| Egypt | Al-Azhar Al-Sharif | Quranic content must follow official Mushaf al-Azhar; hadith per Al-Azhar's approved collections | Quranic content auto-checked against Mushaf al-Azhar standard | Quranic Check Agent + Al-Azhar liaison |
| UK | Charity Commission | Islamic content must not be used for political campaigning or fundraising | Screened by Legal Advisor | Legal Advisor |
| EU | National data authorities | GDPR compliance: data collection, consent, right to erasure | User data handling procedures, cookie consent | Data Protection Specialist |
| USA | FTC, DHS | No false advertising (educational claims), no financing of terrorism | Content fee transparency, anti-extremism screening | Legal Advisor + Safety |

**Regulatory Audit Schedule**:

| Audit Type | Frequency | Scope | Responsible |
|---|---|---|---|
| Internal compliance audit | Monthly | All content in top 5 jurisdictions by volume | Compliance Officer + Legal |
| External regulatory submission | Quarterly | Saudi, UAE, Malaysia, Indonesia | Regulatory Affairs Team |
| Al-Azhar content audit | Bi-annual | All Quranic content, tafseer, aqidah | Al-Azhar liaison |
| Privacy compliance audit | Quarterly | GDPR, PDPL, CCPA requirements | Data Protection Specialist |
| Safety compliance review | Monthly | Content flagged for extremism, hate, sectarianism | Lead Safety Officer |
| Full-scale external audit | Annually | All jurisdictions, all policies | External audit firm |

**Non-Compliance Consequences**:

| Severity | Consequence | Example | Remediation |
|---|---|---|---|
| Minor | Content blocked in specific jurisdiction | Saudi-disapproved content removed | Content edited, re-submitted within 24 hours |
| Moderate | Regulatory fine (up to $50k) | GDPR violation | Fine paid, processes updated, staff retrained |
| Major | Platform suspended in jurisdiction | Malaysia blocks platform | Cease operations in jurisdiction, legal appeal, compliance overhaul |
| Critical | Legal prosecution | Content inciting hatred | Immediate termination of related personnel, cooperate with authorities, platform redesign |

---

*End of Document 16 — Human Review System*