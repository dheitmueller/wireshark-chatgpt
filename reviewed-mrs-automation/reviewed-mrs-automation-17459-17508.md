# Wireshark MR automation review: !17459-!17508

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Review direction: descending from newest toward older MRs.

Selection method: rebuilt the already-reviewed set from `reviewed-mrs.md` plus the per-run files in `reviewed-mrs-automation/`, explicitly preserving/counting the historical !17571-!17620 batch. The fifty highest-numbered corpus MRs not already represented were !17508 through !17459 inclusive.

## Exact reviewed set (50)

!17508, !17507, !17506, !17505, !17504, !17503, !17502, !17501, !17500, !17499,
!17498, !17497, !17496, !17495, !17494, !17493, !17492, !17491, !17490, !17489,
!17488, !17487, !17486, !17485, !17484, !17483, !17482, !17481, !17480, !17479,
!17478, !17477, !17476, !17475, !17474, !17473, !17472, !17471, !17470, !17469,
!17468, !17467, !17466, !17465, !17464, !17463, !17462, !17461, !17460, !17459.

## Review notes

Merged MRs and substantive human review were weighted more strongly than closed, abandoned, superseded, mechanical, or backport-only material.

- **!17508 — closed, no diff:** a transient revert of a test commit with no discussion or surviving implementation. No engineering guidance extracted.
- **!17507 — merged:** fixes the NFAPI CRC indication `true_false_string`: protocol value 0 means pass and 1 means fail, while the Wireshark/C `true_false_string` representation lists the true label first. Martin Mathieson explicitly noted that this ordering is easy to misread. Useful local correctness example, but not enough to justify a new notebook convention beyond existing semantic-value validation guidance.
- **!17493 — closed after architectural discussion:** Martin Mathieson proposed suppressing VLAN expert warnings when dissecting a file as a protocol. Guy Harris identified the deeper Fileshark limitation: the current model presents an entire file as one record rather than independently dissecting each file record, which can trip limits and make tree construction expensive. Martin closed the workaround rather than upstreaming a special-case suppression. This is useful high-authority negative design evidence: avoid papering over an architectural representation problem with protocol-specific warning suppression. Because the MR was closed and the underlying Fileshark architecture was unresolved, this remains contextual evidence rather than a project-wide convention.

The remainder of the batch was scanned for state, purpose, changes, and substantive discussion. Merged implementation evidence was preferred; mechanical updates, backports, abandoned work, and discussion-free fixes were not promoted into notebook rules without independent durable evidence.

## Notebook result

No existing convention file required a change from this batch. The strongest lesson (!17493) comes from a closed workaround and describes an unresolved Fileshark architectural limitation, so recording it here is more appropriate than promoting it to a general Wireshark coding rule. !17507 reinforces existing practice to verify protocol-value semantics carefully when mapping them into generic helper representations.

## Continuation

Rebuild the reviewed set from all tracking before the next run rather than assuming contiguous coverage. Subject to that rebuild, the next descending candidate is !17458.
