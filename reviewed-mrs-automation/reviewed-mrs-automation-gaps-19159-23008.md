# Wireshark MR automation review ledger — gap batch through !19159

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

This run rebuilt the already-reviewed set from `reviewed-mrs.md` plus every per-run ledger in `reviewed-mrs-automation/`, preserving the historical !17571-!17620 batch. Selection is by individual MR number, not inferred numeric ranges.

Exactly 50 previously unreviewed corpus MRs were reviewed, in descending priority/order:

- !23008, !23007, !23006, !23005, !23004, !23003, !23002, !23001, !23000, !22999, !22998, !22997, !22996, !22995, !22994
- !22662
- !22208
- !21133, !21132
- !19189, !19188, !19187, !19186, !19185, !19184, !19183, !19182, !19181, !19180, !19179, !19178, !19177, !19176, !19175, !19174, !19173, !19172, !19171, !19170, !19169, !19168, !19167, !19166, !19165, !19164, !19163, !19162, !19161, !19160, !19159

Outcome weighting followed the notebook policy: merged work was treated as accepted evidence; closed/abandoned/superseded work was down-weighted. In particular, !23008 was closed unmerged with a failed pipeline and no substantive human review, so it is not an implementation exemplar.

Durable observations from the accepted work primarily corroborate existing notebook guidance rather than requiring duplicate convention text. Notable examples include !19180, which keeps a machine-output semantic option consistent between tshark and the GUI while moving format-specific validation/argument population into format-specific UI components, and !19160, which extends an 8-bit protocol sequence number using protocol window invariants and preserves the derived value across redissection for presentation/reassembly consistency. !19170 is a release-4.4 backport of the WSLua `DissectorTable` name lifetime fix and reinforces existing ownership/lifetime guidance.

No convention file was changed in this run because the strongest findings were corroborative rather than genuinely new.
