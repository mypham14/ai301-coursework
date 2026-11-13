# Toolkit Portfolio

Path: `beat-3-portfolio/toolkit-portfolio.md`

This file is Assignment 6, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. It has been waiting here since you created the repo in unit 1;
unit 9 is when you write it.

**Only this page is read.** Your `tools/` folders and your installed skill folders are never
opened, so every piece of evidence you want seen has to live inline here: an output excerpt,
a before/after pair, a dated commit reference.

**Write every sentence yourself.** AI may gather diffs, dates and excerpts for you; it does
not draft entries.

**Keep every section header below under its exact name** — Featured Tools, Inventory, The
Arc, and Limitations and What's Next. Feature three or four tools, each in the entry shape
shown below; give every other tool one inventory line.

**Some lines below are labelled, and the label is how they are read.** Three places on this
page use a `Label: your words` shape — the arc's beats, the tool-limits list, and nothing
else. Keep those labels exactly as written, one per line, and never delete a labelled line:
where you have nothing to put after a label, write `none` and move on. A deleted label reads
as a deleted answer.

**Before you finish**, replace every `[bracketed prompt]` with your own words, then delete
the unused `[Tool name]` entry stub, its copy-me sentence, and the prompt sentences under
each header. A leftover bracketed prompt reads as unfilled.

<!-- **Student:** [Student name] -->
**Student:** My Pham

## Featured Tools

Three or four tools, presented in depth. Open with a few sentences
on why you chose these.

Full credit asks the featured set to show real change: **at least two** of your featured
entries should show a tool that visibly moved through use. An honest entry for a tool that
barely changed — saying so, dating what did happen, and giving the reason — earns the solid
middle tier and never fails you.

Each tool's honest limit is not written here. It goes in the tool-limits list at the
bottom of this page, one line per featured tool, so that the limits sit together and can be
read as a set.

<!-- [Why these tools, in your own words.]

Copy the entry shape below once per featured tool and fill it there.

**[Tool name]**

**What it is, and the judgment hole I wrote:** [what the tool does,
and the part of it that is your judgment: the rubric, strategy,
guide, manifest, or playbook you wrote.]

**How it evolved, dated:** [the changes that made it yours, each
with its date and the friction that drove it; your weekly Toolkit
entries are the source.]

**Evidence:** [show, don't claim: an output excerpt, a before/after
pair, or a dated commit reference, inline here. Draw it from your real
term work where you can - a catch this tool actually made, a
before/after from your own repo or PR - which is graded separately from
whether an artifact is present at all. A short, telling excerpt beats a
pasted transcript.] -->

