---
name: apple-feedback-response
description: Use when designing how the system communicates what is happening, what happened, what requires attention, and how people can recover. Owns the feedback intent taxonomy, the significance → interruption model, the multimodal channel coordination, the haptic semantic mapping, the progress / status model (determinate / indeterminate), the undo / recovery semantics, and the accessibility constraints on feedback channels. Applies to Native iOS / iPadOS / macOS / Mac Catalyst / visionOS / tvOS / watchOS and Web (Desktop / Touch / PWA). Do NOT use for pressed / hover / focused / selected state semantics (use apple-control-states), transition physics (use apple-motion-physics), gesture / drag mechanics (use apple-direct-manipulation), modal type decision (use apple-modality-overlays), navigation history / back (use apple-navigation-spatial), structural reflow (use apple-adaptive-structure), Liquid Glass material behavior (use apple-liquid-glass-fidelity), or basic component appearance (use stable apple-ui-components).
version: "0.5.0-dev"
license: MIT
---

# apple-feedback-response

Feedback & System Response. Owns how the system communicates status, consequence, significance, completion, failure, and recovery through appropriate feedback channels (visual, text, motion, haptic, audio, multimodal). Does NOT own the underlying control state, motion, gesture, modality, navigation, structural, material, or component behaviors.

## When to use

Use when the task involves system feedback: status, progress, success / warning / error, recovery / undo, haptic mapping, multimodal coordination, or feedback accessibility. Symptoms: "should I show a success animation when the save finishes", "what haptic should play on alignment", "how should the export progress look", "should every delete need a confirmation alert", "the spinner is stuck — what should happen", "error needs to explain how to recover".

Do NOT use for: pressed / hover / focused / selected state semantics (`apple-control-states`); transition physics (`apple-motion-physics`); gesture / drag (`apple-direct-manipulation`); modal type decision (`apple-modality-overlays`); navigation history / back (`apple-navigation-spatial`); structural reflow (`apple-adaptive-structure`); Liquid Glass (`apple-liquid-glass-fidelity`); component appearance (stable v1.1 `apple-ui-components`).

## Core model (Skill-owned vocabulary)

The Skill uses a **feedback lifecycle** model. This is **Apple Experience Skill implementation vocabulary**, NOT extracted from Apple HIG as a literal primitive set.

```
Event / State Change
        ↓
Meaning (what just happened)
        ↓
Significance (how much it matters)
        ↓
Feedback Intent (acknowledge / status / progress / outcome / warn / recover / guide)
        ↓
Channel Selection (visual / text / motion / haptic / audio)
        ↓
Delivery (in-context / focused / interruptive)
        ↓
Recovery / Next Action (what can the person do now)
```

The Skill's **feedback intent taxonomy** (Skill-owned, not Apple verbatim):

- **Acknowledge** — the system registered an action.
- **Status** — what is currently happening.
- **Progress** — how an ongoing task advances.
- **Outcome** — success / completion / failure.
- **Warn** — a meaningful negative consequence may occur.
- **Recover** — how to correct or reverse a problem.
- **Guide** — what can happen next.

The Skill's **significance ladder** (Skill-owned synthesis):

| Significance | Delivery | Examples |
|---|---|---|
| Low | local / passive / in-context | slider value changing; selection moving through options |
| Medium | noticeable / focused / non-modal | routine completion; expected action result |
| High | actionable / prominent | failure with recovery path; expected irreversible action |
| Critical | interruptive (alert / sheet — owned by `apple-modality-overlays`) | security-relevant; data loss; severe consequence |

## Hard invariants

