# Wireshark Toolchain Baseline-Transition Conventions

Merged master MRs !5443, !5452, and !5458 show that changing Wireshark's C baseline is a complete toolchain-contract change, not just a compiler-flag change. The accepted C11 transition uses CMake's language-standard mechanism, documents excluded optional features such as VLAs and Annex K, and rejects known-incompatible MSVC/Windows SDK combinations early. Earlier merged !5425 exposed separate CMake, SDK/preprocessor, macOS deployment-target, and C/C++ mode constraints.

**Rule:** validate compiler, build-system, SDK/header, runtime/deployment-target, and mixed-language-mode compatibility together when raising a baseline. Turn known-incompatible floors into configuration errors; keep only narrow workarounds for versions known to provide the required semantics.

**Confidence:** Extremely high; merged master changes with direct review and diagnosis by João Valverde, Gerald Combs, and Guy Harris.
