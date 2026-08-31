# CLAUDE.md — project instructions

Read [AGENTS.md](AGENTS.md) before changing this repository. It is the full,
authoritative contract; these notes keep the essentials visible to Claude Code.

This repository has **two coexisting products** — do not blur their rules.

## `index.html` — the printable A3 sheet (AGENTS.md §1a–§9)

- Single, self-contained print document — no JavaScript, dependencies, build
  step, external assets, or generated source. This is permanent, not a
  starting point to graduate away from.
- The sheet must remain exactly one A3 landscape page (420 × 297 mm). Preserve
  all print invariants and verify the dimensions, row height, fonts, tokens, and
  one-page print preview after every `index.html` change.
- Use physical CSS units: `mm` for lengths and `pt` for type that reaches paper.
- The source is English and monolingual. Translate every user-visible string
  together when localising; do not introduce a runtime locale switch or JSON
  string catalogue.
- Keep route names synchronised between section 02 and the matching log rows.
- After a visual change, regenerate `docs/preview.png` and
  `examples/the-week-that-fits.pdf`. The PDF must be blank, one page, and use
  the A3 MediaBox documented in `AGENTS.md`.
- Update `README.md` for user-facing changes and `docs/CUSTOMIZE.md` for
  customization changes. Do not edit `LICENSE`.

## `app/index.html` — the digital companion (AGENTS.md §10)

- A separate, fillable, local-first digital version. JavaScript, `localStorage`,
  and a live IT/EN language toggle are allowed **here only** — none of that
  belongs in `index.html`, and this file's existence is not a reason to relax
  `index.html`'s rules "to keep things consistent."
- State lives in `localStorage` only: no cookies, no server, no network call,
  no analytics, ever. That privacy promise is stated in the page's own footer
  — verify it still holds (DevTools Network tab empty) after any change.
- The four life-area names/tags are user-editable at runtime (⚙ settings or
  in-place) and must keep propagating automatically from section 02 into the
  section 03 log rows — that sync is the renderer's job, not the editor's.
- Every UI string needs both an `I18N.en` and an `I18N.it` entry — no
  half-translated state after adding or changing text.
- "Export filled PDF" is `window.print()` against the live `.sheet` DOM, not a
  PDF library — keep the `@media print` rules that hide the toolbar/modals and
  strip input/textarea chrome so the export reads as ink, not form controls.
- Don't pre-build the roadmapped-but-not-yet-needed stuff (SQLite/OPFS storage,
  OCR ingestion, scoring/analytics engine) — see AGENTS.md §10.5 for the intent
  and why that's deliberately out of scope until a step is actually underway.
