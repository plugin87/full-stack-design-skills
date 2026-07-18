# ARIA Patterns Guide

Common accessible patterns for interactive components. Use semantic HTML first; ARIA for exceptions.

## Pattern 1: Alert/Toast Notification

Non-modal message that appears without stealing focus.

```html
<div role="alert" aria-live="assertive" aria-atomic="true">
  ✓ Settings saved successfully
</div>
```

**Attributes:**
- `role="alert"` — Announces immediately (equivalent to `aria-live="assertive"`)
- `aria-live="polite"` — Waits for pause in speech before announcing
- `aria-atomic="true"` — Screen reader announces entire content, not just changes

**JavaScript trigger:**
```js
const alert = document.createElement('div');
alert.setAttribute('role', 'alert');
alert.textContent = 'Settings saved';
document.body.appendChild(alert);
// Remove after 5 seconds
setTimeout(() => alert.remove(), 5000);
```

---

## Pattern 2: Modal Dialog

Modal that traps focus and hides background content.

```html
<div role="dialog" aria-modal="true" aria-labelledby="dialog-title">
  <h2 id="dialog-title">Confirm action</h2>
  <p>Are you sure you want to delete this item?</p>
  <button id="cancel-btn">Cancel</button>
  <button id="confirm-btn">Delete</button>
</div>
```

**Attributes:**
- `role="dialog"` — Signals modal to screen readers
- `aria-modal="true"` — Indicates focus is trapped
- `aria-labelledby="id"` — Links to heading

**JavaScript behavior:**
```js
const modal = document.querySelector('[role="dialog"]');
const buttons = modal.querySelectorAll('button');
const firstButton = buttons[0];
const lastButton = buttons[buttons.length - 1];

// Move focus into modal on open
firstButton.focus();

// Trap Tab within modal
modal.addEventListener('keydown', (e) => {
  if (e.key !== 'Tab') return;
  if (e.shiftKey) {
    // Shift+Tab on first button → last button
    if (document.activeElement === firstButton) {
      e.preventDefault();
      lastButton.focus();
    }
  } else {
    // Tab on last button → first button
    if (document.activeElement === lastButton) {
      e.preventDefault();
      firstButton.focus();
    }
  }
});

// Close on Escape
modal.addEventListener('keydown', (e) => {
  if (e.key === 'Escape') closeModal();
});
```

---

## Pattern 3: Tabs

Tablist with panels, keyboard support (↑↓, Home, End to switch tabs).

```html
<div role="tablist" aria-label="Content tabs">
  <button role="tab" aria-selected="true" aria-controls="panel-1" id="tab-1">
    Tab 1
  </button>
  <button role="tab" aria-selected="false" aria-controls="panel-2" id="tab-2" tabindex="-1">
    Tab 2
  </button>
  <button role="tab" aria-selected="false" aria-controls="panel-3" id="tab-3" tabindex="-1">
    Tab 3
  </button>
</div>

<div id="panel-1" role="tabpanel" aria-labelledby="tab-1">
  Content for tab 1
</div>
<div id="panel-2" role="tabpanel" aria-labelledby="tab-2" hidden>
  Content for tab 2
</div>
<div id="panel-3" role="tabpanel" aria-labelledby="tab-3" hidden>
  Content for tab 3
</div>
```

**Keyboard behavior:**
- ←/→ or ↑/↓ — Switch between tabs
- Home/End — First/last tab
- Enter/Space — Activate (not needed; selected tab activates automatically)

**JavaScript:**
```js
const tabs = document.querySelectorAll('[role="tab"]');
const panels = document.querySelectorAll('[role="tabpanel"]');

function selectTab(index) {
  // Deselect all
  tabs.forEach(t => {
    t.setAttribute('aria-selected', 'false');
    t.tabIndex = -1;
  });
  panels.forEach(p => p.hidden = true);

  // Select active
  tabs[index].setAttribute('aria-selected', 'true');
  tabs[index].tabIndex = 0;
  tabs[index].focus();
  document.getElementById(tabs[index].getAttribute('aria-controls')).hidden = false;
}

// Click handler
tabs.forEach((tab, i) => {
  tab.addEventListener('click', () => selectTab(i));
});

// Keyboard handler
document.addEventListener('keydown', (e) => {
  if (!e.target.matches('[role="tab"]')) return;
  const current = Array.from(tabs).indexOf(e.target);
  
  let next = current;
  if (e.key === 'ArrowLeft' || e.key === 'ArrowUp') {
    e.preventDefault();
    next = current === 0 ? tabs.length - 1 : current - 1;
  } else if (e.key === 'ArrowRight' || e.key === 'ArrowDown') {
    e.preventDefault();
    next = current === tabs.length - 1 ? 0 : current + 1;
  } else if (e.key === 'Home') {
    e.preventDefault();
    next = 0;
  } else if (e.key === 'End') {
    e.preventDefault();
    next = tabs.length - 1;
  }
  
  if (next !== current) selectTab(next);
});
```

