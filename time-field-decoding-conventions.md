# Time-Field Decoding Conventions

## Prefer generic time encodings when they match the wire layout

When a registered time field's on-wire representation already matches a supported `ENC_TIME_*` format, let the normal protocol-tree item API perform the decode instead of manually extracting integers into an `nstime_t`.

Merged master MR !566 changes QNX Qnet6 second counters to `proto_tree_add_item(..., ENC_TIME_SECS | encoding)`. Merged master MR !565 makes the same architectural cleanup for GlusterFS seconds-plus-nanoseconds fields using `ENC_TIME_SECS_NSECS | ENC_BIG_ENDIAN`.

**Implementation rule:** choose the registered time field plus the correct time/endian encoding and use `proto_tree_add_item()` when no protocol-specific transform is required.

**Review rule:** a manual `tvb_get_*` → `nstime_t` → `proto_tree_add_time()` sequence should trigger a search for an equivalent `ENC_TIME_*` representation first.

**Confidence:** High. Two independent merged master cleanups converge on the same API pattern.
