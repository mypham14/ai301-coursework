# Rubric: is this pull request ready to submit?

<!--
THIS IS THE PART YOU WRITE (fourth week running; this is the rubric's
final form in the sandbox). Your frame in SKILL.md executes whatever
checks you define here, via your procedure.md. It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the diff read against the plan's scope, the test
     evidence read against the plan's test plan, the description read
     against the diff, the repo-facts block's template asks) or a
     location from your references/evidence-guide.md. "The PR" is not
     a source; "the diff's changed files read against the plan's
     stated boundary" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself
     (does the diff fall inside the plan plus its deviation notes? is
     the claimed evidence observable?), never the write-up's shape
     (how long the description is, how many commits there are).
     Structure-shaped checks are what make graders disagree with
     themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (submit) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. State the `unclear`
   treatment explicitly: the frame here is YOUR SKILL.md, so a rubric
   that stays silent is only covered if your frame's grading
   discipline says what happens (the contract's own default is that
   an unverifiable claim fails).

Cover what actually gets bad PRs submitted. The failure families the
lecture named ARE the harness's scoring categories, same names as the
eval README: silent drift (the diff silently does more or less than
the posted plan, or the description claims fidelity the diff
contradicts), not tested (the evidence proves nothing observable, or
the repo's own checks were never run), unreviewable (debris or
unrelated hunks bury the change), and standards wall (the repo's
stated template and disclosure asks are ignored). Your evidence
guide's four headings map onto these one to one (plan fidelity =
silent drift, test evidence = not tested, diff quality =
unreviewable, standards and comms = standards wall), and the category
floor is scored on exactly these names plus clear accept. A rubric
that ignores a category will fail the eval packages built around
that category. And remember the honest-outcome
rule, fourth week running: a PR that honestly discloses a shortfall
can be ready; a rubric that equates "less than everything" with
"hold" fails the set.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Files in plan | The diff's changed files (every `---`/`+++` path), read against the plan's stated file list, its boundary, and its `## Deviations` notes | Every changed file is named in the plan (the file, its folder, or a kind of file the plan names, such as "a changelog entry"), or is explained by a deviation note that gives the reason | required |
| Plan fully delivered | The plan's stated changes and test plan, read against the diff and the checklist | Every change the plan promises appears in the diff, or the description or a deviation note says it was left out and why | required |
| Description matches diff | Each factual claim in the description and checklist ("exactly as planned", "no options added", "tests added for X", "changelog added"), read against the diff lines | Each claim is true of the diff: a line in the diff supports it, or the diff contains nothing that contradicts a "did not add" claim. A claim of fidelity fails if any changed file is outside the plan with no note | required |
| Tests added | The test files in the diff, read against the plan's test plan and the checklist's list of tests | Every test the plan or checklist says was added is in the diff, for the issue's input and for each other case the plan names (controls, nested or edge cases) | required |
| Test evidence is real | The test-evidence section, read against the plan's test plan and the repro's own steps | The scenario the plan's test plan or the issue's repro names (the same input and command, not a different or easier case) is re-run on the branch, and the evidence states a concrete before result and a concrete after result (printed output, or a note naming what was actually observed, such as "before: error X, after: searches normally"), and the after result is the behavior the plan expects. Fail if the repro was not re-run, a different scenario stands in for it, only "works on my machine" or "tests pass" is offered, or no before and after result is stated. A note that only says the output "matches expected" without stating the result is not a result. A control the plan names that goes unmentioned does not by itself fail this check | required |
| Repo checks run | The test-evidence section and checklist, read against the repo's own checks (the PR template's listed commands, or the contributing guide's) | The project's test suite or test command is named as run on the branch (for example `go test ./...`, `cargo test`, `pytest`, `make test-unit`) and reported as passing, or its failure or absence is stated honestly with the reason (for example "no tests ran"). Where the repo's template or guide names specific checks, each one is either reported this way or its absence is disclosed. Fail if no test command is named at all (a bare "all tests pass") or a repo-named check is silently skipped. Counts or pasted output are welcome but not required | required |
| Diff is reviewable | The unified diff and commit list | The change is visible and nothing else rides along: no debug leftovers, commented-out code, formatting-only churn, or unrelated hunks. A rewrite of code the plan did not mention counts as unrelated even if behavior is unchanged | required |
| Template and policy followed | The repo-facts block (live: `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`, stated AI policy), read against the description's sections | Every section or checklist item the repo asks for is present with real content; each box ticked is true; an AI-use disclosure is present where the template or policy asks for one; the issue is named. Boilerplate left in a section fails | required |
| Title says the change | The PR title, read against the diff | The title names the actual change in plain words and matches what the diff does | preferred |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->

Accept (ready to submit) if every required check passes. Preferred
checks never change the verdict. A required check graded unclear counts
as fail, so any unclear required check means reject (hold). A shortfall
the PR honestly discloses does not fail a check: if the description or a
`## Deviations` note says what was left out or changed and why, and the
diff matches that statement, grade the check pass and put the disclosure
in the evidence line. A shortfall that is only visible in the diff, or a
claim the diff contradicts, fails.
