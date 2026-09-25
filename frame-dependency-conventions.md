# Wireshark Frame Dependency Conventions

This file records durable conventions for frame-to-frame dependency state used by display filtering, reassembly, and redissection. Current upstream source remains authoritative.

## Treat frame dependencies as derived dissection state

A frame's dependency set is derived from the dissection that produced it. It therefore cannot be treated as immutable capture metadata: preferences and reassembly choices can change which earlier packets are required to understand a later packet.

Merged release-4.0 MR !9894, authored and merged by John Thacker, explicitly clears the dependent-frame collection when frame data is reset because the dependencies may no longer be valid after redissection, including when a reassembly preference changes. It also prevents duplicate dependency entries and avoids recursively revisiting frames already marked during the same pass.

**Implementation rule:** invalidate and rebuild frame-dependency state whenever the dissection state that produced it is reset. Do not carry dependency edges across redissection merely because packet bytes are unchanged.

## Preserve transitive dependencies for nested reassembly

A displayed packet may depend on a reassembled packet that itself depends on earlier fragments. Selecting or displaying the outer result requires the complete transitive prerequisite set, not only its immediate dependencies.

Merged release-3.6 MR !9904 saves dependencies on the frame object and recursively marks nested dependent packets. The accepted code deliberately stops recursion for frames already marked and documents the existing invariant that dependencies are expected to point to earlier frames rather than future packets.

**Implementation rule:** when dependency information participates in display-filter inclusion or packet availability, walk the dependency graph transitively while guarding against repeated traversal. Keep the directional invariant explicit; do not silently introduce future-frame dependencies into code that assumes a backward-only graph.

## Use set semantics for unordered dependency collections

Frame dependencies are membership information; ordering is not semantically meaningful. A list becomes unnecessarily expensive for captures where a reassembled packet has many fragments because duplicate detection is linear.

Merged release-4.0 MR !9895, authored and merged by John Thacker, replaces the dependent-frame list with a hash table specifically to avoid excessive CPU use on heavily fragmented captures. !9904 incorporates the same change in the release-3.6 nested-dependency backport.

**Implementation rule:** represent unordered unique dependency identifiers with set semantics. Prefer a hash-based structure when membership checks and duplicate suppression are frequent or can scale with fragment count.

**Confidence:** Very high. All three changes were merged release-branch correctness fixes; !9894 and !9895 were authored and merged by John Thacker, and !9904 was merged by John after integrating the nested-dependency and scaling fixes.
