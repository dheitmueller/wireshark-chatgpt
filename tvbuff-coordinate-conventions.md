# TVBuff Coordinate-System Conventions

This note records durable guidance for offsets and consumed lengths when dissectors use subset tvbuffs. Current upstream source remains authoritative.

## Once parsing enters a subset tvbuff, keep offsets and consumed lengths in that subset's coordinate system

A subset tvbuff establishes both a byte boundary and a new offset origin. If a nested dissector is passed a subset and begins at offset zero, offsets and consumed-length values returned by that nested parser are relative to the subset unless the API explicitly documents otherwise. Do not subtract the parent tvbuff's original base offset from a value that is already subset-relative.

Merged MR !20285, authored and merged by John Thacker, fixes SSH local-extension dissection. The caller passed `payload_tvb`, a subset, and invoked the nested helper at offset zero. The returned dissected length was therefore already the correct payload-relative length; subtracting the enclosing packet's `offset` produced an incorrect result. The accepted change uses the helper's return value directly.

**Implementation rule:** at every subset boundary, identify the active coordinate space explicitly. Keep parent absolute offsets for operations on the parent tvbuff and subset-relative offsets for operations on the subset. Convert between coordinate systems only at a deliberate boundary where the target API actually requires the other representation.

**Review rule:** expressions that combine a child parser's returned length with a parent offset deserve scrutiny. A common bug pattern is applying the parent base twice—once when constructing/selecting the subset and again when adjusting a child-relative result.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker; it directly complements the notebook's existing guidance to use bounded subset tvbuffs for nested records.
