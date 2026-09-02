---
name: apple-liquid-glass-web
description: Use when the user explicitly asks for "Liquid Glass", "glass effect", "frosted glass", "translucent UI", a "floating navbar / toolbar", a glassy "sheet / popover / control surface", or asks to make a control feel like Apple's iOS 26 / macOS 26 / visionOS Liquid Glass. Also use whenever the design has decided a floating interaction layer needs material behavior. NOT for general glassmorphism, dashboard blurs, every-card-glass, blur-as-decoration, "make it look premium" requests without an interaction layer, or for the page background. Always pairs with apple-design-foundations and apple-design-audit.
version: "1.1.0"
license: MIT
---

# apple-liquid-glass-web

The most important skill in the pack. Liquid Glass is **not** glassmorphism. Misapplying it is the single biggest source of "fake Apple" pages.

## Liquid Glass IS NOT Glassmorphism

The following pattern is **frosted glass / glassmorphism baseline** — it is **not** Liquid Glass:

```css
.glass {
  background: rgba(255, 255, 255, 0.4);
  backdrop-filter: blur(12px);
  border: 1px solid rgba(255, 255, 255, 0.3);
  border-radius: 24px;
}
```

This pattern alone fails Liquid Glass on **at least five dimensions** below. If you ship only this, mark the work `FAKE LIQUID GLASS / FROSTED GLASS ONLY` in the audit and stop.

Web has **no native `glassEffect()`** equivalent of SwiftUI. Web "Liquid Glass" is a high-quality Web **approximation** of Apple's material behavior, not a port of it. Acknowledge this in the design.

## The seven dimensions of Liquid Glass

Every Liquid Glass surface must address these dimensions. Missing three or more = not Liquid Glass.

### 1. Translucency

Light passes through the material and the **layer behind affects its color and luminance**. Adjust the alpha based on what's behind, not a fixed value.

- Surface alpha is **adaptive** (light bg vs dark bg vs image bg).
- Provide explicit dark-mode and light-mode versions.
- Text or icons on the surface must remain readable across backgrounds.

### 2. Refraction (lensing / distortion)

Light **bends** through the material. Real refraction shifts pixels at edges; on Web this is approximated by:

- SVG `feDisplacementMap` driven by edge gradient or noise.
- Or shader displacement (Level 3).
- Or `backdrop-filter` with `hue-rotate` + small `saturate` to suggest optical change.
- Do **not** fake refraction by warping static background images.

### 3. Specular response

The surface catches light. A real glass surface has a moving highlight that follows the pointer or scrolls.

- Implement via CSS gradient that updates on `pointermove` (rAF-throttled).
- Or via shader lighting (Level 3).
- A static gradient highlight is decoration, not specular response.

### 4. Adaptivity

The material adapts to:

- Background luminance (light / dark mode, image content).
- Foreground content density.
- Interaction state (idle, hover, active, focused).
- Scroll position (e.g. a navbar that intensifies as content scrolls behind it).

### 5. Depth (separation from content)

Liquid Glass lives **on the interaction layer**, not the content layer.

- Content (body, images, articles) should **not** be glass.
- Interaction surfaces (floating navbar, toolbar, sheet, segmented control, popover, media control overlay) **may** be glass.
- Glass over glass over glass is forbidden.

### 6. Dynamics

Glass responds to input and state, but **restrained**:

- Pointer-driven highlight (capped rAF).
- Subtle scale/opacity on press.
- Material change on focus.
- Continuous morph between states (segmented control selection, sheet detents).
- Reduced-motion users get no morph — instant state change with clear contrast.

### 7. Morphing

Related controls can **merge / separate / stretch / reshape / materialize** with continuous motion.

- Example: floating toolbar buttons can merge into a single pill, then separate.
- Example: a segmented control selection can morph into a popup sheet.
- Example: a search bar can expand into a search surface.
- Implementation: FLIP, View Transitions API, or spring-animated transforms.
- Use only when the morph carries meaning, not as decoration.

## Where Liquid Glass belongs

**Use** on:

