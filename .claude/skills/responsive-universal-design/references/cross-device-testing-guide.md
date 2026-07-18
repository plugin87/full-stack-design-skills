# Cross-Device Testing Guide

How to test responsive design systematically across devices, breakpoints, and conditions.

---

## Step 1: Lab Testing (Figma + Browser DevTools)

### Figma Testing

Before code, test in Figma at different breakpoints.

**Setup:**
1. Create responsive frames at: 375px (mobile), 768px (tablet), 1440px (desktop)
2. Design components at each breakpoint
3. Test interactions in prototype
4. Take screenshots for reference

**Checklist:**
- [ ] Mobile layout (375px): stacked, readable
- [ ] Tablet layout (768px): 2-column or appropriate
- [ ] Desktop layout (1440px): full multi-column
- [ ] Spacing consistent across breakpoints
- [ ] Typography readable at all sizes
- [ ] Images scale proportionally
- [ ] Interactions work at all sizes

### Browser DevTools Testing

Chrome, Firefox, Safari all have DevTools.

**Chrome DevTools Steps:**
1. Open DevTools (F12 or Cmd+Opt+I)
2. Click device toolbar (Cmd+Shift+M)
3. Select device from dropdown or set custom width
4. Test at multiple breakpoints (375, 640, 768, 1024, 1440)
5. Check console for errors

**Test checklist:**
- [ ] Layout reflows correctly
- [ ] Images load
- [ ] Text is readable (not too small)
- [ ] Touch targets are 48px+ (on mobile emulation)
- [ ] No console errors
- [ ] No horizontal scroll
- [ ] Navigation works

### Network Throttling

Test performance under slow conditions.

**Chrome DevTools:**
1. Network tab
2. Dropdown (default: "No throttling")
3. Select "Fast 3G" or "Slow 3G"
4. Reload page
5. Observe load times, LCP, images loading

**Common throttle profiles:**
- **No throttling:** Local/home network (fast)
- **Fast 3G:** Good mobile network (moderate)
- **Slow 3G:** Poor mobile network (slow)
- **Offline:** Test offline handling

**Targets to hit:**
- LCP (Largest Contentful Paint): < 2.5s on 4G
- Images start loading immediately
- Text renders without blocking

---

## Step 2: Real Device Testing

Lab testing isn't perfect. Real devices have variables that emulation can't capture.

### Device Borrowing Strategy

**What devices to test:**
1. **Must-test:**
   - iPhone SE (smallest modern phone, 375px)
   - iPhone 14 (standard phone, 390px)
   - iPad (tablet, 768px landscape)
   - MacBook/Desktop (1440px+)

2. **Nice-to-test:**
   - Older Android phone (Samsung Galaxy S10, 411px)
   - iPad Pro (large tablet, 1024px)
   - Large desktop (1920px+)
   - 5G-connected device (performance testing)

**Where to borrow:**
- Company device lab
- Team members' personal devices
- Mobile testing services (BrowserStack, Sauce Labs)
- Local device lending library

### Real Device Testing Checklist

**On each device:**
- [ ] Website loads without errors
- [ ] Layout reflows correctly
- [ ] Text is readable
- [ ] Touch targets are tappable (not too small)
- [ ] Images load and display correctly
- [ ] Scroll is smooth (no jank)
- [ ] Keyboard interactions work (if applicable)
- [ ] Dark mode works (if implemented)
- [ ] Performance is acceptable (< 3s load)

**Specific device notes:**

**iPhone SE (375px):**
- Smallest screen
- Single-column layout should work
- Small text may be hard to read
- Notch at top (use safe areas)

**iPad (768px landscape):**
- Medium screen
- 2-column layout good
- Touch-friendly
- Landscape mode common

**Android (411px+):**
- Slightly wider than iPhone
- Taller aspect ratio
- Different fonts (Android has own system fonts)
- Gesture navigation different

**Desktop Large (1920px+):**
- Very wide screens
- Content may be too narrow
- Use max-width constraints
- Extra whitespace is OK

---

## Step 3: Testing Tools

### Browser-Based Testing

**Chrome DevTools (Free):**
- Device emulation
- Network throttling
- Performance metrics
- Accessibility audit
- CSS debugging

**Firefox Developer Tools (Free):**
- Similar to Chrome
- Good for responsive design testing
- Useful for accessibility checks

**Safari Web Inspector (Free, Mac only):**
- Test on real Safari rendering
- iOS-specific testing
- Recommended if your users are on Mac/iOS

### Cloud Testing Services

