# Generated Source Follow-up Conventions

A correctness fix to checked-in generated dissector output is incomplete if the authoritative ASN.1, conformance, template, or generator source remains unchanged. Regeneration can otherwise discard a technically correct runtime fix.

Merged !10343, !10341, and !10330 each contained a legitimate generated-output correction. Guy Harris explicitly identified the source-of-truth problem in all three. Merged !10352, !10351, and !10353 repair the corresponding authoritative inputs; !10354–!10357 preserve the SPNEGO correction on maintained branches. !10320 and !10326 independently reinforce the same source/output parity requirement.

**Review rule:** when a patch touches generated output, identify the authoritative input during review. Require the source-side correction and regeneration; treat a missing source change as unfinished repair rather than optional cleanup.

**Confidence:** Extremely high. Three independent examples with direct Guy Harris guidance and merged source-side follow-ups.
