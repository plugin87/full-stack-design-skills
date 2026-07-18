# Responsive Breakpoint Strategy

How to choose breakpoints, structure them, and implement responsive behavior.

---

## Step 1: Identify Your Breakpoints

### Approach 1: Standard Breakpoints (Start Here)

Use proven breakpoints that work for most projects:

```
Mobile:            320px - 479px
Mobile Large:      480px - 639px
Tablet:            640px - 1023px
Desktop:           1024px - 1439px
Desktop Large:     1440px+
```

**Why these numbers?**
- 320px: Minimum phone width (older phones like iPhone SE)
- 480px: Larger phones (iPhone 13)
- 640px: iPad mini width (landscape)
- 1024px: iPad width (portrait)
- 1440px: MacBook Pro width (common laptop)

### Approach 2: Content-Driven Breakpoints (Better)

Choose breakpoints based on **when your layout breaks**, not device sizes.

**Workflow:**
1. Design at mobile (375px)
2. Gradually increase window width
3. When layout looks bad → add breakpoint
4. When layout looks good → stop

**Example:**
```
Start at 375px mobile
↓ (gradually expand)
700px: 2-column layout starts to work
↓
1100px: 3-column layout starts to work
↓
1600px: Extra padding and spacing for large screens

Result:
@media (min-width: 700px) { /* Layout change */ }
@media (min-width: 1100px) { /* Layout change */ }
@media (min-width: 1600px) { /* Spacing change */ }
```

### Approach 3: Feature-Based Breakpoints (Most Flexible)

Name breakpoints by feature, not device:

```
$breakpoint-sidebar-visible: 768px;  /* Sidebar fits beside content */
$breakpoint-multi-column: 1024px;    /* Multiple columns work */
$breakpoint-desktop-spacing: 1440px; /* Extra padding for large screens */
```

Use in code:
```scss
.sidebar {
  display: none;
}

@media (min-width: $breakpoint-sidebar-visible) {
  .sidebar {
    display: block;
  }
}
```

---

## Step 2: Structure Breakpoints in Figma

### Figma Setup

Create a dedicated "Responsive" page:

```
Figma File
├── Pages
│   ├── Cover (overview, breakpoint reference)
│   ├── Responsive (all breakpoints)
│   │   ├── Frame: Mobile (375 × 667) — iPhone 13
│   │   ├── Frame: Mobile Large (480 × 800) — Galaxy S21
│   │   ├── Frame: Tablet (768 × 1024) — iPad Air
│   │   ├── Frame: Desktop (1440 × 900) — MacBook
│   │   └── Frame: Desktop Large (1920 × 1080) — 4K monitor
│   ├── Components (reusable at any breakpoint)
│   ├── Patterns (layout examples)
│   └── Pages (full screens at each breakpoint)
```

### Document Breakpoints

In cover page, create a breakpoint reference:

```
## Responsive Breakpoints

### Mobile (320px - 479px)
- Single column layout
- Full-width content
- Hamburger menu
- Bottom navigation (if needed)
- Touch targets: 48px+
- Padding: 16px

### Tablet (480px - 1023px)
- 2-column layout
- Sidebar navigation
- Medium spacing: 24px
- Touch targets: 44px+ (mouse can be smaller)

### Desktop (1024px+)
- Multi-column (3-4 columns)
- Horizontal navigation
- Large spacing: 32px+
- Optimized for mouse interaction

### Key Layout Changes
- Navigation: hamburger (mobile) → horizontal (tablet) → full menu (desktop)
- Grid: 1 col (mobile) → 2 col (tablet) → 3 col (desktop)
- Sidebar: hidden (mobile) → visible (tablet+)
```

---

## Step 3: Implement Breakpoints in Code

### Using CSS Media Queries

**Mobile-first approach (recommended):**

```css
/* Base: Mobile (320px+) */
.container {
  width: 100%;
  padding: 16px;
  column-count: 1;
}

.sidebar {
  display: none; /* Hidden on mobile */
}

/* Tablet and up (640px+) */
@media (min-width: 640px) {
  .container {
    padding: 24px;
    column-count: 2;
  }

  .sidebar {
    display: block; /* Show sidebar on tablet */
  }
}

/* Desktop and up (1024px+) */
@media (min-width: 1024px) {
  .container {
    padding: 32px;
    column-count: 3;
  }

  .sidebar {
    width: 280px; /* Fixed width on desktop */
  }
}
```

