# Review findings — Wireshark MRs 10362 through 10313

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed exactly 50 previously unreviewed MRs, !10362 down through !10313. The set was selected by explicit membership against the available review tracking, including the prior branch-only !10412–!10363 ledger, rather than by assuming contiguous ranges had been covered. The historical !17571–!17620 batch remains preserved and counted. Outcomes were 44 merged and six closed/unmerged; closed work was down-weighted.

## Durable findings

- **Compiler identity is not dialect compatibility.** Merged !10329, with direct John Thacker review, adds real GCC identification that excludes Clang, Intel Classic and Intel LLVM before enabling GCC-only diagnostics. Merged !10318 uses the project compiler/version helper for a warning introduced in GCC 7.1. This reinforces central compiler-feature selection rather than ad-hoc `__GNUC__` tests.
- **Generated ASN.1 output must be backed by the authoritative input.** Guy Harris explicitly caught generated-file-only changes in merged !10330, !10341 and !10343 and supplied corrective merged !10353, !10351 and !10352. Merged !10320 is another source-side correction after generated output had been changed directly. This is exceptionally strong historical corroboration of the notebook's generated-code rule.
- **Source checkers should evaluate simple macros, then diagnose semantics.** Martin Mathieson's merged !10315 teaches `check_typed_item_calls.py` to substitute straightforward mask macros before checking width/contiguity. The stronger checker immediately identified genuine hf type/mask errors. The lesson is to improve semantic reach without pretending arbitrary expressions are statically known.
- **Recoverable programmer-contract failures need not be fatal fuzzing events.** João Valverde's merged !10332 moves `ws_return.h` invalid-argument diagnostics to an informational `InvalidArg` domain, preserving opt-in fatal debugging while preventing the default fuzzer configuration from aborting on a path designed to return safely.

## Strong corroborating and negative-review evidence

Closed !10345 proposed broad SRT timestamp handling changes. John Thacker instead recommended redissection after time shift; the issue was fixed that way and the MR was closed. This strongly corroborates the later notebook rule that metadata edits affecting first-pass/file-scoped derived state should trigger redissection rather than patching every cached timestamp representation.

Closed !10324 added another packet-bytes copy format, but Stig Bjørlykke pointed out that the existing “Show Packet Bytes” UI already offered C Array and Rust Array output; the author confirmed it solved the use case and closed the MR. This is useful submission/review evidence for checking existing functionality before adding parallel UI surface.

Merged !10362 makes nullable integer-key wmem lookup behavior consistent with string lookup, permitting lazy tree creation without a NULL dereference. Merged Guy Harris !10349 documents `proto == -1` at dissector-table registration as an intentional “no associated protocol” sentinel instead of an accidental consequence of a downstream negative-ID lookup. Merged John Thacker !10361, !10342 and !10322 harden H.264/MPEG byte-stream handling, file probing and program-end semantics; those changes fit already-recorded framing and bounds disciplines rather than creating new general rules.

Closed draft !10360 (WSML) was exploratory and never established accepted architecture. Closed automatic-update MRs !10335, !10334 and !10333 simply missed their update window. All were counted in the exact set but appropriately down-weighted.

The authoritative exact set is recorded in `reviewed-mrs-automation/reviewed-mrs-automation-10313-10362.md`.
