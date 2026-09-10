# Wireshark Portability Conventions

This file records durable portability conventions extracted from accepted upstream Wireshark changes. Current upstream source and build configuration remain authoritative.

## Guard code by the platform where it is semantically applicable, not by the compiler that happens to diagnose it

Conditional compilation should describe whether an implementation is meaningful on a target platform. Do not key a platform exclusion to a particular compiler merely because that compiler is the one that exposed an unused-function or similar warning.

Merged MR !24883 fixes `dumpcap` by excluding a pipe-reading helper on all Windows builds, not merely MSVC builds. John Thacker explicitly distinguished the two questions during review: the helper is not used on any Windows implementation, while GCC and Clang simply diagnose the unused static function more aggressively than MSVC. Guy Harris gave the authoritative architectural reason: Wireshark reads capture pipes differently on UN*X and Windows, so the routine itself is not applicable on Windows.

**Implementation rule:** put the `#if`/`#ifdef` boundary around the semantic platform distinction. Compiler-specific conditionals are appropriate only when behavior truly depends on that compiler; they should not be used as a proxy for platform applicability or to silence a warning that reveals dead platform code.

**Confidence:** Extremely high. Merged master portability fix with direct Guy Harris design feedback and John Thacker authorship/merge.

## Keep timestamps in Wireshark's native time representation until conversion is actually required

Do not prematurely split or narrow timestamps into 32-bit epoch seconds, fractional integers, or floating-point seconds when the surrounding API can carry `nstime_t`. Preserve the full native representation internally and convert at the consumer boundary with the shared nstime helpers.

Merged MR !24870, authored and merged by John Thacker, changes TCP stream and RLC graph tap records to retain `nstime_t` directly instead of casting seconds into unsigned 32-bit fields and carrying separate fractional components. The previous representation triggered Coverity Y2038/Y2106 findings; the accepted implementation uses the existing nstime-to-seconds helper only when the graph needs a `double`.

**Implementation rule:** represent packet/capture time with `nstime_t` through internal state and tap interfaces where practical. Convert to floating point or narrower units only at an interface that requires that representation, rather than baking epoch-width limitations into stored state.

**Confidence:** Very high. Merged master cleanup authored and merged by John Thacker, motivated by concrete time-width defects.

## Stable-branch compatibility must be tested against that branch's supported variants

A backport can compile and pass on master while failing on an older maintained branch because the branch intentionally supports older dependency, UI, or language variants. CI for a stable branch should retain coverage for those branch-specific supported configurations when affected code changes.

Merged MR !24867, authored and merged by John Thacker, adds a Qt 5 build job to release-4.4 merge requests that touch Qt code. Master had moved farther toward Qt 6 and newer C++ requirements, while release-4.4 still supported Qt 5 on more platforms; testing only the modern configuration would therefore leave a supported stable-branch path unvalidated.

**Implementation rule:** when backporting or changing code on a maintained release branch, identify the compatibility matrix of that branch rather than inheriting master's assumptions. Trigger legacy-variant jobs automatically for changes that can affect them, with manual fallback coverage where appropriate.

**Confidence:** Very high. Merged stable-branch CI change authored and merged by John Thacker specifically to preserve supported compatibility coverage.
