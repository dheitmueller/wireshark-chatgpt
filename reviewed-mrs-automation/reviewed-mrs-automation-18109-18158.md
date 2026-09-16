# Automated Wireshark MR review: !18109-!18158

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Review direction: descending MR number (newest toward older). This run rebuilt the already-reviewed set from the available `reviewed-mrs-automation/` ledgers and `reviewed-mrs.md`, preserving the historical !17571-!17620 batch. The fifty highest-numbered corpus MRs not in that set were !18158 through !18109.

## Exact MRs reviewed

!18158, !18157, !18156, !18155, !18154, !18153, !18152, !18151, !18150, !18149, !18148, !18147, !18146, !18145, !18144, !18143, !18142, !18141, !18140, !18139, !18138, !18137, !18136, !18135, !18134, !18133, !18132, !18131, !18130, !18129, !18128, !18127, !18126, !18125, !18124, !18123, !18122, !18121, !18120, !18119, !18118, !18117, !18116, !18115, !18114, !18113, !18112, !18111, !18110, !18109.

Count: 50.

## Durable findings / weighting

- **!18120 + !18130 (merged; John Thacker; Stig Bjørlykke regression report):** I/O Graph UAT compatibility is a strong persisted-schema lesson. The new AOT field had to be appended to the serialized UAT description/defaults while its desired UI location was achieved separately with Qt visual section movement. Stig then found a post-merge failure caused by a parallel column-order enum, and John supplied merged !18130. Added `persisted-configuration-compatibility-conventions.md`: positional persisted schemas are compatibility-sensitive; decouple stored and visual ordering; audit parallel enums/defaults/index mappings; and test loading prior-version configuration plus the normal UI path.
- **!18150 -> !18151:** !18150 was closed unmerged and immediately replaced by merged !18151 for the ML-KEM/IKEv2 value additions. Treat !18151 as implementation authority and !18150 only as supersession context.
- **!18157 (merged):** Kafka JoinGroup fields are gated by the API versions in which they actually exist (`join_reason` >= 8, `skip_assignment` >= 9). This corroborates existing protocol-version boundary guidance; no new notebook rule required.
- **!18158 (merged):** Stratoshark/sshdig documentation cleanup; no new durable engineering convention.
- Remaining MRs in the batch were scanned for state, purpose, discussion, and diff relevance. Merged MRs were weighted above closed/superseded work. No additional lesson was strong enough, relative to existing notebook guidance, to justify another convention update.

## Continuation

On the same corpus snapshot, continue by rebuilding the reviewed set from all ledgers rather than assuming ranges. If no higher-numbered holes exist, the next natural descending candidates begin at !18108. Review no more than fifty in one run.
