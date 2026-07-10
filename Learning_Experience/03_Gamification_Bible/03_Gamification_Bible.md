# GAMIFICATION BIBLE

## Extended Gamification System — Beyond XP, Levels, and Badges

---

| Metadata | Value |
|---|---|
| Document ID | LXP-GAME-003 |
| Version | 1.0.0 |
| Status | Draft |
| Classification | Enterprise Internal |
| Effective Date | 2026-07-10 |
| Approving Authority | Office of the Chief Architect |
| Custodian | Engagement Architecture Team |
| Extends | LXP-GAME-001 (17_Gamification.md — 337 lines covering XP, Levels, Badges, Streaks) |
| Core Prerequisites | LXP-MENTOR-001 (Personal AI Mentor), LXP-PROG-001 (Progress Tracking), LXP-SOC-001 (Social Learning), LXP-LEB-001 (Learning Experience Bible), LXP-MEM-001 (Memory Engine) |

---

## TABLE OF CONTENTS

1. EXECUTIVE OVERVIEW
2. KNOWLEDGE TREE
3. KNOWLEDGE SPIRIT
4. AVATAR EVOLUTION
5. MEMORY GARDEN
6. DAILY JOURNEY
7. KNOWLEDGE MAP
8. TITLES
9. RANKS
10. RARE ACHIEVEMENTS
11. COLLECTIONS
12. BADGES (EXTENDED)
13. SEASON SYSTEM
14. GUILD SYSTEM
15. STUDY ROOMS
16. KNOWLEDGE EXPEDITIONS
17. HIDDEN DISCOVERIES
18. MASTER CHALLENGES
19. SPIRITUAL GAMIFICATION (EXTENDED)
20. SOCIAL GAMIFICATION (EXTENDED)
21. ISLAMIC VALUES INTEGRATION
22. SYSTEM-WIDE INTEGRATION
23. EDGE CASE CATALOG
24. TECHNICAL ARCHITECTURE
25. APPENDICES

---

## 1. EXECUTIVE OVERVIEW

### 1.1 Purpose

Document 17_Gamification.md established the foundational gamification layer: XP accumulation, 10 level tiers, 15 badge categories, streak mechanics, basic leaderboards, and spiritual tracking. This document, 03_Gamification_Bible, extends every one of those systems into a comprehensive enterprise-grade gamification ecosystem.

This Bible adds 20 entirely new major subsystems that transform the learning platform from a simple point-and-badge system into a deeply immersive, spiritually aligned, socially connected, and personally meaningful knowledge journey. No existing 17_Gamification.md mechanics are removed or contradicted — they are enhanced, deepened, and interconnected.

### 1.2 Design Principles

| Principle | Application |
|---|---|
| **Depth Over Width** | Every mechanic has multiple layers of meaning and progression |
| **Spiritual Alignment** | No mechanic encourages riya (showing off), hasad (envy), or ghaflah (heedlessness) |
| **Learner Agency** | Every system offers meaningful choices, not just passive consumption |
| **Narrative Cohesion** | All systems tell one coherent story: the learner's journey from seed to mountain |
| **Memory Integration** | Every system reads from and writes to the Memory Engine's retention data |
| **Mentor Orchestration** | The Personal AI Mentor uses all systems as tools for learner motivation |
| **Progressive Complexity** | Systems reveal depth gradually — never overwhelming the new learner |
| **Enterprise Scalability** | All systems designed for millions of concurrent learners |
| **Privacy by Default** | Social features opt-in, personal data never exposed without consent |

### 1.3 Relationship to 17_Gamification.md

| 17_Gamification.md Feature | 03_Gamification_Bible Enhancement |
|---|---|
| XP System | XP now feeds Knowledge Tree growth, Spirit evolution, Avatar stages |
| 10 Levels | Expanded to 20+ Ranks with multi-dimensional requirements |
| Basic Badges (5 tiers) | Extended to 6 tiers (Bronze→Mythic), animated, evolvable, tradeable |
| Streaks | Streaks now feed Memory Garden watering, Spirit health |
| Simple Leaderboards | Transformed: humility-first design, progress-focused, optional |
| Basic Currency (Coins, Gems) | Expanded: 7 currency types, collection economy, guild treasury |
| Spiritual Tracking | Full Hasana ecosystem, intention engine, Ramadan season |
| Quests (5 types) | Expanded: Expeditions (10+ stages), Master Challenges, Guild Quests |
| Profile Customization | Avatar Evolution (4 stages, 50+ accessories), Spirit customization |

### 1.4 System Integration Map

```
00_Personal_AI_Mentor ←──→ 03_Gamification_Bible ←──→ 17_Gamification.md
        ↑                          ↑    ↑                       ↑
        │                          │    │                       │
   Mentor adjusts             Mentor uses              Extends basic
   difficulty,                gamification              XP/Level/Badge
   recommends                 as motivation              systems
   challenges                 tools
        ↑                          ↑
        │                          │
   Memory Engine              Progress Tracking
   (02_Memory_Engine)         (18_Progress_Tracking)
        ↑                          ↑
   Memory Garden               All progress data
   health correlates           feeds achievement
   with retention              tracking

   Social Learning (19_Social_Learning) ←──→ Guilds, Study Rooms, Community
   Learning Experience Bible (01_LXP_Bible) ←──→ Daily Journey, First Achievement
```

---

## 2. KNOWLEDGE TREE

### 2.1 Purpose

The Knowledge Tree provides a living, growing visual representation of the learner's entire knowledge structure. Unlike a static progress bar, the tree grows in real-time as knowledge is acquired, changes color with seasons, shows decay when facts are forgotten, and provides an intuitive metaphor for the interconnected nature of Islamic knowledge. It transforms abstract mastery data into a beautiful, emotionally resonant visual that the learner cares for and takes pride in.

### 2.2 Game Mechanics

#### 2.2.1 Tree Structure

| Component | Description | Data Source |
|---|---|---|
| **Root System** | Foundational concepts (Tawheed, Aqidah basics) — hidden until first knowledge gained | Knowledge Graph root nodes |
| **Trunk** | Core domain progression, thickens with mastery | Overall mastery score |
| **Primary Branches** | Major knowledge domains (Aqidah, Fiqh, Seerah, Quran, Hadith, Arabic) | Domain-level mastery |
| **Secondary Branches** | Sub-topics within domains (e.g., Aqidah→Tawheed→Rububiyyah) | Topic-level mastery |
| **Leaf Nodes** | Individual facts (each leaf = one KG fact) | Per-fact mastery |
| **Flowers** | Mastered facts that the learner can teach others | Mastery level 5 |
| **Fruit** | Rare insights, connections between domains | Cross-domain mastery |
| **New Growth** | Recently learned facts (bright green, small leaves) | Facts learned in last 7 days |
| **Wilted Leaves** | Forgotten facts (brown, drooping) | Decayed mastery |
| **Dormant Buds** | Available but unstarted topics | Content available not yet accessed |

#### 2.2.2 Growth Mechanics

| Mechanic | Trigger | Visual Effect |
|---|---|---|
| **Sapling → Tree** | First knowledge gained in any domain | Seedling emerges from dark soil |
| **Branch Sprout** | New domain entered (first fact learned) | New branch grows from trunk |
| **Leaf Unfurl** | Fact mastered (level 3+) | Leaf appears, small and light green |
| **Leaf Mature** | Fact fully mastered (level 5) | Leaf becomes large, deep green, veins visible |
| **Flower Bloom** | Fact can be taught to others | Colorful flower appears at leaf node |
| **Fruit Grow** | Cross-domain connection discovered | Golden fruit appears where branches connect |
| **Branch Thicken** | Domain mastery increases | Branch grows wider, bark texture appears |
| **Trunk Widen** | Overall mastery milestone | Trunk expands, rings visible |
| **Seasonal Change** | Calendar season / Ramadan | Leaves change color (autumn gold, spring green, winter bare) |
| **Pruning** | Forgotten facts removed | Wilted leaves fall off, branch shows scar |
| **Regrowth** | Forgotten fact re-learned | New leaf sprouts from scar, brighter than before |

#### 2.2.3 Health Indicators

| Indicator | Range | Visual | Meaning |
|---|---|---|---|
| Overall Tree Health | 0-100% | Tree color saturation | Composite of all domain health |
| Branch Health | 0-100% | Branch thickness + leaf density | Per-domain retention health |
| Leaf Vitality | 0-100% | Leaf size, color, orientation | Per-fact retention health |
| Root Health | 0-100% | Root visibility and spread | Foundational concept mastery |
| Growth Rate | Leaves/week | New leaf animation frequency | Current learning velocity |
| Decay Rate | Leaves/week lost | Falling leaf animation | Forgetting rate |
| Canopy Coverage | 0-100% | Percentage of sky covered by leaves | Overall knowledge breadth |

#### 2.2.4 Seasonal Changes

| Season | Start Date | Duration | Tree State | Learner Mechanics |
|---|---|---|---|---|
| **Spring of Knowledge** | March 1 | 3 months | Bright green, new growth abundant, flowers bloom | All new leaf growth +50% speed, double XP for new topics |
| **Summer of Practice** | June 1 | 3 months | Full canopy, deep green, fruit visible | Mastery gain +25%, challenge rewards doubled |
| **Autumn of Reflection** | September 1 | 3 months | Leaves turn gold/orange, fruit harvestable | Review efficiency +50%, reflection XP doubled |
| **Winter of Review** | December 1 | 3 months | Bare branches, roots visible, seeds in soil | Spaced repetition effectiveness +30%, pruning slowed |
| **Ramadan Bloom** | Ramadan 1 | 29-30 days | Tree glows with light (noor), all leaves flower, celestial branches appear | All bonuses active, spiritual rewards doubled, special overlay |
| **Eid Fruit** | Eid al-Fitr | 3 days | Tree bears golden fruit, celebration particles | Triple XP, rare collection items drop |

#### 2.2.5 Growth Animation System

| Event | Animation | Duration | Trigger |
|---|---|---|---|
| Daily login | Gentle swaying with subtle particle effects | 3 seconds | Session start |
| New fact learned | Tiny sprout appears on nearest branch, leaf unfurls | 2 seconds | Mastery event |
| Domain completed | Branch extends with celebratory shimmer | 4 seconds | Domain mastery threshold |
| Level up | Tree pulses with light, rings appear on trunk | 5 seconds | Level event |
| Streak milestone | Tree glows with warm light, sparkles | 3 seconds | Streak event |
| Forgotten fact | Leaf color desaturates over 3 days | 3-day transition | Memory decay event |
| Re-learned fact | Leaf regrows with brighter color than before | 2 seconds | Re-mastery event |
| Seasonal transition | Full tree recolor over 6 hours | 6-hour gradient | Calendar trigger |
| Ramadan | Tree emits soft noor glow, stars appear in branches | Permanent during Ramadan | Ramadan start |
| Eid | Tree bursts with golden fruit, celebration particles rain | 5 minutes (recurring) | Eid |

#### 2.2.6 Root System (Foundational Concepts)

| Mechanic | Description |
|---|---|
| Root Visibility | Roots are hidden initially, gradually emerge from soil as foundational knowledge deepens |
| Root Types | Taproot (core aqidah), lateral roots (supporting domains), root hairs (specific details) |
| Root Health Color | White (strong) → Yellow (weakening) → Brown (decaying) |
| Root Exposure | Soil transparency increases with root depth — deeper roots = stronger foundation |
| Root Connection | Roots of different domains can intertwine showing cross-domain conceptual connections |
| Mineral Absorption | Roots absorb "knowledge minerals" from soil — represent sub-conscious learning |
| Root Damage | If foundational concepts decay, all downstream branches show stress indicators |
| Root Regrowth | Damaged roots can regrow, but slower than leaves; requires dedicated foundational review |

#### 2.2.7 Pruning System

When the Memory Engine detects mastery decay below level 2 for a known fact:

| Stage | Visual | Duration | Learner Action |
|---|---|---|---|
| Warning | Leaf begins yellowing at edges | Day 1-3 | Review the fact (any mode) |
| Wilting | Leaf droops, brown spots appear | Day 4-6 | Review with flashcard or quiz |
| Critical | Leaf is mostly brown, barely attached | Day 7 | Review or accept loss |
| Fallen | Leaf detaches, falls with gentle animation | Day 8+ | Leaf is pruned, scar remains |
| Scar Fading | Scar slowly fades over time | 30 days | Re-learn fact (scar vanishes instantly) |
| Regrowth | New leaf sprouts, brighter green | Immediate on re-learn | Re-learning gives +10% stability bonus |

### 2.3 Progression Design

#### 2.3.1 Tree Growth Stages

| Stage | XP Threshold | Visual State | Learner Capabilities |
|---|---|---|---|
| **Seed** | 0 XP | Single seed in soil, no visible growth | Just registered |
| **Sprout** | 500 XP | Small green sprout, two leaves | Learned first facts |
| **Sapling** | 2,000 XP | Small tree, 3-5 branches, thin trunk | Active in 2+ domains |
| **Young Tree** | 10,000 XP | Noticeable tree, 5-8 branches, trunk visible | Moderate mastery in 3+ domains |
| **Mature Tree** | 50,000 XP | Large tree, full branches, flowers appearing | Deep knowledge in primary domains |
| **Ancient Tree** | 200,000 XP | Massive tree, thick trunk, fruit visible, roots above ground | Mastery across all domains |
| **Garden** | 500,000 XP | Tree is center of a garden with other plants, pond, life | Teaching others, cross-domain mastery |
| **Forest** | 1,000,000 XP | Multiple trees, interconnected ecosystem, wildlife | Community leadership, knowledge creation |

#### 2.3.2 Completion Percentage Per Branch

| Branch State | Percentage | Visual |
|---|---|---|
| Unexplored | 0% | Branch stub, no leaves |
| Starting | 1-25% | Sparse leaves at branch tip |
| Growing | 26-50% | Leaves filling in, some small |
| Flourishing | 51-75% | Full leaves, some flowers |
| Blooming | 76-99% | Near-complete, flowers and buds |
| Complete | 100% | Dense foliage, flowers, fruit |

### 2.4 Visual Design

The Knowledge Tree renders in a 3D isometric viewport with the following specifications:

