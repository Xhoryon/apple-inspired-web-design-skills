# Web Capability and Fallback Reference

Web feature detection, capability ladder, Glass Quality Governor, and fallback transitions. This file is the Tier 2 reference for `apple-liquid-glass-fidelity`.

The Web has heterogeneous support for the techniques that Liquid Glass uses. The Skill records the principles for detecting what the Web can do and down-shifting gracefully when it cannot.

## Browser support snapshot (2026-09-03)

| Feature | Chromium | Safari | Firefox | Notes |
|---|---|---|---|---|
| `backdrop-filter` | Yes (no prefix) | Yes (`-webkit-` prefix required on older versions) | Yes (no prefix) | Widely supported. |
| `backdrop-filter` blur > 20px | Yes | Yes | Yes | Slower on large surfaces. |
| SVG `feDisplacementMap` | Yes | Historically slow on Safari; check | Yes | Test on Safari before shipping. |
| CSS `light-dark()` | Yes (Chromium 123+) | Yes (Safari 17.5+) | Yes (Firefox 120+) | Recent. |
| `prefers-reduced-transparency` | Yes | Yes | Yes (Firefox 113+) | Supported. |
| `prefers-contrast: more` | Yes | Yes | Yes | Supported. |
| `Save-Data` | Yes | Yes | Yes | Save-Data API. |
| `navigator.deviceMemory` | Yes (Chromium) | No | No | Limited. |
| `navigator.hardwareConcurrency` | Yes | Yes | Yes | Supported. |
| `navigator.connection.saveData` | Yes (Chromium) | Limited | Limited | Best-effort. |
| WebGL 1 / 2 | Yes | Yes | Yes | Widely supported. |
| WebGPU | Limited (Chromium behind flag) | No (Safari announced) | Limited | Not yet broadly available. |
| View Transitions API | Yes (Chromium 111+) | Limited (Safari 18+) | Limited | New. |
| `popover` attribute | Yes (Chromium 114+) | Yes (Safari 17+) | Yes (Firefox 125+) | Recent. |

The implementer must verify on the target browser. The Skill records the principles; the implementer validates.

## Capability ladder

The Skill recommends the **Implementation Capability Level** ladder from the stable v1.1 `apple-liquid-glass-web`:

- **Level 0** — Solid fallback. No glass, no blur, no displacement.
- **Level 1** — CSS glass (`backdrop-filter` + gradient + hairline border + soft shadow). Wide browser support.
- **Level 2** — Enhanced glass (SVG `feDisplacementMap` + pointer-driven specular). Limited surfaces; test on Safari.
- **Level 3** — Optical / shader glass (WebGL / WebGPU). One surface per page; provide Level 1 fallback.

The implementer picks the **lowest level that achieves the design intent**. A well-designed Level 1 is often more appropriate than a theatrical Level 3.

## Glass Quality Governor

The Skill records a conceptual **Glass Quality Governor** that down-shifts the implementation level when conditions warrant. The Governor is not a runtime library; it is a pattern the implementer applies.

### Inputs (signals the implementer may use)

- **CSS feature detection** — `@supports (backdrop-filter: blur(1px))` and similar.
- **Media queries** — `prefers-reduced-transparency`, `prefers-reduced-motion`, `prefers-contrast`, `(hover: hover)`, `(pointer: fine)`.
- **Device capability** — `navigator.hardwareConcurrency`, `navigator.deviceMemory` (Chromium only), `navigator.connection.saveData`.
- **Surface count** — the number of concurrent glass surfaces on the page.
- **Surface size** — the total area of glass surfaces.
- **Interaction frequency** — how often the user interacts with the page (pointer-driven effects may down-shift on high-frequency pages).
- **Power / performance context** — if the page can detect low-power mode or constrained GPU, it may down-shift.

The Skill does not collect invasive device fingerprints. The signals are coarse and user-respecting (preference APIs, standard Web APIs).

### Outputs (down-shift decisions)

The Governor produces one of:

- **Full** — the requested level is supported; ship it.
- **Reduced** — keep the design intent but reduce the optical cost (e.g. Level 2 → Level 1; Level 1 with smaller blur radius).
- **Basic** — Level 0 fallback with a solid surface that still has the design's edge and shadow.
- **No glass** — replace the surface with a solid floating control; the user still gets the interaction layer, just without the material.

### When the Governor down-shifts

