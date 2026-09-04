---
name: apple-direct-manipulation
description: Use when designing or implementing the relationship between user movement and object movement — drag, swipe, pinch / zoom, long-press, and the gesture vocabulary for direct manipulation. Applies to Native iOS / iPadOS / macOS (system drag-and-drop, system gestures) and Desktop Web / Touch Web (HTML5 drag, pointer events, gesture recognition). Required for drag-to-reorder, drag-to-resize, swipe-to-action, pinch / zoom on media, and any custom drag / gesture interaction. Do NOT use for control state semantics (use apple-control-states), pointer-target relationship (use apple-pointer-interaction), motion physics (use apple-motion-physics), navigation (use apple-navigation-spatial), or modal type decision (use apple-modality-overlays). Do NOT hijack system gestures (browser back, OS edge swipe, accessibility gestures).
version: "0.1.0-dev"
license: MIT
---

# apple-direct-manipulation

User movement ↔ object movement. The Skill owns gesture recognition, drag / swipe / pinch vocabulary, gesture lifecycle, and gesture ownership. The motion physics of the drag is cited from `apple-motion-physics` (post-release continuation, not gesture-end-only animation).

## When to use

Use when the task involves the user directly moving an object or surface. Symptoms:

- "User can drag a card to reorder a list."
- "User can swipe a list row left to reveal actions."
- "User can pinch-zoom an image."
- "User can long-press to preview content."
- "Drag follows the pointer, not the gesture-end position."
- "User can cancel the drag and have the object snap back."
- "User can regrab the object during snap-back."

Do **not** use when:

- The task is control state semantics (idle / hover / pressed / focused / selected / disabled). Use `apple-control-states`.
- The task is pointer-target relationship (hover highlight, shared highlight). Use `apple-pointer-interaction`.
- The task is motion physics (spring, interruptibility, reduced motion). Use `apple-motion-physics`.
- The task is navigation hierarchy. Use `apple-navigation-spatial`.
- The task is choosing between sheet / popover / menu / alert / action sheet. Use `apple-modality-overlays`.

## Hard invariants

1. **Movement should feel causally connected to user input.** If the user drags 30 px, the object moves 30 px during the drag, not at the end. The gesture-end position determines commit / cancel; it does not determine the position during the drag.
2. **Standard gestures first.** Use the platform's standard gesture vocabulary (tap, double-tap, long-press, drag, swipe, pinch). Custom gestures only when the behavior is non-standard and the cost (discoverability, learnability, conflict) is justified.
3. **Alternative input for essential actions.** Important actions must have alternative input. A swipe-to-delete must also be accessible via a context menu, a button, or an accessibility action. Gestures are not the only way.
4. **No system gesture hijacking.** Do not hijack browser back, OS edge swipe, scroll, accessibility gestures, or platform-standard gestures. If a conflict exists, ABANDON the custom gesture or ADAPT (e.g. confirm with the user before overriding a system gesture).
5. **Gesture conflict resolved explicitly.** When a custom gesture may conflict with a system gesture (e.g. horizontal drag inside a vertically scrollable page), the Skill records the resolution (CSS touch-action, gesture-zone separation, threshold, lock direction).
6. **Causal update, not gesture-ended update.** The drag / swipe / pinch must update the object during the gesture. Animation at the end of the gesture is a commit / cancel transition, not the position-update phase.
7. **Apple HIG does NOT publish canonical gesture thresholds** (e.g. drag distance, swipe distance, long-press duration). Threshold values are implementation heuristics.

## Gesture vocabulary

A canonical gesture vocabulary covers:

- **Tap** — single discrete activation.
- **Double-tap** — two discrete activations in a short window.
- **Long-press** — sustained activation. Not a replacement for the primary tap action.
- **Drag** — continuous input-driven movement.
- **Swipe** — discrete directional gesture (often with velocity).
- **Pinch / zoom** — two-finger distance change.
- **Rotate** — two-finger rotation (less common, used in media / creative tools).
- **Edge gesture** — input from a screen edge (iOS edge swipe, Android back gesture, etc.).

This Skill covers all of them as vocabulary. The Skill does not require the implementation of all of them; it provides the vocabulary and rules.

## Drag lifecycle

The canonical drag lifecycle is:

```
idle
  → possible drag
  → drag recognized
  → direct movement (object follows pointer / finger)
  → lift / engage
  → commit OR cancel
  → recovery
```

The object follows the user input during the drag phase. The lift / engage / commit / cancel / recovery are the post-release phases, owned by `apple-motion-physics`.

The drag must be cancelled when the user releases outside a valid target, when the gesture is interrupted (system gesture hijack, focus change, etc.), or when the user explicitly cancels (e.g. Escape key on Web).

## Drag vs click

The interaction must distinguish:

- Pointer down → small movement (e.g. < 5 px) → release → click.
- Pointer down → enough movement (e.g. ≥ 5–8 px) → drag.

A naive implementation that fires click on pointer-up regardless of movement will mis-activate drag-and-drop. The threshold value is an implementation heuristic, not Apple canonical.

## Drag threshold

The drag threshold is the distance the pointer must travel before the drag is recognized. This is an implementation heuristic. The Skill provides a range (e.g. 3–10 px depending on density and target size), not a fixed number.

Considerations:

- Accidental movement (touch noise, mouse jitter).
- Input modality (touch has higher noise than mouse).
- Density (denser UIs need higher thresholds to avoid accidental drags).
- Target size (small targets need higher thresholds to avoid accidental drags).

## Drag progress and completion

Drag progress is fed by user input (displacement, velocity). Completion is decided by:

