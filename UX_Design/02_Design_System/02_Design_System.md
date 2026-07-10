# Design System


> **Version:** 2.0.0  
> **Last Updated:** 2026-07-10  
> **Status:** Living Document  
> **Owner:** Design Systems Team  

---

## Table of Contents

1. [Color System](#1-color-system)
2. [Typography System](#2-typography-system)
3. [Spacing System](#3-spacing-system)
4. [Layout Grid](#4-layout-grid)
5. [Breakpoints](#5-breakpoints)
6. [Iconography](#6-iconography)
7. [Illustration Style](#7-illustration-style)
8. [3D Style](#8-3d-style)
9. [Glassmorphism Rules](#9-glassmorphism-rules)
10. [Blur Rules](#10-blur-rules)
11. [Shadow Rules](#11-shadow-rules)
12. [Elevation Rules](#12-elevation-rules)
13. [Corner Radius Rules](#13-corner-radius-rules)
14. [Animation Rules](#14-animation-rules)
15. [Motion Rules](#15-motion-rules)
16. [Transition Rules](#16-transition-rules)
17. [Depth Rules](#17-depth-rules)
18. [Contrast Rules](#18-contrast-rules)
19. [Accessibility Rules](#19-accessibility-rules)
20. [Dark Theme](#20-dark-theme)
21. [Light Theme](#21-light-theme)
22. [Theme Switching](#22-theme-switching)
23. [Brand Identity](#23-brand-identity)
24. [Design Tokens](#24-design-tokens)

---

## 1. Color System

### 1.1 Overview

The color system is built around a warm, scholarly, and accessible palette designed for an Arabic-first, knowledge-centric learning platform. The system supports light and dark themes, meets WCAG AA standards at minimum, and provides semantic meaning through a structured token architecture.

We never use pure black (`#000000`) or pure white (`#FFFFFF`) anywhere in the system. Pure black creates visual vibration on digital screens and causes eye strain during extended reading sessions. Pure white feels harsh, especially in dark environments and on OLED displays. Instead, we use warm-tinted near-blacks and near-whites that reduce glare while maintaining appropriate contrast.

### 1.2 Primary Color Palette

The primary palette conveys wisdom, growth, and scholarly depth. The core hue is a deep teal-green with warm undertones, chosen for its association with knowledge, nature, and spiritual tranquility in Islamic art traditions. It reads as sophisticated and trustworthy across both Arabic and Latin interfaces.

#### 1.2.1 Primary Shades (Lighter — backgrounds, borders, subtle elements)

| Token | Light Theme | Dark Theme | Usage |
|-------|-------------|------------|-------|
| primary-shade-1 | #E8F4F0 | #1A2E28 | Surface tints, table row backgrounds, hover states on dark surfaces |
| primary-shade-2 | #C5E3DA | #2D4D42 | Disabled states, subtle borders, secondary backgrounds, progress track |
| primary-shade-3 | #9DCFC0 | #4A7A6A | Hover backgrounds, read-only fields, decorative elements |

#### 1.2.2 Primary Tones (Mid-range — interactive elements)

| Token | Light Theme | Dark Theme | Usage |
|-------|-------------|------------|-------|
| primary-tone-1 | #3E8E7A | #5DAF9A | Interactive elements, secondary CTAs, selected states, links on dark |
| primary-tone-2 | #2D6B5E | #3D9E88 | Primary buttons, active states, focus rings, main links |
| primary-tone-3 | #1F4E44 | #2D8A75 | Darker interactive hover, pressed states, active navigation |

#### 1.2.3 Primary Base (Deep — text, high emphasis)

| Token | Light Theme | Dark Theme | Usage |
|-------|-------------|------------|-------|
| primary-base-1 | #153A32 | #1F7A68 | Headings, high emphasis text, icon primary fills |
| primary-base-2 | #0E2822 | #1A6B5A | Extra emphasis, masthead backgrounds |
| primary-base-3 | #081712 | #155C4D | Maximum emphasis, branded overlays |

### 1.3 Secondary Color Palette

The secondary palette uses warm amber-gold tones that complement the primary green-teal. This palette draws inspiration from illuminated manuscripts, golden age scholarship, and the warmth of ambient light in traditional learning spaces.

#### 1.3.1 Secondary Shades

| Token | Light Theme | Dark Theme | Usage |
|-------|-------------|------------|-------|
| secondary-shade-1 | #FFF8EC | #2E281A | Warm surface tints, achievement backgrounds |
| secondary-shade-2 | #FFEDC2 | #4D4228 | Warm borders, muted decorative elements |
| secondary-shade-3 | #FFE099 | #7A6930 | Highlight backgrounds, hover on warm surfaces |

#### 1.3.2 Secondary Tones

| Token | Light Theme | Dark Theme | Usage |
|-------|-------------|------------|-------|
| secondary-tone-1 | #D4A043 | #E8B84A | Accent buttons, badges, rating stars, achievement icons |
| secondary-tone-2 | #B8892E | #D4A843 | Hover accent, active filters, category labels |
| secondary-tone-3 | #9A7222 | #BF9838 | Pressed accent, dark mode decorative elements |

#### 1.3.3 Secondary Base

| Token | Light Theme | Dark Theme | Usage |
|-------|-------------|------------|-------|
| secondary-base-1 | #7A5A18 | #A88328 | Warm headings, emphasis on dark backgrounds |
| secondary-base-2 | #5C4312 | #8C6E20 | High contrast warm text, awards text |
| secondary-base-3 | #3D2C0C | #705818 | Maximum warmth emphasis |

### 1.4 Neutral Palette

The neutral palette uses 50 shades from 50 to 950 (where 50 is lightest and 950 is darkest). All neutrals have a warm tint — achieved by adding 3-5% of the secondary amber hue to a standard gray. This prevents the cold, sterile feeling of pure gray neutrals.

#### Full Neutral Scale (50-950)

| Token | Light Theme | Dark Theme |
|-------|-------------|------------|
| neutral-50 | #FAFAF5 | #1A1A1E |
| neutral-100 | #F5F5F0 | #1F1F24 |
| neutral-150 | #F0EFEB | #24242A |
| neutral-200 | #E8E7E3 | #2A2A30 |
| neutral-250 | #E0DFDB | #303038 |
| neutral-300 | #D4D3CF | #383840 |
| neutral-350 | #C8C7C3 | #404048 |
| neutral-400 | #B8B7B3 | #484850 |
| neutral-450 | #A8A7A3 | #52525A |
| neutral-500 | #949390 | #5E5E68 |
| neutral-550 | #807F7C | #6C6C78 |
| neutral-600 | #6C6B68 | #7C7C88 |
| neutral-650 | #5C5B58 | #8C8C98 |
| neutral-700 | #4C4B48 | #9E9EA8 |
| neutral-750 | #3F3E3C | #AEAEB8 |
| neutral-800 | #343330 | #BEBEC8 |
| neutral-850 | #2A2928 | #CECED8 |
| neutral-900 | #1F1E1D | #DEDEE4 |
| neutral-950 | #141413 | #EEEEF0 |

### 1.5 Semantic Color Palette

Semantic colors communicate status, feedback, and information hierarchy. Each semantic category includes three shades. The semantic palette is intentionally desaturated by approximately 15% to maintain visual harmony with the overall warm, scholarly aesthetic.

#### 1.5.1 Success

| Token | Light Theme | Dark Theme | Usage |
|-------|-------------|------------|-------|
| success-shade-1 | #E6F5EB | #1A2E22 | Success background, toast backgrounds |
| success-tone-1 | #3BA86C | #4DCB82 | Success icon, success badge background |
| success-base-1 | #1D6B40 | #38B86E | Success text, success border, success label |

#### 1.5.2 Warning

| Token | Light Theme | Dark Theme | Usage |
|-------|-------------|------------|-------|
| warning-shade-1 | #FFF5E6 | #2E2618 | Warning background |
| warning-tone-1 | #D98C2E | #E8A84A | Warning icon |
| warning-base-1 | #8C5C1A | #D99C38 | Warning text, warning border |

#### 1.5.3 Error

| Token | Light Theme | Dark Theme | Usage |
|-------|-------------|------------|-------|
| error-shade-1 | #FCECEC | #2E1A1A | Error background, destructive toast bg |
| error-tone-1 | #D94A4A | #E85C5C | Error icon, required indicator |
| error-base-1 | #8C1A1A | #D94444 | Error text, error border, validation message |

#### 1.5.4 Info

| Token | Light Theme | Dark Theme | Usage |
|-------|-------------|------------|-------|
| info-shade-1 | #E8F0FC | #1A2230 | Info background, tooltip backgrounds |
| info-tone-1 | #3A7BD5 | #5C9CE8 | Info icon, info badge, link color |
| info-base-1 | #1A4D8C | #4088D4 | Info text, info border |

### 1.6 Learning Modes Colors

Each learning mode has a distinct color identity. These colors are used for mode icons, mode badges, mode-specific decorative elements, and mode transition animations.

#### 1.6.1 Story Mode (Warm Amber)

| Token | Light Theme | Dark Theme |
|-------|-------------|------------|
| mode-story-shade-1 | #FFF3E0 | #2E2418 |
| mode-story-tone-1 | #F5A623 | #FAB840 |
| mode-story-base-1 | #B87318 | #E8A030 |

#### 1.6.2 Quiz Mode (Blue)

| Token | Light Theme | Dark Theme |
|-------|-------------|------------|
| mode-quiz-shade-1 | #E3F0FF | #182430 |
| mode-quiz-tone-1 | #3B82F6 | #5C9EF8 |
| mode-quiz-base-1 | #1A5BBF | #4088E8 |

#### 1.6.3 Flashcard Mode (Teal)

| Token | Light Theme | Dark Theme |
|-------|-------------|------------|
| mode-flashcard-shade-1 | #E0F5F0 | #182E28 |
| mode-flashcard-tone-1 | #14B8A6 | #30D4C0 |
| mode-flashcard-base-1 | #0D7A6E | #20B8A8 |

#### 1.6.4 Podcast Mode (Purple)

| Token | Light Theme | Dark Theme |
|-------|-------------|------------|
| mode-podcast-shade-1 | #F0E8FF | #221830 |
| mode-podcast-tone-1 | #8B5CF6 | #A878F8 |
| mode-podcast-base-1 | #5B2DBF | #8860E0 |

#### 1.6.5 Documentary Mode (Slate)

| Token | Light Theme | Dark Theme |
|-------|-------------|------------|
| mode-documentary-shade-1 | #ECEEF0 | #1E2028 |
| mode-documentary-tone-1 | #64748B | #8896A8 |
| mode-documentary-base-1 | #3B4A5E | #6B7D94 |

#### 1.6.6 Map Mode (Emerald)

| Token | Light Theme | Dark Theme |
|-------|-------------|------------|
| mode-map-shade-1 | #E4F5E8 | #182E1E |
| mode-map-tone-1 | #34D399 | #4EE0A8 |
| mode-map-base-1 | #1A7A50 | #30C488 |

#### 1.6.7 Timeline Mode (Coral)

| Token | Light Theme | Dark Theme |
|-------|-------------|------------|
| mode-timeline-shade-1 | #FCE8E8 | #2E1A1E |
| mode-timeline-tone-1 | #F87171 | #F88A8A |
| mode-timeline-base-1 | #B83A3A | #E06868 |

#### 1.6.8 AI Mode (Indigo)

| Token | Light Theme | Dark Theme |
|-------|-------------|------------|
| mode-ai-shade-1 | #EAE8FF | #1C1830 |
| mode-ai-tone-1 | #6366F1 | #8084F8 |
| mode-ai-base-1 | #3A3DBF | #5C60E0 |

### 1.7 Color Contrast Ratios

All text and UI element color combinations must meet WCAG 2.1 AA standards as a minimum. AAA compliance is the target for reading text and primary interface labels.

#### 1.7.1 Minimum Contrast Requirements

| Context | Normal Text (< 18px or < 14px bold) | Large Text (>= 18px or >= 14px bold) | UI Components |
|---------|-------------------------------------|--------------------------------------|---------------|
| WCAG AA | 4.5:1 | 3:1 | 3:1 |
| WCAG AAA | 7:1 | 4.5:1 | — |

#### 1.7.2 Light Theme Contrast Verification

| Combination | Ratio | Pass AA | Pass AAA |
|-------------|-------|---------|----------|
| neutral-900 on neutral-50 | 14.5:1 | Yes | Yes |
| neutral-700 on neutral-50 | 8.2:1 | Yes | Yes |
| neutral-600 on neutral-100 | 5.8:1 | Yes | No |
| neutral-500 on neutral-50 | 4.8:1 | Yes | No |
| primary-tone-2 on neutral-50 | 5.2:1 | Yes | Yes (large) |

#### 1.7.3 Dark Theme Contrast Verification

| Combination | Ratio | Pass AA | Pass AAA |
|-------------|-------|---------|----------|
| neutral-50 on neutral-950 (dark) | 14.2:1 | Yes | Yes |
| neutral-100 on neutral-900 (dark) | 8.5:1 | Yes | Yes |
| neutral-200 on neutral-800 (dark) | 5.5:1 | Yes | No |
| neutral-300 on neutral-700 (dark) | 4.2:1 | No | No |

#### 1.7.4 Color Blindness Considerations

The color palette is designed to be distinguishable by users with common forms of color vision deficiency:
- Deuteranopia/Protanopia: The primary (teal-green) and secondary (amber-gold) palettes maintain luminance contrast. Do not rely solely on hue to distinguish interactive states — always pair color with icons, text labels, or patterns.
- Tritanopia: The semantic palette uses distinct lightness levels in addition to hue to ensure distinguishability.
- Monochromacy: All interactive elements maintain at least 3:1 luminance contrast against their backgrounds regardless of hue.

### 1.8 Color Usage Rules

#### 1.8.1 General Principles

1. Color communicates hierarchy, not decoration. Every color choice must serve a functional purpose — guiding attention, indicating state, or conveying meaning.
2. Use color sparingly. No more than three color families (primary, one semantic, one mode) in any single view. The neutral palette should dominate at approximately 70% of screen real estate.
3. Surface colors come from neutrals. Use neutral-50 through neutral-200 (light) or neutral-900 through neutral-950 (dark) for backgrounds. Reserve primary/secondary for interactive and accent elements.
4. Text always comes from the neutral palette with the exception of semantic text (success/warning/error/info) and branded headings.
5. Never apply color to text without sufficient contrast. All colored text must meet WCAG AA contrast against its background at minimum.

#### 1.8.2 Interactive State Mapping

| State | Element Color | Background | Border |
|-------|--------------|------------|--------|
| Default | primary-tone-2 | — | neutral-300 |
| Hover | primary-tone-3 | primary-shade-1 | primary-tone-1 |
| Active/Pressed | primary-base-1 | primary-shade-2 | primary-tone-2 |
| Focus | — | — | primary-tone-2 (2px) |
| Disabled | neutral-400 | neutral-200 | neutral-200 |
| Selected | primary-tone-2 | primary-shade-1 | primary-tone-2 |
| Error | error-base-1 | error-shade-1 | error-tone-1 |

#### 1.8.3 Mode Color Usage

Mode colors are reserved for:
- Mode selection UI (tab icons, mode cards, mode switcher)
- Active mode indicator (badges, active states)
- Mode-specific decorative elements (illustrations, animations, gradients)
- Progress indicators within a mode
- Achievement and milestone markers specific to a learning mode

#### 1.8.4 Text on Colored Backgrounds

| Background | Light Text | Dark Text |
|------------|-----------|-----------|
| Dark (L < 40%) | Use neutral-50 or neutral-100 | — |
| Mid (40% <= L <= 60%) | Use neutral-50 or neutral-100 | Use neutral-900 or neutral-800 |
| Light (L > 60%) | — | Use neutral-900 or neutral-800 |

### 1.9 Glassmorphism Color Rules

When applying glassmorphism effects, the following color rules ensure consistent visual appearance:

1. Background tint: Derive from the surface below. For overlays on the primary palette, tint the glass background with primary-shade-1. For neutral overlays, use neutral-100 (light) or neutral-900 (dark).
2. Opacity: Background opacity should be 60-80% (translucent enough to see through, opaque enough to read text).
3. Border: Use a 0.5-1px border with the material's base color at 20-30% opacity.
4. Content opacity: Text within glassmorphic elements should use the same text tokens as non-glass elements. Do not reduce text opacity for glass elements.
5. Contrast: Ensure that any content overlaid on glass passes contrast checks. If the background behind glass is too varied, provide a solid backdrop behind content areas.

---

## 2. Typography System

### 2.1 Overview

The typography system is designed for a bilingual (Arabic-primary, English-secondary) knowledge platform. It balances the calligraphic traditions of Arabic script with the readability requirements of modern digital interfaces.

### 2.2 Typeface Selection

#### 2.2.1 Primary Arabic Typeface: Noto Naskh Arabic

Primary choice: Noto Naskh Arabic (Google Fonts)
Fallback: Amiri, Traditional Arabic

Noto Naskh Arabic is selected as the primary Arabic typeface because:
- Excellent naskh-style calligraphy with modern digital hinting
- Comprehensive character set including all Arabic characters, ligatures, and Quranic marks
- Superior diacritic (tashkeel) positioning that avoids glyph collision
- Variable weight support (400-700 range) for responsive typographic hierarchy
- Clear, legible counters at small sizes (down to 12px)
- Harmonious baseline alignment with Latin scripts

#### 2.2.2 Primary Latin Typeface: Source Serif 4

Primary choice: Source Serif 4 (Google Fonts)
Fallback: Georgia, Charter, serif

Source Serif 4 is selected because:
- Designed specifically for extended reading on screen
- Generous x-height improves legibility at text sizes
- Warm, calligraphic qualities that complement Arabic Naskh
- Variable weight support (200-900 range)

#### 2.2.3 Secondary UI Typeface: Inter

Primary choice: Inter (Rasmus Andersson)
Fallback: SF Pro, Roboto, system-ui

Inter is selected for UI elements because:
- Purpose-built for user interfaces with optimized letterforms
- Excellent legibility at small sizes (down to 10px)
- Tall x-height and open counters improve readability
- Tabular figures for data display
- Clean, neutral appearance that does not compete with content typefaces

#### 2.2.4 Monospace Typeface: JetBrains Mono

Primary choice: JetBrains Mono
Fallback: Fira Code, Source Code Pro, monospace

### 2.3 Type Scale

| Token | Size | Line Height (Tight) | Line Height (Normal) | Line Height (Relaxed) | Weight | Usage |
|-------|------|---------------------|----------------------|-----------------------|--------|-------|
| text-10 | 10px | 12px | 14px | — | 400/500 | Caption, metadata, time stamps, legal text |
| text-12 | 12px | 14px | 18px | 20px | 400/500 | Small label, helper text, tab labels, badge text |
| text-14 | 14px | 16px | 20px | 24px | 400/500 | Body small, secondary text, list items |
| text-16 | 16px | 20px | 24px | 28px | 400 | Body text, paragraph, input text, card content |
| text-18 | 18px | 22px | 27px | 32px | 400 | Lead body, intro paragraphs, large inputs |
| text-20 | 20px | 24px | 30px | 36px | 500 | Subheading, navigation labels, button text |
| text-24 | 24px | 28px | 36px | 44px | 500 | Section heading, card title, modal title |
| text-30 | 30px | 36px | 44px | 54px | 600 | Page heading H2, hero subtitle |
| text-36 | 36px | 42px | 52px | 64px | 600 | Major heading H1, display heading |
| text-48 | 48px | 56px | 70px | 86px | 700 | Hero heading, page title, landing heading |
| text-60 | 60px | 68px | 88px | 108px | 700 | Large display, marketing hero |
| text-72 | 72px | 80px | 104px | 130px | 700 | Maximum display, special occasions |

### 2.4 Line Heights

| Token | Multiplier | When to Use |
|-------|-----------|-------------|
| leading-tight | 1.2 | Headings, labels, buttons, navigation, dense UI |
| leading-normal | 1.5 | Body text, paragraphs, cards, most content |
| leading-relaxed | 1.8 | Long-form reading, articles, stories, poetry |

#### Line Height Selection Guide

| Context | Line Height | Rationale |
|---------|-------------|-----------|
| Headings H1-H3 | 1.2 | Tight spacing emphasizes hierarchy |
| Body text (Latin) | 1.5 | Standard readable line height |
| Body text (Arabic) | 1.6 | Arabic script requires slightly more line spacing due to ascenders and diacritics |
| Long-form content | 1.8 | Maximum readability for extended reading sessions |
| UI labels | 1.2 | Compact, scannable interface elements |
| Buttons | 1.0-1.2 | Vertically centered single-line text |

### 2.5 Font Weights

| Weight | Token | Latin Face | Arabic Face | UI Face | Usage |
|--------|-------|-----------|-------------|---------|-------|
| 300 | weight-light | Source Serif 4 Light | Noto Naskh Arabic Light | Inter Light | Large display text, decorative headings |
| 400 | weight-regular | Source Serif 4 Regular | Noto Naskh Arabic Regular | Inter Regular | Body text, paragraphs, content |
| 500 | weight-medium | Source Serif 4 Medium | Noto Naskh Arabic Medium | Inter Medium | Subheadings, emphasized text, labels |
| 600 | weight-semibold | Source Serif 4 Semibold | Noto Naskh Arabic Semibold | Inter Semibold | Section headings, navigation |
| 700 | weight-bold | Source Serif 4 Bold | Noto Naskh Arabic Bold | Inter Bold | Major headings, strong emphasis |

### 2.6 Letter Spacing

| Token | Value | When to Use |
|-------|-------|-------------|
| tracking-tight | -0.02em | Headings, large text (36px+), Arabic text at display sizes |
| tracking-normal | 0em | Body text, most content, UI elements |
| tracking-wide | 0.05em | Uppercase labels, small caps, tag text, legal text |
| tracking-wider | 0.1em | All-caps navigation, badges, eyeglass UI |

#### Letter Spacing Guidelines

1. Negative tracking only on large text. Never apply negative letter spacing to text smaller than 24px.
2. Arabic text benefits from tighter tracking at display sizes.
3. Uppercase Latin text always needs positive tracking.
4. Never use tracking as a substitute for proper kerning.

### 2.7 Arabic-Specific Typography

#### 2.7.1 Baseline Alignment

Arabic and Latin text must share a consistent optical baseline when appearing together.

| Script | Baseline Position | Adjustment |
|--------|-------------------|------------|
| Latin | Standard alphabetic baseline | Reference baseline |
| Arabic | Slightly higher than Latin baseline | +2% of font size |
| Arabic (with tashkeel) | Variable based on diacritic stack | +6% of font size buffer |

#### 2.7.2 Diacritic (Tashkeel) Handling

1. Minimum line height with tashkeel: 1.6 (normal), 2.0 (relaxed).
2. Diacritic collision detection: At text sizes below 14px, maintain a minimum 4px clearance between diacritic marks and adjacent characters.
3. Tashkeel in UI: Avoid tashkeel in UI elements smaller than 16px.
4. Quranic text: Quranic verses with full tashkeel require leading-relaxed (1.8) minimum.
5. Tashkeel color: In learning contexts, diacritics may be rendered in primary-tone-1 at 70% opacity.

#### 2.7.3 Arabic Font Features

Enable the following OpenType features:
- Initial/Medial/Final forms (init, medi, fina)
- Ligatures (liga)
- Contextual alternates (calt)
- Kerning (kern)
- Justification alternates (jalt)

#### 2.7.4 Arabic Paragraph Width

| Script | Optimal Character Width | Optimal Pixels (16px base) |
|--------|------------------------|---------------------------|
| Latin | 60-75 characters | 480-600px |
| Arabic | 45-65 characters | 360-520px |

### 2.8 Paragraph Widths

| Context | Max Width | Min Width | Recommended |
|---------|-----------|-----------|-------------|
| Long-form content (Latin) | 720px | 340px | 660px |
| Long-form content (Arabic) | 680px | 320px | 580px |
| Article body | 720px | 400px | 640px |
| Card content | 480px | 260px | 360px |

#### Characters Per Line

| Script | Ideal Measure | Acceptable Range |
|--------|---------------|------------------|
| Latin | 66 characters | 45-75 characters |
| Arabic | 55 characters | 40-65 characters |

### 2.9 Typographic Hierarchy

#### Content Hierarchy

H1 (text-48) → Page title, hero heading
H2 (text-30) → Section heading
H3 (text-24) → Subsection heading
H4 (text-20) → Group heading, card title
Body (text-16) → Paragraph, content
Small (text-14) → Metadata, secondary content
Caption (text-12) → Labels, timestamps, footnotes
Micro (text-10) → Legal text, auxiliary data

#### UI Hierarchy

Heading (text-24/600) → Screen title, modal title
Subheading (text-20/500) → Section within screen
Body (text-16/400) → UI content, descriptions
Label (text-14/500) → Input labels, list headers
Caption (text-12/400) → Helper text, metadata

### 2.10 Typography Responsive Behavior

| Token | Mobile | Tablet | Desktop | Wide |
|-------|--------|--------|---------|------|
| text-10 | 10px | 10px | 10px | 10px |
| text-12 | 12px | 12px | 12px | 12px |
| text-14 | 14px | 14px | 14px | 14px |
| text-16 | 15px | 16px | 16px | 16px |
| text-18 | 16px | 17px | 18px | 18px |
| text-20 | 18px | 19px | 20px | 20px |
| text-24 | 20px | 22px | 24px | 24px |
| text-30 | 24px | 26px | 30px | 30px |
| text-36 | 28px | 32px | 36px | 36px |
| text-48 | 32px | 40px | 48px | 48px |
| text-60 | 36px | 48px | 60px | 60px |
| text-72 | 40px | 56px | 72px | 72px |

---

## 3. Spacing System

### 3.1 Overview

The spacing system is based on a 4px base unit, providing consistency across all components and layouts. All spacing values in the design system are multiples of 4px (with the exception of 2px, which exists for micro-adjustments).

### 3.2 Base Unit

| Token | Value | Rationale |
|-------|-------|-----------|
| base-unit | 4px | Provides sufficient granularity for fine-tuning while maintaining consistency. |

### 3.3 Spacing Scale

| Token | Value | Ratio | Typical Usage |
|-------|-------|-------|---------------|
| space-2 | 2px | 0.5x | Micro spacing between nested elements, icon gap in compact buttons |
| space-4 | 4px | 1x | Inline spacing between related elements, icon-text gaps |
| space-8 | 8px | 2x | Between icon and label, small component padding (chips, badges) |
| space-12 | 12px | 3x | Dense card padding, compact form spacing, list item padding |
| space-16 | 16px | 4x | Standard card padding, form field spacing, button horizontal padding |
| space-20 | 20px | 5x | Section spacing within cards, modal padding (compact) |
| space-24 | 24px | 6x | Default card padding, section headings offset, list padding |
| space-32 | 32px | 8x | Page section spacing, large card padding, dialog padding |
| space-40 | 40px | 10x | Major section spacing, hero area padding, large component gap |
| space-48 | 48px | 12x | Feature section separation, large modal padding |
| space-64 | 64px | 16x | Page section margins, content block separation |
| space-80 | 80px | 20x | Major page sections, hero bottom margin, feature blocks |
| space-96 | 96px | 24x | Very large section separation, full-screen sections |
| space-128 | 128px | 32x | Maximum spacing, page top/bottom padding, landings |

### 3.4 Margin Rules

#### 3.4.1 Screen Edge Margins

| Breakpoint | Horizontal Margin | Top Margin | Bottom Margin |
|------------|-------------------|------------|---------------|
| Mobile (portrait) | space-16 | space-16 | space-16 |
| Mobile (landscape) | space-24 | space-16 | space-16 |
| Tablet (portrait) | space-24 | space-24 | space-24 |
| Tablet (landscape) | space-32 | space-24 | space-24 |
| Desktop | space-40 | space-32 | space-32 |
| Wide | space-40 (auto-center) | space-32 | space-32 |

#### 3.4.2 Component Margins

| Component Group | Margin Bottom | Margin Top |
|-----------------|---------------|------------|
| Heading after content | — | space-24 |
| Heading before content | space-16 | — |
| Paragraph after heading | space-8 | — |
| Paragraph after paragraph | space-16 | — |
| List after content | space-16 | — |
| Image after content | space-24 | — |
| Button group | space-16 | — |
| Form field | space-20 | — |
| Form section | space-32 | — |
| Card in grid | space-24 | — |

### 3.5 Padding Rules

#### 3.5.1 Component Padding

| Component | Horizontal Padding | Vertical Padding |
|-----------|-------------------|-----------------|
| Button (default) | space-16 | space-8 |
| Button (compact) | space-12 | space-4 |
| Button (large) | space-24 | space-12 |
| Input field | space-12 | space-10 |
| Select dropdown | space-12 | space-10 |
| Chip/Tag | space-8 | space-4 |
| Badge | space-6 | space-2 |
| Navigation item | space-12 | space-8 |
| Table cell | space-12 | space-10 |
| Tooltip | space-8 | space-4 |
| Toast/Snackbar | space-16 | space-12 |

#### 3.5.2 Card Padding

| Card Size | Horizontal Padding | Vertical Padding |
|-----------|-------------------|-----------------|
| Compact card | space-12 | space-12 |
| Standard card | space-20 | space-20 |
| Expanded card | space-24 | space-24 |
| Full-width card | space-24 | space-32 |
| Modal card | space-24 | space-24 |
| Sheet | space-24 | space-20 |

#### 3.5.3 Content Padding

| Content Area | Padding |
|--------------|---------|
| Article body | space-24 (left), space-24 (right) |
| Rich text content | space-32 (all sides) |
| Code block | space-16 (all sides) |
| Quote block | space-20 (all sides) |
| Media container | space-0 (edge-to-edge preferred) |

### 3.6 List Spacing

| List Type | Item Spacing | Left Padding |
|-----------|-------------|--------------|
| Bulleted list | space-8 | space-20 |
| Numbered list | space-8 | space-24 |
| Definition list | space-12 | space-0 |
| Navigation list | space-4 | space-0 |
| Action list (with icons) | space-2 | space-0 |
| Data list (key-value) | space-12 | space-0 |
| Checklist | space-8 | space-28 |

### 3.7 Section Spacing

| Section Type | Top Gap | Bottom Gap |
|--------------|---------|------------|
| Page sections | space-48 | space-48 |
| Feature blocks | space-64 | space-64 |
| Hero section | space-0 | space-64 |
| Footer section | space-48 | space-0 |
| Sidebar sections | space-24 | space-24 |
| Modal sections | space-20 | space-20 |
| Form sections | space-32 | space-32 |

### 3.8 Stack Spacing

| Stack Density | Gap | When to Use |
|---------------|-----|-------------|
| Dense | space-4 | Compact lists, tight metadata, stacked badges |
| Compact | space-8 | Related controls, inline form, button groups |
| Standard | space-16 | Default content stack, card content, form fields |
| Comfortable | space-24 | Stretched layouts, reading content, step forms |
| Loose | space-32 | Feature sections, dashboard widgets, settings groups |
| Spacious | space-48 | Major page sections, hero content, landing blocks |

### 3.9 Inline Spacing

| Context | Gap | When to Use |
|---------|-----|-------------|
| Icon + text | space-6 | Button with icon, label with indicator |
| Tag/Chip group | space-8 | Topic tags, filter chips |
| Button group | space-8 | Adjacent action buttons |
| Navigation items | space-20 | Top nav, tab bar, breadcrumb items |
| Form field + label | space-12 | Input with label |
| Status indicator + text | space-6 | Online status, progress badge |

### 3.10 Spacing Responsive Behavior

| Token | Mobile | Tablet | Desktop | Wide |
|-------|--------|--------|---------|------|
| space-2 | 2px | 2px | 2px | 2px |
| space-4 | 4px | 4px | 4px | 4px |
| space-8 | 8px | 8px | 8px | 8px |
| space-12 | 12px | 12px | 12px | 12px |
| space-16 | 16px | 16px | 16px | 16px |
| space-20 | 16px | 20px | 20px | 20px |
| space-24 | 20px | 24px | 24px | 24px |
| space-32 | 24px | 28px | 32px | 32px |
| space-40 | 32px | 36px | 40px | 40px |
| space-48 | 32px | 40px | 48px | 48px |
| space-64 | 40px | 48px | 64px | 64px |
| space-80 | 48px | 64px | 80px | 80px |
| space-96 | 64px | 80px | 96px | 96px |
| space-128 | 80px | 96px | 128px | 128px |

---

## 4. Layout Grid

### 4.1 Overview

The layout grid is a 12-column, fluid, responsive system that provides consistent structure across all screen sizes. The grid is built with CSS Grid Layout and supports nesting, variable column spans, gridless areas, and content-aware breakout layouts.

### 4.2 Grid Structure

| Property | Value |
|----------|-------|
| Total columns | 12 |
| Grid type | Fluid (percentage-based) |
| Container type | Max-width with auto margins |
| Max content width | 1200px (desktop) |
| Grid implementation | CSS Grid Layout |

### 4.3 Gutter and Margin by Breakpoint

| Breakpoint | Gutter | Screen Margin | Container Max-Width |
|------------|--------|---------------|---------------------|
| Mobile portrait (0-639px) | 16px | 16px | 100% |
| Mobile landscape (640-767px) | 16px | 24px | 100% |
| Tablet portrait (768-1023px) | 24px | 24px | 100% |
| Tablet landscape (1024-1279px) | 24px | 32px | 100% |
| Desktop standard (1280-1511px) | 32px | 40px | 1200px |
| Desktop wide (1512-1919px) | 32px | 40px | 1200px |
| Ultra-wide (1920px+) | 32px | auto (centered) | 1440px |

### 4.4 Column Behavior

#### Column Spans

| Span | Percentage (12-col) | Typical Use |
|------|--------------------|-------------|
| 1 | 8.33% | Icons, labels, narrow indicators |
| 2 | 16.67% | Sidebar label, small cards, filters |
| 3 | 25% | Quarter-width sidebar, narrow panels |
| 4 | 33.33% | Third-width side content, forms |
| 5 | 41.67% | Asymmetric split, content with sidebar |
| 6 | 50% | Half split, main content areas |
| 7 | 58.33% | Content with narrow sidebar |
| 8 | 66.67% | Two-thirds main content |
| 9 | 75% | Three-quarters content width |
| 10 | 83.33% | Wide content, data tables |
| 11 | 91.67% | Maximum content with gutter margin |
| 12 | 100% | Full-width content, hero sections |

### 4.5 Layout Patterns

#### Single Column
Use when: Mobile, article reading, focused content, forms.

#### Two Column (Sidebar + Content)
Use when: Dashboard, settings, content browsing, learning modules.

#### Two Column (Split)
Use when: Comparison views, split forms, two-panel layouts.

#### Three Column
Use when: Dashboard widget grids, card galleries, resource libraries.

#### Asymmetric
Use when: Content with metadata sidebar, article with table of contents.

#### Full Bleed
Use when: Hero sections, knowledge graph visualization, maps, media galleries.

### 4.6 Grid Behaviors

#### Mobile (0-639px) — Single Column
All content stacks vertically. Grid becomes a 4-column micro-grid. Gutters: 16px.

#### Tablet Portrait (768-1023px) — 6-8 Columns
Two-column layouts become viable. Card grids show 2 columns. Gutters: 24px.

#### Tablet Landscape (1024-1279px) — 8-10 Columns
Full 12-column grid approached. Three-column card grids appear.

#### Desktop (1280px+) — 12 Columns
Full 12-column grid active. Multi-column card grids (3-4 columns).

### 4.7 Nested Grids

Grids may be nested one level deep. Nested grids use the same gutter as their parent.

### 4.8 Gridless Areas

| Area | Gridless Behavior | Rationale |
|------|-------------------|-----------|
| Knowledge graph | Full bleed, free-form | Organic node layouts need unconstrained space |
| Maps | Full bleed, scrollable | Geographic content cannot conform to rigid grid |
| Media galleries | Variable width, staggered | Media aspect ratios determine layout |
| Hero sections | Full bleed background | Visual impact requires edge-to-edge treatment |
| Knowledge tree | Full bleed, zoomable | Interactive tree structures need infinite canvas |
| Memory garden | Full bleed, organic | Organic growth visualization |

### 4.9 Content Max-Widths

| Context | Max Width | Rule |
|---------|-----------|------|
| Standard content | 1200px | max-width: 1200px; margin: 0 auto; |
| Reading content | 720px | max-width: 720px; margin: 0 auto; |
| Full bleed | None | width: 100%; |
| Sidebar | 360px | max-width: 360px; |
| Form | 600px | max-width: 600px; |

---

## 5. Breakpoints

### 5.1 Overview

The breakpoint system uses device-independent pixel widths and follows a mobile-first approach.

### 5.2 Breakpoint Definitions

| Category | Name | Min Width | Max Width | Target Devices |
|----------|------|-----------|-----------|----------------|
| Mobile | mobile-portrait | 0px | 639px | Phones in portrait, small foldables |
| Mobile | mobile-landscape | 640px | 767px | Phones in landscape, large foldables |
| Tablet | tablet-portrait | 768px | 1023px | iPads, tablets in portrait, small laptops |
| Tablet | tablet-landscape | 1024px | 1279px | iPads in landscape, small laptops |
| Desktop | desktop | 1280px | 1511px | Standard monitors, laptops |
| Desktop | desktop-wide | 1512px | 1919px | Large monitors, 15-16 inch laptops |
| Ultra-wide | ultrawide | 1920px | — | 27in+ monitors, 4K displays, ultra-wide |

### 5.3 Media Query Syntax

#### Mobile-First (Min-Width)

Tablet portrait: @media (min-width: 768px) { }
Tablet landscape: @media (min-width: 1024px) { }
Desktop: @media (min-width: 1280px) { }
Desktop wide: @media (min-width: 1512px) { }
Ultra-wide: @media (min-width: 1920px) { }

#### Desktop-First (Max-Width)

Desktop standard: @media (max-width: 1919px) { }
Tablet landscape: @media (max-width: 1279px) { }
Tablet portrait: @media (max-width: 1023px) { }
Mobile: @media (max-width: 767px) { }
Mobile portrait: @media (max-width: 639px) { }

### 5.4 Breakpoint-Specific Behavior

#### Mobile Portrait (0-639px)
Grid: 4 columns, Gutter: 16px, Margin: 16px, Navigation: Bottom tab bar, Touch targets: 44px minimum, Cards: Full width, Modals: Full screen or bottom sheet.

#### Mobile Landscape (640-767px)
Grid: 6 columns, Gutter: 16px, Margin: 24px, Two-pane possible.

#### Tablet Portrait (768-1023px)
Grid: 8 columns, Gutter: 24px, Margin: 24px, Navigation: Sidebar rail + top header, Cards: Two-column grid.

#### Tablet Landscape (1024-1279px)
Grid: 12 columns, Gutter: 24px, Margin: 32px, Navigation: Full sidebar + top header, Cards: Three-column grid.

#### Desktop (1280-1511px)
Grid: 12 columns, Gutter: 32px, Margin: 40px, Content max-width: 1200px, Cards: Three-to-four column grid.

#### Desktop Wide (1512-1919px)
Grid: 12 columns, Gutter: 32px, Margin: 40px (centered), Content max-width: 1200px, Cards: Four-column grid.

#### Ultra-Wide (1920px+)
Grid: 12 columns, Gutter: 32px, Margin: Auto (centered), Content max-width: 1440px, Cards: Four-to-five column grid.

### 5.5 Content Max-Width

| Context | Max Width | When Applied |
|---------|-----------|--------------|
| General content | 1200px | All screens 1280px+ |
| Reading content | 720px | Article readers, story mode, long-form |
| Knowledge graph | 100% | Full bleed, no constraint |
| Map | 100% | Full bleed, no constraint |
| Media | 100% | Edge to edge, full bleed |
| Form content | 600px | Centered, all screens |

### 5.6 Reading Max-Width

The reading max-width (720px) applies to:
1. Article/story content — Long-form narrative text
2. Lesson content — Educational text sections
3. Documentation — Help articles, documentation
4. Discussion threads — Forum posts, comments
5. Quizzes — Question text, explanations

---


## 6. Iconography

### 6.1 Overview

The icon system uses outlined, rounded-terminus icons with consistent stroke weights across all sizes. Icons are semantic, accessible, and designed to support both decorative and communicative functions.

### 6.2 Icon Style

| Property | Value |
|----------|-------|
| Style | Outlined with rounded terminals |
| Stroke weight | 2px (at 24px baseline) |
| Corner radius (joins) | 2px (rounded) |
| Fill | None (transparent interior) |
| Cap | Round (line endings) |

#### Style Characteristics

1. Outlined construction. Icons are built from strokes rather than filled shapes.
2. Rounded terminals. All stroke endings use round caps, giving icons a friendly, approachable feel.
3. Consistent stroke weight. At the 24px baseline grid, all icons use 2px strokes.
4. Geometric precision. Icons are constructed on a geometric grid with 45-degree and 90-degree angles.
5. No filled variants. The system uses outlined icons exclusively.

### 6.3 Icon Sizes

| Token | Size | Stroke | Grid | When to Use |
|-------|------|--------|------|-------------|
| icon-16 | 16px | 1.5px | 16x16 | Inline with text, compact buttons, dense lists |
| icon-20 | 20px | 1.75px | 20x20 | Inline with labels, tab icons, small buttons |
| icon-24 | 24px | 2px | 24x24 | Default icon size, buttons, navigation items |
| icon-32 | 32px | 2.5px | 32x32 | Large buttons, empty states, feature icons |
| icon-40 | 40px | 3px | 40x40 | Mode icons, category icons, hero icons |
| icon-48 | 48px | 3px | 48x48 | Large feature icons, landing page, onboarding |

### 6.4 Icon Grid and Construction

The 24x24 grid is the baseline for all icon construction. The artwork area is 20x20 centered with 2px padding. All construction aligns to 1px increments.

| Property | Value |
|----------|-------|
| Grid size | 24x24 |
| Artwork area | 20x20 (centered with 2px padding) |
| Safe zone | Inner 20x20 (no critical elements outside) |
| Trim area | Outer 2px (allow minor overflow for optical alignment) |
| Pixel grid | Aligned to 1px increments |

### 6.5 Icon Categories

#### Navigation Icons

| Icon Name | Token | Usage |
|-----------|-------|-------|
| Home | icon-home | Dashboard, home screen |
| Explore | icon-explore | Browse, discover content |
| Library | icon-library | Saved content, bookmarks |
| Search | icon-search | Search input trigger |
| Profile | icon-profile | User profile, account |
| Settings | icon-settings | Preferences, configuration |
| Back | icon-arrow-left | Navigate back |
| Forward | icon-arrow-right | Navigate forward |
| Close | icon-close | Dismiss, close panel |
| Menu | icon-menu | Hamburger menu, navigation drawer |
| More | icon-more | Overflow actions |
| History | icon-history | Recent activity |
| Favorites | icon-favorites | Starred, liked content |

#### Action Icons

| Icon Name | Token | Usage |
|-----------|-------|-------|
| Add | icon-add | Create new, add item |
| Edit | icon-edit | Modify, edit content |
| Delete | icon-delete | Remove, trash |
| Save | icon-save | Save changes, bookmark |
| Share | icon-share | Share, export, send |
| Download | icon-download | Download content |
| Upload | icon-upload | Upload, submit |
| Copy | icon-copy | Duplicate, copy to clipboard |
| Filter | icon-filter | Filter list, refine results |
| Sort | icon-sort | Order, arrange |
| Refresh | icon-refresh | Reload, regenerate |
| Play | icon-play | Start media, begin lesson |
| Pause | icon-pause | Pause media, lesson |
| Skip | icon-skip | Skip forward |
| Fullscreen | icon-fullscreen | Expand to full screen |

#### Content Icons

| Icon Name | Token | Usage |
|-----------|-------|-------|
| Article | icon-article | Written content, reading |
| Video | icon-video | Video content |
| Audio | icon-audio | Audio, podcast |
| Book | icon-book | Course, chapter, textbook |
| Document | icon-document | PDF, file |
| Image | icon-image | Image, photo, illustration |
| Link | icon-link | External link, reference |
| Quote | icon-quote | Citation, pull quote |
| Note | icon-note | Annotation, margin note |

#### Media Icons

| Icon Name | Token | Usage |
|-----------|-------|-------|
| Play Circle | icon-play-circle | Media thumbnail overlay |
| Pause Circle | icon-pause-circle | Active media pause |
| Volume | icon-volume | Audio on |
| Volume Muted | icon-volume-muted | Audio off |
| Caption | icon-caption | Subtitles, closed captions |
| Speed | icon-speed | Playback speed |
| Quality | icon-quality | Resolution, quality settings |
| Transcript | icon-transcript | Show transcript |

#### Status Icons

| Icon Name | Token | Usage |
|-----------|-------|-------|
| Check | icon-check | Success, complete, confirmed |
| Warning | icon-warning | Warning, caution |
| Error | icon-error | Error, critical issue |
| Info | icon-info | Information, details |
| Question | icon-question | Help, query, FAQ |
| Progress | icon-progress | In progress, loading |
| Clock | icon-clock | Time, duration, due date |
| Calendar | icon-calendar | Date, schedule, deadline |
| Bell | icon-bell | Notification, alert |
| Bell Off | icon-bell-off | Notifications muted |

#### Learning Icons

| Icon Name | Token | Usage |
|-----------|-------|-------|
| Quiz | icon-quiz | Quiz mode |
| Flashcard | icon-flashcard | Flashcard mode |
| Story | icon-story | Story mode |
| Podcast | icon-podcast | Podcast mode |
| Documentary | icon-documentary | Documentary mode |
| Map | icon-map | Map mode, concept map |
| Timeline | icon-timeline | Timeline mode |
| Brain | icon-brain | AI mode, smart features |
| Achievement | icon-achievement | Badges, rewards |
| Streak | icon-streak | Daily streak |
| Level | icon-level | Level up, progress |
| Knowledge | icon-knowledge | Knowledge graph |
| Memory | icon-memory | Memory garden |

#### Social Icons

| Icon Name | Token | Usage |
|-----------|-------|-------|
| Chat | icon-chat | Discussion, conversation |
| Comment | icon-comment | Add comment |
| Like | icon-like | Thumbs up, like |
| Dislike | icon-dislike | Thumbs down, dislike |
| Team | icon-team | Group, study group |
| Mentor | icon-mentor | Mentor, guide |
| Leaderboard | icon-leaderboard | Rankings, scores |

#### AI Icons

| Icon Name | Token | Usage |
|-----------|-------|-------|
| AI Sparkle | icon-ai-sparkle | AI assistant, smart feature |
| AI Generate | icon-ai-generate | AI content generation |
| AI Explain | icon-ai-explain | AI explanation |
| AI Summarize | icon-ai-summarize | AI summary |
| AI Suggest | icon-ai-suggest | AI suggestion, recommend |
| AI Analyze | icon-ai-analyze | AI analysis, insights |
| AI Chat | icon-ai-chat | AI conversation |
| AI Translate | icon-ai-translate | AI translation |

### 6.6 Accessibility

#### Decorative vs Semantic Icons

| Type | Definition | Handling |
|------|------------|----------|
| Decorative | Icon that repeats or supplements existing text content | aria-hidden=true, empty alt text |
| Semantic | Icon that conveys meaning not present in visible text | Descriptive aria-label, non-empty alt text |

#### Accessibility Rules

1. All interactive icons must have labels. Buttons with only icons must have aria-label describing the action.
2. Decorative icons are hidden from screen readers. Use aria-hidden=true.
3. Status icons paired with text must have a visible text label or aria-label.
4. Touch target minimum. Icon-only buttons must have a minimum 44x44pt touch target.
5. Focus indicators. Icon buttons must have visible focus indicators.

### 6.7 Icon + Label Rules

| Context | Gap | Alignment | Behavior |
|---------|-----|-----------|----------|
| Icon before label | space-6 | Center (vertically) | Icon leads visually; label follows |
| Icon after label | space-6 | Center (vertically) | Icon indicates action, external link |
| Icon above label | space-4 | Center (horizontally) | Feature icons, category icons |

### 6.8 Animation

#### Hover Animation

| Property | Value |
|----------|-------|
| Transform | Scale(1.1) or translateY(-1px) |
| Duration | 200ms |
| Easing | cubic-bezier(0.2, 0, 0, 1) |

#### State Change Animation

| State | Animation | Duration |
|-------|-----------|----------|
| Default to Active | Rotate or flip | 300ms |
| Loading | Spin (360 degrees over 2s) | Continuous |
| Toggle | Slide and fill | 200ms |

#### Reduced Motion

When prefers-reduced-motion: reduce is set:
- Disable hover transformations
- Remove morphing animations
- Keep opacity transitions only (200ms fade)
- Disable spin/rotate animations

---

## 7. Illustration Style

### 7.1 Overview

The illustration system uses a flat-with-depth style characterized by warm color palettes, geometric influences drawn from Islamic art traditions, and a focus on scholarly, knowledge-centered themes.

### 7.2 Core Style Definition

| Property | Value |
|----------|-------|
| Technique | Flat vector with dimensional depth |
| Color palette | Warm, scholarly (derived from design system colors) |
| Geometric influence | Islamic art patterns, arabesques, tessellations |
| Human figures | Stylized, diverse, expressive |
| Environments | Warm, inviting, scholarly spaces |
| Line work | Minimal or none (shape-based) |
| Shadows | Soft, colored (not black) with 30-50% opacity |
| Highlights | Atmospheric, gentle light |

### 7.3 Flat with Depth Technique

#### Depth Layers

| Layer | Description | Opacity | Example |
|-------|-------------|---------|---------|
| Background | Farthest element, defines atmosphere | 100% | Warm gradient, geometric pattern |
| Midground | Main subject area | 100% | Characters, primary objects |
| Foreground | Closest elements, framing | 100% | Overlapping shapes, UI elements |
| Shadow layer | Cast shadows | 30-50% | Soft colored shadows beneath elements |
| Highlight layer | Light effects | 20-40% | Soft glow on top of subjects |

#### Depth Construction Rules

1. Three layers minimum. Every illustration should have at least background, midground, and foreground layers.
2. Overlap creates space. Elements in different layers should overlap to establish spatial relationships.
3. Size hierarchy. Foreground elements are larger; background elements are smaller and lighter.
4. Color temperature. Cooler/lighter colors in background, warmer/darker in foreground.
5. Detail density. More detail in foreground, less in background (atmospheric perspective).

### 7.4 Warm Color Palette

| Color Family | Primary Hex | Usage |
|-------------|-------------|-------|
| Warm cream | #FFF8F0 | Background base, skin tones |
| Terracotta | #D4785C | Warm accents, clothing, objects |
| Deep amber | #C4924A | Scholarly tones, books, wood |
| Sage green | #7A9E8A | Natural elements, plants, knowledge |
| Deep teal | #3D7A6E | Primary illustration color |
| Warm gray | #8A8680 | Neutral elements, shadows |
| Soft gold | #E8C87A | Highlights, illumination, achievement |
| Deep brown | #5C4038 | Line work (if used), dark accents |
| Muted navy | #4A5A6E | Night scenes, depth, shadow |
| Dusty rose | #C48A8A | Warm accents, diversity elements |

### 7.5 Geometric Influences (Islamic Art Inspiration)

#### Pattern Elements

| Pattern Type | Construction | Usage |
|--------------|-------------|-------|
| Star polygons | 8-pointed and 12-pointed stars | Background patterns, decorative frames |
| Tessellations | Repeating geometric units | Page backgrounds, section dividers |
| Arabesques | Flowing, symmetrical plant-based curves | Decorative borders, frame corners |
| Girih | Complex interlocking geometric patterns | Knowledge visualization, learning paths |
| Muqarnas | Three-dimensional geometric stalactites | Architectural elements, portals |

#### Pattern Usage Rules

1. Patterns as backgrounds only. Geometric patterns should not compete with main subject matter.
2. Opacity reduction. Background patterns at 5-15% opacity for subtle texture.
3. Color matched to theme. Patterns use the same base color as background.
4. Cultural respect. Use patterns with understanding and respect for their cultural significance.
5. Moderation. One pattern element per illustration maximum.

### 7.6 Human Figures

#### Style Characteristics

| Feature | Style |
|---------|-------|
| Proportions | Stylized (5-6 heads tall), elongated |
| Faces | Minimal features, expressive eyes, soft features |
| Skin tones | Diverse range (warm undertones) |
| Hair | Stylized shapes, modest styles |
| Clothing | Flowing, modest, timeless scholarly attire |
| Headwear | Optional, diverse representation (hijab, turban, etc.) |
| Hands | Simplified, gentle gestures |
| Posture | Reflective, engaged, learning-oriented |

#### Diversity Requirements

Every illustration set must include representation across:
- Skin tones: Minimum 5 distinct skin tones across any illustration series
- Age: Children, young adults, adults, elders
- Gender: Balanced gender representation
- Ability: Include figures with assistive devices, visible disabilities
- Cultural dress: Varied cultural attire reflecting the global learning community

#### Figure Expression Guide

| Emotion | Visual Cues |
|---------|-------------|
| Curiosity | Head tilted, hand on chin, wide eyes |
| Discovery | Arms open, upward gaze, slight smile |
| Contemplation | Hand on book/chin, soft eyes, slight head tilt |
| Joy | Open smile, arms relaxed, bright eyes |
| Focus | Leaning forward, narrowed eyes, still posture |
| Collaboration | Facing each other, gesturing, nodding |
| Achievement | Arms raised, upright posture, wide smile |

### 7.7 Environments

| Environment | Color Palette | Elements | Mood |
|-------------|--------------|----------|------|
| Library/Study | Warm browns, soft golds, sage | Bookshelves, reading lamps, desks, globes | Focused, tranquil |
| Garden | Sage greens, terracotta, cream | Trees, flowers, water features, paths | Peaceful, reflective |
| Observatory | Deep blues, golds, midnight | Stars, telescopes, celestial maps | Wondrous, expansive |
| Studio | Warm creams, terracotta, amber | Desks, windows, plants, art | Creative, warm |
| Classroom | Sage, warm wood, cream | Desks, boards, books, globes | Engaged, active |

### 7.8 Knowledge Visualization

#### Abstract Knowledge Elements

| Element | Visual Style | Usage |
|---------|-------------|-------|
| Knowledge nodes | Circular, glowing, connected by lines | Knowledge graph visualization |
| Learning paths | Flowing curves with node markers | Progress visualization |
| Concept clouds | Soft overlapping shapes | Abstract idea representation |
| Data streams | Organic flowing lines | Information flow |
| Idea sparks | Small starburst shapes, glowing | Moments of insight |
| Memory particles | Small translucent circles | Memory garden elements |

#### Style Rules

1. Organic forms. Knowledge visualizations use flowing, organic shapes rather than rigid grids.
2. Subtle gradients. Light gradients create depth without harsh transitions.
3. Glow effects. Knowledge elements have soft, ambient glow (20-40% opacity Gaussian blur).
4. Consistent language. Knowledge elements share visual language across all illustrations.
5. Scale hierarchy. More important knowledge nodes are larger and more luminous.

### 7.9 AI Visualization

| Element | Visual Style | Usage |
|---------|-------------|-------|
| AI presence | Luminous sphere or geometric shape | AI assistant indicator |
| Neural patterns | Flowing connected lines with pulsing nodes | AI processing visualization |
| Data particles | Small light points in flowing motion | Information processing |
| AI aura | Soft colored halo (indigo/blue) | AI presence glow |
| Knowledge synthesis | Converging lines merging into light | AI analysis result |

#### AI Visualization Rules

1. Indigo-primary. AI elements use the AI mode color palette (indigo tones).
2. Ethereal quality. AI visualizations should feel luminous, light, and ethereal.
3. Motion-friendly. AI elements are designed to easily animate (pulse, flow, converge).
4. Transparency. AI elements use opacity to suggest their intangible nature.
5. Personality. AI visualizations should feel helpful, warm, and intelligent.

### 7.10 Character Illustrations

#### Proportional Consistency

| Body Part | Proportion (relative to head) |
|-----------|-------------------------------|
| Head | 1 unit (baseline) |
| Torso | 2 units |
| Arms | 2.5 units |
| Legs | 2.5 units |
| Total height | 6 units (stylized) |

#### Character Library

| Character | Role | Visual Signature |
|-----------|------|------------------|
| Scholar | Guide, narrator | Elderly, warm eyes, scholarly robe, book |
| Student | Learner, protagonist | Young, curious, varied attire |
| Mentor | Teacher, coach | Wise, gentle, explaining gesture |
| Explorer | Discovery seeker | Adventurous pose, looking outward |
| Artist | Creative thinker | Playful details, colorful accents |

---

## 8. 3D Style

### 8.1 Overview

The 3D system provides volumetric, interactive elements that enhance the learning experience through spatial visualization. All 3D content follows a consistent stylized aesthetic blending low-poly foundations with organic, hand-crafted details.

### 8.2 General 3D Style Rules

| Property | Value |
|----------|-------|
| Technique | Low-poly meets stylized |
| Polygon count | Optimal (5K-50K tris per scene) |
| Materials | Clean, matte with subtle specular |
| Lighting | Warm, three-point lighting |
| Shadows | Soft, colored, 30-50% opacity |
| Textures | Flat colors with subtle gradients |
| Outlines | Optional, thin (1-2px), same hue darker |
| Anti-aliasing | Required |

### 8.3 Knowledge Tree

The knowledge tree is a central 3D visualization representing the user's learning journey. It grows and branches as knowledge is acquired.

#### Visual Style

| Element | Style | Details |
|---------|-------|---------|
| Trunk | Organic, textured | Warm brown, gentle twist, root structure visible |
| Branches | Organic branching | Smooth curves, parent-child hierarchy, bifurcating |
| Leaves | Particle system | Semi-transparent, gently animated, color-coded by topic |
| Nodes | Geometric fruits | Small polygonal shapes at branch tips |
| Glow | Ambient | Soft green-teal glow at 20-30% opacity |

#### Branching Rules

| Hierarchy | Branch Thickness | Color | Opacity | Detail Level |
|-----------|-----------------|-------|---------|--------------|
| Root | 8-12 units | Deep brown #4A3528 | 100% | High (bark texture) |
| Main branch | 4-8 units | Brown #6B5040 | 100% | Medium |
| Secondary | 2-4 units | Warm brown #8A6B58 | 90% | Low |
| Twig | 1-2 units | Light brown #A88878 | 80% | Minimal |
| Leaf stem | 0.5-1 units | Sage #7A9E8A | 70% | Minimal |

#### Particle Leaf System

| Property | Value |
|----------|-------|
| Particle count | 200-500 (scalable) |
| Size | 0.5-3 units (varied) |
| Color | Mode-specific (per topic area) |
| Animation | Gentle sway, occasional drift |
| Lifetime | Continuous (respawn on fall) |
| Opacity | 40-80% (randomized) |

### 8.4 Memory Garden

The memory garden is a 3D space where learned concepts bloom as stylized plants. Each plant represents a mastered topic.

#### Visual Style

| Element | Style | Details |
|---------|-------|---------|
| Stems | Curved, organic | Smooth bezier curves, gentle green |
| Leaves | Stylized, simple | 3-5 petal shapes, translucent edges |
| Petals | Semi-translucent | Gradient opacity, soft color transitions |
| Bloom cores | Geometric | Small polygonal centers, glowing |
| Ground plane | Soft, organic | Gentle undulation, atmospheric shadow |
| Floating particles | Ambient | Small light points, slow drift |

#### Plant Types

| Plant Type | Structure | Color Palette | Bloom Style |
|------------|-----------|---------------|-------------|
| Wisdom flower | Tall stem, single large bloom | Gold, amber, warm white | Layered petals, luminous |
| Knowledge fern | Multiple fronds | Sage, teal, emerald | Small clustered blooms |
| Concept succulent | Rounded, geometric | Mint, coral, soft purple | Star-shaped bloom |
| Discovery vine | Winding, spreading | Warm green, gold tips | Small bell-shaped |
| Insight blossom | Floating elements | Indigo, soft pink, white | Ethereal, transparent |

#### Growth Stages

| Stage | Visual State | Animation |
|-------|-------------|-----------|
| Seed (new) | Small geometric seed, faint glow | Gentle pulse (2s cycle) |
| Sprout (learning) | Small stem, 1-2 leaves | Slow growth animation |
| Budding (practicing) | Developing bloom, more leaves | Occasional sway |
| Blooming (mastered) | Full flower, particles | Gentle float, particle emission |
| Flourishing (expert) | Multiple blooms, enhanced glow | Rich particle effects |

### 8.5 Avatar

User avatars are 3D representations that blend low-poly construction with stylized, expressive features.

#### Visual Style

| Feature | Style | Details |
|---------|-------|---------|
| Head shape | Low-poly with smooth subdivision | Soft angular, defined jawline |
| Face | Stylized, expressive | Large eyes, gentle features, warm expression |
| Hair | Stylized shapes | Geometric blocks with soft edges |
| Clothing | Simplified, flowing | Modest, scholarly attire (customizable) |
| Accessories | Optional | Glasses, scarf, turban, hijab, book |
| Skin | Flat with subtle gradient | Diverse skin tones (warm palette) |

#### Avatar Polygon Budget

| Component | Polygon Count |
|-----------|--------------|
| Head | 800-1200 tris |
| Hair | 400-800 tris |
| Torso | 600-1000 tris |
| Arms | 300-500 tris (each) |
| Accessories | 200-400 tris |
| Total | 2500-4000 tris |

#### Expression Range

| Expression | Visual Changes |
|------------|---------------|
| Neutral | Relaxed mouth, level brows |
| Happy | Raised cheeks, slight smile, bright eyes |
| Curious | Raised brows, slight head tilt |
| Focused | Slight frown, narrowed eyes |
| Surprised | Raised brows, open mouth, wider eyes |
| Thoughtful | Slight frown, looking upward |

### 8.6 Knowledge Spirit

The knowledge spirit is an ethereal 3D entity that guides users through their learning journey.

#### Visual Style

| Property | Value |
|----------|-------|
| Form | Geometric but fluid |
| Material | Semi-transparent, glowing |
| Colors | Indigo, soft blue, warm white |
| Size | Variable (3-8 units) |
| Particles | Emitting constantly |
| Glow | 40-60% opacity bloom |

#### Spirit Forms

| Form | Shape | Occasion |
|------|-------|----------|
| Orb | Smooth sphere, pulsing | Default presence |
| Flame | Tapered, flowing upward | AI thought processing |
| Ring | Torus shape, rotating | Knowledge connection |
| Portal | Flat circular, particles | Transition, loading |
| Constellation | Connected points | Network visualization |

### 8.7 Objects

#### Style Guidelines

| Property | Value |
|----------|-------|
| Construction | Clean, minimal geometry |
| Edges | Beveled (0.5-2 units) |
| Materials | Matte with 10-20% roughness |
| Colors | From design system palette |
| Scale | Context-appropriate, human-relative |

#### Object Library

| Object | Style | Details |
|--------|-------|---------|
| Book | Rectangular with spine curve | Pages visible, subtle cover detail |
| Pen | Cylindrical with pointed tip | Minimal, elegant |
| Lamp | Angular base, translucent shade | Warm light emission |
| Globe | Sphere with subtle continent relief | Floating in stand |
| Scroll | Cylindrical with unfurling option | Parchment texture |
| Compass | Flat circular, geometric | Clean dial, delicate needle |
| Frame | Rectangular, thin bevel | Ornate corners optional |
| Cup | Tapered cylinder | Warm drink inside |

---

## 9. Glassmorphism Rules

### 9.1 Overview

Glassmorphism is a visual effect that creates the appearance of frosted glass through a combination of background blur, transparency, and subtle borders.

### 9.2 Glassmorphism Properties

| Property | Value | Notes |
|----------|-------|-------|
| Background blur | 20-40px | Gaussian blur applied to backdrop |
| Background transparency | 60-80% opaque | Leaves 20-40% of background visible |
| Border | 0.5-1px solid | White at 20-30% opacity |
| Shadow | Soft, colored | Matches background hue at 10-20% opacity |
| Border radius | 12-24px | Consistent with component radius tokens |

### 9.3 Glassmorphism Formula

glass-component {
  background: color-mix(in srgb, var(--surface) 70%, transparent);
  backdrop-filter: blur(24px);
  -webkit-backdrop-filter: blur(24px);
  border: 0.5px solid rgba(255, 255, 255, 0.25);
  box-shadow: 0 8px 32px rgba(var(--shadow-color), 0.12);
}

#### Light Theme Glass Values

| Component | Background | Opacity | Blur | Border |
|-----------|------------|---------|------|--------|
| Card | neutral-100 | 75% | 24px | 0.5px white at 25% |
| Sheet | neutral-50 | 70% | 32px | 0.5px white at 20% |
| Modal | neutral-100 | 80% | 40px | 1px white at 25% |
| Nav bar | neutral-50 | 70% | 20px | 0.5px white at 20% |
| Tooltip | neutral-100 | 85% | 16px | 0.5px white at 30% |
| Menu | neutral-100 | 80% | 24px | 0.5px white at 25% |

#### Dark Theme Glass Values

| Component | Background | Opacity | Blur | Border |
|-----------|------------|---------|------|--------|
| Card | neutral-900 | 75% | 24px | 0.5px white at 15% |
| Sheet | neutral-950 | 70% | 32px | 0.5px white at 12% |
| Modal | neutral-900 | 80% | 40px | 1px white at 15% |
| Nav bar | neutral-950 | 70% | 20px | 0.5px white at 12% |
| Tooltip | neutral-900 | 85% | 16px | 0.5px white at 18% |
| Menu | neutral-900 | 80% | 24px | 0.5px white at 15% |

### 9.4 When to Use Glassmorphism

#### Appropriate Uses

| Component | Rationale |
|-----------|-----------|
| Cards (overlay) | Shows content beneath while maintaining focus |
| Navigation bars | Creates visual separation from scrolling content |
| Bottom sheets | Indicates layered content hierarchy |
| Modals | Focuses attention on modal content while showing context |
| Context menus | Indicates temporary surface above content |
| Tooltips | Provides information without fully obscuring content |
| Side panels | Keeps main content visible while providing secondary UI |

#### Inappropriate Uses

| Component | Rationale |
|-----------|-----------|
| Text-heavy areas | Blur reduces text readability |
| Input fields | Transparency creates contrast issues |
| Data tables | Transparency reduces data scanability |
| Long-form content | Reading requires high contrast, no blur |
| Error states | Critical feedback needs full opacity |
| Primary CTAs | Action elements need maximum visibility |
| Mobile body content | Performance cost, readability loss |

### 9.5 Performance Considerations

| Factor | Impact | Mitigation |
|--------|--------|------------|
| Backdrop blur | GPU-intensive | Limit to 1-2 glass elements per view |
| Transparency | GPU compositing | Prefer over solid surfaces, not video |
| Mobile devices | Battery, frame rate | Reduce blur to 16px max on mobile |
| Scrolling content | Redraw cost | Use sparingly on scrollable elements |
| Stacked glass | Multiplied cost | Never stack glass elements |

#### Mobile Performance Rules

1. Maximum one glass element per view on mobile (0-767px).
2. Reduce blur to 16px maximum on mobile.
3. Increase opacity to 80-85% on mobile.
4. Avoid glass on scrolling backgrounds.
5. Provide solid background fallback for browsers without backdrop-filter support.

### 9.6 Glassmorphism Accessibility

1. Ensure text meets contrast against the glass background color, not the blurred content behind it.
2. Focus rings inside glassmorphic containers must use 2px solid focus ring in primary-tone-2.
3. When prefers-reduced-motion: reduce is active, keep the glass effect static.
4. Provide option to disable transparency effects system-wide.

---

## 10. Blur Rules

### 10.1 Overview

Blur effects create visual depth, direct attention, and support glassmorphic elements. The system defines four blur levels.

### 10.2 Blur Levels

| Level | Token | Value | When to Use |
|-------|-------|-------|-------------|
| Subtle | blur-subtle | 8px | Background texture softening, image hover effects |
| Standard | blur-standard | 20px | Glassmorphism card backgrounds, nav bar backgrounds |
| Heavy | blur-heavy | 40px | Modal backgrounds, sheet backgrounds, overlay backgrounds |
| Maximum | blur-maximum | 80px | Focus backgrounds, onboarding overlays, dream sequences |

### 10.3 Backdrop Blur vs Element Blur

#### Backdrop Blur (backdrop-filter)

| Property | Behavior | Use Case |
|----------|----------|----------|
| Blurs content behind | Yes | Glassmorphism, context preservation |
| Render cost | Higher | GPU compositing required |
| Accessibility | Better (content adaptive) | Content behind remains visible as context |

#### Element Blur (filter: blur())

| Property | Behavior | Use Case |
|----------|----------|----------|
| Blurs the element itself | Yes | Loading placeholders, inactive content |
| Render cost | Lower | Single element processing |
| Accessibility | Requires alternative | Content behind remains sharp |

### 10.4 Blur + Transparency Combinations

| Effect Type | Blur | Transparency | Best For |
|-------------|------|-------------|----------|
| Light glass | 20px | 30% transparency | Cards on light backgrounds |
| Heavy glass | 40px | 20% transparency | Modals on complex backgrounds |
| Subtle texture | 8px | 50% transparency | Background overlays |
| Focus tunnel | 80px | 40% transparency | Full-screen focus mode |
| Dream state | 60px | 50% transparency | Transitions, loading states |
| Depth fog | 24px | 60% transparency | Parallax background layers |

### 10.5 Performance Budgets

| Device Class | Max Blur Value | Max Blurred Elements | Recommendation |
|-------------|---------------|----------------------|----------------|
| Desktop (GPU) | 80px | 5 | Full effects enabled |
| Desktop (integrated) | 40px | 3 | Reduce heavy blur |
| Tablet (modern) | 40px | 3 | Full effects, limited instances |
| Tablet (older) | 20px | 2 | Standard effects only |
| Mobile (high-end) | 20px | 2 | Standard blur only |
| Mobile (mid-range) | 16px | 1 | Subtle blur only |
| Mobile (low-end) | 8px | 1 | Minimal blur |
| Accessibility mode | 0px | 0 | No blur |

### 10.6 Accessibility: Reduced Motion

When prefers-reduced-motion: reduce is enabled:
1. Disable heavy blur. Maximum blur capped at 20px.
2. Remove blur transitions. Do not animate blur intensity changes.
3. Keep static blur acceptable but reduce to standard level.
4. Blur-in/out transitions become opacity fades (200ms).
5. Provide blur-off mode for users to disable all blur effects.

---

## 11. Shadow Rules

### 11.1 Overview

Shadow creates visual hierarchy by establishing elevation relationships between elements. Shadows are never pure black — they are always tinted by the ambient color.

### 11.2 Elevation Levels

| Level | Token | Offset X | Offset Y | Blur Radius | Spread | Opacity (Light) | Opacity (Dark) |
|-------|-------|----------|----------|-------------|--------|-----------------|----------------|
| 0 | elevation-0 | 0 | 0 | 0 | 0 | 0% | 0% |
| 1 | elevation-1 | 0 | 1px | 2px | 0 | 6% | 10% |
| 2 | elevation-2 | 0 | 2px | 4px | -1px | 8% | 12% |
| 3 | elevation-3 | 0 | 4px | 8px | -2px | 10% | 15% |
| 4 | elevation-4 | 0 | 8px | 16px | -4px | 12% | 18% |
| 5 | elevation-5 | 0 | 12px | 24px | -6px | 14% | 20% |

#### Shadow Multi-Layer Technique

Use two-layer shadows for natural-looking depth:
- Ambient shadow: Large blur, low opacity
- Direct shadow: Small blur, higher opacity

### 11.2 Light Theme Shadows

| Level | Light Theme Shadow |
|-------|-------------------|
| 0 | none |
| 1 | 0 1px 2px rgba(28, 18, 8, 0.06) |
| 2 | 0 2px 4px -1px rgba(28, 18, 8, 0.08), 0 0 1px rgba(28, 18, 8, 0.04) |
| 3 | 0 4px 8px -2px rgba(28, 18, 8, 0.10), 0 0 1px rgba(28, 18, 8, 0.06) |
| 4 | 0 8px 16px -4px rgba(28, 18, 8, 0.12), 0 0 2px rgba(28, 18, 8, 0.08) |
| 5 | 0 12px 24px -6px rgba(28, 18, 8, 0.14), 0 0 3px rgba(28, 18, 8, 0.10) |

### 11.3 Dark Theme Shadows

| Level | Dark Theme Shadow |
|-------|-------------------|
| 0 | none |
| 1 | 0 1px 2px rgba(45, 139, 120, 0.15) |
| 2 | 0 2px 4px -1px rgba(45, 139, 120, 0.18), 0 0 1px rgba(45, 139, 120, 0.08) |
| 3 | 0 4px 8px -2px rgba(45, 139, 120, 0.20), 0 0 1px rgba(45, 139, 120, 0.10) |
| 4 | 0 8px 16px -4px rgba(45, 139, 120, 0.22), 0 0 2px rgba(45, 139, 120, 0.12) |
| 5 | 0 12px 24px -6px rgba(45, 139, 120, 0.25), 0 0 3px rgba(45, 139, 120, 0.14) |

### 11.4 Shadow + Blur Combinations

| Effect | Shadow Level | Blur Level | Use Case |
|--------|-------------|------------|----------|
| Soft card | 2 | Subtle (8px) | Standard cards, panels |
| Elevated card | 3 | Standard (20px) | Featured cards, hover states |
| Floating element | 3 | Standard (20px) | Dragged items, active states |
| Overlay panel | 4 | Heavy (40px) | Side panels, drawers |
| Modal backdrop | 4 | Heavy (40px) | Maximum separation |
| Toast/Notification | 5 | Heavy (40px) | Highest visibility |

### 11.5 Shadow Color by Component

| Component | Light Theme Shadow Color | Dark Theme Shadow Color |
|-----------|--------------------------|------------------------|
| Cards | Warm black (28,18,8) | Primary teal (45,139,120) |
| Buttons | Warm black (28,18,8) | Primary teal (45,139,120) |
| Modals | Warm black (28,18,8) | Primary teal (45,139,120) |
| Navigation | Primary teal (45,107,94) | Primary teal (61,158,136) |
| Tooltips | Warm black (28,18,8) | Primary teal (45,139,120) |
| Media | Primary teal (45,107,94) | Primary teal (61,158,136) |

---

## 12. Elevation Rules

### 12.1 Overview

Elevation defines the visual layering of the interface through z-index mapping, shadow levels, and visual depth. The system uses a base-10 elevation scale.

### 12.2 Elevation Layers

| Layer | Token | Z-Index | Shadow Level | Usage |
|-------|-------|---------|-------------|-------|
| 0 | layer-background | 0 | 0 | Page backgrounds, canvas |
| 10 | layer-content | 10 | 1 | Text content, images, non-interactive elements |
| 20 | layer-card | 20 | 2 | Cards, panels, content containers |
| 30 | layer-navigation | 30 | 2 | Navigation bars, sidebars, tab bars |
| 40 | layer-sticky | 40 | 2 | Sticky headers, sticky sidebars |
| 50 | layer-overlay | 50 | 3 | Overlay backgrounds, scrims |
| 60 | layer-modal | 60 | 4 | Modal dialogs, alert dialogs |
| 70 | layer-sheet | 70 | 4 | Bottom sheets, side sheets |
| 80 | layer-toast | 80 | 5 | Toasts, snackbars, notifications |
| 90 | layer-tooltip | 90 | 3 | Tooltips, popovers, dropdowns |
| 100 | layer-fullscreen | 100 | 5 | Full-screen loading, full-screen media |

### 12.3 Z-Index Mapping

Layer 100 — Fullscreen / Loading (z: 100)
Layer 90 — Tooltips / Popovers / Dropdowns (z: 90)
Layer 80 — Toast / Snackbar / Notifications (z: 80)
Layer 70 — Sheets (bottom, side) (z: 70)
Layer 60 — Modals / Dialogs (z: 60)
Layer 50 — Overlays / Scrims (z: 50)
Layer 40 — Sticky headers / sticky sidebars (z: 40)
Layer 30 — Navigation / Sidebars / Tab bars (z: 30)
Layer 20 — Cards / Panels / Containers (z: 20)
Layer 10 — Content / Text / Images (z: 10)
Layer 0 — Background / Canvas (z: 0)

### 12.4 Elevation Rules

#### General Rules

1. Elevation is relative to container. Elements within a modal stack relative to each other.
2. Portal increases elevation. Elements via portal use the appropriate layer token.
3. Maximum one overlay at a time. Only one layer-50-or-above element visible at a time.
4. Focus management. Moving to higher elevation must move focus to that layer.
5. Animation on elevation change. Animate shadow transition over 200ms.

#### Elevation Change Rules

| Interaction | From Layer | To Layer | Transition | Duration |
|-------------|-----------|----------|------------|----------|
| Card hover | 20 | 25 | Shadow increase | 200ms |
| Button hover | 10 | 15 | Shadow increase | 200ms |
| Drag start | 20 | 30 | Shadow increase + scale | 100ms |
| Dropdown open | 30 | 90 | Portal + shadow | 200ms |
| Modal open | — | 60 | Scale-up + overlay | 300ms |
| Toast enter | — | 80 | Slide in + shadow | 300ms |

#### Component Elevation Assignments

| Component | Base Layer | Active/Hover Layer | Elevated Layer |
|-----------|-----------|-------------------|----------------|
| Card | 20 | 25 | 25 |
| Button | 10 | 15 | — |
| Input | 10 | 15 (focus) | — |
| Navigation bar | 30 | — | — |
| Sidebar | 30 | — | — |
| Dropdown | 90 | — | — |
| Tooltip | 90 | — | — |
| Popover | 90 | — | — |
| Modal | 60 | — | — |
| Sheet | 70 | — | — |
| Toast | 80 | — | — |
| Loading spinner | 100 | — | — |

---

## 13. Corner Radius Rules

### 13.1 Overview

Corner radius contributes to the visual personality of the interface. The system uses a range of radius values from sharp to fully circular, assigned consistently based on component type and size.

### 13.2 Radius Scale

| Token | Value | Character | When to Use |
|-------|-------|-----------|-------------|
| radius-none | 0px | Sharp | Media containers, image grids, tables, dividers |
| radius-subtle | 4px | Gentle | Input fields, small components, checkboxes, tags |
| radius-soft | 8px | Standard | Buttons, navigation items, list items |
| radius-rounded | 12px | Card-default | Cards, panels, containers, modals (default) |
| radius-pill | 24px | Friendly | Chips, badges, pills, toggle switches |
| radius-circular | 50% | Circular | Avatars, icon buttons, circular indicators |

### 13.3 Component Radius Assignments

| Component | Radius Token | Value | Rationale |
|-----------|-------------|-------|-----------|
| Card | radius-rounded | 12px | Defines content containers clearly |
| Card (compact) | radius-soft | 8px | Smaller cards need tighter radius |
| Button (default) | radius-soft | 8px | Approachable but defined |
| Button (pill) | radius-pill | 24px | Accent calls-to-action |
| Button (icon) | radius-soft | 8px | Consistent with standard buttons |
| Input field | radius-subtle | 4px | Subtle definition, feels editable |
| Textarea | radius-subtle | 4px | Consistent with inputs |
| Select | radius-subtle | 4px | Consistent with inputs |
| Modal | radius-rounded | 16px | Larger default for elevated surfaces |
| Sheet (top) | radius-rounded | 20px (top only) | Soft entry from bottom |
| Sheet (bottom) | radius-none | 0px | Edge-to-edge bottom |
| Chip | radius-pill | 24px | Friendly, compact |
| Badge | radius-pill | 24px | Compact indicator |
| Avatar | radius-circular | 50% | Standard circular crop |
| Tooltip | radius-subtle | 4px | Small, unobtrusive |
| Toast | radius-soft | 8px | Noticeable but not dominant |
| Navigation item | radius-soft | 8px | Consistent with buttons |
| Media container | radius-soft | 8px | Framed media |
| Image (thumbnail) | radius-subtle | 4px | Subtle framing |
| Progress bar | radius-pill | 24px | Smooth, friendly |
| Toggle | radius-pill | 24px | Pill-shaped track |
| Accordion | radius-soft | 8px | Section grouping |
| Table | radius-none | 0px | Data needs sharp edges |
| Divider | radius-none | 0px | Line element |

### 13.4 Radius Nesting Rules

When elements are nested, inner radius should be smaller than outer radius by 4px (minimum 0px).

### 13.5 Radius Consistency Rules

1. Never mix radii in the same component group.
2. Radius scales with size. Larger components use slightly larger radius values.
3. Edge alignment. Components touching screen edges have radius-none on the touching edge.
4. Sheets have asymmetric radius: rounded top corners (20px), sharp bottom corners (0px).
5. Dialogs inside sheets have smaller radius than the sheet.

---

## 14. Animation Rules

### 14.1 Overview

Animation serves a functional purpose — guiding attention, providing feedback, and creating continuity between states. Every animation must have a clear purpose.

### 14.2 Duration Mapping

| Token | Duration | Character | When to Use |
|-------|----------|-----------|-------------|
| duration-micro | 100ms | Instant | State changes, toggle, checkbox, switch, press feedback |
| duration-quick | 200ms | Fast | Hover effects, focus rings, small element transitions |
| duration-standard | 300ms | Standard | Most transitions, element appear/disappear, panel slides |
| duration-deliberate | 500ms | Noticeable | Screen transitions, modal appear, significant state changes |
| duration-slow | 800ms | Deliberate | Theme switching, onboarding sequences, achievement reveals |

### 14.3 Easing Functions

#### Standard Easing: cubic-bezier(0.2, 0, 0, 1)

Use for: Most UI transitions, element entry/exit, panel slides.

#### Decelerate Easing: cubic-bezier(0, 0, 0.2, 1)

Use for: Elements entering the screen, content appearing, reveal animations.

#### Accelerate Easing: cubic-bezier(0.4, 0, 1, 1)

Use for: Elements leaving the screen, dismissal, disappearance.

#### Spring Animation: spring(tension: 300, friction: 25, mass: 1)

| Parameter | Default | Gentle Spring | Snappy Spring | Loose Spring |
|-----------|---------|--------------|---------------|--------------|
| Tension | 300 | 180 | 450 | 120 |
| Friction | 25 | 35 | 20 | 15 |
| Mass | 1 | 1 | 0.8 | 1.2 |
| Behavior | Standard bounce | Soft settle | Quick snap | Loose wobble |

### 14.4 Stagger Animation

| Token | Delay Per Item | Total Items | Total Duration |
|-------|---------------|-------------|----------------|
| stagger-fast | 20ms | 10 | 200ms + animation |
| stagger-standard | 30ms | 10 | 300ms + animation |
| stagger-slow | 50ms | 10 | 500ms + animation |

#### Stagger Direction

| Direction | Items animate from | Best for |
|-----------|-------------------|----------|
| Top-down | First to last | Lists, vertically stacked content |
| Bottom-up | Last to first | Chat messages, notifications |
| Center-out | Middle to edges | Grids, galleries, card arrangements |
| Left-right | Left to right | Horizontal lists, table rows |

### 14.5 Shared Element Transition

| Property | Value |
|----------|-------|
| Duration | 350ms |
| Easing | Standard cubic-bezier(0.2, 0, 0, 1) |
| Properties | Transform, opacity, border-radius |

#### Shared Element Rules

1. Identify shared element. Both source and destination must reference the same element.
2. Match geometry. Transition between source position/size and destination position/size.
3. Cross-fade content. Content inside the shared element cross-fades (300ms).
4. Border radius morphing. Animate radius changes.
5. Exit and enter elements. Elements unique to source or destination fade out/in at midpoint.

### 14.6 Animation Principles

#### Purpose-Driven Animation

1. Every animation must serve a function.
2. Functional categories:
   - Feedback: Confirm user action (100-200ms)
   - Focus: Guide attention to important changes (200-300ms)
   - Continuity: Maintain context during transitions (300-500ms)
   - Spatial: Indicate spatial relationships (200-300ms)
   - Hierarchy: Show importance through timing priority

#### Performance

1. Prefer transform and opacity. These are GPU-accelerated.
2. Use will-change sparingly.
3. 60fps target. All animations must maintain 60 frames per second.
4. Avoid animating width, height, top, left, margin, padding.

#### Accessibility: Reduced Motion

When prefers-reduced-motion: reduce is active:
1. Reduce all durations by 50%.
2. Disable parallax.
3. Disable spring animations. Replace with standard easing.
4. Remove scale transforms. Keep opacity-based transitions only.
5. Disable stagger delays. All items animate simultaneously.
6. Remove shared element transitions. Fall back to simple cross-fade.

---

## 15. Motion Rules

### 15.1 Overview

Motion tells the story of where an element comes from, what it is, and where it goes. Consistent motion patterns help users build a mental model of the interface's spatial layout.

### 15.2 Directional Consistency

#### Motion Direction by Context

| Context | Entry Direction | Exit Direction | Rationale |
|---------|----------------|----------------|-----------|
| Drill-down navigation | Right | Left | New content pushes previous aside |
| Go-back navigation | Left | Right | Returning to previous content |
| New page (top-level) | Fade in | Fade out | No directional relationship |
| Modal | Scale from center | Scale to center | Focus on modal content |
| Bottom sheet | Slide up | Slide down | Affordance: sheet from bottom edge |
| Menu | Expand from trigger | Collapse to trigger | Spatial relationship to trigger |
| Tooltip | Fade up from anchor | Fade down to anchor | Appears above content |
| Notification (toast) | Slide from top | Slide to top (or fade) | Noticeable but non-disruptive |
| Sidebar | Slide from left | Slide to left | Content revealed from edge |
| Tab content | Slide horizontally | Slide opposite | Content continuity |
| Accordion | Expand downward | Collapse upward | Natural vertical expansion |
| Search results | Fade in | Fade out | Content replacement |

#### Spatial Continuity Rules

1. Same direction, same relationship.
2. Opposite direction for opposite actions. Back is reverse of forward.
3. Never collide directions.
4. Gesture matching. Touch interactions should match animation direction.

### 15.3 Motion Without Purpose

#### Prohibited Motion

| Motion Type | Why Not |
|-------------|---------|
| Continuous idle animation (spinners excepted) | Distracting, reduces focus |
| Decorative bounce/tilt | No functional purpose |
| Random direction movement | Creates mental confusion |
| Motion that crosses reading content | Disrupts reading flow |
| Parallax on mobile | Battery drain, motion sickness risk |
| Auto-scrolling content | Removes user control |

#### Acceptable Ambient Motion

| Motion Type | Constraint | Purpose |
|-------------|-----------|---------|
| Knowledge tree leaf drift | Subtle, max 10px range | Indicates organic life |
| Loading skeleton pulse | 50% opacity range | Shows activity |
| Background gradient animation | 30s+ cycle | Atmospheric depth |
| Particle effects | Subtle, 5-10 particles | Achievement celebration |

### 15.4 Motion Physics

| Property | Value | Application |
|----------|-------|-------------|
| Friction | 0.85 (per step) | Deceleration on gesture-based motions |
| Tension | 0.5 (per step) | Snap-back animations |
| Mass | 1.0 (normal) | Default spring animations |
| Velocity tracking | Required | Gesture-driven animations follow finger speed |

### 15.5 Motion and Accessibility

1. Respect OS reduced motion. prefers-reduced-motion: reduce disables all non-essential motion.
2. Provide static alternatives. Any information conveyed through motion must be available statically.
3. No flashing content. No animation may flash more than 3 times per second (WCAG 2.3.1).
4. No motion-induced nausea. Avoid parallax, excessive scaling, and disorienting rotation.

---

## 16. Transition Rules

### 16.1 Overview

Transitions define how the interface changes between states and screens. The transition system provides consistent patterns for navigation, content changes, and state shifts.

### 16.2 Screen Transitions

#### Push Transition (Drill Down)

| Element | Animation | Duration | Easing |
|---------|-----------|----------|--------|
| Previous screen exit | Slide left (100%) | 300ms | Accelerate |
| New screen enter | Slide from right (0% to 0) | 300ms | Decelerate |

#### Pop Transition (Go Back)

| Element | Animation | Duration | Easing |
|---------|-----------|----------|--------|
| Current screen exit | Slide right (0 to 100%) | 300ms | Accelerate |
| Previous screen enter | Slide from left (-33% to 0) | 300ms | Decelerate |

#### Modal Transition (Present/Dismiss)

| State | Element | Animation | Duration | Easing |
|-------|---------|-----------|----------|--------|
| Present | Modal | Scale from 0.95 to 1 + fade in | 300ms | Decelerate |
| Present | Backdrop | Fade in (0 to 50% opacity) | 200ms | Standard |
| Dismiss | Modal | Scale from 1 to 0.95 + fade out | 200ms | Accelerate |
| Dismiss | Backdrop | Fade out | 150ms | Standard |

#### Sheet Transition (Slide Up/Down)

| State | Element | Animation | Duration | Easing |
|-------|---------|-----------|----------|--------|
| Present | Sheet | Slide up (100% to 0) | 400ms | Decelerate |
| Present | Backdrop | Fade in | 300ms | Standard |
| Dismiss | Sheet | Slide down (0 to 100%) | 300ms | Accelerate |
| Dismiss | Backdrop | Fade out | 200ms | Standard |

#### Tab Content Transition

| Direction | Animation | Duration | Easing |
|-----------|-----------|----------|--------|
| Tab left to right | Slide from right | 250ms | Standard |
| Tab right to left | Slide from left | 250ms | Standard |
| Same tab reselected | No animation | — | — |

#### Fade Transition (Context Change)

| State | Animation | Duration | Easing |
|-------|-----------|----------|--------|
| Enter | Fade in (0 to 1) | 300ms | Decelerate |
| Exit | Fade out (1 to 0) | 200ms | Accelerate |

### 16.3 Transition Lifecycle

The transition lifecycle follows: Exit Previous → Shared Element → Enter New

#### Exit Phase
1. Non-shared elements exit with fade or directional exit (200ms, accelerate easing).
2. Shared element is identified and geometry captured.

#### Shared Element Phase
1. Shared element transitions from source geometry to target geometry (350ms, standard easing).
2. Content within shared element cross-fades.

#### Enter Phase
1. Non-shared elements enter with fade or directional entrance (300ms, decelerate easing).
2. Stagger applied to list items (30ms per item delay).

### 16.4 Gesture-Driven Transitions

| Gesture | Transition | Behavior |
|---------|-----------|----------|
| Swipe left | Push next screen | Follows finger, snaps at 30% threshold |
| Swipe right | Pop previous screen | Follows finger, snaps at 30% threshold |
| Drag down | Dismiss sheet | Follows finger, snaps at 25% threshold |
| Tap | Trigger action | Immediate feedback (100ms), then standard transition |
| Long press | Preview | Haptic at 500ms, preview after 700ms |

#### Gesture Thresholds

| Threshold | Value | Action |
|-----------|-------|--------|
| Minimum displacement | 20px | Gesture must exceed this to register |
| Velocity minimum | 0.3 (px/ms) | Below this, gesture treated as tap |
| Completion threshold | 30% | Gesture snaps to completion at 30% of target |
| Cancel threshold | < 30% | Returns to starting position with spring |

### 16.5 Transition Best Practices

1. Directional consistency. Users should always know where content comes from.
2. No blind transitions. Never animate to/from invisible state without explanation.
3. Content continuity. Shared element transitions maintain spatial awareness.
4. Performance budget. Total transition time should not exceed 500ms.
5. Reduced motion. All transitions collapse to fade-only at 150ms.

---

## 17. Depth Rules

### 17.1 Overview

Visual depth creates a sense of three-dimensional space in the two-dimensional interface through shadow, blur, scale, opacity, and color working together.

### 17.2 Depth Techniques

#### Shadow (Elevation)

| Depth Level | Token | Visual Effect | Distance |
|-------------|-------|---------------|----------|
| Ground | elevation-0 | No shadow | Surface level |
| Slightly raised | elevation-1 | Subtle shadow | 1px offset |
| Elevated | elevation-2 | Clear shadow | 2px offset |
| Floating | elevation-3 | Soft shadow | 4px offset |
| High | elevation-4 | Broad shadow | 8px offset |
| Maximum | elevation-5 | Wide, soft shadow | 12px offset |

#### Blur (Glassmorphism)

| Depth Level | Blur | Visual Effect |
|-------------|------|---------------|
| Surface | 0px | No depth through blur |
| Near | 8px | Subtle separation |
| Medium | 20px | Noticeable depth |
| Deep | 40px | Strong separation |
| Maximum | 80px | Complete focus separation |

#### Scale (Parallax)

| Depth Level | Scale | Movement Speed | Visual Effect |
|-------------|-------|---------------|---------------|
| Far background | 0.8x | 50% of scroll | Slow moving, distant |
| Background | 0.9x | 70% of scroll | Gentle parallax |
| Content | 1.0x | 100% of scroll | Normal speed |
| Foreground | 1.05x | 120% of scroll | Faster movement, closer |
| Controls | 1.1x | 130% of scroll | Nearest to viewer |

#### Opacity (Distance Fog)

| Depth Level | Opacity | Visual Effect |
|-------------|---------|---------------|
| Nearest (controls) | 100% | Fully visible |
| Foreground | 95% | Nearly full visibility |
| Midground | 85% | Visible with atmospheric effect |
| Background | 70% | Noticeable distance fog |
| Far background | 50% | Strong atmospheric perspective |

#### Color (Atmospheric Perspective)

| Depth Level | Saturation | Lightness | Temperature Shift |
|-------------|-----------|-----------|-------------------|
| Nearest | 100% | Normal | None |
| Midground | 85% | +5% | Slightly cooler |
| Background | 70% | +10% | Cooler (add blue) |
| Far background | 55% | +15% | Significantly cooler |

### 17.3 Depth Layer Map

Layer 5: Controls (closest) — z-index: 90-100, shadow: 5, blur: 40px
Layer 4: Overlays — z-index: 50-80, shadow: 4, blur: 20px
Layer 3: Cards/Controls — z-index: 20-40, shadow: 2-3, blur: 0-8px
Layer 2: Content — z-index: 10, shadow: 1, blur: 0px
Layer 1: Background (furthest) — z-index: 0, shadow: 0, blur: 0px

### 17.4 Knowledge Tree Depth

| Mastery Level | Visual Depth | Scale | Glow | Shadows |
|--------------|-------------|-------|------|---------|
| Unexplored | Background | 0.7x | None | None |
| Exposed | Mid-background | 0.8x | Subtle (10%) | Soft |
| Learning | Midground | 0.9x | Low (20%) | Medium |
| Practiced | Foreground | 1.0x | Medium (40%) | Strong |
| Mastered | Nearest | 1.1x | Strong (60%) | Maximum |

### 17.5 Depth Rules Summary

1. No more than 3 depth layers in a single view for clarity.
2. Each layer must have at least two visual cues distinguishing it.
3. Do not use all five depth techniques simultaneously — pick 2-3 per view.
4. Depth should guide attention to the most important element.
5. Mobile views use fewer depth layers (max 2) for performance.

---

## 18. Contrast Rules

### 18.1 Overview

Contrast ensures readability and accessibility for all users. The system targets WCAG 2.1 AA as a minimum, with AAA as the target for primary reading content.

### 18.2 Text Contrast Requirements

| Context | WCAG AA Minimum | WCAG AAA Minimum | Design System Target |
|---------|----------------|-----------------|-------------------|
| Normal text (< 18px) | 4.5:1 | 7:1 | 7:1 for body text |
| Large text (>= 18px) | 3:1 | 4.5:1 | 4.5:1 |
| UI components | 3:1 | — | 3:1 minimum |

### 18.3 UI Element Contrast

| Element | Minimum Contrast | Target Contrast |
|---------|-----------------|----------------|
| Button text vs background | 4.5:1 | 5:1+ |
| Icon vs background | 3:1 | 4.5:1 |
| Input border vs background | 3:1 | 3:1 |
| Focus indicator | 3:1 vs adjacent | 4.5:1 |
| Disabled text | 3:1 | 3:1 |
| Placeholder text | 3:1 | 4.5:1 |

### 18.4 Graphical Contrast

| Context | Requirement |
|---------|-------------|
| Adjacent colors differ by | Minimum 30 degrees hue OR 20% lightness |
| Information conveyance | Never rely on color alone — always pair with text, icon, or pattern |
| Data visualization | Minimum 3:1 between adjacent data series |
| Chart elements | Pattern/fill + color for all data series |

### 18.5 Dark Theme Contrast Adjustments

In dark theme, contrast ratios are slightly reduced for dark-adapted eyes:
- Body text: Light target 10:1 → Dark target 8.5:1
- Secondary text: Light target 5.8:1 → Dark target 5.5:1
- Disabled text: Light target 3.5:1 → Dark target 3:1

### 18.6 Color Independence

1. Never use color as the sole means of conveying information.
2. Status indicators must use icon + color + text.
3. Link text must have underline or icon indicator in addition to color.
4. Form validation must use icon + color + helper text.
5. Chart data must use pattern/texture + color for each series.

---

## 19. Accessibility Rules

### 19.1 Touch Targets

| Target Type | Minimum Size | Recommended Size | Notes |
|-------------|-------------|-----------------|-------|
| Interactive controls | 44x44pt | 48x48pt | Buttons, links, icons, chips |
| Form inputs | 44px height | 48px height | Text inputs, selects, textareas |
| Slider handles | 44x44pt | 48x48pt | Range sliders, volume controls |
| Toggle switches | 44x44pt | 48x48pt | iOS-style toggle |
| Close/dismiss buttons | 44x44pt | 48x48pt | Modal close, sheet dismiss |

### 19.2 Focus Indicators

| Property | Value | Notes |
|----------|-------|-------|
| Style | Outline | Never use outline: none without replacement |
| Width | 2px | Minimum 2px solid |
| Color | primary-tone-2 | High contrast against any background |
| Offset | 4px | Clear gap between element and focus ring |
| Border radius | Matches element | Follow element's border radius |
| Animation | 200ms fade in | Do not animate with motion |

### 19.3 Focus Order

1. Logical focus order matching visual reading order (LTR for Latin, RTL for Arabic).
2. Use tabindex=0 for interactive elements, tabindex=-1 for programmatic focus. Never use positive tabindex.
3. Skip to content links must be the first focusable element on every page.
4. Focus must cycle within open modals/sheets. Escape returns focus to trigger.
5. When a panel/modal closes, focus returns to the element that triggered it.

### 19.4 ARIA Labels

| Element Type | ARIA Requirement | Example |
|-------------|-----------------|---------|
| Icon-only button | aria-label | aria-label="Close panel" |
| Navigation landmark | aria-label | aria-label="Main navigation" |
| Search region | role="search" | role="search" |
| Progress indicator | aria-valuenow, aria-valuemin, aria-valuemax | aria-valuenow="60" |
| Error message | aria-live="polite" or "assertive" | aria-live="assertive" |
| Expanded state | aria-expanded | aria-expanded="true" |
| Tab panel | aria-controls, aria-selected | aria-selected="true" |

### 19.5 Reduced Motion

| OS Setting | System Response |
|-----------|----------------|
| prefers-reduced-motion: reduce | Disable non-essential animations, reduce durations by 50%, disable parallax |
| prefers-color-scheme: light | Apply light theme |
| prefers-color-scheme: dark | Apply dark theme |
| prefers-contrast: more | Increase contrast by one shade level |
| prefers-contrast: less | Decrease contrast by one shade level |

### 19.6 Additional Accessibility Rules

1. Semantic HTML. Use correct heading hierarchy (h1-h6), landmark elements, and list elements.
2. Alt text. All images must have alt text. Decorative images use alt="".
3. Captioning. All video content must have captions. All audio content must have transcripts.
4. Keyboard navigation. All functionality must be available via keyboard alone.
5. Screen reader announcements. Use aria-live regions for dynamic content updates.
6. Language declaration. HTML lang attribute must match page content language.
7. Arabic RTL support. The interface must properly mirror for right-to-left reading direction.
8. Zoom support. Content must not break when zoomed to 200%.
9. Error identification. Errors must be clearly identified with text descriptions.
10. Context-sensitive help available for complex interactions.

---

## 20. Dark Theme

### 20.1 Overview

The dark theme is designed for comfortable extended use in low-light environments. It uses warm dark tones that reduce eye strain and blue light exposure while maintaining visual hierarchy.

### 20.2 Background and Surface Colors

| Surface Level | Token | Color | Usage |
|--------------|-------|-------|-------|
| Base background | neutral-50 (dark) | #1A1A1E | Page background, canvas |
| Surface 1 | neutral-100 (dark) | #1F1F24 | Card backgrounds, panel backgrounds |
| Surface 2 | neutral-150 (dark) | #24242A | Raised cards, sticky headers |
| Surface 3 | neutral-200 (dark) | #2A2A30 | Elevated surfaces, tooltip backgrounds |
| Navigation | neutral-100 (dark) | #1F1F24 | Sidebar, nav bars |
| Modal backdrop | — | 50% opacity dark | Overlay background |

#### Surface Hierarchy Rules

1. Surfaces get lighter as they elevate (opposite of light theme).
2. Use minimum 2 visible surface levels to establish hierarchy. Recommended 3 levels.
3. Surface differences: Minimum 5% lightness difference between adjacent levels.

### 20.3 Text Colors

| Text Level | Token | Opacity | Color | Usage |
|-----------|-------|---------|-------|-------|
| Primary (high) | neutral-850 (dark) | 87% | #CECED8 | Headings, primary labels, body text |
| Secondary (medium) | neutral-700 (dark) | 60% | #9E9EA8 | Secondary text, metadata, captions |
| Tertiary (low) | neutral-550 (dark) | 38% | #6C6C78 | Placeholder, disabled text, hints |
| Link | primary-tone-2 | 100% | #3D9E88 | Links, interactive text |

### 20.4 Color Desaturation

Colors in dark theme are desaturated by approximately 15% compared to light theme equivalents. This prevents color from appearing overly bright on dark-adapted eyes.

| Color | Light Theme | Dark Theme (desaturated) | Desaturation |
|-------|-------------|-------------------------|--------------|
| Primary | #2D6B5E | #3D9E88 | -15% saturation |
| Secondary | #B8892E | #D4A843 | -12% saturation |
| Success | #1D6B40 | #38B86E | -15% saturation |
| Error | #8C1A1A | #D94444 | -10% saturation |
| Warning | #8C5C1A | #D99C38 | -15% saturation |

### 20.5 Reduced Blue Light

The dark theme deliberately reduces blue light by:
1. Using warm-toned neutrals with amber undertones instead of cool gray.
2. Shifting primary blue tones toward teal rather than pure blue.
3. Using amber-gold for highlights instead of cool white.

### 20.6 No Pure Black

Pure black (#000000) is never used as a background. Minimum brightness for dark backgrounds: L = 10% (#1A1A1E).

### 20.7 Elevated Surfaces

In dark theme, elevated surfaces are lighter than their base (opposite of light theme).

| Elevation | Light Theme | Dark Theme | Rationale |
|-----------|-------------|------------|-----------|
| Base | neutral-50 | neutral-950 (dark) | Darkest background |
| Card | neutral-100 | neutral-900 (dark) | Lighter than base |
| Raised card | neutral-150 | neutral-850 (dark) | Even lighter |
| Tooltip | neutral-200 | neutral-800 (dark) | Lightest surface |

---

## 21. Light Theme

### 21.1 Overview

The light theme is designed for optimal readability in well-lit environments. It uses warm whites with subtle warmth to reduce eye strain while maintaining crisp, high-contrast text.

### 21.2 Background and Surface Colors

| Surface Level | Token | Color | Usage |
|--------------|-------|-------|-------|
| Base background | neutral-50 | #FAFAF5 | Page background, canvas |
| Surface 1 | neutral-100 | #F5F5F0 | Card backgrounds, panel backgrounds |
| Surface 2 | neutral-150 | #F0EFEB | Raised cards, sticky headers |
| Surface 3 | neutral-200 | #E8E7E3 | Elevated surfaces, tooltip backgrounds |
| Navigation | neutral-50/100 | Various | Sidebar, nav bars |

### 21.3 Text Colors

| Text Level | Token | Color | Usage |
|-----------|-------|-------|-------|
| Primary (high) | neutral-900 | #1F1E1D | Headings, primary labels, body text |
| Secondary (medium) | neutral-600 | #6C6B68 | Secondary text, metadata, captions |
| Tertiary (low) | neutral-400 | #B8B7B3 | Placeholder, disabled text, hints |
| Link | primary-tone-2 | #2D6B5E | Links, interactive text |

### 21.4 Color Saturation

In light theme, colors are used at full saturation, creating vibrant accents against warm neutral backgrounds.

### 21.5 Shadow Visibility

Shadows in light theme are clearly visible using warm-tinted black at defined opacity levels.

### 21.6 Maximum Contrast for Reading

Body text achieves a contrast ratio of 10:1 or higher against the base background, exceeding WCAG AAA.

---

## 22. Theme Switching

### 22.1 Overview

The theme switching system provides smooth, accessible transitions between light and dark themes while respecting user preferences, OS settings, and accessibility requirements.

### 22.2 Switch Transition

| Property | Value | Notes |
|----------|-------|-------|
| Duration | 500ms | duration-deliberate |
| Easing | cubic-bezier(0.2, 0, 0, 1) | Standard easing |
| Properties | All color properties, background, box-shadow |
| Method | CSS custom property swap | No repaint flash |

### 22.3 OS Setting Respect

1. On first visit, read prefers-color-scheme from OS/browser.
2. Apply the corresponding theme (light or dark).
3. Listen for prefers-color-scheme changes and switch dynamically.
4. Allow manual override that persists across sessions.

### 22.4 Manual Override

| Feature | Behavior |
|---------|----------|
| Theme toggle | Available in settings and quick-access menu |
| Persistence | Saved to localStorage or user preferences |
| Sync | Option to re-sync with OS setting |
| Icon | Sun/moon icon pair (animated morph) |
| Transition | 500ms smooth transition |

### 22.5 Scheduled Switching

| Schedule Type | Behavior |
|--------------|----------|
| Sunset trigger | Switches to dark at local sunset |
| Sunrise trigger | Switches to light at local sunrise |
| Custom time | User-defined switching times |
| Smart | Based on ambient light sensor (where available) |

### 22.6 Session Persistence

| Storage | Key | Value |
|---------|-----|-------|
| localStorage | theme-preference | "light", "dark", "system" |
| localStorage | theme-schedule | JSON schedule object |
| Cookie | theme-preference | Same as localStorage |

### 22.7 Per-Mode Theming

| Mode | Independent Theme | Use Case |
|------|-----------------|----------|
| Story mode | Yes | Reading in low light |
| Quiz mode | Yes | Focus mode with reduced distractions |
| Dark mode (system) | Default | All modes follow system |
| Accessibility | Override | High contrast mode overrides theme |

### 22.8 Accessibility Override

| Scenario | Behavior |
|----------|----------|
| High contrast mode | Forces high contrast theme regardless of current theme |
| Reduced motion | Disables theme transition animation |
| Color blindness mode | Adjusts theme colors for color vision deficiency |

---

## 23. Brand Identity

### 23.1 Brand Overview

The brand represents a modern, scholarly, and warm approach to digital learning, bridging traditional Islamic scholarship with contemporary educational technology.

### 23.2 Brand Name and Logo Usage

| Element | Specification |
|---------|--------------|
| Logo type | Wordmark with optional icon mark |
| Logo orientation | Horizontal (preferred), stacked (alternative) |
| Minimum size | 32px height (digital), 0.5in (print) |

#### Logo Clear Space

The clear space around the logo must equal the height of the logo mark on all sides.

### 23.3 Logo Versions

| Version | Usage | File Format |
|---------|-------|-------------|
| Full logo (color) | Primary usage, light backgrounds | SVG, PNG |
| Full logo (reversed) | Dark backgrounds | SVG, PNG |
| Full logo (monochrome) | Single-color applications | SVG |
| Icon only (color) | Favicon, app icon, avatar | SVG, PNG |
| Icon only (monochrome) | Watermarks, stamps | SVG |
| Icon only (reversed) | Dark backgrounds | SVG, PNG |

### 23.4 Brand Color Palette

| Color | Token | Hex (Light) | Usage |
|-------|-------|-------------|-------|
| Primary | brand-primary | #2D6B5E | Logo primary, main brand color |
| Secondary | brand-secondary | #B8892E | Logo accent, secondary brand color |
| Dark | brand-dark | #1A1A1E | Logo on light backgrounds |
| Light | brand-light | #FAFAF5 | Logo on dark backgrounds |
| Accent | brand-accent | #D4A043 | Brand highlights, special editions |

### 23.5 Brand Typography

| Context | Typeface | Weight | Usage |
|---------|----------|--------|-------|
| Logo (Latin) | Source Serif 4 | 700 | Brand wordmark |
| Logo (Arabic) | Noto Naskh Arabic | 700 | Brand wordmark (Arabic) |
| Headlines | Source Serif 4 | 600 | Marketing, landing pages |
| Body | Inter | 400 | UI copy, documentation |
| Tagline | Inter | 500 | Brand tagline |

### 23.6 Brand Voice

| Attribute | Description |
|-----------|-------------|
| Wise | Speaks with authority and depth of knowledge |
| Warm | Approachable, inviting, never cold or clinical |
| Scholarly | Precise, well-researched, thoughtful |
| Modern | Forward-looking, embracing technology |
| Inclusive | Welcoming to all backgrounds and perspectives |
| Encouraging | Supportive, growth-oriented, positive |

#### Voice Do's and Don'ts

| Do | Don't |
|----|-------|
| Use clear, precise language | Use jargon or overly complex terms |
| Be encouraging and supportive | Be condescending or dismissive |
| Acknowledge the learner's effort | Focus only on outcomes |
| Celebrate progress and discovery | Emphasize what the user doesn't know |
| Use metaphor and storytelling | Use dry, technical descriptions only |

### 23.7 Tagline Usage

| Context | Tagline | Placement |
|---------|---------|-----------|
| Logo (full) | Knowledge Illuminated | Below logo wordmark |
| App store | Learn with Wisdom | Alongside app icon |
| Marketing | Discover the Light of Knowledge | Hero sections |
| Social media | Illuminate Your Mind | Profile bios |

### 23.8 Brand Patterns

| Pattern | Description | Usage |
|---------|-------------|-------|
| Geometric | 8-pointed stars and polygons | Background textures, section dividers |
| Arabesque | Flowing, symmetrical curves | Decorative borders, frame corners |
| Knowledge lines | Connected node patterns | Learning path backgrounds |
| Illuminated | Gold-accented borders | Achievement cards, certificates |

#### Pattern Usage Rules

1. Use at 5-15% opacity for background texture.
2. Scale proportionally to container size.
3. Never use as primary visual element. Patterns support, not dominate.
4. Ensure patterns do not interfere with text readability.
5. Use sparingly - one pattern type per view maximum.

---

## 24. Design Tokens

### 24.1 Overview

Design tokens are the atomic values that represent the design system's visual properties. They provide a single source of truth across platforms (web, iOS, Android) and ensure consistency.

### 24.2 Token Naming Convention

Tokens follow the naming convention: category-type-variant-state

#### Categories

| Prefix | Category | Examples |
|--------|----------|---------|
| color | Colors | color-primary-base-light |
| typography | Typography | typography-size-body |
| spacing | Spacing | spacing-scale-md |
| radius | Corner radius | radius-component-card |
| shadow | Shadows | shadow-elevation-2 |
| animation | Animation | animation-duration-standard |
| opacity | Opacity | opacity-surface-overlay |

#### Naming Rules

1. All tokens use kebab-case with hyphens separating words.
2. Categories come first, then type, then variant, then state.
3. Avoid abbreviations except for well-known terms (md, lg, xl).
4. Token names should be human-readable and self-documenting.

### 24.3 Token Values by Category

#### Color Tokens

| Token | Value (Light) | Value (Dark) | CSS Custom Property |
|-------|--------------|--------------|---------------------|
| color-primary-shade-1 | #E8F4F0 | #1A2E28 | --color-primary-shade-1 |
| color-primary-shade-2 | #C5E3DA | #2D4D42 | --color-primary-shade-2 |
| color-primary-shade-3 | #9DCFC0 | #4A7A6A | --color-primary-shade-3 |
| color-primary-tone-1 | #3E8E7A | #5DAF9A | --color-primary-tone-1 |
| color-primary-tone-2 | #2D6B5E | #3D9E88 | --color-primary-tone-2 |
| color-primary-tone-3 | #1F4E44 | #2D8A75 | --color-primary-tone-3 |
| color-primary-base-1 | #153A32 | #1F7A68 | --color-primary-base-1 |
| color-primary-base-2 | #0E2822 | #1A6B5A | --color-primary-base-2 |
| color-primary-base-3 | #081712 | #155C4D | --color-primary-base-3 |
| color-secondary-shade-1 | #FFF8EC | #2E281A | --color-secondary-shade-1 |
| color-secondary-shade-2 | #FFEDC2 | #4D4228 | --color-secondary-shade-2 |
| color-secondary-shade-3 | #FFE099 | #7A6930 | --color-secondary-shade-3 |
| color-secondary-tone-1 | #D4A043 | #E8B84A | --color-secondary-tone-1 |
| color-secondary-tone-2 | #B8892E | #D4A843 | --color-secondary-tone-2 |
| color-secondary-tone-3 | #9A7222 | #BF9838 | --color-secondary-tone-3 |
| color-secondary-base-1 | #7A5A18 | #A88328 | --color-secondary-base-1 |
| color-secondary-base-2 | #5C4312 | #8C6E20 | --color-secondary-base-2 |
| color-secondary-base-3 | #3D2C0C | #705818 | --color-secondary-base-3 |
| color-neutral-50 | #FAFAF5 | #1A1A1E | --color-neutral-50 |
| color-neutral-100 | #F5F5F0 | #1F1F24 | --color-neutral-100 |
| color-neutral-200 | #E8E7E3 | #2A2A30 | --color-neutral-200 |
| color-neutral-300 | #D4D3CF | #383840 | --color-neutral-300 |
| color-neutral-400 | #B8B7B3 | #484850 | --color-neutral-400 |
| color-neutral-500 | #949390 | #5E5E68 | --color-neutral-500 |
| color-neutral-600 | #6C6B68 | #7C7C88 | --color-neutral-600 |
| color-neutral-700 | #4C4B48 | #9E9EA8 | --color-neutral-700 |
| color-neutral-800 | #343330 | #BEBEC8 | --color-neutral-800 |
| color-neutral-900 | #1F1E1D | #DEDEE4 | --color-neutral-900 |
| color-neutral-950 | #141413 | #EEEEF0 | --color-neutral-950 |
| color-success-shade-1 | #E6F5EB | #1A2E22 | --color-success-shade-1 |
| color-success-tone-1 | #3BA86C | #4DCB82 | --color-success-tone-1 |
| color-success-base-1 | #1D6B40 | #38B86E | --color-success-base-1 |
| color-warning-shade-1 | #FFF5E6 | #2E2618 | --color-warning-shade-1 |
| color-warning-tone-1 | #D98C2E | #E8A84A | --color-warning-tone-1 |
| color-warning-base-1 | #8C5C1A | #D99C38 | --color-warning-base-1 |
| color-error-shade-1 | #FCECEC | #2E1A1A | --color-error-shade-1 |
| color-error-tone-1 | #D94A4A | #E85C5C | --color-error-tone-1 |
| color-error-base-1 | #8C1A1A | #D94444 | --color-error-base-1 |
| color-info-shade-1 | #E8F0FC | #1A2230 | --color-info-shade-1 |
| color-info-tone-1 | #3A7BD5 | #5C9CE8 | --color-info-tone-1 |
| color-info-base-1 | #1A4D8C | #4088D4 | --color-info-base-1 |

#### Typography Tokens

| Token | Value | CSS Custom Property |
|-------|-------|---------------------|
| typography-font-family-arabic | "Noto Naskh Arabic", "Amiri", serif | --typography-font-family-arabic |
| typography-font-family-latin | "Source Serif 4", Georgia, serif | --typography-font-family-latin |
| typography-font-family-ui | "Inter", -apple-system, sans-serif | --typography-font-family-ui |
| typography-font-family-mono | "JetBrains Mono", "Fira Code", monospace | --typography-font-family-mono |
| typography-size-10 | 10px | --typography-size-10 |
| typography-size-12 | 12px | --typography-size-12 |
| typography-size-14 | 14px | --typography-size-14 |
| typography-size-16 | 16px | --typography-size-16 |
| typography-size-18 | 18px | --typography-size-18 |
| typography-size-20 | 20px | --typography-size-20 |
| typography-size-24 | 24px | --typography-size-24 |
| typography-size-30 | 30px | --typography-size-30 |
| typography-size-36 | 36px | --typography-size-36 |
| typography-size-48 | 48px | --typography-size-48 |
| typography-size-60 | 60px | --typography-size-60 |
| typography-size-72 | 72px | --typography-size-72 |
| typography-weight-light | 300 | --typography-weight-light |
| typography-weight-regular | 400 | --typography-weight-regular |
| typography-weight-medium | 500 | --typography-weight-medium |
| typography-weight-semibold | 600 | --typography-weight-semibold |
| typography-weight-bold | 700 | --typography-weight-bold |
| typography-leading-tight | 1.2 | --typography-leading-tight |
| typography-leading-normal | 1.5 | --typography-leading-normal |
| typography-leading-relaxed | 1.8 | --typography-leading-relaxed |
| typography-tracking-tight | -0.02em | --typography-tracking-tight |
| typography-tracking-normal | 0em | --typography-tracking-normal |
| typography-tracking-wide | 0.05em | --typography-tracking-wide |
| typography-tracking-wider | 0.1em | --typography-tracking-wider |

#### Spacing Tokens

| Token | Value | CSS Custom Property |
|-------|-------|---------------------|
| spacing-scale-2 | 2px | --spacing-scale-2 |
| spacing-scale-4 | 4px | --spacing-scale-4 |
| spacing-scale-8 | 8px | --spacing-scale-8 |
| spacing-scale-12 | 12px | --spacing-scale-12 |
| spacing-scale-16 | 16px | --spacing-scale-16 |
| spacing-scale-20 | 20px | --spacing-scale-20 |
| spacing-scale-24 | 24px | --spacing-scale-24 |
| spacing-scale-32 | 32px | --spacing-scale-32 |
| spacing-scale-40 | 40px | --spacing-scale-40 |
| spacing-scale-48 | 48px | --spacing-scale-48 |
| spacing-scale-64 | 64px | --spacing-scale-64 |
| spacing-scale-80 | 80px | --spacing-scale-80 |
| spacing-scale-96 | 96px | --spacing-scale-96 |
| spacing-scale-128 | 128px | --spacing-scale-128 |

#### Radius Tokens

| Token | Value | CSS Custom Property |
|-------|-------|---------------------|
| radius-none | 0px | --radius-none |
| radius-subtle | 4px | --radius-subtle |
| radius-soft | 8px | --radius-soft |
| radius-rounded | 12px | --radius-rounded |
| radius-pill | 24px | --radius-pill |
| radius-circular | 50% | --radius-circular |

#### Shadow Tokens

| Token | Value | CSS Custom Property |
|-------|-------|---------------------|
| shadow-elevation-0 | none | --shadow-elevation-0 |
| shadow-elevation-1 | 0 1px 2px rgba(28,18,8,0.06) | --shadow-elevation-1 |
| shadow-elevation-2 | 0 2px 4px -1px rgba(28,18,8,0.08), 0 0 1px rgba(28,18,8,0.04) | --shadow-elevation-2 |
| shadow-elevation-3 | 0 4px 8px -2px rgba(28,18,8,0.10), 0 0 1px rgba(28,18,8,0.06) | --shadow-elevation-3 |
| shadow-elevation-4 | 0 8px 16px -4px rgba(28,18,8,0.12), 0 0 2px rgba(28,18,8,0.08) | --shadow-elevation-4 |
| shadow-elevation-5 | 0 12px 24px -6px rgba(28,18,8,0.14), 0 0 3px rgba(28,18,8,0.10) | --shadow-elevation-5 |

#### Animation Tokens

| Token | Value | CSS Custom Property |
|-------|-------|---------------------|
| animation-duration-micro | 100ms | --animation-duration-micro |
| animation-duration-quick | 200ms | --animation-duration-quick |
| animation-duration-standard | 300ms | --animation-duration-standard |
| animation-duration-deliberate | 500ms | --animation-duration-deliberate |
| animation-duration-slow | 800ms | --animation-duration-slow |
| animation-easing-standard | cubic-bezier(0.2, 0, 0, 1) | --animation-easing-standard |
| animation-easing-decelerate | cubic-bezier(0, 0, 0.2, 1) | --animation-easing-decelerate |
| animation-easing-accelerate | cubic-bezier(0.4, 0, 1, 1) | --animation-easing-accelerate |

#### Opacity Tokens

| Token | Value | CSS Custom Property |
|-------|-------|---------------------|
| opacity-surface-overlay | 0.5 | --opacity-surface-overlay |
| opacity-text-primary | 0.87 | --opacity-text-primary |
| opacity-text-secondary | 0.60 | --opacity-text-secondary |
| opacity-text-tertiary | 0.38 | --opacity-text-tertiary |
| opacity-glass-default | 0.75 | --opacity-glass-default |
| opacity-glass-heavy | 0.85 | --opacity-glass-heavy |
| opacity-disabled | 0.50 | --opacity-disabled |

### 24.4 Platform Implementation

#### Web (CSS Custom Properties)

```css
:root {
  --color-primary-tone-2: #2D6B5E;
  --typography-size-body: 16px;
  --spacing-scale-md: 16px;
  --radius-component-card: 12px;
  --shadow-elevation-2: 0 2px 4px -1px rgba(28,18,8,0.08);
  --animation-duration-standard: 300ms;
}

[data-theme="dark"] {
  --color-primary-tone-2: #3D9E88;
  --shadow-elevation-2: 0 2px 4px -1px rgba(45,139,120,0.18);
}
```

#### iOS (xcassets)

Tokens are defined in JSON format within Asset Catalog color sets, text styles, and dimension definitions.

#### Android (Compose)

Tokens are defined as Kotlin objects:

```kotlin
object DesignTokens {
    val PrimaryTone2 = Color(0xFF2D6B5E)
    val SpacingMd = 16.dp
    val RadiusCard = 12.dp
}
```

### 24.5 Token Documentation

Each token must be documented with:
1. Token name and path
2. Light theme value
3. Dark theme value (where applicable)
4. Description of usage
5. Related tokens
6. Figma component reference
7. Status (active, deprecated, experimental)

### 24.6 Token Governance

1. Tokens are the single source of truth. Hard-coded values are prohibited.
2. Token changes must be reviewed by the Design Systems team.
3. Deprecated tokens are marked with a deprecation notice and migration path.
4. New tokens follow the naming convention and are added to the token library.
5. Token audits are performed quarterly to ensure consistency.

### 24.7 Example: --color-primary-base-light

| Property | Value |
|----------|-------|
| Token | --color-primary-base-light |
| Category | color |
| Type | primary |
| Variant | base |
| State | light |
| Light Theme | #153A32 |
| Dark Theme | #1F7A68 |
| Description | Deep primary color for headings and high emphasis elements |
| Related | --color-primary-tone-2, --color-primary-shade-1 |
| Figma | /colors/primary/base-light |
| Status | active |

---

## Document Information

**Document:** Design System v2.0.0
**File:** 02_Design_System.md
**Author:** Design Systems Team
**Last Updated:** 2026-07-10
**Review Cycle:** Quarterly
**Next Review:** 2026-10-10

### Document Conventions

- Tokens are referenced as --token-name throughout.
- Color values use hex notation with documented light and dark variants.
- All measurements are in pixels (px) unless otherwise noted.
- Accessibility references are to WCAG 2.1 AA/AAA.

### Related Documents

- 01_Brand_Guidelines.md — Brand identity and logo usage
- 03_Component_Library.md — Component specifications and behavior
- 04_Pattern_Library.md — Common UI patterns and workflows
- 05_Accessibility_Guidelines.md — Detailed accessibility requirements
- 06_Platform_Guidelines.md — Platform-specific implementation notes

---

*End of Design System Document*
