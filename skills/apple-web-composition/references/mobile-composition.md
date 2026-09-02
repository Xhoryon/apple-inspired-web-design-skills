# Mobile Composition Reference

Mobile is not a shrunken desktop page. It is a redesign.

## Core differences from desktop

- Vertical stacking is the default. Multi-column layouts collapse to single column or 2-up.
- Tap targets ≥ 44×44 px.
- Body text never below 16 px (Apple keeps 17 px on phones).
- Bottom tab bar replaces complex desktop navigation.
- Sheets replace modal dialogs.
- Edge-to-edge media becomes fullscreen.
- Section padding reduces 30–40%.
- Floating surfaces sit closer to the bottom edge with safe-area insets.

## Hero on mobile

```
[ Nav (compact, sometimes hidden) ]
[ Hero — image first OR text first ]
[ Headline (≥ 48 px) ]
[ Supporting line ]
[ Primary CTA ]
[ Secondary CTA (optional, smaller) ]
```

Decide image-first or text-first per page:

- **Image-first** — when the product is the message. Photo leads, type follows.
- **Text-first** — when the headline is the message. Headline leads, image follows (or is inline).

Apple often uses image-first for product launches and text-first for category pages.

## Stacking rules

- Cards in a 3-column row on desktop become a single column on mobile, or a 2-up at most.
- Cards in a 4-column row become 2-up.
- Asymmetric splits (60/40) become stacked.
- Multi-step flows become fullscreen sheets.

## Tap targets

- Buttons: 44 px height minimum, often 48 px.
- Icon buttons: 44×44 px hit area, even if icon is smaller.
- List rows: 44–48 px tall, full-width.
- Form inputs: 48 px tall.
- Spacing between tappable elements: ≥ 8 px.

```css
@media (max-width: 768px) {
  .button { min-height: 48px; padding: 14px 24px; }
  .icon-button { width: 44px; height: 44px; }
  .input { min-height: 48px; }
}
```

## Bottom tab bar

3–5 destinations. Not 7. Icons + optional labels.

```css
.bottom-tab-bar {
  position: fixed;
  inset: auto 0 0 0;
  padding-bottom: env(safe-area-inset-bottom);
  height: calc(49px + env(safe-area-inset-bottom));
  background: rgba(255, 255, 255, 0.85);
  backdrop-filter: saturate(180%) blur(20px);
  border-top: 1px solid rgba(0, 0, 0, 0.06);
  display: flex;
  z-index: 50;
}

.bottom-tab-bar .tab {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 2px;
  color: var(--text-secondary);
}

.bottom-tab-bar .tab.is-active { color: var(--accent); }
```

## Floating elements on mobile

Floating surfaces sit closer to the bottom edge with safe-area insets:

```css
@media (max-width: 768px) {
  .floating-toolbar {
    inset: auto 12px calc(12px + env(safe-area-inset-bottom)) 12px;
  }
}
```

Floating navbars at the top on mobile are usually replaced by either:

- A top bar that's part of the page (not floating), or
- A bottom tab bar, or
- A hamburger / drawer pattern.

## Sheet instead of modal

Desktop modals become fullscreen sheets on mobile. They slide up from the bottom and have a drag handle.

## Section padding

```css
:root {
  --section-pad-block: clamp(56px, 8vw, 96px);
}

@media (max-width: 768px) {
  :root {
    --section-pad-block: clamp(48px, 12vw, 72px);
  }
}
```

Mobile reduces by ~30–40%.

## Typography on mobile

- Body 16–17 px, never below 16 px.
- Display scales down but stays ≥ 40 px.
- Line-height slightly looser for body (1.5 → 1.55).
- Letter-spacing slightly tighter on display.

```css
:root {
  --fs-display-xl: clamp(2.5rem, 9vw, 6rem);
  --fs-body: 1.0625rem;  /* 17 px */
}
```

## Performance on mobile

Mobile is the harder test. Always:

- Capture mobile screenshots.
- Run Lighthouse on mobile profile.
- Verify `prefers-reduced-motion` on mobile.
- Test on a low-end mobile profile (e.g. mid-tier Android).

## Common mobile mistakes

- Shrunken desktop layout (tiny text, horizontal scroll).
- Tap targets below 44 px.
- Body text below 16 px.
- Floating navbar at the top instead of bottom tab bar.
- Modal instead of sheet.
- Horizontal scroll on any element.
- Section padding identical to desktop.
- No safe-area insets on iPhone notch.

## Accessibility

- Touch targets ≥ 44×44 px regardless of visual size.
- Color contrast for body text on mobile.
- `prefers-reduced-motion` respected.
- Orientation: design for both portrait and landscape, but portrait is primary.
- Avoid hover-only interactions. Tap must work.
