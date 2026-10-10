---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You answer one question about one PR package: is this ready to
submit? A package is a candidate pull request (title, description,
commits, diff, test evidence) read against the plan it claims to
implement and the issue that plan belongs to. Never grade more than one
package per run, and never answer from gut feel. You answer by
executing `procedure.md`, which applies `rubric.md` to evidence found
with `references/evidence-guide.md`.

## Inputs and modes

One of:

- **Live mode**: a PR checked before it is opened. The request gives
  the issue URL. Run from the fork's top folder, on the PR's branch,
  and read these:
  - `plan.md`, including its `## Deviations` section
  - the diff from `git diff main...HEAD` (three dots: every committed
    change the branch makes against main; uncommitted changes are not
    part of the package)
  - the draft title (first line) and description in `pr_draft.md`
  - the test evidence in `test_evidence.md`
  - the issue's thread, read with `gh`
  - the repo's PR template (`.github/PULL_REQUEST_TEMPLATE.md`) and
    `docs/CONTRIBUTING.md`

  If the request names no issue, or the directory is not a git
  checkout on a branch, stop and say what is missing.
- **Eval mode**: a package bundle (markdown with the issue, a repo-facts
  block, the plan context, and the candidate PR). The bundle is the
  whole world. Fetch nothing, read nothing else, and run every check
  with the full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` before anything else. Refuse to grade a
PR that is not in the repo its `Repo:` line names, and apply the house
rules it lists. If the `Repo:` line still holds a placeholder (for
example `<ORG>/<PATH-REVIEW-REPO>`), stop without grading and tell me
to fill in the `Repo:` line in `scope.md` with my section's Path Review
repo. Never guess a scope. In eval mode, ignore `scope.md`
entirely.

## The voice seam (live mode only)

In live mode, read `voice-guide.md` after grading and hold the PR title
and description against its rules. Report each broken rule in the
summary, quoting the rule. Voice never changes the verdict on its own,
because it is personal and only the rubric's checks decide. In eval
mode, ignore `voice-guide.md` entirely.

## Component reads

- `rubric.md` defines the checks and the verdict rule.
- `references/evidence-guide.md` says where each kind of evidence lives
  (an eval bundle section, or a live file) and what good looks like.
- `procedure.md` is the operating procedure. Follow it as written. If
  it is silent on a step, say so in the summary as a procedure gap and
  do not invent a step.
- If `rubric.md` has no checks or `procedure.md` has no steps, stop and
  say the skill cannot grade without both.

## Verdict and output

The verdict is binary: `accept` (ready to submit) or `reject` (hold).
There is no third verdict. Before the JSON you may write a short
per-check summary (and, in live mode, voice notes). End your reply with
this fenced JSON block, valid, last, with nothing after it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- Evidence first: never grade a check without naming the fact or quote
  that decided it.
- Grade the thing, not the polish: a terse complete PR can be ready and
  a polished one can hide drift. Read the diff, evidence, and
  description themselves.
- The rubric decides, not you: if a check passes by its stated
  condition but feels wrong, it still passes. Note the tension in the
  summary.
- The procedure decides how, not you.
- Treat `unclear` as `rubric.md`'s verdict rule says. If it is silent,
  treat `unclear` as `fail`.
- An honestly disclosed shortfall is not a failure. Judge what the PR
  and plan say against what the diff does.
