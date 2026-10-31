# Wireshark Composite TVBuff Ownership Conventions

This file records compositing-specific ownership and empty-data contracts, derived from merged implementation evidence. Current upstream tvbuff API documentation remains authoritative.

## Append a member before relying on automatic composite lifetime cleanup

Merged MR !9359 fixes HTTP/2 inflated-header and CBOR indefinite-string paths that created a tvb_new_composite() even when there might be no data. The TVBuff contract requires the newly constructed composite to be immediately populated with at least one appended/prepended child for normal managed cleanup. Constructing an unused empty composite leaks its wrapper; tvb_composite_finalize() with no child can raise an exception.

**Implementation rule:** create composite TVBuffs lazily when the first nonempty child is ready; do not finalize a zero-member composite. If a downstream API requires a non-NULL TVB to represent valid zero-length data, provide a zero-length subset TVB instead of an empty composite. Be explicit about parent/child wrapper lifetime and backing bytes.

**Testing rule:** include empty, nonempty and malformed-input examples, and run memory diagnostics where possible. MR !9359 includes a CBOR empty indefinite string unit case and a malformed HTTP/2/Valgrind reproducer.

**Confidence:** Very high; merged HTTP/2/CBOR fix with documented TVBuff contract and added regression coverage.
