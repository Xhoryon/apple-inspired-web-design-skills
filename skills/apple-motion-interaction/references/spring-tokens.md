# Motion Tokens — Spring and Easing Reference

Extended recipes for Apple-inspired motion. Builds on `apple-motion-interaction/SKILL.md`.

## CSS variables — start here

```css
:root {
  /* Easing curves */
  --ease-spring: cubic-bezier(0.32, 0.72, 0.0, 1.0);     /* approach, settle */
  --ease-out-quint: cubic-bezier(0.22, 1, 0.36, 1);
  --ease-in-quint: cubic-bezier(0.64, 0, 0.78, 0);
  --ease-in-out-quart: cubic-bezier(0.76, 0, 0.24, 1);

  /* Durations */
  --dur-instant: 100ms;
  --dur-quick: 180ms;
  --dur-base: 280ms;
  --dur-slow: 420ms;
  --dur-deliberate: 600ms;
}
```

## JS spring config

For libraries that support spring physics (Framer Motion, React Spring, Motion One, GSAP, etc.):

```js
// Critical-damped spring — Apple-like feel
const springSmooth = { type: "spring", stiffness: 320, damping: 32, mass: 1 };
// Slightly bouncier — for material morph
const springBouncy = { type: "spring", stiffness: 380, damping: 28, mass: 0.8 };
// Quick feedback — for press
const springQuick = { type: "spring", stiffness: 600, damping: 30, mass: 0.7 };
```

## Duration guidance

| Use case | Duration | Curve |
|---|---|---|
| Hover state | 120–180 ms | ease-out |
| Press feedback (scale) | 80–160 ms | spring |
| Toggle / segmented morph | 240–360 ms | spring |
| Popover open | 220–320 ms | spring |
| Popover close | 180–240 ms | ease-in-quint |
| Sheet open | 360–480 ms | spring |
| Sheet close | 240–320 ms | ease-in-quint |
| Page transition | 320–480 ms | spring |
| Material morph | 320–420 ms | spring (overshoot ~3%) |
| Hero parallax | 600–900 ms (slow!) | ease-out |
| Scroll reveal | 280–420 ms | ease-out |

If a routine UI state change lasts longer than 500 ms, it feels sluggish. Don't do it.

## What to animate

**Animate:**

- `transform` (translate, scale, rotate) — compositor only.
- `opacity` — compositor only.
- `clip-path` — compositor only (most browsers).
- `filter` (some properties) — compositor with care.

**Avoid animating:**

- `width`, `height`, `top`, `left`, `right`, `bottom`, `margin`, `padding` — triggers layout.
- `box-shadow`, `border` — triggers paint (except as part of `transition` on hover, where it can be acceptable for buttons).
- `background-position` for parallax — usually slow; use `transform: translate3d` on a positioned child instead.

**Exception (Liquid Glass Level 2+):**

- Pointer-driven specular response may animate `background` (radial gradient position) — must be rAF throttled.
- Shader Glass (Level 3) animates fragment shader uniforms — must be rAF throttled.

## Continuity recipes

### Press feedback

```css
.button {
  transition: transform 120ms var(--ease-spring);
}
.button:active {
  transform: scale(0.97);
}
```

```js
// Framer Motion
<motion.button
  whileTap={{ scale: 0.97 }}
  transition={{ type: "spring", stiffness: 600, damping: 30 }}
>
```

### Popover anchored to trigger

```js
// Framer Motion — popover grows from the trigger
<motion.div
  initial={{ scale: 0.92, opacity: 0 }}
  animate={{ scale: 1, opacity: 1 }}
  exit={{ scale: 0.96, opacity: 0 }}
  transition={{ type: "spring", stiffness: 380, damping: 30 }}
  style={{ originX: 0, originY: 1 }}  // grows from bottom-left
>
```

`originX / originY` are key — they set the transform origin so the popover appears to grow from the trigger, not center-screen.

