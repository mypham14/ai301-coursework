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
**Pull Request:** https://github.com/ChurchCRM/CRM/pull/11430

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

During Unit 6, I added upstream claim, question, and ping rules to my voice guide, installed the voice guard, and verified the hook; the wiring test was blocked as expected because the comment command had no body. I created a concise `CLAUDE.local.md` for my CRM clone and excluded it through `.git/info/exclude` so it stays out of the PR. My first claim check failed because repro-check still had its sandbox scope, so I installed the wild scope and reran it successfully. I initially missed the Cart's shared DataTables CSV button, then corrected my source report: the Cart exports its fixed table columns, while `src/CSVExport.php` has a separate selectable-field flow. The maintainer confirmed that the selector should be brought to the Cart export surface.

**Unit 7: Installing the ship-gate and writing my manifest for the Cart CSV branch**

I wrote a manifest with three must-pass checks (lint, build, and my Cart CSV Cypress spec) and listed the existing `standard.cart.spec.js` as excluded, dated 2026-11-10, because all three of its tests fail on unchanged `master`. On my branch the gate's first pr-precheck rejected my description for the leftover working-draft comment and for a Screenshots section that only listed filenames, so I reworded that section and the gate then passed. Twice my own spec failed in its `before each` hook because a root-owned daily log file made the dev server return 500 on every page; I fixed the owner and kept the log files writable while the gate ran. The pre-push lint could not run on my Windows machine because of spaces in the repo and Node paths, so I pushed with `--no-verify` after the lint had passed in Docker, and said so in the PR.

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

The Cart's CSV export only includes the Cart table's fixed columns. A user cannot choose which fields to include or add fields such as birthday and phone when exporting the people currently in the Cart.

**Expected Behavior**

The Cart export should let the user select fields and download a CSV containing those fields for people currently in the Cart. The existing five columns should remain selected by default, and the Actions column must not be exported.

**Current Behavior**

The Cart view initializes its table with `window.CRM.plugin.dataTable`. The shared configuration in `webpack/crm-bootstrap.js` supplies a CSV button and exports columns except those marked `.no-export`. The Cart therefore exports its fixed Name, Address, Email, Classification, and Family Role columns; its Actions column is excluded. The separate field selector in `src/CSVExport.php` is admin-only and is not the Cart export flow.

**Affected Components**

- `src/people/views/cart/cartview.php`: renders the Cart's field-selection modal and overrides only the Cart table's CSV button to open it.
- `src/people/routes/cart.php`: defines the allowed export fields, validates the requested fields, and generates a CSV for current Cart members.
- `cypress/e2e/ui/people/cart.csv-export.spec.js`: covers default and selected headers, selected row values, and Cart-only output.
- `webpack/crm-bootstrap.js`: defines the shared CSV button and `.no-export` exclusion; changing this globally could affect unrelated tables.
- `src/CSVExport.php` and `src/CSVCreateFile.php`: existing selectable-field export pattern and generation flow, but the page is admin-only.

**Toolkit**

- Tool: Copilot in VS Code, used to trace the Cart view and shared DataTables configuration against the existing CSV exporter.
- What it produced: a source-grounded description of the fixed-column export, the `.no-export` Actions column, and the admin-only selector pattern.
- Difficulties: my initial source read missed the shared CSV button; later, the existing Cart Cypress spec failed all three add/remove tests on the unchanged `master` baseline.
- What changed: I corrected my source report, kept the implementation Cart-specific, and added a separate export spec so it does not depend on the known failing add/remove tests.

---

## Reproduction Process

**Environment Setup**

I inspected `mypham14/CRM` on Windows 10 Pro, version 10.0.19045, build 19045, at commit `493cb45053bd94719dbb9c152afff344b2785bfc` on `master`. On 2026-11-11, I reproduced the current export in the local Docker development stack at `http://localhost`; the database container was healthy on port 3307 because the Windows MySQL service already used port 3306. The app was built in the container to install its dependencies, then I signed in with the local development admin account and used the seeded Church Admin person. The original environment readiness check had reported missing PHP/Composer dependencies before the Docker setup.

