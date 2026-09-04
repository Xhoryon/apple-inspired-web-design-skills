# Material Fidelity Reference

The Material Fidelity dimension of the Liquid Glass Fidelity Profile. This file is the Tier 2 reference for `apple-liquid-glass-fidelity`. The profile is multidimensional; Material Fidelity is one of eight dimensions (see `fidelity-audit.md`).

Material Fidelity is not a CSS property checklist. It is the set of attributes that make a glass surface feel like a **material** rather than a flat color or a static gradient.

## What Material Fidelity covers

A glass surface with high Material Fidelity expresses:

- **Translucency** — light passes through; the layer behind affects the surface's color and luminance.
- **Edge definition** — the surface is visibly bounded; the boundary contributes to the user's understanding of the surface as a discrete object.
- **Separation from content** — the surface is on the interaction layer, not the content layer.
- **Apparent thickness** — the surface has a sense of depth that varies with context (size, role, light source).
- **Luminosity adaptation** — the surface's tone shifts based on the underlying content's brightness and color.
- **Color infusion** — the surface may subtly take on the color of the layer behind it.
- **Tint** — a deliberate, semantic, restrained overlay color (e.g. selected state, primary action).
- **Regular vs clear variant** — the surface uses the right variant for its context (regular-like for legibility, clear-like for media visibility).

A glass surface that achieves 5+ of these attributes is materially expressive. One that achieves 2 or fewer is decoration.

## Translucency

A translucent surface is not a fixed alpha. The alpha is **adaptive**:

- Light background — slightly higher alpha or warmer fill.
- Dark background — slightly higher alpha or cooler fill.
- Image background — a theme-aware fill that goes opaque on busy regions, or a scrim layer under the text region.
- Monochrome background — a neutral fill.

The stable v1.1 `apple-liquid-glass-web` documents the Web approximation. This Skill records the principle: **translucency adapts; it is not a constant.**

## Edge definition

A glass surface must be visibly bounded. The boundary can be:

- A 1 px hairline border (sub-pixel, theme-aware).
- An inset highlight (top edge) and a slight shadow (bottom edge).
- A subtle outer drop shadow.
- A combination of the above.

A glass surface with no visible edge is decoration; the user cannot tell where it ends. A surface with too many edges (stacked shadows) is over-defined; the user notices the chrome.

## Separation from content

A glass surface is on the **interaction layer**, not the content layer. The stable v1.1 baseline already enforces this. The Skill records the principle for Fidelity:

- The surface must be visually distinct from the content beneath it.
- The distinction comes from the material (translucency, edge, light response), not from a heavy border or a static background.
- Glass over glass is rejected. A glass sheet on top of a glass toolbar is wrong.

## Apparent thickness

A real glass surface has a sense of thickness. On Apple platforms, the system primitive handles this. On the Web, the Skill recommends:

- A subtle inner highlight (top edge) and inner shadow (bottom edge).
- A very subtle outer shadow that suggests the surface floats.
- A `border-radius` that matches the surface's role (larger for toolbars, smaller for compact controls).

Apparent thickness is not a numeric value. The implementer chooses the values; the principle is hard.

## Luminosity adaptation

The surface's tone shifts based on the layer behind it. On Apple platforms, the system primitive handles this. On the Web, the implementer can:

- Use CSS variables driven by parent class (`.on-light`, `.on-dark`, `.on-image`).
- Use a small JS luminance sampler for hero surfaces.
- Use CSS `light-dark()` if available.

The implementer must not paint a constant fill. A glass surface that looks the same on every background is not adaptive.

## Color infusion

The surface may take on the color of the layer behind it. On the Web, this is implicit in `backdrop-filter` (the filter samples the backdrop and applies the effect). The implementer must not tint the fill so heavily that the color of the backdrop is lost.

## Tint

Tint is a deliberate, semantic, restrained overlay color. Tint is for:

- **Selected state** — the surface becomes the active identity.
- **Primary action** — the surface is the call-to-action.
- **Destructive action** — the surface warns of an irreversible action.
- **Brand emphasis** — rare; the user's brand may live in a single accent, not everywhere.

Tint is NOT:

- A constant color applied to every glass surface.
- Apple blue.
- A way to make the surface look "Apple-y".

The user sees the tint only when the tint is semantically meaningful.

## Regular vs Clear variant

The native Apple API offers Regular and Clear variants. The Skill records the principle; the exact API surface is owned by Apple documentation.

