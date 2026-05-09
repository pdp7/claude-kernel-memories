---
name: ORC mode mbox workaround
description: create_changes.py fails to extract commit metadata from mbox files; manually populate commit-message.json
type: feedback
---

When running /korcreview on mbox patch files, create_changes.py leaves commit-message.json fields empty (sha, author, subject, body all blank). The diff content and FILE-N-CHANGE-M.json files are extracted correctly.

**Why:** The mbox format includes email headers that the script doesn't fully parse for commit metadata fields.

**How to apply:** After Phase 1 context creation with an mbox input, check commit-message.json. If fields are empty, manually populate them from the mbox file contents before launching Phase 2 agents. This ensures lore, fixes, and report agents have proper context.
