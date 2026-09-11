# Build Download Diagnostics Conventions

This file records durable conventions for build-system download and artifact-fetch failures.

## Preserve the transport error and download log when artifact retrieval fails

Build bootstrap code that downloads required artifacts should fail loudly and report the underlying transport diagnostic, not collapse every failure into a generic "unable to download" message. Network, TLS, authentication, proxy, hash, and server failures often require different remediation, and CI logs may be the only evidence available.

Merged MR !23778, authored and merged by Gerald Combs, changes `FetchArtifacts.cmake` to collect CMake `file(DOWNLOAD)`'s status text and `LOG` output and include both in the fatal error. The release-4.6 backport !23782 preserves the same behavior.

**Implementation rule:** for required build downloads, check the command's actual status, stop on failure, and surface both the concise error/status and the detailed transfer log when the underlying API provides them.

**Review rule:** diagnostics are part of build reliability. A failure path that preserves only a generic wrapper message is incomplete when the called tool already exposes actionable failure details.

**Confidence:** Very high. Merged master build-system change authored by Gerald Combs and preserved in an accepted stable-branch backport.
