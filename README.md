<div align="center">

# Full-Stack Design Skills

**A 15-skill curriculum and a working component library for UX/UI design & frontend engineering — built for [Claude Code](https://claude.com/claude-code).**

From color theory and accessibility to design-to-code, tokens, and CI/CD — plus six live, in-browser examples that apply the whole system to real UI.

<br>

![Skills](https://img.shields.io/badge/skills-15-E61876?style=flat-square)
![Tiers](https://img.shields.io/badge/tiers-3-c4145f?style=flat-square)
![Examples](https://img.shields.io/badge/live_examples-6-94104B?style=flat-square)
![Accessibility](https://img.shields.io/badge/a11y-WCAG_AA%2B-1f7a4d?style=flat-square)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-38bdf8?style=flat-square&logo=tailwindcss&logoColor=white)
![Lucide](https://img.shields.io/badge/Lucide-icons-111?style=flat-square)

</div>

---

## Contents

- [Overview](#overview)
- [The 15 skills](#the-15-skills)
- [Live examples](#live-examples)
- [Quick start](#quick-start)
- [Learning paths](#learning-paths)
- [Design system](#design-system)
- [Repository structure](#repository-structure)
- [Notes & license](#notes--license)

---

## Overview

This repo is two things at once:

1. **A curriculum** — 15 [Agent Skills](https://docs.claude.com/en/docs/claude-code/skills) covering the full arc of building digital products: design fundamentals → accessibility → design systems → frontend → workflow & delivery. Each skill is a self-contained `SKILL.md` (with `references/`) that Claude Code discovers and invokes automatically when relevant.

2. **A component library** — six single-file HTML examples that *apply* the curriculum to real interfaces, verified in a real browser. One brand, one token layer, three visual systems (custom flat, Material 3, Bootstrap 5).

Everything is **token-driven**, ships **light + dark**, and is held to an explicit **anti-AI-pattern / accessibility** checklist.

---

## The 15 skills

### Tier 1 · Core Foundations

Cross-cutting knowledge applied in every phase.

| Skill | What it covers |
|---|---|
| `design-fundamentals` | Color theory, typography, layout & grid, spacing, visual hierarchy |
| `web-accessibility-a11y` | WCAG 2.1, semantic HTML, ARIA, keyboard nav, screen readers |
| `design-systems-architecture` | Tokens, component architecture, governance, scaling |
| `web-performance-optimization` | Core Web Vitals, Lighthouse, asset/network/runtime tuning |
| `anti-ai-design-patterns` | De-slop review: kill AI tells (accent rails, emoji, default palettes) so UI reads as human-crafted |

### Tier 2 · Domain-Specific

<table>
<tr><th>UX / UI branch</th><th>Frontend branch</th></tr>
<tr valign="top"><td>

| Skill |
|---|
| `figma-expert-workflows` |
| `responsive-universal-design` |
| `inclusive-design-patterns` |

</td><td>

| Skill |
|---|
| `frontend-framework-guide` |
| `css-styling-pixel-perfect` |

</td></tr>
</table>

### Tier 3 · Workflow Integration

| Skill | What it covers |
|---|---|
| `design-to-code-workflow` | Single source of truth, spec docs, Figma → React handoff |
| `component-library-mastery` | Storybook, npm publishing, versioning |
| `design-tokens-system` | Token architecture, transformation, multi-platform distribution |
| `qa-testing-visual-regression` | Visual regression, a11y testing, cross-browser, CI |
| `deployment-devops-workflow` | GitHub Actions, release, deploy strategies, rollback |

---

## Live examples

Six self-contained pages in [`skills/examples/`](skills/examples/) — open any in a browser (Tailwind, Lucide and Bootstrap load from CDN; the brand font is bundled locally). Each was driven and console-checked in a real browser.

| Page | Style | Highlights |
|---|---|---|
| [`design-system.html`](skills/examples/design-system.html) | **Documentation** | Living reference: color ramp, gradient, type scale, radius, elevation, tokens table, a11y checklist |
| [`component-showcase.html`](skills/examples/component-showcase.html) | Flat / custom | Buttons, forms, feedback, badges, table, accessible modal, skeleton + a live de-slop self-audit |
| [`dashboard.html`](skills/examples/dashboard.html) | Flat / custom | KPI sparklines, an interactive area chart (hover crosshair), CWV, adoption bars, deploy feed |
| [`registration-form.html`](skills/examples/registration-form.html) | Flat / custom | Inline validation, password-strength meter, accessible errors, gradient hero, success state |
| [`material-design.html`](skills/examples/material-design.html) | **Material 3** | Tonal roles, Roboto, Material Symbols, elevation, pill buttons, FAB, filled/outlined fields |
| [`bootstrap.html`](skills/examples/bootstrap.html) | **Bootstrap 5.3** | Real Bootstrap retinted via `--bs-*` tokens, Bootstrap Icons, native `data-bs-theme` dark |

> All three visual systems share **one brand seed** (`#E61876`) so you can compare flat vs. Material vs. Bootstrap side by side.

---

## Quick start

### Use the skills in Claude Code

Skills are auto-discovered from a `.claude/skills/` directory. To make them available:

```bash
# clone
git clone git@github.com:plugin87/full-stack-design-skills.git
cd full-stack-design-skills

# install globally (all projects)
cp -R .claude/skills/*/ ~/.claude/skills/

# — or — per-project (this repo already ships them in .claude/skills/)
```

Once installed, a skill fires when your request matches its `description`, or invoke one directly by name (e.g. `/design-fundamentals`). Ask an accessibility question and `web-accessibility-a11y` surfaces; ask to "de-slop this UI" and `anti-ai-design-patterns` steps in.

### Open the examples

```bash
cd skills/examples
python3 -m http.server 8000   # then visit http://localhost:8000/
```

---

## Learning paths

| Track | Skills | Route |
|---|---|---|
| **Designer** | 11 | Foundations → UX/UI branch → design-to-code · tokens · QA |
| **Developer** | 12 | Foundations → Frontend branch → all of Tier 3 |
| **Full-stack** | 15 | Everything, Tier 1 → 2 → 3 |

---

## Design system

The examples run on one small, deliberate system:

| Aspect | Choice |
|---|---|
| **Brand / CI** | `#E61876` magenta (with a `#E61876 → #94104B` gradient for hero surfaces) |
| **Typography** | Graphik Thai — weights 400 / 500·600 / 700 (Latin + Thai) |
| **Icons** | Lucide (one family, no emoji in product UI) |
| **Theming** | CSS-variable token layer; light + dark flip values only |
| **Shape** | Radius scale `4 · 6 · 10 · 14 · 20`; elevation kept to flat + one lift |
| **Principles** | WCAG AA+ · visible focus · status = icon **and** text · `prefers-reduced-motion` |

Full documentation lives in [`design-system.html`](skills/examples/design-system.html); the brand palette and eight gradient options are explored there.

---

## Repository structure

```
.
├── .claude/skills/              # 15 skills, flat — auto-discovered by Claude Code
├── skills/                      # source curriculum + docs
│   ├── manifest.json            #   skill registry & learning paths
│   ├── tier-1-core-foundations/
│   ├── tier-2-domain-specific/
│   ├── tier-3-workflow-integration/
│   └── examples/                # 6 live HTML examples + fonts/
└── master-skills-architecture.zip
```

---

## Notes & license

- **Fonts** — `skills/examples/fonts/GraphikThai-*.woff2` are a **commercial typeface** included for local demos only. Do not redistribute; swap in your own licensed font for any published use.
- **Skill content** — curriculum text, references, and examples are yours to adapt. Add a `LICENSE` file to state your terms before sharing publicly.

<div align="center">
<br>
<sub>Built with Tailwind CSS · Lucide · Graphik Thai — for Claude Code.</sub>
</div>
