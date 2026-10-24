# Contribution: Page text index never drops postings of replaced or evicted pages

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Hoang Nguyen Bich Ngoc
**GitHub Username:** zuccamia
**Issue:** https://github.com/Muvon/octoweb/issues/21
**Pull Request:** https://github.com/Muvon/octoweb/pull/27

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Built issue-scout's search step, then fixed its ranking twice**

My Unit 1 skill only graded issues I handed it, so I gave it a search
step: `gh search issues` queries from my fit profile (C++, Rust, Ruby,
and Go, on database and indexing work) and ranking rules that pick the
top 5 to grade with my Unit 1 rubric. The first scan (2026-10-21)
accepted mar123io/sonora#3 and #2, the same issue filed twice in a
0-star repo where no issue had ever gotten a reply, so I added a
10-star floor (a `gh api` lookup per repo, since `gh search issues` has
no stars filter), a rule that drops duplicate titles, and made
multi-query matches only a tie-break. The re-scan on 2026-10-22 dropped
sonora but filled the top 5 with UI panels and game audio, because one
keyword like "index" matches almost anything, so I added storage,
search, and vector queries, dropped frontend words in titles, and
ranked titles with a fit term first. The next scan's top 5 were all
search, storage, or database work, and I also tightened the rubric's
triage-label check back to my Unit 1 rule after loosening it to reuse
the rubric in another tool of mine.

**Unit 6: Installed the voice guard and wrote a repo memory file for octoweb**

I installed the voice guard as a second Bash hook next to my existing
RTK hook, folded my three voice-guide copies into one, and added
Upstream rules for claims, questions, and pings. It clashed with my own
guide twice: my Unit 2 rule banned any date while the claim had to say
when I would report back, so I rewrote my Claims rule to tie the
report-back to the work, not a date (a long window also invites someone
else to jump in); and it stopped my octoweb draft's "I'll write the fix
myself and disclose any AI help in the PR" as a fix promise, so I
reworded it to say how I would work instead. My `CLAUDE.local.md` for
octoweb (37 lines, kept out of git with `.git/info/exclude`) imports
`AGENTS.md`, maps `page_index.rs` and its callers, and records the
issue's rule that I write the fix myself, so every Claude session in
that repo starts from it. My first guess for a failing test on
`master` was Claude Code's sandbox; running it outside the sandbox
proved that wrong (macOS 26 drops the quarantine agent name for
non-app processes, while upstream CI on macos-15 passes), so I
corrected the note.

**Unit 7: Installed the ship-gate, wrote a sourced manifest, and gated PR #27**

I installed the ship-gate on my octoweb clone, which also swapped
pr-precheck's sandbox scope for the real-world one (before that it had
refused to grade octoweb at all), and wrote `ship-manifest.md` from the
issue's own checks and the repo's CI: format and the CI-form test suite
must pass, while Clippy (red on `master` in `agent.rs` and `mail.rs`)
and one macOS-26-only quarantine test are excluded with dated reasons.
The manifest needed full toolchain paths, because the gate's `/bin/sh`
does not read my `~/.zshrc`, so I dry-ran each command in a stripped
shell first; the canary gave both stops, and the first real run passed
all four of my rubric checks. I then made two small wording edits to the
description before opening PR #27, so I re-ran the gate on the text as
shipped, and it passed again, with the same voice note that my AI
disclosure should say what I checked myself.

**Unit 8: Installed the PR-monitor and wrote a playbook from octoweb's own rhythm**

I installed the PR-monitor and ran its free state check, which found
PR #27 quiet, with no CI run yet and merge state UNSTABLE, then wrote my
playbook from the repo's history: its one outside PR merged in 12
hours, but the maintainer once went 25 days without merging anything,
so my first-response threshold is 7 days, which is also my voice
guide's floor for a ping. The full run drafted a wait note instead of a
nudge, quoting that threshold, and it read the missing CI run the way my
playbook says (expected for a first-time fork PR, not a reason to
nudge), so I posted nothing and will re-run it on or after 2026-10-30.

---

## Why I Chose This Issue

