# Unit 1: Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/51

**Verdict output**

```
Live-mode verdict for #51. Accepted (also considered: #68 BM25 empty-index, #61 SQLAlchemy text() fix, #72 verify_password; all accepted, #51 chosen for strongest DB-internals fit).

```json
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/51",
  "checks": [
    {"name": "issue-open", "grade": "pass", "evidence": "state: open"},
    {"name": "no-open-linked-pr", "grade": "pass", "evidence": "no linked PRs, no in-progress claim comments"},
    {"name": "recent-default-branch-commits", "grade": "pass", "evidence": "last default-branch commit 2026-09-16 by Andrew Burke, 4 days before today"},
    {"name": "maintainer-response-sample", "grade": "unclear", "evidence": "classroom repo, sample not meaningful; preferred check"},
    {"name": "repo-not-archived", "grade": "pass", "evidence": "archived: false"},
    {"name": "repo-in-active-use", "grade": "pass", "evidence": "last push 2026-09-16, within 180 days"},
    {"name": "bounded-newcomer-scope", "grade": "pass", "evidence": "issue body names two files (.github/workflows/ci.yml, scripts/validate_migrations.sh) and one concrete deliverable: spin up fresh DB, run migrations in order, verify schema matches SQLAlchemy models; not umbrella, not still-debated"},
    {"name": "not-support-request", "grade": "pass", "evidence": "concrete change request, not a how-do-I question"},
    {"name": "no-ai-contribution-ban", "grade": "pass", "evidence": "no CONTRIBUTING.md or AI_POLICY.md; silence passes"},
    {"name": "has-triage-label", "grade": "pass", "evidence": "labels: enhancement, devops, tests, tier-3"},
    {"name": "no-abandoned-attempts", "grade": "pass", "evidence": "no closed PRs, no claim comments"},
    {"name": "good-first-issue-label", "grade": "fail", "evidence": "no 'good first issue' label; tier-3 instead. Preferred check, does not affect verdict"}
  ],
  "verdict": "accept"
}
```
```

---

## Eval iterations

**Run history**

1. Full run 1: `agreement: 11/20 scored items  (bar: 18/20: below the bar; category floor unmet: no match in clear-accept)`
2. Full run 2: `agreement: 15/20 scored items  (bar: 18/20: below the bar)`. Relaxed `maintainer-response-sample` to "at least one response within 90 days"; promoted `no-abandoned-attempts` to required.
3. Targeted `--only issue-06,issue-09,issue-14,issue-20`: `agreement: 4/4 scored items`. Downgraded `maintainer-response-sample` to preferred; added required `maintainer-triaged`; tightened `no-abandoned-attempts`.
4. Full run 3: `agreement: 17/20 scored items  (bar: 18/20: below the bar)`. New `maintainer-triaged` over-rejected issue-01 and issue-16.
5. Targeted `--only issue-01,issue-16,issue-20`: `agreement: 3/3 scored items`. Replaced `maintainer-triaged` with `has-triage-label`.
6. Full run 4 (committed as `eval-run.txt`): `agreement: 19/20 scored items  (bar: 18/20: PASS)`.

**Issue analysis**

`issue-19`. Rubric: `reject`. Gold: `accept`. The bundle lists *"two potential causes which should be fixed: 1. The matchers are slow ... 2. UI update is waiting for the matching thread to finish."* My `bounded-newcomer-scope` check requires "one bounded piece of work"; the model read the two enumerated causes as two distinct fixes and failed the check. Gold treats them as diagnostic directions inside a single bug (same matcher/UI code path). I chose not to loosen the check to recover this one, since letting multi-item issues pass would also let real umbrella issues through elsewhere in the eval set.

**Check rationale**

Quoted verbatim from `tools/issue-select/rubric.md`:

> | has-triage-label | Issue labels line (eval: `labels:` on the "opened by" line, or the issue's `labels` field; live: label chips on the issue page). See Family 3/4 | The issue has at least one label applied. "labels: none" fails | required |

An earlier draft required a *newcomer-friendly* label applied by a maintainer, which false-rejected issue-01 and issue-16 (clear-accepts opened by a `CONTRIBUTOR` with only a topic label like `type::bug`). The distinguishing signal against un-triaged bot spam (issue-20: `cursor[bot]` opener, `labels: none`) is simply that any label was applied at all. Real repos with a triage workflow (label bot, template, human) apply at least one; drive-by spam usually carries none.

**Trade-offs**

Gives up any judgment on label quality (an issue tagged only `duplicate` would pass) and gives up crediting an issue for a maintainer opener. Canary: `--only issue-01,issue-16,issue-20` returned `agreement: 3/3` after the change (01 and 16 flipped to accept; 20 stayed reject). No other verdicts changed in the following full run, because every other reject was already failing on additional required checks.

---

## Selection rationale

1. **Fit to interests and time.** Database internals is the strongest interest in `scope.md`, and #51 is squarely there: apply Alembic migrations against a fresh Postgres, then reflect the resulting schema and compare it to the SQLAlchemy models. Estimated 5-7h, higher than the tier-1 candidates I also considered, but the extra hours are aimed at the interest I most want to build on.

2. **What the verdict identified vs what I weighed.** The rubric confirmed the issue is bounded (two named files, one deliverable), in an actively pushed repo, staff-authored, and unclaimed. What the rubric could not weigh is a real gap I verified by reading `.github/workflows/ci.yml`: this repo has no deployment step, and the `test-integration` job spins up a Postgres service but never runs migrations against it, so nothing currently catches a broken migration before merge. The `test-integration` service is a natural attach point for the fix. This raised my confidence that the 5-7h estimate is realistic rather than open-ended.

3. **Anticipated difficulty in claiming.** No assignee, no linked PRs, no claim comments. Path Review is a staff repo with active pushes, so approval friction is low. The reproduction shape for Unit 2 is a demonstrable gap in `ci.yml` rather than a stack trace, which is more setup than a tier-1 bug but is grounded in files that already exist.

---

Related paths: `eval-run.txt` in this directory; skill files in `tools/issue-select/`.
