# Forms Reference

Extended recipes for Apple-inspired form design. Builds on `apple-ui-components/SKILL.md`.

## Core principle

> A form is a conversation. Each field is a question. Labels are above fields, not inside them. Help is below the field, not in a tooltip.

## Basic input

```html
<label class="field">
  <span class="field__label">Email</span>
  <input class="field__input" type="email" required>
</label>
```

```css
.field {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.field__label {
  font-size: 13px;
  font-weight: 500;
  color: var(--text-secondary);
}

.field__input {
  height: 44px;
  padding: 0 14px;
  border: 1px solid var(--border-strong);
  border-radius: 10px;
  background: var(--bg-elevated);
  font: inherit;
  font-size: 16px;
  color: var(--text-primary);
  transition: border-color 120ms ease-out, box-shadow 120ms ease-out;
}

.field__input:hover {
  border-color: rgba(0, 0, 0, 0.20);
}

.field__input:focus {
  outline: none;
  border-color: var(--accent);
  box-shadow: 0 0 0 3px rgba(0, 102, 204, 0.20);
}

.field__input::placeholder {
  color: var(--text-tertiary);
}

.field__input:disabled {
  opacity: 0.4;
  cursor: not-allowed;
}

@media (max-width: 768px) {
  .field__input { height: 48px; }
}
```

## Field with helper text

```html
<label class="field">
  <span class="field__label">Email</span>
  <input class="field__input" type="email" required>
  <span class="field__helper">We'll send a verification link.</span>
</label>
```

```css
.field__helper {
  font-size: 12px;
  color: var(--text-tertiary);
}
```

## Field with error

```html
<label class="field field--error">
  <span class="field__label">Email</span>
  <input class="field__input" type="email" aria-invalid="true" aria-describedby="email-error">
  <span class="field__error" id="email-error">Please enter a valid email.</span>
</label>
```

```css
.field--error .field__input {
  border-color: #FF3B30;
}

.field--error .field__input:focus {
  box-shadow: 0 0 0 3px rgba(255, 59, 48, 0.20);
}

.field__error {
  font-size: 12px;
  color: #FF3B30;
}
```

## Textarea

```html
<label class="field">
  <span class="field__label">Message</span>
  <textarea class="field__input field__input--textarea" rows="4"></textarea>
</label>
```

```css
.field__input--textarea {
  height: auto;
  min-height: 96px;
  padding: 12px 14px;
  line-height: 1.4;
  resize: vertical;
}
```

## Select

Native select styled to match inputs:

```html
<label class="field">
  <span class="field__label">Country</span>
  <div class="field__select">
    <select class="field__select-input">
      <option>United States</option>
      <option>United Kingdom</option>
      <option>Japan</option>
    </select>
    <svg class="field__select-chevron" viewBox="0 0 12 12">
      <path d="M3 4.5 L6 7.5 L9 4.5" stroke="currentColor" stroke-width="1.5" fill="none"/>
    </svg>
  </div>
</label>
```

```css
.field__select {
  position: relative;
}

.field__select-input {
  appearance: none;
  width: 100%;
  height: 44px;
  padding: 0 36px 0 14px;
  border: 1px solid var(--border-strong);
  border-radius: 10px;
  background: var(--bg-elevated);
  font: inherit;
  font-size: 16px;
  color: var(--text-primary);
}

.field__select-chevron {
  position: absolute;
  right: 14px;
  top: 50%;
  transform: translateY(-50%);
  width: 12px;
  height: 12px;
  pointer-events: none;
  color: var(--text-tertiary);
}
```

## Checkbox

```html
<label class="checkbox">
  <input type="checkbox" class="checkbox__input">
  <span class="checkbox__box"></span>
  <span class="checkbox__label">Subscribe to newsletter</span>
</label>
```

```css
.checkbox {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  cursor: pointer;
}

.checkbox__input {
  position: absolute;
  opacity: 0;
  pointer-events: none;
}

.checkbox__box {
  display: inline-flex;
  width: 20px;
  height: 20px;
  border: 1px solid var(--border-strong);
  border-radius: 6px;
  background: var(--bg-elevated);
  flex-shrink: 0;
  transition: background-color 120ms ease-out, border-color 120ms ease-out;
}

.checkbox__input:checked + .checkbox__box {
  background: var(--accent);
  border-color: var(--accent);
}

.checkbox__input:focus-visible + .checkbox__box {
  box-shadow: 0 0 0 3px rgba(0, 102, 204, 0.20);
}

.checkbox__input:checked + .checkbox__box::after {
  content: "";
  display: block;
  width: 12px;
  height: 12px;
  margin: auto;
  background: url("data:image/svg+xml,...") center/contain no-repeat;
}

.checkbox__label {
  font-size: 14px;
  color: var(--text-primary);
}
```

