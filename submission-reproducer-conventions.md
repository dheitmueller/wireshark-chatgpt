# Wireshark Submission Reproducer Conventions

## Link fixes to issues in the commit and attach packet reproducers to dissector bugs

During merged !6603, John Thacker tells a first-time contributor that the issue number should be included in the commit message so GitLab closes the issue automatically, and that a capture file showing the dissector problem should be attached to the issue because it makes diagnosis and review substantially easier.

**Submission rule:** for issue-driven fixes, reference the issue from the commit itself rather than relying only on MR discussion. For packet-decoding defects, attach a minimal capture reproducer to the issue or MR whenever redistribution permits.

**Confidence:** High. Direct John Thacker review guidance on a merged dissector correctness fix.

## Establish fixes on master before stable-branch cherry-picks

Closed !6578 proposed the hidden-interface statistics fix directly against a stable branch. Alexis La Goutte asked that it be fixed on master first; the contributor opened merged master !6580. Guy Harris later clarified that merged !6594 was the stable cherry-pick of that master fix.

**Backport rule:** establish the authoritative correctness change on master first, then cherry-pick or adapt it to maintained stable branches. Treat a stable-first MR as an exception requiring explicit maintainer direction, not the normal workflow.

**Confidence:** Very high for workflow. Direct Alexis La Goutte and Guy Harris guidance, with the master fix and stable follow-up both present; the original stable-first MR was closed.
