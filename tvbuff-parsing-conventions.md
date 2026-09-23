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

## Do not use protocol-declared maxima as memory-safety bounds for untrusted captures

A specification may say that a token, parameter name, or other string has a maximum length, but a captured packet is untrusted input and can violate that rule. A fixed local buffer sized to the protocol maximum is therefore not safe unless the parser independently enforces the same bound before every write. Where possible, avoid the copy and scan/compare directly through bounded TVBuff operations.

Merged master MR !13441, authored and merged by John Thacker, fixes an MGCP buffer overrun in vendor-extension parameter parsing. The old code copied alphanumeric packet bytes into a 256-byte stack buffer on the assumption that the specification's 256-octet limit would hold. Fuzzed input did not honor that assumption. The accepted change removes the fixed buffer, scans only within the current TVBuff-backed parameter length, and uses TVBuff string comparison for the recognized extension.

**Implementation rule:** treat protocol maxima as validity constraints, not as proof about hostile capture contents. If a specification maximum matters, validate it explicitly and report malformed data; do not let an unvalidated wire length control writes into a fixed-size local array.

**API rule:** when the desired operation is search, classification, or comparison over packet data, prefer bounded TVBuff helpers to copying the bytes into temporary C strings. This preserves capture bounds and removes a second buffer-size invariant.

**Review/testing rule:** fuzz and test values beyond the protocol-declared maximum as well as boundary-valid values. In review, flag fixed-size packet-derived character buffers whose only safety argument is a statement in the protocol specification.

**Confidence:** Very high. Merged master memory-safety fix authored and merged by John Thacker, with the invalid specification-based assumption and TVBuff-native replacement stated directly.

## Do not equate a non-NULL TVBuff with the presence of backing bytes

A TVBuff can be a valid object representing an empty result. In that state, the TVBuff pointer itself is non-NULL while its reported length is zero and there may be no backing data pointer to return. APIs that require an addressable byte range must therefore be gated by the TVBuff's semantic length/availability, not merely by object existence.

Merged master MR !16192 fixes the packet-bytes dialog's decompression path after valid empty decompression results could produce a non-NULL TVBuff with zero reported length and `real_data == NULL`. The old code tested only `uncompr_tvb` before calling `tvb_get_ptr()` and could hit an assertion; the accepted code additionally requires `tvb_reported_length(uncompr_tvb) > 0` before asking for raw bytes.

**Implementation rule:** when an operation can legitimately produce an empty TVBuff, treat object validity and byte availability as separate predicates. Check the appropriate length/availability condition before calling pointer-returning or byte-reading APIs that require at least one byte.

**Review rule:** nullable-object checks are not a substitute for empty-data checks. Audit decompression, transformation, subset, and synthetic-TVBuff paths especially, because a successful transformation may validly yield an empty buffer.

**Confidence:** High. Merged master crash/assertion fix with a concrete valid-empty TVBuff state.

## A subset's captured length must never exceed its reported length

Captured length describes bytes actually present in the capture, while reported length describes the logical packet extent. For a TVBuff subset, allowing captured length to exceed reported length produces an internally contradictory object and can trigger incorrect exception behavior in downstream dissectors.

Merged master MR !11864, authored by John Thacker, centralizes this invariant in `tvb_new_subset_length_caplen()`: the implementation clamps the subset's captured length to its reported length. Its API documentation also makes the intended hierarchy explicit: callers should normally use `tvb_new_subset_length()` or `tvb_new_subset_remaining()`. The explicit-caplen form is for the less common case where trailing bytes in the backing TVBuff (for example an FCS or padding) must be excluded independently of the subset's reported length.

Merged follow-up !11912 then replaces dozens of obvious `tvb_new_subset_length_caplen()` call sites with the simpler length/remaining variants. The MR notes that some old calls could throw exceptions, including the wrong exception, for snaplen-truncated captures because they had forced captured length to equal the logical requested length.

**Implementation rule:** do not manufacture a captured length from a protocol-declared or reported length. Let the TVBuff layer derive the available captured extent whenever possible, and use `tvb_new_subset_length()` / `tvb_new_subset_remaining()` for ordinary protocol subranges. Use an explicit captured-length subset only when the backing buffer contains captured bytes that semantically must be excluded.

**Review rule:** when a subset call supplies the same value for reported and captured length, ask whether the caller is incorrectly assuming the full logical payload was captured. Test truncated captures so the resulting bounds exception reflects the actual missing-data condition rather than an artificial subset length.

**Confidence:** Very high. Both changes are merged master work authored by John Thacker; !11864 establishes the core invariant and !11912 applies it broadly across dissectors.