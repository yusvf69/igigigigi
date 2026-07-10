# Component Library Documentation

> **Project:** Knowledge AI — Learning Platform  
> **Document Version:** 1.0  
> **Last Updated:** 2026-07-10  
> **Audience:** Designers, Front-End Developers, QA Engineers

---

## Table of Contents

1. [Buttons](#1-buttons)
2. [Inputs](#2-inputs)
3. [Cards](#3-cards)
4. [Feedback](#4-feedback)
5. [Navigation](#5-navigation)
6. [Content Display](#6-content-display)
7. [Media](#7-media)
8. [AI-Specific](#8-ai-specific)
9. [Learning-Specific](#9-learning-specific)
10. [Avatar / Character](#10-avatar--character)

---

## 1. Buttons

---

### 1.1 Primary Button

**Purpose**  
The Primary Button communicates the most important call to action on a screen. It should be used for the main action a user is intended to take — such as "Start Lesson," "Submit Answer," "Save Progress," or "Continue."

**Usage Rules**
- Use only one Primary Button per view or section. Having multiple primary actions dilutes visual hierarchy and confuses users.
- Reserved for positive, progressive, or confirmatory actions. Never use for destructive or irreversible actions.
- Always place the Primary Button in a predictable location — typically bottom-right of a container or bottom-center on mobile.
- Text label must be concise, action-oriented, and start with a verb (e.g., "Begin Quiz," "Save Changes").
- Do not disable a Primary Button unless form validation is actively failing. Show disabled state only after user has interacted but input is invalid.

**Visual Rules**
- Solid fill background using the brand accent color (Primary Blue #1A73E8).
- White text (#FFFFFF) for maximum contrast.
- Border-radius: 8px (rounded square).
- Minimum touch target: 48px height.
- Left-aligned icon optional — only when it adds meaning (e.g., a download icon on "Download").
- No border — the solid fill defines the boundary.
- Font: Inter, Medium weight, 16px on desktop, 14px on mobile.

**Spacing**
- Padding: 16px horizontal, 12px vertical (desktop); 14px horizontal, 10px vertical (mobile).
- Margin below: 16px minimum from next element.
- When placed in a button group, margin-right: 12px between buttons.

**States**

| State | Visual Change | Notes |
|-------|--------------|-------|
| Default | Solid blue fill, white text | — |
| Hover | Darker shade (#1557B0), subtle shadow elevation +1 | Cursor changes to pointer |
| Pressed | Even darker (#0D47A1), scale 0.97 | Instant feedback |
| Focused | Blue outline ring 3px (#1A73E8) with 2px offset | Visible on keyboard nav only |
| Disabled | Opacity 40%, no shadow, cursor not-allowed | Text remains readable |
| Loading | Spinner replaces icon or appears next to text | Button width preserved to avoid layout shift |
| Error | Button shakes briefly (200ms horizontal oscillation) | Only for destructive confirmations |

**Responsive Behavior**
- Desktop: Default size. Full-width only in forms.
- Tablet: Same as desktop; touch target remains 48px minimum.
- Mobile: Full-width in most contexts. Reduce horizontal padding to 14px. Stack buttons vertically in groups.
- In bottom sheets or modals: Always full-width on mobile.

**Accessibility**
- ARIA role: `button` (implicit when using `<button>`).
- Provide `aria-label` if icon-only or if text is ambiguous.
- Disabled state: `aria-disabled="true"`.
- Loading state: `aria-busy="true"`, add `aria-live="polite"` region announcing "Loading."
- Focus indicator must be highly visible — minimum 3px offset ring.
- Keyboard: Enter or Space to activate.
- Screen reader should read label + "button" + state (e.g., "Begin Quiz, button").

**Animation**
- Entrance: Fade in + slide up 8px over 200ms ease-out.
- Hover: Background color transitions over 150ms ease.
- Press: Scale to 0.97 over 80ms ease-in, release scale back over 150ms ease-out.
- Exit: Fade out over 150ms ease-in.

**Interaction Rules**
- Debounce double-clicks with a 500ms cooldown after first activation.
- Show loading state within 100ms of click if action is asynchronous.
- On success, briefly flash green (200ms) before navigating or clearing.

**Do**
- "Save Progress" on a lesson page.
- "Submit Quiz" at the bottom of an assessment.
- "Continue" in a multi-step onboarding flow.

**Do Not**
- Use a Primary Button for "Delete Account."
- Place two Primary Buttons side by side.
- Use disabled Primary Button as a placeholder for incomplete forms — use validation messaging instead.

---

### 1.2 Secondary Button

**Purpose**  
The Secondary Button offers an alternative action that is less prominent than the primary. It complements the primary action without competing for attention.

**Usage Rules**
- Use when there are two actions and one is clearly secondary (e.g., "Cancel" next to "Save").
- Can be used alone for non-critical actions such as "Learn More," "View Details," or "See All."
- Never disable a Secondary Button — if an action is unavailable, hide the button or use a tertiary/ghost variant.
- Can appear multiple times on a single page without confusion.

**Visual Rules**
- Outlined style: 1.5px solid border using Primary Blue (#1A73E8).
- Text color: Primary Blue (#1A73E8).
- Background: Transparent (white on light backgrounds).
- Border-radius: 8px.
- Same height as Primary Button (48px minimum).
- No shadow.

**Spacing**
- Padding: 16px horizontal, 12px vertical.
- Margin: 12px from adjacent buttons.
- Identical spacing profile to Primary Button for alignment in groups.

**States**

| State | Visual Change |
|-------|--------------|
| Default | Blue border, blue text, transparent bg |
| Hover | Light blue background fill (#E8F0FE) |
| Pressed | Darker fill (#D2E3FC), scale 0.97 |
| Focused | 3px blue focus ring with offset |
| Disabled | Opacity 40%, border opacity 40% |
| Loading | Spinner replaces icon, text hidden |

**Responsive Behavior**
- Desktop: Inline with other elements.
- Tablet: Unchanged.
- Mobile: Can go full-width when stacked vertically. Reduce horizontal padding to 14px.

**Accessibility**
- Same ARIA conventions as Primary Button.
- Ensure color contrast between text and hover background meets WCAG AA (4.5:1).

**Animation**
- Hover: Background fill transitions 150ms ease.
- Press: Scale 0.97 over 80ms.

**Interaction Rules**
- Same debounce rules as Primary Button.
- If used as "Cancel," confirm with user only if there is unsaved data.

**Do**
- "Cancel" next to a Primary "Save" button.
- "View Details" on a card.
- "See All" in a section header.

**Do Not**
- Use for the main call to action on a page.
- Place a Secondary Button without a Primary counterpart in a confirmation dialog.
- Disable a Secondary Button.

---

### 1.3 Tertiary Button

**Purpose**  
The Tertiary Button offers a low-emphasis action that is visually subtle. It is used for supplementary or less frequent actions such as "Reset," "Clear All," "Skip," or "View Less."

**Usage Rules**
- Use when an action is optional or supplementary.
- Can appear inline with text.
- Suitable for destructive actions when paired with a confirmation dialog (e.g., "Clear All History").
- Never use as the primary or secondary action in a dialog with only two options.

**Visual Rules**
- No border or background in default state.
- Text color: Primary Blue (#1A73E8) or Neutral Gray (#5F6368) for less emphasis.
- Font: Inter, Medium weight, 14px.
- Border-radius: 4px (minimal).
- Underline: No underline by default; underline on hover for text-like buttons.
- Height: Auto (inline) or 36px minimum as a standalone button.

**Spacing**
- Padding: 12px horizontal, 8px vertical (standalone); 0px when inline.
- Margin: 8px from surrounding elements.

**States**

| State | Visual Change |
|-------|--------------|
| Default | Text only, no background |
| Hover | Light gray background (#F1F3F4), no border |
| Pressed | Darker gray background (#E8EAED) |
| Focused | 2px focus ring |
| Disabled | Opacity 38%, no hover effect |
| Loading | Mini spinner next to text |

**Responsive Behavior**
- Desktop/Tablet/Mobile: Consistent appearance. Touch target minimum 36px.

**Accessibility**
- Must have `aria-label` if text alone is unclear.
- Focus ring must contrast with surrounding content.

**Animation**
- Hover: Background fill transitions 150ms ease.

**Interaction Rules**
- Inline tertiary buttons should not disrupt text flow.
- "Skip" actions should be placed less prominently than primary "Continue."

**Do**
- "Skip for now" on an onboarding screen.
- "Clear filters" in a search results page.
- "View less" to collapse an expanded section.

**Do Not**
- Use as the only action on an empty state.
- Place a Tertiary Button where a user might mistake it for static text.

---

### 1.4 Ghost Button

**Purpose**  
The Ghost Button is a borderless, backgroundless button that sits directly on surfaces. It is used for toolbar actions, inline edits, and situations where visual minimalism is critical.

**Usage Rules**
- Ideal for toolbars, top app bars, and inline editing actions.
- Use when the action is utilitarian (e.g., "Edit," "Delete," "Settings").
- Not suitable for high-emphasis actions.
- Can be icon-only or text-only.

**Visual Rules**
- No border, no background in default state.
- Icon color: Neutral Gray (#5F6368) or on-dark white.
- Text color: matching icon color.
- Border-radius: 8px (hover/pressed background appears within this radius).
- Minimum 36px x 36px touch target.
- Icon size: 20px or 24px.

**Spacing**
- Padding: 8px all around (compact) or 12px (standard).
- Margin: 4px between adjacent ghost buttons.

**States**

| State | Visual Change |
|-------|--------------|
| Default | Icon/text only |
| Hover | Light gray circular background (#F1F3F4) |
| Pressed | Darker gray (#E8EAED) |
| Focused | 2px focus ring |
| Disabled | Opacity 38% |
| Selected/Toggled | Primary Blue background (#E8F0FE) for active filters/tools |

**Responsive Behavior**
- Desktop/Tablet/Mobile: Consistent. On mobile, increase touch target to 44px.

**Accessibility**
- ARIA: `aria-label` mandatory for icon-only ghost buttons.
- `aria-pressed` for toggle states.
- Keyboard: Enter/Space to activate.

**Animation**
- Background fill: 150ms ease on hover.
- Icon color transitions: 150ms ease.

**Interaction Rules**
- Toggle groups (e.g., bold/italic in editor) should use `aria-pressed` and show selected state.
- No ripple effect — keep interaction minimal.

**Do**
- "Edit" icon in a card header.
- "Settings" gear icon in the top app bar.
- "Delete" icon in a list item.

**Do Not**
- Use ghost buttons for primary page actions.
- Place ghost buttons in a row without adequate spacing — they blend together.
- Use for text-only links that navigate outside the app.

---

### 1.5 Icon Button

**Purpose**  
The Icon Button is a circular or square button that uses an icon instead of text to represent an action. It is space-efficient and ideal for toolbars, cards, and dense UIs.

**Usage Rules**
- Use when the icon is universally understood (e.g., heart for favorite, star for bookmark, trash for delete).
- Always provide a text tooltip on hover or focus.
- Use in navigation bars, card actions, and inline editing contexts.
- Never use for complex or ambiguous actions without a supporting label.

**Visual Rules**
- Container: 40px x 40px or 48px x 48px circle (border-radius: 50%).
- Background: Transparent default.
- Icon size: 20px or 24px.
- Icon color: Neutral Gray (#5F6368) or white on dark surfaces.
- On dark surfaces, use a semi-transparent white background (10-20% opacity) default.

**Spacing**
- Padding: Inside the 40px/48px container.
- Margin: 8px minimum between adjacent icon buttons.

**States**

| State | Visual Change |
|-------|--------------|
| Default | Icon only |
| Hover | Circular background fill (#F1F3F4) |
| Pressed | Scale 0.93, darker fill (#E8EAED) |
| Focused | 2px focus ring, circular |
| Disabled | Opacity 38% |
| Selected | Primary Blue fill (#E8F0FE), icon turns blue |

**Responsive Behavior**
- Desktop: 40px standard.
- Tablet: 40px standard.
- Mobile: 48px minimum touch target.

**Accessibility**
- `aria-label` describing the action (e.g., "Add to favorites").
- Tooltip on hover using `title` attribute or custom tooltip.
- Keyboard: Enter/Space to activate.
- Focus ring must be visible and circular.

**Animation**
- Hover: Background expands from center 150ms ease.
- Press: Scale to 0.93 over 80ms.

**Interaction Rules**
- Toggle icon buttons (like/favorite) should animate between filled and outlined states.
- Show immediate feedback — no delay for toggle actions.

**Do**
- Heart icon for "Like" on a knowledge card.
- Star icon for "Bookmark" on a lesson.
- Trash icon for "Delete" in edit mode.

**Do Not**
- Use an Icon Button without a tooltip.
- Use an Icon Button for the primary action on a page.
- Stack icon buttons without adequate spacing (minimum 8px).

---

### 1.6 Floating Action Button (FAB)

**Purpose**  
The Floating Action Button (FAB) is a prominent, elevated circular button that floats above the content to promote a key action. It is used for the most common or important action on a screen.

**Usage Rules**
- Use only one FAB per screen.
- Reserved for the single most frequent or important action (e.g., "Create New Note," "Add to Collection," "Start Quiz").
- Do not use for navigation — FAB should trigger an action, not move to a different screen.
- Speed dial pattern: On press, the FAB can expand into multiple related actions (labeled).
- Hide FAB on scroll down, reveal on scroll up (to maximize content area).

**Visual Rules**
- Shape: Circle, 56px diameter (standard), 40px (mini).
- Background: Primary Blue (#1A73E8) or white on dark themes.
- Icon: White if blue background, blue if white background.
- Elevation: Shadow level 6 (box-shadow: y=3, blur=6, spread=0, opacity=30%).
- Icon size: 24px.
- No text label on the button itself (use speed-dial labels or tooltip).

**Spacing**
- Position: Bottom-right, 16px from edges (mobile), 24px (desktop).
- Margin from bottom: 72px when Bottom Navigation Bar is present.
- Margin from right: 16px.

**States**

| State | Visual Change |
|-------|--------------|
| Default | Elevated circle with icon |
| Hover | Shadow elevation +2, slight scale 1.03 |
| Pressed | Scale 0.95, shadow reduced |
| Focused | Focus ring around circle |
| Disabled | Opacity 40%, no elevation |
| Loading | Spinner replaces icon |

**Responsive Behavior**
- Desktop: 56px, positioned at bottom-right.
- Tablet: 56px, same position.
- Mobile: 56px, may shift to accommodate bottom nav.
- On scroll: Animate out (scale + fade) on scroll down, animate in on scroll up.

**Accessibility**
- `aria-label` mandatory (e.g., "Create new flashcard").
- `aria-haspopup` if FAB expands a menu.
- Keyboard: Tab to focus, Enter/Space to activate.
- Ensure contrast between FAB and background content.

**Animation**
- Entrance: Scale from 0 to 1 + fade in over 250ms ease-out-back.
- Exit: Scale to 0 + fade out over 200ms ease-in.
- Scroll hide: Slide down + fade out 200ms.
- Speed dial: Children fan out with staggered delays (50ms each).

**Interaction Rules**
- Speed dial: Click FAB shows child actions; click again or tap outside dismisses.
- Avoid navigation on FAB click — use it for in-page actions.
- Tooltip on hover for accessibility.

**Do**
- "Add Note" on a lesson detail page.
- "Create Flashcard" in a study set.
- "Start Session" in a review dashboard.

**Do Not**
- Use FAB for "Delete," "Cancel," or destructive actions.
- Place FAB in the center or left side.
- Use FAB on every screen — only where a single prominent action exists.
- Overlap FAB with content — ensure adequate margin.

---

### 1.7 Close Button

**Purpose**  
The Close Button dismisses modals, dialogs, side panels, toasts, overlays, and full-screen views.

**Usage Rules**
- Always position in the top-right corner of the container being dismissed.
- Use an "X" icon (multiplication sign) — never "Cancel" text for the close action.
- Required on all modals, bottom sheets, popovers, and full-screen overlays.
- Optional on toasts (auto-dismiss is preferred).
- On mobile, the close button should be in the top app bar or overlay header.

**Visual Rules**
- Icon: "X" symbol, size 20px or 24px.
- Container: 40px x 40px (square or circle), transparent default.
- Color: Neutral Gray (#5F6368) or white on dark overlays.
- Border-radius: 50% (circular hover area).
- On dark overlays: white icon with semi-transparent white background (15% opacity) on hover.

**Spacing**
- Padding: 8px inside the 40px container.
- Position: top 16px, right 16px (or 24px for desktop).

**States**

| State | Visual Change |
|-------|--------------|
| Default | Icon visible, no background |
| Hover | Circular background (#F1F3F4 or white 15% on dark) |
| Pressed | Scale 0.93, darker background |
| Focused | Focus ring |

**Responsive Behavior**
- Consistent across all breakpoints.
- Touch target: 44px minimum on mobile.

**Accessibility**
- `aria-label="Close"` mandatory.
- Auto-focus on close button when modal opens (for keyboard users).
- Escape key must also dismiss the container.

**Animation**
- Hover: Background expands 150ms ease.
- Click: Subtle scale 0.93.

**Interaction Rules**
- Clicking the close button must dismiss the container.
- Escape key should trigger the same action.
- If there is unsaved state, show a confirmation before closing.

**Do**
- Dismiss a settings modal.
- Close a full-screen image viewer.
- Dismiss a bottom sheet.

**Do Not**
- Use a Close Button without `aria-label`.
- Place the Close Button on the left (exception: some RTL layouts).
- Hide the Close Button behind other elements.

---

### 1.8 Back Button

**Purpose**  
The Back Button returns the user to the previous screen or step in a navigation hierarchy.

**Usage Rules**
- Use in top app bars on detail/secondary screens.
- Never use on the home or root screen.
- Text label optional — "Back" or icon only (left-pointing arrow).
- On mobile, the Back Button is standard in the top app bar.
- On desktop, breadcrumbs may replace the back button.

**Visual Rules**
- Icon: Left-pointing arrow (chevron or triangle).
- Text: "Back" in 16px Medium weight (optional).
- Container: 40px x 40px with transparent background.
- Color: Primary Blue (#1A73E8) or Neutral Gray (#5F6368).
- Border-radius: 50% for icon area.

**Spacing**
- Left-aligned in the top app bar.
- Padding: 8px inside 40px container.
- Margin-left: 4px from the left edge of the app bar.

**States**

| State | Visual Change |
|-------|--------------|
| Default | Arrow icon |
| Hover | Circular background (#F1F3F4) |
| Pressed | Scale 0.95 |
| Focused | Focus ring |

**Responsive Behavior**
- Desktop: May include text "Back"; 40px touch target.
- Tablet: Same as desktop.
- Mobile: Icon-only; 44px touch target.

**Accessibility**
- `aria-label="Go back to previous page"`.
- `aria-label` plus text if icon-only.

**Animation**
- Press: Scale 0.95 over 80ms.
- Page transition: Content slides right while back button remains.

**Interaction Rules**
- On web, use `window.history.back()` or framework router back.
- If there is no history, hide or disable the button.
- Confirm with user if there is unsaved data.

**Do**
- Navigate from lesson detail to lesson list.
- Go back from settings to home.
- Dismiss a search overlay.

**Do Not**
- Show Back Button on the root screen.
- Use Back Button for closing modals — use Close Button instead.
- Navigate to unexpected screens — back must return to the previous screen.

---

### 1.9 Skip Button

**Purpose**  
The Skip Button allows users to bypass optional content or steps, typically in onboarding flows, tutorials, or multi-step processes.

**Usage Rules**
- Use in onboarding, walkthroughs, or multi-step forms where steps are optional.
- Place less prominently than the primary "Continue" or "Next" button.
- Always pair with a primary action button.
- The "Skip" action should not lead to data loss — skipped content should be accessible later.
- Last step of an onboarding flow should replace "Skip" with "Get Started" or "Done."

**Visual Rules**
- Text style: Tertiary or Ghost button appearance.
- Color: Neutral Gray (#5F6368) or lighter.
- Font: Inter, Regular weight, 14px.
- Underline: None.
- Border-radius: 8px if styled as a button.

**Spacing**
- Left-aligned or right-aligned below the primary action.
- Padding: 12px horizontal, 8px vertical.
- Margin-top: 8px from the primary button.

**States**

| State | Visual Change |
|-------|--------------|
| Default | Gray text |
| Hover | Light gray background (#F1F3F4) |
| Pressed | Scale 0.97 |
| Focused | Focus ring |
| Disabled | Opacity 38% (rarely used — consider hiding instead) |

**Responsive Behavior**
- Consistent across breakpoints.
- On mobile, keep the Skip Button close to Continue for thumb reach.

**Accessibility**
- `aria-label="Skip this step"`.
- Keyboard: Tab order after primary action.

**Animation**
- Fade in on screen entrance.
- On click: slide left + fade out over 200ms.

**Interaction Rules**
- On skip, move to next step or dismiss the flow.
- Track skip events for analytics to understand drop-off.
- Never make critical account setup steps skippable.

**Do**
- "Skip intro" on a tutorial video.
- "Skip for now" in profile setup.
- "Skip" in a multi-step lesson flow.

**Do Not**
- Make required steps skippable.
- Hide the Skip Button — it should be visible but low emphasis.
- Use Skip for destructive actions.

---

### 1.10 CTA Button

**Purpose**  
The Call-to-Action (CTA) Button is a high-impact promotional button used in marketing contexts, landing pages, and upgrade prompts to drive user conversion.

**Usage Rules**
- Use for conversion goals: "Sign Up," "Upgrade Now," "Start Free Trial," "Subscribe."
- Can be larger than standard buttons for emphasis.
- Use only one CTA per section or promotional block.
- Typically paired with supporting text or a hero section.
- May include a decorative arrow or icon to indicate progression.

**Visual Rules**
- Larger sizing: 56px height (desktop), 48px (mobile).
- Background: Gradient or solid accent color — Primary Blue (#1A73E8) or a warm accent (#FF6D00).
- Text: White, Bold weight, 18px (desktop), 16px (mobile).
- Border-radius: 8px or 12px for emphasis.
- Shadow: Elevation level 4.
- Icon: Right-arrow optional, 20px.

**Spacing**
- Padding: 24px horizontal, 16px vertical (desktop).
- Margin: 24px from surrounding content.
- Centered in most marketing layouts.

**States**

| State | Visual Change |
|-------|--------------|
| Default | Full color, shadow, icon |
| Hover | Lighter or brighter shade, elevation +2, scale 1.02 |
| Pressed | Scale 0.97, reduced shadow |
| Focused | Focus ring, 3px offset |
| Loading | Spinner replaces icon |
| Disabled | Opacity 40%, no hover effects |

**Responsive Behavior**
- Desktop: 56px height, may be centered or full-width in a constrained container.
- Tablet: 52px height.
- Mobile: Full-width, 48px height.

**Accessibility**
- `aria-label` matching the CTA text.
- High contrast mode: Ensure text/background contrast exceeds 4.5:1.
- Keyboard: Enter/Space.

**Animation**
- Entrance: Scale up from 0.95 + fade in 300ms ease-out.
- Hover: Scale 1.02 + shadow increase 200ms.
- Pulse animation (subtle) for attention on key pages.

**Interaction Rules**
- Click must navigate to signup/purchase flow or trigger conversion event.
- Track clicks via analytics with dedicated event naming.
- Avoid double-click issues with loading state.

**Do**
- "Start Free Trial" on a landing page.
- "Upgrade to Pro" in a feature comparison.
- "Subscribe Now" on a newsletter signup.

**Do Not**
- Use CTAs for in-app utility actions.
- Place multiple CTAs near each other.
- Use gray or low-contrast colors for CTAs.

---

### 1.11 Learning Action Button

**Purpose**  
The Learning Action Button is a specialized button for learning-specific interactions such as "Mark as Complete," "Review Later," "I Know This," or "Study Again."

**Usage Rules**
- Use within lesson cards, flashcard sessions, and study review screens.
- Action is tied to learning progress — clicking updates spaced repetition or progress state.
- Each learning action should have a distinct icon and color for quick recognition.
- Use in conjunction with learning analytics to track user mastery.

**Visual Rules**
- Height: 44px (standard).
- Rounded: 8px border-radius.
- Background: Light color with icon — color varies by action type:
  - "I Know This": Green (#34A853) with checkmark.
  - "Review Later": Yellow (#FBBC04) with clock.
  - "Study Again": Red (#EA4335) with refresh.
- Text: Dark or white depending on background lightness.
- Icon: 18px, left-aligned.

**Spacing**
- Padding: 14px horizontal, 10px vertical.
- Margin: 8px between multiple learning action buttons.
- In flashcard view: bottom of card, horizontally distributed.

**States**

| State | Visual Change |
|-------|--------------|
| Default | Colored background with icon and text |
| Hover | Darker shade of the action color |
| Pressed | Scale 0.97 |
| Focused | Focus ring |
| Disabled | Opacity 50%, used after action is taken |
| Loading | Spinner replaces icon |
| Selected | Active state shown with filled/heart icon for "Save" |

**Responsive Behavior**
- Desktop: Inline group of buttons, equal width.
- Tablet: Same.
- Mobile: Stack vertically or horizontal scroll if more than 3.

**Accessibility**
- `aria-label` describing the learning action.
- `aria-pressed` for toggle actions.
- Color is not the only indicator — include icon and text.

**Animation**
- Entrance: Staggered slide-up 200ms each.
- Click: Brief scale bounce to indicate action recorded.
- State change: Smooth icon transition (e.g., outlined to filled).

**Interaction Rules**
- After click, disable briefly (2s) to prevent double-submission.
- Update SRS algorithm and show brief confirmation.
- Undo option within 5 seconds for accidental actions.

**Do**
- "I Know This" on a flashcard.
- "Review Later" on a lesson card.
- "Mark as Complete" at end of a lesson.

**Do Not**
- Use for generic actions like "Save" or "Cancel."
- Allow rapid clicking of learning actions — enforce cooldown.
- Use same color for different learning actions.

---

### 1.12 Quiz Option Button

**Purpose**  
The Quiz Option Button represents a selectable answer choice in a quiz, assessment, or multiple-choice question.

**Usage Rules**
- Use one button per answer option in multiple-choice questions.
- Group with a Radio Button or Checkbox pattern depending on single vs multi-select.
- After selection, provide immediate visual feedback for correct/incorrect.
- Disable all options after an answer is submitted.
- In review mode, show the correct answer and the user's selected answer with distinct styling.

**Visual Rules**
- Shape: Pill or rounded rectangle (border-radius: 12px).
- Border: 2px solid Neutral Gray (#DADCE0) default.
- Background: White default.
- Text: 16px, Regular weight, dark text.
- Selection indicator: Radio circle on the left or right.
- Width: Full-width of the container.
- Minimum height: 56px (touch target).
- Correct: Green border + background tint (#E6F4EA) with checkmark.
- Incorrect: Red border + background tint (#FCE8E6) with X mark.
- Disabled after submission: Opacity unchanged but no hover.

**Spacing**
- Padding: 16px horizontal, 14px vertical.
- Margin-bottom: 12px between options.
- Group margin-top: 16px from question text.

**States**

| State | Visual Change |
|-------|--------------|
| Default | Gray border, white bg, radio empty |
| Hover | Light gray bg (#F8F9FA), border stays gray |
| Selected | Blue border (#1A73E8), blue radio filled |
| Selected + Correct | Green border, green bg tint, checkmark |
| Selected + Incorrect | Red border, red bg tint, X mark |
| Unselected + Revealed Correct | Green border, light green bg (review) |
| Disabled (after submit) | No hover, cursor default |
| Focused | Focus ring on the option |

**Responsive Behavior**
- Desktop: Full width, max-width 720px for the options container.
- Tablet: Same.
- Mobile: Full-width of screen with 16px side padding.

**Accessibility**
- Role: `radio` (single select) or `checkbox` (multi-select).
- `aria-checked` for selected state.
- `aria-label` adding question context if needed.
- Live region: Announce "Correct" or "Incorrect" after submission.
- Keyboard: Arrow keys to navigate options, Space to select.

**Animation**
- Hover: Border color transitions 150ms.
- Select: Radio fill animates 200ms.
- Correct/Incorrect reveal: Background color transitions 300ms, icon scales in 200ms.
- Wrong answer shake: Horizontal oscillation 300ms.

**Interaction Rules**
- Select on click or keyboard enter/space.
- Once submitted, lock all options — no further selection.
- In review mode, allow inspection but no changes.
- Show explanation after selection with a smooth reveal.

**Do**
- A/B/C/D option buttons in a quiz.
- True/False option buttons.
- Multi-select "Select all that apply" options.

**Do Not**
- Allow deselection in single-select mode without selecting another option.
- Change option order after the quiz has started.
- Show correct/incorrect feedback before the user submits.

---

### 1.13 Flashcard Action Button

**Purpose**  
The Flashcard Action Button controls flashcard review actions such as flipping the card, rating recall difficulty, and navigating between cards.

**Usage Rules**
- Use in flashcard study sessions only.
- Grouped at the bottom of the flashcard interface.
- Includes: Flip Card, Rating buttons (Again/Hard/Good/Easy), and navigation (Previous/Next).
- Rating buttons map to spaced repetition intervals.

**Visual Rules**
- Flip Button: Circular, 48px, centered, with flip icon.
- Rating Buttons: 4 buttons arranged horizontally:
  - "Again" (Red, #EA4335).
  - "Hard" (Orange, #FBBC04).
  - "Good" (Green, #34A853).
  - "Easy" (Blue, #1A73E8).
- Rating button size: 56px x 40px, rounded 8px.
- Text: White, Bold, 14px.
- Navigation buttons: Ghost style, left/right chevrons.

**Spacing**
- Flip button: Margin-bottom 24px.
- Rating buttons: 8px gap between each.
- Navigation: Outer edges of the bottom bar.

**States**

| State | Visual Change |
|-------|--------------|
| Default (Rating) | Colored background, white text |
| Hover | Darker shade of rating color |
| Pressed | Scale 0.95 |
| Disabled | Opacity 50% (e.g., on last card "Next" disabled) |

**Responsive Behavior**
- Desktop: Rating buttons visible always.
- Tablet: Same.
- Mobile: Rating buttons may be smaller (48px x 36px), horizontal scroll if needed.

**Accessibility**
- Each rating button: `aria-label="Rate Again"`, `aria-label="Rate Hard"`, etc.
- Flip button: `aria-label="Flip card to see answer"`.
- Keyboard: 1-4 keys for ratings, Space for flip, arrow keys for navigation.

**Animation**
- Flip: 3D card flip animation (600ms) synchronized with button press.
- Rating: Button scales down briefly, card exits with slide animation.
- Navigation: Slide left/right for next/previous card.

**Interaction Rules**
- After rating, auto-advance to next card.
- Show next card after a 300ms delay.
- After last card, show session summary.
- Keyboard shortcuts for power users.

**Do**
- "Again" for cards the user completely forgot.
- "Easy" for cards recalled instantly.
- Flip button to reveal the back of the card.

**Do Not**
- Allow flipping during the rating animation.
- Show rating buttons before the card is flipped.
- Navigate away without rating the current card.

---

### 1.14 Share Button

**Purpose**  
The Share Button triggers the platform's native sharing mechanism or opens a share sheet to allow users to share content externally.

**Usage Rules**
- Use on content that users may want to share: lessons, knowledge cards, achievements, quotes.
- Position near bookmark and download actions in a content toolbar.
- On mobile, use the native Share API (Web Share API).
- On desktop, fall back to a custom share dialog with link copying.

**Visual Rules**
- Icon: Share icon (connected nodes or upward arrow).
- Style: Ghost or Icon Button style.
- Size: 40px x 40px (icon button).
- Color: Neutral Gray (#5F6368).

**Spacing**
- Standard icon button spacing: 8px padding.

**States**

| State | Visual Change |
|-------|--------------|
| Default | Share icon |
| Hover | Circular background fill |
| Pressed | Scale 0.95 |
| Focused | Focus ring |
| Shared | Brief checkmark animation to confirm |

**Responsive Behavior**
- Desktop: Opens custom share dialog or native OS share.
- Tablet: Opens native share sheet.
- Mobile: Opens native share sheet via Web Share API.

**Accessibility**
- `aria-label="Share this [content type]"`.
- Share dialog must be keyboard navigable.
- Focus trap within share dialog if custom.

**Animation**
- On click: Icon morphs to checkmark (200ms), then reverts.
- Share sheet: Slides up from bottom (mobile) 300ms.

**Interaction Rules**
- Must handle cancellation gracefully.
- Copy link should show a "Copied!" toast.
- Track share events for analytics.
- Respect user's sharing permissions.

**Do**
- Share a completed lesson to social media.
- Share a knowledge card with a friend.
- Share an achievement badge.

**Do Not**
- Share without user confirmation.
- Pre-populate share text with tracking parameters that alter the shared link destination.

---

### 1.15 Bookmark Button

**Purpose**  
The Bookmark Button allows users to save content for later reference. It toggles between bookmarked and unbookmarked states.

**Usage Rules**
- Use on cards, lessons, knowledge items, and articles.
- Toggle action: first tap bookmarks, second tap removes bookmark.
- Must provide visual confirmation of state change.
- Sync bookmark state across devices (requires backend).
- Show bookmark count optionally.

**Visual Rules**
- Icon: Bookmark outline (default) and filled bookmark (active).
- Style: Icon Button.
- Size: 40px x 40px.
- Color: Neutral Gray (#5F6368) default, Primary Blue (#1A73E8) active.
- Transition: Outlined to filled morphing animation.

**Spacing**
- Standard icon button spacing.

**States**

| State | Visual Change |
|-------|--------------|
| Default (unbookmarked) | Outline bookmark icon, gray |
| Hover (unbookmarked) | Light background fill |
| Bookmarked | Filled bookmark icon, blue |
| Hover (bookmarked) | Slightly darker blue background |
| Pressed | Scale 0.93 |
| Focused | Focus ring |
| Loading | Spinner during sync |

**Responsive Behavior**
- Consistent across all breakpoints.
- Touch target: 44px on mobile.

**Accessibility**
- `aria-label="Bookmark this lesson"` or `aria-label="Remove bookmark"`.
- `aria-pressed` to indicate toggle state.
- `aria-live="polite"` region to announce "Lesson bookmarked" or "Bookmark removed."

**Animation**
- Bookmark toggle: Icon morphs from outline to filled 250ms ease.
- Staggered scale bounce: 0.93 -> 1.0 -> 1.05 -> 1.0 over 400ms.
- On remove: Reverse animation.

**Interaction Rules**
- Toggle must be instant in UI; sync to backend asynchronously.
- Show optimistic UI — assume success, revert on failure.
- On failure, show "Failed to save bookmark" toast.

**Do**
- Bookmark a lesson for later.
- Bookmark a knowledge card.
- Bookmark a flashcard set.

**Do Not**
- Use the Bookmark Button for non-bookmark actions.
- Require page reload to reflect bookmark state.
- Hide the bookmark button on content that can't be bookmarked — disable it instead.

---

### 1.16 Download Button

**Purpose**  
The Download Button allows users to download content such as PDFs, images, videos, audio files, and documents for offline access.

**Usage Rules**
- Use on content that has a downloadable asset.
- Show download progress during the download.
- After download, change state to "Open" or "Downloaded."
- On mobile, respect storage permissions.
- Large files should show estimated download time.

**Visual Rules**
- Icon: Download arrow (pointing down into a tray).
- Style: Icon Button or Button with text label.
- Size: 40px x 40px (icon button) or standard button height 48px.
- Color: Neutral Gray (#5F6368) default, Primary Blue (#1A73E8) active/downloaded.
- Border-radius: 8px for text + icon variant.

**Spacing**
- Icon button: standard 8px padding.
- Text + icon: 16px horizontal, 12px vertical padding.

**States**

| State | Visual Change |
|-------|--------------|
| Default | Download icon, gray |
| Hover | Light background fill |
| Downloading | Progress circle replaces icon, or progress bar below |
| Downloaded | Icon changes to checkmark or open folder, blue color |
| Pressed | Scale 0.95 |
| Focused | Focus ring |
| Disabled | Opacity 40%, when download is unavailable |
| Error | Red tint, exclamation icon, tap to retry |

**Responsive Behavior**
- Desktop: Show text label + icon.
- Mobile: Icon-only, 44px touch target.

**Accessibility**
- `aria-label="Download [file name]"`.
- Download progress: `aria-valuenow`, `aria-valuemin`, `aria-valuemax` on progress role.
- Download complete: `aria-live` announcement.

**Animation**
- Download start: Icon arrows animate downward 300ms.
- Progress: Circular progress animates 400ms complete loop.
- Complete: Icon morphs to checkmark 300ms.
- Error: Brief shake 200ms.

**Interaction Rules**
- Enable background downloading with notification on mobile.
- If download fails, show retry option.
- Do not allow multiple simultaneous downloads of the same file.
- Cache management: alert user when storage is low.

**Do**
- Download a lesson PDF for offline reading.
- Download an audio lecture.
- Download a knowledge graph as an image.

**Do Not**
- Start download without user consent.
- Block the UI during download — show progress in background.
- Download files over cellular without warning for large files (>50MB).

---

## 2. Inputs

---

### 2.1 Text Input

**Purpose**  
The Text Input allows users to enter a single line of alphanumeric text. It is the most fundamental data entry component.

**Usage Rules**
- Use for single-line text entry: names, titles, email, passwords, short answers.
- For multi-line text, use Text Area instead.
- Always pair with a visible label.
- Provide placeholder text that shows an example of expected input.
- Use input masking for formatted fields (phone, date, credit card).

**Visual Rules**
- Container: Rounded rectangle, border-radius 8px.
- Border: 1.5px solid Neutral Gray (#DADCE0) default.
- Background: White.
- Text: 16px, Regular, dark (#202124).
- Label: 14px, Medium, above or floating.
- Height: 48px minimum.
- Width: Flexible, min 200px.
- Padding inside: 12px horizontal, 14px vertical.
- Helper text: 12px below input, gray.

**Spacing**
- Margin-bottom: 20px (includes label, input, helper text stack).
- Label to input gap: 4px.
- Input to helper text gap: 4px.
- Between form fields: 24px.

**States**

| State | Visual Change |
|-------|--------------|
| Default | Gray border, white bg, placeholder text |
| Hover | Border darkens (#9AA0A6) |
| Focused | Blue border (#1A73E8), blue ring 2px offset |
| Active/Typing | Blue border, cursor blinking |
| Filled | Blue border stays if persistent focus, else default |
| Error | Red border (#EA4335), red helper text, error icon |
| Success | Green border (#34A853), green checkmark |
| Disabled | Gray background (#F1F3F4), 38% opacity text, no interaction |
| Read-only | Light gray background, regular text weight |
| Loading | Skeleton shimmer in place of input |

**Responsive Behavior**
- Desktop: Fixed width or form width.
- Tablet: Full-width in forms.
- Mobile: Full-width, increase touch target to 48px.

**Accessibility**
- `<label>` element linked via `for` attribute or `aria-labelledby`.
- `aria-describedby` for helper text and error messages.
- `aria-invalid="true"` for error state.
- `aria-required="true"` for required fields.
- Error must be announced by screen reader on validation.
- Focus must be programmatically moved to the first errored field on form submit.

**Animation**
- Focus: Border color transition 150ms ease.
- Error: Input border turns red, shake animation 300ms.
- Placeholder: Fades out on focus 150ms.

**Interaction Rules**
- Character count for fields with max length (optional).
- Auto-capitalization and auto-correct appropriate for field type.
- Show clear button (X) when input has text.
- Trim whitespace on blur.
- On submit, scroll to and focus the first field with error.

**Do**
- Single-line text entry for name, email, title.
- Password field with show/hide toggle.
- Search field with icon.

**Do Not**
- Use Text Input for multi-line content.
- Hide the label — always keep it visible or use floating label.
- Placeholder as label — placeholder disappears on input.

---

### 2.2 Search Input

**Purpose**  
The Search Input allows users to search through content, data, or the entire platform. It may include autocomplete, recent searches, and filters.

**Usage Rules**
- Use in top app bars, search pages, or filterable lists.
- Debounce search queries by 300ms after the user stops typing.
- Show recent searches on focus (if input is empty).
- Show autocomplete suggestions as the user types.
- Minimum 2 characters before triggering search.
- Clear button to reset the input.

**Visual Rules**
- Container: Rounded, border-radius 24px (pill) or 8px.
- Background: Light gray (#F1F3F4) default or white.
- Search icon (magnifying glass) left-aligned inside.
- Text: 16px, Regular.
- Clear button (X) right-aligned when input has text.
- Border: None (filled background) or 1.5px solid.
- Height: 48px.
- Width: Variable — expands on focus in compact mode.

**Spacing**
- Icon left: 12px from left edge.
- Text padding-left: 40px (to accommodate icon).
- Text padding-right: 40px (to accommodate clear button).
- Clear button right: 8px from right edge.

**States**

| State | Visual Change |
|-------|--------------|
| Default | Gray bg, magnifying glass icon, placeholder text |
| Hover | Slightly darker bg (#E8EAED) |
| Focused | Blue border (#1A73E8), bg becomes white, suggestions dropdown |
| Active/Typing | Text visible, clear button appears, suggestions filter |
| Filled | Text with clear button |
| Disabled | Gray bg, 38% opacity |
| Loading | Spinner replaces magnifying glass |

**Responsive Behavior**
- Desktop: Fixed width 360px in app bar; full width on search page.
- Tablet: 280px in app bar, expands on focus.
- Mobile: Full-width with 16px side padding, expands to full screen on focus (search overlay mode).

**Accessibility**
- Role: `search` (landmark) or `combobox` with `aria-expanded`.
- `aria-label="Search"` for the input.
- Suggestions list: `role="listbox"` with `aria-selected`.
- Escape key closes suggestions.
- `aria-live="polite"` for search results count.

**Animation**
- Focus expansion: Width animates 200ms ease (compact to expanded).
- Clear button: Fades in 150ms.
- Suggestions: Dropdown slides down 200ms.

**Interaction Rules**
- Debounce input: 300ms delay before search executes.
- Minimum 2 characters to search.
- Recent searches: Show on focus if input is empty, max 5 items.
- Autocomplete: Highlight matching text in suggestions.
- On clear: Reset results to initial state or show popular content.
- On submit (Enter): Execute full search.

**Do**
- Search for lessons, topics, or flashcards.
- Filter a long list of knowledge items.
- Find specific content in the library.

**Do Not**
- Trigger search on every keystroke without debounce.
- Show autocomplete without recent searches context.
- Navigate away on search without user confirmation.

---

### 2.3 Text Area

**Purpose**  
The Text Area allows users to enter multi-line text for notes, descriptions, comments, and long-form input.

**Usage Rules**
- Use when expected input exceeds one line.
- For notes, lesson reflections, journal entries, or essay answers.
- Resizable (by user) or auto-growing (preferred).
- Character count display for fields with max length.
- Always pair with a visible label.

**Visual Rules**
- Container: Rounded rectangle, border-radius 8px.
- Border: 1.5px solid Neutral Gray (#DADCE0) default.
- Background: White.
- Text: 16px, Regular, dark (#202124).
- Minimum height: 120px (3-4 lines).
- Width: 100% of container.
- Padding: 12px all around.
- Resize handle: Bottom-right corner (optional — auto-grow is preferred).
- Scrollbar: Appears when content exceeds height.

**Spacing**
- Margin-bottom: 20px (with label and helper).
- Label to text area gap: 4px.
- Text area to character count gap: 4px.

**States**

| State | Visual Change |
|-------|--------------|
| Default | Gray border, white bg, placeholder |
| Hover | Border darkens (#9AA0A6) |
| Focused | Blue border, blue ring |
| Filled | Content present |
| Error | Red border, red helper |
| Disabled | Gray bg, 38% opacity |
| Read-only | Light gray bg |

**Responsive Behavior**
- Desktop: Fixed height with scroll or auto-grow.
- Tablet: Same.
- Mobile: Auto-grow preferred to avoid scroll within input.

**Accessibility**
- Same as Text Input for label, error, helper text.
- `aria-multiline="true"` (implied for `<textarea>`).
- Character count: `aria-live="polite"` when approaching limit.

**Animation**
- Focus: Border color 150ms ease.
- Auto-grow: Height transitions 100ms ease.

**Interaction Rules**
- Auto-grow height as user types, up to a max (e.g., 400px).
- Tab to indent (optional: use Shift+Tab for outdent if code input).
- Paste handling: strip formatting by default.

**Do**
- Write lesson notes.
- Submit essay answers in quizzes.
- Add descriptions to flashcards.

**Do Not**
- Use Text Area for single-line input.
- Set a fixed height that clips content.
- Allow unlimited length without warning.

---

### 2.4 Dropdown

**Purpose**  
The Dropdown (Select) allows users to choose one option from a list. The selected option is displayed in the closed state.

**Usage Rules**
- Use when there are 5-15 options. For fewer, use Radio Buttons. For more, use a Select with search.
- Single selection only.
- The trigger shows the current selection or placeholder text.
- Options list appears on click/space/enter.
- Selecting an option closes the dropdown.

**Visual Rules**
- Trigger: Rectangular container, border-radius 8px.
- Border: 1.5px solid #DADCE0.
- Height: 48px.
- Padding: 12px.
- Text: 16px, Regular.
- Chevron icon: Downward arrow on the right.
- Dropdown panel: White, elevation shadow level 4, border-radius 8px.
- Option height: 44px.
- Option padding: 12px 16px.
- Selected option: Blue background (#E8F0FE) or checkmark.

**Spacing**
- Margin-bottom: 20px.
- Options list: No margin between options.

**States**

| State | Visual Change |
|-------|--------------|
| Default (closed) | Gray border, chevron down, placeholder/selection text |
| Hover (trigger) | Border darkens |
| Focused (trigger) | Blue border, blue ring |
| Open | Chevron rotates up, panel visible |
| Option default | White bg |
| Option hover | Light gray (#F1F3F4) |
| Option selected | Blue bg (#E8F0FE) or checkmark |
| Disabled (trigger) | Gray bg, 38% opacity |
| Error | Red border |

**Responsive Behavior**
- Desktop: Dropdown panel opens below or above.
- Tablet: Same.
- Mobile: Opens as a bottom sheet (action sheet) for easier thumb access, or native select.

**Accessibility**
- Role: `combobox` with `aria-expanded`, `aria-haspopup="listbox"`.
- Options: `role="option"`, `aria-selected`.
- `aria-activedescendant` on the trigger for active option.
- Keyboard: Enter/Space to open, Arrow keys to navigate, Enter to select, Escape to close.
- Focus must move to the first selected option when opened.

**Animation**
- Panel: Slide down + fade in 200ms ease.
- Chevron: Rotate 180 degrees 200ms.
- Option hover: Background transition 100ms.

**Interaction Rules**
- Click outside or Escape closes without selecting.
- Selecting an option closes panel and updates trigger text.
- Long lists (>15 items) should include a search filter.

**Do**
- Select a subject category.
- Choose difficulty level.
- Pick a language.

**Do Not**
- Use Dropdown for 2-3 options — use Radio Buttons.
- Use Dropdown for 50+ options without search.
- Close dropdown on option hover — only on click/enter.

---

### 2.5 Select Menu

**Purpose**  
The Select Menu is an enhanced dropdown that may include grouped options, descriptions, icons, and search functionality for larger option sets.

**Usage Rules**
- Use for options that need categorization or additional metadata.
- Supports single and (optionally) multi-select patterns.
- Include search when more than 15 options.
- Can display icons or avatars next to options.
- Groups have group labels (non-selectable).

**Visual Rules**
- Same base styling as Dropdown trigger.
- Options include: icon/avatar, label, optional description (14px gray), optional secondary text.
- Group label: 12px, Bold, uppercase, gray, non-interactive.
- Option height: 48px (with description) or 40px (label only).
- Multi-select: Checkbox on the left of each option.
- Selected items displayed as chips in trigger (multi-select).

**Spacing**
- Trigger: Same as Dropdown.
- Option padding: 12px 16px.
- Group label padding: 8px 16px, margin-top: 8px.

**States**
- Same as Dropdown for trigger and options.
- Multi-select checkbox: checked/unchecked states.
- Search input within the panel.

**Responsive Behavior**
- Desktop: Panel appears as dropdown.
- Tablet: Same.
- Mobile: Full-screen modal or bottom sheet.

**Accessibility**
- Same ARIA as Dropdown.
- Search input inside: `aria-label="Search options"`.
- Group labels: `role="group"` with `aria-label`.

**Animation**
- Same as Dropdown.
- Chip appear: Scale in 200ms (multi-select).

**Interaction Rules**
- Search filters options in real-time.
- Clear search resets to full list.
- Multi-select: chips appear in trigger, can be removed individually.
- Select all / Deselect all option for long lists.

**Do**
- Select topics with category grouping.
- Choose a language with native flag icons.
- Select multiple tags with chip display.

**Do Not**
- Use Select Menu for 3 or fewer flat options.
- Overcrowd options with too much metadata.

---

### 2.6 Multi-Select

**Purpose**  
The Multi-Select allows users to choose multiple items from a list, typically displayed as checkboxes within a dropdown or inline list.

**Usage Rules**
- Use when users need to select zero, one, or many options from a set.
- Visualize selections as checked checkboxes.
- Show selected count in the trigger label (e.g., "3 selected").
- Provide a "Clear all" action.
- Use chips to display selected values when not too many.

**Visual Rules**
- Trigger: Same as dropdown with "N selected" or chips.
- Checkbox: 20px x 20px, border-radius 4px, blue when checked.
- Option: 44px height, checkbox left, label right.
- Chips in trigger: 8px gap, removable with X icon.

**Spacing**
- Trigger: Same padding as dropdown.
- Chips: margin-right 4px between chips.
- Options: 44px height, padding 12px 16px.

**States**
- Checkbox states: unchecked, checked, indeterminate.
- Chip states: default, hover (show X), removed.

**Responsive Behavior**
- Desktop: Dropdown panel.
- Mobile: Bottom sheet with checkboxes.

**Accessibility**
- Role: `listbox` with `aria-multiselectable="true"`.
- Each option: `role="option"`, `aria-selected`.
- Checkbox: `role="checkbox"`, `aria-checked`.

**Animation**
- Checkbox check: Scale animation 200ms.
- Chips: Slide in 200ms.

**Interaction Rules**
- Click toggles selection without closing the dropdown.
- "Select All" at the top of the list.
- Chip remove button removes selection.
- Keyboard: Space to toggle, Arrow keys to navigate.

**Do**
- Select multiple topics of interest.
- Choose features to compare.
- Filter by multiple categories.

**Do Not**
- Use Multi-Select for mutually exclusive options.
- Close dropdown after each selection.

---

### 2.7 Checkbox

**Purpose**  
The Checkbox allows users to select or deselect an option. It can be used individually for binary choices or in groups for multi-select.

**Usage Rules**
- Use for binary choices: "I agree to terms," "Email me updates."
- Use in groups for selecting multiple items from a set.
- Indeterminate state for parent checkboxes with partial child selection.
- Label clickable — clicking the label toggles the checkbox.

**Visual Rules**
- Box: 20px x 20px, border-radius 4px.
- Border: 2px solid #5F6368 default.
- Background: White default, Primary Blue (#1A73E8) checked.
- Checkmark: White, 14px.
- Label: 16px, Regular, to the right, 8px gap.
- Indeterminate: Blue background with a horizontal dash instead of checkmark.

**Spacing**
- Checkbox to label: 8px.
- Between checkboxes in a group: 12px vertical.
- Group margin-bottom: 16px.

**States**

| State | Visual Change |
|-------|--------------|
| Default (unchecked) | Gray border, white bg |
| Hover (unchecked) | Light blue bg (#E8F0FE) |
| Checked | Blue bg, white checkmark |
| Hover (checked) | Darker blue bg |
| Indeterminate | Blue bg, white dash |
| Disabled (unchecked) | Gray bg, 38% opacity border |
| Disabled (checked) | Gray bg, gray checkmark, 38% opacity |
| Focused | Focus ring on the box |
| Error | Red border |

**Responsive Behavior**
- Desktop: Inline or stacked.
- Tablet: Same.
- Mobile: Increase touch target — ensure 44px clickable area (box + label combined).

**Accessibility**
- Role: `checkbox`.
- `aria-checked`: `true`, `false`, or `mixed`.
- Label: `<label>` element wrapping or `aria-labelledby`.
- Keyboard: Space to toggle.

**Animation**
- Checkmark: Draw animation 200ms.
- Background: Color transition 150ms.

**Interaction Rules**
- Clicking the label has same effect as clicking the box.
- In indeterminate state, clicking selects all children (checkbox becomes checked).
- Group checkboxes: parent/child relationship with automatic updates.

**Do**
- "I agree to the Terms of Service."
- "Notify me about new lessons."
- Parent checkbox with children options.

**Do Not**
- Use Checkbox for mutually exclusive options — use Radio Buttons.
- Use Checkbox for a single action that triggers immediately — use a Toggle/Switch.
- Make the hit area only the box — include the label.

---

### 2.8 Radio Button

**Purpose**  
The Radio Button allows users to select exactly one option from a set of mutually exclusive choices.

**Usage Rules**
- Use when only one option can be selected.
- Minimum 2 options, recommended max 5-6. For more, use Dropdown.
- One option should be pre-selected (default) whenever possible.
- Never present a single Radio Button — use a Checkbox instead.
- Label clickable.

**Visual Rules**
- Circle: 20px diameter.
- Border: 2px solid #5F6368 default.
- Fill: Primary Blue (#1A73E8) inner circle when selected, 10px diameter.
- Label: 16px, Regular, 8px gap to the right.
- Group: Stacked vertically, or horizontal for 2-3 options.

**Spacing**
- Radio to label: 8px.
- Between radio options: 12px vertical.
- Group margin-bottom: 16px.

**States**

| State | Visual Change |
|-------|--------------|
| Default (unselected) | Gray border circle, empty center |
| Hover (unselected) | Light blue bg (#E8F0FE) around circle |
| Selected | Blue filled inner circle |
| Hover (selected) | Slightly darker blue inner circle |
| Disabled (unselected) | Gray bg, 38% opacity |
| Disabled (selected) | Gray fill, 38% opacity |
| Focused | Focus ring |
| Error | Red border |

**Responsive Behavior**
- Desktop: Vertical stack.
- Tablet: Same.
- Mobile: Vertical stack, ensure 44px touch target.

**Accessibility**
- Role: `radio`.
- Group role: `radiogroup`.
- `aria-checked`: `true` or `false`.
- Keyboard: Arrow keys to navigate between options, Space to select.
- `aria-label` on radiogroup if no visible label.

**Animation**
- Selection: Inner circle scales in 200ms.
- Border color transition: 150ms.

**Interaction Rules**
- Selecting one option automatically deselects others.
- Cannot deselect once selected (except by selecting another).
- Pre-select the safest or most common option.

**Do**
- "Gender: Male / Female / Other."
- "Difficulty: Beginner / Intermediate / Advanced."
- "Payment method: Credit Card / PayPal / Crypto."

**Do Not**
- Allow deselecting the currently selected radio button.
- Use Radio Buttons for non-mutually-exclusive options.
- Place Radio Buttons too far apart — make the relationship clear.

---

### 2.9 Toggle / Switch

**Purpose**  
The Toggle (Switch) represents a binary on/off state that immediately takes effect when changed. It is used for settings and preferences.

**Usage Rules**
- Use for immediate-action binary settings: notifications, dark mode, auto-play.
- The label should describe what the toggle controls.
- Show current state clearly with color and position.
- Action takes effect immediately — no "Save" button needed.
- Do not use for confirmation-dependent actions.

**Visual Rules**
- Track: 44px width x 24px height, rounded pill (border-radius: 12px).
- Background: Gray (#DADCE0) when off, Primary Blue (#1A73E8) when on.
- Thumb: 20px diameter circle, white, centered vertically.
- Thumb shadow: Small drop shadow (elevation 1).
- Label: 16px, Regular, to the left or right.
- Animation: Thumb slides along the track.

**Spacing**
- Track width: 44px, height: 24px.
- Thumb diameter: 20px.
- Label gap: 8px.
- Between toggles in a list: 16px vertical.

**States**

| State | Visual Change |
|-------|--------------|
| Off | Gray track, thumb left |
| On | Blue track, thumb right |
| Hover | Slightly darkened track color |
| Pressed | Thumb scales to 24px diameter |
| Disabled | 38% opacity, no interaction |
| Focused | Focus ring around the track |
| Loading | Thumb shows spinner |

**Responsive Behavior**
- Desktop: Label left or right.
- Tablet: Same.
- Mobile: Label left, toggle right (aligned to the end).

**Accessibility**
- Role: `switch`.
- `aria-checked`: `true` or `false`.
- Keyboard: Space or Enter to toggle.
- `aria-label` if label is not associated.
- Announce state change: "Notifications on" or "Notifications off."

**Animation**
- Thumb slide: 200ms ease.
- Track color transition: 200ms ease.
- Thumb press scale: 150ms.

**Interaction Rules**
- Toggle must feel immediate — no confirmation dialog.
- If action requires a change in system settings, show a brief toast confirming the change.
- Do not toggle on hover — only on click.

**Do**
- "Enable dark mode."
- "Turn on notifications."
- "Auto-play next lesson."

**Do Not**
- Use Toggle for non-immediate actions (e.g., "Save settings").
- Use Toggle as a substitute for Checkbox in forms.
- Place a Toggle without a clear label describing the controlled setting.

---

### 2.10 Slider

**Purpose**  
The Slider allows users to select a value (or range) from a continuous or discrete set by dragging a thumb along a track.

**Usage Rules**
- Use for volume, brightness, difficulty level, progress scrubbing, or rating.
- Show current value as a tooltip or label above the thumb.
- Discrete sliders snap to predefined steps.
- Continuous sliders allow any value in the range.
- Range slider: Two thumbs for selecting a min and max.

**Visual Rules**
- Track: Horizontal line, 4px height, border-radius 2px.
- Track color: Gray (#DADCE0) for unfilled, Primary Blue (#1A73E8) for filled.
- Thumb: 20px diameter circle, white, shadow elevation 2.
- Thumb border: 2px solid #1A73E8 (focused state).
- Value label: 14px, above the thumb, tooltip style.
- Tick marks for discrete steps (optional).
- Min/max labels at ends of the track.

**Spacing**
- Track height: 4px.
- Thumb size: 20px (touch target: 44px).
- Track left/right padding to accommodate thumb at extremes.
- Label above thumb: 8px gap.
- Min/max labels: Outside the track ends.

**States**

| State | Visual Change |
|-------|--------------|
| Default | Gray track, blue filled portion, gray thumb |
| Hover (thumb) | Thumb scales to 22px, shadow increases |
| Dragging | Thumb scales to 24px, blue border highlight |
| Focused | Focus ring on thumb |
| Disabled | 38% opacity, no interaction |
| Value tooltip | Appears above thumb on hover/drag |

**Responsive Behavior**
- Desktop: Full-width of container.
- Tablet: Same.
- Mobile: Ensure 44px touch target for thumb. Track may need to be thicker (6px) for easier targeting.

**Accessibility**
- Role: `slider`.
- `aria-valuemin`, `aria-valuemax`, `aria-valuenow`.
- `aria-orientation="horizontal"`.
- Keyboard: Left/Right arrows to adjust (small step), Up/Down (larger step), Home/End for min/max.
- `aria-label` describing the slider's purpose.

**Animation**
- Thumb drag: Smooth follow 0ms (direct manipulation).
- Thumb hover scale: 150ms ease.
- Value tooltip: Fade in/out 150ms.

**Interaction Rules**
- Click on the track moves thumb to that position.
- Drag thumb to adjust value.
- For range sliders, thumbs cannot cross each other.
- Provide text input for precise value entry (optional).

**Do**
- Set audio volume.
- Adjust quiz difficulty (1-5).
- Select a price range for filtering.

**Do Not**
- Use Slider for precise numeric entry — use a Number Input.
- Use Slider with too many discrete steps (snapping becomes unusable).
- Make the track too thin for touch interaction.

---

### 2.11 Rating Input

**Purpose**  
The Rating Input allows users to provide a rating or assessment, typically using stars or numeric scales.

**Usage Rules**
- Use for rating content: lessons, courses, flashcards, or knowledge cards.
- Stars: 1-5 scale (most common).
- Hearts or custom icons can replace stars for thematic relevance.
- Display average rating for read-only display.
- Allow half-star ratings for precision (optional).
- User can click to set rating or click again to change.

**Visual Rules**
- Icons: 5 stars (24px or 32px each) in a horizontal row.
- Filled star: Gold (#F9AB00) or brand accent.
- Half star: Half-filled gold, half gray.
- Empty star: Neutral Gray (#DADCE0).
- Spacing: 4px between stars.
- Hover preview: Stars fill up to the hovered position.
- Read-only: Slightly smaller (20px), no interaction.

**Spacing**
- Star size: 24px (interactive), 20px (read-only).
- Gap between stars: 4px.
- Margin below: 8px for optional text "3.5 out of 5."

**States**

| State | Visual Change |
|-------|--------------|
| Default (empty) | All stars gray |
| Hover preview | Stars fill up to hovered position in gold |
| Rated | Stars up to selected value filled |
| Hover on rated | Can show higher preview, reverts on mouse leave |
| Pressed | Star scales briefly 0.8 then bounces to 1.0 |
| Focused | Focus ring on the star group |
| Disabled | 38% opacity, no interaction |
| Read-only | Smaller, no hover effects |

**Responsive Behavior**
- Desktop: 32px stars for interaction.
- Tablet: 28px stars.
- Mobile: 32px stars with 8px gap for touch accuracy.

**Accessibility**
- Role: `radiogroup`.
- Each star: `role="radio"`, `aria-checked`.
- Keyboard: Arrow keys to navigate, Space to select.
- `aria-label="Rating: 3 out of 5 stars"`.
- Announce selected rating on change.

**Animation**
- Star fill: Color transition 150ms.
- Star press: Scale bounce 250ms.
- Consecutive stars fill with staggered delay (50ms each).

**Interaction Rules**
- Clicking the same star again deselects (if already rated) or allows re-rating.
- Hover preview does not persist after mouse leave — reverts to set rating.
- On touch devices, tap to rate directly (no hover).
- After rating, show a brief thank-you message or animation.

**Do**
- Rate a lesson after completion.
- Rate a flashcard quality.
- Rate a knowledge article.

**Do Not**
- Use more than 10 scale points.
- Allow fractional ratings in interactive mode without explicit half-star UI.
- Require login to view ratings.

---

### 2.12 Date Picker

**Purpose**  
The Date Picker allows users to select a date (or date range) from a calendar interface.

**Usage Rules**
- Use for date of birth, scheduling, deadlines, and date-range filtering.
- Provide manual date input as an alternative.
- Show month/year navigation.
- Support single date, date range, or multiple date selection.
- Disable past dates for future-only selections.

**Visual Rules**
- Calendar grid: 7 columns (days of the week), up to 6 rows.
- Day cell: 40px x 40px, rounded 50% for selected.
- Header: Month and year with left/right navigation arrows.
- Day names: 3-letter abbreviations (Sun, Mon...), 12px, gray.
- Selected date: Blue filled circle (#1A73E8), white text.
- Today: Blue outlined circle.
- Hover: Light blue bg (#E8F0FE).
- Disabled dates: Gray, 38% opacity.
- Range: Light blue bg between selected start/end, darker blue at ends.
- Dropdown panel: Elevated, border-radius 8px, padding 16px.

**Spacing**
- Day cell: 40px x 40px.
- Calendar grid gap: 0px (cells flush).
- Panel padding: 16px.
- Navigation arrows: 32px x 32px.

**States**

| State | Visual Change |
|-------|--------------|
| Default day | White bg, dark text |
| Hover day | Light blue bg |
| Selected day | Blue filled circle, white text |
| Today | Blue outlined circle |
| Disabled day | Gray, 38% opacity |
| In-range (range picker) | Light blue bg |
| Range start/end | Blue filled circle |
| Month nav arrow hover | Light gray bg |

**Responsive Behavior**
- Desktop: Dropdown panel positioned below input.
- Tablet: Dropdown or inline calendar.
- Mobile: Full-screen modal or bottom sheet calendar.

**Accessibility**
- Role: `dialog` or `grid` for the calendar.
- Navigation: `aria-label="Next month"`.
- Each day: `role="gridcell"`, `aria-label="July 10, 2026"`.
- Keyboard: Arrow keys to navigate days, Enter to select, Escape to close.
- `aria-live="polite"` for month/year changes.
- Input: `type="date"` as fallback.

**Animation**
- Month transition: Slide left/right 250ms ease.
- Day select: Scale 0.8 -> 1.0 bounce 250ms.
- Panel: Fade + scale 200ms.

**Interaction Rules**
- Clicking a day selects it and closes the picker (single mode).
- Clicking year opens year grid for quick navigation.
- Range mode: Click start, click end (with visual feedback).
- Manual input should parse various date formats.

**Do**
- Select a birth date.
- Choose a lesson deadline.
- Filter content by date range.

**Do Not**
- Use Date Picker for dates far in the past or future without year navigation.
- Block manual date entry — always provide text input alternative.
- Use ambiguous date formats — show "MM/DD/YYYY" or locale-appropriate.

---

### 2.13 File Upload

**Purpose**  
The File Upload component allows users to select and upload files from their device.

**Usage Rules**
- Use for uploading images, documents, audio, or video.
- Support drag-and-drop in addition to click-to-browse.
- Show file type and size restrictions before upload.
- Preview uploaded files when possible (images, documents).
- Support multiple file upload with list display.
- Show upload progress for each file.

**Visual Rules**
- Drop zone: Dashed border, 2px, gray (#DADCE0), border-radius 8px.
- Drop zone height: 160px minimum.
- Upload icon: Cloud upload, 48px, gray.
- Text: "Drag and drop files here, or click to browse."
- File list: Below drop zone, each file as a list item.
- File item: Icon + filename + file size + progress bar + remove button + status.
- Preview: Thumbnail for images, generic icon for other types.
- Progress: Linear bar, 4px height, blue.
- Success: Green checkmark.
- Error: Red X with error message.

**Spacing**
- Drop zone padding: 24px.
- File list gap: 8px between items.
- File item padding: 12px 16px.
- Progress bar height: 4px.

**States**

| State | Visual Change |
|-------|--------------|
| Default | Dashed border, gray icon, instruction text |
| Drag over (hover) | Blue dashed border (#1A73E8), blue bg tint (#E8F0FE) |
| File selected / uploading | File appears in list with progress |
| Upload complete | Green checkmark, file shown as uploaded |
| Upload error | Red X, error message, retry button |
| Disabled | 38% opacity, no drop/click |
| Empty | No files yet, drop zone visible |

**Responsive Behavior**
- Desktop: Drop zone with drag-and-drop.
- Tablet: Drop zone (drag from file manager may be limited).
- Mobile: Click to browse only (camera/gallery/file picker).

**Accessibility**
- Role: `button` for the click-to-browse area.
- `aria-label="Upload files"`.
- Drag-and-drop: `aria-dropeffect`.
- Progress: `role="progressbar"`, `aria-valuenow`, etc.
- Keyboard: Enter/Space on drop zone opens file picker.
- Announce upload status via `aria-live`.

**Animation**
- Drop zone hover: Border color transition 200ms, bg color 200ms.
- File added: Slide in from top 200ms.
- Progress: Bar animates smoothly 300ms updates.
- Complete: Icon transitions to checkmark 200ms.

**Interaction Rules**
- Validate file type and size on selection.
- Show inline error for invalid files without uploading.
- Allow removal of files from the upload queue.
- Auto-upload after selection or manual upload button.
- Large files: show estimated time.
- Chunked upload for files >100MB.

**Do**
- Upload a profile picture.
- Upload a document for a lesson submission.
- Upload multiple images for a knowledge card.

**Do Not**
- Accept files without validating type/size.
- Block the UI during upload — show progress in the list.
- Allow duplicate filenames without warning.

---

### 2.14 Voice Input Button

**Purpose**  
The Voice Input Button allows users to input text using speech recognition instead of typing.

**Usage Rules**
- Use in search inputs, text areas, or note-taking contexts.
- Always require explicit user activation (push-to-talk).
- Show listening state with visual feedback (waveform animation).
- Transcribe speech to text in real-time.
- Support multiple languages.
- Fall back gracefully when speech recognition is unavailable.

**Visual Rules**
- Icon: Microphone, 20px or 24px.
- Container: Icon Button style, 40px x 40px.
- Color: Gray default, Primary Blue (#1A73E8) when listening.
- Listening indicator: Pulsing ring around the icon, waveform animation.
- Tooltip: "Search by voice" or "Tap to speak."
- Error state: Microphone with slash, red.

**Spacing**
- Standard icon button spacing.

**States**

| State | Visual Change |
|-------|--------------|
| Default | Gray microphone icon |
| Hover | Light background fill, tooltip |
| Listening | Blue icon, pulsing ring animation, waveform |
| Processing | Spinner replaces icon |
| Success (transcribed) | Brief green checkmark flash |
| Error | Red icon with slash, shake animation |
| Unavailable | Icon hidden or disabled with explanation tooltip |
| Focused | Focus ring |

**Responsive Behavior**
- Desktop: Positioned inside search input or next to text area.
- Tablet: Same.
- Mobile: Inside search bar or keyboard microphone key alternative.

**Accessibility**
- `aria-label="Voice input"`.
- `aria-expanded` when listening overlay is active.
- `aria-live="polite"` for transcription text.
- Keyboard: Enter/Space to activate.
- Provide text alternative for users who cannot use voice.

**Animation**
- Listening state: Pulsing ring 1.5s cycle infinite.
- Waveform: Bars animate with audio amplitude.
- Transcribed: Text appears character by character.
- Error: Shake 300ms.

**Interaction Rules**
- Click to start listening; click again or say "stop" to end.
- Show interim transcription results in real-time.
- When done, insert transcribed text at cursor position.
- Handle ambient noise gracefully — show "Didn't catch that" on silence.
- Respect microphone permissions — request gracefully.

**Do**
- Voice search for lessons and topics.
- Dictate notes instead of typing.
- Voice input for quick flashcard creation.

**Do Not**
- Auto-activate microphone on page load.
- Send audio data without user consent and clear privacy notice.
- Assume perfect transcription — allow user to edit results.

---

### 2.15 OTP / Code Input

**Purpose**  
The OTP (One-Time Password) Input allows users to enter a verification code sent via SMS or email, typically 4-6 digits.

**Usage Rules**
- Use for two-factor authentication, email verification, password reset.
- Input fields are individual digit boxes (one per character).
- Auto-advance to the next field after typing.
- Auto-submit when all digits are entered.
- Support paste functionality for the full code.
- Accept numeric input only (alphanumeric for certain cases).

**Visual Rules**
- Number of boxes: 4, 5, or 6 (matching the code length).
- Each box: 48px x 48px (desktop), 44px x 44px (mobile).
- Border: 2px solid #DADCE0, border-radius 8px.
- Text: 24px, Bold, centered.
- Background: White.
- Active box: Blue border (#1A73E8).
- Filled box: Blue border, dark text.
- Gap between boxes: 8px.
- Focus: Blue ring on the active box.

**Spacing**
- Box size: 48px.
- Gap: 8px.
- Container: Centered, max-width based on box count.
- Below: Timer text "Resend code in 2:30" and "Resend" link.

**States**

| State | Visual Change |
|-------|--------------|
| Empty | Gray border, empty box |
| Active (focused) | Blue border, cursor blinking |
| Filled | Blue border, digit visible, cursor moves to next |
| Complete (all filled) | All boxes filled, auto-submitting |
| Error | Red border on all boxes, red error message below |
| Success | Green border on all boxes, checkmark animation |
| Disabled | Gray bg, 38% opacity |

**Responsive Behavior**
- Desktop: 48px boxes.
- Tablet: 48px boxes.
- Mobile: 44px boxes, may need horizontal scroll if 6+ digits.

**Accessibility**
- Each input: `aria-label="Digit 1 of 6"`, etc.
- Role: `textbox` with `inputmode="numeric"`.
- `aria-invalid` for error state.
- `aria-describedby` for error messages.
- Keyboard: Auto-advance on digit entry, Backspace to go back.
- Announce "Code entered" on auto-submit.

**Animation**
- Focus: Border color transition 150ms.
- Auto-advance: Digit slides in from top 100ms.
- Error: Boxes shake 300ms.
- Success: Box borders turn green sequentially 200ms each.

**Interaction Rules**
- Typing a digit fills current box and auto-focuses next box.
- Backspace removes digit and focuses previous box.
- Paste full code: distribute digits across boxes and auto-submit.
- Auto-submit delay: 300ms after last digit entered.
- Resend cooldown: Start timer on first send, allow resend after 30-60 seconds.
- Limit attempts: Show "Too many attempts" after 3-5 failures.

**Do**
- Verify email address during signup.
- Two-factor authentication code entry.
- Password reset code.

**Do Not**
- Accept partial submissions.
- Clear the entire input on a single wrong digit — show error and let user correct.
- Use letters when the code is numeric (and vice versa) without clear indication.

---

### 2.16 Chip Input

**Purpose**  
The Chip Input allows users to enter multiple discrete values, displayed as chips/tags within the input field.

**Usage Rules**
- Use for email recipients, tags, keywords, or multi-value fields.
- Each value becomes a chip upon pressing Enter or comma.
- Chips are removable via X button.
- Backspace on empty input removes the last chip.
- Support paste of multiple values (comma-separated).
- Show suggestions dropdown for autocomplete.

**Visual Rules**
- Input area: Rectangular container with chips inside, border-radius 8px.
- Border: 1.5px solid #DADCE0.
- Min-height: 48px (expands as chips are added).
- Chip: Rounded pill, 8px border-radius, #E8F0FE background, blue text.
- Chip height: 28px.
- Chip X: 16px icon, gray.
- Text input: Inline after last chip, 16px.
- Placeholder text: "Add tags..." or "Enter email addresses."

**Spacing**
- Container padding: 8px.
- Chip margin: 4px.
- Chip padding: 8px horizontal, 4px vertical.
- Between chips and input: 4px.

**States**

| State | Visual Change |
|-------|--------------|
| Default | Gray border, empty or chips present |
| Focused | Blue border |
| Chip default | Blue bg, blue text, X icon |
| Chip hover | Darker blue bg (#D2E3FC) |
| Error | Red border, red error message |
| Disabled | Gray bg, 38% opacity |
| Duplicate chip | Shake animation, red highlight temporarily |

**Responsive Behavior**
- Desktop: Fixed width or full-width.
- Tablet: Same.
- Mobile: Full-width, ensure chip X is easily tappable (minimum 24px).

**Accessibility**
- Role: `textbox` or `combobox` (with suggestions).
- `aria-label="Enter tags"`.
- Chips: `role="button"` with `aria-label="Remove tag: [tag name]"`.
- `aria-live="polite"` for added/removed chips.
- Keyboard: Enter to add chip, Backspace to remove last chip, Delete/Backspace on focused chip removes it.

**Animation**
- Chip add: Scale in from 0 -> 1 over 200ms.
- Chip remove: Scale out to 0 over 150ms.
- Error shake on duplicate: 300ms.

**Interaction Rules**
- Press Enter or comma to create a chip from typed text.
- Backspace on empty input removes the last chip.
- Click X on a chip to remove it.
- Duplicate values: Show error, do not add.
- Max chips: Show warning when approaching limit.
- Suggestions: Filter as user types, select with click or arrow + Enter.

**Do**
- Add tags to a knowledge card.
- Enter email recipients for sharing.
- Add topics to a lesson filter.

**Do Not**
- Allow empty chips.
- Allow chips with only whitespace.
- Autocomplete without showing clear suggestions.

---

### 2.17 Color Picker

**Purpose**  
The Color Picker allows users to select a color from a palette, spectrum, or via hex/rgb input.

**Usage Rules**
- Use for theme customization, highlight colors, or color-coded tags.
- Support presets (common colors), spectrum picker, and manual hex input.
- Show a preview of the selected color.
- Eye-dropper tool for advanced use (where supported).

**Visual Rules**
- Trigger: 40px x 40px button showing current color swatch.
- Swatch: Filled with selected color, border-radius 8px, white border.
- Picker panel: Elevated, border-radius 12px, padding 16px.
- Preset colors: Grid of 20px x 20px circles, 4px gap.
- Spectrum: Saturation/brightness square with hue slider.
- Hex input: Text field with "#" prefix, 7 characters (including #).
- Preview: Large swatch showing selected color with hex value.

**Spacing**
- Swatch trigger: 40px x 40px.
- Panel width: 240px.
- Preset circle: 20px diameter.
- Spectrum area: 200px x 160px.
- Control gap: 8px.

**States**

| State | Visual Change |
|-------|--------------|
| Default | Shows current color swatch |
| Hover (trigger) | Light background |
| Open | Panel visible |
| Spectrum drag | Thumb moves with cursor |
| Input focused | Blue border on hex input |
| Preset hover | Scale 1.1, slight glow |
| Preset selected | White border/ring |

**Responsive Behavior**
- Desktop: Panel as dropdown.
- Tablet: Same.
- Mobile: Bottom sheet with larger touch targets.

**Accessibility**
- Trigger: `aria-label="Choose color"`, `aria-haspopup="dialog"`.
- Spectrum: `role="slider"` with relevant aria attributes.
- Presets: `role="button"`, `aria-label="Red"`, `aria-label="#FF0000"`.
- Hex input: `aria-label="Hex color value"`.
- Keyboard: Arrow keys to adjust spectrum, Enter to select preset.
- Announce color changes: `aria-live="polite"`.

**Animation**
- Panel: Fade + scale 200ms.
- Color change: Smooth transition 100ms.
- Preset hover: Scale 1.1 over 150ms.

**Interaction Rules**
- Click preset sets color and may close panel (optional).
- Spectrum: Click or drag to choose custom color.
- Hex input: Validate on blur or Enter.
- Eyedropper: Activates system color picker (where supported).
- Apply color immediately or with "Apply" button depending on context.

**Do**
- Choose a highlight color for notes.
- Pick a theme accent color.
- Select a color for a custom tag.

**Do Not**
- Show only a spectrum picker without presets.
- Ignore accessibility of color labels (name the colors).
- Close picker without saving the selection if user explicitly made changes.

---

## 3. Cards

---

### 3.1 Generic Card

**Purpose**  
The Generic Card is a flexible content container used to group related information in a scannable, bite-sized format.

**Usage Rules**
- Use to present a single concept or item with related actions.
- Cards should be independent — each card is a standalone unit.
- Do not put cards inside other cards.
- Cards in a grid should be equal height.
- Clickable or actionable depending on context.

**Visual Rules**
- Container: White background, border-radius 12px.
- Shadow: Elevation level 1 (subtle), or 1px solid border (#E8EAED).
- Padding: 16px or 20px.
- Max-width: Varies by grid (typically 320px-400px).
- Full-width on mobile.
- Header, body, footer sections (optional).

**Spacing**
- Internal padding: 16px.
- Between elements inside: 12px.
- Between cards in a grid: 16px.
- Card margin-bottom: 16px (when stacked).

**States**

| State | Visual Change |
|-------|--------------|
| Default | White bg, shadow or border |
| Hover (clickable) | Elevation lifts to level 2, subtle transform |
| Pressed (clickable) | Scale 0.98, shadow reduces |
| Focused | Focus ring around card |
| Disabled | 60% opacity, no interaction |
| Selected | Blue left border or blue outline |

**Responsive Behavior**
- Desktop: Fixed-width grid (3-4 columns).
- Tablet: 2-3 columns.
- Mobile: Full-width single column.

**Accessibility**
- Role: `article` or `region`.
- Clickable cards: `role="button"`, `tabindex="0"`.
- `aria-label` if card content is not descriptive enough.
- Interactive elements inside card must have their own tab order.

**Animation**
- Hover: Shadow transitions 200ms ease.
- Entrance: Staggered fade-in + slide-up 300ms.

**Interaction Rules**
- Entire card clickable only if one primary action exists.
- If multiple actions exist, only make internal buttons clickable.
- Keyboard: Enter to activate card link.

**Do**
- Show a lesson preview with title, description, and progress.
- Present a resource link with metadata.
- Display a user's recent activity.

**Do Not**
- Nest cards within cards.
- Use cards for continuous text content like articles.
- Make cards with multiple conflicting actions clickable as a whole.

---

### 3.2 Knowledge Card

**Purpose**  
The Knowledge Card presents a piece of learned information, concept, or fact in a structured, digestible format.

**Usage Rules**
- Use for individual knowledge items in a knowledge base or library.
- Title, definition/concept, source, tags, and actions.
- Bookmarkable and shareable.
- May include a "Learn More" link to detailed content.
- Supports rich media (images, diagrams) within the card.

**Visual Rules**
- Root: Generic Card styling.
- Header: Title (18px, Bold), category badge (12px, rounded pill).
- Body: Description/concept text (14px, Regular, max 3 lines).
- Media: Optional image or diagram, 16:9 ratio, border-radius 8px.
- Footer: Source attribution (12px, gray), tags row, action buttons.
- Bookmark icon: Top-right corner.

**Spacing**
- Card padding: 20px.
- Title to description: 8px.
- Description to tags: 12px.
- Tags to footer: 12px.

**States**
- Hover: Card lifts slightly (shadow elevation +1).
- Bookmark toggle: Icon changes.
- Read/Unread indicator: Left border shade (blue = unread, gray = read).

**Responsive Behavior**
- Desktop: Grid of 3 columns.
- Tablet: Grid of 2 columns.
- Mobile: Single column, full-width.

**Accessibility**
- `aria-label` with card title.
- Tag list: `role="list"` with `aria-label="Tags"`.
- Bookmark: Button with `aria-pressed`.

**Animation**
- Entrance: Staggered fade-in.
- Bookmark: Icon morph 250ms.

**Interaction Rules**
- Click card navigates to knowledge detail.
- Bookmark toggles via icon button.
- Share triggers platform share menu.

**Do**
- Save a key concept from a lesson.
- Display a term and its definition.
- Show a historical fact with source.

**Do Not**
- Overcrowd with too much text — keep to key information.
- Use for complex multi-step instructions.
- Mix unrelated knowledge items in a single card.

---

### 3.3 Memory Card

**Purpose**  
The Memory Card is used in spaced repetition (SRS) systems to present a question or prompt with the answer hidden until the user requests it.

**Usage Rules**
- Use in review sessions for active recall practice.
- Front: A question, prompt, or term.
- Back: Answer, definition, or explanation.
- User rates recall after seeing the back.
- Cards are scheduled based on SRS algorithm.
- One concept per card.

**Visual Rules**
- Same base as Generic Card.
- Front: Question in 18px Bold, centered; optional hint at bottom.
- Back: Answer in 16px Regular; optional example or mnemonic.
- Flip animation: 3D card flip (CSS 3D transform).
- Rating buttons: Again (red), Hard (orange), Good (green), Easy (blue) — after flip.
- Card aspect ratio: 3:2 or 4:3.
- Status indicator: "New," "Learning," "Review," "Mastered" badge.

**Spacing**
- Card padding: 24px.
- Content center-justified.
- Rating buttons margin-top: 24px.
- Button gap: 8px.

**States**

| State | Visual Change |
|-------|--------------|
| Front | Question visible, no answer |
| Back | Answer visible |
| Flipping | 3D transform in progress |
| Rated | Card exits with animation |
| New | Gray badge "New" |
| Mastered | Gold badge "Mastered" |

**Responsive Behavior**
- Desktop: Centered card, max-width 600px.
- Tablet: Same, max-width 500px.
- Mobile: Full-width with 16px padding, aspect ratio adjusts.

**Accessibility**
- Flip button: `aria-label="Reveal answer"`.
- `aria-live="polite"` for answer text.
- Rating buttons: `aria-label` for each difficulty.
- Keyboard: Space to flip, 1-4 for ratings.

**Animation**
- Flip: 3D Y-axis rotation 600ms.
- Card entrance: Slide in from right 300ms.
- Card exit on rating: Slide out to left 200ms.
- Rating button press: Scale 0.95.

**Interaction Rules**
- Must flip before rating.
- After rating, next card appears.
- Cannot go back to previous card without ending session.
- Session ends when all cards rated.

**Do**
- Review a vocabulary term (front: word, back: definition).
- Recall a historical date (front: event, back: date).
- Practice a formula (front: formula name, back: equation).

**Do Not**
- Put multiple concepts on one card.
- Allow skipping cards without rating (optional: bury card).
- Show the answer without user action.

---

### 3.4 Story Card

**Purpose**  
The Story Card presents a narrative or case study in an engaging, visually rich format that encourages reading.

**Usage Rules**
- Use for storytelling content: case studies, historical narratives, lesson stories.
- Progressively reveals content as user scrolls within the card.
- Rich media support: images, pull quotes, embeds.
- "Continue Reading" expand action for long stories.
- Bookmark and share actions available.

**Visual Rules**
- Card: Full-width on mobile, max-width 720px on desktop.
- Hero image: 16:9 or 2:1 ratio at top, border-radius 12px top corners.
- Title: 24px Bold, dark.
- Subtitle: 16px Regular, gray (author, date, read time).
- Body: 16px Regular, 1.6 line-height.
- Pull quotes: 20px Italic, blue left border.
- "Continue Reading" link at truncation point.
- Tags at bottom.

**Spacing**
- Padding: 24px.
- Image to title: 16px.
- Title to subtitle: 8px.
- Subtitle to body: 16px.
- Paragraph gap: 12px.
- Tags margin-top: 16px.

**States**

| State | Visual Change |
|-------|--------------|
| Default (collapsed) | Shows preview, "Continue Reading" button |
| Expanded | Full content visible |
| Bookmarked | Bookmark icon filled |

**Responsive Behavior**
- Desktop: Centered, max-width 720px.
- Tablet: Max-width 600px.
- Mobile: Full-width, smaller images.

**Accessibility**
- `aria-expanded` for expandable content.
- Images: `alt` text describing the image.
- Skip to content link for long stories.

**Animation**
- Expand: Content slides down 300ms.
- Image parallax: Subtle movement on scroll.

**Interaction Rules**
- "Continue Reading" expands inline (no page navigation).
- Long stories: Save reading progress automatically.
- Share: Share story URL.

**Do**
- Tell the history of a scientific discovery.
- Present a case study in a lesson.
- Share a user's learning journey story.

**Do Not**
- Use for non-narrative content (use Knowledge Card).
- Auto-expand long stories.
- Hide the "Continue Reading" affordance.

---

### 3.5 Quiz Card

**Purpose**  
The Quiz Card presents a single quiz question with its options, feedback, and explanation in a self-contained format.

**Usage Rules**
- Use one card per quiz question.
- Supports multiple choice, true/false, and multi-select.
- Show immediate feedback after answering.
- Explanation reveals after answering.
- Track time spent per question.
- Progress indicator within the card.

**Visual Rules**
- Root: Generic Card styling.
- Header: Question number (e.g., "Question 3 of 10"), progress bar below.
- Question: 18px Bold, 1.5 line-height.
- Options: Quiz Option Buttons (see 1.12).
- Feedback: After answering — green/red banner with explanation.
- Explanation: 14px Regular, gray, with source link.
- Timer: Small, top-right corner.

**Spacing**
- Card padding: 20px.
- Question margin-bottom: 16px.
- Options margin-bottom: 12px.
- Feedback margin-top: 16px.
- Explanation margin-top: 12px.

**States**

| State | Visual Change |
|-------|--------------|
| Unanswered | Question + options visible, waiting for input |
| Answered (correct) | Green feedback banner, correct option highlighted green |
| Answered (incorrect) | Red feedback banner, correct option green, user's red |
| Review | Full explanation visible |
| Timed out | Red banner "Time's up" |

**Responsive Behavior**
- Desktop: Max-width 720px, centered.
- Tablet: Same.
- Mobile: Full-width.

**Accessibility**
- Role: `region`, `aria-label="Question 3 of 10"`.
- Progress: `aria-valuenow`, `aria-valuemin`, `aria-valuemax`.
- Feedback: `aria-live="polite"`.
- Keyboard: Tab through options, Enter to select.

**Animation**
- Option selection: Radio fill 200ms.
- Feedback reveal: Slide down 300ms.
- Correct: Green glow 500ms.
- Incorrect: Shake 300ms.
- Transition to next question: Slide left 300ms.

**Interaction Rules**
- Must select an option before proceeding.
- After answering, show feedback immediately.
- "Next" button appears after feedback.
- Cannot change answer after submission.
- Timer auto-submits when time expires.

**Do**
- Test comprehension after a lesson.
- Practice for an exam.
- Assess knowledge retention.

**Do Not**
- Skip feedback — always explain why answer is correct/incorrect.
- Allow multiple submissions for the same question.
- Show the correct answer before the user answers.

---

### 3.6 Flashcard

**Purpose**  
The Flashcard is the core component of the SRS-based review system, presenting a prompt on the front and the answer on the back.

**Usage Rules**
- Use within flashcard decks organized by topic or lesson.
- One discrete fact per card.
- Supports text, images, and code snippets.
- User flips to reveal answer, then rates recall.
- Cards are automatically scheduled for review based on SRS algorithm.

**Visual Rules**
- Card: 3:2 aspect ratio, max-width 600px, centered.
- Front: White background, content centered.
- Back: Light cream or off-white background (#FEFEFE) to differentiate.
- Content: Question 20px Bold (front), Answer 18px Regular (back).
- Deck name: Top bar, 12px gray.
- Card number: "5 of 20" bottom-right.
- Tags: Bottom of back side.
- Hint: Optional, 14px gray italic at bottom of front ("Tap for hint" expands).

**Spacing**
- Inner padding: 32px.
- Content vertical centering.
- Tag margin-top: 16px.
- Card number: Absolute bottom-right.

**States**

| State | Visual Change |
|-------|--------------|
| Front | Question visible |
| Back | Answer visible, slightly different bg |
| Flipping | 3D transform |
| Hint visible | Hint text slides up on front |
| Responded | Card exits |

**Responsive Behavior**
- Desktop: Fixed aspect ratio, centered.
- Tablet: Same, slight size reduction.
- Mobile: Full-width with 16px padding, aspect ratio maintained.

**Accessibility**
- Flip: `aria-label="Tap to reveal answer"`.
- `aria-live` for answer content.
- Card role: `article`.
- Keyboard: Space to flip, arrow keys between cards.

**Animation**
- Flip: 3D Y-axis rotation 600ms.
- Card shuffle: Stack shuffle animation.
- Card entry: Slide from right 300ms.
- Card exit: Slide left 200ms.

**Interaction Rules**
- Tap/click to flip.
- After flip, rating buttons appear.
- Swipe left/right for next/previous (optional).
- Keyboard shortcuts: Space flip, 1-4 rate.
- Session complete shows summary.

**Do**
- Vocabulary: Word on front, definition on back.
- Formula: Name on front, equation on back.
- Q&A: Question on front, answer on back.

**Do Not**
- Put paragraphs on a flashcard — one concept only.
- Allow editing during review mode.
- Mix different subjects in one deck without clear labeling.

---

### 3.7 Lesson Card

**Purpose**  
The Lesson Card represents a single lesson or module in a course or learning path, showing its title, progress, and status.

**Usage Rules**
- Use in lesson lists, curriculum views, and dashboards.
- Click to navigate to lesson content.
- Show completion status: Not started, In progress, Completed.
- Display estimated duration and difficulty level.
- May show a progress bar for partial completion.

**Visual Rules**
- Card: Generic Card base.
- Thumbnail: Left-aligned, 80px x 80px (desktop), 60px x 60px (mobile), border-radius 8px.
- Title: 16px SemiBold, dark.
- Description: 14px Regular, gray, 2 lines max.
- Metadata row: Duration (clock icon), difficulty (bar indicator), XP reward.
- Progress: Thin bar at bottom, 4px height.
- Status badge: "In Progress" (blue), "Completed" (green), "New" (gray).
- Locked overlay: Semi-transparent gray overlay with lock icon if prerequisite not met.

**Spacing**
- Card padding: 16px.
- Thumbnail margin-right: 16px.
- Title to description: 4px.
- Description to metadata: 8px.
- Metadata row gap: 16px.
- Progress bar bottom: 0px (flush with card bottom, rounded bottom corners).

**States**

| State | Visual Change |
|-------|--------------|
| Default (not started) | Gray badge, no progress |
| Hover | Shadow lifts |
| In progress | Blue badge, partial progress bar |
| Completed | Green badge, full progress bar, green left border |
| Locked | Gray overlay, lock icon, no interaction |
| Focused | Focus ring |

**Responsive Behavior**
- Desktop: Grid or list layout.
- Tablet: List layout or 2-column grid.
- Mobile: Full-width list.

**Accessibility**
- Role: `article`, `aria-label="Lesson: [title]"`.
- Progress: `aria-valuenow`.
- Locked: `aria-disabled="true"`.
- Clickable: `tabindex="0"`, `role="button"`.

**Animation**
- Hover: Shadow lift 200ms.
- Progress fill: Width animates 500ms.
- Status change: Badge crossfade 300ms.

**Interaction Rules**
- Click opens lesson (unless locked).
- Locked lesson: Show tooltip explaining prerequisite.
- Long press for context menu (optional).
- Swipe for actions on mobile (optional).

**Do**
- List lessons in a course curriculum.
- Show recommended next lesson.
- Display search results for lessons.

**Do Not**
- Show locked lessons without explanation.
- Make completed lessons unclickable — allow re-review.
- Crowd the card with too much metadata.

---

### 3.8 Achievement Card

**Purpose**  
The Achievement Card displays a badge or accomplishment that the user has earned or can earn in the learning journey.

**Usage Rules**
- Use in achievement/profile pages, notifications, and celebration modals.
- Show earned achievements with a colorful badge.
- Show locked achievements as gray outlines to encourage progress.
- Include title, description, icon, and date earned.
- Rarity indicator: Common, Rare, Epic, Legendary.

**Visual Rules**
- Card: Compact, centered layout.
- Badge icon: 64px x 64px, circular or shape-based.
- Earned: Full color, gradient background, glow effect.
- Locked: Gray outline, 50% opacity.
- Title: 16px Bold.
- Description: 14px Regular, gray.
- Date earned: 12px gray, below description.
- Rarity: Corner badge or border color:
  - Common: Gray.
  - Rare: Blue.
  - Epic: Purple.
  - Legendary: Gold/Orange.
- Progress: If locked, show "3/5 lessons" progress text.

**Spacing**
- Card padding: 20px.
- Icon margin-bottom: 12px.
- Title margin-bottom: 4px.
- Description margin-bottom: 8px.
- Date: bottom.

**States**

| State | Visual Change |
|-------|--------------|
| Earned | Full color badge, glow effect |
| Locked | Gray outline, 50% opacity |
| Newly earned | Celebration animation (confetti/sparkle) |
| Hover | Slight scale 1.02, info tooltip |

**Responsive Behavior**
- Desktop: Grid of 3-4 columns.
- Tablet: Grid of 2-3 columns.
- Mobile: Grid of 2 columns.

**Accessibility**
- Badge image: `alt` text describing the achievement.
- `aria-label="Achievement: [name] - [earned/locked]"`.
- `aria-live` for newly earned achievement announcement.

**Animation**
- Entrance (earned): Scale from 0 -> 1.1 -> 1.0 bounce 500ms.
- Glow: Subtle pulse 2s infinite for legendary.
- Locked: No animation.

**Interaction Rules**
- Click shows detail tooltip or modal with earning criteria.
- Newly earned achievements show auto-celebration.
- Share achievement to social media.

**Do**
- "Completed 10 Lessons" badge.
- "Perfect Quiz Score" achievement.
- "7-Day Streak" milestone.

**Do Not**
- Make achievements impossible to earn — always attainable.
- Use confusing rarity indicators without legend.
- Over-celebrate common achievements.

---

### 3.9 Character Card

**Purpose**  
The Character Card displays information about a character or knowledge spirit in the learning platform, including their appearance, role, and relationship to the user.

**Usage Rules**
- Use in character selection screens, character profile, and companion interfaces.
- Shows character avatar, name, role, level, and relationship status.
- May include personality traits, special abilities, and backstory.
- Interactive: click to select or view detailed profile.
- Characters may be locked (gated by progress or achievement).

**Visual Rules**
- Card: Portrait layout, image top, info bottom.
- Character art: 3:4 aspect ratio illustration, full color.
- Name: 20px Bold.
- Role/title: 14px Medium, gray (e.g., "Knowledge Sage").
- Level: 12px, badge style.
- Relationship bar: "Bond level 3/10" with progress.
- Locked: Glass morphism overlay, lock icon, unlock condition text.
- Rarity frame: Border color by rarity.
- Personality tags: Small chips (e.g., "Wise," "Curious," "Funny").

**Spacing**
- Card border-radius: 16px.
- Image top: No padding at top (image flush with card top corners).
- Info padding: 16px.
- Name to role: 4px.
- Role to level/bond: 8px.
- Tags margin-top: 8px.

**States**

| State | Visual Change |
|-------|--------------|
| Default (selected) | Full color, active border |
| Unselected | Dimmed, no border |
| Locked | Glass overlay, lock icon |
| Hover (unlocked) | Slight scale 1.03, glow |
| Bond level up | Sparkle effect on bond bar |
| Focused | Focus ring |

**Responsive Behavior**
- Desktop: Grid of 3-4 cards.
- Tablet: Grid of 2-3 cards.
- Mobile: Horizontal scrollable row, card width 180px.

**Accessibility**
- Art: `alt` text describing character appearance.
- `aria-label="Character: [name], Level [X]"`.
- Locked: "Locked, complete lesson 5 to unlock."

**Animation**
- Hover: Scale 1.03 200ms.
- Bond increase: Bar fills with sparkle 500ms.
- Unlock: Glass overlay dissolves 500ms.

**Interaction Rules**
- Click to select character as active companion.
- Long press for detailed profile.
- Locked characters show unlock criteria on click.
- Bond increases through learning activities.

**Do**
- Select a learning companion.
- View character progression.
- Unlock new characters through achievements.

**Do Not**
- Make paywalled characters without alternative earn paths.
- Overwhelm with too many locked characters initially.
- Use violent or inappropriate character designs for an educational platform.

---

### 3.10 Timeline Card

**Purpose**  
The Timeline Card presents an event or milestone within a chronological timeline, typically in a learning journey or history view.

**Usage Rules**
- Use in timeline views showing learning progress, historical events, or project milestones.
- Connected vertically or horizontally with other timeline cards via a line.
- Each card represents one point in time.
- Past events are completed/filled; future events show as pending.
- Current event highlighted.

**Visual Rules**
- Card: Compact, attached to a timeline line (left dot/connector).
- Dot: 12px circle on the timeline, filled for completed, outlined for pending.
- Date: 12px Bold, gray (above or below dot).
- Title: 14px SemiBold.
- Description: 12px Regular, gray, 2 lines.
- Status icon: Checkmark (completed), play icon (current), lock (pending).
- Card: Minimal border or no border, subtle shadow.
- Width: 280px (desktop), variable.

**Spacing**
- Card padding: 12px 16px.
- Timeline line: 2px thick, gray, 24px from left.
- Card to dot gap: 12px.
- Between timeline items: 24px.

**States**

| State | Visual Change |
|-------|--------------|
| Past (completed) | Green dot, checkmark, card normal |
| Current | Blue dot, pulsing, card elevated |
| Future (locked) | Gray outlined dot, lock icon, card dimmed |
| Past hover | Card lifts slightly |
| Current click | Active — opens associated content |

**Responsive Behavior**
- Desktop: Horizontal or vertical timeline.
- Tablet: Vertical timeline.
- Mobile: Vertical timeline, full-width cards.

**Accessibility**
- Role: `list` for timeline, `listitem` for each event.
- Timeline: `aria-label="Learning timeline"`.
- `aria-current="step"` for current item.

**Animation**
- Dot completion: Dot fills from outline 300ms, checkmark scales in.
- Current dot: Pulse animation 2s.
- New item added: Slide into timeline 400ms.

**Interaction Rules**
- Click past or current timeline items to view details.
- Future items show unlock date or requirement on click.
- Timeline scrolls horizontally on desktop, vertically on mobile.

**Do**
- Show a user's learning journey over time.
- Present historical events in a history lesson.
- Display project milestones.

**Do Not**
- Put too much text — timeline items should be concise.
- Mix different time scales without clear labels.
- Make the timeline non-linear without indication.

---

### 3.11 Map Card

**Purpose**  
The Map Card displays a location-based or conceptual map preview within a card, used for geography lessons or knowledge space navigation.

**Usage Rules**
- Use in geography lessons, location-based content, or knowledge map overviews.
- Shows a small map preview with a pin or highlighted area.
- Click to expand to full map view.
- May include location name, coordinates, and related content count.

**Visual Rules**
- Card: Generic Card with map as background.
- Map preview: 16:9 or 4:3 ratio, rounded top corners.
- Map pin: Colored pin icon at the relevant location.
- Overlay gradient: Bottom-to-top dark gradient for text readability.
- Location name: 18px Bold, white on overlay.
- Address/region: 14px Regular, white.
- Stats: "12 related lessons" or "5 nearby topics."
- Expand button: "View full map" tertiary button.

**Spacing**
- Card padding: 0 (map flush to edges).
- Text overlay padding: 16px.
- Overlay gradient height: 60px.

**States**

| State | Visual Change |
|-------|--------------|
| Default | Map with pin |
| Hover | Pin scales 1.2, "View full map" appears |
| Focused | Focus ring |

**Responsive Behavior**
- Desktop: 2-3 column grid.
- Tablet: 2 columns.
- Mobile: Full-width.

**Accessibility**
- Map image: `alt` text with location description.
- Pin: `aria-label="Location: [name]"`.
- Button: "Explore [location] on map."

**Animation**
- Pin hover: Bounce 300ms.
- Card hover: Shadow lift 200ms.
- Expand to full map: Transition 400ms.

**Interaction Rules**
- Click map area expands to full map view.
- Pin shows tooltip with location name on hover.

**Do**
- Show a historical site location in a history lesson.
- Present a user's learning locations around the world.
- Display a concept map region.

**Do Not**
- Use Map Card for non-geographic content.
- Show inaccurate or outdated map data.
- Omit the expand affordance — users should know they can view the full map.

---

### 3.12 Source Citation Card

**Purpose**  
The Source Citation Card displays a reference, citation, or source attribution for content used in lessons, knowledge cards, or quiz questions.

**Usage Rules**
- Use whenever content references an external source.
- Shows author, title, publication, date, and link.
- Supports academic citations (APA, MLA, Chicago).
- Click to open source link in a new tab.
- Include "Verify this source" action.

**Visual Rules**
- Card: Compact, minimal, light gray background (#F8F9FA).
- Border-left: 3px solid #1A73E8.
- Icon: Bookmark or link icon top-left.
- Title: 14px SemiBold (source title).
- Author: 13px Regular, gray.
- Publication info: 12px Regular, lighter gray.
- Link: 13px, blue, truncated.
- DOI/ISBN: 12px gray (if applicable).
- Badge: "Peer-reviewed," "Verified," or "Wikipedia."

**Spacing**
- Card padding: 12px 16px.
- Icon margin-right: 8px.
- Title to author: 4px.
- Author to publication: 2px.
- Publication to link: 4px.

**States**

| State | Visual Change |
|-------|--------------|
| Default | Gray bg, blue left border |
| Hover | Slightly darker bg (#E8EAED) |
| Link hover | Link underlines |
| Verified | Green badge "Verified" |
| Focused | Focus ring |

**Responsive Behavior**
- Desktop: Inline with content or in sidebar.
- Tablet: Same.
- Mobile: Full-width.

**Accessibility**
- Link: `aria-label="Open source: [title]"`, `target="_blank"`, `rel="noopener"`.
- Role: `cite`.

**Animation**
- Hover: Background color 150ms.

**Interaction Rules**
- Click card or title opens source in new tab.
- "Verify" checks source credibility and updates badge.
- Copy citation button (APA/MLA) for user reference.

**Do**
- Cite a research paper used in a lesson.
- Attribute a quote to its author.
- Link to a reference article for further reading.

**Do Not**
- Cite sources without verifying the link is functional.
- Use Source Citation Card for internal content links.
- Overwhelm the content with too many citations — show expandable list.

---

### 3.13 AI Recommendation Card

**Purpose**  
The AI Recommendation Card suggests content, lessons, or actions to the user based on their learning behavior and preferences.

**Usage Rules**
- Use in dashboards, home feeds, and recommendation widgets.
- AI-generated title and rationale for the recommendation.
- Shows predicted relevance or match percentage.
- User can dismiss, accept, or provide feedback ("Not relevant").
- Recommendations update periodically based on user activity.

**Visual Rules**
- Card: Generic Card with an AI badge.
- AI badge: "AI Recommended" pill, 12px, gradient background (blue-purple).
- Title: 16px SemiBold.
- Rationale: 14px Regular, gray, 2 lines (e.g., "Based on your interest in Biology...").
- Thumbnail: Small image or icon representing the recommended content.
- Match score: Badge or bar "95% match."
- Action buttons: "Start Lesson," "View Details," or "Dismiss."
- Dismiss: "X" icon button top-right.

**Spacing**
- Card padding: 16px.
- AI badge margin-bottom: 8px.
- Title to rationale: 4px.
- Rationale to actions: 12px.
- Thumbnail: 60px x 60px, margin-right: 12px.

**States**

| State | Visual Change |
|-------|--------------|
| Default | AI badge, content visible |
| Dismissed | Fades out, replaced with next recommendation |
| Feedback | "Thanks for your feedback" text briefly |
| Hover | Shadow lifts |
| Loading | Skeleton while AI generates |

**Responsive Behavior**
- Desktop: 2-3 columns in widget area.
- Tablet: 2 columns.
- Mobile: Single column, full-width.

**Accessibility**
- AI badge: `aria-label="AI recommended"`.
- Dismiss: `aria-label="Dismiss recommendation"`.
- Feedback: `aria-live="polite"`.
- Role: `article`.

**Animation**
- Entrance: Slide in from right 300ms.
- Dismiss: Slide out to left + fade 200ms.
- Refresh: Old card fades out, new fades in 300ms.

**Interaction Rules**
- Click recommendation navigates to content.
- Dismiss removes card and optionally logs dislike.
- Feedback improves future recommendations.
- Recommendations refresh on page reload or periodic polling.

**Do**
- "Recommended lesson: Introduction to Genetics."
- "Based on your quiz results, review these flashcards."
- "Users who enjoyed this also studied..."

**Do Not**
- Make recommendations feel intrusive — limit to dedicated sections.
- Show the same recommendations repeatedly.
- Use AI recommendations without showing why.

---

### 3.14 AI Source Card

**Purpose**  
The AI Source Card presents an AI-generated answer or explanation along with its source references, allowing the user to verify AI-provided information.

**Usage Rules**
- Use in AI chat responses, AI explanations, and AI-generated content.
- Shows the AI-generated text with inline source citations.
- Each source is linked to the Source Citation Card.
- Includes confidence score for the answer.
- User can fact-check or report inaccurate information.

**Visual Rules**
- Card: Generic Card with subtle AI gradient border (left or top).
- AI indicator: Small AI icon or "AI" badge.
- Content: 16px Regular, with highlighted citation markers [1], [2].
- Sources list: Compact source items at bottom.
- Confidence indicator: Bar or percentage (e.g., "92% confidence").
- Feedback: Thumbs up/down buttons.
- Copy and Share actions.

**Spacing**
- Card padding: 16px.
- AI badge margin-bottom: 8px.
- Content margin-bottom: 12px.
- Sources list: 8px between items.
- Feedback buttons: Right-aligned, 8px apart.

**States**

| State | Visual Change |
|-------|--------------|
| Default | Content with sources |
| Generating (loading) | Skeleton text, shimmer animation |
| Complete | Full content visible |
| Feedback given | Thumbs up/down selected blue |
| Reported | "Reported" badge shown |
| Hover on citation | Tooltip with source preview |

**Responsive Behavior**
- Desktop: Max-width 720px.
- Tablet: Same.
- Mobile: Full-width.

**Accessibility**
- Role: `article`.
- `aria-live="polite"` for content generation.
- Feedback: `aria-label="Mark as helpful"` / `aria-label="Report issue"`.
- Citations: Links to source cards.

**Animation**
- Text generation: Text appears word by word or sentence by sentence (typewriter effect 30ms per word).
- Sources expand: Slide down 200ms.
- Confidence bar: Animates from 0 to value 500ms.

**Interaction Rules**
- Click citation number scrolls to source at bottom.
- Copy button copies text with citations.
- Thumbs feedback updates AI training.
- Report opens a form with predefined categories.

**Do**
- Answer "What is photosynthesis?" with sources.
- Explain a concept with cited research.
- Provide a summary with source verification.

**Do Not**
- Present AI answers as absolute fact without sources.
- Hide the confidence score.
- Prevent users from reporting inaccuracies.

---

### 3.15 AI Explanation Card

**Purpose**  
The AI Explanation Card provides a detailed, step-by-step explanation of a concept, answer, or solution generated by AI.

**Usage Rules**
- Use when the user requests an explanation for a quiz answer, concept, or problem.
- Structure the explanation with clear steps or sections.
- Include relevant formulas, diagrams (as ASCII or referenced images), and examples.
- Offer multiple explanation levels: Simple, Detailed, Expert.
- User can expand/collapse sections.

**Visual Rules**
- Card: Generic Card.
- Header: "Explain this" tag, difficulty level toggle.
- Explanation: Structured with numbered steps or bullet points.
- Step: Number + title (14px SemiBold) + body (14px Regular).
- Formula box: Light gray bg, monospace font, border-radius 8px.
- Example: "For example:" in italic, with sample.
- Key takeaway: Highlighted box at the end.
- "Was this helpful?" feedback at bottom.

**Spacing**
- Card padding: 20px.
- Step margin-bottom: 16px.
- Formula box padding: 12px, margin 8px 0.
- Key takeaway padding: 12px, margin-top 16px.
- Feedback section margin-top: 16px.

**States**

| State | Visual Change |
|-------|--------------|
| Default (Simple) | Concise explanation |
| Expanded (Detailed) | Full sections visible |
| Expert level | Technical details shown |
| Generating | Skeleton placeholder |
| Feedback given | Selected state on thumbs |

**Responsive Behavior**
- Desktop: Max-width 720px.
- Tablet: Same.
- Mobile: Full-width, smaller formula boxes.

**Accessibility**
- Role: `article`.
- Section headings: `aria-label` for each step.
- Formula: `aria-label` describing the formula in words.
- Expand/collapse: `aria-expanded`.

**Animation**
- Step reveal: Staggered fade-in 200ms each.
- Level switch: Content crossfade 300ms.
- Formula box: Expand from collapsed 250ms.

**Interaction Rules**
- Click difficulty level toggles explanation depth.
- Copy button for individual steps or whole explanation.
- "Ask follow-up" opens AI chat with context.
- Rate explanation quality.

**Do**
- Explain why a quiz answer is correct.
- Walk through a math problem step by step.
- Elaborate on a complex concept from a lesson.

**Do Not**
- Provide overly technical explanations by default — show level toggle.
- Skip examples — they are critical for understanding.
- Make explanations too long without expand/collapse.

---

### 3.16 Assessment Result Card

**Purpose**  
The Assessment Result Card summarizes a user's performance on a quiz, test, or assessment.

**Usage Rules**
- Use after quiz/submission completion.
- Shows score, time taken, questions correct/incorrect/skipped.
- Provides comparative data (percentile, class average).
- Includes strengths and areas for improvement.
- "Review Answers" and "Retry" actions.

**Visual Rules**
- Card: Prominent, celebratory for high scores, encouraging for low scores.
- Score circle: Large circular progress indicator (80px), percentage inside.
- Score color: Green (80-100%), Yellow (60-79%), Red (<60%).
- Stats row: Correct (green), Incorrect (red), Skipped (gray) — with counts.
- Time: Clock icon + duration.
- Strengths: Green tags with topic names.
- Improvement areas: Orange tags with topic names.
- Action buttons: "Review Answers" (primary), "Retry Quiz" (secondary).
- Encouragement message: Dynamic based on score.

**Spacing**
- Card padding: 24px.
- Score circle: Centered, margin-bottom 16px.
- Stats row: 16px between items.
- Strengths/Improvements: 8px gap between tags.
- Actions: 12px between buttons, margin-top 20px.

**States**

| State | Visual Change |
|-------|--------------|
| Passing | Green score, positive message |
| Failing | Red score, encouraging message |
| Perfect score | Gold score, celebration effect |
| Improvement | Shows improvement deltas from previous attempt |

**Responsive Behavior**
- Desktop: Centered, max-width 500px.
- Tablet: Same.
- Mobile: Full-width, score circle slightly smaller (60px).

**Accessibility**
- Role: `region`, `aria-label="Assessment results"`.
- Score: `aria-label="Score 85 percent"`.
- Stats: `aria-live="polite"`.
- Chart: Provide text alternative for score visualization.

**Animation**
- Score circle: Animates from 0 to final percentage 1s ease-out.
- Stats: Count up animation 500ms.
- Strengths/weaknesses: Staggered fade-in 200ms each.
- Perfect score: Confetti burst 1.5s.

**Interaction Rules**
- "Review Answers" opens quiz in review mode.
- "Retry Quiz" starts a new attempt (confirm if in progress).
- Share result to social media (optional).
- Save result to profile.

**Do**
- Show quiz results after a lesson assessment.
- Display end-of-chapter test performance.
- Present weekly quiz summary.

**Do Not**
- Show comparison with other users without consent.
- Make failing scores feel shameful — use encouraging language.
- Skip the review option — users must be able to see what they got wrong.

---

### 3.17 Hint Card

**Purpose**  
The Hint Card provides progressive hints or clues to help users solve a problem without revealing the full answer.

**Usage Rules**
- Use in quizzes, puzzles, or problem-solving exercises.
- Provide multiple levels of hints (1, 2, 3...) increasing in specificity.
- The last hint may essentially give away the answer — user should be warned.
- Track hint usage for analytics (indicates difficulty).
- User must explicitly request each hint level.

**Visual Rules**
- Card: Compact, yellow/amber left border (#FBBC04).
- Header: "Hint" label with lightbulb icon.
- Hint level: "Hint 1 of 3" text.
- Content: 14px Regular, reveals per hint level.
- Progress dots: 3 dots, filled = viewed.
- Penalty text: "-1 point per hint" if scoring is active.
- Close button to dismiss.

**Spacing**
- Card padding: 12px 16px.
- Icon to text gap: 8px.
- Hint level to content: 8px.
- Progress dots: 8px between dots.

**States**

| State | Visual Change |
|-------|--------------|
| Collapsed (unused) | "Need a hint?" link/button |


## 8. AI-Specific

---

### 8.1 AI Chat Bubble (Sent)

**Purpose**  
The AI Chat Bubble (Sent) displays a message sent by the user in the AI chat interface.

**Usage Rules**
- Use in AI chat, Q&A, and tutoring interfaces.
- Right-aligned bubble (LTR layouts).
- Contains user's text, code, or media.
- Shows timestamp and read status.
- Differentiated visually from received messages.

**Visual Rules**
- Container: Rounded rectangle, border-radius 16px (top-right 4px).
- Background: Primary Blue (#1A73E8).
- Text: White, 16px Regular.
- Padding: 12px 16px.
- Max-width: 80% of container.
- Right-aligned with 12px margin from right edge.
- Tail: Small triangle on the top-right corner.
- Timestamp: 11px Regular, gray (#9AA0A6), below bubble, right-aligned.
- Status: Single check (sent), double check (delivered), double check filled (read).
- Link previews: Embedded within the bubble.

**Spacing**
- Bubble padding: 12px 16px.
- Between consecutive sent messages: 4px.
- Between different user messages: 12px.
- Bubble to timestamp: 4px.
- Margin from right edge: 12px.

**States**

| State | Visual Change |
|-------|--------------|
| Sending | Bubble dimmed (70% opacity), spinner replaces check |
| Sent | Single gray checkmark |
| Delivered | Double gray checkmarks |
| Read | Double blue checkmarks |
| Failed | Red exclamation icon, "Tap to retry" |
| Contains media | Media embedded, text below or above |

**Responsive Behavior**
- Desktop: Max-width 60%.
- Tablet: Max-width 70%.
- Mobile: Max-width 80%.

**Accessibility**
- Role: `listitem` within a `log`.
- `aria-label="Your message: [content]"`.
- Status: `aria-label="Sent"`, "Delivered", "Read".
- Link: Screen reader announces links within.

**Animation**
- Send: Bubble slides in from right 200ms.
- Status update: Checkmark icon crossfade 200ms.
- Failed: Shake 300ms.

**Interaction Rules**
- Tap failed message to retry.
- Long press for context menu (copy, delete, select).
- Double-tap to react (optional).

**Do**
- Send a question to the AI tutor.
- Submit an answer for review.
- Request an explanation.

**Do Not**
- Use sent bubble for system messages.
- Allow editing after sending (use edit action instead).
- Clutter with excessive status updates.

---

### 8.2 AI Chat Bubble (Received)

**Purpose**  
The AI Chat Bubble (Received) displays a message from the AI in the chat interface, including responses, explanations, and suggestions.

**Usage Rules**
- Use in AI chat, Q&A, and tutoring interfaces.
- Left-aligned bubble (LTR layouts).
- Contains AI-generated text, code blocks, lists, or media.
- May include source citations and confidence scores.
- Shows AI avatar/name and timestamp.

**Visual Rules**
- Container: Rounded rectangle, border-radius 16px (top-left 4px).
- Background: Light gray (#F1F3F4) or white with border.
- Text: Dark (#202124), 16px Regular.
- Padding: 12px 16px.
- Max-width: 80% of container.
- Left-aligned with 12px margin from left edge.
- Tail: Small triangle on the top-left corner.
- AI Avatar: 28px circle, left of bubble, top-aligned.
- AI Name: "Knowledge AI" or character name, 11px Bold above bubble.
- Timestamp: 11px Regular, gray, below bubble, left-aligned.
- Source citations: Small blue numbers [1], linked to sources.
- Code blocks: Dark background (#1E1E1E), monospace, copy button.
- Confidence: Small indicator, "92% confidence".
- Regenerate button: Below bubble (optional).

**Spacing**
- Bubble padding: 12px 16px.
- Avatar to bubble gap: 8px.
- Between consecutive received messages: 4px.
- Between different AI messages: 12px.
- Timestamp to next message: 8px.

**States**

| State | Visual Change |
|-------|--------------|
| Typing | Typing indicator visible (see 8.3) |
| Streaming | Text appears word by word |
| Complete | Full message rendered |
| Error | "Failed to generate response" with retry |
| Contains code | Code block with syntax highlighting |
| Contains sources | Blue citation numbers |
| Regenerating | Old content fades, new streams in |

**Responsive Behavior**
- Desktop: Max-width 60%.
- Tablet: Max-width 70%.
- Mobile: Max-width 80%.

**Accessibility**
- Role: `listitem` within a `log`.
- `aria-label="AI: [content summary]"`.
- `aria-live="polite"` for streaming content.
- Code blocks: `aria-label="Code block"`.
- Sources: Links with `aria-label`.

**Animation**
- Message entrance: Slide in from left 200ms.
- Text streaming: Characters appear at 30ms per word.
- Code block: Expand from top 300ms.
- Sources reveal: Fade in 200ms.
- Regenerate: Old content fades out 200ms, new fades in 300ms.

**Interaction Rules**
- Click citation number to view source.
- Copy button on code blocks.
- Thumbs up/down for feedback.
- Regenerate button to request new response.
- Long press for context menu (copy, share, report).
- Streaming can be interrupted by new user message.

**Do**
- AI answers a user's question with explanation.
- AI provides step-by-step solution.
- AI recommends resources with sources.

**Do Not**
- Present AI responses as definitive fact without confidence indicator.
- Stream too slowly (target <50ms per word).
- Hide the regenerate option.

---

### 8.3 AI Typing Indicator

**Purpose**  
The AI Typing Indicator shows that the AI is generating a response, using animated dots to signal activity.

**Usage Rules**
- Use in chat interfaces when the AI is processing a response.
- Appears in the same position as a received message.
- Always accompanied by the AI avatar.
- Disappears when the response starts streaming.
- Three animated dots.

**Visual Rules**
- Container: Same style as received chat bubble but smaller.
- Background: Light gray (#F1F3F4).
- Border-radius: 16px (top-left 4px).
- Height: 40px.
- Width: 64px.
- Three dots: 8px circles, gray (#9AA0A6).
- AI Avatar: 28px circle, left-aligned.
- Dots animate with staggered bounce.

**Spacing**
- Container: 64px x 40px.
- Dot gap: 6px.
- Avatar to container: 8px.

**States**

| State | Visual Change |
|-------|--------------|
| Idle (hidden) | Not visible |
| Active | Dots bouncing, visible |
| Cancelled | Disappears immediately |

**Responsive Behavior**
- Consistent across breakpoints.

**Accessibility**
- Role: `status`.
- `aria-label="AI is typing"`.
- `aria-live="polite"`.

**Animation**
- Dot 1: Bounce up 4px, 0ms delay, 400ms cycle.
- Dot 2: Bounce up 4px, 150ms delay, 400ms cycle.
- Dot 3: Bounce up 4px, 300ms delay, 400ms cycle.
- Fade in: 150ms.
- Fade out: 100ms (when typing starts or cancelled).

**Interaction Rules**
- No interaction.
- Cancelled if user sends another message.
- Replaced by streaming message content when ready.

**Do**
- Show while AI generates a response.
- Indicate AI is processing.

**Do Not**
- Keep indicator visible after response starts streaming.
- Show for cached/instant responses.
- Use if response will appear in <500ms.

---

### 8.4 AI Thinking Indicator

**Purpose**  
The AI Thinking Indicator shows that the AI is processing a complex query, performing reasoning, or searching for information before responding.

**Usage Rules**
- Use for complex queries that require reasoning or search.
- More prominent than Typing Indicator.
- Shows processing stage (e.g., "Searching...", "Analyzing...", "Generating...").
- Appears before the typing indicator for longer operations.
- Replaced by streaming text when ready.

**Visual Rules**
- Container: Full-width, centered, or inline in chat.
- Icon: Rotating gear or brain icon, 24px.
- Text: "Thinking..." or stage-specific text, 14px Regular, gray.
- Stage indicator: "Step 2 of 3: Analyzing sources".
- Progress dots or shimmer alongside text.
- Light background pill: border-radius 20px, padding 8px 16px.

**Spacing**
- Container padding: 8px 16px.
- Icon to text: 8px.
- Stage text margin-top: 2px.

**States**

| State | Visual Change |
|-------|--------------|
| Initial | "Thinking..." with spinner |
| Searching | "Searching knowledge base..." |
| Analyzing | "Analyzing sources..." |
| Generating | "Generating response..." |
| Complete | Fades out, replaced by response |
| Error | "Something went wrong" with retry |

**Responsive Behavior**
- Consistent across breakpoints.

**Accessibility**
- Role: `status`.
- `aria-live="polite"`.
- Stage announcements via `aria-label`.

**Animation**
- Icon rotation: 1s continuous spin.
- Stage text: Crossfade 300ms between stages.
- Progress: Bar or dots animate during each stage.
- Fade out: 200ms when done.

**Interaction Rules**
- User can interrupt with a new message.
- No other interaction during thinking.

**Do**
- Show while AI searches knowledge base.
- Indicate complex reasoning in progress.
- Display processing stages for transparency.

**Do Not**
- Use for simple Q&A that responds instantly.
- Keep visible after response starts.
- Block the user from typing while thinking.

---

### 8.5 AI Suggestion Chip

**Purpose**  
The AI Suggestion Chip offers contextual suggestions or follow-up questions that the user can tap to quickly engage with the AI.

**Usage Rules**
- Use below AI responses or on the chat home screen.
- Provides 2-4 quick action chips.
- Suggestions are contextually relevant to the current conversation.
- Tapping a chip sends it as a user message.
- Chips regenerate based on conversation context.

**Visual Rules**
- Container: Horizontal scrollable row.
- Chips: 32px height, pill shape (border-radius 16px).
- Background: Light gray (#F1F3F4) default.
- Hover: Slightly darker (#E8EAED).
- Text: 13px Regular, dark (#202124).
- Border: 1px solid #DADCE0 or none.
- Icon: Optional 14px icon left of text.
- Selected/used: Chip becomes disabled or hidden after use.
- Regenerate: Refresh icon at end of row (optional).

**Spacing**
- Chip height: 32px.
- Chip padding: 0 12px.
- Between chips: 8px.
- Row margin-top: 8px.
- Row margin-bottom: 12px.

**States**

| State | Visual Change |
|-------|--------------|
| Default | Light gray bg, dark text |
| Hover | Darker gray bg, cursor pointer |
| Pressed | Scale 0.97 |
| Used (clicked) | Disabled, 50% opacity |
| Focused | Focus ring |
| Loading (regenerating) | Chips show shimmer skeleton |

**Responsive Behavior**
- Desktop: Wrapping row or scrollable.
- Tablet: Scrollable row.
- Mobile: Horizontal scrollable, snap to first chip.

**Accessibility**
- Role: `button`.
- `aria-label="Suggestion: [chip text]"`.
- `aria-disabled="true"` for used chips.
- Row: `aria-label="Suggested questions"`.

**Animation**
- Entrance: Staggered slide-up 200ms each.
- Click: Chip fades out 200ms, message appears in chat.
- Regenerate: Old chips slide out 200ms, new slide in 200ms.

**Interaction Rules**
- Tap chip sends message to AI.
- Used chips are hidden or disabled to prevent reuse.
- Refresh button regenerates all suggestions.
- Suggestions update based on conversation context.

**Do**
- "Tell me more about X" after an explanation.
- "Give me an example" after a concept.
- "What is the next topic?" after a lesson.

**Do Not**
- Show more than 4 chips at once.
- Repeat the same suggestions in the same conversation.
- Make chips too long (truncate at 30 characters).

---

### 8.6 AI Quick Action Button

**Purpose**  
The AI Quick Action Button provides one-tap access to common AI actions such as summarizing, explaining, translating, or quizzing.

**Usage Rules**
- Use in AI chat header, lesson pages, or floating on content.
- Each button represents a specific AI action.
- Compact icon + label or icon-only.
- Actions are context-aware (available actions depend on current content).

**Visual Rules**
- Container: 40px x 40px (icon-only) or 48px height (with label).
- Background: White or light gray, border-radius 8px.
- Border: 1px solid #DADCE0 or none.
- Icon: 20px, representing the action.
- Label: 12px SemiBold, below or beside icon.
- Shadow (floating variant): Elevation 2.
- Color: Neutral Gray icon default, blue on active/hover.

**Spacing**
- Button: 40px (icon) or 48px (with label).
- Padding: 8px.
- Between buttons: 8px.
- Label to icon: 4px.

**States**

| State | Visual Change |
|-------|--------------|
| Default | Gray icon, label |
| Hover | Blue icon, light blue bg (#E8F0FE) |
| Pressed | Scale 0.95 |
| Active (selected) | Blue bg, white icon |
| Disabled | 38% opacity |
| Loading | Spinner replaces icon |

**Responsive Behavior**
- Desktop: Visible in toolbar or chat header.
- Tablet: Same, fewer visible (overflow menu).
- Mobile: Collapsible into "More" menu.

**Accessibility**
- Role: `button`.
- `aria-label="[Action]: [context]"` (e.g., "Summarize this lesson").
- Tooltip on hover.

**Animation**
- Hover: Icon color + bg transition 150ms.
- Press: Scale 0.95 80ms.
- Loading: Spinner spin.

**Interaction Rules**
- Click triggers the AI action immediately.
- Action results appear in the chat or as an overlay.
- Context determines available actions dynamically.
- Some actions may require confirmation (e.g., "Generate quiz").

**Do**
- "Summarize" button on a lesson page.
- "Explain" button on a complex concept.
- "Translate" button on foreign language content.
- "Quiz Me" button for quick assessment.

**Do Not**
- Show irrelevant actions for the current context.
- Trigger actions without feedback (show loading state).
- Crowd the interface with too many quick actions.

---

### 8.7 AI Context Panel

**Purpose**  
The AI Context Panel shows the current context the AI is aware of, including the user's current lesson, recent activity, and relevant knowledge.

**Usage Rules**
- Use in AI chat as a sidebar or expandable panel.
- Shows what the AI "knows" about the user's current context.
- Includes: Current lesson, recent topics, quiz performance, learning goals.
- User can edit or add context to improve AI responses.
- Collapsible on mobile.

**Visual Rules**
- Panel: Width 280px (desktop sidebar), full-width (mobile).
- Background: White or light gray.
- Border-left: 1px solid #E8EAED (sidebar).
- Header: "AI Context" title, 16px SemiBold, with edit button.
- Section: Current lesson — thumbnail, title, progress bar.
- Section: Recent topics — chip list.
- Section: Performance — mini stat card (last quiz score).
- Section: Goals — editable text field.
- Add context button: "Add context..." text button.

**Spacing**
- Panel padding: 16px.
- Section margin-bottom: 20px.
- Section header: 12px SemiBold uppercase.
- Chips: 28px height, gap 8px.
- Stat card: Compact, 80px width.

**States**

| State | Visual Change |
|-------|--------------|
| Default | Context visible, AI badge |
| Empty | "No context yet" message |
| Editing | Field highlights blue, save/cancel buttons |
| Collapsed (mobile) | Hidden, toggle button visible |
| Updating | Skeleton while AI refreshes context |

**Responsive Behavior**
- Desktop: Sidebar, persistent.
- Tablet: Collapsible sidebar or slide-out panel.
- Mobile: Bottom sheet or hidden behind icon.

**Accessibility**
- Role: `complementary` (sidebar).
- `aria-label="AI Context Panel"`.
- Editable fields: `aria-label` for each.
- Collapse toggle: `aria-expanded`.

**Animation**
- Slide in/out: 250ms ease (sidebar toggle).
- Edit mode: Field border transition 200ms.
- Context update: Content crossfade 300ms.

**Interaction Rules**
- User can edit context fields directly.
- Changes improve AI response relevance.
- AI may request additional context.
- Panel state persists across chat sessions.

**Do**
- Show current lesson context to the AI.
- Display recent quiz performance.
- Allow user to set learning goals.

**Do Not**
- Expose sensitive user data without consent.
- Make the panel too large (keep it supplementary).
- Auto-update context without user awareness.

---

### 8.8 AI Confidence Indicator

**Purpose**  
The AI Confidence Indicator communicates how confident the AI is in its response, helping users evaluate reliability.

**Usage Rules**
- Use alongside AI responses, explanations, and answers.
- Shows confidence as a percentage or level (High, Medium, Low).
- Color-coded for quick assessment.
- Lower confidence responses should encourage verification.
- Not shown for simple, factual responses with high confidence.

**Visual Rules**
- Container: Inline badge or small bar.
- Badge: Pill shape, 20px height, padding 4px 8px.
- High confidence: Green (#34A853), "High" or "95%".
- Medium confidence: Yellow (#FBBC04), "Medium" or "75%".
- Low confidence: Red (#EA4335), "Low" or "50%".
- Bar variant: 4px height, 60px width, colored fill.
- Icon: Shield or brain icon with confidence level.
- Text: 11px Bold, white on colored bg.
- Tooltip: "Confidence based on source availability and relevance."

**Spacing**
- Badge height: 20px.
- Padding: 4px 8px.
- Margin from response: 8px.
- Bar width: 60px, height 4px.

**States**

| State | Visual Change |
|-------|--------------|
| High | Green badge/bar |
| Medium | Yellow badge/bar |
| Low | Red badge/bar |
| Unknown | Gray badge/bar |
| Loading | Skeleton shimmer |

**Responsive Behavior**
- Consistent across breakpoints.
- Badge inline with text on desktop, same on mobile.

**Accessibility**
- `aria-label="Confidence: High - 95 percent"`.
- Color is not the only indicator — include text.
- Tooltip provides explanation.

**Animation**
- Confidence reveal: Bar fills from 0 to value 500ms.
- Level change: Badge crossfade 300ms.
- Low confidence: Subtle attention pulse.

**Interaction Rules**
- No direct interaction.
- Hover shows tooltip with explanation.
- Low confidence responses may show verification sources.

**Do**
- Show 95% for well-sourced, factual answers.
- Show 70% for synthesized information.
- Show 45% for speculative or ambiguous answers.

**Do Not**
- Show confidence for trivial responses (greetings, confirmations).
- Override user trust — confidence is informative, not definitive.
- Show confidence without explanation of what it means.

---

### 8.9 AI Character Avatar (with Expressions)

**Purpose**  
The AI Character Avatar represents the AI assistant or learning companion with dynamic expressions and animations that convey personality and engagement.

**Usage Rules**
- Use in AI chat, companion panel, and onboarding experiences.
- Shows the character with different expressions based on context:
  - Neutral: Default listening state.
  - Thinking: Processing or searching.
  - Happy: Correct answer, achievement, greeting.
  - Encouraging: User struggling, providing motivation.
  - Explaining: Delivering educational content.
  - Celebrating: User milestone, perfect score.
- Animated transitions between expressions.
- Can be static or animated illustration.

**Visual Rules**
- Shape: Circular or rounded frame, 48px-80px.
- Art style: Consistent with the character's design.
- Expressions: Eyes, mouth, and posture change per state.
- Background: Transparent or subtle glow matching emotion.
- Animation: Subtle idle animation (breathing, blinking).
- Accessories: Glasses, hat, or theme-appropriate items.
- Border: 2px colored ring for active/engaged state.

**Spacing**
- Avatar size: 48px (chat), 56px (panel), 80px (onboarding).
- Status indicator: 12px dot (optional).
- Expression transitions: Immediate or 200ms.

**States**

| State | Visual Change |
|-------|--------------|
| Neutral | Relaxed eyes, slight smile |
| Thinking | Eyes looking up, "hmm" expression |
| Happy | Wide eyes, big smile, cheeks |
| Encouraging | Warm smile, nod |
| Explaining | Pointing gesture, focused eyes |
| Celebrating | Stars/sparkles, big grin |
| Idle | Subtle blink every 3s, gentle breathing |
| Listening | Slight head tilt, attentive eyes |
| Error/apologetic | Sad eyes, apologetic expression |

**Responsive Behavior**
- Desktop: Full size.
- Tablet: Same.
- Mobile: Slightly smaller in chat (40px).

**Accessibility**
- Role: `img`.
- `aria-label="AI Companion - [current emotion]"` (e.g., "AI Companion - Happy").
- `aria-live="polite"` for expression changes.

**Animation**
- Expression transition: Crossfade 300ms, features morph.
- Idle: Slow breathing (scale 1.0 -> 1.02 -> 1.0, 4s cycle).
- Blink: Eyes close 100ms, every 3-5s.
- Celebrating: Bounce 500ms, sparkle particles 1.5s.
- Thinking: Eyes shift side to side 2s cycle.
- Happy: Cheeks appear, smile widens 300ms.

**Interaction Rules**
- No direct interaction (decorative/ambient).
- Expression changes are triggered by conversation context.
- Clicking may trigger a greeting or tip (optional).

**Do**
- Show happy expression when user answers correctly.
- Display thinking expression while AI processes.
- Celebrate achievements with animated expression.

**Do Not**
- Use overly exaggerated or distracting animations.
- Change expression too frequently (annoying).
- Display inappropriate expressions for the context.

---

## 9. Learning-Specific

---

### 9.1 Knowledge Tree Node

**Purpose**  
The Knowledge Tree Node represents a topic, concept, or skill within a hierarchical knowledge tree that maps the user's learning structure.

**Usage Rules**
- Use in knowledge tree/map views showing subject hierarchy.
- Connected to parent and child nodes via branches.
- Status indicates mastery level: Not started, Learning, Practicing, Mastered.
- Click to navigate to related content.
- Size may indicate relative importance or breadth.

**Visual Rules**
- Shape: Circle or rounded rectangle, 60px-100px diameter.
- Background: Color by status:
  - Not started: Gray (#E8EAED).
  - Learning: Blue (#E8F0FE).
  - Practicing: Yellow (#FEF7E0).
  - Mastered: Green (#E6F4EA).
- Border: 2px solid status color.
- Icon/emoji: 24px center, representing the topic.
- Label: 12px SemiBold, centered, below icon.
- Progress ring: Outer ring showing completion percentage.
- Connector ports: Dots where branches connect.

**Spacing**
- Node size: 60px-100px.
- Padding: 8px.
- Icon size: 24px.
- Label margin-top: 4px.
- Between nodes: Determined by tree layout.

**States**

| State | Visual Change |
|-------|--------------|
| Not started | Gray, no progress ring |
| Learning | Blue, partial progress ring |
| Practicing | Yellow, progress ring ~50%+ |
| Mastered | Green, full ring, checkmark |
| Locked | Gray, 50% opacity, lock overlay |
| Active (selected) | Glow effect, scale 1.1 |
| Hover | Scale 1.05, tooltip |
| Prerequisite missing | Red highlight, tooltip |

**Responsive Behavior**
- Desktop: Full tree layout with zoom/pan.
- Tablet: Touch to select, pinch zoom.
- Mobile: Tap to focus, simplified view.

**Accessibility**
- Role: `button` or `treeitem`.
- `aria-label="[Topic] - [status] - [progress]%"`.
- `aria-expanded` for parent nodes.
- Keyboard: Arrow keys to navigate tree.

**Animation**
- Status change: Background color transition 400ms, progress ring fills.
- Unlock: Fade from gray to color 500ms.
- Selection: Glow + scale 300ms.
- Tree layout: Nodes animate to new positions 500ms.

**Interaction Rules**
- Click/navigate to view topic content.
- Hover shows tooltip with topic summary and stats.
- Right-click for context menu (study, review, mark as known).
- Drag to rearrange (edit mode).
- Zoom and pan for large trees.

**Do**
- Map a subject's topics hierarchically.
- Show prerequisite relationships.
- Track mastery progress per topic.

**Do Not**
- Show too many nodes at once (use zoom/collapse).
- Make nodes too small to read or tap (minimum 44px).
- Use complex trees without search/filter.

---

### 9.2 Knowledge Tree Branch

**Purpose**  
The Knowledge Tree Branch visually connects nodes in a knowledge tree, showing relationships, prerequisites, and dependencies.

**Usage Rules**
- Use to connect parent-child nodes in a knowledge tree.
- Style indicates relationship type:
  - Solid: Prerequisite required.
  - Dashed: Recommended but not required.
  - Dotted: Related/optional.
- Color by status: Gray (incomplete), Blue (in progress), Green (complete).
- May include labels for relationship type.

**Visual Rules**
- Line: 2px thick, curved (bezier) or straight.
- Color: Gray (#DADCE0) default.
- Direction: From prerequisite to dependent.
- Solid: Required prerequisite.
- Dashed: Recommended path.
- Dotted: Optional connection.
- Arrowhead (optional): Indicates direction.
- Animation: Flow effect for active learning paths.
- Hover: Line highlights, shows relationship label.

**Spacing**
- Line thickness: 2px (default), 3px (emphasized).
- Arrowhead: 8px.
- Label: 10px, gray, centered on line.

**States**

| State | Visual Change |
|-------|--------------|
| Incomplete | Gray line |
| In progress | Blue line, partial fill animation |
| Complete | Green line |
| Recommended | Dashed blue line |
| Highlighted | Line glows, thickens 1px |
| Hover | Tooltip with relationship info |

**Responsive Behavior**
- Renders within the knowledge tree canvas.
- Scales with zoom level.

**Accessibility**
- `aria-label="[Source] required for [target]"`.
- Role: `img` (decorative) or `group`.

**Animation**
- Completion: Line fills from source to target 500ms.
- Active path: Particles flow along line 2s.
- Hover: Line thickness transition 200ms.

**Interaction Rules**
- Hover to see relationship type.
- Click to highlight the connection path.
- No direct manipulation (node-level interaction).

**Do**
- Show "Algebra required for Calculus".
- Indicate "Recommended: Read Chapter 2 before Chapter 5".
- Visualize a complete prerequisite chain.

**Do Not**
- Overcrowd with too many crossing branches.
- Use too many different line styles without legend.
- Make branches the primary interaction target (nodes are primary).

---

### 9.3 Streak Indicator

**Purpose**  
The Streak Indicator shows the user's consecutive days of learning activity, motivating consistency and habit formation.

**Usage Rules**
- Use in dashboard, profile header, and progress sections.
- Shows current streak count with fire or flame icon.
- Indicates streak status: Active, At risk, Broken.
- Animate on streak milestones (7, 30, 100 days).
- Show daily progress dots for the current week.

**Visual Rules**
- Container: Compact badge or card.
- Icon: Flame/fire, 24px-32px, orange/red gradient.
- Count: 24px-40px Bold, dark.
- Label: "Day streak", 12px Regular, gray.
- Weekly dots: 7 small circles (8px), filled = active day.
- Current day: Pulsing dot.
- Active streak: Orange-red colors.
- At risk (yesterday missed): Yellow-orange, warning pulse.
- Broken: Gray flame, "Start new streak" message.
- Milestone: Special badge (e.g., "7-Day Streak!") at celebrations.

**Spacing**
- Badge padding: 8px 12px.
- Icon to count: 4px.
- Count to label: 2px.
- Weekly dots: 8px circles, gap 4px.
- Between weekly dots and count: 8px.

**States**

| State | Visual Change |
|-------|--------------|
| Active (ongoing) | Orange flame, count visible, today's dot filled |
| At risk (no activity yesterday) | Yellow flame, pulsing alert |
| Broken (missed 2+ days) | Gray flame, "0" or last streak count |
| Milestone reached | Celebration animation (7, 30, 100, 365) |
| Loading | Skeleton for icon and count |
| New (first day) | Small flame, count "1", dot fills |

**Responsive Behavior**
- Desktop: Card or badge in profile section.
- Tablet: Same.
- Mobile: Compact badge in header.

**Accessibility**
- Role: `status`.
- `aria-label="7-day learning streak"`.
- `aria-live="polite"` for streak updates.
- Milestone: Announce "Congratulations! 30-day streak achieved!"

**Animation**
- Streak increment: Flame flickers, count bounces 300ms.
- Daily dot fill: Dot fills with scale 200ms.
- At risk: Pulsing red/orange 1s cycle.
- Milestone: Fire grows, sparkle particles, confetti 1.5s.
- Broken: Flame extinguishes (scale down + fade) 400ms.

**Interaction Rules**
- Click may show detailed streak calendar.
- Streak freeze item (if available) shown next to streak.
- Milestone celebrations auto-play once.
- Streak data updates daily based on activity.

**Do**
- Show "15-Day Streak" with fire icon.
- Display weekly activity dots.
- Celebrate "100-Day Streak" achievement.

**Do Not**
- Reset streak if user misses one day (allow freeze/grace).
- Make the streak indicator too large in the UI.
- Pressure users negatively — keep it motivational.

---

### 9.4 XP Indicator

**Purpose**  
The XP Indicator shows the user's experience points earned through learning activities, tracking progress toward the next level.

**Usage Rules**
- Use in profile, lesson cards, and activity feeds.
- Shows current XP, XP needed for next level, and level.
- XP earned from: completing lessons, quizzes, streaks, achievements.
- Animate XP gain with count-up and visual feedback.
- Bar shows progress to next level.

**Visual Rules**
- Container: Compact card or badge.
- Icon: Star, lightning, or crystal, 20px.
- Current XP: 16px Bold.
- Level badge: "Level 12", pill shape, colored bg.
- Progress bar: Track to next level, 6px height, width 120px-200px.
- Progress fill: Blue (#1A73E8) or gradient.
- XP remaining: "250 XP to next level", 11px gray.
- Gain animation: "+50 XP" floating text on earn.

**Spacing**
- Container padding: 12px 16px.
- Level badge: height 24px, padding 0 8px.
- XP number to level badge: 8px.
- Progress bar: margin 8px 0.
- XP remaining text: margin-top 4px.

**States**

| State | Visual Change |
|-------|--------------|
| Default | Current XP, level, progress |
| Level up | Celebration overlay, new level badge |
| XP gained | Count-up animation, floating +XP text |
| Max level | Special badge, progress bar full gold |
| Loading | Skeleton for XP and level |

**Responsive Behavior**
- Desktop: Full card in profile, compact in header.
- Tablet: Same.
- Mobile: Compact in header, expanded in profile.

**Accessibility**
- Role: `status`.
- `aria-label="Level 12, 1450 experience points, 250 to next level"`.
- `aria-valuenow`, `aria-valuemin`, `aria-valuemax` on progress bar.
- Announce level up: "Congratulations! You reached Level 13!"

**Animation**
- XP gain: Count-up from previous to new XP over 1s.
- Progress bar: Fill animates 800ms.
- Level up: Badge explodes/shines, new level appears 1s.
- Floating XP: "+50 XP" floats up and fades out 1.5s.
- Milestone: Special effect at every 10 levels.

**Interaction Rules**
- Click may show XP breakdown by activity.
- XP updates immediately after activity completion.
- Double XP events shown with special indicator.

**Do**
- Show "+25 XP" after completing a lesson.
- Display "Level 8 - 450/500 XP" in profile.
- Level up celebration at thresholds.

**Do Not**
- Hide the XP breakdown — users want to know how to earn.
- Make XP too hard to earn (demotivating) or too easy (meaningless).
- Use XP as the only progress metric — combine with mastery.

---

### 9.5 Level Indicator

**Purpose**  
The Level Indicator displays the user's current level, representing overall progress and achievement in the learning platform.

**Usage Rules**
- Use in profile header, avatar frame, and leaderboards.
- Badge with level number, icon, and optional rank title.
- Level increases based on XP accumulation.
- Each level may have a themed title (e.g., "Scholar", "Sage", "Master").
- Frame/background changes at milestone levels.

**Visual Rules**
- Badge: Circular or hexagonal shape, 48px-72px.
- Background: Color gradient based on level range:
  - Level 1-10: Bronze (#CD7F32).
  - Level 11-25: Silver (#C0C0C0).
  - Level 26-50: Gold (#FFD700).
  - Level 51-75: Platinum (#E5E4E2).
  - Level 76-100: Diamond (#B9F2FF).
- Number: 20px-32px Bold, white.
- Icon: Small icon representing level tier.
- Title: 11px Regular, below level (e.g., "Apprentice").
- Frame glow: For high levels, subtle animated glow.
- Progress arc: Outer ring showing progress to next level.

**Spacing**
- Badge size: 48px-72px.
- Number font: 20px-32px.
- Title margin-top: 2px.
- Progress arc: 3px thick.

**States**

| State | Visual Change |
|-------|--------------|
| Default | Current level badge |
| Level up | Explosion animation, number flips |
| Max level | Special border, animated glow |
| Newly earned | Brief celebration |
| Static display | Normal badge display |

**Responsive Behavior**
- Desktop: Full size in profile.
- Tablet: Same.
- Mobile: Smaller (40px) in headers.

**Accessibility**
- `aria-label="Level 25 - Gold Tier - Scholar"`.
- Level up: `aria-live="assertive"`.

**Animation**
- Level up: Number flips/scrolls 500ms, badge glows 1s.
- Progress arc: Fills smoothly 500ms.
- Tier change: Badge color transitions 1s.
- Frame glow: Subtle pulse for high levels 3s.

**Interaction Rules**
- Click may show level benefits/rewards.
- Hover shows progress to next level.
- Level up automatically triggers notification.

**Do**
- Show "Level 7 - Apprentice" in profile.
- Display level on leaderboard rankings.
- Indicate level-up rewards available.

**Do Not**
- Make levels too frequent (inflation) or too rare.
- Change level requirements without notice.
- Show level in every context (can feel spammy).

---

### 9.6 Progress Ring

**Purpose**  
The Progress Ring is a circular indicator showing completion progress for a course, lesson, skill, or goal.

**Usage Rules**
- Use for course completion, skill mastery, daily goal progress.
- Circular arc fills proportionally to completion percentage.
- Center shows percentage text or icon.
- Color changes by progress or status.

**Visual Rules**
- Circle: Track ring (gray) + fill ring (colored arc).
- Stroke width: 6px-10px depending on size.
- Sizes: 40px (small), 60px (medium), 100px (large).
- Track color: #E8EAED.
- Fill color: Blue (#1A73E8) default.
- Color thresholds:
  - 0-25%: Red or orange.
  - 26-50%: Yellow.
  - 51-75%: Blue.
  - 76-99%: Blue/cyan.
  - 100%: Green (#34A853).
- Center content: Percentage text or completion icon.
- Animation: Arc fills clockwise.

**Spacing**
- Ring diameter: 40px/60px/100px.
- Stroke width: 6px/8px/10px.
- Center text: Sized proportionally.

**States**

| State | Visual Change |
|-------|--------------|
| Empty (0%) | No fill, gray track |
| In progress | Partial fill, colored |
| Complete (100%) | Full green ring, checkmark |
| Over goal (>100%) | Full ring with +1 animation |
| Error | Red ring, X center |
| Indeterminate | Rotating arc (loading) |

**Responsive Behavior**
- Desktop: At defined size.
- Tablet: Same.
- Mobile: Slightly smaller.

**Accessibility**
- Role: `progressbar`.
- `aria-valuenow`, `aria-valuemin`, `aria-valuemax`.
- `aria-label="[Goal]: [percentage]% complete"`.

**Animation**
- Fill: Arc animates from current to target 800ms ease-out.
- Color transition: 300ms.
- Completion: Ring turns green, checkmark scales in 300ms.
- Indeterminate: Continuous rotation 1s.

**Interaction Rules**
- Read-only indicator.
- Click may expand details.

**Do**
- Show course completion at 75%.
- Display daily study goal progress 3/5 topics.
- Indicate skill mastery progress.

**Do Not**
- Use for precise measurements (use linear progress bar).
- Make ring too small to read percentage.
- Animate on every minor update (batch updates).

---

### 9.7 Knowledge Graph Node

**Purpose**  
The Knowledge Graph Node represents a concept or topic within a knowledge graph, showing connections and relationships to other concepts.

**Usage Rules**
- Use in knowledge graph visualizations, mind maps, and concept maps.
- Connected to related concepts via edges.
- Size indicates relevance or connectedness.
- Color indicates category or domain.
- Click to explore or navigate to content.

**Visual Rules**
- Shape: Circle (default) or rounded square, border-radius 8px.
- Size: 48px-120px based on connections (degree).
- Background: Color by category (Science: blue, Math: red, Language: green).
- Border: 2px solid category color.
- Label: 12px-14px SemiBold, centered, dark.
- Icon: 20px-32px optional.
- Shadow: Elevation 2.
- Active/highlighted: Glow effect, 1.1x scale.
- Outer ring: Thin progress ring showing mastery.

**Spacing**
- Node diameter: 48px-120px.
- Inner padding: 8px.
- Label to edge: 4px.

**States**

| State | Visual Change |
|-------|--------------|
| Default | Normal size, category color |
| Hover | Scale 1.05, tooltip |
| Selected | Glow, scale 1.1, bold border |
| Expanded | Shows connected nodes |
| Mastered | Green border, checkmark |
| Learning | Blue border, partial progress ring |
| Locked | Gray, 50% opacity |

**Responsive Behavior**
- Desktop: Interactive canvas.
- Tablet: Touch to select/drag.
- Mobile: Tap to focus, zoom controls.

**Accessibility**
- Role: `button`.
- `aria-label="[Concept name] - [category] - [mastery level]"`.
- Keyboard: Navigate with arrow keys.

**Animation**
- Hover: Scale 1.05 200ms.
- Selection: Glow + scale 300ms.
- Expand: Connected nodes animate outward 400ms.
- Mastery update: Progress ring fills 500ms.

**Interaction Rules**
- Click to select or navigate.
- Drag to reposition.
- Double-click to open detailed view.
- Connected edges highlight on hover.

**Do**
- Show "Photosynthesis" connected to "Chlorophyll", "Sunlight", "Glucose".
- Display related topics in a concept map.
- Visualize a subject's interconnected concepts.

**Do Not**
- Put too much text inside the node.
- Overcrowd the graph — use zoom levels.
- Use too many categories (max 6-8 distinct colors).

---

### 9.8 Knowledge Graph Edge

**Purpose**  
The Knowledge Graph Edge represents a relationship between two concepts in the knowledge graph, indicating how topics are connected.

**Usage Rules**
- Use in knowledge graphs to show relationships between nodes.
- Relationship types: prerequisite, related, extends, example, contrast.
- Style varies by relationship type.
- Label option for relationship description.
- Weight/thickness indicates strength.

**Visual Rules**
- Line: 2px thick, curved (bezier).
- Color by relationship:
  - Prerequisite: Blue (#1A73E8).
  - Related: Gray (#5F6368).
  - Extends: Green (#34A853).
  - Example: Orange (#FBBC04).
  - Contrast: Red (#EA4335).
- Solid: Strong relationship.
- Dashed: Weak or inferred.
- Arrow: Directed edges have arrowheads.
- Label: 10px Regular, gray, on hover or always visible.
- Hover: Line thickens, label appears.
- Active: Glow effect.

**Spacing**
- Line thickness: 2px (default), 3px (strong).
- Arrowhead: 6px.
- Label: Offset from line by 4px.

**States**

| State | Visual Change |
|-------|--------------|
| Default | Colored line, subtle |
| Hover | Thickens 1px, label appears |
| Selected | Glow, bold |
| Path highlighted | All edges in path glow |
| Animated | Flow effect along edge |

**Responsive Behavior**
- Renders on graph canvas.
- Scales with zoom.

**Accessibility**
- `aria-label="[Source] is prerequisite of [target]"`.
- Role: `img` (decorative).

**Animation**
- Hover: Line thickness transition 200ms.
- Flow: Particles move along edge 2s.
- Render: Edge draws from source to target 400ms.

**Interaction Rules**
- Hover to see relationship type and label.
- Click edge to highlight full connection path.
- Nodes at both ends highlight on edge hover.

**Do**
- "Prerequisite: Algebra -> Calculus".
- "Related: Photosynthesis <-> Cellular Respiration".
- "Example: Newton's Laws -> Rocket Launch".

**Do Not**
- Show all labels in dense graphs.
- Use too many different edge styles.
- Make edges the primary interaction target.

---

### 9.9 Learning Path Node

**Purpose**  
The Learning Path Node represents a milestone or checkpoint in a structured learning path or curriculum sequence.

**Usage Rules**
- Use in learning path views, curriculum maps, and course sequences.
- Connected in a linear or branching sequence.
- Status: Locked, Available, In Progress, Completed.
- Shows estimated duration and completion status.
- Click to navigate to the associated lesson or activity.

**Visual Rules**
- Shape: Circular or rounded rectangle with number.
- Size: 80px x 80px (or 100px x 70px rectangle).
- Background: Color by status.
- Number: 20px Bold, centered.
- Icon: Optional, replaces number for completed nodes.
- Label: 12px SemiBold, below node.
- Duration: "15 min", 10px gray below label.
- Connector line: 3px thick, solid or dashed.
- Progress: Inner ring or checkmark for completed.

**Spacing**
- Node size: 80px x 80px (circle) or 100px x 70px (rect).
- Border-radius: 50% (circle) or 12px (rect).
- Label margin-top: 8px.
- Duration margin-top: 2px.
- Between nodes: 40px (horizontal) or 24px (vertical).

**States**

| State | Visual Change |
|-------|--------------|
| Locked | Gray, lock icon, 50% opacity |
| Available | White bg, blue border, blue number |
| In Progress | Blue bg, white number, progress ring |
| Completed | Green bg, white checkmark |
| Current | Pulsing blue glow, scale 1.05 |
| Hover (available+) | Scale 1.08, shadow lift |
| Focused | Focus ring |

**Responsive Behavior**
- Desktop: Horizontal or vertical path layout.
- Tablet: Scaled, may wrap.
- Mobile: Vertical scrollable path.

**Accessibility**
- Role: `button` or `link`.
- `aria-label="Node [number]: [topic] - [status]"`.
- `aria-current="step"` for current node.

**Animation**
- Completion: Circle fills green 400ms, checkmark scales in.
- Unlock: Fades in, border appears 300ms.
- Active: Pulse glow 2s.
- Entrance: Staggered slide-in along path 200ms each.

**Interaction Rules**
- Click available/in-progress/completed nodes to navigate.
- Locked nodes show unlock requirement on click.
- Hover shows tooltip with node description.
- Path scrolls to keep current node visible.

**Do**
- Learning path: "Lesson 1 -> Lesson 2 -> Lesson 3".
- Curriculum: "Introduction -> Basics -> Advanced -> Expert".
- Milestone path with achievements between nodes.

**Do Not**
- Make the path too long without grouping (limit to 10-12 visible nodes).
- Show complex branching without clear guidance.
- Use inconsistent node sizes in the same path.

---

### 9.10 Memory Card (SRS)

**Purpose**  
The SRS Memory Card is a flashcard optimized for spaced repetition learning, with scheduling based on user recall performance.

**Usage Rules**
- Use in review sessions for active recall practice.
- Front: prompt, question, or term.
- Back: answer, definition, or explanation.
- After flipping, user rates recall difficulty.
- Difficulty rating determines next review interval.
- Cards are scheduled algorithmically (SM-2 or similar).

**Visual Rules**
- Card: 3:2 aspect ratio, max-width 600px.
- Front: White bg, centered content, 20px Bold.
- Back: Light cream bg (#FEFEFE), 18px Regular.
- Rating buttons: 4 buttons after flip.
  - Again (1): Red, reset interval.
  - Hard (2): Orange, 1.2x interval.
  - Good (3): Green, 2.5x interval.
  - Easy (4): Blue, 5x interval.
- Progress: "5 of 20" top-right.
- Deck name: 12px gray, top-left.
- Hint: 14px gray italic, optional.

**Spacing**
- Card padding: 32px.
- Rating buttons: 48px height, 8px gap.
- Button text: 14px Bold, white.
- Card number: absolute top-right 16px.
- Hint: bottom of front, 16px from edge.

**States**

| State | Visual Change |
|-------|--------------|
| Front | Question visible |
| Back | Answer visible, different bg |
| Flipping | 3D Y-axis rotation 600ms |
| Rating hover | Button darkens |
| Card exit after rating | Slide left/right 200ms |
| Card enter | Slide in 300ms |

**Responsive Behavior**
- Desktop: Centered, max-width 600px.
- Tablet: Centered, max-width 500px.
- Mobile: Full-width with 16px padding.

**Accessibility**
- Flip: `aria-label="Tap to reveal answer"`.
- Rating buttons: `aria-label="Rate Again"`, etc.
- Keyboard: Space to flip, 1-4 to rate.
- `aria-live="polite"` for answer reveal.

**Animation**
- Flip: 3D Y-axis rotation 600ms, perspective 1000px.
- Card entrance: Slide from right 300ms.
- Card exit on rating: Slide left 200ms (Easy/Good) or right (Again/Hard).
- Rating press: Scale 0.95 80ms.

**Interaction Rules**
- Must flip to see answer before rating.
- After rating, next card appears.
- Session complete after all cards reviewed.
- Cards due for review determined by SRS algorithm.
- Undo last rating (5s window).

**Do**
- Review vocabulary with spaced repetition.
- Practice historical dates and events.
- Reinforce formulas and equations.

**Do Not**
- Allow editing during review.
- Skip cards without rating (exception: bury).
- Show answer without user action.

---

### 9.11 Flashcard (Front/Back)

**Purpose**  
The Flashcard presents a prompt (front) and its corresponding answer (back) for study and memorization.

**Usage Rules**
- Use in study sessions for active recall.
- One concept per card.
- Front: concise prompt.
- Back: complete but concise answer.
- Supports text, images, code, and audio.
- User flips card to reveal back.

**Visual Rules**
- Front: White background, 20px Bold centered text.
- Back: Off-white (#FEFEFE) background, 18px Regular.
- Card: 3:2 aspect ratio, border-radius 12px, shadow elevation 2.
- Deck badge: Top bar, 12px gray.
- Card number: "12 of 30" bottom-right.
- Tags: Bottom of back, 11px chips.
- Hint: 14px italic gray at bottom of front (optional).
- Image: Max 80% width, centered.
- Code: Monospace, gray background block.

**Spacing**
- Inner padding: 24px (front), 24px (back).
- Content centered vertically.
- Card number margin-top: auto.
- Tags margin-top: 12px.

**States**

| State | Visual Change |
|-------|--------------|
| Front only | Question visible |
| Back revealed | Answer visible, different bg |
| Flipping | 3D rotation |
| Hint visible | Hint text appears at bottom |
| Bookmarked | Bookmark icon filled |

**Responsive Behavior**
- Desktop: Fixed aspect ratio, centered.
- Tablet: Same, slightly smaller.
- Mobile: Full-width, 16px padding, aspect ratio maintained.

**Accessibility**
- `aria-label="Flashcard [number] of [total]"`.
- Flip: `aria-label="Flip card"`, role="button".
- Back content: `aria-live="polite"`.
- Keyboard: Space to flip.

**Animation**
- Flip: 3D Y-axis rotation with perspective, 600ms.
- Hint: Slide up from bottom 200ms.
- Bookmark: Icon morph 250ms.

**Interaction Rules**
- Tap anywhere on card to flip (or dedicated flip button).
- Swipe to navigate between cards.
- Card orientation persists until user flips back or rates.
- In review mode, rating buttons appear after flip.

**Do**
- Vocabulary: "Word" front -> "Definition" back.
- Q&A: "Question" front -> "Answer" back.
- Concept: "Term" front -> "Explanation" back.

**Do Not**
- Put multiple concepts on one card.
- Make front too long (max 2 lines).
- Show back content before user flips.

---

### 9.12 Quiz Question Card

**Purpose**  
The Quiz Question Card presents a single quiz question with all its components: question text, options, media, and progress.

**Usage Rules**
- Use in quiz and assessment interfaces.
- One card per question.
- Supports multiple choice, true/false, multi-select, fill-in-blank.
- Shows question number and total progress.
- Media (images, code) can supplement the question.

**Visual Rules**
- Card: Full-width, white bg, border-radius 12px.
- Progress: Top bar "Question 5 of 15" with progress bar.
- Question: 18px SemiBold, 1.5 line-height.
- Media: Image or diagram below question, max 400px height.
- Options: 4-6 option buttons (see Quiz Option Button 1.12).
- Multi-select: Checkbox style options.
- Fill-in-blank: Text input inline with question.
- Timer (optional): Top-right, 14px bold, red when <30s.

**Spacing**
- Card padding: 20px.
- Progress bar: 4px height, margin-bottom 16px.
- Question margin-bottom: 16px.
- Media margin: 12px 0.
- Options gap: 12px.

**States**

| State | Visual Change |
|-------|--------------|
| Unanswered | Options available, waiting for input |
| Selected (not submitted) | Option highlighted blue |
| Answered (correct) | Green feedback, correct option highlighted |
| Answered (incorrect) | Red feedback, correct shown green |
| Timed out | "Time's up" banner |
| Review mode | Correct/incorrect shown, explanation expanded |

**Responsive Behavior**
- Desktop: Max-width 720px, centered.
- Tablet: Same.
- Mobile: Full-width.

**Accessibility**
- Role: `region`, `aria-label="Question [number] of [total]"`.
- `aria-live="polite"` for feedback.
- Options: `role="radio"` or `role="checkbox"`.
- Keyboard: Tab through options, Space/Enter to select.

**Animation**
- Option select: Radio/checkbox fill 200ms.
- Feedback reveal: Slide down 300ms.
- Timer warning: Red pulse when <30s.
- Next question: Slide left 300ms.

**Interaction Rules**
- Select option before submitting.
- Submit button to confirm answer.
- Feedback shown immediately after submit.
- Cannot change answer after submission.
- Timer auto-submits when expired.

**Do**
- Single-select: "What is the capital of France?" [Paris] [London] [Berlin] [Madrid].
- Multi-select: "Select all prime numbers" [2] [4] [7] [10].
- True/False: "The Earth is flat." [True] [False].

**Do Not**
- Allow multiple submissions.
- Show correct answer before user answers.
- Make questions ambiguous or poorly worded.

---

### 9.13 Quiz Answer Card

**Purpose**  
The Quiz Answer Card displays the user's answer and the correct answer after a quiz question is submitted, providing feedback and explanation.

**Usage Rules**
- Use after the user submits an answer in a quiz.
- Replaces or appears below the Quiz Question Card.
- Shows: user's answer, correct answer, explanation.
- Score impact: "+10 XP" or "+0 XP".
- "Next" button to proceed.

**Visual Rules**
- Card: Same style as Quiz Question Card.
- Result icon: Large green checkmark (correct) or red X (incorrect).
- Your answer: Label "Your answer" with user's selection.
- Correct answer: Label "Correct answer" with green highlight.
- Explanation: 14px Regular, gray, with source link.
- XP earned: "+10 XP" green badge (correct) or "+0 XP" gray (incorrect).
- "Next" button: Primary style.
- "Review" button: Secondary, opens detailed explanation.

**Spacing**
- Card padding: 20px.
- Result icon: 48px, centered.
- Your answer section: margin-top 16px.
- Correct answer section: margin-top 12px.
- Explanation: margin-top 16px.
- XP badge: margin-top 12px.
- Next button: margin-top 20px.

**States**

| State | Visual Change |
|-------|--------------|
| Correct | Green checkmark, green "Your answer" |
| Incorrect | Red X, red "Your answer", green "Correct answer" |
| Partially correct | Yellow warning, shows partial score |
| Explanation expanded | Full explanation visible |
| Reviewing | Additional resources shown |

**Responsive Behavior**
- Desktop: Max-width 720px.
- Tablet: Same.
- Mobile: Full-width.

**Accessibility**
- Role: `region`.
- `aria-live="assertive"` for result announcement.
- "Correct!" or "Incorrect" announced.
- `aria-label` for result summary.

**Animation**
- Result reveal: Card fades in 300ms, icon scales in 400ms.
- Correct: Green glow 500ms.
- Incorrect: Brief shake 300ms.
- Explanation: Slide down 300ms.
- XP gain: Count-up 500ms.

**Interaction Rules**
- "Next" continues to next question.
- "Review" opens detailed explanation panel.
- Score updates immediately.
- Cannot go back to change answer.

**Do**
- Show "Correct! +10 XP" with checkmark.
- Display "Incorrect. The correct answer was Paris." with explanation.
- Provide "Learn more" source link in explanation.

**Do Not**
- Skip explanation — it's critical for learning.
- Delay feedback unnecessarily.
- Make the user wait before proceeding to next question.

---

## 10. Avatar / Character

---

### 10.1 Avatar Display

**Purpose**  
The Avatar Display component shows a user-uploaded or system-generated avatar representing the user or a character.

**Usage Rules**
- Use in profile headers, comments, navigation, and user lists.
- Supports image upload, initials fallback, and default icon.
- Sizes: 24px (tiny), 32px (small), 40px (medium), 56px (large), 80px (xlarge), 120px (hero).
- Badge for online/offline status.
- Clickable to view profile or open menu.

**Visual Rules**
- Shape: Circle (default) or rounded square (8px radius).
- Image: Object-fit cover, centered, 100% width/height.
- Initials: 2 characters, centered, Bold, white.
- Background (initials): Color hash of user ID.
- Border: 2px white (for overlapping contexts).
- Status badge: 12px circle, bottom-right.
  - Online: Green (#34A853).
  - Away: Yellow (#FBBC04).
  - Busy: Red (#EA4335).
  - Offline: Gray (#DADCE0).
- Ring/Frame: Level indicator ring (optional).

**Spacing**
- Size: 24/32/40/56/80/120px.
- Status badge: 12px offset 2px.
- Border: 2px.

**States**

| State | Visual Change |
|-------|--------------|
| Default | Image visible |
| Hover | Subtle scale 1.05, shadow |
| Pressed | Scale 0.98 |
| Image load error | Falls back to initials |
| Focused | Focus ring |
| Disabled | 60% opacity |

**Responsive Behavior**
- Desktop: Specified size.
- Tablet: Same.
- Mobile: Smaller in lists (32px).

**Accessibility**
- `img` role, `alt` text (person's name).
- `aria-label="[Name]'s avatar"`.
- Status: `aria-label="[Status]"`.

**Animation**
- Hover: Scale 1.05 150ms.
- Status change: Badge crossfade 200ms.

**Interaction Rules**
- Click navigates to profile or opens popover.
- Hover shows tooltip with name.
- Grouped avatars show overlap preview.

**Do**
- User avatar on a comment or post.
- Profile picture in settings.
- Character avatar in learning companion.

**Do Not**
- Stretch or distort images.
- Show broken image icon on error — use initials.
- Make avatars too small to recognize (min 32px for recognition).

---

### 10.2 Avatar Customization Component

**Purpose**  
The Avatar Customization Component allows users to personalize their avatar by choosing features, colors, and accessories.

**Usage Rules**
- Use in profile setup, settings, and character creation.
- Offers customization options: hairstyle, eyes, mouth, skin tone, outfit, accessories.
- Real-time preview of changes.
- Preset characters for quick selection.
- Save and apply changes.

**Visual Rules**
- Layout: Preview panel (left) + options panel (right).
- Preview: Large avatar display (120px-200px), real-time updates.
- Options: Categorized sections (Hair, Eyes, Outfit, etc.).
- Each option: Selectable thumbnail (48px x 48px), border on selection.
- Colors: Color picker swatches for skin, hair, outfit.
- Presets: "Quick select" row of pre-made avatars.
- Save button: Primary style.
- Randomize button: Shuffle icon.

**Spacing**
- Preview size: 120px-200px.
- Option thumbnail: 48px, gap 8px.
- Section margin: 16px.
- Section header: 14px SemiBold.
- Save button: margin-top 24px.

**States**

| State | Visual Change |
|-------|--------------|
| Default | Current avatar displayed |
| Option selected | Blue border on thumbnail |
| Hover (option) | Scale 1.05, shadow |
| Preview updating | Avatar refreshes instantly |
| Saving | Spinner on save button |
| Saved | "Saved" confirmation |
| Randomizing | Options cycle rapidly 500ms |

**Responsive Behavior**
- Desktop: Side-by-side preview and options.
- Tablet: Stacked (preview top, options bottom).
- Mobile: Full-screen modal, preview top, scrollable options.

**Accessibility**
- Role: `region`, `aria-label="Avatar customization"`.
- Options: `role="radiogroup"` or `role="listbox"`.
- `aria-label` for each option (e.g., "Brown hair").
- Keyboard: Arrow keys to navigate options, Enter to select.
- Preview: `aria-live="polite"` for changes.

**Animation**
- Preview update: Face crossfades 200ms between changes.
- Option selection: Border transition 150ms.
- Randomize: Options shuffle visually 500ms, then settle.
- Save: Button loading -> checkmark 300ms.

**Interaction Rules**
- Select option to apply immediately to preview.
- Undo/Reset button reverts to saved state.
- Randomize creates a random combination.
- Preset selects all options at once.
- Save persists customization.
- Changes not applied until saved.

**Do**
- Customize avatar hairstyle, color, and outfit.
- Select preset character theme.
- Randomize for inspiration.

**Do Not**
- Limit creativity with too few options.
- Make the UI too complex — categorize clearly.
- Auto-save without user confirmation.

---

### 10.3 Character Portrait

**Purpose**  
The Character Portrait is a large, detailed illustration of a learning companion or character, used for promotional, profile, or narrative contexts.

**Usage Rules**
- Use in character selection, profile pages, and narrative/story screens.
- Full-body or bust illustration.
- Different poses or outfits based on context.
- Expression reflects current mood or scenario.
- Interactive: tap/click for reaction.

**Visual Rules**
- Size: 200px-400px height (bust), full viewport height (full body).
- Art style: Consistent with the character's design system.
- Background: Transparent or themed backdrop.
- Expression: Changes dynamically (see Character Expression).
- Accessories: Visible and themed.
- Animation: Subtle idle (breathing, slight movement).
- Frame: Decorative frame or shadow.

**Spacing**
- Portrait width: 200px-400px.
- Margin from surrounding content: 16px-24px.
- No padding (art extends to edges of container).

**States**

| State | Visual Change |
|-------|--------------|
| Neutral | Relaxed pose, slight smile |
| Happy | Open posture, big smile |
| Thinking | Hand on chin, looking up |
| Teaching | Pointing, explanatory gesture |
| Celebrating | Arms up, confetti |
| Sad/apologetic | Drooped posture |
| Greeting | Waving hand |

**Responsive Behavior**
- Desktop: Full size.
- Tablet: Scaled to 70%.
- Mobile: Bust only, 180px height.

**Accessibility**
- Role: `img`.
- `aria-label="[Character name] - [expression]"`.
- `aria-live="polite"` for expression changes.

**Animation**
- Idle: Breathing (scale 1.0 -> 1.01), 4s cycle.
- Expression change: Crossfade 400ms, features adjust.
- Entrance: Fade in + slide up 500ms.
- Gesture: Arm/hand movement 600ms.

**Interaction Rules**
- Tap for reaction animation.
- Drag to rotate (3D portraits, optional).
- Expression driven by context or user interaction.
- Used in narrative scenes with text overlay.

**Do**
- Show character greeting user on dashboard.
- Display character teaching a lesson concept.
- Celebrate achievements with character portrait.

**Do Not**
- Use portrait where it distracts from primary content.
- Make animations too slow or too fast.
- Change expression without clear trigger.

---

### 10.4 Character Expression

**Purpose**  
The Character Expression component manages and displays facial expressions for the learning companion, conveying emotion and personality.

**Usage Rules**
- Used in conjunction with Avatar Display, Character Portrait, and AI Character Avatar.
- Expression set includes: Neutral, Happy, Sad, Thinking, Surprised, Encouraging, Celebrating, Listening.
- Transitions smoothly between expressions.
- Driven by context: quiz result, conversation tone, achievement.
- Reusable across different avatar/character components.

**Visual Rules**
- Face area: Defined region within avatar bounds.
- Eyes: Shape, direction, openness:
  - Neutral: Open, forward.
  - Happy: Closed upward (^ shape).
  - Sad: Closed downward.
  - Thinking: Looking up/away.
  - Surprised: Wide open.
  - Encouraging: Soft, warm.
  - Celebrating: Sparkling/star eyes.
- Mouth: Shape, openness:
  - Neutral: Small smile.
  - Happy: Wide smile.
  - Sad: Frown.
  - Thinking: Pursed or slight open.
  - Surprised: Open circle.
  - Encouraging: Gentle smile.
  - Celebrating: Big grin.
- Eyebrows: Position and angle.
- Cheeks: Blush for happy/embarrassed.
- Accessories: Glasses, hat position consistent.

**Spacing**
- Expression applied within avatar bounds.
- Feature coordinates are relative to face area.

**States**

| State | Visual Change |
|-------|--------------|
| Neutral | Default relaxed face |
| Happy | Bright eyes, big smile |
| Sad | Drooped eyes, frown |
| Thinking | Looking up, mouth pursed |
| Surprised | Wide eyes, open mouth |
| Encouraging | Soft eyes, warm smile |
| Celebrating | Sparkle eyes, big grin |
| Listening | Slight head tilt, attentive |
| Confused | One eyebrow up, mouth askew |

**Responsive Behavior**
- Scales with avatar size.
- Detail reduces on smaller sizes (minimal features).

**Accessibility**
- `aria-label="Expression: [name]"`.
- Expressions should complement but not replace text/voice tone.

**Animation**
- Transition: Morph between expressions over 300ms.
- Blink: Every 3-5s, 150ms.
- Idle: Micro-expressions (subtle changes).
- Exaggerated: Milestone expressions with particle effects.

**Interaction Rules**
- Driven by application state, not direct user control.
- Expressions should match the context appropriately.
- Hold expression for appropriate duration before returning to neutral.

**Do**
- Happy expression when user answers correctly.
- Encouraging when user struggles.
- Celebrating on achievement unlock.

**Do Not**
- Change expression too rapidly (distracting).
- Use inappropriate expressions for the context.
- Override user's emotional experience with mismatched expressions.

---

### 10.5 Knowledge Spirit Display

**Purpose**  
The Knowledge Spirit Display is a specialized visual representation of the user's knowledge spirit — a mystical or thematic entity that grows and evolves with the user's learning progress.

**Usage Rules**
- Use in the companion panel, profile, and learning journey visualizations.
- Represents the user's accumulated knowledge as a living entity.
- Evolves visually based on:
  - Level: Size, complexity, aura.
  - Mastery: Color vibrancy, accessories.
  - Streak: Activity particles, glow intensity.
  - Achievements: Special visual effects.
- Interactive: tap for reaction, hover for stats.

**Visual Rules**
- Core shape: Abstract, glowing orb/crystal or creature form.
- Size: 60px-150px based on level.
- Color: Theme color, vibrancy based on activity.
- Aura: Glowing halo, size based on streak.
- Particles: Floating motes around spirit, activity-based.
- Level details: Inner patterns or structures that grow.
- Evolution stages: Larva -> Pupal -> Adult -> Ascended.
- Animation: Floating idle, gentle pulse.

**Spacing**
- Container: 100px-200px centered.
- Spirit size: 60px-150px.
- Aura: 20px-40px beyond spirit edge.
- Particle area: 150% of spirit size.

**States**

| State | Visual Change |
|-------|--------------|
| Starting (Level 1-5) | Small, dim, simple shape |
| Growing (Level 6-15) | Medium, brighter, more detail |
| Developed (Level 16-30) | Large, vibrant, complex patterns |
| Mastered (Level 31+) | Max size, brilliant glow, full detail |
| Active (recent activity) | Particles float, gentle pulse |
| Idle (no activity today) | Dimmer, slow pulse, fewer particles |
| Streak active | Flame particles, warm glow |
| Achievement unlocked | Burst of particles, flash |
| New evolution | Dramatic transformation animation |

**Responsive Behavior**
- Desktop: Full spirit display in companion panel.
- Tablet: Scaled to 75%.
- Mobile: Simplified, smaller (80px max).

**Accessibility**
- Role: `img`.
- `aria-label="Knowledge Spirit - Level [X] - [evolution stage]"`.
- State changes: `aria-live="polite"`.

**Animation**
- Idle: Gentle float (translateY -3px to +3px), 4s cycle.
- Pulse: Glow opacity 0.8 to 1.0, 3s cycle.
- Particles: Slow drift outward, 6s cycle.
- Level up: Spirit expands with light burst 1s.
- Evolution: Dramatic morphing animation 2s with particle explosion.
- Achievement: Gold flash, sparkle burst 1s.
- Streak: Fire particles rise continuously.

**Interaction Rules**
- Tap for reaction animation and stat summary.
- Hover shows tooltip with level, XP, and streak.
- Long press for detailed breakdown of growth factors.
- Spirit reacts to learning activity completion.

**Do**
- Display evolving knowledge spirit on profile.
- Show spirit growth as user levels up.
- Celebrate milestones with spirit transformation.

**Do Not**
- Make the spirit too large (overpowering UI).
- Use distracting animations during active learning.
- Show the spirit in contexts where it doesn't fit (e.g., quiz).

---

## Appendix A: Color System Reference

| Token | Hex | Usage |
|-------|-----|-------|
| Primary Blue | #1A73E8 | Buttons, links, active states |
| Primary Dark | #1557B0 | Button hover |
| Primary Pressed | #0D47A1 | Button pressed |
| Success Green | #34A853 | Correct, complete, positive |
| Warning Yellow | #FBBC04 | Warning, medium confidence |
| Error Red | #EA4335 | Error, incorrect, destructive |
| Neutral Gray 100 | #F1F3F4 | Background hover, chips |
| Neutral Gray 200 | #E8EAED | Borders, dividers |
| Neutral Gray 300 | #DADCE0 | Borders, disabled |
| Neutral Gray 400 | #9AA0A6 | Placeholder text |
| Neutral Gray 500 | #5F6368 | Secondary text |
| Neutral Gray 600 | #3C4043 | Body text |
| Neutral Gray 700 | #202124 | Headings, primary text |

## Appendix B: Typography System Reference

| Style | Font | Size | Weight | Line Height |
|-------|------|------|--------|-------------|
| Display | Inter | 32px | Bold | 40px |
| H1 | Inter | 28px | Bold | 36px |
| H2 | Inter | 24px | SemiBold | 32px |
| H3 | Inter | 20px | SemiBold | 28px |
| H4 | Inter | 18px | SemiBold | 24px |
| Body Large | Inter | 16px | Regular | 24px |
| Body | Inter | 14px | Regular | 20px |
| Body Small | Inter | 13px | Regular | 18px |
| Caption | Inter | 12px | Regular | 16px |
| Label | Inter | 14px | Medium | 20px |
| Button | Inter | 14px | Medium | 20px |
| Button Large | Inter | 16px | Medium | 24px |
| Code | JetBrains Mono | 14px | Regular | 20px |

## Appendix C: Spacing System Reference

| Token | Value |
|-------|-------|
| Space 2 | 2px |
| Space 4 | 4px |
| Space 8 | 8px |
| Space 12 | 12px |
| Space 16 | 16px |
| Space 20 | 20px |
| Space 24 | 24px |
| Space 32 | 32px |
| Space 40 | 40px |
| Space 48 | 48px |
| Space 56 | 56px |
| Space 64 | 64px |
| Space 80 | 80px |
| Space 96 | 96px |

## Appendix D: Elevation / Shadow System Reference

| Level | Y Offset | Blur | Spread | Opacity | Usage |
|-------|----------|------|--------|---------|-------|
| 0 | 0 | 0 | 0 | 0% | Flat elements |
| 1 | 1px | 2px | 0 | 12% | Cards, subtle depth |
| 2 | 2px | 4px | 0 | 14% | Elevated cards, hover |
| 3 | 4px | 8px | 0 | 16% | Dropdowns, popovers |
| 4 | 6px | 12px | 0 | 18% | Modals, bottom sheets |
| 6 | 8px | 16px | 0 | 20% | FAB, snackbars |
| 8 | 12px | 24px | 0 | 22% | Dialogs, drawers |
| 12 | 16px | 32px | 0 | 24% | Date picker, top modal |

---

*End of Component Library Documentation.*
