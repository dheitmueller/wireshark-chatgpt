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