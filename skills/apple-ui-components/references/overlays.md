# Overlays Reference — Sheet, Popover, Modal, Drawer

Extended recipes for Apple-inspired overlay components. Builds on `apple-ui-components/SKILL.md`.

## When to use what

| Need | Component | Notes |
|------|-----------|-------|
| Contextual action panel | Popover | Anchored to trigger, dismissible |
| Focused task (form, settings) | Sheet | Mobile: bottom sheet, desktop: side sheet |
| Critical blocking decision | Modal | Centered, focus trapped, only when truly blocking |
| Side navigation on mobile | Drawer | Slide from left |
| Confirm action briefly | Toast / inline | Auto-dismiss |

## Sheet (mobile and desktop)

```html
<div class="sheet" role="dialog" aria-modal="true" aria-labelledby="sheet-title">
  <div class="sheet__scrim"></div>
  <div class="sheet__panel">
    <header class="sheet__handle"></header>
    <h2 id="sheet-title" class="sheet__title">Edit profile</h2>
    <div class="sheet__body">
      <!-- form or content -->
    </div>
    <footer class="sheet__footer">
      <button class="button button--tertiary">Cancel</button>
      <button class="button button--primary">Save</button>
    </footer>
  </div>
</div>
```

```css
.sheet {
  position: fixed;
  inset: 0;
  z-index: 60;
  display: grid;
  place-items: end center;
}

.sheet__scrim {
  position: absolute;
  inset: 0;
  background: rgba(0, 0, 0, 0.40);
  animation: fadeIn 200ms ease-out;
}

.sheet__panel {
  position: relative;
  width: 100%;
  max-width: 480px;
  max-height: 90vh;
  background: var(--bg-page);
  border-radius: 24px 24px 0 0;
  padding: 8px 16px calc(16px + env(safe-area-inset-bottom));
  overflow-y: auto;
  animation: slideUp 360ms var(--ease-spring);
}

@media (min-width: 768px) {
  .sheet { place-items: center; }
  .sheet__panel {
    border-radius: 24px;
    max-width: 480px;
  }
}

@keyframes slideUp {
  from { transform: translateY(100%); }
  to   { transform: translateY(0); }
}

@keyframes fadeIn {
  from { opacity: 0; }
  to   { opacity: 1; }
}

.sheet__handle {
  width: 36px;
  height: 5px;
  margin: 8px auto 16px;
  background: rgba(0, 0, 0, 0.20);
  border-radius: 9999px;
}

.sheet__title {
  font-size: 17px;
  font-weight: 600;
  margin: 0 0 16px;
}

.sheet__footer {
  display: flex;
  justify-content: flex-end;
  gap: 12px;
  margin-top: 24px;
}
```

### Sheet with detents

```js
const sheet = document.querySelector(".sheet__panel");
let startY = 0;
let currentY = 0;
const detents = [0.1, 0.5, 1.0]; // small, medium, large as fraction of viewport

sheet.addEventListener("touchstart", (e) => {
  startY = e.touches[0].clientY;
});

sheet.addEventListener("touchmove", (e) => {
  currentY = e.touches[0].clientY;
  const dy = Math.max(0, currentY - startY);
  sheet.style.transform = `translateY(${dy}px)`;
});

sheet.addEventListener("touchend", () => {
  const dy = currentY - startY;
  const fraction = dy / window.innerHeight;

  if (fraction > 0.3) {
    sheet.style.transform = "translateY(100%)";
    setTimeout(() => sheet.closest(".sheet").remove(), 280);
  } else {
    sheet.style.transform = "translateY(0)";
  }
  startY = 0;
  currentY = 0;
});
```

## Popover

```html
<button class="trigger" aria-haspopup="dialog" aria-expanded="false">
  Options
</button>

<div class="popover" role="dialog" hidden>
  <button class="popover__item">Edit</button>
  <button class="popover__item">Duplicate</button>
  <button class="popover__item">Delete</button>
</div>
```

```css
.popover {
  position: absolute;
  min-width: 200px;
  padding: 6px;
  background: var(--bg-elevated);
  border: 1px solid var(--border-hairline);
  border-radius: 12px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.12);
  z-index: 60;
  transform-origin: var(--origin-x, 0) var(--origin-y, 0);
  animation: popIn 240ms var(--ease-spring);
}

.popover[hidden] { display: none; }

@keyframes popIn {
  from { opacity: 0; transform: scale(0.92); }
  to   { opacity: 1; transform: scale(1); }
}

.popover__item {
  display: block;
  width: 100%;
  padding: 8px 12px;
  text-align: left;
  background: transparent;
  border: 0;
  border-radius: 6px;
  font-size: 14px;
  color: var(--text-primary);
  cursor: pointer;
}

.popover__item:hover {
  background: rgba(0, 0, 0, 0.04);
}
```

Wire up via JS:

