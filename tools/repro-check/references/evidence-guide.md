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

Where it lives: in an eval bundle, the repro report's environment/setup section. In live mode, the environment line(s) near the top of the draft repro comment (before posting) or the posted repro comment on the issue thread.

What good looks like: the exact tool/library version or commit hash and the OS are both named as concrete values — not "latest" or "my machine." If the issue states a target environment, the report's values either match it or the difference is called out as relevant to the bug.

## Steps

Where it lives: in an eval bundle, the repro report's numbered steps section, read against the issue's own repro steps. In live mode, the "steps to reproduce" portion of the draft or posted repro comment.

What good looks like: the steps start from a known, fresh starting state (a clean clone, a fresh install) and every line gives enough detail that a stranger gets the same outcome, in the order run. The literal command is expected wherever the exact value could matter (flags, versions, configs); a description is acceptable only where the exact value genuinely doesn't affect the bug (e.g., arbitrary filler content). No step should only name an intention ("set it up," "run the tool") without saying how, and no step should depend on a private or unshared resource a stranger cannot get.

## Behavior shown

Where it lives: in an eval bundle, the repro report's pasted output/error excerpt, read next to the issue context block that describes the bug. In live mode, the code-fenced output pasted into the draft or posted comment, read against the original issue body and thread.

What good looks like: if the report claims reproduction, the excerpt contains the specific error text, exit code, or wrong value the issue names — not just "it failed" or an unrelated warning — and any meaningful version/environment difference from the issue is called out. If the report honestly could not reproduce, the excerpt instead shows what actually happened, next to a plain statement of what was tried and what differed from the issue's conditions; that is a pass, not a missing artifact. This is graded by reading the pasted artifact against the issue's description; it does not require re-running the reporter's code.

## Honesty

Where it lives: in an eval bundle, the repro report's stated conclusion, read directly next to the Behavior-shown artifact above it. In live mode, the conclusion sentence(s) in the draft or posted repro comment.

What good looks like: the stated outcome (reproduced / could not reproduce / reproduced something adjacent) is exactly what the artifact next to it supports. An honest cannot-reproduce that names what was tried and what happened instead counts as complete, not evasive. A reproduced claim is never stretched to cover a symptom the pasted artifact doesn't actually show.

## Comms

Where it lives: in an eval bundle, the claim and repro comment text, read next to the repo's contribution policy / AI-disclosure statement in the repo-facts block. In live mode, the actual draft or posted comment, read next to the repo's CONTRIBUTING.md or README policy section.

What good looks like: every comment graded here is AI-assisted work by default — that's a given, never something to infer from whether the comment happens to mention AI. Two different policy shapes need two different proofs. An explicit-disclosure policy (names the tool, states the extent of use) needs an actual disclosure sentence in the comment — silence doesn't satisfy it, no matter how good the comment otherwise reads. A human-voice policy (comments must read as human-written, with no ask to announce AI use — an AI-sounding comment may just get hidden) needs the comment to read as specific and grounded in this issue's real detail, not generic or templated; it does not need a disclosure sentence, and demanding one there is applying the wrong shape's rule. The claim comment names the specific issue and promises investigation only — never a fix, a date, or a request to be assigned/reserved through flattery or urgency. The repro comment states findings in the poster's own words, not templated boilerplate interchangeable with any other issue.
