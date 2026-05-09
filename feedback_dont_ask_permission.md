---
name: Don't ask permission — just run everything
description: Never block on permission prompts for file edits, git ops, or shell commands inside ~/dev/linux. Push to remote is also OK.
type: feedback
originSessionId: b14380db-1073-428f-8927-e23a76b251bb
---
Don't ask for permission before doing git operations, file edits, or running shell commands inside ~/dev/linux. Just do it. Never pause to ask "should I proceed?" or "want me to tackle X?" — just execute. Pushing branches to remote is also fine.

**Why:** Drew makes checkpoint branches frequently and can always go back. Asking wastes time because he may not be watching the terminal — he switches tabs and doesn't know Claude is blocked waiting for input. He explicitly said "do not ask" and prefers Claude to finish autonomously. All files inside ~/dev/linux are OK to change. Do not modify files above ~/dev/linux.

**How to apply:** When refactoring a series, just execute the full plan without pausing for confirmation. Create backup branches as needed, but don't ask before proceeding with destructive-looking git operations like reset, rebase, or branch renaming. Run all needed commands directly. If something goes wrong, Drew can go back to his checkpoint branch.
