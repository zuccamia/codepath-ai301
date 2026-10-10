# Procedure: how this tool grades a PR package

## Read order

1. Read `rubric.md`: list the four checks and the verdict rule.
2. Read `references/evidence-guide.md`: note where each check's evidence lives.
3. Read the package in this order:
   1. **Issue and thread**: the ask and any maintainer direction.
   2. **Repo facts / PR template / CONTRIBUTING**: required sections, AI-disclosure policy.
   3. **Plan**: list the files it names, its In and Out lines, its test plan, and any Deviations.
   4. **Diff and commits**: list every changed file and what each hunk does.
   5. **Test evidence**: what was run and what it printed.
   6. **Title and description**: what it claims.
4. Read the plan before the diff and the diff before the description, so each later item is judged against the earlier one, never the reverse.

## Evidence gathering

Record a quote or concrete fact, not a paraphrase.

- **plan-fidelity**: set A = files the plan names (plus deviation files). Set B = files in the diff. Record B minus A (extra) and A minus B (missing). For each description claim, record the hunk that backs it, or "no hunk".
- **tested**: the plan's test plan steps, then the before and after output the evidence shows for each. List each repo check named in the template and the outcome the evidence states for it.
- **diff-reviewable**: list debug lines (print, console.log, TODO, leftover logging), commented-out code, unrelated hunks, whitespace-only churn.
- **standards-met**: each template section and whether it has real content; the issue reference; the AI-use disclosure; any maintainer ask in the thread or policy and whether the PR honors it.

## Check execution

1. Grade all four checks in rubric order. Do not stop at the first fail.
2. Apply each pass condition to the gathered evidence. Grade `pass`, `fail`, or `unclear`, with a one-line quote.
3. `unclear` means the evidence is absent from the package. Present but weak or ambiguous evidence grades `fail`.
4. An extra or missing item is not a fail if the plan's Deviations or the description discloses it honestly. A silent one is a fail.
5. Do not grade a check from another family's evidence, except that description claims are read against the diff under plan-fidelity.
6. If a call needs a rule the rubric lacks, report the gap rather than improvise.

## Verdict assembly

1. Apply the verdict rule: accept if every required check passes, reject otherwise. `unclear` counts as fail.
2. List every check in the JSON with its grade and a one-line evidence quote.
3. The deciding check is the first failing check in rubric order. Quote its evidence verbatim in the summary.
4. Emit exactly one verdict, `accept` or `reject`, in the final JSON block.
