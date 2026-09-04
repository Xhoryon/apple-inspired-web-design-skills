# Pointer Modality Gating Reference

How to gate pointer-specific behavior by input modality on Web. This file is the Tier 2 reference for `apple-pointer-interaction`.

> **HARD RULE:** Pointer-specific behavior on Web must be gated by `(hover: hover)` and `(pointer: fine)` media queries. Touch-only devices must not receive hover effects.

## Why gating matters

A Web control that shows a hover effect on touch is a defect. The user cannot "hover" with a finger; the effect either does not appear (best case) or appears on tap and never goes away (worst case). The Skill requires that pointer effects are gated by input modality.

A Web control that shows a hover effect on keyboard-only is also a defect. The keyboard user has no pointer; the effect is invisible.

The two media queries:

- `(hover: hover)` — the primary input has a hover capability. True for mouse / trackpad. False for touch-only.
- `(pointer: fine)` — the primary input is a fine pointer (mouse / trackpad / stylus). False for touch (coarse pointer).

The combination `(hover: hover) and (pointer: fine)` is the canonical "desktop Web with mouse" gate.

## Canonical media query pattern

```css
/* Default: mobile / touch / keyboard. No pointer effect. */
.button {
  background: var(--color-button-rest);
  color: var(--color-button-text);
}

/* Desktop Web with mouse / trackpad. Pointer effect applies. */
@media (hover: hover) and (pointer: fine) {
  .button:hover {
    background: var(--color-button-hover);
  }
}

/* Touch-only. Hover effect does NOT apply, but pressed state does. */
@media not (hover: hover) {
  .button:active {
    background: var(--color-button-pressed);
  }
}
```

The pattern:

- The default state is for everyone.
- The pointer effect is gated to `(hover: hover) and (pointer: fine)`.
- The pressed state is gated to `not (hover: hover)` (touch-only).

## Pointer media queries

| Query | Meaning |
|---|---|
| `(hover: hover)` | Primary input has hover capability. |
| `(hover: none)` | Primary input has no hover capability. |
| `(pointer: fine)` | Primary input is a fine pointer (mouse, trackpad, stylus). |
| `(pointer: coarse)` | Primary input is a coarse pointer (touch). |
| `(pointer: none)` | Primary input is none (keyboard, screen reader). |
| `(any-pointer: fine)` | ANY input device is a fine pointer. |
| `(any-hover: hover)` | ANY input device has hover capability. |

The Skill uses `(hover: hover) and (pointer: fine)` for the "desktop with mouse" gate. The `any-*` variants are for hybrid devices (e.g. a laptop with a touch screen).

## Pointer events API

On Web, the Pointer Events API provides pointer-type information:

- `event.pointerType` — `mouse`, `touch`, `pen`, or empty string.
- `event.pressure` — 0 to 1 (pen only).
- `event.isPrimary` — true for the primary pointer.

The Pointer Events API can be used to gate behavior in JavaScript:

```javascript
button.addEventListener('pointerenter', (e) => {
  if (e.pointerType === 'mouse' || e.pointerType === 'pen') {
    // Apply hover effect.
  }
});
```

The CSS media query is preferred for static gating; the JS API is for dynamic behavior.

## What the Skill rejects

- A Web hover effect that applies to touch devices.
- A Web hover effect that applies to keyboard-only devices.
- A Web effect that is not gated by pointer modality.
- A Skill that recommends hover behavior without specifying the gate.

## Hybrid devices

A laptop with a touch screen is a hybrid device. The user may use the touch screen or the trackpad. The Skill recommends:

- Use `(any-hover: hover)` and `(any-pointer: fine)` for the "any input has the capability" gate.
- Use `(hover: hover)` and `(pointer: fine)` for the "primary input is the capability" gate.

The hybrid device is the most complex case. The Skill records it as a known complexity; the implementer chooses the gate based on the design.

## Cross-Skill note

- The pointer-target relationship is `apple-pointer-interaction` (this Skill).
- The Web cursor non-replacement rule is `cursor-rule.md`.
- The hover / focus / selection distinctness is `hover-vs-focus-vs-selection.md`.
- The pointer version-sensitive distinction (iPadOS generation A vs generation B) is `pointer-version-notes.md`.
- The control state semantics are `apple-control-states`.
