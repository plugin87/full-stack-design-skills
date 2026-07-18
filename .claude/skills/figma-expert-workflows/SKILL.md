---
name: figma-expert-workflows
description: Master advanced Figma workflows for professional design systems and collaborative design. Use this skill for component creation, design tokens, variables, prototyping, design tokens sync with code, collaborative features, performance optimization, plugin recommendations, and handoff processes. Triggers include: Figma component strategy, design token management in Figma, interactive prototypes, design system setup in Figma, collaborative workflows, Figma plugins, component variants, auto layout, design-to-code handoff, and Figma performance. Essential for UX/UI designers building design systems, component libraries, or collaborating with developers.
---

# Figma Expert Workflows

Advanced techniques and workflows for professional design and design system work in Figma.

Figma is the industry-standard design tool for UX/UI designers and design system architects. Mastering Figma's advanced features—components, variables, tokens, prototyping, and handoff—is essential for efficient, scalable design workflows.

## Core Concepts: Files, Pages, Frames, Components

### File Organization

A well-organized Figma file is the foundation of efficient collaboration.

**Structure:**
```
Project File
├── Cover Page (overview, links, guidelines)
├── Design System (tokens, colors, typography)
├── Components (Button, Card, Modal, etc.)
├── Patterns (Forms, Lists, Navigation)
├── Prototypes (High-fidelity interactive mockups)
├── Pages by Feature (Auth, Dashboard, Settings, etc.)
└── Archive (Old versions, experiments)
```

