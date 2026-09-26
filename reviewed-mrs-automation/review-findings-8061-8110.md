# Wireshark MR review findings: 8061-8110

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

This file records the per-MR disposition for the exact 50-MR batch. Merged master changes and substantive maintainer discussion are weighted above routine backports, automatic updates, or closed/superseded submissions.

| MR | Outcome / depth | Review finding |
|---|---|---|
| 8110 | merged, scanned | release-4.0 local-variable naming cleanup; no additional convention. |
| 8109 | merged, corroborating | release-4.0 backport of the Qt queued-connection crash fix from MR 8088. |
| 8108 | merged, discussion-focused | Gerald Combs fixes 29West dialog self-deletion with WA_DeleteOnClose; Jim Young verified the crash was no longer reproducible on macOS. |
| 8107 | merged, scanned | automatic data/translation update on master. |
| 8106 | merged, scanned | release-4.0 automatic data update. |
| 8105 | merged, scanned | release-3.6 automatic data update. |
| 8104 | merged, scanned | release-3.4 automatic data update. |
| 8103 | merged, deep | STUN FINGERPRINT over RFC 4571/TCP now hashes only the STUN message, excluding the outer TCP framing-length prefix. |
| 8102 | merged, deep | John Thacker makes a SYN after RST/FIN create a fresh TCP conversation even when its sequence number matches the previous base sequence; without a terminal event the same SYN remains a retransmission. |
| 8101 | merged, high-authority corroboration | release-4.0 backport of Guy Harris's MR 8100 dissector-name/protocol-name distinction. |
| 8100 | merged, deep / high-authority | Guy Harris distinguishes dissector entry-point names, protocol identity, and user-facing long names. Exported-PDU tags must name a callable dissector, and transport-dependent syntax should use transport-specific entry points rather than infer parentage from mutable packet_info/layer history. |
| 8099 | merged, scanned | context-menu cleanup using WA_DeleteOnClose; nearby crash-series context. |
| 8098 | merged, scanned | local-variable naming cleanup on master. |
| 8097 | merged, discussion-focused | heap-allocates FieldInformation with the context menu as QObject parent so queued copy actions retain valid state without leaking it. Martin Mathieson raised the lifetime/leak question; Gerald Combs clarified parent ownership. |
| 8096 | merged, scanned | ROHC comments plus narrow byte-count correction. |
| 8095 | merged, scanned | release-3.4 ISAKMP Fortinet VID backport. |
| 8094 | merged, scanned | release-3.6 ISAKMP Fortinet VID backport. |
| 8093 | merged, scanned | release-4.0 ISAKMP Fortinet VID backport. |
| 8092 | merged, scanned | release-3.4 CT log-list schema backport. |
| 8091 | merged, scanned | release-3.6 CT log-list schema backport. |
| 8090 | merged, scanned | release-4.0 CT log-list schema backport. |
| 8089 | merged, discussion-focused | master CT log-list generator update; Alexis La Goutte explicitly asked whether generated files also needed regeneration, reinforcing generator/output synchronization review. |
| 8088 | merged, deep | accepted master fix for nested-event-loop context-menu crashes: explicit Qt::QueuedConnection defers handlers until menu dispatch unwinds. Gerald also notes typed explicit connections remove connect-by-name warnings. |
| 8087 | merged, scanned | release-4.0 OSCORE cleanup backport. |
| 8086 | merged, scanned | Guy Harris OSCORE cleanup: do not mark a used dissector-data argument unused; define the declared handoff function. |
| 8085 | merged, scanned | release-3.6 dumpcap typo backport. |
| 8084 | merged, scanned | release-4.0 dumpcap typo backport. |
| 8083 | merged, scanned | master dumpcap pcap_geterr string-comparison typo fix. |
| 8082 | merged, scanned | Logray icon/resource addition. |
| 8081 | closed, deep negative/superseded | proposed delaying context-menu deletion directly. Guy Harris and Gerald Combs explored ownership/visibility semantics; Tomasz Mon identified nested processEvents as the real hazard and recommended queued action delivery. Closed in favor of MRs 8088 and 8097. |
| 8080 | merged, scanned | release-note-only OCP.1 entry. |
| 8079 | merged, deep | John Thacker fixes form-urlencoded decoding by passing the actual percent-decoded string length to UTF-8 conversion instead of a wire-offset span. |
| 8078 | merged, corroborating | release-4.0 backport of resolved-address Qt6 model/signal fixes from MR 8072. |
| 8077 | merged, deep | John Thacker makes protocol-tree representation-formatting APIs convert arbitrary formatted data to printable valid UTF-8 and preserve truncation marking. |
| 8076 | merged, scanned | ROHC header cleanup/comments. |
| 8075 | merged, deep | MGCP passes explicit setup metadata into SDP so Osmux signalling can prevent RTP conversation setup. John Thacker also catches an unsafe fixed 256-byte vendor-name buffer and directs use of bounded TVBuff helpers; Jaap Keuter requests a sample capture and one is supplied. |
| 8074 | merged, scanned | release-3.6 backport replacing removed Qt combo-box signal and adding NULL checking. |
| 8073 | merged, scanned | release-4.0 backport of MR 8070. |
| 8072 | merged, substantive | Qt6 resolved-address dialog correction: supported combo signal, source-model ordering, numeric sorting, and semantic address grouping. |
| 8071 | merged, scanned | UDS spelling fix. |
| 8070 | merged, scanned | master Qt6 signal compatibility fix plus NULL guard for RPC lookup. |
| 8069 | closed, scanned/down-weighted | abandoned capture-icon draft; no accepted engineering convention. |
| 8068 | merged, corroborating | release-4.0 NSIS backport removing Qt6 networkinformation/tls DLLs during uninstall. |
| 8067 | merged, scanned | master NSIS uninstall manifest updated for Qt6 DLLs. |
| 8066 | merged, corroborating | release-3.4 backport updating digest algorithms for OpenSSL 3 compatibility. |
| 8065 | merged, corroborating | release-3.6 backport of digest-algorithm CI update. |
| 8064 | merged, corroborating | release-4.0 backport of digest-algorithm CI update. |
| 8063 | merged, scanned | release-3.4 version bump. |
| 8062 | merged, scanned | release-3.6 version bump. |
| 8061 | merged, scanned | master CI removes deprecated RIPEMD160 test coverage and adds SHA512 for OpenSSL 3 compatibility. |

