---
name: apple-experience-design
description: Use when designing or reviewing an Apple-platform interaction behavior — press feedback, hover, pointer highlight, shared highlight, selection, focus, spring motion, navigation, drag, sheet/popover/menu/alert, advanced Liquid Glass, adaptive layout, system feedback, keyboard/focus/command, text input/editing/selection, visual media/composition, or any behavior whose intent comes from Apple platform UX. First determine platform, input, capability, and behavior; then route to the correct combination of apple-control-states, apple-pointer-interaction, apple-motion-physics, apple-navigation-spatial, apple-direct-manipulation, apple-modality-overlays, apple-liquid-glass-fidelity, apple-adaptive-structure, apple-feedback-response, apple-keyboard-focus-commands, apple-text-input-editing, apple-visual-media-composition, and the existing stable v1.1 Web Skills. Do NOT use as a replacement for apple-web-design.
version: "0.8.0-dev"
license: MIT
---

# apple-experience-design

The interaction-behavior orchestrator. Routes to the right combination of the 13 Apple Experience Skills (7 v0.2 + 1 v0.3 + 1 v0.4 + 1 v0.5 + 1 v0.6 + 1 v0.7 + 1 v0.8) and the stable v1.1 Web Skills.

## When to use

Use when the task involves an Apple-platform interaction behavior. Symptoms: "make this button feel more like iOS", "build a Hero section", "this view is too dense", "where should focus go after the sheet closes", "save shortcut doesn't work in editor".

Do NOT use for: page-level composition (stable v1.1 `apple-web-design`); component visual appearance (stable v1.1 `apple-ui-components`); typography (stable v1.1 `apple-design-foundations`).

## Nine principles (canonical)

1. **Platform fidelity must never override platform appropriateness.** Every behavior carries a verdict per platform and per input.
2. **System primitive first.** If the system component provides the behavior, use it. Do not re-implement.
3. **Preserve interaction intent, not implementation literalism.** iPadOS pointer highlight on Web becomes a target highlight surface. iPhone touch-only ABANDONs the pointer concept. iOS edge swipe back on Web becomes the browser's native Back gesture.
4. **Glass is a participant, not decoration (v0.3).** Liquid Glass integrates with state, motion, pointer, navigation, modality.
5. **Anatomy stays coherent; presentation may reorganize (v0.4).** Same anatomy across platforms; layout reorganizes to fit.
6. **Feedback is the system consequence layer (v0.5).** Status / progress / outcome / recovery / haptics.
7. **Focus, command, keyboard flow (v0.6).** Focus movement / restoration; keyboard shortcuts; command scope; keyboard alternatives.
8. **Text input is system-text-control-first (v0.7).** Composition / marked text / caret / selection / standard edit actions / plain vs rich text / paste policy.
9. **Visual media is asset-and-role first (v0.8).** Media role determines prominence; asset capability determines visual ambition; whole → detail → whole; Hero is optional; reduced motion preserves narrative; user-owned state ≠ scroll-owned state.

## Routing decision (capability-gated)

Step 1 — Platform. Step 2 — Input scope. Step 3 — Behavior. Step 4 — Look up verdict in `APPLE-EXPERIENCE-PLATFORM-MATRIX-v0.X.md`. Step 5 — Route.

| Verdict | Loads |
|---|---|
| NATIVE | System primitive + the relevant Apple Experience Skills for state / pointer / motion / nav / modality |
| DIRECT on Web | The relevant Apple Experience Skills + the stable v1.1 Skills for component grammar and page composition |
| APPROXIMATE on Web | Same as DIRECT, with explicit "approximation" tagging |
| ADAPT | Replacement concrete behavior + relevant Skills + stable v1.1 Skills |
| ABANDON | This Skill only; no further Skill loads required |

## Routing matrix

