---
name: OpenSBI archive is for general awareness, not series filtering
description: User reads opensbi-mail-archive to stay informed on OpenSBI development broadly, not to find content relevant to the current kernel series; output target is NotebookLM audio digests
type: feedback
originSessionId: c5a707a2-04ef-43ed-8262-1cb8676ed3d6
---
When the user says "read ~/opensbi-mail-archive" or asks about OpenSBI list traffic, do not filter to the current QoS/CBQRI/RQSC series. The user wants broad situational awareness of OpenSBI development.

**Why:** User feeds digests into NotebookLM to generate audio "deep dive" podcasts they listen to while walking dogs. NotebookLM works best with rich, comprehensive source material covering many threads, debates, and technical details, not narrow filters.

**How to apply:** When asked to summarize the OpenSBI archive (or similar mailing-list archives queued for NotebookLM), write a comprehensive digest organized by topic/series with author, revision state, technical scope, and unresolved review threads. Aim for 2000–5000 words of substantive markdown. Save the output as a file the user can paste into NotebookLM. Do not narrow to current-branch relevance unless explicitly asked.
