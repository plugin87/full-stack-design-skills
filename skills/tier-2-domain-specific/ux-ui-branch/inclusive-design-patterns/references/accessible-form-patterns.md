# Accessible Form Patterns

Practical patterns for designing forms that work for everyone, including assistive technology users.

---

## Pattern 1: Basic Text Input with Label

### The Foundation

**HTML:**
```html
<label for="email">Email address</label>
<input type="email" id="email" name="email" required>
```

**Why this works:**
1. `<label>` semantic element (not just text)
2. `for="email"` connects label to input
3. Input `id="email"` matches label `for`
4. Screen readers announce: "Email address input"
5. Clicking label focuses input (larger tap target on mobile)
6. Keyboard navigation includes input

**CSS:**
```css
label {
  display: block;
  font-size: 14px;
  font-weight: 600;
  margin-bottom: 4px;
  color: #1e293b;
}

input {
  width: 100%;
  padding: 10px 12px;
  font-size: 16px; /* Prevents auto-zoom on iOS */
  border: 1px solid #cbd5e1;
  border-radius: 4px;
}

input:focus {
  outline: 2px solid #0052cc;
  outline-offset: 2px;
}
```

---

## Pattern 2: Wrapping Label + Input

Alternative to `for` attribute (sometimes cleaner):

**HTML:**
```html
<label>
  Email address
  <input type="email" name="email" required>
</label>
```

**Advantages:**
- No need for matching id/for
- Input automatically associated with label

**Disadvantages:**
- Clicking label may not focus input in all browsers

**Best practice:** Use `for` + `id` method (Pattern 1) for consistency.

---

## Pattern 3: Required Field Indication

### Bad: Asterisk Alone

```html
<label for="email">Email *</label>
```

**Problem:** Users unfamiliar with asterisk might not understand it means "required".

### Good: Text + Asterisk

```html
<form>
  <p><span aria-label="required">*</span> Required field</p>
  
  <label for="email">
    Email 
    <span aria-label="required">*</span>
  </label>
  <input type="email" id="email" required aria-required="true">
</form>
```

**Why this works:**
- Text label explains what `*` means
- `aria-label="required"` on asterisk (hidden from screen readers in text)
- `aria-required="true"` announces requirement to screen readers
- Both visual and programmatic indication

### Better: Using `<required>` Attribute

```html
<label for="email">Email <abbr title="required">*</abbr></label>
<input type="email" id="email" required>
```

HTML5 `required` attribute:
- Browser validates automatically
- Screen readers announce "required"
- No need for extra ARIA

---

## Pattern 4: Helper Text & Hints

### Associating Hints with Inputs

```html
<label for="password">Password</label>
<input 
  type="password" 
  id="password"
  aria-describedby="password-hint"
  required
>
<div id="password-hint">
  At least 8 characters including uppercase, lowercase, and number
</div>
```

**How it works:**
1. `aria-describedby="password-hint"` links input to hint
2. Screen reader announces hint after input label
3. Users understand requirements before entering

**CSS:**
```css
#password-hint {
  font-size: 12px;
  color: #64748b;
  margin-top: 4px;
  line-height: 1.5;
}
```

---

## Pattern 5: Error Messages

### Bad: No Error Association

```html
<input type="email" id="email">
<div>Please enter a valid email</div>
```

**Problem:** Screen reader doesn't connect message to input.

### Good: Using `aria-describedby`

```html
<label for="email">Email</label>
<input 
  type="email" 
  id="email"
  aria-describedby="email-error"
  aria-invalid="true"
  required
>
<div id="email-error" role="alert">
  ✗ Please enter a valid email (e.g., user@example.com)
</div>
```

**How it works:**
1. `aria-invalid="true"` announces error status
2. `aria-describedby="email-error"` links to error message
3. `role="alert"` announces message immediately
4. Icon + text makes error clear visually

**CSS:**
```css
input[aria-invalid="true"] {
  border-color: #dc2626;
  border-width: 2px;
}

#email-error {
  color: #dc2626;
  font-size: 14px;
  margin-top: 4px;
  display: flex;
  align-items: center;
  gap: 4px;
}
```

### Error Message Best Practices

✅ **Clear, actionable errors:**
- "Please enter a valid email (example@domain.com)"
- "Password must be 8+ characters with uppercase"

❌ **Vague errors:**
- "Invalid input"
- "Error in field"
- "Try again"

---

## Pattern 6: Checkbox Group

### Bad: No Grouping

```html
<label for="agree-terms">I agree to terms</label>
<input type="checkbox" id="agree-terms">

<label for="agree-privacy">I agree to privacy policy</label>
<input type="checkbox" id="agree-privacy">
```

