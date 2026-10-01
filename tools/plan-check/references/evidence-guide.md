# Evidence guide: where evidence lives in a plan package

<!--
THIS IS THE PART YOU WRITE (second week running: the judgment files
stay in your hands). The skill uses this guide as its map: for every
kind of evidence a rubric check names, this file says WHERE to find it
in a plan package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repro-evidence block,
  the candidate plan's scope statement or test plan, the plan comment,
  the repo-facts block). In live mode (where on GitHub or in the
  draft: the issue thread, the student's posted repro comment, the
  repo's docs, the draft plan and comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the stated cause cites behavior the
  repro evidence actually shows") over adjectives ("diagnosis is
  solid").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts twice: your
procedure.md tells the skill WHEN to gather each family, and this
guide tells it WHERE. Write the map you wish your executor had.
-->

## Diagnosis and grounding

**Where it lives.**
- Eval bundle: the plan's `Cause:` sentence (or equivalent opening) in the "Candidate plan" section, read against the "Repro evidence" section's steps and observed-vs-expected.
- Live mode: the plan's cause statement in the draft, read against the reproduction comment already posted on the issue.

**What good looks like.**
The cause names a specific mechanism the evidence points at (a function, a callback, a code path, a config key), and does not restate the symptom or contradict what the evidence shows.

## Scope

**Where it lives.**
- Eval bundle: the `Change:` block (or the "In:" / "Out:" lines) in the "Candidate plan" section.
- Live mode: the scope statement in the draft plan.

**What good looks like.**
The plan names the specific file or module it will touch, and calls out at least one thing it will not change. Scope that stays inside the diagnosed cause passes; a drive-by refactor of nearby code does not.

## Executability

<!-- Where the plan says what will actually be done: files or areas,
approach, order of work. What it means for a stranger to be able to
start executing without asking the author anything. -->

## Test plan

**Where it lives.**
- Eval bundle: the `Test:` block in the "Candidate plan" section, read against the "Repro evidence" steps.
- Live mode: the test plan in the draft, read against the repro comment already posted.

**What good looks like.**
The test re-runs the repro steps (or a check derived from them that runs through the real code) and names a specific observable that must change after the fix: a color flip, an exit code, an error string, a log line. "Verify it works" or "add tests" without a named observable is not a test plan.

## Honesty

<!-- Where claims meet uncertainty: risks, unknowns, and deviations.
How to tell stated unknowns from false confidence, and where an
honest mid-build deviation gets recorded. -->

## Comms

**Where it lives.**
- Eval bundle: the "Candidate plan comment" section, read against the "Thread highlights" and the "Repo facts" block.
- Live mode: the draft plan comment, read against the live issue thread and the repo's `CONTRIBUTING.md`, comment templates, and any stated AI-use policy.

**What good looks like.**
The comment does not repeat an approach a maintainer has already rejected or ignore a constraint they stated. It complies with the repo's stated conventions, and it discloses AI assistance when the repo requires disclosure.