| Task family | Required | Conditional | Usually skip |
|---|---|---|---|
| Atomic component polish | `apple-control-states` | stable v1.1 `apple-design-foundations` (token reference) | motion, pointer, nav, direct manipulation, modality, **visual-media** |
| Button / toggle | `apple-control-states` | `apple-motion-physics` (if nuance), `apple-modality-overlays` (if opens sheet/popover) | nav, direct manipulation, scroll |
| Navbar / toolbar | `apple-control-states`, `apple-navigation-spatial` | `apple-motion-physics`, `apple-modality-overlays` | direct manipulation (unless drag), **visual-media** |
| Page visual design | stable v1.1 `apple-web-design`, `apple-design-foundations`, `apple-web-composition` | `apple-ui-components` | experimental Skills unless interaction involved |
| Landing / product page | stable v1.1 `apple-design-foundations`, `apple-web-composition` | `apple-ui-components`, `apple-motion-interaction` (stable v1.1), `apple-liquid-glass-web` (stable v1.1); **`apple-visual-media-composition` (v0.8) for Hero / media composition** | experimental Skills unless interaction is involved |
| Dense dashboard | stable v1.1 `apple-design-foundations`, `apple-ui-components`, `apple-web-composition` | `apple-motion-physics` (subtle); **`apple-adaptive-structure` for responsive grids** | experimental Skills unless interaction is involved |
| CJK / Chinese product | stable v1.1 `apple-design-foundations` (CJK reference) | `apple-text-input-editing` (v0.7) if interaction is involved | full text-editing Skill if text-only |
| Existing-brand redesign | stable v1.1 `apple-design-foundations` | `apple-visual-media-composition` (v0.8) for media | preserve brand tokens; do not erase |
| **List → detail navigation** | `apple-navigation-spatial` | `apple-motion-physics` (transitions), `apple-modality-overlays` (if detail is modal) | direct manipulation (unless row is also draggable), pointer highlight |
| **Drag / direct manipulation** | `apple-direct-manipulation` | `apple-control-states` (state during drag), `apple-motion-physics` (post-release) | nav, modality, scroll |
| **Sheet / modal task** | `apple-modality-overlays` | `apple-navigation-spatial` (if sheet participates in nav stack), `apple-motion-physics` (entry / dismiss), `apple-direct-manipulation` (if interactive dismiss) | pointer highlight (sheet content usually doesn't need it) |
| **Popover / menu** | `apple-modality-overlays` | `apple-control-states` (selected state), `apple-pointer-interaction` (menu item hover), `apple-navigation-spatial` (popover anchoring) | direct manipulation (unless drag-to-dismiss) |
| **Scroll context chrome** | `apple-navigation-spatial` (reference) | `apple-motion-physics` (scroll-linked transitions) | full nav (unless scroll is part of nav) |
| **Advanced Liquid Glass material** | experimental `apple-liquid-glass-fidelity` | `apple-control-states` (state response on glass), `apple-motion-physics` (spring / morph), `apple-pointer-interaction` (hover / highlight on glass), `apple-navigation-spatial` (group morph), `apple-modality-overlays` (source continuity) | stable v1.1 `apple-liquid-glass-web` (do not double-load for baseline material) |
| **Adaptive structure** | experimental `apple-adaptive-structure` | stable v1.1 `apple-web-composition`, `apple-ui-components`, `apple-navigation-spatial`, `apple-modality-overlays`, `apple-control-states`; **`apple-text-input-editing` (v0.7) if focus on text editors inside the structure** | liquid-glass-fidelity (unless material + structural), pointer-interaction (unless structural pointer behavior) |
| **Feedback / system response** | experimental `apple-feedback-response` | `apple-modality-overlays` (interruption), `apple-adaptive-structure` (if feedback must survive reflow), `apple-control-states`, `apple-motion-physics` | pointer, navigation, direct manipulation |
| **Keyboard / focus / commands** | experimental `apple-keyboard-focus-commands` | `apple-modality-overlays`, `apple-adaptive-structure`, `apple-navigation-spatial`, `apple-direct-manipulation`, `apple-control-states`, `apple-motion-physics` | liquid-glass-fidelity, feedback-response |
| **Text input / editing / selection** | experimental `apple-text-input-editing` | `apple-keyboard-focus-commands` (which editor is focused), `apple-control-states` (editor chrome), `apple-direct-manipulation` (pointer / touch selection), `apple-feedback-response` (validation outcome timing) | motion (unless caret/IME animation), modality (unless modal contains text editor), navigation |
| **Visual media / composition (v0.8)** | experimental `apple-visual-media-composition` | `apple-motion-physics` (scroll cinematic / camera-like motion), `apple-direct-manipulation` (drag orbit / pinch zoom), `apple-adaptive-structure` (responsive art direction), `apple-keyboard-focus-commands` (focus inside media explorers), `apple-control-states` (selected media state), `apple-feedback-response` (loading / failure status) | navigation (unless media is destination), text-editing (unless text editor inside media inspector) |
| Explicit Liquid Glass task | stable v1.1 `apple-liquid-glass-web` (baseline) | experimental `apple-liquid-glass-fidelity` (if adaptive / interactive / morphing) | Batch 1 reference |
| Audit-only task | stable v1.1 `apple-design-audit` | stable v1.1 browser audit adapter | everything else |

The matrix says what to load and what NOT to load. Default bias: "fewer Skills" — over-routing is the biggest waste.

## Examples

20 worked routing examples are in `references/routing-examples.md` (Tier 2). Each maps a natural-language request to the verdict and the Skills to load.

## Reference catalog (Tier 2)

- `references/routing-examples.md` — 20 worked routing examples.
- `references/platform-applicability.md` — full platform-applicability contract.
- `references/state-model.md` — control state model and state-availability matrix.
- `references/pointer-highlight.md` — pointer target highlight.
- `references/cursor-rule.md` — Web cursor non-replacement rule.
- `references/presentation-taxonomy.md` — sheet / popover / menu / alert / action sheet.
- `../apple-pointer-interaction/references/shared-highlight.md`.
- `../apple-motion-physics/references/{spring-response,retargeting,reduced-motion}.md`.
- `../apple-navigation-spatial/references/{navigation-taxonomy,spatial-continuity,scroll-behavior}.md`.
- `../apple-direct-manipulation/references/{gesture-taxonomy,drag-lifecycle,gesture-conflicts}.md`.
- `../apple-modality-overlays/references/focus-and-dismissal.md`.

## Hard invariants

1. The 9 principles above are non-negotiable.
2. The platform-applicability verdict is required for every behavior.
3. Native cursor on Web is not replaced; pointer magnetism on Web is ABANDON.
4. Apple spring numeric values are HEURISTICS, not Apple canonical.
5. The 7 stable v1.1 Skills are READ-ONLY.
6. iOS 26-specific behavior is VERSION-SPECIFIC.
7. Scroll is owned by the platform; the Skill does not hijack native browser / platform scrolling.
8. Custom gestures are ABANDON when standard gestures suffice.
9. Important actions must have alternative input. Gestures are not the only way.

## Companion files

- All references above (`../apple-{name}/references/...`).
- `APPLE-EXPERIENCE-PLATFORM-MATRIX-v0.X.md` for per-platform verdicts.
- `APPLE-EXPERIENCE-FOUNDATION-ARCHITECTURE-v0.X.md` for Skill ownership.
- `scripts/audit-apple-api-symbols.py` for the multi-domain Apple API scanner (gate for canonical Apple API claims).
- `scripts/validate-skill-references.py` for the reference-integrity validator.
- `scripts/count-skill-body-words.py` for the Tier 1 body-word counter.
