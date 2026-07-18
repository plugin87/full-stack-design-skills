# Figma Component Setup: From Basic to Advanced

Practical step-by-step patterns for setting up professional Figma components.

---

## Pattern 1: Simple Button Component

### Step 1: Create the Base Shape

```
Button frame (200 × 44)
├── Auto Layout: Horizontal
├── Padding: 12px vertical, 16px horizontal
├── Gap: 8px (between icon and text)
├── Rounded corners: 8px
└── Fill: #2563eb (primary blue)

Inside:
├── Icon (optional): 20 × 20
└── Text: "Click me" (default)
```

### Step 2: Convert to Component

1. Select the frame
2. Right-click → "Create component"
3. Rename: "Button"

### Step 3: Add Variants

Create Size variant (Small, Medium, Large):

```
Frame: Button/Size=Small
├── 160 × 36
├── Font size: 12px
├── Padding: 8px / 12px

Frame: Button/Size=Medium
├── 200 × 44
├── Font size: 16px
├── Padding: 12px / 16px

Frame: Button/Size=Large
├── 240 × 52
├── Font size: 18px
├── Padding: 16px / 20px
```

### Step 4: Add Style Variant

Create Style variant (Primary, Secondary, Ghost):

```
Primary:
├── Fill: #2563eb (blue)
├── Text: white
└── Border: none

Secondary:
├── Fill: #f1f5f9 (light gray)
├── Text: #1e293b (dark)
└── Border: 1px solid #cbd5e1

Ghost:
├── Fill: transparent
├── Text: #2563eb
└── Border: 1px solid #2563eb
```

### Step 5: Add State Variant

Create State variant (Default, Hover, Active, Disabled):

```
Default:
├── Opacity: 100%
├── Fill: full color

Hover:
├── Opacity: 100%
├── Fill: 10% darker

Active:
├── Opacity: 100%
├── Fill: 20% darker

Disabled:
├── Opacity: 50%
├── Fill: gray (#9ca3af)
├── Cursor: not-allowed (in prototype)
```

### Result

Single Button component with:
- 3 sizes × 3 styles × 4 states = 36 variants
- All managed in one component
- Single source of truth
- Easy to maintain

---

## Pattern 2: Form Field Component

### Anatomy

```
FormField frame (300 × auto)
├── Auto Layout: Vertical
├── Gap: 4px

Children:
├── Label text "Email" (12px, bold)
├── Input field (300 × 40, with padding)
└── Error message "Invalid email" (12px, red, optional)
```

### Implementation

```
1. Create Label:
   - Text: "Email address"
   - Size: 12px
   - Weight: 600
   - Color: #1e293b

2. Create Input field:
   - Frame: 300 × 40
   - Auto Layout: Horizontal
   - Padding: 8px 12px
   - Border: 1px solid #cbd5e1
   - Rounded: 6px
   - Placeholder text: "user@example.com" (optional)

3. Create Error message:
   - Text: "Invalid email address"
   - Size: 12px
   - Color: #dc2626 (red)
   - Hidden by default (toggle in variant)

4. Wrap all in FormField auto layout frame
```

### Variants

```
Variant: Type/Email, State/Default, Filled/Empty
Variant: Type/Email, State/Default, Filled/Text
Variant: Type/Email, State/Focus, Filled/Text
Variant: Type/Email, State/Error, Filled/Text (show error message)
Variant: Type/Password, State/Default, Filled/Empty
Variant: Type/Text, State/Disabled, Filled/Empty
... (multiple combinations)
```

### Key Details

- **Label:** Always visible (required for a11y)
- **Placeholder:** Hint text inside input (disappears on focus)
- **Error message:** Shows conditionally (variant hidden when not error state)
- **Focus state:** Border color changes, shadow added
- **Disabled state:** Opacity 50%, cursor not-allowed

---

## Pattern 3: Card Component

### Anatomy

```
Card frame (300 × auto)
├── Auto Layout: Vertical
├── Gap: 16px
├── Padding: 16px (all sides)
├── Border: 1px solid #e2e8f0
├── Rounded: 12px
├── Background: white

Children:
├── Image (300 × 200)
├── Title (24px, bold)
├── Description (16px, muted)
└── Action (Button or Link)
```

### Variants

```
Variant: Layout/Image, Type/Product, Size/Large
└── Image at top, full width

Variant: Layout/Compact, Type/Article, Size/Small
└── Image hidden (or thumbnail in corner)

Variant: Elevation/Flat
└── No shadow, just border

Variant: Elevation/Elevated
└── Shadow: 0 2px 8px rgba(0,0,0,0.1)
```

### With Auto Layout

```
Card with Auto Layout:
├── Gap: 12px (between children)
├── Padding: 16px (around edges)

Image:
├── Auto layout: OFF
├── 100% width of parent
├── 200px height
├── No manual spacing (auto layout handles it)

Title:
├── Auto layout: OFF
├── 100% width (grows with parent)
├── Font: 18px / 600 weight

Description:
├── Auto layout: OFF
├── 100% width
├── Font: 14px / 400 weight
└── Line height: 1.5

Action button:
├── Defined size (160 × 40)
├── Or "hug contents" if auto layout
```

