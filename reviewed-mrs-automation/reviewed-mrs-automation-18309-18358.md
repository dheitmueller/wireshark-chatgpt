# Wireshark MR review automation: !18309–!18358

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Review direction: descending from newest toward older MRs. Selection was made after consulting the existing `reviewed-mrs-automation/` ledgers and `reviewed-mrs.md`; the historical !17571–!17620 batch remains part of the already-reviewed set. This run reviewed no more than 50 MRs.

## Exact reviewed set

!18358, !18357, !18356, !18355, !18354, !18353, !18352, !18351, !18350, !18349, !18348, !18347, !18346, !18345, !18344, !18343, !18342, !18341, !18340, !18339, !18338, !18337, !18336, !18335, !18334, !18333, !18332, !18331, !18330, !18329, !18328, !18327, !18326, !18325, !18324, !18323, !18322, !18321, !18320, !18319, !18318, !18317, !18316, !18315, !18314, !18313, !18312, !18311, !18310, !18309

## Review notes

Merged MRs were weighted above closed/abandoned/superseded work. Maintainer-authored and maintainer-reviewed changes were treated as stronger evidence than incidental implementation choices.

- !18358 (merged): PTP Management Path Trace bug fix. Corrects list parsing and was explicitly identified for release-4.4 backport. Useful corroboration for keeping maintenance fixes aligned across supported branches when the defect applies there.
- !18350 (merged, John Thacker): extends ethers-file name resolution from EUI-48 to EUI-64 and updates documentation together with parser/data structures. Good example of carrying a representation expansion through parsing, storage, lookup, and user documentation rather than changing only the immediate parser.
- !18340 (merged): IEEE 802.11 EHT 320 MHz-2 support. Uses an explicit semantic mapping to the existing 320 MHz-1 rate-table index rather than allowing the encoded enum value to accidentally index an unrelated table slot. Corroborates separating wire encodings from internal lookup-table indexing.
- !18330 (merged): Kerberos NTSTATUS interpretation accepts the additional observed flag combination. Narrow protocol correction; no broader convention extracted.
- !18320 (merged): Qt advanced-preferences filtering fix. Corrects interaction between the `show changed only` mode and an empty text search. UI-specific; no new durable engineering convention.
- !18310 (merged, Martin Mathieson): eCPRI concatenation handling with O-RAN FH CUS traffic. A stricter length check exposed an LAA bug; the fix makes concatenation framing explicit and ensures subdissection respects each eCPRI message's payload extent. Strong corroboration that tightening parser bounds can expose latent callers that relied on overly broad tvbuffs, and those callers should be fixed rather than weakening the boundary check.
- !18309 (merged release-4.4 backport, Pascal Quantin): LTE RRC AbsoluteTimeInfo uses the big-endian BCD helper and keeps the ASN.1 source configuration and generated dissector output synchronized. Corroborates the existing generated-code rule: change the generator/source configuration as authoritative input, with generated output matching it.

The remaining MRs in this exact set were reviewed/scanned for purpose, outcome, diff shape, and substantive human discussion. None supplied a sufficiently distinct, durable convention to justify adding another notebook rule in this run.

## Notebook impact

No convention file was changed. The strongest lessons in this batch corroborate existing guidance on bounded subdissection, generated-code source-of-truth discipline, representation/semantic separation, and complete propagation of data-model changes.

## Continuation

For the next run, rebuild the already-reviewed set from all tracking files again and choose the 50 highest-numbered corpus MRs not in that set. If no holes exist above this batch, the natural continuation is below !18309.