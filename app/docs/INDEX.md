# app/docs — index

> Stability: **evolving**. Update this list whenever a doc is added, removed,
> or promoted/demoted between `draft` / `evolving` / `stable`.

Governance layer for the digital companion (`app/index.html`), adopting a
**Base (SFAD Compact) profile with three Full-profile borrowings** — slice
tracking, MADR-format ADRs, and a session handoff record — scaled down for a
solo developer working through coding agents, no server, no CI, no second
human reviewer. The full rationale for what was adopted vs adapted vs
dropped is in `AGENTS.md` §10.7; read that first.

Reading order before starting work on the companion (mirrors `AGENTS.md` §10
+ this list):

1. `AGENTS.md` §10 (and §10.7 specifically for this governance layer)
2. `app/docs/planning/ROADMAP.md` — current focus, backlog
3. `app/docs/planning/EXECUTION_SPEC.md` — the active milestone's slices
4. the relevant spec under `app/specs/` for the slice being worked
5. `app/dev/SESSION_HANDOFF.md` — what the last session left off with

## Documents

| Doc | Stability | Purpose |
|---|---|---|
| [`planning/ROADMAP.md`](planning/ROADMAP.md) | evolving | Ordered outcomes, current focus, backlog |
| [`planning/EXECUTION_SPEC.md`](planning/EXECUTION_SPEC.md) | evolving | Per-milestone slice tracker |
| [`decisions/0000-template-adr.md`](decisions/0000-template-adr.md) | — | Blank MADR template, not a real decision |
| [`decisions/0001-stack.md`](decisions/0001-stack.md) | stable | Vanilla JS, no build step, localStorage only |
| [`decisions/0002-local-schema-versioning.md`](decisions/0002-local-schema-versioning.md) | stable | `schemaVersion` field + forward migration on every persisted record |
| [`../specs/feature/spec.md`](../specs/feature/spec.md) | — | Blank feature-spec template, copy per new feature |
| [`../dev/SESSION_HANDOFF.md`](../dev/SESSION_HANDOFF.md) | — | Short-lived; current session state, not a backlog |

No `docs/architecture/`, `docs/requirements/`, or `docs/analysis/` folder yet
— this project is small enough that `AGENTS.md` §10 (identity, boundaries,
SSoT) and `decisions/0001-stack.md` (tech choices) cover what those would
hold. Add them if/when the companion outgrows that (see the promotion
signals in `AGENTS.md` §10.7).
