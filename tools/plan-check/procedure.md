# Procedure: how this skill grades a plan package

<!--
THIS IS THE PART YOU WRITE, and it is a new kind of part. Weeks 1 and
2, SKILL.md carried a numbered workflow and you only wrote judgment
files. This week the workflow is gone from the frame: SKILL.md says
"execute procedure.md", and these are the operating steps you author.
The machinery is in your hands now.

Your operator swap is the design brief. When your executor stalled
because your rubric said WHAT to decide but not HOW to find the
evidence, that was a procedure gap. This file is where those gaps get
closed: a complete procedure lets someone who has never seen a plan
package before (a groupmate, or the skill itself) grade one exactly the
way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
repro evidence before the plan, and note what behavior it pins down"
is a step; "understand the context" is a wish.
-->

## Read order

<!-- What gets read, in what order, before any check is graded, and
what to note down from each part while reading. A complete procedure
decides the order (issue first? repro evidence first?) and says why
the order matters for the checks that come later. -->

1. Read the candidate plan first, start to finish. Note three things as you read: the cause the plan says the bug has, the files it says it will change (and anything it says it won't touch), and how it says it will test the fix.
2. Then read the repro evidence (eval mode: the repro-evidence block in the bundle; live mode: the student's posted repro comment, or the house repro pack as quoted in the drafts). Note the exact error or output it shows and the steps that produced it.
3. Only after both are read, grade. The plan is read first so you know what it claims; the repro evidence is read second so you can hold each claim against what was actually observed.

## Evidence gathering

<!-- For each evidence family your rubric's checks name, the concrete
gathering move: which part of the package (or, live, which page or
thread location per your evidence guide) to pull the fact from, and
what to record. A complete procedure leaves no check whose evidence an
executor would have to hunt for. -->

1. Diagnosis: copy the plan's one-line statement of the cause. Copy from the repro evidence the error message or output that the cause must explain. Record both side by side.
2. Scope: copy the list of files the plan says it will change, and copy the plan's statement of what it will not touch. If either is missing, record "absent".
3. Test: copy the plan's test plan (the command or check it names and the output it expects). Copy from the repro evidence the steps and the output that showed the bug, so the test can be compared with them. If the plan has no test plan, record "absent".
4. Record where each item was found (which section of the plan or bundle). Use only the package text; in eval mode do not fetch anything, and in live mode do not use files outside the drafts.

## Check execution

<!-- How one check runs against gathered evidence: in what order the
checks execute, what an executor does when evidence for a check is
genuinely absent, and when a check may be graded without re-reading
the whole package. A complete procedure makes two executors grade the
same package the same way. -->

1. Run the checks in the rubric's order: Diagnosis, Scope, Test. Grade each one using only the evidence recorded in the previous stage; do not re-read the whole package for each check.
2. Grade each check P (pass), F (fail), or ? (unclear), by the rubric's pass condition and nothing else.
3. Grade P only if the pass condition is clearly met by the recorded evidence. Grade F if the evidence is present and the condition is clearly not met (for example, the plan's cause contradicts the repro evidence).
4. If the evidence for a check is missing or absent (for example, the plan has no test plan, or no repro evidence was given to compare against), grade it ? and write "evidence absent" as the reason. Do not guess.
5. For every check, write one line: its name, its grade, and the fact or quote that decided it. A grade without a named fact is not allowed.

## Verdict assembly

<!-- How the per-check grades become the final accept or reject:
apply your rubric's verdict rule, state how unclear grades enter it,
and say what gets quoted in the output for the deciding check. A
complete procedure produces the same verdict from the same grades,
every time. -->

1. Apply the verdict rule in rubric.md: accept if every required check is P; reject otherwise.
2. Count every ? on a required check as F, as the rubric's verdict rule says. Preferred checks never change the verdict.
3. If the verdict is reject, quote in the output the fact that decided the first failing or unclear required check.
4. Emit the verdict as the final JSON block described in SKILL.md, with one entry per check (grades written as pass, fail, or unclear).
