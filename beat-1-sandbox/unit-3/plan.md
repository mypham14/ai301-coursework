# Plan for #18: repo analyzer never receives a file list, so `has_tests` and `has_ci` are always `False`

Issue: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/18
Branch (on my fork): `fix/18-github-tool-file-structure`

## Repro evidence I rely on

From my reproduction comment on #18 (local commit `2f4e82f52efbcfcc57d65b3fa5348672163ca088`, Windows 11, Python 3.14.2, unauthenticated `GitHubTool()`):

```text
Tool success: True
Returned keys: ['description', 'fork_count', 'has_readme', 'homepage', 'last_commit_date', 'name', 'open_issues_count', 'primary_language', 'star_count', 'topics']
file_structure present: False
ACTUAL has_tests: False
ACTUAL has_ci: False
CONTROL has_tests: True
CONTROL has_ci: True
```

The CONTROL lines come from running `RepoAnalyzer().parse(...)` on the same dictionary with a `file_structure` list added by hand (`tests/__init__.py`, `tests/conftest.py`, `.github/workflows/ci.yml`, `.github/workflows/eval.yml`). My own words from the report: "The tool succeeds, but the returned metadata does not include `file_structure`. Both detection flags are `False` even though the target repository contains a `tests/` directory and `.github/workflows` files. When the same dictionary is given a `file_structure` list, the analyzer flips to `True` for both flags."

## Diagnosis

The cause is in `GitHubTool`, not in the analyzer. `GitHubTool._fetch_repo_metadata` in `agent/tools/github_tool.py` builds a dictionary with ten keys and none of them is `file_structure`. `RepoAnalyzer._detect_tests` and `RepoAnalyzer._detect_ci` in `ingestion/parsers/repo_analyzer.py` both read `repo_data.get("file_structure", "")`, so they always get an empty string and return `False`.

What the repro shows:
- `file_structure present: False` shows the key is missing from the tool's output (the missing input).
- The two `CONTROL` lines show the analyzer already gives the right answer when it is handed a file list (so the analyzer's detection logic is not the problem for these two flags).

What I have not shown: I have not run the fix yet, and I have not checked what happens for very large repositories (see Risks and unknowns).

## Scope

In scope (one change):
- Make `GitHubTool` fetch the repository's file list from GitHub and return it under the key `file_structure`.
- Add unit tests for that change.

Not in scope (I am deliberately leaving these alone):
- The analyzer's detection logic in `ingestion/parsers/repo_analyzer.py` (no edits to `_detect_tests`, `_detect_ci`, or the tech-stack detection).
- Key-name differences between the tool and the analyzer (for example the tool returns `star_count` and `primary_language`, while the analyzer reads `stargazers_count` and `language`). That looks like a separate problem and I will mention it on the issue rather than fix it here.
- Changes to the orchestrator in `agent/orchestrator.py`, rate-limit handling, or authentication.

## Files I will touch

- `agent/tools/github_tool.py`: add the file-list fetch and the `file_structure` key.
- `tests/unit/test_github_tool.py` (new file): unit tests for the change.

I expect no edits to any other file.

## Approach

1. In `_fetch_repo_metadata`, read `default_branch` from the repository response that is already fetched.
2. Add a small helper method on `GitHubTool` (for example `_fetch_file_structure(username, repo_name, branch)`) that calls GitHub's tree endpoint, `GET /repos/{username}/{repo_name}/git/trees/{branch}?recursive=1`, and returns a list of file paths (the `path` of each item in `tree`). It sends the same optional `Authorization` header as the other requests.
3. If that request fails, or GitHub marks the result as truncated, the helper returns an empty list instead of raising, so the tool still returns the other metadata (same style as `_has_readme`, which returns `False` on error).
4. Add `"file_structure": <that list>` to the metadata dictionary.
5. Add unit tests in `tests/unit/test_github_tool.py` that replace the network calls (using `monkeypatch` on `httpx.get` and `httpx.head`) so they do not need internet access:
   - a response with test and workflow paths gives `file_structure` containing them, and passing the tool's output to `RepoAnalyzer().parse(...)` gives `has_tests` and `has_ci` as `True`;
   - a failed tree request gives an empty `file_structure` and the tool still succeeds.
6. Run `make check && make test-unit` (the commands the contributing guide asks for) before pushing.

## Test plan

Re-run my unit 2 reproduction script unchanged (the same script from my report, on the same repo `codepath/pathreview-ai301-fa26-s3`) after the change.

Before the fix (already observed):

```text
file_structure present: False
ACTUAL has_tests: False
ACTUAL has_ci: False
```

What I expect to see after the fix:

```text
Tool success: True
file_structure present: True
ACTUAL has_tests: True
ACTUAL has_ci: True
```

and `file_structure` should appear in the `Returned keys:` list. The `CONTROL` lines should stay `True` and `True`. That result would fail before the change and pass after, so it shows the fix works. I will paste the before and after output into the pull request.

The new unit tests give the same check without internet access. They should fail on `main` and pass on my branch.

## Risks and unknowns

- Truncated results for big repositories: GitHub can cut off a recursive file list when a repository is very large. The Path Review repo is small, so my repro will not hit this. My plan is to return an empty list in that case, which means `has_tests` and `has_ci` would be `False` for such a repo, same as today. I have not tested a large repo.
- Extra request and rate limits: this adds one more GitHub request per repository. Unauthenticated requests are limited, so the extra call could hit the limit sooner. I have not measured this.
- Tech stack results will change: `_detect_tech_stack` in the analyzer also reads `file_structure`, so `tech_stack` will start filling in once the list exists. I believe that is the intended behavior, but it is a visible side effect and I have not checked whether anything depends on it being empty.
- Format of `file_structure`: the analyzer turns the value into text with `str(...)`, so a list of paths works (my CONTROL run used a list). I do not know whether maintainers would prefer a list or one text string, and I will follow their preference if they have one.
- Test style: I am assuming `monkeypatch` on `httpx.get` fits how tests are written here. I have not found an existing test for `GitHubTool` to copy from.

## Deviations

Nothing changed in the approach: the plan held. I changed only `agent/tools/github_tool.py` and added `tests/unit/test_github_tool.py`, as planned, and left the analyzer alone.

Small details the plan did not spell out:
- If GitHub's repository response has no `default_branch`, the new helper falls back to `HEAD` when asking for the file list.
- I wrote four unit tests, not the two I listed. The extra two cover a failed file-list request and a truncated one (both return an empty list and the tool still succeeds), which my risks section had flagged.
- The tests use a small fake response class together with `monkeypatch` on `httpx.get` and `httpx.head`, so they need no internet access.

Results: after the change, my unit 2 repro printed `file_structure present: True`, `ACTUAL has_tests: True` and `ACTUAL has_ci: True` (before: `False` and `False`). The four new tests pass, the full unit suite shows 379 passed and 53 xfailed (the project's expected failures) with no failures, and `ruff check .`, `black --check .` and `mypy api/ core/ ingestion/ rag/ agent/ safety/` all pass (mypy: no issues found in 76 source files).

Still not checked: very large repositories (the truncated case is only covered by a mocked test, not a real big repo) and whether the extra GitHub request matters for rate limits.
