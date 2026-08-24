# CLAUDE.md — project instructions

Read [AGENTS.md](AGENTS.md) before changing this repository. It is the full,
authoritative contract; these notes keep the essentials visible to Claude Code.

- The project is a single, self-contained `index.html` print document — no
  JavaScript, dependencies, build step, external assets, or generated source.
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
