# Protocol Default Enablement Conventions

Merged MR !7020 disables EOBI by default because it has no heuristic recognizer and claims several nonstandard high ports. The source generator is updated along with the generated dissector behavior.

**Rule:** a dissector that cannot positively identify its protocol should not broadly claim deployment-specific or unregistered ports by default. Prefer an assigned binding, Decode As/configuration, or disabled-by-default behavior until identification is sufficiently safe.

**Generator rule:** when generated dissector behavior changes, update the authoritative generator as well as regenerated output.

**Confidence:** High. Merged correctness/dispatch change.
