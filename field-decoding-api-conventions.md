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

## Let the registered field own its mask and shift exactly once

A registered `hf_` mask is not only display metadata: protocol-tree integer APIs apply it to obtain the field's logical value. If code manually extracts and shifts the value first and then passes that already-normalized integer through an API associated with the same masked `hf_`, the mask/shift can be applied a second time and produce the wrong value.

Merged master MR !12059, authored and merged by John Thacker, fixes SCCP RSN dissection by removing a manual fetch-and-shift path and using `proto_tree_add_item()` on the encoded bytes so the registered field performs the transformation once. The previously reviewed release backports !12063 and !12064 preserve the same correction.

**Implementation rule:** decide which layer owns extraction. If an `hf_` registration already describes the bit mask/shift, normally pass the raw wire bytes to the corresponding tree API and let it decode the logical value. If parser logic needs the decoded value as well, prefer the appropriate `proto_tree_add_item_ret_*()` helper rather than independently duplicating the mask operation.

**Review rule:** whenever code combines `tvb_get_*()`, manual `& mask`/`>> shift`, and `proto_tree_add_uint*()` for a field that itself has a nonzero registration mask, check for double transformation. The value supplied to an API that applies field semantics must be in the domain that API expects, not merely an integer that looks convenient at the call site.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker, independently preserved in two accepted stable-branch backports.

## Match encoding flags to the registered field type

`ENC_*` flags are interpreted according to the registered field type; they are not generic annotations that can safely be copied from adjacent fields. String-character encodings belong on string fields. Integer/boolean/other non-string items should use the byte-order or neutral encoding required by that type, and use `ENC_NA` when no encoding transformation applies.

Merged master MR !11938, authored by Guy Harris, fixes the Tibia dissector because it passed a negotiated string encoding into several non-string `proto_tree`/`ptvcursor` additions. The MR states the contract directly: Wireshark does not guarantee that string encodings work for non-string items, and in these cases they did not. The accepted fix changes command bytes to `ENC_NA` and removes the string-encoding bits from a non-string little-endian item. Merged release backports !11941 and !11942 carry the same correction.

**Implementation rule:** choose the encoding argument from the `hf_` field's actual registered type and wire representation, not from a surrounding conversation-wide notion of encoding. A protocol's negotiated text encoding should affect only fields whose semantics are text.

**Review rule:** when a shared `encoding` variable is threaded through a dissector, inspect every call site that ORs or forwards it. If the destination field is not a string/character field, verify independently that the supplied encoding bits are valid for that field type.

**Testing/tooling rule:** treat `tools/fix-encoding-args.pl` and related encoding-argument diagnostics as semantic aids, not cosmetic cleanup. A mismatch can change decoded values or invoke unsupported API behavior.

**Confidence:** Extremely high. The master correctness fix is authored by Guy Harris and preserved in two merged release backports.
