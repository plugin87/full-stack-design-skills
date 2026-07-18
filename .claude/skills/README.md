# Full-Stack Design — Claude Code Skills

15 auto-discovered Agent Skills for UX/UI design + frontend development.
Each folder is a self-contained skill: `SKILL.md` (name + description frontmatter)
plus an optional `references/` directory for progressive-disclosure detail.

Claude Code loads every skill in this directory automatically. Invoke one by
name (`/design-fundamentals`) or let its `description` trigger it.

## Tier grouping (metadata only — structure is intentionally flat)

**Tier 1 — Core Foundations** (cross-cutting, applied in every phase)
- `design-fundamentals`
- `web-accessibility-a11y`
- `design-systems-architecture`
- `web-performance-optimization`
- `anti-ai-design-patterns` — de-slop review: no accent-rail borders, no emoji (icon library only), chosen palettes over defaults

**Tier 2 — Domain-Specific**
- UX/UI: `figma-expert-workflows`, `responsive-universal-design`, `inclusive-design-patterns`
- Frontend: `frontend-framework-guide`, `css-styling-pixel-perfect`

**Tier 3 — Workflow Integration**
- `design-to-code-workflow`
- `component-library-mastery`
- `design-tokens-system`
- `qa-testing-visual-regression`
- `deployment-devops-workflow`

## Learning paths
- **Designer:** Tier 1 → UX/UI branch → design-to-code, design-tokens, qa-testing
- **Developer:** Tier 1 → frontend branch → all of Tier 3
- **Full-Stack:** all 14, Tier 1 → 2 → 3

## Install globally (optional)
To make these available in every project, copy the folders into `~/.claude/skills/`:

```bash
cp -R .claude/skills/*/ ~/.claude/skills/
```
