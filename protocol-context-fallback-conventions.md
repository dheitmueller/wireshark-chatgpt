# Wireshark Protocol Context Fallback Conventions

This file records durable conventions for obtaining equivalent protocol context from multiple capture/dissection layers. Current upstream source remains authoritative.

## Extend context lookup with ordered fallback instead of replacing an established source

The same semantic context can be available from different protocol layers depending on where a capture was taken. Adding support for a new capture layer should not silently replace an established lookup source and regress existing capture modes. Preserve the existing provider and add the new source through an explicit, deterministic fallback order unless the protocol semantics require a user-selectable choice.

Merged master MR !12834 extends NR-RRC UE-ID lookup so RRC configuration works for RLC- and PDCP-level captures as well as the existing MAC-level captures. Martin Mathieson pointed out that replacing MAC lookup with RLC would break DCT2000 and other users that deliberately attach UE identity at MAC level, and suggested a helper that checks MAC, then RLC, then PDCP. Pascal Quantin reinforced the compatibility requirement directly: do not break existing behavior for the new use case; either use ordered fallback or make the source an explicit preference. The accepted implementation uses the ordered MAC → RLC → PDCP fallback and was tested with both MAC and RLC captures.

**Architecture rule:** when several layers can provide equivalent context, centralize lookup policy in one helper and make precedence explicit. Prefer preserving the historically authoritative source first and falling back only when it is absent, rather than scattering layer-specific probes through generated handlers or individual call sites.

**Testing rule:** exercise both the newly supported capture level and the previously supported one. A change that proves RLC/PDCP-only captures now work is incomplete if it does not also protect the established MAC-based path from regression.

The same review also reiterates Wireshark's generated-dissector workflow: for ASN.1 dissectors, persistent custom logic belongs in the template/conformance inputs and the generated source is then regenerated. That convention already exists in `generated-code-conventions.md`, so it is not duplicated here.

**Confidence:** Very high. Merged master change with direct protocol-expert guidance from Martin Mathieson and maintainer review from Pascal Quantin that changed the accepted design.

## Prefer protocol-discovered identity over values inferred from conventional transport mappings

A default port formula can be a useful fallback, but it is weaker evidence than identity/context explicitly learned from protocol discovery. Non-default port mappings are common enough that a derived value should not overwrite or outrank discovered state.

Merged master MR !9654 changes RTPS DomainId handling so the dissector stores discovered DomainId values keyed by participant GUID for both TCP and UDP. Later packets first consult that discovered participant state. Only when no DomainId has been discovered does UDP fall back to the standard port-derived mapping; the tree text explicitly marks that calculated DomainId as potentially inaccurate. TCP, where the port formula is not valid, remains unknown until discovery supplies the value.

**Context rule:** when the protocol itself can advertise or establish an identifier, use that state as the authoritative source. Keep transport/port arithmetic as a documented fallback only in contexts where the specification permits it, and make inferred values visibly distinguishable when they can be wrong under non-default deployment choices.

**Confidence:** High. Merged master correctness fix whose accepted data flow explicitly orders discovered state ahead of port-derived inference.
