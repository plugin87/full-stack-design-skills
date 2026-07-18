# De-Slop Review Checklist

Run this pass over any generated UI before shipping. For each flagged item, either state the decision it encodes or remove it.

## Borders & shape
- [ ] No single-side accent rail on rounded cards (T-01). Colored borders encode state/category/selection only.
- [ ] Radius follows a scale, not one value everywhere (T-09).
- [ ] Elevation is systematic (0/1/2); not every surface has shadow/blur (T-10).

## Icons & glyphs
- [ ] Zero emoji / emoticons in UI and copy (T-02, T-03). Exception: emoji-native surfaces only.
- [ ] Exactly one icon library across the whole product (T-15).
- [ ] Decorative icons `aria-hidden`; icon-only controls have `aria-label`.
- [ ] Icon size on a scale; stroke weight matches adjacent type.

## Color
- [ ] No raw default framework swatches (`indigo-500`, `#808080`) (T-05).
- [ ] Neutrals biased toward the accent hue; no pure hueless grey.
- [ ] One accent carries the boldness; semantic colors are a separate system.
- [ ] No default purple→blue gradient hero (T-04); no rainbow/gradient text (T-12).

## Typography
- [ ] Two+ deliberate typefaces, not Inter/Space-Grotesk-by-reflex (T-07).
- [ ] A committed type scale; headings `text-wrap: balance`; body ~65ch.

## Layout
- [ ] Not everything centered (T-08); real grid with left-aligned body.
- [ ] Intentional asymmetry / focal point; not eerily uniform (T-14).
- [ ] Spacing has rhythm (tight in groups, generous between), not uniform gaps.
- [ ] Sequence numbers/dividers only on real sequences (T-13).

## Content
- [ ] No lorem ipsum; real, specific copy (T-11).
- [ ] Plausible, varied data — no "John Doe / $1,234.56".
- [ ] Copy from the user's side, active voice, no hedged AI marketing prose.

## Theme & polish
- [ ] Both light and dark themes designed (not a naive invert).
- [ ] Visible keyboard focus; `prefers-reduced-motion` respected.
- [ ] No horizontal body scroll; wide content scrolls in its own container.

## Over-correction guard
- [ ] Fixes reduced decoration, not added it. A clean flat page with one accent passes.
- [ ] No emoji-tell swapped for an over-designed icon-and-gradient tell.

---
**Scoring:** any unchecked box is a candidate AI tell. Resolve by encoding a real decision or removing the element.
