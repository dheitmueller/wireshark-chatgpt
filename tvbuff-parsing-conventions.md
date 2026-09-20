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

## Do not equate a non-NULL TVBuff with the presence of backing bytes

A TVBuff can be a valid object representing an empty result. In that state, the TVBuff pointer itself is non-NULL while its reported length is zero and there may be no backing data pointer to return. APIs that require an addressable byte range must therefore be gated by the TVBuff's semantic length/availability, not merely by object existence.

Merged master MR !16192 fixes the packet-bytes dialog's decompression path after valid empty decompression results could produce a non-NULL TVBuff with zero reported length and `real_data == NULL`. The old code tested only `uncompr_tvb` before calling `tvb_get_ptr()` and could hit an assertion; the accepted code additionally requires `tvb_reported_length(uncompr_tvb) > 0` before asking for raw bytes.

**Implementation rule:** when an operation can legitimately produce an empty TVBuff, treat object validity and byte availability as separate predicates. Check the appropriate length/availability condition before calling pointer-returning or byte-reading APIs that require at least one byte.

**Review rule:** nullable-object checks are not a substitute for empty-data checks. Audit decompression, transformation, subset, and synthetic-TVBuff paths especially, because a successful transformation may validly yield an empty buffer.

**Confidence:** High. Merged master crash/assertion fix with a concrete valid-empty TVBuff state.
