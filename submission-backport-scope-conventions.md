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
