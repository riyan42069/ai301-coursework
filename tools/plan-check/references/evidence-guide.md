# Evidence guide: where evidence lives in a plan package

<!--
THIS IS THE PART YOU WRITE (second week running: the judgment files
stay in your hands). The skill uses this guide as its map: for every
kind of evidence a rubric check names, this file says WHERE to find it
in a plan package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repro-evidence block,
  the candidate plan's scope statement or test plan, the plan comment,
  the repo-facts block). In live mode (where on GitHub or in the
  draft: the issue thread, the student's posted repro comment, the
  repo's docs, the draft plan and comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the stated cause cites behavior the
  repro evidence actually shows") over adjectives ("diagnosis is
  solid").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts twice: your
procedure.md tells the skill WHEN to gather each family, and this
guide tells it WHERE. Write the map you wish your executor had.
-->

## Diagnosis and grounding

<!-- Where the plan states its cause, and where the repro evidence
pins down the behavior that cause must explain. What it means for a
diagnosis to follow from the evidence rather than contradict or
ignore it. -->

**Where it lives:**The plan's opening "Cause:" line sattes the diagnosis. The repro evidence block or the posted repro comment shows the behaviours that cause must explain. Look specifically at the "Actual:" outcome and the step where the failure first appears.

**What good looks like:** The stated cause directly predicts the specific symptom the repro captured. 
You should be able to draw a straight line: cause → mechanism → the exact failure step. A diagnosis that names a plausible but untethered explanation (one that doesn't predict why *this*
symptom appears at *this* step) contradicts or ignores the evidence even if it sounds reasonable. A diagnosis that matches a different failure mode than what the repro shows (e.g., "push fails" when the repro shows "push succeeds but display is stale") fails the same way.

## Scope

<!-- Where the plan bounds itself: the in-scope statement, the
not-in-scope line, the files or areas named. What one bounded change
looks like next to a drive-by rewrite. -->

**Where it lives:** In the second section of the *Candidate plan*, "Change:" states the scope of the proposed change. The line satrting with "In:" mention the in-scope seection which is `pkg/gui/controllers/sync_controller.go`. Out-scope contains any other change in the push status computation.  

**What good looks like:** The plan identifies one specific change that directly addresses the reproduced issue and clearly separates what is in scope from what is not. The named file or code area should match the proposed fix, and the plan should avoid unrelated refactoring or changes to behavior that already works correctly.

## Executability

<!-- Where the plan says what will actually be done: files or areas,
approach, order of work. What it means for a stranger to be able to
start executing without asking the author anything. -->
**Where it lives:**\
The candidate plan’s `Change:` section is the primary source. Look for the named file or code area, the function or callback involved, and the concrete action the author intends to take. The `Cause:` statement can provide the mechanism that explains why that action is appropriate. In live mode, use the draft plan together with any code references the student included.

**What good looks like:**\
A developer unfamiliar with the issue should be able to start the implementation without asking what part of the codebase to inspect or what behavior to change. The plan should identify the relevant area and describe the implementation approach at a useful level of specificity, without pretending to know exact implementation details that have not been verified.


## Test plan

<!-- Where the plan says how success will be observed, and how that
maps onto the repro evidence's steps and artifacts. What a decisive
test plan names that a vague one does not. -->

**Where it lives:**\
The candidate plan’s `Test:` section contains the proposed verification steps. Compare those steps against the repro-evidence block or the student’s posted repro comment. Look for the original failing step, the expected fixed behavior, and any additional regression or shared-path checks named by the plan.

**What good looks like:**\
The test plan re-runs the reproduction and makes the previously failing step the decisive success condition. It should state what observable behavior changes after the fix, such as the commit color updating immediately after a successful push without leaving the view. Additional checks should be tied to code paths affected by the change rather than being unrelated general testing.

## Honesty

<!-- Where claims meet uncertainty: risks, unknowns, and deviations.
How to tell stated unknowns from false confidence, and where an
honest mid-build deviation gets recorded. -->

**Where it lives:**\
Look across the candidate plan’s `Cause:`, `Change:`, and `Test:` sections for claims presented as confirmed facts versus assumptions. Also inspect the candidate plan comment for qualifications, risks, unknowns, or any stated deviation from the original plan. In live mode, later implementation comments or updates may record discoveries that changed the plan.

**What good looks like:**\
Confirmed observations are stated as confirmed, while assumptions or unverified implementation details are presented with appropriate uncertainty. The plan should not claim that a specific callback, field, or refresh path is definitely responsible unless that was verified from the code. If implementation reveals that the original diagnosis or scope was wrong, an honest update records the deviation instead of silently presenting the new approach as the original plan.

## Comms

<!-- Where the words meet the thread and the repo: the plan comment
read against the issue's maintainer signals (thread highlights, or
the live thread) and against the repo-facts block's stated templates,
contributing asks, and contribution policy (including AI-use
disclosure requirements). What thread-aware looks like next to
boilerplate. -->

**Where it lives:**\
The candidate plan comment is the primary evidence. Read it alongside the issue thread or `Thread highlights` section and the `Repo facts` block, especially the bug-report template, contributing guide, maintainer expectations, review constraints, and any AI-use disclosure requirements. In live mode, inspect the actual issue discussion and repository contribution documentation.

**What good looks like:**\
The comment reflects the specific issue and repository rather than sounding like generic boilerplate. It should accurately summarize the reproduction and intended fix, avoid claiming more certainty than the evidence supports, and respect maintainer guidance such as keeping the change small when review bandwidth is limited. If the repository requires particular disclosures, testing details, or contribution practices, the comment should comply with them.
