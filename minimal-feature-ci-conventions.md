# Minimal-Feature CI Conventions

Merged master MR !5463 adds a merge-request build with a broad set of optional dependencies disabled. Merged follow-up !5464 removes a narrower feature-off macOS variant because the centralized job already supplies that coverage, and !5479 enables caching for the minimal build.

**CI rule:** keep a broad optional-feature-off build as first-class MR coverage. It catches compile-time dependencies hidden by feature-rich builds. Remove narrower duplicate variants once centralized coverage exists unless they add a distinct platform or behavior dimension.

**Confidence:** Very high; merged master CI work and immediate follow-up cleanup converge on the same coverage model.