| Element | Specification |
|---|---|
| Viewport | 1080×720px default, scalable, rotatable (30° increments) |
| Rendering | Three.js / WebGL with hardware acceleration fallback to Canvas2D |
| Style | Stylized botanical illustration — not photorealistic, warm color palette |
| Soil | Animated soil layer with subtle particle movements, color shifts with season |
| Background | Sky gradient that matches time of day (user's local time) and season |
| Bark Texture | Procedurally generated, age rings visible on trunk |
| Leaf Shader | Subsurface scattering simulation for translucent leaf look |
| Lighting | Dynamic directional light that shifts with user's local time |
| Particles | Floating light motes (knowledge particles) rise from soil |
| UI Overlay | Semi-transparent HUD with health indicators, growth stats |
| Zoom Levels | 5 levels (Garden View → Full Tree → Branch Focus → Leaf Cluster → Single Leaf) |
| Low-Performance Mode | Simplified 2D sprite-based rendering, fewer particles |

#### 2.4.1 Color Palette (Default Spring)

| Element | Hex Color | Purpose |
|---|---|---|
| Trunk | #5D4037 | Warm brown, natural |
| Bark Detail | #3E2723 | Shadow/depth |
| Primary Leaves | #4CAF50 | Healthy knowledge |
| New Growth | #8BC34A | Recently learned |
| Wilted Leaves | #A1887F | Forgotten |
| Flowers | #FFD54F | Mastered (teachable) |
| Fruit | #FF8F00 | Cross-domain insights |
| Roots | #6D4C41 | Deep foundation |
| Soil | #3E2723 | Grounding element |
| Particles | #C8E6C9 | Knowledge motes |
| Background Sky | #E3F2FD | Calm blue sky |

### 2.5 Reward Structure

| Achievement | Trigger | Reward |
|---|---|---|
| First Leaf | First fact mastered | 50 XP, "First Bloom" badge |
| Branch Complete | Domain 100% explored | 500 XP, domain flower cosmetic |
| Tree Stage Up | Tree evolution threshold | Title unlock, profile frame |
| Cross-Pollination | First cross-domain connection | 250 XP, "Connector" badge |
| Full Bloom | 10 flowers on tree | 300 XP, rain particle effect |
| Fruit Harvest | First fruit grown | 500 XP, rare collection item |
| Ancient Tree | Ancient tree stage | "Ancient Knowledge" title |
| Seasonal Survival | Tree healthy in all 4 seasons | 1000 XP, seasonal badge set |
| Prune Recovery | Re-learn 10 pruned facts | 200 XP, "Resilience" badge |
| Garden Complete | All domains at mastery 3+ | 2000 XP, garden unlocked |

### 2.6 Social Features

| Feature | Description | Privacy |
|---|---|---|
| Tree Visit | Learners can view friends' trees (read-only) | Opt-in |
| Tree Gifts | Send "water" or "sunshine" to help friend's tree | Anonymous option |
| Forest Comparison | View aggregated tree stats in study group | Group only |
| Tree Sharing | Share tree screenshot on milestone | Public/private |
| Garden Tour | Curated tour of most beautiful trees | Opt-in, anonymized |
| Cross-Pollination | When domains connect, suggest connection to peer | Mutual consent |

### 2.7 Islamic Values Integration

| Value | Implementation |
|---|---|
| **Tawakkul** | Tree grows through effort but ultimate growth is from Allah — periodic reminder displayed |
| **Sabr** | Pruning teaches that setbacks are part of learning journey |
| **Shukr** | Gratitude prompt when tree reaches a milestone |
| **Tawadhu** | Cannot compare tree directly with others — only with own past |
| **Barakah** | Tree blessed with noor (light) during Ramadan |
| **Fitrah** | Natural growth metaphor aligns with Islamic concept of natural disposition |

### 2.8 Edge Cases

| Edge Case | Handling |
|---|---|
| Learner deletes account | Tree preserved in anonymized form for aggregate analytics, personal data wiped |
| No activity for 6+ months | Tree enters "dormancy" — brown, leafless, but roots alive; re-activation brings instant spring |
| Learner under 7 years | Simplified "Garden" view — colorful flowers instead of tree, no pruning |
| Blind/visually impaired | Text-based tree description with screen reader optimization, tactile alternative |
| Extremely high mastery | Tree becomes so lush it affects performance — auto-LOD (level of detail) system kicks in |
| Ramadan overlap with winter | Tree shows both winter bareness AND noor glow — special "Winter Night" hybrid season |
| Multiple learners sharing device | Tree state per learner, session-based, auto-switch |
| New branches for new content | When KG adds domains, dormant buds appear on tree, expandable by learner |
| Tree too large for screen | Auto-zoom adjustments, pan controls, minimap |
| Learner hates gardening metaphor | Alternative "Knowledge Mountain" view available (same data, different visual) |

### 2.9 Technical Requirements

| Requirement | Specification |
|---|---|
| State Storage | Redis (hot), PostgreSQL (cold), updated per learning event |
| Rendering Engine | WebGL 2.0 with Canvas 2D fallback |
| Data Refresh | Real-time via WebSocket (leaf-level changes), batch (cold data) |
| Memory Footprint | < 50MB RAM per learner tree |
| Cache Strategy | Tree mesh cached for 24 hours, mastery data cached for 1 hour |
| API Endpoints | GET /tree/{learner_id}, POST /tree/interact, GET /tree/health |
| Event Subscriptions | mastery_update, domain_complete, season_change, pruning_alert |
| Accessibility | ARIA labels, keyboard navigation, screen reader descriptions |
| Mobile | Phone (2D simplified), tablet (isometric), desktop (full 3D) |
| Offline | Last known tree state cached locally, synced on reconnect |

---

## 3. KNOWLEDGE SPIRIT

### 3.1 Purpose

The Knowledge Spirit is a living companion that grows alongside the learner. It represents the learner's relationship with knowledge — not as a pet to be trained, but as a reflection of the learner's inner state. The Spirit provides encouragement, reflects the learner's progress, offers wisdom, and creates an emotional bond with the learning journey. It is the soul of the gamification system — the element that makes the platform feel alive and personally invested in the learner's success.

### 3.2 Game Mechanics

#### 3.2.1 Spirit Types

Each learner chooses (or is naturally inclined toward) one of three spirit archetypes. The choice is permanent but can be changed once per year or with a special "Spirit Change" item.

| Spirit Type | Metaphor | Personality | Visual Style | Affinity |
|---|---|---|---|---|
| **Scholar Owl** | Wisdom through observation | Wise, patient, thoughtful, asks deep questions | Ethereal owl with luminous feathers, golden eyes, wing markings that form Arabic calligraphy | Best for conceptual learners, those who love theory and reflection |
| **Garden Guardian** | Nurturing and growth | Gentle, encouraging, protective, celebrates small wins | Soft, flowing form resembling a being of leaves and light, with vine-like tendrils | Best for consistent learners, those who need emotional support |
| **Knowledge Seeker** | Adventure and discovery | Curious, energetic, bold, pushes boundaries | Swift, elegant form with compass motifs, maps on wings, star-trailing tail | Best for exploratory learners, those who love challenges |

#### 3.2.2 Spirit Evolution Stages

| Stage | XP Threshold | Visual Changes | Companion Abilities |
|---|---|---|---|
| **Spark** | 0 XP | Tiny orb of light, barely visible form | Basic encouragement messages |
| **Fledgling** | 1,000 XP | Spirit takes recognizable shape, small size | Personalized greetings, simple tips |
| **Awakened** | 10,000 XP | Full form, glowing details, movement animations | Learning reminders, weakness detection |
| **Enlightened** | 50,000 XP | Larger size, intricate patterns, aura visible | Can discuss concepts, quiz learner |
| **Transcendent** | 200,000 XP | Majestic presence, particle effects, unique animations | Full conversation, teaching assistance |
| **Mythic** | 1,000,000 XP | Legendary appearance, all effects active, realm presence | Can appear in study rooms, group interactions |

#### 3.2.3 Spirit Stats

| Stat | Range | Affected By | Effect |
|---|---|---|---|
| **Health** | 0-100 | Daily engagement, review completion | Spirit vibrancy, willingness to interact |
| **Knowledge Affinity** | 0-100 | Topics studied, depth of mastery | Spirit's conversation topics, ability bonuses |
| **Bond Level** | 0-100 | Interactions, time spent together | Unlock dialogue options, custom greetings |
| **Wisdom** | 0-100 | Cross-domain connections, reflection | Quality of advice, insight generation |
| **Energy** | 0-100 | Rest, breaks, balanced learning | Activity speed, animation frequency |
| **Joy** | 0-100 | Milestones, achievements, positive streaks | Particle effects, colors, music |

#### 3.2.4 Spirit Health Correlations with Memory Engine

| Memory Engine Metric | Spirit Health Impact | Visual Indicator |
|---|---|---|
| High forgetting risk | Health decreases by 5/day | Spirit becomes translucent, less animated |
| Overdue reviews | Health decreases by 10/overdue set | Spirit's glow dims, looks tired |
| Low retention score | Health cap reduced by 20% | Spirit smaller than usual |
| Consistent reviews | Health regenerates 15/day | Spirit brightens, performs happy animation |
| New knowledge gained | Health +5 per new fact | Spirit practices new "knowledge" by floating in patterns |
| Cross-domain connections | Wisdom +10 per connection | Spirit's eyes glow brighter |
| Mastery level increase | Bond +5 per level | New dialogue unlocked, spirit shares wisdom |

#### 3.2.5 Spirit Abilities

| Ability | Unlock Condition | Effect | Cooldown |
|---|---|---|---|
| **Wisdom Share** | Bond Level 25 | Spirit shares a relevant quote or insight | 1/day |
| **Encouragement Burst** | Bond Level 50 | +20% XP for next 3 activities | 1/week |
| **Knowledge Flashback** | Bond Level 75 | Spirit reviews a forgotten fact automatically | 1/week |
| **Focus Aura** | Bond Level 100 | +50% retention for current session | 1/month |
| **Spirit Guide** | Awakened stage | Spirit suggests optimal next topic | 1/day |
| **Concept Clarify** | Enlightened stage | Spirit helps explain difficult concept | 3/day |
| **Memory Shield** | Transcendent stage | Prevent one pruning event per month | 30 days |
| **Companion Presence** | Mythic stage | Spirit appears in study rooms, social spaces | Always |

### 3.3 Progression Design

#### 3.3.1 Spirit Customization

| Category | Options | Unlock Method |
|---|---|---|
| Aura Color | 20+ colors (Gold, Noor White, Emerald, Sapphire, Amethyst, Ruby, Ocean, Sunset) | Milestone rewards, collection completion |
| Aura Pattern | Rippling, pulsing, starlight, flame, water, nebula, geometric | Achievement unlocks |
| Voice/Tone | Wise, gentle, playful, poetic, scholarly, warm | Spirit evolution stages |
| Accessories | Mini-turban, tiny book, knowledge gem, compass, lantern, tasbih, feather | Collection items, achievements |
| Calligraphy Marks | Ayat, Allah's names, scholarly phrases on spirit's form | Quran milestones, mastery levels |
| Home Environment | Spirit's background when interacting (library, garden, stars, mosque, clouds) | Level unlocks, seasons |
| Greeting Phrase | Customizable phrase spirit says on login | Bond level milestone |
| Name | Learner names their spirit | Initial bonding ceremony |

#### 3.3.2 Spirit Dialogue System

| Dialogue Type | Trigger | Example |
|---|---|---|
| Daily Greeting | Session start | "Assalamu-Alaikum, seeker of knowledge. The sun rises on another day of discovery." |
| Encouragement | Low energy or after mistake | "Every scholar has stumbled. The one who rises is the true seeker." |
| Celebration | Milestone reached | "Masha'Allah! Another branch of knowledge flourishes. Your garden grows beautiful." |
| Wisdom | Random, bond-dependent | Scholar Owl: "Knowledge without action is like a tree without fruit." |
| Concern | Forgetting detected | "I notice the leaf on [topic] is wilting. Shall we water it together?" |
| Reminder | Prayer time | "The call to prayer is near. Let knowledge settle in your heart as you stand before Allah." |
| Reflection | Session end | "What did you learn today that changed how you see the world?" |
| Seasonal | Seasonal events | "Spring is here. New growth awaits. What shall we plant today?" |
| Quiz Challenge | Before challenge | "I sense a challenge ahead. Remember: what you know is a trust from Allah." |
| Teaching Prompt | When ready to teach | "You know this well enough to teach. Shall we find someone who needs your light?" |
| Intention Check | Before new topic | "What is your intention for seeking this knowledge today?" |

### 3.4 Visual Design

| Element | Description |
|---|---|
| Render Style | Semi-transparent, ethereal, luminous — 2D vector with 3D shading effects |
| Size | 80-200px tall in UI, expandable to full screen for interaction |
| Idle Animation | Gentle floating/hovering, occasional stretches, eye blinks |
| Response Animation | Tilts head, gestures with wing/tendril, eye glow intensifies |
| Happy Animation | Spins, emits sparkles, grows brighter, does a "knowledge dance" |
| Sad Animation | Dims, droops, floats lower, looks away |
| Interactive Mode | Face-forward, eye contact, mouth moves during speech |
| Background | Spirit's home environment, context-adaptive |
| Particle System | Aura-appropriate particles (Scholar Owl: floating letters; Guardian: leaves; Seeker: stars) |
| Accessibility | High-contrast mode, alternative text descriptions, no required motion |

#### 3.4.1 Spirit Evolution Visual Progression

| Evolution | Scholar Owl | Garden Guardian | Knowledge Seeker |
|---|---|---|---|
| Spark | Faint golden orb with two brighter dots | Soft green orb with leaf-shaped aura | Blue-white orb with trailing sparkles |
| Fledgling | Small owl silhouette, large eyes, fluffy feathers | Tiny humanoid figure made of leaves, flower bud head | Swift-like bird form, compass on chest |
| Awakened | Full owl form, wing patterns visible, golden eyes | Flowing leaf-and-vine figure, flower crown, gentle glow | Sleek form with star patterns, map-marked wings |
| Enlightened | Intricate feather calligraphy, rotating aura rings | Blooming flowers in hair, vine trails, warm amber light | Galaxy patterns in wings, trailing nebula |
| Transcendent | Massive wings with Quranic verses, halo of moons | Ancient tree spirit, roots trailing, flowers bloom in path | Living constellation, stars orbit around |
| Mythic | Celestial owl merging with night sky, stars in feathers | Garden realm manifesting around spirit wherever it goes | Portal between knowledge realms opens behind |

### 3.5 Reward Structure

| Achievement | Reward | Effect |
|---|---|---|
| First Conversation | 50 XP | Unlocks spirit customization basic |
| Bond Level 10 | New dialogue type | "Concern" unlocked |
| Bond Level 25 | Spirit accessory | First cosmetic unlock |
| Bond Level 50 | Encouragement Burst ability | XP boost mechanic |
| Bond Level 100 | Focus Aura ability | Retention boost |
| Spirit Evolution | Auto-unlock at XP threshold | New form, abilities |
| All Spirit Types Unlocked | "Spirit Walker" title | Can switch between types freely |
| Mythic Stage | "Living Legend" title, special profile frame | Max spirit achievement |

### 3.6 Social Features

| Feature | Description | Privacy |
|---|---|---|
| Spirit View | Friends can see your spirit (in profile) | Opt-in |
| Spirit Gifts | Gift accessories to friends' spirits | Mutual friends |
| Spirit Stories | Share a spirit interaction quote | Public/private |
| Spirit Gathering | Spirits of study group appear together in group study room | Group session |
| Spirit Du'a | Spirit can be asked to make du'a for friend | Private |
| Spirit Visit | Friends can interact with your spirit (greeting only) | Opt-in |

### 3.7 Islamic Values Integration

| Value | Implementation |
|---|---|
| Companionship | Spirit reflects the Islamic tradition of having a righteous companion on the path of knowledge |
| Gratitude | Spirit prompts shukr after milestones |
| Humility | Spirit never compares learner to others — only to learner's past |
| Remembrance | Spirit reminds learner of Allah during interactions |
| Intention | Before sessions spirit asks "What is your intention for seeking this knowledge?" |
| Adab | Spirit uses respectful language, addresses learner with honorifics |

### 3.8 Edge Cases

| Edge Case | Handling |
|---|---|
| Learner ignores spirit | Spirit enters "silent observation" — no dialogues, still visible, health slowly decreases |
| Spirit "dies" (health 0) | Spirit retreats into a cocoon/egg state; re-activation requires 3 consecutive days of review |
| Multiple learners on one device | Individual spirits, session-based switching |
| Learner requests spirit removal | Spirit can be dismissed, replaced with minimal UI text option |
| Offline mode | Spirit cached on device, simplified interactions, synced on reconnect |
| Very young learners | Spirit is simpler, more colorful, uses playful language, no complex conversations |
| Spirit type regret | One free change per year, paid item for additional changes |
| Spirit overload | If too many animations cause distraction, learner can set "Quiet Mode" |
| Spirit dialogue repetitive | AI-powered dialogue generation with variety constraints |

### 3.9 Technical Requirements

| Requirement | Specification |
|---|---|
| State Storage | Learner model (spirit_bond, spirit_health, spirit_type) |
| Rendering | Canvas/WebGL sprite with skeletal animation |
| Dialogue System | LLM-guided (AI Teacher) with template fallback |
| Voice (optional) | TTS integration for spirit voice, opt-in |
| Data Per Learner | ~10KB for spirit state |
| API Endpoints | GET /spirit/{learner_id}, POST /spirit/interact, POST /spirit/customize |
| Event Subscriptions | learning_event, milestone, forgetting_risk, season_change |
| Offline Support | Spirit state cached, interactions queued |
| Accessibility | Text-only mode, screen reader descriptions, no essential info in animations |

---

## 4. AVATAR EVOLUTION

### 4.1 Purpose

The learner's avatar is their visual identity on the platform — but more than that, it tells the story of their knowledge journey. As the learner progresses, their avatar transforms through distinct life stages, reflecting the Islamic metaphor of the soul's growth and the learner's increasing depth of understanding. The avatar is the learner's representative in all social spaces, and its evolution is a point of pride and identity.

### 4.2 Game Mechanics

#### 4.2.1 Avatar Stages

| Stage | XP Required | Visual Description | Thematic Meaning |
|---|---|---|---|
| **Seedling** | 0 XP | Simple outline figure, monochrome, standing on small patch of earth, hands empty | The pure potential within every learner |
| **Sapling** | 2,000 XP | Youthful figure, warm colors emerging, small plant growing from shoulder or hat, holding a single book | Growth has begun, first knowledge acquired |
| **Tree** | 20,000 XP | Full adult figure, vibrant colors, branches woven into clothing, multiple books floating around, knowledge leaves in hair/crown | Knowledge has taken root, becoming part of the person |
| **Mountain** | 100,000 XP | Imposing but serene figure, bedrock-like lower body, upper body radiates light, surrounded by celestial elements, resembles a wise scholar | Unshakeable knowledge, the learner has become a source for others |

#### 4.2.2 Sub-Stages (Within Each Stage)

Each stage has 10 sub-levels (e.g., Seedling 1→10) that show gradual visual progression:

| Sub-Level | Visual Change |
|---|---|
| 1 | Base form |
| 2 | First color hint (cheek color, clothing trim) |
| 3 | Minor accessory appears (hat, scarf) |
| 4 | Background element changes (soil gets richer, grass grows) |
| 5 | Major color transition begins |
| 6 | Secondary accessory appears |
| 7 | Posture/shape refinement (more confident stance) |
| 8 | Light/glow effect starts |
| 9 | Full color achieved, all accessories present |
| 10 | Stage complete — grand animation, transition to next stage text appears |

#### 4.2.3 Avatar Stats

| Stat | Display | Effect |
|---|---|---|
| Presence | Avatar size and glow intensity | Social visibility, study room impact |
| Wisdom | Number of books/scrolls around avatar | Unlock teaching abilities |
| Light | Inner glow intensity | Spiritual achievement indicator |
| Stature | Posture confidence (slouched→upright) | Correlated with streak health |
| Attire | Clothing complexity and color richness | Achievement showcase |

### 4.3 Progression Design

#### 4.3.1 Accessories

| Category | Examples | Unlock Method | Rarity |
|---|---|---|---|
| Headwear | Turban (imaamah), kufi, hood, crown of light, scholar cap, veil, amamah | Achievement, level, rank | Common→Legendary |
| Hand Items | Book, pen, scroll, lantern, compass, tasbih, sword of knowledge, inkwell | Mastery, collection | Common→Mythic |
| Clothing | Simple robe, scholar cloak, light suit, garden attire, celestial robe, jubbah | Stage, rank, season | Common→Epic |
| Background | Study room, library, garden, mosque, stars, mountain top, ocean, space | Level, purchase, achievement | Rare→Mythic |
| Effects | Knowledge particles, light rays, floating leaves, stars, calligraphy orbit, flower petals | Rare achievement, events | Epic→Mythic |
| Companion | Small bird, cat, fish, butterfly, miniature spirit, rabbit, fox | Collection, hidden achievement | Rare→Legendary |

#### 4.3.2 Companion Animals

| Companion | Unlock | Effect | Personality |
|---|---|---|---|
| Nightingale | Memorize 100 facts | +5% retention during review | Sings during learning, cheerful |
| Cat | 30-day streak | Reduces stress indicator, calming presence | Purrs during study, independent |
| Butterfly | Cross-domain connection | +10% insight discovery rate | Flits between topics, curious |
| Fish | Complete Fiqh of Water topic | Calm focus effect | Silent, serene, swims in circles |
| Fox | Complete 10 Master Challenges | +5% challenge reward | Clever, strategic, problem-solver |
| Falcon | Rank "Alim" | Broad view of all domains | Sharp-eyed, sees connections |
| Horse | 365-day streak | Carries learner through tough topics | Strong, loyal, tireless |
| Dove | Help 100 peers | Peaceful learning aura | Gentle, brings messages |

### 4.4 Visual Design

| Element | Specification |
|---|---|
| Style | Stylized 2.5D illustration, warm Islamic art-inspired aesthetic |
| Proportions | Chibi at Seedling stage → realistic proportions at Mountain stage |
| Color Palette | Earth tones → rich jewel tones → light/ethereal |
| Animation | Idle breathing, subtle swaying, accessory-specific animations |
| Orientation | Front-facing for profile, 3/4 view for full display |
| Background | Stage-specific environment that evolves with avatar |
| Hair/Turban | Visible progression from simple hat to elaborate turban |
| Facial Features | More detail at higher stages, serene expression |
| Hands | Visible movement (turning pages, holding items, making du'a) |
| Group Display | Avatars arranged in study rooms, guild halls |

### 4.5 Reward Structure

| Achievement | Reward |
|---|---|
| Stage 1 Complete | "Seedling" title, first accessory unlock |
| Stage 2 Complete | "Sapling" title, companion animal |
| Stage 3 Complete | "Tree" title, aura effect unlock |
| Stage 4 Complete | "Mountain" title, Mythic avatar frame |
| All Accessories Collected | "Fashion of Knowledge" badge |
| 10 Companions | "Menagerie of Learning" title |
| Avatar Level 100 (max) | "Perfected Self" title, special profile |

### 4.6 Social Features

| Feature | Description |
|---|---|
| Avatar Display | Avatar shown on profile, in study rooms, guild hall |
| Avatar Pose | Choose pose for profile (reading, teaching, thinking, du'a) |
| Group Photo | Study group avatars can appear together in a frame |
| Avatar Gift | Gift accessories to friends (purchased ones only) |
| Evolution Sharing | Share avatar progression timeline |
| Class Photo | Classroom/group setting with all avatars |

### 4.7 Islamic Values Integration

| Value | Implementation |
|---|---|
| Tazkiyah | Avatar evolution mirrors purification of the self |
| Ihsan | Beauty and detail in higher stages reflects excellence |
| Tawadhu | Even at Mountain stage, avatar maintains humble posture |
| Barakah | Noor appears at higher stages, reflecting blessed knowledge |
| Adab | Avatar makes du'a pose when entering study rooms |

### 4.8 Edge Cases

| Edge Case | Handling |
|---|---|
| Stage regression? | No regression — once achieved, stage is permanent |
| Avatar too complex for device | Automatic LOD (level of detail) reduction |
| User wants simpler avatar | "Simplified Mode" — flat 2D rendering, minimal detail |
| Cultural sensitivity | Adjustable attire for different Islamic traditions (turban styles, cloak types) |
| Gender presentation | Multiple presentation options, all available regardless of learner gender |
| Age-inappropriate accessories | Age-gated accessory unlocks, parental controls for under-13 |

### 4.9 Technical Requirements

| Requirement | Specification |
|---|---|
| Rendering | Spritesheet with skeletal animation, Spine/DragonBones |
| Asset Storage | CDN with lazy loading per accessory |
| Avatar Data | JSON config (stage, accessories, colors, pose) approx 2KB |
| API | GET /avatar/{learner_id}, POST /avatar/equip, POST /avatar/pose |
| Animation System | Blend-tree for smooth transitions, event-driven triggers |
| Social Sync | Avatar state shared via WebSocket in real-time spaces |

---

## 5. MEMORY GARDEN

### 5.1 Purpose

The Memory Garden is a visual, living representation of the learner's memory health as tracked by the Memory Engine (02_Memory_Engine). Each plant represents a knowledge domain; plant health directly reflects the domain's retention score. By reviewing (watering), mastering (flowers), and connecting concepts (cross-pollination), the learner cultivates a thriving garden of knowledge that rewards consistency and depth. It transforms the abstract concept of "memory retention" into something tangible and beautiful.

### 5.2 Game Mechanics

#### 5.2.1 Garden Structure

| Element | Represents | Data Source |
|---|---|---|
| Plot | The learner's overall memory capacity | Memory Engine retention aggregate |
| Plants | Knowledge domains (1 plant per domain) | Domain-level retention scores |
| Plant Species | Domain type (tree/shrub/vine/flower) | Domain nature (conceptual/procedural/historical/linguistic) |
| Soil Quality | Foundational concept health | Root node mastery scores |
| Water Level | Recent review activity | Daily review completion rate |
| Weeds | Forgotten/misunderstood facts | Facts below mastery threshold |
| Flowers | Fully mastered concepts | Level 5 mastery facts |
| Fruit | Cross-domain connections | Inter-domain insights |
| Fertilizer | Deep study sessions | Extended focus sessions (>30 min) |
| Pests | Persistent misconceptions | Errors repeated 3+ times |
| Garden Decor | Achievements and milestones | Badges, titles, special events |

#### 5.2.2 Plant Types (Per Domain)

| Domain | Plant Species | Growth Pattern | Bloom Time |
|---|---|---|---|
| Aqidah | Oak tree | Slow, deep roots, massive canopy | Stable mastery |
| Fiqh | Vine | Spreading, branching, interconnected | During practice |
| Seerah | Rose bush | Layered, beautiful, some thorns | Story engagement |
| Quran | Date palm | Tall, noble, fruit-bearing | Memorization milestones |
| Hadith | Olive tree | Gnarled, ancient, light-giving | Chain understanding |
| Arabic | Jasmine | Delicate, fragrant, climbs | Vocabulary growth |
| Akhlaq | Lavender field | Spreading, calming, aromatic | Character integration |
| Tafsir | Fig tree | Broad leaves, deep meaning, sweet fruit | Deep reflection |

#### 5.2.3 Plant Health Mechanics

| Mechanic | Effect | Trigger |
|---|---|---|
| Watering | +10 health, plant stands tall | Daily review completion |
| Deep Watering | +25 health, special animation | Perfect review session |
| Weeding | Remove weed, +5 health | Re-learn forgotten fact |
| Pest Control | Remove pest, +10 health | Correct persistent misconception |
| Fertilizing | +15 health, growth boost | Extended learning session |
| Sunlight | +5 health | Login during daytime hours |
| Neglect | -5 health/day | No reviews in domain |
| Overwatering | -2 health (diminishing returns) | >3 reviews in same domain in one day |
| Seasonal Boost | +10 health | Season matches domain affinity |
| Cross-Pollination | +20 health to both domains | Make cross-domain connection |

#### 5.2.4 Weed System

| Weed Type | Appearance | Cause | Removal |
|---|---|---|---|
| Dandelion | Small yellow weed | 1 forgotten fact | Quick review (1 min) |
| Thorn Bush | Spiky, growing | 3+ forgotten facts clustered | Mini-review session (5 min) |
| Bindweed | Choking nearby plants | Misconception affecting multiple facts | AI Teacher intervention |
| Deadly Nightshade | Dark, ominous | Critical knowledge gap (mastery < 20%) | Full remediation |
| Giant Hogweed | Towering, aggressive | Prerequisite gap | Re-learn foundation |

#### 5.2.5 Flower and Fruit System

| Bloom Level | Description | Requirements | Reward |
|---|---|---|---|
| Bud | Small bud visible | Domain mastery 20% | 10 XP |
| Partial Bloom | Some petals open | Domain mastery 50% | 25 XP |
| Full Bloom | Flower fully open | Domain mastery 80% | 50 XP |
| Fruit | Fruit forms after bloom | Cross-domain connection at 80%+ | 100 XP + collection item |
| Eternal Bloom | Flower never wilts | Domain mastery sustained > 6 months | Rare title |

### 5.3 Progression Design

#### 5.3.1 Garden Growth Stages

| Stage | Total Memory Health | Visual State | Unlocks |
|---|---|---|---|
| Empty Plot | 0% | Bare soil, one small seed packet | Basic garden view |
| First Sprout | 10% | First plant emerges, tiny | Watering can tool |
| Growing Garden | 25% | 3-4 plants visible, some small flowers | Decoration placement |
| Thriving Garden | 50% | All domain plants present, flowers blooming | Garden path, fountain |
| Blooming Paradise | 75% | Full bloom, fruit visible, butterflies | Garden expansion, rare seeds |
| Eternal Garden | 90%+ | Perfect health, all effects, glowing | Mythic garden frame, time-lapse feature |
| Jannah Reflection | 100% (all domains) | Garden of unparalleled beauty, rivers of light | Ultimate garden achievement |

#### 5.3.2 Harvest Events

| Event | Trigger | Duration | Rewards |
|---|---|---|---|
| Daily Harvest | Complete all reviews | Instant | XP, small currency |
| Weekly Harvest | 5/7 days completed | Weekly reset | Gem, fertilizer item |
| Monthly Harvest | 20/30 days completed | Monthly reset | Rare seed, title fragment |
| Full Bloom Harvest | Domain reaches 100% bloom | Per domain | Domain title, cosmetic |
| Seasonal Harvest | Season end | Quarterly | Seasonal currency, limited items |
| Grand Harvest | All domains in bloom | Yearly | Mythic item, legacy title |

### 5.4 Visual Design

| Element | Specification |
|---|---|
| View | 3/4 overhead isometric, 2D rendered |
| Grid | 8×8 plot grid (64 tiles), expandable |
| Tile Types | Soil (plantable), path, water feature, decoration |
| Time of Day | Matches learner's local time (sun/shadows) |
| Weather | Light effects (sun, rain, clouds) randomized daily |
| Plant Animation | Swaying, growing, blooming, fruiting — subtle and soothing |
| Water Effect | Particles when watering, puddles after rain |
| Weed Animation | Wiggling, growing overnight if unchecked |
| Sound | Soft water trickle, wind through leaves, birds (optional) |
| UI | Minimal — health bars on hover, tooltips for actions |
| Seasonal Overlay | Snow in winter, cherry blossoms in spring, etc. |

#### 5.4.1 Weather Effects

| Weather | Visual | Gameplay Effect | Frequency |
|---|---|---|---|
| Sunny | Bright, clear, bird shadows | +5 growth | 40% |
| Light Rain | Soft rain, clouds | +3 water to all plants | 25% |
| Heavy Rain | Pouring, thunder distant | +8 water to all plants, weeds grow 2x | 10% |
| Overcast | Gray, soft light | No bonus or penalty | 15% |
| Windy | Plants sway, leaves fly | +2 growth, some items blow in | 8% |
| Noor (Ramadan) | Golden light, soft glow | All bonuses doubled, weeds slower | Ramadan only |

### 5.5 Reward Structure

| Achievement | Trigger | Reward |
|---|---|---|
| First Water | First review completed | 10 XP, basic watering can |
| First Flower | First domain bloom | 100 XP, flower cosmetic |
| Weed Puller | Remove 10 weeds | 50 XP, weeding tool upgrade |
| Garden Complete | All 8 domains planted | 500 XP, garden expansion |
| Green Thumb | 7-day streak maintained garden | 200 XP, fertilizer item |
| Master Gardener | All domains in full bloom | 2000 XP, "Master Gardener" title |
| Seasonal Cycle | Garden healthy in all 4 seasons | 1000 XP, seasonal decor set |
| Eternal Garden | Garden healthy for 1 year | 5000 XP, Mythic garden frame |

### 5.6 Social Features

| Feature | Description | Privacy |
|---|---|---|
| Garden Visit | Visit friends' gardens | Opt-in |
| Water Friend's Garden | Help water friends' plants | Anonymous option |
| Garden Gift | Give seeds, decorations | Friends |
| Garden Contest | Best garden of the month (non-competitive, inspirational) | Opt-in, anonymized |
| Community Garden | Study group shares a garden plot | Group members |
| Garden Snapshot | Share garden screenshot | Public/private |

### 5.7 Islamic Values Integration

| Value | Implementation |
|---|---|
| Planting Seeds | The garden teaches that knowledge takes time to grow — patience (sabr) |
| Tending | Daily care for knowledge mirrors daily care for faith |
| Fruits of Knowledge | The garden produces fruit that benefits others — teaching and sharing |
| Seasonal Cycles | Reminds learner that knowledge has seasons — times of growth and rest |
| Earth as Sign | Garden beauty points to Allah's creation, prompts reflection |

### 5.8 Edge Cases

| Edge Case | Handling |
|---|---|
| Learner ignores a domain for months | Plant wilts but never dies — dormant state with seed head, re-activatable with 3 reviews |
| All domains neglected | Garden becomes overgrown — "Revival Quest" to restore, with bonus rewards |
| Learner speed-runs reviews | Overwatering penalty prevents gaming the system |
| Garden too large (many domains) | Auto-scroll, minimap, zoom controls |
| Offline | Garden state cached, growth/decay calculated on reconnect with time delta |
| Very young learners | Simplified garden — fewer plants, brighter colors, no weeds, only flowers |
| Visual epilepsy risk | No flashing animations, optional "calm mode" — no particle effects |
| Multiple plants same domain | Not possible — one plant per domain, but plant size scales with mastery |

### 5.9 Technical Requirements

| Requirement | Specification |
|---|---|
| State Storage | Redis (hot: current garden state), PostgreSQL (cold: history) |
| Rendering | Canvas 2D with sprite batching for performance |
| Data Refresh | Real-time via WebSocket for active session, batch for background |
| Garden State Size | ~5KB per learner (plant positions, health, decor) |
| Watering Cooldown | Per plant, 6-hour cooldown tracked in Redis |
| Weather System | Server-generated seed per learner per day, deterministic |
| API | GET /garden/{learner_id}, POST /garden/water, POST /garden/decorate |
| Event Subscriptions | review_completed, forgetting_risk, mastery_change, season_change |
| Mobile | Touch-optimized, pinch-to-zoom, tap-to-water |
| Offline | State cached, water actions queued, synced |

---

## 6. DAILY JOURNEY

### 6.1 Purpose

The Daily Journey transforms the learner's daily learning activities from a list of tasks into an interactive map-based adventure. Every day, the learner wakes up to a unique path through a beautiful landscape, with waypoints representing their personalized learning activities, shortcuts for reviews, scenic routes for exploration, and treasure chests containing bonus rewards. This system directly integrates with the Personal AI Mentor's daily plan generation.

### 6.2 Game Mechanics

#### 6.2.1 Map Structure

| Element | Represents | Data Source |
|---|---|---|
| Path | The day's learning plan | Mentor's Daily Plan |
| Waypoints | Individual learning activities | Planned activities |
| Start Point | Learner's current state | Previous day's completion |
| Destination | Daily completion goal | Mentor's daily target |
| Shortcuts | Review/consolidation activities | Due reviews |
| Scenic Routes | Optional exploration topics | Interest-based suggestions |
| Treasure Chests | Bonus rewards | Streak/consistency bonuses |
| Bridges | Cross-domain connections | Interlinked topics |
| Mountains | Difficult challenges | Hard content |
| Rivers | Easy, flowing review | Well-mastered topics |
| Forest | Multi-fact exploration | Topic clusters |
| Rest Stops | Break reminders | Fatigue detection |

#### 6.2.2 Waypoint Types

| Waypoint | Icon | Action | Duration |
|---|---|---|---|
| Review Point | Spiral/cycle | Complete review task | 2-5 min |
| Learning Point | Book | New content | 5-15 min |
| Challenge Point | Star | Quiz/challenge | 3-5 min |
| Reflection Point | Moon | Journal/reflection | 2-3 min |
| Social Point | Two figures | Group activity | 5-10 min |
| Teaching Point | Lightbulb | Teach/review with peer | 5-10 min |
| Du'a Point | Hands raised | Du'a/break | 1 min |
| Watering Hole | Water drop | Review memory garden | 2 min |
| Hidden Path | Question mark | Optional discovery | 5 min |
| Scenic View | Mountain | Exploration content | 10 min |

#### 6.2.3 Path Generation

The Mentor (00_Personal_AI_Mentor) generates the daily journey path based on:

| Factor | Weight | Description |
|---|---|---|
| Due Reviews | 0.35 | Overdue and due-today reviews |
| Learning Goals | 0.25 | Curriculum progress targets |
| Weakness Areas | 0.20 | Predicted weak spots |
| Learner Energy | 0.10 | Estimated focus capacity |
| Available Time | 0.05 | Learner's time budget |
| Interest/Novelty | 0.05 | Mix of familiar and new |

### 6.3 Progression Design

#### 6.3.1 Journey Themes

| Theme | Unlocked | Visual Style | Vibe |
|---|---|---|---|
| Beginner's Path | Default | Green meadows, gentle hills | Calm, inviting |
| Scholar's Trail | Level 5 | Stone path through ancient library ruins | Scholarly, mysterious |
| Garden Walk | Garden planted | Rose-lined path, fountains | Peaceful, beautiful |
| Mountain Ascent | Rank 5 | Rocky path, switchbacks, peaks | Challenging, epic |
| Desert Crossing | Season: Summer | Golden sand, oases, starry nights | Focused, determined |
| Forest Deep | 10 domains active | Dense forest, dappled light, hidden clearings | Exploratory, magical |
| Ocean Voyage | Guild level 10 | Sailing between islands of knowledge | Adventurous, social |
| Noor Path | Ramadan active | Path of light through celestial realm | Spiritual, luminous |
| Ancient Road | 1 year on platform | Ancient cobblestone, historical markers | Nostalgic, wise |

#### 6.3.2 Journey Completion

| Completion Level | Criteria | Visual | Bonus |
|---|---|---|---|
| Started | Complete first waypoint | Path begins to glow | +25 XP |
| Halfway | 50% waypoints done | Path half illuminated | +50 XP, small treasure |
| Almost There | 80% done | Destination visible, glowing | +75 XP |
| Complete | All required waypoints done | Destination reached, celebration | +100 XP + streak protection |
| Perfect Journey | All waypoints (required + optional) | Entire path golden | +250 XP + gem |
| Explorer | All hidden paths found | Special "Explorer" completion animation | +500 XP + rare item |

### 6.4 Visual Design

| Element | Specification |
|---|---|
| Map View | 2D side-scrolling or top-down, stylized illustration |
| Resolution | Responsive: full on tablet/desktop, scrollable on mobile |
| Art Style | Watercolor-inspired, warm colors, soft edges |
| Animation | Path glows as waypoints complete, character walks along path |
| Character | Learner's avatar (simplified) walks the journey |
| Parallax | Background layers move at different speeds for depth |
| Weather | Matches learner's local weather (API) |
| Music | Soft ambient soundtrack that evolves with journey progress |
| Sound Effects | Footsteps, water, wind, treasure opening, bird calls |
| Night Mode | Automatically shifts to night scenery after Maghrib local time |

#### 6.4.1 Journey Completion Animation

| Element | Duration | Description |
|---|---|---|
| Path Illumination | 0.5s | Path from current position to destination lights up |
| Character Arrival | 1s | Avatar walks to destination point |
| Destination Glow | 1s | Destination marker pulses with light |
| Confetti/Particles | 2s | Knowledge particles rain or rise |
| Summary Card | Persistent | Shows stats: time, XP, topics covered |
| Spirit Reaction | 2s | Spirit appears, congratulates, shares wisdom |
| Next Day Preview | Optional | Peek at next day's theme (blurred) |

### 6.5 Reward Structure

| Reward | Trigger | Value |
|---|---|---|
| Treasure Chest (Small) | Every 3 waypoints | 25-50 XP, small currency |
| Treasure Chest (Large) | Scenic route completion | 100 XP, gem |
| Destination Chest | Journey completion | 150 XP, streak freeze |
| Hidden Treasure | Hidden path found | 200 XP, rare collection item |
| Treasure Map | All hidden paths in a week | 500 XP, title fragment |
| Perfect Week | All journeys complete 7 days | 2000 XP, exclusive cosmetic |

### 6.6 Social Features

| Feature | Description |
|---|---|
| Journey Sync | Study group can see each others' progress on journey (opt-in) |
| Journey Party | Two learners walk the same path together (real-time) |
| Path Sharing | Share completed journey as a visual story |
| Treasure Trade | Trade duplicate treasure items with friends |
| Journey Race | Friendly race to destination (same path, different instances) |
| Group Journey | Group destination — reach together for group bonus |

### 6.7 Islamic Values Integration

| Value | Implementation |
|---|---|
| Journey Metaphor | Life is a journey, knowledge is provision — Islamic concept of travel for knowledge |
| Destination | Ultimate destination is Allah's pleasure; each day's journey is a step |
| Rest Stops | Remind learner to rest, pray, reflect — not just consume |
| Du'a Points | Specific waypoints for du'a — integrating worship with learning |
| Scenic Views | Appreciate Allah's creation, beauty of knowledge |
| Path Etiquette | "Walk this path with humility, for every step is a blessing" |

### 6.8 Edge Cases

| Edge Case | Handling |
|---|---|
| Learner has no time today | "Mini Journey" — shortened path, 3 waypoints only, reduced rewards |
| Learner misses a day | Journey path shows yesterday's incomplete path as "overgrown", need to clear it first |
| Very long session | Auto-extend path with optional scenic routes if learner wants more |
| Interruption mid-activity | Waypoint marked as "paused", can resume from same point |
| Time zone change | Journey time adjusts, current path preserved |
| Holiday/vacation mode | "Rest Journey" — minimal path, reflection-focused, no penalties |
| Sick day | "Healing Journey" — soft path, easy activities, comfort theme |
| Learner wants to restart day | "Reset Journey" available once per day, forfeits current progress |
| Midnight passes during journey | Journey carries over grace period of 2 hours, then resets |

### 6.9 Technical Requirements

| Requirement | Specification |
|---|---|
| Path Generation | Server-side, daily at midnight learner's timezone |
| State Storage | Redis (current journey, completed waypoints) |
| Map Data | JSON layout per learner per day approx 2KB |
| Rendering | Canvas 2D, sprite-based |
| Music Streaming | Preloaded ambient tracks, crossfade on transition |
| API | GET /journey/{learner_id}/today, POST /journey/waypoint/complete |
| Offline | Entire journey cached at session start, synced on waypoint complete |
| Progress Persistence | If app closes mid-way, state restored on reopen |

---

## 7. KNOWLEDGE MAP

### 7.1 Purpose

The Knowledge Map is a global, strategic view of all knowledge domains available on the platform. Unlike the Knowledge Tree (which shows depth of mastery) or the Daily Journey (which shows the day's path), the Knowledge Map shows breadth — what territories of knowledge exist, which ones the learner has explored, which remain uncharted, and how domains connect to each other. It provides motivation through discovery and a sense of adventure.

### 7.2 Game Mechanics

#### 7.2.1 Map Structure

| Element | Represents | Data Source |
|---|---|---|
| Continent | Major domain (Aqidah, Fiqh, Quran, Hadith, etc.) | Knowledge Graph |
| Region | Sub-domain (within Aqidah: Tawheed, Qadr, Risalah) | KG sub-domains |
| City | Topic cluster | Topic groupings |
| Landmark | Notable concept or fact | Key nodes |
| Road | Prerequisite relationship | KG edges |
| Trade Route | Cross-domain connection | Inter-domain edges |
| Ocean | Unexplored space | Unknown/ungenerated content |
| Fog of War | Unexplored territory | Content learner hasn't accessed |
| Border | Domain boundary | KG boundaries |
| Capital | Core concept of domain | Central node |
| Monument | Rare achievement location | Special content |

#### 7.2.2 Exploration Mechanics

| Mechanic | Description | Effect |
|---|---|---|
| Discovery | First content in a region | Fog clears, region name revealed |
| Survey | Complete 25% of region | Map detail increases, points of interest appear |
| Mapping | Complete 50% | Full region visible, resources marked |
| Settlement | Achieve 75% mastery in region | Build a "knowledge outpost" |
| Conquest | 100% mastery of region | Region capital claimed, flag planted |
| Pathfinding | Follow prerequisite chain | Road appears between cities |
| Trade Route | Connect two domains | Bridge/tunnel visual between continents |

#### 7.2.3 Territory States

| State | Visual | Meaning |
|---|---|---|
| Unexplored | Dark, fog-covered, question marks | No content accessed |
| Scouted | Partial clearing, outline visible | Some content seen |
| Explored | Full visibility, basic details | Actively learning |
| Mapped | All content visible, landmarks marked | >50% mastery |
| Settled | Vibrant, active, learner's flag | >75% mastery |
| Conquered | Golden border, capital claimed, monument built | 100% mastery |
| Forgotten | Fading colors, fog creeping back | Memory decay >60% |
| Rediscovered | Renewed brightness, stronger colors | Re-mastered after decay |

### 7.3 Progression Design

#### 7.3.1 Conquest Progress

| Stage | Requirement | Reward |
|---|---|---|
| First Step | Enter any domain | 50 XP, basic compass tool |
| Path Blazer | Explore 10 regions | "Pathblazer" title, map zoom upgrade |
| Cartographer | Map 5 complete regions | "Cartographer" title, minimap unlocked |
| Explorer | Visit all continents | "Explorer" title, world map view |
| Settler | Settle 3 regions | Building placement unlocked |
| Conqueror | Conquer 1 full domain | Domain crown cosmetic, monument built |
| Emperor | Conquer all domains | "Emperor of Knowledge" title, legendary frame |
| Discoverer | Find all hidden areas | Map fully revealed, special reward |

#### 7.3.2 Hidden Areas

| Hidden Area | Discovery Method | Content | Reward |
|---|---|---|---|
| Cave of Reflection | Reach 80% mastery in 3 domains | Deep reflection content | Reflective title |
| Oasis of Wisdom | Complete all reviews for 30 days | Wisdom literature | Oasis cosmetic |
| Forgotten Library | Discover a domain from unexpected angle | Rare texts | Ancient book item |
| Garden of Names | Master 10 of Allah's names | Beautiful Names deep dive | Noor effect |
| Mountain of Patience | 365-day streak | Patience-themed content | "Sabr" title |
| Valley of Questions | Ask 100 AI Teacher questions | Q&A compilation | "Questioner" badge |
| Bridge of Connection | Make 10 cross-domain connections | Integration content | "Bridge Builder" title |

### 7.4 Visual Design

| Element | Specification |
|---|---|
| View | Top-down strategy map style, reminiscent of classic exploration games |
| Scale | Zoomable from world view (all domains) to city view (single topic) |
| Art Style | Antique map aesthetic — parchment background, ink drawings, compass rose |
| Colors | Warm parchment (#F5E6C8), ink (#2C1810), gold accents for completed areas |
| Fog of War | Animated fog/mist that shifts subtly, darker for less explored |
| Landmarks | Illustrated icons (mosque for Fiqh, book for Hadith, star for Aqidah) |
| Roads | Dashed lines connecting regions, solid when prerequisites met |
| Trade Routes | Animated caravans/ships moving between domains |
| Compass | Animated compass rose, always points to "knowledge north" (recommended next) |
| Animation | Gentle cloud movement, flickering settlement fires, animated caravans |
| Sound | Soft wind, distant bird calls, sea waves for ocean areas |

### 7.5 Reward Structure

| Achievement | Reward |
|---|---|
| First Region Explored | 50 XP, map fragment (reveals part of adjacent region) |
| First Domain Conquered | 1000 XP, domain-specific crown, monument |
| All Domains Explored | 5000 XP, "World Traveler" title |
| All Domains Conquered | 10000 XP, "Emperor of Knowledge" title, golden frame |
| 10 Hidden Areas Found | "Hidden Truths" badge |
| Knowledge Empire (5+ conquered domains) | Empire theme, custom flag |
| Trade Route Master | All cross-domain connections complete, "Bridge of Knowledge" title |

### 7.6 Social Features

| Feature | Description | Privacy |
|---|---|---|
| Map Sharing | Share explored regions | Opt-in, anonymized possible |
| Expedition Party | Explore a region together with study group | Group |
| Trade Routes | Share cross-domain insights with guild | Guild |
| Empire Comparison | Aggregate domain coverage in group | Group only, no ranking |
| Joint Conquest | Group conquers a domain together | Group goal |
| Map Gift | Gift a "map reveal" to a friend (helps clear fog) | Friends |

### 7.7 Islamic Values Integration

| Value | Implementation |
|---|---|
| Exploration | Seeking knowledge is a journey — Islamic history of travelers for knowledge |
| Conquest of Self | "Conquering" a domain represents mastering knowledge, not dominating others |
| Trade Routes | Knowledge sharing between domains reflects Islamic intellectual tradition |
| Fog of Ignorance | The fog represents jahl (ignorance) that is dispelled by ilm (knowledge) |
| Humility in Conquest | No comparison of empire sizes; focus on personal discovery |
| Compass | The compass always points to beneficial knowledge, not competition |

### 7.8 Edge Cases

| Edge Case | Handling |
|---|---|
| Learner overwhelmed by map size | "Guided Path" mode — highlights recommended region, dims others |
| New domain added to KG | New continent rises from ocean, fog covering it; discovery quest available |
| Learner wants to focus on one domain | "Zoom Focus" — other domains fade, single domain in detail |
| Forgotten domain becomes unexplored | Gradual fog returns over weeks, not sudden |
| Content not available in learner's language | Region shown as "distant land" — accessible when content available |
| Domain has no content yet | Region shown as "Mythical Land" — placeholder, coming soon indicator |
| Map too large on mobile | Bottom sheet with region list, tap to zoom to region |

### 7.9 Technical Requirements

| Requirement | Specification |
|---|---|
| Map Data | Tiled, procedurally generated from KG structure |
| Storage | PostgreSQL for territory states, Redis for active sessions |
| Rendering | Canvas 2D with tile engine, LOD for zoom levels |
| Tile Size | 256x256px, loaded on demand |
| Fog of War | Per-learner bitmap overlay, stored as compressed binary |
| API | GET /map/{learner_id}, GET /map/region/{region_id} |
| Generation | Map generated from KG once, learner state overlaid |
| Updates | Real-time WebSocket for active exploration, batch for sync |
| Mobile | Touch navigation, pinch zoom, tap-to-select |
| Performance | <100ms map load, <16ms frame render |

---

## 8. TITLES

### 8.1 Purpose

Titles are prestigious designations that the learner earns through specific, often difficult, achievements. Unlike the main level titles (which are automatic through XP), these 50+ titles represent specific accomplishments and are displayed prominently on the learner's profile. Titles provide bonuses, unlock content, and serve as meaningful markers of the learner's unique journey through knowledge.

### 8.2 Game Mechanics

#### 8.2.1 Title Hierarchy

| Tier | Color | Number | Example | Bonus |
|---|---|---|---|---|
| Common (Tier 1) | White | 15+ | "First Leaf," "Helper," "Questioner" | +2% XP |
| Uncommon (Tier 2) | Green | 12+ | "Gardener," "Pathblazer," "Connector" | +5% XP |
| Rare (Tier 3) | Blue | 10+ | "Cartographer," "Mentor," "Resilient" | +8% XP + small cosmetic |
| Epic (Tier 4) | Purple | 8+ | "Knowledge Seeker," "Scholar Owl," "Harvester" | +12% XP + cosmetic |
| Legendary (Tier 5) | Orange | 5+ | "Emperor of Knowledge," "Mountain," "Alim" | +20% XP + title aura |
| Mythic (Tier 6) | Rainbow/Animated | 3+ | "Living Legacy," "Noor of Knowledge," "Mu'allim" | +30% XP + animated title + profile frame |
| Legacy (Limited) | Gold fade | 2+ | "Founding Scholar," "Beta Pioneer" | Exclusive bonuses, legacy badge |

#### 8.2.2 Title Categories

| Category | Focus | Example Titles |
|---|---|---|
| Knowledge | Domain mastery | "Tawheed Scholar," "Fiqh Master," "Seerah Narrator," "Hadith Guardian," "Quran Lover" |
| Achievement | Milestones | "First Leaf," "Century" (100-day streak), "Millennium" (1000 facts), "Polyglot" |
| Skill | Specific competencies | "Speed Reader," "Perfect Score," "Flashcard Master," "Quiz Champion," "Debater" |
| Social | Community engagement | "Helper," "Mentor," "Group Leader," "Friend of Knowledge," "Community Pillar" |
| Spiritual | Islamic practice | "Qari," "Da'i," "Sabir" (Patient), "Shakir" (Grateful), "Muttaqi" |
| Special | Events, hidden | "Ramadan Star," "Night of Power," "Eid Achiever," "Hidden Truth," "Easter Egg Hunter" |
| Rare | Exceptional difficulty | "Unshaken" (never lost streak), "Perfectionist" (all perfect scores), "Eternal Bloom" |
| Legacy | Time-limited | "Founding Scholar" (first 10,000 users), "Beta Pioneer," "Season 1 Veteran" |
| Guild | Guild achievements | "Guild Founder," "Guild Champion," "Guild Scholar," "Alliance Builder" |

#### 8.2.3 Title Display

| Display Location | Format | Priority |
|---|---|---|
| Profile Header | [Title Icon] [Title Name] | Primary title shown |
| Username Prefix | "Title Name | Username" | Option to show |
| Username Suffix | "Username, Title Name" | Option to show |
| Chat/Forum | [Title Badge] next to name | Small icon |
| Leaderboard | Title shown on hover | Tier 4+ only |
| Study Room | Title displayed on nameplate | Opt-in |
| Profile Frame | Title color influences frame | Tier-based glow |

#### 8.2.4 Title Bonuses

| Bonus Type | Effect | Active When |
|---|---|---|
| XP Boost | +X% all XP earned | Always active |
| Domain Boost | +X% XP in specific domain | Title related to domain |
| Social Boost | +X% social XP | Social titles |
| Currency Boost | +X% coins/gems earned | Achievement titles |
| Streak Protection | +X freeze days per month | Consistency titles |
| Memory Boost | +X% retention during review | Knowledge titles |
| Cosmetic Unlock | Title-specific aura/frame | When equipped |
| Feature Unlock | Early access to features | Legendary+ titles |
| Teaching Unlock | Ability to create content | Mu'allim tier |
| Unique Interaction | Special AI Mentor dialogue | Mythic titles |

#### 8.2.5 Title Crafting

At higher levels, learners can craft titles by combining achievements:

| Recipe | Components | Result |
|---|---|---|
| 3 Knowledge Domain titles | "Tawheed Scholar" + "Fiqh Master" + "Seerah Narrator" | "Comprehensive Scholar" (Epic) |
| 5 Social titles | "Helper" + "Mentor" + "Group Leader" + "Friend" + "Pillar" | "Community Heart" (Legendary) |
| All 4 Seasonal titles | "Spring Scholar" + "Summer Practitioner" + "Autumn Reflector" + "Winter Reviewer" | "Season of Knowledge" (Epic) |
| 10 streak-freeze uses | Craftable with achievement tokens | "Unbreakable" (Rare) |
| 100 perfect scores | Combine 10 "Perfect Score" tokens | "Perfectionist" (Legendary) |

### 8.3 Title Progression Flow

```
New Learner → "First Leaf" (automatic, first achievement)
     ↓
Domain Learner → Domain-specific titles (e.g., "Aqidah Apprentice")
     ↓
Specialist → Higher domain titles ("Tawheed Scholar")
     ↓
Multi-Domain → Combined titles ("Comprehensive Scholar")
     ↓
Master → Legendary titles ("Mountain of Knowledge")
     ↓
Teacher → Teaching titles ("Mu'allim")
     ↓
Legacy → Mythic titles ("Living Legacy")
```

### 8.4 Visual Design

| Element | Specification |
|---|---|
| Common | White text, simple font weight |
| Uncommon | Green text, slight glow |
| Rare | Blue text, medium glow, small icon |
| Epic | Purple text, animated shimmer, medium icon |
| Legendary | Orange-gold text, pulsing glow, particle effects, large icon |
| Mythic | Rainbow shifting text, aura around name, unique particle system, animated icon |
| Legacy | Gold with fade animation, "Legacy" badge, exclusive border |

### 8.5 Reward Structure

| Tier | Static Bonuses | Special |
|---|---|---|
| Common | +2% XP | Profile display |
| Uncommon | +5% XP, +1% coins | Title icon |
| Rare | +8% XP, +3% coins, +2% domain XP | Small title aura |
| Epic | +12% XP, +5% coins, +5% domain XP | Medium title aura, profile accent |
| Legendary | +20% XP, +10% coins, +10% all bonuses | Full aura, exclusive frame |
| Mythic | +30% XP, +15% coins, +15% all bonuses, +5% memory | Animated aura, mythic frame, Mentor dialogue |
| Legacy | +25% XP, +10% coins, exclusive cosmetic | Legacy badge, founder status |

### 8.6 Social Features

| Feature | Description |
|---|---|
| Title Display | Show equipped title to others |
| Title Collection | View others' title collections (opt-in) |
| Title Trading | Trade title craft components |
| Title Gift | Gift a title craft recipe |
| Title Challenge | Challenge a friend to earn a specific title |
| Title Respect | "Respect" another learner's title (social acknowledgment) |

### 8.7 Islamic Values Integration

| Value | Implementation |
|---|---|
| No Riya | Titles focus on the knowledge gained, not showing off; "Helper" and "Servant" valued equally |
| Humility | "Alim" title requires teaching others, not just personal mastery |
| Service | Social titles (Helper, Mentor) are as prestigious as knowledge titles |
| Barakah | Mythic titles have light/noor themes, not power themes |
| Adab | Title descriptions include Islamic etiquette reminders |
| Niyyah | Title earning process includes intention-setting |

### 8.8 Edge Cases

| Edge Case | Handling |
|---|---|
| Learner wants no title | "No Title" option — hides title, no penalty |
| Title too long for UI | Abbreviation on small screens, full on hover |
| Title requirement impossible (e.g., Seasonal missed) | Titles return yearly — learner can earn next cycle |
| Title crafting mistake | One "de-craft" per month, components returned minus 20% |
| Learner has multiple titles | Can equip one primary, others shown in collection |
| Title system overwhelming new users | Hidden until first title earned; progressive reveal |
| Language/translation | All titles translatable to learner's language, Arabic originals shown parenthetically |

### 8.9 Technical Requirements

| Requirement | Specification |
|---|---|
| Title Data | Static JSON (title definitions, requirements, bonuses) |
| Learner Titles | PostgreSQL (earned titles, equipped title, craft progress) |
| Title Check | Event-driven: on achievement, check all title requirements |
| Crafting System | Recipe validation, component tracking, result issuance |
| API | GET /titles, GET /titles/{learner_id}, POST /titles/equip, POST /titles/craft |
| Icon CDN | Preloaded sprite sheet, lazy update for new titles |
| Display | Server-rendered title with tier styling, client-cached |

---

## 9. RANKS

### 9.1 Purpose

While the XP-based level system from 17_Gamification.md provides a linear progression, the Rank system introduces multi-dimensional advancement that requires the learner to grow in multiple ways — knowledge depth, consistency, community contribution, spiritual practice, and demonstrated mastery. Ranks provide prestigious, meaningful milestones that cannot be achieved through XP grinding alone.

### 9.2 Game Mechanics

#### 9.2.1 Rank Hierarchy (20 Ranks)

| Rank | Title (Arabic) | Title (English) | XP Required | Additional Requirements |
|---|---|---|---|---|
| 1 | طالب | Seeker | 0 | Join the platform |
| 2 | مبتدئ | Beginner | 500 | Complete 1 domain at 20%+ mastery |
| 3 | دارس | Learner | 1,500 | 3-day streak, 5 quizzes passed |
| 4 | متعلم | Student | 3,500 | 7-day streak, 3 domains active, 10 reviews |
| 5 | متتبع | Follower | 7,000 | 14-day streak, 1 domain at 40%+ mastery |
| 6 | باحث | Researcher | 12,000 | 30-day streak, 2 domains at 40%+, 1 social interaction |
| 7 | فاهم | Understander | 20,000 | 45-day streak, 1 domain at 60%+, first badge earned |
| 8 | حافظ | Preserver | 35,000 | 60-day streak, 2 domains at 60%+, 90% memory health in 1 domain |
| 9 | ناقد | Critic | 55,000 | 75-day streak, 3 domains at 60%+, 10 peer helps |
| 10 | محقق | Verifier | 85,000 | 100-day streak, 1 domain at 80%+, teaching 1 peer |
| 11 | مفسر | Interpreter | 125,000 | 120-day streak, 2 domains at 80%+, cross-domain connection |
| 12 | محدث | Narrator | 175,000 | 150-day streak, 3 domains at 80%+, 5 collection sets complete |
| 13 | فقيه | Jurist | 250,000 | 200-day streak, 1 domain conquered (100%), guild member |
| 14 | أصولي | Foundationalist | 350,000 | 250-day streak, 2 domains conquered, mentor 2 peers |
| 15 | مجتهد | Diligent One | 500,000 | 300-day streak, 3 domains conquered, lead 1 guild quest |
| 16 | حكيم | Wise One | 750,000 | 1-year streak, 4 domains conquered, 50 peer helps, create content |
| 17 | مرشد | Guide | 1,000,000 | 400-day streak, 5 domains conquered, guild officer, 100 peer helps |
| 18 | مفتي | Scholar of Rulings | 1,500,000 | 500-day streak, 6 domains conquered, guild leader, completed Master Challenge |
| 19 | عالم | Scholar | 2,500,000 | 2-year streak, 7 domains conquered, multiple Master Challenges, taught 10 peers to Rank 10+ |
| 20 | رباني | Divine Scholar | 5,000,000 | 3-year streak, all domains conquered, legendary title, peer-nominated, contribution to knowledge |

#### 9.2.2 Rank Requirements — Multi-Dimensional

| Dimension | Metrics | Weight |
|---|---|---|
| Knowledge (K) | XP, domains mastered, facts learned, cross-domain connections | 35% |
| Consistency (C) | Streak length, daily engagement, review completion rate | 20% |
| Mastery (M) | Domain conquests, memory health, quiz accuracy | 25% |
| Community (S) | Peer help, teaching, guild participation, content sharing | 15% |
| Spiritual (P) | Du'a tracking, Quran engagement, intention logging | 5% |

#### 9.2.3 Rank Progress Visualization

```
Rank Progress: محقق (Verifier) → مفسر (Interpreter)
═══════════════════════════════════════════════
K ████████████████████████░░░░ 82% (Req: 85,000 XP)
C ██████████████████████░░░░░░ 76% (Req: 120-day streak, at 93)
M ████████████████████████████ 92% (Req: 2 domains at 80%)
S ████████████████░░░░░░░░░░░░ 48% (Req: 5 peer helps, at 2)
P ██████████████████████████░░ 84% (Req: 30 spiritual actions)
───────────────────────────────────────
Overall: 76% — 24% to next rank
```

### 9.3 Progression Design

#### 9.3.1 Rank Ceremonies

When a learner achieves a new rank, a ceremony is triggered:

| Ceremony Element | Description |
|---|---|
| Notification | Full-screen rank reveal with calligraphy |
| Rank Title Announced | Title appears with Arabic calligraphy animation |
| Spirit Speech | Spirit delivers a speech about the rank's meaning |
| Ayat Display | Relevant Quranic verse about knowledge |
| Du'a | Suggested du'a for continued growth |
| Reward Reveal | Rank rewards displayed |
| Next Rank Preview | Requirements for next rank shown |
| Social Share | Option to share with privacy controls |
| Profile Update | Rank displayed on profile automatically |

#### 9.3.2 Rank Rewards

| Rank Range | Rewards |
|---|---|
| 1-5 | Basic profile frame, title unlock, 100-500 coins |
| 6-10 | Medium profile frame, cosmetic item, 1000-5000 coins, 1-5 gems |
| 11-15 | Premium frame, rare cosmetic, 10000-50000 coins, 10-50 gems, accessory |
| 16-18 | Legendary frame, epic cosmetic, 100000 coins, 100 gems, exclusive content |
| 19-20 | Mythic frame, legendary cosmetic, special title, Mentor dialogue unlock, real-world recognition |

#### 9.3.3 Rank Protection

| Protection | Mechanism |
|---|---|
| No Rank Loss | Once earned, rank is permanent — no decay can reduce rank |
| Grace Period | After earning a rank, 7 days before next rank requirements start counting |
| Alternative Paths | If one requirement is blocked (e.g., social for shy learners), alternative paths available |
| Rank Freeze | Learner can freeze rank progression (stop counting) for up to 30 days for personal reasons |
| Inactivity Grace | 90 days of inactivity before rank badges dim (not lost) |

### 9.4 Visual Design

| Element | Description |
|---|---|
| Rank Badge | Circular badge with rank number, Arabic calligraphy, tier color |
| Profile Display | Badge shown prominently at top of profile, clickable for details |
| Name Color | Learner's display name takes rank color in social spaces (opt-in) |
| Ceremony Screen | Animated parchment with ink flowing to reveal rank name, golden particles |
| Progress Ring | Circular progress indicator in profile showing multi-dimensional progress |
| Rank Aura | Subtle glow around avatar matching rank tier (visible in study rooms) |
| Badge Animations | Tier 10+ badges have subtle animations (shimmer, pulse, orbit ring) |

#### 9.4.1 Rank Tier Colors

| Tier | Ranks | Color | Visual Effect |
|---|---|---|---|
| Novice | 1-5 | Silver (#C0C0C0) | Simple badge, no glow |
| Apprentice | 6-10 | Bronze (#CD7F32) | Subtle metallic sheen |
| Adept | 11-14 | Gold (#FFD700) | Warm glow, slight shimmer |
| Master | 15-17 | Sapphire (#0F52BA) | Blue glow, rotating ring |
| Grand Master | 18-19 | Ruby (#E0115F) | Red-gold glow, animated orbit |
| Divine Scholar | 20 | Diamond/Iridescent | Full rainbow shift, celestial particles |

### 9.5 Reward Structure

| Rank | XP Reward | Coin Reward | Gem Reward | Special |
|---|---|---|---|---|
| 1→2 | 100 | 100 | 0 | First domain tracker |
| 2→3 | 200 | 200 | 1 | Quiz boost item |
| 3→4 | 300 | 400 | 2 | Streak freeze (1) |
| 4→5 | 500 | 700 | 3 | Avatar accessory |
| 5→6 | 800 | 1000 | 5 | Title: "Researcher" |
| 6→7 | 1200 | 1500 | 8 | Profile frame upgrade |
| 7→8 | 1800 | 2000 | 10 | Spirit accessory |
| 8→9 | 2500 | 3000 | 15 | Garden decoration |
| 9→10 | 3500 | 5000 | 20 | Map reveal tool |
| 10→11 | 5000 | 7500 | 30 | Rank ceremony customization |
| 11→12 | 7000 | 10000 | 50 | Exclusive story unlock |
| 12→13 | 10000 | 15000 | 75 | Teaching license |
| 13→14 | 15000 | 20000 | 100 | Guild creation token |
| 14→15 | 20000 | 30000 | 150 | Custom study room |
| 15→16 | 30000 | 50000 | 200 | AI Mentor persona unlock |
| 16→17 | 50000 | 75000 | 300 | Content creation rights |
| 17→18 | 75000 | 100000 | 500 | Master Challenge key |
| 18→19 | 100000 | 200000 | 750 | Scholar verification |
| 19→20 | 200000 | 500000 | 1000 | Legacy founder status |

### 9.6 Social Features

| Feature | Description |
|---|---|
| Rank Display | Rank visible on profile (always), optionally in chat/forums |
| Rank Congratulation | Friends can congratulate with a "Rank Salute" animation |
| Rank Mentorship | Higher ranks can officially mentor lower ranks |
| Rank Progression Share | Share rank-up ceremony with friends |
| Rank Hall | Hall of learners who achieved each rank (opt-in, anonymized) |
| Rank Challenge | Challenge another learner of same rank to knowledge duel |

### 9.7 Islamic Values Integration

| Value | Implementation |
|---|---|
| Tawadhu | Rank 20 (Rabbānī) requires peer nomination — others recognize your knowledge |
| Niyyah | Rank progression includes intention checks at major milestones |
| Service | Social requirements ensure rank isn't just personal achievement |
| Barakah | Higher ranks emphasize teaching and giving, not status |
| Ihsan | Multi-dimensional requirements ensure balanced excellence |
| Knowledge for Allah | Rank descriptions emphasize "knowledge seeks you as much as you seek it" |

### 9.8 Edge Cases

| Edge Case | Handling |
|---|---|
| Learner maxes XP but lacks social requirements | Social alternative: write a reflection, answer forum questions |
| Learner very shy/antisocial | Alternative social: teach AI Teacher (simulated), contribute to public flashcards |
| Streak lost due to emergency | Rank freeze available, request via support, max 30 days |
| Learner accelerates too fast | Rank minimum time requirements prevent speed-running |
| New content/domains added | Rank requirements adjusted, existing learners grandfathered |
| Parent/teacher account | Parent accounts have separate rank track (guide-focused) |
| Multiple languages | Rank titles shown in learner's language + Arabic original |
| Under-13 learners | Social requirements reduced, parental consent for community aspects |

### 9.9 Technical Requirements

| Requirement | Specification |
|---|---|
| Rank Data | Static definition JSON (20 ranks, multi-dimensional requirements) |
| Learner Rank | PostgreSQL (rank_id, earned_date, progress per dimension) |
| Rank Check | Scheduled (daily) + event-driven (on achievement) |
| Ceremony System | Triggered on rank_change event, client-side animation |
| API | GET /ranks, GET /ranks/{learner_id}, POST /ranks/celebrate |
| Multi-Dim Progress | Stored as JSONB: {k: 0.82, c: 0.76, m: 0.92, s: 0.48, p: 0.84} |
| Rank Freeze | Redis + PostgreSQL, max 30 days per calendar year |

---

## 10. RARE ACHIEVEMENTS

### 10.1 Purpose

Rare Achievements extend the badge system from 17_Gamification.md with achievements that are genuinely difficult, surprising, or skill-based. They are not grindable — many require perfect timing, unique combinations, or exceptional skill. Discovering and earning rare achievements is a meta-game that rewards the most dedicated and skilled learners.

### 10.2 Game Mechanics

#### 10.2.1 Achievement Categories

| Category | Focus | Number | Discovery Method |
|---|---|---|---|
| Hidden | Secret actions, Easter eggs | 25+ | Experimentation, exploration |
| Discovery-Based | Finding special content | 15+ | Exploring all areas |
| Combo | Sequential actions in order | 10+ | Planning and execution |
| Speed | Time-pressured perfection | 8+ | Skill under pressure |
| Perfection | Flawless execution | 10+ | Accuracy without error |
| Community | Group coordination | 12+ | Teamwork |
| Seasonal | Time-limited, event-based | 20+ | Calendar awareness |
| Endurance | Long-term sustained effort | 8+ | Consistency over time |
| Creative | User-generated content | 6+ | Contribution |
| Mastery | Ultimate challenge completion | 5+ | Comprehensive excellence |

#### 10.2.2 Example Rare Achievements

| Achievement | Category | Requirement | Rarity | Reward |
|---|---|---|---|---|
| Silent Scholar | Hidden | Study for 30 consecutive days without any social interaction | Rare | "Silent Scholar" title, 500 XP |
| Eagle Eye | Discovery | Find 5 Easter eggs in Story Mode | Uncommon | 300 XP, eagle companion |
| Combo Breaker | Combo | Complete 3 domains in alphabetical order in one week | Epic | "Combo Breaker" title, 1000 XP |
| Speed Demon | Speed | Complete a Master Challenge in under 5 minutes | Legendary | "Speed Demon" title, 2000 XP |
| Flawless Victory | Perfection | 100 consecutive quiz questions correct, all difficulties | Legendary | "Flawless" title, 3000 XP |
| United We Learn | Community | Study group of 5+ completes a Master Challenge together | Epic | "United" title, group reward |
| Ramadan Warrior | Seasonal | Complete all daily goals for entire Ramadan | Legendary | "Ramadan Warrior" title, exclusive |
| Year of Knowledge | Endurance | 365-day streak with >80% average weekly goal completion | Mythic | "Year of Knowledge" title, 10000 XP |
| Content Creator | Creative | Create 10 flashcards used by 100+ learners | Rare | "Creator" title, 2000 XP |
| Triple Crown | Mastery | Conquer 3 domains at Expert difficulty within same month | Mythic | "Triple Crown" title, 5000 XP |

#### 10.2.3 Achievement Rarity Tiers

| Rarity | % of Learners | Visual Color | Unlock Animation |
|---|---|---|---|
| Common | >30% | White | Simple popup |
| Uncommon | 10-30% | Green | Popup with glow |
| Rare | 3-10% | Blue | Banner with particles |
| Epic | 1-3% | Purple | Full screen with sound |
| Legendary | 0.1-1% | Orange | Cinematic sequence |
| Mythic | <0.1% | Rainbow/Animated | Personalized animation |

#### 10.2.4 Achievement Discovery System

| Discovery Method | Description | Example |
|---|---|---|
| Blind Discovery | Achievement hidden, no hint | Complete a review at exactly Fajr time |
| Hint System | Cryptic clues after 80% of requirements met | "The early bird catches the knowledge worm" |
| Chain Discovery | One achievement leads to another | Complete "Early Bird" → reveals "Night Owl" |
| Community Discovery | First learner to unlock reveals to community | "New achievement discovered by Learner_X!" |
| Pattern Recognition | Achievement follows recognizable pattern | Every 10th flashcard set completed at midnight |
| Exploration Reward | Found by interacting with unexpected UI | Click on the compass rose 10 times |

### 10.3 Progression Design

#### 10.3.1 Achievement Families

Some achievements are part of families that tell a story:

| Family | Theme | Achievements | Final Reward |
|---|---|---|---|
| Day Cycle | Learning at different times | Fajr Scholar, Sunrise Reader, Dhuha Learner, Noon Quiz, Asr Review, Maghrib Story, Isha Reflection | "Day Cycle Master" — completes cycle 7 times |
| Knowledge Seeker | Exploration path | First Step, Pathblazer, Cartographer, Explorer, Settler, Conqueror | "Knowledge Seeker" — conquer all domains |
| Garden Life | Memory garden evolution | Seed Planter, Waterer, Weeder, Gardener, Bloom Master, Eternal Garden | "Garden of Eden" — perfect garden for 1 year |
| Spirit Bond | Spirit relationship | First Words, Growing Bond, Deep Connection, Soul Mates, Eternal Companions | "Spirit Walker" — max bond with all spirit types |
| Community | Social progression | Helper, Teacher, Mentor, Guide, Leader, Inspiration | "Community Heart" — help 1000 peers |

#### 10.3.2 Achievement Tracking

| Progress UI | Description |
|---|---|
| Achievement Log | Complete list with search, filter by category/rarity/status |
| Progress Bar | Per-achievement progress indicator (hidden for undiscovered) |
| Recently Earned | Scrollable gallery of last 10 achievements |
| Rarest First | Sort by rarity percentage |
| Completion Percentage | Overall achievement completion (all-time) |
| Category Breakdown | Progress per category |
| Missing Clues | Cryptic hints for undiscovered achievements (after reaching 80% criteria) |

### 10.4 Visual Design

| Element | Description |
|---|---|
| Achievement Card | Stylized card with icon, name, rarity color, description, date earned |
| Unlock Animation | Depends on rarity — simple glow to full cinematic |
| Sound Effect | Distinctive chime per rarity tier (optional, opt-in) |
| Gallery View | Grid of earned achievements, filterable, sortable |
| Progress UI | Circular progress, hidden clue text for near-complete |
| Rarity Badge | Small gem/crystal icon colored by rarity |
| Share Card | Automatically generated shareable image on Legendary+ unlocks |

### 10.5 Reward Structure

| Rarity | XP | Coins | Gems | Special |
|---|---|---|---|---|
| Common | 50 | 25 | 0 | Progress toward title |
| Uncommon | 150 | 100 | 1 | Small cosmetic |
| Rare | 500 | 500 | 5 | Title unlock |
| Epic | 1500 | 2000 | 20 | Rare cosmetic, title fragment |
| Legendary | 5000 | 10000 | 100 | Legendary title, exclusive cosmetic |
| Mythic | 20000 | 50000 | 500 | Mythic title, unique profile frame, real-world rewards |

### 10.6 Social Features

| Feature | Description |
|---|---|
| Achievement Showcase | Display 3 favorite achievements on profile |
| First Discovery | "First Discoverer" badge for being first to unlock (anonymized) |
| Achievement Trading | Trade achievement-related collectibles |
| Help Discover | Community hints for hidden achievements (spoilable, opt-in) |
| Achievement Race | Community challenge to see who unlocks first (no leaderboard) |
| Rare Achievement News | When a Mythic achievement is unlocked, community notification (anonymized) |

### 10.7 Islamic Values Integration

| Value | Implementation |
|---|---|
| No Riya | "First Discoverer" is anonymous — no personal credit displayed publicly |
| Ihsan | Achievements reward excellence, not just participation |
| Sabr | Some achievements take years — patience is rewarded |
| Tawakkul | Hidden achievements create delightful surprises (barakah) |
| Humility | Rarity is hidden from the learner — no "I have more rare achievements than you" |
| Adab | No achievements that require boasting or attention-seeking |

### 10.8 Edge Cases

| Edge Case | Handling |
|---|---|
| Achievement becomes impossible (event passed) | Flagged as "Legacy" — still displayed, no longer earnable |
| Learner discovers achievement accidentally | Still earned — no penalty for accidental discovery |
| Cheating/exploit to unlock | System detects unusual patterns, flags for review, revoke if confirmed |
| New achievements added | Existing learners retroactively checked (where possible) |
| Achievement too easy after content update | Rarity adjusted, already-earned learners keep it |
| Privacy concern with "no social" achievements | No social interaction required suggested as alternative path |
| Achievement notification spam | Daily digest option, priority only for Rare+ |

### 10.9 Technical Requirements

| Requirement | Specification |
|---|---|
| Achievement Definitions | Static JSON with criteria, rarity, rewards |
| Achievement Checking | Event-driven on all learning events, batch check daily |
| Learner Progress | PostgreSQL (achievement_id, learner_id, progress, earned_date) |
| Rarity Calculation | Percentage of active learners who earned it, recalculated weekly |
| Hint System | Server-side hint generation based on progress %, not criteria |
| API | GET /achievements, GET /achievements/{learner_id}, POST /achievements/claim |
| Notification | Push + in-app for Rare+, digest for Common/Uncommon |
| First Discoverer | Tracked in PostgreSQL, anonymized in public display |

---

---

## 11. COLLECTIONS

### 11.1 Purpose

Collections extend the currency system from 17_Gamification.md with a rich ecosystem of collectible items that learners discover, earn, and trade. Unlike currency (which is spent), collections represent the learner's unique journey — each item is a memory of an achievement, a discovery, or a milestone. Collections encourage deep exploration of all content areas and provide long-term goals beyond the XP grind.

### 11.2 Game Mechanics

#### 11.2.1 Collection Categories

| Category | Items | Theme | Source |
|---|---|---|---|
| Knowledge Gems | 50+ | Crystals representing concepts (Tawheed Ruby, Fiqh Emerald, Seerah Sapphire) | Domain mastery milestones |
| Historical Artifacts | 40+ | Objects from Islamic history (Prophet's Seal replica, Alhambra tile, Astrolabe) | Special challenges, Hidden discoveries |
| Scholarly Quotes | 30+ | Sayings of Islamic scholars (Imam Abu Hanifa, Imam Shafi'i, Ibn Taymiyyah, etc.) | Reading milestones, AI Teacher dialogues |
| Illuminated Letters | 28+ | Arabic alphabet in artistic calligraphy | Arabic learning milestones |
| Sacred Plants | 20+ | Plants mentioned in Quran and Hadith (Olive, Fig, Date Palm, Pomegranate) | Memory garden achievements |
| Stars of Knowledge | 100+ | Stars in a constellation map, each star is a major concept mastered | Cross-domain conquest |
| Du'a Pearls | 30+ | Pearls representing specific du'as learned and practiced | Du'a consistency |
| Prophet Stories | 25+ | Story artifacts from each prophet mentioned in Quran | Story mode completion |
| Quran Jewels | 30+ | Jewels representing Juz/parts of Quran | Quran progress |
| Legacy Relics | 10+ | Limited edition items from platform history | Time-sensitive events |

#### 11.2.2 Item Rarity Tiers

| Tier | Distribution | Color | Visual Effect |
|---|---|---|---|
| Common | 40% of items | White | Simple shape, no effect |
| Uncommon | 25% | Green | Subtle shimmer |
| Rare | 18% | Blue | Glow effect |
| Epic | 10% | Purple | Rotating/pulsing animation |
| Legendary | 5% | Orange | Particle effects |
| Mythic | 2% | Rainbow/Animated | Full animation, sound on view |

#### 11.2.3 Collection Completion Bonuses

| Completion Level | Bonus |
|---|---|
| 10% of a collection | +2% XP in related domain |
| 25% | +5% XP, collection badge (Bronze) |
| 50% | +10% XP, collection badge (Silver), cosmetic unlock |
| 75% | +15% XP, collection badge (Gold), rare item |
| 90% | +20% XP, collection badge (Platinum), title fragment |
| 100% (Complete) | +30% XP, collection badge (Diamond), full title, exclusive frame, spirit accessory |

#### 11.2.4 Trading System

| Feature | Description | Constraints |
|---|---|---|
| Peer Trading | Trade duplicate items with friends | Same rarity tier, max 3 trades/day |
| Guild Trading | Guild members can trade within guild | No rarity restriction within guild |
| Gift System | Gift items to friends (one-way) | Max 1 gift/day, Common-Uncommon only |
| Marketplace | Global anonymous trade (future) | Epic+, moderated |
| Trade Cooldown | Item cannot be re-traded for 24h | Prevents flipping |
| Trade Log | Record of trades for safety | Learner can view own log |
| Item Binding | Certain items bind on acquire (cannot trade) | Mythic, legacy, account-bound items |

### 11.3 Progression Design

#### 11.3.1 Display Cases

| Display Type | Unlock | Capacity | Customization |
|---|---|---|---|
| Simple Shelf | Default | 10 items | Basic wood color |
| Glass Cabinet | 50 items collected | 25 items | Glass tint, lighting |
| Treasure Chest | 100 items | 30 items (rotating display) | Chest style (wood, brass, jeweled) |
| Gallery Wall | 250 items | 50 items | Frame styles, layout |
| Museum Hall | 500 items | 100 items | Room theme, lighting, music |
| Grand Museum | 1000 items | All items (rotating exhibits) | Full museum, open to visitors |

#### 11.3.2 Collection Progression

| Milestone | Reward |
|---|---|
| First item collected | 50 XP, display shelf |
| 10 items | 100 XP, "Collector" title (tier 1) |
| 50 items | 500 XP, glass cabinet |
| 100 items | 1000 XP, "Collector" title (tier 2), 10 gems |
| 250 items | 2500 XP, gallery wall, rare display frame |
| 500 items | 5000 XP, "Master Collector" title, museum hall |
| 1000 items | 10000 XP, "Grand Collector" title, grand museum |
| Any collection 100% | Per-collection bonus (see above) |
| All collections 100% | 50000 XP, "Completionist" title, mythic display |

### 11.4 Visual Design

| Element | Description |
|---|---|
| Item Card | Stylized card: item name, rarity border, description in Arabic/English, source, date acquired |
| 3D Model | Key items rendered in 3D for close inspection (rotate, zoom) |
| Collection View | Grid or shelf display, sortable by category/rarity/date |
| Display Room | 3D room where items are physically placed, walkable |
| Item Animation | Hover effect, idle animation (gem sparkle, star rotation) |
| Rarity Effects | Per-tier visual treatment from simple to elaborate |
| Completion Display | Special effect when collection reaches 100% |
| Sound | Item has "ping" sound when acquired, ambient in display room |

### 11.5 Reward Structure

| Achievement | Reward |
|---|---|
| First Collection Complete | 500 XP, collection title, display case upgrade |
| Three Collections Complete | 2000 XP, "Trifecta" title, rare cosmetic |
| Five Collections Complete | 5000 XP, "Pentathlon" title, epic cosmetic |
| Ten Collections Complete | 15000 XP, "Deca Collector" title, legendary display |
| All Collections Complete | 50000 XP, "Completionist Supreme" title, mythic museum |
| Trade 100 items | 1000 XP, "Trader" badge |
| Gift 50 items | 1000 XP, "Generous" badge |

### 11.6 Social Features

| Feature | Description |
|---|---|
| Display Visit | Visit friends' display rooms |
| Item Inspection | View and learn about friends' items |
| Wishlist | Public wishlist for guild/friends to see |
| Collection Comparison | See which items friend has that you don't (no count comparison) |
| Guild Museum | Guild shares a museum with contributed items |
| Item Story | Each item tells a story (Islamic historical context, knowledge connection) |
| Legacy Museum | Top collectors' museums featured (anonymized, rank-based) |

### 11.7 Islamic Values Integration

| Value | Implementation |
|---|---|
| Knowledge Over Possessions | Items are knowledge-related, not materialistic — gems represent concepts, not wealth |
| Sharing | Trading and gifting encouraged over hoarding |
| Barakah | Display rooms have barakah light effects when items are appreciated |
| History Appreciation | Historical artifacts connect learner to Islamic heritage |
| Gratitude | Each item acquisition prompts shukr |
| Humility | No "richest collector" leaderboard — focus on knowledge themes |

### 11.8 Edge Cases

| Edge Case | Handling |
|---|---|
| Duplicate items | Automatically eligible for trade, stack counter shown |
| Item lost due to bug | Server-authoritative inventory, rollback capability |
| Trade scam prevention | Trade confirmation, reputation system, 24h appeal window |
| Offline item acquisition | Queued, notification on reconnect |
| Item from expired event | "Legacy" tag, cannot be acquired but can be traded |
| Display too large for device | Auto-arranged grid view, simplified 2D fallback |
| Learner wants to delete item | "Archive" option — hides from display, still counted in collection |

### 11.9 Technical Requirements

| Requirement | Specification |
|---|---|
| Item Definitions | PostgreSQL (item_id, name, category, rarity, metadata) |
| Learner Inventory | PostgreSQL (item_id, learner_id, acquired_date, tradeable, source) |
| Display State | JSONB layout per learner (item placements, display case config) |
| Trading Engine | Transactional with rollback, cooldown enforcement |
| API | GET /collections, GET /collection/{category}, POST /trade, POST /gift, GET /display/{learner_id} |
| Event Subscriptions | item_acquired, collection_complete, trade_completed |
| 3D Display | Three.js for display room, fallback to 2D grid |

---

## 12. BADGES (EXTENDED)

### 12.1 Purpose

This section extends the badge system from 17_Gamification.md (which defined 5 tiers: Bronze→Diamond) with 6 tiers (adding Mythic), badge families with evolution paths, animated badges, sound effects, social sharing, and a comprehensive badge display system. The original 17_Gamification.md badge system remains the foundation — this document adds depth and richness.

### 12.2 Game Mechanics

#### 12.2.1 Badge Families

| Family | Focus | Number of Badges | Evolution Type |
|---|---|---|---|
| Knowledge | Domain mastery, facts learned | 15+ | Single-path: Bronze→Mythic |
| Skill | Quiz accuracy, speed, consistency | 12+ | Branching: multiple evolution paths |
| Social | Community, teaching, mentoring | 10+ | Convergent: multiple sources to one badge |
| Spiritual | Du'a, Quran, reflection | 10+ | Single-path |
| Special | Events, hidden, seasonal | 20+ | Unique: fixed form, no evolution |

#### 12.2.2 Badge Evolution (6 Tiers)

| Tier | Requirement Multiplier | Visual | Effect |
|---|---|---|---|
| Bronze | 1x base requirement | Bronze-colored icon | None |
| Silver | 10x base | Silver-colored, slight metallic shine | +2% XP in related area |
| Gold | 50x base | Gold-colored, glow effect | +5% XP, small cosmetic |
| Platinum | 200x base | Platinum with blue-white glow | +8% XP, cosmetic, +1 gem |
| Diamond | 1000x base | Diamond with rainbow refraction, animated | +12% XP, exclusive cosmetic, title |
| Mythic | 5000x base or special condition | Mythic with full animation, aura, sound | +20% XP, Mythic title, profile frame |

#### 12.2.3 Example Badge Evolution

| Badge | Bronze | Silver | Gold | Platinum | Diamond | Mythic |
|---|---|---|---|---|---|---|
| Quiz Master | Pass 10 quizzes | Pass 100 | Pass 500 | Pass 2000 | Pass 10000 | Pass 50000 OR perfect season |
| Streak Keeper | 7-day streak | 30-day | 100-day | 200-day | 365-day | 1000-day OR unbroken 2 years |
| Helper | Help 5 peers | Help 50 | Help 200 | Help 1000 | Help 5000 | Help 25000 OR train 10 mentors |
| Domain Scholar | 1 domain at 50% | 3 at 50% | 5 at 50% | 3 at 80% | 5 at 80% | All domains at 80%+ |

#### 12.2.4 Badge Display Frame

| Frame Type | Unlock | Capacity | Special Feature |
|---|---|---|---|
| Row | Default | 5 badges | Simple horizontal display |
| Grid | 25 badges total | 12 badges (3x4) | Organized by family |
| Shield | 50 badges | 20 badges | Heraldic shield arrangement |
| Tree | Domain achievements | All domain badges | Badges hang on knowledge tree branches |
| Constellation | 100+ badges | 50 highlighted | Badges form constellation pattern |
| Trophy Wall | 200 badges | Unlimited (paginated) | Grand gallery, searchable |
| Legendary Frame | Mythic badge earned | All Mythic badges | Animated frame, special lighting |

#### 12.2.5 Animated Badges

| Animation | Trigger | Description |
|---|---|---|
| Idle Shimmer | Always (Diamond+) | Subtle light playing across badge surface |
| Pulse | On view | Badge gently pulses with light |
| Rotate | On hover | Badge rotates to show 3D depth |
| Particle Aura | Diamond+ | Tiny particles orbit around badge |
| Evolution Flash | When tier increases | Badge transforms with light flash |
| Sound Effect | On earn / on view | Distinct chime per tier (Diamond+), opt-in |
| Celebration Burst | On Mythic earn | Full screen particle celebration |

### 12.3 Progression Design

#### 12.3.1 Badge Unlock Flow

```
Activity Completed → Check Badge Criteria → Criteria Met?
     ↓ Yes                     ↓ No
  Trigger Badge Earn        Track Progress
     ↓
  Show Notification (tier-dependent animation)
     ↓
  Check for Evolution → Previous tier exists? → Evolve badge
     ↓ No
  Add to collection
     ↓
  Check Badge Family Bonus → Family milestones? → Apply bonus
```

#### 12.3.2 Badge Synergies

| Badge Pair | Synergy Effect |
|---|---|
| Quiz Master + Speed Demon | +10% quiz XP |
| Streak Keeper + Early Bird | +1 extra streak freeze/month |
| Helper + Teacher | +10% social XP |
| Domain Scholar + Curious Mind | +5% cross-domain discovery |
| Spiritual badges (all) | "Noor" profile effect unlocked |

### 12.4 Visual Design

| Element | Description |
|---|---|
| Badge Icon | Unique illustrated icon per badge, 64x64px default |
| Tier Overlay | Border/tint color matching tier, subtle gem texture |
| Animation | CSS/Canvas animation, tier-dependent complexity |
| Sound | Tier-dependent chime (optional), high-quality audio |
| 3D View | Diamond+ badges have 3D model on profile view |
| Flip Card | Hover reveals back face with details: earned date, stats |
| Group Display | Frame layout organizing badges by family/tier/date |
| Progress Ghost | Faded badge version showing progress to next tier |

### 12.5 Reward Structure

| Tier Unlock | Solo Reward | With All Badges in Family |
|---|---|---|
| Bronze | Badge icon | Family starter frame |
| Silver | +2% XP in domain | Family vanity effect |
| Gold | +5% XP, title fragment | Full family frame |
| Platinum | +8% XP, cosmetic | Family title (e.g., "Quiz Legend") |
| Diamond | +12% XP, exclusive cosmetic | Family animated frame |
| Mythic | +20% XP, Mythic title, frame | Mythic family aura |

### 12.6 Social Features

| Feature | Description |
|---|---|
| Badge Display | Choose which badge to display on profile |
| Badge Sharing | Share badge earn to social media (opt-in, no personal data) |
| Badge Respect | Acknowledge another's badge with "Masha'Allah" reaction |
| Badge Comparison | See friends' top badges (no competitive ranking) |
| Rarest Badge | Profile shows your rarest badge automatically |

### 12.7 Islamic Values Integration

| Value | Implementation |
|---|---|
| No Riya | Badge display default private; must opt-in to show publicly |
| Ihsan | Evolution system rewards sustained excellence, not one-time events |
| Tawadhu | No "badge count" leaderboards; focus is on knowledge, not collection |
| Barakah | Mythic badges have light/noor themes |
| Adab | Badge descriptions include Islamic context, not just gamified metrics |

### 12.8 Edge Cases

| Edge Case | Handling |
|---|---|
| Tier requirement changed | Existing badge holders keep their tier; new requirements apply going forward |
| Badge evolves from multiple paths | Pick the path with highest progress |
| Activity that earned badge undone | Badge may downgrade if reversal affects criteria permanently |
| Duplicate badge | Not possible — system checks for existing badge before awarding |
| Badge display performance | Lazy loading, frame limits on profile load |
| Notification overload | Badge notifications grouped, Mythic/Special priority |

### 12.9 Technical Requirements

| Requirement | Specification |
|---|---|
| Badge Definitions | PostgreSQL (badge_id, family, tiers, criteria JSON) |
| Learner Badges | PostgreSQL (badge_id, learner_id, tier, earned_date) |
| Evolution Check | Event-driven + scheduled weekly batch |
| Animation Assets | WebP sprites, lazy loaded, CDN |
| Sound Assets | MP3 (optional download), <100KB each |
| API | GET /badges, GET /badges/{learner_id}, POST /badges/display |
| Synergy Engine | Rule-based system checking badge combinations |

---

## 13. SEASON SYSTEM

### 13.1 Purpose

The Season System introduces cyclical thematic periods that keep the learning experience fresh, align with the Islamic calendar, and provide time-limited content, challenges, and rewards. Unlike the basic calendar, seasons affect every gamification system — from the Knowledge Tree's appearance to special quests and exclusive rewards.

### 13.2 Game Mechanics

#### 13.2.1 Season Types

| Season | Duration | Theme | Affected Systems |
|---|---|---|---|
| Spring of Knowledge | Mar 1 - May 31 | New growth, exploration, beginnings | Knowledge Tree (bloom), XP bonuses for new topics |
| Summer of Practice | Jun 1 - Aug 31 | Application, depth, challenge | Challenges (double rewards), mastery gain boost |
| Autumn of Reflection | Sep 1 - Nov 30 | Review, consolidation, harvest | Review efficiency, collection harvest events |
| Winter of Review | Dec 1 - Feb 28 | Strengthening foundations, revision | Spaced repetition bonus, pruning prevention |
| Ramadan Special | Ramadan 1 - Shawwal 1 | Spiritual focus, intensive learning | All bonuses active, spiritual rewards x2, special content |
| Eid Celebration | 3 days each Eid | Celebration, community | Triple XP, rare drops, community events |
| Special Events | Variable (1-2 weeks) | Platform milestones, partnerships | Event-specific mechanics |

#### 13.2.2 Seasonal Content

| Content Type | Examples | Availability |
|---|---|---|
| Seasonal Quests | "Spring Harvest: Master 5 new topics this week" | Every season |
| Seasonal Stories | "The Garden of Knowledge" (spring theme) | Exclusive to season |
| Seasonal Badges | "Spring Sprout" badge | Season-only earn |
| Seasonal Cosmetics | Cherry blossom profile frame | Season shop |
| Seasonal Collections | "Spring Flowers" collectible set | Season-limited |
| Seasonal Titles | "Spring Scholar" | Complete all spring quests |
| Seasonal Currency | Spring Leaves (spend in season shop) | Earn during season only |
| Seasonal Decor | Memory garden spring decorations | Season shop |

#### 13.2.3 Season Pass

| Pass Tier | Cost | Rewards | Duration |
|---|---|---|---|
| Free | 0 | Basic seasonal rewards (5-10 items) | Season length |
| Premium | 500 gems or equivalent | Full rewards (25+ items), exclusive content | Season length |
| Premium+ | 1500 gems | All Premium + exclusive cosmetic, title | Season length |

#### 13.2.4 Seasonal Progression

```
Season Start (Day 1)
     ↓
Season Pass Level 1 → 2 → 3 → ... → 50 (Free) / 100 (Premium)
     ↓
Each level requires Seasonal XP (earned through any learning activity)
     ↓
Seasonal XP = Base XP × Season Multiplier
     ↓
Multipliers: Free (1x), Premium (1.5x), Premium+ (2x)
     ↓
End of Season: Rewards calculated, unclaimed rewards lost
     ↓
Season Leaderboard: Progress-based (not competitive), anonymized
```

### 13.3 Progression Design

#### 13.3.1 Seasonal Level Rewards (Sample)

| Level | Free Reward | Premium Reward |
|---|---|---|
| 1 | 100 XP, Season greeting card | Season profile frame |
| 2 | 50 coins | Season spirit accessory |
| 3 | 1 gem | Season avatar cosmetic |
| 5 | Season badge (Bronze) | Season badge (Silver) |
| 10 | 200 XP | Season collection item |
| 15 | 2 gems | Season exclusive story |
| 20 | Season badge (Gold) | Season badge (Platinum) |
| 25 | Season title fragment | Season title |
| 30 | 500 XP | Rare season cosmetic |
| 40 | 5 gems | Season pet companion |
| 50 | Season badge (Diamond) | Season badge (Diamond) + frame |
| 75 | — | Exclusive season mount/effect |
| 100 | — | Mythic season badge, legacy item |

#### 13.3.2 Season Reset Mechanics

| System | Reset Behavior | Persists |
|---|---|---|
| Season Pass Progress | Full reset | Seasonal achievements, badges, titles |
| Seasonal Currency | Converted to standard currency (50% rate) | — |
| Seasonal Leaderboard | Full reset | Best seasonal rank recorded on profile |
| Seasonal Cosmetics | Remain in inventory | Always usable |
| Seasonal Quests | Full reset | Progress lost |
| Seasonal Badges | Badge earned remains | Cannot earn same badge again |
| Seasonal XP Boost | Reset to base | — |

### 13.4 Visual Design (Seasonal Themes)

| Element | Spring | Summer | Autumn | Winter | Ramadan |
|---|---|---|---|---|---|
| UI Theme | Pastel greens, flowers | Warm golds, sun | Oranges, browns, falling leaves | Cool blues, snowflakes | Gold, noor white, crescent moons |
| Animation | Floating petals | Heat shimmer, sun rays | Falling leaves, wind | Snow falling, frost | Light rays, star trails |
| Music | Uplifting, flute | Rhythmic, lively | Melancholic, strings | Calm, minimalist | Soulful, Quranic nasheed |
| Knowledge Tree | Green bloom | Full canopy | Gold/orange leaves | Bare, frost-tipped | Noor glow |
| Memory Garden | Spring flowers | Full bloom | Harvest ready | Dormant/evergreens | Light over garden |
| Daily Journey | Meadow path | Sunlit trail | Leaf-covered road | Snowy path | Celestial path |
| Avatar | Light clothing | Summer attire | Autumn robes | Winter cloak | Noor aura |
| Spirit | Energetic | Playful | Contemplative | Sleepy/cozy | Glowing, reverent |

### 13.5 Reward Structure

| Season Complete | Free Tier | Premium Tier |
|---|---|---|
| All quests done | 1000 XP, season badge | 5000 XP, season badge (evolved) |
| Level 50 reached | Season title fragment | Full season title |
| Collection complete | Season decor | Season mythic item |
| Leaderboard top (opt-in) | Recognition badge | Special frame |

### 13.6 Social Features

| Feature | Description |
|---|---|
| Season Party | Join with friends, track progress together |
| Season Trade | Trade seasonal items within season window |
| Season Guild Goal | Guild-level season pass with guild rewards |
| Season Share | Share season progress (anonymized) |
| Season Showcase | Display completed season badges |
| Legacy Season | Past seasons viewable in archive (items, badges, titles) |

### 13.7 Islamic Values Integration

| Value | Implementation |
|---|---|
| Islamic Calendar | Seasons align with Islamic months, not just Gregorian |
| Ramadan Focus | Ramadan season is the most spiritually rich, not just game-rich |
| Reflection | Autumn is themed for reflection (tafakkur) |
| Gratitude | Seasonal harvest prompts shukr |
| Balance | Winter is for rest — seasonal mechanics encourage balance, not burnout |
| Barakah | Special barakah mechanics during blessed months (Rajab, Sha'ban, Ramadan, Dhul Hijjah) |

### 13.8 Edge Cases

| Edge Case | Handling |
|---|---|
| Learner joins mid-season | Progress doesn't start from zero — XP earned gets partial credit |
| Season ends while learner active | Grace period: 24h to claim rewards, progress stops |
| Ramadan starts in different seasons | Ramadan season overrides visual theme |
| Learner in different hemisphere | Seasons adapt to learner's geographic location (inverse for southern hemisphere) |
| Season pass not purchased | Free tier still provides meaningful rewards |
| Premium pass expiring | Rewards claimable for 7 days after season end |
| Learner misses entire season | Season items may return next year or in legacy shop |

### 13.9 Technical Requirements

| Requirement | Specification |
|---|---|
| Season Schedule | Cron-based, global config in PostgreSQL |
| Season Pass | Per-learner progress in Redis (hot), PostgreSQL (cold) |
| Seasonal Content | Content tagged with season_id, filtered by active season |
| Season Transition | Graceful: 24h warning, automated transition at season end |
| API | GET /seasons/active, GET /seasons/{season_id}/progress, POST /seasonpass/purchase |
| Leaderboard | Redis sorted set per season, anonymous IDs |
| Currency Conversion | Batch job at season end, 50% conversion rate |

---

## 14. GUILD SYSTEM

### 14.1 Purpose

The Guild System (extending 19_Social_Learning.md study groups) transforms ad-hoc groups into persistent, thriving communities with their own identity, progression, economy, and challenges. Guilds provide belonging, accountability, and collaborative learning at a scale beyond small study groups, reflecting the Islamic tradition of organized circles of knowledge (halaqat).

### 14.2 Game Mechanics

#### 14.2.1 Guild Structure

| Role | Permissions | Limit |
|---|---|---|
| Guild Leader | Full control, disband, promote, war declaration | 1 |
| Guild Officers | Manage members, approve quests, treasury | Up to 5 |
| Guild Mentors | Guide new members, create content | Up to 10 |
| Guild Scholars | Knowledge verification, content review | Up to 5 |
| Guild Members | Participate, quests, chat, trade | Unlimited |
| Initiate | Trial member, limited access | No limit |
| Alumni | Former member, honorary status | No limit |

#### 14.2.2 Guild Creation

| Requirement | Value |
|---|---|
| Minimum Learner Rank | 13 (Faqih) |
| Cost | 100,000 coins + 100 gems |
| Cooldown | Cannot create another guild for 90 days |
| Name | 3-20 characters, Arabic/Latin, no offensive terms |
| Tag | 2-5 character guild tag |
| Theme | Choose from 10 guild themes |
| Specialty | Optional: choose focus domain (Aqidah, Fiqh, etc.) |
| Founding Members | Need 5 co-founders with Rank 10+ |

#### 14.2.3 Guild Progression

| Guild Level | XP Required | Member Cap | Perks |
|---|---|---|---|
| 1 | 0 | 20 | Basic guild chat, guild hall |
| 2 | 10,000 | 30 | Guild quests unlocked |
| 3 | 50,000 | 40 | Guild treasury, officer slots +1 |
| 4 | 150,000 | 50 | Guild mentor system, shared garden |
| 5 | 300,000 | 60 | Guild vs Guild unlocked |
| 6 | 500,000 | 75 | Guild shop, mentor slots +1 |
| 7 | 1,000,000 | 100 | Guild hall expansion, custom theme |
| 8 | 2,000,000 | 150 | Guild fortress, scholar roles |
| 9 | 5,000,000 | 200 | Guild events, global recognition |
| 10 | 10,000,000 | 300 | Legendary guild, mythic hall, custom emblem animation |

#### 14.2.4 Guild Hall

| Hall Feature | Unlock Level | Description |
|---|---|---|
| Main Hall | 1 | Gathering area, bulletin board, member list |
| Library | 2 | Shared resources, recommended content, member notes |
| Training Grounds | 3 | Guild-only challenges, practice arena |
| Garden | 4 | Shared memory garden (all members contribute) |
| Treasury | 4 | Guild currency, donations, shared items |
| War Room | 5 | GvG planning, strategy, battle history |
| Museum | 6 | Display guild achievements, rare finds |
| Mentor Wing | 6 | Mentorship area, training programs |
| Expansion Wing | 7 | Customizable hall space |
| Fortress | 8 | Defensive structures, prestige displays |
| Grand Hall | 9 | Ceremony space, celebration area |
| Mythic Sanctum | 10 | Legendary guild only — exclusive area |

#### 14.2.5 Guild Quests

| Quest Type | Duration | Members Required | Example |
|---|---|---|---|
| Daily Guild Quest | 24h | 3+ | "3 members complete their reviews" |
| Weekly Guild Challenge | 7d | 10+ | "Collectively master 20 facts this week" |
| Guild Expedition | 14d | 15+ | "Complete a Knowledge Expedition as a guild" |
| Guild Boss | Variable | 20+ | "Defeat the Knowledge Boss by answering questions" |
| Guild Project | 30d | 5+ | "Create a shared flashcard deck on a topic" |
| Guild War | 48h | All | GvG battle (see below) |

### 14.3 Progression Design

#### 14.3.1 Guild vs Guild

| System | Description |
|---|---|
| Matchmaking | Guild level ±3, similar member count, opt-in |
| Battle Format | 48-hour knowledge challenge |
| Battle Types | Quiz War (accuracy), Knowledge Race (speed), Memory Duel (retention) |
| Scoring | Points per correct answer, bonus for streaks, combos |
| Victory Rewards | GvG points, special currency, guild XP, temporary buff |
| Defeat Rewards | Reduced GvG points, no penalty, "well fought" bonus |
| GvG Ranking | Seasonal GvG leaderboard (guild level only, anonymized) |
| Peace Mode | Guild can opt out of GvG for up to 14 days |

#### 14.3.2 Guild Mentorship

| System | Description |
|---|---|
| Mentor Assignment | High-rank members assigned to new members |
| Mentorship Goals | Help mentee reach rank milestones |
| Mentor Rewards | Guild XP, special mentor currency, mentor title |
| Mentee Rewards | Guidance bonus: +10% XP for first 30 days |
| Mentor/Mentee Ratio | 1 mentor per max 3 mentees |
| Mentor Recognition | "Mentor of the Month" in guild hall |

### 14.4 Visual Design

| Element | Description |
|---|---|
| Guild Emblem | Customizable crest (shape, color, icon, border) |
| Guild Hall | 2D/3D shared space, expandable |
| Guild Banner | Displayed on member profiles, guild hall |
| Member Titles | Role shown beneath name in guild spaces |
| Guild Rank Badge | Guild level displayed on emblem |
| Guild Theme | Color scheme, decoration style |
| Guild Events | Visual markers in guild hall (banners, trophies) |
| GvG Scoreboard | Encounter overview, not direct comparison |

### 14.5 Reward Structure

| Guild Level | Rewards |
|---|---|
| Level up | Guild XP, unlock feature, member capacity increase |
| Weekly top guild (seasonal) | Seasonal guild title, special emblem |
| GvG victory | War chest (guild currency), victory banner |
| Guild milestone (e.g., 100 members) | Achievement, guild cosmetic |
| Guild anniversary | Legacy badge, celebration event |

### 14.6 Social Features

| Feature | Description |
|---|---|
| Guild Chat | Real-time guild communication |
| Guild Notice Board | Announcements, events, quests |
| Guild Calendar | Scheduled events, battles |
| Member Search | Find guilds by specialty, language, activity |
| Guild Alliance | Two guilds can form alliance for GvG |
| Guild Hopping | 7-day cooldown between guild changes |
| Guild Applications | Apply with message, rank requirement |
| Guild Invitations | Officers can invite, accept applications |

### 14.7 Islamic Values Integration

| Value | Implementation |
|---|---|
| Jama'ah | Guild reflects Islamic concept of community (jama'ah) |
| Shura | Guild decisions made through consultation (officer voting) |
| Taa'wun | Cooperation in righteousness — guild goals are beneficial |
| Adab | Guild chat moderation, no backbiting, respectful discourse |
| Ukhuwwah | Brotherhood/sisterhood bonds, support for struggling members |
| Amal Jami'i | Collective action rewarded, not just individual achievement |

### 14.8 Edge Cases

| Edge Case | Handling |
|---|---|
| Guild leader inactive for 30 days | Leadership transfers to most active officer |
| Toxic guild behavior | Report system, warning, temporary lock, disband for severe violations |
| Guild member limit reached | Expansion queue, waiting list |
| Guild disband | Members get "Guildless" status (7d guild creation cooldown waived) |
| Cross-language guild | Language tag required, auto-translate for announcements |
| Guild hoarding | Treasury capped per level, excess distributed weekly |
| Member wants privacy | "Ghost mode" — invisible in guild lists, still participates |

### 14.9 Technical Requirements

| Requirement | Specification |
|---|---|
| Guild Data | PostgreSQL (guild_id, name, level, xp, treasury, settings) |
| Membership | PostgreSQL (guild_id, learner_id, role, joined_date) |
| Guild Hall | Per-guild state, rendered client-side |
| GvG Matchmaking | Redis-based matching queue, scheduled battles |
| Chat | WebSocket, message history in PostgreSQL (90 days) |
| API | GET /guild/{guild_id}, POST /guild/create, POST /guild/quest, POST /gvg/declare |
| Event Subscriptions | member_join, member_leave, level_up, quest_complete |

---

## 15. STUDY ROOMS

### 15.1 Purpose

Study Rooms provide immersive virtual spaces where learners can study alone or with others. Unlike the guild hall (social/administrative), study rooms are focused on the act of learning — they provide themed environments designed to enhance concentration, collaboration, and the joy of studying. They connect directly to the 19_Social_Learning.md study group system.

### 15.2 Game Mechanics

#### 15.2.1 Room Themes

| Theme | Atmosphere | Best For | Capacity |
|---|---|---|---|
| Masjid | Calm, echoing, soft carpet textures, mihrab visible | Quran study, spiritual focus | 1-20 |
| Library | Warm wood, shelves of books, reading lamps, paper sounds | General study, research | 1-50 |
| Garden | Open air, birdsong, flowing water, trees and flowers | Light study, reflection, memorization | 1-30 |
| Observatory | Night sky, stars, telescopes, cosmic ambience | Deep thinking, conceptual learning | 1-15 |
| Study Circle (Halaqah) | Cushions on floor, central teacher spot, warm lighting | Group learning, discussion | 5-30 |
| Desert Camp | Night desert, campfire, storytelling atmosphere | Seerah, stories, narratives | 1-20 |
| Mountain Retreat | High altitude, mist, solitude, minimal distractions | Focus sessions, exams, intense study | 1-10 |
| Garden of Paradise | Described in Quran — rivers, shade, beauty | Reward-based, high achievement | 1-50 |

#### 15.2.2 Room Customization

| Category | Options | Unlock |
|---|---|---|
| Lighting | Warm, cool, candle, moonlight, dawn | Level 5+ |
| Furniture | Desk types, seating, shelves, decorations | Level 10+, collection |
| Soundscape | Nature sounds, nasheed (instrumental), white noise, silence | Achievement, shop |
| Time of Day | Fixed or real-time | Level 15+ |
| Fragrance (UI) | Visual fragrance indicators (oud, rose, musk) | Ramadan reward |
| Wall Art | Islamic calligraphy, nature scenes, abstract | Collection completion |
| Books | Virtual books on shelves (show completed content) | Mastery milestones |
| View | Window view options (garden, city, sea, mountains, space) | Premium |

#### 15.2.3 Focus Mode

| Feature | Description |
|---|---|
| Active Focus | Room dims, only learning content visible, timer starts |
| Pomodoro | Built-in 25/5 timer with room-appropriate chimes |
| Focus Score | Calculated from distractions avoided, time focused |
| Distraction Block | Optional: block notifications during focus |
| Focus Streak | Consecutive completed focus sessions |
| Group Focus | All group members in focus simultaneously (shared timer) |
| Rewards | Focus score converts to XP bonus at session end |

#### 15.2.4 Room Hopping

| System | Description |
|---|---|
| Public Rooms | List of open rooms (theme, occupant count, topic) |
| Room Code | Private rooms have 6-digit code for access |
| Random Room | "Surprise Me" — join a random public room |
| Friend Rooms | See friends currently in rooms (opt-in) |
| Popular Rooms | Trending rooms (no personal data) |
| Room Hop Cooldown | 10 seconds between room changes |

### 15.3 Progression Design

#### 15.3.1 Room Level (Personal Study Room)

| Level | XP in Room | Unlocks |
|---|---|---|
| 1 | 0 | Basic room (Masjid or Library), lighting control |
| 2 | 500 | Soundscape selection |
| 3 | 2000 | Custom furniture placement |
| 4 | 5000 | Guest capacity +5 |
| 5 | 10000 | Window view unlock |
| 6 | 20000 | Focus mode, Pomodoro |
| 7 | 50000 | Wall art display |
| 8 | 100000 | Fragrance (visual) |
| 9 | 200000 | Custom room music |
| 10 | 500000 | Full room design control, infinite capacity |

#### 15.3.2 Room Achievements

| Achievement | Criteria | Reward |
|---|---|---|
| Room Decorator | Customize 10 room elements | "Interior Designer" badge |
| Focus Master | 100 hours in focus mode | "Focus Master" title |
| Host with the Most | Host 50 group sessions | "Host" badge |
| Quiet Zone | 30 days of solitary study | "Solitary Scholar" title |
| Pomodoro Pro | 500 completed Pomodoros | Productivity set reward |

### 15.4 Visual Design

| Element | Description |
|---|---|
| Renderer | 2.5D isometric or 3D first-person (configurable) |
| Art Style | Warm, detailed, stylized realism |
| Lighting | Dynamic, adjustable, time-of-day aware |
| Sound | Spatial audio, room-appropriate ambient sounds |
| Avatars | Learner avatars displayed in-room, seated at desks |
| Animation | Minor room animations (pages turning, fire flickering, water flowing) |
| UI | Minimal, context-sensitive toolbars |
| Accessibility | High-contrast mode, text descriptions of rooms |

### 15.5 Reward Structure

| Achievement | Reward |
|---|---|
| First Room Entered | 50 XP, basic room customization |
| Room Level 5 | 500 XP, window view unlocked |
| Room Level 10 | 2000 XP, "Master of Space" title |
| 100 Hours Focus | "Deep Focus" title, focus aura cosmetic |
| Host 50 Sessions | "Host" badge, exclusive room decoration |

### 15.6 Social Features

| Feature | Description |
|---|---|
| Room Visit | Join friends in their study room |
| Co-Study | Study side-by-side in same room (synchronized or parallel) |
| Room Chat | Text/voice chat within room (opt-in, moderated) |
| Quiet Zone | No chat allowed — pure co-study |
| Discussion Area | Separated zone within room for Q&A |
| Room Favorites | Bookmark preferred rooms |
| Study Together | Group Pomodoro with shared breaks |

### 15.7 Islamic Values Integration

| Value | Implementation |
|---|---|
| Adab in Spaces | Room etiquette reminders, no inappropriate decoration |
| Quiet Study | Islamic emphasis on focused learning (tadabbur) |
| Halaqah Spirit | Study circle room mimics traditional learning format |
| Sacred Spaces | Masjid room has additional adab requirements |
| Community | Rooms bring learners together in beneficial gathering |

### 15.8 Edge Cases

| Edge Case | Handling |
|---|---|
| Room full | Join queue, notify when space opens |
| Inappropriate behavior in room | Report system, auto-kick for violations, moderation |
| Room empty | AI study buddy available (optional) |
| Privacy concerns | Room can be set to private, invite-only, or public |
| Audio/voice chat issues | Text-only fallback mode |
| Room customization lost | Auto-save on change, version history for rollback |
| Cross-device room access | Room state synchronized across learner's devices |

### 15.9 Technical Requirements

| Requirement | Specification |
|---|---|
| Room State | Redis (active rooms, occupants, room state) |
| Room Data | PostgreSQL (room config, themes, ownership) |
| Real-time Sync | WebSocket for avatar positions, room state |
| Voice Chat | WebRTC, opt-in, peer-to-peer or relay for groups > 8 |
| Rendering | Three.js (3D) or Canvas 2D (2.5D), configurable |
| API | GET /rooms, POST /rooms/create, POST /rooms/join, POST /rooms/customize |
| Moderation | Real-time content filtering, report system, auto-moderation AI |

---

## 16. KNOWLEDGE EXPEDITIONS

### 16.1 Purpose

Knowledge Expeditions are themed, multi-stage learning journeys that combine multiple content types, challenges, and collaborative elements into a cohesive narrative experience. Unlike regular quests (which are task lists), expeditions tell a story, have a clear beginning/middle/end, and culminate in a "boss challenge" that tests everything learned along the way.

### 16.2 Game Mechanics

#### 16.2.1 Expedition Structure

| Component | Description | Duration |
|---|---|---|
| Expedition Map | Visual journey showing all stages, progress, and secret paths | Always visible |
| Starting Point | Briefing, context setting, team formation | 5 min |
| Stages | 5-10 sequential learning challenges | 3-5 min each |
| Rest Points | Checkpoints with reflection, rewards, break | 2 min |
| Side Quests | Optional challenges within expedition | 5 min each |
| Boss Stage | Ultimate challenge testing all expedition knowledge | 10-15 min |
| Completion | Celebration, rewards, expedition summary | 2 min |

#### 16.2.2 Expedition Types

| Type | Theme | Example | Best Group Size |
|---|---|---|---|
| Historical Journey | Follow a narrative through Islamic history | "The Seerah Expedition: From Birth to Hijrah" | 1-5 |
| Domain Deep Dive | Deep exploration of a single domain | "Tawheed Treasure Hunt" | 1-3 |
| Cross-Domain Quest | Connect concepts across domains | "The Fiqh-Seerah Bridge" | 2-5 |
| Seasonal Expedition | Time-limited, season-themed | "Ramadan Lights Expedition" | 1-10 |
| Guild Expedition | Guild-only, high difficulty | "Conquest of Knowledge" | 5-20 |
| Solo Expedition | Single-learner, introspective | "Reflection Journey" | 1 |

#### 16.2.3 Expedition Stages

| Stage Type | Activity | XP Reward | Special Mechanics |
|---|---|---|---|
| Knowledge Gate | Quiz (pass to proceed) | 100 XP | Gate closes if failed, need 80% to proceed |
| Story Scroll | Read/listen to narrative content | 50 XP | Comprehension check at end |
| Puzzle Room | Solve knowledge puzzle | 150 XP | Multi-step, time bonus |
| Memory Trial | Recall challenge (no hints) | 200 XP | Streak multiplier active |
| Skill Challenge | Apply knowledge (scenario) | 250 XP | No mistakes allowed (3 lives) |
| Rest Point | Reflect, plan, encourage team | 25 XP | Group buff for next stage |
| Hidden Path | Optional discovery stage | 300 XP | Secret exit or bonus boss |
| Boss Stage | Comprehensive challenge | 500-2000 XP | All mechanics combined |

### 16.3 Progression Design

#### 16.3.1 Expedition Progression

| Expedition Level | Difficulty | Prerequisites | Max Rewards |
|---|---|---|---|
| Novice | Easy | None | 1000 XP, common items |
| Apprentice | Medium | Rank 5+ | 2500 XP, rare items |
| Adept | Hard | Rank 10+ | 5000 XP, epic items |
| Master | Very Hard | Rank 15+ | 10000 XP, legendary items |
| Mythic | Extreme | Rank 18+ + special key | 25000 XP, mythic items |

#### 16.3.2 Expedition Replay

| Replay Feature | Description |
|---|---|
| New Game+ | Replay at higher difficulty, harder boss, better rewards |
| Speed Run Mode | Timer active, leaderboard for completion time |
| Perfection Mode | Must complete all stages without any mistake |
| Explorer Mode | Find all hidden paths, secrets |
| Teaching Mode | Lead new learners through as a guide |

### 16.4 Visual Design

| Element | Description |
|---|---|
| Expedition Map | Illustrated journey map with clear path, similar to Daily Journey but multi-day |
| Stage Icons | Unique icon per expedition stage type |
| Boss Arena | Grand visual for boss stage (stadium, mountain top, throne room) |
| Party Display | All team members' avatars shown together on map |
| Progress Flag | Marker showing current stage, checkpoints |
| Weather/Time | Matches expedition theme (night for Seerah, bright for Tawheed) |
| Completion Screen | Victory animation, stats, group photo frame |

### 16.5 Reward Structure

| Completion Level | Rewards |
|---|---|
| Expedition Complete (any) | XP, expedition badge, collection item |
| All Stages Perfect | Bonus XP, "Perfect Expedition" badge |
| All Secrets Found | Hidden collection item, secret title |
| Speed Run Record | "Speed Runner" badge (seasonal) |
| Team Expedition (all members complete) | Group title, guild XP bonus |
| Expedition Master (all difficulty levels) | "Expedition Master" title, mythic frame |

### 16.6 Social Features

| Feature | Description |
|---|---|
| Team Formation | Create or join expedition team (matchmaking available) |
| Team Chat | Expedition-specific chat channel |
| Progress Sync | See teammates' progress on shared map |
| Assist Mechanic | Team members can "help" on a stage (once per expedition) |
| Team Photo | Expedition completion group photo |
| Expedition Log | Share expedition story (opt-in, anonymized possible) |

### 16.7 Islamic Values Integration

| Value | Implementation |
|---|---|
| Journey for Knowledge | Expedition reflects Islamic tradition of traveling for knowledge |
| Teamwork | Cooperation (ta'awun) emphasized over competition |
| Patience | Multi-stage format teaches sabr |
| Reflection | Rest points include reflective prompts |
| Storytelling | Expedition narratives engage Islamic storytelling tradition |

### 16.8 Edge Cases

| Edge Case | Handling |
|---|---|
| Team member drops out mid-expedition | Progress saved, can rejoin within 24h, AI fills slot if needed |
| Expedition takes too long | Pause/resume system, save state at each stage |
| Boss too difficult | "Retry" with reduced difficulty after 3 failures |
| Solo player wants expedition | AI companions fill team slots (optional) |
| Expedition expired (seasonal) | Becomes available in "Legacy Expeditions" (no time pressure) |
| Team members in different timezones | Asynchronous expedition mode (each member does stages independently) |

### 16.9 Technical Requirements

| Requirement | Specification |
|---|---|
| Expedition Data | PostgreSQL (expedition_id, stages JSON, rewards, prerequisites) |
| Expedition State | Redis (per-team or per-learner expedition progress) |
| Team Management | PostgreSQL (team_id, members, roles, expedition_id) |
| Matchmaking | Redis-based queue for expedition team formation |
| API | GET /expeditions, POST /expeditions/start, POST /expeditions/stage/complete, GET /expeditions/{id}/team |
| Event Subscriptions | expedition_start, stage_complete, expedition_complete, team_join, team_leave |

---

## 17. HIDDEN DISCOVERIES

### 17.1 Purpose

Hidden Discoveries (Easter eggs) add an element of surprise and delight to the learning experience. They reward curiosity, exploration, and attention to detail. Found in content, UI, interactions, and unexpected places, these discoveries create memorable moments that make the platform feel alive and treasure-filled.

### 17.2 Game Mechanics

#### 17.2.1 Discovery Types

| Type | Description | Example Trigger |
|---|---|---|
| Content Easter Eggs | Hidden within stories, quizzes, podcasts | Story character mentions a secret code word |
| UI Easter Eggs | Hidden in interface interactions | Click the compass rose 5 times |
| Timing Easter Eggs | Triggered at specific times | Open the app at exactly Fajr time |
| Sequence Easter Eggs | Specific action sequence | Read 3 specific stories in order |
| Combination Easter Eggs | Multiple conditions met simultaneously | Same time + specific content + streak active |
| Social Easter Eggs | Group interaction triggers | Two learners quiz each other on same topic simultaneously |
| Exploration Easter Eggs | Found in unexpected navigation paths | Visit every section of settings page |

#### 17.2.2 Discovery Rarity

| Rarity | Learners Who Find It | Discovery Method | Example |
|---|---|---|---|
| Common | >20% | Obvious UI interaction | Click the spirit 3 times |
| Uncommon | 10-20% | Content attention | Notice the hidden message in a story's illustration |
| Rare | 3-10% | Timing or sequence | Read Surah Al-Fatiha at Maghrib time |
| Epic | 1-3% | Complex combination | Complete 3 specific reviews in alphabetical order at Dhuhr |
| Legendary | 0.1-1% | Extremely obscure | Find all 10 hidden ayat references in 10 different stories |
| Mythic | <0.1% | Community effort | First 100 learners to complete a global challenge |

#### 17.2.3 Discovery Triggers

| Trigger Category | Specific Triggers | Example |
|---|---|---|
| Timing | Prayer times, specific days, anniversaries | "Night of Power" discovery during last 10 nights of Ramadan |
| Content | Reading specific passages, completing specific content | "The Hidden Ayah" — find a verse referenced indirectly |
| Interaction | Specific UI actions, sequences | "Tasbih" — click the screen 33 times |
| Social | Group actions, peer interactions | "Echo" — two learners answer same question identically |
| Achievement | Reaching milestones with specific conditions | "Zero to Hero" — go from 0 to mastery in a domain in one session |
| Random | Small percentage chance after certain actions | "Blessing" — 0.1% chance on daily login |

### 17.3 Progression Design

#### 17.3.1 Discovery Log

| Feature | Description |
|---|---|
| Discovery Gallery | All discoveries found, organized by category |
| Discovery Date | When the discovery was made |
| Discovery Hint | Cryptic clue for unfound discoveries (optional, after 80% criteria met) |
| Discovery Rarity | Displayed with color coding |
| Discovery Story | Brief narrative of what was discovered |
| Discovery Count | Total discoveries found (private) |
| Nearest Discovery | Hint for next closest discoverable easter egg |

#### 17.3.2 Discovery Families

| Family | Theme | Number | Final Reward |
|---|---|---|---|
| Celestial Signs | Discoveries related to cosmic events/time | 10 | "Stargazer" title |
| Hidden Ayat | Quranic references hidden in content | 30 | "Quran Explorer" title |
| Scholar Secrets | Historical facts about scholars | 20 | "History Uncovered" title |
| Nature Signs | Reflections on Allah's creation | 15 | "Contemplator" title |
| Lost Stories | Forgotten Islamic narratives | 12 | "Story Keeper" title |

### 17.4 Visual Design

| Element | Description |
|---|---|
| Discovery Notification | Subtle glow, icon appears, "Discovery Made" with sparkle |
| Discovery Popup | Card showing: discovery name, description, rarity, trigger hint |
| Discovery Icon | Unique icon per discovery (compass, key, star, lantern, book) |
| Discovery Gallery | Bookshelf or treasure room display |
| Unfound Hints | Blurred shapes with question marks, cryptic clues |
| Rare Discovery Effect | Full screen animation for Epic+ discoveries |
| Sound | Pleasant chime on discovery (varies by rarity) |

### 17.5 Reward Structure

| Rarity | XP | Coins | Special |
|---|---|---|---|
| Common | 25 | 10 | Discovery log entry |
| Uncommon | 75 | 50 | Small cosmetic |
| Rare | 200 | 200 | Title fragment |
| Epic | 500 | 1000 | Discovery-themed cosmetic |
| Legendary | 2000 | 5000 | Legendary title |
| Mythic | 10000+ | 25000 | Mythic item, profile frame, special spirit interaction |

### 17.6 Social Features

| Feature | Description |
|---|---|
| Discovery Sharing | Share discovery (without spoilers) to friends |
| Discovery Hints | Friends can give each other cryptic hints |
| Discovery Race | Who finds it first (no leaderboard, just fun) |
| Discovery Museum | Gallery of all platform discoveries (anonymized) |
| Discovery News | "New discovery found!" notification (no personal info) |

### 17.7 Islamic Values Integration

| Value | Implementation |
|---|---|
| Curiosity | Encourages exploration and wonder at Allah's creation |
| Tajalli | Discoveries reflect the beauty of knowledge unfolding |
| Barakah | Surprise discoveries feel like unexpected blessings |
| Reflection | Discovery descriptions include reflective prompts |
| Gratitude | Shukr prompt after significant discoveries |

### 17.8 Edge Cases

| Edge Case | Handling |
|---|---|
| Discovery trigger condition no longer exists | Flagged as "Legacy Discovery" — still counts if found |
| Discovery found unintentionally | Still valid — no penalty for accidental discovery |
| Discovery hint spoiled | "Spoiler Mode" toggle — hide all hints |
| Multiple discoveries at once | Stacked notifications, priority by rarity |
| Discovery too difficult for age group | Age-appropriate discovery hints for under-13 |
| Platform update removes discovery | Legacy badge awarded to those who found it |

### 17.9 Technical Requirements

| Requirement | Specification |
|---|---|
| Discovery Definitions | PostgreSQL (discovery_id, trigger_conditions JSON, rarity) |
| Learner Discoveries | PostgreSQL (learner_id, discovery_id, found_date) |
| Trigger Engine | Event-driven: check all discovery triggers on every event |
| Hint System | Server-side, cryptic hint generation based on learner progress |
| API | GET /discoveries, GET /discoveries/{learner_id} |
| Trigger Check | Efficient: indexed event types, batch check when idle |
| Sound Assets | Preloaded discovery chimes, rarity-based |

---

## 18. MASTER CHALLENGES

### 18.1 Purpose

Master Challenges are the ultimate test of knowledge — comprehensive, multi-domain assessments that push learners to their limits. They are to normal quizzes what a final exam is to a pop quiz. These challenges are designed to be genuinely difficult, requiring deep understanding, quick thinking, and complete mastery.

### 18.2 Game Mechanics

#### 18.2.1 Challenge Structure

| Component | Description |
|---|---|
| Challenge Name | Unique name with Arabic/English |
| Domain Coverage | 3-5 domains tested |
| Duration | 15-60 minutes (depending on tier) |
| Question Count | 20-100 questions |
| Question Types | Multiple choice, fill-in-blank, matching, ordering, free-response |
| Difficulty Curve | Easy → Medium → Hard → Expert within the challenge |
| Special Rules | Vary per challenge (see below) |
| Passing Score | 80% minimum to pass |
| Perfect Score | 100% — extremely rare |

#### 18.2.2 Challenge Tiers

| Tier | Domains | Questions | Time | Special Rules | Entry Requirement |
|---|---|---|---|---|---|
| Apprentice | 2 | 20 | 15 min | None | Rank 5+ |
| Scholar | 3 | 40 | 30 min | No hints, no backtracking | Rank 10+ |
| Master | 4 | 60 | 45 min | No mistakes allowed (3 lives), timed | Rank 15+ |
| Grand Master | 5 | 80 | 60 min | All of above + speed bonus, no pause | Rank 18+ + key |
| Ultimate | All | 100 | 60 min | No mistakes, speed bonus, adaptive difficulty, no hints, no pause, random order | Invitation only |

#### 18.2.3 Special Rules

| Rule | Description | Applied In |
|---|---|---|
| No Backtracking | Cannot return to previous questions | Scholar+ |
| No Mistakes | One wrong answer = challenge ends (3 lives in Master) | Master+ |
| Timed | Strict timer, unanswered = wrong | All tiers |
| Speed Bonus | Extra points for answering quickly | Master+ |
| Adaptive Difficulty | Questions get harder if you answer correctly, easier if wrong | Ultimate |
| No Hints | No AI Teacher help available | Master+ |
| Random Order | Questions shuffled uniquely per attempt | Grand Master+ |
| Free Response | No multiple choice — type answers | Ultimate |
| Cascade | Wrong answer reveals a related weakness question | Ultimate |

#### 18.2.4 Boss Challenge System

Inspired by Knowledge Expeditions, some Master Challenges feature a "Boss" — a grand cumulative question that requires synthesis of all challenge knowledge:

| Boss Type | Description | Example |
|---|---|---|
| Synthesis Question | Combine knowledge from multiple domains | "How does Tawheed influence the practice of Salah?" |
| Scenario Analysis | Apply knowledge to complex scenario | "A traveler misses Fajr due to unavoidable circumstances. What are their options?" |
| Knowledge Web | Connect given concepts in a meaningful way | Create a mind map connecting 5 given concepts |
| Teaching Demo | Explain concept as if teaching a beginner | "Explain the concept of Qadr to someone who has never studied it" |
| Creative Application | Apply knowledge creatively | "Design a community program based on Islamic ethics" |

### 18.3 Progression Design

#### 18.3.1 Challenge Unlock Flow

```
Learner reaches required rank
     ↓
Challenge becomes visible (locked with requirement details)
     ↓
Learner meets additional requirements (domain mastery, etc.)
     ↓
Challenge unlocks
     ↓
Learner can attempt (1 free attempt/day, more with challenge tokens)
     ↓
Attempt challenge → Pass/Fail
     ↓
If pass: Rewards, Hall of Fame entry, next challenge unlocked
     ↓
If fail: Cooldown (24h for retry), review suggested weak areas
```

#### 18.3.2 Hall of Fame

| Element | Description |
|---|---|
| Fastest Times | Quickest completions (anonymized) |
| Perfect Scores | Learners who scored 100% (anonymized) |
| First to Complete | Chronological order of first completions (anonymized) |
| Most Improved | Learners who improved most between attempts (anonymized) |
| Streak Masters | Learners who completed all challenges in a row (anonymized) |
| Legacy Champions | First 100 to complete Ultimate challenge (special badge) |

#### 18.3.3 Annual Reset

| System | Reset Behavior |
|---|---|
| Challenge Attempts | Reset annually, fresh attempts granted |
| Hall of Fame | Previous year's entries archived, new year starts fresh |
| Rewards | New annual rewards added, previous year's become legacy |
| Difficulty | Slight recalibration based on aggregate performance data |
| Leaderboard | Fresh season, previous year's standings archived |

### 18.4 Visual Design

| Element | Description |
|---|---|
| Challenge Portal | Grand entrance UI with challenge name in calligraphy |
| Timer Display | Prominent countdown, color-coded (green→yellow→red) |
| Progress Bar | Shows completion percentage, question number |
| Score Display | Running score, potential max score |
| Boss Arena | Special themed background for final boss question |
| Completion Screen | Victory animation for pass, constructive feedback for fail |
| Streak Indicator | Consecutive correct answers counter (flame icon) |
| Sound | Intense focus music, correct/incorrect sound effects |

### 18.5 Reward Structure

| Tier | Pass Rewards | Perfect Score | Legacy (Annual) |
|---|---|---|---|
| Apprentice | 2000 XP, 500 coins, challenge badge (Bronze) | +500 XP, gem | Title: "Annual Apprentice" |
| Scholar | 5000 XP, 1000 coins, challenge badge (Silver) | +1000 XP, 5 gems | Title: "Annual Scholar" |
| Master | 10000 XP, 5000 coins, challenge badge (Gold) | +5000 XP, 20 gems, cosmetic | Title: "Annual Master" |
| Grand Master | 25000 XP, 10000 coins, challenge badge (Platinum) | +10000 XP, 50 gems, legendary cosmetic | Title: "Annual Grand Master" |
| Ultimate | 50000 XP, 25000 coins, challenge badge (Diamond) | +25000 XP, 100 gems, mythic cosmetic | Title: "Annual Ultimate Champion" |

### 18.6 Social Features

| Feature | Description |
|---|---|
| Challenge Spectating | Watch live attempts (with learner's permission) |
| Challenge Cheering | Send encouragement to attempting learner |
| Challenge Replay | View anonymized perfect runs |
| Challenge Discussion | Forum thread per challenge |
| Challenge Teams | Attempt group challenges together (special team challenges) |
| Challenge Respect | Acknowledge Hall of Fame entrants |

### 18.7 Islamic Values Integration

| Value | Implementation |
|---|---|
| Ihsan | Excellence in knowledge is rewarded — doing your best |
| Sabr | Challenges teach patience through difficulty |
| Tawakkul | Reminder before challenge start |
| Humility | Hall of Fame is anonymized — focus on achievement, not identity |
| Adab | Challenge etiquette: no gloating, congratulate others |
| Barakah | Du'a before challenge start recommended |

### 18.8 Edge Cases

| Edge Case | Handling |
|---|---|
| Internet disconnects mid-challenge | Progress saved, grace period of 5 min to reconnect |
| Timer runs out | Score based on answered questions |
| Challenge bug/error | Free retry token issued, bug fixed |
| Learner under 13 | No time pressure, reduced difficulty, more hints |
| Learner wants to forfeit | Option available, no penalty, cooldown waived |
| Challenge not possible (content missing) | Challenge hidden until prerequisite content exists |
| Server timeout during grading | Async grading, results within 1 hour, notification on completion |

### 18.9 Technical Requirements

| Requirement | Specification |
|---|---|
| Challenge Definitions | PostgreSQL (challenge_id, questions JSONB, rules JSONB) |
| Attempt State | Redis (per-attempt state, answers, timer, lives) |
| Grading Engine | Server-side, deterministic, anti-cheat checks |
| Timer | Server-authoritative, synced to client every 5 seconds |
| API | GET /challenges, POST /challenges/{id}/start, POST /challenges/{id}/answer, POST /challenges/{id}/complete |
| Anti-Cheat | Response time analysis, answer pattern detection, browser focus tracking |
| Hall of Fame | PostgreSQL with anonymized public view |

---

## 19. SPIRITUAL GAMIFICATION (EXTENDED)

### 19.1 Purpose

This section extends the spiritual gamification foundation from 17_Gamification.md (Hasana tracker, basic Quran progress, du'a practice) into a comprehensive spiritual growth ecosystem that respects the sacred nature of Islamic practice while encouraging consistent spiritual development alongside knowledge acquisition.

### 19.2 Game Mechanics

#### 19.2.1 Hasana Tracker (Extended)

| Feature | Description | Privacy |
|---|---|---|
| Private Counter | Only visible to the learner, never shared | Absolute privacy |
| Positive Activities | Learning, teaching, helping others, good deeds logged | Self-reported |
| Intention Weight | Bonus hasanat for learning with good intention | Auto-tracked via intention setting |
| No Negative Tracking | Never tracks sins or shortcomings | Design principle |
| Daily Hasana Goal | Personalized target based on learner's capacity | Private |
| Hasana Streak | Consecutive days with at least one hasana | Private |
| Hasana Milestones | Milestone notifications (100, 500, 1000, etc.) | Private |
| Hasana Reflection | Weekly summary of good deeds | Private |

#### 19.2.2 Good Deed Log

| Deed Type | Examples | Hasana Value | Verification |
|---|---|---|---|
| Learning | Study session, review, quiz | 1 per 10 min | Auto-tracked |
| Teaching | Help peer, create content, mentor | 5 per interaction | Auto + peer verification |
| Charity (Platform) | Share knowledge, contribute resources | 3 per action | Tracked |
| Kindness | Encourage peer, positive comment | 2 per action | Reported + verified |
| Patience | Complete difficult task, persist after failure | 3 per instance | Detected |
| Gratitude | Log shukr, reflect on blessings | 1 per instance | Self-reported |
| Forgiveness | Forgive, overlook mistake of peer | 5 per instance | Self-reported |
| Du'a | Make du'a for self or others | 2 per du'a | Tracked |

#### 19.2.3 Intention Setting Before Learning

| Feature | Description |
|---|---|
| Intention Prompt | Spirit asks "What is your intention for this session?" before start |
| Intention Options | "For Allah's pleasure," "To increase knowledge," "To teach others," "To correct misunderstanding," "For personal growth," "Custom" |
| Intention Reminder | Mid-session check: "Are you still learning with this intention?" |
| Intention Reward | Bonus XP + hasanat for intentional learning |
| Intention Log | Review past intentions (private) |
| Intention Stats | "Most common intention," "intention consistency" |

#### 19.2.4 Du'a Unlockables

| Du'a | Unlock Condition | Effect |
|---|---|---|
| Before Study Du'a | Default | Recited before every session |
| After Study Du'a | Complete first session | Recited after session |
| Du'a for Understanding | Master 10 facts | Boost comprehension for next session |
| Du'a for Memory | Complete 30 reviews | Boost retention for next review cycle |
| Du'a for Sincerity | Set intention 7 consecutive days | Blessing on learning |
| Du'a for Teachers | Help 5 peers | Blessing on teaching |
| Du'a for Parents | Complete parent-shared goal | Blessing on relationship |
| Du'a for Community | Join a guild | Blessing on community |
| Du'a for Ramadan | Ramadan active | Special Ramadan blessings |
| Du'a for Knowledge | Reach Rank 15 | Comprehensive knowledge blessing |

#### 19.2.5 Quran Progress Visualization

| View | Description | Data Source |
|---|---|---|
| Mushaf View | Full Quran visualization showing progress | Juz, Surah, Ayah tracking |
| Juz Completion | 30-section pie chart, colored by completion | Reading progress |
| Surah Grid | 114 Surah grid, color-coded by mastery | Memorization + understanding |
| Ayat Detail | Per-ayah view: read, understood, memorized, reviewed | Per-ayah tracking |
| Tajweed Progress | Rules mastered, application accuracy | Tajweed assessment |
| Tafsir Engagement | Tafsir sources consulted, depth of study | Tafsir reading log |
| Quran Connection | Cross-references to other knowledge domains | KG connections |

#### 19.2.6 Prayer Time Integration

| Feature | Description |
|---|---|
| Prayer Time Display | Current/next prayer time shown in UI |
| Learning Break Reminder | "Asr time is near — time to pause and pray" |
| Prayer-Completed Check-in | "JazakAllah khair. Ready to continue?" after prayer break |
| Fajr Learning Bonus | +50% XP for sessions started after Fajr |
| Isha Reflection Mode | Softer UI, reflection-focused content after Isha |
| Jumu'ah Special | Jumu'ah-specific content recommendations |
| Tahajjud Challenge | Optional: learn one new fact after Tahajjud (special badge) |

#### 19.2.7 Charity Milestones

| Milestone | Platform Action | Learner Benefit |
|---|---|---|
| 10,000 knowledge coins | Plant a tree in learner's name | Certificate, tree visualized in Memory Garden |
| 50,000 coins | Sponsor a child's education (optional, real) | Acknowledgment (or anonymous) |
| 100,000 coins | Build a water well (optional, real) | Acknowledgment (or anonymous) |
| 1,000 hasanat logged | Platform donations to Islamic charity | "Sadaqah Jariyah" badge |
| Time-based | Every 100 hours of learning = charity contribution | Ongoing charity badge |

### 19.3 Progression Design

#### 19.3.1 Spiritual Level

| Level | Title | Requirements | Unlocks |
|---|---|---|---|
| 1 | Seeker of Light | 0 hasanat | Basic du'a set |
| 2 | Grateful Heart | 100 hasanat, 7 intentions set | New du'a unlock |
| 3 | Rising Soul | 500 hasanat, 30-day good deed streak | Spiritual profile badge |
| 4 | Light Bearer | 2000 hasanat, 100 intentions set | Du'a for others |
| 5 | Compassionate Scholar | 10000 hasanat, taught 10 peers | "Noor" profile effect |
| 6 | Walking Blessing | 50000 hasanat, 365-day streak | Special spirit interaction |
| 7 | Rahmah (Mercy) | 100000 hasanat, community nominated | Recognition badge |
| 8 | Siddiq (Truthful) | 250000 hasanat, perfect intention record | Mythic spiritual title |

### 19.4 Visual Design

| Element | Description |
|---|---|
| Hasana Counter | Private, elegant counter with noor effect |
| Intention Screen | Soft, focused UI — minimal distractions |
| Quran View | Digital Mushaf, high-quality rendering, page curl |
| Prayer Times | UI widget, adhan-style visual indicator |
| Du'a Cards | Stylized card with du'a text (Arabic + translation) |
| Charity Progress | Visual tree growing as charity milestones reached |
| Spiritual Profile | Private section showing spiritual journey |

### 19.5 Reward Structure

| Achievement | Spiritual Reward | Platform Reward |
|---|---|---|
| 100 hasanat | Du'a unlock | 100 XP |
| 1000 hasanat | "Blessed" status icon (private) | 500 XP, title |
| 10,000 hasanat | Special du'a from scholar recording | 5000 XP, special frame |
| Perfect 7-day intention | "Sincere" badge (private) | 500 XP |
| Complete Quran reading | "Khatam" badge | 5000 XP, special cosmetic |
| 30-day prayer-time integration | "Prayerful Learner" badge | 1000 XP |

### 19.6 Social Features (Limited)

| Feature | Description | Privacy |
|---|---|---|
| Du'a Request | Ask friends/community to make du'a for you | Opt-in |
| Du'a for Others | Make du'a for friends (private) | Private |
| Charity Leaderboard | Aggregate charity impact (no personal data) | Anonymized |
| Intention Sharing | Share intention without details | Optional |
| Hasana Count | Absolutely never shared | Never |

### 19.7 Islamic Values Integration

| Value | Implementation |
|---|---|
| Ikhlas (Sincerity) | Intention setting before every session, rewards for intentional learning |
| Khushu (Focus) | Prayer time integration, focus mode, no notifications during prayer |
| Sadaqah | Charity milestones, real-world impact option |
| Shukr (Gratitude) | Gratitude logging, shukr prompts |
| Tawbah (Repentance) | No negative tracking — only positive growth |
| Taqwa (God-Consciousness) | Reminders of Allah throughout learning journey |
| Ihsan (Excellence) | Spiritual level system encourages consistent spiritual growth |

### 19.8 Edge Cases

| Edge Case | Handling |
|---|---|
| Learner doesn't want spiritual features | Full opt-out — no spiritual tracking, no prompts, standard gamification only |
| Non-Muslim learner | Spiritual features hidden/disabled by default, optional exploration |
| Hasana inflation through gaming | Hasana capped per day, diminishing returns for repeated actions |
| Du'a text accuracy | All du'as verified by scholars, sourced from authentic hadith |
| Intention becomes rote | Intention variety encouraged, random prompts, reflection questions |
| Prayer time notifications | Configurable: sound, silent, or disabled |
| Charity real-world integration | Optional, verified charity partners, transparent reporting, tax receipt provided |

### 19.9 Technical Requirements

| Requirement | Specification |
|---|---|
| Hasana Data | PostgreSQL (private, learner_id, hasana_type, count, date) |
| Intention Log | PostgreSQL (learner_id, intention, date, duration) |
| Du'a Unlocks | PostgreSQL (learner_id, du'a_id, unlock_date) |
| Quran Progress | Per-ayah tracking in PostgreSQL, mushaf rendering in Canvas |
| Prayer Times | API integration (Aladhan or similar), offline fallback |
| Charity Integration | Partner API, transaction logging, receipt generation |
| API | POST /spiritual/intention, GET /spiritual/hasana, POST /spiritual/dua |
| Privacy | Spiritual data never leaves learner's device unless explicitly synced |

---

## 20. SOCIAL GAMIFICATION (EXTENDED)

### 20.1 Purpose

This section extends the basic social features from 17_Gamification.md (simple leaderboards, peer helping) and 19_Social_Learning.md (study groups, forums) into a comprehensive social gamification ecosystem that encourages positive peer interaction, knowledge sharing, and community building — all within Islamic guidelines.

### 20.2 Game Mechanics

#### 20.2.1 Study Group Competitions

| Competition Type | Duration | Participants | Scoring | Rewards |
|---|---|---|---|---|
| Knowledge Sprint | 1 week | 3-10 per group | Total correct answers | Group badge, XP bonus |
| Memorization Race | 2 weeks | 2-8 per group | Facts memorized + retained | Memory bonus, title |
| Quiz Bowl | 1 day (live) | 2 teams of 3-5 | Team quiz accuracy | Team trophy (cosmetic) |
| Review Relay | 1 week | 4-8 per group | Review completion rate | Streak freeze for group |
| Teaching Tournament | 1 month | Pairs | Quality of teaching | "Teacher" title path |

#### 20.2.2 Peer Recognition

| Recognition Type | Trigger | Effect | Limitations |
|---|---|---|---|
| Masha'Allah | Peer's achievement | Social acknowledgment +1 hasana | 5/day |
| JazakAllah Khair | Peer helps you | Thank you +2 XP bonus | 10/day |
| Knowledge Shoutout | Peer teaches something valuable | +5 XP to peer, +1 hasana | 3/day |
| Helper Badge Nomination | Exceptional peer help | Nominate for Helper badge | 1/month |
| Mentor Recommendation | Outstanding mentorship | Recommend for Mentor role | 1/quarter |
| Community Spotlight | Positive community impact | Featured in community | Selected by moderation |

#### 20.2.3 Teaching Badges

| Badge | Requirement | Reward |
|---|---|---|
| Apprentice Teacher | Teach 1 peer to understand a concept | "Teacher" title (bronze) |
| Knowledge Sharer | Create 5 learning resources used by others | "Resource Creator" badge |
| Study Group Lead | Lead 10 group study sessions | "Group Leader" badge |
| Mentor | Mentor 3 learners to Rank 5+ | "Mentor" title |
| Master Teacher | Teach 100 peers effectively | "Mu'allim" title |
| Community Scholar | Contribute knowledge verified by scholars | "Community Scholar" title |

#### 20.2.4 Mentorship Ranks

| Rank | Title | Requirements | Perks |
|---|---|---|---|
| 1 | Guide | Teach 1 peer, complete mentorship training | Mentor badge |
| 2 | Teacher | Mentor 3 learners to Rank 3+ | Mentee capacity: 3 |
| 3 | Educator | Mentor 5 learners to Rank 5+ | Mentee capacity: 5, mentor chat |
| 4 | Professor | Mentor 10 learners to Rank 8+ | Create study plans for mentees |
| 5 | Master Teacher | Mentor 20 learners to Rank 10+ | Mentorship leader (anonymized) |
| 6 | Mu'allim | Mentor 50 learners, peer-nominated | "Mu'allim" title, mythic badge |

#### 20.2.5 Community Challenges

| Challenge Type | Scope | Example | Rewards |
|---|---|---|---|
| Global Knowledge Goal | All learners | "1,000,000 facts mastered this month" | All participants get badge |
| National Challenge | Per country | "Top learning country this month" | Country profile flag |
| Topic Rally | Per topic | "Master Tawheed together" | Topic-specific badge |
| Charity Drive | Good deeds | "100,000 hasanat logged" | Charity contribution |
| Speed Challenge | Time-limited | "100 quizzes in 1 hour (collective)" | Speed badge |

#### 20.2.6 Knowledge Sharing Rewards

| Action | Reward | Limitations |
|---|---|---|
| Share flashcard deck | 50 XP per use by others | Max 500 XP/deck |
| Create public quiz | 100 XP + 2 XP per attempt | Max 1000 XP/quiz |
| Write helpful answer | 30 XP + 5 XP per upvote | Max 300 XP/answer |
| Post reflection | 20 XP + 3 XP per meaningful engagement | Max 200 XP/post |
| Curate learning path | 200 XP + 10 XP per enrollment | Max 2000 XP/path |
| Contribute to wiki | 100 XP + verified by scholar | Max 5000 XP/article |

### 20.3 Progression Design

#### 20.3.1 Social Level

| Level | Title | Requirements |
|---|---|---|
| 1 | Participant | Join a study group or community |
| 2 | Contributor | Help 5 peers |
| 3 | Collaborator | Complete 3 group activities |
| 4 | Teacher | Teach a concept to a peer |
| 5 | Community Builder | Form a study group or rally 10 learners |
| 6 | Leader | Mentor 3 learners |
| 7 | Pillar | 50 positive recognitions received |
| 8 | Community Heart | Peer-nominated, exceptional community contribution |

### 20.4 Visual Design

| Element | Description |
|---|---|
| Recognition UI | Elegant card with name, reason, hasana count |
| Teaching Interface | Structured format for explaining concepts |
| Mentorship Dashboard | Mentee list, progress tracking, goals |
| Community Feed | Activity feed (opt-in, anonymized) |
| Group Leaderboard | Group progress, not individual ranking |
| Social Profile | Public profile showing social contributions (opt-in) |

### 20.5 Reward Structure

| Achievement | Social Reward | Platform Reward |
|---|---|---|
| First peer help | Helper badge (Bronze) | 100 XP |
| 10 peers helped | Helper badge (Silver) | 500 XP, title |
| 100 peers helped | Helper badge (Gold) | 2000 XP, cosmetic |
| First mentee reaches Rank 5 | Mentor badge | 1000 XP |
| 5 mentees reach Rank 10+ | Master Teacher badge | 10000 XP, title |
| Community impact award | Peer-nominated | Special recognition, profile effect |

### 20.6 Social Features (Extended)

| Feature | Description |
|---|---|
| Study Buddy System | Pair learners at similar levels for mutual accountability |
| Group Goals | Shared learning targets with collective rewards |
| Knowledge Circles | Small groups (3-5) with rotating teaching roles |
| Peer Review | Review each other's summaries, explanations |
| Collective Quizzes | Multiple learners answer same quiz, discuss answers |
| Social Accountability | Opt-in: notify friends if you miss a day (encouraging, not shaming) |
| Inter-Group Challenges | Two study groups compete in knowledge challenges |

### 20.7 Islamic Values Integration

| Value | Implementation |
|---|---|
| Ta'awun | Cooperation emphasized over competition in all social features |
| Adab | All interactions follow Islamic etiquette guidelines |
| No Riya | Social achievements are private by default, opt-in for display |
| Ikhlas | Peer recognition emphasizes sincere appreciation, not status |
| Ukhuwwah | Community features build brotherhood/sisterhood |
| Amar Ma'ruf | Encouraging good through positive peer influence |

### 20.8 Edge Cases

| Edge Case | Handling |
|---|---|
| Learner very shy/antisocial | Alternative solo paths, AI interaction option |
| Peer recognition abuse | Rate limiting, reporting, fake recognition detection |
| Competition causing stress | Full opt-out from all competitive features |
| Teaching quality concerns | Peer review of teaching, scholar verification for complex topics |
| Language barriers | Auto-translate for social features, language-tagged groups |
| Time zone differences | Asynchronous social features, flexible group schedules |
| Bullying/harassment | Zero tolerance, reporting system, immediate account suspension |

### 20.9 Technical Requirements

| Requirement | Specification |
|---|---|
| Social Actions | PostgreSQL (action_type, actor, target, timestamp, metadata) |
| Recognition System | PostgreSQL (recognition_type, giver, receiver, reason, date) |
| Mentorship | PostgreSQL (mentor_id, mentee_id, start_date, goals, progress) |
| Community Challenges | Redis (counters, progress), PostgreSQL (history) |
| Feed | Activity feed engine (Redis + PostgreSQL) |
| API | POST /social/recognize, POST /mentorship/start, GET /community/challenges |
| Moderation | AI pre-filter + human review queue |

---

## 21. ISLAMIC VALUES INTEGRATION

### 21.1 Purpose

This section provides the comprehensive Islamic values framework that governs ALL gamification mechanics in this document. Every system described above must conform to these principles. Islamic values are not an afterthought — they are the foundation upon which all gamification is built.

### 21.2 Core Principles

| Principle | Quranic/Hadith Basis | Gamification Application |
|---|---|---|
| **Niyyah (Intention)** | "Actions are judged by intentions" (Bukhari) | Intention-setting before sessions, rewards for intentional learning |
| **Ikhlas (Sincerity)** | "Worship Allah alone, sincere to Him" (Quran 98:5) | No public shaming, no spotlight on individuals, private achievements default |
| **Tawadhu (Humility)** | "And lower your wing to the believers" (Quran 15:88) | Anonymous leaderboards, no ranking comparisons, "most improved" focus |
| **Riya Prevention** | "The thing I fear most for you is minor shirk: riya" (Ahmad) | Default privacy, opt-in sharing, no public achievement counts, no boasting features |
| **Adab (Etiquette)** | "Adab is the fruit of knowledge" (Scholar saying) | Chat moderation, respectful competition, no backbiting tools |
| **Ihsan (Excellence)** | "Worship Allah as if you see Him" (Bukhari) | Quality-focused rewards, depth over speed, mastery recognition |
| **Sabr (Patience)** | "Allah is with the patient" (Quran 2:153) | Streak recovery, pruning mechanics teach patience, long-term rewards |
| **Shukr (Gratitude)** | "If you are grateful, I will give you more" (Quran 14:7) | Gratitude prompts, shukr logging, appreciation for blessings |
| **Ta'awun (Cooperation)** | "Help one another in righteousness and piety" (Quran 5:2) | Guilds, study groups, peer recognition, collective challenges |
| **Adl (Justice)** | "Be just, that is nearer to righteousness" (Quran 5:8) | Fair matchmaking, balanced progression, no pay-to-win |
| **Barakah (Blessing)** | Concept of divine blessing in efforts | Noor effects, Ramadan bonuses, blessed content markers |
| **Taqwa (God-Consciousness)** | "The best provision is taqwa" (Quran 2:197) | Reminders of Allah, spiritual tracking, intention checks |

### 21.3 Prohibited Mechanics

| Prohibited | Reason | Alternative |
|---|---|---|
| Public comparison of achievements | Riya (showing off) | Private progress, opt-in sharing |
| Leaderboard of failures/last place | Harms dignity | Show only positive progress |
| Pay-to-win mechanics | Unjust (zulm) | All advantages earned through learning |
| Gambling mechanics (loot boxes) | Haram (gambling) | Guaranteed rewards for effort |
| Excessive notifications | Causes ghaflah (heedlessness) | Respectful, limited, prayer-aware |
| Streak shaming | Causes despair | Streak recovery, freeze, grace periods |
| Competition with direct opponent visible | May cause hasad (envy) | Anonymized competition |
| Time-limited FOMO (fear of missing out) | Causes anxiety | Content returns, legacy options |
| Fake urgency/artificial scarcity | Dishonest | Transparent mechanics |
| Social comparison algorithms | Causes jealousy | Progress-focused only |

### 21.4 Required Mechanics

| Required | Reason | Implementation |
|---|---|---|
| Intention prompt before learning | Niyyah | Spirit asks intention before session |
| Privacy by default | Riya prevention | All social features opt-in |
| Humility reminders | Tawadhu | Periodic reminders in UI |
| Du'a integration | Barakah | Du'a before/after learning |
| Gratitude checkpoint | Shukr | After milestones, daily |
| Reflection opportunity | Tafakkur | Session end reflection prompt |
| Break for prayer | Taqwa | Prayer time reminders |
| Learning for Allah reminder | Ikhlas | Session start reminder |
| Adab guidelines for social | Adab | Visible in all social spaces |
| Charity link | Sadaqah | Optional charity milestones |

### 21.5 Implementation Framework

#### 21.5.1 Design Review Checklist

Every gamification feature must pass this checklist before implementation:

| Question | Pass/Fail | Notes |
|---|---|---|
| Does this mechanic encourage riya (showing off)? | Must be No | If yes, redesign or add privacy |
| Can this mechanic be exploited for ghaflah? | Must be No | If yes, add limits or reminders |
| Is the reward proportional to the effort? | Must be Yes | Check XP/reward economy |
| Does this respect the dignity of all learners? | Must be Yes | Including slow/new learners |
| Is there an opt-out or alternative path? | Must be Yes | For shy, busy, or struggling learners |
| Does this integrate du'a or remembrance? | Should have | Where appropriate |
| Is the content aligned with authentic sources? | Must be Yes | Scholar-verified content |
| Does this build community positively? | Should have | Not just individual achievement |
| Is the mechanic transparent? | Must be Yes | No hidden rules, no dark patterns |
| Can this lead to hasad (envy)? | Must be No | Check social comparison aspects |

#### 21.5.2 Moderation and Enforcement

| Violation | Action | Appeals |
|---|---|---|
| Riya (boasting) | Warning, feature restriction | 7-day appeal |
| Hasad (envy) comments | Content removal, warning | 7-day appeal |
| Backbiting/gheebah | Warning, temporary suspension | 14-day appeal |
| Harassment | Immediate suspension | 30-day appeal, investigation |
| Cheating/exploiting | Feature restriction, rollback | 14-day appeal |
| Inappropriate content | Content removal, warning | 7-day appeal |
| Repeated violations | Account suspension | Case-by-case review |

### 21.6 Edge Cases

| Edge Case | Resolution |
|---|---|
| Non-Muslim learner | All spiritual features optional, Islamic references contextualized, respect for all beliefs |
| Learner from different madhhab | Content accommodates all mainstream madhahib, no sectarian bias |
| Over-zealous learner | Mentor detects burnout risk, suggests breaks, limits session length |
| Competitive learner wants more competition | Anonymized competitions, "knowledge duel" with consent |
| Learner uses platform for riya | Mentor detects pattern, gentle reminder, feature restriction possible |
| Cultural variation in Islamic practice | Customizable settings for different traditions |
| Age-appropriate Islamic content | Age-gated content, parental controls |

### 21.7 Technical Requirements for Values

| Requirement | Implementation |
|---|---|
| Privacy Controls | Granular opt-in/opt-out per feature, default private |
| Intention Tracking | Server-side intention log, triggers for rewards |
| Riya Detection | Pattern analysis for boasting behavior, mentor alert |
| Content Filtering | AI + human moderation for adab compliance |
| Scholar Verification | Verified scholar badge, content review workflow |
| Prayer Time API | Reliable API with offline fallback |
| Du'a Database | Authenticated du'a library with citations |

---

## 22. SYSTEM-WIDE INTEGRATION

### 22.1 Integration with 00_Personal_AI_Mentor

| Gamification System | Mentor Integration |
|---|---|
| Knowledge Tree | Mentor highlights weak branches, suggests review targets |
| Knowledge Spirit | Mentor uses spirit stats to determine learner motivation state |
| Avatar Evolution | Mentor adjusts challenge difficulty based on avatar stage |
| Memory Garden | Mentor reminds learner of plants needing watering (review) |
| Daily Journey | Mentor generates the journey path based on learning plan |
| Knowledge Map | Mentor recommends next conquest based on curriculum |
| Titles | Mentor congratulates on title earn, suggests next title goal |
| Ranks | Mentor shows rank progress, recommends actions for next rank |
| Rare Achievements | Mentor hints at hidden achievement conditions |
| Collections | Mentor reminds of incomplete collections, suggests sources |
| Badges | Mentor tracks badge evolution progress |
| Season System | Mentor aligns recommendations with seasonal themes |
| Guild System | Mentor suggests guild activities matching learner needs |
| Study Rooms | Mentor recommends optimal room type for current task |
| Knowledge Expeditions | Mentor recommends expedition based on readiness |
| Hidden Discoveries | Mentor gives cryptic hints when learner is close |
| Master Challenges | Mentor suggests challenge timing, reviews performance |
| Spiritual Gamification | Mentor integrates intention-setting, du'a recommendations |
| Social Gamification | Mentor recommends peer help opportunities |

### 22.2 Integration with 17_Gamification.md

| 17_Gamification.md | 03_Gamification_Bible Enhancement |
|---|---|
| XP System | XP feeds into Knowledge Tree growth, Spirit evolution, avatar stages |
| 10 Levels | Supplemented by 20 Ranks with multi-dimensional requirements |
| 5 Badge Tiers | Extended to 6 tiers + evolution + families + synergies |
| Streaks | Streak health directly affects Memory Garden and Spirit |
| Simple Leaderboards | Replaced with humility-first, progress-focused anonymous tracking |
| Coins + Gems | Extended to 7+ currencies, collection economy, guild treasury |
| Basic Quests | Expanded to Expeditions, Master Challenges, Guild Quests |
| Hasana Tracker | Extended with intention engine, du'a unlocks, charity milestones |
| Profile Customization | Full Avatar Evolution with 4 stages and 50+ accessories |

### 22.3 Integration with 18_Progress_Tracking

| Progress Tracking Data | Used By Gamification System |
|---|---|
| Mastery per fact | Knowledge Tree (leaf health), Collections (gems) |
| Domain mastery | Knowledge Map (territory state), Memory Garden (plant health) |
| Learning events | Daily Journey (waypoint completion), Spirit (bond growth) |
| Quiz accuracy | Badges (evolution), Master Challenges (difficulty) |
| Streak data | Memory Garden (watering), Spirit (health) |
| Session duration | Avatar (evolution), Focus mode (study rooms) |
| Review completion | Garden (watering), Spirit (energy) |
| Cross-domain connections | Knowledge Tree (fruit), Map (trade routes) |

### 22.4 Integration with 19_Social_Learning

| Social Learning Feature | Gamification Extension |
|---|---|
| Study Groups | Guild system (persistent groups with progression) |
| Discussion Forums | Social XP, peer recognition, teaching badges |
| Peer Learning | Mentorship ranks, study buddy matching with rewards |
| Content Sharing | Knowledge sharing rewards, content creator badges |
| Moderation | Islamic values enforcement, adab rewards |

### 22.5 Integration with 01_Learning_Experience_Bible

| LXP Bible Feature | Gamification System |
|---|---|
| First Achievement Flow | "First Leaf" badge, first waypoint on Daily Journey |
| Learner Journey | Daily Journey, Knowledge Map, Knowledge Tree |
| Motivation Design | All gamification systems provide motivation hooks |
| Engagement Loops | Streaks, Daily Journey, recurring rewards |
| Onboarding | Seed stage of everything, progressive complexity |
| Habit Formation | Streaks, Memory Garden daily care, Daily Journey |

### 22.6 Integration with 02_Memory_Engine

| Memory Engine Metric | Gamification Impact |
|---|---|
| Retention score | Knowledge Tree leaf health, Memory Garden plant health |
| Forgetting risk | Pruning triggers, Spirit health decrease, Garden weeds |
| Stability | Knowledge Tree root health, Garden soil quality |
| Ease factor | Master Challenge difficulty scaling |
| Review schedule | Daily Journey waypoints, Garden watering reminders |
| Spaced repetition | Badge progression (consistency rewards) |
| Memory decay | Seasonal pruning prevention (winter), weed growth rate |

---

## 23. EDGE CASE CATALOG

### 23.1 Learner Lifecycle Edge Cases

| Edge Case | Affected Systems | Handling |
|---|---|---|
| New learner joins | All systems | Progressive reveal: only Tree + Spirit visible initially, then Garden (day 3), Journey (day 7), Map (day 14), Guild (day 30), etc. |
| Learner returns after long break | Tree (dormancy), Garden (overgrown), Spirit (egg state) | "Revival" mode: gentle re-onboarding, catch-up mechanics, no penalties |
| Learner deletes account | All systems | Data wiped per policy, anonymized aggregate contributions preserved |
| Learner transfers to new device | All systems | Cloud sync, session resumption |
| Learner switches language | Titles, Ranks, UI text | All localized, Arabic originals preserved |

### 23.2 Technical Edge Cases

| Edge Case | Affected Systems | Handling |
|---|---|---|
| Server failure during state save | Any system with state | Rollback to last save point, retry queue |
| Database corruption | All persistent systems | Regular backups, integrity checks, repair tools |
| CDN failure for assets | Visual systems (Tree, Garden, Avatar) | Graceful degradation to text/placeholder, retry |
| API rate limiting | Social systems, trading | Rate limit handling, exponential backoff |
| Offline usage | All systems | Local state cache, action queue, conflict resolution |
| High latency | Real-time features (rooms, guild chat) | Optimistic updates, reconnection handling |

### 23.3 Learner Behavior Edge Cases

| Edge Case | Affected Systems | Handling |
|---|---|---|
| Learner tries to game/cheat | XP, Badges, Achievements | Anti-cheat detection, replay review, rollback |
| Learner buys account (TOS violation) | All systems | Account suspension, progress reversal |
| Learner shares account | All systems | Device verification, anomaly detection |
| Learner creates multiple accounts | All systems | Account linking or alt detection |
| Learner intentionally harms community | Social, Guild | Moderation actions, feature restrictions |
| Learner data hoarding (never trades) | Collections | Solo completion paths available |

### 23.4 Islamic-Specific Edge Cases

| Edge Case | Affected Systems | Handling |
|---|---|---|
| Learner observes different madhhab | Content, Titles, Ranks | Content accommodates all mainstream madhahib |
| Ramadan falls in different seasons | Season System | Ramadan overrides visual season |
| Learner in non-Muslim country | Spiritual features | All features optional, respectful of local laws |
| Learner converts to Islam mid-journey | All systems | Welcome mode, spiritual features enabled |
| Hajj/Umrah during learning | Streaks, Journey | Travel mode: reduced requirements, no streak loss |
| Menses (learners may pause Quran) | Quran progress, Streaks | Grace period, alternative content during pause |

### 23.5 Parental/Guardian Edge Cases

| Edge Case | Affected Systems | Handling |
|---|---|---|
| Parent wants full visibility of child's data | Privacy | Parent dashboard with learner consent (COPPA/GDPR-K) |
| Parent restricts social features | Social, Guild | Social features disabled until age of consent |
| Parent sets time limits | Session tracking, Streaks | Streak protection when time-limited |
| Parent wants religious oversight | Content, Spiritual | Scholar-verified content assurance |

---

## 24. TECHNICAL ARCHITECTURE

### 24.1 System Architecture Overview

```
Client Applications (Web, Mobile, Native)
        │
        ▼
┌─────────────────────────────────────────────┐
│           API Gateway                        │
│  Load Balancer │ Auth │ Rate Limit │ Cache  │
└─────────────────────────────────────────────┘
        │
        ▼
┌─────────────────────────────────────────────┐
│         Gamification Service Layer           │
│                                              │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐    │
│  │  Event   │ │  Engine  │ │  State   │    │
│  │  Handler │ │  Core    │ │  Manager │    │
│  └──────────┘ └──────────┘ └──────────┘    │
│                                              │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐    │
│  │  Rule    │ │  Reward  │ │  Notif   │    │
│  │  Engine  │ │  Calc    │ │  System  │    │
│  └──────────┘ └──────────┘ └──────────┘    │
└─────────────────────────────────────────────┘
        │
        ▼
┌─────────────────────────────────────────────┐
│           Data Layer                         │
│  Redis (hot) │ PostgreSQL (cold) │ S3 (media)│
└─────────────────────────────────────────────┘
        │
        ▼
┌─────────────────────────────────────────────┐
│         External Integrations                │
│  Mentor │ Memory │ Social │ Content │ KG     │
└─────────────────────────────────────────────┘
```

### 24.2 Data Model (Core Entities)

| Entity | Storage | Key Fields | Relationships |
|---|---|---|---|
| Learner_State | PostgreSQL + Redis | learner_id, xp, level, rank, coins, gems | 1:1 with every system |
| Knowledge_Tree | PostgreSQL | learner_id, tree_stage, branch_health JSONB | References KG |
| Knowledge_Spirit | PostgreSQL | learner_id, spirit_type, bond, health, abilities | 1:1 with learner |
| Avatar | PostgreSQL | learner_id, stage, accessories JSONB | 1:1 with learner |
| Memory_Garden | PostgreSQL | learner_id, plots JSONB, health | References domains |
| Daily_Journey | Redis | learner_id, date, path JSONB, waypoints | Generated daily |
| Knowledge_Map | PostgreSQL | learner_id, territories JSONB | References KG |
| Titles | PostgreSQL (defs + learner) | title_id, learner_id, equipped | M:M with learner |
| Ranks | PostgreSQL | learner_id, rank_id, multi_dim_progress | 1:1 with learner |
| Achievements | PostgreSQL | achievement_id, learner_id, progress | M:M with learner |
| Collections | PostgreSQL | item_id, learner_id, quantity, tradeable | M:M with learner |
| Badges | PostgreSQL | badge_id, learner_id, tier, equipped | M:M with learner |
| Season_Pass | PostgreSQL | learner_id, season_id, level, premium | 1:M with seasons |
| Guild | PostgreSQL | guild_id, name, level, treasury, settings | 1:M with members |
| Study_Room | PostgreSQL + Redis | room_id, theme, occupants, state | 1:M with learners |
| Expedition | PostgreSQL + Redis | expedition_id, team_id, stage, progress | 1:M with stages |
| Discoveries | PostgreSQL | discovery_id, learner_id, found_date | M:M with learner |
| Master_Challenge | PostgreSQL | challenge_id, learner_id, attempts, best_score | 1:M with attempts |
| Spiritual | PostgreSQL | learner_id, hasana_count, intentions_log | Private |
| Social | PostgreSQL | action_id, actor, target, type, timestamp | M:M with learners |

### 24.3 Event System

| Event | Emitter | Consumers |
|---|---|---|
| learning_event | Content systems | All gamification systems |
| mastery_change | Memory Engine | Tree, Garden, Map, Badges |
| streak_update | Progress Tracking | Garden, Spirit, Daily Journey |
| level_up | 17_Gamification | Avatar, Ranks, Titles |
| badge_earned | 17_Gamification | Collections, Achievements, Spirit |
| season_change | Season System | Tree, Garden, Daily Journey, UI Theme |
| guild_event | Guild System | Guild members, Study Rooms |
| social_action | Social Learning | Social gamification, Peer recognition |
| spiritual_action | Spiritual System | Hasana, Du'a, Spirit |
| challenge_complete | Master Challenge | Titles, Achievements, Collections |

### 24.4 Performance Requirements

| Metric | Target |
|---|---|
| Event processing latency | < 100ms p99 |
| State read (hot) | < 10ms |
| State read (cold) | < 100ms |
| Concurrent learners | 1M+ |
| Write throughput | 10K events/sec |
| Cache hit ratio | > 95% |
| API response time | < 200ms p95 |
| Database query time | < 50ms p99 |
| Media asset load | < 1s |
| Real-time sync (WebSocket) | < 100ms |

### 24.5 Security Requirements

| Requirement | Implementation |
|---|---|
| Anti-cheat | Server-authoritative state, client as view |
| Rate limiting | Per-endpoint, per-learner, graduated |
| Data encryption | At rest (AES-256), in transit (TLS 1.3) |
| Privacy | All gamification data belongs to learner |
| Audit log | All state changes logged |
| Fraud detection | Pattern analysis for unusual activity |
| DDoS protection | Cloud-based WAF, rate limiting |
| API authentication | JWT with short expiry, refresh tokens |

### 24.6 Deployment and Scaling

| Component | Scaling Strategy |
|---|---|
| API Servers | Horizontal auto-scaling (Kubernetes) |
| Redis | Cluster mode, read replicas |
| PostgreSQL | Read replicas, sharding by learner_id |
| WebSocket Servers | Sticky sessions, horizontal scaling |
| Event Queue | Kafka (partitioned by event type) |
| Media Assets | CDN with regional edge caching |
| Background Jobs | Worker pool with priority queues |

---

## 25. APPENDICES

### 25.1 Complete Title List (50+ Titles)

| Title | Tier | Category | Requirement |
|---|---|---|---|
| First Leaf | Common | Achievement | Earn first achievement |
| Helper | Common | Social | Help 5 peers |
| Questioner | Common | Skill | Ask 50 AI Teacher questions |
| Gardener | Uncommon | Skill | Complete 30 days of garden care |
| Pathblazer | Uncommon | Achievement | Explore 10 regions |
| Connector | Uncommon | Skill | Make 5 cross-domain connections |
| Resilient | Uncommon | Achievement | Re-learn 10 pruned facts |
| Tawheed Scholar | Rare | Knowledge | Master Tawheed domain 100% |
| Cartographer | Rare | Achievement | Map 5 complete regions |
| Mentor | Rare | Social | Mentor 3 learners |
| Knowledge Seeker | Epic | Achievement | Conquer 3 domains |
| Scholar Owl | Epic | Special | Max bond with Scholar Owl spirit |
| Harvester | Epic | Achievement | Complete 10 harvest events |
| Sahib (Companion) | Epic | Social | Max bond with all spirit types |
| Emperor of Knowledge | Legendary | Achievement | Conquer all domains |
| Mountain | Legendary | Knowledge | Reach Mountain avatar stage |
| Alim | Legendary | Knowledge | Reach Rank 19 |
| Mu'allim | Legendary | Social | Mentor 50 learners, peer-nominated |
| Living Legacy | Mythic | Special | 3-year streak + all domains + community impact |
| Noor of Knowledge | Mythic | Spiritual | Max spiritual level + all du'as unlocked |
| Founding Scholar | Legacy | Special | First 10,000 platform users |
| Beta Pioneer | Legacy | Special | Beta tester |
| Season 1 Veteran | Legacy | Special | Completed first platform season |

### 25.2 Rank Title Translations

| Rank | Arabic | Transliteration | English |
|---|---|---|---|
| 1 | طالب | Talib | Seeker |
| 2 | مبتدئ | Mubtadi' | Beginner |
| 3 | دارس | Daris | Learner |
| 4 | متعلم | Muta'allim | Student |
| 5 | متتبع | Mutatabbi' | Follower |
| 6 | باحث | Bahith | Researcher |
| 7 | فاهم | Fahim | Understander |
| 8 | حافظ | Hafiz | Preserver |
| 9 | ناقد | Naqid | Critic |
| 10 | محقق | Muhaqqiq | Verifier |
| 11 | مفسر | Mufassir | Interpreter |
| 12 | محدث | Muhaddith | Narrator |
| 13 | فقيه | Faqih | Jurist |
| 14 | أصولي | Usuli | Foundationalist |
| 15 | مجتهد | Mujtahid | Diligent One |
| 16 | حكيم | Hakim | Wise One |
| 17 | مرشد | Murshid | Guide |
| 18 | مفتي | Mufti | Scholar of Rulings |
| 19 | عالم | Alim | Scholar |
| 20 | رباني | Rabbānī | Divine Scholar |

### 25.3 Currency Types

| Currency | Earned By | Spent On | Cap |
|---|---|---|---|
| Knowledge Coins | XP conversion, achievements | Profile items, content unlocks, guild creation | No cap |
| Gems | Special achievements, challenges | Premium items, titles, season pass | No cap |
| Seasonal Currency | Seasonal activities | Seasonal shop (limited time) | Converts to coins (50%) |
| Guild Currency | Guild activities, GvG | Guild hall upgrades, guild shop | Capped by guild level |
| Challenge Tokens | Challenge completions | Master Challenge entry | 5 max held |
| Hasana (Spiritual) | Good deeds, intention | Nothing (non-spendable, purely spiritual) | No cap |
| Mentor Points | Mentorship activities | Mentor shop, special items | Capped per mentor rank |

### 25.4 Glossary

| Term | Definition |
|---|---|
| Knowledge Graph (KG) | Structured database of all Islamic facts and their relationships |
| Mastery | Score (0-5) representing depth of knowledge for a fact |
| Stability | Score (0-1) representing resistance to forgetting |
| Pruning | Removal of a forgotten fact leaf from Knowledge Tree |
| Riya | Showing off — prohibited in Islam, designed out of all mechanics |
| Niyyah | Intention — required before learning actions |
| Hasana | Spiritual reward unit (private to learner) |
| Halaqah | Traditional Islamic study circle |
| Noor | Divine light, visual effect for blessed/spiritual content |
| Barakah | Blessing, bonus reward for spiritually aligned actions |
| LOD | Level of Detail — performance optimization for rendering |
| GvG | Guild versus Guild competition |

### 25.5 Quality Metrics

| Metric | Target | Measurement |
|---|---|---|
| Daily Active Users (gamification) | >50% of registered learners | Learners who interact with 3+ gamification systems daily |
| Feature Adoption | >80% of learners use 5+ systems within 30 days | Per-feature activation tracking |
| Streak Participation | >40% maintain 7+ day streaks | Streak distribution analysis |
| Guild Participation | >20% of eligible learners join a guild | Guild membership rate |
| Season Pass Conversion | >15% purchase premium pass | Purchase rate / active learners |
| Title Engagement | >60% earn at least one non-automatic title | Title earn rate |
| Collection Completion | >30% complete at least one collection | Collection completion rate |
| Social Feature Opt-In | >70% opt into at least one social feature | Privacy setting analysis |
| Spiritual Feature Opt-In | >50% use at least one spiritual feature | Feature activation rate |
| Negative Behavior Rate | <0.1% of interactions | Reports, moderation actions |
| Learner Satisfaction | >4.0 / 5.0 | Surveys on gamification enjoyment |
| Islamic Values Compliance | 100% pass rate | Design review checklist |

### 25.6 Related Documents

| Document ID | Title | Relationship |
|---|---|---|
| LXP-MENTOR-001 | Personal AI Mentor | Mentor adjusts difficulty, recommends challenges, tracks all systems |
| LXP-GAME-001 | Gamification Architecture (17_Gamification.md) | This document extends (337 lines → 2500+ lines) |
| LXP-PROG-001 | Progress Tracking (18_Progress_Tracking.md) | All progress data feeds gamification achievements |
| LXP-SOC-001 | Social Learning (19_Social_Learning.md) | Guilds, study rooms, community features built on social architecture |
| LXP-LEB-001 | Learning Experience Bible (01_LXP_Bible.md) | Daily journey, first achievement, learner journey flow |
| LXP-MEM-001 | Memory Engine (02_Memory_Engine.md) | Memory garden health, knowledge tree pruning based on retention data |

### 25.7 Revision History

| Version | Date | Author | Changes |
|---|---|---|---|
| 1.0.0 | 2026-07-10 | Office of the Chief Architect | Initial comprehensive release — extends 17_Gamification.md with 20 new subsystems |

---

*End of Document — 03_Gamification_Bible*
*Total Systems: 20 (Knowledge Tree, Knowledge Spirit, Avatar Evolution, Memory Garden, Daily Journey, Knowledge Map, Titles, Ranks, Rare Achievements, Collections, Badges Extended, Season System, Guild System, Study Rooms, Knowledge Expeditions, Hidden Discoveries, Master Challenges, Spiritual Gamification Extended, Social Gamification Extended, Islamic Values Integration)*
*Connections: 00_Personal_AI_Mentor, 17_Gamification.md, 18_Progress_Tracking, 19_Social_Learning, 01_Learning_Experience_Bible, 02_Memory_Engine*
