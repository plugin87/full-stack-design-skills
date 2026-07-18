# Motion, Animation & Multilingual Design Patterns

Accessible patterns for animations, motion, and designing for multiple languages.

---

## Pattern 1: Respecting `prefers-reduced-motion`

### Why This Matters

Some users experience:
- Motion sickness (vestibular disorders)
- Seizures (epilepsy triggered by flashing/motion)
- Migraines (motion triggers migraines)
- Sensory overload (autism spectrum)

Always respect the OS setting for reduced motion.

### Basic Pattern

```css
/* Animated by default */
.card {
  transition: transform 300ms ease;
}

.card:hover {
  transform: translateY(-8px);
}

/* Respect user preference */
@media (prefers-reduced-motion: reduce) {
  .card {
    transition: none; /* Remove animation */
  }

  .card:hover {
    transform: none; /* Remove transform */
  }
}
```

### Testing Reduced Motion

**On Mac:**
```
System Preferences → Accessibility → Display → 
"Reduce motion" → Enable
```

**On Windows 10/11:**
```
Settings → Ease of Access → Display → 
Show animations → Off
```

**In Chrome DevTools:**
```
DevTools → Rendering → Emulate CSS media feature prefers-reduced-motion
```

### Pattern 2: Fade Instead of Motion

**Animations to keep (safe):**
- Fade in/out (opacity)
- Color changes
- Brightness/blur
- Static position changes (no movement)

**Animations to remove:**
- Large movement/translation
- Parallax effects
- Spinning/rotation
- Jittery motion

```css
/* Animation that's OK to keep (fade) */
.overlay {
  opacity: 0;
  transition: opacity 300ms ease;
}

.overlay.visible {
  opacity: 1;
}

/* Still respects prefers-reduced-motion if transition disabled */
@media (prefers-reduced-motion: reduce) {
  .overlay {
    transition: none;
  }
}
```

---

## Pattern 3: Safe Animation Guidelines

### Flash Rate (Seizure Safety)

**Critical:** No more than 3 flashes per second.

```css
/* ❌ Dangerous: Flashing 60 times per second */
@keyframes dangerous-flash {
  0% { opacity: 1; }
  50% { opacity: 0; }
  100% { opacity: 1; }
}

/* Safe version (1 flash per second) */
@keyframes safe-flash {
  0%, 95% { opacity: 1; }
  100% { opacity: 0; }
}

animation: safe-flash 1s infinite;
```

### Large Parallax (Vestibular Safety)

**Avoid large parallax effects.** Small parallax is OK.

```css
/* ❌ Bad: Large, disorienting parallax */
.background {
  transform: translateY(calc(var(--scroll) * 0.5px));
}

/* ✅ OK: Subtle parallax (if needed) */
.background {
  transform: translateY(calc(var(--scroll) * 0.1px));
}

/* ✅ Better: No parallax */
.background {
  transform: none;
}
```

### Animation Duration

- Minimum: 200ms (too fast feels jarring)
- Comfortable: 300-500ms
- Maximum: 1000ms+ (feels sluggish)

```css
/* ❌ Too fast */
transition: all 100ms ease;

/* ✅ Comfortable */
transition: all 300ms ease;

/* ✅ Good for slow motion */
transition: all 500ms ease;
```

---

## Pattern 4: Auto-Playing Content

**Never auto-play audio or video.**

### Bad: Auto-Playing

```html
<!-- ❌ Plays immediately, annoying -->
<video autoplay>
  <source src="video.mp4" type="video/mp4">
</video>

<audio autoplay>
  <source src="audio.mp3" type="audio/mpeg">
</audio>
```

**Problems:**
- Users can't anticipate sound (startling)
- Battery drains quickly
- Bandwidth consumed
- Accessibility: no warning for Deaf users

### Good: User Controls

```html
<!-- ✅ User controls playback -->
<video controls>
  <source src="video.mp4" type="video/mp4">
</video>

<!-- ✅ Muted auto-play acceptable (rare cases) -->
<video autoplay muted playsinline>
  <source src="video.mp4" type="video/mp4">
</video>
```

### Captions for Video

Always include captions for Deaf/hard of hearing users.

```html
<video controls>
  <source src="video.mp4" type="video/mp4">
  <track kind="captions" src="captions.vtt" srclang="en" label="English">
</video>
```

