# Contribution: Document `ignore_class` in cross-entropy loss class docstrings

**Contribution Number:** [2]
**Student:** Hoang Nguyen Bich Ngoc
**GitHub Username:** zuccamia
**Issue:** https://github.com/keras-team/keras/issues/24169
**Pull Request:** https://github.com/keras-team/keras/pull/24175

<!--
Extra contribution, copied from extra-round-template.md. Extra
contributions are never graded; contribution-story.md is the graded one.
All times are UTC.
-->

---

## Toolkit

This was a short extra round (issue to merge in three days), so it has
no weekly tool entries. The Toolkit blocks below record what each phase
used.

---

## Why I Chose This Issue

I found the gap when I needed to look up how to use the cross-entropy
loss class, and could only find `ignore_class` described in the source
code.

**Toolkit**
- Tool: the rendered TensorFlow API docs for `SparseCategoricalCrossentropy`,
  then the Keras source.
- What it produced: the `ignore_class` description, found only in the
  functional `sparse_categorical_crossentropy` docstring.
- Difficulties: the rendered docs for the class did not describe
  `ignore_class` at all.
- What changed: I opened #24169 to add it to the class docstring.

---

## Understanding the Issue

### Problem Description

The `SparseCategoricalCrossentropy` loss class accepts `ignore_class`,
but its class docstring did not describe it, so the rendered API docs
left it out. Users had to read the source to learn what it does.

### Expected Behavior

The class docstring's `Args` section describes `ignore_class`, the same
way the functional `sparse_categorical_crossentropy` docstring does.

### Current Behavior

