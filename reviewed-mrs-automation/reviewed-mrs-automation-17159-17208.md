# Wireshark MR automation review: !17159-!17208

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Direction: descending from newest toward older MRs. This batch was selected only after consulting `reviewed-mrs.md` and all available ledgers in `reviewed-mrs-automation/`. The previously reviewed !17571-!17620 batch remains counted. No numeric-range inference was used for selection.

## Exact reviewed set (50)

!17208, !17207, !17206, !17205, !17204, !17203, !17202, !17201, !17200, !17199,
!17198, !17197, !17196, !17195, !17194, !17193, !17192, !17191, !17190, !17189,
!17188, !17187, !17186, !17185, !17184, !17183, !17182, !17181, !17180, !17179,
!17178, !17177, !17176, !17175, !17174, !17173, !17172, !17171, !17170, !17169,
!17168, !17167, !17166, !17165, !17164, !17163, !17162, !17161, !17160, !17159.

## Weighting and durable findings

Merged master changes were weighted most heavily; release backports, automatic updates, documentation-only changes, abandoned/superseded proposals, and changes without substantive review discussion were weighted less.

- !17208 (merged master): Coverity-driven Qt fix initializes `current_rtp_sai_selected_` explicitly to `nullptr`. Straightforward defensive initialization; no new convention beyond existing initialization/static-analysis guidance.
- !17200 (merged master, John Thacker): changes only the human-facing protocol-tree rendering of `FT_RELATIVE_TIME` to use readable units, deliberately preserving machine-oriented/filter/JSON/PDML and other API representations. Durable design evidence: presentation improvements should not casually alter serialization, filtering, or API semantics.
- !17190 (merged release-4.2): automatic registry/manufacturer data update. Low independent engineering weight.
- !17180 (merged master, John Thacker): improves human-readable relative-time formatting to use ms/us/ns for sub-second values. Together with !17200, this reinforces keeping display formatting concerns separate from stable machine-facing representations.
- !17170 (merged master): BLF date/time validation before `mktime()`. Guy Harris's review explicitly asked whether the malformed timestamp came from the producer or later corruption and compared behavior with CANoe before accepting recovery behavior. High-authority evidence for understanding provenance and reference-implementation behavior when deciding how wiretap readers should recover from damaged-but-partially-readable files.
- !17160 (merged release-4.2): TCP ACKed-unseen-segment regression backport. Weighted below the corresponding master fix.
- !17159 (merged master): WSUG spelling consistency only; no coding/architecture lesson.

## Notebook action

No separate convention file was changed. The useful lessons in this batch substantially reinforce existing guidance: separate human-facing display formatting from stable machine/API representations; validate external metadata before passing it to system APIs; and, for recoverable file corruption, investigate producer/reference-tool behavior before choosing fallback semantics.

## Continuation

Rebuild the already-reviewed set from all ledgers before selecting the next batch. If no newly added higher-numbered unreviewed MRs exist, the next descending candidate after this run is !17158.
