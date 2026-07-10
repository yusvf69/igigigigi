# Religious Verification Bible

> Enterprise Islamic Content Verification Architecture
> Version 1.0
> Chief Knowledge Factory Architect / Senior Islamic Content Verification Officer
> Created: 2026-07-10

---

## Table of Contents

1. Religious Verification Philosophy
2. Quran Validation
3. Hadith Validation
4. Hadith Grading System
5. Chain Verification (Isnad)
6. Scholar Opinions
7. Multiple Opinions
8. Citation Rules
9. Reference Matching
10. Source Hierarchy
11. Conflict Resolution
12. Hallucination Prevention
13. Human Review Workflow
14. Confidence Score
15. Evidence Score
16. Authenticity Score
17. Knowledge Graph Integration
18. Verification Agent Specifications
19. Integration with Knowledge Factory Pipeline
20. Appendix: Reference Tables

---

## 1. Religious Verification Philosophy

### 1.1 Core Principle

Islamic content requires the highest level of verification because it pertains to the religion of Allah — the ultimate truth. Errors in religious content are not mere factual mistakes; they carry ethical and spiritual consequences. The Religious Verification Bible establishes the complete verification framework for all Islamic content within the Knowledge Factory.

### 1.2 The Nature of Islamic Revealed Knowledge

Islamic knowledge is fundamentally different from secular knowledge in its epistemological structure. The Quran is the literal word of Allah — never altered, never paraphrased, never subject to editorial revision. Hadith are the prophetic traditions that explain and complement the Quran. Scholarly opinions (ijtihadat) are human attempts to derive rulings from the primary sources. These three categories have fundamentally different verification requirements.

**Quran:** The verbatim word of Allah, transmitted via tawatur (mass transmission) with perfect preservation. Any Quranic content in the platform must be letter-for-letter identical to the verified mushaf. No paraphrase, no transliteration errors, no formatting that distracts from the sacred text.

**Hadith:** The sayings, actions, and approvals of Prophet Muhammad (peace be upon him). Hadith vary in authenticity based on their chain of narration (isnad) and textual content (matn). Every hadith presented must include its grading, source collection, and chain information.

