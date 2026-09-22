# Wireshark Protocol Version Inference Conventions

This file records durable conventions for choosing protocol variants when a wire format does not carry an explicit version marker. Current upstream source remains authoritative.

## Infer implicit versions from normative structural invariants

When a protocol revision changes layout without carrying an explicit version field, variant selection should be based on an independent structural property that the specification defines for the relevant revisions. Avoid inferring version from incidental payload values merely because they happened to distinguish the captures used during development.

Merged master MR !13293, authored by John Thacker, fixes RSVD `SenseDataEx` decoding across protocol revisions. Versions 7.0 through 10.0 used a fixed 20-byte `SenseDataEx`, while later revisions reverted the structure. Because the packet has no explicit version indicator, the accepted dissector identifies the older layout from the version-dependent response structure/length (`Length == 36`, together with the corresponding sense-information length) and then dispatches to the matching decoder.

**Implementation rule:** first identify a specification-defined invariant that is independent of the data being interpreted differently between variants: total structure length, reserved field, mandatory discriminator, framing relationship, or another normative property. Use that invariant to choose the variant, then decode variant-specific fields. Do not make the value of one variant-dependent field prove which interpretation of that same field is correct.

**Review implication:** document which revisions define the discriminator and why the chosen invariant is unambiguous. Include representative packets for each supported layout when possible, plus malformed/ambiguous cases if the invariant can be violated on the wire.

**Confidence:** Very high. Merged master fix by John Thacker whose MR description and implementation explicitly use the specification's version-dependent fixed structure size to distinguish revisions lacking an explicit version marker.