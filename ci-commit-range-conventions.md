# CI Commit-Range Identity Notes

Wireshark merge request 9377, authored and merged by Gerald Combs, changed the GitLab pre-commit job from deriving its range from ambient HEAD to deriving it from the CI-provided commit SHA. The change makes the range depend on the revision identified by the pipeline rather than the checkout's incidental current ref.

Durable convention: CI range calculations use an explicit CI revision identity whose semantics match the job. Pipeline-revision checks use the pipeline commit identity; checks that specifically need an MR source revision use the MR source identity. Ambient HEAD is not treated as an authoritative substitute.

Confidence: very high.
