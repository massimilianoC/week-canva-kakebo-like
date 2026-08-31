# 0002 — Version every localStorage record, migrate forward on read

> Stability: **stable**. Changes only via a superseding ADR.

- **Status:** accepted
- **Date:** 2026-08-31
- **Deciders:** Massimiliano Camillucci
- **Affected docs:** `AGENTS.md` §10
- **Affected milestones/slices:** M0.9 (this slice)

## Context

The digital companion has no server and no API — so the boundary that
`localStorage` normally hides behind a typed HTTP schema doesn't exist here.
But there **is** a real boundary: the shape of `twtf:settings` and
`twtf:week:<key>` JSON blobs, written by one version of `app/index.html` and
read back — possibly weeks or months later, possibly after the file has been
edited — by a different version of the same code. Without a version marker,
a future field rename or restructure silently corrupts every user's saved
week the next time they open the page, with no way to detect or migrate it.

This is the one non-negotiable pillar (forward-compatible schemas, §2 rule
10 of the SFAD pattern this project's governance borrows from) that maps
directly onto a localStorage-only app, unlike the server-oriented pillars
(idempotency keys, job orchestration, server-side RBAC, audit trail,
client-side secrets) which this ADR explicitly declines to adopt — see
`AGENTS.md` §10.7 for that list and the reason each one is out of scope.

## Options considered

1. **No version field; treat every stored blob as "whatever shape today's
   code expects."** What already existed before this ADR. Works until the
   first breaking change to `weekData` or `settings`, then fails silently.
2. **A `schemaVersion` integer on every persisted record, with a migration
   function run on load that upgrades old records to the current shape
   before the app touches them.** Mirrors how the printed sheet treats
   physical dimensions as a contract — the shape of saved data is a contract
   too, just one enforced in code instead of in `mm`.
3. **Full migration framework (versioned migration chain, dry-run,
   rollback).** Overkill for a single-user local app with, realistically,
   one or two schema changes a year.

## Decision

**Option 2.** Every object written to `localStorage` under `twtf:settings`
or `twtf:week:<key>` carries a top-level `schemaVersion` integer. On read,
`migrateSettings()` / `migrateWeek()` upgrade older (or absent — pre-ADR
records with no field at all count as version 1) records to the current
version before the rest of the app sees them, and the upgraded shape is
written straight back so the migration runs at most once per record.

## Consequences

- **Positive:** a future field rename/restructure is a new migration step,
  not a silent data-loss incident; a returning user's saved week never just
  vanishes because the code moved on without them.
- **Negative:** every future change to the persisted shape requires writing
  and testing a migration step, not just changing the shape in place — small
  discipline tax, paid once per schema change.
- **Risks:** a migration bug could corrupt data worse than doing nothing.
  Mitigation: migrations are additive/defensive (`Object.assign(defaults,
  parsed)` as the floor — an unrecognised or partially-broken record still
  degrades to defaults for missing fields rather than throwing).

## Implementation notes

- `CURRENT_SCHEMA_VERSION = 1` as of this ADR (the shape already shipped in
  M0 is retroactively declared version 1 — no migration needed yet, only the
  version field and the migration *mechanism* are new).
- `loadSettings()` / `loadWeek()` call `migrateSettings()` / `migrateWeek()`
  immediately after `JSON.parse`, before merging with defaults.
- When the shape changes next: bump `CURRENT_SCHEMA_VERSION`, add one `if
  (data.schemaVersion < N) { …transform…; data.schemaVersion = N; }` step per
  version jump inside the relevant migrate function — never rewrite history,
  only add forward steps.

## Changelog

- 2026-08-31 — accepted; `schemaVersion` field and migration scaffolding
  added to `app/index.html`.
