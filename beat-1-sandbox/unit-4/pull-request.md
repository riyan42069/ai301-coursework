# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/240

**Branch**

fix/60-faithfulness-none-text

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. 14/20 (full run: every reject package agreed, but the six clear accepts pkg-02, 05, 08, 11, 13, 16 were rejected)
2. 8/10 (partial `--only` run on the six failing accepts plus canaries: pkg-08 still rejected and pkg-20 was wrongly accepted)
3. 19/20 (final full run, saved as `eval-run.txt`: "agreement: 19/20 scored items  (bar: 18/20: PASS)")

**Package analysis**

pkg-20 (ghostty-org/ghostty#13604, category standards-wall). The gold label is reject. The
gold note reads: "excellent bounded fix implementing the maintainer-authored diagnosis with
decisive mode-2031 evidence, but the description contains no AI-use disclosure and ghostty's
stated policy requires disclosing all AI usage". My rubric rejects it, and the saved run shows
`pkg-20  standards-wall  reject  reject   yes`. It rejects because of the check "Standards:
AI-use disclosure": the repo-facts block says "All AI usage in any form must be disclosed",
and the description has no disclosure sentence, so that required check fails however good the
fix is. My rubric reads it this way because of the instruction "Treat every package as
AI-assisted work". The package is the reason that check exists on its own. In the 8/10 run
pkg-20 was wrongly accepted, because the disclosure ask sat inside a broader "stated policy"
check and the grader missed it. Splitting it into its own required check fixed it.

**Check rationale**

The check, exactly as it reads in `tools/pr-precheck/rubric.md`:

| Repo's own checks were run | The test-evidence section, read against the checks the repo or the plan's test plan names (CONTRIBUTING, PR template, CI config, the plan's "run X") | The test suite and any command the plan's test plan names are stated with their specific command and outcome (for example "`pytest tests/ -q` passes (1462 passed)" or "`go test ./...` passes"). Formatting, lint, and regenerate-type checks the repo's template lists count when the description attests them in its requirements list (for example "code formatted; cheatsheets regenerated, no diff"). Or the description says honestly which weren't run and why. Output dumps are not required. A bare "tests pass" or "all tests pass" with no command named, or a required check never mentioned, fails | required |

It reads this way because of what I rejected. My first version required the repo's check
output to be shown, and in the 14/20 run it failed five clear accepts (pkg-02, 05, 11, 13, 16)
that state only "`pytest tests/ -q` passes (1462 passed, 12 skipped)". I loosened it to a named
command plus its outcome. A bare "tests pass" still fails, which keeps pkg-04 and pkg-14 as
rejects. The formatting sentence came from the 8/10 run, where pkg-08 attested "code formatted"
in its requirements list and was still rejected.

**Trade-offs**

Loosening "Repo's own checks were run" means the rubric no longer verifies that a check ran at
all. It accepts any package that names the command and an outcome, so a fabricated "`pytest`
passes (1462 passed)" would pass this check. I accept that miss because the other required
checks still read the diff itself (the plan fidelity checks and "Test evidence is observable").
I checked the cost in the saved run: the not-tested rejects pkg-04, pkg-07, pkg-10 and pkg-14
all still reject (not-tested 4/4), and the clear accepts went from 1/7 to 6/7. The one remaining
miss is a clear accept, so the tool errs toward holding a good PR, not toward submitting a bad one.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
