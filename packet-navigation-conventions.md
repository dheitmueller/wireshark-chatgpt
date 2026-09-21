# Packet navigation conventions

This file records durable conventions for packet-list navigation and selection behavior in the Qt UI. Current upstream source remains authoritative.

## Let the navigation layer that proves success own selection and scrolling

Do not repeat selection and scrolling in an outer caller when the lower-level navigation operation already performs those side effects as part of its success contract. Duplicating the work creates extra model lookups and UI updates and makes it easier for the two paths to drift semantically.

Merged master MR !14812, authored by John Thacker and approved/merged by Anders Broman, removes a second row lookup, selection, and `scrollTo()` from `PacketList::goToPacket()`. `cf_goto_frame()` only returns success when `packet_list_select_row_from_data()` succeeds, and that lower layer already selects the frame and scrolls it into view. The outer caller retains only its genuinely additional responsibility: moving the protocol tree to a requested field when appropriate.

**Architecture rule:** define one owner for each navigation side effect. If a successful helper already establishes the current packet and visible selection, callers should build only their extra semantic action on top of that result rather than replaying the selection workflow.

**Confidence:** Very high. Merged master cleanup authored by John Thacker with the duplicated work and success contract described explicitly.

## Reuse an authoritative inverse mapping instead of linearly rediscovering it

When the packet-list model already maintains a direct packet-number-to-visible-row mapping, navigation code should use that mapping rather than scanning all visible rows to rediscover the same relationship. Large captures make apparently small O(N) helpers expensive when they sit on interactive paths.

Merged master MR !14813, authored by John Thacker and approved/merged by Gerald Combs, changes `PacketListModel::visibleIndexOf()` from a linear walk over `visible_rows_` to `packetNumberToRow(fdata->num)`, while keeping a null-input guard. The MR explicitly notes that the direct mapping already exists whenever visible rows are maintained.

**Performance rule:** before adding a search over packet-list state, check whether the model already maintains the inverse/index mapping required by the operation. Reuse the authoritative mapping so lookup cost and semantics stay coupled to the model's own bookkeeping.

**Confidence:** Very high. Merged master performance fix authored by John Thacker and accepted by Gerald Combs.

## Make exact-target versus nearest-visible fallback an explicit caller policy

Navigating to a frame that is currently hidden by a display filter has more than one reasonable semantic. A user command that asks to go near a hidden frame may benefit from falling back to the closest displayed packet; a protocol-tree `FT_FRAMENUM` reference normally means one exact frame and should not silently land somewhere else. The shared navigation API should expose that distinction instead of imposing one global fallback rule.

Merged master MR !14820, authored by John Thacker and approved/merged by Anders Broman, adds an explicit policy parameter to `cf_goto_frame()`. General navigation can request the nearest displayed frame when the target is hidden, while following an exact frame-number field deliberately keeps the previous fail-if-hidden behavior. The implementation uses the existing displayed-frame bookkeeping and compares the nearest candidates rather than changing exact-reference semantics.

**API rule:** when fallback changes the meaning of a navigation request, make fallback a deliberate call-site policy. Exact semantic references should remain exact; convenience navigation may opt into a nearby visible target. Avoid a hidden global behavior change that makes all callers approximate.

**Confidence:** Very high. Merged master behavior change authored by John Thacker with the caller distinction and compatibility rationale documented in the MR.
