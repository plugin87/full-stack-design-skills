# Mobile-First Layout Patterns

Common, proven layout patterns for responsive design at different breakpoints.

---

## Pattern 1: Hero + Content Section

### Mobile Layout

```
┌────────────────┐
│     Hero       │ Full width, centered
│   Image/Text   │ 16px padding
└────────────────┘
┌────────────────┐
│                │
│  Content       │ Single column
│  Section       │ Full width text
│                │
└────────────────┘
```

### Tablet Layout

```
┌──────────────────────────────────┐
│          Hero Section             │ Full width
│     (Image + Text Overlay)       │
└──────────────────────────────────┘
┌────────────┬──────────────────────┐
│ Sidebar    │  Main Content        │
│ (280px)    │  (flex: 1)           │
└────────────┴──────────────────────┘
```

### Desktop Layout

```
┌──────────────────────────────────────────┐
│              Hero Section                 │
│         (Full width, centered)           │
│              Max width: 1200px            │
└──────────────────────────────────────────┘
┌─────────┬──────────────────────────┬─────────┐
│Sidebar 1│    Main Content          │Sidebar 2│
│(280px)  │     (flex: 1)            │(280px)  │
└─────────┴──────────────────────────┴─────────┘
```

### CSS Implementation

```css
/* Mobile: Single column */
.hero {
  width: 100%;
  padding: 16px;
}

.content-wrapper {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.sidebar {
  display: none;
}

/* Tablet: 2 columns */
@media (min-width: 640px) {
  .content-wrapper {
    flex-direction: row;
    gap: 24px;
  }

  .sidebar {
    display: block;
    width: 280px;
    flex-shrink: 0;
  }
}

/* Desktop: 3 columns + constraints */
@media (min-width: 1024px) {
  .hero {
    max-width: 1200px;
    margin: 0 auto;
  }

  .content-wrapper {
    display: grid;
    grid-template-columns: 280px 1fr 280px;
    gap: 32px;
    max-width: 1200px;
    margin: 0 auto;
  }
}
```

---

## Pattern 2: Card Grid

### Mobile Layout

```
┌──────────────┐
│   Card 1     │
│ Image (100%) │
│ Title + Text │
└──────────────┘
┌──────────────┐
│   Card 2     │
│ Image (100%) │
│ Title + Text │
└──────────────┘
┌──────────────┐
│   Card 3     │
│ Image (100%) │
│ Title + Text │
└──────────────┘
```

### Tablet Layout

```
┌────────────────┬────────────────┐
│    Card 1      │    Card 2      │
├────────────────┼────────────────┤
│    Card 3      │    Card 4      │
└────────────────┴────────────────┘
```

### Desktop Layout

```
┌────────────┬────────────┬────────────┐
│  Card 1    │  Card 2    │  Card 3    │
├────────────┼────────────┼────────────┤
│  Card 4    │  Card 5    │  Card 6    │
└────────────┴────────────┴────────────┘
```

### CSS Implementation

**Auto-fit approach (responsive without media queries):**
```css
.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 16px;
  padding: 16px;
}

/* Automatically adapts:
   - Mobile (< 300px): 1 column
   - Tablet (600px+): 2 columns
   - Desktop (900px+): 3 columns
*/
```

**With explicit breakpoints:**
```css
.card-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 16px;
  padding: 16px;
}

@media (min-width: 640px) {
  .card-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 24px;
  }
}

@media (min-width: 1024px) {
  .card-grid {
    grid-template-columns: repeat(3, 1fr);
    gap: 32px;
    padding: 32px;
  }
}
```

---

## Pattern 3: Navigation Sidebar

### Mobile Layout

```
Header (flex)
├── Logo (left)
└── Menu button ☰ (right)

On menu button tap:
Overlay + Sidebar
├── Home
├── About
├── Products
└── Contact
```

### Tablet Layout

```
┌─────────────────────────────┐
│ Logo │ Nav │ Profile      │
└─────────────────────────────┘
┌──────┬─────────────────────┐
│      │                     │
│ Side │   Main Content      │
│ bar  │                     │
│      │                     │
└──────┴─────────────────────┘
```

### Desktop Layout

```
┌───────────────────────────────────────────────┐
│ Logo │ Navigation Menu              │ Profile │
└───────────────────────────────────────────────┘
┌──────┬───────────────────────────────┬────────┐
│      │                               │        │
│Side  │    Main Content               │Sidebar │
│bar   │                               │        │
│      │                               │        │
└──────┴───────────────────────────────┴────────┘
```

