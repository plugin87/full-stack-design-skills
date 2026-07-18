---
name: responsive-universal-design
description: Master responsive design principles and universal design for mobile-first web and cross-device experiences. Use this skill for mobile-first strategy, responsive breakpoints, flexible layouts, touch-friendly interfaces, device testing, accessibility across devices, performance on mobile, viewport configuration, and responsive images. Triggers include: mobile-first design strategy, breakpoint planning, responsive grids and flexbox, touch targets and spacing, mobile navigation patterns, responsive typography, device testing, viewport settings, performance optimization for mobile, and cross-device design systems. Essential for UX/UI designers creating products that work seamlessly across phones, tablets, and desktops.
---

# Responsive & Universal Design

Principles and practices for designing products that work beautifully across all devices and screen sizes.

Responsive design isn't just about making things smaller on mobile—it's about rethinking the entire experience for different contexts. Mobile-first design means starting with the most constrained device (mobile) and progressively enhancing for larger screens.

---

## Core Principle: Mobile-First Design

### Why Mobile-First?

Mobile users face real constraints:
- **Small screen** (360-480px) — less visual real estate
- **Touch interaction** — larger touch targets needed (48px minimum)
- **Limited bandwidth** — fewer/smaller images
- **One-handed use** — thumb-reachable zones matter
- **On-the-go context** — simpler, more focused tasks

Designing for mobile first forces you to prioritize ruthlessly. You can't fit everything on a 375px screen, so you cut the noise. Then, for larger screens, you add back layers of complexity.

**Principle:** Start with mobile, progressively enhance for larger screens (not the reverse).

### Mobile-First Workflow

**Traditional approach (desktop-first):**
```
1. Design at 1440px (desktop)
2. Squish into 375px (mobile)
3. Things break, cut features
4. Result: Poor mobile experience
```

**Mobile-first approach:**
```
1. Design at 375px (mobile)
2. Expand to 768px (tablet)
3. Expand to 1440px (desktop)
4. Result: Intentional, layered experience
```

---

## Responsive Breakpoints

### Standard Device Breakpoints

Define breakpoints based on **design needs**, not device sizes. But here's a practical starting point:

```
Mobile (small):   320px - 479px   (phones in portrait)
Mobile (large):   480px - 639px   (larger phones, landscape)
Tablet:           640px - 1023px  (tablets in portrait)
Desktop:          1024px - 1439px (laptops, small desktops)
Desktop (large):  1440px +        (large desktops, 4K)
```

### Implementation in CSS

```css
/* Mobile-first: Base styles for mobile */
.container {
  width: 100%;
  padding: 16px;
  display: flex;
  flex-direction: column; /* Stack vertically on mobile */
}

/* Tablet: 640px and up */
@media (min-width: 640px) {
  .container {
    width: 90%;
    margin: 0 auto;
    flex-direction: row; /* Side-by-side on tablet */
  }
}

/* Desktop: 1024px and up */
@media (min-width: 1024px) {
  .container {
    width: 1200px;
    max-width: 100%;
  }
}
```

### Figma Breakpoint Setup

Create a "Responsive breakpoints" page in Figma:

```
Figma file
├── Page: Responsive Breakpoints
│   ├── Frame: Mobile (375 × 812) — iPhone 13
│   ├── Frame: Tablet (768 × 1024) — iPad
│   ├── Frame: Desktop (1440 × 900) — Laptop
│   └── Frame: Desktop Large (1920 × 1080) — Large monitor
```

Document breakpoints in cover page:
```
Breakpoints:
- Mobile: max 479px
- Tablet: 480px - 1023px
- Desktop: 1024px+

Key points:
- Images scale responsively (max-width: 100%)
- Text reflows (no fixed widths)
- Navigation changes (mobile: hamburger, desktop: top nav)
- Columns adjust (mobile: 1 col, tablet: 2 col, desktop: 3 col)
```

---

## Flexible Layouts: Grid & Flexbox

### CSS Grid for Layouts

Grid is powerful for multi-column layouts that respond to screen size.

**Example: 3-column desktop, 2-column tablet, 1-column mobile**

