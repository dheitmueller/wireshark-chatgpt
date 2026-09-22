# Wireshark Request/Response State Conventions

This file records durable conventions for correlating request/response protocols across packets and dissection passes. Current upstream source remains authoritative.

## Build transaction state from whichever side is observed first

A capture does not guarantee that the request side of a transaction is present before the response side. Capture can begin mid-session, packets can be missing, and Wireshark can revisit frames out of order. Request/response bookkeeping therefore must not require a request-first visitation sequence in order to establish a useful transaction object.

Merged master MR !13289, authored and merged by John Thacker, changes the RSVD dissector to key task state by the protocol request ID, look up an existing task before creating one, and create role-aware partial state on the first pass. If a response is seen first, the transaction can record response/last-exchange information without inventing request-side data; if the matching request is later available, the missing request frame, first-exchange metadata, and opcode can be filled in. Redissection reads the established state rather than creating a second transaction.

**Architecture rule:** choose a stable protocol transaction key and permit the associated state object to be partially populated from either direction. Treat request-side and response-side facts independently until both are known. Do not encode “request packet was visited first” as an invariant unless the protocol/capture contract truly guarantees it.

**Redissection rule:** mutate persistent correlation state only at the intended first-pass points and make later passes consume that state idempotently. Lookup-before-create is part of correctness: redissection or reverse-order access must not manufacture duplicate transaction objects.

**Review implication:** test response-before-request, missing-request, missing-response, ordinary request/response, and redissection/random-access cases. A sequential complete capture can conceal order-dependent state bugs.

**Confidence:** Very high. Merged master state-management fix authored and merged by John Thacker, with the request-ID key and response-before-request behavior visible in the accepted implementation.