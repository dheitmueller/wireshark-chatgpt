# Wireshark Packet Address Conventions

This file records durable conventions for populating packet address metadata used by columns, taps, graphs, and other downstream consumers. Current upstream APIs remain authoritative.

## Populate semantic packet addresses instead of writing presentation columns directly

When a dissector knows the source and destination identities for a packet, record them in the appropriate `packet_info` address fields. Do not use resolved/unresolved display columns as the primary storage mechanism for that information: packet addresses are consumed by more than the packet-list renderer, and the column machinery can derive both resolved and unresolved presentation from the semantic address object.

Merged master MR !15485, authored and merged by John Thacker, fixes LAPD after both `pinfo->src` and `pinfo->dst` had been set to the destination and the dissector separately wrote only the resolved data-link columns. That left the general Flow Graph without correct address data and made resolved/unresolved presentation inconsistent. The accepted fix sets `pinfo->dl_src` and `pinfo->dl_dst` using `AT_STRINGZ`, shallow-copies those values to `pinfo->src` and `pinfo->dst`, and removes the direct `COL_RES_DL_SRC`/`COL_RES_DL_DST` writes. Merged !15497 carries the same fix to the maintained 4.2 branch.

**Implementation rule:** make `packet_info` address fields the source of truth when the protocol supplies address semantics. Populate the layer-specific address fields (`dl_*`, `net_*`, etc.) and the effective `src`/`dst` view as required by the dissector stack; let standard column and name-resolution handling render them rather than manually synchronizing presentation strings.

**Review rule:** when changing address handling, test consumers beyond the packet-list columns, especially Flow Graph/sequence analysis and taps. Verify both resolved and unresolved display modes and ensure source/destination direction is represented correctly in the underlying `packet_info`, not only visually in one UI path.

**Confidence:** Very high. Merged master correctness change authored and merged by John Thacker, followed by a merged stable-branch backport of the same semantic fix.
