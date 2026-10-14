# Procedure: how this tool grades a PR package

<!--
THIS IS THE PART YOU WRITE (second week running for the procedure).
Week 3 you wrote these steps for a plan package; this week the graded
object is a PR package, and the read that matters most is a
side-by-side: the diff against the plan, the evidence against the test
plan, the description against both. Your week-3 procedure is the
pattern; do not paste it unchanged, because its read order was built
for a different object.

Your rotation is the design brief again, and this week friction routes
three ways: a stall on WHAT to decide is a rubric gap, a stall on
WHERE to look is a procedure gap (this file), and a stall on what the
tool even reads or outputs is a frame gap (your SKILL.md). A complete
procedure lets someone who has never seen a PR package before grade
one exactly the way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
plan's scope pair before opening the diff, and list the files the plan
names" is a step; "understand the change" is a wish.
-->

## Read order

<!-- What gets read, in what order, before any check is graded, and
what to note down from each part while reading. A complete procedure
decides where the plan sits in the order (before the diff? before the
description?) and says why the order matters for the side-by-side
checks that come later. -->

1. Read the plan first, start to finish (eval mode: the "Plan context" block; live mode: `plan.md`, including any `## Deviations`). Note four things: the files it says it will change, what it says it will not touch, its test plan (the command or check and the expected output), and each deviation note with its reason.
2. Read the repo's asks second (eval mode: the repo-facts block; live mode: `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`, and the issue thread). Note each section or checklist item the template asks for, and any stated AI-use or disclosure policy.
3. Read the diff third. Note every changed file path, and for each file whether the hunk is the plan's change or something else (formatting, a rewrite, debug lines, commented-out code).
4. Read the test evidence fourth. Note each command shown, whether its output is printed, and what the output says.
5. Read the title, description, and checklist last. Note every factual claim they make (fidelity, tests added, files added, nothing else changed). The description is read last so each claim can be held against what the diff and evidence actually contain, not the other way round.

## Evidence gathering

<!-- For each evidence family your rubric's checks name, the concrete
gathering move: which part of the package (or, live, which file, diff,
or page per your evidence guide) to pull the fact from, and what to
record. The load-bearing gathers this week are pairings: diff files
against plan scope, claimed evidence against the plan's test plan and
the repo's checks, description claims against diff contents. A
complete procedure leaves no check whose evidence an executor would
have to hunt for. -->

1. Files in plan: list the diff's changed files in one column and the plan's named files, folders, and deviation notes in another. Mark each changed file yes if the plan or a deviation names it (or its folder, or a kind of file such as "a changelog entry"), otherwise no. Mark each file the plan promises that the diff lacks.
2. Plan fully delivered: list each change and each test the plan promises, and record where in the diff each one appears, or that it is absent and whether the description or a deviation note discloses it.
3. Description matches diff: list each factual claim from step 5 of the read order. Beside each, record the diff line that supports it or "no support". Treat a "did not add X" claim as supported if nothing in the diff adds X.
4. Tests added: list each test the plan's test plan and the checklist name (the issue's input, controls, nested or edge cases). Beside each, record the test file and lines in the diff that cover it, or "absent".
5. Test evidence is real: for each result the PR claims (before output, after output, control unchanged, suite passing), record the command and the before and after result it states (printed output or a concrete note). Compare the scenario run with the plan's test plan and the issue's own steps: record whether it is the same input and command, or a different or easier case. If no before or after result is stated, record "no result". A control the plan names that goes unmentioned is noted but does not by itself fail.
6. Repo checks run: list each check the template or contributing guide asks for. Beside each, record the command named and that it passed, an honest stated failure with its reason, or "not shown". If the repo names no check command, record whether the project's test suite or command is named as run (a bare "all tests pass" names no command).
7. Diff is reviewable: from step 3 of the read order, list every hunk that is debris (debug lines, commented-out code, formatting-only churn, a rewrite the plan did not name, unrelated edits), with its file.
8. Template and policy followed: list each template section and checklist item from the repo's asks. Beside each, record whether the description has real content for it, whether each ticked box is true of the evidence, and whether the issue is named and any required disclosure is present. Record boilerplate left in place as "empty".
9. Title says the change: copy the title and the one-line description of what the diff does, side by side.
10. Record where each item was found. In eval mode use only the bundle text and fetch nothing; in live mode use only committed changes (`git diff main...HEAD`), `plan.md`, `pr_draft.md`, `test_evidence.md`, the repo's template and guide, and the issue thread.

## Check execution

<!-- How one check runs against gathered evidence: in what order the
checks execute, what an executor does when evidence for a check is
genuinely absent, and when a check may be graded without re-reading
the whole package. A complete procedure makes two executors grade the
same package the same way. -->

1. Run the checks in the rubric's order: Files in plan, Plan fully delivered, Description matches diff, Tests added, Test evidence is real, Repo checks run, Diff is reviewable, Template and policy followed, Title says the change. Grade each from the evidence recorded in the previous stage; do not re-read the whole package for each check.
2. Grade each check P (pass), F (fail), or ? (unclear), by the rubric's pass condition and nothing else.
3. Grade P only if the pass condition is clearly met by the recorded evidence. Grade F if the evidence is present and the condition is clearly not met (for example, a changed file the plan never names and no deviation explains, or a test claimed in the checklist that is not in the diff).
4. Proof means a line in the diff, or a result the evidence states concretely: printed output, or a note naming what was observed ("before: error X, after: searches normally"). A bare sentence saying something passed or "matches expected" with no stated result is not proof, and a claim that rests only on one is F.
5. If a shortfall is disclosed in the description or a deviation note and the diff matches that statement, grade P and name the disclosure in the evidence line. A shortfall visible only in the diff is F.
6. If the evidence for a check is genuinely missing (for example, no test-evidence section, no plan context to compare with), grade it ? and write "evidence absent". Do not guess.
7. For every check write one line: its name, its grade, and the fact or quote that decided it. A grade without a named fact is not allowed.

## Verdict assembly

<!-- How the per-check grades become the final accept or reject:
apply your rubric's verdict rule, state how unclear grades enter it,
and say what gets quoted in the output for the deciding check: the
check whose failure the verdict turned on. When more than one check
failed, your procedure picks which one gets quoted (first failing
required check in rubric order is a fine rule); nothing picks it for
you, so write the rule down. A complete procedure produces the same
verdict from the same grades, every time. -->

1. Apply the verdict rule in rubric.md: accept if every required check is P; reject otherwise.
2. Count every ? on a required check as F. Preferred checks (Title says the change) never change the verdict.
3. If the verdict is reject, quote in your summary the fact that decided the first failing or unclear required check in the rubric's order, and list any other failing checks after it.
4. In live mode, add the voice-guide notes to the summary after the check lines. They do not change the verdict.
5. Emit the verdict as the final fenced JSON block described in SKILL.md, with one entry per check (grades written as pass, fail, or unclear) in the rubric's order, and nothing after it.
