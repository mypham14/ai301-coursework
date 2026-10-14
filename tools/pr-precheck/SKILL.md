---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

<!--
THIS IS THE PART YOU WRITE, and it is the last one: the frame itself.
Weeks 1 through 3 handed you a working SKILL.md and you filled the
files behind it; this week the frame ships as headings, and you write
what it says. The frontmatter above and the section headings below are
fixed (CONTRACT.md's layout rule); the instructions under each heading
are yours. Write instructions to the tool, in the imperative, the way
weeks 1-3's frames spoke to you: what to read, in what order, what to
refuse, what to emit. Your executor in the rotation is the test: a
frame gap they hit (cannot tell what the tool reads, or how a verdict
gets assembled) is a missing sentence here.

One section is not yours: the JSON schema in "Verdict and output" is
reproduced from CONTRACT.md verbatim and may not be altered. Your
words decide everything around it.
-->

## The question

<!-- State, in your words, the one question this tool answers and what
a PR package is: what artifacts it contains and what they are read
against. CONTRACT.md fixes the question; your frame has to say it so
the tool cannot wander into grading something else. -->

You are grading one PR package to answer a single question: is this
ready to submit? A PR package is a candidate pull request (its title,
description, commits, diff, and test evidence) read against the plan it
claims to implement and the issue that plan belongs to. Grade only that
question, and only one package per run. Do not review the code's
design, suggest a different fix, or answer from gut feel: you answer by
executing `procedure.md`, which applies `rubric.md` to evidence
gathered per `references/evidence-guide.md`.

## Inputs and modes

<!-- Define both modes. Live mode: name every input (your plan.md with
its deviation notes, your branch's diff, your draft PR title and
description, your test evidence, your issue), where each comes from,
and what a house-chain student reads instead. The branch's diff is
everything the branch changes relative to the repo's default branch:
`git diff main...HEAD` (three dots), run from the working copy, is
the command that produces it; name the source that concretely. Eval
mode: state that the bundle is the whole world, nothing is fetched,
and every check runs with the full verdict rule. -->

Work in exactly one of two modes.

- **Live mode**: the student's own submission, checked before it goes
  out. Read these inputs:
  - `plan.md` (with any `## Deviations` section) from the top folder of
    the working copy.
  - The diff on the branch: run `git diff main...HEAD` (three dots)
    from the working copy. It shows every committed change the branch
    makes relative to `main`; uncommitted work is not part of the
    package.
  - The draft PR title (first line) and description in `pr_draft.md`.
  - The test evidence in `test_evidence.md`.
  - The issue's thread, fetched live (`gh` or the GitHub API), and the
    repo's PR template (`.github/PULL_REQUEST_TEMPLATE.md`) and
    `docs/CONTRIBUTING.md`.
  - A house-chain student has no plan of their own: read the house plan
    and the house repro pack in their place. The same checks grade the
    same things.
  Read the package the way a maintainer will read the PR: what the
  diff, description, and evidence contain is the package. Do not
  credit anything that is only in the student's head or in other files.
- **Eval mode**: the input is a package bundle (a markdown file such as
  `calib-01.md`). The bundle is the whole world. Use only its text as
  evidence, fetch nothing, read nothing else, and do not read
  `scope.md` or `voice-guide.md`. Eval mode always grades a complete
  package: every check, with the full verdict rule.

If you cannot tell which mode you are in, say so and stop.

## The scope seam (live mode only)

<!-- Tell the tool when to read scope.md, what to do with the rules it
finds there, what to refuse, and what to do when the scope's repo line
is an unfilled placeholder. State that eval mode ignores scope.md
entirely. CONTRACT.md names the required behavior; your frame has to
instruct it. -->

In live mode, read `scope.md` before anything else, before running any
git or GitHub command. It names the one repo a PR may target and the
house rules of that environment; apply those rules when reading
evidence (for example, the PR template is always required and the
disclosure section needs real content). Refuse to grade a PR aimed at
any other repo. If the `Repo:` line still carries an unfilled
placeholder, stop without grading and tell the student to fill in the
`Repo:` line in `scope.md` with their section's Path Review repo. Never
guess a scope. In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

<!-- Tell the tool when to read voice-guide.md, which outgoing text it
gates (the PR title and description), how to report a broken rule, and
why it never changes the verdict on its own. State that eval mode
ignores it entirely. -->

In live mode, after reading the scope, read `voice-guide.md`: the
student's own rules for how they write upstream. Hold the outgoing PR
text, the title and the description, against those rules. Report each
rule the draft breaks in the summary, quoting the rule and the
offending words. The voice guide never changes the verdict on its own;
only a rubric check that reads it can do that. In eval mode, ignore
`voice-guide.md` entirely: voice is personal and carries no gold
labels, and the universal communication-quality checks live in the
rubric.

## Component reads

<!-- Tell the tool how the pieces connect: rubric.md defines the
checks and the verdict rule, references/evidence-guide.md maps where
each evidence family lives, procedure.md is executed as written. Say
what the tool does when the procedure is silent on a step (report the
gap, never improvise around it) and what it does when rubric.md or
procedure.md has no content (the refusal rule, stated as an
instruction). -->

Read these and follow them; each has one job.

- `rubric.md` defines the checks (name, evidence, pass condition,
  weight of `required` or `preferred`) and the verdict rule that turns
  grades into accept or reject.
- `references/evidence-guide.md` maps where each kind of evidence lives
  in a bundle and in a live PR, and what good looks like there.
- `procedure.md` is the operating procedure: read order, evidence
  gathering, check execution, verdict assembly. Execute it exactly as
  written.

When the procedure is silent on a step, do not improvise. Say what is
missing in your summary and carry on with the nearest written step. If
`rubric.md` has no checks, or `procedure.md` has no steps, stop and
say so: you cannot grade without both, and that is by design. Never
invent checks at runtime.

## Verdict and output

<!-- State the binary verdict space (accept means submit, reject means
hold) and instruct the tool to end its reply with the fenced JSON
block below, valid and last, with nothing after it. The schema is
CONTRACT.md's, verbatim; do not edit it. -->

The verdict space is binary: `accept` means the PR is ready to submit,
`reject` means hold. There is no third verdict and no score; put any
reservation in a check's evidence line. Combine grades only as the
verdict rule in `rubric.md` directs. Before the JSON block you may show
a short readable summary: one line per check, plus voice-guide notes in
live mode. End your reply with this fenced JSON block, valid and last,
with nothing after it. The harness parses the last fenced JSON block.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

<!-- Write the standing rules the tool grades under: evidence first,
grade the thing not the polish, the rubric decides, the procedure
decides how, and how unclear grades are treated when the rubric's
verdict rule is silent. CONTRACT.md states each as a guarantee; your
frame has to make them instructions. -->

- Evidence first: never grade a check without naming the fact or quote
  that decided it. "Looks fine" is not evidence. A sentence saying
  something passed is not proof; a diff line or pasted command output is.
- Grade the thing, not the polish: a terse complete PR can be ready and
  a long confident one can hide drift. Read the diff, evidence, and
  description against the plan, the issue, and the repo's stated
  standards, never the formatting.
- The rubric decides, not you: if a check passes by its stated
  condition but feels wrong, it still passes. Note the tension in the
  summary; the fix belongs in the rubric.
- The procedure decides how, not you: follow `procedure.md` as written
  and report its gaps.
- Treat `unclear` as the rubric's verdict rule directs. If the rule is
  silent, treat `unclear` as `fail`: a PR you cannot verify from the
  package is not ready to submit.
