# Wireshark Field Display Policy Conventions

This file records durable conventions for separating a protocol field's stored/filterable value from how that value is rendered in the packet tree. Current upstream `proto` APIs remain authoritative.

## Express value-display policy in the field registration when the API supports it

A field can need to retain its complete value for filtering, export, or `tshark` output while intentionally suppressing that value in the packet-tree label. When the field-registration API has a display flag for that contract, use the declarative flag rather than adding the field normally and then rewriting its label with `proto_item_set_text()` solely to hide the rendered value.

Merged master MR !12536, authored by John Thacker and approved/merged by Anders Broman, extended `BASE_NO_DISPLAY_VALUE` to string-like field types by validating the base display via `FIELD_DISPLAY()`, consistent with how display modifiers are handled for other field classes. SIP then changed `hf_sip_msg_hdr` to `BASE_NONE | BASE_NO_DISPLAY_VALUE` and removed the manual `proto_item_set_text()` used only to replace the string-bearing label with `Message Header`.

**Registration rule:** keep the machine-visible field value and the tree-label presentation as separate concerns. If a display modifier exactly expresses the desired presentation, encode it in `header_field_info` rather than mutating the item afterward.

**Review rule:** when code adds a field and immediately calls `proto_item_set_text()` merely to suppress or replace its normal rendered value, check whether a field display flag already expresses the intended policy. Manual text replacement is still appropriate when the label itself is dynamically enriched; it should not substitute for an existing declarative display contract.

**Confidence:** High. The generic field validator and a real SIP consumer were changed together in a merged master MR, demonstrating both API intent and accepted use.

## Do not assume every numeric field insertion path applies unit display modifiers identically

In the historical 2023 behavior documented by merged MR !9983, `BASE_UNIT_STRING` was rendered automatically for integer temperature fields, but the TECMP dissector did not get the intended unit label by using `proto_tree_add_double()` on an `FT_DOUBLE` field. John Thacker pointed the author to `proto_tree_add_double_format_value()`. The accepted change renders the voltage using an explicit format string and `unit_name_string_get_double()`, while using `BASE_UNIT_STRING` for integer temperatures.

**Compatibility/review rule:** distinguish a field's stored numeric value from its displayed label and verify that the specific field type and insertion API honor a display modifier in the target Wireshark branch. When they do not, format the value explicitly without changing the underlying numeric/filterable field representation. This documents a **historical API gap**, not an assertion that the limitation persists in current master; recheck `proto.c` before applying the workaround today.

**Confidence:** High for the observed 2023 behavior. Direct John Thacker technical guidance incorporated into a merged dissector fix.
