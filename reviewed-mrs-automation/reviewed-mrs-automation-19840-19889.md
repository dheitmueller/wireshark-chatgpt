# Wireshark MR review automation: !19840-!19889

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Direction: newest to oldest. Selection was made after consulting the existing per-run ledgers and `reviewed-mrs.md`; the historical !17571-!17620 batch and previously backfilled !22966 remain counted. The corpus revision is unchanged from the preceding run, so no newly populated higher-numbered hole displaced this batch.

Exactly 50 merge requests reviewed:

`!19889, !19888, !19887, !19886, !19885, !19884, !19883, !19882, !19881, !19880, !19879, !19878, !19877, !19876, !19875, !19874, !19873, !19872, !19871, !19870, !19869, !19868, !19867, !19866, !19865, !19864, !19863, !19862, !19861, !19860, !19859, !19858, !19857, !19856, !19855, !19854, !19853, !19852, !19851, !19850, !19849, !19848, !19847, !19846, !19845, !19844, !19843, !19842, !19841, !19840`

## Durable findings

- **!19841 (merged, deep):** Sysdig pcapng blocks were moved onto `register_pcapng_block_type_handler()` to exercise a plugin-style block registration API. Guy Harris explained that registration must prevent plugins from overriding block types already owned by core pcapng handling, while distinguishing officially assigned but not-yet-supported identifiers from arbitrary unregistered identifiers. Added collision-safe registration / namespace-policy guidance to `pcapng-extension-architecture-conventions.md`.
- **!19866 (merged, deep):** TCP out-of-bounds fix originated from OSS-Fuzz even though the author could not reproduce it with the local capture set. The final merged change was rebased and approved by John Thacker. Useful fuzzing evidence, but it corroborates existing malformed-input/fuzzer guidance rather than creating a new rule.
- **!19853 (open snapshot, discussion-focused):** proposed `BASE_DEFAULT_VALS` exposed the danger of stealing an in-band value as a sentinel when the underlying `value_string` domain permits every `uint32_t`. Jaap Keuter explicitly framed the design issue as in-band versus out-of-band metadata. Because the MR is open/unmerged in the corpus snapshot, this is retained as provisional design evidence and was not promoted over merged conventions.
- **!19868 (merged):** Wi-Fi 7 KDE malformed-packet fix based on a real capture; corroborates capture-driven parser validation.
- **!19874 (merged):** follow-up to older PPP review comments, switching to common OUI registration/retrieval conventions; corroborates reuse of shared protocol infrastructure.
- **!19879 (merged):** Wiretap helper for initializing packet records; consistent with centralizing invariant initialization rather than duplicating field setup.
- **!19882 (merged):** SMB packet-list/header presentation improvement; no additional durable architecture rule identified.
- **!19888 (merged):** DNS local-part/domain escape handling bug fix; parser-specific correction, no new general rule.

The remaining MRs were scanned for state, purpose, diffs/discussion signal, and overlap with existing notebook guidance. Automatic data updates, licensing/SPDX changes, build/CI maintenance, spelling/cosmetic work, backports/duplicate attempts, and narrow protocol updates did not justify manufacturing additional conventions. Closed !19871/!19872 and open !19853 were weighted below merged master changes.
