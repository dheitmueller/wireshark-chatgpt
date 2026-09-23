# Wireshark Registered-Field Decoding Conventions

This file records durable conventions for choosing protocol-tree field APIs and encoding flags. Current upstream APIs and dissectors remain authoritative.

## Prefer declarative registered-field decoding when the field API can express the wire encoding

When a registered field's type plus Wireshark's encoding flags fully describe its wire representation, prefer adding the field directly with the standard protocol-tree API instead of manually extracting, converting, allocating, and then adding an already-decoded value.

Merged master MR !12465 updates DECT NWK portable-identity BCD fields after `ENC_BCD_DIGITS_0_9` gained selectable digit endianness. The accepted code removes a local BCD digit table and manual `tvb_get_bcd_string()` / `proto_tree_add_string()` path and instead lets `proto_tree_add_item()` decode the registered string field using the appropriate BCD and endian flags. Merged master MR !12512 independently applies the same pattern to R09 BCD fields.

**Implementation rule:** before writing a local extraction/formatting path for a field that is ultimately only being displayed and filtered, check whether the registered field type and existing `ENC_*` flags already model the wire representation. If they do, prefer `proto_tree_add_item()` (or the corresponding standard tree helper) so byte ownership, bounds checking, field conversion, filtering, and presentation stay within the common API.

**Review rule:** treat manual extraction as justified only when the common API cannot express the protocol semantics or when the decoded value is genuinely required for additional control/state logic. If parser logic also needs the value, first look for an appropriate `proto_tree_add_item_ret_*` or related return-value helper rather than independently fetching the same bytes and adding them afterward.

**Testing rule:** encoding flags are part of the decoding contract. Exercise byte/digit ordering and partial-width edge cases that distinguish the requested encoding, rather than considering compile-only validation sufficient when a representative capture or focused synthetic vector can be produced.

**Confidence:** Very high for the API-selection rule. Two independent merged master dissector changes adopt the same conversion from manual BCD extraction to declarative registered-field decoding. The stronger runtime-test recommendation is review guidance motivated by !12465, whose author explicitly noted that the change was compile-tested only because no DECT NWK sample capture was available.