## Promoted durable findings

- MR 8100: a protocol can have multiple callable dissector entry points with different transport contracts. Registry/Exported-PDU metadata that will be consumed by find_dissector must carry the dissector name; UI text should use the relevant protocol/long description. Do not infer transport from the previous packet_info layers entry or other mutable packet-global state when the call path can state it explicitly.
- MR 8102: state keys need lifecycle boundaries, not only tuple/sequence equality. A repeated opener after a terminal event can be a new logical connection even if tuple and initial sequence match old state.
- MRs 8081, 8088, and 8109: nested event loops can process deleteLater before the original QAction/QMenu stack has returned. Queue the action handler when the operation can re-enter the event loop and invalidate participating UI objects.
- MR 8077: framework APIs that build labels/representations may sanitize arbitrary formatted packet-derived bytes to printable valid UTF-8. That is distinct from changing the semantic value of an FT_STRING field.
- MR 8079: after percent-decoding or another transformation, pass the transformed buffer's length to subsequent conversion APIs while retaining the original wire span only for source-range display.
- MR 8075: parent semantic knowledge belongs in an initialized dissector-data structure passed to the child. Review also reinforces bounded TVBuff operations over guessed fixed-size stack buffers for fuzz-controlled input.
- MR 8103: checksum scope follows the semantic protocol message; outer stream-framing bytes are excluded when the specification does not include them.
