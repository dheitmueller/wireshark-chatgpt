# Wireshark CI Source Identity Conventions

This file records durable CI commit-identity guidance extracted from accepted upstream Wireshark changes. Current upstream CI configuration remains authoritative.

## Distinguish an MR's source commit from GitLab's synthetic merge-result commit

A merge-request pipeline can run on a temporary commit synthesized by GitLab from the source branch and target branch. That commit is useful for integration testing, but it is not the contributor's source commit and must not be used when a test, report, artifact name, or lookup needs the identity of the submitted revision itself.

Merged MR !24783, authored and merged by John Thacker, fixes Wireshark's CI tests to use `CI_MERGE_REQUEST_SOURCE_BRANCH_SHA` for merge-request pipelines and fall back to `CI_COMMIT_SHA` for pipeline types where the MR-specific variable is empty. The MR explicitly notes that `HEAD` and `CI_COMMIT_SHA` identify the temporary merged commit in an MR pipeline, while the source-branch SHA is the revision the test intends to identify.

**Implementation rule:** decide whether a CI operation needs the integration-test commit or the contributor/source commit. In GitLab MR pipelines, use the MR source SHA for source identity; use the synthetic merge-result SHA only when intentionally testing or referring to the merged result. Provide an explicit fallback for non-MR pipelines where MR variables are unset.

**Confidence:** Extremely high. Merged master CI correction authored and merged by John Thacker, with the identity distinction stated directly in the change rationale.
