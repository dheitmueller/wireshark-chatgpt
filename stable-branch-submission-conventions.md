# Wireshark Stable-Branch Submission Conventions

This file records durable conventions for choosing merge-request target branches and preparing changes for Wireshark's maintained branches. Current upstream contribution and release policy remains authoritative.

## New feature intake belongs on master; maintained release branches stay stable

Do not target an existing stable release branch merely because the feature would be useful there. New functionality is developed and reviewed on `master`; maintained release branches are primarily for stabilization and appropriate backports.

Merged MR !24662 provides direct maintainer guidance. The contributor initially moved a new OAMPDU feature between `master` and `release-4.6`. Jaap Keuter explicitly stated that new feature intake is on master and release branches are kept stable, requiring the change to be recreated against master. The contributor ultimately did so, incorporated additional protocol coverage requested in review, and Jaap approved and merged the MR.

**Submission rule:** submit new protocol/features against current `master`. Treat release branches as stabilization/backport targets unless maintainers explicitly request otherwise. If a change was developed on the wrong base, retargeting the MR does not by itself make the commit history or diff suitable; rebase or recreate the change against the intended branch and verify the resulting diff.

**Confidence:** Very high. Direct branch-policy instruction from Jaap Keuter on a merged first-contribution MR, with the requested workflow followed before merge.

## Keep backport source-branch names short enough for generated merge subjects

Wireshark's commit-subject length check can apply to the merge commit synthesized by GitLab, not only to the cherry-picked commit supplied by the contributor. Because GitLab incorporates source and target branch names into that generated subject, an unnecessarily long source-branch name can make an otherwise compliant backport fail the 80-character subject limit.

In closed backport MRs !23503 and !23506, Guy Harris traced the validation failure to GitLab's generated merge subject rather than the original BLF fix. He concluded that the practical remedy was to abandon the MR and recreate it with a shorter branch name. Merged successor !23507 uses the compact branch `backport-2b163010` and carries the same fix into `release-4.6` successfully. Merged !23504/!23505 also improved the validator to print the exact offending subject, making this distinction explicit.

**Submission rule:** when creating a stable-branch/backport MR, use a concise source branch name, especially for automated cherry-pick branches. If `validate-commit` reports an overlong subject while the cherry-picked commit subject is compliant, inspect the generated merge commit subject before rewriting the actual commit.

**Confidence:** Extremely high. Direct diagnosis and resolution by Guy Harris, with the abandoned long-branch attempts superseded by a merged short-branch successor.