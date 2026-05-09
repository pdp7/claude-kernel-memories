---
name: resctrl_arch_get_num_closid called for unpicked rids
description: fs/resctrl calls resctrl_arch_get_num_closid() on resources the arch driver doesn't back; the callback must tolerate that
type: feedback
originSessionId: edefd009-4f27-401c-8677-defdd3d43783
---
`resctrl_arch_get_num_closid(struct rdt_resource *r)` is invoked by fs/resctrl
on resctrl-defined rids that the arch driver may not back. Specifically,
`set_mba_sc()` in `fs/resctrl/rdtgroup.c:2542` calls it on
`RDT_RESOURCE_MBA` during every mount and unmount, even when the arch driver
exposes MB_MIN/MB_PROP instead of MBA.

**Why:** I removed CBQRI's `cbqri_res->max_rcid` snapshot field (a per-resource
cache of `ctrl->rcid_count`) and substituted `hw_res->ctrl->rcid_count`. For
the unpicked MBA rid, `hw_res->ctrl == NULL`, so the dereference NULL-faulted
during `umount /sys/fs/resctrl` -> `rdt_kill_sb` -> `set_mba_sc(false)`. The
snapshot had silently masked this with a zero-init `max_rcid = 0`.

**How to apply:**
- An arch driver's `resctrl_arch_get_num_closid()` MUST handle resources it
  did not register (return 0 for unpicked rids).
- More generally: any resctrl_arch_* callback that takes a `struct
  rdt_resource *` and is callable from generic mount/unmount/init paths can
  be invoked for rids the driver doesn't back. Check before dereferencing
  driver-private state.
- MPAM avoids this entirely by returning a system-wide `mpam_partid_max + 1`
  with no per-resource lookup. CBQRI cannot do that because rcid_count is
  per-controller.
- Other CBQRI readers of `hw_res->ctrl->rcid_count`
  (`resctrl_arch_reset_all_ctrls`, `qos_init_domain_ctrlval`) are safe
  because they only walk `r->ctrl_domains`, which is empty for unpicked
  rids.