- Commit: user released over a valid target.
- Cancel: user released outside a valid target, or gesture interrupted.
- Snap: drag dropped; object snaps to a snap point.
- Return: drag cancelled; object returns to its origin.

Snap and return are motion behaviors (cites `apple-motion-physics`).

## Velocity

The drag may have a release velocity. The completion decision can use velocity (e.g. fast flick → commit; slow release → snap or return). Velocity is input data; the motion-physics Skill owns the post-release continuation.

## Regrab interruptibility

If the user is mid-snap-back (after a cancel) and regrabs the object:

- The Skill RECOMMENDS the snap-back can be interrupted; the user can regrab.
- This is platform-dependent and may have a cost (visual transition interruption).
- The Skill records the capability, not a mandate.

This is interaction physics, not navigation. The motion-physics Skill owns the snap-back animation; this Skill owns the gesture recognition.

## Long-press

Long-press is a sustained activation. It is appropriate for:

- Contextual actions (right-click equivalent on touch).
- Preview (peek at content).
- Secondary functionality.

Long-press is NOT a replacement for the primary tap action. A control that activates a destructive action only on long-press must have alternative input for the destructive action (a menu, a button, an accessibility action).

## Swipe actions

Swipe actions on list rows (e.g. swipe-to-delete, swipe-to-archive) are:

- **NATIVE** on Native iOS / iPadOS via UITableView swipe actions.
- **NATIVE** on Touch Web via touch events.
- **ADAPT** on Desktop Web — desktop users do not swipe. Provide a context menu (right-click) or a hover-revealed action.

The Skill records this. The implementation may use the platform primitive on each platform.

## Pinch / zoom

Pinch / zoom is appropriate for:

- Media (images, video).
- Canvas / maps.
- Creative tools.
- Data visualization (rare).

Pinch is NOT appropriate for general UI scaling. Most UI does not need pinch. The Skill records this.

## Gesture conflict matrix

The Skill records a conflict matrix:

| Custom gesture | Conflicts with | Resolution |
|---|---|---|
| Horizontal drag | Browser back gesture | ABANDON custom edge swipe; use browser back |
| Horizontal drag | Horizontal page scroll | Lock direction; do not allow drag inside horizontal scroll containers unless scoped |
| Pinch | Page zoom (browser) | Allow; the platform handles browser zoom; do not preventDefault |
| Custom long-press | System long-press (iOS context menu) | ADAPT: use system context menu on iOS; on Web, use right-click instead of long-press |
| Custom tap | Accessibility tap (VoiceOver / Switch Control) | Do not preventDefault on tap; let the platform's accessibility tap fire |

## Pointer modality gating

Gestures are input-modality-dependent. The Skill records the modality:

- **Touch** — coarse pointer, no hover. Gestures are tap / double-tap / long-press / drag / swipe / pinch.
- **Mouse** — fine pointer, hover. Gestures are click / double-click / drag (mousedown) / right-click (context menu) / wheel.
- **Trackpad** — fine pointer, hover, multi-finger gestures. Gestures are click / drag / two-finger scroll / two-finger pinch (zoom).
- **Keyboard** — no pointer. Gestures are keypress / key combinations. No pointer gesture applies.
- **Stylus** — fine pointer with pressure / tilt. Gestures include tap / drag / pressure-aware. The Apple Pencil on iPadOS is the canonical case.

The Skill declares which gestures apply to which input modality. A gesture on a non-matching modality is **ABANDON** (do not implement; the modality does not have that gesture).

## Ownership boundaries

- **Gesture recognition** (tap, double-tap, long-press, drag, swipe, pinch) — `apple-direct-manipulation`.
- **Drag lifecycle** (idle, possible drag, recognized, drag, lift, commit / cancel) — `apple-direct-manipulation`.
- **Drag threshold** (heuristic value) — `apple-direct-manipulation`.
- **Drag progress and completion** (input-fed) — `apple-direct-manipulation`.
- **Snap and return motion** (post-release) — `apple-motion-physics` (cited).
- **Velocity** (input data) — `apple-direct-manipulation`.
- **Regrab interruptibility** — `apple-direct-manipulation` + `apple-motion-physics` (cited).
- **Press / hover / selected state** — `apple-control-states`.
- **Pointer-target relationship** (hover highlight, shared highlight) — `apple-pointer-interaction`.
- **Scroll ownership** (native scroll is platform's) — `apple-navigation-spatial` (reference).
- **Sheet interactive dismiss gesture** — `apple-modality-overlays` (semantics) + `apple-direct-manipulation` (gesture) + `apple-motion-physics` (physics).

## Reference catalog (Tier 2)

- `references/gesture-taxonomy.md` — standard gesture vocabulary, modality mapping.
- `references/drag-lifecycle.md` — drag lifecycle states, threshold heuristic, completion.
- `references/gesture-conflicts.md` — gesture conflict matrix and resolutions.
- `references/alternatives-and-discoverability.md` — alternative input requirements, gesture discoverability rules.

## Hard invariants (recap)

1. Causal update during gesture (HARD).
2. Standard gestures first (HARD).
3. Alternative input for essential actions (HARD).
4. No system gesture hijacking (HARD).
5. Gesture conflict resolved explicitly (HARD).
6. Gesture-end is a commit / cancel transition, not a position update (HARD).
7. Apple HIG does not publish canonical gesture thresholds (HEURISTIC only).
8. The 7 stable v1.1 Skills are READ-ONLY.

## Companion files

- `references/gesture-taxonomy.md`
- `references/drag-lifecycle.md`
- `references/gesture-conflicts.md`
- `references/alternatives-and-discoverability.md`
