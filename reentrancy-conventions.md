# Wireshark Reentrancy and Callback Conventions

This file records durable event-loop and callback-lifecycle conventions extracted from accepted upstream Wireshark changes. Current upstream code remains authoritative.

## Prevent invalid re-entry at the event source instead of teaching every lower layer to survive it

When a subsystem is deliberately paused and nested event processing can otherwise trigger the same subsystem again, prefer suppressing the initiating work at the highest reliable event boundary. Adding re-entrancy exceptions to every dissector, callback, file-handler, or helper below that boundary increases the number of states the subsystem must support and makes lifetime reasoning harder.

Merged MR !24864 changes the Qt Lua debugger so packet-list dissection does not run while the debugger is paused. That allowed earlier special re-entrant handling in Lua dissector and file-handler paths to be removed rather than extended. Merged follow-up !24885 applies the same lifecycle rule to asynchronous Lua tap drawing: `lua_tap_draw()` is skipped while the debugger is paused instead of allowing an asynchronous callback to enter Lua in the middle of debugger execution.

**Implementation rule:** identify the event source that can trigger re-entry and gate it on the subsystem's lifecycle state. Prefer one well-defined paused/quiescent boundary over scattered lower-layer `already running` guards, unless re-entrancy is an intentional supported contract.

**Confidence:** Very high. Two merged master changes by Stig Bjørlykke that deliberately replace lower-level re-entrant handling with lifecycle gating at packet-dissection and asynchronous tap-callback boundaries.
