# Field Value Semantics Conventions

This file records durable Wireshark conventions for representing protocol field values whose raw encodings have semantic special cases. Current upstream source remains authoritative.

## Keep protocol-specific sentinel meanings attached to the field

A raw value that sometimes represents a normal value and sometimes represents a protocol-specific sentinel such as “unknown”, “now”, or “cancel” should not be encoded as a global display-mode rule when different fields or protocols can assign different meanings to that same raw value. Keep the base field type semantically correct and attach the exceptional interpretation to the field through a value mapping or equivalent field-specific mechanism.

Merged !19694, authored by Guy Harris, extended `FT_ABSOLUTE_TIME` so a field can carry a `time_value_string` table for special raw time values. This supports cases such as Zigbee, where zero can mean “Now” and `0xffffffff` can mean cancellation rather than literal timestamps. In discussion, John Thacker asked whether the older `ABSOLUTE_TIME_NTP_UTC` display mode—where zero globally means unknown/null—should be deprecated in favor of this mechanism. Guy agreed, explicitly noting that different uses can require different strings for the special case. Merged !19704 then applied the mechanism to Zigbee UTCTime fields and added `TIME_VALS` to the API checker.

**Implementation rule:** distinguish the field's underlying data type from protocol-specific sentinel semantics. If identical raw values have context-dependent meanings, represent those meanings in field-local metadata/value tables rather than proliferating special global display modes or hard-coded formatter behavior.

**Confidence:** Extremely high. Merged framework change authored by Guy Harris, direct Guy Harris/John Thacker design discussion, and an immediate merged protocol conversion using the new mechanism.

## Value-string keys for masked fields use the extracted logical value

For an integer field with a nonzero bitmask, Wireshark applies the mask and shift as part of extracting the field value. A `value_string` associated with that field therefore maps the resulting logical value, not the original raw bit pattern at its on-wire bit position. Encoding raw positioned bits in the table can silently make entries unreachable or display the wrong symbolic value.

Merged master MR !14551, authored and merged by Martin Mathieson, audits cases reported by the typed-item checker where `VALS` entries did not fit their field masks. The accepted corrections change values such as `0x20/0x40/0x60` to logical values `0x01/0x02/0x03` and `0x80` to `0x01` for masked fields. The MR also leaves an explicitly documented exceptional LoRa case rather than mechanically rewriting an entry whose surrounding API behavior is unusual.

Earlier merged MR !12607, also by Martin Mathieson, fixes another set of `check_typed_item_calls.py` findings by changing masked-field value tables from wire-positioned constants to the corresponding logical values. It provides independent evidence that these checker warnings identify a real field-registration semantic error rather than a formatting preference.

**Implementation rule:** when registering a masked field with `VALS`, define table keys in the post-mask/post-shift value domain consumed by the field. Review the field type, mask, and value table together rather than treating the table as a representation of raw wire-positioned bits.

**Review rule:** checker warnings that a value does not fit a field mask should normally be treated as evidence of a semantic mismatch, not silenced with casts or a wider field. If an exceptional API path intentionally uses a different value domain, document why rather than relying on an apparently impossible table entry.

**Confidence:** Very high. Repeated merged correctness cleanups by Martin Mathieson, with multiple concrete field corrections and an intentionally documented exception in the later audit.

## Size a field for the value stored in the protocol tree, not only for its wire encoding

A field's on-wire width and its semantic value range can differ when the dissector scales, converts, or otherwise transforms the encoded value before adding it to the protocol tree. The registered `FT_UINT*` width constrains display-filter constants and other typed operations on the tree value, so choosing the type solely from the number of bytes consumed on the wire can make valid displayed values impossible to filter.

Merged master MR !14507, authored by John Thacker and merged by Anders Broman, fixes GTP QoS fields that occupy one octet on the wire but are multiplied by protocol-defined factors before being added with `proto_tree_add_uint[_format_value]`. Values such as a maximum SDU size of 1500 were visible in the tree but the `FT_UINT8` registration caused `dftest` to reject `gtp.qos_max_sdu_size == 1500` as out of range. The accepted change registers the affected fields as `FT_UINT16` and documents why their semantic width exceeds their encoded width.

**Implementation rule:** choose the registered field type from the full range of values that can actually be added to that field after decoding and transformation. Wire length still determines how bytes are read, but it does not by itself determine the correct `hf_` value type.

