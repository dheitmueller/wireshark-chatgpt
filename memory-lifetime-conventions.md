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

## Treat initial capacity as an optimization, not as the logical maximum

Growable containers should normally start with a modest reservation that matches expected near-term use and expand through their checked append path. Do not reserve a protocol or implementation maximum up front merely because the container may theoretically grow that large; allocator rounding can make the actual reservation even larger, and allocation failure may be fatal before any input is parsed.

Merged MR !26180, authored by Ronnie Sahlberg and approved/merged by John Thacker, fixes the 3GPP 32.423 nettrace reader. It had passed `INT_MAX` as the initial `GArray` reservation, which GLib rounded to a roughly 2 GiB allocation even though `read_until()` filled the streaming buffer only 1024 bytes at a time. The accepted fix reserves 64 KiB and relies on the normal grow path. The old behavior was especially harmful on 32-bit builds, constrained address spaces, systems without overcommit, and long-lived legitimate traces because the huge reservation was never released during the open lifetime.

**Implementation rule:** distinguish logical maximum, current length, and initial capacity. For a growable parser buffer, choose a defensible modest initial capacity and let the container grow as data arrives; reserve maximum-sized storage only when the entire allocation is actually required immediately and its failure behavior is acceptable.

**Confidence:** Very high. Merged master resource-usage fix approved and merged by John Thacker with the allocator behavior and constrained-platform failure modes documented in the MR.

## Encode exception-safe tvbuff ownership in parent/child relationships when possible

When temporary real-data tvbuffs are logically owned by another tvbuff, prefer an ownership API that attaches the child's lifetime to the parent rather than relying on every exception path to free the temporary object manually. Wireshark dissectors can throw while parsing malformed input, so cleanup that exists only in ordinary control flow or scattered `CATCH` handlers is fragile.

Merged MR !26287, authored and merged by John Thacker, changes the CIP dissector from `tvb_new_real_data()` to `tvb_new_child_real_data()` specifically so the temporary tvbuff is freed through the parent/child lifecycle even when dissection exits by exception. The MR explicitly contrasts this with having to maintain cleanup in multiple `CATCH` statements or callbacks. Merged !26288 independently reinforces the same exception-lifetime concern for ordinary GLib lists by ensuring temporary `GList` containers are freed even when XMPP dissection throws.

**Implementation rule:** when an API can express ownership structurally, use it. Attach temporary tvbuffs to their logical parent or use scope-managed allocations so exception unwinding performs the cleanup automatically; reserve explicit `CATCH`-path cleanup for resources whose ownership cannot be represented by the existing lifetime mechanisms.

**Confidence:** Very high. Merged master memory-safety fixes authored and merged by John Thacker, with !26287 explicitly selecting the child-tvbuff API to eliminate exception-path cleanup obligations.

## Reusable static formatting buffers must be thread-local when calls can overlap

A function that formats into a mutable static buffer and returns a pointer to that storage has hidden shared scratch state. Even when the caller does not retain the pointer long-term, concurrent calls can overwrite one another or race on the buffer.

Merged MR !25662, authored and merged by Guy Harris, changes the static buffers used by `wtap_strerror()`, `file_open_error_message()`, and `file_write_error_message()` to `static WS_THREAD_LOCAL` storage. The semantic lifetime remains “valid until the next call in this thread,” but calls from other threads no longer share the same writable object. Release-branch backports !25663 and !25664 preserve the fix.

**Implementation rule:** prefer caller-owned or scope-owned result storage when practical. When an API intentionally returns a pointer to a reusable internal formatting buffer, make the scratch storage thread-local if the API can be invoked concurrently, and document/retain the per-thread overwrite lifetime rather than relying on process-global mutable static state.

**Confidence:** Extremely high. Merged master concurrency/lifetime correction authored and merged by Guy Harris and carried to supported release branches.

## Separate successful finalization from emergency resource release after an I/O error

Normal close/finalize paths for compressed or structured output may write trailers, flush compression state, or perform other operations that assume the stream is still healthy. After a write or flush error, retrying those semantic finalization operations can be pointless or harmful; the remaining obligation is often only to release memory and the underlying descriptor.

Merged MR !25679, authored and merged by Guy Harris, adds `ws_cwstream_close_after_error()` plus compression-specific error-close helpers. They deliberately skip finishing compression and writing additional output, ignore secondary close errors, free internal buffers/state, and close the underlying file descriptor. `dumpcap` uses this path after an already-reported write error instead of the ordinary successful close path.

**Implementation rule:** distinguish “finish a valid output object” from “abandon a failed output object and release resources.” Once an earlier I/O error has invalidated the stream, use an abort/error-close path that performs only teardown unless the file format/API explicitly requires and can safely perform recovery finalization.

