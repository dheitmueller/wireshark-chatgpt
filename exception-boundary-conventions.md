# Wireshark Exception-Boundary Conventions

This file records durable conventions for code that crosses Wireshark/libwireshark exception handling and foreign runtime error mechanisms. Current upstream implementation remains authoritative.

## Do not let one non-local-jump exception system escape through another's active frame

When two runtimes both implement exceptions with `setjmp`/`longjmp`, their exception frames are not composable merely because both mechanisms work independently. A `longjmp` from one runtime can bypass cleanup and stack restoration required by the other runtime, leaving pointers to invalid stack-resident jump frames and causing later crashes.

Merged master MR !24449, authored by John Thacker, changes WSLua `TreeItem:add` handling to validate Lua-visible value types before entering the libwireshark operation that can establish/throw through an epan exception frame. Its stated rationale is specifically that a Lua exception must not jump out of an epan TRY block. Merged master MR !24419, also authored by John, adds a defensive WSLua recovery check that detects when Lua error handling has skipped libwireshark's exception-stack unwind and resets the damaged exception-stack state. That fallback deliberately accepts possible cleanup leaks because the skipped stack frames can no longer be safely traversed; it is a crash-containment measure, not the preferred control flow.

**Implementation rule:** perform foreign-runtime validation and error generation on the safe side of the libwireshark exception boundary whenever possible. Never rely on a Lua (or other foreign runtime) `longjmp` safely crossing an active epan TRY frame. If defensive corruption detection is needed, treat forced exception-stack reset as last-resort containment rather than normal error handling.

**Confidence:** Very high. Two related merged master fixes authored by John Thacker, one preventing the invalid crossing and one containing the corrupted state left by such a crossing.
