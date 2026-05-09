---
name: Kernel log levels in QoS/CBQRI, debug prints OK at the right level
description: Keep DEBUG prints, but pick the right level. pr_err/pr_warn are default-visible and must be reserved for things users need to see. pr_info/pr_debug are opt-in verbose for investigation.
type: feedback
originSessionId: f3330f30-92b0-40a4-866f-b17fadc2d3ed
---
Keep development/DEBUG prints in QoS/CBQRI code, but pick the level
carefully. The level is the contract with the user about whether they'll
see the message in default dmesg.

- `pr_err`, default-visible. Reserve for genuine errors a user or admin
  needs to know about.
- `pr_warn`, default-visible. Use only when the user should reasonably
  act on something. Use carefully, this is shared dmesg space.
- `pr_info`, more verbose. Users can opt in (loglevel, dynamic debug,
  console settings) when investigating a problem.
- `pr_debug`, more verbose. Same idea, opt-in for investigation, also
  compiles out unless DEBUG is enabled.

**Why:** Drew first rejected stripping debug prints (useful during
development). He then clarified the issue is the level, not the print.
`pr_err` and `pr_warn` pollute default dmesg for every user. `pr_info`
and `pr_debug` are appropriate for verbose information a user opts in
to seeing when they're trying to investigate.

**How to apply:**
- When touching arch/riscv/kernel/qos*.c or drivers/resctrl/cbqri*
  files, leave existing debug prints in place but flag any at
  `pr_err`/`pr_warn` for downgrade. Do not strip the print itself
  unless explicitly asked.
- Routine internal state, dev-time tracing, init/teardown chatter,
  use `pr_debug` (or `pr_info` if it's the kind of thing a user might
  want at boot for "is the controller registered?").
- `pr_err` only for failure paths a user needs to see.
- `pr_warn` only when the user should act, e.g. a quirk requiring
  config change, a deprecated path.