**VTT format (captions.vtt):**
```
WEBVTT

00:00:00.000 --> 00:00:03.000
Welcome to our video

00:00:03.500 --> 00:00:07.000
Today we'll discuss accessibility
```

---

## Pattern 5: Focus Indicators on Animated Elements

Focus indicators should work even with animations.

```css
/* Default state: animated */
.button {
  transition: background 300ms ease;
  background: #2563eb;
}

.button:hover {
  background: #1d4ed8;
}

/* Focus: visible regardless of animation */
.button:focus {
  outline: 3px solid #0052cc;
  outline-offset: 2px;
}

/* Reduced motion: disable animations but keep focus */
@media (prefers-reduced-motion: reduce) {
  .button {
    transition: none;
  }

  /* Focus still visible! */
  .button:focus {
    outline: 3px solid #0052cc;
    outline-offset: 2px;
  }
}
```

---

## Pattern 6: Loading States with Motion

Show loading without disorienting animation.

```html
<!-- ✅ Safe: Static spinner with dots -->
<div class="loader">
  <span>.</span><span>.</span><span>.</span>
</div>

<style>
  .loader span {
    opacity: 0.3;
    animation: blink 1.5s infinite;
  }

  .loader span:nth-child(2) {
    animation-delay: 0.2s;
  }

  .loader span:nth-child(3) {
    animation-delay: 0.4s;
  }

  @keyframes blink {
    0%, 100% { opacity: 0.3; }
    50% { opacity: 1; }
  }

  /* Respect reduced motion */
  @media (prefers-reduced-motion: reduce) {
    .loader span {
      animation: none;
      opacity: 1;
    }
  }
</style>
```

---

## Pattern 7: Multilingual Text Layout

### Handling Text Direction

**Left-to-right (LTR):** English, French, Spanish, etc.
**Right-to-left (RTL):** Arabic, Hebrew, Persian, etc.

```html
<!-- ✅ Good: Language declared -->
<html lang="en">
  <body>English content here</body>
</html>

<!-- ✅ Good: RTL language -->
<html lang="ar" dir="rtl">
  <body>محتوى عربي</body>
</html>

<!-- ✅ Good: Mixed languages -->
<html lang="en">
  <p>English text</p>
  <p lang="ar" dir="rtl">نص عربي</p>
</html>
```

### CSS for Multilingual

```css
/* Automatically adjust text alignment based on direction */
p {
  text-align: start; /* Left on LTR, right on RTL */
  margin-inline-start: 16px; /* Left on LTR, right on RTL */
}

.sidebar {
  margin-inline-end: 24px; /* Right on LTR, left on RTL */
}
```

**CSS logical properties:**
- `margin-inline-start` = left (LTR) or right (RTL)
- `margin-inline-end` = right (LTR) or left (RTL)
- `padding-inline-start/end` = same for padding
- `inset-inline-start/end` = same for positioning

---

## Pattern 8: Font Selection for Multiple Languages

Different languages need different fonts.

```css
/* Latin scripts (English, French, Spanish) */
@font-face {
  font-family: "System";
  src: local("San Francisco"), local("Segoe UI"), local("Roboto");
}

/* Arabic script */
@font-face {
  font-family: "ArabicFont";
  src: local("Arial"), local("Droid Arabic Kufi");
  unicode-range: U+0600-U+06FF; /* Arabic block */
}

/* Thai script */
@font-face {
  font-family: "ThaiFont";
  src: local("Tahoma"), local("Cordia New");
  unicode-range: U+0E00-U+0E7F; /* Thai block */
}

/* Apply fonts based on language */
html {
  font-family: "System";
}

html[lang="ar"] {
  font-family: "ArabicFont";
}

html[lang="th"] {
  font-family: "ThaiFont";
}
```

---

## Pattern 9: Text Length in Different Languages

**Text length varies dramatically between languages:**

- English: "Hello" = 5 characters
- German: "Guten Tag" = 9 characters
- Polish: "Cześć" = 5 characters, but "Wiadomość" = 9
- Arabic: "مرحبا" = 5 characters, but longer when written
- Japanese: "こんにちは" = 5 characters (compact)

### Design Implications

```
English: "Submit"
German: "Einreichen"
Russian: "Отправить"

Button width must accommodate longest language!
```

**Best practices:**
- Don't hardcode button widths
- Use `max-width` instead
- Test with longest language variant
- Avoid abbreviations that break in other languages

