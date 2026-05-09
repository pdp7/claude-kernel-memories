---
name: Build matrix runs sequentially
description: When running multiple kernel builds (matrix), run them sequentially with -j16, not in parallel
type: feedback
originSessionId: 7f1372ba-0843-4150-948f-53c1a64bac6c
---
When running a build matrix (multiple configs/arches), run the builds **sequentially** with `-j16` each — do not run them in parallel.

**Why:** Drew's build server has 16 CPUs. Sequential `-j16` per build gives each build the full machine with no oversubscription; parallel builds would compete for CPU/RAM and not finish faster.

**How to apply:** A build-matrix script should loop over configs and run each `make` to completion before starting the next. Logs go to per-build files; user can `tail -f` the currently active log. This was first applied to `build-matrix.sh` for the v4 QoS series (rv64-current, rv64-defconfig, rv64-allnoconfig, rv64-randconfig, x86_64-defconfig, arm64-defconfig).
