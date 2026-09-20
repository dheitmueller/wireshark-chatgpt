# Wireshark Protocol-Tree Ownership Conventions

This file records durable conventions for protocol IDs used to own and organize protocol-tree content. Current upstream source remains authoritative.

## Unknown protocol content still needs a valid registered protocol owner

A protocol ID of zero is not a generic "unknown protocol" sentinel for APIs that require a registered protocol identity. If a dissector must create a protocol-owned tree for an unknown or unregistered protocol variant, provide a valid registered fallback owner rather than passing an invalid protocol ID through tree APIs.

Merged master MR !15556, authored and merged by Guy Harris, fixes ONC RPC handling for unknown RPC programs. The old path set `proto_id = 0` and then used that value for the tree containing the program version and procedure number, which could trigger an assertion failure. The accepted fix registers an explicit `Unknown RPC protocol` protocol and uses that protocol ID for the fallback tree.

**Implementation rule:** distinguish semantic "unknown" from invalid internal identity. Unknown input is an ordinary runtime condition and should map to a valid fallback representation when downstream APIs require a registered protocol, field, subtree, handle, or other registry object. Do not overload zero or another invalid registry ID unless the called API explicitly documents that sentinel.

**Review rule:** follow fallback/error paths through the same API contracts as the recognized path. A branch that handles unknown protocol numbers may still have to satisfy tree-ownership and registration invariants even though it cannot name the concrete protocol.

**Confidence:** Extremely high. This is a merged master correctness fix authored and merged by Guy Harris with an explicit assertion failure and an unambiguous accepted replacement.
