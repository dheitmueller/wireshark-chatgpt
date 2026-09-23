# Wireshark Protocol Context Fallback Conventions

This file records durable conventions for obtaining equivalent protocol context from multiple capture/dissection layers. Current upstream source remains authoritative.

## Extend context lookup with ordered fallback instead of replacing an established source

The same semantic context can be available from different protocol layers depending on where a capture was taken. Adding support for a new capture layer should not silently replace an established lookup source and regress existing capture modes. Preserve the existing provider and add the new source through an explicit, deterministic fallback order unless the protocol semantics require a user-selectable choice.

Merged master MR !12834 extends NR-RRC UE-ID lookup so RRC configuration works for RLC- and PDCP-level captures as well as the existing MAC-level captures. Martin Mathieson pointed out that replacing MAC lookup with RLC would break DCT2000 and other users that deliberately attach UE identity at MAC level, and suggested a helper that checks MAC, then RLC, then PDCP. Pascal Quantin reinforced the compatibility requirement directly: do not break existing behavior for the new use case; either use ordered fallback or make the source an explicit preference. The accepted implementation uses the ordered MAC → RLC → PDCP fallback and was tested with both MAC and RLC captures.

**Architecture rule:** when several layers can provide equivalent context, centralize lookup policy in one helper and make precedence explicit. Prefer preserving the historically authoritative source first and falling back only when it is absent, rather than scattering layer-specific probes through generated handlers or individual call sites.

**Testing rule:** exercise both the newly supported capture level and the previously supported one. A change that proves RLC/PDCP-only captures now work is incomplete if it does not also protect the established MAC-based path from regression.

The same review also reiterates Wireshark's generated-dissector workflow: for ASN.1 dissectors, persistent custom logic belongs in the template/conformance inputs and the generated source is then regenerated. That convention already exists in `generated-code-conventions.md`, so it is not duplicated here.

**Confidence:** Very high. Merged master change with direct protocol-expert guidance from Martin Mathieson and maintainer review from Pascal Quantin that changed the accepted design.
