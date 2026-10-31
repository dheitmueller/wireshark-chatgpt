# Wireshark MR automation review ledger — !9161 through !9112

Reviewed **2026-10-31** against `dheitmueller/wireshark-corpus-mrs` at **commit `ddcaa22b51c68f594e425a23388c3a2086813054`**.
Notebook starting head was `da85a17d459cdbb20e4d207370eefd4a894f6b21`. These outcomes are historical corpus snapshot states; no live GitLab status is implied.

**Exact reviewed membership (descending; 50 distinct):** !9161, !9160, !9159, !9158, !9157, !9156, !9155, !9154, !9153, !9152, !9151, !9150, !9149, !9148, !9147, !9146, !9145, !9144, !9143, !9142, !9141, !9140, !9139, !9138, !9137, !9136, !9135, !9134, !9133, !9132, !9131, !9130, !9129, !9128, !9127, !9126, !9125, !9124, !9123, !9122, !9121, !9120, !9119, !9118, !9117, !9116, !9115, !9114, !9113, !9112.

**Outcomes:** 45 merged, 3 closed without merge (!9160, !9140, !9127), and 2 open (!9131, !9117). The exact number in each row is the membership authority, not a filename range.

## Selection and tracking verification

Before selection, inventoried and read all **363** existing files in `reviewed-mrs-automation/`, plus root `reviewed-mrs.md`. Reviewed noncontiguous, exceptional, backfill and exact-list tracking rather than assuming numeric ranges from filenames. No row recorded any MR in this batch as previously reviewed. Only two textual cross-references were found: !9161 as the previous ledger's *next candidate*, and !9140 as historical predecessor in the !11766 discussion. Neither was counted as reviewed. Each of the 50 selected corpus JSON records exists and its metadata, discussions and diff were inspected; substantive cases were examined more closely while routine/generated/backport changes were scanned.

The historical `reviewed-mrs-automation-17571-17620.md` ledger was re-read and verified to contain **exactly 50 distinct table entries, every number !17571–!17620**. No prior ledger was modified.

## Exact individual assessments

