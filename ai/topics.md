# Topic Tracker

Purpose: before writing a new quiz, check this file and pick something that
hasn't been touched recently (or at all), instead of repeating the last
theme/category. Update it after each new day is added.

## Format A days (weeks 1–3, legacy 15-question rotation)

These didn't have a single "theme" per day beyond a broad category:

| Date | Week | Category |
|---|---|---|
| 2026-10-01 | 1 | mixed basics (no fixed category yet) |
| 2026-10-02 | 1 | mixed basics (no fixed category yet) |
| 2026-10-05 | 2 | numbers & basics |
| 2026-10-06 | 2 | strings & words |
| 2026-10-07 | 2 | lists & sets |
| 2026-10-08 | 2 | matrices, vectors & stats |
| 2026-10-09 | 2 | dates, dicts, tables & number theory |
| 2026-10-12 | 3 | numbers & number theory |
| 2026-10-13 | 3 | strings & words |
| 2026-10-14 | 3 | lists & sets |
| 2026-10-15 | 3 | matrices, vectors & stats |
| 2026-10-16 | 3 | dates, dicts, tables & parsing |

## Format B days (weeks 4–5, current standard — one dataset, 10 functions)

| Date | Week | Theme | Dataset shape | Category |
|---|---|---|---|---|
| 2026-10-19 | 4 | trade blotter analytics | single table | single-table analytics (finance) |
| 2026-10-20 | 4 | nested dictionaries | dict of dicts | nested structure |
| 2026-10-21 | 4 | joining tables | 2 tables | multi-table join |
| 2026-10-22 | 4 | time-series readings | single table, irregular stream | time-series |
| 2026-10-23 | 4 | parsing raw logs | raw string list → table | text parsing → table |
| 2026-11-02 | 5 | restaurant tab analytics | single table | single-table analytics (retail) |
| 2026-11-03 | 5 | nested lists (student grades) | dict of lists | nested structure |
| 2026-11-04 | 5 | joining across three tables | 3 tables | multi-table join |
| 2026-11-05 | 5 | quotes, trades & as-of joins | 2 tables, asof | time-series / asof join (finance) |
| 2026-11-06 | 5 | parsing a clickstream | raw string list → table, funnel | text parsing → table |

### Observed rotation

Weeks 4 and 5 both cycled through the same 5 category slots in the same
order: **single-table analytics → nested structure → multi-table join →
time-series → text-parsing-to-table**, each time with a different domain
(finance → retail, warehouse → grades, 2-table HR → 3-table
customers/products/orders, sensor stream → asof quotes/trades, server logs →
clickstream).

For week 6+: either keep rotating these 5 slots with a new domain each time,
or break the pattern and reach into the untouched categories/techniques
below so the series doesn't get formulaic.

## Domains used so far (don't reuse until the pool below is thin)

finance/trades, warehouse/stock, HR (employees/departments), IoT sensors,
server logs, restaurant orders, school grades, e-commerce (customers/
products/orders), market quotes/trades (asof), web clickstream.

## Not yet touched — ideas pool

Domains:
- order book / market depth (multiple price levels per side)
- flights / scheduling (delays, connections)
- sports stats (teams, players, games, standings)
- banking transactions / fraud-flag style detection
- social graph / followers (adjacency list, simple traversal)
- project/timesheet tracking (people × projects × hours)
- delivery/logistics routes (stops, distances, time windows)
- chat/support tickets (messages, response times, SLA)
- inventory reordering (stock levels, thresholds, reorder points)

q-language techniques not yet deliberately exercised as the focus of a day
(weeks 4–5 lean heavily on `select`/`exec`/`by`/`fby`/joins — these would make
good standalone themes):
- functional forms of select/exec/update (`?[t;...]`, `![t;...]`)
- `each`, `each-left`/`each-right`/`each-prior`, `peach` semantics
- `scan`/`over` (`\`/`/`), recursion, fixed-point iteration
- vector conditionals: `?`, `within`, `in`, `bin`
- enumerations / symbol interning (`sym?`, `` `sym$ ``)
- functional amend (`@`, `.`, `amend at`)
- windowed joins (`wj`, `wj1`) beyond simple asof
- splayed/partitioned table basics, attributes (`` `s# ``, `` `g# ``, `` `p# ``)
- string handling: `like`, regex-ish matching, `ss`/`ssr`
- error handling / protected evaluation (`@`/`.` with error trap, `signal`)
- IPC basics (`hopen`, sync vs async messages) — may be out of scope if quizzes
  must run standalone
- date/time arithmetic edge cases (month-end, DST-adjacent, `` `month$ ``)
- random sampling (`?`, `deal` vs `roll`) and simulation-style questions
