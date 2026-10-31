# Wireshark MR automation review ledger — !9061 through !9012

Reviewed **2026-10-31** with **GPT-6** against `dheitmueller/wireshark-corpus-mrs` at exact commit **`ddcaa22b51c68f594e425a23388c3a2086813054`**. Notebook base commit: `c635117e79c273fe62467ac80bcc69565323a725`.

**Exact reviewed membership (descending, 50 distinct):** !9061, !9060, !9059, !9058, !9057, !9056, !9055, !9054, !9053, !9052, !9051, !9050, !9049, !9048, !9047, !9046, !9045, !9044, !9043, !9042, !9041, !9040, !9039, !9038, !9037, !9036, !9035, !9034, !9033, !9032, !9031, !9030, !9029, !9028, !9027, !9026, !9025, !9024, !9023, !9022, !9021, !9020, !9019, !9018, !9017, !9016, !9015, !9014, !9013, !9012.

**Snapshot outcomes:** 45 merged, 3 closed/unmerged, 2 open. The status is historical corpus state, not a claim about current upstream GitLab status. Stable backports and release boilerplate were counted as reviewed but down-weighted for independent engineering conclusions.

## Selection and historical-membership checks

Inspected the notebook root and all 365 filenames under `reviewed-mrs-automation/`. Reconciled the 347 normal range-named ledgers against the 18 unusual, gap, backfill, exact-list, noncontiguous and aggregate trackers; explicitly read the two companion exact-list ledgers for !10813–!10862 and !10963–!11012, rather than counting their placeholder parents. Cross-checked `reviewed-mrs.md` and `reviewed-mrs-automation/reviewed-mrs-automation.md`; neither aggregate mentions a member of !9012–!9061, nor do the exceptional membership records. The fixed corpus has records for all 50 numbers. The previously-reviewed frontier ended at !9062 and !9061 had only been named as the **next candidate**, not as a reviewed entry. The source corpus commit has not changed since the preceding review.

Historical `reviewed-mrs-automation-17571-17620.md` remains untouched: **50 table rows, 50 unique numbers, all !17571–!17620 present**, original blob `9393377c3adeb3a38d502532839765e3abe771fe`.

## Individual diff and discussion assessments

