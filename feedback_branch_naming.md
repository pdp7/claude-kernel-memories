---
name: Branch naming for gitlab pushes
description: Tenstorrent gitlab branch naming preference — user prefix + SoC prefix
type: feedback
originSessionId: 22bd9803-f32c-458b-b123-fa3e3f888244
---
For branches that may be pushed to the Tenstorrent gitlab server, name
them `dfustini/<soc-prefix>-<topic>` (e.g. `dfustini/atl-cbqri-test`
for Atlantis CBQRI work).

**Why:** The user prefix scopes the branch to its owner; the SoC
prefix (`atl` for Atlantis, etc.) makes the target hardware obvious
to collaborators browsing the branch list. "ascalon" (the core
complex) is too generic — same Ascalon cores ship in multiple SoCs.

**How to apply:** When creating new branches that might be shared,
default to `dfustini/<soc>-<topic>` rather than topic-only names.
Worktree directory paths under `.worktrees/` don't need to match the
branch name — those are local-only.
