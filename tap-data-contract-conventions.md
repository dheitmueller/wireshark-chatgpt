# Wireshark Tap-Data Contract Conventions

This file records durable conventions for values exported by dissectors to taps and consumed by statistics/analysis code. Current upstream source remains authoritative.

## Publish canonical normalized values once and reuse them in tap consumers

When a dissector has already converted a wire value into the semantic representation downstream analysis needs, export that canonical value through the tap contract rather than forcing every tap consumer to reconstruct it independently. Repeating normalization logic in the UI/statistics layer creates multiple subtly different interpretations of the same packet.

Merged master MR !14229, authored and merged by John Thacker, changes RTP Analysis to consume `info_extended_timestamp`, which the RTP dissector already calculates with wrap handling and places in the tap data. The analysis code had separately tried to extend the 32-bit RTP timestamp; its version contained subtle signed-versus-absolute-difference and casting problems. The accepted fix removes that duplicate state/calculation and uses the dissector-provided 64-bit extended timestamp throughout the statistics path. The MR was tested against multiple wrap/reordering edge cases referenced by earlier RTP issues/MRs.

**Architecture rule:** normalize a packet value at the layer that has authoritative protocol context, then carry that semantic value across the tap/API boundary. Downstream graphs, statistics, dialogs, and exporters should consume the normalized field instead of re-deriving it from the raw wire field unless they intentionally implement a different semantic operation.

**API rule:** if both raw and normalized forms are useful, name and type them distinctly in the tap structure so consumers cannot accidentally substitute one for the other. Prefer a representation wide enough to preserve the normalized domain rather than making each consumer repeat wrap-extension or sign-conversion policy.

**Testing rule:** when replacing duplicated downstream normalization with a canonical tap value, exercise wrap, reordering, boundary, and other cases that made the original reconstruction ambiguous. The objective is semantic equivalence across all consumers, not merely simpler code on ordinary captures.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker; the MR directly documents both the duplicate calculation and the edge cases that exposed its differences.

## Emit taps at the layer where the logical event exists

Tap delivery should represent the logical object that a consumer is counting or analyzing, not an incidental optional subfield. If a valid protocol message can have an empty payload or can hand its payload to a subdissector, placing `tap_queue_packet()` only inside the payload-specific path silently makes statistics depend on those unrelated conditions.

Merged master MR !13931 moves SOME/IP tap emission from payload dissection to the main message path so messages with empty payloads are still visible to statistics. During review, John Thacker explicitly checked two semantic consequences of the move: whether reassembled fragments versus complete PDUs were being reported, and whether a successfully selected payload subdissector should suppress the tap. The accepted placement makes the tap correspond to the SOME/IP message rather than to the presence or ownership of its payload; release backports !13948 and !13949 carry the same behavior.

**Architecture rule:** define the tap's event contract first (message, PDU, transaction, fragment, decoded payload, etc.) and queue it at the first layer where that logical event is complete. Do not make delivery conditional on optional payload length or on whether another dissector claims a subordinate region unless those conditions are part of the tap contract itself.

**Review rule:** moving a tap is a semantic change even if the tap structure is unchanged. Check reassembly boundaries, zero-length cases, subdissector success/failure, and duplicate-delivery risk so statistics do not change from “one logical message” to “one fragment” or vice versa by accident.

**Confidence:** Very high. Merged master correctness fix with direct John Thacker review of tap/reassembly/subdissector semantics and accepted stable backports.

## Preserve already-established packet-level tap metadata across later dissection exceptions

A later malformed/truncated region should not erase packet-level metadata that the dissector has already established and that remains semantically valid. If tap delivery is skipped simply because a subsequent TVB read throws, statistics can silently lose packets even though enough information was available to classify the packet or association.

Merged master MR !13765, authored and merged by John Thacker, changes SCTP so association information is still sent to the tap when later chunk dissection raises an exception, provided at least one chunk TVB was successfully established. The change also initializes the packet-level association index and direction before potentially throwing reads and uses the association identity determined for the first bundled chunk rather than allowing later parsing to leave the packet metadata unset or replace it opportunistically.

**Architecture rule:** identify which tap fields are packet-level facts and establish their safe defaults/identity before entering parsing that can throw. If those facts remain valid after a later malformed region, arrange final tap/tree publication so the exception does not suppress them.

**State rule:** for bundled protocol units that are required to share one packet-level association, select that identity at the protocol-defined point (for example, the first chunk) rather than repeatedly overwriting it as subordinate units are visited. If later units are illegally inconsistent, diagnose that inconsistency separately instead of letting it redefine the packet's tap identity.

**Testing rule:** include truncated or malformed packets that fail after the association/packet metadata has been established and verify that the tap still receives the valid metadata exactly once. Also cover legal bundled messages and, where possible, illegal mixtures so packet identity and expert diagnostics remain separate concerns.

**Confidence:** Very high. Merged master correctness change authored and merged by John Thacker, with the exception/tap behavior and first-chunk association semantics documented directly in the MR.