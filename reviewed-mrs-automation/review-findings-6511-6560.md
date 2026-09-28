# Wireshark MR review findings !6511-!6560

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

The exact 50-MR reviewed set is recorded in `reviewed-mrs-automation-6511-6560.md`. This file records the strongest reusable findings from the batch; routine maintenance MRs were scanned but are not repeated here.

- !6560: retained display-filter field values need copies whose ownership matches the field type when they outlive the protocol tree that produced them.
- !6559: Alexis La Goutte requested a representative capture and complete visibility for a structured option word, including reserved bits.
- !6547: generated ASN.1 dissector corrections belong in the authoritative template/configuration or generator path, not only in derived output.
- !6542, !6525, !6535: a protocol field can be valid and present at zero length even when a presentation helper requires nonempty input.
- !6530, !6531: after specialized dispatch or reassembly, an unknown sibling subtype should remain visible as raw payload rather than disappearing.
- !6522: John Thacker distinguished recoverable specification violations from conditions that prevent reliable continued parsing; diagnostics should preserve that distinction.
- !6516 and !6534: moving a primary CI platform to a new dependency major should retain deliberate coverage for an older major that remains supported.
- !6518: a new display-filter operator spans syntax, typed capability checks, execution behavior, diagnostics, and tests; parser acceptance alone is not sufficient.

Closed/unmerged !6558, !6528, and !6523 were down-weighted and were not treated as accepted implementation precedent.
