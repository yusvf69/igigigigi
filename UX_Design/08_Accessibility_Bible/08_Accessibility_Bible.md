# Accessibility Bible

**Document Version:** 1.0  
**Last Updated:** July 2026  
**WCAG Target:** Level AA Minimum, AAA Where Possible  
**Platform Coverage:** iOS, Android, Web, Desktop  
**Owner:** UX Design Team — Accessibility Guild  

---

## Table of Contents

1. [Accessibility Philosophy](#1-accessibility-philosophy)
2. [WCAG 2.1 Compliance Target](#2-wcag-21-compliance-target)
3. [Color Blind Support](#3-color-blind-support)
4. [Screen Reader Support](#4-screen-reader-support)
5. [Keyboard Navigation](#5-keyboard-navigation)
6. [Motion Reduction](#6-motion-reduction)
7. [High Contrast Mode](#7-high-contrast-mode)
8. [Font Scaling](#8-font-scaling)
9. [Language Direction](#9-language-direction)
10. [Audio Support](#10-audio-support)
11. [Captions & Subtitles](#11-captions--subtitles)
12. [Focus Indicators](#12-focus-indicators)
13. [Touch Targets](#13-touch-targets)
14. [Assistive Technology Support](#14-assistive-technology-support)
15. [Cognitive Accessibility](#15-cognitive-accessibility)
16. [Testing Protocol](#16-testing-protocol)
17. [Accessible Components Documentation](#17-accessible-components-documentation)
18. [Related Documents](#18-related-documents)

---

## 1. Accessibility Philosophy

### 1.1 Core Belief

Learning is a human right. The platform must be usable by everyone regardless of ability, circumstance, or environment. Accessibility is not a compliance checkbox, a feature flag, or a post-launch polish task. It is a core design principle embedded in every decision from concept to delivery.

### 1.2 Founding Principles

**Principle 1 — Accessibility Is a Core Design Principle, Not an Afterthought**

Every design review begins with an accessibility consideration. Every user story includes an accessibility acceptance criterion. Every component specification documents its accessible behavior before its visual style. Accessibility is not retrofitted. It is architected in from the first sketch.

**Principle 2 — Every Feature Must Have at Least Two Modalities**

No information may be conveyed through a single sensory channel. Visual content must have an auditory alternative. Auditory content must have a visual or textual alternative. Interactive controls must be operable by pointing device, keyboard, voice, and assistive technology. Redundancy of modality is the foundation of inclusive design.

**Principle 3 — Investment in Accessibility Is Investment in Learning Outcomes**

Students who rely on assistive technology are not edge cases. They are learners whose success depends on our design decisions. Every accessibility improvement directly improves educational outcomes. Accessible design benefits all users: captions help non-native speakers, keyboard shortcuts help power users, high contrast helps outdoor reading, clear language helps everyone.

**Principle 4 — Nothing About Us Without Us**

Accessibility decisions must involve people with disabilities. User research includes participants across the full spectrum of ability. Design reviews include accessibility experts. Testing protocols mandate testing with real users who rely on assistive technology. Assumptions about disability are never made in isolation.

**Principle 5 — Accessibility Is a Continuous Practice, Not a Final State**

As platforms evolve, assistive technology evolves, and standards evolve. Accessibility is maintained through every release, reviewed in every sprint, and tested in every deployment. There is no "done" for accessibility. There is only ongoing commitment.

### 1.3 Business Case

- **Legal Compliance:** WCAG 2.1 Level AA is the legal standard in the European Accessibility Act, Americans with Disabilities Act (ADA), Section 508, Accessibility for Ontarians with Disabilities Act (AODA), and accessibility legislation in over 20 countries.
- **Market Reach:** Over 1 billion people worldwide have a disability. The accessible market represents over $6 trillion in annual disposable income.
- **SEO Benefits:** Accessible semantic markup improves search engine ranking.
- **Universal Design:** Curb-cut effect — accessibility improvements benefit all users. Captions help users in noisy environments. High contrast helps users in bright sunlight. Keyboard navigation helps users with temporary injuries.
- **Brand Trust:** An accessible platform signals genuine commitment to equity and inclusion.

### 1.4 Accessibility Statement

The platform is committed to making learning accessible to every person, regardless of ability. We target WCAG 2.1 Level AA as our minimum compliance standard, with Level AAA compliance for text contrast, captions, and sign language interpretation for key educational content. Our accessibility program includes automated testing in CI/CD, manual testing by accessibility specialists, and user testing with people with disabilities. Accessibility issues are treated with the same severity as security issues. If you encounter an accessibility barrier, please contact our accessibility team.

---

## 2. WCAG 2.1 Compliance Target

### 2.1 Compliance Level

**Minimum Standard:** WCAG 2.1 Level AA for all content and functionality.  
**Target Standard:** WCAG 2.1 Level AAA for text contrast, captions, sign language interpretation, and audio descriptions.  
**Future Standard:** WCAG 2.2 and WCAG 3.0 preparedness tracked.

### 2.2 Principle 1 — Perceivable

Information and user interface components must be presentable to users in ways they can perceive.

#### 1.1.1 Non-text Content (Level A)

| Requirement | Implementation | Verification |
|---|---|---|
| All images have meaningful alt text | Alt text describes the content and function of the image | Automated + manual review |
| Decorative images marked with empty alt | `alt=""` for decorative images | Automated check |
| Complex images (charts, diagrams) have long descriptions | `aria-describedby` or adjacent text description | Manual review |
| CAPTCHA alternatives | Audio CAPTCHA + accessible challenge for each visual CAPTCHA | Manual review |
| Icons have accessible names | `aria-label` or visible text label for all icon-only buttons | Automated check |

#### 1.2.1 Audio-only and Video-only (Prerecorded) (Level A)

| Requirement | Implementation |
|---|---|
| Prerecorded audio-only content has transcript | Full verbatim transcript with speaker identification |
| Prerecorded video-only content has transcript or audio description | Text transcript or audio description track |

#### 1.2.2 Captions (Prerecorded) (Level A)

| Requirement | Implementation |
|---|---|
| All prerecorded video content has captions | Synchronized captions in the content language |
| Captions are accurate, synchronized, and complete | Human-reviewed, machine-generated baseline |
| Speaker identification included | [Speaker Name]: before each speaker's dialogue |

#### 1.2.3 Audio Description or Media Alternative (Prerecorded) (Level A)

| Requirement | Implementation |
|---|---|
| Video with audio provides audio description or full text alternative | Audio description track or detailed transcript describing visual information |

#### 1.2.4 Captions (Live) (Level AA)

| Requirement | Implementation |
|---|---|
| All live video content has captions | Real-time captioning via professional CART provider or AI with human review |
| Latency under 5 seconds | Captions stream with minimal delay |

#### 1.2.5 Audio Description (Prerecorded) (Level AA)

| Requirement | Implementation |
|---|---|
| Audio description provided for all prerecorded video content | Separate audio track with narrated descriptions of visual elements |

#### 1.2.6 Sign Language (Prerecorded) (Level AAA)

| Requirement | Implementation |
|---|---|
| Key educational content includes sign language interpretation | Sign language video overlay or separate track for core instructional videos |
| Sign language interpreter is clearly visible | Interpreter positioned in a dedicated frame, minimum 25% of viewport |

#### 1.2.7 Extended Audio Description (Prerecorded) (Level AAA)

| Requirement | Implementation |
|---|---|
| When pauses in dialogue are insufficient for description | Extended audio description with video pause mechanism |
| Seamless playback integration | Video pauses during description, resumes automatically |

#### 1.2.8 Media Alternative (Prerecorded) (Level AAA)

| Requirement | Implementation |
|---|---|
| All video content has a full text alternative | Complete transcript describing both audio and visual content |

#### 1.2.9 Audio-only (Live) (Level AAA)

| Requirement | Implementation |
|---|---|
| Live audio-only content has a text alternative | Real-time captioning or live transcript |

#### 1.3.1 Info and Relationships (Level A)

| Requirement | Implementation |
|---|---|
| Semantic structure conveyed through markup | Proper heading hierarchy (h1-h6), lists (ul/ol), tables (th/td) |
| Form labels programmatically associated | `label for` or `aria-labelledby` on all form controls |
| Headings nested properly | No skipped heading levels |
| Data tables have header cells | `<th>` with `scope` attribute |
| CSS pseudo-content used for decoration only | Content added via CSS `::before`/`::after` has no semantic meaning |

#### 1.3.2 Meaningful Sequence (Level A)

| Requirement | Implementation |
|---|---|
| Content presented in an order that preserves meaning | DOM order matches visual order |
| Tab order matches reading order | Logical navigation sequence |
| No reading-order disruptions from CSS | Absolute/fixed positioning does not reorder content |

#### 1.3.3 Sensory Characteristics (Level A)

| Requirement | Implementation |
|---|---|
| Instructions do not rely solely on shape, size, or location | "Press the button on the right" => "Press the Submit button" |
| Color is not the only way to identify content | "Click the green button" => "Click the Confirm button (green)" |

#### 1.3.4 Orientation (Level AA)

| Requirement | Implementation |
|---|---|
| Content does not restrict view to single orientation | Both portrait and landscape supported |
| Exception: applications with fixed orientation essential to functionality | Documented and approved |

#### 1.3.5 Identify Input Purpose (Level AA)

| Requirement | Implementation |
|---|---|
| Input fields that collect user information have autocomplete attributes | `autocomplete="name"`, `autocomplete="email"`, etc. |
| Standard HTML autocomplete values used | Full list per HTML specification |

#### 1.3.6 Identify Purpose (Level AAA)

| Requirement | Implementation |
|---|---|
| UI components and icons have programmatically identifiable purpose | `aria-label` + additional semantic identification where possible |

#### 1.4.1 Use of Color (Level A)

| Requirement | Implementation |
|---|---|
| Color is not the sole means of conveying information | Patterns, icons, text labels accompany all color-coded information |
| Status indicators: text or icon + color | Error: "Error: [message]" with icon, not just red text |
| Charts: patterns + labels + color | Bar charts use hatching patterns; line charts use dash styles + labels |
| Links: underlined + color | Underlined by default, not distinguished by color alone |

#### 1.4.2 Audio Control (Level A)

| Requirement | Implementation |
|---|---|
| Auto-playing audio can be stopped or controlled | Pause/stop button visible and keyboard accessible |
| Audio duration over 3 seconds has control | Play/pause, volume, mute controls accessible |
| No auto-playing audio unless user initiates | Default: all media is paused until user plays |

#### 1.4.3 Contrast Minimum (Level AA)

| Requirement | Implementation |
|---|---|
| Text contrast ratio: minimum 4.5:1 (body text) | Regular text under 18pt: 4.5:1 minimum |
| Text contrast ratio: minimum 3:1 (large text) | Bold text over 14pt or regular text over 18pt: 3:1 minimum |
| UI component contrast: 3:1 minimum | Interactive component boundaries and states |
| Active state indicators: 3:1 against adjacent colors | Focus rings, selection indicators, error states |
| Testing automated via CI/CD | Color contrast check in build pipeline |

#### 1.4.4 Resize Text (Level AA)

| Requirement | Implementation |
|---|---|
| Text can be resized up to 200% without loss of content | Responsive layouts using relative units (rem/em) |
| No horizontal scrolling at 200% zoom | Content reflows to remain in viewport |
| No text truncation at any zoom level | All text remains readable and complete |

#### 1.4.5 Images of Text (Level AA)

| Requirement | Implementation |
|---|---|
| Text is rendered as text, not images | No images of text except logos |
| Exceptions: logos, essential branding | Documented exceptions only |
| WCAG AAA: no images of text at all | Strictly avoided |

#### 1.4.6 Contrast Enhanced (Level AAA)

| Requirement | Implementation |
|---|---|
| Text contrast ratio: minimum 7:1 (body text) | Regular text under 18pt: 7:1 minimum |
| Text contrast ratio: minimum 4.5:1 (large text) | Bold text over 14pt or regular text over 18pt: 4.5:1 minimum |

#### 1.4.7 Low or No Background Audio (Level AAA)

| Requirement | Implementation |
|---|---|
| Prerecorded audio with no background noise or background noise 20dB below foreground | Audio production standards enforced |

#### 1.4.8 Visual Presentation (Level AAA)

| Requirement | Implementation |
|---|---|
| Customizable text styles: foreground, background, font | User settings control text and background color |
| Line spacing: minimum 1.5 within paragraphs | CSS `line-height: 1.5` |
| Paragraph spacing: minimum 1.5 times line spacing | CSS `margin-bottom` |
| Text resizable up to 200% without assistive tech | Browser zoom support |
| Text not justified | Left-aligned or right-aligned based on language direction |

#### 1.4.9 Images of Text (No Exception) (Level AAA)

| Requirement | Implementation |
|---|---|
| No images of text under any circumstances | All text rendered as live text |

#### 1.4.10 Reflow (Level AA)

| Requirement | Implementation |
|---|---|
| Content presented without loss of information at 400% zoom | Responsive single-column layout at maximum zoom |
| No horizontal scrolling at 400% | All content reflows vertically |
| Two-dimensional layout only required for content that requires it | Data tables, maps, diagrams: horizontal scroll indicated |

#### 1.4.11 Non-text Contrast (Level AA)

| Requirement | Implementation |
|---|---|
| UI components and graphic objects have 3:1 contrast ratio against adjacent colors | Buttons, form controls, focus indicators, icons, charts |
| Focus indicators: 3:1 against all adjacent colors | Focus ring visible on all backgrounds |
| State indicators: 3:1 against component | Active, hover, selected, error states |

#### 1.4.12 Text Spacing (Level AA)

| Requirement | Implementation |
|---|---|
| Content does not break when user overrides text spacing: | |
| Line height: 1.5 x font size | CSS `line-height: 1.5` minimum |
| Paragraph spacing: 2 x font size | CSS `margin-bottom: 2em` |
| Letter spacing: 0.12 x font size | CSS `letter-spacing: 0.12em` |
| Word spacing: 0.16 x font size | CSS `word-spacing: 0.16em` |

#### 1.4.13 Content on Hover or Focus (Level AA)

| Requirement | Implementation |
|---|---|
| Dismissible: content can be dismissed without moving pointer or keyboard focus | Escape key or click outside dismisses |
| Hoverable: pointer can be moved to hover content | No requirement to keep pointer on trigger |
| Persistent: content remains visible until dismissed, not based on hover timeout | Tooltip/content does not disappear automatically |

### 2.3 Principle 2 — Operable

User interface components and navigation must be operable.

#### 2.1.1 Keyboard (Level A)

| Requirement | Implementation |
|---|---|
| All functionality operable through keyboard interface | Every interactive element reachable and activatable via keyboard |
| No specific timings for keystrokes | Keys can be pressed sequentially with any interval |
| Mouse-specific interactions have keyboard alternatives | Drag-and-drop has move up/down buttons or keyboard reorder |
| Platform-standard keyboard conventions followed | Tab, Enter, Space, Arrow keys, Escape as expected |

#### 2.1.2 No Keyboard Trap (Level A)

| Requirement | Implementation |
|---|---|
| Keyboard focus can be moved away from any component | Standard Tab navigation not blocked |
| Focus trapping only in modals, sheets, and dialogs | Trapping announced to screen reader, Escape releases |
| Help text available for trapped focus | Instructions visible: "Press Escape to close" |

#### 2.1.3 Keyboard (No Exception) (Level AAA)

| Requirement | Implementation |
|---|---|
| All functionality operable via keyboard, even complex gestures | Drawing, drag-and-drop, complex interactions all have keyboard alternatives |

#### 2.1.4 Character Key Shortcuts (Level A)

| Requirement | Implementation |
|---|---|
| Keyboard shortcuts using single character keys can be turned off | User setting to disable character key shortcuts |
| Shortcuts can be remapped | Custom shortcut configuration |
| Shortcuts only active when component has focus | Preventing accidental activation |

#### 2.2.1 Timing Adjustable (Level A)

| Requirement | Implementation |
|---|---|
| Time limits have controls: turn off, adjust, extend | User can disable or extend any time limit |
| Minimum 10x default time limit for extension | Session timeout warning at 20 seconds remaining |
| No time limits on critical learning activities | Quizzes, assessments: no time limits unless essential and approved |
| Exception: real-time events (live sessions) | Clearly communicated to user |

#### 2.2.2 Pause, Stop, Hide (Level A)

| Requirement | Implementation |
|---|---|
| Moving, blinking, scrolling, or auto-updating content can be paused, stopped, or hidden | Controls visible and keyboard accessible |
| Auto-advancing carousels have pause button | Play/pause control on carousel |
| Auto-updating content (feeds, notifications) has pause | Pause updates button |
| Animations lasting more than 5 seconds can be stopped | Animation stop control |

#### 2.2.3 No Timing (Level AAA)

| Requirement | Implementation |
|---|---|
| No time limits present in the experience | All interactions are self-paced |

#### 2.2.4 Interruptions (Level AAA)

| Requirement | Implementation |
|---|---|
| Interruptions (notifications, updates) can be postponed or suppressed | Do Not Disturb mode, notification schedule |

#### 2.2.5 Re-authenticating (Level AAA)

| Requirement | Implementation |
|---|---|
| When session expires, data is preserved and user can resume | Form data saved, progress preserved, re-authentication returns to same state |

#### 2.2.6 Timeouts (Level AAA)

| Requirement | Implementation |
|---|---|
| Users informed of inactivity timeout duration | Warning displayed before timeout |
| Data preserved for at least 20 hours after timeout | Drafts, progress saved |

#### 2.3.1 Three Flashes or Below Threshold (Level A)

| Requirement | Implementation |
|---|---|
| No content flashes more than three times per second | Animation frame rate and flash count controlled |
| Flashes below the general flash and red flash thresholds | Automated testing for photosensitive epilepsy risk |
| No flashing content anywhere in the platform | Prohibited design pattern |

#### 2.3.2 Three Flashes (Level AAA)

| Requirement | Implementation |
|---|---|
| No content flashes more than three times in any one-second period | Absolute prohibition on flashing content |

#### 2.3.3 Animations from Interactions (Level AAA)

| Requirement | Implementation |
|---|---|
| Motion animations triggered by interaction can be disabled | `prefers-reduced-motion` respected; user setting to disable all non-essential animation |

#### 2.4.1 Bypass Blocks (Level A)

| Requirement | Implementation |
|---|---|
| Skip link at top of every page | "Skip to main content" link visible on focus |
| Skip link is first focusable element | Tab order begins with skip link |
| Heading structure supports navigation | Proper heading hierarchy enables screen reader navigation |
| Landmark regions enable navigation | ARIA landmarks: navigation, main, complementary, contentinfo, search |

#### 2.4.2 Page Titled (Level A)

| Requirement | Implementation |
|---|---|
| Every page has a descriptive, unique title | `<title>` element describes page content and purpose |
| Title identifies page in context of site | "Page Name — Platform Name" format |
| Titles change on dynamic navigation | Single Page Application updates document title on route change |

#### 2.4.3 Focus Order (Level A)

| Requirement | Implementation |
|---|---|
| Focus order preserves meaning and operability | Tab order follows visual reading order (left-to-right, top-to-bottom in LTR) |
| Focus does not jump unpredictably | No random or illogical focus movement |
| Interactive elements in correct sequence | Form fields follow logical order |

#### 2.4.4 Link Purpose (In Context) (Level A)

| Requirement | Implementation |
|---|---|
| Link purpose can be determined from link text alone or from link text plus programmatically determined context | Descriptive link text, not "click here" or "read more" |
| Context can be the enclosing heading, paragraph, list item, or table cell | ARIA context available |

#### 2.4.5 Multiple Ways (Level AA)

| Requirement | Implementation |
|---|---|
| Multiple methods to find content: navigation menu, search, sitemap | At least two of: navigation, search, site map, related links |

#### 2.4.6 Headings and Labels (Level AA)

| Requirement | Implementation |
|---|---|
| Headings and labels describe topic or purpose | Clear, descriptive, concise text |
| No duplicate headings unless same content | Unique headings for different sections |

#### 2.4.7 Focus Visible (Level AA)

| Requirement | Implementation |
|---|---|
| Keyboard focus indicator is visible on all interactive elements | 2px outline + 4px offset, high contrast, visible on all backgrounds |
| Focus indicator disappears only when focus moves | Never hidden while element has focus |

#### 2.4.8 Location (Level AAA)

| Requirement | Implementation |
|---|---|
| User's location within the platform is indicated | Breadcrumb navigation, progress indicator, current section highlighted in nav |

#### 2.4.9 Link Purpose (Link Only) (Level AAA)

| Requirement | Implementation |
|---|---|
| Link purpose can be determined from link text alone | Every link is independently descriptive without surrounding context |

#### 2.4.10 Section Headings (Level AAA)

| Requirement | Implementation |
|---|---|
| Section headings organize content | Beyond page title: section-level headings throughout content |

#### 2.5.1 Pointer Gestures (Level A)

| Requirement | Implementation |
|---|---|
| All pointer gestures have single-point alternative | Path-based gestures (swipe, drag) have button-based alternatives |
| Complex gestures have simple alternatives | Multi-finger gestures have single-tap alternatives |

#### 2.5.2 Pointer Cancellation (Level A)

| Requirement | Implementation |
|---|---|
| Down-event does not trigger action; action triggers on up-event | No action on mousedown/touchstart — only on mouseup/touchend |
| Abort or undo functionality for actions | User can cancel by moving pointer away before release |

#### 2.5.3 Label in Name (Level A)

| Requirement | Implementation |
|---|---|
| Accessible name includes visible text | Button labeled "Submit" has `aria-label="Submit"` not "Send" |
| Speech input users can activate by saying visible label | Visible label text matches accessible name |

#### 2.5.4 Motion Actuation (Level A)

| Requirement | Implementation |
|---|---|
| Functionality triggered by device motion has alternative | Shake, tilt, motion gestures have button-based alternatives |
| Motion can be disabled | User setting to disable motion actuation |
| Notifications for motion-activated features | User informed before motion functionality activates |

#### 2.5.5 Target Size (Level AA)

| Requirement | Implementation |
|---|---|
| Touch targets minimum 44x44 CSS pixels | All interactive elements meet size requirement |
| Exceptions: inline links, non-interactive elements | Documented and minimized |
| Spacing between targets: minimum 8px | Adequate spacing prevents mis-taps |

#### 2.5.6 Concurrent Input Mechanisms (Level AA)

| Requirement | Implementation |
|---|---|
| Content does not restrict input modality | Keyboard, mouse, touch, voice, and assistive technology all simultaneously supported |

#### 2.5.7 Dragging Movements (Level AA)

| Requirement | Implementation |
|---|---|
| Drag-and-drop has single-pointer alternative | Buttons to move items (Move Up, Move Down, Move To) |

#### 2.5.8 Target Size Minimum (Level AAA)

| Requirement | Implementation |
|---|---|
| Touch targets minimum 44x44 CSS pixels with no exceptions | Inline links also meet 44x44 minimum |

### 2.4 Principle 3 — Understandable

Information and the operation of the user interface must be understandable.

#### 3.1.1 Language of Page (Level A)

| Requirement | Implementation |
|---|---|
| Human language of each page programmatically determinable | `lang` attribute on `<html>` element |
| Language attribute matches content | Correct language code for every page |

#### 3.1.2 Language of Parts (Level AA)

| Requirement | Implementation |
|---|---|
| Language changes within content are marked | `lang` attribute on elements with different language content |

#### 3.1.3 Unusual Words (Level AAA)

| Requirement | Implementation |
|---|---|
| Mechanism to define unusual words or phrases | Glossary, tooltips, inline definitions |
| Jargon and technical terms explained | Definitions available on first use |

#### 3.1.4 Abbreviations (Level AAA)

| Requirement | Implementation |
|---|---|
| Mechanism to expand or define abbreviations | `<abbr>` with `title` attribute, glossary |

#### 3.1.5 Reading Level (Level AAA)

| Requirement | Implementation |
|---|---|
| Content requires reading ability no higher than lower secondary education | Plain language writing standard |
| Simplified version available for complex content | Abstract, summary, simplified explanation |

#### 3.1.6 Pronunciation (Level AAA)

| Requirement | Implementation |
|---|---|
| Mechanism to identify pronunciation of ambiguous words | Pronunciation guide, audio pronunciation |

#### 3.2.1 On Focus (Level A)

| Requirement | Implementation |
|---|---|
| No context change when component receives focus | Focus on a component does not trigger navigation, form submission, or significant UI change |

#### 3.2.2 On Input (Level A)

| Requirement | Implementation |
|---|---|
| No context change when user changes input setting | Changing a form field does not auto-submit or navigate |
| Exception: approved user-controlled auto-save | Clearly communicated to user |

#### 3.2.3 Consistent Navigation (Level AA)

| Requirement | Implementation |
|---|---|
| Navigation patterns repeat across pages | Primary navigation in same location, same order throughout |
| Consistent labeling across the platform | "Courses" always labeled "Courses", not "My Classes" elsewhere |

#### 3.2.4 Consistent Identification (Level AA)

| Requirement | Implementation |
|---|---|
| Components with same functionality are identified consistently | Same icon means same action everywhere |
| Terminology consistent throughout | "Enroll" always "Enroll", not "Join" on some pages |

#### 3.2.5 Change on Request (Level AAA)

| Requirement | Implementation |
|---|---|
| Context changes initiated only by user request | No automatic updates, refreshes, or redirects |

#### 3.3.1 Error Identification (Level A)

| Requirement | Implementation |
|---|---|
| Input errors described clearly to user | Error message identifies which field and what went wrong |
| Error messages in text, not just visual | Text description accompanies any visual error indicator |

#### 3.3.2 Labels or Instructions (Level A)

| Requirement | Implementation |
|---|---|
| Labels or instructions provided when input requires user input | Visible label for every form field |
| Format requirements described | "Date format: MM/DD/YYYY" |
| Required fields clearly indicated | "Required" label or asterisk with explanation |

#### 3.3.3 Error Suggestion (Level AA)

| Requirement | Implementation |
|---|---|
| Suggestions for fixing errors provided | "Please enter a valid email address (e.g., name@example.com)" |

#### 3.3.4 Error Prevention (Legal, Financial, Data) (Level AA)

| Requirement | Implementation |
|---|---|
| For submissions causing legal or financial consequences: | |
| Reversible: submissions can be undone | Undo option after submission |
| Checked: user can review and correct before final | Confirmation screen before final submission |
| Confirmed: user can confirm before submission | Explicit confirmation action (not just navigation) |

#### 3.3.5 Help (Level AAA)

| Requirement | Implementation |
|---|---|
| Context-sensitive help available | Help button, tooltip, or link provides field-specific guidance |

#### 3.3.6 Error Prevention (All) (Level AAA)

| Requirement | Implementation |
|---|---|
| All user submissions have error prevention mechanisms | Reversible, checked, confirmed for all submissions |

### 2.5 Principle 4 — Robust

Content must be robust enough to be interpreted by a wide variety of user agents, including assistive technologies.

#### 4.1.1 Parsing (Level A)

| Requirement | Implementation |
|---|---|
| Elements have complete start and end tags | HTML validation in CI/CD |
| Elements nested according to specifications | Proper DOM structure |
| No duplicate attributes | Validation enforced |
| Unique IDs | No duplicate ID attributes |

#### 4.1.2 Name, Role, Value (Level A)

| Requirement | Implementation |
|---|---|
| All user interface components have programmatically determinable name and role | ARIA roles, states, properties correctly applied |
| Custom controls have proper ARIA | `role`, `aria-*` attributes for non-semantic HTML |
| States communicated programmatically | `aria-expanded`, `aria-pressed`, `aria-selected`, `aria-disabled` |
| Changes in state communicated | Dynamic updates to ARIA attributes |
| User agent can monitor changes | Mutation observers, ARIA live regions |

#### 4.1.3 Status Messages (Level AA)

| Requirement | Implementation |
|---|---|
| Status messages communicated to assistive technology without focus change | `role="status"`, `role="alert"`, `aria-live` regions |
| Loading states announced | `aria-busy="true"`, progress indicators |
| Success/failure messages announced | `role="alert"` for errors, `role="status"` for success |
| Dynamic content updates announced | `aria-live="polite"` for non-critical, `aria-live="assertive"` for time-sensitive |

### 2.6 WCAG 2.2 Preparedness

| Success Criterion | Target | Current Status |
|---|---|---|
| 2.4.11 Focus Not Obscured (Minimum) (AA) | Focus indicator fully visible, not hidden by other content | Implemented |
| 2.4.12 Focus Not Obscured (Enhanced) (AAA) | Focus indicator entirely visible | Implemented |
| 2.4.13 Focus Appearance (AAA) | Focus indicator: minimum 2px thick, contrast 3:1, area of focus indicator at least area of 1 CSS pixel perimeter of component | Implemented |
| 2.5.7 Dragging Movements (AA) | Single-pointer alternative for all drag operations | Implemented |
| 2.5.8 Target Size (Minimum) (AA) | 24x24 minimum target size | Implemented |
| 3.2.6 Consistent Help (A) | Help mechanism in consistent location | Implemented |
| 3.3.7 Accessible Authentication (AA) | No cognitive function test required for authentication | Implemented |
| 3.3.8 Accessible Authentication (No Exception) (AAA) | No cognitive function test at all | Implemented |

---

## 3. Color Blind Support

### 3.1 Prevalence and Types

| Type | Scientific Name | Prevalence (Male) | Prevalence (Female) | Description |
|---|---|---|---|---|
| Protanopia | Red-blind | 1.0% | 0.01% | Cannot perceive red light; red appears dark or gray |
| Protanomaly | Red-weak | 1.3% | 0.02% | Reduced sensitivity to red light |
| Deuteranopia | Green-blind | 1.2% | 0.01% | Cannot perceive green light; green and red appear similar |
| Deuteranomaly | Green-weak | 5.0% | 0.1% | Reduced sensitivity to green light (most common) |
| Tritanopia | Blue-blind | 0.008% | 0.008% | Cannot perceive blue light; blue appears green |
| Tritanomaly | Blue-weak | Extremely rare | Extremely rare | Reduced sensitivity to blue light |
| Achromatopsia | Total color blindness | 0.003% | 0.003% | No color perception; sees only black, white, gray |
| Achromatomaly | Partial color blindness | Extremely rare | Extremely rare | Very limited color perception |

**Total color vision deficiency (CVD) affected population:** Approximately 8% of males, 0.5% of females — roughly 300 million people worldwide.

### 3.2 Design Principle: Never Rely on Color Alone

**Absolute rule:** No information may be conveyed through color alone. Every color-coded element must have at least one additional differentiator.

| Pattern | Color-Only (Forbidden) | Accessible Alternative |
|---|---|---|
| Status indicators | Green circle for success, red circle for error | Green circle + checkmark icon + "Success" label |
| Chart data series | Blue line vs orange line | Blue solid line vs orange dashed line + direct labels |
| Form validation | Red border on invalid fields | Red border + error icon + "Invalid" text message |
| Quiz answers | Green highlight for correct, red for incorrect | Correct: green + checkmark; Incorrect: red + X |
| Progress tracking | Color-coded completion dots | Checkmark, partial (half-filled), empty states + text labels |
| Tag categories | Color-coded topic tags | Color + icon + category name text |

### 3.3 Accessible Color Palette

The accessible palette is designed to maintain maximum distinction even when desaturated to grayscale.

#### 3.3.1 Primary Palette (High Contrast, Color-Safe)

| Token | Hex | Name | Usage | Passes AAA Text (7:1) |
|---|---|---|---|---|
| --color-primary | #0057B3 | Deep Blue | Primary actions, links | Yes (on white) |
| --color-primary-hover | #003D7A | Darker Blue | Hover states | Yes |
| --color-primary-light | #E8F0FE | Light Blue Background | Selected states, subtle backgrounds | — |
| --color-secondary | #2E7D32 | Forest Green | Success states, completion | Yes |
| --color-secondary-hover | #1B5E20 | Dark Green | Hover states | Yes |
| --color-error | #C62828 | Dark Red | Errors, warnings | Yes |
| --color-error-hover | #B71C1C | Darker Red | Hover states | Yes |
| --color-warning | #E65100 | Deep Orange | Warnings, alerts | Yes |
| --color-warning-hover | #BF360C | Darker Orange | Hover states | Yes |
| --color-info | #0050A4 | Information Blue | Info messages | Yes |
| --color-success | #2E7D32 | Success Green | Success messages | Yes |
| --color-text-primary | #1A1A1A | Near Black | Body text | — |
| --color-text-secondary | #4D4D4D | Dark Gray | Secondary text | Yes (on white) |
| --color-text-disabled | #8C8C8C | Medium Gray | Disabled text | — |
| --color-background-primary | #FFFFFF | White | Primary background | — |
| --color-background-secondary | #F5F5F5 | Light Gray | Secondary backgrounds | — |

#### 3.3.2 Colors to Avoid (Problematic for Color Blind Users)

| Color | Problematic For | Reason |
|---|---|---|
| Red alone | Protanopia, Deuteranopia | Appears dark gray, indistinguishable from dark colors |
| Green alone | Protanopia, Deuteranopia | Appears gray, indistinguishable from gray |
| Red-Green combination | Protanopia, Deuteranopia | Both appear same shade of brown/gray |
| Light Blue-Pink combination | Tritanopia | Both appear gray |
| Yellow-Light Green | Tritanopia | Both appear gray |
| Green-Blue | Tritanopia | Hues shifted, both appear similar |
| Purple-Blue | Tritanopia | Hues shifted, both appear similar |

#### 3.3.3 Color Combinations That Work for All Types

| Combination | Protanopia | Deuteranopia | Tritanopia | Achromatopsia |
|---|---|---|---|---|
| Blue (#0057B3) + Orange (#E65100) | Excellent | Excellent | Good | Good (different brightness) |
| Blue (#0057B3) + Yellow (#FFC107) | Excellent | Excellent | Good | Good (different brightness) |
| Black + White + Blue | Excellent | Excellent | Excellent | Excellent |
| Dark Blue + Light Blue (different brightness) | Excellent | Excellent | Good | Good |
| Purple (#7B1FA2) + Yellow (#FFC107) | Good | Good | Excellent | Good |
| Teal (#00796B) + Orange (#E65100) | Good | Good | Good | Good |

### 3.4 Patterns per Color Blind Type

#### 3.4.1 Protanopia (Red-Blind)

- Reds appear dark brown or black — do not use red as a signal color
- Use blue/orange instead of blue/red for contrast
- Avoid dark red text on dark backgrounds (becomes invisible)
- For status indicators: use position + icon + shape + text, not just color
- Chart lines: use dashed/dotted patterns, not just red vs blue
- **Testing:** simulate with Coblis or browser dev tools, verify all red content is distinguishable

#### 3.4.2 Deuteranopia (Green-Blind)

- Greens appear brownish — do not use green as a positive signal
- "Green means go, red means stop" is invisible — add icons (checkmark/X)
- Avoid green text on gray backgrounds (becomes invisible)
- For charts: avoid green-red gradient — use blue-orange or blue-yellow
- **Testing:** simulate with Chrome DevTools or Stark plugin, verify green content has alternatives

#### 3.4.3 Tritanopia (Blue-Blind)

- Blues appear green — do not use blue as a primary differentiator
- Yellow and pink are indistinguishable — avoid pairing them
- Blue and green become identical — never use blue/green as contrasting pair
- **Testing:** simulate specific tritanopia filter, verify blue content has non-color differentiators

#### 3.4.4 Achromatopsia (Total Color Blindness)

- All colors appear as shades of gray — rely on brightness, pattern, shape, position, text
- Ensure minimum contrast ratio of 7:1 for all text (AAA standard)
- Use patterns (dots, stripes, crosshatch) for all data visualization
- Icons, labels, and text must differentiate what color differentiates for others
- **Testing:** view entire interface in grayscale mode, verify all information is conveyed through non-color channels

### 3.5 Accessible Charts and Data Visualization

| Chart Element | Color-Only (Forbidden) | Accessible Alternative |
|---|---|---|
| Data series | Different colors only | Different colors + different line styles (solid, dashed, dotted) + direct labels |
| Bar segments | Different colors | Different colors + different fill patterns (stripes, dots, solid) |
| Pie slices | Different colors | Different colors + different patterns + text labels directly on slices |
| Map regions | Color gradient | Color + pattern fill + text labels + data table alternative |
| Scatter plot | Colored dots | Colored dots + different shapes (circle, triangle, square) |

**Pattern library for charts:**

| Pattern | SVG/Canvas Implementation | Name |
|---|---|---|
| Solid fill | `fill="solid"` | Solid |
| Horizontal stripes | `fill="url(#horizontal-stripe)"` | Horizontal |
| Vertical stripes | `fill="url(#vertical-stripe)"` | Vertical |
| Diagonal stripes (/) | `fill="url(#diagonal-forward)"` | Forward Diagonal |
| Diagonal stripes (\) | `fill="url(#diagonal-backward)"` | Backward Diagonal |
| Crosshatch | `fill="url(#crosshatch)"` | Crosshatch |
| Dots (small) | `fill="url(#dots-small)"` | Small Dots |
| Dots (large) | `fill="url(#dots-large)"` | Large Dots |

### 3.6 Color Blind Simulation Tools

| Tool | Platform | Usage |
|---|---|---|
| Chrome DevTools — Rendering — Emulate vision deficiencies | Web | Development testing |
| Firefox Accessibility Inspector — Simulate | Web | Development testing |
| Stark Plugin (Figma, Sketch, Adobe XD) | Design | Design phase testing |
| Color Oracle (Desktop app) | macOS, Windows, Linux | Full screen simulation |
| Coblis — Color Blindness Simulator | Web | Image simulation |
| Adobe Color — Color Blind Safe | Web | Palette validation |
| WhoCanUse — Color contrast checker | Web | Contrast + CVD simulation |
| Sim Daltonism | macOS | Real-time color blindness simulation |

### 3.7 Color Blind Testing Protocol

1. Identify all screens and components that use color to convey information
2. For each element, document what information color conveys
3. Verify that the information is also conveyed through: icon, pattern, text label, position, or shape
4. Simulate each color blindness type (Protanopia, Deuteranopia, Tritanopia, Achromatopsia)
5. Verify all information remains distinguishable in each simulation
6. Test contrast in grayscale (all colors desaturated)
7. Run automated color-blindness checks in CI/CD pipeline
8. Include color-blind test cases in QA regression suite

---

## 4. Screen Reader Support

### 4.1 Supported Screen Readers

| Screen Reader | Platform | Primary Browser | Market Share | Keyboard Commands |
|---|---|---|---|---|
| VoiceOver | iOS, iPadOS, macOS | Safari | 72% (iOS) | VO keys (Control+Option) |
| TalkBack | Android | Chrome | 20% (Android) | Single-finger gestures |
| NVDA | Windows | Firefox, Chrome | 63% (Windows) | Insert key + commands |
| JAWS | Windows | Chrome, Firefox | 30% (Windows) | Insert key + commands |
| Narrator | Windows | Edge | 7% (Windows) | Caps Lock + commands |
| Orca | Linux | Firefox | <1% | Modifier key + commands |
| ChromeVox | ChromeOS | Chrome | ChromeOS built-in | Search/Launcher + commands |

### 4.2 General Requirements

#### 4.2.1 Accessible Labels

- Every interactive element must have an accessible name
- Prefer visible text labels over `aria-label` when possible
- Icon-only buttons must have `aria-label` describing the action
- Form controls must have programmatically associated labels (`<label for="">` or `aria-labelledby`)
- `aria-label` content must match visible text when both are present (Label in Name compliance)
- No element may have an empty accessible name unless intentionally decorative

#### 4.2.2 Alt Text for Images

| Image Type | Requirement | Implementation |
|---|---|---|
| Informative images | Meaningful alt text describing the content | `alt="Student clicking on a course card titled "Introduction to Biology""` |
| Decorative images | Empty alt text | `alt=""` |
| Complex images (charts, diagrams) | Descriptive alt + long description | `alt="Bar chart showing..."` + `aria-describedby` linking to text description |
| Functional images (icon links) | Alt text describing action | `alt="Close dialog"`, not `alt="X"` |
| Images of text | Alt text matching the text | `alt="Company Logo: Edulearn"` |
| Logos | Alt text with company name | `alt="Company Name"` |

#### 4.2.3 ARIA Landmarks

| Landmark | Role | Usage |
|---|---|---|
| `<header role="banner">` | `banner` | Site-wide header (once per page) |
| `<nav>` | `navigation` | Navigation blocks |
| `<main>` | `main` | Primary content (once per page) |
| `<aside>` | `complementary` | Sidebars, related content |
| `<footer>` | `contentinfo` | Site-wide footer (once per page) |
| `<form>` or `role="search"` | `search` | Search functionality |
| `role="form"` | `form` | Form elements |

**Landmark requirements:**

- Every page must have `banner`, `main`, and `contentinfo` landmarks
- Multiple navigation landmarks must be differentiated with `aria-label`
- Landmarks must nest properly (no overlapping landmarks)
- No more than one `banner`, `main`, or `contentinfo` per page

#### 4.2.4 ARIA Live Regions

| Use Case | Region Role | aria-live | aria-atomic | aria-relevant |
|---|---|---|---|---|
| Chat messages | `role="log"` | polite | false | additions |
| Notifications | `role="alert"` or `role="status"` | assertive (alert) / polite (status) | true | additions |
| Loading state | `role="progressbar"` or `aria-busy="true"` on container | N/A | N/A | N/A |
| Form error summary | `role="alert"` | assertive | true | additions |
| Search results update | `role="region"` with `aria-live` | polite | false | additions text |
| Timer/clock | `role="timer"` | polite | true | additions text |
| Auto-updating content | `role="region"` with `aria-live` | polite | false | additions text |

#### 4.2.5 Heading Hierarchy

- Single `h1` per page (page title)
- Headings nest in order: h1 -> h2 -> h3 -> h4 -> h5 -> h6
- No skipped heading levels (h1 -> h3 is invalid unless visually demonstrated to be correct)
- Headings describe the content that follows
- Use semantic heading tags, not styled text
- Heading hierarchy matches content outline

#### 4.2.6 Focus Management

| Scenario | Behavior |
|---|---|
| Modal opens | Focus moves to first interactive element in modal |
| Modal closes | Focus returns to trigger element |
| Sheet opens | Focus moves to first interactive element in sheet |
| Sheet closes | Focus returns to trigger element |
| Navigation changes | Focus moves to new content heading (skip to main content) |
| Dynamic content added | Focus management based on context (auto-focus new content or maintain position) |
| Error on form submit | Focus moves to first error field |
| Page load | Focus starts at top of page (skip link available) |
| Tab panel changes | Focus stays on tab, content updates |
| Accordion opens/closes | Focus stays on accordion button |

#### 4.2.7 Skip Links

- Skip link is the first focusable element on every page
- Skip link text: "Skip to main content"
- Skip link is visible on focus (not hidden until focused)
- Skip link navigates to `main` landmark
- Skip link visible to all users (not just screen reader)
- Additional skip links for complex pages: "Skip to navigation", "Skip to search"

### 4.3 Platform-Specific Requirements

#### 4.3.1 VoiceOver (iOS/iPadOS)

- All UIKit/SwiftUI elements use built-in accessibility APIs (never disable accessibility)
- Custom views implement `UIAccessibilityProtocol`
- Adjustable traits for sliders, pickers (`UIAccessibilityTraitAdjustable`)
- Proper `accessibilityLabel`, `accessibilityHint`, `accessibilityValue`, `accessibilityTraits`
- `accessibilityElementsHidden` for decorative elements
- `accessibilityViewIsModal` for modal presentations
- Rotor support for headings, links, landmarks, form controls
- Proper `accessibilityFrame` for custom layout elements
- Keyboard commands documented for Full Keyboard Access users

#### 4.3.2 TalkBack (Android)

- All views use `contentDescription` for images and icon-only controls
- Custom views implement `View.AccessibilityDelegate`
- `importantForAccessibility` set appropriately (yes/no/auto)
- `accessibilityTraits` set for custom controls
- `contentDescription` matches visible text
- Live regions on dynamic content updates
- `focusable="true"` on interactive elements
- Linear layout navigation by default (use `accessibilityTraversalBefore/After` sparingly)
- Explore by Touch support verified

#### 4.3.3 NVDA/JAWS (Web)

- All semantic HTML used appropriately
- ARIA attributes correctly applied
- No ARIA misuse (ARIA can break accessibility if used incorrectly)
- `aria-hidden="true"` on decorative content
- Focus management tested with NVDA and JAWS
- Dynamic content updates announced via live regions
- Form validation errors announced
- Single Page Application routing updates announced to screen reader

### 4.4 Content and Structure

- Use semantic HTML elements: `<button>`, `<nav>`, `<main>`, `<aside>`, `<header>`, `<footer>`, `<section>`, `<article>`
- Lists use `<ul>` or `<ol>` with `<li>` (never styled divs)
- Tables use `<table>` with `<th>`, `<thead>`, `<tbody>`, `scope` attributes
- Quotations use `<blockquote>` and `<q>`
- Emphasis uses `<em>` and `<strong>` (not styled text)
- Landmarks clearly identify regions

### 4.5 Screen Reader Announcement Patterns

| Event | Announcement | Implementation |
|---|---|---|
| Success | "[Action] completed successfully" | `aria-live="polite"` + visible text |
| Error | "Error: [description]. [Action needed]" | `role="alert"` + visible text |
| Loading | "Loading [content name]..." | `aria-busy="true"` + spinner with `aria-label` |
| Timer | "[Time] remaining" | `role="timer"` with live region |
| Notification | "[Notification content]" | `role="status"` (polite) or `role="alert"` (critical) |
| Item added | "[Item] added to [list]" | `aria-live="polite"` |
| Item removed | "[Item] removed from [list]" | `aria-live="polite"` |
| Drag started | "Dragging [item]. Current position [n]. Use arrow keys to move." | Multiple announcements |
| Drag ended | "[Item] moved to position [n]" | Single announcement |

---

## 5. Keyboard Navigation

### 5.1 Core Principles

- All functionality must be operable through a keyboard interface
- Tab order follows visual order (left-to-right, top-to-bottom in LTR; right-to-left, top-to-bottom in RTL)
- All interactive elements reachable via keyboard
- No keyboard traps (except in modals/sheets, with escape exit)
- Expected keyboard behavior matches platform conventions
- Focus indicator visible at all times

### 5.2 Tab Navigation

| Key | Behavior |
|---|---|
| Tab | Move focus forward through interactive elements |
| Shift + Tab | Move focus backward through interactive elements |
| Enter/Space | Activate focused element (button, link, form control) |

**Tab order requirements:**

- Tab order must be logical and predictable
- Tab order matches visual layout
- No use of positive `tabindex` values (use `tabindex="0"` or `tabindex="-1"` only)
- `tabindex="-1"` for programmatic focus only (not in tab order)
- Interactive elements in correct sequence: navigation -> search -> main content -> sidebar -> footer
- Skip link is first in tab order

### 5.3 Standard Keyboard Interactions

| Component | Keys | Behavior |
|---|---|---|
| Button | Enter, Space | Activates button action |
| Link | Enter | Navigates to link target |
| Text input | All keys | Text entry |
| Checkbox | Space | Toggle checked state |
| Radio group | Arrow keys | Navigate between options; Space/Enter to select |
| Select/dropdown | Enter, Space, Arrow keys | Open, navigate, select |
| Slider | Arrow keys, Home, End | Adjust value |
| Tab panel | Arrow keys (on tab) | Switch tab panels |
| Accordion | Enter, Space | Toggle accordion section |
| Modal/sheet | Tab (cycle), Shift+Tab (reverse), Escape | Cycle focus within, close |
| Listbox | Arrow keys | Navigate options |
| Tree view | Arrow keys, Enter, Space | Navigate, expand/collapse |
| Grid | Arrow keys | Navigate cells |
| Toolbar | Arrow keys, Tab | Navigate between and within |
| Menu/Menubar | Arrow keys, Enter, Escape | Navigate, select, close |
| Dialog | Tab cycle, Escape | Cycle focus, close |
| Date picker | Arrow keys, Enter | Navigate days, select |
| Color picker | Arrow keys, Enter | Adjust values, select |

### 5.4 Platform Keyboard Shortcuts

| Shortcut | Action | Scope | Customizable |
|---|---|---|---|
| `n` | Open notifications panel | Global | Yes |
| `s` | Focus search field | Global | Yes |
| `h` | Navigate to home/dashboard | Global | Yes |
| `/` | Focus search (from anywhere) | Global | Yes |
| `?` | Show keyboard shortcuts help | Global | No |
| `Escape` | Close modal/sheet/panel, go back | Contextual | No |
| `Ctrl+Enter` | Send message, submit form | Contextual | No |
| `g + d` | Go to dashboard | Global | Yes |
| `g + c` | Go to courses | Global | Yes |
| `g + p` | Go to profile | Global | Yes |
| `g + s` | Go to settings | Global | Yes |
| `j` | Next item in list | Contextual | Yes |
| `k` | Previous item in list | Contextual | Yes |
| `m` | Toggle mute/unmute | Media | Yes |
| `f` | Toggle fullscreen | Media | Yes |
| `Space` | Play/pause | Media | No |
| `Left/Right` | Seek backward/forward | Media | No |
| `Up/Down` | Volume up/down | Media | No |

### 5.5 Focus Trapping

**Modals and sheets:**

- Tab cycles within the modal/sheet
- Shift+Tab cycles in reverse within the modal/sheet
- Focus does not leave the modal/sheet while it is open
- First focusable element receives focus when modal/sheet opens
- Focus returns to trigger element when modal/sheet closes
- Escape key closes the modal/sheet
- Visible focus indicator on all elements inside modal/sheet
- `aria-modal="true"` and `role="dialog"` on the container

### 5.6 Visible Focus Indicator

- 2px solid outline around the focused element
- 4px offset from the element boundary
- High contrast color: `#0057B3` (blue) on light backgrounds, `#90CAF9` (light blue) on dark backgrounds
- Focus indicator must be visible on all backgrounds (light and dark modes)
- Focus indicator must not be clipped or hidden
- Circular elements: use `outline` or `box-shadow` for visible focus ring
- Focus indicator changes must be animated smoothly (transition: outline-opacity 100ms)

### 5.7 Arrow Key Navigation

| Component | Arrow Key Behavior | Implementation |
|---|---|---|
| List | Up/Down to navigate items, Enter to select | `role="listbox"` with `aria-activedescendant` or roving tabindex |
| Horizontal tab bar | Left/Right to switch tabs | `role="tablist"` with arrow key handling |
| Slider | Left/Right (or Up/Down) to adjust | `role="slider"` with `aria-valuenow` update |
| Tree view | Up/Down to navigate, Right to expand, Left to collapse | `role="tree"` with `aria-expanded` |
| Grid/Table | Arrow keys navigate cells | `role="grid"` with cell navigation |
| Knowledge graph | Arrow keys to navigate connected nodes | Custom implementation with focus management |
| Carousel | Left/Right to navigate slides | `aria-roledescription="carousel"` |
| Menu bar | Left/Right between menus, Up/Down in menu | `role="menubar"` and `role="menu"` |

### 5.8 Keyboard Testing Protocol

1. Navigate entire application using Tab key only
2. Verify all interactive elements are reachable
3. Verify all actions are operable (no mouse-dependent functions)
4. Verify focus order is logical
5. Verify visible focus indicator on every element
6. Test all keyboard shortcuts
7. Test with modals/sheets open (focus trapping, escape)
8. Test with screen reader + keyboard simultaneously
9. Test on all supported platforms (macOS, Windows, iOS, Android)
10. Document any keyboard accessibility issues

---

## 6. Motion Reduction

### 6.1 Philosophy

Motion and animation enhance the experience for many users but can cause discomfort, disorientation, vestibular disorders, migraines, or nausea for others. The platform respects user preferences and provides full functionality without reliance on motion.

### 6.2 Respecting prefers-reduced-motion

**CSS media query:** `@media (prefers-reduced-motion: reduce)`

When the user's operating system has Reduce Motion enabled:

- All animations are replaced with instant state changes
- All movement-based animations are replaced with opacity fades (0ms or 80% reduced duration)
- All animation durations are reduced by 80% or set to 0ms
- Transition durations reduced to 0ms (instant) for all state changes

**JavaScript detection:**

```javascript
const reducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;
```

When reduced motion is detected, all JavaScript-driven animations are disabled.

### 6.3 Motion Reduction Behavior by Component

| Animation | Normal Behavior | Reduced Motion Behavior |
|---|---|---|
| Page transitions | Slide in from right | Instant appear (fade at most) |
| Modal open | Scale up from center | Instant appear |
| Sheet open | Slide up from bottom | Instant appear |
| List item entrance | Staggered fade in from bottom | All items appear instantly |
| Hover effects | Scale, lift, shadow | Color change only (no transform) |
| Parallax backgrounds | Move at different rate while scrolling | Static, no movement |
| Confetti/celebrations | Particle burst animation | Static badge or message: "Congratulations" |
| Shimmer loading | Animated gradient sweep | Static gray placeholder (no animation) |
| Floating elements | Gentle float/bob animation | Static — no floating movement |
| Pulsing indicators | Gentle pulse animation | Static indicator (color change only) |
| Progress animations | Animated fill | Instant percentage update |
| Carousel auto-advance | Smooth slide transition | Instant slide change |
| Drag and drop | Animated item movement | Instant position change |
| Swipe to delete | Interactive swipe | Button-based delete with no swipe |
| Pull to refresh | Animated pull indicator | Static refresh button |

### 6.4 Reduced Motion Profile

When `prefers-reduced-motion: reduce` is active:

```
--duration-instant: 0ms
--duration-fast: 50ms (reduced from 200ms)
--duration-normal: 100ms (reduced from 300ms)
--duration-slow: 200ms (reduced from 500ms)
--motion-transform: none
--motion-scale: none
--motion-rotate: none
--motion-translate: none
--motion-opacity: 0.3 (allowed)
--motion-color: allowed
--motion-blur: none
--motion-filter: none
--motion-clip-path: none
```

### 6.5 Disabled Animations

The following animation types are completely disabled in reduced motion mode:

- Parallax scrolling
- Confetti and particle effects
- Floating or bobbing elements
- Shimmer loading skeletons
- Pulsing or breathing elements
- Scale/zoom hover effects
- Staggered entrance animations
- Continuous rotation (spinners except essential loading)
- Ghost/phantom elements
- Typing animations
- Animated gradients
- Ken Burns effect on images
- Smooth scroll (instant scroll instead)
- Morphing shapes

### 6.6 Essential Feedback (Kept in Reduced Motion)

- Color change on hover/focus (no transform)
- Opacity change for press/active states
- Progress indicators (non-animated, static percentage)
- Focus ring appearance (instant)
- Error/success state changes (instant color + icon)

### 6.7 User Setting for Motion

In addition to OS-level `prefers-reduced-motion`, the platform provides an in-app motion setting:

| Setting | Options | Effect |
|---|---|---|
| Motion preference | System default / Reduced / Full | "System default" respects OS; "Reduced" applies reduced motion; "Full" enables all animations |

### 6.8 Testing Protocol for Motion Reduction

1. Enable Reduce Motion in OS settings (macOS: System Settings -> Accessibility -> Display -> Reduce Motion; iOS: Settings -> Accessibility -> Motion -> Reduce Motion; Windows: Settings -> Ease of Access -> Display -> Show animations)
2. Navigate through all critical flows
3. Verify all state changes happen instantly (no lingering animations)
4. Verify no content is missing due to disabled animations
5. Verify hover/focus state changes still provide feedback
6. Test all modal/sheet transitions
7. Test all page navigation
8. Test loading states
9. Test celebrations/success states
10. Test with screen reader active to verify announcements are not tied to animation timings

---

## 7. High Contrast Mode

### 7.1 Philosophy

Users with low vision, light sensitivity, or those using devices in extreme lighting conditions require enhanced contrast. The platform supports both OS-level high contrast settings and provides a custom high contrast theme.

### 7.2 Supported Modes

| Mode | Trigger | Priority |
|---|---|---|
| prefers-contrast: more | OS-level "Increase Contrast" | High |
| forced-colors: active | Windows High Contrast Mode | High |
| In-app high contrast theme | User setting in platform preferences | High |

### 7.3 High Contrast Design Requirements

#### 7.3.1 General

- All borders enhanced from 1px to 2px minimum
- All text contrast: 7:1 minimum (AAA compliance in high contrast mode)
- Visible separators between all elements (not whitespace alone)
- All interactive states clearly visible (hover, focus, active, selected, disabled)
- Background images removed or have solid fallback
- No transparency or opacity-based text
- No gradients for backgrounds (solid colors only)

#### 7.3.2 Windows High Contrast Mode (forced-colors)

When `forced-colors: active` is detected:

- All background colors are overridden by the user's high contrast theme
- All text colors are overridden by the user's high contrast theme
- All links are underlined (`text-decoration: underline` enforced)
- All borders respect `CanvasText` and `ButtonText` system colors
- Icons use `currentColor` or `CanvasText` to remain visible
- Focus indicators use system `Highlight` color
- `background-image` and `box-shadow` are overridden — use borders instead
- `forced-color-adjust: none` only used when necessary (and documented)

**CSS implementation pattern:**

```css
/* Base styles work with forced-colors */
.button {
  border: 2px solid transparent;
}

@media (forced-colors: active) {
  .button {
    border-color: ButtonText;
    background-color: ButtonFace;
    color: ButtonText;
  }

  .button:focus {
    outline: 3px solid Highlight;
    outline-offset: 2px;
  }

  .button:hover {
    background-color: Highlight;
    color: HighlightText;
  }
}
```

#### 7.3.3 Custom High Contrast Theme

The platform provides an in-app high contrast theme toggle:

| Element | Normal Theme | High Contrast Theme |
|---|---|---|
| Background | White (#FFFFFF) | Black (#000000) |
| Background secondary | Light Gray (#F5F5F5) | Dark Gray (#1A1A1A) |
| Text primary | Near Black (#1A1A1A) | White (#FFFFFF) |
| Text secondary | Dark Gray (#4D4D4D) | Light Gray (#CCCCCC) |
| Links | Blue (#0057B3) | Yellow (#FFD600) + underlined |
| Borders | Light Gray (#E0E0E0) | White (#FFFFFF) |
| Primary buttons | Blue bg, white text | Yellow bg, black text |
| Error states | Red (#C62828) + icon | Red (#FF5252) + icon + border |
| Success states | Green (#2E7D32) + icon | Green (#69F0AE) + icon + border |
| Focus indicator | Blue 2px + 4px offset | Yellow 3px + 4px offset |
| Glassmorphism | Translucent blur | Solid color, no transparency |
| Shadows | Box shadow | 2px solid border (no shadow) |
| Dividers | 1px light gray | 2px solid white |

#### 7.3.4 High Contrast Focus Indicators

- Focus indicator: 3px solid outline (instead of 2px)
- Focus indicator color: Yellow (#FFD600) on dark backgrounds, Blue (#0057B3) on light backgrounds
- Focus indicator offset: 4px from element
- Focus indicator must be visible in all high contrast modes
- Windows High Contrast mode uses system `Highlight` color

### 7.4 Elements That Must Remain Visible in High Contrast Mode

- Icons (use `currentColor` or `ButtonText`)
- Status indicators (use border + icon + text, not color alone)
- Loading spinners (use border-based animation, not background images)
- Progress bars (use border contrast, not color fill alone)
- Tooltips (use borders, not shadows, to define boundaries)
- Badges and tags (use borders, not colored backgrounds alone)
- Avatar initials (ensure contrast against any forced background)
- Charts (use patterns + borders, not color alone)

### 7.5 Testing Protocol for High Contrast Mode

1. Enable OS-level Increase Contrast (macOS: System Settings -> Accessibility -> Display -> Increase Contrast; iOS: Settings -> Accessibility -> Display & Text Size -> Increase Contrast)
2. Enable Windows High Contrast Mode (Windows: Settings -> Ease of Access -> High Contrast)
3. Verify all text meets 7:1 contrast ratio
4. Verify all interactive elements are visible with enhanced borders
5. Verify all UI states (hover, focus, active, selected, disabled) are distinguishable
6. Verify no information is lost (icons, status, charts all visible)
7. Verify glassmorphism/transparency effects have solid fallbacks
8. Verify links are underlined
9. Verify focus indicators are highly visible
10. Test all critical flows in high contrast mode
11. Verify with color blind simulation + high contrast simultaneously
12. Verify with screen reader + high contrast simultaneously

---

## 8. Font Scaling

### 8.1 Philosophy

Users must be able to read text comfortably regardless of their visual abilities. The platform supports system-level font size settings and allows text scaling up to 200% without loss of content, functionality, or visual integrity.

### 8.2 Supported Font Size Adjustments

| Platform | Adjustment Method | Maximum Supported |
|---|---|---|
| iOS/iPadOS | Dynamic Type (text styles) | Accessibility Extra Extra Extra Large |
| Android | Font size settings | Largest setting (200%) |
| Web | Browser zoom + OS settings | 200% minimum |
| macOS | System Preferences + browser zoom | 200% minimum |
| Windows | Ease of Access + browser zoom | 200% minimum |

### 8.3 Dynamic Type Support (iOS)

All iOS text must use Dynamic Type text styles:

| TextStyle | Point Size (Default) | Usage |
|---|---|---|
| UIFontTextStyleLargeTitle | 34pt | Hero headings |
| UIFontTextStyleTitle1 | 28pt | Section headings |
| UIFontTextStyleTitle2 | 22pt | Subsection headings |
| UIFontTextStyleTitle3 | 20pt | Card titles |
| UIFontTextStyleHeadline | 17pt (bold) | Headlines |
| UIFontTextStyleBody | 17pt | Body text |
| UIFontTextStyleCallout | 16pt | Callouts |
| UIFontTextStyleSubheadline | 15pt | Subheadings |
| UIFontTextStyleFootnote | 13pt | Footnotes |
| UIFontTextStyleCaption1 | 12pt | Captions |
| UIFontTextStyleCaption2 | 11pt | Small captions |

**Implementation requirements:**

- Use `UIFontMetrics` for custom fonts (scaling calculation)
- `adjustsFontForContentSizeCategory = true` on all labels and text views
- `maximumContentSizeCategory` should not be set (allow all sizes up to accessibility sizes)
- Test at each Dynamic Type size: XS, S, M, L, XL, XXL, XXXL, Accessibility XL, Accessibility XXL, Accessibility XXXL

### 8.4 Dynamic Layout Behavior at Large Font Sizes

| Font Size | Layout Behavior |
|---|---|
| 100% (default) | Standard layout, multi-column where appropriate |
| 125% | Slightly increased spacing, text starts wrapping earlier |
| 150% | Multi-column collapses to single column, increased padding |
| 175% | Single column, significantly increased vertical spacing |
| 200% | Single column, maximum spacing, no truncation, no overlap |

### 8.5 Minimum Font Sizes

| Context | Minimum Size | Notes |
|---|---|---|
| Body text | 16px (1rem) | Never smaller |
| Caption/helper text | 12px (0.75rem) | Never smaller; consider increasing to 14px for accessibility |
| Button text | 14px (0.875rem) | Minimum for readability |
| Input text | 16px (1rem) | Prevents iOS zoom on focus |
| Navigation text | 14px (0.875rem) | Minimum for readability |
| Heading | 16px+ (1rem+) | Scale relative to body |

### 8.6 Using Relative Units

- All font sizes use `rem` (web) or Dynamic Type (iOS) or `sp` (Android)
- All spacing/padding use `em` or `rem`
- All container sizes use relative units or flexible layouts
- No fixed pixel values for anything related to text size
- `clamp()` function used for responsive type: `clamp(1rem, 1rem + 0.5vw, 1.5rem)`

**Conversion reference:**

```css
/* DO: Relative units */
font-size: 1rem;       /* 16px base */
padding: 0.75em;       /* Relative to font-size */
margin-bottom: 1.5rem; /* Relative to root */

/* DO NOT: Fixed pixel values */
font-size: 14px;       /* Use 0.875rem */
padding: 12px;         /* Use 0.75rem */
line-height: 20px;     /* Use 1.5 or 1.6 (unitless) */
```

### 8.7 Layout Requirements at 200%

| Requirement | Verification |
|---|---|
| No text truncation | All text visible at all sizes |
| No text overlap | Elements do not overlap |
| No horizontal scrolling | Content reflows vertically |
| All buttons still tappable | Touch targets stay >= 44pt |
| All images still visible | Images scale or are not obscured |
| All interactive elements accessible | No elements hidden or pushed off-screen |
| Navigation still functional | Hamburger menu or stacked nav at large sizes |
| Cards stack vertically | Grid layouts collapse to single column |
| Modals scroll if needed | Content within modals scrolls |

### 8.8 Text Not Breaking

| Issue | Prevention |
|---|---|
| Truncation | `overflow: visible` or `min-width: 0` on flex items |
| Overlap | `word-break: break-word` or `overflow-wrap: break-word` |
| Horizontal scroll | `max-width: 100%` on all containers |
| Text pushed off-screen | Responsive containers, no fixed widths |
| Buttons with long text | Padding scales, button width flexible |

### 8.9 Testing Protocol for Font Scaling

1. Set system font size to smallest setting
2. Navigate all critical flows, verify readability
3. Set system font size to 200% / Accessibility XXXL
4. Navigate all critical flows, verify:
   - No text truncation or overlap
   - No horizontal scrolling
   - All touch targets meet 44pt minimum
   - All content is accessible
   - Navigation still usable (hamburger menu works)
   - Modals/dialogs scroll if needed
   - Buttons and inputs remain usable
5. Test at intermediate sizes (125%, 150%, 175%)
6. Verify Dynamic Type for all text elements (iOS)
7. Verify with screen reader active (text size + screen reader interaction)
8. Verify on all device sizes (phone, tablet, desktop)
9. Verify custom fonts scale correctly (not just system fonts)

---

## 9. Language Direction

### 9.1 Philosophy

The platform serves a global audience, including millions of users who read right-to-left (RTL) scripts: Arabic, Urdu, Persian/Farsi, Hebrew, and Pashto. Full RTL support is not an afterthought — it is a core layout capability. Every component must function correctly in both LTR and RTL contexts.

### 9.2 Supported Languages by Direction

| Direction | Languages | Script |
|---|---|---|
| LTR | English, Spanish, French, German, Portuguese, etc. | Latin, Cyrillic, etc. |
| RTL | Arabic, Urdu, Persian/Farsi, Hebrew, Pashto, Sindhi, Kurdish (Sorani), Uyghur | Arabic, Hebrew |

### 9.3 Layout Mirroring Requirements

When the interface language is RTL:

| Element | LTR Behavior | RTL Behavior |
|---|---|---|
| Navigation order | Left to right | Right to left |
| Logo position | Top left | Top right |
| Navigation menu | Left-aligned | Right-aligned |
| Page content | Left-aligned text | Right-aligned text |
| Back button | Left-pointing arrow (←) | Right-pointing arrow (→) |
| Forward button | Right-pointing arrow (→) | Left-pointing arrow (←) |
| Progress bar | Left (start) to Right (end) | Right (start) to Left (end) |
| Slider | Left (min) to Right (max) | Right (min) to Left (max) |
| Scrollbar | Right side | Left side |
| Close button | Top right | Top left |
| Search bar icon | Left side of input | Right side of input |
| Checkbox | Left of label | Right of label |
| Radio button | Left of label | Right of label |
| Star rating | Left to right (1st to 5th) | Right to left (1st to 5th) |
| Pagination | Left (first) to Right (last) | Right (first) to Left (last) |
| Timeline | Left to right | Right to left |
| Card actions | Bottom right | Bottom left |
| Avatar list | Left to right (first to last) | Right to left (first to last) |

### 9.4 Icon Mirroring

Some icons must be mirrored in RTL layouts because they imply direction:

| Icon | Mirrored in RTL? | Reason |
|---|---|---|
| Arrow left/right | Yes | Directional |
| Chevron left/right | Yes | Directional |
| Back arrow | Yes | Directional |
| Forward/next arrow | Yes | Directional |
| Send (paper plane) | Yes | Direction implied |
| Share | No | Universal (platform convention) |
| Search (magnifying glass) | No | Universal |
| Home | No | Universal |
| Settings (gear) | No | Universal |
| User/Profile | No | Universal |
| Notification (bell) | No | Universal |
| Like/Heart | No | Universal |
| Bookmark | No | Universal |
| Download (arrow down) | No | Pointing down |
| Upload (arrow up) | No | Pointing up |
| Undo arrow | Yes | Directional |
| Redo arrow | Yes | Directional |
| Previous/Next media | Yes | Directional |
| Sort arrows | Yes (if ascending/descending) | Directional |

**Rule:** If an icon implies direction (pointing left/right, clockwise/counterclockwise motion with direction), mirror it in RTL. If an icon is universal/symmetric, do not mirror it.

### 9.5 Text and Typography

#### 9.5.1 Arabic Script Requirements

- Proper Arabic script rendering (initial, medial, final, isolated forms)
- Correct tashkeel (diacritical marks) positioning: Fatha, Damma, Kasra, Sukun, Shadda, Tanween
- Ligature support (lam-alif and other standard ligatures)
- Proper numeral rendering: Arabic numerals (٠١٢٣٤٥٦٧٨٩) in Arabic content, or Eastern Arabic numerals depending on region
- Fonts must support Arabic character set (minimum 600+ glyphs for Arabic script)
- Line breaking rules: Arabic text breaks at word boundaries (not mid-word)

#### 9.5.2 Mixed LTR + RTL Content

- Numbers and embedded English terms remain LTR within RTL text (bidirectional text support)
- Code snippets remain LTR even in RTL interface
- User-generated content follows its own direction (not forced to interface direction)
- URLs, email addresses, and technical identifiers remain LTR
- Proper use of `<bdi>` (Bi-Directional Isolation) for user-generated content

#### 9.5.3 CSS Logical Properties

Use logical properties instead of physical properties:

```css
/* DO: Logical properties (auto-adapt to LTR/RTL) */
margin-inline-start: 1rem;
margin-inline-end: 1rem;
padding-inline: 1rem;
border-inline-start: 2px solid;
text-align: start;

/* DO NOT: Physical properties (break in RTL) */
margin-left: 1rem;
margin-right: 1rem;
padding-left: 1rem;
border-left: 2px solid;
text-align: left;
```

### 9.6 Animations and Gestures in RTL

| Animation | LTR Direction | RTL Direction |
|---|---|---|
| Page push transition | Right -> Left (new page slides in from right) | Left -> Right (new page slides in from left) |
| Modal/sheet slide | Slides up from bottom (same in RTL) | Slides up from bottom (no change) |
| Slide-to-delete | Swipe left to reveal delete | Swipe right to reveal delete |
| Carousel advance | Swipe left to go next | Swipe right to go next |
| Pull to refresh | Pull down (same in RTL) | Pull down (no change) |
| Drag to reorder | Drag left/right based on list order | Drag right/left based on list order |
| Progress animation | Animate left to right | Animate right to left |
| Skeleton loading | Shimmer left to right | Shimmer right to left |

### 9.7 Testing Protocol for RTL

1. Set interface language to Arabic (or other RTL language)
2. Verify navigation is reversed (Logo on right, nav on right)
3. Verify text alignment is right-aligned
4. Verify all directional icons are mirrored
5. Verify forms: labels on right, inputs on left
6. Verify phone number/email fields maintain LTR entry
7. Verify number entry (keypad) shows same order
8. Verify all animations reverse direction
9. Verify all gestures reverse (swipe direction)
10. Verify modals, sheets, dialogs render correctly
11. Verify scrollbar position
12. Verify touch target alignment
13. Verify with screen reader (VoiceOver RTL support)
14. Complete QA pass for every screen in RTL mode
15. Test with user-generated content in mixed directions

---

## 10. Audio Support

### 10.1 Philosophy

Audio content must be accessible to users who are deaf, hard of hearing, or have auditory processing disorders. Visual alternatives must be provided for all audio content. Audio quality must meet production standards for clarity.

### 10.2 Required Audio Alternatives

| Audio Content Type | Required Alternative |
|---|---|
| Prerecorded video | Captions + Audio description |
| Live video | Real-time captions |
| Audio-only recording | Full transcript |
| Podcast/lecture | Full transcript |
| Interactive audio (quizzes) | Visual text alternative |
| Audio notifications | Visual notification + text |
| Sound effects | Visual indicator + text description |
| Music with lyrics | Lyrics displayed synchronously |
| Background music | Can be silenced independently |

### 10.3 Audio Descriptions

**Definition:** Audio descriptions are narrated descriptions of key visual elements in video content, inserted during natural pauses in dialogue.

**Requirements:**

- All prerecorded educational video includes audio description track
- Audio description clearly describes: actions, characters, scene changes, on-screen text, graphics, and other visual information
- Audio description does not overlap with dialogue or important audio
- Default: audio description off (user can enable)
- Extended audio description available when pauses are insufficient (video pauses for description)
- Audio description voices are clear, neutral, and consistent

**Audio description script guidelines:**

| Visual Element | Description Example |
|---|---|
| Scene change | "The instructor moves to the whiteboard and draws a diagram of the water cycle." |
| On-screen text | "A slide appears titled 'Photosynthesis Formula' showing: 6CO2 + 6H2O -> C6H12O6 + 6O2" |
| Graphic/chart | "A bar chart shows reading comprehension scores improving from 65% in Week 1 to 92% in Week 8." |
| Gesture/expression | "The instructor points to the highlighted equation and nods approvingly." |
| Action | "The student clicks on the 'Submit Quiz' button, and a checkmark appears." |

### 10.4 Transcripts

**Requirements:**

- Every audio and video content piece has a complete transcript
- Transcript includes: all dialogue, speaker identification, description of relevant sounds
- Transcript is synchronized (time-stamped for reference)
- Transcript is searchable
- Transcript visible alongside or below media player
- Transcript available for download
- Transcripts meet WCAG readability guidelines

**Transcript format:**

```
[00:00:00] Narrator: Welcome to Introduction to Biology. In this lesson, we'll explore cell structure.
[00:00:05] Narrator: [A diagram of a cell appears on screen, showing the nucleus, mitochondria, and cell membrane.]
[00:00:10] Dr. Smith: The nucleus is often called the control center of the cell. It contains our DNA.
[00:00:15] Dr. Smith: [Points to the nucleus on the diagram] Let's zoom in to understand its structure.
```

### 10.5 Auto-Generated Captions

- Auto-generated captions are provided by default for all video content
- Accuracy target: 99%+ (human-reviewed for all educational content)
- Auto-captions can be edited by instructors
- Auto-captions support multiple languages
- Auto-captions include punctuation for readability
- Review queue: auto-captions flagged for low confidence sent for human review

### 10.6 Audio Cues

| Event | Audio Cue | Visual Alternative |
|---|---|---|
| New message | Short notification sound | Badge + notification banner |
| Quiz submitted | Confirmation chime | Checkmark animation + text |
| Error | Error alert sound | Error message + red indicator |
| Session timeout | Warning tone | Warning modal + countdown |
| Achievement unlocked | Celebration sound | Confetti + badge + text |
| Live session starts | Chime | Notification banner + icon |
| Recording started | Beep | Red recording indicator + text |
| Download complete | Completion sound | Download badge + text |

**Audio cue requirements:**

- Audio cues can be toggled off independently (sound effects toggle)
- Audio cues have visual alternatives (never rely on audio alone)
- Audio cue volume is independent of media volume
- Audio cues respect system Silent mode / Do Not Disturb
- Audio cues are short (< 1 second)
- Audio cues are not startling (no loud, sudden sounds)

### 10.7 VoiceOver/TalkBack Audio Ducking

- When media is playing and screen reader speaks:
  - Media volume automatically ducks (reduces to 20% volume)
  - Screen reader voice remains at full volume
  - Media volume restores when screen reader finishes speaking
- User can control ducking behavior:
  - Always duck during speech
  - Never duck
  - Duck only when media has dialogue

### 10.8 Volume Control

- In-app volume control independent of system volume
- Volume slider in media player
- Volume slider keyboard accessible
- Volume range: 0% to 100% in 1% increments
- Mute toggle available
- Separate controls for: media volume, notification volume, sound effects volume, voiceover/guide volume

### 10.9 Testing Protocol for Audio Support

1. Verify all video content has synchronized captions
2. Verify all audio content has transcripts
3. Verify audio descriptions available for key content
4. Verify transcripts are searchable and downloadable
5. Verify audio cues have visible alternatives
6. Verify sound effects toggle works independently
7. Verify audio ducking during screen reader speech
8. Verify volume controls are accessible via keyboard
9. Test with hearing aid compatibility (telecoil, Bluetooth streaming)
10. Test with user who is deaf or hard of hearing

---

## 11. Captions & Subtitles

### 11.1 Philosophy

Captions are essential for users who are deaf or hard of hearing, users in noisy environments, users learning in a non-native language, and users with auditory processing disorders. Captions must be customizable, accurate, synchronized, and complete.

### 11.2 Caption Types

| Type | Description | Content | Required For |
|---|---|---|---|
| Captions (CC) | Dialogue + speaker ID + sound descriptions | Everything audible | All video |
| Subtitles | Dialogue only (language translation) | Dialogue only | Multi-language support |
| SDH (Subtitles for Deaf and Hard of Hearing) | Dialogue + speaker ID + sound descriptions + music notes | Everything audible | Accessibility compliance |
| Transcript | Full text with timestamps | Everything in complete document | Audio and video |

### 11.3 Caption Customization Options

| Setting | Options | Default | WCAG AAA |
|---|---|---|---|
| Font size | 50% — 200% in 10% increments | 100% | Yes |
| Font family | Sans-serif, Serif, Monospace, Dyslexic | Sans-serif | Yes |
| Font color | White, Yellow, Green, Cyan, Custom | White | Yes |
| Font weight | Normal, Bold | Normal | Yes |
| Background color | Black, Semi-transparent, Custom | Semi-transparent black | Yes |
| Background opacity | 0% — 100% in 10% increments | 75% | Yes |
| Text shadow | None, Subtle, Strong | Subtle | Yes |
| Position | Bottom, Top, Overlay (custom Y position) | Bottom | Yes |
| Max lines | 1 — 4 | 2 | Yes |
| Character edge style | None, Drop shadow, Raised, Outline, Uniform | Drop shadow | Yes |
| Window color | None, Black, White, Custom | None | Yes |
| Language tracks | All available languages | Content language | Yes |

### 11.4 Caption Content Requirements

| Element | Included? | Format |
|---|---|---|
| Dialogue | Yes | What is spoken, verbatim |
| Speaker identification | Yes | "[Speaker Name]: " or positioned label |
| Sound effects | Yes | [Bell ringing], [Door closes], [Thunder] |
| Music | Yes | [Music playing] or [Song Name] |
| Emphasis | Yes | Italics for emphasized words |
| Off-screen dialogue | Yes | "[Speaker Name (off-screen)]: " |
| Multiple speakers | Yes | Each speaker identified |
| Foreign language | Yes | Translation in brackets [speaking French] |
| Silence | When significant | [Silence] or [Pause] |
| Whisper | Yes | "(whispering) text" |
| Screaming | Yes | "TEXT" (all caps, avoid if possible) |
| Fast speech | Yes | "text — text — text" (hyphens) |
| Overlapping dialogue | Yes | "- Dialogue A" on left, "- Dialogue B" on right |

### 11.5 Caption Formatting Guidelines

| Rule | Specification |
|---|---|
| Characters per line | Maximum 42 characters (SDH standard) |
| Lines per caption | Maximum 2 lines (3 for SDH if needed) |
| Duration minimum | 1 second for shortest caption |
| Duration maximum | 7 seconds per caption (split longer) |
| Frame rate | Match video frame rate |
| Synchronization | +/- 2 frames accuracy |
| Reading speed | Maximum 160 words per minute (children: 120 wpm) |
| Line breaks | At phrase boundaries, not mid-phrase |
| Paragraph breaks | Same-speaker continuous dialogue: combine |
| Background clarity | Solid or semi-transparent background behind text |

### 11.6 Auto-Generated Captions

- Auto-generated via speech-to-text engine
- Human review for all educational content
- Accuracy target: 99%+
- Auto-generated confidence score displayed to reviewer
- Flagged low-confidence segments prioritize human review
- Punctuation auto-inserted (periods, commas, question marks)
- Speaker diarization (identification of different speakers)
- Support for: English, Arabic, Spanish, French, German, Portuguese, Russian, Chinese, Japanese, Korean, Hindi, Urdu

### 11.7 Multiple Language Tracks

- Captions available in all languages the platform supports
- Users can select any available language for captions
- Default: content language
- Remember last selected language across sessions
- Language selector visible and accessible in media player
- Language list ordered by relevance (user's language first)

### 11.8 Synchronized Highlighting

- Current word or phrase highlighted as it is spoken
- Highlight text in caption as audio progresses
- Highlight color: bright yellow or white on dark background
- Smooth highlight progression (per-word, not per-caption-block)
- Synchronization accuracy: +/- 50ms
- Accessible alternative: progress bar on caption timeline

### 11.9 Testing Protocol for Captions

1. Verify captions appear synchronized with audio (+/- 2 frames)
2. Verify all dialogue is captioned
3. Verify speaker identification is present
4. Verify sound effects are described in brackets
5. Test each caption customization option
6. Verify caption position does not obscure important content
7. Verify multiple language tracks work
8. Verify auto-generated caption accuracy (99%+ target)
9. Verify reading speed (max 160 wpm)
10. Test with screen reader (captions read aloud)
11. Test on all devices (phone, tablet, desktop, TV)
12. Verify SDH format includes full auditory information

---

## 12. Focus Indicators

### 12.1 Philosophy

Focus indicators are the primary way keyboard and switch control users navigate the interface. A visible, predictable focus indicator is essential for accessibility. Without it, users cannot know where they are or what they can interact with.

### 12.2 Focus Indicator Requirements

| Requirement | Specification |
|---|---|
| Visible on all interactive elements | Every button, link, input, select, textarea, custom control |
| Minimum size | 2px solid outline |
| Offset from element | 4px minimum |
| Contrast against all backgrounds | 3:1 minimum (against element background and page background) |
| Not relying on color alone | Must be distinguishable even for color blind users |
| Appears on all input methods | Tab (keyboard), tap (touch), click (mouse) |
| Never removed | Default focus indicator never removed without custom replacement |
| Inside components | Cards, list items have focus indicator with proper scope |
| Smooth appearance | Transition: outline-opacity 100ms ease |

### 12.3 Focus Indicator Specifications by Component

| Component | Focus Indicator | Implementation |
|---|---|---|
| Button | 2px solid outline + 4px offset | `outline: 2px solid #0057B3; outline-offset: 4px;` |
| Link (inline) | 2px solid outline + 2px offset | `outline: 2px solid #0057B3; outline-offset: 2px;` |
| Text input | 2px solid outline on container | `outline: 2px solid #0057B3;` or border change with 3:1 contrast |
| Select | 2px solid outline + 4px offset | Same as button |
| Checkbox/Radio | 2px solid outline + 2px offset | Outline on the control itself |
| Card | 2px solid outline + 4px offset on card container | Entire card has visible focus |
| List item | 2px solid outline + 2px offset on item | Each interactive list item |
| Tab | 2px solid outline + 2px offset | Focus on active tab |
| Slider | 2px solid outline on thumb | Focus ring around thumb handle |
| Modal container | 2px solid outline on first focusable element | Focus ring inside modal |
| Icon button | 2px solid outline + 4px offset | Outline around the icon (not label) |
| Toolbar | 2px solid outline on active tool | Focus on currently focused tool |
| Menu item | 2px solid outline + 2px offset | Outline on menu item |
| Tree item | 2px solid outline + 2px offset | Outline on tree item row |

### 12.4 Focus Indicator Colors

| Background | Outline Color | Hex |
|---|---|---|
| Light background | Deep Blue | #0057B3 |
| Dark background | Light Blue | #90CAF9 |
| Colored background (light) | Deep Blue | #0057B3 |
| Colored background (dark) | Light Blue | #90CAF9 |
| High contrast mode (light) | Blue | #0057B3 |
| High contrast mode (dark) | Yellow | #FFD600 |
| Windows High Contrast | System Highlight | Highlight |

### 12.5 Focus Order Management

- Focus starts at top of page (skip link first)
- Tab order follows visual layout: left-to-right, top-to-bottom in LTR
- Tab order in RTL: right-to-left, top-to-bottom
- No focus jumps (focus moves to logically next element)
- Tab order skips visually hidden elements
- Tab order includes off-screen focusable elements only if logically next in order

**Focus sequence by page region:**
1. Skip link (visible on focus)
2. Primary navigation
3. Search bar
4. Main content (in reading order)
5. Sidebar/complementary content
6. Footer navigation
7. Footer links

### 12.6 Focus Management in Special Cases

| Case | Focus Behavior |
|---|---|
| Modal opens | Focus moves to first interactive element in modal |
| Modal closes | Focus returns to element that triggered modal |
| Sheet opens | Focus moves to first interactive element in sheet |
| Sheet closes | Focus returns to element that triggered sheet |
| New content loaded | Focus moves to new content (or "Content loaded" announcement) |
| Error on form | Focus moves to first field with error |
| Inline error | Focus stays on field, error announced |
| Tab panel changes | Focus remains on tab, content updates |
| Accordion toggles | Focus remains on accordion button |
| Menu opens | Focus moves to first menu item |
| Menu closes | Focus returns to menu trigger |
| Auto-complete list | Focus stays on input, arrow keys navigate suggestions |
| Infinite scroll | Focus stays on current position, new content appears above/below |
| Page transition (SPA) | Focus moves to main content heading |
| Drag starts | Focus moves to drag handle or source |
| Drag ends | Focus returns to final position |

### 12.7 Never Remove Default Focus Without Replacement

```css
/* NEVER: */
*:focus {
  outline: none;
}

/* ALWAYS: provide custom focus indicator */
*:focus-visible {
  outline: 2px solid #0057B3;
  outline-offset: 4px;
}
```

### 12.8 Focus Indicator Testing Protocol

1. Navigate entire application using Tab key only
2. Verify focus indicator visible on every interactive element
3. Verify focus indicator has sufficient contrast on all backgrounds
4. Verify focus indicator offset is consistent
5. Verify focus order follows visual layout
6. Verify focus management in modals (trap + return)
7. Verify focus management in sheets
8. Verify focus moves to errors on form validation
9. Test focus in high contrast mode
10. Test focus with screen reader active
11. Test focus with Windows High Contrast mode
12. Verify no element receives focus that should not (hidden, disabled, decorative)

---

## 13. Touch Targets

### 13.1 Philosophy

Touch targets must be large enough and well-spaced enough for users of all hand sizes, with limited dexterity, using assistive touch devices, or operating the device with non-preferred hand. Small, crowded targets cause errors, frustration, and exclusion.

### 13.2 Target Size Requirements

| Criteria | Minimum | Recommended | WCAG Reference |
|---|---|---|---|
| Touch target size | 44x44 CSS pixels | 48x48 CSS pixels | WCAG 2.5.5 Target Size (AA) |
| Spacing between targets | 8px minimum | 12px minimum | N/A |
| Inline links | 44x44 tap area via padding | 44x44 | Exception (AA) |
| Scrollable list items | Full row height: 44pt minimum | 48pt | N/A |
| Interactive cards | Card itself should be tappable | Entire card surface | N/A |

### 13.3 Touch Target Sizes by Component

| Component | Target Size | Notes |
|---|---|---|
| Primary button | 48pt height minimum | Width flexible to content |
| Secondary button | 44pt height minimum | Width flexible to content |
| Icon button (standalone) | 44x44pt minimum | Larger recommended: 48x48pt |
| Icon button (in toolbar) | 44x44pt minimum | Ensure adequate spacing |
| Link (inline) | 44x44pt tap area | Achieved via padding around link text |
| Form input | 44pt height minimum | Width flexible |
| Checkbox/Radio | 44x44pt tap area | Actual control can be smaller with padding |
| Switch/Toggle | 44pt height minimum | Minimum 44x28pt |
| Slider thumb | 44x44pt minimum | Larger for precision: 48x48pt |
| Segmented control | 44pt height minimum | Each segment touchable |
| Tab | 44pt height minimum | Width proportional to content |
| Bottom tab bar icon | 48x48pt minimum | Actual hit area |
| Navigation bar button | 44x44pt minimum | Both left and right |
| Close/X button | 44x44pt minimum | Critical for dismissal |
| Back button | 44x44pt minimum | Critical for navigation |
| Pull to refresh | 44pt trigger area | Pull distance threshold |
| Drag handle | 44x44pt minimum | For reorderable lists |
| Dropdown chevron | 44x44pt minimum | Or make entire row tappable |
| Badge | Not tappable alone | Associated element handles tap |
| Tooltip close | 44x44pt minimum | If dismissible by tap |

### 13.4 Spacing Guidelines

| Context | Minimum Spacing | Rationale |
|---|---|---|
| Buttons in a row | 12px between | Prevents mis-taps |
| Form fields | 16px between fields | Adequate separation |
| List items | 8px between items | Edge of item is target boundary |
| Bottom tab items | 0px (full width) | Each item large enough by default |
| Icon toolbar | 12px minimum between icons | Prevents wrong-icon taps |
| Cards in grid | 16px between cards | Card boundary is target edge |
| Links in paragraph | Determined by line-height | Adequate line spacing |
| Radio/Checkbox groups | 8px between options | Clear separation of options |

### 13.5 Visual Boundary vs. Tappable Area

- The visual boundary of a touch target must match (or be smaller than) the tappable area
- Tappable area must never be smaller than the visual boundary
- Use `::before` pseudo-element or `padding` to extend tappable area without changing visual size
- Touch highlight (tap state) should fill the tappable area

**Inline link tap area example:**

```css
/* Tappable area extends beyond visual text */
.inline-link {
  padding: 12px 4px; /* 44pt tap area with 20pt font */
  margin: -12px -4px; /* Offset padding visually */
}
```

### 13.6 Touch Target States

| State | Visual Feedback | Timing |
|---|---|---|
| Default | Normal appearance | — |
| Hover (desktop) | Slight color change (2% brightness) | 100ms |
| Active/Pressed | Darker background or scale(0.97) | 50ms |
| Focus (keyboard) | Outline indicator | Instant |
| Disabled | Reduced opacity (40%) | — |
| Selected | Highlighted background | — |
| Error | Red border/bg + icon | Instant |

### 13.7 Testing Protocol for Touch Targets

1. Measure all touch targets in the interface
2. Verify minimum 44x44pt for all interactive elements
3. Verify inline links have 44x44pt tap area via padding
4. Verify minimum 8pt spacing between adjacent targets
5. Test on physical devices (not just simulator)
6. Test one-handed operation (thumb reach)
7. Test with larger fingers
8. Test with moisture on screen (finger slips)
9. Test with accessibility features: AssistiveTouch, Switch Control
10. Test with voice control (target identification)
11. Verify no overlapping touch targets
12. Verify tappable area matches visual boundary

---

## 14. Assistive Technology Support

### 14.1 Philosophy

The platform must work with the full ecosystem of assistive technologies beyond screen readers. Users navigate the platform through switches, voice commands, eye tracking, and other alternative input methods. Every feature must be accessible through every input modality.

### 14.2 Supported Assistive Technologies

| Technology | Platform | Use Case |
|---|---|---|
| Switch Control | iOS, iPadOS, macOS | Users with limited mobility; single-switch scanning |
| Voice Control | iOS, iPadOS, macOS | Users who cannot use hands; voice commands |
| Full Keyboard Access | iPadOS | Users who prefer or require keyboard navigation |
| Eye Tracking (iOS 18+) | iOS, iPadOS | Users who navigate with eye gaze |
| AssistiveTouch | iOS, iPadOS | Users with limited dexterity; custom gestures |
| Dwell Control | macOS | Users who click by holding gaze |
| Head Tracking | iOS, iPadOS | Users who navigate with head movements |
| Third-party switch devices | All | External hardware compatibility |
| Siri Shortcuts | iOS, iPadOS | Automation and voice commands |
| Voice Access | Android | Voice control for Android devices |
| Switch Access | Android | Switch scanning for Android devices |

### 14.3 Switch Control (iOS) Requirements

Switch Control allows users to navigate by sequentially scanning through elements on screen and activating with a switch.

**Requirements:**

- All interactive elements must be reachable via Switch Control scanning
- Grouping of related elements (do not scan every word in a paragraph)
- Order of scanning follows visual layout (logical progression)
- No elements that require multi-touch gestures (pinch, rotate)
- Custom controls must be accessible as individual items
- Point Mode (direct selection via switch) supported where appropriate
- Item mode scanning: items highlighted one at a time
- Activation via: switch press, sound, or camera detection

**Testing:**

1. Enable Switch Control (Settings -> Accessibility -> Switch Control)
2. Add a switch (camera, screen tap, or Bluetooth switch)
3. Navigate through all screens using scanning
4. Verify all interactive elements are reachable
5. Verify scanning order is logical
6. Verify all actions can be performed
7. Verify no elements are skipped or unreachable

### 14.4 Voice Control (iOS/macOS) Requirements

Voice Control allows users to control the device entirely with voice commands.

**Requirements:**

- Every interactive element has a unique, predictable name
- Numbered labels visible on all interactive elements (for overlay navigation)
- `accessibilityLabel` must match the visible text (voice commands use visible labels)
- Items with the same label must be differentiated (numbered or contextual)
- Custom actions available via voice commands
- Voice Control grid overlay works for targeting
- Commands: "Tap [label]", "Show Numbers", "Show Grid", "Open [app]", "Scroll [direction]"

**Voice command compatibility:**

| Action | Voice Command | Implementation |
|---|---|---|
| Tap button | "Tap Submit" | Button label = "Submit" |
| Tap link | "Tap Learn More" | Link text = "Learn More" |
| Scroll down | "Scroll down" | Native scrolling |
| Go back | "Go Back" or "Tap Back" | Back button label = "Back" |
| Open notifications | "Open Notifications" | Notification button labeled "Notifications" |
| Search | "Tap Search" | Search field accessible |

**Testing:**

1. Enable Voice Control (Settings -> Accessibility -> Voice Control)
2. Say "Show numbers" — verify all elements have numbered labels
3. Say "Tap [number]" — verify correct element activates
4. Say common commands: "Go back", "Scroll down", "Open [section]"
5. Verify all critical flows work with voice alone
6. Verify no ambiguous labels (two elements with same label)

### 14.5 Full Keyboard Access (iPadOS) Requirements

iPadOS Full Keyboard Access allows users to navigate and control the iPad entirely with an external keyboard.

**Requirements:**

- All features work with keyboard shortcuts (see Section 5.4)
- Tab navigation works to all interactive elements
- Arrow key navigation works for lists, grids, sliders
- Focus indicator visible at all times (system focus ring)
- Custom keyboard shortcuts documented
- No keyboard conflicts (app shortcuts do not conflict with system shortcuts)

### 14.6 Eye Tracking (iOS 18+) Requirements

Eye Tracking allows users to navigate and control the device using only their eye gaze.

**Requirements:**

- All interactive elements must have adequate size for eye gaze targeting (minimum 44x44pt, recommended 48x48pt)
- Adequate spacing between targets (minimum 12pt recommended)
- Dwell duration for activation: default 500ms (configurable)
- Visual feedback during dwell: progress indicator showing dwell time
- Elements must not change position (no floating or moving elements that would confuse eye tracking)
- Dwell activation should be the primary interaction mode
- No interactions requiring precise targeting (tiny buttons, closely packed links)

**Optimizing for eye tracking:**

- Place primary actions in predictable locations
- Large, well-spaced touch targets
- Consistent layout (no layout shifts)
- High contrast between targets and background
- Generous padding around all interactive elements

**Testing:**

1. Enable Eye Tracking (Settings -> Accessibility -> Eye Tracking)
2. Calibrate eye tracking
3. Navigate through all screens using eye gaze
4. Verify all elements are reachable with comfortable dwell time
5. Verify no elements are too small for eye gaze targeting
6. Verify no rapid layout changes during dwell

### 14.7 AssistiveTouch Requirements

AssistiveTouch provides an on-screen menu for users who have difficulty using physical buttons or gestures.

**Requirements:**

- All common gestures available through AssistiveTouch menu
- Custom gestures supported
- Single-tap alternatives for multi-touch gestures (pinch, rotate, swipe)
- AssistiveTouch menu does not obscure critical content

### 14.8 Third-Party Switch Devices

- Support for Bluetooth switch devices (Accessibility Switch)
- Support for Made for iPhone (MFi) switch hardware
- Common switch actions: Select, Home, Notification Center, Siri
- Scanning speed adjustable
- Switch configuration available in accessibility settings

### 14.9 Dwell Control (macOS)

- Users can click by holding the cursor still for a configurable duration
- All interactive elements must be reachable by cursor
- Dwell duration: configurable (default 500ms)
- Visual indication of dwell progress
- Preview of dwell target (element highlighting)

### 14.10 Testing Protocol for Assistive Technology

1. Test with Switch Control: scanning, activation, all screens
2. Test with Voice Control: numbered labels, voice commands, all actions
3. Test with Full Keyboard Access: tab navigation, shortcuts
4. Test with Eye Tracking: target size, dwell, activation
5. Test with AssistiveTouch: custom gestures, menu navigation
6. Test with Dwell Control (macOS): cursor targeting
7. Test with at least one third-party switch device
8. Test all critical flows across each assistive technology
9. Document any issues per assistive technology type
10. Test with users who rely on each assistive technology

---

## 15. Cognitive Accessibility

### 15.1 Philosophy

Cognitive accessibility addresses the diverse ways people process information, maintain focus, and interact with digital content. The platform provides customizable experiences for users with dyslexia, ADHD, autism, and other cognitive differences. A flexible, predictable, and clear interface benefits all users.

### 15.2 Simplify Mode

A configurable mode that reduces visual complexity and cognitive load.

**Features:**

- Reduced UI: non-essential elements hidden (decorative graphics, secondary modules, non-critical actions)
- Focused content: primary content takes full width, no sidebars
- No animations: all motion disabled (see Motion Reduction)
- Clean layout: minimal visual elements, generous whitespace
- Single task mode: one task visible at a time (wizard-style flow)
- Limited choices: maximum 3-4 options at a time
- Simplified navigation: back/next only, no complex menus
- Larger text: 125% of default font size minimum
- Higher contrast: 7:1 text contrast minimum
- Reduced color palette: monochromatic or limited colors
- Fewer transitions: instant page changes, no sliding/animations
- Clear labels: no jargon, no abbreviations, explicit language

**Toggle:** User can enable/disable Simplify Mode from: Accessibility settings, Quick settings panel, Onboarding preference.

### 15.3 Reading Support Mode

Features designed to improve reading comprehension and focus.

| Feature | Description | Implementation |
|---|---|---|
| Highlight as read | Text highlights as user reads, tracking reading position | Line-by-line or phrase-by-phrase highlighting |
| Line focus mode | Dims all lines except the one being read | Current line at full opacity, others dimmed to 30% |
| Syllable highlighting | Syllables visually separated for decoding | Color alternation or subtle separation |
| Reading ruler | Horizontal overlay bar that moves with reading position | Semi-transparent bar highlighting current line |
| Text spacing | Increased letter, word, and line spacing | Configurable spacing sliders |
| Column width | Narrower text columns for easier tracking | Max 60-70 characters per line |
| Font options | OpenDyslexic, monospace, or high-readability fonts | Multiple font options |
| Background color | Colored overlays to reduce visual stress | Configurable: cream, light blue, light green, etc. |

### 15.4 Dyslexia-Friendly Mode

Specific optimizations for users with dyslexia.

**Settings:**

| Setting | Dyslexia-Friendly Value | Benefit |
|---|---|---|
| Font | OpenDyslexic or Dyslexie | Weighted letterforms reduce letter reversal |
| Font size | 18pt minimum | Larger text reduces crowding |
| Letter spacing | 0.35em (35% of font size) | Reduced crowding, fewer letter reversals |
| Word spacing | 0.5em | Clear word boundaries |
| Line height | 1.8 (180%) | Prevents line skipping |
| Paragraph spacing | 2em | Clear paragraph boundaries |
| Background | Cream (#FFF8E7) or Light Blue (#E8F0FE) | Reduced visual stress vs white |
| Text color | Dark Gray (#2D2D2D) | Softer than pure black |
| Column width | Max 65 characters | Easier eye tracking |
| Text alignment | Left-aligned (never justified) | Consistent word spacing |
| Bold text | Enabled | Heavier letters easier to distinguish |
| Underline links | Enabled | Clear link identification |
| Avoid italics | Convert to bold | Italics harder to read |
| Avoid all caps | Normal case | All caps harder to recognize |

**Additional features:**

- Bionic Reading: bold first half of each word (user can toggle)
- Spell-check with dyslexia-aware algorithms
- Grammar suggestions focused on common dyslexic error patterns
- Read-aloud with synchronized highlighting
- Speech-to-text for writing input
- Word prediction

### 15.5 ADHD-Friendly Mode

Optimizations for users with attention deficit hyperactivity disorder.

**Features:**

- Single task focus: only one primary action per screen
- Timer integration: Pomodoro-style work/break timer
- Break reminders: periodic reminders to take breaks (configurable interval)
- Distraction blocking: notification suppression during focus sessions
- Progress indicators: clear, visible progress toward goals
- Immediate feedback: actions provide instant, clear response
- Minimal choices: 2-3 options maximum per decision point
- Estimated time: each task shows estimated completion time
- Step breakdown: complex tasks broken into visible, completable steps
- Reward system: visual celebration for completing tasks
- Deadline mode: countdown timer, high urgency visuals
- Sound cues: completion sounds, reminder chimes

**Interface adjustments:**

- High-contrast mode enabled by default
- Strong color coding for task states (to-do / doing / done)
- Clear headings and sections
- No parallax, floating, or distracting animations
- Consistent navigation (no hidden menus)
- Save state: everything auto-saves, no lost work
- Undo available for all actions

### 15.6 Autism-Friendly Mode

Optimizations for users on the autism spectrum.

**Features:**

- Predictable layouts: consistent page structure across all screens
- Literal language: no idioms, metaphors, or figurative speech
- No surprises: no auto-playing media, no pop-ups without user action
- Sensory considerations: no flashing lights, no loud sounds, no intense colors
- Clear routines: visible, predictable flow through tasks
- Concrete information: direct, factual content presentation
- Visual schedules: step-by-step visual guides for multi-step processes
- Social stories: for features involving social interaction (forums, group work)
- Reduced sensory input: no background patterns, no gradients, no noise textures
- Explicit instructions: each step clearly described, no inferred actions
- Warning for changes: advance notice of layout or content changes
- Choice presentation: options presented as clear, distinct choices
- Routine support: ability to save and repeat workflows
- Comfortable pace: no countdown timers, no rushing elements
- Safe exploration: undo always available, no permanent consequences for exploration

**Interface adjustments:**

- Muted color palette: soft, desaturated colors (no bright neons)
- No auto-advancing content: carousels, slideshows are manual
- No unexpected sound effects: all sounds opt-in
- Clear content boundaries: well-defined sections with visible borders
- One thing at a time: sequential presentation of information
- Explicit labels: "This is a button. Press it to continue."
- Familiar patterns: consistent iconography, no abstract symbols

### 15.7 Cognitive Accessibility Universal Design Patterns

| Pattern | Description | Benefits |
|---|---|---|
| Predictable navigation | Same navigation structure everywhere | All cognitive types |
| Clear headings | Descriptive, concise section titles | All cognitive types |
| Plain language | Simple vocabulary, short sentences | All cognitive types |
| Consistent terminology | Same word for same concept everywhere | All cognitive types |
| Chunking | Information presented in small, digestible pieces | All cognitive types |
| Progressive disclosure | Start simple, reveal complexity gradually | All cognitive types |
| Error prevention | Confirm before destructive actions | All cognitive types |
| Forgiveness | Undo, recover, redo available | All cognitive types |
| Visual schedules | Step-by-step progress indicators | Autism, ADHD |
| Immediate feedback | Clear, instant response to actions | ADHD, all cognitive types |
| Reduced choices | 3-4 options maximum | All cognitive types |
| White space | Generous spacing between elements | Dyslexia, all cognitive types |
| Icons with text | Icon + text label for all controls | All cognitive types |
| Save state | Auto-save, no lost work | ADHD, anxiety |
| Time estimates | How long each task will take | ADHD |
| Break reminders | Periodic rest suggestions | ADHD |
| Reading tools | Highlight, line focus, text spacing | Dyslexia |
| Simplify mode | Reduced UI, no distractions | All cognitive types |
| Literal language | No metaphors or idioms | Autism |
| Social stories | Explanations of social features | Autism |

### 15.8 Testing Protocol for Cognitive Accessibility

1. Enable Simplify Mode and verify reduced UI
2. Enable Dyslexia-Friendly Mode and verify font/spacing changes
3. Enable ADHD-Friendly Mode and verify focus tools
4. Enable Autism-Friendly Mode and verify predictability
5. Test all reading support features (highlight, line focus, ruler)
6. Verify plain language in all interface text
7. Verify consistent terminology across all screens
8. Verify no flashing or auto-playing content
9. Verify undo/redo available for all actions
10. Test with users who have cognitive disabilities
11. Verify all modes can be enabled/disabled independently
12. Verify preferences persist across sessions

---

## 16. Testing Protocol

### 16.1 Philosophy

Accessibility testing is not a one-time event. It is a continuous, multi-layered practice integrated into every stage of development. Testing must be automated for speed, manual for depth, and user-driven for validity.

### 16.2 Testing Layers

```
Layer 1: Automated (CI/CD — every commit)
Layer 2: Manual Expert (every feature)
Layer 3: Assistive Technology (every release)
Layer 4: User Testing (every major release)
Layer 5: Regression (every build)
```

### 16.3 Automated Testing

#### 16.3.1 Tools

| Tool | Purpose | Integration |
|---|---|---|
| axe-core | Comprehensive accessibility audit | Unit tests, E2E tests, CI/CD |
| Lighthouse | Performance + accessibility score | CI/CD, PR checks |
| WAVE | Visual accessibility audit | Browser extension |
| Pa11y | Automated accessibility testing | CI/CD |
| ESLint-plugin-jsx-a11y | Code-level accessibility rules | IDE, pre-commit |
| Android Accessibility Test Framework | Android accessibility checks | Unit tests, CI/CD |
| XCUITest Accessibility | iOS accessibility checks | Unit tests, CI/CD |
| Color contrast checker | Automated contrast verification | CI/CD, design tokens |
| HTML validator | Semantic HTML validation | CI/CD |
| ARIA validator | ARIA usage validation | CI/CD |

#### 16.3.2 Automated Checks (Every Commit)

| Check | Tool | Threshold |
|---|---|---|
| All images have alt text | axe-core, Lighthouse | 100% pass |
| Color contrast ratios | axe-core | 4.5:1 (AA), 7:1 (AAA) |
| ARIA attributes valid | axe-core | 0 violations |
| Heading hierarchy correct | axe-core | 0 violations |
| Form labels associated | axe-core | 0 violations |
| Landmarks present | axe-core | banner, main, contentinfo |
| Skip link present | axe-core | Present and functional |
| Tab order logical | axe-core | 0 violations |
| Focus indicator visible | axe-core | 0 violations |
| Language attribute set | axe-core | 0 violations |
| Link text descriptive | axe-core | 0 violations |
| List markup correct | axe-core | 0 violations |
| Semantic HTML used | axe-core | 0 violations |
| HTML validation | W3C Validator | 0 errors |

#### 16.3.3 CI/CD Pipeline Integration

```
Pre-commit hook:
  - ESLint a11y rules

Commit -> PR created:
  - Automated accessibility tests
  - Lighthouse score check (min 90)
  - Color contrast check
  - HTML validation

PR merged -> Deploy to staging:
  - Full axe-core scan
  - Pa11y scan
  - WAVE evaluation report

Release -> Production:
  - Full accessibility audit report
  - Screen reader recording comparison
  - Regression test suite
```

### 16.4 Manual Testing

#### 16.4.1 Keyboard-Only Navigation

| Test Case | Verification |
|---|---|
| Tab through all interactive elements | All elements reachable, no traps |
| Shift+Tab reverses correctly | Logical reverse order |
| Enter/Space activates focused elements | All elements work |
| Arrow key navigation in complex components | Lists, tabs, sliders, grids work |
| Escape closes modals, dialogs, menus | All overlays close |
| Skip link appears on first Tab | Visible on focus |
| Focus indicator visible on every element | 2px + 4px offset |

#### 16.4.2 Screen Reader Testing

| Screen Reader | Platform | Test Flow |
|---|---|---|
| VoiceOver | macOS + Safari | Navigate full learning flow |
| VoiceOver | iOS + Safari | Navigate full learning flow |
| TalkBack | Android + Chrome | Navigate full learning flow |
| NVDA | Windows + Firefox | Navigate full learning flow |
| JAWS | Windows + Chrome | Navigate full learning flow |

**Test checklist per screen reader:**

- Navigate by heading (h1-h6)
- Navigate by landmark (banner, main, navigation, contentinfo)
- Navigate by link
- Navigate by form control
- Navigate by table
- Use Rotor / Element List to find content
- Verify dynamic content announcements (live regions)
- Verify form validation announcements
- Verify modal/dialog announcements
- Verify custom control roles and states
- Verify alt text for images
- Verify skip link functionality

#### 16.4.3 Zoom Testing

| Zoom Level | Verification |
|---|---|
| 100% | Default layout, all content visible |
| 125% | Text wraps, no truncation |
| 150% | Single column if multi-column, no horizontal scroll |
| 175% | Single column, generous spacing |
| 200% | All content visible, functional, no horizontal scroll |

#### 16.4.4 High Contrast Mode Testing

- macOS: System Settings -> Accessibility -> Display -> Increase Contrast
- iOS: Settings -> Accessibility -> Display & Text Size -> Increase Contrast
- Windows: Settings -> Ease of Access -> High Contrast
- Verify all text readability (7:1 target)
- Verify all interactive states visible
- Verify no loss of information

#### 16.4.5 Color Blind Simulation

- Chrome DevTools -> Rendering -> Emulate vision deficiencies
- Test: Protanopia, Deuteranopia, Tritanopia, Achromatopsia
- Verify no information conveyed by color alone
- Verify charts, status indicators, and error states distinguishable

#### 16.4.6 Reduced Motion Testing

- macOS: System Settings -> Accessibility -> Display -> Reduce Motion
- iOS: Settings -> Accessibility -> Motion -> Reduce Motion
- Verify all animations disabled
- Verify instant state changes
- Verify no loss of feedback (color/opacity changes still present)

#### 16.4.7 RTL Layout Testing

- Set language to Arabic
- Complete full QA pass for every screen
- Verify layout mirroring
- Verify directional icons
- Verify text alignment
- Verify gestures

### 16.5 User Testing

#### 16.5.1 Participant Recruitment

| Disability Type | Minimum Participants | Recruited Through |
|---|---|---|
| Blind (screen reader users) | 3-5 | Accessibility organizations |
| Low vision (zoom, high contrast) | 2-3 | Accessibility organizations |
| Deaf or hard of hearing | 2-3 | Deaf community organizations |
| Motor disability (switch, voice) | 2-3 | Disability organizations |
| Cognitive disability | 2-3 | Cognitive accessibility groups |
| Color blind | 2-3 | General recruitment + screening |

#### 16.5.2 Testing Protocol

1. Provide participants with task scenarios
2. Observe without guiding (note pain points)
3. Record screen + audio (with permission)
4. Collect qualitative feedback (what worked, what did not)
5. Collect quantitative data (task completion rate, time on task, error rate)
6. Prioritize issues by severity
7. Create accessibility issue report with reproduction steps

#### 16.5.3 Metrics

| Metric | Target | Measurement |
|---|---|---|
| Task completion rate | 100% | Task completed without assistance |
| Error rate | < 5% | Errors per task |
| Time on task | Comparable to non-disabled users | Time measurement |
| User satisfaction | 4/5+ | Post-task satisfaction rating |
| Critical issues | 0 | Issues blocking task completion |
| Serious issues | < 3 | Issues significantly impeding task |

### 16.6 Accessibility Acceptance Criteria

Every user story must include accessibility acceptance criteria:

**Template:**

```
Accessibility:
  - [ ] All interactive elements are keyboard accessible
  - [ ] All images have appropriate alt text
  - [ ] Color is not the only means of conveying information
  - [ ] Touch targets meet 44x44pt minimum
  - [ ] Focus indicator visible on all elements
  - [ ] Screen reader announces all content correctly
  - [ ] All text meets minimum contrast ratio (4.5:1)
  - [ ] Content reflows correctly at 200% zoom
  - [ ] Animations respect prefers-reduced-motion
  - [ ] All forms have associated labels and error messages
```

### 16.7 Severity Classification

| Severity | Definition | Response |
|---|---|---|
| Critical | User cannot complete essential task | Block release, fix immediately |
| Serious | User can complete task with significant difficulty | Fix in current sprint |
| Moderate | User can complete task but with inconvenience | Fix within 2 sprints |
| Minor | User can complete task, minor annoyance | Fix within backlog |
| Enhancement | Current behavior meets standards but can be improved | Consider for roadmap |

### 16.8 Regression Testing

- Full accessibility suite run before every release
- Automated checks in CI/CD
- Manual spot-check of critical flows
- Compare against baseline (previous release)
- Document any regressions with reproduction steps

---

## 17. Accessible Components Documentation

### 17.1 Overview

Every component in the design system must have documented accessibility behavior including: ARIA roles, keyboard interactions, focus management, screen reader announcements, touch target size, and accessible name computation.

### 17.2 Component: Button

| Property | Specification |
|---|---|
| ARIA role | `role="button"` (native `<button>` preferred) |
| Accessible name | Visible text content or `aria-label` for icon buttons |
| Keyboard | Space, Enter to activate |
| Focus | Visible 2px outline + 4px offset |
| Touch target | 44pt minimum height (48pt recommended) |
| States | default, hover, focus, active, disabled, loading |

**Screen reader announcement:** "[Button label], button" (or "[Button label], button, disabled" if disabled)

**Usage:**
- Use native `<button>` element (semantic, focusable, keyboard accessible by default)
- For icon-only buttons: `aria-label="[action description]"`
- Disabled state: `aria-disabled="true"` (use `disabled` attribute on native `<button>`)
- Loading state: `aria-busy="true"`, spinner has `aria-label="Loading"`
- Link-style buttons: use `<button>` styled as link, never `<a>` for actions

### 17.3 Component: Link

| Property | Specification |
|---|---|
| ARIA role | `role="link"` (native `<a>` preferred) |
| Accessible name | Visible link text or `aria-label` |
| Keyboard | Enter to navigate |
| Focus | Visible 2px outline + 2px offset |
| Touch target | 44x44pt tap area via padding |

**Screen reader announcement:** "[Link text], link"

**Usage:**
- Use native `<a>` with `href` for navigation
- Never use `<a>` without `href` (use `<button>` instead)
- Link text must describe destination (never "click here")
- External links: `aria-label="[text] (opens in new tab)"`, `target="_blank"` with indicator
- Download links: `aria-label="[filename] (download)"`
- Skip link: "Skip to main content" — first focusable element

### 17.4 Component: Modal/Dialog

| Property | Specification |
|---|---|
| ARIA role | `role="dialog"` or `role="alertdialog"` (for alerts) |
| ARIA properties | `aria-modal="true"`, `aria-labelledby` (heading), `aria-describedby` (description) |
| Keyboard | Tab (cycle), Shift+Tab (reverse), Escape (close) |
| Focus management | Trap focus within modal; focus first element on open; return to trigger on close |
| Screen reader announcement | "[Dialog title] dialog. [Description]" |

**States:** open, closed

**Implementation:**
- Focus trap: Tab cycles within modal, does not exit
- Escape always closes the modal
- Close button visible and keyboard accessible (X button or Cancel)
- `aria-labelledby` references modal title
- `aria-describedby` references modal description (if any)
- Background content: `aria-hidden="true"` while modal is open
- Body scroll locked while modal is open
- Focus returns to trigger element when modal closes

### 17.5 Component: Alert Dialog

| Property | Specification |
|---|---|
| ARIA role | `role="alertdialog"` |
| ARIA properties | `aria-modal="true"`, `aria-labelledby`, `aria-describedby` |
| Keyboard | Tab (cycle within actions), Shift+Tab (reverse), Escape (cancel/dismiss) |
| Focus management | Focus moves to first action button or the destructive action (with caution) |
| Screen reader announcement | "[Alert title], alert. [Description]. [Action buttons]" |

**Usage:** Confirmation dialogs, destructive action warnings, critical notifications.

### 17.6 Component: Tab Panel

| Property | Specification |
|---|---|
| ARIA role | Tab list: `role="tablist"`, Tabs: `role="tab"`, Panels: `role="tabpanel"` |
| ARIA properties | `aria-selected` on active tab, `aria-controls` on tab (references panel ID), `aria-labelledby` on panel (references tab ID) |
| Keyboard | Left/Right (or Up/Down) to switch tabs; Home/First Tab; End/Last Tab; Tab moves focus into tab panel |
| Focus management | Roving tabindex: only active tab in tab order; Enter/Space not required for tab activation |
| Screen reader announcement | "[Tab label], tab, selected/not selected. [N] of [Total]" |

**States:** selected, not selected, disabled

**Usage:**
- Tabs should be in a `<div>` or other container with `role="tablist"`
- Each tab is a `<button>` with `role="tab"`
- Each panel is a `<div>` with `role="tabpanel"`
- `aria-controls` on tab points to `id` of panel
- `aria-labelledby` on panel points to `id` of tab

### 17.7 Component: Accordion

| Property | Specification |
|---|---|
| ARIA role | No specific role (heading + button pattern) |
| ARIA properties | Accordion button: `aria-expanded="true/false"`, `aria-controls` (panel ID); Panel: `aria-labelledby` (button ID), `role="region"` |
| Keyboard | Enter, Space to toggle accordion section; Tab to navigate to next accordion button |
| Focus management | Focus remains on accordion button after toggle |
| Screen reader announcement | "[Accordion heading], expanded/collapsed, button. [N] of [Total]" |

**States:** expanded, collapsed

### 17.8 Component: Slider

| Property | Specification |
|---|---|
| ARIA role | `role="slider"` |
| ARIA properties | `aria-valuemin` (minimum value), `aria-valuemax` (maximum value), `aria-valuenow` (current value), `aria-valuetext` (optional, for non-numeric values) |
| Keyboard | Left/Right (decrease/increase by step), Down/Up (decrease/increase), Home (minimum), End (maximum), PageDown/PageUp (larger step) |
| Focus management | Focus on slider thumb |
| Touch target | Thumb: 44x44pt minimum (48x48pt recommended) |
| Screen reader announcement | "[Slider label], slider, [value], [minimum] to [maximum]" |

**States:** default, focused, disabled

### 17.9 Component: Tree View

| Property | Specification |
|---|---|
| ARIA role | `role="tree"`, `role="treeitem"` |
| ARIA properties | `aria-expanded` (on expandable items), `aria-selected` (on selected items), `aria-level` (depth level), `aria-setsize` / `aria-posinset` |
| Keyboard | Up/Down (navigate items), Right (expand), Left (collapse), Home (first), End (last), Enter/Space (activate), * (expand all) |
| Focus management | Roving tabindex: only focused treeitem in tab order |
| Screen reader announcement | "[Item name], tree item, level [N], expanded/collapsed, [N] of [Total]" |

**States:** expanded, collapsed, selected, focused

### 17.10 Component: Menu/Menubar

| Property | Specification |
|---|---|
| ARIA role | `role="menu"`, `role="menubar"`, `role="menuitem"`, `role="menuitemcheckbox"`, `role="menuitemradio"` |
| ARIA properties | `aria-expanded` (submenus), `aria-checked` (checkbox/radio items), `aria-orientation` (horizontal/vertical) |
| Keyboard | Menubar: Left/Right between top-level items, Down opens menu; Menu: Up/Down between items, Enter/Space activates, Right opens submenu, Left closes submenu, Escape closes menu |
| Focus management | Roving tabindex within menu, focus returns to trigger on close |

### 17.11 Component: Tooltip

| Property | Specification |
|---|---|
| ARIA role | No specific role (use `aria-describedby` on trigger) |
| ARIA properties | Trigger: `aria-describedby` referencing tooltip ID |
| Keyboard | Tooltip appears on focus (not just hover); Escape dismisses |
| Focus management | Focus remains on trigger |
| Dismiss | Escape dismisses; content goes away on blur; WCAG 2.1.4.13: dismissible, hoverable, persistent |
| Screen reader announcement | Tooltip content read via `aria-describedby` |

### 17.12 Component: Toast / Notification

| Property | Specification |
|---|---|
| ARIA role | `role="alert"` (for critical/time-sensitive) or `role="status"` (for non-critical) |
| ARIA properties | `aria-live="assertive"` (alert) or `aria-live="polite"` (status); `aria-atomic="true"` |
| Keyboard | Toast appears, optionally focusable if it contains actions (Close, Undo) |
| Auto-dismiss | Announce when toast appears; dismiss after timeout (configurable, minimum 5 seconds) |
| Screen reader announcement | "[Toast message]" (via live region) |

**States:** visible, dismissed

### 17.13 Component: Progress Bar

| Property | Specification |
|---|---|
| ARIA role | `role="progressbar"` |
| ARIA properties | `aria-valuenow` (current value), `aria-valuemin` (minimum, usually 0), `aria-valuemax` (maximum, usually 100), `aria-label` (purpose description) |
| Indeterminate | `aria-valuenow` not set, `aria-busy="true"` on container |
| Screen reader announcement | "[Label], progress, [value] percent" (or "Loading, progress, indeterminate") |

**States:** determinate, indeterminate

### 17.14 Component: Switch / Toggle

| Property | Specification |
|---|---|
| ARIA role | `role="switch"` |
| ARIA properties | `aria-checked="true/false"` |
| Keyboard | Space or Enter to toggle |
| Focus management | Focus on switch control |
| Touch target | 44pt height minimum |
| Screen reader announcement | "[Switch label], switch, on/off" |

**States:** on (checked), off (unchecked), disabled

### 17.15 Component: Listbox

| Property | Specification |
|---|---|
| ARIA role | `role="listbox"`, `role="option"` |
| ARIA properties | `aria-multiselectable` (if applicable), `aria-selected` on options, `aria-activedescendant` or roving tabindex |
| Keyboard | Up/Down (navigate), Home (first), End (last), Space (toggle selection), typeahead support |
| Focus management | Roving tabindex or `aria-activedescendant` |

### 17.16 Component: Combobox / Autocomplete

| Property | Specification |
|---|---|
| ARIA role | `role="combobox"`, `role="listbox"`, `role="option"` |
| ARIA properties | `aria-expanded`, `aria-controls` (listbox ID), `aria-activedescendant`, `aria-autocomplete="list"` |
| Keyboard | Down arrow opens list, Up/Down navigate options, Enter selects, Escape closes, typeahead support |
| Focus management | Roving tabindex in listbox, focus remains on input |
| Screen reader announcement | "[Label], combobox, expanded/collapsed, [N] options available" |

### 17.17 Component: Date Picker

| Property | Specification |
|---|---|
| ARIA role | `role="dialog"`, `role="grid"` for calendar grid, `role="gridcell"` for days |
| ARIA properties | `aria-label` on date picker button, `aria-selected` on selected date, `aria-current="date"` on today |
| Keyboard | Arrow keys navigate days; Enter/Space selects; Tab moves to month/year controls; Escape closes |
| Focus management | Focus on selected date or today when opening; roving tabindex on grid cells |
| Screen reader announcement | "[Day name], [Month] [Date], [Year], selected/not selected, today" |

### 17.18 Component: Data Table

| Property | Specification |
|---|---|
| ARIA role | Native `<table>` with `<th>`, `scope` attributes; or `role="table"`, `role="row"`, `role="columnheader"`, `role="rowheader"`, `role="cell"` |
| ARIA properties | `aria-sort` on sortable headers, `aria-colindex`/`aria-rowindex` for large tables, `aria-rowcount`/`aria-colcount` |
| Keyboard | Arrow keys navigate cells; Tab enters/leaves table; Home/End to first/last cell in row |
| Focus management | Focus on cell level (not individual content) unless cell contains interactive elements |
| Screen reader announcement | "Table with [N] rows and [N] columns. [Column header]: [cell content], row [N]" |

### 17.19 Component: Carousel

| Property | Specification |
|---|---|
| ARIA role | `role="region"` with `aria-roledescription="carousel"` |
| ARIA properties | `aria-label` on carousel region, `aria-roledescription="carousel"`, `aria-live="polite"` (or `off` if user controls), `aria-atomic="false"` |
| Keyboard | Left/Right (previous/next slide), Tab to slide content, Pause key to stop auto-advance |
| Focus management | Focus moves to slide content when navigating between slides; or focus stays on controls |
| Screen reader announcement | "[Slide title], slide [N] of [Total], carousel" |

### 17.20 Component: Knowledge Graph / Network Visualization

| Property | Specification |
|---|---|
| ARIA role | `role="img"` or `role="application"` with `aria-label` describing the graph |
| ARIA properties | `aria-label` for overall graph; each node: `role="button"` or `role="link"` with accessible name |
| Keyboard | Arrow keys navigate between connected nodes; Enter/Space selects a node; Tab moves focus into/out of graph |
| Focus management | Roving tabindex on nodes; visible focus ring on selected node; focus stays within graph area |
| Alternative | Full text-based hierarchical list or data table describing all nodes and connections |
| Screen reader announcement | "Knowledge graph: [N] nodes, [N] connections. Currently focused on [node name]. Connected to: [connected node names]" |

**Additional requirements:**
- Data table alternative: provide a complete table of all nodes, their connections, and relationship types
- Ability to navigate the graph structure via keyboard using directional keys based on graph layout
- Zoom controls accessible via keyboard (+ and - keys)
- Filter/search functionality accessible via keyboard
- Screen reader announcements for zoom level changes and filter results

---

## 18. Related Documents

This Accessibility Bible connects to the following UX Design documents:

| Document | Connection |
|---|---|
| **00_UX_UI_Audit** | Accessibility findings from UX/UI audits feed into the requirements in this Bible. Audit results identify accessibility gaps that must be addressed per the standards defined here. |
| **01_Experience_Design_Bible** | The experience design principles defined in the Experience Design Bible must all be applied with accessibility as a core lens. User journeys must be validated for accessibility at every touchpoint. |
| **02_Design_System** | All design tokens, components, and patterns in the Design System must implement the accessibility specifications detailed in this Bible. Color tokens must meet contrast ratios. Component specifications must include ARIA roles, keyboard interactions, and focus management. |
| **03_Component_Library** | Every component in the library must be built to the accessibility standards defined in Section 17 (Accessible Components Documentation). No component ships without documented accessible behavior. |
| **04_Screen_Specifications** | Screen specifications must reference this Bible for all accessibility requirements. Every screen must be validated against the WCAG 2.1 checklist, tested with screen readers, and verified for keyboard navigation. |

### Cross-Reference Matrix

| Accessibility Requirement | Primary Document | Design System Impact | Component Library Impact | Screen Spec Impact |
|---|---|---|---|---|
| WCAG 2.1 AA Compliance | This Bible Section 2 | Color tokens, spacing tokens | All component ARIA specs | Every screen checklist |
| Color Blind Support | This Bible Section 3 | Accessible palette | Chart patterns, status indicators | Color usage audit |
| Screen Reader Support | This Bible Section 4 | Semantic structure | ARIA roles, live regions | Heading hierarchy |
| Keyboard Navigation | This Bible Section 5 | Focus indicators | Keyboard interactions | Tab order |
| Motion Reduction | This Bible Section 6 | Animation tokens | Animation behavior | Animation timing |
| High Contrast Mode | This Bible Section 7 | High contrast tokens | forced-colors support | Contrast validation |
| Font Scaling | This Bible Section 8 | Typography scale | Responsive behavior | Layout at 200% |
| RTL Support | This Bible Section 9 | Layout tokens | Icon mirroring | Directional layout |
| Audio/Captions | This Bible Section 10, 11 | Media components | Media player | Caption placement |
| Touch Targets | This Bible Section 13 | Spacing tokens | All interactive sizes | Target measurement |
| Cognitive Accessibility | This Bible Section 15 | Mode tokens | Simplified components | Simplified layout |
| Testing Protocol | This Bible Section 16 | QA checklist | Component testing | Per-screen testing |

---

*This document is maintained by the UX Design Team — Accessibility Guild. For questions, updates, or to report an accessibility issue, contact the Accessibility Guild lead.*

**Review frequency:** Quarterly  \
**Next review:** October 2026  \
**Document version:** 1.0
