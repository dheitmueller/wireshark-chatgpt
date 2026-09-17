# Wireshark MR review automation: !16959-!17008

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Direction: descending from newest unreviewed MRs toward older MRs.

## Exact reviewed set

!17008, !17007, !17006, !17005, !17004, !17003, !17002, !17001, !17000, !16999, !16998, !16997, !16996, !16995, !16994, !16993, !16992, !16991, !16990, !16989, !16988, !16987, !16986, !16985, !16984, !16983, !16982, !16981, !16980, !16979, !16978, !16977, !16976, !16975, !16974, !16973, !16972, !16971, !16970, !16969, !16968, !16967, !16966, !16965, !16964, !16963, !16962, !16961, !16960, !16959.

Count: **50**.

The historical !17571-!17620 batch remains part of the already-reviewed set and must continue to be counted when selecting future batches.

## Review notes

Merged master changes were weighted above release backports and closed/superseded work. High-authority maintainer comments, where present, were weighted above incidental discussion.

### Durable/representative evidence

- **!17000 (merged master, John Thacker): GSMTAP header/tree completeness.** Adds the GSMTAP protocol item and common header fields before trying the type-specific dissector table. This avoids a gap in dissected bytes and avoids the confusing state where `frame.protocols` contains GSMTAP but no corresponding GSMTAP field/protocol tree is visible. This reinforces the existing convention that a framing/dispatch dissector should account for its own header consistently even when payload handling is delegated.
- **!16960 (merged master, Ivan Nardi): STUN unknown attributes remain filterable.** Unknown STUN attributes now cover the full attribute in expert information while also adding the ordinary `stun.att.type` field with the unknown numeric value. This preserves filtering/analysis capability for values not yet known to the dissector rather than representing them only as free-form expert text.
- **!16959 (merged master, John Thacker): language-specific compiler flags.** Moves `-Wincompatible-pointer-types` and `-Wint-conversion` into the C-only warning list because they describe C constructs and some compilers warn when they are supplied to C++ compilation. This reinforces existing build guidance that warning/diagnostic flags should be scoped to the language/toolchain where they are meaningful.
- **!16990 (merged master): WSUG Follow documentation.** Consolidates the evolving list of followable protocols in the canonical Follow section instead of duplicating it in several menu descriptions. This is useful documentation-maintenance evidence: volatile enumerations should have one authoritative documentation location and other sections should link to it.
- **!16980 (merged master, Anders Broman): Exported PDU stream metadata.** Adds `tcp.stream` tagging/layer information to exported PDUs so downstream analysis retains stream identity. This reinforces preserving useful analysis metadata when packets are transformed/exported.
- **!16970 (merged release-4.4): SMB packet-list presentation.** Replaces unwieldy filename lists in the Info column with a count and moves useful search ID/information-level context into the packet summary while retaining detailed filenames in the protocol tree. As a release backport it carries less independent weight, but it illustrates the established UI distinction between concise packet-list summaries and complete packet-detail data.
- **!17008 (closed draft): BTLE split attempt.** The snapshot ended with an empty/conflicted diff after master commits were pulled into the branch. It is not treated as an accepted refactoring exemplar.

## Notebook action

No separate durable convention file was changed in this run. The strongest findings reinforce existing guidance on framing-dissector completeness, preserving filterable numeric fields for unknown protocol values, language/toolchain-specific compiler flags, metadata preservation, and keeping packet-list summaries concise while retaining detail in the tree. The documentation single-source-of-truth observation is useful but not sufficiently Wireshark-specific to justify a new convention entry by itself.

## Continuation

Rebuild the already-reviewed set from all tracking before the next run rather than assuming numeric coverage. Subject to that rebuild, the next descending candidate after this batch is **!16958**.
