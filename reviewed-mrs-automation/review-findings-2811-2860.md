# Wireshark MR review findings — !2811–!2860

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged MRs are the strongest implementation evidence. Closed/superseded work is retained only where its discussion explains a rejected design or submission failure and is explicitly down-weighted.

| MR | Outcome | Review result |
|---|---|---|
| !2860 | merged | BGP MP_REACH/MP_UNREACH cleanup by John Thacker. Adds structured next-hop/NLRI fields and three sample captures. Uli Heilmeier caught switch-style consistency and a display-filter abbreviation typo; both were corrected. |
| !2859 | merged | Packet-option migration. Direct Guy Harris guidance: once packet options are carried by a `wtap_block`, all packet options should converge on that option container rather than retaining duplicate dedicated header fields. Merged successor to !2823. |
| !2858 | merged | ARM64 GCC 9.3 false-positive workaround. Pascal Quantin verified the compiler-specific nature and requested a commit subject that identifies the dissector, compiler/platform, and that this is a workaround rather than a semantic fix. |
| !2857 | merged | RTPS initializes `guid.fields_present`; straightforward correctness fix. |
| !2856 | merged | RTPS-PROC Coverity NULL-dereference fix; no broader new rule beyond existing defensive-state checks. |
| !2855 | merged | NVMe replaces `val_to_str()` with `val_to_str_const()` for literal fallbacks, avoiding packet-scope allocation where no formatting is needed. |
| !2854 | merged | Guy Harris removes 32-bit narrowing when assigning to `nstime_t.secs`. The member is `time_t`, which is not necessarily 32-bit; a `guint32` cast creates a Y2038-style truncation bug. |
| !2853 | merged | Guy Harris immediately fixes basename extraction introduced in !2844: use the shell variable (`basename $FILE`), not the literal word `file`. Tooling fixes need execution coverage too. |
| !2852 | merged | Gerald Combs temporarily disables failing Fedora RPM tests due distro minizip packaging changes. CI-infrastructure-specific. |
| !2851 | merged | 802.11 Partial TSF presentation. Pascal Quantin caught a flag/value misclassification and pointed to existing `UTF8_MICRO_SIGN`/unit-string infrastructure instead of local presentation text. |
| !2850 | merged | WSLua GCC 11 misleading-indentation build fix. |
| !2849 | closed/unmerged | QtMultimedia/RTP Player work. Roland Knall argued dialogs should remain as standalone as practical and not be tightly coupled to MainWindow/singleton state. The MR conflicted and was later superseded; retain only as lower-weight architecture discussion. |
| !2848 | merged | USBLL transaction-to-transfer reassembly. Generic reassembly accepted pragmatically; endpoint/configuration state should be maintained by the USB dissector rather than duplicated in USBLL. Mostly author discussion. |
| !2847 | merged | M3AP template header version update; follow-up requested during !2846. |
| !2846 | merged | M3AP ASN.1 release-version update. Pascal Quantin requested updating the corresponding template/reference metadata, completed in !2847. Corroborates source/template/output synchronization. |
| !2845 | merged | NVMe value-string conflict found by pipeline. Martin Mathieson asked the protocol maintainer to verify the guessed correction; Constantine Gavrilov checked the NVMe-oF specification and confirmed value 5. |
| !2844 | merged | Guy Harris makes `validate-clang-check.sh` portable to shells without `[[ ... ]]` and skips a Windows-only source file when running clang-check on Unix. Host analysis tools must respect each translation unit's build domain. |
| !2843 | merged | NAS 5GS adds TCP Decode As support; targeted registration change. |
| !2842 | merged | DCT2000 protocol lookup update; localized integration change. |
| !2841 | merged | Guy Harris replaces numeric direction constants with named definitions and masks `PACK_FLAGS_DIRECTION_MASK` before interpreting direction. Unrelated flag bits must not change enum interpretation. |
| !2840 | merged | Guy Harris fixes tfshark compilation but notes that successful compilation did not imply operation—the program still crashed on JPEG input. Compile-only validation is not runtime validation. |
| !2839 | merged | Gerald Combs adds JSON-defined external TShark tests with reusable count/grep/in assertions and case-relative paths, intended to let happy-shark host dissector tests without hard-coding them in Wireshark's Python suite. |
| !2838 | merged | RTP Player export-from-cursor option; UI feature with no substantive review. |
| !2837 | merged | 802.11 availability bitmap fix (missing multiply by 8). |
| !2836 | merged | 802.11 duration presentation. Pascal Quantin reduced decimal precision to match the protocol's actual 100 µs resolution. |
| !2835 | merged | Guy Harris centralizes Wiretap interface-filter cleanup through `if_filter_free()` instead of duplicating teardown. |
| !2834 | merged | Keysight/Ixia NetFlow fields. Martin Mathieson requested ordinary `value_string` + `VALS()` tables for fixed mappings instead of `CF_FUNC`; the final code follows that declarative model. |
| !2833 | merged | RTP Player keyboard-focus fix. |
| !2832 | merged | Diameter Huawei XML AVP additions; data-table update. |
| !2831 | merged | RTPS large Type Object crash fix. Anders Broman explicitly requests `proto_tree_add_subtree()` / `proto_tree_add_subtree_format()` instead of the old/general tree method; accepted code follows that API and caps displayed elements. |
| !2830 | merged | Guy Harris fixes 64-bit formatting: `%l[doux]` is not guaranteed to match a 64-bit integer; use `G_GUINT64_FORMAT` for `guint64`. |
| !2829 | merged | PTP spelling cleanup. |
| !2828 | merged | Moves Windows VLD option to CMakeOptions.txt. |
| !2827 | merged | Stable-branch PTP bounds fix; validates enough bytes before reading length/type. |
| !2826 | merged | Removes obsolete optional c-ares checks after c-ares became mandatory and renames third-party DLL/PDB lists accordingly. |
| !2825 | merged | 802.11 repetition-count formatter. Pascal Quantin requested showing transformed semantic count plus raw value, and corrected `%d` to `%u` for `guint32`. |
| !2824 | merged | Correct HTTP/2 GOAWAY field naming to Last-Stream-ID; merged successor to !2820. |
| !2823 | closed/unmerged | First multiple-packet-comment implementation introduced a parallel generic TLV mechanism. Guy Harris pointed to existing `wiretap/wtap_opttypes.{c,h}`; contributor closed it in favor of merged !2859. Strong negative-plus-successor evidence. |
| !2822 | merged | PFCP pool identity. Anders Broman requested preserving the on-wire OCTET STRING as `FT_BYTES` and using `BASE_SHOW_ASCII_PRINTABLE` for presentation rather than changing semantic type to string. |
| !2821 | merged | Release-3.4 PTP bounds backport; same lesson as !2827. |
| !2820 | closed/unmerged | Earlier HTTP/2 naming submission, superseded by merged !2824. |
| !2819 | merged | RTP Player disk-backed temporary storage preferences. |
| !2818 | merged | BCG729 URL update. |
| !2817 | merged | Focused one-line RTP Player clang compilation fix; clean successor after accidental oversized !2816. |
| !2816 | closed/unmerged | Contributor accidentally resubmitted an entire large feature when intending a one-line compilation fix. Guy Harris noted title/change mismatch; contributor closed it and submitted focused !2817. |
| !2815 | merged | RTP Player memory-consumption improvements; substantial UI/media implementation without substantive review discussion. |
| !2814 | merged | Early no-libpcap RTP Player compilation fix. Later reviewed !2894/!2905 is stronger/current authority and shows this dependency boundary needed correction; treat !2814 only as historical context. |
| !2813 | merged | Windows Npcap 1.31 package update. |
| !2812 | merged | Automatic release-3.2 data update. |
| !2811 | merged | Automatic release-3.4 data update. |

## Strongest durable evidence

- **Packet-option architecture (!2823 → !2859):** Guy Harris rejected a parallel generic TLV mechanism in favor of Wiretap's existing block-option abstraction; the merged successor carries packet option state through a `wtap_block`.
- **Time width (!2854):** `nstime_t.secs` follows the platform's `time_t` width. Do not narrow it through a 32-bit intermediary.
- **Packed flags (!2841):** mask a bitfield to the defined semantic subfield before comparing it with enum constants.
- **Tree API (!2831):** use supported structured subtree APIs in dissectors rather than obsolete/general text-tree construction.
- **Wire type versus presentation (!2822):** preserve the field's real on-wire type and add human-readable presentation via field metadata.
- **Tooling portability (!2844 + !2853):** portable shell syntax and platform-aware source selection are part of static-analysis correctness.
