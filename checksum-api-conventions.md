# Wireshark Checksum API Conventions

This file records durable conventions for checksum presentation and verification. Current upstream APIs remain authoritative.

## Prefer the common checksum tree helper when it matches the protocol

When a packet carries a checksum that Wireshark can verify, prefer the common checksum protocol-tree API rather than manually constructing the checksum value, status field, and bad-checksum expert item independently. The shared helper keeps field presentation, verification status, and expert reporting aligned with the rest of Wireshark.

During review of merged master MR !8907, Jaap Keuter explicitly said the MongoDB OP_MSG checksum should use `proto_tree_add_checksum()`, and Alexis La Goutte agreed. The contributor changed the implementation accordingly. The MR also supplied focused captures for checksum-present valid/invalid cases and checksum-absent traffic.

**Implementation rule:** use `proto_tree_add_checksum()` or the corresponding checksum-bytes helper when its verification model matches the wire format. Keep protocol-specific checksum calculation separate if necessary, but feed the result through the standard presentation/status path instead of recreating it locally.

**Testing rule:** cover a valid checksum, an invalid checksum, and any protocol-defined checksum-absent case so both calculation and status presentation are exercised.

**Confidence:** Very high. Merged master change with the helper choice requested explicitly by Jaap Keuter and Alexis La Goutte; later already-reviewed checksum cleanups independently corroborate the same direction.


## Preserve the wire value while representing checksum validity as a separate semantic status

A checksum field containing a protocol-significant value such as zero should still display the actual wire value. Whether that value means absent, ignored, illegal, valid, or invalid belongs in the checksum-status semantics and Expert Info, not in a fabricated replacement field value.

Merged master MR !1830, authored by João Valverde, changes UDP zero-checksum presentation from a synthetic-looking "[missing]" form to the real wire value `0` plus either "[zero-value ignored]" or "[zero-value illegal]". It adds an explicit `PROTO_CHECKSUM_E_ILLEGAL` status and keeps the status item generated. Merged !1818 separately makes the IPv6 zero-checksum exception a user preference while retaining the standards-default behavior.

**Presentation rule:** show the bytes that are actually on the wire, then annotate their semantic status separately.

**Status rule:** distinguish "not present/ignored" from "present but illegal"; do not overload one checksum state merely because both cases skip normal verification.

**Preference rule:** when interoperability requires tolerating protocol-invalid checksum behavior, preserve the standards-conforming default and make the compatibility exception explicit/configurable.

**Confidence:** Very high. Two merged master changes, including a shared checksum-status API extension.