**Testing rule:** when a compact wire encoding expands to larger logical values, exercise a representative value above the raw encoding's numeric range with `dftest` or an equivalent display-filter test. A tree display that looks correct is not sufficient if the field's type metadata makes that value unfilterable.

**Confidence:** Very high. Merged master correctness fix authored by John Thacker, approved/merged by Anders Broman, with a concrete before/after `dftest` reproducer.

## Checker range validation must use the field's effective masked domain

A typed-item checker that validates a `value_string` against only the storage width of an `FT_UINT*` field can miss impossible symbolic values when the field has a mask. The relevant domain is the extracted logical field value after Wireshark applies the mask and shift. For an ordinary bitmask field, the number of representable logical bits is the number of set bits in the mask, not the width of the underlying storage type.

Merged master MR !12185, authored and merged by Martin Mathieson, first connected `VALS(...)` registrations to their parsed `value_string` tables and warned when the table's maximum value exceeded the registered field width. Its own source comment called out the remaining limitation that the calculation did not yet reduce the width for a mask. Merged master follow-up !12195 immediately fixes that limitation by counting the bits set in the field mask and using that effective width when checking the `value_string` range; the warning also reports the mask so the mismatch is diagnosable.

**Checker rule:** static checks for symbolic-value ranges must model the same value domain that the protocol-tree API exposes. When a mask transforms the stored integer into a smaller logical field, validate the value table against the mask-derived logical width rather than the container type's raw width.

**Review rule:** checker implementation changes should be tested against both unmasked fields and masked fields whose storage width is larger than their semantic width. A checker that understands only the `FT_UINT*` container can produce false negatives precisely where masked-field `VALS` mistakes are most likely.

**Confidence:** Very high. Two consecutive merged master changes by Martin Mathieson establish both the initial range check and the accepted mask-aware correction, and later checker-driven field fixes independently corroborate the semantics.

## Represent encoding-neutral payload as bytes until a lower layer defines text semantics

A payload is not text merely because some common content carried there happens to be printable. If the protocol layer defines the value only as an octet sequence and delegates media/content interpretation to another dissector, register the field as `FT_BYTES` and preserve the raw bytes. Do not invent an ASCII or other character encoding at the transport/application framing layer.

Merged master MR !12156, authored by John Thacker and approved by Anders Broman, changes `http.file_data` from `FT_STRING` to `FT_BYTES`. HTTP message content can be text or arbitrary binary data, and its encoding belongs to the selected media handler/subdissector rather than the HTTP framing code. The accepted change removes `tvb_get_string_enc(..., ENC_ASCII)`, adds the bytes field directly, and updates decryption tests because TShark now exports the field as its byte representation rather than pretending it is an ASCII string.

**Implementation rule:** choose `FT_STRING` only when the owning protocol layer actually specifies text semantics and an encoding contract. For opaque/raw content, use `FT_BYTES`; let a media type, subdissector, or later semantic layer decode text when that layer has enough information to do so correctly.

**Testing rule:** a field-type correction can intentionally change display-filter/export output even when the packet-tree label remains human-friendly. Update tests of `-e`/export behavior to validate the typed representation, not the old incidental formatting.

**Confidence:** Very high. Merged master semantic correction authored by John Thacker, with matching tests and explicit rationale about HTTP content being an encoding-neutral octet stream at this layer.

## Preserve an octet-string field when the specification permits both binary and textual forms

A protocol field can be defined as an octet string while allowing particular encodings inside those octets, such as an APN or domain-name form. The existence of common printable examples does not narrow the wire contract to `FT_STRING`. Retain a bytes-typed representation when arbitrary/non-text octets remain legal, and add human-friendly text presentation only when the actual bytes and specified sub-encoding support it.

Merged master MR !11176 initially proposed displaying the NGAP Common Network Instance as a string. Pascal Quantin cited the governing 3GPP text: the value is an `OctetString` and may contain a network identifier encoded as a domain name or APN, so it is not necessarily a string. He recommended preserving the bytes and using printable-text display support where appropriate; the discussion also notes that APN encoding is not simply the dotted-string form seen in documentation. John Thacker reinforced that protocol-specific APN/DNS handling may require more than generic string rendering.

