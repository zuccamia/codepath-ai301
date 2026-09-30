# Rubric: is this reproduction package ready to post?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| env-recorded | the environment record in the repro report | names the tool or library version AND the OS, and either matches the issue's stated target or calls out the difference explicitly | required |
| steps-complete | the steps section of the repro report | all four hold: (a) install and setup commands are included; (b) commands are exact (no "run the build"); (c) any input file or config is either shown in full or specified precisely enough (exact fields, values, and shape) that a reader could reconstruct it without inventing content; (d) the report is self-contained, so a stranger reading only the report (not the issue thread) could follow it from a clean state to the failing behavior | required |
| behavior-matches-issue | the artifact in the report (output excerpt, log, screenshot, diff) read against the behavior the issue describes | the artifact shows the specific behavior the issue names, not an adjacent failure in the same tool | required |
| outcome-honest | the report's stated conclusion read against its own artifacts | the outcome does not claim more than the artifacts show; an evidenced cannot-reproduce passes, a confident reproduction of the wrong behavior fails | required |
| respects-conventions | the claim comment and repro comment read against the repo-facts block (contributing docs, issue and comment templates, AI-assistance disclosure policy) | the comments comply with the repo's stated conventions, including any AI-assistance disclosure requirement when one is on record | required |

## Verdict rule

Accept if every required check passes. Reject if any required check fails.
`unclear` counts as fail: proof that cannot be verified from the package is
proof that is not ready to post. Preferred checks (none in this rubric) would
never change the verdict.