---

## Pattern 4: Accordion

Expandable sections with toggle buttons.

```html
<div class="accordion">
  <div>
    <button aria-expanded="false" aria-controls="section-1">
      Section 1
    </button>
    <div id="section-1" hidden>
      Content for section 1
    </div>
  </div>
  
  <div>
    <button aria-expanded="false" aria-controls="section-2">
      Section 2
    </button>
    <div id="section-2" hidden>
      Content for section 2
    </div>
  </div>
</div>
```

**Attributes:**
- `aria-expanded="false"` — Indicates collapsed state
- `aria-controls="id"` — Links to controlled content

**JavaScript:**
```js
const buttons = document.querySelectorAll('.accordion button');

buttons.forEach(button => {
  button.addEventListener('click', () => {
    const isExpanded = button.getAttribute('aria-expanded') === 'true';
    button.setAttribute('aria-expanded', !isExpanded);
    
    const content = document.getElementById(button.getAttribute('aria-controls'));
    content.hidden = isExpanded;
  });
});
```

---

## Pattern 5: Tooltip

Supplementary text on hover/focus, non-modal, non-focusable.

```html
<button aria-describedby="tooltip-1">
  Hover for help
</button>
<div id="tooltip-1" role="tooltip" hidden>
  This is the help text
</div>
```

**JavaScript (show on hover/focus):**
```js
const button = document.querySelector('button');
const tooltip = document.querySelector('[role="tooltip"]');

button.addEventListener('mouseenter', () => tooltip.hidden = false);
button.addEventListener('mouseleave', () => tooltip.hidden = true);
button.addEventListener('focus', () => tooltip.hidden = false);
button.addEventListener('blur', () => tooltip.hidden = true);
```

**Note:** Tooltips are difficult to make fully accessible. Consider using `<title>` attribute on SVG or visible help text instead.

---

## Pattern 6: Combobox (Autocomplete)

Input with filtered list suggestions.

```html
<label for="country">Country</label>
<input 
  id="country" 
  autocomplete="off" 
  aria-autocomplete="list"
  aria-controls="listbox"
  aria-expanded="false"
/>
<ul id="listbox" role="listbox" hidden>
  <li role="option" data-value="US">United States</li>
  <li role="option" data-value="CA">Canada</li>
  <li role="option" data-value="UK">United Kingdom</li>
</ul>
```

**Attributes:**
- `aria-autocomplete="list"` — Suggests from list (vs. "inline" or "both")
- `aria-controls="id"` — Links to suggestions container
- `aria-expanded="true/false"` — Is list visible?

**Keyboard support:**
- ↑/↓ — Navigate suggestions
- Enter — Select suggestion
- Escape — Close list

---

## Pattern 7: Menu Button

Button that opens a menu with keyboard navigation.

```html
<button aria-haspopup="menu" aria-expanded="false" aria-controls="menu">
  Options
</button>
<ul id="menu" role="menu" hidden>
  <li role="menuitem"><button>Edit</button></li>
  <li role="menuitem"><button>Delete</button></li>
  <li role="menuitem"><button>Share</button></li>
</ul>
```

**Keyboard behavior:**
- Space/Enter — Open menu
- ↑/↓ — Navigate items
- Home/End — First/last item
- Escape — Close menu

---

## Pattern 8: Disclosure (Details/Summary)

Collapsible content. Prefer semantic `<details>` over custom ARIA.

```html
<details>
  <summary>More information</summary>
  <p>Hidden content appears here when expanded</p>
</details>
```

**ARIA equivalent (if `<details>` unavailable):**
```html
<button aria-expanded="false" aria-controls="details">
  More information
</button>
<div id="details" hidden>
  <p>Hidden content appears here</p>
</div>
```

---

## Golden Rules for ARIA Patterns

1. **Use semantic HTML first** — `<button>`, `<a>`, `<label>` cover 80% of cases
2. **Don't hide from keyboard** — If it needs ARIA, it needs keyboard support
3. **Always test with screen readers** — ARIA sounds right in code but breaks in practice
4. **Pair ARIA + keyboard + visual feedback** — All three required for robust UX
5. **Avoid ARIA for cosmetics** — It's for conveying meaning, not styling

---

## Testing ARIA Patterns

- **Screen reader:** NVDA (Windows), VoiceOver (Mac), TalkBack (Android)
- **Automated check:** axe DevTools (catches missing attributes)
- **Manual test:** Keyboard-only (arrow keys, Tab, Enter, Escape) — everything should work
