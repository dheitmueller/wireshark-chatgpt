# Automated Wireshark MR review: !14749 through !14798

- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit reviewed: `1c7ad509887ee25079a7865cc62ba18cba06f49a`
- Review direction: newest available previously-unreviewed MRs toward older MRs
- Exact MR count: 50
- Status composition: 50 merged, 0 closed/unmerged

## Selection / duplicate-avoidance

Before selecting this batch, the reviewed set was reconciled against the current `reviewed-mrs.md`, `reviewed-mrs-automation/reviewed-mrs-automation.md`, the complete repository inventory of per-run files under `reviewed-mrs-automation/`, and the immediately preceding exact ledger `reviewed-mrs-automation-14799-14848.md`. The historical !17571-!17620 batch remains part of the reviewed set. Selection was performed by excluding already-recorded MR numbers rather than by assuming that a numeric interval was fully reviewed. At this corpus/notebook state, the 50 highest-numbered remaining MRs are exactly !14798 down through !14749.

## Exact MRs reviewed

!14798, !14797, !14796, !14795, !14794, !14793, !14792, !14791, !14790, !14789,
!14788, !14787, !14786, !14785, !14784, !14783, !14782, !14781, !14780, !14779,
!14778, !14777, !14776, !14775, !14774, !14773, !14772, !14771, !14770, !14769,
!14768, !14767, !14766, !14765, !14764, !14763, !14762, !14761, !14760, !14759,
!14758, !14757, !14756, !14755, !14754, !14753, !14752, !14751, !14750, !14749.

## Durable findings promoted to the notebook

### Dynamic configuration-derived fields must refresh through the module lifecycle (!14779, !14793)

Merged master !14779 replaces direct SOME/IP dynamic-HF rebuilds from UAT callbacks with preference-change signaling on the owning module, explicitly setting `PREF_EFFECT_DISSECTION | PREF_EFFECT_FIELDS` so normal handoff occurs even when SOME/IP is not currently bound to a port. Follow-up merged master !14793 moves the signal into the relevant reset callbacks as well, covering profile/reset paths that otherwise could leave field registrations stale. Durable rule: configuration mutations should invalidate through the lifecycle that owns the derived registrations, at callbacks guaranteed to cover edits and resets, rather than relying on unrelated binding state or one ad-hoc update path. Added to `state-refresh-conventions.md`.

### Transaction keys do not by themselves identify a frame's request/response role (!14784, !14792 and related stable backports)

Merged master !14784, authored by Jaap Keuter and committed by John Thacker, fixes SNMP request/response decoration when requests repeat with the same request ID. The accepted code determines whether the current frame is the response by comparing it with the recorded `response_frame_id`; other matching frames remain request frames and point to `response_in`. Stable backports including !14792 reinforce the fix but carry less weight than the master change. Durable rule: once several frames can share a transaction key, use authoritative stored frame identities/direction to determine each frame's semantic role. Added to `transaction-matching-conventions.md`.

### Parser-reachable display-filter expressions must fail normally, not through internal assertions (!14789)

Merged master !14789, authored and merged by John Thacker, fixes a crash for arithmetic between string literals. The parser can legitimately create `STTYPE_STRING` operands even though their use with arithmetic is semantically invalid. The accepted implementation routes them through ordinary semantic checking and reports a user-facing type error rather than a fatal DFilter assertion. Durable rule: semantic dispatch must cover every AST form the parser admits at that position; syntactically valid but semantically invalid user input belongs on the normal diagnostic path. Added to `dfilter-conventions.md`.

### Repository dissector checkers must treat plugin source paths as legitimate inputs (!14783)

Merged master !14783 changes `check_dissector.py` and helper checkers to honor an existing supplied source path before falling back to `epan/dissectors/`, and adapts build-object lookup for plugin sources. The author explicitly characterized this as an incremental step rather than proof of universal plugin-checker support. Durable rule: checker path resolution should preserve real repository-relative input paths and use core-dissector assumptions only as fallback behavior; plugin dissectors should be exercised as first-class checker inputs where supported. Added to `checker-target-conventions.md`.

## Important corroborating / historical evidence

- !14797, !14796, !14795, and master !14790 queue TCP stream-graph replots during high-frequency mouse motion. They corroborate the existing Qt event-coalescing guidance; later reviewed !14854/!14817 provide broader/stronger precedent, so no duplicate rule was added.
- !14785, authored and merged by John Thacker, fixes code that assumed a non-NULL `hfinfo->strings` always denotes a value-string table; `FT_PROTOCOL` and other field types can overload that member. Guy Harris added a post-merge architectural suggestion to centralize the type-plus-pointer test in a helper/macro. Because that suggestion was not itself implemented in the accepted MR, it is retained as high-authority review evidence rather than recorded as an adopted project rule.
- !14794 makes an explicit Diameter vendor-dictionary `Address` type override a weaker numeric-code heuristic. This supports the general preference for authoritative schema metadata over inference, but the evidence is narrow enough not to justify another notebook rule.
- !14782/!14781/!14778 and !14777/!14776 are display-filter language fixes/features. They were checked for durable compiler conventions; the stronger new generalizable finding in this batch is the crash-to-diagnostic rule from !14789.
- !14771 is a merged QUIC multipath draft-07 implementation and therefore valid historical evidence, but later reviewed !14844 specifically repairs compatibility with the older draft mode after this work. For durable guidance the later negotiated-mode/session-state fix is therefore weighted more heavily than !14771's intermediate implementation.
- The recursion-hardening group !14765 through !14754 substantially corroborates existing bounded-recursion/depth-bookkeeping notebook guidance and was not duplicated.
- !14757's frame-number field cleanup corroborates existing field-semantic guidance.
- !14753 changes CIGI heuristics to length-check before reading so short candidate UDP packets do not throw exceptions. It corroborates the existing heuristic-dissector rule that a rejected candidate must fail cleanly rather than by exception.
- !14798, !14788, !14787, !14780, !14775, !14774, !14773, !14772, !14770, !14769, !14768, !14767, !14766, !14752, !14751, !14750, and !14749 were reviewed as merged maintenance, UI/build, protocol, generated-data, cleanup, or documentation work; no additional durable convention stronger than existing notebook guidance was extracted.
- !14786 adds/checks tooling for possible `FT_FRAMENUM` candidates but leaves the detection path without actionable current findings; retained as tooling-history evidence rather than a project-wide convention.

## Weighting notes

All 50 MRs in this run are merged, so none required abandoned/superseded down-weighting on status alone. Master changes were weighted more strongly than their stable cherry-picks. Later accepted fixes were used to qualify intermediate historical implementations when applicable (notably !14771 versus later !14844). Guy Harris's !14785 comment was given high architectural-review weight while preserving the distinction between an authoritative suggestion and an implemented/accepted convention.

## Notebook updates made by this run

- `state-refresh-conventions.md`: configuration-derived registration invalidation/lifecycle rule from !14779/!14793.
- `transaction-matching-conventions.md`: explicit frame-role identity rule from !14784 and stable propagation.
- `dfilter-conventions.md`: parser-admitted expressions must reach ordinary semantic diagnostics from !14789.
- `checker-target-conventions.md`: plugin-aware checker path resolution from !14783.

## Continuity

At corpus commit `1c7ad509887ee25079a7865cc62ba18cba06f49a`, `mr_14748.json` exists and is merged, so the corpus is not exhausted. Absent newly scraped higher-numbered unreviewed material, !14748 is the next descending candidate.
