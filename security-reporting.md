# Wireshark Security Reporting Conventions

This file records durable submission/reporting practices established by accepted upstream Wireshark changes. Current `SECURITY.md` remains authoritative.

## Security issue reports should be reproducible and minimally noisy

Merged MR !26017, authored by Gerald Combs and accepted by Jaap Keuter, expanded Wireshark's `SECURITY.md` with explicit reporting guidance:

- Use a confidential GitLab issue for application-security reports.
- Make the subject a concise summary of the issue; do not add superfluous prefixes such as `[Security]`.
- In the body, describe the problem and the steps required to reproduce it, along with other information requested by the issue template.
- A proposed fix or patch is appreciated but is not a substitute for a clear reproducer.
- Strongly prefer attaching a capture file that reproduces the issue. This speeds diagnosis and verification.
- Capture files attached to security issues are also used for fuzz testing. If a reproducer contains confidential information, state that explicitly.

**Submission rule:** for security-sensitive parser/dissector defects, treat a small reproducing capture plus exact reproduction steps as first-class review material. Prefer a concise factual title and keep the sensitive details inside the confidential report rather than encoding them in a decorative subject prefix.

**Confidence:** Very high. This is project policy text authored by Gerald Combs and merged into `SECURITY.md`.

## Do not infer a broad anti-test rule from one review

In merged MR !26014, Pascal Quantin considered the cell-broadcast language-indication change correct but questioned whether its proposed non-regression test was worth retaining, and preferred the final focused change as a single commit. This is useful evidence that test value is judged proportionally to the change and maintenance cost, but it should not override the much broader corpus evidence favoring captures and automated tests for behavior with meaningful regression, state, malformed-input, or integration risk.

**Review rule:** ask whether a proposed test protects a durable behavior or failure mode rather than adding tests mechanically. A maintainer declining one low-value regression test is context-specific, not a general reason to omit validation.

**Confidence:** Medium. Direct review feedback on a merged MR, deliberately scoped narrowly.