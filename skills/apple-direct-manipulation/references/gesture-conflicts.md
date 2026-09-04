# Gesture Conflicts Reference

The conflict matrix between custom gestures, system gestures, browser gestures, and accessibility gestures. This file is the Tier 2 reference for `apple-direct-manipulation`.

> **HARD RULE:** System gestures (browser back, OS edge swipe, scroll, accessibility gestures) must not be hijacked. If a conflict exists, ABANDON the custom gesture or ADAPT.

## The conflict matrix

| Custom gesture | Conflicts with | Resolution (Platform Applicability verdict) |
|---|---|---|
| Horizontal drag inside a vertical scroll container | Vertical scroll | Lock direction (CSS `touch-action: pan-y` on the draggable; resolve direction from initial movement). ADAPT. |
| Horizontal drag inside a horizontal scroll container | Horizontal scroll | Scope the drag to a non-scrolling child; do NOT allow drag inside a horizontal scroll container unless the drag is the dominant intent. ADAPT. |
| Edge swipe (from screen edge) | Browser back gesture (Web); system back gesture (iOS / iPadOS) | ABANDON custom edge swipe. Use the system / browser back. |
| Edge swipe (from screen edge) | OS app-switcher gesture (iPadOS) | ABANDON. The system gesture is canonical. |
| Pinch | Browser pinch-to-zoom (Web) | Allow browser zoom; do NOT preventDefault. If the page should not zoom, set `user-scalable=no` in the viewport meta (consider accessibility implications). ADAPT. |
| Long press | System long-press (iOS context menu) | ADAPT: use the system context menu on iOS; on Web, use right-click. Do not implement a custom long-press that does the same thing. |
| Long press | Pull-to-refresh (iOS) | ABANDON. The system pull-to-refresh is canonical. |
| Two-finger swipe | Page back/forward (macOS trackpad) | ABANDON. The system two-finger swipe is canonical. |
| Custom tap | Accessibility tap (VoiceOver double-tap, Switch Control tap) | Do NOT preventDefault on tap; let the platform's accessibility tap fire. The accessibility tap is a system gesture. |
| Custom drag | Browser image drag (Web) | ABANDON. The browser's native image drag is canonical. |
| Custom drag | Native text selection | ADAPT: do not start a drag from selectable text unless the user explicitly picks up the drag handle. |
| Wheel-zoom (Ctrl+wheel) | Browser zoom (Web) | ADAPT: the browser zoom is canonical; do not preventDefault. If the page should not zoom, set `user-scalable=no`. |
| Touch-and-hold (drag entry on touch) | Scroll start (touch) | Use a movement threshold to resolve: a small initial movement is scroll; a larger one is drag. ADAPT. |
| Swipe on a list row | Horizontal page scroll (mobile Web) | Lock to row-level: the swipe is recognized within the row; the page does not scroll horizontally. ADAPT. |
| Multi-finger gesture | System multi-finger gesture (trackpad) | ABANDON. The system multi-finger gesture is canonical. |

## Vertical scroll vs horizontal gesture

The most common conflict on Web. A horizontal drag (e.g. swipe-to-delete on a list row) inside a vertically scrolling page can conflict with the page's vertical scroll.

The resolution:

1. **Direction lock.** When the user starts moving, decide the direction. If the initial movement is more horizontal than vertical, lock to horizontal. If more vertical, lock to vertical.
2. **CSS `touch-action`.** Set `touch-action: pan-y` on the row to allow the page to scroll vertically; the row's own JS handles the horizontal swipe.
3. **Threshold.** A small initial movement is ambiguous. The lock should occur after a small threshold (e.g. 5–10 px) to avoid premature locking.

The Skill does not publish a canonical threshold. The implementer chooses.

## Horizontal scroll containers

A horizontal swipe-to-delete row inside a horizontal scroll carousel is a more complex conflict. The Skill recommends:

- Do NOT allow a swipe-to-delete inside a horizontal scroll container.
- If the row is inside a horizontal scroll, the swipe-to-delete is not available; use a context menu or explicit button instead.
- Alternatively, scope the swipe-to-delete to a non-scrolling child (e.g. a row that is not inside a horizontal scroll).

The Skill prefers explicit affordances over gesture conflicts.

## Browser navigation gesture

On iOS Safari, the edge-swipe-back is a system gesture. On Android Chrome, the back gesture is a system gesture. On desktop Web, the browser back is a button (and on some platforms, a swipe).

A Web app that implements a custom edge-swipe back gesture is **ABANDON**. The reasons:

- The custom gesture conflicts with the system gesture (both fire on edge swipe).
- The user expects browser back, not app back.
- The browser back preserves the URL; the custom gesture may not.
- The custom gesture breaks horizontal scroll on the left edge.
- The custom gesture is not discoverable (no visual affordance on the left edge).

The Skill does not implement custom edge-swipe back on Web.

## OS system gesture

iPadOS has a multi-finger app switcher gesture. iOS has the home-indicator gesture. macOS has a multi-finger trackpad gesture for Mission Control. These are system gestures.

A custom gesture that overlaps with a system gesture is **ABANDON**. The system gesture is canonical. The user expects the system behavior.

## Accessibility gesture

VoiceOver, Switch Control, and AssistiveTouch on iOS / iPadOS / macOS use gestures (double-tap, two-finger swipe, etc.) for accessibility. A custom gesture that preventDefaults an accessibility gesture is a serious defect — the user cannot use the accessibility feature.

The Skill requires:

- Do NOT preventDefault on accessibility-driven gestures.
- Let the platform's accessibility gesture fire.
- The custom gesture may run alongside, but it must not block the accessibility gesture.

## Pointer interaction

The pointer (mouse) on a Web page has its own semantics:

- Left-click: primary action.
- Right-click: context menu.
- Middle-click: open in new tab.
- Wheel: scroll.
- Ctrl+wheel: zoom.
- Drag with left button: text selection (default), or image drag.

A Web app that overrides these (e.g. right-click that does not show a context menu) is breaking the user's expectation. The Skill requires:

- Right-click should show a context menu (the system context menu, or a custom one).
- Wheel should scroll.
- Drag should be initiated by the app's drag affordance, not by overriding the default drag.

## Nested controls

A draggable element inside a scrollable container, where the container also has a click handler, is a triple conflict. The Skill recommends:

- The draggable element has its own gesture handler.
- The container's scroll / click is unaffected.
- The draggable's gesture does NOT fire on the container's scroll.

A common implementation: the draggable uses `pointer-events: auto` and a `touch-action: none` (or `pan-y` for partial). The container's scroll is unaffected.

## Resolution priority

When multiple gestures are possible at the same input event, the priority is:

1. **System gesture** (browser back, OS edge swipe, accessibility gesture). The system wins.
2. **Container's primary gesture** (vertical scroll on a scrollable page). The container wins unless the inner element is the recognized target.
3. **Inner element's gesture** (drag, swipe). The inner element wins if it has captured the input (via direction lock + threshold).

The system gesture is always the highest priority. The implementer does not override the system.

## Cross-Skill note

- The gesture vocabulary is `gesture-taxonomy.md`.
- The drag lifecycle is `drag-lifecycle.md`.
- The alternative input requirements are `alternatives-and-discoverability.md`.
- The scroll ownership is `apple-navigation-spatial/references/scroll-behavior.md`.
- The pointer-target relationship (hover, focus, selection) is `apple-pointer-interaction`.
