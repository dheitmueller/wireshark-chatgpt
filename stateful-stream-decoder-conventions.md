# Wireshark Stateful Stream-Decoder Conventions

This file records durable conventions for integrating external or internal decoders that maintain stream history across calls. Current upstream dissector and reassembly APIs remain authoritative.

## Separate forward stream-state advancement from redissection presentation

A stateful stream decoder is not a pure packet decoder. Feeding it the same bytes again during Wireshark redissection can advance its internal history twice, produce different results depending on packet-access order, or eventually make the decoder reject a stream that was valid on the initial forward pass. Advance such decoder state only while Wireshark is traversing the stream in the intended forward execution model, and preserve enough semantic state for later packet revisits.

Merged master MR !13611, authored and merged by John Thacker, fixes HTTP/3 QPACK encoder-stream redissection by invoking `nghttp3_qpack_decoder` only on the first pass. The dissector stores QPACK encoder-stream state such as insertion counts and retrieves that state on subsequent passes. The MR explicitly states that passing the data back through the decoder on later dissections causes confusion and inaccurate results.

Merged master MR !13565, also authored and merged by John Thacker, supplies the preceding half of the integration problem. QPACK instructions can be split across QUIC packet boundaries, so the decoder must receive sufficient stream context to preserve its state and incomplete instructions must fail through Wireshark's tvbuff bounds machinery rather than unsafe raw-pointer arithmetic. That MR also notes the remaining redissection problem that !13611 subsequently resolves. Merged master MR !13632, recorded in `reassembly-conventions.md`, further tightens the same path by feeding only the consumed/committed prefix so an incomplete suffix is not advanced once before desegmentation and then replayed a second time after reassembly.

**Architecture rule:** treat state advancement, reassembly, and presentation as separate responsibilities. A stateful decoder should see each committed stream byte exactly once in forward order; revisits should consume cached Wireshark-facing state rather than mutating the decoder again.

**Boundary-safety rule:** keep offsets and bounds inside tvbuff-aware APIs until the decoder boundary. Validate availability before pointer arithmetic so truncation produces Wireshark's normal bounds exception/reassembly behavior instead of a native crash.

**Reassembly rule:** if the decoder retains partial-instruction state, define precisely whether it receives the complete accumulated stream, only newly committed bytes, or another documented prefix. Whatever convention is chosen, incomplete bytes that Wireshark will replay after desegmentation must not be committed to decoder state twice.

**Testing rule:** cover an instruction split across transport packets, a normal sequential first pass, random backward/forward packet revisits after that pass, and repeated redissection. Verify both that later header decoding remains correct and that decoder state does not change merely because the user revisited an earlier frame.

**Confidence:** Very high. Three merged master changes authored by John Thacker describe successive parts of the same stateful QPACK integration contract, including explicit first-pass/redissection and consumed-byte failure modes.