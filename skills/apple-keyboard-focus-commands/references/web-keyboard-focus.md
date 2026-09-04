# Web Keyboard & Focus

Web keyboard navigation, focus, and shortcuts. Verified from W3C / WAI-ARIA 1.2 / HTML Living Standard / WCAG 2.2. The Skill does NOT fake AppKit responder-chain behavior on Web.

## Native focus order

Web focus order follows DOM order (HTML Living Standard). The Skill recommends:

- Logical DOM / semantic structure.
- Avoid positive `tabindex` choreography (e.g. `tabindex="1"`, `tabindex="2"`) to force focus order via DOM override.
- Use `tabindex="0"` to make a non-focusable element focusable (e.g. custom widget).
- Use `tabindex="-1"` to make an element focusable programmatically but not via Tab (e.g. a programmatically focused heading).

## `:focus-visible`

CSS `:focus-visible` distinguishes keyboard-driven focus from pointer-driven focus. The Skill uses `:focus-visible` for visible focus indication:

```css
:focus-visible {
  outline: 2px solid var(--accent);
  outline-offset: 2px;
}
```

`:focus-visible` is supported in Baseline 2023 browsers (Chrome, Edge, Firefox, Safari).

## Natively focusable elements

The following are natively focusable on Web:

- `<button>`
- `<a href="...">`
- `<input>`
- `<select>`
- `<textarea>`
- `<summary>` (within `<details>`)
- `[tabindex]` non-negative value

The Skill uses these primitives for keyboard operability. For custom widgets, the Skill applies WAI-ARIA roles + `tabindex` as appropriate.

## Modal dialog (`<dialog>`)

HTML `<dialog>` with `showModal()` provides modal focus containment:

- Focus moves into the `<dialog>` when opened.
- Focus is constrained to the `<dialog>` while open.
- Focus returns to the invoking control when closed.
- Browser provides Escape to close the modal `<dialog>`.

The Skill uses native `<dialog>` rather than reimplementing modal focus containment.

## Keyboard event handling

The Skill uses standard Web keyboard event handling:

- `keydown` for press.
- `keyup` for release.
- `key` (deprecated; do not use).
- `keyCode` (deprecated; do not use).

Standard key values:

| Key | `event.key` value |
|---|---|
| Tab | `"Tab"` |
| Enter / Return | `"Enter"` |
| Escape | `"Escape"` |
| Arrow keys | `"ArrowUp"` / `"ArrowDown"` / `"ArrowLeft"` / `"ArrowRight"` |
| Space | `" "` (space) |
| Backspace | `"Backspace"` |
| Delete | `"Delete"` |
| Meta (Cmd on macOS, Windows key on Windows) | `"Meta"` |
| Control | `"Control"` |
| Alt / Option | `"Alt"` |
| Shift | `"Shift"` |

Modifier flags: `event.metaKey` / `event.ctrlKey` / `event.altKey` / `event.shiftKey`.

## Shortcuts on Web

Web shortcuts may use:

- `Ctrl` (Windows / Linux)
- `Cmd` (macOS — `event.metaKey`)
- Cross-platform helper: `(event.metaKey || event.ctrlKey)` for compatibility.

Standard Web shortcuts:

| Shortcut | Conventional meaning |
|---|---|
| Ctrl/Cmd + C | Copy |
| Ctrl/Cmd + V | Paste |
| Ctrl/Cmd + X | Cut |
| Ctrl/Cmd + A | Select All |
| Ctrl/Cmd + Z | Undo |
| Ctrl/Cmd + Shift + Z | Redo |
| Ctrl/Cmd + S | Save (web app convention) |
| Ctrl/Cmd + F | Find (browser provides) |
| Tab / Shift-Tab | Focus next / previous |

The Skill does NOT hijack browser-reserved shortcuts (browser navigation; reload; find; address-bar) without strong reason.

## Hmm what about touch Web + hardware keyboard

A touch device (smartphone / tablet) can have a hardware keyboard. When a hardware keyboard exists:

- Semantic focus should work.
- Keyboard alternatives should function where relevant.
- Do NOT inject desktop-hover behavior just because a keyboard is connected.
- Input modalities remain independent capabilities.

## ARIA live regions + focus changes

When focus changes non-obviously (programmatic focus after async completion; focus restoration after reflow), the Skill uses `aria-live="polite"` or `aria-live="assertive"` to announce the change.

## Custom widgets

For custom focusable widgets:

- Use `tabindex="0"` + `role="button"` (or appropriate role).
- Provide keyboard activation (Return / Space).
- Provide visible focus indicator.
- Do NOT invent a focus API; use HTML + ARIA.

## What the Skill rejects

- **Positive `tabindex` choreography** — `tabindex="1"`, `tabindex="2"` to force focus order via DOM override.
- **Browser shortcut hijacking** — hijacking Ctrl + R, Ctrl + L, Ctrl + F, etc.
- **Custom focus API invention** — building a focus system instead of using native focus + ARIA.
- **AppKit responder-chain emulation** — fake macOS-style first-responder on Web.

## Reference

- `references/focus-topology.md` — focus topology.
- `references/focus-continuity.md` — restoration.
- `references/keyboard-navigation.md` — traversal.
- `references/keyboard-accessibility.md` — accessibility.