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
