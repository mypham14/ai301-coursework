# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/61

**Verdict output**

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

```
 4 background agents launched (↓ to manage)
Fetch repo facts for pathreview repo
Fetch issue 61 details
Fetch issue 69 details
Fetch issue 73 details

I've kicked off four background fetches (repo facts + issues #61, #69, #73). I'll wait for those to come back before grading against the rubric.

✻ Cooked for 49s · done 1:40 AM

● Auto mode lets Claude handle permission prompts automatically — Claude checks each tool call for risky actions and prompt injection before executing. Actions Claude identifies as safe are executed, while actions Claude identifies as risky are blocked and Claude may try a different approach. Ideal for long-running tasks. Sessions are slightly more expensive. Claude can make mistakes that allow harmful commands to run, it's recommended to only use in isolated environments. Shift+Tab to change mode.

● Agent "Fetch issue 61 details" finished · 1m 6s

Issue #61 data is in. Waiting on the repo-facts fetch and issues #69/#73 before I grade anything.

✻ Crunched for 2s · done 1:41 AM

● Agent "Fetch issue 69 details" finished · 1m 38s

Issue #69 is in — two unassociated students have already claimed it in comments, but no maintainer/assignee action. Still waiting on repo facts and issue #73.

✻ Waiting for 2 background agents to finish

● Agent "Fetch repo facts for pathreview repo" finished · 2m 5s

Repo facts are in — active repo, no AI-contribution ban, no releases (fine, course repo). Still waiting on issue #73.

✻ Waiting for 1 background agent to finish

● Agent "Fetch issue 73 details" finished · 3m 10s

Summary

All three issues live in the scoped repo (codepath/pathreview-ai301-fa26-s3), which is active (last push 2026-09-16, not archived), has an active maintainer/collaborator (Aburke225) committing through 2026-09-16, and has no ban on AI-assisted contribution (docs/CONTRIBUTING.md is silent on it, and staff's own commits carry Co-Authored-By: Claude Opus 5). So maintainer_activity, repo_is_active, and policy_allows_contribution pass identically for all three.

Ranked (accepted, in fit order):

1. #61 — Health check DB probe passes raw SQL under SQLAlchemy 2.x (accept). Best fit: it's a SQLAlchemy/database bug, directly in your stated SQL/database wheelhouse. Zero comments, zero assignee, zero linked PR — completely uncontested. Precise repro steps and exact exception text given.
2. #73 — README/.env.example disagree on LLM API key var (accept). Clean, uncontested (repo has 0 PRs total, 0 comments), tightly bounded (2 files, verified real 3-line discrepancy, 1–2 hr estimate). Less SQL/DB-flavored than #61, but the lowest-friction pick if you want a guaranteed-fast, zero-contention first PR.
3. #69 — Output parser crashes on top-level JSON array fallback (accept). Also a well-scoped Python bug (specific file, specific fix, xfail marker to remove), but two classmates have already posted claim comments on it. Per this course's house rule, claim comments don't block you and don't sink the verdict — but given #61 and #73 are equally valid and uncontested, this is the least convenient of the three to pick up first.

[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/61",
    "checks": [
      {"name": "maintainer_activity", "grade": "pass", "evidence": "Collaborator Andrew Burke pushed commits through 2026-09-16; repo has an active human maintainer."},
      {"name": "repo_is_active", "grade": "pass", "evidence": "Not archived; pushed_at 2026-09-16, well within 365 days."},
      {"name": "scope_is_small_enough", "grade": "pass", "evidence": "Single file api/routes/health.py, exact root cause (raw 'SELECT 1' needs sqlalchemy.text()), exact repro/exception text given."},
      {"name": "nobody_else_is_on_it", "grade": "pass", "evidence": "0 comments, assignees: [], no linked or cross-referenced PRs in the timeline."},
      {"name": "policy_allows_contribution", "grade": "pass", "evidence": "docs/CONTRIBUTING.md has a standard workflow with no AI-contribution ban; staff commits themselves use Claude co-authorship."},
      {"name": "clear_beginner_signal", "grade": "pass", "evidence": "Labeled 'good first issue' and 'tier-1' (Starter difficulty)."},
      {"name": "decision_is_clear", "grade": "pass", "evidence": "Fix is unambiguous: wrap the literal SQL string in sqlalchemy.text()."}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/73",
    "checks": [
      {"name": "maintainer_activity", "grade": "pass", "evidence": "Collaborator Andrew Burke pushed commits the same day this issue was filed (2026-09-16)."},
      {"name": "repo_is_active", "grade": "pass", "evidence": "Not archived; pushed_at 2026-09-16."},
      {"name": "scope_is_small_enough", "grade": "pass", "evidence": "Two named files (README.md, .env.example), verified real discrepancy, 1-2 hr estimate."},
      {"name": "nobody_else_is_on_it", "grade": "pass", "evidence": "0 comments, assignees: [], and the entire repo has 0 pull requests (open or closed)."},
      {"name": "policy_allows_contribution", "grade": "pass", "evidence": "Same repo-level CONTRIBUTING.md; no AI-contribution ban."},
      {"name": "clear_beginner_signal", "grade": "pass", "evidence": "Labeled 'good first issue', 'docs', and 'tier-1'."},
      {"name": "decision_is_clear", "grade": "pass", "evidence": "Verified against core/config.py that OPENROUTER_API_KEY is a real, undocumented option; fix is to sync README and .env.example."}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/69",
    "checks": [
      {"name": "maintainer_activity", "grade": "pass", "evidence": "Collaborator Andrew Burke pushed commits through 2026-09-16; repo has an active maintainer."},
      {"name": "repo_is_active", "grade": "pass", "evidence": "Not archived; pushed_at 2026-09-16."},
      {"name": "scope_is_small_enough", "grade": "pass", "evidence": "Two named files, exact bug (.items() on a list), explicit xfail marker (H-02) to remove, 2-4 hr estimate."},
      {"name": "nobody_else_is_on_it", "grade": "pass", "evidence": "Two NONE-association students posted 'I'd like to work on this/claim this' (2026-09-20, 2026-09-21) with no maintainer response or assignment; per Path Review house rule, classmates' claim comments do not block a claim."},
      {"name": "policy_allows_contribution", "grade": "pass", "evidence": "Same repo-level CONTRIBUTING.md; no AI-contribution ban."},
      {"name": "clear_beginner_signal", "grade": "pass", "evidence": "Labeled 'good first issue' and 'tier-1'."},
      {"name": "decision_is_clear", "grade": "pass", "evidence": "Issue specifies the exact failure, the fix target, and the test to un-xfail."}
    ],
    "verdict": "accept"
  }
]

✻ Brewed for 5m 39s · done 1:44 AM

※ recap: Grading three candidate first issues from the pathreview course repo against your rubric. All three passed as accept, ranked #61, #73, #69 by fit; next step is to confirm which one you want to claim. (disable recaps in /config)

────────────────────────────────────────────────────────────────────────────
❯ claim issue 61
────────────────────────────────────────────────────────────────────────────
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

agreement: 17/20 scored items  (bar: 18/20: below the bar)
agreement: 20/20 scored items  (bar: 18/20: PASS)
agreement: 17/20 scored items  (bar: 18/20: below the bar)
agreement: 18/20 scored items  (bar: 18/20: PASS)

**Issue analysis**

issue-19. Gold label: accept (category clear-accept, note: "maintainer-diagnosed performance bug with named causes, unclaimed"). My rubric's verdict: reject. The issue lists two diagnosed causes (quadratic matchers, UI thread blocked on matching) plus three "additional suggestions" (multi-processing across cores, matching only expanded categories, threading the rewrite step too). My scope_is_small_enough check asks whether "there's one clear thing to build or fix and it's already decided what that is" — it doesn't separate the maintainer's core diagnosis from the optional follow-on ideas, so the grader reads the whole suggestions list as the scope and calls it not-yet-decided, producing a reject on a gold-accept item.

**Check rationale**

Check: scope_is_small_enough, as currently written in rubric.md:
"pass if there's one clear thing to build or fix and it's already decided what that is; being long, short, or spread across a few files doesn't automatically make it too big. Fail if it's actually open-ended: people still arguing over how it should work, a list of many unrelated issues to pick from, an ongoing 'fix this anywhere' task with no real end, a couple of failed attempts already, or nobody's agreed yet on what to even build."

Earlier drafts failed 3 of 8 clear-accept issues because the grading model was using length, terseness, and file count as stand-ins for scope. I rewrote the check to name the real test directly, decided vs. open-ended, and gave concrete markers of "actually open-ended" pulled from the eval's genuine scope-reject cases (an ongoing codebase-wide sweep, a tracker issue that's really a list of unrelated sub-issues, years of unresolved design debate, abandoned prior attempts). That fixed the false rejects without loosening the check enough to accept the issues that should reject.

**Trade-offs**

It still misses issue-01 and issue-19, both gold-accept. I drafted explicit carve-outs for a multi-file docs deliverable and for a diagnosed-bug-plus-optional-extras pattern, but rolled them back to keep the check short rather than growing it into a list of specific examples. I reran the eval after rolling back and confirmed agreement holds at 18/20 with exactly those two items flagged — no other item flipped, so the rollback traded a slightly higher score for a shorter, more general check.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. #61 is a single-file SQLAlchemy fix (raw SQL string needs sqlalchemy.text()) with an exact repro and exception given, matching my SQL/database interest directly and fits in the time I have this week.
2. The rubric correctly caught that it's uncontested (0 comments, no assignee, no linked PR), that the repo has an active maintainer and no AI-contribution ban, and that the fix is unambiguous. What I weighed beyond that: personal fit over #73 (equally valid but pure docs, less aligned with what I want to practice) and over #69 (rubric correctly says classmates' claim comments don't block me, but I still didn't want the minor race/awkwardness of two people already circling it).
3. Low difficulty anticipated: the root cause and fix are already spelled out in the issue, so the main risk is just confirming there's no complication around the raw-SQL health check, not diagnosing the bug itself.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
