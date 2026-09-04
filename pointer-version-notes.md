# Pointer Interaction — Version Notes

**Date:** 2026-09-02
**Status:** Public version-sensitive reference

This file records the version-sensitive distinctions in Apple's pointer behavior. It exists because the previous closeout pattern showed: Apple-system claims without version tagging can be silently wrong on different OS versions.

## Two distinct generations

### Generation A — Earlier iPadOS (≤ 25 / pre-iPadOS 26)

Approximate behavior (drawn from HIG Pointing Devices prior to iPadOS 26):

- A pointer enters a region; the system identifies the active target within that region.
- A highlight surface materializes over the active target.
- A "lift" effect raises the highlighted target slightly (visual elevation, often via shadow or scale).
- A separate "hover" state may apply to elements designed for hover (e.g. links).
- "Magnetism" or "pointer transformation" may apply — the pointer or the target exhibits attraction / snap behavior near edges.
- The system manages the highlight as a separate layer that retargets to a new target when the pointer moves between controls.

These behaviors are described in HIG Pointing Devices prior to iPadOS 26. The exact numeric parameters are not published by Apple.

### Generation B — iPadOS 26+

Approximate behavior (from WWDC25 "Elevate the design of your iPad app" + iPadOS 26 release notes):

- A more direct 1:1 pointer tracking: the highlight responds immediately to pointer movement, not after a delay or snap.
- A separate highlight surface is still used; the highlight retargets from its current visual state when the pointer moves between targets (no "finish A then start B" sequence).
- Lift and magnetism are reduced; the system relies more on highlight retargeting than on physical-elevation metaphors.
- The pointer itself is no longer kidnapped; the cursor remains a normal system cursor; the highlight retargets around it.

This is a version-specific system behavior. It applies to iPadOS 26 and later. It does NOT necessarily apply to earlier iPadOS.

## Forbidden merge

> A Skill must NOT write "Apple pointer behavior is X" without specifying the version it refers to.

Both generations are valid in their own context. The Skill applies:

- **Native iPadOS 26+** → Generation B (direct 1:1 highlight retargeting).
- **Native earlier iPadOS / pre-iPadOS-26** → Generation A (highlight, lift, hover, magnetism).
- **Web desktop** → APPROXIMATE: pointer highlight on a target surface; do NOT replace the native cursor; do NOT add magnetism that yanks the cursor.
- **Web touch-only** → ABANDON the pointer model; use press + selection + touch feedback instead.
- **macOS** → Generation A semantics with system cursor unchanged; no Liquid Glass highlight surface unless the project ships its own.

## Web approximation rules

When the Web approximates Apple pointer behavior:

1. **Do not replace the system cursor.** The native cursor remains user-controlled. The Skill implements highlight surfaces that retarget around the cursor.
2. **Do not add cursor magnetism.** Cursor kidnapping breaks user control. If target acquisition is desired, animate the highlight's snap to a new target — not the cursor.
3. **Pointer modality gating.** Use `@media (hover: hover) and (pointer: fine)` to gate pointer-specific behavior. Touch-only devices must not receive hover effects.
4. **Highlight retargeting from current state.** When the pointer moves from target A to target B, animate the highlight from its current visual position and state to the new target's position. Do not "finish A then start B".
5. **Pointer exit resolution.** When the pointer leaves all targets, the highlight dissolves. If a target is selected, the selection state persists.
6. **Selection ≠ hover.** Hover is transient. Selection is persistent. The two must be visually distinct.
7. **Focus ≠ hover.** Keyboard focus is accessibility-required. Focus indication must not depend on pointer state.

These rules are **APPROXIMATION** (not Apple behavior) and are subject to the `version-sensitive` annotation in `APPLE-EXPERIENCE-SOURCE-MATRIX.md` for the iPadOS 26 case (which is system behavior, not approximation).
