# Wireshark MR review automation ledger: !11563–!11612

- **Corpus repository:** `dheitmueller/wireshark-corpus-mrs`
- **Corpus commit reviewed:** `ddcaa22b51c68f594e425a23388c3a2086813054`
- **Notebook starting commit:** `46ae9a3dbbd1f6d39ed3901490c247558027a5bf`
- **Review direction:** descending MR number, newest available previously-unreviewed first
- **Exact MR count:** 50
- **Disposition:** 50 merged; 0 closed/unmerged

## Selection and duplicate avoidance

Before selecting this run, the available review tracking in `dheitmueller/wireshark-chatgpt` was reconciled, including `reviewed-mrs.md`, the supplemental automation tracker, and the per-run files under `reviewed-mrs-automation/`. The historical `!17571`–`!17620` batch remains part of the already-reviewed set. Numeric ranges were not assumed complete merely because some entries appeared in a ledger. The prior lookup of `!11612` was only a frontier probe from the preceding run and was not counted as a review.

## Exact reviewed MR set

The following fifty MRs were reviewed in descending order:

- !11612
- !11611
- !11610
- !11609
- !11608
- !11607
- !11606
- !11605
- !11604
- !11603
- !11602
- !11601
- !11600
- !11599
- !11598
- !11597
- !11596
- !11595
- !11594
- !11593
- !11592
- !11591
- !11590
- !11589
- !11588
- !11587
- !11586
- !11585
- !11584
- !11583
- !11582
- !11581
- !11580
- !11579
- !11578
- !11577
- !11576
- !11575
- !11574
- !11573
- !11572
- !11571
- !11570
- !11569
- !11568
- !11567
- !11566
- !11565
- !11564
- !11563

All fifty corpus entries are merged. Merge status was still not treated as the only weighting signal: !11571 was accepted historically but later produced parser regressions explicitly diagnosed by John Thacker and corrected by the already-reviewed !11690, so its parser design was down-weighted as superseded evidence rather than promoted as a durable convention.

## Durable findings promoted to the notebook

- **!11601, reinforced by !11602 and prior !11655 — flush startup framing before waiting for payload (extremely high confidence).** John Thacker's merged master extcap fix explicitly flushes the pcap header after writing it so dumpcap can complete `cap_pipe_open_live()` even when no packet ever arrives. Guy Harris strongly approved the behavior, compared it to tcpdump's flush-at-file-start behavior, and suggested that the common pcap-header writer could eventually own pipe flushing. The release-4.0 backport !11602 preserves the same contract with Guy as committer/approver, and merged !11655 from the previous batch independently demonstrates the same header-boundary flush. Added `stream-startup-conventions.md` in commit `0694511e72b0bb70f3278ae28137c016d02c48aa`.
- **!11597 — keep shared generated inputs reviewable and generator changes separable (very high confidence).** The merged DRSUAPI/PIDL update received extensive review from Stefan Metzmacher in his Samba/PIDL area of authority. He required the IDL diff to avoid wholesale whitespace churn so it remained comparable/portable to Samba, clarified the correct PIDL-owned source tree and regeneration workflow, and requested that generator changes be isolated so Samba could import them independently. Added this guidance to `generated-code-conventions.md` in commit `0563424a5d470842a1a0a576bb6d2d539e2dd401`.
- **!11574 — derive masks from the protocol's own bit-numbering convention (very high confidence).** The Wi-SUN Join Metrics IE was decoded with reversed ID/length masks because its specification numbers/transmits bits differently from the convention commonly assumed from RFC-style diagrams. The accepted fix follows Wi-SUN's explicit bit-order prose and was approved/merged by Alexis La Goutte. Added `bit-order-conventions.md` in commit `f4ae2b3cf2bceca375a91959a17a62716290ed30`.
- **!11567 — apply each representation transform exactly once across dissector layers (very high confidence).** Telnet subnegotiation had already unescaped doubled IAC bytes before dispatch, but the Kerberos AUTHENTICATION path performed the same unescape again, corrupting legitimate repeated `0xff` data. The merged fix removes the child-side duplicate normalization and passes a subset TVBuff of the already-normalized option bytes to Kerberos. Anders Broman approved/merged it. Added `layered-decoding-conventions.md` in commit `d1010658d49bae93b0ad8d58fc976b439293f616`.

## Strong corroborating evidence retained without duplicating existing rules

