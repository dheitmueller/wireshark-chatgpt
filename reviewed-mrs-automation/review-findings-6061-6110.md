# Review findings: !6061–!6110

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged MRs receive primary architectural weight. Closed !6091 is retained only as review/supersession evidence.

| MR | Outcome / depth | Result |
|---|---|---|
| !6110 | merged — deep/corroboration | Moves Expert pseudo-protocol GUI eligibility into preference/module metadata so UI consumers share the same semantic source. |
| !6109 | merged — deep | X.509 passes a real `hf_index` to `dissect_ber_bitstring()`, fixing the caller without narrowing shared BER helper behavior. |
| !6108 | merged — superseded negative | Signed-cast length-overflow check later replaced by the stronger structural invariant in !6147; do not treat the cast heuristic as current guidance. |
| !6107 | merged stable — discussion | Alexis raised ABI concerns; João clarified that the changed helper is not ABI-public because it is not exported with `WS_DLL_PUBLIC`. |
| !6106 | merged — discussion | Kerberos work; Alexis caught a dead store and Jaap required component-prefixed commit-message style. |
| !6105 | merged — scanned | Removes invalid Expert severity “Ok”. |
| !6104 | merged — scanned | TECMP explicitly handles “Not Available” chassis temperature. |
| !6103 | merged — discussion | Rewrites libgcrypt error tests so assigned errors are actually read, satisfying analyzer semantics. |
| !6102 | merged — deep | Martin's `--mask` cleanup; Alexis catches that a wider mask also requires widening the registered field type. |
| !6101 | merged — scanned | GTP checked-message tables updated to TS 29.060 V16.0.0. |
| !6100 | merged — scanned | Adds GTP UE Registration Query messages. |
| !6099 | merged — scanned | Further GTP checked-message V16.0.0 updates. |
| !6098 | merged — deep/corroboration | Master source of later stable !6116/!6117: valid PER NULL elements may consume zero bits, so bound repeated work/cardinality instead of requiring fake cursor progress. |
| !6097 | merged — scanned | Adds Initiate PDP Context Activation checked messages. |
| !6096 | merged — scanned | GTP Tunnel Management validation update. |
| !6095 | merged — scanned | Uses semantic GGSN control/user-plane address decoders where the message structure identifies the roles. |
| !6094 | merged — deep/corroboration | ESP NULL heuristic validates derived padding/payload length before creating a TVB subset. |
| !6093 | merged — deep, Guy Harris | Wiretap byte-swaps pflog UID/PID only after min(captured,reported) bounds and pseudo-header-length validation. |
| !6092 | merged — deep/corroboration | AMP adds progress checks/expert reporting to prevent large/infinite loops. |
| !6091 | closed — superseded | John Thacker notes BER/PER helpers may intentionally use negative `hf_id` for value-only decode; author agrees caller should be fixed. Superseded by !6109. |
| !6090 | merged — scanned | Gives ZBOSS fields distinct filter abbreviations. |
| !6089 | merged — scanned | Simplifies TPNCP dynamic field-array size tracking and fixes an off-by-one/missing-events crash. |
| !6088 | merged — scanned | Fixes Qt PacketDialog preference context menu. |
| !6087 | merged — scanned | Documents SOME/IP tshark statistics. |
| !6086 | merged stable — scanned | TShark/Wireshark TCP capture-input documentation backport. |
| !6085 | merged stable — scanned | Same documentation backport. |
| !6084 | merged stable — scanned | dumpcap TCP capture-input documentation backport. |
| !6083 | merged stable — scanned | dumpcap TCP capture-input documentation backport. |
| !6082 | merged — scanned | Master TShark/Wireshark TCP capture-input documentation. |
| !6081 | merged stable — corroboration | Backport of SCTP raw/relative TSN fix from !6079. |
| !6080 | merged — scanned | Modernizes tshark read-filter terminology to display-filter/`-Y`. |
| !6079 | merged — deep, John Thacker | Keeps raw SCTP TSN for retransmission/state logic while computing relative TSN separately for presentation. |
| !6078 | merged — scanned | Master dumpcap TCP capture-input documentation. |
| !6077 | merged — scanned | pflog reference URL typo fix. |
| !6076 | merged — scanned | OER rejects zero-length integer input before typed return-value helpers. |
| !6075 | merged — scanned | Developer Guide begins Visual Studio 2022 migration. |
| !6074 | merged — deep/corroboration, Guy Harris | pflog UID/PID are native host-endian; preference distinguishes host/big/little and anticipates file-reader normalization. |
| !6073 | merged — deep/corroboration | Corrects too-narrow field registrations; John Thacker explains why the correct width changes the Elastic mapping baseline. |
| !6072 | merged — scanned | Clang Analyzer cleanup. |
| !6071 | merged stable — corroboration | Stable backport of semantic SIP stat-table lookup. |
| !6070 | merged — deep, John Thacker | Replaces hard-coded request/response table array indices with lookup by semantic table name. |
| !6069 | merged stable — deep/corroboration | Fixes PROFINET call that passed a decoded value as `proto_tree_add_item()` encoding; uses `proto_tree_add_uint()`. |
| !6068 | merged stable — deep/corroboration | Same typed proto-tree API correction. |
| !6067 | merged — deep, Guy Harris | pflog cleanup correctly rounds header length, handles OS variants, and uses typed add-item-return helpers. |
| !6066 | merged — discussion | MPEG Telephone Descriptor review tightens types and encourages reuse of return-value proto-tree helpers. |
| !6065 | merged — scanned | Guards nullable DCM description before formatting. |
| !6064 | merged stable — discussion | Adds ABI dump/compliance CI for core libraries against release baselines. |
| !6063 | merged — discussion | Roland Knall/Guy Harris portability review: do not infer `int` width from pointer width or architecture folklore; honor the actual API contract. |
| !6062 | merged — discussion | MPEG NVOD descriptor review refines integer types/parser style. |
| !6061 | merged — scanned | Automatic generated/reference-data update; low architectural weight. |

## Highest-value conclusions

1. Wiretap owns pcap pseudo-header byte-order normalization; validate record bounds and pseudo-header-declared length before touching optional fields (!6067, !6074, !6093).
2. Keep raw protocol identity/state values separate from presentation-normalized values (!6079, !6081).
3. Use semantic identity rather than container position (!6070, !6071).
4. Typed proto-tree APIs have distinct contracts: decode-from-TVB APIs take encoding flags; typed value APIs take already-decoded values (!6068, !6069).
5. Treat field type, mask, access width and value tables as one invariant (!6102); correct field-width changes can propagate into exporter schemas (!6073).
6. Fix a bad caller rather than narrowing a shared parser helper's legitimate sentinel behavior (!6091 versus merged !6109).
7. Validate heuristic-derived lengths before TVB slicing (!6094).
8. Valid zero-width grammar elements need bounded work, not artificial cursor movement (!6098, later !6116/!6117).
