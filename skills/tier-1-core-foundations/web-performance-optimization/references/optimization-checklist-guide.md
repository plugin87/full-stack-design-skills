# Performance Optimization Checklist & Quick Reference

Practical checklist for auditing and optimizing web application performance.

---

## Pre-Audit Setup

Before optimizing, establish a baseline:

- [ ] Run Lighthouse audit (DevTools → Lighthouse tab)
- [ ] Record baseline metrics (LCP, INP, CLS, overall score)
- [ ] Take screenshot of Opportunities section (prioritized by impact)
- [ ] Note your performance budget (if you have one)
- [ ] Test on different devices/networks if possible

---

## High-Impact Optimizations (Do These First)

These optimizations typically yield the biggest improvements per hour spent.

### 1. Image Optimization (LCP Impact: HIGH)

- [ ] Identify LCP element (Lighthouse shows which element)
- [ ] If LCP is an image:
  - [ ] Compress image (target: 50-80% size reduction)
  - [ ] Convert to WebP format with JPEG fallback
  - [ ] Declare `width` and `height` attributes (prevent layout shift)
  - [ ] Add `loading="lazy"` to below-the-fold images
- [ ] For all images:
  - [ ] Use `srcset` and `sizes` for responsive images
  - [ ] Lazy load images not in initial viewport
  - [ ] Consider using a CDN with image optimization

**Estimated impact:** 20-40% LCP improvement

**Tools:** ImageOptim, mozjpeg, cwebp, Cloudinary, Imgix

---

### 2. Reduce JavaScript Bundle (INP Impact: HIGH)

- [ ] Check bundle size (DevTools → Coverage tab)
- [ ] Identify unused code:
  - [ ] Run Coverage tool (Chrome: Cmd+Shift+P → Coverage)
  - [ ] Note red (unused) sections
- [ ] Implement code splitting:
  - [ ] Split by route (lazy load pages)
  - [ ] Split by feature (modals, settings, admin)
  - [ ] Defer non-critical bundles (use `dynamic import()`)
- [ ] Tree-shake unused code:
  - [ ] Ensure build tool uses `sideEffects: false` in package.json
  - [ ] Remove dead code manually if needed
- [ ] Minify and compress:
  - [ ] Enable minification in build (webpack, esbuild, Vite)
  - [ ] Enable Gzip/Brotli on server

**Estimated impact:** 15-30% INP improvement (plus faster downloads)

**Tools:** webpack, esbuild, Vite, rollup, source-map-explorer

---

### 3. Render-Blocking CSS (LCP Impact: MEDIUM-HIGH)

- [ ] Identify render-blocking CSS:
  - [ ] DevTools → Network tab
  - [ ] Look for `<link rel="stylesheet">` in red (blocking)
- [ ] Inline critical CSS:
  - [ ] Tools: CriticalCSS, Penthouse
  - [ ] Extract above-fold styles
  - [ ] Inline in `<style>` tag in `<head>`
- [ ] Defer non-critical CSS:
  - [ ] Move to end of body or load asynchronously
  - [ ] Use `media="print" onload="this.media='all'"` trick
- [ ] Minimize CSS:
  - [ ] Remove unused selectors (PurgeCSS, Tailwind)
  - [ ] Minify (cssnano, postcss)

**Estimated impact:** 10-20% LCP improvement

**Tools:** CriticalCSS, Penthouse, PurgeCSS, cssnano

---

### 4. Font Loading (LCP + CLS Impact: MEDIUM)

- [ ] Check if LCP element uses web font:
  - [ ] DevTools → Performance tab, look for font loading
- [ ] Choose font loading strategy:
  - [ ] Option A: System fonts only (fastest, no download)
  - [ ] Option B: Asynchronous loading with fallback
  - [ ] Option C: Preload critical font
- [ ] Implement choice:
  ```css
  /* Option B: Swap with fallback */
  @font-face {
    font-family: 'CustomFont';
    src: url('font.woff2') format('woff2');
    font-display: swap;
  }
  ```
- [ ] Use variable fonts if loading multiple weights:
  - [ ] Reduces from 4 files (400, 600, 700, 800) to 1
  - [ ] 50-70% size reduction

**Estimated impact:** 5-15% LCP improvement + reduced CLS

---

### 5. Server Response Time / TTFB (LCP Impact: MEDIUM)

- [ ] Check TTFB (Time to First Byte):
  - [ ] DevTools → Network tab → click HTML request → Timing tab
  - [ ] Target: < 600ms
- [ ] Optimize server response:
  - [ ] Use CDN for static HTML
  - [ ] Cache database queries / API responses
  - [ ] Optimize server-side rendering
  - [ ] Consider edge functions (Cloudflare Workers, Vercel Edge)
- [ ] Enable compression:
  - [ ] Gzip (older browsers)
  - [ ] Brotli (modern browsers, better compression)
- [ ] Check if database queries are slow:
  - [ ] Add indexes to frequently queried columns
  - [ ] Cache hot data in Redis
  - [ ] Use APM tool (Datadog, New Relic) to profile

**Estimated impact:** 5-20% LCP improvement (depends on current TTFB)

---

## Medium-Impact Optimizations

