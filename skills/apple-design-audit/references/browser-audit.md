# Browser Execution Adapter (v1.1 — Tier 2)

This is the **browser execution layer** for `apple-design-audit`. It is an **optional** enhancement, not a requirement. The universal audit (in `apple-design-audit/SKILL.md`) works on text descriptions; this layer adds real rendered verification when a browser-capable environment is available.

## Capability gate

Before running this adapter, confirm the agent has browser / rendering capability. If not, fall back to the universal audit and explicitly note: **"browser verification not performed."**

Do not pretend to have rendered output when no browser ran. Do not skip the universal audit because a browser is missing — the universal audit is the floor; the browser adapter is the ceiling.

## What the browser adapter does

The adapter captures real rendered output and verifies invariants that cannot be checked from source code alone:

- Layout rendering at multiple viewports (e.g. 1440 px desktop, 390 px mobile, 360 px narrow).
- Computed styles (font, color, background, backdrop-filter, transform, etc.) at runtime.
- Color-scheme (light / dark) and `prefers-reduced-motion` behavior.
- Real touch-target sizes via `getBoundingClientRect()`.
- Horizontal overflow at each viewport.
- Glass surface optical behavior (refraction, specular, fallback) when implemented.
- Reduced-motion behavior (matchMedia + computed transition durations).

## Tooling

The browser adapter is **tool-agnostic**. It does not require Playwright specifically. Any tool that can:

- Render an HTML page in a real browser.
- Capture screenshots at multiple viewports and color schemes.
- Read computed styles and bounding rects.
- Emulate media features (`prefers-reduced-motion`, `prefers-color-scheme`).

is acceptable. Common choices:

- **Playwright** (Node.js) — multi-browser, well-documented.
- **Puppeteer** (Node.js) — Chromium-only.
- **Selenium WebDriver** — multi-language, multi-browser.
- **Browser automation in any other framework** that meets the capability gate.

Do not hard-code Playwright in a way that excludes agents with a different browser tool. State the capability the agent has; pick the tool that provides it.

## Capture matrix (recommended)

For a single audit pass, capture:

| Viewport | Color scheme | Reduced motion | Purpose |
|---|---|---|---|
| 1440 × 900 | light | no-preference | Desktop baseline |
| 1440 × 900 | dark | no-preference | Dark mode |
| 1440 × 900 | light | reduce | Reduced motion |
| 390 × 844 | light | no-preference | Mobile baseline |
| 390 × 844 | dark | no-preference | Mobile dark |
| 360 × 780 | light | no-preference | Narrow mobile (F-09 closure) |

This produces 4–6 viewports per audit, which is sufficient to verify the 100-point checklist across form factors.

## Behavioral evidence (v1.1 addition)

The v1.0 round's reduced-motion capture produced byte-identical screenshots between light and reduced-motion because static settled-state images do not differ — only the transition behavior differs. v1.1 emphasizes **behavioral evidence** over visual diff:

| Check | Evidence type | Acceptance |
|---|---|---|
| `prefers-reduced-motion` actually applied | `matchMedia` query | returns `true` in reduced context |
| Transition behavior changed | computed `transition-duration` | `≤ 0.05s` (was 0.12s+) under reduced |
| Touch targets ≥ 44 × 44 | `getBoundingClientRect()` on all interactive elements | 0 elements below threshold |
| Horizontal overflow | `getBoundingClientRect().right > viewportWidth + 1` | 0 elements overflow at all viewports |
| Liquid Glass Level 2 active | computed `filter` on `.glass-wrap__backdrop` references SVG | exactly 1 surface, not more |
| Reduced transparency fallback | `backdrop-filter: none` under `@media (prefers-reduced-transparency: reduce)` | surfaces solid |

The browser adapter should report these **computed values**, not just visual screenshots. A static screenshot is necessary but not sufficient evidence.

## Anti-pattern detection in the browser

A subset of the 27 anti-patterns can be detected in the rendered output (Level 2 Glass usage, touch-target failures, overflow, etc.). For these, the browser adapter runs the detection and reports the count and items. The remaining anti-patterns are reasoning checks handled by the universal audit.

## Loop

After the browser adapter reports, return to the universal audit's fix loop. Iterate until the page passes the gates.

## What this adapter is NOT

- Not the only valid way to capture screenshots. Use whatever browser tool the agent has.
- Not a Playwright-only contract. State the capability, not the library.
- Not a replacement for the universal audit. The universal audit is the floor; the browser adapter is the ceiling.