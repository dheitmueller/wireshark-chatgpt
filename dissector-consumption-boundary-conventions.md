# Wireshark Dissector Consumption Boundary Conventions

This file records durable conventions for how a dissector should claim bytes when its protocol can end before the enclosing tvbuff or frame does. Current upstream source remains authoritative.

## Claim only bytes the protocol can structurally justify

A dissector that recognizes a protocol unit ending before the enclosing frame must not consume arbitrary trailing bytes merely because no explicit terminator was seen. Optional terminators, malformed length fields, and unknown element types require the parser to distinguish bytes that can still belong to the protocol from bytes that should remain available to another dissector.

Merged MR !15859 fixes LLDP after IEEE 802.1AB made the End-of-LLDPDU TLV optional. The old parser could consume the rest of the frame when the terminator was absent and could also return one byte too little when the terminator was present, confusing downstream dissectors. The accepted implementation fixes the consumed-length result, defaults to leaving unrecognized trailing bytes unclaimed, and refuses to call an unknown TLV valid when its declared length would run beyond the reported tvbuff. A user preference can opt back into consuming otherwise unrecognized bytes as an unknown TLV.

**Implementation rule:** make the dissector's return value and any subset tvbuff boundaries match the bytes actually recognized as belonging to the protocol. If an unknown element cannot be structurally complete within the available protocol data, stop rather than treating all remaining bytes as that element.

**Compatibility rule:** if historical behavior greedily consumed ambiguous trailing data, a compatibility preference may preserve that behavior, but conservative non-consumption should be preferred when it lets enclosing or following dissectors recover their own data correctly.

**Confidence:** Very high. Merged master parser-boundary correction whose MR description states both the old over-consumption/under-return failure modes and the accepted conservative behavior.

## Guard and failure paths must not invent a consumed length

A parser that stops because of a recursion/resource guard or an unsupported nested form may not know the complete byte extent of the item it was about to decode. In that case, its return value must describe only progress it can prove. Claiming an arbitrary remainder as consumed can be worse than returning no progress: callers may interpret the result as a valid item boundary, backtrack using it, or skip data that should instead be shown as undecoded/unknown.

Merged MR !15499 fixes exactly this in ZigBee TLV recursion handling. When the nesting limit was reached, `dissect_zbee_tlvs()` previously returned the remaining tvbuff length even though it did not know the length of the nested value. With carefully constructed input that false consumption allowed caller backtracking to set up an infinite loop. The accepted fix returns the current offset instead; callers that check for unconsumed bytes can then treat the nested value as unknown rather than pretending its boundary was known.

**Implementation rule:** on an early guard/failure exit, return the last definitely consumed offset or length. Do not use “consume the rest” as a generic escape from recursion, malformed input, or unsupported structure unless the enclosing format itself proves that the remainder belongs to the current item.

**Review rule:** check the semantic contract of parser return values on failure as carefully as on success. A termination guard that prevents one resource failure can still create non-progress, backtracking, or ownership bugs if it reports an invented boundary.

**Confidence:** Very high. Merged fuzz-found infinite-loop fix; the MR explicitly explains why the old return value was semantically false and how it interacted with caller backtracking.