**Steps to Reproduce**

These steps reproduce the baseline Cart CSV output in the local development app.

1. Clone the fork and check out the inspected commit:

   ```powershell
   git clone https://github.com/mypham14/CRM.git CRM
   Set-Location CRM
   git checkout 493cb45053bd94719dbb9c152afff344b2785bfc
   ```

2. Confirm the checkout with `git rev-parse HEAD`; expected output is `493cb45053bd94719dbb9c152afff344b2785bfc`.
3. Start the project's local Docker development environment following `DEVELOPING.md`, then sign in at `http://localhost` with the local development account.
4. Open `/people/view/1`, add Church Admin to the Cart, and open `/people/cart`.
5. In the Cart table toolbar, click the grid-icon button with the tooltip “Export CSV” and open the downloaded `ChurchCRM Cart.csv`.
6. Inspect the Cart table's export configuration and initialization:

   ```powershell
   rg -n -i "csv|export|Directory|Actions|No export|DataTable" src/people/views/cart/cartview.php webpack/crm-bootstrap.js
   ```

7. Compare the downloaded header with the rendered Cart table headers in `src/people/views/cart/cartview.php`.

**Reproduction Evidence**

- **Commit showing reproduction:** `493cb45053bd94719dbb9c152afff344b2785bfc` (baseline `master`, before the feature branch).
- **Downloaded CSV:** `ChurchCRM Cart.csv`, with Church Admin as the only Cart member:

  ```csv
  "Name","Address","Email","Classification","Family Role"
  "Church Admin","","","Unassigned","Unassigned"
  ```

- **Screenshots/logs:** No screenshot was saved. The local browser showed the Cart table and its CSV toolbar; the downloaded file above is the runtime output.
- **Source output:**

  ```text
  src/people/views/cart/cartview.php:39:    <a href="<?= SystemURLs::getRootPath() ?>/DirectoryReports.php?cartdir=Cart+Directory" class="btn btn-outline-warning" title="<?= gettext('Generate phone directory') ?>">
  src/people/views/cart/cartview.php:40:      <i class="fa-solid fa-book me-2"></i><?= gettext('Directory') ?>
  src/people/views/cart/cartview.php:60:            <th class="no-export"><?= gettext('Actions') ?></th>
  src/people/views/cart/cartview.php:129:    $("#cart-listing-table").DataTable(window.CRM.plugin.dataTable);
  webpack/crm-bootstrap.js:23:function dataTableConfig(pageLength, languageUrl) {
  webpack/crm-bootstrap.js:36:    buttons: [
  webpack/crm-bootstrap.js:38:        extend: "csv",
  webpack/crm-bootstrap.js:42:        exportOptions: { columns: ":not(.no-export)" },
  ```

- **My findings:** At this pinned commit, the Cart table uses the shared DataTable configuration, which adds a CSV button and exports columns except those marked `.no-export`. The Cart columns are Name, Address, Email, Classification, Family Role, and Actions; Actions is excluded. The downloaded header confirms that the current export contains only the five fixed data columns. The separate admin CSV flow in `src/CSVExport.php` also offers selectable fields and a “People in Cart” source, handled by `src/CSVCreateFile.php`. The maintainer's direction is to bring field selection to the Cart export surface.

**Toolkit**

- Tool: repro-check graded my claim and source-inspection report; I inspected the pinned Cart view and shared DataTable configuration.
- What it produced: an `accept` verdict on all required checks and evidence that the Cart table's CSV button exports its fixed visible columns, excluding Actions.
- Difficulties: the Cart table and export button are hidden when the Cart is empty, so the seeded Church Admin person had to be added to the local Cart before capturing output. My first source read also missed the shared CSV button.
- What changed: I corrected the report after tracing `window.CRM.plugin.dataTable` to `webpack/crm-bootstrap.js` and captured the actual CSV output from the baseline app. I used that evidence in the plan-check package before implementation.

---

## Solution Approach

**Analysis**

