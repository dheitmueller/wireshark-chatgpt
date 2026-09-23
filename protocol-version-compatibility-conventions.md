# Protocol version compatibility conventions

This file records durable review conventions for protocol specifications whose drafts or released revisions reuse or redefine on-wire values. Current upstream behavior and authoritative specifications remain authoritative.

## Verify reassigned values against released specifications and capture compatibility

When a newer protocol specification assigns a numeric message, TLV, enum, or flag value that conflicts with an older Wireshark definition, do not resolve the collision by mechanically preserving both names or deleting the older interpretation. First determine whether the older definition belonged to a released interoperable version, a draft/pre-release allocation, or an implementation-specific extension, and explicitly consider how the change affects existing captures.

Merged master MR !12314 updates the IEEE 1905/EasyMesh dissector for EasyMesh 5.0 and removes older TLV definitions whose numeric IDs collide with the current specification. Alexis La Goutte explicitly challenged the removal because it could break dissection of old pcaps. Albert Chuang responded by checking EasyMesh 3.0, 4.0, and 5.0 plus the project/history of the older definitions; the conflicting entries came from work predating the 3.0 release and had already been removed in the Wi-Fi Alliance branch as later specifications assigned those values differently. That provenance resolved the compatibility concern and the MR was merged.

**Review rule:** for protocol-value reassignment, ask for the authoritative released specifications and, where necessary, the history of the old definition. Treat an old Wireshark constant as evidence that captures may exist, not as proof that the value was ever standardized.

**Compatibility rule:** preserve old interpretation when it represents a real supported protocol version that can still be distinguished reliably. If the old meaning came only from a superseded draft and now collides indistinguishably with the released specification, prefer the authoritative released meaning after documenting the provenance and compatibility tradeoff.

**Testing rule:** when distinguishable old and new versions remain supported, include captures for both. When they cannot be distinguished because the same wire value was reassigned, document that limitation so reviewers understand why simultaneous decoding is impossible rather than silently choosing one interpretation.

**Confidence:** High. Merged master specification update with an explicit maintainer compatibility challenge and a standards/history-based answer from a Wi-Fi Alliance contributor before acceptance.

## When the specification is incomplete or conflicting, preserve interoperable deployed variants when evidence supports them

A dissector should not manufacture certainty that the protocol ecosystem does not have. If the available specification is incomplete or contradictory and independent implementation/history evidence shows multiple wire encodings in use for the same semantic operation, accepting the known deployed variants can be more correct than rejecting one solely to force a single reading of an uncertain document.

Merged master MR !11820, authored by Guy Harris, changes the PGM dissector to recognize both `0x0b` and `0x0d` as packet-type values for a PGMCC ACK. The MR and in-code commentary trace the ambiguity to incomplete protocol documentation and implementation history rather than treating either value as a random malformed alternative. The accepted behavior therefore recognizes both established encodings.

**Protocol rule:** distinguish ambiguity in the specification from malformed traffic. Where credible implementation/source/history evidence establishes multiple deployed encodings and the dissector can recognize them without creating harmful false positives, support the variants and document why they exist.

**Review rule:** do not broaden recognition merely because a sample exists. Ask for provenance: standards drafts, reference implementations, historical code, vendor behavior, or captures that demonstrate interoperability. High-authority maintainer analysis can justify compatibility behavior when the formal specification is genuinely insufficient, but the uncertainty and evidence should remain visible in comments or review history.

**Confidence:** Extremely high for the concrete PGMCC behavior and high for the general rule. The merged master change was authored by Guy Harris and explicitly framed around incomplete specification evidence rather than convenience.
