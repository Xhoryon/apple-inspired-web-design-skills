# Text Editing Accessibility

Accessibility constraints on text editing. Critical text editing must be operable via keyboard, screen reader, and assistive technology.

## WCAG 2.2 Success Criteria (verified)

| SC | Title | Application |
|---|---|---|
| 1.3.5 | Identify Input Purpose | input elements have `autocomplete` attributes for known purposes |
| 2.1.1 | Keyboard | all text editing operable via keyboard |
| 3.3.1 | Error Identification | invalid input has programmatically determinable error state |
| 3.3.2 | Labels or Instructions | input elements have visible labels |
| 3.3.3 | Error Suggestion | error messages include a recovery suggestion |
| 3.3.4 | Error Prevention | reversible / confirmable actions for legal / financial / data-loss contexts |

## WAI-ARIA 1.2 (verified)

- `role="textbox"` for editable text fields (when not using native input).
- `aria-invalid` for invalid input state.
- `aria-describedby` for linking error messages.
- `aria-required` for required field.
- `aria-label` / `aria-labelledby` for accessible name.
- `aria-multiline` for multi-line text fields (when using `contenteditable`).
- `aria-autocomplete` for autocomplete model.

## Apple accessibility (verified)

- `isAccessibilityElement = true` for custom editor surfaces.
- `accessibilityLabel`, `accessibilityValue`, `accessibilityHint` (UIKit).
- VoiceOver: navigates the accessibility tree; reads label + value + hint; supports text editing rotor.
- Increase Contrast: editor chrome increases contrast.
- Reduce Motion: caret blink may be reduced.

## Selection / caret accessibility

- `UIAccessibility` exposes `accessibilitySelectedTextRange` and `accessibilityValue` for selected text.
- VoiceOver may announce selection changes.
- For custom `contenteditable` on Web: use ARIA roles + `aria-multiline`; ensure screen reader navigates the editable region.

## IME independence

- Screen readers must operate independently of IME composition state.
- Composition text should be announced as provisional when relevant.
- VoiceOver on iOS: integrates with system IME; custom editors must not break this integration.

## Hard principle

> **Do NOT assume VoiceOver or other assistive technology automatically repairs inaccessible app semantics.**

The app must be accessible by default. Custom editors carry accessibility responsibility.

## Standard controls preferred

- Use `UITextField` / `UITextView` / `NSTextField` / `NSTextView` / SwiftUI `TextField` / `TextEditor` to preserve compatibility with system accessibility.
- Custom editors: implement accessibility explicitly.

## Reference

- `references/text-input-lifecycle.md` — input lifecycle.
- `references/uikit-text-input.md` — UIKit accessibility.
- `references/appkit-text-system.md` — AppKit accessibility.
- `references/web-ime-editing.md` — Web ARIA.