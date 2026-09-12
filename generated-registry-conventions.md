# Generated Registry and Downloaded-Data Conventions

This file records durable conventions for Wireshark tooling that turns externally maintained registries or downloaded source data into committed source artifacts. Current upstream tooling remains authoritative.

## Treat unexpectedly small downloaded registries as generation failures

A generator that downloads an external registry should not assume that a syntactically parseable response is complete. Remote sites can accidentally publish empty, truncated, or structurally changed content, and blindly regenerating committed source from such a response can turn an upstream-site problem into a large destructive Wireshark change.

Merged MR !22532 adds a generator for the ESTA manufacturer-ID registry. During review Gerald Combs explicitly requested a minimum-value check, noting that sites surprisingly often remove content inadvertently and pointing to `make-pci-ids.py` as the established model. The accepted generator checks both that parsed rows match the page structure and that the number of manufacturers has not fallen below a known historical floor before rewriting the generated table. Gerald also requested avoiding an unnecessary third-party Python dependency and decoding the HTTP response explicitly as UTF-8 with replacement rather than trusting the page declaration.

**Implementation rule:** generators that consume mutable external registries should validate plausibility before publishing output. Check structural consistency and, where the data set is expected to grow or remain roughly stable, enforce a defensible lower bound or equivalent invariant. Prefer standard-library retrieval when it is sufficient, decode external text explicitly, and fail before replacing committed data when the source looks incomplete.

**Confidence:** Very high. Merged master tooling change with specific Gerald Combs review requests implemented before approval and merge.
