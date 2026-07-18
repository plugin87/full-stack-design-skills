# Typography Scale Generator

A practical guide to building a harmonious type scale for your product.

## The Math

A typographic scale uses a base size and ratio to generate harmonious sizes.

**Formula:** Size(n) = Base × Ratio^n

Example with 16px base and 1.25 ratio:
- Size 0: 16 × 1.25^0 = 16px (base)
- Size 1: 16 × 1.25^1 = 20px
- Size 2: 16 × 1.25^2 = 25px
- Size 3: 16 × 1.25^3 = 31px
- Size 4: 16 × 1.25^4 = 39px (round to 40px)

## Ratio Selection

Choose a ratio based on your aesthetic:

| Ratio  | Name               | Feel               | Best For           |
|--------|--------------------|--------------------|-------------------|
| 1.067  | Minor Second       | Subtle, safe       | Minimal designs    |
| 1.125  | Major Second       | Balanced, neutral  | Most web products  |
| 1.25   | Major Third        | Slightly dramatic  | Modern startups    |
| 1.5    | Perfect Fifth      | Bold, dramatic     | Editorial, brand   |

**Recommendation for UI:** Start with 1.125 or 1.25. Go higher if you want bigger contrast between sizes, lower for subtlety.

## Template: 16px Base, 1.125 Ratio

| Name  | Size | Weight | Use Case                        | Line Height |
|-------|------|--------|--------------------------------|------------|
| Xs    | 12px | 400    | Captions, helper text           | 1.5        |
| Sm    | 14px | 400    | Small labels, metadata          | 1.6        |
| Base  | 16px | 400    | Body text, default              | 1.7        |
| Md    | 18px | 500    | Label emphasis, small heading   | 1.5        |
| Lg    | 20px | 600    | Section heading                 | 1.4        |
| Xl    | 24px | 700    | Page heading                    | 1.3        |
| 2xl   | 32px | 700    | Hero title                      | 1.2        |

## Implementation in CSS

```css
:root {
  --font-size-xs: 12px;
  --font-size-sm: 14px;
  --font-size-base: 16px;
  --font-size-md: 18px;
  --font-size-lg: 20px;
  --font-size-xl: 24px;
  --font-size-2xl: 32px;
  
  --line-height-tight: 1.2;
  --line-height-normal: 1.5;
  --line-height-loose: 1.7;
  
  --font-weight-regular: 400;
  --font-weight-medium: 500;
  --font-weight-bold: 700;
}

body {
  font-size: var(--font-size-base);
  line-height: var(--line-height-loose);
  font-weight: var(--font-weight-regular);
}

h1 {
  font-size: var(--font-size-2xl);
  line-height: var(--line-height-tight);
  font-weight: var(--font-weight-bold);
}

h2 {
  font-size: var(--font-size-xl);
  line-height: var(--line-height-normal);
  font-weight: var(--font-weight-bold);
}

label {
  font-size: var(--font-size-sm);
  font-weight: var(--font-weight-medium);
}
```

## Quick Check: Does Your Scale Work?

- [ ] Sizes increase consistently (each step is ~12% larger)
- [ ] Smallest size is 12px or larger
- [ ] Largest size (hero) is 2.5–3× the base
- [ ] You have 5–7 distinct sizes max
- [ ] Headings are noticeably larger than body
- [ ] Captions are noticeably smaller than body
- [ ] All sizes are whole pixels (no decimals)

## Common Pitfall: The Fibonacci Trap

Don't use Fibonacci sequence (1, 1, 2, 3, 5, 8, 13...) — it creates inconsistent ratios:
- Ratio 2/1 = 2.0 (way too big jump)
- Ratio 3/2 = 1.5 (huge jump)
- Ratio 5/3 = 1.67 (still inconsistent)

Use a consistent multiplier instead (1.125, 1.25, 1.5). Your users' eyes will thank you.
