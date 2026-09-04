# IME & Composition

IME (Input Method Editor) composition semantics. The Skill's rule: **composition is first-class; provisional text is not equivalent to committed text.**

## Why this matters

For non-Latin / multistage input systems (Chinese Pinyin, Japanese Kana-Kanji, Korean, dead-key composition, handwriting, alternate input), the user types phonetic input, the IME presents candidate characters, the user selects a candidate, and only then is text committed. During this multistage process, the editable value may repeatedly change. Apps that treat every keystroke as a final semantic commit risk:

- destroying the composition session;
- resetting the caret mid-composition;
- validating / reformatting the field while the user is still composing;
- prematurely triggering edit operations based on incomplete input.

## Per-platform IME lifecycle

| Platform | Lifecycle events | Commit |
|---|---|---|
| UIKit | `setMarkedText(_:selectedRange:)` repeated | `unmarkText()` |
| AppKit | `setMarkedText:selectedRange:` repeated | `unmarkText` |
| SwiftUI | delegates to platform | delegates |
| Web | `compositionstart` → `compositionupdate` (multiple) → `compositionend` | `compositionend` event |

## Per-platform IME safety helpers

| Platform | Helper |
|---|---|
| Web | `KeyboardEvent.isComposing` (true during composition) |
| UIKit | `markedTextRange != nil` (composition in progress) |
| AppKit | `hasMarkedText` (NSTextInputClient protocol) |

## Skill rules (Layer B synthesis)

1. **Do not rewrite the editable element's value out-of-band during composition.** The composition is in progress; rewriting destroys the session.
2. **Do not move the caret to end during composition.** The composition has its own caret semantics; resetting destroys the session.
3. **Do not trigger destructive validation that overrides the composition.** Validation timing: on committed edit; on field submit; after composition ends; non-destructive live validation.
4. **Do not programmatically commit or cancel the composition.** The platform / IME handles it.
5. **Do not replace the entire editable element during composition.** Preserves the editing surface.
6. **App shortcuts do not steal composition keys.** Check `KeyboardEvent.isComposing` on Web; check `markedTextRange != nil` on UIKit; check `hasMarkedText` on AppKit.
7. **Enter / Escape during composition may participate in IME behavior.** Do NOT assume Enter always means submit, or Escape always means cancel form / modal. The IME may consume these keys.

## IME-safe validation

**Reject the anti-pattern:** "validate the field after every character regardless of composition state."

Validation timing options:

- On committed edit (composition ends) — preferred for many fields.
- On field submit (e.g. Enter pressed when NOT in composition) — common for forms.
- After composition ends — common for search / filter fields.
- Non-destructive live validation (e.g. visual hint without rewriting the value) — acceptable when it does not destroy composition.

## IME-safe rerendering (Web / reactive frameworks)

When using a reactive framework (React / Vue / Svelte / etc.):

- Do NOT recreate the `<input>` / `<textarea>` / `contenteditable` element during composition.
- Do NOT overwrite the `value` (controlled input) during composition unless the value is derived from the composition itself.
- Do NOT programmatically set `selectionStart` / `selectionEnd` during composition.
- Preserve the existing DOM node; preserve its current `value`; preserve its selection.

If rerender is unavoidable, defer the rerender to after `compositionend`.

## CJK IME first-class

The Skill explicitly treats CJK IMEs as first-class:

- Chinese Pinyin (拼音) — phonetic → candidate characters.
- Japanese Kana-Kanji (かな漢字) — hiragana → kanji candidates.
- Korean (한글) — jamo → syllable blocks.
- Vietnamese, Thai, Arabic, Hebrew — all have their own composition semantics.

The Skill does NOT defer IME support to "later" or treat it as a Latin-keyboard-first assumption.

## Reference

- `references/text-input-lifecycle.md` — lifecycle model.
- `references/uikit-text-input.md` — UIKit `UITextInput` protocol.
- `references/appkit-text-system.md` — AppKit `NSTextInputClient`.
- `references/web-ime-editing.md` — Web CompositionEvent / InputEvent.
- `references/text-editing-accessibility.md` — accessibility + IME independence.