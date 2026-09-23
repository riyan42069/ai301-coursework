# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Repo is alive | Repo-facts block: last default-branch commit dates, latest release, last push to any branch, archived flag | The repo isn't archived, and something landed on the default branch (a commit or a push) in the last 180 days. Measure from the bundle's capture date in eval mode, or from today in live mode. | required |
| Sized for a newcomer | Issue body, linked files/components, and comment thread | Judge this by whether the issue points at one connected outcome, not by how many files, steps, or contributing causes it takes to get there. Fixing one bug, writing one doc page (even if a few other pages need small pointer updates), or shipping one small feature all count as bounded, even across several files. A bug that's been traced to more than one contributing cause (e.g. "this freezes because of A and also B") is still one task — fixing the one symptom, not several unrelated ones. It only fails as an umbrella when the sub-items are genuinely independent deliverables that don't share one outcome — separate features, separate bugs, or a checklist meant to be split into its own issues. Don't fail this check just because the fix sounds technically hard or unfamiliar; that's a difficulty judgment, not a scope one, and isn't what this check is for. Ignore anything the reporter clearly marks as optional or extra ("additional suggestions," "could also," "nice to have," "lower priority") — grade only the required core, not the brainstormed extras. Also fails if the thread shows the design is still being debated with nothing settled, if a maintainer has said the fix touches core internals, if it's really a support question in disguise, or if two or more closed-and-unmerged PRs already sit in its history — that last one is a sign the "first issue" has quietly become a hard one. A short or under-explained bug report can still pass if the core ask is otherwise clear and contained. | required |
| Nobody's already on it | The issue's linked pull requests, assignees, and comment thread | Pass if there's no assignee, no currently open PR tied to this issue, and no "I'll take this" / "working on it" comment sitting unanswered. An open linked PR or an assignee is an automatic fail, comment or not. A closed, unmerged PR just means someone tried and stopped — that alone doesn't fail this check. | required |
| Contribution rules allow this | CONTRIBUTING.md, .github contributor docs, any dedicated AI-policy file (e.g. AI_POLICY.md), and PR/issue templates — see references/evidence-guide.md | Passes unless the repo flat-out bans AI-generated or AI-assisted contributions. Rules that fall short of a ban — disclosure requirements, "you must understand and test what you submit," human review — are just terms to follow, not a reason to reject. If the repo says nothing about it, that's a pass. | required |
| Reproducible | Issue body and comment thread | Pass if there's something concrete to verify the fix against: repro steps, a failing example, an expected-vs-actual description, a referenced test, or similar. | preferred |
| Newcomer guidance | Issue labels, issue body, and comment thread | Pass if there's a newcomer-friendly label, an implementation hint, a pointer to the relevant file/component, or maintainer guidance that gives a new contributor somewhere to start. | preferred |

## Verdict rule

Accept only if all four required checks pass: Repo is alive, Sized for a newcomer, Nobody's already on it, and Contribution rules allow this. One failed required check is enough to reject. If the evidence for a required check is unclear or just isn't there, treat it as a fail rather than guessing — reject rather than gamble on a first issue you can't actually verify. The two preferred checks never flip the verdict; they only rank the issues that already passed, with more preferred-check passes ranking higher.
