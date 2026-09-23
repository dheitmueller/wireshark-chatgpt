# Protocol version compatibility conventions

This file records durable review conventions for protocol specifications whose drafts or released revisions reuse or redefine on-wire values. Current upstream behavior and authoritative specifications remain authoritative.

## Verify reassigned values against released specifications and capture compatibility

When a newer protocol specification assigns a numeric message, TLV, enum, or flag value that conflicts with an older Wireshark definition, do not resolve the collision by mechanically preserving both names or deleting the older interpretation. First determine whether the older definition belonged to a released interoperable version, a draft/pre-release allocation, or an implementation-specific extension, and explicitly consider how the change affects existing captures.

Merged master MR !12314 updates the IEEE 1905/EasyMesh dissector for EasyMesh 5.0 and removes older TLV definitions whose numeric IDs collide with the current specification. Alexis La Goutte explicitly challenged the removal because it could break dissection of old pcaps. Albert Chuang responded by checking EasyMesh 3.0, 4.0, and 5.0 plus the project/history of the older definitions; the conflicting entries came from work predating the 3.0 release and had already been removed in the Wi-Fi Alliance branch as later specifications assigned those values differently. That provenance resolved the compatibility concern and the MR was merged.

**Review rule:** for protocol-value reassignment, ask for the authoritative released specifications and, where necessary, the history of the old definition. Treat an old Wireshark constant as evidence that captures may exist, not as proof that the value was ever standardized.

**Compatibility rule:** preserve old interpretation when it represents a real supported protocol version that can still be distinguished reliably. If the old meaning came only from a superseded draft and now collides indistinguishably with the released specification, prefer the authoritative released meaning after documenting the provenance and compatibility tradeoff.

**Testing rule:** when distinguishable old and new versions remain supported, include captures for both. When they cannot be distinguished because the same wire value was reassigned, document that limitation so reviewers understand why simultaneous decoding is impossible rather than silently choosing one interpretation.

**Confidence:** High. Merged master specification update with an explicit maintainer compatibility challenge and a standards/history-based answer from a Wi-Fi Alliance contributor before acceptance.