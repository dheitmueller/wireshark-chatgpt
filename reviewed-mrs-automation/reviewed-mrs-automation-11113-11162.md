# Automated Wireshark MR review: !11113–!11162

Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`

Review direction: descending from the highest-numbered previously unreviewed MR. Before selection, the existing tracking in `reviewed-mrs.md`, the aggregate automation tracking, and the available per-run ledgers in `reviewed-mrs-automation/` were reconciled into the already-reviewed set. The historical !17571–!17620 batch remains counted. The prior run ended at !11163; the earlier lookup of !11162 was only a frontier probe and did not count as a review. No reviewed-range inference was used in selecting this batch.

Exact reviewed set (50 MRs):

`!11162, !11161, !11160, !11159, !11158, !11157, !11156, !11155, !11154, !11153, !11152, !11151, !11150, !11149, !11148, !11147, !11146, !11145, !11144, !11143, !11142, !11141, !11140, !11139, !11138, !11137, !11136, !11135, !11134, !11133, !11132, !11131, !11130, !11129, !11128, !11127, !11126, !11125, !11124, !11123, !11122, !11121, !11120, !11119, !11118, !11117, !11116, !11115, !11114, !11113`

Status weighting: 48 merged, one closed/unmerged (!11162), and one still-open draft in the corpus snapshot (!11150). The closed/open work was down-weighted; stable-branch backports were used mainly as corroboration when the master change is separately identifiable.

| MR | State / weight | Review result |
|---|---|---|
| !11162 | Closed, down-weighted | APN6 release-note proposal; unmerged/canceled pipeline. No durable convention. |
| !11161 | Merged | SMB2 TreeConnectResponse adds the Isolated Transport share flag. Protocol-specific correctness update. |
| !11160 | Merged | RTPS CRC-32C/MD5 validation support. Useful checksum-validation feature; no distinct cross-cutting rule promoted. |
| !11159 | Merged | Adds initial CQL authentication message dissection. Protocol-specific incremental coverage. |
| !11158 | Merged, deep / promoted | Removes redundant explicit initialization of static protocol/field/expert/subtree variables so zero-valued registration storage can use `.bss`; reported binary-size reduction about 1.27 MiB. Promoted with !11155/!11156 to registered-identity/static-registration guidance. |
| !11157 | Merged release backport / promoted corroboration | Release-4.0 backport of the `hf_text_only` symbolic-ID fix; Gerald Combs approved/merged. |
| !11156 | Merged master, deep / promoted | Replaces `hfinfo->id == 0` with `hfinfo->id == hf_text_only`; the numeric zero was only an incidental registry assignment. Promoted to `registered-identity-conventions.md`. |
| !11155 | Merged master, deep / promoted | Establishes zero as the static registration-variable initial state and tooling for conversion, reducing initialized binary data. Promoted with !11158 while preserving the distinction for nonzero/sentinel defaults. |
| !11154 | Merged | PostgreSQL routes GSS-API/SSPI authentication payloads to GSSAPI or NTLMSSP based on negotiated state/signature. The code explicitly notes a sample is still desirable for one SSPI/Kerberos assumption; retained as protocol-specific evidence rather than a general rule. |
| !11153 | Merged | macOS bundle dependency handling learns usable `@rpath` prefixes from executables instead of relying on manual special cases. Packaging-specific maintenance. |
| !11152 | Merged, strong corroboration | Broad conversion from anonymous `create_dissector_handle()` handles to named `register_dissector()` registrations, enabling `find_dissector`, Lua, rawshark, and fuzzshark. This strongly corroborates the existing initialization-lifecycle rule already promoted from later MRs !12158/!12160. |
| !11151 | Merged, corroboration | Frees the previous validated color-filter list before replacement rather than nulling the pointer and leaking it. Reinforces existing cleanup/ownership guidance. |
| !11150 | Open draft, strongly down-weighted | Proposed wildcard-conversation mutation by John Thacker. John explicitly withheld confidence pending a breaking capture; later OS/400 SNMP reproduction showed the proposal breaks packet-list/conversation behavior and follow-up work moved elsewhere. Valuable negative review evidence: long-standing conversation semantics need representative regression captures, but the proposed mechanics were not promoted. |
| !11149 | Merged | Makes WSLua `pinfo.in_error_pkt` writable so Lua can mark an intentionally incomplete embedded header before calling a native dissector. API capability addition; no separate convention. |
| !11148 | Merged, strong corroboration | John Thacker sets up BFCP-over-UDP conversations from SDP, avoiding reliance on BFCP's weak heuristic. Reinforces signaling/metadata-driven dispatch and conversation setup. |
| !11147 | Merged | Debian PCRE dependency correction. Packaging maintenance. |
| !11146 | Merged | GitLab CI removes an obsolete Lintian check. CI maintenance. |
| !11145 | Merged | Companion/stable CI Lintian cleanup. Corroborating maintenance only. |
| !11144 | Merged | BFCP exposes fragment-related fields when the F bit is set. Protocol-local presentation improvement. |
| !11143 | Merged | Renames `IS_FT_*` ftype macros for clearer API naming. Refactor without a new architectural rule. |
| !11142 | Merged | Display-filter second compilation stage now consumes `dfs->flags`, including parser modifications made in stage one, instead of stale original flags. Good phase-state propagation example; retained as corroboration. |
| !11141 | Merged | Display-filter diagnostic correction. User-facing error maintenance. |
| !11140 | Merged | User guide example for braces around mathematical expressions. Documentation only. |
| !11139 | Merged | MAC-LTE recent control-element support. Protocol-specific update. |
| !11138 | Merged | Adds IPFIX SRv6 SRH information elements 492–502 with a representative capture. Good capture-backed protocol update; no new cross-cutting rule. |
| !11137 | Merged | Adds the UDS standard's All Groups/all DTCs value. Protocol-specific standards correction. |
| !11136 | Merged stable backport | PPPoE/IPv6CP payload-length diagnostic correction backport. Corroborates !11134. |
| !11135 | Merged release-4.0 backport | PPPoE/IPv6CP payload-length diagnostic correction. Corroborates !11134. |
| !11134 | Merged master | PPPoE avoids a false payload-length error for IPv6CP, paralleling IPCP handling. Protocol-specific diagnostic correctness. |
| !11133 | Merged | Freezes the Qt packet list before profile/preference changes and thaws it after redissection, preventing column updates against transitional dissection state. Strong lifecycle/UI corroboration. |
| !11132 | Merged | Removes a dead store found by Clang Analyzer. Reinforces static-analysis cleanup expectations. |
| !11131 | Merged corrective revert | Reverts removal of a SequenceDialog null check after an internal `mouseMoved(NULL)` caller was missed and the change introduced a crash. Strong review lesson to audit all internal call paths before strengthening callback preconditions; retained as corroboration rather than a new file. |
| !11130 | Merged, very authoritative | Guy Harris removes casts left over from `g_snprintf()` after Wireshark moved to standard `snprintf()` under its C11 baseline. Strong corroboration that obsolete compatibility casts should not survive API/baseline changes. |
| !11129 | Merged | Removes the final `g_snprintf()` usage. Same C11/standard-library modernization series. |
| !11128 | Merged | Companion cleanup of obsolete casts from the `g_snprintf` transition. Corroboration of !11130. |
| !11127 | Merged | Debian PCRE dependency maintenance. |
| !11126 | Merged | `make-reg.py` output uses current C99/C11-compatible types. Generated/tooling modernization. |
| !11125 | Merged | Extends GLib-type conversion tooling for long/ulong forms. Mechanical modernization support. |
| !11124 | Merged | Debian PCRE dependency maintenance/backport. |
| !11123 | Merged | TLS CT log/generated data adopts current C integer types. Mechanical C baseline modernization. |
| !11122 | Merged | ASTERIX generated/source types adopt current C integer types. Mechanical C baseline modernization. |
| !11121 | Merged | PCI-ID generated/source types adopt current C integer types. Mechanical C baseline modernization. |
| !11120 | Merged | Automatic registry/data update. No convention. |
| !11119 | Merged | Automatic registry/data update. No convention. |
| !11118 | Merged | Automatic registry/data update. No convention. |
| !11117 | Merged stable backport | Netlink RTM_SETLINK handling backport. Protocol-specific correction. |
| !11116 | Merged | TCP completeness bitmask string/presentation update. Protocol-specific analysis presentation. |
| !11115 | Merged, strong corroboration | John Thacker restructures RTP Stream Dialog value/sort paths so every `rtpstream_info_calculate()` result reaches `rtpstream_info_calc_free()`. Reinforces cleanup on every return path. |
| !11114 | Merged release-4.0, deep / promoted corroboration | John Thacker fixes TCP out-of-order MSP progress and adds a representative capture plus both normal and `tshark -2` tests. Promoted as a testing rule while deferring primary architecture weight to master MR !11063. |
| !11113 | Merged master, deep / promoted | John Thacker makes recursive BER OCTET STRING reassembly IDs unique per nesting level so distinct reassemblies in the same frame cannot collide/leak. Promoted to `reassembly-instance-identity-conventions.md`. |

## Durable notebook updates

- `registered-identity-conventions.md` — commit `a90e9cb672a799055f40a0e4b2332cd1672c5736` — !11156/!11157 establish symbolic comparison of registered identities rather than incidental numeric IDs; !11155/!11158 establish the complementary zero-initialized-static storage rule while preserving explicit initialization for nonzero/sentinel defaults.
- `reassembly-instance-identity-conventions.md` — commit `4923ee72fba72f737bb0a7f68fe881c402297622` — !11113 establishes nested parser/reassembly instance identity; !11114 adds out-of-order first-pass plus second-pass testing evidence, with master architectural weight deferred to !11063.

Strong corroboration retained without duplicate rules includes !11152 (named `register_dissector()` identities), !11151 and !11115 (cleanup ownership/all exits), !11148 (SDP-driven conversation setup), !11133 (UI consistency across preference-triggered redissection), !11131 (audit internal callback call sites before removing nullability), and Guy Harris's !11130 (remove compatibility casts once the owning API/language baseline changes).

## Frontier

MR !11112 (`XMPP: Cleanup xmpp element even if there's an exception`) exists in the same corpus commit and is merged. It was inspected only to verify that the corpus continues below this batch and was **not** reviewed or counted here. The corpus therefore has not run out and no scraper restart notice is required for this run.
