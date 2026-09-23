# Automated Wireshark MR review: !12513 through !12562

Corpus commit used: `ddcaa22b51c68f594e425a23388c3a2086813054`

Selection method: rebuilt the already-reviewed MR set from the available `reviewed-mrs-automation/` per-run ledgers, supplemental automation tracking, and `reviewed-mrs.md`, preserving the historical !17571-!17620 batch. The previous lookup of !12562 was only a frontier probe and was not counted as reviewed. No candidate in this batch appeared in the reviewed set. These are therefore the fifty highest-numbered previously unreviewed MRs present in this corpus snapshot. The set happens to be contiguous; that was verified from the tracking rather than assumed from numeric range coverage.

Exact reviewed MR set (50):

`!12562, !12561, !12560, !12559, !12558, !12557, !12556, !12555, !12554, !12553, !12552, !12551, !12550, !12549, !12548, !12547, !12546, !12545, !12544, !12543, !12542, !12541, !12540, !12539, !12538, !12537, !12536, !12535, !12534, !12533, !12532, !12531, !12530, !12529, !12528, !12527, !12526, !12525, !12524, !12523, !12522, !12521, !12520, !12519, !12518, !12517, !12516, !12515, !12514, !12513`

Status: 45 merged; 5 closed/unmerged (!12549, !12547, !12546, !12545, !12531). Closed work was down-weighted as implementation evidence while retaining useful maintainer review.

## Durable findings promoted to notebook conventions

- **!12552, with !12555-!12558 — locale-independent machine-text comparison.** John Thacker's merged master fix avoids the host `strcasestr()` for protocol-oriented case-insensitive searching because libc comparison is locale-sensitive; Turkic `i`/`I` mappings are the concrete failure mode. Guy Harris specifically requested the semantically explicit name `ws_ascii_strcasestr()`. John also separated the behavior fix from the API rename so stable branches could receive the correctness repair without unnecessary compatibility churn. Added `protocol-text-comparison-conventions.md`.
- **!12515, with !12542/!12543 — regression tests belong with the invariant they should have caught.** `wmem_list_insert_sorted()` updated list contents without incrementing the public count. Jaap Keuter explicitly required a regression in `wsutil/wmem/wmem_test.c:wmem_test_list()`, noting that the existing unit-test family should have caught the defect. Added `regression-test-placement-conventions.md`.
- **!12536 — declare field-display policy instead of rewriting item text.** John Thacker extended `BASE_NO_DISPLAY_VALUE` to string-like fields and converted SIP from `proto_item_set_text()` label replacement to `FT_STRING, BASE_NONE | BASE_NO_DISPLAY_VALUE`. Added `field-display-policy-conventions.md`.
- **!12524, corroborated by release-4.2 !12647 — Wiretap record/interface link types must agree.** !12524 is the original merged master BLF change; !12647 is its later stable cherry-pick. Corrected `wiretap-record-interface-conventions.md`, which had previously mislabeled !12647 as the master evidence. The substantive rule remains: `WTAP_ENCAP_NONE` means an interface has not yet been established, `WTAP_ENCAP_UNKNOWN` means unknown/bogus, and records cannot be attached to an IDB with a different link type.

## Strong corroborating/review findings retained without duplicating existing rules

- **!12561 — extcap CLI additions require documentation updates.** Gerald Combs requested adding the new `--extcap-capture-filter` option to `doc/androiddump.adoc` as part of the same MR; the accepted master change was later backported in !12645. This reinforces the existing rule that implementation, help/docs, and public CLI syntax form one interface contract.
- **!12541 — normalize differing entry mechanisms into conversation state.** John Thacker's merged WebSocket change distinguishes HTTP Upgrade, Decode As, and heuristic entry, derives the server port appropriately for each, and stores that choice in the WebSocket conversation before port-table dispatch. This corroborates existing conversation/context-flow guidance rather than requiring a new rule.
- **!12548 — publish parent column text before a subdissector can own/overwrite it.** The merged BT Mesh fix moves the parent column update before invoking the Lua/vendor subdissector. Useful call-order evidence, but narrower than existing subdissector/column ownership guidance.
- **!12527 — reuse existing proto APIs and avoid misleading helper names.** Alexis La Goutte pointed out that `proto_tree_add_time_item()` already exists and that a local dissector helper should not be named like a core `proto_tree_add_*` API. The accepted refactor used a protocol-local helper. This reinforces existing helper-reuse and naming practices.
- **!12530 — standards extensions should include the related normative bit semantics.** Jaap Keuter asked that OSPF's N/H bit text be filled from the IANA registry while the new S bit was being added. Useful protocol completeness evidence, but not a distinct cross-project architecture rule.
- **!12525 — preserve authoritative ASN.1 source material.** The merged IEEE 1609.2 cleanup restored original ASN.1 files, comments, and provenance links with only generator-required ordering adjustments. This corroborates the notebook's authoritative-source/generator-input guidance.
- **!12528/!12544/!12551 — plugin discovery should identify compatibility semantically rather than by an awkward directory convention.** The accepted series removes mandatory type subdirectories, adds an explicit type/signature compatibility check, limits recursion depth, then moves Falco plugins outside Wireshark's plugin scanner because they use a different plugin API. Useful plugin-architecture evidence, but subsystem-specific enough not to generalize beyond existing architecture guidance.
- **!12534/!12535/!12537/!12529 — logging should use the owning subsystem's domain.** Guy Harris's master/release Wiretap work reinforces consistent log-domain attribution. Narrow operational convention; retained here rather than adding another standalone notebook rule.

## Closed/unmerged work and negative review evidence

- **!12549, !12546, !12545** — early BT Mesh proposals were closed while the accepted fixes were split/reworked into merged !12548/!12550 and related changes. They are not treated as implementation exemplars.
- **!12547** — closed BTATT Read Multiple Variable work. Jaap Keuter identified that a `remain` variable had become a loop-cycle counter rather than a TVB-byte-remaining value; Gerald Combs additionally warned that making it unsigned brought the code close to underflow. Because the MR closed with unresolved review/conflicts, this is negative review evidence only, reinforcing the existing rule to keep units/domains explicit and avoid unsigned countdown underflow.
- **!12531** — closed Wiretap log-domain backport attempt. The accepted master/release forms in !12529/!12537 carry the implementation weight instead.

## Remaining MRs in this batch

The other merged MRs were scanned for discussions/diffs and were primarily protocol-specific feature additions, stable backports, documentation/build fixes, generated-parser updates, info-column improvements, logging polish, or narrow dissector cleanup. They did not add durable conventions beyond those already recorded. These include !12562, !12560, !12559, !12554, !12553, !12550, !12540, !12539, !12538, !12533, !12532, !12526, !12523, !12522, !12521, !12520, !12519, !12518, !12517, !12516, !12514, and !12513.

Frontier check only (not reviewed or counted): !12512 (`r09: use proto_tree_add_item for bcd fields`) exists in the same corpus commit and is merged. It is the next descending candidate absent newly scraped higher-numbered unreviewed material.
