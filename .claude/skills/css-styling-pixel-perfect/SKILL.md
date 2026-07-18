---
name: css-styling-pixel-perfect
description: Master CSS architecture, styling strategies, and pixel-perfect component implementation. Use this skill for CSS architecture patterns (BEM, OOCSS, atomic), Tailwind CSS configuration and best practices, CSS-in-JS libraries, component styling patterns, CSS modules, responsive styling, performance optimization, and accessibility-aware styling. Triggers include: CSS organization and naming conventions, Tailwind configuration, styled-components or emotion usage, component styling strategies, CSS specificity issues, responsive design implementation, CSS performance, and pixel-perfect design implementation. Essential for frontend developers building maintainable, performant, and accessible stylesheets.
---

# CSS Styling & Pixel-Perfect Design

Practical CSS patterns and strategies for building scalable, maintainable, and performant stylesheets.

CSS is often overlooked, but architectural decisions made early determine scalability, maintainability, and performance. Understanding CSS architecture, choosing the right approach (BEM vs Tailwind vs CSS-in-JS), and applying best practices is essential for professional development.

---

## Core Principle: Choose an Architecture

### CSS Architecture Options

**Traditional CSS (BEM, OOCSS):**
- Classes and naming conventions
- Organized by specificity
- Pros: Simple, works everywhere
- Cons: Class naming is hard, specificity conflicts

**Utility-First (Tailwind):**
- Predefined utility classes
- Build custom designs from utilities
- Pros: Fast development, consistent spacing/colors
- Cons: HTML feels cluttered, learning curve

**CSS-in-JS (styled-components, emotion):**
- Write CSS in JavaScript
- Scoped to component
- Pros: No naming conflicts, dynamic styles
- Cons: Runtime cost, harder to debug

**CSS Modules:**
- Local-scoped CSS
- Imports into components
- Pros: No naming conflicts, traditional CSS
- Cons: Extra build step, less reusable

---

## Architecture Pattern 1: BEM (Block Element Modifier)

**Naming convention:** `.block__element--modifier`

```css
/* Block: main component */
.button { ... }

/* Element: part of block */
.button__text { ... }

/* Modifier: state or variation */
.button--primary { ... }
.button--disabled { ... }

/* Usage */
.button--primary.button--large { ... }
```

**HTML:**
```html
<button class="button button--primary">
  <span class="button__text">Click me</span>
</button>
```

**Pros:**
- Clear structure
- No cascading issues
- Reusable across projects

**Cons:**
- Verbose class names
- Takes discipline to maintain

---

## Architecture Pattern 2: Tailwind CSS (Utility-First)

**Apply utility classes to build components:**

```html
<!-- Direct in HTML -->
<button class="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600 focus:outline-none focus:ring-2">
  Click me
</button>

<!-- Or extract to component -->
<button class="btn btn-primary">Click me</button>
```

**CSS:**
```css
@layer components {
  .btn {
    @apply px-4 py-2 rounded font-medium transition-colors;
  }

  .btn-primary {
    @apply bg-blue-500 text-white hover:bg-blue-600;
  }

  .btn-primary:focus {
    @apply outline-none ring-2 ring-blue-300;
  }
}
```

**Configuration (tailwind.config.js):**
```javascript
module.exports = {
  theme: {
    extend: {
      colors: {
        brand: '#2563eb',
      },
      spacing: {
        '128': '32rem',
      },
    },
  },
  plugins: [
    require('@tailwindcss/forms'),
    require('@tailwindcss/typography'),
  ],
};
```

**Pros:**
- Fast development
- Consistent design tokens
- Small bundle size with purging
- Great docs and community

**Cons:**
- HTML feels cluttered
- Harder to see styles at a glance
- Learning curve for beginners

---

## Architecture Pattern 3: CSS-in-JS (styled-components)

**Write CSS in JavaScript, scoped to component:**

