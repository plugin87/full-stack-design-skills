# Keyboard Navigation Guide

Essential keyboard interactions for accessible web experiences.

## Universal Keyboard Shortcuts

| Key | Action | Context |
|-----|--------|---------|
| Tab | Move to next focusable element | Everywhere (links, buttons, inputs, custom controls) |
| Shift+Tab | Move to previous focusable element | Everywhere |
| Enter | Activate button or submit form | Buttons, form submit |
| Space | Activate button or toggle | Buttons, checkboxes, radio buttons |
| Escape | Close modal/menu/dropdown | Modals, context menus |
| ← / → | Navigate (left/right) | Tabs, carousels, menus, sliders |
| ↑ / ↓ | Navigate (up/down) | Dropdown, tabs, lists, combobox, sliders |
| Home | Jump to first | Menu, list, combobox |
| End | Jump to last | Menu, list, combobox |
| F1 | Help | Anywhere (if implemented) |

---

## Component-Specific Keyboard Interactions

### Buttons
| Key | Behavior |
|-----|----------|
| Enter | Activate |
| Space | Activate |

**Note:** Never add `onclick` to non-button elements. Use `<button>`.

### Links
| Key | Behavior |
|-----|----------|
| Enter | Navigate to href |
| Shift+Enter | Navigate in new tab (browser default) |

**Note:** Never use `<div onclick="">` for navigation. Use `<a href="">`.

### Form Inputs
| Type | Keyboard Behavior |
|------|-------------------|
| Text input | Type text, Tab to next |
| Checkbox | Space to toggle |
| Radio button | ↑/↓ or ←/→ to switch, Space to select |
| Dropdown select | ↑/↓ to navigate options, Enter to select |
| Textarea | Type text, Tab to next (Shift+Tab for previous in some browsers) |

### Tabs

| Key | Behavior |
|-----|----------|
| ← / → | Switch to previous/next tab |
| Home | Switch to first tab |
| End | Switch to last tab |

**Implementation:**
```js
tablist.addEventListener('keydown', (e) => {
  let target;
  
  if (e.key === 'ArrowLeft') {
    e.preventDefault();
    target = currentTab.previousElementSibling || lastTab;
  } else if (e.key === 'ArrowRight') {
    e.preventDefault();
    target = currentTab.nextElementSibling || firstTab;
  } else if (e.key === 'Home') {
    e.preventDefault();
    target = firstTab;
  } else if (e.key === 'End') {
    e.preventDefault();
    target = lastTab;
  }
  
  if (target) selectTab(target);
});
```

### Menu / Dropdown List

| Key | Behavior |
|-----|----------|
| ↑ / ↓ | Navigate items |
| Home / End | First/last item |
| Enter | Select item |
| Escape | Close menu |

### Accordion / Disclosure

| Key | Behavior |
|-----|----------|
| Enter / Space | Toggle expanded state |
| (If arrow keys applicable) ↑ / ↓ | Navigate between headers |

### Combobox (Autocomplete)

| Key | Behavior |
|-----|----------|
| Type | Filter suggestions |
| ↑ / ↓ | Navigate suggestions |
| Enter | Select suggestion |
| Escape | Close suggestions |

### Slider

| Key | Behavior |
|-----|----------|
| ← / → | Decrease/increase value |
| ↑ / ↓ | Increase/decrease value (alternative) |
| Home | Minimum value |
| End | Maximum value |
| Page Up / Page Down | Large increment/decrement (10% range) |

**Implementation:**
```js
slider.addEventListener('keydown', (e) => {
  const step = 1;
  const max = 100;
  let value = parseInt(slider.value);
  
  if (e.key === 'ArrowLeft' || e.key === 'ArrowDown') {
    value = Math.max(value - step, 0);
    e.preventDefault();
  } else if (e.key === 'ArrowRight' || e.key === 'ArrowUp') {
    value = Math.min(value + step, max);
    e.preventDefault();
  } else if (e.key === 'Home') {
    value = 0;
    e.preventDefault();
  } else if (e.key === 'End') {
    value = max;
    e.preventDefault();
  }
  
  slider.value = value;
  slider.dispatchEvent(new Event('input'));
});
```

---

## Focus Management

### Setting Focus Programmatically

