# Automated MR review ledger: !11463–!11512

Reviewed on 2026-09-24 using GPT-5.6 Sol.

Corpus: `dheitmueller/wireshark-corpus-mrs`

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Notebook starting commit: `90290426c6ed87a67e712b4d3ebe121b0f42fd92`

Review direction: descending from the newest previously unreviewed MR toward older MRs.

Before selecting this batch, the already-reviewed set was reconstructed from the available per-run files under `reviewed-mrs-automation/` together with `reviewed-mrs.md`. The historical !17571–!17620 batch remains explicitly preserved and counted. Selection was based on individual MR membership, not inferred numeric-range coverage. The prior mention of !11512 was only a frontier probe and therefore did not count as a review.

Exactly 50 previously unreviewed MRs were selected and reviewed in descending order:

!11512, !11511, !11510, !11509, !11508, !11507, !11506, !11505, !11504, !11503, !11502, !11501, !11500, !11499, !11498, !11497, !11496, !11495, !11494, !11493, !11492, !11491, !11490, !11489, !11488, !11487, !11486, !11485, !11484, !11483, !11482, !11481, !11480, !11479, !11478, !11477, !11476, !11475, !11474, !11473, !11472, !11471, !11470, !11469, !11468, !11467, !11466, !11465, !11464, !11463.

There are 46 merged MRs and four closed/unmerged MRs in the batch. !11511, !11481, !11470, and !11465 were deliberately down-weighted. !11487 was merged but is also down-weighted because !11488 immediately corrects its CI-job placement. Stable-branch backports were treated mainly as corroboration of accepted master behavior.

## Per-MR review

