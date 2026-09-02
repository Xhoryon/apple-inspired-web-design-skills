---
name: apple-ui-components
description: Use when designing or implementing UI components in an Apple-inspired web build — navigation, toolbar, buttons, segmented control, search, tabs, media controls, sheet, popover, modal, list, settings row, card, image container, form. Defines component grammar (when to use what, hierarchy between components, when to stay native/simple) and the explicit anti-patterns (pill-everything, card-everything, glass-everything, icon-bubble-everything, eyebrow-everything). Does NOT prescribe a fixed component library — it tells the agent what to build and what NOT to do. Always paired with apple-design-foundations.
version: "1.0.0"
license: MIT
metadata:
  hermes:
    tags: [design, apple, components, ui]
---

# apple-ui-components

A component is not a visual style — it is a **role with a job**. This skill defines the role, the job, the visual restraint, and the explicit anti-patterns.

## Component hierarchy

Apple-inspired UIs have a clear hierarchy of components. Mixing roles creates visual noise.

```
Floating navigation layer  (rare, 1 per page max, often glass)
   ├── Toolbar / segmented / search / media controls
   └── Sheet / popover / modal / drawer
Content layer
   ├── Section header + content
   ├── Card / panel / image container
   ├── List / settings row / table
   ├── Form / input
   └── Footer
```

The floating navigation layer is **opt-in** and **separated** from content. The content layer is the bulk of the page and stays calm.

## When to use what (decision table)

| Need | Use | Don't use |
|---|---|---|
| Primary page navigation | Top navbar or left rail (desktop), bottom tab bar (mobile) | Floating glass navbar on every page |
| Within-page switch (3–6 options) | Segmented control | Tab bar of 6+ items |
| Within-page switch (2 options) | Toggle / switch | Two buttons |
| Within-page switch (3+ options, complex) | Dropdown / menu | Segmented control of 8 items |
| Trigger a contextual action panel | Popover / menu | Sheet |
| Trigger a focused task | Sheet | Popover |
| Critical blocking decision | Modal | Sheet (sheet is dismissible) |
| Brief confirmation | Toast / inline | Modal |
| Single image with caption | Image container (no card) | Card with image |
| Image grid (3–8) | Edge-to-edge grid | Stack of 8 cards |
| Form input | Inline field with label | Card-wrapped input |
| Settings row | List row with chevron / toggle | Card with settings |
| Marketing CTA | Pill button or text link | Three buttons competing |
| Quick action | Icon button (round) | Pill button with icon |
| Filter / tag | Chip or pill (when genuinely a filter) | Pill on every noun |

## Navigation

### Top navbar

