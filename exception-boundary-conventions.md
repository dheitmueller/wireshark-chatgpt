# Wireshark Exception-Boundary Conventions

This file records durable conventions for code that crosses Wireshark/libwireshark exception handling and foreign runtime error mechanisms. Current upstream implementation remains authoritative.

## Do not let one non-local-jump exception system escape through another's active frame

When two runtimes both implement exceptions with `setjmp`/`longjmp`, their exception frames are not composable merely because both mechanisms work independently. A `longjmp` from one runtime can bypass cleanup and stack restoration required by the other runtime, leaving pointers to invalid stack-resident jump frames and causing later crashes.

Merged master MR !24449, authored by John Thacker, changes WSLua `TreeItem:add` handling to validate Lua-visible value types before entering the libwireshark operation that can establish/throw through an epan exception frame. Its stated rationale is specifically that a Lua exception must not jump out of an epan TRY block. Merged master MR !24419, also authored by John, adds a defensive WSLua recovery check that detects when Lua error handling has skipped libwireshark's exception-stack unwind and resets the damaged exception-stack state. That fallback deliberately accepts possible cleanup leaks because the skipped stack frames can no longer be safely traversed; it is a crash-containment measure, not the preferred control flow.

**Implementation rule:** perform foreign-runtime validation and error generation on the safe side of the libwireshark exception boundary whenever possible. Never rely on a Lua (or other foreign runtime) `longjmp` safely crossing an active epan TRY frame. If defensive corruption detection is needed, treat forced exception-stack reset as last-resort containment rather than normal error handling.

**Confidence:** Very high. Two related merged master fixes authored by John Thacker, one preventing the invalid crossing and one containing the corrupted state left by such a crossing.

## Isolate transient foreign-runtime exception state across nested dissector calls

Avoid sharing a foreign runtime's transient exception-stack or jump-buffer chain across nested dissector invocations when Wireshark exceptions can unwind one invocation independently of another. Shared globals do not require shared execution-stack state.

Merged master MR !21759, authored by John Thacker and merged by Michael Mann, documents a WSLua failure mode in which nested Lua dissector calls shared one `lua_State`. Lua's protected calls maintain an `errorJmp` chain backed by stack-resident `jmp_buf` objects, while Wireshark's exception mechanism can `longjmp` out of an inner Lua protected call before Lua removes that entry normally. A later Lua error could then jump to the stale buffer from an already-exited frame, which is undefined behavior and can crash. The accepted fix uses `lua_newthread()` per call: globals remain shared, but each invocation gets an independent execution stack and `errorJmp` chain. Cleanup is arranged so the temporary thread is closed/collectable even when a Wireshark exception occurs. John tested the fix against several existing Lua crashers, large captures, and Lua-plugin reloads; Stig Bjørlykke independently confirmed the fix worked.

**Implementation rule:** when nested calls cross two non-local-jump exception systems, give each invocation its own transient foreign-runtime execution/exception context unless the runtime explicitly guarantees safe nesting across external unwinds. Preserve shared long-lived state separately, and make destruction of per-call contexts exception-safe.

**Testing rule:** exercise nested calls and historical crash reproducers, not only a single successful invocation. Also test repeated/reloaded execution, because stale jump targets and cleanup errors are often exposed only after an earlier unwind has damaged transient state.

**Confidence:** Very high. Merged master correctness fix authored by John Thacker, independently exercised by Stig Bjørlykke, with the stale-jump-buffer mechanism explicitly documented in the MR.

## Put mandatory parent-state finalization on every exceptional and early-exit path

A parent dissector can establish tree metadata or other state whose final value depends on work performed while dispatching extension headers or a child dissector. If that parent invariant must hold even when the child throws, capture truncation raises a bounds exception, or reassembly deliberately stops before normal child dispatch completes, it must not be finalized only on the normal continuation path.

Merged master MR !12321, authored by John Thacker and approved/merged by Anders Broman, fixes IPv6 extension-header tree length accounting. The IPv6 item could retain a stale length when `ipv6_dissect_next()` did not return normally—for example after an exception or when a fragment path terminated before reassembly completed. The accepted implementation wraps child dispatch in `TRY`/`FINALLY` and performs the pending parent-item length update from the `FINALLY` block, so the parent tree invariant is restored regardless of how the child path exits.

**Implementation rule:** identify parent-dissection postconditions that remain mandatory after child failure or early termination and place them in guaranteed finalization (`FINALLY`, cleanup handlers, or an equivalent ownership mechanism). Do not make correctness of parent metadata depend on a subdissector reaching its ordinary return statement.

**Review rule:** when code updates a parent item only after a nested dissector call, ask what happens if TVB access throws, capture data is truncated, reassembly defers completion, or the nested path exits early. Distinguish mandatory state restoration from optional work that should occur only after successful child dissection.

**Confidence:** Very high. Merged master correctness fix authored by John Thacker, with the exceptional/truncated/unreassembled cases explicitly described in the MR and accepted by Anders Broman.
## Locals that survive a non-local jump must satisfy the C storage rules

Wireshark's `TRY`/`CATCH` machinery is a non-local-jump boundary. Automatic local variables whose values are modified after the jump point and then observed after an exception cannot be treated like ordinary locals; the C `setjmp`/`longjmp` rules can leave non-`volatile` values indeterminate, and some supported compilers diagnose or reject code that assumes otherwise.

Merged master MR !8622 moves the varint test's exception/result locals outside the `TRY` scope and marks the values that must survive the exception path `volatile`. The same MR was reviewed by João Valverde, who approved the portability fix. It also uses GLib's width-explicit integer-constant macro in the varint overflow check rather than depending on an implementation-sensitive literal expression.

**Implementation rule:** when a value is assigned on one side of a Wireshark non-local-jump boundary and consumed after `CATCH`, audit it under the C `setjmp`/`longjmp` rules. Use `volatile` only for locals whose post-jump value must be preserved, and keep unrelated values under normal type/optimization semantics.

**Review rule:** compiler-specific warnings around `TRY`/`CATCH` are not merely cosmetic. Trace which variables cross the exceptional control-flow edge and fix the storage contract rather than suppressing the diagnostic.

**Confidence:** High. Merged master compiler-portability correction with maintainer approval; the rule follows directly from the exception mechanism and the accepted storage changes.
