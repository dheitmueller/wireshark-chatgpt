# Wiretap Timestamp Metadata Conventions

Merged MRs !5466 and !5465, both authored by John Thacker, fix cases where internal timestamp precision did not agree with serialized pcapng Interface Description Block metadata. Dummy IDBs and BLF now serialize the non-default resolution so later readers interpret timestamp units correctly.

**Rule:** treat Wiretap timestamp precision, units-per-second, and serialized pcapng resolution metadata as one semantic value. Synthetic IDBs are not exempt, and omission is safe only when the format default has exactly the same meaning.

**Testing rule:** round-trip through pcapng and compare interpreted timestamps, not only raw integer fields.

**Confidence:** High; two merged maintained-branch fixes independently enforce the same invariant.
