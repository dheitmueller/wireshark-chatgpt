# Automated Wireshark MR review: !19340-!19389

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Selection rule: after consulting `reviewed-mrs.md` and all available files under `reviewed-mrs-automation/` (including preserving/counting the historical !17571-!17620 batch), these were the fifty highest-numbered corpus MRs not already recorded as reviewed. Review direction remains newest toward older. Exactly 50 MRs were reviewed in this run.

## Exact reviewed set

!19389, !19388, !19387, !19386, !19385, !19384, !19383, !19382, !19381, !19380,
!19379, !19378, !19377, !19376, !19375, !19374, !19373, !19372, !19371, !19370,
!19369, !19368, !19367, !19366, !19365, !19364, !19363, !19362, !19361, !19360,
!19359, !19358, !19357, !19356, !19355, !19354, !19353, !19352, !19351, !19350,
!19349, !19348, !19347, !19346, !19345, !19344, !19343, !19342, !19341, !19340.

## Review notes

The batch was reviewed from the corpus JSON, including MR metadata, final diffs, and available discussion. Merged MRs were weighted more heavily than abandoned/superseded work; substantive maintainer review was weighted more heavily than mechanical/system notes.

- **!19357 — Deep.** Merged `pkcs1` → `pkixalgs` rename. Pascal Quantin explicitly required `proto_register_alias()` so existing display filters would not all break. Added a durable compatibility rule: protocol/filter namespaces are user-facing interfaces; preserve old identities with aliases when renaming where possible.
- **!19345 — Deep.** Merged NAS EPS detach-request direction-detection fix. New optional IEs made the old total-length heuristic too weak; the accepted fix checks a mandatory UL structure length as an additional discriminator. Pascal Quantin explicitly confirmed the old heuristic was no longer strong enough and considered the fix worth backporting. Added guidance to prefer mandatory protocol structure over incidental packet-shape thresholds and to revisit heuristics as protocols evolve.
- **!19362 — Discussion-focused.** Merged MKA/MACsec cleanup. Treats names as C strings, uses `ws_strdup()` when formatting is unnecessary, updates UAT defaults so older 4.4 UAT files remain readable, and corrects presentation of unwrapped SAK so it does not falsely reference ICV packet bytes. Useful compatibility/presentation corroboration, but no additional standalone rule was needed.
- **!19370 — Discussion-focused.** Merged multi-part RDP improvements with substantial review iteration. Useful evidence for normal reviewer-driven cleanup and protocol-tree correctness, but no new durable convention beyond existing notebook material.
- **!19341 — Discussion-focused.** Merged EBHSCR expansion. Review caught a signed/unsigned comparison build error; reinforces existing compiler-hygiene and warning-as-error guidance.
- **!19389 — Scanned.** Release-4.4 F1AP backport selecting the appropriate NR RRC UL CCCH dissector based on RRC-container size; merged with successful pipeline and no substantive human review beyond approval.
- **!19388 — Scanned.** Merged S7COMM typo/mask-format cleanup; no additional durable lesson.
- **All remaining MRs in the exact set above — Scanned unless noted above.** Their metadata, purpose, available discussion, and final changes were checked. They were primarily protocol-specific corrections/extensions, backports, generated/registry data, build/UI maintenance, warning cleanup, or changes whose reusable lessons were already represented in the notebook. No additional convention was promoted merely to increase notebook volume.

## Notebook changes

Created `protocol-compatibility-conventions.md` with two durable rules extracted from merged !19357 and !19345:

1. Protocol/filter renames must explicitly preserve or account for user-facing display-filter compatibility; use `proto_register_alias()` where appropriate.
2. Heuristic recognition must evolve with protocol revisions, and mandatory structural discriminators are preferable to incidental length/shape tests when available.

Notebook convention commit: `eddba8979fdb7df668b546ccd1e432305fc6fafc`.
