# Success / Warning / Error

Semantic distinctions between success, warning, and error. The Skill uses these to drive delivery: routine success needs no celebration; warning demands attention; error demands recovery where possible.

## Definitions (Skill-owned, not Apple verbatim)

| Term | Meaning | Typical significance |
|---|---|---|
| **Success** | the action completed as expected | Low / Medium (unless delayed, financial, security-relevant) |
| **Warning** | a meaningful negative consequence MAY occur or requires attention | Medium / High |
| **Error** | the operation FAILED or an invalid state occurred | Medium / High (interruptive only if severe) |

These are **Skill synthesis** derived from observable Apple patterns. They are NOT verbatim Apple HIG enumerations.

## Success feedback

Routine success does not need a celebration. Examples of routine success that need NO explicit feedback:

- Tapping a button that toggles state.
- Selecting a list item.
- Saving a setting.

Examples of success that DO benefit from explicit feedback:

- Long-running operation completes (delayed success → user benefits from completion indicator).
- Significant / irreversible action that the user might want to confirm (purchase submitted; message sent).
- Financial / security action (transfer complete; permission granted).
- Ambiguous operation (did the upload finish? did the form submit?).
- User benefits from confidence (task progress milestone).

The Skill rejects "Success Confetti Syndrome" — celebrating every routine success with prominent visual / haptic / audio. The Skill DOES recommend success feedback when meaning justifies it.

## Warning feedback

A warning communicates that a meaningful negative consequence MAY occur or requires attention. The warning happens BEFORE the consequence so the user can decide.

Examples:

- Unsaved changes — warn before discarding.
- Destructive action preview — warn before deleting.
- Expensive operation preview — warn before submitting.
- Network state — warn before the operation if the network is unreliable.

A warning is not an error. The action has not yet failed; the warning is a **preview of consequence** so the user can choose differently. The warning is usually focused / prominent; interruptive only if the consequence is irreversible.

## Error feedback

An error communicates that the operation FAILED or an invalid state occurred. The error happens AFTER the consequence.

An error must support recovery where possible:

1. **What happened?** — clear, specific language.
2. **What is affected?** — which file / row / field / state.
3. **Can the system recover?** — auto-retry, fallback, etc.
4. **What can the person do next?** — retry, contact support, undo, etc.

The Skill rejects error feedback that is merely "red color + generic message". The Skill rejects silent failures.

## Semantic mapping (Skill synthesis)

| Outcome | Visual | Text | Motion | Haptic | Audio | Interruptive? |
|---|---|---|---|---|---|---|
| Success (routine) | optional | optional | optional | optional | optional | no |
| Success (significant) | preferred | preferred | optional | possible | possible | no |
| Warning | required | required | optional | possible | possible | only if irreversible |
| Error (recoverable) | required | required | optional | possible | possible | only if requires decision |
| Error (critical, requires decision) | required | required | optional | possible | possible | yes (alert — owned by `apple-modality-overlays`) |

## Error message construction

A good error message:

- **States what happened** in plain language (not technical jargon).
- **States what is affected** if known.
- **States what can be done** to recover.
- **Suggests a next action** when possible.
- **Does NOT blame the user** unless the action was clearly theirs.
- **Does NOT contain** "Something went wrong" / "Error" / "Unknown error" without further context.

Examples:

- **Bad**: "Something went wrong."
- **Better**: "Couldn't save your changes. Your work is still in the editor — try Save again."
- **Better still**: "Couldn't save to iCloud. Your work is saved locally — choose another location to continue editing."

The Skill does NOT invent specific error message strings; the Skill publishes the construction rules.

## Apple platform surfaces

| Surface | Where to render | Reference |
|---|---|---|
| Inline error | form field; row; surface | Inline. |
| Toast / banner | top of screen, time-limited | Web / iOS / macOS. |
| Alert | modal | Owned by `apple-modality-overlays`. |
| System notification | OS-level | iOS NotificationCenter; macOS Notification Center. |

## What the Skill rejects

- **Red Means Error** — relying on color alone. Rejected by WCAG 1.4.1.
- **Generic "Something went wrong"** — fails the construction rules above.
- **Alert Everything** — interrupting on every success / warning. The interruption model demands meaning justify interruption.
- **Error feedback that doesn't support recovery** — fails the recovery rules above.
- **Success Confetti Syndrome** — celebrating every routine save.
- **Error without next step** — leaves the user stuck.
- **Silent failure** — error swallowed.