# Core Web Vitals: Detailed Reference

In-depth guidance for understanding and optimizing Google's Core Web Vitals.

---

## Largest Contentful Paint (LCP)

### What Gets Measured?

Only these elements count toward LCP:

- `<img>` elements
- `<svg>` with `<image>` child
- Backgrounds with `background-image` (only above the fold)
- `<video>` elements (poster frame counts, not first play)
- Text blocks (rendered text nodes with font applied)
- Canvas elements

### What Doesn't Count?

- Elements outside viewport
- Elements with opacity: 0
- Opacity animations (changing opacity after paint)
- Text inside canvas or SVG
- Invisible text (color: transparent)

### LCP Timeline

```
0ms       Page starts loading
50ms      HTML parsing begins
200ms     CSS/JS start downloading
400ms     Hero image starts downloading
800ms     Hero image finishes   ← LCP = 800ms ✓ Good
1200ms    Page fully interactive (TTI)
```

### Optimization Strategies

**Priority 1: Defer Below-the-Fold Content**
```html
<!-- LCP element at top of page -->
<img src="hero.jpg" alt="..." width="1200" height="600" />

<!-- Below-the-fold images lazy load -->
<img src="product.jpg" loading="lazy" alt="..." />
```

**Priority 2: Preload Critical Resources**
```html
<link rel="preload" href="hero.jpg" as="image" />
<link rel="preload" href="fonts.woff2" as="font" type="font/woff2" crossorigin />
```

**Priority 3: Reduce Server Response Time (TTFB)**
- Use CDN for static HTML
- Cache database queries
- Optimize server-side rendering
- Consider edge functions (Cloudflare Workers, Vercel Edge)

**Priority 4: Eliminate Render-Blocking Resources**
```html
<!-- Bad: blocks rendering -->
<link rel="stylesheet" href="critical.css" />
<script src="app.js"></script>

<!-- Good: inline critical, defer rest -->
<style>
  /* Critical CSS only (above fold) */
</style>
<link rel="stylesheet" href="non-critical.css" media="print" onload="this.media='all'" />
<script src="app.js" defer></script>
```

**Priority 5: Compress Images**

Image size is the #1 factor in LCP. Example:

```
Original: photo.jpg (2.4MB)
Optimized: photo.webp (180KB) @ 75% quality

95% size reduction. Same perceived quality.
```

Tools:
- ImageOptim (Mac)
- OptiPNG (PNG)
- mozjpeg (JPEG)
- cwebp (WebP)
- Cloud services (Cloudinary, Imgix)

---

## First Input Delay (FID) & Interaction to Next Paint (INP)

### FID: Old Metric (Deprecated, But Still Measured)

**What it measures:** Delay from user input to browser starts processing JavaScript.

**Timeline:**
```
User clicks at: 0ms
Browser waits (FID): 0-150ms (because JavaScript is running)
Browser processes click: 150-180ms
Paint: 180-185ms
Response shown: 185ms
```

**Problem:** Only measures first interaction. Site could be fast initially, then slow later.

### INP: New Metric (Recommended)

**What it measures:** Delay from any user interaction to paint appears.

**Measures all interactions** throughout page lifetime (clicks, scrolls, keyboard).

**Timeline (same example):**
```
User clicks: 0ms
Browser waits (processing time): 0-150ms
Browser repaints: 150-185ms
→ INP = 185ms (entire process, not just FID)
```

**Key difference:** FID = delay before processing. INP = delay + processing + paint.

### Optimization Strategies

**Break Long Tasks (> 50ms)**

Bad: Single 250ms task blocks everything
```javascript
// This freezes the page for 250ms
function heavyComputation() {
  let sum = 0;
  for (let i = 0; i < 1000000000; i++) {
    sum += i;
  }
  return sum;
}

heavyComputation();
```

Good: Split into smaller tasks
```javascript
async function heavyComputationChunked() {
  let sum = 0;
  for (let i = 0; i < 100000000; i++) {
    sum += i;
    if (i % 10000000 === 0) {
      // Yield to browser every 10M iterations
      await scheduler.yield();
    }
  }
  return sum;
}
```

**Use Web Workers**
```javascript
// Main thread (UI)
const worker = new Worker('heavy-computation.js');
worker.postMessage(data);
worker.onmessage = (e) => {
  console.log('Result:', e.data); // UI stays responsive
};

// heavy-computation.js (worker thread)
self.onmessage = (e) => {
  const result = expensiveCalculation(e.data);
  self.postMessage(result);
};
```

**Lazy Load JavaScript**
```html
<!-- Route-based code splitting -->
<script src="app.js"></script>
<script src="admin-page.js" data-route="/admin"></script>

<!-- Loads only when needed -->
<script>
  if (location.pathname === '/admin') {
    import('./admin-page.js');
  }
</script>
```

**Avoid Layout Thrashing**
```javascript
// Bad: read-write-read alternates = reflows
for (let i = 0; i < 100; i++) {
  element.style.width = element.offsetWidth + 10 + 'px';
}

// Good: batch reads, then batch writes
const widths = [...elements].map(e => e.offsetWidth);
widths.forEach((w, i) => {
  elements[i].style.width = (w + 10) + 'px';
});
```

