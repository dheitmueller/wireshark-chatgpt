# Wireshark Reassembly Instance Identity Conventions

This file records durable conventions for distinguishing simultaneously active or nested reassembly instances. It complements the broader rules in `stateful-reassembly-conventions.md`; current upstream source remains authoritative.

## Reassembly identity must distinguish nested instances that can coexist under the same base protocol identifier

A protocol/session/message discriminator is not necessarily a complete reassembly key if the parser can recursively or repeatedly start more than one logical reassembly with that same discriminator before the earlier instance is finished. The identity must be unique over all reassembly instances that can be active at the same time, including multiple instances inside one frame.

Merged master MR !11113, authored and merged by John Thacker, fixes recursive BER OCTET STRING reassembly. `reassemble_octet_string()` can call `fragment_add_seq_next()` repeatedly within one frame and recursively at different ASN.1 nesting levels. Using only the 16-bit `dst_ref` therefore let separate nested reassemblies collide. The accepted code combines the bounded `nest_level` with `dst_ref` to form the sequence ID, preventing one nesting level from reusing another's reassembly key. The MR notes that these collisions could leak reassembly state, especially for single-fragment instances that do not create a replacement fragment-head tvbuff.

**Implementation rule:** enumerate every dimension that can distinguish concurrently active reassemblies, not merely the dimensions that distinguish packets or conversations. If recursive parsing, repeated sibling constructs, multiple logical PDUs in one frame, or another local parser scope can instantiate the same base message ID more than once, include a nesting/instance discriminator or use a structured reassembly key that represents that scope without collisions.

**Review implication:** test reassembly with more than one eligible instance in the same frame and, for recursive formats, at multiple nesting depths. A capture with one fragmented object per frame can hide a key that is globally unique across packets but not unique among simultaneous parser instances.

**Confidence:** Extremely high. Merged master correctness fix authored and merged by John Thacker; the collision mechanism, bounded nesting dimension, and resulting leaked state are documented directly in the MR.

## Out-of-order reassembly tests should cover both first-pass and redissection behavior

Reassembly fixes that work during a sequential first pass can still fail when Wireshark revisits packets in a second pass. For stateful out-of-order paths, representative captures should therefore exercise both modes when practical.

Merged release-4.0 MR !11114, authored and merged by John Thacker, fixes TCP out-of-order reassembly so that after an out-of-order segment is attached to a multi-segment PDU, already-added segments are reconsidered when computing the maximum contiguous next sequence. The MR adds a dedicated out-of-order capture and tests TLS-over-TCP reassembly both normally and with `tshark -2`, verifying that all expected HTTP responses are recovered. The corresponding master change is commit `e31067cb` from MR !11063, which should carry primary weight when that lower-numbered MR is reviewed.

**Testing rule:** for a stateful reassembly change involving reordering, retained fragment state, or derived sequence boundaries, include a representative out-of-order capture and exercise both the normal first pass and second-pass/redissection path. Treat a stable-branch backport as corroboration when the master MR is available separately.

**Confidence:** High for the testing pattern, with primary architectural weight deferred to the not-yet-reviewed master MR !11063.
