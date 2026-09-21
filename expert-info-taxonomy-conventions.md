# Wireshark Expert-Info Taxonomy Conventions

This file records durable conventions for choosing expert-info groups and keeping those categories available consistently across Wireshark APIs. Current upstream source remains authoritative.

## Classify diagnostics by the layer that originated the condition

Expert information is not only a severity level; its group should describe the kind of condition being reported. Capture-receive failures and interface/device events are semantically different from malformed protocol data, even when both ultimately produce a warning or error in the packet view.

Merged master MR !14255 was authored and merged by Guy Harris and adds two explicit expert groups. `PI_RECEIVE` is for indications associated with receiving packets, such as CRC errors and short/long-frame indications. `PI_INTERFACE` is for interface indications not tied to packet reception itself, such as out-of-buffer conditions, hardware errors, and link-speed changes. The accepted change also exposes the new groups through WSLua and updates the surrounding documentation comments rather than adding C-only enum values that scripting users cannot select.

**Architecture rule:** choose an expert group from the semantic source of the condition, not merely from the protocol currently owning the tree item. Packet-receive status belongs in a receive-oriented group; device/interface status belongs in an interface-oriented group; protocol-structure failures remain in protocol/malformed-style groups as appropriate. Severity (`PI_NOTE`, `PI_WARN`, `PI_ERROR`, etc.) is a separate axis.

**API rule:** when a public expert taxonomy is extended, carry the new category through all supported front ends and bindings that expose that taxonomy, including WSLua and developer documentation where applicable. Do not leave scripting/plugin APIs with a smaller or stale category set unless that limitation is intentional and documented.

**Confidence:** Extremely high. The merged change was authored, iterated, approved, and merged by Guy Harris, and its descriptions explicitly define the intended semantic boundary between receive and interface indications.