| MR | Corpus outcome | Depth | Assessment |
| --- | --- | --- | --- |
| !9061 | merged | Deep | John Thacker: distinguish absent UMTS FP Payload CRC on an empty payload; use proto_tree_add_checksum status and the reported remaining length. |
| !9060 | merged | Deep | MongoDB Zstd support extracts shared tvb_uncompress_zstd/tvb_child_uncompress_zstd API; Kafka reuses it; multi-frame, malformed and truncated tests plus sample/fuzz captures; Alexis asks for reusable helper and tests. |
| !9059 | merged | Discussion / diff | OAM/DPoE extension; Alexis flags duplicate/conflicting value_string entries, dead stores, and CI failures; corrected before merge. |
| !9058 | merged | Discussion / diff | ASN.1 CMS additions with source/template and generated output; Alexis catches compiler unused-function error. |
| !9057 | merged | Scanned | Release-3.6 backport of master MBIM duplicate map-count correction (!9043). |
| !9056 | merged | Scanned | Release-4.0 backport of MBIM duplicate map-count correction (!9043). |
| !9055 | merged | Scanned | New Qt IO Graph line/step styles; merged after approval workflow. |
| !9054 | merged | Deep | Bundle Protocol v7 source/destination EIDs become address objects; CONVERSATION_BP; visible header and ABI metadata changed; large packet-bpv7.c diff unavailable in snapshot. |
| !9053 | merged | Discussion / diff | Falcodump opens FIFO directly; Gerald drops provisional dummy pcapng SHB workaround after Tomasz notes !9077. |
| !9052 | merged | Scanned | Release-3.6 backport of AsciiDoc leftover POD markup correction. |
| !9051 | merged | Scanned | Release-4.0 backport of AsciiDoc leftover POD markup correction. |
| !9050 | open | Deep | Open UI proposal for field-exists filtering; Roland, Stig, Tomasz and Gerald debate usability, discoverability and context-menu conventions; not accepted. |
| !9049 | merged | Scanned | Debian libwireshark/libwiretap/libwsutil symbols metadata synchronized with exported ABI. |
| !9048 | closed / unmerged | Discussion / diff | Closed Arista AVSP/MetaMako submission; Alexis requests a capture, fixes and separate scope; Jaap asks not to rebase excessively; no accepted implementation. |
| !9047 | merged | Scanned | License checker excludes generated build* directory output rather than obsolete Chromium directories. |
| !9046 | merged | Deep | MSVC Release/RelWithDebInfo warning suppression, _U_ and ws_assert portability; João requests separating Qt deprecation; Gerald asks for actual compiler/environment; merged targeted resolution. |
| !9045 | merged | Scanned | John Thacker exposes H224 as RTP Decode As via dissector_add_for_decode_as(rtp.pt). |
| !9044 | merged | Scanned | Asciidoctor etwdump author markup repaired in master, basis of !9051/!9052. |
| !9043 | merged | Scanned | MBIM removes a duplicate map-count field that shifted subsequent dissection, master basis of !9056/!9057. |
| !9042 | merged | Scanned | Release-3.6 version bump from 3.6.10 to 3.6.11; release bookkeeping. |
| !9041 | merged | Scanned | Release-4.0 version bump 4.0.2 to 4.0.3; release bookkeeping. |
| !9040 | merged | Scanned | Release-3.6.10 build and ChangeLog/NEWS release packaging. |
| !9039 | merged | Scanned | Release-4.0.2 build and ChangeLog/NEWS release packaging. |
| !9038 | merged | Discussion / diff | 802.11 SAE failure Confirm lacks success-only fields; submitted focused four-packet capture. |
| !9037 | open | Discussion / diff | Open SMI UAT per-profile change; Stig notes that profile switching still requires MIB registration restart; not accepted. |
| !9036 | merged | Discussion / diff | Gerald requests PDF fallback font/theme configuration in shared docbook/attributes.adoc; merged. |
| !9035 | merged | Deep | pcapng option UTF-8 validation and owned option strings; Guy/John demand early NUL truncation by pcapng spec before replacement; final patch does not visibly implement this correction. |
| !9034 | merged | Deep | BER unknown extension advances using returned length offset and computed span, avoiding conflation of adjacent extensions. |
| !9033 | merged | Scanned | Release-3.6 backport of Qt toUtf8() fix for packet comments. |
| !9032 | merged | Scanned | Release-4.0 backport of Qt toUtf8() fix for packet comments and UI boundaries. |
| !9031 | merged | Scanned | PCEP IANA object classes and decoded stubs expanded; no additional human-review convention. |
| !9030 | merged | Discussion / diff | Merged master successor of closed release-3.6 RPM %{?dist} proposal !9012. |
| !9029 | merged | Scanned | Release-4.0 backport of optional Nghttp2-aware Follow HTTP/2 test skip. |
| !9028 | merged | Discussion / diff | John Thacker: skip HTTP/2 Follow test when optional Nghttp2 header decompression unavailable. |
| !9027 | merged | Deep | John Thacker optimizes URL-encoded parsing using tvb_ws_mempbrk_pattern_guint8, tvb_find_guint8, bounded subsets and tvb_memcpy spans; keeps malformed escape checks. |
| !9026 | merged | Deep | USB control/bulk/interrupt tables expand to FT_UINT32 selectors with disjoint class/subclass/protocol key; preserve URB-specific state during conversation replacement. |
| !9025 | merged | Scanned | Release-3.6 fuzz-pass/Valgrind duration trimmed to free resources for master/release-4.0. |
| !9024 | merged | Deep | Gerald Combs replaces Qt toLocal8Bit with toUtf8 for pcapng packet comments; Guy Harris approves and requests stable backports. |
| !9023 | merged | Scanned | Release-3.6.10 release-note prep; security and bug-fix bookkeeping. |
| !9022 | merged | Scanned | Release-4.0.2 release-note prep; security and bug-fix bookkeeping. |
| !9021 | merged | Discussion / diff | Release-3.6 Kafka decompression guards: bounded iterations and buffer retention/cleanup; historical code later superseded by shared Zstd TVB helper. |
| !9020 | merged | Discussion / diff | Release-4.0 Kafka decompression guards: bounded iterations and buffer retention/cleanup; historical code later superseded by shared Zstd TVB helper. |
| !9019 | merged | Scanned | Qt action objectName synchronized after rename of Apply as Column action. |
| !9018 | closed / unmerged | Deep | Closed Windows CMake auto-default proposal; João and Gerald reject hiding VS platform mismatch and putting downloaded support libs under transient build directory. |
| !9017 | merged | Scanned | Release-4.0 backport of DTLS Connection ID/block-cipher MAC correction. |
| !9016 | merged | Discussion / diff | DTLS Connection ID supports block-cipher MAC in master, basis of !9017. |
| !9015 | merged | Scanned | Release-3.6 Kafka Zstd loop exits when input has been consumed, avoiding an unnecessary iteration. |
| !9014 | merged | Scanned | Release-4.0 Kafka Zstd loop exits when input has been consumed, avoiding an unnecessary iteration. |
| !9013 | merged | Deep | Merged MP-DCCP support; Alexis requires collision-free hf fields, clean checks, six example captures and fuzz test evidence (20k passes, ~24h). |
| !9012 | closed / unmerged | Discussion / diff | Closed RPM rename proposal targeted release-3.6; Gerald requests master-first; accepted implementation submitted as !9030. |

