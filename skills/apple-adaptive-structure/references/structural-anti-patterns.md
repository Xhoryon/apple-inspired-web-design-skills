# Structural Anti-Patterns

The Skill's INTERNAL anti-pattern candidates. Each has a written rejection. These are candidates; the Skill does NOT promote them to the public anti-pattern library without independent review.

## 1. Orientation Primacy

**Pattern:** Using orientation as the primary layout driver instead of width / size class.

**Example:** `@media (orientation: landscape) { .layout { ... } }` as the primary structural rule.

**Why it's wrong:** Per Apple HIG guidance, the available width and the size class (not the orientation) should determine the layout. An iPhone in landscape may report `compact-width` or `regular-width` depending on the model and the current traits; an iPad in portrait reports `regular-width`. Orientation is a SECONDARY signal — not a derivation rule. (Layer B Skill synthesis derived from current Apple HIG adaptive-design guidance.)

**Rejection:** The Skill rejects Orientation Primacy. The structural rules reference width / size class, not orientation.

**Replaced by:** Width / size class as the primary driver; orientation as a SECONDARY signal (e.g. for portrait-vs-landscape-specific affordances like the home indicator).

## 2. Width-Class Monoculture

**Pattern:** Using only `@media (min-width: ...)` for all structural decisions.

**Example:** A layout that switches to 2-column at 600 px, 3-column at 1024 px, with no other modifiers.

**Why it's wrong:** The width class is one signal among many. Window state, input context, user preference, and content density also matter. A 1024 px window in Stage Manager with a pointer has different affordances than a 1024 px window in a touch-only context.

**Rejection:** The Skill rejects Width-Class Monoculture. The Skill pairs width with size class + window state + input context + user preference.

**Replaced by:** The Skill's matrix (in `window-state-and-input.md`) which pairs width with the other modifiers.

## 3. Task Identity Loss

**Pattern:** Sidebar / inspector / detail collapsing without preserving the open task.

**Example:** The user clicks a list item; the detail opens. The user resizes the window; the sidebar collapses; the detail is replaced with the list. The user's selected item is gone.

**Why it's wrong:** The user's open task is the detail view, not the list. Collapsing the sidebar should NOT collapse the detail.

**Rejection:** The Skill rejects Task Identity Loss. The Skill publishes the `task-continuity` invariant: structural change must not silently destroy the open task. The Skill preserves focus, selection, scroll position, and back stack across reflow.

**Replaced by:** The Skill's task-continuity reference (`task-continuity.md`).

## 4. Always Sidebar

**Pattern:** Forcing a sidebar on widths that cannot afford one.

**Example:** A 360 px Web layout with a 280 px sidebar; the content area is 60 px wide.

**Why it's wrong:** The sidebar consumes horizontal space that the content needs. The user cannot read the content. The sidebar should be on-demand or absent at compact width.

**Rejection:** The Skill rejects Always Sidebar. The sidebar is visible at regular width; on-demand or absent at compact width. The Skill preserves the sidebar's content in the back stack or off-screen state.

**Replaced by:** The Skill's verb set (in `structural-taxonomy.md`) where `collapse` and `on-demand` are the canonical verbs for the sidebar at compact width.

## 5. Fixed Geometry

**Pattern:** Using px-widths as structural anchors.

**Example:** `width: 320px; min-width: 320px; max-width: 320px` as a structural rule.

**Why it's wrong:** The width depends on the device, the size class, the window state, the input context, and the user's Dynamic Type setting. Pinned px-widths do not adapt.

**Rejection:** The Skill rejects Fixed Geometry. Geometry is fluid. The Skill uses `min()` / `max()` / `clamp()` and container queries for fluid geometry.

**Replaced by:** The Skill's web-structural-primitives reference (`web-structural-primitives.md`) which uses Container Queries + CSS Grid + View Transitions.

## 6. Modal As Adaptive

**Pattern:** Opening a sheet when a layout reflow would do.

**Example:** The user resizes the window to narrow; instead of reflowing the multi-column layout to a single column, the app opens a sheet that overlays the column.

**Why it's wrong:** Sheets are for focused tasks, not for papering over a missing anatomy. The user should see the reflowed layout, not a modal overlay.

**Rejection:** The Skill rejects Modal As Adaptive as a structural answer. The Skill reserves sheets for focused tasks. The Skill publishes: "If a layout reflow would do, the Skill does NOT open a sheet."

**Replaced by:** The Skill's verb `reflow` (in `structural-taxonomy.md`).

## 7. Tablet-as-Phone-XL

**Pattern:** Treating iPad as a bigger iPhone.

**Example:** An iPad app that uses a bottom tab bar + drill-in via NavigationStack, with no multi-column structure. The iPad is treated as a wider iPhone.

**Why it's wrong:** iPad has its own structural vocabulary: `NavigationSplitView`, multi-window, Stage Manager, Slide Over, and a regular-width size class. iPad is not a wider iPhone.

**Rejection:** The Skill rejects Tablet-as-Phone-XL. The Skill publishes per-platform anatomy (`platform-anatomy.md`) where iPad has a distinct structural primitive set.

