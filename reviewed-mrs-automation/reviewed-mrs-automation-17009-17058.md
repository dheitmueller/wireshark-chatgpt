# Automated MR review batch: !17009–!17058

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Direction: descending from newest available unreviewed MRs toward older MRs.

Previously reviewed tracking was consulted before selection, including `reviewed-mrs.md` and the per-run files in `reviewed-mrs-automation/`. The historical !17571–!17620 batch remains preserved and counted.

## Exact reviewed MR set

!17058, !17057, !17056, !17055, !17054, !17053, !17052, !17051, !17050, !17049, !17048, !17047, !17046, !17045, !17044, !17043, !17042, !17041, !17040, !17039, !17038, !17037, !17036, !17035, !17034, !17033, !17032, !17031, !17030, !17029, !17028, !17027, !17026, !17025, !17024, !17023, !17022, !17021, !17020, !17019, !17018, !17017, !17016, !17015, !17014, !17013, !17012, !17011, !17010, !17009

Count: **50**

## Review notes

Merged master MRs and substantive maintainer discussion were weighted above release backports, abandoned drafts, and mechanical/superseded changes.

Notable evidence in this batch includes:

- !17030 (merged master, Guy Harris): CommView NCFX open handling distinguishes a malformed candidate that merely means “not my format” from a true hard I/O/open error. It also frees the provisional error string before returning `WTAP_OPEN_NOT_MINE`. This is strong wiretap evidence that format probes must distinguish recognition failure from actual read/open failure and must clean up error state when downgrading a candidate-format parse failure to “not mine”.
- !17009 (merged master, Gerald Combs): 802.11be radio pseudo-header handling enforces the fixed four-user storage capacity before indexing the user array and emits expert information when the advertised count exceeds available slots. !17010 is its release-4.4 backport and therefore carries less independent weight. This reinforces existing fixed-capacity/bounds-check guidance.
- !17050 (release-4.4 backport): display-filter IP-address plugin semantic checking resolves unparsed parameters to the appropriate concrete IPv4/IPv6 field types and treats IPv4-only and IPv6-only functions distinctly. As a backport it is weighted below the originating master change.
- !17040 (merged master): corrects YMSG command/status values after protocol/binary investigation. Useful protocol-correctness evidence but no durable review-process rule beyond validating enumerations against authoritative or empirical evidence.
- !17020 is a release-4.2 backport of Ixia pcap/lcap timestamp-resolution and cleanup work; it was weighted below the master change.
- !17058 is a release-4.2 cherry-pick correcting the GCC `__GNUC_PATCHLEVEL__` macro spelling. Although authored/merged by Guy Harris, it is mechanical backport evidence rather than a new architectural convention.

No separate convention file was modified in this run. The strongest durable observation, !17030's format-probe distinction between `WTAP_OPEN_NOT_MINE` and hard errors, is consistent with existing defensive wiretap/parser guidance and does not justify duplicating the rule.

## Continuation

Rebuild the already-reviewed set from all tracking before the next run. If no higher-numbered gap is found, the next descending candidate is !17008.
