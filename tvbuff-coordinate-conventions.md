# TVBuff Coordinate-System Conventions

This note records durable guidance for offsets and consumed lengths when dissectors use subset tvbuffs. Current upstream source remains authoritative.

## Once parsing enters a subset tvbuff, keep offsets and consumed lengths in that subset's coordinate system

A subset tvbuff establishes both a byte boundary and a new offset origin. If a nested dissector is passed a subset and begins at offset zero, offsets and consumed-length values returned by that nested parser are relative to the subset unless the API explicitly documents otherwise. Do not subtract the parent tvbuff's original base offset from a value that is already subset-relative.

Merged MR !20285, authored and merged by John Thacker, fixes SSH local-extension dissection. The caller passed `payload_tvb`, a subset, and invoked the nested helper at offset zero. The returned dissected length was therefore already the correct payload-relative length; subtracting the enclosing packet's `offset` produced an incorrect result. The accepted change uses the helper's return value directly.

**Implementation rule:** at every subset boundary, identify the active coordinate space explicitly. Keep parent absolute offsets for operations on the parent tvbuff and subset-relative offsets for operations on the subset. Convert between coordinate systems only at a deliberate boundary where the target API actually requires the other representation.

**Review rule:** expressions that combine a child parser's returned length with a parent offset deserve scrutiny. A common bug pattern is applying the parent base twice—once when constructing/selecting the subset and again when adjusting a child-relative result.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker; it directly complements the notebook's existing guidance to use bounded subset tvbuffs for nested records.

## Apply an enclosing base exactly once when an API still consumes the parent tvbuff

The converse boundary matters too: an offset discovered while parsing a framed or nested record is not automatically valid against the original parent tvbuff. If a helper API is invoked on the parent tvbuff, translate the record-local offset back into the parent's coordinate system exactly once before the call. Keep the tvbuff argument and every offset/length supplied with it in the same coordinate system.

Merged master MR !15194 fixes Kafka Snappy decompression after a refactor to shared tvbuff decompression helpers. Chunk offsets were computed relative to a Kafka message beginning at a nonzero outer `offset`, but `tvb_child_uncompress_snappy()` was called on the parent tvbuff. Passing `pos` directly therefore decompressed from the wrong bytes; the accepted change uses `offset + pos`. The same MR restores explicit handling of a NULL child tvbuff when decompression fails.

**Implementation rule:** for every tvbuff API call, reason about the coordinate space of the tvbuff argument first, then express offsets and lengths in that same space. A nested parser can legitimately maintain local offsets while still calling an operation on the parent, but the parent base must be added at that boundary and nowhere else.

**Failure-path rule:** APIs that create derived/decompressed tvbuffs can fail. Check the returned child tvbuff before adding it as a data source, querying its length, or handing it to a nested dissector; coordinate correctness does not remove the need to honor nullable-result contracts.

**Confidence:** Very high. Merged master decompression correctness fix with a concrete regression and an accepted change that fixes both parent-coordinate arithmetic and decompression failure handling.