```jsx
import styled from 'styled-components';

const ButtonStyle = styled.button`
  padding: 12px 16px;
  border-radius: 4px;
  background-color: ${props => props.primary ? '#2563eb' : '#e5e7eb'};
  color: ${props => props.primary ? 'white' : '#1e293b'};
  border: none;
  cursor: pointer;
  transition: background-color 200ms ease;

  &:hover {
    background-color: ${props => props.primary ? '#1d4ed8' : '#cbd5e1'};
  }

  &:focus {
    outline: 2px solid #0052cc;
    outline-offset: 2px;
  }

  /* Responsive */
  @media (max-width: 640px) {
    padding: 8px 12px;
    font-size: 14px;
  }
`;

function Button({ primary, children }) {
  return <ButtonStyle primary={primary}>{children}</ButtonStyle>;
}
```

**Pros:**
- No naming conflicts
- Dynamic styles with props
- Component-scoped (no global conflicts)
- Can use JavaScript variables

**Cons:**
- Runtime performance cost
- Larger bundle size
- Harder to override styles
- Not cacheable like separate CSS files

---

## Architecture Pattern 4: CSS Modules

**Locally-scoped CSS files linked to components:**

**Button.module.css:**
```css
.button {
  padding: 12px 16px;
  border-radius: 4px;
  border: none;
  cursor: pointer;
  transition: background-color 200ms ease;
}

.button:focus {
  outline: 2px solid #0052cc;
  outline-offset: 2px;
}

.primary {
  background-color: #2563eb;
  color: white;
}

.primary:hover {
  background-color: #1d4ed8;
}
```

**Button.jsx:**
```jsx
import styles from './Button.module.css';

export function Button({ primary, children }) {
  return (
    <button className={`${styles.button} ${primary ? styles.primary : ''}`}>
      {children}
    </button>
  );
}
```

**Pros:**
- Traditional CSS (good tooling)
- Locally-scoped (no conflicts)
- Good performance
- Easy to debug (dev tools work)

**Cons:**
- Extra files to manage
- Can't share styles easily
- Naming patterns required

---

## Component Styling Patterns

### Pattern 1: Atomic Components (Small, Reusable)

```jsx
// Atomic components: single responsibility
export function Button({ children, variant, size }) {
  return <button className={`btn btn-${variant} btn-${size}`}>{children}</button>;
}

export function Badge({ children, color }) {
  return <span className={`badge badge-${color}`}>{children}</span>;
}

export function Icon({ name, size }) {
  return <svg className={`icon icon-${size}`} data-icon={name} />;
}
```

**Benefits:**
- Highly reusable
- Easy to test
- Small, focused responsibility

### Pattern 2: Compound Components (Complex UI)

```jsx
// Compound components: work together
export function Card({ children }) {
  return <div className="card">{children}</div>;
}

export function CardHeader({ children }) {
  return <div className="card__header">{children}</div>;
}

export function CardBody({ children }) {
  return <div className="card__body">{children}</div>;
}

export function CardFooter({ children }) {
  return <div className="card__footer">{children}</div>;
}

// Usage
<Card>
  <CardHeader>Title</CardHeader>
  <CardBody>Content</CardBody>
  <CardFooter>Actions</CardFooter>
</Card>
```

### Pattern 3: Flexible Components (Props-Based)

```jsx
export function Text({ children, size = 'base', weight = 'normal', color = 'default' }) {
  const sizeClass = `text-${size}`;
  const weightClass = `font-${weight}`;
  const colorClass = `text-${color}`;
  
  return <p className={`${sizeClass} ${weightClass} ${colorClass}`}>{children}</p>;
}

// Usage
<Text size="lg" weight="bold" color="primary">Large bold primary text</Text>
```

---

## CSS Performance

### Minimize CSS Size

**Before (1,200 lines):**
```css
/* Lots of unused code */
.button { ... }
.button-primary { ... }
.button-secondary { ... }
.button-disabled { ... }
/* etc for all components */
```

