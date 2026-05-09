---
name: Use git worktrees for branch work
description: Always use git worktrees instead of switching branches in the main working directory
type: feedback
originSessionId: e1955cba-8b57-4bdc-99e2-7dcdfda45083
---
Use `git worktree add .worktrees/<name> <branch>` for all branch work. Do not
switch branches in the main working directory.

**Why:** The main working directory has many untracked in-progress files; branch
switching risks conflicts and confusion. Worktrees keep each branch isolated.

**How to apply:**
- `.worktrees/` is already gitignored in this repo — safe to use immediately
- Build in the worktree: `make W=1 ARCH=riscv LLVM=1 -C .worktrees/<name> <target>`
- The `llvm.sh` script only exists in the main tree; use the explicit make flags
  in worktrees
