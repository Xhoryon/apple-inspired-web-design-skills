---
name: apple-liquid-glass-fidelity
description: Use when designing or implementing advanced Liquid Glass material behavior on Native iOS / iPadOS / Mac Catalyst / macOS / tvOS / watchOS (system primitives preferred) and on Desktop Web / Touch Web / PWA (high-quality approximation). The new SwiftUI Liquid Glass family is NOT available on visionOS. Required for interactive glass control states, shared glass highlights with retarget, glass morphing between siblings or between a source and a presentation surface, material adaptivity to light / dark / image / busy backgrounds, and multi-element glass relationships. Triggers on "interactive glass", "glass morphing", "shared glass highlight", "glass that adapts to the background", "segmented glass selection morph", "sheet source continuity with glass", "GlassEffectContainer", "glassEffectID", "glassEffectTransition", "UIGlassEffect", "UIGlassContainerEffect". NOT for baseline Web material implementation (use the stable v1.1 `apple-liquid-glass-web`), not for press / selected semantics (use `apple-control-states`), not for pointer targeting (use `apple-pointer-interaction`), not for motion physics (use `apple-motion-physics`), not for navigation semantics (use `apple-navigation-spatial`), not for drag progress (use `apple-direct-manipulation`), not for sheet / popover / menu type decision (use `apple-modality-overlays`). NOT for the page background, NOT for every content card, NOT for body text containers, NOT for "make blur stronger" requests, NOT for pixel-accurate Apple.com clones.
version: "0.1.0-dev"
license: MIT
---

# apple-liquid-glass-fidelity

Liquid Glass as a dynamic material that participates in the interaction system. This Skill is the **advanced** layer above the stable v1.1 `apple-liquid-glass-web` baseline. It does not redefine what Liquid Glass is; it defines how Liquid Glass **behaves** when it is part of the interaction system.

## When to use

Use when Liquid Glass must do more than sit still:

- Glass control states (idle, hover, press, focus, selected, disabled).
- Shared glass highlight with retarget (segmented selection morph).
- Glass morphing between siblings or between a source and a presentation surface.
- Material adaptivity to light / dark / image / busy backgrounds.
- Multi-element glass relationships (groups, sampling regions, proximity).
- Native SwiftUI / UIKit Liquid Glass APIs (`GlassEffectContainer`, `glassEffectID`, `glassEffectTransition`, `UIGlassEffect`, `UIGlassContainerEffect`).

Do **not** use when:

- The task is a baseline Web material (use the stable v1.1 `apple-liquid-glass-web`).
- The task is to clone Apple.com visuals, to make every card glass, or to add "more blur" as a goal — all rejected.
- The task is to put WebGL on every surface (Shader Supremacy) — rejected.
- The task is page composition or only visual styling (use stable v1.1 Skills).

## Relationship with the stable baseline

The stable v1.1 `apple-liquid-glass-web` Skill owns:

- The "Liquid Glass ≠ glassmorphism" distinction.
- The 7-dimension model (translucency, refraction, specular, adaptivity, depth, dynamics, morphing).
- The Level 0–3 implementation capability ladder.
- The baseline Web fallback guidance.
- The default performance budget.

This Skill (`apple-liquid-glass-fidelity`) adds the **fidelity** layer:

- How each of the 7 dimensions **responds** to the interaction system.
- How material state integrates with `apple-control-states`.
- How material adaptivity integrates with the page lifecycle and user preferences.
- How multiple glass elements share sampling regions and morph.
- How source continuity integrates glass with sheet / popover presentation.
- How the material is **evaluated** (fidelity audit, not implementation level).

This Skill is **not** a replacement for the stable baseline. It is the advanced layer that uses the baseline as the substrate.

## Hard invariants

