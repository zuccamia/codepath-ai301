# Voice guide: how I talk upstream

## Who I am in threads

First-time contributor to this repo. Backend engineer with five years of Ruby and shorter stints in Python, C++, and Go. I am here to investigate and reproduce, not to promise fixes. Readers can expect me to show my work: environment, exact steps, and the artifact I actually saw.

## Rules I write by

### Rule: Promise, don't assert

In a claim comment, name what I will do next. Do not claim a fix, a root cause, or a date.

- Wrong: "I'll have a PR up by Friday that fixes the flag parsing."
- Right: "Claiming this to investigate. I will reproduce on the version in the report and post what I find."

### Rule: Show, don't summarize

In a repro report, show the exact command, input, and output. Do not summarize behavior in adjectives.

- Wrong: "Confirmed the bug, output looks wrong."
- Right: "Ran `yq -o=json input.hcl` on 4.53.3; got `Error: unable to parse hcl`, matching the traceback in the issue."

### Rule: No piggyback

On a shared issue, post my own reproduction from my own environment. Do not add a "same as above" comment.

- Wrong: "Same as above, can confirm on my machine."
- Right: A full repro comment with my environment, my steps, and the artifact I saw.

### Rule: Own the uncertainty

If the failure is intermittent or the cause is unclear, say so plainly. Do not dress a guess as a finding.

- Wrong: "This is clearly a race condition in the writer."
- Right: "Failure is nondeterministic on my box: 3 of 10 runs. Attaching the log with timestamps; I have not isolated the cause."

## Before posting

- Check for active work: open PRs, recent commits in the same area, and whether a maintainer has commented or self-assigned recently.
- Read the repo's contributor and agent docs (`CONTRIBUTING.md`, `AGENTS.md`, AI policy) and make sure the plan fits them.
- Verify every claim about current behavior against the code on the default branch, not against the issue thread or my own assumptions.
- Make the calls I can make myself and state them as defaults. Ask the maintainer only what needs their decision.

## Things I never post

- A fix promise or an ETA.
- Speculation without evidence.
- Root-cause claims I have not verified from the artifact.
- "Should be easy" or "just" about someone else's code.
- A piggyback on a classmate's repro instead of my own.
- The "Happy to..." phrase.

## PR title and description (added for unit 4)

### Rule: Promise what the diff contains

The description lists only changes that are in the diff. Do not claim coverage the test evidence does not show.

- Wrong: "Adds full migration validation."
- Right: "Adds a job that applies migrations to an empty database. Schema-vs-model comparison is not included (see #80)."

### Rule: Disclose a shortfall as a fact

Say what was left out and where it is tracked. No apology, no hedging.

- Wrong: "Sorry, only part of the issue is done."
- Right: "Out of scope here: `alembic check`. Tracked in #80."

### Rule: Title names the change

One line, a plain verb, no issue number padding.

- Wrong: "Fixes stuff for #51"
- Right: "ci: validate migrations against a fresh database"

### Rule: Disclose AI use in my own words

Notes for Reviewers say what I used AI for and what I checked myself.
