---
name: Reduce permission prompts
description: Batch commands to minimize permission checks, never ask permission for git log or read-only ops
type: feedback
originSessionId: 42d32e6b-f72f-4fb3-9c52-b20b3cedcbc0
---
Combine multiple commands into single Bash calls to reduce the number
of permission prompts. Never ask permission for read-only commands
like git log, git show, git diff, grep, cat.

**Why:** The user was repeatedly frustrated by permission prompts on
safe commands. Each prompt interrupts their workflow.

**How to apply:** Chain commands with && or ;. Use a single Bash call
for build+commit+squash sequences. Never split read-only git commands
into separate tool calls.
