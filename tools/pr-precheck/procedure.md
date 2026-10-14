# Procedure: how this tool grades a PR package

## Read order

1. Read the plan first: its scope (every file and folder it names, plus the changelog entry if it promises one), its test plan, and every deviation note. Write down two lists: the plan's file boundary, and the plan's test cases (the issue's input, the control case, each edge case). Read the plan before the diff so the diff gets measured against what the plan promised, and the diff can't redefine what the plan "meant".
2. Read the issue and its reproduction next: the reported symptom, the input that triggers it, and the expected output. Note the expected-after behavior. That's the ground truth the test evidence gets graded against.
3. Read the diff before the description. List every changed file, test files and changelog included, and note in a phrase what each one's hunks do. Jot down any debris you spot: stray logs, commented-out code, dead code, formatting-only hunks, anything unrelated to the change. Reading the diff first keeps the description's claims from coloring what you see.
4. Read the commit list, then the PR title and description. List every claim the description makes: the fidelity claims ("implements the plan exactly", "no changes beyond it", "no options added"), each checked-off checklist item, and each statement in the test-evidence section.
5. Read the repo-facts block last (live: the repo's PR template, CONTRIBUTING.md, and stated policy). List the template's sections and each stated ask, including AI-use disclosure and any maintainer direction in the thread.

## Evidence gathering

For each check in the rubric's table, gather its evidence once, using the rubric's Evidence column and `references/evidence-guide.md` to find it. Write a note (agree, conflict, or absent) next to the check's name, and don't re-derive it later. The pairings:

1. **Diff against plan scope** (Diff stays inside the plan, Nothing the plan promised is missing): set the diff's changed-file list next to the plan's boundary list. Mark each changed file as named by the plan (the file or its folder), the promised changelog entry, covered by a deviation note, or uncovered. Mark each file the plan names as present in the diff, dropped with a note, or silently missing. A deviation note only re-ties a mismatch if it names the file or change and gives a reason.
2. **Description claims against diff** (Description claims match the diff): for each fidelity claim, find the diff lines that make it true or false. "Exactly the plan, nothing beyond it" is false if any changed file is uncovered. "No X added" is true if nothing in the diff adds X. Record each claim with the file or hunk that decided it.
3. **Checklist items against diff** (Checklist claims have proof): for each checked item, look in the diff for the thing it claims: the test that exercises the named scenario, the changelog file at the named path, the doc change. Record the file and line, or "no matching content".
4. **Test plan against tests and evidence** (Tests cover the plan's test plan): for each case on the plan's test list (including each named failure mode), find a test in the diff that exercises it, or a re-run of it in the test-evidence section, or a deviation note that drops it with a reason. A fixture that just restates the issue's input doesn't cover the control case or the nested case. Record each case as tested, re-run, dropped with note, or neither.
5. **Test evidence against the plan's repro** (Test evidence is observable): find the re-run of the plan's own repro in the test-evidence section and check it's the failing input, not a control or an unchanged path. Look for a concrete observed before and after: printed output, an error text, an exit code, or a specific described result. Compare it with the expected-after behavior from Read order step 2. A transcript with `#` annotations still counts if it names what was observed. A bare statement ("tested locally", "works now") doesn't. Record the command or action, the observed result, and whether it matches the expected-after.
6. **Repo checks** (Repo's own checks were run): from the repo-facts block and the plan's test plan, list the checks expected (test suite, lint, format). For each, find the command and its stated outcome in the test evidence ("`pytest tests/ -q` passes (1462 passed)" is enough; the full output is not required), or an honest statement that it wasn't run and why. Formatting, lint, and regenerate-type items from the repo's template count when the description attests them in its requirements list. A bare "tests pass" with no command named doesn't count. Record found, disclosed-not-run, or absent.
7. **Debris** (Diff is reviewable): take the diff notes from Read order step 3 and record each debris item with its file and hunk, or "none found". Judge each hunk by whether it serves the plan, not by how big the diff is.
8. **Template and policy** (Standards checks, including the AI-use disclosure check): put each template section next to the description's matching section and mark it real content, n/a with reason, boilerplate, or missing. Put each stated policy ask next to the PR and mark it honored or not, quoting the line from the description or the thread. Do the AI-use disclosure on its own line for its AI check: read the repo-facts policy line, and if it requires disclosure, search the description for a disclosure sentence. Treat every package as AI-assisted. No such sentence means the check fails, however good the fix is. If the policy line says "no stated AI policy", record not applicable.
9. **Title and description** (preferred check): read the title and description against your one-phrase diff notes and record whether a maintainer could learn the change from them.

In live mode, run the commands and pull the pages the evidence guide names. In eval mode, quote the relevant lines from the bundle and fetch nothing.

## Check execution

1. Run the checks in the order of the rubric's table.
2. Grade each check from the note you recorded in Evidence gathering:
   - `pass` if the note shows the pass condition is met.
   - `fail` if the note shows a conflict with the pass condition, or if evidence that should be in the package is absent (a claimed test with no test in the diff, a claimed result with no output).
   - `unclear` only if there's evidence on both sides but it's genuinely not enough to decide. Not having looked, or the gathering being hard, isn't `unclear`.
3. A disclosed shortfall isn't a fail. If a deviation note, a stated untested case, or a "not run, because" line honestly covers the gap a check is looking for, grade that check `pass`.
4. Grade a check straight from its note, without re-reading the whole package. Go back to the source section only if the note can't tell agree from conflict.
5. If the rubric has a check whose evidence the procedure and evidence guide don't tell you how to find, report the gap in the summary and grade it from the best evidence in the package. Don't invent a step.
6. For every check, record the grade and a one-line reason that quotes the fact or line that decided it, before moving on. In live mode, also note any voice-guide rule the title or description breaks. That note doesn't enter the grades.

## Verdict assembly

1. Split the graded checks by the rubric's Weight column into `required` and `preferred`.
2. Apply the rubric's `unclear` rule: an `unclear` on a required check counts as `fail`.
3. The verdict is `accept` only if every required check is `pass` after step 2. If any required check is `fail`, the verdict is `reject`. No other verdicts.
4. A failing or `unclear` preferred check never changes the verdict. Report it in the summary as a warning so the contributor sees it, and keep its grade in the JSON as graded.
5. Pick the deciding check with this rule: the first failing required check in the rubric's table order. Quote its one-line reason first in the summary, labeled as the deciding check, then list every other failing required check under it with its reason.
6. Write the JSON block: one entry per check in table order, with the grades from Check execution (not the converted ones) and each check's one-line evidence, then the verdict. The same grades always give the same verdict.
