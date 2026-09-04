# Gesture Taxonomy Reference

The canonical gesture vocabulary. This file is the Tier 2 reference for `apple-direct-manipulation`.

The Skill owns the recognition, mapping, and discoverability of gestures. The implementation uses the platform's standard primitives (UIKit gesture recognizers, Pointer Events on Web).

> **EVIDENCE-CLASS STATEMENT:** Apple HIG does NOT publish canonical numeric gesture thresholds (drag distance, swipe distance, long-press duration). Threshold values in this file are HEURISTICS.

## The canonical gestures

For each gesture, the Skill records:

- **Intent** — what the gesture is for.
- **Platform / input applicability** — where the gesture exists.
- **Discoverability** — how the user finds the gesture.
- **Potential conflicts** — what system or platform gestures it can interfere with.
- **Alternative input need** — whether an alternative input method is required.

### Tap

- **Intent:** single discrete activation. The most common gesture. Activates the target element.
- **Platform / input:** touch (Native iOS / iPadOS / macOS trackpad, Touch Web, native touch); mouse click (Desktop Web, native macOS / iPadOS with pointer).
- **Discoverability:** the target is visible and labelled. Tap does not require user education.
- **Potential conflicts:** accessibility tap (VoiceOver double-tap, Switch Control tap). The Skill requires that the platform's accessibility tap still fires.
- **Alternative input:** keyboard (Enter / Space on the focused element). On Web, `<button>` and `<a>` provide this for free.

### Double tap

- **Intent:** two discrete activations in a short window. Common for "zoom in" on a media view, or "favorite" / "like" toggle.
- **Platform / input:** touch; mouse double-click (Desktop Web, native macOS). Single-tap behavior must still work; double-tap is an addition.
- **Discoverability:** discoverable for the most common cases (zoom, like). Otherwise documented in the surrounding UI.
- **Potential conflicts:** a system double-tap (e.g. smart-zoom on iOS Safari). The Skill notes this on Web.
- **Alternative input:** keyboard shortcut (e.g. Cmd+= for zoom in). A button labeled "Like" or "Zoom" is also an alternative.

### Long press

- **Intent:** sustained activation. Used for context menus, peeking at content, drag-and-drop entry on touch (since touch has no hover).
- **Platform / input:** touch (the canonical case); trackpad force-touch on macOS; right-click on mouse.
- **Discoverability:** the long press often reveals a hint (a brief hint animation, a vibration, a label). The system long-press is a recognized gesture; the user knows it.
- **Potential conflicts:** the system long-press on iOS reveals a context menu. A custom long-press that does the same is a duplicate. A custom long-press that does something different must coexist with the system long-press.
- **Alternative input:** right-click on mouse; a context menu affordance (button with "..."); keyboard shortcut.

Long-press is NOT a replacement for the primary tap action. A control that activates a destructive action only on long-press must have an alternative input.

### Drag

- **Intent:** continuous input-driven movement. The user moves an object on the screen by moving the pointer / finger.
- **Platform / input:** touch (Native iOS / iPadOS via `UIDragInteraction`, Touch Web via Pointer Events); mouse (Desktop Web via HTML5 drag / Pointer Events); trackpad; stylus.
- **Discoverability:** a draggable object is visually indicated (e.g. a "grab" cursor, a handle icon, a long-press affordance on touch).
- **Potential conflicts:** scroll (vertical or horizontal). The drag must be resolved against scroll via direction lock, threshold, or CSS `touch-action`.
- **Alternative input:** keyboard (e.g. arrow keys to move a list item up / down, Space to pick up, arrow keys to move, Space to drop). The Skill requires keyboard alternative for essential drag actions.

### Swipe

