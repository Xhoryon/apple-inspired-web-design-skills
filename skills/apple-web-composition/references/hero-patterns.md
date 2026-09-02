# Hero Patterns Reference

Extended recipes for Apple-inspired hero composition. Builds on `apple-web-composition/SKILL.md`.

## The five hero archetypes

Apple pages use a small number of hero archetypes. Pick one per page; don't blend.

### 1. Big Type Hero

A short headline (3–7 words) and minimal supporting text, centered or asymmetric, no product image.

```
┌──────────────────────────────────────┐
│                                      │
│                                      │
│                                      │
│           Hero headline              │
│           Optional subhead           │
│                                      │
│           [CTA]                      │
│                                      │
│                                      │
└──────────────────────────────────────┘
```

Use for: campaign pages, brand moments, concept launches.

Watch out:

- Don't use this without a strong headline. Empty big-type hero is "minimalism as excuse for empty page".
- Don't add three CTAs.

### 2. Full-bleed Image Hero

A large, edge-to-edge image carries the page. Minimal text overlay or text below.

```
┌──────────────────────────────────────┐
│ Nav                                  │
├──────────────────────────────────────┤
│                                      │
│                                      │
│      [ Full-bleed product photo ]    │
│                                      │
│                                      │
│   Headline (often below image)       │
│   Optional CTA                       │
└──────────────────────────────────────┘
```

Use for: product launches, feature reveals, photography-led pages.

Watch out:

- Image must be excellent. Bad photo = bad hero.
- Text must have contrast protection (scrim, shadow, or below-image placement).

### 3. Asymmetric Split Hero

Two-column or 60/40 split, with text on one side and media on the other. Apple uses this for product pages where the product is the message.

```
┌──────────────────────────────────────┐
│ Nav                                  │
├──────────────────────────────────────┤
│                                      │
│ Headline              [ Image ]      │
│ Supporting text                      │
│ [CTA] [Secondary CTA]                │
│                                      │
└──────────────────────────────────────┘
```

Use for: product detail pages, features, integration pages.

Watch out:

- Avoid the "left text + right dashboard mockup" cliché.
- The split should serve the content; don't use 50/50 if 60/40 reads better.

### 4. Centered Headline + Product

Centered text above a centered product image. Iconic Apple style.

```
┌──────────────────────────────────────┐
│ Nav                                  │
├──────────────────────────────────────┤
│                                      │
│                                      │
│           Product name               │
│           Tagline                    │
│                                      │
│      [   Product image / render ]    │
│                                      │
│           [CTA]                      │
│                                      │
└──────────────────────────────────────┘
```

Use for: product launches where the product itself is the hero.

Watch out:

- Don't crowd the product with text or controls.
- Image must be sharp and well-lit.

### 5. Cinematic Reveal

A dark or moody hero that reveals the product on scroll, often with a brief scroll-driven sequence. Used for major launches.

```
┌──────────────────────────────────────┐
│                                      │
│                                      │
│     [ Dark, atmospheric hero ]       │
│                                      │
│     (scroll triggers reveal)         │
│                                      │
└──────────────────────────────────────┘
```

Use for: very few pages per year — major launches only. Don't overuse.

Watch out:

- This is expensive. Only when the product warrants it.
- Reduced-motion users get the static version (still moody, but no scroll-driven sequence).

## Anatomy checklist

Every hero has these elements. Tick them off:

- [ ] One focal point.
- [ ] One short headline (3–7 words).
- [ ] Optional supporting line (8–18 words).
- [ ] 1–2 CTAs (not 3+).
- [ ] Optional eyebrow (only if it adds context).
- [ ] ≥ 12 vh air above the headline.
- [ ] First viewport answers what / look-at / do without scrolling.

## First viewport rules

The first 700–900 px (without scrolling) must:

1. Tell the user what this page is about (headline).
2. Tell them what to look at first (visual focal point).
3. Tell them what they can do here (CTAs).

If any of these are missing from the first viewport, the hero isn't doing its job.

## Hero composition mistakes

- Three CTAs.
- Left headline + right dashboard mockup on every page.
- Big stat trio above the fold.
- Headline + 3 feature cards stacked under hero.
- Headline below 48 px on mobile.
- Hero where the focal point is two competing elements.
- Hero that requires scrolling to understand what the page is.
