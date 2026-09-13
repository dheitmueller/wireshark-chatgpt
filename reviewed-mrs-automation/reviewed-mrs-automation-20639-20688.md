# Wireshark MR review automation ledger: !20639-!20688

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Selection method: rebuilt the already-reviewed set from all available per-run files in `reviewed-mrs-automation/` plus `reviewed-mrs.md`, preserving the separately reviewed !17571-!17620 batch. Selected the fifty highest-numbered corpus MRs not already present in that tracking. All IDs !20688 through !20639 exist at the pinned corpus commit, so this run contains exactly that contiguous set and no gap extension.

Review weighting: merged master work and explicit senior-maintainer review were weighted most heavily; closed, superseded, intermediate, or questionable changes were retained in the audit set but not promoted as accepted conventions unless later accepted work corroborated the lesson.

## Exact reviewed MR set

- !20688 — deep: epan/dissector data-boundary refactor; promoted.
- !20687 — scanned: merged PFCP change with representative capture evidence.
- !20686 — scanned: merged reserved-value expert validation.
- !20685 — scanned: merged PFCP change with representative capture evidence.
- !20684 — deep: accepted XML/libxml2 successor restoring established registration/code flow; promoted with !20665/!20674 context.
- !20683 — scanned: merged regex change, but not used as positive precedent because its static-pointer lifetime pattern is not a durable exemplar.
- !20682 — scanned: merged Qt presentation fix.
- !20681 — scanned: merged PFCP additions with per-IE sample captures.
- !20680 — scanned: merged sentinel/index validation fix.
- !20679 — scanned: merged shared UTF-8 constant cleanup.
- !20678 — scanned: merged translation successor to !20669.
- !20677 — scanned: release backport of !20674.
- !20676 — scanned: merged stale-comment cleanup.
- !20675 — scanned: merged checker exception for a legitimate protocol relationship.
- !20674 — deep: explicit completion of deferred protocol-prefix registration before generic enumeration; promoted with XML registration lifecycle guidance.
- !20673 — scanned: automated update.
- !20672 — scanned: automated update.
- !20671 — scanned: automated update.
- !20670 — scanned: merged fractional-second range constant cleanup.
- !20669 — lower weight: closed/superseded translation MR; submission/rebase discussion retained only as supporting review-process evidence.
- !20668 — scanned: stable backport of SSH null validation.
- !20667 — scanned: merged revert of unrelated whitespace churn.
- !20666 — scanned: merged SSH null validation.
- !20665 — deep but intermediate: XML deferred-registration/performance refactor; interpreted through later accepted !20684 correction rather than promoted verbatim.
- !20664 — scanned: merged GENEVE label/encoding cleanup.
- !20663 — scanned: merged URL/comment cleanup.
- !20662 — scanned: merged XML default-media-map preload optimization.
- !20661 — scanned: merged DTD standards-compliance fix and libxml2 discussion supporting the XML migration context.
- !20660 — scanned: merged application-flavor UI check; not promoted because later notebook evidence establishes a stronger frontend-specialization architecture.
- !20659 — scanned: merged references update.
- !20658 — scanned: merged XML field-abbreviation collision fix.
- !20657 — deep: callers use dedicated wmem map destruction rather than freeing only the container; promoted.
- !20656 — deep: introduces and documents `wmem_map_destroy()` while warning that explicit destruction can indicate a lifetime-design problem; promoted.
- !20655 — lower weight: closed/unmerged Qt auto-resize change.
- !20654 — scanned: merged nettrace transport fallback.
- !20653 — scanned: merged nettrace proxy fallback handling.
- !20652 — scanned: merged NAS-5GS dissector extension.
- !20651 — deep: documents that wmem maps are allocator-lifetime containers and NULL/manual ownership should generally use `GHashTable`; promoted.
- !20650 — deep: removes direct `wmem_free()` of an RTP wmem map and relies on allocator/container lifecycle; promoted.
- !20649 — scanned: merged CI portability fix using `CI_API_V4_URL` instead of a hard-coded GitLab endpoint.
- !20648 — scanned: merged Roon discovery mapping update.
- !20647 — deep corroboration: map-owned non-wmem auxiliary state is released by allocator callback; supports wmem lifetime guidance.
- !20646 — deep, very high authority: Guy Harris-authored/merged graceful degradation when exact picosecond arithmetic is unavailable; promoted.
- !20645 — deep corroboration, very high authority: Guy Harris-authored/merged C portability fix for unused parameters on compilers without the 128-bit path.
- !20644 — scanned: merged PFCP additions with concrete per-IE captures/timestamps supplied for testing.
- !20643 — scanned: merged Json2Pcap compatibility update.
- !20642 — scanned: merged Plot markers feature and review discussion.
- !20641 — scanned: merged Windows portability/build fix.
- !20640 — scanned: merged BLF robustness work requested by senior maintainers.
- !20639 — scanned: merged duplicate preference-module registration invariant hardening.

## Notebook changes promoted from this run

- `epan-layering-conventions.md`: non-dissector consumers should obtain protocol metadata through epan-owned interfaces rather than link directly against dissector implementation tables (!20688).
- `initialization-lifecycle-conventions.md`: deferred/lazy registration must preserve the externally observable registration contract; force completion before consumers that require a complete registry, and interpret !20665 through accepted successor !20684 plus !20674.
- `wmem-container-lifetime-conventions.md`: wmem containers normally follow allocator lifetime; do not directly `wmem_free()` a map, use dedicated destruction only when genuinely necessary, and prefer manually managed containers for NULL/manual lifetime (!20650/!20651/!20656/!20657, corroborated by !20647).
- `dissector-resilience-conventions.md`: if a supported build cannot compute an exact derived protocol value, omit the bogus value and report the capability limitation with Expert Info rather than fabricating precision (!20646, Guy Harris).

No update to `reviewed-mrs.md` was required for correctness; this exact per-run ledger is authoritative for this batch.