| MR | Review outcome |
| --- | --- |
| !11512 | Merged automatic data/translation update. Low architectural signal. |
| !11511 | **Closed/unmerged; down-weighted.** Initial WOWW definition update was closed while CI/issues were corrected and superseded by merged !11515 from the previous batch. |
| !11510 | Merged Qt/UI cleanup for the protocol-tree display-filter reference link. Review consolidated duplicated QString/URL construction; useful DRY/UI code-review evidence, not a new cross-cutting rule. |
| !11509 | Merged `make-enterprises` maintenance, including generated-source last-updated metadata and script/documentation cleanup. Generated-data maintenance. |
| !11508 | Merged RTP cleanup. `get_rtp_packet_info()` has a non-NULL postcondition at the use site and now owns complete population of the packet-info structure; callers drop a redundant check. Corroborates trusting documented helper contracts and centralizing struct initialization. |
| !11507 | Merged release-3.6 backport of DHCPFO control-flow fix from !11504. A NULL protocol tree must not suppress expert/non-tree dissection side effects. Corroborates `dissector-pipeline-conventions.md`. |
| !11506 | Merged release-4.0 backport of the DHCPFO NULL-tree fix. |
| !11505 | Merged typed-item/add-bitmask field-mask corrections by Martin Mathieson. Strong corroboration of typed-item checker guidance. |
| !11504 | **Deep/corroboration, merged master.** Removes DHCPFO's early `if (!tree) return` because expert information and other semantic dissection must still happen when no tree is being built, including tap/display-filter paths. Existing `dissector-pipeline-conventions.md` already captures this rule. |
| !11503 | Merged checker-driven correction of `add_bitmask()` field arrays/masks. Corroborates typed-item checker conventions. |
| !11502 | **Deep/corroboration, merged.** RLC-NR fixes the declared length of a composite `wmem_tree_key_t`; omitting the intended component made distinct logical keys alias. Reinforces complete semantic-key identity in `conversation-identity-conventions.md`/container key guidance. |
| !11501 | Merged SCTP zero-checksum acceptable-parameter update to newer specification text. Protocol-specific standards synchronization. |
| !11500 | Merged Q.931 symbolic-value correction so value strings occupy the field's decoded logical domain. Corroborates masked-field/value-string semantics. |
| !11499 | Merged dead-store cleanup found by Clang Analyzer. Static-analysis maintenance. |
| !11498 | Merged MySQL/MariaDB `CLIENT_SSL_VERIFY_SERVER_CERT` capability support. Protocol/security feature work; no new general rule extracted. |
| !11497 | Merged Windows Arm64 CI path/plumbing update. Build-system maintenance. |
| !11496 | **Deep, merged master; authored and merged by John Thacker.** Separates RTP conversation state from per-packet state and explicitly avoids copying conversation-stable data into every packet. Promoted to `resource-lifetime-scope-conventions.md`. |
| !11495 | Merged typed-item checker improvements plus resulting field fixes. Strong corroboration of checker-driven semantic validation. |
| !11494 | **Deep/corroboration, merged.** SRT handshake request type can encode rejection values over 1000. Review by Alexis La Goutte explicitly suggested `BASE_CUSTOM` for transformed presentation rather than ad-hoc field text. Accepted behavior reinforces keeping typed field semantics and custom presentation in field metadata/formatting mechanisms. |
| !11493 | **Deep, merged master; authored and merged by John Thacker.** RTP sequence/timestamp history is scoped by SSRC, not merely the RTP 5-tuple/session. Multiple SSRCs therefore require independent state namespaces. Promoted to `conversation-identity-conventions.md`. |
| !11492 | Merged Windows Arm64 release-note update. Documentation maintenance. |
| !11491 | Merged Windows Arm64 CI tooling adjustment so the job uses a native/appropriate Python-based copy path instead of relying on incompatible tooling. Build-plumbing evidence. |
| !11490 | Merged Windows Arm64 CMake/Qt prefix-path adjustment. Platform build maintenance. |
| !11489 | Merged spelling/comment cleanup. No engineering rule. |
| !11488 | Merged correction placing the Python-path setup in the intended Windows Arm64 job. This is the accepted follow-up to !11487. |
| !11487 | Merged but immediately superseded/corrected by !11488 after the setup was placed in the wrong CI job. Down-weighted as transient accepted state. |
| !11486 | Merged stable-branch CI tag/variable maintenance. |
| !11485 | Merged stable-branch CI tag/variable maintenance. |
| !11484 | Merged Windows Arm64 package-job support and related CMake/build adjustments. Useful platform CI evidence, but no separate durable convention extracted. |
| !11483 | Merged, authored by Guy Harris. Refactors `rdps` generated text emission into static byte arrays plus a single `fwrite()` path rather than many formatting calls; also removes escaping required only by printf formatting. High-authority code-generation/performance cleanup, but narrower than existing generated-code guidance. |
| !11482 | Merged, authored by Guy Harris. Removes redundant `script_name` assignment already performed by the common main path. Straightforward initialization cleanup. |
| !11481 | **Closed/unmerged; down-weighted.** Proposed suppressing a Debian `dh_shlibdeps` failure with `--ignore-missing-info`; author later resolved the problem by using the correct pbuilder environment and closed the MR. Negative evidence against masking an environment/setup failure with a broad checker suppression. |
| !11480 | Merged DNS Info-column fencing/append behavior for packets containing multiple DNS messages (for example DoQ). Includes a representative capture. Corroborates preserving/aggregating summary columns when one packet contains multiple logical PDUs. |
| !11479 | Merged, authored by Guy Harris. Expands commentary/research around reliable Linux distribution identification and fallbacks. Useful platform investigation notes but no runtime behavior change. |
| !11478 | Merged Wireshark User's Guide preference-pane subsection organization. Documentation-only. |
| !11477 | **Deep/corroboration, merged; authored by Guy Harris.** Renames pcap/pcapng length fields, variables, and expert text to the specification's terms “Original Packet Length” and “Captured Packet Length.” Strong evidence that ambiguous protocol/file-format concepts should use the normative specification terminology consistently. |
| !11476 | Merged John Thacker leak fix calling `wtap_dump_params_cleanup()` on success, abort, and failure exits. Corroborates the cleanup-ownership rules promoted from !11546. |
| !11475 | Merged checker-driven item/call and bitmask corrections by Martin Mathieson. Strong corroboration of typed-item checker conventions. |
| !11474 | **Deep/corroboration, merged.** Adds the Zabbix dissector with TCP desegmentation, compression/TLS handling, conversation response-time tracking, explicit supported-version limits, protocol documentation references, and representative captures for both current and old Zabbix deployments. Strong submission/testing exemplar for substantial new dissectors. |
| !11473 | Merged removal of an unused AMR protocol-item assignment. Straightforward cleanup. |
| !11472 | Merged John Thacker memory/semantic cleanup: a dependent-frame `GHashTable` is conceptually a set, so it uses `g_hash_table_add()` rather than storing unused NULL values. Good evidence for choosing a container API that exposes the real data model and can enable implementation optimizations. |
| !11471 | Merged John Thacker packet-range dependency fix. Selection is determined first and then all transitive dependencies are included for every range mode, instead of adding only an ad-hoc subset of non-displayed dependencies. Strong graph/dependency correctness evidence, but application-specific. |
| !11470 | **Closed/unmerged; down-weighted.** Later iPerf3 dissector proposal with extensive review and sample captures. Despite a successful later pipeline it was closed without merging, so discussion is useful context but not accepted implementation precedent. |
| !11469 | **Deep, merged master; authored by Guy Harris.** Windows `GetNativeSystemInfo()` can expose an emulated x86/x64 view on ARM64, so Wireshark reports only the reliable 32/64-bit property instead of claiming a native instruction-set identity it cannot know. Promoted to `platform-api-portability-conventions.md`. |
| !11468 | **Deep, merged master; authored by Guy Harris.** Removes an obsolete MSVC 6 workaround because the supported codebase already relies on the modern `VER_NT_WORKSTATION`/`OSVERSIONINFOEX` contract elsewhere. Promoted to `platform-api-portability-conventions.md`. |
| !11467 | Merged release-4.0 backport of the !11466 comment typo fix. No engineering rule. |
| !11466 | Merged master comment typo fix, authored by Guy Harris. No engineering rule. |
| !11465 | **Closed/unmerged; down-weighted.** Draft predecessor of the iPerf3 dissector work, superseded by !11470. No accepted convention extracted. |
| !11464 | Merged release-4.0 backport adding a Linux capabilities-specific permission diagnostic for dumpcap. Corroborates actionable, platform-conditioned error guidance. |
| !11463 | Merged release-3.6 backport of the same dumpcap Linux capabilities permission diagnostic. |

