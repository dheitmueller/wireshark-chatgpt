# Wireshark Stable-Branch CI Conventions

This file records durable lessons about CI and submission checks on maintained release branches. Current branch policy and CI configuration remain authoritative.

## Stable-branch validation can differ when provenance and tooling differ

Merged release-branch MRs !216 and !232 disable `validate-commit.py` and an old cppcheck invocation on maintained branches. The rationale is not to lower standards generally: stable commits are normally cherry-picked from already-validated master commits, GitLab adds cherry-pick provenance text that can trip master-oriented whitespace validation, and those branches did not contain cppcheck performance improvements, making the check impractically slow.

Guy Harris asked about GitLab's built-in cherry-pick operation in !216 and, after Gerald Combs explained it, added the workflow to Wireshark's SubmittingPatches wiki.

**CI rule:** branch CI should validate the artifacts and invariants that are meaningful for that branch's actual provenance and maintained tooling. Do not mechanically mirror a master-only checker when its input is generated differently on stable branches or when the maintained branch lacks the tool changes that make the check viable.

**Backport rule:** preserve provenance for stable fixes and use the project's supported cherry-pick/backport workflow rather than manually reconstructing a change where that would lose history. Treat the exact GitLab UI details in these 2020 MRs as historical; the durable rule is provenance-preserving backporting.

**Confidence:** High. Two merged release-branch changes plus direct Guy Harris documentation follow-up.
