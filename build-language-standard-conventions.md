# Wireshark Build Language-Standard Conventions

This file records durable conventions for scoping C and C++ language requirements in Wireshark's build. Current upstream CMake remains authoritative.

## Scope dependency-driven language requirements to the targets that need them

A dependency that needs a newer C or C++ standard does not by itself justify raising the language mode of unrelated Wireshark targets. Apply the required language standard at the narrowest target scope that actually consumes the dependency, while keeping the project-wide minimum aligned with the baseline Wireshark intentionally supports.

Merged master MR !23333, authored and merged by Gerald Combs, changed the CMake setup so C++17 is required on the individual targets that use libsinsp instead of setting C++17 globally. This preserves a lower general language baseline while satisfying the stricter dependency contract where it matters.

**Implementation rule:** distinguish Wireshark's global minimum language contract from a component or dependency's stronger requirement. Express a stronger requirement on the consuming target or targets, preferably with normal target properties/features, unless the project has separately decided to raise the global baseline.

**Confidence:** Very high. Merged master CMake policy change authored and merged by Gerald Combs.

## Check both C++ standard and Qt 5/6 container compatibility

Merged master MR !9822 introduced `std::as_const` (C++17) into a codebase then requiring C++11. The merged intermediate correction !9836 used `qsizetype` loop indexes but failed on Qt 5, where `QList::at()` took `int`; Nicolas F. reported a Clang `-Wshorten-64-to-32` build failure despite green CI. John Thacker explained Qt-version and compiler differences. The final accepted MR !9844 used C++11 range-for over `qAsConst(indices)`, which avoided both problems.

**Implementation rule:** prefer C++ constructs supported by the target branch's project-wide baseline; check Qt 5 and Qt 6 APIs independently before changing index types. Use range-for when an index has no semantic purpose. Treat an intermediate merged workaround as weaker evidence than the subsequent accepted corrective patch.

**Testing rule:** CI passing on one image does not establish every supported Qt/compiler configuration; specifically reproduce reported warning-as-error failures.

**Confidence:** Very high: explicit build regressions corrected by three merged master MRs.
