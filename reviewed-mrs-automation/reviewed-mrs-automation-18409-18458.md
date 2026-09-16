# Automated Wireshark MR review: !18409-!18458

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Review direction: newest toward older. This run reconstructed the already-reviewed set from `reviewed-mrs-automation/` and `reviewed-mrs.md`, preserving the historical !17571-!17620 batch. The next fifty highest-numbered unreviewed corpus entries were !18458 down through !18409 inclusive.

## Exact reviewed MR set

!18458, !18457, !18456, !18455, !18454, !18453, !18452, !18451, !18450, !18449, !18448, !18447, !18446, !18445, !18444, !18443, !18442, !18441, !18440, !18439, !18438, !18437, !18436, !18435, !18434, !18433, !18432, !18431, !18430, !18429, !18428, !18427, !18426, !18425, !18424, !18423, !18422, !18421, !18420, !18419, !18418, !18417, !18416, !18415, !18414, !18413, !18412, !18411, !18410, !18409.

## Durable findings

- **!18458 (merged, John Thacker) — display-filter field identity matters independently of presentation.** WASSP had two fields sharing `wassp.tlv.type`, with the later `FT_NONE` registration making the actual TLV type effectively unfilterable. The merged repair gives the sub-TLV a distinct abbreviation (`wassp.tlv.type.sub`) and a real `FT_UINT16` value while retaining custom display formatting. This corroborates the existing notebook guidance that field abbreviations are API-like identities: separately meaningful/filterable values need distinct registrations, and presentation formatting should not replace typed field semantics.
- **!18420 (merged, John Thacker) — reassembly/state logic should follow protocol invariants, including negative boundaries.** RFC 9293's keep-alive constraints imply that a multi-segment PDU cannot legitimately span a keep-alive boundary. The fix therefore prevents the optional garbage octet in a keep-alive from being appended to an MSP and clears a false unfinished MSP when the keep-alive reveals that the prior byte was misclassified. Review explicitly discussed the unavoidable ambiguity between a one-octet retransmission and a keep-alive with garbage data. This is useful corroboration for basing state-machine recovery on protocol semantics rather than merely local byte patterns.
- **!18410 (merged release-4.4 backport, John Thacker) — parser compatibility sometimes must cover syntax used by the ecosystem even when formal language documentation is incomplete.** The Protobuf parser accepts extension declaration syntax present in Google's well-known files/examples despite its omission from the cited formal grammar, reusing the existing generic option-value productions rather than deeply interpreting content that Wireshark does not need. This is a useful conservative-parser exemplar but not broad enough to add a new notebook rule.
- **!18440 (merged, Gerald Combs) — dependency headers should use their exported include namespace.** Falcodump switches bare `plugin_manager.h`/`scap_engines.h` includes to `libsinsp/plugin_manager.h` and `libscap/scap_engines.h`. This is build-system cleanup and corroborates existing dependency-interface guidance.
- **!18430 (merged, Martin Mathieson) — reuse shared project-level true/false strings instead of protocol-local duplicates.** O-RAN replaces a local `applicable_not_applicable_tfs` with the common `tfs_applicable_not_applicable`. This is straightforward deduplication and does not warrant a new convention.

## Notebook impact

No convention file was changed in this run. The strongest lessons corroborate conventions already represented in the notebook (stable/unique field identity, typed filterable fields, protocol-semantic state handling, reuse of shared helpers/constants, and canonical dependency interfaces) rather than establishing a genuinely new durable rule.

## Weighting

Merged MRs were treated as stronger evidence than closed/abandoned or superseded work. Substantive maintainer discussion was weighted above mechanical approvals and system notes. Backports were treated mainly as corroboration unless they contained distinct review or implementation evidence.

## Continuation

Future backward review should reconstruct the reviewed set again from all tracking files rather than assuming contiguous ranges. Subject to any holes revealed by that reconstruction, the next candidate below this batch is !18408.
