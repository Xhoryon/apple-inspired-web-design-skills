# Web IME & Editing

Web text input, editing, and IME composition. Verified from W3C / WHATWG / HTML Living Standard.

## Native text fields: `<input>` / `<textarea>`

- `<input type="text">` (HTML Living Standard) — single-line.
- `<input type="textarea">` (HTML Living Standard) — multi-line.
- `<textarea>` (HTML Living Standard) — multi-line free-form.

These provide:

- text input;
- selection (caret + range via `Selection` API);
- IME / composition via `compositionstart` / `compositionupdate` / `compositionend` events;
- standard browser edit actions (Cut / Copy / Paste / Select All);
- clipboard (with permission model);
- accessibility (when using system controls with proper labels).

## Rich / structured editing: `contenteditable`

`contenteditable` (HTML Living Standard) makes a region editable. Provides:

- text input;
- selection;
- IME / composition;
- standard edit actions;
- rich text (HTML serialization; not portable across browsers without normalization);
- multi-range selection (limited support).

**Caution:** `contenteditable` introduces more complex selection, DOM mutation, clipboard, undo, accessibility, browser interoperability. Use only when product / editor requirements justify rich text editing in Web.

## Composition lifecycle (W3C)

```
compositionstart
  ↓
compositionupdate (multiple)
  ↓
compositionend
```

Events on the editable element. `event.data` contains the composition text.

## `KeyboardEvent.isComposing`

W3C UI Events: `KeyboardEvent.isComposing` is `true` during an active IME composition. Apps check this to avoid stealing composition keys.

```javascript
document.addEventListener('keydown', function (e) {
  if (e.isComposing) return; // let IME handle the key
  // ...app-level shortcut handling
});
```

## `InputEvent.inputType`

W3C Input Events: `InputEvent.inputType` provides semantic type (`insertText`, `insertCompositionText`, `deleteContentBackward`, etc.). Apps can use this for semantic event handling.

## `beforeinput` event

`beforeinput` (Input Events Level 2) fires before the input is applied. Apps can intercept and prevent default. Use carefully — preventing default during composition can break the IME.

## Selection API (W3C)

```javascript
const sel = document.getSelection();
if (sel.rangeCount > 0) {
  const range = sel.getRangeAt(0);
  // range.startContainer, range.startOffset, range.endContainer, range.endOffset
  // range.collapsed === true means insertion point
  // range.toString() returns the selected text
}
```

## Clipboard API

W3C Clipboard API: `navigator.clipboard.read()`, `navigator.clipboard.write()`. Permission model varies by browser (Chrome / Edge / Firefox generally permit; Safari requires user gesture for write).

## Reactive framework hazards (Web)

When using React / Vue / Svelte / etc.:

- Do NOT recreate the `<input>` / `<textarea>` element during composition.
- Do NOT overwrite the `value` from state that does not include the composition.
- Do NOT programmatically set `selectionStart` / `selectionEnd` during composition.
- Preserve the existing DOM node across rerenders.
- If rerender is unavoidable, defer to after `compositionend`.

## contenteditable selection

`contenteditable` selection is exposed via `document.getSelection()`. Multi-range support is limited (most browsers coalesce into a single range).

## What the Skill does NOT do

- Does NOT recommend `contenteditable` for every Web text field.
- Does NOT recommend custom text engine for visual novelty.
- Does NOT assume Chromium behavior equals Safari behavior (test in both when possible).
- Does NOT pretend browser simulation proves UIKit / AppKit IME runtime.

## Reference

- `references/text-input-lifecycle.md` — input lifecycle.
- `references/ime-composition.md` — IME composition.
- `references/text-selection.md` — caret / selection.
- `references/selection-continuity.md` — selection continuity.
- `references/text-editing-accessibility.md` — accessibility.