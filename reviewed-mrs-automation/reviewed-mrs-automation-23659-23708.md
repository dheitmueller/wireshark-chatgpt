# Wireshark MR Review Automation: !23659–!23708

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Before selecting this batch, the already-reviewed set was rebuilt from the available tracking in `dheitmueller/wireshark-chatgpt`, including `reviewed-mrs.md`, the aggregate automation tracking, and the individual files under `reviewed-mrs-automation/`. Individual MR entries were used rather than treating range filenames as proof of coverage. This reconciliation included the separately represented !25759–!25827 material and explicitly preserves/counts the historical !17571–!17620 batch. The highest-numbered corpus MRs not present in that reconstructed set were therefore !23708 downward.

Exactly 50 previously unreviewed corpus MRs were reviewed, in descending order. The exact set is:

!23708, !23707, !23706, !23705, !23704, !23703, !23702, !23701, !23700, !23699,
!23698, !23697, !23696, !23695, !23694, !23693, !23692, !23691, !23690, !23689,
!23688, !23687, !23686, !23685, !23684, !23683, !23682, !23681, !23680, !23679,
!23678, !23677, !23676, !23675, !23674, !23673, !23672, !23671, !23670, !23669,
!23668, !23667, !23666, !23665, !23664, !23663, !23662, !23661, !23660, !23659.

## Review weighting and findings

