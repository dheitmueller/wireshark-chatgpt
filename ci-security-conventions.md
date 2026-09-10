# Wireshark CI Security and Failure Conventions

This file records durable security and failure-propagation conventions for Wireshark CI workflows. Current upstream workflow configuration remains authoritative.

## Do not persist checkout credentials when later build steps do not need them

CI checkout helpers can install repository credentials into the working copy by default even when the job only needs to read and build the source. Disable that behavior when subsequent steps do not require authenticated Git operations; this reduces the credential exposure available to arbitrary build/test steps.

Merged MR !24617, authored and merged by John Thacker, sets `persist-credentials: false` for `actions/checkout` throughout the release-4.4 GitHub workflows, backporting the same hardening from master. The change explicitly addresses checkout's default credential persistence rather than relying on job authors to remember that credentials have been written into Git configuration.

**Implementation rule:** grant CI credentials only for the operation and lifetime that need them. For read-only checkout/build jobs, disable persistent checkout credentials; if a later authenticated Git action is genuinely required, provide the narrow credential at that step instead.

**Confidence:** Very high. Merged security hardening authored and merged by John Thacker and applied consistently across multiple workflows.

## Make external-command failure explicit and preserve the diagnostic output

A CI step is only useful if failures from the command it is validating become job failures and the output explaining the failure remains visible. Shell wrappers—especially PowerShell/process-launch wrappers—must explicitly propagate exit status when the shell or launcher does not do so automatically.

Merged MR !24616, authored and merged by John Thacker, hardens the MSYS2 installer verification workflow by setting PowerShell's error policy, checking that the installer/executable exists, checking process and `$LASTEXITCODE` results, and explicitly exiting nonzero when installation or version probing fails. Merged master MR !24609 separately makes the version-failure path print the captured TShark output before returning the failing exit code.

**Implementation rule:** for every CI command used as a correctness gate, verify that the runner sees a nonzero status on failure and that captured stdout/stderr needed for diagnosis is emitted before exiting. Do not assume a process launcher, pipeline, or scripting language automatically maps the child command's failure into the CI step's status.

**Confidence:** Very high. Two merged John Thacker CI fixes addressing a concrete workflow that could fail opaquely or fail without being surfaced correctly.
