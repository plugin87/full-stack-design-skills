---
name: web-accessibility-a11y
description: Build accessible web experiences that work for everyone. Use this skill whenever dealing with WCAG compliance, semantic HTML, ARIA patterns, screen reader testing, keyboard navigation, contrast ratios, accessibility audits, or inclusive design patterns. Triggers include: accessibility concerns, form design, modal dialogs, focus management, live regions, color-only differentiation, keyboard shortcuts, keyboard traps, link/button semantics, landmark navigation, heading hierarchy, skip links, and alt text strategy. Essential for designers, developers, and QA ensuring digital products are usable by people with disabilities.
---

# Web Accessibility (a11y)

A practical guide to building digital products that work for everyone, regardless of ability.

Accessibility is not an afterthought—it's a foundational design principle. This skill covers the technical standards (WCAG 2.1) and practical patterns for building inclusive web experiences.

## Why Accessibility Matters

**Legal:** WCAG 2.1 compliance is increasingly required (ADA, ATAG, EN 301 549, UK EQA).

**Business:** ~15% of the global population has a disability. Accessible design benefits everyone (captions help in noisy environments, voice control helps people with mobility issues, clear navigation helps users with cognitive disabilities).

**Technical:** Accessible patterns are often better for SEO, performance, and maintainability.

---

## Core Principles: WCAG 2.1

WCAG (Web Content Accessibility Guidelines) has four principles, abbreviated as POUR:

### 1. Perceivable
Information must be perceivable—users must be able to see, hear, or otherwise sense it.

**Guidelines:**
- **Text alternatives** — Images need alt text
- **Captions & transcripts** — Audio/video needs text
- **Adaptable content** — Info not lost when zoomed or reflow
- **Distinguishable** — Text is readable (4.5:1 contrast, font size ≥12px)

### 2. Operable
Interfaces must be operable—users must be able to navigate and interact.

**Guidelines:**
- **Keyboard accessible** — All functions available via keyboard
- **Enough time** — Users can pause auto-playing content
- **Seizure prevention** — No flashing/flickering (3+ times/second)
- **Navigable** — Clear landmarks, skip links, heading hierarchy

### 3. Understandable
Content and operation must be understandable.

**Guidelines:**
- **Readable** — Clear language, short sentences, defined jargon
- **Predictable** — Consistent navigation, no surprises
- **Input assistance** — Form errors are identified and labeled clearly

### 4. Robust
Code must be robust—compatible with assistive technologies.

**Guidelines:**
- **Valid HTML** — Correct use of semantic elements
- **ARIA correctly** — Only use ARIA when HTML can't do it
- **Naming, role, value** — Every interactive element has a label (name), is understood (role), and exposes state (value)

---

## Semantic HTML: The Foundation

**Semantic HTML** means using HTML elements for their intended purpose. This is 80% of accessibility.

### Correct Element Usage

**Don't:**
```html
<div onclick="submit()">Submit</div>
<span class="heading">Page Title</span>
<div role="button" tabindex="0">Click me</div>
```

**Do:**
```html
<button>Submit</button>
<h1>Page Title</h1>
<button>Click me</button>
```

### Essential Semantic Elements

| Element | Purpose | Screen Reader Announces |
|---------|---------|------------------------|
| `<button>` | Clickable action | "Button: [label]" |
| `<a href="">` | Navigation link | "Link: [label]" |
| `<h1>`–`<h6>` | Headings (hierarchy) | "Heading level 1: [text]" |
| `<nav>` | Navigation region | "Navigation" |
| `<main>` | Main content region | "Main" |
| `<article>` | Self-contained content | "Article" |
| `<section>` | Thematic grouping | "Region" |
| `<aside>` | Sidebar/supplementary | "Complementary" |
| `<header>`, `<footer>` | Page regions | Landmarks |
| `<label for="">` | Form label (paired to `input`) | "Label: [text]" |
| `<fieldset>` & `<legend>` | Group related form inputs | "Group: [legend]" |
| `<ul>`, `<ol>`, `<li>` | Lists | "List, X items" |
| `<table>`, `<thead>`, `<th>`, `<td>` | Data tables (not layout) | Headers + cells announced |
| `<figure>` & `<figcaption>` | Images with captions | "Figure: [caption]" |

### Landmark Navigation

Users of screen readers navigate by landmarks (regions). Provide at least:
- One `<main>` — Contains primary content
- One `<nav>` — Contains navigation
- Optional `<header>`, `<footer>`, `<aside>`