- **!23708 — Deep/promoted, merged master.** Large QCDIAG WCDMA extension with sample captures and substantial review. John Thacker caught a derived tvbuff backed by automatic stack storage and explained that tvbuff bytes remain usable after the dissector returns (including tshark/GUI packet-byte consumers); the accepted code allocates backing storage from `pinfo->pool`. Promoted to `memory-lifetime-conventions.md`. The same review also led to later allocator cleanup in !23762, reinforcing existing wmem-scope guidance.
- **!23707 — Scanned, merged; authored by John Thacker.** Multipart fixes a sentinel comparison after a variable became unsigned, using the unsigned-domain sentinel instead of testing `<= 0`. Reinforces existing signedness/domain guidance.
- **!23706 — Scanned, merged release maintenance.** Version preparation for the 4.4 line; no reusable engineering convention.
- **!23705 — Scanned, merged release maintenance.** Version preparation for the 4.6 line; no reusable engineering convention.
- **!23704 — Deep/corroboration, merged.** MGCP removes obsolete maximum-offset bookkeeping and uses the remaining-length line-search API, eliminating an overflow-prone length calculation. Reinforces existing remaining-length/unsigned-offset API guidance.
- **!23703 — Scanned, merged.** GSM SIM exposes the MANAGE CHANNEL response channel number. Protocol-specific enhancement.
- **!23702 — Scanned, merged release maintenance.** 4.4 build preparation; no durable convention.
- **!23701 — Scanned, merged release maintenance.** 4.6 build preparation; no durable convention.
- **!23700 — Scanned, merged.** Art-Net protocol short name corrected to match the specification's canonical spelling. Protocol metadata cleanup.
- **!23699 — Deep/corroboration, merged.** More dissectors eliminate duplicate TVB value fetches. Reinforces the notebook's fetch-once/use-returning-tree-API guidance.
- **!23698 — Deep/corroboration, merged master; authored by John Thacker and merged by Michael Mann.** NVMe persists transport type in command context so a later partial RDMA dissection can recover state learned in an earlier piece. Strongly reinforces existing guidance that persistent dissection state must contain every value needed when later fragments/passes cannot reconstruct earlier context.
- **!23697 — Scanned, merged.** BLF truncated-container handling from the !23671 fix carried to another supported branch; no additional lesson.
- **!23696 — Scanned, merged.** BLF truncated-container handling from the !23671 fix carried to another supported branch; no additional lesson.
- **!23695 — Scanned, merged.** IEEE 802.11 HE Trigger Special User Info bounds/presence correction in the same accepted fix family as !23682/!23684. Reinforces validation before retrieval.
- **!23694 — Scanned, merged.** Windows GnuTLS 3.8.12 dependency update; dependency maintenance only.
- **!23693 — Scanned, merged.** GnuTLS 3.8.12 build/dependency update; no new convention.
- **!23692 — Scanned, merged.** GnuTLS 3.8.12 build/dependency update; no new convention.
- **!23691 — Scanned, merged.** Adds MCData to SDP media handling. This is the accepted successor to closed !23689, so the merged result is weighted more heavily.
- **!23690 — Deep/corroboration, merged.** Continues migration away from `tvb_find_line_end()` toward remaining-length/status-aware APIs and unsigned offsets. Reinforces existing parser API/type-domain rules.
- **!23689 — Down-weighted, closed/unmerged.** Earlier MCData SDP submission superseded by merged !23691; no durable rule taken from the abandoned revision.
- **!23688 — Deep/promoted, merged master; authored by John Thacker and approved/merged by Gerald Combs.** NSIS/MinGW packaging distinguishes paths interpreted on the build host from paths interpreted by the target installer; CMake “native” conversion follows the build system rather than magically the target. Promoted to `cross-compilation-conventions.md`.
- **!23687 — Scanned, merged.** Temporarily disables the macOS Build merge-request CI job. Operational CI maintenance, not a durable coding convention.
- **!23686 — Scanned, merged.** Windows extcap/libssh configuration-location correction in the same family as !23685/!23673; portability maintenance.
- **!23685 — Scanned, merged.** Prevents libssh from probing an inappropriate `c:\\etc\\ssh\\ssh_config` path on Windows. Platform integration fix; no broader rule beyond existing platform-semantic guidance.
- **!23684 — Scanned, merged.** IEEE 802.11 HE Trigger Special User Info presence/bounds correction; same accepted fix family as !23682/!23695.
- **!23683 — Scanned, merged.** Plot UI removes an unavailable export option. UI correctness only.
- **!23682 — Scanned, merged.** IEEE 802.11 HE Trigger Special User Info presence/bounds correction; validates the flag/state before retrieving optional data.
- **!23681 — Deep/corroboration, merged.** Multipart terminates parsing when a malformed/fuzzed input lacks the expected CRLF, preventing a non-progressing loop. Strong corroboration of the existing strict parser-progress rule.
- **!23680 — Scanned, merged.** RLC graph handles a selected bearer that may be LTE or NR. Tap/UI protocol-specific correction.
- **!23679 — Scanned, merged.** Plot automatic x-axis range considers all groups/data. UI behavior correction.
- **!23678 — Scanned, merged.** Telnet corrects a signed/unsigned comparison. Reinforces existing C type-domain guidance.
- **!23677 — Scanned, merged.** Diameter fixes argument order for `tvb_find_line_end_remaining()`. API-migration correctness; no new general rule.
- **!23676 — Scanned, merged release maintenance.** Preparation for 4.4.14; no reusable engineering lesson.
- **!23675 — Scanned, merged release maintenance.** Preparation for 4.6.4; no reusable engineering lesson.
- **!23674 — Scanned, merged.** O-RAN tap fixes end-of-beams-column termination. Boundary-condition correction covered by existing capacity/progress guidance.
- **!23673 — Scanned, merged.** Windows extcap updates the per-user SSH configuration path using the libssh expansion contract. Platform-specific integration fix.
- **!23672 — Deep/corroboration, merged master; authored and merged by John Thacker.** WCCP validates address element length every time the table is dissected so a zero element size combined with a huge packet-controlled table length cannot spin through an excessive loop. Strong corroboration of hostile-dimension validation and guaranteed-progress/resource-bound guidance.
- **!23671 — Deep/corroboration, merged master and approved/merged by Michael Mann.** BLF stops reclassifying a genuinely truncated container as `WTAP_ERR_INTERNAL`; abrupt capture truncation is an input/file condition rather than an impossible program invariant. This strongly corroborates the wiretap error-taxonomy rule already promoted from later MRs. !23696/!23697 carry the behavior to supported branches.
- **!23670 — Deep/corroboration, merged.** More dissector double-fetch elimination. Reinforces existing fetch-once/tree-return API guidance.
- **!23669 — Scanned, merged.** I/O Graph adds Alt-arrow coarse movement. UI feature only.
- **!23668 — Deep/corroboration, merged.** Earlier portion of the migration from `tvb_find_line_end()` to remaining-length/status-aware APIs with unsigned offsets; reinforced again by !23690.
- **!23667 — Scanned, merged automatic update.** Generated registries/translations/data refresh; no durable convention.
- **!23666 — Scanned, merged automatic update.** Generated registries/translations/data refresh; no durable convention.
- **!23665 — Scanned, merged automatic update.** Generated registries/translations/data refresh; no durable convention.
- **!23664 — Scanned, merged.** ICMPv6 enhancements required by RFC 9926. Standards-tracking/protocol-specific work.
- **!23663 — Scanned, merged.** Restores VCS version-header inclusion after prior refactoring. Build/source dependency correction, but too narrow for a new rule.
- **!23662 — Deep/corroboration, merged.** More dissectors use APIs that return already-fetched values rather than fetching the same bytes twice. Reinforces the existing double-fetch rule.
- **!23661 — Scanned, merged.** Corrects a `STRATOSHARK` initialization typo in `make-version.py`. Straightforward bug fix.
- **!23660 — Scanned, merged.** ICMPv6 6LoWPAN Capability Indication updated for the RFC 9926 F-bit. Standards-tracking change.
- **!23659 — Scanned, merged.** Windows libssh update to 0.11.4. Dependency maintenance only.

## Durable notebook promotion

- `memory-lifetime-conventions.md`: backing bytes for a real-data/derived tvbuff must outlive the creating dissector call and remain valid through packet processing; stack arrays are therefore invalid backing storage, while packet-scope/ownership-aware storage satisfies the contract (!23708, direct John Thacker review).
- `cross-compilation-conventions.md`: distinguish path syntax by the process/system that interprets a path. Build-time source paths follow the build host; install/runtime paths follow the target. CMake native-path conversion follows the build environment and must not be treated as an implicit target-path conversion (!23688, John Thacker; approved/merged by Gerald Combs).

Merged master work and accepted review were weighted most heavily. John Thacker's direct lifetime review in !23708 and authored build fix !23688 received elevated weight; Michael Mann's acceptance of the BLF error-taxonomy correction in !23671 was treated as strong corroboration. Closed !23689 was explicitly down-weighted in favor of merged !23691. Repeated backports, release preparation, dependency bumps, and generated-data updates were counted in the exact reviewed set but were not allowed to create duplicate notebook rules.