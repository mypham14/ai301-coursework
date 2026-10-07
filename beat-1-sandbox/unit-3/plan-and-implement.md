# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

<!-- Your GitHub username, exactly as it appears on your profile - no @, no
profile URL. Your comment upstream is identified by this name, and it is
the only thing that ties it to you. Several students may plan the same
house issue, so this is what keeps their comments off your score and
yours off theirs. -->

mypham14

**Plan comment**

<!-- Link to the comment where you posted your plan on the issue. Use the comment's own
permalink. **Then paste the text of that comment underneath the link** — the pasted text is
what this field is graded on, so copy across what you actually posted. -->

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/18#issuecomment-6030593171

## Plan for #18

I reproduced this on my fork (commit `2f4e82f`, Windows 11, Python 3.14.2, unauthenticated). The tool succeeds but its output has no `file_structure` key, and the analyzer then reports both flags as `False`:

```text
file_structure present: False
ACTUAL has_tests: False
ACTUAL has_ci: False
CONTROL has_tests: True
CONTROL has_ci: True
```

The `CONTROL` lines are the same dictionary with a file list added by hand, so the analyzer already gives the right answer when it gets a file list.

**What I think the cause is:** `GitHubTool._fetch_repo_metadata` in `agent/tools/github_tool.py` never fetches the repo's file list, and `_detect_tests()` and `_detect_ci()` in `ingestion/parsers/repo_analyzer.py` both read `repo_data.get("file_structure", "")`. My repro supports this, but I have not run a fix yet.

**What I plan to change (one change):** make `GitHubTool` ask GitHub for the repo's file list (the `git/trees/{branch}?recursive=1` endpoint) and return the paths under `file_structure`. If that request fails or comes back truncated, it returns an empty list and the tool still returns the rest of the metadata. I'd add unit tests in a new `tests/unit/test_github_tool.py` that don't need internet access.

**What I'm leaving alone:** the analyzer's detection logic, the orchestrator, and rate-limit or auth handling. I also noticed the tool's key names (`star_count`, `primary_language`) differ from what the analyzer reads (`stargazers_count`, `language`). That looks like a separate problem, so I'm not touching it here.

**How I'll check it worked:** re-run the same script from my repro. I expect `file_structure present: True`, `ACTUAL has_tests: True` and `ACTUAL has_ci: True`, and I'll paste the before and after output in the PR.

**What I haven't checked yet:** very large repos (GitHub can truncate a recursive file list), whether the extra request matters for rate limits, and that `tech_stack` will now start filling in because the analyzer reads the same key.

Does this approach work for you? If you'd prefer `file_structure` in a different shape than a list of paths, or want me to handle truncation differently, I'll follow that before I start.

---

## Your branch

**Branch**

<!-- The name of the branch you built the change on, exactly as it appears in your fork. The
naming shape is a type prefix, then the issue number, then a short description. **The issue
number in the branch name must be the number of the issue you claimed** — a name carrying
any other number does not satisfy this field. -->

fix/18-github-tool-file-structure

**Evidence**

<!-- Your Unit 2 reproduction steps re-run against the built change: the before, then the
after. Paste both, including the commands you ran and their output. -->

**Before the fix** (my Unit 2 reproduction on `main`, commit `2f4e82f`; script run from the repo root):

```python
from agent.tools.github_tool import GitHubTool
from ingestion.parsers.repo_analyzer import RepoAnalyzer

owner = "codepath"
repo = "pathreview-ai301-fa26-s3"

result = GitHubTool().execute({
    "github_username": owner,
    "repo_name": repo,
})
print("Tool success:", result.success)
print("Returned keys:", sorted(result.data))
print("file_structure present:", "file_structure" in result.data)

analysis = RepoAnalyzer().parse(result.data)
print("ACTUAL has_tests:", analysis.metadata["has_tests"])
print("ACTUAL has_ci:", analysis.metadata["has_ci"])

control = RepoAnalyzer().parse({**result.data, "file_structure": [
    "tests/__init__.py",
    "tests/conftest.py",
    ".github/workflows/ci.yml",
    ".github/workflows/eval.yml",
]})
print("CONTROL has_tests:", control.metadata["has_tests"])
print("CONTROL has_ci:", control.metadata["has_ci"])
```

```text
Tool success: True
Returned keys: ['description', 'fork_count', 'has_readme', 'homepage', 'last_commit_date', 'name', 'open_issues_count', 'primary_language', 'star_count', 'topics']
file_structure present: False
ACTUAL has_tests: False
ACTUAL has_ci: False
CONTROL has_tests: True
CONTROL has_ci: True
```

**After the fix** (branch `fix/18-github-tool-file-structure`, commit `495d197`; the same tool-to-analyzer path against the same repository, run as a one-line command without the CONTROL lines):

