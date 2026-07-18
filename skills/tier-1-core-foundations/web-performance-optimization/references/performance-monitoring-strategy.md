# Performance Monitoring & Measurement Strategy

Setting up real-world performance monitoring and defining metrics that matter.

---

## Monitoring Stack: What You Need

A complete performance monitoring setup has 3 layers:

### Layer 1: Lab Testing (Development)
- Lighthouse (automated audits during CI/CD)
- DevTools Performance profiler
- Network throttling simulation

### Layer 2: Synthetic Testing (Production-like)
- Automated page loads from multiple locations
- Consistent conditions (same network, device)
- Catches issues before users see them

### Layer 3: Real User Monitoring (Actual Users)
- Measures real user experience
- Accounts for variable networks, devices, geography
- Only source of truth for performance

**Recommended:** Layer 1 + Layer 3. Layers 2 is optional but nice-to-have.

---

## Real User Monitoring (RUM) Setup

### Option 1: Google Analytics (Free, Built-in)

Google Analytics 4 automatically collects Core Web Vitals. No setup needed.

**What's collected:**
- LCP (Largest Contentful Paint)
- INP (Interaction to Next Paint, replacing FID)
- CLS (Cumulative Layout Shift)
- Plus: page load time, scroll engagement, etc.

**Access in GA4:**
1. Go to Google Analytics 4 property
2. Reports → Engagement → Page loads
3. Scroll down → "Web Vitals"
4. Breakdown by device, country, page path

**Pros:**
- Already in GA if you use it
- Free
- Automatic collection
- No additional code needed

**Cons:**
- Basic reporting only
- No detailed debugging information
- Limited filtering options
- 48-hour delay in data

### Option 2: Custom Implementation (Free)

Collect Web Vitals yourself and send to your own backend.

**Minimal implementation:**
```javascript
// core-vitals.js
import { getCLS, getFCP, getFID, getLCP, getTTFB } from 'web-vitals';

function sendToAnalytics(metric) {
  // Send to your backend
  navigator.sendBeacon('/api/metrics', JSON.stringify(metric));
}

getCLS(sendToAnalytics);
getFCP(sendToAnalytics);
getFID(sendToAnalytics); // Or INP
getLCP(sendToAnalytics);
getTTFB(sendToAnalytics);

// On page unload, also send basic info
window.addEventListener('beforeunload', () => {
  sendToAnalytics({
    type: 'page-info',
    url: location.href,
    userAgent: navigator.userAgent,
    network: navigator.connection?.effectiveType,
  });
});
```

**Backend storage (pseudocode):**
```javascript
// POST /api/metrics
app.post('/api/metrics', (req, res) => {
  const metric = req.body;
  
  // Store in database
  db.metrics.insert({
    url: metric.url,
    lcp: metric.lcp,
    inp: metric.inp,
    cls: metric.cls,
    timestamp: new Date(),
    userAgent: metric.userAgent,
    network: metric.network,
  });
  
  res.json({ ok: true });
});
```

**Query and analyze:**
```sql
-- Average LCP by page
SELECT 
  url, 
  AVG(lcp) as avg_lcp,
  COUNT(*) as samples
FROM metrics
WHERE timestamp > NOW() - INTERVAL 7 DAY
GROUP BY url
ORDER BY avg_lcp DESC;

-- LCP by network type
SELECT 
  network,
  AVG(lcp) as avg_lcp
FROM metrics
WHERE timestamp > NOW() - INTERVAL 7 DAY
GROUP BY network;
```

**Pros:**
- Complete control
- No vendor lock-in
- Custom metrics possible
- Zero cost (if you have backend)

**Cons:**
- Requires backend infrastructure
- Need to build dashboard
- Maintenance burden
- No out-of-the-box dashboards

### Option 3: Datadog (Premium)

Paid real user monitoring platform with advanced features.

**Setup:**
```javascript
<script src="https://www.datadoghq-browser-agent.com/datadog-rum.js"></script>
<script>
  window.DD_RUM.init({
    applicationId: '<APP_ID>',
    clientToken: '<CLIENT_TOKEN>',
    service: 'my-website',
    env: 'production',
  });

  window.DD_RUM.startSessionReplayRecording();
</script>
```

**What's included:**
- Core Web Vitals
- Session replays (see what user saw)
- Error tracking
- Performance profiling
- Log aggregation

**Pricing:** $15-40/month per user (steep for small sites)

