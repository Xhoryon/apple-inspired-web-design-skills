---
name: apple-pointer-interaction
description: Use when designing or implementing the relationship between a pointer (mouse / trackpad / touch-as-pointer) and a target surface — hover, target highlight, shared highlight, target retargeting, pointer exit, and the difference between hover / focus / selection. Applies to Native iPadOS 26+ (system behavior), Desktop Web (APPROXIMATE), and ABANDON on touch-only. Required for "segmented control highlight follows pointer", "iPad-style pointer highlight", "tab cluster hover highlight", and similar tasks. Do NOT use for control state semantics (use apple-control-states) or for motion physics (use apple-motion-physics). Do NOT use to implement pointer magnetism or cursor kidnapping on Web.
version: "0.1.0-dev"
license: MIT
---

# apple-pointer-interaction

The pointer ↔ target relationship. Distinguishes hover, focus, selection. Handles retargeting. Refuses pointer magnetism on Web.

## When to use

Use when the task involves a pointer interacting with a target surface. Symptoms:

- "Add an iPadOS-style pointer highlight to this Web component."
- "Make the segmented control highlight follow the pointer."
- "When the pointer moves between tabs, animate the highlight from where it was to where it is."
- "The hover state should not appear on touch-only."
- "When the pointer leaves, the highlight should dissolve but the selected state should remain."

Do **not** use when:

- The task is the control state model. Use `apple-control-states`.
- The task is the motion physics of the highlight transition. Use `apple-motion-physics`.
- The task is page-level composition. Use the stable `apple-web-design`.
- The user wants pointer magnetism (cursor snap to target). On Web, this is **ABANDON**. See `pointer-version-notes.md` and `references/cursor-rule.md`.

## Hard invariants

1. **Native iPadOS 26+ is system behavior.** On iPadOS 26+ with a pointer (mouse, trackpad, Magic Keyboard trackpad), Apple provides a highlight surface that retargets from its current state. The Skill recommends the system primitive (`UIHoverEffect` / `UIPointerInteraction`) on native, not a custom re-implementation.
2. **Desktop Web is APPROXIMATE.** The Web Skill animates a highlight surface around the native cursor. It MUST NOT replace the native cursor, add cursor magnetism, or add pointer lag.
3. **Touch-only is ABANDON.** When the input is touch-only (no hover capability), the pointer concept does not apply. The Skill's verdict for touch is `ABANDON`, and the implementation substitutes press + selection + touch feedback (via `apple-control-states`).
4. **Pointer highlight is a separate surface.** On all platforms where it is implemented, the highlight is a separate visual surface that retargets when the pointer moves between targets. It is NOT the cursor, the target, or the target's color.
5. **Hover ≠ focus ≠ selection.** A pointer highlight is `hover`. Keyboard focus is `focused`. Persistent choice is `selected`. They are distinct states with distinct visual treatments.

## Generation distinction (do NOT merge)

There are two distinct generations of Apple pointer behavior. The Skill must NOT merge them into a single "Apple pointer" rule.

| Generation | Platform | Behavior |
|---|---|---|
| **A — Earlier iPadOS (≤ 25 / pre-iPadOS 26)** | Older iPadOS | Highlight, lift, hover, pointer transformation, magnetism. |
| **B — iPadOS 26+** | iPadOS 26+ | More direct 1:1 pointer tracking, separate highlight surface, retarget from current state. Reduced lift and magnetism. |

A Skill that recommends a behavior on iPadOS 26+ must NOT generalize to "Apple pointer" and apply it to earlier iPadOS or to macOS pre-iPadOS-26. Conversely, a behavior on earlier iPadOS must NOT be claimed as current. Both are valid in their own context. The Skill applies the version-appropriate generation.

See `pointer-version-notes.md` in the apple-experience-foundation/ directory for the full version-sensitive distinction.

## Hover / focus / selection distinct

Three states. Three visual treatments. They MAY share a color family but they MUST have distinct visual cues.

- **hover** — transient, pointer-driven, fades on pointer exit.
- **focus** — accessibility-required, keyboard-driven, must be visible, independent of hover.
- **selected** — persistent, the user's chosen state, survives pointer exit.

A pointer highlight implementation that conflates any two of these is wrong.

## Pointer modality gating (Web)

```css
@media (hover: hover) and (pointer: fine) {
  /* Desktop Web with fine pointer. Pointer effects apply. */
}
@media not (hover: hover) {
  /* Touch-only. Pointer effects do NOT apply. */
}
```

A Web pointer highlight MUST be gated by hover + fine pointer capability. Touch-only devices MUST NOT show the highlight.

## Cursor non-replacement (Web, HARD)

> On Web, the native cursor remains user-controlled. The Skill implements highlight surfaces that retarget around the cursor. The Skill does NOT replace the native cursor.

This is a hard rule. The pointer highlight is the moving visual element. The cursor is a separate user-controlled element. The Skill MUST NOT:

- Replace the native cursor with a custom one.
- Add cursor magnetism (cursor snap to target).
- Add cursor lag (slowing the cursor to give the highlight time to catch up).
- Add fake cursor trajectory (parabolic or eased cursor motion).

If target acquisition is desired, animate the highlight's snap to a new target — not the cursor's motion.

## Shared highlight

A shared highlight is a single visual surface that animates between targets in a cluster. Use cases:

- Segmented control
- Tab bar
- Toolbar
- Compact navigation cluster
- Filter chip cluster

Do NOT use shared highlight for:

- Large card grids
- Data table rows
- Whole-page sections

The shared highlight retargets from its current state when the active target changes. It does NOT queue animations: A finishes, then B starts. Instead, it retargets mid-flight: the current visual state becomes the starting state of the new transition.

## Retargeting (rule)

When the pointer moves from target A to target B, the highlight animates from its current visual state to B's geometry. The current state is the starting point. This is the retargeting rule.

A Skill that queues (A finishes, then B starts) is wrong. A Skill that resets the highlight to a default state on every target change is also wrong (loses visual continuity).

## Selection ≠ hover ≠ focus (Web pointer highlight)

The Web pointer highlight (transient, follows pointer) is distinct from the Web selected indicator (persistent, marks the user's choice) and the keyboard focus indicator (accessibility, marks the focused element). Three visual treatments. Three semantic roles.

A Skill that uses the pointer highlight as a focus indicator is wrong (focus must be visible without pointer state). A Skill that uses the pointer highlight as the selected indicator is wrong (selected must persist after pointer exit).

## Reference catalog (Tier 2)

- `references/shared-highlight.md` — shared highlight implementation and use cases.
- `references/retargeting.md` — retargeting from current state.
- `references/hover-vs-focus-vs-selection.md` — three distinct states, visual treatment rules.
- `references/cursor-rule.md` — Web cursor non-replacement rule.
- `references/pointer-modality.md` — pointer modality gating for Web.
- `references/pointer-version-notes.md` — pointer version-sensitive distinction (links to apple-experience-foundation/pointer-version-notes.md).

## Hard invariants (recap)

1. iPadOS 26+ system primitive on native (HARD).
2. Web highlight is APPROXIMATE, not system behavior (HARD).
3. Touch-only Web is ABANDON (HARD).
4. Native cursor on Web is not replaced (HARD).
5. Hover ≠ focus ≠ selection (HARD).
6. Retargeting from current state, not "finish A then start B" (HARD).
7. The 7 stable v1.1 Skills are READ-ONLY.

## Companion files

- `references/shared-highlight.md`
- `references/retargeting.md`
- `references/hover-vs-focus-vs-selection.md`
- `references/cursor-rule.md`
- `references/pointer-modality.md`
- `references/pointer-version-notes.md`
