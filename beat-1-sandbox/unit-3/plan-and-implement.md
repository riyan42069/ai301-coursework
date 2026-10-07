# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

riyan42069

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/60#issuecomment-6047739018

Plan for #60, based on the reproduction above.

**Cause:** `chunk.get("text", "")` at `faithfulness_checker.py:38` only
substitutes `""` when the `text` key is *missing*. A chunk shaped
`{"text": None}` still has the key, so `.get()` returns the stored `None`,
and `" ".join(...)` can't join it. My control case (key missing entirely →
no crash, `score = 0.0`) isolates this to the present-but-`None` case
specifically.

**Change:** one line, in `rag/evaluator/faithfulness_checker.py`, coercing a
`None` text value to `""` the same way a missing key already resolves:

```python
context_text = " ".join([chunk.get("text") or "" for chunk in context_chunks])
```

No change to `check()`'s signature, return type, or scoring logic.

**Out of scope:** `relevance_scorer.py` has the same `.get("text", "")`
pattern, but #60 doesn't report a failure there and I haven't reproduced one,
so I'm leaving it alone. Issue #59's scoring-logic bug in the same file is
unrelated and I won't touch its `xfail` tests.

**Tests:** I'll remove the `xfail` marker from `test_none_context_chunk_text`
(it currently fails under `--runxfail` with the exact traceback from my
repro) and confirm it passes unmarked, run the missing-key control test to
confirm it's unaffected, and run the full file plus `make lint`/`make
typecheck`.

No promises beyond this plan yet — if review turns up something the repro
didn't show, I'll post that instead.

---

## Your branch

**Branch**

fix/60-faithfulness-none-text

**Evidence**

Deviations: None. The build matched the plan exactly: the one-line fix in
`faithfulness_checker.py:38` (`chunk.get("text") or ""`), and removing the
`xfail` marker from `test_none_context_chunk_text`.

Test plan re-run against the actual change:
- Repro snippet (`{'text': None}`): before, `TypeError: sequence item 0:
  expected str instance, NoneType found`; after, returns `0.0`, no crash.
- Control (missing key): `0.0` before and after, unaffected.
- `test_none_context_chunk_text` alone: fails under `--runxfail` before
  (same traceback); passes after the marker is removed.
- Full `tests/unit/test_faithfulness_checker.py`: 19 passed, 3 xfailed
  (the unrelated #59 tests, untouched).
- `ruff check` and `mypy` on the changed files: no issues.

No scope change, so no follow-up comment needed on the issue.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Single run, scoring all 20 packages: **18/20 scored items, bar 18/20: PASS**.
This matches the agreement line in the committed `eval-run.txt`.

**Package analysis**

`pkg-20`, category `thread-convention`. Gold label: **reject**. My rubric's
verdict: **accept** — a disagreement (`eval-run.txt` marks it `NO — graded
accept`).

My rubric (`rubric.md`) runs four required checks: `Diagnosis`, `Behavior
matches`, `Scope`, and `Test`. None of those checks reads the plan comment
against the issue thread's own conventions or prior discussion — they only
judge the plan's internal soundness (does the stated cause match the repro,
does the change stay in scope, does the test plan prove something). So a
plan that is technically sound — correct diagnosis, bounded scope, a real
falsifiable test — but that ignores or contradicts something the thread
itself already established (the `thread-convention` family this package is
built to test) passes all four of my checks and comes out `accept`, even
though gold says the thread-level problem should hold it. The miss isn't a
bug in any one check; it's a gap in coverage — nothing in `rubric.md` is
aimed at that failure family at all.

**Check rationale**

Quoting the `Test` row from `rubric.md` as it's uploaded right now:

> Passes if the test plan adds an automated test that exercises the repro
> scenario and would fail before the fix is applied

I wrote it this strict on purpose, rejecting a weaker version I considered —
"passes if the plan mentions running tests" — because that version can't
actually tell a real test plan from a plan that just gestures at testing
without describing anything that would fail pre-fix. Anchoring the check to
"exercises the repro scenario and would fail before the fix" forces the
pass condition to judge the test's relationship to the bug itself, not
whether a Test section exists or how it's formatted.

**Trade-offs**

The cost of the `Test` check's strictness, and of the rubric having no
check at all for thread/repo conventions, shows up together in this run:
`pkg-20` is exactly the case I accept my rubric will miss — a
`thread-convention` package can be internally sound (good diagnosis, good
scope, a real falsifiable test) and still get `accept` from my rubric when
gold says `reject`, because none of my four checks is built to catch a plan
that ignores what the thread already established. I'm accepting that miss
for now: the `thread-convention` family only cost one package this run, and
narrowing my existing checks further to also police thread history would
risk making `Diagnosis` or `Scope` fail sound plans for an unrelated reason
(over-firing), the same way the `repro-check` rubric's broadened
`repo-conventions` clause cost it two otherwise-correct `clear-accept`
packages. Everywhere else, the four checks held: `scope-creep` 4/4,
`unbuildable` 3/3, `wrong-cause` 4/4, and `clear-accept` 6/7 — so nothing
else moved as a result of keeping the rubric scoped to the plan's internal
soundness rather than adding thread-convention coverage this round.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
