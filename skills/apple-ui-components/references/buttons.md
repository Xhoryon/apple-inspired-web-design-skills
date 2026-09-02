# Button System Reference

Extended recipes for Apple-inspired buttons. Builds on `apple-ui-components/SKILL.md`.

## Hierarchy

There are three button levels: primary, secondary, tertiary. Plus icon buttons. **One primary CTA per region** is the rule.

## Primary button

```html
<button class="button button--primary">Buy now</button>
```

```css
.button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  padding: 12px 22px;
  min-height: 44px;
  font-size: 15px;
  font-weight: 500;
  letter-spacing: -0.005em;
  border: 0;
  border-radius: 12px;
  cursor: pointer;
  transition:
    transform 120ms var(--ease-spring),
    background-color 120ms ease-out,
    box-shadow 120ms ease-out;
  user-select: none;
}

.button--primary {
  background: var(--accent);
  color: var(--text-on-accent);
}

.button--primary:hover {
  background: var(--accent-hover);
}

.button--primary:active {
  transform: scale(0.97);
}

.button:focus-visible {
  outline: 2px solid var(--accent);
  outline-offset: 2px;
}
```

## Secondary button

```css
.button--secondary {
  background: transparent;
  color: var(--accent);
  border: 1px solid var(--accent);
}

.button--secondary:hover {
  background: rgba(0, 102, 204, 0.08);
}

.button--secondary:active {
  transform: scale(0.97);
}
```

Or as a tinted button (less emphasis than primary, more than tertiary):

```css
.button--tinted {
  background: rgba(0, 102, 204, 0.10);
  color: var(--accent);
}

.button--tinted:hover {
  background: rgba(0, 102, 204, 0.15);
}
```

## Tertiary / text link

```html
<button class="button button--tertiary">Learn more</button>
```

```css
.button--tertiary {
  background: transparent;
  color: var(--accent);
  padding: 12px 8px;
}

.button--tertiary:hover {
  text-decoration: underline;
}
```

## Pill variant (rhythm choice)

Pill buttons (`border-radius: 9999px`) are allowed for hero CTAs and special rhythm. Not the default.

```css
.button--pill {
  border-radius: 9999px;
  padding: 14px 28px;
}
```

## Icon button

```html
<button class="icon-button" aria-label="Search">
  <svg viewBox="0 0 24 24" width="20" height="20">...</svg>
</button>
```

```css
.icon-button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 44px;
  height: 44px;
  padding: 0;
  background: transparent;
  color: var(--text-primary);
  border: 0;
  border-radius: 50%;
  cursor: pointer;
  transition: background-color 120ms ease-out;
}

.icon-button:hover {
  background: rgba(0, 0, 0, 0.06);
}

.icon-button:active {
  background: rgba(0, 0, 0, 0.10);
  transform: scale(0.95);
}

.icon-button:focus-visible {
  outline: 2px solid var(--accent);
  outline-offset: 2px;
}
```

## Icon button with bubble background

Allowed for primary icon buttons (FAB-style). Not for every icon.

```css
.icon-button--bubble {
  background: rgba(0, 102, 204, 0.10);
  color: var(--accent);
}

.icon-button--bubble:hover {
  background: rgba(0, 102, 204, 0.18);
}
```

## Button group

When two buttons sit together (primary + secondary):

```html
<div class="button-group">
  <button class="button button--primary">Buy</button>
  <button class="button button--secondary">Learn more</button>
</div>
```

```css
.button-group {
  display: inline-flex;
  gap: 12px;
  align-items: center;
  flex-wrap: wrap;
}
```

## Disabled state

```css
.button:disabled,
.button[aria-disabled="true"] {
  opacity: 0.4;
  cursor: not-allowed;
  pointer-events: none;
}
```

Don't remove the button from the layout. Keep it visible but disabled.

## Loading state

```css
.button--loading {
  position: relative;
  color: transparent;
  pointer-events: none;
}

.button--loading::after {
  content: "";
  position: absolute;
  width: 16px;
  height: 16px;
  border: 2px solid currentColor;
  border-top-color: transparent;
  border-radius: 50%;
  animation: spin 800ms linear infinite;
  color: white;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}
```

## Mobile sizing

```css
@media (max-width: 768px) {
  .button {
    min-height: 48px;
    padding: 14px 24px;
    font-size: 16px;
  }
}
```

## Common mistakes

- All buttons pill-shaped → no hierarchy.
- Three buttons competing in the hero.
- Icon button without aria-label.
- Tap target below 44 px.
- Hover changes the entire color (use brightness / background tint instead).
- Press feedback missing — buttons feel dead.
- Loading state without `aria-busy="true"`.
