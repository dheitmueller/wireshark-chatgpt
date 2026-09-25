# Wireshark Submission and Backport Scope Conventions

This file records durable conventions for structuring changes so fixes can be reviewed and carried to supported release branches safely. Current upstream contribution and release-branch policy remains authoritative.

## Keep bug fixes separable from enhancements when stable backports may be needed

A master-branch change can be technically coherent yet still be poorly structured for release maintenance if a necessary bug fix is mixed with unrelated feature expansion. Stable branches often need the correctness fix without accepting new behavior, so the MR/commit structure should preserve that choice.

Merged master MR !12635 (`BLF: Fix CAN parsing`) provides explicit maintainer guidance. During review, the contributor asked about adding additional BRS/ESI functionality to the same change. Lars Völker recommended against mixing the enhancement with the bug fix because doing so makes the fix substantially harder to pull into release branches. The accepted MR kept the CAN parsing correction focused.

**Submission rule:** when a correctness fix is a plausible stable-branch candidate, keep unrelated enhancements out of the fix commit/MR or structure the series so the fix can be cherry-picked independently. Do not make release maintainers disentangle feature work from the minimal correction.

**Review rule:** evaluate scope not only for readability on master but also for cherry-pickability. If reviewers would want the fix on a release branch but not the enhancement, that is strong evidence they should be separate changes.

**Confidence:** Very high. Direct maintainer review guidance in a merged master bug-fix MR, with the requested scope separation reflected in the accepted outcome.

## Stable branches normally take fixes rather than enhancements

Merged MR 10474 and its release backports 10489 and 10490 provide direct maintainer evidence for the existing scope rule. Alexis La Goutte explained that fixes are candidates for backport while enhancements generally are not, with judgment required for borderline cases. Keep correctness changes separable so release branches can take the fix without unrelated feature work.

Closed MR !9977 supplies direct release-policy corroboration for the rule above. It attempted to carry already-merged IPv6 APN6 feature support to release-4.0; Jaap Keuter explicitly cited Wireshark's release policy and closed it because new features are not backported to stable releases. The implementation itself is down-weighted because the MR was not merged, but the maintainer statement is authoritative evidence for branch scope.

## Keep cleanup separate when a discovered fix needs independent backporting

Even a tiny correctness issue found during an otherwise harmless cleanup can deserve its own MR when release branches need the fix but not the cleanup. Splitting at that point is useful maintenance structure, not unnecessary patch fragmentation.

In merged master MR !9906, Martin Mathieson noticed a UDS spelling bug while reviewing removal of unused helpers. Lars Völker initially considered folding the correction into the cleanup, then deliberately opened separate MR !9952 so the typo fix could be cherry-picked to release-4.0 independently. The cleanup remained focused and merged separately.

**Submission rule:** when review uncovers a stable-worthy bug inside a cleanup/refactor, prefer a separate fix if the cleanup itself does not belong on the stable branch. Keep each commit's release intent obvious.

**Confidence:** High. Explicit contributor/reviewer discussion in a merged master cleanup and a separately submitted fix chosen specifically for stable-branch cherry-pickability.

## Automated generated-data updates must obey release-branch feature policy

Automation does not exempt generated updates from branch scope. If a scheduled data refresh also regenerates code or protocol definitions that constitute new functionality, its branch-aware configuration must prevent that feature expansion from landing on stable branches.

Closed MR !9907 was an automatic update targeting a release branch that unexpectedly extended the Asterix dissector. Jaap Keuter flagged that this did not fit Wireshark's release policy. Gerald Combs agreed, changed the update automation so Asterix dissector generation occurs only on master, and closed the MR in favor of a corrected update.

**Automation rule:** encode stable-branch policy into update tooling itself. Review automated diffs for generated code or feature-bearing artifacts rather than assuming that a routine registry/data refresh is branch-neutral.

**Confidence:** Very high for the policy lesson. The MR itself was closed and is not an implementation exemplar, but Jaap Keuter's review and Gerald Combs's accepted automation correction are authoritative evidence.
