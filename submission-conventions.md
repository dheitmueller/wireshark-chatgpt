# Wireshark Submission Conventions

This file records durable merge-request and commit-presentation conventions extracted from upstream maintainer review. Current upstream contribution guidance and CI checks remain authoritative.

## Put the dissector/component name before the colon in commit and MR subjects

Wireshark subjects use the component being changed as the prefix. For dissector work, the protocol/dissector name belongs before the colon; a generic action prefix such as `fix:` is ambiguous and does not identify the affected component.

Merged MR !26145 began with a generic `fix:` subject. John Thacker explicitly asked the contributor to change both the MR title and the commit subject to the form `QUIC: Fix length of payload buffer passed to DATAGRAM dissector`, explaining that Wireshark requires the dissector name before the colon and that `FIX` itself is a protocol/dissector name (Financial Information Exchange). The contributor updated the title/commit and the MR was merged by John.

**Submission rule:** use `Component-or-Dissector: brief summary` for the subject, with the actual Wireshark component/protocol before the colon. Do not use generic conventional-commit prefixes such as `fix:` in place of the component name.

This sharpens the existing notebook rule that commit messages use a short `component: brief summary` subject followed by a blank line before any longer body.

**Confidence:** Very high. Direct maintainer instruction on a merged first-contribution MR, with the requested form adopted before merge.