```html
<body>
  <header>
    <nav><!-- Primary navigation --></nav>
  </header>
  <main>
    <article><!-- Primary content --></article>
  </main>
  <aside><!-- Sidebar --></aside>
  <footer><!-- Footer --></footer>
</body>
```

---

## Form Accessibility

Forms are the most common source of accessibility issues.

### Labeling

**Every input needs a label**, associated via `for`/`id`:

```html
<label for="email">Email address</label>
<input id="email" type="email" name="email" />
```

**Not this:**
```html
<input type="email" placeholder="Email" />
```
Placeholder text vanishes when you start typing — users with memory issues won't know what the field was for.

### Required Fields

Indicate required fields clearly in label + legend text:

```html
<label for="name">Name <span aria-label="required">*</span></label>
<input id="name" type="text" name="name" aria-required="true" />
```

### Error Handling

When validation fails:
1. Announce the error immediately (use `aria-live="polite"`)
2. Identify which field failed
3. Describe the error (not "Invalid" but "Phone number must be 10 digits")
4. Suggest correction

```html
<label for="phone">Phone</label>
<input id="phone" type="tel" aria-invalid="true" aria-describedby="phone-error" />
<div id="phone-error" role="alert">Phone must be 10 digits, e.g., 415-555-0123</div>
```

### Fieldsets & Legends

Group related inputs (radio buttons, checkboxes) with `<fieldset>` and label with `<legend>`:

```html
<fieldset>
  <legend>Select your experience level</legend>
  <label><input type="radio" name="level" value="beginner" /> Beginner</label>
  <label><input type="radio" name="level" value="intermediate" /> Intermediate</label>
  <label><input type="radio" name="level" value="expert" /> Expert</label>
</fieldset>
```

---

## Keyboard Navigation

All interactive elements must be keyboard accessible. Users without mice rely entirely on keyboard.

### Tab Order

Use the natural tab order (HTML source order). If visual order differs from source order, fix the source:

**Bad:**
```html
<!-- visually: 1, 2, 3 but tab order is: 1, 3, 2 -->
<button style="order: 2">Button 2</button>
<button style="order: 3">Button 3</button>
<button style="order: 1">Button 1</button>
```

**Good:**
```html
<!-- visual and tab order match source order -->
<button>Button 1</button>
<button>Button 2</button>
<button>Button 3</button>
```

### Focus Management

- **Never remove focus outline** — Users need visual feedback of where they are
- **Focus should be visible** on all interactive elements (buttons, links, inputs, custom controls)
- **Custom focus style** must have ≥3px visible area, 3:1 contrast

```css
button:focus-visible {
  outline: 2px solid #2563eb;
  outline-offset: 2px;
}
```

### Keyboard Traps

Avoid situations where keyboard users can't escape using keyboard. Test: Press Tab repeatedly — can you reach every element and move past it?

**Example trap:**
```html
<button onclick="openModal()">Open Modal</button>
<!-- Modal opens, focus not moved to modal, Tab cycles through background -->
```

**Fix:**
```html
<button onclick="openModal()">Open Modal</button>

<div role="dialog" aria-modal="true">
  <button autofocus>Close Modal</button>
  <!-- Focus trap: Tab cycles only within modal until closed -->
  <!-- Last focusable element's Tab cycles back to first -->
</div>
```

### Keyboard Shortcuts

If you define custom keyboard shortcuts (Ctrl+S, etc.):
- Avoid single-key shortcuts (break form inputs)
- Allow users to disable or remap shortcuts
- Document all shortcuts in Help

---

## ARIA (Accessible Rich Internet Applications)

ARIA adds accessibility information to HTML. **Use semantic HTML first; ARIA only when HTML can't do it.**

### Golden Rules of ARIA

1. **No ARIA is better than bad ARIA** — Incorrect ARIA breaks things for screen reader users
2. **ARIA doesn't change DOM** — `role="button"` on a `<div>` doesn't make it keyboard accessible; still need `tabindex="0"` and `onkeydown`
3. **ARIA is for exceptions** — 90% of a11y is semantic HTML

### Common ARIA Attributes

