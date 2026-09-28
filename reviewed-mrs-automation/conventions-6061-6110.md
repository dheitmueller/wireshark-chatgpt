# Durable conventions from !6061–!6110

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

- **Capture pseudo-header endianness:** Guy Harris's merged !6067, !6074 and !6093 show that native host-endian pseudo-header fields from pcap are normalized in wiretap when the file byte order differs. Bound against the smaller of captured and reported record lengths, then verify the pseudo-header's own declared length reaches the field before swapping.
- **Raw state versus presentation:** John Thacker's merged master !6079 (stable !6081) keeps raw SCTP TSN for retransmission/state logic and computes relative TSN separately for display.
- **Semantic lookup identity:** John Thacker's merged !6070 (stable !6071) replaces hard-coded stat-table positions with lookup by semantic table name.
- **Typed proto-tree API contract:** merged stable !6068/!6069 fix code that passed a decoded field value as the `encoding` argument to `proto_tree_add_item()`. Use typed value APIs when the caller already decoded the value.
- **Field/mask consistency:** merged !6102, authored by Martin Mathieson, shows that correcting a mask can require widening the registered `FT_UINT*` type too; Alexis caught the incomplete first correction.
- **Downstream schema effect:** merged !6073 shows that correcting `hf_` width can legitimately change Elastic mapping output; update schema baselines when the field metadata correction is semantically intended.
- **Parser work budgets:** merged master !6098 is the origin of the zero-width PER rule already represented by stable !6116/!6117.
- **Supersession discipline:** closed !6091 carries useful John Thacker API-contract review, but merged !6109 is the accepted implementation and therefore carries more weight.