- **Intent:** discrete directional gesture, often with velocity. Used for swipe-to-delete, swipe-to-archive, page navigation.
- **Platform / input:** touch (Native iOS / iPadOS via `UITableView` swipe actions, Touch Web via touch events); trackpad two-finger swipe (macOS).
- **Discoverability:** a swipe action is typically revealed by a small "swipe to reveal" affordance in onboarding. A first-time user may not discover it.
- **Potential conflicts:** page back gesture (iOS edge swipe), browser back gesture (Web), system navigation gesture.
- **Alternative input:** context menu (right-click / long-press); explicit action button (visible "Delete" button in an overflow menu). The Skill REQUIRES alternative input for swipe actions that are essential.

### Pinch / zoom

- **Intent:** two-finger distance change. Used for image zoom, map zoom, canvas zoom.
- **Platform / input:** touch (Native iOS / iPadOS, Touch Web); trackpad two-finger pinch (macOS); mouse wheel + modifier (Desktop Web).
- **Discoverability:** zoom is a common gesture on media and maps. Other UIs should not assume pinch.
- **Potential conflicts:** browser pinch-to-zoom (Web). The Skill recommends allowing the browser's pinch-to-zoom by default; do NOT preventDefault on the browser's zoom.
- **Alternative input:** zoom in / out buttons; double-tap (mobile); Cmd+= / Cmd+- (Desktop Web).

Pinch is NOT appropriate for general UI scaling. Most UI does not need pinch.

### Rotate

- **Intent:** two-finger rotation. Used in creative tools (image editing, drawing), maps (rotate to north).
- **Platform / input:** touch; trackpad.
- **Discoverability:** uncommon gesture; usually accompanied by an on-screen rotation control (a compass / rotate button).
- **Alternative input:** rotate button; keyboard shortcut (e.g. arrow keys).
- Rotate is rare. The Skill records it as vocabulary but does not require it.

### Edge gesture

- **Intent:** input from a screen edge. Examples: iOS edge-swipe back, iPadOS edge swipe for app switcher, Android back gesture.
- **Platform / input:** touch; sometimes trackpad (macOS).
- **Discoverability:** edge gestures are system-defined. The user knows them from the platform.
- **Potential conflicts:** the system edge gesture is the canonical. A custom edge gesture that conflicts (e.g. Web custom edge-swipe back) is ABANDON.
- **Alternative input:** the back button (where present); the in-app navigation chrome; the system menu.

The Skill does NOT implement custom edge gestures on Web. Browser back is canonical.

## Modality mapping

The same gesture may not exist on every input modality. The Skill records the mapping:

| Gesture | Touch | Mouse | Trackpad | Keyboard | Stylus |
|---|---|---|---|---|---|
| Tap | YES | click | click | Enter / Space | tap |
| Double tap | YES | double-click | double-click | n/a | double-tap |
| Long press | YES | right-click | force-click / right-click | context menu key | sustained press |
| Drag | YES | drag (mousedown) | drag | arrow + Space (alternative) | drag |
| Swipe | YES | n/a (use button / menu) | two-finger swipe | n/a | n/a |
| Pinch / zoom | YES | wheel + modifier | two-finger pinch | Cmd+= / Cmd+- | n/a |
| Rotate | YES | n/a | two-finger rotate | arrow keys (alt) | n/a |
| Edge | YES (system) | n/a | n/a | n/a | n/a |

A gesture on a non-matching modality is **ABANDON**. The Skill does not invent a gesture for a modality that does not have it.

## Discoverability rules

- **Default gestures** (tap, drag, swipe on touch, scroll) are discoverable without education.
- **Less common gestures** (long-press, double-tap, multi-finger) require an affordance or onboarding hint.
- **Custom gestures** (e.g. two-finger twist to undo) require explicit user education. The Skill does not recommend custom gestures.

The default bias: visible alternatives (buttons, menus) over discoverable gestures. A gesture is a power-user shortcut, not the only path.

## Cross-Skill note

- The drag lifecycle is `drag-lifecycle.md`.
- Gesture conflicts (with system gestures) are `gesture-conflicts.md`.
- Alternative input requirements are `alternatives-and-discoverability.md`.
- The motion physics of drag, snap, return is `apple-motion-physics` (cited).
- The state semantics of press (pressed, released) are `apple-control-states`.
