# Wireshark Generated-Code Conventions

This file records durable conventions for generated source and CI validation. Current upstream generators and CI configuration remain authoritative.

## Regenerate generated dissectors in CI and reject hand edits to generated output

Generated dissector source is an artifact of its authoritative input and generator. Review should happen against those inputs; allowing committed generated output to drift independently creates changes that disappear or conflict the next time regeneration occurs.

Merged MR !22694, authored by John Thacker and merged by Anders Broman, adds CORBA IDL dissectors to the CI regeneration checks. The MR states the intended invariant directly: like Wireshark's other generated dissectors, CI should regenerate them and verify that contributors have not changed the generated code directly.

**Implementation rule:** when a generated source family is committed to the tree and its generator is available in CI, regenerate it from the authoritative inputs and fail on a diff. Fix the generator or source specification rather than patching generated output by hand.

**Confidence:** Extremely high. Merged master CI change authored by John Thacker and approved/merged by Anders Broman, with the generated-source invariant explicitly stated.

## Generate derivative metadata instead of maintaining parallel hand-written copies

When a dissector is already generated, metadata that can be derived reliably from the same authoritative input should be generated as part of that workflow rather than maintained in a separate hand-written file. Parallel manual copies create another source of drift and unnecessary intervention.

Merged master MRs !21852 and !21858 move SGP22 type/export information toward the generator output, and merged master MR !21860 removes a manually maintained PKIX implicit export configuration in favor of the generated export. During !21860 review, Michael Mann stated the architectural preference directly: if the dissectors are generated, he wants the least amount of manual intervention, and asked whether the corresponding explicit-PKIX exports could also be generated.

**Implementation rule:** if the generator has enough information to produce export tables, type metadata, or equivalent derivative configuration, teach the generator/build workflow to produce it rather than keeping a parallel hand-edited representation. Treat the authoritative specification plus generator as the source of truth.

**Confidence:** Very high. Multiple merged master generator cleanups, reinforced by explicit generated-code maintenance guidance from Michael Mann.