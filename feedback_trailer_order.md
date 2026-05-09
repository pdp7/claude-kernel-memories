---
name: Trailer order: Assisted-by before Signed-off-by
description: In commit messages, Assisted-by: trailers must come before Signed-off-by: trailers
type: feedback
originSessionId: 75ef4f95-8e04-4c35-81f5-6265a3435bf4
---
`Assisted-by:` trailers go before `Signed-off-by:` trailers in commit messages.

**Why:** Signed-off-by is the author's attestation and conventionally comes last in the trailer block; assistance/co-authorship credits belong above it.

**How to apply:** When composing or editing commit messages that include both `Assisted-by:` (e.g. Claude attribution) and `Signed-off-by:`, place all `Assisted-by:` lines first, then `Signed-off-by:` last. Same applies to `Co-developed-by:` style trailers paired with their SoBs.