**Problem:** No semantic connection between checkboxes.

### Good: Using `<fieldset>` + `<legend>`

```html
<fieldset>
  <legend>Agreements</legend>
  
  <div>
    <label for="agree-terms">
      <input type="checkbox" id="agree-terms">
      I agree to the terms of service
    </label>
  </div>
  
  <div>
    <label for="agree-privacy">
      <input type="checkbox" id="agree-privacy">
      I agree to the privacy policy
    </label>
  </div>
</fieldset>
```

**How it works:**
1. `<fieldset>` groups related inputs
2. `<legend>` labels the group
3. Screen reader announces: "Agreements, checkbox, I agree to terms"
4. Visual grouping with `<div>` wrappers

**CSS:**
```css
fieldset {
  border: 1px solid #e5e7eb;
  border-radius: 4px;
  padding: 16px;
  margin-bottom: 16px;
}

legend {
  font-size: 16px;
  font-weight: 600;
  padding: 0 4px;
}

fieldset div {
  margin-bottom: 12px;
}

fieldset label {
  display: flex;
  align-items: center;
  gap: 8px;
  cursor: pointer;
}

input[type="checkbox"] {
  width: 20px;
  height: 20px;
  cursor: pointer;
}
```

---

## Pattern 7: Radio Button Group

### Good: Using `<fieldset>` + `<legend>`

```html
<fieldset>
  <legend>Which payment method?</legend>
  
  <div>
    <label for="payment-credit">
      <input type="radio" id="payment-credit" name="payment" value="credit">
      Credit card
    </label>
  </div>
  
  <div>
    <label for="payment-paypal">
      <input type="radio" id="payment-paypal" name="payment" value="paypal">
      PayPal
    </label>
  </div>
  
  <div>
    <label for="payment-bank">
      <input type="radio" id="payment-bank" name="payment" value="bank">
      Bank transfer
    </label>
  </div>
</fieldset>
```

**Why this works:**
- `<fieldset>` groups radio buttons semantically
- `<legend>` labels the group
- All radios must have same `name` attribute (ensures only one selected)
- Each radio has unique `id` + label `for` match

---

## Pattern 8: Select Dropdowns

### Basic Accessible Select

```html
<label for="country">Country</label>
<select id="country" name="country" required>
  <option value="">Select a country...</option>
  <option value="us">United States</option>
  <option value="ca">Canada</option>
  <option value="uk">United Kingdom</option>
  <option value="au">Australia</option>
</select>
```

**Best practices:**
- Include placeholder option (empty value)
- Label above select (for clarity)
- Options should be concise
- No auto-selection (user chooses)

### Good: Grouped Options

```html
<label for="sport">Choose a sport:</label>
<select id="sport" name="sport">
  <option value="">Select a sport...</option>
  
  <optgroup label="Team Sports">
    <option value="soccer">Soccer</option>
    <option value="basketball">Basketball</option>
    <option value="baseball">Baseball</option>
  </optgroup>
  
  <optgroup label="Individual Sports">
    <option value="tennis">Tennis</option>
    <option value="golf">Golf</option>
    <option value="cycling">Cycling</option>
  </optgroup>
</select>
```

**Why `<optgroup>` works:**
- Screen readers announce group names
- Visual grouping (indentation in select)
- Easier to navigate many options

---

## Pattern 9: Multi-Line Text (Textarea)

```html
<label for="message">Message (max 500 characters)</label>
<textarea 
  id="message" 
  name="message"
  rows="4"
  cols="50"
  maxlength="500"
  aria-describedby="message-count"
  required
></textarea>
<div id="message-count" aria-live="polite">
  <span id="char-count">0</span> / 500 characters
</div>

<script>
  const textarea = document.getElementById('message');
  const charCount = document.getElementById('char-count');
  
  textarea.addEventListener('input', (e) => {
    charCount.textContent = e.target.value.length;
  });
</script>
```

**Features:**
- Label describes purpose
- `rows="4"` sets visible height
- `maxlength="500"` prevents exceeding limit
- Character counter updates live (`aria-live="polite"`)
- Screen reader announces character count

---

## Pattern 10: Form Submission & Validation

### Good: Server-Side Validation + Error Summary

```html
<form method="post">
  <!-- Error summary at top -->
  <div role="alert" id="errors" class="error-summary">
    <h2>Please fix these errors:</h2>
    <ul>
      <li><a href="#email">Email is required</a></li>
      <li><a href="#password">Password must be 8+ characters</a></li>
    </ul>
  </div>

  <!-- Form fields -->
  <label for="email">Email *</label>
  <input type="email" id="email" aria-required="true" required>
  
  <label for="password">Password *</label>
  <input type="password" id="password" aria-required="true" required minlength="8">
  
  <button type="submit">Submit</button>
</form>
```

