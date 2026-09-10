# Wireshark Build-System Conventions

This file records durable build-system conventions extracted from accepted upstream Wireshark changes. Current upstream build files remain authoritative.

## Model object libraries as targets and propagate their usage requirements

When a CMake object library is a reusable build component, link it as a target rather than manually injecting `$<TARGET_OBJECTS:...>` into each consumer's source list. Put the component's include paths and library requirements on the component target with the appropriate `PUBLIC` or `PRIVATE` scope so CMake can propagate them transitively.

Merged MR !26062, authored, approved, and merged by John Thacker, updates the extcap build this way now that Wireshark's minimum CMake version supports object libraries in `target_link_libraries()`. In particular, `ssh-base` owns its libssh include and link requirements as `PUBLIC`, because its public header exposes libssh types, while extcap executables link `cli_main`, `extcap-base`, and `ssh-base` as targets instead of splicing their object files into source lists.

**Implementation rule:** express build dependencies at the target that semantically owns them and let CMake usage requirements propagate. Avoid duplicating transitive include/library dependencies in every final executable merely because an object library is involved.

**Confidence:** Very high. Merged master build-system change authored and merged by John Thacker.

## Remove obsolete configure/probe scaffolding when it no longer performs a check

Build-system code should describe the checks and dependencies Wireshark actually performs today. Legacy setup for a feature test should not remain after the test itself has disappeared: unused `CMAKE_REQUIRED_*` state, push/pop check-state calls, or included check modules make a find module look as though it verifies capabilities that it does not.

Merged MR !26117, authored by John Thacker and approved/merged by Gerald Combs, removes leftover `CMakePushCheckState`, `CheckFunctionExists`, and `CMAKE_REQUIRED_INCLUDES`/`CMAKE_REQUIRED_LIBRARIES` setup from `FindMaxMindDB.cmake`. The code dated from the legacy GeoIP implementation and had become a no-op after the relevant check was removed.

**Implementation rule:** when deleting or replacing a configure-time capability test, remove its setup and state-management scaffolding as well. Keep find modules minimal enough that readers can distinguish actual dependency discovery/validation from historical residue.

**Confidence:** Very high. Merged master build-system cleanup authored by John Thacker and approved/merged by Gerald Combs.

## Represent external dependencies with imported targets, including configuration-specific usage requirements

CMake find modules should expose an external dependency as a target that carries the dependency's complete usage contract. Consumers should link that target instead of separately repeating raw library lists, include directories, transitive dependencies, and platform-specific link options.

Merged MR !26122, authored by John Thacker and approved/merged by Gerald Combs, adds `MaxMindDB::MaxMindDB` and tracks the Debug and Release MaxMindDB artifacts separately on Windows. That prevents an MSVC Debug executable from accidentally linking the Release library and mixing incompatible CRT variants. Merged MR !26132, also authored by John Thacker and approved/merged by Gerald Combs, applies the same target-oriented design to libsinsp. Because libsinsp is represented by a bundle of libraries rather than one file suitable for `IMPORTED_LOCATION`, the accepted implementation uses an `INTERFACE IMPORTED` target and attaches its include directories, Debug/optimized library sets, and the narrowly scoped MSVC `/IGNORE:4099` option to that target. Consumers then link `sinsp::sinsp` without reproducing those details.

**Implementation rule:** put per-configuration artifact selection and transitive compile/link requirements on the imported dependency target. If an external dependency is a collection of libraries rather than one importable artifact, an `INTERFACE IMPORTED` target can model the bundle. Scope unavoidable warning suppressions to the dependency target that causes them rather than applying them globally.

**Confidence:** Very high. Two merged master CMake changes authored by John Thacker and approved/merged by Gerald Combs.

## Detect the dependency layout or toolchain contract, not merely the operating system

Platform macros such as `WIN32` describe the target OS, but they do not imply one packaging model, compiler runtime, or Debug/Release library layout. CMake logic should test the condition that actually guarantees the artifact arrangement it expects.

Merged MR !26216, authored and merged by John Thacker, restricts the separate Debug-library search used by several Windows dependency find modules to `USE_REPOSITORY`. The repository/MSVC-style dependency bundles provide distinct configurations; MSYS2 and cross-compilation environments generally provide a single library set and should not be forced through the repository-specific lookup merely because the target is Windows.

**Implementation rule:** when dependency discovery differs by package source, toolchain, CRT model, or repository bundle, condition on that semantic property. Avoid using the target OS as a proxy for a narrower build-environment assumption.

**Confidence:** Very high. Merged master build-system correction authored and merged by John Thacker.

## Compile-time API availability does not guarantee deployment-runtime availability

A configure or compile-time symbol probe can prove that the SDK exposes an API without proving that every runtime supported by the resulting binary implements it. This matters on platforms where Wireshark is intentionally built against a newer SDK while retaining an older deployment target.

