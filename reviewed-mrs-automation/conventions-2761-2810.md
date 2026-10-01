# Durable conventions from Wireshark MRs !2761–!2810

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This batch contains 47 merged MRs and three closed MRs (!2790, !2777, !2776). The corpus snapshots contain no non-system human discussion notes, so accepted merged implementations and maintainer authorship are the principal evidence weights.

## Normalize identifiers before keying state — !2806
When an identifier shares storage with framing/status flags, remove non-identity bits before comparison, hashing, or lookup. The accepted CAN changes apply `CAN_EFF_MASK` before AUTOSAR NM matching and Signal PDU lookup.

## Align field type, wire width, and extraction length — !2809
A registered `FT_UINT*` width and the item byte length must both represent the actual wire value. A correct parser cursor does not make a mismatched field registration semantically correct.

## Pass stable identity across component boundaries — !2801
When a component can recompute mutable analysis data from the capture, pass the stable identity rather than a snapshot whose statistics can become stale. RTP Player cross-dialog APIs were reduced from `rtpstream_info_t` to `rtpstream_id_t`.

## Bound the complete next fixed header before reading it — !2787
A repeated TLV parser should enter an iteration only when every fixed header byte needed by that iteration lies within the protocol-declared message boundary.

## Put reusable units in field metadata — !2786
Use Wireshark's unit display mechanism for dB/dBm-style units rather than embedding units in field labels. The semantic field name and reusable presentation metadata are separate concerns.

## Prefer explicit protocol metadata to compatibility heuristics — !2785
When a protocol revision provides an explicit length/location, use it. Keep an older fixed-offset assumption only as a fallback for captures that do not carry the explicit value.

## Keep generated output synchronized with its source — !2779
The accepted GSM MAP correction updates both ASN.1 input and generated C. Generated derivatives are not a substitute for changing the authoritative source.

## Separate reusable semantic layers from carrier framing — !2764
A coherent semantic subprotocol that can appear over multiple transports belongs behind a reusable boundary. UAVCAN keeps CAN transport handling separate from DSDL message semantics so DSDL can later be reused over another carrier.

## Give distinct capture formats explicit openers — !2762
Guy Harris-authored merged !2762 gives CommView NCF and NCFX separate Wiretap openers and extension identities. Related formats with different recognition/parsing contracts should have explicit format-specific entry points; common internals can still be shared below them.

## Bound packet-controlled resource commitments — !2766
Validate packet-controlled counts before committing storage and then verify that the corresponding bytes are present. This is historical corroboration; where valid large counts are possible, the notebook's later grow-as-validated collection rule is the stronger design.

No new testing, review-comment, or submission convention was promoted from this batch because its corpus snapshots contain no substantive human review discussion.

No SMPTE ST 291/VANC packet type was encountered.
