# Feature spec: <FEATURE_NAME>

> Status: **draft** | approved | implemented

- **Owner:** <you, or the agent driving this slice>
- **Target slice:** M<n>.<m> (add to `app/docs/planning/EXECUTION_SPEC.md`
  once this spec is approved — a slice doesn't get a table row before it has
  a spec)

## Outcome and boundary

- **User outcome:** <one sentence — what can the person filling in their week
  do after this ships that they couldn't before?>
- **In scope:** …
- **Out of scope:** <be explicit — this is what stops scope creep mid-slice>

## Requirements

Plain language is fine for anything unambiguous. Reach for EARS
(`WHEN <event> IF <condition> THEN THE SYSTEM SHALL <outcome>`) only where
timing, ordering, or a conditional makes plain language actually ambiguous —
it's a precision aid, not mandatory ceremony.

- `REQ-1`: …

## Acceptance examples

Cover the observable happy path, at least one boundary/failure path, and
anything with a privacy or accessibility angle (this app's whole premise is
"nothing leaves the device" — a feature that quietly breaks that promise
needs a negative example here, not just a happy path).

```gherkin
Given <precondition>
When <action>
Then <observable outcome>
```

```gherkin
Given <failure precondition>
When <action>
Then <safe failure outcome — data isn't lost, nothing silently breaks>
```

## Verification

- **Baseline:** open in a real browser (or via `python -m http.server` +
  Playwright, same as M0's checks — see `EXECUTION_SPEC.md`), exercise the
  acceptance examples above, confirm no network request fires (DevTools
  Network tab — the no-server promise from `AGENTS.md` §10 is load-bearing).
- **Risk-specific:** classify per `AGENTS.md` §10.7's adapted risk tiers.
  Anything touching the `localStorage` schema also needs a migration check
  per ADR `0002-local-schema-versioning.md` — confirm an old-shape record
  still loads without data loss.

## Decisions needed

| Question | Recommended option | Alternatives | Latest safe decision point |
|---|---|---|---|
| … | … | … | before implementation starts |

<!--
Delete this comment block once the spec above is filled in.
This file is the template — copy it to app/specs/<feature-id>/spec.md for a
real feature; don't edit this one in place except to improve the template
itself (that's a change to the template, worth its own commit message saying so).
-->
