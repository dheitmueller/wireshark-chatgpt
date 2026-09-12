# Wireshark Public-Header Validation Conventions

This file records durable conventions for validating public header interfaces. Current upstream source remains authoritative.

## Public headers should compile independently of higher-layer headers

A public library header is part of that library's supported interface and must not compile only because a consumer happened to include headers from a higher layer first. Build validation should exercise public headers in isolation with only the dependencies that library is legitimately allowed to expose.

Merged MR !22933 adds an explicit compile check for `wsutil` public headers without first including libwiretap or libwireshark headers. During review, Guy Harris argued that this should not merely be a Debian-packaging check: the same class of public-header self-sufficiency should ideally be tested on all build platforms because platform-specific header problems can otherwise escape one environment and fail another. The accepted MR provides an executable guard for the dependency-boundary invariant and later discussion notes CMake's `VERIFY_INTERFACE_HEADER_SETS` as another possible mechanism for such checks.

**Implementation rule:** treat public-header self-sufficiency as a build/test invariant. A public header should include the authoritative declarations needed for the types and macros it exposes, without relying on include order or accidental higher-layer transitive includes. Where practical, compile public headers independently in CI across supported build environments rather than relying on downstream packaging to discover breakage.

**Confidence:** Extremely high. Merged master build validation with direct, substantive Guy Harris review advocating broader cross-platform enforcement.