- [ ] **Defer Offscreen Images** (LCP-adjacent impact)
  - Load images below viewport only when needed
  - Use `loading="lazy"` or Intersection Observer
  - Estimated impact: 5-10% LCP improvement

- [ ] **Reduce CSS-in-JS Runtime Cost** (INP impact)
  - Extract static CSS at build time (styled-components, emotion)
  - Avoid CSS-in-JS on critical path
  - Estimated impact: 2-8% INP improvement

- [ ] **Optimize Third-Party Scripts** (LCP + INP impact)
  - Load analytics, ads, chat widgets asynchronously
  - Use `async` or defer attributes
  - Consider loading after page interactive
  - Estimated impact: 5-15% overall

- [ ] **Enable GZIP/Brotli Compression** (Bundle size impact)
  - Most servers support; just enable
  - 60-80% compression for text
  - Estimated impact: 30-50% download time improvement

---

## Low-Impact But Important

- [ ] **Unsized Images Cause Layout Shift** (CLS impact)
  - Every image needs `width` and `height` attributes
  - Or use CSS `aspect-ratio`
  - Estimated impact: 0.01-0.05 CLS reduction

- [ ] **Minify HTML** (Minor impact)
  - Remove whitespace, comments
  - 10-15% size reduction
  - Estimated impact: 2-5% LCP improvement

- [ ] **Upgrade to HTTP/2 or HTTP/3** (Minor impact)
  - Multiplexing speeds up parallel requests
  - Requires server/CDN support
  - Estimated impact: 5-10% overall

---

## Testing & Validation

### Quick Validation Workflow

1. [ ] Make one optimization
2. [ ] Run Lighthouse again (DevTools → Lighthouse)
3. [ ] Compare to baseline (record time and score)
4. [ ] If improved: keep, move to next optimization
5. [ ] If not improved: revert, try different approach

### Comprehensive Testing

- [ ] Test on real devices (phone, tablet, desktop)
- [ ] Test on slow networks (throttle in DevTools)
  - [ ] DevTools → Network tab → Slow 3G, Fast 3G, 4G
- [ ] Test in different browsers (Chrome, Firefox, Safari)
- [ ] Measure on real users (Google Analytics, RUM dashboard)

---

## Performance Budget Template

Set targets for your metrics. Enforce in CI/CD to prevent regressions.

```json
{
  "lighthouse": {
    "performance": 90,
    "accessibility": 90,
    "best-practices": 90,
    "seo": 90
  },
  "core-web-vitals": {
    "lcp": 2500,
    "inp": 200,
    "cls": 0.1
  },
  "budgets": {
    "javascript": 150000,
    "css": 50000,
    "images": 500000,
    "fonts": 200000,
    "html": 50000,
    "total": 1000000
  },
  "network": {
    "ttfb": 600,
    "fcp": 1800,
    "tti": 3800
  }
}
```

---

## Optimization Impact Reference

### By Metric Impact

**LCP Improvements (< 2.5s target)**
| Optimization | Typical Impact | Effort |
|---|---|---|
| Image compression | 20-40% | Low |
| Reduce JavaScript | 5-15% | Medium |
| Inline critical CSS | 10-20% | Medium |
| Preload critical resources | 5-10% | Low |
| Improve TTFB | 5-20% | High |
| Use CDN | 10-30% | Medium |

**INP Improvements (< 200ms target)**
| Optimization | Typical Impact | Effort |
|---|---|---|
| Reduce JavaScript | 15-30% | Medium |
| Code splitting | 10-25% | Medium |
| Defer non-critical code | 10-20% | Low |
| Avoid layout thrashing | 5-15% | Low |
| Use Web Workers | 10-50% | High |

**CLS Improvements (< 0.1 target)**
| Optimization | Typical Impact | Effort |
|---|---|---|
| Declare image dimensions | 0.01-0.05 | Low |
| Reserve space for ads | 0.05-0.15 | Low |
| Font loading strategy | 0.02-0.1 | Low |
| Avoid DOM insertion | 0.05-0.2 | Low |

---

## Common Mistakes to Avoid

❌ **Optimizing the wrong thing**
- Don't spend 10 hours optimizing 10KB JavaScript
- Follow Lighthouse Opportunities (ranked by impact)

❌ **Optimizing without measuring**
- Always get baseline before and after metrics
- "It feels faster" ≠ actually faster

❌ **Ignoring real user data**
- Lab testing (Lighthouse) is useful but doesn't capture all conditions
- Supplement with real user monitoring (Google Analytics, Datadog)

❌ **Using wrong tools**
- Don't use gzip when Brotli is supported (older = wrong choice)
- Don't use system fonts when brand font is critical
- Don't ignore user intent (some users want ads, customization)

❌ **Breaking the site to optimize**
- Never disable features to save 10ms
- Performance is important, but not at cost of functionality

---

## Next Steps After Optimization

1. **Set performance budget** in CI/CD to prevent regressions
2. **Monitor real users** (Google Analytics, Datadog, Sentry)
3. **Review quarterly** and re-optimize as code grows
4. **Train team** on performance best practices
5. **Celebrate wins** — faster sites = happier users = better metrics
