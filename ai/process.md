# Daily q Quiz — Process

This document describes how this repo's daily kdb+/q quizzes are structured, and
the process for generating new ones. It's derived from what's actually in
`week-1` … `week-5` (weeks 4–5 are the current/preferred format).

## Layout

```
week-N/
  YYYY-MM-DD.md   <- the quiz (questions + expected output)
  YYYY-MM-DD.q    <- blank working file, same base name, to solve the quiz in
```

- Each `week-N` folder holds one working week (Mon–Fri), 5 quizzes.
- Every `.md` has a matching `.q` with the identical base filename.
- `hub_quizzes/` is a separate scratch/notes file, not part of the daily series — ignore it for this process.

## Two formats

### Format A — "quick basics" (weeks 1–3, legacy)

- 15 small, independent questions per day, no shared dataset.
- Each solution is expected to fit in 1–3 lines.
- Questions are answered as bare expressions (e.g. `q)sq 1 2 3 4`), not
  necessarily wrapped in a named function.
- Weeks 2–3 rotate a fixed set of 5 daily categories Mon→Fri:
  numbers/number-theory, strings & words, lists & sets, matrices/vectors/stats,
  dates/dicts/tables/parsing. Week 1 predates that rotation (mixed basics).

This format is retired. New quizzes should use Format B below.

### Format B — "themed deep-dive" (weeks 4–5, current standard)

- One theme per day, built around a **single shared dataset** (a table, a
  dict, a dict-of-lists, a list of raw strings, etc.) introduced once at the
  top of the file.
- 10 questions, each digging a bit deeper into the same dataset than the last
  (difficulty ramps up across the set).
- **Every answer is a named function** taking the dataset (and/or other stated
  arguments), e.g. `vwapBySym trades` or `addTestScore[grades;`Bob;80]` —
  never a bare inline expression.
- Most solutions still fit in a handful of lines.

Use Format B for all new quizzes.

## MD schema (Format B)

```markdown
# q Daily Quiz — YYYY-MM-DD (Weekday)

Theme: <short theme name>. 10 harder challenges built around <one-sentence
description of the dataset>. Write each answer as a function that takes
`<mainVar>` (or the stated arguments) and returns the result. Most solutions
still fit in a handful of lines.

---

Assume this <table/dictionary/list> for every question today:

	q)<var>:<construction expression>
	q)<var>
	<console output of the var>

<optional 1-2 sentences explaining the shape/columns of the data>

---

### 1. <short question title>

<1-3 sentences describing what the function should do>

	q)<funcName> <mainVar>
	<expected console output>

---

### 2. ...
...
### 10. ...
```

Rules that matter (learned from the `no solutions` cleanup commit on
week-4/week-5):

- **No raw solving expressions anywhere in the `.md`.** The example line is
  always a call to the named function (`q)funcName trades`, `q)funcName[a;b]`),
  never the q expression that computes the answer (e.g. never
  `q)select vwap:size wavg price by sym from trades`). The `.md` shows what the
  function returns, not how to write it.
- Multi-arg functions use bracket call syntax in the example:
  `q)funcName[arg1;arg2;arg3]`.
- A question may introduce a small one-off value inline for its own example
  only (e.g. a `newOrder` row to test an edge case) — that's fine, it's not
  part of the shared dataset and doesn't go in the `.q` setup block.
- A later question may build on a value produced by an earlier question's
  function (e.g. "the remaining questions all work on `evTbl`" after Q1 builds
  it via `parseEvents`) rather than on the original raw dataset.
- Separate every section (intro, each question) with a `---` horizontal rule.

## .q schema

The `.q` file is a **blank working template**, not a solution file. For a
quiz `YYYY-MM-DD.md`, the matching `YYYY-MM-DD.q` contains:

1. The dataset setup — the exact assignment line(s) copied verbatim from the
   `.md`'s "Assume this ... for every question today" block (just the
   `var:construction` line, not the printed console output below it). If
   there are multiple tables/vars (e.g. `customers`/`products`/`orders`),
   include all of them, in the same order as the `.md`.
2. One blank line.
3. One empty function stub per question, **in question order**:
   `funcName:{}`
   — just the name and empty braces. No parameters filled in, no body, no
   partial logic, no comments carrying hints. The solver fills in both the
   signature and the implementation.

Example (`.q` for a quiz about student grades):

```
grades:`Alice`Bob`Carol!(85 90 78 92;60 65 70;88 95 91 85 90)

avgPerStudent:{}

studentWithHighestAvg:{}

bestScoreOverall:{}
```

Do **not**:
- Pre-declare a variable that only exists as the output of an earlier
  question's function (like `evTbl` in a log-parsing quiz) — it's created by
  calling that stub once solved, not declared up front.
- Leave any worked-out logic, partial attempts, or copy-pasted solutions in
  this file — if a day's `.q` already has solutions in it (from solving it
  live), that's the solver's own work product and shouldn't be scrubbed
  automatically; this rule is about what gets *generated* going forward.

## Topic rotation

Before writing a new quiz, read [topics.md](topics.md) and pick a theme that
hasn't been used yet, or that's been used least recently. After writing the
quiz, add an entry to topics.md so the rotation stays accurate.

## Step-by-step: adding a new day

1. Check [topics.md](topics.md) for themes already covered and pick a fresh
   one (new domain, or a new q-language technique not yet exercised).
2. Design one dataset (table / dict / dict-of-lists / raw string list) that
   can support 10 questions of increasing difficulty.
3. Write `week-N/YYYY-MM-DD.md` using the Format B template above — function
   names only in examples, never raw solving expressions.
4. Write the matching `week-N/YYYY-MM-DD.q` — dataset setup + 10 empty
   `funcName:{}` stubs, in order.
5. Append a row to topics.md recording the date, theme, and category tags.
6. Start a new `week-(N+1)` folder once the current one has 5 days.
