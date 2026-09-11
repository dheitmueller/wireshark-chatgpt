# Wireshark CI Job Scope Conventions

This file records durable conventions for scoping advisory and validation jobs in Wireshark's GitLab CI. Current upstream CI configuration remains authoritative.

## Do not duplicate MR-only advisory work in merge trains, and make superseded advisory jobs interruptible

A job whose purpose is to provide feedback on the contributor's merge request does not need to run again in a merge-train pipeline when the regular merge-request pipeline already performs the same check and the result is not required to validate the merged result. Such advisory jobs should also be interruptible when a newer commit supersedes the pipeline, so stale work does not consume runner capacity or delay current feedback.

Merged MR !23975, authored and merged by John Thacker, removes the AI Trailer Reminder from merge-train pipelines because it already runs for the ordinary merge request and marks the job `interruptible`, explicitly to avoid stale jobs backing up when new commits are pushed.

**Implementation rule:** scope CI jobs to the pipeline context that gives their result meaning. Re-run integration-sensitive checks in merge trains, but avoid redundant merge-train execution for purely MR-facing reminders or advisory checks; make replaceable advisory jobs interruptible when their output is obsolete after a new commit.

**Confidence:** High. Merged master CI change authored and merged by John Thacker.