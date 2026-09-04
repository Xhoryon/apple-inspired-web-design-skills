# Feedback Anti-Patterns

The Skill's INTERNAL anti-pattern candidates. Each has a written rejection. These are candidates; the Skill does NOT promote them to the public anti-pattern library without independent review.

## 1. Haptic Everywhere

**Pattern:** Play a haptic on every button tap, every selection change, every interaction.

**Why it's wrong:** Overuse degrades meaning. The user stops noticing. The haptic becomes background noise, not semantic reinforcement.

**Rejection:** The Skill rejects Haptic Everywhere. Use haptics to reinforce meaningful cause/effect, not to fill silence.

**Replaced by:** The causal-relationship rule. Haptic feedback should correspond to a meaningful user action or system event.

## 2. Haptic Numerology

**Pattern:** "Apple says success = intensity 0.7, warning = 0.8, error = 1.0." Or: "Mouse feedback = 0.3, touch feedback = 0.8."

**Why it's wrong:** Apple does NOT publish canonical numeric feedback intensity values. The Skill explicitly records: no invented canonical numeric scale.

**Rejection:** The Skill rejects Haptic Numerology. Any numeric value is implementation heuristic, not Apple canonical.

**Replaced by:** Use Apple's documented system patterns (`UINotificationFeedbackGenerator.success`, `.warning`, `.error`); let the system choose intensity. Custom Core Haptics is ADVANCED / CONDITIONAL.

## 3. Alert Everything

**Pattern:** Show an alert after every success, every warning, every state change.

**Why it's wrong:** Interruption is expensive. Routine state changes don't deserve the user's full attention.

**Rejection:** The Skill rejects Alert Everything. The significance ladder escalates interruption only when meaning justifies it.

**Replaced by:** The significance → interruption model. Routine success → in-context; critical failure → interruptive (alert — owned by `apple-modality-overlays`).

## 4. Confirm Every Delete

**Pattern:** Show a confirmation alert before every delete.

**Why it's wrong:** Confirmation without undo is friction. Confirmation for routine recoverable actions wastes the user's time.

**Rejection:** The Skill rejects "confirm every delete" as a canonical rule. The Skill publishes the recoverability × confirmation matrix.

**Replaced by:** The recoverability × confirmation table in `references/undo-recovery.md`. Routine recoverable actions need NO confirmation; irreversible critical actions may benefit from confirmation.

## 5. Fake Progress

**Pattern:** Jump to 90% instantly to make the operation "feel faster". Or: present fake time remaining like "5 seconds remaining" when duration is unknown.

**Why it's wrong:** Misleading precision degrades user trust. The user notices the lie and stops trusting the indicator.

**Rejection:** The Skill rejects Fake Progress. No invented progress percentages; no fake time remaining.

**Replaced by:** Determinate when measurable, indeterminate when not. Adapt as the operation becomes measurable. Surface stall as failure / recovery.

## 6. Frozen Spinner

**Pattern:** Leave an indeterminate spinner forever when the request has actually failed.

**Why it's wrong:** The user waits forever. The user can't distinguish "still running" from "failed silently".

**Rejection:** The Skill rejects Frozen Spinner. Surface failure / recovery when the operation stalls.

**Replaced by:** Stall detection + transition to error state owned by `apple-feedback-response`. Error semantics owned by `references/success-warning-error.md`.

## 7. Red Means Error

**Pattern:** Use red color alone to indicate an error. No icon, no text label.

**Why it's wrong:** WCAG 2.2 Success Criterion 1.4.1 (Use of Color) explicitly forbids color-only meaning. Color-blind users miss the signal.

**Rejection:** The Skill rejects Red Means Error. Errors use red + icon + text label.

**Replaced by:** The accessibility rule in `references/feedback-accessibility.md`. Color is supplementary, not the primary signal.

## 8. Haptic-Only Meaning

**Pattern:** Communicate critical information through haptics alone. The user has no visual / text / audio fallback.

**Why it's wrong:** Hardware may lack haptic capability. User may have haptic feedback disabled. User may be deaf to subtle vibrations.

**Rejection:** The Skill rejects Haptic-Only Meaning. Critical information must survive channel removal.

**Replaced by:** The multimodal fallback rule in `references/multimodal-feedback.md`. Visual + text + audio are the baseline; haptic is supplementary.

## 9. Sound-Only Meaning

