# Evidence guide: where evidence lives in a PR package

<!--
THIS IS THE PART YOU WRITE (third week running: the map stays in your
hands). Your tool uses this guide as its map: for every kind of
evidence a rubric check names, this file says WHERE to find it in a PR
package and WHAT GOOD LOOKS LIKE when you do.

The four families below are the harness's failure categories under
the names the eval README uses: plan fidelity = silent-drift, test
evidence = not-tested, diff quality = unreviewable, standards and
comms = standards-wall. A package that fails none of them is a
clear-accept. Under each family heading below, write:
- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the plan-context block's scope pair and test
  plan, the candidate PR's diff, commits, description, or
  test-evidence section, the repo-facts block's template asks and
  stated policy). In live mode (where in your working copy and on
  GitHub: your plan.md and its deviation notes, your branch's diff,
  your draft title and description, your captured test output, the
  repo's PR template and CONTRIBUTING.md).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("every changed file falls inside the
  plan's stated boundary or a deviation note") over adjectives ("the
  diff is clean").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts three ways: your
procedure says WHEN to gather each family, this guide says WHERE, and
your SKILL.md says the tool reads both. Write the map you wish your
executor had.
-->

## Plan fidelity (harness category: silent-drift)

<!-- Where the plan states its scope, boundary, and deviation notes,
and where the diff shows what actually changed. What it means for a
diff to match the plan, for an honest deviation to re-tie a mismatch,
and what silent drift looks like in each direction (more than the
plan, or less with no note). The description's fidelity claims read
against the diff live here too: a description claiming more or less
than the diff delivers is silent drift, not a comms problem. -->

**Where it lives.**
- Eval bundle: the "Plan context" block holds the plan's scope, its "Files:" list, and its "Not in scope" statement. The candidate PR's "Diff" section holds every changed file (each `---`/`+++` path). The "Description" and checklist hold the fidelity claims ("exactly as planned", "no options added", "tests added for X").
- Live: `plan.md` (the scope, the file list, and `## Deviations`), the output of `git diff main...HEAD` (and `--stat` for the file list), and the claims in `pr_draft.md`.

**What good looks like.** Every changed file is named by the plan (the file, its folder, or a kind of file such as "a changelog entry") or by a deviation note that gives a reason, and every promised change and test is in the diff. Silent drift goes two ways: the diff does more than the plan (an unnamed file, an unrequested refactor) or less (a promised test or change that is absent with no note). A description that says "exactly as planned" while the diff touches an unnamed file is drift. A shortfall the description or a deviation note discloses is not.

## Test evidence (harness category: not-tested)

<!-- Where the PR shows its proof: the test-evidence section's
before/after against the plan's test plan and the reproduction's own
steps, and the outcome of the repo's own checks or suite. What
decisive looks like (an observable behavior named, the expected-after
stated, the checks' outcome visible) next to "tests pass". -->

**Where it lives.**
- Eval bundle: the candidate PR's "Test evidence" section, read against the plan context's test plan and the repro evidence's steps. The repo-facts block names the checks the repo asks for.
- Live: `test_evidence.md` (the before and after of your repro steps, and the output of `make test-unit`, `make test-integration`, `make lint`, and `make typecheck`) and the Testing section of `pr_draft.md`, read against the test plan in `plan.md`.

**What good looks like.** The scenario the plan's test plan or the issue names (the same input and command, not an easier case) is re-run on the branch, and a concrete before result and after result are stated: printed output, or a note naming what was observed. The suite or test command is named as run and reported passing. "Works on my machine", a different scenario standing in for the repro, a bare "all tests pass" with no command, or "matches expected" with no result stated, is not proof. A failing or empty check reported honestly with its reason (for example "no tests ran") counts as evidence. A claimed result with no output, or a check that was never run, does not.

## Diff quality (harness category: unreviewable)

<!-- Where the change itself lives: the unified diff and the commit
list. What a reviewable change looks like (the fix visible, nothing
unrelated riding along) and the debris tells: debug leftovers, dead
code, commented-out blocks, formatting churn, drive-by edits. -->

**Where it lives.**
- Eval bundle: the candidate PR's "Diff" section (every hunk) and its "Commits" list.
- Live: `git diff main...HEAD` and `git log main..HEAD --oneline`. Only committed changes count.

**What good looks like.** The fix is visible and nothing unrelated rides along. The debris tells are: debug leftovers (`console.log`, `print`, stray `TODO`s), commented-out code, whitespace or formatting-only changes, and a rewrite of code the plan never mentioned (even one that keeps behavior the same: it buries the change and adds review work). Each hunk should be explainable by the plan, a deviation note, or the issue.

## Standards and comms (harness category: standards-wall)

<!-- Where the repo states its asks (the repo-facts block's PR
template sections, contributing instructions, and stated policy,
including AI-use disclosure) and where the PR honors them: the
stated sections filled with real content, the disclosure present,
explicit maintainer direction in the thread engaged. What compliant
looks like next to boilerplate or a visibly ignored ask. (Whether
the description's claims match the diff is plan fidelity, above.) -->

**Where it lives.**
- Eval bundle: the repo-facts block (its PR template asks and stated policy, including any AI-use rule) and the thread highlights, read against the candidate PR's title, description, and checklist.
- Live: `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md` in the Path Review repo, and the issue thread, read against `pr_draft.md`. The AI-use disclosure goes under "Notes for Reviewers", since the template has no section for it.

**What good looks like.** Every template section (Summary, Issue with `Closes #<number>`, Changes, Testing, Screenshots / Demo, Notes for Reviewers) has real content, not the placeholder comment. Each ticked box is true of the evidence (the CI box stays unticked before the PR opens). The disclosure says in the author's own words how AI was used. Any explicit maintainer ask in the thread is answered. Boilerplate, an empty section, a ticked box the evidence does not support, or a missing disclosure the repo asks for is a standards-wall fail.
