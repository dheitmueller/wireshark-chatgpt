# Automated Wireshark MR review: !19059-!19108

- Corpus: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`
- Review direction: newest to older
- Exact MRs selected: 50
- Selection method: consulted the accumulated per-run tracking under `reviewed-mrs-automation/` plus `reviewed-mrs.md`; preserved the historical !17571-!17620 batch; selected the fifty highest-numbered corpus MRs not present in the reconstructed reviewed set.

## Exact reviewed set

!19108, !19107, !19106, !19105, !19104, !19103, !19102, !19101, !19100,
!19099, !19098, !19097, !19096, !19095, !19094, !19093, !19092, !19091, !19090,
!19089, !19088, !19087, !19086, !19085, !19084, !19083, !19082, !19081, !19080,
!19079, !19078, !19077, !19076, !19075, !19074, !19073, !19072, !19071, !19070,
!19069, !19068, !19067, !19066, !19065, !19064, !19063, !19062, !19061, !19060,
!19059.

## Durable findings / weighting notes

- !19108 was closed without merge because the contributor attempted to submit from a protected `master` branch. Jaap Keuter directed the contributor to use a separate topic branch and resubmit. This corroborates existing clean-topic-branch/submission guidance; the closed MR is workflow evidence rather than an implementation exemplar.
- !19107 is an automatic data/translation update and was merged; it carries little reusable engineering-review evidence.
- !19099 (merged release-4.4 backport, John Thacker) hardens display-filter value-string handling around fields sharing an abbreviation. In particular, `FT_FRAMENUM` may be integer-compatible while its `hfinfo->strings` has different semantics and must not be blindly interpreted as a `value_string *`. The MR also documents that optimizations based on one field registration are unsafe when multiple fields share an abbreviation unless all relevant registrations satisfy the optimization's assumptions. This reinforces the notebook's semantic-type/API-domain guidance.
- !19090 (merged release-4.4 backport, John Thacker) distinguishes presentation strings from full machine-consumed values: Protobuf JSON/PDML dumping must retrieve the actual string value rather than a display string truncated to `ITEM_LABEL_LENGTH`. It also avoids constructing the same string twice. This strongly corroborates the notebook's machine-output/serialization convention.
- !19068 (merged release-4.4 backport, John Thacker) fixes a packet-list crash caused by mutating sort-state members before determining whether a sort can actually start. The comparison function consumes those members, so changing them during an already-running sort can invalidate `std::sort` assumptions. The accepted fix performs admission/read-lock checks first and commits sort state only after the operation is allowed to proceed. This is useful state-transition evidence: do not publish mutable operation state until preconditions and exclusivity checks have succeeded.
- !19096 reuses the E.212 MCC/MNC dissector for LTE-RRC PLMN identity rather than maintaining a parallel local interpretation, corroborating helper-reuse and protocol-ownership guidance.
- !19092 and !19078 are NAS-5GS selection-criteria parsing work; merged protocol-specific correctness changes, useful primarily as implementation evidence rather than a new general convention.

No new convention file was added in this run because the strongest durable findings reinforce existing notebook rules (machine-output values vs display labels, semantic API/type domains, helper reuse, and safe state transitions) rather than justifying duplicate guidance.

## Continuation

Future descending review should rebuild the reviewed set again rather than assuming this or any other numeric range is wholly covered. Subject to newer unreviewed holes being discovered in tracking, the next contiguous frontier below this batch is !19058.
