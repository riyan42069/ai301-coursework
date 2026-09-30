# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

riyan42069

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/60#issuecomment-5921102603

Hi, I'd like to investigate this issue. I'll reproduce the TypeError in FaithfulnessChecker.check() using a context chunk where text is None, verify it against test_none_context_chunk_text, and follow up with my reproduction findings.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/60#issuecomment-5921243804

Confirmed: the `TypeError` matches the issue at the reported code path.

**Environment**
- Commit: `2f4e82f5`
- OS: Windows 11
- Python: `3.13.3`

**Reproduction**

```bash
python -c "from rag.evaluator.faithfulness_checker import FaithfulnessChecker; FaithfulnessChecker().check('Knows Python.', [{'text': None}])"
```

**Observed**

```text
TypeError: sequence item 0: expected str instance, NoneType found
```

The exception occurs at `faithfulness_checker.py:38` when `chunk.get("text", "")` returns `None` for a present `text` key, which is then passed into `" ".join(...)`.

**Control case**

When the `text` key is missing entirely, there is no crash and the method returns `0.0`. This confirms that `.get()`'s default only handles a missing key, not a key whose value is `None`.

I also ran:

```bash
pytest tests/unit/test_faithfulness_checker.py -k test_none_context_chunk_text --runxfail
```

It fails with the same traceback. `--runxfail` is needed because the test is currently marked `xfail`.

**Expected**

`check()` should return a float instead of raising, consistent with the behavior when the `text` key is missing.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Started with a quick 3-package sanity check on my first rubric draft — 3/3 agreement, so I
moved on to a full run. That first full run of all 20 came back at 17/20, and the
`disclosure` category was flat-out missed (0/1): pkg-20 got accepted even though ghostty's
repo-facts block says all AI use has to be disclosed and the comments said nothing about
it. pkg-05 and pkg-12 also disagreed, both on `steps-followable` — the reports gave every
value that mattered but in prose instead of a pasted file/script, and my check was reading
that as "guessing."

So I added a line to `repo-conventions` making silence on a stated disclosure requirement
an automatic fail, and loosened the `steps-followable` evidence-guide wording so a
referenced-but-not-pasted value still counts as stated. Re-ran just those three
(`--only pkg-05,pkg-12,pkg-20`): 1/3 — pkg-20 fixed, the other two still failing. Loosened
`steps-followable` again (covering values already quoted in the issue, and incidental
details that don't actually change whether the bug triggers) and re-ran the same three:
still 1/3, no movement on pkg-05/pkg-12.

Ran the full 20 again: 18/20, bar passed, every category matched. Then did one more
confirming full run and saved it as `eval-run.txt` — also **18/20, bar passed**. The
packages that disagreed shifted a bit between these last two runs (pkg-12 agreed this time,
pkg-03 and pkg-05 didn't, both on `repo-conventions`), which looks like grading variance on
borderline calls plus the known over-firing issue described below — not a new regression.

**Package analysis**

`pkg-20` (ghostty-org/ghostty#13604). Gold says **reject**, and in the committed run my
rubric agrees: **reject**. The repo's AI policy is strict — "All AI usage in any form must
be disclosed, stating the tool used and the extent of the assistance" — and the candidate's
claim and repro comments are genuinely solid (environment's there, steps check out, behavior
matches, nothing overclaimed), but neither one says a word about AI assistance. That's
exactly what my `repo-conventions` check is built to catch now: it fails the package on
missing disclosure alone, regardless of how good everything else is. Before I added that
rule, my rubric got this one wrong — it accepted pkg-20, because "follow the repo's stated
disclosure requirements" was vague enough that the grader judged the comment's overall
quality instead of actually checking whether a disclosure sentence existed.

**Check rationale**

Quoting the `repo-conventions` row from `rubric.md` as it's uploaded right now:

> The claim and repro follow the repository's stated contribution, formatting, disclosure,
> and communication requirements. If the repo-facts block states an AI-disclosure policy,
> this check fails unless the claim or repro comment contains an explicit disclosure
> statement — silence on disclosure is a fail even if every other convention is met

I bolted on the second sentence right after pkg-20 came back wrong — the original one-line
version left too much room for the grader to just vibe-check the comment instead of looking
for the actual missing sentence. I did think about narrowing it further, to only fire when a
policy specifically demands a tool-and-extent disclosure (instead of any AI-related policy
language at all), but I hadn't finished verifying that version when I had to lock in a run,
so it's still the broader clause. The cost of that is in the trade-off below.

**Trade-offs**

That same `repo-conventions` clause is a mixed bag. It's what gets `disclosure` right, but
it also fails pkg-03 (ripgrep) and, in the run I committed, pkg-05 (conda) — both of which
are gold `accept`. Neither of those repos actually requires disclosing AI use; they just say
comments need to be human-written and human-understood, which isn't the same thing, but my
clause can't tell the two kinds of policy apart, so it treats any AI-related policy language
as a disclosure requirement. I'm accepting that miss for now — the disclosure category only
has the one package this week and it matters more to get that right than to keep pkg-03/05,
which are just two of eight `clear-accept` packages the category still clears fine without
them (6/8). The narrower fix is drafted, just not re-verified with a canary run yet.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
