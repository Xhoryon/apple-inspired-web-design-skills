# SwiftUI Text Editing

SwiftUI text editing APIs (verified). SwiftUI delegates substantial editing behavior to the underlying platform text system; the Skill does NOT manufacture lower-level APIs not in the public SwiftUI surface.

## Verified SwiftUI APIs

| API | Availability | Purpose |
|---|---|---|
| `TextField` | iOS 14+ / iPadOS 14+ / Mac Catalyst 14.0+ / macOS 11.0+ / tvOS 14.0+ / visionOS 1.0+ / watchOS 7.0+ | single-line editable text |
| `TextEditor` | same as `TextField` | multi-line editable text |
| `.disableAutocorrection(_:)` | same as `TextField` | disable autocorrection |
| `.keyboardType(_:)` | same as `TextField` | configure keyboard type |
| `.textInputAutocapitalization(_:)` | same as `TextField` | configure autocapitalization |
| `.submitLabel(_:)` | same as `TextField` | configure submit button label |
| `.onSubmit(_:)` | same as `TextField` | submit handler |
| `.focused(_:)` | same as `TextField` | focus binding |
| `@FocusState` | (Batch 6) | focus state |

## What SwiftUI delegates to the platform text system

- selection management (caret / range);
- IME / composition handling;
- standard edit actions (Cut / Copy / Paste / Select / Select All / Delete);
- clipboard / pasteboard;
- spell / autocorrect / Writing Tools (when platform provides);
- accessibility (when using standard controls).

The Skill does NOT claim that SwiftUI exposes lower-level marked-text APIs (e.g. SwiftUI does NOT publicly expose `markedTextRange` under a similarly named property). SwiftUI delegates to the platform.

## SwiftUI text selection

Selection APIs in SwiftUI have evolved across OS versions. The Skill does NOT claim a single canonical SwiftUI selection API. The Skill documents what SwiftUI publicly exposes in current SDK:

- `TextField` and `TextEditor` provide native selection through the underlying platform text system.
- For SwiftUI list / table selection, use the existing selection binding model (separate concern from text selection).

## SwiftUI text editor vs custom

- Use SwiftUI `TextField` / `TextEditor` for standard text input.
- Custom editor: requires substantive reason (specialized document model, advanced layout, etc.).
- Custom editor in SwiftUI: typically wraps a `UIViewRepresentable` (UIKit) or `NSViewRepresentable` (AppKit) for the rich text control; not a custom `UITextInput` implementation in pure SwiftUI.

## Reference

- `references/text-input-lifecycle.md` — input lifecycle.
- `references/ime-composition.md` — IME composition.
- `references/plain-rich-text.md` — plain vs rich.
- `references/uikit-text-input.md` — UIKit for custom editor wrapping.
- `references/appkit-text-system.md` — AppKit for custom editor wrapping.