```css
/* Mobile: 1 column */
.grid {
  display: grid;
  grid-template-columns: 1fr; /* Single column */
  gap: 16px;
}

/* Tablet: 2 columns */
@media (min-width: 640px) {
  .grid {
    grid-template-columns: 1fr 1fr; /* Two equal columns */
  }
}

/* Desktop: 3 columns */
@media (min-width: 1024px) {
  .grid {
    grid-template-columns: 1fr 1fr 1fr; /* Three equal columns */
  }
}
```

**Or use CSS Grid auto-fit (no media queries):**

```css
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  /* Automatically adapts:
     - Mobile (< 280px): 1 column
     - Tablet (560px+): 2 columns
     - Desktop (840px+): 3 columns
  */
  gap: 16px;
}
```

### Flexbox for Components

Flexbox is ideal for flexible component layouts.

**Example: Button group that stacks on mobile**

```css
/* Mobile: Stack buttons vertically */
.button-group {
  display: flex;
  flex-direction: column; /* Vertical stack */
  gap: 8px;
}

.button {
  width: 100%; /* Full width on mobile */
}

/* Desktop: Buttons in a row */
@media (min-width: 640px) {
  .button-group {
    flex-direction: row; /* Horizontal layout */
  }

  .button {
    width: auto; /* Auto width on desktop */
    flex: 1; /* Equal width sharing */
  }
}
```

### Container Queries (Modern Approach)

Container queries let you respond to component size, not viewport size.

```css
/* Define container context */
.card-container {
  container-type: inline-size;
}

/* Respond to container width, not viewport */
.card {
  display: grid;
  grid-template-columns: 1fr;
  gap: 16px;
}

@container (min-width: 480px) {
  .card {
    grid-template-columns: 1fr 1fr;
  }
}

@container (min-width: 768px) {
  .card {
    grid-template-columns: 200px 1fr;
  }
}
```

**Benefit:** Card adapts based on its own container size, not screen size. Reusable in sidebars, full-width sections, etc.

---

## Touch-Friendly Interface Design

### Touch Target Sizes

Fingers are bigger than mouse cursors. Apple and Google recommend:

**Recommended:**
- **48px minimum** (44px for iOS, 48dp for Android)
- **54px ideal** (more comfortable)
- **8px minimum spacing** between touch targets

**Example: Button sizing**

```
❌ Too small: 32px height
- Hard to tap accurately
- Causes mis-taps

✅ Good: 44px height
- Easy to tap
- Accidental taps rare

✅ Better: 56px height
- Very easy to tap
- Comfortable spacing
```

### Touch-Friendly Layouts

**Navigation:**
```
Mobile (touch):
├── Bottom navigation bar (56px height)
├── Icons + labels
└── Thumb-reachable (bottom of screen)

Desktop (mouse):
├── Top navigation bar
├── Horizontal menu
└── Cursor-friendly
```

**Forms:**
```
Mobile (touch):
├── Large input fields (48px height)
├── Large labels (16px font)
├── Spacing: 24px between fields
└── Keyboard takes up 50% screen

Desktop (mouse):
├── Compact inputs (40px height)
├── Smaller labels (14px font)
├── Spacing: 16px between fields
```

### Avoiding Hover States on Touch

Mobile devices don't have hover. Design accordingly.

**Bad (hover-only):**
```html
<button>Learn more
  <div class="tooltip">Details</div>
</button>
```
```css
button:hover .tooltip { display: block; }
```
Result: Tooltip never shows on mobile.

**Good (mobile-friendly):**
```html
<button>Learn more</button>
<details>
  <summary>Show details</summary>
  <p>Details here...</p>
</details>
```

Or use tap-to-show patterns instead of hover reveals.

---

## Responsive Typography

### Font Size Scaling

Text needs to be readable on both small (375px) and large (1440px) screens.

**Traditional (fixed font sizes):**
```css
body { font-size: 16px; } /* Same size everywhere */
h1 { font-size: 32px; } /* Same size everywhere */
```
Result: Headings feel cramped on mobile, oversized on desktop.

