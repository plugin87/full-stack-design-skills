# Icon Library Guide — No Emoji, One Family

Emoji and emoticons must never be used as iconography in product UI. This guide covers choosing one icon library and using it correctly.

## Why not emoji

| Problem | Consequence |
|---|---|
| OS-rendered | 🎨 looks different on macOS, Windows, Android, and in every browser |
| Fixed color | Cannot inherit `currentColor`; ignores light/dark theme |
| No stroke control | Breaks the visual weight of a stroke-based icon set |
| Weak a11y | Screen readers announce inconsistent names ("artist palette") |
| Metaphor drift | Emoji semantics are imprecise; a 🚀 is not a "deploy" icon |

## Choosing a library (pick ONE)

| Library | Style | Import (React) |
|---|---|---|
| **Lucide** | Clean, 2px stroke, huge set (default recommendation) | `lucide-react` |
| **Heroicons** | Tailwind-native, outline + solid | `@heroicons/react` |
| **Phosphor** | Flexible weights (thin→fill) | `@phosphor-icons/react` |
| **Radix Icons** | 15px grid, crisp for dense UI | `@radix-ui/react-icons` |
| **Tabler** | 3000+ icons, 2px stroke | `@tabler/icons-react` |

**Rule: one family per product.** Mixing sets (different stroke widths, corner styles, optical sizes) is itself an incoherence tell.

## Correct usage

```jsx
import { Check, TriangleAlert, Rocket, Search } from "lucide-react";

// Decorative (next to a text label) → hide from AT
<button><Rocket size={16} aria-hidden="true" /> Deploy</button>

// Meaningful (icon-only control) → label it
<button aria-label="Search"><Search size={20} aria-hidden="true" /></button>

// State icon inherits the semantic token color
<span className="alert"><TriangleAlert size={16} aria-hidden="true" /> Quota exceeded</span>
```

### Sizing & alignment
- Size on a scale: **16** (inline/dense), **20** (default), **24** (touch/primary).
- Match icon stroke weight to adjacent text weight; a bold heading pairs with a heavier icon weight (Phosphor) or larger size.
- Vertically center against the text using flex `align-items: center` and a small `gap` (6–8px), not manual margins.
- Use `currentColor` (the default in these libs) so icons theme automatically.

### Accessibility
- Decorative icon beside text: `aria-hidden="true"`.
- Icon-only interactive element: `aria-label` (or visually-hidden text).
- Never rely on an icon alone to convey status — pair with text or a label.

## Non-React / plain HTML

Use SVG sprites or inline SVG from the same set — still never emoji.

```html
<svg width="16" height="16" aria-hidden="true"><use href="/icons.svg#check"></use></svg>
```

## The one allowed exception

Emoji is acceptable only where it **is the content**, not decoration:
- Emoji picker / reactions bar
- Country flag / language selector
- User-generated content that literally contains emoji

Everywhere else: icon library.
