# Wireshark Public-Header Validation Conventions

This file records durable conventions for validating public header interfaces. Current upstream source remains authoritative.

## Public headers should compile independently of higher-layer headers

A public library header is part of that library's supported interface and must not compile only because a consumer happened to include headers from a higher layer first. Build validation should exercise public headers in isolation with only the dependencies that library is legitimately allowed to expose.

Merged MR !22933 adds an explicit compile check for `wsutil` public headers without first including libwiretap or libwireshark headers. During review, Guy Harris argued that this should not merely be a Debian-packaging check: the same class of public-header self-sufficiency should ideally be tested on all build platforms because platform-specific header problems can otherwise escape one environment and fail another. The accepted MR provides an executable guard for the dependency-boundary invariant and later discussion notes CMake's `VERIFY_INTERFACE_HEADER_SETS` as another possible mechanism for such checks.

**Implementation rule:** treat public-header self-sufficiency as a build/test invariant. A public header should include the authoritative declarations needed for the types and macros it exposes, without relying on include order or accidental higher-layer transitive includes. Where practical, compile public headers independently in CI across supported build environments rather than relying on downstream packaging to discover breakage.

**Confidence:** Extremely high. Merged master build validation with direct, substantive Guy Harris review advocating broader cross-platform enforcement.

## Public C headers must provide their own C++ linkage boundary

A public header that declares C functions and is intended for external use must be directly includable from C++ without making each consumer supply its own `extern "C"` wrapper. Put the linkage guards around the declarations owned by the header, after includes that may themselves contain C++ constructs; wrapping transitive headers inside an external consumer-side `extern "C"` block can make otherwise valid C++ headers fail to compile.

Merged MR !22586 fixes `epan/reassemble.h` and `epan/ptvcursor.h` after C++ users received mangled-name link failures. The MR specifically notes that consumer-side wrapping was not a sound workaround for `ptvcursor.h`, because a transitively included GLib header on Windows contains C++ `template<>` declarations that cannot appear inside an `extern "C"` block. The accepted change adds `#ifdef __cplusplus` linkage guards in the Wireshark public headers themselves, and John Thacker approved and merged it.

**Implementation rule:** if a public Wireshark C header exposes linkable C symbols, make the header itself safe for direct inclusion by C++ consumers. Do not require downstream code to wrap the whole include, and do not place unrelated/transitive C++ declarations inside the C linkage block.

**Confidence:** Very high. Merged master interoperability fix, approved and merged by John Thacker, for a concrete downstream C++ link/Windows-header failure.

## Derive installed public-header sets from explicit public-interface evidence

Lists of headers installed for external consumers are part of the plugin/API contract and should not depend on a stale manually maintained inventory. Prefer deterministic discovery from explicit public-interface evidence, while retaining a deliberate opt-in mechanism for headers that are public for data structures or tap/plugin interchange rather than exported functions.

Merged MR !22579 replaces hand-maintained ASN.1/public-header lists with generated lists. The accepted generator recognizes `WS_DLL_PUBLIC`, tap-oriented structures, and an explicit `PUBLIC_HEADER` marker. Review discussion from Michael Mann called out the important compatibility case: some third-party-plugin headers contain shared data structures but no exported function and therefore cannot be discovered from `WS_DLL_PUBLIC` alone. The final merged design preserves an explicit marker for those non-obvious public headers and was merged by John Thacker.

**Implementation rule:** automate public-header inventory from authoritative source markers, but do not equate "contains an exported function" with "is the entire public API." Provide an explicit source-level declaration for intentionally public structure/tap/plugin headers and treat changes to the installed set as API-compatibility changes.

**Confidence:** Very high. Merged master build/tooling change by Gerald Combs with substantive maintainer discussion about third-party plugin compatibility and final merge by John Thacker.
