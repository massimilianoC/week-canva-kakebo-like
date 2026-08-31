# app/ — Roadmap

> Stability: **evolving**. Update at the close of each milestone or when the
> backlog order changes; no ADR required for reordering, only for scope that
> touches a `stable` doc (e.g. `decisions/0001-stack.md`).

Ordered by next valuable outcome for the person actually using this, not by
technical layer. Detailed slice-by-slice status lives in
[`EXECUTION_SPEC.md`](EXECUTION_SPEC.md); this file is the "what's next and
why," not the "what's the status of each checkbox."

## Current focus

**M0 is closed.** The companion exists, is usable end to end, and is
governed. No milestone is in progress right now — M1 has not been started
and nothing in it is scoped yet. The next real work should begin by writing
a feature spec under `app/specs/` (copy `specs/feature/spec.md`), not by
opening `app/index.html` and typing.

## M0 — Digital companion, first usable version (closed 2026-08-31)

**Goal:** a person can open `app/index.html`, fill in a week digitally, and
get back a filled PDF — with nothing lost between sessions.

Retrofitted into this governance layer after the fact (per the Pre-`src/`
gate's retrofit rule) — the code shipped first, this roadmap documents it
second. See `EXECUTION_SPEC.md` for the slice-by-slice breakdown.

## M1 — Backlog (not started, not scoped)

These are the long-term directions already named in `AGENTS.md` §10.5 (the
project owner's stated intent), listed here **only as candidate outcomes**,
not as committed scope. None of these gets built until it has its own spec
under `app/specs/` with acceptance examples — writing code against an
un-specced roadmap line is exactly what this governance layer exists to
prevent (see `AGENTS.md` §10.7, pillar 1).

| Order | Outcome | Status | Notes |
|---|---|---|---|
| 1 | *(none yet — pick the next real outcome and spec it)* | — | — |
| — | Local structured storage beyond `localStorage`'s per-origin cap (SQLite via `sql.js`/OPFS) | unscoped | Only once `localStorage` is actually a constraint, not preemptively |
| — | Scoring / trend feedback across saved weeks | unscoped | Depends on there being enough saved weeks to make trends meaningful |
| — | OCR ingestion of photographed paper sheets | unscoped | Closes the loop between the two products; furthest out |

## Promotion note

If this project ever gets a second contributor (human or a coding agent
working unsupervised across sessions on a schedule), material external
dependency, or recurring architecture churn, promote from Base-Compact to a
fuller SFAD profile (add `docs/architecture/`, `docs/requirements/`) per the
signals in `AGENTS.md` §10.7. Until then, this file plus `EXECUTION_SPEC.md`
is the whole planning layer, by design.
