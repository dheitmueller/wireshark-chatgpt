# Protocol Default Enablement Conventions

Merged MR !7020 disables EOBI by default because it has no heuristic recognizer and claims several nonstandard high ports. The source generator is updated along with the generated dissector behavior.

**Rule:** a dissector that cannot positively identify its protocol should not broadly claim deployment-specific or unregistered ports by default. Prefer an assigned binding, Decode As/configuration, or disabled-by-default behavior until identification is sufficiently safe.

**Generator rule:** when generated dissector behavior changes, update the authoritative generator as well as regenerated output.

**Confidence:** High. Merged correctness/dispatch change.


## Assigned ports still require a false-positive review before broadening defaults

Merged master MR !4096 fixes IEEE 1722 AVTP over UDP. John Thacker initially proposed adding a second IANA-registered port to the default binding. Anders Broman objected that claiming more UDP traffic by default could cause misdissection in environments that do not use the protocol. The final MR retains the protocol fix but drops the new second-port default.

**Dispatch rule:** standards registration is strong evidence for a default port binding, but it is not conclusive. Before expanding an automatic binding, consider collision history, recognizer strength, and the cost of false positives; Decode As or explicit configuration can be safer even for an assigned port.

**Confidence:** Very high. Direct maintainer disagreement resolved by narrowing the merged MR's default-binding scope.
