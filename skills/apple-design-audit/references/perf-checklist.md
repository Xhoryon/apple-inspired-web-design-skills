# Performance Checklist Reference

For Apple-inspired builds with Liquid Glass surfaces, performance is a feature. Builds that fail perf fail the audit.

## Lighthouse mobile targets

| Metric | Target | Failing |
|--------|--------|---------|
| Performance | ≥ 80 | < 70 |
| LCP | < 2.5 s | > 4 s |
| CLS | < 0.1 | > 0.25 |
| TBT | < 200 ms | > 600 ms |

Run with: `lighthouse <url> --preset=mobile --output=json --quiet`

## Backdrop-filter budget

| Level | Persistent surfaces | Blur radius (desktop) | Blur radius (mobile) |
|-------|---------------------|------------------------|----------------------|
| 0     | 0                   | 0                      | 0                    |
| 1     | ≤ 3                 | 16–24 px               | 12–18 px             |
| 2     | ≤ 1                 | 20–28 px               | 14–20 px             |
| 3     | ≤ 1                 | shader                 | shader (downsampled) |

A page with 5+ persistent `backdrop-filter` surfaces fails the audit.

## Shader glass budget

- One shader-glass surface per page max.
- Downsample framebuffer to ≤ 0.6× device pixel ratio.
- Pause on `visibilitychange`, `Save-Data`, low battery, low CPU.
- Use `powerPreference: "low-power"` when getting the WebGL context on low-end devices.
- Always provide a Level 1 fallback.

## Animation budget

- Continuous animations per page: ≤ 3.
- Pointer-driven specular: rAF throttled, idle after 200 ms.
- Reveal-on-scroll elements: ≤ 6 per page (use `IntersectionObserver` with `once: true`).
- Pinned storytelling: ≤ 1 per page.
- Parallax: ≤ 1 (hero only).

## Image budget

- Hero images: AVIF or WebP, ≤ 200 KB at the displayed size.
- Below-fold images: lazy-load with `loading="lazy"`.
- Use `srcset` and `sizes` to serve smaller images on mobile.
- Provide width and height attributes to prevent CLS.

## Font budget

- Self-host fonts or use system stack.
- If self-hosting, use `font-display: swap` or `font-display: optional`.
- Limit to 2–3 font files (one regular, one semibold, optionally italic).
- Subset fonts if non-Latin is not needed.

## CSS budget

- Total CSS ≤ 50 KB (gzipped).
- Avoid CSS-in-JS that ships runtime cost.
- Use CSS variables for theming (not runtime style updates).

## JS budget

- Total JS ≤ 200 KB (gzipped) for landing pages.
- Defer non-critical scripts.
- Use `requestIdleCallback` for non-critical initialization.
- Avoid layout-thrashing animations.

## Manual perf checks

- [ ] Lighthouse mobile Performance ≥ 80.
- [ ] LCP < 2.5 s on simulated 4G.
- [ ] CLS < 0.1.
- [ ] TBT < 200 ms.
- [ ] No more than 3 persistent backdrop-filter surfaces.
- [ ] No layout-thrashing animations.
- [ ] Animations pause on tab hidden.
- [ ] Reduced-motion respected.
- [ ] Reduced-transparency respected.
- [ ] Tested on a low-end mobile profile.

## Profiling recipe

```js
// Measure frame rate during scroll
let frames = 0;
let lastTime = performance.now();

const measure = () => {
  frames++;
  const now = performance.now();
  if (now - lastTime >= 1000) {
    console.log(`FPS: ${frames}`);
    frames = 0;
    lastTime = now;
  }
  requestAnimationFrame(measure);
};

requestAnimationFrame(measure);
```

If FPS drops below 50 during scroll on a mid-tier mobile profile, the page has perf issues.

## Common perf mistakes

- 5+ persistent backdrop-filter surfaces.
- Blur radius ≥ 30 px.
- Shader glass on multiple surfaces.
- Continuous animation on every section.
- Reveal-on-scroll on every paragraph.
- Lazy-loading not used for below-fold images.
- Self-hosted fonts without `font-display: swap`.
- Layout-thrashing animations (animating `width`, `height`, `top`, `left`).
- Auto-playing video on page load.
