# Wireshark Dissector Consumption Boundary Conventions

This file records durable conventions for how a dissector should claim bytes when its protocol can end before the enclosing tvbuff or frame does. Current upstream source remains authoritative.

## Claim only bytes the protocol can structurally justify

A dissector that recognizes a protocol unit ending before the enclosing frame must not consume arbitrary trailing bytes merely because no explicit terminator was seen. Optional terminators, malformed length fields, and unknown element types require the parser to distinguish bytes that can still belong to the protocol from bytes that should remain available to another dissector.

Merged MR !15859 fixes LLDP after IEEE 802.1AB made the End-of-LLDPDU TLV optional. The old parser could consume the rest of the frame when the terminator was absent and could also return one byte too little when the terminator was present, confusing downstream dissectors. The accepted implementation fixes the consumed-length result, defaults to leaving unrecognized trailing bytes unclaimed, and refuses to call an unknown TLV valid when its declared length would run beyond the reported tvbuff. A user preference can opt back into consuming otherwise unrecognized bytes as an unknown TLV.

**Implementation rule:** make the dissector's return value and any subset tvbuff boundaries match the bytes actually recognized as belonging to the protocol. If an unknown element cannot be structurally complete within the available protocol data, stop rather than treating all remaining bytes as that element.

**Compatibility rule:** if historical behavior greedily consumed ambiguous trailing data, a compatibility preference may preserve that behavior, but conservative non-consumption should be preferred when it lets enclosing or following dissectors recover their own data correctly.

**Confidence:** Very high. Merged master parser-boundary correction whose MR description states both the old over-consumption/under-return failure modes and the accepted conservative behavior.
