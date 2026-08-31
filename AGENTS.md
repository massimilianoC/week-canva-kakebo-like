# AGENTS.md — instructions for AI coding agents

This file is the contract for automated changes to this repository. Read it
before touching anything. Human contributors should read it too; the rules are
not agent-specific, only the phrasing is.

---

## 1. What this repository is

This repository has **two coexisting products**, and the rules below are
scoped accordingly:

1. `index.html` — a **single-file, print-first document**. Renders one A3
   landscape sheet (420 × 297 mm) that a person prints and fills in with a
   pen. §2's hard constraints (no JS, no build step, no dependencies) apply
   to this file only, and always will — it is not a legacy stage on the way
   to becoming an app, it is the point of the project.
2. `app/index.html` — a **digital companion**, covered separately in §10.
   It is allowed JavaScript and browser-local storage; it does not touch
   paper, it does not call a server, and it must never be merged back into
   the constraints of §2.

If you are only here to edit the printable sheet, §2–§9 are the full contract
and you can ignore §10. If you are working on the digital companion, read §10
before touching `app/index.html` — most of §2 does not apply there, but a few
things (palette tokens, monolingual-per-string discipline, no server) still
do, and §10 says which.

Neither file is a stepping stone to replace the other. The intent (see the
README) is to keep the paper sheet as the primary artifact indefinitely, with
the digital companion growing alongside it — not to eventually delete
`index.html` in favour of `app/`.

---

## 1a. `index.html` — the printable sheet

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
| `index.html` | **source of truth, printable sheet** | Edit this for the paper product. Governed by §2–§9. |
| `app/index.html` | **source of truth, digital companion** | Edit this for the digital product. Governed by §10. |
| `README.md` | user-facing project overview | Update it when user-facing behaviour changes, in either variant. |
| `docs/CUSTOMIZE.md` | customization guide | Update it when customization behaviour changes (paper variant). |
| `docs/preview.png` | regenerate | Stale after any visual change to `index.html` — see §6. |
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

This checklist is for changes to `index.html` (the printable sheet). For
`app/index.html` (the digital companion), use §10's own checklist instead.

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

---

## 10. `app/index.html` — the digital companion

### 10.1 What it is, and why it is a separate file

`app/index.html` is a fillable, local-first digital version of the same
weekly sheet. It exists because the paper sheet's hard constraints (§2: no
JS, no state) are permanent and non-negotiable, but there is real demand for
the sheet to also be usable as an on-screen, self-populating, exportable form.
Rather than compromise `index.html`, that demand gets its own file with its
own, looser contract.

**The two files stay in visual sync but are not code-shared.** There is no
build step to keep them identical (§1a still bans one), so a visual change to
one (palette, section order, route/log structure) should be considered for
the other, by hand, in the same PR when it's structural. Cosmetic print-only
details (fiducials, tear line, ink-saving print override) do not need to
carry over — the companion is screen/export-first, not print-first.

### 10.2 What is allowed here that is not allowed in `index.html`

- **JavaScript.** All state management, rendering, and interaction is vanilla
  JS in one inline `<script>` block. No framework, no bundler, no npm
  dependency — "no build step" (§1a) still applies; "no JavaScript" does not.
