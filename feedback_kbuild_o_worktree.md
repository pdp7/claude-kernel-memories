---
name: Kbuild O= needs a clean srctree
description: When kbuild errors with "source tree is not clean", use a linked git worktree as srctree, not mrproper
type: feedback
originSessionId: 7f1372ba-0843-4150-948f-53c1a64bac6c
---
If `make O=<dir>` fails with "*** The source tree is not clean, please run 'make ARCH=foo mrproper'", do **not** run `mrproper` in the main tree — it nukes the user's `.config` and existing build state.

**Why:** kbuild rejects `O=` when the srctree has any in-tree build artifacts (vmlinux, *.o, generated headers). Drew typically has an in-tree build alongside development; `mrproper` would destroy his current build context and `.config`.

**How to apply:** Create a linked git worktree at `.worktrees/<name>/` from HEAD and use it as srctree. Pattern:
```
git worktree add --detach .worktrees/foo HEAD
make -C .worktrees/foo O=$PWD/build-out/<name> ARCH=<arch> LLVM=1 ...
```
The worktree is freshly checked out (no build artifacts) so kbuild accepts it; the O= dir lives where you want, and the main tree is untouched. `.worktrees/` is already gitignored. `build-matrix.sh` does this automatically.
