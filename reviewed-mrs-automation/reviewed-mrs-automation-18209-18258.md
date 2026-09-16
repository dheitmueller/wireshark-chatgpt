# Wireshark MR review automation: !18209-!18258

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Reviewed exactly these 50 MRs, selected as the highest-numbered corpus MRs not present in existing review tracking:

!18258, !18257, !18256, !18255, !18254, !18253, !18252, !18251, !18250, !18249, !18248, !18247, !18246, !18245, !18244, !18243, !18242, !18241, !18240, !18239, !18238, !18237, !18236, !18235, !18234, !18233, !18232, !18231, !18230, !18229, !18228, !18227, !18226, !18225, !18224, !18223, !18222, !18221, !18220, !18219, !18218, !18217, !18216, !18215, !18214, !18213, !18212, !18211, !18210, !18209.

The historical !17571-!17620 batch remains reviewed and counts toward the persistent reviewed set.

## Weighting and findings

Merged MRs were weighted above closed/abandoned/superseded submissions. !18258 is closed and was later superseded by merged !18343; its useful review evidence is submission hygiene rather than implementation authority. Alexis La Goutte requested a component-prefixed commit message; the contributor rebased and changed it, but the MR was ultimately resubmitted from a non-`master` source branch as !18343. This corroborates the notebook's existing commit-message and clean topic-branch guidance.

!18220 (John Thacker, merged) is the strongest architectural item in this batch. Display-filter code cannot assume that `header_field_info.strings` has value-string semantics merely because a field is integer-compatible: `FT_FRAMENUM` overloads that member for a different semantic type. It also documents a broader issue when multiple fields share one abbreviation: an optimization based on one registered field's value-string mapping is valid only if the mapping is semantically compatible across all fields under that abbreviation. This corroborates existing notebook guidance treating display-filter abbreviations and field registrations as API-like semantic identities; no new convention file was necessary.

!18240 (Stig Bjørlykke, merged) is a focused protocol-correctness fix changing a NAS EPS optional TLV discriminator from 0x66 to 0x67 to match the specification/comment. Useful as accepted correctness evidence, but no new durable convention beyond existing wire-value/spec verification guidance.

!18210 (Stig Bjørlykke, merged) corrects a GSM A display-filter abbreviation for uncertainty radius. This independently reinforces the existing rule that display-filter abbreviations are user-visible API identities and should describe the semantic field accurately.

!18209 (Gerald Combs, merged by John Thacker) improves AUTHORS sorting using Unicode Collation Algorithm support when available and a locale-aware fallback otherwise. This is tooling-specific and does not justify a new general Wireshark convention.

## Notebook update decision

No convention file was changed in this run. The strongest durable lessons in the batch corroborate conventions already captured by later-reviewed MRs: display-filter abbreviation/field semantic identity, commit-message/topic-branch hygiene, and verification of protocol discriminators against authoritative definitions. Adding duplicate prose would reduce notebook signal.

Next review should rebuild the reviewed set from all tracking and continue with the highest-numbered corpus MRs below this batch that are not already explicitly reviewed; do not infer completeness solely from this numeric range.
