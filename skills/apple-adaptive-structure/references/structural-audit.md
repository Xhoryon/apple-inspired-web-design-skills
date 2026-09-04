# Structural Audit

How to audit a design for structural compliance. The Skill publishes a per-platform, per-state, per-input audit checklist. The audit is run on every design that touches adaptive structure.

## Audit categories

1. **Anatomy coherence** — does the same anatomy appear at multiple sizes / states / inputs?
2. **Task continuity** — does structural change preserve focus, selection, scroll, and back stack?
3. **Reversibility** — can the user override the structural choice?
4. **Primitive conformance** — does the design use the system primitive (Native) or the canonical Web primitive (Web)?
5. **Anti-pattern absence** — does the design avoid the 14 INTERNAL anti-patterns?
6. **Per-platform separation** — does the design respect the per-platform structural family separation (iPhone / iPad / Mac / visionOS / tvOS / watchOS / Web)?
7. **Window state handling** — does the design adapt to the window state?
8. **Input context handling** — does the design adapt to the input context?

## Per-platform audit (Apple)

### iPhone

- [ ] The bottom tab bar is the primary peer-navigation surface.
- [ ] Drill-in uses `NavigationStack` (or `UINavigationController`).
- [ ] No sidebar at compact-width size class.
- [ ] Sheets are used for focused tasks, not for missing columns.
- [ ] No 3-column anatomy.
- [ ] Multi-window (`UIWindowScene`) is supported if the app allows multiple instances.
- [ ] Dynamic Type is honored.

### iPad

- [ ] `NavigationSplitView` (2- or 3-column) is used for multi-column anatomy.
- [ ] The sidebar collapses to a tab bar / back button at compact-width size class.
- [ ] Stage Manager is honored: the layout adapts to the resized window width.
- [ ] Slide Over is honored: the layout uses the compact-width form.
- [ ] Multi-window is supported.
- [ ] Inspector is on-demand.
- [ ] No 1-column-forced design.
- [ ] `NavigationStack` is used inside the detail column of `NavigationSplitView`.

### Mac Catalyst

- [ ] Same as iPad (the iPad structural family).
- [ ] The app does NOT use `NSWindow` or `MenuBarExtra` from Mac Catalyst.
- [ ] The user can resize the window and the layout adapts.

### Mac (native)

- [ ] `NSWindow` + `NSWindowController` are the window primitives.
- [ ] SwiftUI `WindowGroup` for multiple windows with the same content.
- [ ] `MenuBarExtra` for menu bar items.
- [ ] `Settings { }` for the preferences window.
- [ ] `NSSplitViewController` (AppKit) or `NavigationSplitView` (SwiftUI) for multi-column.
- [ ] The window resize is honored.
- [ ] The Mac menu bar is NOT re-implemented.

### visionOS

- [ ] `WindowGroup` for 2D windows.
- [ ] `.volumetric` for 3D volumes.
- [ ] `immersiveSpace` for full-immersion scenes.
- [ ] `.ornament` for chrome attached to a 3D scene.
- [ ] `RealityView` for 3D content.
- [ ] No iPad multi-column pattern applied to a visionOS window.
- [ ] No 2D-only UI in an immersive scene; the Skill uses 3D primitives.

### tvOS

- [ ] Focus system (`UIFocusSystem`, `.focusable`, `@FocusState`) is the primary input.
- [ ] `NavigationStack` (focus-driven).
- [ ] `TabView` (focus-driven).
- [ ] No touch-driven affordances.
- [ ] No 3-column anatomy.

### watchOS

- [ ] `NavigationStack` (vertical stack).
- [ ] `TabView` (Smart Stack).
- [ ] No 3-column anatomy.
- [ ] No sidebar.
- [ ] Digital Crown is the scroll / navigate input.

## Per-platform audit (Web)

### Desktop Web

