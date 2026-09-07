# Reviewed Wireshark Merge Requests

This is the persistent ledger of Wireshark merge requests already examined for this engineering notebook. Consult this file before selecting MRs for future review so compute is not wasted repeating work.

An MR may be revisited when there is a reason: new substantive review comments, significant new commits, a changed merge outcome, a need to verify an earlier conclusion, or an explicit user request. Record revisits rather than silently treating them as new reviews.

## Review status meanings

- **Deep** — diff/relevant implementation and substantive review discussion were examined sufficiently to extract engineering lessons.
- **Discussion-focused** — useful human review discussion was examined, but the complete code/diff was not necessarily analyzed deeply.
- **Scanned** — metadata, purpose, discussion presence, and relevant final diff/context were checked; no additional durable lesson justified deeper analysis. Treat as reviewed unless it later changes.
- **Partial** — some useful material was examined, but access or scope prevented a complete review. A later revisit may be worthwhile.
- **Probe only** — attempted access but did not obtain enough material to count as a substantive review. These should not normally block a future review.

## Corpus review: !26365 through !26393

The JSON corpus snapshot in `dheitmueller/wireshark-corpus-mrs` was reviewed across the full contiguous range !26365-!26393 on 2026-09-07. Low-information MRs were deliberately scanned rather than over-analyzed; MRs containing useful implementation or human-review evidence received deeper treatment.

| MR | Status | Notes |
|---|---|---|
| !26365 | Deep | BT AVCTP reassembly overflow hardening. Uses checked addition (`ckd_add`), caps overlong reassembly, emits expert info, and notes manual code should migrate to standard reassembly API. |
| !26366 | Deep | BGP-MUP ADD-PATH. Alexis La Goutte requested a pcap; contributor supplied one and thread was resolved. Third independent evidence for sample-capture expectation. |
| !26367 | Deep | RSL SACCH Information IE. Uses `proto_tree_add_item_ret_uint8/uint16` when displayed fields are also needed by parser logic; useful corroboration for avoid-double-fetch idiom. |
| !26368 | Scanned | Qt profile ZIP hardening backport to release-4.6. Security/size/path checks; no substantive human review discussion. |
| !26369 | Scanned | Same Qt ZIP hardening for release-4.4. CI exposed unrelated GCC warning; workaround kept in separate !26371, reinforcing scope discipline. |
| !26370 | Scanned | Python/ruff cleanup across scripts. No substantive human review discussion relevant to current notebook goals. |
| !26371 | Scanned | Targeted release-4.4 compiler-warning workaround created separately from !26369. |
| !26372 | Scanned | Release-build/version MR; no substantive review discussion. |
| !26373 | Scanned | EAP-GTC dissector support; merged without substantive human review comments in corpus snapshot. |
| !26374 | Deep (open snapshot) | RADIUS/RadSec. Anders Broman twice requested keeping protocol-ID lookup in `packet-radius.c` via `proto_get_id_by_short_name()` rather than adding RADIUS-specific plumbing to TLS/DTLS. Threads fixed/resolved. Revisit when MR changes. |
| !26375 | Scanned | Version bump/preparation MR; no substantive review discussion. |
| !26376 | Deep (open snapshot) | CoAP/Thread decoupling. John Thacker rejected automatic binding to dynamic/unassigned UDP 61631 and said to use Decode As; MR also moves toward a generic CoAP heuristic payload extension point. Revisit when discussion/head changes. |
| !26377 | Deep | C2P injected facility packet dissection. Merged implementation demonstrates dispatch to available protocol dissectors with fallback behavior; no substantive reviewer comments. |
| !26378 | Scanned | Extcap `--extcap-config` caching; relevant to extcap architecture but no substantive review discussion for current dissector-focused notebook. |
| !26379 | Deep | DTLS retransmission detection. Merged deliberately bounded implementation whose limitations are explicitly documented, corroborating incremental self-contained protocol changes. |
| !26380 | Scanned | SGP.32/SGP.22 dispatch discrimination by ASN.1 tag; merged without substantive review discussion. |
| !26381 | Scanned | IEEE1905 EasyMesh flag-display correction; merged without substantive review discussion. |
| !26382 | Deep | BT HCI ACL reassembly overflow hardening. Avoids overflow-prone `cur_off + len` test by subtracting remaining capacity and clipping copy length. |
| !26383 | Scanned | NAS EPS EMM transport dissection improvement; merged without substantive review discussion. |
| !26384 | Scanned | Release-4.6 backport of !26380. |
| !26385 | Scanned | CMake path quoting fix; no substantive review discussion. |
| !26386 | Scanned (open snapshot) | Qt packet-list turbo navigation. No human review comments in snapshot; revisit if discussion/state changes. |
| !26387 | Scanned | Automatic data/translation update for release-4.4. |
| !26388 | Scanned | Automatic data/translation update for release-4.6. |
| !26389 | Scanned | Automatic data/translation update for master. |
| !26390 | Deep (open snapshot) | ST 2110-40 plus related VANC dissectors. Anders Broman review: protocol-prefix `hf_`/`ett_`; use native bit APIs; provide pcaps; squash commits; consider consolidating small related dissectors. Snapshot head `d7fd5e9c1bc2cedf092bc0bef7bfa33a6e18712c`; revisit on change. |
| !26391 | Deep | `dissectors: detect more double fetching and fix some`. Systematically replaces `tvb_get_*` + `proto_tree_add_item` pairs with `proto_tree_add_item_ret_*`, strongly establishing the display-and-return idiom. |
| !26392 | Discussion-focused (open snapshot) | Direct Korean translation MR. Alexis La Goutte instructed contributor to use Transifex; repository translation files are weekly auto-synced. Revisit if outcome changes. |
| !26393 | Deep | eCPRI/O-RAN interaction. Merged code adds eCPRI fields but hides them with `proto_item_set_hidden()` when O-RAN FH claims the PDU, preserving filterability without duplicate visible presentation. |

