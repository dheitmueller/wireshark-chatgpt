# Wireshark Reentrancy and Callback Conventions

This file records durable event-loop and callback-lifecycle conventions extracted from accepted upstream Wireshark changes. Current upstream code remains authoritative.

## Prevent invalid re-entry at the event source instead of teaching every lower layer to survive it

When a subsystem is deliberately paused and nested event processing can otherwise trigger the same subsystem again, prefer suppressing the initiating work at the highest reliable event boundary. Adding re-entrancy exceptions to every dissector, callback, file-handler, or helper below that boundary increases the number of states the subsystem must support and makes lifetime reasoning harder.

Merged MR !24864 changes the Qt Lua debugger so packet-list dissection does not run while the debugger is paused. That allowed earlier special re-entrant handling in Lua dissector and file-handler paths to be removed rather than extended. Merged follow-up !24885 applies the same lifecycle rule to asynchronous Lua tap drawing: `lua_tap_draw()` is skipped while the debugger is paused instead of allowing an asynchronous callback to enter Lua in the middle of debugger execution.

**Implementation rule:** identify the event source that can trigger re-entry and gate it on the subsystem's lifecycle state. Prefer one well-defined paused/quiescent boundary over scattered lower-layer `already running` guards, unless re-entrancy is an intentional supported contract.

**Confidence:** Very high. Two merged master changes by Stig Bjørlykke that deliberately replace lower-level re-entrant handling with lifecycle gating at packet-dissection and asynchronous tap-callback boundaries.

## Put mutable working state in the narrowest lifetime and ownership scope that actually needs it

Mutable `static` storage is a hidden cross-call and cross-instance sharing contract. If the data is only scratch state for one invocation, put it on that invocation's stack. If it must survive across calls for one open reader/parser instance, attach it to that instance rather than sharing it process-wide.

Merged master MR !21530 moves an EAX working structure from function-static storage to the stack. The MR explicitly states that none of the values need to persist across calls and that removing the static storage eliminates a possible concurrency conflict. Merged master MR !21510 handles a different lifetime correctly: an Ericsson eNode-B Wiretap parser's 128-KiB line buffer must persist while that parser instance is open, so it is dynamically allocated and stored in `wtap->priv` instead of being one global static array. Besides reducing `.bss`, the change explicitly avoids conflicts if multiple parser instances are used concurrently.

**Implementation rule:** choose storage from the state lifetime: stack/local storage for per-call scratch, packet/file/conversation/instance-private storage for state with that corresponding lifetime, and mutable static/global storage only when process-wide sharing is intentional and its concurrency semantics are explicit. Do not use `static` merely to avoid allocation or a large stack object when it accidentally couples otherwise independent calls or parser instances.

**Confidence:** Very high. Two independent merged master changes explicitly motivated by eliminating unintended shared mutable state and concurrency conflicts.