## Toggle / Switch

```html
<label class="toggle">
  <input type="checkbox" class="toggle__input">
  <span class="toggle__track">
    <span class="toggle__thumb"></span>
  </span>
  <span class="toggle__label">Dark mode</span>
</label>
```

```css
.toggle {
  display: inline-flex;
  align-items: center;
  gap: 12px;
  cursor: pointer;
}

.toggle__input {
  position: absolute;
  opacity: 0;
  pointer-events: none;
}

.toggle__track {
  position: relative;
  display: inline-block;
  width: 48px;
  height: 28px;
  background: rgba(120, 120, 128, 0.32);
  border-radius: 9999px;
  transition: background-color 200ms var(--ease-spring);
}

.toggle__thumb {
  position: absolute;
  top: 2px;
  left: 2px;
  width: 24px;
  height: 24px;
  background: white;
  border-radius: 50%;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.20);
  transition: transform 200ms var(--ease-spring);
}

.toggle__input:checked + .toggle__track {
  background: #34C759;
}

.toggle__input:checked + .toggle__track .toggle__thumb {
  transform: translateX(20px);
}

.toggle__input:focus-visible + .toggle__track {
  box-shadow: 0 0 0 3px rgba(0, 102, 204, 0.20);
}
```

## Search input

```html
<label class="search">
  <svg class="search__icon" viewBox="0 0 24 24" width="16" height="16">...</svg>
  <input class="search__input" type="search" placeholder="Search">
</label>
```

```css
.search {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  height: 36px;
  padding: 0 12px;
  background: rgba(0, 0, 0, 0.05);
  border: 0;
  border-radius: 10px;
  min-width: 200px;
}

.search:focus-within {
  background: var(--bg-elevated);
  box-shadow: 0 0 0 3px rgba(0, 102, 204, 0.20);
}

.search__input {
  border: 0;
  background: transparent;
  font: inherit;
  font-size: 14px;
  outline: none;
  flex: 1;
}
```

## Fieldset / grouped form

```html
<fieldset class="form-group">
  <legend class="form-group__legend">Shipping address</legend>
  <div class="form-row">
    <label class="field">
      <span class="field__label">First name</span>
      <input class="field__input" type="text" required>
    </label>
    <label class="field">
      <span class="field__label">Last name</span>
      <input class="field__input" type="text" required>
    </label>
  </div>
</fieldset>
```

```css
.form-group {
  border: 0;
  margin: 0;
  padding: 0;
}

.form-group__legend {
  font-size: 17px;
  font-weight: 600;
  margin-bottom: 12px;
}

.form-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 12px;
}

@media (max-width: 768px) {
  .form-row { grid-template-columns: 1fr; }
}
```

## Inline validation

```js
const form = document.querySelector("form");

form.addEventListener("submit", (e) => {
  e.preventDefault();

  let valid = true;
  form.querySelectorAll("[required]").forEach((field) => {
    if (!field.value.trim()) {
      field.closest(".field").classList.add("field--error");
      field.setAttribute("aria-invalid", "true");
      valid = false;
    } else {
      field.closest(".field").classList.remove("field--error");
      field.removeAttribute("aria-invalid");
    }
  });

  if (valid) {
    form.submit();
  }
});
```

## Common mistakes

- Placeholder as label (placeholder disappears when user types).
- Tiny font on input (16 px minimum, prevents iOS zoom).
- Card-wrapped inputs (looks like settings list, not a form).
- Error state without `aria-invalid` and `aria-describedby`.
- Inline labels above but no helper text.
- Submit button without a loading state.
- Form submission without success/error feedback.
- Inputs that aren't keyboard accessible (custom widgets need proper ARIA).
- Disabled inputs without explanation.

## Accessibility

- Labels above fields, not placeholders.
- `aria-invalid="true"` on error.
- `aria-describedby` pointing to helper / error text.
- All fields keyboard-navigable.
- Focus visible on every input.
- Submit button focusable and labeled.
- Error messages announced via `role="alert"` or `aria-live="polite"`.
- Form is usable without JavaScript (server-side validation fallback).
