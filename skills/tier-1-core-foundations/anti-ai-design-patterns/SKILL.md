---
name: anti-ai-design-patterns
description: Detect and eliminate AI-generated design tells — "AI slop" and generic "AI-alien" aesthetics — so interfaces read as human-crafted and intentional. Use this skill whenever generating or reviewing any UI, component, landing page, email, or design-token set. Triggers include: single-side accent borders / left-rail cards, emoji or emoticons used as icons or section markers, default purple-to-blue gradients, everything-centered layouts, uniform rounded-lg corners everywhere, raw default Tailwind palette colors, Inter/Space-Grotesk-as-the-safe-default, cream + serif + terracotta clichés, lorem ipsum, glassmorphism on everything, uncanny uniform spacing, and any request to "make this not look AI-generated", "de-slop this UI", or "use an icon library instead of emoji". Pairs with design-fundamentals and css-styling-pixel-perfect.
---

# Anti-AI Design Patterns

A field guide to the visual tells that mark an interface as machine-generated, and the intentional choices that replace them. The goal is not a *different* template — it is design that reads as **deliberate**: chosen neutrals, a real type system, asymmetry with reason, and iconography that belongs to one family.

## Core principle: intentional beats default

An AI tell is almost never a single ugly element. It is the *absence of a decision* — the palette that was inherited instead of chosen, the border that decorates instead of encodes, the emoji dropped in because a real icon was more work. Every rule below reduces to one question:

> **Did a human decide this, or did it fall out of a default?**

If you cannot state *why* a color, radius, border, or glyph is there, it is probably a tell.

---

## 1. The border tell — single-side accent rails

The single most common AI slop signature: a rounded card with a colored stripe on **one** side (usually left) and neutral borders on the other three.

```css
/* ❌ AI TELL — accent rail on a rounded card */
.card {
  border: 1px solid #e5e7eb;
  border-left: 4px solid #6366f1;   /* the giveaway */
  border-radius: 12px;
}
```

Why it reads as AI: the accent has no informational job — it is applied to *every* card regardless of state, so it decorates rather than encodes. A colored edge on a rounded corner also fights the geometry (the stripe clips awkwardly at the radius).

**Fixes — pick based on intent:**

```css
/* ✅ A. If the color means nothing: drop it. Let type + spacing carry the card. */
.card { border: 1px solid var(--line); border-radius: 12px; }

/* ✅ B. If the color must encode STATE (error/warn/success): use a full token
   set — background tint + text + a same-color full border, not a lone rail. */
.card--critical {
  border: 1px solid var(--danger-line);
  background: var(--danger-bg);
  color: var(--danger-ink);
}

/* ✅ C. If you genuinely want an edge accent, make it a deliberate full-bleed
   top border on a SQUARE-topped card, or an inset marker — a decision, not a default. */
.stat { border-top: 3px solid var(--accent); border-radius: 0 0 10px 10px; }
```

Rule of thumb: **a colored border must earn its place by encoding something true** (severity, category, selection). If it is on everything, it is on nothing.

---

## 2. The emoji / emoticon tell — use an icon library only

Emoji as UI is the loudest AI-alien signal. It renders differently on every OS, breaks visual weight, ignores your stroke width, and cannot inherit color or a11y semantics.

```jsx
{/* ❌ AI TELL — emoji as icons / section markers / bullets */}
<h2>🎨 Design</h2>
<li>✅ Ship it</li>
<button>🚀 Deploy</button>
<p>Nice work! 😄</p>          {/* emoticons in product copy */}
```

**Rule: emoji and emoticons never appear in product UI. Use one icon library.**

```jsx
{/* ✅ Icons from a single library — inherits color, size, stroke, a11y */}
import { Palette, Check, Rocket } from "lucide-react";

<h2><Palette size={18} aria-hidden="true" /> Design</h2>
<li><Check size={16} aria-hidden="true" /> Ship it</li>
<button><Rocket size={16} aria-hidden="true" /> Deploy</button>
```

Icon-library discipline (see `references/icon-library-guide.md`):
- **One family per product.** Never mix Lucide + Heroicons + Font Awesome — differing stroke widths and metaphors read as incoherent.
- Recommended sets: **Lucide**, **Heroicons**, **Phosphor**, **Radix Icons**, **Tabler**. Pick one.
- Size on a scale (16 / 20 / 24), match the icon stroke to your type weight, align to the text baseline.
- Decorative icon → `aria-hidden="true"`. Meaningful icon (icon-only button) → `aria-label`.
- Never rasterize emoji into an `<img>` to "use it as an icon." That is the same tell with extra steps.

> Exception: genuinely emoji-native surfaces (a chat reactions bar, an emoji picker, a flag/country selector) — there the emoji *is* the content, not decoration.

---

