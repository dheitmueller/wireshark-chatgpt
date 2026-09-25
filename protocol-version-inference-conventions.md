# Wireshark Protocol Version Inference Conventions

This file records durable conventions for choosing protocol variants when a wire format does not carry an explicit version marker. Current upstream source remains authoritative.

## Infer implicit versions from normative structural invariants

When a protocol revision changes layout without carrying an explicit version field, variant selection should be based on an independent structural property that the specification defines for the relevant revisions. Avoid inferring version from incidental payload values merely because they happened to distinguish the captures used during development.

Merged master MR !13293, authored by John Thacker, fixes RSVD `SenseDataEx` decoding across protocol revisions. Versions 7.0 through 10.0 used a fixed 20-byte `SenseDataEx`, while later revisions reverted the structure. Because the packet has no explicit version indicator, the accepted dissector identifies the older layout from the version-dependent response structure/length (`Length == 36`, together with the corresponding sense-information length) and then dispatches to the matching decoder.

**Implementation rule:** first identify a specification-defined invariant that is independent of the data being interpreted differently between variants: total structure length, reserved field, mandatory discriminator, framing relationship, or another normative property. Use that invariant to choose the variant, then decode variant-specific fields. Do not make the value of one variant-dependent field prove which interpretation of that same field is correct.

**Review implication:** document which revisions define the discriminator and why the chosen invariant is unambiguous. Include representative packets for each supported layout when possible, plus malformed/ambiguous cases if the invariant can be violated on the wire.

**Confidence:** Very high. Merged master fix by John Thacker whose MR description and implementation explicitly use the specification's version-dependent fixed structure size to distinguish revisions lacking an explicit version marker.

## Derive negotiated version state from both peers and update it only on the learning pass

When a protocol negotiates a version or capability from information advertised by both endpoints, the dissector should model the negotiation rule rather than assuming that one peer's maximum is the negotiated result. If that inferred value becomes conversation state, repeated random-access dissection should consume the recorded state rather than mutating it according to whichever handshake packet happened to be revisited.

Merged master MR !10611, authored by John Thacker and merged by Anders Broman, improves TDS version detection. It records the client and server program versions separately, derives the expected negotiated TDS version from the highest version supported by both sides when both are known, and guards version mutations with `PINFO_FD_VISITED(pinfo)`. This also handles captures where a server response is visible before the corresponding client information or where an encrypted LOGIN7 hides the definitive version until later.

**State rule:** learn negotiation state on the first sequential analysis pass and make redissection read that state. Do not let random packet access reorder or repeatedly overwrite conversation-wide version inference.

**Inference rule:** when both peers contribute to negotiation, apply the protocol's actual compatibility/selection rule to both advertisements. Do not equate "server supports version X" (or the corresponding client fact) with "the session negotiated X" unless the protocol guarantees that implication.

**Review/testing rule:** include captures with asymmetric peer capabilities and, where practical, partial or encrypted handshakes in which only one side is initially visible. Verify that packet display remains stable under random access and two-pass dissection.

**Confidence:** Very high. Merged master state/inference correction authored by John Thacker with the random-access and asymmetric-capability cases documented in the MR.
