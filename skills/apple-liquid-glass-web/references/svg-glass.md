# SVG Glass — Level 2 Recipes

Level 2 adds **refraction** (subtle pixel displacement) and **pointer-driven specular response** (a moving highlight that follows the cursor). Still CSS-and-SVG only; no WebGL.

## When to use

- Hero centerpiece floating controls (e.g. a search bar on a hero image).
- A premium command palette.
- A primary sheet or popover that needs to feel like a "real" glass surface.

Don't use on every navbar. Reach for Level 1 there.

## Components

1. **Refraction** — SVG `feTurbulence` + `feDisplacementMap` applied to a snapshot of the backdrop.
2. **Specular response** — pointer-tracked radial gradient highlight, rAF-throttled.
3. **Edge tint** — slight white at top edge, slight dark at bottom edge.

---

## Refraction — capturing and displacing the backdrop

For real refraction, you need pixels from behind the surface. There are two web approaches:

### Approach 1: backdrop-filter + SVG displacement filter (visual approximation)

The browser's `backdrop-filter` provides the blurred backdrop. We then **layer** an SVG that adds a displacement-map-derived distortion. This isn't physically accurate, but it adds the visual cue of refraction.

```html
<svg width="0" height="0" style="position:absolute" aria-hidden="true">
  <defs>
    <filter id="glass-refract" x="-10%" y="-10%" width="120%" height="120%">
      <feTurbulence type="fractalNoise" baseFrequency="0.9 1.2" numOctaves="2" seed="3" result="noise"/>
      <feDisplacementMap in="SourceGraphic" in2="noise" scale="3"/>
    </filter>
  </defs>
</svg>

<div class="glass glass-level-2" style="filter: url(#glass-refract);">
  <!-- surface content -->
</div>
```

Constraints:

- The displacement scale of 3 is intentionally small; visible refraction should be barely perceptible, not gimmicky.
- This filter runs on the GPU. Don't apply it to many elements.
- Test in Safari — `feDisplacementMap` has historically been slow on Safari.

### Approach 2: Capture backdrop with html2canvas and sample

For accurate refraction, you can capture the area behind the surface, sample its pixels, and warp them in a canvas. This is heavy and rare; only use if the visual effect is the centerpiece.

Pseudo-flow:

```
1. On surface mount, capture rect of background into an offscreen canvas.
2. For each pointermove, recompute a small displacement field (radial from pointer).
3. Draw warped canvas to the surface's backdrop layer.
4. rAF-throttle; idle after 200 ms of no pointer movement.
```

See `shader-glass.md` for the shader version of the same approach.

---

## Specular response — pointer-tracked highlight

A real glass surface catches light. A static highlight gradient looks decorative. Wire the highlight to the pointer position.

### CSS variable approach

```css
.glass-level-2 {
  --mx: 50%;
  --my: 30%;
  --specular-opacity: 0;

  position: relative;
  background:
    radial-gradient(
      circle at var(--mx) var(--my),
      rgba(255, 255, 255, 0.35),
      transparent 35%
    ),
    linear-gradient(
      180deg,
      rgba(255, 255, 255, 0.55),
      rgba(255, 255, 255, 0.35)
    );
  border: 1px solid rgba(255, 255, 255, 0.40);
  backdrop-filter: saturate(180%) blur(20px);
  transition: --specular-opacity 200ms ease-out;
}

.glass-level-2:hover { --specular-opacity: 1; }
```

### JS to wire pointer to vars (rAF throttled, idle-aware)

```js
const surface = document.querySelector(".glass-level-2");
let raf = 0;
let lastMove = 0;
let idleTimer = 0;

const update = (e) => {
  const rect = surface.getBoundingClientRect();
  const x = ((e.clientX - rect.left) / rect.width) * 100;
  const y = ((e.clientY - rect.top) / rect.height) * 100;
  surface.style.setProperty("--mx", `${x}%`);
  surface.style.setProperty("--my", `${y}%`);
  lastMove = performance.now();
  // Hide specular when pointer has been still
  clearTimeout(idleTimer);
  idleTimer = setTimeout(() => {
    surface.style.setProperty("--mx", "50%");
    surface.style.setProperty("--my", "30%");
  }, 200);
};

surface.addEventListener("pointermove", (e) => {
  if (raf) return;
  raf = requestAnimationFrame(() => {
    raf = 0;
    update(e);
  });
});
```

