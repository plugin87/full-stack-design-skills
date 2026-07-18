# Color Palette Guide

Building an accessible, cohesive color system for digital products.

## Palette Structure: 3-4 Color Ramps

A professional palette has 3–4 base hues, each with a 7-stop ramp (lightest to darkest).

### Example: Neutral Gray Ramp (0-100 scale)

| Stop | Hex     | Lightness | Use Case                          |
|------|---------|-----------|----------------------------------|
| 50   | #F9F8F6 | 98%       | Lightest background, hover state |
| 100  | #F0EEEA | 94%       | Page background, input focus      |
| 200  | #E4E1DB | 89%       | Card background, light fill      |
| 300  | #D3D1C7 | 82%       | Border (default)                  |
| 400  | #B4B2A9 | 71%       | Secondary text, muted labels      |
| 500  | #888780 | 53%       | Placeholder text                  |
| 600  | #5F5E5A | 38%       | Border (strong), secondary text   |
| 700  | #3D3D3A | 24%       | Primary text, headings            |
| 800  | #2C2C2A | 17%       | Strong emphasis, dark mode bg     |
| 900  | #1A1A18 | 10%       | Darkest text, dark mode           |

### Example: Blue Ramp (Accent)

| Stop | Light Mode | Dark Mode | Contrast on 50 | Contrast on 950 |
|------|------------|-----------|----------------|-----------------|
| 50   | #E6F1FB   | #042C53   | Text: 900      | Text: 50        |
| 100  | #B5D4F4   | #0C447C   | Text: 700      | Text: 100       |
| 400  | #378ADD   | -         | Text: 900      | -               |
| 600  | #185FA5   | -         | Text: 50       | -               |
| 700  | #0C447C   | #B5D4F4   | Text: 50       | Text: 900       |
| 900  | #042C53   | #E6F1FB   | Text: 50       | Text: 700       |

**Pattern:** Light mode uses 50-700, dark mode inverts (50↔900, 100↔800, etc.).

## Building Your Palette

### Step 1: Choose Primary Colors

Select 2–4 hues (as hex or HSL):
- **Neutral/Gray** — Primary for text, borders, backgrounds
- **Accent/Primary** — Your brand color for buttons, links, highlights
- **Status colors** — Success (green), Warning (amber), Danger (red)
- Optional: **Secondary brand color**

Example HSL selection:
- Neutral: hsl(30, 8%, 55%) — warm gray
- Accent: hsl(220, 90%, 56%) — bright blue
- Success: hsl(142, 76%, 36%) — vibrant green
- Warning: hsl(38, 92%, 50%) — golden amber
- Danger: hsl(0, 84%, 60%) — crisp red

### Step 2: Generate Ramps

For each hue, create 7–9 stops from lightest (50) to darkest (900).

**Tool-based approach:**
- Use Chroma.js library (`chroma.scale()`)
- Use Ant Design Color Generator
- Use Leonardo (Adobe's color tool)
- Use Tailwind color generator

**Manual approach (for brand colors):**
1. Start with your brand color (usually the 500 or 600 stop — medium saturation/brightness)
2. Lighten by reducing saturation and increasing lightness (50, 100, 200 stops)
3. Darken by increasing saturation and decreasing lightness (700, 800, 900 stops)

### Step 3: Verify Contrast

For each color pair you plan to use, check WCAG contrast:
- Text on background: 4.5:1 minimum (AA)
- UI components: 3:1 minimum
- Large text (18pt+): 3:1 minimum

| Combination       | Light Mode              | Dark Mode             | Contrast Ratio |
|-------------------|-------------------------|----------------------|----------------|
| Text on background| 900 on 50              | 100 on 900           | 15:1+ ✓        |
| Secondary text    | 600 on 100             | 400 on 800           | 7:1+ ✓         |
| Border on bg      | 300 on 50              | 700 on 900           | 8:1+ ✓         |
| Button fill       | Accent 500 bg, white text | Accent 700, white  | 10:1+ ✓        |

### Step 4: Implement as CSS Variables

```css
:root {
  /* Neutral */
  --color-neutral-50: #f9f8f6;
  --color-neutral-100: #f0eeea;
  --color-neutral-200: #e4e1db;
  --color-neutral-300: #d3d1c7;
  --color-neutral-400: #b4b2a9;
  --color-neutral-500: #888780;
  --color-neutral-600: #5f5e5a;
  --color-neutral-700: #3d3d3a;
  --color-neutral-800: #2c2c2a;
  --color-neutral-900: #1a1a18;

  /* Accent (Blue) */
  --color-accent-50: #e6f1fb;
  --color-accent-100: #b5d4f4;
  --color-accent-400: #378add;
  --color-accent-600: #185fa5;
  --color-accent-700: #0c447c;
  --color-accent-900: #042c53;

  /* Semantic */
  --color-success-50: #eaf3de;
  --color-success-500: #639922;
  --color-success-900: #173404;

  --color-warning-50: #faeeda;
  --color-warning-500: #ba7517;
  --color-warning-900: #412402;

  --color-danger-50: #fcebeb;
  --color-danger-500: #e24b4a;
  --color-danger-900: #501313;
}
```

## Dark Mode Strategy

**Option 1: Inverted ramp**
- Light mode: use 50–700
- Dark mode: use 900–200 (reversed)
- Example: `--text-primary` is `neutral-900` light, `neutral-50` dark

**Option 2: Separate system**
- Define unique dark-mode ramps (may not be exact inversions)
- Allows fine-tuning for visual balance
- More maintenance but more control

**Option 3: HSL-based**
- Use JavaScript to shift lightness at runtime
- High maintenance, rarely recommended

**Recommendation:** Use inverted ramps for simplicity + consistency.

## Testing Your Palette

- [ ] Test all text colors on all background colors
- [ ] Test with deuteranopia, protanopia, tritanopia (colorblind modes)
- [ ] Test in dark mode and light mode
- [ ] Compare printed output (colors shift in print)
- [ ] Test on different monitors (calibration varies)
- [ ] Test on mobile (colors look different at different brightness levels)

Use tools:
- Coblis (colorblind simulator)
- WebAIM Contrast Checker
- Chrome DevTools color picker with "Emulate CSS media feature prefers-color-scheme"

## Common Mistakes

**Too many colors:** Results in cluttered palette, hard to maintain. Stick to 3–4 hues.

**Insufficient contrast:** Text at 3:1 ratio looks washed out. Aim for 7:1 for primary text.

**Single-stop ramp:** "We only need one green." Then you need hover, focus, disabled, background — now you have 5 greens. Build the full ramp upfront.

**Forgetting brand:** Every color should serve a purpose. If "we like purple," make purple your secondary accent, not a scattered color.

**Not testing dark mode:** A light gray that works on white may be invisible on near-black. Test both modes equally.
