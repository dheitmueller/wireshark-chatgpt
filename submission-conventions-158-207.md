# Submission Conventions from MRs !158-!207

## Verify that “upstream” really names the canonical repository

During merged master MR !166, a contributor repeatedly received an “up to date” result while attempting to rebase, but both configured remotes pointed at the contributor's own fork. After correcting the upstream remote to the canonical Wireshark repository, the topic branch could be rebased against the real project history.

**Submission rule:** verify remote identities before relying on rebase status. A clean rebase against the wrong repository does not establish that an MR is current.

## Keep the semantic diff reviewable

In the same MR, Pascal Quantin explained that an oversized source-file diff exceeded GitLab's web-rendering limit and asked for the accumulated work to be split. The contributor reduced the submitted change so reviewers could inspect it before merge.

**Submission rule:** split or narrow a change when the review system cannot render its substantive diff. Build success does not replace human inspection of the semantic delta.

## Remove review-system metadata that the current workflow no longer consumes

In merged master MR !158, Pascal Quantin asked the contributor to update the local commit-message hook from the current Wireshark tree and remove old Gerrit `Change-Id` lines because the GitLab workflow no longer used them.

**Submission rule:** use the commit-message tooling supplied by the current tree and keep trailers aligned with the active review system. When history is amended, ensure the replacement revision receives fresh CI validation.

## Search duplicated protocol paths after finding a repeated defect

Merged !202 corrected a bit-position error in one GSM RR Channel Description decoder. Pascal Quantin immediately identified the same bug in the two sibling decoders and requested that all three be fixed.

**Review rule:** when a parser has cloned or sibling implementations, a bug found in one should trigger a targeted search of the others before the MR is considered complete.