## Durable findings and evidence weighting

**pcapng and text encodings (!9035, !9024, !9032, !9033).** Guy Harris quotes the pcapng option rule: string values need not be terminated, but an embedded zero octet must be treated as the terminator. John Thacker notes that direct `g_utf8_make_valid()` on a counted option with an embedded NUL replaces that octet instead of truncating. The merged !9035 final diff visibly repairs invalid UTF-8 and introduces an owned-value helper, **but does not visibly perform Guy's recommended early-NUL `memchr` trimming**; preserve this as an unresolved caution, not a final behavioral exemplar. Separately Gerald Combs's merged !9024 and two release backports replace locale-dependent Qt `toLocal8Bit()` with `toUtf8()` for packet comments, with Guy's approval.

**TVBuff and decompression (!9060, !9021, !9020, !9015, !9014).** Alexis La Goutte requested a reusable TVBuff Zstd helper; merged !9060 adds standalone/child uncompression APIs, updates MongoDB and replaces Kafka's custom decoder, and tests good, malformed, truncated and multi-frame data. Old release-branch Kafka safety fixes bound iteration and stop at actual input exhaustion, but their manual composite design should not be copied over the later shared helper. The parent/child lifetime distinction is corroborated by !9236.

**Conversation and extensible dispatch (!9054, !9026).** Merged BPv7 code promotes endpoint identifiers to typed `address` objects and adds a `CONVERSATION_BP` enum, acknowledging that a protocol can define identities outside a transport five-tuple. The large `packet-bpv7.c` diff is absent in this corpus record; the assessment is based on visible header/ABI changes and description. Merged USB changes give `usb.control`, `usb.bulk`, and `usb.interrupt` a 32-bit selector with a high-bit protocol key for class/subclass/protocol triples, while retaining the class-only dispatch and copying URB-specific context when the conversation object changes.

**Parser and test discipline (!9027, !9034, !9013, !9028).** John Thacker's URL-encoded parser scans/bulk-copies validated stretches with TVBuff helpers instead of a byte-by-byte path, maintaining delimiter and escape bounds. BER !9034 uses the returned extension-length cursor and explicit covered span. Alexis La Goutte requested sample captures and fuzzing for MP-DCCP !9013; the author supplied six categorized pcaps and reported more than 20,000 passes and a 24-hour clean run. John Thacker's !9028 makes an HTTP/2 Follow test conditional on Nghttp2 availability rather than asserting feature-independent expected output.

**Review and submission boundaries (!9046, !9018, !9048, !9012, !9030, !9037, !9050).** Gerald Combs and João Valverde explicitly rejected a Windows CMake fallback that could mask an x86/x64 Visual Studio environment mismatch; support libraries are not build artifacts and should not default to a transient build directory. !9046 shows target-specific warning diagnosis and separating an unrelated Qt deprecation. Jaap Keuter requested limiting unnecessary rebases in closed !9048; Alexis requested a capture and separation of MetaMako work. Gerald requested master-first for RPM %{?dist} rather than release-branch package renames (!9012 → merged !9030). Unmerged UI/profile proposals !9050 and !9037 provide review considerations, **not** accepted behavior.

**Validation scope:** historical corpus diff/discussion inspection only. No builds, capture playback, runtime testing or fuzzing executed in this review.

**Next expected candidate:** !9011, subject to the next run's full exact-membership reconciliation and corpus inventory.
