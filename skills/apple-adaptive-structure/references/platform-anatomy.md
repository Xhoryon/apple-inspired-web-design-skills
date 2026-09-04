# Platform Anatomy

The per-platform structural vocabulary. Each platform has its own primitive set; the Skill uses the canonical verb set (from `structural-taxonomy.md`) and pairs it with the platform's primitives.

## iPhone (iOS)

### Structural primitives

- `NavigationStack` — push / drill-in. The canonical navigation primitive.
- `TabView` (bottom tab bar) — peer navigation. The default iPhone chrome.
- `.sheet` — modal task overlay.
- `.popover` (on iPhone in landscape) — non-modal anchored surface.
- `.inspector` — iOS 17+ inspector (rare on iPhone; canonical on iPad).
- `UIWindowScene` — multi-scene API available since iOS 13. **User-facing iPhone multi-window arrived with Stage Manager in iOS 16**, not earlier.

### Default anatomy by size class

| Size class | Anatomy |
|---|---|
| compact-width, regular-height (portrait) | Tab bar + content. NavigationStack for drill-in. |
| compact-width, compact-height (landscape) | Tab bar + content. Sheet is more frequent (compact height). |
| regular-width, regular-height (iPhone Plus / Max in landscape) | Tab bar + content. The tab bar is wider; otherwise same as portrait. |

### Anti-patterns

- **Sidebar on iPhone** — `NavigationSplitView` collapsed to one column. The Skill does NOT recommend exposing the sidebar as a separate surface on iPhone; the sidebar is folded into a tab bar.
- **Compact-height sheet dismissal** — using a drag-to-dismiss on a compact-height sheet. The Skill recommends the system primitive.

## iPad (iPadOS)

### Structural primitives

- `NavigationSplitView` (2- or 3-column) — the canonical iPad multi-column primitive.
- `NavigationStack` (inside `NavigationSplitView` detail) — drill-in.
- `TabView` — peer navigation. iPad can use either a bottom tab bar or a sidebar as the primary chrome.
- `inspector(isPresented:)` (iOS 17+) — on-demand right-side inspector.
- `UIWindowScene` + `UISceneSession` — multi-scene API since iPadOS 13.
- `UISplitViewController` (UIKit) — the UIKit multi-column primitive.
- Stage Manager (iPadOS 16+ / macOS 13+) — system-managed overlapping resizable windows. Apps participate via `NavigationSplitView(columnVisibility:)` and the SwiftUI `Scene.windowManagerRole(_:)` modifier (iOS 18+). **There is no `UIStageManager` class**; the Stage Manager experience is provided by the system, not by an app-facing class of that name.

### Default anatomy by size class

| Size class | Anatomy |
|---|---|
| regular-width, regular-height (standard) | 2- or 3-column. `NavigationSplitView` with sidebar + content (2) or sidebar + content + detail (3). Optional inspector on-demand. |
| compact-width, regular-height (Slide Over) | 1-column. Sidebar collapses; navigation is via drill-in. |
| regular-width, regular-height (Stage Manager, narrow) | 2-column. Sidebar visible; inspector on-demand. |
| regular-width, regular-height (Stage Manager, wide) | 3-column. Sidebar + content + detail; inspector on-demand. |

### Multi-column visibility

`NavigationSplitView` exposes `columnVisibility` (`.all`, `.automatic`, `.double-column`, `.detail-only`). The Skill recommends `.automatic` as the default; the system chooses based on the size class.

### Anti-patterns

- **Always-3-column** — pinning the sidebar + content + detail visible at all widths. Rejected. The Skill lets the system collapse the sidebar at compact width.
- **Forced sidebar on iPhone-as-iPad** — the Skill rejects "iPad in iPhone-as-iPad mode must have a sidebar" as a rule. The iPhone-as-iPad mode is iPhone anatomy on iPhone hardware.
- **Re-implementing `NavigationSplitView`** — building a custom multi-column from `HStack` + `if` branches. Rejected. Use the system primitive.

## Mac Catalyst (iPad-on-Mac)

### Structural primitives

- The same as iPad. Mac Catalyst is iPad anatomy on Mac hardware.
- The app can opt into Mac idiom via `UISceneSession` configuration.

### Default anatomy by size class

| Size class | Anatomy |
|---|---|
| regular-width, regular-height (default) | 2- or 3-column. Same as iPad. |
| compact-width, regular-height (resized small) | 1-column. Same as iPad in Slide Over. |

### Anti-patterns

- **Treating Mac Catalyst as Mac** — using `NSWindow` and `MenuBarExtra` from Mac Catalyst. Rejected; the Mac Catalyst binary uses the UIKit runtime.
- **Treating Mac Catalyst as iPhone** — collapsing all chrome because the window is narrow. Rejected; the user can resize the window and the chrome should adapt.

## Mac (macOS)

### Structural primitives

- `NSWindow` (AppKit) — the canonical window primitive.
- `NSWindowController` — manages a window + its content.
- `NSWindowController` + `NSPopover` — non-modal auxiliary surface.
- SwiftUI `WindowGroup` — multiple windows with the same content.
- SwiftUI `Window` — single, addressable window.
- SwiftUI `Settings { }` — the standard preferences window.
- `MenuBarExtra` — menu bar item (Mac-specific).
- `NSSplitViewController` — multi-column view controller (AppKit).
- SwiftUI `NavigationSplitView` — multi-column (AppKit + Mac Catalyst).
- `NSWindow` + `NSWindow.styleMask` — full-screen, miniaturize, zoom.

### Default anatomy

- macOS does NOT use the size class model. macOS uses window resize as the structural modifier.
- The Skill recommends a 2- or 3-column anatomy in the main window, with optional inspector on-demand.
- The `Settings` window is a separate window; the Skill treats it as a structural surface.

