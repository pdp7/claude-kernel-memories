---
name: resctrl_enable_mon_event must precede online_mon_domain
description: Critical ordering requirement - enable monitoring event before onlining monitoring domains to avoid NULL deref
type: feedback
originSessionId: b2da4f0c-2f25-419f-ad9b-8547f283d45a
---
`resctrl_enable_mon_event()` must be called BEFORE `resctrl_online_mon_domain()`.

**Why:** `domain_setup_l3_mon_state()` inside `resctrl_online_mon_domain()` only
allocates `rmid_busy_llc` if `resctrl_is_mon_event_enabled()` returns true. If the
event isn't enabled yet, the bitmap stays NULL, causing a NULL pointer deref in
`free_rmid()` -> `find_first_bit(d->rmid_busy_llc, ...)` when rmdir-ing a control group.

**How to apply:** When adding monitoring support for a new resource, call
`resctrl_enable_mon_event()` during resource init (e.g. `qos_init_cache_resource()`)
before any monitoring domains are onlined, not after the controller loop.
