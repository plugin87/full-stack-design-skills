# Accessible Color & Contrast Patterns

Practical patterns for designing with accessible colors and sufficient contrast.

---

## Understanding Contrast Ratio

Contrast ratio is calculated as: (L1 + 0.05) / (L2 + 0.05)
- L1 = lighter color's luminance
- L2 = darker color's luminance
- Result ranges from 1:1 (no contrast) to 21:1 (maximum contrast)

### WCAG Standards

**WCAG AA (standard, minimum):**
- Regular text (< 18px): 4.5:1
- Large text (≥ 18px bold or ≥ 24px): 3:1
- UI components (borders, icons): 3:1

**WCAG AAA (enhanced):**
- Regular text: 7:1
- Large text: 4.5:1

**Most websites should aim for WCAG AA.** AAA is for specialized content (legal, medical, educational).

---

## Text Color Contrast Patterns

### Pattern 1: Dark Text on Light Background

**Most common pattern for readability.**

```
Background: White (#ffffff)
Text: Black (#000000)
Contrast: 21:1 (excellent)
Status: ✅ Passes WCAG AAA
```

**Variations (all passing AA):**

| Background | Text | Ratio | AA? | AAA? |
|-----------|------|-------|-----|------|
| #ffffff | #000000 | 21:1 | ✅ | ✅ |
| #ffffff | #333333 | 12.6:1 | ✅ | ✅ |
| #ffffff | #666666 | 3.3:1 | ❌ | ❌ |
| #f1f5f9 | #1e293b | 13.5:1 | ✅ | ✅ |

**Rule:** On white, use text darker than #888888 for regular text, #999999 for large text.

### Pattern 2: Light Text on Dark Background

**For dark mode, night mode, overlays.**

```
Background: Dark (#1e293b)
Text: Light (#f1f5f9)
Contrast: 12.6:1 (excellent)
Status: ✅ Passes WCAG AAA
```

**Variations:**

| Background | Text | Ratio | AA? | AAA? |
|-----------|------|-------|-----|------|
| #000000 | #ffffff | 21:1 | ✅ | ✅ |
| #1e293b | #f1f5f9 | 12.6:1 | ✅ | ✅ |
| #334155 | #f1f5f9 | 9.2:1 | ✅ | ✅ |
| #475569 | #f1f5f9 | 5.5:1 | ✅ | ✅ |

**Rule:** On dark backgrounds, use text lighter than #999999.

### Pattern 3: Color + Contrast

For colored backgrounds, ensure text contrast.

```
❌ Bad: Blue background (#2563eb) + dark text
- Contrast: 2.8:1
- Fails all standards
- Hard to read

✅ Good: Blue background (#2563eb) + white text
- Contrast: 3.5:1
- Passes WCAG AA (large text)
- Easy to read
```

**CSS:**
```css
.blue-button {
  background: #2563eb;
  color: white; /* Ensure white text, not dark */
  padding: 12px 16px;
  border-radius: 4px;
}
```

---

## Color Status Indicators (No Color Alone)

Status colors (success, warning, error) shouldn't rely on color alone.

### Pattern 1: Color + Icon

```html
<!-- ❌ Bad: Color only -->
<div style="background: #10b981;">Success!</div>

<!-- ✅ Good: Color + icon -->
<div style="background: #10b981; display: flex; gap: 8px;">
  <span>✓</span>
  <span>Success!</span>
</div>
```

### Pattern 2: Color + Text

```html
<!-- ✅ Good: Different background + label -->
<div style="background: #dcfce7; color: #166534; padding: 12px;">
  ✓ Account verified
</div>

<div style="background: #fee2e2; color: #991b1b; padding: 12px;">
  ✗ Error uploading file
</div>

<div style="background: #fef3c7; color: #92400e; padding: 12px;">
  ⚠ Please review before submitting
</div>
```

### Pattern 3: Icons + Labels for All States

```
Success:  ✓ Label + green background
Warning:  ⚠ Label + yellow background
Error:    ✗ Label + red background
Info:     ℹ Label + blue background
```

