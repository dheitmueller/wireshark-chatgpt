# Wireshark Fuzz Harness Conventions

This file records durable implementation and testing conventions for Wireshark fuzz targets. Current upstream fuzz infrastructure remains authoritative.

## Initialize shared harness state for every target configuration

Initialization needed by the fuzz entry path must not be hidden behind a compile-time branch that only applies to the monolithic or aggregate fuzz binary. Individual protocol/entry-point fuzz targets need the same deterministic baseline for shared application state they consume.

Merged MR !24675, authored and merged by John Thacker, moves zeroing of `app_data` outside an `#ifdef` after discovering that individual targets such as `fuzzshark_ip_proto-udp` did not execute the aggregate-only initialization path. The resulting fix makes initialization unconditional for the target variants that share the state.

**Testing rule:** when fuzz infrastructure supports both aggregate and specialized targets, audit initialization and teardown across every compile-time target shape. Put shared-state initialization at the common boundary unless the state is genuinely target-specific; do not assume exercising the aggregate fuzz binary proves the specialized harness starts from an equivalent state.

**Confidence:** Very high. Merged fuzz-harness correction authored and merged by John Thacker with a concrete individual-target failure mode.
