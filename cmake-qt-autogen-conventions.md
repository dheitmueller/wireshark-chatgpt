# CMake Qt AUTOGEN conventions

This file records durable build-system conventions for Qt AUTOGEN behavior in Wireshark.

## Prefer target-level AUTOGEN properties and validate workaround removal across generators

Qt autogeneration requirements belong on the concrete target when that is where the requirement exists. Enabling global `CMAKE_AUTOMOC`, `CMAKE_AUTOUIC`, or `CMAKE_AUTORCC` can cause unrelated targets to be scanned and can hide the distinction between single- and multi-configuration generator behavior.

Merged master MR !3004, authored by Gerald Combs, sets `AUTOMOC`, `AUTOUIC`, and `AUTORCC` directly on both `qtui` and `wireshark`. The rationale is specifically that multi-configuration generators such as MSBuild need the Wireshark target itself to carry these properties.

Merged !3008 then removes a temporary CMake-3.20 global AUTOGEN workaround after Gerald tested the target-level configuration with CMake 3.10.3, 3.20.1, and 3.20.2. Closed !3009 is useful diagnostic history rather than implementation precedent: a broader removal looked fine in one clean local configuration, but Gerald's cross-platform testing showed that the target properties were still required for Windows/multi-config builds.

**Build rule:** express Qt AUTOGEN requirements at target scope whenever possible. Treat generator family and CMake version as part of the supported behavior contract rather than assuming one successful build configuration proves the setting is redundant.

**Validation rule:** retire a build workaround only after testing the tool versions/generators it originally protected and representative unaffected configurations. A clean Ninja build on one platform is not evidence that an MSBuild or other multi-config requirement has disappeared.

**Confidence:** Very high. Two merged master build-system changes authored by Gerald Combs; the closed follow-up is used only as corroborating diagnostic evidence.
