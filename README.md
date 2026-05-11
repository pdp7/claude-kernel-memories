# claude-kernel-memories

A curated set of memory files for Claude Code [1], distilled from a year of
RISC-V Linux kernel work. They teach Claude project-specific rules that are
hard to derive from the kernel tree alone, things like build flags, series
mechanics, b4 quirks, and code-comment conventions.

The rules came out of real upstream review cycles, not a whiteboard. Most
generalize beyond RISC-V to any kernel subsystem.

## In practice

A concrete example from April 2026. A maintainer reviewing my QoS series
asked me to split a 2700-line driver patch into reviewable pieces. The
first split Claude proposed was layered, HW callbacks in one patch and
resctrl wiring in another, which would have left several functions dead
until later patches and forced `__maybe_unused` to keep W=1 clean. I
rejected that and asked for a feature-axis split with a scaffolding patch
carrying stubs first, then real implementations replacing stub bodies in
later patches. Claude saved the redirection as `feedback_split_large_commit.md`.
The next time a similar split came up, Claude went straight to the
feature-axis shape, no dead code, no `__maybe_unused`, W=1 clean per
commit. The lesson stuck without me having to teach it twice.

## What this is

Claude Code has a file-based auto-memory system [2]. At session start, Claude
loads `MEMORY.md` from a per-project directory and uses it as an index into
individual memory files. When the user gives feedback ("don't do X", "always
do Y"), Claude saves that as a new file and adds a line to `MEMORY.md`.

This repo is a snapshot of the `feedback_*.md` files from one developer's
working directory after the v1 to v4 cycle of an upstream RISC-V kernel
series. The files are markdown with a small YAML frontmatter block. There
is no executable code here.

## Layout

The repo is split into two sets so users who don't work on resctrl can
take only the general rules:

- Root `feedback_*.md`, general kernel-development rules (build, series
  mechanics, code-comment conventions, personal voice). Useful for any
  kernel subsystem.
- `resctrl/feedback_*.md`, rules specific to the resctrl filesystem,
  RISC-V CBQRI, and adjacent MPAM/QoS work. Skip this directory unless
  you're touching `fs/resctrl/`, `drivers/resctrl/`, or an arch's QoS
  controller code.

Each directory has its own `MEMORY.md` index.

## Install

The memory directory lives at:

    ~/.claude/projects/<mangled-tree-path>/memory/

where `<mangled-tree-path>` is your kernel tree's absolute path with `/`
replaced by `-`. For `/home/alice/src/linux`, the mangled path is
`-home-alice-src-linux`. Claude Code creates this directory on first run.

Three install patterns:

### General rules only (most users)

    git clone https://github.com/pdp7/claude-kernel-memories.git \
        ~/src/claude-kernel-memories
    cd ~/.claude/projects/-home-YOU-src-linux/memory/
    ln -s ~/src/claude-kernel-memories/feedback_*.md .
    ln -s ~/src/claude-kernel-memories/MEMORY.md .

The glob only matches the root `feedback_*.md` files, not the ones under
`resctrl/`. Pull updates with `git -C ~/src/claude-kernel-memories pull`.

### General plus resctrl/CBQRI

    git clone https://github.com/pdp7/claude-kernel-memories.git \
        ~/src/claude-kernel-memories
    cd ~/.claude/projects/-home-YOU-src-linux/memory/
    ln -s ~/src/claude-kernel-memories/feedback_*.md .
    ln -s ~/src/claude-kernel-memories/resctrl/feedback_*.md .
    # merge the two indexes into one MEMORY.md
    cat ~/src/claude-kernel-memories/MEMORY.md \
        ~/src/claude-kernel-memories/resctrl/MEMORY.md > MEMORY.md

Claude loads a single `MEMORY.md` per project, so concatenate rather than
symlinking both.

### Cherry-pick

    cp ~/src/claude-kernel-memories/feedback_use_worktrees.md \
       ~/.claude/projects/-home-YOU-src-linux/memory/
    # then add a line to your own MEMORY.md:
    # - [Use git worktrees](feedback_use_worktrees.md): short hook

## What's in here

Each file is a self-contained rule with **Why** (the reasoning) and **How to
apply** (when it kicks in).

### General rules (root)

#### Build and workspace

- `feedback_build_llvm.md` LLVM=1 W=1, never GCC, never blanket -Werror or W=2
- `feedback_use_worktrees.md` Always git worktree, never switch branches in main tree
- `feedback_build_sequential.md` Sequential -j16 builds beat parallel on a 16-core box
- `feedback_kbuild_o_worktree.md` Kbuild O= rejects dirty srctree, use a linked worktree, do not mrproper

#### Series mechanics

- `feedback_branch_naming.md` `<user>/<chip>-<topic>` for shared push targets
- `feedback_b4_cover.md` Use `b4 prep --edit-cover`, not manual amend, with a non-interactive trick
- `feedback_lore_access.md` lore.kernel.org blocks WebFetch via Anubis, use `b4 mbox`
- `feedback_skip_checkpatch_cover.md` Skip checkpatch on the b4 cover commit
- `feedback_ready_to_send_gates.md` checkpatch --strict, sparse, and a 6-config build matrix before "ready to send"
- `feedback_orc_mbox.md` ORC's `create_changes.py` needs a manual `commit-message.json` for mbox inputs

#### Series shape

- `feedback_separate_commits.md` One logical change per commit
- `feedback_no_refactor_in_series.md` No refactor-of-same-series-code commits, fold into the originating patch
- `feedback_split_large_commit.md` Splitting a large patch: feature axis, scaffolding patch first, no `__maybe_unused`
- `feedback_changelog_no_dev_timeline.md` Cover changelog describes v(N) state, not the dev timeline of v(N)
- `feedback_cover_letter_style.md` Six patterns for kernel cover letters

#### Code style

- `feedback_kernel_terse.md` Audience is experienced kernel devs, no narration
- `feedback_no_patch_refs_in_comments.md` "this patch", "later patch" make no sense at HEAD
- `feedback_no_at_in_regular_comments.md` `@name` is reserved for kernel-doc

#### Personal voice (optional, take what you like)

- `feedback_no_em_dash.md` No em-dash, no `-` or `;` mid-sentence in prose
- `feedback_single_space_sentences.md` One space after a period, never two
- `feedback_american_english.md` American English defaults
- `feedback_trailer_order.md` `Assisted-by:` before `Signed-off-by:`
- `feedback_no_mirrors_attribution.md` Drop "Mirrors MPAM's foo()" framing
- `feedback_dont_ask_permission.md` Just proceed with git ops, do not block on confirmation
- `feedback_reduce_permissions.md` Batch read-only commands
- `feedback_opensbi_archive_purpose.md` Read OpenSBI list archive broadly, do not filter to current series

The voice rules are personal. Pick the ones that match how you write, ignore
the rest.

### resctrl, CBQRI, and QoS rules (`resctrl/`)

Skip this set unless you're touching `fs/resctrl/`, `drivers/resctrl/`,
RISC-V CBQRI, or another arch's QoS controller code.

- `resctrl/feedback_mon_event_ordering.md` `resctrl_enable_mon_event()` must precede `resctrl_online_mon_domain()`, or `rmid_busy_llc` stays NULL
- `resctrl/feedback_resctrl_arch_get_num_closid.md` fs/resctrl invokes this on rids the arch may not back, NULL-check `hw_res->ctrl` first
- `resctrl/feedback_minimize_fs_resctrl_changes.md` Don't touch `fs/resctrl/` in a CBQRI series unless CBQRI needs it; QoL fixes go in a separate fs/resctrl series
- `resctrl/feedback_keep_debug_prints.md` Pick the right `pr_*` level for QoS/CBQRI debug prints; do not strip them

## What's not in here

- `project_*.md` files (in-flight work state for a specific series)
- `reference_*.md` files (paths and tool layouts on one developer's machine)
- `user_*.md` files (one developer's role and writing voice)

These exist in the source directory but are scrubbed from the public repo.
The .gitignore enforces this.

## Adapt for your tree

A few rules reference Tenstorrent-flow specifics:

- `feedback_branch_naming.md` uses `dfustini/<soc>-<topic>`. Substitute your
  username and your team's chip prefixes.
- `feedback_ready_to_send_gates.md` references a local `build-matrix.sh` and
  `checkpatch-series.sh`. Those scripts are not in this repo. Either write
  your own or remove the script references and run the underlying make
  commands directly.

## Add your own

A new memory file looks like this:

    ---
    name: Short title
    description: One-line summary, used by Claude when scanning for relevance
    type: feedback
    ---
    The rule, in one sentence.

    **Why:** The reasoning, often a past incident or strong preference.

    **How to apply:** When this rule kicks in and what to do.

After writing the file, add one line to `MEMORY.md`:

    - [Short title](feedback_short_title.md): one-line hook under ~150 chars

`MEMORY.md` is an index, not a memory. Lines after 200 are truncated when
Claude loads it, so keep entries one line each.

## Why this exists

A teammate walked into a 35-minute developer-efficiency session and asked
"what have you actually accumulated beyond the defaults." This is the answer.
Most of these came out of moments where Claude got something wrong, I
corrected it, and Claude saved the correction. Each `originSessionId` field
in the file frontmatter ties the rule back to the conversation that produced
it.

If a rule turns out to be wrong, fix it or delete it. Memory is meant to be
edited.

## License

CC0 1.0 [3]. No attribution required, do whatever.

## References

[1] https://docs.claude.com/en/docs/claude-code

[2] https://docs.claude.com/en/docs/claude-code/memory

[3] https://creativecommons.org/publicdomain/zero/1.0/

thanks,
drew