My first pick was valeriansaliou/sonic#394, making Sonic's typo factor
configurable. A personal tool of mine found it: a daily GitHub Actions
run that grades the open issues in the repos I watch, using a copy of my
Unit 1 rubric. It ranked #394 first on 2026-10-19, the day the
maintainer wrote that the earlier contributor's PR was lost with their
account. My course scout accepted it too (moderate), and I claimed it,
but the maintainer replied within hours that the contributor was only
suspended and would reopen their PR, so I released it. I chose
Muvon/octoweb#21 next. My course scout's second wild scan on
2026-10-22 surfaced it at rank 6 (the Rust "query" and "search"
queries). I graded it directly and it was accepted; under the Unit 6
scope patch its difficulty note is straightforward, because the issue names
the lines, the fix (a private `unindex` helper), and the two tests. It
fits what I want to learn: `PageIndex` is a small inverted index in
Rust, and the bug is what happens to posting lists when a document is
replaced or evicted. At my previous job I migrated our search from
MySQL to Elasticsearch, so I have used inverted indexes from the
outside; this lets me maintain one from the inside. The issue asks
that a human write the fix and disclose any AI help, so I am writing
the fix myself. The repo has no CONTRIBUTING.md; the issue body states
its rules instead. I claimed before asking about that in the course's
issue-selection channel, and asked right after. My claim:
https://github.com/Muvon/octoweb/issues/21#issuecomment-6354983975

My course scout worked better as a filter than as a finder. Its first
scan accepted a duplicate issue in a 0-star repo, and the next one put
UI panels and game audio on top, so I added a star floor, fit-first
ranking, and three more queries; after that, the top five were real
search and storage work, octoweb#21 among the first six. But it missed
sonic#394, because "typo" is not a fit term and sonic marks search work
with labels my ranking never reads. And no rubric could have seen that
#394 was taken: a suspended account's PR is invisible to the API.
Asking in the claim surfaced that within hours. The rubric also passed
octoweb#21's AI check without noticing the issue's own rule ("don't let
an agent claim or solve it"), because the check reads only repo policy
files, not the issue body.

**Toolkit**

- Tool: issue-scout (my search strategy and my rubric). I ran two full
  scans, then graded six candidates directly, including sonic#394
  (which my personal daily scout surfaced) and octoweb#21.
- What it produced: an accept on octoweb#21 with a straightforward
  difficulty note, and an accept on sonic#394 (moderate) that turned
  out to be taken.
- Difficulties: ranking had no fit signal at first; the strict scope
  check rejected most real issues; the scans missed sonic#394; and the
  rubric cannot see a suspended contributor's PR or an AI rule written
  in an issue body.
- What changed: a 10-star floor, fit-first ranking, a frontend title
  filter, and three new queries. In Unit 6 I replaced
  `bounded-newcomer-scope` with `scope-feasible` plus a difficulty
  note, which turned serenedb#1198 from reject to accept. Still to do:
  read labels in ranking, and read the issue body in the AI check.

---

## Understanding the Issue

**Problem Description**

Octoweb keeps a small full-text index of visited pages (`PageIndex`):
`docs` holds each page, and `terms` maps every word to the ids of the
pages that contain it (its posting list). When a page is re-indexed or
evicted at the 2,000-page cap, its doc leaves `docs`, but its id stays
in every posting list, and words only that page used stay as keys.

**Expected Behavior**

When a doc is removed, its id leaves every term it was added under, and
a term with no ids left is removed, so `terms` only describes live
pages.

**Current Behavior**

Every revisit adds a new id under each of the page's terms while the old
id stays, so `terms` grows for the whole session until a restart
rebuilds it. Search results are still correct, because `search()`
filters out ids that are not in `docs`, but dead keys count against the
200-key prefix budget (`MAX_PREFIX_KEYS`), so a short prefix can stop
before reaching a term a live page still has.

**Affected Components**

Only `src/page_index.rs`: `index_capped` (the two removals, the
re-index `retain` and the cap eviction `remove(0)`), `push` (the only
place ids are added), and `search` (which hides the bug). The callers in
`src/main.rs` do not change.

**Toolkit**

- Tool: my repo memory file, `CLAUDE.local.md`, written from one scoped
  orientation of `page_index.rs` and its callers, plus Claude Code to
  explain the code.
- What it produced: a 38-line map of the subsystem (structs, the write
  and read paths, callers, where tests live, build commands), loaded in
  every session in the clone.
- Difficulties: the issue forbids an agent solving it, so I had to keep
  Claude to explaining and reviewing; and while exploring the app I saw a
  related but different bug (a page removed from history stays
  searchable), which I first mixed up with #21.
