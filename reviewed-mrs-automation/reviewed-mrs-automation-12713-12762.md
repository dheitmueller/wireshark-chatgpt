# Automated Wireshark MR review: !12713–!12762

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`

Review direction: descending by MR number after reconstructing the exact already-reviewed set from `reviewed-mrs.md`, `reviewed-mrs-automation/reviewed-mrs-automation.md`, and the available per-run ledgers in `reviewed-mrs-automation/`. The historical !17571–!17620 batch remains preserved and counted. The previous run's inspection of !12762 was only a frontier check and was not treated as a prior review. No numeric interval was assumed reviewed merely from its endpoints.

Exactly 50 previously unreviewed MRs were reviewed in this run. Of these, 45 were merged. Three were closed/unmerged and were down-weighted: !12747, !12739, and !12713. Two were still open in the corpus snapshot and were also down-weighted: !12719 and !12716.

## Exact MRs reviewed

- !12762
- !12761
- !12760
- !12759
- !12758
- !12757
- !12756
- !12755
- !12754
- !12753
- !12752
- !12751
- !12750
- !12749
- !12748
- !12747
- !12746
- !12745
- !12744
- !12743
- !12742
- !12741
- !12740
- !12739
- !12738
- !12737
- !12736
- !12735
- !12734
- !12733
- !12732
- !12731
- !12730
- !12729
- !12728
- !12727
- !12726
- !12725
- !12724
- !12723
- !12722
- !12721
- !12720
- !12719
- !12718
- !12717
- !12716
- !12715
- !12714
- !12713

## Durable findings promoted to the notebook

### Empty-value and compatibility-wrapper API semantics — !12720, !12722

Merged master !12720, authored and merged by John Thacker, fixes `wmem_strjoinv()` so an empty but valid string array returns a wmem-allocated empty string, matching `g_strjoinv()`, while a `NULL` array remains invalid. João Valverde's review also moved the invalid-input path to the project's standard `ws_return_val_if()` idiom. Merged release-4.2 backport !12722 carries the same contract and documentation. This strengthens `empty-value-api-conventions.md`: allocator/lifetime variants of an established API should preserve the modeled API's edge-case semantics, and valid-empty must remain distinct from invalid/absent.

### Semantic sub-PDU lengths — !12741, !12743, !12744

Merged master !12741 fixes TECMP's CAN metadata by setting `can_info.len` to the already-established CAN payload length rather than `tvb_captured_length_remaining()` after the parser cursor had advanced. The old value described bytes remaining after the payload and caused downstream dissectors such as ISO 15765 to fail. The correction was immediately backported in merged !12743 and !12744. This extends `framing-boundary-conventions.md`: once the parent has a semantic child-PDU length, pass that value to subdissectors rather than substituting a cursor-relative remaining length.

## Strong corroboration retained without duplicate notebook rules

- !12750, authored and merged by Guy Harris, explicitly selects libgcrypt's Unix random backend for macOS rather than allowing configure-time defaults to assume Linux-style `getentropy()`/`getrandom()` behavior. This is high-authority corroboration for the existing platform-capability and platform-assumption guidance, but it did not justify a duplicate rule.
- !12756 together with !12757 and !12758 improves dumpcap/Npcap diagnostics by recognizing older Npcap versions and recommending an upgrade for promiscuous-mode failures. Guy Harris authored the accepted master change and release backports; useful accepted evidence, but primarily Windows capture UX rather than a new architecture convention.
- !12745 registers the known WS-Discovery UDP port while leaving the broader XML heuristic disabled, reinforcing the existing rule that heuristic recognition should remain selective rather than using a broad heuristic where a well-known binding is available.
- !12714 and release backport !12737 fix several Thrift compact-protocol details, including a heuristic length check intended to avoid false positives/reassembly recursion; the author also reported fuzzing during review. This reinforces existing heuristic-selectivity and validation/testing guidance.
- !12752 reduces Follow Stream copying by relying on Qt implicit sharing/raw-data views until mutation is actually needed. It is a useful localized performance pattern, but not broad enough to promote independently.

## Lower-weight closed/open evidence

- !12747 was a closed/unmerged cherry-pick attempt whose branch contained hundreds of unrelated changes/conflicts. It is useful negative submission evidence that a backport/cherry-pick must be checked for scope and history cleanliness, but it is not accepted implementation evidence.
- !12739 was a closed draft attempting to alter 802.11 protocol-layer accounting. João Valverde rejected the proposed mechanism and asked for a comprehensive feature request explaining the need to non-802.11 experts and including a capture. Useful review-process evidence, but down-weighted because the implementation was not accepted.
- !12719 remained open. Jaap Keuter's review distinguishes packet-tree items that represent packet bytes from synthetic interpretation, suggesting generated/separate presentation for inferred HTTP reason text. Useful design feedback, but not promoted because the MR was not merged.
- !12716 remained open. It proposes epan unit-test infrastructure and contains useful discussion around `epan_init()` and scope lifecycle, but its unresolved/open state makes it unsuitable as authoritative convention evidence for this pass.
- !12713 was closed by its author in favor of combining the BLF Ethernet-status changes elsewhere; it was therefore not treated as accepted implementation evidence.

## Frontier

!12712 (`BLF: Fix BLF_OBJTYPE_ETHERNET_STATUS`) exists in the same corpus commit and is merged. It was inspected only to establish that the corpus continues and **was not reviewed or counted in this run**. Absent newly scraped higher-numbered unreviewed material, it is the next descending candidate.
