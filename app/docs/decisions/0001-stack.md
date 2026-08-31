# 0001 — Vanilla stack, no build step, localStorage only

> Stability: **stable**. Changes only via a superseding ADR.

- **Status:** accepted
- **Date:** 2026-08-31
- **Deciders:** Massimiliano Camillucci
- **Affected docs:** `AGENTS.md` §10, `app/docs/planning/ROADMAP.md`
- **Affected milestones/slices:** M0 (all)

## Context

`app/index.html` is the digital companion to a print-only A3 weekly planner
(`index.html`, repo root) that is governed by a hard "no JavaScript, no build
step, no dependencies" contract (`AGENTS.md` §1a). The companion needed real
interactivity — persisted form state, live language switching, a settings UI
— which that contract permanently forbids for the printable file. Rather than
weaken the printable file's contract, the companion became a second,
separately-governed file (`AGENTS.md` §10).

The question this ADR settles: given that JavaScript is now allowed, how much
tooling comes with it?

## Options considered

1. **Vanilla HTML/CSS/JS, single file, no build step, no framework, no npm.**
2. **A framework (React/Vue/Svelte) with a bundler (Vite).** Gives component
   structure and a dev server, at the cost of a `node_modules` tree, a build
   step, and dependency-update maintenance for a project whose print sibling
   explicitly rejects all of that.
3. **Do nothing / keep the companion as static as the printable sheet.**
   Not viable — it fails the actual requirement (persisted, interactive,
   settings-driven form).

## Decision

**Option 1.** `app/index.html` stays a single self-contained file: inline
`<style>`, inline `<script>`, vanilla DOM APIs, no framework, no bundler, no
`package.json`. State lives in `localStorage` only. The only external
resource is the Google Fonts stylesheet (already an accepted precedent in the
printable sheet). This keeps both products in the repo at the same
"download one file, open it" level of friction, and keeps the project
honestly sized for what it is — a single-user local tool, not a web app with
a deployment pipeline.

## Consequences

- **Positive:** no dependency-update burden, no build step to keep working
  across future Node/npm versions, trivially auditable (the whole app is
  readable top to bottom), consistent with the printable sheet's philosophy.
- **Negative:** no component reuse across files (state/render logic in
  `app/index.html` is hand-rolled, not componentized); refactors that would
  be mechanical with a framework are manual here.
- **Risks:** as the file grows, a single inline `<script>` block gets harder
  to navigate. Mitigation: keep functions small and grouped by comment
  section (state / rendering / week math / persistence / modals), per the
  soft 200-line-per-unit guidance this project borrows from SFAD's coding
  conventions (not a hard file-count limit, since there is deliberately only
  one file).

## Implementation notes

No action required — this ADR documents the choice already made when
`app/index.html` was created, per the Pre-`src/` gate's retrofit rule
(document existing decisions rather than block on a gate that predates the
governance layer).

## Changelog

- 2026-08-31 — accepted (retrofit, documenting the existing implementation)
