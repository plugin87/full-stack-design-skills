# CSS Architecture Patterns

Organization strategies for scalable CSS.

---

## BEM (Block Element Modifier) Pattern

```css
/* Block: standalone component */
.card { }

/* Element: part of block */
.card__header { }
.card__body { }
.card__footer { }

/* Modifier: state or variation */
.card--featured { }
.card--disabled { }

/* Combined */
.card--featured .card__header { }
```

**HTML:**
```html
<div class="card card--featured">
  <div class="card__header">
    <h2>Title</h2>
  </div>
  <div class="card__body">
    <p>Content</p>
  </div>
</div>
```

**Pros:**
- Clear naming structure
- No cascading/specificity issues
- Easy to maintain
- Reusable patterns

---

## OOCSS (Object-Oriented CSS)

```css
/* Define core object */
.box {
  margin: 0;
  padding: 1rem;
  border: 1px solid #ccc;
}

/* Create variations */
.box-large {
  padding: 2rem;
}

.box-dark {
  background: #333;
  color: #fff;
}

/* Combine */
<div class="box box-large box-dark">Content</div>
```

**Pros:**
- Reusable components
- Minimal CSS

---

## Atomic CSS (Utility-First)

```css
.m-0 { margin: 0; }
.p-4 { padding: 1rem; }
.border { border: 1px solid #ccc; }
.text-lg { font-size: 1.125rem; }
.font-bold { font-weight: 700; }
```

**HTML:**
```html
<div class="m-0 p-4 border text-lg font-bold">
  Content
</div>
```

**Pros:**
- Fast development
- Reusable utilities
- Consistent design system

**Cons:**
- HTML gets cluttered
- Large learner curve

---

## Folder Structure for Scale

```
styles/
├── base/
│   ├── reset.css (browser defaults)
│   └── typography.css (font settings)
├── components/
│   ├── button.css
│   ├── card.css
│   ├── navbar.css
│   └── form.css
├── utilities/
│   ├── spacing.css
│   ├── colors.css
│   └── layout.css
├── layout/
│   ├── container.css
│   ├── grid.css
│   └── flexbox.css
└── index.css (imports all)
```

---

## CSS Specificity Management

```css
/* Level 0 (least specific) */
body { }
p { }

/* Level 1 (class) */
.button { }

/* Level 2 (multiple classes) */
.button.primary { }

/* Level 3 (ID, avoid!) */
#main { }

/* Rule: Use lowest specificity necessary */
.button { }  /* Good */
.nav .button { }  /* Not ideal */
div.nav button.primary { }  /* Bad */
```

**Best practice:**
- Use classes, not IDs
- Avoid element + class selector
- Keep specificity consistent

---

## Performance Tips

### CSS Size

- Remove unused styles
- Avoid !important
- Minimize nesting
- Reuse classes

### Rendering Performance

```css
/* ❌ Avoid (triggers reflow) */
.box {
  width: 100%;
  height: auto;
  position: relative;
}

/* ✅ Better (transform is fast) */
.box {
  transform: translateX(10px);
}
```

### Critical CSS

```html
<!-- Inline critical CSS for above-the-fold -->
<style>
  /* Above-the-fold styles only */
  body { font-family: sans-serif; }
  .header { background: blue; }
</style>

<!-- Load rest async -->
<link rel="stylesheet" href="styles.css" media="print" onload="this.media='all'">
```

---

## Mobile-First CSS

```css
/* Mobile first (base) */
.grid {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

/* Tablet and up */
@media (min-width: 640px) {
  .grid {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
  }
}

/* Desktop and up */
@media (min-width: 1024px) {
  .grid {
    grid-template-columns: repeat(3, 1fr);
  }
}
```

---

## Naming Convention Comparison

| Pattern | Example | Pros | Cons |
|---------|---------|------|------|
| BEM | .button__text--primary | Clear, reusable | Verbose |
| SMACSS | .btn-primary | Organized | Learning curve |
| OOCSS | .box .box-large | Simple | Can get messy |
| Atomic | .p-4 .text-lg | Fast | Cluttered |
| Tailwind | Use Tailwind | Productive | Lock-in |

---

## Checklist

- [ ] CSS organized by architecture
- [ ] Naming convention consistent
- [ ] No ID selectors
- [ ] Specificity managed
- [ ] Mobile-first approach
- [ ] Reusable components
- [ ] Performance optimized
- [ ] Accessible focus states
- [ ] Dark mode support
- [ ] Documented
