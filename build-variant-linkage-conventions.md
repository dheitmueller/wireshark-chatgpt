# Wireshark Build-Variant and Linkage Conventions

This file records durable conventions for optional-feature build variants and executable/static-library linkage. Current upstream CMake and CI definitions remain authoritative.

## Treat feature-disabled builds as supported semantic configurations, not just compile curiosities

When an optional feature is disabled, every layer that depends on it must agree: source references must be guarded or omitted, related UI actions must not remain active, and the resulting application must still have a coherent runtime state. A menu container being disabled is not sufficient if individual actions still participate in state or shutdown logic.

Merged master MR !13582 added a binary-plugin installation action and initially exposed a no-plugin link failure because plugin helper definitions are not built when plugin support is disabled. Guy Harris explicitly called out that plugin-dependent code should be protected by `HAVE_PLUGINS` and that the Install Plugin action should not exist in a build without plugin support. The accepted implementation guards the plugin-specific include and implementation. In the same discussion João Valverde explained that the CI "No options" job is intended specifically to catch failures when features are disabled, and Guy clarified that it means disabling features that normally default on rather than merely invoking CMake without command-line options.

Merged master MR !13572, authored and merged by John Thacker, shows the runtime side of the same contract for `HAVE_LIBPCAP`: no-libpcap builds disable the individual capture actions, not just the Capture menu. Otherwise toolbar actions remained visibly enabled and, more seriously, shutdown logic could interpret `actionCaptureStop` as an active capture state and prevent quitting. Release-4.2 backport !13573 carries the same correction. Merged master MR !13571, also authored and merged by John, expands the No Options CI configuration to explicitly disable newly added optional dependencies and `mmdbresolve` so the job continues to exercise the intended minimal feature surface.

**Architecture rule:** an optional feature's compile-time condition is part of the application's state model. Audit link references, registrations, menus/actions, startup initialization, and shutdown checks together when the feature is absent.

**CI rule:** maintain a deliberately minimal/no-options configuration that turns off optional features, including newly introduced defaults and helper targets. Its purpose is to exercise feature-absence paths that ordinary developer builds usually miss.

**Review rule:** for code added under an optional subsystem, ask both "does a full build work?" and "what symbols/actions/state remain when this feature is disabled?" A successful default pipeline does not prove the disabled variant is coherent.

**Confidence:** Extremely high. The merged series includes direct Guy Harris review defining the feature-off contract, John Thacker master fixes, an accepted stable backport, and CI specifically maintained to exercise the disabled state.

## Static-library consumers must link the transitive system dependencies of the objects they pull in

A static archive normally does not make its own dependent system libraries appear automatically on the final executable's link line. If an object pulled from the archive calls a symbol supplied by another library, the final consumer must link that dependency explicitly (or consume a CMake target whose usage requirements propagate it). A platform where the symbol happens to live in the default system runtime does not prove portability.

Merged master MR !13570, authored and merged by Guy Harris, fixes `dumpcap` on FreeBSD after statically linking `libwsutil`. Pulling in the JSON-dumper object also pulled in a call to `isfinite()`. On macOS that symbol was available without an extra library, but on FreeBSD it resides in `libm`; because the statically linked `libwsutil` archive did not itself cause its dependency to be linked, `dumpcap` had to add `${M_LIBRARIES}` explicitly.

This complements the target-oriented guidance in `build-conventions.md`: where practical, dependency-owning CMake targets should propagate their usage requirements. The !13570 case records the underlying link-model invariant that must still be satisfied when a static archive is consumed directly.

**Implementation rule:** trace the actual object/dependency graph for static archives and ensure the final target links every required external/system library. Prefer target usage requirements where they can express that relationship reliably; otherwise put the dependency on the final consumer explicitly.

**Portability rule:** validate static-link paths on platforms where common math, threading, networking, or other runtime symbols are separate libraries. Do not infer that a link dependency is unnecessary because one OS folds the symbol into its default system library.

**Review rule:** when adding a new call inside a utility archive, consider which executables might begin pulling that object into a static link and whether the new symbol introduces a library dependency they do not currently list.

**Confidence:** Extremely high. The rule is directly explained and implemented in a merged master portability fix authored and merged by Guy Harris.

## Privileged executables require a loader-safe relocation strategy, not just relative RPATH

A relative ELF RPATH/RUNPATH can make ordinary applications relocatable, but a privileged executable can run under dynamic-loader security restrictions that ignore nonstandard or relative library search paths. Merely setting install-time RPATH to `$ORIGIN` is not sufficient for a setuid/capability-bearing helper such as `dumpcap`.

Merged master MR !9793 (João Valverde) resolves this by building a static `wsutil_static` variant and linking `dumpcap` against it while letting other installed Wireshark components use relative paths as appropriate. It updates `include/ws_symbol_export.h` for `ENABLE_STATIC` and records relocatable installation support in release notes. The immediately preceding take-2 implementation was **reverted** in merged !9792 because it broke the MSYS2 build; that failed variant is not an exemplar. Later compiler/platform cleanups !9794 and !9791 separately show the need to distinguish the MSYS2 target environment (including `MSYSTEM_CARCH`) and gate x86-only SSE4.2 flag probing on actual target architecture.

**Design/review rule:** first identify the security context and dynamic-loader behavior of each executable; choose static/shared linkage per executable rather than imposing one global RPATH assumption. Ensure the statically linked variant propagates its full dependency closure, export/import annotations suit static builds, and both privileged Linux execution and Windows/MSYS2 cross-platform CI are exercised before claiming relocation support. Align packaging and release notes with user-visible changes.

**Confidence:** Very high. Accepted third implementation by João Valverde, with an explicit preceding merged revert documenting a portability regression. Historical build flags may have evolved.
