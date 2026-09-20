# Wireshark MR review automation — 2026-09-20

Model: GPT-5.6 Sol

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `130eea3c8a60902c0fc98c9e1130e73bafc6f815`

## Selection and tracking

Before selecting this batch, the already-reviewed set was reconstructed from the available tracking in `dheitmueller/wireshark-chatgpt`, including `reviewed-mrs.md`, the per-run files in `reviewed-mrs-automation/`, and the historical !17571-!17620 run. The previously reviewed !17571-!17620 batch remains counted. The immediately preceding reviewed batch ended at !15399, and candidate MR numbers were checked against the available tracking rather than treating a numeric interval as automatically reviewed.

The corpus contains a file named `mr_15376.json`, but at this corpus commit its contents are empty. It is therefore not a reviewable MR record and is not counted among the 50 MRs below. To keep this run at exactly 50 actual merge requests, selection continues through !15348.

## Exact MRs reviewed in this run

!15398, !15397, !15396, !15395, !15394, !15393, !15392, !15391, !15390, !15389

!15388, !15387, !15386, !15385, !15384, !15383, !15382, !15381, !15380, !15379

!15378, !15377, !15375, !15374, !15373, !15372, !15371, !15370, !15369, !15368

!15367, !15366, !15365, !15364, !15363, !15362, !15361, !15360, !15359, !15358

!15357, !15356, !15355, !15354, !15353, !15352, !15351, !15350, !15349, !15348

Count: **50 actual MRs**.

Status weighting: **48 merged**, **2 closed/unmerged** (!15373 and !15350). The closed MRs were down-weighted. !15373 was superseded by the accepted !15374 implementation. Repeated stable-branch backports were used as corroboration but were not weighted as independent architectural evidence equal to their master changes.

## Durable findings promoted to the notebook

### State belongs to its semantic owner, not merely the available transport conversation

Merged !15398 (John Thacker; merged by Anders Broman) fixes SDP-generated Call-ID state being stored on an SDP transport conversation that can be reused by unrelated calls. The accepted design hands the state across the current packet and persists it on the higher-level SIP transaction/file-scoped object when that object exists. Added to `dissector-state-conventions.md`.

Notebook commit: `c8381649c5dcf01cd06ea146223a733085ba7eb7`

### Reassembly identity must survive endpoint migration when the protocol session survives it

Merged !15380 (authored and merged by John Thacker) fixes TLS handshake fragments split across QUIC connection migration by replacing endpoint-derived identity with a structured key containing the stable TLS session, reassembly ID, and direction. Stable !15353 independently corroborates the connection-migration identity model for zero-length QUIC CIDs. Added to `stateful-reassembly-conventions.md`.

Notebook commit: `8ea10ef8ca2272ac6ac3f2def66e03f19e19f644`

### Speculative protocol state transitions should commit only after validation

Merged !15365 (John Thacker) fixes QUIC key-phase state being advanced before payload decryption proves that the apparent phase change is real. The accepted code prepares candidate cipher state, commits it only on successful decryption, and discards it otherwise. Added to `state-management-conventions.md`.

Notebook commit: `7c398eaa4a8197da3660061c256015c1ff831b0e`

### Nested dissection must honor enclosing security/encoding state

Merged !15355 (authored and merged by Pascal Quantin), with stable backports !15357 and !15358, carries the outer NAS-5GS security-header state to the nested NAS message-container IE and avoids recursively dissecting ciphered bytes as plaintext. Added to `dissector-context-flow-conventions.md`.

Notebook commit: `2114d1c643209a347c394c2107157f8db114cdb9`

### CI post-job hooks must distinguish success, failure, and cancellation

Merged !15389 (Gerald Combs; merged by Anders Broman) adapts Wireshark to GitLab 17 running `after_script` on canceled jobs. Merged follow-up !15392 (authored and merged by Gerald Combs) corrects the fuzz path so ordinary failures still produce the failure report while canceled jobs exit early. Added to `ci-tooling-conventions.md`.

Notebook commit: `80841182b12fda9a8ce9850694a0ba6275a26811`

### Protocol-declared message length outranks enclosing tvbuff length

Merged !15363, authored and merged by Guy Harris, fixes TIPC filler sizing by using the protocol's `msg_size` instead of the remaining tvbuff length, since the enclosing tvbuff can contain extra junk. This was given especially high weight as direct Guy Harris implementation evidence and was added to `dissector-consumption-boundary-conventions.md`.

Notebook commit: `c64cbc2f3a0200b59ccebd69fc2298522f5e0eaf`

## Additional evidence retained without duplicate notebook rules

- !15366 plus stable !15371/!15368/!15367, authored and merged by Guy Harris, fix four-byte-alignment padding so an already aligned length has zero padding rather than four. This is strong parser arithmetic evidence but did not warrant a duplicate convention.
- !15361, authored and merged by Guy Harris, uses `proto_tree_add_item_ret_length()` so the tree helper that decodes the string also supplies its consumed length instead of independently re-deriving it.
- !15383 makes byte-to-text copy behavior explicitly ASCII versus UTF-8 rather than relying on an ambiguous locale-dependent “Printable Text” operation.
- !15379 contains useful compatibility discussion about whether a renamed display-filter field should be retained as hidden compatibility state; the accepted discussion suggests hidden fields are not automatically justified for every rename.
- !15375 shows pipeline/source checkers catching duplicate field labels/copy-paste mistakes during a substantial Zigbee dissector addition.
- !15351 follows the protocol specification's timestamp semantics by displaying generalized time as UTC rather than local time.
- !15374 is the accepted replacement for closed !15373; the latter was closed because the source branch arrangement was unsuitable, so the accepted MR carries the implementation weight.
- !15350 was closed without merge and was therefore not used as durable implementation evidence.

## Continuation

The corpus is not exhausted. `mr_15347.json` exists and contains a valid MR record at the same corpus commit, so the next review run can continue backward from !15347. No scraper-restart notification is needed for this run.
