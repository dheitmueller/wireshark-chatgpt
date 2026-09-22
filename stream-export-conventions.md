# Wireshark Stream-Export Conventions

This file records durable conventions for exporting higher-layer PDUs from stream dissectors. Current upstream source remains authoritative.

## Export only bytes actually dissected on the current pass

A TVB handed to a stream subdissector can contain bytes that are present in the current TCP segment but are not yet a complete higher-layer PDU. If the subdissector requests desegmentation, export/tap logic must not publish those incomplete bytes as though they had already been successfully dissected.

Merged master MR !13129, authored and merged by John Thacker, fixes TCP PDU export by checking the subdissector's desegmentation request before exporting. When `pinfo->desegment_len` is nonzero, the accepted code exports only the prefix before `desegment_offset`; if `desegment_offset` is zero, it suppresses the export entirely and leaves the bytes to be handled after reassembly. The MR specifically notes that this matters during the first pass and for one-pass TShark operation.

**Implementation rule:** treat a desegmentation request as a statement about consumption, not merely as a request for future convenience. Export/tap consumers should receive only the bytes the dissector committed as complete in that invocation. Do not use the full currently available stream TVB when a suffix will be replayed after reassembly.

**Review rule:** when adding PDU export around TCP or another desegmented transport, audit all dispatch paths—table, heuristic, conversation, and similar entry points—for the same consumed-byte semantics. An export path that bypasses the desegmentation decision can emit duplicate, partial, or misleading PDUs.

**Testing rule:** cover a PDU split across transport segments with both `desegment_offset == 0` and a nonzero consumed prefix, and exercise one-pass processing as well as normal multi-pass redissection. Exported bytes should correspond exactly to complete PDUs visible on that pass.

**Confidence:** Very high. Merged master stream/reassembly correctness fix authored and merged by John Thacker, with the first-pass/one-pass failure mode described directly in the MR.
