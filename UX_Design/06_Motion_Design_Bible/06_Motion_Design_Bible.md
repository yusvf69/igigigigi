# Motion Design Bible

## Enterprise UX Animation Specification

| Metadata | |
|---|---|
| **Document ID** | UX-MOTION-001 |
| **Version** | 1.0 |
| **Status** | Final |
| **Classification** | Enterprise UX Specification |
| **Last Updated** | 2026-07-10 |

### Cross-Reference Links

| Document | Location |
|---|---|
| UX/UI Audit | [00_UX_UI_Audit.md](../00_UX_UI_Audit/00_UX_UI_Audit.md) |
| Design System | [02_Design_System.md](../02_Design_System/02_Design_System.md) |
| Component Library | [03_Component_Library.md](../03_Component_Library/03_Component_Library.md) |
| Learning Mode UX | [05_Learning_Mode_UX/](../05_Learning_Mode_UX/) |

---

## Table of Contents

1. [Motion Philosophy](#1-motion-philosophy)
2. [Animation Principles](#2-animation-principles)
3. [Duration Mapping](#3-duration-mapping)
4. [Easing System](#4-easing-system)
5. [Shared Element Transitions](#5-shared-element-transitions)
6. [Screen Transitions](#6-screen-transitions)
7. [Micro-Interactions Catalog](#7-micro-interactions-catalog)
8. [Gesture Animations](#8-gesture-animations)
9. [Learning Mode Animations](#9-learning-mode-animations)
10. [Character/Avatar Animations](#10-characteravatar-animations)
11. [Knowledge Tree Growth](#11-knowledge-tree-growth)
12. [Memory Pulse Animation](#12-memory-pulse-animation)
13. [AI Thinking Animation](#13-ai-thinking-animation)
14. [Loading Animations](#14-loading-animations)
15. [Achievement & Reward Animations](#15-achievement--reward-animations)
16. [Success & Failure Animations](#16-success--failure-animations)
17. [Daily Reward Animation](#17-daily-reward-animation)
18. [Scroll & Parallax](#18-scroll--parallax)
19. [Reduced Motion Profile](#19-reduced-motion-profile)
20. [Performance Budget](#20-performance-budget)

---

## 1. Motion Philosophy

### 1.1 Purpose-Driven Animation

Motion in the learning ecosystem is never decorative. Every animated element serves a distinct communicative function. Animation is a vector for meaning, not visual flair. The motion system operates under the core tenet that movement conveys intent, hierarchy, and state change with surgical precision.

**The Five Jobs of Animation:**

| Job | Description | Example |
|---|---|---|
| **Orient** | Answering "Where am I and where did I come from?" | Screen transitions, shared element morphing, breadcrumb animation |
| **Confirm** | Answering "Did my action register?" | Button press feedback, toggle response, form submission acknowledgment |
| **Guide** | Answering "What should I do next?" | Pulsing CTAs, sequential reveal, handoff animations between elements |
| **Celebrate** | Answering "Did I achieve something?" | Streak milestones, level-up sequences, achievement unlock cascades |
| **Comfort** | Answering "Is the system working?" | Loading states, skeleton screens, AI thinking indicators, progress persistence |

### 1.2 Motion Respects the User's Time

Every animation has a maximum permissible duration based on the user's cognitive context. Users in a learning environment are task-oriented; they are reading, answering, recalling, and progressing. Motion that delays task completion without adding informational value is eliminated.

**Time Budget Hierarchy:**

- **Task-critical feedback** (button press, toggle, swipe action): 50-200ms. Instantaneous enough that the user perceives no delay between action and reaction.
- **Context-establishing transitions** (screen change, modal presentation, navigation): 200-400ms. Fast enough to maintain flow, slow enough to establish spatial continuity.
- **Celebratory sequences** (achievement, level-up, streak milestones): 1000-3000ms, but always skippable. Never lock the user into watching an animation.
- **Loading states** (skeleton, progress, AI thinking): displayed for the actual duration of the operation. Never artificially extended. Minimum 300ms to avoid flash-of-content.

### 1.3 Motion Feels Natural (Physics-Based)

All motion that simulates physical interaction adheres to real-world physics principles. Digital elements that move like physical objects create instinctive understanding. Users should never feel surprised by how an element behaves.

**Physics Model Specifications:**

- **Spring:** Default tension 200, friction 20, mass 1. Used for: card tap lift, badge bounce, toggle thumb, drag-and-drop settle.
- **Damping:** Overdamped (friction > critical) for UI elements that should never overshoot. Underdamped (friction < critical) for playful elements with subtle bounce.
- **Inertia:** For scroll and drag operations. Deceleration rate: 0.015 per frame for finger-driven interactions. Momentum: preserved at release with exponential decay.
- **Gravity:** Not used directly in UI (avoid simulating weight in interface elements). Reserved only for particle systems in achievement celebrations.
- **Restitution (Bounce):** 0.3 for reward elements (achievement badges, XP popups). 0.0 for all UI controls (buttons, cards, lists).

### 1.4 Motion is Accessible

The system provides a complete reduced-motion alternative that respects the user's operating system accessibility settings and in-app preferences. Motion accessibility is not an afterthought; it is a first-class constraint.

**Accessibility Requirements:**

- All animations must respect `prefers-reduced-motion` OS-level media query.
- All animations must be controllable via an in-app setting (reduced, standard, enhanced).
- No animation may flash more than three times per second (WCAG 2.1 Success Criterion 2.3.1).
- No animation may occupy more than 25% of the viewport at any time unless user-initiated.
- All information conveyed through motion must also be conveyed through a static alternative (text, color change, icon change).
- Animation duration must not exceed 5 seconds for any non-skippable sequence.

### 1.5 Motion Hierarchy

Elements on screen exist in a motion hierarchy that mirrors the visual hierarchy. Primary content moves first, fastest, and most prominently. Secondary content follows. Decorative elements move last, if at all.

**Motion Priority Levels:**

| Level | Elements | Behavior |
|---|---|---|
| **P0 — Critical** | User target (tapped item), feedback elements | 0-100ms response, highest opacity, most pronounced movement |
| **P1 — Primary** | Content being revealed, incoming screen elements | 200-350ms, standard easing, moderate displacement |
| **P2 — Secondary** | Supporting UI, navigation controls, backgrounds | 300-500ms, delayed by stagger, subtle movement |
| **P3 — Tertiary** | Decorative, atmospheric, non-essential | Reduced motion profile, may be omitted entirely |

### 1.6 Motion Language

The system defines a consistent motion language that users learn once and apply everywhere. This language is built on three movement archetypes:

**Enter:** Elements arrive from a direction that makes spatial sense. Content enters from the direction of its origin. Detail views expand from the tapped item. Sheets slide up from the bottom because gravity and gesture both reinforce bottom-origin.

**Exit:** Elements depart toward their origin or in the direction of dismissal. Modal content scales down toward the dismiss point. List items slide out toward the swipe direction. Screens slide out in the direction opposite to their entry.

**Reorient:** Elements change state in place. Toggles switch, badges update, progress fills. These animations never involve translation; they occur entirely within the element's bounding box.

---

## 2. Animation Principles

### 2.1 The 12 Principles Applied to UI

Walt Disney's 12 principles of animation, adapted for interface design. Every principle has explicit UI applications with measurable parameters.

#### 2.1.1 Ease In / Ease Out (Slow In / Slow Out)

**Principle:** Objects in motion accelerate and decelerate naturally. Nothing moves at a constant velocity in the natural world.

**UI Application:** All UI motion MUST use non-linear easing. Linear easing is reserved exclusively for continuous, looping animations (spinners, pulses, shimmers).

**Specifications:**
- **Enter (appear):** Accelerate from rest. Use decelerate easing `cubic-bezier(0, 0, 0.2, 1)`. Element starts slow, reaches peak velocity at midpoint, decelerates to rest.
- **Exit (disappear):** Decelerate to rest. Use accelerate easing `cubic-bezier(0.4, 0, 1, 1)`. Element starts at full velocity, decelerates quickly to rest.
- **Move (translate):** Use standard easing `cubic-bezier(0.2, 0, 0, 1)`. Smooth acceleration and deceleration profile.
- **Scale in/out:** Same as enter/exit above.

#### 2.1.2 Anticipation

**Principle:** A small preparatory movement precedes the main action, directing the user's attention to where the action will occur.

**UI Application:** Elements briefly move or change state in the opposite direction before their primary animation, signaling what is about to happen.

**Specifications:**
- **Button press:** Scale up by 1.02 for 50ms, then scale down to 0.97 for 100ms. The brief scale-up anticipates the press.
- **Card expansion:** Brief contraction (scale 0.98 for 50ms) before expanding to full size (scale 1.0). Signals that expansion is imminent.
- **Swipe reveal:** Content shifts backward (2px translate for 50ms) before swiping away. Signals the swipe threshold approaching.
- **Modal dismiss:** Brief scale-up (1.02 for 50ms) before scaling down to 0.95 and fading out. Prepares user for dismissal.
- **Timing:** Anticipation phase MUST be 50-80ms. Never longer; the user should not wait for the anticipation to complete before the main action begins.

#### 2.1.3 Follow-Through

**Principle:** Elements continue moving slightly after the main action stops, simulating inertia and realistic physics.

**UI Application:** When a UI element completes its primary motion, it settles into its final position with a small overshoot or trailing effect.

**Specifications:**
- **Card lift on tap:** Card lifts 2px with shadow increase over 200ms, then settles back to rest with a 0.5px overshoot and spring decay over 100ms. Total: 300ms.
- **Drawer close:** Drawer slides closed over 300ms. Handle element has a trailing follow-through of 5px over 100ms after drawer reaches closed position.
- **Scroll bounce:** At scroll boundary, content overshoots by 10% of viewport and springs back over 250ms. Spring tension: 180, friction: 25.
- **Toggle thumb:** Thumb slides to new position over 200ms then settles with a 0.3mm overshoot oscillation decaying over 50ms.

#### 2.1.4 Overlapping Action

**Principle:** Different parts of an element or related elements move at different rates, creating a more natural and sophisticated motion.

**UI Application:** Related elements in a sequence do not move in perfect unison. Each element has a slightly different timing, creating a cascading effect.

**Specifications:**
- **Card list entrance:** Cards enter with staggered delay. First card: 0ms delay. Second: 50ms. Third: 100ms. Fourth: 150ms. Fifth+: 200ms. Each card animates for 350ms with standard ease. Total stagger window: 250ms max.
- **Modal presentation:** Backdrop fades in (200ms). Modal scales from 0.95 to 1.0 (300ms, delayed 50ms from backdrop). Content fades in (200ms, delayed 100ms from modal). Close button fades in (150ms, delayed 200ms from modal).
- **Page transition:** Current page content fades out (150ms, decelerate). New page background slides in (250ms, delayed 50ms). New page content fades in (200ms, delayed 100ms from background).
- **Section reveal on scroll:** Each section header animates (300ms). Content items stagger at 80ms intervals. Images within section animate at 400ms with 100ms stagger.
- **Stagger timing formula:** `delay = index * staggerInterval`. Where staggerInterval = totalDesiredDuration / count * 0.3. Each element animates over totalDesiredDuration * 0.7.

#### 2.1.5 Staging

**Principle:** The animation clearly presents the most important element to the user at the right moment.

**UI Application:** Motion directs the user's gaze to the most important UI element at each moment. The active element is the most animated; static elements recede.

**Specifications:**
- **Primary action emphasis:** The primary CTA on any screen has 1.5x the animation amplitude of secondary elements. If a card lifts 2px on tap, the primary CTA lifts 3px.
- **Sequential reveal:** Content reveals in priority order. Headline first, then supporting content, then actions, then decorative elements. Each stage offset by 100ms stagger.
- **Focus management:** When an element receives focus (keyboard or tap), it animates at full specification. All other elements reduce animation amplitude by 50%.
- **Error state staging:** Error icon appears first (200ms). Error message fades in second (300ms, 50ms delay). Correction action button appears third (200ms, 100ms delay).

#### 2.1.6 Arcs

**Principle:** Objects in nature move in curved paths, not straight lines.

**UI Application:** Translational motion follows a gentle arc rather than a perfectly straight line, creating more natural movement.

**Specifications:**
- **Notification slide-in:** Notification slides from top edge following a slight arc (3px y-axis offset at midpoint of travel). Duration: 350ms.
- **Drag-to-dismiss:** Card follows a parabolic arc when thrown off screen. Horizontal velocity determines trajectory. Vertical offset: `y = x^2 * 0.001` where x is horizontal distance from origin. Maximum vertical deviation: 40px.
- **Floating action button expansion:** Menu items radiate from FAB along a 90-degree arc. Each item at 15-degree increments. Arc radius: 60px. Duration: 300ms per item, 50ms stagger.
- **Drag reorder:** Items swap positions following a curved path (arc height: 10px). Duration: 200ms.
- **Particle systems:** Emitted particles follow procedural arcs with gravity (0.5px/frame^2 downward), initial velocity (50-150px/s at random angle +/- 30 degrees).

#### 2.1.7 Secondary Action

**Principle:** Minor actions that support the main action add richness without distracting.

**UI Application:** A primary animation may be accompanied by subtle secondary animations that enhance the experience without competing for attention.

**Specifications:**
- **Button press:** Primary: scale to 0.97 (100ms). Secondary: subtle shadow depth change (darker by 5%) (100ms). Tertiary: ripple effect from touch point (400ms, 50ms delay).
- **Card tap:** Primary: card lifts 2px + shadow increases (200ms). Secondary: subtle background color change on the tapped area (150ms). Tertiary: adjacent cards shift 0.5px away (200ms, 100ms delay).
- **Toggle switch:** Primary: thumb slides (200ms). Secondary: background color transitions (200ms). Tertiary: subtle track shadow change (150ms).
- **Checkbox check:** Primary: checkmark path draws (200ms). Secondary: box background fills (150ms). Tertiary: subtle scale bounce on completion (1.0 to 1.05 to 1.0 over 100ms).
- **Rules:** Secondary action amplitude MUST be 50% or less of primary action amplitude. Secondary action duration MUST be equal to or less than primary action duration.

#### 2.1.8 Timing

**Principle:** The number of frames or duration assigned to an action determines its perceived weight, importance, and emotional quality.

**UI Application:** Duration communicates hierarchy. Faster animations feel lighter, more urgent, more responsive. Slower animations feel weightier, more significant, more deliberate.

**Specifications:**
- **Micro (50-100ms):** Imperceptible as animation. Feels instantaneous. Used for: haptic feedback response, button downstate, ripple initiation.
- **Quick (150-200ms):** Perceived as responsive. Used for: hover effects, focus rings, chip selection, toggle states.
- **Standard (250-350ms):** Perceived as smooth and natural. Used for: card entrance, list item appearance, toast presentation, element transitions.
- **Deliberate (400-600ms):** Perceived as meaningful. Used for: screen transitions, modal presentation, sheet expansion, significant state changes.
- **Slow (700-1000ms):** Perceived as dramatic or celebratory. Used for: hero animations, achievement reveals, milestone celebrations.
- **Narrative (1000-3000ms):** Perceived as storytelling. Used for: knowledge tree growth sequences, loading experiences, AI processing visualization.
- **Continuous:** Looping animation. Used for: loading spinners, skeleton shimmers, pulsing indicators, ambient effects.
- **Timing adjustment:** On mobile devices, reduce all durations by 20% from the desktop specification to account for shorter attention spans and faster interaction patterns.

#### 2.1.9 Exaggeration

**Principle:** Amplifying motion beyond realistic physics to communicate emotion, importance, or playfulness.

**UI Application:** Subtle exaggeration makes UI feel alive and responsive without crossing into cartoonish territory.

**Specifications:**
- **Achievement celebration:** Exaggerated scale bounce: 0 to 1.3 to 0.9 to 1.05 to 1.0 over 600ms. Spring with tension 300, friction 12.
- **Badge notification:** Exaggerated spring: scale 0 to 1.4, settle to 1.0 over 400ms. Tension 400, friction 15.
- **XP number increment:** Number scales up to 1.3 and back to 1.0 over 300ms plus color brighten.
- **Error shake:** Element translates horizontally by 10px for 3 oscillations, decaying to 0. Duration: 300ms.
- **Correct answer celebration:** Card briefly scales to 1.05 and back (200ms). Subtle gold glow overlay appears and fades (400ms).
- **Exaggeration limits:** Maximum scale exaggeration: 1.5x (achievement). Maximum displacement exaggeration: 20px (error shake). Maximum rotation exaggeration: 15 degrees (swipe gesture).

#### 2.1.10 Squash and Stretch

**Principle:** Objects deform under force, compressing when hitting a surface and elongating when moving quickly.

**UI Application:** UI elements subtly deform on interaction to communicate physicality and responsiveness.

**Specifications:**
- **Button press:** Scale x: 0.97, scale y: 0.97 (uniform squash). No directional squash/stretch for buttons to avoid distortion of text.
- **Card tap:** Scale x: 0.98, scale y: 0.97 (subtle non-uniform squash on press). Release: overshoot to x: 1.01, y: 1.02 (slight stretch), settle to 1.0.
- **Toggle thumb:** Thumb compresses (scale x: 0.9, scale y: 1.1) at start of slide, stretches (scale x: 1.1, scale y: 0.9) at end, settles to 1.0.
- **Drag-and-drop:** Element being dragged compresses by 2% on pickup (squash), stretches by 2% when moving (stretch in direction of motion).
- **Swipe card:** Card stretches by 1-3% in direction of swipe based on velocity. At 100px/s: 1% stretch. At 500px/s: 3% stretch.
- **Squash/stretch limits:** Maximum deformation: 10% on any axis. Text elements: 0% deformation (text must never squash or stretch; apply deformation to container only).

#### 2.1.11 Solid Drawing

**Principle:** Forms must feel three-dimensional with appropriate weight, volume, and lighting to be believable.

**UI Application:** Elements use elevation, shadow, and lighting changes to communicate physical presence during motion.

**Specifications:**
- **Elevation system:** Every interactive element has a resting elevation and an interactive elevation. Resting: 1dp (subtle shadow). Hover/press: 4dp (lifted shadow). Active/dragging: 8dp (floating shadow).
- **Shadow properties on lift:**
  - Resting: offsetY 1px, blur 3px, spread 0px, opacity 0.2.
  - Hover: offsetY 2px, blur 6px, spread 1px, opacity 0.25.
  - Press: offsetY 1px, blur 3px, spread 0px, opacity 0.2 (matches resting, simulates pressing down).
  - Drag: offsetY 8px, blur 16px, spread 2px, opacity 0.3.
- **Shadow transition:** Shadow MUST animate with the element. When a card lifts over 200ms, the shadow transitions smoothly from resting to hover specification over the same 200ms.
- **Depth layers:**
  - Layer 0 (background): No shadow. No elevation change.
  - Layer 1 (surface): Resting shadow. 200ms transition to interactive state.
  - Layer 2 (card/content): Resting shadow. 200ms interactive transition. 300ms modal transition.
  - Layer 3 (modal/overlay): Elevated shadow. 300ms entrance. 200ms exit.
  - Layer 4 (toast/tooltip): Highest shadow. Fade-based entrance. No scale change.
- **Light source:** Consistent light source from top-left (45 degrees). Shadows fall to bottom-right.

#### 2.1.12 Appeal

**Principle:** Movement should be pleasing to watch. Characters and objects should be engaging, not mechanical or jarring.

**UI Application:** Animations feel crafted, not automated. Curves are smooth, timing is musical, and motion brings joy without being gratuitous.

**Specifications:**
- **Motion curves:** All curves defined in the easing system (Section 4). No default CSS ease or linear where avoidable.
- **Musical timing:** Stagger intervals follow musical subdivisions (multiples of 50ms, aligned to 16th notes at 120bpm: 125ms = 8th note, 250ms = quarter note, 500ms = half note, 1000ms = whole note).
- **Cohesion:** All animations within a single view share the same easing family. No mixing of decelerate and accelerate curves within one transition sequence.
- **Whitespace in motion:** Allow 50-100ms of "rest" between sequential animation phases.
- **Pleasant defaults:** Default spring tension 200, friction 20 produces a motion that feels "just right." Default duration 300ms is the "golden duration."
- **Anti-patterns:** No random animation durations for the same interaction. No overlapping entrances and exits in the same view layer.

### 2.2 Applied Motion: Element-Specific Principles

#### 2.2.1 Card Motion

Cards are the primary content container. Their motion communicates hierarchy, interactivity, and spatial relationships.

**Principles:**
- Cards exist in a Z-space stack. When interacted with, they lift (increase Z) to signal active state.
- Card entrance follows a linear entry from below or from the direction of navigation.
- Card exit reverses the entrance path. If a card entered by scaling up, it scales down to exit.
- Adjacent cards shift proportionally when a card expands or is removed (items collapse into vacated space over 300ms with standard ease).
- Card swipe communicates intention. Horizontal swipe signals dismissal or completion. Vertical swipe signals browsing or exploration.

**Parameters:**
- Resting elevation: 1dp
- Active elevation: 4dp (tap), 8dp (drag)
- Entrance duration: 350ms
- Exit duration: 250ms
- Elevation transition: 200ms
- Content stagger within card: 80ms

#### 2.2.2 Button Motion

Buttons are the primary action trigger. Their motion provides clear, immediate feedback.

**Principles:**
- Button press is the fastest micro-interaction. The user must feel the response within the same frame as the touch.
- The pressed state communicates "I have registered your input."
- The released state communicates "Your action is being processed."
- Ripple effect communicates the origin point of the interaction.
- Disabled state communicates "This action is not currently available."

**Parameters:**
- Press scale: 0.97
- Press duration: 100ms
- Release spring-back: 200ms (tension 250, friction 20)
- Ripple duration: 400ms
- Ripple origin: touch point
- Ripple max radius: button diagonal / 2
- Elevation change: rest 0dp, press-shadow decreases by 20%

#### 2.2.3 Transition Motion

Transitions connect separate screens and states. They establish spatial continuity.

**Principles:**
- The user must always understand where content came from and where it went.
- Content that the user tapped must be the hero of the transition (shared element).
- Exit before enter: the current state begins to transition out before the new state transitions in.
- Direction of transition should match the user's mental model of navigation (right = forward, left = back).

**Parameters:**
- Shared element: 350ms standard ease
- Push transition: 300ms new content slides in from right
- Pop transition: 300ms current content slides to right
- Fade transition: 200ms cross-fade
- Modal: 300ms scale-up + fade

#### 2.2.4 Character Motion

Characters (avatars, guides, teaching assistants) have organic, expressive motion.

**Principles:**
- Characters must feel alive, not mechanical. Even subtle idle motion (breathing, micro-gestures) is essential.
- Expression changes are gradual, not binary.
- Gestural motion (waving, pointing, nodding) follows anticipation -> action -> follow-through arcs.
- Lip sync motion maps to phoneme timing from the AI voice system.
- Character motion should be confined to the character's bounding area.

**Parameters:**
- Idle breathing cycle: 3000ms (gentle 1% scale oscillation)
- Blink interval: 4000-6000ms (randomized)
- Expression morph: 500ms
- Gesture duration: 600-1000ms
- Lip sync: real-time driven by audio amplitude, with phoneme groups mapped to 6 mouth shapes
- Reaction time: 200-400ms delay for natural feel

#### 2.2.5 Knowledge Element Motion

Knowledge elements (tree nodes, graph connections, memory cards) have organic, growth-based motion.

**Principles:**
- Growth animation simulates organic expansion, not binary appearance.
- Connections draw from source to target, establishing visual causality.
- Nodes scale from center outward, suggesting internal organization.
- Branching follows natural bifurcation patterns.
- Color transitions signal state changes (unlocked -> in progress -> mastered).

**Parameters:**
- Node scale: 0 to 1 over 500ms (decelerate)
- Branch extension: 800-1000ms (organic curve growth)
- Connection draw: 500ms (stroke-dashoffset animation)
- State color transition: 400ms (spring, tension 180, friction 22)
- Particle burst on new node: 800ms (20-40 particles, radial, gravity-affected)

---

## 3. Duration Mapping

### 3.1 Duration Tiers

The duration system defines six tiers of animation duration, each mapped to specific interaction types and emotional contexts.

| Tier | Range | Perceived As | Emotional Valence | Use Cases |
|---|---|---|---|---|
| **Micro** | 50-100ms | Instant | Neutral, efficient | Button press, ripple, toggle downstate, haptic-coupled feedback |
| **Quick** | 150-200ms | Responsive | Assuring, nimble | Hover, focus, chip selection, color transition, shadow transition |
| **Standard** | 250-350ms | Smooth | Confident, polished | Card enter/exit, list item, toast, element reveal, accordion |
| **Deliberate** | 400-600ms | Meaningful | Intentional, significant | Screen transition, modal present, sheet, share sheet, context menu |
| **Slow** | 700-1000ms | Dramatic | Celebratory, weighty | Hero animation, achievement unlock, milestone, level-up reveal |
| **Narrative** | 1000-3000ms | Story-driven | Immersive, emotional | Knowledge tree growth, loading experience, AI processing sequence |

### 3.2 Duration by Component

| Component | Micro | Quick | Standard | Deliberate | Slow | Narrative |
|---|---|---|---|---|---|---|
| Button press | 100ms | - | - | - | - | - |
| Button ripple | - | 200ms start | 400ms total | - | - | - |
| Toggle switch | 100ms (thumb) | 200ms (track) | 300ms (total) | - | - | - |
| Checkbox | - | 200ms | - | - | - | - |
| Radio button | - | 200ms | - | - | - | - |
| Card tap (lift) | - | 200ms | - | - | - | - |
| Card enter | - | - | 350ms | - | - | - |
| Card exit | - | - | 250ms | - | - | - |
| Card swipe | - | - | - | 250-400ms | - | - |
| List item | - | - | 300ms | - | - | - |
| Stagger (per item) | - | - | 50-80ms | - | - | - |
| Hover (desktop) | - | 150ms | - | - | - | - |
| Focus ring | - | 150ms | - | - | - | - |
| Chip select | - | 200ms | - | - | - | - |
| Search bar expand | - | - | 300ms | - | - | - |
| Tooltip fade | - | 200ms in | - | - | - | 3000ms stay, 200ms out |
| Badge update | - | - | 300ms | - | - | - |
| Toast | - | - | 350ms in | - | - | 3000ms stay, 250ms out |
| Notification | - | - | 350ms in | - | - | 4000ms stay, 250ms out |
| Modal open | - | - | - | 300ms | - | - |
| Modal close | - | - | 200ms | - | - | - |
| Bottom sheet | - | - | - | 400ms in | - | - |
| Bottom sheet close | - | - | - | 300ms | - | - |
| Screen push | - | - | - | 300ms | - | - |
| Screen pop | - | - | - | 300ms | - | - |
| Tab switch | - | - | 250ms | - | - | - |
| Fade transition | - | 200ms | - | - | - | - |
| Skeleton pulse | - | - | - | - | - | 1500ms cycle |
| Progress bar fill | - | - | 300ms min | - | - | - |
| Loading spinner | - | - | - | - | - | 800ms/rotation |
| Pull to refresh | - | - | 400ms (indicator) | - | - | - |
| Drag reorder | - | - | 200ms settle | - | - | - |
| Swipe delete snap | - | - | 250ms | - | - | - |
| Long press | - | - | - | 400ms | - | - |
| Star rating | - | 200ms each | - | - | - | stagger 50ms |
| Slider thumb | 0ms (follow touch) | - | - | - | - | - |
| Skeleton shimmer | - | - | - | - | - | 1500ms cycle |
| Achievement unlock | - | - | - | - | - | 1500ms total |
| XP gain | - | - | - | 1000ms | - | - |
| Level up | - | - | - | - | - | 2500ms total |
| Confetti | - | - | - | - | - | 2000ms |
| Correct answer | - | - | 300ms | 400ms | - | - |
| Wrong answer | - | - | 300ms | - | - | - |
| Card memory pulse | - | - | - | - | - | 2000ms cycle |
| Knowledge node grow | - | - | - | - | 600ms | - |
| Knowledge branch | - | - | - | - | 1000ms | - |
| Tree seasonal change | - | - | - | - | - | 3000ms |
| AI thinking dots | - | - | - | - | - | 300ms each |
| AI thinking brain | - | - | - | - | - | 2000ms cycle |
| Page turn | - | - | 250ms | 350ms (curl) | - | - |
| Quiz card flip | - | - | - | 400ms | - | - |
| Flashcard swipe | - | - | - | 400ms | - | - |
| Memory consolidation | - | - | - | - | - | 1200ms |
| Water/splash | - | - | 500ms | - | - | - |
| Forgetting | - | - | - | - | - | 3000ms |
| Daily reward | - | - | - | - | - | 2000ms total |
| Streak fire | - | - | 400ms ignite | 600ms grow | - | continuous |

### 3.3 Duration Adjustment Factors

**Device Factor:**
- Desktop: 1.0x (baseline duration)
- Tablet: 0.9x (slightly faster due to touch interaction model)
- Phone: 0.8x (faster for mobile attention context)

**Connection Speed Factor:**
- WiFi: 1.0x
- 5G: 0.9x
- 4G: 0.8x
- Offline: 0.7x (minimize perceived latency)
- Unknown: 0.9x (conservative approach)

**User Preference Factor:**
- Standard motion: 1.0x
- Reduced motion: 0.0-0.5x (see Section 19)
- Enhanced motion: 1.0x (no increase; enhanced motion adds additional effects, not longer durations)

**Accessibility Factor:**
- No accessibility setting: 1.0x
- prefers-reduced-motion (reduce): 0.25x
- prefers-reduced-motion (no-preference): 1.0x
- Vestibular disorder accommodation: 0.0x (no motion, use opacity-only transitions)

**Net effective duration formula:**
```
effectiveDuration = baseDuration * deviceFactor * connectionFactor * accessibilityFactor
```

### 3.4 Duration by Interaction Context

**Navigation Context:**
- User drilling deeper into content: 300ms (push transition)
- User returning to previous level: 300ms (pop transition)
- User switching tabs: 250ms (slide)
- User opening modal: 300ms (scale + fade)
- User closing modal: 200ms (scale + fade, reverse)
- User opening sheet: 400ms (slide up)
- User closing sheet: 300ms (slide down)

**Input Context:**
- User taps button: 100ms (scale feedback)
- User types in field: No animation (immediate text display)
- User selects option: 200ms (selected state transition)
- User toggles: 300ms (thumb + track transition)
- User submits form: 200ms (button loading state transition)
- User drags: 0ms (follows finger), 200ms (settle on release)

**Feedback Context:**
- Success confirmation: 300ms (checkmark + subtle bounce)
- Error notification: 300ms (gentle shake + appearance)
- Warning: 350ms (caution indicator) + 3000ms (persistence) + 250ms (dismiss)
- Progress update: Real-time + minimum 300ms total display time
- Achievement: 1500ms total sequence (burst + icon + text + background)

---

## 4. Easing System

### 4.1 Easing Curve Definitions

The easing system defines five primary curve families, with specific cubic-bezier values for each.

#### 4.1.1 Standard Ease (Default)

**Curve:** `cubic-bezier(0.2, 0, 0, 1)`
**Also known as:** Ease-in-out, smooth
**Character:** Gentle acceleration from rest, sustained peak velocity, gentle deceleration to stop.
**When to use:** Default for most UI animation. Use when an element appears, moves, or changes state without directional bias.
**Velocity profile:** Ramp up over first 20% of duration, maintain near-peak velocity for 60%, ramp down over final 20%.
**Components:** Card enter, list item, toast, element reveal, accordion open/close, panel expand/collapse.

#### 4.1.2 Decelerate Ease (Out)

**Curve:** `cubic-bezier(0, 0, 0.2, 1)`
**Also known as:** Ease-out, entrance, fade-in
**Character:** Starts at peak velocity, gently decelerates to rest.
**When to use:** Elements that are entering the screen or appearing. The fast start gets the element on screen quickly, and the gradual settle allows the user to read it.
**Components:** Screen entrances, element appears, modal enters, tooltip fades in, notification slides in.

#### 4.1.3 Accelerate Ease (In)

**Curve:** `cubic-bezier(0.4, 0, 1, 1)`
**Also known as:** Ease-in, exit, fade-out
**Character:** Starts slow, accelerates to peak velocity at the end.
**When to use:** Elements that are exiting the screen or disappearing. The slow start gives the user a moment to register what is leaving, and the fast exit gets it off screen quickly.
**Components:** Screen exits, element disappears, modal exits, tooltip fades out, notification slides out.

#### 4.1.4 Linear

**Curve:** `linear` or `cubic-bezier(0, 0, 1, 1)`
**Character:** Constant velocity throughout.
**When to use:** Continuous, looping motion where variable velocity would be distracting.
**Components:** Loading spinner rotation, skeleton gradient sweep, pulsing indicator, progress bar (indeterminate sweep).

#### 4.1.5 Spring

**Physics-based curve (not CSS cubic-bezier). Uses spring physics parameters.**

| Parameter | Default | Range | Effect |
|---|---|---|---|
| Tension (stiffness) | 200 | 50-500 | Higher = faster, bouncier |
| Friction (damping) | 20 | 5-50 | Higher = less bounce, faster settle |
| Mass | 1 | 0.1-3 | Higher = heavier, slower |
| Velocity (initial) | 0 | any | Starting velocity for gesture-driven springs |

**Spring Profiles by Use Case:**

| Use Case | Tension | Friction | Mass | Overshoot | Settle Time |
|---|---|---|---|---|---|
| Button press release | 250 | 20 | 1 | 1.02x | 200ms |
| Card tap lift | 200 | 18 | 1 | 1.03x | 250ms |
| Card tap release | 300 | 25 | 1 | 1.01x | 150ms |
| Badge update | 400 | 15 | 0.5 | 1.15x | 350ms |
| Achievement reveal | 300 | 12 | 0.7 | 1.25x | 500ms |
| Toggle thumb | 180 | 22 | 1 | 1.01x | 200ms |
| Drag settle | 150 | 25 | 1.5 | 1.0x (no overshoot) | 300ms |
| Scroll bounce | 180 | 25 | 1 | 1.1x | 250ms |
| Notification badge | 350 | 16 | 0.8 | 1.2x | 400ms |
| Swipe snap | 200 | 30 | 1 | 1.0x (no overshoot) | 250ms |
| Star rating pop | 300 | 18 | 0.5 | 1.15x | 250ms |
| Particle emission | 100 | 15 | 0.3 | varies | 600ms |
| Node connection | 180 | 22 | 1 | 1.02x | 400ms |

### 4.2 Easing by Component

| Component | Enter Easing | Exit Easing | Move Easing | State Change Easing |
|---|---|---|---|---|
| Button | Spring (250/20/1) | Accelerate | - | Spring (250/20/1) |
| Card | Decelerate | Accelerate | Standard | Spring (200/18/1) |
| List item | Decelerate | Accelerate | - | Standard |
| Modal | Decelerate | Accelerate | - | - |
| Sheet | Decelerate (y) | Accelerate (y) | - | - |
| Toast | Decelerate | Accelerate | - | - |
| Tooltip | Decelerate (opacity) | Accelerate (opacity) | - | - |
| Notification | Decelerate | Accelerate | - | - |
| Screen push | Standard (new) | Standard (old) | - | - |
| Screen pop | Standard (new) | Standard (old) | - | - |
| Tab switch | Standard | Standard | - | - |
| Fade | Decelerate | Accelerate | - | - |
| Progress bar | Linear (indeterminate) | - | - | Decelerate (fill) |
| Skeleton | - | - | - | Linear (shimmer sweep) |
| Spinner | - | - | - | Linear (rotation) |
| Achievement | Spring (300/12/0.7) | Accelerate | - | Spring (350/15/0.5) |
| Knowledge node | Decelerate | Accelerate | - | Spring (180/22/1) |
| Knowledge branch | Decelerate (organic) | - | - | Standard |
| Connections | Decelerate (draw) | - | - | - |
| Character idle | - | - | - | Spring (100/30/1) |
| Character expression | - | - | - | Decelerate (morph) |
| AI thinking | - | - | - | Spring (150/20/0.5) |
| Swipe gesture | Spring (200/30/1) | - | Follows finger | Spring (200/30/1 snap) |

### 4.3 Custom Curves

Specialized curves for specific component patterns. These are optimized for their specific use case and should not be applied elsewhere.

| Curve ID | Value | Applied To |
|---|---|---|
| `ease-hero` | `cubic-bezier(0.15, 0.85, 0.35, 1)` | Hero image transitions, full-screen media entrance |
| `ease-bounce` | `cubic-bezier(0.34, 1.56, 0.64, 1)` | Badge updates, notification count changes |
| `ease-smooth` | `cubic-bezier(0.25, 0.1, 0.25, 1)` | Screen transitions, shared element morphing |
| `ease-slow` | `cubic-bezier(0.4, 0.15, 0.6, 1)` | Deliberate reveals, narrative sequences |
| `ease-spring-lite` | `cubic-bezier(0.34, 1.3, 0.64, 1)` | Subtle spring effect without physics engine |
| `ease-lift` | `cubic-bezier(0.18, 0.89, 0.32, 1.28)` | Card lift on tap, element pick-up |
| `ease-stagger-in` | `cubic-bezier(0.22, 1, 0.36, 1)` | Staggered list item entrance |
| `ease-stagger-out` | `cubic-bezier(0.55, 0, 0.86, 0.25)` | Staggered list item exit |
| `ease-shake` | `cubic-bezier(0.36, 0, 0.64, 1)` | Error shake oscillation |
| `ease-pulse` | `cubic-bezier(0.4, 0, 0.6, 0.8)` | Pulsing attention indicators |

### 4.4 Easing Anti-Patterns

**NEVER USE:**
- CSS `ease-in` (default) -- too slow at start, too abrupt at end.
- CSS `ease-out` (default) -- too abrupt at start, too slow at end.
- CSS `ease` (default) -- symmetrical acceleration/deceleration creates a mechanical feel.
- CSS `ease-in-out` (default) -- plateau at midpoint creates a "pause" sensation.
- `step()` functions -- only for state-based transitions.
- `cubic-bezier()` values outside the 0-1 range for x coordinates.
- Different easing for the same component type in different contexts.

**CURVE INTEGRITY RULES:**
- All cubic-bezier curves must have x1, x2 in the range [0, 1]. Y1, Y2 may exceed [0, 1] for spring-like effects but should not exceed [-1, 2] maximum.
- All curves must be monotonic in x (no backtracking in time).
- All curves must pass through (0,0) and (1,1).

---

## 5. Shared Element Transitions

### 5.1 Overview

Shared element transitions create visual continuity between two screens by animating a common element from its position on the source screen to its position on the destination screen. This is the most important technique for establishing spatial navigation in the application.

**Purpose:** Answer the question "What happened to the thing I just tapped?" -- by keeping the thing visible throughout the transition.

### 5.2 Transition Types

#### 5.2.1 Scale Transition

**Used when:** A thumbnail or preview expands to a full-size view.

| Parameter | Value |
|---|---|
| Duration | 350ms |
| Easing | Standard: cubic-bezier(0.2, 0, 0, 1) |
| Scale range | Source scale -> scale 1.0 |
| Position range | Source position -> destination position |
| Content clipping | Clip to destination bounds at transition midpoint |
| Shadow | Source shadow -> destination shadow over duration |
| Border radius | Source radius -> destination radius (smooth morph) |
| Background | Fade background over top 100ms of transition |

**Implementation sequence:**
1. Shared container rendered at the higher of the two Z-indices
2. Source element rendered within container at source scale/position
3. Container transitions to destination scale/position over 350ms
4. At midpoint (175ms), clip from source bounds to destination bounds
5. Opacity of source background fades out over first 100ms
6. Opacity of destination background fades in over last 100ms
7. Content reveal within destination: stagger at 80ms intervals

#### 5.2.2 Position Transition

**Used when:** A list item transitions to a detail view, maintaining the item's position relative to the viewport.

| Parameter | Value |
|---|---|
| Duration | 350ms |
| Easing | Standard: cubic-bezier(0.2, 0, 0, 1) |
| Path | Direct from source to destination. No arc for list items |
| Translation | x, y from source center to destination center |
| Scale | Same as scale transition |
| Items leaving | Fade out with 150ms, delay 50ms |
| Items returning | Fade in with 200ms, delay 100ms after element arrives |

#### 5.2.3 Morph Transition

**Used when:** An element changes shape between two states (e.g., a circle avatar to a rectangular profile header).

| Parameter | Value |
|---|---|
| Duration | 400ms |
| Easing | Standard: cubic-bezier(0.2, 0, 0, 1) |
| Border radius morph | Continuous interpolation |
| Clip-path morph | Continuous interpolation |
| Element content | Content scales/positions smoothly within morphing container |
| Background color | Interpolated through sRGB space |

#### 5.2.4 Combined Transition

**Used when:** Multiple elements transition together.

| Parameter | Value |
|---|---|
| Duration | 350ms for primary element, 400ms for secondary |
| Easing | Standard for all sub-elements |
| Primary element start | 0ms delay |
| Secondary element start | 50ms delay |
| Tertiary element start | 100ms delay |

### 5.3 Transition Sequence

**Phase 1: Exit (50-100ms)**
- Source content fades out or moves aside
- Background begins to transition to destination background
- Duration: 80ms (standard)
- Easing: Accelerate on source content fade

**Phase 2: Shared Element Animation (200-350ms)**
- Shared element animates from source position/scale to destination position/scale
- Duration: 350ms (standard)
- Easing: Standard cubic-bezier(0.2, 0, 0, 1)

**Phase 3: Enter (100-200ms)**
- Shared element arrives at destination, content within begins to fade in/reveal
- Duration: 150ms for primary content, 200ms stagger for secondary
- Easing: Decelerate for content reveals

### 5.4 Gesture-Driven Transitions

| Parameter | Value |
|---|---|
| Tracking mode | Direct: element position matches finger offset exactly |
| Scale interpolation | Interpolate between source and destination based on gesture progress |
| Progress mapping | `progress = distance / threshold`, clamped to [0, 1] |
| Threshold for completion | 40% of screen width (horizontal), 30% of screen height (vertical) |
| Snap behavior | Below threshold: snap back to source (200ms spring). Above threshold: snap to destination (300ms standard ease) |
| Rotation | Subtle rotation based on progress: max 5 degrees at 100% progress |

### 5.5 Hero Image Transitions

| Parameter | Value |
|---|---|
| Duration | 400ms |
| Easing | Decelerate: cubic-bezier(0, 0, 0.2, 1) |
| Scale progression | Thumbnail scale -> full viewport width |
| Content overlay | Fade in over last 150ms |
| Parallax during transition | Hero image has 1.05x scale overshoot at midpoint, settling to 1.0x |

### 5.6 Card-to-Detail Transition

**Complete Specification:**
1. User taps card (at card position in list)
2. **Anticipation (50ms):** Card scales to 0.98. Adjacent cards begin to fade
3. **Exit phase (80ms):** Adjacent cards fade out (accelerate ease). Background starts shifting
4. **Morph phase (350ms):** Card scales and translates from list position to detail position. Border radius morphs from 12px to 0px. Shadow transitions from card-level to no shadow (detail view)
5. **Content reveal (150ms stagger):** Primary content fades in (50ms). Secondary content staggers at 80ms intervals
6. **Completion:** Transition complete at ~450ms from tap

**Return Transition:**
1. User taps back or swipes
2. Content fades out (80ms)
3. Card morphs back to list position (300ms)
4. Adjacent cards fade in (150ms)
5. Return complete at ~380ms

### 5.7 Failure State

| Parameter | Value |
|---|---|
| Duration | 250ms |
| Easing | Decelerate (incoming), Accelerate (outgoing) |
| Source fade | 100ms, accelerate |
| Destination fade | 200ms, decelerate (50ms delay from source fade start) |

---

## 6. Screen Transitions

### 6.1 Push Transition (Drill-Down)

**Used when:** User navigates deeper into the content hierarchy (e.g., topic -> subtopic -> lesson).

| Parameter | Value |
|---|---|
| Slides in from | Right edge of viewport |
| Slides out to | Left (behind the incoming screen) |
| Duration | 300ms |
| New screen easing | Standard: cubic-bezier(0.2, 0, 0, 1) |
| Old screen easing | Accelerate: cubic-bezier(0.4, 0, 1, 1) |
| Overlap | Incoming screen covers outgoing at midpoint. No gap |
| Shadow on incoming | Left edge shadow: offset -4px, blur 12px, opacity 0.15 |
| Navigation bar | Title cross-fades: 150ms fade out current, 150ms fade in new (50ms overlap) |
| Back button | Appears at 150ms (slide in from left, 100ms) |
| Content entrances | Content fades in after screen arrives: 50ms delay, 200ms duration, decelerate |

### 6.2 Pop Transition (Go Back)

**Used when:** User navigates backward in the content hierarchy.

| Parameter | Value |
|---|---|
| Slides in from | Left (behind the outgoing screen), middle of transition |
| Slides out to | Right edge |
| Duration | 300ms |
| New (previous) screen easing | Decelerate: cubic-bezier(0, 0, 0.2, 1) |
| Old (current) screen easing | Standard: cubic-bezier(0.2, 0, 0, 1) |
| Shadow on outgoing | Right edge shadow: offset 4px, blur 12px, opacity 0.15 |
| Navigation bar | Back button slides out (80ms). Previous title slides in (150ms) |

### 6.3 Modal Presentation

**Parameters (Open):**

| Parameter | Value |
|---|---|
| Backdrop | Fade in from 0 to 0.6 opacity |
| Backdrop easing | Decelerate |
| Backdrop duration | 200ms (50ms before modal starts) |
| Modal scale | 0.95 -> 1.0 |
| Modal opacity | 0 -> 1 |
| Modal duration | 300ms |
| Modal easing | Decelerate: cubic-bezier(0, 0, 0.2, 1) |
| Modal start delay | 50ms (after backdrop starts) |
| Content within modal | Fade in with 100ms delay after modal opens (150ms, decelerate) |
| Close button | Appears at 200ms (50ms, fade in) |

**Parameters (Close):**

| Parameter | Value |
|---|---|
| Backdrop | Fade out from 0.6 to 0 |
| Backdrop duration | 150ms |
| Modal scale | 1.0 -> 0.95 |
| Modal opacity | 1 -> 0 |
| Modal duration | 200ms |
| Modal easing | Accelerate: cubic-bezier(0.4, 0, 1, 1) |

### 6.4 Bottom Sheet Presentation

**Parameters (Open):**

| Parameter | Value |
|---|---|
| Backdrop | Fade in from 0 to 0.4 opacity |
| Backdrop easing | Decelerate |
| Backdrop duration | 200ms (50ms before sheet starts) |
| Sheet translation | TranslateY: 100% -> 0% |
| Sheet opacity | 0.8 -> 1.0 |
| Sheet duration | 400ms |
| Sheet easing | Decelerate: cubic-bezier(0, 0, 0.2, 1) |
| Corner radius | 16px top corners |
| Shadow on sheet | Top edge shadow: offset 0, -4px, blur 20px, opacity 0.2 |
| Content stagger | 80ms per item, 200ms per item, decelerate |
| Grab handle | 4px tall, 36px wide, rounded. Fades in at 100ms |

**Parameters (Close):**

| Parameter | Value |
|---|---|
| Backdrop | Fade out from 0.4 to 0 |
| Backdrop duration | 200ms |
| Sheet translation | TranslateY: 0% -> 100% |
| Sheet duration | 300ms |
| Sheet easing | Accelerate: cubic-bezier(0.4, 0, 1, 1) |

**Gesture-Driven Close:**
- Gesture: Swipe down on sheet content
- Sheet follows finger with no easing (0ms response)
- Opacity decreases by 0.3 per 100px of downward drag
- Scale decreases by 0.02 per 100px of downward drag
- Threshold for dismiss: 30% of sheet height
- Below threshold: snap back with spring (tension 200, friction 25, mass 1, 250ms)
- Above threshold: dismiss (200ms, accelerate)

### 6.5 Tab Switch Transition

| Parameter | Value |
|---|---|
| Direction | Content slides horizontally |
| Duration | 250ms |
| Outgoing content easing | Accelerate: cubic-bezier(0.4, 0, 1, 1) |
| Incoming content easing | Decelerate: cubic-bezier(0, 0, 0.2, 1) |
| Tab indicator | Spring-based: tension 300, friction 25, mass 1. Duration: ~300ms |
| Tab icon | Selected tab icon: subtle scale 1.1 for 100ms, then settle to 1.0 |
| Tab label | 100ms fade out old, 100ms fade in new |

**Tab Switch Direction Mapping:**

| Tab Order | Direction | Slide (outgoing) | Slide (incoming) |
|---|---|---|---|
| Left -> Right | Rightward | Slides left | Slides in from right |
| Right -> Left | Leftward | Slides right | Slides in from left |

### 6.6 Fade Transition

| Parameter | Value |
|---|---|
| Outgoing content | Fade out from 1.0 to 0 |
| Outgoing duration | 100ms |
| Outgoing easing | Accelerate: cubic-bezier(0.4, 0, 1, 1) |
| Incoming content | Fade in from 0 to 1.0 |
| Incoming duration | 200ms |
| Incoming delay | 50ms after outgoing starts |
| Incoming easing | Decelerate: cubic-bezier(0, 0, 0.2, 1) |
| Total transition | 250ms |

### 6.7 Browser Back Gesture (iOS/Android)

| Parameter | Value |
|---|---|
| Tracking mode | Current screen follows finger position exactly |
| Progress mapping | `progress = fingerX / screenWidth` |
| Previous screen | Revealed behind current screen at progress > 0 |
| Previous screen scale | 0.9 at 0% progress -> 1.0 at 100% progress |
| Shadow on current screen | Left edge shadow: offset -8px, blur 24px, opacity 0.3 at max |
| Threshold for completion | 40% of screen width |
| Snap-back (below threshold) | Spring: tension 250, friction 30, mass 1. 250ms |
| Snap-forward (above threshold) | Standard ease: 200ms |

### 6.8 Transition Anti-Patterns

- Never combine push and fade transitions in the same navigation stack
- Never use different transition types for the same navigation direction
- Never exceed 400ms for standard screen transitions
- Never animate the navigation bar and content with different timing signatures
- Never leave the user in a state where both screens are partially visible without clear affordance
- Never interrupt a transition in progress unless the user initiates a back gesture

---

## 7. Micro-Interactions Catalog

### 7.1 Button Press

| Parameter | Value |
|---|---|
| Trigger | Touch down / click |
| Response type | Scale + shadow change |
| Press scale | 0.97 (uniform) |
| Press duration | 100ms |
| Press easing | Accelerate: cubic-bezier(0.4, 0, 1, 1) |
| Release scale | 1.0 |
| Release duration | 200ms |
| Release easing | Spring: tension 250, friction 20, mass 1 |
| Shadow change | Rest: offsetY 1px, blur 3px, opacity 0.2. Press: offsetY 0.5px, blur 2px, opacity 0.15 |
| Shadow transition | Linear, 100ms |
| Ripple effect | Radial expansion from touch point |
| Ripple duration | 400ms (expand in 300ms, fade in last 100ms) |
| Ripple easing | Decelerate (expand), Linear (fade) |
| Ripple max radius | Button diagonal / 2 |
| Ripple start delay | 50ms after press |
| Ripple color | Current color with 20% opacity |
| Long press | At 400ms: haptic feedback + visual state change |
| Disabled state | No scale change on press. No ripple. Opacity 0.4 |
| Loading state | Button scales to disabled state (100ms). Spinner appears (fade in, 150ms). Text fades out (100ms) |
| Success state | Spinner transitions to checkmark (200ms draw). Button returns to rest (100ms) |

### 7.2 Card Tap

| Parameter | Value |
|---|---|
| Trigger | Touch down / click on card surface |
| Response type | Lift (translation + shadow) |
| Press lift | TranslateY: -2px |
| Press duration | 200ms |
| Press easing | Spring: tension 200, friction 18, mass 1 |
| Press shadow | Rest: offsetY 2px, blur 6px, opacity 0.2. Active: offsetY 6px, blur 12px, opacity 0.3 |
| Release | TranslateY: 0px |
| Release duration | 150ms |
| Release easing | Spring: tension 300, friction 25, mass 1 |
| Adjacent card response | During tap: 0.5px movement away. 200ms, standard ease, 50ms delay |
| Multi-select | Long press (400ms) enters selection mode. Toggle selection with 200ms scale bounce (1.0 -> 1.05 -> 1.0) |

### 7.3 Toggle Switch

| Parameter | Value |
|---|---|
| Trigger | Tap on track or drag thumb |
| Track width | 44px (standard), 36px (compact) |
| Track height | 24px (standard), 20px (compact) |
| Thumb size | 20px (standard), 16px (compact) |
| Thumb translation (on) | TranslateX: trackWidth - thumbSize - 4px |
| Thumb translation (off) | TranslateX: 4px |
| Track color (off) | Gray-300 (#D1D5DB) |
| Track color (on) | Brand primary |
| Thumb color | White (#FFFFFF) |
| Duration (thumb) | 200ms slide |
| Duration (track color) | 200ms |
| Easing (thumb slide) | Spring: tension 180, friction 22, mass 1 |
| Easing (track color) | Decelerate: cubic-bezier(0, 0, 0.2, 1) |
| Thumb overshoot | 1.02x at midpoint, settle to 1.0x |
| Thumb press | Scale to 1.15 during drag. Spring back to 1.0 on release (200ms, tension 250, friction 18) |
| Haptic | Light impact on toggle |
| Disabled state | Track opacity 0.5. No animation |

### 7.4 Checkbox

| Parameter | Value |
|---|---|
| Trigger | Tap on checkbox area |
| Box size | 20px x 20px (standard), 16px x 16px (compact) |
| Border radius | 4px |
| Border (unchecked) | 2px solid Gray-400 |
| Background (checked) | Brand primary |
| Checkmark color | White |
| Checkmark path | SVG path |
| Duration | 200ms |
| Easing (checkmark draw) | Decelerate: cubic-bezier(0, 0, 0.2, 1) |
| Background fill timing | Box fills from center outward (200ms) |
| Checkmark draw timing | Path draws from start to end (200ms, stroke-dashoffset) |
| Scale bounce on completion | 1.0 -> 1.1 -> 1.0 (100ms, spring tension 300, friction 20) |
| Press feedback | Brief scale 0.95 (50ms) before check animation |
| Disabled state | No animation. Opacity 0.5 |

### 7.5 Radio Button

| Parameter | Value |
|---|---|
| Trigger | Tap on radio area |
| Outer circle | 20px diameter, 2px stroke |
| Inner circle (selected) | 10px diameter, brand primary fill |
| Inner circle animation | Scale from 0 to 1.0 |
| Duration (inner circle) | 200ms |
| Easing (inner circle) | Spring: tension 250, friction 20, mass 0.5 |
| Group response | Previously selected deselects: inner circle scales to 0 (150ms, accelerate). New selects: inner circle scales to 1.0 (200ms) |
| Disabled state | No animation. Opacity 0.5 |

### 7.6 Search Bar Expand

| Parameter | Value |
|---|---|
| Trigger | Tap on search bar / search icon |
| Width change | Icon/compact width -> full screen width minus margins |
| Duration (expand) | 300ms |
| Easing (expand) | Decelerate: cubic-bezier(0, 0, 0.2, 1) |
| Duration (collapse) | 250ms |
| Easing (collapse) | Accelerate: cubic-bezier(0.4, 0, 1, 1) |
| Placeholder text | Fades out as user types (opacity 1 -> 0 over 150ms on first character) |
| Clear button | Appears when text entered: fade + scale (150ms, spring tension 200, friction 20) |
| Cancel button (iOS) | Slides in from right (200ms, decelerate) |
| Shadow | Shadow increases on expand: offsetY 2px, blur 8px, opacity 0.15 (200ms) |
| Results dropdown | Slides down from search bar: 300ms, decelerate. Items stagger in at 50ms each |

### 7.7 Chip Select

| Parameter | Value |
|---|---|
| Trigger | Tap on chip |
| Duration | 200ms |
| Easing (scale) | Spring: tension 250, friction 20, mass 0.5 |
| Scale animation | 1.0 -> 1.05 -> 1.0 |
| Background fill | Fills from chip center outward (200ms) |
| Border transition | Solid -> transparent: 150ms |
| Color transition | Text color: gray -> white (150ms). Background: transparent -> brand (200ms) |
| Dismiss (filter chip) | X icon fades in (100ms). Chip shrinks: scale 1.0 -> 0.8 -> 0 (200ms, accelerate) |
| Group selection | Previous chip deselects (150ms) as new chip selects (200ms with 50ms overlap) |

### 7.8 Star Rating

| Parameter | Value |
|---|---|
| Trigger | Tap on star icon |
| Stars | 5 stars in horizontal row |
| Timing (per star) | 200ms |
| Stagger interval | 50ms between stars |
| Easing | Spring: tension 300, friction 18, mass 0.5 |
| Star scale on fill | 1.0 -> 1.3 -> 1.0 (bounce) |
| Star color transition | Gray -> gold (or brand accent). Interpolated: 150ms, decelerate |
| Haptic | Light tap per star |
| Total animation time (5 stars) | 5 x 200ms + 4 x 50ms stagger = 1200ms |

### 7.9 Slider

| Parameter | Value |
|---|---|
| Trigger | Touch drag on slider track or thumb |
| Track height | 4px (interactive area: 44px minimum touch target) |
| Thumb size | 24px diameter |
| Thumb response | Follows finger with 0ms latency |
| Track fill | Fills from start to thumb position in real-time |
| Track fill color | Gray-200 (unfilled) -> Brand primary (filled) |
| Thumb shadow | Rest: offsetY 1px, blur 2px, opacity 0.25. Active: offsetY 2px, blur 6px, opacity 0.3 |
| Thumb scale on press | 1.0 -> 1.25 (150ms, spring tension 200, friction 20) |
| Thumb scale on release | 1.25 -> 1.0 (200ms, spring tension 250, friction 25) |
| Value label | Appears above thumb on press: fade + scale (150ms). Disappears on release (100ms fade) |
| Step snapping | Thumb snaps to nearest step within 100ms of release |
| Haptic | Light impact per step (if step slider) |
| Disabled state | Opacity 0.4. No animation |

### 7.10 Pull to Refresh

| Parameter | Value |
|---|---|
| Trigger | Touch drag down at top of scrollable content |
| Threshold | 60px (indicator fully revealed) |
| Pull phase | Content scrolls past top boundary. Rubber-band effect: offset = distance * 0.5 |
| Indicator reveal | Fades in + rotates based on pull distance: progress = min(pullDistance / threshold, 1.0) |
| Indicator rotation | 0 degrees at 0 pull -> 360 degrees at threshold pull |
| Release threshold | If pullDistance < threshold: snap back (250ms). No refresh |
| Release above threshold | Snap to threshold position (100ms spring). Refresh begins |
| Refresh phase | Spinner rotates at 800ms/cycle. Duration: actual network request time. Minimum display: 400ms |
| Complete | Spinner fades out (200ms). Content refreshes. List items stagger in (50ms stagger, 200ms each) |
| Total maximum | 3000ms (if network allows). Progress timeout at 10000ms shows error state |
| Error state | Spinner transitions to error icon (200ms morph). "Tap to retry" fades in (300ms) |

### 7.11 Swipe to Delete

| Parameter | Value |
|---|---|
| Trigger | Horizontal swipe on list item |
| Direction | Left swipe (reveals delete action) |
| Tracking | Item translateX follows finger delta with 1:1 mapping |
| Resistance | After action button is fully revealed: follows at 0.3x finger delta |
| Action button width | 80px (delete) |
| Action button reveal | Button slides in from right as item moves left |
| Action button color | Red (#EF4444) |
| Threshold for snap | 50% of action button width |
| Snap to reveal (above threshold) | 250ms, spring tension 200, friction 25 |
| Snap back (below threshold) | 250ms, spring tension 200, friction 25 |
| Delete trigger | Tap on delete button: item collapses (scale 1.0 -> 0.8 -> 0, 250ms, accelerate). Adjacent items slide up to fill gap (300ms, standard ease, 50ms stagger) |
| Undo | After delete: toast "Item deleted. Undo." Duration: 4000ms. If undo: item scales back in (300ms spring) |
| Haptic | Medium impact at threshold crossing. Heavy impact on delete |

### 7.12 Drag Reorder

| Parameter | Value |
|---|---|
| Trigger | Long press (400ms) on drag handle or item |
| Lift animation | Item scale: 1.0 -> 1.05 (200ms). Shadow increases: offsetY 4px, blur 12px, opacity 0.3 |
| Drag phase | Item follows finger with 0ms latency |
| Auto-scroll | Near scroll boundary (top/bottom 50px): scroll speed 10px per 100ms |
| Other items response | Items shift to make room: 200ms, spring tension 150, friction 20 |
| Drop | Snaps to nearest valid position: 200ms, spring tension 200, friction 25, mass 1.5 |
| Failure to find position | Returns to original position: 250ms, spring tension 200, friction 25 |
| Haptic | Light at drag start. Light per item slot crossed. Medium on drop |

### 7.13 Long Press

| Parameter | Value |
|---|---|
| Trigger | Touch hold for 400ms |
| Detection window | 400ms without significant movement (>10px) |
| Visual progress | Circular timer fills around touch point over 400ms with decelerate ease |
| Visual cue at 300ms | Ring reaches 75% fill. Subtle scale on element: 1.0 -> 1.02 (100ms) |
| Completion (400ms) | Ring reaches full fill. Haptic feedback (heavy impact). Element enters active state |
| Context menu | Appears at touch point: fades in + scales up (200ms, spring tension 200, friction 20). Items stagger at 50ms each |
| Cancel | Finger moves >10px before 400ms: ring fades out (100ms). Element returns to rest (100ms) |

### 7.14 Tooltip

| Parameter | Value |
|---|---|
| Trigger | Hover (desktop) or tap-info-icon (mobile) |
| Show duration | 200ms |
| Show easing | Decelerate: cubic-bezier(0, 0, 0.2, 1) |
| Show property | Opacity: 0 -> 1. TranslateY: 4px -> 0px (below element) or -4px -> 0px (above) |
| Stay duration | 3000ms (auto-hide) or until user interacts elsewhere |
| Hide duration | 200ms |
| Hide easing | Accelerate: cubic-bezier(0.4, 0, 1, 1) |
| Hide property | Opacity: 1 -> 0 |
| Arrow | Small triangle pointing to trigger element. Fades in with tooltip |
| Dismiss on | Tap outside, press Escape, or auto-hide timer expires |
| Rich tooltip (with actions) | No auto-hide. Dismiss on action taken or tap outside |

### 7.15 Badge Update

| Parameter | Value |
|---|---|
| Trigger | Count changes (increment, decrement, set) |
| Badge position | Top-right corner of parent element |
| Badge size | 18px x 18px (single digit), 22px x 18px (double digit) |
| Badge background | Red (notification) or Brand primary (info count) |
| Badge text color | White |
| Update animation | Spring bounce |
| Duration | 300ms |
| Easing | Spring: tension 400, friction 15, mass 0.5 |
| Scale sequence | 0 -> 1.3 -> 0.9 -> 1.0 |
| Content (text) | If text changes: old text fades out (100ms). New text fades in (100ms, 50ms delay) |
| Decrement | If count goes to 0: badge scales to 0 (200ms, accelerate). Badge removed after animation |
| Notification bell | Badge appears: bell icon shakes (30 degree rotation, 3 oscillations, 300ms) |

### 7.16 Notification Slide In

| Parameter | Value |
|---|---|
| Trigger | Push notification received / in-app notification |
| Entry position | Top of viewport (or right edge for side-panel style) |
| Entry animation | Slides down from top (or in from right) |
| Duration (entry) | 350ms |
| Easing (entry) | Decelerate: cubic-bezier(0, 0, 0.2, 1) |
| Translation | TranslateY: -100% -> 0 |
| Opacity | 0 -> 1 (fades in during first 150ms) |
| Stay duration | 4000ms (configurable per notification type) |
| Exit animation | Slides up (or back to right): 250ms, accelerate |
| Swipe to dismiss | Swipe up/right: follows finger. Snap at 30% threshold. Dismiss: 200ms |
| Content fade | Message text fades in at 100ms delay (150ms, decelerate) |
| Action button | Appears at 200ms delay (150ms fade in) |

### 7.17 Progress Fill

| Parameter | Value |
|---|---|
| Trigger | Determinable progress operation (download, upload, processing) |
| Track | Full width of container, 4px height |
| Fill | Brand primary color, rounded right edge |
| Fill animation | Width percentage animates from previous value to current value |
| Duration | Based on actual progress. Minimum: 300ms for any fill change |
| Easing | Decelerate: cubic-bezier(0, 0, 0.2, 1) |
| Fill step | If progress jumps more than 20%: animate at minimum 300ms |
| Continuous fill | If progress updates every <100ms: throttle to 100ms updates |
| Completion | Fill reaches 100%: brief pause (200ms) then bar glows (300ms). Then either dismiss (fade out, 200ms) or change to success indicator |
| Indeterminate | Bar has animated gradient/shadow sweep. Duration: 1000ms per cycle |
| Buffer (video/streaming) | Second lighter bar ahead of primary fill. Animates independently |

### 7.18 Skeleton Pulse

| Parameter | Value |
|---|---|
| Trigger | Content is loading, show placeholder |
| Skeleton element | Rectangular blocks matching eventual content shape |
| Skeleton color | Light gray (Gray-200) |
| Pulse type | Shimmer: gradient sweep across skeleton elements |
| Shimmer gradient | Linear gradient: Gray-200 -> Gray-100 -> Gray-200 |
| Shimmer direction | 45 degrees, left to right |
| Cycle duration | 1500ms |
| Easing | Linear |
| Opacity of shimmer | Peak: 0.8 opacity on Gray-100 stripe. Trough: 0.4 on Gray-200 base |
| Element-specific skeletons | Avatar: circle. Text: 3 lines, varying width. Image: rectangle. Button: pill |
| Content replacement | When content loads: skeleton fades out (150ms). Content fades in (200ms, 50ms overlap) |
| Error state | Skeleton fades to error state (red tint, 200ms). Error icon appears (150ms) |

---

## 8. Gesture Animations

### 8.1 Swipe Card

**Used when:** User swipes a card to dismiss, complete, or categorize content.

| Parameter | Value |
|---|---|
| Trigger | Horizontal touch drag on card |
| Tracking | Card position follows finger position with 1:1 mapping |
| Translation | translateX = fingerDeltaX |
| Rotation | rotate = fingerDeltaX * 0.05 (max +/- 15 degrees) |
| Scale | Starts at 1.0. At 50% threshold: 0.95. At 100% threshold: 0.85 |
| Opacity | Starts at 1.0. At 50% threshold: 0.8. At 100% threshold: 0.5 |
| Threshold | 30% of card width |
| Below threshold release | Card snaps back to center. Spring: tension 250, friction 25, mass 1. 300ms |
| Above threshold release | Card continues in swipe direction and exits screen. Duration: 400ms from release. Scale continues to 0.5. Opacity continues to 0 |
| Dismiss direction | Left: dismiss/reject. Right: complete/approve |
| Overlay opacity | abs(fingerDeltaX) / cardWidth * 2 (max opacity 0.3 at 50% threshold, 0.5 at 100%) |
| Stack behavior | Next card: scale from 0.95 to 1.0, opacity 0.5 to 1.0. Starts at 50% threshold. Duration: 300ms. Decelerate |
| Undo | After dismiss: toast "Card dismissed. Undo." Duration: 3000ms. Tap undo: card flies back from dismiss direction (300ms, decelerate) |
| Completion indicator | When card passes 100% threshold: haptic feedback (heavy impact) |

### 8.2 Swipe List Item

| Parameter | Value |
|---|---|
| Trigger | Horizontal touch drag on list item |
| Direction | Left (primary) or right (secondary actions) |
| Tracking | Item translateX follows finger delta |
| Resistance | After action buttons fully revealed: 0.3x rate |
| Primary action width | 80px (single action) or 80px per action (multiple actions) |
| Icon appearance | Scale from 0.5 to 1.0 as button is revealed. Progress-based: scale = min(revealPercent * 2, 1.0) |
| Action button color | Delete: Red (#EF4444). Archive: Blue (#3B82F6). Mark as read: Green (#10B981) |
| Threshold | 50% of the first action button width |
| Snap to reveal | Spring: tension 200, friction 25. Duration: 250ms |
| Snap back | Spring: tension 200, friction 25. Duration: 250ms |
| Action trigger | Tap on revealed action: action executes. Item returns to rest (200ms). Haptic |
| Full swipe (delete) | If user swipes >70% of screen width: item deletes immediately. Item collapses to 0 (250ms, accelerate) |

### 8.3 Pinch Zoom

| Parameter | Value |
|---|---|
| Trigger | Two-finger pinch gesture |
| Tracking | Scale follows distance between fingers: scale = initialScale * (currentDistance / initialDistance) |
| Scale origin | Midpoint between two fingers |
| Min scale | 1.0 (or image-native scale) |
| Max scale | 5.0 (images), 3.0 (maps/graphs), 2.0 (timelines) |
| Scale clamping | Clamped to [minScale, maxScale]. Rubbery feel at limits: 10% overshoot allowed |
| Snap back (beyond limits) | Spring: tension 300, friction 25, mass 1. Duration: 200ms |
| Content pan | Content translates within container when zoomed beyond bounds |
| Double-tap zoom | Tap twice: zoom to 2.0x centered on tap point (300ms, decelerate). Double-tap again: zoom to 1.0x (300ms, decelerate) |

### 8.4 Drag

| Parameter | Value |
|---|---|
| Trigger | Touch drag on draggable element |
| Tracking | Element follows finger with 0ms latency. Position = initialPosition + fingerDelta * 1.0 |
| Lift | On drag start: scale 1.0 -> 1.05 (100ms, spring). Shadow: offsetY 4px -> 8px, blur 8px -> 16px, opacity 0.2 -> 0.3 |
| Container bounds | Element constrained to container bounds |
| Inertia on release | If released with velocity > 100px/s: continues with momentum. Deceleration: friction coefficient 0.95 per frame |
| Snap behavior | Nearest snap point on release. Threshold: 20px. Snap animation: 200ms, spring tension 200, friction 25 |
| Return to origin | If cancelled: fly back. Animation: 300ms, decelerate. Path: slightly arced |

### 8.5 Gesture Conflict Resolution

| Gesture Combination | Priority | Resolution |
|---|---|---|
| Scroll vs. Swipe card | Scroll: vertical. Swipe: horizontal | If finger moves more than 10px in detected axis, lock to that axis |
| Swipe list item vs. Scroll | Scroll: vertical. Swipe list: horizontal | If vertical movement > horizontal, treat as scroll |
| Drag reorder vs. Scroll | Drag: long press first. Scroll: immediate | Long press (400ms) activates drag. Before 400ms, treat as scroll |
| Pinch zoom vs. Pan | Both active simultaneously | Pan if one finger moves. Pinch if both fingers move relative to each other |
| Tap vs. Swipe | Tap is < 300ms and < 10px movement | If finger lifts before threshold: tap. After threshold: swipe |
| Long press vs. Tap | Long press: >400ms, <10px | Temporal threshold determines classification |

---

## 9. Learning Mode Animations

### 9.1 Story Page Turn

**Page Curl (Standard):**

| Parameter | Value |
|---|---|
| Trigger | Tap on page edge or swipe from corner |
| Duration | 350ms |
| Easing | Standard: cubic-bezier(0.2, 0, 0, 1) |
| Curl origin | Bottom-right corner (or direction of swipe) |
| Shadow on curling page | Dynamic shadow: width 8px, blur 12px, opacity 0.2 |

**Page Slide (Quick):**

| Parameter | Value |
|---|---|
| Duration | 250ms |
| Easing | Standard: cubic-bezier(0.2, 0, 0, 1) |
| Current page | Slides left: translateX 0 -> -100% |
| Next page | Slides in from right: translateX 100% -> 0 |

**Navigation:**
- Forward: swipe from right edge or tap right side
- Backward: swipe from left edge or tap left side
- Multiple page turn: skip animation (replace with instant page change + 100ms fade)

### 9.2 Quiz Answer Reveal

**Card Flip (Standard):**

| Parameter | Value |
|---|---|
| Trigger | User taps an answer option |
| Duration | 400ms |
| Easing (rotation) | Standard: cubic-bezier(0.2, 0, 0, 1) |
| Rotation axis | Y-axis (horizontal flip) |
| Rotation | 0 degrees -> 90 degrees (front hidden) -> 90 degrees -> 0 degrees (back shown) |
| Scale during flip | 1.0 -> 0.9 at 90 degrees -> 1.0 |
| First half duration | 200ms (0 -> 90 degrees) |
| Second half duration | 190ms (90 -> 0 degrees) |
| Correct result | Back face: green background, checkmark, "Correct!" text, particle burst on completion |
| Incorrect result | Back face: gentle red tint, X icon, correct answer highlighted, encouraging message |

**Scale Reveal (Alternative):**

| Parameter | Value |
|---|---|
| Duration | 300ms |
| Easing | Spring: tension 250, friction 20, mass 1 |
| Selected answer | Scales from 1.0 -> 1.1 |
| Background | Fills with color (correct: green, incorrect: red). Duration: 200ms |
| Correct indicator | Checkmark scales from 0 -> 1 (200ms, spring tension 300, friction 18) |
| Incorrect indicator | X scales from 0 -> 1 (200ms, spring tension 200, friction 25) |
| Explanation | Fades in below answer (300ms, decelerate, 100ms delay) |
| Other answers | Fade out (150ms, accelerate) or dim (opacity 0.4, 200ms) |

### 9.3 Flashcard Swipe

| Parameter | Value |
|---|---|
| Trigger | Horizontal swipe on flashcard |
| Duration (swipe out) | 400ms (when user releases beyond threshold) |
| Easing (swipe out) | Decelerate: cubic-bezier(0, 0, 0.2, 1) |
| Duration (next card in) | 350ms |
| Easing (next card in) | Decelerate |
| Swipe threshold | 30% of card width |
| Below threshold | Card snaps back: spring tension 250, friction 25, mass 1. 300ms |
| Above threshold | Card flies out. Rotation: +/- 15 degrees. Scale: 1.0 -> 0.85. Opacity: 1.0 -> 0.5 |
| Next card entrance | Slides from opposite direction. Scale: 0.9 -> 1.0. Opacity: 0.5 -> 1.0. Translates 60px -> 0px |
| Stack | 3-5 cards in stack. Top: full. Next: scale 0.95, slight vertical offset. Each subsequent: scale reduced by 0.03, offset 4px |
| Card flip (reveal answer) | Tap on card: flips (see Section 9.2). Duration: 400ms |
| Know / Don't Know buttons | On tap: card animates to left (don't know) or right (know). Same swipe animation: 400ms |
| Auto-advance | After flip: card auto-advances after 2000ms (configurable). Card slides out (300ms, accelerate). Next card enters (350ms, decelerate) |

### 9.4 Knowledge Tree Node Grow

| Parameter | Value |
|---|---|
| Root grow duration | 200ms |
| Root grow easing | Decelerate: cubic-bezier(0, 0, 0.2, 1) |
| Node body appearance | Scale from 0 to 1 |
| Node body duration | 500ms |
| Node body easing | Decelerate |
| Node body overshoot | 1.05 at midpoint, settle to 1.0 |
| Node content (icon/text) | Fades in: 200ms, 50ms after node body appears |
| Node color transition | Gray (locked) -> Brand color (unlocked). Duration: 300ms |
| Particle burst | 20-40 particles from node center |
| Particle duration | 800ms |
| Particle physics | Gravity: 0.5px/frame^2. Initial velocity: 50-150px/s |
| Particle colors | Brand color variants, 50-80% opacity |
| Particle sizes | 2-6px diameter circles |
| Particle spread | 360 degrees, random |

### 9.5 Knowledge Graph

| Parameter | Value |
|---|---|
| Physics simulation | Force-directed layout |
| Repulsion force | Strength 500, distance 200px |
| Attraction force | Strength 0.05, distance 100px |
| Damping | 0.9 per tick |
| Tick rate | 60 ticks per second |
| Iterations | 300 iterations for initial layout |
| Node entry | Fly in from edge to calculated position. Duration: 1000ms. Easing: decelerate |
| Connection draw | Stroke-dashoffset animation. Duration: 500ms. Easing: linear |
| Hover on node | Node scales to 1.15 (200ms spring). Unconnected nodes dim (opacity 1.0 -> 0.3, 200ms) |
| Tap on node | Graph re-centers on node (500ms, decelerate). Connections brighten (200ms) |
| Pinch zoom | Min: 0.5x. Max: 3.0x |

### 9.6 Timeline Scroll

| Parameter | Value |
|---|---|
| Scroll type | Horizontal or vertical |
| Snap behavior | Snaps to nearest event: 300ms after scroll ends |
| Snap threshold | 50% of event item width/height |
| Snap easing | Standard: cubic-bezier(0.2, 0, 0, 1) |
| Event entrance | Fade + translate: opacity 0 -> 1, translateX 20px -> 0px |
| Event duration | 400ms |
| Event easing | Decelerate |
| Event stagger | 100ms between events |
| Timeline line | Draws as user scrolls (stroke-dashoffset). Length follows scroll progress |
| Timeline dot | Scales from 0 to 1 when event becomes active (200ms, spring) |
| Active event | Dot: brand color, enlarged (1.3x). Pulse: 2s cycle, 1.0 -> 1.1 scale |

### 9.7 Map Zoom

| Parameter | Value |
|---|---|
| Zoom type | Smooth pinch-zoom (see Section 8.3) |
| Min zoom | 0.5x |
| Max zoom | 4.0x |
| Default zoom | 1.0x |
| Zoom to location | Animated zoom to tapped point. Duration: 400ms. Easing: decelerate |
| Map markers | Scale non-linearly with map |
| Marker tap | Marker expands to show label: scale 1.0 -> 1.3 (200ms spring). Label appears (fade + translate, 150ms) |
| Tile loading | Tiles load progressively from center outward. Each tile fades in (200ms). Stagger: 50ms between rings |

### 9.8 Memory Garden Plant Grow

| Parameter | Value |
|---|---|
| Plant growth | Scale from 0 to 1 |
| Growth duration | 800ms |
| Growth easing | Decelerate: cubic-bezier(0, 0, 0.2, 1) |
| Color saturation | Desaturated (weak) -> vibrant (strong). Duration: 800ms |
| Bloom/Flower | At milestones: flower blooms. Duration: 600ms. Easing: spring (tension 200, friction 18) |
| Leaf appearance | Each review: new leaf grows. Duration: 500ms. Easing: decelerate |
| Plant sway | Gentle 5 degree rotation oscillation, 4s cycle |
| Water drop effect | Drop falls from top, lands on plant. Duration: 600ms. Gravity: 0.8px/frame^2. Splash: 5 particles, 400ms |
| Growth sparkle | At growth threshold: sparkle particles (10, gold/white, 600ms) |

---

## 10. Character/Avatar Animations

### 10.1 Idle Breathing

| Parameter | Value |
|---|---|
| Cycle duration | 3000ms |
| Scale oscillation | 1.0 -> 1.01 -> 1.0 (chest/body area) |
| Easing | Standard: cubic-bezier(0.2, 0, 0, 1) |
| Y-offset | 0 -> -1px -> 0 |
| Amplitude randomization | +/- 0.001 scale variation per cycle |
| Cycle variation | 2800-3200ms |

### 10.2 Blink

| Parameter | Value |
|---|---|
| Interval | 4000-6000ms (randomized) |
| Duration | 100ms (closed), 200ms total |
| Easing | Standard for close, decelerate for open |
| Eye shape | Eyelids close from top and bottom, meeting at middle |
| Both eyes | Synchronized |
| During speaking | 3000-5000ms interval |
| Emotional blink | Surprise: delayed open (120ms closed). Sad: slow blink (300ms total) |

### 10.3 Expression Change

| Parameter | Value |
|---|---|
| Duration | 500ms |
| Easing | Decelerate: cubic-bezier(0, 0, 0.2, 1) |
| Morph type | Blend shapes (morph targets) with 0-100% weight |
| Expression states | Neutral, Happy, Sad, Thinking, Surprised, Confused, Encouraging |
| Sequence | Anticipation (50ms) -> Morph (400ms) -> Settle (50ms) |

### 10.4 Speaking

| Parameter | Value |
|---|---|
| Lip sync | Driven by audio amplitude and phoneme detection |
| Phoneme groups | 6 mouth shapes: rest, wide (E), round (O), closed (M/B/P), open (A), tight (F/V) |
| Mouth shape transition | 50ms blend between phonemes |
| Gesture frequency | Every 5-15 seconds of speech |
| Gesture types | Nod, head tilt, hand movement, eyebrow raise |
| Gesture duration | 600-1000ms |
| Gesture easing | Anticipation (100ms) -> Action (400-700ms) -> Follow-through (100-200ms) |
| Breathing during speech | Faster cycle: 1500-2000ms. Scale: 1.0 -> 1.008 -> 1.0 |
| Pause behavior | Holds current expression. Micro-movements. Resumes with anticipation (150ms) |

### 10.5 Listening

| Parameter | Value |
|---|---|
| Head tilt | 2-4 degrees, alternating sides every 3-5 seconds |
| Eye direction | Looking slightly upward and to the side |
| Sway | 3px horizontal oscillation. 4000ms cycle |
| Occasional nod | Every 10-15 seconds: 3-5 degree nod, 500ms |
| Blink | 5000-7000ms interval |
| Micro-expression | Eyebrow raise every 8-12 seconds (100ms raise, hold 500ms, release 100ms) |

### 10.6 Happy

| Parameter | Value |
|---|---|
| Transition from neutral | 400ms |
| Easing | Spring: tension 200, friction 18, mass 0.8 |
| Mouth | Smile: corners move up 8px |
| Eyes | Lower eyelid raises 2px. Eyebrow raises 2px |
| Cheeks | Lift 3px upward |
| Brightness | 2% luminance increase |
| Decay to neutral | Over 500ms |

### 10.7 Sad

| Parameter | Value |
|---|---|
| Transition from neutral | 400ms |
| Easing | Decelerate: cubic-bezier(0, 0, 0.2, 1) |
| Mouth | Corners turn down 4px |
| Eyes | Upper eyelid drops 3px. Eyebrow inner corners raise 2px, angle 10 degrees |
| Head | Downward tilt: 5 degrees |
| Posture | Shoulders drop 5px |
| Decay to neutral | Over 800ms |

### 10.8 Thinking

| Parameter | Value |
|---|---|
| Transition from neutral | 600ms |
| Easing | Decelerate |
| Gaze | Eyes look upward 10 degrees. Slight squint |
| Head tilt | 5-8 degrees to one side |
| Eyebrow | One raised (3px), one neutral or furrowed (1px down) |
| Mouth | Slight pursing |
| Posture | Lean back 5 degrees |
| Micro-movements | Head bobs (1-2 degrees, 2000ms cycle). Eye saccades (20 degrees, 300ms each, every 2-4 seconds) |

### 10.9 Surprise

| Parameter | Value |
|---|---|
| Transition from neutral | 300ms |
| Easing | Accelerate (onset) then decelerate (hold) |
| Eyes | Upper eyelid raises 5px. Eyebrow raises 6px. Visible eye area +30% |
| Mouth | Jaw drops 8-12px |
| Head | Pulls back 5px, tilts up 3 degrees |
| Hold | Freeze: 500-800ms |
| Recovery | Return to neutral: 500ms. Blink at start |

### 10.10 Greeting

| Parameter | Value |
|---|---|
| Trigger | User opens app or character becomes visible |
| Duration | 800ms |
| Wave | Hand rises (300ms). Waves: 30 degree rotation, 3 oscillations (300ms). Returns (200ms) |
| Nod | 10 degree nod down, then up. Duration: 500ms |
| Expression | Happy expression onset 200ms before gesture |
| Speech bubble | "Hello!" appears: scale 0 -> 1 (200ms spring) |

### 10.11 Farewell

| Parameter | Value |
|---|---|
| Trigger | User closes app or character exits |
| Duration | 600ms |
| Wave | 30 degree rotation, 2 oscillations (300ms) |
| Expression | Soft smile, slight eyebrow raise (400ms onset) |
| Fade out | 1.0 -> 0 opacity. Duration: 300ms. Easing: decelerate |
| Speech bubble | "See you tomorrow!" fades in (200ms). Fades out with character (200ms) |

---

## 11. Knowledge Tree Growth

### 11.1 New Node Creation

**Root Growth (600ms):**

| Parameter | Value |
|---|---|
| Root appearance | Thin stem extends from parent toward new node position |
| Duration | 600ms |
| Easing | Decelerate: cubic-bezier(0, 0, 0.2, 1) |
| Root path | Organic S-curve between parent and child. Control point offset: 20-40px orthogonal |
| Root width | 2px -> 4px over duration |
| Root color | Parent branch color -> node-specific color. Duration: 300ms after connection |
| Root taper | Wider at parent, narrower at child |

**Node Bloom (400ms):**

| Parameter | Value |
|---|---|
| Trigger | Root connects to position (200ms before root completes) |
| Node appears | Circle/rounded shape scales from 0 to 1 |
| Duration | 400ms |
| Easing | Spring: tension 250, friction 18, mass 0.8 |
| Scale sequence | 0 -> 1.1 -> 0.95 -> 1.0 |
| Color | Saturates from gray to brand color: 300ms after scale completes |
| Shadow | Scale 0 -> 1 (200ms, decelerate). Shadow: offsetY 2px, blur 6px, opacity 0.2 |
| Node size | 32-48px diameter (depending on hierarchy) |

**Particle Burst (800ms):**

| Parameter | Value |
|---|---|
| Trigger | Node bloom completes (at 400ms) |
| Particle count | 20-40 particles |
| Emission point | Node center |
| Initial velocity | Radial: 50-150px/s (randomized) |
| Gravity | 0.3px/frame^2 downward |
| Particle lifetime | 800ms |
| Particle size | 2-6px diameter |
| Particle color | Brand color palette, 60-100% opacity |
| Particle fade | Fade out over last 200ms of life |
| Spread | 360 degrees, random |

**Icon/Leaf Appear (300ms):**

| Parameter | Value |
|---|---|
| Trigger | Node bloom completes (at 400ms) |
| Animation | Fade in + scale from 0 to 1 |
| Duration | 300ms |
| Easing | Spring: tension 200, friction 20, mass 0.5 |
| Icon position | Centered within node |
| Icon color | White (on brand-colored node) |

### 11.2 Branch Extension

| Parameter | Value |
|---|---|
| Trigger | New node added to tree |
| Branch path | Organic curve from parent node edge to child node |
| Duration | 1000ms |
| Easing | Decelerate: cubic-bezier(0, 0, 0.2, 1) |
| Branch width | 3px (parent level), tapering to 2px (child level) |
| Branch color | Brand color (mastered), gray (unvisited), gradient (in progress) |
| Drawing method | Stroke-dasharray / stroke-dashoffset: line draws from parent to child |
| Branch fork | Main branch to fork point (500ms). Sub-branches to each child (500ms each, 100ms stagger) |
| Branch animation on scroll | Draw as user scrolls into view. Closer branches animate first |

### 11.3 Leaf/Icon on Node

| Parameter | Value |
|---|---|
| Default state | Icon visible at full opacity |
| Hover | Icon scales to 1.1 (200ms spring). Subtle glow (opacity 0.2, 200ms) |
| Tap | Icon scales 1.0 -> 0.9 -> 1.05 -> 1.0 (300ms spring). Info panel appears |
| Seasonal change | Old icon fades out (200ms). New icon fades in (200ms) |
| Leaf variant | Gentle sway: 5 degree rotation oscillation, 3s cycle |

### 11.4 Mastery Glow

| Parameter | Value |
|---|---|
| Effect | Subtle pulsating glow around node |
| Pulse cycle | 2000ms |
| Glow radius | 0 -> 8px -> 0 |
| Glow opacity | 0.3 -> 0.1 -> 0.3 |
| Glow easing | Standard: cubic-bezier(0.2, 0, 0, 1) |
| Glow color | Gold (#FFD700) or brand accent |
| Glow blur | 8px |
| Node border | 0 to 2px, gold color. Duration: 500ms |
| Star indicator | Small star fades in (300ms). Subtle sparkle: 5s cycle (scale 1.0 -> 1.1 -> 1.0) |

### 11.5 Connection Lines

| Parameter | Value |
|---|---|
| Drawing animation | Stroke-dashoffset from parent to child |
| Duration | 500ms |
| Easing | Decelerate: cubic-bezier(0, 0, 0.2, 1) |
| Line style | Solid (mastered), dashed (incomplete), dotted (future) |
| Solid line width | 2px |
| Dashed line width | 2px, dash 4px gap 4px |
| Dotted line width | 2px, dot 2px gap 6px |
| Line color | Brand (active), Gray (inactive), Gold (mastered) |
| Hover on line | Width increases to 4px (150ms). Tooltip: relationship type (200ms) |
| Animate on hover | Path from root to hovered node pulses (opacity 0.5 -> 1.0 -> 0.5, 1s cycle) |

### 11.6 Tree Seasonal Changes

| Parameter | Value |
|---|---|
| Seasonal transition duration | 3000ms |
| Color shift | Spring (greens), Summer (vibrant greens + flowers), Autumn (oranges/reds), Winter (muted blues/grays) |
| Leaf density | Spring: 30%. Summer: 100%. Autumn: 50% (falling). Winter: 0% |
| Leaf fall (autumn) | 20-50 leaves over 3000ms. Each: 2s fall. Gravity: 0.5px/frame^2. Sway: 10 degree oscillation |
| Snow (winter) | Snow particles: 3px. Fall speed: 30px/s. Continuous during winter |

---

## 12. Memory Pulse Animation

### 12.1 Card Memory Pulse

| Parameter | Value |
|---|---|
| Effect | Gentle heartbeat pulse on the card |
| Pulse cycle | 2000ms |
| Scale range | 0.98 -> 1.01 -> 0.98 |
| Easing | Standard: cubic-bezier(0.2, 0, 0, 1) |
| Pulse shape | Quick expansion (300ms to 1.01), gentle return (1700ms to 0.98) |
| Opacity change | 1.0 -> 0.97 at peak |
| Shadow change | Rest shadow -> slightly larger at peak (offsetY 2px -> 3px, blur 6px -> 8px) |
| When active (user reviewing) | Pulse pauses. Card returns to rest (100ms) |
| When review is due | Pulse intensifies: scale range 0.97 -> 1.02. Cycle shortens to 1500ms |

### 12.2 Strength Indicator

| Parameter | Value |
|---|---|
| Duration | 2000ms per stage change |
| Easing | Decelerate: cubic-bezier(0, 0, 0.2, 1) |
| Stage thresholds | 0% (new), 25% (seedling), 50% (growing), 75% (blooming), 100% (mastered) |
| Stage transition color | Gray -> green -> vibrant green -> gold. Duration: 2000ms |
| Stage indicator icon | Old icon fades out (200ms). New icon fades in (200ms) |
| Numerical (1-10) | Counting animation. Duration: 1000ms. Easing: decelerate |
| Growth sparkle | On stage completion: 10 particles, 600ms |

### 12.3 Review Reminder

| Parameter | Value |
|---|---|
| Effect | Soft glow pulse on the card/item due for review |
| Pulse cycle | 1500ms |
| Glow color | Brand accent or gentle orange |
| Glow opacity | 0 -> 0.15 -> 0 |
| Glow radius | 0px -> 12px -> 0px |
| Easing | Standard: cubic-bezier(0.2, 0, 0, 1) |
| Scale | No scale change |
| Card border | Brand color, 1px -> 2px -> 1px (matches glow cycle) |
| Badge (multiple reminders) | Number of items due: badge appears (Section 7.15). Scale: 0 -> 1.3 -> 1.0 (300ms spring) |

### 12.4 Memory Consolidation

| Parameter | Value |
|---|---|
| Effect | Particle swirl from card (short-term) to tree (long-term storage) |
| Duration | 1200ms |
| Particle count | 30-50 particles |
| Particle path | Spiral or arc from card center to tree/network node |
| Particle size | 3-5px |
| Particle color | Brand color gradient |
| Particle lifetime | Full duration (1200ms) |
| Particle speed | Starts slow (200ms), accelerates (600ms), decelerates (400ms) |
| Card completion | Particles depart: card fades out (300ms) or changes state |
| Tree reception | Gentle glow pulse at reception point (300ms). Node brightens (200ms) |
| Multiple consolidations | Stagger particle paths by 200ms each |

### 12.5 Forgetting

| Parameter | Value |
|---|---|
| Effect | Gradual desaturation and dimming |
| Duration | 3000ms |
| Desaturation | Color -> reduced saturation -> grayscale |
| Opacity | 1.0 -> 0.6 |
| Scale | 1.0 -> 0.95 |
| Easing | Linear |
| Plant (memory garden) | Leaves droop (5 degrees over 500ms), color desaturates (3000ms) |
| Card (review list) | Opacity: 1.0 -> 0.8. Border becomes dashed |
| Threshold alerts | 50% strength: reminder sent. 25%: urgent pulse + badge. 0%: removed (fade out, 500ms) |
| Recovery | If user reviews: animation reverses. Duration: 1000ms |

### 12.6 Water/Strengthen

| Parameter | Value |
|---|---|
| Effect | Splash effect + brighten |
| Duration | 500ms |
| Splash particles | 15-25 water-drop particles from top of card/plant |
| Particle physics | Gravity: 0.8px/frame^2. Initial velocity: 100-200px/s downward. Spread: 30 degree cone |
| Splash color | Light blue, opacity 0.3-0.6 |
| Card/plant brighten | Brand color sweep across card (300ms). 20% luminance increase, fades to 0% over 500ms |
| Scale pop | 1.0 -> 1.03 -> 1.0 (200ms spring) |
| Strength indicator | Bar/indicator increments (300ms). Decelerate ease |

---

## 13. AI Thinking Animation

### 13.1 Thinking Dots (Default)

| Parameter | Value |
|---|---|
| Effect | Three dots, sequential bounce |
| Dot count | 3 |
| Dot size | 8px diameter each |
| Dot spacing | 12px between centers |
| Dot color | Brand primary |
| Animation | Each dot bounces vertically |
| Bounce height | 0 -> -8px -> 0 (translateY) |
| Bounce duration | 300ms per dot |
| Easing (up) | Accelerate: cubic-bezier(0.4, 0, 1, 1) |
| Easing (down) | Decelerate: cubic-bezier(0, 0, 0.2, 1) |
| Stagger | Dot 1: 0ms. Dot 2: 300ms. Dot 3: 600ms |
| Cycle restart | After dot 3 completes (at 900ms), dot 1 starts again. Continuous loop |
| Opacity | Rest: 0.4. At peak: 1.0 |
| Transition to response | Dots stop at end of current cycle. Fade out: 200ms. Response fades in: 200ms. Cross-fade: 100ms |
| Reduced motion | Static dots at 0.6 opacity. No animation |

### 13.2 Brain Animation

| Parameter | Value |
|---|---|
| Effect | Stylized brain icon with neural network pulse |
| Cycle duration | 2000ms |
| Neural pathways | 6 pathways. Each: 300ms (fade in 100ms, hold 100ms, fade out 100ms) |
| Pathway stagger | 100ms between each pathway activation |
| Pathway order | Center -> left hemisphere -> right hemisphere -> center -> top -> bottom |
| Pulse color | Brand primary |
| Resting color | Gray, opacity 0.3 |
| Glow | 0 -> 0.1 -> 0, 2000ms cycle |
| Brain scale | 1.0 -> 1.02 -> 1.0, 2000ms cycle |
| Nodes | 5-8 dots on brain surface. Random slow pulse (each 1000-3000ms) |
| Reduced motion | Static brain icon with slow color pulse only |

### 13.3 Book Animation

| Parameter | Value |
|---|---|
| Effect | Book with pages turning slowly |
| Cycle duration | 3000ms |
| Page turn | Pages turn from left to right. 3-5 pages over cycle |
| Page turn duration | 600ms per page |
| Page turn easing | Standard: cubic-bezier(0.2, 0, 0, 1) |
| Page curl | Subtle page curl physics. Curl height: 5px at midpoint |
| Book glow | Gentle warm glow: opacity 0 -> 0.08 -> 0, 3000ms cycle |
| Reduced motion | Static book icon with gentle opacity pulse (1000ms) |

### 13.4 Lamp Animation

| Parameter | Value |
|---|---|
| Effect | Warm glow pulsing from lamp icon |
| Cycle duration | 1500ms |
| Glow opacity | 0.1 -> 0.25 -> 0.1 |
| Glow radius | 4px -> 10px -> 4px |
| Glow color | Warm amber/yellow |
| Easing | Standard: cubic-bezier(0.2, 0, 0, 1) |
| Lamp icon | Gentle brightness shift to match glow |
| Reduced motion | Static lamp with constant subtle glow (opacity 0.1) |

### 13.5 Thinking Stage Transitions

| Parameter | Value |
|---|---|
| Stage 1: Receiving | Icon appears (200ms fade). Subtle pulse (cycle duration: 1000ms) |
| Stage 2: Processing | Icon transitions to processing state (dots, brain, book, or lamp). Cross-fade: 300ms |
| Stage 3: Formulating | Icon accelerates pulse slightly (cycle shortens by 20%) |
| Transition to response | Icon fades out (200ms). Response content fades in (200ms). Cross-fade: 100ms |

---

## 14. Loading Animations

### 14.1 Skeleton Shimmer

| Parameter | Value |
|---|---|
| Trigger | Content loading indicator |
| Shimmer type | Gradient sweep across placeholder blocks |
| Gradient | Gray-200 -> Gray-100 -> Gray-200 |
| Gradient direction | 45 degrees, left to right |
| Cycle duration | 1500ms |
| Easing | Linear |
| Opacity range | 0.4 (base) -> 0.8 (peak shimmer) |
| Element shapes | Avatar: circle. Text lines: varying width rectangles. Image: rectangle. Button: pill |
| Content replacement | Skeleton fades out (150ms). Content fades in (200ms, 50ms overlap) |
| Error state | Red tint fade (200ms). Error icon (150ms) |

### 14.2 Progress Bar

| Parameter | Value |
|---|---|
| Determinate fill | Width animates to percentage. Minimum duration: 300ms |
| Fill easing | Decelerate: cubic-bezier(0, 0, 0.2, 1) |
| Fill color | Brand primary |
| Completion | Fill reaches 100%: glow (300ms) then success state or dismiss (200ms) |
| Indeterminate | Animated gradient sweep. Duration: 1000ms per cycle |
| Buffer (video) | Second lighter bar ahead of primary fill |

### 14.3 Spinner

| Parameter | Value |
|---|---|
| Rotation speed | 800ms per full rotation |
| Easing | Linear (continuous) |
| Icon | Circle outline or custom icon (leaf, brain, book) |
| Size | 20-40px (context-dependent) |
| Stroke width | 3px |
| Color | Brand primary |
| Stop transition | Decelerate rotation to stop over 300ms. Then cross-fade to content (200ms) |

### 14.4 Page Load

| Parameter | Value |
|---|---|
| Progressive reveal | Items appear in order: header, content blocks, images, CTAs |
| Item entrance | Fade + translateY 10px -> 0px |
| Duration per item | 300ms |
| Stagger interval | 50ms between items |
| Easing | Decelerate: cubic-bezier(0, 0, 0.2, 1) |
| Max stagger window | 500ms (then items continue appearing but faster) |
| Background | Background appears first (100ms). No animation on background |

### 14.5 Knowledge Load

| Parameter | Value |
|---|---|
| Effect | Tree grows from center outward |
| Duration | 1000ms |
| Node appearance | Center node first (200ms). Surrounding nodes radiate outward (800ms) |
| Node stagger | 100ms per ring of nodes |
| Branch draw | Branches extend from center to outer nodes (500ms each) |
| Easing | Decelerate for all elements |

### 14.6 Content Load Stagger

| Parameter | Value |
|---|---|
| Card entrance | Fade + translateY 20px -> 0px |
| Duration per card | 350ms |
| Stagger interval | 80ms between cards |
| Easing | Decelerate: cubic-bezier(0, 0, 0.2, 1) |
| Content within card | 50ms stagger per element |
| Image load | Image fades in on load: 300ms, decelerate |

---

## 15. Achievement & Reward Animations

### 15.1 Achievement Unlock

| Parameter | Value |
|---|---|
| Total duration | ~1500ms |
| Always skippable | User can tap to dismiss at any point |

**Phase 1: Burst Particles (600ms):**

| Parameter | Value |
|---|---|
| Particle count | 30-50 |
| Emission | Radial from center of achievement icon |
| Initial velocity | 100-300px/s (randomized) |
| Gravity | 0.4px/frame^2 |
| Particle size | 4-10px |
| Particle colors | Gold, brand primary, white |
| Particle lifetime | 600ms |
| Fade | Fade out over last 150ms |

**Phase 2: Icon Scale (400ms spring):**

| Parameter | Value |
|---|---|
| Start | Icon scales from center, size 0 |
| Easing | Spring: tension 300, friction 12, mass 0.7 |
| Scale sequence | 0 -> 1.3 -> 0.9 -> 1.05 -> 1.0 |
| Icon background | Circular background fills with brand color (200ms) |
| Icon shadow | Appears at 200ms: offsetY 4px, blur 12px, opacity 0.3 |

**Phase 3: Text Fade (300ms):**

| Parameter | Value |
|---|---|
| Achievement title | Fades in: opacity 0 -> 1, translateY 10px -> 0px |
| Duration | 300ms |
| Delay | 200ms after icon phase starts |
| Easing | Decelerate: cubic-bezier(0, 0, 0.2, 1) |
| Subtitle/description | Fades in at 400ms delay (250ms, decelerate) |

**Phase 4: Background Flash (200ms):**

| Parameter | Value |
|---|---|
| Screen background | Brief color flash: overlay opacity 0 -> 0.1 -> 0 |
| Duration | 200ms (first 100ms rise, second 100ms fall) |
| Color | Gold or brand accent |
| Easing | Standard: cubic-bezier(0.2, 0, 0, 1) |

### 15.2 XP Gain

| Parameter | Value |
|---|---|
| Effect | Number floats up + fades, +N text flies to XP bar |
| Total duration | 1000ms |
| Number pop | "+50 XP" text appears at point of action |

**Phase 1: Number Pop (300ms):**

| Parameter | Value |
|---|---|
| Scale | 0 -> 1.3 -> 1.0 |
| Easing | Spring: tension 300, friction 15, mass 0.5 |
| Color | Gold or brand accent, bright |

**Phase 2: Float Up (400ms):**

| Parameter | Value |
|---|---|
| Translation | TranslateY: 0px -> -40px |
| Easing | Decelerate: cubic-bezier(0, 0, 0.2, 1) |
| Opacity | 1.0 -> 0.8 |

**Phase 3: Fly to XP Bar (600ms):**

| Parameter | Value |
|---|---|
| +N text | Shrinks and flies to XP bar position |
| Path | Arc from text position to XP bar |
| Duration | 600ms |
| Arc height | 20px offset at midpoint |
| Easing (x) | Linear (horizontal) |
| Easing (y) | Decelerate (vertical) |
| Scale | Current scale -> 0 (arrives at bar) |
| XP bar fill | Bar fills by XP amount: 300ms, decelerate |

### 15.3 Level Up

| Parameter | Value |
|---|---|
| Total duration | ~2500ms |
| Always skippable | User can tap to dismiss |

**Phase 1: Screen Flash (100ms):**

| Parameter | Value |
|---|---|
| Full screen overlay | White opacity 0 -> 0.3 -> 0 |
| Duration | 100ms |
| Easing | Accelerate then decelerate |

**Phase 2: Progress Ring Fill (600ms):**

| Parameter | Value |
|---|---|
| Ring | Circular progress ring fills from current to 100% |
| Duration | 600ms |
| Easing | Decelerate: cubic-bezier(0, 0, 0.2, 1) |
| Ring color | Brand primary -> gold at completion |
| Ring stroke width | 6px |

**Phase 3: Celebration Particles (1000ms):**

| Parameter | Value |
|---|---|
| Particle count | 50-80 |
| Colors | Gold, white, brand colors |
| Initial velocity | 200-400px/s in all directions |
| Gravity | 0.3px/frame^2 |
| Particle shapes | Circles, stars, confetti rectangles |
| Particle lifetime | 1000ms |
| Fade | Over last 300ms |

**Phase 4: New Level Reveal (400ms):**

| Parameter | Value |
|---|---|
| Level number | "Level 5" scales from 0 to 1 |
| Easing | Spring: tension 250, friction 15, mass 0.7 |
| Scale | 0 -> 1.2 -> 0.95 -> 1.0 |
| Subtitle | "You leveled up!" fades in at 200ms delay (250ms) |
| Reward summary | Items stagger in at 100ms intervals (200ms each) |

### 15.4 Star Burst

| Parameter | Value |
|---|---|
| Effect | 5 stars animate in sequence |
| Stars | 5 stars arranged in arc or row |
| Duration per star | 200ms |
| Stagger interval | 100ms between stars |
| Easing | Spring: tension 350, friction 15, mass 0.5 |
| Scale per star | 0 -> 1.4 -> 0.9 -> 1.0 |
| Color | Gray -> gold (or brand accent). Duration: 150ms |
| Opacity | 0 -> 1 (stars invisible initially) |
| Rotation | -180 degrees -> 0 degrees per star (optional arc path) |
| Total duration | 5 x 200ms + 4 x 100ms = 1400ms |

### 15.5 Confetti

| Parameter | Value |
|---|---|
| Effect | 50+ particles showering from top of screen |
| Total duration | 2000ms |
| Particle count | 50-100 |
| Particle types | Rectangles (4x8px), circles (4px), stars (6px) |
| Colors | Gold, brand primary, brand secondary, white, accent colors |
| Initial position | Top of screen (random x within viewport) |
| Initial velocity (y) | 200-500px/s downward |
| Initial velocity (x) | -100 to 100px/s (random horizontal drift) |
| Rotation | Random initial rotation, rotates at 360 degrees/s |
| Gravity | 0.5px/frame^2 |
| Air resistance | 0.98 per frame (slight deceleration) |
| Particle lifetime | 2000ms |
| Fade | Fade out over last 500ms |
| Wind | Subtle horizontal oscillation: sin wave, amplitude 20px, period 1000ms |

### 15.6 Badge Earned

| Parameter | Value |
|---|---|
| Effect | Badge flips from back to front, shine sweeps across |
| Total duration | ~1100ms |

**Phase 1: Badge Flip (500ms):**

| Parameter | Value |
|---|---|
| Rotation axis | Y-axis |
| Start | Back face visible (gray/silhouette) |
| End | Front face visible (full color badge) |
| Duration | 500ms |
| Easing | Standard: cubic-bezier(0.2, 0, 0, 1) |
| Scale during flip | 1.0 -> 0.9 at 90 degrees -> 1.0 |
| Back face fade | Back face opacity 1 -> 0 at 270 degrees |
| Front face fade | Front face opacity 0 -> 1 at 90 degrees |

**Phase 2: Shine Sweep (600ms):**

| Parameter | Value |
|---|---|
| Effect | Shine/gloss sweeps across badge surface |
| Shine shape | Linear gradient with white highlight |
| Shine direction | Top-left to bottom-right |
| Shine duration | 600ms |
| Delay | 100ms after flip completes |
| Easing | Linear |
| Shine width | 30% of badge width |
| Shine opacity | 0 -> 0.4 -> 0 |

### 15.7 Streak Fire

| Parameter | Value |
|---|---|
| Effect | Flame ignites, grows, pulses for streak days |

**Ignite (400ms):**

| Parameter | Value |
|---|---|
| Flame opacity | 0 -> 1 |
| Flame scale | 0.5 -> 1.0 |
| Easing | Decelerate: cubic-bezier(0, 0, 0.2, 1) |
| Color | Start: blue base -> orange middle -> yellow tip. Duration: 400ms |

**Grow (600ms):**

| Parameter | Value |
|---|---|
| Flame scale | 1.0 -> 1.3 (larger flame for longer streak) |
| Easing | Decelerate |
| Color intensification | Saturates and brightens |
| Additional flames | Secondary flames appear for milestones (7-day, 30-day, 365-day) |

**Continuous Pulse:**

| Parameter | Value |
|---|---|
| Cycle duration | 2000ms |
| Scale oscillation | 1.0 -> 1.05 -> 1.0 |
| Opacity oscillation | 1.0 -> 0.85 -> 1.0 |
| Easing | Standard: cubic-bezier(0.2, 0, 0, 1) |
| Color flicker | Subtle hue shift (+/- 5 degrees) over cycle |

---

## 16. Success & Failure Animations

### 16.1 Success

| Parameter | Value |
|---|---|
| Effect | Green checkmark draws, subtle scale bounce, positive particles |
| Total duration | ~500ms |

**Phase 1: Checkmark Draw (300ms):**

| Parameter | Value |
|---|---|
| Circle background | Fills from center: 150ms, decelerate. Color: Green (#10B981) |
| Checkmark path | Draws via stroke-dashoffset: 200ms, decelerate |
| Checkmark color | White |

**Phase 2: Scale Bounce (400ms):**

| Parameter | Value |
|---|---|
| Element scale | 1.0 -> 1.1 -> 0.95 -> 1.0 |
| Easing | Spring: tension 250, friction 18, mass 0.8 |
| Duration | 400ms (starts 200ms after checkmark begins) |

**Phase 3: Positive Particles (500ms):**

| Parameter | Value |
|---|---|
| Particle count | 15-25 |
| Colors | Green, white, light green |
| Emission | From checkmark center, radial |
| Initial velocity | 50-150px/s |
| Particle lifetime | 500ms |
| Fade | Over last 150ms |

### 16.2 Correct Answer

| Parameter | Value |
|---|---|
| Effect | Card turns green, checkmark appears |
| Total duration | ~500ms |

**Card Color Transition (200ms):**

| Parameter | Value |
|---|---|
| Card background | White -> light green (#ECFDF5). Duration: 200ms, decelerate |
| Card border | Gray -> green (#10B981). Duration: 200ms |
| Text color | Dark -> green (if answer text). Duration: 200ms |

**Checkmark Appears (300ms):**

| Parameter | Value |
|---|---|
| Delay | 100ms after color transition starts |
| Checkmark scale | 0 -> 1.2 -> 1.0 |
| Easing | Spring: tension 300, friction 18, mass 0.5 |
| Checkmark color | Green (#10B981) |
| Duration | 300ms |

### 16.3 Failure

| Parameter | Value |
|---|---|
| Effect | Gentle red shake, X appears |
| Total duration | ~500ms |

**Shake (300ms):**

| Parameter | Value |
|---|---|
| Translation | Horizontal oscillation: 0px -> 5px -> -5px -> 3px -> -3px -> 0px |
| Oscillations | 3 (decaying amplitude) |
| Duration | 300ms |
| Easing | Custom shake curve or spring with tension 200, friction 10 |
| Color tint | Card tints red (#FEF2F2) over 200ms, decelerate |

**X Appears (200ms):**

| Parameter | Value |
|---|---|
| Delay | 150ms after shake starts |
| X scale | 0 -> 1.0 |
| Easing | Decelerate: cubic-bezier(0, 0, 0.2, 1) |
| X color | Red (#EF4444) |
| Duration | 200ms |

### 16.4 Wrong Answer

| Parameter | Value |
|---|---|
| Effect | Card briefly tints red, correct answer revealed |
| Total duration | ~900ms |

**Card Tint (400ms):**

| Parameter | Value |
|---|---|
| Card background | White -> light red (#FEF2F2) -> returns to white |
| Duration | 400ms total (rise 200ms, fall 200ms) |
| Easing | Accelerate (rise), decelerate (fall) |

**Correct Answer Reveal (500ms):**

| Parameter | Value |
|---|---|
| Delay | 300ms after tint starts |
| Correct answer | Highlights with green border (2px) + green checkmark |
| Highlight duration | 300ms, decelerate |
| Checkmark scale | 0 -> 1.0 (200ms spring) |
| Incorrect answer | Gentle red X appears on selected wrong answer |
| Explanation | Fades in below card: 300ms, decelerate, 100ms delay |

### 16.5 Encouraging Message

| Parameter | Value |
|---|---|
| Effect | Encouraging text fades in (never shaming, always constructive) |
| Trigger | Wrong answer or failure |
| Duration | 300ms |
| Easing | Decelerate: cubic-bezier(0, 0, 0.2, 1) |
| Translation | TranslateY: 10px -> 0px |
| Opacity | 0 -> 1 |
| Message examples | "Not quite — but you're learning!" / "Close! Review and try again." / "Every mistake is a step forward." |
| Color | Warm, encouraging (brand secondary or warm amber) |
| Stay duration | 3000ms (or until user interacts) |
| Fade out | 200ms, accelerate |

---

## 17. Daily Reward Animation

### 17.1 Daily Reward Sequence

| Parameter | Value |
|---|---|
| Total duration | ~2000ms |
| Always skippable | User can tap to dismiss |

**Phase 1: Reward Icon Spins In (400ms spring):**

| Parameter | Value |
|---|---|
| Icon | Daily reward icon (gift, star, chest) |
| Entrance | Scale from 0, rotate 360 degrees |
| Duration | 400ms |
| Easing (scale) | Spring: tension 250, friction 18, mass 0.7 |
| Scale sequence | 0 -> 1.2 -> 0.95 -> 1.0 |
| Rotation | 360 degrees over duration, decelerate ease |

**Phase 2: Streak Count Increments (300ms):**

| Parameter | Value |
|---|---|
| Streak number | Number counts up (e.g., 3 -> 4) |
| Counting animation | Each digit animates: old digit scrolls up, new digit scrolls in from bottom |
| Duration | 300ms |
| Easing | Decelerate: cubic-bezier(0, 0, 0.2, 1) |
| Color | Gold or brand accent when streak > 3. Fire icon appears at streak > 7 |
| Fire icon | Scale 0 -> 1.2 -> 1.0 (200ms spring) at streak = 7, 30, 100, 365 |

**Phase 3: Flames/Flowers Grow (600ms):**

| Parameter | Value |
|---|---|
| Effect | Decorative elements grow based on streak length |
| Short streak (1-6) | Small flowers bloom around icon. Duration: 400ms each, 100ms stagger |
| Medium streak (7-29) | Flame grows from small to medium. Duration: 500ms |
| Long streak (30-99) | Large flame + star sparkles. Duration: 600ms |
| Epic streak (100+) | Crown + fire + sparkle aura. Duration: 600ms |

**Phase 4: Reward Summary Fades In (200ms stagger per item):**

| Parameter | Value |
|---|---|
| Reward items | XP, coins, streak bonus, item unlocks |
| First item | 0ms delay, 200ms fade in |
| Second item | 100ms delay, 200ms fade in |
| Third item | 200ms delay, 200ms fade in |
| Fourth item | 300ms delay, 200ms fade in |
| Easing per item | Decelerate: cubic-bezier(0, 0, 0.2, 1) |
| Translation | TranslateY: 8px -> 0px |

**Phase 5: "Come Back Tomorrow" Fades (300ms):**

| Parameter | Value |
|---|---|
| Delay | 1200ms from sequence start |
| Duration | 300ms |
| Easing | Decelerate |
| Opacity | 0 -> 0.7 |
| Translation | TranslateY: 5px -> 0px |
| Icon | Small moon or clock icon appears next to text (200ms spring) |

### 17.2 Weekly Reward (Extended)

| Parameter | Value |
|---|---|
| Total duration | ~3000ms |
| Additional elements vs. daily | Mid-size confetti burst (30 particles, 1500ms). Larger reward icon. Sound effect on reveal |
| Phase timing | All phases same as daily but with 50% longer durations and 100ms additional stagger |

### 17.3 Monthly Reward (Full Celebration)

| Parameter | Value |
|---|---|
| Total duration | ~4000ms |
| Additional elements vs. weekly | Full confetti (50-80 particles, 2000ms). Screen flash (100ms). Animated title card. Progress ring fills to 100%. Special badge/icon unlock animation |
| Phase timing | All phases same as daily but with 100% longer durations and 150ms additional stagger |

---

## 18. Scroll & Parallax

### 18.1 Standard Scroll

| Parameter | Value |
|---|---|
| Scroll type | Smooth, momentum-based |
| Momentum | Preserved at finger release. Deceleration: 0.015 per frame |
| Rubber-banding | At scroll boundaries: content overshoots by 10% of viewport, springs back |
| Rubber-band spring | Tension 180, friction 25, mass 1. Duration: 250ms |
| Scroll bar | Auto-hides after 1000ms of inactivity. Fades out (200ms). Reappears on scroll (0ms) |
| Velocity tracking | Tracks finger velocity at release for natural momentum |

### 18.2 Parallax

| Parameter | Value |
|---|---|
| Effect | Background elements move at slower rate than scroll |
| Background speed | 0.5x scroll speed |
| Midground speed | 0.75x scroll speed |
| Foreground speed | 1.0x scroll speed (standard content) |
| Applied to | Maps (knowledge maps), timelines, memory gardens, landscape illustrations |
| Duration | Continuous (follows scroll position in real-time) |
| Easing | None (position is directly tied to scroll offset) |
| Depth layers | Max 3 layers (background, midground, foreground) |
| Parallax intensity | Max 30px displacement per layer at full scroll extent |
| Motion blur | None (parallax is smooth enough without blur) |

### 18.3 Sticky Header

| Parameter | Value |
|---|---|
| Trigger | User scrolls past header threshold |
| Header behavior | Normal position -> compact position |
| Transition duration | 300ms |
| Transition easing | Standard: cubic-bezier(0.2, 0, 0, 1) |
| Header height (normal) | 64px (varies per screen) |
| Header height (compact) | 48px |
| Elements | Title reduces font size. Subtitle hides (fade, 200ms). Actions condense |
| Shadow | Appears when header becomes sticky: offsetY 2px, blur 8px, opacity 0.15. Duration: 200ms |
| Scroll direction | Scroll down: header compresses. Scroll up: header expands. Reverse transition: 200ms |

### 18.4 Section Headers

| Parameter | Value |
|---|---|
| Trigger | Section header reaches top of viewport |
| Behavior | Header becomes sticky with elevated shadow |
| Elevation transition | Shadow appears: offsetY 1px, blur 4px, opacity 0.1. Duration: 200ms |
| Easing | Standard: cubic-bezier(0.2, 0, 0, 1) |
| Background | Header background becomes opaque (if was transparent). Duration: 200ms |
| Collapse | Content below header scrolls under it (standard sticky behavior) |

### 18.5 Fade on Scroll

| Parameter | Value |
|---|---|
| Effect | Content at viewport edges fades based on distance from edge |
| Top edge fade | Content within 100px of top edge: opacity = distance / 100 |
| Bottom edge fade | Content within 100px of bottom edge: opacity = distance / 100 |
| Left/right edge fade | Horizontal scroll: same logic, 50px threshold |
| Fade curve | Linear (opacity directly proportional to distance from edge) |
| Applied to | Scrollable containers with overflow content |

---

## 19. Reduced Motion Profile

### 19.1 Global Reduced Motion Settings

When `prefers-reduced-motion` is active OR user has selected "Reduced Motion" in app settings:

| Setting | Value |
|---|---|
| Duration multiplier | All durations reduced to 50% or 0ms |
| Scale animations | Replaced with opacity animations |
| Translational movement | Replaced with fade in/out |
| Parallax | Disabled entirely |
| Confetti | Disabled (use static sparkle icon instead) |
| Skeleton shimmer | Disabled (use static gray blocks) |
| AI thinking dots | Disabled (use static text "Thinking..." or typing indicator) |
| Card hover lift | Disabled (cards remain at rest elevation) |
| Screen transitions | Cross-fade only (no slide) |
| Ripple effects | Disabled (instant color change instead) |

### 19.2 Component-Specific Reduced Motion

| Component | Reduced Motion Alternative |
|---|---|
| Button press | Instant color change (no scale). Duration: 0ms |
| Card tap | Instant background tint change. Duration: 0ms |
| Toggle switch | Instant position + color change. Duration: 0ms |
| Checkbox | Instant fill + checkmark appear. Duration: 0ms |
| Modal | Instant appear (no scale). Opacity 0 -> 1 |
| Sheet | Instant appear (no slide). Opacity 0 -> 1 |
| Screen transition | Cross-fade only. Duration: 100ms (reduced from 300ms) |
| Card entrance | Fade in only. Duration: 100ms (reduced from 350ms) |
| List item stagger | All items appear simultaneously. Duration: 100ms |
| Pull to refresh | Instant status change. No rotation animation |
| Swipe card | Instant dismiss (no gesture tracking) |
| Knowledge node grow | Instant appear (no growth animation) |
| Character idle | Static image (no breathing/blinking) |
| Achievement unlock | Static badge appears. Single text line fades in. Duration: 100ms |
| Confetti | Static sparkle icon (no particle animation) |
| Loading spinner | Static icon (no rotation) |
| Progress bar | Instant fill (no animation) |
| Success/failure | Instant color + icon change. Duration: 0ms |
| Daily reward | Static reward card appears. Text fades in. Duration: 100ms |
| AI thinking | Static typing indicator text only. Duration: 0ms |
| Notification | Instant appear (no slide). Duration: 0ms |
| Tooltip | Instant appear/disappear. Duration: 0ms |

### 19.3 Implementation Requirements

| Requirement | Specification |
|---|---|
| Media query | `@media (prefers-reduced-motion: reduce)` in CSS |
| JavaScript API | `window.matchMedia('(prefers-reduced-motion: reduce)')` |
| In-app toggle | Settings > Accessibility > Reduced Motion (iOS/Android toggle) |
| Storage | Preference persisted in user settings (server-side, synced across devices) |
| Animation library | All animation libraries must support dynamic duration override |
| Duration override | All durations multiplied by system factor. Factor = 0 (no motion) or 0.25 (minimal) |
| Opacity-only fallback | Every animation must have an opacity-only equivalent |
| Information equivalence | All information conveyed by motion must also be available statically |
| Testing requirement | Every screen must be tested and verified with reduced motion enabled |
| WCAG compliance | WCAG 2.1 Success Criterion 2.3.3 (Animation from Interactions) |
| Battery impact | Reduced motion reduces GPU usage by ~70% |

### 19.4 Motion Sensitivity Accommodation

For users with vestibular disorders or motion sensitivity:

| Accommodation | Implementation |
|---|---|
| Disable all parallax | All layered scroll effects removed |
| Disable all scale animations | All scale transitions replaced with opacity |
| Disable all perspective/3D | No rotateY, rotateX, perspective transforms |
| Disable all spring animations | Springs replaced with standard cubic-bezier curves |
| Maximum duration | No animation longer than 200ms |
| Maximum movement | No element moves more than 10px |
| No pulsing animations | Remove all continuous pulse/glow effects |
| Static characters | Character remains in neutral pose (no idle animation) |
| Confirm before celebration | Ask user if they want to see achievement animation |

---

## 20. Performance Budget

### 20.1 Frame Rate Target

| Metric | Target |
|---|---|
| Frame rate | 60fps (consistent) |
| Frame budget | 16ms per frame maximum |
| Acceptable frame drop | 0 dropped frames per animation sequence |
| Long task warning | Any animation task > 50ms flagged for optimization |
| Measured by | Performance tools (Chrome DevTools Performance tab, Safari Web Inspector) |
| Testing requirement | Every animation type tested on target device classes |

### 20.2 GPU-Composited Properties Only

**DO animate (GPU-composited):**
- `transform` (translate, scale, rotate)
- `opacity`

**NEVER animate (causes layout/reflow):**
- `width`, `height`
- `top`, `left`, `right`, `bottom`
- `margin`, `padding`
- `border-width`
- `font-size`
- `line-height`
- `background-position`
- `box-shadow` (use filter: drop-shadow instead)

### 20.3 Will-Change Usage

| Rule | Specification |
|---|---|
| When to use | Only on elements that will animate within the next 200ms |
| Property value | `will-change: transform, opacity` (only properties being animated) |
| Duration | Apply 200ms before animation starts. Remove 200ms after animation ends |
| Overuse warning | Do not apply to more than 3 elements simultaneously |
| Anti-pattern | Never use `will-change: all` or `will-change: auto` |
| Performance cost | Creates new compositing layer (uses GPU memory). Use sparingly |

### 20.4 Animation Complexity Budget

| Metric | Limit |
|---|---|
| Simultaneous heavy animations | Max 3 |
| Simultaneous light animations | Max 10 |
| Heavy animation defined | Particle system, skeleton screen, parallax layer, spring physics calculation |
| Light animation defined | Single element transform, opacity fade, color transition |
| Particle count per system | Max 100 particles per view |
| Spring physics calculations | Pre-calculated where possible (re-use identical spring curves) |
| Compositing layers | Max 15 per viewport |
| Layer creation trigger | Elements with `will-change`, `transform: translateZ(0)`, `<video>`, `<canvas>` |

### 20.5 Spring Physics Optimization

| Optimization | Specification |
|---|---|
| Pre-calculation | Spring curves with fixed tension/friction/mass calculated once, cached, reused |
| Pre-calculated springs | Store as keyframes: 60 keyframes over duration. Use CSS keyframe animation |
| Dynamic springs | Use JavaScript spring for gesture-driven springs only (tension, friction, mass vary) |
| Spring cache | Cache key by tension-friction-mass combination. Max 20 cached springs |
| Spring resolution | 60fps resolution (one keyframe per 16ms interval) |
| Decay threshold | Stop spring simulation when displacement < 0.5px |

### 20.6 Asset Optimization

| Asset Type | Specification |
|---|---|
| Animation assets | SVG preferred for all animated icons (scales without quality loss, small file size) |
| Lottie/Bodymovin | Use for complex character animations. Max file size: 200KB per animation |
| Lottie renderer | Use Canvas renderer for > 5 simultaneous animations. Use SVG renderer for < 5 |
| JSON animation size | Target: < 50KB per animation. Max: 200KB per animation |
| Total animation assets | Max 2MB total for initial load. Lazy load additional animations |
| Reduced mode assets | Load separate reduced-animation assets. Target: 50% file size reduction |

### 20.7 Memory Management

| Practice | Specification |
|---|---|
| Animation cleanup | All animations cancelled on component unmount |
| requestAnimationFrame | All rAF loops must have cleanup (cancel on unmount) |
| Timers | All setTimeout/setInterval for animation must have clear on unmount |
| GPU memory | Compositing layers released when element is off-screen (>100px outside viewport) |
| Particle cleanup | Particle systems capped at max particles. Oldest particles removed first |
| Object pooling | Reuse particle objects instead of creating new instances |

### 20.8 Device-Specific Budgets

| Device Class | GPU Memory | Compositing Layers | Particles | Simultaneous Animations |
|---|---|---|---|---|
| Flagship phone (2024+) | 256MB | 15 | 100 | 3 heavy / 10 light |
| Mid-range phone (2024) | 128MB | 10 | 60 | 2 heavy / 6 light |
| Budget phone (2024) | 64MB | 6 | 30 | 1 heavy / 4 light |
| High-end desktop | 512MB | 30 | 200 | 6 heavy / 20 light |
| Mid-range desktop | 256MB | 20 | 100 | 4 heavy / 12 light |
| Low-end desktop | 128MB | 10 | 50 | 2 heavy / 6 light |
| Tablet (iPad Pro) | 256MB | 15 | 100 | 3 heavy / 10 light |
| Tablet (standard) | 128MB | 10 | 60 | 2 heavy / 6 light |

### 20.9 Performance Monitoring

| Metric | Threshold | Action |
|---|---|---|
| Frame rate | < 55fps for 1 second | Log warning. Reduce non-essential animations |
| Frame rate | < 30fps for 500ms | Log error. Disable heavy animations. Fall back to reduced motion |
| Animation duration | > 16ms (1 frame) per animation | Log optimization opportunity |
| Compositing layers | > 15 per viewport | Log warning. Consolidate layers |
| GPU memory | > 80% of device budget | Log warning. Release off-screen layers |
| JavaScript execution | > 10ms per frame on animation | Log optimization opportunity. Move to CSS animations |
| Layout thrashing | > 3 forced reflows per frame | Log error. Audit animation properties |

### 20.10 Testing Protocol

| Test | Specification |
|---|---|
| Device coverage | Test on top 5 target device classes |
| Frame rate verification | Each animation measured via requestAnimationFrame callback timing |
| Scroll performance | Test with 1000+ list items. Monitor frame rate during scroll + animation |
| Reduced motion | Verify all animations respect reduced motion preference |
| Battery impact | Continuous animation should not exceed 5% battery drain per hour |
| Memory leak | Navigate through all screens. Verify memory returns to baseline after 3 cycles |
| Stress test | Trigger all possible animations within 5 seconds. Monitor for dropped frames |

---

## Appendix A: Cross-Reference Index

| This Document | Related Document | Key Connections |
|---|---|---|
| Section 2 (Animation Principles) | UX_UI_Audit | Audit findings on current animation inconsistency directly addressed by these principles |
| Section 3 (Duration Mapping) | Design System | Duration tokens map to design system timing tokens |
| Section 4 (Easing System) | Design System | Easing curves are design system motion tokens |
| Section 5-6 (Transitions) | Component Library | Navigation components implement these transitions |
| Section 7 (Micro-Interactions) | Component Library | Every micro-interaction maps to a component in the library |
| Section 9 (Learning Mode) | Learning Mode UX | Learning mode UX flows consume these animation specifications |
| Section 10 (Characters) | Illustration Bible | Character animation specifications drive illustration asset requirements |
| Section 11 (Knowledge Tree) | Learning Mode UX | Knowledge tree interaction specifications reference these growth parameters |
| Section 12-13 (Memory/AI) | Learning Mode UX | Memory and AI states reference these animation profiles |
| Section 15-17 (Rewards) | Experience Design Bible | Reward system specifications reference these celebratory sequences |
| Section 19 (Reduced Motion) | Accessibility Bible | Reduced motion profile aligns with accessibility requirements |

## Appendix B: Animation Quick Reference Card

```mermaid
graph TD
    A[Interaction Trigger] --> B{Animation Type?}
    B --> C[Micro (50-200ms)]
    B --> D[Standard (250-400ms)]
    B --> E[Deliberate (400-700ms)]
    B --> F[Celebratory (1000-3000ms)]
    
    C --> G[Button: scale 0.97, 100ms, spring release]
    C --> H[Toggle: 300ms, spring thumb]
    C --> I[Checkbox: 200ms, decelerate draw]
    
    D --> J[Card: 350ms, decelerate entrance]
    D --> K[Toast: 350ms in, decelerate]
    D --> L[Modal: 300ms scale, decelerate]
    
    E --> M[Screen push: 300ms, standard]
    E --> N[Sheet: 400ms slide, decelerate]
    E --> O[Knowledge node: 500ms scale]
    
    F --> P[Achievement: 1500ms, spring burst]
    F --> Q[Dail reward: 2000ms, layered]
    F --> R[Level up: 2500ms, full celebration]
```

## Appendix C: Duration Decision Matrix

| Context | Priority | Duration | Easing | Notes |
|---|---|---|---|---|
| User tapped button | Response | 100ms | Accelerate (press) | Must feel instant |
| User navigated forward | Continuity | 300ms | Standard | Must establish spatial continuity |
| User completed task | Celebration | 1500ms | Spring+Decelerate | Skippable |
| User is waiting | Comfort | Actual+300ms min | Linear | Never artificial extension |
| User made error | Feedback | 300ms | Spring (shake) | Gentle, not punishing |
| User earned reward | Reward | 2000ms | Spring+Decelerate | Progressive reveal, skippable |
| AI is processing | Transparency | Continuous | Spring (dots) | Indicates activity |
| Content is loading | Patience | 1500ms cycles | Linear | Skeleton/shimmer |
| User idle | Atmosphere | Subtle cycles | Standard | Low battery impact |
| User prefers reduced motion | Accessibility | 0-100ms | Opacity only | Respect preferences |