### Using SCSS Mixins

Create reusable breakpoint mixins:

```scss
$breakpoints: (
  'mobile': 320px,
  'mobile-lg': 480px,
  'tablet': 640px,
  'desktop': 1024px,
  'desktop-lg': 1440px
);

@mixin media($breakpoint) {
  @media (min-width: map-get($breakpoints, $breakpoint)) {
    @content;
  }
}

/* Usage */
.container {
  padding: 16px; /* Mobile default */

  @include media('tablet') {
    padding: 24px;
  }

  @include media('desktop') {
    padding: 32px;
  }
}
```

### Using Tailwind CSS

Tailwind has built-in responsive prefixes:

```html
<!-- Mobile: full width, Tablet+: fixed width -->
<div class="w-full md:w-2/3 lg:w-1/2">
  Content
</div>

<!-- Mobile: stacked, Tablet+: side-by-side -->
<div class="flex flex-col md:flex-row gap-4">
  <div>Column 1</div>
  <div>Column 2</div>
</div>

<!-- Hidden on mobile, visible on tablet+ -->
<aside class="hidden md:block">
  Sidebar
</aside>
```

**Tailwind breakpoints (can customize):**
```javascript
// tailwind.config.js
module.exports = {
  theme: {
    screens: {
      'sm': '640px',   // mobile-lg
      'md': '768px',   // tablet
      'lg': '1024px',  // desktop
      'xl': '1280px',  // desktop
      '2xl': '1536px'  // desktop-lg
    }
  }
}
```

---

## Step 4: Layout Patterns by Breakpoint

### Single Column (Mobile)

```
┌─────────────────┐
│     Header      │ 100% width
├─────────────────┤
│                 │
│   Main Content  │ Full width, padding 16px
│   (full width)  │
│                 │
├─────────────────┤
│     Footer      │ 100% width
└─────────────────┘
```

**CSS:**
```css
body {
  display: flex;
  flex-direction: column;
}

main {
  flex: 1;
  width: 100%;
}
```

### Two Column (Tablet)

```
┌──────────────────────────────────────┐
│          Header (100%)               │
├────────────────┬──────────────────────┤
│                │                      │
│   Sidebar      │   Main Content       │
│  (fixed 280px) │   (flex: 1)         │
│                │                      │
├────────────────┴──────────────────────┤
│          Footer (100%)                │
└──────────────────────────────────────┘
```

**CSS:**
```css
@media (min-width: 640px) {
  body {
    display: grid;
    grid-template-columns: 280px 1fr;
  }

  main {
    flex: 1;
  }

  aside {
    grid-column: 1;
  }

  main {
    grid-column: 2;
  }
}
```

### Three Column (Desktop)

```
┌─────────────────────────────────────────────────────┐
│                  Header (100%)                      │
├──────────┬──────────────────────┬──────────┐
│          │                      │          │
│ Sidebar  │   Main Content       │ Sidebar  │
│ (280px)  │   (flex: 1)          │ (280px)  │
│          │                      │          │
├──────────┴──────────────────────┴──────────┤
│              Footer (100%)                 │
└─────────────────────────────────────────────┘
```

**CSS:**
```css
@media (min-width: 1024px) {
  body {
    display: grid;
    grid-template-columns: 280px 1fr 280px;
    grid-gap: 24px;
  }
}
```

---

## Step 5: Common Breakpoint Patterns

### Navigation Transformation

**Mobile:**
```
Header
├── Logo (left)
└── Menu button (☰) (right)

On tap menu button:
├── Overlay
└── Menu
    ├── Home
    ├── About
    ├── Products
    └── Contact
```

**Tablet+:**
```
Header (full width)
├── Logo (left)
├── Menu (center/right)
│   ├── Home
│   ├── About
│   ├── Products
│   └── Contact
└── Profile (right)
```

