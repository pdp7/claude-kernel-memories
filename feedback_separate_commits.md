---
name: Separate commits per change
description: User prefers one commit per logical change, not bundled commits
type: feedback
---

Make a separate commit for each logical change rather than combining multiple fixes into one commit.

**Why:** Drew corrected this when I tried to bundle 4 fixes into a single commit.

**How to apply:** When committing multiple fixes, stash all changes, apply each individually, and commit separately. Don't try to use `git add -p` with stdin — it doesn't work well. Instead: stash, apply edits one at a time, commit each.
