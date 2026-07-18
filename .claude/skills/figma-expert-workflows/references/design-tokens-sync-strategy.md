# Design Tokens Sync: Figma to Code

Strategy and tools for syncing design tokens from Figma to CSS, JSON, and code.

---

## The Problem: Manual Token Updates

**Without token sync:**
```
Designer in Figma updates color:
- color/primary: #2563eb → #3b82f6

Developer must manually update:
- CSS: --color-primary: #3b82f6
- JSON: { "color": { "primary": "#3b82f6" } }
- React constants: export const PRIMARY = '#3b82f6'
- Tailwind config: primary: '#3b82f6'

Result: Inconsistencies, bugs, wasted time
```

**With token sync:**
```
Designer updates in Figma:
- color/primary: #3b82f6

Tool syncs automatically:
- CSS ✓
- JSON ✓
- React constants ✓
- Tailwind config ✓

Everyone has the latest tokens
```

---

## Step 1: Set Up Figma Variables

### Create Variable Collection

1. **Open Figma file**
2. **Assets panel** → Variables
3. **Create collection**: "Design Tokens"
4. **Add mode**: "Light" (default mode)

### Add Variables

Organize by category:

```
Colors:
├── color/primary: #2563eb
├── color/secondary: #64748b
├── color/success: #10b981
├── color/warning: #f59e0b
└── color/danger: #ef4444

Spacing:
├── space/xs: 4
├── space/sm: 8
├── space/md: 16
├── space/lg: 24
└── space/xl: 32

Typography:
├── font-family/sans: "Inter"
├── font-size/body: 16
├── font-size/heading: 24
├── line-height/normal: 1.5
└── font-weight/bold: 700

Border:
├── radius/sm: 4
├── radius/md: 8
└── radius/lg: 12
```

### Add Light/Dark Modes

1. **Variables panel** → Click "Mode +"
2. Create "Dark" mode
3. Override values for dark mode:
   ```
   Light mode:
   - color/primary: #2563eb

   Dark mode:
   - color/primary: #60a5fa
   ```

---

## Step 2: Choose Sync Tool

### Option 1: Design Tokens Bridge (Free)

**What it does:**
- Syncs Figma variables → CSS custom properties
- Syncs Figma variables → JSON
- One-way sync (Figma → Code)

**Setup:**
1. Install plugin: "Design Tokens Bridge" (Figma plugins)
2. Select variables to export
3. Generate CSS file
4. Copy to codebase

**Output (CSS):**
```css
:root {
  --color-primary: #2563eb;
  --color-secondary: #64748b;
  --space-md: 16;
  --font-size-body: 16;
}

@media (prefers-color-scheme: dark) {
  :root {
    --color-primary: #60a5fa;
  }
}
```

**Output (JSON):**
```json
{
  "color": {
    "primary": {
      "light": "#2563eb",
      "dark": "#60a5fa"
    },
    "secondary": "#64748b"
  },
  "space": {
    "md": 16
  }
}
```

### Option 2: Tokens Studio (Figma Plugin)

**What it does:**
- Stores tokens in Figma (visual + JSON)
- Syncs to GitHub (commits JSON files)
- Supports multiple sync destinations

**Setup:**
1. Install plugin: "Tokens Studio for Figma"
2. Create token sets in plugin panel
3. Configure sync: GitHub, Gitlab, etc.
4. Commit JSON files to repo
5. Code build pipeline reads tokens

**Workflow:**
```
Designer updates token in Figma
↓
Tokens Studio plugin
↓
Commits to GitHub tokens branch
↓
Build pipeline runs
↓
Generates CSS/JSON/TypeScript
↓
All consumers (web, iOS, Android) have latest tokens
```

### Option 3: Custom Figma API + Script

**What it does:**
- Uses Figma API to fetch variables
- Custom script converts to your format
- Maximum flexibility

**Setup:**
```javascript
// 1. Get Figma API token
// 2. Create script
const figmaToken = 'YOUR_FIGMA_API_TOKEN';
const fileId = 'YOUR_FILE_ID';

// 3. Fetch variables from Figma
const response = await fetch(
  `https://api.figma.com/v1/files/${fileId}/variables`,
  { headers: { 'X-Figma-Token': figmaToken } }
);

// 4. Parse and convert
const variables = await response.json();
const cssOutput = convertToCSS(variables);

// 5. Write to file
fs.writeFileSync('tokens.css', cssOutput);
```

---

## Step 3: Integrate with Build Pipeline

### CI/CD Integration

**GitHub Actions example:**
```yaml
name: Sync Design Tokens

on:
  workflow_dispatch

jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Sync Figma tokens
        run: |
          npm run tokens:sync
      
      - name: Commit changes
        run: |
          git config user.name "Design Tokens Bot"
          git config user.email "bot@example.com"
          git add tokens/*
          git commit -m "chore: sync design tokens from Figma"
          git push
```

**npm script:**
```json
{
  "scripts": {
    "tokens:sync": "figma-tokens export --output ./src/tokens",
    "tokens:watch": "figma-tokens export --watch"
  }
}
```

---

## Step 4: Consume Tokens in Code

### CSS Custom Properties

```css
/* Import tokens */
@import 'tokens/tokens.css';

/* Use tokens */
.button {
  background-color: var(--color-primary);
  padding: var(--space-md);
  border-radius: var(--radius-md);
  font-size: var(--font-size-body);
}

/* Dark mode */
@media (prefers-color-scheme: dark) {
  .button {
    background-color: var(--color-primary); /* CSS var updates automatically */
  }
}
```

### JavaScript/TypeScript

```typescript
// Import token JSON
import tokens from './tokens/tokens.json';

// Use in code
const buttonColor = tokens.color.primary; // #2563eb
const spacing = tokens.space.md; // 16

// React example
<div style={{ color: tokens.color.primary }}>
  Hello
</div>
```

### Tailwind Config

```javascript
// tailwind.config.js
const tokens = require('./tokens/tokens.json');

module.exports = {
  theme: {
    colors: tokens.color,
    spacing: tokens.space,
    borderRadius: tokens.radius,
  }
};
```

### SCSS Variables

```scss
// _tokens.scss (generated from Figma)
$color-primary: #2563eb;
$color-secondary: #64748b;
$space-md: 16px;

// Use in components
.button {
  background-color: $color-primary;
  padding: $space-md;
}
```

---

## Best Practices

### 1. Naming Consistency

**Figma variable name:**
```
color/primary
```

**Generated CSS custom property:**
```css
--color-primary
```

**Generated JSON path:**
```
tokens.color.primary
```

All reference the same token, just different syntax.

### 2. Semantic vs. Primitive

**Primitive tokens** (raw values):
```
color/red-600: #dc2626
color/blue-600: #2563eb
```

**Semantic tokens** (meaningful):
```
color/primary: color/blue-600
color/danger: color/red-600
```

**In Figma variables:**
- Set semantic colors to reference primitive colors
- Designers use semantic colors (primary, danger)
- If primitive changes, semantic updates automatically

### 3. Versioning Tokens

When you make breaking changes:
```
Version 1.0: color/primary: #2563eb
Version 2.0: color/primary: #3b82f6

In package.json:
"@design-tokens/core": "^2.0.0"
```

Document breaking changes in CHANGELOG.

### 4. Documentation

Include a README in tokens folder:
```
# Design Tokens

## Token categories
- **Color:** brand and semantic colors
- **Space:** padding, margin, gap
- **Typography:** font sizes, weights, line heights

## Sync
Tokens are synced from Figma. To update:
1. Edit in Figma Design System file
2. Run `npm run tokens:sync`
3. Commit changes

## Consuming tokens
- CSS: `var(--color-primary)`
- JS: `import tokens from 'tokens.json'; tokens.color.primary`
- Tailwind: Uses tokens in config automatically
```

---

## Troubleshooting

### Tokens not updating?

1. Check Figma API token is valid
2. Ensure variables are in correct collection
3. Verify export script has correct file ID
4. Check build log for errors
5. Run `npm run tokens:sync` manually

### Naming conflicts?

If two variables have same semantic meaning:
```
❌ Bad:
color/primary-button: #2563eb
color/link-primary: #2563eb

✅ Good:
color/primary: #2563eb
(Use in both button and link)
```

### Dark mode not working?

Make sure:
1. Variables have Light and Dark modes defined
2. CSS includes dark mode media query
3. Consumer code uses `@media (prefers-color-scheme: dark)`

### Performance issues?

- Limit number of tokens (no more than 500)
- Archive old token files
- Consider splitting tokens by feature
- Load tokens asynchronously

---

## Workflow: Start to Finish

**Week 1:**
1. Set up Figma variables (colors, spacing, typography)
2. Choose sync tool (recommend: Tokens Studio or Design Tokens Bridge)
3. Generate initial token files

**Week 2:**
4. Integrate into build pipeline (CI/CD)
5. Update codebase to use tokens (CSS, JS, Tailwind)
6. Test light/dark modes

**Ongoing:**
7. Designers update tokens in Figma
8. Tokens sync automatically
9. Developers use latest tokens
10. No manual sync needed (automation handles it)

---

## Tools Comparison

| Tool | Cost | Setup | Automation | Flexibility |
|------|------|-------|------------|-------------|
| Design Tokens Bridge | Free | 30 min | Manual | Low |
| Tokens Studio | Paid ($60/mo) | 1 hour | Full | High |
| Custom API script | Free | 2 hours | Full | Very high |

**Recommendation:**
- **Start:** Use Tokens Studio (easiest, well-maintained)
- **Scale:** Build custom solution if needed
