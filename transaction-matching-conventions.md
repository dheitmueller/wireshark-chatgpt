# Wireshark Transaction-Matching Conventions

This file records durable conventions for matching protocol requests and replies across long captures. Current upstream source remains authoritative.

## Bound ambiguous transaction identifiers with independent context

A protocol sequence or transaction identifier is not necessarily unique over the lifetime of a capture. When the wire identifier can be reused, pairing solely on that identifier can associate a request with an unrelated response from a much later transaction.

Merged master MR !15240, authored and merged by John Thacker, adds an optional maximum request/reply matching interval to PFCP. The preference defaults to zero to preserve the historical behavior when no bound is requested. Jaap Keuter's review also led the matcher to test sequence-number equality first and only compute the timestamp delta when the optional time bound is active; John noted that GTP and GTPv2 had similar matching code that could benefit from the same treatment.

**Implementation rule:** when a protocol's transaction ID can be reused, identify whether another independent dimension such as time, direction, conversation, message type, or negotiated context is required to disambiguate transactions in long captures. Do not silently assume a numerically equal identifier means the same transaction forever.

**Compatibility rule:** when adding a new disambiguation policy that could change historical pairing, prefer an explicit preference or a protocol-justified default rather than unexpectedly invalidating existing workflows. If disabled behavior is supported, make the disabled value unambiguous.

**Implementation detail:** short-circuit on the cheapest definitive key components before computing optional or expensive secondary context such as timestamp deltas.

**Testing rule:** include captures or synthetic cases in which the identifier is reused after enough time to create a plausible stale match, not just one request/reply pair.

**Confidence:** Very high for PFCP and high as a general review rule for protocols with reusable transaction identifiers. The master change was authored and merged by John Thacker and refined through maintainer review.

## Determine a matched frame's role from recorded frame identity, not merely from the shared key

A correlation key identifies a transaction family; it does not necessarily identify whether the current frame is the request, a retransmitted/repeated request, or the response. Once multiple frames can legitimately share the same key, request-versus-response presentation must use the explicit frame identities stored by the matcher.

Merged master MR !14784, authored by Jaap Keuter and committed by John Thacker, fixes SNMP request/response decoration when requests are repeated. The old code treated the originally recorded request frame specially and let other key matches fall into the response path. The accepted change instead identifies the actual response by comparing the current frame number with `response_frame_id`; matching frames that are not that response remain request frames and receive `response_in`. Stable backports including !14792 propagate the same correction.

**Implementation rule:** when a transaction record stores authoritative request and response frame identifiers, use those identities to decide the current frame's semantic role. Do not infer role from a key collision or from the assumption that there can be only one request-side frame.

**Testing rule:** include request, repeated/retransmitted request with the same transaction identifier, and response in one test. Verify that every request-side frame points to the response and that the response points back to the appropriate request while response timing is attached only to the actual response.

**Confidence:** Very high. Merged master correctness fix by Jaap Keuter, committed by John Thacker, with stable-branch propagation.