1. **Functional / interactivity layer only.** Glass belongs to the interaction layer (toolbar, tab bar, navigation chrome, segmented control, floating cluster, popover, sheet where appropriate). Rejected on body text, articles, every content card, table cells, every dashboard panel. (Inherited from v1.1.)
2. **Use sparingly.** If a glass surface can be removed and the page reads better, remove it. (Inherited from v1.1.)
3. **Platform primitive first.** Native SwiftUI Liquid Glass APIs (iOS / iPadOS / Mac Catalyst / macOS / tvOS / watchOS 26.0+ — **NOT visionOS**) provide correct behavior automatically. AppKit provides `NSGlassEffectView` / `NSGlassEffectContainerView` stable from macOS 26.0; the `effectIsInteractive` property is Beta, macOS 27.0+. UIKit provides `UIGlassEffect` / `UIGlassContainerEffect` (iOS / iPadOS / Mac Catalyst 26.0+ — not native macOS). Custom Web re-imitation is rare.
4. **Web is approximation.** No native `glassEffect()`. Tag Web implementations as APPROXIMATION.
5. **Brand remains the user's.** Replicate behavior, not palette. No "Apple blue" tint everywhere. No SF clone. No copied system apps.
6. **Distortion is restraint, not theater.** The user should notice the interface before noticing the optical trick.
7. **No glass-on-glass stacking by default.** A `GlassEffectContainer` shares a sampling region; the Web analog is a coordinated cluster, not stacked layers.
8. **Reduce Transparency / Increase Contrast / Reduce Motion degrade gracefully.** Material identity is preserved; optical cost is reduced.
9. **Capability / performance fallback is required.** Down-shift to Level 1 or Level 0 when the device cannot sustain the requested level.
10. **Apple-specific optical claims require Apple evidence.** Chromatic aberration, depth-of-field, anisotropy, caustics, lens dispersion, physically accurate glass refraction are WEB OPTICAL APPROXIMATION HYPOTHESES, classified as IMPLEMENTATION HEURISTIC / APPROXIMATION, never as Apple canonical.
11. **State semantics are owned by `apple-control-states`.** Glass describes how the material responds, not what the state is.
12. **Motion physics are owned by `apple-motion-physics`.** Glass motion (morph, retarget, specular) cites, not duplicates.
13. **The 7 stable v1.1 Web Skills are READ-ONLY. The 7 v0.2 experimental Skills are read-mostly.** This Skill consumes them; it does not redefine them.

## Material state model

The interaction system has states (idle, hover, pressed, focused, selected, disabled) owned by `apple-control-states`. Liquid Glass does **not** redefine those states; it describes how the material **responds** to them. The full state-by-state intent table is in `references/glass-interaction.md`. In short:

- `idle` — at rest, no specular movement.
- `hover` — local specular highlight (Desktop pointer only, gated by `(hover: hover)` and `(pointer: fine)`).
- `pressed` — compress / brighten / saturate; tactile feedback; fast recovery; input-sensitive amplitude.
- `focused` — visible boundary that does not depend on pointer state.
- `selected` — becomes the active identity; may trigger morphing to or from siblings.
- `disabled` — less prominent; no hover, no press, no morphing.

The motion values (spring stiffness, amplitude, duration) remain HEURISTICS owned by `apple-motion-physics`. Glass describes the perceptual intent, not the numeric values.

## Glass morphing model

Morphing is a core Fidelity 2.0 feature. The model:

```text
source material
  → geometry relationship (source rect, target rect)
  → transition (shared element OR shared surface identity)
  → destination material
```

Supported transitions: materialize, dematerialize, merge, separate, reshape, retarget. The full model with examples is in `references/glass-morphing.md`.

Hard rule: morphing must communicate **continuity, common identity, common container, or source / destination relationship.** Two unrelated glass controls should not morph just because both are glass. Morph without meaning is the rejected anti-pattern.

Native APIs: `GlassEffectContainer`, `glassEffectID`, `glassEffectTransition` (SwiftUI). UIKit equivalents. The Skill recommends the system primitive on native; on Web, morphing is APPROXIMATE.

Retargetable morphing: when the user changes target mid-morph (A → B, then B → C before settling), the material continues from its **current state** to C. The motion does NOT queue A → B → C. This is the retargeting rule from `apple-motion-physics`.

