# Durable Wireshark conventions from MR batch 8061-8110

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

## Separate protocol identity, dissector entry-point identity, and presentation text

Merged master MR 8100, authored by Guy Harris, is high-authority evidence that a dissector name is not the same thing as a protocol name. A single protocol may need more than one callable dissector entry point when its wire framing differs by transport. Metadata such as an Exported-PDU tag that is later consumed by find_dissector must therefore name the callable dissector entry point, while user-visible labels may instead need the protocol's long or short presentation name.

Rule: choose the name namespace from the consumer. Registry and lookup metadata must carry a stable dissector identifier; protocol identity and human descriptions belong to their own APIs.

## Do not reconstruct parent transport from mutable dissection history when registration can state it

The same MR 8100 rationale explains why pinfo->layers is not a protocol-stack oracle: it is a history of dissection, so nested dissectors or multiple PDUs can place unrelated entries immediately before a later invocation. Other packet_info members can also be temporarily changed by nested dissectors.

Rule: when protocol syntax differs by transport, prefer separate transport-specific entry points that call common parsing code with an explicit parameter or context. Registration and call path are more reliable than inspecting the previous layer or mutable ambient state.

## Treat terminal protocol events as identity boundaries for persistent state

Merged master MR 8102, authored by John Thacker, creates a fresh TCP conversation for a SYN observed after RST or FIN even when its sequence number matches the previous conversation's base sequence. The same-sequence SYN without a prior terminal event remains a retransmission.

Rule: tuple or key equality alone is insufficient when the protocol defines lifecycle boundaries. Reset or create new state at authoritative close, reset, or restart events; use retransmission logic only while the old lifecycle is still active.

## Queue Qt handlers when nested event processing can destroy objects still on the dispatch stack

Closed MR 8081 is valuable negative evidence because its discussion isolates the real failure. WA_DeleteOnClose already schedules deleteLater, but a nested processEvents call inside a menu action can process that deferred deletion before the original QAction/QMenu call stack has unwound. Tomasz Mon recommended the accepted MR 8088 solution: connect affected actions with Qt::QueuedConnection, which runs the slot after menu handling completes. MR 8109 carries the same fix to release-4.0.

Rule: when an action can enter a nested event loop or trigger destruction of sender-owned context, explicitly queue the handler so current dispatch completes before destructive side effects run. Do not fix the symptom by merely changing delete timing without understanding reentrancy.

## Give asynchronous Qt actions ownership-safe state

Merged MR 8097 allocates the FieldInformation needed by a queued context-menu action on the heap with the menu as QObject parent. Martin Mathieson explicitly questioned leakage; Gerald Combs pointed out that parent ownership destroys it with the menu.

Rule: when queued delivery extends state beyond the creating stack frame, move that state to an object lifetime that covers the queued callback and make ownership explicit.

## Sanitize protocol-tree representation strings at the presentation boundary

Merged master MR 8077, authored by John Thacker, makes proto_item text and formatted-representation APIs convert their formatted output to printable valid UTF-8 before exposing it in Wireshark or tshark labels. It also keeps truncation marking correct.

Rule: label and representation APIs are presentation boundaries and may enforce printable valid text. This must not be confused with mutating the semantic value stored in a protocol string field merely to make its display prettier.

## Never reuse wire length for a transformed string without proving the lengths are identical

Merged master MR 8079, authored by John Thacker, fixes form-urlencoded parsing after percent decoding. The old code passed next_offset - offset, a wire-coordinate length, to get_utf_8_string operating on the decoded buffer. The accepted code uses the decoded string's own length.

Rule: after percent decoding, decompression, character conversion, unescaping, or any other transformation, keep source-range length and transformed-buffer length as separate variables and coordinate spaces.

## Pass parent-known semantic setup through dissector data and initialize the whole contract

Merged MR 8075 makes MGCP pass an initialized sdp_setup_info_t through call_dissector_with_data so SDP knows that an X-Osmux parameter means it should not install RTP or RTCP conversations. Review also found that other producers needed the expanded setup structure fully initialized.

Rule: when a parent knows information that changes a child's semantic action, carry it through the existing typed/context data path rather than re-detecting it from payload or global state. Adding a member to a shared context struct requires auditing and initializing all producers.

## Use TVBuff-bounded operations for attacker-controlled variable-length tokens

During MR 8075 review, John Thacker rejected a fixed 256-byte stack buffer used while scanning a vendor extension name because fuzz input could exceed the assumed limit. He recommended finding the actual end offset and using bounded TVBuff functions.

Rule: do not impose undocumented fixed stack capacities on packet-controlled tokens. Let TVBuff bounds define the accessible range and use bounded packet APIs.

## Scope checksums to the semantic message defined by the specification

Merged MR 8103 fixes STUN FINGERPRINT verification over RFC 4571/TCP by excluding the outer stream-framing length from the CRC input.

Rule: when an inner protocol defines a checksum over its own message, outer transport or framing prefixes are not part of the checksum merely because they share the same TVBuff. Compute over the protocol-defined semantic span.
