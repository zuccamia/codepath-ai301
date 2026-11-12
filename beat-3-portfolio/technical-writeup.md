# Fixing Leftover Search Entries in Octoweb

2026年11月11日 · @Nguyen Bich Ngoc Hoang

## Overview

I fixed a bug in Octoweb's page search index, and the maintainer merged it as pull request #27. Octoweb is a mouse-free browser built for AI agents: instead of clicking around, an agent reads and searches pages through text.

The bug: when a page was revisited or dropped from the browser, its old search entries stayed behind. Over time, the index filled up with entries that no longer matched any page.

The fix: clear out a page's old entries when it is revisited or dropped, so each page has only its current entries and the index stays lean.

## What happened

I found the issue, wrote the tests and the fix myself, and used AI tools I built in @codepath's AI301 course to get oriented and check my work.

1. **Finding the issue.** My tools help me find open-source projects and issues that match my interests. Octoweb's index is a small inverted index in Rust. At my previous job I moved our search from MySQL to Elasticsearch, so I had used inverted indexes from the outside, and this issue let me work on one from the inside.
2. **Understanding the code.** I kept a memory file of Octoweb's codebase to get oriented in unfamiliar code. The bug lived in `src/page_index.rs`, where `terms` maps every word to the ids of the pages that contain it. A page was removed when revisited or when the index hit its 2,000-page cap, but its id stayed in `terms`. Search skipped those ids, so results still looked correct.
3. **Writing the tests.** Before changing anything, I wrote tests that show the bug: visit a page, revisit or drop it, then check that its old entries are gone. Both tests (`reindex_or_cap_prunes_old_terms` and `shared_terms_preserved_for_live_page_after_reindex_and_cap`) failed before the fix: for example, a shared word still listed the old page's id (`[1, 2]` instead of `[2]`).
4. **Writing the fix.** When a page is revisited or dropped, the fix now removes that page's old entries from the index. The new tests pass. I added a private `unindex` helper that removes the page's id from each of its words and deletes words no page uses anymore. My first version flipped a condition and removed every other page, but the existing tests caught it right away.
5. **Checking before submitting.** My ship-gate check ran on the change and passed before I opened PR #27.
6. **Getting it merged.** The maintainer reviewed and merged the pull request. There were no review comments, just "Thanks for PR! All good" and a merge on 2026-10-25.

## Limitations and what's next

The fix covers pages that are revisited or dropped. A few open questions are worth noting:

- A page removed from browser history can still be found by its content. This is a separate bug I reported on the issue, with no reply yet.
- A revisit now processes the page twice, once to remove old entries and once to add new ones. I did not measure the cost.

What's next: open a new issue for the history bug, so it can be fixed on its own. The full change is in pull request #27.
