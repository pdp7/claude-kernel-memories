---
name: Splitting a large kernel commit into a series
description: Refactor approach validated when splitting CBQRI driver (e98f63b73824, 2690 lines) into 5 feature-axis patches
type: feedback
originSessionId: d92ab308-473e-49a2-b5d9-57e308527149
---
When a maintainer asks to split a large patch into smaller patches for review,
this approach worked and the user endorsed it.

**Why:** Reinette asked for smaller CBQRI patches (2026-04-28).  The 4-patch
HW-vs-resctrl × cap-vs-bw split I proposed first would have required
`__maybe_unused` to keep W=1 clean (HW funcs dead until later patches wired
them up).  The user explicitly rejected that and asked instead for feature-axis
split with alloc-vs-mon sub-split, no `__maybe_unused`, W=1 clean per patch.

**How to apply** (next time a series needs splitting):

1.  **Split along feature axis, not implementation layer.** "cache support"
    and "bandwidth support" each as feature; HW + resctrl wiring stay
    together so each patch is self-contained.  If feature is large,
    sub-split on alloc-vs-mon (operations users perceive as distinct).

2.  **Add a scaffolding patch as P1** when stub-only resctrl_arch_*
    callbacks would otherwise force `__maybe_unused`.  The scaffolding
    patch carries: headers, Kconfig, Makefile, MAINTAINERS, all
    resctrl_arch_* with safe-default stubs, late_initcall, registration
    API.  Subsequent feature patches *replace* stub bodies (not extend
    via `__maybe_unused`).

3.  **Reorder commits when it fixes a hidden bisection break.**  CBQRI
    series originally had `8e6d8256bc7f ACPI: PPTT helper` *after* the
    driver patch that referenced the helper.  Moved it before; user
    accepted without comment.  Don't be precious about commit order if
    a reorder makes the series bisectable.

4.  **Verify W=1 clean per patch via targeted build:**
        touch drivers/resctrl/cbqri_resctrl.c
        make W=1 ARCH=riscv LLVM=1 -j16 drivers/resctrl/cbqri_resctrl.o 2>&1 \
          | grep -cE "warning|error"

    Should print 0 for each commit boundary.  Loop with
    `git checkout $commit -- <files>; touch; build; checkout HEAD --`.

5.  **Mechanics:**  Use a worktree (`.worktrees/<name>`) so the b4 prep
    branch stays untouched.  `git reset --hard <target>~`, cherry-pick
    any commits you're reordering, then build forward: write the P1 file
    fresh, Edit-tool incrementally to P2/P3/..., commit each, finally
    cherry-pick remaining downstream commits.

6.  **Cover letter and b4 tracking-marker stay on the cover commit**
    (786ddb31a3bf in this series).  Update its body to reflect the new
    series organisation before sending.

7.  **Watch for sequencing bugs in your own work:** I committed P1 with
    one of two W=1 fixes missing (had `git add` before the second Edit).
    Caught later by per-commit verification, fixed via `git rebase -i`
    edit + amend.  Always re-`git add` after every Edit before commit.
