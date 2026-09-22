# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer_activity | repo-facts block: last 5 default-branch commits, issue comments, author_association values in the comment thread | pass if the repo shows recent human maintainership (recent commits, releases, or maintainer responses) and there is no hint that the project is abandoned; do not require a maintainer comment on the issue itself when the repo is clearly active and the task is bounded. Fail only when the repo is stale or lacks any clear maintainer signal. | required |
| repo_is_active | repo-facts block: latest release, last push to any branch, archived flag, stars | pass if the repo is not archived and has a release or a push within the last 365 days. Fail if repos have no recent activity or an archived banner. | required |
| scope_is_small_enough | issue body, comment thread, issue labels or linked design discussion | pass if there's one clear thing to build or fix and it's already decided what that is; being long, short, or spread across a few files doesn't automatically make it too big. Fail if it's actually open-ended: people still arguing over how it should work, a list of many unrelated issues to pick from, an ongoing "fix this anywhere" task with no real end, a couple of failed attempts already, or nobody's agreed yet on what to even build. | required |
| nobody_else_is_on_it | issue assignees, linked PRs, comment thread claim comments | pass if no one is actively assigned, no open linked PR is already handling it, no clear "I'm taking this" claim that is still active. Closed or abandoned attempts don't automatically fail. | required |
| policy_allows_contribution | repo-facts block: contribution policy, issue or PR templates, any AI policy or AGENTS file mentioned in the repo facts | pass if the repo doesn't explicitly ban the contribution workflow you plan to use. Conditions like disclosure, human review or personal understanding are acceptable. Fail if it's a direct ban. | required |
| clear_beginner_signal | issue body, comment thread, labels | preferred only: pass if the issue has a clear beginner-friendly signal such as a small fix, a reproducible bug, a "good first issue" label, or a maintainer explicitly describing it as manageable for a newcomer. A maintainer/collaborator filing a terse but concrete bug report, or a maintainer naming a diagnosed root cause, counts even without the literal label. | preferred |
| decision_is_clear | issue body, comment thread | preferred only: pass if the change target is nameable from the issue text even if not exhaustively enumerated (e.g. extend an existing pattern to more named cases, or fix a named root cause) without major design churn or missing requirements. Do not fail only because the issue is long, multi-part, or lists optional extra ideas alongside the core ask. | preferred |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict, they
rank accepted issues; unclear counts as fail." -->
Accept the issue only if every required check passes. Preferred do not change the verdict; they only help rank issues that are already acceptable. If any required check is unclear, treat it as a fail, so the issue is rejected unless the evidence clearly supports a pass.