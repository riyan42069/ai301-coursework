# Evidence guide: where evidence lives in a PR package

A practice package (say `eval/packages/calib-01.md`) is laid out in the same order every time: Repo facts, Issue, Thread highlights, Plan context (the repro evidence, then the plan with its scope, "Not in scope", "Files:", and test plan), and the Candidate PR (Title, Description, Commits, Diff, Test evidence). Live, the same pieces come from your working copy and the repo, and each section below says where. In eval mode the bundle is all you have, so nothing gets fetched.

## Plan fidelity (harness category: silent-drift)

**Where to look**
- Practice package: the plan's boundary is in Plan context, in the "Files:" line, the scope sentence, and "Not in scope". What actually changed is in Candidate PR → Diff, where each `--- a/<file>` header is one changed file (tests and changelog included). The fidelity claims are in the Description.
- Your own PR: the boundary is `plan.md`, including every deviation note. What changed is `git diff main...HEAD` (add `--stat` for a quick file list). The claims are in `pr_draft.md`.

**What good looks like**
- Every changed file is something the plan names (the file or its folder), the changelog entry it promised, or something a deviation note explains with a reason.
- Everything the plan promised shows up in the diff, or a note says it was dropped and why.
- Silent drift goes both ways: a file the plan never mentioned that got changed, or a promised file or case that quietly vanished.
- A deviation written into `plan.md` or the description fixes a mismatch. One that only exists in the diff doesn't.
- Fidelity claims in the description have to be true of the diff. "Implements the plan exactly" is false if even one changed file is uncovered. "No options added" is true as long as nothing in the diff adds one. A description that claims more or less than the diff delivers is drift, not a wording problem.

## Test evidence (harness category: not-tested)

**Where to look**
- Practice package: the commands and their output are in Candidate PR → Test evidence. The test plan and the expected-after behavior are in Plan context. The tests that were actually added are in the Diff. The checks the repo expects are in Repo facts.
- Your own PR: `test_evidence.md` (step 8) for the commands and captured output, `plan.md` for the test plan, the test files in `git diff main...HEAD`, and the repo's CONTRIBUTING.md, PR template, and CI config for what it requires.

**What good looks like**
- Real proof is the plan's own repro (the failing input, not a control) re-run on the branch, with a concrete observed before and after: printed output, an error message, an exit code, or a specific described result like "toast appears, file still listed". A transcript annotated with `#` comments still counts if it names what was observed.
- A bare claim is not proof: "tested locally", "colors work now", "works on my machine", "matches the expected output" with nothing shown. Evidence that only exercises a control or the unchanged path isn't proof either.
- Each case in the plan's test plan is covered by a test in the diff or a re-run shown in the test evidence. If the plan names two failure modes and only one has a test or a re-run, the other is untested no matter how clean the first looks.
- The repo's own checks (tests, lint, format) are stated with the command and the outcome, like "`pytest tests/ -q` passes (1462 passed)" or "`go test ./...` passes". You don't need the full output. Formatting, lint, and regenerate-type items from the repo's template count when the description attests them in its requirements list, like "code formatted; cheatsheets regenerated, no diff". A bare "all tests pass" with no command named isn't enough. If a check wasn't run, the description says so and why. An honest "didn't run X because Y" is fine.
- Each checklist item (a test for the issue's input, the control, the nested case) has something behind it in the diff.

## Diff quality (harness category: unreviewable)

**Where to look**
- Practice package: Candidate PR → Diff for the hunks, and Commits for the list.
- Your own PR: `git diff main...HEAD` for the hunks and `git log main..HEAD --oneline` for the commits.

**What good looks like**
- The fix is easy to find and every hunk is there because the plan needs it. A big diff where every hunk earns its place is fine. A small diff with one unrelated hunk is not.
- Things that bury a change: leftover debug prints or logs, dead code, commented-out blocks, formatting-only churn (whitespace, reordered imports, reflowed lines nobody meant to touch), and drive-by edits the plan never mentions.
- Judge each hunk on whether it serves the plan, not on how long the diff is or how tidy the description sounds.

## Standards and comms (harness category: standards-wall)

**Where to look**
- Practice package: Repo facts lists what the repo asks for: the PR template line (or "no PR template"), the contribution guide's asks (tests with behavior changes, a changelog entry, small PRs that reference the issue), and any AI policy. Thread highlights has anything a maintainer said directly. The PR's answer to all of it is in the Description and Title.
- Your own PR: the repo's PR template (usually `.github/pull_request_template.md`), `CONTRIBUTING.md`, any AI-use policy, and the issue thread (`gh issue view <n> --comments`). Your answer is `pr_draft.md`. Path Review's house rules in `scope.md` add three more: always use the template, open the PR from your own fork's branch, and one PR per issue.

**What good looks like**
- Every template section has real content for this change, or "n/a" with a reason when it truly doesn't apply. Boilerplate left as-is or a skipped section is a fail. If the repo has no template, its stated asks are the bar: the changelog entry is in the diff, tests come with behavior changes, and the PR references the issue.
- If the policy asks for AI-use disclosure, the description itself says so plainly (the tool used and how much it helped, if the policy asks). Assume every PR is AI-assisted work, so a description with no disclosure sentence fails, however good the fix is. If the repo says nothing about AI ("no stated AI policy"), there's nothing to disclose and silence is fine.
- When a maintainer gave direction in the thread, the PR follows it or says why it doesn't.
- Each ask is honored in the package itself, not just asserted in the description. (Whether the description's claims match the diff is plan fidelity, above.)
