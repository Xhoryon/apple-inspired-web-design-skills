# Keyboard Accessibility

The Skill's accessibility constraints on keyboard / focus / command interaction. Critical information must be operable via keyboard. The Skill honors WCAG 2.2 + WAI-ARIA 1.2 + Apple accessibility guidance.

## Hard invariants

1. **All functionality operable via keyboard** unless inherently pointer-only (signature drawing; on-screen drawing; precise spatial gestures that the user cannot reasonably reproduce via keyboard — and that have a keyboard alternative).
2. **Visible focus indicator** for any focusable element.
3. **No keyboard trap** — focus can always be moved away using only a keyboard interface.
4. **Logical focus order** — DOM / semantic order; not arbitrary positive tabindex.
5. **Alternative input** — essential pointer-only actions have a keyboard alternative where applicable.

## WCAG 2.2 Success Criteria (verified)

| SC | Title | Application |
|---|---|---|
| 2.1.1 | Keyboard | All functionality via keyboard |
| 2.1.2 | No Keyboard Trap | Focus can be moved away with only a keyboard |
| 2.4.7 | Focus Visible | Visible focus indicator on focusable elements |
| 2.4.11 | Focus Not Obscured (Minimum) | Focus indicator is not entirely hidden by other content |
| 4.1.3 | Status Messages | Status messages programmatically determinable (ARIA live regions) |

The Skill honors all five.

## WAI-ARIA 1.2 (verified)

- `role="button"`, `role="menuitem"`, `role="tab"`, `role="dialog"` for custom widgets.
- `aria-live="polite"` for status updates.
- `aria-live="assertive"` for errors / urgent updates.
- `aria-label`, `aria-labelledby` for accessible names.
- `aria-current` for current item in a list / tab / step.
- `aria-keyshortcuts` for advertising shortcuts.

## Apple accessibility guidance

| Setting | Effect |
|---|---|
| Full Keyboard Access (macOS) | Tab through focusable elements + cross-group navigation |
| Reduce Motion | Reduce / remove animations |
| Increase Contrast | Strengthen contrast |
| Reduce Transparency | More opaque surfaces |
| VoiceOver (iOS / iPadOS / macOS) | Screen reader; navigates accessibility elements |
| AssistiveTouch (iOS / iPadOS) | Alternative touch gestures |

The Skill honors Apple accessibility settings; the app exposes accessibility information through ARIA / VoiceOver / accessibility hints.

## Visible focus

The Skill publishes the rule:

> **Keyboard users need to know where focus is. Do NOT intentionally hide meaningful focus indication merely for cleaner visuals.**

- Use system focus effects where appropriate (Web `:focus-visible`; macOS native focus ring; iOS focus ring).
- Custom focus visuals are allowed only when system effects are insufficient AND the custom visual preserves accessibility.
- WCAG 2.2 SC 2.4.11: focus indicator must not be entirely obscured by other content.

## System primitive first

The Skill publishes the rule:

> **System primitive / focus effect first. Custom focus visuals only when genuinely necessary.**

- Use platform focus effects (SwiftUI `FocusState` indicator; macOS native focus ring; Web `:focus-visible`).
- Custom focus visuals: rare; must remain accessible.

## Discoverability

Important commands should be accessible through:

- Menu items (with shortcut hints).
- Toolbars.
- Context menus.
- Buttons.
- VoiceOver / accessibility rotor.
- Discoverability surfaces (settings; first-run; help).

The Skill does NOT allow important commands to be hidden behind shortcut-only access.

## No keyboard trap

Hard invariant:

> **Do NOT unintentionally trap keyboard focus.**

A true modal may constrain focus to its temporary interaction context. But closing / dismissing it must restore appropriate focus. Nonmodal regions must NOT act like accidental focus prisons.

## Alternative input

Hard rule:

> **Essential pointer-only gestures should have a keyboard / command path where keyboard is an applicable alternative.**

Examples:

- Reorder → keyboard shortcut to move up / down + menu command.
- Delete → Delete key + menu command.
- Open → Return / Space + menu command.
- Move → arrow keys + menu command.
- Zoom actions → ⌘+ / ⌘- + menu command.
- Contextual commands → right-click menu + keyboard equivalent.

Coordination with `apple-direct-manipulation`.

## What the Skill rejects

- **Hidden Focus** — suppressing the visible focus indicator.
- **Keyboard Trap** — focus enters a widget / context and cannot leave.
- **No Keyboard Alternative** — essential gesture has no keyboard equivalent.
- **App assumptions about Accessibility mode** — accessibility mode does NOT automatically repair inaccessible app semantics. The app must be accessible by default.

## Reference

- `references/web-keyboard-focus.md` — Web keyboard + focus.
- `references/focus-continuity.md` — restoration.
- `references/keyboard-navigation.md` — traversal.
- `references/shortcuts-and-conflicts.md` — shortcuts + standard preservation.