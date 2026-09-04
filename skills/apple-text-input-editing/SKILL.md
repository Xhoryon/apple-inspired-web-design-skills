---
name: apple-text-input-editing
description: Use when designing how people enter, compose, select, edit, replace, copy, paste, and manipulate text without breaking platform text systems, language input, selection continuity, or standard editing conventions. Owns the text input lifecycle, IME / marked-text composition semantics, caret / selection, standard edit actions (Cut / Copy / Paste / Select / Select All / Delete), paste policy, plain vs rich text distinction, and selection continuity. Applies to Native iOS / iPadOS / macOS / Mac Catalyst / visionOS / tvOS / watchOS and Web (Desktop / Touch / PWA). Do NOT use for typography or text visual hierarchy (use stable apple-design-foundations / apple-web-composition), focus routing (use apple-keyboard-focus-commands), focused-state appearance (use apple-control-states), pointer / drag mechanics (use apple-direct-manipulation), validation outcome / error communication (use apple-feedback-response), edit-menu visual presentation (use existing UI / modality owners), structural layout (use apple-adaptive-structure), undo system semantics broadly (use apple-feedback-response), or animation (use apple-motion-physics).
version: "0.7.0-dev"
license: MIT
---

# apple-text-input-editing

Text Input, Editing & Selection. Owns the lifecycle and semantics of editable text — composition, insertion, selection, replacement, standard edit operations, and editing continuity. Does NOT own typography, focus routing, validation outcome, or undo system broadly.

## When to use

Use when the task involves text input, insertion point, text selection, IME / marked-text composition, CJK input, Cut / Copy / Paste, Select / Select All, plain vs rich text distinction, multiline editing, secure text input, or selection continuity. Symptoms: "Chinese IME breaks when typing", "preserve selection while editor rerenders", "Paste rich text as plain text", "where should caret go after replacement", "custom text engine", "edit menu".

Do NOT use for: typography or text visual hierarchy (stable v1.1 `apple-design-foundations` / `apple-web-composition`); focus routing (`apple-keyboard-focus-commands`); focused-state appearance (`apple-control-states`); pointer / drag mechanics (`apple-direct-manipulation`); validation outcome / error communication (`apple-feedback-response`); edit-menu visual presentation; structural layout (`apple-adaptive-structure`); broad undo / recovery (`apple-feedback-response`); animation (`apple-motion-physics`).

## Core model (Skill-owned vocabulary)

The Skill uses a **text input lifecycle** (Layer C Skill-owned, NOT extracted from Apple HIG as a literal primitive set):

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

Composition is first-class: provisional composition text is not equivalent to committed text.

## Hard invariants

1. **System text control first.** Use `UITextField` / `UITextView` / `NSTextField` / `NSTextView` / SwiftUI `TextField` / `TextEditor` / Web `<input>` / `<textarea>` / `contenteditable` before custom text engine. Custom engine requires substantive reason.
2. **Composition is first-class.** Provisional composition text is not equivalent to committed text. Marked text may still change or be cancelled. Treat CJK Pinyin / Japanese Kana-Kanji / Korean / dead-key / handwriting as first-class, not edge cases.
3. **IME-safe validation.** Avoid destructive semantic validation / reformatting that breaks an active composition session. Validation timing: on committed edit; on field submit; after composition ends; non-destructive live validation. No universal rule.
4. **IME-safe rerendering.** Preserve marked / composing text, caret, selection, focus when reactive frameworks update the model. Do not recreate the editable element or overwrite its value while composition is active.
5. **App shortcuts do not steal composition keys.** Key events during composition must not be automatically interpreted as application shortcuts. Web: `KeyboardEvent.isComposing` and composition lifecycle.
6. **Caret ≠ extended selection.** Insertion point is a zero-length editing position; extended selection spans editable content. Distinct semantic states.
7. **Focus ≠ text selection.** Focus = which editor receives keyboard input. Text selection = caret / range inside the focused editor. Do not collapse.
8. **Text selection ≠ component selection.** Selected paragraph text is not equivalent to selected sidebar row. Different owners / semantics.
9. **Selection continuity preserved where possible.** Do not reset caret to end after every model update. Benign updates should preserve caret / selection.
10. **Standard edit actions preserved.** Cut / Copy / Paste / Select / Select All / Delete provided by system controls. Custom edit menu should add semantic value, not gratuitously replace.
11. **Deprecated edit menu API (`UIMenuController`) is superseded by `UIEditMenuInteraction`** (iOS 16+). The Skill does NOT teach `UIMenuController` as the current default.
12. **Legacy `NSTextInput` is superseded by `NSTextInputClient`** on AppKit. The Skill documents `NSTextInputClient` as the modern contract.
13. **Paste is destination-driven.** Plain text destination → strip rich formatting. Rich text destination → preserve formatting. Product semantics matter.
14. **Undo is recovery semantics.** `Feedback Response` owns broad recovery. `Text Editing` owns meaningful grouping of edits from the editor perspective. Do not duplicate the undo architecture.
15. **Custom editor gate.** Custom `UITextInput` / `NSTextInputClient` is justified only for specialized document models, advanced layout, code / structured editors, or unusual canvas / spatial editors. Comes with IME / accessibility / clipboard / editing commands / Writing Tools responsibilities.
16. **CJK / IME first-class.** Not deferred because most users type Latin characters.
17. **Three evidence layers explicit.** Layer A (Apple official / W3C) / Layer B (Skill synthesis) / Layer C (Skill implementation vocabulary) — never collapsed.
18. **Paraphrase by default.** Apple HIG portal pages return JS-rendered SPA shells; verbatim HIG text is UNVERIFIED in this session.
19. **No fabricated Apple APIs.** Every symbol passes the scanner's `TEXT_INPUT_EDITING_SELECTION` allowlist. No `UITextCompositionManager`, `UIIMEController`, `NSTextCompositionEngine`, `TextSelectionManager`, etc.