`ignore_class` is in the class's `__init__` signature, and only the
functional variant's docstring describes it. It was added to both in
[91435bf](https://github.com/keras-team/keras/commit/91435bffd38e8a74b9eb46a438fe60f551928754),
but only the function's docstring got the description.

### Affected Components

`keras/src/losses/losses.py`: the `SparseCategoricalCrossentropy`
class docstring.

**Toolkit**
- Tool: the GitHub web UI, to read `losses.py` and commit 91435bf.
- What it produced: where `ignore_class` was added, and that only the
  functional docstring got its description.
- Difficulties: none met.
- What changed: nothing.

---

## Reproduction Process

### Environment Setup

None needed for a docstring change. The triager reproduced it in Colab
with Keras 3.15.1 on the TensorFlow backend.

### Steps to Reproduce

1. `inspect.signature(keras.losses.SparseCategoricalCrossentropy.__init__)`
   lists `ignore_class=None`.
2. `"ignore_class" in keras.losses.SparseCategoricalCrossentropy.__doc__`
   is `False`.
3. `"ignore_class" in keras.losses.sparse_categorical_crossentropy.__doc__`
   is `True`.

### Reproduction Evidence

- **Commit showing reproduction:** none (docs-only change).
- **Screenshots/logs:** pandeyshub-byte's repro
  [gist](https://colab.sandbox.google.com/gist/pandeyshub-byte/8abd55709baef6843f3101bf7a6031cc/-24169.ipynb)
  ends with an `assert` that prints "REPRODUCED: `ignore_class` is a
  valid argument of keras.losses.SparseCategoricalCrossentropy but is
  missing from its docstring."
- **My findings:** the gist also shows how `ignore_class` works: with
  `ignore_class=-1`, the ignored sample's loss is `0.0`, and the reduced
  loss (1.1769) averages only the two non-ignored samples.

**Toolkit**
- Tool: pandeyshub-byte's Colab gist.
- What it produced: the triager's confirmation, and an example of how
  `ignore_class` changes the loss.
- Difficulties: none met.
- What changed: nothing.

---

## Solution Approach

### Analysis

When `ignore_class` was added in 91435bf, the description went into the
functional `sparse_categorical_crossentropy` docstring only, not the
class docstring.

### Proposed Solution

Reuse the functional docstring's description in the class docstring's
`Args` section, bringing the two into parity. No code changes.

### Implementation Plan

1. Add the `ignore_class` description to the `SparseCategoricalCrossentropy`
   class docstring in `keras/src/losses/losses.py`, before `reduction`.

**Toolkit**
- Tool: the GitHub web UI's file editor, since it was a small docs change.
- What it produced: the 5-line docstring edit, committed as `3c37f93`.
- Difficulties: none met.
- What changed: nothing.

---

## Testing Strategy

The gist's check flips once the docstring has the description:
`"ignore_class" in keras.losses.SparseCategoricalCrossentropy.__doc__`
becomes `True`. I checked this myself in GitHub Codespaces: I built
Keras from `master` and from my fix branch, and compared the class's
`inspect.signature` and `__doc__` in each. On the PR, Keras CI passed:
the code format check and the CPU tests on jax, jax (NNX), numpy, and
tensorflow.

**Toolkit**
- Tool: GitHub Codespaces, on `master` and on my fix branch.
- What it produced: a before/after check of the class docstring with
  `inspect` and `__doc__`.
- Difficulties: none met.
- What changed: nothing.

---

## Implementation Notes

### 2026-11-07

Opened #24169 with the missing parameter, the commit that added it,
the proposed fix (reuse the functional description), and a request to
self-assign.

### 2026-11-09

pandeyshub-byte labeled it `type:docs-bug`, reproduced it with a Colab
gist, and asked me to raise the PR. I replied with thanks and said I
would open the PR.

Opened #24175 (one commit, `3c37f93`), adding 5 lines to the class
docstring. An `approved-issue` check failed twice and moved the PR to
draft, because #24169 was assigned to pandeyshub-byte, not me. The
`cla/google` check passed. hertschuh approved it ("Thanks!") and
merged it at 19:47, as `e8e9f3f`, about three hours after I opened it.

**Toolkit**
- Tool: Claude Code, to improve the wording of my issue reply and PR
  description as a non-native English speaker, and to look up the CLA
  step in Keras's CONTRIBUTING.md.
- What it produced: suggested edits to my reply and PR description, and
  the `cla/google` check details.
- Difficulties: none met.
- What changed: added `Fixes #24169` and a line on where the wording
  came from to the PR description.

---

## Pull Request

**PR Link:** https://github.com/keras-team/keras/pull/24175

**PR Description:** Adds the `ignore_class` description to the
`SparseCategoricalCrossentropy` class docstring, notes it has been a
valid argument since 91435bf while only the functional docstring
described it, says the text matches the function's docstring, and
links `Fixes #24169`.

**Maintainer Feedback:**

- 2026-11-09: gemini-code-assist left an automated review comment.
- 2026-11-09: hertschuh approved ("Thanks!") and merged.

**Status:** Merged (2026-11-09, merge commit `e8e9f3f`; #24169 closed as completed).

**Toolkit**
- Tool: the GitHub web UI, to commit and open the PR.
- What it produced: PR #24175 with one commit.
- Difficulties: the `approved-issue` check failed because the issue was
  assigned to the triager, not me; it did not block the merge.
- What changed: nothing.

---

## Learnings and Reflections

### Technical Skills Gained

- Checking a docs gap in code: `inspect.signature` for the arguments,
  `__doc__` for the docstring, and an `assert` as a pass/fail repro.
- Building a project from two branches in GitHub Codespaces to compare
  a change before and after.

### Challenges Overcome

- The `approved-issue` check failed and moved the PR to draft twice,
  because #24169 was assigned to the triager, not me. The maintainer
  approved and merged it anyway.

### What I'd Do Differently Next Time

- Ask to be assigned on the issue before opening the PR, so the
  `approved-issue` check passes.

---

## Resources Used

- keras-team/keras#24169, the issue
  (https://github.com/keras-team/keras/issues/24169).
- The rendered `SparseCategoricalCrossentropy` API docs
  (https://www.tensorflow.org/api_docs/python/tf/keras/losses/SparseCategoricalCrossentropy?version=stable).
- Keras's CONTRIBUTING.md: the CLA step and the class docstring rules.
- pandeyshub-byte's repro gist, linked above.
