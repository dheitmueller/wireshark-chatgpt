# Wireshark Generated-Code Conventions

This file records durable conventions for generated source and CI validation. Current upstream generators and CI configuration remain authoritative.

## Regenerate generated dissectors in CI and reject hand edits to generated output

Generated dissector source is an artifact of its authoritative input and generator. Review should happen against those inputs; allowing committed generated output to drift independently creates changes that disappear or conflict the next time regeneration occurs.

Merged MR !22694, authored by John Thacker and merged by Anders Broman, adds CORBA IDL dissectors to the CI regeneration checks. The MR states the intended invariant directly: like Wireshark's other generated dissectors, CI should regenerate them and verify that contributors have not changed the generated code directly.

**Implementation rule:** when a generated source family is committed to the tree and its generator is available in CI, regenerate it from the authoritative inputs and fail on a diff. Fix the generator or source specification rather than patching generated output by hand.

**Confidence:** Extremely high. Merged master CI change authored by John Thacker and approved/merged by Anders Broman, with the generated-source invariant explicitly stated.
