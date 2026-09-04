# Drag Lifecycle Reference

The canonical drag lifecycle. This file is the Tier 2 reference for `apple-direct-manipulation`.

> **HARD RULE:** The object must follow the user's input causally during the drag. The gesture-end position determines commit / cancel, not the position during the drag.

> **EVIDENCE-CLASS STATEMENT:** Apple HIG does NOT publish canonical drag threshold values. The values in this file are HEURISTICS.

## Lifecycle (canonical)

```
idle
  → possible drag
  → drag recognized
  → direct movement (object follows pointer / finger)
  → lift / engage
  → commit OR cancel
  → recovery
```

The object follows the user input during the `direct movement` phase. The `lift / engage / commit / cancel / recovery` phases are post-release; their motion is owned by `apple-motion-physics` (cited).

## Idle

The drag is not in progress. The object is at rest. The user may interact with the surface in other ways.

## Possible drag

The user has pressed on a draggable surface (touch / mouse / pointer down). The system is determining whether the press is a click or a drag. A small movement (e.g. < 5 px) keeps the gesture in the "possible drag" state; the press is a click candidate.

### Drag threshold

The drag threshold is the distance the pointer must travel before the gesture is recognized as a drag instead of a click. The threshold is a HEURISTIC. The Skill provides a range, not a fixed number:

- **3–5 px** on a fine pointer (mouse, trackpad, stylus) with a small target.
- **5–8 px** on a coarse pointer (touch) with a small target.
- **8–10+ px** on dense UIs (small targets close together) to avoid accidental drags.

Considerations:

- **Accidental movement (touch noise, mouse jitter).** Touch has higher noise than mouse.
- **Density.** Denser UIs need higher thresholds.
- **Target size.** Smaller targets need higher thresholds.
- **Modality.** Trackpad treated as mouse.

The threshold is the implementer's choice. The Skill does not publish a canonical value.

## Drag recognized

The user has moved past the drag threshold. The gesture is now a drag. The object begins to follow the user's input.

At this point:

- The click is no longer a candidate.
- The drag is owned by the gesture.
- The visual treatment changes (e.g. a "lifted" state, a shadow, a higher z-index).

## Direct movement

The object follows the pointer / finger. The position is updated continuously (per frame, or per pointer event). The motion is **causally connected** to the input:

- If the user moves the pointer 30 px right, the object moves 30 px right.
- If the user moves the pointer 12 px up, the object moves 12 px up.
- The motion is direct; it is NOT a gesture-end-only animation.

### Causal update

A drag that does not update the object's position until the user releases the pointer is a gesture-end animation. This is **WRONG** for direct manipulation. The object must follow the input during the drag.

### Valid target

During the drag, the system tracks which drop targets the object is over. A drop target is "valid" if it accepts the dragged object's type. A target that is not valid may show a "rejected" visual treatment.

## Lift / engage

The user has lifted the pointer / finger while the drag is still in progress. The drag is now in the post-release phase. The motion physics of the lift is owned by `apple-motion-physics`.

At the lift, the system checks:

- Is the pointer over a valid drop target?
- What is the pointer's velocity?

## Commit (drop on valid target)

The user has released over a valid drop target. The object is dropped. The system performs the action associated with the drop (e.g. reorder the list, archive the item, move the file).

The motion to the drop point is `apple-motion-physics`. The commit itself is the application of the action.

## Cancel (drop outside valid target, or interrupted)

The user has released outside a valid drop target, or the drag was interrupted (system gesture hijack, focus change, ESC key on Web, etc.). The drag is cancelled. The object returns to its origin.

The motion back to the origin is `apple-motion-physics` (snap-back or return). The cancellation is recorded in the gesture state.

### Snap

The user has released, and the object snaps to a snap point (a list position, a grid cell, a magnetic target). The snap is owned by `apple-motion-physics`.

### Return

The user has released, and the object returns to its origin (cancel case). The return is owned by `apple-motion-physics`.

## Regrab

If the user is mid-snap-back (after a cancel) and regrabs the object:

- The Skill RECOMMENDS that the snap-back can be interrupted; the user can regrab.
- This is platform-dependent; native iOS / iPadOS may or may not support regrab mid-animation. Web Pointer Events can implement it.
- The Skill records the capability as RECOMMENDED, not mandated. The implementer may disable regrab if it adds too much complexity.

The regrab interruptibility is a UX nicety, not a hard requirement.

## Drag vs click

The distinction between drag and click is critical. A control that fires `click` on every pointer-up regardless of movement is broken.

The pattern:

```
on pointerdown:
  record start position
  set state = possible_drag

on pointermove:
  if state == possible_drag AND distance > threshold:
    set state = drag_recognized
    begin drag
  if state == drag_recognized:
    update object position

on pointerup:
  if state == possible_drag:
    fire click
  if state == drag_recognized:
    commit or cancel based on target
```

The state is a single value. The transition is explicit. The threshold is a HEURISTIC.

## Input modality notes

- **Touch:** `touchstart` / `touchmove` / `touchend` / `touchcancel`. The cancel event fires when the system interrupts the gesture (e.g. system gesture hijack, notification banner).
- **Mouse:** `mousedown` / `mousemove` / `mouseup` / `mouseleave`. Pointer Events unify these.
- **Trackpad:** same as mouse.
- **Keyboard:** there is no drag. Keyboard drag is an alternative input (arrow keys + modifier). See `alternatives-and-discoverability.md`.
- **Stylus:** same as touch. Pressure / tilt are extra input data, recorded for future applicability.

## Handoff to `apple-motion-physics`

The drag lifecycle recognizes, tracks, and commits the drag. The motion of the lifted object (commit animation, cancel return, snap) is `apple-motion-physics`. The handoff is:

- The drag lifecycle says: "the drag is committed; the object should move to the drop point."
- The motion physics decides: spring stiffness, damping, duration.

The drag lifecycle does NOT specify motion values. The motion physics does NOT specify gesture recognition. The Skills cite, not duplicate.

## Cross-Skill note

- The gesture vocabulary is `gesture-taxonomy.md`.
- The gesture conflicts (with scroll, system gestures) are `gesture-conflicts.md`.
- The alternative input for drag (keyboard) is `alternatives-and-discoverability.md`.
- The motion physics (spring, return, regrab) is `apple-motion-physics`.
- The state semantics of press (pressed, released, dragged) are `apple-control-states`.