- Floating navigation bar
- Floating toolbar / contextual controls
- Segmented control
- Search surface (collapsed/expanded)
- Media control overlay
- Compact overlay / sheet / popover / drawer
- Floating action group (FAB-style cluster)
- Tab bar (mobile bottom bar)
- Status indicators that float over media
- Spotlight-style command surface

**Use carefully** on:

- Card containers (only if they are floating panels over media, not content cards)
- Image with caption overlay (caption bar only, not the whole card)

**Do not use** on:

- Whole page background
- Every section / every card
- Body text containers
- Tables, forms, data lists
- Article bodies
- Landing page hero text container
- Static marketing surfaces that do not interact

**Hard rule**: a page with N floating glass surfaces and N+5 "decorative" glass cards is wrong. If you can remove a glass surface and the page reads better, remove it.

## Web implementation levels (progressive enhancement)

Pick the lowest level that achieves the design intent.

### Level 0 — Fallback

For low-end devices, `prefers-reduced-transparency`, `prefers-reduced-motion`, browsers without `backdrop-filter`.

- Solid background color (theme-aware).
- 1 px subtle border.
- No blur. No displacement.
- Same shape and proportions as Level 1+ so layout doesn't shift.

```css
@supports not (backdrop-filter: blur(1px)) {
  .glass { background: var(--surface-solid); }
}
@media (prefers-reduced-transparency: reduce) {
  .glass { background: var(--surface-solid); backdrop-filter: none; }
}
```

### Level 1 — CSS Glass (baseline approximation)

`backdrop-filter` + layered gradient + 1 px hairline border + soft shadow.

- Use for: floating navbar, sheet, popover, simple toolbar.
- Limits: no real refraction, no real specular response.
- Must include contrast protection for text on translucent surfaces (subtle text-shadow, scrim layer, or theme-aware fill).

```css
.glass {
  background: linear-gradient(
    180deg,
    rgba(255, 255, 255, 0.55),
    rgba(255, 255, 255, 0.35)
  );
  border: 1px solid rgba(255, 255, 255, 0.4);
  backdrop-filter: saturate(180%) blur(20px);
  -webkit-backdrop-filter: saturate(180%) blur(20px);
  box-shadow:
    0 1px 0 rgba(255, 255, 255, 0.6) inset,
    0 10px 30px rgba(0, 0, 0, 0.12);
}
```

Rules:

- `backdrop-filter` is GPU-expensive. Cap persistent instances to ~3 per page.
- Always provide a solid-color fallback.
- Hairline border must be **sub-pixel and theme-aware**, not pure white.
- One inset highlight, one outer shadow. No stacked shadows.

### Level 2 — Enhanced Glass (SVG + pointer-driven)

Adds real-feeling refraction and specular response using SVG filters and pointer-driven highlights.

