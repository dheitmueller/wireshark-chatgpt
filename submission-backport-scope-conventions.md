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

## Split a narrow regression fix from a larger follow-up refactor or feature

Reviewability is itself a reason to separate work even when two changes touch the same protocol. A small correction to an already-merged regression can often be reviewed and merged immediately, while a broader feature/refactor needs more design time. Keeping them in one MR unnecessarily couples the urgent, low-risk fix to the slower change.

Merged master MR !9731 originally combined a fix for header-field problems introduced by an earlier CoAP change with additional Q-Block refactoring. Stig Bjørlykke explicitly asked for the two commits to be split: the bug fix was easy to review and merge, while the Q-Block improvement required more time. The contributor narrowed !9731 to the fix and moved the Q-Block work to draft MR !9742. The focused fix merged; !9742 remained open in this corpus snapshot and is therefore lower-weight design evidence.

**Submission rule:** when one part of a series repairs a concrete regression and another part expands or restructures behavior, submit them separately unless they are inseparable for correctness. Let the obvious fix land without making it wait for review of exploratory follow-up work.

**Review rule:** if reviewers can confidently approve one commit while needing substantially more protocol/design review for another, treat that as a strong signal that the work should be split into separate MRs.

**Confidence:** Very high. Explicit scope guidance from Stig Bjørlykke with the requested split performed before the accepted fix was merged.

## Stable branches do not take refactors or enhancements as backports

Closed MR !9501 attempted to backport a PFCP grouped-IE refactor to release-4.0. Alexis La Goutte explicitly stated that stable branches backport bug fixes, not enhancements; the contributor accepted that policy and the MR remained unmerged.

**Submission rule:** classify a master change before proposing a stable backport. Correctness and security fixes are candidates; cleanup, refactors, and enhancements should remain on master unless project policy explicitly says otherwise.

**Confidence:** High as branch-policy evidence. The implementation itself is down-weighted because it was not merged, but the maintainer statement is explicit and corroborates other stable-branch review history.

## Land the master fix first, then create release-branch backports from the accepted change

Closed MR !9299 targeted the OPC UA DiagnosticInfo fix at a release branch; Alexis La Goutte asked the contributor to put the fix on master first and backport it afterward. Merged master MR !9266 provides corroborating workflow evidence for a CQL fix: after the master change was ready, the affected stable branch was handled separately in merged release-4.0 backport !9280.

**Submission rule:** unless project policy or an emergency release process says otherwise, settle the fix on master first, then create explicit backports for supported release branches that demonstrably need it.

**Review rule:** keep each backport mechanically close to the accepted master fix and avoid mixing unrelated branch history or multiple release targets into one submission.

**Confidence:** High. Direct maintainer guidance from Alexis La Goutte, corroborated by a merged master-fix/backport sequence.
