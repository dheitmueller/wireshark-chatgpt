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