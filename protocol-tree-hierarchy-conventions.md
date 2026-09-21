# Wireshark Protocol-Tree Hierarchy Conventions

This file records durable conventions for preserving structural parent/child relationships in protocol-tree output. Current upstream source remains authoritative.

## Keep child dissection beneath the tree context supplied by its parent

A dissector invoked on a payload or encapsulated protocol should attach its visible subtree to the structural context supplied by the caller rather than escaping to the packet-tree root. The distinction is normally invisible when every layer happens to be top-level, but it becomes important for nested copies, tunnels, error payloads, and other cases where a complete protocol stack is itself already inside another subtree.

Merged master MR !15669 fixes UDP payload handling that always added the payload subtree at the root. That produced misleading hierarchy when UDP itself represented an embedded packet, such as the quoted original datagram inside an ICMP Destination Unreachable message. During review Anders Broman explicitly questioned the tunneling/error-packet implications; the accepted change preserves UDP's existing placement and ensures its payload remains at that level or deeper rather than being promoted to the global root.

**Implementation rule:** treat the `proto_tree *` handed to a dissector or payload-dispatch path as part of the structural API contract. Add descendant protocol items and delegated payload dissection beneath that context unless the protocol architecture explicitly calls for a separate top-level representation. Do not reach for the packet root merely because that produces the same appearance for ordinary non-nested captures.

**Review/testing rule:** exercise encapsulated and quoted-packet cases, not just normal top-level traffic. Protocol-tree hierarchy can affect exports and machine consumers even when display filters continue to work, so a visually subtle nesting regression is still behaviorally significant.

**Confidence:** Very high. Merged master structural correction reviewed and merged by Anders Broman, with nested ICMP payloads supplied as the concrete failure case and the intended hierarchy clarified during review.

## Do not collapse distinct mutable tree items onto the same node

Protocol-tree optimizations may omit work for items whose representation is not needed, but they must preserve node identity whenever callers can subsequently mutate or inspect an item's length, representation, parentage, or other per-item state. Returning a parent's node as a stand-in for a logically distinct child makes later operations ambiguous: a call intended for the child can silently modify the parent instead.

Merged master MR !14203, authored by John Thacker, fixes this in the protocol-tree core. When a `field_info` is visible, Wireshark permits its representation and length to be adjusted after construction. The old fake-child optimization could return the existing parent node for a child, leaving no way to distinguish a later `proto_item_set_len()`/representation change intended for the child from one intended for the parent. This also broke consumers such as protocol-hierarchy statistics that depend on the final protocol-item length.

**Implementation rule:** an optimization may suppress allocation only when the observable semantics of the omitted object are provably irrelevant. If a tree item is visible, referenced, or otherwise needs independent mutable metadata, create a distinct node even if the rendered tree could appear identical without it.

**Review/testing rule:** when changing tree-faking or hidden-item logic, test not only display output but also late length/representation adjustment, referenced descendant fields, and consumers of protocol-item ranges such as hierarchy/statistics code.

**Confidence:** Very high. Merged master protocol-tree correctness fix authored by John Thacker, with the identity ambiguity and downstream range consumer stated directly in the MR.