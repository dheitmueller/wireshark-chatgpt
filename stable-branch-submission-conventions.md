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

## Verify the target branch's dependency and API surface before treating a cherry-pick as a backport

A change that is self-contained on `master` can depend on refactoring or APIs that do not yet exist on a maintained release branch. Review a backport against the target branch's actual module boundaries and exported APIs, not against the source branch's current architecture.

Merged release-4.6 MR !21367 backported BLF application-name/version handling. Guy Harris identified that its call to `try_val_to_str()` could not be used from Wiretap on release-4.6 because that helper still lived in `epan` there; the corresponding move of value-string functionality into `wsutil` existed only on newer code. The backport was therefore updated after merged prerequisite !21368 brought the intended lower-level utility placement to release-4.6.

**Submission rule:** before proposing or approving a stable-branch cherry-pick, enumerate the APIs and architectural prerequisites the change relies on and verify that they exist in the target branch at the same usable layer. If a prerequisite refactor is itself appropriate for the stable branch, backport it first and preserve the intended dependency direction. Otherwise adapt the fix deliberately to the older architecture; do not paper over a missing prerequisite by introducing an ad hoc duplicate helper or an improper cross-layer dependency merely to make the cherry-pick compile.

**Confidence:** Extremely high. Direct architectural backport diagnosis by Guy Harris, followed by the prerequisite backport and successful merged release-branch change.