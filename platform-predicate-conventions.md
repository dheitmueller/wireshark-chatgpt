# Platform Predicate Conventions

Merged MR !7051, authored by Guy Harris, gates MSVC's x86 `__cpuid` intrinsic on the actual x86/x64 target architecture and supplies a non-x86 fallback. Merged !7022 fixes MSYS2 by narrowing a workaround from broad Windows gating to the MSVC toolchain.

**Rule:** operating system, compiler family, CPU architecture, package environment, and runtime capability are separate predicates. Condition platform-specific code on the narrow property that makes it valid. Windows does not imply MSVC, and MSVC does not imply x86.

**Confidence:** Extremely high for !7051; high corroboration from merged !7022.
