# Wireshark Composite TVBuff Ownership Conventions

This file records compositing-specific ownership and empty-data contracts, derived from merged implementation evidence. Current upstream tvbuff API documentation remains authoritative.

## Append a member before relying on automatic composite lifetime cleanup

Merged MR !9359 fixes HTTP/2 inflated-header and CBOR indefinite-string paths that created a tvb_new_composite() even when there might be no data. The TVBuff contract requires the newly constructed composite to be immediately populated with at least one appended/prepended child for normal managed cleanup. Constructing an unused empty composite leaks its wrapper; tvb_composite_finalize() with no child can raise an exception.

**Implementation rule:** create composite TVBuffs lazily when the first nonempty child is ready; do not finalize a zero-member composite. If a downstream API requires a non-NULL TVB to represent valid zero-length data, provide a zero-length subset TVB instead of an empty composite. Be explicit about parent/child wrapper lifetime and backing bytes.

**Testing rule:** include empty, nonempty and malformed-input examples, and run memory diagnostics where possible. MR !9359 includes a CBOR empty indefinite string unit case and a malformed HTTP/2/Valgrind reproducer.

**Confidence:** Very high; merged HTTP/2/CBOR fix with documented TVBuff contract and added regression coverage.

## For decompression, a single owning real-data TVB can avoid intermediate wrapper leaks

Merged master !9273 addresses Valgrind-reported leaks in `tvb_uncompress_zstd` by replacing one composite TVB and child wrapper per output chunk with a contiguous output buffer. It reuses the Zstd output workspace, grows an accumulation buffer, wraps it once in `tvb_new_real_data()`, and gives that TVB ownership through `tvb_set_free_cb(..., g_free)` only on success; a failed decode frees the accumulated memory directly. A new `tvbtest` case covers decompressing zero bytes. This independently corroborates !9359's rule against constructing or finalizing zero-member composites. **Implementation rule:** when the consumer only needs one logically contiguous decompressed byte stream, prefer one owning TVB over gratuitous composites, and audit which object frees backing data on **every** success/failure path. **Testing:** run the zero-input case and Valgrind/memory diagnostics. **Confidence:** High; merged implementation plus targeted regression case (!9273).

## Attach newly decompressed TVBs to an owner or free them explicitly

Merged master !9236 fixes leaks in GELF, MCPE, multipart, MySQL, RTPS and SIP by changing suitable `tvb_uncompress*` calls to the corresponding `tvb_child_uncompress*` operations. John Thacker explicitly confirmed the ownership issue and requested API documentation making the distinction clear: `tvb_uncompress*` can return an **unattached** TVB that its caller must either free with `tvb_free()` or attach to another TVB's lifetime. The child helper attaches its returned TVB to the specified **parent**; saying it was merely attached to the compressed input `tvb` was a documentation error corrected in this MR.

**Ownership rule:** a returned TVB wrapper is not automatically managed just because its input was packet-managed. Prefer the child-decompression helper when the derived data has the parent's lifetime; otherwise document and implement an explicit free/attachment path. Check failure returns and ownership when the compressed source itself is an intermediate TVB. This complements the separate rules for zero-member composites (!9359) and owning Zstd buffers (!9273).

**Confidence:** Very high: merged leak fixes across multiple dissectors and explicit John Thacker review.

## Prefer shared child-TVB decompression helpers over dissector-local streaming composites

Merged master !9060 adds `tvb_uncompress_zstd()` and `tvb_child_uncompress_zstd()`, updates MongoDB Zstd dispatch, and replaces Kafka's bespoke Zstd streaming/composite path with the shared child helper. **Alexis La Goutte** requested the reusable helper and explicit decompression tests. The tests cover valid and concatenated Zstd frames, malformed bytes, truncated frames, and a malformed second frame; the author also supplied captures and `fuzz-test.sh` results. Use the child helper when the decoded TVB should follow the parent lifetime, and handle `NULL` on failure without dereferencing/dispatching incomplete data. Windows size_t-conversion warnings in tests required follow-up. Older merged release-branch Kafka loop fixes !9020/!9021 guard excessive iterations and buffer lifetime, but are **historical safeguards**, not reasons to reimplement Zstd decompression locally after !9060. **Confidence:** very high; merged API, consumer migration, reviewer feedback and tests, corroborating !9236.
