# Numeric field display conventions

## Quantities are decimal-first

For protocol fields whose semantics are quantities people count or measure — lengths, sizes, counts, sequence numbers, capacities, block counts, and similar numeric quantities — prefer decimal display. If hexadecimal is also useful for correlation with protocol documentation or nearby wire values, use `BASE_DEC_HEX` so decimal remains primary.

Do not copy an existing hex presentation merely because neighboring storage/network fields historically used it. In !2045 Guy Harris explicitly called hex-only size presentation a Wireshark bug, then authored !2057 and !2058 to make the policy systematic across InfiniBand, iSCSI, NVMe, and SCSI.

Identifiers whose natural notation is hexadecimal — for example opaque keys or addresses defined/presented that way by the protocol — may remain `BASE_HEX`.

Evidence: merged !2045 (direct Guy Harris review), !2057 and !2058 (Guy Harris-authored).