- **!11607 — partial/fragmented packet handling.** John Thacker's merged SNMP fix avoids rejecting a fragmented or error-embedded packet solely because the complete BER length is not present in the current TVBuff. Complete-PDU validation remains for normal packets, while partial paths can display the available tree and allow normal fragment bounds behavior. This reinforces existing truncation/heuristic guidance rather than creating a new rule.
- **!11596 — wire-backed versus generated fields.** Jaap Keuter explicitly rejected marking an Ethernet client-identifier interpretation as generated because the bytes are actually present in the packet. This directly corroborates the existing rule that generated fields are for derived values, not merely alternate interpretations of wire-backed bytes.
- **!11586 — field registration must match byte ownership.** A typed-item warning exposed one four-byte displayed range registered as `FT_UINT16`; the accepted Wi-SUN fix exposes the two actual 16-bit start/end fields separately. This reinforces field-width and checker guidance already in the notebook.
- **!11581 — use bitmask helpers for packed flag fields.** Anders Broman recommended `proto_tree_add_bitmask_list()` rather than manually adding individual bits when a byte is structurally a flag/bitmask container. Existing bit-field API guidance already covers this direction.
- **!11580 — regenerate golden output after output-contract changes.** John Thacker identified failing JSON/EK tests after the new displayed-frame-number field and pointed to the documented `test/README.test` regeneration process. The MR was fixed before merge, reinforcing that machine-readable output changes must update their expected fixtures.
- **!11576 with !11589/!11590 — model variable-length opaque data as bytes and scope backports to the bug.** The PFCP Offending IE value was corrected from a fixed four-byte integer to `FT_BYTES` spanning the actual remaining IE value. Stable-branch discussion also separated the correctness fix from unrelated dead-field cleanup, useful corroboration for semantically focused backports.
- **!11582 — provenance matters for contributed protocol code.** The HSFZ contributor explicitly obtained signed permission to publish the protocol code after the earlier attempt. Useful submission/provenance evidence, but not generalized beyond existing contribution/licensing practice.
- **!11565 — checker warnings on value-string APIs are actionable review findings.** Alexis La Goutte called out `val_to_str()` use with an unsuitable unknown-value string; the contributor fixed it before merge. Existing value-string/static-checker guidance already captures the principle.

## Down-weighted or superseded evidence

- **!11571 — merged, but later parser behavior was superseded.** The BT-DHT robustness series attempted stricter parsing and separated valid zero-length strings from failure, but John Thacker later documented that the accepted version created infinite loops and stack overflows. In particular, callers converted child-parser failure value `0` into apparent progress or allowed offsets to move backward. He pointed to !11690, which later fixed the failure propagation. The useful lesson is therefore the already-recorded parser-progress/error-propagation rule from !11690, not the intermediate !11571 implementation.
- **Stable backports !11602, !11595, !11594, !11593, !11592, !11590, !11589, !11588, and !11587** were reviewed and used as corroboration where applicable, but their merged master counterparts carry the primary architectural weight.
- **Automatic generated-data updates !11612, !11611, !11610, !11570, !11569, and !11568**, plus narrow documentation, UI, packaging, registry-data, and protocol-specific fixes in the remainder of the batch, were reviewed but did not add a durable convention beyond existing notebook material.

## Additional reviewed material

The remaining merged MRs were checked for discussion, diff semantics, testing evidence, and relationship to existing rules. Highlights include !11609's representative capture for the SVCCTL parsing fix; !11604/!11603's distinction between system locale settings and Wireshark-selected display language; !11598's correction of an apparent specification-diagram typo using the surrounding normative text; !11585's TFTP static-analysis fix with stable backports; !11584/!11575 macOS signing/build fixes; !11579/!11564 manuf-generation maintenance; !11578 Qt packaging maintenance; !11577 falcodump corrective maintenance; !11573/!11572 focused Qt/progress arithmetic fixes; !11566 dead TECMP FlexRay cleanup; and !11563 platform-correct profile path presentation. None justified duplicating a notebook rule already covered more authoritatively elsewhere.

## Frontier probe (not reviewed)

`!11562` (`debian: New lintian override format`) exists in the same corpus commit and is merged. It was inspected only to verify that the corpus continues below this batch and is **not** part of the fifty reviewed MRs above. It remains eligible for the next descending run unless newly scraped higher-numbered unreviewed material appears.