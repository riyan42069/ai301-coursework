# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| `environment-recorded` | Repro report's environment details and the issue's reported environment | The report records the relevant tool/version, OS or platform, and setup/install method, and explains any important differences from the issue environment | required |
| `steps-followable` | Commands, inputs, setup steps, and prerequisites in the repro report | Another contributor could repeat the attempted reproduction without having to guess a required command, input, dependency, or setup step | required |
| `behavior-matches` | Output/error artifact compared with the behavior described in the issue | The observed behavior matches the specific bug described in the issue, rather than a different or adjacent error | required |
| `outcome-honest` | Repro conclusion compared with the commands, outputs, and artifacts shown | The stated result matches the evidence: reproduced only when the target behavior is shown; otherwise the report honestly states cannot-reproduce or uncertainty | required |
| `repo-conventions` | Contribution guide, issue template, repo-facts block, AI/disclosure policy, and other rules identified in the evidence guide | The claim and repro follow the repository's stated contribution, formatting, disclosure, and communication requirements. If the repo-facts block states an AI-disclosure policy, this check fails unless the claim or repro comment contains an explicit disclosure statement — silence on disclosure is a fail even if every other convention is met | required |

## Verdict rule

Accept if every required check passes. If any required check fails or is unclear, reject.