**Implementation rule:** register according to the protocol's full value domain, not the most common sample. If a bytes field has recognized textual sub-encodings, preserve the raw `FT_BYTES` value and layer the interpretation/presentation on top rather than changing the field to `FT_STRING` and making non-text values unrepresentable.

**Review rule:** when a patch changes `FT_BYTES` to a string type for presentation reasons, check the normative field definition and every allowed encoding first. “Usually printable” is a display observation, not a type guarantee.

**Confidence:** Very high. Merged master change with detailed specification-based review from Pascal Quantin and corroborating encoding discussion from John Thacker.

## Never index a value_string directly with packet data

A `value_string` is a key/value mapping, not a dense array whose indexes are guaranteed to match every possible wire value. Directly using a packet field as the C array index can read beyond the table when the packet value is larger than the set of defined entries, and it also bypasses the table's normal unknown-value behavior.

Merged master MR !10704, authored and merged by John Thacker, fixes Synphasor by replacing direct indexing of `conf_phasor_type` with `val_to_str_const(..., conf_phasor_type, "Unknown")`. The release-branch counterparts !10714 and !10713 had already supplied corroborating evidence and are now anchored by the merged master change.

**Implementation rule:** resolve packet-derived numeric values through the `val_to_str*` / `try_val_to_str*` family (or the appropriate Wireshark mapping helper) and provide an intentional unknown fallback when the protocol permits unrecognized values. Do not treat a `value_string` as a C array indexed by the wire value.

**Review rule:** flag expressions where a packet-derived value indexes a `value_string`, `range_string`, or similar mapping table directly. Check both bounds safety and the semantic behavior for unknown/reserved values.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker, with two previously reviewed stable counterparts.

## Do not use an unmasked Boolean field for a numeric value whose domain is 0 or 1

A full-byte or full-word protocol value that happens to define 0 and 1 is not automatically a bitmask. An FT_BOOLEAN registration with mask zero computes truthiness over the whole value, so unexpected nonzero values (including even values) are silently shown as true. Merged !9336, authored by Martin Mathieson, reverts an earlier over-broad conversion from numeric fields to shared true/false strings across COPS, LOG3GPP, NetFlow, SIMPLE, Soulseek, and TDS. The fix uses FT_UINT8 and explicit VALS maps where a full-byte numeric domain must be represented.

**Rule:** reserve FT_BOOLEAN/TFS for genuine flag/bit semantics with the correct containing width and mask. Use a numeric FT_UINT* with a value_string for a whole on-wire numeric enum, even if currently defined values are 0 and 1; keep unexpected values visible as numeric unknowns. **Confidence:** very high (merged corrective cross-dissector change).

## A two-state-looking numeric field remains numeric when the protocol defines a whole-word enumeration

Merged master !9281 mechanically converted multiple zero-mask numeric value-string fields to `FT_BOOLEAN` with shared `TFS` values. Pascal Quantin explicitly objected that MBIM defines the affected members as full 32-bit numeric fields, **not** single-bit predicates; merged master !9298 restores their `FT_UINT32` and `VALS` registrations. Merged !9336 later reversed other over-broad conversions. **Rule:** follow the wire specification's numeric domain, not just the names of two known values. Preserve distinguishable unknown/reserved numeric values, and audit conversions across every field rather than assuming one mechanically applicable `TFS` type. **Confidence:** Very high: direct Pascal review and accepted corrective master MR (!9281, !9298, !9336).

## Signed lengths may encode a value state rather than a byte count

Merged CQL master !9266 repairs the parser's assumption that a signed 32-bit length can always be used as a byte count or offset increment. In this protocol `-1` means NULL and `-2` means unset; the accepted fix obtains these lengths via `proto_tree_add_item_ret_int`, consumes payload bytes only for positive lengths, and uses a separate unsigned 16-bit length for the short-byte query identifier. Explicit semantic presentation/validation for both negative sentinel values was still marked TODO in that diff, so **do not** claim it was fully implemented. **Rule:** decode signed lengths into a signed type, dispatch special protocol sentinel values *before* using the value as a size or advancing the offset, and distinguish malformed negative values from legal sentinels. **Confidence:** High; merged master fix and release-4.0 backport !9280.

## Display-only masked enums cannot impersonate ordinary value_string filters

