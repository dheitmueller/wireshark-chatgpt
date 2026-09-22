# Wireshark Single-Pass Analysis Conventions

This file records durable conventions for dissectors and taps that must work both when packets can be revisited and when a frontend consumes the capture in one pass. Current upstream source remains authoritative.

## Do not make one-pass output depend on information that can only be known later

A two-pass or interactive workflow can revisit earlier packets after later packets have established completeness or other derived state. A one-pass streaming frontend such as TShark cannot. Code shared by those execution models must not require future packets to have already established a fact before it preserves the data needed to produce the final result.

Merged master MR !14901, authored by John Thacker and merged by Gerald Combs, fixes TFTP Export Objects in single-pass TShark. The old path copied transfer blocks only after `last_package_available` was known, but during the first sequential pass that fact cannot be known until the final block is encountered; earlier blocks had therefore already been discarded. The accepted implementation preserves blocks during the first pass while no missing blocks are known, grows the object in file-scope memory, and only relies on the known final size on a visited/later pass. Release MRs !14908, !14909, and !14910 carry the same behavior to supported branches.

**Architecture rule:** when a tap/export/statistics result may be produced in a one-pass frontend, identify which predicates require future packets. Preserve the minimum state needed to complete the result before those predicates become knowable rather than discarding early input and assuming redissection will recover it.

**Lifetime rule:** speculative or partially accumulated data that might never be emitted should use a Wireshark-managed scope matching the analysis lifetime when that avoids manual cleanup on abandoned/incomplete paths. Do not trade one-pass correctness for leaks or fragile conditional frees.

**Review rule:** test stateful tap/export behavior in both one-pass and revisit/two-pass execution models when the code branches on `visited` state or on facts learned only at end-of-transfer/end-of-capture.

**Confidence:** Very high. Merged John Thacker master correctness fix with three accepted stable backports and an execution-model explanation in the change itself.

## Carry protocol identity across transport conversations when one-pass decoding needs earlier state

A logical protocol exchange can span multiple lower-layer conversations. When a later connection contains an ambiguous field whose interpretation depends on state learned on an earlier connection, keeping that state only on the first transport conversation makes one-pass decoding impossible even though a two-pass GUI redissection may appear to work after later packets have populated caches.

Merged master MR !14424 adds VMware vSPC vMotion tracking to the Telnet dissector. The protocol has `sequence` and `secret` fields with no delimiter, and the destination-side `VMOTION-PEER` message can arrive on a different Telnet conversation before that destination conversation has any independent way to know the sequence length. The accepted implementation adds a generic `CE_BLOB` conversation-element type, constructs a higher-level vMotion conversation keyed by the concatenated sequence+secret blob, and attaches the source Telnet session state to that higher-level identity. The later peer-side message looks up that vMotion conversation and imports the previously learned sequence length. The MR description explicitly distinguishes the one-pass problem from a two-pass/revisit case in which future information could otherwise mask the missing state.

**Architecture rule:** if a protocol-defined session or handoff spans transport connections, key the shared state by the protocol identity that actually survives the handoff rather than by one endpoint tuple. Carry only the state needed by the later connection and terminate or bound the synthetic/higher-level conversation when the protocol says the handoff is complete.

**Testing rule:** test both single-pass and revisit/two-pass decoding for cross-connection state. A capture that decodes correctly only after a second pass is evidence that required state was published too late or attached to the wrong conversation identity.

**Confidence:** High. Merged master feature approved and merged by Anders Broman, with the execution-model and cross-connection identity problem documented directly in the accepted implementation.

## Cache results from order-sensitive external analyzers on the first pass

Redissection does not necessarily visit packets in capture order. If a dissector delegates semantic interpretation to a stateful external engine whose contract requires chronological event processing, calling that engine again during arbitrary redissection can corrupt its state or produce results that depend on which packet the user happened to revisit.

Merged master MR !13977, authored and merged by Gerald Combs, fixes the Falco bridge for libsinsp's ordering requirement. The MR states explicitly that libsinsp requires events to be processed in order, so Wireshark processes/extracts the event data on the first pass and caches the resulting Wireshark-facing representation for later use rather than relying on the external analyzer to tolerate redissection order.

**Architecture rule:** separate chronological state advancement from repeatable presentation. Run an order-sensitive decoder/analyzer while packets are traversed in the guaranteed forward pass, retain the minimum semantic output needed by Wireshark, and make revisits consume that cached output instead of advancing the external engine again.

**Review rule:** identify libraries and helpers with hidden stream/history state before wiring them directly into a dissector. A function that looks like a pure “decode this packet” call may actually require all previous events in order; if so, arbitrary GUI redissection is a distinct execution model that must be handled explicitly.

**Testing rule:** after an initial sequential dissection, revisit packets out of order (including jumping backward and forward) and confirm that displayed fields and analyzer state remain stable. Also verify a fresh one-pass run so the caching path does not accidentally depend on prior redissection.

**Confidence:** Very high. Merged master architecture/correctness change authored and merged by Gerald Combs, with the third-party ordering constraint and first-pass caching strategy stated directly in the MR.

## Dispatch early protocol phases from information available at that phase

Some protocols permit useful payload before final negotiation has completed. Such early data must not depend on state learned only from a later handshake message or a redissection pass. When the early-phase selection and final negotiated selection can legitimately differ, keep them as distinct pieces of state instead of overwriting one with the other.

Merged master MR !13940 fixes QUIC 0-RTT application dissection. Before the ServerHello arrives, the server-selected ALPN is not yet known; the only valid application-protocol indication for 0-RTT is the client's offered ALPN. The accepted implementation adds access to the client ALPN and stores a separate 0-RTT application handle, because the finally negotiated application protocol can differ. John Thacker explicitly asked whether the bug only affected live/single-pass dissection; the author confirmed that the old path depended on second-pass state while the fix provides correctly parsed HTTP/3 on the first pass.

**Architecture rule:** make protocol-phase dependencies explicit. Dispatch data using only negotiation/state that is semantically available when that data is sent, and retain separate early/final state when later negotiation can select a different result.

**Testing rule:** test early-data paths in a fresh one-pass/live-equivalent run as well as in two-pass/redissection mode. A result that becomes correct only under `tshark -2` or after GUI redissection is a strong signal that future state is leaking backward into an earlier protocol phase.

**Confidence:** Very high. Merged master fix with direct John Thacker review of the one-pass distinction and an implementation that separates early and final application dispatch.