# Typography Reference

Extended recipes for Apple-inspired web typography. Builds on `apple-design-foundations/SKILL.md`.

## The full type scale

This is a starting point. Adjust to your product's tone.

```css
:root {
  /* Type scale — modular, ratio 1.25 (major third) at large sizes, 1.2 at body */
  --fs-display-xl: clamp(3.5rem, 7vw + 0.5rem, 6rem);     /* 56–96 px */
  --fs-display-l:  clamp(2.5rem, 4vw + 0.5rem, 4rem);      /* 40–64 px */
  --fs-title:      clamp(1.625rem, 1.5vw + 1rem, 2.25rem);/* 26–36 px */
  --fs-lead:       clamp(1.125rem, 0.5vw + 1rem, 1.5rem); /* 18–24 px */
  --fs-body:       1.0625rem;                             /* 17 px */
  --fs-caption:    0.8125rem;                             /* 13 px */
  --fs-eyebrow:    0.75rem;                               /* 12 px */

  /* Line heights */
  --lh-display-xl: 1.05;
  --lh-display-l:  1.10;
  --lh-title:      1.18;
  --lh-lead:       1.40;
  --lh-body:       1.50;
  --lh-caption:    1.30;

  /* Letter spacing */
  --ls-display-xl: -0.035em;
  --ls-display-l:  -0.025em;
  --ls-title:      -0.01em;
  --ls-body:       0;
  --ls-caption:    0;
  --ls-eyebrow:    0.06em;

  /* Weights */
  --fw-regular: 400;
  --fw-medium:  500;
  --fw-semibold: 600;
  --fw-bold:    700;

  /* Body measure */
  --measure-body: 65ch;
}
```

## Font stack — the safe Apple-leaning stack

```css
:root {
  --font-sans:
    "SF Pro Display", "SF Pro Text", "SF Pro",
    -apple-system, BlinkMacSystemFont,
    "Helvetica Neue",
    "Segoe UI", system-ui,
    Roboto, "Noto Sans", "Liberation Sans", sans-serif;
  --font-serif:
    "New York", "New York Pro", ui-serif, Georgia, "Times New Roman", serif;
  --font-mono:
    ui-monospace, "SF Mono", Menlo, Monaco, "Cascadia Code", monospace;
}
```

Notes:

- Don't `@font-face` SF Pro unless you have an explicit license.
- "New York" is Apple's serif; on non-Apple platforms it falls back to the system serif (Georgia on Windows, Noto Serif on Linux, etc.).
- For products that need a more distinctive voice, pair the system sans with a paid or open-licensed display face (e.g. Inter, IBM Plex, Söhne). Do **not** call Inter "Apple-like" — it is its own thing.

## Heading recipes

### Display XL (hero)

```css
.h-display-xl {
  font-size: var(--fs-display-xl);
  line-height: var(--lh-display-xl);
  letter-spacing: var(--ls-display-xl);
  font-weight: var(--fw-semibold);
  color: var(--text-primary);
  /* Optical centering for big type */
  text-wrap: balance;
  /* Avoid orphans */
  text-wrap: pretty;
}
```

`text-wrap: balance` is supported in modern browsers and produces more even line lengths for headlines.

### Title

```css
.h-title {
  font-size: var(--fs-title);
  line-height: var(--lh-title);
  letter-spacing: var(--ls-title);
  font-weight: var(--fw-semibold);
  text-wrap: balance;
}
```

### Body lead

```css
.body-lead {
  font-size: var(--fs-lead);
  line-height: var(--lh-lead);
  max-width: var(--measure-body);
  color: var(--text-secondary);
}
```

### Body

```css
.body {
  font-size: var(--fs-body);
  line-height: var(--lh-body);
  max-width: var(--measure-body);
  color: var(--text-primary);
}
```

### Caption

```css
.caption {
  font-size: var(--fs-caption);
  line-height: var(--lh-caption);
  color: var(--text-secondary);
}
```

### Eyebrow

```css
.eyebrow {
  font-size: var(--fs-eyebrow);
  line-height: var(--lh-caption);
  letter-spacing: var(--ls-eyebrow);
  font-weight: var(--fw-medium);
  text-transform: uppercase;
  color: var(--text-tertiary);
}
```

## Hierarchy by role, not by color

Hierarchy is created by **size + weight + spacing**, not by greying out secondary text.

| Role | Size | Weight | Color |
|---|---|---|---|
| Display XL | 56–96 px | 600 | text-primary |
| Display L | 40–64 px | 600 | text-primary |
| Title | 26–36 px | 600 | text-primary |
| Body lead | 18–24 px | 400 | text-secondary |
| Body | 17 px | 400 | text-primary |
| Caption | 13 px | 500 | text-secondary |
| Eyebrow | 12 px | 500 (uppercase) | text-tertiary |

text-secondary is around 60% of the contrast to text-primary, not 30%.

## Responsive rules

- Body never below 16 px on mobile (Apple keeps 17 px).
- Display scales via `clamp()` for fluid responsiveness.
- On mobile, increase line-height slightly (1.5 → 1.55) for body.
- Avoid fluid scaling for body text — clamp body from 17 px to 18 px max, then leave it.

## CJK and mixed-script content

For Chinese / Japanese / Korean typography, **do not** directly apply Latin rules (negative tracking, tight line-height, weight 600 across the board). See `cjk-typography.md` for the full guidance, including PingFang SC / Source Han Sans / Noto Sans CJK fallbacks, mixed-script spacing, and line-length rules.

## Number formatting

- Use `font-variant-numeric: tabular-nums` for tables, comparisons, prices.
- Use `font-feature-settings: "tnum"` for older browsers.

## What NOT to do

- Set body to 14 px "to fit more content".
- Use `letter-spacing: 0` on a 96 px display headline.
- Use `text-transform: uppercase` on body text.
- Center long-form prose.
- Use color alone to indicate hierarchy.
- Use a different font family per section (creates typographic chaos).
- Apply English rules to Chinese / Japanese / Korean text. See `cjk-typography.md`.

## Accessibility

- Body text 17 px or larger (Apple's standard, also WCAG-friendly).
- Sufficient contrast: text-primary on surface must clear 7:1 for AAA, 4.5:1 for AA.
- Don't use light-weight text (300 or below) for body — readability suffers.
- Don't disable user font scaling. Use `rem` or `clamp()`, not `px` alone for text.
