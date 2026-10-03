# Wireshark Raw Payload Field Conventions

This file records how raw payload fields can coexist with successful subdissection.

## Keep a real raw payload field available, but hide redundant presentation after successful subdissection

A raw payload is still a genuine packet field even when a more specific dissector can decode its contents. Keeping that field registered makes the bytes available to display filters and programmatic consumers. Showing both raw bytes and the complete semantic decode in the tree, however, can be redundant.

Merged master MR !121 changes RTP so `rtp.payload` is always added. If the RTP payload-type dissector successfully claims the data, Wireshark marks the raw payload item hidden; if no subdissector claims it, the raw bytes remain visible.

**Field rule:** when raw bytes are independently useful to filters/exporters but usually superseded in the visible tree by a successful child dissector, add the real field consistently and hide it conditionally after successful dispatch. Do not fabricate a hidden field solely for filtering; the raw payload here corresponds to actual captured bytes.

**Dispatch rule:** hide the raw item only on successful subdissector dispatch. If dispatch fails or no dissector exists, preserve a visible representation of the undecoded payload.

**Confidence:** Very high. Merged master change authored and merged by Pascal Quantin.
