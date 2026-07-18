---
name: web-performance-optimization
description: Optimize web applications for speed, responsiveness, and efficiency. Use this skill for Core Web Vitals analysis, Lighthouse interpretation, asset optimization strategies, network performance tuning, runtime performance improvements, monitoring setup, and performance budgets. Triggers include: page speed optimization, Lighthouse audit review, Core Web Vitals improvement, image/CSS/JS optimization, bundle size reduction, first-paint delays, layout shifts, interaction delays, performance monitoring questions, and performance budgets. Essential for anyone responsible for user experience, loading speed, or application responsiveness.
---

# Web Performance Optimization

A practical guide to measuring, diagnosing, and improving web application performance.

Performance is a user experience feature. A fast site converts better, keeps users engaged, and ranks higher in search. This skill covers the metrics that matter, tools that measure them, and concrete optimization strategies.

## Core Web Vitals: The Three Metrics That Matter

Google's Core Web Vitals are the three quantitative measures of user experience. They directly impact search rankings and user satisfaction.

### 1. Largest Contentful Paint (LCP)

**What it measures:** How fast the largest visible element (text, image, video) appears on screen.

**Target:** < 2.5 seconds

**Why it matters:** Users perceive "fast" as "appears quickly." If the main content takes 4+ seconds, users bounce.

**What counts as LCP:**
- `<img>` elements
- `<image>` inside `<svg>`
- Backgrounds with `background-image`
- Text blocks (paragraphs, headings)
- Videos (poster frame)
- Canvas elements

**What doesn't count:**
- Off-screen images
- Transparent images
- Text in canvas

**Example LCP Timeline:**
```
0s    — Page begins to load
0.8s  — Hero image starts loading
1.2s  — Hero image finishes (LCP = 1.2s)  ✓ Good (< 2.5s)
2.1s  — Page fully interactive
```

**Optimization strategies:**
- Preload critical images (`<link rel="preload">`)
- Compress images aggressively (WebP format, lazy loading for below-the-fold)
- Use a CDN for fast global delivery
- Avoid render-blocking CSS/JavaScript
- Use server-side rendering for critical content

### 2. First Input Delay (FID) → Interaction to Next Paint (INP)

**What it measures:** Delay between user input (click, tap, keystroke) and browser response.

**Target:** < 100ms (FID) or < 200ms (INP)

**Why it matters:** Slow response feels like the app is frozen. Users think something broke and abandon the interaction.

**FID vs. INP:**
- **FID** (First Input Delay): Only measures the *first* user interaction. Being fast once doesn't mean consistently fast.
- **INP** (Interaction to Next Paint): Measures all interactions throughout the page lifetime. More realistic; now recommended over FID.

**Example Input Timeline:**
```
User clicks button at: 0ms
Browser processes JavaScript: 0-80ms
Browser repaints: 80-85ms
Paint completes: 85ms
→ INP = 85ms ✓ Good (< 200ms)
```

