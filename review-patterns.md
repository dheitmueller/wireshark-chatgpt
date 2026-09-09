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

## MR preparation checklist

The reviewed corpus has not yet shown a canonical checkbox-style Wireshark submission template. However, several successful MRs explicitly document the validation and review material they supplied, and reviewer feedback repeatedly asks for missing items such as captures. Treat those recurring practices as a practical pre-submission checklist rather than waiting for CI or reviewers to discover omissions.

Before submitting one of our Wireshark MRs, check and, where useful, state in the MR description:

- The change is on a named topic branch, not the fork's `master` branch.
- Commits are clean and focused; squash fixup/development-history commits when the MR represents one logical change.
- Commit messages follow Wireshark's checked format: a short `component: brief summary` subject, then a blank line before any longer description. Merged MR !25803 was held by the pipeline/reviewer until a missing blank line after the subject was fixed; treat commit-message checks as part of submission readiness, not cosmetic cleanup.
- Unrelated cleanup or prerequisite work is split into a separate MR where appropriate.
- The code builds cleanly with warnings treated as errors.
- Relevant Wireshark validation/static-analysis scripts have been run. For dissector work, merged MR !26218 explicitly reported clean `tools/check_dissector.py` and `tools/fuzz-test.sh` runs; use the current tree to determine the applicable commands rather than blindly copying an old command list. Merged !25766 also shows `check_dissector.py --commits <N>` being used to focus warning checks on recent changes.
- Representative capture file(s) are supplied for new or materially changed protocol dissection. Prefer small focused captures covering distinct behaviors and edge cases.
- Automated dissector tests accompany the captures when practical, including expert-info/error behavior and malformed/truncated cases relevant to the change.
- New dissectors use existing registration/dispatch mechanisms and include any expected integration updates such as build-system entries and release notes.
- Stateful/reassembly changes are tested across meaningful sequences, not only individual happy-path packets; use differential validation against a mature reference implementation when one exists.
- The MR description says what was tested and calls out deliberate limitations, deferred functionality, compatibility/filter changes, or cases that remain intentionally opaque.
- The submitted head has a passing pipeline before merge readiness is assumed.

**Evidence:** !26218 is a particularly useful exemplar: it included a five-packet capture, four dissector tests, malformed/expert-info cases, build/release-note integration, and explicitly reported clean `check_dissector.py` and `fuzz-test.sh` runs. !26211 added five captures and six focused tests and used differential validation against libudx. !25977 is another merged exemplar whose MR description contains an explicit `Testing` section: it names the included pcap, states the exact tshark behavior verified for both changed and unaffected packets, and reports a clean warning-free build. !25803 adds direct evidence that commit-message formatting is CI-enforced/reviewer-visible. !22662, !26366, and !26390 independently establish that reviewers expect sample captures. !22208 establishes the topic-branch expectation, and !26390 supplies direct review evidence for cleaning/squashing a focused commit series.

**Confidence:** High for the checklist as our submission practice. It is synthesized from repeated accepted/reviewer-requested behavior, not claimed to be an official Wireshark checklist.

## Patterns

### Use existing Wireshark helpers and already-fetched values instead of duplicating work

**Evidence:** Merged MR !25946 (NMEA0183 message consolidation). Pascal Quantin pointed out that the code could reuse the already fetched `sentence_id` rather than fetch it again, and specifically pointed to existing helpers in `epan/strutil.h` (`convert_string_case()` and `convert_string_to_hex()`) instead of reinventing conversion logic.

**Lesson:** Before adding a local parser/conversion helper or re-reading bytes that were already parsed, search current Wireshark utility APIs and retain/reuse the value already obtained. This complements the `_ret_*` tree APIs: the general preference is one authoritative fetch/conversion followed by reuse.

**Confidence:** High as an implementation habit; direct review feedback on a merged MR and consistent with other anti-double-fetch guidance.

### Reassembly tables must be initialized/registered, and this is suitable for automated checking

**Evidence:** Merged MR !25984 fixed a Bluetooth BR/EDR reassembly table that had been declared/used without the required initialization/registration. During review, Stig Bjørlykke asked whether this class of bug could be caught by a commit check; Martin Mathieson explored detecting reassembly tables that were not registered/initialized.

**Lesson:** When adding or modifying code that uses a `reassembly_table`, explicitly verify its initialization/registration path as part of review. Treat declaration + use without registration as a structural bug worth catching before runtime.

**Confidence:** High for the registration requirement (merged bug fix); medium-high for making it a pre-submit/static check, because maintainers explicitly discussed doing so.

### Use separate dissector entry points for different call contracts, with common parsing underneath

**Evidence:** MR !26224 attempted to make one IEEE 802.15.4 dissector distinguish whether its `void *data` argument was an FCS-type integer or the pseudo-header supplied by a top-level `wtap_encap` call by checking pointer identity. Guy Harris rejected that design: the cases should use different dissectors calling common code. He then implemented that design in !26229, which was merged; !26224 was closed as superseded.

