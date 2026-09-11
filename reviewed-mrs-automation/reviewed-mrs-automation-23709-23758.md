# Wireshark MR Review Automation: !23709–!23758

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Before selecting this batch, the already-reviewed set was rebuilt from the available tracking in `dheitmueller/wireshark-chatgpt`, including `reviewed-mrs.md`, the aggregate automation tracking where applicable, and the per-run files under `reviewed-mrs-automation/`. Individual MR entries were used rather than treating a filename/range as proof that every number was reviewed. The historical !17571–!17620 batch remains explicitly preserved and counted. The immediately preceding exact ledger records every MR !23759–!23808, and the higher-numbered tracking covers the corpus through its current maximum !26393.

Exactly 50 previously unreviewed corpus MRs were reviewed, in descending order. The exact set is:

!23758, !23757, !23756, !23755, !23754, !23753, !23752, !23751, !23750, !23749,
!23748, !23747, !23746, !23745, !23744, !23743, !23742, !23741, !23740, !23739,
!23738, !23737, !23736, !23735, !23734, !23733, !23732, !23731, !23730, !23729,
!23728, !23727, !23726, !23725, !23724, !23723, !23722, !23721, !23720, !23719,
!23718, !23717, !23716, !23715, !23714, !23713, !23712, !23711, !23710, !23709.

## Review weighting and findings