**BrowserStack (Paid):**
- Test on 3000+ real devices
- Automated and manual testing
- Live testing on real phones
- Integration with CI/CD

**Sauce Labs (Paid):**
- Real device testing
- Automated testing framework
- Cross-browser testing
- Mobile testing

**LambdaTest (Paid):**
- Real device cloud
- Live testing
- Screenshot testing
- Network throttling

### Responsive Testing Tools

**Responsively (Free):**
- Open source responsive testing tool
- Test multiple breakpoints simultaneously
- Device templates
- Screenshot testing

**Screenfly (Free):**
- Responsive design testing
- Multiple breakpoints at once
- Online tool

---

## Step 4: Automated Testing

For ongoing quality assurance.

### Visual Regression Testing

Catch unintended layout changes.

**Chromatic (Free tier):**
- Automatically snapshots components
- Detects visual changes
- Works with Storybook
- Runs in CI/CD

**Percy (Paid/Free tier):**
- Visual testing for responsive
- Screenshots at multiple breakpoints
- Detects visual regression
- CI/CD integration

### Automated Accessibility Testing

Catch a11y issues early.

**WAVE (Free browser plugin):**
- Quick accessibility audit
- Identifies missing alt text, contrast issues
- Browser extension

**Axe DevTools (Free browser plugin):**
- Detailed accessibility report
- WCAG compliance checking
- Easy to run

**Pa11y (Free CLI tool):**
- Automated a11y testing
- CI/CD integration
- HTML, JS, and CSS testing

```bash
# Install pa11y
npm install -g pa11y

# Run accessibility test
pa11y https://yourwebsite.com
```

---

## Step 5: Performance Testing

Measure real performance across devices.

### Lighthouse

Built into Chrome DevTools.

**How to run:**
1. Open DevTools (F12)
2. Click "Lighthouse" tab
3. Select "Desktop" or "Mobile"
4. Click "Analyze page load"
5. Review results

**Key metrics:**
- Performance score (0-100)
- LCP (Largest Contentful Paint)
- INP (Interaction to Next Paint)
- CLS (Cumulative Layout Shift)

**Target scores:**
- Performance: 90+
- LCP: < 2.5s
- INP: < 200ms
- CLS: < 0.1

### WebPageTest

Detailed performance analysis.

**How to use:**
1. Go to webpagetest.org
2. Enter URL
3. Select location and device
4. Run test
5. Review waterfall, filmstrip, metrics

**Useful for:**
- Real network conditions (3G, 4G)
- Real device simulation
- Detailed waterfall analysis

---

## Step 6: Breakpoint-Specific Testing

Test each breakpoint thoroughly.

### Mobile (320px - 479px) Testing

**Layout:**
- [ ] Single column layout
- [ ] Full-width content (with padding)
- [ ] No horizontal scroll
- [ ] Text readable

**Navigation:**
- [ ] Hamburger menu works
- [ ] Menu opens/closes
- [ ] Tap outside closes menu

**Forms:**
- [ ] Input fields 48px+ height
- [ ] Labels visible
- [ ] Error messages visible
- [ ] Keyboard doesn't cover content

**Images:**
- [ ] Load (even on slow network)
- [ ] Display correctly
- [ ] No distortion
- [ ] Aspect ratio maintained

**Touch:**
- [ ] All interactive elements 48px+
- [ ] No accidental taps
- [ ] Sufficient spacing between buttons

### Tablet (640px - 1023px) Testing

**Layout:**
- [ ] 2-column layout (if applicable)
- [ ] Sidebar visible (if applicable)
- [ ] Max width reasonable (usually 768px)

**Keyboard:**
- [ ] Tab navigation works
- [ ] Focus visible
- [ ] No keyboard traps

**Orientation:**
- [ ] Works in portrait mode
- [ ] Works in landscape mode
- [ ] Reflows on rotation

**Touch:**
- [ ] Touch targets 44px+ (smaller than mobile)
- [ ] Pinch-to-zoom works
- [ ] Double-tap to zoom works

### Desktop (1024px+) Testing

**Layout:**
- [ ] Multi-column layout
- [ ] Max-width constraint (usually 1200-1440px)
- [ ] Centered content
- [ ] Balanced whitespace

**Mouse:**
- [ ] Hover states work
- [ ] Cursor changes appropriately
- [ ] Links are obvious

**Keyboard:**
- [ ] Tab navigation complete
- [ ] Focus visible
- [ ] Shortcuts work (if implemented)

