# Wireshark Field-Mask Semantics Conventions

This file records durable conventions for `hf_register_info` masks and the distinction between a whole numeric value and a bitfield extracted from a container. Current upstream checker behavior remains authoritative.

## A whole-width numeric field should normally be unmasked

For ordinary numeric protocol fields, a nonzero `hf` mask communicates that the field is a bitfield extracted from a containing value. Setting the mask to every bit in the field width is therefore usually misleading: the field is not a subfield at all, and the correct registration is an unmasked field with mask `0`.

Merged master MR !11053, authored by Martin Mathieson, adds checking for all-set masks and fixes existing `0xff`, `0xffff`, and `0xffffffff` registrations that represented complete values rather than component bitfields. This is complementary to merged !11698: an all-set mask has a narrow legitimate use as the first and only whole-value entry in a `proto_tree_add_bitmask()` field list, but that structural exception should not turn full-width masks into the normal registration form for standalone whole values.

**Field-definition rule:** use mask `0` when the registered field represents the complete decoded numeric value. Use a nonzero mask when Wireshark is expected to select and shift a genuine subset of bits from a containing value.

**Review rule:** treat a mask equal to the complete width of an integer field as suspicious. Determine whether the registration is a true component bitfield or merely a whole-value field written with a redundant mask; preserve the narrow `proto_tree_add_bitmask()` whole-header exception recognized by the checker.

**Testing rule:** run `tools/check_typed_item_calls.py` with `--check-bitmask-fields` before submission so suspicious masks are caught mechanically rather than relying only on manual review.

**Confidence:** Very high. Merged master checker/cleanup work from Martin Mathieson, consistent with the later checker refinement in !11698.

## Model genuine one-bit masked flags as boolean fields with the right label polarity

When a protocol element is a **masked one-bit flag**, prefer `FT_BOOLEAN` with its containing width (8, 16, 32, etc.), its nonzero bit mask, and an appropriate shared `true_false_string` via `TFS(...)`. A separately defined two-value `value_string` applied to `FT_UINT*` obscures the boolean contract and duplicates translations. Confirm that true/false labels preserve the protocol's actual 1/0 meaning; some flags use inverted labels such as No/Yes.

Merged John Thacker MR !10347 converts the current/next indicator across MPEG/DVB dissectors to shared `tfs_current_not_yet`. Merged Martin Mathieson MR !10359 extends the same cleanup to C/R, enable, acknowledgment, and related flags after running `tools/check_tfs.py --check-value-strings`; the author explicitly limits safe conversion to entries with a nonzero mask.

**Review rule:** only apply this transformation to real masked binary flags, preserving their mask width, filter semantics, and label polarity. A whole-value integer with a zero mask is not automatically interchangeable with `FT_BOOLEAN` merely because its observed values happen to be 0 or 1.

**Confidence:** High — merged broad cleanups by two experienced contributors, reinforced by a project checker and explicit scope caveat.

## Every field in a bitmask-set array must use the same containing width

A `proto_tree_add_bitmask` or `proto_tree_add_bitmask_list` group represents fields from one underlying container. Registering a mixture of `FT_BOOLEAN` containing widths (or otherwise mismatched `hf` widths) in the same bitmask array is inconsistent even if each individual mask looks plausible.

Merged master MR !9789 (Martin Mathieson) extends `tools/check_typed_item_calls.py` to discover field arrays, obtain each member's registered width, and warn when those widths disagree. The same MR corrects many dissectors' 8/16-bit boolean registrations, including cases where the mask remains `0x0001` but the containing width changes to 16. The checker also normalizes boolean widths for comparisons.

**Implementation/review rule:** derive every bitmask member's containing width from the actual on-wire parent field, check the width of the **whole array**, and run the typed-item checker after changing related registrations. Do not treat mask magnitude or `FT_BOOLEAN` alone as proof of a matching container width; validate both the parent and each member with asymmetric bit patterns.

**Confidence:** Very high. Merged mainline cross-dissector cleanup plus mechanical checker enhancement by Martin Mathieson.
