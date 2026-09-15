# Automated MR review: !19390–!19439

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Review direction: descending from newest toward older MRs. This ledger records the exact MRs counted as reviewed in this run. The previously reviewed !17571–!17620 batch remains part of the already-reviewed set.

## Exact reviewed set

!19439, !19438, !19437, !19436, !19435, !19434, !19433, !19432, !19431, !19430,
!19429, !19428, !19427, !19426, !19425, !19424, !19423, !19422, !19421, !19420,
!19419, !19418, !19417, !19416, !19415, !19414, !19413, !19412, !19411, !19410,
!19409, !19408, !19407, !19406, !19405, !19404, !19403, !19402, !19401, !19400,
!19399, !19398, !19397, !19396, !19395, !19394, !19393, !19392, !19391, !19390

Count: **50**.

## Review notes

- Merged !19439 (Gerald Combs): Falco Bridge catches `sinsp_exception` around `open_savefile()` and reports the exception instead of allowing an external-library failure to escape across the integration boundary. This corroborates existing error-boundary guidance.
- Merged !19438 (Gerald Combs, merged by John Thacker): distinguishes Sysdig event-header length from event-data length and uses the latter for packet/capture lengths. This reinforces the existing wire-layout rule that serialized/data lengths must reflect the actual external representation rather than a convenient aggregate structure size.
- Merged !19430 (John Thacker): RTSP/SDP dynamic RTP payload setup follows protocol semantics, resolving relative `control` URIs and retaining media descriptions under the RTSP URI when transport ports are not yet meaningful. This corroborates existing state/context-flow guidance.
- Merged !19412 (John Thacker): request/response tracking explicitly removes an `if (tree)` gate because parsing the request URI is needed for persistent protocol state even when no protocol tree is requested. This independently corroborates the existing notebook rule not to gate stateful dissection on `proto_tree` availability.
- Merged !19407: replaces `json_validate()` because its fixed 1024-token limit rejects otherwise valid large JSON, using `json_parse()` and its return contract instead; a large-JSON capture is supplied as the reproducer. This reinforces API-contract and boundary-case regression-test guidance.
- Merged !19398 (Gerald Combs): recursive Falco configuration-schema parsing receives an explicit maximum-depth check, with the clang-tidy recursion suppression scoped to the intentionally recursive function. This corroborates malformed/untrusted-input resource-bound guidance.
- !19408 reverted the initial Lua Gcrypt integration until Windows/macOS builds and tests were healthy; !19428 is the corrected successor. The accepted successor is weighted more heavily than the reverted attempt.
- The remaining MRs were reviewed for discussion/diff signal and were predominantly protocol-specific corrections, automated registry/dependency updates, warning cleanups, UI/build changes, backports, or changes whose reusable lessons are already represented in the notebook. No new convention was added solely to duplicate existing guidance.