**Large screens (1600px+):**
- [ ] Content doesn't stretch too wide
- [ ] Sidebar/columns sized reasonably
- [ ] Still readable (not overwhelming)

---

## Step 7: Dark Mode Testing

If you support dark mode.

**Test at each breakpoint:**
- [ ] Colors correct in dark mode
- [ ] Text readable (sufficient contrast)
- [ ] Images visible (not too dark)
- [ ] Icons distinguishable
- [ ] Form fields visible

**Implementation:**
```css
/* Light mode (default) */
body {
  background: white;
  color: #1e293b;
}

/* Dark mode */
@media (prefers-color-scheme: dark) {
  body {
    background: #1e293b;
    color: #f1f5f9;
  }
}
```

**Testing:**
- Chrome DevTools → Rendering → Emulate CSS media feature prefers-color-scheme
- Select "dark" → Site should update colors

---

## Step 8: Accessibility Testing at All Breakpoints

Accessibility isn't just for keyboard users—it applies to all screen sizes.

### Keyboard Navigation

Test at each breakpoint:
- [ ] Tab through all interactive elements
- [ ] Focus is visible (outline, highlight, etc.)
- [ ] Logical tab order (left-to-right, top-to-bottom)
- [ ] No keyboard traps (can always tab out)
- [ ] Skip links work (skip to main content)

### Screen Reader Testing

Use a screen reader at different sizes:
- [ ] Content hierarchy makes sense (headings)
- [ ] Images have alt text
- [ ] Form labels associated with inputs
- [ ] Buttons have labels
- [ ] Lists marked up correctly

**Free screen readers:**
- NVDA (Windows): nvaccess.org
- VoiceOver (Mac): Built-in, Cmd+F5
- JAWS (paid, Windows)

### Color Contrast

At all breakpoints:
- [ ] Text on background: 4.5:1 (WCAG AA)
- [ ] Large text: 3:1 (WCAG AA)
- [ ] Disabled text: no contrast requirement
- [ ] Test with Contrast Checker tool

**Tools:**
- WAVE browser extension
- Contrast Checker (contrastchecker.com)
- Chrome DevTools Accessibility audit

---

## Step 9: Testing Checklist Template

**Website:** _________________  
**Date:** _________________  
**Tester:** _________________

### Mobile (375px)
- [ ] Layout single-column
- [ ] Text readable (16px minimum)
- [ ] Touch targets 48px+
- [ ] No horizontal scroll
- [ ] Navigation works
- [ ] Images load
- [ ] No console errors
- [ ] LCP < 2.5s (on 4G)

### Tablet (768px)
- [ ] Layout 2-column (if applicable)
- [ ] Portrait and landscape work
- [ ] Touch targets 44px+
- [ ] Performance good
- [ ] Keyboard navigation works

### Desktop (1440px)
- [ ] Multi-column layout works
- [ ] Max-width constraint applied
- [ ] Centered content
- [ ] Hover states work
- [ ] LCP < 2.5s

### Cross-Device
- [ ] Dark mode works (all breakpoints)
- [ ] Accessibility passes (a11y audit)
- [ ] Keyboard navigation works (all breakpoints)
- [ ] Performance acceptable (all breakpoints)
- [ ] Images load (all breakpoints)

### Issues Found
1. _______________
2. _______________
3. _______________

---

## Common Responsive Testing Mistakes

### ❌ Only Testing on Desktop

Missing mobile issues that only appear on real devices.

Right: Test on actual phone, tablet, and desktop.

### ❌ Not Testing Network Throttling

Apps feel fast on office WiFi, slow on 3G.

Right: Test on "Fast 3G" and "Slow 3G" in DevTools.

### ❌ Emulation Only

Chrome emulation is good but not perfect (rendering, performance, fonts differ).

Right: Test on real devices regularly.

### ❌ Not Testing Rotation

Portrait → Landscape reflow often breaks.

Right: Test rotation on tablets.

### ❌ Forgetting Dark Mode

Works fine in light mode, broken in dark mode.

Right: Test both modes at all breakpoints.

---

## Testing Workflow Summary

1. **Design in Figma** at 3 breakpoints (375, 768, 1440)
2. **Code** and test locally (DevTools, Lighthouse)
3. **Lab test** at main breakpoints (DevTools emulation)
4. **Real device test** on 3-4 key devices
5. **Automated testing** (Lighthouse CI, Percy, accessibility)
6. **Monitor performance** (Real User Monitoring)
7. **Fix issues** discovered in testing

Regular testing prevents regressions and keeps responsive design working.
