---
name: design-to-code-workflow
description: Bridge the gap between design and development for seamless component creation. Use this skill for design-to-code processes, component specification documentation, developer handoff, component generation strategies, design system implementation in code, Figma → React workflows, automated component generation, design tokens integration, and development team collaboration. Triggers include: converting Figma designs to React components, specifying components for developers, automating component generation, design system implementation, design-to-code handoff documentation, component creation workflows, and bridging designer-developer workflows. Essential for teams implementing design systems and automating the design-to-development pipeline.
---

# Design to Code Workflow

Bridging design systems and development: from Figma specifications to production React components.

The gap between design and code is where friction happens. A smooth design-to-code workflow means designers can specify components once, developers implement them accurately, and updates propagate automatically. This skill focuses on processes, tools, and patterns that eliminate this friction.

---

## Core Principle: Single Source of Truth

### The Design-to-Code Pipeline

```
Figma Design System
       ↓
Design Tokens (colors, spacing, typography)
       ↓
Component Specifications (props, states, variants)
       ↓
Code Generation (or manual implementation)
       ↓
Component Library (Storybook)
       ↓
Production Applications
```

**Goal:** Changes in Figma automatically propagate to:
- Design tokens in code
- Component props and defaults
- Component documentation
- Type definitions (TypeScript)

---

## Stage 1: Design Specification

### What Developers Need

Before writing code, developers need:

1. **Component Purpose**
   - What problem does this component solve?
   - When should it be used?

2. **Component Specifications**
   - Visual design (colors, sizes, spacing)
   - Interaction states (hover, focus, active, disabled)
   - Responsive behavior
   - Accessibility requirements

3. **Props & Variants**
   - What props does component accept?
   - What values for each prop?
   - Default values?
   - Type definitions (TypeScript)?

4. **Content & Copy**
   - Labels, helper text, error messages
   - Placeholder text
   - Call-to-action buttons

5. **Examples**
   - Primary use cases
   - Edge cases
   - Common variations

### Figma as Single Source

Figma should contain:
- Components with variants (primary/secondary, size variants, states)
- Design tokens in Figma Variables (colors, spacing, typography)
- Component documentation (description in component properties)
- Interaction examples (prototypes showing states)

**Example: Button Component in Figma**
```
Button (component)
├── Variants
│   ├── Variant: type (primary, secondary, ghost)
│   ├── Variant: size (sm, md, lg)
│   ├── Variant: state (default, hover, focus, active, disabled)
│   └── Variant: icon (with icon, without icon)
├── Properties
│   ├── Label (text)
│   ├── Icon (boolean)
│   └── Loading (boolean)
└── Documentation
    └── "Primary action button. Use for main CTAs."
```

---

## Stage 2: Component Specification Document

### Creating Developer-Friendly Specs

**Component Spec Template:**

```markdown
# Button Component

## Purpose
Primary action button for user interactions.

## Variants
- **Type:** primary (blue), secondary (gray), ghost (transparent)
- **Size:** sm (12px font), md (14px font), lg (16px font)
- **State:** default, hover, focus, active, disabled, loading
- **Icon:** with/without leading/trailing icon

## Props
```typescript
interface ButtonProps {
  variant?: 'primary' | 'secondary' | 'ghost';
  size?: 'sm' | 'md' | 'lg';
  disabled?: boolean;
  loading?: boolean;
  icon?: React.ReactNode;
  children: React.ReactNode;
  onClick?: () => void;
  type?: 'button' | 'submit' | 'reset';
}
```

## States
- **Default:** Normal appearance
- **Hover:** Background darkens by 10%
- **Focus:** 2px solid blue outline, 2px offset
- **Active:** Darker background, slightly inset
- **Disabled:** 50% opacity, no cursor pointer
- **Loading:** Spinner replaces icon, disabled

## Accessibility
- Focus indicator visible (2px outline)
- Icon has aria-label if icon-only button
- Disabled buttons announce status
- Color not sole indicator (icon + color)

## Examples
- Submit button (primary, md)
- Cancel button (secondary, md)
- Delete button (danger, md, loading state)
```

---

## Stage 3: Component Implementation

### Option A: Manual Implementation (Reliable)

**Button.tsx:**
```jsx
import React from 'react';
import styles from './Button.module.css';

interface ButtonProps {
  variant?: 'primary' | 'secondary' | 'ghost';
  size?: 'sm' | 'md' | 'lg';
  disabled?: boolean;
  loading?: boolean;
  icon?: React.ReactNode;
  children: React.ReactNode;
  onClick?: () => void;
  type?: 'button' | 'submit' | 'reset';
}

export function Button({
  variant = 'primary',
  size = 'md',
  disabled = false,
  loading = false,
  icon,
  children,
  onClick,
  type = 'button',
}: ButtonProps) {
  const className = `${styles.button} ${styles[`button--${variant}`]} ${styles[`button--${size}`]}`;

  return (
    <button
      className={className}
      disabled={disabled || loading}
      onClick={onClick}
      type={type}
      aria-busy={loading}
    >
      {loading ? <Spinner /> : icon}
      {children}
    </button>
  );
}
```

### Option B: Automated Generation (Faster)

**From Figma components → code:**

