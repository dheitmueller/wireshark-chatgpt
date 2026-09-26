# Partial Initialization and Cleanup Conventions

This note records durable Wireshark guidance for cleanup when object construction or registration fails before normal initialization has completed. Current upstream source remains authoritative.

## Do not run a full destructor on an object whose initialization contract has not completed

A general destructor is only safe when the object's invariants required by that destructor have been established. If construction or registration fails before callbacks, owned members, or subordinate resources have been installed, do not pretend the object is fully initialized merely to reuse the ordinary teardown path.

Merged MR !20282 fixes `register_tap_listener()`. One display-filter error path called `free_tap_listener()` while the tap-listener structure was still only partially set up. That destructor may invoke the tap `finish` callback, while callers of `register_tap_listener()` also perform their own cleanup after a registration error because other error paths never invoked `finish`. The result was an inconsistent ownership contract with double-free potential. At the failing stage, only the listener structure itself was known to have been allocated, so the accepted fix frees only that structure.

**Implementation rule:** define cleanup responsibility for every construction stage. Before the object reaches its normal initialized state, release only resources whose ownership has actually transferred and whose initialization is known complete. Invoke the canonical full destructor only once its preconditions are satisfied.

**API rule:** failure semantics should be consistent across all exits. A caller should not have to guess whether a failed registration secretly ran user callbacks or released caller-owned state on one internal error path but not another.

**Confidence:** Very high. Merged master lifecycle/correctness fix by Jaap Keuter, approved and merged by John Thacker, with the inconsistent callback/cleanup contract documented directly in the MR.

## Track registration success before teardown

Optional registration paths create the same partial-initialization problem at a broader lifecycle level: cleanup must be conditioned on which registrations actually succeeded.

Merged master MR !8419 changed the F5 Ethernet Trailer's expensive analysis preference to default disabled. Joakim Karlsson immediately reported cleanup warnings because the dissector still attempted to remove tap listeners that had never been registered. Merged master MR !8427 fixes the lifecycle contract by tracking each successful tap registration, removing only registered listeners, and clearing the corresponding flag after removal; stable backport !8429 preserves the fix.

**Implementation rule:** for optional or failure-prone registration, record successful ownership acquisition explicitly. Teardown must consult that state and return it to the unregistered condition after releasing the resource. Do not infer registration from configuration intent or from the existence of callback storage.

**Confidence:** Very high. The bug was exposed by a real preference-path change, corrected in merged master, and backported.