**Lesson:** When the same protocol parser is entered through call paths with materially different `data` contracts or semantics, do not make one entry point guess the argument type from runtime pointer values. Give each call contract an explicit dissector entry point and funnel both into shared parsing logic with typed/explicit parameters.

**Confidence:** Very high. Direct architectural correction from Guy Harris, followed by a merged successor authored by Guy implementing the stated design.

### Prefix internal hf/ett symbols with the protocol name

**Evidence:** MR !26390 (ST 2110-40 and related VANC dissectors). Anders Broman explicitly requested that header-field symbols follow `hf_<protocol>_<name>` throughout, giving `hf_sdp_identifier` -> `hf_op47_sdp_identifier` as the example. A second inline comment requested `ett_op47_wst` instead of the generic `ett_wst`.

**Lesson:** Internal `hf_` and `ett_` identifiers should be protocol-qualified, even when file-local/static, to make ownership obvious and follow Wireshark naming conventions. Do not generate generic symbols such as `hf_length`, `hf_payload_type`, or `ett_data` in a dissector when a protocol prefix is available.

**Confidence:** High for new dissector code. Direct review feedback explicitly states this as the project naming pattern and asks that it be applied throughout.

### Prefer Wireshark tvbuff/proto bit APIs over custom bit extractors

**Evidence:** MR !26390. In `packet-op47.c`, a custom `get_bits_buf()` helper manually walked bits in a copied byte buffer. Anders Broman requested use of `tvb_get_bits()` or direct addition using `proto_add_bits_item` / `proto_add_bits_item_ret_uint` instead.

**Lesson:** Before writing local bit-extraction helpers, check the tvbuff and protocol-tree bit APIs. Prefer Wireshark's existing bit-access/add APIs when the data is available in a `tvbuff_t`; this reduces custom parsing code and better matches project idioms.

**Confidence:** High for the reviewed pattern; direct maintainer review with specific replacement APIs.

### Small related protocols may share one source file

**Evidence:** MR !26390. The change introduced several small ST/VANC dissector source files. Anders Broman suggested folding the ST dissectors into one file and explicitly noted that registering multiple protocols from one source file is acceptable/positive. Merged MR !25763 provides the complementary case: Alexis La Goutte questioned splitting NVMe-MI because Wireshark already has many dissector files, but the author justified it as preparation for several large command sets and to keep the framing layer thin; the split-by-type design ultimately merged.

**Lesson:** Do not assume one protocol registration requires one C source file. Consolidate small tightly related dissectors; split a protocol family when scale, cohesion, and expected growth genuinely justify independent modules. Source-file layout should be argued from maintainability, not applied mechanically.

**Confidence:** High for the contextual rule because we now have merged/reviewer evidence on both sides of the tradeoff.

### New protocol functionality should have a sample capture

**Evidence:** MR !22662 (PTP/NTP: Add support for NTP over PTP) received an explicit sample-capture request from Michael Mann. MR !26390 independently received the same request from Anders Broman. MR !26366 (BGP-MUP ADD-PATH) provides a third independent example: Alexis La Goutte asked for a pcap, the author supplied one, and the review thread was resolved.

**Lesson:** When adding or materially extending a dissector, expect reviewers to want representative capture file(s) that exercise the feature. This complements automated dissector tests and makes manual validation/review possible.

**Confidence:** High. Independently requested by three established reviewers on separate protocol/dissector MRs.

### Present review branches as clean, focused commits

**Evidence:** MR !26390 contained three commits for the initial implementation/tests. Anders Broman requested that all commits be squashed into one. Merged MR !25705 independently corroborates the practice: Jaap Keuter explicitly asked the HiSLIP 2.0 contributors to squash their development series, and they did so before merge.

**Lesson:** For a focused Wireshark MR, reviewers may expect fixup/development-history commits to be squashed into a clean logical commit before merge. Preserve meaningful independent commits only when they represent genuinely separable changes.

**Confidence:** High for focused single-change MRs. Two independent review examples, including one merged first-contribution protocol feature.

### Keep unrelated cleanup out of a focused MR

**Evidence:** MR !22662. While extending `dissect_ptp_v2_tlvs()`, the author noticed existing calls that appeared to swap `ptp_tree` and `ti_root`, but explicitly left that issue out of the MR. MR !26369 exposed an unrelated GCC warning while testing a Qt backport; the warning workaround was handled separately in !26371 instead of being folded into the Qt change.

**Lesson:** Preserve scope discipline. Discovery of adjacent existing problems during implementation or CI is not by itself a reason to fold those fixes into the same change.

**Confidence:** High. Repeated project practice plus documented contribution guidance.

### Keep merge requests small enough to review effectively

**Evidence:** Merged MR !25884 added a large set of NMEA0183 parametric messages. Anders Broman explicitly said the change was too large to review, and Pascal Quantin had to move review feedback out of the normal GitLab diff flow because the diff was too large for the UI. The contributor acknowledged that additional work still remained for future commits.

**Lesson:** Reviewability is a real engineering constraint, not just presentation polish. When a protocol expansion is large, split it into coherent, independently reviewable MRs or commits so reviewers can use the normal diff UI, reason about behavior, and give line-specific feedback. This strengthens the existing scope-discipline rule: even related work can be too large for one effective review unit.

