# Examples — Skills Applied

Three self-contained reference implementations that exercise the 15-skill curriculum
on real UI. Each is a single HTML file built with **Tailwind CSS** + the **Lucide**
icon library (both via CDN), driven by one CSS-variable token layer, and held to the
`anti-ai-design-patterns` de-slop checklist.

## Run them

No build step. Just open a file in a browser — Tailwind and Lucide load from their CDNs:

```bash
open component-showcase.html      # macOS
# or serve the folder:
python3 -m http.server 8000       # then visit http://localhost:8000/
```

> The Tailwind Play CDN prints a "not for production" console warning — expected for a
> demo. For production, compile Tailwind to a static CSS file and self-host Lucide.

## The three examples

| File | What it is | Primary skills exercised |
|---|---|---|
| `bootstrap.html` | **Bootstrap 5.3 option** — real Bootstrap loaded from CDN with the primary retinted to the brand pink (#E61876) via proper `--bs-*` variable overrides, Bootstrap Icons, native `data-bs-theme` light/dark. Buttons, forms (floating labels + validation), alerts, badges, progress, cards, list group, pagination, accordion, modal. A framework alternative to the flat token system. | design-systems-architecture · css-styling-pixel-perfect · component-library-mastery |
| `material-design.html` | **Material Design 3 option** — the same brand pink (#E61876 seed) expressed as an M3 system: tonal color roles, Roboto, Material Symbols, elevation, state layers, pill buttons, filled/outlined text fields, switch/chips, cards, snackbar, dialog. A *stylistic alternative* to the flat token system (deliberately elevation-heavy, per Material's own rules). | design-systems-architecture · design-fundamentals · css-styling-pixel-perfect |
| `design-system.html` | **The documentation** — living reference for foundations (color ramp, gradient, typography, radius, elevation, icons), components, the a11y/anti-ai checklist, and the full token table | design-systems-architecture · design-tokens-system · design-fundamentals · anti-ai-design-patterns |
| `component-showcase.html` | A component library page — buttons, forms, feedback, badges, table, accessible modal, skeleton, plus a live de-slop self-audit | design-fundamentals · css-styling-pixel-perfect · component-library-mastery · web-accessibility-a11y · inclusive-design-patterns · anti-ai-design-patterns |
| `dashboard.html` | A design-system health dashboard — KPI tiles with sparklines, an interactive area chart (hover crosshair + tooltip), a11y-issue severity, Core Web Vitals, adoption bars, deploy feed | dataviz · design-systems-architecture · web-performance-optimization · responsive-universal-design · qa-testing-visual-regression · deployment-devops-workflow |
| `registration-form.html` | A registration flow — inline validation, a password-strength meter, show/hide toggle, accessible error handling (icon+text, `aria-invalid`, focus management), and a success state | web-accessibility-a11y · inclusive-design-patterns · frontend-framework-guide · design-tokens-system |

Together they touch all 15 skills. `figma-expert-workflows` and `design-to-code-workflow`
show up as the data-driven, spec-shaped component patterns across all three.

## What every example demonstrates (the shared contract)

- **One token layer** — Tailwind colors map to CSS variables; dark mode flips values only. Brand/CI color is **#E61876** (magenta).
- **Typography** — a native **system-ui sans stack** (no bundled fonts). The originals used a commercial brand typeface; swap in your own licensed font at the `font-sans` / `--bs-body-font-family` token.
- **Both themes designed** — light + dark, not a naive invert; respects OS preference + a toggle.
- **Lucide only** — every icon is from one library; zero emoji in the UI.
- **De-slop** — flat cards (no accent rails), a chosen brand palette (magenta #E61876, not `indigo-500`),
  a radius scale, an elevation system, status shown as icon **and** text.
- **Accessible** — skip links, visible focus rings, labels, ARIA, keyboard-operable
  dialogs, `prefers-reduced-motion` respected.
- **Verified in-browser** — each was opened in Chrome, interactions driven, and console
  checked clean before shipping.

## Notes

- `registration-form.html` is a **demo** — nothing is submitted; validation and the
  success state run entirely client-side.
- Charts in `dashboard.html` follow the `dataviz` skill: single-hue magnitude + reserved
  status colors (the categorical multi-hue palette failed the CVD validator, so it was
  avoided by design).