- **Local persistence.** `localStorage` only — keyed `twtf:settings`,
  `twtf:week:<ISO-year>-W<ISO-week>`, `twtf:lastWeek`. No cookies (they add
  nothing `localStorage` doesn't already give here, and cap out at ~4KB), no
  IndexedDB yet (see §10.5 for where that's headed), no server, no analytics,
  no network call of any kind. This must remain true even as the app grows —
  it is the privacy promise stated in the page's own footer note.
- **Runtime language switching.** `index.html` stays English-only, translated
  wholesale if ever (§1a / §2 rule 8 of the old numbering). `app/index.html`
  ships English **and** Italian behind a live toggle (`I18N` object in the
  `<script>` block). Adding a third language means adding a third key to
  every entry in `I18N`, not a partial one.
- **User-editable structure at runtime.** The four life-area names/tags
  (`route.name` / `route.tag`) are edited in the Settings modal (⚙) or
  in-place on the sheet, stored in `settings.routes`, and propagate to the
  log rows in section 03 automatically (`renderLogRows()` re-reads
  `effectiveRoutes()`). This is the digital equivalent of AGENTS.md §4.2's
  "rename a route — two places" — except here it's one place, because the
  renderer is what keeps section 02 and 03 in sync, not the editor's care.

### 10.3 What still carries over from the paper sheet's discipline

- **Palette and geometry tokens.** The `:root` custom properties mirror
  `index.html`'s (`--paper`, `--ink`, `--accent`, `--protected-*`,
  `--limit-*`, `--sheet-w/h`, `--margin`, `--gutter`, `--col-label`,
  `--col-total`). Recolouring one should usually mean recolouring the other,
  deliberately, in the same commit.
- **Physical units for anything meant to print.** The export path
  (`window.print()`, §10.4) still renders through the same `.sheet` box at
  `420mm × 297mm`, so `mm`/`pt` discipline still matters inside `.sheet`.
  The app-only chrome (toolbar, modals) is UI, not paper, and may use `px`.
- **"Protected" reads safe, "limit" reads stop.** Don't repurpose the green/
  red tokens for anything else.

### 10.4 Export ("compiled PDF")

The ⬇ **Export filled PDF** button calls `window.print()`. There is no PDF
library — printing *is* the export mechanism, same as the static file's
`.dl-pdf` link, except here the browser's print pipeline is rendering the
live, filled-in DOM instead of a pre-baked example. `@media print` hides
`.app-toolbar` and the modals, and strips input/textarea chrome (borders,
backgrounds) so filled fields read as ink on paper, not as form controls.
If you change the field markup, check that this still holds — an export with
visible input outlines is a regression.

### 10.5 Where this is headed (context, not a license to build ahead of need)

The long-term intent, per the project owner, is to keep evolving this file
step by step from "digital form filled by hand-typing" toward "instrument
with scoring, suggestions, and automated feedback," eventually with local
structured storage (e.g. SQLite via `sql.js` or OPFS) once `localStorage`'s
per-origin cap becomes a real constraint, and later still with OCR ingestion
of *photographed paper sheets* — closing the loop between the two products
instead of forcing a choice between them. None of that is built yet. Do not
pre-build scaffolding for it (no dead SQLite import, no stub OCR module, no
speculative schema) until a specific step is actually being implemented —
that's the project's own no-speculative-abstraction stance (see CLAUDE.md),
applied here too. When a step *is* implemented, log the architectural
decision in this section so the next agent has the same context.

### 10.6 Definition of done for `app/index.html` changes

- [ ] No new npm dependency, bundler, or build step introduced.
- [ ] No network call added — verify in DevTools' Network tab, not just by
      reading the diff.
- [ ] Every string added to `I18N.en` has a matching `I18N.it` entry (or
      vice versa) — no half-translated UI state.
- [ ] Route rename (via ⚙ or in-place) still propagates to section 03 —
      re-check after any change to `renderRoutesAndLog()` / `renderLogRows()`.
- [ ] `window.print()` export still hides `.app-toolbar` and modals, and
      still strips input/textarea chrome — check via print preview.
- [ ] `localStorage` schema changes are additive or migrated, not silently
      breaking — a returning user's saved week must not vanish. See §10.7's
      note on ADR `0002-local-schema-versioning.md`.
- [ ] `README.md` updated if user-facing behaviour of the companion changed.

### 10.7 Spec-first governance for `app/`

The digital companion is governed by a **spec-first agentic development**
discipline: docs precede non-trivial code, decisions are recorded, and work
moves in small, checkable slices. This is deliberately scoped to `app/` —
the printable sheet stays exactly as light-touch as §1a–§9 already describe;
do not import any of this section's process onto `index.html`.

**Adopted profile:** Base ("SFAD Compact") plus three borrowings from the
fuller profile — a slice tracker, MADR-format ADRs, and a committed session
handoff record — because this is a solo developer working through coding
agents across sessions, not a team with a second human reviewer or a CI
pipeline. That combination is itself a documented calibration point of the
methodology this was adapted from, not an ad-hoc shortcut.

**Where things live:**

| What | Where |
|---|---|
| Reading order, index of docs | `app/docs/INDEX.md` |
| Current focus + backlog | `app/docs/planning/ROADMAP.md` |
| Per-slice status tracker | `app/docs/planning/EXECUTION_SPEC.md` |
| Architecture/tooling decisions | `app/docs/decisions/NNNN-kebab-title.md` (MADR format, sequential, never deleted — only superseded) |
| Per-feature requirements + acceptance examples | `app/specs/<feature-id>/spec.md`, copied from `app/specs/feature/spec.md` |
| Cross-session working memory | `app/dev/SESSION_HANDOFF.md` (committed; overwritten each session, not appended to) |

**Before any non-trivial change to `app/index.html`:** read `app/docs/INDEX.md`
→ `ROADMAP.md` → the current milestone in `EXECUTION_SPEC.md` → the relevant
spec under `app/specs/` → `SESSION_HANDOFF.md`. A change with no spec and no
tracker row is the thing this governance layer exists to prevent — write the
spec first, even a short one, before writing the code.

**Slice discipline:** one slice (`M<n>.<m>`) in progress at a time; a slice
closes only when it is reachable/usable end to end, its declared check has
actually been run, and `EXECUTION_SPEC.md` is updated in the same change —
not "later." No slice is purely internal infrastructure; each maps to a
user-observable outcome, however small.

**Decision gates (adapted for solo work):** routine, reversible edits within
an already-approved spec need no check-in. Anything that changes product
scope, the persisted data shape, the SSoT for a concern, or is hard to
reverse gets presented as an explicit choice — recommended option,
alternatives, consequences, latest safe decision point — before
implementation, per `decision`-gate framing borrowed from the source
methodology. Don't ask for ceremonial approval of routine file edits; do ask
before inventing scope the project owner hasn't stated.

**Risk-based verification (adapted — no CI, no second reviewer):**

| Tier | Example in this app | Extra check beyond "open it and exercise the acceptance examples" |
|---|---|---|
| Low | copy/label changes, styling | none beyond the baseline |
| Medium | new persisted field, new interactive control | confirm `localStorage` round-trips correctly after a reload |
| High | any change to the shape of `settings` or `weekData` | migration check per ADR `0002` — an old-shape record must still load without data loss |

There is no "Critical" tier in this app in the source methodology's sense
(no payments, no irreversible server-side mutation) — the closest equivalent
is silent data loss in `localStorage`, which High tier already covers.

**Pillars explicitly not adopted, and why** (per the source methodology's own
rule: don't drop a pillar silently — record the reason): idempotency keys,
job/queue orchestration, server-side RBAC, an audit trail, and client-side
secret handling are all boundary/server concerns from the upstream pattern
this was adapted from, and this app has no server and no API — there is
nothing on the other side of a boundary for them to apply to. The one
server-oriented pillar that *does* apply despite there being no server is
forward-compatible, versioned data schemas — `localStorage` is this app's
real persistence boundary, and it gets the same discipline an API response
schema would. See ADR `0002-local-schema-versioning.md`.

**Commit style:** Conventional Commits (`feat:`, `fix:`, `docs:`, `chore:`,
scoped to `app` when useful, e.g. `feat(app): …`), same convention already in
use at repo root (§7) — no new convention introduced for this subfolder.

**Promotion signal:** if `app/` ever gets a second contributor with recurring
handoffs, a real external dependency, or material security/privacy risk,
promote from Base-Compact toward the fuller profile (add
`docs/architecture/`, `docs/requirements/`) rather than stretching this
lighter layer past what it's built for.
