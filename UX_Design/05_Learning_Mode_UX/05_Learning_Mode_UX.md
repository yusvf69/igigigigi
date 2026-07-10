# Learning Mode UX Documentation

> **Document Version:** 1.0  
> **Last Updated:** 2026-07-10  
> **Classification:** Enterprise Internal — Experience Design Reference  
> **Author:** Chief Experience Architect  
> **Status:** Living Document  

---

## Table of Contents

1. [Document Purpose & Scope](#1-document-purpose--scope)
2. [Cross-Reference Architecture](#2-cross-reference-architecture)
3. [Mode 01: Story Mode](#3-mode-01-story-mode)
4. [Mode 02: Documentary Mode](#4-mode-02-documentary-mode)
5. [Mode 03: Podcast Mode](#5-mode-03-podcast-mode)
6. [Mode 04: Dialogue Mode](#6-mode-04-dialogue-mode)
7. [Mode 05: Timeline Mode](#7-mode-05-timeline-mode)
8. [Mode 06: Map Mode](#8-mode-06-map-mode)
9. [Mode 07: Character Mode](#9-mode-07-character-mode)
10. [Mode 08: Quiz Mode](#10-mode-08-quiz-mode)
11. [Mode 09: Flashcard Mode](#11-mode-09-flashcard-mode)
12. [Mode 10: Memory Engine Mode](#12-mode-10-memory-engine-mode)
13. [Mode 11: Knowledge Tree Mode](#13-mode-11-knowledge-tree-mode)
14. [Mode 12: Knowledge Graph Mode](#14-mode-12-knowledge-graph-mode)
15. [Mode 13: AI Teacher Mode](#15-mode-13-ai-teacher-mode)
16. [Mode 14: AI Mentor Mode](#16-mode-14-ai-mentor-mode)
17. [Mode 15: AI Chat Mode](#17-mode-15-ai-chat-mode)
18. [Mode 16: Memory Garden Mode](#18-mode-16-memory-garden-mode)
19. [Cross-Mode Integration](#19-cross-mode-integration)
20. [Global Interaction Patterns](#20-global-interaction-patterns)
21. [Performance & Technical Specifications](#21-performance--technical-specifications)

---

## 1. Document Purpose & Scope

### 1.1 Purpose

This document serves as the definitive UX reference for all sixteen learning modes within the platform. It provides exhaustive specifications covering user experience, user interface, interaction flows, animation systems, adaptive behavior, AI integration, memory system hooks, avatar integration, knowledge tree connectivity, reward mechanisms, and accessibility compliance for every mode.

Each mode is documented as a self-contained experience system while maintaining explicit connections to the broader platform ecosystem. This document is designed to be the single source of truth for designers, developers, product managers, and quality assurance teams working on any learning mode implementation.

### 1.2 Document Conventions

| Convention | Meaning |
|------------|---------|
| **Bold** | Component names, state names, action labels |
| `Code` | Token names, variable references, design system values |
| *Italic* | Emphasis, user mental models, conceptual framing |
| → | Flow direction, progression, navigation |
| [n] | Cross-reference to section number |
| ⚠️ | Critical UX consideration |
| ♿ | Accessibility requirement |

### 1.3 Design Principles Applied Across All Modes

1. **Frictionless Immersion** — The interface must dissolve into the background during active learning, reappearing only when the learner needs to make a conscious decision.
2. **Spatial Consistency** — All modes maintain consistent spatial mapping: left = back/past, right = forward/future, up = expansion/detail, down = collapse/return.
3. **Cognitive Load Management** — Each screen presents exactly one primary action. Secondary options are deferred, grouped, or hidden behind progressive disclosure.
4. **Emotional Resonance** — Every interaction includes micro-delight elements that create moments of joy, surprise, or satisfaction without becoming distracting.
5. **Adaptive Intelligence** — The UI adapts to the learner's proficiency, pace, preference, and emotional state inferred from interaction patterns.
6. **Memory-First Architecture** — Every learning interaction writes to the memory system, ensuring that no learning moment is lost and spaced repetition is continuously optimized.
7. **Progressive Mastery** — Each mode supports a journey from novice to expert interaction patterns, with advanced features unlocking as competence grows.
8. **Contextual Presence** — The AI avatar provides contextually aware presence across all modes, appearing when guidance is needed and receding when the learner is in flow.

---

## 2. Cross-Reference Architecture

### 2.1 Document Dependencies

This document is the central node in a network of UX documentation. It references and is referenced by the following documents:

| Document | Relationship | Integration Points |
|----------|--------------|-------------------|
| UX_Design/00_UX_UI_Audit | Audit compliance reference | All modes are audited against this document's inventory |
| UX_Design/01_Experience_Design_Bible | Philosophical foundation | Every mode implements principles from sections 1-26 |
| UX_Design/02_Design_System | Visual and interaction tokens | Color, typography, spacing, animation tokens used throughout |
| UX_Design/03_Component_Library | Reusable component specifications | All mode interfaces built from component library |
| UX_Design/04_Screen_Specifications | Screen-level specifications | Each mode maps to specific screen specifications |
| UX_Design/06_Motion_Design_Bible | Animation and transition specifications | All mode animations reference this document |
| UX_Design/07_Illustration_Bible | Illustration and icon specifications | Mode-specific illustrations defined here |
| UX_Design/08_Accessibility_Bible | Accessibility standards | All modes comply with specifications in this document |

### 2.2 Cross-Mode Data Flow Architecture

Memory Engine reads/writes spaced repetition data with all modes
Knowledge Tree creates and traverses knowledge nodes across all modes
Knowledge Graph maps relationships and enables discovery across all modes
Avatar System integrates with Story, Dialogue, AI Teacher, AI Mentor, AI Chat
Reward Engine triggers achievements and tracks progress across all modes
AI Orchestrator provides context-aware AI behavior across all modes

### 2.3 Shared Design Tokens Reference

All modes utilize the following shared design tokens as defined in the Design System:

| Token Category | Usage Across Modes |
|----------------|-------------------|
| color-primary-* | Brand identity, primary actions, active states |
| color-secondary-* | Accent highlights, achievements, positive feedback |
| color-tertiary-* | Knowledge connections, graph elements, map features |
| color-surface-* | Mode backgrounds, cards, containers |
| color-text-* | All typography across every mode |
| color-state-* | Success, error, warning, info states |
| spacing-* | Consistent padding and margin across all mode interfaces |
| elevation-* | Z-depth hierarchy for modals, overlays, and floating elements |
| radius-* | Corner rounding consistency |
| animation-* | Duration and easing shared across modes |
| type-* | Typography scale applied uniformly |

---

## 3. Mode 01: Story Mode

### 3.1 Overview

Story Mode is an immersive narrative learning experience that transforms educational content into interactive stories. Learners navigate through historically accurate or narratively constructed worlds, making choices that affect their journey while acquiring knowledge organically through compelling storytelling. This mode bridges the gap between entertainment and education by leveraging narrative psychology to enhance retention and emotional engagement.

### 3.2 Complete UX

**User Experience Philosophy:** The learner is the protagonist of their own educational journey. Every story places the learner inside a narrative context where knowledge is acquired through experience rather than instruction. The interface operates on the principle of *invisible instruction* — the learner is always doing, choosing, exploring, or reflecting, never passively reading instructions about how to proceed.

**Emotional Journey Map:**

| Phase | Emotion | UX Strategy |
|-------|---------|-------------|
| Entry | Curiosity, anticipation | Cinematic chapter select with atmospheric previews |
| Beginning | Orientation, mild uncertainty | Gentle onboarding narration, contextual tooltips |
| Rising action | Engagement, investment | Meaningful choices with visible consequences |
| Climax | Tension, excitement, flow | Critical decision points with time-pressure options |
| Resolution | Satisfaction, accomplishment | Narrative closure, knowledge revealed, rewards |
| Reflection | Contemplation, integration | Knowledge node update, comprehension check, journal prompt |

**Cognitive Load Management:**
- Left rail contains only chapter navigation (collapsed by default)
- Right rail contains optional character reference and glossary (collapsed by default)
- Bottom bar shows only reading progress and AI narrator toggle
- All secondary controls accessible via a single overflow menu
- Choice interfaces appear only at decision points, never during reading flow

**Flow State Design:**
- Reading speed detected and used to auto-adjust text reveal pacing
- Distraction-free mode removes all UI except text and page-flip controls
- Immersion mode transitions to full-screen with atmospheric background
- Choices presented at natural narrative pauses, never interrupting mid-sentence
- AI narrator voice activates during high-emotion passages for enhanced impact

### 3.3 Complete UI

**Story Reader Interface (Primary Screen):**

Top bar: back button to Library, Story Title centered, settings, bookmark, font controls, narrator toggle, overflow menu

Main content area: Rich text content with inline illustrations, historical annotations accessible via tap, character dialogue in speech bubbles, interactive glossary words highlighted

Bottom bar: Previous/Next page buttons with page counter, progress bar showing reading progress

**Chapter Select Interface:**

Grid of chapter cards. Each card shows: chapter number, title, estimated reading time, completion status (completed/unlocked/locked), page range, knowledge contribution indicator. Locked chapters show prerequisite requirements. Continue button appears at last-uncompleted chapter. Overall progress bar at bottom.

**Interactive Choice Interface:**

At decision points, narrative text dims by 15%. Choice cards slide up from bottom with staggered animation. Each card shows choice text, moral/strategic category icon, and potential consequence preview on hover. Timer appears for time-sensitive choices. Consequence indicators show which attributes (knowledge, honor, relationships) will be affected.

**UI Component Inventory:**

| Component | States | Behavior |
|-----------|--------|----------|
| Page Container | default, fullscreen, split-view | Smooth transitions between states |
| Text Block | default, highlighted, annotated | Tap word for definition, long-press for context |
| Illustration Frame | loading, visible, expanded | Lazy-loaded with blur-up placeholder |
| Speech Bubble | incoming, outgoing, thinking | Typing animation with character avatar |
| Choice Card | default, hover, selected, locked, revealed | Hover expands preview, selection triggers branch |
| Progress Bar | passive, active, chapter-complete | Fills smoothly, pulses on milestone |
| Narrator Toggle | on, off, auto | Auto mode narrates key passages only |
| Bookmark Button | default, bookmarked, filled | Animated transition with haptic feedback |
| Font Controls | size slider, line-height, font-family | Preview panel shows changes live |
| Chapter Card | locked, unlocked, completed, active | Parallax thumbnail, progress ring |

### 3.4 Complete Flow

**Entry → Chapter Selection → Reading → Interaction → Reflection → Knowledge Update**

**Detailed Flow Steps:**

1. **Entry Point:** Learner taps Story Mode from mode selection grid. A 1.2-second atmospheric transition plays: the mode grid dissolves into a rotating book animation that resolves into the Story Mode hub. Background ambient audio begins with page rustling and distant oud music.

2. **Story Library:** The learner is presented with available stories organized by civilization, era, or topic. Each story card features an illustrated cover, title, author, estimated reading time, knowledge value (number of nodes contributed), and completion status. Cards support swipe-left for quick preview showing first paragraph as overlay. Pull-to-refresh fetches newly published stories.

3. **Story Selection to Chapter Select:** Tapping a story triggers a page-turn animation (0.6s cubic-bezier) transitioning to chapter select. The chapter tree is displayed as a vertical timeline. Each chapter shows: title, duration, page range, knowledge contribution, and completion state. Locked chapters show prerequisite requirements. A Start from beginning CTA floats at top. Continue CTA appears at the last-uncompleted chapter.

4. **Chapter Start to Reading View:** Selecting a chapter triggers a cinematic transition. The chapter cover illustration zooms to fill screen, fades, and the first page slides in from right. Reading view opens with the text positioned at the top of the first page. Ambient audio adjusts to match chapter mood. AI narrator state restores to previous preference.

5. **Reading Interaction:** Learner reads by scrolling vertically (mobile) or using page-turn gestures (tablet/desktop). Left/right arrow keys, swipe left/right, or click page edges. Scroll indicator appears as a thin line at screen edge. Auto-scroll option reads at user's tracked pace. Tap-hold on a word opens definition panel. Tap historical figure name opens mini character card. Double-tap paragraph opens annotation and comprehension check.

6. **Choice Encounter:** At pre-authored decision points, reading pauses automatically. The narrative text dims by 15%. Choice cards slide up from bottom with staggered animation (50ms delay between each card, 0.4s duration). Timer appears if choice is time-sensitive for historical reenactment scenarios. Hovering a choice card expands it to show potential consequences. Selection triggers haptic feedback (gentle pulse) and the unchosen cards slide out with a 15% scale-down and fade (0.3s). The chosen card expands to full width, new narrative text continues.

7. **Branch Resolution:** After choice, the narrative continues along the determined branch. A subtle visual indicator in the margin shows that the story has branched. Branch metadata is recorded: choice made, timestamp, deliberation time, and any post-choice reflection notes.

8. **Comprehension Check:** At chapter end, before reflection, a comprehension check appears. This is a brief (1-3 question) interactive checkpoint embedded in the narrative transition. Questions are contextually relevant to the chapter just read. Format matches the narrative tone. Completion marks the chapter as finished.

9. **Chapter Complete to Reflection:** A chapter complete animation plays: a beautiful illustrated spread with the chapter title, a knowledge summary visual with 3-5 key concepts as floating cards that orbit and collapse into a knowledge node icon, completion time, choices made visualized as a branching diagram. The reflection prompt appears asking what resonated with the learner. Learner can type notes, record voice memo, or skip. Submitting the reflection triggers knowledge node creation.

10. **Knowledge Node Update:** The AI analyzes the chapter content, the learner's choices, comprehension check answers, and reflection notes. It creates or updates knowledge nodes in the Knowledge Tree. Connections are established to existing knowledge. Memory strength is initialized based on comprehension score. Spaced repetition schedule is calculated.

11. **Next Chapter or Exit:** Continue to next chapter CTA appears with a preview of what comes next. Return to library option available. Auto-advance toggle (Netflix-style) for binge-reading sessions. Exit saves exact position, scroll state, and mode state for seamless resumption.

12. **Exit Transition:** If learner exits mid-chapter, a subtle bookmark animation (0.5s) confirms position saved. A mini-summary card shows for 3 seconds before fading showing where they left off.

### 3.5 Animations

**Page Turn Animation:**
- Type: Curl page turn simulation
- Duration: 400ms
- Easing: cubic-bezier(0.22, 1, 0.36, 1)
- Physics: Page follows finger position during drag; release commits or cancels turn based on velocity threshold
- Shadow: Dynamic shadow on turning page corner, opacity 0.3-0.6 based on angle
- Audio: Soft paper rustle synced to curl angle (directional audio, left/right channel)

**Chapter Transition Animation:**
- Type: Zoom-fade through chapter cover illustration
- Duration: 800ms
- Sequence: Current content fades out (200ms) with cover art zooming to 120% and blurring (300ms) then new chapter content slides in from right with slight over-shoot (300ms)
- Particle: Dust motes float across screen during transition for atmospheric continuity

**Choice Card Animation:**
- Entry: Cards slide up from bottom edge with staggered delay (0 + 50n ms per card)
- Duration: 400ms
- Easing: cubic-bezier(0.34, 1.56, 0.64, 1) (spring-like overshoot)
- Hover: Card lifts 4px on elevation shadow, border highlight in secondary color
- Selection: Selected card scales 1.02, other cards blur and scale 0.85 with 0.3 opacity, then exit downward
- Rejected card exit: 300ms, ease-in, translateY(60px) plus opacity(0)

**Comprehension Check Animation:**
- Entry: A stylized quill icon draws a circle (reveal animation, 600ms)
- Question card: Flips in from back (3D card flip, 500ms)
- Option selection: Gentle scale bounce (0.95 to 1.05 to 1.00, 300ms)
- Correct feedback: Green glow pulse, checkmark draws itself, 500ms
- Incorrect feedback: Gentle red shimmer, correct answer highlights, 700ms

**Knowledge Node Creation Animation:**
- Type: Particle convergence into glowing node
- Duration: 1200ms
- Sequence: Knowledge concepts float as text fragments, orbit in circle, converge to center, coalesce into pulsing node, then node settles with gentle glow
- Particle count: 5-15 depending on chapter knowledge density

### 3.6 Interactions

| Gesture | Action | Visual Feedback |
|---------|--------|----------------|
| Swipe left (mobile) | Next page | Page curl follows finger, 1:1 tracking |
| Swipe right (mobile) | Previous page | Page curl reverse direction |
| Tap left edge | Previous page | Left edge highlight, page slides right |
| Tap right edge | Next page | Right edge highlight, page slides left |
| Scroll down | Scroll content | Smooth scroll, progress indicator updates |
| Long-press word | Definition panel | Word highlights, panel expands from word position |
| Tap figure name | Character card | Mini card appears as overlay with character portrait |
| Pinch text area | Font size adjust | Size indicator overlay, live preview |
| Double-tap paragraph | Quick note/annotation | Text selection highlight, annotation input appears |
| Swipe down from top | Open chapter list | Chapter list slides down with blur backdrop |
| Swipe up from bottom | Open settings/controls | Control panel slides up with spring animation |

**Haptic Feedback Patterns:**
- Page turn complete: Gentle thud (iOS) or short tick (Android), 50ms
- Choice selection: Sharp confirmation buzz, 50ms
- Chapter complete: Series of 3 escalating pulses

### 3.7 Adaptive UI

**Screen Size Adaptations:**
- Below 480px (mobile portrait): Single column, bottom navigation, gestures primary
- 480-768px (mobile landscape): Single column, side page indicators, split keyboard support
- 768-1024px (tablet portrait): Two-column layout possible, glossary panel on right
- 1024-1440px (tablet landscape): Full reading view with persistent left chapter index
- 1440px and above (desktop): Three-column layout with chapter index, text, and notes/glossary

**Reading Speed Adaptation:**
- First 3 chapters: System tracks words-per-minute, scroll speed, pause frequency
- After calibration: Auto-scroll speed adjusts to learner's natural pace
- If learner frequently pauses at complex passages: Auto-highlight key terms, offer simplified paraphrase toggle
- If learner speeds through: Verify comprehension with optional checkpoint prompt

**Content Complexity Adaptation:**
- Novice mode: Simplified vocabulary, more illustrations, frequent comprehension checks
- Intermediate mode: Standard text, contextual annotations, moderate check frequency
- Advanced mode: Original source texts, minimal annotation, scholarly apparatus available
- Expert mode: Multiple manuscripts/critical editions side-by-side, academic apparatus

**Environmental Adaptation:**
- Low light: Automatic night mode with warm sepia tint, reduced blue light
- High ambient noise: Auto-enable narrator with increased volume, subtitle-like text highlight
- Interruption detection: If learner stops interacting mid-page, auto-bookmark after 2 minutes
- Motion detection (mobile): If walking detected, suggest audio-only mode with larger text

**Emotional State Adaptation:**
- If choice deliberation time exceeds threshold: Simplify choices, add recommendation badge
- If comprehension scores drop: Reduce new content density, increase review integration
- If engagement signals high: Increase content length, add deeper narrative branches
- If frustration signals detected: Offer hint system, reduce consequence severity

### 3.8 AI Interaction

**AI Narrator System:**
- Voice: Warm, scholarly tone with cultural authenticity
- Dynamic modulation: Pitch shifts for character dialogue, pace slows for dramatic moments
- Context awareness: AI adjusts emphasis based on importance of passage to knowledge goals
- Multi-voice capability: Different AI voices for different characters, narrator, and historical commentator
- Pronunciation accuracy: AI trained on historical linguistics for proper name pronunciation
- Language mixing: Code-switching support for Arabic/English bilingual narratives

**AI Context Provider:**
- When learner taps highlighted term: AI generates contextual explanation in 2-3 sentences
- When learner pauses more than 5 seconds on passage: AI whisper offers explanation
- When learner completes chapter: AI summarizes key knowledge points with connections to previous learning
- When learner makes a choice: AI provides historical context for what actually happened vs alternative scenarios

**AI Comprehension Check Generator:**
- Questions generated dynamically based on chapter content
- Difficulty calibrated to learner's demonstrated knowledge level
- Follow-up questions adapt based on previous answer correctness
- AI generates personalized explanations for incorrect answers using learner's known concepts as references

**AI Branching Narrative Engine:**
- For adaptive stories: AI generates narrative branches in real-time based on learner's choices and demonstrated interests
- AI tracks narrative threads across sessions and ensures coherence
- When learner returns to story after gap: AI provides contextual recap of relevant narrative threads
- AI adjusts future narrative based on knowledge gaps detected in comprehension checks

### 3.9 Memory Integration

**Reading Memory Events:**
- Every page view: Records exposure event in memory engine with timestamp and duration
- Every choice: Records decision event with choice, deliberation time, context
- Every comprehension check: Records knowledge verification event with score
- Every annotation: Records learner-generated content with reference to source passage
- Every reflection: Records synthesis event with learner's own words

**Spaced Repetition Scheduling:**
- Initial review: 24 hours after chapter completion
- Second review: 7 days after if comprehension score above 80% (sooner if lower)
- Knowledge nodes created from story content enter SRS with priority based on relevance to learner's knowledge goals, connection density to existing knowledge, comprehension check performance, and time spent on related passages
- Review sessions pull story excerpts as context for recall prompts

**Memory Strength Calculation:**
- Base strength: Determined by comprehension check score
- Reinforcement: Each re-reading of passage adds strength multiplier
- Contextual strength: Connections to other knowledge nodes increase strength
- Emotional strength: Choices with strong emotional engagement receive retention bonus
- Narrative strength: Information embedded in narrative arc receives 30% retention bonus over isolated facts

### 3.10 Avatar Integration

**Avatar Role in Story Mode:**
- Avatar appears as the learner's narrative guide within the story world
- Rendered as a stylized companion character (spirit animal, scholarly guide, or abstract light being)
- Appears at bottom-right corner of reading view, subtly pulsing
- Reacts to story events: leans forward during tension, relaxes during resolution
- Voice activates for contextual explanations (different from narrator voice)
- Can be tapped for guidance, hints about choices, or historical context

**Avatar Expressions:**
- Curiosity: Head tilt, eyes widen, slight forward lean
- Understanding: Nod, gentle smile, relaxed posture
- Surprise: Eyes widen, slight backward motion, raised brow
- Encouragement: Nod, open palm gesture, warm expression
- Contemplation: Hand on chin, gaze upward, slow blink
- Excitement: Brightened aura, quick bounce, sparkling particles

**Avatar Interaction States:**
- Passive: Subtle idle animation (breathing, occasional glance around)
- Attentive: Avatar watches text as it's read, follows highlighted content
- Active: Avatar speaks, gestures toward relevant content
- Suggesting: Avatar raises hand, question mark icon appears
- Celebrating: Avatar performs victory animation on chapter complete

### 3.11 Knowledge Tree Integration

**Node Creation:**
- Each story chapter generates 3-8 knowledge nodes depending on content density
- Nodes are created under the story's domain in the knowledge tree
- Narrative choices create branch nodes that track decision paths
- Comprehension check items create verification nodes
- Learner annotations create personal insight nodes

**Node Relationships:**
- Sequential nodes linked by narrative order (prerequisite flow)
- Character nodes linked to their story appearances
- Concept nodes linked across different stories (cross-narrative connections)
- Choice nodes linked to their narrative consequences
- Historical event nodes linked to timeline entries

**Knowledge Tree Visualization in Story Mode:**
- Accessible via Knowledge Map button in chapter select
- Shows which knowledge nodes will be affected by each chapter
- After reading, shows new nodes added and connections strengthened
- Growth rings on nodes indicate memory strength (more rings = stronger)
- Color coding: Blue for historical knowledge, Green for conceptual knowledge, Gold for character knowledge, Purple for personal insight

### 3.12 Reward System

**Immediate Rewards:**
- Page turn: Subtle sparkle particles on milestone pages (every 10th page)
- Chapter complete: Illustrated spread with achievement card
- Choice made: Branch path visualization with unfolding animation
- Comprehension question correct: Confetti burst contained to question area
- Annotation created: Insight planted micro-animation with seed icon

**Progression Rewards:**
- Story complete: Special cover illustration variant, Story Master badge
- All choices explored (completionist): Pathfinder achievement
- 100% comprehension score on all checks: Deep Reader achievement
- 10 stories completed: Bibliophile achievement
- 100 annotations created: Scholar achievement

**Streak Rewards:**
- Daily reading streak: Each day adds a leaf to story tree in hub
- 7-day streak: Story Seeker title unlock
- 30-day streak: Exclusive story content access
- 100-day streak: Custom story cover illustration with learner's name

### 3.13 Accessibility

Visual Accessibility:
- All text supports dynamic type scaling up to 200%
- VoiceOver/TalkBack: Full semantic labeling of all interactive elements
- High contrast mode for text and UI elements
- Reduce motion mode: Disables page curl animation, uses simple fade transitions
- Color blind mode: All information conveyed with text/icons, not color alone
- Bold text option: System-wide bold text rendering

Auditory Accessibility:
- All audio content has text alternatives (narrator has visible transcript)
- Ambient audio can be disabled independently of narrator audio
- Sound effects have visual alternatives (screen flash for page turn)
- Volume normalization across all audio sources
- Mono audio option for unilateral hearing

Motor Accessibility:
- All gesture interactions have button alternatives (tap left/right edges instead of swipe)
- Configurable tap targets (increase hit area by 24px increments)
- Swipe sensitivity adjustment for page turn
- Auto-scroll with speed control reduces need for manual interaction
- Keyboard navigation: Arrow keys, Tab for focusable elements, Space/Enter for activation
- Switch control support for all interactions
- Voice control: Turn page, Read this, Choose option one

Cognitive Accessibility:
- Dyslexia-friendly font option (OpenDyslexic or equivalent)
- Reading ruler overlay option
- Line spacing and word spacing customization
- Simplified view: Reduces page to single column, removes illustrations
- Focus mode: Highlights one sentence at a time
- Auto-pause at configurable intervals for processing time
- Progress indicator shows remaining time to chapter end
- Consistent navigation patterns throughout

---

## 4. Mode 02: Documentary Mode

### 4.1 Overview

Documentary Mode delivers rich media learning through professionally produced documentary content augmented by interactive elements. Learners watch, explore, and engage with video content enhanced by contextual AI, interactive timelines, knowledge checkpoints, and deep linking to related resources. The experience bridges passive viewing with active learning through strategic interaction points.

### 4.2 Complete UX

**User Experience Philosophy:** Watching a documentary on this platform is fundamentally different from watching on any streaming service. The learner is not a passive viewer but an active explorer. Every frame is an entry point to deeper knowledge. Every timestamp is a node in the knowledge graph. The playback experience is layered with contextual information that the learner can pull or ignore based on their current engagement level.

**Emotional Journey Map:**

| Phase | Emotion | UX Strategy |
|-------|---------|-------------|
| Discovery | Excitement, curiosity | Curated documentary grid with atmospheric trailers |
| Selection | Anticipation, intention | Detailed preview with knowledge value proposition |
| Viewing Start | Engagement, absorption | Cinematic intro sequence, minimal UI overlay |
| Deep Viewing | Flow, wonder, curiosity | Contextual information bubbles, interactive timeline |
| Knowledge Check | Confidence, challenge | Non-disruptive checkpoint integration |
| Exploration | Discovery, connection | Related content, deep dives from timestamp anchors |
| Completion | Accomplishment, fulfillment | Knowledge summary, achievement, recommendations |

### 4.3 Complete UI

**Documentary Player Interface:**

Top bar: back to Library, Documentary Title, settings, bookmark, comments, knowledge map, overflow menu

Video Player Area: Content fills available space, aspect ratio maintained, pillarbox/letterbox as needed, subtitles overlay bottom third

Playback Controls: Play/Pause, skip back/forward 10s, progress bar with chapter markers, current time / total time, speed control, chapters menu, transcript toggle, overflow settings

Context Panel: AI contextual information for current timestamp. Dynamic, non-intrusive. Shows relevant historical context, key terms, related knowledge. Expandable for deeper dives.

**Documentary Library Interface:**

Category filter chips: Featured, All, History, Science, Culture, Art. Documentary cards in responsive grid. Each card shows: thumbnail, title, duration, difficulty level, progress indicator. My Playlist, My Notes, and Knowledge quick access buttons.

**Interactive Timeline Overlay:**

Timeline shows major events/chapters with knowledge density indicators. Scrub on timeline shows frame-accurate thumbnails. Timestamp bookmarks appear as flags. Pinch to zoom timeline. Two-finger drag to pan.

### 4.4 Complete Flow

**Browse → Select → Watch → Interactive Timeline → Knowledge Check → Related Content**

1. **Entry to Documentary Hub:** Cinematic grid of documentary thumbnails loads with staggered fade-in animation (50ms per card). Featured documentary plays muted auto-preview on hover/scroll-past. Category filters at top. Search bar expands on focus.

2. **Documentary Selection:** Tapping a documentary card triggers a smooth transition: card rises to fill screen (600ms), thumbnail crossfades to video player background, play button appears centered with pulse animation. Documentary metadata slides in from bottom.

3. **Pre-Play Options:** Learner can preview 30-second trailer, view chapters with timestamps, read expanded description with learning objectives, check prerequisite knowledge nodes required, preview knowledge gain, add to playlist with optional reminder.

4. **Playback Start:** Tapping Play triggers a 1-second cinematic intro sequence. UI overlays fade out after 3 seconds of playback. AI context panel starts in collapsed mode.

5. **Active Viewing:** Learner watches content. Auto-hide controls disappear after 3s, reappear on tap/click/mousemove. Context panel updates every 30-60 seconds with relevant AI-generated information.

6. **Interactive Pause:** When learner pauses, the frame subtly sharpens, contextual information expands around the frozen frame. Key elements in the frame are identified with labels. Timeline appears at bottom showing current position.

7. **Timeline Engagement:** Learner can open the interactive timeline at any point. Timeline shows major events with knowledge density indicators. Learner can jump to any timeline point.

8. **Transcript Interaction:** Learner opens transcript panel. Transcript scrolls in sync with playback. Current sentence is highlighted. Tap any sentence to jump to that timestamp. Key terms hyperlinked to knowledge graph.

9. **Knowledge Check Insertion:** At pre-defined intervals (every 5-7 minutes or at chapter boundaries), a non-disruptive knowledge check appears. Question overlaid on video with auto-pause. Immediate feedback with explanation. Playback resumes automatically.

10. **Bookmarking:** Learner can bookmark any moment. Bookmark captures timestamp, frame thumbnail, AI-generated context summary, and learner's optional note.

11. **Related Content Suggestions:** AI analyzes content and suggests related documentaries, articles, knowledge tree nodes, and quiz challenges.

12. **Completion:** Full-screen completion screen with knowledge summary, comprehension score, watch time, bookmarks count, related documentaries grid.

### 4.5 Animations

- Video Playback Transitions: Load start spinner with documentary brand colors, quality change seamless (200ms buffer), seeking with smooth jump (150ms fade), end with crossfade to completion (500ms)
- Context Panel: Slides up from bottom (350ms spring), content fades between updates (200ms each direction)
- Timeline Overlay: Rises from bottom with blur backdrop (400ms spring), event markers pulse
- Knowledge Check: Question card scales up from center (0 to 1, 400ms spring), video dims to 40% behind. Correct answer draws green checkmark (300ms). Incorrect shows red X with correct answer highlight (400ms)
- Bookmark: Ribbon icon bursts from center with particle celebration (500ms)

### 4.6 Interactions

| Gesture | Action |
|---------|--------|
| Tap video | Toggle controls (fade in/out, 300ms) |
| Double-tap left/right | Skip back/forward 10s |
| Swipe up on video | Open context panel |
| Swipe down on context | Close context panel |
| Pinch on timeline | Zoom timeline in/out |
| Long-press bookmark | Add note |
| Swipe left on recommendation | Dismiss |
| Swipe right on recommendation | Save to playlist |
| Two-finger double-tap | Toggle fullscreen |

### 4.7 Adaptive UI

- Connection Speed: High bandwidth (5Mbps+) defaults to 1080p with 4K option. Medium (1-5Mbps) 720p adaptive. Low (<1Mbps) 480p with audio-only fallback. Offline plays at original quality.
- Viewing Context: Full attention mode with full UI. Casual mode with audio emphasis and larger subtitles. Commute mode with download-for-offline prompt and gesture-optimized controls.
- Knowledge Level: Beginner gets basic explanations and frequent checks. Intermediate gets balanced depth. Advanced gets scholarly sources and debates. Expert gets analytical knowledge checks.
- Content Sensitivity: Content warnings based on learner's sensitivity preferences. Sensitive scenes have warning with skip option.

### 4.8 AI Interaction

- AI Context Provider: Generates real-time contextual annotations for current video content. Identifies key concepts, people, locations, and events visible on screen. Provides depth options from Tell me more to Deep dive.
- AI Content Recommendation: Analyzes viewing patterns, pause points, rewinds, and note-taking to identify interest areas. Suggests next documentaries based on interest affinity scores.
- AI Transcript Enhancement: Generates keyword highlighting based on knowledge relevance. Produces chapter summaries automatically. Creates named entity links to knowledge graph.
- AI Note Generation: Auto-generates structured notes from documentary content. Extracts key quotes, statistics, and dates with timestamps.

### 4.9 Memory Integration

- Viewing Memory Events: Watch duration, pauses, rewinds, fast-forwards, bookmarks, notes, knowledge check results, timeline exploration
- Spaced Repetition: Key facts identified by AI scheduled for review. Visual memory prompts using documentary frames. Audio memory prompts using excerpts. Timeline-based chronological ordering quizzes.
- Memory Strength: Watch completion adds 25% strength. Knowledge check success adds 15% per correct answer. Note creation adds 10% per note. Re-watching adds 5% per re-watch (diminishing after 3rd).

### 4.10 Avatar Integration

Avatar appears as documentary host/guide in context panel. Provides commentary, context, and reactions to content. Aware of learner's viewing history. Can be minimized to audio-only. Tap to ask questions about current content. Suggests related documentaries. Can debate or offer alternative perspectives on claims.

### 4.11 Knowledge Tree Integration

Each documentary chapter generates dedicated knowledge nodes. Visual content nodes with image references. Narrative and thematic nodes. Timestamps serve as knowledge references. Knowledge edges include documented in relationship type.

### 4.12 Reward System

- First documentary completed: Viewer badge
- 10 documentaries: Documentarian badge
- 50 documentaries: Scholar of the Screen badge
- 100 bookmarks: Curator badge
- 50 notes taken: Note Taker badge
- 90%+ average knowledge check score: Attentive Viewer badge
- Daily viewing streak fills film roll icon

### 4.13 Accessibility

Visual: Closed captions with customization, audio description track, high contrast controls, screen reader support, flash reduction settings
Auditory: Full transcript synchronized with playback, visual indicators for audio-only information, volume normalization, sign language interpretation window option
Motor: All gestures have button alternatives, keyboard shortcuts, configurable control timeout, large skip buttons, voice control support
Cognitive: Playback speed from 0.25x to 2x, chapter markers, auto-pause option at configurable intervals, Summarize so far button, content warnings with skip, simplified transcript view, focus mode

---

## 5. Mode 03: Podcast Mode

### 5.1 Overview

Podcast Mode provides an audio-first learning experience designed for mobile and background consumption. Learners browse educational podcast episodes, listen with synchronized transcripts, take timestamped notes, create clips, and manage playlists. The mode is optimized for hands-free, eyes-free operation during commutes, exercise, and daily routines.

### 5.2 Complete UX

**User Experience Philosophy:** Podcast Mode respects that the learner is often doing something else while listening. The interface is designed for minimal visual engagement, maximum audio quality, and seamless transitions between listening contexts. Every interaction that requires visual attention can be deferred, voice-activated, or simplified to a single tap.

### 5.3 Complete UI

**Podcast Player Interface:**
- Episode artwork with audio-reactive visualizer option
- Progress bar with chapter markers
- Playback controls: Play/Pause, skip forward/backward, speed selector, sleep timer
- Tab bar: Transcript (synced), Notes, Clips, Show Notes
- Synced transcript panel with word-level highlighting

**Episode Library Interface:**
- Currently playing prominent at top
- Continue Listening carousel showing in-progress episodes
- Recommended for You row with AI-curated content
- Category chips for filtering
- Search with voice search option

### 5.4 Complete Flow

1. **Entry to Podcast Hub:** Dynamic layout loads with currently playing episode prominent. Continue listening carousel. Recommendations row. Category chips.

2. **Episode Selection:** Card transitions to player view with artwork crossfade (400ms). Episode metadata appears. Play button pulses. Start from beginning vs Resume options.

3. **Playback Start:** Audio begins after short loading (<2s startup). Artwork animates subtly. Transcript begins syncing. Lock screen/notification center now playing activates. AI-generated show notes summary slides up briefly then collapses.

4. **Active Listening:** Audio continues with screen off. Lock screen shows episode info and controls. Transcript scrolls automatically if screen is on.

5. **Transcript Following:** Current sentence highlighted. Tap any line to seek. Key terms highlighted and linked to knowledge graph.

6. **Note Taking:** Double-tap transcript line or tap note button. Timestamped note entry appears with selected text context and auto-generated topic tag.

7. **Clip Creation:** Long-press clip button to start clipping. Selection handles on progress bar. Set start/end. Preview with 5-second rewind. Save with title and tags.

8. **Playlist Management:** Add episodes to playlists. Drag-reorder, auto-play next, crossfade between episodes (0-5s). Smart playlists for unfinished episodes and new content.

9. **Sleep Timer:** Set timer from player options (15/30/45/60 minutes or end of episode). Audio gently fades at 30 seconds remaining. Position saved.

10. **Episode Completion:** Sonic wave resolves to checkmark. Knowledge summary card appears. Next Episode auto-plays after configurable countdown. Add to Review Queue option.

### 5.5 Animations

- Player Transition: Card scales and moves to player position (500ms spring)
- Progress Bar: Continuous smooth fill with scrub preview tooltip
- Transcript: Line highlight transitions smoothly between lines (200ms crossfade)
- Sleep Timer: Timer icon animates from default to active. Countdown with gentle pulse. Warning vibration at 30s. Audio fade out (3s).
- Clip Creation: Button transitions to active, handles slide in (300ms). Selection area highlights. Clip card flies into panel (400ms).

### 5.6 Interactions

| Gesture | Action |
|---------|--------|
| Tap artwork | Toggle mini/full player |
| Swipe down on player | Minimize to mini-player (300ms) |
| Swipe up on mini-player | Expand to full player (300ms) |
| Double-tap left/right | Skip 15s back/forward |
| Swipe left on transcript | Reveal note creation |
| Long-press clip button | Start clip selection |
| Pinch player | Toggle fullscreen artwork |
| Shake device | Undo last action |

### 5.7 Adaptive UI

- Driving mode: Giant buttons, voice control priority, minimal screen interaction
- Walking mode: Larger tap targets, gesture-based skip
- Workout mode: High contrast controls, sweat-resistant gesture detection
- Bedtime mode: Warm color theme, volume normalization, gentle fade timer
- Commute mode: Download-for-offline prompt, data-saving mode
- Audio quality adapts: WiFi high bitrate (320kbps), cellular unlimited (256kbps), cellular limited (128kbps mono)

### 5.8 AI Interaction

- AI Show Notes: Auto-generates episode summary from speech-to-text analysis. Extracts key topics, people, places. Creates structured outline with timestamps.
- AI Voice Synthesis: Option to have AI narrate transcripts when video unavailable. Multi-language AI dubbing.
- AI Content Recommendations: Analyzes listening patterns, skip behavior, note-taking frequency. Creates personalized podcast feed.

### 5.9 Memory Integration

- Listening events: Duration, completion percentage, skip pattern
- Note events: Timestamp, content, associated knowledge tags
- Clip events: Audio segment saved with metadata
- Spaced repetition: Key concepts scheduled for review. Audio-based recall with 30-second clips. Note-based review prompts.

### 5.10 Avatar Integration

Avatar appears as podcast co-host or commentator. Small avatar icon in player corner. Can interject with contextual comments. Toggle for guided listening with pre-episode context, mid-episode explanations, post-episode synthesis.

### 5.11 Knowledge Tree Integration

Episode completion creates knowledge pod (audio-linked node). Key concepts create sub-nodes. Learner notes create personal insight nodes. Clips create reference nodes with audio attachment.

### 5.12 Reward System

- First episode: Listener badge
- 10 hours: Dedicated Listener badge
- 100 hours: Audiophile Scholar badge
- 500 hours: Walking Library badge
- 50 notes: Note Catcher badge
- 25 clips: Clip Curator badge
- Daily listening streak with sonic wave fill

### 5.13 Accessibility

Visual: Full VoiceOver/TalkBack, dynamic type to 200%, high contrast controls, transcript always available, large tap targets (60x60px)
Auditory: Volume normalization (LUFS -16), speech enhancement mode, mono audio option, left/right balance, MFi hearing aid support
Motor: Full keyboard control, voice control, switch control, configurable skip duration, headphone controls, Siri/Google Assistant shortcuts
Cognitive: Speed 0.25x to 2x, word-level transcript highlighting, sleep timer, chapter markers, Catch me up summary

---

## 6. Mode 04: Dialogue Mode

### 6.1 Overview

Dialogue Mode enables learners to have natural conversations with AI-powered personas of historical figures, scholars, and fictional characters. Each persona is built on a comprehensive knowledge base of the figure's actual writings, recorded statements, historical context, and scholarly interpretations. The AI generates responses in the character's voice, with period-accurate knowledge, while the avatar renders the character with appropriate visual appearance, expressions, and gestures.

### 6.2 Complete UX

**User Experience Philosophy:** This is not a chatbot. This is a time machine for conversation. The learner should feel they are genuinely speaking with the historical figure. The suspension of disbelief is paramount. Every response must be authentic to the character's known views, speaking style, and historical context. Anachronisms are avoided. The character cannot know about events after their lifetime.

### 6.3 Complete UI

**Dialogue Conversation Interface:**
- Character avatar with full animation in conversation area
- Message bubbles: Character messages with avatar, learner messages from opposite side
- Thinking indicator with animated dots and avatar expression shift
- Suggested follow-up questions generated by AI
- Source citation indicator on character messages
- Context panel showing topic map, key terms, source texts

**Character Selection Interface:**
- Category filters: Historical, Scholars, Scientists, Philosophers, Literary, Artists, Explorers
- Featured character with cinematic background, rating, conversation count
- Character grid with portrait, name, field, rating, conversation count
- Previously conversed characters show Continue badge

**Character Profile Interface:**
- Large hero portrait in scholarly setting
- Full name, lifespan, location, disciplines
- Biography with key works and known for sections
- Action buttons: Start Conversation, View Works, Influences, Timeline
- Conversation stats and learner's history

### 6.4 Complete Flow

1. **Entry to Dialogue Hub:** Character grid loads with staggered card animation. Featured character with cinematic background. Previously conversed characters highlighted.

2. **Character Selection:** Card zooms to full profile (500ms). Profile shows comprehensive character information.

3. **Conversation Initiation:** Transition to conversation view (800ms). Character renders with greeting animation. Opening message appears with typing animation.

4. **Learner Input:** Type question or tap microphone for voice input. Voice input transcribes in real-time. Character expression shifts to listening state.

5. **AI Response Generation:** Multi-layer processing: knowledge retrieval, voice synthesis, historical context check, source attribution, expression mapping. Takes 1-3 seconds.

6. **Response Delivery:** Character shifts to thinking expression. Response streams with character voice synthesis. Text appears word-by-word. Source citation appears.

7. **Suggested Follow-ups:** AI generates 3-4 follow-up questions based on unfinished threads, learner interests, knowledge gaps, and natural progression.

8. **Deep Exploration:** Conversation continues with context awareness within session. Key concepts tracked in context panel.

9. **Context Learning:** Context panel updates in real-time with topic map, key terms, source texts, related knowledge nodes, historical timeline placement.

10. **Insight Saving:** Save any response as insight. Includes character name, full text, timestamp, source citations, learner's notes.

11. **Session End:** Farewell button triggers period-appropriate farewell. Session summary: topics covered, insights saved, knowledge nodes updated. Option to schedule follow-up.

### 6.5 Animations

**Character Avatar Animations:**
- Idle: Subtle breathing (2s cycle), occasional glance, micro-expressions every 8-12s
- Listening: Head tilt, attentive eyes, eyebrows slightly raised, 5% forward lean
- Thinking: Gaze shifts upward 15 degrees, hand toward chin, eyes narrow (1.5s transition)
- Speaking: Lipsync with audio, natural head movements, hand gestures, expression changes
- Greeting: Character-specific entrance animation
- Farewell: Character-specific departure animation
- Reacting: Emotional response to learner's input

**Message Bubble Animations:**
- Character message: Bubble expands from avatar side, text types word-by-word
- Learner message: Bubble expands from input side (200ms)
- Streaming text: Characters appear at voice-speed with subtle glow
- Source citation: Icon fades in at bubble corner (300ms)
- Error state: Gentle red shimmer, retry button (400ms)

**Thinking Indicator:**
- Three dots sequentially scaling up and down (400ms per dot, 200ms stagger)
- Character avatar expression shifts during thinking
- Duration dynamically matches response generation (min 1s, max 4s)

### 6.6 Interactions

| Gesture | Action |
|---------|--------|
| Tap suggested question | Send question, response begins |
| Long-press message | Save as insight |
| Swipe left on message | Reply with quote |
| Double-tap message | View sources |
| Pinch character avatar | Zoom/tilt camera around character |
| Swipe up on context panel | Expand context |
| Tap source citation | Open source text overlay |
| Microphone tap | Voice input with waveform |
| Swipe down on conversation | View history list |

### 6.7 Adaptive UI

**Conversation Depth:**
- Beginner: Simple vocabulary, more context, shorter responses
- Intermediate: Balanced responses, occasional specialized concepts
- Advanced: Full scholarly vocabulary, primary source references
- Expert: Character challenges assumptions, presents counterarguments

**Language Adaptation:**
- Character responds in learner's chosen language
- Historical figures code-switch for precision terms
- Key terms in original language with translation
- Classical language option available

**Session Context:**
- Short sessions (<5 min): Single topic, concise answers
- Medium (5-15 min): Natural flow, multiple topics
- Long (>15 min): Deep exploration, character introduces related topics
- Returning: Character references previous conversations

### 6.8 AI Interaction

**AI Persona Engine:**
- Learner Query flows through Intent Classification, Topic Identification, Knowledge Level Assessment
- Persona Knowledge Base Retrieval from Primary Sources, Secondary Scholarship, Historical Context, Conversation Memory
- Response Generation ensures Content Accuracy, Period-Appropriate Voice, Anachronism Filtering, Source Attribution
- Multi-modal Output includes Text, Voice Synthesis, Expression Mapping, Gesture Selection

**Character Knowledge Boundaries:**
- Character only knows information up to their death date
- Cannot reference post-era events or discoveries
- Exception: Can discuss how their ideas were received by later generations
- Unknown topics get period-appropriate curiosity or deflection
- Disagreements reflect actual historical positions

**Voice Synthesis:**
- Character-specific voice models
- Period accent based on historical linguistics
- Multiple language support
- Emotional range modulating for different response types

### 6.9 Memory Integration

- Message event: Each exchange recorded with full text, timestamp, topic tags
- Insight saved: Full response stored as knowledge artifact
- Topics explored tracked with depth indicator
- Questions recorded for interest profiling
- Spaced repetition: Key concepts scheduled for review with character-contextual prompts
- Knowledge nodes attributed to character with perspective attribute

### 6.10 Avatar Integration

Avatar IS the historical figure, rendered with historical accuracy. Period-accurate appearance, attire, setting, and props. Expression system with 12 base expressions mapped across valence, arousal, dominance, and focus dimensions. Micro-expressions (200ms) that flash during conversation. Smooth morphing between expressions (300-800ms). Voice system with TTS, lip-sync, emotional tone mapping, breath sounds, and non-verbal vocalizations.

### 6.11 Knowledge Tree Integration

Each conversation creates character-specific knowledge nodes. Topics create concept nodes linked to character. Saved insights create personal reference nodes. Threads create narrative nodes. Relationship types include Discussed with, Learned from, Character's view on, and Explored in conversation on.

### 6.12 Reward System

- First conversation: First Encounter badge
- 10 conversations: Dialoguer badge
- 50 conversations: Prolific Conversationalist badge
- 10 different characters: Renaissance Spirit badge
- 100 messages in one conversation: Deep Dive badge
- 50 insights saved: Insight Collector badge
- 500 questions asked: Curious Mind badge

### 6.13 Accessibility

Visual: Character responses as pure text, screen reader announces character name and expression state, high contrast bubbles, font size adjustable, simplified text-only view
Auditory: All voice has text equivalent, voice speed adjustable, character voice can be disabled, full transcript available, captions visible by default
Motor: Suggested questions reduce typing, voice input support, keyboard navigation, switch control, large tap targets on suggestions (60px height minimum)
Cognitive: Conversation history always visible, suggested questions reduce formulation load, clear thinking/speaking/finished indicators, topic tags on messages, Summarize so far button, pause feature

---

## 7. Mode 05: Timeline Mode

### 7.1 Overview

Timeline Mode enables chronological exploration of historical events, biographical milestones, and conceptual developments. Learners navigate through time with interactive scrolling, zooming, and filtering. Events are presented with rich detail, causal connections, and parallel perspectives. The mode transforms abstract chronology into a navigable, visual experience.

### 7.2 Complete UX

**User Experience Philosophy:** Time is the ultimate organizing principle of knowledge, but it is not a single line. Every moment contains multiple threads — political, scientific, cultural, personal — running in parallel. Timeline Mode gives learners the ability to see these threads simultaneously, to zoom from the panoramic view of centuries to the intimate view of a single year, and to understand how events connect across domains.

### 7.3 Complete UI

**Timeline View:**
- Horizontal timeline track with infinite scroll
- Events displayed as labeled nodes on timeline
- Connection lines between related events (Bezier curves)
- Era markers as vertical bands indicating historical periods
- Perspective lanes for parallel timelines (1-4 lanes)
- Category filter chips and view mode toggle

**Event Detail:**
- Full event card with title, date (multiple calendar systems), location, category tags
- Comprehensive description with expandable sections
- Timeline context: Before and After events with time distance
- Related connections with type labels
- Related media cards (documentaries, podcasts, quizzes)

### 7.4 Complete Flow

1. **Entry to Timeline Hub:** Available timelines displayed as horizontal bands with era previews. Each card shows title, date range, event count, knowledge value.

2. **Timeline Selection:** Full-screen timeline loads at scroll position of earliest event or last viewed. Smooth entry animation (600ms).

3. **Navigation:** Horizontal scroll through time. Vertical position for category/perspective. Pinch to zoom between centuries and individual years.

4. **Event Discovery:** Event nodes sized by historical significance. Color-coded by category. Connected events with curved lines. Related clusters pulse gently.

5. **Event Selection:** Tap expands event inline (preview card, 300ms). Tap View Details for full event detail. Connection lines highlight.

6. **Event Detail View:** Slides up from bottom (400ms spring). Comprehensive content with description, dates, location, tags, sources, images, media links.

7. **Parallel Timeline:** Toggle multiple perspectives (Political, Scientific, Cultural, Religious). Vertical dashed lines connect related events across lanes.

8. **Connection Exploration:** Causes, effects, contemporaneous events, related figures, locations, thematic connections. Each connection type has distinct visual style.

9. **Knowledge Capture:** Save events to knowledge tree. Add to study list. Related media links.

10. **Exit:** Saves scroll position, zoom level, filters, and open event.

### 7.5 Animations

- Timeline Scroll: Smooth 60fps with inertia, optional snap to decade/century
- Zoom: Smooth with focus point under finger, content fade transition at thresholds
- Event Nodes: Subtle pulse idle (3s cycle), hover expands 15%, selection grows to card (300ms)
- Connection Lines: Ripple effect to connected nodes on selection (500ms)
- Parallel Timeline Split: Timeline splits into colored lanes (500ms spring)
- Era Transition: Background color subtly shifts to era theme (800ms crossfade)

### 7.6 Interactions

| Gesture | Action |
|---------|--------|
| Horizontal scroll/drag | Pan timeline with momentum |
| Pinch | Zoom in/out with density change |
| Double-tap | Focus on era or event |
| Tap event node | Expand preview inline |
| Long-press event | Quick save to knowledge |
| Swipe up on preview | Open full detail |
| Swipe down on detail | Close detail |
| Two-finger tap | Toggle parallel view |
| Tap connection line | Show connection type label |

### 7.7 Adaptive UI

- Century view: Major events only, categorically grouped, density clusters
- Decade view: Significant events, color differentiation
- Year view: All events, full labels, connection lines
- Month/day view: Detailed sequences, precise chronology
- Sparse periods: Events spread with contextual background
- Dense periods: Compacted with label collision avoidance
- Personal relevance: Learner's saved events always visible

### 7.8 AI Interaction

AI provides contextual connections (At this same time, in another part of the world...). Identifies patterns across events. Suggests missed connections. Generates thematic groupings. Supports guided exploration: Show causes, Show what happened next, Compare to another period, Find events related to concept.

### 7.9 Memory Integration

- Event view recorded with timestamp and duration
- Connection exploration tracked
- Saved events trigger knowledge node creation
- Spaced repetition: Chronological ordering quizzes, temporal association, era identification

### 7.10 Avatar Integration

Avatar appears as time-traveling guide at bottom of timeline. Provides commentary on events. Can narrate timeline tours. Expression shifts based on period being viewed. Highlights connections between learner's viewed events. Suggests events based on knowledge tree.

### 7.11 Knowledge Tree Integration

Events viewed in detail create timeline nodes. Saved events create permanent temporal nodes. Connection exploration creates relationship edges. Timeline branch in tree visualizes chronological understanding. Temporal gaps highlighted as exploration areas.

### 7.12 Reward System

- First event: Time Traveler badge
- 100 events: Century Walker badge
- 1000 events: Millennium Voyager badge
- All events in a century: Century Master badge
- All connection types explored: Connector badge
- 50 events saved: Historian badge

### 7.13 Accessibility

Visual: Timeline data as sortable list alternative. Color/shape/pattern differentiation. High contrast connections. Zoom text with content. Screen reader announces events in view.
Auditory: Event details read aloud. Audio-described tours. Ambient optional audio.
Motor: Keyboard navigation (arrows, +/-, Enter). Tab through chronological events. +/- buttons as pinch alternative. Large touch targets (48px minimum). Voice control.
Cognitive: Adjustable timeline complexity. Consistent navigation. Clear labels. Guided tour option. Automatic bookmarks at event boundaries.

---

## 8. Mode 06: Map Mode

### 8.1 Overview

Map Mode enables geographic exploration of history, culture, and knowledge. Learners navigate an interactive map with historical layers, discovering how geography shaped events and how knowledge traveled across regions. The mode features zoom levels with changing information density, historical map overlays, and animated routes showing migrations, trade routes, and the spread of ideas.

### 8.2 Complete UX

**User Experience Philosophy:** Geography is not the background of history; it is a primary actor. Map Mode lets learners see knowledge spatially — where ideas were born, how they traveled, what terrain they crossed, and why certain places became centers of learning. The map is both a discovery tool and a visualization of knowledge geography.

### 8.3 Complete UI

**Map View Interface:**
- Full-screen interactive map with terrain/base map
- Historical overlay toggle (boundaries, trade routes, knowledge centers)
- Location markers (knowledge sites) with cluster expansion
- Animated routes (trade, migration, ideas)
- Knowledge density heatmap layer
- Location preview card on tap

**Route Animation Interface:**
- Animated routes with dashed flow direction
- Traveler icons moving at adjustable speed
- Waypoint markers with city names
- Timeline slider for historical period adjustment
- Route details: length, duration, era, goods, ideas, key stops

### 8.4 Complete Flow

1. **Entry to Map Hub:** Globe or flat map renders with smooth animation. Quick start suggestions. Previously explored regions highlighted.

2. **Map Navigation:** Pan and zoom. Levels: Continent, Region, Country, City, District, Building. Each level reveals appropriate information density.

3. **Historical Layers:** Toggle overlays for historical boundaries, trade routes, knowledge centers, conflict zones, cultural regions.

4. **Location Discovery:** Tap marker for preview card. Cluster markers expand on tap. Card shows significance, knowledge node count, related figures.

5. **Location Deep Dive:** Full detail card with historical description, timeline of events, notable figures, images, related knowledge nodes, media links.

6. **Route Exploration:** Select historical routes. Animation plays with traveling markers. Timeline slider adjusts route evolution. Waypoint cities expandable.

7. **Knowledge Layer:** Heatmap overlay shows scholarly output density across geography. Timeline slider adjusts for different eras.

8. **Location Connection:** Connected locations shown with lines. Journey mode creates multi-stop exploration path.

9. **Knowledge Capture:** Save locations, routes, regions to knowledge tree.

### 8.5 Animations

- Map Panning: Smooth 60fps with momentum and deceleration
- Zoom: 1:1 finger tracking, content fades at thresholds, labels resize
- Location Markers: Gentle pulse idle (2s cycle), scale-in on new discovery (500ms)
- Routes: Path draws from start to end (3-8s), animated dashes flow direction, cities light up on passage
- Historical Overlay: Fade in (500ms), borders draw themselves (1-2s), palette shifts to period colors

### 8.6 Interactions

| Gesture | Action |
|---------|--------|
| Drag | Pan map with momentum |
| Pinch | Zoom in/out with 1:1 tracking |
| Double-tap | Zoom in to point |
| Two-finger double-tap | Zoom out |
| Tap location marker | Preview card from marker point |
| Long-press location | Quick save |
| Swipe up on preview | Full detail card |
| Swipe down on detail | Close detail |
| Two-finger rotate | Rotate map, compass updates |
| Three-finger tap | Reset north (300ms) |

### 8.7 Adaptive UI

- Globe (z1-3): Continents, major historical regions, knowledge heatmap
- Region (z4-6): Countries, major cities, empire boundaries, trade routes
- Local (z7-9): Cities, towns, historical sites, battle locations
- Detailed (z10-12): Districts, buildings, specific locations, archaeological sites
- Maximum (z13+): Individual structures, street-level historical overlays
- Exploration modes: Free, Guided tour, Quest mode, Comparison mode
- Offline: Downloaded regions, cached details, reduced tile resolution

### 8.8 AI Interaction

AI Geography Guide provides regional context on focus. Explains geographic influence on historical developments. Identifies environmental factors. Generates location narratives. AI Route Generator supports queries like Show how paper-making spread, Trace Al-Biruni's travels, Show routes connecting learning centers. AI Location Recommendations based on knowledge gaps.

### 8.9 Memory Integration

- Location view recorded with duration
- Route exploration logged
- Layer usage tracked
- Spaced repetition: Location recall (map with blank markers), Route recall, Geographic context
- Geographic knowledge nodes created, route connection nodes, regional clusters

### 8.10 Avatar Integration

Avatar as geographer/explorer in lower corner. Provides geographic context. Reacts to explored locations. Can suggest next exploration based on knowledge gaps. Highlights locations related to recent learning. Can narrate route journeys.

### 8.11 Knowledge Tree Integration

Geographic nodes for explored locations. Regional nodes organize clusters. Route nodes connect geographically separated knowledge. Knowledge tree viewable with geographic layout option. Connections show knowledge transfer across geography.

### 8.12 Reward System

- First location: Explorer badge
- 50 locations: Cartographer badge
- 200 locations: Geographer badge
- All major trade routes: Silk Road Scholar badge
- All locations in region: Regional Expert badge
- All historical layers used: Deep Mapper badge
- 100 connected locations: Network Cartographer badge

### 8.13 Accessibility

Visual: List view alternative to visual map. Labels not just color. High contrast map style. Reduced motion disables route animations. Screen reader announces location and directions.
Auditory: Location descriptions read aloud. Route narration. Audio cues for layers. Optional ambient geographic sounds.
Motor: Keyboard arrows, +/- zoom, Tab through markers. +/- button alternative to pinch. Voice control. Configurable gesture sensitivity.
Cognitive: Adjustable map complexity. Guided tour reduces decisions. Simplified layer toggles. Clear labels. Consistent navigation.

---

## 9. Mode 07: Character Mode

### 9.1 Overview

Character Mode provides deep profiles of historical and scholarly figures. Learners explore comprehensive biographies, primary works, influences, timelines, and relationships between characters. The mode functions as a character encyclopedia with rich interconnections, quote collections, and a works browser.

### 9.2 Complete UX

**User Experience Philosophy:** Every character is a gateway to a world of knowledge. By understanding a figure's life, works, influences, and context, learners gain a human-centered perspective on history and ideas. Characters are not entries in a database but living portals to their era and domain.

### 9.3 Complete UI

**Character Profile Interface:**
- Large hero portrait with parallax scroll
- Character name, lifespan, location, disciplines, famous quote
- Tab bar: Biography, Works, Influences, Timeline, Relations, Quotes, Influenced
- Biography section with expandable subsections
- Key works cards with cover, date, rating, description

**Character Relationship Graph:**
- Force-directed graph showing character connections
- Nodes sized by significance, edge labels by relationship type
- Relation types: influenced, collaborated, studied under, corresponded, translated, patron, contemporary
- Drag to explore, tap for detail, pinch to zoom
- Relationship summary panel below graph

### 9.4 Complete Flow

1. **Entry to Character Hub:** Character grid by category (Philosophers, Scientists, Writers, etc.). Featured character with cinematic background. Search with autocomplete. Filter by era, region, field.

2. **Character Selection:** Card zooms to hero portrait (600ms). Profile content slides up below. Tabs for different sections.

3. **Biography Exploration:** Life story with expandable sections (Early Life, Education, Contributions, Later Years, Legacy). Inline illustrations. Terms linked to knowledge graph.

4. **Works Browser:** Works tab with cards for each known work. Sort by date or significance. Each card includes translated title, date, description, rating, available formats.

5. **Influence Network:** Influences tab shows who influenced this character and who they influenced. Visual graph with timeline context.

6. **Character Timeline:** Mini timeline of character's life with major events. Linked to full Timeline Mode.

7. **Relationship Graph:** Force-directed graph of character connections. Explore relationships interactively.

8. **Quote Collection:** Curated quotes with source citations. Save favorites. Share quotes. Each quote linked to context.

9. **Connection to Learner:** Shows how this character connects to learner's knowledge tree. Character's nodes already learned. Suggested learning paths involving this character.

### 8.5 Animations (Character Mode)

- Hero Portrait: Parallax effect on scroll, subtle zoom on entry (600ms)
- Tab Transitions: Content slides horizontally between tabs (300ms)
- Relationship Graph: Force-directed physics with drag physics (continuous)
- Graph Node Tap: Node pulses, connection lines highlight (400ms)
- Quote Card: Flips to show source citation on tap (3D flip, 400ms)
- Works Card: Expand animation showing details (300ms spring)

### 8.6 Interactions (Character Mode)

| Gesture | Action |
|---------|--------|
| Scroll profile | Parallax hero, section navigation |
| Tap tab | Switch content section (300ms slide) |
| Tap work card | Open work detail |
| Drag graph node | Reposition in force layout |
| Tap graph node | Open character detail |
| Pinch graph | Zoom in/out |
| Long-press quote | Save to favorites |
| Swipe left on work | Quick add to study list |

### 8.7 Adaptive UI (Character Mode)

- Novice: Simplified biography, key works only, basic relationships
- Intermediate: Full biography, all works, detailed relationships
- Advanced: Scholarly apparatus, critical editions, influence debates
- Expert: Primary sources, manuscript variations, academic discourse

### 8.8 AI Interaction (Character Mode)

AI provides contextual connections between characters and learner's knowledge. Generates personalized significance assessments. Creates study paths through related characters. Answers questions about character relationships. Synthesizes character's overall worldview from their works.

### 8.9 Memory Integration (Character Mode)

Character view creates biographical knowledge nodes. Works creation adds literary/scholarly nodes. Relationship exploration creates connection edges. Quote saving creates reference nodes. Spaced repetition includes character identification and work attribution.

### 8.10-8.12 (Structure continued)

### 8.10 Avatar Integration (Character Mode)

Avatar appears as a librarian or archivist guide within character mode. Provides navigational assistance, suggests related characters, highlights interesting connections, and offers context on character's historical significance.

### 8.11 Knowledge Tree Integration (Character Mode)

Character profiles create comprehensive biographical nodes with relationships to works, influences, and contemporaries. Character nodes serve as hubs connecting multiple knowledge domains. Learner's interactions with characters build a personal scholarly network.

### 8.12 Reward System (Character Mode)

- First character viewed: Biographer badge
- 20 characters viewed: Historian of Lives badge
- All works of one character explored: Complete Scholar badge
- 50 quotes saved: Quote Collector badge
- 10 relationship connections explored: Networker badge

### 8.13 Accessibility (Character Mode)

Visual: Text alternative for hero portrait. Screen reader announces character details. High contrast for relationship graph lines. Font scaling for biography text.
Auditory: Biography read aloud. Character names pronounced correctly. Graph connections narrated.
Motor: Keyboard navigation through tabs and sections. Tab through graph nodes. Voice control for actions.
Cognitive: Clear section organization. Progressive disclosure of biography. Consistent tab layout. Graph complexity adjustable.

---

## 10. Mode 08: Quiz Mode

### 10.1 Overview

Quiz Mode provides assessment and knowledge reinforcement through diverse question types. Learners test their understanding, receive immediate feedback, and strengthen memory through active recall. The mode features multiple question formats, timer modes, adaptive difficulty, and spaced repetition integration.

### 10.2 Complete UX

**User Experience Philosophy:** A quiz is not a test; it is a conversation with the learner about what they know. Every question is an opportunity to reinforce understanding, identify gaps, and build confidence. Feedback is immediate, constructive, and educational. The goal is not to catch the learner being wrong but to help them discover what they know and what they need to revisit.

### 10.3 Complete UI

**Quiz Question Interface:**

Question card centered with progress indicator. Question type adapts UI:
- Multiple Choice: Option cards with radio buttons, images optional
- Drag and Drop: Draggable items with drop zones
- Ordering: Numbered list items, drag to reorder
- Matching: Two columns, drag lines between matches
- Fill in Blank: Text input field with auto-complete suggestions

Feedback panel: Correct/incorrect indicator, explanation, confidence self-assessment, related knowledge link.

**Quiz Selector Interface:**

Quiz types: Topic quizzes, Mixed review, Daily challenge, Timed challenge, Spaced repetition review. Quiz cards with question count, estimated time, difficulty, topic tags.

### 10.4 Complete Flow

1. **Quiz Selection:** Learner selects quiz type from available options. Quiz preview shows question count, topics covered, estimated time, difficulty, knowledge value.

2. **Quiz Start:** Transition to question view. First question appears with entrance animation. Timer starts if enabled.

3. **Question Interaction:** Learner answers question. Question type determines interaction method. Progress indicator updates.

4. **Immediate Feedback:** After answering, feedback appears: correct/incorrect with color indicator, explanation of correct answer, confidence self-rating, related knowledge node link.

5. **Question Progression:** Next question appears with transition animation. Adaptive difficulty adjusts based on performance.

6. **Quiz Completion:** Results screen shows: score, time, breakdown by topic, strengths, areas for improvement. Knowledge node update summary. Review incorrect answers option.

7. **Knowledge Update:** Performance data sent to memory system. Knowledge nodes strengthened or flagged for review. Spaced repetition schedule updated.

### 10.5 Animations

- Question Entry: Card slides in from right (300ms spring)
- Option Selection: Option pulses on tap, others dim (200ms)
- Correct Feedback: Green glow, checkmark draw (400ms), gentle confetti
- Incorrect Feedback: Red shimmer, correct answer highlight (500ms)
- Progress Update: Bar fills smoothly, milestone sparkle
- Completion: Score reveals with counting animation (1-2s)

### 10.6 Interactions

| Gesture | Action |
|---------|--------|
| Tap option | Select answer |
| Drag item | Drag-and-drop positioning |
| Double-tap blank | Focus fill-in field |
| Swipe left on card | Skip question (review mode) |
| Swipe right on card | Mark for later review |
| Tap feedback explanation | Expand full explanation |
| Tap confidence slider | Self-rate understanding |

### 10.7 Adaptive UI

- Difficulty scales based on performance: correct answers increase difficulty, incorrect decreases
- Question type varies based on content type
- Timer mode: None, relaxed (2x normal), standard (1x), challenge (0.5x)
- Hints available (progressively revealing): first hint subtle, second hint direct, third shows answer

### 10.8 AI Interaction

AI generates personalized quizzes based on knowledge gaps. Generates explanations tailored to learner's understanding level. Identifies patterns in mistakes (conceptual misunderstanding vs. recall failure). Creates follow-up questions targeting weak areas.

### 10.9 Memory Integration

- Performance data updates memory strength for tested knowledge nodes
- Spaced repetition rescheduled based on quiz results
- Incorrect answers trigger priority review scheduling
- Confidence ratings calibrate memory strength calculations

### 10.10 Reward System

- First quiz completed: Assessor badge
- 100% score: Perfect Score badge
- 50 quizzes: Quiz Master badge
- Streak of daily quizzes: Daily Scholar badge

### 10.11 Accessibility

Visual: All question types have text alternatives. High contrast for correct/incorrect indicators. Screen reader announces question and options. Font scaling.
Auditory: Questions read aloud. Answer options announced. Timer audio cues (optional).
Motor: Large tap targets. Keyboard navigation. Voice input for fill-in-blank. Configurable timer extends for motor difficulties.
Cognitive: Clear question formatting. Consistent layout across types. Unlimited time option. Review mode separates learning from assessment.

---

## 11. Mode 09: Flashcard Mode

### 11.1 Overview

Flashcard Mode enables rapid recall practice through digital flashcards with spaced repetition scheduling. Learners browse decks, swipe through cards, rate their recall, and track progress. Cards support rich media (images, audio, video) and smart scheduling based on the SM-2 algorithm.

### 11.2 Complete UX

**User Experience Philosophy:** Flashcards are the most efficient tool for building automatic recall. The interface must disappear entirely, leaving only the card and the learner's memory. The rhythm of swiping, rating, and progressing creates a meditative focus state. Every swipe is a micro-commitment to learning.

### 11.3 Complete UI

**Card Swipe Interface:**
- Card centered with front/back content
- Swipe right = known, swipe left = unknown, swipe up = partial
- Rating buttons (1-4) as alternative to swipe
- Progress indicator: cards remaining in session
- Deck info: name, cards studied today, streak

**Deck Manager:**
- Deck list with progress bars, next review counts
- Deck creation with card editor
- Import/export options
- Spaced repetition statistics per deck

### 11.4 Complete Flow

1. **Deck Selection:** Learner selects deck from library. Deck preview shows card count, due cards, mastery level, last studied.

2. **Session Start:** First card appears with entrance animation. Card shows one side (front by default).

3. **Recall Attempt:** Learner attempts to recall answer. Tap card or press Space to flip.

4. **Self-Rating:** After viewing answer, learner rates recall: 1 = forgot, 2 = difficult, 3 = good, 4 = easy. Rating determines next review interval.

5. **Card Progression:** Next card appears. Spaced repetition algorithm determines order. Session continues until all due cards reviewed or learner ends.

6. **Session End:** Stats summary: cards reviewed, accuracy, time spent, next review count. Knowledge update.

### 11.5 Animations

- Card Entry: Slides in from deck stack (300ms)
- Card Flip: 3D flip animation (400ms) with content crossfade
- Swipe Right (Known): Card flies off right with green trail (350ms)
- Swipe Left (Unknown): Card flies off left with red trail (350ms)
- Swipe Up (Partial): Card flies up with yellow trail (350ms)
- Stack View: Cards in deck shown as overlapping stack
- Session Complete: Cards spiral into completion icon (600ms)

### 11.6 Interactions

| Gesture | Action |
|---------|--------|
| Swipe right | Rate known (4) |
| Swipe left | Rate unknown (1) |
| Swipe up | Rate partial (2-3) |
| Tap card | Flip to show answer |
| Double-tap | Toggle mark for review |
| Long-press | Open card edit |
| Shake | Shuffle remaining cards |

### 11.7 Adaptive UI

- Session length configurable: 10, 20, 30, 50, or all due cards
- Card reveal direction: front first, back first, random
- Auto-advance: After rating, next card appears automatically (configurable delay)
- Image zoom: Pinch to zoom card images
- Night mode: Dark background for late-night study

### 11.8 AI Interaction

AI generates cards from learning content automatically. Creates example sentences and mnemonics. Identifies cards needing reinforcement based on performance patterns. Suggests card groupings for better learning.

### 11.9 Memory Integration

- SM-2 algorithm for spaced repetition scheduling
- Interval calculation based on rating and card history
- Performance tracked per card, per deck, overall
- Ease factor adjustment per card
- Daily review limit configurable

### 11.10 Reward System

- First 100 cards reviewed: Flashcard Novice badge
- 1000 cards: Flashcard Adept badge
- 10000 cards: Memory Master badge
- 30-day study streak: Consistent Learner badge
- 100% accuracy on a deck: Deck Master badge

### 11.11 Accessibility

Visual: Front/back text scaling. High contrast mode. Card content read aloud. Color not only indicator for correct/incorrect.
Auditory: Cards can include audio prompts. Text-to-speech for card content. Swipe sounds with directional audio.
Motor: Tap to flip (large target). Button alternatives for swipe gestures. Keyboard: Space flip, 1-4 rate. Voice control: Flip, Known, Unknown.
Cognitive: Simple interface with single focus. Clear rating labels. Progress visibility. No time pressure.

---

## 12. Mode 10: Memory Engine Mode

### 12.1 Overview

Memory Engine Mode is the spaced repetition review system visualized through a garden metaphor. Learners enter their memory garden daily, inspect memory plants representing knowledge nodes, and strengthen them through review. Plants grow from seedlings to blooming to fruiting as memory strengthens.

### 12.2 Complete UX

**User Experience Philosophy:** Memory is not a database; it is a living garden. Some memories are fragile seedlings needing daily care. Others are robust trees that need occasional pruning. The garden metaphor makes abstract spaced repetition tangible, emotional, and motivating. Watching your knowledge garden grow is intrinsically satisfying.

### 12.3 Complete UI

**Memory Garden View:**
- Lush garden landscape with plants at various growth stages
- Plant types by knowledge domain (flower = arts, tree = sciences, etc.)
- Growth stages: Seedling, Sprouting, Growing, Blooming, Fruiting, Ancient
- Garden customization options (pathways, decorations, water features)
- Daily review notification: plants needing water highlighted

**Plant Detail Modal:**
- Plant visual at current growth stage
- Knowledge node title and domain
- Memory strength indicator (rings/luminescence)
- Next review due date and interval
- Review history graph
- Water/Strengthen button to begin review

### 12.4 Complete Flow

1. **Daily Entry:** Learner enters Memory Garden. Camera pans across garden showing all plants. Plants needing review gently pulse with a water droplet icon.

2. **Garden Inspection:** Learner can tap any plant to inspect. Plant detail shows knowledge node info, memory strength, review history, next due date.

3. **Review Trigger:** Learner taps Water/Strengthen on a plant or the daily review prompt. Review session begins for due items.

4. **Review Session:** Flashcard-style review of the plant's knowledge node. After each review, memory strength recalculates.

5. **Growth Update:** Successful review triggers growth animation. Plant may advance to next stage if threshold reached.

6. **New Knowledge Planting:** When new knowledge nodes are created (from other modes), new seedlings appear in the garden. Learner can arrange their garden layout.

7. **Ecosystem Growth:** As garden grows, new features unlock: decorative items, garden expansions, special plants, companion creatures (spirit animals).

8. **Harvest:** When plants reach Fruiting/Ancient stage, learner can Harvest to gain special rewards or unlock advanced content related to that knowledge domain.

### 12.5 Animations

- Garden Entry: Camera descends from sky to garden level (1.5s)
- Plant Growth: Smooth size increase, color intensification, flower/sparkle emergence (1s)
- Watering: Water drops fall on plant, plant brightens and grows slightly (800ms)
- Harvest: Plant glows, fruit/sparkles collect into inventory (1.2s)
- New Seedling: Seed drops from sky, plants itself, sprouts (1s)
- Garden Customization: Items place with snap animation (300ms)
- Plant Needs Watering: Gentle pulse with droplet icon (continuous, 3s cycle)
- Season Change: Background transitions through seasons (2s)

### 12.6 Interactions

| Gesture | Action |
|---------|--------|
| Tap plant | Inspect detail |
| Long-press plant | Quick water (start review) |
| Drag plant | Rearrange in garden |
| Pinch garden | Zoom in/out |
| Swipe garden | Pan camera |
| Tap water droplet | Start daily review |
| Double-tap empty space | Open garden shop |
| Shake | Refresh garden (recalculate states) |

### 12.7 Adaptive UI

- Garden complexity adjusts to number of knowledge nodes
- New users start with small garden plot, expand over time
- Plant density management: automatic grouping for large gardens
- Performance mode: Simplified garden for lower-end devices
- Time of day: Garden lighting matches real-world time

### 12.8 AI Interaction

AI provides garden health summary: Which domains are thriving, which need attention, new seedlings to expect. Suggests review order for optimal memory reinforcement. Comments on garden ecosystem health. Can recommend planting new knowledge in weak areas.

### 12.9 Memory Integration

- Each plant = knowledge node in memory system
- Growth stage = memory strength level
- Watering = spaced repetition review
- SM-2 algorithm determines watering frequency
- Harvested plants have strongest memory retention
- Garden ecosystem health = overall memory system health

### 12.10 Reward System

- First seedling planted: Gardener badge
- First bloom: Bloom Watcher badge
- First harvest: Harvester badge
- 10 plants at Ancient stage: Master Gardener badge
- 30-day garden maintenance: Daily Gardener badge
- All knowledge domains represented: Ecosystem badge

### 12.11 Accessibility

Visual: Plant differentiation by shape, not just color. Growth stages have distinct silhouettes. High contrast garden mode. Screen reader announces plant states.
Auditory: Garden ambient sounds. Plant interaction sounds. Growth achievement sound. Water droplet audio.
Motor: Tap targets adjusted for plant size. Garden navigation with keyboard arrows. Voice control: Inspect [plant], Water garden, Harvest.
Cognitive: Clear visual indicators of plant needs. Simple interaction model. Progressive garden complexity. Consistent garden layout.

---

## 13. Mode 11: Knowledge Tree Mode

### 13.1 Overview

Knowledge Tree Mode visualizes the learner's knowledge structure as a growing tree. Learners explore branches representing domains, nodes representing concepts, and connections representing relationships. The tree grows and changes as knowledge is acquired, with mastery visualization, prerequisite chains, and knowledge gap highlighting.

### 13.2 Complete UX

**User Experience Philosophy:** Knowledge is not a list of facts; it is a living structure that grows, branches, and connects. The Knowledge Tree makes this structure visible, giving learners a sense of their intellectual journey. Seeing the tree grow over time provides motivation and a clear map of what has been mastered and what remains to explore.

### 13.3 Complete UI

**Tree View Interface:**
- Root at bottom, branches spreading upward (inverted tree)
- Major knowledge domains as main branches
- Sub-domains as secondary branches
- Individual concepts as leaf/bud nodes
- Node size indicates mastery level
- Node color indicates knowledge domain
- Glowing nodes indicate recently strengthened
- Translucent nodes indicate knowledge gaps

**Node Detail Panel:**
- Concept title and domain
- Mastery percentage
- Memory strength
- Prerequisite chain (required knowledge)
- Related concepts and connections
- Learning resources linked to this node
- Related modes showing where this was learned

### 13.4 Complete Flow

1. **Tree Entry:** Learner enters Knowledge Tree. Tree renders with growth animation. Branches spread from root to full extent of learner's knowledge.

2. **Branch Exploration:** Learner can zoom into specific branches (domains). Pinch to zoom, drag to pan. Branches highlight on hover.

3. **Node Inspection:** Tap any node to view detail. Node detail shows mastery, strength, prerequisites, related knowledge, learning resources.

4. **Gap Identification:** Translucent nodes show knowledge gaps. Tapping a gap shows how to learn it: prerequisite chain, available learning modes, estimated time to mastery.

5. **Prerequisite Navigation:** Node detail shows prerequisite chain. Tap any prerequisite to navigate to it. System can recommend learning path from current knowledge to target.

6. **Growth Visualization:** Tree animated growth on each visit. New nodes appear with sprout animation. Strengthened nodes glow brighter. Achievement milestones trigger branch flowers.

7. **Learning Path Creation:** Learner can select a target node and generate a learning path. Path shows sequence of prerequisite nodes with recommended learning modes.

8. **Mode Integration:** From any node, learner can jump to related content in other modes: Story, Documentary, Podcast, Quiz, Flashcard.

### 13.5 Animations

- Tree Entry: Branches grow from root with spreading animation (2s)
- Node Create: New node sprouts with green shimmer and unfurling leaf (800ms)
- Node Strengthen: Node pulses, grows slightly, glow intensifies (600ms)
- Branch Growth: New branch extends with segments appearing sequentially (1s)
- Gap Pulse: Translucent nodes gently pulse to indicate discoverability (4s cycle)
- Zoom Focus: Tree smoothly repositions to center selected branch (500ms)
- Learning Path: Path draws connecting nodes with animated line (1.5s)

### 13.6 Interactions

| Gesture | Action |
|---------|--------|
| Pinch | Zoom tree in/out |
| Drag | Pan tree view |
| Tap node | Open node detail |
| Long-press node | Quick mark for learning |
| Double-tap branch | Focus branch (zoom to fit) |
| Swipe up on detail | Expand full detail |
| Tap prerequisite | Navigate to prerequisite |
| Tap learning path | Generate learning plan |

### 13.7 Adaptive UI

- Tree complexity scales with knowledge size
- Initial view shows top-level branches, zoom reveals detail
- Focus mode highlights selected domain, dims others
- Comparison mode shows two trees side-by-side
- Timeline slider shows tree at different points in learner's journey

### 13.8 AI Interaction

AI provides tree health assessment: balanced growth, neglected domains, strong foundations. Recommends next learning targets based on gaps and learner's goals. Generates personalized learning paths. Identifies prerequisite chains. Predicts mastery timelines.

### 13.9 Memory Integration

- Tree structure mirrors knowledge node hierarchy
- Mastery percentage derived from memory strength of constituent nodes
- Prerequisite relationships determine learning sequence
- Tree growth tracked over time
- Gap identification uses memory system data

### 13.10 Reward System

- First branch completion: Branch Master badge
- All nodes in domain mastered: Domain Expert badge
- Tree reaches 100 nodes: Grove Keeper badge
- 500 nodes: Forest Walker badge
- Balanced tree (all domains growing): Harmony badge

### 13.11 Accessibility

Visual: Node shapes and patterns supplement color. High contrast tree mode. Screen reader announces node structure. Text labels always visible.
Auditory: Branch structure narrated. Node selection announced. Tree growth soundscapes.
Motor: Keyboard tree navigation. Tab through visible nodes. Zoom controls accessible. Voice: Focus on [domain], Show [node], Zoom in.
Cognitive: Hierarchical organization makes structure clear. Progressive disclosure prevents overwhelm. Learning paths reduce navigation decisions.

---

## 14. Mode 12: Knowledge Graph Mode

### 14.1 Overview

Knowledge Graph Mode provides connected knowledge exploration through a force-directed graph visualization. Learners explore nodes (concepts, people, events, locations, works), follow connections, discover relationships, and save exploration paths. The graph uses physics simulation for natural layout and supports filtering by category and relationship types.

### 14.2 Complete UX

**User Experience Philosophy:** Knowledge is a web, not a tree. Everything connects to everything else, often in unexpected ways. The Knowledge Graph reveals these connections, enabling learners to discover relationships they never knew existed. Exploration is open-ended and serendipitous.

### 14.3 Complete UI

**Graph View Interface:**
- Force-directed graph with physics simulation
- Node types: Concept (circle), Person (portrait), Event (diamond), Location (pin), Work (book)
- Node sizes by significance/relevance
- Relationship types: Influenced, Caused, Related to, Part of, Example of, Opposes
- Color coding by domain
- Filter panel for node types and relationship types
- Search bar with autocomplete
- Legend for node/relationship types

**Node Detail Panel:**
- Node type icon and title
- Description/summary
- Connected nodes grouped by relationship type
- Related learning resources
- Save to collection button
- Add to learning path button

### 14.4 Complete Flow

1. **Graph Entry:** Learner enters Knowledge Graph. Graph renders with physics simulation settling into stable layout. Initial view shows high-significance nodes.

2. **Graph Exploration:** Learner drags nodes, pans view, pinches to zoom. Physics simulation responds naturally. Nodes repel slightly, connections attract.

3. **Node Focus:** Tap any node to highlight it and its immediate connections. Other nodes dim. Detail panel shows node information.

4. **Connection Following:** Tap a connection or connected node to navigate. Graph smoothly re-centers on new focus. Path history tracked for back navigation.

5. **Search and Filter:** Search for specific nodes. Filter by type, domain, relationship. Filtered graph re-layouts with physics animation.

6. **Path Discovery:** Follow chains of connections to discover unexpected relationships. Path breadcrumbs shown. Save interesting paths.

7. **Deep Dive:** From node detail, open in related mode (character profile, timeline, documentary, etc.)

8. **Save and Collection:** Save nodes and paths to collections. Collections create sub-graphs.

### 14.5 Animations

- Initial Render: Nodes fly in from random positions, physics settles (2-3s)
- Node Selection: Selected node scales up 20%, connections brighten, others dim (400ms)
- Pinch Zoom: Smooth camera zoom, nodes scale with distance
- Drag Node: Node follows cursor with spring physics, connected nodes stretch (continuous)
- Filter Update: Non-matching nodes fade out with shrink, matching rearrange (600ms)
- Path Highlight: Path nodes pulse sequentially (1s per node)
- New Connection: Line draws between nodes with extend animation (500ms)

### 14.6 Interactions

| Gesture | Action |
|---------|--------|
| Drag node | Reposition in graph |
| Tap node | Select and show detail |
| Double-tap node | Focus and center |
| Drag background | Pan graph view |
| Pinch | Zoom in/out |
| Long-press node | Quick save |
| Swipe up on detail | Expand detail panel |
| Tap connection | Show relationship type |
| Two-finger tap | Open filter panel |

### 14.7 Adaptive UI

- Graph density adapts to screen size
- Initial view shows top-N nodes by significance
- Performance mode reduces particle effects for large graphs
- Layout styles: Force-directed, Hierarchical, Radial, Geographic
- 3D mode available for complex graphs (desktop/tablet)

### 14.8 AI Interaction

AI guides exploration: Did you know X is connected to Y through Z? Suggests interesting paths. Identifies clusters and hubs. Generates exploration narratives. Answers natural language queries about graph structure.

### 14.9 Memory Integration

- Node exploration recorded for interest profiling
- Connection discovery creates knowledge graph edges
- Saved paths stored as learning sequences
- Node detail viewed adds exposure to memory system

### 14.10 Reward System

- First node explored: Explorer badge
- 100 connections followed: Pathfinder badge
- 10 domains explored: Polymath badge
- 1000 nodes visited: Graph Walker badge
- Discovered rare connection: Serendipity badge

### 14.11 Accessibility

Visual: Node shapes distinguish types beyond color. Connection lines have patterns. High contrast graph mode. Screen reader announces node and connection structure.
Auditory: Graph structure narrated. Node selection announced. Connection types spoken. Exploration sounds optional.
Motor: Keyboard graph navigation (arrows move focus). Tab through visible nodes. Filter reduces visual clutter. Voice: Select [node], Show connections, Zoom in.
Cognitive: Filtering reduces complexity. Search targets specific nodes. Node labels always visible. Legend available. Saved paths for return.

---

## 15. Mode 13: AI Teacher Mode

### 15.1 Overview

AI Teacher Mode provides one-on-one AI tutoring sessions. The AI assesses the learner's knowledge, delivers personalized lessons with interactive teaching, asks and answers questions, provides practice opportunities, and gives detailed feedback. Features include whiteboard integration, Socratic method, adaptive pacing, and misconception detection.

### 15.2 Complete UX

**User Experience Philosophy:** Every learner needs a personal tutor who understands exactly what they know, how they learn best, and where they struggle. AI Teacher Mode strives to approximate the ideal tutorial experience: patient, knowledgeable, adaptive, and encouraging. The AI teacher never gets tired, never judges, and always has time for one more question.

### 15.3 Complete UI

**Tutoring Session Interface:**
- AI Teacher avatar: Scholarly figure in teaching setting
- Main content area: Lesson content with rich formatting
- Whiteboard integration: Shared canvas for explanations
- Chat panel: Conversation history with teacher
- Practice questions panel: Side panel for exercises
- Progress indicators: Lesson progress, mastery tracking

**Whiteboard Interface:**
- Drawing canvas with pen, highlighter, shapes, text tools
- AI can draw diagrams, write equations, illustrate concepts
- Learner can draw, annotate, ask AI to explain drawings
- Save whiteboard as note

### 15.4 Complete Flow

1. **Session Start:** Learner selects topic or enters free-form request. AI assesses current knowledge through brief pre-quiz or conversation.

2. **Personalized Lesson:** AI generates lesson plan adapted to learner's level. Lesson objectives displayed. Estimated duration shown.

3. **Interactive Teaching:** AI presents content with natural conversation. Lesson alternates between explanation, examples, questions, and practice.

4. **Whiteboard Use:** AI uses whiteboard for visual explanations. Learner can request whiteboard demonstrations.

5. **Question and Answer:** Learner asks questions at any point. AI answers with depth appropriate to context.

6. **Practice Integration:** After concept introduction, practice questions appear. AI provides immediate feedback and explanation.

7. **Misconception Detection:** AI identifies when learner holds incorrect understanding. Gently corrects with evidence and explanation.

8. **Socratic Method:** AI asks probing questions that lead learner to discover answers themselves. Encourages critical thinking.

9. **Session End:** Summary of what was learned. Knowledge node update. Recommended follow-up practice. Next session suggestion.

### 15.5 Animations

- Teacher Avatar: Attentive idle, teaching gestures, writing motion
- Content Transition: Lesson sections slide in with smooth transitions (400ms)
- Whiteboard Drawing: Draws content with stroke animation
- Practice Question: Card slides in from side (300ms)
- Correct Answer: Green glow, checkmark, teacher nods approvingly (500ms)
- Misconception Correction: Gentle redirection animation, not negative (400ms)
- Session Complete: Knowledge growth visualization (800ms)

### 15.6 Interactions

| Gesture | Action |
|---------|--------|
| Type message | Ask question or respond |
| Tap raise hand | Interrupt with question |
| Tap whiteboard | Draw/annotate |
| Pinch whiteboard | Zoom canvas |
| Tap practice question | Answer |
| Swipe lesson content | Navigate sections |
| Long-press content | Save as note |

### 15.7 Adaptive UI

- Pacing adjusts to learner's response time and accuracy
- Content depth adapts to demonstrated understanding
- Visual/auditory/kinesthetic teaching style adapts to learner preference
- Session length adapts to attention signals
- Language complexity adjusts to learner's level

### 15.8 AI Interaction

AI Teacher uses large language model with teaching-specific prompting. Maintains teaching persona consistently. Detects confusion from learner responses. Adjusts teaching strategy in real-time. Provides multiple explanation approaches for difficult concepts. Uses analogies based on learner's known domains.

### 15.9 Memory Integration

- Lesson topics create knowledge nodes
- Practice performance updates memory strength
- Misconception corrections create reinforcement entries
- Session content enters spaced repetition queue
- Whiteboard notes saved as knowledge artifacts

### 15.10 Avatar Integration

AI Teacher avatar is a scholarly figure appropriate to the subject (philosopher for philosophy, scientist for science). Avatar uses teaching-appropriate gestures (pointing, writing, illustrating). Expression conveys patience and encouragement. Avatar can demonstrate experiments or concepts visually.

### 15.11 Reward System

- First session: Student badge
- 10 sessions: Dedicated Learner badge
- 50 sessions: Scholar Apprentice badge
- Mastery of a topic: Topic Master badge
- Asked 100 questions: Curious Mind badge

### 15.12 Accessibility

Visual: Whiteboard content screen-readable. High contrast teaching materials. Text scaling for lesson content. Teacher avatar optional (text-only mode).
Auditory: Lesson content read aloud. Whiteboard actions described. Voice interaction for questions.
Motor: Voice input for questions. Keyboard navigation. Whiteboard accessible via keyboard/drawing tools. Configurable response time.
Cognitive: Lesson broken into digestible segments. Clear learning objectives. Progress visibility. Pause/resume anytime. Review previous content easily.

---

## 16. Mode 14: AI Mentor Mode

### 16.1 Overview

AI Mentor Mode serves as a long-term learning companion. The AI Mentor checks in daily, reviews goals, tracks progress, provides recommendations, helps set daily plans, and offers motivational support. The relationship builds over time with increasing personalization.

### 16.2 Complete UX

**User Experience Philosophy:** Learning is a journey, not a destination. The AI Mentor is a companion on that journey — someone who knows where you've been, where you're going, and how to help you stay on track. The relationship deepens over time as trust builds and the mentor learns the learner's patterns, preferences, and aspirations.

### 16.3 Complete UI

**Mentor Dashboard:**
- AI Mentor avatar: Warm, approachable, professional
- Daily greeting personalized to learner's state
- Today's learning goal with progress
- Streak and consistency metrics
- Upcoming reviews and deadlines
- Recommended learning activities

**Goal Tracking Interface:**
- Long-term goals with milestone markers
- Weekly/monthly progress charts
- Goal adjustment with mentor consultation
- Achievement timeline

### 16.4 Complete Flow

1. **Daily Check-In:** Learner opens AI Mentor. Mentor greets with personalized message referencing recent activity. Asks about readiness and motivation.

2. **Review Session:** Mentor reviews previous day's learning: what was learned, what was reviewed, knowledge nodes strengthened.

3. **Goal Progress Update:** Mentor shows progress toward learning goals. Celebrates milestones. Offers encouragement for challenges.

4. **Daily Plan Creation:** Mentor suggests optimal daily learning activities based on learner's goals, memory review schedule, and available time.

5. **Recommendations:** Mentor recommends specific content across modes: story chapters relevant to goals, documentary matching interests, quiz for review, etc.

6. **Motivational Support:** Mentor provides personalized encouragement based on learner's history. References past achievements during difficult periods.

7. **Habit Reinforcement:** Mentor reinforces positive habits (streaks, daily practice). Gentle nudges for consistency.

8. **Farewell:** Mentor ends session with encouragement and preview of next session. Option to set a reminder for next check-in.

### 16.5 Animations

- Mentor Greeting: Avatar appears with warm smile, gentle wave (500ms)
- Progress Update: Charts animate with counting fill (1-2s)
- Goal Celebration: Confetti, achievement sparkle, avatar claps (800ms)
- Streak Visual: Chain links connect forming a streak line (500ms)
- Daily Plan: Cards stack into schedule with staggered entry (600ms)
- Farewell: Avatar nods warmly, fades slightly (500ms)

### 16.6 Interactions

| Gesture | Action |
|---------|--------|
| Type message | Chat with mentor |
| Tap goal | View goal details |
| Tap recommendation | Open learning content |
| Swipe streak | View history |
| Long-press plan item | Reschedule |
| Tap celebrate | View achievements |

### 16.7 Adaptive UI

- Mentor personality adapts to learner preference: encouraging, direct, humorous, scholarly
- Check-in frequency adapts to learner engagement
- Goal complexity adjusts to learner's progress pace
- Recommendation density adjusts to available time indicated by learner
- Communication style adapts to learner's language level

### 16.8 AI Interaction

AI Mentor maintains long-term memory of all learner interactions. Builds a comprehensive learner model over time. Recognizes patterns in learning behavior. Provides emotionally intelligent responses based on learner state. Adapts motivational strategies to what works for each learner.

### 16.9 Memory Integration

- Daily check-ins create learning habit nodes
- Goal progress tracked as achievement nodes
- Mentor recommendations logged as learning path data
- Streak data stored in gamification system
- Learner model continuously updated

### 16.10 Avatar Integration

AI Mentor has a consistent visual identity. Warm, professional appearance. Expression reflects emotional tone of conversation. Wears different attire for different contexts (scholarly for learning, casual for check-ins). Ages or changes subtly over long-term relationship.

### 16.11 Reward System

- First check-in: Mentee badge
- 7-day streak: Weekly Warrior badge
- 30-day streak: Monthly Dedicate badge
- 365-day streak: Year of Growth badge
- Goal achieved: Goal Crusher badge
- Followed 50 recommendations: Open Mind badge

### 16.12 Accessibility

Visual: Mentor avatar optional. Text-only interaction available. High contrast mode. Screen reader friendly.
Auditory: Mentor speech with voice. Voice input for check-in responses. Audio encouragement.
Motor: Voice interaction for daily check-in. Simple tap interface. Keyboard navigation. Configurable response time.
Cognitive: Consistent daily routine. Clear progress visualization. Encouraging without pressure. Break goals into manageable chunks.

---

## 17. Mode 15: AI Chat Mode

### 17.1 Overview

AI Chat Mode provides open-ended knowledge conversation with the platform's AI. Learners can ask any question, explore any topic, and receive informed responses with source citations. The chat maintains context awareness, suggests follow-up topics, and integrates with the knowledge graph.

### 17.2 Complete UX

**User Experience Philosophy:** The AI Chat is the learner's intellectual companion. It answers questions, explores ideas, debates concepts, and helps synthesize understanding. It is a conversation partner that respects the learner's intelligence while providing accurate, sourced information.

### 17.3 Complete UI

**Chat Interface:**
- Conversation messages with distinct user/AI styling
- Source citations in AI responses (numbered references)
- Confidence indicators for AI claims
- Follow-up suggestion chips
- Knowledge graph connection indicators
- Context panel showing topics discussed

### 17.4 Complete Flow

1. **Chat Start:** Learner opens AI Chat. Welcome message with suggested starter questions based on recent learning activity.

2. **Question Input:** Learner types question or uses voice input. AI processes in real-time with thinking indicator.

3. **Response Generation:** AI generates response with source citations. Confidence level shown for each key claim.

4. **Follow-up Suggestions:** AI suggests 3-4 follow-up questions based on response content and learner's knowledge tree.

5. **Context Management:** AI maintains conversation context within session. Can reference previous turns.

6. **Knowledge Integration:** Key concepts discussed are linked to knowledge graph. Learner can save insights.

7. **Deep Dive:** Learner can ask for deeper explanation, sources, alternative perspectives, or related topics.

8. **Session End:** Summary of topics discussed. Knowledge nodes updated. Suggested next learning activities.

### 17.5 Animations

- AI Response: Streaming text appearance with word-by-word reveal
- Source Citation: Numbers appear as response streams (200ms each)
- Thinking Indicator: Animated dots with subtle AI avatar pulse
- Confidence Indicator: Bar fills according to confidence level (300ms)
- Follow-up Chips: Slide up after response complete (staggered, 300ms)
- Knowledge Link: Node icon pulses when concept linked to graph (400ms)

### 17.6 Interactions

| Gesture | Action |
|--------|--------|
| Type message | Send question |
| Tap microphone | Voice input |
| Tap source number | View source detail |
| Tap follow-up chip | Send follow-up question |
| Long-press message | Save as insight |
| Swipe left on response | View related content |
| Tap knowledge icon | Open knowledge graph for concept |

### 17.7 Adaptive UI

- Response length adapts to learner preference: concise, standard, detailed
- Language complexity adapts to learner level
- Source visibility: Always shown, on request only, summarized
- Context panel adaptable size

### 17.8 AI Interaction

AI uses full knowledge base with source attribution. Employs retrieval augmented generation for accurate responses. Maintains coherent conversation state. Detects when it doesn't know something and says so. Can express confidence levels. Can provide multiple perspectives on debated topics.

### 17.9 Memory Integration

- Questions asked recorded for interest profiling
- Concepts discussed tracked for knowledge graph
- Saved insights create knowledge artifacts
- Chat content analyzed for knowledge gap detection

### 17.10 Avatar Integration

AI Chat avatar appears as a friendly scholarly assistant. Subtle expressions during conversation. Engaged listening posture. Can be minimized to audio-only.

### 17.11 Reward System

- First question asked: Curious badge
- 100 questions: Inquisitor badge
- 500 questions: Knowledge Seeker badge
- 50 insights saved: Insight Collector badge
- Explored 20 topics: Polymath Chat badge

### 17.12 Accessibility

Visual: Chat text scalable. High contrast mode. Screen reader optimized. Source citations accessible.
Auditory: Chat read aloud. Voice input supported. Audio responses.
Motor: Voice input primary alternative. Keyboard navigation. Large tap targets. Switch control.
Cognitive: Clear question/answer separation. Source transparency builds trust. Follow-up suggestions reduce formulation load. Context maintained for coherence.

---

## 18. Mode 16: Memory Garden Mode

### 18.1 Overview

Memory Garden Mode is a gamified visual memory management experience. Learners enter a garden where each memory plant represents a knowledge domain. Plants grow and thrive based on spaced repetition adherence. The ecosystem metaphor makes memory maintenance intuitive, engaging, and rewarding.

### 18.2 Complete UX

**User Experience Philosophy:** Your memory is a living ecosystem. Some memories are delicate flowers that need daily attention. Others are mighty oaks that stand strong with occasional care. The Memory Garden makes the abstract process of memory maintenance tangible, beautiful, and intrinsically motivating.

### 18.3 Complete UI

**Garden Overview:**
- Beautiful garden landscape divided into domain plots
- Plant types specific to knowledge domains
- Growth stages: Seedling, Sprouting, Growing, Blooming, Fruiting, Ancient
- Garden customization options
- Weather and time-of-day effects
- Companion creatures that appear based on garden health

**Plant Detail:**
- Plant visual at current stage
- Knowledge domain represented
- Memory strength (glow intensity)
- Last review, next review due
- Growth history graph
- Review/care action button

### 18.4 Complete Flow

1. **Garden Entry:** Learner enters Memory Garden. Camera shows overview of entire garden. Plants at various stages. Weather/time matches real world.

2. **Garden Inspection:** Learner can explore garden freely. Plants needing attention show subtle indicators (drooping, color fade, water droplet).

3. **Plant Care (Review):** Tapping a needy plant starts review session for that knowledge domain. Successful review waters the plant and triggers growth.

4. **New Plant Addition:** When new knowledge domain is started, a seed appears in a new plot. Learner can choose where to plant it.

5. **Ecosystem Growth:** As more plants are cared for, garden becomes more vibrant. New features unlock: decorative items, water features, pathways, companion creatures.

6. **Harvesting:** Fully grown plants (Ancient stage) can be harvested for special rewards. Harvested plants remain but with reduced maintenance needs.

7. **Garden Customization:** Learner can arrange plants, add decorations, choose garden themes, unlock new landscapes.

8. **Seasonal Events:** Real-world seasons affect garden appearance. Special events (holidays, learner milestones) trigger garden transformations.

### 18.5 Animations

- Garden Entry: Camera flies in from above, settles (1.5s)
- Plant Growth: Stem extends, leaves unfurl, flowers bloom (smooth progression, 1-2s)
- Watering: Animated water droplets fall, plant brightens, grows (1s)
- Wilting: Plant slowly droops, color desaturates (continuous over days)
- Recovery: Wilted plant perks up, color returns (800ms)
- Harvest: Plant glows, particles rise, reward icon appears (1.2s)
- New Seed: Seed icon drops, plants in ground, first sprout emerges (1s)
- Weather Effects: Rain, sunlight, wind animations (continuous, gentle)
- Companion Creatures: Butterflies, birds, fireflies animate through garden

### 18.6 Interactions

| Gesture | Action |
|---------|--------|
| Tap plant | Inspect and care |
| Long-press plant | Quick review session |
| Drag plant | Rearrange in plot |
| Swipe garden | Pan camera |
| Pinch | Zoom in/out |
| Tap decorations | Browse customization shop |
| Double-tap empty plot | Plant new knowledge seed |
| Tap companion creature | View garden health summary |

### 18.7 Adaptive UI

- Garden scale adjusts to number of domains (expandable plots)
- Plant density management for large gardens
- Simplified garden for performance on lower-end devices
- Garden theme matches learner's preferred aesthetic
- Companion creatures increase with garden health

### 18.8 AI Interaction

AI Garden Keeper provides garden health report: Thriving domains, neglected areas, new growth expected. Offers personalized care recommendations. Celebrates garden milestones. Narrates garden changes. Can suggest optimal care schedule.

### 18.9 Memory Integration

- Each plant = knowledge domain memory strength
- Watering = spaced repetition review of domain content
- Growth stage = aggregate memory strength for domain
- Wilting = overdue reviews in domain
- Harvest = domain mastery achieved
- Garden health = overall memory system status

### 18.10 Avatar Integration

Avatar appears as a Garden Keeper or Spirit of the Garden. Wears nature-themed attire. Tends to plants alongside learner. Offers gardening tips and domain knowledge. Reacts happily to garden growth, concerned to neglected areas. Companion creatures react to avatar.

### 18.11 Reward System

- First seed planted: Seed Planter badge
- First bloom: Bloom Watcher badge
- First harvest: Harvest Master badge
- All domains at Blooming or higher: Vibrant Garden badge
- 30-day garden care: Dedicated Gardener badge
- Unlocked all garden themes: Landscape Artist badge
- Companion creature collection complete: Menagerie badge

### 18.12 Accessibility

Visual: Plant shapes differentiate types beyond color. Growth stages have distinct silhouettes. High contrast garden mode. Screen reader describes garden state.
Auditory: Garden ambient sounds (birds, water, wind). Plant interaction sounds. Growth achievement chimes. Weather audio.
Motor: Simplified garden interaction for motor difficulties. Auto-care option. Keyboard garden navigation. Voice: Water [plant], Harvest, Open shop.
Cognitive: Clear visual indicators of plant needs. Simple care routine. Progressive complexity. Encouraging feedback. No penalty for neglected plants (gentle recovery).

---

## 19. Cross-Mode Integration

### 19.1 Unified Navigation Architecture

All sixteen learning modes share a consistent navigation architecture that enables seamless transition between modes:

```
Mode Selector Grid → Selected Mode Hub → Mode-Specific Experience → Exit to Mode Selector or Cross-Mode Link
```

Every mode has a consistent top bar with: Back to mode selector, Mode title, Knowledge quick-access button, Profile/avatar, Overflow menu.

### 19.2 Cross-Mode Data Sharing

| Data | Shared Across Modes | Purpose |
|------|-------------------|---------|
| Knowledge Nodes | All modes | Unified knowledge representation |
| Memory Strength | Quiz, Flashcard, Memory Engine, Memory Garden | Consistent spaced repetition |
| Learner Progress | All modes | Unified progress tracking |
| Learning Goals | AI Mentor, Knowledge Tree, Memory Garden | Goal-aligned recommendations |
| Saved Items | All modes with save functionality | Unified collection management |

### 19.3 Mode Transition Patterns

| From | To | Transition | Trigger |
|------|----|-----------|---------|
| Story Mode | Knowledge Tree | Knowledge Map button | Learner taps to see node impact |
| Story Mode | Quiz Mode | Comprehension check failure | Recommended review |
| Documentary | Timeline | Context panel timeline link | Learner taps related event |
| Documentary | Map Mode | Location in documentary | Learner taps location link |
| Dialogue Mode | Character Mode | Character profile button | Learner views full profile |
| Timeline | Map Mode | Event location | Learner taps location on event |
| Map Mode | Timeline | Location events | Learner views location timeline |
| Quiz Mode | Flashcard Mode | Missed concepts | Recommended recall practice |
| Flashcard Mode | Memory Engine | Review completion | Auto-sync of review data |
| Any Mode | AI Chat | Ask AI button | Learner asks contextual question |
| AI Mentor | Any Mode | Recommendation tap | Learner follows suggestion |
| Knowledge Graph | Any Mode | Resource link | Learner opens related content |

### 19.4 Cross-Mode AI Context

AI maintains context as learner moves between modes. If learner asks about a concept in AI Chat, then moves to Story Mode, the AI narrator can reference the previous conversation. AI Mentor coordinates across all modes for unified recommendations.

### 19.5 Unified Search Across Modes

Search returns results from all modes: story chapters, documentaries, podcast episodes, characters, timeline events, map locations, quizzes, flashcards, knowledge nodes. Results organized by mode with previews.

---

## 20. Global Interaction Patterns

### 20.1 Gesture Consistency

All modes follow consistent gesture mapping:
- Swipe left: Navigate forward/next
- Swipe right: Navigate back/previous
- Swipe up: Reveal more content/expand
- Swipe down: Dismiss/collapse
- Tap: Select/activate
- Long-press: Context menu/quick action
- Double-tap: Focus/zoom
- Pinch: Zoom in/out
- Two-finger tap: Alternate action (mode-specific)

### 20.2 Haptic Feedback Standards

| Interaction | Haptic Pattern | Duration |
|-------------|---------------|----------|
| Selection | Sharp click | 10ms |
| Success | Double pulse | 100ms |
| Error | Buzz | 50ms |
| Transition | Soft thud | 30ms |
| Achievement | Rising cascade | 500ms |
| Warning | Repeated pulse | 200ms |

### 20.3 Loading States

- Initial load: Branded animation (mode icon transforms, 1-2s)
- Content load: Skeleton screens matching content layout
- Action processing: Inline spinner on action button
- Network retry: Gentle bounce of failed content area
- Offline indicator: Persistent banner with connection status

---

## 21. Performance & Technical Specifications

### 21.1 Performance Targets

| Metric | Target | Critical |
|--------|--------|----------|
| Time to Interactive | < 2s | < 3s |
| First Contentful Paint | < 1s | < 1.5s |
| Animation Frame Rate | 60fps | 30fps minimum |
| Mode Transition | < 800ms | < 1.2s |
| AI Response Time | < 2s | < 4s |
| Memory Operations | < 100ms | < 300ms |

### 21.2 Memory Management

- Knowledge graph: Lazy-load nodes within viewport + 2 levels of connections
- Media content: Progressive loading with quality tiers
- Garden mode: Simplified rendering for large gardens (>100 plants)
- Timeline: Virtualized event rendering for long timelines
- Chat history: Paginated loading, session-based context limits

### 21.3 Offline Capabilities

- Downloaded documentaries and podcasts for offline playback
- Cached knowledge graphs and trees for offline browsing
- Quiz and flashcard modes fully functional offline
- Memory calculations performed locally, sync on reconnection
- AI chat: Cached responses, offline mode with local model fallback (optional)

### 21.4 Platform Support

| Platform | Support Level | Notes |
|----------|--------------|-------|
| iOS (15+) | Full | Native performance optimizations |
| Android (12+) | Full | Material Design adaptation |
| Web (Chrome, Safari, Firefox) | Full | Responsive design, PWA capable |
| Tablet | Full | Optimized for larger screens |
| Desktop | Full | Keyboard and mouse optimization |
| TV (tvOS, Android TV) | Limited | Documentary and podcast playback |

---

*End of Document — Version 1.0*
