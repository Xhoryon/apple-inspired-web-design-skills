# Text Input Lifecycle

The Skill-owned text input lifecycle model. This is **Apple Experience Skill implementation vocabulary**, NOT extracted from Apple HIG as a literal primitive set.

## The lifecycle (Skill-owned Layer C)

```
Input Intent
      ↓
Composition / Direct Input
      ↓
Provisional Text
      ↓
Commit
      ↓
Selection / Caret Update
      ↓
Edit Operation
      ↓
Model Update
```

Each layer is an answer to a separate question:

- **Input Intent** — what the user wants to do (insert character; select; cut; copy; paste; etc.).
- **Composition / Direct Input** — IME composition (for non-Latin input or multistage systems) OR direct key-by-key input (for simple Latin input).
- **Provisional Text** — composition result; not yet committed; may still change or be cancelled.
- **Commit** — composition accepted; provisional text becomes the editable value.
- **Selection / Caret Update** — caret position or selected range after commit / replace.
- **Edit Operation** — what changed (insertion; deletion; replacement).
- **Model Update** — the editable element's value is updated; selection / caret may need to be re-applied.

## Per-platform lifecycle

| Platform | Composition events | Commit event | Model update |
|---|---|---|---|
| UIKit | `setMarkedText(_:selectedRange:)` repeated | `unmarkText()` | `text` / `attributedText` |
| AppKit | `setMarkedText:selectedRange:` repeated | `unmarkText` | `string` / `attributedString` / `textStorage` |
| SwiftUI | delegates to platform | delegates | `@State` / `@Binding` |
| Web | `compositionupdate` events | `compositionend` event | DOM `value` / `textContent` |

## What the Skill owns

- The rule that composition is first-class.
- The rule that provisional text is not equivalent to committed text.
- The rule that validation should respect composition.
- The rule that app shortcuts do not steal composition keys.
- The rule that selection continuity is preserved where possible.

## What the Skill does NOT own

- The exact platform text system internals.
- The text input protocol details (see `references/uikit-text-input.md` / `references/appkit-text-system.md` / `references/web-ime-editing.md`).
- The animation of caret blinking (`apple-motion-physics`).
- The visual state of the editor control (`apple-control-states`).
- The validation outcome / error communication (`apple-feedback-response`).

## Reference

- `references/ime-composition.md` — IME lifecycle detail; CJK / multistage; keyboard shortcut during composition.
- `references/text-selection.md` — caret / selection distinction.
- `references/selection-continuity.md` — selection survives benign updates.
- `references/uikit-text-input.md` — UIKit text input protocol.
- `references/appkit-text-system.md` — AppKit text system.
- `references/web-ime-editing.md` — Web composition / IME.
- `references/swiftui-text-editing.md` — SwiftUI text editing.