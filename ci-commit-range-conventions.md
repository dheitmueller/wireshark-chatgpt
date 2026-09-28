# CI Commit-Range Identity Notes

Wireshark merge request 9377, authored and merged by Gerald Combs, changed the GitLab pre-commit job from deriving its range from ambient HEAD to deriving it from the CI-provided commit SHA. The change makes the range depend on the revision identified by the pipeline rather than the checkout's incidental current ref.

Durable convention: CI range calculations use an explicit CI revision identity whose semantics match the job. Pipeline-revision checks use the pipeline commit identity; checks that specifically need an MR source revision use the MR source identity. Ambient HEAD is not treated as an authoritative substitute.

Confidence: very high.


## Use first-parent generation syntax when a CI job means "N commits back"

Git revision operators encode different graph relationships. `commit^N` means the Nth parent of one commit, while `commit~N` follows the first parent for N generations. A CI check that wants the base before N sequential commits therefore must not substitute caret-parent syntax for generation ancestry.

Merged master MR !5954, authored by John Thacker, changes the commit-check range from `HEAD^$NUM_COMMITS` to `HEAD~$NUM_COMMITS`. The old form failed for values such as `HEAD^3` because an ordinary commit does not have a third parent. MR !5943 independently exposed the failure in a multi-commit pipeline and points to !5954 as the fix.

**Implementation rule:** choose Git revision syntax from the topology the job means to traverse. For "the ancestor N commits before this revision along normal history," use first-parent generation ancestry; reserve `^N` for selecting parent number N of a merge commit.

**Confidence:** Very high. Merged CI fix by John Thacker with an independently observed pipeline failure in the same batch.
