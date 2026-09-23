# Wireshark Shared-Helper Ownership Conventions

This file records durable conventions for deciding where reusable packet-display/parsing helpers belong and how their public API should be named. Current upstream source remains authoritative.

## Put protocol-neutral behavior in the common layer that owns the abstraction

A helper should not remain owned by the first dissector that happened to implement it when its behavior is actually generic. If multiple protocols need the same semantic operation, move the operation to the common layer that owns that abstraction and give it a name consistent with neighboring public APIs.

Merged master MR !11967 began by sharing Ethernet address display logic with IEEE 802.11. Guy Harris explicitly pointed out that the helper contained nothing specific to IEEE 802.3, so it should not live in `packet-eth.c`; he also noted that non-dissector-specific protocol-tree helpers conventionally use `proto_tree_add_XXX` naming. The accepted MR ultimately places `proto_tree_add_mac48_detail()` in `epan/proto.c` and uses it from both Ethernet and IEEE 802.11, centralizing name/OUI resolution plus I/G and U/L bit presentation.

Guy additionally called out the terminology issue: MAC-48 addressing is broader than Ethernet. That distinction is reflected in the accepted helper name rather than baking an Ethernet-specific term into a protocol-neutral API.

**Architecture rule:** locate a shared helper according to the abstraction it implements, not according to the module where its code originated. Protocol-neutral protocol-tree behavior belongs in a common protocol-tree/API layer when doing so avoids duplicated behavior and presentation drift.

**Naming rule:** public helper names should follow the conventions of the layer that owns them and should describe the actual semantic domain. Avoid protocol-specific terminology in an API that is intended for multiple protocols when a more accurate generic term exists.

**Review rule:** when extracting shared code, review more than mechanical deduplication. Check whether the helper's behavior, terminology, field ownership, and public location remain valid for every new caller; a helper that is only superficially common can create cross-protocol coupling.

**Confidence:** Extremely high. The architectural placement and naming direction came from substantive Guy Harris review, and the final merged implementation follows that direction.