## 3. The palette tell — inherited, not chosen

```css
/* ❌ AI TELL — raw framework defaults, straight out of the box */
color: #6366f1;                 /* Tailwind indigo-500, untouched */
background: linear-gradient(135deg, #a855f7, #3b82f6);  /* the purple→blue hero */
--gray: #808080;                /* pure, hueless mid-grey */
```

Tells: the default Tailwind/Bootstrap swatches used verbatim, the purple-to-blue gradient hero, and pure hueless greys.

```css
/* ✅ Choose a palette. Bias neutrals slightly toward the accent hue so they
   read as selected, not inherited. Define as tokens; style through tokens. */
:root {
  --accent: #0c7c7c;            /* a specific teal, not indigo-500 */
  --ink:    #1a1d23;            /* near-black with a cool bias */
  --ink-soft:#575d68;           /* grey that leans blue toward the accent */
  --line:   #dce0e6;            /* borders share the same bias */
}
```

Guidance:
- **Never ship raw default swatches.** Shift hue/lightness even slightly so the palette is yours.
- Neutrals get a subtle hue bias toward the accent — a pure `#808080` is the tell.
- Spend boldness in **one** place; keep everything around it quiet. If the accent fights the ground, drop saturation or move to an analogous hue rather than adding a second loud color.
- Semantic colors (success/warn/danger) are a *separate* system from your brand accent.

---

## 4. The layout & shape tells

| Tell | Fix |
|---|---|
| Everything centered (`text-align:center` on whole sections) | Use a real grid; left-align running text; center only what deserves emphasis |
| One radius everywhere (`rounded-lg` on cards, buttons, inputs, avatars) | A radius *scale* (e.g. 6 / 10 / 16 / full) mapped to element size |
| Perfect symmetry, no focal point | Intentional asymmetry — an off-center hero, an uneven 60/40 split |
| Uniform spacing (everything `gap-4`) | A spacing scale with rhythm — tight within groups, generous between them |
| Drop shadow + glassmorphism on every surface | Elevation as a *system* (0/1/2 levels); most surfaces are flat |
| Pill/badge on every label | Badges encode state; plain text labels for everything else |

---

## 5. The typography tell

```
❌ Inter (or Space Grotesk) as the reflexive "safe" choice for every project,
   at one weight, one size, no scale.
```

Fixes:
- Pair **two** deliberate typefaces (a display face used with restraint + a readable body face); add a mono utility face for data/labels if the subject is technical.
- Commit to a **type scale** and stay on it. Give headings `text-wrap: balance`, body ~65ch line length, uppercase labels a touch of letter-spacing.
- Inter is not banned — but if it is there because it is the default, that is the tell. Choose it *for a reason*.

---

## 6. The content tell

- **Lorem ipsum** is an instant AI/placeholder signal. Always write real, specific copy.
- Fake data that looks fake ("John Doe", "$1,234.56", "Lorem Company") — use plausible, varied, domain-true content.
- Copy written from the system's side ("webhook config") instead of the user's ("Notifications"). See `design-fundamentals`.
- Em-dash-heavy, evenly-hedged marketing prose — write specific, active, human copy.

---

## 7. The "AI-alien" uncanny tells

Subtler signals that something is *off* even when nothing is obviously wrong:

- **Too consistent.** Real design has intentional exceptions; machine output is eerily uniform. Vary card heights, let one item break the grid on purpose.
- **Gradient text** on headings, rainbow or purple→pink. Almost always a tell.
- **Icon + emoji mixed** in the same list. Pick one system (icons).
- **Over-rounded everything** including images and section containers.
- **Centered max-w-prose everything** with no layout ambition.
- **Decorative dividers/numbers (01 / 02 / 03)** on content that is not actually a sequence. Structure must encode something true.

---

## Workflow: generate, then de-slop

1. **Build to spec** using the other skills (`design-fundamentals`, `css-styling-pixel-perfect`, etc.).
2. **Run the anti-AI review pass** — walk `references/review-checklist.md` against the output.
3. **For each flagged item, state the decision** it should encode. If there is none, remove it.
4. **Swap every emoji/emoticon** for an icon from the chosen library; verify a single family.
5. **Verify tokens** — no raw default swatches, neutrals biased, one accent.

## When NOT to over-correct

Avoiding AI slop is not "add more decoration." Restraint is the point:
- A clean, flat, well-spaced page with one accent is *never* the wrong answer.
- Do not replace an emoji tell with an over-designed icon-and-gradient tell.
- A deliberately single-theme, minimal aesthetic is a valid choice, not an omission.

## References
- `references/ai-tells-catalog.md` — full before/after catalog with code.
- `references/icon-library-guide.md` — choosing and using one icon set correctly.
- `references/review-checklist.md` — the de-slop audit rubric.
