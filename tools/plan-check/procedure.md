# Procedure: how this skill grades a plan package

<!--
THIS IS THE PART YOU WRITE, and it is a new kind of part. Weeks 1 and
2, SKILL.md carried a numbered workflow and you only wrote judgment
files. This week the workflow is gone from the frame: SKILL.md says
"execute procedure.md", and these are the operating steps you author.
The machinery is in your hands now.

Your operator swap is the design brief. When your executor stalled
because your rubric said WHAT to decide but not HOW to find the
evidence, that was a procedure gap. This file is where those gaps get
closed: a complete procedure lets someone who has never seen a plan
package before (a groupmate, or the skill itself) grade one exactly the
way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
repro evidence before the plan, and note what behavior it pins down"
is a step; "understand the context" is a wish.
-->

## Read order

1. Read `rubric.md`: list the four checks and the verdict rule.
2. Read `references/evidence-guide.md`: note where each check's evidence lives.
3. Read the package in this order, noting as you go:
   1. **Issue**: the specific behavior and any stated constraint.
   2. **Thread highlights**: what a maintainer has already said (rejected approach, scoping ask).
   3. **Repo facts**: contribution policy, AI-disclosure requirement.
   4. **Repro evidence**: exact steps, observed behavior, mechanism the evidence points at.
   5. **Candidate plan**: cause, named file/module, in/out-of-scope, test plan.
   6. **Candidate plan comment**: what it claims and what it defers.
4. Read the plan and comment last: repro evidence is the yardstick, not the other way around.

## Evidence gathering

For each check, record a quote or concrete fact, not a paraphrase.

- **diagnosis-grounded**: the plan's cause sentence, the repro line it explains, the mechanism named. If cause = symptom restated, record that.
- **scope-bounded**: the scope statement (in and out). The file or module named. If no out-of-scope, record "none stated."
- **test-plan-observable**: the test steps and the specific observable named (color flip, exit code, error string). If only "verify," record that.
- **comment-respects-context**: the comment's key sentences, the thread highlight and repo-facts line it must respect, whether AI disclosure is required and present.

## Check execution

1. Grade all four checks in rubric order. Do not short-circuit on an early fail.
2. Apply each pass condition to the gathered evidence. Grade `pass`, `fail`, or `unclear` with a one-line evidence quote.
3. `unclear` means the evidence is genuinely absent. Present-but-ambiguous grades `fail`.
4. Do not re-grade a check from a later section's evidence.
5. If a grade needs a judgment the rubric does not state, the missing rule belongs in the rubric, not in this run.

## Verdict assembly

1. Apply the verdict rule: accept if every required check passes; reject otherwise.
2. `unclear` counts as fail.
3. In the JSON, list every check with its grade and one-line evidence. Quote the deciding check's evidence verbatim.
4. Emit exactly one verdict: `accept` or `reject`.
