# Wireshark Field-Mask Semantics Conventions

This file records durable conventions for `hf_register_info` masks and the distinction between a whole numeric value and a bitfield extracted from a container. Current upstream checker behavior remains authoritative.

## A whole-width numeric field should normally be unmasked

For ordinary numeric protocol fields, a nonzero `hf` mask communicates that the field is a bitfield extracted from a containing value. Setting the mask to every bit in the field width is therefore usually misleading: the field is not a subfield at all, and the correct registration is an unmasked field with mask `0`.

Merged master MR !11053, authored by Martin Mathieson, adds checking for all-set masks and fixes existing `0xff`, `0xffff`, and `0xffffffff` registrations that represented complete values rather than component bitfields. This is complementary to merged !11698: an all-set mask has a narrow legitimate use as the first and only whole-value entry in a `proto_tree_add_bitmask()` field list, but that structural exception should not turn full-width masks into the normal registration form for standalone whole values.

**Field-definition rule:** use mask `0` when the registered field represents the complete decoded numeric value. Use a nonzero mask when Wireshark is expected to select and shift a genuine subset of bits from a containing value.

**Review rule:** treat a mask equal to the complete width of an integer field as suspicious. Determine whether the registration is a true component bitfield or merely a whole-value field written with a redundant mask; preserve the narrow `proto_tree_add_bitmask()` whole-header exception recognized by the checker.

**Testing rule:** run `tools/check_typed_item_calls.py` with `--check-bitmask-fields` before submission so suspicious masks are caught mechanically rather than relying only on manual review.

**Confidence:** Very high. Merged master checker/cleanup work from Martin Mathieson, consistent with the later checker refinement in !11698.