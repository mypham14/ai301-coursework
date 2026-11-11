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

During Unit 6, I added upstream claim, question, and ping rules to my voice guide, installed the voice guard, and verified the hook; the wiring test was blocked as expected because the comment command had no body. I created a concise `CLAUDE.local.md` for my CRM clone and excluded it through `.git/info/exclude` so it stays out of the PR. My first claim check failed because repro-check still had its sandbox scope, so I installed the wild scope and reran it successfully. I initially missed the Cart's shared DataTables CSV button, then corrected my source report: the Cart exports its fixed table columns, while `src/CSVExport.php` has a separate selectable-field flow. The maintainer confirmed that the selector should be brought to the Cart export surface.

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

The Cart's CSV export only includes the Cart table's fixed columns. A user cannot choose which fields to include or add fields such as birthday and phone when exporting the people currently in the Cart.

**Expected Behavior**

The Cart export should let the user select fields and download a CSV containing those fields for people currently in the Cart. The existing five columns should remain selected by default, and the Actions column must not be exported.

**Current Behavior**

The Cart view initializes its table with `window.CRM.plugin.dataTable`. The shared configuration in `webpack/crm-bootstrap.js` supplies a CSV button and exports columns except those marked `.no-export`. The Cart therefore exports its fixed Name, Address, Email, Classification, and Family Role columns; its Actions column is excluded. The separate field selector in `src/CSVExport.php` is admin-only and is not the Cart export flow.

**Affected Components**

- `src/people/views/cart/cartview.php`: renders the Cart's fixed columns and initializes the shared DataTable.
- `webpack/crm-bootstrap.js`: defines the shared CSV button and `.no-export` exclusion; changing this globally could affect unrelated tables.
- `src/CSVExport.php` and `src/CSVCreateFile.php`: existing selectable-field export pattern and generation flow, but the page is admin-only.
- Cart-specific export handling and focused Cypress coverage: to be determined and added without changing the general exporter or unrelated Cart actions.

**Toolkit**

- Tool: Copilot in VS Code, used to trace the Cart view and shared DataTables configuration against the existing CSV exporter.
- What it produced: a source-grounded description of the fixed-column export, the `.no-export` Actions column, and the admin-only selector pattern.
- Difficulties: my initial source read missed the shared CSV button; later, the existing Cart Cypress spec failed all three add/remove tests on the unchanged `master` baseline.
- What changed: I corrected my source report and kept the planned change Cart-specific. I will isolate new export coverage from the known failing add/remove tests.

---

## Reproduction Process

**Environment Setup**

I inspected `mypham14/CRM` on Windows 10 Pro, version 10.0.19045, build 19045, at commit `493cb45053bd94719dbb9c152afff344b2785bfc` on `master` (2026-11-10). The source inspection did not require running the app. Before attempting runtime setup, `npm run env:check -- php` reported 2/11 checks passed (Node and Git); PHP 8.4+, Composer, dependencies, PHP extensions, and the PHP test runner were missing. Docker could not connect because the Docker Desktop Linux engine was not running, so I did not install dependencies, start services, or run the application.

**Steps to Reproduce**

These steps inspect the source for the route reached by `GET /people/cart`; they do not launch the application.

1. Clone the fork and check out the inspected commit:

   ```powershell
   git clone https://github.com/mypham14/CRM.git CRM
   Set-Location CRM
   git checkout 493cb45053bd94719dbb9c152afff344b2785bfc
   ```

2. Confirm the checkout with `git rev-parse HEAD`; expected output is `493cb45053bd94719dbb9c152afff344b2785bfc`.
3. Inspect the Cart table's export configuration and initialization:

   ```powershell
   rg -n -i "csv|export|Directory|Actions|No export|DataTable" src/people/views/cart/cartview.php webpack/crm-bootstrap.js
   ```

4. Compare the configured export columns with the Cart table headers in `src/people/views/cart/cartview.php`. This source inspection does not verify a downloaded CSV in a running browser.

**Reproduction Evidence**

- **Commit showing reproduction:** none (source inspection only; no code changes).
- **Screenshots/logs:** No runtime screenshot, downloaded CSV, or application log. The PHP readiness check reported `php: 2/11 checks passed. No application tests were run.`
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

