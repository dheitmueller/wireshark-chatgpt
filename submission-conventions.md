# Wireshark Submission Conventions

This file records durable merge-request and commit-presentation conventions extracted from upstream maintainer review. Current upstream contribution guidance and CI checks remain authoritative.

## Put the dissector/component name before the colon in commit and MR subjects

Wireshark subjects use the component being changed as the prefix. For dissector work, the protocol/dissector name belongs before the colon; a generic action prefix such as `fix:` is ambiguous and does not identify the affected component.

Merged MR !26145 began with a generic `fix:` subject. John Thacker explicitly asked the contributor to change both the MR title and the commit subject to the form `QUIC: Fix length of payload buffer passed to DATAGRAM dissector`, explaining that Wireshark requires the dissector name before the colon and that `FIX` itself is a protocol/dissector name (Financial Information Exchange). The contributor updated the title/commit and the MR was merged by John.

**Submission rule:** use `Component-or-Dissector: brief summary` for the subject, with the actual Wireshark component/protocol before the colon. Do not use generic conventional-commit prefixes such as `fix:` in place of the component name.

This sharpens the existing notebook rule that commit messages use a short `component: brief summary` subject followed by a blank line before any longer body.

**Confidence:** Very high. Direct maintainer instruction on a merged first-contribution MR, with the requested form adopted before merge.

## A green pipeline only validates the code actually present in the MR diff

Passing CI is not evidence for a feature if rebasing, force-pushing, or commit reconstruction accidentally drops the feature from the submitted diff. Before declaring an MR ready, inspect the current GitLab diff itself and verify that it contains the intended implementation, its tests, and no unrelated or stale changes.

In still-open MR !26280, the contributor reported a clean rebase and green CI for address-masking work. Pascal Quantin inspected the actual MR diff and pointed out that it contained only an unrelated `packet-trdp.c` include change, so CI was not testing the claimed feature at all. Later review by Pascal and Jaap Keuter also identified unrelated changes and accidental removal of existing CSV formula-escaping behavior.

**Submission rule:** after rebases, force-pushes, or history cleanup, review the MR's current diff as a reviewer would. Confirm that all intended files and tests remain, unrelated changes are absent, and existing behavior outside the feature scope has not been silently reverted. Treat pipeline status as validation of that verified diff, not as a substitute for verifying the diff.

**Confidence:** High for the submission/review rule. The implementation MR remained open and unresolved in the reviewed corpus snapshot, so its proposed masking design is not accepted evidence; the diff-integrity correction itself came from Pascal Quantin and is independent of whether that feature ultimately merges.
