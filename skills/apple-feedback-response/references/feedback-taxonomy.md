# Feedback Taxonomy

The Skill-owned feedback intent taxonomy and significance ladder. This is **Apple Experience Skill implementation vocabulary**, NOT extracted from Apple HIG as a literal primitive set.

## Feedback intent taxonomy (Skill-owned)

Seven intents. Each is an answer to the question "what is the user being told?"

- **Acknowledge** — the system registered an action. The action occurred; the system is responding. Lightweight. Often passive (in-context). Examples: tap registered; toggle flipped; option selected.
- **Status** — what is currently happening. Lightweight. Often passive. Examples: loading; "Recording…" indicator; connection state; export in progress.
- **Progress** — how an ongoing task advances. Often passive or focused. Examples: determinate percentage; indeterminate spinner; multi-step progress.
- **Outcome** — success / completion / failure. May be passive, focused, or interruptive (depending on significance). Examples: save succeeded; upload complete; network failure.
- **Warn** — a meaningful negative consequence may occur. Usually focused; interruptive only if irreversible. Examples: unsaved changes; destructive-action preview.
- **Recover** — how to correct or reverse a problem. Usually focused; interruptive only if recovery requires immediate decision. Examples: undo available; retry button; error explanation with next step.
- **Guide** — what can happen next. Lightweight. Often passive. Examples: suggested next action; available command hint; contextual menu preview.

These categories are derived from observed Apple feedback patterns (status indicators, progress UI, notification feedback generators, undo affordances) but are NOT verbatim Apple HIG enumerations. They are the Skill's own vocabulary for reasoning about feedback.

## Significance ladder (Skill-owned synthesis)

| Significance | Delivery | Visual | Text | Motion | Haptic | Audio | Interruptive? |
|---|---|---|---|---|---|---|---|
| **Low** | passive / in-context | preferred | optional | optional | optional | optional | no |
| **Medium** | focused / non-modal | preferred | preferred | optional | possible | possible | no |
| **High** | focused / prominent | required | required | optional | possible | possible | only if requires decision |
| **Critical** | interruptive (alert / sheet — owned by `apple-modality-overlays`) | required | required | optional | possible | possible | yes |

The skill's **significance → interruption model** is derived from observing Apple's pattern: native apps present destructive-action confirmations and recovery flows but not routine state changes. The model escalates interruption only when meaning justifies it.

## Feedback intent × significance matrix (Skill-owned)

| Intent | Typical significance | Typical delivery | Examples |
|---|---|---|---|
| Acknowledge | Low | passive / in-context | tap feedback (system button press); toggle flip |
| Status | Low / Medium | passive / focused | loading indicator; recording state |
| Progress | Low / Medium | passive / focused | determinate progress bar; indeterminate spinner |
| Outcome (success) | Low / Medium | passive / focused | save succeeded; download complete (delayed) |
| Outcome (failure) | Medium / High | focused / prominent | upload failed; validation error |
| Warn | Medium / High | focused / prominent | unsaved changes warning; destructive-action preview |
| Recover | Medium / High | focused / prominent | undo available; retry button |
| Guide | Low / Medium | passive / focused | suggested next step; available command |

## Decision tree

```
1. Did something happen that the user must understand?
   NO → no feedback
   YES → continue

2. Is the action expected to succeed routinely?
   YES → Acknowledge / Status only; no Outcome celebration
   NO → continue

3. Is the action's success or failure ambiguous / delayed / security-relevant?
   YES → Outcome (Success / Failure as appropriate); routine success need not celebrate
   NO → Acknowledge / Status

4. Does the action's potential failure have meaningful negative consequences?
   YES → Warn (before) + Outcome (Failure) + Recover (after)
   NO → Status + Outcome (Success)

5. Is the consequence IRREVERSIBLE and CRITICAL?
   YES → escalate to interruptive (alert — owned by `apple-modality-overlays`)
   NO → focused / prominent in-context feedback

6. Does the user need to know what can happen next?
   YES → Guide
   NO → end
```

## What the Skill rejects

- **Haptic Everywhere** — playing a haptic on every action. Overuse degrades meaning.
- **Alert Everything** — interrupting on every success.
- **Confirm Every Delete** — requiring confirmation for routine recoverable actions.
- **Success Confetti Syndrome** — celebrating every routine success.
- **Red Means Error** — relying on color alone for error signaling (rejected by WCAG 1.4.1).
- **Frozen Spinner** — leaving an indeterminate spinner when the operation has failed.
- **Fake Progress** — misleading precision (e.g. jumping to 90% to "feel faster").
- **Haptic-Only Meaning** — relying on haptics alone for critical information.
- **Sound-Only Meaning** — relying on audio alone.
- **Semantic Haptic Swap** — using error pattern for success because it "feels stronger".
- **Invisible Undo** — undoing offscreen content without showing what changed.
- **Platform Feedback Parity** — assuming iPhone-style haptics everywhere.
- **Custom Haptics by Default** — using Core Haptics merely for "premium feel".
- **Feedback Without Cause** — playing feedback that doesn't correspond to a user action or system event.