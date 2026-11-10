# Contribution: [Feature] Cart CSV export: field selector for custom column selection

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** 1  
**Student:** My Pham  
**GitHub Username:** mypham14  
**Issue:** https://github.com/ChurchCRM/CRM/issues/8900  
**Pull Request:** [GitHub PR link, once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

<!-- [one line on the tool work] -->
**Unit 5: Building and testing my issue-scout search strategy**

<!-- [2-4 sentences: what you built (the scout, your strategy), what broke
or fell short, what changed because of it.] -->
I built a profile-based search strategy for my issue-scout and ran it across Python, JavaScript/React, and SQL issues. The scan's top five all came from the Python query, and when I compared direct grading with my manual review, the scout marked ChurchCRM #819 unclear because I had not verified that the chart was absent from the member dashboard. I saved the scout's readout and will verify that premise before committing to the issue, rather than changing the rubric without evidence that it graded the check incorrectly.

**Unit 6: Installing my voice guard and repo memory**

During Unit 6, I added upstream claim, question, and ping rules to my voice guide, installed the voice guard, and verified the hook; the wiring test was blocked as expected because the comment command had no body. I created a concise `CLAUDE.local.md` for my CRM clone and excluded it through `.git/info/exclude` so it stays out of the PR. My first claim check failed because repro-check still had its sandbox scope, so I installed the wild scope and reran it successfully; while orienting in the code, I found that the Cart page has no CSV export action although the admin exporter can export selected fields from the cart, so I need maintainer clarification before coding.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I used issue-scout to grade three issues I had picked: ChurchCRM #8900, ChurchCRM #819, and WebDevTools #159. After I patched the scope check, the scout accepted all three and rated #8900 straightforward, #819 moderate, and #159 ambitious. I chose #8900 because it is a bounded CSV-export feature that connects to my data-science and analytics experience, and the maintainer pointed to an existing field-selection pattern in `src/CSVExport.php`. I hope to learn how ChurchCRM's PHP export flow is organized and how that pattern can be applied to the Cart export.

The scout helped me compare the options, but I treated its verdict as evidence rather than a decision: its maintainer-activity check accepts ChurchCRM based on a non-owner merge even though most merges are concentrated with DawoudIO. It also surfaced the uncertainty around #819 and the old assignment on #159, which I would want to verify before choosing either. I selected #8900 because its scope and implementation precedent were clearer. I posted my claim here: https://github.com/ChurchCRM/CRM/issues/8900#issuecomment-6760912314.

**Toolkit**

- Tool: issue-scout (your scout: your search strategy, your rubric),
  used in direct mode to grade my three candidate issues
- What it produced: accepted verdicts for #8900, #819, and #159, with
  straightforward, moderate, and ambitious difficulty notes respectively;
  I chose #8900
- Difficulties: the original scope check treated open edges too strictly,
  and the maintainer-activity result needs human judgment when merges are
  concentrated among a small number of contributors
- What changed: I replaced `scope_is_small_enough` with `scope-feasible`,
  added the difficulty-note guidance, and recorded the patch and regrade
  cautions in the scout's notes

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

[What a stranger would need to stand this up: your operating system,
the relevant versions (language, runtime, package manager, the project
itself), and how you set the project up. Then the challenges you faced
and how you solved them, or that none came up.]

**Steps to Reproduce**

1. [Step 1]
2. [Step 2]
3. [Observed result]

**Reproduction Evidence**

- **Commit showing reproduction:** [link to commit in your fork]
- **Screenshots/logs:** [if applicable]
- **My findings:** [what you discovered during reproduction; an
  honest, evidenced cannot-reproduce that led you to re-pick or ask
  the maintainer belongs here and costs you nothing]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
