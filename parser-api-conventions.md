# Wireshark Parser and API Conventions

This file records durable parser/API correctness conventions extracted from upstream Wireshark merge-request review and accepted fixes. Current upstream source remains authoritative; these notes are a curated cache for future implementation and review.

## Public API ownership and lifetime must be explicit

Public helper documentation should state whether returned data is borrowed or newly allocated, how long borrowed storage remains valid, and how owned storage must be released. Do not document two similar accessors so ambiguously that callers can swap their ownership assumptions.

Merged MR !25947, authored and merged by John Thacker, corrected the Doxygen contracts for two data-source APIs: `get_data_source_name()` returns a string valid for the packet-dissection pool lifetime and must not be freed, while `get_data_source_description()` returns newly allocated storage that must be released with `g_free()`. Treat allocation, lifetime, and deallocation as part of the API contract, not incidental implementation detail.

**Confidence:** High. Merged API-documentation correction by a senior maintainer, and the distinction directly affects memory correctness.

## Warning-driven fixes are not automatically semantic fixes

Compiler/static-analysis warnings can reveal real problems, but a warning-driven change to a dissector's field width, mask, enum interpretation, or protocol semantics must still be checked against the protocol's actual runtime representation. Do not widen masks or alter field meaning merely because a declaration appears to permit more values.

In merged MR !25952, Martin Mathieson explicitly asked the ETW domain expert to review a mask change because he was unsure whether all documented enum values could actually appear on the wire/in the structure. The expert explained that some documented types are converted at runtime and cannot appear there, and that the proposed change was wrong in that respect. The MR was revised before merge.

**Review rule:** if a warning fix crosses from C-type hygiene into protocol interpretation, seek the specification/current implementation and, when the semantics remain unclear, domain-expert review before treating the warning as mechanically actionable.

**Confidence:** High. Direct review correction on a merged MR, with the uncertainty explicitly identified before merge.

## `tvb_get_ptr()` length is a verification span from the supplied offset

For `tvb_get_ptr(tvb, offset, length)`, `length` is the number of bytes requested beginning at `offset`; it is not an absolute end offset and should not have `offset` subtracted from it unless the caller is genuinely converting an end position into a length. The returned value is a raw pointer into the tvbuff's backing data; the length argument validates availability, it does not create a separately length-carrying slice.

Merged MR !25954, authored and merged by John Thacker, fixed KNX/IP code that had already computed `size` as the encrypted-data length but then called `tvb_get_ptr(tvb, offset, size - offset)`. The double subtraction could reduce the requested length to zero and yield `NULL` even though the intended encrypted bytes had already been bounds-checked. The correct call is `tvb_get_ptr(tvb, offset, size)` once `size` bytes at that offset have been validated. Release-4.6 backport !25957 preserves the same correction; release-4.4 backport !25958 independently confirms the same semantics.

**Implementation rule:** keep offsets, lengths, and end positions semantically distinct. Once a variable is a length relative to a known offset, pass that length directly to tvbuff APIs that expect a byte count; do not subtract the offset again.

**Confidence:** Very high. Security-motivated merged fix by John Thacker plus accepted release backports.

## Clear transient parser state before decoding that may fail or be empty

When parser callbacks store temporary/current values outside the immediate local expression, reset that state to a safe default before entering a parse that can throw, short-circuit, encounter an empty container, or omit a mandatory-looking element. Clearing the previous value only after successfully parsing the replacement can leave stale data from an earlier element or packet visible to later logic.

Merged MR !25955, authored and merged by John Thacker, moved clearing of CMS `algorithm_id` to the start of `AlgorithmIdentifier` dissection, before sequence processing. The stated rationale is that an exception or empty sequence must not leave the previous digest algorithm in place. The verifier also treats a missing algorithm as "unable to verify" instead of dereferencing/comparing a null pointer. Release backports !25959 and !25961 preserve both the reset-before-parse ordering and the explicit missing-value check.

**Implementation rule:** establish the failure-state value first, then parse and overwrite it only on success. This is especially important in generated/ASN.1 callback code and other parsers where exceptions can bypass ordinary cleanup/assignment paths.

**Confidence:** Very high. Merged defensive parser-state fix by John Thacker with explicit stale-state rationale and two accepted backports.

## Wiretap `caplen` and packet `len` are different semantic quantities

A wiretap reader must keep the number of bytes actually captured/stored separate from the packet's original/on-wire length. Set `caplen` from the captured payload that the reader actually materializes, and set `len` from the format's original packet-length field. Do not assign them based on variable proximity or naming alone; trace each source field's file-format semantics.

Merged MR !25956, authored and merged by John Thacker, fixes RTPDump where these assignments were swapped. After accounting for the exported-PDU header, `caplen` is based on the captured RTPDump record length while `len` is based on the original packet length. The same fix validates that the total record length can include the mandatory 8-byte header using checked subtraction, reporting malformed input as `WTAP_ERR_BAD_FILE` with format-specific context. Release backports !25962 and !25963 corroborate the same rule.

**Implementation rule:** validate mandatory-header subtraction before consuming payload, and preserve the capture-file distinction between stored bytes and original packet size all the way into `wtap_rec` metadata.

**Confidence:** Very high. Merged wiretap correctness/security fix by John Thacker plus accepted release backports.

## Validate fixed-format text length before raw indexing or substring parsing

