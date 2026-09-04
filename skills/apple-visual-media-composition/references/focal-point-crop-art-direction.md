# Focal Point, Crop & Art Direction

## Focal point

For important media identify:
- **Primary subject** — what the eye should land on.
- **Protected region** — pixels that must remain visible across crops.
- **Safe text region** — if text overlay is justified.
- **Crop tolerance** — how much the image may be safely cropped.
- **Minimum recognizable context** — smallest context the user needs to recognize the subject.

Focal point should drive responsive art direction. Coordinate with `apple-adaptive-structure`.

## Crop policy follows role

- **Atmospheric media** can often tolerate stronger crop.
- **Product proof** must preserve key evidence.
- **Preview** prioritizes identity recognition.
- **Inspection** preserves content and lets the user control zoom / pan.
- **Working content** does NOT silently crop meaningful data.

Do NOT turn these examples into universal formulas.

## Responsive art direction

Hard invariant: **Responsive media design ≠ width: 100%.**

Different contexts may require:
- different crop
- different focal point
- alternate asset
- alternate media orientation
- text / media order change
- image-first vs text-first change
- cinematic → static fallback
- 3D → sequence / video / static fallback

## Web primitives

- HTML Living Standard: `<picture>`, `<source>`, `srcset`, `sizes`.
- CSS Images: `object-fit` (cover / contain / fill / scale-down / none), `object-position`.
- CSS Sizing: `aspect-ratio`.
- W3C: `IntersectionObserver` for lazy loading.

## Reference

- `references/static-media-composition.md` — large centered / split / full-bleed
- `references/web-responsive-cinematic-media.md` — Web primitives
- `references/media-role-architecture.md` — Media Role Contract