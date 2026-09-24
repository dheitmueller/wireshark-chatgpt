# Wireshark Dissector-Table Registration Conventions

This file records durable rules about registering and matching dissector-table keys. Current upstream source remains authoritative.

## Match table key semantics to the protocol, not to an accidental canonical spelling

When the protocol defines a textual discriminator as case-insensitive, register the corresponding string dissector table with case-insensitive matching rather than requiring every producer and every dissector registration to normalize manually to one spelling. Preferred/canonical presentation case does not imply case-sensitive matching.

Merged MR !24157 changes the HTTP Upgrade dissector table from `STRING_CASE_SENSITIVE` to `STRING_CASE_INSENSITIVE`, citing RFC 9110's requirement that protocol-name tokens are case-insensitive. The motivation also notes that registrations had already used preferred-case spellings, making a lowercase-only convention easy to violate. Anders Broman approved and merged the change.

**Registration rule:** derive `STRING_CASE_SENSITIVE` versus `STRING_CASE_INSENSITIVE` from the wire protocol's comparison semantics. Do not encode an undocumented normalization convention in callers when the table itself can express the correct matching rule.

**Confidence:** Very high. Small merged master change directly aligning the generic registration mechanism with normative protocol semantics.

## Apply text-key equality consistently at registration and lookup boundaries

A case-insensitive protocol identifier remains case-insensitive when it passes through a registry other than a dissector table. It is not sufficient for one side of a registry to normalize or compare loosely if the other side stores keys with case-sensitive semantics; registration and lookup must implement the same protocol-defined equality relation.

Merged master MR !11291, authored and merged by John Thacker, changes the RTP codec registry to register and look up encoding names case-insensitively. RFC 4855 defines RTP media subtype/encoding names as case-insensitive, and a case mismatch between SDP and the codec registration had prevented valid codecs such as AMR from being found for playback and Decode As use. Adjacent merged MR !11290 replaces a bare Boolean argument in the string-table implementation with the named `STRING_CASE_INSENSITIVE` constant, making that equality contract explicit in the call site.

**Registry rule:** for textual keys whose protocol grammar defines case-insensitive equality, use case-insensitive semantics for both insertion/registration and lookup. Do not rely on every producer to emit one canonical spelling unless the protocol itself requires that spelling.

**API-expression rule:** when an API offers a named semantic mode such as `STRING_CASE_INSENSITIVE`, prefer it over a bare Boolean literal. The named mode documents the equality contract and makes review less dependent on remembering an argument's Boolean meaning.

**Confidence:** Very high. Merged master codec-registry correctness change authored and merged by John Thacker, reinforced by an adjacent merged cleanup that makes the matching mode explicit.