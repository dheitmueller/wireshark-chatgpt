# Wireshark Protocol Identity-Key Conventions

This file records durable conventions for choosing identifiers used to correlate, label, or key protocol state across dissector layers. Current upstream source remains authoritative.

## Choose an identifier whose uniqueness scope covers every consumer of the key

Protocols often expose several identifiers for the same logical endpoint at different hierarchy levels. The most local identifier is not necessarily safe to propagate downstream: if its uniqueness is only guaranteed within one parent entity, using it without that parent context can collide when a higher-level consumer combines several such entities.

Merged master MR !14262, authored and merged by Martin Mathieson, adds an F1AP-derived UE ID to PDCP-NR metadata. The initial implementation considered the gNB-DU UE F1AP ID. During review, Martin explicitly asked Pascal Quantin whether the DU or CU identifier was the correct choice. Pascal pointed out that one CU can control several DUs, so a DU-scoped identifier can duplicate across those DUs; he recommended the CU UE ID instead. The accepted diff captures `GNB-CU-UE-F1AP-ID` and propagates that value as the PDCP-NR `ueid`.

**Architecture rule:** before using a protocol identifier as a key outside the layer where it was defined, write down its uniqueness domain. The chosen key must be unique across the full set of objects that the downstream state/table/UI can combine. If no single identifier has the required scope, use a composite key that includes the parent identity rather than silently widening the assumptions attached to a local ID.

**Review rule:** when two candidate identifiers differ mainly by protocol hierarchy (device-local vs controller-wide, bearer-local vs UE-wide, interface-local vs chassis-wide, and similar), review the topology/cardinality rules before choosing the convenient field. Tests should include multiple parent entities where a local identifier could legitimately repeat.

**Confidence:** Very high. The collision risk was identified explicitly in review by Pascal Quantin, the author changed the implementation accordingly, and the merged diff uses the controller-scoped identifier.

## Preserve bus context in protocol identity keys

Merged master MR !3811 adds a bus ID to CAN state and changes Signal-PDU lookup from a CAN-ID-only key to CAN ID plus bus ID. This prevents the same numeric CAN ID on two buses from being treated as one identity. Bus ID zero remains an explicit fallback for mappings intended to match any bus.

**Architecture rule:** when a wire identifier is only unique within a bus or interface, include that context in state and lookup keys. If configuration supports a wildcard, make that wildcard explicit rather than discarding the context.

**Confidence:** High. Merged master multi-CAN support that directly demonstrates the uniqueness-scope rule.


## LIN frame IDs require bus context too

Merged !3689 predates and independently supports the later CAN multi-bus lesson. LIN has only a small frame-ID space and deployments commonly reuse the same numeric ID on multiple parallel buses. The accepted implementation adds bus identity to LIN metadata and Signal-PDU lookup keys; bus ID zero is an explicit wildcard fallback rather than an implicit loss of bus context.

**Architecture rule:** the same numeric identifier can be safe only inside its protocol-defined uniqueness domain. For LIN and similar bus protocols, include bus/interface context in state and dispatch keys, and encode wildcard behavior explicitly.

**Confidence:** Very high. Merged master architecture change, independently corroborated by later CAN multi-bus work.