- What changed: I recorded the no-agent rule in the memory file, and
  separated the two bugs by reading the code: search only returns live
  docs, so what I saw in the app was a live doc, not a dead id. I
  reported the second one on the issue thread instead of folding it in.

---

## Reproduction Process

**Environment Setup**

macOS 26.5.2 on Apple Silicon (arm64), Rust 1.99.1 stable through
rustup (octoweb pins no version; upstream CI also runs 1.99.1), Xcode
Command Line Tools, octoweb `master` at `9257131`. I forked
Muvon/octoweb and cloned the fork. No API key or octomind is needed for
`page_index` work, and the crate builds only on macOS. The one setup
problem was my Homebrew Rust: it broke after Homebrew upgraded LLVM
(`rustc` failed to start with a missing LLVM symbol), so I replaced it
with Homebrew's rustup and put its keg-only `bin` on my PATH.

**Steps to Reproduce**

1. `git clone https://github.com/zuccamia/octoweb.git && cd octoweb`,
   then `git checkout 9e94008`, the commit that adds only two tests on
   top of `master` `9257131`. They re-index `a.example/pricing/` without
   "dollars", and re-index the "Rust notes" page without "notes" while a
   second page still has it, then read the private `terms` map.
2. `cargo test --locked --bin octoweb page_index`
3. Observed: the two existing tests pass and both new ones fail. After
   the re-index, `"dollars"` is still a key in `terms` (line 264), and
   `terms["notes"]` is `[1, 2]` instead of `[2]` (line 281): id 1 is the
   replaced page, gone from `docs` but still in the posting.

**Reproduction Evidence**

- **Commit showing reproduction:** https://github.com/zuccamia/octoweb/commit/9e94008591e47b55610c7843ee729fa6365783e6
- **Screenshots/logs:**

  ```text
  test page_index::tests::reindex_or_cap_prunes_old_terms ... FAILED
  test page_index::tests::shared_terms_preserved_for_live_page_after_reindex_and_cap ... FAILED
  panicked at src/page_index.rs:264:9: old keys pruned from terms
  panicked at src/page_index.rs:281:9: assertion `left == right` failed
    left: [1, 2]
   right: [2]
  test result: FAILED. 2 passed; 2 failed; 0 ignored; 0 measured; 254 filtered out
  ```

- **My findings:** the bug is real on current `master`, and only in
  storage: the search checks in the same test pass, because `search()`
  filters dead ids. The app itself cannot show it, since `terms` lives
  only in memory and `page_index.json` holds only docs, so a unit test
  that reads `terms` was the only direct evidence. On the same commit,
  two unrelated things fail: Clippy (`-D warnings`) in `agent.rs` and
  `mail.rs`, as in upstream CI, and `saved_attachments_are_quarantined`,
  which I traced to macOS 26 refusing a non-app process's quarantine
  agent name (it passes in CI on macos-15).

**Toolkit**

- Tool: repro-check, with the wild scope from Unit 6, grading my claim
  and my repro report together.
- What it produced: accept on all five checks, after one round. It
  confirmed the line numbers against the test commit and that
  `page_index.rs` is unchanged since the issue's permalinks.
- Difficulties: my first octoweb draft pointed at no commit, because I
  had added the tests to `master` only temporarily; and on my earlier
  pick (sonic) repro-check rejected my report for claims the data did
  not show, before the maintainer said that issue was taken.
- What changed: I split my branch into a tests-only commit and a fix
  commit, so the reproduction is a commit anyone can check out, and the
  report now points at it.

---

## Solution Approach

**Analysis**

`push` is the only place ids enter `terms`: it tokenizes the doc's
title and text and appends the new id under each term. `index_capped`
removes docs in two places, `self.docs.retain(...)` for the old copy of
a re-indexed URL and `self.docs.remove(0)` for eviction at the cap, and
both drop the doc without ever undoing what `push` added. Nothing else
removes ids, so they stay until `load()` rebuilds `terms` on the next
launch.

**Proposed Solution**

Add a private `unindex(id, doc)`, the inverse of `push`: tokenize the
doc the same way, remove the id from each term's posting, and drop terms
left empty. Call it at both removal points. I deliberately did not
change the SQLite FTS5 move noted in the module header, the `MAX_*` limits,
the tokenizer, or `search()`'s live-id filter, and I did not fix the
history-removal bug, which is a separate issue.

**Implementation Plan**

