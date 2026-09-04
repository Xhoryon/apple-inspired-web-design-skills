# Apple Experience — Accessibility Contract

**Date:** 2026-09-03
**Status:** Public cross-cutting contract
**Scope:** project-level cross-cutting contract (not Skill 14; not a per-Skill concern)

This contract establishes accessibility invariants that apply across the whole Apple Experience system, regardless of which Skill owns the local interaction. It is NOT a replacement for local Skill accessibility guidance. It is a project-level rule.

---

## Purpose

The project-level accessibility contract ensures:

1. Local Skills do not silently weaken accessibility requirements.
2. Cross-cutting accessibility concerns (Reduced Motion, single-channel dependence, keyboard / focus) are enforced at the project level.
3. The contract does NOT create an Accessibility Skill. It does NOT duplicate entire HIG accessibility guidance. It defines the cross-cutting invariant.

---

## Cross-cutting accessibility invariants

### Meaning cannot depend on one sensory channel alone

| Channel | If absent, accessible alternative required? |
|---|---|
| Color | yes — WCAG 2.2 SC 1.4.1 (color is not the only means). Error / success / warning state must be conveyed by icon + text label + state, not by color alone. |
| Haptic | yes — essential information must NOT depend on haptic alone. Haptic is supplementary, never the only accessible channel. |
| Motion | yes — Reduced Motion must preserve task and information. Information ordering, explanation, and destination must remain. Motion is the optional channel. |
| Audio | yes — essential information must NOT depend on audio alone. Captions / transcripts / visual alternatives for audio-essential information. |
| Sound-only error | reject — system feedback must NOT be sound-only. |

These are project-level rules. Implementation details (color palette, motion duration, haptic pattern) remain with domain Skills.

### Reduced Motion preserves task and information

Already established in Motion and Visual Media Skills. The project-level contract references this rule. It does NOT duplicate the mechanics.

- Remove / shorten secondary motion.
- Keep key poses; fade between them.
- Preserve information ordering, explanation, destination.
- Do NOT merely set `animation-duration: 0` and call reduced motion done.

### Keyboard / focus accessibility

- Important actions remain reachable / understandable through appropriate supported input.
- Keyboard / focus concerns route to `apple-keyboard-focus-commands` for keyboard semantics; to `apple-control-states` for focused visual state.
- This contract does NOT claim every platform has identical keyboard behavior. It enforces that the platform-appropriate keyboard support exists where the task is keyboard-accessible.

### System settings / preferences

- Respect Apple platform accessibility settings (Reduce Motion, Increase Contrast, Reduce Transparency, VoiceOver, etc.).
- Respect Web accessibility media queries (`prefers-reduced-motion`, `prefers-reduced-transparency`, `prefers-contrast`).
- Do NOT simulate native accessibility behavior incorrectly on Web. If a system setting does not exist on Web, use the Web equivalent or document the gap.

### Media accessibility

- Route captions / transcripts / descriptions / semantic alternatives to the appropriate Skill.
- Visual Media owns media-level accessibility (alt text, captions, descriptions).
- This contract enforces that media accessibility is NOT omitted; the implementation lives in the media Skill.

### Adaptation must not destroy accessibility state

- Focus, task context, and accessible state should survive reasonable structural adaptation.
- `apple-adaptive-structure` owns the structural transformation; the contract enforces that accessibility state is preserved across that transformation.
- Resetting focus / context purely because the layout changed is an accessibility regression.

---

## Accessibility ownership

The project-level contract owns **cross-cutting invariants**. It does NOT own detailed implementation.

| Concern | Owner |
|---|---|
| Reduced Motion mechanics | `apple-motion-physics` + `apple-visual-media-composition` |
| Focus model | `apple-keyboard-focus-commands` |
| Text editing semantics | `apple-text-input-editing` |
| Modal focus return | `apple-modality-overlays` + `apple-keyboard-focus-commands` |
| State communication | `apple-feedback-response` + `apple-control-states` |
| Media alternatives | `apple-visual-media-composition` |
| Adaptive structural continuity | `apple-adaptive-structure` |
| General visual hierarchy | stable v1.1 `apple-design-foundations` |
| Pointer / touch | `apple-pointer-interaction` |
| Component visual state | stable v1.1 `apple-ui-components` |

The contract enforces that the domain Skills honor these cross-cutting rules. Domain Skills retain the implementation details.

---

## Scope-Proportional Process

- Small scoped tasks do NOT trigger unnecessary accessibility bureaucracy.
- Whole-product / whole-site accessibility requires evidence-first workflow.
- Asset Manifest may include accessibility constraints (captions required / alt text required / Reduced Motion support required).

---

## Evidence labels (interaction with Capability Contract)

The accessibility contract integrates with `APPLE-EXPERIENCE-CAPABILITY-EVIDENCE-CONTRACT.md`:

- `DIRECT VISUAL INSPECTION` may be required to confirm a visual hierarchy is accessible (color contrast, focus indicator visibility).
- `DELEGATED VISUAL INSPECTION` may be required when the current Agent cannot perform visual accessibility review.
- `USER-SUPPLIED DESCRIPTION` is acceptable for accessibility requirements (e.g. user confirms "this section requires a video caption" even if the Agent cannot view the video).

---

## Hard invariants

1. Essential meaning must NOT depend on color alone.
2. Essential meaning must NOT depend on haptic alone.
3. Essential narrative must NOT depend on motion alone.
4. Essential meaning must NOT depend on audio alone.
5. Essential information must NOT be conveyed via sound-only error.
6. Reduced Motion must preserve task and information; it is NOT a deletion.
7. Adaptive reflow must NOT casually lose accessible focus / task context.
8. Media accessibility alternatives must be routed, not omitted.
9. Local Skills may NOT silently override accessibility requirements.
10. Cross-cutting concerns remain cross-cutting; they do NOT create Skill 14.

---

## Failure modes this contract prevents

- "Color alone is enough to communicate destructive state." (REJECT.)
- "Haptic alone confirms success." (REJECT for essential meaning.)
- "Reduce Motion means remove the feature explanation." (REJECT.)
- "Touch users can discover this only by hover." (REJECT.)
- "Keyboard support means every platform must use desktop shortcuts." (REJECT.)
- "Accessibility contract owns focus implementation instead of Keyboard/Focus." (REJECT.)
- "Because VoiceOver exists on Apple platforms, Web can pretend identical native semantics." (REJECT.)
- "Modal dismissal can lose previous focus because accessibility is handled elsewhere." (REJECT.)
- "Responsive reflow may reset focused task context every time layout changes." (REJECT where continuity should be preserved.)
- "Captions concern means Visual Media should implement the entire video-player architecture." (REJECT — captions route, not own playback.)
- "Every accessibility requirement should become its own Skill." (REJECT.)
- "Accessibility means making every platform look and behave identically." (REJECT.)
- "If a task is mouse-accessible, keyboard / focus can always be ignored." (REJECT where keyboard / accessibility applicability requires support.)
- "Cross-cutting accessibility contract means local Skills no longer need any accessibility guidance." (REJECT — local Skills retain domain-specific accessibility consequences.)
