# Plan: FaithfulnessChecker crashes on a context chunk with `text: None` (#60)

## Diagnosis

From my repro comment: `.get("text", "")`'s default only applies when the
key is missing. `{"text": None}` has the key, so `.get()` returns `None`,
and `" ".join([None, ...])` raises the `TypeError` at
`faithfulness_checker.py:38`. Control run (key missing entirely) returns
`0.0`, no crash — confirms the bug is specific to a present `None`.

## Scope

In: `FaithfulnessChecker.check()`, `faithfulness_checker.py:38`.

Out: `relevance_scorer.py:32` (same pattern, not reported, not reproduced);
issue #59's scoring bug in the same file.

## Files

- `rag/evaluator/faithfulness_checker.py` — fix line 38.
- `tests/unit/test_faithfulness_checker.py` — remove the `xfail` marker on
  `test_none_context_chunk_text`.

## Approach

```python
context_text = " ".join([chunk.get("text") or "" for chunk in context_chunks])
```

`or ""` normalizes `None` the same way a missing key already resolves.

## Test plan

```
$ python -c "from rag.evaluator.faithfulness_checker import FaithfulnessChecker; FaithfulnessChecker().check('Knows Python.', [{'text': None}])"
```
Before: `TypeError`. After: returns a float in `[0.0, 1.0]`.

```
$ pytest tests/unit/test_faithfulness_checker.py -k test_none_context_chunk_text -v
```
Before (xfail removed): fails with the same `TypeError`. After: passes.

Also: re-run the missing-key control (still `0.0`), full test file (no
regressions, #59 tests still xfail), `make lint`/`make typecheck`.

## Risks

- `or ""` also no-ops on an empty-string `text`; no behavior change.
- Haven't traced whether a `None`-text chunk occurs from real retrieval, or
  only from hand-built input like the issue's snippet.

## Deviations

None. The build matched the plan exactly: the one-line fix in
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
