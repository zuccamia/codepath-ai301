# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/124

**Branch**

`feat/51-validate-migrations`

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full: 19/20 (missed pkg-16).
2. Full: 15/20. I only removed the instruction comments from my files. Five clear-accepts failed on `tested`.
3. Partial (9 packages): 9/9 after loosening `tested`.
4. Full: 20/20.
5. Full: 20/20, after a wording cleanup in SKILL.md and procedure.md.
6. Full: 20/20, after rewording the live-mode inputs in SKILL.md.
7. Full: 20/20, after removing the house-chain lines from SKILL.md. Matches `eval-run.txt`.

**Package analysis**

`pkg-05`. My rubric: accept. Gold: accept. It touches only the files the plan names, and the description matches the diff. The tests are reported as "`cargo test -p nu-protocol` passes (312 tests)" with no pasted output. A stated result that nothing contradicts passes `tested`. My first rubric rejected it for that.

**Check rationale**

> | tested | the test evidence read against the plan's test plan and the repro steps | all hold: (a) the repro steps are re-run with an observable before and after; (b) the repo's own checks or test suite are reported with a stated outcome (a one-line result such as "suite passes" counts; pasted output is not required; a failure reported honestly with its reason counts; checks never mentioned, or a result the evidence contradicts, fail); (c) the evidence names a specific observable, not just "tests pass" | required |

My first version required pasted output for the repo's checks. That rejected five clear-accepts, so I accepted a stated result and kept the fail for checks never mentioned or contradicted.

Finding: removing the instruction comments looked harmless but changed the score, from 19/20 to 15/20. The harness puts each file in the prompt with comments included. The comments held a rule that an honest shortfall can pass, so I put that rule in the rubric itself. I did not test whether the comments or run-to-run variance caused the drop.

**Trade-offs**

The loosened check can miss a PR that falsely claims "tests pass" when nothing contradicts it. I re-ran pkg-04, 07, 10 and 14 (not-tested) as canaries and all still reject.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
