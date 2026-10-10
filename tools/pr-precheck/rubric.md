# Rubric: is this pull request ready to submit?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| plan-fidelity | the diff's changed files and hunks read against the plan's In/Out scope and its `## Deviations` note; the description's claims read against the diff | every changed file and behavior falls inside the plan or a deviation note, nothing the plan promised is missing without a note, and the description claims nothing the diff does not deliver (more or less) | required |
| tested | the test evidence read against the plan's test plan and the repro steps | all hold: (a) the repro steps are re-run with an observable before and after; (b) the repo's own checks or test suite are reported with a stated outcome (a one-line result such as "suite passes" counts; pasted output is not required; a failure reported honestly with its reason counts; checks never mentioned, or a result the evidence contradicts, fail); (c) the evidence names a specific observable, not just "tests pass" | required |
| diff-reviewable | the unified diff and commit list | no debug leftovers, commented-out code, unrelated hunks, or formatting churn bury the change, and every hunk serves the plan | required |
| standards-met | the repo-facts block (or live: PR template, docs/CONTRIBUTING.md) read against the description and the thread | all hold: (a) every template section has real content; (b) the issue is referenced; (c) AI use is disclosed if the repo asks; (d) the PR does not ignore an explicit maintainer or policy ask | required |

## Verdict rule

Accept if every required check passes. Reject if any required check
fails. `unclear` counts as fail. A shortfall the PR description and
plan disclose honestly (something left out on purpose, a change the
plan did not name but a deviation note records) does not fail
plan-fidelity or tested; the same shortfall hidden does. Preferred
checks (none here) would never change the verdict.
