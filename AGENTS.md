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
   No external stylesheet, no JS file, no image file, no icon font. Two
   permitted exceptions, both already present and both deliberate:
   the Google Fonts `<link>` in `<head>`, and the `.dl-pdf` link in the
   closing `<body>` — a plain `<a download>` to `examples/…pdf`, screen-only
   (`display: none` in `@media print`, see rule 6). Do not add a third.
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
8. **Keep the sheet monolingual.** It ships in English. If asked to translate,
   translate *all* user-visible strings, including the tear line, the legend and
   the column headings. Do not leave a half-translated sheet.
9. **`examples/*.pdf` is a generated artefact, not something to hand-edit.**
   See §6a — regenerate it, never open it in a PDF editor.

---

## 3. File map

| Path | Status | Notes |
|---|---|---|
| `index.html` | **the source of truth** | Edit this. It is the product. |
| `README.md` | user-facing project overview | Update it when user-facing behaviour changes. |
| `docs/CUSTOMIZE.md` | customization guide | Update it when customization behaviour changes. |
| `docs/preview.png` | regenerate | Stale after any visual change — see §6. |
| `examples/*.pdf` | regenerate | Generated from `index.html` — see §6a. Never hand-edit a PDF. |
| `AGENTS.md`, `CLAUDE.md` | edit deliberately | Agent instructions; keep their shared constraints aligned. |
| `LICENSE` | **do not edit** | Verbatim AGPL-3.0 text from the FSF. |

There is no `design-source/` in this repository. Earlier drafts kept the
original [Claude Design](https://claude.ai/design) mock-up as provenance, but
its runtime files (`doc-page.js`, `support.js`) came from Anthropic's design
tool with no license header of their own — redistributing them inside a repo
whose top-level `LICENSE` is AGPL-3.0 was a real, not hypothetical, legal risk.
They were removed for the public release. **Do not reintroduce a mock-up
export, a design-tool runtime, or a bundled "design system" into this repo,**
even as read-only reference material, without first confirming its licensing
allows redistribution under `LICENSE`.

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
05 (`Family ✓`) and 06 (`Not-to-do ✓`) are *not* routes — they are the
protected and limit rows, they carry no target, and they must keep their
`log-row-protected` / `log-row-limit` classes.

After renaming, re-read both sections and confirm the order still lines up.

### 4.3 Change the legend symbols

Search `class="legend"`. Each item is
`<span class="legend-item"><span class="glyph">SYMBOL</span>label</span>`.

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

## 6a. Regenerating the example PDF

`examples/the-week-that-fits.pdf` goes stale after any change to
`index.html`'s layout or content — it is a rendered snapshot, not hand-authored.
Regenerate it with Playwright, forcing print media so the ink-saving default
(§2 rule 6) actually applies:

```js
await page.goto('http://127.0.0.1:8080/index.html');
await page.emulateMedia({ media: 'print' });
await page.pdf({
  path: 'examples/the-week-that-fits.pdf',
  width: '420mm', height: '297mm',
  printBackground: true,
  margin: { top: 0, right: 0, bottom: 0, left: 0 },
  preferCSSPageSize: true,
});
```

Verify the result before committing: exactly one `/Type/Page` in the PDF body,
and a `/MediaBox` of `[0 0 1191.12 841.92]` pt (= 420 × 297 mm). A second page
or a mismatched MediaBox means §5 was failing when you generated it — fix
`index.html` first, regenerate after.

The PDF ships **empty** — no personal data filled in. Never commit a PDF that
contains a real filled-in week; that is someone's private planning data, not a
template.

---

## 7. Commits and scope

- Conventional-commit prefixes: `feat:`, `fix:`, `docs:`, `style:`, `chore:`.
- One logical change per commit. A recolour and a route rename are two commits.
- If a change touches `index.html` visually, the same commit should refresh
  `docs/preview.png`.
- **Never** commit a change to `index.html` that has not passed §5.
- Do not add files to the repository root, except recognised agent-instruction
  files such as `AGENTS.md` and `CLAUDE.md`. New human documentation goes in `docs/`.

## 8. Definition of done

- [ ] §5 verification run, all criteria pass
- [ ] print preview reports exactly 1 page
- [ ] no new files, dependencies, JS, or build steps introduced
- [ ] route names still match between section 02 and the log in section 03
- [ ] `README.md` updated, if user-facing behaviour changed
- [ ] `docs/CUSTOMIZE.md` updated, if customization changed
- [ ] `docs/preview.png` regenerated, if the sheet looks different
- [ ] `examples/the-week-that-fits.pdf` regenerated (§6a), if `index.html`'s
      layout or content changed
- [ ] no personal/filled-in data ever committed to `examples/`

## 9. License note

This project is AGPL-3.0. Contributions are accepted under the same license.
Do not add code copied from an incompatibly-licensed source, and do not remove
the copyright header comment at the top of the `<style>` block in `index.html`.
