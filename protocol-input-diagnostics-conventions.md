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

## Distinguish undecoded extension data from malformed structure

Unknown input is not automatically malformed input. If the parser can safely skip an unrecognized field and continue processing the surrounding structure, classify the condition according to what Wireshark failed to decode rather than implying that the wire representation is structurally invalid.

Merged MR !25717 changes BER expert categories for extra unknown SEQUENCE/SET fields from `PI_MALFORMED` to `PI_UNDECODED`. The fields can be skipped while the remainder of the ASN.1 object is processed, and the change deliberately aligns BER with similar BER/PER diagnostics.

**Diagnostic rule:** use malformed/error categories for violated structural contracts that compromise parsing or validity. Use undecoded/unsupported categories when the data is structurally consumable but Wireshark lacks semantic knowledge of that extension or field.

**Confidence:** Very high. Merged master diagnostic correction authored by John Thacker and merged by Anders Broman.

## Put malformed-representation enforcement in the shared parser helper when the contract is universal

When a reusable parser API has one universally invalid representation, make the helper enforce that invariant and provide the standard diagnostic/exception behavior. Requiring every caller to rediscover and check the same invalid return value creates inconsistent handling and unnecessary branches.

Merged MR !25734, authored by Gerald Combs and approved by John Thacker, makes `proto_tree_add_item_ret_varint()` treat a zero-length varint as malformed: the helper adds the expert item and throws the normal bounds/malformed exception. The LTP and TCPCL dissectors then remove their duplicate zero-length checks because the helper contract now guarantees the failure semantics.

**Implementation rule:** centralize invariant validation at the lowest shared API layer that has enough information to classify the error correctly. Once that contract is established, remove redundant caller checks unless a caller genuinely needs different semantics.

**Confidence:** Extremely high. Merged core-API change authored by project lead Gerald Combs and approved by John Thacker.