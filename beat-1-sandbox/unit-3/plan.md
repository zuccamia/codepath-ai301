# Plan: add DB migration validation to CI (issue codepath/pathreview-ai301-fa26-s1#51, part 1 of 2)

## Diagnosis

Two related gaps in CI today:

**(a) Migrations are never applied to a fresh DB in CI.** A PR whose migration cannot apply cleanly on an empty schema passes every existing job and only fails at deploy time. My unit 2 repro's broken migration (`alembic/versions/003_broken_demo.py`, dropping a nonexistent column) errored on `alembic upgrade head`:

> `sqlalchemy.exc.ProgrammingError: column "nonexistent_column" of relation "reviews" does not exist`

and no CI job caught it:

> `$ grep -E "alembic|migration" .github/workflows/ci.yml || echo "no migration check in ci.yml"`
> `no migration check in ci.yml`
>
> `$ ls scripts/validate_migrations.sh`
> `ls: scripts/validate_migrations.sh: No such file or directory`

**(b) The migrated schema is never compared against the SQLAlchemy models.** A model change that lands without a matching migration, or a migration that adds a constraint the models don't declare, drifts silently. `main` already has one such drift today: running `alembic check` on a freshly-migrated database flags a named `UniqueConstraint("email", name="uq_users_email")` on `users` from `001_initial_schema.py` that `core/models/user.py` does not declare (the model has `email: ... unique=True`, which SQLAlchemy renders as a unique index only).

**This PR closes gap (a) only.** Landing (b) without either declaring the constraint on the model or dropping it from the DB would make the new job fail on every PR from day one. The decision depends on a maintainer answer that lives in follow-up issue codepath/pathreview-ai301-fa26-s1#80, so (b) is scoped out of this PR and will be done in the PR that resolves codepath/pathreview-ai301-fa26-s1#80.

**Why a standalone script.** In a repo with a CD pipeline, migration validation usually rides on the pre-deploy step (the same `alembic upgrade head` that runs against staging before the release image ships). This repo has CI only, so the standalone script the issue names fills that gap.

## Scope

**In.** Add a `validate-migrations` job to `.github/workflows/ci.yml` and create `scripts/validate_migrations.sh`. The script starts from an empty Postgres and runs `alembic upgrade head`. If any migration errors, the script exits non-zero and the job fails.

**Out.** No `alembic check` or schema-vs-model comparison (deferred to follow-up PR). No changes to existing migrations (`alembic/versions/001_*.py`, `002_*.py`) or to `core/models/*`. No fix for the pre-existing `uq_users_email` drift. No refactor of the existing `test-integration` postgres service block.

## Files

- `.github/workflows/ci.yml` — add one job.
- `scripts/validate_migrations.sh` — new file, executable.

## Approach

1. **Script (`scripts/validate_migrations.sh`).** Bash, `set -euo pipefail`. Reads `DATABASE_URL` from env. Runs `alembic upgrade head` from the current working directory. Any non-zero exit from alembic propagates. Header comment points at issue codepath/pathreview-ai301-fa26-s1#80 as the follow-up that will add `alembic check`.
2. **CI job (`validate-migrations` in `ci.yml`).** Mirrors the postgres service block from `test-integration`. Steps: checkout, `setup-python@v5` with `python-version: "3.11"`, `pip install -e ".[dev]"`, then `bash scripts/validate_migrations.sh`. `DATABASE_URL: postgresql+asyncpg://pathreview:pathreview@localhost:5432/pathreview_test` (this project's `alembic/env.py` uses `create_async_engine`, so alembic needs the async scheme).
3. The existing `test-integration` job uses the sync `postgresql://` scheme because it runs pytest, which goes through its own SQLAlchemy setup. The new job uses the async scheme because it runs alembic directly. Not refactoring either; just noting the difference so the two job blocks aren't "cleaned up" to match later. Header comment in the script makes the async requirement explicit so a future reader doesn't swap it.

## Test plan

Re-run the unit 2 repro against the change:

1. Add `alembic/versions/003_broken_demo.py` (the same file as unit 2, dropping `reviews.nonexistent_column`).
2. Run the new job locally: `DATABASE_URL=postgresql+asyncpg://pathreview:pathreview@localhost:5433/pathreview_dev bash scripts/validate_migrations.sh`.
3. **Expected after fix:** the script exits non-zero with the same `ProgrammingError: column "nonexistent_column" of relation "reviews" does not exist` line the repro produced. In CI, the `validate-migrations` job fails and blocks the PR.
4. Remove `003_broken_demo.py`, re-run. **Expected:** exit 0, job passes.
5. Confirm the pre-existing `uq_users_email` drift does NOT cause a failure (this PR does not run `alembic check`).

Before / after snapshots go under Evidence in `plan-and-implement.md` after the build.

## Risks and unknowns

- **Follow-up work is visible, not silent.** This PR delivers half the issue's ask. The script's header comment and the PR description both point at issue codepath/pathreview-ai301-fa26-s1#80. If reviewers push back on the split, the fallback is to wait for codepath/pathreview-ai301-fa26-s1#80's answer and bundle the `alembic check` addition into this PR.
- **Async URL requirement.** `alembic/env.py` uses `create_async_engine(settings.database_url)`, so `DATABASE_URL` must use the async scheme. The script passes the env var through directly; the risk is a future reader "aligning" the new job's `DATABASE_URL` with the sync one in `test-integration`. The header comment and the plan.md note call this out, and the fallback is a one-line scheme check in the script if the trap gets hit.
- **Job runtime.** Adds ~1 min to CI. Acceptable.

## Deviations

Nothing changed; the plan held. Script and CI job landed as described, and the test plan produced the predicted results: exit 1 with the expected `ProgrammingError` when `003_broken_demo.py` was present, exit 0 after removal, and the `uq_users_email` drift did not fail the script.
