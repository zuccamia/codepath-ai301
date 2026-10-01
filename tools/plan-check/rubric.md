# Rubric: is this plan ready to post and build from?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here (via your procedure.md). It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the plan's scope statement, the test plan read against
     the repro evidence's steps, the plan comment read against the
     thread highlights, the repo-facts block) or a location from your
     references/evidence-guide.md. "The plan" is not a source; "the
     plan's stated cause read against what the repro evidence shows"
     is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (is
     this one bounded change? could a stranger start executing it?),
     never the write-up's shape (how many sections it has, how long it
     is, whether it uses headings). Structure-shaped checks are what
     make graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad plans posted. The lecture named the
failure families: the diagnosis ignores or contradicts the reproduced
evidence, the change is unbounded (scope creep), the plan targets the
symptom while the evidence points at the cause, a stranger could not
start executing it, the test plan proves nothing observable, the
unknowns are dressed up as certainty, and the comment ignores what the
thread or the repo's stated conventions ask. A rubric that ignores a
family will fail eval packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis-grounded | the plan's stated cause read against the repro evidence | both hold: (a) the cause names a specific mechanism (function, callback, code path, config key) the evidence points at; (b) the cause does not restate the symptom or contradict the evidence | required |
| scope-bounded | the plan's scope statement read against the diagnosis | both hold: (a) the plan names the file or module it will touch; (b) it calls out at least one thing it will not change | required |
| test-plan-observable | the test plan read against the repro evidence's steps | both hold: (a) the test re-runs the repro steps (or a check derived from them against the real code); (b) it names a specific observable that must change after the fix (not "verify it works") | required |
| comment-respects-context | the plan comment read against the thread highlights and the repo-facts block | all three hold: (a) the comment does not ignore or contradict what a maintainer has said in the thread; (b) it complies with the repo's stated conventions; (c) it discloses AI assistance if the repo requires it | required |

## Verdict rule

Accept if every required check passes. Reject if any required check
fails. `unclear` counts as fail. Preferred checks (none in this rubric)
would never change the verdict.
