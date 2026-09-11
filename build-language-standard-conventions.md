# Wireshark Build Language-Standard Conventions

This file records durable conventions for scoping C and C++ language requirements in Wireshark's build. Current upstream CMake remains authoritative.

## Scope dependency-driven language requirements to the targets that need them

A dependency that needs a newer C or C++ standard does not by itself justify raising the language mode of unrelated Wireshark targets. Apply the required language standard at the narrowest target scope that actually consumes the dependency, while keeping the project-wide minimum aligned with the baseline Wireshark intentionally supports.

Merged master MR !23333, authored and merged by Gerald Combs, changed the CMake setup so C++17 is required on the individual targets that use libsinsp instead of setting C++17 globally. This preserves a lower general language baseline while satisfying the stricter dependency contract where it matters.

**Implementation rule:** distinguish Wireshark's global minimum language contract from a component or dependency's stronger requirement. Express a stronger requirement on the consuming target or targets, preferably with normal target properties/features, unless the project has separately decided to raise the global baseline.

**Confidence:** Very high. Merged master CMake policy change authored and merged by Gerald Combs.
