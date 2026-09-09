# Wireshark Dissector Context-Flow Conventions

This file records durable conventions for carrying state and metadata across dissector layers and repeated dissection. Current upstream source remains authoritative.

## Propagate transport/framing facts explicitly when child dissectors need them

A child dissector should not have to reconstruct transport-layer facts that the parent already knows and that cannot be inferred reliably from payload bytes alone. Put such information in the established per-call metadata structure passed to the child.

Merged MR !25641 extends `quic_stream_info` with the QUIC FIN state and passes it to stream subdissectors. FIN is a transport semantic known by QUIC and meaningful to protocol consumers, so exposing it in the existing subdissector context is more robust than forcing downstream dissectors to infer stream termination.

**Implementation rule:** when a parent dissector owns authoritative knowledge about stream termination, direction, reassembly state, or other framing semantics required downstream, extend/pass the appropriate dissector-data structure rather than inventing payload heuristics or hidden global state.

**Confidence:** Very high. Merged master QUIC change authored and merged by John Thacker.

## Run enclosing-unit analysis once when multiple child PDUs share the same enclosing packet

Analysis keyed to an enclosing packet/frame must not be repeated merely because that packet contains multiple child PDUs. Re-running frame-level bookkeeping per child can create false retransmission/sequence conclusions or duplicate state transitions.

Merged MR !25642 changes QUIC retransmission checks so they run only for the first QUIC PDU in a frame. The retransmission analysis is associated with the enclosing frame, while the dissection loop can expose multiple QUIC PDUs from that same frame.

**Implementation rule:** identify the semantic unit to which analysis state belongs. If state is frame-level, gate it once per frame even when the parser invokes PDU-level logic multiple times; if it is PDU-level, keep it inside the child loop.

**Confidence:** Very high. Merged master QUIC correctness fix authored and merged by John Thacker.

## Match mutable state lifetime to redissection and matcher scope

Wireshark may dissect the same packet more than once. Persistent objects should not be recreated merely because a second pass revisits the packet, and packet-local matcher state should not leak into the next packet or a different protocol namespace.

Merged MR !25654 fixes HTTP/3 upgrade state so file-scoped information is not recreated on a second pass. Merged MR !25630 restructures JSON protocol-scoped wildcard matching so active state is reset for each packet and field lookup is constrained to the protocol whose predicate matched, instead of allowing a global field dictionary to make unrelated regex fields appear active.

**Implementation rule:** distinguish packet-local, conversation/file-scope, and process/global state explicitly. Use redissection indicators such as the established visited-pass mechanisms for persistent state creation, reset packet-local parser/matcher state at the packet boundary, and scope lookup tables to the same protocol namespace that gives their entries meaning.

**Confidence:** High. Two merged master fixes with concrete stale/cross-scope failure modes.

## Do not gate visible diagnostics on first-pass state

The `visited`/redissection distinction is useful for preventing duplicate mutation of persistent analysis state; it is not a general-purpose guard around tree construction or expert information. Visible diagnostics must be recreated whenever the packet is dissected so that later display/filter passes still contain them.

Merged MR !25595 adds HTTP request-target whitespace diagnostics. During review, Jaap Keuter specifically questioned a `!pinfo->fd->visited` guard around the expert item; the guard was removed before merge so the warning is generated on redissection as well as the first pass.

**Implementation rule:** use first-pass guards around stateful side effects that must happen once, not around protocol-tree fields or expert diagnostics whose presence is part of the current dissection result. If a diagnostic should be visible after redissection, emit it on every applicable dissection pass.

**Confidence:** Very high. Direct maintainer correction incorporated into a merged master MR.

## Restore shared metadata after a nested call that needs temporary context

Sometimes a nested dissector legitimately needs a different view of shared packet/record metadata than the enclosing layer. If the caller temporarily changes that shared metadata, the change must be scoped to the child call and the original value restored before control returns to the enclosing path.

Merged MR !25541 temporarily changes a pcap record's `pkt_encap` so an inner dissector such as Bluetooth can see the encapsulation it expects. The value is then restored after the child call; without restoration, later wiretap output can reject the packet because the packet encapsulation no longer matches the IDB/outer frame encapsulation.

**Implementation rule:** treat temporary changes to shared `packet_info`, record, or dissector-context metadata as push/pop state. Save the outer value, establish the child-specific value only for the nested call, and restore it on every return path before unrelated consumers observe the object.

**Confidence:** Very high. Merged master correctness fix authored by John Thacker and merged by Anders Broman, with a concrete fuzz reproducer showing the leaked-state failure.

## Attach diagnostics only when their packet/session association is justified

Expert information in a protocol tree implies that the diagnostic belongs to the packet/session being displayed. Errors discovered while parsing an external configuration source, key-log file, or Decryption Secrets Block should not be attached to an arbitrary protocol tree merely because that protocol eventually consumes the data.

During review of merged MR !25557, Guy Harris asked whether malformed SSH key-log entries could be exposed as SSH expert information. John Thacker pointed out that the bad entry may come from a configured file or a DSB and cannot necessarily be tied to the packet currently being dissected; attaching it would require evidence that the key is intended for that particular session.

**Implementation rule:** keep configuration/input-source diagnostics in their own reporting path unless there is a defensible identity mapping to the packet or conversation. If a protocol-tree expert item is used, ensure the error is actually attributable to that packet/session rather than merely to globally supplied auxiliary data.

**Confidence:** Very high. Direct Guy Harris review question and John Thacker design reasoning on a merged master MR.