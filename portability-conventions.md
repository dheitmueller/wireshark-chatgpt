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

## Do not package build-host system DLLs as application-private dependencies

On Windows, a deployment tool may discover a DLL used by the build host and copy it beside the application even when that DLL is fundamentally supplied and versioned by the target operating system. Because application-local DLL lookup can take precedence over the system copy, doing so can accidentally bind the package to the build host's OS revision and break supported older targets.

Merged master MR !24709, authored by John Thacker, fixes exactly this problem for Qt's handling of Windows ICU. `windeployqt` copied the build machine's `icuuc.dll`, whose newer wrapper expected `icu.dll` that does not exist on still-supported Windows 10 1809/Windows Server 2019 systems. The accepted packaging change excludes that build-host DLL and lets the target operating system supply its compatible system implementation. Merged MR !24712 propagates the exclusion to PortableApps packaging.

**Implementation rule:** distinguish redistributable application dependencies from OS-owned system libraries during packaging. Do not blindly ship a build-host copy merely because deployment tooling discovers it; account for loader precedence and verify that packaged binaries do not create a dependency on a newer build-host OS ABI than the supported target matrix.

**Confidence:** Very high. Merged master portability fix with a merged second-package backport and a concrete supported-target runtime failure.

## Keep Debian packaging recipes compatible with their declared shell

`debian/rules` and helper commands execute in the shell selected by the packaging environment, which is normally `/bin/sh`; do not rely on Bash-only parameter expansion merely because it works on a developer's system. Either express the transformation portably or deliberately invoke the shell whose syntax is required.

Merged master MR !23490 initially derived Debian package names with Bash `${var//pattern/replacement}` syntax. John Thacker's merged follow-up !23493 replaces that bashism with a portable `sed` transformation specifically because the rules file is not a Bash script.

**Implementation rule:** treat packaging recipes as constrained by the shell they actually declare/use. In Debian rules and similar `/bin/sh` contexts, prefer POSIX shell syntax and portable utilities; use Bash-specific expansion only when Bash is explicitly part of the execution contract.

**Confidence:** Very high. Corrective master change authored and merged by John Thacker immediately after the nonportable form was introduced.