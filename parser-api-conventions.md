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

Merged MR !25992, authored and merged by John Thacker, fixes the 3GPP log wiretap reader's hex decoder from `n <= data_chars` to `n < (data_chars & ~1)`. The corrected bound processes only complete two-character hex pairs and cannot read the second nibble beyond the validated input. Release-4.6 backport !25995 preserves the fix; release-4.4 backport !26000 supplies additional corroboration.

**Implementation rule:** derive the iteration limit from complete input units, not merely the raw input count. For pairwise hex decoding, round the usable character count down to an even boundary before reading `n` and `n+1`.

**Confidence:** Very high. Merged memory-safety fix by John Thacker plus accepted backports.

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

## Keep mutable dissector state in the narrowest semantic lifetime

Mutable state that belongs to one packet must not live in process-global/static storage merely because several generated callbacks need to share it. Merged MR !25998, authored and merged by John Thacker, moves CMS `top_tree` and `cap_tree` pointers into the packet's CMS private data. The stated failure mode was stale proto-tree reuse in a later frame for unusual non-conforming captures. Release backports !26001 and !26002 preserve the same design.

**Implementation rule:** choose packet-, conversation-, capture-, or process-lifetime storage from the semantics of the state. In particular, tree/item pointers and current-element parser context are normally packet-local; storing them globally creates redissection and cross-frame hazards.

**Confidence:** Very high. Security-motivated master fix authored and merged by John Thacker plus two accepted backports.

## Stateful lookup keys must be stable across both directions when the state is bidirectional

A field visible on every packet is not necessarily a valid request/response lookup key. If its value denotes the destination or another direction-specific endpoint, opposite directions can legitimately carry different values for the same connection.

Merged MR !26003 fixes OBEX-over-L2CAP request/response association by replacing the packet's L2CAP CID with `local_cid` in the OBEX state key. L2CAP packets carry the CID of the endpoint they are directed toward, so request and response CIDs normally differ; `local_cid` is stable for the connection and therefore supports consistent lookup.

**Implementation rule:** before choosing a state key, ask whether each component is invariant under direction reversal for the state being modeled. If the analysis links both directions, use connection identity rather than a directional packet identifier.

**Confidence:** High. Merged master fix with a concrete incorrect-association failure mode and maintainer acceptance.

## Parser bounds must describe bytes actually present in the backing buffer

Do not pass a logical record remainder, advertised length, or total object size to a parser when only a smaller prefix has actually been read into memory. The callee's bound must describe the bytes it can physically access.

Merged MR !26004, authored and merged by John Thacker, fixes Network General Sniffer header processing where only up to 256 bytes were read but the helpers were given the larger `rec_length_remaining`. The corrected calls pass `bytes_to_read` in both code paths. Release backports !26005 and !26006 preserve the same correction.

**Implementation rule:** distinguish “bytes remaining in the format” from “bytes materialized in this buffer.” Bounds supplied to raw-buffer parsers must use the latter.

**Confidence:** Very high. Security-motivated master fix authored and merged by John Thacker plus two release backports.

## Prefer iterative traversal for attacker-controlled nesting when recursion adds no semantic value

Recursive traversal of untrusted structured input can turn nesting depth into stack consumption even when every individual access is bounds-safe. When the traversal can be expressed with an explicit counter/worklist, prefer the iterative form.

Merged MR !26007, authored by John Thacker, replaces recursive `json_get_next_object()` token walking with an iterative token-count traversal specifically to reduce stack use in ASan Debug/fuzz builds. Guy Harris explicitly connected the recursive form with fuzz-test stack overflows in review. Release backports !26009 and !26010 preserve the change.

**Implementation rule:** treat stack depth as a resource controlled by hostile input. Parser recursion is appropriate only when a meaningful, bounded grammar invariant justifies it; otherwise use iterative traversal.

**Confidence:** Very high. Merged security/fuzzing fix plus direct Guy Harris review feedback and two release backports.