### CSS Implementation

```css
/* Mobile: Hamburger menu */
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 16px;
  height: 60px;
}

.nav-menu {
  display: none; /* Hidden on mobile */
}

.menu-button {
  display: block; /* Hamburger visible */
  width: 48px;
  height: 48px;
}

.sidebar {
  display: none;
}

/* Mobile menu (overlay) */
.nav-menu.open {
  display: flex;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  z-index: 100;
}

/* Tablet: Horizontal navigation */
@media (min-width: 768px) {
  .nav-menu {
    display: flex;
    gap: 24px;
  }

  .menu-button {
    display: none;
  }

  .sidebar {
    display: block;
    width: 280px;
  }
}

/* Desktop: Full menu + sidebars */
@media (min-width: 1024px) {
  .header {
    padding: 24px;
    height: 80px;
  }

  .nav-menu {
    gap: 32px;
  }
}
```

---

## Pattern 4: Form Layout

### Mobile Layout

```
┌──────────────────┐
│ Form             │
├──────────────────┤
│ Label            │ Full width labels
│ Input field      │ Full width inputs
│                  │ 48px height
├──────────────────┤
│ Label            │
│ Input field      │
│                  │
├──────────────────┤
│ Label            │
│ Checkbox / Radio │
├──────────────────┤
│  [Button]        │ Full width button
└──────────────────┘
```

### Tablet Layout

```
┌──────────────────────────────────┐
│ Form                             │
├──────────────────────────────────┤
│ First Name        │ Last Name    │ Two columns
├──────────────────────────────────┤
│ Email                            │ Full width
├──────────────────────────────────┤
│ Address                          │ Full width
├──────────────────────────────────┤
│ City        │ State │ ZIP        │ Three columns
├──────────────────────────────────┤
│ [Cancel]              [Submit]   │ Buttons right-aligned
└──────────────────────────────────┘
```

### CSS Implementation

```css
/* Mobile: Full width fields */
.form-group {
  display: flex;
  flex-direction: column;
  gap: 8px;
  margin-bottom: 16px;
}

.form-group label {
  font-size: 14px;
  font-weight: 600;
}

.form-group input {
  padding: 12px;
  font-size: 16px;
  height: 48px;
  width: 100%;
}

.form-row {
  display: grid;
  grid-template-columns: 1fr;
  gap: 16px;
}

.button-group {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.button {
  width: 100%;
  height: 48px;
}

/* Tablet: Multi-column layout */
@media (min-width: 640px) {
  .form-row.two-column {
    grid-template-columns: 1fr 1fr;
  }

  .form-row.three-column {
    grid-template-columns: 1fr 1fr 1fr;
  }

  .button-group {
    flex-direction: row;
    justify-content: flex-end;
  }

  .button {
    width: auto;
    min-width: 120px;
  }
}
```

---

## Pattern 5: Product Listing

### Mobile Layout

```
Card (full width):
┌────────────────┐
│   Image        │ 100% width
│ (auto height)  │
├────────────────┤
│ Title          │
├────────────────┤
│ Price: $99.99  │
├────────────────┤
│ ★★★★★ (4.5)   │
├────────────────┤
│  [Add to Cart] │ Full width button
└────────────────┘
```

### Tablet Layout

```
┌──────────────────┬──────────────────┐
│     Image        │   Image          │
│   (50% width)    │   (50% width)    │
├──────────────────┼──────────────────┤
│ Title + Price    │ Title + Price    │
│ Rating + Button  │ Rating + Button  │
└──────────────────┴──────────────────┘
```

### Desktop Layout

```
┌────────────┬────────────┬────────────┬────────────┐
│  Image     │  Image     │  Image     │  Image     │
│ + Details  │ + Details  │ + Details  │ + Details  │
└────────────┴────────────┴────────────┴────────────┘
```

### CSS Implementation

```css
.product-card {
  display: flex;
  flex-direction: column;
  background: white;
  border-radius: 8px;
  padding: 12px;
  gap: 12px;
}

.product-image {
  width: 100%;
  height: auto;
  aspect-ratio: 1 / 1;
  object-fit: cover;
  border-radius: 4px;
}

.product-title {
  font-size: 16px;
  font-weight: 600;
}

.product-price {
  font-size: 18px;
  color: #2563eb;
  font-weight: 700;
}

.product-rating {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 14px;
}

.product-button {
  width: 100%;
  height: 48px;
}

/* Grid container */
.product-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 16px;
  padding: 16px;
}

@media (min-width: 640px) {
  .product-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 24px;
  }
}

@media (min-width: 1024px) {
  .product-grid {
    grid-template-columns: repeat(4, 1fr);
    gap: 32px;
  }
}
```

