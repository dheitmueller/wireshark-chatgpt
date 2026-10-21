# Review findings: !1060-!1109

This batch contains 49 merged MRs and one closed/superseded MR (!1076). Merged master changes and direct maintainer guidance were weighted most heavily; release backports were treated mainly as corroboration.

## Durable findings

### Reassembly must model the logical upper-layer byte stream, not incidental lower-layer padding

Merged !1109 fixes RPC-over-RDMA Position-Zero Read Chunk reassembly. InfiniBand BTH padding is transport-layer padding and must be removed before the fragment enters RPCoRDMA reassembly because the XDR stream already contains its own required roundup padding. The implementation carries both BTH `pad_count` and the RDMA segment's XDR position so it can make that decision from protocol state rather than byte coincidence. Anders Broman approved/merged the change.

Merged !1095 supplies a complementary state-machine rule for Bluetooth L2CAP: once a prior fragmented PDU is known broken because a fragment is missing, advance the reassembly identity and clear the active segmentation state. A later packet whose length happens to match the old remainder must not resume the stale reassembly.

### Protocol recognition should use negotiated/structural invariants, not greasable bit patterns

Merged !1106 changes QUIC coalesced-packet handling. Instead of treating a zero first byte as padding, it checks the invariant that coalesced QUIC packets in one datagram use the same destination connection ID. The old byte shortcut was invalid because the Fixed bit can be greased. Rejected trailing data is reported as padding rather than forced through short-header dissection.

### Use packet-scope allocation for packet-lifetime tree data

Merged !1102 changes `tvb_get_bits_array(NULL, ...)` to `tvb_get_bits_array(wmem_packet_scope(), ...)` in the proto-tree bits path. Gerald Combs explicitly asked the original author whether heap allocation was intentional; the author said it was not and that the internal API detail had been missed in review. Release backport !1107 corroborates the fix.

### Supported dependency versions constrain API adoption

Merged !1088 fixes byte-view text geometry with `QFontMetrics::horizontalAdvance()`, but Jim Young caught that Ubuntu's supported Qt 5.9.5 could not compile it because the API arrived in Qt 5.11. The accepted implementation centralizes width calculation and version-guards the newer API, retaining `boundingRect().width()` as the old-Qt fallback.

Merged !1096 is a smaller compile-time analogue: the local variable needed only by libgcrypt OCB code is declared only when `GCRY_OCB_BLOCK_LEN` exists, while the protocol-tree item creation that is valid on old libgcrypt remains outside the guard. Pascal Quantin confirmed the scope of the guarded use after Anders Broman questioned the unusual-looking change.

### UI sentinel rows must not participate in locale-dependent data sorting

Merged !1094 fixes Export Objects after translations caused “All Content-Types” to sort among real content types even though index zero semantically means “all”. The accepted code sorts only actual content types and inserts the synthetic all-values row separately at the first position.

### Deprecation/warning cleanup still needs behavioral regression testing

Merged !1085 removed deprecated Qt font-metric behavior and changed a width member from floating point to integer. Dario Lombardo later recorded that the change introduced bug #17143. !1088 then addresses byte-view positioning with the API that measures horizontal advance correctly on newer Qt while retaining an old-version fallback. Treat compatibility/warning cleanup as a semantic UI change, not compile-only churn.

### Interpret flag bits only in the structural context where the standard defines them

Merged master !1081, authored and merged by Guy Harris, removes the assumption that a non-QoS 802.11 data frame with the +HTC/Order bit has an HT Control field. That interpretation is valid only in the relevant QoS context. Guy's release backports !1083 and !1084 preserve the same correction.

Guy's master !1078 and backports !1079/!1080 further refactor related WLAN tests by factoring the shared precondition into an outer branch instead of repeating `A && B` and `A && !B`. Master !1072 and backports !1073/!1074 similarly replace an early-break form with an explicit `if/else` when the two paths are semantic alternatives.

### Bounds checks should precede optional lookahead and unnecessary reads

Merged !1104 raises the RSL remaining-length condition from `> 0` to `> 3` before reading at `offset + 3`, and fixes an IE subtree's displayed range. Guy Harris's merged master !1067 (with backports !1069/!1070) goes further in LLC: it no longer reads a two-byte EtherType unconditionally, but first proves that two bytes exist and only fetches the value on the path that needs it. This avoids exceptions on short input while preserving ordinary LLC decoding.

### Sample captures and commit presentation are part of dissector submissions

Merged !1097 added IEEE 802.11 PXU/PXUC decoding with focused PXU and PXUC capture files supplied by the contributor. Graham Bloice also required the commit messages to be amended to Wireshark's documented format, and Alexis La Goutte asked that the PXUC acronym be clarified before approving.

Merged !1077 likewise included a small VXLAN capture plus before/after decoded output. Closed !1076 is the superseded duplicate and carries no additional implementation weight.

### Generated dissectors must be changed at their authoritative inputs

Merged !1075 upgrades LPP to 3GPP TS 37.355 v16.2.0 through the ASN.1 source, conformance file, template, and regenerated dissector outputs. It is additional accepted evidence for the existing rule to modify generator inputs/conformance/template rather than treating generated C as the source of truth.

### Object ownership should be expressed through the framework when possible

Merged !1060 fixes a repeated Capture Options leak by constructing `SparkLineDelegate(this)`. Giving the QObject a parent makes Qt own and destroy it with the dialog instead of requiring ad-hoc manual deletion.

### Large recursive/container parsers need malformed-input and sanitizer coverage

Merged !1061 substantially expands D-Bus parsing to arrays, structs, dictionary entries, variants, validation, nesting limits, and expert information, and the contributor supplied a real capture. Gerald Combs later reported that the change introduced the invalid-pointer read tracked as issue #17176; that issue was eventually fixed by later merged D-Bus fuzzing work. The negative evidence is useful: a representative happy-path capture is not enough for a recursive type grammar with attacker-controlled nesting and lengths.

## Corroborating / lower-yield MRs

!1108 and !1100 harden Kafka decompression with an explicit resource-size limit, boolean success values, and success-dependent cursor advance; !1108 is the stable backport. !1105 carries QUIC loss-bit negotiation state into header-protection semantics. !1103 is PDCP-LTE cleanup. !1101 records Anders Broman's request to keep numeric value tables numerically sorted. !1099 updates macOS bootstrap dependencies and adapts to actual dependency requirements/build systems. !1098 is documentation-only. !1093 is Windows dependency packaging. !1092/!1091/!1089 are the same EHDLC display correction across branches. !1090 replaces reverse-engineered GSM IPA names/values with a newly available authoritative source. !1087 is the release backport around the Qt metric deprecation change. !1086 is a large TPNCP data refresh. !1082 gives the little-endian ICMP identifier its own `icmp.ident_le` filter identity instead of colliding with `icmp.ident`. !1071 wires DNS-over-QUIC through the QUIC protocol dispatch table and provides a capture. !1068/!1066/!1063 are generated periodic data/translation updates. !1065/!1064/!1062 are Guy Harris indentation-only LLC cleanups.
