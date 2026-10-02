# Review findings — !2011–!2060

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Exactly 50 MRs were reviewed, descending !2060 through !2011. Outcomes: 44 merged; closed/unmerged !2060, !2041, !2026, !2025, !2019, and !2012. Merged work was weighted above closed or superseded work; direct guidance from Guy Harris and other core maintainers was given correspondingly high weight.

## Exact per-MR findings

| MR | Outcome | Review result |
|---|---|---|
| !2060 | Closed | Guy Harris-authored precursor moving Wiretap format registration into owning modules. Down-weighted because it did not merge and later merged Guy-authored registry work (!2092 and related) is stronger evidence. Guy explicitly said not to reopen this branch. |
| !2059 | Merged | Guy Harris RTP-player narrowing-warning cleanup. Straightforward type-safety maintenance; no new convention beyond existing narrowing guidance. |
| !2058 | Merged | Guy Harris changes SCSI counts, lengths, sizes, sequence-like values, and similar quantities to `BASE_DEC_HEX`; strong corroboration of semantic numeric-display policy. |
| !2057 | Merged | Guy Harris makes the same `BASE_DEC_HEX` policy explicit for InfiniBand, iSCSI, and NVMe quantities. |
| !2056 | Merged | DNS ZONEMD RR support (RFC 8976). Straightforward protocol addition; no durable review correction beyond ordinary correctness. |
| !2055 | Merged | Van Jacobson PPP compression support. Guy Harris clarifies that an unmasked/non-bitfield `FT_BOOLEAN` has no numeric base; the boolean display argument is not reused as a containing-field width unless bitfield semantics require it. |
| !2054 | Merged | ESP UAT key validation. Invalid key text is rejected at the UAT boundary with concrete allocated error strings; Gerald Combs steers the implementation toward direct `g_strdup_printf()` use. |
| !2053 | Merged | SOME/IP crash after faulty UAT config. Relevant tables are marked `UAT_AFFECTS_DISSECTION | UAT_AFFECTS_FIELDS`, making dynamic-field rebuild semantics explicit. |
| !2052 | Merged | SOME/IP copy/paste fix correctly clears an invalid method name rather than the service name. Local correctness fix. |
| !2051 | Merged | Moves GLib includes outside broad C-linkage regions in shared headers. Merged successor to !2041; corroborates the stronger later header-linkage convention already present in the notebook. |
| !2050 | Merged | NVMe/RDMA property decoding plus substantial workflow discussion. Anders Broman describes the historical expectation of one coherent MR/commit and independent MRs (or holding a dependent MR until its prerequisite lands); the accidental squash with !2049 destroyed clean commit identity. |
| !2049 | Merged | RDMA payload tracking. Cross-platform CI caught a Windows C4098 error from returning values from a void function. Also demonstrates the practical cost of stacking a dependent MR on an unmerged sibling. |
| !2048 | Merged | NR RRC preference allowing NAS to be attached at the root tree. Straightforward presentation preference. |
| !2047 | Merged | RRC counterpart of !2048. No additional convention. |
| !2046 | Merged | NVMe Identify command fix. Anders Broman requests `proto_tree_add_item_ret_uint()`; accepted code uses the standard display-and-return helper even though the on-wire field is one byte and the helper returns through wider integer storage. |
| !2045 | Merged | Very strong Guy Harris field-presentation guidance: addresses/keys may naturally be hex, but sizes, counts, lengths, and sequence numbers should be decimal, or `BASE_DEC_HEX` when showing both is useful. The subsequent Guy-authored !2057/!2058 series makes this policy systematic. |
| !2044 | Merged | NVMe-oF/RDMA private-data offset correction. Review confirms that the subdissector receives an already-sliced private-data TVB, so offsets are relative to the child TVB rather than the original parent packet. |
| !2043 | Merged | Adds documentation to ESP key-setting API. Documentation-only, no new rule. |
| !2042 | Merged | Makes unused/external-by-accident dissector symbols static. Ordinary linkage hygiene. |
| !2041 | Closed | First GLib/`extern "C"` submission. Guy Harris, Alexis La Goutte, and Pascal Quantin point out that submitting from fork `master` prevents the maintainer-edit/rebase setting; contributor must use a separate topic branch. Superseded by merged !2051. |
| !2040 | Merged | P1 extension-attribute state moves from an ad-hoc `pinfo->private_table` string-key hash into `p_add_proto_data` / protocol-scoped packet data. Prefer the packet proto-data API for protocol-owned per-packet context. |
| !2039 | Merged | Adds more Diameter 3GPP Access-Restriction-Data flags. Routine field coverage. |
| !2038 | Merged | Guy Harris removes Wiretap's ad-hoc custom-block registry and defines explicit semantic internal block classes. Strong abstraction evidence when combined with !2033/!2036. |
| !2037 | Merged | UFTP default-port macro typo fix. No durable lesson. |
| !2036 | Merged | Guy Harris adds public `wtap_block_get_type()` and makes block registration derive the type from the block descriptor. Opaque objects should expose semantic queries instead of forcing callers to know representation details. |
| !2035 | Merged | Gerald Combs makes TShark extcap preference registration demand-driven. Windows measurements show process count and test runtime dropping sharply versus unconditional extcap startup; conditional initialization must also deliberately tolerate preferences for a subsystem intentionally not registered in that execution path. |
| !2034 | Merged | 29West documentation. Documentation-only. |
| !2033 | Merged | Guy Harris renames Wiretap block constants by semantic meaning and documents that these identifiers are internal abstractions, not pcapng on-disk block-type numbers; multiple on-disk forms may map to one internal block type. |
| !2032 | Merged | GSM statistics tables gain an item-free callback. Routine lifecycle cleanup. |
| !2031 | Merged | GSM statistics table is created once and reset on reinitialization rather than rebuilt repeatedly. Useful lifecycle cleanup, but no new broader rule. |
| !2030 | Merged | Clang Analyzer dead-store cleanup. Retained mainly as corroboration that static-analysis warnings require semantic inspection; later reviewed maintainer guidance is stronger. |
| !2029 | Merged | Guy Harris: do not tell users to file an Npcap bug unless the actual runtime capture library is Npcap. Diagnostic remediation must follow the real backend, not merely the platform. |
| !2028 | Merged | Stable-branch counterpart of !2029; same runtime-backend diagnostic rule. |
| !2027 | Merged | TCP RTO reference calculation is corrected to prefer the relevant previous unacknowledged packet. Anders Broman notes that reporter validation of the proposed behavior would speed review. |
| !2026 | Closed | Sharkd session-request proposal. Peter Wu catches the fixed-buffer off-by-one: reject `strlen(src) >= sizeof(dst)` so the terminating NUL fits. He also requires API direction to be discussed first; author abandons this design in favor of JSON-RPC alignment. |
| !2025 | Closed | QUIC version-negotiation draft support. Alexis La Goutte rules it an enhancement, not a stable-branch bug fix; no backport. |
| !2024 | Merged | More internal dissector data/functions made static. Ordinary linkage hygiene. |
| !2023 | Merged | RTP stream dialog preserves selection across retap/recalculation. UI-state fix without broader review guidance. |
| !2022 | Merged | Pascal Quantin fixes PDCP-NR builds when no cipher/integrity implementation is available by moving implementation-specific variables into their `#ifdef` scopes. Optional-feature-off configurations must compile cleanly. |
| !2021 | Merged | Guy Harris enriches Npcap bug-report diagnostics with OS, capture-library, adapter, and native-error context. |
| !2020 | Merged | Stable counterpart of !2021; same diagnostic-context lesson. |
| !2019 | Closed | Proposed PDCP-NR unused-variable workaround. Pascal Quantin closes it because merged !2022 fixes the problem in a way better matching the function's code structure. Superseded evidence only. |
| !2018 | Merged | Guy Harris uses the human-facing interface display name in capture errors. |
| !2017 | Merged | Guy Harris expands capture-error remediation so Npcap-specific failures are directed to the correct upstream project with useful troubleshooting details. |
| !2016 | Merged | Guy Harris includes interface identity in capture errors and separates primary diagnosis from secondary detail. |
| !2015 | Merged | Stable counterpart of !2018; same human-facing interface-name rule. |
| !2014 | Merged | Guy Harris recognizes Windows "device has been removed" as a real detach condition rather than automatically a Wireshark bug. |
| !2013 | Merged | Guy Harris replaces exact English matching of a Windows PacketReceivePacket error with stable prefix + native error-code-suffix matching because the embedded message may be localized. Strong platform-error parsing precedent. |
| !2012 | Closed | Gerald Combs draft Homebrew/pkg-config workaround. Peter Wu identifies a mismatched local pkg-config/OS environment as the root cause and prefers fixing that environment plus staying close to upstream CMake Find modules; Gerald confirms and closes the workaround. |
| !2011 | Merged | Large RTP Player routing/silence/filter improvement. Accepted UI work with no substantive human-review convention in the corpus snapshot. |

