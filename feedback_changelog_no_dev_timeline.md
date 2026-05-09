---
name: Cover letter changelog describes v(N) state, not v(N) development
description: Cover letter "Changes in vN" bullets must describe what is different in vN versus v(N-1), as a reviewer of vN would see it - never the dev timeline of vN itself.
type: feedback
originSessionId: 749d56cb-2bf9-4a05-bc8f-9fb3709aff3f
---
Cover letter "Changes in vN:" bullets describe what changed at the v(N) vs v(N-1) level, from the perspective of a fresh reviewer reading v(N).  They never describe the internal development timeline of v(N) (which fix landed in which iteration, what was folded where, what was amended).

**Why:** A reviewer who reads v(N) does not see v(N)-development history; for them, every patch in v(N) is just "the patch", and the "folded into X" annotation is noise that does not exist in their world.  Saying it leaks dev process into the public record.

**How to apply:**
- Never write "Folded into the X patch", "amend the commit message", "rework of v3 was rolled into …", "this fix lives in patch Y", or any phrase that refers to where a change was placed during the cycle.
- Never describe commit-message-only edits as a separate item ("amend X commit message: …").  If the message changed, the v(N) commit just says the new thing; reviewers never knew the old wording.
- Bullet shape: state the change as a fact about v(N).  Give the WHY (what the failure was, what spec rule applies, what reviewer asked for it) but not the WHEN.
- For pure documentation/comment edits that have a real semantic effect (e.g. clarified UAPI contract, updated kdoc that fixes a documented invariant), describe the doc state in v(N) - "kdoc now documents the fourth -ENOENT case" rather than "amend kdoc to add the case".
