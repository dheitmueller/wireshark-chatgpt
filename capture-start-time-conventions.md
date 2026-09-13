# Wireshark Capture Start-Time Conventions

This file records durable conventions for representing capture/file start time when a capture format provides that information independently of packet timestamps. Current upstream Wiretap source remains authoritative.

## Preserve an explicit file-start timestamp as authoritative per-file state

An explicit start time stored in a capture-file header is a different semantic fact from the timestamp of the first packet or other timestamped record. The first timestamped record may occur later, metadata may precede it, and some records may have no timestamp at all. Code should therefore not reconstruct an authoritative file-start value from record order when the format supplies it directly.

Merged master MR !21333, authored by John Thacker, adds an explicit `file_start_time` to Wiretap's per-file state for formats such as BLF and LOG3GPP whose headers provide the capture start independently of the first timestamped record. Follow-up consumers can calculate relative-to-capture-start presentation from that authoritative file-level value instead of carrying redundant per-record derived state.

**Implementation rule:** when a file format supplies an authoritative session/capture start, store it once at the file/session lifetime and preserve its distinction from the first record timestamp. Derive relative times and other dependent values from the authoritative file-level fact as needed rather than duplicating derived state or inferring the start from packet ordering.

**Review rule:** when changing timestamp presentation or capture statistics, identify which time quantity is actually intended: explicit file start, first timestamped record, first packet, or another event. Do not treat those concepts as interchangeable merely because they often coincide in common captures.

**Confidence:** Very high. Merged master Wiretap state-modeling change authored by John Thacker and accepted upstream.
