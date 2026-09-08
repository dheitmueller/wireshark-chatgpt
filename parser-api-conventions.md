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

Merged MR !25954, authored and merged by John Thacker, fixed KNX/IP code that had already computed `size` as the encrypted-data length but then called `tvb_get_ptr(tvb, offset, size - offset)`. The double subtraction could reduce the requested length to zero and yield `NULL` even though the intended encrypted bytes had already been bounds-checked. The correct call is `tvb_get_ptr(tvb, offset, size)` once `size` bytes at that offset have been validated. Release-4.6 backport !25957 preserves the same correction.

**Implementation rule:** keep offsets, lengths, and end positions semantically distinct. Once a variable is a length relative to a known offset, pass that length directly to tvbuff APIs that expect a byte count; do not subtract the offset again.

**Confidence:** Very high. Security-motivated merged fix by John Thacker plus an accepted release backport.

## Clear transient parser state before decoding that may fail or be empty

When parser callbacks store temporary/current values outside the immediate local expression, reset that state to a safe default before entering a parse that can throw, short-circuit, encounter an empty container, or omit a mandatory-looking element. Clearing the previous value only after successfully parsing the replacement can leave stale data from an earlier element or packet visible to later logic.

Merged MR !25955, authored and merged by John Thacker, moved clearing of CMS `algorithm_id` to the start of `AlgorithmIdentifier` dissection, before sequence processing. The stated rationale is that an exception or empty sequence must not leave the previous digest algorithm in place. The verifier also treats a missing algorithm as "unable to verify" instead of dereferencing/comparing a null pointer.

**Implementation rule:** establish the failure-state value first, then parse and overwrite it only on success. This is especially important in generated/ASN.1 callback code and other parsers where exceptions can bypass ordinary cleanup/assignment paths.

**Confidence:** Very high. Merged defensive parser-state fix by John Thacker with explicit stale-state rationale.

## Wiretap `caplen` and packet `len` are different semantic quantities

A wiretap reader must keep the number of bytes actually captured/stored separate from the packet's original/on-wire length. Set `caplen` from the captured payload that the reader actually materializes, and set `len` from the format's original packet-length field. Do not assign them based on variable proximity or naming alone; trace each source field's file-format semantics.

Merged MR !25956, authored and merged by John Thacker, fixes RTPDump where these assignments were swapped. After accounting for the exported-PDU header, `caplen` is based on the captured RTPDump record length while `len` is based on the original packet length. The same fix validates that the total record length can include the mandatory 8-byte header using checked subtraction, reporting malformed input as `WTAP_ERR_BAD_FILE` with format-specific context.

**Implementation rule:** validate mandatory-header subtraction before consuming payload, and preserve the capture-file distinction between stored bytes and original packet size all the way into `wtap_rec` metadata.

**Confidence:** Very high. Merged wiretap correctness/security fix by John Thacker and consistent with Wireshark's established wiretap error taxonomy.