**Scholarly Opinions:** The interpretations and legal rulings derived by qualified scholars. Multiple valid opinions may exist on a single issue. The platform respects all four Sunni madhhabs (Hanafi, Maliki, Shafi'i, Hanbali) and provides full context for differing views.

### 1.3 Verification Hierarchy

The verification approach follows a strict hierarchy based on the epistemological certainty of the source:

1. **Certain Knowledge (Qati):** Quran and mutawatir hadith — absolutely certain, no room for doubt
2. **Probable Knowledge (Dhanni):** Sahih hadith, scholarly consensus (ijma) — highly reliable, requires proper attribution
3. **Acceptable Knowledge (Maqbul):** Hasan hadith, strong scholarly opinions — acceptable for practice with proper context
4. **Weak Knowledge (Daif):** Weak hadith (used only for virtues), minority scholarly opinions — clearly labeled with warnings

### 1.4 Verification Principles

**Principle 1 — Absolute Verbatim Accuracy:** The Quran is never paraphrased. Every ayah presented must match the Uthmani mushaf character for character.

**Principle 2 — Attribution:** Every religious claim is attributed to its source. No anonymous hadith, no unattributed scholarly opinions, no vague references.

**Principle 3 — Multi-Perspective Presentation:** When valid scholarly disagreement exists, all major positions are presented with their evidence. The platform does not favor one madhhab over another.

**Principle 4 — Honest Grading:** Hadith are graded according to established hadith terminology (mustalah al-hadith). Grades are attributed to the scholar who performed the grading.

**Principle 5 — Context Provision:** Every religious text is presented with its context — asbab al-nuzul (circumstances of revelation) for Quran, asbab al-wurud (circumstances of narration) for hadith.

**Principle 6 — Humility Before Uncertainty:** When a religious claim cannot be verified with high confidence, the platform acknowledges uncertainty rather than fabricating certainty.

**Principle 7 — Timeless Sanctity:** Quranic content is NEVER versioned. It is the unchanging word of Allah. Hadith content is versioned only to track different narration variants, not to suggest change in the prophetic text.

### 1.5 Epistemological Framework

The Islamic verification framework recognizes multiple levels of certainty:

| Level | Category | Certainty | Presentation Rule |
|-------|----------|-----------|-------------------|
| Qati | Quran, Mutawatir | 100% | Presented as absolute fact |
| Dhanni | Sahih, Ijma | 80-99% | Presented as established |
| Maqbul | Hasan, Qiyas | 60-79% | Presented with context |
| Mardud | Daif, Weak Opinions | Below 60% | Flagged, limited use |

### 1.6 Scope of This Bible

This document governs all religious content verification within the Knowledge Factory. It applies to:

- All AI-generated Islamic content
- All human-reviewed Islamic content
- All imported or referenced Islamic content
- All learning modes involving Islamic topics
- All assessment and quiz content on Islamic topics
- All audio and video content related to Islam

### 1.7 Connected Documents

This Bible connects to:
- `Knowledge_Factory/00_Knowledge_Factory_Bible` — Parent architecture for the entire Knowledge Factory
- `Documentation/09_Verification` — General verification frameworks and protocols
- `AI_Company/11_Workflows` — Enterprise workflow definitions for human-in-the-loop review

### 1.8 Non-Religious Content

Content that is not explicitly religious (Islamic history without legal rulings, Arabic language lessons, cultural content) follows the general verification frameworks in `Documentation/09_Verification`. However, if such content cites religious sources, the religious citation rules in this Bible apply to those citations.

---

## 2. Quran Validation

### 2.1 Core Principle

The Quran is the literal, uncreated word of Allah, revealed to Prophet Muhammad (peace be upon him) through the Angel Jibril, transmitted via tawatur (mass uninterrupted transmission), and preserved in the mus'haf (written codex). Any Quranic content on the platform must be verified against an approved digital mushaf.

### 2.2 Approved Digital Mushaf

The platform uses the following approved digital mushaf as the authoritative reference:

| Standard | Specification | Source |
|----------|--------------|--------|
| Uthmani Script | Madinah Mushaf (King Fahd Complex for the Printing of the Holy Quran) | Official digital text |
| Text Encoding | Unicode Arabic, with diacritical marks (tashkeel), pause marks (waqf), and sajdah markers | UTF-8 |
| Verification | Cross-referenced against printed Madinah Mushaf | Physical copy |
| Version | Static — never updated | Permanent |

### 2.3 Verse Matching Protocol

All Quranic references must pass through the following verification steps:

**Step 1 — Surah Identification:**
- Surah name is matched against the official 114 surah names
- Alternative names are mapped to official names (e.g., "Al-Fatihah" is "Surah 1", "Al-Ikhlas" is "Surah 112")
- Surah number is verified (1-114)
- Confirmation that the surah is Meccan or Medinan is optional context

**Step 2 — Ayah Number Verification:**
- Ayah numbers per surah are verified against the Kufic count (standard for the Madinah Mushaf)
- Some surahs have differing ayah counts in different qiraat — the Kufic count is used by default
- Alternative ayah counts (Basran, Syrian, etc.) are noted when relevant

**Step 3 — Text Matching:**
- The full Arabic text of each ayah must match the approved mushaf character for character
- No characters may be added, removed, altered, or transposed
- Diacritical marks (fatha, kasra, damma, sukun, shadda, madd) must match
- Pause marks (waqf marks) are preserved when quoting with pause notation
- Prostration markers (sajdah symbols) are included when relevant

**Step 4 — Translation Verification:**
- Translations are verified against approved translations
- Each translation is attributed to its translator
- Multiple translations may be presented for clarity
- Translations are clearly marked as "Translation" and never presented as Quran

### 2.4 Recitation (Qiraat) Variants

The Quran was revealed in seven ahruf (modes), and the canonical qiraat (recitation styles) preserve these variations:

| Qiraah | Transmitter | Region | Recognition |
|--------|-------------|--------|-------------|
| Nafi' | Warsh, Qalun | Madinah | Canonical |
| Ibn Kathir | Al-Bazzi, Qunbul | Makkah | Canonical |
| Abu Amr | Al-Duri, Al-Susi | Basra | Canonical |
| Ibn Amir | Hisham, Ibn Dhakwan | Damascus | Canonical |
| Asim | Hafs, Shu'bah | Kufa | Canonical |
| Hamzah | Khalaf, Khallad | Kufa | Canonical |
| Al-Kisai | Al-Duri, Abu Harith | Kufa | Canonical |

**Default:** The platform uses Hafs 'an Asim by default (most widespread).
**Variant Display:** When a qiraah variant affects meaning or recitation, the variant is displayed with the qiraah identified.
**Verification:** Each variant is verified against the transmitted text of that qiraah.

### 2.5 Tajweed Rule Verification

When the platform produces Quranic recitation content (audio or text with tajweed notation):

**Tajweed Rules to Verify:**
1. **Madd (elongation):** Mandatory (madd wajib), permissible (madd jaiz), natural (madd tabi'i) — each with correct duration
2. **Ghunnah (nasalization):** Correct application on noon and meem mushaddadah
3. **Idgham (merging):** With and without ghunnah, correct merging of letters
4. **Iqlab (conversion):** Noon saakinah before ba
5. **Izhar (clarification):** Noon saakinah before throat letters
6. **Ikhfa (concealment):** Noon saakinah before remaining letters
7. **Qalqalah (echoing):** Correct application on qaf, ta, ba, jim, dal
8. **Lam and Ra rules:** Tafkhim (heavy) and tarqiq (light)

**Verification Source:** Tajweed rules are verified against the standard works of Ibn al-Jazari and contemporary tajweed authorities.

### 2.6 Tafsir References

All tafsir (Quranic exegesis) content on the platform:

**Approved Tafsir Sources:**
- Tafsir al-Tabari (Jami' al-Bayan) — Ibn Jarir al-Tabari (d. 310 AH) — the mother of all tafasir
- Tafsir al-Qurtubi (Al-Jami' li-Ahkam al-Quran) — Methodology: fiqh-oriented
- Tafsir Ibn Kathir (Tafsir al-Quran al-Azim) — Methodology: athar-based
- Tafsir al-Sa'di (Taysir al-Karim al-Rahman) — Methodology: contemporary accessible
- Tafsir al-Baghawi (Ma'alim al-Tanzil) — Methodology: traditional athar-based
- Tafsir al-Razi (Mafatih al-Ghayb) — Methodology: rationalist/theological
- Tafsir al-Shawkani (Fath al-Qadir) — Methodology: comprehensive
- Tafsir al-Muyassar — Methodology: simplified modern

**Citation Format:** (Tafsir:ScholarName:Volume:Page)

**Attribution Rules:**
- Every tafsir opinion is attributed to the scholar
- Multiple tafsir opinions are presented when they differ
- The scholar's methodology is briefly noted when relevant
- Tafsir is clearly distinguished from the Quranic text itself
- Tafsir content is versioned (scholarly interpretations are human knowledge, not revelation)

### 2.7 Presentation Rules

**Text Presentation:**
- Quranic text is displayed in its own distinct formatting (larger font, different styling)
- No distractions (ads, links, images) adjacent to Quranic text
- Bismillah is included at the start of each surah (except Surah al-Tawbah)
- Ayah numbers are clearly displayed
- Proper line breaks respect ayah boundaries

**Audio Presentation:**
- Recitations are from qualified reciters with verified ijazah (license)
- Audio quality meets minimum standards (44.1 kHz, 128 kbps minimum)
- Each audio file is tagged with reciter name, qiraah, surah, and ayah range
- No background music accompanies Quranic recitation
- No interruptions during recitation

**Digital Respect:**
- Quranic text is not displayed in impure contexts
- Quranic verses are not used for decorative purposes
- Audio recitation is not used as background audio
- The platform shows appropriate respect in handling and displaying the sacred text

### 2.8 Timelessness Principle

Quranic content is NEVER versioned. Unlike all other content in the Knowledge Factory:
- Quranic text has no version number
- Quranic text has no status flags (draft, review, published)
- Quranic text is never modified except to correct an error in the stored text
- If a correction occurs, it is treated as a bug fix, not an update
- Tafsir content IS versioned (it is scholarly interpretation, not revelation)

### 2.9 Quran-Related Error Handling

| Error Type | Detection | Resolution |
|-----------|-----------|------------|
| Text mismatch | Automated comparison with mushaf | Block content until fixed |
| Wrong ayah number | Cross-reference check | Auto-correct |
| Missing diacritical | Unicode validation | Reject and regenerate |
| Translation error | Human review | Flag for revision |
| Fabricated verse | AI hallucination detection | Block permanently, log incident |

---

## 3. Hadith Validation

### 3.1 Core Principle

Hadith are the sayings, actions, tacit approvals, and descriptions of Prophet Muhammad (peace be upon him). Unlike the Quran, hadith vary in authenticity and require meticulous verification through chain of narration analysis and textual criticism.

### 3.2 Approved Hadith Collections

The platform recognizes the following hadith collections as primary sources. Each collection has a unique identifier used in all references:

| ID | Collection Name | Author | Death (AH) | Total Hadith | Status |
|----|----------------|--------|------------|-------------|--------|
| BUK | Sahih al-Bukhari | Muhammad ibn Ismail al-Bukhari | 256 | 7,275 (incl. repeats) | Most authentic after Quran |
| MUS | Sahih Muslim | Muslim ibn al-Hajjaj al-Naysaburi | 261 | 9,200 (incl. repeats) | Second most authentic |
| ABU | Sunan Abi Dawud | Sulayman ibn al-Ash'ath al-Sijistani | 275 | 5,274 | One of six major collections |
| TIR | Jami' al-Tirmidhi | Muhammad ibn Isa al-Tirmidhi | 279 | 3,956 | Includes gradings |
| NAS | Sunan al-Nasai | Ahmad ibn Shu'ayb al-Nasai | 303 | 5,761 | Strong verification standards |
| IJM | Sunan Ibn Majah | Muhammad ibn Yazid al-Qazwini | 273 | 4,341 | Included in six major books |
| AHM | Musnad Ahmad | Ahmad ibn Hanbal | 241 | 27,000+ | Largest early collection |
| MAL | Muwatta Malik | Malik ibn Anas | 179 | 1,720 | Earliest comprehensive work |
| DAR | Sunan al-Darimi | Abdullah ibn Abd al-Rahman al-Darimi | 255 | 3,500 | Well-regarded |
| BAY | Sunan al-Bayhaqi | Ahmad ibn al-Husayn al-Bayhaqi | 458 | 20,000+ | Comprehensive |
| HAK | Al-Mustadrak ala al-Sahihayn | Al-Hakim al-Naysaburi | 405 | 8,800 | Contains authentic and weak |
| TAH | Sharh Ma'ani al-Athar | Al-Tahawi | 321 | 5,000+ | Fiqh-oriented |
| TAB | Al-Mu'jam al-Kabir | Al-Tabarani | 360 | 25,000+ | Comprehensive compendium |

### 3.3 Hadith Classification Categories

**By Authenticity:**
- **Sahih (Authentic):** A hadith with a continuous chain of reliable narrators, free from irregularity (shudhudh) and hidden defect (illah)
- **Hasan (Good):** Slightly lower reliability than sahih, but still acceptable
- **Daif (Weak):** Fails one or more conditions of acceptance
- **Maudu (Fabricated):** Invented, falsely attributed to the Prophet

**By Chain:**
- **Mutawatir:** Mass-transmitted at every level of the chain — yields certain knowledge
- **Ahad:** Single or limited transmission — yields probable knowledge
- **Aziz:** At least two narrators at every level
- **Gharib:** A single narrator at some level
- **Mashhur:** Three or more narrators but not reaching tawatur

**By Chain Continuity:**
- **Muttasil:** Continuous chain, each narrator heard from the previous
- **Munqati:** Chain with a missing narrator at any non-companion level
- **Mursal:** Companion omitted (a Successor narrates directly from the Prophet)
- **Mu'allaq:** The beginning of the chain is missing (one or more narrators omitted)
- **Mu'dal:** Two or more consecutive narrators missing

### 3.4 Hadith Verification Protocol

**Step 1 — Source Identification:**
- Identify the hadith collection, book (kitab), chapter (bab), and hadith number
- Cross-reference the hadith number across different editions and digital databases
- Note: Hadith numbering can vary between editions (especially between printed editions and digital databases)

**Step 2 — Text Verification:**
- Compare the Arabic text against the verified digital text of the collection
- Check for textual variants (riwayat) in different transmissions
- Document all wording differences between narrations
- Verify that the matn (text) is consistent with known authentic versions

**Step 3 — Chain Verification:**
- Extract the full isnad (chain of narration)
- Identify each narrator by name, kunya, and nisba
- Verify narrator reliability through jarh wa ta'dil sources
- Check chain continuity (each narrator must have received from the previous)
- Reference the chain against known isnad databases

**Step 4 — Grading Verification:**
- Record the grading given by the original compiler (if available, e.g., Tirmidhi's gradings)
- Record gradings from major hadith scholars (Ibn Hajar, al-Dhahabi, al-Albani, etc.)
- Note any disagreement among scholars regarding the grading
- Ascribe each grading to the specific scholar

**Step 5 — Context Documentation:**
- Document asbab al-wurud (circumstances of the saying)
- Identify whether the hadith is general or specific ('amm vs khas)
- Check for abrogation (naskh) — whether another hadith or Quran verse supersedes this one
- Document scholarly disagreement on interpretation or applicability

### 3.5 Multiple Narration Variants

A single hadith may have multiple wordings across different collections:

**Handling Protocol:**
1. All known major variants are documented
2. Each variant is attributed to its source collection and book
3. The primary wording (usually from Bukhari or Muslim) is presented as the main text
4. Variants are presented as footnotes or alternates
5. Meaning differences between variants are explained when significant

**Example:**
Main Text: "Actions are by intentions..." (Bukhari)
Variant: "Actions are by intention..." (Muslim, slight wording difference)
Explanation: Both carry the same meaning; the difference is in Arabic phrasing.

### 3.6 Context of Narration (Asbab al-Wurud)

Understanding the context of a hadith is essential for correct interpretation:

**Elements to Document:**
1. **Historical Context:** What was happening when the Prophet said this?
2. **Occasion:** What question or situation prompted the statement?
3. **Recipient:** Was it addressed to a specific person, a group, or all Muslims?
4. **Temporal Context:** Was it in Makkah or Madinah? Early or late in revelation?
5. **Special Circumstances:** Was it a response to a specific event?

**Sources for Context:**
- Sharh al-Nawawi 'ala Muslim (commentary on Muslim)
- Fath al-Bari by Ibn Hajar (commentary on Bukhari)
- Specialized works on asbab al-wurud (e.g., al-Suyuti's "Asbab al-Wurud")

### 3.7 Abrogation (Naskh) in Hadith

When two hadith appear to conflict and cannot be reconciled through other means:

**Protocol:**
1. Attempt reconciliation (al-jam') first — most apparent conflicts can be resolved
2. Check historical dating to determine which came later
3. Document the abrogating (nasikh) and abrogated (mansukh) hadith
4. Cite scholarly sources that confirm the naskh
5. Note if scholars disagree on whether naskh occurred

### 3.8 Scholarly Disagreement on Grading

When scholars disagree on a hadith's grade:

**Presentation Protocol:**
1. Present the majority opinion first (with attribution)
2. Present minority opinions (with attribution)
3. Explain the basis for each grading
4. Provide context for why scholars differed
5. Let the user see the full picture rather than hiding disagreement

---

## 4. Hadith Grading System

### 4.1 Platform Grading Scale

The platform uses a 10-level numerical grading system for hadith, mapped to traditional hadith terminology. This system enables precise expression of authenticity levels for AI processing.

| Score | Term | Definition | Usage |
|-------|------|-----------|-------|
| 10 | Sahih (Agreed Upon) | Narrated by both Bukhari and Muslim | Rulings, beliefs |
| 9 | Sahih (Bukhari) | Narrated by Bukhari alone | Rulings, beliefs |
| 8 | Sahih (Muslim) | Narrated by Muslim alone | Rulings, beliefs |
| 7 | Sahih (Others) | Graded sahih by other recognized scholars | Rulings, beliefs |
| 6 | Hasan Sahih | Graded hasan sahih by Tirmidhi or others | Rulings, virtues |
| 5 | Hasan | Good, acceptable chain and text | Virtues, some rulings |
| 4 | Hasan li-Ghayrihi | Hasan due to supporting chains | Virtues, encouragement |
| 3 | Daif (Mild) | Weak with mild weakness | Virtues only |
| 2 | Daif (Significant) | Significant weakness in chain or text | Not for rulings |
| 1 | Daif Jiddan | Very weak — serious issues | Not used except in rare cases |
| 0 | Maudu | Fabricated — falsely attributed | NEVER used except to warn |

### 4.2 Scholar-Based Grading

Each grade on the platform is attributed to the scholar who performed the grading:

**Primary Grading Scholars:**
- **Al-Bukhari (d. 256 AH):** Strictest conditions, highest standard
- **Muslim (d. 261 AH):** Slightly less strict than Bukhari on chain continuity
- **Al-Tirmidhi (d. 279 AH):** Explicit gradings in his Jami', uses hasan sahih terminology
- **Ibn Hajar al-Asqalani (d. 852 AH):** Master of hadith criticism, Fath al-Bari
- **Al-Dhahabi (d. 748 AH):** Critical historian of hadith narrators
- **Al-Albani (d. 1420 AH / 1999 CE):** Contemporary hadith critic, re-evaluated many hadith
- **Ibn Hibban (d. 354 AH):** Known for strict grading standards
- **Al-Hakim (d. 405 AH):** Known for lenient grading in al-Mustadrak
- **Ibn Khuzaymah (d. 311 AH):** Extremely strict conditions
- **Al-Daraqutni (d. 385 AH):** Critical scholar, expert in subtle defects

### 4.3 Grading Sub-Categories

**Sahih Sub-Grades:**
- **Sahih li-Dhatihi (صحيح لذاته):** Sahih in itself — meets all conditions independently
- **Sahih li-Ghayrihi (صحيح لغيره):** Sahih due to supporting evidence — raised from hasan by corroboration

**Hasan Sub-Grades:**
- **Hasan li-Dhatihi (حسن لذاته):** Hasan in itself — acceptable chain with mild weakness
- **Hasan li-Ghayrihi (حسن لغيره):** Hasan due to supporting evidence — raised from daif by corroboration

**Daif Sub-Grades:**
- **Daif al-Isnad (ضعيف الإسناد):** Weak in chain but text may be acceptable
- **Daif al-Matn (ضعيف المتن):** Weak in text (more serious)
- **Daif Jiddan (ضعيف جدا):** Very weak — serious narrator issues or chain breaks
- **Munkar (منكر):** Rejected — narrator contradicts those more reliable
- **Maudu (موضوع):** Fabricated — knowingly or unknowingly invented

### 4.4 Grading Basis Documentation

For every graded hadith, the following is documented:

1. **Grading Scholar(s):** Who graded it and at what level
2. **Chain Assessment:** Strength of the isnad, narrator by narrator
3. **Textual Assessment:** Any issues in the matn (text)
4. **Supporting Evidence:** Other narrations that support or weaken this hadith
5. **Scholar Rationale:** Why the scholar gave this grade
6. **Alternative Opinions:** Differing grades from other scholars

### 4.5 Handling Disputed Gradings

When scholars disagree on a hadith's grade:

**Example Case:**
- Hadith X is graded Sahih by Al-Albani
- Hadith X is graded Daif by Al-Arnaut
- Hadith X is graded Hasan by Shu'ayb al-Arna'ut

**Platform Handling:**
- Present all three gradings with attribution
- Explain the basis for each scholar's grading
- Note that Al-Albani is known for somewhat lenient sahih grading
- Note that Shu'ayb al-Arna'ut is known for precise, balanced grading
- Let the user see the complete picture

### 4.6 Grade Usage Rules

| Grade | Can Be Used For Rulings | Can Be Used For Virtues | Can Be Used For Beliefs |
|-------|------------------------|------------------------|------------------------|
| 10-7 | Yes | Yes | Yes |
| 6 | Yes (with caution) | Yes | Yes |
| 5 | Yes (fiqh) | Yes | Yes |
| 4 | Preferred not | Yes | Not alone |
| 3 | No | Yes (virtues & encouragement) | No |
| 2 | No | Preferred not | No |
| 1 | No | No | No |
| 0 | No | No | No |

---

## 5. Chain Verification (Isnad)

### 5.1 Core Principle

The chain of narration (isnad) is the backbone of hadith verification. A hadith is only as strong as its narrators and the continuity of its transmission. The platform maintains a comprehensive narrator database and verification system.

### 5.2 Narrator Identification

Every narrator in a chain must be identified with:

**Identification Fields:**
- **Ism (Name):** Full given name
- **Kunya (Patronymic):** Abu so-and-so, Umm so-and-so
- **Nisba (Attribution):** Tribal, regional, professional attribution
- **Laqab (Honorific):** Descriptive title (e.g., al-A'raj, al-A'mash)
- **Lifespan:** Birth and death years (AH or CE)
- **Generation (Tabaqah):** Which generation of narrators they belong to
- **Teachers:** Key scholars they learned from
- **Students:** Key scholars who narrated from them

### 5.3 Narrator Reliability Classification

| Classification | Arabic Term | Meaning | Usage in Grading |
|---------------|-------------|---------|------------------|
| Trustworthy | Thiqah (ثقة) | Highly reliable memory and character | Supports sahih grading |
| Truthful | Saduq (صدوق) | Generally reliable, minor issues | Supports hasan grading |
| Acceptable | Maqbul (مقبول) | Acceptable when supported | May support hasan |
| Weak | Daif (ضعيف) | Questionable memory or character | Weakens hadith |
| Unknown | Majhul (مجهول) | Not sufficiently known | Suspends judgment |
| Abandoned | Matruk (متروك) | Accused of lying or extremely weak | Strongly weakens |
| Fabricator | Kadhdhab (كذاب) | Known to fabricate hadith | Invalidates chain |

### 5.4 Jarh wa Ta'dil Sources

The platform references the following major biographical evaluation works:

| Work | Author | Focus |
|------|--------|-------|
| Al-Jarh wa al-Ta'dil | Ibn Abi Hatim al-Razi | Comprehensive evaluations |
| Al-Thiqat | Ibn Hibban | Trustworthy narrators only |
| Al-Du'afa | Al-Dhahabi | Weak narrators |
| Tahdhib al-Tahdhib | Ibn Hajar al-Asqalani | Comprehensive biographical encyclopedia |
| Taqrib al-Tahdhib | Ibn Hajar al-Asqalani | Concise summaries of narrators |
| Al-Kamil fi Du'afa al-Rijal | Ibn Adi | Weak narrators with evidence |
| Al-Tarikh al-Kabir | Al-Bukhari | Early biographical work |
| Mizan al-I'tidal | Al-Dhahabi | Balanced critical evaluations |
| Lisan al-Mizan | Ibn Hajar | Narrators not in Tahdhib |

### 5.5 Chain Continuity Verification

**Conditions for Chain Continuity:**
1. Each narrator must have been alive at the time of transmission
2. Each narrator must have met the previous narrator (for musalsal bil-mulaqat)
3. The possibility of hearing must exist (mu'asarah — contemporaneity is sufficient for Bukhari and Muslim if meeting was possible)
4. No missing links in the chain

**Chain Types by Continuity:**
- **Muttasil (متصل):** Continuous — every narrator heard from the previous
- **Munqati (منقطع):** Broken — a narrator is missing (non-companion level)
- **Mursal (مرسل):** A Successor narrates directly from the Prophet (companion omitted)
- **Mu'allaq (معلق):** The beginning of the chain is missing (usually the compiler omits one or more top narrators)
- **Mu'dal (معضل):** Two or more consecutive narrators missing

### 5.6 Chain Strength Classification

| Strength | Definition | Authentication Level |
|----------|-----------|---------------------|
| A'la (Very Strong) | All narrators thiqah, chain continuous, no defects | Sahih |
| Qawi (Strong) | Mostly thiqah narrators, minor issues | Sahih/Hasan |
| Hasan (Good) | Acceptable narrators, some weakness | Hasan |
| Daif (Weak) | Notable weakness in chain | Daif |
| Wahi (Very Weak) | Serious chain issues | Daif Jiddan |

### 5.7 Chain Verification Against Databases

The platform verifies chains against:

1. **Local Narrator Database:** Pre-loaded with data from major biographical works
2. **Hadith Corpus Database:** Pre-loaded with verified texts and chains from the approved collections
3. **Isnad Pattern Recognition:** Known chain patterns are recognized and matched
4. **Cross-Reference Engine:** Chains are compared across multiple collections to identify variants

**Automated Chain Verification Steps:**
1. Parse the chain into individual narrator nodes
2. Match each node against the narrator database
3. Verify each narrator's reliability classification
4. Check continuity between consecutive narrators
5. Identify known chain patterns (e.g., the "golden chain" of Malik → Nafi → Ibn Umar)
6. Calculate chain strength score
7. Flag any anomalies for human review

### 5.8 Common Chain Patterns

| Pattern | Description | Typical Strength |
|---------|-------------|-----------------|
| A'la al-Isnad | Shortest chain (fewest narrators) | Generally stronger |
| Silsilat al-Dhahab | "Golden chain" — Malik → Nafi → Ibn Umar | Strongest possible |
| Nuzul al-Isnad | Longer chain (more narrators) | Generally weaker |
| Affected by Tadlis | Narrator uses ambiguous transmission terms | Weaker |
| Musalsal | Chain with a recurring feature | Variable |

### 5.9 Tadlis (Concealment) Detection

Tadlis occurs when a narrator uses ambiguous terms to conceal who they heard from, creating false impressions of chain continuity:

**Types of Tadlis:**
- **Tadlis al-Isnad:** Narrator says "from so-and-so" without confirming hearing directly
- **Tadlis al-Shuyukh:** Narrator refers to a teacher by a name that doesn't identify them clearly

**Detection:**
- Known mudallis (those who practice tadlis) are flagged in the narrator database
- Narrations from mudallis using ambiguous terms (عن, أن, قال) are treated as potentially broken
- Only explicit hearing terms (سمعت, حدثنا, أخبرنا) are accepted from known mudallis

---

## 6. Scholar Opinions

### 6.1 Core Principle

The platform respects all four Sunni madhhabs (Hanafi, Maliki, Shafi'i, Hanbali) and presents scholarly opinions from each school with full attribution. No madhhab is presented as "the correct one" to the exclusion of others. All qualified scholarly opinions are treated with respect.

### 6.2 The Four Madhhabs

| Madhhab | Founder | Death (AH) | Geographic Spread | Methodology |
|---------|---------|------------|-------------------|-------------|
| Hanafi | Abu Hanifah al-Nu'man | 150 | South Asia, Turkey, Balkans, Central Asia | Ra'y (reason), qiyas extensive |
| Maliki | Malik ibn Anas | 179 | North Africa, West Africa, parts of Gulf | Amal ahl al-Madinah (Medinan practice) |
| Shafi'i | Muhammad ibn Idris al-Shafi'i | 204 | Egypt, East Africa, Indonesia, Malaysia | Fusion of ra'y and athar, usul al-fiqh founder |
| Hanbali | Ahmad ibn Hanbal | 241 | Saudi Arabia, parts of Gulf | Zahir al-nass (literal texts), minimal ra'y |

### 6.3 Presenting Madhhab Positions

For each fiqh topic, the platform presents:

**Standard Format:**
- **Topic:** Clear statement of the issue
- **Hanafi Position:** Ruling + evidence (dala'il) + strength of evidence
- **Maliki Position:** Ruling + evidence (dala'il) + strength of evidence
- **Shafi'i Position:** Ruling + evidence (dala'il) + strength of evidence
- **Hanbali Position:** Ruling + evidence (dala'il) + strength of evidence
- **Preferred Position:** If one position has significantly stronger evidence, it is noted (but not enforced)
- **Summary:** Balanced overview of all positions

### 6.4 Evidence Classification for Opinions

| Evidence Type | Strength | Description |
|--------------|----------|-------------|
| Quran (clear text) | Highest | Text is unambiguous and directly applicable |
| Quran (interpreted) | High | Text requires interpretation |
| Mutawatir Hadith | Very High | Mass-transmitted, certain |
| Sahih Hadith (clear) | High | Clear meaning, authentic chain |
| Sahih Hadith (interpreted) | Medium-High | Authentic but open to interpretation |
| Ijma (Consensus) | Very High | Scholarly agreement on a ruling |
| Qiyas (Analogy) | Medium | Analogical reasoning from established rulings |
| Istihsan (Preference) | Medium | Equitable preference, Hanafi methodology |
| Maslahah Mursalah (Public Interest) | Medium | Unrestricted public interest, Maliki methodology |
| Urf (Custom) | Low-Medium | Local custom as a source |
| Sadd al-Dharai (Blocking Means) | Low-Medium | Preventing harm by blocking its means |

### 6.5 Contemporary Scholars

The platform includes qualified contemporary scholarship when relevant:

**Criteria for Contemporary Scholar Inclusion:**
1. Proper Islamic education (degree from recognized institution or traditional ijazah)
2. Recognition by peers and established institutions
3. Adherence to sound methodology (usul)
4. No extreme or deviant positions (according to mainstream scholarship)
5. Publications and fatwas that demonstrate scholarship

**Notable Contemporary Scholars Referenced:**
- Abdullah ibn Bayyah (Maliki, Mauritania)
- Yusuf al-Qaradawi (comparative fiqh, Qatar) [deceased 2022]
- Ali Gomaa (Shafi'i, Egypt, former Grand Mufti)
- Salman al-Ouda (Hanbali, Saudi Arabia)
- Muhammad al-Yaqoubi (Shafi'i, Syria)
- Umar Faruq Abd-Allah (Hanafi/Maliki, USA)
- Akram Nadwi (Hanafi, India/UK)
- Suhaib Webb (contemporary, USA)

### 6.6 Scholarly Consensus (Ijma)

**When Consensus Exists:**
- Clearly identified as "The scholars are in consensus (ijma) that..."
- Evidence for the consensus is provided
- Dissenting opinions (if any, even if very weak) are noted
- The source documenting the consensus is cited (e.g., Ibn Qudamah, al-Nawawi, Ibn Rushd)

**When Consensus is Claimed but Disputed:**
- The claim is presented as "Some scholars have claimed consensus on this, but others disagree"
- Both sides of the dispute are presented
- Scholars who disputed the consensus are named

### 6.7 Minority Opinions

**Handling:**
- Minority opinions are presented as such: "A minority of scholars, including [names], hold the view that..."
- The evidence for the minority position is presented honestly
- The reasons the majority disagreed are explained
- Minority opinions are not dismissed or ridiculed
- The user is informed of the weight of scholarly support for each position

### 6.8 Scholarly Respect Protocol

- No scholar is insulted, dismissed, or disrespected
- All scholars are referred to with appropriate honorifics (e.g., "Imam", "Hafiz", "Shaykh")
- When scholars err, it is stated factually: "In this matter, scholar X's position is considered weak because..." rather than "Scholar X was wrong"
- Disagreements between scholars are presented neutrally

---

## 7. Multiple Opinions

### 7.1 Core Principle

When valid scholarly disagreement exists, the platform presents ALL major opinions with their evidence. It never presents one opinion as "the truth" when there is legitimate scholarly disagreement. The AI's role is to present and explain, not to favor.

### 7.2 When to Present Multiple Opinions

| Scenario | Action |
|----------|--------|
| Sahih hadith apparently conflict | Present reconciliation or preference |
| Scholars differ on hadith interpretation | Present all interpretations |
| Madhhabs differ on a ruling | Present all madhhab positions |
| Contemporary scholars disagree | Present differing views |
| Scholars differ on hadith grading | Present all gradings |
| Quranic interpretation differs | Present all valid interpretations |

### 7.3 Multi-Opinion Presentation Format

**Standard Format:**
```
**Issue:** [Clear statement of the question]

**Position 1:** [Ruling/View]
- Scholars/Schools: [Names]
- Evidence: [Dala'il with sources]
- Strength: [Assessment of the evidence]

**Position 2:** [Ruling/View]
- Scholars/Schools: [Names]
- Evidence: [Dala'il with sources]
- Strength: [Assessment of the evidence]

[Additional positions as needed]

**Summary:** [Balanced overview]

**Recommended:** [If one position is clearly stronger — noted as such, but not enforced]
```

### 7.4 Handling Preponderance

When one opinion is clearly stronger in evidence:

- The stronger opinion is identified with its evidence explained
- The weaker opinion is still presented respectfully
- The user is told WHY the stronger opinion is stronger
- The user is free to follow either opinion (in matters of legitimate disagreement)

### 7.5 Prohibition on False Equivalence

Not all opinions are equally valid. The platform does NOT create false equivalence by presenting a fringe opinion alongside established scholarly consensus as if they are equal. The following rules apply:

- **Majority vs. Minority:** Clearly labeled as such
- **Strong vs. Weak Evidence:** Evidence strength is assessed and communicated
- **Accepted vs. Rejected:** If an opinion is rejected by mainstream scholarship, it is identified as such
- **Fringe vs. Mainstream:** Fringe opinions are not presented without context

### 7.6 Neutral AI Stance

The AI presenting religious content does not:
- Favor one madhhab over others
- Express personal preference for scholarly opinions
- Dismiss valid scholarly disagreement
- Claim certainty where none exists
- Present one opinion when multiple valid opinions exist
- Use first-person ("I think") for religious rulings

---

## 8. Citation Rules

### 8.1 Core Principle

Every religious claim on the platform must have a verifiable citation. No anonymous citations, no vague references, no unsourced claims. Citations follow a strict format that enables verification by scholars and automated systems.

### 8.2 Citation Format

**Quran Citation:**
- Format: `(Quran [Surah Name] [Surah Number]:[Ayah Number])`
- Example: `(Quran al-Baqarah 2:255)`
- Optional: qiraah variant: `(Quran al-Fatihah 1:1 — Qiraah: Warsh)`

**Hadith Citation:**
- Format: `(Hadith [Collection Code]:[Book Number]:[Hadith Number])`
- Example: `(Hadith BUK:59:3162)`
- With grading: `(Hadith BUK:59:3162 — Sahih)`
- With scholar grading: `(Hadith BUK:59:3162 — Graded Sahih by Ibn Hajar)`
- Multiple sources: `(Hadith BUK:59:3162, MUS:22:1302)`

**Collection Codes:**
| Code | Collection |
|------|-----------|
| BUK | Sahih al-Bukhari |
| MUS | Sahih Muslim |
| ABU | Sunan Abi Dawud |
| TIR | Jami' al-Tirmidhi |
| NAS | Sunan al-Nasai |
| IJM | Sunan Ibn Majah |
| AHM | Musnad Ahmad |
| MAL | Muwatta Malik |

**Scholarly Opinion Citation:**
- Format: `(Opinion: [Scholar]:[Book Name]:[Volume:Page])`
- Example: `(Opinion: Ibn Qudamah: al-Mughni: 3:215)`
- Alternative: `(Opinion: [Madhhab]: [Scholar]: [Book Name]: [Volume:Page])`

**Ijma Citation:**
- Format: `(Ijma: [Scholar claiming ijma]: [Book Name]: [Volume:Page])`
- Example: `(Ijma: al-Nawawi: al-Majmu': 1:45)`

### 8.3 Citation Hierarchy

When multiple sources support a claim, citations are ordered by authority:

1. Quran
2. Mutawatir Hadith
3. Sahih Hadith (Bukhari and Muslim first, then others)
4. Hasan Hadith
5. Scholarly Consensus (Ijma)
6. Strong Scholarly Opinion
7. Qiyas (Analogical Reasoning)
8. Weak Hadith (if used at all)
9. Weak Scholarly Opinion

### 8.4 Citation Verifiability

All citations must be verifiable:

**Verification Check:**
- Quran: Reference can be looked up in any mushaf
- Hadith: Reference can be looked up in the specified collection
- Scholarly: Reference can be looked up in the specified book and page
- Online databases: If using digital identifiers, the database is specified

**Prohibited Citations:**
- "A hadith says..." without specification
- "Scholars say..." without naming scholars
- "According to the madhhab..." without naming which madhhab or scholar
- "It is well-known that..." without source

### 8.5 Citation in Different Contexts

**Learning Content:**
- Full citations with all details
- Grading information for hadith
- Multiple madhhab opinions with citations

**Quick Answers (AI Chat):**
- Abbreviated citations allowed
- Quran: surah and ayah number minimum
- Hadith: collection and hadith number minimum
- Full citation available on request

**Audio/Video:**
- Verbal attribution: "This is based on the hadith narrated by Bukhari..."
- On-screen text citation for visual content
- Description box citations for platform content

---

## 9. Reference Matching

### 9.1 Core Principle

Automated matching of religious references to known databases enables verification at scale. The platform maintains comprehensive reference databases and matching algorithms.

### 9.2 Quran Reference Matching

**Surah Name Variants:**
| Official Name | Variants |
|--------------|----------|
| Al-Fatihah | Fatihat al-Kitab, Umm al-Quran, al-Sab' al-Mathani |
| Al-Baqarah | — |
| Al-Imran | Al-i-Imran |
| Al-Masad | Tabbat, al-Lahab |
| Al-Fil | — |
| Quraysh | Li-ilafi Quraysh |
| Al-Ma'un | Ara'ayta |
| Al-Kawthar | — |
| Al-Kafirun | — |
| Al-Nasr | Idha Ja'a |
| Al-Ikhlas | Qul Huwa Allahu Ahad |
| Al-Falaq | — |
| Al-Nas | — |

**Matching Algorithm:**
1. Normalize surah name to canonical form
2. Map to surah number (1-114)
3. Verify ayah number is within valid range for that surah
4. Cross-check ayah count per surah from approved mushaf
5. If text is provided, match against approved mushaf text

### 9.3 Hadith Reference Matching

**Text-Based Matching:**
- The hadith text is normalized (diacritics optional, common variants)
- Matched against the hadith corpus database
- Multiple wording variants are handled
- Partial matches flagged for human review
- Confidence score assigned to the match

**Number-Based Matching:**
- Hadith numbers are verified against the edition
- Multiple editions may have different numbering
- The platform specifies which edition's numbering is used
- Cross-referencing between different numbering systems is supported

**Known Edition Variations:**
- Sunan Abi Dawud: Turkish edition numbering vs. modern editions
- Sunan Ibn Majah: Single vs. two-volume numbering
- Musnad Ahmad: Old print numbering vs. modern reprints

### 9.4 Scholarly Work Matching

**Book Title Matching:**
- Arabic titles are matched in Arabic script
- Translated titles are mapped to Arabic originals
- Common abbreviations are expanded
- Edition information is verified

**Author Attribution:**
- Each work is attributed to its author
- Multiple works by the same author are distinguished
- Authenticity of attribution to the author is checked (some works are falsely attributed)
- Manuscript/holograph status is noted when relevant

### 9.5 Automated vs. Human Matching

| Match Type | Automated | Human Review Threshold |
|-----------|-----------|----------------------|
| Direct Quran reference | 100% automated | Not needed |
| Direct hadith reference (text match >95%) | Automated | Flag if below 95% |
| Hadith meaning match (text match 70-95%) | Partial | Required |
| Hadith with unclear reference | Minimal | Required |
| Scholarly reference in major work | Automated (with verification) | Sample review |
| Scholarly reference in obscure work | Partial | Required |
| Contemporary scholar reference | Partial | Required |

### 9.6 Reference Database Maintenance

The platform's reference databases are maintained through:

1. **Initial Loading:** Verified digital texts of all approved collections
2. **Cross-Referencing:** Multiple editions compared for accuracy
3. **Updates:** Only when new verified editions become available
4. **Quality Control:** Random sampling with human verification
5. **Version Control:** Database versioning for tracking changes
6. **Error Reporting:** User and scholar error reports are investigated

---

## 10. Source Hierarchy

### 10.1 Core Principle

Islamic sources have a clear hierarchy of authority. Higher-level sources take precedence over lower-level sources when there is apparent conflict. This hierarchy is fundamental to Islamic legal theory (usul al-fiqh).

### 10.2 Authority Levels

**Level 1 — Quran (Absolute Authority)**
- The literal word of Allah, unchanged and unchangeable
- Certain knowledge (qat'i al-thubut)
- Preferred over all other sources
- Never abrogated by any other source
- Basis for all Islamic knowledge

**Level 2 — Mutawatir Hadith (Certain Transmission)**
- Narrated by so many narrators at each level that collusion on falsehood is impossible
- Yields certain knowledge (qat'i)
- Number of narrators varies (minimum typically 10+ per level)
- Examples: "Whoever lies about me intentionally..." hadith, "Actions are by intentions"
- Priority: Below Quran, above all other hadith

**Level 3 — Sahih Hadith (Authentic)**
- Meets strict authenticity criteria
- Yields probable knowledge (dhanni) in most cases
- Bukhari and Muslim's hadith have highest priority within this level
- Other sahihat (authentic collections) follow
- Agreed upon (Bukhari + Muslim) > Bukhari alone > Muslim alone > others

**Level 4 — Hasan Hadith (Good)**
- Acceptable for most purposes
- Lower certainty than sahih
- Used in fiqh rulings, especially when supported by other evidence
- Hasan saheh (Tirmidhi's category) bridges level 3 and 4

**Level 5 — Scholarly Consensus (Ijma)**
- Agreement of all qualified scholars of a generation
- Cannot contradict Quran or authentic hadith
- Binding in matters where it exists
- Disputed ijma is noted (some claimed ijma is actually disputed)

**Level 6 — Qiyas (Analogical Reasoning)**
- Extending a ruling from an established case to a new case
- Requires 'illah (effective cause) that is shared
- Strength varies based on the clarity of the 'illah
- Rejected by Zahiri school (small minority)
- Used extensively in Hanafi and Shafi'i methodology

**Level 7 — Strong Scholarly Opinion (Ra'y)**
- Ijtihad of qualified scholars
- Must be based on sound methodology
- Multiple valid opinions may exist
- Strength depends on the scholar's qualifications and evidence

**Level 8 — Daif Hadith (Weak)**
- Used ONLY for:
  - Virtues of good deeds (fada'il al-a'mal)
  - Encouragement and warning (al-targhib wa al-tarhib)
  - Historical narratives (qisas)
  - Not for rulings (ahkam)
- Must be clearly identified as daif
- Should not be extremely weak (daif jiddan)

**Level 9 — Weaker Scholarly Opinions**
- Minority positions
- Opinions based on weaker evidence
- Opinions from less authoritative scholars
- Presented with full context of their weakness

### 10.3 Conflict Resolution Across Levels

| Conflict | Resolution |
|----------|-----------|
| Quran vs. anything else | Quran prevails |
| Mutawatir vs. Sahih | Mutawatir prevails |
| Sahih vs. Hasan | Sahih prevails (or reconcile) |
| Higher vs. Lower hadith | Higher prevails |
| Text vs. Qiyas | Text prevails (except Hanafi some cases) |
| Ijma vs. Individual opinion | Ijma prevails |
| Consensus vs. Individual qiyas | Consensus prevails |

### 10.4 Multiple Sources at Same Level

When multiple equally authentic sources exist:

1. **Quran:** All ayat equally authoritative; reconcile apparent conflicts through tafsir
2. **Hadith (same grade):** Attempt reconciliation; if impossible, use tarjih (preference)
3. **Scholarly opinions (same level):** All presented; no single one enforced

### 10.5 Presentation Based on Source Level

| Source Level | Presentation Style |
|-------------|-------------------|
| 1-2 (Certain) | "The Quran states..." / "The Prophet definitively said..." |
| 3 (Sahih) | "The Prophet said (as narrated by Bukhari)..." |
| 4 (Hasan) | "The Prophet is reported to have said (hasan hadith)..." |
| 5 (Ijma) | "The scholars are in consensus that..." |
| 6 (Qiyas) | "By analogy with..., scholars have concluded..." |
| 7 (Ra'y) | "Some scholars, including [name], hold the opinion that..." |
| 8 (Daif) | "A weak hadith mentions..." (with explicit weakness warning) |
| 9 (Weak) | "A minority opinion holds that..." |

---

## 11. Conflict Resolution

### 11.1 Core Principle

Apparent conflicts between religious sources are not actual contradictions. They arise from differing contexts, levels of understanding, or application conditions. The platform uses established Islamic methodologies to resolve apparent conflicts.

### 11.2 Categories of Conflict

**Apparent Conflict:** Sources appear to say different things but can be reconciled through proper understanding
- Example: "No compulsion in religion" (Quran 2:256) vs. "Fight those who do not believe" (Quran 9:29) — contextualized by abrogation or specificity

**Actual Conflict (Impossible in Revelation):** True contradiction cannot exist in divinely revealed sources
- If two revealed texts seem to truly contradict and cannot be reconciled, the issue is with human understanding, not the texts
- Resolution is sought through established usul al-fiqh methods

**Text vs. Interpretation Conflict:**
- The text itself is not in conflict; scholarly interpretations differ
- The platform presents all valid interpretations with their evidence

### 11.3 Resolution Method 1: Al-Jam' (Reconciliation)

**The preferred method — reconciling both sources as valid in different contexts.**

**Conditions:**
- Both sources are authentic
- A reasonable reconciliation is possible
- The reconciliation is supported by scholarly evidence

**Example:**
- "When the prayer is called on Friday, hasten to the remembrance of Allah and leave trade" (Quran 62:9)
- "There is no sin upon you if you seek bounty from your Lord" (Quran 2:198) — referring to trade after hajj
- Reconciliation: Both are valid — one addresses Friday prayer obligation, the other addresses trade during hajj season

**Reconciliation Techniques:**
- **Contextualization:** Each applies to different circumstances
- **Specification (Takhsis):** General vs. specific application
- **Conditioning (Taqyid):** Absolute vs. conditional application
- **Time-Based:** Different times, different rules
- **Person-Based:** Different people, different obligations

### 11.4 Resolution Method 2: Al-Naskh (Abrogation)

**One source supersedes another (applies only to Quran and hadith between which reconciliation is impossible).**

**Conditions:**
- Both sources are authentically from revelation
- Reconciliation is genuinely impossible
- The chronological order is known (later abrogates earlier)
- Evidence for naskh is provided

**Types of Naskh:**
- **Naskh al-Hukm wa al-Tilawah:** Both ruling and recitation abrogated (rare, mostly related to early Madinah period)
- **Naskh al-Hukm Duna al-Tilawah:** Ruling abrogated but text remains (e.g., inheritance verses)
- **Naskh al-Tilawah Duna al-Hukm:** Recitation abrogated but ruling remains (very rare)

**Presentation:**
- The abrogating verse/hadith is identified
- The abrogated verse/hadith is identified
- The scholar(s) who identified the naskh are cited
- If scholars disagree on whether naskh occurred, all positions are presented

### 11.5 Resolution Method 3: Al-Tarjih (Preference)

**Choosing one source over another based on preponderance of evidence.**

**Preference Criteria:**
- **Stronger chain:** Sahih preferred over hasan, hasan preferred over daif
- **More narrators:** More transmission paths preferred
- **Direct wording:** Explicit preference over implicit
- **Contextual clues:** Specific over general, later over earlier (if naskh cannot be confirmed)
- **Quranic consistency:** Position more consistent with Quran
- **Stronger analogy:** Qiyas with clearer 'illah

**Presentation:**
- The preferred position is identified
- The basis for preference is explained
- The non-preferred position is still presented
- The user is informed that preference is a scholarly judgment

### 11.6 Resolution Method 4: Al-Tawaqquf (Suspension)

**Acknowledging that we cannot definitively resolve the apparent conflict with available knowledge.**

**When Used:**
- Both sources are equally strong in evidence
- No reconciliation is clearly superior
- No definitive chronological data exists
- Scholars themselves have disagreed and not resolved

**Presentation:**
- The apparent conflict is clearly stated
- Both positions are presented with their evidence
- The reason suspension is appropriate is explained
- The user is informed that qualified scholars disagree on the resolution
- Humility before uncertainty is maintained

### 11.7 Conflict Resolution Documentation

Every conflict resolution is documented with:

1. **Sources involved:** Both texts with full citations
2. **Nature of conflict:** Apparent or actual
3. **Resolution method:** Jam', naskh, tarjih, or tawaqquf
4. **Scholarly support:** Scholars who support this resolution
5. **Alternative resolutions:** If other scholars resolved it differently
6. **Confidence level:** How certain the resolution is

### 11.8 Prohibited Approaches

The platform NEVER:
- Dismisses clear Quranic text in favor of any other source
- Claims naskh without scholarly evidence
- Fabricates a reconciliation where none exists
- Presents a personal opinion as a resolution of scholarly disagreement
- Dismisses a source simply because it is inconvenient
- Claims that all opinions are equally valid when one opinion has overwhelmingly stronger evidence

---

## 12. Hallucination Prevention

### 12.1 Core Principle

Religious content hallucination is the most serious category of error the platform can produce. Hallucinated Quranic verses, fabricated hadith, or false attribution to scholars constitute misinformation with spiritual consequences. The platform employs multiple layers of hallucination prevention specifically for religious content.

### 12.2 Prohibited AI Behaviors

The AI is explicitly prohibited from:

1. **Inventing Quranic Verses:** The AI never generates Quranic text from memory or prediction. All Quranic text must be retrieved from the verified mushaf database.
2. **Fabricating Hadith:** The AI never creates hadith texts. All hadith must be retrieved from the verified hadith corpus.
3. **Creating False Attribution:** The AI never attributes statements to scholars without verification from the source database.
4. **Fabricating Chains:** The AI never creates chains of narration. All isnads must be verified against the narrator database.
5. **Inventing Consensus:** The AI never claims scholarly consensus (ijma) that is not documented in the verifiable consensus database.
6. **Creating False Context:** The AI never fabricates asbab al-nuzul or asbab al-wurud.
7. **Paraphrasing Revelation:** The AI never paraphrases Quranic text or presents paraphrase as Quran.

### 12.3 Detection Mechanisms

**Pre-Generation Detection:**
- The prompt/query is analyzed for religious content
- The topic is identified and mapped to approved sources
- Any request for fabricated or unverifiable content is rejected

**During-Generation Detection:**
- Generated content is checked in real-time against source databases
- Any unverifiable claim triggers an immediate halt
- The generation is redirected to verified content only

**Post-Generation Detection:**
- Complete content is scanned for:
  - Unattributed claims
  - Suspected fabricated hadith
  - Incorrect Quranic references
  - False scholarly attributions
- Automated detection with configurable thresholds
- Failed content is quarantined for human review

### 12.4 Unknown Source Protocol

When the platform encounters a religious claim it cannot verify:

**Response Protocol:**
1. **Low Confidence (Below 60%):** "I cannot verify this claim. The sources I have do not confirm this statement. Please consult a qualified scholar."
2. **Medium Confidence (60-79%):** "This claim has some support, but I cannot fully verify it. The available evidence suggests [summary], but this should be reviewed by a scholar."
3. **Unrecognized Source:** "The source you are referencing is not in my verified database. I cannot confirm the accuracy of this citation."

**The AI NEVER:**
- Pretends to know an unverified hadith
- Guesses at a source
- Fills in missing details
- Creates a chain of narration from partial information

### 12.5 Confidence Thresholds for Presentation

| Confidence Level | Presentation Style | Required Action |
|-----------------|-------------------|----------------|
| 95-100% | Presented as established fact | None |
| 90-94% | Presented with minor qualifiers | Optional human review |
| 80-89% | Presented with context and qualifiers | Recommended human review |
| 75-79% | Presented as scholarly opinion | Human review required |
| 60-74% | Presented with alternatives and low confidence | Human review required |
| Below 60% | Not presented as content | Flagged, sent to review or rejected |

### 12.6 Source Database Referencing

All religious claims reference the verified source databases:

**Quran Database:**
- Contains the full Uthmani mushaf text
- Surah-by-surah, ayah-by-ayah
- Indexed by surah number and ayah number
- Cannot be modified by AI

**Hadith Database:**
- Contains verified texts from approved collections
- Indexed by collection, book, hadith number
- Includes gradings from major scholars
- Read-only for AI (AI cannot add to database)

**Scholar Database:**
- Contains verified biographical data
- Indexed by name, kunya, nisba
- Includes reliability classifications
- Read-only for AI

### 12.7 Hallucination Incident Response

If a hallucination is detected:

1. **Immediate Quarantine:** Affected content is removed from user-facing systems
2. **Incident Logging:** Full details logged for analysis
3. **Root Cause Analysis:** Determine why the hallucination occurred
4. **System Update:** Prevention mechanisms are updated
5. **Content Review:** All affected content reviewed and corrected
6. **Notification:** If content reached users, corrective communication issued

### 12.8 Guardrails Configuration

The AI system has hard guardrails that cannot be bypassed:

- **Guardrail 1:** Quranic text output is ALWAYS sourced from the verified database — AI never generates Quran text
- **Guardrail 2:** Hadith output is ALWAYS sourced from the verified corpus — AI never generates hadith text
- **Guardrail 3:** Scholarly attributions are ALWAYS verified against the scholar database
- **Guardrail 4:** The AI CANNOT present unverified religious claims as fact
- **Guardrail 5:** The AI MUST respond "I cannot verify this" for unrecognized sources
- **Guardrail 6:** The AI CANNOT claim consensus that is not in the consensus database

---

## 13. Human Review Workflow

### 13.1 Core Principle

While AI handles the majority of religious content verification, human scholars are essential for edge cases, disputed content, and quality assurance. The human review workflow ensures that AI-generated religious content meets scholarly standards before reaching users.

### 13.2 Reviewer Tier System

**Tier 1 — Basic Islamic Knowledge Reviewer**

**Qualifications:**
- Completed memorization of at least 5 juz of Quran
- Basic knowledge of hadith terminology
- Familiarity with the four madhhabs
- Training on the platform's verification system

**Responsibilities:**
- Review standard content for obvious errors
- Verify citations format and completeness
- Flag content for Tier 2 review when issues are found
- Pass rate: 95% of correctly verified content

**Review Scope:**
- Non-controversial fiqh content
- Basic Islamic beliefs (pillars of iman, etc.)
- Quranic recitation verification
- Hadith citations that match known gradings

---

**Tier 2 — Specialized Scholar Per Topic**

**Qualifications:**
- Traditional ijazah or recognized university degree in Islamic studies
- Specialization in the relevant field (fiqh, hadith, tafsir, aqidah)
- Minimum 5 years of teaching or research experience
- Familiarity with multiple madhhabs

**Responsibilities:**
- Review all disputed content
- Verify hadith gradings and chains
- Review scholarly attributions
- Approve or modify content flagged by Tier 1
- Document their reasoning for modifications

**Review Scope:**
- Fiqh rulings with multiple madhhab positions
- Hadith with disputed gradings
- Tafsir content with multiple interpretations
- Contemporary fatwas and applications

**Authority:**
- Can override Tier 1 decisions
- Can request Tier 3 review
- Their approval is required for content reaching users

---

**Tier 3 — Senior Scholar Panel**

**Qualifications:**
- Senior scholar with 15+ years of experience
- Recognized authority in their field
- Publication record in peer-reviewed Islamic studies
- Ijazah to teach and issue fatwas

**Composition:**
- Minimum 3 scholars for any review
- Representation from relevant madhhabs
- Gender balance where relevant

**Responsibilities:**
- Review ijtihad-level content
- Resolve disputes between Tier 2 reviewers
- Authorize content on controversial issues
- Set precedent for recurring issues

**Review Scope:**
- Novel ijtihad issues
- Disputed scholarly consensus claims
- Content with significant inter-madhhab disagreement
- Issues with contemporary political or social sensitivity
- Content that may affect the platform's religious standing

**Authority:**
- Final decision on all religious content
- Can override Tier 1 and Tier 2 decisions
- Decisions are binding across the platform
- Decisions are documented as platform precedent

### 13.3 Review Workflow Steps

**Step 1 — AI Generation:**
- AI generates religious content according to this Bible
- AI attaches confidence scores, evidence scores, and source citations
- Content enters the review queue

**Step 2 — Automated Pre-Review:**
- System checks: format, citation format, confidence thresholds
- Automated checks pass: content moves to appropriate tier
- Automated checks fail: content rejected or modified

**Step 3 — Tier Assignment:**
- Content with confidence > 95% and standard content: Tier 1
- Content with disputed sources or moderate confidence: Tier 2
- Content with novel issues or low confidence: Tier 2 or Tier 3
- Content with platform-wide implications: Tier 3

**Step 4 — Human Review:**
- Reviewer sees: AI-generated content, sources, confidence scores, evidence scores, alternative views
- Reviewer: confirms as-is, modifies content, adds notes, or rejects
- Reviewer provides reasoning for any change

**Step 5 — Quality Check:**
- Random sampling of reviewed content
- Cross-reviewer consistency checks
- Periodic audits by Tier 3 panel

**Step 6 — Publication:**
- Approved content enters the verified knowledge base
- Content is versioned with reviewer information
- Original AI content and reviewer modifications are both stored

### 13.4 Review Interface Requirements

The review interface must show:

**For Every Claim:**
- The claim text
- Source citations (Quran, hadith, scholarly opinion)
- AI confidence score
- AI evidence score
- Alternative views (if applicable)
- Scholar grading (for hadith)
- Original Arabic text (for Quran and hadith)

**Reviewer Actions:**
- ✅ Confirm: Accept content as-is
- ✏️ Modify: Edit content with reasoning
- ❌ Reject: Reject with explanation
- ⬆️ Escalate: Send to higher tier

### 13.5 Multi-Scholar Review Protocol

**When Required:**
- Ijtihad-level issues
- Novel contemporary rulings
- Disputed historical events
- Content with significant theological implications
- Content that affects multiple madhhabs

**Process:**
1. Three or more qualified scholars review independently
2. Each scholar provides their assessment
3. If unanimous: content is approved
4. If majority opinion exists: content presents majority position with minority noted
5. If no consensus: content is held for further deliberation or presents all positions

### 13.6 Review Metrics and Quality Assurance

| Metric | Target | Measurement |
|--------|--------|-------------|
| Tier 1 accuracy | 98% | Random audits by Tier 2 |
| Tier 2 accuracy | 99.5% | Random audits by Tier 3 |
| Review turnaround | 24 hours (Tier 1), 72 hours (Tier 2), 1 week (Tier 3) | Time tracking |
| Dispute rate | < 5% | Reviewer disagreement tracking |
| Error escape rate | < 0.1% | Post-publication error detection |

### 13.7 Reviewer Training

**Initial Training:**
- Full reading of the Religious Verification Bible
- Hands-on training with the review interface
- Review of 50 sample cases with expert feedback
- Assessment: 90% accuracy required for certification

**Ongoing Training:**
- Monthly updates on platform policies
- Quarterly review of common errors
- Annual recertification

---

## 14. Confidence Score

### 14.1 Core Principle

Every religious claim on the platform has a confidence score that reflects the platform's certainty in the claim's accuracy. This score is based on multiple factors, including source hierarchy, multiple source confirmation, scholar agreement level, chain strength, and textual clarity.

### 14.2 Confidence Score Components

**1. Source Authority (Weight: 35%)**
| Source | Score |
|--------|-------|
| Quran | 100 |
| Mutawatir Hadith | 98 |
| Sahih Hadith (Agreed) | 95 |
| Sahih Hadith (Bukhari) | 93 |
| Sahih Hadith (Muslim) | 92 |
| Sahih Hadith (Other) | 85 |
| Hasan Hadith | 75 |
| Scholarly Consensus | 90 |
| Qiyas (Strong) | 65 |
| Qiyas (Weak) | 50 |
| Strong Scholarly Opinion | 60 |
| Daif Hadith | 35 |
| Weak Opinion | 25 |

**2. Multiple Source Confirmation (Weight: 15%)**
| Confirmation Level | Score |
|-------------------|-------|
| 5+ independent sources | 100 |
| 3-4 independent sources | 90 |
| 2 independent sources | 80 |
| 2 related sources | 70 |
| 1 source | 60 |
| No confirmation | 0 |

**3. Scholar Agreement (Weight: 20%)**
| Agreement Level | Score |
|----------------|-------|
| Consensus (Ijma) | 100 |
| Overwhelming majority | 95 |
| Strong majority (75%+) | 85 |
| Majority (60%+) | 75 |
| Plurality | 65 |
| Divided | 50 |
| Minority | 40 |
| Single scholar | 30 |

**4. Chain Strength — For Hadith (Weight: 15%)**
| Chain Classification | Score |
|---------------------|-------|
| Highest (A'la al-Isnad) | 100 |
| Strong (Qawi) | 85 |
| Good (Hasan) | 70 |
| Weak (Daif) | 40 |
| Very Weak (Wahi) | 20 |
| Fabricated | 0 |

**5. Textual Clarity (Weight: 15%)**
| Clarity Level | Score |
|---------------|-------|
| Clear and unambiguous (muhkam) | 100 |
| Clear with context | 90 |
| Requires minor interpretation | 80 |
| Requires significant interpretation | 65 |
| Ambiguous (mutashabihat) | 50 |
| Multiple valid interpretations | 45 |
| Unclear meaning | 30 |

### 14.3 Score Calculation

**Formula:**
```
Confidence Score = (Source Authority × 0.35) +
                   (Multiple Sources × 0.15) +
                   (Scholar Agreement × 0.20) +
                   (Chain Strength × 0.15) +
                   (Textual Clarity × 0.15)
```

**Rounding:** Scores are rounded to the nearest whole number.

### 14.4 Score Interpretation

| Score Range | Meaning | Presentation |
|-------------|---------|-------------|
| 95-100 | Established fact | "The Quran states..." / "The Prophet definitively said..." |
| 90-94 | Strongly established | "Based on strong evidence..." |
| 80-89 | Established with majority support | "The majority of scholars hold..." |
| 75-79 | Probable | "The available evidence suggests..." |
| 70-74 | Likely but uncertain | "Many scholars have held the opinion..." |
| 60-69 | Scholarly opinion | "Some scholars, including [name], hold..." |
| 50-59 | Weak claim | "A weak report indicates..." |
| Below 50 | Unreliable | "This claim cannot be verified. Consult a scholar." |

### 14.5 Score Display

**In Content:**
- Scores 95+: No score display needed (present as fact)
- Scores 80-94: Score shown in metadata (not in content text)
- Scores 60-79: Qualitative description shown in content
- Scores below 60: Flagged, not presented without modification

**In Review Dashboard:**
- Full score breakdown shown
- Each component score visible
- Reviewer can see why a score is what it is

### 14.6 Score Modification

Human reviewers can modify confidence scores:
- Reviewer provides reasoning
- Score change is logged
- Score changes are audited periodically
- Systematic score errors trigger model retraining

---

## 15. Evidence Score

### 15.1 Core Principle

The evidence score measures the quantity and quality of evidence supporting a religious claim. Unlike confidence (which measures certainty), evidence score measures how much evidence exists, regardless of interpretation.

### 15.2 Evidence Score Components

**1. Evidence Quantity (Weight: 40%)**
| Quantity of Sources | Score |
|--------------------|-------|
| 10+ independent sources | 100 |
| 5-9 independent sources | 90 |
| 3-4 independent sources | 80 |
| 2 independent sources | 70 |
| 1 source, multiple transmission paths | 65 |
| 1 source, single transmission | 50 |
| Indirect evidence only | 30 |
| No evidence | 0 |

**2. Evidence Quality (Weight: 40%)**
| Quality Level | Score |
|--------------|-------|
| Multiple Quranic verses + mutawatir hadith | 100 |
| Quran + multiple sahih hadith | 95 |
| Multiple sahih hadith | 90 |
| Single sahih hadith (agreed) | 85 |
| Single sahih hadith | 80 |
| Quranic indication (isharah) | 75 |
| Hasan hadith + supporting evidence | 70 |
| Single hasan hadith | 60 |
| Scholarly consensus (documented) | 85 |
| Strong qiyas | 65 |
| Weak qiyas | 50 |
| Daif hadith alone | 30 |

**3. Evidence Diversity (Weight: 20%)**
| Diversity of Source Types | Score |
|--------------------------|-------|
| Quran + Hadith + Ijma + Qiyas | 100 |
| Quran + Hadith + Ijma | 95 |
| Quran + Hadith | 90 |
| Hadith + Ijma | 85 |
| Hadith + Qiyas | 75 |
| Hadith only (multiple) | 70 |
| Hadith only (single) | 55 |
| Ijma only | 65 |
| Qiyas only | 45 |
| Single scholarly opinion | 30 |

### 15.3 Score Calculation

**Formula:**
```
Evidence Score = (Evidence Quantity × 0.40) +
                (Evidence Quality × 0.40) +
                (Evidence Diversity × 0.20)
```

### 15.4 Score Interpretation

| Score Range | Meaning | Presentation |
|-------------|---------|-------------|
| 90-100 | Overwhelming evidence | "This is established by multiple sources..." |
| 80-89 | Strong evidence | "There is strong evidence for this position..." |
| 70-79 | Good evidence | "There is good evidence supporting this view..." |
| 60-69 | Moderate evidence | "There is some evidence for this position..." |
| 50-59 | Limited evidence | "The evidence for this is limited..." |
| Below 50 | Weak evidence | "There is insufficient evidence for this claim..." |

### 15.5 Evidence Score vs. Confidence Score

| Aspect | Confidence Score | Evidence Score |
|--------|-----------------|----------------|
| Measures | Certainty | Quantity + Quality |
| Focus | How sure are we? | How much evidence? |
| Affected by | Interpretation | Source counting |
| Use | Presentation style | Weight in review |
| Changes | With new understanding | With new sources |

---

## 16. Authenticity Score

### 16.1 Core Principle

The authenticity score specifically measures the textual authenticity of religious texts — how confident we are that a text is authentically from the source it claims to be from.

### 16.2 Authenticity by Text Type

| Text Type | Base Score | Rationale |
|-----------|-----------|-----------|
| Quran | 100 | Absolute — the word of Allah, mass-transmitted, perfectly preserved |
| Mutawatir Hadith | 95-100 | Mass-transmitted at every level, certain knowledge |
| Mashhur Hadith | 85-94 | Widely transmitted, high probability |
| Sahih Hadith | 80-94 | Meets strict authenticity criteria |
| Hasan Hadith | 60-79 | Acceptable with minor concerns |
| Hasan li-Ghayrihi | 50-69 | Acceptable due to supporting evidence |
| Daif Hadith (mild) | 30-59 | Weak, some issues |
| Daif Hadith (significant) | 15-29 | Significant weakness |
| Daif Jiddan | 5-14 | Very weak, near fabrication |
| Maudu (Fabricated) | 0-4 | Falsely attributed |

### 16.3 Authenticity Score Factors

**1. Chain of Transmission Quality (Weight: 40%)**
| Factor | Contribution |
|--------|-------------|
| Number of transmission paths | Multiple paths increase score |
| Narrator reliability | Trustworthy narrators increase score |
| Chain continuity | Continuous chains score higher |
| Absence of defects | No 'illah (hidden defects) |
| Absence of irregularity | No shudhudh (irregularities) |

**2. Manuscript Evidence (Weight: 25%)**
| Factor | Contribution |
|--------|-------------|
| Earliest manuscript witness | Earlier = higher score |
| Number of manuscripts | More = higher score |
| Textual consistency | Consistent across manuscripts |
| Geographical distribution | Widely distributed = higher |
| Scribal quality | Better preservation = higher |

**3. Scholarly Acceptance (Weight: 20%)**
| Factor | Contribution |
|--------|-------------|
| Accepted by early scholars | Early acceptance = strong indicator |
| Accepted by hadith masters | Ibn Hajar, al-Dhahabi, etc. |
| Used in fiqh works | Practical acceptance |
| No serious criticism | If criticized, score drops |
| Consistent with known authentic texts | Textual consistency |

**4. Textual Consistency (Weight: 15%)**
| Factor | Contribution |
|--------|-------------|
| Consistency with Quran | Speaks to compatibility |
| Consistency with known authentic hadith | No contradiction |
| Consistency with historical context | Makes historical sense |
| Consistency with Arabic usage | Linguistically sound |
| Absence of anachronism | No historical errors |

### 16.4 Authenticity Score Calculation

**For Quranic Texts:**
- Score is always 100 — no calculation needed
- This is hardcoded and cannot be changed

**For Hadith Texts:**
```
Authenticity Score = (Chain Quality × 0.40) +
                    (Manuscript Evidence × 0.25) +
                    (Scholarly Acceptance × 0.20) +
                    (Textual Consistency × 0.15)
```

**For Other Texts (historical reports, scholarly works):**
```
Authenticity Score = (Manuscript Evidence × 0.30) +
                    (Attribution Evidence × 0.35) +
                    (Scholarly Acceptance × 0.20) +
                    (Textual Consistency × 0.15)
```

### 16.5 Binding of Scores

| Score Type | Quran | Hadith | Scholarly Works |
|-----------|-------|--------|----------------|
| Authenticity | Fixed at 100 | Variable (5-100) | Variable |
| Confidence | Fixed at 100 | Based on grading | Based on source hierarchy |
| Evidence | N/A | Based on corroboration | Based on evidence diversity |

---

## 17. Knowledge Graph Integration

### 17.1 Core Principle

Religious verification data feeds into the Knowledge Factory's knowledge graph, enabling rich connections between sources, scholars, concepts, and rulings. This section documents how verification data is structured for graph integration.

### 17.2 Graph Node Types

**Quran Nodes:**
- Type: `quran_surah`, `quran_ayah`
- Properties: number, name (Arabic + English), revelation_type, ayah_count
- Relationships: contains, references, abrogates, is_abrogated_by

**Hadith Nodes:**
- Type: `hadith_collection`, `hadith_book`, `hadith`
- Properties: collection_id, book_number, hadith_number, grade, grading_scholar
- Relationships: narrated_by, related_to_topic, graded_by

**Narrator Nodes:**
- Type: `narrator`
- Properties: full_name, kunya, nisba, lifespan, reliability, tabaqah
- Relationships: narrated_from, narrated_to, contemporaneous_with

**Scholar Nodes:**
- Type: `scholar`
- Properties: full_name, madhhab, lifespan, specialization, works
- Relationships: authored, gave_opinion_on, disagreed_with, student_of

**Concept Nodes:**
- Type: `islamic_concept`, `fiqh_ruling`, `aqidah_belief`
- Properties: name, category, madhhab_positions
- Relationships: derived_from, supported_by, opposed_by

### 17.3 Verification Metadata in Graph

Every node and relationship can carry verification metadata:
- confidence_score
- evidence_score
- authenticity_score
- verification_status (unverified, verified_tier1, verified_tier2, verified_tier3)
- last_verified_date
- verified_by

### 17.4 Graph Queries for Verification

**Example: Verify a hadith's chain**
```
MATCH (h:Hadith {hadith_number: "3162", collection: "BUK"})
MATCH (h)-[:HAS_CHAIN]->(c:Chain)
MATCH (c)-[:INCLUDES]->(n:Narrator)
RETURN h, c, n
```

**Example: Find all opinions on a fiqh topic**
```
MATCH (c:FiqhConcept {name: "wudu_breaking"})
MATCH (s:Scholar)-[:GAVE_OPINION_ON]->(o:Opinion)-[:ABOUT]->(c)
RETURN s.madhhab, o.ruling, o.evidence
```

---

## 18. Verification Agent Specifications

### 18.1 Agent Types

The Religious Verification pipeline uses specialized AI agents:

**Agent: Quranic Text Verifier**
- Purpose: Verify Quranic text matches the approved mushaf
- Input: Arabic text, surah name, ayah number
- Process: Character-by-character comparison with database
- Output: Verification status (match/mismatch), error details
- Confidence threshold: 100% required for approval

**Agent: Hadith Verifier**
- Purpose: Verify hadith text and citations
- Input: Hadith text, collection reference, chain
- Process: Text matching, chain verification, grading lookup
- Output: Verification status, confidence score, alternative gradings
- Automation: Full automation for matching entries, human referral for mismatches

**Agent: Chain Analyzer**
- Purpose: Analyze and verify chains of narration
- Input: Full isnad (narrator list)
- Process: Narrator identification, reliability lookup, continuity check
- Output: Chain strength score, narrator-by-narrator analysis
- Automation: Fully automated against narrator database

**Agent: Scholar Opinion Matcher**
- Purpose: Match scholarly opinions to verified sources
- Input: Claim, attributed scholar, source
- Process: Database lookup, cross-reference
- Output: Verification status, confidence score
- Automation: Fully automated for known works, human review for rare works

**Agent: Reference Format Checker**
- Purpose: Verify citation formatting
- Input: Full content text
- Process: Pattern matching against citation format rules
- Output: List of valid citations, malformed citations, missing citations
- Automation: Fully automated

**Agent: Confidence Calculator**
- Purpose: Calculate confidence scores
- Input: Verified claim metadata
- Process: Score calculation per formula in Section 14
- Output: Confidence score with component breakdown
- Automation: Fully automated

**Agent: Hallucination Detector**
- Purpose: Detect potential religious content hallucinations
- Input: Generated content
- Process: Pattern analysis, source cross-reference, anomaly detection
- Output: Hallucination risk score, flagged content
- Automation: Fully automated, escalation to human on high risk

### 18.2 Agent Communication

Agents communicate via the AIOS Message Bus (see AIOS documentation):

```
[Content Generator] → [Reference Format Checker] → [Quranic Text Verifier or Hadith Verifier]
    → [Chain Analyzer (if hadith)] → [Confidence Calculator] → [Hallucination Detector]
    → [Human Review Queue (if needed)] → [Knowledge Graph Integration]
```

### 18.3 Agent Error Handling

| Error | Detection | Resolution |
|-------|-----------|------------|
| Quran mismatch | Quranic Text Verifier | Block, regenerate with correct text |
| Hadith not found | Hadith Verifier | Flag for human review |
| Chain incomplete | Chain Analyzer | Flag, attempt reconstruction |
| Scholar not in database | Scholar Opinion Matcher | Manual addition required |
| Malformed citation | Reference Format Checker | Auto-correct or reject |
| High hallucination risk | Hallucination Detector | Immediate human review |

---

## 19. Integration with Knowledge Factory Pipeline

### 19.1 Pipeline Position

The Religious Verification Bible governs the Verification stage specifically for Islamic content. It integrates with the broader Knowledge Factory as follows:

```
[Content Acquisition] → [Document Processing] → [Verification]
    → [Knowledge Extraction] → [Knowledge Graph] → [Learning Mode Generation]
```

Within the Verification stage:
```
[Source Specific Verification] → [Religious Verification] → [General Verification] → [Human Review]
    [Quran]
    [Hadith]
    [Scholar Opinions]
    [Fiqh Content]
    [Aqidah Content]
```

### 19.2 Connections to Other Bibles

**00 Knowledge Factory Bible:**
- This Bible implements Section 5 (Verification Pipeline) of the Knowledge Factory Bible
- Verification agent specifications align with the Knowledge Factory's agent architecture
- Confidence scores from this Bible feed into the Knowledge Factory's quality metrics

**09 Verification Documentation:**
- General verification frameworks from Documentation/09_Verification apply where not overridden
- Religious verification is a specialization of general verification
- This Bible's protocols take precedence for Islamic content

**11 Workflows:**
- Human review workflows from AI_Company/11_Workflows define reviewer management
- This Bible specifies the religious qualifications for reviewers
- Workflow integration: review tasks from this Bible are executed via the enterprise workflow system

### 19.3 Content Type Routing

| Content Type | Verification Pathway |
|-------------|---------------------|
| Quranic text | Direct to Quranic Text Verifier |
| Hadith (with citation) | Hadith Verifier → Chain Analyzer |
| Hadith (without citation) | Hadith Verifier → Reference Search |
| Fiqh ruling | Madhhab-specific verification → Scholar Opinion Matcher |
| Aqidah content | Cross-madhhab verification → Consensus check |
| Islamic history | General verification + religious source verification |
| Contemporary fatwa | Scholar verification → Tier 2 or Tier 3 review |
| Sirah (biography) | Source-based verification → chain analysis |
| Arabic grammar examples | Grammar verification + religious source verification if applicable |

### 19.4 Quality Metrics Integration

The Religious Verification Bible contributes to the Knowledge Factory's quality metrics:

| Metric | Source | Target |
|--------|--------|--------|
| Verification accuracy | Random audit score | 99.5%+ |
| Hallucination rate | Automated detection | 0% target |
| Citation completeness | Automated check | 100% |
| Confidence score accuracy | Post-audit score comparison | 95%+ correlation |
| Human review turnaround | Time tracking | Per tier targets (Sec 13.6) |

### 19.5 Pipeline Failure Recovery

If the verification pipeline detects an error:

1. **Content is quarantined:** Not passed to Knowledge Extraction
2. **Error is logged:** Full incident record with all verification agent outputs
3. **Root cause identified:** Which agent/step detected the error
4. **Human notified:** Tier 1 or Tier 2 reviewer assigned based on error type
5. **Fix applied:** Content corrected or pipeline step adjusted
6. **Content re-verified:** Passes through verification again
7. **Released:** Only after passing all verification steps

---

## 20. Appendix: Reference Tables

### 20.1 Surah Reference Table

| No | Name (Arabic) | Name (English) | Ayah Count | Revelation Type | Juz |
|----|--------------|----------------|------------|-----------------|-----|
| 1 | الفاتحة | Al-Fatihah | 7 | Makki | 1 |
| 2 | البقرة | Al-Baqarah | 286 | Madani | 1-3 |
| 3 | آل عمران | Al-Imran | 200 | Madani | 3-4 |
| 4 | النساء | An-Nisa | 176 | Madani | 4-6 |
| 5 | المائدة | Al-Maidah | 120 | Madani | 6-7 |
| 6 | الأنعام | Al-An'am | 165 | Makki | 7-8 |
| 7 | الأعراف | Al-A'raf | 206 | Makki | 8-9 |
| 8 | الأنفال | Al-Anfal | 75 | Madani | 9-10 |
| 9 | التوبة | At-Tawbah | 129 | Madani | 10-11 |
| 10 | يونس | Yunus | 109 | Makki | 11 |
| 11 | هود | Hud | 123 | Makki | 11-12 |
| 12 | يوسف | Yusuf | 111 | Makki | 12-13 |
| 13 | الرعد | Ar-Ra'd | 43 | Madani | 13 |
| 14 | إبراهيم | Ibrahim | 52 | Makki | 13-14 |
| 15 | الحجر | Al-Hijr | 99 | Makki | 14 |
| 16 | النحل | An-Nahl | 128 | Makki | 14-15 |
| 17 | الإسراء | Al-Isra | 111 | Makki | 15 |
| 18 | الكهف | Al-Kahf | 110 | Makki | 15-16 |
| 19 | مريم | Maryam | 98 | Makki | 16 |
| 20 | طه | Taha | 135 | Makki | 16 |
| 21 | الأنبياء | Al-Anbiya | 112 | Makki | 17 |
| 22 | الحج | Al-Hajj | 78 | Madani | 17 |
| 23 | المؤمنون | Al-Mu'minun | 118 | Makki | 18 |
| 24 | النور | An-Nur | 64 | Madani | 18 |
| 25 | الفرقان | Al-Furqan | 77 | Makki | 19 |
| 26 | الشعراء | Ash-Shu'ara | 227 | Makki | 19 |
| 27 | النمل | An-Naml | 93 | Makki | 19-20 |
| 28 | القصص | Al-Qasas | 88 | Makki | 20 |
| 29 | العنكبوت | Al-Ankabut | 69 | Makki | 21 |
| 30 | الروم | Ar-Rum | 60 | Makki | 21 |
| 31 | لقمان | Luqman | 34 | Makki | 21 |
| 32 | السجدة | As-Sajdah | 30 | Makki | 21 |
| 33 | الأحزاب | Al-Ahzab | 73 | Madani | 21-22 |
| 34 | سبأ | Saba | 54 | Makki | 22 |
| 35 | فاطر | Fatir | 45 | Makki | 22 |
| 36 | يس | Ya-Sin | 83 | Makki | 22-23 |
| 37 | الصافات | As-Saffat | 182 | Makki | 23 |
| 38 | ص | Sad | 88 | Makki | 23 |
| 39 | الزمر | Az-Zumar | 75 | Makki | 23-24 |
| 40 | غافر | Ghafir | 85 | Makki | 24 |
| 41 | فصلت | Fussilat | 54 | Makki | 24-25 |
| 42 | الشورى | Ash-Shura | 53 | Makki | 25 |
| 43 | الزخرف | Az-Zukhruf | 89 | Makki | 25 |
| 44 | الدخان | Ad-Dukhan | 59 | Makki | 25 |
| 45 | الجاثية | Al-Jathiyah | 37 | Makki | 25 |
| 46 | الأحقاف | Al-Ahqaf | 35 | Makki | 26 |
| 47 | محمد | Muhammad | 38 | Madani | 26 |
| 48 | الفتح | Al-Fath | 29 | Madani | 26 |
| 49 | الحجرات | Al-Hujurat | 18 | Madani | 26 |
| 50 | ق | Qaf | 45 | Makki | 26 |
| 51 | الذاريات | Adh-Dhariyat | 60 | Makki | 27 |
| 52 | الطور | At-Tur | 49 | Makki | 27 |
| 53 | النجم | An-Najm | 62 | Makki | 27 |
| 54 | القمر | Al-Qamar | 55 | Makki | 27 |
| 55 | الرحمن | Ar-Rahman | 78 | Madani | 27 |
| 56 | الواقعة | Al-Waqi'ah | 96 | Makki | 27 |
| 57 | الحديد | Al-Hadid | 29 | Madani | 27 |
| 58 | المجادلة | Al-Mujadilah | 22 | Madani | 28 |
| 59 | الحشر | Al-Hashr | 24 | Madani | 28 |
| 60 | الممتحنة | Al-Mumtahanah | 13 | Madani | 28 |
| 61 | الصف | As-Saff | 14 | Madani | 28 |
| 62 | الجمعة | Al-Jumu'ah | 11 | Madani | 28 |
| 63 | المنافقون | Al-Munafiqun | 11 | Madani | 28 |
| 64 | التغابن | At-Taghabun | 18 | Madani | 28 |
| 65 | الطلاق | At-Talaq | 12 | Madani | 28 |
| 66 | التحريم | At-Tahrim | 12 | Madani | 28 |
| 67 | الملك | Al-Mulk | 30 | Makki | 29 |
| 68 | القلم | Al-Qalam | 52 | Makki | 29 |
| 69 | الحاقة | Al-Haqqah | 52 | Makki | 29 |
| 70 | المعارج | Al-Ma'arij | 44 | Makki | 29 |
| 71 | نوح | Nuh | 28 | Makki | 29 |
| 72 | الجن | Al-Jinn | 28 | Makki | 29 |
| 73 | المزمل | Al-Muzzammil | 20 | Makki | 29 |
| 74 | المدثر | Al-Muddaththir | 56 | Makki | 29 |
| 75 | القيامة | Al-Qiyamah | 40 | Makki | 29 |
| 76 | الإنسان | Al-Insan | 31 | Madani | 29 |
| 77 | المرسلات | Al-Mursalat | 50 | Makki | 29 |
| 78 | النبأ | An-Naba | 40 | Makki | 30 |
| 79 | النازعات | An-Nazi'at | 46 | Makki | 30 |
| 80 | عبس | Abasa | 42 | Makki | 30 |
| 81 | التكوير | At-Takwir | 29 | Makki | 30 |
| 82 | الانفطار | Al-Infitar | 19 | Makki | 30 |
| 83 | المطففين | Al-Mutaffifin | 36 | Makki | 30 |
| 84 | الانشقاق | Al-Inshiqaq | 25 | Makki | 30 |
| 85 | البروج | Al-Buruj | 22 | Makki | 30 |
| 86 | الطارق | At-Tariq | 17 | Makki | 30 |
| 87 | الأعلى | Al-A'la | 19 | Makki | 30 |
| 88 | الغاشية | Al-Ghashiyah | 26 | Makki | 30 |
| 89 | الفجر | Al-Fajr | 30 | Makki | 30 |
| 90 | البلد | Al-Balad | 20 | Makki | 30 |
| 91 | الشمس | Ash-Shams | 15 | Makki | 30 |
| 92 | الليل | Al-Layl | 21 | Makki | 30 |
| 93 | الضحى | Ad-Duhaa | 11 | Makki | 30 |
| 94 | الشرح | Ash-Sharh | 8 | Makki | 30 |
| 95 | التين | At-Tin | 8 | Makki | 30 |
| 96 | العلق | Al-Alaq | 19 | Makki | 30 |
| 97 | القدر | Al-Qadr | 5 | Makki | 30 |
| 98 | البينة | Al-Bayyinah | 8 | Madani | 30 |
| 99 | الزلزلة | Az-Zalzalah | 8 | Madani | 30 |
| 100 | العاديات | Al-Adiyat | 11 | Makki | 30 |
| 101 | القارعة | Al-Qari'ah | 11 | Makki | 30 |
| 102 | التكاثر | At-Takathur | 8 | Makki | 30 |
| 103 | العصر | Al-Asr | 3 | Makki | 30 |
| 104 | الهمزة | Al-Humazah | 9 | Makki | 30 |
| 105 | الفيل | Al-Fil | 5 | Makki | 30 |
| 106 | قريش | Quraysh | 4 | Makki | 30 |
| 107 | الماعون | Al-Ma'un | 7 | Makki | 30 |
| 108 | الكوثر | Al-Kawthar | 3 | Makki | 30 |
| 109 | الكافرون | Al-Kafirun | 6 | Makki | 30 |
| 110 | النصر | An-Nasr | 3 | Madani | 30 |
| 111 | المسد | Al-Masad | 5 | Makki | 30 |
| 112 | الإخلاص | Al-Ikhlas | 4 | Makki | 30 |
| 113 | الفلق | Al-Falaq | 5 | Makki | 30 |
| 114 | الناس | An-Nas | 6 | Makki | 30 |

### 20.2 Hadith Collection Hadith Counts

| Collection | Total (incl. repeats) | Unique | Verified in Database |
|-----------|----------------------|--------|---------------------|
| Sahih al-Bukhari | 7,275 | 2,602 | Yes |
| Sahih Muslim | 9,200 | 3,033 | Yes |
| Sunan Abi Dawud | 5,274 | 4,800 | Yes |
| Jami' al-Tirmidhi | 3,956 | 3,900 | Yes |
| Sunan al-Nasai | 5,761 | 5,270 | Yes |
| Sunan Ibn Majah | 4,341 | 4,000 | Yes |
| Musnad Ahmad | 27,000+ | 14,000+ | Partial |
| Muwatta Malik | 1,720 | 1,600 | Yes |

### 20.3 Narrator Reliability Terms (Arabic-English)

| Arabic | Transliteration | English Meaning |
|--------|-----------------|----------------|
| ثقة | Thiqah | Trustworthy, reliable |
| ثقة حافظ | Thiqah Hafiz | Trustworthy with strong memory |
| ثقة ثبت | Thiqah Thabt | Highly reliable, precise |
| صدوق | Saduq | Truthful, generally reliable |
| صدوق سيئ الحفظ | Saduq Sayy al-Hifz | Truthful but had poor memory |
| صدوق يخطئ | Saduq Yukhti | Truthful but sometimes erred |
| مقبول | Maqbul | Acceptable (when supported) |
| ضعيف | Daif | Weak |
| ضعيف الحديث | Daif al-Hadith | Weak in hadith narration |
| متروك الحديث | Matruk al-Hadith | Abandoned — narrations rejected |
| كذاب | Kadhdhab | Liar — fabricates hadith |
| مجهول | Majhul | Unknown — not identified |
| مجهول الحال | Majhul al-Hal | Unknown reliability |

### 20.4 Tafsir Works Reference

| Work | Author | Death AH | Methodology | Volumes |
|------|--------|----------|-------------|---------|
| Jami' al-Bayan | Al-Tabari | 310 | Historical-linguistic | 30+ |
| Al-Kashshaf | Al-Zamakhshari | 538 | Linguistic, Mu'tazili | 4 |
| Ma'alim al-Tanzil | Al-Baghawi | 516 | Traditional athar | 8 |
| Al-Muharrar al-Wajiz | Ibn Atiyyah | 541 | Comprehensive | 5 |
| Mafatih al-Ghayb | Al-Razi | 606 | Philosophical-theological | 32 |
| Anwar al-Tanzil | Al-Baydawi | 685 | Theological-linguistic | 5 |
| Tafsir al-Quran al-Azim | Ibn Kathir | 774 | Athar-based | 8 |
| Al-Bahr al-Muhit | Abu Hayyan | 745 | Grammatical | 10 |
| Al-Jami' li-Ahkam al-Quran | Al-Qurtubi | 671 | Fiqh-oriented | 22 |
| Fath al-Qadir | Al-Shawkani | 1255 | Comprehensive | 6 |
| Ruh al-Ma'ani | Al-Alusi | 1270 | Comprehensive | 30 |
| Taysir al-Karim al-Rahman | Al-Sa'di | 1376 | Contemporary accessible | 8 |
| Al-Tafsir al-Muyassar | Multiple scholars | 1430 | Simplified modern | 1 |
| Fi Zilal al-Quran | Sayyid Qutb | 1386 | Contemporary literary | 8 |
| Al-Tahrir wa al-Tanwir | Ibn Ashur | 1393 | Comprehensive modern | 30 |

### 20.5 Hadith Terminology (Arabic-English Glossary)

| Arabic | Transliteration | English |
|--------|-----------------|---------|
| صحيح | Sahih | Authentic |
| حسن | Hasan | Good |
| ضعيف | Daif | Weak |
| موضوع | Maudu | Fabricated |
| متواتر | Mutawatir | Mass-transmitted |
| آحاد | Ahad | Single/limited transmission |
| مشهور | Mashhur | Widely known |
| عزيز | Aziz | At least two narrators at each level |
| غريب | Gharib | Single narrator at some level |
| متصل | Muttasil | Continuous chain |
| منقطع | Munqati | Broken chain |
| مرسل | Mursal | Companion omitted |
| معلق | Mu'allaq | Beginning of chain missing |
| معضل | Mu'dal | Two or more missing |
| مسند | Musnad | Chain reaching the Prophet |
| عال | 'Ali | High chain (few narrators) |
| نازل | Nazil | Low chain (many narrators) |
| ثقة | Thiqah | Trustworthy |
| عدل | 'Adl | Upright, morally reliable |
| ضابط | Dabit | Precise, accurate |
| مدلس | Mudallis | Practices concealment |
| متابع | Mutabi' | Supporting narration |
| شاهد | Shahid | Corroborating witness |
| شاذ | Shadhdh | Irregular, contradicts stronger |
| منكر | Munkar | Rejected, contradicts reliable |
| علة | 'Illah | Hidden defect |
| ناسخ | Nasikh | Abrogating |
| منسوخ | Mansukh | Abrogated |

### 20.6 Fiqh Topics — Madhhab Mapping

| Topic | Hanafi | Maliki | Shafi'i | Hanbali |
|-------|--------|--------|---------|---------|
| Wudu — touch spouse | Does not break | Breaks | Breaks (with desire) | Does not break |
| Wudu — nosebleed | Breaks | Does not break | Breaks | Breaks |
| Tayammum — number of strikes | Two | One | Two | Two |
| Adhan — tathwib (as-salatu khayrun min al-nawm) | Yes (fajr) | Yes | Yes (fajr) | Yes (fajr) |
| Basmalah in prayer | Silent | Silent | Loud | Silent |
| Qunut in Fajr | No | No | Yes | No |
| Zakat — minimum nisab (gold) | 85g | 85g | 85g | 85g |
| Zakat al-Fitr — amount | 1 sa' | 1 sa' | 1 sa' | 1 sa' |
| Fasting — nullified by injection | No | Yes (nutritional) | Yes (nutritional) | Yes (nutritional) |
| Fasting — nullified by unintentional vomiting | Does not break | Breaks | Does not break | Does not break |
| Marriage — wali required | Not for adult woman | Yes | Yes | Yes |
| Marriage — mahr minimum | None specified | None specified | None specified | None specified |
| Divorce — triple talaq in one sitting | One talaq | Three talaq | Three talaq | Three talaq |
| Meat — People of the Book | Permitted | Permitted | Permitted | Permitted |
| Hunting with trained dogs | Permitted | Permitted | Permitted | Permitted |

### 20.7 Verification Error Codes

| Code | Error | Description | Action |
|------|-------|-------------|--------|
| Q-001 | Quran text mismatch | Text does not match mushaf | Block content |
| Q-002 | Invalid surah | Surah number out of range | Correct reference |
| Q-003 | Invalid ayah | Ayah number exceeds surah length | Correct reference |
| Q-004 | Missing diacritic | Quranic text missing tashkeel | Flag for correction |
| H-001 | Hadith not found | Reference not in database | Human review |
| H-002 | Chain mismatch | Chain does not match known variants | Human review |
| H-003 | Grade mismatch | Grade differs from database | Verify both |
| H-004 | Unknown collection | Collection not in approved list | Reject |
| S-001 | Scholar not found | Name not in database | Manual addition |
| S-002 | Work not attributed | Claim without scholar attribution | Flag for citation |
| S-003 | Invalid citation | Format does not match standards | Auto-correct |
| C-001 | Confidence too low | Below 60 threshold | Block or human review |
| C-002 | Evidence insufficient | Below minimum threshold | Flag for human review |
| I-001 | Ijma unverified | Consensus claim not verified | Human review |
| I-002 | Contradicts consensus | Claim contradicts documented ijma | Block |
| A-001 | Hallucination suspected | Pattern analysis flags potential | Immediate review |
| A-002 | Unverifiable claim | Source not in database | "Cannot verify" response |

---

## Document Control

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2026-07-10 | Chief Knowledge Factory Architect | Initial creation |

**Reviewers:** Senior Islamic Content Verification Officer
**Next Review Date:** 2026-10-10
**Status:** Published

---

## Connections

This document connects to:

1. **Knowledge_Factory/00_Knowledge_Factory_Bible** — Parent architecture for the Knowledge Factory
2. **Documentation/09_Verification** — General verification frameworks and protocols
3. **AI_Company/11_Workflows** — Enterprise workflow definitions for human-in-the-loop review
4. **Knowledge_Factory/02_Prompt_Engineering_Bible** — Prompt engineering guidelines for religious content
5. **Knowledge_Factory/03_Dataset_Bible** — Dataset specifications for religious content
6. **Knowledge_Factory/04_Testing_Bible** — Testing protocols for religious content verification
