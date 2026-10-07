# Voice guide: how I talk upstream

<!--
THIS IS A CARRY-OVER SLOT, not a new hole. You wrote this guide in
week 2; paste your filled week-2 voice-guide.md here, whole. It is not
re-authored and it is not graded as new work this week.

Then reread it with the plan comment in mind. Your claim and repro
comments promised and reported; a plan comment commits you to an
approach in front of the people who maintain the code. If your rules
do not cover that register (for example: how you state an approach you
are not certain of, or how you respond when a maintainer already
suggested a direction), extend the guide with what it needs. Extending
is allowed and encouraged; starting over is not required.

Live mode reads this file before your plan comment goes out and
reports any rule your draft breaks. Eval mode ignores it entirely,
because your voice is yours and carries no gold labels.
-->

## Who I am in threads

<!-- Paste your week-2 section here. -->
I'm a data scientist with a strong Python and SQL background, picking up open-source contribution practice through this course. On this repo I'm working my first issue end to end — claiming it, reproducing it, and eventually opening a fix. Readers should expect plain, non-jargon explanations of what I found and why, not fluent familiarity with this codebase's internals.

## Rules I write by

<!-- Paste your week-2 rules here, wrong/right pairs and all. Add any
rule the plan-comment register needs that your week-2 comments did
not. -->

### Rule: Plain language over jargon

State what I found in everyday language instead of leaning on technical terms; if a term is unavoidable, say briefly what it means in context.

- Wrong: "The health probe passes raw SQL under a legacy execution path the ORM no longer implicitly coerces."
- Right: "The health check runs a raw SQL string directly, but the newer version of the library (SQLAlchemy 2.x) needs raw SQL wrapped in a `text()` function first — without that, the query fails."

### Rule: Name the specific issue, not the general topic

When I claim or report on an issue, I reference the exact number and behavior, not a vague restatement of the topic.

- Wrong: "I'll take a look at the database issue."
- Right: "I'm picking up #61 — the `/health` endpoint's DB probe fails because it passes a raw SQL string directly to SQLAlchemy 2.x."

### Rule: Promise investigation, not outcomes

I only commit to what I'm about to do next; I never promise a fix is coming or give a timeline.

- Wrong: "I'll have a fix up by tomorrow."
- Right: "I'm going to reproduce this locally and report back what I find."

### Rule: Show, don't just assert

Every claim I make is backed by the actual output or error I saw, not a summary sentence standing in for it.

- Wrong: "I confirmed the bug happens."
- Right: "Running the health check locally throws `ObjectNotExecutableError`, which matches the traceback in the issue."

### Rule: Say plainly when something didn't reproduce

If I can't reproduce the bug, I report exactly what I tried and what happened instead, in plain terms — I don't round it up to "looks fine."

- Wrong: "Looks fine on my end."
- Right: "I ran the same steps against SQLAlchemy 2.0.31 on Windows and didn't see the error — here's exactly what I ran and what it returned instead."

## Things I never post

<!-- Paste your week-2 list here; extend it if planning tempts you
toward new ones (overpromised timelines are the classic). -->

- A promised fix or a delivery date before I've actually reproduced and understood the bug.
- "Same as above, can confirm" on a shared issue — my proof goes up in my own words, even if someone else already posted.
- Jargon-heavy phrasing to sound more expert than I am — if I don't fully understand a term, I don't use it.
- A claim of reproduction without pasting the actual output that backs it up.