---

## Pattern 6: Sticky Header + Bottom Navigation

### Mobile Layout

```
┌────────────────────┐
│ Header (sticky)    │ Stays at top when scroll
├────────────────────┤
│                    │
│   Scrollable       │
│   Content          │
│                    │
├────────────────────┤
│ Bottom Nav (sticky)│ Stays at bottom
│ ◄ │ ◆ │ ★ │ ≡ │
└────────────────────┘
```

### Tablet+ Layout

```
┌──────────────────────────────┐
│ Header (sticky)              │
├────────┬──────────────────────┤
│ Sidebar│                      │
│        │   Scrollable Content │
│        │                      │
│ (stick)│ (scroll)             │
└────────┴──────────────────────┘
```

### CSS Implementation

```css
/* Sticky header */
.header {
  position: sticky;
  top: 0;
  z-index: 10;
  background: white;
  border-bottom: 1px solid #e5e7eb;
  padding: 16px;
  height: 60px;
}

/* Scrollable content */
.main {
  padding-bottom: 60px; /* Space for bottom nav */
  overflow-y: auto;
}

/* Sticky bottom navigation (mobile only) */
.bottom-nav {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  display: flex;
  justify-content: space-around;
  background: white;
  border-top: 1px solid #e5e7eb;
  height: 60px;
  z-index: 10;
}

.bottom-nav-item {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 4px;
  font-size: 12px;
  color: #64748b;
  cursor: pointer;
}

.bottom-nav-item.active {
  color: #2563eb;
}

/* Tablet+: Hide bottom nav */
@media (min-width: 768px) {
  .bottom-nav {
    display: none;
  }

  .main {
    padding-bottom: 0;
  }

  /* Show sidebar instead */
  .sidebar {
    position: sticky;
    top: 60px; /* Below header */
    height: calc(100vh - 60px);
    overflow-y: auto;
  }
}
```

---

## Pattern 7: Modal/Dialog Responsive

### Mobile Layout

```
┌──────────────────┐
│ Full screen      │ Modal takes full screen
│ modal            │ with small padding
│                  │
│ (mostly content) │
│                  │
│  [Cancel] [OK]   │
└──────────────────┘
```

### Tablet+ Layout

```
                    ┌──────────────────┐
                    │                  │
                    │   Modal          │ Centered, smaller
                    │   (centered)     │ max-width: 500px
                    │                  │
                    │ [Cancel] [OK]    │
                    └──────────────────┘
```

### CSS Implementation

```css
/* Modal backdrop */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 100;
}

/* Modal content */
.modal {
  background: white;
  border-radius: 8px;
  padding: 24px;
  width: 100%;
  max-width: 90vw; /* Mobile: full width minus padding */
  max-height: 90vh;
  overflow-y: auto;
  box-shadow: 0 20px 25px rgba(0, 0, 0, 0.15);
}

/* Mobile: Full screen modal */
@media (max-width: 639px) {
  .modal-overlay {
    align-items: flex-end; /* Push to bottom on mobile */
  }

  .modal {
    width: 100%;
    max-width: 100%;
    border-radius: 16px 16px 0 0;
    padding: 16px;
    max-height: 90vh;
  }
}

/* Tablet+: Centered modal */
@media (min-width: 640px) {
  .modal {
    max-width: 500px;
  }

  .modal-overlay {
    align-items: center;
  }
}
```

---

## Quick Pattern Checklist

When designing a layout:

- [ ] Mobile first: start at 375px
- [ ] Choose responsive pattern (grid, sidebar, hero, etc.)
- [ ] Define breakpoints (typically 640px, 1024px)
- [ ] Test at each breakpoint
- [ ] Ensure touch targets are 48px+ on mobile
- [ ] No horizontal scroll at any size
- [ ] Images are responsive (width: 100%)
- [ ] Padding/spacing scales with screen size
- [ ] Navigation adapts (hamburger → horizontal)
- [ ] Forms stack on mobile, grid on tablet+

Pick a pattern that matches your content, not the other way around.