- [ ] CSS Grid is used for multi-column.
- [ ] Container queries are used for component-level adaptivity.
- [ ] Viewport media queries are used for page-level adaptivity.
- [ ] Popover API is used for non-modal auxiliary surfaces.
- [ ] CSS Anchor Positioning is used for popover anchoring (where supported).
- [ ] View Transitions API is used for in-page transitions (where supported).
- [ ] `@media (prefers-reduced-motion)` and `@media (prefers-reduced-transparency)` are honored.
- [ ] Dynamic Type is honored via `rem` / `em` units and `font-size: clamp()`.
- [ ] No `display: flex` for primary 2-D layout.
- [ ] No fixed px-widths as structural anchors.
- [ ] No JavaScript-driven layout for work CSS can do declaratively.

### Touch Web

- [ ] Card stack is the default at compact width.
- [ ] No hover affordances.
- [ ] `@media (hover: hover) and (pointer: fine)` is used to gate hover-only behavior.
- [ ] Touch targets are ≥ 44×44 px.
- [ ] Popover API is used (where supported) for inspector / secondary detail.
- [ ] Viewport meta tag is set correctly.
- [ ] No forced multi-column at compact width.

### PWA

- [ ] Web App Manifest is present and valid.
- [ ] `display: standalone` / `minimal-ui` / `fullscreen` modes are honored.
- [ ] Safe-area insets are honored.
- [ ] Theme color and background color are set.
- [ ] Icons are present (multiple sizes).
- [ ] Install prompt is exposed (where appropriate).

## Per-window-state audit

For each window state, the design must:

- [ ] Adapt to the resized width.
- [ ] Preserve the open task.
- [ ] Honor the input context.
- [ ] Honor the user's override.

## Per-input-context audit

For each input context, the design must:

- [ ] Expose input-appropriate affordances.
- [ ] Not expose input-inappropriate affordances (e.g. no hover on touch).
- [ ] Provide alternative input (keyboard accessibility).

## Task-continuity audit

For every structural change, the design must:

- [ ] Preserve focus (or explicitly opt out with a written justification).
- [ ] Preserve selection (or explicitly opt out).
- [ ] Preserve scroll position (or explicitly opt out).
- [ ] Preserve back stack (or explicitly opt out).
- [ ] Expose the user's override affordance (where supported).

## Anti-pattern audit

For each of the 14 INTERNAL anti-patterns, the design must NOT exhibit the pattern. If the design does exhibit the pattern, it is a candidate for rejection.

## What the audit is NOT

- The audit is NOT a check for visual design quality. The Skill does NOT own visual design; the stable `apple-design-foundations` and `apple-design-audit` do.
- The audit is NOT a check for component grammar. The stable `apple-ui-components` does that.
- The audit is NOT a check for motion physics. The `apple-motion-physics` does that.
- The audit is NOT a check for navigation hierarchy. The `apple-navigation-spatial` does that.
- The audit is NOT a check for material behavior. The `apple-liquid-glass-fidelity` does that.

The audit is a structural compliance check. It answers: does the design use the canonical structural primitives, the canonical verb set, the canonical task-continuity rules, and avoid the INTERNAL anti-patterns?

## When to run the audit

- After a design change that touches the layout, the multi-column, the window state, the input context, the task continuity, or the reversibility.
- Before shipping a release.
- During a peer review.
- During the Batch 4 evaluation (AE4-T1..T20 + AE4-G1..G10).

## What to do if the audit fails

1. Identify the failing category.
2. Reference the relevant Skill section.
3. Fix the design.
4. Re-run the audit.
5. If the design cannot be fixed, mark the design as a candidate for rejection or ADAPT.

## Companion files

- `structural-taxonomy.md` — the canonical verb set.
- `size-classes-and-width.md` — the size class model and width ladder.
- `platform-anatomy.md` — per-platform anatomy.
- `window-state-and-input.md` — window state and input context.
- `web-structural-primitives.md` — the canonical Web primitives.
- `task-continuity.md` — the four pillars.
- `structural-anti-patterns.md` — the 14 INTERNAL anti-patterns.
