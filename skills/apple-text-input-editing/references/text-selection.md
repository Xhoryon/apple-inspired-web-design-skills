# Text Selection

The Skill's caret / selection distinction. **Caret ≠ extended selection.** **Text selection ≠ component selection.** **Text selection ≠ focus.**

## Hard distinction

- **Caret (insertion point)**: zero-length editing position. Where the next character will be inserted.
- **Extended selection**: non-empty range. Spans editable content from start to end.
- These are distinct semantic states. The model may expose them differently per platform.

## Per-platform selection representation

| Platform | Caret | Extended selection |
|---|---|---|
| UIKit | `selectedTextRange` with empty range | `selectedTextRange` with non-empty range |
| AppKit | `selectedRanges[0]` with empty `NSRange` | `selectedRanges[0]` with non-empty `NSRange`; supports multi-range |
| SwiftUI | delegates to platform | delegates |
| Web | `Selection.collapse(node, offset)` | `Selection.extend(node, offset)` or `Selection.addRange(range)` |

## Hard invariants

1. **Caret ≠ extended selection.** A caret is a zero-length position; an extended selection is a non-empty range. Distinct semantic states.
2. **Text selection ≠ component selection.** Text selection inside an editor (e.g. selected paragraph text) is NOT the same as component / item selection (e.g. selected sidebar row, selected list item). Different owners / semantics.
3. **Text selection ≠ focus.** Focus = which editor / control receives keyboard input. Text selection = caret / range inside the focused editor. A text editor can retain selection while focus moves (e.g. when a menu opens).

## Per-platform selection ownership

- **UIKit**: `UITextInput.selectedTextRange`; modern text controls conform.
- **AppKit**: `NSText.selectedRanges` (array of ranges; supports multi-range / non-contiguous).
- **SwiftUI**: selection model varies by control and OS version; may delegate to UIKit / AppKit.
- **Web**: `Selection` API: zero or more `Range` objects; supports multiple non-contiguous selections.

The Skill does NOT assume parity.

## Selection granularity (per platform)

| Platform | Granularity options |
|---|---|
| UIKit | character; word (programmatic; UIKit does not expose word selection as a public API; system handles double-tap word selection); line; paragraph; all |
| AppKit | character; word; line; paragraph; all (selectable via `NSTextSelectionGranularity`) |
| SwiftUI | character; word; line; paragraph (varies by control / OS version) |
| Web | character; word (system double-click); line (system triple-click) |

Do not force identical granularity behavior across platforms.

## Pointer / touch selection ownership

- **Direct Manipulation** owns: pointer / touch movement mechanics; drag-to-select physics; selection handle physics.
- **Text Editing** owns: resulting text selection semantics; what the selection means; how the selection updates on further input.

**System primitive first:** use system touch-selection handles. Do not custom-build handles merely to resemble Apple.

## Standard edit actions and selection

The standard edit actions (`cut` / `copy` / `paste` / `select` / `selectAll` / `delete`) operate on the current selection:
- Empty selection (caret) + `copy` → typically a no-op.
- Extended selection + `cut` / `copy` → cut / copy the selected text.
- `selectAll` → replace selection with whole-content range.
- `paste` → replace selection with pasteboard content (collapsed selection = insert; extended selection = replace).

## Reference

- `references/text-input-lifecycle.md` — input lifecycle.
- `references/selection-continuity.md` — selection survives benign updates.
- `references/edit-actions-and-menus.md` — Cut / Copy / Paste / Select / Select All.
- `references/uikit-text-input.md` — UIKit selection.
- `references/appkit-text-system.md` — AppKit selection.
- `references/web-ime-editing.md` — Web Selection API.