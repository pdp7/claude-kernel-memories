---
name: Terse commits/comments for experienced kernel devs
description: Kernel commit messages and code comments target experienced kernel developers; don't narrate code or explain basics, terse beats verbose
type: feedback
originSessionId: 15e7df56-b30a-4a75-ada5-5f4df9cb07ae
---
The audience for kernel commit messages and code comments is experienced kernel developers, not beginners. Do not write commentary that explains common kernel concepts, restates what the code obviously does, or walks through the diff. Terse is better than verbose.

**Why:** Reviewers and future readers are subsystem maintainers and kernel devs who already know the framework, idioms, and standard patterns. Beginner-level narration dilutes the signal, lengthens patches, and reads as patronizing or padded.

**How to apply:**
- Commit messages: state *what changes* and *why*. No preambles teaching how the subsystem works. No diff walkthroughs ("This patch adds X. First it does Y. Then it does Z."). No "this is needed because we want to..."  fluff; just state the constraint or motivation.
- Code comments: only when the *why* is non-obvious (hardware quirk, spec corner case, hidden invariant, surprising ordering). No comments restating function names, no "// initialize the lock", no docstrings explaining standard plumbing.
- Cut anything an experienced kernel dev would skim past.
- One short line beats a paragraph; deletion beats one short line if the code is self-evident.
- Applies across all subsystems, not just resctrl/CBQRI.
