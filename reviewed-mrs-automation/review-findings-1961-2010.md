# Review findings — !1961–!2010

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This file records the per-MR outcome and review depth used for durable convention extraction. Merged work is stronger evidence than closed/superseded work; direct maintainer guidance is weighted accordingly.

| MR | Outcome | Depth | Review result |
|---|---|---|---|
| !2010 | merged | Discussion-focused | Guy Harris release-3.4 backport of improved capture-error secondary diagnostics; corroborates the master-side diagnostic series. |
| !2009 | merged | Deep | Guy Harris improves dumpcap diagnostics so the primary error is preserved while secondary text explains likely component ownership/remediation. |
| !2008 | merged | Deep | Guy Harris includes the human-facing interface name in capture errors; diagnostics should identify the concrete object that failed. |
| !2007 | merged | Scanned | Removes an unused MPTCP field registration; straightforward cleanup. |
| !2006 | merged | Discussion-focused | Peter Wu asked F5 statistics documentation to derive semantics from implementation comments and explain TMM rather than leave a superficial description. |
| !2005 | merged | Deep | Adds a capacity guard before storing TCP SACK ranges in fixed arrays, fixing an out-of-bounds write. |
| !2004 | merged | Scanned | Adds 802.11 RM capability bits and narrows the reserved mask accordingly. |
| !2003 | merged | Scanned | Moves RTP sample-byte sizing to the shared RTP media header; ordinary refactor. |
| !2002 | merged | Scanned | Initializes a length variable to preserve compatibility with older GCC diagnostics/analysis. |
| !2001 | merged | Scanned | Narrows internal dissector functions/variables to `static`; reduces unnecessary external linkage. |
| !2000 | merged | Deep | WSP statistics tables are created/populated only once; subsequent initialization resets existing tables through the registered reset callback. |
| !1999 | merged | Deep | MTP3 applies the same create-once/reset-on-reinit stat-tap lifecycle. |
| !1998 | merged | Scanned | Adds 6 GHz HE Operation decoding; protocol-specific feature. |
| !1997 | merged | Deep / high-authority | Guy Harris distinguishes a genuinely removed device from a likely Npcap fault and avoids automatically blaming Npcap when the user actually removed the adapter. |
| !1996 | merged | Deep / high-authority | Guy Harris stops matching a localized Windows error sentence and instead recognizes stable framing plus native code 1617; the intervening text may be in any language. |
| !1995 | merged | Deep / high-authority | Guy Harris introduces primary + secondary capture-error messaging that preserves the runtime error and adds actionable ownership/remediation context. |
| !1994 | merged | Scanned | Displays SMC reserved bytes explicitly; protocol presentation improvement. |
| !1993 | merged | Scanned | Adds ONC-RPC Programs documentation. |
| !1992 | merged | Scanned | WSP indentation/style cleanup only. |
| !1991 | merged | Deep | H.225 statistics create/populate fixed table rows only once and reset existing state on reinitialization. |
| !1990 | merged | Deep | GSM MAP statistics adopt the same create-once/reset lifecycle. |
| !1989 | merged | Deep | ANSI MAP statistics adopt the same create-once/reset lifecycle. |
| !1988 | merged | Deep | CAMEL statistics adopt the same create-once/reset lifecycle. |
| !1987 | merged | Deep | SIP request/response stat tables reset existing state while creating/populating fixed rows only when absent. |
| !1986 | merged | Deep | RPC program statistics adopt the same create-once/reset lifecycle. |
| !1985 | merged | Deep | ANSI-A DTAP statistics adopt the same create-once/reset lifecycle. |
| !1984 | merged | Deep | DHCP statistics adopt the same create-once/reset lifecycle. |
| !1983 | merged | Deep | ANSI-A BSMAP statistics adopt the same create-once/reset lifecycle. |
| !1982 | merged | Discussion-focused | Peter Wu and Jaap Keuter pushed UDP Multicast Streams docs toward accurate scope/use cases and away from implying all video traffic is multicast. |
| !1981 | merged | Discussion-focused | Peter Wu recommended linking identical IPv6-statistics behavior to the IPv4 section instead of duplicating documentation. |
| !1980 | merged | Discussion-focused | Peter Wu requested concrete breakdown semantics, examples, use cases, and links to related statistics views. |
| !1979 | merged | Scanned | Converts a netlink nanosecond timestamp explicitly to `nstime_t` before adding the time field. |
| !1978 | merged | Deep | Anders Broman caught missing declarations and unused parameters; the accepted sharkd change includes the owning header unconditionally and marks intentionally unused parameters explicitly. |
| !1977 | closed | Discussion-focused / superseded | Earlier sharkd submission was replaced by merged !1978; implementation evidence is down-weighted. |
| !1976 | merged | Discussion-focused | Peter Wu corrected Flow Graph terminology and interaction semantics and asked documentation to describe actual packet-list/navigation behavior. |
| !1975 | merged | Deep | Alexis La Goutte required an unrelated MsQuic change to move to a separate MR and improved QUIC field naming/source references; accepted MR stays focused on version negotiation. |
| !1974 | merged | Deep | NAS decoded user data is placed consistently at the top tree for LTE and 5GS; Pascal Quantin explicitly asked for LTE/NR consistency. |
| !1973 | merged | Deep | Pascal Quantin required a preference, off by default, for a long-standing LTE-RRC tree-layout change; Anders discussed the competing usability case. Accepted design preserves the old default while making the alternative available. |
| !1972 | merged | Scanned | Automatic release-3.2 data/translation update. |
| !1971 | merged | Scanned | Automatic release-3.4 data/translation update. |
| !1970 | merged | Scanned | Automatic master data/translation update. |
| !1969 | merged | Deep / high-authority | Guy Harris consolidates btsnoop output around one writer that branches on encapsulation semantics, fixes H1/H4 header/data handling, and validates via byte-identical editcap round trips. |
| !1968 | closed | Scanned | Unmerged attempt to add a Wireshark `--compress-type` option; no substantive maintainer guidance, so no durable implementation weight. |
| !1967 | merged | Deep / high-authority | Guy Harris introduces generated built-in Wiretap module registration: discover module-owned `register_*` entry points from the actual source list, generate a callback table, and invoke it during `wtap_init()`. |
| !1966 | merged | Deep / high-authority | Guy Harris completes pcapng FCS-length IDB-option serialization by accounting for its size and writing its uint8 value. |
| !1965 | merged | Deep / high-authority | Guy Harris removes `HAVE_PLUGINS` guards around pcapng handler infrastructure because those tables will also serve built-in handlers; feature-disabled builds must retain shared mechanisms needed by built-ins. |
| !1964 | merged | Scanned | Homebrew setup package-list maintenance; no new cross-cutting rule. |
| !1963 | closed | Discussion-focused / superseded | Proposed no-plugins build fix was closed after Guy Harris pointed to merged !1965; use !1965 as the authoritative implementation. |
| !1962 | merged | Discussion-focused | Clang Analyzer dead-store cleanup; Anders Broman asked that the commit subject identify the affected component/file, reinforcing component-prefixed submission hygiene. |
| !1961 | merged | Scanned | Aruba display-string spacing cleanup only. |

## Strongest durable conclusions

- The Guy Harris dumpcap series !1995–!2010 is strong evidence that diagnostics should preserve the real failure, identify the affected interface/backend, and use stable machine-readable/native structure rather than localized error text when classifying platform errors.
- Guy-authored !1967 is the architectural foundation for later runtime Wiretap registration: module-local registration entry points are discovered/generated from the actual module source set, while common initialization invokes the generated registry.
- Guy-authored !1969 demonstrates that one capture format writer can support multiple encapsulations through one semantic writer path keyed by the writer encapsulation, and that format conversion fixes deserve round-trip/byte-identity validation.
- Guy-authored !1965 shows that a compile-time “plugins disabled” switch must not remove registration/dispatch machinery that built-in modules also use.
- The merged !1983–!2000 stat-tap series repeatedly separates immutable table schema/rows from mutable statistics state: create/populate once, reset on reinit.
- !1973 is strong maintainer-reviewed default-behavior evidence: when a long-standing tree-layout choice has legitimate competing workflows, expose the new presentation as a preference and preserve backward-compatible default behavior unless there is consensus to change it.
- !1975 reinforces MR scope discipline: an unrelated cleanup/change should move to its own MR rather than ride along with a protocol feature.
