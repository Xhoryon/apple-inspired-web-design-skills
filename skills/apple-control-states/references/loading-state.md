# Loading State Reference

A loading state has variants. The Skill does not add a spinner to every button on click. This file is the Tier 2 reference for `apple-control-states`.

## Variants (canonical)

- **Optimistic action** — UI updates immediately. Loading state is invisible to the user.
- **Short asynchronous action** (< 1 s) — show a subtle progress indicator or disable the control. Do NOT add a spinner for sub-second actions.
- **Long-running action** — show a progress indicator. Disable the control. Optionally show a cancel affordance.
- **Duplicate-submit prevention** — after the first activation, ignore further activations until the action completes.

## Optimistic action

The UI updates immediately, before the server confirms. The user sees the result instantly. The server may later reject the action; if so, the UI reverts to the previous state and shows an error.

This is the right pattern for actions that are usually fast and rarely fail. Examples: toggling a setting, marking a notification as read, liking a post.

## Short asynchronous action

The action takes less than 1 s. The user perceives the action as immediate. The control may briefly show a progress indicator (e.g. a subtle opacity shift) or disable itself for the duration.

A spinner for a sub-second action is wrong. It adds visual noise for an action the user barely perceives as taking time.

## Long-running action

The action takes more than 1 s. The user is waiting. The control should show a progress indicator (spinner, progress bar, or estimated time). The control should be disabled during the action. Optionally, the user may cancel the action.

A progress indicator must be perceivable but not overwhelming. A small spinner in the corner of the control, or a progress bar below the control, is appropriate.

## Duplicate-submit prevention

After the first activation, ignore further activations until the action completes. This is critical for actions that submit data to a server (form submission, payment, etc.).

A common pattern:

```javascript
let isSubmitting = false;
button.addEventListener('click', (e) => {
  if (isSubmitting) return;
  isSubmitting = true;
  submit();
  // Reset isSubmitting when the action completes
});
```

## Cancel affordance

A long-running action may offer a cancel affordance. The cancel button is a separate control, not the same button. The user clicks Cancel to abort the action.

The cancel button has its own state. It is not in a "loading" state. It is an interactive control that, when activated, cancels the in-flight action.

## Visual treatment (heuristic)

Common patterns:

- Optimistic: no special treatment. UI updates immediately.
- Short action: subtle opacity shift on the control (e.g. 0.6–0.8). No spinner.
- Long-running: spinner inside the control, or a progress bar below the control. Disable the control.
- Duplicate-submit prevention: ignore further activations.

The exact value is a HEURISTIC. The Skill must NOT publish fake Apple canonical numbers.

## Loading vs disabled

A loading control is NOT disabled. The control shows a loading state and remains active (so the user can cancel). A control that is loading and also disabled is broken.

## Loading vs pressed

A loading control may have been pressed once (the activation that started the loading). After that first activation, the control is in `loading` state, not `pressed`. The press state is consumed by the activation.

A Skill that keeps the control in `pressed` state during loading is wrong. The press state is transient; the loading state is the next state.

## Cross-Skill note

The loading state is part of the state catalog (`apple-control-states`). The motion of the loading transition is `apple-motion-physics`. The platform-applicability verdict for loading on each platform is in `APPLE-EXPERIENCE-PLATFORM-MATRIX.md` (DIRECT on all current platforms).
