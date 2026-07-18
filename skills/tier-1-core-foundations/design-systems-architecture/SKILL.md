---
name: design-systems-architecture
description: Build scalable, maintainable design systems that grow with your product. Use this skill for design system foundations, component architecture, token strategies, governance decisions, scaling patterns, documentation standards, versioning, and cross-team collaboration. Triggers include: design system audits, component library structure, design token organization, system governance questions, scaling decisions, component versioning, breaking changes, documentation strategy, multi-brand support, and system health metrics. Essential for anyone architecting or maintaining a design system serving multiple teams or products.
---

# Design Systems Architecture

A practical guide to building, scaling, and maintaining design systems that serve teams and products long-term.

A design system is more than a component library—it's a shared language, a source of truth, and a scalable architecture for consistent digital experiences. This skill covers the principles, organization, and governance that make systems sustainable.

## What Is a Design System?

A design system comprises:

**Foundation** — Design tokens (colors, typography, spacing, etc.) that define the visual language

**Components** — Reusable UI building blocks (buttons, cards, modals) paired with guidelines

**Patterns** — Common interaction and layout patterns (forms, navigation, error handling)

**Documentation** — Clear guidance on when/how to use each piece

**Governance** — Decision-making framework, versioning, deprecation policy

**Tools** — Platforms that host, maintain, and distribute the system (Figma, Storybook, npm, etc.)

---

## Design System Principles

### 1. Atomic Design Methodology

**Atomic Design** (by Brad Frost) organizes components by complexity:

**Atoms** (smallest)
- Buttons, labels, inputs, icons
- Standalone; can't be broken down further
- Example: `<button>`, `<label>`, `<input />`

**Molecules** (combination of atoms)
- Form field (label + input + error message)
- Card header (icon + title + subtitle)
- Search box (input + icon + button)
- Example: `<FormField>`, `<CardHeader>`

**Organisms** (combination of molecules + atoms)
- Contact form (multiple form fields + button)
- Header (logo + nav + search box)
- Product card (image + title + description + button)
- Example: `<ContactForm>`, `<Header>`

**Templates** (layout + organisms, no content)
- Product listing page layout (header + sidebar + product grid)
- Article template (nav + hero + content + footer)
- Example: `<ProductListingTemplate>`

**Pages** (templates + real content)
- Actual product listing page with real products
- Actual article with real text
- Example: `<ProductListingPage>`

**When to use:** Atomic design is useful for thinking through hierarchy but shouldn't lock you into strict folder structure. Many teams flatten the hierarchy in practice.

### 2. Component-Based Architecture

Design systems organize around components, not pages. A component is a:

- **Self-contained unit** with clear inputs (props) and outputs (rendered UI)
- **Reusable** across different contexts and pages
- **Documented** with usage guidelines and examples
- **Testable** with a clear contract (props → output)
- **Accessible** by default, not as an afterthought

**Characteristics of well-designed components:**

✅ **Single responsibility** — One component, one purpose (button activates action; modal confirms decision)

✅ **Composable** — Can combine with other components without knowing their internals

✅ **Predictable** — Same props + state = same output every time

✅ **Resilient** — Handles edge cases (long text, many items, missing data)

✅ **Documented** — Clear examples, do's and don'ts, accessibility notes

### 3. Scalability Over Perfection

A design system evolves. Prioritize:

1. **Foundation first** — Tokens, typography, spacing scales are more important than perfect components
2. **Coverage over perfection** — Better to have 80% of components working well than 20% perfect
3. **Iteration** — Version early and often; don't wait for "complete"
4. **User feedback** — Listen to teams using the system; they know what's missing

---

## Design System Organization

### Layer Model: Primitives → Semantic → Components

**Layer 1: Primitives**
- Raw colors (hex values, RGB)
- Raw font families
- Raw spacing increments (8px, 16px, 24px, etc.)
- Raw shadows, borders, border-radius
- Example: `color-red-600`, `font-family-sans`, `space-4`

**Layer 2: Semantic**
- Colors with meaning (primary, secondary, danger, success)
- Typography scales (display, heading, body, caption)
- Semantic spacing (section-gap, component-gap, element-gap)
- Semantic shadows (sm, md, lg)
- Example: `color-primary`, `typography-body`, `space-section-large`

**Layer 3: Components**
- Composed semantic tokens into UI elements
- Buttons, cards, forms, modals
- Include styling, structure, and interaction
- Example: `<Button>`, `<Card>`, `<Modal>`

**Why layers matter:**
- Maintenance is easier (change one primitive, all semantics and components update)
- Teams speak the same language (everyone understands "primary color")
- Flexibility (different contexts can use different semantics, same primitives)

### Folder Structure Example