## Earlier reviews

| MR | Status | Notes |
|---|---|---|
| !26361 | Partial | TCP unusual-flags change; MR page/relevant test context accessible during calibration run. No major durable reviewer convention extracted yet. |
| !26358 | Probe only | Initial web-only calibration attempt; corpus did not contain this MR in the 2026-09-07 snapshot. |
| !26323 | Partial | HyperDHT dissector; malformed/truncation and heuristic-recognition testing observed. No major durable reviewer convention extracted yet. |
| !26313 | Probe only | Initial web-only calibration attempt. |
| !26280 | Probe only | Initial web-only calibration attempt. |
| !26245 | Probe only | Initial web-only calibration attempt. |
| !22662 | Discussion-focused | NTP over PTP. Michael Mann requested a sample capture. Review/discussion exposed first-pass dissection problems caused by `if (tree)` guards and demonstrated splitting a prerequisite fix into separate MR !22663. Lessons recorded in `review-patterns.md`. |
| !22208 | Discussion-focused | Alexis La Goutte requested use of a named topic branch rather than submitting from fork `master`; contributor-workflow lesson recorded in `review-patterns.md`. |
| !20793 | Discussion-focused | John Thacker recognized an incremental prerequisite change even though reassembled content was not yet dissected; lesson recorded in `review-patterns.md`. |
| !16672 | Discussion-focused | Older MR with substantive Guy Harris review threads identified as valuable evidence. Revisit for deeper extraction if not already fully captured in `review-patterns.md`. |
| !19481 | Partial | MR activity/reviewer discussion was confirmed accessible during initial GitLab-access testing. No durable lesson yet recorded; may be worth revisiting. |

## Selection guidance

Use `dheitmueller/wireshark-corpus-mrs` as the preferred source. Before selecting work, compare corpus entries against this ledger. Do not re-review an unchanged JSON snapshot.

Prioritize information density over recency: substantive human review (especially Guy Harris and other core maintainers), dissector/libwireshark API changes, malformed/truncation handling, field/filter semantics, registration/handoff, reassembly, RTP/media, tests, and fuzzing.

For open MRs marked `Deep (open snapshot)`, `Scanned (open snapshot)`, or similar, revisit when `updated_at`, head SHA, diff-version set, discussion set, or merge state changes. For merged/scanned MRs, revisit only for a concrete reason.
