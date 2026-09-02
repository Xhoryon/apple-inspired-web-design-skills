# Space, Grid, and Geometry Reference

Extended recipes for Apple-inspired spacing and layout. Builds on `apple-design-foundations/SKILL.md`.

## The spacing scale

Pick **one** modular scale and use it consistently.

```css
:root {
  --space-1:  4px;
  --space-2:  8px;
  --space-3:  12px;
  --space-4:  16px;
  --space-5:  20px;
  --space-6:  24px;
  --space-8:  32px;
  --space-10: 40px;
  --space-12: 48px;
  --space-16: 64px;
  --space-20: 80px;
  --space-24: 96px;
  --space-32: 128px;
  --space-40: 160px;
}
```

Or fluid:

```css
:root {
  --space-1:  clamp(0.25rem, 0.5vw, 0.5rem);
  --space-2:  clamp(0.5rem, 1vw, 0.75rem);
  --space-4:  clamp(1rem, 1.5vw, 1.25rem);
  --space-8:  clamp(1.5rem, 2vw, 2rem);
  --space-16: clamp(3rem, 5vw, 4rem);
  --space-24: clamp(4rem, 7vw, 6rem);
  --space-40: clamp(6rem, 10vw, 10rem);
}
```

Mix fluid for section-level spacing, fixed for component-level spacing.

## Content width

```css
:root {
  --content-width: 1200px;     /* max for most marketing pages */
  --content-narrow: 720px;     /* article body / long-form */
  --content-text: 65ch;        /* body measure */
  --gutter: clamp(20px, 4vw, 48px);
}

.container {
  max-width: var(--content-width);
  margin-inline: auto;
  padding-inline: var(--gutter);
}
```

## Section rhythm

Vary section padding. Identical padding everywhere feels mechanical.

```css
.section { padding-block: clamp(56px, 8vw, 96px); }
.section--quiet { padding-block: clamp(40px, 5vw, 64px); }
.section--loud  { padding-block: clamp(80px, 12vw, 160px); }
.section--full  { padding-block: 0; }     /* full-bleed media */
```

Pick **at least two** of these per long page. Don't use the same one five times.

## Vertical rhythm (within sections)

Within a section, maintain rhythm:

```css
.section > * + * { margin-top: var(--space-6); }
.section h2 + p  { margin-top: var(--space-4); }
.section h2 + .cta { margin-top: var(--space-8); }
```

Avoid hard-coding 23 px, 37 px etc. — if you need a value outside the scale, add it to the scale.

## Optical alignment

Mathematical alignment is not visual alignment. Adjust for:

- **Icon + text rows.** A 20 px icon and 16 px text need the icon to be ~1 px lower than math suggests to look centered.
- **Button text.** Optical center is ~1 px above mathematical center. Adjust line-height to push text down by 1 px.
- **Card content.** Card content should sit slightly above mathematical center because the bottom feels heavier.

## Grid

For most Apple-inspired pages, the grid is generous:

- 12-column grid, 24 px gutter desktop, 16 px gutter mobile.
- Content blocks span 6–8 columns, leaving whitespace on the sides.
- Hero typically spans the full width.

```css
.grid {
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  gap: 24px;
}

@media (max-width: 768px) {
  .grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 16px;
  }
}
```

Don't use the 12-column grid for everything. Two-column asymmetric layouts (60/40, 70/30) are common in Apple pages.

## Concentric corners

When one rounded element is nested in another:

```css
.outer  { border-radius: 28px; padding: 16px; }
.inner  { border-radius: 20px; padding: 16px; }
.button { border-radius: 980px; padding: 12px 22px; } /* pill inside inner */
```

Outer > inner > button. The outer radius is at least 8 px more than the inner. Pill buttons inside a card feel right because the pill is conceptually a separate layer.

## Radius by role

```css
:root {
  --r-window: 24px;     /* sheet, large panel */
  --r-card:   18px;     /* card, panel */
  --r-input:  10px;     /* input field */
  --r-button: 12px;     /* default button */
  --r-pill:   9999px;   /* pill button, tag */
  --r-icon:   50%;      /* icon button (circle) */
}
```

Don't apply one of these everywhere. The point is **role-based** radius.

## Edge-to-edge media

When media is the message, it goes edge-to-edge:

```css
.full-bleed {
  width: 100vw;
  margin-left: calc(50% - 50vw);
}
```

Use sparingly. Edge-to-edge should feel intentional, not "we forgot to constrain".

## Floating surfaces

Floating toolbars, navbars, sheets float inset from the viewport edge:

```css
.floating {
  position: fixed;
  inset: 12px;
  border-radius: 22px;
}
```

Inset of 12–24 px from edges. Larger radius than inline cards.

## Mobile layout

Mobile is a redesign, not a shrink:

- 4-column grid (or 2-column + edge-to-edge).
- Section padding reduces 30–40%.
- Hero stacks vertically: image first or text first, decided per page.
- Cards stack to single column or 2-up.
- Tap targets ≥ 44×44 px.
- No horizontal scroll. Test aggressively.

## Common spacing mistakes

- Random spacing values (23 px, 37 px, 41 px).
- All sections with identical padding.
- 80% empty viewport in the name of "minimalism".
- Cards with 4 px of internal padding (content feels cramped).
- Cards with 64 px of internal padding (content feels lost).
- Centering everything. Many things should be left-aligned.
- Mixing 4 px and 8 px scales in the same page (pick one).

## Accessibility

- Maintain logical tab order with the visual order.
- Don't use whitespace to fake a focus indicator.
- Don't rely on a particular background color for content to be visible — body should read on the page surface, not on a "container".
- Touch targets ≥ 44×44 px regardless of visual size.