```css
/* ❌ Bad: Fixed width */
button {
  width: 100px;
  overflow: hidden;
}

/* ✅ Good: Flexible width */
button {
  padding: 12px 24px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
```

---

## Pattern 10: Text with Numbers/Dates

Numbers and dates format differently across locales.

```javascript
// English (US): 1,234.56
const enUS = new Intl.NumberFormat('en-US').format(1234.56);
// Result: "1,234.56"

// German: 1.234,56
const deDE = new Intl.NumberFormat('de-DE').format(1234.56);
// Result: "1.234,56"

// French: 1 234,56
const frFR = new Intl.NumberFormat('fr-FR').format(1234.56);
// Result: "1 234,56"

// Arabic: ١٬٢٣٤٫٥٦ (different numerals!)
const arEG = new Intl.NumberFormat('ar-EG').format(1234.56);
// Result: "١٬٢٣٤٫٥٦"
```

**Dates:**
```javascript
// English (US): 3/15/2024
const enUS = new Intl.DateTimeFormat('en-US').format(new Date());

// German: 15.3.2024
const deDE = new Intl.DateTimeFormat('de-DE').format(new Date());

// Japanese: 2024年3月15日
const jaJP = new Intl.DateTimeFormat('ja-JP').format(new Date());
```

---

## Pattern 11: Avoid Directional Terminology

Some terms assume left-to-right.

```
❌ Bad:
- "Click the arrow on the right"
- "The menu is on the left"
- "Swipe right to proceed"

✅ Good:
- "Click next"
- "The menu is in the navigation"
- "Continue to the next step"

✅ Better:
- Use directional terms only when necessary
- Use "start" and "end" (adapts to LTR/RTL)
```

---

## Pattern 12: Locale-Specific Content

Some content changes by locale.

```html
<form>
  <!-- English: Phone numbers are 10 digits -->
  <label for="phone" lang="en">Phone number</label>
  <input type="tel" pattern="[0-9]{10}" lang="en">

  <!-- German: Phone numbers vary -->
  <label for="phone-de" lang="de">Telefonnummer</label>
  <input type="tel" lang="de">

  <!-- Japanese: Address format is different -->
  <label for="address-ja" lang="ja">住所</label>
  <input type="text" lang="ja">
</form>
```

---

## Pattern 13: Animation in Multilingual Contexts

Text animations must work in all languages.

```css
/* ❌ Bad: Animation assumes single-line English text */
@keyframes slide-in {
  from {
    transform: translateX(-100px);
    opacity: 0;
  }
  to {
    transform: translateX(0);
    opacity: 1;
  }
}

/* ✅ Good: Fade-in works regardless of text length */
@keyframes fade-in {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

.heading {
  animation: fade-in 300ms ease;
}

@media (prefers-reduced-motion: reduce) {
  .heading {
    animation: none;
  }
}
```

---

## Multilingual & Motion Checklist

- [ ] HTML lang attribute set correctly
- [ ] RTL languages (Arabic, Hebrew) have dir="rtl"
- [ ] Text alignment uses `text-align: start` (not left/right)
- [ ] Margins/padding use logical properties
- [ ] Fonts support all languages used
- [ ] Numbers/dates formatted per locale
- [ ] No truncation of text (buttons accommodate longest language)
- [ ] No directional language ("click right" → "click next")
- [ ] Animations respect prefers-reduced-motion
- [ ] No flashing (> 3 flashes/sec)
- [ ] Large parallax removed or minimal
- [ ] Auto-play disabled (no sound without user control)
- [ ] Videos have captions
- [ ] Loading states don't disorient
- [ ] Translation strings account for text expansion

---

## Common Multilingual Mistakes

### ❌ English-Only Assumptions

```html
<!-- Assumes single-line English text -->
<button style="width: 120px;">Submit</button>
```

German "Einreichen" (10 chars) > English "Submit" (6 chars).

Solution: Use flexible widths, test with longer languages.

### ❌ Hardcoded Text Alignment

```css
.heading {
  text-align: right; /* Only works for RTL accidentally */
}
```

Solution: Use `text-align: start` (adapts to lang).

### ❌ Animation Ignores Motion Preference

```css
.card {
  animation: spin 1s infinite; /* Always plays */
}
```

Solution: Wrap with `@media (prefers-reduced-motion: reduce)`.

### ❌ No Font for Other Scripts

```css
body {
  font-family: "Arial"; /* Doesn't support all Unicode */
}
```

Solution: Use system fonts or load fallback fonts per language.
