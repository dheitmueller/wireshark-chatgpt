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

## Query capabilities directly; do not parse a human-readable capability summary

During merged master MR !6240, compile/runtime feature reporting was converted from direct string concatenation to a structured feature list. In review, David Perry pointed out an existing path that decided whether Npcap was present by examining text produced by a routine that itself already knew that fact. João Valverde explicitly agreed that the code should use a direct semantic test rather than parse presentation text, preferably as a separate focused change. The later merged capability helper work follows that direction.

**Implementation rule:** if program behavior depends on whether a dependency/runtime capability is present, expose and call a predicate or structured query for that capability. Human-readable version/About text is an output format, not a machine-facing detection API; parsing it creates accidental coupling to wording and formatting.

**Review rule:** when refactoring diagnostic/version output, search for callers that parse the old text. Either migrate them to a semantic capability API in the same series or record the follow-up explicitly; do not preserve string parsing as the long-term contract.

**Confidence:** Very high. Direct João Valverde review on a merged master architecture change, aligned with the structured feature-reporting design.

## Package identity does not prove the compatibility API installed

Merged stable MR !4482 handles distributions that provide minizip-ng compatibility code under the traditional minizip package name. Wireshark probes the concrete header member it needs and selects compatibility code from the observed API rather than from package branding.

**Implementation rule:** when distributions can substitute implementations behind the same dependency name, detect the exact declaration, member, or signature the source uses. Package identity alone is not a capability contract.

**Confidence:** High. Merged maintained-branch backport, consistent with the direct-capability guidance above.


## Strengthening: package identity can hide a different compatibility implementation

Merged master MR 4275 is the master-origin evidence for the Minizip compatibility rule later seen through stable MR 4482. Some distributions expose minizip-ng compatibility code under the traditional minizip package/library identity, but the `zip_fileinfo` member spelling differs. Wireshark therefore probes the concrete struct member in the installed header and selects the compatibility path from observed API shape.

**Strengthened rule:** when downstream distributions can substitute implementations behind the same dependency name, make the configure test about the exact declaration/member/signature the source consumes. The installed package name is provenance, not a sufficient API capability test.

**Confidence:** Very high. Merged master implementation by João Valverde, later corroborated on a maintained branch.

## Probe required C-library semantics, not merely symbol availability

Merged master MR 4263 uses a configure-time run test to verify the C99 `snprintf`/`vsnprintf` truncation-return contract that Wireshark depends on. If the behavior is absent, configuration fails with the target system and compiler identified instead of allowing a build whose formatting semantics are incompatible.

**Implementation rule:** if correctness depends on a library function's behavior rather than its existence, use a semantic configure/run probe when the build environment permits it. Make a mandatory contract fail early and diagnostically rather than relying on platform/version folklore.

**Confidence:** Very high. Merged master portability/build change by João Valverde, followed by MinGW adjustments that make the probe run under the intended runtime semantics.
