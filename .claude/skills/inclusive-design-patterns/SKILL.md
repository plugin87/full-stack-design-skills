---
name: inclusive-design-patterns
description: Design accessible and inclusive UI patterns that work for everyone, regardless of ability. Use this skill for accessible color combinations, accessible typography, form patterns with proper labeling, motion and animation safety, icon accessibility, accessible interactive components, multilingual design, and inclusive testing strategies. Triggers include: color contrast and colorblind accessibility, readable typography at all sizes, accessible form design, prefers-reduced-motion handling, accessible icons and buttons, modal and dialog accessibility, internationalization basics, and testing design for accessibility. Essential for UX/UI designers creating products that are usable by people with disabilities, colorblindness, low vision, motor challenges, and different cognitive abilities.
---

# Inclusive Design Patterns

Practical patterns for designing interfaces that work for everyone, including people with disabilities.

Accessibility isn't a nice-to-have feature—it's a core design principle. Inclusive design means designing from the start for people with disabilities, rather than retrofitting later. This benefits everyone: captions help in noisy environments, large text helps when tired, and clear navigation helps when distracted.

---

## Core Principle: Design for Constraints

### Who Benefits from Accessible Design?

**People with permanent disabilities:**
- Low vision (reduce text size? Problem. Use zoom + readable fonts.)
- Colorblindness (red/green can't distinguish? Use patterns + labels.)
- Motor impairments (can't use mouse? Keyboard navigation needed.)
- Deaf/hard of hearing (video has no captions? Can't understand.)
- Cognitive differences (complex language? Use clear, simple language.)

**People in temporary situations:**
- Broken arm (can't use mouse or type)
- Temporary vision loss (bright sun, dark room)
- Noisy environment (can't hear audio)
- Slow internet (images load slowly)
- Interrupted attention (loud office)

**People in situational constraints:**
- On mobile (small screen, touch, distraction)
- Older adults (presbyopia, tremor, slower reflexes)
- Non-native speakers (complex language is harder)
- Low literacy (jargon and long sentences confusing)

**All of the above makes your product better for everyone.**

---

## Accessible Color Design

### Color Contrast: WCAG Standards

Text must have sufficient contrast to be readable. WCAG defines two levels:

**WCAG AA (minimum recommended):**
- Regular text (< 18px): 4.5:1 contrast ratio
- Large text (≥ 18px bold or ≥ 24px regular): 3:1 contrast ratio

**WCAG AAA (enhanced, for sensitive content):**
- Regular text: 7:1 contrast ratio
- Large text: 4.5:1 contrast ratio

### Contrast Examples

```
✅ Good: Black text on white background
- Contrast: 21:1 (very high)
- Passes WCAG AAA

✅ Good: Dark gray (#333) on white
- Contrast: 12.6:1
- Passes WCAG AAA

⚠️ Borderline: Medium gray (#666) on white
- Contrast: 3.3:1
- Fails WCAG AA for regular text
- Passes only for large text (18px+)

❌ Bad: Light gray (#ccc) on white
- Contrast: 1.4:1
- Fails all standards

❌ Bad: Red text on blue background
- Visually colorful but low contrast
- Fails for colorblind users
```

### Color + Contrast Tools

**Accessible color checkers:**
- WebAIM Contrast Checker (webaim.org/resources/contrastchecker/)
- Chrome DevTools (automatic contrast checking)
- WAVE browser extension
- Accessible Colors (accessible-colors.com)

**Colorblind simulation:**
- Chrome DevTools → Rendering → Emulate vision deficiencies
- Options: Protanopia, Deuteranopia, Tritanopia, Achromatopsia
- Figma plugins: Color Blindness, A11y

### Using Color Without Relying on Color

**Problem:** Status indicator is red for error, green for success.
- Colorblind users can't distinguish red from green.

**Solution:** Add text label + icon + color
```
✅ Good:
┌──────────────────────┐
│ ✓ Account verified   │ Green background + text label + icon
└──────────────────────┘

✅ Better:
┌──────────────────────┐
│ ✓ Account verified   │ Color + text + icon (redundant cues)
└──────────────────────┘

❌ Bad:
┌──────────────────────┐
│           [GREEN]    │ Color only, no label
└──────────────────────┘
```

**Implementation:**
- Never use color alone to convey information
- Always add text label, icon, or pattern
- Test with colorblind simulation

### Dark Mode Contrast

Dark mode must maintain contrast standards.

```css
/* Light mode */
body {
  background: white;
  color: #1e293b; /* Dark text on white */
}

/* Light text on dark background */
@media (prefers-color-scheme: dark) {
  body {
    background: #1e293b;
    color: #f1f5f9; /* Light text on dark */
  }
}

/* Check contrast: #f1f5f9 on #1e293b = 12.6:1 (passes WCAG AAA) */
```

---

## Accessible Typography

### Font Size & Readability

**Minimum font sizes:**
- Body text: 14px minimum (16px better)
- Captions/labels: 12px minimum
- Never go below 12px for extended reading

**Reasoning:**
- 12px is smallest readable by most people
- 16px is optimal for body text (easy to read, comfortable)
- Older adults need larger text (18px+)

### Line Length for Readability

Optimal line length: 45-75 characters.

**Too short (< 45 chars):**
```
This is very short
line length text that
requires many line
breaks and interrupts
reading flow.
```
Result: Tiring to read, hard to track line-to-line.

**Optimal (50-70 chars):**
```
This is an optimal line length that allows the reader
to focus without jumping too far between line endings
and beginnings. It's comfortable and doesn't require
too many line breaks or leave too much whitespace.
```
Result: Easy, comfortable reading.

**Too long (> 75 chars):**
```
This is very long line length text that makes it hard for the reader to track from the end of one line to the beginning of the next, especially for people with low vision or dyslexia who might lose their place.
```
Result: Hard to track, tiring.

### Line Height for Readability

Minimum line height: 1.5 (150%).

```css
body {
  line-height: 1.5; /* 150% of font size */
  /* For 16px font, this is 24px line height */
}

/* Heading can be tighter */
h1 {
  line-height: 1.3; /* 130% of font size */
}

/* Very small text needs more space */
small, .caption {
  line-height: 1.6; /* 160% of font size */
}
```

### Letter Spacing for Readability

For people with dyslexia, increased letter spacing helps.

```css
/* Default (tight) */
.text {
  letter-spacing: 0;
}

/* Better for readability */
.text {
  letter-spacing: 0.05em; /* Slight increase */
}

/* Dyslexia-friendly */
.text.dyslexia-friendly {
  letter-spacing: 0.1em;
  word-spacing: 0.16em;
  line-height: 1.8;
}
```

### Font Choice for Readability

**Good fonts for readability:**
- Sans-serif: Inter, Arial, Helvetica (clean, modern)
- Serif: Georgia, Times New Roman (traditional, familiar)
- Dyslexia-friendly: Dyslexie, Open Dyslexic (specialized)

**Avoid:**
- Thin/ultra-light weights (hard to read)
- All-caps (harder to scan)
- Decorative fonts for body text

### Avoiding Text Justification

Justified text (align both edges) creates uneven spacing that's hard to read.

```
❌ Justified:
The quick brown fox jumps  over  the  lazy  dog
to escape the hunters and make a great escape.

✅ Left-aligned:
The quick brown fox jumps over the lazy dog to
escape the hunters and make a great escape.
```

**CSS:**
```css
/* Bad: justified text */
body { text-align: justify; }

/* Good: left-aligned (natural reading flow) */
body { text-align: left; }
```

---

## Accessible Forms

### Proper Label Association

Labels must be linked to inputs with `<label>` element and `for` attribute.

```html
<!-- ❌ Bad: Label not associated -->
<label>Email</label>
<input type="email" id="email">

<!-- ✅ Good: Label linked to input -->
<label for="email">Email</label>
<input type="email" id="email">

<!-- ✅ Good: Label wraps input -->
<label>
  Email
  <input type="email">
</label>
```

**Why it matters:**
- Screen readers announce: "Email input"
- Clicking label focuses input (larger tap target on mobile)
- Keyboard navigation works correctly

### Required Field Indicators

Indicate required fields clearly, with text + visual.

```html
<!-- ❌ Bad: Asterisk only (meaning unclear) -->
<label for="email">Email *</label>
<input type="email" id="email" required>

<!-- ✅ Good: Text + asterisk -->
<label for="email">Email <span aria-label="required">*</span></label>
<input type="email" id="email" required aria-required="true">

<!-- ✅ Better: Text at top of form -->
<p><span aria-label="required">*</span> indicates required field</p>
<label for="email">Email <span aria-label="required">*</span></label>
<input type="email" id="email" required aria-required="true">
```

### Error Messages

Error messages must be:
1. Visible (red text, icon)
2. Associated with input (linked with aria-describedby)
3. Clear and actionable (not vague)

```html
<!-- ❌ Bad: Just turns input red -->
<input type="email" id="email" style="border: 2px solid red;">

<!-- ✅ Good: Text + visual + linked to input -->
<label for="email">Email</label>
<input 
  type="email" 
  id="email"
  aria-describedby="email-error"
  aria-invalid="true"
>
<div id="email-error" role="alert">
  Please enter a valid email (e.g., user@example.com)
</div>

<style>
  input[aria-invalid="true"] {
    border: 2px solid #ef4444;
  }
  #email-error {
    color: #ef4444;
    font-size: 14px;
    margin-top: 4px;
  }
</style>
```

### Helper Text & Hints

Provide context for complex fields.

```html
<label for="password">Password</label>
<input 
  type="password" 
  id="password"
  aria-describedby="password-hint"
  required
>
<div id="password-hint">
  At least 8 characters, including uppercase, lowercase, and number
</div>
```

---

## Motion & Animation Accessibility

### Prefers-Reduced-Motion

Some users experience motion sickness, vertigo, or seizures from animations. Respect `prefers-reduced-motion` setting.

**CSS media query:**
```css
/* Animated (default) */
.card {
  transition: transform 300ms ease;
}

.card:hover {
  transform: translateY(-4px);
}

/* Respect user preference for reduced motion */
@media (prefers-reduced-motion: reduce) {
  .card {
    transition: none; /* Remove animation */
  }

  .card:hover {
    transform: none; /* Remove transform */
  }
}
```

**Users who need this:**
- Vestibular disorders (inner ear issues → motion sickness)
- Epilepsy (animations can trigger seizures)
- Migraine (motion can trigger migraines)
- Autism spectrum (sensory overload from movement)

**Testing:**
- Mac: System Preferences → Accessibility → Display → Reduce motion
- Windows: Settings → Ease of Access → Display → Show animations
- Chrome DevTools → Rendering → Emulate CSS media feature prefers-reduced-motion

### Safe Animation Guidelines

If animations must exist:

**Safe:**
- Fade in/out (opacity changes)
- Color changes
- Slow transitions (300ms+)
- Linear motion (predictable)

**Avoid:**
- Rapid flashing (> 3 flashes per second = seizure risk)
- Large parallax effects (disorienting)
- Auto-playing video/audio
- Unexpected animations (user didn't trigger)

### Auto-Playing Content

Never auto-play video or audio. Users should control.

```html
<!-- ❌ Bad: Auto-plays when page loads -->
<video autoplay>
  <source src="video.mp4" type="video/mp4">
</video>

<!-- ✅ Good: User controls with play button -->
<video controls>
  <source src="video.mp4" type="video/mp4">
</video>
```

---

## Accessible Icons

### Icon + Text Labels

Never use icons alone for important functions. Combine with text.

```html
<!-- ❌ Bad: Icon only, no label -->
<button>🔍</button>

<!-- ✅ Good: Icon + text label -->
<button>
  <span class="icon">🔍</span>
  <span>Search</span>
</button>

<!-- ✅ Good: Icon with aria-label -->
<button aria-label="Search">
  <span class="icon">🔍</span>
</button>
```

### SVG Icon Accessibility

SVG icons need labels.

```html
<!-- ❌ Bad: No label -->
<svg class="icon" viewBox="0 0 24 24">
  <path d="..."></path>
</svg>

<!-- ✅ Good: With title -->
<svg class="icon" viewBox="0 0 24 24" aria-label="Search">
  <title>Search</title>
  <path d="..."></path>
</svg>

<!-- ✅ Good: With aria-label -->
<svg class="icon" viewBox="0 0 24 24" aria-label="Home">
  <path d="..."></path>
</svg>
```

---

## Accessible Interactive Components

### Buttons

Buttons must be:
- Keyboard-focusable (visible focus indicator)
- Proper semantic element (not styled div)
- Large enough (48px minimum on mobile)
- Clear label (text or aria-label)

```html
<!-- ❌ Bad: Styled div, not keyboard accessible -->
<div onclick="action()" style="cursor: pointer;">Click me</div>

<!-- ✅ Good: Proper button element -->
<button onclick="action()">Click me</button>

<!-- ✅ Good: Button with aria-label (icon only) -->
<button aria-label="Close">×</button>
```

### Links

Links should be distinguishable (underline or color + underline).

```css
/* ❌ Bad: Color only, hard to spot in text */
a {
  color: #2563eb;
  text-decoration: none;
}

/* ✅ Good: Underlined for clarity */
a {
  color: #2563eb;
  text-decoration: underline;
}

/* ✅ Good: Color change on hover/focus */
a {
  color: #2563eb;
  text-decoration: underline;
}

a:hover, a:focus {
  color: #1d4ed8;
  background: #dbeafe;
}
```

### Focus Indicators

All interactive elements need visible focus indicators.

```css
/* ❌ Bad: No focus indicator */
button:focus {
  outline: none;
}

/* ✅ Good: Visible outline */
button:focus {
  outline: 2px solid #2563eb;
  outline-offset: 2px;
}

/* ✅ Good: Custom focus style */
button:focus {
  box-shadow: 0 0 0 3px #dbeafe; /* Blue glow */
}
```

---

## Multilingual & Internationalization

### Language Declaration

Always declare the page language.

```html
<html lang="en">
  <!-- English content -->
</html>

<!-- Or multilingual sections -->
<html lang="en">
  <div lang="es">Hola mundo</div>
  <div lang="th">สวัสดีชาวโลก</div>
</html>
```

### Text Direction (Right-to-Left)

Some languages flow right-to-left (Arabic, Hebrew).

```html
<html lang="ar" dir="rtl">
  <!-- Content flows right to left -->
</html>
```

### Readable Language

Use clear, simple language for all users:
- Non-native speakers
- People with cognitive differences
- Older adults
- People in a hurry

**Bad (jargon-heavy):**
"Utilize our proprietary interface to facilitate seamless integration with your existing infrastructure."

**Good (clear, simple):**
"Connect easily to your current system."

---

## Testing Accessible Designs

### Automated Accessibility Audit

Use tools to catch common issues.

**Browser tools:**
- Chrome DevTools (Accessibility audit)
- WAVE browser extension
- Axe DevTools browser extension

**Command-line tools:**
- Pa11y (automated accessibility testing)
- Lighthouse CI (includes a11y scoring)

### Manual Testing

Automated tools catch ~30% of issues. Manual testing catches the rest.

**Manual checks:**
- Keyboard navigation (Tab through all interactive elements)
- Screen reader testing (VoiceOver on Mac, NVDA on Windows)
- Color contrast (WebAIM Contrast Checker)
- Colorblind simulation (Chrome DevTools)
- Form testing (labels, errors, required fields)
- Focus indicators (visible on all elements)

### Real User Testing

Include people with disabilities in testing.

- Users with low vision
- Users who are Deaf
- Users with motor impairments
- Users with cognitive differences
- Users who speak English as second language

Their insights catch issues that automated/manual testing misses.

---

## When to Use This Skill

✅ **Do use for:**
- Accessible color combinations and contrast
- Accessible typography sizing and spacing
- Form design with proper labeling and error handling
- Motion and animation safety (prefers-reduced-motion)
- Icon accessibility and labeling
- Accessible interactive patterns (buttons, links, focus)
- Multilingual and international design
- Accessible design testing strategies
- Inclusive design principles and patterns

❌ **Don't use for:**
- Semantic HTML (see web-accessibility-a11y)
- ARIA implementations (see web-accessibility-a11y)
- Screen reader optimization (see web-accessibility-a11y)
- Testing automation frameworks (see qa-testing-visual-regression)
- Design system architecture (see design-systems-architecture)
