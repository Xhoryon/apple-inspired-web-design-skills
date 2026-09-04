# Feedback Accessibility

The Skill's accessibility constraints on feedback channels. Critical information must not depend on one channel. The Skill honors Apple's accessibility settings and W3C / WCAG / WAI-ARIA guidance.

## Hard invariants

1. **Critical information must not depend on color alone.** WCAG 2.2 Success Criterion 1.4.1.
2. **Critical information must not depend on sound alone.**
3. **Critical information must not depend on haptics alone.**
4. **Honored accessibility settings**: `prefers-reduced-motion`, `prefers-reduced-transparency`, `prefers-contrast`, system Reduce Motion / Increase Contrast / Reduce Transparency.
5. **Readable status / error text**: text labels accompany visual / haptic indicators.
6. **Persistent enough recovery information**: the user can read the recovery path after the indicator is gone.

## WCAG / WAI-ARIA / Apple guidance

| Standard | | Application to feedback |
|---|---|---|
| WCAG 2.2 SC 1.4.1 (Use of Color) | color is not the only means of conveying information | error indicators: red + icon + text label, not red alone |
| WCAG 2.2 SC 1.4.11 (Non-text Contrast) | 3:1 contrast for UI components | status / error text contrast |
| WCAG 2.2 SC 1.4.13 (Content on Hover or Focus) | hoverable / focusable content is dismissible / hoverable / persistent | tooltips, error popovers |
| WCAG 2.2 SC 2.1.1 (Keyboard) | all functionality via keyboard | feedback navigable via keyboard |
| WCAG 2.2 SC 4.1.3 (Status Messages) | status messages programmatically determinable | `role="status"` for polite updates; `role="alert"` for assertive |
| WAI-ARIA 1.2 `aria-live` | live region politeness | polite for status; assertive for errors |
| WAI-ARIA 1.2 `progressbar` | progress UI role + value | accessible progress |
| Apple `prefers-reduced-motion` | user-set reduce motion preference | reduce or remove animations |
| Apple `prefers-reduced-transparency` | user-set reduce transparency preference | use more opaque surfaces |
| Apple `prefers-contrast` | user-set contrast preference | strengthen contrast |
| Apple Reduce Motion (system setting) | iOS / iPadOS / macOS / tvOS / watchOS / visionOS | reduce or remove animations |
| Apple Increase Contrast (system setting) | same | strengthen contrast |
| Apple Reduce Transparency (system setting) | same | use more opaque surfaces |

## Reduced motion

When the user has `prefers-reduced-motion: reduce` or the system Reduce Motion is enabled:

- Reduce or remove non-essential animations.
- Keep state-feedback transitions (e.g. color change on press) intact.
- Replace bouncy springs with simpler transitions.
- Haptic feedback may continue; the system may reduce haptic intensity independently.

The Skill publishes: **reduce amplitude, duration, and translation; keep small state-feedback transitions intact.**

## Reduced transparency

When the user has `prefers-reduced-transparency: reduce` or the system Reduce Transparency is enabled:

- Use more opaque surfaces.
- Reduce blur / translucency.
- Visual identity may be preserved (still a glass-like surface) but opacity increases.

## Increased contrast

When the user has `prefers-contrast: more` or the system Increase Contrast is enabled:

- Strengthen contrast on text.
- Strengthen borders / outlines.
- Use stronger visual indicators for status / error.

## Status messages (Web accessibility)

The Skill uses `role="status"` for non-urgent updates and `role="alert"` for urgent:

```html
<div role="status" aria-live="polite">3 of 5 files uploaded.</div>
<div role="alert">Could not save changes. Your work is saved locally.</div>
```

The Skill uses `<progress>` element with `aria-valuenow`, `aria-valuemin`, `aria-valuemax`:

```html
<progress value="3" max="5" aria-label="Upload progress">3 of 5</progress>
```

## VoiceOver / assistive narration

When the user has VoiceOver (iOS / iPadOS / macOS) or another screen reader active:

- All feedback must be navigable.
- Text labels accompany visual / haptic / audio feedback.
- Status updates are announced (via `role="status"` / `aria-live="polite"`).
- Errors are announced (via `role="alert"` / `aria-live="assertive"`).

The Skill publishes: feedback should be reachable via assistive technology, not hidden behind visual / haptic / audio alone.

## User preferences for feedback channels

The Skill publishes: the user may want to disable specific channels. Examples:

- User wants haptics disabled (system Reduce Motion may reduce but not fully disable; system Accessibility settings can fully disable).
- User wants audio feedback disabled (system settings allow this).
- User wants reduced animation (Reduce Motion).

The Skill does NOT mandate how the user toggles these. The Skill recommends exposing the toggle where appropriate (e.g. a "Haptic feedback" toggle in app settings if the app allows disabling system-level settings).

## Channel survival

The Skill publishes: when a channel is unavailable, the meaning must survive via other channels.

| Channel unavailable | Fallback |
|---|---|
| Haptic | visual + text + audio |
| Audio | visual + text + haptic |
| Color (e.g. color-blindness) | shape + icon + text |
| Animation | static state + text |
| VoiceOver / screen reader | visual + haptic + text-readable labels |
| Tactile capability | visual + text + audio |
| Visual capability (low vision) | text + haptic + audio (VoiceOver) |

## What the Skill rejects

- **Color-only meaning** — WCAG 1.4.1 violation. Rejected.
- **Haptic-only meaning** — accessibility violation. Rejected.
- **Sound-only meaning** — accessibility violation. Rejected.
- **Animation-only meaning** — reduced motion breaks it. Rejected.
- **No keyboard navigation** — feedback unreachable via keyboard. Rejected.
- **Status text that disappears too quickly** — user can't read it. Rejected.
- **Error message that requires gesture to dismiss** — must be reachable. Rejected.
- **Critical information behind visual-only** — accessibility violation. Rejected.