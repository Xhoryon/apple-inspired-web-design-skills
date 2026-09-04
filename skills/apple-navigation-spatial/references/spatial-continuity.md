# Spatial Continuity Reference

Spatial continuity is the user's ability to understand where they came from and where they went. This file is the Tier 2 reference for `apple-navigation-spatial`.

## The principle

> When the user moves from A to B, A and B should share enough visual or contextual language that the user knows the relationship.

Spatial continuity is NOT about copying a shared element. It is about preserving context. The user must understand:

- Where they came from (source).
- Where they went (destination).
- How to return (back affordance).
- What is preserved (selection, scroll, filter, identity).

## Source → destination relationship

The destination should make the source obvious. The relationship is communicated by one or more of:

- **Visual continuity** — the destination shares a color family, a typography family, a layout system, or a specific visual element with the source. Example: the destination uses the same accent color as the source; the destination's title is the source's selected item name.
- **Geometry continuity** — the destination emerges from the source's geometry. Example: a card detail view opens with the card's bounding box as the origin; a sheet emerges from the trigger element's geometry.
- **Identity continuity** — the destination identifies itself as a continuation of the source. Example: a detail page shows the source list item's title in the navigation bar; a sheet's title is the same as the trigger button's label.
- **State continuity** — the destination inherits the source's relevant state. Example: a filter applied in the list is applied in the detail; a selected tab is the active tab; a selected row in the list is the row being detailed.

A destination that has none of these is disorienting. The user does not know where they came from. The Skill rejects this as a design defect, not a stylistic choice.

## Relative direction

The transition should suggest a spatial direction. Different navigation events have different directions:

- **Navigate (deeper)** — forward, into the destination. The source recedes; the destination emerges.
- **Navigate (peer)** — horizontal or lateral. The source slides out; the destination slides in.
- **Back** — reverse of the forward transition. The destination recedes; the source returns.
- **Modal** — the source is occluded; the modal is on top.

The direction is heuristic. It is communicated by the transition (slide, fade, scale, position). The Skill records the principle; the implementation chooses the values.

## Identity continuity

The destination's identity should match the source's intent:

- A list item's detail view shows the list item's name as the destination title.
- A tab's content has the tab's name as the section title.
- A popover's content is anchored to the element that opened it; the popover's title (if any) is the source's label.

A destination whose title, color, or layout does not match the source's intent is broken. The user cannot connect them.

## Source anchoring

Source anchoring is the visual relationship between the source element and the destination. The destination is visually "from" the source. Examples:

- A card detail view's initial position is the card's position.
- A sheet's transform origin is the trigger element's position.
- A popover is anchored to the source element's bounding box.

Source anchoring is a HEURISTIC. The implementer chooses the values. The principle is hard: the relationship is visible.

## Returning to previous context

The user must be able to return. The return affordance is:

- A back button in the destination's navigation chrome.
- The browser back on Web.
- The system Back gesture on iOS / iPadOS.
- ⌘[ on macOS.

The return affordance must:

- Return to the previous destination.
- Restore the source's scroll position (browser-native on Web; system-native on iPadOS multi-window).
- Restore the source's relevant state (selection, filter, etc.).
- Return focus to the source element (see `apple-modality-overlays/references/focus-and-dismissal.md` for focus return on dismissal; the principle is similar).

A destination that the user cannot return from is a defect.

## Shared-element transitions

A shared-element transition is a specific technique where an element visually persists from the source to the destination. The element changes position, size, and content during the transition. Examples:

- A list row that grows into a detail view.
- A thumbnail that grows into a media viewer.
- An icon that grows into a sheet's title.

### When shared elements are appropriate

- The source and destination share a single, named element (an image, an icon, a title).
- The transition can be implemented without breaking the layout.
- The element is the visual focus of the source.

### When shared elements are inappropriate

- The source and destination have multiple visually-prioritized elements; picking one breaks the others' visual hierarchy.
- The source and destination have no clear shared element.
- The transition would require hiding / re-arranging content in the source.
- The user has `prefers-reduced-motion: reduce`; shared-element transitions are large-amplitude motion (see reduced-motion treatment below).

### Reduced-motion adaptation

Shared-element transitions involve large amplitude motion. On `prefers-reduced-motion: reduce`:

- Replace the shared-element motion with a cross-fade.
- Keep the source anchoring (geometry, identity).
- Keep the focus return and back affordance.
- The user still understands the spatial relationship; the visual continuity is preserved without the large-amplitude motion.

## What spatial continuity is NOT

- It is NOT a heuristic for "use the same background image everywhere."
- It is NOT a requirement to copy visual elements verbatim.
- It is NOT a substitute for back navigation.
- It is NOT a substitute for clear hierarchy.

The principle is context preservation. The implementation is the designer's choice.

## Cross-Skill note

- The taxonomy of navigation events is `navigation-taxonomy.md` (this Skill).
- The scroll context chrome (navbar / title response) is `scroll-behavior.md`.
- The modal source-continuity (sheet / popover anchored to source) is `apple-modality-overlays/references/source-continuity.md`.
- The motion physics of a transition is `apple-motion-physics` (cited).
- The drag-to-dismiss gesture of a sheet is `apple-direct-manipulation` (gesture) + `apple-motion-physics` (physics).
