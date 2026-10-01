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

## Things I never post

- A fix promise or an ETA.
- Speculation without evidence.
- Root-cause claims I have not verified from the artifact.
- "Should be easy" or "just" about someone else's code.
- A piggyback on a classmate's repro instead of my own.
