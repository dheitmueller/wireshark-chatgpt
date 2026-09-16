# Automated Wireshark MR review: !18459–!18508

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Review direction: descending MR number. Existing per-run tracking and `reviewed-mrs.md` were consulted first; the historical !17571–!17620 batch remains part of the reviewed set. This run selected the fifty highest-numbered corpus MRs not already represented by the tracking set.

## Exact reviewed MR set

!18508, !18507, !18506, !18505, !18504, !18503, !18502, !18501, !18500, !18499, !18498, !18497, !18496, !18495, !18494, !18493, !18492, !18491, !18490, !18489, !18488, !18487, !18486, !18485, !18484, !18483, !18482, !18481, !18480, !18479, !18478, !18477, !18476, !18475, !18474, !18473, !18472, !18471, !18470, !18469, !18468, !18467, !18466, !18465, !18464, !18463, !18462, !18461, !18460, !18459.

Count: **50**.

## Review notes

The batch was reviewed with merged changes weighted above abandoned/superseded work and with maintainer-authored/maintainer-reviewed changes given additional authority. Most items were small protocol corrections, generated-field cleanup, dependency/build maintenance, or backports and did not add a durable convention beyond rules already present in the notebook.

Notable examples:

- **!18508 (merged, Anders Broman):** SBC-AP ASN.1 generated-field cleanup separates the CHOICE selector field from the actual gNB-ID value field, eliminating duplicate/ambiguous generated fields. Useful corroboration for keeping protocol-field identity semantically distinct even when generated code initially produces overlapping names.
- **!18499 (merged, John Thacker):** fixes incorrect Diameter enumerations against authoritative protocol/IANA sources. This is a straightforward exemplar of correcting value tables from normative registries rather than preserving historical-but-wrong dissector values.
- **!18490 (merged):** Bluetooth Channel Sounding mode-3 parsing passes the *remaining* step-data length after mode-1 consumption into the mode-2 parser. The prior use of the enclosing length caused over-parsing that corrupted subsequent steps. This reinforces the existing convention that nested parser lengths must describe the exact remaining semantic payload, not the original enclosing object's length.
- **!18480 (merged, Anders Broman; merged by John Thacker):** generated XML DTD header fields gain a distinct suffix to prevent duplicate field registrations. This corroborates the existing field-namespace/ownership guidance: independently generated field families must have identities that cannot collide with another family.
- **!18470 (merged, Anders Broman):** removes an alias that produced a duplicate numeric `value_string` entry. This is useful validation hygiene but does not warrant a separate convention.
- **!18460 (merged; John Thacker review/merge):** cleanup-only ICMPv6 changes were explicitly *not* considered worth immediate backporting merely for branch consistency. John Thacker's review distinguishes bug/functionality backports from cleanup churn: cleanup can wait until a functional fix requires touching the release branch. This is durable release-branch guidance, but is already consistent with the notebook's conservative backport/scope discipline.

## Notebook update decision

No convention file was changed in this run. The strongest findings corroborate existing rules on exact nested lengths, unique field namespaces, authoritative protocol registries, scope discipline, and conservative release-branch backports rather than establishing a new convention strongly enough to justify additional prose.

## Continuation

For the next run, rebuild the reviewed set from all tracking files again rather than assuming this numeric range or any other range is complete. Continue backward and select the fifty highest-numbered corpus MRs absent from that rebuilt set. If no tracking gaps exist above this batch, the natural continuation is below !18459.
