# Wireshark Memory and Lifetime Conventions

This file records durable allocation, ownership, and transient-state conventions extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Check independently nullable lookup results before using related state

The existence of one state object does not prove that a separate lookup or per-packet result succeeded. When downstream code dereferences both, guard the independently nullable result at the point where it is consumed rather than relying on a correlated object being non-NULL.

Merged MR !26139, authored, approved, and merged by Guy Harris, fixes O-RAN U-plane dissection after a tree lookup can legitimately return no `result`. The old code checked `cplane_state` but then unconditionally used `result->expected_sections`; the accepted fix requires `result` as well before copying or looking up expected-section state. The crash was reproduced with the Holy Grail PCAP corpus.

**Implementation rule:** model each lookup/output according to its own nullability contract. A valid neighboring state pointer is not evidence that another lookup produced a value; validate the exact object immediately before dereference or transfer.

**Confidence:** Extremely high. Merged master correctness fix authored and merged by Guy Harris with a concrete reproducer.

## Keep speculative packet-derived allocations packet-scoped until the parse is committed

Temporary arrays built while decoding a packet should use the narrowest allocator lifetime that satisfies their use. If their contents are ultimately copied into persistent/file-scope state, allocating the temporary representation itself at file scope wastes persistent memory and can orphan partially built data when malformed input throws an exception midway through parsing.

Merged MR !26141, authored, approved, and merged by John Thacker, changes Synchrophasor configuration parsing so temporary phasor and analog arrays are allocated from `pinfo->pool`, then appended/copied into the file-scoped arrays only after the temporary values have been decoded. The MR explicitly notes that packet-scope cleanup on exceptions makes even a packet-supplied 16-bit count manageable without accumulating persistent allocations. Stable-branch MRs !26143 and !26156 preserve the same fix.

**Implementation rule:** parse speculative/intermediate structures in packet scope (or another suitably short-lived scope), validate/finish the logical unit, and only then copy the data that truly must survive into file/conversation scope. Persistent allocators are for persistent state, not temporary construction buffers.

**Confidence:** Very high. Merged master memory-hardening change authored and merged by John Thacker and independently carried to two stable branches.

## Use a growable string buffer for repeated appends

Repeatedly rebuilding an ever-growing string with `wmem_strdup_printf(pool, "%s...", old_string, ...)` copies the entire prefix and leaves every previous packet-scope allocation alive until the scope is reclaimed. On attacker-controlled or high-cardinality packet data this creates quadratic copying and extreme peak memory use.

Merged MR !26151, authored, approved, and merged by John Thacker, replaces repeated DICOM string reconstruction with `wmem_strbuf_t` plus `wmem_strbuf_append_printf()` and `wmem_strbuf_finalize()`. On the reported sample, massif peak memory dropped from about 5.3 GiB to 56.9 MiB. Release-4.6 and release-4.4 backports !26154 and !26155 preserve the same implementation.

**Implementation rule:** when text is assembled incrementally, use Wireshark's growable string-buffer API rather than allocating a new full copy for every append. This is especially important inside loops whose iteration count or field multiplicity comes from packet data.

**Confidence:** Very high. Merged master resource-exhaustion fix authored and merged by John Thacker, quantitatively validated, and accepted on two stable branches.

## Give owned parser state one cleanup routine and invoke it on both failure and close paths

When an open/probe routine builds an owned state object with nested allocations, ownership must be closed on every path after acquisition: unsuccessful open paths must unwind it, and successful opens must register the corresponding close callback. Keep the nested deallocation logic in one cleanup routine so the two paths cannot silently diverge.

Merged MR !26165, authored by Gerald Combs and approved/merged by John Thacker, fixes the Procmon wiretap reader by extending `file_info_cleanup()` to free every separately allocated string before freeing the containing array, invoking that cleanup when `procmon_open()` fails after private state has been populated, and registering `procmon_close()` as `wth->subtype_close` for successful opens. The close routine clears `wth->priv` after teardown. Merged !26171, authored and merged by John Thacker after a Coverity report, independently reinforces the same failure-path ownership rule by freeing an extcap buffer on the FIFO-open error return.

**Implementation rule:** centralize teardown for an owned state aggregate; free children before their container; route every post-acquisition failure through that teardown; register the matching close hook once ownership passes to the long-lived object; and clear stale owner pointers after final teardown where appropriate.

**Confidence:** Very high. Merged master lifecycle fix authored by Gerald Combs and approved/merged by John Thacker, independently corroborated by John Thacker's merged Coverity cleanup fix.
