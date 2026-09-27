# Wireshark Field-Mask Semantics Conventions

This file records durable conventions for `hf_register_info` masks and the distinction between a whole numeric value and a bitfield extracted from a container. Current upstream checker behavior remains authoritative.

## A whole-width numeric field should normally be unmasked

For ordinary numeric protocol fields, a nonzero `hf` mask communicates that the field is a bitfield extracted from a containing value. Setting the mask to every bit in the field width is therefore usually misleading: the field is not a subfield at all, and the correct registration is an unmasked field with mask `0`.

Merged master MR !11053, authored by Martin Mathieson, adds checking for all-set masks and fixes existing `0xff`, `0xffff`, and `0xffffffff` registrations that represented complete values rather than component bitfields. This is complementary to merged !11698: an all-set mask has a narrow legitimate use as the first and only whole-value entry in a `proto_tree_add_bitmask()` field list, but that structural exception should not turn full-width masks into the normal registration form for standalone whole values.

**Field-definition rule:** use mask `0` when the registered field represents the complete decoded numeric value. Use a nonzero mask when Wireshark is expected to select and shift a genuine subset of bits from a containing value.

**Review rule:** treat a mask equal to the complete width of an integer field as suspicious. Determine whether the registration is a true component bitfield or merely a whole-value field written with a redundant mask; preserve the narrow `proto_tree_add_bitmask()` whole-header exception recognized by the checker.

**Testing rule:** run `tools/check_typed_item_calls.py` with `--check-bitmask-fields` before submission so suspicious masks are caught mechanically rather than relying only on manual review.

**Confidence:** Very high. Merged master checker/cleanup work from Martin Mathieson, consistent with the later checker refinement in !11698.

## Runtime-configurable bit partitions should use bit coordinates, not mutable static masks

A header field registration mask is a static description of a fixed wire layout. If preferences redefine how a fixed-width field is partitioned at runtime, validate that partition as one invariant and decode by bit offset/length rather than pretending each preference is an independent static mask.

Merged MR !7503 makes the O-RAN eAxC ID a configurable four-part 16-bit field. Martin Mathieson requested expert information when the configured widths do not add up to 16 and suggested the bit-oriented tree API. The accepted code verifies all component widths and their 16-bit total, reports an expert error for an inconsistent configuration, and uses `proto_tree_add_bits_ret_val()` for the runtime layout.

**Implementation rule:** for preference-defined bit layouts, validate the complete width budget before decoding and use bit-offset APIs whose coordinates can vary at runtime. Do not mutate registration-time masks to represent dynamic layouts.

**Confidence:** Very high. Merged master dissector change with direct Martin Mathieson review incorporated.


## Register one logical control word as one correctly-endian container with semantic masks

Guy Harris's merged master MR !7315, with release-3.6 and release-3.4 backports !7316 and !7317, corrects IEC 104 control-field handling by treating the four wire octets as one little-endian 32-bit value. The registered fields use masks for the actual semantic subfields; I frames and S/U frames have different type masks because their type encodings differ. `proto_tree_add_item_ret_uint()` then supplies Tx/Rx/U-type values from the same registered interpretation used for display.

**Field-definition rule:** when the wire format specifies one multi-octet control word split into bitfields, register the actual containing integer with the correct endianness and masks rather than independently reconstructing byte fragments. Where program logic also needs the value, prefer add-and-return field APIs so programmatic and displayed interpretations cannot drift apart.

**Confidence:** Extremely high. Master implementation and two stable backports authored by Guy Harris.
