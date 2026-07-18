# AI Tells Catalog — Before / After

A reference catalog of specific AI-generated design signatures and their intentional replacements. Each entry: the tell, why it reads as machine-made, and the fix.

---

## T-01 · Single-side accent border (the "rail card")

**Tell**
```css
.card { border: 1px solid #e5e7eb; border-left: 4px solid #6366f1; border-radius: 12px; }
```
**Why** The accent is on every card, encodes nothing, and clips against the radius.
**Fix** Remove it, OR promote to a full state-token set (bg + text + border), OR make it a deliberate square-top edge accent. A colored border must encode state/category/selection.

---

## T-02 · Emoji as icons / bullets / section markers

**Tell** `<h2>🎨 Design</h2>`, `<li>✅ …</li>`, `🚀 Deploy`
**Why** OS-dependent rendering, no color inheritance, breaks stroke weight, weak a11y.
**Fix** One icon library (Lucide/Heroicons/Phosphor). `aria-hidden` when decorative. See `icon-library-guide.md`.

---

## T-03 · Emoticons in product copy

**Tell** "Nice work! 😄", "Oops! 😅 Something went wrong"
**Why** Reads as chatbot filler; unprofessional and inconsistent.
**Fix** Specific, active copy. Errors explain what happened and how to fix it — no emoji, no apology theatre.

---

## T-04 · Purple→blue gradient hero

**Tell** `linear-gradient(135deg, #a855f7, #3b82f6)` on a white hero.
**Why** The single most overused AI landing-page look.
**Fix** A chosen palette; if you want a gradient, derive it from your accent with a real reason (depth, brand), and keep it subtle.

---

## T-05 · Raw default framework swatches

**Tell** `indigo-500`, `blue-600`, `#808080` used verbatim.
**Why** Inherited, not chosen — no ownership of the palette.
**Fix** Shift hue/lightness; define brand tokens; bias neutrals toward the accent.

---

## T-06 · Cream + serif + terracotta cliché

**Tell** `#F4F1EA` background, a serif display face, a terracotta accent.
**Why** A specific, now-recognizable "tasteful AI" template.
**Fix** Only use if the subject genuinely calls for it; otherwise choose a direction specific to the content.

---

## T-07 · Inter / Space Grotesk as reflexive default

**Tell** Inter at one weight, no scale, "because it's safe."
**Why** The default face signals no typographic decision was made.
**Fix** Deliberate pairing + a type scale. Inter is fine *if chosen for a reason*.

---

## T-08 · Everything centered

**Tell** `text-align:center` on entire sections, stacked centered blocks.
**Why** Avoids layout decisions; no reading rhythm.
**Fix** Real grid, left-aligned body text, center only for deliberate emphasis.

---

## T-09 · One radius everywhere

**Tell** `rounded-lg` on cards, buttons, inputs, avatars, images.
**Why** No shape system.
**Fix** A radius scale mapped to element size (e.g. 6/10/16/full).

---

## T-10 · Glassmorphism / shadow on everything

**Tell** `backdrop-blur` + heavy `box-shadow` on every surface.
**Why** Elevation with no hierarchy — everything floats.
**Fix** An elevation system (0/1/2). Most surfaces are flat; elevation marks the few that matter.

---

## T-11 · Lorem ipsum & fake-looking data

**Tell** "Lorem ipsum dolor…", "John Doe", "$1,234.56", "Company Inc."
**Why** Placeholder content signals unfinished / generated.
**Fix** Real, specific, domain-true copy and plausible varied data.

---

## T-12 · Gradient / rainbow text

**Tell** `background-clip:text` rainbow or purple→pink headings.
**Why** Decorative flourish with no meaning; strong AI signal.
**Fix** Solid ink; spend emphasis on weight, size, and space instead.

---

## T-13 · Decorative sequence numbers on non-sequences

**Tell** `01 / 02 / 03` eyebrows on cards that are not an ordered process.
**Why** Structure decorates instead of encoding truth.
**Fix** Number only real sequences (a process, a ranked list, a timeline).

---

## T-14 · Uncanny uniformity ("AI-alien")

**Tell** Identical card heights, identical gaps, perfect symmetry, zero exceptions.
**Why** Machine output lacks the intentional irregularity of human design.
**Fix** Deliberate asymmetry and rhythm — let one element break the grid with purpose.

---

## T-15 · Mixed icon families

**Tell** Lucide + Font Awesome + Heroicons in the same view.
**Why** Differing strokes/metaphors read as incoherent.
**Fix** One family, product-wide.
