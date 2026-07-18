# Design Fundamentals - Light-Weight Test Cases

A set of 6 practical scenarios to validate the skill works well for real-world design challenges.

## Test Case 1: Typography Scale Review

**Prompt:**
```
Review this typography scale and identify issues:
- Body text: 16px, weight 400, line-height 1.4
- Small heading: 20px, weight 500, line-height 1.3
- Large heading: 32px, weight 600, line-height 1.2
- Caption: 11px, weight 400, line-height 1.5

Is this appropriate for a SaaS dashboard? What should I adjust?
```

**Expected Output:**
- Identifies caption size too small (11px < 12px minimum)
- Notes that line-height for body is too tight (1.4 < recommended 1.5–1.7)
- Suggests ratios or specific numbers
- Explains reasoning (readability, WCAG, scanning)
- Provides a corrected scale

**Criteria:** Skill references typography fundamentals, line-height guidelines, and accessibility concerns from the guide.

---

## Test Case 2: Color Contrast Issue

**Prompt:**
```
Our design uses #D4A574 (light tan) text on #E8DCC8 (pale cream) background for helper text. 
Is this accessible? A user said they have trouble reading it. What's the contrast ratio and how do we fix it?
```

**Expected Output:**
- Calculates or references contrast ratio (likely ~2:1, fails WCAG AA)
- Identifies issue as insufficient contrast
- Suggests specific fixes: darken text, lighten background, or both
- Recommends testing with WebAIM or similar tool
- Explains WCAG AA vs AAA standards for helper text

**Criteria:** Skill applies color contrast knowledge; references accessibility standards; provides actionable solutions.

---

## Test Case 3: Layout Spacing Inconsistency

**Prompt:**
```
I'm building a component library and noticed spacing is all over the place:
- Button padding: 12px
- Card padding: 20px
- Form field padding: 16px
- Gap between form fields: 18px

How do I standardize this? Should I use an 8px grid?
```

**Expected Output:**
- Validates the 8px grid approach
- Suggests consistent spacing scale (8, 16, 24, 32, etc.)
- Maps existing values to nearest grid size
- Provides CSS approach (custom properties)
- Explains rationale (consistency, responsive scaling, maintenance)

**Criteria:** Skill applies spacing system knowledge; recommends practical standards; explains implementation strategy.

---

## Test Case 4: Mobile Typography at Scale

**Prompt:**
```
Our desktop heading is 32px, body is 16px. On mobile (360px width), text looks tiny and hard to read. 
Do I scale everything down proportionally, or use a different scale for mobile?
```

**Expected Output:**
- Advises against proportional shrinking (breaks readability)
- Recommends maintaining or slightly reducing base size (16px → 15px max)
- Suggests keeping hierarchy ratio (headings still 2× body minimum)
- References viewport width and readable line lengths
- Provides breakpoint strategy (e.g., mobile 15px, tablet 16px, desktop 17px)

**Criteria:** Skill references responsive design, typography hierarchy, and mobile-first thinking.

---

## Test Case 5: Visual Hierarchy in a Dashboard

**Prompt:**
```
My dashboard has 15 metrics displayed in equal-sized cards. Users can't tell which are most important. 
How do I create visual hierarchy without cluttering the design?
```

**Expected Output:**
- Suggests size differentiation (3 tiers: large/medium/small)
- Recommends color intensity (primary metric saturated, secondary muted)
- Proposes layout strategy (featured metric at top, others in grid)
- Suggests supporting visuals (icons, position, whitespace)
- Avoids decoration; keeps visual weight meaningful

**Criteria:** Skill applies hierarchy principles; references contrast, position, size, color, whitespace tools.

---

## Test Case 6: Dark Mode Color Ramp

**Prompt:**
```
Our light mode uses #F0F0F0 for backgrounds and #1A1A1A for text.
For dark mode, we're thinking #1A1A1A for backgrounds and #F0F0F0 for text.
Will this look right? Any issues with the colors we should consider?
```

**Expected Output:**
- Confirms the inverted approach is correct (high contrast maintained)
- Notes contrast ratio remains strong (15:1+)
- Suggests testing actual outputs (colors may appear different on dark screens)
- Recommends checking for "halo" effect on light text on dark backgrounds
- Advises on mid-tone colors (buttons, borders, secondary text) in dark mode
- References that not all colors need full inversion (e.g., accent colors may shift hue slightly)

**Criteria:** Skill applies dark mode strategy knowledge; references color theory and practical implementation concerns.

---

## Success Criteria

All test cases should result in:
- ✅ Practical, actionable advice
- ✅ References to specific guidelines from the skill
- ✅ No tool-specific jargon (framework/tool-agnostic)
- ✅ Explanation of "why" not just "what"
- ✅ WCAG/accessibility considerations where relevant
- ✅ Responses under 300 words (concise, focused)

---

## Evaluation Notes

This skill should:
- **Never** dive into Figma-specific workflows
- **Never** discuss implementation (CSS, React, etc.)
- **Always** reference the universal principles in the guide
- **Always** provide actionable next steps
- **Always** consider accessibility and WCAG compliance

Test success = User can take the advice and apply it directly without needing clarification.
