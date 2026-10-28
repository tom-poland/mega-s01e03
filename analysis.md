# S01E03 "what it shows": analysis of #4471

Source: kb export (`kb/support-kb/`), `reports-4471.json` (427 rows, 412 unique ids), `tenants/*.json`, `desk-notes.json`, and the rules in `causes.ts`.

## Result: 412 reports, three problems

| problem | reports | what it is |
|---|---|---|
| `parser` | 374 | Shared parser reads dates month first; all 11 tenants write day first. This is what the queued fix covers. |
| `hint` | 33 | `booking-v3` tenants (alder.physio, copper.studio, pale.harbour) show `MM/DD/YYYY` under the date field; `form` channel only. The parser fix does not touch it. |
| `oneDayMove` | 5 | Month and day number right, weekday one day off (R-41202, R-41209, R-41236, R-41344, R-41620). Cause unknown; hints at a time zone. |

374 + 33 + 5 = 412.

## How the split was made
1. De-duplicate by id: 427 rows → 412 (the export pages overlap).
2. Day-first check on the tenant locale (`Intl`, as `causes.ts` does): all 11 tenants are day-first.
3. `form` channel + tenant on `booking-v3` (hint `MM/DD/YYYY`) → `hint`; else → `parser` (the rule in `causeOf`).
4. The five ids in `desk-notes.json` are not a month/day swap: their bodies show a one-day shift. Moved to `oneDayMove`.
5. Check by text: 403 bodies hold two dates that are a true day/month swap; 9 do not. Five of those nine are the one-day moves; four (R-41290, R-41417, R-41514, R-41615) are vague and stay in `parser` (no evidence against it).

## Traps found
- `intakeTag` is a guess from the browser: 15 of 33 `hint` reports are tagged `parser`, 23 of 374 `parser` reports are tagged `hint`.
- Decision 0001 merges on subject; 12 subject lines cover all 412 reports, so they cannot tell causes apart. That is why one ticket hid three problems.
- The old #4471 page and `CauseTable` count `parser` as 379 (it keeps the 5 one-day moves under it) and call it "one cause, fix queued". The kb page `incidents/4471` stays unchanged (it is the desk's page); the new page `incidents/4471-split` adds the split.
- The page's number are computed in a component (`ProblemSplit`) not typed in text, per the kb conventions.

## Page
Signed with the `ticket` from start-challenge as `signature:`, ends with `<RenderStamp {signature} />`. Rendered with a local Vite + mdsvex build: desk ref DR-F35V-NS99.
