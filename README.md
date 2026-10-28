# S01E03 — #4471 is three problems

The 412 reports merged into #4471 hold three problems:

| problem | reports | issue |
|---|---|---|
| parser reads dates month first | 374 | #1 |
| booking-v3 form hint says MM/DD/YYYY | 33 | #2 |
| invite day one day off | 5 | #3 |

- Page (.svx): [`src/routes/kb/incidents/4471-split/+page.svx`](src/routes/kb/incidents/4471-split/+page.svx), with its component and logic under `src/lib/kb/`
- The page as the desk sees it: [`rendered/4471-split.html`](rendered/4471-split.html)
- Analysis and method: [`analysis.md`](analysis.md)
