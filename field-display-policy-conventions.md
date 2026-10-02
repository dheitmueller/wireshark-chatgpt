# Wireshark Field Display Policy Conventions

This file records durable conventions for separating a protocol field's stored/filterable value from how that value is rendered in the packet tree. Current upstream `proto` APIs remain authoritative.

## Express value-display policy in the field registration when the API supports it

A field can need to retain its complete value for filtering, export, or `tshark` output while intentionally suppressing that value in the packet-tree label. When the field-registration API has a display flag for that contract, use the declarative flag rather than adding the field normally and then rewriting its label with `proto_item_set_text()` solely to hide the rendered value.

Merged master MR !12536, authored by John Thacker and approved/merged by Anders Broman, extended `BASE_NO_DISPLAY_VALUE` to string-like field types by validating the base display via `FIELD_DISPLAY()`, consistent with how display modifiers are handled for other field classes. SIP then changed `hf_sip_msg_hdr` to `BASE_NONE | BASE_NO_DISPLAY_VALUE` and removed the manual `proto_item_set_text()` used only to replace the string-bearing label with `Message Header`.

**Registration rule:** keep the machine-visible field value and the tree-label presentation as separate concerns. If a display modifier exactly expresses the desired presentation, encode it in `header_field_info` rather than mutating the item afterward.

**Review rule:** when code adds a field and immediately calls `proto_item_set_text()` merely to suppress or replace its normal rendered value, check whether a field display flag already expresses the intended policy. Manual text replacement is still appropriate when the label itself is dynamically enriched; it should not substitute for an existing declarative display contract.

**Confidence:** High. The generic field validator and a real SIP consumer were changed together in a merged master MR, demonstrating both API intent and accepted use.


## Prefer one truthful field identity with declarative printable-byte presentation

Do not create parallel fields with the same filter identity merely to switch between raw bytes and a printable-string presentation at runtime. If the wire object is semantically bytes but ASCII presentation is useful when possible, keep one bytes field and use the field-registration display policy that can render printable content.

Merged master MR !1706, authored and merged by Anders Broman, removes PFCP's paired `FT_BYTES` / `FT_STRING` registrations and the runtime `tvb_ascii_isprint()` branch. The accepted replacement keeps the fields as `FT_BYTES` and uses `BASE_SHOW_ASCII_PRINTABLE`, while also correcting duplicate filter abbreviations.

**Field rule:** choose the registered field type from the wire value's semantics; use supported display modifiers to improve human presentation without inventing a second semantic field. Filter abbreviations must remain unique and stable identifiers, not presentation variants.

**Confidence:** Very high. Merged master cleanup authored and merged by Anders Broman.