```
design-system/
├── tokens/
│   ├── primitives/
│   │   ├── colors.json
│   │   ├── typography.json
│   │   └── spacing.json
│   └── semantic/
│       ├── colors.json
│       ├── typography.json
│       └── spacing.json
├── components/
│   ├── Button/
│   │   ├── Button.tsx
│   │   ├── Button.stories.tsx
│   │   ├── Button.test.tsx
│   │   └── README.md
│   ├── Card/
│   ├── Modal/
│   └── ...
├── patterns/
│   ├── FormField.tsx
│   ├── Layout.tsx
│   └── ...
├── docs/
│   ├── Getting Started.md
│   ├── Design Tokens.md
│   ├── Component API.md
│   └── Contributing.md
└── README.md
```

---

## Design Tokens

Design tokens are the atomic units of a design system—the decisions that power everything else.

### Token Categories

**Color tokens:**
```json
{
  "color": {
    "primary": "#2563eb",
    "primary-hover": "#1d4ed8",
    "success": "#10b981",
    "warning": "#f59e0b",
    "error": "#ef4444"
  }
}
```

**Typography tokens:**
```json
{
  "typography": {
    "display": { "size": "32px", "weight": "700", "lineHeight": "1.2" },
    "heading": { "size": "24px", "weight": "600", "lineHeight": "1.3" },
    "body": { "size": "16px", "weight": "400", "lineHeight": "1.5" },
    "caption": { "size": "12px", "weight": "400", "lineHeight": "1.4" }
  }
}
```

**Spacing tokens:**
```json
{
  "spacing": {
    "xs": "4px",
    "sm": "8px",
    "md": "16px",
    "lg": "24px",
    "xl": "32px",
    "2xl": "48px"
  }
}
```

### Token Naming Convention

Use a hierarchical structure: `category-semantic-variant`

✅ `color-primary`
✅ `color-primary-hover`
✅ `spacing-section-gap`
✅ `typography-body-large`