## Durable notebook updates

1. `conversation-identity-conventions.md` — commit `0dcc6f49370cafd360c6024b1228de1837dd8f1d`
   - !11493: scope sequence/timestamp/counter state by the protocol-defined namespace (SSRC for RTP), not merely by the enclosing transport conversation.
   - !11502 retained as corroboration that composite key descriptors must include every intended identity component.

2. `resource-lifetime-scope-conventions.md` — commit `30a802615098282ba813584cf75490c35a72e4bc`
   - !11496: partition persistent state by semantic lifetime; conversation-stable values should not be duplicated in every packet record.

3. `platform-api-portability-conventions.md` — commit `833eaba468784797a730fc9a34d6a7e6ec3bd930`
   - Guy Harris's !11469: report only platform facts an introspection API can guarantee under emulation/compatibility layers.
   - Guy Harris's !11468: remove legacy compatibility branches when the current supported baseline and surrounding code already require the newer contract.

Existing notebook rules were deliberately reused instead of duplicated: !11504/!11506/!11507 reinforce the NULL-tree semantic-processing rule in `dissector-pipeline-conventions.md`; !11505/!11503/!11495/!11475 reinforce typed-item checker conventions; !11476 reinforces cleanup ownership; and !11500 reinforces masked-field logical-value semantics.

## Frontier probe

After fixing the exact 50-MR batch, !11462 (`Qt: Change handling of folder name text entry`) was inspected only to verify that the corpus continues. It exists and is merged, but **was not counted as reviewed in this run**. It remains the next descending candidate absent newly scraped higher-numbered unreviewed material.

The corpus therefore has not run out, and no scraper restart is required.