---

## Pattern 4: Modal/Dialog Component

### Anatomy

```
Modal frame (500 × auto, centered on screen)
├── Auto Layout: Vertical
├── Padding: 24px
├── Gap: 16px
├── Background: white
├── Border radius: 12px
├── Shadow: 0 20px 25px rgba(0,0,0,0.2)

Children:
├── Header
│   ├── Title: 20px, bold
│   ├── Close button (icon only)
│   └── Divider line (optional)
├── Body
│   └── Content (variable height)
├── Footer
│   ├── Button: Cancel
│   ├── Button: Confirm (primary)
│   └── Auto Layout: Horizontal, right-aligned
```

### Variants

```
Variant: Type/Confirmation, Size/Small
- Simple confirmation dialog
- Title + message + 2 buttons

Variant: Type/Form, Size/Large
- Form with fields
- Title + form fields + buttons

Variant: Type/Alert, Size/Medium
- Alert or warning
- Icon + title + message + 1 button

Variant: State/Open
- Visible modal

Variant: State/Closed
- Hidden (or separate non-modal frame for design)
```

### Responsive Notes

- Fixed width on desktop (500px)
- Responsive on mobile (100% - 32px padding, max 500px)
- Use prototype modes for responsive

---

## Pattern 5: Complex Data Table

### Anatomy

```
Table frame (100% × auto)
├── Auto Layout: Vertical
├── Gap: 0 (no gap between rows)

Header row:
├── Auto Layout: Horizontal
├── Padding: 12px per cell
├── Background: #f8fafc
├── Text: 12px, bold, #64748b
├── Children: Name column, Email column, Status column, Actions column

Data rows (repeated):
├── Auto Layout: Horizontal
├── Padding: 12px per cell
├── Background: white (or alternating #f9fafb)
├── Text: 14px, #1e293b
├── Children: name, email, status badge, action menu

Footer (optional):
├── Pagination info
├── Pagination buttons
```

### Making It Responsive

**Desktop (1200px+):**
- All columns visible
- Actions menu: horizontal (Edit, Delete, etc.)

**Tablet (768px):**
- Hide secondary columns
- Actions menu: dots (...)

**Mobile (< 768px):**
- Stack as card list instead of table
- Use separate Card component variant

### Variants

```
Variant: Density/Compact
├── Padding: 8px per cell
├── Font size: 12px
└── Height: tight

Variant: Density/Comfortable
├── Padding: 12px per cell
├── Font size: 14px
└── Height: standard

Variant: State/Empty
├── Show "No data" message
└── Hide rows

Variant: State/Loading
├── Show skeleton loaders
└── Hide actual data
```

---

## Common Mistakes to Avoid

### ❌ Creating Individual Components for Each Variant

Wrong: Button-Small, Button-Medium, Button-Large (3 components)
Right: Button with Size variant (1 component, cleaner)

### ❌ Not Using Auto Layout

Wrong: Manual spacing, have to adjust every element when content changes
Right: Auto Layout, everything reflows automatically

### ❌ Detaching Instances from Main Component

Wrong: Instance → Right-click → Detach (now it's not updated when main changes)
Right: Keep instances attached, modify main component for updates

### ❌ Inconsistent Naming

Wrong: btn, button, Button-Default, buttonPrimary (inconsistent)
Right: Button, Button/Size=Large, Button/Style=Primary (consistent)

### ❌ Not Using Shared Styles

Wrong: Every button has unique color values (#2563eb)
Right: All buttons use shared style or variables (color/primary)

---

## Performance Tips

1. **Flatten repeated elements** — Don't create 100 individual button instances when you can create 1 component
2. **Compress images** — Use Figma's image optimization
3. **Remove unused components** — Archive old versions, don't leave them taking up space
4. **Use simple colors** — Solid fills are faster than gradients
5. **Limit shadow effects** — Keep to realistic shadows (blur ≤ 12px)
6. **Organize layers** — Group related elements (don't have 200 top-level layers)

---

## Checklist: Component Ready for Handoff?

- [ ] Component has descriptive name (Button, not BTN or Btn)
- [ ] All variants are created (size, style, state)
- [ ] Auto layout is configured (not manual spacing)
- [ ] Padding/gap use consistent spacing tokens
- [ ] Colors use variables (not hex values)
- [ ] Typography uses shared styles (not manual font settings)
- [ ] Dark mode variant exists (if applicable)
- [ ] Accessibility notes are documented (ARIA roles, keyboard behavior)
- [ ] Component is locked (prevent accidental edits)
- [ ] Nested components are used (Button inside Card, not duplicated)
- [ ] File size is reasonable (< 50MB per file)
