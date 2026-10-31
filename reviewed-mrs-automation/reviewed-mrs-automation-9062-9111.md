# Wireshark MR automation review ledger — !9111 through !9062

Reviewed **2026-10-31** using **GPT-6** against `dheitmueller/wireshark-corpus-mrs`, commit **`ddcaa22b51c68f594e425a23388c3a2086813054`**. Notebook starting HEAD: `67247163d607bdc6957a8a7b30adac7f0c655944`.

**Exact reviewed membership (descending, 50 distinct):** !9111, !9110, !9109, !9108, !9107, !9106, !9105, !9104, !9103, !9102, !9101, !9100, !9099, !9098, !9097, !9096, !9095, !9094, !9093, !9092, !9091, !9090, !9089, !9088, !9087, !9086, !9085, !9084, !9083, !9082, !9081, !9080, !9079, !9078, !9077, !9076, !9075, !9074, !9073, !9072, !9071, !9070, !9069, !9068, !9067, !9066, !9065, !9064, !9063, !9062.

**Outcomes:** 50 merged, 0 closed/unmerged, 0 open in the pinned historical corpus. Outcomes do not assert current GitLab MR status. Backports are counted as separate reviewed MR numbers but down-weighted as independent conventions.

## Selection and prior-membership verification

Inventoried **364 existing per-run files** under `reviewed-mrs-automation/` and inspected the prior frontier's exact !9112–!9161 ledger, exceptional/noncontiguous/backfill/gap ledger files, both `reviewed-mrs.md` and the supplemental aggregate ledger. The preceding run had explicitly reconciled its then-363 tracking files; it had not reviewed !9111. Existing per-run ledger filename lower boundaries begin at !9112, but ordinary numeric filenames alone were **not** used to infer membership: examined the exceptional ledgers and the aggregate records for the actual reviewed entries. No actual existing review row or exact-membership declaration contained a selected number !9062–!9111. The one !9111 mention in the preceding ledger is expressly a **future candidate**, not a completed review. The corpus commit is unchanged, and all 50 selected JSON records were individually fetched at that commit; available discussions and diffs were examined, with deeper attention to maintainer reviews and behavior-changing patches.

The original historical `reviewed-mrs-automation/reviewed-mrs-automation-17571-17620.md` was separately read and confirmed to preserve **all 50 distinct !17571–!17620 table entries**, unchanged. Neither aggregate ledger nor historical files were edited.

## Exact individual assessments