## File-format probes should reject implausible input cheaply before expensive parsing

A wiretap format recognizer should perform a small, semantically meaningful plausibility check before committing to expensive whole-file parsing when unrelated input can share the outer syntax. At the same time, the probe should not impose incidental serialization details that the format does not require.

Merged MR !26008 changes Chrome NetLog recognition to inspect only the first two JSON tokens first: the top level must be an object and the first member name must be one of the known NetLog members. This quickly rejects deeply nested unrelated JSON that would otherwise consume substantial parsing work. The check deliberately accepts several valid member names rather than requiring `constants` first, because a standards-compliant JSON rewrite may reorder object members.

**Implementation rule:** make format probes cheap, discriminating, and semantically tolerant. Reject obvious nonmatches early, but do not confuse a producer's usual ordering/layout with a required format invariant.

**Confidence:** High. Merged master hardening by John Thacker with explicit rationale about both denial-of-service cost and member-order tolerance.

## Decode wire lengths in their real domain and validate before arithmetic

If a file or protocol defines a length as an unsigned N-bit quantity, decode and hold it in a corresponding unsigned type until its validity has been established. Validate relational constraints, implementation capacity, and arithmetic headroom before adding headers, narrowing, casting to signed types, or using the value in buffer operations.

Merged MR !26012, authored and merged by John Thacker, hardens Android btsnoop capture handling by reading reported and captured lengths as `uint32_t`, rejecting captured lengths larger than the allocated packet buffer, rejecting reported lengths smaller than captured lengths, and checking addition headroom before subsequent size arithmetic. The prior signed conversion could turn hostile high-bit values negative and undermine the checks.

Merged MR !26013 independently reinforces the local-boundary part of the rule: before a Netflix BBlog TCPINFO option reads its two mandatory `uint64_t` fields, it verifies the option length is at least `2 * sizeof(uint64_t)`. Release-4.6 backport !26016 preserves that guard.

**Implementation rule:** validation precedes arithmetic. First prove the raw wire value is representable and structurally plausible; only then derive totals, offsets, allocation sizes, or signed API values from it.

**Confidence:** Very high. Two merged security fixes, one authored/merged by John Thacker, plus an accepted release backport.

## Prefer portable byte-assembly helpers over architecture-specific pointer tricks

Portable byte-at-a-time shift/mask implementations are not necessarily a performance concession. In merged MR !25999, authored and merged by Guy Harris, `wsutil/pint.h` documents that newer GCC and Clang recognize these unaligned fetch/store idioms and lower them to efficient platform operations such as unaligned loads/stores and byte swaps where appropriate.

**Implementation rule:** favor the project's portable integer access helpers and clear byte-order semantics instead of introducing alignment/aliasing-sensitive casts solely for presumed speed. Let supported compilers optimize recognized idioms unless measurement proves a real need for a specialized path.

**Confidence:** High. Direct documentation change authored and merged by Guy Harris.

## Macro-generated low-level APIs should have one canonical signature

When a public or widely included header generates families of inline helpers, define the API signature once and vary only the implementation where platform/compiler branches require it. Duplicating whole function definitions behind conditionals makes it possible for alternate paths to drift to different parameter widths or semantics. Internal helper macros used to generate those functions should also be undefined after use so they do not leak into includers' namespaces.

Merged MR !26020, authored by Guy Harris and merged after John Thacker review, restructures `pint.h` specifically to avoid defining the inline helper family twice. The motivating bug was the mismatched width fixed in !25994. John explicitly characterized the consolidated version as easier to read and less likely to lead to mistakes.

**Implementation rule:** a conditional implementation path must not silently become a second API declaration. Keep one source of truth for parameter/return types, and clean up private generation macros at the end of the header.

**Confidence:** Very high. Merged master change authored by Guy Harris, directly tied to a prior memory-safety bug and positively reviewed by John Thacker.

## Remaining-length counters should be consumed with checked subtraction

