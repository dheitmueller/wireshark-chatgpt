# Wireshark CI Hygiene Conventions

This file records durable conventions for repository-hygiene checks in continuous integration. Current upstream CI configuration remains authoritative.

## Nonessential hygiene checks may begin as advisory when contributor tooling is uneven

A useful repository-hygiene check does not have to become a hard merge gate on day one. If satisfying it depends on uncommon local tools that contributors may not have, the check can report an actionable finding without failing the overall job while the ecosystem/tooling support matures.

Merged MR !24693, authored and merged by John Thacker, adds lossless PNG-compressibility checking to Commit Check but deliberately reports rather than gates the MR because contributors may lack the compression utilities. During development John also discovered that the CI image itself initially lacked those utilities, making the check inert until its environment was corrected.

**CI rule:** distinguish correctness gates from opportunistic hygiene improvements. For a nonessential optimization whose remediation tooling is not broadly available, prefer an explicit advisory result with clear remediation over an avoidable hard failure. Even an advisory check must actually execute in CI: ensure the job image contains or can invoke the checker dependencies.

**Confidence:** Very high for the advisory-rollout pattern. Merged master CI/tooling change authored and merged by John Thacker with the tool-availability constraint documented in the MR.
