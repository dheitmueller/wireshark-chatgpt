# Wireshark MR review ledger — !10312–!10300 plus !9999–!9963

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`
Notebook starting commit: `763dc4e20c53296be240fd21befd85d2862a84a1`
Review date: 2026-10-30
Exact count: **50** (**48 merged, 2 closed/unmerged**). Direction: descending MR number.

## Duplicate avoidance and selection

Inventoried all **346** entries in `reviewed-mrs-automation/`, examined the exact-membership and noncontiguous/gap tracking files, the aggregate `reviewed-mrs-automation.md`, and `reviewed-mrs.md`. The latest ordinary batch ledger ends at !10313; the previous early-numbered run ends at !10000. Other ordinary ledger filenames and their boundaries were inventoried without treating filenames alone as proof that their entire numeric ranges were covered. Special mixed/gap ledgers were inspected for individual MR mentions; their only references to this batch's candidate numbers are the *next-candidate* mentions of !10312 and !9999 in preceding ledger prose, not previous reviews. No batch candidate appears as reviewed in the aggregate tracking. The preserved !17571–!17620 ledger was independently parsed: **50 unique reviewed table rows, spanning the full 50-number interval**, and remains counted. No new high-numbered corpus entries exist above the prior frontier in the unchanged pinned corpus.

The fifty highest-numbered unreviewed corpus entries are thirteen numbers !10312 down to !10300 followed by thirty-seven !9999 down to !9963 (the intervening !10000–!10299 already have explicit historical per-run coverage). This is intentional noncontiguous selection, **not** an assumption that intermediate ranges are reviewed merely because a filename exists. The existing corpus contains all fifty JSON records; discussions and diffs were reviewed at the indicated depth.

## Exact reviewed MR set

!10312, !10311, !10310, !10309, !10308, !10307, !10306, !10305, !10304, !10303, !10302, !10301, !10300, !9999, !9998, !9997, !9996, !9995, !9994, !9993, !9992, !9991, !9990, !9989, !9988, !9987, !9986, !9985, !9984, !9983, !9982, !9981, !9980, !9979, !9978, !9977, !9976, !9975, !9974, !9973, !9972, !9971, !9970, !9969, !9968, !9967, !9966, !9965, !9964, !9963

## Per-MR findings

| MR | Outcome | Depth | Finding |
| --- | --- | --- | --- |
| !10312 | merged | Deep | DRDA SQLATTR uses the same SQLSTTGRP decoder as SQLSTT; registers one handler for both opcodes. |
| !10311 | merged | Scanned | Release backport: LLVM 15 Ubuntu packaging workaround uses DPkg --force-overwrite in CI. |
| !10310 | merged | Scanned | Companion release backport of the same temporary LLVM 15 packaging workaround. |
| !10309 | merged | Scanned | BFCP fixes text attribute payload lengths: length includes two type/length bytes, not padding. |
| !10308 | merged | Scanned | Master CI temporary workaround for conflicting Ubuntu LLVM 15 package ownership. |
| !10307 | merged | Scanned | CIP internal function receives static linkage rather than an exported symbol. |
| !10306 | merged | Scanned | BFCP stable backport of the two-byte attribute-header length accounting fix. |
| !10305 | merged | Deep | DRDA uses dissector_try_uint_new(..., FALSE, NULL) for nested dispatch, avoiding duplicate protocol-list entries. |
| !10304 | merged | Scanned | ISAKMP identity-type bit extraction uses big-endian for the two low-order bits; stable backport. |
| !10303 | merged | Scanned | Companion ISAKMP release backport of the same bit-order correction. |
| !10302 | merged | Scanned | Master ISAKMP correction: bit extraction must use wire-format bit ordering. |
| !10301 | merged | Deep | Guy Harris restructures NHRP type-specific parsing; diagnostics attach to the offending extension-offset item and nested in_error_pkt state is restored. |
| !10300 | merged | Scanned | I/O Graph documentation explains automatic updates/legend and uses sentence-case checkbox labels. |
| !9999 | merged | Discussion-focused | Guy Harris centralizes JSON dumper overflow/underflow and sticky error handling; corpus holds only short diff fragments, so use accepted description cautiously. |
| !9998 | merged | Scanned | Wi-SUN adds LBATS/JM IEs and updates NR handling. |
| !9997 | merged | Scanned | IEC104 groups BCR counters/flags under a common filterable field/subtree. |
| !9996 | merged | Deep | SOME/IP-TP reassembly identity incorporates service, method, client, session, type, version, addresses and ports; persistent keys deep-copy address buffers. |
| !9995 | merged | Discussion-focused | Linux 6.2 netlink identifiers updated; Alexis questioned a numeric gap and Jaap pointed to authoritative kernel numbering. |
| !9994 | merged | Deep | John Thacker centralizes dumpcap process/pipe startup while retaining OS-specific handle cleanup and explicit ownership/error contracts. |
| !9993 | merged | Scanned | O-RAN FH C/U section-extension usability and text/presentation adjustments. |
| !9992 | merged | Scanned | 802.11 adds a generated flags string field for custom-column filtering and display. |
| !9991 | merged | Deep | SOME/IP UDP heuristic returns whether udp_dissect_pdus actually claimed the packet, rather than always TRUE; backport guidance was discussed. |
| !9990 | merged | Scanned | Qt QByteArray-to-C-string conversion uses constData() for newer GLib compiler compatibility. |
| !9989 | merged | Scanned | dumpcap manpage removes unsupported predef: capture-filter-name claim. |
| !9988 | merged | Deep | Guy Harris allows nameless base64 values and objects inside JSON arrays in sharkd. |
| !9987 | merged | Scanned | O-RAN ext12 settings feed ext11 beamforming bundle allocation. |
| !9986 | merged | Scanned | Grammar correction to documentation (possessive its). |
| !9985 | merged | Scanned | Qt System Default copy-from-profile QAction supplies required profile_filename property. |
| !9984 | merged | Scanned | ASTERIX spec converter validates revised structure before using it. |
| !9983 | merged | Deep | TECMP uses BASE_UNIT_STRING for integer temperatures; for historical FT_DOUBLE behavior, John Thacker recommends explicit formatted-value rendering. |
| !9982 | merged | Scanned | USBLL only calls packet_ends_transfer when an active transfer key exists; avoids status-only STALL assertion. |
| !9981 | merged | Scanned | 802.11 WPA vendor IE stores AKM_KEY for decryption; release backport. |
| !9980 | merged | Scanned | Companion 802.11 WPA vendor IE AKM_KEY release backport. |
| !9979 | merged | Discussion-focused | Wi-SUN EAPOL KDE support supplies a sample pcapng and discusses a CI regression. |
| !9978 | merged | Scanned | Master WPA tag-221 AKM_KEY persistence for later decryption. |
| !9977 | closed/unmerged | Discussion-focused | Closed new IPv6 APN6 stable-branch feature backport; Jaap Keuter cited release policy against new features in stable branches. |
| !9976 | closed/unmerged | Scanned | Draft APN6 MR closed immediately; snapshot has an unrelated 561-file/1179-commit diff due to branch-base mismatch, not reliable implementation precedent. |
| !9975 | merged | Scanned | UDS decoder expands subfunction and diagnostic-response parsing, terminology and error guards. |
| !9974 | merged | Scanned | O-RAN ext13 settings inform ext11 beamforming bundle allocation; bounded local bundle storage. |
| !9973 | merged | Scanned | Resources remove developer-machine absolute export paths from SVG metadata. |
| !9972 | merged | Scanned | Routine generated registry/manufacturer updates; no new durable convention. |
| !9971 | merged | Scanned | Stable release automatic registry, NEWS, and help-text refresh. |
| !9970 | merged | Scanned | Another stable release automatic registry/help refresh. |
| !9969 | merged | Scanned | Manpages describe environment-governed temp directories on Windows and Unix instead of obsolete fixed Windows paths. |
| !9968 | merged | Deep | Guy Harris pairs sharkd JSON object begin/end helpers and gives arrays/object members correct key semantics. |
| !9967 | merged | Scanned | Qt packet-list show() precedes setVisible() to avoid pending resize/column effects. |
| !9966 | merged | Scanned | sshdump builds default remote capture filter from actual runtime SSH port, not stale config-dialog defaults. |
| !9965 | merged | Scanned | sharkd deprecation warning uses actual warning text rather than nonexistent success-path error; backport. |
| !9964 | merged | Scanned | Second stable backport of sharkd filter-deprecation warning correction. |
| !9963 | merged | Scanned | Guy Harris master fix avoids dereferencing nonexistent dfilter error on successful compile with deprecated tokens. |

## Durable guidance promoted

- **!9996 (merged; Dr. Lars Völker review):** a segmented transport's fragment key must represent the complete logical transaction *and* transport endpoint identity. Use deep-copied address storage for persistent keys and shallow data only for transient lookups. Recorded in `reassembly-instance-identity-conventions.md`.
- **!9991 (merged):** a UDP heuristic must return the helper's actual accept/decline result, not unconditional success. Recorded in `heuristic-dissector-conventions.md`.
- **!10301 (merged, Guy Harris):** attach an expert diagnostic to the actual invalid field and save/restore contextual packet flags around nested dissection. Recorded in `protocol-input-diagnostics-conventions.md`.
- **!9999, !9968 and !9988 (merged, Guy Harris):** JSON APIs must distinguish array elements from named object members, pair container transitions, and centralize depth/error-state validation. Recorded in `json-marshalling-conventions.md`. The corpus provides only a short captured `!9999` diff, so that part is supported chiefly by the accepted MR description, not a claim of full source-diff inspection.
- **!9983 (merged, John Thacker discussion):** field-registration unit modifiers and explicit-value construction were not historically interchangeable for FT_DOUBLE; check the API/branch implementation before relying on implicit unit display. Recorded in `field-display-policy-conventions.md`.
- **!9994 (merged, John Thacker):** consolidate shared child-process launch semantics but preserve per-platform pipe/handle and error-output contracts. Recorded in `capture-diagnostic-conventions.md`.

**Evidence weighting:** merged mainline work is the primary implementation precedent. Closed !9977 is useful as explicit Jaap Keuter release-policy feedback, but its proposed feature backport is not treated as accepted. Closed draft !9976's mismatched 561-file/1179-commit snapshot is not an implementation exemplar. Stable-branch duplicates corroborate but do not multiply independent evidence. Automated merge/CI retry chatter is not treated as human design review.

## Next run

Next expected frontier: **!9962**, subject to re-enumerating corpus and exact tracking for newly scraped backfills. Corpus is not exhausted. Keep the recurring automation enabled and unpaused.
