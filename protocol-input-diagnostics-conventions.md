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

Merged MR !24064 independently reinforces the same distinction in TCP reset analysis. Jaap Keuter pointed out during review that an actual TCP reset is warning-worthy, while the derived information describing which endpoint sent the first reset is informational and belongs at NOTE/CHAT severity. The accepted implementation was adjusted accordingly before merge.

**Diagnostic rule:** choose expert severity according to the likely operational consequence and diagnostic value. A safely decoded, commonly tolerated noncanonical form or derived explanatory fact may merit CHAT/NOTE severity even when related to a warning-worthy event; reserve stronger severity for conditions that indicate malformed structure, likely interoperability failure, data loss, or unsafe parsing.

**Confidence:** Very high. Independent merged evidence now covers both tolerated protocol nonconformance and derived troubleshooting metadata, including direct reviewer correction in !24064.

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

## Use PI_PROTOCOL for specification violations that remain safely dissectable; reserve PI_MALFORMED for parse-stopping malformed data

Wireshark's expert categories communicate more than the fact that bytes violate a specification. `PI_MALFORMED` has a narrower operational meaning: the malformed structure prevents the dissector from continuing normally. A protocol violation that can still be parsed should be classified as a protocol problem instead.

In merged MR !25453, which adds HTTP diagnostics for whitespace before a header colon and embedded NULs in header values, John Thacker explicitly requested changing the expert group from `PI_MALFORMED` to `PI_PROTOCOL`. His rationale was that Wireshark uses `PI_MALFORMED` when the dissector has to give up, while `PI_PROTOCOL` covers data that violates the specification but can still be dissected. The MR was updated accordingly and merged.

**Diagnostic rule:** classify a standards violation as `PI_PROTOCOL` when the dissector can identify the violation and continue parsing safely. Use `PI_MALFORMED` when structural corruption prevents normal dissection or forces the parser to abandon the affected structure. Treat expert group and expert severity as separate decisions.

**Confidence:** Extremely high. Direct merged-review guidance from John Thacker, followed by an accepted implementation change.

## Decode known real-world noncompliance when useful, but do not silently bless it

Interoperability-oriented dissectors may need to recognize a known vendor or implementation encoding that violates the formal specification. Supporting that traffic is compatible with strict diagnostics: decode the observed representation when it can be identified safely, while still telling the user that the wire encoding is noncompliant.

Merged MR !25238 extends HI2Operations decoding for APN and ULI encodings emitted by real implementations. During review Anders Broman explicitly requested expert information for the noncompliant forms; the author added it before merge.

**Diagnostic rule:** compatibility decoding and standards validation are separate concerns. A dissector may recover and display a known nonstandard encoding, but should attach an appropriate expert indication rather than silently normalizing it into an apparently conforming packet.

**Confidence:** Very high. Merged master compatibility change with explicit Anders Broman review and requested diagnostic added before merge.

## Return parse/conversion failure details explicitly when callers need to classify malformed values

A low-level decoder that can produce a useful value while also detecting invalid, overflow, or underflow conditions should expose that status through its API rather than turning packet-controlled failures into assertions or forcing callers to infer a process-global side channel. The protocol-facing caller can then map the result to expert information appropriate to its encoding context.

Merged MR !24143 rewrote ASN.1 REAL conversion so illegal encodings and range conditions are reported with normal error status and expert items instead of `DISSECTOR_ASSERT`. During review Guy Harris specifically proposed changing `asn1_get_real()` so the errno-style result is returned through an output pointer; John Thacker adopted the API change and resolved the thread. The MR also accounts for platform differences in floating-point error reporting instead of assuming a single C-library behavior.

**Implementation rule:** when a shared conversion helper needs to return both a decoded value and a diagnostic classification, make both parts explicit in the function contract. Keep packet-controlled malformed/range errors recoverable and let the dissector layer decide how to present them.

**Confidence:** Extremely high. Merged master parser change by John Thacker with a concrete API-contract correction from Guy Harris incorporated before merge.