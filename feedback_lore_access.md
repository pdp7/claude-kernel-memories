---
name: Use b4 for lore.kernel.org
description: WebFetch fails on lore.kernel.org due to Anubis anti-bot - use b4 mbox instead
type: feedback
---

Do not use WebFetch for lore.kernel.org URLs - they are blocked by Anubis anti-bot challenge.

**Why:** lore.kernel.org uses Anubis JS challenge that blocks non-browser fetches.

**How to apply:** Use `b4 mbox <message-id>` to download the mbox file, then analyze it locally. The user corrected this approach directly.
