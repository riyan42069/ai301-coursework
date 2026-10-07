# Rubric: is this plan ready to post and build from?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here (via your procedure.md). It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the plan's scope statement, the test plan read against
     the repro evidence's steps, the plan comment read against the
     thread highlights, the repo-facts block) or a location from your
     references/evidence-guide.md. "The plan" is not a source; "the
     plan's stated cause read against what the repro evidence shows"
     is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (is
     this one bounded change? could a stranger start executing it?),
     never the write-up's shape (how many sections it has, how long it
     is, whether it uses headings). Structure-shaped checks are what
     make graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad plans posted. The lecture named the
failure families: the diagnosis ignores or contradicts the reproduced
evidence, the change is unbounded (scope creep), the plan targets the
symptom while the evidence points at the cause, a stranger could not
start executing it, the test plan proves nothing observable, the
unknowns are dressed up as certainty, and the comment ignores what the
thread or the repo's stated conventions ask. A rubric that ignores a
family will fail eval packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Diagnosis | The plan's Diagnosis section, read against the repro evidence | Passes if the stated cause agrees with every repro step and timing in the repro evidence; a diagnosis resting only on a thread comment, without repro support, fails | required |
| Behavior matches | The observed behavior described in the plan, read against the bug/cause described in the issue | Passes if the described behavior matches the bug or its cause as described in the issue; fails if it instead points to a different component unrelated to the error, or to a different, unrelated bug | required |
| Scope | The plan's Scope section, read against its Changes section | Passes if every file the Changes touch is named along with what will change in it, and nothing listed as out of scope is where the repro evidence points to the cause | required |
| Test | The plan's Changes section, read against its Test plan | Passes if the test plan adds an automated test that exercises the repro scenario and would fail before the fix is applied | required |

## Verdict rule

Ready if every required check passes. All four checks above are required; a fail on any one holds the package. `unclear` on any required check counts as fail.
