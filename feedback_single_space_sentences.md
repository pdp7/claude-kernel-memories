---
name: Single space after sentences (kernel style)
description: Kernel style uses a single space after the end of a sentence, not double-space; applies to commit messages, comments, docs
type: feedback
originSessionId: 15e7df56-b30a-4a75-ada5-5f4df9cb07ae
---
Use a single space after the end of a sentence, never two. This applies to commit messages, code comments, kernel documentation (RST/.txt), and any prose written for the kernel tree.

**Why:** Linux kernel style convention. Drew flagged this directly.

**How to apply:** When writing or editing any text in the kernel tree (commit subjects, commit bodies, comments, Documentation/, cover letters, patch descriptions), use exactly one space between sentences. Watch for accidental double-spaces when joining sentences or reflowing text.
