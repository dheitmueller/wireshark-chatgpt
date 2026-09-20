# Wireshark Merge-Request CI Context Conventions

This file records durable conventions for CI logic that operates on merge-request metadata and repository identity. Current upstream CI configuration remains authoritative.

## Use merge-request-specific identity for merge-request-scoped API queries

Generic CI project variables describe the project in which the job is executing; they are not automatically the correct identity for every merge-request API operation. In pipelines involving forks or other cross-project MR contexts, API paths must use the variables whose semantics match the merge request itself.

Merged master MR !15659, authored and merged by Gerald Combs, fixes Wireshark's MR commit-count query by replacing `CI_PROJECT_ID` with `CI_MERGE_REQUEST_PROJECT_ID` when querying `/projects/.../merge_requests/$CI_MERGE_REQUEST_IID/commits`. The accepted command also uses silent curl output so the value passed to `jq` is the API response rather than transfer-progress noise.

**Implementation rule:** when CI code calls the GitLab API for an MR, distinguish execution-project identity, source-project identity, and merge-request project identity. Select the variable defined for the API object being queried instead of assuming the current job's `CI_PROJECT_ID` denotes that object. This matters especially for contributions originating from forks.

**Review/testing rule:** validate MR-specific CI helpers on fork-originated merge requests as well as same-project branches. A helper that works only when all project IDs coincide can remain latent until an external contributor triggers it.

**Confidence:** Extremely high. Merged master CI fix authored and merged by Gerald Combs, whose project-wide CI guidance is exceptionally authoritative, with the incorrect and corrected GitLab variables visible directly in the accepted diff.