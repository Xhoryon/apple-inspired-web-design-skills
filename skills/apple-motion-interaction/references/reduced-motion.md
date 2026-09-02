# Reduced Motion — Full Reference

Mandatory handling for `prefers-reduced-motion: reduce` and low-end devices.

## Core stance

> Reduce, don't remove. State changes still happen. Information still appears. The user just gets there without motion.

A naive reduced-motion CSS resets every transition to `0.01ms`. That technically respects the preference but visually is the same as a snap, which can feel jarring. Better approach:

- Snap transitions: instant, but the change still happens.
- Continuous animations: off.
- Pinned storytelling: replaced with a static layout.
- Pointer-driven specular: off.
- Parallax: off.

## The CSS reset (the floor)

```css
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

This is the safety net. It catches everything. Now add targeted handling for the things the blanket reset breaks.

## Targeted overrides

### Continuous animation — off

```css
@media (prefers-reduced-motion: reduce) {
  .specular,
  .parallax,
  .shimmer,
  .pulse {
    animation: none !important;
  }
}
```

### Pinned storytelling — collapse

```css
@media (prefers-reduced-motion: reduce) {
  .sticky-stage {
    position: static !important;
    height: auto !important;
  }
  .sticky-stage .step {
    position: static !important;
    opacity: 1 !important;
    margin-bottom: 16px;
  }
}
```

### Reveal-on-scroll — already done

The blanket reset of `animation-duration` covers IntersectionObserver-triggered CSS animations.

### Pointer-driven specular — off (JS)

```js
const reduceMotion = window.matchMedia("(prefers-reduced-motion: reduce)").matches;
if (!reduceMotion) {
  surface.addEventListener("pointermove", throttledUpdate);
}
```

### Reduced transparency — off (JS, for canvas / shader)

```js
const reduceTransparency = window.matchMedia("(prefers-reduced-transparency: reduce)").matches;
if (reduceTransparency) {
  surface.style.backdropFilter = "none";
  surface.style.webkitBackdropFilter = "none";
}
```

## What to keep

- **State changes.** When a user clicks a button, the button should still react. Press feedback (instant scale, no transition) is fine.
- **Focus rings.** Instant appearance, no fade.
- **Pointer cursor change.** Switching from `cursor: default` to `cursor: pointer` on hover is a state change, not motion.
- **Form input feedback.** Validation messages should appear (instantly, no fade).
- **Toast / inline notification.** Appears instantly, stays visible, can be dismissed.

## What to remove

- Reveal-on-scroll for non-essential elements (keep only critical above-the-fold reveals if any).
- Pinning (sticky scroll-driven storytelling).
- Parallax on hero images.
- Continuous specular highlight.
- Spring / overshoot on transitions.
- Continuous scroll-jacking.
- Auto-playing video that can't be paused.
- Marquee / infinite-scroll animation.

## Detection at runtime

```js
const reduceMotion = window.matchMedia("(prefers-reduced-motion: reduce)").matches;
const reduceTransparency = window.matchMedia("(prefers-reduced-transparency: reduce)").matches;
const saveData = navigator.connection?.saveData ?? false;
const lowPowerCPU = navigator.hardwareConcurrency < 4;

const simplifyMotion = reduceMotion || saveData || lowPowerCPU;
const simplifyTransparency = reduceTransparency || saveData || lowPowerCPU;
```

When `simplifyMotion === true`, use the static versions of every continuous animation.

When `simplifyTransparency === true`, switch glass surfaces to solid (Level 0).

## Reduced transparency

`prefers-reduced-transparency` is separate from reduced motion. Users who request reduced transparency want solid backgrounds (no glass, no translucency).

```css
@media (prefers-reduced-transparency: reduce) {
  .glass,
  .glass-level-2,
  .glass-level-3 {
    background: var(--surface-solid) !important;
    backdrop-filter: none !important;
    -webkit-backdrop-filter: none !important;
  }
}
```

For shader glass, disable the canvas and show a static solid surface.

## Forced colors (Windows High Contrast)

```css
@media (forced-colors: active) {
  .glass {
    background: Canvas;
    color: CanvasText;
    border: 1px solid CanvasText;
  }
}
```

Test in Windows High Contrast mode if your audience uses Windows.

## Common mistakes

- Removing all motion entirely (state changes feel broken).
- Forgetting `prefers-reduced-transparency` (glass becomes opaque to some users but still on others).
- Not testing on actual low-end devices (a 4-core CPU laptop still feels fast; a 2-core phone with `Save-Data` is the real test).
- Removing parallax but leaving pinned storytelling on (still continuous motion).
- Allowing auto-playing video to keep playing under reduced motion.

## Accessibility checklist

- [ ] `prefers-reduced-motion: reduce` respected in CSS.
- [ ] `prefers-reduced-transparency: reduce` switches glass to solid.
- [ ] Pinned storytelling collapses to a static stack.
- [ ] Pointer-driven specular is off.
- [ ] Parallax is off.
- [ ] Continuous animations are paused.
- [ ] State changes still happen, just instantly.
- [ ] Focus rings remain visible.
- [ ] Forced-colors mode renders surfaces with system colors.
- [ ] Tested with `Save-Data: 1` simulation.
