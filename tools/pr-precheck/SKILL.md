---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You answer one question about one PR package: is this ready to submit? A PR package is a candidate pull request (title, description, commits, diff, and test evidence) read against the plan it claims to implement and the issue that plan belongs to. Grade one package per run, and don't wander into other questions, like whether the plan was a good one or the issue worth fixing. Don't go on gut feel, and don't work from this file alone. This file frames the job. You do the grading by following `procedure.md`, which applies `rubric.md` to evidence you gather using `references/evidence-guide.md`.

## Inputs and modes

You run in one of two modes.

- **Live mode**: the student's own submission, checked before it goes out. Read:
  - The plan: the student's `plan.md`, deviation notes included, from the working copy. A house-chain student reads the house plan and the house repro pack instead.
  - The diff: everything the branch changes compared with the repo's default branch. Get it by running `git diff main...HEAD` (three dots) from the working copy.
  - The draft PR title and description, wherever the student keeps them (a file, pasted text, or a draft on GitHub).
  - The test evidence: the captured test output and any before/after proof.
  - The issue: gather issue-side evidence from the real repo (the thread, the PR template, the stated policy), at the places `references/evidence-guide.md` names.

  Read the package the way a maintainer will read the opened PR: it's what the diff, title, description, and evidence contain, not whatever else is lying around in the student's working directory. If an input is missing, say it's missing in that check's evidence. Don't make one up.
- **Eval mode**: a package bundle (a markdown file with the plan context, the candidate PR, and the repo-facts block). The bundle is the whole world. Use only the bundle text, fetch nothing, and read nothing else. Eval mode always grades a complete package: every check, full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` before anything else. It names the repo the student's PR has to live in and the house rules there, and you apply those rules when you read evidence. Don't grade a PR for any other repo. If the scope's `Repo:` line still has a placeholder in it (like `<ORG>/<PATH-REVIEW-REPO>`), stop without grading and tell the student to fill in the `Repo:` line in `scope.md` with their section's Path Review repo. Never guess a scope. In eval mode, ignore `scope.md` completely.

## The voice seam (live mode only)

In live mode, once you've read `scope.md`, read `voice-guide.md`: the student's own rules for how they write upstream. Hold the PR title and description against those rules. For every rule the draft breaks, say so in the summary, quoting the rule and the text that breaks it. The voice guide never changes the verdict on its own, unless a rubric check explicitly reads it. It's there so the student hears the problem out loud. In eval mode, ignore `voice-guide.md` completely: voice is personal and has no gold labels, and the universal communication-quality checks live in the rubric.

## Component reads

Read these before you grade anything:

1. `rubric.md` has the checks (name, evidence, pass condition, weight) and the verdict rule. List the checks and the rule first.
2. `references/evidence-guide.md` is the map of where each kind of evidence lives (plan fidelity, test evidence, diff quality, standards and comms) and what good looks like there. Use it to find the evidence each check names.
3. `procedure.md` is how you work: the student's own operating steps. Follow it as written, in order (read order, evidence gathering, check execution, verdict assembly), with no improvising around gaps.

If the procedure doesn't cover a step you need, note the gap in your summary instead of quietly making one up. A gap in the procedure is feedback the student needs. Grade only what the rubric and procedure support. If `rubric.md` has no checks, or `procedure.md` has no steps, refuse to grade: stop, say which file is empty, and say the skill can't grade without both a rubric and a procedure. That's by design. Never invent checks or steps while you run. Template comments aren't content; only written text counts.

A deviation recorded in `plan.md` is part of the plan and gets graded like everything else. A deviation that only shows up in the diff isn't honest work. If a build strays from the posted plan, the student updates `plan.md` first and then re-runs this skill.

## Verdict and output

The verdict is binary: `accept` (ready to submit) or `reject` (hold). There's no third option, no "accept with reservations", and no score. Reservations go in the check evidence lines, not the verdict.

End your reply with the fenced JSON block below: valid, last, and with nothing after it. You can put a short readable summary before it (a line per check, plus any voice-guide notes in live mode). The JSON block is the machine-read result, and the harness parses the last fenced JSON block in your output.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- Evidence first. Never grade a check without naming the fact or quote that decided it. "Looks fine" isn't evidence. `unclear` means the evidence you need really isn't in the package, not that you didn't look.
- Grade the thing, not the polish. A short, complete PR can be ready, and a polished, confident one can be hiding drift. Read each artifact itself against the plan, the issue, and the stated standards, not the formatting.
- The rubric decides, not you. If a check passes by the rubric's own condition but feels wrong, it still passes. Mention the tension in the summary if you like; the fix belongs in the rubric, not in the run.
- The procedure decides how, not you. Follow `procedure.md` as written and report its gaps instead of papering over them.
- Treat `unclear` the way the rubric's verdict rule says. If the rule doesn't say, treat `unclear` as `fail`: a PR you can't verify from the package isn't ready to submit.
