---
name: No refactor commits in a series
description: Patch series structure should reflect logical fit for a fresh reviewer, not the development timeline — fold any refactor/rework of same-series code back into the patches that introduced it
type: feedback
originSessionId: e6112329-eda9-4740-aeaa-12cb3193d938
---
No refactor commits in a series.  When a series introduces code and a
later commit refactors that code, the refactor changes must be folded
into wherever they logically fit in the series.  The timeline of
development and refactoring has no bearing on how the series is
structured - only the logical fit for a fresh person reviewing matters.

A series should never introduce code in one patch just to clean it up
in a later patch.  Removing lines from an earlier patch in the series
should only happen when truly required to break up the work into
logical pieces for review.  Cleanup-on-top commits are an artifact of
the dev timeline of this revision and should never exist; they belong
folded into the logical patch.

**Why:** A patch series is read by reviewers and maintainers who never
saw the development order.  They see N commits as a story: each commit
should make sense on its own, build on the prior ones, and contribute
something the next commits don't immediately undo.  A "reorganize" or
"refactor" commit that only touches code introduced earlier in the same
series means the earlier patches should have been written that way from
the start; reviewers waste effort on a shape that later patches throw
away, and the diff stats misrepresent what each patch contributes.
This is stricter than "no churn": even a pure restructure with no
add/remove churn doesn't belong as its own commit in the series.

**How to apply:** Treat the series as if reviewing it fresh, with no
knowledge of how it was developed.  Ask: "if I were writing this from
scratch today, would this commit exist?"  If the answer is no — because
its hunks belong in earlier patches — split and fold them into those
patches.  Triggers to watch for in commit subjects: "reorganize",
"restructure", "rework", "refactor as <style>", "split <function> into
phases", "convert to <pattern>".  If such a commit already exists,
suggest folding before sending the series.  Refactors of pre-existing
upstream code are fine; this rule applies only to code added by the
same series.
