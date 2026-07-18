# WCAG 2.1 Level AA Compliance Checklist

A practical checklist for auditing digital products against WCAG 2.1 AA standards.

## 1. Perceivable

### 1.1 Text Alternatives
- [ ] Every image has an alt attribute (can be empty if decorative)
- [ ] Alt text is concise (under 125 characters typically)
- [ ] Alt text describes purpose, not just "image of X"
- [ ] Complex images (charts, diagrams) have longer description or linked transcript
- [ ] Images of text: use actual text instead, or provide high-contrast alternative
- [ ] Background images (CSS) have textual fallback or are decorative only

### 1.2 Time-based Media
- [ ] Audio/video has captions (synchronized, complete)
- [ ] Audio/video has transcript (text version of audio)
- [ ] Video has audio description (describes visual content)
- [ ] Auto-play audio is muted or can be paused before 3 seconds
- [ ] Avoid flashing content (no >3 flashes/second in 5°F area)

### 1.3 Adaptable
- [ ] Content doesn't rely on shape/size alone (pair with color, text, icon)
- [ ] Heading structure is logical (h1 → h2 → h3, no skips)
- [ ] Lists are marked as `<ul>`, `<ol>`, `<li>` (not `<br>` or manually indented)
- [ ] Table has header row (`<th>`), data cells (`<td>`), caption
- [ ] Reading order makes sense (tab order = visual order, CSS order not reversed)
- [ ] Zoom to 200% — no loss of content, no horizontal scrolling required

### 1.4 Distinguishable
- [ ] Text on background has ≥4.5:1 contrast ratio (AA) or ≥7:1 (AAA)
- [ ] Large text (18pt+ or 14pt+ bold) has ≥3:1 contrast ratio
- [ ] UI components (buttons, borders) have ≥3:1 contrast
- [ ] Color is not the only means of conveying information (pair with icon, text, pattern)
- [ ] Resize text to 200% — still readable, no loss of functionality
- [ ] No autoplay audio >3 seconds, or provide visible mute button

---

## 2. Operable

### 2.1 Keyboard Accessible
- [ ] All functionality available via keyboard (no mouse-only actions)
- [ ] Tab order is logical (top-to-bottom, left-to-right in LTR languages)
- [ ] Focus visible on all interactive elements (outline, underline, background change)
- [ ] No keyboard traps (users can tab through and past all elements)
- [ ] Skip links provided (e.g., "Skip to main content")
- [ ] Custom widgets have keyboard support (↑↓←→, Home, End, Enter/Space to activate)

### 2.2 Enough Time
- [ ] No session timeout without warning, or timeout is >20 hours
- [ ] If timeout exists, user can extend or disable it
- [ ] Auto-play animations/carousels can be paused
- [ ] Scrolling text/marquee can be paused
- [ ] Interaction can be completed without time limit (or time limit is essential, e.g., timed test)

### 2.3 Seizures & Physical Reactions
- [ ] No content flashes >3 times in 1 second (in any 5° area)
- [ ] Motion triggered by user interaction (not auto-play)

### 2.4 Navigable
- [ ] Every page has a descriptive `<title>` (e.g., "Product Details - Acme Store")
- [ ] Links have descriptive text (e.g., "Learn more about accessibility" not "Click here")
- [ ] Landmark regions: `<main>`, `<nav>`, `<header>`, `<footer>`, `<aside>`
- [ ] Heading hierarchy (h1 → h2 → h3, no skips)
- [ ] Skip link to main content (visible or visible on focus)
- [ ] Focus order is logical and meaningful
- [ ] Purpose of each link is clear from text + context

---

## 3. Understandable

### 3.1 Readable
- [ ] Page language declared in `<html lang="en">`
- [ ] Vocabulary appropriate for audience (avoid jargon without explanation)
- [ ] Abbreviations explained on first use (e.g., "WCAG (Web Content Accessibility Guidelines)")
- [ ] No words that change meaning based on pronunciation

### 3.2 Predictable
- [ ] Navigation is consistent across pages (menu in same place)
- [ ] Interactive elements behave consistently (buttons do same thing everywhere)
- [ ] No unexpected navigation changes on focus or input
- [ ] Changes of context are initiated by user (not automatic)

### 3.3 Input Assistance
- [ ] Form labels associated with inputs via `<label for="">`
- [ ] Error messages identify the problematic field specifically
- [ ] Error messages suggest correction (not just "Invalid")
- [ ] Required fields marked with `aria-required="true"` or visible indicator
- [ ] Form validation on submit, not per character (unless live feedback is helpful)
- [ ] Undo option for important actions (e.g., delete, submit payment)

---

## 4. Robust

### 4.1 Compatible
- [ ] HTML is valid (run through W3C validator)
- [ ] No duplicate IDs on same page
- [ ] Attributes complete (src, href, type where required)
- [ ] Proper nesting (e.g., `<ul><li>` not `<ul><div>`)
- [ ] Opening and closing tags properly matched
- [ ] ARIA attributes used correctly (role, aria-label, aria-required, etc.)
- [ ] Role + Name + Value exposed for all interactive elements

---

## Quick Audit Steps

1. **Automated scan** — Run axe DevTools, WAVE, or Lighthouse (catches ~30%)
2. **Keyboard-only test** — Unplug mouse, navigate entire site. Tab through, check focus, identify traps.
3. **Screen reader test** — Use NVDA/VoiceOver. Can you understand page structure? Are headings logical?
4. **Zoom test** — Zoom to 200%, 400%. Any horizontal scroll? Is content still readable?
5. **Colorblind test** — Use Coblis. Is information conveyed by color also marked another way?
6. **Manual review** — Check forms, error messages, alt text, heading hierarchy against this checklist.

---

## Common Issues & Fixes

| Issue | Check | Fix |
|-------|-------|-----|
| Inaccessible color | Contrast ratio <4.5:1 | Darken text or lighten background |
| Missing focus ring | `outline: none` in CSS | Use `:focus-visible`, not `outline: none` |
| Form no labels | Inputs without `<label>` | Add `<label for="id">` to each field |
| Image no alt | Missing `alt=""` attribute | Add alt text or `alt=""` if decorative |
| Keyboard trap | Can't Tab past modal | Add focus trap (first/last element Tab cycles) |
| No heading hierarchy | h1, then h3, skipping h2 | Use sequential order (h1 → h2 → h3) |
| Color-only info | Red text = error | Add icon, text label, or pattern with color |

---

## Resources

- **WCAG 2.1 Full Spec:** https://www.w3.org/WAI/WCAG21/quickref/
- **WebAIM Contrast:** https://webaim.org/resources/contrastchecker/
- **axe DevTools:** https://www.deque.com/axe/devtools/
- **WAVE:** https://wave.webaim.org/
- **Coblis (Colorblind Simulator):** https://www.color-blindness.com/coblis-color-blindness-simulator/
