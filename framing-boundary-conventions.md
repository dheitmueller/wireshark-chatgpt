# Wireshark Framing-Boundary Conventions

This file records durable conventions for dissectors that must distinguish protocol payload, padding, trailers, and frame checksums across nested link-layer encapsulations. Current upstream source remains authoritative.

## Classify padding, trailers, and FCS from semantic frame boundaries, not raw enclosing length

An enclosing tvbuff or captured packet length does not necessarily identify where the original frame payload ended. Tags can be inserted after padding was added; trailers can precede or follow padding depending on the path through the network; an FCS may be present, absent, truncated, or only heuristically suspected. Framing logic therefore needs the semantic payload origin/length supplied by the dissector that actually knows it, together with captured-vs-reported length state.

Merged master MR !13089, authored by John Thacker, reworks Ethernet trailer handling around this principle. `add_ethernet_trailer()` is given the payload offset so strict zero-padding calculations can be based on the Ethernet frame geometry rather than blindly on `pinfo->fd->pkt_len`. It also passes the effective payload length to heuristic trailer dissectors instead of forcing them to reconstruct context from a sliced trailer tvbuff.

The same MR handles uncertain FCS presence conservatively: when `fcs_len == -1`, it first offers the candidate trailer to heuristic trailer dissectors without stripping an assumed FCS. Only if that interpretation is not accepted does it try the alternate interpretation with an FCS removed. It also avoids computing an FCS when the full frame is not actually captured. This prevents an early framing guess from destroying bytes needed by a more specific trailer interpretation.

**Implementation rule:** carry semantic boundary information explicitly across dissector layers. Do not infer original payload length from a later encapsulated packet length when tags, padding, trailers, or truncation can change that relationship.

**Heuristic rule:** when mutually exclusive framing interpretations are plausible, avoid destructive preprocessing based on the weaker guess. Try the interpretation that preserves all candidate bytes first, and commit to removing an FCS/trailer only when configuration or evidence justifies it.

## Keep the protocol-tree span and the tvbuff's semantic actual length synchronized

When a dissector determines that its meaningful protocol data ends before the enclosing tvbuff, both the data-source boundary used by outer framing logic and the displayed protocol item's span should reflect that same endpoint.

Merged master MR !13072, authored and merged by John Thacker, fixes the short HomePNA path by calling both `proto_item_set_len(ti, offset)` and `set_actual_length(tvb, offset)`. The actual-length update lets the preceding Ethernet dissector recognize bytes beyond HomePNA as padding/trailer/FCS; updating the proto item at the same time keeps the tree highlight from visually claiming those bytes as HomePNA.

Merged master MR !13088, also authored and merged by John Thacker, applies the same semantic-boundary idea to MACsec short frames: once the short-length field establishes the real MACsec extent, `set_actual_length()` lets the previous Ethernet layer calculate the FCS over the correct whole-frame region instead of treating the checksum bytes as MACsec padding.

**Implementation rule:** when a child dissector knows a shorter authoritative extent, update every representation that downstream/upstream consumers rely on: the tvbuff actual length for framing calculations and the protocol-item length for UI ownership. A correct parser boundary with a stale tree span, or vice versa, leaves contradictory interpretations of the same bytes.

## Pass semantic sub-PDU lengths to child dissectors, not parser-cursor residuals

A parent dissector that has already parsed a delimited sub-PDU usually has a stronger statement of that child's length than any "bytes remaining" calculation made after advancing its own cursor. Metadata handed to a child or subdissector must describe the semantic object being passed, not the parent's current parsing position within the enclosing tvbuff.

Merged master MR !12741 fixes TECMP CAN payload metadata by setting `can_info.len` to the already-established CAN payload length (`length2`) instead of `tvb_captured_length_remaining(sub_tvb, offset2)`. The old expression was evaluated after the parent had advanced `offset2`, so it described what remained after the payload rather than the payload itself; downstream dissectors such as ISO 15765 consequently received the wrong CAN length. The same correction was merged to release branches in !12743 and !12744.

**Implementation rule:** once a framing field, container descriptor, or parent parser has established a child PDU's semantic length, pass that value through child-dissection metadata. Treat `*_length_remaining(tvb, offset)` as a cursor-relative parsing primitive, not as a substitute for an already-known protocol length.

**Review rule:** for metadata structures passed to subdissectors, trace each offset and length back to its coordinate system and semantic owner. A numerically plausible "remaining length" can be wrong if the cursor has already moved past the object whose metadata is being populated.

**Confidence:** Extremely high. The original TECMP fix was merged to master with Pascal Quantin's approval and was immediately backported to both supported release branches, demonstrating that the semantic-length bug affected real downstream dissection.

## Establish framing discriminators before interpreting dependent fields

Some container formats place a discriminator such as byte-order magic, a version, or another representation selector before fields whose meaning depends on that selector. Do not interpret dependent lengths or offsets until the discriminator has been validated and the representation is known.

Merged master MR !12421 fixes the pcapng file dissector when a later Section Header Block has different endianness from the preceding section. The old path used the SHB length before establishing the new section's byte order, which could make the protocol tree claim bytes outside the actual block. The accepted fix determines the SHB byte order before using the length and hardens the invalid-magic path. During review, John Thacker specifically recommended avoiding duplicate special-case parsing: if the byte-order magic is invalid, constrain the tvbuff to the 12-byte minimum block size and let the normal block parser add the magic field and expert information.

**Implementation rule:** parse and validate representation-defining fields before any dependent framing value. Never decode a length under inherited, guessed, or previous-container endianness merely because a prior block used that representation.

**Failure-path rule:** when the discriminator itself is invalid, do not trust fields whose decoding depends on it. If useful diagnostics still need the normal parser, pass a safely bounded minimum-size subset rather than inventing an interpretation or duplicating the parser's error-reporting logic.

**Testing rule:** for formats that permit representation changes between containers or sections, include a case that switches representation mid-file and a malformed discriminator case. Verify both byte ownership/highlighting and parser termination, not only decoded field values.

**Confidence:** Very high. !12421 is a merged master correctness fix; John Thacker materially shaped the accepted failure path and merged the change.

**Overall confidence:** Extremely high. The core evidence consists of merged master fixes with accepted maintainer review and release backports, including multiple independent examples where semantic boundaries—not enclosing-buffer or cursor-relative lengths—are required for correct downstream behavior.