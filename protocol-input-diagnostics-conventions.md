# Wireshark Protocol Input and Diagnostic Conventions

This file records durable malformed-input and expert-diagnostic conventions extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Validate packet address representation before direct address-data access

`packet_info` address storage should not be treated as an untyped byte array whose expected shape is guaranteed by the surrounding dissector. If code directly dereferences or copies address data, first verify that the address has the expected type and sufficient/exact length for the structure being consumed.

Merged MR !17590 (`AppleTalk: Make sure we have valid addresses`) hardens ATP, ZIP, and ASP paths by validating the source/destination address representation before directly using its data. It also replaces hard-coded byte counts with `sizeof(struct atalk_ddp_addr)`. Merged MR !17596 independently adds address-data validity checks in RELOAD Framing before constructing state keys.

**Implementation rule:** before direct access to `pinfo->src`, `pinfo->dst`, or related address backing data, validate the representation contract required by the code (type, data presence, and length). Use `sizeof` of the actual representation instead of duplicating magic byte counts.

**Confidence:** Very high. Merged master security/correctness fixes, with the same defensive pattern appearing independently in two dissectors.

## Expert severity should represent operational significance, not only formal nonconformance

A packet can violate canonical ordering or encoding recommendations while remaining common, interoperable, and safely decodable. Expert severity should help users prioritize meaningful problems rather than turn every standards deviation into a warning.

Merged MR !17591 (`bt-dht: flag duplicate and unordered keys`) detects duplicate and noncanonical dictionary-key ordering in BitTorrent DHT bencoding but deliberately uses CHAT severity. The implementation notes that noncanonical messages occur in real traffic and are generally tolerated.

**Diagnostic rule:** choose expert severity according to the likely operational consequence and diagnostic value. A safely decoded, commonly tolerated noncanonical form may merit CHAT/low-severity information even when it violates a canonical specification rule; reserve stronger severity for conditions that indicate malformed structure, likely interoperability failure, data loss, or unsafe parsing.

**Confidence:** High. Merged master implementation explicitly calibrates severity to observed real-world behavior.
