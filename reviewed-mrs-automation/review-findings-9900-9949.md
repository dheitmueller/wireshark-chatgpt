# Review findings for !9900-!9949

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

The batch contains 48 merged MRs and two closed/unmerged MRs (!9930 and !9907). Merged changes were weighted more heavily than abandoned or superseded proposals. Direct maintainer guidance in the closed MRs was retained where it describes durable project policy.

## Strongest durable findings

- **!9925 — validate signed lengths before normalization.** John Thacker fixed the core bit-item APIs so a negative bit count is rejected before `(no_of_bits + 7) >> 3` can turn -1 through -7 into zero octets. Negative input is a bounds failure; zero width remains a distinct dissector-programming error.
- **!9921 — shallow for transient lookup, deep at persistence.** The RTP tap now borrows packet addresses for the temporary stream key and deep-copies them only when creating a persistent stream entry. The shallow-copy helper explicitly must not be passed to the owning free routine.
- **!9919 — machine identifiers must round-trip.** Sharkd emitted RTP SSRCs as decimal even though follow-up analyse/download tokens parse them as hexadecimal. The accepted fix standardizes the externally emitted representation and adds end-to-end list/analyse/download tests.
- **!9918 — RPC failures must still complete the request.** A nonzero Wiretap error during Sharkd load previously printed a useful console message but sent no JSON-RPC response, leaving clients hanging. The fix returns both human-readable status and the numeric error and adds a truncated-capture regression.
- **!9914 — reset state at the semantic boundary, not every call return.** John Thacker reverted per-dissector restoration of conversation elements because GUI/post-dissection users query the packet's final conversation/address state. The desired reset point is between peer PDUs, not indiscriminately after each nested dissector.
- **!9904 — nested reassembly dependencies are transitive frame state.** The accepted stable change moves dependency tracking from ephemeral `packet_info` state onto `frame_data`, allowing filtered save/export to retain indirect prerequisites across multiple reassembly layers.
- **!9901 and !9912 — keep mirrored frontends aligned.** Gerald Combs, John Thacker, and Gilbert Ramirez independently required corresponding Wireshark/Logray changes where the code was copied. !9901 also shows why a safe Qt workaround can remain necessary after an upstream fix exists: supported Linux distributions may still ship affected Qt versions.
- **!9907 — branch-aware automation must obey release policy.** Jaap Keuter rejected an automatic release-4.0 update because it also extended the ASTERIX dissector. Gerald Combs changed the update tooling so that functional ASTERIX updates are master-only and replaced the MR with merged !9915.
- **!9930 — generated dissectors must be fixed through authoritative inputs.** Alexis La Goutte rejected direct SRVSVC generated-code edits and directed the contributor to update the IDL/CNF and regenerate. The contributor moved that work to !10009. Because !9930 itself was closed, this is used as corroborating maintainer guidance rather than accepted implementation precedent.
- **!9949 — use scoped allocation for packet-lifetime strings.** John Thacker replaced `g_strdup_printf()` with `wmem_strdup_printf(wmem_packet_scope(), ...)`, eliminating a manual-lifetime leak for a temporary UDS display string and corroborating the notebook's existing allocator-scope rule.

## Per-MR inventory

