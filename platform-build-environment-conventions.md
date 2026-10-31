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

## Do not hide a misconfigured Visual Studio target behind CMake defaults

Closed/unmerged !9018 proposed defaulting Windows `PLATFORM` to x64 and `WIRESHARK_BASE_DIR` to the current binary/build directory. **João Valverde** rejected losing an environment-vs-generator architecture sanity check, and **Gerald Combs** explained that a generic VS Developer Command Prompt may target x86 with no `Platform` variable. Silently assuming x64 can make builds fail much later and obscure the actual compiler setup problem. Downloaded Windows support libraries are not build products; they belong outside ephemeral build trees. **Rule:** require a compiler-vendor-supported configured target environment and verify target/platform consistency early, with dependency storage independent of an individual build checkout. This is **authoritative review of a rejected proposal**, not evidence the proposed defaults were merged.

Merged !9046 corrects MSVC Release/RelWithDebInfo unused-variable warnings but was reviewed for scope and reproducibility: João requested separating an unrelated Qt deprecation fix and Gerald asked for exact VS, Windows SDK and CMake versions. **Rule:** separate independent portability fixes where practical, and diagnose the precise supported toolchain/build configuration before broadening a warning-suppression macro. **Confidence:** high for the explicit maintainer guidance.

## Honor explicitly configured macOS installation RPATHs without changing the default

Merged **!8776** changes the initial `LIBRARY_INSTALL_RPATH`, `EXECUTABLE_INSTALL_RPATH` and `EXTCAP_INSTALL_RPATH` values from unconditional empty strings to `${CMAKE_INSTALL_RPATH}`. This permits an explicit `-DCMAKE_INSTALL_PREFIX=... -DCMAKE_INSTALL_RPATH=...` installation to resolve libraries at its custom location while retaining the prior empty default when no override is supplied.

**Rule:** build-system defaults should not silently erase a supported user's explicit deployment-path configuration; test both the default install layout and the nonstandard-prefix install/load path, especially for extcap executables. **Confidence:** high, merged focused CMake change.
