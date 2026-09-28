# CI Publication Scope Conventions

This file records durable CI placement and publication-scope rules from accepted Wireshark changes. Current upstream CI remains authoritative.

## Keep high-signal validation pre-merge while deferring expensive artifact production

Merged MR !6205, authored by Gerald Combs, moves the slow Ubuntu package-production job to post-merge, adds a package test job, and moves the latest-Clang build into merge-request pipelines because that compiler had been finding defects relevant to macOS.

**CI rule:** place jobs according to both cost and defect-detection value. Expensive final artifact production can be deferred when a cheaper pre-merge job still exercises the relevant packaging contract. A compiler or configuration with demonstrated cross-platform detection value belongs in pre-merge coverage even if a superficially similar job already exists.

**Confidence:** Very high. Merged CI restructuring by Gerald Combs with the cost and coverage rationale stated directly.

## Publication destinations must distinguish maintained release lines

Merged release-branch MRs !6192 and !6193 disable documentation publication until versioned documentation can be introduced because those release jobs would otherwise overwrite the master documentation.

**Publishing rule:** a CI job that publishes persistent documentation or artifacts must encode branch/version identity in its destination when multiple maintained branches can run it. If the destination cannot distinguish those lines, disable publication rather than allowing an older release branch to replace the canonical/current output.

**Confidence:** Very high. The same accepted safeguard was applied by Gerald Combs to both maintained release branches.
