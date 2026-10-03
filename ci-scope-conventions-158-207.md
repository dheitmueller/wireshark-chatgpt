# CI scope findings from MRs !158-!207

## Match CI jobs to the environments that can execute them

Merged master MR !161, authored by Gerald Combs, limits the Windows merge-request build to the canonical Wireshark project because that job depends on a project-specific Windows runner and custom image not available to ordinary forks. The accepted configuration also uses GitLab rules to require merge-request context.

**CI rule:** only schedule a job in repository contexts where its required runner environment exists. Keep portable validation available to contributor forks, and document why any canonical-project-only job is scoped that way.

**Confidence:** Very high. Merged master CI change authored by Gerald Combs.
