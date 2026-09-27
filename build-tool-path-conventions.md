# Build Tool Path Conventions

Merged MR !7019 fixes Windows setup after archive extraction began using CMake. The parent CMake build passes its resolved `CMAKE_COMMAND` path into `win-setup.ps1` instead of assuming a `cmake` executable can be found through PATH.

**Rule:** when the parent build system has already resolved a required tool, pass that exact executable to helper scripts. A second PATH lookup can select a different version or fail in an otherwise valid configured build environment.

**Confidence:** High. Merged build-system fix by Gerald Combs.
