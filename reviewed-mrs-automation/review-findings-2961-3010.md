# Wireshark MR review findings — !2961–!3010

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged MRs are weighted more heavily than closed or superseded work. Maintainer-authored and maintainer-reviewed guidance is weighted by authority and specificity; direct Guy Harris, Gerald Combs, John Thacker, Pascal Quantin, and other maintainer feedback is called out where it materially changes the lesson.

| MR | Outcome | Depth | Findings |
|---|---|---|---|
| !3010 | merged | Deep | John Thacker carries E212 semantic context from enclosing SBC-AP ASN.1 objects into PLMNidentity via packet-private state, consumes it once, and resets to E212_NONE. Template/conformance inputs and generated C stay synchronized. |
| !3009 | closed | Discussion / down-weighted | Graham Bloice proposed removing additional Qt autogen settings. Gerald Combs showed multi-config/MSBuild behavior still required target-level AUTOGEN properties and compared several CMake versions/platforms. Closed; useful build-history evidence, not implementation precedent. |
| !3008 | merged | Deep | Gerald Combs removes the temporary global CMAKE_AUTOMOC/AUTOUIC/AUTORCC workaround after the target-level fix, explicitly testing CMake 3.10.3, 3.20.1, and 3.20.2 on macOS. |
| !3007 | closed | Discussion / down-weighted | Debian artifact-ignore proposal closed after the contributor found the WSDG-supported dpkg-buildpackage workflow and João Valverde noted the custom target was not a sound packaging path. Prefer documented packaging workflows over hiding their byproducts. |
| !3006 | merged | Scanned | WoW authentication/two-factor/realm corrections with an attached capture. Protocol feature work; no distinct cross-cutting rule extracted. |
| !3005 | merged | Scanned | Debian/Ubuntu setup replaces removed qt5-default meta-package with concrete Qt development packages. Packaging maintenance only. |
| !3004 | merged | Deep / build architecture | Gerald Combs sets AUTOMOC/AUTOUIC/AUTORCC directly on qtui and wireshark so Qt autogen works with multi-configuration generators. Strong counterpart to !3008 and the rejected !3009 simplification. |
| !3003 | merged | Scanned | UDS UAT-based routine/data identifier name resolution. Useful implementation but no new project-wide convention beyond existing UAT/lifetime guidance. |
| !3002 | merged | Scanned | Pascal Quantin fixes a Coverity-detected WoW version comparison using patch_version rather than minor_version. Narrow correctness fix. |
| !3001 | merged | Corroboration | AUTOSAR NM widens UAT masks/data extraction to 64 bits and uses the matching GLib 64-bit formatting/UAT APIs. Reinforces width/format-domain consistency. |
| !3000 | merged | Scanned | SOME/IP-SD decomposes configuration strings into filterable elements and flags malformed encodings. Protocol-specific parser improvement. |
| !2999 | merged | Deep | John Thacker's HNBAP generated dissector carries a one-shot E212 context. The nested RAI→LAI case explicitly preserves the more-specific RAI context instead of letting the inner LAI overwrite it. |
| !2998 | merged | Corroboration | John Thacker applies the same one-shot semantic-context pattern to X2AP, updating conformance/template sources and generated C together. |
| !2997 | merged | Deep | RPCoRDMA reassembly completion now uses the segment type associated with the matching reassembly message ID instead of the arbitrary list element left in a loop variable. Completion metadata must be bound to the target reassembly identity. |
| !2996 | merged | Deep / Guy Harris review | Guy Harris requires the MR title and commit subject to describe the actual Geneve change, with the issue reference later; he also reminds the contributor to amend the Git commit and separately edit the GitLab MR, and explains that a stale pre-rebase fork pipeline failure is irrelevant. |
| !2995 | merged | Discussion-focused | Pascal Quantin asks that MPTCP flag bits use FT_BOOLEAN and questions value_string_ext for only seven values. Reinforces semantic field typing and avoiding needless extended tables. |
| !2994 | closed | Superseded | Tshark dfilter ownership/leak fix closed in favor of already-existing !2601. Down-weighted. |
| !2993 | merged | Deep | MaxMind response values retained by file/epan-scope hash maps are switched from g_memdup2 to wmem_memdup(wmem_epan_scope()). ASAN test output demonstrated the lifetime mismatch leak. |
| !2992 | merged | Scanned | GEONW fixes signed 15-bit speed interpretation and presentation. Narrow field-semantics correction. |
| !2991 | merged | Deep | John Thacker follows up RANAP E212 context handling so an inner LAI does not overwrite enclosing RAI semantics. Strong evidence that nested semantic context should be consumed/reset and more-specific outer context preserved. |
| !2990 | merged | Corroboration | John Thacker applies one-shot E212 context to NGAP, including the distinction between EPS TAI and 5GS TAI. Generated source inputs and output remain synchronized. |
| !2989 | merged | Backport | Release-3.2 backport of Gerald Combs's graceful fuzz/randpkt signal cleanup. Same rule as !2987. |
| !2988 | merged | Backport | Release-3.4 backport of Gerald Combs's graceful fuzz/randpkt signal cleanup. Same rule as !2987. |
| !2987 | merged | Deep | Gerald Combs makes fuzz-test/randpkt signal handlers remove temporary artifacts and exit successfully on ordinary termination signals. Harness interruption should clean up predictably instead of leaving partial state. |
| !2986 | merged | Scanned | Large WoW dissector expansion with capture; Alexis La Goutte requires Wireshark's fallthrough annotation form. Mostly protocol-specific. |
| !2985 | merged | Deep corroboration | Pascal Quantin disables the weak DRBD TCP heuristic by default because of false positives and adds a captured-length guard for recognition. John Thacker notes interaction with a TCP reassembly/API bug. Strong corroboration for default-disable of weak heuristics and bounds-safe recognition. |
| !2984 | closed | Down-weighted | Large IEC 60870-5-7/TLS proposal received substantial review, capture/key material, and CI cleanup, but remained unmerged. Style/duplication/debug-code feedback is historical only. |
| !2983 | merged | Deep, later corrected | COSE plus reusable CBOR decoder added with tests. Gerald Combs required fuzzing and challenged large array/map behavior; fuzzing found a truncated-input issue before merge. A later Guy Harris comment identifies an incompatible media_type data-context contract, fixed in later merged !10376; that later fix is the architectural precedent. |
| !2982 | merged | Scanned | 802.11 FTM terminology corrected to match the standard. No cross-cutting rule. |
| !2981 | closed | Superseded / down-weighted | Earlier IEC 60870-5-7 submission. Graham Bloice required rebase, removal of unrelated whitespace, and commit squashing; contributor moved to !2984. Not implementation precedent. |
| !2980 | merged | Discussion-focused | Erlang arbitrary-width integer encoding did not fit Wireshark's existing 64-bit varint API. Review explored reuse, but the accepted implementation kept protocol-specific handling rather than forcing a mismatched abstraction. |
| !2979 | merged | Scanned | TCP reassembly ignores spurious retransmissions whose payload was already accounted for. Reassembly/retransmission correctness; existing notebook guidance covers it. |
| !2978 | merged | Corroboration | John Thacker applies the one-shot E212 semantic-context pattern to S1AP and adds 5GS TAI field identity, updating generated sources and output together. |
| !2977 | merged | Scanned | Gerald Combs zero-initializes struct tm after Coverity reports tm_isdst could be read uninitialized by mktime. Straightforward static-analysis correction. |
| !2976 | closed | Superseded | DoIP diagnostic-address name resolution was reworked into an alternative UAT design and merged later as !3597; this global AddrResolve proposal is down-weighted. |
| !2975 | merged | Scanned | Qt VoIP dialog singleton/factory refactor. UI lifecycle work; no new convention extracted beyond existing Qt object-lifetime guidance. |
| !2974 | merged | Corroboration | John Thacker applies one-shot E212 context to F1AP NR-CGI and resets it after PLMNidentity. |
| !2973 | merged | Scanned | GTPv2 distinguishes UE Usage Type vs Remaining Running Service Gap Timer by IE instance. Protocol-specific semantic correction. |
| !2972 | merged | Maintenance | Automatic release-3.2 registry/data/translation update. No engineering convention. |
| !2971 | merged | Maintenance | SOME/IP formatting/help cleanup. No durable convention. |
| !2970 | merged | Maintenance | Automatic release-3.4 registry/data/translation update. No engineering convention. |
| !2969 | merged | Maintenance | Automatic master registry/data/translation update. No engineering convention. |
| !2968 | merged | Discussion-focused | GMTLS support receives Pascal Quantin cleanup and Alexis La Goutte requests a sample capture, which the contributor supplies. Reinforces representative-capture review for protocol additions. |
| !2967 | merged | Corroboration | John Thacker applies one-shot E212 context to E1AP NR-CGI with generated input/output parity. |
| !2966 | merged | Deep | John Thacker introduces the general RANAP pattern: enclosing ASN.1 objects set an E212 semantic type in private state; PLMNidentity consumes and immediately resets it. !2991 subsequently refines nested RAI→LAI inheritance. |
| !2965 | merged | Scanned | SABP can use E212_SAI directly because PLMN identity is always within a Service Area Identifier. Protocol-specific field identity. |
| !2964 | merged | Backport | Master-3.2 CI centralizes the Clang major version in one variable used by build/check jobs. Build-maintenance corroboration. |
| !2963 | merged | Backport | Release-3.4 counterpart of the Clang-version CI centralization. |
| !2962 | merged | Scanned | NFSv4 FS_CHARSET_CAP attribute support. Protocol feature only. |
| !2961 | merged | Scanned | Master-3.2 fuzz job gets a branch-specific resource_group so stable-branch fuzzing does not serialize on the master resource. CI scheduling maintenance. |

