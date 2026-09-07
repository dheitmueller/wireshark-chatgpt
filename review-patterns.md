# Upstream Merge Request Review Patterns

This file records reusable lessons learned from Wireshark GitLab merge-request reviews.

## Evidence standard

For each pattern, record:

- MR number/link or other provenance.
- Reviewer/maintainer feedback in paraphrased form.
- The resulting code change when known.
- Whether the lesson appears to be a one-off preference or a reusable convention.
- Confidence level based on corroboration from other MRs/current source.

Do not normally turn a single review comment into a universal rule without checking current source and, where practical, other reviews.

## Reviewer authority

### Guy Harris

Treat technical review feedback from Guy Harris as extremely authoritative for Wireshark architecture, packet-dissection semantics, capture-file behavior, protocol interpretation, portability, and long-standing project conventions. He is a key Wireshark maintainer with more than two decades of project experience. A clear technical correction from him should carry substantially more evidentiary weight than an ordinary one-off review comment.

This does not mean mechanically generalizing every context-specific comment into a universal rule. Preserve the context and rationale, then corroborate against current source where the scope is unclear. However, when his feedback identifies an architectural or API convention, assume it reflects intentional project practice unless current upstream code demonstrates otherwise.

## Patterns

### New protocol functionality should have a sample capture

**Evidence:** MR !22662 (PTP/NTP: Add support for NTP over PTP). Michael Mann explicitly asked for a sample capture demonstrating the new functionality before the MR was approved.

**Lesson:** When adding or materially extending a dissector, expect reviewers to want a representative capture that exercises the feature. This complements automated dissector tests and makes manual validation/review possible.

**Confidence:** Medium. Strong direct maintainer evidence, and consistent with Wireshark contribution guidance, but continue collecting examples.

### Keep unrelated cleanup out of a focused MR

**Evidence:** MR !22662. While extending `dissect_ptp_v2_tlvs()`, the author noticed existing calls that appeared to swap `ptp_tree` and `ti_root`, but explicitly left that issue out of the MR. The MR proceeded independently.

**Lesson:** Preserve scope discipline. Discovery of adjacent existing problems during implementation is not by itself a reason to fold those fixes into the same change.

**Confidence:** Medium-high; also consistent with Wireshark's documented contribution guidance to avoid unrelated changes in one MR.

### Use a feature branch, not the fork's master branch, for merge requests

**Evidence:** MR !22208 (LLRP: Add support for ImpinjRFDopplerFrequency parameter). Alexis La Goutte requested that the contributor create a named branch rather than submit from the fork's `master`; the contributor replaced the MR with !22212.

**Lesson:** Wireshark's expected GitLab workflow is a named topic branch in the contributor's fork.

**Confidence:** High; direct reviewer feedback and documented project workflow.

### Separate prerequisite infrastructure from later semantic decoding when appropriate

**Evidence:** MR !20793 (gsm_sim: Add GET RESPONSE and APDU reassembly). John Thacker noted that the change did not yet dissect the reassembled content but was a necessary prerequisite for doing so later; the MR was approved and merged.

**Lesson:** A self-contained infrastructure/reassembly improvement can be an acceptable MR even if higher-level content decoding is deliberately deferred, provided the intermediate change is useful and architecturally necessary.

**Confidence:** Medium. Context-specific but useful evidence for incremental dissector development.

### Avoid tree-presence guards when they suppress non-UI dissection side effects

**Evidence:** MR !22662 discussion. The author identified `if (tree)` fences in `packet-ptp.c` as preventing TLV dissection on the first visit, which in turn broke statistics and request/response tracking in the nested NTP dissector. The issue was addressed separately in !22663.

**Lesson:** Dissection required for state, statistics, request/response tracking, or other non-display behavior must not depend solely on whether a protocol tree is being built. Be suspicious of `if (tree)` fences around logic that does more than add display items.

**Confidence:** Medium-high. Direct behavioral evidence; corroborate against current dissector guidance and additional reviews.

## Review-access note

GitLab search/index access is currently uneven for very recent MRs. The MR list may expose an MR before its overview, diff, or discussions are retrievable through the available read-only web path. Do not claim an MR has been fully reviewed unless its actual discussion/diff was available. Track inaccessible recent MRs and revisit after indexing catches up.
