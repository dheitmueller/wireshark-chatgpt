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

## Prefer declarative numeric display bases over hand-built table formatting

Protocol-tree fields should use the display policy encoded by their registration whenever the standard bases already express the intended numeric representation. Hand-building labels to align columns or repeat hexadecimal and decimal values makes one dissector visually idiosyncratic and bypasses common rendering behavior.

In merged MR !1048, Anders Broman, Alexis La Goutte, and Jaap Keuter steer MQ integer presentation toward `BASE_HEX_DEC` and `BASE_DEC_HEX` rather than custom spacing and format strings. Jaap explicitly points out that those bases exist to keep representation consistent among protocols; the final field registration adopts `BASE_HEX_DEC | BASE_EXT_STRING`.

**Field rule:** use registered display bases, value-string tables, and other field metadata before custom formatted labels. Do not make packet-tree text imitate a fixed-width table when ordinary field rendering already carries the information.

**Confidence:** Very high. Direct review from Anders Broman, Alexis La Goutte, and Jaap Keuter was incorporated before the MR merged.

## Keep captured alignment and padding bytes visible when they explain wire layout

Bytes that exist on the wire solely for alignment are still useful evidence when validating offsets. Silently advancing over them can make a correct parser look mysterious and malformed alignment harder to diagnose.

Merged MR !1037 initially skipped the two XDR alignment bytes following each six-byte sFlow LAG system ID. Anders Broman questioned the invisible gap and Alexis La Goutte explicitly asked that all bytes be displayed. The accepted implementation adds a `Padding` `FT_BYTES` field.

**Tree rule:** when padding or alignment bytes are physically captured and showing them clarifies the serialized structure, represent them explicitly rather than only incrementing the offset.

**Confidence:** High. The change was requested in review and incorporated into the merged implementation.



## Treat display-filter abbreviations as compatibility identifiers, and name fields for their semantic value

Field presentation has two separate naming surfaces: the human-facing label and the display-filter abbreviation used by saved filters, scripts, profiles, command lines, and external tooling. A cleanup that merely aligns wording with a specification can therefore have very different compatibility consequences depending on which surface is changed.

Merged master MR !483 prompted explicit review on this point. Alexis La Goutte raised the risk to external tools, Graham Bloice objected to arbitrary filter renames without a functional reason, and Christopher Maynard suggested aliases as a possible compatibility bridge. João Valverde also clarified that the IPv4/TCP value shown as `Header Length` is a computed byte length, not just the raw IHL/Data Offset bitfield from the RFC diagram; copying the raw field name would misdescribe the value Wireshark actually exposes.

**Compatibility rule:** avoid gratuitous display-filter abbreviation renames, especially in core protocols. If a rename is necessary, consider an alias/migration path where the current API supports one.

**Semantic-label rule:** choose the tree label for the semantic value registered by Wireshark. A decoded or transformed value need not reuse the specification's raw-bitfield name when that would imply different units or semantics.

**Confidence:** Very high. Merged master change with direct review from Gerald Combs, Alexis La Goutte, Graham Bloice, Christopher Maynard, Anders Broman, and João Valverde.
## BASE_SPECIAL_VALS means unmatched values remain numeric, including for 64-bit fields

`BASE_SPECIAL_VALS` is a display policy for tables whose named entries are exceptional values, not a request to label every unmatched number as unknown. A match should use the special descriptive string; an unmatched value should retain its ordinary numeric rendering.

Merged master MR !203, authored by Pascal Quantin, brings `fill_label_number64()` into parity with the existing 32-bit behavior. Before the fix, 64-bit fields with a value table could fall back to the literal string `Unknown`; with `BASE_SPECIAL_VALS`, the accepted implementation uses the table string only when a match exists and otherwise renders the number.

**Field rule:** use `BASE_SPECIAL_VALS` when a value table names special cases while the rest of the numeric domain remains meaningful. Do not convert an ordinary unmatched number into an “Unknown” semantic value merely because the display table has no entry.

**API rule:** when extending field rendering to a new integer width, preserve the established display modifiers and fallback semantics of the corresponding existing-width implementation.

**Confidence:** Very high. Merged master core-proto change authored by Pascal Quantin and accepted for backport.