| MR | Corpus outcome | Depth | Diff/discussion assessment |
| --- | --- | --- | --- |
| !9161 | merged | Deep | Display-filter unary minus now checks ftype_can_unary_minus before constant folding, reporting a semantic error instead of crashing. |
| !9160 | closed | Discussion | Lua ProtoField 64-bit mask proposal was abandoned after !3686 merged; Anders requested closing the duplicate. |
| !9159 | merged | Scanned | UAT dialog removes a debug FIX reminder from normal runtime output; no new design rule. |
| !9158 | merged | Scanned | WLAN fixes duplicate filter abbreviations and a noncontiguous 0x00F1 bitmask identified by typed-item checker. |
| !9157 | merged | Scanned | PDCP-NR adds a filterable decrypted-data field and distinguishes first-pass UE security state from revisits. |
| !9156 | merged | Scanned | BSD setup promotes speexdsp from optional to required dependency. |
| !9155 | merged | Scanned | WLAN resolves conflicting value_string numeric key 4 by correcting one constant to 3. |
| !9154 | merged | Scanned | Qt font preference reader handles both ten- and eleven-field legacy Qt5 strings; stable backport. |
| !9153 | merged | Deep | John batches valid UTF-8 spans; Guy identifies MSVC signedness warning and analyzes portable endian loads; experimental wide scans remain separate. |
| !9152 | merged | Scanned | Qt master-side compatibility fix for Qt5 versus Qt6 serialized QFont field counts. |
| !9151 | merged | Scanned | Debian setup promotes libspeexdsp-dev to required package to match configure dependency. |
| !9150 | merged | Scanned | TLS CT registry generator compares semantic content separately from volatile Last-Modified timestamps. |
| !9149 | merged | Deep | Moves dependent frames from disposable packet_info into persistent frame_data for transitive filtered-save traversal; older regression/fix lineage matters. |
| !9148 | merged | Scanned | Automated registry and translation data refresh, including services after an earlier failed generation. |
| !9147 | merged | Scanned | Earlier automated registry refresh with services generation failure noted; routine generated data. |
| !9146 | merged | Scanned | Routine automated USB/PCI/TLS CT registry refresh. |
| !9145 | merged | Deep | TCP conversation base sequence and stream identity are initialized even with sequence-number analysis disabled. |
| !9144 | merged | Scanned | JSON compact tree creates nested array subtrees and correct element indices for multidimensional arrays. |
| !9143 | merged | Deep | Bounded packet-list QCache prevents unbounded column-string retention; review traces sorting cost and later !9222 budget. |
| !9142 | merged | Scanned | Vagrant development environment updated to Ubuntu 22.04 and 8 GB RAM for parallel builds. |
| !9141 | merged | Deep | QUIC tracks multiple logical connections per UDP five-tuple, uses CID-specific state and handles migration/stateless reset. |
| !9140 | closed | Discussion | Guy and Anders oppose adding a preference for pcapng ordinal block numbers; superseded by merged !11766. |
| !9139 | merged | Scanned | IPP date/time formatting sanitizes malformed timezone-sign bytes to avoid invalid UTF-8. |
| !9138 | merged | Discussion | H.224 CME/H.281 dissector; Alexis checks display-filter name consistency, and reviewer requests common T.35 lookup. |
| !9137 | merged | Scanned | FT_ETHER custom columns honor MAC resolution settings across display-label and preferences paths. |
| !9136 | merged | Scanned | Spelling corrections across dissectors, documentation, and project dictionary. |
| !9135 | merged | Deep | RTPS overflow-hardening backport adds checked offset advancement with expert diagnostic and bounds exception. |
| !9134 | merged | Scanned | RTPS overflow-hardening release backport with checked offset advancement. |
| !9133 | merged | Deep | CLI profile bootstrap copies all source files when profile file-hash state is not yet initialized. |
| !9132 | merged | Scanned | Corrects UTF-8 truncation and related GSM/LZ77 bounds issues; never truncate multibyte text mid-sequence. |
| !9131 | opened | Scanned | Unmerged draft delegates Protobuf-to-JSON rendering to JSON dissector using child TVB; not an accepted design. |
| !9130 | merged | Deep | Protobuf JSON base64 writer keeps begin/end transitions balanced even for zero-length bytes; same change as !9126. |
| !9129 | merged | Scanned | Protobuf Timestamp display respects preferences and UTC/local formatting in stable branch. |
| !9128 | merged | Scanned | NBAP stable backport includes CommunicationContextID/UEID in all HS-DSCH flow binding paths. |
| !9127 | closed | Scanned | Duplicate Qt 6.4 QMouseEvent fix; maintainer reports it already corrected. |
| !9126 | merged | Deep | Master-side Protobuf empty-bytes JSON state fix; balances base64 transitions and exposes diagnostic suppression flag. |
| !9125 | merged | Deep | SpeexDSP migration removes bundled resampler and updates CMake, Windows dependency bundle, NSIS/WiX and documentation. |
| !9124 | merged | Deep | Master RTPS offset-overflow fix checks G_MAXUINT32 - value and raises ReportedBoundsError; later backports !9134/!9135. |
| !9123 | merged | Deep | SNMP private conversations must not be confused with UDP dispatch; John notes generated ASN.1 overwrite and later !11629 repair. |
| !9122 | merged | Scanned | Qt 6.4 QMouseEvent deprecation fix carried to stable branch. |
| !9121 | merged | Scanned | Packet-comment shortcut created eagerly so it works before menu first opens; applied to Wireshark and Logray. |
| !9120 | merged | Scanned | Traffic-dialog CSV/YAML/JSON copy respects filtered/visible proxy rows, not the unfiltered source model. |
| !9119 | merged | Scanned | JSON dissector provides selectable raw-form tree presentation and release-note entry. |
| !9118 | merged | Scanned | Qt 6.4 QMouseEvent new constructor adopted on master. |
| !9117 | opened | Discussion | Stig objects to globally forbidding leading-dot profile names because existing Unix profiles would break; import/export warnings discussed. |
| !9116 | merged | Scanned | CMake comment clarifies compiler-version/optimization dependent stringop-overflow warnings with narrowly scoped no-error policy. |
| !9115 | merged | Scanned | NBAP master fix supplies CommunicationContextID/UEID to every HS-DSCH flow binding and generated template. |
| !9114 | merged | Deep | ASan SIP use-after-free fixed by duplicating transaction method into file scope before retaining pointer. |
| !9113 | merged | Scanned | SGsAP MME name uses ENC_APN_STR to decode network-encoded APN text. |
| !9112 | merged | Scanned | Falco extcap fixes GCC 11 C++ warnings: aggregate initialization and set element type. |

## Durable notebook changes

Updated: `conversation-identity-conventions.md`, `conversation-api-conventions.md`, `reassembly-conventions.md`, `qt-model-conventions.md`, `text-encoding-conventions.md`, `c-wire-layout-portability-conventions.md`, `parser-boundary-conventions.md`, `json-marshalling-conventions.md`, `memory-lifetime-conventions.md`, `build-conventions.md`, `profile-conventions.md`, `platform-gui-conventions.md`, `dfilter-type-inference-conventions.md`, `submission-conventions.md`.

The central high-confidence findings are QUIC-versus-UDP conversation identity (!9141), John Thacker's corrective post-merge conversation binding analysis (!9123), Guy Harris's endian-load portability guidance and UTF-8 optimization constraints (!9153), bounded Qt packet-column caches and sorting limits (!9143, later !9222), RTPS offset overflow diagnostics (!9124, !9134–!9135), JSON writer state on empty bytes (!9126, !9130), and packaging completeness for the SpeexDSP migration (!9125, !9151, !9156). Closed/open and superseded implementations were down-weighted; post-merge review and follow-up fixes took precedence over the historical merged flag.

No builds, runtime regression tests or fuzzing were performed in this notebook-only review. **Next candidate: !9111**, subject to the next run's complete review-membership check. Preserve the recurring automation; do not pause or disable it.
