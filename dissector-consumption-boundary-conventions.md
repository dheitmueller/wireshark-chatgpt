# Wireshark Dissector Consumption Boundary Conventions

This file records durable conventions for how a dissector should claim bytes when its protocol can end before the enclosing tvbuff or frame does. Current upstream source remains authoritative.

## Claim only bytes the protocol can structurally justify

A dissector that recognizes a protocol unit ending before the enclosing frame must not consume arbitrary trailing bytes merely because no explicit terminator was seen. Optional terminators, malformed length fields, and unknown element types require the parser to distinguish bytes that can still belong to the protocol from bytes that should remain available to another dissector.

Merged MR !15859 fixes LLDP after IEEE 802.1AB made the End-of-LLDPDU TLV optional. The old parser could consume the rest of the frame when the terminator was absent and could also return one byte too little when the terminator was present, confusing downstream dissectors. The accepted implementation fixes the consumed-length result, defaults to leaving unrecognized trailing bytes unclaimed, and refuses to call an unknown TLV valid when its declared length would run beyond the reported tvbuff. A user preference can opt back into consuming otherwise unrecognized bytes as an unknown TLV.

Merged master MR !15363, authored and merged by Guy Harris, provides high-authority corroboration from TIPC. Its tvbuff can contain bytes beyond the protocol-declared message. Filler used for MTU discovery therefore must be bounded by the TIPC message size (`msg_size - offset`), not by `tvb_reported_length_remaining()`, which can include unrelated trailing junk.

**Implementation rule:** make the dissector's return value and any subset tvbuff boundaries match the bytes actually recognized as belonging to the protocol. If an unknown element cannot be structurally complete within the available protocol data, stop rather than treating all remaining bytes as that element. When the protocol supplies an authoritative unit length, compute protocol-local padding/filler against that length rather than assuming the enclosing tvbuff ends at the same boundary.

**Compatibility rule:** if historical behavior greedily consumed ambiguous trailing data, a compatibility preference may preserve that behavior, but conservative non-consumption should be preferred when it lets enclosing or following dissectors recover their own data correctly.

**Confidence:** Extremely high. Merged LLDP boundary correction plus an independently merged master fix authored and merged by Guy Harris showing that protocol-declared length outranks enclosing tvbuff length.

## Guard and failure paths must not invent a consumed length

A parser that stops because of a recursion/resource guard or an unsupported nested form may not know the complete byte extent of the item it was about to decode. In that case, its return value must describe only progress it can prove. Claiming an arbitrary remainder as consumed can be worse than returning no progress: callers may interpret the result as a valid item boundary, backtrack using it, or skip data that should instead be shown as undecoded/unknown.

Merged MR !15499 fixes exactly this in ZigBee TLV recursion handling. When the nesting limit was reached, `dissect_zbee_tlvs()` previously returned the remaining tvbuff length even though it did not know the length of the nested value. With carefully constructed input that false consumption allowed caller backtracking to set up an infinite loop. The accepted fix returns the current offset instead; callers that check for unconsumed bytes can then treat the nested value as unknown rather than pretending its boundary was known.

**Implementation rule:** on an early guard/failure exit, return the last definitely consumed offset or length. Do not use “consume the rest” as a generic escape from recursion, malformed input, or unsupported structure unless the enclosing format itself proves that the remainder belongs to the current item.

**Review rule:** check the semantic contract of parser return values on failure as carefully as on success. A termination guard that prevents one resource failure can still create non-progress, backtracking, or ownership bugs if it reports an invented boundary.

**Confidence:** Very high. Merged fuzz-found infinite-loop fix; the MR explicitly explains why the old return value was semantically false and how it interacted with caller backtracking.

## Advance using the parser's validated effective length

When a parsing helper validates, clamps, or otherwise normalizes an untrusted wire length, its returned consumed length is the authoritative boundary for the caller. The caller must not discard that result and later advance using the original packet field; doing so can reintroduce a negative, undersized, or otherwise bogus length that the helper deliberately corrected.

Merged master MR !15047, authored and merged by John Thacker, fixes Mongo BSON handling. BSON document lengths are signed 32-bit values. `dissect_bson_document()` already sanity-checks malformed lengths and returns the effective length it safely used, but its callers advanced using the original `section_len` read from the packet. The accepted change assigns the helper's returned length back to `section_len` and advances with that value. Release-4.2 MR !15048, together with the already reviewed stable backports !15049 and !15050, carries the same correction across maintained branches.

**Implementation rule:** if a helper both parses a region and returns how much input it safely consumed, treat that return value as the post-validation truth. Do not recompute progress from the pre-validation wire field unless the API explicitly documents the two values as equivalent.

**Review rule:** trace length values across helper boundaries. Validation inside a callee is ineffective if later caller arithmetic still uses the unsanitized source value.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker, with multiple accepted stable-branch backports.

## Error-recovery helpers used by loops must make monotonic progress

A helper whose contract is to skip or recover past malformed input must not quietly rewind the caller to the same malformed item and return a recoverable failure. If the caller loops based on the offset, that turns error handling into non-progress and can amplify a malformed count or nested structure into an infinite loop or memory-exhaustion condition.

Merged master MR !9752, authored and merged by John Thacker, fixes this in the shared CBOR support. `wscbor_skip_next_item_internal()` called `wscbor_chunk_read()`, but on an error it reset `*offset` to the chunk start before returning `FALSE`. That contradicted `wscbor_skip_if_errors()`'s purpose: a caller trying to skip a bad item could be handed the same starting offset again. The MR explicitly identifies memory exhaustion and infinite looping for malformed lists with very large item counts. The accepted fix stops rewinding the offset, preserving the progress already made while diagnosing the malformed item.

**Implementation rule:** for parser/recovery APIs used inside loops, define the failure-path progress contract explicitly. A recover-and-continue result must either advance past the offending input or provide an unmistakable terminal result that makes the caller stop. Do not combine “try to skip this item” semantics with resetting the input cursor to the same item.

**Review rule:** trace offsets/iterators on every malformed-input exit, not only successful decode paths. If a failure is intended to be recoverable, verify that the next loop iteration cannot observe exactly the same state indefinitely.

**Confidence:** Extremely high. Merged core-library robustness fix authored and merged by John Thacker, with the denial-of-service failure mode stated directly in the MR.


## Validate packet-derived cursor arithmetic before advancing

When a packet length is used to advance a parser cursor, validate that the new cursor is representable before calculating it. A later bounds check cannot repair a cursor value that has already wrapped.

Merged master MR !9124, authored by Gerald Combs, applies this rule throughout RTPS parsing. The accepted implementation checks cursor advancement before the addition and reports malformed input through Wireshark's normal bounds path when the requested advance is not representable. Release-4.0 MR !9134 and release-3.6 MR !9135 carry the same correction.

The same master change makes unknown native-type length and alignment recovery use a small positive progress value rather than a negative result, so iterative parsing continues with a defined forward-progress contract.

**Implementation rule:** validate cursor-plus-length, count-times-element-size, and similar packet-derived arithmetic at the arithmetic boundary. Malformed values should enter the ordinary bounds/error path instead of producing a wrapped cursor.

**Recovery rule:** unsupported-element recovery inside a parser loop must either stop or make deliberate forward progress. Avoid sentinel values that can become invalid cursor arithmetic.

**Confidence:** Very high. Merged master correctness fix by Gerald Combs with accepted backports to both maintained release lines.
