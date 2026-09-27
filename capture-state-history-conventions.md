# Wireshark Capture-State History Conventions

This file records durable conventions for state learned from earlier packets and consumed by later dissection. Current upstream source remains authoritative.

## Preserve time-varying capture mappings as history, and preserve the frames that establish them

When protocol state can redefine an identifier during a capture, a last-value map is not sufficient for redissection or packet-subset export. Store enough history to resolve the mapping as of the packet being dissected, and make context-establishing packets explicit dependencies when later packets require them.

Merged MR !25040, authored and merged by John Thacker, changes generic-netlink family-name tracking for `CTRL_CMD_NEWFAMILY` from a single-value mapping to a `wmem_multimap` that retains mappings across their defining frames. The same dynamic family ID may legitimately identify different families at different points in a capture. The MR also marks the defining control packet as depended upon so exporting related packets retains the state needed to decode them correctly.

**Implementation rule:** for capture-learned mappings whose meaning can change over time, key lookup by both semantic identifier and capture position (directly or through a history-aware structure). If later packets are not independently decodable without the defining packet, register that packet dependency so packet export preserves the required context.

**Confidence:** Very high. Merged master state-management change authored and merged by John Thacker.

## Snapshot mutable conversation state at packet boundaries when redissection must be deterministic

Merged master MR !6852 adds session context to AT command/response decoding. Conversation proto-data keeps the evolving command state, but each packet stores an initial snapshot before parsing and reuses that snapshot when the same packet is parsed again. This prevents an earlier packet from being reinterpreted using conversation state learned only from later packets. Stig Bjørlykke also asked about packets outside a USB conversation; the accepted implementation preserves the old stateless behavior when no suitable conversation exists.

**Implementation rule:** when parsing a packet both consumes and advances mutable conversation state, distinguish the state entering the packet from the state after the packet. Persist enough pre-packet state in packet proto-data or an equivalent history so redissection is repeatable and capture-navigation order does not change semantics.

**Fallback/testing rule:** define behavior when conversation context is unavailable, and test original-order dissection, backward navigation/redissection, and invocation without the expected conversation transport.

**Confidence:** High. Merged master state architecture with direct Stig Bjørlykke review and a supplied capture.