- Use for: hero floating controls, premium sheet, command palette.
- Adds:
  - SVG `feDisplacementMap` with an edge gradient (subtle, ≤4 px shift).
  - Pointer-tracked specular highlight (rAF throttled to one update per frame, idle when pointer hasn't moved for 200 ms).
  - Mask gradients to simulate refraction bending near edges.

```html
<svg width="0" height="0" style="position:absolute">
  <filter id="glass-refract">
    <feTurbulence type="fractalNoise" baseFrequency="0.9" numOctaves="2" seed="3"/>
    <feDisplacementMap in="SourceGraphic" scale="3"/>
  </filter>
</svg>
```

Level 2 only on hero/primary surfaces. Never on every navbar.

### Level 3 — Optical / Shader Glass

WebGL / WebGPU fragment shader for true optical simulation.

- Use for: hero centerpiece, marketing hero where the surface **is** the design.
- Adds: real lens distortion, real edge highlight, dynamic environment response, optional very subtle chromatic variation.
- Cost: high. Must downsample (≤0.6x devicePixelRatio), pause when off-screen or idle.
- Provide Level 1 fallback automatically.

```text
// pseudo-shader outline — see references/shader-glass.md for full implementation
uniform sampler2D uBackdrop;
uniform vec2 uResolution;
uniform vec2 uPointer;
uniform float uTime;
void main() {
  vec2 uv = vUv;
  vec2 dir = uv - 0.5;
  float dist = length(dir);
  // edge displacement
  vec2 refracted = uv + normalize(dir) * pow(dist, 2.0) * 0.03;
  vec4 bg = texture2D(uBackdrop, refracted);
  // specular highlight near pointer
  float spec = exp(-pow(distance(uv, uPointer) * 30.0, 2.0));
  gl_FragColor = vec4(bg.rgb + spec * 0.15, 0.85);
}
```

Rules:

- Never on more than one surface per page.
- Pause on `visibilitychange`, on battery saver, when GPU is constrained.
- Respect `prefers-reduced-motion` and `prefers-reduced-transparency`.

## Contrast and readability

A glass surface is **only as good as the text it carries**.

- Text on glass needs either:
  - A scrim layer (semi-opaque gradient behind the text region only), or
  - A theme-aware fill that switches to opaque on busy backgrounds, or
  - A text shadow / backdrop stack small enough to remain subtle.
- Body copy on glass over busy imagery is **forbidden** unless the busy region is masked.
- Icons need at least 4.5:1 against the worst-case background behind the surface.
- Always check light-mode AND dark-mode AND over-imagery.

## Performance budget

- Persistent `backdrop-filter` surfaces per page: ≤ 3 at Level 1, ≤ 1 at Level 2+, 1 max at Level 3.
- Blur radius: 16–24 px on desktop, 12–18 px on mobile.
- Pointer-driven specular: rAF throttled; idle after 200 ms.
- Shader Glass: pause on tab hidden, low battery, `Save-Data`, slow CPU detected via `navigator.hardwareConcurrency`.
- Avoid `backdrop-filter` on large full-bleed surfaces (a fullscreen blurred hero costs >5× a navbar).

## Accessibility

- `prefers-reduced-transparency: reduce` → solid color, no blur.
- `prefers-reduced-motion: reduce` → no morph, no specular animation, instant state change.
- Focus rings must remain visible on glass surfaces (don't blur them into invisibility).
- Tap targets ≥ 44×44 px.
- Text on glass must meet WCAG AA contrast against worst-case backdrop.

## Hard rules (no exceptions)

- **Liquid Glass is not glassmorphism.** A `blur + white + 1 px border` is not enough.
- **Glass is for interaction surfaces, not content surfaces.** Body copy, tables, forms, articles stay solid.
- **No glass-on-glass stacking.** One floating layer over content. Period.
- **Always include a solid fallback.** `prefers-reduced-transparency` and old browsers must still render a usable surface.
- **No more than three persistent backdrop-filter surfaces on a page.**
- **Never fake refraction with a static image.** Either displace pixels or don't pretend to.
- **Specular response is dynamic, not decorative.** A static highlight gradient is decoration.
- **Refraction requires optical information.** If the backdrop has no texture, contrast, geometry, or content variation, the glass is invisible. Skip glass or move it to a backdrop that has something to refract.
- **Performance is non-negotiable.** Test on a low-end mobile profile before shipping.
- **Brand stays the user's.** Apple's demo Liquid Glass surfaces use Apple color and lighting; replicate the **behavior**, not the palette.

## Refraction requires optical information

A glass surface is only "real" when the backdrop beneath it has **optical information** — texture, contrast, geometry, color variation, content edges.

When the backdrop is:

- Solid color or near-solid color
- Pure white / off-white on off-white
- A flat gradient with no contrast variation

…the `backdrop-filter` is technically active but the result looks like a solid pill. The user sees no glass effect. The GPU cost is wasted. The demo is misleading.

### Decision rule

Before applying any Liquid Glass surface, evaluate the backdrop:

| Backdrop | Glass decision |
|---|---|
| Image with content / variation | **Glass appropriate.** Refraction will be visible. |
| Gradient field with strong color contrast | **Glass appropriate.** Distortion will be visible. |
| Typography or geometric content below | **Glass appropriate.** Edges will be visible. |
| Solid single color | **Skip glass.** Use a solid floating control with subtle shadow. |
| Near-flat off-white on off-white | **Skip glass.** Glass becomes invisible. |
| Dark single color | **Skip glass.** Same reasoning — no edges to refract. |

### When the user insists on glass over a flat backdrop

Push back. Explain that glass over flat color is wasted GPU and creates an invisible surface. If glass is still required, **add optical information to the backdrop first** (a gradient field, an image, structured content) — then place the glass.

### Showcase rule

Any demo page that claims to demonstrate Liquid Glass **must include a backdrop with optical information**. Otherwise the demo is "blur over white", which is the exact pattern the skill labels `FAKE LIQUID GLASS / FROSTED GLASS ONLY`.

## When the user insists on calling something Liquid Glass

Real-world clients may insist on the term "Liquid Glass" for marketing, even when the implementation doesn't meet the multi-dimension bar.

### The discipline

The skill label is a **technical term**, not a marketing term. "Liquid Glass" means: at least 3 of the 7 dimensions, glass belongs to the interaction layer, the user can see the effect. If the implementation is `backdrop-filter + border + shadow` only, it is Frosted Glass, and calling it Liquid Glass is technically wrong.

### How to push back without losing the client

**Acknowledge the brand value of the term, then redirect the implementation.**

> "I hear you on the Liquid Glass branding — that's a strong positioning move. To make the claim credible, the implementation needs to be observable in the browser. The minimum bar is real refraction (not just blur) and adaptive contrast over varying backgrounds. I'll spec a Level 2 surface that meets both, and we'll demonstrate it in the showcase. If you'd rather keep the Level 1 cost, I'll label the build 'Frosted Glass inspired by Apple Liquid Glass' so we don't claim more than we ship."

### What NOT to do

- Don't ship `backdrop-filter + border + shadow` and call it Liquid Glass in client materials.
- Don't tell the client "yes, we shipped Liquid Glass" when the work was Level 1 baseline.
- Don't change the definition of Liquid Glass to fit the budget. Either budget for Level 2+ or label the build honestly.

### What to do

- Surface the gap early: "to call this Liquid Glass we need ≥ 3 dimensions; here's what each costs."
- Offer the Level 2 / Level 3 implementation as the path to the brand promise.
- Use a tasteful Frosted Glass label when that's what shipped.

## Review checklist

Before declaring a Liquid Glass surface done:

- [ ] Identifies the dimension(s) being expressed (translucency / refraction / specular / adaptivity / dynamics / morphing / depth).
- [ ] Has a Level 0 solid fallback for `prefers-reduced-transparency` and old browsers.
- [ ] Has dark-mode and light-mode variants.
- [ ] Text on the surface passes WCAG AA in both modes.
- [ ] Pointer-driven highlight (if any) is rAF-throttled and pauses on idle.
- [ ] No more than three persistent `backdrop-filter` surfaces on the page.
- [ ] No glass-on-glass nesting.
- [ ] Surface is on the interaction layer, not the content layer.
- [ ] Reduced-motion users get an instant state change, not a morph.
- [ ] Tested on a low-end mobile profile (see `references/performance.md`).

## Common mistakes

- Calling `backdrop-filter: blur(20px)` + white fill "Liquid Glass". It is glassmorphism.
- Putting glass on body text containers.
- Stacking glass layers for "depth".
- Using a static gradient as a fake highlight.
- Animating the highlight on every frame instead of only on pointer movement.
- Forgetting `prefers-reduced-transparency`.
- Putting a glass navbar over a hero image without contrast protection for the search text.
- Shader Glass on a regular button — way too much cost for way too little gain.

## Companion references

- `references/css-glass.md` — Level 0/1 recipes with theme variants.
- `references/svg-glass.md` — Level 2 SVG displacement + pointer highlight.
- `references/shader-glass.md` — Level 3 WebGL/WebGPU recipes.
- `references/contrast-on-glass.md` — readable text on translucent surfaces.
- `references/performance.md` — budgets, profiling, fallbacks.

This skill must be paired with apple-design-foundations (typography/space) and apple-design-audit (real browser check). Never ship glass without screenshotting it in light, dark, and over imagery.