| MR | Corpus outcome | Depth | Diff/discussion assessment |
| --- | --- | --- | --- |
| !9111 | merged | Deep | Release-3.6 backport of !9093: John Thacker widens UMTS RLC fragment offsets to 16 bits for PDUs >255 bytes; avoids wrapping into a spurious extra fragment. |
| !9110 | merged | Scanned | Gerald Combs adds an optional Wireshark Foundation donation page to the NSIS install flow; packaging/presentation only. |
| !9109 | merged | Scanned | Release-3.6 backport of !9104: NAS-5GS CAG Information List is an extended TLV, not ordinary TLV. |
| !9108 | merged | Deep | João Valverde synchronizes malformed UTF-8 diagnostic output and underlines by rendered escape width; bounded source lengths and wmem string-buffer repeat-append test. |
| !9107 | merged | Scanned | Release-4.0 backport of !9104: NAS-5GS CAG Information List uses TLV_E per 3GPP. |
| !9106 | merged | Deep | Configurable 400ms UI input debounce; Martin Mathieson examines default, consumer scope and OK-to-apply semantics of a preference affecting its own dialog. |
| !9105 | merged | Discussion/diff | Qt Conversations crash: blank-area context menu yields row -1, so model storage lookup must reject negative and too-large rows. |
| !9104 | merged | Discussion/diff | Pascal Quantin accepts master CAG list parser correction from TLV to TLV_E per 3GPP TS 24.501; basis for !9107/!9109. |
| !9103 | merged | Discussion/diff | Release-3.6 backport of Gerald Combs NCP checked offset advance and malformed Expert Info; generator and emitted include kept aligned. |
| !9102 | merged | Discussion/diff | Release-4.0 backport of NCP offset-overflow protection !9092; generator and generated dissector updated. |
| !9101 | merged | Discussion/diff | Debug-only UTF-8 validation becomes conditional on Debug/explicit CMake option rather than default Release; version text exposes assertion/validation switches. |
| !9100 | merged | Deep | Cross-dissector encoding repair: wire bytes are not automatically printable UTF-8; use format_char, encoded tvb helpers and bounded growable strings. |
| !9099 | merged | Discussion/diff | Release-4.0 backport of OCP.1 variable-length notification context parsed as OcaBlob/FT_BYTES, preserving ocp1.context abbreviation. |
| !9098 | merged | Discussion/diff | OCP.1 notification context is length-prefixed bytes, not four-byte integer; use nested OcaBlob decoder and retain the existing filter name. |
| !9097 | merged | Scanned | Release-4.0 backport of !9093, widening UMTS RLC reassembly offsets past 255 bytes. |
| !9096 | merged | Scanned | Developer/User Guides use correct GitLab capitalization; straightforward documentation cleanup. |
| !9095 | merged | Scanned | Developer Guide removes outdated Gerrit reference; submission documentation maintenance. |
| !9094 | merged | Deep | John Thacker clears ASN.1 QCStatement global object_identifier_id before potentially failing BER; edit .cnf plus generated C to avoid stale state. |
| !9093 | merged | Deep | Master John Thacker UMTS RLC offset-width correction: guint8 wraps on larger AMD/UMD PDUs; guint16 propagates through parser/reassembly functions. |
| !9092 | merged | Deep | Master Gerald Combs NCP check_offset_addition plus malformed Expert Info prevents offset overflow, with authoritative Python generator and generated include updates. |
| !9091 | merged | Deep | Extcap immutable multicheck informational nodes render as labels, not unchecked choices. Guy Harris confirms semantics; Roland Knall objected after unresolved UI thread was merged. |
| !9090 | merged | Deep | SIP CSeq UTF-8 extraction avoids fixed-buffer truncation but initially stores packet-owned pointer in transaction state; later ASAN UAF corrected by merged !9114. |
| !9089 | merged | Discussion/diff | ciscodump Coverity fix initializes bytes_written; author notes squash lost detailed Coverity CID from commit description. |
| !9088 | merged | Scanned | SCTP may be Decode-As-selected under DTLS via registration at dtls.port; small integration change. |
| !9087 | merged | Deep | BGP searches marker with tvb_find_guint16/tvb_memeql then passes marker-aligned subset to tcp_dissect_pdus instead of custom reassembly loop. |
| !9086 | merged | Deep | Martin Mathieson adds opt-in --mask-exact-width FT_BOOLEAN warning; Jaap questions mandatory padding; 564 legacy warnings remain; real masks corrected. |
| !9085 | merged | Deep | QUICv2 final draft/RFC constants, salt and version negotiation updated together; author supplies sample capture; Alexis later requests stable backport. |
| !9084 | merged | Scanned | Release-3.6 backport of !9082: BPv6 invalid extension stops parsing and returns absolute offset, preventing infinite loop. |
| !9083 | merged | Scanned | Release-4.0 backport of BPv6 malformed-offset/termination fix !9082. |
| !9082 | merged | Deep | John Thacker fixes BPv6 error branch returning bytes remaining as offset; signals last header and returns absolute current offset to terminate. |
| !9081 | merged | Deep | Gerald Combs fixes !9077 macOS FIFO teardown hang: O_WRONLY needs O_NONBLOCK when no extcap reader exists. |
| !9080 | merged | Scanned | ciscodump adds IOS XE 17.x remote CLI handling and release notes; vendor-version compatibility. |
| !9079 | merged | Scanned | Release-3.6 backport consolidates Chocolatey CMake and winflexbison install in Windows GitHub CI. |
| !9078 | merged | Scanned | Release-4.0 backport consolidates Windows CMake/winflexbison CI installation. |
| !9077 | merged | Deep | Opening/closing an extcap FIFO to unblock dumpcap can itself block without a reader; Gerald identifies macOS regression and fixes in !9081. |
| !9076 | merged | Discussion/diff | John Thacker uses tvb_format_text for four-byte USB-video FourCC instead of treating arbitrary bytes as safe %s UTF-8. |
| !9075 | merged | Discussion/diff | ROHC IP profile recognizes alternate IP version encoding; Martin questions static-chain semantics and requests alignment; Pascal explains RFC 3843. |
| !9074 | merged | Scanned | macOS packaging comment ties libbrotli install-name workaround specifically to upstream versions <=1.0.9. |
| !9073 | merged | Deep | DNS RR-type descriptions appended to numeric tree labels rather than hiding canonical typed fields; John flags filters/Apply-as-Column usability. |
| !9072 | merged | Deep | USB CCID level parameter corrected from FT_UINT8 to FT_UINT16; reviewers discuss readable FT_BOOLEAN bitmask width and future checker. |
| !9071 | merged | Deep | GRE Decode As adds payload fallback; John Thacker advises passing &gre_hdr_info via dissector_try_payload_new and questions alternate primary-table design. |
| !9070 | merged | Scanned | Weekly master vendor/enterprise registry and NEWS synchronization; largely generated maintenance, no separate convention. |
| !9069 | merged | Scanned | Weekly release-4.0 autogenerated registries, translated resources and help output snapshots; duplicate maintenance evidence. |
| !9068 | merged | Scanned | Weekly release-3.6 autogenerated registry/help output refresh; no new implementation convention. |
| !9067 | merged | Deep | Gerald Combs replaces temp regular-file/unlink/FIFO reuse race with unique temporary directory and FIFO inside it; sanitize names and clean both. |
| !9066 | merged | Scanned | Release-3.6 backport of USB Darwin URB source/destination and locationID offset correction. |
| !9065 | merged | Scanned | Release-4.0 backport of USB Darwin URB submit/completion and locationID offset correction. |
| !9064 | merged | Discussion/diff | Tomasz Moń fixes Darwin USB capture metadata: URB direction byte offset 3 and locationID offset 24 for correct host/source/bus. |
| !9063 | merged | Discussion/diff | Guy Harris documents Lua Proto init callback on capture open/close and prefs_changed on preference updates; important lifecycle distinction. |
| !9062 | merged | Deep | Darwin pcapng Wake Packet flag and Flow ID option parsed; Martin requests capture for future private-block registration example; Alexis asks spec reference. |