1. Add `unindex(&mut self, id: u64, doc: &Doc)` in `src/page_index.rs`.
2. In `index_capped`, keep the removed docs instead of discarding them,
   and call `unindex` for each.
3. Update the comment on `terms`.
4. Add the two tests the issue describes, next to
   `reindex_replaces_and_cap_evicts_oldest`.

Deviations: `retain` throws the removed doc away, so I used
`extract_if` and collected the removed docs first (calling `unindex`
inside the loop would borrow `self` twice). My shared-term test uses
`"notes"` and adds checks the issue did not ask for: the search count at
each step, and that the key disappears once its last page is evicted.
The plan was the maintainer's own Fix and Tests sections, so I did not
run plan-check; I wrote it down as `plan.md` for the gate instead.

**Toolkit**

- Tool: the issue's own Fix section as the plan, written up as
  `plan.md` (with a Deviations section) for pr-precheck to read.
- What it produced: a plan with In and Out scope and three recorded
  deviations, which the gate's plan-fidelity check matched against the
  diff.
- Difficulties: plan-check, my Unit 3 tool, had no plan of mine to
  grade, because the maintainer had already written it; and the first
  version of my fix inverted the `extract_if` condition (I copied `!=`
  from `retain`), which removed every other page.
- What changed: the maintainer's two existing tests caught the inverted
  condition immediately, so I flipped it; the deviations went into
  `plan.md` as they happened.

---

## Testing Strategy

The reproduction re-run before and after: the tests-only commit
`9e94008` fails at line 264 (`"dollars"` still a key) and line 281
(`[1, 2]` instead of `[2]`), and the fix commit passes all four
`page_index` tests. Each test checks `terms` directly, since search was
already correct, and the shared-term test also checks search at every
step. The repo's own checks, from the issue: `cargo fmt --all -- --check`
is clean; `cargo test --locked --all` gives 255 passed, 1 failed (the
macOS-26-only `saved_attachments_are_quarantined`, which also fails on
`master` locally), 2 ignored; CI's form with that test skipped gives
255 passed, 0 failed; and Clippy fails only on the two `master` errors,
with nothing from `page_index.rs`.

**Toolkit**

- Tool: the ship-gate's manifest, which runs the repo's checks in a
  fresh shell before every ship.
- What it produced: two must-pass rows (format, CI-form test suite),
  both ok on the branch, and two dated exclusions (Clippy red on
  `master`; the quarantine test I cannot run here).
- Difficulties: a bare `cargo` would have exited 127 in the gate's
  shell, and `cargo test --locked --all` can never pass on my macOS 26
  machine.
- What changed: I wrote the commands with full toolchain paths and
  dry-ran them in a stripped shell, and skipped exactly one named test
  instead of excluding the whole suite.

---

## Implementation Notes

**2026-10-22**

Claimed valeriansaliou/sonic#394 and built a timing repro (typos made
long queries about 90x slower). The maintainer replied that an earlier
contributor was only suspended and would reopen their PR, so I released
it and claimed Muvon/octoweb#21 instead.

**2026-10-23**

donk8r assigned me #21. Forked and cloned, replaced my broken Homebrew
Rust with rustup, and wrote `CLAUDE.local.md`. A baseline run showed one
failing test on `master`; I first blamed Claude Code's sandbox, then
proved that wrong by running outside it and with a plain Swift script:
macOS 26 drops the quarantine agent name for non-app processes. Clippy
is red on `master` too. While exploring the app I found that a page
removed from history stays searchable by content, a separate bug I
reported on the thread.

**2026-10-23**

Wrote the two tests. My first assertions passed when they should have
failed, because I left out the `!`, so they were asserting the bug.
Then wrote `unindex`; my first `extract_if` used `!=` copied from
`retain` and removed every other page, which the maintainer's existing
tests caught. Split the work into a tests-only commit and a fix commit,
and lost the `terms` comment update along the way (it had never actually
been saved), which a rubric read of the PR draft caught before I opened
the PR.

**Toolkit**

- Tool: Claude Code as an explainer and reviewer, inside the issue's
  rule that a human writes the fix; my memory file kept that rule in
  every session.
- What it produced: explanations of Rust ownership, borrowing, the
  `entry` and `extract_if` APIs, and reviews of my test logic and diff.
- Difficulties: drawing the line between explaining syntax and writing
  my code; I asked for syntax on unrelated examples rather than for the
  lines themselves.
- What changed: I wrote the tests and the fix myself and disclosed the
  rest in the PR's AI section.

