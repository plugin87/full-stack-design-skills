# Web Accessibility (a11y) - Light-Weight Test Cases

A set of 6 practical scenarios to validate the skill works well for real-world accessibility challenges.

## Test Case 1: Alt Text Strategy

**Prompt:**
```
I have a dashboard with 5 types of images:
1. Decorative header image (no meaning, just aesthetic)
2. Product photo (shows what product looks like)
3. Chart/graph (shows quarterly sales trend)
4. Icon next to button (button says "Download")
5. Infographic (contains 10+ pieces of info)

How should I write alt text for each? Which images might need special handling?
```

**Expected Output:**
- Identifies decorative image: `alt=""` (empty, intentional)
- Product photo: descriptive alt (`alt="Red coffee mug, 12oz, ceramic"`)
- Chart: either long description or linked transcript (alt text can't convey all data)
- Icon next to button: omit alt, button text serves both (`<button><img alt="" src="...">Download</button>`)
- Infographic: reference to full description elsewhere or very detailed alt
- Recommends testing with screen reader
- References WCAG 1.1 (text alternatives)

**Criteria:** Skill applies alt text guidelines from SKILL.md, recognizes edge cases, provides specific alt text examples.

---

## Test Case 2: Keyboard Navigation Trap

**Prompt:**
```
I built a modal dialog that opens when clicking a button. 
My QA tester reported: "When I press Tab repeatedly, I can Tab behind the modal and interact with the background page. 
The modal doesn't trap focus."

What's wrong and how do I fix it?
```

**Expected Output:**
- Identifies issue: focus not trapped within modal
- Explains: Tab on last button should cycle to first button in modal
- Provides JavaScript solution (focus trap pattern)
- Also mentions: move focus into modal on open, move back to trigger on close
- References keyboard navigation fundamentals
- Suggests testing: keyboard-only tab through modal

**Criteria:** Skill applies focus management knowledge, references tab order principles, provides actionable code pattern.

---

## Test Case 3: Form Accessibility Issue

**Prompt:**
```
My form has:
- Input field with only placeholder text ("Email address")
- Radio buttons grouped with no fieldset/legend
- Required field marked with red asterisk only

A user with a screen reader said they can't tell what the field is for or if it's required.
What should I fix?
```

**Expected Output:**
- Issue 1: Placeholder text disappears when typing; use `<label for="id">`
- Issue 2: Radio buttons need `<fieldset>` + `<legend>` to announce grouping
- Issue 3: Red asterisk alone isn't enough; add aria-required or text label like "*Required"
- Provides corrected HTML structure
- References form accessibility fundamentals (SKILL.md section on forms)
- Mentions WCAG 3.3.2 (labels or instructions)

**Criteria:** Skill identifies multiple a11y issues, references HTML semantics, provides complete corrected code.

---

## Test Case 4: Color-Only Differentiation

**Prompt:**
```
My design uses color to show status:
- Green text = success
- Red text = error
- Gray text = pending

A colorblind user reported they can't tell the difference. 
What's the problem and how do I fix it?
```

**Expected Output:**
- Identifies issue: color alone doesn't work for colorblind users (15% of males)
- Recommends: add icons, text labels, or patterns alongside color
- Provides fixes: add ✓ icon for success, ✗ for error, "—" for pending
- Or: use text labels ("Success", "Error", "Pending")
- Or: add pattern (checkered, striped) to distinguish
- References WCAG 1.4.1 (use of color)
- Suggests testing with Coblis (colorblind simulator)

**Criteria:** Skill applies accessibility principles beyond technical specs, addresses user impact, suggests practical solutions.

---

## Test Case 5: ARIA Misuse

**Prompt:**
```
I have a custom toggle button (looks like a button, styled with CSS). 
I added:
<div class="custom-button" role="button" aria-pressed="false">
  Dark Mode
</div>

Users reported they can't activate it with keyboard. 
What's wrong with my ARIA implementation?
```

**Expected Output:**
- Identifies: ARIA describes, doesn't make interactive
- Problem: `role="button"` + `aria-pressed` without keyboard support = broken
- Solution: Either use `<button>` (semantic, auto-keyboard support) or add:
  - `tabindex="0"` (focusable)
  - Keyboard handler (Enter/Space to toggle)
  - Visual focus indicator
- Recommends: just use `<button>` (simpler, more robust)
- References ARIA golden rules from SKILL.md: "ARIA doesn't change DOM"
- Explains: ARIA = metadata only; must pair with JS + keyboard support

**Criteria:** Skill identifies ARIA misuse, explains the principle, provides correct implementation.

---

## Test Case 6: Screen Reader Testing

**Prompt:**
```
My modal dialog looks good visually. 
But a screen reader user said: "I can't understand the page structure. 
The heading is missing, I don't know what buttons do, and I can't escape."

What should I check/fix?
```

**Expected Output:**
- Issue 1: Missing heading in modal (add `<h2>` or `aria-labelledby`)
- Issue 2: Buttons have no labels or unclear labels (add descriptive text or aria-label)
- Issue 3: No way to close (add Escape key handler, plus Close button)
- Provides corrected modal structure with heading, labeled buttons
- Recommends testing: open NVDA/VoiceOver, navigate through modal, can you understand content?
- References modal accessibility pattern from references
- Suggests: announce modal to AT with `aria-modal="true"`

**Criteria:** Skill applies multiple a11y principles, provides complete corrected structure, suggests testing methodology.

---

## Success Criteria

All test cases should result in:
- ✅ Practical, actionable advice
- ✅ References to WCAG standards or SKILL.md sections
- ✅ Specific code examples or patterns
- ✅ Testing recommendations (NVDA, VoiceOver, keyboard, Coblis)
- ✅ Explanation of user impact (not just "do this because rules")
- ✅ Recognition of edge cases and exceptions

---

## Evaluation Notes

This skill should:
- **Never** prescribe ARIA without explaining why (not a checklist tool)
- **Always** prioritize semantic HTML over ARIA
- **Always** consider keyboard users and screen reader users separately
- **Always** reference testing methods (automated + manual)
- **Always** provide specific, implementable solutions

Test success = User can take the advice and apply it immediately without needing clarification or additional research.
