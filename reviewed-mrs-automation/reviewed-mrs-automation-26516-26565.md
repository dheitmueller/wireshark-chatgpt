# Automated Wireshark MR review: !26516–!26565

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit: `130eea3c8a60902c0fc98c9e1130e73bafc6f815`

Selection method: enumerated the available corpus at the pinned commit, rebuilt the already-reviewed set from `reviewed-mrs.md` plus all available ledgers under `reviewed-mrs-automation/`, and preserved/counts the historical !17571–!17620 batch. The 50 highest-numbered corpus MRs not in that exact set were selected. No numeric range was treated as reviewed merely because neighboring MRs appeared in a ledger.

Reviewed MRs (exact set, descending):

!26565, !26564, !26563, !26562, !26561, !26560, !26559, !26558, !26557, !26556,
!26555, !26554, !26553, !26552, !26551, !26550, !26549, !26548, !26547, !26546,
!26545, !26544, !26543, !26542, !26541, !26540, !26539, !26538, !26537, !26536,
!26535, !26534, !26533, !26532, !26531, !26530, !26529, !26528, !26527, !26526,
!26525, !26524, !26523, !26522, !26521, !26520, !26519, !26518, !26517, !26516.

Count: 50.

Review weighting: merged master changes and accepted stable backports were treated as stronger evidence than opened, draft, closed, or superseded work. Maintainer review was weighted by authority; in particular, Guy Harris's detailed comments on !26531 were treated as strong UI/localization review evidence even though the core implementation was John Thacker's.

Durable findings promoted to notebook:

- !26517: LEAP and EAP used the same `p_add_proto_data()` / `p_get_proto_data()` key for independent `frame_state_t` records, causing state collision and uninitialized-memory access. Added the rule that protocol-data keys form a semantic namespace and independent records need distinct keys to `dissector-state-conventions.md`.
- !26541 (with stable backports !26546 and !26547): UMTS FP now validates `no_ddi_entries` against the consumer's fixed-array capacity even though in-tree producers already cap it. Added consumer-side count/capacity validation to `c-array-size-conventions.md`.
- !26533: Capture Options moves from a separately populated `QTreeWidget` to the shared `InterfaceTreeCacheModel` / `InterfaceSortFilterModel`, putting three interface-list views on common model code. Added shared-domain-model reuse guidance to `qt-model-conventions.md`.

Strong corroborating evidence not duplicated as a new rule:

- !26563: after truncating decoded Catapult DCT2000 payload data, padding and child-TVB lengths are based on the bytes actually materialized; `tvb_new_child_real_data()` also ties temporary tvbuff lifetime to the parent. This reinforces existing authoritative-length and child-TVBuff lifetime guidance.
- !26560: ETWDump keeps the size passed to `TdhGetProperty()` consistent with the actual backing buffer, validates property indices/sizes, and avoids carrying a larger heap-buffer size when falling back to stack storage. This reinforces existing destination-capacity and parser-boundary rules.
- !26531: native language names are sorted case-insensitively using system-locale collation, and the language selector connects its change signal only after initial population. Guy Harris's detailed review supported the case-insensitive treatment of inconsistent native capitalization. Useful Qt/i18n evidence, but not enough to duplicate broader UI/state initialization guidance already represented elsewhere.

Opened/unresolved work was intentionally down-weighted. Examples include !26565 (new OPC UA PubSub UADP dissector), !26550 (process-block/packet association), !26543 (PROFINET security block-version update with unresolved discussion), !26518 (large dumpcap process-attribution series MR with unresolved scalability/design review), and !26516 (profile path separator change). Their discussions were reviewed for context, but they were not used as accepted implementation precedent.
