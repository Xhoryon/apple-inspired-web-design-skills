# Alternatives and Discoverability Reference

Alternative input requirements and gesture discoverability. This file is the Tier 2 reference for `apple-direct-manipulation`.

> **HARD RULE:** Important actions must not depend solely on undiscoverable gestures. Every essential action must have at least one alternative input.

## The rule

> If an action is essential (destructive, irreversible, frequently needed), it must be reachable through an input other than the gesture.

The Skill records this as a **DESIGN PRINCIPLE** (HIG Gestures). The reasons:

- Gestures are not discoverable. A user who does not know the gesture cannot perform the action.
- Gestures are not accessible. A user with motor impairments may not be able to perform the gesture.
- Gestures are not keyboard-reachable. A keyboard user cannot perform a touch gesture.
- Gestures are platform-dependent. A gesture on touch may not exist on mouse.

## When alternative input is required

| Action | Required alternative |
|---|---|
| Swipe-to-delete on a list row | Context menu (right-click / long-press → Delete); explicit "Delete" button in a row's overflow menu; keyboard Delete key |
| Long-press to activate a destructive action | Right-click / context menu; explicit "Delete" button; keyboard shortcut |
| Drag-to-reorder a list | Up / Down arrow keys with a modifier (e.g. Cmd+Up) to move; explicit "Move" menu in an overflow |
| Pinch-to-zoom on media | Double-tap to zoom; explicit "+" / "−" buttons; Cmd+= / Cmd+- on Desktop Web |
| Swipe-to-archive on a list row | Context menu; explicit "Archive" button |
| Long-press to preview a card | Hover preview (mouse); explicit "Preview" button; keyboard Space (on focused card) |
| Two-finger rotate (creative tool) | Explicit "Rotate" button; arrow keys (rotate 90°); number input |
| Swipe between pages (carousel) | Tab key to focus the next page indicator; arrow keys; explicit "Next" / "Previous" buttons |
| Edge swipe to go back | Browser back button; explicit "Back" button; Alt+Left (Desktop Web); system back gesture |
| Three-finger tap to do X | ABANDON. Three-finger gestures are not discoverable; do not use them. |

## Discoverability rules

A gesture is discoverable if:

- The user can find the gesture without documentation.
- The gesture has a visible affordance (a "grab" cursor, a hint animation, a label).
- The user can perform the gesture without prior knowledge of the system.

The default gestures (tap, scroll, drag) are discoverable. The less common gestures (long-press, double-tap, multi-finger, two-finger pinch) require a hint or an alternative input.

## Visible alternatives

A visible alternative is a UI element (a button, a menu item, a link) that performs the same action as the gesture. The visible alternative:

- Is always present (or appears in a predictable context).
- Has a clear label.
- Is reachable by keyboard, by mouse, and by touch.
- Is accessibility-friendly (`aria-label`, focusable, focus indicator).

A visible alternative is the canonical accessibility solution. The gesture is a shortcut for users who know it.

## Keyboard alternatives

A keyboard alternative is a key (or key combination) that performs the same action as the gesture. Examples:

- Enter / Space to activate a focused button.
- Delete to delete a focused list item.
- Arrow keys to navigate a list; Cmd+Up / Cmd+Down to reorder.
- Cmd+= / Cmd+- to zoom.
- Esc to cancel.
- Tab / Shift+Tab to move focus.

A keyboard alternative is required for any essential action. The alternative may be a key combination, a focusable button, or a focusable menu.

## Menu / context menu alternatives

A context menu (right-click, long-press) is a canonical alternative for:

- Swipe-to-delete / archive (context menu has the action).
- Long-press actions (context menu has the action).
- Drag-to-reorder (context menu has "Move to...").

A context menu on iOS / iPadOS is `UIContextMenuInteraction`. On Web, the context menu is a `<menu>` element or a custom DOM tree shown on `contextmenu` event.

The context menu:

- Lists the available actions.
- Is keyboard-reachable (Shift+F10 on Windows; Ctrl+Space on macOS, etc.).
- Is accessibility-friendly (`role="menu"`, `role="menuitem"`, focus management).
- Is dismissable (Esc, click outside).

## Accessibility

A gesture-only essential action is an accessibility defect. The reasons:

- VoiceOver users navigate by swiping and double-tapping. A custom gesture that is not the platform's accessibility gesture is not reachable.
- Switch Control users activate by switch + scan. A gesture is not reachable.
- Full Keyboard Access users use Tab + arrow + Enter / Space. A gesture is not reachable.
- Motor-impaired users may not be able to perform a precise gesture (long-press, multi-finger).

The Skill requires a visible alternative for every essential action. The visible alternative is the accessibility solution.

## Learnability

A custom gesture that the user must learn is a design defect. Examples of unlearnable custom gestures:

- Two-finger twist to undo.
- Three-finger swipe up to refresh.
- Diagonal swipe to do X.
- Long-press + drag to do Y.

The default bias: visible affordances + keyboard alternatives. A custom gesture is justified only when the action is genuinely non-standard and the cost (visible alternative) is too high.

The Skill does not recommend custom gestures.

## Custom gesture discoverability

If a custom gesture is necessary (e.g. a creative tool requires a custom drawing gesture), the implementer must:

- Show a hint animation on first use.
- Provide a visible alternative (button) for the same action.
- Document the gesture in an onboarding screen.
- Allow the user to disable the gesture if it conflicts with their input habits.

The Skill records the principle: a custom gesture is justified only when the action is genuinely non-standard AND the visible alternative is provided.

## Reduced motion

Gestures themselves are not motion. The motion of the gesture's effect (the snap, the return, the animation) respects `prefers-reduced-motion: reduce`. The gesture recognition is unaffected by the motion preference; only the visual motion is reduced.

## Cross-Skill note

- The gesture vocabulary is `gesture-taxonomy.md`.
- The drag lifecycle is `drag-lifecycle.md`.
- The gesture conflicts are `gesture-conflicts.md`.
- The control state semantics (focus, keyboard activation) are `apple-control-states`.
- The reduced-motion rule is `apple-motion-physics/references/reduced-motion.md`.
