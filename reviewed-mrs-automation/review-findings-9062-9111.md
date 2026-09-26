# Wireshark MR Review Findings: 9062-9111

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Exactly 50 previously unreviewed merge requests were examined, newest to oldest. Selection was checked by individual MR identity against `reviewed-mrs.md`, supplemental automation tracking, and the available per-run ledgers. The historical 17571-17620 batch was revalidated as exactly 50 unique tracked MRs. All 50 MRs in this run are merged.

## Strong durable findings

### !9091: information should not masquerade as an option

Tomasz Moń explained that the permanently non-checkable extcap rows are descriptive device information rather than capture options. Guy Harris explicitly agreed with that semantic distinction and with label-like presentation, while separately asking whether the information might be more discoverable elsewhere. This is recorded in `qt-widget-state-conventions.md`.

The MR also contains qualified process evidence: Roland Knall objected after merge that an unresolved design discussion had not actually been resolved. That does not reverse Guy's semantic clarification, but it is a useful reminder not to infer agreement merely from a gap in reviewer responses.

### !9086 and !9072: checker severity should follow semantic consequence

Martin Mathieson's !9086 distinguishes real registered-width problems from the optional readability preference of padding an FT_BOOLEAN mask with leading zeroes to its full width. Jaap Keuter questioned enforcing the latter, and the accepted checker leaves that exact-width check outside normal pipeline use. !9072 independently shows the checker finding a real USB-CCID field-width typo. This corroboration was added to `source-checker-conventions.md`.

### !9082-!9084: length and offset are different coordinate systems

John Thacker's !9082 fixes BPv6 code that assigned a remaining byte count to an absolute parser cursor. The returned count could be lower than the current cursor and make the outer loop revisit data. The accepted code returns the current absolute offset and marks the extension terminal; both stable backports merged. This is now explicit in `dissector-consumption-boundary-conventions.md`.

### !9071: Decode As must preserve required parent context

During GRE Decode As review, Alexis La Goutte asked how to preserve `gre_hdr_info`. John Thacker pointed to `dissector_try_payload_new()`, which accepts the parent data pointer, and the merged code uses it. John also suggested checking whether an existing keyed table can expose Decode As directly before creating a parallel fallback table. The resulting rule is recorded in `decode-as-conventions.md`.

### !9067, !9077, !9081: temporary IPC endpoints and shutdown wakeups

Gerald Combs's !9067 replaces temp-file pathname reuse with a dedicated temporary directory containing the extcap FIFO. !9077 then added an open/close wakeup during shutdown, but Gerald reproduced a macOS hang when the FIFO writer open waited for a reader; !9081 immediately changed it to nonblocking mode. These lifecycle lessons are recorded in `ipc-endpoint-lifecycle-conventions.md`.

### !9108, !9100, !9076: packet text and diagnostic display

João Valverde's !9108 separates source traversal from rendered diagnostic width so escaped malformed UTF-8 and its underline remain synchronized. !9100 fixes packet-derived text across several dissectors using printable/escaped forms and encoding-aware extraction. John Thacker's !9076 independently replaces a raw FourCC C-string copy with `tvb_format_text()`. These were added to `text-encoding-conventions.md`.

!9090 is useful but must be read with later evidence: it removes SIP's fixed CSeq method buffer and extracts UTF-8 directly, solving truncation, but the resulting retained pointer lifetime was later corrected by the already-reviewed !9114. The later fix remains the authoritative lifetime model.

### !9092-!9103 and !9094: generated-source discipline

Gerald Combs's NCP master change !9092 modifies the generator and regenerated include together, with release backports !9102 and !9103. !9094 independently changes PKIX Qualified ASN.1 conformance input and generated C together. Both reinforce the source-of-truth rules in `generated-code-conventions.md`.

### !9087: use the common TCP PDU machinery

Merged !9087 replaces a large hand-written BGP TCP message loop with `tcp_dissect_pdus()` and replaces repeated copies with TVBuff search/compare helpers. This strongly corroborates the existing transport-framing rule that TCP protocols with determinable message lengths should use Wireshark's common desegmentation/PDU machinery.

### !9085: wait for finalized protocol constants, then backport interoperability fixes when needed

Ivan Nardi kept the QUICv2 constants work draft while the version-information parameter assignment was unsettled, then supplied a focused capture after final values were known. Once standardized QUICv2 traffic was not recognized by the maintained release, Alexis La Goutte requested a backport and Ivan verified it was clean. This is useful standards-evolution evidence but did not require a new notebook file.

### !9093, !9097, !9111: parser intermediates must span the protocol domain

John Thacker's UMTS RLC master fix widens reassembly/status offsets because valid AMD/UMD PDUs can exceed 255 bytes; otherwise the cursor wraps and the tail is treated as an extra fragment. Both release backports merged. This corroborates existing parser arithmetic/domain guidance.

### !9073: field presentation has generic UI consumers

John Thacker noted that hiding a DNS field may leave display-filter use possible while making right-click “Apply as Column” awkward. The accepted revision keeps the type field useful for generic UI workflows and applies the approach consistently to related DNS type fields.

## Qualified evidence