**Implementation:**
```html
<div class="alert alert-success">
  <span class="alert-icon">✓</span>
  <span class="alert-text">Successfully saved</span>
</div>

<div class="alert alert-error">
  <span class="alert-icon">✗</span>
  <span class="alert-text">Error: Please try again</span>
</div>
```

---

## Colorblind-Safe Palettes

People with colorblindness have difficulty distinguishing certain colors.

### Types of Colorblindness

**Protanopia (Red-Blind):**
- Can't distinguish red from green
- 1% of males affected

**Deuteranopia (Green-Blind):**
- Can't distinguish red from green
- 1% of males affected

**Tritanopia (Blue-Yellow Blind):**
- Can't distinguish blue from yellow
- Rare (< 0.001% of people)

**Achromatopsia (Color-Blind):**
- See only grayscale
- Extremely rare (< 0.003%)

### Safe Color Combinations

**For red-green colorblind users (most common):**

✅ Safe:
- Blue + Orange
- Blue + Yellow
- Dark Blue + Light Blue
- Purple + Yellow

❌ Avoid:
- Red + Green
- Light Red + Light Green

**For all colorblind users:**

✅ Always safe:
- Black + White
- Dark + Light (any colors, if sufficient contrast)
- Blue + Yellow
- Blue + Orange

### Using Patterns + Color

Combine color with patterns/textures for better distinction.

```html
<!-- ✅ Good: Color + pattern -->
<div class="status-success">
  <span class="pattern pattern-checkmark"></span>
  Success
</div>

<div class="status-error">
  <span class="pattern pattern-x"></span>
  Error
</div>
```

**CSS:**
```css
.status-success {
  background-color: #10b981; /* Green */
  background-image: repeating-linear-gradient(
    45deg,
    transparent,
    transparent 10px,
    rgba(255, 255, 255, 0.5) 10px,
    rgba(255, 255, 255, 0.5) 20px
  );
}

.status-error {
  background-color: #ef4444; /* Red */
  background-image: repeating-linear-gradient(
    -45deg,
    transparent,
    transparent 10px,
    rgba(255, 255, 255, 0.5) 10px,
    rgba(255, 255, 255, 0.5) 20px
  );
}
```

### Testing for Colorblindness

**Tools:**
- Chrome DevTools → Rendering → Emulate vision deficiencies
- Figma plugins: Color Blindness Simulator, A11y
- Online: Vischeck, Coblis

**Process:**
1. Enable colorblind simulation
2. View your design
3. Can you still distinguish elements?
4. Adjust colors/patterns if needed

---

## Accessible Color Palettes

### Palette Pattern 1: Neutral + Accent

**Works for most interfaces.**

```
Neutrals:
├── Black: #000000
├── Dark Gray: #1e293b
├── Medium Gray: #64748b
├── Light Gray: #cbd5e1
└── White: #ffffff

Accent (Blue):
├── Primary: #2563eb
├── Hover: #1d4ed8
└── Light: #dbeafe

Status colors:
├── Success: #10b981 (text on white: 6.5:1) ✅
├── Warning: #d97706 (text on white: 4.2:1) ✅
└── Error: #dc2626 (text on white: 4.0:1) ✅
```

### Palette Pattern 2: High Contrast

**For users with low vision.**

```
Background: #ffffff
Text: #000000 (21:1 contrast)

Accent: #0000ff (pure blue)
├── Dark state: #000080
└── Light state: #0080ff

Status:
├── Success: #008000 (strong green)
├── Warning: #ff8000 (strong orange)
└── Error: #ff0000 (strong red)
```

### Palette Pattern 3: Dark Mode Safe

**Ensures contrast in both light and dark modes.**

```
Light mode:
├── Background: #ffffff
├── Text: #000000 (21:1)
└── Accent: #0052cc

Dark mode:
├── Background: #0f172a
├── Text: #f1f5f9 (16:1)
└── Accent: #60a5fa
```

**CSS:**
```css
:root {
  --bg-primary: #ffffff;
  --text-primary: #000000;
  --accent: #0052cc;
}

@media (prefers-color-scheme: dark) {
  :root {
    --bg-primary: #0f172a;
    --text-primary: #f1f5f9;
    --accent: #60a5fa;
  }
}

body {
  background: var(--bg-primary);
  color: var(--text-primary);
}

a {
  color: var(--accent);
}
```

