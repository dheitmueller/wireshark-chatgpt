# Wireshark Field-Mask Semantics Conventions

This file records durable conventions for `hf_register_info` masks and the distinction between a whole numeric value and a bitfield extracted from a container. Current upstream checker behavior remains authoritative.

## A whole-width numeric field should normally be unmasked

For ordinary numeric protocol fields, a nonzero `hf` mask communicates that the field is a bitfield extracted from a containing value. Setting the mask to every bit in the field width is therefore usually misleading: the field is not a subfield at all, and the correct registration is an unmasked field with mask `0`.

Merged master MR !11053, authored by Martin Mathieson, adds checking for all-set masks and fixes existing `0xff`, `0xffff`, and `0xffffffff` registrations that represented complete values rather than component bitfields. This is complementary to merged !11698: an all-set mask has a narrow legitimate use as the first and only whole-value entry in a `proto_tree_add_bitmask()` field list, but that structural exception should not turn full-width masks into the normal registration form for standalone whole values.

**Field-definition rule:** use mask `0` when the registered field represents the complete decoded numeric value. Use a nonzero mask when Wireshark is expected to select and shift a genuine subset of bits from a containing value.

**Review rule:** treat a mask equal to the complete width of an integer field as suspicious. Determine whether the registration is a true component bitfield or merely a whole-value field written with a redundant mask; preserve the narrow `proto_tree_add_bitmask()` whole-header exception recognized by the checker.

**Testing rule:** run `tools/check_typed_item_calls.py` with `--check-bitmask-fields` before submission so suspicious masks are caught mechanically rather than relying only on manual review.

**Confidence:** Very high. Merged master checker/cleanup work from Martin Mathieson, consistent with the later checker refinement in !11698.
## Value tables for masked fields use the normalized post-mask value

An `hf_register_info` mask does not merely control highlighting. For integer fields, Wireshark extracts and shifts the selected bits before applying a `value_string`. A value table therefore must be written in the domain of the normalized field value, not the raw on-wire bit positions.

Merged master MR !458 fixes BSSAP's DLCI Control Channel field. The field mask is `0xc0`, so raw patterns `0x80` and `0xc0` become normalized values `0x02` and `0x03`. The old table used the raw patterns and therefore failed after normal field extraction; the accepted table uses the normalized values. Anders Broman also suggested `proto_tree_add_bitmask_list()` as a cleaner way to represent the grouped bitfield.

**Field-definition rule:** when a numeric field has a nonzero mask and a value table, derive the table keys from the value Wireshark exposes after masking/shifting. Do not copy the raw bit patterns from a packet diagram into the table without accounting for the field mask.

**Review rule:** audit the mask, declared field width, add-item API, and value table together. A table that looks correct against the specification's byte diagram can still be wrong for the registered field's normalized value domain.

**Confidence:** Very high. Merged master correctness fix, approved after maintainer review.


## Q.933 independently confirms normalized value-table semantics

Merged master MR !217 fixes Q.933 PVC Status by correcting both the registered mask and the `value_string`. The old mapping used bit-position values while the masked field exposed normalized values, and its mask also omitted the Active bit. Stable MRs !218-!220 carry the same correction.

This independently corroborates the !458 BSSAP rule above: review a nonzero mask and its value table together, and write table keys in the logical post-mask/post-shift domain that Wireshark exposes.

**Confidence:** Very high. Merged master correctness fix with direct Pascal Quantin review and three accepted release backports.
## When supplying a masked boolean value manually, preserve the mask's bit position

A registered Boolean mask describes the bit position that the field occupies in its containing value. If code first reduces that bit to a logical 0/1 and then calls a value-taking tree API associated with the masked field, the field machinery can apply the mask to the already-normalized value and turn a true value into false.

Merged master MR !169 fixes QUIC Key Phase display. The dissector computed `key_phase = (first_byte & SH_KP) != 0`, but `hf_quic_key_phase` is a masked Boolean whose bit is not bit zero. Passing 0/1 directly to `proto_tree_add_boolean()` therefore did not preserve the registered bit position; the accepted correction passes the true value aligned to the field mask.

**Implementation rule:** when a masked field is added from bytes, prefer a tree API that consumes the raw containing value/bytes and lets the registered mask extract the field. If a value-taking API is required, pass a value in the domain that API and the registered mask expect; do not normalize to 0/1 or shift to bit zero and then apply the same mask again.

**Review rule:** audit manually computed values passed to masked `proto_tree_add_uint*()` and `proto_tree_add_boolean*()` calls for double normalization or lost bit position. The display value and the API input domain are not always the same thing.

**Confidence:** Very high. Merged master correctness fix; it independently reinforces the broader rule that registered masks should own extraction exactly once.
