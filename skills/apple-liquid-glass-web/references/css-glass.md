# CSS Glass — Level 0 / Level 1 Recipes

Drop-in CSS for the basic Liquid Glass surface. **Level 1 only** — for refraction and specular response, see `svg-glass.md` and `shader-glass.md`.

## Rules for using these recipes

- Always pair with a Level 0 solid fallback.
- Cap persistent instances at 3 per page.
- Don't combine these classes with other translucent surfaces.
- Always provide a dark-mode variant.
- Always include contrast protection for any text on the surface.

---

## Level 0 — Solid fallback

```css
:root {
  --surface-floating-light: rgba(245, 245, 247, 0.92);
  --surface-floating-dark:  rgba(28, 28, 30, 0.86);
  --hairline-light:         rgba(0, 0, 0, 0.08);
  --hairline-dark:          rgba(255, 255, 255, 0.10);
}

.glass {
  background: var(--surface-floating-light);
  border: 1px solid var(--hairline-light);
  box-shadow:
    0 1px 0 rgba(255, 255, 255, 0.5) inset,
    0 10px 30px rgba(0, 0, 0, 0.10);
}

@media (prefers-color-scheme: dark) {
  .glass {
    background: var(--surface-floating-dark);
    border-color: var(--hairline-dark);
    box-shadow:
      0 1px 0 rgba(255, 255, 255, 0.06) inset,
      0 10px 30px rgba(0, 0, 0, 0.40);
  }
}
```

This works in every browser, every device, every accessibility setting. Use it as the floor.

---

## Level 1 — CSS Glass

```css
.glass {
  background:
    linear-gradient(
      180deg,
      rgba(255, 255, 255, 0.55) 0%,
      rgba(255, 255, 255, 0.35) 100%
    );
  border: 1px solid rgba(255, 255, 255, 0.40);
  backdrop-filter: saturate(180%) blur(20px);
  -webkit-backdrop-filter: saturate(180%) blur(20px);
  box-shadow:
    0 1px 0 rgba(255, 255, 255, 0.6) inset,
    0 10px 30px rgba(0, 0, 0, 0.12);
}

@media (prefers-color-scheme: dark) {
  .glass {
    background:
      linear-gradient(
        180deg,
        rgba(40, 40, 44, 0.55) 0%,
        rgba(20, 20, 22, 0.45) 100%
      );
    border-color: rgba(255, 255, 255, 0.12);
    box-shadow:
      0 1px 0 rgba(255, 255, 255, 0.08) inset,
      0 10px 30px rgba(0, 0, 0, 0.50);
  }
}

/* Solid fallback when backdrop-filter unsupported or reduced transparency */
@supports not ((backdrop-filter: blur(1px)) or (-webkit-backdrop-filter: blur(1px))) {
  .glass {
    backdrop-filter: none;
    -webkit-backdrop-filter: none;
    background: var(--surface-floating-light);
  }
}

@media (prefers-reduced-transparency: reduce) {
  .glass {
    backdrop-filter: none;
    -webkit-backdrop-filter: none;
    background: var(--surface-floating-light);
  }
  @media (prefers-color-scheme: dark) {
    .glass {
      background: var(--surface-floating-dark);
    }
  }
}
```

### Sizing for common surfaces

```css
/* Floating navbar */
.glass-navbar {
  position: fixed;
  inset: 12px 12px auto 12px;
  border-radius: 18px;
  height: 48px;
  padding: 0 14px;
  display: flex;
  align-items: center;
  gap: 12px;
  z-index: 50;
}

/* Floating toolbar */
.glass-toolbar {
  position: fixed;
  inset: auto 12px 12px 12px;
  border-radius: 22px;
  height: 56px;
  padding: 6px 8px;
  display: flex;
  align-items: center;
  gap: 4px;
}

/* Sheet */
.glass-sheet {
  position: fixed;
  inset: auto 0 0 0;
  border-radius: 24px 24px 0 0;
  padding: 16px;
  z-index: 60;
}

/* Popover */
.glass-popover {
  position: absolute;
  border-radius: 14px;
  padding: 8px;
  min-width: 200px;
  z-index: 60;
}
```

---

## Contrast protection for text on glass

Body text on glass must read against the worst-case background. Three strategies:

### Strategy A — Scrim layer (most common)

A semi-opaque gradient **only behind the text region** of the glass surface:

```css
.glass-navbar.has-text::before {
  content: "";
  position: absolute;
  inset: 0;
  border-radius: inherit;
  pointer-events: none;
  background: linear-gradient(
    180deg,
    rgba(255, 255, 255, 0.45) 0%,
    rgba(255, 255, 255, 0.20) 100%
  );
}
```

### Strategy B — Theme-aware fill that goes opaque on busy backgrounds

Detect busy backgrounds (image bg) via a parent class:

```css
.glass.has-image-bg {
  background: rgba(245, 245, 247, 0.78);
  backdrop-filter: saturate(180%) blur(24px);
}
```

### Strategy C — Text shadow

For very light text on a busy surface, a subtle text shadow:

```css
.glass-text {
  color: #fff;
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.4);
}
```

Avoid heavy text shadows — they feel like 2008 web.

---

## What this recipe is NOT

This Level 1 recipe is **frosted glass**, the visual baseline Apple starts from. It is **not** the full Liquid Glass experience. To add refraction, see `svg-glass.md`. To add real specular response, see `shader-glass.md`. To get adaptivity (light/dark/image response), wire it up via CSS variables driven by the surface's actual backdrop — usually that means SVG or shader, not pure CSS.

For most floating toolbars, navbars, sheets, and popovers, **Level 1 is enough**. Reach for Level 2 only when the surface is a hero centerpiece or the user explicitly asked for a "wow" effect. Reach for Level 3 only when the surface **is** the design (a single hero element).

---

## Performance budget (Level 1)

- Cap persistent `backdrop-filter` surfaces per page: **3**.
- Blur radius: 16–24 px desktop, 12–18 px mobile.
- `saturate()` is cheap; `contrast()` and `hue-rotate()` are more expensive — use sparingly.
- Animating `backdrop-filter` properties at runtime is expensive — avoid.

## Accessibility

- Always include the `prefers-reduced-transparency` fallback.
- Always include `@supports not` fallback.
- Focus rings must remain visible on glass (use a 2 px focus outline outside the surface).
- Tap targets ≥ 44×44 px on mobile.
