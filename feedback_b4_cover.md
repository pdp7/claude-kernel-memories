---
name: Use b4 prep --edit-cover for cover letters
description: Cover letter lives in b4's cover commit, update it via b4 prep --edit-cover, not manual git commit edits
type: feedback
originSessionId: 32225a4b-4e88-4e93-a972-c644b476a15c
---
Use `b4 prep --edit-cover` to update the cover letter. b4 manages the cover letter commit and will update it automatically. Do not manually amend the cover letter commit or edit standalone cover letter files expecting them to be picked up.

**Why:** b4 owns the cover letter lifecycle. Manual edits to the commit message or standalone .txt files won't be reflected when `b4 send` generates the series.

**How to apply:**
- If the user wants to edit interactively: tell them to run `! b4 prep --edit-cover`. Prepare the updated text and show it to them to paste in.
- If the user asks me to update it non-interactively ("update it for me"): run b4 with a substituting editor, overriding git's core.editor since b4 resolves editor from git config before EDITOR env:
    `cd /home/pdp7/dev/linux && GIT_CONFIG_COUNT=1 GIT_CONFIG_KEY_0=core.editor GIT_CONFIG_VALUE_0='cp /path/to/newcover.txt' b4 prep --edit-cover`
  The input file must be a full commit message: line 1 = subject, line 2 = blank, line 3+ = body + To:/Cc: trailers. Supplying body-only makes the first body paragraph become the subject and mangles the cover (re-run with `subject\n\n<body>` to recover).
  Do NOT include `--- b4-submit-tracking ---` or its JSON — b4 strips the marker before editing and re-appends after. Including it produces two markers and breaks subsequent `b4 prep` calls with `ValueError: too many values to unpack`.
- Always create a backup tag first (`git tag -f backup-before-cover-edit HEAD`) — b4 uses git-filter-repo which rewrites all descendant commit SHAs.
