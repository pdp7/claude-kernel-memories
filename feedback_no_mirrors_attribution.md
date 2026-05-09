---
name: Don't say "Mirrors X" when describing code in this driver
description: Drop "Mirrors MPAM's foo()" / "Mirrors x86's bar" framing from comments and commit messages; experienced reviewers can draw the parallel themselves.
type: feedback
originSessionId: f3330f30-92b0-40a4-866f-b17fadc2d3ed
---
Don't introduce comments or commit messages with "Mirrors X" / "Same
as MPAM's foo()" / "Like x86's bar" where the role is purely to point
at sibling-driver code. Reviewers on resctrl are experienced enough
to draw the parallel from reading the code; the attribution adds
noise.

**Why:** Drew's stylistic preference. Resctrl reviewers (Reinette,
Tony, etc.) work across the x86, MPAM, and CBQRI implementations
and don't need the hand-holding pointer. Repeated "Mirrors MPAM's"
in particular reads as defensive cross-referencing rather than
explaining the actual reason for the code.

**How to apply:**
- If the only content is the attribution ("Mirrors mpam_foo()."),
  delete the sentence outright.
- If the comment also explains *why* the code does what it does,
  keep the substantive reason and drop the "Mirrors X" framing.
  Example: "Mirrors MPAM's pre-write dsb(ish)." -> just keep the
  preceding sentence about retaining the prior tag at the cache
  interconnect; drop the dsb attribution.
- Concept-naming usages like "AT-mirror" / `need_at_mirror` are
  fine, that's not the same as the comparative "Mirrors X" framing.
- "Mirrors the spec's binary layout" type usage on ACPI/DT structs
  is borderline; if the type is obviously laid out for the spec,
  drop it. If it's load-bearing (e.g. "byte-packed to match the
  spec's 20-byte size"), keep the substantive constraint and drop
  the "Mirrors" word.
