# Wireshark TVBuff Parsing Conventions

This file records durable conventions for parsing packet data through Wireshark's TVBuff APIs. Current upstream source remains authoritative.

## Preserve TVBuff bounds and reassembly semantics instead of bypassing them with raw pointers

When a parser can encounter truncated or fragmented input, prefer TVBuff-aware decoding helpers over extracting a raw pointer and decoding outside the TVBuff abstraction. The TVBuff layer carries captured/reported-length semantics and raises the bounds exceptions that higher-level dissectors use to distinguish malformed data from data that merely needs reassembly.

Merged master MR !23170, authored and merged by John Thacker, is a strong example. HTTP/3 prefixed-integer decoding was changed from raw-pointer access to the TVBuff API specifically so a prefixed integer split across fragments raises `ReportedBoundsError`; the HTTP/3 dissector interprets that as a request for more reassembly. The same fix also stops immediately when a prefixed integer exceeds the supported value range, rather than returning `-1` and feeding that sentinel into offset arithmetic, which had produced an infinite loop. Stable-branch backport !23172 preserved the behavior.

**Implementation rule:** if truncation, fragmentation, captured-vs-reported length, or reassembly is semantically meaningful, keep decoding inside TVBuff-aware APIs. Do not replace those APIs with raw pointer parsing unless you explicitly reproduce their bounds contract. On decode failure, terminate or propagate the failure; never let an error sentinel participate in offset/progress arithmetic.

**Confidence:** Extremely high. Merged master correctness fix authored and merged by John Thacker, with an accepted stable-branch backport and a concrete hang/reassembly failure mode.

## Keep every raw TVBuff pointer coupled to an explicit captured-length bound

`tvb_get_ptr()` removes the TVBuff abstraction and returns only a bare pointer. Once code crosses that boundary, the pointer itself carries no safe extent, so a caller must also know the exact captured length it may access. Passing `-1` as the requested length obscures that contract and invites raw-buffer helpers to scan beyond the data the caller actually owns.

Merged master MR !23078, authored by John Thacker and merged by Anders Broman, removes the remaining `tvb_get_ptr(..., -1)` uses. The MR explicitly notes that callers of a bare pointer always need the captured length for safety and that most affected callers already retrieved that length alongside the pointer. Closely related merged MR !23076 replaces SOCKS' `tvb_get_ptr()` plus raw `find_line_end()` combination with `tvb_find_line_end()`, keeping the search within TVBuff-aware bounds while preserving the old tree presentation.

**Implementation rule:** when a raw packet pointer is genuinely required, obtain and carry an explicit captured-length bound with it and pass that bound to every raw-buffer consumer. Prefer a TVBuff-aware search/parse helper when one exists, because it keeps the extent and exception semantics attached to the data rather than splitting them across a pointer and separate bookkeeping.

**Review rule:** treat `tvb_get_ptr(..., -1)` and raw scans over TVBuff-derived pointers as red flags. Verify not only that the starting offset is valid, but that every downstream operation is bounded by captured data.

**Confidence:** Very high. Merged master API-safety cleanup authored by John Thacker, independently reinforced by the accepted SOCKS conversion to a TVBuff-native helper.
