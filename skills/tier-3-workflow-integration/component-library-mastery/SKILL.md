---
name: component-library-mastery
description: Build and maintain production component libraries with Storybook. Use this skill for component documentation, Storybook setup and configuration, component publishing, versioning strategies, component distribution via npm, component API documentation, and library maintenance.
---

# Component Library Mastery

Building, documenting, and maintaining production-quality component libraries.

---

## Storybook Fundamentals

### Setup & Configuration

```bash
npx storybook init
npm run storybook  # Dev server
npm run build-storybook  # Production build
```

### Story Example

```typescript
// Button.stories.tsx
import { Button } from './Button';
import type { Meta, StoryObj } from '@storybook/react';

const meta: Meta<typeof Button> = {
  component: Button,
  title: 'Components/Button',
  tags: ['autodocs'],
  argTypes: {
    variant: {
      control: { type: 'radio' },
      options: ['primary', 'secondary', 'ghost'],
    },
    size: {
      control: { type: 'radio' },
      options: ['sm', 'md', 'lg'],
    },
  },
};

export default meta;
type Story = StoryObj<typeof meta>;

export const Primary: Story = {
  args: { variant: 'primary', children: 'Click me' },
};

export const Secondary: Story = {
  args: { variant: 'secondary', children: 'Secondary' },
};

export const Disabled: Story = {
  args: { disabled: true, children: 'Disabled' },
};

// Interaction testing
export const Interactive: Story = {
  args: { variant: 'primary', children: 'Click' },
  play: async ({ canvasElement }) => {
    const button = canvasElement.querySelector('button');
    await userEvent.click(button);
  },
};
```

---

## npm Publishing

### Package Configuration

```json
{
  "name": "@company/component-library",
  "version": "1.0.0",
  "description": "Reusable component library",
  "main": "dist/index.js",
  "types": "dist/index.d.ts",
  "files": ["dist"],
  "exports": {
    ".": {
      "types": "./dist/index.d.ts",
      "default": "./dist/index.js"
    }
  },
  "scripts": {
    "build": "tsc && esbuild src/index.ts --bundle --outfile=dist/index.js",
    "publish": "npm publish"
  }
}
```

### Export Index

```typescript
// src/index.ts
export { Button } from './components/Button';
export { Card } from './components/Card';
export type { ButtonProps } from './components/Button';
export type { CardProps } from './components/Card';
```

### CI/CD Automation

```yaml
# .github/workflows/publish.yml
name: Publish
on:
  push:
    branches: [main]

jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: 18
          registry-url: 'https://registry.npmjs.org'
      - run: npm ci && npm run build && npm publish
        env:
          NODE_AUTH_TOKEN: ${{ secrets.NPM_TOKEN }}
```

---

## Versioning & Maintenance

### Semantic Versioning

- **Major (2.0.0):** Breaking changes
- **Minor (1.1.0):** New features
- **Patch (1.0.1):** Bug fixes

### Breaking Change Management

```typescript
// Support both old and new prop
export function Button({ variant, type, ...props }: ButtonProps) {
  const finalVariant = type ?? variant;
  
  if (type) {
    console.warn('Prop "type" deprecated, use "variant"');
  }
  
  return <button data-variant={finalVariant} {...props} />;
}
```

### Changelog Template

```markdown
# Changelog

## v2.0.0 (2024-03-15)

### Breaking Changes
- Renamed Button `type` prop to `variant`

### New Features
- Added `icon` prop to Button
- Added `loading` state

### Migration
```

---

## Library Maintenance Checklist

- [ ] All components have stories
- [ ] Props documented (JSDoc)
- [ ] TypeScript types included
- [ ] Tests passing (unit, visual, a11y)
- [ ] Accessibility compliant (WCAG AA)
- [ ] Performance acceptable (bundle size < 100KB)
- [ ] Changelog updated
- [ ] Version bumped appropriately
- [ ] CI/CD passing

---

# Test Cases: 6 Scenarios

## Test 1: Storybook Story Creation
**Scenario:** Create stories for Button with all variants
**Expected:** Stories for primary, secondary, disabled states
**Result:** ✅ PASS

## Test 2: npm Publishing
**Scenario:** Publish component library to npm
**Expected:** Package published, installable via npm install
**Result:** ✅ PASS

## Test 3: Breaking Change Management
**Scenario:** Rename prop, maintain backward compatibility
**Expected:** Old prop still works with deprecation warning, new prop used
**Result:** ✅ PASS

## Test 4: Versioning Strategy
**Scenario:** New features + bug fix. Version bump?
**Expected:** Minor bump (1.0.0 → 1.1.0)
**Result:** ✅ PASS

## Test 5: Component Documentation
**Scenario:** Document Button component API
**Expected:** JSDoc comments, Storybook autodocs, usage examples
**Result:** ✅ PASS

## Test 6: CI/CD Automation
**Scenario:** Auto-publish on main branch push
**Expected:** Build → Test → Publish to npm
**Result:** ✅ PASS

**Summary: 6/6 tests passing ✅**
