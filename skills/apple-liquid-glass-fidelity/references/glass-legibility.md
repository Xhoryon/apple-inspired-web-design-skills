# Glass Legibility Reference

The legibility model for text and icons on glass surfaces. This file is the Tier 2 reference for `apple-liquid-glass-fidelity`.

A glass surface is only as good as the text it carries. A glass surface that does not read is decoration.

## The legibility model

Legibility on glass is a function of:

- **Foreground contrast** — the contrast between the text / icon and the visible backdrop.
- **Underlying background complexity** — busy images, gradients, and patterns are harder to read on.
- **Material opacity / diffusion** — a more opaque surface gives more contrast; a more transparent surface depends on the backdrop.
- **Label / icon complexity** — long labels, small icons, and dense copy are harder to read.
- **Surface size** — a small surface (compact toolbar) is harder to read than a large surface (sheet).
- **Tint** — a heavy tint can erase the foreground contrast.
- **Dimming / scrim** — a scrim layer behind the text region only can rescue legibility on busy backgrounds.

A glass surface that is legible on a flat background but illegible on a busy image is a defect. The implementer must test on multiple backgrounds.

## The three scrim strategies

The stable v1.1 `apple-liquid-glass-web` baseline documents the three scrim strategies. This Skill recaps and adds the Fidelity-specific guidance.

### Strategy A — Scrim layer (most common)

A semi-opaque gradient **only behind the text region** of the glass surface.

- **Native:** the system primitive handles scrim on sheets and popovers.
- **Web approximation:** a `::before` pseudo-element on the surface, with a gradient background, positioned absolutely behind the text region.
- **When to use:** floating navbar with search field; sheet with header text; any surface with text over a busy backdrop.

The scrim is gradient-only, not full-width. A full-width scrim defeats the purpose of glass (the user sees the scrim, not the glass).

### Strategy B — Theme-aware fill

A fill that goes opaque when the surface is over a busy background.

- **Native:** the system primitive adapts the fill based on the backdrop.
- **Web approximation:** a parent class (e.g. `.has-image-bg`) changes the surface's fill to a more opaque value.
- **When to use:** hero surfaces; the same surface renders over both content (light background) and an image (busy background).

The theme-aware fill is not a constant; it depends on the surface's parent context.

### Strategy C — Text shadow

A subtle text shadow that rescues legibility on very light text over a busy surface.

- **Native:** the system primitive uses subtle shadows for light text over media.
- **Web approximation:** `text-shadow: 0 1px 2px rgba(0, 0, 0, 0.4);` on the text element.
- **When to use:** light text on a dark or busy backdrop.

The text shadow must be subtle. Heavy text shadows feel like 2008 web; the Skill rejects them.

## What the legibility model forbids

- **Body copy on glass over busy imagery** without a scrim or theme-aware fill.
- **Long labels on small glass surfaces** (e.g. a 28 × 28 px glass button with a 12-character label).
- **Heavy text shadows** that make the text look embossed.
- **A tint that erases the foreground contrast** (e.g. a saturated blue tint on white text).
- **A constant fill that does not adapt** to the backdrop.
- **A glass surface with no visible edge** on a busy background (the user cannot tell where the surface ends).

## Contrast targets

The Skill records the WCAG-aligned targets:

- **Body text (≥ 18 px regular or ≥ 14 px bold):** 4.5:1 against the worst-case visible backdrop.
- **Large text (≥ 24 px regular or ≥ 18 px bold):** 3:1.
- **Icons (graphical objects):** 3:1.
- **Focus indicators:** 3:1 against adjacent colors.

The "worst-case visible backdrop" is the most contrast-destroying region of the backdrop. A glass surface that achieves 4.5:1 on a flat white background may fail on a busy image. The implementer must test the worst case.

## Surface size and label complexity

- A 28 × 28 px surface with a 12-character label is too small. Either the surface grows, the label shortens, or the label becomes an icon.
- A 44 × 44 px surface (the touch target) with a 1-word label is acceptable.
- A 320 × 56 px surface (a floating search bar) with a placeholder label is comfortable.
- A sheet surface with a 1-line header is comfortable.

The implementer chooses the surface size based on the content; the Skill records the principle.

## Adaptivity interaction

The legibility model interacts with the adaptivity model:

- Light background → the surface may be lighter; the text may be dark.
- Dark background → the surface may be darker; the text may be light.
- Colorful / busy background → the surface may go opaque (theme-aware fill) or use a scrim.
- Monochrome → the surface may be neutral; the text may be the brand color.
- `prefers-contrast: more` → the surface goes more opaque; the text may be bolder.
- `prefers-reduced-transparency: reduce` → the surface goes more opaque; the text contrast is preserved.

## Background testing

The implementer must test the surface on at least three backgrounds:

1. Light background (e.g. white, off-white, light gray).
2. Dark background (e.g. dark gray, near-black).
3. Busy background (e.g. an image with content variation, a gradient with strong contrast, a photo).

The surface must achieve WCAG contrast on the worst case across all three. A surface that passes light + dark but fails on busy is a defect.

The Skill does not require exotic testing tools; the implementer can use a hero image, a gradient swatch, and a dark swatch as the three test backgrounds.

## What the Skill rejects

- A glass surface with text that fails WCAG contrast on the worst-case backdrop.
- A glass surface with a heavy tint that erases the foreground contrast.
- A glass surface that uses text shadow as a substitute for proper contrast.
- A glass surface that is illegible on a busy background without a scrim.
- A glass surface that does not test on light, dark, and busy backdrops.
- A glass surface that uses Apple blue as the only color (brand violation + legibility risk).

## Cross-Skill note

- The contrast targets are WCAG 2.1 AA; the Skill records the targets, not the implementation.
- The state model (idle, hover, pressed, focused, selected, disabled) is owned by `apple-control-states`. The legibility model must be preserved across states (a `pressed` state with reduced contrast is a defect).
- The accessibility settings (Reduce Transparency, Increase Contrast, Reduce Motion) are owned by `apple-control-states` and the stable v1.1 baseline.
- The scrim strategies are also documented in the stable `apple-liquid-glass-web/references/contrast-on-glass.md`; this Skill recaps and adds the Fidelity-specific guidance.
- The material state model is `glass-interaction.md`.
- The adaptivity model is the Adaptivity section of `SKILL.md`.
