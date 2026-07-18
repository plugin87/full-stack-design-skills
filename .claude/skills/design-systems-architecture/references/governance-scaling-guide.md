# Governance & Scaling: Strategies for Sustainable Design Systems

Managing versions, breaking changes, team contributions, and multi-brand growth.

## Governance Model: Who Decides?

Before any changes reach production, decide: who approves?

### Model 1: Centralized (Design Systems Team)

A dedicated team owns the design system. All changes go through them.

**Process:**
1. Team requests feature/fix
2. Design systems team evaluates
3. Design systems team implements
4. Design systems team releases

**Pros:**
- Consistency and quality control
- Prevents off-brand patterns
- Clear ownership

**Cons:**
- Bottleneck (team waits for approvals)
- Requires dedicated headcount
- Slow iteration

### Model 2: Federated (Multi-Team)

Multiple teams contribute; a steering committee reviews.

**Process:**
1. Any team proposes component/feature
2. Steering committee (reps from multiple teams) reviews
3. Contributor implements (with feedback)
4. Steering committee approves
5. Anyone can release (if they follow the process)

**Pros:**
- Faster iteration
- Teams feel ownership
- More feedback

**Cons:**
- Requires discipline (process adherence)
- May diverge from central vision
- Harder to maintain consistency

### Model 3: Open Source (Community)

Anyone can contribute; maintainers merge high-quality PRs.

**Process:**
1. Anyone forks/branches
2. Contributor proposes PR
3. Maintainers review
4. Maintainers merge + release

**Pros:**
- Maximum participation
- Great for external design systems
- Community-driven

**Cons:**
- Quality control is harder
- Risk of feature bloat
- Requires active maintainers

**Recommendation:** Start centralized (Model 1) with 1-2 people. As usage grows, move toward federated (Model 2).

---

## Versioning Strategy: Semantic Versioning

**Version format:** `MAJOR.MINOR.PATCH` (e.g., `1.5.3`)

### PATCH (Bug Fixes)

No changes to public API. Pure bug fixes, accessibility improvements, performance tweaks.

**Example:**
- Fixed Button focus outline rendering on Firefox
- Updated TextField placeholder color for better contrast
- Reduced CSS bundle size by 2KB

**Release frequency:** As needed (weekly or more often).

**Upgrade:** Safe; encourage all teams to upgrade immediately.

```json
{
  "version": "1.2.5",
  "changelog": "- Fixed Button focus state on Safari"
}
```

### MINOR (Features, Backward Compatible)

New features, new components, new props. Everything is backward compatible.

**Examples:**
- Added new `size="xl"` to Button component
- Added new `Tooltip` component
- Added `aria-label` prop to Icon

**Release frequency:** Monthly or quarterly.

**Upgrade:** Safe; upgrade when needed. Old code continues to work.

```json
{
  "version": "1.3.0",
  "changelog": "- Added Tooltip component\n- Added size=\"xl\" to Button"
}
```

### MAJOR (Breaking Changes)

Remove or change public API. Old code breaks; teams must update.

**Examples:**
- Removed deprecated `color` prop from Button (use `variant`)
- Changed Modal API from `open`/`onClose` to `isOpen`/`onDismiss`
- Dropped support for IE11

**Release frequency:** Annually or when necessary.

**Upgrade:** Requires team effort. Provide migration guide.

```json
{
  "version": "2.0.0",
  "changelog": "BREAKING: Removed Button.color prop. Use variant instead. See MIGRATION.md"
}
```

---

## Deprecation Policy: Avoiding Surprise Breaking Changes

### Phase 1: Announce Deprecation (v1.5.0)

```javascript
// Button.tsx - version 1.5.0
if (color) {
  console.warn(
    '[Button] The "color" prop is deprecated in v1.5.0. ' +
    'Use "variant" prop instead (e.g., variant="danger"). ' +
    'This prop will be removed in v2.0.0.'
  );
}
```

**Release notes:**
```markdown
## v1.5.0 (2025-01-15)

### Deprecations
- **Button.color** — Use `variant` instead
  - Deprecated in: v1.5.0
  - Removal planned: v2.0.0
  - Migration: See MIGRATION.md
```

### Phase 2: Keep It Working, Warn (v1.5.0 - v1.9.0)

- Old prop still works
- Console warning shown
- Code comment points to migration guide
- Give teams 2-3 MINOR releases to migrate

**Timeline example:**
- v1.5.0 — Deprecate (announce)
- v1.6.0 — Release features
- v1.7.0 — Release features
- v1.8.0 — Release features (last chance to migrate)
- v2.0.0 — Remove deprecated API

### Phase 3: Remove in Major Release (v2.0.0)

```javascript
// Button.tsx - version 2.0.0
// color prop is gone; use variant instead
```

**Release notes:**
```markdown
## v2.0.0 (2025-06-01)

### Breaking Changes
- **Button**: Removed `color` prop
  - Before: `<Button color="red">Delete</Button>`
  - After: `<Button variant="danger">Delete</Button>`
  - See MIGRATION.md for full upgrade guide
```

---

## Breaking Change Communication

When breaking changes are necessary:

### 1. Release Notes (Detailed)

