# Automated Wireshark MR review: !19009-!19058

- Corpus: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`
- Review direction: newest to older
- Exact MRs selected: 50
- Selection method: consulted accumulated review tracking under `reviewed-mrs-automation/` and `reviewed-mrs.md`, preserving/counting the historical !17571-!17620 batch; selected the fifty highest-numbered corpus MRs not in the reconstructed reviewed set. The previous contiguous frontier ended at !19059, so this batch is !19058 downward through !19009.

## Exact reviewed set

!19058, !19057, !19056, !19055, !19054, !19053, !19052, !19051, !19050, !19049,
!19048, !19047, !19046, !19045, !19044, !19043, !19042, !19041, !19040, !19039,
!19038, !19037, !19036, !19035, !19034, !19033, !19032, !19031, !19030, !19029,
!19028, !19027, !19026, !19025, !19024, !19023, !19022, !19021, !19020, !19019,
!19018, !19017, !19016, !19015, !19014, !19013, !19012, !19011, !19010, !19009.

## Durable findings / weighting notes

- !19011 (merged, John Thacker; approved/merged by Alexis La Goutte) is the strongest new durable lesson. HiPerConTracer's timestamp-based UDP/TCP heuristic had accepted only about 0.01437% of the 64-bit value space with microsecond timestamps; adding nanosecond timestamps expanded the accepted range to about 14.37%. The accepted response was to disable the now-weak heuristic by default while leaving it available. Added guidance to `heuristic-dissector-conventions.md`: protocol changes that expand the valid value domain must trigger a re-evaluation of heuristic selectivity and false-positive risk; quantify that risk where practical.
- !19050 (merged, Jaap Keuter) continues the project-wide migration from generic `tvb_get_bits()` toward width-specific accessors and explicit endian flags. Martin Mathieson explicitly favored the most appropriate sized accessor and noted that at-most-16-bit O-RAN values should be read into `uint16_t` rather than wider storage. This strongly corroborates existing API-domain/type-width guidance already captured from later MRs.
- !19034 (merged) fixes UDP stream/conversation identity when a deinterlacing key and ephemeral server ports are involved, with visible SNMP/TFTP side effects. It reinforces the existing state/conversation rule that identity keys must model the actual protocol/conversation semantics rather than a convenient subset of endpoint attributes.
- !19055 (merged) adds captured-length checking before dissecting optional IEEE 802.15.4 beacon sections, corroborating existing malformed/truncation defensive parsing guidance.
- !19054 and !19043 are WebSocket decompression-flag fixes around fragmented messages; they reinforce that per-message state cannot be inferred from conversation-global or first-conversation-fragment assumptions.
- !19042 (merged) factors PKCS #8 structures out of PKCS #12 into a dedicated dissector, useful protocol-ownership/reuse evidence but not a new general rule.
- !19027 updates the developer guide away from deprecated dissector APIs, corroborating that documentation examples should model current preferred APIs rather than merely compiling legacy forms.
- !19019 makes the CMake 3.20 requirement explicit after adopting `cmake_path`, reinforcing build-system dependency/version declarations.
- !19048 was closed while !19049 is the merged EEA2 deciphering work; the merged successor was weighted more heavily. !19038 is draft/unmerged and was treated as provisional. Automatic update MRs !19039-!19041 carry little reusable engineering-review evidence.

The remainder of the batch consists primarily of merged protocol correctness, backports, generated/automatic updates, UI/tap plumbing, build fixes, and closely related predecessor/successor variants. They were reviewed for outcome and reusable evidence but did not justify duplicating notebook conventions already present.

## Notebook update

`heuristic-dissector-conventions.md` was updated in commit `c20cef34c5648ffd43f07106aa011d245eb6686f` with the !19011 selectivity-regression lesson.

## Continuation

Future descending review must rebuild the reviewed set again rather than assuming numeric ranges are complete. Subject to discovery of higher-numbered holes in accumulated tracking, the next contiguous frontier below this batch is !19008.