- The browser does not support `backdrop-filter` → Level 0.
- `prefers-reduced-transparency: reduce` → Level 0 or a more opaque Level 1.
- `prefers-reduced-motion: reduce` → instant state change; no morph; no specular animation.
- `prefers-contrast: more` → more opaque Level 1; scrim layer.
- `Save-Data: true` → Level 0 or Level 1.
- `navigator.hardwareConcurrency < 4` → Level 0 or Level 1 (skip Level 2+).
- More than 3 concurrent glass surfaces → reduce to Level 1 or Level 0 for the new ones.
- A fullscreen glass surface → Level 0; fullscreen glass is too expensive.
- WebGL fails or the device reports constrained GPU → Level 0 or Level 1.

## Feature detection patterns

### CSS

```css
/* Level 0 fallback when backdrop-filter is not supported */
@supports not ((backdrop-filter: blur(1px)) or (-webkit-backdrop-filter: blur(1px))) {
  .glass {
    background: var(--surface-floating-light);
  }
}

/* Level 0 fallback when reduced transparency is requested */
@media (prefers-reduced-transparency: reduce) {
  .glass {
    background: var(--surface-floating-light);
    backdrop-filter: none;
    -webkit-backdrop-filter: none;
  }
}

/* Increase contrast for prefers-contrast */
@media (prefers-contrast: more) {
  .glass {
    background: var(--surface-floating-opaque);
    border-color: var(--hairline-strong);
  }
}
```

### JavaScript (feature detection)

```js
function detectGlassCapability() {
  const supportsBackdrop = CSS.supports('backdrop-filter', 'blur(1px)') ||
    CSS.supports('-webkit-backdrop-filter', 'blur(1px)');
  const reducedTransparency = window.matchMedia('(prefers-reduced-transparency: reduce)').matches;
  const reducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;
  const moreContrast = window.matchMedia('(prefers-contrast: more)').matches;
  const saveData = navigator.connection?.saveData ?? false;
  const cores = navigator.hardwareConcurrency ?? 4;

  if (!supportsBackdrop) return 'no-glass';
  if (reducedTransparency) return 'basic';
  if (reducedMotion) return 'reduced';
  if (moreContrast) return 'reduced';
  if (saveData) return 'basic';
  if (cores < 4) return 'reduced';

  return 'full';
}
```

The implementer applies the Governor output as a class on the root element:

```js
document.documentElement.classList.add(`glass-${detectGlassCapability()}`);
```

```css
.glass-full .glass {
  backdrop-filter: blur(20px) saturate(180%);
  -webkit-backdrop-filter: blur(20px) saturate(180%);
}
.glass-reduced .glass {
  backdrop-filter: blur(8px) saturate(140%);
  -webkit-backdrop-filter: blur(8px) saturate(140%);
}
.glass-basic .glass {
  background: rgba(255, 255, 255, 0.92);
  border: 1px solid rgba(0, 0, 0, 0.10);
  backdrop-filter: none;
  -webkit-backdrop-filter: none;
}
.glass-no-glass .glass {
  background: rgba(255, 255, 255, 0.95);
  border: 1px solid rgba(0, 0, 0, 0.12);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
  backdrop-filter: none;
  -webkit-backdrop-filter: none;
}
```

## Fallback transitions

When the Governor down-shifts mid-page (e.g. the user toggles `prefers-reduced-transparency`), the transition should be smooth:

- The surface's visual identity is preserved (same shape, same content, same interaction).
- The optical cost is reduced (less blur, less displacement, less specular animation).
- The transition is brief (≤ 200 ms); no morph.

The implementer may use CSS `@media` queries to update the surface automatically; the JavaScript `matchMedia` listener is not required.

## What the Skill rejects

- Shipping glass without a Level 0 fallback.
- Using WebGL on every surface (Shader Supremacy).
- Using WebGL on a fullscreen surface.
- Hiding the down-shift decision from the user (the design must work at every level).
- Collecting invasive device fingerprints to make the Governor decision.

## Cross-Skill note

- The Level 0–3 ladder is inherited from the stable v1.1 `apple-liquid-glass-web`.
- The accessibility settings (Reduce Transparency, Increase Contrast, Reduce Motion) are documented in `apple-control-states` and the stable v1.1 baseline.
- The motion values (reduced-motion adaptations) are `apple-motion-physics`.
- The state model (idle, hover, press, focus, selected, disabled) is `apple-control-states`.
- The legibility model is `glass-legibility.md`.
- The native Liquid Glass APIs (`GlassEffectContainer`, `glassEffectID`, `glassEffectTransition`, `UIGlassEffect`, `UIGlassContainerEffect`) are owned by the platform; the Skill recommends the system primitive.
