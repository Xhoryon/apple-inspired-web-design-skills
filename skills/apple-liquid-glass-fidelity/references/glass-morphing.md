# Glass Morphing Reference

The morph model for Liquid Glass. This file is the Tier 2 reference for `apple-liquid-glass-fidelity`.

Morphing is a core Fidelity 2.0 feature. The model:

```text
source material
  → geometry relationship (source rect, target rect)
  → transition (shared element OR shared surface identity)
  → destination material
```

> **HARD RULE:** Morphing must communicate **continuity, common identity, common container, or source / destination relationship.** Two unrelated glass controls should not morph just because both are glass. Morph without meaning is the rejected anti-pattern.

## The six morph transitions (Skill's design vocabulary)

The Skill publishes six transitions in its own design vocabulary. They are NOT a direct mapping to SwiftUI `GlassEffectTransition` members.

SwiftUI's `GlassEffectTransition` (see `native-liquid-glass.md` §"SwiftUI transition identity model") has exactly three documented members: `matchedGeometry`, `materialize`, `identity`. There is **no `dematerialize`, no `merge`, no `separate`, no `reshape`, no `retarget`** in SwiftUI's `GlassEffectTransition`. The Skill's `dematerialize` / `merge` / `separate` / `reshape` / `retarget` are the Skill's design vocabulary for what a glass morph SHOULD communicate. The implementer maps each to the appropriate native transition (or to a Web approximation) at implementation time.

| Skill transition | Native mapping (when used) | Web approximation |
|---|---|---|
| `materialize` | `glassEffectTransition(.materialize)` | transform + opacity fade-in |
| `dematerialize` | (Skill vocabulary; on Apple use `.materialize` in reverse / system default transition) | transform + opacity fade-out |
| `merge` | `GlassEffectContainer` + `glassEffectID` (sibling children share a sampling region; the system merges them visually) | geometry interpolation of a single shared element |
| `separate` | (reverse of merge; system behavior) | the shared element expands |
| `reshape` | `glassEffectTransition(.matchedGeometry)` (default) | transform + width/height + border-radius interpolation |
| `retarget` | `glassEffectID` + `glassEffectTransition(.matchedGeometry)` (the system animates from current to new identity) | transform + width/height interpolated from current state to new target |

The Skill does NOT publish a separate UIKit transition API; `UIGlassEffectTransition` does not exist in the installed SDK or in Apple DocC. UIKit transitions are described via `UIGlassEffect` configuration changes, not a transition class.

### 1. Materialize

The material appears. A surface enters the interaction layer with a material-specific entry, not just opacity 0 → 1.

- **Native (SwiftUI):** `glassEffectTransition(.materialize)`.
- **Web approximation:** transform (scale 0.9 → 1) + opacity (0 → 1) over a brief duration. Optionally: a soft glow that fades in.
- **When to use:** a toolbar that appears on scroll; a button that appears on hover; a sheet that appears on tap.
- **Reduced motion:** instant appearance, no scale / opacity animation.

### 2. Dematerialize

The material leaves. A surface exits the interaction layer with a material-specific exit, not just opacity 1 → 0.

- **Native (SwiftUI):** no separate `dematerialize` value. The Skill uses `glassEffectTransition(.matchedGeometry)` (default) with the reverse direction, or relies on the system default removal transition.
- **Web approximation:** transform (scale 1 → 0.9) + opacity (1 → 0) over a brief duration. Optionally: a soft glow that fades out.
- **When to use:** a toolbar that hides on scroll; a button that disappears after the action.
- **Reduced motion:** instant disappearance.

### 3. Merge

Sibling glass surfaces collapse into one shared material.

- **Native (SwiftUI):** `GlassEffectContainer` + `glassEffectID` so sibling children with the same identity are rendered as a single combined effect.
- **Native (AppKit):** `NSGlassEffectContainerView` merges descendant `NSGlassEffectView`s within `spacing` proximity.
- **Web approximation:** the two surfaces' geometry is interpolated; the smaller surface's content fades out as the larger surface's content fades in. A single shared element is the right model, not cross-fading.
- **When to use:** a segmented control that collapses into a single button; a toolbar that compresses into a single icon.
- **Reduced motion:** instant state change.

### 4. Separate

One shared material splits into siblings.

- **Native:** the reverse of merge (system behavior).
- **Web approximation:** the shared element expands; sibling content fades in as the shared content fades out.
- **When to use:** a button that expands into a toolbar; a single icon that separates into multiple.
- **Reduced motion:** instant.

### 5. Reshape

The same material changes geometry. The identity persists; the size, shape, and content shift.