For a parser or wiretap reader that tracks an unsigned `*_remaining` count, make consumption of that count explicit and monotonic. Before a read whose size has not already been proven to fit, use checked subtraction and report malformed input if it underflows; if the size has already been proven `<= remaining`, the read may occur first and the known-safe subtraction may follow.

Merged MR !26021, authored by Guy Harris and merged after John Thacker review, applies this pattern throughout the Network General Sniffer reader and states that it should ultimately become a general reader pattern. Several structure reads replace a separate comparison plus later decrement with `ckd_sub(&rec_length_remaining, rec_length_remaining, sizeof structure)` before the read.

**Implementation rule:** do not let a remaining-byte counter become merely advisory metadata. Every consumption must either atomically prove-and-subtract with checked arithmetic, or follow an earlier proof that makes the subtraction incapable of underflow.

**Confidence:** Very high. Explicit normative rationale in a merged master MR authored by Guy Harris.

## Validate exact cryptographic input sizes at the external-data boundary

A cryptographic primitive with a fixed-size key/input contract must not be called merely because the surrounding data source is nominally trusted metadata. Validate the exact algorithm-required size before passing externally supplied packet, capture, key-log, or Decryption Secrets Block material to the primitive.

Merged MR !26026, authored and merged by John Thacker, checks that both Curve25519 public and private keys are exactly 32 bytes before calling `crypto_scalarmult_curve25519()`. The malformed length can originate in a bogus Decryption Secrets Block rather than ordinary packet payload. Release backports !26033 and !26034 preserve the check.

**Implementation rule:** library preconditions are parser boundaries too. Validate them explicitly where untrusted/external data crosses into fixed-size crypto APIs, and fail locally rather than relying on the primitive to tolerate malformed sizes.

**Confidence:** Very high. Security-motivated merged master fix by John Thacker plus two accepted release backports.

## Output normalization must not fabricate bytes or underflow semantic lengths

When a capture-file writer must align or normalize an output record, preserve the semantic distinction between bytes actually captured and bytes missing because the packet was snapped. Padding a truncated packet can be wrong when the file format would interpret that padding as captured payload; conversely, rounding down must be rejected if the required subtraction exceeds the captured data.

Merged MR !26028, authored and merged by John Thacker, hardens the ERF writer. ERF records require 64-bit alignment. For already-truncated packets the writer rounds down rather than pads, because padding would masquerade as missing captured bytes; it now checks `caplen < round_down` and returns `WTAP_ERR_UNWRITABLE_REC_DATA` instead of underflowing. Release-4.6 backport !26032 corroborates the behavior.

**Implementation rule:** validate the semantics and arithmetic of writer-side transformations before applying them. If a format constraint cannot be met without inventing payload or making a length negative/wrap, reject the record as unwritable.

**Confidence:** Very high. Security-motivated merged master wiretap fix by John Thacker plus accepted release backport.

## Reject impossible variable headers before allocating from derived sizes

If a file-format field controls a pseudo-header or subrecord size, validate its structural relationship to the enclosing record and perform checked arithmetic before using any derived size for allocation. A later short-read failure is not an adequate guard if overflow can first turn malformed metadata into a huge allocation request.

Merged MR !26036, authored and merged by John Thacker, fixes the Shomiti/Finisar Surveyor snoop reader. A variable pseudo-header larger than the total packet size could overflow derived arithmetic and trigger a near-4-GiB buffer allocation before the subsequent read failed. The fix makes the header length unsigned and uses `ckd_sub()` both for the minimum pseudo-header calculation and for subtracting the pseudo-header from the packet size, returning `WTAP_ERR_BAD_FILE` before allocation on failure.

**Implementation rule:** validate enclosing-size relationships before allocation, not after it. Checked arithmetic should guard the first derivation of a size that can influence allocation or reads.

**Confidence:** Very high. Security-motivated merged master wiretap fix by John Thacker with a concrete resource-exhaustion failure mode.