---

## Pull Request

**PR Link:** https://github.com/Muvon/octoweb/pull/27

**PR Description:** `Closes #21`. It explains that removed docs left
their ids in every posting, then lists the change (a private
`PageIndex::unindex`, called from `index_capped` for the re-indexed doc
via `extract_if` and for each evicted doc; the `terms` comment), the two
tests and the separate failing test commit `9e94008`, the repo's checks
with honest outcomes (fmt clean; one macOS-26-only test failure; Clippy
red on `master` in `agent.rs` and `mail.rs`, nothing new), and an AI-use
section stating what Claude Code helped with and that I wrote the tests
and the fix.

**Maintainer Feedback:**

- 2026-10-23: none yet on the PR. On the issue, donk8r replied "Feel
  free to take it" and assigned me. Upstream CI has not run on the PR
  yet; it is a first-time contributor's fork PR.

**Status:** Awaiting review (opened 2026-10-23).

**Toolkit**

- Tool: the ship-gate, wrapping my own pr-precheck (now with the
  real-world scope) and my manifest, run through the manual doorway.
- What it produced: PASS on the branch, with all four rubric checks
  passing (plan-fidelity, tested, diff-reviewable, standards-met), and
  voice notes about my AI disclosure and one summarized test line.
- Difficulties: before the Unit 7 install, pr-precheck refused to grade
  octoweb because its scope was still the sandbox; and I edited two
  words of the description after the gate passed, so what I shipped was
  not exactly what it had read.
- What changed: I re-ran the gate on the description as shipped, and it
  passed again; the remaining voice note (say what I checked myself) is
  a wording change I can still make on the PR.

---

## Learnings and Reflections

**Technical Skills Gained**

- How an inverted index stays consistent: every place a document leaves
  `docs` has to undo what indexing added to the posting lists, or the
  index keeps dead ids that its own search quietly filters out.
- Rust ownership in practice: moving a value into a collection versus
  borrowing it, the borrow checker ending a borrow at its last use, and
  why a method taking `&mut self` cannot run while an iterator still
  borrows one of `self`'s fields.
- The standard collection APIs the fix needed: `BTreeMap::get_mut`,
  `entry`, `remove`, `Vec::retain`, `remove`, and `extract_if`, and that
  `extract_if` removes items only as it is iterated.
- Committing only some of a file's changes with `git add -p`: staging
  hunk by hunk (`y`, `n`, `s` to split) let me turn one mixed set of
  edits into a tests-only commit and a fix commit.
- Toolchain and platform details: replacing a broken Homebrew Rust with
  rustup, and that macOS 26 drops the quarantine agent name for non-app
  processes.

**Challenges Overcome**

- My first pick, sonic#394, was taken by a suspended contributor that no
  API could show me; asking in my claim surfaced it within hours, and I
  released it and re-picked.
- My first test assertions passed when they should have failed (a
  missing `!`), and my first fix removed every other page (`!=` copied
  from `retain` into `extract_if`). The maintainer's existing tests
  caught the second one at once.
- A failing test on `master` that I first blamed on Claude Code's
  sandbox; I proved that wrong by running it outside the sandbox and
  with a plain Swift script.
- Keeping the issue's rule that a human writes the fix: I asked for
  syntax on unrelated examples and wrote the tests and the fix myself.

**What I'd Do Differently Next Time**

- Read the whole thread and ask about any earlier attempt before
  building a reproduction; on sonic I built a timing harness before the
  maintainer told me the issue was taken.
- Do the course's ask-before-claiming step before posting the claim,
  not after.
- Check that an edit is saved, and run `git diff`, before every commit;
  my `terms` comment update was never in the file, and only a rubric
  read of the PR draft caught it.
- Re-run the gate after the last wording edit, not before it.

---

## Resources Used

- Muvon/octoweb#21, the issue itself: its Where, Fix, Tests, and Out of
  scope sections were the plan, and its closing paragraph set the rules
  (https://github.com/Muvon/octoweb/issues/21).
- octoweb's `.github/workflows/ci.yml`, for the exact test command CI
  runs and the macos-15 runner it runs on.
- The Rust standard library docs for `BTreeMap`
  (https://doc.rust-lang.org/std/collections/struct.BTreeMap.html) and
  `Vec::extract_if`
  (https://doc.rust-lang.org/std/vec/struct.Vec.html#method.extract_if).