## Glass groups

A glass group is a set of glass surfaces that share a sampling region, a material identity, or a morphing relationship. Web approximation:

- **Independent glass** — a single surface; no shared identity.
- **Coordinated glass cluster** — a `GlassEffectContainer` analog. Multiple surfaces share a sampling region. On Web, a single parent with multiple children whose optical effects are computed against a shared backdrop.
- **Shared highlight glass** — segmented / cluster pattern; one highlight surface retargets between siblings.
- **Morphing group** — a set that can merge, separate, or reshape.
- **Proximity interaction** — when two surfaces approach each other, they may share a sampling region. Optional and HEURISTIC.

The Web must not assume equivalent sampling capability. Multiple nearby glass surfaces must not produce obviously inconsistent sampling. If they do, the design is broken.

## Adaptivity

The material must adapt to context. The model:

- **Light / dark background** — switch fill, border, specular treatment.
- **Colorful / busy / monochrome background** — increase background dependence or use a scrim layer for legibility.
- **Large text surface vs. compact icon surface** — choose Regular-like or Clear-like profile accordingly.
- **High contrast preference** — strengthen separation and foreground readability.
- **Reduced transparency** — switch to more opaque material; reduce background dependence.
- **Reduced motion** — retain material identity / state; reduce morph travel, bounce, shimmer, large geometry changes.
- **Light / dark appearance** — switch theme.

## Regular vs Clear profile (Web approximation)

The native Apple API offers two variants — `Regular` (default, legibility-first) and `Clear` (high translucency, see-through). The Skill records the principle; the exact API surface is owned by Apple documentation.

The two conceptual profiles for the Web:

- **Regular-like** — Web approximation of the design intent of `Regular`. Legibility first; background adaptation; stronger separation; larger textual or control surfaces. The default profile for toolbars, tab bars, segmented controls, sheet surfaces.
- **Clear-like** — Web approximation of the design intent of `Clear`. High translucency; underlying media visibility; restrained foreground content; context-dependent dimming. The profile for media overlays, image-caption bars, and the rare surface where the **point is to see through it**.

These are named with "Web approximation" in their description because the native Apple API has its own distinct Regular vs Clear distinction; the Web profiles are the closest Web equivalent. A Web implementation must NOT claim that changing opacity alone creates native Clear.

### Apple's three-condition rule for Clear (preserve explicitly)

A Clear variant is appropriate **only when ALL THREE of the following conditions hold simultaneously**:

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

Clear is NOT simply "more transparent Regular." Clear is a distinct variant with its own constraints. The Skill does not weaken this rule.

### Regular semantics (preserve)

Regular remains the **default / versatile** Liquid Glass variant. The Skill preserves:

- Broad contextual adaptability (the system adapts the surface to the backdrop).
- Stronger legibility behavior than Clear.
- Suitability for larger / text-heavier surfaces where Clear becomes risky (text legibility is the priority).

The Web approximation should NOT become a universal opacity recipe. Native Apple behavior and Web approximation remain distinct: Web may publish a `Regular-like` profile, but it is not the native `Regular` variant.

### Do not mix Regular-like and Clear-like within one design treatment

The Skill preserves Apple's literal rule: **Never mix Regular and Clear in the same interface / design treatment.** Choose one variant and stick with it for visual consistency.

On the Web: **Do not mix Regular-like and Clear-like within one design treatment.** Mixing the two profiles casually within the same control cluster, screen, or design treatment breaks visual consistency and is rejected. This rule is the Web-approximation equivalent of Apple's literal rule.

The rule is not weakened to "avoid mixing when possible." It is preserved. If a future Apple source revises this guidance, the rule is updated by source-matrix revision; until then, it stands.

## Web optical approximation hypotheses

These techniques are **implementation heuristics** for the Web. They are not Apple behavior. The Skill evaluates each on a hypothesis-lab matrix in `references/optical-fidelity.md`:

- CSS `backdrop-filter` (translucency, blur, saturate).
- SVG `feDisplacementMap` (refraction approximation).
- SVG `feTurbulence` (noise source for displacement).
- CSS radial gradients (specular response).
- WebGL / WebGPU fragment shader (true optical approximation).
- DOM / background duplication with sampling (rare, expensive).

Chromatic aberration, depth-of-field, anisotropy, caustics, lens dispersion, and Fresnel simulation are evaluated as WEB OPTICAL APPROXIMATION HYPOTHESES (APPROXIMATION / IMPLEMENTATION HEURISTIC). They must NOT be presented as Apple canonical. Techniques that produce RGB fringe, gaming-UI feel, or call attention to themselves are rejected or downgraded.

## Performance & capability

The Skill records a conceptual **Glass Quality Governor** that may downshift the implementation level when:

- The browser does not support the requested filter / shader.
- The device reports low-power mode or constrained GPU.
- `Save-Data` is requested.
- `prefers-reduced-transparency` is set.
- `prefers-reduced-motion` is set.
- The number of concurrent glass surfaces exceeds the budget.
- The interaction frequency is too high (pointer-driven effects on many surfaces).

Downshift ladder (per the stable baseline): Level 3 → Level 2 → Level 1 → Level 0. A well-designed Level 1 is often more appropriate than a theatrical Level 3.

## Implementation capability vs fidelity profile

The stable baseline's Level 0–3 is an **Implementation Capability Level** (what the browser can do). This Skill adds a separate **Liquid Glass Fidelity Profile** (a multidimensional score across Material, Optical, Adaptivity, Interaction, Morphing, Hierarchy / Semantic, Accessibility, Performance). The two are independent: a Level 1 implementation can achieve high Material, Adaptivity, Interaction, and Accessibility fidelity even though it has low Optical fidelity. The skill recommends that the implementation capability should match the design intent, not the other way around.

## Reference catalog (Tier 2)

- `references/material-fidelity.md` — translucency, edge definition, separation, regular vs clear, legibility, scrim strategy.
- `references/optical-fidelity.md` — hypothesis lab, technique matrix, restraint, what is rejected.
- `references/glass-groups.md` — coordinated cluster, shared highlight, morphing group, proximity interaction.
- `references/glass-interaction.md` — state-by-state material response; hover, press, focus, selected, disabled, loading.
- `references/glass-morphing.md` — materialize / dematerialize / merge / separate / reshape / retarget; retargetable morphing; morphing without meaning rejected.
- `references/glass-legibility.md` — foreground contrast, background complexity, scrim, dimming, label / icon complexity, surface size.
- `references/web-capability-fallback.md` — feature detection, capability ladder, Glass Quality Governor, fallback transitions.
- `references/native-liquid-glass.md` — current SwiftUI / UIKit / AppKit API surface; system-primitive-first routing.
- `references/fidelity-audit.md` — fidelity profile rubric; semantic placement, hierarchy, material response, optical restraint, context adaptation, interaction continuity, morph continuity, accessibility, performance, brand preservation.

## Hard invariants (recap)

1. Functional / interactivity layer only (HARD).
2. Use sparingly (HARD).
3. Platform primitive first (HARD).
4. Web is approximation (HARD).
5. Brand remains the user's (HARD).
6. Distortion is restraint, not theater (HARD).
7. No glass-on-glass stacking by default (HARD).
8. Accessibility degrades gracefully (HARD).
9. Capability / performance fallback required (HARD).
10. Apple-specific optical claims require Apple evidence (HARD).
11. State semantics owned by `apple-control-states`; motion physics owned by `apple-motion-physics` (HARD).
12. The 7 stable v1.1 Skills READ-ONLY; v0.2 Skills read-mostly (HARD).

## Companion files

- `references/material-fidelity.md`
- `references/optical-fidelity.md`
- `references/glass-groups.md`
- `references/glass-interaction.md`
- `references/glass-morphing.md`
- `references/glass-legibility.md`
- `references/web-capability-fallback.md`
- `references/native-liquid-glass.md`
- `references/fidelity-audit.md`