```js
// Move focus to element
element.focus();

// Move focus to element with smooth scroll into view
element.focus({ behavior: 'smooth' });

// Focus on element's first focusable child (modal opening)
modal.querySelector('button').focus();
```

### Trapping Focus (Modal)

Prevent Tab from escaping a modal:

```js
const focusableElements = modal.querySelectorAll(
  'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
);
const firstElement = focusableElements[0];
const lastElement = focusableElements[focusableElements.length - 1];

modal.addEventListener('keydown', (e) => {
  if (e.key !== 'Tab') return;
  
  if (e.shiftKey) {
    // Shift+Tab on first → last
    if (document.activeElement === firstElement) {
      e.preventDefault();
      lastElement.focus();
    }
  } else {
    // Tab on last → first
    if (document.activeElement === lastElement) {
      e.preventDefault();
      firstElement.focus();
    }
  }
});
```

### Restoring Focus After Modal Close

```js
const triggerButton = document.querySelector('[data-modal-trigger]');

function openModal() {
  modal.hidden = false;
  modal.querySelector('button').focus(); // Move focus into modal
}

function closeModal() {
  modal.hidden = true;
  triggerButton.focus(); // Restore focus to trigger
}
```

---

## Testing Keyboard Navigation

### Manual Keyboard-Only Test

1. **Unplug your mouse** (or disable trackpad)
2. **Navigate the entire site using only Tab, Shift+Tab, Enter, Escape, Arrow keys**
3. **Verify:**
   - Can you reach every interactive element (link, button, input)?
   - Is focus visible (outline, underline, highlight)?
   - Can you activate buttons (Enter/Space)?
   - Can you navigate dropdowns (↑/↓, Enter)?
   - Are there any keyboard traps (can you Tab past every element)?
   - Can you close modals (Escape)?
   - Is tab order logical (top-to-bottom, left-to-right)?

### Automated Keyboard Testing

Use browser DevTools or testing libraries:

**Playwright:**
```js
await page.keyboard.press('Tab');
await page.keyboard.press('Tab');
await page.keyboard.press('Enter');
```

**Testing Library (Jest):**
```js
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';

test('button is keyboard accessible', async () => {
  render(<MyButton />);
  const button = screen.getByRole('button');
  
  // Simulate Tab to focus
  await userEvent.tab();
  expect(button).toHaveFocus();
  
  // Simulate Enter
  await userEvent.keyboard('{Enter}');
  expect(button).toBePressed();
});
```

---

## Common Keyboard Issues & Fixes

| Issue | Symptom | Fix |
|-------|---------|-----|
| Missing focus visible | Focus ring removed with `outline: none` | Add `:focus-visible` style; never remove |
| Incorrect tab order | Tab skips elements or goes out of order | Fix HTML source order (CSS `order` breaks tab) |
| Keyboard trap | Keyboard user can't Tab past element | Add focus trap release; avoid infinite Tab loops |
| No keyboard support | Buttons don't respond to Enter/Space | Use `<button>` element; never use `<div>` for buttons |
| Unnavigable menu | Can't use ↑/↓ in dropdown | Implement keyboard handler on menu items |
| No Escape support | Can't close modal with Escape | Add `addEventListener('keydown', (e) => { if (e.key === 'Escape') ... })` |

---

## Browser Keyboard Support Differences

Most major browsers support the above keyboard patterns. Edge cases:

| Interaction | Chrome | Firefox | Safari | Edge |
|------------|--------|---------|--------|------|
| Tab navigation | ✅ | ✅ | ✅ | ✅ |
| Enter on button | ✅ | ✅ | ✅ | ✅ |
| Space on button | ✅ | ✅ | ✅ | ✅ |
| Arrow keys (custom) | ✅ | ✅ | ✅ | ✅ |
| Shift+Tab | ✅ | ✅ | ✅ | ✅ |
| Focus visible | ✅ | ✅ | ✅ | ✅ |

**Mobile:** Touch devices don't support Tab/keyboard navigation (only during accessibility mode).

---

## Accessibility Inspector Tools

Test keyboard navigation visually:

- **Chrome DevTools → Elements → Accessibility Tree** — See what screen readers see
- **Firefox DevTools → Accessibility → Keyboard** — Simulate keyboard navigation
- **axe DevTools → Issue Details** — Check "Missing ARIA" or "No keyboard support"
