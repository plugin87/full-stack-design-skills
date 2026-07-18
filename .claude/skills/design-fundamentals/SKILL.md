---
name: design-fundamentals
description: Master universal design principles for UI, web, and applications. Use this skill whenever you need to apply color theory, typography, layout systems, spacing, visual hierarchy, or contrast guidelines. Triggers include design critiques, UI specs, accessibility concerns, design system foundations, responsive layout problems, and questions about why something doesn't look right. Essential for both designers and frontend developers building pixel-perfect, accessible interfaces.
---

# Design Fundamentals

A comprehensive guide to universal design principles applicable across all digital products, frameworks, and platforms.

## Core Principles Overview

Design fundamentals are the bedrock of every digital product. They transcend frameworks, tools, and trends. Whether you're designing in Figma, building components in React, or architecting a design system, these principles apply universally.

**The five pillars:**

1. **Color** — How color creates meaning, contrast, hierarchy, and mood
2. **Typography** — Font selection, sizing, weight, and spacing for readability
3. **Layout & Grid** — Structural systems that organize information
4. **Spacing & Scale** — Visual breathing room and proportion
5. **Contrast & Hierarchy** — Guiding attention and creating relationships

---

## 1. Color Theory & Application

### Color Fundamentals

**Color Dimensions** (HSL model):
- **Hue** (0-360°) — The color itself (red, blue, yellow, etc.)
- **Saturation** (0-100%) — Color intensity (gray is 0%, pure color is 100%)
- **Lightness** (0-100%) — Brightness (0% = black, 100% = white, 50% = pure color)

Web designers typically work in:
- **HEX** — #FF5733 (shorthand: #F57)
- **RGB/RGBA** — rgb(255, 87, 51) with opacity
- **HSL** — hsl(10, 100%, 60%)

### Color Relationships

**Complementary** — Colors opposite on the color wheel. High contrast, use for CTAs and accents.

**Analogous** — Colors adjacent on the wheel. Harmonious, calming, use for neutral palettes.

**Triadic** — Three colors evenly spaced. Vibrant, balanced, use for playful interfaces.

**Monochromatic** — One hue in different saturations/lightnesses. Elegant, professional, use for coherent systems.

### Accessibility & Contrast

**WCAG Standards** (minimum contrast ratios):
- **Normal text** — 4.5:1 (AA) or 7:1 (AAA)
- **Large text** (18pt+ or 14pt+ bold) — 3:1 (AA) or 4.5:1 (AAA)
- **UI Components & borders** — 3:1 (AA)

**Checking contrast:**
- Use WebAIM Contrast Checker, Accessible Colors, or browser DevTools
- Never rely on color alone; pair with icons, patterns, or text
- Dark mode requires separate contrast checks — don't assume 1:1 translations

### Color in UI Design