```js
const trigger = document.querySelector(".trigger");
const popover = document.querySelector(".popover");

const open = () => {
  const rect = trigger.getBoundingClientRect();
  popover.style.top = `${rect.bottom + 8}px`;
  popover.style.left = `${rect.left}px`;
  popover.style.setProperty("--origin-x", "0");
  popover.style.setProperty("--origin-y", "0");
  popover.hidden = false;
  trigger.setAttribute("aria-expanded", "true");
};

const close = () => {
  popover.hidden = true;
  trigger.setAttribute("aria-expanded", "false");
};

trigger.addEventListener("click", () => {
  if (popover.hidden) open();
  else close();
});

document.addEventListener("click", (e) => {
  if (!popover.contains(e.target) && e.target !== trigger) close();
});

document.addEventListener("keydown", (e) => {
  if (e.key === "Escape") close();
});
```

## Modal

Use only for critical blocking decisions. Most cases should use a sheet or popover.

```html
<div class="modal" role="alertdialog" aria-modal="true" aria-labelledby="modal-title">
  <div class="modal__scrim"></div>
  <div class="modal__panel">
    <h2 id="modal-title">Delete this item?</h2>
    <p>This action cannot be undone.</p>
    <div class="modal__actions">
      <button class="button button--tertiary">Cancel</button>
      <button class="button button--primary">Delete</button>
    </div>
  </div>
</div>
```

```css
.modal {
  position: fixed;
  inset: 0;
  z-index: 70;
  display: grid;
  place-items: center;
  padding: 16px;
}

.modal__scrim {
  position: absolute;
  inset: 0;
  background: rgba(0, 0, 0, 0.40);
  animation: fadeIn 200ms ease-out;
}

.modal__panel {
  position: relative;
  width: 100%;
  max-width: 400px;
  background: var(--bg-elevated);
  border-radius: 16px;
  padding: 20px;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.30);
  animation: popIn 240ms var(--ease-spring);
}

.modal__actions {
  display: flex;
  justify-content: flex-end;
  gap: 8px;
  margin-top: 16px;
}
```

### Focus trap

```js
const modal = document.querySelector(".modal__panel");
const focusable = modal.querySelectorAll(
  'a, button, input, select, textarea, [tabindex]:not([tabindex="-1"])'
);
const first = focusable[0];
const last = focusable[focusable.length - 1];

modal.addEventListener("keydown", (e) => {
  if (e.key !== "Tab") return;
  if (e.shiftKey && document.activeElement === first) {
    e.preventDefault();
    last.focus();
  } else if (!e.shiftKey && document.activeElement === last) {
    e.preventDefault();
    first.focus();
  }
});

first.focus();
```

## Drawer (mobile side nav)

```html
<button class="menu-trigger" aria-controls="drawer" aria-expanded="false">Menu</button>

<div id="drawer" class="drawer" hidden>
  <div class="drawer__scrim"></div>
  <aside class="drawer__panel" role="dialog" aria-label="Navigation">
    <nav>
      <a href="/">Home</a>
      <a href="/products">Products</a>
      <a href="/about">About</a>
      <a href="/support">Support</a>
    </nav>
  </aside>
</div>
```

```css
.drawer { position: fixed; inset: 0; z-index: 60; }
.drawer[hidden] { display: none; }

.drawer__scrim {
  position: absolute;
  inset: 0;
  background: rgba(0, 0, 0, 0.40);
  animation: fadeIn 200ms ease-out;
}

.drawer__panel {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  width: min(320px, 80vw);
  background: var(--bg-page);
  padding: 16px;
  overflow-y: auto;
  animation: slideInLeft 280ms var(--ease-spring);
}

@keyframes slideInLeft {
  from { transform: translateX(-100%); }
  to   { transform: translateX(0); }
}

.drawer__panel nav { display: flex; flex-direction: column; gap: 4px; }
.drawer__panel a {
  padding: 12px;
  border-radius: 8px;
  color: var(--text-primary);
  text-decoration: none;
}
.drawer__panel a:hover { background: rgba(0, 0, 0, 0.04); }
```

## Toast

```html
<div class="toast" role="status" aria-live="polite">
  Saved.
</div>
```

```css
.toast {
  position: fixed;
  bottom: calc(24px + env(safe-area-inset-bottom));
  left: 50%;
  transform: translateX(-50%);
  padding: 12px 20px;
  background: rgba(28, 28, 30, 0.92);
  color: white;
  border-radius: 9999px;
  font-size: 14px;
  z-index: 80;
  animation: toastIn 280ms var(--ease-spring);
}

@keyframes toastIn {
  from { opacity: 0; transform: translate(-50%, 20px); }
  to   { opacity: 1; transform: translate(-50%, 0); }
}
```

## Accessibility

- All overlays have `role="dialog"` or `role="alertdialog"` + `aria-modal="true"` + `aria-labelledby`.
- Focus is trapped inside modals.
- Escape closes the overlay.
- Click outside closes the overlay (except for alertdialogs).
- `prefers-reduced-motion: reduce` removes the slide/scale animation; content snaps in.
- Focus returns to the trigger when the overlay closes.

## Common mistakes

- Using a modal for non-critical actions (use sheet or popover).
- Sheet that doesn't slide from the bottom on mobile.
- Popover that grows from center-screen instead of from the trigger.
- Modal without focus trap.
- Overlay that doesn't close on Escape.
- Backdrop scrim with heavy blur (blur is for the surface, not the backdrop).
- Animation > 500 ms on overlay open/close.
