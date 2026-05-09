---
name: Build with LLVM not GCC
description: Always use LLVM/clang to compile the kernel, never GCC, with W=1
type: feedback
originSessionId: 7f1372ba-0843-4150-948f-53c1a64bac6c
---
Always compile the Linux kernel with LLVM, not GCC. Use W=1 for extra warnings.

**Why:** Drew's preferred toolchain is clang/LLVM. The build script is `llvm.sh` which runs `make W=1 ARCH=riscv LLVM=1 -j16`.

**How to apply:** Use `make W=1 ARCH=riscv LLVM=1 -j16` or just run `./llvm.sh`. Do not use `CROSS_COMPILE=riscv64-linux-gnu-` — LLVM handles cross-compilation via `ARCH=riscv` alone.

**Avoid W=12/W=2 + blanket -Werror.** W=1 is what 0day-bot actually checks; W=2 surfaces a long tail of pre-existing `-Wshadow` and `-Wunused-macros` in upstream code (rseq_entry.h, x86/events/core.c, kernel/sched/core.c, etc.) — and `KCFLAGS="-Werror"` on top fails every build at scripts/mod with noise that has nothing to do with the series. The kernel's own curated `-Werror=foo` flags (`implicit-function-declaration`, `return-type`, `incompatible-pointer-types`, …) already fire at W=0; don't override them.
