# Wireshark Fuzz Harness Conventions

This file records durable implementation and testing conventions for Wireshark fuzz targets. Current upstream fuzz infrastructure remains authoritative.

## Initialize shared harness state for every target configuration

Initialization needed by the fuzz entry path must not be hidden behind a compile-time branch that only applies to the monolithic or aggregate fuzz binary. Individual protocol/entry-point fuzz targets need the same deterministic baseline for shared application state they consume.

Merged MR !24675, authored and merged by John Thacker, moves zeroing of `app_data` outside an `#ifdef` after discovering that individual targets such as `fuzzshark_ip_proto-udp` did not execute the aggregate-only initialization path. The resulting fix makes initialization unconditional for the target variants that share the state.

**Testing rule:** when fuzz infrastructure supports both aggregate and specialized targets, audit initialization and teardown across every compile-time target shape. Put shared-state initialization at the common boundary unless the state is genuinely target-specific; do not assume exercising the aggregate fuzz binary proves the specialized harness starts from an equivalent state.

**Confidence:** Very high. Merged fuzz-harness correction authored and merged by John Thacker with a concrete individual-target failure mode.

## Reproduce fuzz findings through the same entry point before changing the reproducer shape

A fuzz input is interpreted in the context of its target. Wrapping the same bytes in a generic Ethernet/IP/UDP capture, changing the top-level dissector, or otherwise moving the bytes to a different dispatch path can change which dissector runs and which offsets/state it sees. Failure to reproduce after changing the harness shape does not show that the original fuzz finding is invalid.

Merged master MR !13950 adds a CBOR recursion-depth guard. Evan Huus initially could not reproduce the report by embedding the supplied bytes in an ordinary capture, while John Thacker reproduced it directly with the corresponding fuzzshark target and POC. Evan then noted that a fuzz target can hard-call a dissector in a way that is materially different from wrapping the same bytes in Ethernet/IP/UDP.

**Testing rule:** first reproduce fuzz findings with the exact target/entry point and raw input that produced them. Only after that baseline is established should the input be converted into a conventional capture or exercised through another dispatch path, and differences between those paths should be treated as evidence about reachability rather than as contradictory results.

**Review rule:** record the fuzz target together with the minimized input. If a proposed regression test uses a different outer framing, verify that it still reaches the same parser state; otherwise retain a target-specific test rather than silently changing what is being tested.

**Confidence:** Very high. Merged master hardening fix with direct John Thacker reproduction and explicit maintainer discussion of why the generic capture path did not reproduce the fuzz-target behavior.

## Match the sanitizer and allocator environment when reproducing memory fuzz failures

A reproducer that does not fail in a normal development build can still be valid if the original fuzz environment changes allocation layout, bounds checking, or use-after-free detection. Reproduction should match the relevant instrumentation before concluding that the minimized input is no longer effective.

Merged master MR !13870 fixes an E2AP NULL-table-pointer crash found through fuzzing. Martin Mathieson noted difficulty reproducing the supplied failure directly. Gerald Combs explained that he generally needs an ASAN build together with `WIRESHARK_DEBUG_WMEM_OVERRIDE=strict` and `G_SLICE=debug-blocks` when running tshark to reproduce the fuzz environment closely enough; John Thacker confirmed reproduction with ASAN and that the fix addressed it.

**Testing rule:** for memory-safety fuzz findings, preserve not just the input and target but also the sanitizer/allocator/debug settings needed to expose the failure. Attempt reproduction under equivalent instrumentation before modifying the input, weakening the test, or treating a normal-build non-reproduction as evidence that the bug is gone.

**Review rule:** distinguish reachability from detectability. The same bad state can be reached in an ordinary build without visibly crashing because allocator layout or runtime checks differ; use the fuzzing instrumentation as part of the regression context when that instrumentation is material to observing the defect.

**Confidence:** Very high. Merged master fuzz fix with explicit reproduction guidance from Gerald Combs and independent ASAN confirmation from John Thacker.