**Confidence:** High for our submission practice. Direct feedback from two established maintainers on a merged MR, with a concrete review-tool limitation observed during the review.

### Use a feature branch, not the fork's master branch, for merge requests

**Evidence:** MR !22208 (LLRP: Add support for ImpinjRFDopplerFrequency parameter). Alexis La Goutte requested that the contributor create a named branch rather than submit from the fork's `master`; the contributor replaced the MR with !22212.

**Lesson:** Wireshark's expected GitLab workflow is a named topic branch in the contributor's fork.

**Confidence:** High; direct reviewer feedback and documented project workflow.

### Separate prerequisite infrastructure from later semantic decoding when appropriate

**Evidence:** MR !20793 (gsm_sim: Add GET RESPONSE and APDU reassembly). John Thacker noted that the change did not yet dissect the reassembled content but was a necessary prerequisite for doing so later; the MR was approved and merged. MR !26379 similarly described a deliberately limited first implementation of DTLS retransmission detection and was merged with that limitation clearly stated.

**Lesson:** A self-contained infrastructure/state improvement can be an acceptable MR even if more complete semantic behavior is deliberately deferred, provided the current change is useful, bounded, and its limitations are explicit.

**Confidence:** Medium-high. Multiple merged examples, though applicability remains contextual.

### Avoid tree-presence guards when they suppress non-UI dissection side effects

**Evidence:** MR !22662 discussion. The author identified `if (tree)` fences in `packet-ptp.c` as preventing TLV dissection on the first visit, which in turn broke statistics and request/response tracking in the nested NTP dissector. The issue was addressed separately in !22663.

**Lesson:** Dissection required for state, statistics, request/response tracking, or other non-display behavior must not depend solely on whether a protocol tree is being built. Be suspicious of `if (tree)` fences around logic that does more than add display items.

**Confidence:** Medium-high. Direct behavioral evidence; corroborate against current dissector guidance and additional reviews.

### Do not claim an unassigned/dynamic port as a fixed protocol port

**Evidence:** MR !26376 proposed automatically binding UDP port 61631 for Thread/CoAP. John Thacker rejected this because the port is in the dynamic/unassigned range and explicitly said to use Decode As; he cited RFC 6282 discussion of the same issue for 6LoWPAN/Thread.

**Lesson:** A protocol's common implementation convention is not enough to justify `dissector_add_uint("udp.port", ...)` when the port is not actually assigned to that protocol. Prefer Decode As or a sufficiently robust heuristic mechanism. Treat fixed-port registration as a protocol/registry claim, not merely a convenience.

**Confidence:** High for direct binding to an unassigned dynamic port. Whether a specific heuristic is safe is a separate question.

### Keep protocol-specific lookup/dependency logic in the protocol that needs it

**Evidence:** MR !26374 (RADIUS/RadSec). Anders Broman twice requested removing protocol-ID plumbing from generic DTLS/TLS code and using `proto_get_id_by_short_name()` in `packet-radius.c` instead. The author changed the lines and the threads were resolved.

**Lesson:** Avoid modifying generic transport/security dissectors merely to expose or carry protocol-specific identity that the consuming dissector can resolve itself. Prefer keeping such dependency/lookup logic local to the consumer when an existing API supports it.

**Confidence:** Medium-high. Repeated inline review feedback on both TLS and DTLS paths in the same MR; seek broader corroboration before treating every cross-dissector dependency this way.

### Translation updates go through Transifex, not direct translation-file MRs

**Evidence:** MR !26392 directly edited the Korean Qt translation file. Alexis La Goutte told the contributor to request Transifex access and complete the translation there, noting the repository is automatically synchronized weekly.

**Lesson:** Wireshark UI translations should be contributed through the project's Transifex workflow rather than by directly editing generated/synchronized translation files in a normal MR.

**Confidence:** High for translation contributions; direct maintainer workflow guidance.

### Check historical-capture compatibility when updating a dissector to a newer specification

**Evidence:** Merged MR !25704 removed a withdrawn PROFINET security operation and updated other definitions to a newer specification. Jaap Keuter explicitly asked how old capture files would be dissected. The contributor investigated the versioning question and, after checking with a PROFINET architect, established that the affected forms had existed only in unreleased developer specifications and therefore did not require compatibility decoding for shipped traffic.

**Lesson:** When a specification revision removes, renames, or changes wire semantics, review the change against historical captures rather than assuming the newest specification is the only contract that matters. If an older wire form was actually released/deployed, preserve a version-aware path or other compatible decode strategy where practical; if it never shipped, record that evidence so removal is an informed compatibility decision.

**Confidence:** High as a review requirement. Direct maintainer review on a merged protocol-specification update, with the compatibility question investigated before merge.

## Review-access note

Use the local JSON corpus in `dheitmueller/wireshark-corpus-mrs` as the preferred source for MR mining. It contains MR metadata, full discussions (including DiffNote `position` objects), changes/diffs, commits, and diff-version metadata, avoiding dependence on GitLab web indexing while preserving review context.