---
name: No em-dash, no hyphen or semicolon mid-sentence
description: In any output (chat, code comments, commit messages, docs), never em-dash, and don't use ` - ` or `;` mid-sentence; use `,` or break into smaller sentences.
type: feedback
originSessionId: 749d56cb-2bf9-4a05-bc8f-9fb3709aff3f
---
Never emit em-dash (`—`, U+2014) in any output. Also do not use `-` or `;` as
mid-sentence punctuation. Use a comma, or break the thought into smaller
sentences.

**Why:** Drew's stylistic preference, consistent with his lkml voice
(see user_drew_writing_style.md). Em-dash also renders inconsistently
across email clients in kernel-development workflow. Drew explicitly
clarified that `-` and `;` are also off-limits mid-sentence; he uses
`,` or just shorter sentences.

**How to apply:**
- Where you'd reach for an em-dash, en-dash, ` - `, or `;` in prose, use:
  - `,` for a soft pause
  - or a period (start a new sentence) when the pause is heavier
- ASCII hyphens are still fine inside compound adjectives (`mon-capable`,
  `late-initcall`) and in code identifiers; the ban is on dash-as-prose-pause.
- Applies to chat replies, code comments, commit messages, cover letters,
  documentation, file contents, anything Drew will read.
- Do not write `--` either.
