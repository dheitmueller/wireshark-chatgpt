# Compile-Time Disabled Code Conventions

Merged master MR 3293 fixes unused-variable build failures that appeared when assertion or debug macros vanished completely. The accepted macros keep the underlying expression or call behind a compile-time-false condition so variables referenced only by a disabled assertion or debug message remain syntactically used, while the compiler can remove the runtime work.

Rule: disabling a diagnostic or assertion should not unexpectedly change whether surrounding variables are considered used. Prefer an optimization-dead reference pattern that preserves compile-time checking and warning stability without evaluating the operation at runtime.

Confidence: high. Merged project-wide warning fix authored by João Valverde.