## Durable synthesis and cautions

- **Follow corrective chains, not just merge state.** Initial merged SIP !9090 retained a packet-allocated string in file-scoped state, later repaired by John Thacker's merged ASAN fix !9114; initial merged FIFO wakeup !9077 deadlocked on macOS and Gerald Combs's merged !9081 adds nonblocking open. Both initial commits are negative examples, not final design models.
- **Human maintainer guidance matters.** Guy Harris distinguishes read-only extcap informational labels from actual capture choices in !9091 and John Thacker specifies preserving context across Decode As fallback in !9071, keeping canonical DNS fields usable for columns in !9073 and correcting memory lifetime via !9114. Martin Mathieson and Jaap Keuter's checker discussions in !9086 mean an optional exact-width cosmetic warning must not be mistaken for mandatory policy.
- **Defensive parsing:** Gerald Combs's generator-based NCP check (!9092) and John Thacker's BPv6 offset contract (!9082), UMTS RLC wider offsets (!9093) and ASN.1 state reset (!9094) support concrete safeguards for arithmetic, progress and initialization.
- **Bigger changes:** BGP !9087 delegates TCP message framing to `tcp_dissect_pdus`; 9108 tracks invalid UTF-8 display/underline widths; 9073 separates canonical numeric values from display prose. For 9062 a maintainer requested a sample capture; no delivered file was established from this snapshot.
- **Submission:** !9089 documents lost Coverity traceability from an ill-described squash commit; it does not establish an anti-squash policy.

**Test scope:** this was a historical corpus discussion/diff review and notebook documentation pass. No build, runtime test, capture replay or fuzzing was executed.

**Next expected candidate:** !9061, pending verification of newly available corpus entries and complete existing-ledger membership in the next run.