Merged MR !25677 initially added `strchrnul` through the usual capability-test/fallback pattern. Merged follow-up !25680, authored and merged by John Thacker, corrects the macOS case: `strchrnul` is present in newer SDKs but only exists at runtime starting with macOS 15.4, so `ws_strchrnul()` uses `__builtin_available(macOS 15.4, *)` before calling it and otherwise executes the generic fallback.

**Implementation rule:** for APIs introduced after the minimum deployment runtime, distinguish header/SDK availability from runtime availability. Keep a fallback reachable at runtime or raise the deployment requirement explicitly; do not let a successful configure probe silently create a binary that links or calls an unavailable runtime symbol on supported older systems.

**Confidence:** Very high. Merged master portability fix authored and merged by John Thacker, directly correcting the earlier compile-time-only capability assumption.

## ABI-affecting build options require ABI agreement from dependencies

A build option that changes the representation of a type exposed in library interfaces is not an isolated compiler choice. Every linked dependency that exchanges that type, or a structure containing it, must be built with a compatible ABI.

Merged MR !25538 adds an experimental option to force 64-bit `time_t` for Y2038 compliance and explicitly warns that libpcap and any other library exposing `time_t` in its API must use the same `time_t` size. The implementation includes a libpcap compatibility check rather than assuming that successfully compiling Wireshark itself proves the resulting binary is ABI-safe. During review Gerald Combs also questioned a Win32-specific branch that was irrelevant to Wireshark's supported 64-bit Windows targets; it was removed.

**Implementation rule:** when a CMake/toolchain option changes fundamental ABI-visible types, validate ABI compatibility at dependency boundaries and limit platform-specific handling to configurations Wireshark actually supports. A successful local compile is not sufficient evidence when externally built libraries participate in the affected ABI.

**Confidence:** Very high. Merged master build-system change by John Thacker with direct Gerald Combs review.

## Validate required toolchain capabilities before downstream feature probes

When the project has a non-negotiable language or compiler requirement, check that requirement explicitly near configuration startup. Do not wait for an unrelated feature probe or package check to fail later with an obscure symptom caused by an incapable compiler.

Merged MR !25231, authored and merged by Guy Harris, adds explicit C17 and C++17 capability checks and emits actionable fatal diagnostics on Solaris rather than allowing later checks to fail mysteriously with the vendor compiler. The surrounding change documents why supported GCC/Clang toolchains satisfy the requirement while the Sun/Oracle compiler does not.

**Implementation rule:** test mandatory compiler/language capabilities before dependent configure logic and diagnose the actual prerequisite that is missing. A later feature-test failure should not be the first indication that the selected compiler cannot meet Wireshark's baseline language contract.

**Confidence:** Extremely high. Merged master build-system design authored and merged by Guy Harris.

## Dependency discovery must follow the target ABI, not the host's default search layout

On multi-architecture systems, a generic package/library search can locate an installed artifact for the wrong architecture and produce failures that look like missing packages or inexplicable link incompatibilities. Discovery logic must search the layout that corresponds to the binary being built.

Merged MR !25224, authored and merged by Guy Harris, handles Solaris's `/usr/lib/${isa}` arrangement by deriving the target ISA for 64-bit builds and setting `CMAKE_LIBRARY_ARCHITECTURE`, so CMake and pkg-config do not accidentally select 32-bit metadata/libraries. Merged MR !25256 provides a related Windows example: for the platform `bcrypt` system library, using the canonical linker name avoids a generic `find_library()` result tied to an inappropriate SDK architecture.

**Implementation rule:** make library/package lookup architecture-aware whenever host installations contain multiple ABIs. For true platform system libraries, prefer the platform's stable linker contract when generic filesystem discovery can select an architecture-specific artifact incorrectly.

**Confidence:** Extremely high for the Solaris rule (Guy Harris authored/merged); high corroboration from merged Windows build work.

## External API types belong to the external API

Do not locally recreate or forward-declare an external library's public typedef by assuming the library's private representation. Different implementations of the same API can legally map an opaque/public type to different structure tags, and a hand-written replacement can therefore compile against one provider while conflicting with another.

Merged MR !25201, authored by Guy Harris and merged after John Thacker review, removes Wireshark's attempted local typedef of `krb5_context`. The assumption matched one Kerberos implementation but conflicted with Heimdal as shipped by NetBSD 10, where the public typedef names a different structure type. The discussion explicitly considered including the Kerberos header at the interface that exposes the type instead of reconstructing the typedef locally.

**Implementation rule:** if a Wireshark header or API exposes a dependency-owned type, obtain that type from the dependency's authoritative public header (and propagate that dependency appropriately), or redesign the interface so the external type stays private. Never duplicate an opaque typedef based on one implementation's internal tag name merely to avoid including its defining public header.

**Confidence:** Extremely high. Merged master portability fix authored by Guy Harris and merged by John Thacker.