```text
python -c "from agent.tools.github_tool import GitHubTool; from ingestion.parsers.repo_analyzer import RepoAnalyzer; r=GitHubTool().execute({'github_username':'codepath','repo_name':'pathreview-ai301-fa26-s3'}); print('Tool success:', r.success); print('Returned keys:', sorted(r.data)); print('file_structure present:', 'file_structure' in r.data); a=RepoAnalyzer().parse(r.data); print('ACTUAL has_tests:', a.metadata['has_tests']); print('ACTUAL has_ci:', a.metadata['has_ci'])"
2026-10-06 23:17:51 [info     ] github_repo_fetched            language=Python repo=pathreview-ai301-fa26-s3 stars=6 username=codepath
Tool success: True
Returned keys: ['description', 'file_structure', 'fork_count', 'has_readme', 'homepage', 'last_commit_date', 'name', 'open_issues_count', 'primary_language', 'star_count', 'topics']
file_structure present: True
ACTUAL has_tests: True
ACTUAL has_ci: True
```

Other checks on the branch: `python -m pytest tests/unit/test_github_tool.py -v` gave `4 passed`; the full unit suite gave `379 passed, 53 xfailed`; `ruff check .` and `black --check .` passed; `mypy api/ core/ ingestion/ rag/ agent/ safety/` gave `Success: no issues found in 76 source files`.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

<!-- The agreement score of each run you did, in order. A single run is a complete answer if
only one run occurred. **The last score in your list must match the agreement line in the
`eval-run.txt` you committed** — that file is the record of your final run. -->

```text
agreement: 19/20 scored items  (bar: 18/20: PASS)
```

**Package analysis**

<!-- Pick one scored package (`pkg-01` through `pkg-20` — the four `calib-` packages are never
scored). Name it by id, say what your rubric decided and what the gold label said, and
explain why your rubric read it that way. -->

I chose `pkg-20` (ghostty-org/ghostty#11261). My rubric decided `accept`; the gold label says `reject` (category `thread-convention`), so this is the one package I disagreed with (`pkg-20  thread-convention  reject  accept  NO`).

My rubric read it as ready because all three of its checks pass on the plan itself. Diagnosis passes: the plan's cause, a stale `prev` pointer after the page reallocates mid-print, matches the repro artifact (the panic in `appendGrapheme`) and the control that removes the hyperlink start and passes. Scope passes: it names `Terminal.print`'s grapheme paths and a page change-detection counter, and says what it won't do (recompute `prev` unconditionally, or refactor page memory management). Test passes: both of the issue's fuzz cases plus the no-hyperlink control, run with `zig build test`, would fail before the fix and pass after.

The gold label's note says: "excellent bounded plan that follows the thread's direction, but the comment contains no AI-use disclosure and ghostty's stated policy requires disclosing all AI usage". The package's repo facts do state that policy ("All AI usage in any form must be disclosed"), and the plan comment has no disclosure. My rubric has no check that reads the repo's contribution policy against the comment, so it could not see that.

**Check rationale**

<!-- Quote one check from the `rubric.md` you uploaded to `tools/plan-check/`, exactly as it reads now.
Then say why it reads that way — what you revised to get there, or what you rejected in
favour of it. -->

> `| Diagnosis | The plan's stated cause, read against what the repro evidence shows (the error, output, and behavior) | The cause the plan names is consistent with the repro evidence, and the planned change targets that cause, not just the symptom | required |`

The sample rubric's version of this check was "passes if the plan says what causes the bug". When my group graded `calib-03` with it, it passed a plan whose stated cause (a pager key-binding bug, taken from a thread comment) contradicts the repro evidence, which shows the same slowness with no pager involved. I rewrote the check so the plan's cause is read against what the repro evidence actually shows, and so the change has to target that cause, not just the symptom. It reads that way because the check has to catch a confident, well-written plan with the wrong cause. A cause that merely appears in the plan is not enough.

**Trade-offs**

<!-- Every check gives something up. Any one of these is a complete answer: a package whose
result it changes, a canary you re-ran with `--only`, a case you accept it will miss, or a
stated reason nothing changed elsewhere. "Nothing changed, and here is how I know" earns
the point in full when the reason follows. -->

A case it will miss: a plan that is sound on diagnosis, scope and test but whose comment breaks the repo's stated contribution rules. `pkg-20` is the example. My rubric has only three checks (Diagnosis, Scope, Test), all `required`, and none reads the repo-facts block's contribution policy against the plan comment, so it graded `accept` where the gold label says `reject` for the missing AI-use disclosure. In the last full run that was the only miss, and the categories line shows it: `thread-convention 1/2`, while `clear-accept 7/7`, `scope-creep 4/4`, `unbuildable 3/3` and `wrong-cause 4/4` all matched. I accept this trade-off because adding a comms check would change how all 20 packages are graded, and the run already cleared the bar (19/20 against 18/20).

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