- Logo (text or icon, your brand, not Apple's) on the leading edge.
- Nav links inline, regular weight. Hover: subtle color shift, no underline animation.
- Search icon, account icon, cart icon on the trailing edge.
- Height: 44 px desktop, 48 px tablet, 44 px on mobile (touch).
- Background: solid by default. Translucent / glass only if there's content scrolling beneath.
- Sticky / floating navbar: inset 12–24 px from viewport edges, larger radius (16–24 px), with a hairline border.

### Bottom tab bar (mobile)

- 3–5 destinations. Not 7.
- Icon + optional label. Use a label for clarity.
- Translucent background is appropriate (interaction layer over content).
- Height: 49 px + safe area.
- Active state: filled icon or color shift on icon, optional small dot.

### Sidebar (desktop apps)

- 240–280 px wide, full-height.
- Sections grouped, headers in caps.
- Selected row: tinted background or accent leading bar.
- Collapsible to icon rail.

## Buttons

### Primary button

- Solid fill in the **accent color**.
- 8–14 px radius (not always 24). Pills are allowed when visual rhythm demands them but should not dominate.
- Padding: 12 px vertical × 22 px horizontal desktop; 14 × 24 on mobile.
- Font: body weight, slight letter-spacing.
- Hover: subtle elevation / brightness shift, not color shift.
- Press: scale 0.97–0.98, spring back.
- Focus: visible focus ring at 2 px, accent or contrast color.
- Min tap target: 44×44 px.

### Secondary button

- Outline or tinted background.
- Same height and padding as primary.
- Visually quieter than primary. Never the same weight.

### Tertiary / text link

- Color: accent or primary text color.
- No background.
- Hover: underline or color shift.
- Use for low-priority actions.

### Icon button

- Square or circle, 36–44 px.
- Icon centered.
- Subtle background on hover (8% overlay), or no background.
- Aria-label required.

### Anti-pattern: pill-everything

> "All buttons are pill" kills hierarchy. Use the radius that fits the form. Pill is for special rhythm (e.g. CTA in hero) — not the default.

### Anti-pattern: icon-bubble-everything

> Putting every icon in a circular tinted background makes the page look like a control panel. Icon bubbles are for primary icon buttons, not decoration.

## Segmented control

- 2–5 segments. Not 8.
- Equal-width segments preferred. Variable-width is OK with restraint.
- Selected segment: pill-shaped fill that morphs between positions.
- Spring animation (240–360 ms).
- Labels or icons. Mixed (icon + label) is OK if consistent.

## Search

- Collapsed: icon button or compact pill.
- Expanded: full-width input, often translucent.
- Magnifier, optional clear button, optional scope dropdown.
- Press feedback on icon button.

## Tabs

- Underline indicator that morphs width and position between tabs.
- Use only for in-page content switching, not navigation.
- Don't use tabs where a segmented control fits better.

## Media controls

- Floating overlay (translucent is appropriate).
- Play / pause / scrub / volume / fullscreen.
- Auto-hide on idle after 2–3 s.
- Larger controls on mobile, smaller on desktop.

## Sheet / drawer

- Slides from bottom (mobile) or side (desktop).
- Drag-to-dismiss on mobile.
- Detents (small / medium / large) where useful.
- Spring open (360–480 ms), ease-in-quint close (240–320 ms).
- Background scrim is dimmed, not blurred (blur is reserved for the surface itself).

## Popover

- Anchored to the trigger element.
- Grows from the trigger (transform-origin on the anchor).
- Arrow pointing to the trigger is OK but optional.
- Closes on outside click or Escape.
- Spring animation (220–320 ms).

## Modal

- Centered (desktop) or full-screen (mobile).
- Scrim: dimming background, not heavy blur.
- Focus is trapped. Escape closes.
- Use sparingly — only for blocking decisions.

## Lists

### Settings row (iOS settings style)

- Full-width row, 44–48 px tall.
- Leading icon (optional), title, trailing accessory (chevron, toggle, value).
- Grouped rows in a card-like container with 8–12 px inner radius.
- Separators are optional; if used, they're hairline and inset to the leading icon edge.

### Content list

- Generous row height (60–80 px).
- Leading thumbnail (if applicable), title, optional subtitle, trailing action.
- No heavy backgrounds per row.

### Table

- Reserved for data display.
- Sticky header. Hairline row separators.
- Right-align numerics.
- No card per row.

## Cards

A card is **a content container with internal padding**, not decoration.

- Use cards for: grouped related content, image + caption blocks, distinct product tiles.
- Don't use cards for: every paragraph, every feature, every icon-with-title-text.
- Don't apply glass to every card. Most cards are solid surface.
- Card radius: 16–22 px.
- Inner radius (for nested cards): 12–16 px.
- Card padding: 20–28 px desktop, 16–20 px mobile.

### Anti-pattern: card-everything

> "Title + 3 cards" stacked down the page is the SaaS template. Break it up with image-led sections, full-bleed, or asymmetric layouts (see apple-web-composition).

## Image containers

- Edge-to-edge when the image is the message.
- Caption bar (translucent) only when overlaying a busy image.
- Don't wrap every image in a card with rounded corners and shadow.

## Forms

- Labels above fields, not placeholders as labels.
- Single-column layout preferred. Two columns only for short related pairs (city/zip).
- Field height: 44 px desktop, 48 px on mobile.
- Field radius: 8–12 px.
- Field border: 1 px subtle, 2 px on focus with accent color.
- Inline validation, not a giant error card.
- Helper text below the field.

## Anti-patterns (component-specific)

- **Pill-everything.** All buttons pill-shaped → no hierarchy.
- **Card-everything.** Every block wrapped in a card → no rhythm.
- **Glass-everything.** Every surface translucent → no separation.
- **Icon-bubble-everything.** Every icon in a tinted circle → control-panel feel.
- **Eyebrow-everywhere.** Small uppercase tag above every title → template feel.
- **Badge spam.** "New", "Beta", "Pro" on every other item → noise.
- **Three CTAs in the hero.** Hierarchy collapses.
- **Dropdown menus 6 levels deep.** Use navigation, not mega-menus.

## Hard rules

- The floating navigation layer is **opt-in** and **separated** from content.
- Buttons have hierarchy: primary (one per region), secondary, tertiary.
- Pill is **not** the default radius.
- Cards are **content containers**, not decoration.
- Glass belongs to interaction surfaces (see apple-liquid-glass-web).
- Icon buttons need an aria-label.
- Tap targets ≥ 44×44 px on mobile.
- Forms use labels above fields; placeholders are not labels.

## Review checklist

Before declaring components done:

- [ ] One primary CTA per region (not three competing).
- [ ] Button radius is varied (not all pill).
- [ ] Cards are used for content, not decoration.
- [ ] Glass is only on interaction surfaces (see apple-liquid-glass-web).
- [ ] No icon-bubble on every icon.
- [ ] No eyebrow tag on every title.
- [ ] Mobile bottom tab bar has 3–5 destinations.
- [ ] Segmented control has 2–5 segments.
- [ ] All icon buttons have aria-labels.
- [ ] All tap targets ≥ 44×44 px.
- [ ] Form labels are above fields.
- [ ] Focus rings visible on all interactive elements.

## Common mistakes

- Three buttons competing in the hero.
- Every block wrapped in a card with glass.
- All buttons pill-shaped.
- All icons in tinted circles.
- Eyebrow uppercase label above every headline.
- Settings list as a stack of cards.
- Form inputs as labeled-card blocks instead of inline fields.

## Companion references

- `references/navigation.md` — top navbar, bottom tab bar, sidebar patterns.
- `references/buttons.md` — primary, secondary, tertiary, icon, anti-patterns.
- `references/overlays.md` — sheet, popover, modal, drawer.
- `references/forms.md` — input, select, validation, accessibility.
- `examples/button-system.css` — drop-in button styles.
- `examples/navbar.css` — top navbar with optional glass surface.

This skill defines the parts. apple-web-composition decides where they go. apple-motion-interaction decides how they move.
