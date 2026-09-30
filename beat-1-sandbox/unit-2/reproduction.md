# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**mypham14**

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/18

I'm investigating #18. I’m tracing the repo metadata flow to confirm whether `GitHubTool` is returning repository metadata without a `file_structure` field, because `_detect_tests()` and `_detect_ci()` in `ingestion/parsers/repo_analyzer.py` both read `repo_data.get("file_structure", "")`. That means the analyzer can report `has_tests=False` and `has_ci=False` even when the repo clearly contains a `tests/` directory and GitHub Actions workflows. I’m reproducing that path locally and will post the exact output here before making any change.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/18

## Reproduction Report

### Environment
- Windows 11
- Python 3.14.2
- Local fork: https://github.com/mypham14/pathreview-ai301-fa26-s3
- Local commit: 2f4e82f52efbcfcc57d65b3fa5348672163ca088
- Target repository: https://github.com/codepath/pathreview-ai301-fa26-s3
- GitHub requests were unauthenticated; `GitHubTool()` was constructed without a token.

### Steps to reproduce
1. Clone the fork and activate the project virtual environment.
2. Run the following script from the repo root:

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

3. Compare the output to the repository contents.

### Observed output
```text
Tool success: True
Returned keys: ['description', 'fork_count', 'has_readme', 'homepage', 'last_commit_date', 'name', 'open_issues_count', 'primary_language', 'star_count', 'topics']
file_structure present: False
ACTUAL has_tests: False
ACTUAL has_ci: False
CONTROL has_tests: True
CONTROL has_ci: True
```

### Outcome
The tool succeeds, but the returned metadata does not include `file_structure`. Both detection flags are `False` even though the target repository contains a `tests/` directory and `.github/workflows` files. When the same dictionary is given a `file_structure` list, the analyzer flips to `True` for both flags. This matches the reported issue at the direct tool-to-parser boundary and supports the root cause described in #18.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

```text
agreement: 19/20 scored items  (bar: 18/20: PASS)
agreement: 19/20 scored items  (bar: 18/20: PASS)
agreement: 20/20 scored items  (bar: 18/20: PASS)
```

**Package analysis**

I chose `pkg-03`. The rubric decided `accept` and the gold label also said `accept`. This package is a good example of a reproduction package that was precise about the environment, included the exact command sequence, and pasted the output showing the failure; because the issue-specific artifact matched the behavior named in the issue, the rubric and gold label agreed.

**Check rationale**

> `env-recorded | The repro report's environment/setup section. | Pass if it states the exact tool/library version or commit hash and the OS, both as real values (e.g. "SQLAlchemy 2.0.31, Windows 11"). Fail if either is missing, vague ("latest", "my machine", "should work anywhere"), or contradicts the issue's stated environment.`

This check is the tightest guard against vague reproduction reports. I kept it because a real bug report needs environment details that a stranger could use to compare their own setup; without exact OS and version info, a report can sound plausible while not actually proving the issue. I did not weaken it to allow vague “works on my machine” wording, because that would hide the difference between a verified bug and an untested guess.

**Trade-offs**

The trade-off I accepted is that a reproduction is only as strong as the evidence it pastes. I kept the rubric strict on environment detail and issue-matching output instead of broadening the checks to allow hand-wavy “looks fine” reports. This is why the evaluation stayed stable: the model reached the full 20/20 agreement bar, and the accepted packages were the ones with concrete environment, steps, and pasted output that matched the issue exactly.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