I picked the four tools that sit on the path of my real contribution (ChurchCRM issue #8900, PR #11430) and that I changed because something went wrong: issue-scout picked the issue, plan-check reviewed my plan before I wrote code, pr-precheck (inside my ship-gate) judged the PR before I opened it, and the PR monitor watched it afterwards. Each one has a rubric or playbook I wrote and rewrote, so the changes are mine and dated.

**issue-scout**

**What it is, and the judgment hole I wrote:** issue-scout searches GitHub for first-contribution issues and grades each one against a rubric. The judgment is my search strategy and my rubric checks, especially `scope_is_small_enough`, which decides whether an issue has one clear thing to build.

**How it evolved, dated:** 9/2026: my first `scope_is_small_enough` failed 3 of 8 clear-accept issues, because the grader used length and file count as stand-ins for scope. I rewrote it around "decided vs open-ended", and my run history went 17/20, 20/20, 17/20, 18/20 against a bar of 18/20. November 2026 (Unit 6): the check rejected my #8900 candidates as too open, so I replaced it with `scope-feasible`, added difficulty-note guidance and wrote the regrade cautions into the scout's notes.

**Evidence:** run history from `beat-1-sandbox/unit-1/selection.md`: `agreement: 17/20` → `20/20` → `17/20` → `18/20 (bar: 18/20: PASS)`. After the November patch, the scout accepted all three of my candidates and rated #8900 straightforward, #819 moderate and WebDevTools #159 ambitious; I claimed #8900 at https://github.com/ChurchCRM/CRM/issues/8900#issuecomment-6760912314.

**plan-check**

**What it is, and the judgment hole I wrote:** plan-check grades a plan and its plan comment against the issue and the reproduction evidence. The judgment is my rubric's three required checks: Diagnosis, Scope and Test.

**How it evolved, dated:** 10/2026 (Unit 3): the sample Diagnosis check passed a plan that only said what caused the bug. When my group graded `calib-03`, it passed a plan whose cause (a pager key binding) contradicted the repro evidence. I rewrote Diagnosis so the cause is read against the repro evidence and the change must target the cause, not the symptom. The run scored 19/20.

**Evidence:** my rewritten check, as it reads now: "The cause the plan names is consistent with the repro evidence, and the planned change targets that cause, not just the symptom". On my own work (2026-11-11) its first run rejected my #8900 drafts for having no observed output, no named files and no measurable test expectations. I captured the baseline CSV (`"Name","Address","Email","Classification","Family Role"` / `"Church Admin","","","Unassigned","Unassigned"`), named the three files and the expected headers, and the second run accepted the plan.

**pr-precheck (my ship-gate)**

**What it is, and the judgment hole I wrote:** pr-precheck grades a pull request against the plan it claims to implement. My ship-gate runs it after a manifest I wrote with three must-pass checks (lint, build, my Cart CSV Cypress spec). The judgment is the rubric, above all the "Test evidence is real" check.

**How it evolved, dated:** 10/2026 (Unit 4): my first version demanded printed output for every result and scored 13/20, rejecting every clear-accept. A second version still wanted pasted output and re-graded the clear-accepts at 1/7. Reading the not-tested packages showed the gold labels reject PRs that never re-run the plan's repro, not PRs with compact evidence, so I rewrote the check to ask for the same scenario re-run with a stated before and after result. It then scored 20/20. 2026-11-11: I wrote the gate manifest for the Cart branch.

**Evidence:** `agreement: 13/20` → `agreement: 20/20 (bar: 18/20: PASS)`. On my real branch the gate log of 2026-11-11 22:55 says BLOCK, and the failed check was: "Template and policy followed [fail]: [...] the Screenshots section only says 'Upload these six PNGs to the PR' with filenames, no actual screenshots." I reworded the Screenshots section, and the next run ended "PASS ... Verdict: accept". I later embedded six real screenshots in the PR.

**PR monitor and playbook**

**What it is, and the judgment hole I wrote:** the monitor reads my open PR and drafts what to do next. The judgment is my playbook, written from five recent merged outside PRs in ChurchCRM/CRM (first maintainer reply in about 1 day, merge in 0.1 to 4.2 days): nudge after 7 quiet days, walk away after 30, and a bot review is never a maintainer reply.

**How it evolved, dated:** 2026-11-12 (Unit 8): the playbook rule that a bot review does not count as maintainer engagement is what stopped the monitor from treating CodeRabbit's approval as a human reply. The monitor's `--state` option also crashed on Windows until I ran Python in UTF-8 mode.

**Evidence:** the 2026-11-12 run drafted "Don't send anything yet. Wait." (correct: only a bot had reviewed). The same draft claimed CodeRabbit's description check had flagged my PR description. I opened CodeRabbit's comment: that check passed with no complaint. So I did not edit the PR body, and my next nudge date is about 2026-11-19.

## Inventory

One line each for the rest of the toolkit: what it is and where it
stands (one bullet per tool; a compact sentence or two is fine).

<!-- - [Tool name: one line.] -->
- repro-check: grades my claim comment and reproduction report; its rubric stays strict on exact OS and version (`env-recorded`), and in Unit 6 I swapped its sandbox scope for the wild scope after my first claim check failed. It accepted my #8900 report.
- Voice guard and voice guide: a hook that checks my upstream comments against my claim, question and ping rules (added 2026-11-10); the wiring test was blocked as expected because the comment had no body.
- Repo memory (`CLAUDE.local.md`): my short notes on ChurchCRM conventions, kept out of the PR through `.git/info/exclude` (2026-11-10); it is still current.

## The Arc

The story of the toolkit in order: friction, change, result, dated.
Your weekly Toolkit entries are the source; read your own record and
tell it oldest first.

Write one block per beat, in the shape below, oldest first. Keep all four labels on every
beat, each on its own line. The `Result:` line is what the beat was for: it says what
happened *after* the change, which is not the same sentence as the change itself. If a beat
never reached a result, write `Result: none` — an honest beat that did not land still counts
fully as a beat.

Full credit asks for **at least two** beats whose `Result:` line names something that
actually happened afterwards. A beat about something that did not work counts fully as a
beat, and when the work happened never moves this score.

<!-- **Beat 1**
Date: [a date, or a month and year]
Friction: [what was going wrong, and how you noticed]
Change: [what you changed because of it]
Result: [what happened afterwards, or the word none]

**Beat 2**
Date: [a date, or a month and year]
Friction: [what was going wrong, and how you noticed]
Change: [what you changed because of it]
Result: [what happened afterwards, or the word none]

Copy the beat shape above for as many beats as your term had. -->

**Beat 1**
Date: 9-10/2026
Friction: in the sandbox my tools graded wrongly: my issue-scout `scope_is_small_enough` check failed 3 of 8 clear-accept issues (17/20), my first pr-precheck check scored 13/20 by rejecting every clear-accept package, and the sample plan-check Diagnosis check passed a plan whose cause contradicted the repro evidence (`calib-03`); I noticed each from the eval run.
Change: I rewrote all three checks: scope now asks whether the work is decided or open-ended, "Test evidence is real" now asks for the same scenario re-run with a stated before and after result, and Diagnosis now reads the plan's cause against the repro evidence.
Result: the runs passed their bars (issue-scout 18/20, pr-precheck 20/20, plan-check 19/20), and the two not-tested canaries I re-ran with `--only` (`pkg-04`, `pkg-14`) still graded reject.

**Beat 2**
Date: 11/2026
Friction: on my real contribution (#8900), four things stopped me:
1. My scout's scope check rejected my #8900 candidates as too open.
2. My first plan-check run rejected my drafts: no observed output, no named files, no measurable test expectations.
3. On 2026-11-11 my ship-gate said BLOCK, because my PR description's Screenshots section only listed filenames.
4. On 2026-11-12 my PR monitor claimed CodeRabbit's description check had flagged my PR description, which looked wrong to me.

Change: what I did about each one, in the same order:
1. I replaced `scope_is_small_enough` with `scope-feasible`.
2. I captured the baseline Cart CSV and named the three files and the expected headers in the plan.
3. I reworded the Screenshots section and later embedded the six real screenshots.
4. I opened CodeRabbit's comment and checked the monitor's claim myself, then followed my playbook's "wait" advice.

Result: what happened after each change:
1. The scout accepted all three candidates, so I claimed #8900 (https://github.com/ChurchCRM/CRM/issues/8900#issuecomment-6760912314).
2. Plan-check accepted the plan.
3. The next gate run passed with "Verdict: accept", and I opened PR #11430 on 2026-11-12.
4. CodeRabbit's check had in fact passed, so I avoided an unneeded edit. The PR is still open, with a bot approval at 18:32 UTC and no human maintainer reply, and my next nudge is about 2026-11-19.

## Limitations and What's Next

What the toolkit as a whole still does not do, and what you would
build or change next. Write this section last; it will be better
after the rest.

<!-- [The honest limits, and what comes next, in your own sentences.] -->

My toolkit has only been used on one real contribution, so most of it is tuned to a single PHP repo and a single issue. Every tool grades text I give it: none can run my code or tell whether a pasted result is true. The one tool that tried to read the outside world, the PR monitor, got a fact wrong. Next I would add a check to plan-check that reads the repo's contribution policy against the plan comment, make the monitor quote the source line behind each claim it makes, and look into why the existing `standard.cart.spec.js` fails on unchanged `master`.

**Tool limits.** One line per featured tool, in the same order as the entries above, naming
where that tool still stops or what it still gets wrong. A small honest limit is full credit;
"none really" is not a limit. If a tool genuinely has none you can name, write `none` after
its label rather than deleting the line.

<!-- issue-scout: [where this tool still stops, in plain words, or the word none]
[tool name]: [where this tool still stops, in plain words, or the word none] -->
issue-scout: its maintainer-activity check accepts a repo on one non-owner merge even when most merges go to one person, and it still misses two gold-accept items (issue-01, issue-19).
plan-check: it has no check that reads a repo's contribution policy, so it accepted `pkg-20` where the gold label rejects for a missing AI-use disclosure.
pr-precheck: a plausible before-and-after note with no real output now passes "Test evidence is real", because the package text cannot show whether the note is true.
PR monitor: it stated a false fact about CodeRabbit's review, and its `--state` option crashes on Windows unless Python runs in UTF-8 mode.