**Best practices:**
- One file per product/section (not one massive file)
- Use clear naming (PascalCase for components, kebab-case for instances)
- Lock shared components (prevent accidental edits)
- Use boards to organize pages (Figma feature)
- Archive old versions (don't delete)

### Components vs. Instances

**Component:** The "source of truth" — the original design.
**Instance:** A copy that inherits properties from the component.

When you edit a component, all instances update automatically. This is the power of components.

**Example:**
```
Component: Button (primary, secondary, ghost variants)
├── Instance in Design A (used in 5 places)
├── Instance in Design B (used in 3 places)
└── Instance in Design C (used in 2 places)

Edit component → All 10 instances update
```

### Main Components vs. Copies

In Figma, there's a difference:
- **Main component** (original, has ◆ icon) — is the source
- **Instance** (copy, has ◊ icon) — inherits from main
- **Regular copy** (no icon) — independent copy, won't update

**Always use instances,** not regular copies.

---

## Advanced Components: Variants & Auto Layout

### Component Variants

Variants let you create multiple versions of a component in one place.

**Example: Button component with variants**
```
Button (main component)
├── Variant: Size/Large, Style/Primary, State/Default
├── Variant: Size/Large, Style/Primary, State/Hover
├── Variant: Size/Large, Style/Primary, State/Active
├── Variant: Size/Small, Style/Primary, State/Default
└── ... 20+ variants total
```

**In Figma:**
1. Select frame (button)
2. Right-click → "Create component"
3. In Design panel → "Add +" next to "Variant property"
4. Add properties: Size (Small/Medium/Large), Style (Primary/Secondary), State (Default/Hover/Active)
5. Duplicate and modify for each variant

**Benefits:**
- Single component, multiple variants
- Teams see all options in one place
- Instance swapping is intuitive
- Smaller file size than multiple components
- Easier to maintain (one source of truth)

### Auto Layout

Auto Layout makes responsive components that adapt to content.

**Without Auto Layout:**
- Manually adjust frame size when text changes
- Elements don't reflow
- Hard to maintain consistency

**With Auto Layout:**
- Add padding
- Set gap between children
- Components expand/contract automatically
- Text wrapping is built-in

**Example: Button with Auto Layout**
```
Button frame
├── Auto Layout: Horizontal (left to right)
├── Padding: 12px (all sides)
├── Gap: 8px (between icon and text)
└── Grows with: Text changes, icon size changes

User sees:
- Text is 12px → Button is tight
- Text is 24px → Button expands automatically
- Add icon → Icon + text auto-spaced with 8px gap
```

**When to use:**
- Buttons with icons and text
- Form fields with labels
- Cards with variable content
- Lists that can grow/shrink
- Basically everything (it's that useful)

---

## Design Tokens in Figma

Design tokens are design decisions (colors, typography, spacing) stored as variables.

### Variables: Color, Number, Boolean, String

Figma Variables let you store design decisions and apply them across designs.

**Variable types:**
- **Color:** Hex, RGB, HSLA values
- **Number:** Spacing, size, opacity, stroke width
- **Boolean:** True/false (useful for plugin logic)
- **String:** Font names, icon names (advanced use)

**Example: Semantic color variables**
```
Variable Name: color/primary
Value: #2563eb

Use in component:
- Button fill: color/primary
- Button hover: color/primary + 10% darker
- Button active: color/primary + 20% darker

Change variable → All components using color/primary update
```

### Creating Variables

**In Figma:**
1. Assets panel → Variables
2. Create collection: "Design Tokens"
3. Add variables:
   - color/primary: #2563eb
   - color/secondary: #64748b
   - space/sm: 8
   - space/md: 16
   - space/lg: 24
   - typography/body-size: 16
   - typography/body-weight: 400

**Naming convention:** `category/subcategory/variant`
- `color/primary`
- `color/primary-hover`
- `space/section-gap`
- `typography/body/size`

### Modes: Light, Dark, and Beyond

Variables support **modes**—different sets of values for different contexts.

**Example: Light and Dark modes**
```
Variable: color/primary
├── Mode "Light": #2563eb (blue)
└── Mode "Dark": #60a5fa (lighter blue)

In design:
- Select component
- Right-click → Edit variables
- Color/Primary = [color/primary]
- Switch mode → Colors update automatically
```

**Use cases:**
- Light/dark themes
- Breakpoints (mobile, tablet, desktop)
- Brands (brand-a, brand-b)
- Locales (en, th, ja)

---

## Design System Setup in Figma

A professional design system file has layers.

### Layer 1: Tokens & Styles

**File: Design System**
- Primitives page: Raw colors, typography, spacing
- Tokens page: Semantic tokens (primary, success, danger)
- Color board: Swatches of all colors
- Typography board: Text styles at different scales

**Setup:**
1. Create color swatches (use Groups to organize)
2. Create text styles (Body, Heading, Caption)
3. Create component styles (Button, Card, Input)
4. Document in cover page (link to tokens, explain usage)

### Layer 2: Base Components

**File: Design System → Components page**
- Atoms: Button, Input, Icon, Label, Badge
- Molecules: FormField, SearchBox, Card
- Organisms: Header, Footer, Modal, Sidebar

**Each component includes:**
- All variants (size, state, style)
- Auto layout configured
- Proper spacing and padding
- Dark/light mode support (via variables)

### Layer 3: Patterns & Templates

**File: Design System → Patterns page**
- Common layouts (two-column, hero + content)
- Form patterns (login, signup, settings)
- Card patterns (product, article, user)

**Each pattern:**
- Uses base components
- Documents best practices
- Shows do's and don'ts

### Layer 4: Example Pages

**File: Design System → Example pages**
- Real product pages using components
- Demonstrates proper usage
- Serves as reference for design teams

---

## Prototyping & Interaction

### Interactive Components (Smart Animate)

Create clickable prototypes with Smart Animate.

**Example: Button click → Modal appears**
1. Create two frames: "Default" and "Modal Open"
2. Position modal in both frames (same position)
3. Select modal frame in "Default"
4. Prototype panel → Click "+" → Drag to "Modal Open" frame
5. Interaction: On click → Change to → "Modal Open" (Smart Animate)

**Result:** Click button → Modal smoothly animates in.

**Smart Animate vs. Instant:**
- **Smart Animate:** Smoothly transitions (good for UX, feels native)
- **Instant:** Jumps to next frame (good for testing logic)

### Variable-Driven Interactions

Use variables to create complex interactions.

**Example: Toggle button**
1. Create boolean variable: `isToggled`
2. Two button states: "Toggled Off" and "Toggled On"
3. On click: Toggle isToggled variable
4. Button appearance driven by isToggled (color, text changes)

**Result:** Click button → Variable changes → Button appearance updates.

---

## Collaboration & Handoff

### Sharing & Permissions

**Shared libraries:**
- One file = source of truth (Design System file)
- Other files link to it (Project files)
- Any change to library updates all linked files

**Setup:**
1. Design System file → Assets → Publish library
2. Project file → Add file → Search Design System file
3. Now project file can use Design System components
4. Updates to Design System components appear in project file

**Permissions:**
- **Owner:** Full access
- **Editor:** Can edit (in Drafts) but not publish
- **Viewer:** Read-only
- **Developer:** Read-only (good for engineers)

### Design-to-Code Handoff

**Bad handoff:**
- Designer exports screenshots
- Engineer has to guess spacing, colors, typography
- Inconsistencies emerge
- Rework required

**Good handoff:**
- Figma Design tokens sync to code (via tools like Design Token Bridge)
- Components have clear specs (width, height, padding, font size)
- Colors are tokenized (color/primary, not #2563eb)
- Typography is tokenized (typography/body, not "Roboto 16px 400")

**Tools for handoff:**
- **Figma specs:** Export component specs as HTML/JSON
- **Design tokens bridge:** Sync Figma variables → CSS/JSON
- **Storybook:** Embed Figma links in Storybook
- **Zeroheight:** Visual documentation with Figma embeds

### Developer Handoff Checklist

Before handing off to engineers:
- [ ] All components are variants (not separate components)
- [ ] Component names match codebase (Button, not Btn)
- [ ] All colors are tokenized (color/primary, not #2563eb)
- [ ] All typography is tokenized (typography/body, not font settings)
- [ ] All spacing uses tokens (space/md, not 16px)
- [ ] Dark mode variables are set up
- [ ] Responsive breakpoints documented
- [ ] Accessibility notes included (ARIA roles, keyboard behavior)
- [ ] Naming is consistent with code (camelCase or kebab-case)
- [ ] Cover page has links to design system docs, component specs, dev setup

---

## Performance & Organization

### File Size Optimization

Large files slow down Figma.

**Keep file size down:**
- Delete unused components (Archive old versions instead)
- Flatten non-component shapes (if not reused)
- Compress images (use "Optimize image" in Assets)
- Limit zoom levels in prototypes (simpler = faster)
- Use high-fidelity prototypes only when needed (remove once tested)

**Warning signs:**
- File takes >5 seconds to open
- Zoom/pan is laggy
- Prototype is slow to interact with
- → Time to split into multiple files

### Multiple Files vs. One Large File

**One large file:**
- ✅ Easy to manage (one source of truth)
- ❌ Slow (file size bloats)
- ❌ Hard to organize (too much content)
- ❌ Conflicts when multiple people edit

**Multiple files (recommended):**
- Design System file (shared library)
- Project files per feature/team
- Each project file links to Design System library
- ✅ Fast (smaller files)
- ✅ Organized
- ✅ Parallel work (less conflicts)

**Strategy:**
```
Design System (main library, published)
├── Shared: All colors, typography, base components
└── Linked from:
    ├── Project A (Dashboard features)
    ├── Project B (Auth features)
    ├── Project C (Settings features)
    └── Project D (Onboarding features)
```

---

## Useful Plugins

Figma plugins extend capabilities.

**Design System plugins:**
- **Design Tokens:** Sync Figma variables to CSS/JSON (code)
- **Component Organizer:** Organize components by group
- **Storybook:** Embed Storybook stories in Figma

**Workflow plugins:**
- **Unsplash:** Insert free images
- **LottieFiles:** Insert animations
- **Rename It:** Bulk rename layers
- **Figma to HTML:** Export frames as HTML
- **Wireframer:** Quick wireframe templates

**Quality plugins:**
- **Contrast:** Check WCAG contrast compliance
- **Color Blindness:** Simulate colorblind vision
- **A11y Highlight:** Highlight missing alt text, labels

**How to find plugins:**
1. Figma → Resources → Plugins
2. Search by name or category
3. Install (one-click)
4. Use from Figma menu

---

## When to Use This Skill

✅ **Do use for:**
- Figma component strategy and setup
- Design tokens and variables management
- Component variants and auto layout
- Prototyping workflows
- Collaborative design workflows
- Design-to-code handoff
- Figma file organization
- Performance optimization
- Plugin recommendations

❌ **Don't use for:**
- General design principles (see design-fundamentals)
- Accessibility in components (see web-accessibility-a11y or inclusive-design-patterns)
- Design system architecture (see design-systems-architecture)
- CSS implementation (see css-styling-pixel-perfect)
- React component code (see frontend-framework-guide)
