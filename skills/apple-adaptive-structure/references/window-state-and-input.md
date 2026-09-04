# Window State and Input Context

Window state and input context are STRUCTURAL MODIFIERS. The Skill does NOT own the implementation of any window state or input context (the platform does); the Skill records how each modifier affects the recommended anatomy.

## Window state (Apple platforms)

| State | Platform | Default anatomy | Modifier |
|---|---|---|---|
| `single-window` | All | The default for the form factor. | n/a |
| `multi-window` | iPad (iPadOS 13+), iPhone (iOS 13+), Mac Catalyst, Mac | Multiple instances of the main scene. Each is independent. | The Skill does NOT recommend a different anatomy per window; each window is a full instance. |
| `split-view` (2-up / 3-up / 4-up) | iPad | iPad multi-column. The Skill does NOT collapse the sidebar. | n/a |
| `slide-over` | iPad | compact-width size class. The sidebar collapses. | `reflow` verb. |
| `stage-manager` | iPad (M-series) and Mac (Apple Silicon) | Resizable windows. Anatomy adapts to the resized width. | The Skill publishes: at narrow Stage Manager widths, the anatomy is the compact form; at wide widths, the regular form. |
| `full-screen` | iOS / iPadOS / macOS | The default for the form factor. | n/a |
| `picture-in-picture` | iOS / iPadOS / macOS | A separate floating surface. The Skill does NOT recommend showing the main anatomy in PiP. | n/a |
| `popover-anchored` | Mac | A non-modal anchored surface. The main window is hidden or shrunk. | `collapse` verb on the main window. |

## Window state (Web)

| State | Default anatomy | Modifier |
|---|---|---|
| `browser` | The default Web anatomy. | n/a |
| `standalone` (PWA) | The default Web anatomy. | The Skill records: standalone mode looks like a native app; the Safe Area insets are different. |
| `minimal-ui` (PWA) | Same as `standalone` for the structural frame. | n/a |
| `fullscreen` (PWA) | The default Web anatomy, fullscreen. | The Skill records: the URL bar is hidden; the available height is the viewport height. |

## Input context

| Input | Platform | Structural implication |
|---|---|---|
| `touch` | iPhone, iPad, iPod touch, Touch Web | No hover affordances. The Skill does NOT expose structural surfaces on hover. The Skill DOES expose them on tap / press. |
| `pointer` | iPad (Magic Keyboard / trackpad), Mac, Desktop Web, Mac Catalyst, iPhone (M-series, with external display) | Hover affordances. The Skill MAY expose structural surfaces on hover (e.g. an inspector preview). |
| `keyboard` | Mac, Desktop Web | Keyboard navigation. The Skill MUST preserve focus order across structural change. |
| `hybrid` | iPad (touch + Magic Keyboard), Mac Catalyst (touch + pointer) | Both touch and pointer are available. The Skill exposes BOTH touch and pointer affordances. The Skill does NOT default to one. |
| `focus-driven` | tvOS, visionOS (sometimes) | Focus halo is the primary input. The Skill does NOT expose structural surfaces on hover or touch. |
| `gaze-driven` | visionOS | Gaze + pinch is the primary input. The Skill does NOT expose structural surfaces on hover. |
| `stylus` | iPad (Apple Pencil) | Pointer + pressure. The Skill MAY expose structural surfaces on Pencil hover (iPadOS 16.4+). |

## Input × size class matrix

The Skill publishes the recommended anatomy per (size class, input context). The matrix is HEURISTIC; the app's choice may differ.

| Size class | Input | Anatomy |
|---|---|---|
| compact-width, regular-height | touch | Card stack. No sidebar. |
| compact-width, regular-height | pointer | Card stack. The Skill MAY expose a secondary surface on hover. |
| regular-width, regular-height | touch | 2- or 3-column. Sidebar visible. |
| regular-width, regular-height | pointer | 2- or 3-column. Sidebar + content + optional inspector. Hover affordances. |
| regular-width, regular-height | hybrid | 2- or 3-column. Both touch and pointer affordances. |
| regular-width, regular-height | focus-driven | 1-column (focus-driven). The Skill abandons iPad multi-column. |
| regular-width, regular-height | gaze-driven | 1-column (gaze-driven). The Skill abandons iPad multi-column. |
| compact-width, compact-height | touch | Card stack. Sheets are more frequent (compact height). |
| compact-width, compact-height | pointer | Card stack. Sheets are more frequent. |

## Window state × input context matrix

| Window state | Input | Anatomy |
|---|---|---|
| `single-window` (default) | any | The default for the form factor. |
| `multi-window` (each window) | any | The default for the form factor. Each window is independent. |
| `split-view` (2-up) | any | 2-column or 1-column. The Skill does NOT collapse the sidebar in a 2-up split. |
| `split-view` (3-up / 4-up) | touch | 2-column per window. The sidebar is per window. |
| `split-view` (3-up / 4-up) | pointer | 2- or 3-column per window. Inspector per window. |
| `slide-over` | any | 1-column. The Skill reflows. |
| `stage-manager` (narrow) | any | 1- or 2-column. The Skill reflows. |
| `stage-manager` (wide) | any | 2- or 3-column. |
| `full-screen` | any | The default for the form factor. |
| `picture-in-picture` | any | The PiP surface is a stripped surface. The Skill does NOT show the main anatomy in PiP. |
| `popover-anchored` | pointer | 1-column. The parent surface is hidden. |
| `browser` (Web) | any | The default for the form factor. |
| `standalone` (PWA) | any | The default for the form factor + safe-area insets. |
| `minimal-ui` (PWA) | any | The default for the form factor. |
| `fullscreen` (PWA) | any | The default for the form factor, fullscreen. |

## Why the Skill separates window state from input context

The Skill does NOT bundle "Stage Manager = iPad with touch" because Stage Manager can be iPad-with-pointer (Magic Keyboard), iPad-with-touch, Mac-with-keyboard, or Mac-with-pointer. The recommended anatomy depends on both. The Skill rejects "always use the iPad default in Stage Manager" as a rule.

## Anti-patterns

- **Window state as the structural driver** — using `stage-manager` to choose the anatomy without considering the resized width. Rejected. The resized width + input context is the driver.
- **Input as the only modifier** — using `(pointer: fine)` alone to choose the anatomy without considering the size class. Rejected. The Skill pairs input with size class.
- **Display-mode as the only modifier** — using `display-mode: standalone` to choose the anatomy without considering the form factor. Rejected.
- **No input fallback** — designing a focus-driven anatomy that fails when the user has a Magic Keyboard attached. Rejected. The Skill supports hybrid input.
- **Web window state re-implementation** — drawing custom window chrome in JS on Web. Rejected. The Web is fullscreen by default; PWA display-modes are the only Web window state.

## What the Skill does NOT do

- Does NOT own the implementation of any window state or input context. The platform does.
- Does NOT recommend a custom window manager. The system provides one.
- Does NOT recommend a custom input handler. The system provides one.
- Does NOT publish a per-device window-state matrix. The window state is a modifier, not a structural driver.
