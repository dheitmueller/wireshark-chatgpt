# Durable conventions from Wireshark MRs !1–!7

## Keep feature-only declarations inside the feature boundary

If a declaration is only used when a dependency version or optional capability is compiled in, guard the declaration with the same predicate as its consumers. Otherwise configurations that compile the consumers out can still fail strict builds on unused static constants or helpers.

**Evidence:** merged !5 moves Bluetooth Mesh reassembly descriptors under the old-libgcrypt version guard and QUIC stream-fragment descriptors under `HAVE_LIBGCRYPT_AEAD`.

**Confidence:** High; merged master portability fix.

## Treat sample captures as part of a dissector submission

A new or materially changed dissector should come with a small representative capture when practical. Test it with the project's fuzzing/randpkt tooling, and regard the capture as a reusable fuzzing/review artifact rather than merely a screenshot substitute.

**Evidence:** merged !2 updates the official `README.dissector` submission instructions and explicitly states that a new dissector normally will not be accepted without a sample capture; it also notes that sample captures are used by automated fuzz testing.

**Confidence:** Extremely high; accepted project developer documentation, later corroborated repeatedly by maintainer review.

## Wire-width fixes require downstream offset audit

When correcting the length of an on-wire field, recompute every following offset/cursor derived from the old width. A field can display correctly while all subsequent fields remain misaligned if the geometry update is incomplete.

**Evidence:** merged !7 changes the SR IGP IPv6 FEC from 32 bytes to 16 and moves mask, protocol, and reserved fields from offsets +36/+37/+38 to +20/+21/+22.

**Confidence:** High; direct merged parser fix.

## Forge migrations must migrate validation semantics too

When the project changes review systems, remove metadata and hooks specific to the old forge and teach commit validation the syntax of the new one. Do not carry obsolete identifiers forward as ritual.

**Evidence:** merged !1 removes the Gerrit `commit-msg` hook and rejects legacy `Bug:` / `Ping-Bug:` trailers in favor of GitLab issue syntax. Closed !6 contains Dario Lombardo's direct request to remove the stale Gerrit `Change-Id`.

**Confidence:** High for the general workflow principle; exact issue-reference syntax remains subject to current project documentation.

## Rebase and squash review churn

Avoid merge commits from synchronizing a topic branch with upstream, and squash multiple iterations of the same logical fix before review/merge when practical.

**Evidence:** Dario Lombardo's review on closed !6 explicitly asked for the merge commit to be removed by rebasing and the two change commits to be squashed; the clean successor !7 merged.

**Confidence:** High as submission-workflow evidence; implementation weight comes from the merged successor.