**After with Tailwind (40 KB purged):**
- Only keeps classes used in templates
- Builds smaller CSS file
- Same functionality

### Avoid Expensive CSS

```css
/* ❌ Expensive (triggers layout) */
.box {
  width: 100%;
  transform: translateX(10px); /* Re-calculates layout */
}

/* ✅ Better (no layout recalc) */
.box {
  transform: translateX(10px); /* No layout cost */
}
```

### CSS Specificity Management

```css
/* ❌ High specificity (hard to override) */
div.header .nav ul li a.active { ... }

/* ✅ Low specificity (easy to override) */
.nav__link--active { ... }
```

---

## Responsive CSS

### Mobile-First Approach

```css
/* Mobile first (base styles) */
.button {
  width: 100%;
  padding: 12px;
  font-size: 16px;
}

/* Tablet and up */
@media (min-width: 640px) {
  .button {
    width: auto;
    padding: 10px 16px;
    font-size: 14px;
  }
}

/* Desktop and up */
@media (min-width: 1024px) {
  .button {
    padding: 12px 24px;
  }
}
```

### CSS Custom Properties (Variables)

```css
:root {
  --color-primary: #2563eb;
  --color-primary-dark: #1d4ed8;
  --spacing-xs: 4px;
  --spacing-sm: 8px;
  --spacing-md: 16px;
}

.button {
  padding: var(--spacing-sm) var(--spacing-md);
  background-color: var(--color-primary);
}

.button:hover {
  background-color: var(--color-primary-dark);
}

/* Dark mode */
@media (prefers-color-scheme: dark) {
  :root {
    --color-primary: #60a5fa;
    --color-primary-dark: #3b82f6;
  }
}
```

---

## Accessibility in CSS

### Focus States (Must Have)

```css
/* ❌ Remove default focus (bad for accessibility) */
button:focus {
  outline: none; /* Never do this! */
}

/* ✅ Provide visible focus */
button:focus {
  outline: 2px solid #0052cc;
  outline-offset: 2px;
}
```

### Reduced Motion

```css
/* Animations by default */
.card {
  transition: transform 300ms ease;
}

.card:hover {
  transform: translateY(-4px);
}

/* Respect user preference */
@media (prefers-reduced-motion: reduce) {
  .card {
    transition: none;
  }

  .card:hover {
    transform: none;
  }
}
```

### Color + Text (No Color Alone)

```css
/* ❌ Status by color alone */
.status-success {
  color: green;
}

/* ✅ Status with icon + color + text */
.status-success::before {
  content: '✓';
}

.status-success {
  color: green;
}
```

---

## Common CSS Mistakes

### ❌ Inline Styles

```jsx
/* ❌ Wrong: No reusability */
<div style={{ padding: '16px', color: 'blue' }}>Content</div>

/* ✅ Right: Use CSS classes */
<div className="card">Content</div>
```

### ❌ Overusing !important

```css
/* ❌ Wrong: Makes code hard to maintain */
.button { color: blue !important; }

/* ✅ Right: Use specificity correctly */
.button { color: blue; }
```

### ❌ Magic Numbers

```css
/* ❌ Wrong: Where does 17px come from? */
.button {
  padding: 17px 24px;
}

/* ✅ Right: Use system spacing */
.button {
  padding: var(--spacing-md) var(--spacing-lg);
}
```

---

## When to Use This Skill

✅ **Do use for:**
- CSS architecture and organization
- Tailwind CSS configuration and best practices
- CSS-in-JS libraries (styled-components, emotion)
- Component styling strategies
- CSS modules setup and patterns
- Responsive CSS implementation
- CSS performance optimization
- Accessibility-aware styling
- Naming conventions (BEM, etc.)

❌ **Don't use for:**
- Design principles (see design-fundamentals)
- Responsive design strategy (see responsive-universal-design)
- Accessibility standards (see web-accessibility-a11y)
- Design tokens (see design-systems-architecture)
- HTML semantics (see web-accessibility-a11y)