Tools like Penpot, Figma plugins, or custom scripts can:
1. Read Figma components and variants
2. Generate React component with props
3. Generate TypeScript types
4. Generate Storybook stories
5. Generate CSS/styled-components

**Generated Output:**
```
Button.tsx (component)
Button.types.ts (TypeScript interfaces)
Button.stories.tsx (Storybook)
Button.module.css (styles)
```

**Trade-off:**
- Manual: Reliable, customizable, slower to update
- Automated: Fast, consistent, less flexible

**Best approach:** Automated with manual refinement.

---

## Stage 4: Design Tokens Sync

### Tokens Flow

```
Figma Variables (colors, spacing, typography)
         ↓
Token Export (JSON, CSS, TypeScript)
         ↓
Code Integration (CSS, Tailwind config, design system)
         ↓
Component Usage
```

**Example: Color Tokens**

Figma Variables:
```
Colors/
  Primary/
    50: #eff6ff
    500: #2563eb
    900: #1e3a8a
  Neutral/
    50: #f9fafb
    900: #111827
```

Exported to `tokens.json`:
```json
{
  "color": {
    "primary": {
      "50": { "value": "#eff6ff" },
      "500": { "value": "#2563eb" },
      "900": { "value": "#1e3a8a" }
    }
  }
}
```

Used in code:
```jsx
import { tokens } from '@design-system/tokens';

const Button = styled.button`
  background-color: ${tokens.color.primary[500]};
  
  &:hover {
    background-color: ${tokens.color.primary[900]};
  }
`;
```

---

## Stage 5: Developer Handoff

### Before Implementation

**Design team provides:**
- ✅ Figma file with components
- ✅ Design tokens exported
- ✅ Component specifications (props, states)
- ✅ Figma → Code handoff document
- ✅ Storybook examples of each variant

### During Implementation

**Developers:**
- Implement component following spec
- Add TypeScript types
- Write Storybook stories
- Add tests (unit, visual, accessibility)
- Update component documentation

### After Implementation

**Design team reviews:**
- Does component match Figma?
- Are all states covered?
- Is accessibility correct?
- Performance acceptable?

**Deploy:**
- Publish component to npm
- Update Storybook
- Document in design system

---

## Stage 6: Updates & Versioning

### When Design Changes

1. **Designer updates Figma**
2. **Export new tokens** (if applicable)
3. **Developer updates component**
4. **Version bump** (major/minor/patch)
5. **Apps update** to new version

### Breaking Changes

When API changes (new required prop, removed prop):
- Major version bump (1.0.0 → 2.0.0)
- Document migration guide
- Consider deprecation period

### Non-Breaking Changes

When visual-only changes:
- Patch version bump (1.0.0 → 1.0.1)
- Apps auto-update

---

## Tools & Technologies

### Design-to-Code Tools

**Figma Plugins:**
- Figma to React (generates React components)
- HTML Export (generates HTML/CSS)
- Component Generator (creates component boilerplate)

**Design Token Tools:**
- Tokens Studio for Figma
- Style Dictionary (token transformation)
- Theo (design tokens)

**Code Generation:**
- Penpot (design tool with code export)
- Framer (design tool, exports React)
- Storybook (component documentation)

### Workflow Automation

**GitHub Actions:**
```yaml
name: Token Update
on: [push]
jobs:
  update-tokens:
    runs-on: ubuntu-latest
    steps:
      - name: Export tokens from Figma
        run: npm run export:tokens
      - name: Commit and push
        run: |
          git add tokens/
          git commit -m "Update design tokens"
          git push
```

---

## Common Challenges

### ❌ Designer-Developer Gap

Problem: Designers and developers work independently, updates don't sync.

Solution: 
- Use Figma as single source of truth
- Automate token export
- Regular design system reviews
- Shared documentation

### ❌ Stale Documentation

Problem: Figma doesn't match component in code.

Solution:
- Automate component generation
- CI/CD checks (compare Figma to code)
- Regular audits
- Version control for components

### ❌ Prop Explosion

Problem: Component has too many props, becomes unmaintainable.

Solution:
- Limit variants (3-4 per prop max)
- Use composition for complex UIs
- Keep related props together
- Document when to use variations

---

## Best Practices

### For Designers

- Keep Figma components organized
- Use variants for all states
- Document component usage
- Export tokens regularly
- Review code implementations

### For Developers

- Follow design specs exactly
- Implement TypeScript types
- Create Storybook stories
- Write accessible code
- Test all states/variants
- Keep documentation current

### For Teams

- Regular sync meetings (design + dev)
- Shared design system documentation
- Automated token export
- Version management strategy
- Migration guides for breaking changes

---

## When to Use This Skill

✅ **Do use for:**
- Design-to-code workflows
- Component specification and handoff
- Design tokens sync and integration
- Component generation strategies
- Design system implementation
- Figma → React component conversion
- Automation and CI/CD for design systems
- Designer-developer collaboration
- Version management for components

❌ **Don't use for:**
- Figma basics (see figma-expert-workflows)
- CSS styling (see css-styling-pixel-perfect)
- React component patterns (see frontend-framework-guide)
- Design system architecture (see design-systems-architecture)
- Component documentation (see component-library-mastery)
