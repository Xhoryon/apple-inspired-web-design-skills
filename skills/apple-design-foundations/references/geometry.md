# Geometry Reference — Radius, Edges, Floating Surfaces

Extended recipes for Apple-inspired geometry. Builds on `apple-design-foundations/SKILL.md`.

## Radius by role (recap)

```css
:root {
  --r-window: 24px;     /* sheet, large panel */
  --r-card:   18px;     /* card, panel */
  --r-input:  10px;     /* input field */
  --r-button: 12px;     /* default button */
  --r-pill:   9999px;   /* pill button, tag */
  --r-icon:   50%;      /* icon button (circle) */
  --r-image:  0;        /* edge-to-edge media — no rounding */
}
```

The **concentric corners** rule:

```
Window / sheet       r-window = 24px
└── Card              r-card = 18px    (less than window)
    └── Pill button   r-pill  = 9999px (much more than card, pill shape)
```

Outer > inner. Pill breaks the line because the pill is conceptually a separate floating layer.

## When to round and when not to round

**Round:**

- Cards, panels, sheets, windows.
- Buttons, inputs, controls.
- Floating surfaces (navbar, toolbar, popover).
- Image containers that are *small* thumbnails (avatars, icons).

**Don't round:**

- Full-bleed media (hero images, video, large product photos).
- The page itself (no border-radius on body).
- Internal grid lines.
- Separators (hairlines are usually horizontal lines, not rounded).

## Pill is a rhythm choice, not a default

Pill buttons (`border-radius: 9999px`) are common in:

- Hero CTAs ("Buy now").
- Tag chips.
- Search bars (collapsed).
- Toggle controls.

But:

- Not every button is a pill. Default is 12 px or 14 px.
- Not every input is a pill. Default is 10 px or 12 px.
- Mixing radius types in the same view (one pill, one 12 px button) creates rhythm.
- All-pill is monotony.

## Edge-to-edge media

When media is the message, it breaks the rounded corner rule:

```css
.full-bleed {
  width: 100vw;
  position: relative;
  left: 50%;
  right: 50%;
  margin-left: -50vw;
  margin-right: -50vw;
  border-radius: 0;
}
```

Use for:

- Hero images.
- Large product photos.
- Full-screen video.
- Section background images.
- Wide product galleries.

Don't use for:

- Inline thumbnails.
- Avatars (circle is fine here).
- Image-with-caption blocks.

## Floating surfaces

Floating elements (navbar, toolbar, sheet, popover, command palette) sit inset from the viewport edges with a larger radius.

```css
.floating {
  position: fixed;
  inset: 12px;
  border-radius: 22px;
  /* Optional: backdrop-filter for glass (Level 1+) */
}
```

Inset by 12–24 px from edges. Larger radius (18–24 px) than inline surfaces.

Mobile floating surfaces should sit closer to the bottom edge with safe-area inset:

```css
@media (max-width: 768px) {
  .floating {
    inset: auto 12px calc(12px + env(safe-area-inset-bottom)) 12px;
  }
}
```

## Optical alignment

Mathematical alignment is not visual alignment.

### Buttons

Button text optical center is ~1 px above mathematical center. Compensate:

```css
.button {
  display: inline-flex;
  align-items: center;
  padding: 12px 22px;
  /* The text needs to sit ~1 px lower than math suggests */
  /* If text appears too high, increase line-height slightly */
  line-height: 1.2;
}
```

### Icon + text

Icon next to body text needs vertical alignment that looks centered:

```css
.icon-text {
  display: inline-flex;
  align-items: center;
  gap: 8px;
}
.icon-text svg { width: 20px; height: 20px; }
/* If the icon visually floats up, add a 1px translateY */
.icon-text svg { transform: translateY(1px); }
```

### Card content

Card content should sit slightly above mathematical center because the bottom edge of a card feels heavier than the top:

```css
.card {
  padding: 28px 24px;
  /* If the content feels like it's sliding down, decrease top padding by 4px */
}
```

## Nested corners

When one rounded element is nested in another:

```css
.outer  { border-radius: 28px; padding: 12px; background: var(--bg-elevated); }
.inner  { border-radius: 18px; padding: 16px; background: var(--bg-tinted); }
.button { border-radius: 980px; padding: 10px 20px; }
```

Outer radius > inner radius. The gap (outer padding) makes the corners feel concentric.

## Mixed radii

Mixing radii in the same view creates rhythm:

```css
.card-pill-row {
  display: flex;
  gap: 12px;
  align-items: center;
}
.card-pill-row .pill   { border-radius: 9999px; padding: 8px 16px; }
.card-pill-row .button { border-radius: 12px; padding: 10px 18px; }
```

A pill next to a 12 px button creates visual rhythm. Two pills next to each other is monotonous.

## Common geometry mistakes

- `border-radius: 24px` on every element.
- Pill button as the default.
- All cards with the same radius.
- Full-bleed images with rounded corners (looks weird, fights the edge).
- Floating surfaces flush with viewport edges (looks unfinished).
- Nested surfaces with the same radius as their parent (no concentric relationship).
- Optical center ignored — text sits too high or too low in buttons.

## Accessibility

- Border radius doesn't affect accessibility directly, but it does affect perceived affordance. Use rounded elements for touchable surfaces; sharp corners for non-interactive content.
- Focus rings should follow the surface's shape but stay visually distinct.
- Don't round the page itself — no `border-radius` on `body` or `html`.
