# Wireshark CI Test Configuration Conventions

This file records durable conventions for selecting build configurations used by automated tests. Current upstream CI configuration remains authoritative.

## Run behavioral tests in a configuration that keeps programmer checks active

The build mode used for a test suite determines which classes of defects the suite can expose. If assertions and other debug-only runtime checks encode programmer invariants, disabling those checks in the primary test job weakens otherwise-valid tests.

Merged master MR !12230, authored and merged by João Valverde, changes merge-request CI to run the test suite in a Debug build specifically so assertions and additional runtime errors are exercised. The MR explicitly acknowledges the tradeoff: the previous Release build could expose optimization-only compiler warnings, and recommends adding a distinct Release job if that coverage becomes important again rather than sacrificing assertion coverage in the behavioral test job.

**CI rule:** run the primary behavioral/unit test suite in a configuration that enables Wireshark's intended runtime assertions and programmer checks. Do not assume passing the same tests in an assertion-disabled optimized build provides equivalent coverage.

**Coverage rule:** treat debug-runtime validation and optimization-specific compiler diagnostics as separate dimensions. If both matter, add an appropriately scoped optimized build/check job instead of forcing one configuration to stand in for both.

**Confidence:** Very high. The rule is the explicit rationale of a merged master CI change authored and merged by João Valverde.