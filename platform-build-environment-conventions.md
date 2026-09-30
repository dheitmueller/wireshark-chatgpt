# Platform Build Environment Conventions

This file records durable conventions for Wireshark platform/bootstrap scripts and dependency builds. Current upstream source remains authoritative.

## Revalidate generated dependency metadata after toolchain or SDK changes

Bootstrap artifacts can encode absolute paths into a particular SDK or toolchain installation. Their mere continued presence does not prove they are still valid after the host is upgraded.

Merged master MR !12176, authored and merged by Guy Harris, fixes `macos-setup.sh` after a generated `libffi.pc` survived an Xcode update while referring to an SDK include directory that no longer existed. Before deciding that libffi metadata is already available, the setup script now asks `pkg-config` for the advertised include directory, removes the generated `.pc` file when that directory is gone, and lets the normal setup path regenerate valid metadata.

**Bootstrap rule:** when Wireshark generates dependency-discovery metadata containing SDK/toolchain paths, validate the referenced path or identity before reusing the metadata on a later setup run. Prefer regenerating stale derived state over allowing it to steer a new build toward an obsolete SDK.

## Standards compliance does not guarantee third-party configure compatibility

A platform replacement can satisfy the governing standard yet differ in behavior that the standard leaves unspecified. Third-party configure scripts may nevertheless probe for one implementation's behavior and incorrectly treat another conforming implementation as unusable.

Merged master MR !12165, authored and merged by Guy Harris, handles the macOS transition from GNU-style `iconv()` behavior to the FreeBSD-derived implementation. Both behaviors are POSIX-conforming for the relevant unrepresentable-character case, but GNU gettext's configure test expects the GNU behavior and otherwise disables iconv in a way that later breaks its own build. Wireshark adopts the same targeted configure override used by Homebrew on the affected platform.

**Compatibility rule:** diagnose dependency configuration failures in terms of the exact behavior being probed, not only API presence or standards conformance. If an upstream configure test assumes implementation-specific semantics, use a narrowly scoped, documented override or workaround backed by known-good platform behavior rather than globally pretending a capability exists.

## Dependency install names must support the development execution model

A dependency build can install successfully yet still be unusable by Wireshark binaries executed directly from the build tree if the dependency's recorded runtime identity does not match how the development environment resolves libraries.

Merged master MR !12198, authored and merged by Guy Harris, centralizes CMake invocation in `macos-setup.sh` so CMake-built dependencies record a full installed library path rather than the newer default `@rpath/<library>` form. On macOS Sonoma/Xcode 15 the latter caused Wireshark build-tree binaries to fail at runtime unless developers manually supplied a `DYLD_LIBRARY_PATH`; the accepted setup makes CMake-built dependencies behave consistently with the other dependencies installed by the script. Later MR !12433 explicitly refers back to this fix while removing downstream workarounds after affected libraries are rebuilt.

**Build-environment rule:** validate bootstrap dependencies by running the same kind of build-tree executables developers and CI use, not only by checking that libraries compiled and installed. Runtime loader metadata such as install names/RPATHs is part of the dependency-build contract.

**Confidence:** Extremely high. All three master changes were authored and merged by Guy Harris and document concrete failures caused by stale SDK metadata, implementation-specific configure assumptions, and runtime loader metadata respectively.

## Platform-specific source needs platform-specific merge-request coverage

Merged master MR !3645 fixes Windows-only ETW source that used pointer syntax on a stack object. The mistake escaped non-Windows compilation because the affected code is only built on Windows. In direct review, Guy Harris states that a Windows build is needed in the merge pipeline specifically to catch code that passes on UNIX systems only because the Windows source path is not compiled there. Gerald Combs notes that the project did have a Windows runner, but infrastructure constraints meant it did not execute for every fork-originated MR.

**CI rule:** if a source path is selected only by a platform guard, at least one ordinary pre-merge configuration should compile that guarded path. Success on platforms that exclude the code is not evidence of portability.

**Review rule:** distinguish “the project has a runner for this platform” from “this MR actually executed that runner.” Gaps caused by runner availability remain coverage gaps and should be treated explicitly.

**Confidence:** Extremely high. Direct Guy Harris review on a merged correction, with Gerald Combs documenting the actual CI coverage limitation.


## Feature-test optional dependency functions at the link boundary

Merged master MR !3570 adds Kerberos PAC ticket-signature verification. The code compiled far enough on Unix to expose ordinary signedness and const-correctness issues, but Anders Broman's Windows build then failed to link because the required `decode_krb5_enc_tkt_part` and `encode_krb5_enc_tkt_part` functions were not exported by the Windows Kerberos library. The accepted implementation adds configure-time function checks and compiles the optional verification path only when the required functions are available.

**Dependency rule:** do not infer that an optional external-library function is usable merely because a header declares it, another platform exports it, or the dependency version appears new enough. Probe the exact function at configure/link time when platform packages can expose different symbol sets, and guard the optional feature on that result.

**Confidence:** Very high. Merged master implementation with direct cross-platform build review from Anders Broman and Isaac Boukris.

## Version-gated compiler/linker flags still need workflow validation

Merged master MR !3571 reverts Windows CET/EHCONT hardening that had been gated to MSVC versions advertising those options. In Wireshark's real incremental-build workflow, the combination caused internal compiler and linker failures.

**Toolchain rule:** a compiler-version test proves nominal option availability, not that the option combination is stable in every supported build mode. Validate hardening and unusual linker flags in the same full and incremental workflows developers and CI actually use before treating the version gate as sufficient.

**Confidence:** High. Merged master revert of a concrete Windows build regression.
