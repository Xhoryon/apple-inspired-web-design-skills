# Screenshot Script Reference

The audit requires **real browser screenshots**. This reference gives drop-in recipes for Playwright (Node) and headless Chrome.

## Playwright (Node) — full audit script

```js
// audit.mjs
import { chromium } from "playwright";
import fs from "node:fs";

const URL = process.env.URL || "http://127.0.0.1:8080";
const OUT = "./audit-screenshots";
fs.mkdirSync(OUT, { recursive: true });

const cases = [
  { name: "desktop-light", viewport: { width: 1440, height: 900 }, colorScheme: "light" },
  { name: "desktop-dark",  viewport: { width: 1440, height: 900 }, colorScheme: "dark" },
  { name: "mobile-light",  viewport: { width: 390, height: 844 }, colorScheme: "light", isMobile: true },
  { name: "mobile-dark",   viewport: { width: 390, height: 844 }, colorScheme: "dark", isMobile: true },
];

const browser = await chromium.launch();

for (const c of cases) {
  const ctx = await browser.newContext({
    viewport: c.viewport,
    colorScheme: c.colorScheme,
    reducedMotion: "no-preference",
    deviceScaleFactor: c.isMobile ? 2 : 1,
    userAgent: c.isMobile
      ? "Mozilla/5.0 (iPhone; CPU iPhone OS 17_0 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/17.0 Mobile/15E148 Safari/604.1"
      : undefined,
    isMobile: !!c.isMobile,
    hasTouch: !!c.isMobile,
  });
  const page = await ctx.newPage();
  await page.goto(URL, { waitUntil: "networkidle" });
  await page.waitForTimeout(800); // settle animations

  // Full page
  await page.screenshot({
    path: `${OUT}/${c.name}-full.png`,
    fullPage: true,
  });

  // First viewport
  await page.screenshot({
    path: `${OUT}/${c.name}-viewport.png`,
    fullPage: false,
  });

  await ctx.close();
}

// Reduced motion capture
const ctx = await browser.newContext({
  viewport: { width: 1440, height: 900 },
  colorScheme: "light",
  reducedMotion: "reduce",
});
const page = await ctx.newPage();
await page.goto(URL, { waitUntil: "networkidle" });
await page.waitForTimeout(500);
await page.screenshot({ path: `${OUT}/desktop-reduced-motion.png`, fullPage: true });
await ctx.close();

await browser.close();

console.log("Screenshots written to", OUT);
```

Run: `node audit.mjs`

## Puppeteer (Node) — minimal

```js
import puppeteer from "puppeteer";

const browser = await puppeteer.launch();
const page = await browser.newPage();
await page.setViewport({ width: 1440, height: 900 });
await page.goto("http://127.0.0.1:8080", { waitUntil: "networkidle" });
await page.screenshot({ path: "./audit/desktop.png", fullPage: true });
await browser.close();
```

## Headless Chrome via CLI

```bash
google-chrome --headless --disable-gpu --no-sandbox \
  --window-size=1440,900 \
  --screenshot=./audit/desktop.png \
  --hide-scrollbars \
  http://127.0.0.1:8080
```

Limited — no full-page, no reduced-motion emulation. Use Playwright for real audits.

## Glass close-up capture

If the design has glass surfaces, capture them over light, dark, and image backgrounds:

```js
// Capture a specific selector over different backgrounds
const selector = ".glass-navbar";
const backgrounds = ["light", "dark", "image"];

for (const bg of backgrounds) {
  await page.evaluate((b) => {
    document.body.dataset.bg = b;
    document.body.style.background = b === "light" ? "#f5f5f7"
      : b === "dark" ? "#1c1c1e"
      : "url('/test-image.jpg') center/cover no-repeat";
  }, bg);

  await page.waitForTimeout(300);
  await page.locator(selector).screenshot({ path: `${OUT}/glass-${bg}.png` });
}
```

## Hover and focus capture

```js
// Hover state
await page.locator(".primary-cta").hover();
await page.waitForTimeout(300);
await page.screenshot({ path: `${OUT}/hover.png` });

// Focus state
await page.keyboard.press("Tab");
await page.keyboard.press("Tab");
await page.screenshot({ path: `${OUT}/focus.png` });
```

## Reduced-motion verification

```js
const ctx = await browser.newContext({ reducedMotion: "reduce" });
// Capture same hero; verify no parallax / no pinned scroll
```

## What to capture for a full audit

- [ ] Desktop light, full page
- [ ] Desktop dark, full page
- [ ] Mobile light, full page
- [ ] Mobile dark, full page
- [ ] Hero only (first viewport), desktop and mobile
- [ ] Glass surface close-ups (light bg, dark bg, image bg)
- [ ] Hover state on primary CTA
- [ ] Hover state on a floating control (if any)
- [ ] Reduced-motion variant, desktop
- [ ] Open sheet / popover / modal (if any)

Save to `./audit-screenshots/` and **open every file**. Do not trust the file count; trust the pixels.