**Why this works:**
1. Error summary at top (before form)
2. Links in error summary jump to fields
3. Each field shows inline error
4. `role="alert"` announces errors
5. Server-side validation (reliable)
6. Client-side validation (fast feedback)

### Real-Time Validation (Client-Side)

```html
<label for="email">Email</label>
<input 
  type="email" 
  id="email"
  aria-describedby="email-feedback"
>
<div id="email-feedback" aria-live="polite"></div>

<script>
  const input = document.getElementById('email');
  const feedback = document.getElementById('email-feedback');
  
  input.addEventListener('blur', (e) => {
    const value = e.target.value;
    const isValid = value.includes('@') && value.includes('.');
    
    if (value && !isValid) {
      input.setAttribute('aria-invalid', 'true');
      feedback.textContent = '✗ Enter a valid email';
      feedback.style.color = '#dc2626';
    } else {
      input.setAttribute('aria-invalid', 'false');
      feedback.textContent = '✓ Looks good';
      feedback.style.color = '#10b981';
    }
  });
</script>
```

---

## Pattern 11: Multi-Step Forms

Breaking forms into steps improves usability for everyone.

```html
<form>
  <!-- Progress indicator -->
  <div class="progress" aria-label="Form progress">
    <div class="step active">1. Personal</div>
    <div class="step">2. Address</div>
    <div class="step">3. Confirm</div>
  </div>

  <!-- Step 1 -->
  <fieldset id="step-1" aria-label="Step 1: Personal information">
    <legend>Personal Information</legend>
    
    <label for="first-name">First Name *</label>
    <input type="text" id="first-name" required>
    
    <label for="last-name">Last Name *</label>
    <input type="text" id="last-name" required>
  </fieldset>

  <button type="button" onclick="nextStep()">Next</button>
</form>
```

**Best practices:**
- Clear step indicators (visual + aria-label)
- Show current step
- Each step is a `<fieldset>`
- Clear back/next buttons
- Option to save progress

---

## Pattern 12: Accessible Form Validation

### HTML5 Attributes

```html
<input type="email" required> <!-- Required field -->
<input type="number" min="0" max="100"> <!-- Range validation -->
<input type="text" pattern="[A-Z]{2}[0-9]{3}"> <!-- Pattern match -->
<input type="password" minlength="8"> <!-- Minimum length -->
```

**Advantages:**
- Browser enforces validation
- No JavaScript needed
- Screen readers announce requirements
- Mobile keyboards adapt (e.g., numeric keyboard for numbers)

### Custom Validation Message

```html
<input 
  type="email" 
  id="email"
  required
  oninvalid="setCustomValidity('Please enter a valid email')"
  onchange="setCustomValidity('')"
>
```

---

## Form Accessibility Checklist

- [ ] Every input has associated `<label>`
- [ ] Label text is clear and descriptive
- [ ] Focus indicators visible (outline or glow)
- [ ] Form can be completed with keyboard alone
- [ ] Required fields marked (text + aria-required)
- [ ] Helper text linked with aria-describedby
- [ ] Error messages linked with aria-describedby
- [ ] Error messages include actionable advice
- [ ] Validation happens on blur (not keystroke)
- [ ] Error summary at top of form (if needed)
- [ ] Select dropdowns use `<optgroup>` if many options
- [ ] Checkboxes/radios in `<fieldset>` with `<legend>`
- [ ] Submit button text is clear ("Submit", not "OK")
- [ ] Form submission has confirmation (if important)
- [ ] Tested with keyboard navigation
- [ ] Tested with screen reader

---

## Common Form Mistakes

### ❌ Placeholder as Label

```html
<input type="email" placeholder="Email address">
```

Problems:
- Placeholder disappears when typing
- Not announced by screen readers
- Doesn't satisfy label requirement

Solution:
```html
<label for="email">Email address</label>
<input type="email" id="email" placeholder="user@example.com">
```

### ❌ Validation on Keystroke

```html
<input oninput="validateEmail(this)">
```

Problem: Users can't finish typing before error appears.

Solution: Validate on blur (when user leaves field).

### ❌ No Error Association

```html
<input type="email" id="email">
<div>Please enter a valid email</div>
```

Solution: Use aria-describedby to link error to input.

### ❌ Auto-Focus Issues

```html
<input type="text" autofocus>
```

Problem: Can disorient screen reader users.

Solution: Avoid autofocus unless critical (like login page).

### ❌ Disabled Buttons

```html
<button disabled>Submit</button>
```

Problem: Users don't know why button is disabled.

Solution: Show error message explaining why form can't be submitted.