**Implementation:**
```css
/* Mobile: hamburger menu */
.nav-menu {
  display: none; /* Hidden by default */
}

.nav-toggle {
  display: block; /* Hamburger button visible */
}

/* Tablet+: horizontal menu */
@media (min-width: 768px) {
  .nav-menu {
    display: flex; /* Show menu */
  }

  .nav-toggle {
    display: none; /* Hide hamburger */
  }
}
```

### Content Grid Transformation

**Mobile:**
```
┌─────────┐
│ Card 1  │
└─────────┘
┌─────────┐
│ Card 2  │
└─────────┘
┌─────────┐
│ Card 3  │
└─────────┘
```

**Tablet:**
```
┌─────────┬─────────┐
│ Card 1  │ Card 2  │
├─────────┴─────────┤
│     Card 3        │
└───────────────────┘
```

**Desktop:**
```
┌─────────┬─────────┬─────────┐
│ Card 1  │ Card 2  │ Card 3  │
└─────────┴─────────┴─────────┘
```

**Implementation (auto-fit approach):**
```css
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 16px;
}

/* Automatically adapts:
   - Mobile (< 300px): 1 column
   - Tablet (600px+): 2 columns
   - Desktop (900px+): 3 columns
*/
```

---

## Step 6: Responsive Spacing

### Spacing Scale by Breakpoint

```
Mobile (16px padding):
├── Section gap: 16px
├── Element spacing: 8px
└── Icon + text: 4px

Tablet (24px padding):
├── Section gap: 24px
├── Element spacing: 12px
└── Icon + text: 8px

Desktop (32px padding):
├── Section gap: 32px
├── Element spacing: 16px
└── Icon + text: 8px
```

**Implementation:**
```css
.section {
  padding: 16px; /* Mobile */
  margin-bottom: 16px;
}

@media (min-width: 640px) {
  .section {
    padding: 24px;
    margin-bottom: 24px;
  }
}

@media (min-width: 1024px) {
  .section {
    padding: 32px;
    margin-bottom: 32px;
  }
}
```

---

## Step 7: Breakpoint Testing Checklist

- [ ] Mobile (320px): Single column, hamburger menu, full-width buttons
- [ ] Mobile Large (480px): Layout adapts, still readable
- [ ] Tablet (768px): 2-column layout, sidebar visible
- [ ] Desktop (1024px): Multi-column, horizontal navigation
- [ ] Desktop Large (1440px): Extra padding, content width limited
- [ ] Images responsive (scale with screen size)
- [ ] Text readable at all sizes (45-75 characters per line)
- [ ] Touch targets 48px+ on mobile
- [ ] No horizontal scroll at any breakpoint
- [ ] Dark mode works at all breakpoints

---

## Common Breakpoint Mistakes

### ❌ Too Many Breakpoints

Wrong:
```css
@media (min-width: 320px) { }
@media (min-width: 360px) { }
@media (min-width: 375px) { }
@media (min-width: 480px) { }
@media (min-width: 768px) { }
@media (min-width: 1024px) { }
@media (min-width: 1440px) { }
```
Result: Complex, hard to maintain.

Right:
```css
/* Mobile default */
/* Tablet: 640px+ */
@media (min-width: 640px) { }
/* Desktop: 1024px+ */
@media (min-width: 1024px) { }
```

### ❌ Desktop-First Approach

Wrong:
```css
/* Desktop first */
.container { width: 1200px; }

/* Then subtract for mobile */
@media (max-width: 768px) {
  .container { width: 100%; }
}
```
Harder to maintain, forces overriding desktop styles.

Right:
```css
/* Mobile first */
.container { width: 100%; }

/* Then enhance for larger screens */
@media (min-width: 1024px) {
  .container { width: 1200px; }
}
```

---

## Breakpoint Decision Tree

Use this to decide breakpoints for your project:

```
1. Start with standard breakpoints
   ├── Mobile: 320px
   ├── Tablet: 640px
   └── Desktop: 1024px

2. Test in browser
   ├── Does layout look good?
       └── YES → Keep these breakpoints
   ├── Does layout break?
       └── Add breakpoint where it breaks

3. Document in Figma + cover page

4. Update CSS/Tailwind config

5. Test again on real devices
```

If in doubt, start with 3 breakpoints (mobile, tablet, desktop). Add more only if content breaks.
