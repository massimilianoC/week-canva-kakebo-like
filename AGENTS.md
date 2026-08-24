# AGENTS.md — instructions for AI coding agents

This file is the contract for automated changes to this repository. Read it
before touching anything. Human contributors should read it too; the rules are
not agent-specific, only the phrasing is.

---

## 1. What this repository is

A **single-file, print-first document**. `index.html` renders one A3 landscape
sheet (420 × 297 mm) that a person prints and fills in with a pen.

It is not a web app. It has no build step, no package manager, no dependencies,
no JavaScript, and no test suite. The entire product is one HTML file with an
inline `<style>` block.

This constraint is the design, not an accident. A user must be able to download
one file, double-click it, and print. **Do not "improve" it into a project with
tooling.**

---

## 2. Hard constraints

Violating any of these is a regression, even if the page still looks fine.

1. **`index.html` stays self-contained.** One file. CSS inline in `<style>`.
   No external stylesheet, no JS file, no image file, no icon font.
   *The single permitted exception* is the Google Fonts `<link>` already in
   `<head>`.
2. **No JavaScript.** Not for interactivity, not for "progressive enhancement",
   not for a dark mode toggle. The sheet is paper.
3. **No build step.** No `package.json`, no bundler, no CSS preprocessor, no
   minifier, no CI that transforms the file.
4. **The sheet is exactly 420 × 297 mm and prints as exactly one page.**
   Verify it (§5) after any change to layout, type size, or content volume.
5. **Physical units for anything that reaches paper.** Lengths in `mm`, type
   sizes in `pt`. Do not convert to `px` or `rem`; a print document is specified
   in real-world units and `px` breaks that contract.
6. **Never remove these print invariants:**
   - `@page { size: 420mm 297mm; margin: 0 }`
   - `print-color-adjust: exact` (and the `-webkit-` prefix) on `html, body`
   - the `@media print` block that pins `html, body` to the sheet size with
     `overflow: hidden` — it is what stops a sub-millimetre rounding overflow
     from emitting a second blank page
   - the `body:not(.print-tinted)` rule inside `@media print` that overrides
     `--paper` / `--paper-tile` to white — deliberate ink-saving default (the
     paper tint is full-bleed across an A3 sheet; on screen it stays tinted).
     `class="print-tinted"` on `<body>` opts back into the tinted background at
     print time. Do not delete this rule to "fix" a colour that looks different
     on paper than on screen — that difference is intended.
7. **Colours and geometry go through the `:root` custom properties.** If you
   need a new colour, add a token; do not hard-code a hex value in a rule.
8. **Keep the sheet monolingual.** It ships in Italian. If asked to translate,
   translate *all* user-visible strings, including the tear line, the legend and
   the column headings. Do not leave a half-Italian sheet.
9. **`design-source/` is read-only.** See §3.

---

## 3. File map

| Path | Status | Notes |
|---|---|---|
| `index.html` | **the source of truth** | Edit this. It is the product. |
| `README.md`, `README.it.md` | edit in pairs | Any user-facing change must land in **both** languages. Never update one alone. |
| `docs/CUSTOMIZE.md`, `docs/PERSONALIZZA.md` | edit in pairs | Same rule. |
| `docs/preview.png` | regenerate | Stale after any visual change — see §6. |
| `AGENTS.md` | edit deliberately | This file. |
| `LICENSE` | **do not edit** | Verbatim AGPL-3.0 text from the FSF. |
| `design-source/**` | **READ-ONLY** | |

### About `design-source/`