During merged USB-PTP port !9230, Martin Mathieson questioned adding a protocol-specific "masked value" variant to shared `proto.h`/`value_string.h` without the supporting generic API and filtering semantics. The contributor confirmed that the historical structure layout only happened to resemble `value_string`; ordinary string-value display filters would compare against the **mask** rather than the actual enumerated numeric value. The contributor favored local `CF_FUNC` formatting while retaining a genuinely numeric filterable value, instead of broadening a broken pseudo-generic table. The large newly added `packet-usb-ptp.c/.h` patches are not retained in this corpus snapshot, so this is **review-discussion guidance**, not a line-by-line validation of the final generated code.

**Rule:** keep the registered field's stored value and its filter comparisons semantically correct. When a protocol needs context/masking-specific display strings, implement a scoped formatter rather than abusing a lookup-table layout that other consumers interpret differently. Do not export a new general-purpose data type without specifying conversion, filters, display, and plugin/API behavior.

**Confidence:** High for Martin Mathieson's direct architectural review and the contributor's stated resolution; limited final-diff visibility.

## Use a typed character field for an ASCII codepoint with named semantics

Merged **!8765** replaces AllJoyn's hand-built header type-character formatter with `FT_CHAR` plus a normal `value_string`. This preserves a single-byte type code as a typed, filterable value and avoids constructing malformed UTF-8 for arbitrary byte values. **Rule:** for a one-byte protocol discriminator, register the correct numeric/character type and a lookup table; do not manufacture a string representation by passing unchecked bytes into UTF-8 labels. **Confidence:** high.

## Generate match-selected literals using the registered display base

Merged **!8764**, approved by **João Valverde**, removes duplicate hand-coded signed/unsigned integer cases from `construct_match_selected_string()` because `fvalue_to_string_repr()` already receives the field's base. The special `BASE_NONE` plus `strings` case remains significant: registered display settings can alter how a selected field is serialized into a filter.

**Rule:** centralize value-to-filter spelling in the typed fvalue API while retaining the distinct field-display/enum cases that affect filter literals. Test generated filters for signedness, base and string/enum-backed values; a visible label is not automatically the appropriate comparison literal. **Confidence:** high for merged change and João's approval.

## Store actual wire values in the protocol tree, then decorate their labels

Merged **!8724** (John Thacker review) replaces IPP's proto_tree_add_string_format(..., NULL, ...) placeholder value with proto_tree_add_item(..., ENC_ASCII) plus proto_item_prepend_text(). The old entry looked informative in the tree but its FT_STRING value was not the packet string, so filtering and JSON exports could lie. John identified the alternative NULL-argument change as wrong in **closed !8719**; the earlier incorrect master change was reverted by merged **!8731** before the typed-field correction.

Merged **!8721** similarly converts AMQP field-table and array members from purely formatted labels into typed, separately registered name, type and value fields so display filters and JSON output can address them. Merged **!8750** and stable backports **!8754/!8755** restore the removed GOOSE goose.floating_point abbreviation as a hidden field while retaining the actual IEEE 754 single-precision value, protecting existing filters.

**Rule:** when a value is known from bytes, register its proper FT_* type and populate the field's actual value; use proto_item_prepend_text()/append_text() for prose. A visually correct tree entry with a dummy field value is not correct for filters, columns, exports or scripts. Preserve a compatible hidden abbreviation when an old, useful filter was renamed or replaced. Closed !8719 is negative evidence, not an accepted fix. **Confidence:** high for merged corrective chain; especially high for John Thacker's explicit semantic objection.
## Do not let a crash workaround turn the presentation label into the field value

Merged **!8711** replaced a NULL value in an IPP proto_tree_add_string_format() call with the same pretty-printed text used in its label. **João Valverde** explicitly objected in post-merge discussion and said not to backport it: preventing a NULL crash does not justify making the stored filterable value a decorated label. Later merged **!8731** reverted the incorrect approach, and **!8724** stored the actual decoded IPP charstring and separately decorated the display label (as detailed above).

**Review rule:** when a tree API takes both a typed field value and a formatting string, verify the value independently with display filters and TShark output. Trace corrective follow-ups before promoting a merged emergency fix into a general convention. **Confidence:** very high for the corrected chain and João's explicit negative review.