## Routing integration

The orchestrator (`apple-experience-design`) adds a row for the new Skill. **Selective load rule:** the row applies ONLY when the task explicitly involves text editing / composition / selection / standard edit operations. For typography / focus / motion / structural / material / visual tasks, the orchestrator routes WITHOUT loading `apple-text-input-editing`.

Discriminator checklist (the orchestrator uses these to gate loading):

- Is the task about IME / composition / marked text? If yes, load.
- Is the task about caret / selection continuity? If yes, load.
- Is the task about Cut / Copy / Paste semantics inside an editor? If yes, load.
- Is the task about plain vs rich text? If yes, load.
- Is the task about validation timing relative to composition? If yes, load.
- Is the task purely about typography / visual hierarchy? If yes, do NOT load — use stable v1.1 `apple-design-foundations`.
- Is the task about a custom text engine? If yes, load (gate decision), but document the system-text-control-first principle.
- Is the task about app-level Save / Open shortcuts? If yes, do NOT load — use `apple-keyboard-focus-commands`.
- Is the task about the focused editor's focus? If yes, do NOT load — use `apple-keyboard-focus-commands`.
- Is the task about the focused editor's visual state? If yes, do NOT load — use `apple-control-states`.

## Per-platform text input / editing API surface (summary)

| Platform | Single-line | Multi-line | IME / composition | Standard edit actions | Notes |
|---|---|---|---|---|---|
| iOS / iPadOS | `UITextField` (iOS 2.0+) | `UITextView` (iOS 2.0+) | `UITextInput.markedTextRange` / `setMarkedText` / `unmarkText` | `UIResponderStandardEditActions`; `UIEditMenuInteraction` (iOS 16+) | Touch selection handles system-provided |
| macOS | `NSTextField` (macOS 10.0+) | `NSTextView` (macOS 10.0+) | `NSTextInputClient` (modern); `markedText` | AppKit responder chain + `NSMenu` / `NSMenuItem` | Multi-range selection; rich text via `NSAttributedString` |
| watchOS | limited (small text fields) | limited (no multi-line) | limited (Digital Crown + small) | limited | System text controls; not the primary form input |
| tvOS | focused field | n/a (focus-driven) | n/a (focus-driven) | n/a | Text entry via remote keyboard; not the primary form input |
| visionOS | focused field | limited | gaze + pinch + spatial keyboard | limited (spatial menu) | Spatial UI |
| SwiftUI (all) | `TextField` (iOS 14+) | `TextEditor` (iOS 14+) | delegates to platform text system | delegates to platform | System text control integration |
| Web | `<input type="text">` | `<textarea>` / `<input type="textarea">` | `compositionstart` / `compositionupdate` / `compositionend` events; `KeyboardEvent.isComposing` | native browser edit; `document.execCommand` (legacy) / Clipboard API | `contenteditable` for rich text (with caution) |

## Reference catalog (Tier 2)

- `references/text-input-lifecycle.md` — input intent → composition → commit → selection update → model update.
- `references/ime-composition.md` — IME lifecycle; composition-as-provisional; CJK / multistage; keyboard shortcut during composition; Enter / Escape during composition.
- `references/text-selection.md` — caret vs selection; selection granularity; pointer / touch selection ownership; system touch handles.
- `references/selection-continuity.md` — selection survives benign updates; reflow; replacement caret/selection; focus changes; editor switching.
- `references/edit-actions-and-menus.md` — Cut / Copy / Paste / Select / Select All / Delete; `UIEditMenuInteraction` (iOS 16+); `UIMenuController` legacy.
- `references/clipboard-paste.md` — paste policy; multiple representations; plain vs rich.
- `references/plain-rich-text.md` — plain text vs rich text; NSAttributedString; contenteditable.
- `references/swiftui-text-editing.md` — SwiftUI `TextField` / `TextEditor`; selection; focus integration.
- `references/uikit-text-input.md` — `UITextField` / `UITextView` / `UITextInput` protocol; modern `UIEditMenuInteraction`.
- `references/appkit-text-system.md` — `NSTextField` / `NSTextView` / `NSTextInputClient` (modern); legacy `NSTextInput`.
- `references/web-ime-editing.md` — `<input>` / `<textarea>` / `contenteditable`; `CompositionEvent`; `isComposing`; reactive framework hazards.
- `references/text-editing-accessibility.md` — WCAG 2.2 SC 1.3.5 / 3.3.1 / 3.3.2 / 3.3.3 / 3.3.4; ARIA roles; VoiceOver; standard controls.

## Process notes

- The Skill does NOT introduce a fabricated Apple API symbol. Every symbol is verified in DocC + installed SDK per the inventory.
- The Skill does NOT make verbatim Apple HIG claims (HIG portal pages return JS-rendered shells in this session).
- The Skill does NOT assume cross-platform text system parity.
- The Skill is verified by 20 functional tests (AE7-T1..T20) + 20 adversarial tests (AE7-G1..G20) + 12 router tests (AE7-R1..R12).
- The Skill consumes but does not redefine the v0.6 Skills (`apple-control-states`, `apple-keyboard-focus-commands`, `apple-motion-physics`, `apple-navigation-spatial`, `apple-direct-manipulation`, `apple-modality-overlays`, `apple-adaptive-structure`, `apple-feedback-response`).
- The Skill coordinates with `apple-feedback-response` on validation timing (Text Editing decides when validation runs against committed text; Feedback decides how invalid result is communicated).
- The Skill coordinates with `apple-keyboard-focus-commands` on focus vs selection (which editor is focused; where caret is inside the editor).