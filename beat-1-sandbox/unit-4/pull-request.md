# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

<!-- Link to the pull request you opened. It must be the pull request's own page on the Path
Review repo, not your fork's branch page. -->

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/225

**Branch**

<!-- The name of the branch the pull request comes from, exactly as it appears in your fork.
The naming shape is a type prefix, then the issue number, then a short description. **The
issue number in the branch name must be the number of the issue the pull request fixes** —
a name carrying any other number does not satisfy this field. -->

`fix/18-github-tool-file-structure`

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

<!-- The agreement score of each run you did, in order. A single run is a complete answer if
only one run occurred. **The last score in your list must match the agreement line in the
`eval-run.txt` you committed** — that file is the record of your final run. -->

1. `agreement: 13/20 scored items`
2. `agreement: 20/20 scored items  (bar: 18/20: PASS)`

**Package analysis**

<!-- Pick one scored package (`pkg-01` through `pkg-20` — the four `calib-` packages are never
scored). Name it by id, say what your rubric decided and what the gold label said, and
explain why your rubric read it that way. -->

I chose `pkg-14` (httpie/cli#1834, category `not-tested`). My rubric decided `reject` and the gold label says `reject`, so we agree (`pkg-14  not-tested  reject  reject  yes`).

My rubric rejected it because the PR never re-runs the plan's repro. The plan's test plan says: "re-run both repro commands, expect `Content-Type: application/json` on the request in BOTH". The PR's test evidence instead runs `http GET pie.dev/headers 'X-Custom-Header: value'`, which is a different scenario. My "Test evidence is real" check graded it fail with this evidence line: "Evidence runs `http GET pie.dev/headers 'X-Custom-Header: value'`, a different scenario from the repro's POST with x=1; no before result and no Content-Type result." "Repo checks run" also failed, because the only test claim is a bare "All tests pass" with no command named, and "Tests added" failed because the sole new test checks header casing, which the plan says is out of scope.

The gold label's note says: "the evidence exercises a GET with header casing (the unchanged path); the issue's single-header POST Content-Type repro is never re-run, and the added test also covers casing, not the fix". That is the same reason my rubric gave: the proof is about a different behavior than the fix.

**Check rationale**

<!-- Quote one check from the `rubric.md` you uploaded to `tools/pr-precheck/`, exactly as it reads now.
Then say why it reads that way — what you revised to get there, or what you rejected in
favour of it. -->

> `| Test evidence is real | The test-evidence section, read against the plan's test plan and the repro's own steps | The scenario the plan's test plan or the issue's repro names (the same input and command, not a different or easier case) is re-run on the branch, and the evidence states a concrete before result and a concrete after result (printed output, or a note naming what was actually observed, such as "before: error X, after: searches normally"), and the after result is the behavior the plan expects. Fail if the repro was not re-run, a different scenario stands in for it, only "works on my machine" or "tests pass" is offered, or no before and after result is stated. A note that only says the output "matches expected" without stating the result is not a result. A control the plan names that goes unmentioned does not by itself fail this check | required |`

It reads this way because of two failed versions. My first version required a command and its printed output for every claimed result. My first full run then scored `agreement: 13/19 scored items`, with every clear-accept rejected: for example `pkg-02` reports `pytest tests/ -q` passes (1462 passed, 12 skipped), which that version treated as no proof. My second version still asked for real printed output on the repro, and re-grading the clear-accepts gave `1/7`, because accepts like `pkg-11` state the before and after as `#` notes. Reading the not-tested packages (`pkg-04`, `pkg-07`, `pkg-10`, `pkg-14`) showed where the gold labels really draw the line: they reject PRs that do not re-run the plan's repro at all, not PRs with compact evidence. So the check now asks whether the same scenario is re-run with a stated before and after result, and I rejected demanding pasted output blocks.

**Trade-offs**

<!-- Every check gives something up. Any one of these is a complete answer: a package whose
result it changes, a canary you re-ran with `--only`, a case you accept it will miss, or a
stated reason nothing changed elsewhere. "Nothing changed, and here is how I know" earns
the point in full when the reason follows. -->

The loosened check gives up one case: a PR that re-runs the right repro but writes a plausible result in a `#` note with no real output now passes "Test evidence is real", because the check accepts "a note naming what was actually observed". I accept that miss, since the package text alone cannot show whether a note is true or invented. It also changed one package-type result: a "passes" line with a named command and no counts, like `pkg-16`'s "go test ./pkg/minikube/machine/... ./cmd/... passes", now passes "Repo checks run" where my second version failed it. Because the change loosened a check, I re-ran two not-tested canaries with `--only`, `pkg-04` and `pkg-14`, and both still graded reject (`agreement: 8/8 scored items` together with the six clear-accepts). The final full run then showed `pkg-07` and `pkg-10` also still reject, 20/20.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
