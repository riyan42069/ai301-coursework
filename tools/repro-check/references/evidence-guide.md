# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

## Environment

<!-- Where the environment record lives, and what a sufficient one
looks like against the issue's stated target. -->
Where to check:
- in an eval bundle, the repro report's environment section, compare the issue context's reported environment (the version/OS the
  original reporter used) and the repo-facts block (the repo's
  supported versions/platforms)
- in live mode, the equivalent section of the draft repro report, compare the issue thread and the repo's install docs (README/CONTRIBUTING)

What good looks like: 
- the report states a specific version of a tool/library used. It should be a version number(*v2.3.4*) **NOT** just *latest*.
- it should report a specific OS/platform like *Windows 11* and should be left out
- it should include the steps used of the methods used to install in the report
  

## Steps

<!-- Where the reproduction steps live, and what makes them followable
by a stranger, starting state to trigger. -->
Where to check:
- in an eval bundle, the repro report's steps/commands section, compare the issue's own stated steps (starting state,
  inputs, any prerequisites the issue names) and the repo-facts block
- in live mode, the equivalent section of the draft repro report, compare the issue thread's description of how to trigger the bug and the repo's setup docs (README/CONTRIBUTING)

What good looks like: 
- every command should be written out in full (*run npm install && npm test*) **NOT** just *run the usual build steps*.
- inputs, flags, or test data need their concrete values stated somewhere in the report (quoted strings, parameter numbers, a described file's contents) — it does not have to be a literally pasted file or script, as long as no value is left for the reader to guess or fetch externally
- it should state any prerequisite (a dependency version, an env variable, a config file, a login step), not assume it
- it should make the starting state clear, what to check out or configure before the first command runs
- a self-authored minimal input that swaps in an equivalent case (e.g. a hand-written config instead of fetching the issue's exact external file) still passes, as long as the substitution is described and triggers the same named condition
- an input already quoted exactly in the issue counts as stated, even if the report only references it by a distinguishing label (*shape A, rangeStart 19, rangeEnd 32*) instead of re-pasting it
- an unspecified incidental value (e.g. which packages fill an otherwise-irrelevant dependencies list) is not a guess if any valid value would trigger the same bug

## Behavior shown

<!-- Where the artifacts live (output excerpts, logs, screenshots),
and what it means for an artifact to show the issue's behavior rather
than an adjacent one. -->
Where to check:
- in an eval bundle, the repro report's output/error artifacts (the "Actual" block, logs, screenshots), compare the issue context's described behavior (the exact error, message, or wrong result the reporter saw)
- in live mode, the equivalent artifacts in the draft repro report, compare the issue thread's description of the bug

What good looks like: 
- the artifact shows the *same* specific symptom the issue names (*git stash list prints nothing, file stays untracked*) **NOT** a different error or a nearby-but-distinct failure
- it should be an actual captured output (a terminal excerpt, a screenshot), not a paraphrase like *it didn't work*
- if the issue names an expected vs. actual behavior, the artifact should let you tell the two apart, not just show one blob of output

## Honesty

<!-- Where claims and their backing meet: how to tell a report that
says exactly what happened (including an honest cannot-reproduce) from
one that claims more than its evidence shows. -->
Where to check:
- in an eval bundle, the repro report's conclusion/verdict line, compare the commands and artifacts shown earlier in the same report
- in live mode, the equivalent conclusion in the draft repro report, compare the draft's own steps and output

What good looks like: 
- the stated result should match what the evidence actually shows (*reproduced* only when the artifact shows the issue's exact behavior) **NOT** a confident *reproduced* resting on a mismatched or missing artifact
- a *cannot-reproduce* is a pass when it's backed by the attempted steps and the differing output, not a fail by default
- it should not claim more than the evidence supports (*this confirms the root cause is X*) when the artifact only shows a symptom, not a diagnosis

## Comms

<!-- Where the words meet the repo: the claim comment against the
issue, the comments against the repo's stated templates and
contribution policy (including AI-use disclosure requirements), and
what specific-and-honest looks like next to boilerplate. -->
Where to check:
- in an eval bundle, the candidate claim comment and repro comment, compare the issue context, the repo-facts block's contribution guide/template/AI-disclosure rules
- in live mode, the equivalent comments (claim and repro) on the issue thread, compare the repo's actual CONTRIBUTING.md, issue template, and any AI-use disclosure policy in its docs

What good looks like: 
- the claim comment should name the specific issue and what will be investigated (*I'll look at the stash-name prompt for untracked files*) **NOT** generic boilerplate (*I'd like to work on this*)
- it should follow the repo's required format (issue template fields, required sections) if one is stated
- if the repo requires disclosing AI assistance, the comment should disclose it plainly, not stay silent
- the tone should match the repo's stated contribution norms (e.g. promise investigation only, not a fix or a date)
