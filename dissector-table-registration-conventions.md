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
## Extend a shared file-protocol parser via registered local handlers

Merged master !9688 by Martin Mathieson adds registration for local/vendor-defined pcapng blocks and an Apple Darwin block dissector. A callback registration structure carries the local block handler and option dissector, while the common pcapng dissector retains shared framing. Joakim Karlsson requested and received a consistent field namespace `pcapng.darwin` instead of a hyphenated `pcapng-darwin` namespace, aligning protocol grouping and filter names with the base parser's tables.

**Architecture rule:** for an extensible file or protocol container, keep common framing/record parsing centralized, register typed callbacks for extension-specific bodies and options, and pass explicit parent/context data to handlers. Keep extension filter abbreviations under an intentional shared namespace that matches actual registered protocol/table ownership. **Evidence caveat:** the corpus omitted the oversized `file-pcapng.c` diff, so exact internal registration mechanics must be checked against current source. **Confidence:** high for accepted design and explicit reviewer changes; limited for omitted diff details.

## Use the table's matched selector rather than assuming a fixed well-known port

In John Thacker's review of the closed Windows Delivery Optimization proposal !9658, he advised comparing `pinfo->match_uint` with `pinfo->destport` to determine traffic direction. `match_uint` represents the matched numeric key from the subdissector table, so this continues to work when a user redirects dispatch using **Decode As**, unlike a hard-coded IANA port.

**Dispatch rule:** when deriving direction or service identity from a dissector-table registration, use the actual dispatch key and destination/source context; don't silently assume the canonical port was the one used. This is explicit high-authority **review guidance**, not acceptance of the closed implementation; !9658 was replaced by !10124. **Confidence:** high for the reviewer advice, implementation unverified in the closed snapshot.

Merged !9638 (John Thacker) separates standardized multiplexing fallback for RTP invoked nonheuristically from the separate responsibility of each protocol's heuristic recognizer. Merged !9652 registers TLS as an HTTP Upgrade subdissector under the appropriate dispatch table. **Registration rule:** attach protocol transitions at the table that actually represents the transition and preserve heuristic handoff/decline behavior; a shared port or five-tuple alone is not a sufficient ownership decision. **Confidence:** very high for merged code.

## Preserve external extension registration when refactoring vendor-specific IE dispatch

**Merged master !9368** reworks PFCP enterprise IE dissection. **Joakim Karlsson** explicitly objected when an intermediate design removed the enterprise-number dissector table because third-party vendor dissectors could no longer register without modifying core source. **Anders Broman** reinforced that built-in vendor handling should use the same registration path as an external extension. The author restored the table before merge and added `create_dissector_handle_with_data()` in `epan/packet.c` and `epan/packet.h` so a callback handle can carry per-handler context. Review also debated preserving expert handling for undecoded trailing IE bytes.

**Architecture rule:** an in-tree cleanup must not silently remove a public extension table or restrict vendor registration to compiled-in switch cases. Maintain a consistent callback contract for in-tree and out-of-tree dissectors, and preserve visibility of trailing data not consumed by a registered IE handler. **Evidence limit:** the corpus omits the oversized `packet-pfcp.c` diff for !9368; the restored extension behavior is established by the maintainer discussion, acceptance, and the visible packet API diff, not by an inspection of that missing file diff. **Confidence:** very high for the registration requirement.
