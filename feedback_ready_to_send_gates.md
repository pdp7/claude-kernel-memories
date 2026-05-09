---
name: Don't say a series is ready to send without running the gates
description: Before declaring a series ready to send to lore, run checkpatch --strict, sparse, and the build matrix
type: feedback
originSessionId: 7f1372ba-0843-4150-948f-53c1a64bac6c
---
**Never declare a series "ready to send" without running, at minimum:**

1. `./checkpatch-series.sh <N>` — `--strict` over the N commits in the series.
2. `make C=2 W=1 ARCH=riscv LLVM=1 -j16` — sparse over the kernel.
3. `./build-matrix.sh` — the 6-config build matrix (current, x86_64-defconfig, arm64-defconfig, rv64-defconfig, rv64-allnoconfig, rv64-randconfig). See `reference_build_matrix.md`.

**Why:** 0day-bot will run all of this within hours of posting. Catching issues locally first avoids public review noise and a needless v-bump. Cross-arch breakage on `fs/resctrl/`, `drivers/resctrl/`, `drivers/acpi/pptt.c`, or `include/linux/resctrl.h` is especially likely on x86_64-defconfig and arm64-defconfig and isn't visible from the riscv-only build. Series-touched warnings under W=1 are what 0day actually flags.

**How to apply:** When the user says "ready to send" / "send v4" / "let's mail this", do not skip the checks. Run them first, surface any series-touched errors or W=1 warnings, and only then proceed. If the series passes all three with zero series-touched diagnostics, say so explicitly in the readiness summary.
