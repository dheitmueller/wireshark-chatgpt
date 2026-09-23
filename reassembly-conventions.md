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

## Advance stateful stream decoders only with bytes committed exactly once

When Wireshark desegments a stream for a stateful decoder, bytes that are incomplete in the current packet may be presented again after reassembly. Feeding those bytes to the decoder before the dissector has actually consumed them advances external decoder state too early and can cause the same input to be processed twice when the completed segment is redisected.

Merged master MR !13632, authored and merged by John Thacker, fixes HTTP/3 QPACK encoder-stream desegmentation. The dissector now returns the number of bytes it actually decoded, requests desegmentation beginning at `offset + decoded`, and passes only that consumed prefix to `nghttp3_qpack_decoder`. The previous path passed the entire currently available stream buffer, including an incomplete instruction that would later be supplied again after reassembly.

**Implementation rule:** make consumed-byte count part of the boundary between a Wireshark stream dissector and any stateful decoder. Advance decoder state only for bytes that the dissector has committed as complete; leave the unconsumed suffix for the reassembly framework to replay after more data arrives.

**Review rule:** when adding desegmentation around a stateful library, audit whether the same bytes can appear once in an incomplete packet and again in a completed reassembled TVB. Any library call that consumes the incomplete suffix before the reassembly decision risks double advancement.

**Testing rule:** split one logical instruction/PDU across packet boundaries and verify that the stateful decoder observes each byte exactly once, including captures with several stream segments or coalesced transport packets in one frame.

**Confidence:** Very high. Merged master reassembly correctness change authored and merged by John Thacker, with the duplicate-input failure mode and consumed-byte solution stated explicitly.

## Treat post-completion retransmissions as reassembly metadata, not automatically as malformed input

Receiving another fragment after a message has already been reassembled does not necessarily mean the packet is malformed. On transports where retransmission, multipath duplication, or repeated lower-layer delivery is possible, a fragment that falls within the completed result should normally be represented using the reassembly engine's overlap/retransmission state so users can relate it to the original reassembly. Throwing a `ReassemblyError` turns an ordinary transport event into a misleading malformed-packet diagnosis.

Merged master MR !12077, authored and merged by John Thacker, changes the byte-offset `fragment_add()` path to match the sequence-number reassembly behavior. A fragment wholly within an already-completed reassembly now reaches the normal overlap bookkeeping and sets `FD_OVERLAP` rather than raising an exception. The MR was validated with a multipath retransmission capture where the previous behavior produced alarming `ReassemblyError` output. The same change also corrects the boundary test from `>=` to `>`: a fragment ending exactly at the completed length overlaps the existing result but does not extend beyond it.

The MR's public API documentation makes the key-lifetime distinction explicit. The non-`_check` reassembly APIs retain completed reassemblies under their key and therefore interpret reuse of that key as the same message/retransmission; if a protocol can legitimately reuse an identifier for a different message, the dissector must include sufficient additional `data` or custom key functions to distinguish generations. Conversely, the `_check` variants remove completed entries from the in-progress table and permit a key to start a new message, but callers outside reliable transports must then avoid feeding retransmissions of an already-completed message as if they were new fragments.

**Implementation rule:** classify a duplicate fragment according to the reassembly API's documented completed-key semantics. For a retransmission of the same completed message, record overlap/provenance rather than converting it into a malformed exception. If the protocol legitimately reuses identifiers, make the reassembly key generation-aware enough to distinguish separate messages or choose the API whose completed-entry lifecycle matches that protocol.

**Review rule:** examine both exact-end and beyond-end duplicates, key reuse for a genuinely new message, and transports where retransmissions are not filtered below the dissector. A reassembly key that is only unique “most of the time” is not sufficient when completed state remains addressable by that key.

**Confidence:** Very high. Merged master core-reassembly behavior and API documentation authored and merged by John Thacker, with a concrete retransmission capture used for validation.

## Only claim reassembly when analysis state proves it, and expose provenance when available

User-facing packet text should describe what the reassembly engine actually established, not what the current code path merely expected might happen. A generic “segment of a reassembled PDU” message is misleading when no completed reassembly exists; when Wireshark knows the frame that completed the PDU, reporting that frame makes the analysis claim concrete and auditable.

Merged master MR !13397 changes TCP's segment annotation so it is added only when the reassembly state has a nonzero `reassembled_in` frame, and changes the text to identify that frame explicitly. The previous unconditional-style wording could label a segment as belonging to a reassembled PDU even when the corresponding reassembly frame was absent.

**Presentation rule:** derive reassembly annotations from positive framework state. Do not advertise a packet as reassembled merely because it traversed a reassembly-capable path or because a dissector expected later data.

**Provenance rule:** when the framework records where the completed PDU was reconstructed, expose that frame identifier in the tree/summary rather than emitting an unqualified generic claim. This improves both troubleshooting and confidence in cross-frame analysis.

**Testing rule:** cover both a capture that really completes a multi-frame PDU and a case where completion never occurs, such as a truncated or missing-tail capture. The completed case should name the reassembly frame; the incomplete case should not claim that reassembly occurred.

**Confidence:** High. Merged master TCP presentation fix whose accepted condition is the concrete `reassembled_in` state supplied by the reassembly framework.