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


## Do not expand a focused MR to repair a broad pre-existing issue found during review

Merged master MR !7819 adds missing GSUP IEs. During review, Pascal Quantin noticed that one-byte fields could use stronger length validation, then recognized that the same issue applied to many existing IEs. Pascal and the contributor agreed that the broad cleanup should be handled separately rather than enlarging this focused feature MR.

**Submission rule:** when review discovers a broader pre-existing defect that is not necessary for the proposed change to be correct, record it for a separate follow-up rather than forcing unrelated subsystem cleanup into the current MR.

**Confidence:** High. Direct Pascal Quantin review guidance in a merged master MR.

## A stable backport must be dependency-closed

Merged release-4.0 MR !7824 depended on related cipher-suite changes !7825 and !7826 and on dependency support already introduced on the release branch. Pascal Quantin explicitly called out the prerequisite MRs while reviewing the backport.

**Backport rule:** review a cherry-pick as part of its dependency graph, not as an isolated commit. Before accepting a stable-branch backport, verify that all prerequisite code and required dependency capabilities are already present or are included in an ordered, reviewable backport series.

**Confidence:** High. Explicit prerequisite review in a merged stable-branch MR.


## Keep generated build artifacts out of submissions and use a maintainer-editable topic branch

Closed MR !7659 is not an implementation exemplar, but its maintainer review gives clear submission guidance. Guy Harris identified configuration/build-generated files that do not belong in the repository and asked that they be removed. Pascal Quantin separately noted that upstream CI already built with the claimed toolchain and requested a much smaller, relevant patch rather than environment output and unrelated edits. Guy also advised that a dissector intended to ship with Wireshark should generally be submitted as a built-in dissector rather than as an external-style plugin.

Closed/superseded MR !7623 provides a second workflow lesson. Gerald Combs required the contributor to enable **Allow commits from members who can merge** so maintainers could rebase or make minor fixes. The contributor could not enable that setting while the work lived on the protected `master` branch, so they recreated it on a topic branch and resubmitted as merged MR !7629.

**Submission rule:** review the diff for generated CMake/build outputs, local configuration products, copied generated files, and unrelated environment artifacts before posting. Submit the smallest source change that explains and fixes the upstream problem.

**Branch rule:** develop contribution MRs on a topic branch that permits maintainer collaboration/rebase rather than the fork's protected default branch.

**Dissector rule:** when the intent is to add protocol support to upstream Wireshark, prefer the normal built-in dissector structure unless there is a specific project reason to keep it as a plugin.

**Confidence:** High for process guidance because it comes directly from Guy Harris, Pascal Quantin, and Gerald Combs. The underlying !7659 implementation is down-weighted because the MR was closed.


## Split a stable-worthy bug fix from the feature that exposed it

Merged master MR !5115 began as a change that combined an MKA Announcement padding bug fix with new Announcement TLV parsing. Jaap Keuter explicitly asked for two MRs: the padding correction as one fix and the parsing implementation as a separate feature, because the fix could then be backported cleanly. The contributor reworked the series accordingly; feature MR !5128 remained on master, while the focused bug fix was carried to release-3.6 as !5154 and release-3.4 as !5155.

**Submission rule:** if a new feature uncovers or depends on an independently useful correctness fix, separate the fix from the feature when supported branches may need only the fix. The master series should preserve a cherry-pickable correctness unit rather than force release maintainers to disentangle feature code.

**Review rule:** backportability is a concrete reason to request MR/commit separation even when the combined master change would otherwise be understandable.

**Confidence:** Extremely high. Direct Jaap Keuter review shaped the merged master series, and the resulting focused fix was in fact backported to two maintained branches.


## Keep unrelated contribution work separate and make bug fixes traceable

Merged master MR !5109 initially combined an Ubuntu test-environment fix with additional pytest work. Uli Heilmeier asked the contributor to create a separate MR per logical change and to improve the fix commit message with the associated issue reference (for example, `fixes #17730`). The contributor updated the commit and moved the unrelated pytest work to another MR.

**Submission rule:** do not bundle unrelated work merely because it was discovered or tested together. A focused bug-fix commit/MR should state the problem clearly and carry the relevant issue reference so review, history, and later backport decisions remain traceable.

**Confidence:** High. Explicit reviewer guidance followed by contributor restructuring in a merged MR.
