# Color and Depth Reference

Extended recipes for Apple-inspired color and depth. Builds on `apple-design-foundations/SKILL.md`.

## The neutral axis

Apple's palette is dominated by a single neutral axis. Color comes from imagery.

```css
:root {
  /* Light mode neutrals */
  --bg-page:        #FBFBFD;       /* off-white page */
  --bg-elevated:    #FFFFFF;       /* elevated surface */
  --bg-subtle:      #F5F5F7;       /* subtle background variation */
  --bg-tinted:      #F2F2F4;       /* slightly tinted (cards on tinted page) */
  --border-hairline: rgba(0, 0, 0, 0.08);
  --border-strong:  rgba(0, 0, 0, 0.16);

  --text-primary:   #1D1D1F;       /* near-black, not pure */
  --text-secondary: #424245;       /* ~60% contrast */
  --text-tertiary:  #6E6E73;       /* metadata / eyebrow */
  --text-on-accent: #FFFFFF;
}

@media (prefers-color-scheme: dark) {
  :root {
    --bg-page:        #000000;
    --bg-elevated:    #1C1C1E;
    --bg-subtle:      #1D1D1F;
    --bg-tinted:      #2C2C2E;
    --border-hairline: rgba(255, 255, 255, 0.10);
    --border-strong:  rgba(255, 255, 255, 0.20);

    --text-primary:   #F5F5F7;
    --text-secondary: #AEAEB2;
    --text-tertiary:  #8E8E93;
  }
}
```

Rules:

- Pure `#FFFFFF` and `#000000` are reserved for small accents (text on buttons, brand mark). Page surfaces are off-white / near-black.
- Body text never pure black on white; never 30% opacity grey for secondary copy. Aim for ~60% contrast for secondary.
- Hairline borders must be subtle — rgba alpha 0.06–0.12 in light, 0.08–0.14 in dark. Anything more is a "border", not a hairline.

## The single accent

One accent color, used semantically:

```css
:root {
  --accent:        #0066CC;   /* Apple's blue, used semantically */
  --accent-hover:  #0077ED;
  --accent-press:  #0050A3;
}
```

Apple uses blue for primary actions and links. Your product may use any color, but:

- One accent only. Not a primary and a secondary and a tertiary.
- Accent is for: primary CTA, link color, focus ring on interactive elements, active tab indicator. That's it.
- Don't tint cards, backgrounds, icons, hover states with accent color. Accent is rare.
- Accent must be tested for accessibility. Apple's `#0066CC` on white passes AA; on `#1D1D1F` it does not — switch to lighter blue or add an underline.

## When imagery carries color

Apple pages often have **no other accent** because the product photography carries the color. If your page is image-heavy:

- Let the imagery provide the color palette.
- The accent becomes neutral (text-primary or a single brand color used very sparingly).
- Backgrounds stay neutral so the imagery pops.

## Dark mode is a first-class theme

Don't treat dark mode as `background: #000; color: #fff`. It's a re-design of the entire palette:

- Backgrounds are not pure black — `#000` to `#1C1C1E` is typical.
- Text is off-white, not pure white.
- Accents may need lighter variants for dark mode (Apple uses `#2997FF` blue in dark mode).
- Shadows are different — dark mode surfaces use very subtle inner highlights instead of outer shadows.
- Glass surfaces are darker in dark mode; their backdrop-filter saturate may be lower.

## Contrast

| Element | Light | Dark |
|---|---|---|
| Body text on bg | ≥ 4.5:1 (AA), aim 7:1 (AAA) | ≥ 4.5:1 |
| Large display text | ≥ 3:1 | ≥ 3:1 |
| UI components (buttons, icons) | ≥ 3:1 | ≥ 3:1 |
| Hairline borders | ≥ 1.5:1 | ≥ 1.5:1 |

Use `prefers-contrast: more` to switch to higher-contrast variants for users who request it.

```css
@media (prefers-contrast: more) {
  :root {
    --text-secondary: #2C2C2E;     /* darker in light mode */
    --border-hairline: rgba(0, 0, 0, 0.20);   /* stronger */
  }
}
```

## Color and depth

Depth comes from separation of concerns, not from color.

### Five sources of depth

1. **Content / control separation.** A floating control over content reads as foreground.
2. **Material.** Glass, paper, video.
3. **Overlap.** Toolbar overlapping image.
4. **Scale.** Larger objects feel closer.
5. **Subtle shadow.** Last resort, used very sparingly.

### Shadow recipes

Apple's shadows are subtle. One ambient, one key. Low opacity, large blur, small offset.

```css
:root {
  --shadow-sm:
    0 1px 2px rgba(0, 0, 0, 0.04),
    0 1px 1px rgba(0, 0, 0, 0.04);

  --shadow-md:
    0 4px 12px rgba(0, 0, 0, 0.06),
    0 1px 2px rgba(0, 0, 0, 0.04);

  --shadow-lg:
    0 10px 30px rgba(0, 0, 0, 0.10),
    0 1px 2px rgba(0, 0, 0, 0.04);
}

@media (prefers-color-scheme: dark) {
  :root {
    --shadow-sm:
      0 1px 2px rgba(0, 0, 0, 0.40),
      0 1px 1px rgba(0, 0, 0, 0.30);

    --shadow-md:
      0 4px 12px rgba(0, 0, 0, 0.50),
      0 1px 2px rgba(0, 0, 0, 0.30);

    --shadow-lg:
      0 10px 30px rgba(0, 0, 0, 0.60),
      0 1px 2px rgba(0, 0, 0, 0.40);
  }
}
```

Apply **one** shadow level per element. Don't stack `shadow-sm` and `shadow-md`.

### Glow

Apple does not use colored glow. Avoid `box-shadow: 0 0 40px rgba(120, 80, 255, 0.6)`. The exception is a focus ring on an accent button:

```css
:focus-visible {
  outline: 2px solid var(--accent);
  outline-offset: 2px;
}
```

### Inset highlight

For glass / floating surfaces, a 1 px inset highlight at the top edge:

```css
.floating {
  box-shadow:
    inset 0 1px 0 rgba(255, 255, 255, 0.5),
    0 10px 30px rgba(0, 0, 0, 0.10);
}
```

For dark mode, the inset is much more subtle:

```css
.floating-dark {
  box-shadow:
    inset 0 1px 0 rgba(255, 255, 255, 0.08),
    0 10px 30px rgba(0, 0, 0, 0.50);
}
```

## What color is NOT

- A purple-blue gradient hero.
- An accent on every interactive element.
- Greying out secondary text to 30% opacity (unreadable).
- Multiple accent colors competing.
- Random glow halos.
- Stacked shadows on flat cards.

## Accessibility

- All text must pass WCAG AA against its background.
- `prefers-contrast: more` increases contrast.
- `forced-colors` (Windows High Contrast) must be respected — test with the system colors override.
- Color is never the only indicator of state. Pair color with icon, label, or shape.