---

## Links & Interactive Elements

### Link Colors

Links must be distinguishable from regular text.

```html
<!-- ❌ Bad: Link color same as text -->
<p>Visit our <a href="/">website</a> for more info.</p>

<style>
  a { color: #1e293b; } /* Same as text */
</style>

<!-- ✅ Good: Link color distinct -->
<p>Visit our <a href="/">website</a> for more info.</p>

<style>
  a { 
    color: #0052cc; /* Blue, distinct from text */
    text-decoration: underline;
  }
</style>
```

**Checklist:**
- [ ] Link color is distinct from body text (not just color, use underline too)
- [ ] Focus state is visible (outline or glow)
- [ ] Visited links are marked (different color is optional)
- [ ] Contrast is sufficient (4.5:1 for AA)

---

## Button & Control Contrast

Buttons and controls need sufficient contrast.

```html
<!-- ❌ Bad: Light button on light background -->
<button style="background: #f0f0f0; border: 1px solid #e0e0e0;">
  Click me
</button>
<!-- Contrast: 1.5:1 (fails) -->

<!-- ✅ Good: High contrast -->
<button style="background: #0052cc; color: white; border: none;">
  Click me
</button>
<!-- Contrast: 8.6:1 (passes AAA) -->

<!-- ✅ Good: Dark button on light background -->
<button style="background: #1e293b; color: white;">
  Click me
</button>
<!-- Contrast: 12.6:1 (passes AAA) -->
```

### Focus Indicator Contrast

Focus indicators (outlines) also need contrast.

```css
/* ❌ Bad: Focus outline blends in */
button:focus {
  outline: 2px solid #ccc; /* Light gray (low contrast) */
}

/* ✅ Good: Focus outline contrasts */
button:focus {
  outline: 3px solid #0052cc; /* Bold blue */
  outline-offset: 2px;
}

/* ✅ Good: Focus box shadow */
button:focus {
  box-shadow: 0 0 0 3px #0052cc;
}
```

---

## Testing Color Contrast

### Manual Testing Steps

1. **Use WebAIM Contrast Checker**
   - webAIM.org/resources/contrastchecker/
   - Enter foreground and background colors
   - Check ratios for AA and AAA

2. **Chrome DevTools**
   - Right-click element → Inspect
   - Elements panel → Styles
   - Click color swatch
   - DevTools shows contrast ratio

3. **WAVE Extension**
   - Browser extension for accessibility
   - Highlights contrast errors
   - Shows exact ratios

4. **Figma Plugins**
   - "A11y - Contrast Checker"
   - "Color Blindness Simulator"
   - Test directly in design file

### Automated Testing

```bash
# Using Lighthouse
# Chrome DevTools → Lighthouse → Audit
# Reports on contrast issues

# Using Pa11y
npm install -g pa11y
pa11y https://yoursite.com
```

---

## Color Accessibility Checklist

- [ ] Text contrast: 4.5:1 (AA) or 7:1 (AAA)
- [ ] Large text contrast: 3:1 (AA) or 4.5:1 (AAA)
- [ ] UI component contrast: 3:1
- [ ] Color not only means (icon, text, or pattern also used)
- [ ] Status colors use redundant cues (color + icon/text)
- [ ] Links are underlined (not color-only)
- [ ] Focus indicators have contrast
- [ ] Dark mode has sufficient contrast
- [ ] Tested with colorblind simulator
- [ ] Tested with contrast checker tool

---

## Common Color Mistakes

### ❌ Gray text on white

```css
p { color: #999999; } /* Fails contrast */
```

Solution: Use darker gray (#666666 or darker).

### ❌ Green + Red for status

Only colorblind users can't distinguish.

Solution: Add text label + icon, not color alone.

### ❌ Light blue focus outline

```css
button:focus { outline: 2px solid #add8e6; }
```

Solution: Use darker blue (#0052cc), larger outline (3px).

### ❌ No focus indicator

```css
button:focus { outline: none; }
```

Solution: Add outline or box-shadow.

### ❌ Relying on hover only

On touch devices, hover doesn't exist.

Solution: Use focus states, active states, not hover alone.
