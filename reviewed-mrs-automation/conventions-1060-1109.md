# Durable conventions from !1060-!1109

- !1109: feed reassembly the logical upper-layer byte stream; remove lower-layer padding that is not part of it.
- !1095: after a fragmented PDU is known broken, advance its reassembly identity and clear active segmentation state so later length-compatible packets cannot resume stale state.
- !1106: recognize coalesced protocol units with protocol invariants such as QUIC connection-ID consistency; do not rely on greasable bits as fixed signatures.
- !1102: packet-lifetime buffers used by the protocol tree should use packet-scope allocation rather than unmanaged heap allocation.
- !1088 and !1096: adopting a newer dependency API must respect the supported version matrix; guard version-specific APIs and preserve a correct fallback when older supported versions remain.
- !1094: semantic sentinel rows such as “All” must not be mixed into locale-dependent sorting of ordinary data rows when position has meaning.
- !1085: deprecation-warning cleanup can change behavior; test rendering/geometry, not only compilation.
- !1081, authored and merged by Guy Harris: interpret a flag only in the frame/subtype context where the standard gives it that meaning.
- !1067, authored and merged by Guy Harris: prove optional bytes exist before fetching a field that is only needed on one path.
- !1097 and !1077: focused sample captures materially strengthen dissector submissions; commit subjects/messages must also follow project conventions.
- !1075: for generated ASN.1 dissectors, change authoritative ASN.1/conformance/template inputs and regenerate rather than treating generated C as primary source.
- !1060: express QObject lifetime with parent ownership when the child should die with the dialog.
- !1061: complex recursive/container parsers need malformed-input and sanitizer/fuzz coverage in addition to a representative valid capture; a post-merge invalid-pointer issue was reported against this D-Bus expansion.
