# Focus Continuity

The Skill's rules for restoring focus after dismissal / reflow / navigation / deletion. **Restore task continuity**, not arbitrary previous DOM / view identity at all costs.

## Hard invariant

> **Restore task continuity, not arbitrary previous DOM / view identity at all costs.**

If the focused control survives the contextual change (modal close; reflow; navigation; deletion of an adjacent item), focus stays. If the focused control disappears, the Skill moves to a meaningful reachable target. The Skill does NOT reset focus to the top / root of the app merely because something changed.

## Modal dismissal

When a temporary presentation closes (sheet / popover / alert / context menu / drawer), focus returns to:

- the invoking control if it still exists and remains appropriate; OR
- the most meaningful continuation target (e.g. a parent control; the next sibling; a contextual default).

The Skill owns **post-dismiss focus continuity**. `apple-modality-overlays` owns the dismissal semantics.

**Coordination:**

| What modality decides | What this Skill decides |
|---|---|
| Dismissal timing; animation; type | Where focus goes after dismissal |

## Deleted focused target

If the currently focused item disappears (user deletes a list item that had focus; user navigates away from a detail that had focus):

1. Do NOT leave focus in a dead / impossible location.
2. Move focus to a predictable nearby target:
   - the next sibling, if it exists and is focusable;
   - the previous sibling, if no next sibling;
   - the parent / container, if no siblings;
   - the page / scene root, if no parent.

The Skill does NOT hardcode "always move to previous item" — context matters. A deleted focused list item in a non-default position may have a meaningful "next" target.

## Adaptive reflow

Major Batch 6 integration with v0.4 (`apple-adaptive-structure`).

Scenario:

```
sidebar | list | detail  →  detail-only  →  sidebar | list | detail
```

If focus was inside the detail:

- Reflow to detail-only: detail survives; focus stays on the focused control.
- Reflow back to sidebar | list | detail: detail survives; focus stays on the focused control.

If the focused control disappears during reflow:

- Move to a meaningful reachable target.
- Do NOT reset to the top / root.

Coordination with `apple-adaptive-structure`: reflow coordinates where the structural surfaces are; this Skill coordinates where focus lives across that structural change.

## Navigation return

`apple-navigation-spatial` owns **where the user navigated**. This Skill owns **what should receive focus in the destination**.

Examples:

- User pushes to a detail screen → focus moves to the detail's primary heading or the first focusable element of the detail.
- User returns from detail → focus returns to the list item that triggered the navigation.
- User uses Tab to traverse a form → focus moves through form fields.

The Skill does NOT create a second history / back model. Focus placement supports task continuation; it does not override navigation semantics.

## Overlay close / temporary toolbar / menu interaction

When an overlay (tooltip / menu / temporary toolbar) closes:

- Focus returns to the element that triggered the overlay, if it still exists.
- Otherwise, focus moves to the most reasonable parent or sibling.

## Failed action

When an action fails and the user is still in the same context:

- Focus stays where it is (the action did not succeed, but the user is still editing the same field).
- If the failure requires a recovery path (e.g. validation error on a form field), focus may move to the specific field that needs attention — with explanation; with accessibility; not repeatedly.

## List changes

When a list changes (item added; item removed; item reordered):

- If the focused item survives, focus stays.
- If the focused item is removed, focus moves to a sensible replacement (next / previous / parent).

## Hard rules

- Do NOT reset focus to the top of the page / scene on every navigation return.
- Do NOT move focus to a hidden / disabled / off-screen target.
- Do NOT repeatedly steal focus on async completion.
- Do NOT silently change focus without explaining the change (when the change is non-obvious).

## Per-platform note

- **iOS / iPadOS**: focus restoration is often automatic via SwiftUI / UIKit focus API; the Skill coordinates with `UIFocusEnvironment` updates.
- **macOS**: first-responder restoration is often automatic via `NSResponder` chain; the Skill coordinates with the responder chain.
- **Web**: native focus restoration via DOM / `<dialog>`; the Skill coordinates with `aria-live` / `role="status"` for announce if the change is non-obvious.

## What the Skill rejects

- **Reflow Focus Reset** — resetting focus to top / root on adaptive layout change.
- **Focus = Selected = Removed** — collapsing focus to selection to context loss when items disappear.
- **Silent focus movement** — moving focus without explanation when the change is non-obvious.
- **Focus Stealing on Validation** — moving focus on every validation feedback tick. Move once; explain; preserve accessibility.

## Reference

- `references/swiftui-focus-commands.md` — SwiftUI `@FocusState` / `@FocusedValue` continuity.
- `references/uikit-keyboard-focus.md` — UIKit `UIFocusEnvironment` continuity.
- `references/web-keyboard-focus.md` — Web native focus + `<dialog>` continuity.