## Strongest durable evidence

- !2045 + Guy-authored !2057/!2058: semantic numeric quantities are decimal-first; use `BASE_DEC_HEX` when dual presentation helps, while identifiers/addresses/keys may remain hex.
- Guy-authored !2033/!2036/!2038: Wiretap block identifiers model internal semantics, not file-format numeric namespaces. Name them semantically, query them through the opaque API, and avoid ad-hoc custom-slot mechanisms.
- Guy-authored !2013 plus !2016–!2021 and !2028/!2029: diagnostics should identify the actual failing interface/backend, preserve useful native context, and match localized platform failures using stable machine structure such as native codes rather than full localized text.
- !2035 (Gerald Combs): expensive optional external-process initialization should be demand-driven and justified with measurements; preference parsing must consciously support execution modes where the optional subsystem was intentionally not registered.
- !2054/!2053: validate UAT input before it becomes runtime dissector state, return actionable errors, and declare when UAT changes affect dynamic field registration.
- !2050/!2049 plus !2041: keep MRs coherent and independently mergeable where possible; sequence genuine dependencies explicitly and use a topic branch that permits maintainer rebasing instead of fork `master`.
- !2055 (Guy Harris) and !2046 (Anders Broman): model field semantics correctly and use standard typed add-and-return APIs instead of manual fetch/display duplication.
- !2040: use protocol-scoped packet proto-data APIs for per-packet protocol context rather than an ad-hoc private hash.
- Closed !2026 and !2012 supply useful lower-weight negative guidance on NUL-capacity checks/API design and on fixing broken build environments at the root rather than accumulating local CMake workarounds.

No SMPTE ST 291/VANC packet type was encountered.
