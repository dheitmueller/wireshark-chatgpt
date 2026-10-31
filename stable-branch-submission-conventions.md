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

## Split independent fixes out of feature work when they need their own backport path

A bug discovered while implementing a feature should not remain inseparably buried in the feature change when the fix is independently useful, especially when maintained release branches should receive the fix but not the new feature. Giving the fix its own commit/MR makes review intent, release-note scope, and cherry-pick eligibility explicit.

Merged master MR !13900 added RSVP SESSION_ATTRIBUTE support. During review Alexis La Goutte noticed that the feature diff also corrected an unrelated bad `proto_tree_add_item()` encoding argument and asked whether that fix should become a specific commit so it could be backported. John Thacker agreed that splitting it was preferable. The correction became merged master MR !13910, with release-4.2 !13911 and release-4.0 !13912 carrying the fix independently of the feature.

**Submission rule:** when feature development exposes an unrelated correctness fix, ask whether the fix has a different backport or release policy from the feature. If so, split it into a focused commit/MR before merge. This lets stable branches take the correction without taking new functionality and gives reviewers a clean unit whose risk and intent can be evaluated independently.

**Review rule:** when a feature MR contains an incidental bug fix, do not judge only whether the combined diff is correct on master. Check whether the correction should be independently backportable; if it should, request a split before the history makes that distinction harder to preserve.

**Confidence:** Very high. Direct maintainer review in a merged feature MR, followed by exactly the requested split-out master fix and accepted backports to both maintained stable branches.

## Submit a bug fix to master before requesting its stable-branch backport

Closed OPC UA !9299 proposed correcting `parseDiagnosticInfo` field order directly on `release-4.0`. Alexis La Goutte explicitly requested submitting the correction on `master` first and arranging the release-4.0 backport afterward. Several adjacent closed attempts (!9307–!9311) show why branch selection and reviewable commit scope matter; !9310's mistaken cherry-pick produced a 502-file diff for a one-file protocol fix. **Submission rule:** normally land the primary fix on master and then request or prepare focused stable backports; inspect the actual compare diff and base branch before opening or revising an MR, especially after cherry-picking. These are **review/workflow** lessons from closed drafts, not accepted implementation precedents. **Confidence:** High for Alexis's stated process; low for the abandoned source proposals.

## Move new protocol support to master even when the initial request targets a release branch

Closed Locamation timestamp submission !9189 originally targeted a release branch. Martin Mathieson explicitly explained that new feature code normally lands and stabilizes on `master`; release branches primarily receive fixes. The author resubmitted the feature as merged master !9193, providing a concrete accepted successor. Conversely, the **bug fix** for extcap path resolution was intentionally backported from merged master !9168 as accepted !9179/!9181 after Guy Harris requested it.

Open, unmerged TPM20 reassembly !9166 adds a separate branch-hygiene warning: Alexis La Goutte identified that the contributor opened the MR from the fork's `master`, producing an unsuitable compare/pipeline state. Its implementation is not a Wireshark precedent.

**Submission rule:** send new capabilities to a dedicated topic branch targeting master; only propose narrow backports of accepted fixes after evaluating stable-branch compatibility. Inspect the actual compare base, not merely the intended title. **Confidence:** Very high for Martin's explicit policy and the merged replacement/backport chain; the TPM example is process-only evidence from an open MR.
