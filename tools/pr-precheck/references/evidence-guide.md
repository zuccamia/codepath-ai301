# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

**Where it lives.**
- Eval bundle: the plan's Files, In/Out and any deviation text in "Plan context"; the changed files in the candidate PR's "Diff" and "Commits"; the claims in the "Description".
- Live mode: `plan.md` (scope, Files, `## Deviations`), `git diff main...HEAD`, and the description in `pr_draft.md`.

**What good looks like.** Every changed file and behavior is in the plan or a deviation note, nothing promised is missing without a note, and the description claims exactly what the diff does. Extra or missing work with no note, or a description that claims more or less than the diff, is silent drift.

## Test evidence (harness category: not-tested)

**Where it lives.**
- Eval bundle: the "Test evidence" section, read against the plan's test plan and repro steps.
- Live mode: `test_evidence.md` (before and after of the repro, output of `make test-unit`, `make test-integration`, `make lint`, `make typecheck`), read against `plan.md`'s test plan and pasted again in the description's Testing section.

**What good looks like.** The repro steps are re-run with a named observable that changes (an error line, exit code, output), and the repo's own checks show their output. A check that failed, reported with the reason, still counts. "Tests pass" with no output, or checks never run, does not.

## Diff quality (harness category: unreviewable)

**Where it lives.**
- Eval bundle: the unified diff and commit list.
- Live mode: `git diff main...HEAD` and `git diff main...HEAD --stat`, plus `git log main..HEAD`.

**What good looks like.** The fix is visible and nothing else rides along. Debris tells: debug prints, commented-out code, dead code, whitespace-only churn, drive-by edits to files the issue does not touch.

## Standards and comms (harness category: standards-wall)

**Where it lives.**
- Eval bundle: the "Repo facts" block (PR template asks, contribution and AI policy) and the "Thread highlights", read against the description.
- Live mode: `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`, the issue thread, and `pr_draft.md`.

**What good looks like.** Every template section has real content, the issue is referenced (`Closes #N`), the AI-use disclosure is present if the repo asks, and an explicit maintainer or policy ask is honored. Boilerplate or a skipped ask fails. Whether claims match the diff belongs to plan fidelity.
