# Reviewed Wireshark Merge Requests !25059–!25108

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Review direction: descending MR number. This ledger records exactly the 50 previously unreviewed MRs selected for this run after consulting the existing per-run ledgers and `reviewed-mrs.md`. The previously reviewed historical batch !17571–!17620 remains part of the already-reviewed set and was preserved when selecting this batch.

| MR | Status | Review notes |
|---|---|---|
| !25108 | Scanned, merged | WSDG PowerShell environment-variable syntax correction. Documentation-only maintenance. |
| !25107 | Deep, merged, John Thacker-authored | IEEE 802.11 WFA vendor-IE fix corrects a caller/callee offset contract after the OUI had already been stripped. Strong parser-boundary corroboration: once a sub-tvb is rebased, child offsets and lengths must be relative to that representation. |
| !25106 | Deep, merged, John Thacker-authored | UDP proxy handling stopped emitting Follow Stream data twice from a helper that does not create a new stream ID. Promoted: tap/follow payload should be emitted by the semantic layer that owns the stream identity, not duplicated by nested dispatch helpers. |
| !25105 | Deep, merged | FreeBSD USB control transfers are reconstructed across related capture records before common USB dissection. Reinforces reassembly before handing logically complete control-transfer data to shared decoding. |
| !25104 | Scanned, merged | Documents UDP multicast stream behavior in the User's Guide. Documentation-only. |
| !25103 | Scanned, merged | O-RAN preference-label/UI clarification. No durable engineering rule extracted. |
| !25102 | Deep, merged | Adds MCTP-over-SMBus/I2C via a named I2C heuristic list and discoverable MCTP dissector handle, with positive, negative, malformed, truncated, reassembly, and NVMe-MI tests. Anders Broman suggested `proto_tree_add_item_ret_uint()` instead of a second tvb fetch, reinforcing existing single-extraction API guidance. |
| !25101 | Scanned, merged backport | release-4.4 backport of the EPL profile-load use-after-free fix. Corroborates !25099. |
| !25100 | Scanned, merged backport | release-4.6 backport of the EPL profile-load use-after-free fix. Corroborates !25099. |
| !25099 | Deep, merged | EPL profile-load failure deletes the partially created profile and immediately nulls the pointer so a freed allocator-backed object cannot escape the error path. Reinforces existing ownership/lifetime guidance. |
| !25098 | Scanned, merged backport | release-4.4 backport of DSM-CC loop-offset advancement. Corroborates !25094. |
| !25097 | Scanned, merged backport | release-4.6 backport of DSM-CC loop-offset advancement. Corroborates !25094. |
| !25096 | Scanned, merged, John Thacker-authored | WSDG updates remaining Visual Studio solution references from `.sln` to `.slnx`. Documentation/tooling consistency. |
| !25095 | Scanned, merged | Galileo OSNMA DSM block count corrected from 15 to the specification's 16. Straightforward spec-conformance fix. |
| !25094 | Deep, merged, John Thacker-authored | DSM-CC loop now advances `offset` for every decoded 2-byte element. Strong corroboration that parser loops must make progress in lockstep with consumed fields. |
| !25093 | Scanned, merged, John Thacker-authored | Debian packaging follows the moved `idl2wrs` path. Packaging-location synchronization. |
| !25092 | Scanned, merged, John Thacker-authored | Normalizes `epan/follow.c` to the project's four-space local convention and removes the obsolete editorconfig exception. Style-only. |
| !25091 | Scanned, merged | WSDG updates MSVC/Visual Studio 2026 and `.slnx` references. Documentation maintenance. |
| !25090 | Scanned, merged backport, John Thacker-authored | release-4.4 backport of MIH unknown-link-address offset correction. Corroborates !25088. |
| !25089 | Scanned, merged backport | release-4.6 backport of MIH unknown-link-address offset correction. Corroborates !25088. |
| !25088 | Deep, merged, John Thacker-authored | MIH helper returns the offset after the link-address type even for unknown types instead of inventing `0` as a failure sentinel that callers do not understand. Reinforces explicit parser-helper return contracts and preserving consumed progress. |
| !25087 | Scanned, merged | IEEE 802.11 Wrapped Data EXT gains authentication-frame element dissection. Protocol coverage extension. |
| !25086 | Scanned, merged backport | Stable-branch EPL profile-load UAF correction. Same accepted lifetime fix as !25099. |
| !25085 | Deep, merged | Follow-up that mandates C++ `override`; review explicitly moved its prerequisite !25084 into GitLab's MR dependency/blocking relationship. Promoted: encode real MR dependencies in repository metadata rather than only prose. |
| !25084 | Deep, merged | Adds `override` across existing overridden C++ methods, improving signature checking and making override intent explicit. Discussion records AI-assisted tooling disclosure and its dependency relationship with !25085. |
| !25083 | Scanned, merged | Restores executable bits for dissector-generator scripts. Repository metadata/tooling fix. |
| !25082 | Scanned, merged | Debian packaging fix follows the moved `idl2wrs` path in an additional patch. Packaging follow-up. |
| !25081 | Deep, merged, John Thacker-authored | OSS-Fuzz-found V5UA UB: `uint16_t` promotes to signed `int`, so the cast must occur before multiplication. Strong corroboration of the existing rule to prove and widen intermediate arithmetic, not merely the final destination. |
| !25080 | Scanned, merged | DMX manufacturer-ID update script tolerates known upstream table formatting defects. Tooling robustness; no broader rule promoted. |
| !25079 | Scanned, merged | `check_apis.py` checks optional dictionary keys when `--group` has no maximum count. Tooling correctness. |
| !25078 | Deep, merged, John Thacker-authored | NTP mode-7 parsing treats a nonzero item count with zero item length as malformed and throws rather than repeatedly parsing a zero-width structure. Reinforces parser-progress and malformed-input invariants. |
| !25077 | Scanned, merged | DECT NR CVG PDU IE dispatch refactored to a dissector table, making IE handling consistent and extensible. Reinforces registry-based subdissector dispatch. |
| !25076 | Scanned, merged | MSVC builds unconditionally use `/bigobj` to avoid object-section limits. Build-system compatibility. |
| !25075 | Deep, merged | FreeBSD USB parsing is refactored into common USB code to share semantics across platforms and reduce API-maintenance duplication. John Thacker requested a sample capture and explicitly noted that captures attached to MRs/issues feed automatic fuzz testing. Promoted to testing notes. |
| !25074 | Scanned, merged | nl80211 adds HT/VHT MCS and capability decoding. Protocol coverage extension. |
| !25073 | Scanned, merged, Gerald Combs-authored | Automated weekly data update; MR records a DMX source-table parsing failure while the remaining generated updates passed and merged. Tooling/data maintenance. |
| !25072 | Scanned, merged | Automated weekly generated-data update. No durable coding rule extracted. |
| !25071 | Scanned, merged | Automated weekly generated-data update. No durable coding rule extracted. |
| !25070 | Scanned, merged backport | WiX generated Qt component IDs sanitize against an allowlist of valid characters. Packaging robustness; no general convention promoted. |
| !25069 | Scanned, merged backport | Stable-branch FCoE/Fibre Channel ELS loop-offset correction. Corroborates !25066. |
| !25068 | Scanned, merged backport | Stable-branch FCoE/Fibre Channel ELS loop-offset correction. Corroborates !25066. |
| !25067 | Scanned, merged | Defines an explicit log domain for UI preferences. Logging organization. |
| !25066 | Deep, merged | Fibre Channel ELS parser advances an offset that historically remained fixed inside a bounded loop. Reinforces parser-loop progress and one-element/one-advance accounting. |
| !25065 | Scanned, merged | Registers a previously unregistered subtree and initializes protocol IDs. Structural registration correctness; aligns with existing checker/lifecycle guidance. |
| !25064 | Scanned, merged | `tvbuff` captured-length wrapper delegates to the internal helper rather than duplicating its logic. Single-source implementation cleanup. |
| !25063 | Deep, merged | CDP loop termination broke after a signed loop variable was changed to unsigned because the code relied on reaching `-1`. Reinforces auditing control-flow semantics when changing integer signedness. |
| !25062 | Scanned, merged | GitLab Windows CI switches to Visual Studio 2026. Build/CI maintenance. |
| !25061 | Scanned, merged | DCT2000 caps E-DCH DDI entry counts to the fixed implementation maximum before iterating/copying. Reinforces bounding externally supplied counts to destination capacity. |
| !25060 | Scanned, merged backport | Stable-branch version of the DCT2000 E-DCH DDI count cap. Corroborates !25061. |
| !25059 | Scanned, merged | nl80211 decodes `NL80211_ATTR_PROTOCOL_FEATURES` values rather than leaving the bitset opaque. Straightforward protocol-field coverage. |

## Durable conclusions

* **Follow/tap emission belongs to the stream-identity owner.** !25106 shows that a nested UDP dispatch helper which does not create a new stream ID must not re-emit the same payload to Follow Stream; doing so duplicates bytes under one logical stream.
* **Use explicit MR dependency metadata for dependent changes.** !25085's review moved its dependency on !25084 into GitLab's blocking/dependency relationship rather than leaving it merely mentioned in prose.
* **Attached captures participate in upstream fuzzing.** In !25075 John Thacker explicitly requested the reproducer capture and explained that captures attached to MRs/issues are among the inputs to automatic fuzz testing. This strengthens the practical value of supplying small representative captures during review.
* !25081 strongly corroborates the existing arithmetic rule that widening must happen before the potentially overflowing operation; a cast of the completed result is too late after C integer promotions.
* !25088, !25094, !25078, !25066, and !25063 collectively reinforce parser/control-flow invariants around monotonic offset progress, explicit return semantics, nonzero element widths, and signedness-sensitive loop termination.

Exactly 50 MRs were reviewed in this run; no MR below !25059 was included.