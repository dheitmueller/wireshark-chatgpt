# Wiretap Timestamp Metadata Conventions

Merged MRs !5466 and !5465, both authored by John Thacker, fix cases where internal timestamp precision did not agree with serialized pcapng Interface Description Block metadata. Dummy IDBs and BLF now serialize the non-default resolution so later readers interpret timestamp units correctly.

**Rule:** treat Wiretap timestamp precision, units-per-second, and serialized pcapng resolution metadata as one semantic value. Synthetic IDBs are not exempt, and omission is safe only when the format default has exactly the same meaning.

**Testing rule:** round-trip through pcapng and compare interpreted timestamps, not only raw integer fields.

**Confidence:** High; two merged maintained-branch fixes independently enforce the same invariant.

## Timestamp provenance controls precision even through a wrapper format

Merged master MR !3399, authored by Guy Harris, fixes LINKTYPE_ERF files carried in pcap by setting each record's timestamp precision from ERF. The outer pcap packet header did not supply the timestamp; the encapsulated ERF record did. Merged !3401 and !3402 carry the same fix to maintained branches.

The companion merged master !3387, also authored by Guy, sets the precision on a newly created ERF Interface Description Block, with !3388 and !3389 as stable-branch counterparts.

**Implementation rule:** determine timestamp precision from the format layer that actually supplied the timestamp, not merely from the outer container in which the record happened to be transported. Keep per-record precision and generated interface metadata consistent with that provenance.

**Confidence:** Extremely high. Repeated Guy Harris-authored master and stable-branch fixes.