| MR | Outcome | Review | Durable note |
|---|---|---|---|
| !9949 | merged | Deep | Packet-lifetime formatted string moved to packet-scope wmem; allocator-scope corroboration. |
| !9948 | merged | Scanned | `make-version.py` follows relocated WSDG/WSUG docinfo paths; maintenance only. |
| !9947 | merged | Scanned | Release-3.6 Perl version tool path fix; backport of doc layout maintenance. |
| !9946 | merged | Scanned | Release-4.0 Python version tool path fix; backport of doc layout maintenance. |
| !9945 | merged | Scanned | 3.6.13 version/release metadata bump; no new convention. |
| !9944 | merged | Scanned | 4.0.5 version/release metadata bump; no new convention. |
| !9943 | merged | Scanned | Stable Qt sequence-diagram comment axis forced to non-scientific numeric formatting. |
| !9942 | merged | Scanned | Release-4.0 backport of sequence-diagram comment-format fix. |
| !9941 | merged | Scanned | Build/release notes for 3.6.12; release preparation only. |
| !9940 | merged | Scanned | Build/release notes for 4.0.4; release preparation only. |
| !9939 | merged | Scanned | Prep for 3.6.12; release-note maintenance only. |
| !9938 | merged | Scanned | Stable Qt fix avoids double HTML escaping where the receiving widget already escapes. |
| !9937 | merged | Scanned | Release-4.0 backport of double-escape fix. |
| !9936 | merged | Scanned | Stable help URL follows moved WLAN Traffic documentation anchor. |
| !9935 | merged | Scanned | Release-4.0 backport of help-anchor fix. |
| !9934 | merged | Deep | RTP stream-ID cleanup must free both owned members and the heap-allocated containing struct. |
| !9933 | merged | Scanned | Master documentation/help anchor synchronization. |
| !9932 | merged | Deep | Escape at one ownership/presentation boundary; avoid encoding the same UI text twice. |
| !9931 | merged | Scanned | Treat sequence-diagram comments as textual labels, not numeric values. |
| !9930 | closed | Discussion-focused | Alexis La Goutte: fix generated SRVSVC through IDL/CNF and regeneration; successor !10009. |
| !9929 | merged | Scanned | WSUG anchor naming synchronized with help lookup. |
| !9928 | merged | Deep | Avoid heap allocation when a Qt container copies the value object; ownership follows actual copy semantics. |
| !9927 | merged | Deep | RTP Analysis lifetime fixes free tab metadata and calculation data at their owning boundaries. |
| !9926 | merged | Discussion-focused | Release-note prep; John Thacker supplied a missing fixed issue, Martin Mathieson caught a presentation typo. |
| !9925 | merged | Deep | Reject negative bit widths before alignment arithmetic can normalize them to zero. |
| !9924 | merged | Discussion-focused | John Thacker caught encoding-dependent DRDA length semantics; EBCDIC path was corrected before merge. |
| !9923 | merged | Deep | Parent RTP graph QObject to the QCustomPlot owner so Qt lifetime cleanup is automatic. |
| !9922 | merged | Scanned | Clang Analyzer dead-store cleanup; no broader convention beyond static-analysis hygiene. |
| !9921 | merged | Deep | Borrow packet addresses for transient lookup; deep-copy only for stored RTP stream state. |
| !9920 | merged | Scanned | Documents GLib/Clang TSAN false-positive rationale for mutex rather than atomics. |
| !9919 | merged | Deep | Canonicalize Sharkd RTP SSRC representation and test producer-to-consumer round trips. |
| !9918 | merged | Deep | Every Sharkd load failure returns a JSON response; stderr diagnostics alone are insufficient. |
| !9917 | merged | Scanned | ICMPv6 lifetime keeps raw field while appending a human-readable time rendering. |
| !9916 | merged | Scanned | Internal RTPS helper made static; symbol-scope cleanup only. |
| !9915 | merged | Deep | Correct stable automatic-data refresh after !9907; functional ASTERIX extension omitted. |
| !9914 | merged | Deep | Revert over-eager conversation-state restoration because final packet state has post-dissect consumers. |
| !9913 | merged | Scanned | Use `ws_debug()` for debug-only display-filter logging so release builds optimize it away. |
| !9912 | merged | Discussion-focused | Gilbert Ramirez required the equivalent Logray change; accepted diff keeps copied frontends in sync. |
| !9911 | merged | Scanned | UDS tree cleanup removes structure not justified by the standard. |
| !9910 | merged | Deep | Stable UDS path avoids constructing/dissecting a zero-length payload subset. |
| !9909 | merged | Scanned | Master automatic registry/translation update; generated-data maintenance. |
| !9908 | merged | Scanned | Release-3.6 automatic PCI/manuf refresh; data maintenance. |
| !9907 | closed | Discussion-focused | Jaap Keuter rejected master-only ASTERIX functionality in a stable automated update; tooling was fixed. |
| !9906 | merged | Discussion-focused | Lars Völker split a typo fix into its own MR so it could be cherry-picked independently; corroborates backport-scope rule. |
| !9905 | merged | Deep | UDS WDBI zero-length bugfix on master, explicitly intended for stable backport. |
| !9904 | merged | Deep | Frame dependencies moved to frame-owned state so nested reassembly preserves transitive prerequisites. |
| !9903 | merged | Scanned | Removes long-unused Qt model member; cleanup only. |
| !9902 | merged | Deep | Parent newly created QActions to their tool buttons so QObject ownership performs cleanup. |
| !9901 | merged | Deep | Qt queued-connection workaround applied across mirrored Wireshark/Logray paths; supported-version reality matters. |
| !9900 | merged | Deep | Stable UDS fix checks zero length before byte-string conversion; malformed/empty payload resilience. |

## VANC tracking

No SMPTE ST 291/VANC packet type was encountered in this batch.
