# Wireshark Protocol Field Backing Conventions

This file records durable conventions for protocol-tree field backing data extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## A field whose length may grow needs backing data that can grow with it

When a protocol-tree item's API permits its displayed length to be increased after creation, the item's backing tvbuff must satisfy two requirements simultaneously: it must start at the field's actual source offset, and it must retain access to the remaining source bytes that the later length adjustment is allowed to expose. A tvbuff rooted at offset zero gives the field the wrong origin; a subset limited to only the initial field length prevents a later valid extension.

Merged MR !25263, authored by John Thacker and merged by Anders Broman, fixes `FT_PROTOCOL` construction in `proto_tree_new_item()`. The previous protocol tvbuff ignored the item's nonzero start offset. The accepted implementation creates `tvb_new_subset_remaining(new_fi->ds_tvb, new_fi->start)`: the subset is rebased at the field start but retains the rest of the data source so `proto_item_set_len()` / `finfo_set_len()` can legitimately increase the field length. The change also re-enables assertions that the protocol value's backing tvbuff is at least as long as the recorded field length.

**Implementation rule:** model both origin and permitted future extent in a tree field's backing tvbuff. If an item's length is mutable, do not use an initial-length slice unless the API contract forbids later growth; instead rebase at the correct start while preserving the maximum source extent the item is allowed to claim.

**Invariant rule:** assertions around field length and backing-buffer extent are useful only after every construction path establishes the same invariant. Fix the backing-data model first, then enable the invariant check rather than suppressing an assertion that exposes inconsistent object state.

**Confidence:** Very high. Merged master parser/API correctness fix authored by John Thacker and accepted by Anders Broman, with the API-length semantics stated directly in the MR description and implementation.


## Store logical field extent separately from a larger backing tvbuff

Merged core MR !6993, authored by João Valverde, fixes negative slicing of `FT_PROTOCOL` values. A protocol field may be backed by a tvbuff that extends through later layers, so using the tvbuff's captured length as the protocol value's end made expressions such as negative slices operate on bytes outside the logical protocol item. The accepted change stores the field's semantic length in the protocol fvalue and updates it when `proto_item_set_len()` / `finfo_set_len()` finalizes the item later.

**Implementation rule:** when a field/value represents a subrange of a larger backing object, carry the logical range extent with the value. Do not infer semantic length from backing-buffer capacity merely because the buffer starts at the right place.

The later !25263 rule above refines the complementary backing-tvb requirement: preserve enough backing data to support legal later growth while separately recording the logical length that current consumers must honor.

**Confidence:** Very high. Merged core display-filter/value fix authored by João Valverde, with an explicit regression test for negative protocol-field slicing.
