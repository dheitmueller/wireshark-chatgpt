# Wireshark MR review automation ledger: !20189-!20238

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Selection method: rebuilt the already-reviewed set from `reviewed-mrs.md` and all available files under `reviewed-mrs-automation/`, including and preserving the historical !17571-!17620 batch. Selected the fifty highest-numbered corpus MRs not already present in that reviewed set, continuing newest-to-oldest. Membership was determined per MR rather than assuming a numeric interval was reviewed wholesale.

Exactly 50 MRs reviewed in this run:

!20238, !20237, !20236, !20235, !20234, !20233, !20232, !20231, !20230, !20229,
!20228, !20227, !20226, !20225, !20224, !20223, !20222, !20221, !20220, !20219,
!20218, !20217, !20216, !20215, !20214, !20213, !20212, !20211, !20210, !20209,
!20208, !20207, !20206, !20205, !20204, !20203, !20202, !20201, !20200, !20199,
!20198, !20197, !20196, !20195, !20194, !20193, !20192, !20191, !20190, !20189.

## Review weighting and notable findings

- Merged master MRs were treated as the strongest evidence; release backports were used mainly as corroboration.
- !20234 is an open/draft Guy Harris API design MR and was deliberately treated as provisional rather than accepted architecture despite its authoritative authorship.
- !20216 is closed/unmerged and was down-weighted as implementation evidence, but its protocol-expert discussion is valuable negative evidence about assigning semantics from reverse-engineered captures. It also explicitly challenges parts of merged !20198, demonstrating that merge status does not make an inferred proprietary-protocol interpretation infallible when later knowledgeable review contradicts it.
- !20218 and !20223 provide strong merged evidence that dissectors supporting heuristic/conversation entry paths must not assume `pinfo->match_uint` or other dispatch-table-only context is available.
- !20217 fixes incomplete Bluetooth ATT state identity by adding the remote BD_ADDR to MTU tracking; !20235 independently reinforces reuse of established session tracking rather than adding a redundant parallel state mechanism.
- !20227 distinguishes reported length (original protocol packet semantics) from captured length (bytes locally available), using reported length for a protocol message-length comparison.
- !20189, authored and merged by Guy Harris, is strong architecture evidence for making a registration-information table authoritative and eliminating parallel switches/manual indices; it also rejects duplicate block-type registration.
- !20208 is another Guy Harris-authored/merged Wiretap structural change, adding a file-type-specific block category and registering block types through common block metadata; it is consistent with the table-driven direction in !20189.
- !20205 adds an explicit redissection operation usable during live capture, useful when later state such as DSBs changes how earlier packets can be interpreted; this corroborates the broader requirement that dissector state and presentation tolerate redissection rather than assuming a single immutable forward pass.
- !20194 fixes BLF channel XML parsing when logical XML spans multiple physical blocks, reinforcing the general stream/file-reader rule that logical structures may cross container-record boundaries.
- The FT_CHAR/FT_BYTES/string/IP encoding cleanup series (!20224, !20225, !20203, !20207) was treated as corroboration for existing notebook guidance that `proto_tree_add_item()` encoding arguments must match field type and actual wire encoding.
- Automatic update/backport MRs and straightforward protocol-value additions were reviewed but not promoted to new notebook conventions unless they supplied reusable architectural or review evidence.

## Notebook changes

Created `dissector-entry-and-state-conventions.md` with durable guidance extracted from !20218/!20223, !20217/!20235, !20227, !20198/!20216, and !20189.

No update to `reviewed-mrs.md` was required; this per-run ledger is authoritative for the exact batch membership.
