# Source Continuity Reference

Anchoring, source-presentation relationship, iOS 26 source continuity. This file is the Tier 2 reference for `apple-modality-overlays`.

> **HARD RULE:** A popover or contextual surface is anchored to its source. The relationship is visible (geometry, anchor, visual continuity). A surface that has no visible source is broken.

> **VERSION-SENSITIVE:** iOS 26 source-presentation continuity is VERSION-SPECIFIC. The Skill does not generalize it to all Apple platforms. Web approximation is HEURISTIC.

## Presenting control

An overlay has a presenting control — the element that the user acted on to open the overlay. Examples:

- A button that opens a sheet.
- An element that a popover is anchored to.
- A list item that triggers a context menu.
- A trigger that opens a navigation destination.

The presenting control is:

- The focus-return target on dismiss.
- The visual anchor for source-continuity (geometry, identity).
- The owner of the overlay's commit / cancel semantic.

A modal that has no presenting control is a defect. The user does not know what triggered it or where the result will be applied.

## Source anchor

The source anchor is the visual relationship between the presenting control and the overlay. The overlay:

- Emerges from the presenting control's geometry.
- Has a transform origin at the presenting control.
- Has a position that is consistent with the presenting control (e.g. a popover appears next to the control, not in a random corner).
- Has an identity that matches the presenting control (label, icon, color).

The source anchor is HEURISTIC. The implementer chooses the values. The principle is hard: the relationship is visible.

## Overlay origin

The overlay origin is the point from which the overlay emerges. The origin is:

- The center of the presenting control.
- The edge of the presenting control that is closest to the available space.
- The transform origin for the entry animation.

The origin is HEURISTIC. The principle is hard: the overlay is visibly from the source.

## Contextual relationship

The overlay is contextual to the presenting control. The relationship is:

- **Topical.** The overlay is about the same topic as the presenting control.
- **Spatial.** The overlay is near the presenting control.
- **Temporal.** The overlay is opened by the user's action on the presenting control.

A popover about "Share" anchored to a "Share" button is contextual. A popover about "Settings" anchored to a "Share" button is not contextual — the user is confused.

## Return path

The user can return to the presenting control. The return path is:

- The dismissal of the overlay (Done, Cancel, swipe, backdrop, Escape).
- The focus return to the presenting control.
- The visible relationship (the user can see the source even when the overlay is open).

A modal that hides the presenting control (e.g. a full-screen sheet on iPhone) is a presentation choice. The return path is still the dismissal + focus return; the user is not blind to the source, but the source is occluded by the sheet.

## Geometry continuity

The geometry continuity is the visual relationship between the presenting control and the overlay. The overlay:

- Emerges from the presenting control's bounding box.
- May have a "stem" or "arrow" pointing to the presenting control (popovers on iOS / iPadOS / macOS).
- May have a transform animation from the presenting control's geometry to the overlay's final position.

The geometry continuity is HEURISTIC. The principle is hard: the user can see where the overlay came from.

## Temporary surface identity

A temporary surface (sheet, popover, menu) is a separate identity from the underlying hierarchy. The surface:

- Has its own title (if any).
- Has its own focus order.
- Has its own dismissal semantics.
- Is visually distinct (a backdrop, a sheet, a popover border).

The temporary surface identity is:

- Recognizable (the user knows it's a temporary surface).
- Distinguishable (the surface looks different from the underlying content).
- Self-consistent (the surface uses the same design system as the rest of the app).

## iOS 26 source-presentation continuity (VERSION-SPECIFIC)

iOS 26 introduces stronger source-presentation continuity:

- Sheets and popovers emerge from the trigger element's geometry with a coordinated animation.
- The dismissal returns to the trigger with the same geometry.
- The content identity matches the trigger (label, icon).

This is VERSION-SPECIFIC. The Skill does not generalize it to all Apple platforms. The implementer may approximate it on iOS 26+ using the system primitive (`UISheetPresentationController` or `.sheet` in SwiftUI).

## Web approximation

Web approximation of source continuity is HEURISTIC:

- The popover's transform origin is set to the source element.
- The popover's geometry is computed from the source's bounding box.
- The popover may have a "stem" pointing to the source.
- On dismiss, the popover may animate back toward the source.

The Web approximation is not the same as iOS 26 source continuity. The Skill does not promise iOS-equivalent behavior on Web. The implementer chooses the values; the principle is "the relationship is visible."

## Web popover API

Modern Web has a Popover API (`popover` attribute) that provides:

- A popover element with `popover="auto"` or `popover="manual"`.
- Top layer rendering (above the rest of the page).
- Light-dismiss (click outside dismisses the popover).
- Browser-provided focus management (focus moves into the popover on open, returns to the trigger on close).

The Popover API is a platform primitive. The Skill recommends the platform primitive when the design fits. Custom popover implementations are acceptable when the platform primitive does not provide the needed behavior.

## When source continuity is inappropriate

Source continuity is NOT appropriate for:

- An alert that is not tied to a specific element (a "The network is unavailable" alert has no specific source).
- A navigation destination (the destination is a full screen; the source may be the previous page, not a specific element).
- A full-screen sheet on iPhone (the sheet covers the source).

For these cases, the focus-return target is the system's focus order (the previous focused element) or the trigger element (if it still exists).

## Cross-Skill note

- The decision tree is `modality-decision-tree.md`.
- The sheet semantics, dismissal, focus return are `sheets.md`.
- The popover / menu distinction is `popovers-and-menus.md`.
- The alert / confirmation distinction is `alerts-and-confirmation.md`.
- The focus return and unsaved-change protection are `focus-and-dismissal.md`.
- The motion physics of the entry / dismiss animation is `apple-motion-physics`.
- The drag-to-dismiss gesture is `apple-direct-manipulation` (cited).
