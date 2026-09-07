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

**Evidence:** MR !26390. The change introduced several small ST/VANC dissector source files. Anders Broman suggested folding the ST dissectors into one file and explicitly noted that registering multiple protocols from one source file is acceptable/positive.

**Lesson:** Do not assume one protocol registration requires one C source file. For a family of small, tightly related dissectors, a shared source file can be preferred over many tiny files. Consider cohesion and size when choosing file boundaries.

**Confidence:** Medium-high. Direct reviewer guidance, but apply contextually rather than as a universal requirement.

### New protocol functionality should have a sample capture

**Evidence:** MR !22662 (PTP/NTP: Add support for NTP over PTP) received an explicit sample-capture request from Michael Mann. MR !26390 independently received the same request from Anders Broman. MR !26366 (BGP-MUP ADD-PATH) provides a third independent example: Alexis La Goutte asked for a pcap, the author supplied one, and the review thread was resolved.

**Lesson:** When adding or materially extending a dissector, expect reviewers to want representative capture file(s) that exercise the feature. This complements automated dissector tests and makes manual validation/review possible.

**Confidence:** High. Independently requested by three established reviewers on separate protocol/dissector MRs.

### Present review branches as clean, focused commits

**Evidence:** MR !26390 contained three commits for the initial implementation/tests. Anders Broman requested that all commits be squashed into one.

**Lesson:** For a focused Wireshark MR, reviewers may expect fixup/development-history commits to be squashed into a clean logical commit before merge. Preserve meaningful independent commits only when they represent genuinely separable changes.

**Confidence:** Medium. Direct review feedback, but commit structure can be context-dependent.

### Keep unrelated cleanup out of a focused MR

**Evidence:** MR !22662. While extending `dissect_ptp_v2_tlvs()`, the author noticed existing calls that appeared to swap `ptp_tree` and `ti_root`, but explicitly left that issue out of the MR. MR !26369 exposed an unrelated GCC warning while testing a Qt backport; the warning workaround was handled separately in !26371 instead of being folded into the Qt change.

**Lesson:** Preserve scope discipline. Discovery of adjacent existing problems during implementation or CI is not by itself a reason to fold those fixes into the same change.

**Confidence:** High. Repeated project practice plus documented contribution guidance.

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

## Review-access note

Use the local JSON corpus in `dheitmueller/wireshark-corpus-mrs` as the preferred source for MR mining. It contains MR metadata, full discussions (including DiffNote `position` objects), changes/diffs, commits, and diff-version metadata, avoiding dependence on GitLab web indexing while preserving review context.
