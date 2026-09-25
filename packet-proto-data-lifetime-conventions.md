# Packet Proto-Data Lifetime Conventions

This file records durable lifetime rules for objects stored with Wireshark packet proto-data APIs. Current upstream source remains authoritative.

## The proto-data container must not outlive objects stored inside it

Choosing a long-lived allocator for a proto-data entry does not extend the lifetime of pointers placed in that entry. If the stored object is valid only for the current packet, the proto-data entry that references it must use a compatible packet lifetime.

Merged master MR !9322, authored and merged by Gerald Combs, fixes GeoNetworking security TVBuff storage by replacing `p_add_proto_data(wmem_file_scope(), ...)` and `p_get_proto_data(wmem_file_scope(), ...)` with the packet allocator `pinfo->pool`. The value being stored is a TVBuff tied to packet dissection, so a file-scoped reference could survive beyond the object it referenced. Merged stable-branch backports !9339 and !9340 carry the same correction.

**Implementation rule:** choose proto-data allocator scope from the lifetime of the stored value and all objects reachable through it, not from how long it might be convenient to retain the container. A packet-derived TVBuff or packet-owned object normally implies packet scope unless it is explicitly copied or promoted into a longer-lived representation.

**API rule:** use the same semantic scope and key contract for `p_add_proto_data()` and `p_get_proto_data()`. Prefer a named key constant over repeating an unexplained numeric key when the entry has a stable role.

**Review rule:** audit long-lived proto-data for borrowed pointers. A file-scope container containing packet-scope children is still a lifetime bug even if the outer container itself remains valid.

**Confidence:** Very high. Merged master fix from Gerald Combs with two accepted stable-branch backports.