**Confidence:** Extremely high. Merged master error-path lifecycle design authored and merged by Guy Harris.

## Initialize every byte of a derived buffer before exposing it as packet data

A derived tvbuff or byte field may outlive the narrow code that populated only selected members of its backing allocation. If any byte of that allocation can be displayed, copied, hashed, or passed to another dissector, unwritten bytes must have a defined value rather than containing allocator residue.

Merged MR !25246 fixes the WoW World dissector by changing a partially populated GUID allocation to zero-initialized storage before wrapping it with `tvb_new_child_real_data()`. Without initialization, absent GUID bytes exposed uninitialized heap contents and made derived packet data nondeterministic.

**Implementation rule:** when constructing synthetic/derived byte sequences sparsely, either initialize the complete destination first or explicitly write every exposed byte. The logical absence of a field is not permission to leave its backing bytes indeterminate.

**Confidence:** Very high. Merged master memory-correctness fix with a direct uninitialized-data mechanism.

## Run teardown at the lifecycle callback that is guaranteed to occur after asynchronous work finishes

Cleanup tied to a retap, worker, signal, or other asynchronous lifecycle must run at the callback that semantically marks completion, not merely immediately after starting or requesting the operation. Code that assumes a synchronous return can free UI/model state while callbacks are still able to use it.

Merged MR !24913, authored and merged by John Thacker, moves Export Objects post-retap cleanup from `show()` into `endRetapPackets()`. The MR documents the concrete failure mode: closing the dialog before tapping finished could leave the ongoing callback path accessing already-freed memory and segfaulting. Merged stable-branch counterparts !24915 and !24916 carry the same fix.

**Implementation rule:** identify the framework event that guarantees the last consumer has finished and attach cleanup there. Starting an asynchronous operation and subsequently returning from the initiating function is not a lifetime boundary; teardown belongs after completion notification unless the API explicitly guarantees synchronous execution.

**Confidence:** Very high. Merged master lifecycle fix authored and merged by John Thacker and independently carried to supported release branches.

## Backing storage for a derived tvbuff must live through packet processing

A tvbuff may continue to expose its backing bytes after the dissector that created it has returned. The bytes can still be consumed by downstream dissection, tshark output, or the GUI packet-bytes view, so stack storage is not a valid backing store merely because the tvbuff is constructed and initially used inside one call frame.

In merged MR !23708, John Thacker identified a QCDIAG helper that wrapped stack-allocated synthetic data in a tvbuff. His review explicitly notes that the real data cannot be declared on the stack and should be allocated from `pinfo->pool`, because the tvbuff's data must remain valid until frame processing is complete and can be used after the creating dissector returns. The accepted change moved the backing allocation into packet scope.

**Implementation rule:** when constructing a real-data or derived tvbuff, give its backing bytes a lifetime at least as long as the tvbuff's packet-processing lifetime. Use `pinfo->pool`, an appropriate wmem scope, or an ownership-aware tvbuff API; never point a tvbuff at an automatic local array that disappears on function return.

**Confidence:** Very high. Merged master change with direct, specific review from John Thacker explaining the lifetime contract.

## Tear down state-owned helpers before resetting their owner

Objects that are stored directly in a reusable interpreter, parser, or other state owner must be destroyed at that owner's reset/reinitialization boundary. A nearby broader lifecycle, such as end-of-packet cleanup, is not interchangeable when the owner can be reset first: references retained by the owner can become invalid before the later cleanup runs.

Merged master MR !23294, authored and merged by John Thacker, fixes WSLua FuncSaver handling. Unlike neighboring outstanding allocations cleared with packet-oriented cleanup, FuncSavers are stored directly in the `lua_State`. They therefore must be freed before the Lua thread/state is reset, not deferred until the end of `packet_info`. The accepted implementation also uses `g_ptr_array_new_with_free_func()` so the container expresses element teardown directly.

The MR notes that the code had already been performing an illegal access while tests happened to pass; garbage-collection and allocation details masked the bug until later changes increased memory use in each Lua state. That is a useful warning that allocator behavior or a stable test run cannot override the semantic lifetime contract.

**Implementation rule:** identify the object that actually owns or retains each helper. If that owner can be reset, recycled, or reinitialized, release its dependent helpers before the reset and use container destroy callbacks or other ownership-aware mechanisms where practical. Do not postpone cleanup merely because another surrounding scope will eventually end.

**Testing rule:** treat apparently harmless lifetime violations as real bugs even when current allocator/GC behavior masks them. Perturbations in allocation size, collection timing, or object layout can expose the invalid access later.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker with the ownership mismatch and previously masked illegal access documented explicitly.
