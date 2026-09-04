# SwiftUI Focus & Commands

SwiftUI focus and command APIs (verified in DocC + installed SDK). Only after per-symbol verification; the Skill does NOT invent SwiftUI focus APIs.

## FocusState

`@FocusState` (DocC `FocusState`; installed SwiftUI swiftinterface). A SwiftUI property wrapper for focus state associated with the current SwiftUI scene / view. Use to read / write keyboard focus.

| Availability | |
|---|---|
| iOS | 15.0+ |
| iPadOS | 15.0+ |
| Mac Catalyst | 15.0+ |
| macOS | 12.0+ |
| tvOS | 15.0+ |
| visionOS | 1.0+ |
| watchOS | 8.0+ |

`@FocusState` is appropriate for reading / writing focus placement on a SwiftUI view. Do NOT misuse it as a global selection state, navigation history, or app-wide singleton focus manager.

## `.focused(_:)`

The `.focused(_:)` modifier binds a Boolean focus state to a control. Use to make a control focusable / unfocusable based on focus state. Same availability as `@FocusState`.

## FocusedValue

`@FocusedValue` reads a value exported from the focused sub-hierarchy. `@FocusedValues` is the container of exported values.

| Availability | |
|---|---|
| iOS | 17.0+ |
| iPadOS | 17.0+ |
| Mac Catalyst | 17.0+ |
| macOS | 14.0+ |
| tvOS | 17.0+ |
| visionOS | 1.0+ |
| watchOS | 10.0+ |

`FocusedValue` is for **command context**, not focus placement. Commands consume `@FocusedValue` to know the target context.

`.focusedSceneValue(_:)` (iOS 15+ / macOS 12.0+) exports a value at the scene level for the focused sub-hierarchy.

## FocusState ≠ FocusedValue (HARD invariant)

| Concern | FocusState | FocusedValue |
|---|---|---|
| Role | Focus placement / state | Command context (exported values) |
| Lifecycle | Per-view / per-scene | Per-focused-sub-hierarchy |
| Use case | "Is this control focused?" | "What document is focused?" |
| Implementation | `@FocusState`; `.focused`; environment | `@FocusedValue`; `@FocusedValues`; `.focusedSceneValue` |

The Skill does NOT collapse them.

## Commands / CommandGroup / CommandMenu

`Commands` protocol describes a set of related commands. `CommandGroup` adds to a standard group (e.g. file menu). `CommandMenu` adds a new menu.

| Availability | |
|---|---|
| iOS | 14.0+ |
| iPadOS | 14.0+ |
| Mac Catalyst | 14.0+ |
| macOS | 11.0+ |
| tvOS | 14.0+ |
| visionOS | 1.0+ |
| watchOS | 7.0+ |

**Platform behavior varies**: `CommandMenu` requires a menu bar (macOS). On iOS / iPadOS / tvOS / visionOS / watchOS, `Commands` may surface through different mechanisms.

The Skill does NOT claim identical menu presentation on every platform just because `Commands` exists.

## keyboardShortcut(_:)

`.keyboardShortcut(_:)` attaches a keyboard shortcut to a SwiftUI view (button; toggle; etc.).

`KeyboardShortcut` / `KeyEquivalent` / `EventModifiers` are the SwiftUI types used in the modifier.

| Availability | |
|---|---|
| iOS | 14.0+ |
| iPadOS | 14.0+ |
| Mac Catalyst | 14.0+ |
| macOS | 11.0+ |
| tvOS | 14.0+ |
| visionOS | 1.0+ |
| watchOS | 7.0+ |

## What the Skill does NOT do

- Does NOT claim per-pattern SwiftUI focus behavior parity across platforms.
- Does NOT recommend `@FocusState` for global selection state.
- Does NOT confuse `FocusedValue` (command context) with `FocusState` (focus placement).
- Does NOT fabricate SwiftUI focus symbols not in DocC + installed SDK.

## Reference

- `references/focus-topology.md` — focus topology + scope rule.
- `references/focus-continuity.md` — restoration across dismissal / reflow / navigation.
- `references/shortcuts-and-conflicts.md` — shortcut choice + standard preservation.
- `references/command-scope.md` — scope rule + enablement consistency.