# Screen Specifications Document

## Version History
| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 1.0 | 2026-07-10 | UX Team | Initial complete screen specifications for all application screens |

---

# Table of Contents

1. [ONBOARDING & AUTH](#onboarding--auth)
   - Splash Screen
   - Welcome Onboarding
   - Language Selection
   - Interest Selection
   - Learning Style Quiz
   - Goal Setting
   - AI Mentor Introduction
   - Login Screen
   - Register Screen
   - Password Reset
   - OTP Verification
2. [HOME](#home)
   - Learner Dashboard
   - Continue Learning
   - Daily Goals & Streaks
   - Recommendations Feed
   - Quick Actions Panel
   - Weekly Summary
3. [LEARNING MODES](#learning-modes)
   - Story Reader (Page View)
   - Story Interactive (Choices)
   - Story Chapter Select
   - Documentary Player
   - Documentary Chapters
   - Podcast Player
   - Podcast Episodes
   - Podcast Playlist
   - Dialogue Conversation
   - Dialogue Character Selection
   - Timeline Viewer
   - Timeline Event Detail
   - Map Explorer
   - Map Location Detail
   - Map Historical Layers
   - Character Profiles
   - Character Relationships
4. [ASSESSMENT](#assessment)
   - Quiz Question Screen
   - Quiz Results
   - Quiz Review & Explain
   - Quiz Leaderboard
   - Flashcard Swipe View
   - Flashcard Deck View
   - Flashcard Creation
   - Memory Review Session
   - Memory Garden View
   - Memory Plant Detail
5. [KNOWLEDGE](#knowledge)
   - Knowledge Tree Overview
   - Knowledge Tree Branch Detail
   - Knowledge Tree Node Detail
   - Knowledge Tree Growth History
   - Knowledge Graph Explorer
   - Knowledge Graph Node Relationships
   - Knowledge Graph Filter & Focus
6. [AI MODES](#ai-modes)
   - AI Teacher Live Session
   - AI Teacher Whiteboard
   - AI Teacher Session History
   - AI Mentor Chat
   - AI Mentor Progress Report
   - AI Mentor Recommendations
   - AI Mentor Goal Tracking
   - AI Chat Conversation
   - AI Chat Context Panel
   - AI Chat Source Citations
7. [PROFILE & ACHIEVEMENTS](#profile--achievements)
   - Avatar Customization Studio
   - Avatar Evolution View
   - Knowledge Spirit Dashboard
   - Knowledge Spirit Evolution
   - Achievements Gallery
   - Achievement Detail
   - Achievement Progress
   - Leaderboards Global
   - Leaderboards Friends
   - Streaks Calendar
   - Streaks History
8. [CONTENT](#content)
   - Search Results Page
   - Search Advanced Filters
   - Search Voice
   - Library Browse
   - Library Categories
   - Library Filter & Sort
   - Bookmarks List
   - Bookmarks Folders
   - Notes Editor
   - Notes Collection View
   - Notes AI-Suggested
   - Collections Manager
   - Curriculum Path Overview
   - Curriculum Node Detail
   - Curriculum Progress Map
9. [ANALYTICS & PROGRESS](#analytics--progress)
   - Progress Analytics Dashboard
   - Progress Heatmap
   - Progress Subject Breakdown
   - Progress Time Analytics
   - Profile Overview
   - Profile Learning Stats
   - Profile Badges
10. [SETTINGS](#settings)
    - Settings Account
    - Settings Preferences
    - Settings Notifications
    - Settings Appearance (Theme)
    - Settings Accessibility
    - Settings Privacy & Data
    - Settings Language & Region
    - Settings Audio & Captions
11. [NOTIFICATIONS](#notifications)
    - Notification Center
    - Notification Preferences
12. [ADMIN/TEACHER/PARENT](#adminteacherparent)
    - Admin Dashboard
    - Admin User Management
    - Admin Content Management
    - Admin Analytics
    - Admin System Health
    - Teacher Dashboard
    - Teacher Student Detail
    - Teacher Class Management
    - Teacher Assignment Builder
    - Teacher Progress Reports
    - Teacher Communication
    - Parent Dashboard
    - Parent Child Progress
    - Parent Activity Feed
    - Parent Controls & Limits
13. [RESEARCH](#research)
    - Research Dashboard
    - Citation Manager
    - Source Library
14. [UTILITIES](#utilities)
    - Whiteboard Canvas
    - Document Viewer
    - CMS Dashboard
    - CMS Content Editor
    - CMS Publishing Workflow
    - Content Moderation Queue
    - Content Moderation Review
    - About/Licenses
    - Help & Support

---

# ONBOARDING & AUTH

## Splash Screen

**Purpose:** Display branded application splash screen on launch while initializing core systems, checking authentication state, and loading cached content.

**User Goal:** Experience smooth application launch with visual feedback that the app is loading.

**Business Goal:** Reinforce brand identity, measure cold-start performance, and prepare backend connections.

**Learning Goal:** Set learner expectations for immersive educational experience.

**Layout Description:** Full-screen centered layout with application logo, animated mascot silhouette, loading indicator, and version number at bottom.

**Sections:**
- Background section with dynamic gradient or animated pattern
- Logo section centered vertically
- Mascot/Spirit character animation region
- Loading progress section at bottom third
- Version and copyright footer

**Widgets Used:**
- AnimatedLogo widget
- LoadingProgressBar widget
- VersionText widget
- MascotAnimation widget
- GradientBackground widget

**Components Used:**
- SplashScreen component
- SplashAnimationController
- SystemInitChecker
- AuthStateResolver
- CacheLoader

**Navigation:**
- Enter: Application cold start or background-to-foreground transition
- Exit: Fade transition to Welcome Onboarding or Login depending on auth state
- Transitions: 1.5-second minimum display time, then crossfade (800ms) to next screen

**Animations:**
- Logo entrance: Scale from 0.8 to 1.0 with 500ms ease-out
- Mascot idle: Subtle floating/bobbing animation (2s loop)
- Loading dots: Sequential dot animation (3 dots cycling)
- Gradient background: Slow color shift animation (8s loop)
- Exit: Crossfade with 800ms duration and slight scale-up

**AI Behavior:**
- AI pre-fetches user model from cache or prepares default onboarding state
- If returning user detected, AI prepares personalized welcome data
- AI may trigger loading of large language models in background

**Loading State:**
- Determinate progress bar (0-100%) reflecting initialization stages
- Stage labels: "Connecting...", "Preparing content...", "Loading your profile...", "Almost ready..."

**Empty State:**
- First-time launch showing default branding with no cached data
- Progress bar moves slower to indicate full initialization

**Error State:**
- If critical system fails, show error screen with retry button
- Non-critical failures allow proceeding with limited functionality
- Timeout after 10 seconds with retry prompt

**Offline State:**
- Detect offline and show cached splash with offline badge
- Loading bar completes using cached data only
- Show "Offline Mode" indicator below logo

**Accessibility Rules:**
- Support VoiceOver/TalkBack on all elements
- Minimum contrast ratio 4.5:1 for all text
- Respect reduced motion setting by disabling animations
- Loading indicator must announce progress via accessibility API
- Logo must have accessible label "Application Logo"
- All animations must pause when accessibility screen reader is active

**Dark Mode Differences:**
- Darker gradient palette (deep blues, purples)
- Logo in lighter shade for contrast
- Loading bar with higher brightness
- Background pattern uses darker base tones

**Light Mode Differences:**
- Brighter gradient palette (light golds, teals)
- Logo in brand standard colors
- Loading bar with brand primary color
- Cleaner minimal background

**Tablet Layout Differences:**
- Larger logo size (40% larger)
- Mascot animation occupies more screen real estate
- Loading indicators scaled proportionally
- Landscape mode shows horizontal layout with mascot left, info right

**Desktop Layout Differences:**
- Centered in viewport with max-width container (800px)
- Keyboard shortcuts visible hint (any key to skip)
- Window resize responsive within constraints
- Larger mascot animation area

**Mobile Layout Differences:**
- Full bleed background
- Compact loading indicators
- Status bar integration
- Bottom-safe-area-aware layout

**Landscape vs Portrait Differences:**
- Portrait: Vertical stacking of logo, mascot, loading
- Landscape: Horizontal layout with mascot on left, loading info on right
- Both maintain aspect ratio of mascot animation

**Responsive Rules:**
- Min-width: 320px, max-width: 1920px
- Logo scales between 60px and 160px
- All measurements use relative units (%, vw, vh)
- Breakpoints at 480px, 768px, 1024px, 1440px
- Touch targets minimum 44px on mobile

---

## Welcome Onboarding

**Purpose:** Introduce first-time users to the application's value proposition, core features, and learning philosophy through a series of visually engaging slides.

**User Goal:** Understand what the app offers and feel motivated to begin the learning journey.

**Business Goal:** Maximize user conversion from download to active learner, reduce abandonment.

**Learning Goal:** Establish the metacognitive framework of the learning system before user engages with content.

**Layout Description:** Full-screen vertical pager with illustrative content area on top (60%), descriptive text in middle (25%), and navigation controls at bottom (15%). Page indicator dots between text and controls.

**Sections:**
- Hero illustration area (top 60%)
- Headline text section
- Descriptive body text section
- Feature highlight with icon row
- Page indicator dots
- Navigation buttons (Skip, Next, Get Started)
- Progress bar across top

**Widgets Used:**
- OnboardingIllustration widget
- FeatureHighlightCard widget
- PageIndicator widget
- PrimaryButton widget
- SkipButton widget
- AnimatedTransition widget
- ParallaxBackground widget

**Components Used:**
- WelcomeOnboarding component
- OnboardingSlide component (x4-5 slides)
- SlideNavigationController
- ParallaxScrollEngine
- FeatureDiscoveryHighlight

**Navigation:**
- Enter: From Splash Screen via crossfade
- Exit: Forward to Language Selection or Interest Selection depending on configuration
- Transitions: Horizontal slide between onboarding pages (300ms cubic-bezier)
- Skip button jumps to last slide then proceeds
- Swipe gesture supported for page navigation
- Back gesture returns to Splash Screen within 2-second window, otherwise closes app

**Animations:**
- Illustration entrance: Scale and fade sequence per page (600ms)
- Text reveal: Staggered type-in animation for headlines (400ms per line)
- Icon row: Sequential slide-in from bottom (200ms stagger)
- Page dots: Active dot pulse animation (1s loop)
- Parallax background: Subtle movement based on device tilt or scroll position
- Button entrance: Fade-in with slight scale (300ms)
- Progress bar: Smooth width transition (400ms)
- Slide transition: Page curl or horizontal slide (300ms)

**AI Behavior:**
- AI analyzes first interaction timing on each slide to determine user's reading speed
- AI notes which illustrations user lingers on for interest inference
- AI prepares adaptive onboarding sequence for future steps
- AI may decide to shorten or lengthen onboarding based on engagement signals
- AI triggers personalized messaging engine for later screens

**Loading State:**
- Illustration lazy-loading with shimmer placeholder
- Text content pre-rendered, no loading delay
- Page transition smooth with pre-cached next slide

**Empty State:**
- N/A - content is static onboarding material always available

**Error State:**
- If illustration fails to load, show text-only version with fallback icon
- Retry illustration load on page re-enter
- Non-blocking error for individual slide assets

**Offline State:**
- All onboarding content is bundled, fully functional offline
- No external asset loading required
- Offline indicator hidden to maintain immersive experience

**Accessibility Rules:**
- All illustrations require detailed alt text
- Swipe navigation must have button alternative
- Screen reader must announce page number and total pages
- Reduced motion: disable parallax and slide animations, use fade transitions
- Font size adjustable via system settings
- VoiceOver rotor support for quick navigation between slides
- Button labels must be descriptive ("Get Started" not "Next")
- Focus management must trap within onboarding on mobile

**Dark Mode Differences:**
- Illustrations use dark-themed variants with glowing accents
- Background: deep navy to dark purple gradient
- Text: white with gold highlights
- Page dots: white with gold active
- Buttons: gold filled on dark background

**Light Mode Differences:**
- Illustrations use light-themed variants with soft colors
- Background: light cream to soft blue gradient
- Text: dark navy with teal highlights
- Page dots: dark gray with teal active
- Buttons: teal filled on light background

**Tablet Layout Differences:**
- Side-by-side layout: illustration left (50%), text right (50%)
- Larger typography hierarchy
- Additional space for feature highlight cards
- Multi-column feature layout in landscape
- Wider button targets

**Desktop Layout Differences:**
- Centered max-width container (900px)
- Higher resolution illustrations
- Mouse hover effects on navigation buttons
- Keyboard arrow key navigation support
- Escape key returns to previous screen
- Window resize responsive with fluid layout

**Mobile Layout Differences:**
- Full-screen vertical stack
- Bottom-sheet style navigation controls
- Thumb-friendly swipe zones
- Compact text with scrollable content area
- Status bar integrated into background

**Landscape vs Portrait Differences:**
- Portrait: Traditional vertical pager layout
- Landscape: Horizontal split with illustration and text side-by-side
- Landscape: Page indicator moves to right side vertically
- Both orientations support full functionality

**Responsive Rules:**
- Breakpoints: 360px, 480px, 768px, 1024px, 1440px
- Illustration scales between 200px and 500px height
- Text container max-width 600px
- Navigation buttons minimum 48px height
- Content padding responsive: 16px mobile, 24px tablet, 32px desktop
- Touch targets minimum 44px on touch devices

---

## Language Selection

**Purpose:** Allow users to select their preferred application language and content languages for learning.

**User Goal:** Choose interface language and target learning languages comfortably.

**Business Goal:** Support global audience expansion, reduce localization friction.

**Learning Goal:** Set up multilingual learning environment based on user's language goals.

**Layout Description:** Two-tier selection layout with interface language at top and learning language(s) below. Search bar for quick language discovery. Visual flags and language names displayed.

**Sections:**
- Header with title and back button
- Interface language selector (single select)
- Learning language selector (multi-select with limits)
- Search/filter bar
- Language list with flags
- Selected languages summary chip row
- Continue button (enabled when selections made)
- Recommended languages section

**Widgets Used:**
- LanguageSelectorTile widget
- LanguageFlagBadge widget
- SearchInput widget
- SelectionChip widget
- SelectedChipRow widget
- PrimaryButton widget
- LanguageRecommendationCard widget

**Components Used:**
- LanguageSelection component
- LanguageListRenderer
- SearchFilterController
- MultiSelectController
- LanguageProficiencyEstimator

**Navigation:**
- Enter: From Welcome Onboarding or Settings
- Exit: Forward to Interest Selection
- Transitions: Slide from right (300ms), back to previous with slide from left
- Back button returns to previous screen
- Continue button validates minimum selection then proceeds

**Animations:**
- Language list: Staggered fade-in (50ms per item)
- Flag icons: Rotate-in animation (400ms)
- Selected chip: Spring-scale entrance (200ms)
- Search results: Smooth list reorder (300ms)
- Continue button: Pulse animation when enabled (1.5s loop)
- Section headers: Fade-in from top (300ms)
- Chip removal: Shrink and fade (200ms)
- Recommendation cards: Slide-in from bottom (400ms stagger)

**AI Behavior:**
- AI predicts user's native language from device settings and location
- AI recommends learning languages based on popularity and user profile
- AI suggests optimal language combinations for polyglot learning
- AI adjusts interface language to match device language by default
- AI stores language preferences to personalize future content recommendations
- AI notes language proficiency levels if user provides them

**Loading State:**
- Language list skeleton loading with shimmer placeholders
- Flag icons lazy-loaded with caching
- Search indexing ready immediately (local data)

**Empty State:**
- No languages match search: "Language not found" with suggestion to browse all
- Empty recommended section hidden entirely
- Zero selections: prompt "Select at least one language"

**Error State:**
- Language data fails to load: show cached or bundled language list
- Retry button on data fetch failure
- Graceful degradation to English-only if data corruption

**Offline State:**
- Language data bundled offline, fully functional
- Flag assets cached locally
- Search operates on local dataset
- No network required for this screen

**Accessibility Rules:**
- Language names read in both native language and English
- Flags require text label alongside visual
- Selected state announced by screen reader ("Arabic selected")
- Search results dynamically announced
- Multi-select state clearly communicated
- Minimum touch target 44px for language items
- Remove chip gesture requires confirmation via accessible action
- Focus order: header, search, interface language, learning language, continue

**Dark Mode Differences:**
- List items: dark surface with white text
- Selected chips: gold border, dark fill
- Search bar: dark input field with light text
- Flags: slight glow effect on borders
- Background: dark gradient

**Light Mode Differences:**
- List items: white surface with dark text
- Selected chips: teal fill, white text
- Search bar: white input field with dark text
- Flags: standard rendering
- Background: light gradient

**Tablet Layout Differences:**
- Side-by-side panes: Interface language left, learning language right
- Larger flag icons (48px vs 32px)
- Multi-column language grid option (3 columns)
- Search bar with inline filter chips
- Additional space for language detail tooltips

**Desktop Layout Differences:**
- Wider layout with max-width 1000px
- Hover preview of language details
- Keyboard navigation: tab between sections, arrow keys in lists
- Scrollable with sticky headers
- Right-click context menu for language options

**Mobile Layout Differences:**
- Full-screen single-column list
- Bottom sheet for additional options
- Compact chip row with horizontal scroll
- Search bar sticky at top
- Swipe to remove language chips

**Landscape vs Portrait Differences:**
- Portrait: Vertical stack of sections
- Landscape: Two-column layout with sections side-by-side
- Landscape: More items visible without scrolling

**Responsive Rules:**
- Breakpoints: 360px, 600px, 840px, 1200px
- Language list item height: 56px mobile, 64px tablet, 72px desktop
- Flag icon sizes: 24px mobile, 32px tablet, 40px desktop
- Grid columns: 1 mobile, 2 tablet, 3 desktop
- Search bar width: full mobile, 400px max desktop
- Chip container max-height with scroll

---

## Interest Selection

**Purpose:** Allow users to select topics and subject areas they are interested in learning about to personalize content recommendations.

**User Goal:** Quickly indicate areas of curiosity and passion to receive tailored content.

**Business Goal:** Collect user interest data for content recommendation engine, increase engagement.

**Learning Goal:** Help users identify and articulate their learning interests across knowledge domains.

**Layout Description:** Grid-based category browser with expandable interest groups. Visual icon-driven tiles with text labels. Selected state clearly indicated.

**Sections:**
- Header with title, subtitle, and progress indicator
- Category grid (categories of interests)
- Selected interests summary at bottom
- Search/filter for specific interests
- "Surprise me" random selection button
- Continue button
- Interest count indicator (e.g., "3 of 10 selected")

**Widgets Used:**
- InterestTile widget
- CategorySectionHeader widget
- SelectedInterestBadge widget
- InterestSearchField widget
- SurpriseMeButton widget
- SelectionCounter widget
- InterestIcon widget
- PrimaryButton widget

**Components Used:**
- InterestSelection component
- InterestGridRenderer
- CategoryAccordionController
- SelectionManager
- InterestRecommendationEngine
- SurpriseMeSelector

**Navigation:**
- Enter: From Language Selection via slide
- Exit: Forward to Learning Style Quiz
- Transitions: Fade and scale for tile selection, slide for page transitions
- Back returns to Language Selection
- Toggle selection with tap, minimum 3 required, maximum vary by configuration

**Animations:**
- Category expansion: Accordion slide-down (300ms)
- Tile selection: Scale bounce and color fill (250ms spring)
- Selected count badge: Number flip animation (200ms)
- Surprise me: Random tile highlight sequence (800ms total)
- Category icons: Entrance stagger (100ms per category)
- Continue button: Enable/disable state transition (200ms)
- Search filter: Real-time tile show/hide (200ms fade)
- Summary chips: Slide up from bottom (400ms)

**AI Behavior:**
- AI recommends interests based on selected language and typical learner profiles
- AI uses collaborative filtering to suggest "learners like you also picked"
- AI dynamically adjusts interest groups based on selections for relevance
- AI predicts learning path based on interest combination
- AI skips this screen for returning users with established profile
- AI stores interest data as weighted vectors for content matching

**Loading State:**
- Category grid skeleton cards (12 shimmer placeholders)
- Icons lazy-loaded with progressive enhancement
- Categories expand on demand with lazy content

**Empty State:**
- No interests match search: "Can't find what you're looking for?" with suggestion form
- No categories configured: emergency fallback to text list
- Zero selected: "Choose at least 3 interests to continue"

**Error State:**
- Interest data fails: use cached interest taxonomy
- Individual icon failure: show text-only tile
- Retry full load option in header

**Offline State:**
- Interest taxonomy bundled offline
- Icon assets precached
- Full functionality without network
- Sync selections when online restored

**Accessibility Rules:**
- Selection state announced: "[Interest Name], selected" or "not selected"
- Category expansions announce expanded/collapsed state
- Grid navigation with arrow keys on desktop
- Touch targets minimum 48px for tiles
- Selected state shown with both color and icon indicator
- Focus indicator clearly visible on all interactive elements
- Screen reader announces selection count and minimum requirement

**Dark Mode Differences:**
- Tiles: Dark surface with colored borders
- Selected tiles: vibrant accent fill with icon
- Categories: Dark expandable headers
- Icons: lighter shades for visibility
- Grid background: spaced repetition patterns

**Light Mode Differences:**
- Tiles: White surface with shadow elevation
- Selected tiles: accent color background
- Categories: Light gray headers
- Icons: brand standard colors
- Clean minimal background

**Tablet Layout Differences:**
- 4-column grid for interest tiles
- Category preview shows first row of tiles
- Larger tile size with description text
- Side panel for selected interests summary
- Hover tooltips on tiles

**Desktop Layout Differences:**
- 5-column grid with max-width 1200px
- Category side navigation for quick jump
- Detailed interest description on hover/focus
- Multi-select using Shift+click range
- Drag to select multiple
- Window resize fluid grid

**Mobile Layout Differences:**
- 2-column grid compact tiles
- Full-width category expansion
- Bottom sheet for selected interests
- Swipe to scroll categories horizontally
- Thumb-friendly selection zones

**Landscape vs Portrait Differences:**
- Portrait: 2-column grid, vertical scroll
- Landscape: 3-4 column grid, more visible items
- Landscape: category header collapsible to save space

**Responsive Rules:**
- Breakpoints: 360px, 480px, 768px, 1024px, 1280px
- Tile size: 100px-160px depending on viewport
- Grid columns: 2 (mobile), 3-4 (tablet), 5 (desktop)
- Category header height: 48px responsive
- Selection counter fixed bottom
- Padding: 16px mobile, 24px tablet, 32px desktop

---

## Learning Style Quiz

**Purpose:** Assess user's preferred learning modalities (visual, auditory, reading/writing, kinesthetic) to optimize content delivery format.

**User Goal:** Get learning content in the format that works best for them.

**Business Goal:** Increase content retention and user satisfaction through personalized delivery.

**Learning Goal:** Help users understand their own learning preferences metacognitively.

**Layout Description:** Card-based quiz with question on top, answer options in middle, progress indicator at bottom. Visual and interactive answer types (not just text).

**Sections:**
- Question progress indicator (current/ total)
- Question card with illustration
- Answer options (4-5 choices with icons)
- Selected answer highlight
- Next/Previous navigation buttons
- Skip question link
- Estimated time remaining

**Widgets Used:**
- QuizQuestionCard widget
- AnswerOptionTile widget
- ProgressDots widget
- QuizNavigationButtons widget
- TimeEstimateLabel widget
- QuizIllustration widget
- SkipLink widget

**Components Used:**
- LearningStyleQuiz component
- QuizQuestionRenderer
- AnswerSelector
- QuizProgressTracker
- LearningStyleCalculator
- AdaptiveQuestionController

**Navigation:**
- Enter: From Interest Selection
- Exit: Forward to Goal Setting
- Transitions: Card flip or slide between questions (350ms)
- Next/Previous buttons within quiz
- Back returns to Interest Selection with confirmation dialog
- Skip moves to next question without recording

**Animations:**
- Question entrance: Card slide-up with fade (400ms)
- Answer selection: Scale and highlight with ripple (200ms)
- Progress dots: Animated fill transition (300ms)
- Card flip: 3D rotation for question change (500ms)
- Illustration transition: Crossfade (300ms)
- Answer feedback: Correct/wrong indicator pulse (600ms)
- Completion: Confetti burst animation (1.5s)
- Skip action: Card dismisses left (250ms)

**AI Behavior:**
- AI adapts subsequent questions based on previous answers (adaptive testing)
- AI analyzes answer patterns to determine learning style with confidence score
- AI may ask fewer questions if learning style is quickly identified
- AI provides learning style result with explanation of what it means
- AI stores learning style vector for content format recommendation
- AI cross-references with actual user behavior over time for refinement
- AI may present contradictory scenarios for more accurate assessment

**Loading State:**
- Question card skeleton with shimmer
- Illustration progressive loading
- Pre-fetch next question in background
- Answer options appear simultaneously after load

**Empty State:**
- N/A - questions are static quiz content

**Error State:**
- Question fails to load: retry individual question
- Progress lost: restart quiz from last answered
- Network error: cached quiz data used if available

**Offline State:**
- Quiz content fully bundled offline
- Results stored locally, sync when online
- Complete functionality without network

**Accessibility Rules:**
- Questions read aloud by screen reader
- Answer options clearly labeled and announced
- Progress announced as "Question 3 of 10"
- Touch targets minimum 48px for answer options
- Color not sole indicator of correct/incorrect
- Keyboard navigation: number keys 1-4 for answers
- Reduced motion: disable card flip, use fade transitions
- Answer confirmation required before proceeding

**Dark Mode Differences:**
- Question cards: dark surface with high contrast text
- Answer options: bordered style with hover fill
- Illustrations: dark-adapted color schemes
- Progress dots: white with gold active
- Background: dark gradient matching theme

**Light Mode Differences:**
- Question cards: white with subtle shadow
- Answer options: filled style with border
- Illustrations: light-adapted color schemes
- Progress dots: gray with teal active
- Background: light gradient matching theme

**Tablet Layout Differences:**
- Wider question cards with more white space
- Answer options in 2x2 grid layout
- Larger illustration area
- Side panel showing learning style profile being built
- Question number visible as progress bar

**Desktop Layout Differences:**
- Centered max-width 800px
- Answer options with hover preview effects
- Keyboard shortcuts (1-4 for answers, Enter to confirm)
- Question timer visible
- Detailed tooltip on answer hover

**Mobile Layout Differences:**
- Full-width cards with minimal padding
- Answer options in vertical list (easier thumb reach)
- Bottom-fixed navigation buttons
- Compact progress indicators
- Swipe between questions

**Landscape vs Portrait Differences:**
- Portrait: Vertical card layout
- Landscape: Illustration beside question for wider format
- Answers may reflow to 2-column in landscape

**Responsive Rules:**
- Breakpoints: 360px, 480px, 768px, 1024px
- Card max-width: 600px mobile, 700px tablet, 800px desktop
- Answer option height: 56px mobile, 64px tablet
- Font sizes: 16px body, 20px question mobile scaling up
- Navigation buttons: 44px minimum touch target
- Illustration size: 120px-250px based on viewport

---

## Goal Setting

**Purpose:** Allow users to define their learning goals, frequency, duration, and commitment level.

**User Goal:** Set realistic and personalized learning targets.

**Business Goal:** Establish habit formation early, increase retention through goal commitment.

**Learning Goal:** Apply SMART goal framework to learning objectives.

**Layout Description:** Multi-step form with goal type selection, commitment frequency, daily time commitment, and goal visualization. Progress bar across top.

**Sections:**
- Step indicator (1-4 steps)
- Goal type selector (exploratory, mastery, casual, academic)
- Learning frequency picker (days per week)
- Daily duration slider (5-120 minutes)
- Goal end date or ongoing toggle
- Goal visualization preview
- Commitment confirmation with animated seal
- Set Goals button

**Widgets Used:**
- GoalTypeCard widget
- FrequencySelector widget
- DurationSlider widget
- GoalVisualizationPreview widget
- CommitmentSeal widget
- StepIndicator widget
- DatePicker widget
- ToggleSwitch widget
- PrimaryButton widget

**Components Used:**
- GoalSetting component
- MultiStepFormController
- GoalCalculator
- SMARTGoalValidator
- CommitmentAnimator

**Navigation:**
- Enter: From Learning Style Quiz
- Exit: Forward to AI Mentor Introduction
- Transitions: Step transitions with slide animation (300ms)
- Back between steps
- Back to Learning Style Quiz from step 1
- Set Goals completes and proceeds

**Animations:**
- Step transition: Horizontal slide content swap (300ms)
- Goal type selection: Card scale and shadow change (200ms)
- Duration slider: Animated thumb and value display (immediate)
- Frequency selector: Dot fill animation (200ms)
- Goal preview: Dynamic update with each change (300ms)
- Commitment seal: Stamp animation with glow (800ms)
- Progress bar: Smooth step indicator fill (400ms)
- Confetti on goal set (1.2s)
- Checkmark animation on each step completion (500ms)

**AI Behavior:**
- AI recommends goal types based on learning style quiz results
- AI suggests realistic frequency based on user's calendar (if permission)
- AI calculates optimal session duration for retention based on learning style
- AI sets milestone reminders based on goals
- AI adapts goals over time based on actual engagement
- AI may suggest adjusting goals if too ambitious or too easy
- AI prepares personalized learning plan based on goals
- AI creates initial content roadmap aligned with goals

**Loading State:**
- Goal templates loading with skeleton cards
- Calendar integration loading indicator (if applicable)
- Goal preview rendering placeholder

**Empty State:**
- No goals set yet: default recommendations shown
- Calendar not available: text input alternatives
- Empty recommendations: show all options unfiltered

**Error State:**
- Goal save fails: data cached locally, retry on next launch
- Calendar access denied: manual input fallback
- Validation error: inline message on relevant field

**Offline State:**
- Full functionality offline
- Goals saved locally and synced when online
- Calendar integration limited without network

**Accessibility Rules:**
- Each step labeled and announced
- Slider accessible via increment/decrement buttons
- Frequency selector requires both visual and text labels
- Goal preview described in detail via screen reader
- Commitment confirmation requires explicit action
- Error messages announced immediately
- Step progress announced as "Step 2 of 4"
- All interactive elements focusable

**Dark Mode Differences:**
- Goal cards: dark surface with accent borders
- Slider: dark track with light active fill
- Frequency dots: dark with gold active
- Step indicator: dark circles with gold step number
- Commitment seal: gold on dark background

**Light Mode Differences:**
- Goal cards: white with shadow elevation
- Slider: light track with teal active fill
- Frequency dots: light with teal active
- Step indicator: light circles with teal step number
- Commitment seal: teal on light background

**Tablet Layout Differences:**
- Two-column layout for goal type selection
- Larger slider with tick marks
- Side preview panel showing goal visualization
- Calendar integration displayed inline
- More detailed goal descriptions

**Desktop Layout Differences:**
- Centered form with max-width 700px
- Step navigation with sidebar
- Drag-and-drop goal prioritization
- Detailed calendar week view for frequency
- Print goal plan option

**Mobile Layout Differences:**
- Full-screen single-step view
- Bottom sheet for date picker
- Simplified goal visualization
- Compact step indicator
- Thumb-optimized slider

**Landscape vs Portrait Differences:**
- Portrait: Vertical step progression
- Landscape: Steps shown as tab bar on left, content on right
- Landscape: preview panel always visible

**Responsive Rules:**
- Breakpoints: 360px, 600px, 840px, 1024px
- Form width: 100% mobile, 600px tablet, 700px desktop
- Slider min-width: 200px
- Button height: 48px minimum
- Padding: 20px mobile, 32px tablet, 40px desktop
- Touch targets: 44px minimum

---

## AI Mentor Introduction

**Purpose:** Introduce the AI mentor character, explain its role, and establish the mentor-learner relationship.

**User Goal:** Meet and connect with their personal AI learning mentor.

**Business Goal:** Create emotional attachment to AI mentor, increase engagement and retention.

**Learning Goal:** Understand how AI mentor supports the learning journey.

**Layout Description:** Full-screen introduction sequence with animated AI mentor character, speech bubbles, and interactive name input. Warm, inviting visual design.

**Sections:**
- Animated AI mentor character (Knowledge Spirit)
- Mentor name display
- Speech bubble with introduction text
- User name input field
- Mentor personalization options (voice, tone)
- "Start Learning" button
- Background with magical/educational theme
- Feature highlights of mentor capabilities

**Widgets Used:**
- MentorCharacterAnimation widget
- SpeechBubble widget
- NameInputField widget
- MentorCustomizationPanel widget
- FeatureHighlightCard widget
- PrimaryButton widget
- ParticleBackground widget

**Components Used:**
- AIMentorIntroduction component
- MentorAnimationController
- IntroductionSequence
- MentorPersonalityEngine
- MentorVoiceSelector

**Navigation:**
- Enter: From Goal Setting
- Exit: Forward to Learner Dashboard
- Transitions: Cinematic zoom-in to character (1s), then proceed to dashboard
- Tap through speech sequence
- Name input auto-advances
- Can revisit from settings

**Animations:**
- Mentor entrance: Character rises from bottom with glow (1.2s)
- Speech bubbles: Sequential appear with typewriter text (500ms per bubble)
- Character idle: Subtle breathing and blink animation (3s loop)
- Name input: Focus glow animation (300ms)
- Feature highlights: Staggered slide-up with icons (400ms stagger)
- Background particles: Floating knowledge symbols (8s loop)
- Voice selection: Waveform preview animation (1s loop)
- Transition to dashboard: Zoom out and fade (800ms)
- Sparkle effects on mentor's magic reveal (600ms)

**AI Behavior:**
- AI mentor introduces itself with personality adapted to user's learning style
- AI generates unique mentor name based on user's language and interests
- AI adapts dialogue style (formal/casual) based on user age and preferences
- AI remembers user's name and uses it consistently
- AI sets initial communication preferences (voice tone, response length)
- AI prepares welcome message tailored to user's goals and interests
- AI establishes first interaction pattern for future mentor sessions
- AI calibrates its personality based on user's reactions (engagement signals)

**Loading State:**
- Character model loading with shimmer silhouette
- Speech content rendering (pre-generated but animated)
- Voice samples loading indicator
- Background particle system initializing

**Empty State:**
- N/A - core introduction always present

**Error State:**
- Character animation fails: static image fallback
- Voice samples unavailable: text-only introduction
- Speech sequence corrupted: skip to end
- Retry animation on error

**Offline State:**
- Mentor character and speech fully bundled offline
- Voice samples cached after first download
- Full introduction experience available offline
- Personalization options limited to cached data

**Accessibility Rules:**
- All speech bubble text available as static text for screen readers
- Character animations described via accessibility API
- Name input properly labeled with autofill support
- Touch to advance speech requires alternative action for keyboard users
- Voice selection: visual indicator alongside audio preview
- Reduced motion: disable character animations, show static panels
- Font size respects system settings
- Speech speed adjustable

**Dark Mode Differences:**
- Background: deep space-like gradient with stars
- Mentor character: warmly lit with golden glow
- Speech bubbles: dark with gold text
- Particles: luminous and bright on dark
- Character shadows: subtle and warm

**Light Mode Differences:**
- Background: light celestial gradient with clouds
- Mentor character: brightly colored with soft glow
- Speech bubbles: white with dark text
- Particles: colorful but subtle
- Character shadows: standard rendering

**Tablet Layout Differences:**
- Mentor character larger (40% of screen height)
- Speech bubbles with more text capacity
- Side panel for mentor customization
- Additional character detail visible
- Background more elaborate

**Desktop Layout Differences:**
- Mentor character centered with environmental background
- Speech bubbles using cinematic letterboxing
- Advanced customization panel always visible
- Character detail at highest resolution
- Ambient sound controls visible

**Mobile Layout Differences:**
- Mentor character sized to fit top half
- Compact speech bubbles
- Full-screen immersive feel
- Swipe to advance speech
- Simplified customization

**Landscape vs Portrait Differences:**
- Portrait: Vertical character-intro-text layout
- Landscape: Character left, speech bubbles right
- Landscape: name input and customization in right panel

**Responsive Rules:**
- Breakpoints: 360px, 600px, 840px, 1024px
- Character height: 30-50% of screen based on viewport
- Speech bubble max-width: 500px
- Name input: 280px max-width
- Font sizes: 16-24px range
- Padding: responsive with min 24px

---

## Login Screen

**Purpose:** Authenticate existing users and grant access to the application.

**User Goal:** Securely and quickly access their learning account.

**Business Goal:** Maintain security while minimizing login friction to reduce abandonment.

**Learning Goal:** N/A - authentication screen.

**Layout Description:** Clean, focused login form with email/username field, password field, remember me checkbox, login button, and links to register and password reset. Social login options below.

**Sections:**
- App logo and branding at top
- Welcome back greeting
- Email/Username input field
- Password input field with visibility toggle
- Remember me checkbox
- Login button (primary action)
- Forgot password link
- Register account link
- Social login options (Google, Apple, Microsoft)
- Biometric login option (if available)
- Divider between email and social login

**Widgets Used:**
- LogoWidget
- TextInputField (email)
- PasswordInputField
- CheckboxWidget
- PrimaryButton
- SocialLoginButton (Google, Apple, Microsoft)
- BiometricLoginButton
- LinkText (forgot password, register)

**Components Used:**
- LoginForm component
- SocialAuthProvider
- BiometricAuthController
- SessionManager
- AuthValidator

**Navigation:**
- Enter: From Splash Screen (if not authenticated), from Logout, from Session Expired
- Exit: Forward to Learner Dashboard (on success), back to Welcome Onboarding
- Transitions: Fade in on enter, slide up on success
- Forgot password link navigates to Password Reset
- Register link navigates to Register Screen
- Social login callbacks redirect accordingly

**Animations:**
- Form entrance: Staggered slide-up of fields (200ms stagger)
- Logo: Subtle pulse animation (3s loop)
- Input focus: Border highlight and label float (200ms)
- Password visibility: Eye icon crossfade (150ms)
- Login button: Loading spinner on submit, success checkmark (1.5s)
- Error shake: Form shake on invalid credentials (400ms)
- Social buttons: Hover scale on desktop (150ms)
- Biometric icon: Thumbprint pulse animation (2s loop)
- Remember me checkbox: Checkmark draw animation (300ms)

**AI Behavior:**
- AI detects suspicious login attempts and triggers additional verification
- AI analyzes login patterns for anomaly detection
- AI pre-loads user profile and preferences upon successful authentication
- AI may suggest account recovery options if repeated failed attempts
- AI adapts login screen based on device trust level
- AI triggers welcome message preparation for authenticated user

**Loading State:**
- Button loading spinner during authentication
- Form fields remain visible but disabled
- Background authentication processes indicated
- Social login loading with provider-specific indicators

**Empty State:**
- N/A - form always has fields

**Error State:**
- Invalid credentials: inline error "Incorrect email or password"
- Account locked: "Account temporarily locked. Try again in X minutes"
- Network error: "Connection failed. Check your internet."
- Too many attempts: progressive delay notification
- Server error: "Something went wrong. Please try again."
- Session expired on token: clear message with re-login prompt
- Each error has specific icon and recovery action

**Offline State:**
- Show offline indicator prominently
- Disable login button with "Connect to internet to sign in"
- Biometric offline login available if previously cached
- Show cached user avatar and name as hint
- "Use offline mode" link if available

**Accessibility Rules:**
- All form fields have associated labels
- Error messages linked to fields via aria-describedby
- Password visibility toggle announced
- Biometric option announced with available status
- Focus management: first field on load, error focus
- Touch targets minimum 48px
- Keyboard navigation: Tab through fields, Enter to submit
- AutoComplete attributes set for all fields
- Screen reader announces login success/failure
- CAPTCHA alternatives for vision impaired users

**Dark Mode Differences:**
- Form surface: dark card elevated from darker background
- Input fields: dark fill with light border on focus
- Social buttons: dark variant with brand icons
- Logo: light version for contrast
- Background: deep gradient

**Light Mode Differences:**
- Form surface: white card with subtle shadow
- Input fields: white fill with gray border on focus
- Social buttons: light variant with brand icons
- Logo: standard brand version
- Background: light gradient

**Tablet Layout Differences:**
- Centered form panel with max-width 450px
- Social login buttons in horizontal row
- Biometric option displayed more prominently
- Background visual decoration on sides
- Larger touch targets

**Desktop Layout Differences:**
- Centered form with max-width 420px
- Background illustration or pattern on full screen
- Hover states on all interactive elements
- Keyboard shortcuts: Enter to submit, Tab to navigate
- CAPTCHA if needed with proper integration
- Password manager integration hints

**Mobile Layout Differences:**
- Full-width form with edge-to-edge inputs
- Bottom sheet keyboard avoidance
- Social login buttons stacked vertically
- Biometric option at bottom
- Status bar integration
- Safe area insets respected

**Landscape vs Portrait Differences:**
- Portrait: Standard vertical form
- Landscape: Scrolling form with keyboard avoidance
- Landscape: logo smaller to fit viewport

**Responsive Rules:**
- Breakpoints: 360px, 480px, 768px, 1024px
- Form width: 100% (mobile), 450px (tablet/desktop)
- Input height: 48px minimum
- Button width: full on mobile, auto on desktop
- Logo size: 48px-80px
- Padding: 24px mobile, 32px tablet, 48px desktop

---

## Register Screen

**Purpose:** Allow new users to create an account and join the learning platform.

**User Goal:** Quickly and easily create an account to start learning.

**Business Goal:** Minimize sign-up friction while collecting necessary information for personalization.

**Learning Goal:** N/A - registration screen.

**Layout Description:** Multi-field registration form with name, email, password, confirm password, and optional fields. Progress indicator for multi-step registration if used.

**Sections:**
- App logo and branding
- Create Account heading
- Full name input
- Email input
- Password input with strength indicator
- Confirm password input
- Optional: age/grade input
- Terms and privacy policy checkboxes
- Create Account button
- Sign In link for existing users
- Social registration options

**Widgets Used:**
- TextInputField
- PasswordInputField (with strength meter)
- PasswordStrengthMeter widget
- CheckboxWidget (terms, privacy)
- PrimaryButton
- SocialLoginButton
- LinkText
- DatePickerWidget (age/dob)
- ProgressStepper (if multi-step)

**Components Used:**
- RegistrationForm component
- PasswordStrengthEvaluator
- AgeValidator
- TermsAgreementController
- SocialAuthProvider

**Navigation:**
- Enter: From Login (register link), from Welcome Onboarding
- Exit: Forward to OTP Verification or directly to Learner Dashboard (depending on config)
- Transitions: Slide from right (300ms)
- Sign In link returns to Login
- Terms link opens Terms modal/page
- Privacy link opens Privacy modal/page

**Animations:**
- Form fields: Staggered entrance slide-up (150ms stagger)
- Password strength: Real-time bar fill and color change (200ms)
- Checkbox: Checkmark draw animation (250ms)
- Password match: Validation indicator appear (200ms)
- Button state: Disabled to enabled transition (200ms)
- Error states: Field shake (350ms)
- Success: Button transforms to checkmark (500ms)
- Social buttons: Hover/scale on desktop (150ms)
- Terms modal: Slide-up bottom sheet (350ms)

**AI Behavior:**
- AI suggests strong passwords if user enters weak one
- AI checks email validity and suggests corrections for typos
- AI pre-creates user learning profile based on registration data
- AI analyzes sign-up pattern for bot detection
- AI prepares personalized onboarding sequence
- AI suggests username based on real name if applicable
- AI checks for duplicate accounts with same email

**Loading State:**
- Button loading spinner on submit
- Email availability checking indicator
- Password strength calculating
- Registration processing overlay

**Empty State:**
- N/A - form fields always displayed

**Error State:**
- Email already registered: "This email is already registered. Sign in instead." with link
- Weak password: specific improvement suggestions
- Passwords don't match: inline error
- Invalid email format: format guidance
- Network error: retry option
- Server error: "Registration temporarily unavailable"
- Age validation: appropriate message for underage
- Terms not accepted: required field error

**Offline State:**
- Registration disabled offline
- Form filled but submission blocked
- "Connect to internet to create account" message
- Cache form data temporarily to avoid re-typing

**Accessibility Rules:**
- All fields have explicit labels
- Password strength announced on change
- Error suggestions provided in text, not just color
- Terms checkbox linked to full text
- Focus order logical through form
- Autofill attributes set (name, email, new-password)
- Minimum touch targets 48px
- Screen reader announces progress for multi-step
- CAPTCHA alternatives where used

**Dark Mode Differences:**
- Form: dark elevated card
- Inputs: dark fill, light border
- Strength meter: red to green on dark background
- Checkboxes: dark with gold check
- Social buttons: dark theme variants

**Light Mode Differences:**
- Form: white card with shadow
- Inputs: white fill, gray border
- Strength meter: red to green standard
- Checkboxes: white with teal check
- Social buttons: light theme variants

**Tablet Layout Differences:**
- Side-by-side fields for name (first/last)
- Password fields side by side with strength panel
- Larger form width with more white space
- Optional fields in collapsible section
- Calendar widget for DOB inline

**Desktop Layout Differences:**
- Two-column layout for related fields
- Password strength panel with detailed tips
- Privacy/terms links open in-page overlay
- Tab navigation with visible focus rings
- Keyboard shortcuts for submission

**Mobile Layout Differences:**
- Single-column stacked layout
- Full-width inputs
- Keyboard-aware scrolling
- Date picker uses native picker
- Compact everything for screen fit

**Landscape vs Portrait Differences:**
- Portrait: scrollable single column
- Landscape: fields may split into two columns
- Landscape: compact view with less whitespace

**Responsive Rules:**
- Breakpoints: 360px, 480px, 768px, 1024px
- Form max-width: 480px mobile, 600px desktop
- Input height: 48px minimum
- Button full width mobile, auto desktop
- Font: 16px minimum for inputs
- Padding: 20px mobile, 32px tablet

---

## Password Reset

**Purpose:** Allow users to reset their forgotten password via email verification.

**User Goal:** Regain access to their account quickly and securely.

**Business Goal:** Reduce support tickets for password recovery, maintain account security.

**Learning Goal:** N/A - password recovery utility.

**Layout Description:** Three-step flow: email input, verification sent confirmation, new password creation. Clean minimal design.

**Sections:**
- Step 1: Email input with logo, heading, email field, send button, back to login link
- Step 2: Success confirmation with check email message, resend link (cooldown), open email app button, OTP alternative
- Step 3: New password screen with password fields, strength meter, reset button, success with login link

**Widgets Used:**
- TextInputField, PasswordInputField, PasswordStrengthMeter, PrimaryButton, SuccessAnimation, CountdownTimer, LinkText, EmailAppLauncher, StepIndicator

**Components Used:**
- PasswordResetFlow, EmailValidator, TokenVerifier, PasswordUpdateController, CooldownManager

**Navigation:**
- Enter: From Login "Forgot Password" link
- Exit Step 1: Back to Login or forward to Step 2
- Exit Step 2: Back resets to Step 1, forward to email app or wait
- Exit Step 3: Success redirects to Login
- Deep link from email opens Step 3 directly
- Token expiry returns to Step 1 with message

**Animations:**
- Step transitions: Content crossfade (400ms)
- Email sent animation with paper plane (800ms)
- Resend countdown: Circular timer animation (1s tick)
- Password strength: Real-time bar update (200ms)
- Success: Checkmark circle draw (600ms)
- Error states: Gentle shake (350ms)
- Step indicator: Animated step fill (300ms per step)

**AI Behavior:**
- AI checks if email exists before sending (privacy-preserving)
- AI detects suspicious reset attempts (brute force protection)
- AI may suggest account recovery options if email not found
- AI generates secure reset tokens with expiry
- AI sends personalized reset email with user name
- AI may offer SMS-based reset if phone number on file

**Loading State:**
- Button shows "Sending..." spinner on Step 1 submit
- Button shows "Resetting..." spinner on Step 3 submit
- Email verification checking indicator
- Token validation loading state

**Empty State:**
- Email field empty on mount
- Resend timer initially hidden

**Error State:**
- Email not found: "No account found with this email" (privacy-preserving)
- Invalid email format: format guidance
- Network error: retry option
- Token expired: "Reset link expired. Request a new one."
- Token invalid: "Invalid reset link."
- Rate limit exceeded: "Too many requests. Try again in X minutes."

**Offline State:**
- Step 1: disabled with "Connect to internet" message
- Step 3: can submit new password if token already validated
- Offline badge visible throughout

**Accessibility Rules:**
- Step progress announced: "Step 1 of 3: Enter email"
- Password strength announced
- Resend timer announced: "Resend available in 30 seconds"
- Error messages announced and linked to fields
- Focus management between steps
- Keyboard: Enter to submit, Escape to go back
- Touch targets minimum 44px

**Dark Mode Differences:**
- Cards: dark elevated surface, dark inputs with light border
- Success animation: green on dark
- Background: dark gradient

**Light Mode Differences:**
- Cards: white with shadow, white inputs with gray border
- Success animation: green standard
- Background: light gradient

**Tablet Layout Differences:**
- Wider form with max-width 480px
- Step 2: email preview mockup shown
- Larger animations and icons

**Desktop Layout Differences:**
- Centered with max-width 440px
- Background pattern or illustration
- Tab navigation with visual indicators

**Mobile Layout Differences:**
- Full-width compact form
- Native email client launch button prominent
- Keyboard-aware layout adjustments

**Landscape vs Portrait Differences:**
- Portrait: Vertical scrollable steps
- Landscape: Compact form with smaller margins

**Responsive Rules:**
- Breakpoints: 360px, 480px, 768px, 1024px
- Form width: 100% mobile, 480px max desktop
- Input height: 48px minimum
- Button height: 48px minimum
- Padding: 24px standard

---

## OTP Verification

**Purpose:** Verify user's identity via one-time password sent to email or phone during registration or sensitive actions.

**User Goal:** Prove ownership of email/phone quickly and continue their process.

**Business Goal:** Prevent fake accounts and ensure contact information validity.

**Learning Goal:** N/A - verification utility.

**Layout Description:** Focused OTP input screen with 4-6 digit input boxes, timer, and verification status. Clean minimal design.

**Sections:**
- App logo/branding
- Verification heading with masked contact info
- OTP input (individual digit boxes)
- Resend link with countdown timer
- Verify button
- Change contact link
- Help/Support link
- Auto-detecting SMS integration notice

**Widgets Used:**
- OTPInputWidget (digit boxes), CountdownTimer, PrimaryButton, LinkText, ContactMaskLabel, AutoReadOTPHandler, ResendButton

**Components Used:**
- OTPVerification, OTPAutoDetector (SMS/callback), OTPTimerController, OTPValidator

**Navigation:**
- Enter: From Registration, Password Reset step 2, or sensitive actions
- Exit: Forward to next registration step or Learner Dashboard
- Transitions: Fade in (300ms), success zoom out (500ms)
- Back returns to previous step with confirmation

**Animations:**
- OTP boxes: Sequential focus animation (100ms per box)
- Digit entry: Box scale on input (150ms)
- Auto-fill: Digits populating in sequence (300ms total)
- Timer: Circular countdown animation (1s tick)
- Resend: Button enable animation (200ms)
- Verification success: Checkmark with confetti (800ms)
- Error: Box shake (400ms)
- Incorrect digit: Red border flash (300ms)

**AI Behavior:**
- AI monitors for brute force OTP attempts and triggers rate limiting
- AI detects suspicious verification patterns
- AI may offer alternative verification methods if OTP fails repeatedly
- AI optimizes delivery method based on user preferences
- AI may use behavioral analysis to reduce OTP challenges for trusted users

**Loading State:**
- OTP input ready immediately
- Verify button shows spinner during validation
- Resend shows timer countdown
- Auto-detection shows "Detecting code..." indicator

**Empty State:**
- All OTP boxes empty on entry
- Cursor in first box ready for input

**Error State:**
- Incorrect OTP: "Invalid code. Try again." with X attempts remaining
- Expired OTP: "Code expired. Request a new one."
- Too many attempts: "Too many attempts. Try again in X minutes."
- Network error: "Could not verify. Check connection."
- Delivery failure: "Failed to send code. Try again."

**Offline State:**
- Cannot request or verify OTP offline
- "Connect to internet to verify" message
- Offline badge displayed

**Accessibility Rules:**
- Each OTP digit box individually labeled
- Auto-fill announced: "Code detected from messages"
- Timer announced: "Code expires in 1 minute 30 seconds"
- Keyboard input: digits auto-advance to next box
- Paste support for entire code
- Touch targets minimum 48px
- Screen reader announces "Enter digit 1 of 6"

**Dark Mode Differences:**
- OTP boxes: dark fill, light text, gold focus border
- Timer: light text on dark, Background: dark gradient

**Light Mode Differences:**
- OTP boxes: white fill, dark text, teal focus border
- Timer: dark text on light, Background: light gradient

**Tablet Layout Differences:**
- Larger OTP boxes (56px vs 44px)
- Keypad visible for non-touch input
- More spacing between digit boxes

**Desktop Layout Differences:**
- OTP boxes larger with spacious layout
- Keyboard number input with auto-focus
- Paste from clipboard support

**Mobile Layout Differences:**
- OTP boxes compact with minimal spacing
- Auto-SMS detection integrated
- Native keyboard numeric input

**Landscape vs Portrait Differences:**
- Portrait: Vertical centered layout
- Landscape: Horizontal layout with OTP on left, info on right

**Responsive Rules:**
- Breakpoints: 360px, 480px, 768px, 1024px
- Digit box size: 44px min, 56px preferred
- Box spacing: 8px mobile, 12px tablet, 16px desktop
- Form max-width: 400px mobile, 480px desktop

---

# HOME

## Learner Dashboard

**Purpose:** Central hub displaying personalized learning overview, quick access to continue learning, daily goals, streaks, recommendations, and progress snapshots.

**User Goal:** Get a comprehensive overview of their learning journey and quick access to resume or discover content.

**Business Goal:** Drive daily engagement, highlight platform value, and encourage content consumption.

**Learning Goal:** Provide metacognitive overview of progress, reinforce learning commitment.

**Layout Description:** Personalized scrollable dashboard with widget-style cards arranged in a flexible grid. Header with avatar, greeting, and notification bell. Content organized by priority.

**Sections:**
- Top header bar: User avatar, personalized greeting, notification bell, settings gear
- Continue Learning card (prominent, top priority)
- Daily Goals & Streaks summary card
- Quick Actions panel (horizontal scroll)
- Recommendations Feed (vertical scroll, infinite)
- Weekly Summary snapshot card
- Knowledge Tree preview card
- Learning Spirit status mini card
- Recent Activity timeline
- Achievement highlights row

**Widgets Used:**
- UserAvatarWidget, GreetingWidget (time-aware), NotificationBadgeWidget, ContinueLearningCard, StreakCounterWidget, DailyGoalProgressWidget, QuickActionChip, RecommendationCard, WeeklySummaryMini, KnowledgeTreePreview, SpiritStatusMini, ActivityTimelineItem, AchievementBadgeRow

**Components Used:**
- LearnerDashboard, DashboardLayoutController, PersonalizedGreetingEngine, ContinueLearningResolver, RecommendationFeedController, DashboardStateManager

**Navigation:**
- Enter: After login/onboarding completion, from deep links, or app foreground
- Exit: Navigate to any content, profile, settings, or specific features
- Transitions: Content cards fade/stagger on load (200ms stagger)
- Pull-to-refresh for content update
- Tap Continue Learning goes to specific content
- Tap Quick Actions opens specific tools
- Tab bar or hamburger menu for major sections

**Animations:**
- Greeting: Slide-down with fade (400ms)
- Cards: Staggered entrance from bottom (100ms stagger per card, max 800ms)
- Continue Learning: Subtle pulse to draw attention (3s loop)
- Streak counter: Flame animation when active (2s loop)
- Progress bars: Smooth fill from 0 to current (800ms)
- Quick Actions: Horizontal scroll snap with card scale (200ms)
- Notifications badge: Bounce on new notification (400ms)
- Background: Gentle gradient shift (10s loop)
- Pull-to-refresh: Spinner animation
- Card hover: Shadow elevation change on desktop (200ms)

**AI Behavior:**
- AI generates personalized greeting using user's name and time of day
- AI prioritizes Continue Learning based on last session, unfinished content, and optimal learning time
- AI selects recommendation cards based on user's interests, history, and learning goals
- AI adapts dashboard layout based on user engagement patterns
- AI predicts which content to promote based on likelihood of engagement
- AI may show contextual nudges based on learning gaps
- AI pre-fetches next likely content for zero-latency navigation

**Loading State:**
- Skeleton screen matching widget layout (8-10 skeleton cards)
- Avatars and images load progressively
- Cards load in priority order: greeting, continue, goals, then others
- Shimmer animation on skeleton cards

**Empty State:**
- First-time user: Welcome card with "Start your first lesson" CTA
- No continue learning: "Begin your journey" with browse content CTA
- No recommendations: "Explore our library" with category links
- No streak: "Start a streak today!" encouragement card
- Empty sections hidden

**Error State:**
- Failed content load: individual card shows error state with retry
- Network error banner at top (non-blocking)
- Profile load error: static fallback greeting
- Recommendations failed: show cached or default recommendations
- Critical failure: full-screen retry with "Something went wrong"

**Offline State:**
- Cached dashboard displayed with "Offline" banner
- Continue Learning shows last cached content
- Recommendations use cached/offline content
- Streak counter shows last known value
- Sync status indicator in header

**Accessibility Rules:**
- Header greeting announced on page load
- Cards navigable by heading levels (h2 for card titles)
- Continue Learning card marked as primary action
- Notification badge count announced
- Touch targets minimum 48px on all tiles
- Reduced motion: disable staggered animations, show all at once
- Screen reader region labels for each section

**Dark Mode Differences:**
- Background: dark navy/charcoal
- Cards: dark surface with subtle borders
- Greeting text: white with gold accent
- Progress bars: bright accent colors
- Streak flame: vibrant orange on dark

**Light Mode Differences:**
- Background: light gray/cream
- Cards: white with shadow elevation
- Greeting text: dark with teal accent
- Progress bars: standard accent colors
- Streak flame: standard orange on white

**Tablet Layout Differences:**
- 2-column grid layout for cards
- Continue Learning spans full width
- Recommendations in 2-column grid
- Side panel for Knowledge Tree preview

**Desktop Layout Differences:**
- 3-column grid for maximum content density
- Continue Learning spans 2 columns
- Fixed left sidebar for quick navigation
- Hover effects and tooltips on all interactive cards
- Resizable widgets (user preference)

**Mobile Layout Differences:**
- Single column full-width layout
- Quick Actions horizontally scrollable
- Compact headers with less padding
- Bottom nav bar for section access
- Thumb-optimized layout

**Landscape vs Portrait Differences:**
- Portrait: Full vertical scroll, single column
- Landscape: 2-column grid, more cards visible

**Responsive Rules:**
- Breakpoints: 360px, 480px, 768px, 1024px, 1280px, 1440px
- Grid columns: 1 (mobile), 2 (tablet), 3 (desktop small), 4 (desktop large)
- Card min-width: 280px, Card max-width: 500px
- Gutter: 12px mobile, 16px tablet, 24px desktop
- Header height: 56px mobile, 64px tablet, 72px desktop
- Touch targets: 44px minimum all devices

---

## Continue Learning

**Purpose:** Provide immediate one-tap access to resume the user's most recent or most important unfinished learning content.

**User Goal:** Effortlessly pick up where they left off without searching.

**Business Goal:** Reduce friction to re-engage, increase daily session starts.

**Learning Goal:** Maintain learning continuity and reduce forgetting curve effects.

**Layout Description:** Prominent card or panel at the top of the dashboard showing current content, progress, and quick resume action. May show multiple items in a carousel.

**Sections:**
- Primary continue card: Content thumbnail/cover, title and type badge, progress bar with percentage, last position indicator, time remaining estimate, large "Continue" button
- Secondary items (if multiple): horizontal scrollable list
- Session context reminder (brief synopsis)

**Widgets Used:**
- ContinueLearningHero, ContentThumbnail, ContentTypeBadge, ProgressBar, PositionIndicator, TimeRemainingLabel, PrimaryButton (Continue), HorizontalContentStrip

**Components Used:**
- ContinueLearning, ContentResumeResolver, ProgressCalculator, SessionContextBuilder, ContentQueueManager

**Navigation:**
- Enter: Auto-appears on Dashboard, also accessible via "Continue" in header
- Exit: Tap Continue card navigates directly into content at saved position
- Transitions: Card tap expands then fades to content screen (400ms)
- Swipe carousel for multiple items
- Long press for context menu

**Animations:**
- Card entrance: Scale and fade from top (400ms)
- Progress bar: Animated fill on load (600ms)
- Continue button: Pulse animation (3s loop)
- Carousel scroll: Snap with momentum (300ms)
- Transition to content: Zoom and fade (400ms)
- Context text: Typewriter reveal (500ms)

**AI Behavior:**
- AI determines optimal resume point based on session history and content type
- AI provides brief recall prompt before resuming (2-sentence summary)
- AI predicts if user needs review before continuing
- AI may suggest branching paths at decision points
- AI pre-loads content for instant resume

**Loading State:**
- Card skeleton with thumbnail placeholder and shimmer
- Progress bar skeleton, Text line placeholders
- Pre-fetch initiated immediately on dashboard load

**Empty State:**
- New user: "Start your first lesson" with large CTA
- All content completed: "Great job! Find something new." with browse link

**Error State:**
- Content unavailable: "This content is no longer available" with browse new
- Progress sync error: last known progress displayed
- Thumbnail failed: gradient placeholder with title

**Offline State:**
- Cached content resume available
- Streaming content shows "Download to continue offline"
- Progress saved locally, sync on reconnect

**Accessibility Rules:**
- Continue button labeled with content title and position
- Progress announced: "45% complete, Chapter 3"
- Carousel items accessible via swipe and buttons
- Touch target minimum 48px for continue button

**Dark Mode Differences:**
- Card: dark elevated surface
- Thumbnail: dark gradient overlay
- Progress bar: bright accent, Text: white

**Light Mode Differences:**
- Card: white with shadow
- Thumbnail: light overlay
- Progress bar: standard accent, Text: dark

**Tablet Layout Differences:**
- Hero card takes 60% width
- Secondary items in 2-column grid below
- Side information panel

**Desktop Layout Differences:**
- Hero card with high-res thumbnail
- Multiple items in a row (3-4)
- Drag to reorder priority

**Mobile Layout Differences:**
- Hero card full width
- Compact secondary items row
- Swipe primary carousel

**Landscape vs Portrait Differences:**
- Portrait: Vertical stack
- Landscape: Hero card beside secondary items

**Responsive Rules:**
- Breakpoints: 360px, 600px, 840px, 1024px
- Hero card height: 160-280px responsive
- Thumbnail: 80-160px
- Secondary item width: 140-200px

---

## Daily Goals & Streaks

**Purpose:** Display user's daily learning goals, progress toward completion, and current streak count to motivate consistent engagement.

**User Goal:** Track daily learning commitments and maintain motivation streaks.

**Business Goal:** Drive daily active usage through habit formation mechanics.

**Learning Goal:** Build consistent learning habits and understand the power of daily practice.

**Layout Description:** Card or panel combining a circular progress ring for daily goal, streak counter with flame icon, and goal customization controls.

**Sections:**
- Daily goal progress ring (circular, percentage)
- Goal metric (minutes, lessons, or XP)
- Current streak counter with flame icon
- Best streak record
- Goal customization button (pencil icon)
- Weekly overview mini-calendar showing last 7 days
- Milestone progress (e.g., "3 days until 7-day streak")
- Motivation message from AI mentor
- XP earned today counter

**Widgets Used:**
- CircularProgressRing, StreakCounter (with flame animation), StreakCalendarMini, GoalMetricDisplay, IconButton (edit goal), MotivationMessage, XPCounter, MilestoneProgress

**Components Used:**
- DailyGoalsStreaks, StreakCalculator, GoalProgressTracker, GoalCustomizationSheet, MilestoneDetector

**Navigation:**
- Enter: Dashboard card, or full-screen view from card tap
- Exit: Tap outside or back button from full view
- Transitions: Card expands to full view (400ms)
- Edit goal: Opens bottom sheet/modal
- Streak calendar tap: Opens streak history

**Animations:**
- Progress ring: Animated fill from 0 to current (1s, easing)
- Ring color change: Gradual from gray to green (1s)
- Streak flame: Particle fire animation when active (2s loop)
- Flame size increase with streak length (dynamic)
- Number counter: Animated number roll for XP/percentage (500ms)
- Weekly dots: Sequential fill animation (200ms per day)
- Goal complete: Confetti burst (1.2s)
- Streak milestone: Special animation on 7/30/100 days (2s)

**AI Behavior:**
- AI sets initial daily goal based on user's Goal Setting input
- AI dynamically adjusts goal difficulty based on user's historical achievement rate
- AI predicts days when user might break streak and sends motivational push notification
- AI celebrates milestones with personalized messages
- AI may reduce goal on days user has low energy signals
- AI generates streak recovery plan if streak is broken

**Loading State:**
- Circular ring skeleton with shimmer
- Streak counter skeleton number
- Calendar dots loading placeholder

**Empty State:**
- No goal set: "Set your first daily goal" with CTA
- No streak started: "Start today!" with begin lesson CTA
- Zero progress: gray ring, "Make your first move"

**Error State:**
- Streak sync error: last cached streak shown
- Goal save error: revert to previous goal
- Non-blocking error banner

**Offline State:**
- Streak continues counting locally
- Goal progress tracked offline
- Sync when online with conflict resolution

**Accessibility Rules:**
- Progress ring value announced: "Daily goal: 65% complete"
- Streak counter: "Day streak: 5 days"
- Calendar days announced with completion status
- Color not sole indicator (text labels accompany)

**Dark Mode Differences:**
- Ring background: dark circle, bright progress fill
- Flame: vibrant orange/yellow on dark
- Calendar: dark cells with bright filled dots

**Light Mode Differences:**
- Ring background: light gray circle, teal progress fill
- Flame: standard orange on white
- Calendar: white cells with teal filled dots

**Tablet Layout Differences:**
- Larger circular progress ring
- Weekly calendar displayed prominently
- Goal metrics side by side

**Desktop Layout Differences:**
- Dashboard panel with ring, metrics, calendar all visible
- Goal customization in place
- Drag ring to set goal (interactive)

**Mobile Layout Differences:**
- Compact card on dashboard
- Ring sized to fit card
- Calendar dots minimal
- Edit goal opens bottom sheet

**Landscape vs Portrait Differences:**
- Portrait: Vertical stack of elements
- Landscape: Ring and metrics side by side, calendar below

**Responsive Rules:**
- Breakpoints: 360px, 600px, 840px, 1024px
- Ring size: 80-160px responsive
- Flame icon: 24-48px
- Calendar cell: 24-40px

---

## Recommendations Feed

**Purpose:** Display a personalized, infinite-scrolling feed of content recommendations tailored to the user's interests, history, and learning goals.

**User Goal:** Discover new learning content that is relevant and engaging.

**Business Goal:** Increase content consumption, cross-pollinate between content types, and surface under-explored content.

**Learning Goal:** Broaden knowledge horizons through diverse content discovery.

**Layout Description:** Vertical infinite-scrolling feed with content cards of varying sizes. Cards adapt layout based on content type and priority. Pull-to-refresh at top, load more at bottom.

**Sections:**
- Feed header with title and filter/sort options
- "Because you liked..." row (horizontal scroll of similar content)
- "New this week" section divider
- Content cards (mixed sizes: small, medium, large/hero)
- "Trending in your areas" section
- "Pick up where you left" section
- Sponsored/recommended by AI mentor section
- Load more indicator at bottom

**Widgets Used:**
- RecommendationCard widget (variants: hero, standard, compact), ContentThumbnail, ContentTypeBadge, ProgressIndicator, SaveButton, LikeButton, ShareButton, SectionDivider, FilterChipRow, LoadMoreSpinner

**Components Used:**
- RecommendationsFeed, FeedLayoutController, ContentRecommendationEngine, FeedPersonalizer, InfiniteScrollManager, FeedFilterController

**Navigation:**
- Enter: From Dashboard feed section, or from bottom nav "Discover"
- Exit: Tap card to content, tap header to search
- Transitions: Card entrance stagger, content navigation slide
- Pull-to-refresh updates feed
- Endless scroll loads more (pagination: 20 items)

**Animations:**
- Card entrance: Staggered fade-up (80ms stagger per card, 1s total)
- Like button: Heart pop animation (300ms)
- Save button: Bookmark fold animation (300ms)
- Horizontal row: Scroll snap (250ms)
- Filter chips: Selection animation (200ms)
- Pull-to-refresh: Spinner with brand colors
- Load more: Infinite dots at bottom
- Card dismiss: Swipe-to-dismiss with fade (250ms)

**AI Behavior:**
- AI generates recommendations using collaborative filtering, content-based filtering, and knowledge graph traversal
- AI mixes content types (story, quiz, podcast, documentary) for variety
- AI considers recency, diversity, and serendipity in recommendations
- AI personalizes card order based on predicted engagement probability
- AI continuously learns from user's feed interactions (likes, saves, skips, dwell time)
- AI may insert contextual recommendations based on current streak or goals
- AI uses contextual bandits for explore-exploit trade-off

**Loading State:**
- Skeleton cards: 6-8 placeholders matching card types
- Shimmer animation on all skeleton cards
- Progressive loading: hero cards first, then standard, then compact
- Load more: spinner at bottom, new cards appended

**Empty State:**
- No recommendations: "We're finding content for you" with loading CTA
- Filter returns none: "No results. Try different filters."
- New user: "Start learning to get recommendations" with browse CTA

**Error State:**
- Feed load failed: cached feed displayed with "Updated X ago"
- Individual card failed: placeholder with retry
- Network error: banner "Connect to see new recommendations"

**Offline State:**
- Cached recommendations displayed
- "Offline recommendations" label
- New content unavailable, show cached suggestions

**Accessibility Rules:**
- Feed items navigable via heading structure
- Card content announced: "[Title], [Type], [Duration]"
- Like/save states announced
- Horizontal rows accessible via arrow keys and buttons
- Touch targets minimum 44px

**Dark Mode Differences:**
- Cards: dark surface with subtle borders
- Thumbnails: dark overlay with bright text
- Type badges: bright accent colors

**Light Mode Differences:**
- Cards: white with shadow
- Thumbnails: standard overlay
- Type badges: brand accent colors

**Tablet Layout Differences:**
- 2-column card grid
- Hero card spans full width
- Side filter panel

**Desktop Layout Differences:**
- 3-column card grid
- Hero card spans 2 columns
- Sticky filter bar at top
- Right sidebar for trending/popular
- Masonry layout for varied card sizes

**Mobile Layout Differences:**
- Single column card list
- Horizontal rows with snap
- Sticky top filter
- Swipe to dismiss cards

**Landscape vs Portrait Differences:**
- Portrait: Single column with horizontal rows
- Landscape: 2-column grid, more visible cards

**Responsive Rules:**
- Breakpoints: 360px, 600px, 840px, 1024px, 1280px
- Grid columns: 1 (mobile), 2 (tablet), 3 (desktop)
- Card aspect ratios: 16:9 hero, 1:1 standard, 4:3 compact
- Gutter: 12px mobile, 16px tablet, 24px desktop

---

## Quick Actions Panel

**Purpose:** Provide instant access to frequently used tools and actions without navigating through menus.

**User Goal:** Quickly start common tasks like search, bookmarks, notes, or a new learning session.

**Business Goal:** Reduce time-to-action, increase feature discovery and usage.

**Learning Goal:** N/A - utility navigation panel.

**Layout Description:** Horizontal or vertical scrollable row of icon-and-label action chips placed prominently on the dashboard.

**Sections:**
- Action chips row (horizontal scroll on mobile, full grid on desktop)
- Each chip: icon + short label
- Optional: recently used actions sorted first
- "More" action to expand full grid

**Widgets Used:**
- QuickActionChip, IconWidget, LabelWidget, MoreActionButton

**Components Used:**
- QuickActionsPanel, ActionUsageTracker, ActionDynamicOrderer

**Navigation:**
- Enter: Visible on dashboard and relevant screens
- Exit: Tapping an action navigates to target screen
- Horizontal scroll on mobile
- Expandable grid on larger screens

**Animations:**
- Chip entrance: Staggered slide-up (80ms stagger)
- Chip hover: Scale 1.05 on desktop (100ms)
- Chip press: Ripple effect (200ms)
- Scroll: Momentum with snap
- More expand: Grid animation (300ms)

**AI Behavior:**
- AI learns which actions user frequents and reorders priority
- AI may suggest contextual actions based on time/day
- AI highlights rarely used features to encourage discovery

**Loading State:**
- Chip skeletons (8 rounded rectangles with icon placeholders)
- Icons load progressively

**Empty State:**
- Always has default actions (Search, Bookmarks, Notes, Library)

**Error State:**
- Individual action disabled if related feature unavailable
- Grayed out with tooltip explanation

**Offline State:**
- Actions that require online (Search) show offline variant
- Local actions (Notes, Bookmarks) fully functional

**Accessibility Rules:**
- Each chip labeled with action name and icon
- Touch target minimum 44px
- Horizontal scroll requires button alternatives
- Focus ring visible on selected chip

**Dark Mode Differences:**
- Chips: dark surface with bright icons
- Text: white

**Light Mode Differences:**
- Chips: light gray surface with dark icons
- Text: dark

**Tablet Layout Differences:**
- 2-row grid of action chips
- More chips visible (8-10)
- Larger chip size with more text

**Desktop Layout Differences:**
- Full grid of all actions (2-3 rows)
- Tooltip on hover with shortcut keys
- Customizable (drag to reorder)

**Mobile Layout Differences:**
- Single row horizontal scroll
- 4-5 chips visible at a time
- Compact chip size

**Landscape vs Portrait Differences:**
- Portrait: Horizontal scroll row
- Landscape: 2-row grid, more visible

**Responsive Rules:**
- Breakpoints: 360px, 480px, 768px, 1024px
- Chip height: 40-56px responsive
- Icon size: 20-28px
- Gap: 8-12px between chips

---

## Weekly Summary

**Purpose:** Provide a concise weekly overview of learning activity, achievements, and progress to encourage reflection and planning.

**User Goal:** Review their week's learning accomplishments and plan next week.

**Business Goal:** Increase retention through regular progress reflection, drive weekly return visits.

**Learning Goal:** Encourage metacognitive reflection on learning patterns and outcomes.

**Layout Description:** Card or full-page summary with statistics, achievements, and highlights from the past 7 days.

**Sections:**
- Week label (e.g., "Week of July 6 - July 12")
- Total learning time, Lessons completed, Topics covered list
- Streak status for the week, XP earned total
- Achievements unlocked, Knowledge tree growth summary
- Most engaged content type, Comparison with previous week
- AI mentor weekly message, Next week's goals preview
- Share summary button

**Widgets Used:**
- WeeklySummaryCard, StatHighlight, TopicChipRow, AchievementMiniList, ComparisonArrow, MentorMessageWidget, ShareButton, GoalPreviewCard

**Components Used:**
- WeeklySummary, WeeklyStatsCalculator, AchievementDetector, WeeklyComparisonEngine, MentorWeeklyMessageGenerator

**Navigation:**
- Enter: From Dashboard "Weekly Summary" card, or from Profile/Analytics
- Exit: Back to previous screen
- Transitions: Card expands to full page (400ms)
- Share button opens system share sheet

**Animations:**
- Card entrance: Stats count-up animation (1.2s total)
- Numbers: Animated counter from 0 to final (800ms)
- Streak days: Green dot sequential fill (200ms per day)
- Achievements: Staggered reveal (100ms per item)
- Comparison: Arrow animation with color (400ms)
- Mentor message: Fade-in with character (500ms)
- Confetti if exceptional week (1.5s)

**AI Behavior:**
- AI generates personalized weekly summary narrative
- AI highlights most significant learning moments
- AI compares current week with previous and predicts trends
- AI provides encouraging feedback based on performance
- AI suggests focus areas for next week
- AI identifies patterns (e.g., "You learn best in the evening")

**Loading State:**
- Summary skeleton with stat placeholders
- Circular stat skeletons
- Chart skeletons
- Mentor message skeleton (text lines)

**Empty State:**
- First week: "Welcome! This will be your first weekly summary."
- No activity: "Start learning to see your weekly summary"

**Error State:**
- Stats calculation error: cached data shown
- Achievement sync error: show known achievements
- Non-critical: banner "Some data may be incomplete"

**Offline State:**
- Cached weekly summary displayed
- "Last updated: [time]" indicator
- New data syncs when online

**Accessibility Rules:**
- All statistics have text labels accompanying numbers
- Charts have data table alternatives
- Comparison announced: "25% more than last week"
- Reduced motion: disable count-up, show final values

**Dark Mode Differences:**
- Card: dark surface with light text
- Stats: white with gold accents
- Comparison arrows: green/red bright
- Charts: dark background, bright data

**Light Mode Differences:**
- Card: white with shadow
- Stats: dark with teal accents
- Comparison arrows: standard green/red
- Charts: light background, colored data

**Tablet Layout Differences:**
- Two-column stat layout
- Larger typography hierarchy
- Mentor message with character avatar larger

**Desktop Layout Differences:**
- Full-page dashboard view
- Detailed charts and graphs
- Week selector for historical comparison
- Print or export summary option

**Mobile Layout Differences:**
- Scrollable single-column layout
- Compact stat cards
- Chart simplified
- Swipe between weeks

**Landscape vs Portrait Differences:**
- Portrait: Vertical scrolling summary
- Landscape: Side-by-side stat sections

**Responsive Rules:**
- Breakpoints: 360px, 600px, 840px, 1024px
- Stat font: 24-48px depending on importance
- Chart height: 150-300px
- Padding: 16-24px

---

# LEARNING MODES

## Story Reader (Page View)

**Purpose:** Immersive page-by-page reading experience for narrative educational content with optional narration, highlighting, and interaction.

**User Goal:** Read educational stories with an engaging, book-like experience.

**Business Goal:** Deliver narrative-based learning content with high engagement.

**Learning Goal:** Comprehend and retain narrative educational content through focused reading.

**Layout Description:** Book-like page layout with text on left/center, optional illustrations on right/top. Page curl transitions. Header with chapter info, footer with page controls.

**Sections:**
- Top bar: Chapter title, page number, close button, settings (font, theme)
- Reading area: Text content with optional illustration
- Bottom bar: Page navigation (previous/next), progress indicator, narration controls
- Highlight/annotation mode overlay
- Dictionary tooltip on long press
- Illustration viewer (tap to expand)
- Font size/theme controls panel

**Widgets Used:**
- PageView, TextContentRenderer, PageIllustration, PageNavigationControls, NarrationControl (play/pause/skip), FontSettingsPanel, HighlightTool, DictionaryPopup, AnnotationMarker, ProgressBarWidget

**Components Used:**
- StoryReader, PageLayoutController, NarrationEngine, AnnotationManager, FontThemeController, PageCurlAnimationController

**Navigation:**
- Enter: From Story Chapter Select, Continue Learning, or recommendations
- Exit: Close button to Chapter Select, back gesture to previous screen
- Transitions: Page curl (300ms) or slide (200ms) for page turns
- Tap left/right zones for page navigation
- Swipe for page turn
- Pinch to zoom illustration

**Animations:**
- Page entrance: Book opening animation (500ms)
- Page turn: Page curl 3D animation (400ms) or slide (250ms)
- Narration highlight: Word-level highlighting synchronized with audio
- Illustration fade-in: On page load (300ms)
- Dictionary popup: Scale and fade (200ms)
- Highlight: Marker draw animation (200ms)
- Font change: Text reflow animation (300ms)
- Book close on exit: Reverse open animation (400ms)

**AI Behavior:**
- AI narrates text with natural-sounding voice synchronized to word highlighting
- AI provides real-time translations for unfamiliar words
- AI summarizes previous pages when user returns
- AI may insert comprehension checks at natural breaks
- AI adapts reading speed to user's pace
- AI answers questions via text selection context menu
- AI tracks reading patterns for comprehension analysis

**Loading State:**
- Page content skeleton with text line shimmer
- Illustration placeholder with loading indicator
- Narration audio buffering indicator
- Page pre-loading (next/previous pages cached)

**Error State:**
- Page content failed: retry with "Page failed to load"
- Illustration failed: text-only layout
- Narration failed: text-only mode with error icon
- Page corrupted: skip to next available page

**Offline State:**
- Downloaded books fully functional offline
- Streaming books: show "Download for offline" prompt
- Narration: cached audio plays offline

**Accessibility Rules:**
- All text content available to screen reader
- Page turn works with swipe and buttons
- Font size adjustable up to 200%
- High contrast mode support
- Touch zones: left/right 30% for page turns
- Keyboard: Arrow keys for page turns
- Reduce motion: disable page curl, use slide
- VoiceOver rotor: navigate by paragraph, page, or chapter
- Closed captions for narration audio

**Dark Mode Differences:**
- Page background: dark cream/sepia
- Text: light color for reduced eye strain
- Illustrations: adapted for dark background
- Narration highlight: bright underline

**Light Mode Differences:**
- Page background: light cream/white
- Text: dark for reading comfort
- Illustrations: standard color
- Narration highlight: colored underline

**Tablet Layout Differences:**
- Two-page view (book spread) in landscape
- Single page in portrait
- Larger illustration alongside text
- Dictionary panel as side sheet

**Desktop Layout Differences:**
- Resizable reading pane with side panels
- Full book spread view optional
- Keyboard shortcuts (Space, arrows)
- Mouse wheel page turn
- Right-click annotation menu

**Mobile Layout Differences:**
- Single page full screen
- Tap zones: left/right thirds for navigation
- Minimal chrome, auto-hide controls
- Swipe to turn pages

**Landscape vs Portrait Differences:**
- Portrait: Single page, vertical text flow
- Landscape: Book spread (tablet), wider text area

**Responsive Rules:**
- Breakpoints: 360px, 480px, 768px, 1024px, 1280px
- Text column width: 400-700px optimal
- Font size: 16-24px body text
- Line height: 1.5-1.8
- Tap zones: minimum 60px width for navigation

---

## Story Interactive (Choices)

**Purpose:** Branching narrative experience where users make choices that affect story outcomes, combining reading with interactive decision-making.

**User Goal:** Engage with interactive stories where their choices matter.

**Business Goal:** Increase engagement through agency and replayability.

**Learning Goal:** Understand cause-effect relationships and consequences of decisions.

**Layout Description:** Story page with narrative text, followed by choice cards at the bottom. Choices have consequence previews. Branching paths shown in background.

**Sections:**
- Narrative text area (top 60%)
- Interactive illustration or scene art
- Choice cards (2-4 options) at bottom
- Choice consequence preview (on hover/tap-hold)
- Decision timer (optional, for urgency)
- Branch path indicator
- Undo last choice button (if enabled)
- Story state tracker (ending %, paths discovered)

**Widgets Used:**
- StoryNarrativeText, SceneIllustration, ChoiceCard (with consequence preview), DecisionTimer, BranchPathIndicator, ChoiceHistoryWidget, UndoButton, DiscoveryTracker

**Components Used:**
- StoryInteractive, ChoiceRenderer, BranchingEngine, ConsequencePreviewGenerator, StoryStateManager, ChoiceTimerController

**Navigation:**
- Enter: From Story Chapter Select (interactive variant)
- Exit: Close button, story completion, or back gesture
- Transitions: Choice selection animates to next scene (500ms)
- Swipe up to scroll narrative text
- Tap choice card to proceed
- Back within story returns to previous choice (undo)

**Animations:**
- Scene entrance: Fade-in with slight scale (500ms)
- Narrative text: Typewriter reveal (adjustable speed)
- Choice cards: Slide-up from bottom (400ms stagger)
- Choice selection: Selected card scales up, others fade (300ms)
- Consequence preview: Tooltip fade on long press (200ms)
- Branch path: Animated tree expansion (600ms)
- Decision timer: Circular countdown (real-time)
- Scene transition: Dramatic wipe or fade (600ms)
- Story completion: Path visualization with ending reveal (800ms)

**AI Behavior:**
- AI generates dynamic consequence text based on user's choice
- AI adapts narrative complexity based on reading level
- AI tracks user's decision patterns for psychological insights
- AI may offer hints if user takes too long to decide
- AI generates personalized epilogue based on all choices made
- AI recommends alternative paths for replay
- AI provides moral/educational reflection on choices

**Loading State:**
- Scene background loading with gradient placeholder
- Choice cards skeleton (2-4 cards)
- Pre-load of next scene paths

**Error State:**
- Branch path load error: fallback to linear continuation
- Choice data corruption: skip to next valid choice
- Illustration failure: text-only scene

**Offline State:**
- Downloaded interactive stories fully functional
- Branching decisions processed locally
- Story state saved locally

**Accessibility Rules:**
- Choices clearly labeled with text and icons
- Consequence preview text available to screen reader
- Timer includes non-visual indication (sound/vibration)
- Touch targets minimum 48px for choices
- Keyboard: number keys 1-4 for choices
- Decision timer optional for accessibility

**Dark Mode Differences:**
- Scene art: dark-adapted color palette
- Choice cards: dark surface, bright border on hover
- Branch paths: dark background, bright lines

**Light Mode Differences:**
- Scene art: standard color palette
- Choice cards: white surface, shadow on hover
- Branch paths: light background, colored lines

**Tablet Layout Differences:**
- Narrative text and illustration side by side
- Choice cards in 2x2 grid
- Branch path tree visible on side panel

**Desktop Layout Differences:**
- Narrative and illustration in flexible layout
- Choice cards with hover effects and descriptions
- Branch path tree fully visible
- Keyboard shortcuts for all actions

**Mobile Layout Differences:**
- Full-screen narrative with illustration above
- Choice cards stacked vertically
- Compact branch indicator
- Thumb-friendly choice buttons at bottom

**Landscape vs Portrait Differences:**
- Portrait: Narrative top, choices bottom
- Landscape: Narrative left, choices right panel

**Responsive Rules:**
- Breakpoints: 360px, 600px, 840px, 1024px
- Text area: 40-60% of viewport height
- Choice cards: min height 64px
- Font sizes: 16-20px narrative

---

## Story Chapter Select

**Purpose:** Allow users to browse, select, and navigate between chapters of educational stories.

**User Goal:** Choose which chapter to read and track reading progress.

**Business Goal:** Organize story content into digestible chapters, encourage completion.

**Learning Goal:** Understand narrative structure and chapter progression.

**Layout Description:** Grid or list of chapter cards showing title, illustration, progress, and lock/unlock status.

**Sections:**
- Story title and cover art at top
- Chapter count and overall progress bar
- Chapter list/grid with: chapter number and title, thumbnail illustration, progress indicator, lock/unlock icon, duration estimate, interactive/branching badge
- Continue button (resumes current chapter)
- Story info panel (expandable synopsis)

**Widgets Used:**
- ChapterCard (with progress, lock, badge), StoryCoverArt, OverallProgressBar, ContinueButton, StoryInfoPanel, ChapterBadge

**Components Used:**
- StoryChapterSelect, ChapterGridController, ChapterUnlockValidator, ProgressAggregator

**Navigation:**
- Enter: From Library, Recommendations, or content browsing
- Exit: Tap chapter card to Story Reader or Story Interactive
- Transitions: Card tap expands to reader (400ms)

**Animations:**
- Chapter cards: Staggered entrance (100ms stagger)
- Progress bars: Fill animation on load (600ms)
- Locked chapters: Shake on tap (300ms)
- Chapter unlock: Glow and scale animation (600ms)
- Continue button: Pulse (2s loop)

**AI Behavior:**
- AI recommends next chapter based on reading pace
- AI suggests skipping previously mastered chapters
- AI unlocks chapters based on prerequisite completion
- AI provides chapter preview/summary on hover

**Loading State:**
- Cover art skeleton with shimmer
- Chapter card skeletons (6-8)
- Chapter thumbnails progressive loading

**Empty State:**
- No chapters: "Story coming soon" with notification option

**Error State:**
- Chapter list fail: retry with cached chapter data
- Unlock validation error: fallback to linear unlock

**Offline State:**
- Downloaded chapters available with badge
- Chapter progress tracked offline

**Accessibility Rules:**
- Chapter titles announced with number
- Progress: "Chapter 3, 60% complete"
- Locked status announced with prerequisite info
- Grid navigation with directional keys

**Dark Mode Differences:**
- Cards: dark with light text, Locked chapters: darker, gray icon
- Progress: bright fill

**Light Mode Differences:**
- Cards: white with shadow, Locked chapters: grayed out
- Progress: accent fill

**Tablet Layout Differences:**
- Grid layout (3 columns), Larger chapter cards
- Chapter preview on hover

**Desktop Layout Differences:**
- Grid layout (4 columns), Chapter preview tooltip
- Sort and filter options, Keyboard navigation

**Mobile Layout Differences:**
- List layout (single column), Compact cards

**Landscape vs Portrait Differences:**
- Portrait: List/grid vertical scroll
- Landscape: Wide grid, more columns

**Responsive Rules:**
- Breakpoints: 360px, 600px, 840px, 1024px
- Card size: 160-240px width
- Grid columns: 1-4 based on viewport