**Optimization strategies:**
- Break long JavaScript into shorter tasks (< 50ms each)
- Defer non-critical code (code splitting, lazy loading)
- Use Web Workers for heavy computation
- Avoid layout thrashing (read DOM, then write; don't alternate)
- Use `requestAnimationFrame` for animations

### 3. Cumulative Layout Shift (CLS)

**What it measures:** Unexpected layout shifts that happen after the page loads. Every time an element moves without user interaction, CLS increases.

**Target:** < 0.1

**Why it matters:** Layout shifts are annoying. Ads suddenly pushing content down, buttons moving mid-click—users hate it.

**Common culprits:**
- Images without declared dimensions
- Ads inserted dynamically
- Embeds (video players, maps)
- Web fonts causing text reflow

**Example Layout Shift:**
```
User is reading article at position 200px.
Ad loads below: shifts layout down 80px.
User's eyes jump; reading is disrupted.
CLS += 0.15 (poor)

Worse: User clicks where they thought a button was,
but button has shifted. Click lands on wrong element.
```

**Optimization strategies:**
- Reserve space for images/ads before they load (set explicit width/height)
- Load critical fonts synchronously; non-critical asynchronously
- Avoid inserting DOM elements above the fold after load
- Use CSS `transform: translate()` instead of changing top/left (doesn't cause reflow)
- Test with slow networks to catch layout shifts that appear only on slow connections

---

## Lighthouse: The Diagnostic Tool

Lighthouse is an automated audit tool that measures performance, accessibility, best practices, and SEO. It runs in Chrome DevTools, as a CLI, or via web (PageSpeed Insights).

### Understanding Lighthouse Scores

Lighthouse scores are weighted averages of metrics:

**Performance score: 0-100**
- LCP: 25% weight
- INP: 30% weight
- CLS: 25% weight
- First Contentful Paint (FCP): 10%
- Time to First Byte (TTFB): 10%

**Score ranges:**
- 90-100: Excellent ✅
- 50-89: Needs work ⚠️
- 0-49: Poor ❌

### Interpreting Lighthouse Reports

A Lighthouse report shows:

**Metrics** (top section)
- Quantitative measurements (LCP 2.1s, INP 85ms, CLS 0.05)
- Visual representation of how you're doing vs. targets

**Opportunities** (middle section)
- Ranked by potential impact (high impact fixes first)
- Examples: "Eliminate render-blocking resources," "Defer offscreen images"
- Estimated savings (e.g., "Saves 2.5s")

**Diagnostics** (bottom section)
- Additional info: unused CSS, accessibility issues, security warnings
- Not scored, but useful for investigation

### Common Lighthouse Opportunities

**Eliminate render-blocking resources** (high impact)
- Problem: CSS/JS files loaded in `<head>` block page rendering
- Fix: Inline critical CSS, defer non-critical JS, use `async`/`defer` attributes

**Defer offscreen images** (high impact)
- Problem: All images load immediately, even those below the fold
- Fix: Use lazy loading (`loading="lazy"` attribute or Intersection Observer)

**Unsized images cause layout shift** (high impact)
- Problem: Images load without width/height, causing reflow
- Fix: Always declare `width` and `height` attributes or CSS aspect-ratio

**Reduce CSS** (medium impact)
- Problem: Large unused CSS files
- Fix: Remove dead code, split by route, use CSS-in-JS with automatic splitting

**Reduce JavaScript** (medium impact)
- Problem: Large bundle size
- Fix: Code splitting, tree-shaking, lazy loading routes

---

## Asset Optimization

### Images: Biggest Bang for Buck

Images typically account for 50-80% of page weight. Optimizing images has the highest ROI.

**Image optimization checklist:**
- ✅ Use modern formats (WebP with JPEG fallback)
- ✅ Compress aggressively (quality 75-85 is often imperceptible)
- ✅ Serve responsive sizes (`srcset`, `sizes` attributes)
- ✅ Lazy load below-the-fold images (`loading="lazy"`)
- ✅ Declare dimensions (prevent layout shift)
- ✅ Use a CDN with image optimization (Cloudflare, Akamai, Fastly)

**Example: Optimized Image Tag**
```html
<!-- Before: 2MB, unoptimized -->
<img src="photo.jpg" alt="..." />

<!-- After: 150KB, optimized -->
<picture>
  <source type="image/webp" srcset="photo-400w.webp 400w, photo-800w.webp 800w" sizes="(max-width: 600px) 100vw, 50vw" />
  <source type="image/jpeg" srcset="photo-400w.jpg 400w, photo-800w.jpg 800w" sizes="(max-width: 600px) 100vw, 50vw" />
  <img src="photo-800w.jpg" alt="..." width="800" height="600" loading="lazy" />
</picture>
```

**Result:** 95% size reduction, faster LCP, no layout shift.

### Fonts: Load Strategically

Web fonts are often invisible bottleneck. Default behavior blocks rendering.

**Font loading strategies:**

**Strategy 1: System Fonts Only (Fastest)**
```css
font-family: system-ui, -apple-system, sans-serif;
```
- No download; instant rendering
- Sacrifice brand consistency for speed

**Strategy 2: Critical Font (Synchronous)**
```html
<link rel="preload" href="font.woff2" as="font" type="font/woff2" crossorigin />
```
- Loads early; unblocks rendering after download
- Good for display fonts (headings)

**Strategy 3: Non-Critical Font (Asynchronous)**
```html
<link rel="preload" href="font.woff2" as="font" type="font/woff2" crossorigin media="(prefers-reduced-data: no-preference)" />
```
- Loads in background; doesn't block
- Good for body text

**Strategy 4: Variable Font (Modern)**
```css
@font-face {
  font-family: 'Recursive';
  src: url('recursive.var.woff2') format('woff2-variations');
  font-weight: 100 900;
}
```
- Single file for all weights/styles (400 normal + 700 bold = 1 file instead of 2)
- Huge savings for large font families

### CSS & JavaScript: Load Only What You Need

**CSS optimization:**
- Remove unused CSS (PurgeCSS, Tailwind)
- Split by route (critical styles inline, route styles async)
- Compress and minify
- Use CSS-in-JS with automatic code splitting (styled-components, emotion)

**JavaScript optimization:**
- Code splitting by route (lazy load pages, modals, etc.)
- Tree-shaking (remove dead code)
- Minification and compression
- Consider moving to Edge runtime (Cloudflare Workers, Vercel Edge) for faster execution

---

## Network Optimization

### Caching Strategy

**Browser cache** (HTML `Cache-Control` header)
- Set long expiry for static assets (1 year for versioned files)
- Set short expiry for HTML (1-5 minutes or no-cache with ETag)

```
# Versioned assets (never change)
Cache-Control: max-age=31536000, immutable

# HTML (check for updates frequently)
Cache-Control: max-age=300, must-revalidate
```

**CDN cache**
- Edge caching for global delivery
- Geographically distributed servers reduce latency
- Example services: Cloudflare, Fastly, Akamai

**Service Worker cache**
- Offline support
- Predictable performance (don't wait for network)
- Good for PWAs

### Compression

**Gzip/Brotli compression** (most important)
- Compress text (HTML, CSS, JS) at 70-90% reduction
- Enable on server (nearly zero CPU cost for modern servers)
- Most browsers support it automatically

```
# Enable Brotli (better than gzip)
Content-Encoding: br
```

### Lazy Loading

**Intersection Observer API** (modern, recommended)
```javascript
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      const img = entry.target;
      img.src = img.dataset.src;
      observer.unobserve(img);
    }
  });
});

document.querySelectorAll('img[data-src]').forEach(img => {
  observer.observe(img);
});
```

**Native lazy loading** (simplest, browser support ≈95%)
```html
<img src="..." loading="lazy" />
```

---

## Runtime Performance

### Rendering Performance

**Frame budget:** 60fps = 16ms per frame

- Rendering (layout, paint): ≤ 10ms
- JavaScript: ≤ 3-5ms (rest is browser work)

**Avoid jank (stuttering):**
- Don't block main thread (use Web Workers for heavy computation)
- Batch DOM reads/writes (all reads first, then all writes)
- Use `requestAnimationFrame` for animations
- Use CSS transforms instead of top/left (GPU-accelerated)

**Example: Layout Thrashing (BAD)**
```javascript
// Bad: read-write-read-write alternates
for (let i = 0; i < 10; i++) {
  element.style.width = element.offsetWidth + 10 + 'px'; // read then write
}

// Good: batch reads, then batch writes
const widths = [];
for (let i = 0; i < 10; i++) {
  widths.push(element.offsetWidth); // all reads first
}
widths.forEach((w, i) => {
  elements[i].style.width = (w + 10) + 'px'; // then all writes
});
```

### Memory Leaks

Common causes:
- Detached DOM nodes still referenced in JavaScript
- Event listeners not removed
- Timers (setInterval) not cleared
- Circular references in objects

**Detection:**
- Chrome DevTools Memory profiler
- Heap snapshots to find retained objects
- Performance monitor for memory growth over time

---

## Monitoring & Observability

### Real User Monitoring (RUM)

Synthetic (lab) testing like Lighthouse is good, but doesn't capture real-world conditions. Real User Monitoring measures actual user experiences.

**Key RUM metrics:**
- Core Web Vitals (LCP, INP, CLS)
- Time to First Byte (TTFB)
- Page load time
- Error rates
- User interactions (clicks, scrolls, form submissions)

**Popular RUM services:**
- Google Analytics (free, built-in Web Vitals)
- Datadog (premium)
- New Relic (premium)
- Custom implementation using `PerformanceObserver`

**Custom RUM implementation:**
```javascript
// Measure Core Web Vitals
new PerformanceObserver((list) => {
  for (const entry of list.getEntries()) {
    console.log('LCP:', entry.renderTime || entry.loadTime);
  }
}).observe({ entryTypes: ['largest-contentful-paint'] });

new PerformanceObserver((list) => {
  for (const entry of list.getEntries()) {
    console.log('CLS:', entry.value);
  }
}).observe({ type: 'layout-shift', buffered: true });
```

### Performance Budgets

A **performance budget** is a maximum limit on page metrics. It prevents performance regression.

**Example budget:**
- JavaScript bundle: ≤ 150KB (gzipped)
- CSS: ≤ 50KB (gzipped)
- Images: ≤ 500KB
- LCP: ≤ 2.5s
- INP: ≤ 200ms
- CLS: ≤ 0.1

**How to enforce:**
- CI/CD checks (fail build if budget exceeded)
- Lighthouse CI (automated Lighthouse runs on every PR)
- Bundle size monitoring (bundlesize, size-limit)

**Example CI check:**
```yaml
# GitHub Actions
- name: Lighthouse CI
  uses: treosh/lighthouse-ci-action@v10
  with:
    uploadArtifacts: true
    temporaryPublicStorage: true
    budgetPath: ./lighthouse-budget.json
```

---

## Common Performance Issues & Fixes

### Slow LCP

**Problem:** Page takes 4+ seconds to show main content.

**Diagnosis:**
- Largest element is a hero image? Problem is image loading
- Largest element is text? Problem is font loading or render-blocking CSS

**Fixes:**
- Hero images: Preload, compress, use CDN
- Fonts: Use system fonts initially, load custom font asynchronously
- Render-blocking CSS: Move to `<head>`, inline critical styles, defer rest

### High INP

**Problem:** Page feels unresponsive (clicks take 500ms+ to register).

**Diagnosis:** DevTools Performance profiler will show long JavaScript execution times.

**Fixes:**
- Break JavaScript into shorter tasks (use `scheduler.yield()` in modern browsers)
- Defer non-critical work (debounce, lazy load)
- Use Web Workers for heavy computation

### High CLS

**Problem:** Page layout shifts unexpectedly.

**Diagnosis:** Lighthouse report identifies specific culprits (images, ads, fonts).

**Fixes:**
- Declare image dimensions
- Reserve space for ads/embeds before load
- Use `font-display: swap` to avoid font swap causing layout shift

---

## Performance Testing Workflow

**Step 1: Establish baseline**
- Run Lighthouse audit (lab test)
- Set up RUM (real-world monitoring)
- Define performance budget

**Step 2: Identify bottlenecks**
- Lighthouse Opportunities section (ranked by impact)
- Performance profiler for runtime issues
- Network tab for slow assets

**Step 3: Prioritize & fix**
- High-impact fixes first (e.g., image optimization)
- Test in isolation (fix one thing at a time)
- Re-measure to confirm improvement

**Step 4: Monitor & maintain**
- Set performance budget in CI/CD
- Monitor RUM dashboard weekly
- Alert on regressions

---

## When to Use This Skill

✅ **Do use for:**
- Core Web Vitals optimization and interpretation
- Lighthouse audit review and prioritization
- Asset optimization strategy (images, fonts, CSS, JS)
- Network and caching optimization
- Runtime performance improvements
- Performance monitoring setup
- Performance budget definition and enforcement
- Performance testing methodology

❌ **Don't use for:**
- Specific tool setup (Google Analytics, Datadog config — see tool-specific skills)
- Framework-specific optimization (React.memo, Vue computed properties — see framework skills)
- Server optimization (Node.js, database tuning — see backend skills)
- SEO optimization (though performance helps SEO)
