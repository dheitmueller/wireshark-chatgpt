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

## Keep duplicated frontend implementations behaviorally synchronized

When Wireshark and another frontend such as Logray contain copy-pasted implementations of the same user-facing behavior, a correctness fix to one copy must be audited against the other copy in the same submission. Leaving the duplicate untouched creates a predictable cross-frontend regression even when both applications compile successfully.

Merged master MR !9901 works around a Qt action-trigger crash by using a queued connection. Gerald Combs immediately asked for the corresponding Logray change, and John Thacker clarified that this was specifically required because Logray had copy-pasted the Wireshark implementation. The accepted MR updates both frontends. Merged !9912 supplies independent corroboration: a packet-byte export enablement cleanup was likewise applied to both Wireshark and Logray once the Logray build was available for testing.

**Implementation rule:** whenever changing duplicated frontend code, search for sibling copies in Wireshark, Logray, Stratoshark, or other maintained frontends and apply the semantic fix consistently where the same behavior exists. Treat parity as part of the change's correctness, not as optional follow-up cleanup.

**Architecture rule:** repeated parity fixes are evidence that shared behavior may deserve common ownership. Do not force an unrelated refactor into every bug fix, but prefer a shared helper/model when doing so can eliminate recurring copy-paste drift without obscuring frontend-specific policy.

**Confidence:** Very high. Two merged master changes, with explicit cross-frontend review from Gerald Combs and John Thacker.
