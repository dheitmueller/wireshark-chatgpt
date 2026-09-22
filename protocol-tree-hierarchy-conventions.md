# Wireshark Protocol-Tree Hierarchy Conventions

This file records durable conventions for preserving structural parent/child relationships in protocol-tree output. Current upstream source remains authoritative.

## Keep child dissection beneath the tree context supplied by its parent

A dissector invoked on a payload or encapsulated protocol should attach its visible subtree to the structural context supplied by the caller rather than escaping to the packet-tree root. The distinction is normally invisible when every layer happens to be top-level, but it becomes important for nested copies, tunnels, error payloads, and other cases where a complete protocol stack is itself already inside another subtree.

Merged master MR !15669 fixes UDP payload handling that always added the payload subtree at the root. That produced misleading hierarchy when UDP itself represented an embedded packet, such as the quoted original datagram inside an ICMP Destination Unreachable message. During review Anders Broman explicitly questioned the tunneling/error-packet implications; the accepted change preserves UDP's existing placement and ensures its payload remains at that level or deeper rather than being promoted to the global root.

**Implementation rule:** treat the `proto_tree *` handed to a dissector or payload-dispatch path as part of the structural API contract. Add descendant protocol items and delegated payload dissection beneath that context unless the protocol architecture explicitly calls for a separate top-level representation. Do not reach for the packet root merely because that produces the same appearance for ordinary non-nested captures.

**Review/testing rule:** exercise encapsulated and quoted-packet cases, not just normal top-level traffic. Protocol-tree hierarchy can affect exports and machine consumers even when display filters continue to work, so a visually subtle nesting regression is still behaviorally significant.

**Confidence:** Very high. Merged master structural correction reviewed and merged by Anders Broman, with nested ICMP payloads supplied as the concrete failure case and the intended hierarchy clarified during review.

## Do not collapse distinct mutable tree items onto the same node

Protocol-tree optimizations may omit work for items whose representation is not needed, but they must preserve node identity whenever callers can subsequently mutate or inspect an item's length, representation, parentage, or other per-item state. Returning a parent's node as a stand-in for a logically distinct child makes later operations ambiguous: a call intended for the child can silently modify the parent instead.

Merged master MR !14203, authored by John Thacker, fixes this in the protocol-tree core. When a `field_info` is visible, Wireshark permits its representation and length to be adjusted after construction. The old fake-child optimization could return the existing parent node for a child, leaving no way to distinguish a later `proto_item_set_len()`/representation change intended for the child from one intended for the parent. This also broke consumers such as protocol-hierarchy statistics that depend on the final protocol-item length.

**Implementation rule:** an optimization may suppress allocation only when the observable semantics of the omitted object are provably irrelevant. If a tree item is visible, referenced, or otherwise needs independent mutable metadata, create a distinct node even if the rendered tree could appear identical without it.

**Review/testing rule:** when changing tree-faking or hidden-item logic, test not only display output but also late length/representation adjustment, referenced descendant fields, and consumers of protocol-item ranges such as hierarchy/statistics code.

**Confidence:** Very high. Merged master protocol-tree correctness fix authored by John Thacker, with the identity ambiguity and downstream range consumer stated directly in the MR.

## Treat protocol-tree presentation changes as externally observable until their scope is verified

A reordering or formatting change in the packet-details tree may look cosmetic in the GUI, but protocol-tree structure is also exposed through command-line and export paths and can be consumed by scripts. Before describing a tree change as display-only, verify whether it changes field identity, field ordering, TShark/export output, or only the human-facing presentation.

Merged master MR !14102 reorders IEEE 802.15.4 source and destination address fields for presentation consistency. During review, Graham Bloice explicitly raises the possibility that users post-process captures and distinguishes a genuinely display-only change from one that would alter fields or TShark-visible output. The merged change was accepted with the visible behavior documented in release notes. Merged !14097 independently reinforces the documentation side of the rule by changing how truncated values are rendered and recording the user-visible ellipsis behavior in the release notes.

