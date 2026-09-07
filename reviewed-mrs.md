# Reviewed Wireshark Merge Requests

This is the persistent ledger of Wireshark merge requests already examined for this engineering notebook. Consult this file before selecting MRs for future review so compute is not wasted repeating work.

An MR may be revisited when there is a reason: new substantive review comments, significant new commits, a changed merge outcome, a need to verify an earlier conclusion, or an explicit user request. Record revisits rather than silently treating them as new reviews.

## Review status meanings

- **Deep** — diff/relevant implementation and substantive review discussion were examined sufficiently to extract engineering lessons.
- **Discussion-focused** — useful human review discussion was examined, but the complete code/diff was not necessarily analyzed deeply.
- **Scanned** — metadata, purpose, discussion presence, and relevant final diff/context were checked; no additional durable lesson justified deeper analysis. Treat as reviewed unless it later changes.
- **Partial** — some useful material was examined, but access or scope prevented a complete review. A later revisit may be worthwhile.
- **Probe only** — attempted access but did not obtain enough material to count as a substantive review. These should not normally block a future review.

## Corpus review in progress: commit 2ed161d1 (new !25759-!25932 batch)

Corpus commit `2ed161d19398e620fa89227724ed928d4f5edf27` added !25759-!25932. Review began on 2026-09-07. This is a large batch; only MRs explicitly listed below have been examined so far. Do **not** treat the entire range as reviewed.

| MR | Status | Notes |
|---|---|---|
| !25759 | Scanned | release-4.6 backport of sharkd IO-graph cleanup-on-error fix. Merged; no substantive human review in this snapshot. |
| !25760 | Scanned | release-4.4 backport of the same sharkd IO-graph cleanup fix. Merged; no additional lesson. |
| !25761 | Scanned | release-4.6 sharkd tap-registration cleanup-on-failure fix. Merged; refactors single-tap registration so already registered taps are deregistered/freed on later failure. No substantive human review beyond approval. |
| !25762 | Scanned | release-4.6 setup-script change to prefer Qt 6 based on package availability instead of distro-version heuristics. Merged; no substantive human review. |
| !25763 | Deep | NVMe-MI split-by-type + Control Primitive dissector. Alexis questioned creating more dissector files; author justified the split by expected large follow-up command sets and keeping framing thin. Split design ultimately merged, providing the large/expanding-protocol counterexample to the !26390 “consolidate tiny related dissectors” guidance. |
| !25764 | Deep | MCTP-over-SMBus propagates stable physical addresses for downstream state and independently guards captured length for Decode As entry. Merged; strong evidence for choosing stable conversation identity and enforcing entry-point preconditions rather than relying on heuristic callers. Added to `dissector-conventions.md`. |
| !25765 | Scanned | OSPFv3 Authentication Trailer preference/LLS handling. Merged, no substantive human review. |
| !25766 | Discussion-focused | Clang Analyzer dead-store fixes. Martin Mathieson described using cppcheck and `./tools/check_dissector.py --commits 10` to find recent-commit issues. Added recent-commit static-check guidance. |
| !25767 | Scanned | Broad dissector-warning cleanup. Merged; no substantive human review. |
| !25768 | Scanned | NAS 5GS S-NSSAI location-validity fix: corrects NR-CGI count from 8 to 16 bits and attaches decoded MCC/MNC to the intended subtree. Merged; no substantive human review. |
| !25769 | Scanned | release-4.6 backport of !25768 NAS 5GS fix. Merged; no additional lesson. |
| !25775 | Scanned | O-RAN FH CUS c-plane/u-plane section/timing-header consistency checks. Merged; no human review lesson beyond accepted validation logic. |
| !25777 | Deep | Display-filter FT_SCALAR NULL-deref hardening. Merged fix makes pseudo-type handling explicit, keeps error formatting safe, uses a non-compiled-out abort check for invalid lookup bounds, and adds five regression tests. Useful defensive-library exemplar. |
| !25781 | Discussion-focused | PortableApps plugin preservation. Gerald Combs requested a version adjustment; merged after change. Mostly packaging-specific. |
| !25795 | Discussion-focused | release-4.6 backport of OPC UA Abort parsing fix. Anders Broman explicitly noted that correctness issues found in a backport should be fixed in master rather than independently changing the backport. Useful branch/backport workflow rule. |
| !25797 | Discussion-focused | UET SACK PSN display. Alexis suggested representing calculated information as a generated field; discussion emphasized consistency with existing related fields. Merged final implementation retained the protocol's established style, so do not generalize the generated-field suggestion as mandatory. |
| !25800 | Scanned | Shared-library version simplification in CMake. Merged; no substantive human review. |
| !25803 | Deep | OPC UA padding memory-safety fix found by Clang ASan + `fuzz-test.sh`. Strong sanitizer/reproducer exemplar. Pipeline/reviewer also enforced Wireshark commit-message format (brief component-prefixed subject, blank line before body), now added to our MR checklist. |
| !25810 | Discussion-focused (open snapshot) | Draft NTPv5 preparation with substantial timescale/era discussion. John Thacker favored solving the underlying wider-time/timescale representation first rather than encoding a narrow workaround. Open/unmerged, so treat as provisional architecture evidence and revisit on outcome/change. |

