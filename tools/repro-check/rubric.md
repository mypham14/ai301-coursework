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
| env-recorded | The repro report's environment/setup section. | Pass if it states the exact tool/library version or commit hash and the OS, both as real values (e.g. "SQLAlchemy 2.0.31, Windows 11"). Fail if either is missing, vague ("latest", "my machine", "should work anywhere"), or contradicts the issue's stated environment. | required |
| steps-rerunnable | The repro report's steps list, read against the issue's own repro steps. | Pass if every step gives enough detail that a stranger could reliably get the same outcome, starting from a clean/fresh setup — either the literal command run, or, when an exact value genuinely doesn't affect the bug (arbitrary filler content, unrelated package names), enough specifics that no choice affecting the outcome is left unstated. Fail if a step names an action with no way to tell how to do it, omits a detail that actually matters to triggering the bug (a required flag, a specific environment/config, a private or unshared resource a stranger cannot get), or requires guessing what "it" refers to. | required |
| behavior-matches | The output/error excerpt pasted in the report, read against the exact behavior the issue describes (error text, exit code, wrong output). | Pass if either: the report claims reproduction and the pasted artifact shows the same failure the issue describes (not an adjacent one), with any meaningful environment/version difference from the issue explicitly acknowledged; or the report honestly states it could not reproduce and pastes the artifact showing what happened instead, naming what it tried and what differed from the issue's conditions. Fail if the report claims or implies reproduction with no artifact pasted, the pasted artifact doesn't show the issue's specific behavior, or a meaningful environment/version deviation from the issue is left unacknowledged. This check grades the pasted artifact against the issue text — it never requires re-running the reporter's code to verify it. | required |
| honest-outcome | The report's stated conclusion (reproduced / could not reproduce), read against the evidence actually shown for it. | Pass if the stated outcome matches the evidence — an honest, evidenced "could not reproduce" (what was tried, what happened instead) is a pass. Fail if the report claims reproduction without supporting evidence, or claims the issue's behavior while the pasted artifact actually shows a different one. | required |
| respects-conventions | The repo's stated contribution/AI policy (from repo-facts or CONTRIBUTING.md, per references/evidence-guide.md), read against the claim/repro comment text. | Every comment graded here is AI-assisted work, full stop — never infer that from the comment's wording. The policy takes one of two shapes, and they need different proof: (a) an explicit-disclosure policy (names the tool used and the extent of assistance) — pass only if the comment contains that explicit statement; a comment that never mentions AI has not met this, regardless of how good it otherwise reads. (b) a human-voice policy (comments must read as human-written; no ask to announce AI use, AI-sounding comments may be hidden) — pass if the comment reads as specific and grounded in this issue's actual technical detail, not generic or templated; do not fail it for lacking a disclosure sentence the policy never asked for. Pass automatically if the repo states no AI-related policy at all. Fail only when the applicable shape's own condition isn't met. | required |
| no-overreach | The claim comment text. | Pass if it reads as a specific, honest investigation update: it references the actual bug, and promises only next steps (investigating, testing, reading a specific code path) — no fix, no delivery date, no request to be assigned or have the issue reserved through flattery or urgency. Fail if it promises a fix or a timeline, asks to be assigned/reserved via generic flattery or urgency, or reads as boilerplate interchangeable with any other issue. | required |

## Verdict rule

Accept ("ready") only if every required check passes. Preferred checks never change the verdict. Any required check graded `unclear` counts as a fail.
