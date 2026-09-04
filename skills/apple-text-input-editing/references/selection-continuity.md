# Selection Continuity

The Skill's rule: **preserve editing intent where possible; do not reset caret to end after every model update.**

## Hard principle

> **Preserve editing intent where possible.**

Benign updates (formatting changes, non-editing state refreshes, reflow) should preserve the caret position and selection range. Destructive updates (replace entire value) reset the selection to end or 0; the Skill does NOT pretend this is "selection continuity" because the underlying value has changed.

## When selection survives (Layer B synthesis)

- formatting change (font, color, weight) that doesn't change the underlying text — preserve caret + selection.
- structural reflow (container resize) — preserve caret + selection if the underlying text is unchanged.
- focus change (e.g. user opens a menu) — preserve selection inside the editor.
- editor switching (e.g. user navigates to another editor and back) — preserve selection across the switch.
- benign content update (server-rendered replacement that preserves the user's text) — preserve caret + selection.
- composition transition (e.g. IME composition starts / ends) — preserve selection where the platform allows.

## When selection does NOT survive

- destructive update (replace entire value) — selection resets to 0 or to the end of the new value.
- document reload (user explicitly reloads) — selection resets.
- editor destroyed / re-created (e.g. config change) — selection is lost; the new editor starts with caret at 0.
- composition cancelled (e.g. user dismisses IME candidate window) — the IME controls the selection; the app does NOT override.

## IME composition + selection continuity

- During composition: the IME controls the selection. The app does NOT programmatically move the selection.
- After composition ends: the system updates the selection based on the committed text.
- If the app rewrites the value during composition: the composition is destroyed, and the selection is lost.

## Reactive framework rerender (Web)

**Reject the anti-pattern:** "controlled rerender can always put caret at end."

- Preserve the existing DOM node across rerenders.
- Preserve the `value` of the input unless the value is derived from user input.
- Preserve `selectionStart` / `selectionEnd` / `selectionDirection` across rerenders.
- During composition: do NOT update `value` from state that does not include the composition.

## React / Vue / Svelte implementation notes

- React: use `defaultValue` (uncontrolled) when composition is critical; or maintain a stable `value` + `key` to avoid recreating the element.
- Vue: use `v-model` carefully during composition; consider `lazy` modifier.
- Svelte: use `bind:value` carefully during composition.

These are implementation hints; verify with real IME testing.

## Reference

- `references/text-input-lifecycle.md` — input lifecycle.
- `references/text-selection.md` — caret / selection distinction.
- `references/ime-composition.md` — IME composition.
- `references/web-ime-editing.md` — Web reactive framework hazards.