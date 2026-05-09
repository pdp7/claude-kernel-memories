---
name: Kernel patch series cover letter style
description: Validated structural and writing patterns for the cover letter body of a multi-version RFC/PATCH series
type: feedback
originSessionId: 4cada5d7-20fe-48d1-8921-22e55b6d6cd0
---
When writing or improving the cover letter of a kernel patch series,
apply these patterns (all were approved by the user during the v4
ssqosid-cbqri-rqsc cover rewrite):

1. **Opening paragraph leads with user-facing value, not specs.**
   State what the series enables and compare to peer architectures
   when relevant (e.g., "parity with x86 RDT / Arm MPAM"), then
   introduce the layers/components as a bulleted list. Don't front-load
   the reader with four acronyms and four footnote references.

2. **"Series organization" section flags the review center-of-gravity.**
   Call out which patches contain the main logic so reviewers know
   where to spend their time. Surface external dependencies on the
   patches they affect (e.g., ACPICA dependency flagged inline on the
   DO NOT MERGE commit).

3. **Changelog grouped by theme with sub-headers, not a flat bullet list.**
   Use "New feature:" and "Bug fix:" sub-headers (or similar). Each
   item has a what/where/why arc. Bug fixes follow symptom → root
   cause → fix:
       - Symptom: what the hardware/kernel did wrong
       - Root cause: why the old code triggered it
       - Fix: what v_N now does differently
   Include Reported-by: on the sub-header line, not inside prose.

4. **Function/register names stay** — they help reviewers grep the
   code — but anchor them to patch numbers so reviewers know where
   to look.

5. **"Open issues" states follow-up plan** — don't just list
   limitations. Reviewers want to know if a gap is permanent or
   planned. Fix typos.

6. **Keep older changelogs verbatim** — don't rewrite v2/v3 entries
   when adding v4. Reviewers may cross-reference.

**Why:** Kernel reviewers scan dozens of series per week. A cover
letter that front-loads value, flags review weight, and presents
changelogs with clear what/where/why arcs reduces the cognitive load
of figuring out "what should I actually look at here."

**How to apply:** When the user asks to improve a cover letter or
rewrite a changelog, audit for these six patterns. Propose targeted
edits (opening para, series org, changelog, open issues) rather than
a wholesale rewrite. Leave body sections that are already clear
(technical background, example hardware, reproduction steps) alone.
