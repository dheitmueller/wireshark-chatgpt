# Dependency Capability Detection Conventions

This file records durable conventions for detecting external dependency behavior in Wireshark's build system. Current upstream code and build configuration remain authoritative.

## Probe the API behavior you need instead of inferring it from version metadata

When compatibility depends on the actual signature or behavior exposed by an installed dependency, prefer a configure-time capability probe over a version-number comparison if downstream vendors can backport API changes without changing the advertised upstream version.

Merged MR !22507, authored and merged by Guy Harris, handles an especially clear case. Lua 5.4.5 temporarily changed `lua_resetthread()` from one argument to two, and Fedora backported the change to packages whose `LUA_VERSION_RELEASE_NUM` still identified an earlier release. A version test therefore could not reliably identify which signature was present. The accepted Wireshark solution compiles a small source fragment calling the two-argument form and uses the result to select the call signature in `wslua`.

The configure logic also documents an important limitation: CMake's available source-compile check links as well, so unrelated link failure can make the capability probe conservative. Capability tests should therefore set the required includes/libraries deliberately and document cases where the probing mechanism is broader than the property being tested.

**Implementation rule:** test the dependency contract that the source code actually needs. Treat version macros as reliable only when the relevant ecosystem guarantees that API state and version identity remain coupled; otherwise prefer a narrowly scoped compile/configure probe and make its assumptions explicit.

**Confidence:** Extremely high. Merged master portability fix authored and merged by Guy Harris specifically because real downstream packaging invalidated version-number inference.
