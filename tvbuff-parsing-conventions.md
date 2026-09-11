# Wireshark TVBuff Parsing Conventions

This file records durable conventions for parsing packet data through Wireshark's TVBuff APIs. Current upstream source remains authoritative.

## Preserve TVBuff bounds and reassembly semantics instead of bypassing them with raw pointers

When a parser can encounter truncated or fragmented input, prefer TVBuff-aware decoding helpers over extracting a raw pointer and decoding outside the TVBuff abstraction. The TVBuff layer carries captured/reported-length semantics and raises the bounds exceptions that higher-level dissectors use to distinguish malformed data from data that merely needs reassembly.

Merged master MR !23170, authored and merged by John Thacker, is a strong example. HTTP/3 prefixed-integer decoding was changed from raw-pointer access to the TVBuff API specifically so a prefixed integer split across fragments raises `ReportedBoundsError`; the HTTP/3 dissector interprets that as a request for more reassembly. The same fix also stops immediately when a prefixed integer exceeds the supported value range, rather than returning `-1` and feeding that sentinel into offset arithmetic, which had produced an infinite loop. Stable-branch backport !23172 preserved the behavior.

**Implementation rule:** if truncation, fragmentation, captured-vs-reported length, or reassembly is semantically meaningful, keep decoding inside TVBuff-aware APIs. Do not replace those APIs with raw pointer parsing unless you explicitly reproduce their bounds contract. On decode failure, terminate or propagate the failure; never let an error sentinel participate in offset/progress arithmetic.

**Confidence:** Extremely high. Merged master correctness fix authored and merged by John Thacker, with an accepted stable-branch backport and a concrete hang/reassembly failure mode.
