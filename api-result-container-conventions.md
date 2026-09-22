# Wireshark Result-Container API Conventions

This file records durable conventions for APIs that return caller-owned result containers alongside separate error information. Current upstream source remains authoritative.

## Once an API creates its result container, return that container consistently on recoverable failure

If a function's contract separates the result collection from status/error details, returning `NULL` on some failure paths after already allocating an empty result container creates two representations of the same logical result and can leak the allocated object. Prefer returning the initialized empty container while reporting the failure through the function's existing error channel.

Merged master MR !13491, authored and merged by John Thacker, changes `capture_get_if_list_capabilities()` so failures from dumpcap, invalid JSON, and JSON parsing errors return the already-created capability hash table instead of `NULL`. The commit message explicitly notes both reasons: the caller receives an empty table on failure and the table is no longer leaked on those paths.

**Implementation rule:** decide independently (1) what object/result shape the function returns and (2) how failure is reported. If an empty result container is a valid, already-created object and detailed failure information is carried separately, preserve the container-return contract across all recoverable exits rather than switching to `NULL` as an extra status signal.

**Ownership rule:** audit every early return after allocation. A failure path must either return ownership to the caller under the normal API contract or destroy the object before returning a sentinel; it must not silently abandon the allocation.

**Review rule:** callers should not need to guess whether `NULL` means “failure,” “no results,” or “object was never constructed” when the API already has explicit error/status outputs. Keep result shape and error signaling orthogonal where practical.

**Confidence:** Very high. Merged master API/ownership cleanup authored and merged by John Thacker, with the empty-container behavior and leak prevention stated explicitly in the commit.