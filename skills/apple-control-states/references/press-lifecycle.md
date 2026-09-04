# Press Lifecycle Reference

The press lifecycle is the canonical interactive control contract. This file is the Tier 2 reference for `apple-control-states`.

## Lifecycle (canonical)

```
idle
  → pointerdown / touchstart
  → pressed
  → release inside
  → activate
  → recovery
  → idle
```

Cancellation branch:

```
pressed
  → pointerleave / pointercancel / touchcancel
  → cancel
  → recovery
  → idle
```

A control that does not handle the cancellation branch is not a real interactive control.

## Press feedback (HEURISTIC)

Press feedback may be implemented as any of:

- scale (e.g. 0.97 — heuristic, not Apple canonical)
- opacity (e.g. 0.85 — heuristic)
- tint / background-color shift
- highlight appearance
- material change (e.g. Liquid Glass surface intensifies)
- shadow / depth change
- icon state change (e.g. chevron rotates)
- a combination of the above

The exact value is a HEURISTIC. The Skill must NOT write "Apple uses scale 0.96" — that is unsupported.

## Why cancellation is mandatory

If the user moves the pointer or finger away from the control during a press, the control must:

- Return to the pre-press visual state.
- NOT fire the activation.

This is what the user expects from every interactive control on every platform. A control that activates regardless of where the pointer ends up is broken.

A common implementation mistake is binding `activate` to `pointerdown` instead of `pointerup`. The correct binding is `pointerup` — and the pointer must still be over the control at that moment.

## Input modality variants

The lifecycle above is the canonical form. Each input modality implements the same lifecycle with the appropriate events:

- **Mouse:** `pointerdown` / `pointerup` / `pointerleave` / `pointercancel` (Pointer Events API; covers mouse and pen).
- **Touch:** `touchstart` / `touchend` / `touchcancel` (Touch Events API; for older code; Pointer Events can also abstract touch).
- **Keyboard:** `keydown` for Enter / Space (treated as activate, not press; the keyboard does not have a press lifecycle — the activation happens on the keydown of the activate key).
- **Trackpad:** treated as mouse.

The lifecycle is the same. The events are different.

## Native iOS / iPadOS

The Apple system primitive `UIButton` (and similar controls) implements the lifecycle internally. The Skill recommends the system primitive, not a custom re-implementation.

## Native macOS

The Apple system primitive `NSButton` (and similar controls) implements the lifecycle internally.

## Web

On Web, the implementation depends on whether Pointer Events or Touch Events are used. Both should yield the same lifecycle.

A common pattern with Pointer Events:

```javascript
button.addEventListener('pointerdown', (e) => {
  e.currentTarget.classList.add('pressed');
  e.currentTarget.setPointerCapture(e.pointerId);
});

button.addEventListener('pointerup', (e) => {
  e.currentTarget.classList.remove('pressed');
  if (e.currentTarget.contains(e.relatedTarget || document.elementFromPoint(e.clientX, e.clientY))) {
    e.currentTarget.click();
  }
});

button.addEventListener('pointercancel', () => {
  e.currentTarget.classList.remove('pressed');
});

button.addEventListener('pointerleave', () => {
  e.currentTarget.classList.remove('pressed');
});
```

This pattern handles the cancellation branch: if the pointer leaves the control, the pressed state is removed.

## Cross-Skill note

The press lifecycle is the contract. The motion physics of the press animation is `apple-motion-physics`. The hover and pointer-target relationship is `apple-pointer-interaction`. This Skill owns the lifecycle, not the implementation.
