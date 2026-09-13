# Wireshark Sanitizer Configuration Conventions

This file records durable conventions for configuring sanitizer builds so the instrumentation can actually observe the defect classes it is intended to detect. Current upstream source and build configuration remain authoritative.

## Do not define away behavior that a sanitizer is meant to diagnose

A sanitizer configuration is only valid if the compiler flags used in that configuration preserve the language semantics the sanitizer is intended to observe. A hardening or optimization-control flag that is useful in ordinary builds can make a sanitizer silently ineffective if it changes undefined behavior into defined behavior.

Merged master MR !21340, authored and merged by John Thacker, removes `-fno-strict-overflow` from UBSan builds. GCC and Clang make that option imply `-fwrapv`, defining signed integer overflow as two's-complement wrapping rather than undefined behavior. That in turn prevents the normal Undefined Behavior Sanitizer configuration from diagnosing signed overflow. Wireshark therefore retains the flag for ordinary builds but omits it when `ENABLE_UBSAN` is active.

**Implementation rule:** audit sanitizer builds as complete semantic configurations, not merely normal builds plus instrumentation. Do not combine a sanitizer with compiler options that redefine, suppress, or otherwise make unobservable the behavior being tested. When an ordinary-build hardening flag conflicts with sanitizer observability, scope the flag conditionally rather than weakening the sanitizer.

**Review rule:** when adding or changing compiler flags, consider their interaction with ASan, UBSan, TSan, and other instrumented configurations. A sanitizer build that compiles and runs successfully is not sufficient evidence that the targeted defect class remains detectable.

**Confidence:** Extremely high. Merged master build-system change authored and merged by John Thacker, with the compiler-semantic reason stated explicitly in the MR and implementation comments.
