# Procedure: how this skill grades a plan package

<!--
THIS IS THE PART YOU WRITE, and it is a new kind of part. Weeks 1 and
2, SKILL.md carried a numbered workflow and you only wrote judgment
files. This week the workflow is gone from the frame: SKILL.md says
"execute procedure.md", and these are the operating steps you author.
The machinery is in your hands now.

Your operator swap is the design brief. When your executor stalled
because your rubric said WHAT to decide but not HOW to find the
evidence, that was a procedure gap. This file is where those gaps get
closed: a complete procedure lets someone who has never seen a plan
package before (a groupmate, or the skill itself) grade one exactly the
way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
repro evidence before the plan, and note what behavior it pins down"
is a step; "understand the context" is a wish.
-->

## Read order

1. Read the issue first. Note the reported symptom, the component or file it names, and any stated expected behavior.
2. Read the repro evidence next. Note each repro step, its observed output/behavior, and any timing or ordering details. This is the ground truth every check below is graded against — read it before the plan so the plan's claims don't anchor your reading of the evidence.
3. Read the candidate plan last: its Diagnosis, Scope, Changes, and Test plan sections. Note the plan's stated cause, the files it says it will touch, and what its test plan claims to exercise.

## Evidence gathering

For each check in the rubric's table, do the following before grading it:

1. Open the rubric row for the check and read its Evidence column to find the exact section(s) of the plan it names.
2. Copy the plan's claim from that named section verbatim (or near-verbatim) — e.g., the stated cause from Diagnosis, the file list from Scope, the described behavior, or the test plan's steps.
3. Put that claim side by side with the matching lines from the repro evidence gathered in Read order step 2 (the specific repro steps, timing, or observed behavior that bear on this claim).
4. Note explicitly whether the two agree, conflict, or whether the repro evidence simply doesn't address the claim (in which case the evidence is absent, not conflicting).
5. Record this agree/conflict/absent note next to the check's name — it is what Check execution and Verdict assembly will consume. Do not re-derive it later; gather it once per check here.

## Check execution

1. Execute checks in the order they appear in the rubric's table.
2. For each check, take the agree/conflict/absent note gathered above and grade it:
   - Grade **P** if the evidence (the matched plan claim and repro lines) agrees with the check's pass condition.
   - Grade **F** if the evidence is missing entirely (absent) or if it contradicts the check's pass condition.
   - Grade **?** only if there is evidence on both sides but it is genuinely insufficient to tell whether the pass condition is met — not merely because gathering felt effortful.
3. A check may be graded directly from the note recorded in Evidence gathering without re-reading the whole package; re-open the original section only if the note itself is ambiguous about agree vs. conflict.
4. Record the grade (P/F/?) and a one-line reason (quoting the conflicting or confirming detail) for every check before moving to the next.

## Verdict assembly

1. Partition the graded checks by weight: `required` and `preferred`, per the rubric's Weight column.
2. Apply the rubric's unclear rule: a `?` grade on a required check counts as a fail (F) unless the rubric states otherwise.
3. Verdict is **PASS** only if every required check grades P (after step 2's `?`-to-F conversion). If any required check grades F (or F-via-? ), the verdict is **HOLD**.
4. A `preferred` check failing (F or unresolved `?`) never changes the verdict — it does not block a PASS. Instead, attach a warning/notification line to the output naming the preferred check(s) that failed, so the contributor sees it without it holding the package.
5. In the output, quote the one-line reason recorded in Check execution for every required check that graded F — these are the deciding checks and must be visible to whoever reads the verdict. Quote preferred-check failure reasons too, but label them as warnings, not blockers.