### Sheet slide-up (mobile)

```js
<motion.div
  initial={{ y: "100%" }}
  animate={{ y: 0 }}
  exit={{ y: "100%" }}
  transition={{ type: "spring", stiffness: 320, damping: 32 }}
>
```

### Segmented control selection morph

```js
// The indicator element morphs between segments
<motion.div
  className="indicator"
  layout
  layoutId="segmented-indicator"  // shared layout for morph
  transition={{ type: "spring", stiffness: 380, damping: 32 }}
/>
```

`layoutId` is Framer Motion's shared-layout pattern — the indicator morphs between positions rather than fades between two separate indicators.

### View Transitions API (cross-route)

```js
// In a router or navigation handler
document.startViewTransition(() => {
  // navigate
});
```

```css
::view-transition-old(root) {
  animation: fadeOut 200ms ease-out;
}
::view-transition-new(root) {
  animation: fadeIn 280ms var(--ease-spring);
}
```

Browser support is growing but not universal. Provide fallback.

### FLIP for shared-element transitions

When you don't have View Transitions or a layout library:

```js
const first = el.getBoundingClientRect();
// ... element moves / resizes ...
const last = el.getBoundingClientRect();
const dx = first.left - last.left;
const dy = first.top - last.top;

el.animate(
  [
    { transform: `translate(${dx}px, ${dy}px)` },
    { transform: "translate(0, 0)" }
  ],
  { duration: 320, easing: "cubic-bezier(0.32, 0.72, 0, 1)" }
);
```

## Scroll-driven motion

### Reveal on scroll (use sparingly)

```js
const io = new IntersectionObserver((entries) => {
  entries.forEach((e) => {
    if (e.isIntersecting) {
      e.target.classList.add("revealed");
      io.unobserve(e.target);  // one-shot
    }
  });
}, { threshold: 0.1 });

document.querySelectorAll("[data-reveal]").forEach((el) => io.observe(el));
```

```css
[data-reveal] {
  opacity: 0;
  transform: translateY(20px);
  transition: opacity 320ms var(--ease-out-quint), transform 320ms var(--ease-out-quint);
}
[data-reveal].revealed {
  opacity: 1;
  transform: translateY(0);
}
```

**Important**: apply to **at most one element per section**. If every paragraph fades up, the page feels exhausting.

### Pinned storytelling (one per page max)

Use a sticky container with internal scroll-driven animation:

```css
.sticky-stage {
  position: sticky;
  top: 0;
  height: 100vh;
  overflow: hidden;
}
.sticky-stage .step {
  position: absolute;
  inset: 0;
  opacity: 0;
  transition: opacity 360ms var(--ease-spring);
}
.sticky-stage .step.is-active {
  opacity: 1;
}
```

Use IntersectionObserver or scroll position to swap `.is-active`. Don't pin more than one section per page.

## Reduced motion

`prefers-reduced-motion: reduce` is **mandatory**.

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

  /* Disable continuous animation */
  .specular-highlight { animation: none !important; }
  .parallax { transform: none !important; }
  .pin-stage { position: static !important; height: auto !important; }
}
```

But keep:

- Press feedback (instant scale, not animated).
- Focus rings (instant appearance).
- State changes (the change itself, just without motion).

Some users have `prefers-reduced-motion: no-preference` but are on low-end devices. Detect via:

```js
const reduceMotion =
  window.matchMedia("(prefers-reduced-motion: reduce)").matches ||
  navigator.hardwareConcurrency < 4 ||
  navigator.connection?.saveData;
```

When `reduceMotion === true`, apply the same simplifications.

## Accessibility

- All animations respect `prefers-reduced-motion`.
- All state changes have a non-animated fallback (so the user always knows the state changed).
- Continuous animations (parallax, specular, pinned storytelling) pause on `visibilitychange` and on battery saver.
- Tap targets ≥ 44×44 px regardless of animation.
