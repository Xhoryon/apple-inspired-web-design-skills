---
name: apple-adaptive-structure
description: Use when designing how the same application anatomy reorganizes across platform / width / window / input / context without losing task continuity. Owns the structural transformation vocabulary, the width / size-class model, the platform anatomy vocabulary, the window-state and input-context adaptations, the reversibility and task-continuity invariants, the structural anti-patterns, and the Web structural primitives (Container Queries, CSS Grid, View Transitions, Popover, Anchor Positioning). Do NOT use for component state semantics (use apple-control-states), pointer-target relationship (use apple-pointer-interaction), motion physics (use apple-motion-physics), navigation hierarchy (use apple-navigation-spatial), gesture recognition (use apple-direct-manipulation), modal type decision (use apple-modality-overlays), or material behavior (use apple-liquid-glass-fidelity). Do NOT use for page-level composition (stable apple-web-composition), component grammar (stable apple-ui-components), or page visual design (stable apple-design-foundations).
version: "0.4.0-dev"
license: MIT
---

# apple-adaptive-structure

Adaptive layout & platform structure. Owns how the same application anatomy reorganizes across platform / width / window / input / context without losing task continuity.

## When to use

Use when the task involves structural reorganization. Symptoms: "the same app should work on iPhone, iPad, and Mac", "at narrow width the sidebar becomes a tab bar", "Stage Manager window resize should change the multi-column", "the layout should reflow but keep the focused item", "visionOS should use a different structural family than iPad".

Do NOT use for: page-level composition (stable `apple-web-composition`); component grammar (stable `apple-ui-components`); state semantics (`apple-control-states`); pointer-target (`apple-pointer-interaction`); motion (`apple-motion-physics`); navigation hierarchy (`apple-navigation-spatial`); gesture (`apple-direct-manipulation`); modal type (`apple-modality-overlays`); material (`apple-liquid-glass-fidelity`).

## Core model

**Anatomy stays coherent; presentation may reorganize.** A `list → detail` relationship is the same anatomy whether it appears as a sidebar + content, a tab bar + drill-in, or a card stack. The Skill preserves the anatomy; the layout can change.

**Structural transformation vocabulary (Skill-owned).** Eight verbs describe how anatomy reorganizes. These are an **Apple Experience Skill design vocabulary**, NOT extracted from Apple HIG as a literal primitive set. Implementation uses the documented platform primitives (`NavigationSplitView`, `UISplitViewController.Style`, container queries, etc.).

- `inherit` — keep the same anatomy at the new context. Default.
- `slide` — child surface enters from an edge (popover / inspector).
- `push` — new surface pushed onto the navigation stack.
- `replace` — peer / tab switching; previous content preserved by the platform.
- `on-demand` — surface shown only when the user requests it.
- `reflow` — same anatomy reorganizes into a different form. Primary verb.
- `collapse` — surface removed; content preserved in nav stack / app state.
- `abandon` — anatomy incompatible with new context. Last resort.

Full verb × platform matrix and decision tree in `references/structural-taxonomy.md`.

**Size class vocabulary.** Apple exposes `compact` and `regular` per dimension via `UITraitCollection.horizontalSizeClass` / `verticalSizeClass` (UIKit) and `@Environment(\.horizontalSizeClass)` (SwiftUI). The Skill uses this vocabulary on Apple and pairs it with a Web width ladder (HEURISTIC). **Do NOT infer size class from device category or orientation alone** — read the trait at runtime. Full model in `references/size-classes-and-width.md`.

**Per-platform anatomy.** iPhone, iPad, Mac Catalyst, Mac, visionOS, tvOS, watchOS, and Web each have their own structural primitives and conventions. visionOS uses Window / Volume / Space (NOT iPad size classes). tvOS is focus-driven (`UIFocusSystem`, NOT the private `UIFocusEngine`). watchOS is stack-driven. Web uses CSS Grid + Container Queries. Full per-platform anatomy in `references/platform-anatomy.md`.

**Window state and input context.** Stage Manager (iPadOS 16+ / macOS 13+ — via `NavigationSplitView(columnVisibility:)` and `windowManagerRole(_:)` on `Scene`), multi-window, Slide Over, full-screen, PiP are window-state modifiers. Touch / pointer / keyboard / hybrid / focus-driven / gaze-driven / stylus are input contexts. Both modify the recommended anatomy. Full matrix in `references/window-state-and-input.md`.

## Hard invariants