I exported the Cart with Church Admin as its only person. Its CSV header was `"Name","Address","Email","Classification","Family Role"` and its row was `"Church Admin","","","Unassigned","Unassigned"`; there is no way to choose Birth Date or Mobile Phone. In `src/people/views/cart/cartview.php`, the Cart renders five fixed data columns and initializes its table with `window.CRM.plugin.dataTable`. The shared `dataTableConfig` in `webpack/crm-bootstrap.js` supplies a CSV button that exports rendered columns except those marked `.no-export`, such as the Actions column. It cannot provide selectable fields not rendered in the table. `src/CSVExport.php` demonstrates a selector but is admin-only, so routing Cart users there would not preserve the Cart surface and access behavior.

**Proposed Solution**

Add field selection to the Cart's own CSV export flow, following the existing selector conventions and generating a CSV only for people currently in the Cart. Keep the existing five fields selected by default, allow selecting additional fields such as Birth Date and Mobile Phone, exclude the Actions column, and preserve the Cart's existing access rules. Do not change the shared DataTables export behavior for other pages, make the general admin exporter available to non-admin users, alter unrelated Cart actions, or modify the database schema.

**Implementation Plan**

1. `src/people/views/cart/cartview.php` adds the field-selection modal, with the original five columns selected by default and First Name, Birth Date, and Mobile Phone available as optional fields. The Cart's DataTable CSV button opens this modal; the shared DataTables configuration is copied and overridden locally.
2. `src/people/routes/cart.php` adds `GET /people/cart/csv`. It validates selected fields against an allowlist, builds headers and rows with `CsvExporter`, and uses only the current Cart members.
3. `cypress/e2e/ui/people/cart.csv-export.spec.js` covers the default CSV header and a selected First Name/Birth Date/Mobile Phone export, including the row values and absence of an Actions column.
4. The general exporter, shared DataTables configuration, database schema, and unrelated Cart actions remain unchanged.

The focused test passed on this branch on 2026-11-11. From the CRM repo root, use the running Docker dev webserver:

   ```powershell
   docker compose -f docker/docker-compose.dev.yaml exec -T -e ROW_GUARD_DB_HOST=database -e ROW_GUARD_DB_PORT=3306 webserver bash -lc "source /root/.nvm/nvm.sh && cd /home/ChurchCRM && ./node_modules/.bin/cypress run --config-file cypress/configs/docker.config.ts --spec cypress/e2e/ui/people/cart.csv-export.spec.js"
   ```

   Expected result: the new Cart CSV spec passes. On the baseline, the selected-field header assertion should fail because only the five fixed headers are exported. Run the manifest's lint and build commands and record their results in `test_evidence.md`; report the existing Cart add/remove spec's known baseline failures separately.

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool: plan-check reviewed `plan.md` and `plan-comment.md` against issue #8900; Copilot in VS Code traced the Cart export code and edited the drafts.
- What it produced: plan-check accepted the plan. It identified the current CSV output, the three files in scope, an exact focused Cypress command, and default/selected-field header assertions.
- Difficulties: the first plan-check run rejected the drafts because they lacked observed output, specific files, and measurable test expectations; the Cart export button is hidden when the Cart is empty.
- What changed: I captured the Cart CSV with one local test person, added its exact header and row to the plan, named the implementation and test files, and specified the expected CSV headers before implementing the Cart-specific selector.

---

## Testing Strategy

Run the new focused Cypress spec from the CRM repo root using the command in Implementation. It asserts the unchanged default header `"Name","Address","Email","Classification","Family Role"` and then selects First Name, Birth Date, and Mobile Phone and asserts the exact header `"First Name","Birth Date","Mobile Phone"` and row `"Church","",""`. It also asserts that only one Cart member is exported and that Actions is absent. The baseline runtime CSV is recorded in Reproduction Evidence. On 2026-11-11 the focused spec passed on the branch (1 passing, 0 failing) after `npm run docker:test:reset:db`, `XDEBUG_MODE=off npm run lint` passed all 29 checks, and `npm run build` exited 0. In a browser pass on seeded demo data, the Export CSV modal fit and worked in light and dark at 1440, 820 and 390 px, with the five default fields checked, and closing it exported nothing.

