# Partial Initialization and Cleanup Conventions

This note records durable Wireshark guidance for cleanup when object construction or registration fails before normal initialization has completed. Current upstream source remains authoritative.

## Do not run a full destructor on an object whose initialization contract has not completed

A general destructor is only safe when the object's invariants required by that destructor have been established. If construction or registration fails before callbacks, owned members, or subordinate resources have been installed, do not pretend the object is fully initialized merely to reuse the ordinary teardown path.

Merged MR !20282 fixes `register_tap_listener()`. One display-filter error path called `free_tap_listener()` while the tap-listener structure was still only partially set up. That destructor may invoke the tap `finish` callback, while callers of `register_tap_listener()` also perform their own cleanup after a registration error because other error paths never invoked `finish`. The result was an inconsistent ownership contract with double-free potential. At the failing stage, only the listener structure itself was known to have been allocated, so the accepted fix frees only that structure.

**Implementation rule:** define cleanup responsibility for every construction stage. Before the object reaches its normal initialized state, release only resources whose ownership has actually transferred and whose initialization is known complete. Invoke the canonical full destructor only once its preconditions are satisfied.

**API rule:** failure semantics should be consistent across all exits. A caller should not have to guess whether a failed registration secretly ran user callbacks or released caller-owned state on one internal error path but not another.

**Confidence:** Very high. Merged master lifecycle/correctness fix by Jaap Keuter, approved and merged by John Thacker, with the inconsistent callback/cleanup contract documented directly in the MR.