## Corpus review in progress: commit 95ef115d (new !25933-!26205 batch)

Corpus commit `95ef115dffb8e01e1896fcaebdd09c78764d1712` added the large !25933-!26205 range. Review began on 2026-09-07. This section intentionally records only MRs actually examined so far; do **not** treat the entire numeric range as reviewed yet.

| MR | Status | Notes |
|---|---|---|
| !25942 | Scanned | Automatic data/translation update; merged, no substantive human review. |
| !25943 | Discussion-focused (closed) | Experimental Lua UTF-8 manifest work. Human discussion clarified manifest/test behavior, but MR was closed unmerged; useful context only, not an accepted implementation exemplar. |
| !25946 | Deep | NMEA0183 consolidation. Pascal Quantin requested reuse of the already-fetched sentence ID and pointed to existing `epan/strutil.h` conversion helpers instead of local reinvention. Merged; strong anti-double-fetch/helper-reuse evidence. |
| !25950 | Scanned | Export Packet Dissections C-array index/options feature. Merged with release-note/API updates; no substantive human review. |
| !25952 | Discussion-focused | Dissector warning cleanup. Review demonstrates checking protocol-specific assumptions with the knowledgeable maintainer rather than mechanically applying warning-driven changes; merged after correction. |
| !25953 | Scanned | DECT NR public `dect_nr_info_t` for parent-supplied PHF type. Merged; no substantive human review. |
| !25971 | Discussion-focused (open snapshot) | Tagging Rules feature. Stig Bjørlykke argued that behavior with different semantics/lifecycle from coloring rules should be modeled as a separate feature rather than overloaded into coloring rules. Open/unmerged, so architecture lesson remains provisional. |
| !25973 | Discussion-focused | WSLua Python/Ruff cleanup. John Thacker caught import ordering via the project pipeline even when the author's local Ruff did not report it; reminder that project CI/tool version is authoritative for submission readiness. Merged. |
| !25977 | Deep | LBMC TSNI Request refinement. Merged MR description contains a useful explicit Testing section: named pcap, tshark verification of changed and unaffected cases, and clean warning-free build. Additional corroboration for our MR checklist practice. |
| !25984 | Deep | BT BR/EDR reassembly-table registration bug. Merged fix; Stig/Mathieson discussed adding automated checks to ensure declared/used reassembly tables are initialized/registered. Added to conventions/pre-submit checks. |

## Corpus review: !26206 through !26226

The `faa3a72bd6a67bf0548e1d646336c6c714ccfa18` corpus batch added !26206-!26226 and was reviewed on 2026-09-07. !26229 was additionally reviewed because Guy Harris explicitly superseded !26224 with it; the merged successor is the stronger architectural evidence.

