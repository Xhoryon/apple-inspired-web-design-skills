# Contrast on Glass — Text Readability on Translucent Surfaces

A Liquid Glass surface is only as good as the text it carries. This reference covers how to keep text readable across light, dark, and busy backgrounds.

## The problem

A translucent surface lets the background show through. That background might be:

- Light surface (off-white, `#FBFBFD`).
- Dark surface (near-black, `#000000` to `#1C1C1E`).
- Imagery (product photo, hero image, gradient).
- Another translucent surface (forbidden — see apple-liquid-glass-web).

Each background demands a different treatment. A single surface can't be readable over all of them by itself — it needs a contrast strategy.

## Strategy A — Scrim layer

A semi-opaque gradient **only behind the text region** of the glass surface.

```html
<div class="glass glass-with-text">
  <div class="glass__scrim"></div>
  <div class="glass__content">
    <input type="search" placeholder="Search">
  </div>
</div>
```

```css
.glass-with-text {
  position: relative;
  border-radius: 18px;
  overflow: hidden;
}

.glass__scrim {
  position: absolute;
  inset: 0;
  background: linear-gradient(
    180deg,
    rgba(255, 255, 255, 0.55) 0%,
    rgba(255, 255, 255, 0.20) 100%
  );
  pointer-events: none;
}

.glass__content {
  position: relative;
  padding: 12px 16px;
}
```

The scrim boosts the local background brightness/darkness behind the text without changing the rest of the surface. The text reads because the scrim makes its backdrop predictable.

For dark scrim over imagery:

```css
.glass-with-text.on-image .glass__scrim {
  background: linear-gradient(
    180deg,
    rgba(0, 0, 0, 0.45) 0%,
    rgba(0, 0, 0, 0.20) 100%
  );
  color: white;
}
```

The `on-image` class is applied by the parent context (e.g. the section class).

## Strategy B — Theme-aware fill

The surface switches to opaque on busy backgrounds, driven by the surface's parent context:

```css
.glass {
  background: rgba(255, 255, 255, 0.40);
  backdrop-filter: saturate(180%) blur(20px);
}

.hero-image .glass {
  background: rgba(20, 20, 22, 0.78);
  color: #fff;
}
```

This is simpler than a scrim but loses some of the "glass" effect on busy backgrounds. Acceptable for navbars and toolbars where readability matters more than optical realism.

## Strategy C — Text shadow

A subtle text shadow helps light text on busy backgrounds:

```css
.glass .label {
  color: #fff;
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.40);
}
```

Avoid heavy shadows (they feel like 2008 web). A 1 px offset and a tight blur radius is enough.

## Strategy D — Backdrop-filter with stronger blur

When the surface is over an image, increase the blur radius to push the background further out of focus:

```css
.glass.on-image {
  backdrop-filter: saturate(180%) blur(28px);
}
```

This is the "soft pillow" effect — text reads because the background is so blurred it becomes a tonal field rather than identifiable content.

## Strategy E — Mask the busy region

For surfaces that overlap a specific region of an image (e.g. a caption bar across the bottom of a hero), use a mask gradient that darkens only behind the surface:

```html
<div class="hero">
  <img src="hero.jpg" alt="">
  <div class="hero__caption-glass"></div>
</div>
```

```css
.hero { position: relative; }
.hero img { display: block; width: 100%; }

.hero__caption-glass {
  position: absolute;
  inset: auto 0 0 0;
  height: 80px;
  background: linear-gradient(180deg, transparent, rgba(0, 0, 0, 0.55));
  /* No backdrop-filter needed if the gradient is strong enough */
}
```

A gradient overlay is sometimes enough. Glass is not required.

## Strategy F — Solid fallback (always)

```css
@media (prefers-reduced-transparency: reduce) {
  .glass {
    backdrop-filter: none;
    background: var(--surface-solid);
  }
}
```

When `prefers-reduced-transparency: reduce` is set, the user gets a solid surface. Contrast is then a standard solid-on-solid calculation.

## Worst-case testing

For any glass surface, test the **worst-case** background:

```js
// Manual: capture the surface over light, dark, and image backgrounds.
const cases = ["light", "dark", "image"];
for (const c of cases) {
  // set background, capture glass surface, measure text contrast
}
```

Text must pass WCAG AA (4.5:1 for body, 3:1 for large display) against the worst-case background.

Tools:

- Chrome DevTools → Rendering → "Emulate CSS prefers-reduced-transparency".
- Manually capture over a real hero image.
- Use a contrast checker on the captured pixels.

## Common mistakes

- A glass surface over a busy image with no scrim, no theme switch, no text shadow → unreadable.
- "Glass" with `backdrop-filter: blur(8px)` over a complex hero image → text becomes a smear.
- Body text on a glass surface without a contrast strategy → fails WCAG.
- Using only a text shadow over a glass surface. Shadows help but don't replace scrims or fills.
- Forgetting dark mode. A scrim that works in light mode may be invisible in dark mode.

## Accessibility

- WCAG AA: body text 4.5:1, large display 3:1.
- WCAG AAA: body text 7:1, large display 4.5:1.
- Don't rely on color alone. Pair with icon, label, or shape.
- Test with `prefers-contrast: more` and `prefers-reduced-transparency: reduce`.
