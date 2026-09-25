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