- **!23758 — Deep/corroboration, merged stable backport.** BT HCI ISO reassembly uses captured length equal to the bytes actually initialized rather than the logical expected length. Strongly corroborates the existing captured-vs-reported-length/reassembly rule represented by the master fix.
- **!23757 — Scanned, merged.** PA-HBBACKUP switches a literal/string column update to `col_add_str()` rather than unnecessary formatting. Straightforward API cleanup.
- **!23756 — Deep/corroboration, merged.** Gerald Combs fixes clang scan-build findings by zero-initializing process structures and removing a duplicate free already performed by cleanup. Reinforces existing initialization, ownership, and static-analysis guidance.
- **!23755 — Scanned, merged stable backport.** GitLab package-job environment assignments; weighted below the master change !23753.
- **!23754 — Scanned, merged stable backport.** Same CI environment work for another stable branch, plus the corresponding macOS runner-tag correction; no new general rule.
- **!23753 — Scanned, merged master.** Gerald Combs assigns explicit GitLab environments to Windows/macOS packaging jobs. Useful CI organization, but not broad enough for a new notebook convention here.
- **!23752 — Discussion-focused, merged.** ICMP Code=0 validation was expanded after Jaap Keuter questioned inconsistent coverage of deprecated versus nondeprecated message types. Useful evidence that protocol validation should follow the protocol rule consistently rather than incidental deprecation status; existing validation guidance already captures the broader principle.
- **!23751 — Scanned, merged.** RPM packaging accepts the distro-specific OpenCore-AMR development package name on SUSE. Packaging portability maintenance.
- **!23750 — Scanned, merged.** `rpm-setup.sh` similarly tries the distro-specific OpenCore-AMR package alternative. No new cross-cutting convention.
- **!23749 — Deep/corroboration, merged master.** Master BT HCI ISO fix corresponding to !23758: never expose uninitialized reassembly storage as captured bytes. Existing TVB/reassembly guidance already records the durable lesson.
- **!23748 — Discussion-focused, closed/unmerged.** Proposed broad `ws_free()` wrapper triggered scope questions and was ultimately closed. Useful negative/design evidence, but deliberately down-weighted because the API was not accepted.
- **!23747 — Scanned, merged.** Procmon frees a temporary string buffer on an early-return path after Coverity reports a leak. Reinforces ordinary ownership cleanup.
- **!23746 — Scanned, merged.** BLF avoids repeatedly allocating/losing a channel-name string. Reinforces ownership/lifetime guidance.
- **!23745 — Scanned, merged.** PDCP-NR digest calculation stops subtracting SDAP bytes a second time after deciphering already removed them. Protocol-specific offset/state correction.
- **!23744 — Deep, merged.** ISAKMP NAT-detection verification received concrete Jaap Keuter review on commit-message format and use of standard `stdint.h` integer types. Both conventions already exist in the notebook, so this is corroboration rather than a duplicate promotion. This merged MR supersedes closed !23743.
- **!23743 — Down-weighted, closed.** Earlier ISAKMP NAT-detection submission was closed and replaced by !23744; no rule is taken from the abandoned revision over the merged result.
- **!23742 — Deep/corroboration, merged.** Avoids passing `tvb_reported_length_remaining()` directly through `MIN` because the macro may evaluate an argument more than once; accepted review kept the intent explicit by computing `remaining` once. Reinforces the existing fetch-once/avoid duplicated evaluation rule.
- **!23741 — Deep/corroboration, merged.** SolarEdge adds length checks, avoids unnecessary copies, and addresses Coverity issues. Reinforces existing hostile-length, capacity, and ownership guidance.
- **!23740 — Scanned, merged.** TPNCP adds a preference controlling the `tpncp.dat` path. Preference/user-configurability feature; no new cross-cutting rule.
- **!23739 — Scanned, merged automated update.** Generated registries/translations/data refresh; no durable review convention.
- **!23738 — Scanned, merged automated stable update.** Same generated-data maintenance family; weighted below the source update.
- **!23737 — Scanned, merged automated update.** Generated registries/translations/data refresh; no new rule.
- **!23736 — Scanned, merged.** Initializes a scalar flagged by Coverity. Reinforces total initialization guidance.
- **!23735 — Deep/corroboration, merged.** Conversion calls now treat `ws_strtoi64()`/`ws_strtou64()` status correctly rather than leaving checked results unexamined. John Thacker merged the corrected form; existing status/out-parameter and static-analysis guidance covers the general lesson.
- **!23734 — Scanned, merged.** Initializes `tm_isdst` before time conversion in CoLa/Navitrol, resolving a Coverity warning and making the structure contract explicit.
- **!23733 — Scanned, merged.** `.editorconfig` records tab style for CoLa/Navitrol dissectors. Local formatting/tooling maintenance.
- **!23732 — Deep/corroboration, merged.** Removes impossible/redundant null checks that confused Coverity where control flow already guarantees allocation. Reinforces the rule to fix analyzer models/code clarity without adding meaningless defensive branches.
- **!23731 — Strong corroboration, merged release-4.4 backport.** Backports the `G_GNUC_MALLOC` semantic-contract correction from !23728.
- **!23730 — Strong corroboration, merged release-4.6 backport.** Backports the `G_GNUC_MALLOC` semantic-contract correction from !23728.
- **!23729 — Scanned, merged.** Makes a couple of dissector symbols `static`; ordinary symbol-scope cleanup.
- **!23728 — Deep/promoted, merged master; authored and merged by John Thacker.** Removes `G_GNUC_MALLOC`/`__attribute__((malloc))` from constructors whose returned objects retain pointers to their allocator. Compiler attributes are semantic contracts: the optimizer may infer non-aliasing guarantees, so they must not be applied merely because the outer object is newly allocated. Promoted to `assertion-static-analysis-conventions.md`; stable backports !23730/!23731 corroborate it.
- **!23727 — Down-weighted, open draft.** Experimental QCDIAG USB dissector registration remains unmerged, so it is retained only as provisional design evidence.
- **!23726 — Scanned, merged.** Fourth batch removing protocol-registration macros. Continues existing cleanup away from unnecessary registration wrappers; no distinct new rule.
- **!23725 — Scanned/corroboration, merged stable backport.** UAT lexer handles an empty final hex string/EOF record correctly; weighted below the master change.
- **!23724 — Deep/corroboration, merged master.** UAT lexer returns the newline expected by `END_OF_RECORD` and handles final empty hex strings consistently at EOF. Useful parser-state-machine edge case but already covered by parser progress/edge-case guidance.
- **!23723 — Deep/corroboration, merged.** More dissectors avoid separate repeated TVB fetches. Reinforces the existing fetch-once rule for clarity, bounds consistency, and avoiding duplicate work.
- **!23722 — Scanned, merged.** MMS ASN.1 typo correction to match the actual/generated specification expectations. Protocol-generator maintenance.
- **!23721 — Deep/corroboration, merged.** Uses the proper `wmem_strbuf_finalize()` ownership transition to resolve a Coverity resource leak. Reinforces allocator/ownership contracts already recorded.
- **!23720 — Scanned, merged.** Qt calculation fixes unintended integer division reported by Coverity. Numeric-expression correctness, already covered by arithmetic/type guidance.
- **!23719 — Deep/corroboration, merged.** Follow-up to !23715: intentionally ignored checked-return results are made explicit where the caller truly does not need them, rather than inventing meaningless handling. Reinforces established static-analysis guidance.
- **!23718 — Deep/corroboration, merged.** Nettrace removes/changes unsafe XML parser configuration after Coverity. Strong independent corroboration of the existing untrusted-XML rule.
- **!23717 — Deep/corroboration, merged.** WiMAX ASN CP similarly fixes unsafe XML parser configuration. Reinforces the existing `XML_PARSE_NOENT`/unsafe-substitution security guidance.
- **!23716 — Scanned/corroboration, merged stable backport.** UAT empty-final-hex-string parser fix, weighted below master !23724.
- **!23715 — Deep/corroboration, merged; authored and merged by John Thacker.** Explicit `(void)` casts document intentionally ignored checked returns when truncation/failure is irrelevant or impossible by construction. Strong evidence for existing analyzer-handling guidance; no duplicate rule added.
- **!23714 — Scanned, merged.** JSON-3GPP frees `GMatchInfo` and matched substrings correctly. Reinforces GLib ownership/lifetime rules.
- **!23713 — Scanned, merged.** Removes deprecated `tvb_find_line_end()` after migration to unsigned/status-aware replacements. API lifecycle cleanup.
- **!23712 — Scanned, merged stable backport.** Plot automatic x-axis range correction; GUI behavior fix, weighted below its source change.
- **!23711 — Scanned, merged.** O-RAN FH CUS Section Extension 30 support was explicitly described as untested because the specification was insufficiently clear to construct a reliable Scapy vector. Protocol-specific implementation context; no new testing rule inferred merely from acceptance.
- **!23710 — Scanned, merged.** TCP RST diagnostic payload updated from draft-14 to draft-15. Standards-tracking maintenance.
- **!23709 — Scanned, merged; authored by John Thacker.** ASN.1 include ordering ensures generated exports precede manual declarations that depend on them and adds the needed ASN.1 header for OER. Build/generated-header dependency maintenance.

## Durable notebook promotion

- `assertion-static-analysis-conventions.md`: compiler/analyzer attributes are optimizer-visible semantic contracts. In particular, do not use `G_GNUC_MALLOC` / `__attribute__((malloc))` on constructors whose returned objects retain or alias pre-existing objects such as a `wmem_allocator_t`; use such attributes only when the complete documented non-aliasing/allocation contract is true (!23728, corroborated by !23730 and !23731).

Merged master work was weighted most heavily. Stable backports were used primarily as corroboration. John Thacker's authored/merged compiler-contract and static-analysis work received elevated weight; Jaap Keuter's concrete accepted review on !23742/!23744 was treated as strong corroboration of conventions already present. Closed !23743/!23748 and open draft !23727 were explicitly down-weighted and were not allowed to override merged evidence.