**Responsive (fluid typography):**
```css
body {
  font-size: clamp(14px, 2vw, 18px);
  /* Minimum: 14px (mobile)
     Preferred: 2% of viewport width
     Maximum: 18px (desktop)
  */
}

h1 {
  font-size: clamp(24px, 5vw, 48px);
  /* Scales between 24px and 48px */
}
```

### Line Length & Readability

Optimal line length for readability: 45-75 characters.

**Mobile (375px):**
```
Padding: 16px both sides
Content width: 375 - 32 = 343px
Font size: 16px
Characters per line: ~60 (good)
```

**Desktop (1440px):**
```
Padding: 40px both sides
Content width: 1440 - 80 = 1360px
Font size: 16px
Characters per line: ~180 (too long!)

Solution: Use max-width on content
.content { max-width: 720px; }
Characters per line: ~90 (good)
```

### Responsive Spacing

Spacing should scale with screen size.

**Fixed spacing (bad):**
```css
.section {
  padding: 32px; /* Same everywhere */
}
```
Result: Excessive padding on mobile, too tight on desktop.

**Responsive spacing (good):**
```css
.section {
  padding: 16px; /* Mobile */
}

@media (min-width: 640px) {
  .section {
    padding: 24px;
  }
}

@media (min-width: 1024px) {
  .section {
    padding: 48px;
  }
}
```

Or use fluid spacing:
```css
.section {
  padding: clamp(16px, 5vw, 48px);
}
```

---

## Responsive Images

### Image Sizing Strategies

**Responsive image (adapts to container):**
```html
<img src="photo.jpg" alt="Description" style="width: 100%; height: auto;">
```
Image scales with container, maintains aspect ratio.

**Picture element (art direction):**
```html
<picture>
  <source media="(min-width: 1024px)" srcset="photo-desktop.jpg">
  <source media="(min-width: 640px)" srcset="photo-tablet.jpg">
  <img src="photo-mobile.jpg" alt="Description">
</picture>
```
Different images for different screen sizes (art direction).

**Srcset (resolution adaptation):**
```html
<img 
  src="photo.jpg"
  srcset="photo-small.jpg 480w, photo-large.jpg 1200w"
  alt="Description"
>
```
Browser chooses appropriate size based on device pixel ratio.

### Mobile Image Optimization

Mobile users have limited bandwidth. Optimize aggressively.

```
Desktop: 1200px wide image, 150KB
Mobile: 375px wide image, 30KB (80% size reduction)

Format:
- Desktop: WebP 150KB + JPEG fallback
- Mobile: WebP 30KB + JPEG fallback

Loading:
- Above fold: eager (load immediately)
- Below fold: lazy (load on scroll)
```

---

## Navigation Patterns

### Mobile Navigation

**Hamburger menu (tabs-based):**
```
Mobile view:
├── Header with menu button (☰)
├── On tap → slides in sidebar
│   ├── Home
│   ├── About
│   ├── Products
│   └── Contact
└── Tap outside → closes

Advantages:
- Saves screen space
- Familiar pattern
- Touch-friendly

Disadvantages:
- Hidden menu (less discoverable)
- Extra tap needed
```

**Bottom navigation (tabs-based):**
```
Mobile view:
├── Content area (80% height)
└── Bottom nav bar (20% height)
    ├── Home (icon + label)
    ├── Search (icon + label)
    ├── Add (icon + label)
    └── Profile (icon + label)

Advantages:
- Always visible
- Thumb-reachable
- Quick access

Disadvantages:
- Takes up screen space
- Works for 4-5 items only
```

**Adaptive navigation (changes at breakpoints):**
```
Mobile (< 640px):
├── Hamburger menu (sidebar or bottom sheet)

Tablet (640px - 1023px):
├── Horizontal tabs at top
├── Icon + label

Desktop (1024px+):
├── Full navigation menu at top
├── Icon + label + description (if space)
```

---

## Performance on Mobile

### Core Web Vitals for Mobile

Mobile has stricter performance requirements.

**Target metrics:**
- **LCP** (Largest Contentful Paint): < 2.5s
- **INP** (Interaction to Next Paint): < 200ms
- **CLS** (Cumulative Layout Shift): < 0.1

