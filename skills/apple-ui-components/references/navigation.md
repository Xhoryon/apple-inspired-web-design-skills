# Navigation Reference

Extended recipes for Apple-inspired navigation components. Builds on `apple-ui-components/SKILL.md`.

## Top navbar

```html
<nav class="navbar">
  <a href="/" class="navbar__logo">Brand</a>
  <ul class="navbar__links">
    <li><a href="/products">Products</a></li>
    <li><a href="/about">About</a></li>
    <li><a href="/support">Support</a></li>
  </ul>
  <div class="navbar__trailing">
    <button class="icon-button" aria-label="Search">
      <svg>...</svg>
    </button>
    <button class="icon-button" aria-label="Account">
      <svg>...</svg>
    </button>
  </div>
</nav>
```

```css
.navbar {
  position: sticky;
  top: 0;
  height: 48px;
  padding-inline: clamp(20px, 4vw, 48px);
  display: flex;
  align-items: center;
  gap: 24px;
  background: rgba(251, 251, 253, 0.85);
  backdrop-filter: saturate(180%) blur(20px);
  -webkit-backdrop-filter: saturate(180%) blur(20px);
  border-bottom: 1px solid rgba(0, 0, 0, 0.06);
  z-index: 50;
}

.navbar__logo {
  font-weight: 600;
  font-size: 17px;
  color: var(--text-primary);
}

.navbar__links {
  display: flex;
  gap: 24px;
  list-style: none;
  padding: 0;
  margin: 0;
}

.navbar__links a {
  color: var(--text-primary);
  text-decoration: none;
  font-size: 14px;
  transition: opacity 120ms ease-out;
}

.navbar__links a:hover { opacity: 0.7; }
.navbar__links a:focus-visible {
  outline: 2px solid var(--accent);
  outline-offset: 2px;
  border-radius: 4px;
}

.navbar__trailing {
  margin-left: auto;
  display: flex;
  gap: 8px;
}
```

### Floating navbar variant

```css
.navbar--floating {
  position: fixed;
  inset: 12px 12px auto 12px;
  height: 48px;
  border-radius: 18px;
  background: rgba(255, 255, 255, 0.55);
  border: 1px solid rgba(255, 255, 255, 0.4);
  box-shadow:
    inset 0 1px 0 rgba(255, 255, 255, 0.5),
    0 10px 30px rgba(0, 0, 0, 0.10);
}
```

### Mobile navbar

On mobile, the top navbar collapses:

- Logo on the left.
- Hamburger / drawer trigger on the right.
- Or a bottom tab bar (see below).

```css
@media (max-width: 768px) {
  .navbar__links { display: none; }
  .navbar__menu-trigger { display: block; }
}
```

## Bottom tab bar (mobile)

```html
<nav class="tab-bar" aria-label="Primary">
  <a href="/" class="tab-bar__tab is-active">
    <svg>...</svg>
    <span>Home</span>
  </a>
  <a href="/search" class="tab-bar__tab">
    <svg>...</svg>
    <span>Search</span>
  </a>
  <a href="/account" class="tab-bar__tab">
    <svg>...</svg>
    <span>Account</span>
  </a>
</nav>
```

```css
.tab-bar {
  position: fixed;
  inset: auto 0 0 0;
  height: calc(49px + env(safe-area-inset-bottom));
  padding-bottom: env(safe-area-inset-bottom);
  display: flex;
  background: rgba(251, 251, 253, 0.85);
  backdrop-filter: saturate(180%) blur(20px);
  -webkit-backdrop-filter: saturate(180%) blur(20px);
  border-top: 1px solid rgba(0, 0, 0, 0.06);
  z-index: 50;
}

.tab-bar__tab {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 2px;
  color: var(--text-secondary);
  text-decoration: none;
  font-size: 10px;
  font-weight: 500;
}

.tab-bar__tab svg { width: 24px; height: 24px; }
.tab-bar__tab.is-active { color: var(--accent); }
```

## Sidebar (desktop)

```css
.sidebar {
  width: 240px;
  height: 100vh;
  padding: 16px 8px;
  background: var(--bg-subtle);
  border-right: 1px solid var(--border-hairline);
  overflow-y: auto;
}

.sidebar__section {
  margin-bottom: 16px;
}

.sidebar__heading {
  padding: 8px 12px;
  font-size: 12px;
  font-weight: 500;
  letter-spacing: 0.04em;
  text-transform: uppercase;
  color: var(--text-tertiary);
}

.sidebar__item {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 8px 12px;
  border-radius: 8px;
  color: var(--text-primary);
  text-decoration: none;
  font-size: 14px;
}

.sidebar__item:hover { background: rgba(0, 0, 0, 0.04); }
.sidebar__item.is-active {
  background: var(--accent-tint);
  color: var(--accent);
}
```

## Drawer / hamburger menu

```html
<button class="menu-trigger" aria-expanded="false" aria-controls="drawer">
  <svg>...</svg>
</button>

<div id="drawer" class="drawer" hidden>
  <div class="drawer__backdrop"></div>
  <div class="drawer__panel">
    <nav>
      <a href="/products">Products</a>
      <a href="/about">About</a>
      <a href="/support">Support</a>
    </nav>
  </div>
</div>
```

```css
.drawer { position: fixed; inset: 0; z-index: 60; }
.drawer[hidden] { display: none; }

.drawer__backdrop {
  position: absolute;
  inset: 0;
  background: rgba(0, 0, 0, 0.4);
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
  animation: slideInLeft 280ms var(--ease-spring);
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes slideInLeft {
  from { transform: translateX(-100%); }
  to { transform: translateX(0); }
}
```

## Breadcrumb

```html
<nav class="breadcrumb" aria-label="Breadcrumb">
  <a href="/">Home</a>
  <span aria-hidden="true">/</span>
  <a href="/products">Products</a>
  <span aria-hidden="true">/</span>
  <span aria-current="page">MacBook Pro</span>
</nav>
```

```css
.breadcrumb {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 13px;
  color: var(--text-secondary);
}

.breadcrumb a { color: var(--text-secondary); text-decoration: none; }
.breadcrumb a:hover { color: var(--text-primary); }
.breadcrumb [aria-current="page"] { color: var(--text-primary); }
```

## Common mistakes

- Sticky navbar with `position: fixed` and no body padding → content jumps behind it.
- Bottom tab bar with 7 destinations → too many.
- Drawer that slides from the wrong side (left vs right) → consistency matters.
- Navbar with a logo that resembles Apple → trademark issue.
- "Floating navbar" on every page → only on pages where it adds value.
- Hamburger on desktop → only on mobile.
- No focus styles on nav links.
