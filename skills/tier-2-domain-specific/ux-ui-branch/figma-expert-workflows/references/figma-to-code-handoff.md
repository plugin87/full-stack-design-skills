# Figma to Code Handoff: Designer → Developer

Practical handoff process and checklist for moving from Figma design to code implementation.

---

## The Handoff Process

### Traditional (Manual, Error-Prone)

```
Designer creates designs in Figma
↓
Designer exports screenshots / PDFs
↓
Developer reads specs manually
↓
Developer codes based on screenshots
↓
Discrepancies discovered in QA
↓ (Rework)
```

### Modern (Automated, Consistent)

```
Designer creates designs with components, tokens, variables in Figma
↓
Designer publishes shared library
↓
Design tokens sync to codebase automatically
↓
Developer codes using components + tokens from Figma
↓
No discrepancies (source of truth: Figma)
```

---

## Pre-Handoff Checklist

### Design Quality

- [ ] All designs use shared components (no duplicates)
- [ ] All colors are tokenized (color/primary, not #2563eb)
- [ ] All spacing uses tokens (space/md, not 16px)
- [ ] All typography uses shared styles (typography/body, not manual font)
- [ ] Dark mode is designed and tested
- [ ] Responsive breakpoints are defined
- [ ] All interactions are prototyped
- [ ] Accessibility annotations added (ARIA roles, labels)

### Component Naming

- [ ] Component names match codebase (Button, not BTN)
- [ ] Component variants are named consistently (Size/Large, Style/Primary)
- [ ] Instance names are descriptive (not "Component copy 1")
- [ ] Pages are organized and named (not "Page 1", "Page 2")

### Spec Generation

- [ ] Component specs exported (dev mode → Inspect panel)
- [ ] Spacing spec available (margin, padding values)
- [ ] Typography spec available (font family, size, weight, line height)
- [ ] Color spec available (token names + hex values)
- [ ] Responsive behavior documented (mobile, tablet, desktop breakpoints)

### Documentation

- [ ] Cover page exists with:
  - Design system overview
  - Link to Figma component specs
  - Link to design token documentation
  - Link to developer setup guide
  - Contact info for design team
- [ ] Component README exists for each major component (usage, do's/don'ts)
- [ ] Prototypes linked (for interaction reference)
- [ ] Known limitations documented

---

## During Handoff

### Developer Onboarding

**What developers need:**

1. **Access to Figma file**
   - Share link or add to team
   - Grant "Developer" permission (read-only)
   - Ensure they know how to use Inspect panel

2. **Design system documentation**
   - How to access components
   - How to use design tokens
   - Responsive breakpoints
   - Accessibility requirements

3. **Development setup**
   - How to install design tokens
   - Where tokens live in codebase
   - Build commands
   - Testing procedures

### Developer Workflow

```
1. Open Figma design
2. Use Inspect panel to see specs
3. Check design tokens documentation
4. Code component using:
   - Token names for colors, spacing, typography
   - Figma component specs for dimensions
   - Figma variables for responsive behavior
5. Test against Figma design
6. Create pull request
7. Design review (using Figma comments)
```

### Design Review

**Setup Figma for comments:**
1. Share dev/staging URL in Figma design comment
2. Developer adds live component to Figma (embed or screenshot)
3. Designer reviews and comments
4. Developer iterates until approved

---

## Post-Handoff

### Keeping Code & Design in Sync

**Problem:** Design updates but code doesn't.
**Solution:** Automate sync where possible.

**Automated:**
- ✅ Design tokens sync (via Tokens Studio or similar)
- ✅ Color updates (CSS variables update automatically)
- ✅ Spacing updates (token references update)

**Manual review:**
- ⚠️ Component structure changes (require code refactor)
- ⚠️ New variants (may need code adjustments)
- ⚠️ Responsive breakpoint changes (require CSS updates)

### Version Communication

When design changes:

```
Figma: Button component updated
- Old: Style/Primary only
- New: Style/Primary, Secondary, Ghost

Email to devs:
Subject: Button component updated in Figma
Body:
---
Button component has new variants:
- Style/Primary (existing)
- Style/Secondary (new)
- Style/Ghost (new)

Code: These correspond to button[variant="secondary"] and button[variant="ghost"]

If you're using Button, no changes needed.
If you want to use new variants, import latest component.

Full specs: [Figma link]
---
```

---

## Handoff Artifacts

### 1. Component Specs Document

Auto-generated from Figma (Inspect panel):

```
Button
├── Dimensions: 200 × 44
├── Padding: 12px vertical, 16px horizontal
├── Corner radius: 8px
├── Border: none
├── Shadow: none (or 0 2px 8px rgba(0,0,0,0.1) for hover)
├── Typography:
│   ├── Font family: Inter
│   ├── Font size: 16px
│   ├── Font weight: 600
│   ├── Line height: 1.5
├── Fills:
│   ├── Primary: color/primary (#2563eb)
│   ├── Hover: color/primary-hover (#1d4ed8)
│   └── Disabled: #9ca3af
└── Instances: Button in 36 places (Dashboard, Settings, etc.)
```

### 2. Design Token Export

```json
{
  "color": {
    "primary": "#2563eb",
    "primary-hover": "#1d4ed8",
    "success": "#10b981",
    "warning": "#f59e0b",
    "danger": "#ef4444"
  },
  "space": {
    "xs": 4,
    "sm": 8,
    "md": 16,
    "lg": 24,
    "xl": 32
  },
  "typography": {
    "body": { "size": 16, "weight": 400, "lineHeight": 1.5 },
    "heading": { "size": 24, "weight": 600, "lineHeight": 1.3 }
  }
}
```

### 3. Responsive Breakpoints

```
Mobile: max-width 640px
- Single column
- Full-width buttons
- Stacked navigation

Tablet: 641px - 1024px
- 2 columns
- Medium buttons
- Horizontal navigation

Desktop: 1025px +
- 3-4 columns
- Full-width layouts
- All features visible
```

### 4. Accessibility Notes

```
Button
- Semantic: <button> element (not <div>)
- ARIA: aria-label if icon-only
- Keyboard: Tab to focus, Enter to activate
- Focus indicator: 2px solid outline
- Color contrast: 4.5:1 minimum (WCAG AA)

Form field
- Label: Always visible <label> with htmlFor
- Placeholder: Hint text, not label
- Error: aria-describedby with error message
- Focus: Border color change, 2px outline
```

---

## Common Handoff Issues & Solutions

### ❌ Issue: "This spacing isn't right"

**Root cause:** Developer guessed spacing from screenshot.

**Solution:**
- Use Figma Inspect panel (shows exact values)
- Use design tokens (no guessing)
- Test against Figma design side-by-side

### ❌ Issue: "The color looks different"

**Root cause:** Hex value copied wrong, or monitor color profile different.

**Solution:**
- Use design tokens (color/primary, not #2563eb)
- Test on multiple monitors
- Use Figma's color picker to verify

### ❌ Issue: "I don't know which component to use"

**Root cause:** Component options unclear or poorly named.

**Solution:**
- Create component README (when to use each variant)
- Add examples showing correct usage
- Link from Figma to Storybook

### ❌ Issue: "Design changed but code wasn't updated"

**Root cause:** No automated sync; manual process breaks down.

**Solution:**
- Automate token sync (Design Tokens Bridge, Tokens Studio)
- Email notifications when design changes
- Use Figma Webhooks to trigger CI/CD

---

## Handoff Checklist: Final Review

### Designer Checklist (Before handoff)

- [ ] All components are variants (not separate components)
- [ ] All components use auto layout
- [ ] All colors are tokenized
- [ ] All spacing uses tokens
- [ ] All typography uses shared styles
- [ ] Components are locked (prevent edits)
- [ ] Naming is consistent with code
- [ ] Dark mode is designed
- [ ] Responsive behavior documented
- [ ] Accessibility notes added
- [ ] Figma specs are accurate
- [ ] Cover page has all resources

### Developer Checklist (After handoff)

- [ ] Figma access granted
- [ ] Component specs reviewed
- [ ] Design tokens installed in codebase
- [ ] Responsive breakpoints understood
- [ ] Accessibility requirements noted
- [ ] Design system documentation read
- [ ] Build environment set up
- [ ] First component built as reference
- [ ] Design review process agreed upon

### Team Checklist (Ongoing)

- [ ] Design token sync automated
- [ ] Weekly sync between design and engineering
- [ ] Component usage tracked (is code using Figma components?)
- [ ] Discrepancies resolved within 48 hours
- [ ] Breaking changes communicated proactively
- [ ] Documentation kept up-to-date

---

## Tools for Better Handoff

### Browser Plugins
- **Figma to HTML:** Export frames as HTML
- **Measure:** Measure distances in Figma
- **Figma to React:** Export components as React code (experimental)

### Workflow Tools
- **Storybook:** Visual documentation with Figma embeds
- **Zeroheight:** Beautiful design documentation with Figma live
- **Chromatic:** Visual testing with Storybook
- **Abstract:** Version control for design files

### Communication
- **Figma comments:** In-context feedback
- **Slack integration:** Figma → Slack notifications
- **GitHub integration:** Link Figma to issues

---

## Handoff Template Email

```
Subject: Design Handoff: [Feature Name]

Hi team,

The [Feature Name] design is ready for handoff. Here's what you need:

**Design Specs:**
- Figma file: [link]
- Component overview: [link]
- Responsive breakpoints: [link]
- Accessibility notes: [link]

**Design Tokens:**
- Token export: [link to tokens.json]
- Setup guide: [link to docs]
- Sync status: Automated (syncs on commit)

**Getting Started:**
1. Add yourself to Figma team
2. Review component specs in Inspect panel
3. Install tokens in codebase: npm run tokens:sync
4. Start with Button component as reference

**Questions?**
- Figma specs: Use Inspect panel
- Tokens: See token documentation
- Accessibility: Check accessibility notes in Figma
- General: Reach out to [designer]

**Timeline:**
- Design review: [date]
- Dev launch: [date]
- Production release: [date]

Thanks,
[Designer]
```

---

## Handoff Success Metrics

Track these to know if handoff is working:

- ✅ Code matches Figma design (no discrepancies)
- ✅ Token sync is automated (manual updates ≤ 1x/month)
- ✅ Design changes communicated < 24 hours
- ✅ Design review time ≤ 2 hours
- ✅ Developer questions answered < 1 hour
- ✅ No emergency design fixes in production
- ✅ Design system component usage > 80%

If any metric is failing, improve the handoff process.