**Replaced by:** The Skill's iPad anatomy section in `platform-anatomy.md`.

## 8. Mac-as-iPad

**Pattern:** Using only iPad structural patterns on Mac.

**Example:** A Mac Catalyst app that uses only `NavigationStack` and a tab bar, with no menu bar, no `MenuBarExtra`, no `Settings { }` window, no `NSWindow` chrome.

**Why it's wrong:** Mac has its own structural primitives: `NSWindow`, `MenuBarExtra`, `Settings { }`, the standard Mac menu bar. The Mac user expects these.

**Rejection:** The Skill rejects Mac-as-iPad. The Skill publishes per-platform anatomy where Mac has a distinct structural primitive set.

**Replaced by:** The Skill's Mac anatomy section in `platform-anatomy.md`.

## 9. Stage Manager = iPhone

**Pattern:** Treating Stage Manager as a generic windowed mode without considering the resized width.

**Example:** A Stage Manager app that always uses the iPhone anatomy regardless of the resized window width.

**Why it's wrong:** Stage Manager is iPad / Mac only; the resized window can be narrow or wide. The anatomy must adapt to the resized width.

**Rejection:** The Skill rejects "Stage Manager = iPhone" as a rule. The Skill publishes a window-state × input-context matrix (`window-state-and-input.md`) where Stage Manager anatomy is the compact form at narrow widths and the regular form at wide widths.

**Replaced by:** The Skill's Stage Manager section in `platform-anatomy.md` and the window-state matrix in `window-state-and-input.md`.

## 10. visionOS = iPad with Depth

**Pattern:** Applying iPad structural patterns to visionOS.

**Example:** A visionOS app that uses `NavigationSplitView` for multi-column, treating the visionOS window as an iPad window.

**Why it's wrong:** visionOS has its own spatial primitives: `WindowGroup`, `.volumetric`, `immersiveSpace`, `.ornament`, `RealityView`, `Attachment`. visionOS does NOT use the size class model.

**Rejection:** The Skill rejects visionOS = iPad with Depth. The Skill publishes per-platform anatomy where visionOS is a separate structural family.

**Replaced by:** The Skill's visionOS anatomy section in `platform-anatomy.md`.

## 11. Structural Re-Implementation on Native

**Pattern:** Re-implementing `NavigationSplitView` / `TabView` / `NSWindow` in custom code.

**Example:** A SwiftUI app that uses `HStack { if condition { SidebarView() } ContentView() }` to emulate `NavigationSplitView`.

**Why it's wrong:** The system provides the structural primitive. Re-implementing it loses the system behavior: state preservation, animations, accessibility, Dynamic Type integration, multi-window support.

**Rejection:** The Skill rejects Structural Re-Implementation on Native. The Skill routes to the system primitive. Custom re-implementations require a written justification.

**Replaced by:** The Skill's platform anatomy (`platform-anatomy.md`) which lists the canonical native primitives.

## 12. Containerless Component Adaptivity

**Pattern:** Components that adapt only via viewport media queries, not container queries.

**Example:** A card component that switches to 2-column at `@media (min-width: 600px)` (viewport), not `@container card (min-width: 600px)`.

**Why it's wrong:** The component should adapt to its container's width, not the viewport's. A card in a sidebar should not be 2-column just because the viewport is wide; it should be 1-column because the sidebar is narrow.

**Rejection:** The Skill rejects Containerless Component Adaptivity. The Skill uses container queries for component-level adaptivity and viewport media queries for page-level adaptivity.

**Replaced by:** The Skill's web-structural-primitives reference (`web-structural-primitives.md`).

## 13. Nav-Stack Clear on Reflow

**Pattern:** Clearing the navigation stack when the layout reflows.

**Example:** A 3-column iPad app where collapsing the sidebar also clears the drill-in stack in the detail.

**Why it's wrong:** The drill-in stack is part of the user's task. Clearing it destroys the task.

**Rejection:** The Skill rejects Nav-Stack Clear on Reflow. The Skill preserves the back stack across reflow.

**Replaced by:** The Skill's task-continuity reference (`task-continuity.md`), pillar 4.

## 14. Sheet-During-Reflow

**Pattern:** Opening a sheet to preserve content during a reflow.

**Example:** The user resizes the window; the multi-column layout reflows; the system opens a sheet to preserve the column that would otherwise disappear.

**Why it's wrong:** Sheets are for focused tasks, not for reflow preservation. The column should be preserved in the back stack or off-screen state, not in a sheet.

**Rejection:** The Skill rejects Sheet-During-Reflow. The Skill preserves content in the back stack or off-screen state, not in a sheet.

**Replaced by:** The Skill's task-continuity reference (`task-continuity.md`).

## What the Skill rejects

The Skill rejects all 14 patterns. Each is documented with a written rationale. The Skill does NOT promote these to the public anti-pattern library without independent review. They are INTERNAL candidates.

## How to add a new anti-pattern

1. Write the pattern, why it's wrong, and the replacement.
2. Add the pattern to this file.
3. Add a check to `structural-audit.md`.
4. Add a test (AE4-G*) to the Batch 4 test set.
5. The new anti-pattern is a CANDIDATE. It is INTERNAL until independent review.
