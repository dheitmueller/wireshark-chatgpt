# Wireshark Reassembly Identity Conventions

## A reassembly key must represent semantic PDU identity

Merged release-3.6 MR !7548, authored by John Thacker and backported from master, fixes TCP reassembly collisions. A frame number is usually distinctive but is not sufficient when one encapsulating frame contains multiple TCP PDUs. A TCP sequence number is also not sufficient because sequence numbers can wrap or be reused in long/reused connections.

The accepted TCP key therefore combines endpoint identity with the multisegment PDU's first frame and starting sequence. The hash may optimize for the commonly distinctive first-frame value, but equality still tests the complete semantic key.

**Rule:** define equality from the full semantic identity needed to distinguish reassembly instances. Do not confuse a convenient hash discriminator with the complete key.

## Reuse the transport's reassembly-key semantics

Merged release-3.6 MR !7551, also authored by John Thacker, moves TLS desegmentation to TCP's reassembly table functions. TLS had mixed pieces of the starting sequence into the frame-number ID; the shared TCP functions can compare the full multisegment-PDU identity directly.

**Rule:** when a higher-layer dissector is reassembling units that are defined by transport reassembly state, reuse the transport's established key/equality mechanism rather than building a parallel lossy key.

## Retransmission classification is not reassembly identity

Closed/superseded MR !7543 contains useful John Thacker review explaining that retransmissions not belonging to a multisegment PDU must not simply be passed again to stateful subdissectors. Sender-side retransmission analysis and the capture reassembler's question of whether bytes are already represented are distinct state domains. The later merged !7949 remains stronger authority for this rule.

**Confidence:** Very high for !7548/!7551; supporting/negative evidence only for !7543.

## Foundational master evidence: complete equality and lifetime-aware keys

Merged master MR !6440, authored by John Thacker, provides the master implementation behind the same identity rule. Encapsulation can place more than one TCP PDU from one stream in a physical frame, so the accepted key includes endpoint addresses and ports, the multisegment PDU's first frame, and its starting sequence. The hash intentionally favors the usually distinctive first-frame value, while equality checks every semantic discriminator.

The MR also makes ownership part of the key contract: temporary lookup/delete keys use shallow address copies because they do not escape the operation, while persistent table keys deep-copy addresses and free those copies on destruction.

Merged master MR !6447 then switches TLS desegmentation to these TCP reassembly-table functions instead of mixing truncated sequence bits into an integer fragment ID.

**Identity rule:** a hash is only an accelerator; collision-safe equality must compare the full PDU identity. Higher layers should reuse the transport's identity contract rather than inventing a compressed parallel key.

**Ownership rule:** temporary search keys may borrow storage that outlives the lookup, while persistent keys must own every component whose source lifetime would otherwise end.

**Confidence:** Extremely high. Both are merged master changes authored by John Thacker and provide the original implementation evidence for the later stable-branch records above.


## Include security or epoch context when fragment identifiers can be reused

Merged master MR !3206 expands the Bluetooth Mesh upper-transport reassembly key from source + SeqZero to source + SeqZero + IV index + a network-key/IV-index hash. The shorter key could identify fragments from different mesh security/IV contexts as though they belonged to one reassembly instance.

**Identity rule:** if a fragment sequence identifier can be reused across keys, IV epochs, interfaces, directions, or comparable protocol contexts, include that context in reassembly equality. A key is complete only when equal keys necessarily refer to the same logical PDU instance.

**Confidence:** High. Merged master correctness change with no contrary maintainer discussion.