**Mobile considerations:**
- Slower networks (3G, 4G vs. home WiFi)
- Slower processors (older phones)
- Battery constraints (minimize heavy computation)

### Mobile Optimization Checklist

- [ ] Images optimized (compressed, correct format)
- [ ] Lazy load images below fold
- [ ] JavaScript deferred/split by route
- [ ] CSS minified and only loaded when needed
- [ ] Fonts optimized (font-display: swap)
- [ ] Avoid layout shifts (set image dimensions)
- [ ] Touch targets are 48px+
- [ ] No horizontal scroll (content fits viewport)

---

## Viewport Configuration

### Viewport Meta Tag

Critical for responsive design. Always include:

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

**What it does:**
- `width=device-width` — Set viewport width to device width (not desktop width)
- `initial-scale=1.0` — Initial zoom level is 1:1 (no zoom)

**Without this tag:** Browser assumes 980px width (desktop), scales down. Result: tiny text, poor mobile experience.

### Safe Areas (Notch Support)

Support devices with notches and safe areas:

```css
body {
  padding-top: max(16px, env(safe-area-inset-top));
  padding-bottom: max(16px, env(safe-area-inset-bottom));
  padding-left: max(16px, env(safe-area-inset-left));
  padding-right: max(16px, env(safe-area-inset-right));
}
```

---

## Testing Responsive Behavior

### Device Testing Strategy

**Lab testing (Figma + DevTools):**
- Figma frames at different breakpoints
- Chrome DevTools device emulation (fast, convenient)
- Test common devices (iPhone SE, iPhone 14, iPad, Galaxy Tab)

**Real device testing:**
- Borrow devices when possible
- Test on actual phones/tablets (emulation isn't perfect)
- Check performance on real networks

**Network throttling:**
- Chrome DevTools → Network → Throttle (3G, 4G)
- Test on actual slow connections
- Measure how long images take to load

### Responsive Testing Checklist

- [ ] Mobile breakpoint (375px): layout stacks, navigation works
- [ ] Tablet breakpoint (768px): 2-column layout, touch targets 48px+
- [ ] Desktop breakpoint (1440px): multi-column, horizontal nav
- [ ] Images responsive (no fixed widths)
- [ ] Text readable (no horizontal scroll)
- [ ] Touch targets 48px+ (mobile)
- [ ] No layout shifts when images load
- [ ] Keyboard navigation works (mobile + desktop)
- [ ] Dark mode works at all breakpoints
- [ ] Performance meets targets (LCP < 2.5s on 4G)

---

## Common Responsive Design Mistakes

### ❌ Fixed Widths

Wrong:
```css
.container { width: 1200px; }
```
Result: Content too wide for mobile (horizontal scroll).

Right:
```css
.container {
  width: 100%;
  max-width: 1200px;
  padding: 16px;
}
```

### ❌ Horizontal Scroll

Wrong:
```css
.card { width: 300px; overflow-x: auto; }
```
Result: Awkward horizontal scroll on mobile.

Right:
```css
.cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
}
```

### ❌ Forgetting Touch Targets

Wrong:
```html
<button style="width: 32px; height: 32px;">X</button>
```
Result: Hard to tap, frustrating on mobile.

Right:
```html
<button style="width: 48px; height: 48px;">X</button>
```

### ❌ Hover-Only Interactions

Wrong:
```css
button:hover .tooltip { display: block; }
```
Result: Tooltip never shows on touch devices.

Right:
```html
<details>
  <summary>Button</summary>
  <p>Tooltip text</p>
</details>
```

---

## When to Use This Skill

✅ **Do use for:**
- Mobile-first design strategy
- Responsive breakpoint planning
- Flexible grid and flexbox layouts
- Touch-friendly interface design
- Responsive typography and spacing
- Responsive image strategies
- Mobile navigation patterns
- Cross-device testing
- Performance optimization for mobile
- Accessibility across devices

❌ **Don't use for:**
- General accessibility (see web-accessibility-a11y)
- Inclusive design patterns (see inclusive-design-patterns)
- CSS implementation details (see css-styling-pixel-perfect)
- Performance metrics (see web-performance-optimization)
- Component implementation (see figma-expert-workflows)
