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

## Keep capture artifacts out of the repository root

Packet captures used to demonstrate a bug should normally be attached to the issue, and captures that become regression fixtures should live in the repository's established test-data structure. The repository root is not an acceptable dumping ground even when the file is relevant to testing.

Merged MR !26325, authored by John Thacker and merged by Gerald Combs, removes a capture accidentally committed at the repository root. The commit explicitly states that the file should have been attached to the issue and that even a test capture should not have been placed at the root.

**Submission rule:** attach reproducer captures to the issue/MR unless they are intentionally being added as maintained test data; when adding a test fixture, put it in the project's designated test/capture location and wire it into the test suite rather than committing it at top level.

**Confidence:** Very high. Merged repository-hygiene correction accepted by project leader Gerald Combs.

## Keep contribution-policy wording and CI enforcement synchronized

When a contribution trailer or policy keyword changes, update automated checks and their user-facing remediation text in the same change. A stale CI check can otherwise enforce a superseded convention even though the documentation says something else.

Merged MR !26304 updates the GitLab CI check from the former `AI-Assisted` trailer to `Assisted-by` after the contribution policy was renamed, and rewords the reminder to match CONTRIBUTING: disclosure is required when AI assistance was used, not unconditionally. Gerald Combs merged the correction.

**Submission rule:** treat policy docs, commit-trailer spelling, CI detection, and CI error/help text as one interface. When one changes, search for and update all enforcement and guidance consumers.

**Confidence:** Very high. Merged CI/policy synchronization change accepted by Gerald Combs.

## Use closing-keyword issue references when the MR is intended to resolve the issue

A plain textual mention establishes context but does not express the intended repository workflow. When the change is expected to close a tracked issue, use GitLab's supported closing syntax in the commit/MR message.

In open MR !26313, Jaap Keuter explicitly asked the contributor to add `Fixes #21544`, noting that this automatically links and closes the issue and pointing to Wireshark's SubmittingPatches guidance. The implementation itself remained unresolved and is not treated as accepted architectural evidence, but the submission instruction is independent of that outcome.

**Submission rule:** for a change intended to resolve an issue, use `Fixes #NNNN` (or the currently documented equivalent) rather than relying on an informal mention.

**Confidence:** High. Direct maintainer guidance tied to Wireshark's documented submission process; implementation MR remained open in the corpus snapshot.

## Prefer reverting an incomplete feature over a narrow band-aid at a release boundary

A patch that prevents the most obvious crash does not necessarily make an incomplete feature release-ready. Near a test release or final release, evaluate the feature as a whole—including every supported application that exposes it—rather than treating a localized crash fix as sufficient evidence that it should remain enabled.

MR !25157 proposed a narrow Stratoshark initialization fix for the new Find in Packet feature while explicitly acknowledging that the feature remained incomplete there. John Thacker recommended reverting the feature until after the 4.7.0 test release. The author agreed, !25157 was closed, and merged MR !25158 reverted the feature because of the known Stratoshark problems and proximity to the release.

**Review/submission rule:** when a newly added feature has known incomplete or broken behavior across supported applications close to a release boundary, prefer restoring the last known-good feature set over shipping a minimal crash-only repair that leaves the incomplete feature exposed. Reintroduce the feature after its cross-application behavior is complete and reviewable.

**Confidence:** Very high. The proposed partial repair was explicitly superseded after John Thacker's review, and the full revert was the merged project outcome.