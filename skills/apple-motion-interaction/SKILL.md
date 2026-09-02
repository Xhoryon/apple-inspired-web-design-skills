---
name: apple-motion-interaction
description: Use whenever the page or component has state changes, transitions, hover/focus feedback, sheet/popover/modal open-close, navigation transitions, scroll-driven effects, or any animation. Defines Apple's motion language: spring with continuity, spatial transitions, restrained scale/opacity feedback, why-the-thing-moved-everywhere motion, and reduced-motion fallback. NOT for marketing video, character animation, or 3D scene choreography (use a different skill).
version: "1.1.0"
license: MIT
---

# apple-motion-interaction

Motion in an Apple-inspired interface is **explanation, not decoration**. Every animation answers "where did this come from, where is it going, and how does it relate to what I just did?" This skill defines the motion vocabulary.

## Core principle

> Motion must explain. If you cannot say in one sentence what the animation communicates, remove it.

This single principle eliminates ~80% of bad motion in Apple-inspired builds.

## The motion grammar

Apple's motion language is built from a small set of primitives used in disciplined ways.

### Spring (the default curve)

Apple platforms use spring physics, not Bézier curves, for most UI transitions. On the web, approximate with critically-damped springs or smooth Béziers tuned to feel spring-like.

Preferred web approximation (CSS / JS):

```css
:root {
  --ease-spring: cubic-bezier(0.32, 0.72, 0.0, 1.0);   /* approach, settle */
  --ease-out-quint: cubic-bezier(0.22, 1, 0.36, 1);   /* exit */
  --ease-in-quint: cubic-bezier(0.64, 0, 0.78, 0);    /* enter (rare) */
  --dur-quick: 180ms;
  --dur-base: 280ms;
  --dur-slow: 420ms;
}
```

For real spring physics on the web, use a small spring utility (Framer Motion, React Spring, Motion One, or vanilla):

```js
// critically-damped, ~280ms feel
const spring = { type: "spring", stiffness: 320, damping: 32, mass: 1 };
```

### What uses what

| Interaction | Curve | Duration |
|---|---|---|
| Hover state | ease-out | 120–180 ms |
| Press feedback | spring / ease-out | 80–160 ms |
| Toggle (segmented, switch) | spring | 240–360 ms |
| Popover / tooltip appear | spring | 220–320 ms |
| Sheet / drawer open | spring | 360–480 ms |
| Sheet / drawer close | ease-in-quint | 240–320 ms |
| Page transition | spring | 320–480 ms |
| Material morph | spring (overshoot ~3%) | 320–420 ms |

### Continuity

State changes should feel **continuous** — the next state begins where the previous state ended.

- A button press scales down to 0.97, then springs back to 1.0 on release.
- A popover grows from the trigger element's position, not from `(0, 0)` or center-screen.
- A sheet slides up from the bottom edge, not fades in.
- A toggle's thumb travels from off to on, not snaps.

Tools for continuity:

- **View Transitions API** for cross-route transitions.
- **FLIP** (First, Last, Invert, Play) for shared-element transitions.
- **Layout animations** (Framer Motion `layout`, etc.) for elements that resize or move.

### Spatial transition

When something appears, it should **come from somewhere**. When something disappears, it should **go somewhere**.

- A popover comes from the button that triggered it (scale + position).
- A sheet comes from the bottom edge.
- A toolbar appears from above the keyboard or from a hidden state.
- A toast comes from a corner.
- A modal comes from the focused element (or center, only as a last resort).

Anti-pattern: every element fades in from `opacity: 0; translateY(20px)` regardless of where it came from.

### Scale, opacity, transform

Animate **transform** and **opacity** only. Never animate `width`, `height`, `top`, `left`, `margin`, `padding`, or `box-shadow` on every frame — they trigger layout / paint and feel janky.

```css
/* Good: animates compositor-only properties */
.card { transition: transform 280ms var(--ease-spring), opacity 200ms ease-out; }
.card:hover { transform: translateY(-2px); }

/* Avoid: animates layout, jank on lower-end devices */
.card { transition: width 280ms, height 280ms, top 280ms; }
```

For real Liquid Glass surface changes (Level 2+), pointer-driven specular response and morphing transitions are allowed to animate non-compositor properties, but they must be **rAF throttled** and **paused on idle**.

### Press feedback

Every interactive element responds to press:

