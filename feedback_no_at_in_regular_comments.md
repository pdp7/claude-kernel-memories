---
name: No @symbol in non-kernel-doc comments or commit messages
description: Reserve @-prefix for actual kernel-doc parameter docs. In regular block comments and commit messages, use the bare symbol name.
type: feedback
originSessionId: f3330f30-92b0-40a4-866f-b17fadc2d3ed
---
Reserve `@name` syntax for actual kernel-doc blocks (the `/** ... */`
ones describing function/struct parameters). Inside regular block
comments (`/* ... */`) and commit messages, refer to symbols by their
bare name, no `@` prefix. The `@name` form looks weird outside a
kernel-doc context.

**Why:** Drew's stylistic preference. `@param` is a kernel-doc marker
that has formal meaning inside `/** */`. Carrying it into regular
comments or commit messages reads as a kernel-doc tic without serving
kernel-doc's purpose, and just looks odd.

**How to apply:**
- Inside a `/** */` kernel-doc block describing parameters or struct
  fields, keep `@name` as normal.
- Inside a `/* */` comment that explains code, use the bare name:
  `Capture the cfg->at half before write` not `@cfg->at half`.
- In commit messages, same rule, bare names.
- When reviewing or writing comments, scan for `@[a-z_]` inside
  non-kernel-doc comments and convert to bare names.