```markdown
## v2.0.0 Migration Guide

### Button Component

**What changed:** `color` prop removed; use `variant` instead.

**Why:** Prop naming wasn't clear. `variant` is more consistent with design system terminology.

**Migration steps:**
1. Find all `<Button color="..." />`
2. Replace with `<Button variant="..." />`
3. See color mapping table below

**Color mapping:**
| Old (v1.x) | New (v2.0) |
|------------|-----------|
| `color="red"` | `variant="danger"` |
| `color="green"` | `variant="success"` |
| `color="blue"` | `variant="primary"` |

**Example:**
```diff
- <Button color="red">Delete</Button>
+ <Button variant="danger">Delete</Button>
```
```

### 2. Slack/Email Announcement

```
@channel: Design System v2.0 Released

TL;DR: Button "color" prop changed to "variant". See migration guide.

Timeline:
- Now (v2.0): Major version released
- Next 2 weeks: Migrate your code
- June 15: We'll help debug migration issues

Migration guide: [link]
Ask questions: #design-system
```

### 3. Office Hours / Q&A

Offer to help teams upgrade. Be patient.

---

## Multi-Brand Support Strategy

### Stage 1: Single Brand (Easy)

All products use the same design system. One set of tokens.

```
design-system/
├── tokens/
│   └── default.json
├── components/
└── ...
```

### Stage 2: Multiple Themes (Moderate)

Same components, different tokens for different "modes" (light, dark, high-contrast).

```
design-system/
├── tokens/
│   ├── light.json
│   ├── dark.json
│   └── high-contrast.json
├── components/
└── ...
```

**Implementation:**
```css
:root {
  --color-primary: #2563eb; /* light mode */
}

@media (prefers-color-scheme: dark) {
  :root {
    --color-primary: #60a5fa; /* dark mode */
  }
}
```

### Stage 3: Multiple Brands (Complex)

Different brands want different components or tokens (e.g., B2B vs. B2C products).

**Option A: Monorepo with Layers**

```
design-system-monorepo/
├── @ds/core/              # Shared foundation
│   ├── Button/
│   ├── tokens/
│   └── ...
├── @ds/brand-a/           # Brand A overrides
│   ├── tokens/
│   └── components/
└── @ds/brand-b/           # Brand B overrides
    ├── tokens/
    └── components/
```

**Usage:**
```json
{
  "dependencies": {
    "@ds/core": "^2.0.0",
    "@ds/brand-a": "^2.0.0"
  }
}
```

**Option B: Single System with Brand Tokens**

```
design-system/
├── tokens/
│   ├── primitives.json
│   ├── brand-a-semantic.json
│   └── brand-b-semantic.json
└── components/
```

**Usage:**
```tsx
// In brand A app
import { Button } from '@ds/core';
import tokensBrandA from '@ds/tokens/brand-a-semantic';

// Apply brand tokens at runtime
const theme = createTheme(tokensBrandA);
export const App = () => <ThemeProvider theme={theme}><App /></ThemeProvider>;
```

---

## System Health Metrics

Track and report on system health quarterly.

### Adoption Metrics

**Component usage:**
```
Audit: How many teams use design system components vs. custom code?

Goal: 80% of components in production come from DS

Measure:
- Codebase audit (grep for DS component imports)
- Visual regression testing (compare deployed components vs. DS versions)
- Manual review of high-traffic pages
```

**Team coverage:**
```
Goal: 90% of engineering teams using DS

Measure:
- Survey: "Have you used DS components in the last sprint?"
- Repository scan: grep for DS imports across all repos
```

### Quality Metrics

**Component test coverage:**
```
Goal: 80%+ unit test coverage for all components

Measure:
- Coverage reports from CI/CD
- Identify gaps (untested components)
- Prioritize high-risk components
```

**Accessibility compliance:**
```
Goal: 100% WCAG AA compliance

Measure:
- Automated audits (axe, WAVE)
- Manual screen reader testing (quarterly)
- User feedback from accessibility community
```

**Documentation completeness:**
```
Goal: 100% of components documented

Measure:
- Component audit: every component has README + examples
- Check for outdated examples (aligned with current version)
```

### Velocity Metrics

**Time to add new component:**
```
Goal: <1 sprint (8 days)

Measure:
- Track time from proposal to release
- Identify bottlenecks (design review? implementation? testing?)
```

**Upgrade adoption:**
```
Goal: 80% of teams on latest minor version within 1 month

Measure:
- Scan package-lock.json across repos
- Survey: "What version of DS are you on?"
- Identify blockers (why teams stay on old versions)
```

**Issue resolution time:**
```
Goal: Bug fixes released within 1 week

Measure:
- Track time from issue report to release
- Monitor open issue count (should be stable)
```

---

## Contribution Guide Template

Every design system should have a CONTRIBUTING.md:

```markdown
# Contributing to the Design System

## Getting Started
1. Clone the repo
2. Run `npm install`
3. Run `npm start` to start Storybook

## Before You Start
- Check existing components (don't duplicate)
- Open a GitHub issue to propose new component/feature
- Wait for feedback before implementing

## Implementation
1. Create component (TypeScript + JSX)
2. Add unit tests (target: 80%+ coverage)
3. Add accessibility tests (axe scanning)
4. Add Storybook stories (at least 3 variants)
5. Add README with examples and guidelines

## Code Review
- Maintainers review within 3 business days
- Address feedback; resubmit
- Once approved: maintainer merges + releases

## Versioning
- PATCH releases: automatic (every merge)
- MINOR releases: quarterly
- MAJOR releases: annual planning + migration guides

## Conduct
- Be respectful and inclusive
- Assume good intent
- Help others learn
```