**Pattern:** Play an error sound with no visual / text feedback.

**Why it's wrong:** User may be in a noisy environment (audio not heard). User may be deaf (audio not heard). User may have audio disabled.

**Rejection:** The Skill rejects Sound-Only Meaning.

**Replaced by:** Visual + text + haptic fallback.

## 10. Success Confetti Syndrome

**Pattern:** Celebrate every routine success with prominent visual / haptic / audio feedback. Every save → green checkmark + success haptic + success sound.

**Why it's wrong:** Overuse degrades meaning. The user becomes desensitized. Significant successes lose their impact.

**Rejection:** The Skill rejects Success Confetti Syndrome. Routine successes need no celebration.

**Replaced by:** The success feedback rule in `references/success-warning-error.md`. Routine success → Acknowledge / Status only. Significant success → Outcome (Success).

## 11. Semantic Haptic Swap

**Pattern:** Use `error` haptic for success because it "feels stronger". Use `selection` haptic as a generic click noise everywhere.

**Why it's wrong:** Arbitrary remapping breaks the documented Apple semantic. The user learns the documented mapping; remapping confuses them.

**Rejection:** The Skill rejects Semantic Haptic Swap. Use Apple-documented semantic mappings.

**Replaced by:** The haptic semantic mapping in `references/haptic-feedback.md`. Use system-defined patterns per their documented meanings.

## 12. Invisible Undo

**Pattern:** Undo an offscreen change without showing what changed. The user thinks nothing happened; they undo again.

**Why it's wrong:** The user can't verify the action was reversed. They may undo multiple times thinking nothing happened.

**Rejection:** The Skill rejects Invisible Undo. Undo result must be visible or understandable.

**Replaced by:** The undo visibility rule in `references/undo-recovery.md`. Show the result briefly if needed; don't let users repeatedly undo because they think nothing happened.

## 13. Platform Feedback Parity

**Pattern:** Assume iPhone-style haptics work on macOS / visionOS / tvOS / watchOS. Or: assume Web feedback approximates native Apple feedback.

**Why it's wrong:** Haptic capabilities are platform-specific. UIKit `UIFeedbackGenerator` is iOS-family only. AppKit uses `NSHapticFeedbackManager` (different API). visionOS has its own spatial feedback system. Web `navigator.vibrate` is asymmetric.

**Rejection:** The Skill rejects Platform Feedback Parity. The Skill uses platform-appropriate APIs.

**Replaced by:** The platform feedback capability matrix in `references/platform-feedback-capabilities.md`.

## 14. Custom Haptics by Default

**Pattern:** Use Core Haptics (`CHHapticEngine`) everywhere because it "feels more premium".

**Why it's wrong:** Custom haptics are ADVANCED / CONDITIONAL. Most feedback is served by system patterns. Custom haptics add complexity (engine lifecycle, pattern design, capability checking).

**Rejection:** The Skill rejects Custom Haptics by Default. System patterns first; Core Haptics only when system patterns are insufficient AND the use has semantic / product value.

**Replaced by:** The system-primitive-first rule. Use `UIFeedbackGenerator` family / `SensoryFeedback` / `NSHapticFeedbackManager` first.

## 15. Feedback Without Cause

**Pattern:** Play feedback that doesn't correspond to a user action or system event.

**Why it's wrong:** The user can't connect the feedback to a cause. It becomes noise.

**Rejection:** The Skill rejects Feedback Without Cause. Every feedback has a cause (user action, system event, or state change).

**Replaced by:** The causal-relationship rule. Haptic / audio / visual feedback correspond to a meaningful cause.

## 16. No Indication of Failure Recovery

**Pattern:** Error message says "Something went wrong" with no next step.

**Why it's wrong:** The user can't recover. They retry the same action or contact support with no context.

**Rejection:** The Skill rejects No Indication of Failure Recovery. Errors support recovery where possible.

**Replaced by:** The error message construction in `references/success-warning-error.md` — what happened, what is affected, can the system recover, what can the person do next.

## How to add a new anti-pattern

1. Write the pattern, why it's wrong, and the replacement.
2. Add the pattern to this file.
3. Add a check to `references/feedback-audit.md` (if added).
4. Add a test (AE5-G*) to the Batch 5 adversarial test set.
5. The new anti-pattern is a CANDIDATE. It is INTERNAL until independent review.