1. **Feedback is the system consequence layer.** It does NOT redefine control state, motion, gesture, modality, navigation, structural, material, or component behavior. Defer to the relevant Skill for those.
2. **System primitive first.** Apple platforms provide built-in feedback for standard controls (toggle, slider, picker, date picker). Custom feedback is for additional semantic value only.
3. **Multimodal, not single-channel.** Critical information must survive channel removal. Don't rely on color alone (WCAG 1.4.1), on sound alone, or on haptics alone.
4. **Significance → interruption.** Interruption increases only when meaning justifies it. Routine successes need no celebration. Critical irreversible actions may justify alert-level interruption (owned by `apple-modality-overlays`).
5. **No feedback intensity numerology.** No invented canonical numeric scale (intensity / sharpness / duration / delay). Any prototype value is implementation heuristic, NOT Apple canonical.
6. **No fake progress.** No misleading precision (e.g. `0 → 90% instantly`). If progress becomes known, adapt. If stalled, surface failure / recovery rather than frozen spinner.
7. **Haptics are complementary.** Critical meaning must not depend on haptics alone. Haptics can reinforce visual / text / audio confirmation but must not be the only accessible channel.
8. **Haptic semantic mapping follows Apple documentation.** Use system-defined patterns per their documented meanings (e.g. `UINotificationFeedbackGenerator` `.error` for failure). Reject arbitrary remapping (error pattern for success because it "feels stronger").
9. **Undo is system capability.** Not "every delete must show an Undo toast" canonically. Undo appears through system menus, shake gestures, SwiftUI `@Environment(\.undoManager)`, macOS menu-bar `NSUndoManager`. The Skill owns recovery **semantics**, not a fixed presentation.
10. **Web haptics is asymmetric.** iOS Safari does NOT reliably support `navigator.vibrate`; Android Chrome does. Visual / text / audio fallback is mandatory on Web.
11. **Per-pattern playback ≠ symbol existence.** A SwiftUI `SensoryFeedback` value being available does NOT mean it plays on every Apple platform. Per-pattern playback requires per-pattern DocC verification (Layer A #21).
12. **Feedback survives structural adaptation.** Progress / status must persist across collapses / reflows. Coordinate with `apple-adaptive-structure`.
13. **Fabricated Apple APIs are forbidden.** Every Feedback symbol passes the scanner's `FEEDBACK_RESPONSE` allowlist. No `CHHapticPatternComposer`, `UIWindowSceneFeedback`, `NSHapticEngine`, etc.
14. **Three evidence layers explicit.** Layer A (Apple official) / Layer B (Skill synthesis) / Layer C (Skill implementation taxonomy) — never collapsed.
15. **Paraphrase by default.** Apple HIG portal pages return JS-rendered SPA shells; literal HIG text is NOT extractable in this session. Verbatim HIG text is UNVERIFIED. Paraphrase + derivation.

## Routing integration

The orchestrator (`apple-experience-design`) adds a row for the new Skill. **Selective load rule:** the row applies ONLY when the task explicitly requires feedback (status, progress, success / warning / error, recovery, haptics). For ordinary state / motion / modal / navigation / structural / material / component tasks, the orchestrator routes WITHOUT loading `apple-feedback-response`.

Discriminator checklist (the orchestrator uses these to gate loading):

- Is the task about how the system reports what happened / is happening? If yes, load.
- Is the task purely about a single control's state / appearance? If yes, do NOT load — use `apple-control-states`.
- Is the task purely about modal type decision (sheet vs alert)? If yes, do NOT load — use `apple-modality-overlays`. The new Skill may signal that interruption is needed, but `apple-modality-overlays` decides the modal.
- Is the task about transition physics? If yes, do NOT load — use `apple-motion-physics`.
- Is the task about gesture / drag mechanics? If yes, do NOT load — use `apple-direct-manipulation`.
- Is the task about navigation history? If yes, do NOT load — use `apple-navigation-spatial`.
- Is the task about structural reflow? If yes, do NOT load — use `apple-adaptive-structure`. The new Skill coordinates: feedback persists across reflow.
- Is the task about Liquid Glass material? If yes, do NOT load unless feedback semantics are also required.
- Is the task about basic component appearance? If yes, do NOT load — use stable v1.1 `apple-ui-components`.

## Feedback channel model (Skill synthesis)

| Meaning | Visual | Text | Motion | Haptic | Audio | Interruptive? |
|---|---|---|---|---|---|---|
| Acknowledge | optional | optional | optional | optional | optional | no |
| Status | preferred | preferred | optional | optional | optional | no |
| Progress | preferred | preferred | optional | optional | optional | no |
| Outcome (success) | preferred | preferred | optional | possible | possible | no |
| Outcome (failure) | required | required | optional | possible | possible | only if recoverable & requires decision |
| Warn | preferred | required | optional | possible | possible | only if irreversible & requires decision |
| Recover | required | required | optional | possible | optional | only if recovery requires decision |
| Guide | preferred | preferred | optional | — | — | no |

This matrix is **Skill synthesis**, NOT verbatim HIG. Each cell is `preferred` / `possible` / `optional` / `required` / `no`. The matrix operationalizes the significance ladder.

## Platform capability map (Layer A summary)

- **iOS / iPadOS**: `SensoryFeedback` (iOS 17+); `UIFeedbackGenerator` family (iOS 10+); `UICanvasFeedbackGenerator` (iOS 17.5+); `UndoManager` (Foundation, cross-platform).
- **macOS**: `NSHapticFeedbackManager` (macOS 10.10+); `NSHapticFeedbackPerformer` (macOS 10.10+); `NSHapticFeedbackPattern` (Generic / Alignment / LevelChange); `NSUndoManager` (macOS 10.0+); `NSProgressIndicator` (macOS 10.0+).
- **watchOS**: `WKInterfaceDevice.current().play(_:)` (watchOS 2.0+).
- **visionOS**: no first-class public haptic API like `UIFeedbackGenerator`; visionOS has its own spatial feedback system. `UICanvasFeedbackGenerator` is `API_UNAVAILABLE(visionos)` per the installed UIKit header.
- **Web**: `aria-live` for status / error; `<progress>` element + ARIA `progressbar` role; `prefers-reduced-motion` / `prefers-reduced-transparency` / `prefers-contrast`. `navigator.vibrate` is asymmetric (Android Chrome yes, iOS Safari unreliable).

Per-pattern playback availability requires per-pattern DocC verification. The Skill records API existence + base availability, NOT per-pattern parity.

## Reference catalog (Tier 2)

- `references/feedback-taxonomy.md` — feedback intent taxonomy + significance ladder detail.
- `references/progress-and-status.md` — determinate / indeterminate progress; stall handling; truthful representation.
- `references/success-warning-error.md` — semantic distinctions + recovery-oriented error messaging.
- `references/undo-recovery.md` — undo semantics; presentation; redo; action naming.
- `references/haptic-feedback.md` — haptic semantic mapping; system vs custom; platform availability.
- `references/multimodal-feedback.md` — channel coordination; accessibility.
- `references/feedback-accessibility.md` — accessibility constraints on feedback channels.
- `references/platform-feedback-capabilities.md` — per-platform capability matrix (iOS / iPadOS / macOS / watchOS / visionOS / Web).
- `references/feedback-anti-patterns.md` — INTERNAL anti-pattern candidates.

## Process notes

- The Skill does NOT introduce a new Apple API symbol at the **fabricated** level. Every symbol is verified in DocC + installed SDK per the inventory.
- The Skill does NOT make verbatim Apple HIG claims (HIG portal pages return JS-rendered shells in this session).
- The Skill does NOT publish canonical numeric feedback intensity scales.
- The Skill does NOT assume Web parity with native Apple haptics.
- The Skill is verified by 20 functional tests (AE5-T1..T20) + 15 adversarial tests (AE5-G1..G15) + 10 router tests (AE5-R1..R10).
- The Skill consumes but does not redefine the v0.4 Skills (`apple-control-states`, `apple-motion-physics`, etc.).