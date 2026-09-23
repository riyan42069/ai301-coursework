# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/60

**Verdict output**

```
I'm hitting two blockers that need your help... (evidence gathering complete)

Grading summary

Shared checks (same for all three issues):
- Repo is alive: Last commit Sep 16, 2026 (7 days before today Sep 23), not archived → pass
- Contribution rules allow this: docs/CONTRIBUTING.md has no mention of AI; no AI policy file found → pass

---
Issue #60 — Faithfulness checker crashes when context chunk has text: None

| Check                          | Grade | Evidence                                                                                      |
|---------------------------------|-------|------------------------------------------------------------------------------------------------|
| Repo is alive                   | pass  | Last commit Sep 16, 2026; not archived                                                        |
| Sized for a newcomer            | pass  | One bounded bug: fix .get() None-value handling in check() method                             |
| Nobody's already on it          | pass  | No assignee, no linked PRs, no comments                                                       |
| Contribution rules allow this   | pass  | docs/CONTRIBUTING.md silent on AI                                                              |
| Reproducible (preferred)        | pass  | Exact repro snippet + TypeError message + named failing test test_none_context_chunk_text     |
| Newcomer guidance (preferred)   | pass  | Labels: good first issue, tier-1; exact repro code; file test_faithfulness_checker.py named   |

Verdict: ACCEPT (4/4 required pass, 2/2 preferred pass)

---
Ranking by fit (all three accepted)

All three pass and all three are Python. Ranked by your profile — Python-focused, wants
codebase-reading practice, wants real workflow experience:

1. #60 (top pick) — The root cause is already pinpointed, a failing test is already
   written and named, and the fix is a Python one-liner. It puts you inside real
   application logic (FaithfulnessChecker.check()) and gives you a ready-made test
   scaffold to learn from. Maximum codebase-reading value with minimum ambiguity about
   what "done" looks like.
2. #36 — Writing a net-new test forces you to read and follow the existing test patterns
   in the FastAPI backend, which builds the unfamiliar-codebase skill.
3. #73 — The most accessible entry point (pure docs/config alignment, no code logic to
   read), but gives the least Python practice.

[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/60",
    "checks": [
      {"name": "Repo is alive", "grade": "pass", "evidence": "Last commit Sep 16, 2026 (7 days ago); no archived banner found"},
      {"name": "Sized for a newcomer", "grade": "pass", "evidence": "Single bounded bug: fix dict.get() None-value handling in FaithfulnessChecker.check(); one root cause, one outcome"},
      {"name": "Nobody's already on it", "grade": "pass", "evidence": "No assignee, no linked PRs, no comments"},
      {"name": "Contribution rules allow this", "grade": "pass", "evidence": "docs/CONTRIBUTING.md has no AI mention; no AI_POLICY.md found"},
      {"name": "Reproducible", "grade": "pass", "evidence": "Exact repro snippet + TypeError message + named failing test test_none_context_chunk_text already in the test file"},
      {"name": "Newcomer guidance", "grade": "pass", "evidence": "Labels: good first issue, tier-1; repro code given; test file tests/unit/test_faithfulness_checker.py named"}
    ],
    "verdict": "accept"
  }
]
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. Smoke run, `--limit 3`: 3/3
2. Full run with my first rubric (4 checks, no claim or policy check): 13/20 —
   below the bar, and the policy category was 0/1
3. Added "Unclaimed" and "Contribution policy" checks, re-ran the 7 issues that
   were wrong with `--only`: 5/7
4. Reworded the rubric to be plainer/less legalistic, re-ran the same 7: 5/7
   (different 2 wrong this time)
5. Tightened "Sized for a newcomer" to ignore a reporter's optional "additional
   suggestions," re-ran the 2 that check touched: 1/2
6. Added the "one bug with two causes is still one task" and
   "hard ≠ unbounded" fixes, re-ran all 7: 7/7
7. Re-ran the other 13 to make sure nothing broke: 13/13
8. Full run, `--save-run eval-run.txt`: **20/20 (bar: 18/20: PASS)** — the run in
   `eval-run.txt`

**Issue analysis**

`issue-19` (zxcalc/zxlive#517). My rubric says **accept**, gold says **accept**
too, but it took a few tries to get there. The issue names two causes of one UI
freeze, plus a list of "additional suggestions." Early on, my rubric read the two
causes as two separate jobs and rejected it as too big/too hard for a newcomer.
Really it's one bug with two named causes — still one task — and the suggestions
were just extra ideas, not required work. Once I said that plainly in the check,
it passed like it should.

**Check rationale**

> Nobody's already on it | The issue's linked pull requests, assignees, and
> comment thread | Pass if there's no assignee, no currently open PR tied to this
> issue, and no "I'll take this" / "working on it" comment sitting unanswered. An
> open linked PR or an assignee is an automatic fail, comment or not. A closed,
> unmerged PR just means someone tried and stopped — that alone doesn't fail this
> check. | required

My first rubric didn't have this check at all, which is exactly why it missed the
"claimed" category. An issue could look perfectly alive and well-scoped and still
have someone already working it. I added this straight from the evidence guide's
"is anyone already on it" section so a claimed issue can't slip through anymore.

**Trade-offs**

I made a closed, unmerged PR *not* count against an issue — someone tried and
stopped reads as "open again," not "claimed." That means this check alone won't
catch an issue that's already been quietly abandoned once. I'm okay with that gap
because "Sized for a newcomer" catches the worse version of it (two or more
closed PRs in the history). `issue-15` shows this in practice: it has two closed
PRs and gets rejected, but by the sizing check, not this one. A single abandoned
PR would still pass "Nobody's already on it."

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. I'm strongest in Python and don't have a ton of free time outside my IT support
   job, so a small, already-diagnosed bug fit well — no guesswork about what to do.
2. The verdict correctly confirmed it's alive, unclaimed, and allowed by the repo's
   contribution rules. What I weighed on top of that is fit, not correctness: this
   bug forces me into real application code instead of just docs, and there's
   already a named test to learn from.
3. Low difficulty. The cause and the fix location are already known, so most of my
   time will go into getting the project running locally and understanding
   `FaithfulnessChecker.check()` well enough to fix it safely.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
