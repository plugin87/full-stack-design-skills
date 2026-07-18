# Web Performance Optimization - Light-Weight Test Cases

A set of 6 practical scenarios to validate the skill works well for real-world performance challenges.

## Test Case 1: Lighthouse Audit Interpretation

**Prompt:**
```
I ran Lighthouse and got these scores:
- Performance: 68/100
- LCP: 3.2s (poor)
- INP: 245ms (poor)
- CLS: 0.08 (good)

The Opportunities section shows:
1. "Eliminate render-blocking resources" (saves 1.8s)
2. "Defer offscreen images" (saves 0.9s)
3. "Reduce JavaScript" (saves 0.6s)

How should I prioritize these? What should I do first?
```

**Expected Skill Response:**
- Recognize that Opportunities section is prioritized by impact
- Recommend "Eliminate render-blocking resources" first (1.8s potential saving)
- Explain what render-blocking resources are (CSS/JS files in head)
- Provide concrete fixes (inline critical CSS, defer JS)
- Acknowledge INP is also poor (245ms > 200ms target)
- Recommend addressing JS reduction (helps both LCP and INP)
- Suggest a prioritized order:
  1. Eliminate render-blocking (LCP impact: 1.8s)
  2. Reduce JavaScript (INP impact + LCP impact: 0.6s)
  3. Defer images (LCP impact: 0.9s, lower priority)

**Criteria:** Skill interprets Lighthouse priorities correctly, provides actionable fixes, explains trade-offs.

---

## Test Case 2: Core Web Vitals Diagnosis

**Prompt:**
```
Our LCP is 3.5s and users are bouncing off the site.
I think it's because the hero image is loading slowly.

How do I confirm this is the problem?
How do I fix it?
What else could be causing slow LCP?
```

**Expected Skill Response:**
- Recommend using Lighthouse or DevTools to identify LCP element
- Explain how to check which element is LCP (could be image, text, video)
- If LCP is image:
  - Recommend preloading hero image
  - Suggest compression and CDN delivery
  - Ask: is it above or below fold?
  - Recommend WebP format with JPEG fallback
- If LCP is text:
  - Problem is likely font loading or render-blocking CSS
  - Recommend font-display: swap
  - Recommend inlining critical CSS
- Other causes:
  - Server response time (TTFB)
  - JavaScript blocking parsing
  - Large CSS files
- Provide concrete debugging steps (DevTools Performance tab)

**Criteria:** Skill diagnoses LCP causes systematically, distinguishes between image/text/other, provides measurement methodology.

---

## Test Case 3: Image Optimization Strategy

**Prompt:**
```
My site has dozens of product images. They're 1-3MB each.
I need to optimize them but can't hire an expert.

What's the fastest (and cheapest) way to fix this?
I have maybe 4 hours.
```

**Expected Skill Response:**
- Recognize this is high-impact optimization (50-80% page weight is often images)
- Provide quick-fix options:
  - Option 1: Bulk compress using ImageOptim (Mac) or similar tool
  - Option 2: Use online tool like TinyPNG (free tier available)
  - Option 3: Hire Cloudinary/Imgix to handle optimization (they auto-optimize)
- Recommend approach:
  - Identify 5-10 most-viewed pages
  - Start with those product images
  - Compress to 75-85% quality (usually imperceptible)
  - Target 150-300KB per image
- Additional quick wins:
  - Add width/height attributes (prevents layout shift)
  - Lazy load below-fold images (loading="lazy")
  - Use CDN if not already
- Expected result: 80-90% image size reduction, noticeably faster LCP

**Criteria:** Skill prioritizes high-impact work, provides tool recommendations, realistic timeline expectations.

---

## Test Case 4: JavaScript Bundle Size Reduction

**Prompt:**
```
My JavaScript bundle is 450KB (gzipped).
Lighthouse says "Reduce JavaScript" is a high-impact opportunity.

I don't have time to rewrite the app.
What can I do in 1-2 days?
```

**Expected Skill Response:**
- Acknowledge 450KB is high (target: ≤150KB for most sites)
- Provide quick wins (1-2 day timeline):
  1. Check for unused code (DevTools Coverage tab)
  2. Remove obviously unused dependencies
  3. Enable minification if not already
  4. Implement code splitting by route (lazy load pages/modals)
  5. Check for duplicate dependencies (npm ls)
