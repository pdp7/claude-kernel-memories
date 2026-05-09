---
name: Use American English spelling
description: Drew writes in American English; prose, code comments, commit messages, and documentation should use American spellings (color, behavior, serialize, organization, ...) not British spellings (colour, behaviour, serialise, organisation, ...)
type: feedback
originSessionId: 56dffc97-8c75-430e-8ed2-7896f448b175
---
Use American English spelling everywhere I generate text for Drew: code
comments, commit messages, kernel-doc, cover letters, Documentation/ RST,
selftest output strings, log messages.

**How to apply:** Default to the American form. Common substitutions:

| British | American |
|---------|----------|
| behaviour | behavior |
| colour | color |
| centre | center |
| metre | meter |
| serialise / serialised | serialize / serialized |
| synchronise / synchronisation | synchronize / synchronization |
| organisation | organization |
| analyse / analysed | analyze / analyzed |
| realise / realised | realize / realized |
| optimise / optimised | optimize / optimized |
| utilise / utilised | utilize / utilized |
| recognise / recognised | recognize / recognized |
| catalogue | catalog |
| dialogue (in code) | dialog |
| traveller / travelled | traveler / traveled |
| favourite / favour | favorite / favor |
| neighbour / neighbouring | neighbor / neighboring |
| labelled / labelling | labeled / labeling |
| modelled / modelling | modeled / modeling |
| cancelled / cancelling | canceled / canceling |
| -ise / -isation suffix in general | -ize / -ization |

**Why:** Drew is a US-based RISC-V kernel maintainer; the kernel community is
mixed (Arm/MPAM contributors lean British, x86 contributors lean American)
but Drew's own voice is American. Mixing British spellings into prose Drew
authors makes the voice inconsistent and visibly machine-generated.

**Exceptions:**
- Pre-existing kernel code, comments, or strings that already use British
  spelling: do NOT rewrite them in unrelated patches. Convert only when the
  current patch is already touching the line for another reason.
- Identifiers (function names, struct fields, macros) keep whatever spelling
  upstream chose. Don't rename `mpam_serialise_foo()` to
  `mpam_serialize_foo()`.
- Spec quotations (CBQRI/RISC-V specs use mostly American spellings already,
  but if a spec section uses British, quote it verbatim).

**Auditing existing prose:** A quick grep finds British forms in Drew-authored
text:

```bash
grep -rEn "\b(behaviour|colour|centre|metre|serialis|synchronis|organis|analys|realis|optimis|utilis|recognis|catalogue|favour|neighbour|labelled|modelling|cancelled)" \
  drivers/resctrl/ drivers/acpi/riscv/ arch/riscv/ include/linux/cbqri.h \
  Documentation/filesystems/resctrl.rst tools/testing/selftests/resctrl/cbqri_*
```