Before shipping, run the focused Cart CSV UI test, lint/regression checks, and build using the exact commands in `ship-manifest.md`. Record actual outcomes in the CRM repo's `test_evidence.md`. The existing `standard.cart.spec.js` is tracked separately because its three add/remove tests failed on the unchanged `master` baseline; that failure is not evidence about the CSV feature.

**Toolkit**

- Tool: Cypress focused UI test, repository lint/regression checks, and build.
- What it produced: the focused spec passed (1 passing), lint passed all 29 checks, and the build exited 0; the existing `standard.cart.spec.js` still fails the same 3 tests as on `master`.
- Difficulties: after the dev container was rebuilt, Cypress needed `cypress install` and the Xvfb and GTK packages. The spec's first run timed out at 30 s because the dev server is slow with Xdebug on and Cypress shares its container.
- What changed: I reran the spec with longer request and command timeouts passed on the command line and recorded the results in `test_evidence.md`.

---

## Implementation Notes

**2026-11-11**

Added a Cart-specific field selector and CSV endpoint in `src/people/views/cart/cartview.php` and `src/people/routes/cart.php`. The original five columns remain selected by default; First Name, Birth Date, and Mobile Phone are optional. The endpoint accepts only allowlisted fields and exports current Cart members. I kept the shared export configuration and admin-only exporter unchanged.

**2026-11-11**

Added `cypress/e2e/ui/people/cart.csv-export.spec.js` to cover the default and selected headers and the one-person Cart row. The spec was run later that day; it, lint and the build all passed (see Testing Strategy and `test_evidence.md`).

**2026-11-11**

Checked the Export CSV modal in a browser on seeded demo data, in light and dark at 1440, 820 and 390 px. The modal fit each width, the five default fields were checked, and closing it triggered no download. No code changes were needed.

**Toolkit**

- Tool: Copilot in VS Code, used to implement the route/view change and add focused Cypress coverage.
- What it produced: a Cart-only export flow and a separate test file; no validation result yet.
- Difficulties: the existing Cart UI spec has unrelated add/remove failures on the baseline, so the new export checks need an isolated spec.
- What changed: I kept the new test independent and recorded the existing baseline failures separately instead of broadening the feature.

---

## Pull Request

**PR Link:** https://github.com/ChurchCRM/CRM/pull/11430

**PR Description:** Fixes #8900. Export CSV on the Cart page now opens a modal where users choose the fields. The five original fields (Name, Address, Email, Classification, Family Role) are checked by default; First Name, Birth Date and Mobile Phone are optional. The modal's Download CSV button calls a new `GET /people/cart/csv` route in `src/people/routes/cart.php`, which checks the chosen fields against an allowlist, returns a 400 for an empty or unknown field, and exports only the people in the Cart without the Actions column. The shared DataTables export, the admin-only exporter and the database schema are unchanged. Validation: the new `cart.csv-export.spec.js` passes, `npm run lint` passed all 29 checks and `npm run build` passed in Docker, and the existing `standard.cart.spec.js` fails the same 3 tests as on `master`. I checked the modal in light and dark at 1440, 820 and 390 px on seeded demo data. I pushed with `--no-verify` because the pre-push lint cannot run on my Windows host, and the description says so.

**Maintainer Feedback:**

- 2026-11-12: none yet.

**Status:** Awaiting review

**Toolkit**

- Tool: the ship-gate (my manifest's lint, build and Cart CSV spec, then my pr-precheck), plus Claude Code to open the PR so the voice guard checked the description.
- What it produced: a PASS on the branch, then PR #11430 opened against ChurchCRM/CRM.
- Difficulties: the dev server returned 500 during two gate runs because of a root-owned log file, and the pre-push lint could not run on Windows because of spaces in the repo and Node paths.
- What changed: I fixed the log file's owner and kept it writable during the gate, then pushed with `--no-verify` and noted that in the PR description.

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
