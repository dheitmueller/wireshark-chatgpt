# Automated MR review ledger: !18859–!18908

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Review direction: descending from newest toward older MRs. This run rebuilt the already-reviewed set from the available per-run ledgers plus `reviewed-mrs.md`, preserving and counting the historical !17571–!17620 batch. The fifty highest-numbered corpus MRs not already present in that tracking set were selected. No numeric range was assumed reviewed merely from a ledger filename.

## Exact reviewed set

!18908, !18907, !18906, !18905, !18904, !18903, !18902, !18901, !18900, !18899,
!18898, !18897, !18896, !18895, !18894, !18893, !18892, !18891, !18890, !18889,
!18888, !18887, !18886, !18885, !18884, !18883, !18882, !18881, !18880, !18879,
!18878, !18877, !18876, !18875, !18874, !18873, !18872, !18871, !18870, !18869,
!18868, !18867, !18866, !18865, !18864, !18863, !18862, !18861, !18860, !18859.

Count: **50**.

## Durable findings

### !18907 — bulk dynamic-field deregistration and namespace ownership

Merged master MR by Lars Völker. `proto_deregister_field()` previously searched all fields belonging to a protocol for each removal, making large dynamically generated field sets very expensive to rebuild. The accepted implementation adds a bulk/prefix-oriented path that makes removal linear rather than repeatedly scanning the parent field set. The author reported profile-switch time for large configurations falling from roughly 10 seconds to 1 second.

The review discussion raised an important compatibility issue: Signal PDU field abbreviations changed from `signal_pdu.*` to the narrower `signal_pdu.signals.*`. The author explained that this was required because prefix-based deregistration needs an exclusive ownership namespace; otherwise unrelated fields could be deregistered. The discussion additionally records that dynamically generated names could collide with and overwrite existing filters, potentially causing crashes, with SOME/IP having the same class of problem.

Durable rule extracted to `field-deregistration-lifecycle-conventions.md`: dynamically registered fields that are rebuilt/deregistered as a family should have an exclusive display-filter namespace matching their lifecycle ownership boundary. Prefix-based bulk teardown must not select unrelated static or independently owned fields. Review compatibility impact when changing abbreviations, but do not preserve an unsafe shared namespace merely for compatibility.

### !18908 — dependency/header ABI matching

Merged John Thacker minizip-ng compatibility fix. The implementation documents that selecting a compression header merely because it exists on the build host is insufficient: the included zlib/zlib-ng declarations must match the ABI/configuration against which minizip was built. This is useful build-system evidence for dependency pairing and feature detection, but is sufficiently library-specific that no new notebook convention was added.

### !18859 — UAT teardown/state reset hardening

Merged LIN follow-up to !18401. The fix restores NULL checks and resets UAT-related state to prevent dangling pointers after configuration lifecycle changes. This corroborates existing state/lifetime guidance: configuration objects that can be destroyed and recreated must not leave cached pointers live across teardown/reload boundaries.

## Notebook changes

Updated `field-deregistration-lifecycle-conventions.md` with the !18907 exclusive-namespace rule and its compatibility/review implications.

Notebook convention update commit: `0294f8e61e958091fc21a8623a7b5cd602a28b80`.

## Continuation

Future runs must rebuild the reviewed set from individual entries. Subject to any higher-numbered holes discovered in tracking, the descending frontier after this run is below !18859.
