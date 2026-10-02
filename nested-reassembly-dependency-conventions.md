# Wireshark Nested Reassembly Dependency Conventions

This file records durable conventions for preserving frame provenance through layered reassembly. Current upstream source remains authoritative.

## Preserve transitive frame dependencies across nested reassembly layers

A frame that exposes a high-level reassembled PDU can depend on a lower-level frame that itself depends on still earlier frames. Dependency bookkeeping therefore has to form a transitive frame graph rather than live only in ephemeral state for the currently dissected PDU.

Merged release-3.6 MR !9904 carries nested-dependent-packet support into the stable branch. The accepted change moves dependency ownership from transient `packet_info` state to `frame_data` and changes `mark_frame_as_depended_upon()` to operate on the frame being annotated. This allows dependencies discovered by nested reassembly layers to survive and be followed when selected/displayed packets are saved.

**Reassembly rule:** dependency provenance belongs to the frame graph, not merely to one dissection call. When reassembly is layered, ensure the saved dependency closure includes indirect contributors as well as direct contributors to the final PDU.

**Testing rule:** include a nested-reassembly filtered-save case in which the displayed high-level packet depends indirectly on an earlier frame. Reopening the saved result should still have every frame required to reconstruct the displayed PDU.

**Confidence:** Very high. Merged stable correctness backport of framework behavior, applied across the core dependency API and multiple reassembly users.
