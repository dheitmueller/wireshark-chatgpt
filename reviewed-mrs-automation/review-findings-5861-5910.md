# Review findings: !5861–!5910

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

All 50 MRs in this batch were merged. Maintainer-authored changes and specific accepted review feedback are weighted most heavily; routine backports, automatic updates, documentation-only changes, and protocol-local fixes are retained as reviewed but not promoted unless they corroborate a broader convention.

| MR | Depth | Review result |
|---|---|---|
| !5910 | Deep | Gerald Combs replaces partial member initialization of a zlib `z_stream` with whole-object zero initialization after Coverity found an untouched member. Strong corroboration that third-party state structs should be initialized as complete objects when later library/error paths may inspect fields not explicitly assigned by the caller. |
| !5909 | Scanned | Developer Guide Asciidoctor anchor modernization; documentation-only. |
| !5908 | Scanned | Automatic data/translation update on master; no durable engineering lesson. |
| !5907 | Scanned | Automatic data/translation update on release-3.6; no additional lesson. |
| !5906 | Scanned | Automatic data/translation update on release-3.4; no additional lesson. |
| !5905 | Scanned | Narrow maintenance/build update; no new cross-cutting convention. |
| !5904 | Scanned | Narrow maintenance change; no new cross-cutting convention. |
| !5903 | Scanned | Automatic release-3.4 data/translation update; no durable lesson. |
| !5902 | Deep | EAP-IKEv2 support reuses the existing ISAKMP dissector via `call_dissector()`, supplies a representative pcap after Alexis La Goutte requests one, and splits an incidental ISAKMP tree-length correction into separate !5917 so it can be reviewed/backported independently. Strong reuse/scope/testing evidence. |
| !5901 | Backport | release-3.6 backport bundle of the BLF zlib/error and hang fixes represented by !5884/!5885. |
| !5900 | Deep | Jaap Keuter replaces `DISSECTOR_ASSERT()` checks on packet-controlled IPDC lengths with ordinary validation. Malformed wire input is not a programmer invariant and must not abort through assertion machinery. |
| !5899 | API-hardening | Jaap Keuter makes TVBuff copy helpers tolerate no-op/empty cases safely: zero-length `tvb_memdup()` returns NULL and `tvb_memcpy()` avoids dereferencing a NULL target. Useful API-specific evidence, but not generalized beyond the helper's accepted contract. |
| !5898 | Deep / high-authority | Guy Harris restructures libpcap open-time format detection into staged variant identification. Seek-based multi-record heuristics are used only where possible; pipes avoid heuristics that would block waiting for future records, and exact subtype/timestamp semantics are finalized only after the variant is known. Promoted to Wiretap format-detection notes. |
| !5897 | Backport | release-3.6 HTTP/3 QPACK filter fix; no new lesson. |
| !5896 | Scanned | RPM guide-build dependency correction; packaging-specific. |
| !5895 | Scanned | HTTP extended-CONNECT settings support and a related HTTP/3 setting fix; protocol-local. |
| !5894 | Discussion-focused | HTTP/2+HTTP/3 PRIORITY_UPDATE support. CI caught a Windows narrowing warning and required an explicit type/cast correction before merge; corroborates cross-platform compiler coverage. |
| !5893 | Deep | John Thacker adds Wiretap short-name encapsulation selection to text2pcap while retaining pcap numeric link-type compatibility and documenting that selecting an encapsulation does not transform packet bytes. Useful CLI/data-model separation. |
| !5892 | Scanned | HTTP/2 ORIGIN frame support; protocol-local. |
| !5891 | Scanned | Visual Studio 2022 packaging updates; maintenance. |
| !5890 | Scanned | GitLab CI migration toward Visual Studio 2022; build-infrastructure maintenance. |
| !5889 | Scanned | Spelling cleanup. |
| !5888 | Backport | release-3.6 Wiretap Custom Block description fix. |
| !5887 | Scanned | Import-from-hex now derives writable encapsulations from pcapng, the temporary format actually used. Good local correctness but no additional broad rule. |
| !5886 | Scanned | Radiotap bug fix; protocol-local. |
| !5885 | Deep | Fuzzing exposed BLF hangs when object/header lengths did not guarantee forward movement. The merged fix rejects undersized base headers and advances by at least the structural minimum / declared header length, preventing repeated reads at the same position. Promoted to parser-progress notes. |
| !5884 | Scanned | BLF begins checking zlib return codes instead of ignoring them. Useful robustness change, but later lifetime/cleanup work should remain authoritative for exact zlib cleanup behavior. |
| !5883 | Scanned | BLF debug-output improvement. |
| !5882 | Discussion-focused | New MSRCP dissector. Alexis requests squashing and cleanup of trailing whitespace; useful submission-hygiene corroboration, but no new architectural rule. |
| !5881 | Backport | release-3.4 radiotap typo fix. |
| !5880 | Backport | release-3.6 radiotap typo fix. |
| !5879 | Scanned | Master radiotap field-title typo fix. |
| !5878 | Discussion-focused | Build failure with an older libgcrypt configuration is fixed; Pascal Quantin requests a component-prefixed commit subject following Wireshark's submission guidance. Corroborates commit-subject conventions. |
| !5877 | Backport | release-3.6 registration fix for systemd Journal Export blocks. |
| !5876 | Deep | ETW feature review includes Gerald Combs requiring the Windows-only source to be excluded from a clang-analysis configuration that cannot compile it, and João Valverde preferring Wireshark's `ws_strdup_printf()` wrapper over needless replacement with GLib because the project wrapper uses native I/O, is more optimized, and performs stricter checking. Also reinforces updating docs/release notes with a new user-visible feature. |
| !5875 | Backport | release-3.6 removal of an unused libpcap private-structure type. |
| !5874 | Scanned / high-authority | Guy Harris removes an unused libpcap dumper-private structure definition rather than leaving a type implying ownership/state that does not exist. Small cleanup; no separate convention promoted. |
| !5873 | Backport | release-3.4 netlink identifier addition. |
| !5872 | Backport | release-3.6 netlink identifier addition. |
| !5871 | Discussion-focused | Radiotap NESS bit correction. Review challenges the apparent duplicate bit definitions against the specification before merge; protocol-specific but good example of reconciling bitfield semantics with the standard. |
| !5870 | Backport | release-3.6 OpenFlow/TDS robustness bundle represented by master fixes below. |
| !5869 | Backport | release-3.4 OpenFlow/TDS robustness bundle represented by master fixes below. |
| !5868 | Scanned | Qt Show Packet Bytes Rust-array output; UI feature-specific. |
| !5867 | Deep / high-authority review | John Thacker carefully distinguishes TShark's `-c` count of packets read from `-a packets:` count of packets written after display-filter/dependency handling. Accepted docs and implementation align the counters with the processing stage users actually asked to limit. Promoted to CLI counting semantics. |
| !5866 | Scanned | Wiretap Custom Block description fix. |
| !5865 | Scanned | PFCP UE IP Address correction; protocol-local. |
| !5864 | Scanned | TDS treats zero token size as invalid; malformed-length robustness. |
| !5863 | Deep | OpenFlow v6 validates property length before subtracting fixed header sizes; malformed entries emit expert info and terminate parsing of the containing structure rather than underflowing or looping. |
| !5862 | Deep | OpenFlow v5 applies the same minimum-length discipline across multiple match/action/property/instruction parsers after reviewer follow-up found additional sites. Strong malformed-input and progress evidence. |
| !5861 | Workflow-focused | Contributor moved work off personal `master` and enabled maintainer collaboration after the GitLab utility could not operate on the original setup. Reinforces topic-branch/collaboration guidance already in the notebook. |

## Strongest promoted conclusions

- Packet-controlled lengths are malformed-input conditions, not assertion-worthy programmer invariants (!5900).
- File/parsing loops need an explicit structural progress guarantee even when a corrupt length says to advance zero or too little (!5885, !5862, !5863).
- Wiretap format detection must respect streamability: heuristics requiring lookahead/rewind are appropriate for seekable files but can deadlock or delay pipes (!5898, Guy Harris).
- CLI counters should be defined by the exact processing stage they count; "packets read" and "packets written after display filtering" are not interchangeable (!5867, John Thacker).
- Reuse an existing dissector entry point instead of duplicating its wire parser, and split unrelated fixes into separate commits/MRs when they need independent review/backporting (!5902).
