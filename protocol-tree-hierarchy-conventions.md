# Wireshark Protocol-Tree Hierarchy Conventions

This file records durable conventions for preserving structural parent/child relationships in protocol-tree output. Current upstream source remains authoritative.

## Keep child dissection beneath the tree context supplied by its parent

A dissector invoked on a payload or encapsulated protocol should attach its visible subtree to the structural context supplied by the caller rather than escaping to the packet-tree root. The distinction is normally invisible when every layer happens to be top-level, but it becomes important for nested copies, tunnels, error payloads, and other cases where a complete protocol stack is itself already inside another subtree.

Merged master MR !15669 fixes UDP payload handling that always added the payload subtree at the root. That produced misleading hierarchy when UDP itself represented an embedded packet, such as the quoted original datagram inside an ICMP Destination Unreachable message. During review Anders Broman explicitly questioned the tunneling/error-packet implications; the accepted change preserves UDP's existing placement and ensures its payload remains at that level or deeper rather than being promoted to the global root.

**Implementation rule:** treat the `proto_tree *` handed to a dissector or payload-dispatch path as part of the structural API contract. Add descendant protocol items and delegated payload dissection beneath that context unless the protocol architecture explicitly calls for a separate top-level representation. Do not reach for the packet root merely because that produces the same appearance for ordinary non-nested captures.

**Review/testing rule:** exercise encapsulated and quoted-packet cases, not just normal top-level traffic. Protocol-tree hierarchy can affect exports and machine consumers even when display filters continue to work, so a visually subtle nesting regression is still behaviorally significant.

**Confidence:** Very high. Merged master structural correction reviewed and merged by Anders Broman, with nested ICMP payloads supplied as the concrete failure case and the intended hierarchy clarified during review.