---

## Cumulative Layout Shift (CLS)

### How CLS is Calculated

Every time an element moves without user input:
```
CLS += (fraction of viewport affected) × (distance moved)
```

Example:
- Button moves 80px down
- Button occupies 10% of viewport
- CLS += 0.1 × 0.08 = 0.008

Multiple shifts accumulate:
```
Initial CLS: 0
Ad loads: CLS += 0.15 → Total: 0.15
Font swap: CLS += 0.08 → Total: 0.23
Image loads: CLS += 0.05 → Total: 0.28 ❌ Poor (> 0.1)
```

### Common Culprits

**1. Images Without Dimensions**

Bad:
```html
<img src="photo.jpg" alt="..." />
```
Renders as 0×0 until loaded, then expands to full size → layout shift.

Good:
```html
<img src="photo.jpg" alt="..." width="800" height="600" />
```
Browser reserves space immediately.

Or use CSS aspect-ratio:
```css
img {
  aspect-ratio: 16 / 9;
  width: 100%;
}
```

**2. Ads Inserted Dynamically**

Bad:
```html
<div id="ad"></div>
<script>
  setTimeout(() => {
    document.getElementById('ad').innerHTML = '<iframe src="ad.html"></iframe>';
  }, 2000);
</script>
```
Ad suddenly appears 2 seconds later → layout shift.

Good: Reserve space in HTML
```html
<div style="height: 250px; width: 300px;">
  <div id="ad"></div>
</div>
```

**3. Web Fonts Causing Text Reflow**

Bad: Default behavior blocks text until font loads
```css
@font-face {
  font-family: 'MyFont';
  src: url('font.woff2');
  /* Blocks text rendering until font loaded */
}
```

Good: Use `font-display: swap`
```css
@font-face {
  font-family: 'MyFont';
  src: url('font.woff2');
  font-display: swap; /* Show fallback immediately, swap when loaded */
}
```

**4. CSS Transform Instead of Position**

Bad: Causes layout reflow
```css
/* Avoid: causes layout recalc */
#element {
  position: absolute;
  top: 50px;
}

#element.shifted {
  top: 130px; /* Layout recalculated */
}
```

Good: Use transform (GPU-accelerated, no layout reflow)
```css
#element {
  transform: translateY(0);
}

#element.shifted {
  transform: translateY(80px); /* No layout recalc */
}
```

### Optimization Checklist

- [ ] All images have explicit `width` and `height` (or CSS aspect-ratio)
- [ ] Ads/embeds have reserved space in HTML
- [ ] Web fonts use `font-display: swap`
- [ ] Position changes use `transform: translate()`, not `top`/`left`
- [ ] No DOM insertion above the fold after page load
- [ ] Animations use CSS transforms, not position changes

---

## Measuring Core Web Vitals

### Tools

**Lighthouse (Lab)**
- Simulated conditions
- Fully controlled environment
- Good for: development, comparing before/after

**Chrome DevTools**
- Real device data
- Slow network simulation (3G, 4G)
- Good for: development iteration

**PageSpeed Insights (Lab + Field)**
- Lab data from Lighthouse
- Field data from real users (Chrome User Experience Report)
- Good for: baseline assessment

**Google Analytics (Field)**
- Real user data
- Breakdown by device, country, page
- Good for: understanding user experience

**Custom PerformanceObserver (Field)**
```javascript
// Measure LCP
new PerformanceObserver((list) => {
  const entries = list.getEntries();
  const lastEntry = entries[entries.length - 1];
  console.log('LCP:', lastEntry.renderTime || lastEntry.loadTime);
}).observe({ entryTypes: ['largest-contentful-paint'] });

// Measure INP
new PerformanceObserver((list) => {
  let maxDuration = 0;
  list.getEntries().forEach((entry) => {
    if (entry.duration > maxDuration) {
      maxDuration = entry.duration;
    }
  });
  console.log('INP:', maxDuration);
}).observe({ type: 'first-input', buffered: true });

// Measure CLS
let clsValue = 0;
new PerformanceObserver((list) => {
  list.getEntries().forEach((entry) => {
    if (!entry.hadRecentInput) {
      const firstSessionEntry = clsValue + entry.value;
      console.log('CLS:', firstSessionEntry);
    }
  });
}).observe({ type: 'layout-shift', buffered: true });
```

### Real User vs. Lab Testing

| Aspect | Lab (Lighthouse) | Field (RUM) |
|--------|-----------------|------------|
| **Conditions** | Simulated (fixed) | Real (variable) |
| **Speed** | Fast | Depends on network |
| **Device** | Simulated | Real devices |
| **Network** | 4G | WiFi, 3G, 4G, 5G |
| **Sample size** | 1 run | Millions of users |
| **Good for** | Development, regression testing | Understanding real users |

**Best practice:** Use both. Lab testing during development, field testing for production.
