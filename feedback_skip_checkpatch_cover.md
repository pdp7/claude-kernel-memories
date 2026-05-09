---
name: Skip checkpatch on b4 cover letter commit
description: The cover letter commit at the start of a b4 series is a tracking artefact, not a real patch; checkpatch always reports false-positive errors against it (RST section underlines, b4-submit-tracking marker, # comment lines)
type: feedback
originSessionId: 56dffc97-8c75-430e-8ed2-7896f448b175
---
Don't run `checkpatch.pl` against the cover-letter commit at the start of a
b4-managed series. It is not a real patch; b4 stores cover-letter prose
plus the `--- b4-submit-tracking ---` JSON block in that commit's message
so the prep tooling can round-trip cover edits. The commit has 0 lines
of code diff, so checkpatch always emits noise:

- "Invalid commit separator" on every `---` and `------` underline used
  for RST sections (Series organisation, Key design decisions, Open
  issues, Changelog).
- "Invalid commit separator" on the literal `--- b4-submit-tracking ---`
  marker that b4 itself requires verbatim.
- "Commit log lines starting with '#' are dropped by git as comments" on
  any `# ` lines (e.g. quoted shell prompts in the cover, anchor
  lines).
- "Prefer a maximum 75 chars per line" on long sentences inside the
  changelog block where wrapping would change meaning.

**Why:** None of these would block `git am` or kernel acceptance because
the cover never lands in mainline; it only exists in the b4 branch state
and as the [PATCH 0/N] message on lkml. Treating its checkpatch noise as
a series gate produces false-negative anxiety and wastes time.

**How to apply:** When iterating "all commits in the series should
checkpatch --strict 0/0/0", scope the loop to the *code* commits only.
Sample skip:

```bash
COVER=$(git rev-parse "$(b4 prep --show-info | awk '/^start-commit:/{print $2}')")
for sha in $(git log --format=%H "$COVER..HEAD"); do
  git format-patch -1 --stdout "$sha" | scripts/checkpatch.pl --strict --no-tree -
done
```

The `start-commit` from `b4 prep --show-info` is the cover; iterate
`start-commit..HEAD` to skip it. Treat checkpatch noise on the cover as
out-of-scope and do not waste time investigating it.