**Pros:**
- Professional dashboards
- Session replay (debugging)
- Error tracking
- Advanced querying

**Cons:**
- Expensive
- Adds JavaScript overhead
- Data privacy concerns (session replay)

---

## Lighthouse CI: Automated Regression Testing

Prevent performance regressions by running Lighthouse on every PR.

### Setup (GitHub Actions)

1. **Create config file** (`lighthouse-budget.json`):
```json
{
  "ci": {
    "collect": {
      "numberOfRuns": 3,
      "url": [
        "http://localhost:3000/",
        "http://localhost:3000/products",
        "http://localhost:3000/blog"
      ],
      "headless": true
    },
    "assert": {
      "preset": "lighthouse:recommended",
      "assertions": {
        "categories:performance": ["error", { "minScore": 0.9 }],
        "categories:accessibility": ["error", { "minScore": 0.9 }]
      }
    }
  }
}
```

2. **Create GitHub Action** (`.github/workflows/lighthouse.yml`):
```yaml
name: Lighthouse CI

on: [push, pull_request]

jobs:
  lighthouse:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: 18

      - name: Install dependencies
        run: npm ci

      - name: Build
        run: npm run build

      - name: Start server
        run: npm start &

      - name: Wait for server
        run: npx wait-on http://localhost:3000

      - name: Run Lighthouse CI
        uses: treosh/lighthouse-ci-action@v10
        with:
          uploadArtifacts: true
          temporaryPublicStorage: true
          configPath: ./lighthouse-budget.json
```

3. **Result on PR:**
- Automatically runs Lighthouse
- Compares to baseline
- Comments with score/regression
- Blocks merge if scores below target

### Alternative: Bundle Size Monitoring

Track JavaScript bundle size to catch regressions:

```yaml
- name: Check bundle size
  uses: andresz1/size-limit-action@v1
  with:
    github_token: ${{ secrets.GITHUB_TOKEN }}
    package_manager: npm
    main: dist/main.js
    limit: 150KB
```

---

## Performance Monitoring Dashboard

Create a dashboard to track performance over time.

### Metrics to Track (Weekly)

- **LCP:** Average and 75th percentile (p75)
- **INP:** Average and p75
- **CLS:** Average and p75
- **Bundle size:** JavaScript, CSS, images
- **Core Web Vitals pass rate:** % of users meeting targets

### Dashboard Queries

**Google Analytics 4 (built-in Reports):**
1. Go to Reports → Engagement → Page loads
2. View "Web Vitals" card
3. Segment by: Device, Country, Page path, Browser
4. Compare date ranges

**Custom Dashboard (SQL/Python):**
```sql
-- Weekly performance trends
SELECT 
  DATE(timestamp) as date,
  ROUND(AVG(lcp), 0) as avg_lcp_ms,
  ROUND(PERCENTILE_CONT(0.75) WITHIN GROUP (ORDER BY lcp), 0) as p75_lcp_ms,
  ROUND(AVG(inp), 0) as avg_inp_ms,
  ROUND(AVG(cls), 3) as avg_cls,
  COUNT(*) as samples
FROM metrics
WHERE timestamp > NOW() - INTERVAL 30 DAY
GROUP BY DATE(timestamp)
ORDER BY date DESC;
```

**Alert thresholds:**
- Alert if LCP > 3000ms
- Alert if INP > 250ms
- Alert if CLS > 0.15
- Alert if pass rate < 50%

---

## Performance Budget: Enforcement

Define maximum allowable metrics. Enforce automatically.

### Bundle Size Budget

File: `size-limit.json`
```json
[
  {
    "path": "dist/main.js",
    "limit": "150KB"
  },
  {
    "path": "dist/*.css",
    "limit": "50KB"
  }
]
```

CI check:
```bash
npm run size-limit
```

### Core Web Vitals Budget

File: `.lighthouserc.json`
```json
{
  "ci": {
    "assert": {
      "preset": "lighthouse:recommended",
      "assertions": {
        "largest-contentful-paint": ["error", { "maxNumericValue": 2500 }],
        "cumulative-layout-shift": ["error", { "maxNumericValue": 0.1 }],
        "first-input-delay": ["error", { "maxNumericValue": 100 }]
      }
    }
  }
}
```

### Enforcement in CI/CD

```bash
# On each commit
npm run lighthouse:ci  # Exits with code 1 if over budget
npm run size-limit     # Exits with code 1 if over budget

# If either fails, build is rejected
```

