# Dependency Capability Detection Conventions

This file records durable conventions for detecting external dependency behavior in Wireshark's build system. Current upstream code and build configuration remain authoritative.

## Probe the API behavior you need instead of inferring it from version metadata

When compatibility depends on the actual signature or behavior exposed by an installed dependency, prefer a configure-time capability probe over a version-number comparison if downstream vendors can backport API changes without changing the advertised upstream version.

Merged MR !22507, authored and merged by Guy Harris, handles an especially clear case. Lua 5.4.5 temporarily changed `lua_resetthread()` from one argument to two, and Fedora backported the change to packages whose `LUA_VERSION_RELEASE_NUM` still identified an earlier release. A version test therefore could not reliably identify which signature was present. The accepted Wireshark solution compiles a small source fragment calling the two-argument form and uses the result to select the call signature in `wslua`.

The configure logic also documents an important limitation: CMake's available source-compile check links as well, so unrelated link failure can make the capability probe conservative. Capability tests should therefore set the required includes/libraries deliberately and document cases where the probing mechanism is broader than the property being tested.

**Implementation rule:** test the dependency contract that the source code actually needs. Treat version macros as reliable only when the relevant ecosystem guarantees that API state and version identity remain coupled; otherwise prefer a narrowly scoped compile/configure probe and make its assumptions explicit.

**Confidence:** Extremely high. Merged master portability fix authored and merged by Guy Harris specifically because real downstream packaging invalidated version-number inference.

## A compiler option is available only when its required toolchain components are available

Recognizing a command-line option is not always sufficient to make that option usable. Some compiler features depend on optional libraries, runtime components, SDK pieces, or linker support that can be installed separately from the compiler itself.

Merged MR !16165 initially made MSVC `/Qspectre` unconditional on the assumption that supported Visual Studio versions accepted the flag. Review identified that Spectre-mitigated libraries are an optional Visual Studio component, so flag recognition alone did not imply a usable build environment. The subsequent merged MR !16173 reverted the unconditional setting and returned `/Qspectre` to Wireshark's tested common flags, with an explicit comment that the optional Spectre component requires availability detection.

**Implementation rule:** capability checks for compiler or linker features must exercise enough of the actual build contract to cover required auxiliary components, not merely test whether the compiler parses an option. If a feature requires optional libraries or SDK/toolchain packages, either probe the complete compile/link behavior or keep the feature conditional on an equivalent verified capability.

**Review rule:** when broadening a compiler flag from probed/conditional to unconditional, verify installation prerequisites as well as compiler-version documentation. A feature present in the compiler product can still be absent from a particular installed toolchain.

**Confidence:** Very high. The initial merged assumption was explicitly corrected by the maintainer and superseded by a merged revert that documents the missing prerequisite.
