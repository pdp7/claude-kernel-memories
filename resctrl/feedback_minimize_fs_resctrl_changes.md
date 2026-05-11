---
name: Minimize fs/resctrl changes in cbqri series
description: Don't modify fs/resctrl code in the RISC-V CBQRI/Ssqosid series unless required for cbqri to function; quality-of-life cleanups belong in a separate fs/resctrl series
type: feedback
originSessionId: 56dffc97-8c75-430e-8ed2-7896f448b175
---
Don't modify `fs/resctrl/` code in the RISC-V QoS / CBQRI / Ssqosid
patch series unless the change is necessary for the series to function.
Quality-of-life cleanups for fs/resctrl (diagnostic wording, helper
refactors, commit-message-only nits) should land in a separate series
owned by the fs/resctrl maintainers.

**Why:** The cbqri series already touches fs/resctrl in patch
"fs/resctrl: add RDT_RESOURCE_MB_MIN and RDT_RESOURCE_MB_WGHT"
(necessary, since CBQRI bandwidth semantics need new RDT_RESOURCE_*
values).  Anything beyond that increases reviewer surface area for
the wrong audience: fs/resctrl reviewers don't want to context-switch
into RISC-V semantics to evaluate a pseudo-lock diagnostic string,
and RISC-V reviewers can't usefully sign off on fs/resctrl restructuring.
Sashiko-style suggestions about `fflags_from_resource()` or
diagnostic strings should be sent to fs/resctrl as their own series.

**How to apply:** When a reviewer (Sashiko, Reinette, others) flags a
quality-of-life item in `fs/resctrl/`, classify it:
- **Required** for cbqri to function (e.g. the MB_MIN/MB_WGHT enum
  additions, the `default_to_min` field): include in the cbqri series.
- **Improvement** that fs/resctrl could ship independently: skip in
  the cbqri series, note in memory as "closed without action", and
  optionally suggest the reviewer raise it as a separate fs/resctrl
  cleanup series.