- Explain tradeoffs:
  - Aggressive minification: safe, 5-10% savings
  - Code splitting: requires changes but high impact (20-40% savings)
  - Replacing libraries: risky, could break site
- Recommend starting with:
  1. Enable minification (safest, 5-10% savings)
  2. Defer non-critical JS (load after page interactive)
  3. Identify one large feature to code split (modals, admin pages)
- Explain why this helps INP (less code = faster processing)

**Criteria:** Skill distinguishes quick wins from long-term improvements, provides realistic timeline, explains impact on both LCP and INP.

---

## Test Case 5: Layout Shift (CLS) Investigation

**Prompt:**
```
My CLS is 0.18 (poor, target is < 0.1).
I think ads are causing layout shifts, but I'm not 100% sure.

How do I identify which element is causing shifts?
How do I fix it?
```

**Expected Skill Response:**
- Recommend measurement approach:
  - DevTools Performance tab: record page load, look for layout shifts (red)
  - DevTools Console: run PerformanceObserver to log shifts
  - Lighthouse report: check "Cumulative Layout Shift" section
- Common culprits (check each):
  1. Images loading without dimensions → Add width/height
  2. Ads inserted above fold → Reserve space (div with fixed size)
  3. Web fonts causing text reflow → Use font-display: swap
  4. JavaScript inserting DOM → Avoid DOM insertion above fold
- Explain why 0.18 is bad:
  - Users experience jarring layout changes
  - Accidental clicks (button moved mid-click)
  - Poor experience overall
- Provide fixes for each culprit:
  - Images: `<img width="800" height="600" />`
  - Ads: `<div style="height: 250px; width: 300px;"><div id="ad"></div></div>`
  - Fonts: `@font-face { font-display: swap; }`
- Recommend testing in slow networks (shifts might not appear on fast networks)

**Criteria:** Skill diagnoses CLS systematically, identifies common causes, provides specific fixes, explains user impact.

---

## Test Case 6: Performance Monitoring Setup

**Prompt:**
```
I want to track performance metrics over time.
I want to know when performance degrades.
I have no budget for premium tools.

What's the simplest setup to get real-world performance data?
```

**Expected Skill Response:**
- Recommend Google Analytics (free, built-in):
  - Already installed? Google Analytics 4 automatically collects Web Vitals
  - No code needed
  - View in: Reports → Engagement → Page loads → Web Vitals
  - Can segment by device, country, page path
- For deeper insights, recommend custom implementation:
  - Use `web-vitals` library (npm install web-vitals)
  - Send data to own backend with sendBeacon
  - Store in database
  - Query and alert when metrics exceed thresholds
- For CI/CD regression prevention:
  - Add Lighthouse CI (free, GitHub Action)
  - Runs on every PR
  - Blocks merge if scores below target
  - Takes 5 minutes to set up
- For real-time alerting:
  - Set up email/Slack alerts when LCP > 3s
  - Monitor weekly trends
- Explain why this matters:
  - Lab tests (Lighthouse) don't capture all real-world conditions
  - Real user data is the source of truth
  - Monitoring prevents regressions before users notice

**Criteria:** Skill recommends free/simple first options, explains tradeoffs, provides implementation guidance, emphasizes RUM importance.

---

## Success Criteria

All test cases should result in:
- ✅ Practical, actionable advice
- ✅ References to Core Web Vitals or optimization strategies from SKILL.md
- ✅ Measurement methodology (how to diagnose)
- ✅ Prioritization (what to fix first)
- ✅ Realistic timelines and tradeoffs
- ✅ Explanation of user impact

---

## Evaluation Notes

This skill should:
- **Never** prescribe specific tools without alternatives (unless appropriate)
- **Always** explain which metric is affected (LCP, INP, CLS)
- **Always** provide measurement/diagnosis methodology first
- **Always** acknowledge tradeoffs (time, complexity, impact)
- **Always** explain user-facing impact (why this matters)
- **Always** rank by impact (Lighthouse's approach)
- **Acknowledge** that performance is ongoing work, not one-time fix

Test success = User understands the performance issue, can diagnose it, and can prioritize fixes based on impact.