It holds the original [Claude Design](https://claude.ai/design) mock-up that
`index.html` was reimplemented from: `La Settimana Possibile.dc.html` plus the
design tool's runtime (`doc-page.js`, `support.js`) and the `_ds/` design system.

**None of it is loaded at runtime. None of it should ever be edited.** It exists
so a human can compare against the original intent.

The trap: `La Settimana Possibile.dc.html` looks like source. It is not. It uses
custom elements (`<x-dc>`, `<doc-page>`, `<sc-if>`) that only work inside the
design tool. If a task says "change the sheet", the file you want is
`index.html` in the repository root.

---

## 4. Recipes for common changes

### 4.1 Recolour the sheet

Edit the `:root` block at the top of `index.html`. Nothing else. The eleven
palette tokens (`--paper`, `--ink`, `--accent`, the `--protected-*` trio, the
`--limit-*` trio, …) reach every rule.

Keep the semantics intact: `--protected` must read as "safe / non-negotiable"
and `--limit` as "stop / avoid". Swapping them for two arbitrary colours breaks
the sheet's meaning even though nothing errors.

Remember the `--paper` / `--paper-tile` values you set here are the **on-screen**
and `class="print-tinted"` colours. What actually reaches paper by default is
white, per the `body:not(.print-tinted)` override in `@media print` (§2 rule 6)
— that is intentional and untouched by this recipe.

### 4.2 Rename a route — ⚠️ two places

**This is the single most common way to corrupt this document.** Each route name
appears **twice**, and the two must always match:

1. the card in section 02 — search `class="route-name display"`
2. the log row in section 03 — search `class="log-row"`, then the
   `<span class="text">` inside its `.col-label`

Rows 01–04 of the log are the four routes, in the same order as the cards. Rows
05 (`Famiglia ✓`) and 06 (`Not-to-do ✓`) are *not* routes — they are the
protected and limit rows, they carry no target, and they must keep their
`log-row-protected` / `log-row-limit` classes.

After renaming, re-read both sections and confirm the order still lines up.

### 4.3 Change the legend symbols

Search `class="legend"`. Each item is
`<span class="legend-item"><span class="glyph">EMOJI</span>label</span>`.

Keep the trailing `legend-custom` block — the empty box plus dotted line is
where a user invents their own symbol by hand. It is a feature, not filler.

Six items is what fits. Adding a seventh will squeeze the dotted line to
nothing; if you must add one, shorten the labels.

### 4.4 Add or remove a log row

Duplicate a `<div class="log-row">` block. Every row must contain exactly:
one `.col-label`, **seven** `.col-day` divs, one `.col-total`.

Rows share the height with `flex: 1`, so adding one makes them all shorter
automatically. Past roughly eight rows the rows get too thin to write in — run
§5 and look at the resulting row height, not just at whether it fits.

### 4.5 Add or remove review lines

Inside `.review-lines`, add or remove `<div class="rule-dotted"></div>`.
`justify-content: space-between` redistributes them; no CSS change needed.

### 4.6 Change the paper size — ⚠️ two places

The `@page` rule **cannot read custom properties**. Both of these must change
together:

```css
:root { --sheet-w: 420mm; --sheet-h: 297mm; }
@page { size: 420mm 297mm; margin: 0; }
```

Then be honest about the consequence: this is a hand-tuned layout in absolute
units. Shrinking A3 → A4 does not rescale the typography — 6 pt labels stay
6 pt on a 30 % smaller sheet and the content overflows. A real A4 variant means
re-tuning type sizes and every fixed `mm` height, and it is a substantial task.
Do not deliver a resized `@page` and call it an A4 version.

---

## 5. Verification — mandatory

Run this after **any** change to `index.html`. Do not report the work as done
without it. "It looks right" is not verification for a document specified in
millimetres.

Open `index.html` in Chrome (`file://` is fine) and paste into the DevTools
console:

```js
(async () => {
  await document.fonts.ready;
  const mm = 96 / 25.4, q = s => document.querySelector(s);
  const h = el => +(el.getBoundingClientRect().height / mm).toFixed(2);
  const w = el => +(el.getBoundingClientRect().width / mm).toFixed(2);
  const sheet = q('.sheet'), frame = q('.frame');
  const rowH = [...document.querySelectorAll('.log-row')].map(h);
  console.table({
    'sheet width  (must be 420)': w(sheet),
    'sheet height (must be 297)': h(sheet),
    'content overflow (must be <= 297.5)': +(frame.scrollHeight / mm).toFixed(2),
    'log row height (should be >= 5mm to write in)': Math.min(...rowH),
    'log rows': rowH.length,
    'fonts loaded (must be true)':
      document.fonts.check('700 31pt "Space Grotesk"') &&
      document.fonts.check('400 12pt "Inter"') &&
      document.fonts.check('400 12pt "IBM Plex Mono"'),
    'tokens resolved (must be true)':
      getComputedStyle(q('.tile-protected')).backgroundColor !== 'rgba(0, 0, 0, 0)'
  });
})();
```

**Pass criteria**

| Check | Required |
|---|---|
| sheet width | exactly `420` |
| sheet height | exactly `297` |
| content overflow | `<= 297.5` (297.13 is the normal sub-pixel value) |
| minimum log row height | `>= 5` mm, or nobody can write in it |
| every `.log-row` | exactly 7 `.col-day` children |
| fonts loaded | `true` |
| tokens resolved | `true` — `false` means a `var()` typo |

Then **print-preview it** (`Ctrl+P`) and confirm the dialog reports **1 page**,
not 2. A second blank page is the classic failure of this layout and it does not
show up on screen.

While the print preview is open, confirm the ink-saving default is still
working: the `.route` / `.tile` neutral boxes should render **white**, while
`.tile-protected` (green) and `.tile-limit` (red) keep their tint. If a change
to `index.html` touched `--paper`, `--paper-tile`, or the `@media print` block,
verify it did not silently drop the `body:not(.print-tinted)` override — that
regression is invisible on screen and only shows in print preview.

---

## 6. Regenerating the preview

`docs/preview.png` goes stale after visual changes. Regenerate with any headless
browser element-screenshot of `.sheet`, e.g. Playwright:

```js
await page.goto('http://127.0.0.1:8080/index.html');   // file:// is blocked in Playwright
await page.locator('.sheet').screenshot({ path: 'docs/preview.png', scale: 'css' });
```

Serve the folder first (`python -m http.server 8080`). Do not commit a
screenshot of the browser window with chrome and scrollbars — capture the
`.sheet` element only.

---

## 7. Commits and scope

- Conventional-commit prefixes: `feat:`, `fix:`, `docs:`, `style:`, `chore:`.
- One logical change per commit. A recolour and a route rename are two commits.
- If a change touches `index.html` visually, the same commit should refresh
  `docs/preview.png`.
- **Never** commit a change to `index.html` that has not passed §5.
- Do not add files to the repository root. New documentation goes in `docs/`.

## 8. Definition of done

- [ ] §5 verification run, all criteria pass
- [ ] print preview reports exactly 1 page
- [ ] no new files, dependencies, JS, or build steps introduced
- [ ] route names still match between section 02 and the log in section 03
- [ ] both `README.md` and `README.it.md` updated, if user-facing behaviour changed
- [ ] both `docs/CUSTOMIZE.md` and `docs/PERSONALIZZA.md` updated, if customization changed
- [ ] `docs/preview.png` regenerated, if the sheet looks different
- [ ] `design-source/` untouched

## 9. License note

This project is AGPL-3.0. Contributions are accepted under the same license.
Do not add code copied from an incompatibly-licensed source, and do not remove
the copyright header comment at the top of the `<style>` block in `index.html`.