- Buttons: scale to 0.97–0.98 on `:active`, spring back on release.
- Cards: subtle elevation / shadow on hover (only when the card is the click target).
- Icon buttons: opacity 0.7 → 1.0 on hover, no scale jump.

### Hover feedback (desktop only)

- Text links: subtle color shift, optionally 1 px underline that grows from left.
- Buttons: subtle elevation / brightness shift, not a color swap.
- Cards: 2 px translateY, optional shadow lift. No "card grows" hover.
- Never animate the entire layout on hover.

### Scroll reveal

- Reveal-on-scroll is allowed but **restrained**.
- Maximum scroll-driven distance: 24 px translateY.
- Reveal one element per section, not every paragraph.
- Use `IntersectionObserver` with `once: true`; never tie reveal to scroll position continuously for many elements.
- A pinned scroll sequence is allowed **once per page** (see apple-web-composition).

### Material morphing

Apple's Liquid Glass surfaces morph between states (segmented control selection, sheet detents, search expansion). On the web:

- Use spring animations for morph.
- Use `clip-path` or `mask` for shapes that change.
- Use the View Transitions API where available.
- The morph should carry meaning — not animate for the sake of motion.

## What motion is NOT

- Every section fading up 20 px on scroll.
- Big parallax on every image.
- Bouncing / overshoot on every transition.
- Motion that lasts longer than 500 ms for routine UI state changes.
- Reveal-on-scroll for body text.
- Continuous scroll-driven animation on multiple elements simultaneously.
- Color flashes / blink effects.
- Auto-playing video that the user can't pause.

## Reduced motion

`prefers-reduced-motion: reduce` is **mandatory**. Reduce, don't remove.

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

But also:

- Disable pointer-driven specular response.
- Disable parallax.
- Disable pinned storytelling (replace with a static layout that fits the viewport).
- Keep press feedback (instant scale, not animated).
- Keep focus rings (instant appearance).

Some users have `prefers-reduced-motion: no-preference` but are on a low-end device. Use the same simplifications when `Save-Data` is set or `navigator.hardwareConcurrency < 4`.

## Hard rules

- No animation runs longer than 500 ms for routine UI state changes.
- Animate `transform` and `opacity` only (with Liquid Glass surface exceptions).
- Every state change is continuous, not a snap.
- Every appearance comes from somewhere spatial; every disappearance goes somewhere.
- No more than one pinned scroll sequence per page.
- No element does the "fade-up 20 px" reveal if it isn't part of a deliberate reveal sequence.
- `prefers-reduced-motion` is non-negotiable.
- No layout-thrashing animations (`width`, `height`, `top`, `left`, `margin`, `padding`).
- No color flashes / blink effects.

## Review checklist

Before declaring motion done:

- [ ] All transitions use spring or carefully tuned ease curves.
- [ ] All transitions ≤ 500 ms for routine state changes.
- [ ] Animations target only `transform`, `opacity`, and approved properties.
- [ ] State changes are continuous (no snap).
- [ ] Popovers / sheets / modals come from a spatial origin, not center-screen.
- [ ] Press feedback exists on all interactive elements.
- [ ] Hover feedback is subtle and bounded.
- [ ] Reveal-on-scroll is restrained (≤ 24 px, max once per section).
- [ ] At most one pinned scroll sequence on the page.
- [ ] `prefers-reduced-motion` disables parallax, pinned sequences, morphing, and continuous animation.
- [ ] No layout-thrashing animations.
- [ ] Tested on a low-end mobile profile.

## Common mistakes

- Fade-up 20 px on every element that enters the viewport.
- 600 ms ease-in-out on every transition.
- All transitions snap (no spring).
- Popovers fading in from center instead of growing from the trigger.
- Parallax on every section.
- Pinned storytelling on multiple sections.
- Press feedback missing — buttons feel "dead".
- Animation that runs forever (auto-playing video, infinite shimmer).
- Reduced-motion fallback that just removes animation entirely, breaking the state change clarity.

## Companion references

- `references/spring-tokens.md` — full spring token library for CSS/JS.
- `references/reduced-motion.md` — complete reduced-motion fallback rules.
- `examples/press-feedback.css` — drop-in press/hover feedback styles.

For pinned scroll sequences, see `apple-web-composition/references/scroll-storytelling.md`.

This skill works with apple-ui-components (button/card press feedback) and apple-liquid-glass-web (morph + specular). Composition chooses what animates; motion defines how.
