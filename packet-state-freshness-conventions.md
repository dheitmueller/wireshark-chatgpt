# Packet State Freshness Conventions

This file records durable rules for packet-scoped dissector state. Current upstream source remains authoritative.

## Packet scope controls lifetime; explicit reset controls freshness

Merged MR !6161 replaces CMS globals whose values pointed at packet-owned objects with a packet-scoped private-data structure. This prevents state from surviving beyond the packet that owns the referenced OID string or TVBuff.

The same change also clears `object_identifier_id` and `content_tvb` immediately before the individual decode operations that are expected to populate them. The MR explains that otherwise a failed or incomplete decode could reuse a value from an earlier CMS PDU in the same packet.

**Lifetime rule:** packet-derived pointers and TVBuffs belong in packet-scoped state unless their contents are copied into an independently owned longer-lived representation.

**Freshness rule:** allocator scope does not establish semantic freshness. If a packet can contain multiple PDUs or repeated substructures, reset scratch fields at the boundary where each new value is expected so a later decode cannot inherit an earlier same-packet value.

**Generated-source rule:** for generated dissectors, make the state change in the generator/template/configuration source and regenerate the derivative source in the same logical change.

**Confidence:** Very high. Merged correctness fix propagated from master to maintained release branches.
