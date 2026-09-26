# Wireshark Checksum API Conventions

This file records durable conventions for checksum presentation and verification. Current upstream APIs remain authoritative.

## Prefer the common checksum tree helper when it matches the protocol

When a packet carries a checksum that Wireshark can verify, prefer the common checksum protocol-tree API rather than manually constructing the checksum value, status field, and bad-checksum expert item independently. The shared helper keeps field presentation, verification status, and expert reporting aligned with the rest of Wireshark.

During review of merged master MR !8907, Jaap Keuter explicitly said the MongoDB OP_MSG checksum should use `proto_tree_add_checksum()`, and Alexis La Goutte agreed. The contributor changed the implementation accordingly. The MR also supplied focused captures for checksum-present valid/invalid cases and checksum-absent traffic.

**Implementation rule:** use `proto_tree_add_checksum()` or the corresponding checksum-bytes helper when its verification model matches the wire format. Keep protocol-specific checksum calculation separate if necessary, but feed the result through the standard presentation/status path instead of recreating it locally.

**Testing rule:** cover a valid checksum, an invalid checksum, and any protocol-defined checksum-absent case so both calculation and status presentation are exercised.

**Confidence:** Very high. Merged master change with the helper choice requested explicitly by Jaap Keuter and Alexis La Goutte; later already-reviewed checksum cleanups independently corroborate the same direction.