When a dissector receives a textual parameter whose grammar requires a fixed minimum width, validate that width before passing raw pointers into helpers or indexing fixed offsets. A malformed short parameter should remain dissectable: mark the field with appropriate expert information and stop parsing the fixed subfields rather than reading beyond the supplied bytes.

Merged MR !25976, authored and merged by John Thacker, checks that the first Bluetooth HFP `AT+XAPL` parameter is at least 14 bytes before extracting several fixed-position hexadecimal substrings. Short values receive an expert warning and the parser returns without attempting those fixed-offset reads. Release backports !25981 and !25983 preserve the same guard.

**Implementation rule:** derive the minimum input width from the format grammar, check it once at the boundary where fixed-position parsing begins, and treat failure as malformed protocol input rather than a process-level fault.

**Confidence:** Very high. Security-motivated merged fix by John Thacker with accepted release backports.

## Diagnostic previews must respect the actual buffer length

Debug, expert-info, and logging paths are still parser code and must not assume that a value is as long as it normally ought to be. When displaying a bounded preview of an externally derived byte string, clamp the preview length to the actual available length and add truncation indication only when more bytes truly exist.

Merged MR !25970, authored and merged by John Thacker, replaced repeated direct reads of the first four Kerberos key bytes with a helper that emits up to four bytes and an ellipsis only for longer keys. This avoids out-of-bounds reads if an unexpectedly short key reaches an expert-message path. Release backports !25974 and !25975 corroborate the same pattern.

**Implementation rule:** never exempt formatting-only code from length safety; compute `min(actual_length, preview_limit)` before reading the preview bytes.

**Confidence:** Very high. Security-motivated merged fix by John Thacker plus two accepted release backports.

## Bound explicit-length nested structures with a subset tvb

When a protocol element carries an explicit length for its own body/record, make that length an actual parser boundary rather than continuing to parse against the enclosing tvbuff and merely carrying the length as metadata. Create a subset tvb that represents the element, then use offsets relative to that subset for all nested reads and tree items.

Merged MR !25978, authored and merged by John Thacker, changes LBMR Topic Management Record parsing to construct a `tvb_new_subset_length()` from the retrieved TMR length before reading its type, flags, string, and subsequent fields. This means a declared-short element naturally fails at the element boundary, and a zero-length record cannot silently walk into following data.

**Implementation rule:** for self-delimiting nested structures, encode the protocol boundary in the tvbuff hierarchy. A bounded subset is preferable to repeatedly trusting callers to compare every inner offset against the outer buffer plus a separate declared length.

**Confidence:** Very high. Merged bounds/security fix authored and merged by John Thacker.

## Pairwise or fixed-unit decoders must prove a complete unit exists per iteration

Loops that decode input in units larger than one byte/character must use a loop bound that guarantees every element read by the iteration exists. An inclusive bound is especially suspicious when the loop body accesses `n + 1`, `n + k`, or emits a smaller number of output units than input units.

Merged MR !25992, authored and merged by John Thacker, fixes the 3GPP log wiretap reader's hex decoder from `n <= data_chars` to `n < (data_chars & ~1)`. The corrected bound processes only complete two-character hex pairs and cannot read the second nibble beyond the validated input. Release-4.6 backport !25995 preserves the fix.

**Implementation rule:** derive the iteration limit from complete input units, not merely the raw input count. For pairwise hex decoding, round the usable character count down to an even boundary before reading `n` and `n+1`.

**Confidence:** Very high. Merged memory-safety fix by John Thacker plus accepted backport.

## Width-specific helper APIs must carry the exact width in their type contract

A helper whose semantic operation is explicitly 8/16/32/64-bit should accept a value type of that width unless there is a deliberate documented reason otherwise. This matters particularly when alternate compiler/architecture paths implement the operation with `memcpy()` or `sizeof(value)`: a wider formal parameter can silently change the number of bytes copied even when arithmetic uses only the low bits.

Merged MR !25994, authored and merged by John Thacker, changes `phtoleu16()` on the memcpy-based path from a `uint32_t` value parameter to `uint16_t`. The old signature could cause out-of-bounds reads/writes because the implementation's storage width followed the incorrectly wide value. Release-4.6 backport !25997 additionally documents that the destination needs at least two bytes.

**Implementation rule:** treat integer width as part of the API's memory-safety contract. Audit compiler-specific and endian-specific implementations together; do not assume a correct generic path implies a correct alternate path.

**Confidence:** Very high. Merged security fix authored and merged by John Thacker plus accepted release backport.

## Checksum and endian helpers have their own byte-order contracts

Do not stack byte swaps, `hton*()`/`ntoh*()` conversions, and checksum-library return values by intuition. Trace what byte order the helper consumes and returns and convert exactly once at the boundary that requires a different representation.

Merged MR !25985 fixes UET CRC verification on big-endian hosts after the CRC32C API was misread: an unnecessary `CRC32C_SWAP()` followed by `htonl()` made the result wrong on s390x while appearing correct on little-endian hosts. Removing the redundant conversions restored the API's intended semantics.

**Implementation rule:** when checksum behavior differs by architecture, inspect the checksum API contract before adding host/network conversions. Include big-endian CI/testing evidence where practical for low-level endian helpers.

**Confidence:** High. Merged portability/correctness fix with a concrete big-endian failure mode.