### Anti-patterns

- **Re-implementing window chrome** — drawing custom close / minimize / zoom buttons. Rejected. Use the system chrome.
- **Forced multi-column** — pinning the sidebar visible at narrow window widths. Rejected. The Skill uses the system split view controller to handle resize.
- **iPad anatomy on Mac** — using `NavigationStack` for primary navigation when a `WindowGroup` is appropriate. The Skill recommends the Mac-native primitive.

## visionOS (visionOS)

### Structural primitives

- `WindowGroup` — standard 2D window.
- `.volumetric` WindowGroup — a 3D volume.
- `immersiveSpace` — full-immersion scene.
- `.ornament` — a 2D chrome element attached to a 3D scene.
- `RealityView` — a 3D content view.
- `Attachment` — content shared between a 3D scene and a 2D window.

visionOS structural design uses the publicly documented `Window` / `Volume` / `Space` building blocks. **There is no public `SpatialLayout` API** in the installed visionOS SDK; the Skill does NOT cite one.

### Default anatomy

- visionOS does NOT use the size class model. The structural primitives are spatial: a 2D window has a 2D layout (1-column typically); a 3D scene has spatial arrangement.
- The Skill records visionOS as a separate structural family. The Skill does NOT claim iPad-size-class behavior on visionOS.

### Anti-patterns

- **iPad anatomy on visionOS** — applying iPad multi-column to a visionOS window. Rejected. The visionOS window is typically 1-column (focus-driven).
- **iPhone anatomy on visionOS** — collapsing all chrome to a tab bar. Rejected. The visionOS chrome is `.ornament`-driven.

## tvOS (tvOS)

### Structural primitives

- `UIFocusSystem` (UIKit, iOS 11+, tvOS 11+) — focus management. The public Apple focus API. **There is no public `UIFocusEngine` class** in any installed Apple SDK; that name refers to a private symbol. Use `UIFocusSystem` and the SwiftUI focus-driven APIs.
- SwiftUI `.focusable` + `@FocusState` — focus binding.
- `NavigationStack` (focus-driven).
- `TabView` (focus-driven).
- `UICollectionView` (UIKit) — focus-driven collection view.

### Default anatomy

- tvOS does NOT use the size class model. tvOS is focus-driven: the user moves a focus halo across the screen, and the focused surface is the primary surface.
- The Skill records tvOS as a separate structural family. The Skill does NOT claim iPad-size-class behavior on tvOS.

### Anti-patterns

- **iPad anatomy on tvOS** — applying iPad multi-column. Rejected. tvOS is focus-driven.
- **Touch-driven affordances on tvOS** — drawing a "tap here" surface. Rejected. tvOS uses a remote / focus halo, not touch.

## watchOS (watchOS)

### Structural primitives

- `NavigationStack` (vertical stack-driven).
- `TabView` (vertical tab bar — Smart Stack).
- Digital Crown — scroll / navigate.

### Default anatomy

- watchOS does NOT use the size class model. watchOS is stack-driven: a single vertical stack of items, with drill-in via the crown.
- The Skill records watchOS as a separate structural family. The Skill does NOT claim iPad-size-class behavior on watchOS.

### Anti-patterns

- **3-column on watchOS** — forcing 3 columns on a 40 mm watch. Rejected.
- **Sidebar on watchOS** — exposing a sidebar on a 40 mm watch. Rejected. Smart Stack is the canonical chrome.

## Web (Desktop / Touch / PWA)

### Structural primitives

- CSS Grid (multi-column / responsive grid).
- CSS Flexbox (single-row or single-column layout).
- CSS Container Queries (component-level adaptivity).
- CSS Media Queries (page-level adaptivity, prefers-color-scheme, prefers-reduced-motion, prefers-reduced-transparency).
- View Transitions API (in-page animated transitions).
- Popover API (non-modal auxiliary surfaces).
- CSS Anchor Positioning (popover / menu anchoring).
- CSS Subgrid (nested grid alignment).

### Default anatomy

The Skill does NOT publish a single "Web anatomy"; the Web is platform-of-platforms (Desktop, Touch, PWA, Hybrid). The Skill publishes anatomy per (form factor, display mode):

| Form factor | Display mode | Anatomy |
|---|---|---|
| Desktop Web | browser | 2- or 3-column. Sidebar + content + optional inspector. |
| Desktop Web | fullscreen (PWA) | Same as browser. |
| Touch Web | browser | Card stack + on-demand secondary surfaces. |
| Touch Web | standalone (PWA) | Same as Touch Web. |
| Hybrid | browser | Best of both — sidebar visible, content scrolls, secondary surfaces on-demand. |

### Anti-patterns

- **Re-implementing platform chrome on Web** — drawing custom tab bars, custom toolbars, custom window chrome. The Skill recommends the platform chrome where possible; the Web is the limit case.
- **Forced 3-column on mobile Web** — pinning the sidebar + content + detail at 360 px. Rejected. The Skill uses container queries + viewport media queries to reflow.
- **JavaScript-driven layout** — using JS for layout work that CSS Grid + Container Queries can do declaratively. Rejected on Web. JS is for state, not layout.

## What the Skill does NOT do

- Does NOT publish a "best anatomy" per device. The choice is the app's, guided by the Skill's invariants.
- Does NOT recommend iPad anatomy on watchOS / tvOS. Each is a separate structural family.
- Does NOT recommend re-implementing system primitives. The Skill routes to the system primitive.
- Does NOT recommend a fixed "Web anatomy". The Web is platform-of-platforms; the Skill publishes per-form-factor anatomy.
