# Wireshark Output-Parameter Conventions

This file records durable conventions for APIs that return data through caller-provided output parameters. Current upstream source remains authoritative.

## Set output parameters on every normal return path

A helper that accepts an output pointer creates a contract for callers even when the operation takes an early no-op or degenerate path. Returning normally with that output untouched leaves caller behavior dependent on stale stack or prior values.

Merged master MR !6415, authored by Gerald Combs, fixes the `proto_tree_add_item_ret_*` family and related cursor helpers. When a zero or invalid item length caused the common early-return macro to return without adding an item, the routines now initialize numeric outputs to zero and boolean outputs to false before returning.

**Implementation rule:** if a return path is part of normal API behavior rather than an exception/fatal bug, initialize every advertised output parameter to the documented neutral/result value before returning. Centralized early-return macros should provide a cleanup/output hook when several APIs share the same exit condition.

**Review rule:** when adding an output parameter or factoring a common early-return helper, audit all exits—not just the main success path—for deterministic output state.

**Confidence:** Very high. Merged master core API correctness change.