**Implementation rule:** classify presentation changes by their observable surface before treating them as cosmetic. Preserve field identities and machine-consumed semantics unless a compatibility change is intentional; if the change is genuinely limited to presentation, still consider whether it is visible enough to document for users.

**Review/testing rule:** for tree reordering, label changes, or rendering changes, compare GUI packet details with relevant TShark/export output and display-filter behavior. Do not infer machine-output neutrality from visual simplicity alone.

**Confidence:** High. The distinction was raised directly in review of a merged master change and is corroborated by another merged user-visible rendering change that was explicitly release-noted.

## Give semantically useful structural nodes real fields rather than text-only placeholders

A tree node that represents a real protocol element can be useful to display-filter consumers even when the element has no scalar value of its own. Leaving such a node as text-only makes its presence invisible to filter operations such as counting occurrences.

Merged master MR !13756, authored and merged by John Thacker, changes SCTP chunk subtrees from text-only nodes to a registered `FT_NONE` field (`sctp.chunk`). This preserves the human-facing subtree while enabling natural filters such as `count(sctp.chunk)`.

**Implementation rule:** when a repeated or structural protocol element has meaningful presence/identity for users, prefer a registered `FT_NONE` field as the subtree item rather than a text-only node. Keep text-only items for genuinely presentational annotations that should not become part of the display-filter API.

**Review rule:** when converting a text-only node, choose the field name as a stable semantic interface, not merely a rendering implementation detail. Consider count/existence filters and machine consumers in addition to the packet-details view.

**Confidence:** Very high. Merged master field-API improvement authored and merged by John Thacker with the display-filter use case stated explicitly.

## Let common item machinery enforce common captured-length semantics

A wrapper around protocol-tree item creation should not preemptively impose stricter tvbuff bounds behavior than the common item machinery it delegates to. In particular, structural items may legitimately span a reported range larger than the captured bytes; the tree can show the captured portion and an exception can be raised later when dissection actually attempts to fetch unavailable data.

Merged master MR !13755, authored and merged by John Thacker, removes a duplicate early `tvb_ensure_*` check from the text-only subtree path. The underlying `proto_tree_add_pi()`/`get_hfi_length()` path already clips `FT_NONE` item length to what is present in the tvbuff. The accepted behavior makes text-only subtrees consistent with other `FT_PROTOCOL`/`FT_NONE` item creation: truncated captures can retain the structural node and fail only at the first actual unavailable access.

**Implementation rule:** centralize length clipping and bounds semantics in the shared protocol-tree primitives. Do not add an earlier wrapper-level existence check merely to duplicate the underlying helper, especially when that changes truncated-capture behavior.

**Testing rule:** for tree-helper changes, test snaplen-truncated packets and distinguish creating a structural item over a partially captured range from reading bytes that are absent. Verify that exceptions occur at the semantic read that requires unavailable data, not prematurely during harmless tree construction.

**Confidence:** Very high. Merged master core-protocol-tree change authored and merged by John Thacker, explicitly aligning the helper with existing FT_PROTOCOL/FT_NONE behavior.

## A valid zero-length element must not manufacture child fields

If a protocol specification defines zero length as a valid way to say that an information element is not included, the dissector should preserve that structural fact without decoding nonexistent subfields. Showing child bit fields anyway creates affirmative-looking values unsupported by the packet.

Merged master MR !13744 fixes GTP MS Network Capability handling. TS 29.060 explicitly permits length zero to mean the capability is not included; the old path still dissected bits and produced misleading fields. Review concluded that showing the zero length in the subtree was sufficient and that invented child fields were the actual problem.

**Implementation rule:** distinguish a valid empty/absent payload from malformed truncation. When zero length has defined protocol semantics, do not call a fixed-field decoder that assumes bytes exist and do not synthesize child values from following data.

**Review/testing rule:** include valid empty encodings in dissector tests, especially for optional IEs. Confirm both that no misleading child fields are emitted and that the enclosing item remains understandable.

**Confidence:** High. Merged master protocol fix with the zero-length semantics quoted from the specification and discussed during review.