# AGENTS.md

Single-page static app that converts lunar birthdays to ICS calendar files. All HTML/CSS/JS lives in `index.html`. No bundler, no package.json, no build step, no tests.

## Running locally
```bash
python3 -m http.server 8000   # then open http://localhost:8000
```
There is no lint/typecheck/test command. Verification is manual: open the page, watch the browser console, exercise add/generate/delete/language-switch/refresh.

## Dependencies (CDN, pinned — never use `@latest`)
Loaded from jsDelivr at the **end of `<body>`** in `index.html` (after the DOM, before the inline script — they're render-blocking and `lunar.js` alone is ~300 KB; the inline `init()` calls `Lunar` synchronously, so they must load before it runs):
- `lunar-javascript@1.7.7` — lunar↔solar conversion
- `chinese-conv@4.0.0` — simplified↔traditional Chinese
- `ics.js@2.3.2` — ICS file generation

Bumping a version requires manual re-testing of ICS output; breaking changes have shipped on minor bumps before.

## lunar-javascript date API — critical
Verified from the 1.7.7 source, and it contradicts native JS `Date`:
- `Solar.getMonth()` returns **1–12** (not 0–11). `Solar.getDay()` returns **1–31**.
- `Solar.toYmd()` already returns a zero-padded `YYYY-MM-DD` string — safe for display.

Format solar dates **without** adding 1 to the month:
```javascript
const solar = lunar.getSolar();
const month = String(solar.getMonth()).padStart(2, '0'); // already 1-12, NO +1
const day   = String(solar.getDay()).padStart(2, '0');
```

**Leap months:** `Lunar.fromYmd(y, m, d)` takes exactly **3 args**; a leap month is a **negative** month (e.g. `-5` for leap fifth month), never a boolean 4th arg. Use the shared helper so both conversion paths stay consistent:
```javascript
Lunar.fromYmd(year, lunarMonthArg(month, isLeapMonth), day)
```
`lunarMonthArg(month, isLeap)` returns `-Math.abs(month)` when `isLeap`, else `month`. Both `getNextSolarDates` and `generateICS` call it.

**Leap-month check:** `LunarYear.fromYear(y).getLeapMonth()` returns the leap month (1–12) for year `y`, or `0` if none. Do **not** call `.getLeapMonth()` on `lunar.getYear()` — that returns a plain number and throws.

## ICS generation
- All-day, `YEARLY` recurrence, no `UNTIL`/`COUNT` (permanent).
- Date format `YYYYMMDDT000000Z` (UTC, `Z` suffix).
- First occurrence uses **next year** (`new Date().getFullYear() + 1`).
- Guard with `if (typeof ics === 'undefined')` before calling `ics()` — the CDN load can fail.

## Data & language
- `localStorage` key `'lunarBirthdays'` → JSON array of `{ id, name, referenceYear, lunarMonth, lunarDay, isLeapMonth, note, createdAt }`.
- `localStorage` key `'language'` → `'zh-CN'` (default) or `'zh-TW'`.
- IDs: `Date.now().toString()`.
- Languages: **zh-CN and zh-TW only.** All translatable UI text is driven by `data-i18n` (element text) and `data-i18n-placeholder` (input placeholders) attributes, backed by the `translations` table; `applyLanguage()` walks both. When adding UI text, add the key to **both** locale blocks (they must stay in parity) and tag the element. Do not add English or other locales unless explicitly asked.

## Conventions
- **Single-file architecture is mandatory:** do not split into `.css`/`.js` files, do not add bundlers or npm deps.
- Direct event handlers in HTML (`onclick=...`); functions live in one `<script>` block.
- camelCase vars/funcs; UPPER_SNAKE_CASE for translation/month tables; kebab-case classes.
- Use `showToast(msg, type)` for user feedback (never `alert`). `confirm()` is acceptable for destructive actions (e.g. clear-all).
- Wrap external-library calls in try/catch and surface failures via `showToast`.
- Avoid loop-variable name collisions across functions (a recurring source of bugs here — use distinct names per loop).
- Theme colors: primary `#667eea`, success `#2ed573`, danger `#ff4757`. `(必填)/(可选)` indicators use `<span class="optional-indicator">`.

## Deployment
- GitHub Pages, auto-deploys on push to `main` via `.github/workflows/deploy.yml`.
- Live URL: https://oroliy.github.io/lunar-birthday-calendar/
- Work directly on `main`; manual browser verification expected before considering work done.
