# Wireshark Cross-Compilation Conventions

This file records durable cross-compilation and host/target build distinctions extracted from accepted upstream Wireshark changes. Current upstream build files remain authoritative.

## Keep build-host paths and target/install paths in their own path domains

Cross-compilation has at least two path namespaces: paths interpreted while building on the host, and paths interpreted later by the target system or installer. A path conversion described as “native” is not automatically native to the target; in CMake it can mean the build system's path style.

Merged MR !23688, authored by John Thacker and approved/merged by Gerald Combs, fixes NSIS profile packaging under MinGW. NSIS `File` directives locate source files while the package is being built and therefore need build-host path syntax, while `SetOutPath` is interpreted when the package is installed and therefore needs Windows target syntax. The accepted implementation also avoids `file(TO_NATIVE_PATH ...)` because its space escaping interacts badly with MinGW's path conversion, using the newer `cmake_path()` conversion for the build-host source path instead.

**Implementation rule:** for every path emitted by cross-build tooling, identify which process will interpret it and on which system. Convert build-time source paths according to the build host and install/runtime paths according to the target; do not use target-OS conditionals or a generic “native path” conversion as a substitute for that distinction.

**Confidence:** Very high. Merged master build/packaging correction authored by John Thacker and approved/merged by Gerald Combs, with the host-versus-target semantics explicitly documented in the MR and patch.

## Build-time generators are host tools

Merged stable MRs !4466 and !4467 make a useful host/target distinction for Lemon. A generator executed during the build must be compiled for the build host, and generated-file rules should invoke the actual CMake target path rather than depend on an executable name being present on PATH.

**Implementation rule:** identify every program executed during a cross build as a host-side tool or a target artifact. Host-side generators need a host compiler and a concrete build-target path even when their generated output is consumed by the target build.

**Confidence:** High. Both are merged maintained-branch backports of accepted master fixes.
