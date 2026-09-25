# Wireshark Build-Baseline Compatibility Conventions

This file records durable rules for preserving Wireshark's declared language and dependency baselines in source and CI. Current upstream build policy remains authoritative.

## Source must honor both the declared C++ standard and supported Qt API baseline

Code can compile with a modern compiler and still violate Wireshark's declared language or dependency baseline. C++ standard-library availability and Qt API signatures are separate compatibility constraints and both must match the oldest supported configuration.

Merged !9822 introduced `std::as_const`, which requires C++17 even though Wireshark's project baseline at the time was C++11. Merged !9836 removed that dependency but used `qsizetype` as a `QList` index; that still failed on Qt 5 because the relevant `QList::at()` interface used `int`. Merged !9844, authored by Nicolás Alvarez, supplied the accepted cross-version form: a C++11 range-for over `qAsConst(indices)`, compatible with Qt 5 and Qt 6.

**Implementation rule:** before using a language/library helper, check both the project's declared C/C++ standard and the minimum supported dependency version. For Qt code, do not infer API compatibility merely because a type exists in both Qt generations; container signatures and typedef roles can differ.

## Explicitly exercise the minimum supported combination in CI when compatibility code changes

The same !9822 → !9836 → !9844 sequence shows that a green general pipeline can miss the configuration that defines the minimum compatibility contract. In !9844, Nicolás Alvarez explicitly noted that the prior changes had passed available GitLab CI despite breaking a supported Qt 5 build and requested validation across both Qt generations.

**CI rule:** when code sits on a compatibility boundary—minimum language standard, oldest Qt, optional dependency, or platform-specific API—identify a job or targeted build that actually exercises that boundary. A newer environment that accepts the source is not proof that the supported minimum still works.

**Review rule:** when a compatibility failure is found outside the main pipeline, consider whether a lightweight targeted configuration can permanently cover the missed baseline. A correction for one compiler error should be tested against the full baseline rather than only the environment that exposed the first failure.

**Confidence:** Very high. Three consecutive merged changes expose both source-level and CI-matrix failure modes, with the final correction explicitly motivated by Qt 5/Qt 6 compatibility.

## Evaluate minimum-version changes against the declared support matrix

Merged !9697 raises the Qt minimum from 5.10 to 5.11 after review compared the proposed baseline against distributions that then-current Wireshark actually supported. John Thacker noted that RHEL 8 already provided Qt 5.11+ and that older RHEL generations cited in discussion could not build that Wireshark generation because of other dependency requirements. The change deliberately stopped at Qt 5.11 because no code required 5.12 and Debian Buster still shipped 5.11.3.

**Review rule:** judge a dependency-baseline change against the project's declared supported platforms and their real dependency versions. Require a concrete technical or maintenance reason for the bump, and do not infer indefinite support solely from the existence of older deployed systems.

**Confidence:** High. Merged change with substantive review discussion and explicit distribution/version evidence.

## Treat generated-code warning behavior as part of the supported compiler/generator matrix

Merged !9272 simplified the display-filter Flex scanner and removed old warning workarounds. Joakim Karlsson then reproduced unused-parameter warnings in Flex-generated functions on RHEL 7.9 with GCC 4.8.5. João Valverde asked for the exact environment, added a narrow generated-Flex diagnostic suppression in merged follow-up !9303, and Joakim confirmed that it fixed the affected build.

**Implementation rule:** when a warning comes from generator-emitted boilerplate that project code cannot reasonably rewrite, keep any suppression narrow and generator-specific rather than disabling the warning globally.

**Review rule:** after deleting a generator workaround, exercise the oldest relevant generator/compiler combination or get confirmation from an environment that still uses it. A clean modern build is not proof that generated output is warning-clean across the support matrix.

**Confidence:** High. The regression was reported against a concrete older environment and the targeted follow-up was merged and explicitly verified there.
