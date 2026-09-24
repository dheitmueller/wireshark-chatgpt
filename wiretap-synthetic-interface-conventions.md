# Synthetic Interface Metadata

Merged MRs !10888 and !10892, both authored and merged by John Thacker, establish a Wiretap rule for pcapng output when the source has per-packet encapsulation but no interface ID. Reuse an existing IDB only when its encapsulation and timestamp precision match the packet. If no compatible IDB exists, derive one from the packet record and emit it before the Enhanced Packet Block that references it.

Implementation rule: missing interface metadata is not the same as unknown packet semantics. When packet metadata is sufficient, synthesize the required output interface rather than forcing a generic default.

Ordering rule: required metadata must appear before records that reference it.

Confidence: Very high.
