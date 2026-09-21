# Wireshark Truncated-Packet Presentation Conventions

This file records durable conventions for building packet-list and protocol-tree summary text when a packet may be truncated or malformed. Current upstream source remains authoritative.

## Publish summary text only after the corresponding field has been decoded

A dissector should not fetch a collection of later fields up front merely so it can build the complete Info-column or top-level protocol-item text in one operation. If the frame is cut short, an early fetch can throw before earlier valid fields have been presented, and precomputed summary text can imply that fields were successfully decoded when they were not.

Merged master MR !14351, authored and merged by Guy Harris, changes SocketCAN CAN XL so priority/VCID and length are appended to the Info column and top-level protocol item as each field is added to the protocol tree. The commit message states the intent explicitly: if the frame is cut short, dissect as much as possible and put as much as safely decoded into the Info column and top-level item as possible. Release-4.2 MR !14352 carries the same behavior. Merged master !14348 and release backport !14350 independently reinforce the same sequencing rule by moving appended text until after the underlying value has been placed in the tree.

**Implementation rule:** build presentation progressively in parse order. Add or append column/protocol-item text only after the field extraction or tree-add operation that establishes the value has succeeded. When the displayed value is also needed by control flow or summary text, prefer the appropriate `proto_tree_add_item_ret_*` helper so the tree and the derived presentation consume one bounds-checked extraction.

**Review rule:** test a fully captured packet and truncations at several field boundaries. Earlier fields should remain visible, later undecoded fields should not appear in summaries, and a truncation should not erase useful presentation that was already safely decoded.

**Confidence:** Extremely high. The master change and stable backport were authored/merged by Guy Harris, and the rationale is explicit in the accepted commit history; adjacent merged SocketCAN changes corroborate the same pattern.
