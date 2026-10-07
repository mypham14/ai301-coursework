# Evidence guide: where evidence lives in a plan package

<!--
THIS IS THE PART YOU WRITE (second week running: the judgment files
stay in your hands). The skill uses this guide as its map: for every
kind of evidence a rubric check names, this file says WHERE to find it
in a plan package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repro-evidence block,
  the candidate plan's scope statement or test plan, the plan comment,
  the repo-facts block). In live mode (where on GitHub or in the
  draft: the issue thread, the student's posted repro comment, the
  repo's docs, the draft plan and comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the stated cause cites behavior the
  repro evidence actually shows") over adjectives ("diagnosis is
  solid").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts twice: your
procedure.md tells the skill WHEN to gather each family, and this
guide tells it WHERE. Write the map you wish your executor had.
-->

## Diagnosis and grounding

<!-- Where the plan states its cause, and where the repro evidence
pins down the behavior that cause must explain. What it means for a
diagnosis to follow from the evidence rather than contradict or
ignore it. -->

**Where it lives.** The plan states its cause under `## Candidate plan`: a line starting "Cause:" in a short plan, or a "Diagnosis" subsection in a longer one. The behavior that cause must explain is under `## Repro evidence`: the numbered steps, the timings or output, and the "Actual:" line. Also check `## Thread highlights`, because a plan may borrow its cause from a commenter (for example "as identified in this thread") instead of from the repro.

**What good looks like.** The stated cause explains every observation in the repro evidence, including any step that contradicts it. For example, if one repro step shows the slowness remains with no pager involved at all, a cause in the pager cannot be right. A cause copied from a thread comment passes only if the repro evidence supports it. A plan that fixes a symptom (for example, refreshing something) passes only if the repro evidence points at that spot as the cause.

## Scope

<!-- Where the plan bounds itself: the in-scope statement, the
not-in-scope line, the files or areas named. What one bounded change
looks like next to a drive-by rewrite. -->

**Where it lives.** Under `## Candidate plan`: the "In:" and "Out:" lines of the Change paragraph, or a "Scope" subsection plus the numbered "Changes" list. Compare with the files named in the "Changes" list and in the plan comment, since the scope statement and the actual changes should match.

**What good looks like.** The plan names each file or area it will change and says what it will not touch. It describes one bounded change, and every item in the changes list falls inside the stated scope. A scope that names "the whole module", or a changes list that reaches past the "Out:" line, is a drive-by rewrite, not a bounded change.

## Executability

<!-- Where the plan says what will actually be done: files or areas,
approach, order of work. What it means for a stranger to be able to
start executing without asking the author anything. -->

**Where it lives.** Under `## Candidate plan`: the "Change:" paragraph, or the numbered "Changes" list. Use `## Repo facts` to check that named files, functions and tools are plausible for this repo.

**What good looks like.** A stranger could start work using only the plan: it names the specific file and the specific function or place to edit, and says what to do there. "Fix the bug" or "improve the refresh logic" does not qualify. Steps appear in an order that could be followed.

## Test plan

<!-- Where the plan says how success will be observed, and how that
maps onto the repro evidence's steps and artifacts. What a decisive
test plan names that a vague one does not. -->

**Where it lives.** Under `## Candidate plan`: the "Test:" paragraph or the "Test plan" subsection, plus any test item in the "Changes" list. Hold it against the numbered steps and the "Expected:" / "Actual:" lines under `## Repro evidence`.

**What good looks like.** The test replays the repro evidence's steps, or an equivalent of them, and names the observable result that would flip from the repro's "Actual" to its "Expected". It would fail before the fix and pass after. "Test it manually" or "make sure it works" proves nothing observable. Extra cases beyond the repro (other views, other inputs) are a bonus, not a requirement.

## Honesty

<!-- Where claims meet uncertainty: risks, unknowns, and deviations.
How to tell stated unknowns from false confidence, and where an
honest mid-build deviation gets recorded. -->

**Where it lives.** In the `## Candidate plan` text (risks, "not sure", "will check" wording, or its absence) and in the `## Candidate plan comment`. Read both against `## Repro evidence`: what the repro actually proved versus what the plan claims. In live mode, a mid-build deviation is recorded in the student's updated `plan.md`.

**What good looks like.** What the repro proved is stated as fact, and anything not yet checked is labeled as an assumption or a to-do. Confident wording about something the repro never showed (for example, "this is the cause" with no supporting step) is false confidence. A deviation from the posted plan is written into the plan with the reason, not left only in the diff.

## Comms

<!-- Where the words meet the thread and the repo: the plan comment
read against the issue's maintainer signals (thread highlights, or
the live thread) and against the repo-facts block's stated templates,
contributing asks, and contribution policy (including AI-use
disclosure requirements). What thread-aware looks like next to
boilerplate. -->

**Where it lives.** The `## Candidate plan comment`, read against `## Thread highlights` (what maintainers or collaborators have said, including any suggested direction) and against `## Repo facts` (the bug-report template, the contribution policy in CONTRIBUTING.md, and any AI-use disclosure rule).

**What good looks like.** The comment responds to what the thread actually says: it follows, or explains why it departs from, a maintainer's stated direction. It respects the repo's stated contribution asks (for example, review-time limits or a disclosure requirement) and does not promise a delivery date. A comment that could be pasted onto any issue unchanged is boilerplate, not thread-aware.
