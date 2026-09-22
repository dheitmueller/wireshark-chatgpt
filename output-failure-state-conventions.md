# Wireshark Output Failure-State Conventions

This file records durable conventions for functions that communicate failure through caller-provided output buffers or result objects. Current upstream source remains authoritative.

## Establish the documented failure sentinel before any early return can occur

If callers distinguish success from failure by inspecting an output value, initialize that output to its failure sentinel before performing validation or computation that can return early. Initializing the output only near the successful end of the function leaves earlier failure exits exposing stale or uninitialized caller-visible data.

Merged master MR !13924, authored and merged by John Thacker, fixes NTLMSSP v1 session-key generation. A zeroed session key is the failure state checked by callers, but the function could return early while validating the challenge response before reaching the old `memset()`. The accepted change moves zero-initialization to the start of the function, matching the existing v2 behavior. Merged release-4.2 backport !13925 carries the same correction.

**Implementation rule:** when an output parameter has a defined failure representation, write that representation at function entry (or otherwise before the first failing exit) unless the API explicitly guarantees that outputs are untouched on failure. Do not rely on every error branch remembering to initialize the result independently.

**Review rule:** when adding an early return to a function with output parameters, audit what the caller observes on that path. Pay special attention to cryptographic/session keys, lengths, handles, and state flags where stale data can be mistaken for a valid result.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker with an accepted stable backport and an explicit caller-visible failure-state contract.