**Semantic colors:**
- **Success** — Green (#10B981, #22C55E)
- **Warning** — Amber/Yellow (#F59E0B, #FBBF24)
- **Danger/Error** — Red (#EF4444, #F87171)
- **Info** — Blue (#3B82F6, #60A5FA)
- **Neutral** — Gray (for backgrounds, dividers, secondary text)

**Color application best practices:**
- Use a consistent color palette (8-12 primary colors max)
- Create color ramps — light tints, mid tones, dark shades for each color
- Reserve accent colors for high-priority actions (buttons, links, focus states)
- Use neutral grays for supporting elements (borders, backgrounds, disabled states)
- Test colors in context — a color that works in isolation may clash in the layout
- Always test with colorblind vision (protanopia, deuteranopia, tritanopia) using tools like Coblis or plugins

---

## 2. Typography & Text Design

### Font Selection

**Serif vs. Sans-serif:**

**Serif** (Times New Roman, Georgia, EB Garamond):
- Formal, editorial, traditional
- Good for body copy in long-form content
- Can feel dated if overused

**Sans-serif** (Helvetica, Inter, System UI, -apple-system):
- Modern, clean, neutral
- Excellent for UI and web
- System fonts are fastest (no load time)

**Best practices:**
- Use max 2-3 typefaces per product (1 for UI, 1 for body, optionally 1 decorative)
- Pair serif + sans-serif, or two sans-serifs with different weights
- Choose web-safe fonts or properly load custom fonts (WOFF2)
- Avoid "fun" decorative fonts for core UI — they hurt readability

**Recommended web typefaces:**
- **System stack:** -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif
- **Friendly sans:** Inter, Poppins, Open Sans
- **Geometric sans:** Montserrat, Raleway
- **Serif for body:** Merriweather, Lora, Crimson Text

### Font Sizing & Scale

**Establishing a scale:**

A typographic scale is a set of predefined font sizes that relate to each other through a mathematical ratio. Common ratios: 1.125 (minor second), 1.25 (major third), 1.5 (perfect fifth).

**Example scale (base 16px, ratio 1.25):**
- Xs: 12px (caption, metadata)
- Sm: 14px (helper text, small UI labels)
- Base: 16px (body text, default)
- Md: 20px (section headings, emphasis)
- Lg: 25px (page title)
- Xl: 31px (hero/banner heading)

**Rules for sizes:**
- Use no more than 6 distinct sizes in a product
- Don't go below 12px for body text (accessibility issue)
- Headings should be at least 1.5× larger than body text
- Maintain consistency — use the same size for the same content type across the product

### Line Height (Leading)

The vertical space between lines in a paragraph.

**Guidelines:**
- **Headlines** — 1.1–1.2 (tight, controlled)
- **Body text** — 1.5–1.7 (loose, readable)
- **Lists/code** — 1.6–1.8 (very loose, scannable)

**Formula:** line-height = font-size × multiplier. For 16px body: 16px × 1.6 = 25.6px line-height.

### Font Weight & Emphasis

**Weight ladder** (most web fonts offer 400, 500, 700; premium fonts offer 300-900):
- **300 (Light)** — Use sparingly; harder to read, good for subtle UI
- **400 (Regular)** — Default body text weight
- **500 (Medium)** — UI labels, navigation, emphasis
- **600 (Semibold)** — Subheadings, strong emphasis
- **700 (Bold)** — Headings, important text
- **800+ (Black)** — Rare; only for dramatic effect

**Never use multiple weights for the same content type.** Stick to 400 for body, 500 for UI, 700 for headings.

### Letter Spacing (Tracking)

The horizontal space between characters.

**Guidelines:**
- **-0.02em** — Tighter, for large headings (cool, dramatic)
- **0em** — Default, neutral
- **0.02em–0.05em** — Looser, for all-caps text, small sizes
- **0.1em+** — Very loose, decorative headers only

**Rule:** Smaller text needs no tracking adjustment. Larger headings (40px+) may look better tighter.

### Paragraph Spacing (Margins)

Space between paragraphs and elements.

**Guidelines:**
- Space below a heading = 0.5–1× the heading size
- Space below body paragraph = 1–1.5× the body size
- Space between sections = 2–3× the body size

**Example (16px body):**
- Heading bottom margin: 8–16px
- Paragraph bottom margin: 16–24px
- Section margin: 32–48px

---

## 3. Layout & Grid Systems

### Grid Foundations

A layout grid divides the screen into columns and rows, creating structure and alignment.

**12-column grid** (most common):
- Desktop (1200px): 12 columns, 20px gutter, ~80px per column
- Tablet (768px): 8 columns, 16px gutter
- Mobile (360px): 4 columns, 12px gutter

**8-point grid (soft grid):**
All measurements are multiples of 8px: 8, 16, 24, 32, 40, 48, 56, 64, 72...
- Makes spacing consistent
- Simplifies CSS (no fractional pixels)
- Scales well across devices

### Flexbox vs. Grid

**CSS Flexbox:**
- 1D layout (rows or columns)
- Great for component-level layouts (buttons, cards, navigation)
- Responsive by default with flex-wrap

**CSS Grid:**
- 2D layout (rows and columns simultaneously)
- Great for page-level layouts (hero, content, sidebar)
- More explicit, harder to learn, more powerful

**Use Flexbox when:** wrapping, alignment within a container, responsive columns by content.
**Use Grid when:** two-dimensional layout, template areas, explicit positioning.

### Responsive Breakpoints

Define breakpoints for different screen sizes. Mobile-first approach means starting at 360px and expanding up.

**Standard breakpoints (Tailwind convention):**
- **Sm:** 640px (small tablets)
- **Md:** 768px (tablets)
- **Lg:** 1024px (small laptops)
- **Xl:** 1280px (desktops)
- **2xl:** 1536px (large desktops)

**When designing:** test at 360px (mobile), 768px (tablet), 1200px (desktop), and 1920px (ultrawide).

### Safe Spacing Around Content

**Padding from viewport edges:**
- Mobile: 16px or 24px
- Tablet: 24px or 32px
- Desktop: 32px to 60px (content shouldn't span full width above 1200px)

**Max content width:** 1200px for most products (leaves ~40px margin on 1280px screen).

---

## 4. Spacing & Visual Rhythm

### Spacing Scale

Build a spacing scale in 8px increments:
- **Xs:** 4px (micro-spacing within components)
- **Sm:** 8px (small gaps)
- **Md:** 16px (standard spacing)
- **Lg:** 24px (large gaps, section separation)
- **Xl:** 32px (very large, between major sections)
- **2xl:** 48px
- **3xl:** 64px

Use consistently throughout the product — all gaps, padding, margins.

### Margin vs. Padding

**Padding** — Space inside a component, between content and its border.
**Margin** — Space outside a component, between components.

**Rule of thumb:**
- Use padding for internal breathing room
- Use margin to separate components
- Avoid margin collapse issues by using padding on parent or margin on one axis only

### Whitespace Strategy

Generous whitespace (negative space) improves:
- Readability (easier to scan)
- Focus (directs attention)
- Sophistication (feels premium, not cramped)
- Mobile performance (perceived speed is faster)

**Golden ratio for spacing:**
- Small elements: small gaps between them
- Large containers: generous gaps, centered content with wide margins
- Dense sections: medium gaps with clear grouping

---

## 5. Visual Hierarchy & Contrast

### Creating Hierarchy

Users scan pages in F-pattern or Z-pattern. Guide them through:

**Primary elements** (highest contrast, largest size):
- Main heading
- Primary CTA
- Hero image

**Secondary elements** (medium contrast, medium size):
- Subheading
- Supporting text
- Secondary CTAs

**Tertiary elements** (low contrast, small size):
- Helper text
- Disabled states
- Hints and tips

**Visual tools to create hierarchy:**
- **Size** — Larger = more important
- **Weight** — Bold = emphasis
- **Color** — Saturated/brighter = attention
- **Contrast** — High contrast = important
- **Position** — Top-left = read first (in LTR languages)
- **Proximity** — Group related items
- **Whitespace** — Isolated = focal point

### Contrast Levels

**High contrast (ratio 7:1+):**
- Primary headings
- Critical CTAs
- Focus states
- Status indicators (error, warning)

**Medium contrast (ratio 4.5:1):**
- Body text
- Secondary headings
- Normal buttons

**Low contrast (ratio 3:1):**
- Disabled states
- Placeholder text
- Subtle dividers
- Metadata

### Focus States & Interactive Feedback

**Focus indicators:**
- **Color:** Distinct color (usually primary/accent), 2px+ width
- **Outline or underline** — Don't rely on background change alone
- **Accessible:** Must be visible on all backgrounds
- **Round:** Border-radius 4-8px feels friendly

**Minimum visible size:** 3px × 3px (WCAG 2.4.7 Enhanced)

**Interactive feedback:**
- Hover: Color change or slight scale (1.05x)
- Active/Pressed: Darker color or inset shadow
- Focus: Clear outline or ring
- Disabled: Reduced opacity (50%+) or gray

---

## Common Mistakes & How to Fix Them

### Text Contrast Issues
**Problem:** Text is hard to read on a colored background.
**Solution:** Check contrast ratio (WebAIM tool). If <4.5:1, lighten background or darken text. Avoid light text on light backgrounds.

### Inconsistent Spacing
**Problem:** 16px here, 20px there, 24px somewhere else.
**Solution:** Establish an 8px scale and stick to it. Use CSS custom properties (--space-sm, --space-md, etc.) to enforce consistency.

### Poor Typography Hierarchy
**Problem:** All text looks the same size/weight.
**Solution:** Use a 1.25–1.5× scale ratio between sizes. Make headings bold (600–700 weight), body regular (400), and labels medium (500).

### Cluttered Layouts
**Problem:** Too much content with no breathing room.
**Solution:** Add whitespace. Group related items. Remove non-essential elements. Simplify.

### Color-Only Differentiation
**Problem:** Red/green to show status, but colorblind users can't tell.
**Solution:** Pair color with icons, patterns, or labels. Never use color alone for meaning.

### Bad Mobile Experience
**Problem:** Desktop layout shoehorned onto mobile.
**Solution:** Start mobile-first. Stack content vertically. Make buttons large (44px+). Reduce text volume. Test on actual devices.

---

## Reference Files & Further Reading

See the bundled resources for:
- **typography-scale-generator.md** — Calculate your perfect scale
- **color-palette-guide.md** — Build accessible color systems
- **responsive-layout-patterns.md** — Common layout structures
- **spacing-system-checklist.md** — Audit your spacing consistency
- **accessibility-checklist.md** — WCAG compliance reference

---

## Quick Audit Checklist

Use this before finalizing any design:

- [ ] Font sizes: 6 or fewer distinct sizes
- [ ] Line height: Body ≥1.5, headings ≤1.2
- [ ] Contrast ratio: 4.5:1 (body), 7:1 (headings)
- [ ] Color ramps: Light → mid → dark for each semantic color
- [ ] Spacing: All gaps are multiples of 8px
- [ ] Margin vs. padding: Consistent logic
- [ ] Breakpoints: Tested at 360, 768, 1200, 1920px
- [ ] Focus states: Clear, 2px+, accessible color
- [ ] Hierarchy: 3 visual weight levels, obvious at a glance
- [ ] Whitespace: Generous, breathing room around groups
- [ ] Mobile typography: No text below 12px, readable at arm's length
- [ ] Form labels: Associated with inputs, not just placeholder
- [ ] Links: Underlined or obvious color, not color alone

---

## When to Use This Skill

✅ **Do use this skill for:**
- Reviewing design specs for consistency
- Advising on typography choices and scale
- Color palette creation and accessibility checks
- Layout structure and responsive design review
- Fixing visual hierarchy issues
- Spacing and rhythm analysis
- Accessibility contrast concerns
- Design system foundations and principles

❌ **Don't use this skill for:**
- Tool-specific workflows (Figma shortcuts, advanced plugin work)
- Animated transitions and motion design (see responsive-universal-design)
- Interactive prototyping (see design-systems-architecture)
- Code implementation details (see css-styling-pixel-perfect)
