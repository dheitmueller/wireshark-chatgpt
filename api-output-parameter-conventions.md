# Wireshark Output-Parameter Conventions

This file records durable conventions for APIs that return data through caller-provided output parameters. Current upstream source remains authoritative.

## Set output parameters on every normal return path

A helper that accepts an output pointer creates a contract for callers even when the operation takes an early no-op or degenerate path. Returning normally with that output untouched leaves caller behavior dependent on stale stack or prior values.

Merged master MR !6415, authored by Gerald Combs, fixes the `proto_tree_add_item_ret_*` family and related cursor helpers. When a zero or invalid item length caused the common early-return macro to return without adding an item, the routines now initialize numeric outputs to zero and boolean outputs to false before returning.

**Implementation rule:** if a return path is part of normal API behavior rather than an exception/fatal bug, initialize every advertised output parameter to the documented neutral/result value before returning. Centralized early-return macros should provide a cleanup/output hook when several APIs share the same exit condition.

**Review rule:** when adding an output parameter or factoring a common early-return helper, audit all exits—not just the main success path—for deterministic output state.

**Confidence:** Very high. Merged master core API correctness change.

## Initialize string output buffers before any normal early return

Merged master MR !6223, authored by Gerald Combs, fixes proto_item_fill_label after static analysis found a path where a caller could later treat the destination as a C string even though no terminator had been written. The accepted implementation first handles a NULL destination explicitly, then writes label_str[0] = '\0' before checking whether field information is available and returning early.

**Implementation rule:** for an API that fills a caller-provided C-string buffer, establish the neutral valid result (an empty terminated string) before any normal branch that may return without producing content. Handle a NULL destination as a separate contract violation or supported special case rather than conflating it with an empty result.

This is the string-buffer form of the broader output-parameter rule already documented above: every normal return path must leave advertised outputs deterministic.

**Confidence:** Extremely high. Merged core API correction authored by Gerald Combs and motivated by a concrete static-analysis finding.