---

## Debugging Performance Issues

### Workflow: When LCP is High

**Step 1: Identify LCP element**
```javascript
new PerformanceObserver((list) => {
  const entries = list.getEntries();
  const lastEntry = entries[entries.length - 1];
  console.log('LCP Element:', lastEntry.element);
  console.log('LCP Time:', lastEntry.renderTime || lastEntry.loadTime);
  console.log('LCP Type:', lastEntry.element.tagName);
}).observe({ entryTypes: ['largest-contentful-paint'] });
```

**Step 2: Check what's slow**
- Element is an image? → Image optimization needed
- Element is text? → Font loading or render-blocking CSS
- Element is video? → Video preload or CDN

**Step 3: Profile with DevTools**
1. DevTools → Performance tab
2. Click record
3. Reload page
4. Click stop
5. Look for:
   - Long tasks (red)
   - CSS parsing (yellow)
   - Image loading timeline
6. Check Network tab for slow requests

### Workflow: When INP is High

**Step 1: Identify slow interaction**
```javascript
const observer = new PerformanceObserver((list) => {
  list.getEntries().forEach((entry) => {
    console.log('Interaction:', {
      name: entry.name,
      duration: entry.duration,
      processingStart: entry.processingStart,
    });
  });
});
observer.observe({ type: 'first-input', buffered: true });
```

**Step 2: Find the bottleneck**
- Processing duration > 100ms? → JavaScript work taking too long
- Duration includes paint > 50ms? → Rendering is slow

**Step 3: Profile JavaScript**
1. DevTools → Performance tab
2. Record during interaction
3. Look for long functions
4. Use DevTools Profiler to identify hot functions

### Workflow: When CLS is High

**Step 1: Identify shift**
```javascript
new PerformanceObserver((list) => {
  list.getEntries().forEach((entry) => {
    if (!entry.hadRecentInput) {
      console.log('Layout Shift:', {
        element: entry.sources[0]?.node,
        value: entry.value,
        hadRecentInput: entry.hadRecentInput,
      });
    }
  });
}).observe({ type: 'layout-shift', buffered: true });
```

**Step 2: Identify culprit**
- Which element shifted?
- Common causes: image, ad, font loading, DOM insertion

**Step 3: Fix**
- Image: Add width/height
- Ad: Reserve space
- Font: Use font-display: swap
- DOM: Avoid insertion above fold

---

## Alerting & Response

### Alert Channels

**Set up notifications when metrics exceed threshold:**

Email (Google Analytics):
- Set up email alerts in GA4 (Reports → Create custom alert)

Slack (Custom implementation):
```javascript
// When metric exceeds threshold, post to Slack
async function alertSlack(message) {
  await fetch('https://hooks.slack.com/services/YOUR/WEBHOOK', {
    method: 'POST',
    body: JSON.stringify({
      text: message,
      channel: '#performance-alerts',
    }),
  });
}

// In monitoring code:
if (avgLcp > 3000) {
  alertSlack(`🚨 LCP high: ${avgLcp}ms (target: 2500ms)`);
}
```

### Response Checklist

When alerted to regression:

1. [ ] Confirm it's real (check multiple times, different device)
2. [ ] Identify cause (Lighthouse, DevTools, git log)
3. [ ] Revert last commit if obvious culprit
4. [ ] Optimize if non-obvious (follow debugging workflow above)
5. [ ] Re-measure to confirm fix
6. [ ] Post-mortem (why did it regress? Prevent in future)

---

## Annual Performance Review

Quarterly or annually, review performance holistically:

- [ ] Compare YoY trends (same period last year)
- [ ] Check competitive benchmarks (comparable sites)
- [ ] Review real user experience (survey, user interviews)
- [ ] Check if business metrics improved (conversion, engagement)
- [ ] Update performance budget if needed
- [ ] Plan Q1 goals (next priority improvements)

---

## Tools Reference

| Task | Tool | Cost | Setup |
|------|------|------|-------|
| Lab testing | Lighthouse | Free | Built-in DevTools |
| Monitoring (basic) | Google Analytics | Free | 1-line script |
| Monitoring (advanced) | Datadog | $15-40/mo | 5-min setup |
| Bundle monitoring | size-limit | Free | npm package |
| Regression testing | Lighthouse CI | Free | GitHub Action |
| Dashboarding | Grafana | Free/Paid | Self-hosted |
| APM (for server) | New Relic | $50-500/mo | Depends |
