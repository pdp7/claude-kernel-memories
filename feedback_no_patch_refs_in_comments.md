---
name: No patch references in code comments
description: Comments in committed code should never refer to "this patch", "a later patch", "next commit", etc. The file at HEAD is already complete and self-contained, so patch-relative references are nonsense to anyone reading the source.
type: feedback
originSessionId: f3330f30-92b0-40a4-866f-b17fadc2d3ed
---
Code comments must not reference patches, commits, or relative
position in a series. Phrases like "in a later patch", "this patch
adds", "next commit", "a follow-on patch", "stubbed until patch N"
all break when the reader looks at the file at HEAD, because there
is no "later patch" anymore, the code is already there. Patch-talk
belongs in commit messages and cover letters, not in source files.

**Why:** Drew's stylistic preference, consistent with kernel review
guidance. A patch is a development artifact, the file in tree is the
finished product. A comment saying "implemented in a later patch"
makes sense only to someone reading the patch as it was being
applied, never to someone reading the file later.

**How to apply:**
- Commit messages and cover letters: free to use "this patch", "a
  later patch", "follow-up", "introduces X", etc. That's the natural
  voice for that audience.
- Code comments: describe the code in terms of what it is and what
  it does today. If a function is a stub, say "stub" or "no-op", not
  "stub until the X patch lands". If behavior depends on something
  added elsewhere, name the something (function, file, feature),
  not the patch that added it.
- When reviewing or writing, scan for: "this patch", "later patch",
  "next patch", "follow-on patch", "this commit", "earlier commit",
  "a previous commit", "in series", "lands in patch N". All of these
  are red flags in source files.