- **My findings:** At this pinned commit, the Cart table uses the shared DataTable configuration, which adds a CSV button and exports columns except those marked `.no-export`. The Cart columns are Name, Address, Email, Classification, Family Role, and Actions; Actions is excluded, so the source supports the issue's report of a fixed-column Cart CSV export. The separate admin CSV flow in `src/CSVExport.php` also offers selectable fields and a “People in Cart” source, handled by `src/CSVCreateFile.php`. I did not launch the application or download a CSV, so I have not verified the runtime output. Before coding, I need to ask whether #8900 intends selectable additional fields such as Birthday and Phone in the Cart table's CSV button, or a different export flow.

**Toolkit**

- Tool: repro-check graded my claim and source-inspection report; I inspected the pinned Cart view and shared DataTable configuration.
- What it produced: an `accept` verdict on all required checks and evidence that the Cart table's CSV button exports its fixed visible columns, excluding Actions.
- Difficulties: the local PHP and Docker prerequisites were unavailable, so I could not verify a downloaded CSV at runtime. My first source read missed the shared CSV button and led to an incorrect conclusion.
- What changed: I corrected the report after tracing `window.CRM.plugin.dataTable` to `webpack/crm-bootstrap.js`; I will clarify the intended selectable-field behavior with the maintainer before coding.

---

## Solution Approach

**Analysis**

I exported the Cart with Church Admin as its only person. Its CSV header was `"Name","Address","Email","Classification","Family Role"` and its row was `"Church Admin","","","Unassigned","Unassigned"`; there is no way to choose Birth Date or Mobile Phone. In `src/people/views/cart/cartview.php`, the Cart renders five fixed data columns and initializes its table with `window.CRM.plugin.dataTable`. The shared `dataTableConfig` in `webpack/crm-bootstrap.js` supplies a CSV button that exports rendered columns except those marked `.no-export`, such as the Actions column. It cannot provide selectable fields not rendered in the table. `src/CSVExport.php` demonstrates a selector but is admin-only, so routing Cart users there would not preserve the Cart surface and access behavior.

**Proposed Solution**

Add field selection to the Cart's own CSV export flow, following the existing selector conventions and generating a CSV only for people currently in the Cart. Keep the existing five fields selected by default, allow selecting additional fields such as Birth Date and Mobile Phone, exclude the Actions column, and preserve the Cart's existing access rules. Do not change the shared DataTables export behavior for other pages, make the general admin exporter available to non-admin users, alter unrelated Cart actions, or modify the database schema.

**Implementation Plan**

1. Add the field-selection UI to `src/people/views/cart/cartview.php`.
2. Add Cart-specific CSV generation for selected fields and current Cart members in `src/people/routes/cart.php`; leave `src/CSVExport.php`, `src/CSVCreateFile.php`, and `webpack/crm-bootstrap.js` unchanged.
3. Add `cypress/e2e/ui/people/cart.csv-export.spec.js`. Assert the default header remains `"Name","Address","Email","Classification","Family Role"`. Then select First Name, Birth Date, and Mobile Phone and assert the header is exactly `"First Name","Birth Date","Mobile Phone"`, the selected row values are correct, no Actions column appears, and no people outside the Cart are exported.
4. Run the focused spec from the CRM repo root using the running Docker dev webserver:

   ```powershell
   docker compose -f docker/docker-compose.dev.yaml exec -T -e ROW_GUARD_DB_HOST=database -e ROW_GUARD_DB_PORT=3306 webserver bash -lc "source /root/.nvm/nvm.sh && cd /home/ChurchCRM && ./node_modules/.bin/cypress run --config-file cypress/configs/docker.config.ts --spec cypress/e2e/ui/people/cart.csv-export.spec.js"
   ```

   Expected result: the new Cart CSV spec passes; its selected-field assertion fails before the fix because the baseline export always has the five fixed headers above. Run the manifest's lint and build commands and record before/after CSV output and check results in `test_evidence.md`. Re-run the existing Cart spec and report its known add/remove failures separately.

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool: plan-check reviewed `plan.md` and `plan-comment.md` against issue #8900; Copilot in VS Code traced the Cart export code and edited the drafts.
- What it produced: plan-check accepted the plan. It identified the current CSV output, the three files in scope, an exact focused Cypress command, and default/selected-field header assertions.
- Difficulties: the first plan-check run rejected the drafts because they lacked observed output, specific files, and measurable test expectations; the Cart export button is hidden when the Cart is empty.
- What changed: I captured the Cart CSV with one local test person, added its exact header and row to the plan, named the implementation and test files, and specified the expected CSV headers. I will wait for any maintainer direction before coding.

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
