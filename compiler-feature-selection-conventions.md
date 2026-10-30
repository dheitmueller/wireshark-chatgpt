# Wireshark Compiler Feature Selection Conventions

This file records durable conventions for selecting compiler-specific attributes, pragmas, and compatibility paths. Current upstream build policy remains authoritative.

## Compatibility macros are not reliable compiler identity or feature guarantees

Compilers commonly define compatibility macros for other compiler ecosystems, but that does not imply they implement every pragma, attribute, or command-line behavior associated with those ecosystems. Conversely, a compiler mode can omit a compatibility macro that another mode of the same compiler defines. Do not infer a compiler-specific feature path from an impersonated macro when the implementation actually depends on the real compiler family.

Merged master MR !12820 fixes attribute selection for Clang, including clang-cl. Clang can advertise compatibility with GCC or MSVC while not implementing the full pragma surface expected from those compilers, and clang-cl does not provide `__GNUC__` in the way the previous selection logic assumed. The accepted code explicitly recognizes `__clang__` and sends Clang through the `__attribute__` path used for `unused` and `warn_unused_result`.

**Implementation rule:** prefer direct capability probes when a usable compile-time feature test exists. When selecting a deliberately compiler-family-specific compatibility path, test the actual compiler identity (for example `__clang__`) rather than relying on GCC/MSVC compatibility macros as proxies.

**Review rule:** exercise alternate front ends/modes such as clang-cl separately from ordinary Clang or GCC builds. A condition that works under one compatibility personality may select a different and unsupported pragma/attribute path under another.

**Confidence:** High. Merged master portability fix with a concrete clang-cl failure mode and an explicit rationale for the compiler test.

## Gate compiler diagnostic pragmas by real vendor and introduction version

A compiler's generic `__GNUC__` compatibility macro is insufficient evidence that it recognizes a GCC-specific diagnostic name; using an unknown name under `-Werror=pragmas` itself breaks the build. Merged !10318 restricted `stringop-overflow=` diagnostic control to GCC at least 7.1, using the project's version helper instead of a hand-written check that might accidentally exclude all later major versions.

Merged !10329 then tightened that contract: the accepted `WS_GCC_VERSION` classification excludes Clang (`__clang__`) and both Intel Classic (`__INTEL_COMPILER`) and Intel LLVM (`__INTEL_LLVM_COMPILER`), before gating the pragma on `WS_GCC_VERSION >= 70100`. John Thacker specifically called out the second Intel compiler family and the correct spelling of the Clang macro in review.

**Implementation rule:** identify the actual compiler family and the first version supporting a diagnostic before emitting a pragma, reusing Wireshark's central compiler-test macros. Do not equate GNU dialect compatibility with GCC-only pragma support, or write ad hoc major-version expressions that fail for later versions.

**Confidence:** Very high — two merged fixes with concrete GCC/Clang/Intel portability risks and explicit John Thacker review.