Notes:

- `pointermove` (not `mousemove`) handles touch and pen.
- rAF throttle: at most one update per frame.
- Idle after 200 ms: highlight settles to default position.
- Reduced motion: skip the JS entirely; use a static highlight or none.

---

## Edge tint — subtle white at top, dark at bottom

Real glass catches light more at the top edge. Add a 1 px inset highlight:

```css
.glass-level-2 {
  box-shadow:
    inset 0 1px 0 rgba(255, 255, 255, 0.50),
    inset 0 -1px 0 rgba(0, 0, 0, 0.08),
    0 10px 30px rgba(0, 0, 0, 0.12);
}
```

For dark mode:

```css
@media (prefers-color-scheme: dark) {
  .glass-level-2 {
    box-shadow:
      inset 0 1px 0 rgba(255, 255, 255, 0.10),
      inset 0 -1px 0 rgba(0, 0, 0, 0.40),
      0 10px 30px rgba(0, 0, 0, 0.50);
  }
}
```

---

## Adaptivity — light/dark/image response

Level 2 surfaces should adapt to what's behind them. Three approaches:

### A. CSS media query (light/dark mode only)

Already covered in `css-glass.md`. Simple and effective.

### B. Surface-level theme via parent class

```html
<section class="hero-image">
  <div class="glass-level-2">...</div>
</section>
```

```css
.hero-image .glass-level-2 {
  background:
    radial-gradient(...),
    rgba(20, 20, 22, 0.55);  /* darker fill over image */
  color: #fff;
}
```

### C. Dynamic theme via JS

Detect the average luminance behind the surface and switch variants. Practical for hero surfaces only.

```js
// pseudo
const luminance = sampleLuminanceBehind(surface);
if (luminance < 0.5) surface.classList.add("on-dark");
else surface.classList.add("on-light");
```

---

## Morphing — segmented control selection

A segmented control's selected pill **morphs** between positions, not snaps.

### Implementation outline

```js
const segs = document.querySelectorAll(".segmented button");
const indicator = document.querySelector(".segmented .indicator");

segs.forEach((seg) => {
  seg.addEventListener("click", () => {
    const rect = seg.getBoundingClientRect();
    const parent = seg.parentElement.getBoundingClientRect();
    indicator.style.transform = `translate(${rect.left - parent.left}px, 0)`;
    indicator.style.width = `${rect.width}px`;
  });
});
```

CSS:

```css
.segmented {
  position: relative;
  display: inline-flex;
  background: rgba(0, 0, 0, 0.05);
  border-radius: 9999px;
  padding: 2px;
}

.segmented .indicator {
  position: absolute;
  top: 2px;
  left: 0;
  height: calc(100% - 4px);
  background: rgba(255, 255, 255, 0.95);
  border-radius: 9999px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.10);
  transition: transform 280ms var(--ease-spring), width 280ms var(--ease-spring);
}

.segmented button {
  position: relative;
  z-index: 1;
  padding: 6px 14px;
  background: transparent;
  border: 0;
}
```

Reduced motion: use a 1 ms transition. Indicator still moves, but instantly.

---

## What this recipe is NOT

Level 2 is still an **approximation** of Liquid Glass. Real refraction needs pixel-level warping (canvas or shader). Real specular response on Apple platforms uses metal/compositor lighting. On the web, the visual cues (slight displacement, pointer-tracked highlight, edge tint) get you 80% of the perceived quality with 20% of the cost.

---

## Performance budget (Level 2)

- Persistent Level 2 surfaces per page: **1**.
- `feDisplacementMap` runs on GPU; cap scale ≤ 4 px.
- Pointer handler: rAF throttled.
- Idle after 200 ms; pause entirely when surface is off-screen or `visibilitychange === 'hidden'`.
- Provide Level 1 fallback if Level 2 isn't supported or `prefers-reduced-transparency`.

## Accessibility

- `prefers-reduced-transparency: reduce` → Level 1 / Level 0.
- `prefers-reduced-motion: reduce` → no morph, no pointer specular.
- Focus rings must remain visible.
- Don't make the entire surface pointer-driven; only the highlight follows the pointer.
