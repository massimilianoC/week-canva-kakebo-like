# app/ — Execution spec

> Stability: **evolving**. Update in the same change that moves a slice
> between states — never batch tracker updates into a separate "housekeeping"
> commit.

Slice IDs are `M<milestone>.<n>`. State machine (adapted for a solo,
no-PR-reviewer, no-backend project — see `AGENTS.md` §10.7 for why the
upstream methodology's `ready-backend` state and Gate A don't apply here):

```
planned → in-progress → qa-ready → done
            ↘ blocked ↗
```

- **qa-ready** = the declared check passes locally.
- **done** = qa-ready **and** the tracker + any touched `stable` doc are
  updated in the same change.

Closure rule for a slice (adapted from the upstream "3 gates" pattern down to
one, since there is no separate backend/frontend split here): the described
UI is reachable and usable end to end, the declared check has actually been
run (not just "should pass"), and any state persisted survives a reload.

---

## M0 — Digital companion, first usable version — **closed**

Retrofitted: all of M0 was implemented before this governance layer existed,
then documented here per the Pre-`src/` gate's retrofit rule. "Check" below
describes the verification actually performed (manual + Playwright-driven
browser checks against a local `python -m http.server`), not a hypothetical
CI step this project doesn't have.

| Slice | Goal | Check | Status |
|---|---|---|---|
| M0.1 | Auto-detect the current ISO week and pre-fill the seven day-dates | Loaded page on 2026-08-31, confirmed week 36/2026 and Mon 31/08 – Sun 06/09 rendered without user input | done |
| M0.2 | Persist all form fields to `localStorage`, scoped per ISO week | Filled fields, reloaded, confirmed values survived; confirmed no network requests fired (DevTools) | done |
| M0.3 | Settings modal (⚙): language toggle (EN/IT), writing-font choice, per-user route names/tags | Toggled language live, confirmed all `data-i18n` strings switched; changed a route name, confirmed propagation (M0.4) | done |
| M0.4 | Route rename propagates from section 02 into the matching section 03 log row automatically | Renamed "Music" → "Chitarra" via the on-sheet field, confirmed the log row label updated without a page reload | done |
| M0.5 | New-week detection: detect a stale filled week on load and ask before clearing | Seeded a stale, filled `2026-W20` record + mismatched `lastWeek` key, reloaded, confirmed the confirm modal appeared; confirmed "start new week" clears the sheet while the old week's data stays in `localStorage` and reachable via ‹ › | done |
| M0.6 | Export via `window.print()` against the live filled sheet, matching the printable sheet's A3 physical layout | Measured `.sheet` at exactly 420×297mm with `frame.scrollHeight` ≤ 297.5mm (no second page) after filling every section | done |
| M0.7 | Default writing font: legible and distinct from the mono label/legend typeface, not handwriting-styled; handwriting fonts demoted to selectable options | Switched default to Atkinson Hyperlegible; confirmed via `getComputedStyle` that filled fields render in it; kept Patrick Hand/Caveat/Shadows Into Light as ⚙ options | done |
| M0.8 | Custom-symbol field: cross-device picker, since no web API can force the OS emoji keyboard open | Added 🙂 popover with checkmark/arrow/emoji grid; clicked an entry, confirmed it wrote into the field and closed the popover | done |
| M0.9 | Every persisted record carries `schemaVersion`; migration runs on load | See ADR `0002-local-schema-versioning.md`. Manual check: cleared a `schemaVersion`-less record, reloaded, confirmed `migrateWeek()`/`migrateSettings()` backfilled `schemaVersion: 1` without throwing | done |
| M0.10 | Log-table day cells: free-text entry, visually clipped with an ellipsis rather than hard-capped at 2 characters | Typed a long sentence into a day cell, confirmed full string round-trips through `localStorage` (`weekData.log[row][col]`) while the on-screen box shows `…` | done |
| M0.11 | Symbol popover: highlight/enlarge the entry matching the currently-set custom symbol, so the active choice is visually obvious | Set the custom symbol to 🔥, reopened the popover, confirmed `button.selected` matched `🔥` and rendered larger/highlighted via CSS | done |
| M0.12 | Per-log-cell quick-glyph corner button (optional UI/UX): insert a legend glyph into a day cell without typing, without narrowing the cell's writing area | Added a `.cell-symbol-btn` per cell, invisible until hover/focus (`opacity:0` → `0.9`), opening a compact popover with the 6 legend glyphs + the user's custom symbol; clicked a glyph, confirmed it wrote into that cell's input; confirmed `.sheet` still measures exactly 420×297mm and hidden entirely under `@media print` | done |
| M0.13 | Log day-cell text right-aligned (was centered) — pairs with the corner glyph button and truncates predictably with the M0.10 ellipsis | Switched `.log-row .col-day input` to `text-align: right`; visually confirmed a long entry truncates from a consistent edge | done |
| M0.14 | Score (0–10) and mood-face selections zoom/emphasize the chosen entry, not just a border/background change | Added `transform: scale()` + z-index on `.selected` for both `.score-scale .box` and `.mood button`; confirmed via `getComputedStyle().transform` after clicking | done |
| M0.15 | Toolbar moved out of normal document flow to a fixed, unframed bottom-left cluster, so it costs zero vertical space above the sheet | Wrapped `.sheet` in `#sheet-viewport`; toolbar restyled as floating pill buttons (`position: fixed; left/bottom`); confirmed no overlap with the footer note via bounding-rect check | done |
| M0.16 | On-screen auto-fit: scale `.sheet` (CSS `transform: scale()`, capped at 1) to the viewport so it suits a landscape tablet or modest laptop window, not just a large desktop monitor — print/export always use the true, unscaled 420×297mm | Added `fitSheetToViewport()` + debounced resize listener; confirmed `scale(0.84)` at a 1366×1024 viewport with no horizontal scroll; confirmed `@media print` resets `transform: none` and the sheet still measures exactly 1587.4×1122.5px (= 420×297mm) under `page.emulateMedia({media:'print'})` | done |

## M1 — not started

No slices yet. The next slice gets planned (spec written, risk classified,
acceptance examples drafted) before it appears in this table — an empty M1
section is the correct state until that happens, not a gap to fill
preemptively.
