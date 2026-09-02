# Liquid Glass Performance Reference

A consolidated perf guide for Liquid Glass surfaces. The headline: glass costs GPU. Every persistent `backdrop-filter` is a real-time compositor operation.

## Per-instance cost

| Level | Per-instance cost | Recommended max per page |
|-------|-------------------|----------------------------|
| 0 (no blur) | None | Unlimited |
| 1 (CSS blur) | GPU, scales with surface area | 3 |
| 2 (CSS blur + SVG displacement) | GPU, expensive | 1 |
| 3 (WebGL/WebGPU shader) | GPU + main thread | 1 |

A 1440×900 floating navbar at Level 1 with `blur(20px)` costs more than a 200×44 navbar at the same blur — backdrop-filter cost scales with pixel area, not surface count.

## Blur radius vs. cost

Blur cost is roughly linear in radius up to ~30 px and super-linear above:

- 8 px: cheap
- 16 px: moderate
- 24 px: noticeable
- 32 px: heavy
- 40+ px: very heavy, may drop frames on low-end devices

Recommended maximum: 24 px desktop, 18 px mobile.

## Mobile considerations

Mobile GPUs are roughly 1/10th the throughput of desktop. Rules:

- Reduce blur radius to 12–18 px.
- Reduce saturation filter (or skip it).
- Cap persistent surfaces at 1–2 instead of 3.
- Provide a solid-only fallback for low-end devices.

Detection:

```js
const isLowEnd =
  navigator.hardwareConcurrency < 4 ||
  navigator.deviceMemory < 4 ||
  navigator.connection?.saveData === true ||
  /Android.*Mobile/.test(navigator.userAgent) === false && /iPhone/.test(navigator.userAgent) === false;
```

When `isLowEnd === true`, render Level 0 only.

## `saturate()` and `contrast()` cost

`backdrop-filter: blur()` is the main cost. Adding `saturate(180%)` adds a small extra cost. `contrast()` and `hue-rotate()` are more expensive. Use sparingly:

- `blur()` — main effect.
- `saturate(150–200%)` — light additional cost.
- `contrast()` — expensive.
- `hue-rotate()` — expensive.

For most surfaces, `blur() saturate(180%)` is enough.

## When to skip blur

- Mobile devices with `Save-Data` enabled.
- Low CPU cores.
- Battery saver mode.
- `prefers-reduced-transparency: reduce`.

```js
const canBlur = !(
  navigator.connection?.saveData ||
  navigator.hardwareConcurrency < 4 ||
  window.matchMedia("(prefers-reduced-transparency: reduce)").matches
);

if (canBlur) {
  surface.classList.add("glass-level-1");
} else {
  surface.classList.add("glass-level-0");
}
```

## Pause and resume

Continuous animation (pointer specular, parallax) must pause when:

- The page is not visible (`document.hidden`).
- The surface is not in the viewport.
- The user has `prefers-reduced-motion: reduce`.
- Battery saver is active.

```js
document.addEventListener("visibilitychange", () => {
  if (document.hidden) stopAnimations();
  else startAnimations();
});

const io = new IntersectionObserver((entries) => {
  entries.forEach((e) => {
    if (e.isIntersecting) startAnimations();
    else stopAnimations();
  });
});
io.observe(surface);
```

## Profiling

```js
// Measure frame rate
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

If FPS drops below 50 during scroll on a mid-tier mobile profile, glass is the likely culprit.

## Chrome DevTools

- **Layers** panel → see the compositor layers created by `backdrop-filter`.
- **Performance** panel → record a scroll, look for "compositing" costs.
- **Rendering** panel → "Highlight layer borders" and "Highlight composited layers" show which elements create layers.

## Common perf mistakes

- 5+ persistent `backdrop-filter` surfaces.
- Blur radius ≥ 30 px.
- Shader glass on more than one surface.
- Continuous specular animation that doesn't idle.
- Pointer handler that runs on every `pointermove` without rAF throttle.
- Animating `backdrop-filter` itself (e.g. changing blur radius) — extremely expensive.

## Accessibility

- `prefers-reduced-transparency: reduce` → solid surface, no blur.
- `prefers-reduced-motion: reduce` → no continuous animation.
- Don't make the surface pointer-driven for users on low-end devices — use the static highlight instead.