- !9089 records Dario Lombardo's complaint that a squash lost a Coverity identifier from the original commit message. The provenance concern is retained, but it is contributor opinion rather than an accepted project-wide ban on squashing.
- !9106 contains Martin Mathieson's questions about debounce defaults, naming/scope, and the fact that a preference affecting the Preferences dialog itself does not take effect until the dialog is accepted. Useful UI/configuration evidence, not promoted to a new rule.
- !9062 contains Martin Mathieson's request for a representative pcap and his stated desire for an in-tree private pcapng extension registration example. The MR merged with the direct implementation, so the proposed extension architecture remains future-direction discussion rather than accepted architecture from this MR.
- !9063, authored by Guy Harris, documents that WSLua `Proto.init` runs when a capture is opened or closed and `Proto.prefs_changed` runs when protocol preferences change. Strong API documentation evidence, but narrow.
- !9064 with Guy Harris-authored backports !9065 and !9066 corrects Darwin USB direction/header-offset handling. Strong implementation confidence, but protocol-format-specific.

## Per-MR inventory

| MR | Outcome | Depth | Review result |
|---|---|---|---|
| !9111 | merged | backport | release-3.6 UMTS RLC offset-width correction. |
| !9110 | merged | scanned | NSIS donation page; no cross-cutting engineering rule. |
| !9109 | merged | backport | release-3.6 NAS-5GS CAG Information List TLV-E correction. |
| !9108 | merged | deep | UTF-8 diagnostic source/display resynchronization. |
| !9107 | merged | backport | release-4.0 NAS-5GS CAG Information List TLV-E correction. |
| !9106 | merged | discussion | configurable GUI debounce timer; default, scope, and apply timing discussed. |
| !9105 | merged | scanned | Conversations model rejects negative row before indexing. |
| !9104 | merged | master | CAG Information List changed from TLV to TLV-E. |
| !9103 | merged | backport | release-3.6 NCP parser arithmetic correction from generated source. |
| !9102 | merged | backport | release-4.0 NCP parser arithmetic correction from generated source. |
| !9101 | merged | scanned | UTF-8 diagnostic checks become Debug/opt-in and build capabilities are reported. |
| !9100 | merged | deep | cross-dissector invalid-text cleanup with encoding-aware formatting. |
| !9099 | merged | backport | release-4.0 OCP.1 notification context parsing correction. |
| !9098 | merged | master | OCP.1 notification context parsing correction. |
| !9097 | merged | backport | release-4.0 UMTS RLC offset-width correction. |
| !9096 | merged | scanned | documentation consistently capitalizes GitLab. |
| !9095 | merged | scanned | documentation removes obsolete Gerrit reference. |
| !9094 | merged | deep | PKIX Qualified conformance source and generated dissector changed together. |
| !9093 | merged | deep | UMTS RLC offset widened for valid large PDUs. |
| !9092 | merged | deep | NCP generated dissector correction made through generator source. |
| !9091 | merged | deep | extcap informational rows restored to label-like presentation; Guy Harris clarification. |
| !9090 | merged | qualified | SIP fixed string buffer removed; later !9114 remains authoritative for retained lifetime. |
| !9089 | merged | discussion | squashing lost Coverity provenance; no universal no-squash rule extracted. |
| !9088 | merged | scanned | SCTP registered for DTLS dispatch. |
| !9087 | merged | deep | BGP hand-written TCP PDU loop replaced with `tcp_dissect_pdus()`. |
| !9086 | merged | deep | typed-item checker separates real width problems from optional exact-width formatting. |
| !9085 | merged | discussion | QUICv2 final constants, focused capture, and stable backport discussion. |
| !9084 | merged | backport | release-3.6 BPv6 loop correction. |
| !9083 | merged | backport | release-4.0 BPv6 loop correction. |
| !9082 | merged | deep | BPv6 remaining-length/absolute-offset confusion corrected. |
| !9081 | merged | corrective | extcap FIFO wakeup made nonblocking after macOS hang. |
| !9080 | merged | scanned | ciscodump support for IOS XE 17.x. |
| !9079 | merged | backport | release-3.6 Windows CI package install cleanup. |
| !9078 | merged | backport | release-4.0 Windows CI package install cleanup. |
| !9077 | merged | qualified | initial extcap FIFO wakeup, immediately corrected by !9081. |
| !9076 | merged | deep | USB Video FourCC uses text-aware TVBuff formatting. |
| !9075 | merged | discussion | ROHC alternate IP-version encoding; reviewer checked RFC semantics. |
| !9074 | merged | scanned | macOS libbrotli packaging comment clarification. |
| !9073 | merged | discussion | DNS field presentation revised for generic filter/column usability. |
| !9072 | merged | deep | checker finds real USB-CCID field-width issue and drives mask audit. |
| !9071 | merged | deep | GRE Decode As preserves parent header context. |
| !9070 | merged | automation | master automatic data/registry update. |
| !9069 | merged | automation | release-4.0 automatic data/release update. |
| !9068 | merged | automation | release-3.6 automatic data/release update. |
| !9067 | merged | deep | extcap FIFO created in dedicated temporary directory. |
| !9066 | merged | backport | Guy Harris-authored release-3.6 Darwin USB correction. |
| !9065 | merged | backport | Guy Harris-authored release-4.0 Darwin USB correction. |
| !9064 | merged | master | Darwin USB URB direction and location-field offsets corrected. |
| !9063 | merged | high-authority author | Guy Harris documents WSLua init and preference callback timing. |
| !9062 | merged | discussion | Darwin pcapng Flow ID/Wake flag; capture and future registration architecture discussed. |

## Notebook files updated

- `text-encoding-conventions.md`
- `source-checker-conventions.md`
- `generated-code-conventions.md`
- `decode-as-conventions.md`
- `dissector-consumption-boundary-conventions.md`
- `qt-widget-state-conventions.md`
- `ipc-endpoint-lifecycle-conventions.md`