| MR | Status | Notes |
|---|---|---|
| !26206 | Scanned | `ws_label_strcpy` safety cleanup. Merged; documents why the string is kept NUL-terminated incrementally and why blindly truncating at the final byte could split UTF-8. No substantive human review. |
| !26207 | Scanned | Release-4.6 CMake dependency-update backport. No substantive human review. |
| !26208 | Deep | DICOM Export Objects overflow fix/backport. Uses size-appropriate types and checked arithmetic (`ckd_add`) for accumulated object size; corroborates defensive length handling. |
| !26209 | Scanned | Release-4.4 backport of the same DICOM overflow fix as !26208. |
| !26210 | Scanned | Extcap control-pipe capability negotiation and backward-compatible defaults. Merged; no durable reviewer feedback relevant to current priorities. |
| !26211 | Deep | UDX sequence analysis/reassembly corrections. Validated packet-by-packet against an instrumented libudx over its own test suite and added five captures/six focused tests. Strong differential-testing exemplar. |
| !26212 | Scanned | Capture header include portability fix. Merged; no substantive human review. |
| !26213 | Scanned | Stops writing extcap preferences into the main preferences file. Merged extcap-specific change; no substantive human review. |
| !26214 | Scanned | Python Ruff cleanup. Merged; no substantive human review relevant to notebook goals. |
| !26215 | Deep | DICOM 32-bit portability follow-up. Changes accumulated length to `size_t`, combines `ckd_add()` with an explicit realistic cross-platform upper bound, and preserves traversal needed to rewind state. |
| !26216 | Scanned | MSYS2/cross-compiled Windows library lookup fix. No substantive human review. |
| !26217 | Scanned | GSM cell-ID discriminator/fallthrough correction backport. Merged; no substantive human review. |
| !26218 | Deep | New GUE dissector. Merged exemplar: IANA port registration, payload dispatch via `ip.proto`, undefined extensions left opaque, expert-info validation, five-packet capture, four suite tests, and clean `check_dissector.py`/`fuzz-test.sh`. |
| !26219 | Scanned | Large merged extcap bookmark/info feature. Human discussion is primarily author design/TODO tracking rather than reusable reviewer correction; no additional dissector convention extracted. |
| !26220 | Scanned | Removes obsolete MinGW `-municode` linkage from CLI tools. Merged; no substantive human review. |
| !26221 | Scanned | Follow-up removal of `-municode` from extcaps. Merged; no substantive human review. |
| !26222 | Scanned | Adds `jsonschema` to macOS CI Python environment. Merged; no substantive human review. |
| !26223 | Deep | Large merged NVMe/NVMe-MI/TCG Storage expansion and hardening. Key lessons: conservative dispatch, don't hide bytes unless decoding actually occurred, mark incomplete MIC as unverified rather than invalid, audit filter compatibility when changing hf semantics, include complete identity tuple in reassembly keys, and validate protocol-specific minimum lengths. |
| !26224 | Discussion-focused (closed/superseded) | IEEE 802.15.4 FCS fix proposed runtime pointer-identity discrimination for different meanings of dissector `data`. Guy Harris said it should use different dissectors calling common code, implemented !26229, and closed this MR. The rejected implementation is not an exemplar; Guy's correction is high-authority negative guidance. |
| !26225 | Scanned | Release-4.6 backport of !26215 DICOM 32-bit fix. |
| !26226 | Scanned | Release-4.4 backport of !26215 DICOM 32-bit fix. |
| !26229 | Deep (successor to !26224) | Guy Harris-authored and merged replacement. Uses separate dissector entry points for “FCS type supplied in data” versus top-level/pseudo-header call paths, sharing common parsing beneath them. Very high-confidence architectural exemplar. |

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

Prioritize information density over recency: substantive human review (especially Guy Harris and other core maintainers), dissector/libwireshark API changes, malformed/truncation handling, field/filter semantics, registration/handoff, reassembly, RTP/media, tests, and fuzzing. Prefer merged implementations as exemplars; for closed/superseded work, retain authoritative negative review guidance but follow through to the merged successor where possible.

For open MRs marked `Deep (open snapshot)`, `Scanned (open snapshot)`, or similar, revisit when `updated_at`, head SHA, diff-version set, discussion set, or merge state changes. For merged/scanned MRs, revisit only for a concrete reason.