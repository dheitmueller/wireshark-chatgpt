# Wireshark Protocol Implementation Compatibility Conventions

This file records durable conventions for reconciling protocol specifications with deployed implementation behavior and for handling unsupported/version-skewed inputs. Current upstream source and protocol specifications remain authoritative.

## Prefer documented protocol semantics; isolate compatibility with known implementation deviations

Real implementations can disagree with protocol documentation, and even an official implementation can contain behavior that another official component does not share. A dissector should not silently redefine a field's width or meaning from a single observed implementation when the documented wire contract and other implementations disagree.

Merged MR !15539 reverts an earlier MySQL multibyte-collation interpretation after evidence showed a three-way inconsistency: current MySQL documentation and server behavior treated the handshake collation as one byte, while Connector/Python emitted two bytes and could therefore advertise a collation ID the server itself did not consume as such. The MR documents the ambiguity in detail and explicitly favors following documentation where possible while tolerating released client/server behavior where needed. The accepted outcome was to revert the broad interpretation rather than institutionalize an uncertain wire format.

**Implementation rule:** use the documented protocol contract as the default interpretation. When released implementations violate or extend it, support the deviation narrowly when interoperability value justifies it, and keep the compatibility path distinguishable from the normative interpretation. Avoid changing a common field's fundamental width or semantics solely because one implementation emits a different representation.

**Review rule:** when specification and implementation evidence conflict, identify the exact actors and bytes involved, compare both sender and receiver behavior where possible, and document the uncertainty. Reverting or deferring a speculative generalization is preferable to turning a localized quirk into an undocumented dissector rule.

**Confidence:** High. Merged master revert with detailed empirical evidence from MySQL server and official Connector/Python behavior and an explicit rationale for preferring documented semantics while retaining room for real-world compatibility.

## Do not use fields the current protocol says to ignore as dissection gates

A compatibility or legacy field can remain on the wire while the current specification explicitly removes it from protocol semantics. Once protocol identity has been established by valid dispatch context, such a field should not reject or redirect otherwise valid traffic merely because its value resembles an older-version discriminator.

Merged MR !15503 removes an extra DTLS `legacy_record_version` check from the main DTLS dissector. The heuristic path had already validated that the packet looked like DTLS, and explicit port/Decode As dispatch is itself an instruction to use DTLS. RFC 9147 states that the legacy record version is to be ignored for all purposes, so rechecking it inside the dissector could only reject traffic based on a field that no longer has that semantic role. The record content type remains available for actual structural validation.

**Implementation rule:** distinguish displayable legacy/reserved fields from active protocol-selection fields. If the applicable standard says a field is ignored, reserved, or no longer semantically meaningful, do not promote it into a recognition gate after other authoritative dispatch evidence has selected the dissector. An expert item may still call attention to unusual values when that is useful without changing dispatch semantics.

**Confidence:** Very high. Merged master change authored by John Thacker and approved/merged by Anders Broman, with the RFC requirement and the dissector's dispatch paths explained directly in the MR.

## If best-effort decoding substitutes a supported version, make the assumption explicit and preserve progress

Unsupported version numbers occur in malformed captures, fuzzing, bit-damaged traffic, and occasionally forward/backward-compatible protocol deployments. A best-effort dissector may choose a nearby supported version instead of abandoning the payload, but the substitution is an interpretation assumption and must not create a parser path that repeatedly consumes nothing.

Merged MR !15516 changes Kafka handling so an unsupported API version can be decoded using a nearest supported version while adding expert information that records the assumption. The motivating fuzz case showed that passing an invalid version through message-specific code could select no fields and fail to advance the offset, leading to an effective infinite loop. The MR discussion explicitly considered the alternative of refusing to dissect invalid versions entirely; the accepted best-effort path therefore couples fallback decoding with visible diagnostic context.

**Implementation rule:** when using a supported version as a proxy for an unsupported on-wire version, choose the fallback deterministically, annotate the assumption for the user, and ensure the selected decode path either advances or terminates cleanly. If no bounded and semantically plausible fallback exists, stop dissection rather than looping or fabricating structure.

**Confidence:** Very high. Merged master change authored and merged by John Thacker, motivated by a concrete fuzz-found non-progress failure and explicit discussion of best-effort versus abort behavior.