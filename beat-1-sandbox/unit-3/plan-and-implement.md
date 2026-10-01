# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

zuccamia

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/51#issuecomment-5922402847

Plan for this one, following the [repro comment above](https://github.com/codepath/pathreview-ai301-fa26-s1/issues/51#issuecomment-5863628115). Splitting into two PRs.

**Cause.** Two related gaps in CI today:

(a) **Migrations are never applied to a fresh DB in CI.** A PR whose migration cannot apply cleanly on an empty schema passes every existing job and only fails at deploy time. My repro's broken migration (`003_broken_demo.py`, dropping `reviews.nonexistent_column`) errored with `sqlalchemy.exc.ProgrammingError: column "nonexistent_column" of relation "reviews" does not exist` on `alembic upgrade head`, and no CI job caught it:

> `$ grep -E "alembic|migration" .github/workflows/ci.yml || echo "no migration check in ci.yml"`
> `no migration check in ci.yml`

(b) **The migrated schema is never compared against the SQLAlchemy models.** A model change that lands without a matching migration, or a migration that adds a constraint the models don't declare, drifts silently. `main` already has one such drift today: `alembic check` on a freshly-migrated database flags a named `UniqueConstraint("email", name="uq_users_email")` on `users` from `001_initial_schema.py` that `core/models/user.py` does not declare.

**This PR (part 1 of 2): closes gap (a).** One new CI job `validate-migrations` in `.github/workflows/ci.yml`, plus `scripts/validate_migrations.sh` (the file the issue names). The script spins up on the postgres service block and runs `alembic upgrade head` from empty. Any migration error exits non-zero and fails the job.

**Follow-up PR (part 2 of 2): closes gap (b).** Landing (b) in this PR would make the new job fail on every PR from day one because of the existing `uq_users_email` drift, so it needs its own scope. Follow-up issue filed as #80 with the maintainer question that decides the fix shape; keeping that question out of this thread to stay focused.

**Not in scope for this PR:** No changes to existing migrations, models, or other CI jobs. No fix or allow-list for the drift. The `test-integration` postgres service block gets mirrored, not refactored.

**Small side note:** Migration validation usually rides on a CD pre-deploy step (the same `alembic upgrade head` that runs against staging before release). This repo has CI only, so the standalone script fills that gap.

**Test:** Re-add the repro's `003_broken_demo.py` after the change: expect the new job to fail with the same `ProgrammingError`. Remove it, re-run: expect exit 0. Confirm the pre-existing `uq_users_email` drift does NOT cause a failure (this PR does not run `alembic check`).

Will open the PR from `feat/51-validate-migrations` if this sounds reasonable.

---

## Your branch

**Branch**

feat/51-validate-migrations

**Evidence**

**Before (unit 2 repro, main at f89c06f):** no CI job runs migrations, so the broken migration would pass CI unobserved.

```
$ grep -E "alembic|migration" .github/workflows/ci.yml || echo "no migration check in ci.yml"
no migration check in ci.yml

$ ls scripts/validate_migrations.sh
ls: scripts/validate_migrations.sh: No such file or directory
```

The broken migration itself, run locally, errors as expected:

```
$ alembic upgrade head
INFO  [alembic.runtime.migration] Running upgrade 002 -> 003_broken_demo, demo broken migration referencing a nonexistent column
...
sqlalchemy.exc.ProgrammingError: (sqlalchemy.dialects.postgresql.asyncpg.ProgrammingError)
column "nonexistent_column" of relation "reviews" does not exist
[SQL: ALTER TABLE reviews DROP COLUMN nonexistent_column]
```

**After (branch `feat/51-validate-migrations`):** the new `scripts/validate_migrations.sh` catches the same broken migration and exits non-zero. Baseline (no broken migration) exits 0.

```
$ DATABASE_URL=postgresql+asyncpg://pathreview:pathreview@localhost:5433/migval_test \
    bash scripts/validate_migrations.sh
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 001, Initial schema creation ...
INFO  [alembic.runtime.migration] Running upgrade 001 -> 002, Add error_message column to reviews table.
$ echo $?
0
```

With `alembic/versions/003_broken_demo.py` re-added (same file as the unit 2 repro), against a fresh DB:

```
$ DATABASE_URL=postgresql+asyncpg://pathreview:pathreview@localhost:5433/migval_test \
    bash scripts/validate_migrations.sh
INFO  [alembic.runtime.migration] Running upgrade 002 -> 003, ...
...
sqlalchemy.exc.ProgrammingError: (sqlalchemy.dialects.postgresql.asyncpg.ProgrammingError)
column "nonexistent_column" of relation "reviews" does not exist
[SQL: ALTER TABLE reviews DROP COLUMN nonexistent_column]
$ echo $?
1
```

Removing `003_broken_demo.py` and re-running against a fresh DB returns to exit 0. The pre-existing `uq_users_email` drift does not fail the script, as scoped — this PR runs only `alembic upgrade head`, not `alembic check`.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Run 1 (full): 20/20, all categories matched (clear-accept 7/7, scope-creep 4/4, thread-convention 2/2, unbuildable 3/3, wrong-cause 4/4). Matches `agreement: 20/20 scored items (bar: 18/20: PASS)` in `eval-run.txt`.

**Package analysis**

`pkg-15` (laurent22/joplin#16215; scope-creep). My rubric: **reject**. Gold: **reject**. Agreed.

The diagnosis isolates one mechanism (250 ms `autoSelectFamilyAttemptTimeout` aborting slow TCP connects; one-line fix: bump to 500 ms). "Proposed changes" then piles on four unrelated items — replace `node-fetch` with `undici` across desktop sync, add a Settings timeout field, fix the silent-success UI (#12810), wrap sync fetches in retry-with-backoff — framed as "while touching the network stack". No out-of-scope line. `scope-bounded` requires both naming the file *and* an explicit exclusion; the plan does the former but not the latter, and the touched areas sprawl well past what the diagnosis supports.

**Check rationale**

Quoting the `scope-bounded` row of `rubric.md` verbatim:

> | scope-bounded | the plan's scope statement read against the diagnosis | both hold: (a) the plan names the file or module it will touch; (b) it calls out at least one thing it will not change | required |

Both halves matter. (a) alone is too easy — any plan with a Files section passes, missing pkg-15-style sprawl where the plan names *all* the files. (b) forces an explicit boundary; without it, scope can grow to anything under review. Together they catch scope-creep (pkg-06, 12, 15, 19) without hitting clear-accept packages that touch one area and say so.

**Trade-offs**

Making (b) required rejects a genuine one-liner with nothing to exclude (typo fix, boolean default flip). Didn't bite this run — every clear-accept package (pkg-02, 03, 05, 08, 09, 13, 14) carries an explicit "not in scope" line. The alternative — (b) as preferred — would have passed scope-creep packages whose only tell is unlisted sprawl, which is the family this check exists for. Strict is the right trade.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
