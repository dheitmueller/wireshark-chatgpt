# Wireshark TVBuff Persistent-State Lifetime Conventions

This file records durable ownership rules for tvbuff-backed data kept across packets. Current upstream source remains authoritative.

## Persist backing bytes rather than transient tvbuff wrappers across packets

A tvbuff wrapper and the bytes it references have separate ownership semantics. A real-data tvbuff is not automatically reclaimed merely because the backing bytes use a managed allocator, and retaining a packet-oriented wrapper in file-scoped state can give it the wrong lifetime. When only the bytes must survive for later packets, persist the bytes at the required scope and construct a packet-lifetime child tvbuff when they are consumed.

Merged master MR !10663, authored and merged by John Thacker, fixes FiveCo Legacy request tracking. The old file-scoped table retained `tvb_new_real_data()` objects, which were not automatically freed. The accepted code stores only the copied request bytes in file scope, creates `tvb_new_child_real_data()` from those bytes on demand for the packet that needs to inspect them, and lets the child wrapper follow the current packet tvbuff's lifetime. The change also replaces a manually reset GLib hash with an autoreset wmem map whose container lifetime matches the capture lifecycle.

**Implementation rule:** decide independently how long the backing bytes and the tvbuff view must live. If cross-packet state needs persistent bytes but not a persistent tvbuff object, store the bytes in file/conversation scope and build a child/subset tvbuff for each consuming packet so wrapper cleanup remains automatic.

**Review rule:** when a long-lived table stores a `tvbuff_t *`, ask what frees the tvbuff itself, what owns its backing bytes, and whether the wrapper really needs the same lifetime as the stored data. Managed backing memory does not by itself confer managed lifetime on a separately created tvbuff wrapper.

**Confidence:** Very high. Merged master memory-lifetime fix authored and merged by John Thacker with the ownership failure stated directly in the MR.

## Packet-derived child TVBs stored in proto data need packet lifetime

Merged master !9322, authored by Gerald Combs (corroborated by release backports !9339 and !9340), fixes GNW secured-payload parsing: a child subset TVB was passed to p_add_proto_data with wmem_file_scope(), even though the TVB view belongs to a particular dissection packet. The accepted registration passes pinfo->pool as the allocator and names a local SEC_TVB_KEY, matching the stored view's lifetime.

**Rule:** choose the allocator for packet-protocol data according to the referenced object's lifetime, not a convenient capture-wide arena. Packet-attached transient TVB wrappers belong with pinfo->pool; if bytes truly need to survive into other packets, persist independent backing bytes at the appropriate longer scope and reconstruct a packet-owned view. **Confidence:** extremely high (Gerald Combs master fix plus two merged backports).
