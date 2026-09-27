# Wireshark Compiler Feature Selection Conventions

This file records durable conventions for selecting compiler-specific attributes, pragmas, and compatibility paths. Current upstream build policy remains authoritative.

## Compatibility macros are not reliable compiler identity or feature guarantees

Compilers commonly define compatibility macros for other compiler ecosystems, but that does not imply they implement every pragma, attribute, or command-line behavior associated with those ecosystems. Conversely, a compiler mode can omit a compatibility macro that another mode of the same compiler defines. Do not infer a compiler-specific feature path from an impersonated macro when the implementation actually depends on the real compiler family.

Merged master MR !12820 fixes attribute selection for Clang, including clang-cl. Clang can advertise compatibility with GCC or MSVC while not implementing the full pragma surface expected from those compilers, and clang-cl does not provide `__GNUC__` in the way the previous selection logic assumed. The accepted code explicitly recognizes `__clang__` and sends Clang through the `__attribute__` path used for `unused` and `warn_unused_result`.

**Implementation rule:** prefer direct capability probes when a usable compile-time feature test exists. When selecting a deliberately compiler-family-specific compatibility path, test the actual compiler identity (for example `__clang__`) rather than relying on GCC/MSVC compatibility macros as proxies.

**Review rule:** exercise alternate front ends/modes such as clang-cl separately from ordinary Clang or GCC builds. A condition that works under one compatibility personality may select a different and unsupported pragma/attribute path under another.

**Confidence:** High. Merged master portability fix with a concrete clang-cl failure mode and an explicit rationale for the compiler test.


## Distinguish genuine GCC from compilers that merely advertise GNU compatibility

A compiler-specific warning pragma must be gated by the compiler that actually implements it, not merely by a compatibility macro such as `__GNUC__`. Intel Classic, Intel LLVM and Clang can advertise parts of the GNU dialect while having different predefined macros and diagnostic support.

Merged master MR !10329 adds an explicit Wireshark GCC-version identity helper that excludes Clang, Intel Classic and Intel LLVM before enabling a GCC-only diagnostic. During review John Thacker specifically caught both the Intel LLVM case and the correct Clang predefined macro spelling (`__clang__`). Merged !10318 then uses the project compiler-version helper to guard `-Wstringop-overflow`, which exists only in sufficiently new GCC.

**Implementation rule:** centralize compiler-family/version detection in project helpers and use those helpers at pragma/attribute sites. Treat `__GNUC__` as a dialect-compatibility signal, not proof that the compiler is GCC.

**Review rule:** when changing compiler feature tests, check alternate vendors and compatibility modes explicitly, including LLVM-based replacements for older vendor compilers.

**Confidence:** Very high. Merged portability work with direct John Thacker review correcting concrete compiler-identification edge cases.


## Suppress understood compiler false positives narrowly with project diagnostic helpers

Merged MR !6984, authored by John Thacker, addresses a GCC 12.1 `-Wstringop-overread` false positive in Qt byte-array/string conversion paths. João Valverde specifically directed the change toward Wireshark's `DIAG_OFF` / `DIAG_ON` helpers rather than raw ad-hoc pragmas; Guy Harris then traced the relevant Qt overload and null/length behavior in detail, supporting the conclusion that the warning was spurious for these paths. The merged implementation surrounds only the affected expressions, version-gates the workaround, and documents why it exists.

**Implementation rule:** after establishing that a compiler diagnostic is a false positive, keep the suppression local to the affected expression and use Wireshark's diagnostic-control helpers. Document the compiler/version and reason. Do not globally disable the warning or rewrite correct code into a less idiomatic form solely to satisfy one optimizer diagnostic.

**Confidence:** Extremely high. Merged change with detailed review from John Thacker, João Valverde, and Guy Harris.
