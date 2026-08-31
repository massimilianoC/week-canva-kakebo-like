# Session handoff

Short-lived working memory for picking up mid-project across sessions/agents.
**Not a backlog** — durable facts belong in `app/docs/planning/ROADMAP.md`,
`EXECUTION_SPEC.md`, or an ADR under `app/docs/decisions/`, not here. This
file gets overwritten each session, not appended to. Never record secrets,
tokens, or real filled-in personal planning data here.

- **Committed or ignored:** this file **is committed** (declared in
  `AGENTS.md` §10.7). Since this is a solo project with no throwaway
  scratch/CI environment, a committed handoff is more useful than a
  gitignored one — it survives a fresh clone.

## Objective (as of 2026-08-31)

M0 (first usable version of the digital companion) is closed and retrofitted
into this governance layer. No M1 slice is in progress.

## Current state

- `app/index.html` is feature-complete for M0 — see
  `app/docs/planning/EXECUTION_SPEC.md` for the slice-by-slice list.
- Governance layer (`app/docs/`, `app/specs/`, this file) was just created,
  adopting SFAD's Base-Compact profile + 3 Full-profile borrowings, per
  `AGENTS.md` §10.7. This is the first session operating under it.
- `AGENTS.md`, `CLAUDE.md`, `README.md` at repo root were updated earlier in
  this project to document the two-product (`index.html` print / `app/`
  digital) split.

## Changed files this session

- `app/index.html` — created, then iterated: default font → Atkinson
  Hyperlegible with larger sizes across fill fields; log row height increased
  (borrowed from the review section's flex share); 🙂 symbol picker added;
  log day-cell `maxlength` removed in favour of CSS ellipsis truncation
  (full value still persisted); `schemaVersion` + migration scaffolding added.
- `AGENTS.md`, `CLAUDE.md`, `README.md` — two-product architecture documented
  (§10 in AGENTS.md).
- `app/docs/**`, `app/specs/feature/spec.md`, this file — new, this session.

## Evidence run

Manual + Playwright-driven browser checks against a local
`python -m http.server`, itemized per slice in `EXECUTION_SPEC.md`'s M0
table. No automated test suite exists yet (Base-Compact profile; see
`AGENTS.md` §10.7 for why "typecheck/lint/test/build" doesn't apply as-is to
this project and what the real baseline check is instead).

## Blocker

None. Nothing is in progress.

## Next safe action

Pick the next real outcome from `ROADMAP.md`'s M1 backlog (or a new one the
project owner names), write its spec under `app/specs/<feature-id>/spec.md`
from the template, get it to at least "draft reviewed by the project owner"
before writing implementation code — that's the whole point of adopting this
governance layer starting now rather than after M1 also ships ungoverned.
