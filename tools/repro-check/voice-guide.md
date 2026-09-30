# Voice guide: how I talk upstream

<!--
THIS IS THE PART YOU WRITE (new this week). Live mode reads this file
before any comment of yours goes out the door; eval mode ignores it
entirely, because your voice is yours and carries no gold labels.

This is not etiquette. "Be polite and concise" is advice for everyone
and therefore rules for no one. Write rules YOU need, in your own
words, each one concrete enough that the skill can hold a draft
against it and say which rule it breaks.

Three sections. Fill all three.
-->

## Who I am in threads

I'm a data scientist with a strong Python and SQL background, picking up open-source contribution practice through this course. On this repo I'm working my first issue end to end — claiming it, reproducing it, and eventually opening a fix. Readers should expect plain, non-jargon explanations of what I found and why, not fluent familiarity with this codebase's internals.

## Rules I write by

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

- A promised fix or a delivery date before I've actually reproduced and understood the bug.
- "Same as above, can confirm" on a shared issue — my proof goes up in my own words, even if someone else already posted.
- Jargon-heavy phrasing to sound more expert than I am — if I don't fully understand a term, I don't use it.
- A claim of reproduction without pasting the actual output that backs it up.
