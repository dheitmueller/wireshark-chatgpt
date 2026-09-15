# Automated Wireshark MR review: !26128–!26177

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Notebook base before this run: `d7348a360a7d985467ac22d6607815bef458180b`

Review scope: exactly 50 previously unreviewed merge requests, **!26128 through !26177 inclusive**. No MR outside this range is counted by this ledger. Selection was made only after consulting the existing per-run ledgers and `reviewed-mrs.md`; in particular, !26178–!26197, !26198–!26247, !26248–!26297, !26298–!26347, the newer 263xx/264xx ledgers, and the historical !17571–!17620 batch remain part of the already-reviewed set.

Merged master work and substantive maintainer review were weighted most heavily. Stable-branch cherry-picks were treated primarily as corroboration, and abandoned/open work was not treated as accepted architecture.

## Notable findings

- !26177: 802.11 frame-duration calculation uses the updated `fcs_len` propagated by radiotap rather than an earlier/default assumption. This reinforces that downstream calculations must consume the authoritative normalized metadata after encapsulation-specific processing.
- !26172 with !26173/!26174: SCTP reassembly overflow hardening plus stable backports. Strong corroboration for checked fragment/aggregate arithmetic and hostile-input reassembly bounds already present in the notebook.
- !26167: capture toolbar mutex lifetime is paired with capture-session lifetime: a `GMutex` cleared between captures must be initialized for each corresponding lifecycle. Corroborates explicit init/clear ownership and repeated-session lifecycle rules.
- !26168: IDN endless-loop correction. Corroborates the parser rule that malformed input must not defeat forward progress/termination.
- !26166: DICOM Export Objects checks size arithmetic before allocation/export and uses the destination API's actual `size_t` domain. Corroborates checked arithmetic and practical consumer limits.
- !26165: Procmon cleanup makes allocated-string and close-path ownership explicit. Corroborates exception/error/close-path lifetime hygiene.
- !26157: extcap control-pipe temporary FIFO directories are removed along with the FIFO objects. Corroborates the existing extcap runtime rule that temporary IPC resources require complete teardown ownership.
- !26156 and the related !26141/!26143 sequence: transient Synchrophasor packet arrays belong in packet scope rather than file scope. Strong corroboration for allocating according to the shortest correct semantic lifetime.
- !26151 plus stable backports !26154/!26155: repeated DICOM string appends use `wmem_strbuf_t` instead of repeated whole-string reconstruction. Corroborates choosing a data structure whose growth semantics match the operation.
- !26150: removes/obsoletes the long-unused `capture.columns` preference after its GTK+ consumer disappeared years earlier. Corroborates removing stale preferences rather than indefinitely preserving knobs with no active semantics.
- !26146: Windows OVERLAPPED I/O priming is arranged so initial ready data cannot cause duplicate callback delivery. Corroborates the extcap asynchronous-control rule that readiness registration and initial I/O state must have one event-delivery contract.
- !26145 plus !26159: QUIC DATAGRAM hands the child dissector a payload tvbuff bounded to the DATAGRAM payload rather than bytes beyond the frame. Corroborates strict child-dissector subset boundaries.
- !26144: ZigBee Touchlink commissioning state is cleared on redissect even though entries are file-scope allocated. Reinforces the distinction between allocator lifetime and logical state lifetime: file-scope storage does not imply state remains valid across redissection/reset.
- !26140: extended 802.11 tags retain the ordinary wire tag number/length while exposing the extended length as generated metadata. Corroborates the existing rule to distinguish wire-backed fields from derived/generated presentation fields.

The remaining MRs in !26128–!26177 were reviewed as protocol-specific additions/fixes, stable backports, documentation/build maintenance, or narrow robustness cleanups. Their reusable lessons were already represented by the notebook's parser-bounds, checked-arithmetic, lifetime, reassembly, extcap-runtime, generated-field, and submission conventions, so no duplicate convention text was added solely to manufacture a notebook change.

## Exact MR accounting

Reviewed MRs: !26128, !26129, !26130, !26131, !26132, !26133, !26134, !26135, !26136, !26137, !26138, !26139, !26140, !26141, !26142, !26143, !26144, !26145, !26146, !26147, !26148, !26149, !26150, !26151, !26152, !26153, !26154, !26155, !26156, !26157, !26158, !26159, !26160, !26161, !26162, !26163, !26164, !26165, !26166, !26167, !26168, !26169, !26170, !26171, !26172, !26173, !26174, !26175, !26176, !26177.
