# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

zuccamia

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/51#issuecomment-5861693913

Claiming this one to investigate as a first contribution. I will set up the sandbox repo locally per the README, try to add a fresh-database migration step in `.github/workflows/ci.yml` calling `scripts/validate_migrations.sh`, and confirm the SQLAlchemy-model check catches a deliberately broken migration. I will post a reproduction of the current gap (CI passing on a migration that would fail against a clean DB) as my next comment.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/51#issuecomment-5863628115

Reproduction of the gap this issue describes: CI accepts a migration that will not apply on a fresh database.

## Environment

- macOS 26.5.2 (arm64)
- Python 3.11.15
- alembic 1.20.0
- PostgreSQL 16.14 (via the repo's `docker compose` `db` service, port 5433, credentials as declared in `docker-compose.yml`)
- Fork clone at `zuccamia/pathreview-ai301-fa26-s1@main` (no local changes to committed migrations)
- `DATABASE_URL=postgresql+asyncpg://pathreview:pathreview@localhost:5433/pathreview_dev`
  - Note: the README's plain `postgresql://` scheme does not work with the async engine in `core/database.py`; asyncpg dialect is what the declared deps support. Not the subject of this issue, flagging as observed.

## Baseline: current migrations apply cleanly

```
$ alembic downgrade base
$ alembic upgrade head
INFO  [alembic.runtime.migration] Running upgrade  -> 001, Initial schema creation ...
INFO  [alembic.runtime.migration] Running upgrade 001 -> 002, Add error_message column to reviews table.
$ alembic current
002 (head)
```

Two committed migrations. Both apply on a fresh DB with no error.

## Reproducing the gap

Added a deliberately broken migration at `alembic/versions/003_broken_demo.py`:

```python
"""demo broken migration referencing a nonexistent column

Revision ID: 003_broken_demo
Revises: 002
"""
from alembic import op

revision = "003_broken_demo"
down_revision = "002"
branch_labels = None
depends_on = None

def upgrade():
    op.drop_column("reviews", "nonexistent_column")

def downgrade():
    pass
```

Ran `alembic upgrade head` on a clean DB:

```
INFO  [alembic.runtime.migration] Running upgrade 002 -> 003_broken_demo, demo broken migration referencing a nonexistent column
...
sqlalchemy.exc.ProgrammingError: (sqlalchemy.dialects.postgresql.asyncpg.ProgrammingError)
column "nonexistent_column" of relation "reviews" does not exist
[SQL: ALTER TABLE reviews DROP COLUMN nonexistent_column]
```

Migration fails immediately, as expected.

## The gap

Current CI (`.github/workflows/ci.yml`) has no step that runs migrations against a fresh DB:

```
$ grep -E "alembic|migration" .github/workflows/ci.yml || echo "no migration check in ci.yml"
no migration check in ci.yml
```

The script the issue names does not exist:

```
$ ls scripts/validate_migrations.sh
ls: scripts/validate_migrations.sh: No such file or directory
```

Meaning: a PR introducing the broken migration above would pass the current lint, typecheck, unit-test, integration-test, and frontend jobs, and only fail at deploy time on a fresh install.

## Observed vs. expected

Observed today: CI approves PRs whose migrations would not apply on a fresh database, because no job attempts that.

Expected once this issue is resolved: a CI job spinning up an empty Postgres, running `alembic upgrade head`, and (per the issue) confirming the resulting schema matches the SQLAlchemy models in `core/models/` would fail on the migration above and block the PR.

Cleaning up: `alembic downgrade base` and `rm alembic/versions/003_broken_demo.py` return the fork to a clean state.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run, initial rubric: 18/20 (PASS). Disagreements on pkg-05 and pkg-12, both flagged `failed: steps-complete`.
2. Partial run `--only pkg-05,pkg-12,pkg-18,pkg-20` after loosening `steps-complete` to accept "shown or specified precisely enough (exact fields, values, and shape) that a reader could reconstruct them without inventing content": pkg-05 flipped to accept, pkg-12 still failed, pkg-18 (steps canary) held reject, pkg-20 (disclosure canary) held reject.
3. Confirming full run: 19/20 (PASS). This matches the `agreement: 19/20 scored items  (bar: 18/20: PASS)` line in the committed `eval-run.txt`.

**Package analysis**

pkg-12 (prettier/prettier#19795). My rubric decided `reject` on `steps-complete`; the gold label is `accept` with the note "both shapes reproduced on the current release with outputs shown; version delta stated; next step concrete". My rubric read it that way because the report's steps section says `with repro.mjs containing the issue's two prettier.format calls` and lists the range parameters, but never pastes the actual `prettier.format` call site inline. The input strings do appear in the issue context earlier in the bundle, so a reader can reconstruct the script, but only by reading the issue as well as the report. My `steps-complete` treats the report as self-contained, so pointing to "the issue's two prettier.format calls" reads as a reference the report itself does not carry.

**Check rationale**

> steps-complete | the steps section of the repro report | a stranger with only the report could go from a clean state to the failing behavior without guessing: install and setup are included, commands are exact, and inputs are shown or specified precisely enough (exact fields, values, and shape) that a reader could reconstruct them without inventing content | required

It reads that way after one loosening. The first version ended at "and inputs are shown". That flagged pkg-05 (which described a `env.yml` with a valid `dependencies:` list and a `category:` section, without pasting the file) and pkg-12 (which described `repro.mjs` by parameters). Both are clear-accepts in the gold labels. I added "or specified precisely enough (exact fields, values, and shape) that a reader could reconstruct them without inventing content" to let a shape-and-values description pass while still rejecting handwave like "some yaml with a bad section". I rejected the wider version "or reference material in the issue" because that would give the report credit for content it does not carry, and would open the door to reject packages like pkg-18 whose repro also "points to" material a stranger cannot reach (a private monorepo).

**Trade-offs**

The loosening fixed pkg-05 and left pkg-12 still failing on `steps-complete`. I accept that miss: pkg-12's report chooses to reference "the issue's two prettier.format calls" rather than paste them, and my check keeps its "self-contained report" bar to hold the line against pkg-18-shaped rejects where the repro also lives outside the comment. I confirmed the loosening did not regress elsewhere with two canaries in the `--only` re-run: pkg-18 (unfollowable-comms, closest steps-shaped reject) stayed reject, and pkg-20 (the single-package disclosure category the floor exists for) stayed reject. The confirming full run held every other agreeing package.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