- **Regular** — legibility first; stronger background adaptation; more visible edge; more opacity. Default for toolbars, tab bars, segmented controls, sheets, popovers, and any surface where the user reads text or interacts with controls.
- **Clear** — high translucency; underlying media visibility; restrained foreground content. Used for media overlays, image-caption bars, and the rare surface where the point is to see through it. Clear is NOT simply "more transparent Regular" — it is a distinct variant with its own constraints.

On the Web, the Skill publishes two **Web approximation profiles** that mirror the design intent:

- **Regular-like** — Web approximation of the design intent of `Regular`. Legibility first; the user reads text on the surface easily; the surface is comfortably opaque enough to be a control.
- **Clear-like** — Web approximation of the design intent of `Clear`. High translucency; the user sees the media through the surface; the surface is decorative or carries minimal foreground.

These are named with "Web approximation" in their description. The names make clear the Web profiles are not claims of equivalence to the native variants. A Web implementation must NOT claim that changing opacity alone creates native Clear.

### Apple's three-condition rule for Clear (preserve explicitly)

A Clear variant (native or Clear-like on the Web) is appropriate **only when ALL THREE of the following conditions hold simultaneously**:

1. The Liquid Glass element is positioned over **media-rich content** (photograph, illustration, video, map, or other visually dense backdrop).
2. Introducing the required **dimming layer** does not negatively harm the content layer beneath the glass.
3. The **foreground content above the glass is bold and bright** enough to remain legible against the see-through + dimming effect.

Decision logic for Clear:

```text
Clear candidate
  → condition 1 (over media-rich content)?
  → condition 2 (dimming layer does not harm content)?
  → condition 3 (foreground is bold and bright)?
  → all three yes: potentially appropriate
  → any condition no: use Regular / another material / adapt
```

The Skill does not weaken this rule. Clear is NOT "more transparent Regular." A large text-heavy settings panel over arbitrary content fails all three conditions and is REJECTED for Clear (use Regular-like, or a standard material).

### Regular semantics (preserve)

Regular remains the **default / versatile** Liquid Glass variant. The Skill preserves:

- Broad contextual adaptability (the system adapts the surface to the backdrop).
- Stronger legibility behavior than Clear.
- Suitability for larger / text-heavier surfaces where Clear becomes risky (text legibility is the priority).

The Web approximation should NOT become a universal opacity recipe. Native Apple behavior and Web approximation remain distinct: Web may publish a `Regular-like` profile, but it is not the native `Regular` variant.

### Do not mix Regular-like and Clear-like within one design treatment

The Skill preserves Apple's literal rule: **Never mix Regular and Clear in the same interface / design treatment.** Choose one variant and stick with it for visual consistency.

On the Web: **Do not mix Regular-like and Clear-like within one design treatment.** Mixing the two profiles casually within the same control cluster, screen, or design treatment breaks visual consistency and is rejected. This rule is the Web-approximation equivalent of Apple's literal rule.

The rule applies at every level of the surface: in a single surface, in a control cluster (segmented / shared highlight), in a sheet, in a popover, and across a design treatment. **Within one Regular-like segmented control the selected pill is Regular-like with a different tint, foreground emphasis, border, or shared highlight geometry — it is NOT Clear-like.** Within one sheet the trailing edge is NOT a Clear-like variant patch. The whole control cluster / surface / design treatment uses one variant.

The rule is not weakened to "avoid mixing when possible." It is preserved. If a future Apple source revises this guidance, the rule is updated by source-matrix revision; until then, it stands.

The variant for a context is determined once for the whole control cluster / surface / design treatment:

- A floating toolbar → **Regular-like**, not Clear-like.
- A media overlay (over photos / video / maps with bold foreground) → **Clear-like**, not Regular-like.
- A segmented control → **Regular-like** (and the selected pill is Regular-like with selection state via tint / foreground / shared highlight).
- An image-caption bar → **Clear-like**.
- A tab bar → **Regular-like**.
- A popover (anchored to a source) → **Regular-like by default**. Clear-like is rejected (the popover is not over media in the usual case).
- A sheet → **Regular-like**. Clear-like is rejected.
- A compact icon toolbar with large icons and a non-text heavy context → **Regular-like**. Clear-like is rejected even with large icons, because Clear-like depends on condition 1 (media-rich backdrop) and condition 3 (bold foreground), not on icon size alone.