❌ `blue-600` (too specific; doesn't convey meaning)
❌ `p1` (too cryptic)
❌ `LargeSpacing` (inconsistent casing)

### DTCG (Design Tokens Community Group) Standard

The W3C-backed DTCG format enables token sharing across tools:

```json
{
  "$schema": "https://tokens.figma.com/schema.json",
  "color": {
    "primary": {
      "$value": "#2563eb",
      "$type": "color",
      "$description": "Primary action color"
    },
    "primary-hover": {
      "$value": "{color.primary}",
      "$type": "color",
      "$description": "Primary hover state (darker)"
    }
  }
}
```

**Benefits:** Tools (Figma, Storybook, design token generators) can read this standard format and generate code automatically.

---

## Component Documentation

Every component needs clear documentation.

### Component README Template

```markdown
# Button

Primary call-to-action element.

## Usage

```jsx
<Button variant="primary">Click me</Button>
```

## Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `variant` | enum | `"primary"` | `"primary"` \| `"secondary"` \| `"ghost"` |
| `size` | enum | `"md"` | `"sm"` \| `"md"` \| `"lg"` |
| `disabled` | boolean | `false` | Disables button |
| `onClick` | function | — | Click handler |
| `children` | node | — | Button text or content |

## Guidelines

✅ **Do**
- Use primary variant for main actions
- Pair with descriptive text ("Save changes" not "OK")
- Provide loading state for async actions

❌ **Don't**
- Use as link (use `<a>` or Link component)
- Stack multiple primary buttons
- Remove focus outline

## Accessibility

- Has visible focus indicator (outlined, 2px)
- Keyboard accessible (Enter, Space to activate)
- Disabled buttons have reduced opacity (50%), not gray
- No aria-label needed if text is clear

## Examples

### Primary Button
```jsx
<Button variant="primary">Save</Button>
```

### Secondary Button
```jsx
<Button variant="secondary">Cancel</Button>
```

### Disabled State
```jsx
<Button disabled>Disabled</Button>
```

### With Loading
```jsx
<Button loading>Processing...</Button>
```
```

### What Good Documentation Includes

✅ Purpose — What is this component for?
✅ Usage — Code example (copy-paste ready)
✅ Props — What can be configured?
✅ Guidelines — When to use, when not to use
✅ Accessibility — a11y considerations
✅ Examples — Multiple real-world variants
✅ Changelog — What changed in recent versions

---

## Governance & Versioning

### Semantic Versioning

Use `MAJOR.MINOR.PATCH`:

- **MAJOR** — Breaking changes (e.g., removed prop, changed API)
- **MINOR** — New features, backward compatible
- **PATCH** — Bug fixes, backward compatible

Example: `Button` component moves from `1.2.0` → `2.0.0` if a prop changes.

**Release strategy:**
- Patch releases frequently (as needed)
- Minor releases quarterly (or as features land)
- Major releases annually or when necessary (communicate deprecations 2+ releases ahead)

### Deprecation Policy

Don't remove features abruptly. Follow this pattern:

**v1.5.0** — Deprecate (add warning in console)
```js
console.warn('`size` prop is deprecated in Button v1.5.0. Use `variant` instead.');
```

**v1.6.0–v1.9.0** — Keep deprecated prop, warn
(Give teams 2–3 releases to migrate)

**v2.0.0** — Remove deprecated prop
(Major version bump, documented migration guide)

### Breaking Change Policy

Before introducing breaking changes:

1. **Deprecate** the old API (1–2 releases)
2. **Warn** teams (in release notes, team meetings, Slack)
3. **Document** migration path (before/after code examples)
4. **Update** v1 docs to include deprecation notice
5. **Release** as major version bump (v1.x → v2.0)

Example breaking change changelog:
```markdown
## v2.0.0 (2025-01-15)

### Breaking Changes
- **Button**: Removed `color` prop. Use `variant` instead.
  - Before: `<Button color="red">Delete</Button>`
  - After: `<Button variant="danger">Delete</Button>`
- **Modal**: Renamed `isOpen` to `open`. Use `onClose` instead of `onDismiss`.

### Migration Guide
See MIGRATION.md for detailed upgrade instructions.
```

---

## Scaling: Single Brand → Multi-Brand

**Phase 1: Single Brand**
- One set of tokens (colors, typography, spacing)
- Components inherit brand tokens
- Simpler, but limited

**Phase 2: Multi-Brand Tokens**
- Multiple token sets (e.g., `light-mode`, `dark-mode`, `brand-a`, `brand-b`)
- Components use CSS variables or token overrides
- Allows brand variation without duplicating components

Example:
```css
/* theme-light.css */
:root {
  --color-primary: #2563eb;
  --color-success: #10b981;
}

/* theme-dark.css */
:root {
  --color-primary: #60a5fa;
  --color-success: #6ee7b7;
}
```

Components stay the same; tokens change:
```jsx
<button style={{ color: 'var(--color-primary)' }}>Click me</button>
```

**Phase 3: Multi-Product Architecture**
- Shared foundation (core tokens, base components)
- Product-specific layers (custom components, extended tokens)
- Reduces duplication, maintains coherence

Example structure:
```
design-system-monorepo/
├── @ds/core/              # Shared tokens, base components
│   ├── tokens/
│   ├── Button/
│   └── ...
├── @ds/mobile/            # Mobile-specific extensions
│   ├── tokens/            # Mobile overrides
│   └── MobileMenu/
└── @ds/web/               # Web-specific extensions
    ├── tokens/            # Web overrides
    └── Sidebar/
```

---

## System Health Metrics

Monitor your design system's adoption and health:

**Adoption metrics:**
- % of components used in products
- % of teams using design system tokens
- Reduction in custom CSS (goal: <10% custom styles)

**Quality metrics:**
- Component test coverage (goal: ≥80%)
- Accessibility audit pass rate (goal: 100% WCAG AA)
- Documentation completeness (goal: 100% of components)

**Velocity metrics:**
- Time to add new component (goal: <1 sprint)
- Time to release new version (goal: <2 weeks)
- Upgrade adoption (% of teams on latest version)

**Maintenance metrics:**
- GitHub issues resolved/month
- Feature requests backlog (prioritize high-impact items)
- Deprecation migration completion (track teams still on old versions)

---

## Common Mistakes & How to Fix Them

### Too Many Tokens
**Problem:** 50+ color tokens, teams confused about which to use.
**Fix:** Reduce to semantic tokens (primary, secondary, danger, success). Document use cases for each.

### Components Without Documentation
**Problem:** Team doesn't know how to use Button component; creates 3 duplicate versions.
**Fix:** Every component needs a README. Include examples, props, and dos/don'ts.

### No Versioning Strategy
**Problem:** Breaking changes shipped without warning; teams' builds break.
**Fix:** Implement semantic versioning. Deprecate → warn → remove (over 2+ releases).

### Governance Vacuum
**Problem:** Anyone can add components; no standards. System becomes bloated and inconsistent.
**Fix:** Create contribution guidelines. Require design review, accessibility audit, documentation before merge.

### Treating It Like a Repository, Not a System
**Problem:** Design system is just a code repo; no one thinks about the ecosystem.
**Fix:** Treat it as a product. Have a product owner, roadmap, user research (talk to teams using it), and quarterly reviews.

---

## Reference Files & Further Reading

Bundled resources:
- **atomic-design-to-code.md** — Translating Atomic Design principles into folder structure and components
- **component-api-patterns.md** — Designing component props, composition patterns, slot-based architecture
- **governance-scaling-guide.md** — Governance models, versioning strategies, team structure

---

## When to Use This Skill

✅ **Do use for:**
- Design system architecture and organization decisions
- Component structure and naming strategy
- Design token strategy and organization
- Versioning and governance decisions
- Documentation standards and templates
- Scaling strategies (single → multi-brand)
- System health metrics and KPIs
- Deprecation and migration planning
- Atomic Design application

❌ **Don't use for:**
- Specific tool setup (Figma, Storybook, npm config — see tool-specific skills)
- Component implementation details (CSS, React hooks, framework code)
- Design language decisions (colors, typography — see design-fundamentals)
- Accessibility patterns (see web-accessibility-a11y)
- Performance optimization (see web-performance-optimization)
