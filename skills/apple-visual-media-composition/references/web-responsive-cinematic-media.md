# Web Responsive & Cinematic Media

Web standards for visual media composition. NOT Apple API scanner entries (verified through DocC + W3C).

## HTML Living Standard

- `<picture>` — picture element with multiple sources.
- `<source srcset="..." sizes="..." media="...">` — responsive image candidate selection.
- `srcset` — image candidate set.
- `sizes` — image size hints.
- `<video poster="...">` — video key visual.
- `<img loading="lazy" decoding="async">` — lazy load + async decode.

## CSS

- `object-fit: cover | contain | fill | scale-down | none` — image scaling.
- `object-position: <length> | <percentage> | <keyword>` — image positioning.
- `aspect-ratio: <ratio>` — reserve layout space.
- `@media (prefers-reduced-motion: reduce)` — user motion preference.
- `@media (prefers-reduced-transparency: reduce)` — transparency preference.
- `@media (prefers-contrast: more)` — contrast preference.

## W3C

- `IntersectionObserver` — lazy load trigger.
- `ResizeObserver` — viewport / element resize detection.

## WAI-ARIA 1.2

- `aria-label` — accessible name.
- `aria-describedby` — description reference.
- `aria-labelledby` — label reference.
- `role="img"` — image role.
- `role="application"` — application role.
- `aria-live="polite"` / `aria-live="assertive"` — live region politeness.

## WCAG 2.2

- SC 1.1.1 Non-text Content — alt text for non-text content.
- SC 1.2.x Time-based Media — captions + audio description.
- SC 2.1.1 Keyboard — all functionality via keyboard.
- SC 2.4.7 Focus Visible — visible focus indicator.
- SC 4.1.2 Name, Role, Value — programmatic name + role + value.

## Responsive art direction on Web

Do NOT use `width: 100%` as art direction. Use `<picture>` with multiple `<source>` and `srcset` to deliver different crops / focal points / aspect ratios at different viewport sizes.

Use CSS `object-position` to control focal point within the same asset.

Use `aspect-ratio` to reserve layout space and prevent layout jump.

## Reduced motion on Web

`@media (prefers-reduced-motion: reduce)` — replace animation with:
- key pose transitions
- fades
- static states

Preserve narrative (feature ordering, explanation, destination) even when motion is reduced.

## Reference

- `references/focal-point-crop-art-direction.md` — focal point + art direction
- `references/scroll-cinematic-storytelling.md` — reversibility + reduced motion
- `references/media-loading-failure.md` — loading / failure
- `references/media-accessibility.md` — accessibility