Wrong mapping is a defect. A Clear-like floating toolbar is wrong (the user cannot read the labels). A Regular-like image caption overlay is wrong (the user cannot see the media through it). A Clear-like selected pill inside a Regular-like segmented control is wrong (it violates the no-mix rule and bypasses condition 1 of the three-condition gate).

## Surface size and context (within one variant family)

The surface's role affects the Material Fidelity choices. The variant (Regular-like or Clear-like) is chosen once for the whole control cluster / surface / design treatment; the size and context then inform fill, blur, edge, and tint within that one family.

- **Large text surface** (navbar with search field) — Regular-like, scrim layer under text, larger blur, more visible edge.
- **Compact icon surface** (toolbar of icons) — Regular-like. Selection is expressed via tint, foreground emphasis, border, or shared highlight geometry within the same variant.
- **Tab bar** — Regular-like. The selected tab may have a slightly different tint.
- **Sheet** — Regular-like throughout. The sheet does not mix Clear-like into its trailing edge; any emergent edge effect is part of the same Regular-like treatment.
- **Popover** — Regular-like by default. Clear-like is rejected (the popover is not over media in the usual case).
- **Segmented control** — Regular-like container; the selected pill is also Regular-like. Selection state is expressed via tint, foreground, border, or shared highlight geometry — NOT by introducing a Clear-like pill. The same variant applies throughout the cluster.
- **Media overlay (Clear-like)** — only when all three Apple conditions hold. Selection state within a Clear-like cluster uses tint / foreground / shared highlight within the same Clear-like variant.
- **Image-caption bar (Clear-like)** — only when the bar floats over media with bold foreground and the dimming layer does not harm the media.

Selection-state techniques within one material family (no variant mixing):

- **Tint** — a deliberate semantic overlay color (selected / primary / destructive / brand emphasis). Tint is restrained and semantically meaningful.
- **Foreground treatment** — text / icon color and weight change for the selected item.
- **Luminance treatment** — slightly brighter or darker fill on the selected item within the same variant.
- **Border / highlight** — a 1 px hairline border or an inset highlight on the selected item.
- **Shared geometry** — a single shared highlight surface (one element, not multiple) that retargets between sibling targets within the same variant cluster.
- **Semantic selection state** — the `selected` state is persistent, distinct from hover and focus, and survives pointer exit.

These techniques preserve one material family within a cluster while expressing selection, focus, and hover. Mixing in a different variant family within the same cluster is rejected.

## Legibility interaction

Material Fidelity is meaningless if the user cannot read the text. The legibility reference (`glass-legibility.md`) covers the legibility model. Material Fidelity without legibility is decoration.

## When Material Fidelity is wrong

- A surface that looks the same on every background (not adaptive).
- A surface with no visible edge (decoration, not material).
- A surface that uses Apple blue on every state (not semantic).
- A surface that is Clear-like where the user needs to read (wrong variant for the context).
- A surface that is Regular-like where the user needs to see the media through it (wrong variant for the context).
- A surface that mixes Regular-like and Clear-like within one control cluster, sheet, popover, or design treatment (mixing violation).

## What the Skill rejects

- Glass on body text, articles, every content card, table cells, every dashboard panel. (Inherited from v1.1.)
- Constant fill (no adaptive translucency).
- Heavy tints that erase the backdrop color.
- "Apple blue" tint everywhere.
- Heavy borders / shadows that call attention to the chrome instead of the content.
- Mixing Regular-like and Clear-like within one design treatment, within one control cluster, within one sheet, within one popover, or within one segmented control. Selection is expressed via tint / foreground / luminance / border / shared highlight / semantic state — NOT via variant-family mixing.

## Cross-Skill note

- The 7-dimension model (translucency, refraction, specular, adaptivity, depth, dynamics, morphing) is inherited from the stable v1.1 `apple-liquid-glass-web`. Material Fidelity is the deepening of those dimensions into a profile.
- The state semantics (idle, hover, press, focus, selected, disabled) are owned by `apple-control-states`. The Material state model is in `glass-interaction.md`.
- The adaptivity (light / dark / image / busy) is owned by this Skill's `glass-interaction.md` and the platform-applicability matrix.
- The legibility model is in `glass-legibility.md`.
- The optical techniques (refraction, specular) are in `optical-fidelity.md`.
- The group-level rule (no variant mixing within a cluster) is in `glass-groups.md`.
- The selection-state techniques within one material family are in `glass-interaction.md`.
