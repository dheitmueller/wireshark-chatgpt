# Wireshark Static Analysis Conventions

This file records durable conventions for reconciling static-analysis findings with compiler diagnostics and source-level invariants. Current upstream build configuration and source remain authoritative.

## Fix analyzer warnings without destroying stronger compiler diagnostics

A local change that silences one analyzer can make another useful check impossible. In particular, an exhaustive `switch` over an enum should generally remain structurally exhaustive so compilers can diagnose newly added enum values that have not been handled.

Merged MR !13002, authored by João Valverde, reverts an attempted Clang Analyzer cleanup that introduced a `default` path. The accepted fix instead gives the result a safe initialization while preserving the switch without `default`, so compiler exhaustiveness diagnostics remain available when the enum grows. Merged master/release analyzer-cleanup work in !12991 and !12982 provides the surrounding context: dead initializations are worth removing when control flow actually guarantees assignment, but that cleanup must preserve other diagnostic invariants.

**Implementation rule:** treat warnings from different tools as interacting constraints, not independent boxes to check. Prefer a refactoring or safe initialization that satisfies the analyzer while retaining compiler checks for missing enum cases; do not add a catch-all `default` solely to make an analyzer prove initialization.

**Review rule:** when a warning fix changes control-flow shape, ask what diagnostics the previous shape enabled. A `default`, assertion, cast, or initialization may suppress useful future warnings even if it makes the current report disappear.

**Confidence:** High. The accepted merged correction explicitly restores the compiler's ability to diagnose unhandled enum values, reinforced by merged Clang Analyzer cleanup work in adjacent MRs.
