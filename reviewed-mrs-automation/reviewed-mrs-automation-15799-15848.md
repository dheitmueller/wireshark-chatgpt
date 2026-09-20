# Wireshark MR review automation ledger — !15799–!15848

Corpus commit: `130eea3c8a60902c0fc98c9e1130e73bafc6f815`

Notebook base before this run: `698be88e43251072c75c8aac0a1b170cbe09807c`

## Selection

The already-reviewed set was rebuilt from the available review tracking in `reviewed-mrs.md` and all per-run files under `reviewed-mrs-automation/`, including the historical !17571–!17620 batch. Selection was done by exact MR-number set subtraction, not by assuming numeric intervals were complete. The immediately preceding exact ledger covers !15898–!15849; the other available ledgers and aggregate tracking do not mark any MR in !15848–!15799 as already reviewed.

The fifty highest-numbered previously unreviewed MRs present in the corpus are therefore exactly:

`!15848, !15847, !15846, !15845, !15844, !15843, !15842, !15841, !15840, !15839, !15838, !15837, !15836, !15835, !15834, !15833, !15832, !15831, !15830, !15829, !15828, !15827, !15826, !15825, !15824, !15823, !15822, !15821, !15820, !15819, !15818, !15817, !15816, !15815, !15814, !15813, !15812, !15811, !15810, !15809, !15808, !15807, !15806, !15805, !15804, !15803, !15802, !15801, !15800, !15799`.

`mr_15798.json` is present at the same corpus commit, so the available corpus is not exhausted.

## Review weighting and durable findings

All fifty MRs above were inspected for state, description, discussions, and substantive diffs where they carried reusable review signal. Forty-eight are merged, !15839 remains an open draft, and !15822 was closed without merge. Merged master changes and direct maintainer guidance received the most weight; release-branch cherry-picks were used primarily as corroboration; the open draft and closed backport were down-weighted as implementation exemplars.

Promoted durable conventions:

- **!15836, !15840, !15841 — explicit empty values in sparse preference persistence.** The master fix, authored by John Thacker and approved/merged by Anders Broman, pre-seeds preference-map entries for settings whose serializer otherwise records only non-default interfaces. Without an explicit empty entry, clearing the last comment or monitor-mode override leaves the old persisted value untouched. The same correction was accepted on both maintained release branches. Added `preference-persistence-conventions.md`: sparse serialization must distinguish "no update" from "set to empty/default," and review should test the transition from some customized elements to none.
- **!15835 — compound UI state belongs in one stack entry.** John Thacker's merged `LabelStack` fix stores the tooltip together with text/context, so pushing a temporary status and later popping it restores the complete underlying presentation rather than only the text. Added `ui-state-stack-conventions.md`.
- **!15813 — keep deprecated aliases for renamed script-visible identifiers.** John Thacker's merged Telephony group rename deliberately retains the older Lua names because scripts are reused across Wireshark versions without recompilation. Added `scripting-api-compatibility-conventions.md`.
- **!15806 — local analyzer suppressions require a proved semantic bound.** RTPS secure-message dissection intentionally re-enters the ordinary submessage parser after decryption, triggering clang-tidy's recursion check. The contributor documented and guarded the protocol invariant that PSK encryption cannot nest at the submessage level; Gerald Combs then requested the narrow `NOLINTNEXTLINE(misc-no-recursion)` suppression at the affected function. Added this rule to `checker-tooling-conventions.md`.
- **!15824 / !15825 — optional-feature configurations need runtime tests, not only successful compilation.** The HTTP/2 sharkd test now skips when HTTP/2 support is absent. John Thacker explicitly noted that the existing "no options" CI configuration built successfully but did not run tests, identifying the coverage gap. Added this rule to `ci-tooling-conventions.md`.

Strong corroborating evidence not promoted as duplicate rules:

- **!15829 — AX.25 display-filter semantics.** The accepted field type becomes string-like and normal filtering operates on the normalized `CALLSIGN-SSID` semantic value, while the `@` operator remains available for raw-byte filtering. This directly corroborates `protocol-field-semantic-conventions.md`: user-facing filters should model normalized protocol concepts while raw encoding remains separately accessible when needed.
- **!15822 — no new functionality on stable branches.** Anders Broman explicitly rejected the release-4.2 sharkd backport with "We don't backport new functionality." The master feature !15817 remained valid. This strongly corroborates the existing `stable-branch-submission-conventions.md`; the closed backport is not used as primary implementation evidence.
- **!15815 — zlib-ng integration.** Review by John Thacker and others identified that distributions package zlib-ng in different compatibility modes. The merged design uses the zlib-ng-specific API rather than assuming compatibility-mode packaging, allowing zlib and zlib-ng consumers to coexist. This corroborates existing dependency/API-boundary guidance and the later zlib/zlib-ng capability work already recorded from subsequent MRs.
- **!15844 — MiWi dissector import.** Automated commit checks caught unused field/expert registrations and filter abbreviations that did not match the protocol abbreviation. The contributor fixed those errors before merge, corroborating the existing rule that deterministic structural conventions belong in pre-submit tooling.
- **!15816 — Protobuf array bounds.** A maintained-branch fix checks the fixed array limit before writing, reinforcing existing bounds/resource-safety guidance.
- **!15826 / !15827 / !15828 — cleanup on error paths.** The same CaptureFileDialog preview leak fix was merged on master and both maintained branches: `wtap_close()` is required even when preview-stat collection exits on an error. This corroborates existing resource-lifetime guidance.
- **!15804 — UI/backend numeric domains.** The accepted capture-options change derives GUI limits from the actual backend representation, enforces positive minimums and unit-dependent maximums in the controls, and fixes SI/IEC labeling. Useful UI validation evidence, but not sufficiently Wireshark-specific to justify another notebook rule.
- **!15843 — standards-driven default change.** VRRPv3 checksum behavior changes its preference default after RFC 9568 clarifies the IPv4 rule, while retaining configurability for compatibility. This is good protocol-maintenance evidence but does not add a distinct convention beyond existing preference/standards guidance.
- **!15839 — Matter conversation tracking draft.** Kept in the reviewed set but down-weighted because it remains an open draft and was not used as accepted architectural precedent.

The remaining MRs were protocol-local corrections/features, C99 type migration, generated data refreshes, spelling/documentation cleanups, straightforward UI fixes, or stable-branch cherry-picks whose durable lessons are already represented in the notebook.

## Notebook commits from this run

- `5185885da84d20aa3b40dfed90c387c2a12adc57` — sparse preference persistence must represent an explicit clear/default transition.
- `c65e01820304ec05d7483b392d4ce821555e2bf8` — stacked UI state must restore coupled presentation attributes atomically.
- `51823c8c1d875c81c17f438c5c309b380d8c63ab` — preserve deprecated aliases for renamed script-visible API identifiers.
- `a3f86abc293730d34eae9570ecc6690bbe8a0cf8` — justify and localize static-analysis suppressions after proving the semantic invariant.
- `85ba115295b2ebff0242f3c2ece4c033b229ea35` — run tests for optional-feature configurations rather than treating a build-only job as sufficient coverage.
