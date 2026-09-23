# Wireshark CI Test Configuration Conventions

This file records durable conventions for selecting build configurations used by automated tests. Current upstream CI configuration remains authoritative.

## Run behavioral tests in a configuration that keeps programmer checks active

The build mode used for a test suite determines which classes of defects the suite can expose. If assertions and other debug-only runtime checks encode programmer invariants, disabling those checks in the primary test job weakens otherwise-valid tests.

Merged master MR !12230, authored and merged by João Valverde, changes merge-request CI to run the test suite in a Debug build specifically so assertions and additional runtime errors are exercised. The MR explicitly acknowledges the tradeoff: the previous Release build could expose optimization-only compiler warnings, and recommends adding a distinct Release job if that coverage becomes important again rather than sacrificing assertion coverage in the behavioral test job.

Earlier merged master MR !12175, also authored and merged by João Valverde, establishes the build-system semantics that make this distinction meaningful. It separates assertions from the broader `WS_DEBUG` switch and follows conventional `NDEBUG` behavior: Debug and RelWithDebInfo keep assertions enabled, Release and MinSizeRel disable them, while `ENABLE_ASSERT` can explicitly override the default. The developer documentation was updated at the same time so build type, logging/debug code, and assertion behavior are not conflated.

**CI rule:** run the primary behavioral/unit test suite in a configuration that enables Wireshark's intended runtime assertions and programmer checks. Do not assume passing the same tests in an assertion-disabled optimized build provides equivalent coverage.

**Build-configuration rule:** treat assertion enablement, debug logging/debug-only code, optimization level, and debug-symbol generation as distinct build properties even when common build types choose them together. Follow the toolchain/build-system `NDEBUG` convention for default assertion behavior and use the explicit assertion override only when a nonstandard combination is intentionally required.

**Coverage rule:** treat debug-runtime validation and optimization-specific compiler diagnostics as separate dimensions. If both matter, add an appropriately scoped optimized build/check job instead of forcing one configuration to stand in for both.

**Confidence:** Very high. The assertion semantics and the CI policy are explicit rationales of two merged master changes authored and merged by João Valverde.