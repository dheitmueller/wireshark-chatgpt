# Wireshark Registered-Field Decoding Conventions

This file records durable conventions for choosing protocol-tree field APIs and encoding flags. Current upstream APIs and dissectors remain authoritative.

## Prefer declarative registered-field decoding when the field API can express the wire encoding

When a registered field's type plus Wireshark's encoding flags fully describe its wire representation, prefer adding the field directly with the standard protocol-tree API instead of manually extracting, converting, allocating, and then adding an already-decoded value.

Merged master MR !12239 extends packed-BCD tree decoding so `ENC_BCD_DIGITS_0_9` can be combined with explicit big- or little-endian digit ordering, and audits existing users to preserve their intended semantics. That API work enables later dissector simplifications. Merged master MR !12465 updates DECT NWK portable-identity BCD fields after selectable BCD endianness became available: the accepted code removes a local BCD digit table and manual `tvb_get_bcd_string()` / `proto_tree_add_string()` path and instead lets `proto_tree_add_item()` decode the registered string field using the appropriate BCD and endian flags. Merged master MR !12512 independently applies the same pattern to R09 BCD fields.

**Implementation rule:** before writing a local extraction/formatting path for a field that is ultimately only being displayed and filtered, check whether the registered field type and existing `ENC_*` flags already model the wire representation. If they do, prefer `proto_tree_add_item()` (or the corresponding standard tree helper) so byte ownership, bounds checking, field conversion, filtering, and presentation stay within the common API.

## Keep encoded field values distinct from derived protocol values

A `proto_tree_add_item_ret_*` helper returns the registered field's decoded value, including the field's mask/shift semantics. That is exactly what parser logic should reuse when the protocol semantic value is the same as the registered field value, but some protocols apply additional scaling or transformation after the encoded field is extracted.

Merged master MR !12254 demonstrates this in SOME/IP-TP. Martin Mathieson suggested `proto_tree_add_item_ret_uint()` to avoid separately fetching the 32-bit word. The TP offset is encoded in a masked field, so the return helper produces the post-mask/post-shift encoded offset; the specification then defines the semantic byte offset as that value multiplied by 16. The accepted implementation keeps an encoded wire-backed field, obtains it through the return-value helper, performs the protocol-defined shift, and presents the scaled offset as a separate generated field rather than pretending the derived value occupies the original bits.

**Review rule:** prefer a `proto_tree_add_item_ret_*` helper when its registered-field transform produces the value parser logic actually needs. If the protocol applies an additional multiplier, unit conversion, or combination with other state, preserve the wire-backed value as such and derive the semantic value separately. Do not change an `hf_` mask merely to make the returned integer convenient for control flow.

**Testing rule:** encoding flags and post-extraction transforms are both part of the decoding contract. Exercise byte/digit ordering, masks, scaling, and partial-width edge cases that distinguish the requested encoding, rather than considering compile-only validation sufficient when a representative capture or focused synthetic vector can be produced.

**Confidence:** Very high. !12239 establishes the shared BCD encoding capability that later merged dissector changes use; !12465 and !12512 independently converge on declarative field decoding. !12254 records substantive maintainer discussion of the boundary between encoded registered values and derived protocol semantics.