# Wireshark Reassembly Conventions

This file records durable packet-reassembly conventions extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Track bytes actually received, and expose a reassembly only when it is complete

A protocol's advertised final PDU/SDU length is not the amount of data present in an individual fragment. Reassembly bookkeeping must copy and advance by the bytes actually available in each fragment; attempting to copy the final aggregate length from the first fragment can trigger bounds failures and corrupt the reassembly state. Likewise, reaching a packet marked as the last fragment does not by itself prove that the aggregate buffer is complete when earlier data was missing.

Merged master MR !22271, authored and merged by John Thacker, fixes Bluetooth L2CAP I-frame reassembly so the first fragment copies only its actual `length` and initializes the accumulated offset from that length, rather than copying the advertised complete SDU length. The change was validated against an existing issue capture whose fragments subsequently reassemble into an OBEX packet. Release backports !22273 and !22274 carry the same fix.

Merged master MR !22283, also authored by John Thacker, adds the complementary completion invariant: the dissector creates and publishes a reassembled child TVB only when the accumulated offset exactly equals the expected total length (`cur_off == tot_len`). A nominal last fragment with a mismatch indicates missing data or other reassembly failure and must not be presented as a complete SDU. Release backports !22286 and !22287 reinforce that this was considered release-worthy correctness behavior.

**Reassembly rule:** distinguish expected aggregate length from bytes actually captured. Copy and advance state by real fragment lengths, and do not hand a reassembled buffer to downstream dissection until the bookkeeping proves the expected aggregate has actually been filled.

**Confidence:** Very high. Two adjacent merged master fixes authored by John Thacker, both backported to release branches; one is explicitly validated against a known failing capture.

## Custom reassembly must preserve framework dependency bookkeeping

Wireshark's standard reassembly facilities provide more than byte concatenation. They also participate in cross-frame bookkeeping that other features rely on. A dissector that implements its own equivalent reassembly or unchunking cannot assume that reconstructing the payload alone reproduces all framework semantics.

Merged master MR !14014, authored and merged by John Thacker, fixes RTMPT's custom unchunking path. RTMPT does not use the native reassembly API, so it now records every frame contributing bytes to a reconstructed packet and calls `mark_frame_as_depended_upon()` for those frames when the packet completes. Without that bookkeeping, exporting selected packets could include the final reconstructed packet while omitting earlier frames required to reconstruct it.

**Implementation rule:** when a dissector bypasses Wireshark's normal reassembly machinery, audit the framework behavior the custom path is replacing, not just the reconstructed bytes. In particular, preserve contributing-frame dependency information when later operations such as packet export need the original fragments.

**Testing rule:** for custom reassembly, test a selection/export workflow in addition to successful dissection. Selecting the frame that exposes the completed higher-level PDU should retain every earlier frame needed to reconstruct that PDU.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker; the behavior is directly tied to Wireshark's packet-export semantics.

## Use the reassembly primitive that matches the protocol's native sequence-number semantics

Do not compensate for a mismatch between a protocol's fragment numbering and a generic reassembly call by walking or rewriting the reassembly framework's internal fragment chain. If Wireshark exposes a public reassembly primitive for that numbering model, use it so the framework owns normalization, ordering, and bookkeeping.

Merged master MR !14004, authored and merged by John Thacker, changes OPC UA chunk reassembly to use `fragment_add_seq_offset()`. OPC UA chunks can begin at a sequence number other than zero. The prior workaround examined the accumulated fragment chain and adjusted numbering manually; the accepted change uses the API specifically intended for fragments whose sequence numbering starts at an arbitrary value. The MR explicitly notes that this is simpler than examining the whole chain and has a better chance of remaining correct if fragments arrive out of order.

**Implementation rule:** choose the `fragment_add_*` variant whose contract matches the protocol's actual offset/sequence model. For an arbitrary starting sequence, establish the sequence offset through the supported API and then pass the protocol's native sequence values rather than inventing a second numbering scheme in the dissector.

**Review rule:** direct traversal or mutation of reassembly internals is a warning sign when the goal is merely to adapt sequence-number semantics. First check whether the framework already has an offset-aware, sequence-aware, or otherwise specialized public primitive.

**Testing rule:** exercise a non-zero initial sequence number and, where the transport/capture permits it, reordering or missing-fragment cases. A workaround that succeeds only for a perfectly ordered chain is weaker than a framework operation that preserves the reassembly engine's normal ordering behavior.

**Confidence:** Very high. Merged master reassembly cleanup authored and merged by John Thacker, with the API-selection rationale stated directly in the MR.