- **Native (SwiftUI):** `glassEffectTransition(.matchedGeometry)` is the default transition.
- **Web approximation:** transform (translate, scale) + width / height + border-radius interpolated. Content may cross-fade if the geometry change is large.
- **When to use:** a compact toolbar that expands into a search surface; a small button that grows into a sheet.
- **Reduced motion:** instant resize.

### 6. Retarget

The same material moves between sibling targets (segmented selection).

- **Native (SwiftUI):** `glassEffectID` + `glassEffectTransition(.matchedGeometry)` — the system animates from the current state of the identity to the new target.
- **Web approximation:** transform (translateX, translateY) + width / height interpolated from the current state to the new target. The motion is the retargeting rule from `apple-motion-physics/references/retargeting.md`.
- **When to use:** segmented control selection; tab bar indicator; carousel dot indicator.
- **Reduced motion:** instant.

## Retargetable morphing (mid-flight retarget)

When the user changes target mid-morph (A → B, then B → C before settling):

- The material continues from its **current state** to C.
- The motion does NOT queue A → B → C.
- The current visual state (position, size, visual treatment, velocity) is the starting point for the new motion.

This is the retargeting rule from `apple-motion-physics`. The Skill cites, not duplicates. The implementer is responsible for ensuring the Web implementation retargets from the current state.

## Morphing must communicate meaning

A morph is justified when one or more of the following is true:

- The source and destination share an identity (e.g. a tab and its content area).
- The source and destination share a container (e.g. a `GlassEffectContainer` with related surfaces).
- The source and destination represent a continuous transformation (e.g. compact → expanded toolbar).
- The morph makes a state transition visually continuous (e.g. selection morph in a segmented control).

A morph is **not** justified when:

- The two surfaces are unrelated (e.g. a button morphing into a sheet that is not presented from it).
- The morph hides the visual transition (the user cannot tell what changed).
- The morph is decorative (no semantic content; the user is being entertained, not informed).

The candidate anti-pattern **Morph Without Meaning** (internal) records this rejection.

## Source continuity integration

A glass sheet presented from a glass button may use a morph transition. The morph is the source-continuity relationship: the sheet emerges from the button's geometry. See `apple-modality-overlays/references/source-continuity.md` for the modality-side treatment; this Skill records the material-side.

When the morph is part of a presentation:

- The source button's material identity transitions to the sheet's material identity.
- The geometry interpolates from the button's bounding box to the sheet's bounding box.
- The sheet's content fades in as the morph progresses.
- The dismiss reverses the morph (the sheet collapses back into the button).

A presentation morph that does not preserve source continuity is rejected. A presentation morph that has no source is rejected.

## Sheet / popover integration

A glass sheet or popover may emerge from a glass control. The morph is the source-continuity relationship. The Skill records:

- The morph is one-way (presentation); the dismiss is the reverse.
- The morph respects focus management (the sheet's first focusable element is focused at the end of the morph).
- The morph respects reduced motion (instant state change instead of morph).
- The morph respects unsaved-change protection (the dismiss is protected; the morph is not the dismiss).

## Reduced motion

Reduced motion reduces the morph. The morph is not removed entirely; the state change is still perceivable.

- **Materialize / dematerialize:** instant appearance / disappearance. No scale, no opacity.
- **Merge / separate:** instant state change.
- **Reshape:** instant resize.
- **Retarget:** instant position change.

The state feedback (which surface is now selected; which surface is now visible) is preserved. The motion is reduced.

## What the Skill rejects

- Morphing unrelated controls.
- Morphing that hides the visual transition.
- Decorative morphing (no semantic content).
- Cross-fading multiple glass surfaces where a single shared surface is the right model.
- A morph that exceeds 600 ms of travel time.
- A morph that breaks focus management.
- A morph that breaks unsaved-change protection.
- A claim that `GlassEffectTransition` has `dematerialize` / `merge` / `separate` / `reshape` / `retarget` members (rejected — SwiftUI's `GlassEffectTransition` has exactly `matchedGeometry`, `materialize`, `identity`; the Skill's six transitions are the Skill's design vocabulary, not SwiftUI API members).

## Cross-Skill note

- The native APIs are owned by Apple; the Skill recommends the system primitive.
- The motion physics (spring, retargeting, reduced motion) are owned by `apple-motion-physics`.
- The source-continuity semantics are owned by `apple-modality-overlays/references/source-continuity.md`.
- The shared highlight pattern is in `glass-groups.md`.
- The material state model is in `glass-interaction.md`.
- The legibility model is in `glass-legibility.md`.
- The native transition identity model is in `native-liquid-glass.md` §"SwiftUI transition identity model".