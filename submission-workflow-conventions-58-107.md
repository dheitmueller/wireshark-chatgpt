# Submission Workflow Conventions from !58–!107

## Maintainer-edit permission, linear history, and commit presentation are review requirements

Merged master MR !107, authored by Gerald Combs, makes the GitLab setting that allows maintainers to push commits to an MR branch a validation requirement so maintainers can rebase or make minor fixes. In !101 Pascal Quantin explicitly asks the contributor to enable that setting.

In merged !64 Pascal asks the contributor to rebase instead of merging the target branch into the topic, specifically to avoid a "merge branch" commit, and asks for a component-prefixed commit subject matching Wireshark's submission guidelines. In stable backport !88, Guy Harris questions a misleading MR title; Gerald Combs explains that two commits were accidentally pushed and fixes the title/history. Merged !95 (after superseded !93) records the project-specific rebase recovery workflow.

**Submission rule:** keep the topic branch linear and focused, use the project-style component prefix in commit subjects, and enable maintainer edits when required by project workflow. The MR's history and metadata are part of reviewability, not incidental transport.

**Evidence weight:** Very high for !107 and !64, with direct maintainer guidance from Gerald Combs, Pascal Quantin, and Guy Harris.
