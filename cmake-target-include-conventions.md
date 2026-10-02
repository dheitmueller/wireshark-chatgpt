# CMake Target Include Conventions

This file records focused CMake include-path conventions extracted from accepted Wireshark changes.

## Put dependency include paths on the consuming target and mark external headers as SYSTEM

Merged master MR !2104, authored by Gerald Combs, replaces several directory-wide `include_directories()` uses with `target_include_directories()`. Third-party paths such as libxml2, GnuTLS, libgcrypt, Lua, compression libraries, and Qt-side dependencies are attached to the targets that consume them and are marked `SYSTEM` with the appropriate `PRIVATE` or `PUBLIC` scope.

The immediate motivation included avoiding compiler diagnostics originating in third-party headers, such as macOS nullability warnings from libxml2, without globally suppressing useful warnings in Wireshark code.

**Build rule:** attach external include requirements to the target that consumes them instead of leaking them through directory scope. Mark genuine third-party headers `SYSTEM` when their diagnostics should not be treated as Wireshark source warnings.

**Scope rule:** choose `PRIVATE` unless the target's public headers expose dependency types or otherwise require the include path transitively; do not make dependency includes public by default.

**Confidence:** Very high. Merged master CMake cleanup authored by Gerald Combs.