## Durable findings promoted from this run

- Enclosing generated-protocol structures can carry a transient semantic selector into a generic child through packet-private state, but that selector should be consumed once and reset. If a child is nested inside a more-specific enclosing context, it must not overwrite that outer semantic identity (!2966, !2991, !2998, !2999, !3010, plus !2974/!2967/!2978/!2990).
- Reassembly completion and classification must use metadata belonging to the matching reassembly instance, not the arbitrary object left after iterating a broader container (!2997).
- Qt/CMake autogen behavior is generator-sensitive. Prefer target-level AUTOGEN properties over global workaround variables, and retire workarounds only after testing both the affected tool versions and generator/platform combinations (!3004, !3008; closed !3009 supplies diagnostic history).
- Submission subjects should describe the actual change, with issue references in the body; amending the commit and editing the GitLab MR are separate operations. Keep development branches current enough that stale pre-rebase pipeline failures are not mistaken for current MR failures (!2996, Guy Harris).
- Allocations retained by long-lived wmem-owned state should use a compatible allocator/lifetime; targeted ASAN runs are useful validation for this class of leak (!2993).
- Weak heuristic recognizers should remain disabled by default and must perform bounds-safe recognition before reading packet bytes (!2985).
- For a reusable decoder or other parser with potentially large loops, fuzzing before merge can uncover malformed/truncated-input paths that positive examples miss (!2983). Its media-type caller-context bug was later corrected in !10376, so the later MR remains the stronger API precedent.
- Do not force a protocol encoding into a generic helper whose representable value/length domain does not match the wire format merely for abstraction uniformity (!2980).

No SMPTE 291/VANC packet type was encountered in this batch.