| Attribute | Use | Example |
|-----------|-----|---------|
| `role=""` | Tells AT the element's purpose | `<div role="button">` (only if not semantic element) |
| `aria-label=""` | Visible label missing | `<button aria-label="Close">×</button>` |
| `aria-labelledby=""` | Connect to visible heading | `<h2 id="x">Title</h2> <div aria-labelledby="x">` |
| `aria-describedby=""` | Additional description | `<input aria-describedby="hint">` + `<div id="hint">` |
| `aria-required="true"` | Field is required | `<input aria-required="true">` |
| `aria-invalid="true"` | Field has error | `<input aria-invalid="true">` |
| `aria-expanded="false"` | Toggle button state | `<button aria-expanded="false">Menu</button>` |
| `aria-hidden="true"` | Hide from AT | `<span aria-hidden="true">→</span>` |
| `aria-live="polite"` | Announce content changes | `<div aria-live="polite">3 items added</div>` |

### When to Use ARIA

**Don't use ARIA for:**
- Semantic HTML does it already (`<h1>`, `<button>`, `<label>`, etc.)
- Simple styling or layout (just use CSS)

**Do use ARIA for:**
- Custom components without semantic HTML (tabs, accordion, tooltip)
- Live regions that update (chat, notifications, search results)
- Hidden/collapsed content state (`aria-expanded`)
- Relationships between non-adjacent elements (`aria-labelledby`, `aria-describedby`)

---

## Testing & Validation

### Automated Tools (Catch ~30% of issues)

- **axe DevTools** (browser extension) — Fast, reliable
- **WebAIM Contrast Checker** — Verify color contrast
- **WAVE** — Visual accessibility feedback
- **Lighthouse** (Chrome DevTools) — Performance + accessibility audit

### Manual Testing (Required for remaining 70%)

**Keyboard-only navigation:**
- Unplug your mouse
- Navigate entire site with Tab, Enter, Shift+Tab, Arrow keys
- Identify any keyboard traps or missing focus

**Screen reader testing:**
- **NVDA** (Windows, free)
- **JAWS** (Windows, paid)
- **VoiceOver** (Mac, free — press Cmd+F5 to enable)
- **TalkBack** (Android, free)
- Test with real AT — browser extensions don't replicate the experience

**Zoom testing:**
- Zoom to 200% — text should reflow, no horizontal scroll
- Zoom to 400% — should still be readable

**Colorblind testing:**
- Simulator: Coblis
- Ensure information conveyed by color also uses icons, text, patterns

### Accessibility Audit Checklist

See bundled resources for comprehensive WCAG 2.1 AA checklist.

---

## Common Mistakes & Fixes

### Missing Alt Text
**Problem:** Images have no alt text.
**Fix:** Every image needs `alt=""` (can be empty if decorative, but must be present). Describe content concisely: `alt="Coffee cup on wooden table"` not `alt="image.jpg"`.

### Color-Only Indicators
**Problem:** Red = error, green = success (no other differentiation).
**Fix:** Add icons, text labels, or patterns alongside color.

### No Focus Visible
**Problem:** `outline: none` applied globally to remove focus ring.
**Fix:** Never remove focus outline. Customize it: `outline: 2px solid blue; outline-offset: 2px`.

### Inaccessible Modals
**Problem:** Modal opens; focus stays on background; keyboard user can't close it.
**Fix:** Move focus into modal on open. Trap Tab within modal. Move focus back to trigger on close.

### Incorrect Heading Hierarchy
**Problem:** h1, then h3, skipping h2.
**Fix:** Use sequential hierarchy (h1 → h2 → h3). Users rely on heading structure to navigate.

### Placeholder ≠ Label
**Problem:** Form fields use only placeholder; label removed "for clarity."
**Fix:** Always use `<label>`. Placeholder disappears when user types.

### Unlabeled Buttons
**Problem:** `<button>←</button>` or `<button><i class="icon-menu"></i></button>` (no text).
**Fix:** Add visible text or `aria-label="Back"` / `aria-label="Menu"`.

---

## Reference Files & Further Reading

Bundled resources:
- **wcag-2-1-checklist.md** — WCAG AA compliance checklist
- **aria-patterns-guide.md** — Common ARIA patterns (tabs, accordion, tooltip, etc.)
- **keyboard-navigation-guide.md** — Keyboard interaction patterns

---

## When to Use This Skill

✅ **Do use for:**
- WCAG compliance review and audit
- Form accessibility design and testing
- Keyboard navigation and focus management
- ARIA attribute selection and implementation
- Alt text strategy for images
- Modal, tooltip, popover, tab panel accessibility
- Color contrast compliance
- Heading hierarchy and landmark review
- Error handling and form validation
- Screen reader testing strategies

❌ **Don't use for:**
- CSS styling details (see design-fundamentals)
- Framework-specific implementation (see appropriate framework skill)
- Performance optimization (see web-performance-optimization)
- Design system component creation (see design-systems-architecture)
