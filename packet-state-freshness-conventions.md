# Packet State Freshness Conventions

This file records durable rules for packet-scoped dissector state. Current upstream source remains authoritative.

## Packet scope controls lifetime; explicit reset controls freshness

Merged MR !6161 replaces CMS globals whose values pointed at packet-owned objects with a packet-scoped private-data structure. This prevents state from surviving beyond the packet that owns the referenced OID string or TVBuff.

The same change also clears `object_identifier_id` and `content_tvb` immediately before the individual decode operations that are expected to populate them. The MR explains that otherwise a failed or incomplete decode could reuse a value from an earlier CMS PDU in the same packet.

**Lifetime rule:** packet-derived pointers and TVBuffs belong in packet-scoped state unless their contents are copied into an independently owned longer-lived representation.

**Freshness rule:** allocator scope does not establish semantic freshness. If a packet can contain multiple PDUs or repeated substructures, reset scratch fields at the boundary where each new value is expected so a later decode cannot inherit an earlier same-packet value.

**Generated-source rule:** for generated dissectors, make the state change in the generator/template/configuration source and regenerate the derivative source in the same logical change.

**Confidence:** Very high. Merged correctness fix propagated from master to maintained release branches.


## Master provenance: CMS packet state originated in MR 5766

Merged master MR 5766, authored by John Thacker, is the source change behind this rule. The MR explicitly removes CMS globals because a non-fatal OID decode failure could otherwise leave a pointer or value from a previous packet, or even a previous file, available to a later entry point. Because CMS has many PDU entry points, clearing globals only at selected top-level paths was not reliable. The accepted implementation moves the OID and content TVBuff into packet-scoped protocol data and resets each scratch slot immediately before the decode operation expected to populate it.

Later release-branch MRs in the 6160 and 6161 family carry the same correction, so those changes are corroborating backports rather than the architectural origin.

**Provenance rule:** when a durable notebook convention was first established on master and later repeated in release branches, treat the master MR as the primary implementation precedent and use the backports as confirmation that the behavior was important enough to stabilize.

**Confidence:** Very high. The master change was authored by John Thacker and its stated bug mechanism directly matches the lifetime and freshness distinction.