1. **Anatomy stays coherent; presentation may reorganize.** Same anatomy across platforms; layout can change.
2. **Width over orientation.** Width and size class are the primary driver. Do NOT infer size class from device name or orientation — read the trait at runtime.
3. **Non-destructive adaptation.** Focus, selection, scroll, back stack preserved across structural change. Reject "structural change silently destroys task context."
4. **Reversibility where supported.** User can override (pin a sidebar open). Skill exposes the override.
5. **System primitive first.** Use `NavigationSplitView` / `TabView` / `NavigationStack` / `WindowGroup` / `UISplitViewController` / `NSWindow` / `UIWindowScene`. Stage Manager participation: `NavigationSplitView(columnVisibility:)` + `windowManagerRole(_:)` on `Scene`. **No `UIStageManager` class exists** — Stage Manager is a system experience.
6. **Web structural primitives first.** CSS Grid, Container Queries, View Transitions, Popover, Anchor Positioning. No JS for layout work CSS does declaratively.
7. **Structural honesty.** Navigation looks like navigation; content looks like content. Sidebar ↔ tab bar is allowed; tab-bar-item → content is not.
8. **No width-class monoculture.** Size class is one signal among many (window state, input context, user preference).
9. **No orientation primacy.** Portrait/landscape is a SECONDARY signal.
10. **No fixed geometry.** Geometry is fluid; use size class / container queries, not px.
11. **No modal-as-adaptive.** When reflow would do, do NOT open a sheet.
12. **Task continuity.** Focused item stays focused; selection preserved; scroll preserved; back stack intact.
13. **Nav-stack integrity on reflow.** Collapsing must NOT clear the drill-in.
14. **No sheet to compensate for reflow.** Preserve content in app state, not in a sheet.
15. **visionOS / watchOS / tvOS are separate families.** Do NOT apply iPad size classes. Each has its own primitives.
16. **Dynamic Type is orthogonal.** Independent of size class.
17. **7 stable v1.1 Skills are READ-ONLY.** Reference but do not modify.

Full anti-pattern library (14 entries) in `references/structural-anti-patterns.md`. Critical prohibitions (the 17 above) must be enforced at routing time.

## Routing integration

The orchestrator (`apple-experience-design`) adds a row for the new Skill. **Selective load rule:** the row applies ONLY when the task explicitly requires structural reorganization across platform / width / window / input / context. For ordinary responsive Web pages, visual-only requests, sheet / popover / menu / modal type decisions, button polish, or state semantics, the orchestrator routes to the appropriate Skills WITHOUT loading `apple-adaptive-structure`.

Discriminator checklist (the orchestrator uses these to gate loading):

- Does the task describe a reflow / collapse / re-organization across sizes, windows, or inputs? If yes, load.
- Is the task purely visual (color, typography, spacing, page composition)? If yes, do NOT load.
- Is the task a single component's state / behavior? If yes, do NOT load — use the relevant v0.3 Skill.
- Is the task about modal type (sheet vs popover vs menu vs alert)? If yes, do NOT load — use `apple-modality-overlays`.
- Is the task about material behavior on a control? If yes, do NOT load unless structural context is also required.
- Does the request explicitly require adaptive / responsive structure? If yes, load.

The Skill's own description (frontmatter `Do NOT use for …`) is the binding negative boundary. The orchestrator's routing matrix lists the row; the discriminator above prevents over-routing.

## Reference catalog (Tier 2)

- `references/structural-taxonomy.md` — verb set + decision tree + verb × platform matrix.
- `references/size-classes-and-width.md` — Apple size class + Web width ladder.
- `references/platform-anatomy.md` — per-platform structural primitives.
- `references/window-state-and-input.md` — 14 window states × 7 input contexts.
- `references/web-structural-primitives.md` — CSS Container Queries, Grid, View Transitions, Popover, Anchor Positioning.
- `references/task-continuity.md` — focus / selection / scroll / back stack + reversibility.
- `references/structural-anti-patterns.md` — 14 INTERNAL anti-patterns.
- `references/structural-audit.md` — per-platform + per-state + per-input audit checklist.

## Process notes

- The Skill does NOT introduce a new Apple API symbol. Apple API symbol scanner (extended for Adaptive Structure) is the gate.
- The 8-verb taxonomy is Skill-owned vocabulary, NOT Apple canonical.
- The Skill is verified by 20 functional tests (AE4-T1..T20) + 10 adversarial tests (AE4-G1..G10) + 8 router tests (AE4-R1..R8).
- The Skill consumes but does not redefine the v0.3 Skills.
- Stage Manager is iPadOS 16+ / macOS 13+ (iPad / Mac only). Not on iPhone.
- iPhone user-facing multi-window arrived with iOS 16 (Stage Manager). The `UIWindowScene` API has existed since iOS 13 — these are different facts.
