# Wireshark Stable-Branch Submission Conventions

This file records durable conventions for choosing merge-request target branches and preparing changes for Wireshark's maintained branches. Current upstream contribution and release policy remains authoritative.

## New feature intake belongs on master; maintained release branches stay stable

Do not target an existing stable release branch merely because the feature would be useful there. New functionality is developed and reviewed on `master`; maintained release branches are primarily for stabilization and appropriate backports.

Merged MR !24662 provides direct maintainer guidance. The contributor initially moved a new OAMPDU feature between `master` and `release-4.6`. Jaap Keuter explicitly stated that new feature intake is on master and release branches are kept stable, requiring the change to be recreated against master. The contributor ultimately did so, incorporated additional protocol coverage requested in review, and Jaap approved and merged the MR.

**Submission rule:** submit new protocol/features against current `master`. Treat release branches as stabilization/backport targets unless maintainers explicitly request otherwise. If a change was developed on the wrong base, retargeting the MR does not by itself make the commit history or diff suitable; rebase or recreate the change against the intended branch and verify the resulting diff.

**Confidence:** Very high. Direct branch-policy instruction from Jaap Keuter on a merged first-contribution MR, with the requested workflow followed before merge.
