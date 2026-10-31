# Wireshark Protocol Field Semantic Conventions

This file records durable conventions for choosing protocol-field representations and using field-return helper APIs. Current upstream source remains authoritative.

## Model filterable protocol fields according to their semantic value

When the protocol defines a value as text, register and expose it as an appropriate string field when users would naturally filter on the textual value. Do not split a string into separately filterable byte fields merely because byte-by-byte extraction is convenient for implementation.

Merged MR !24258 added CIP Identity Object attributes. During review, Michael Mann challenged an implementation that made individual bytes of a language string filterable instead of the language value itself. The merged revision reflects the semantic representation. This is consistent with Wireshark's general goal that display filters describe protocol concepts rather than parser implementation artifacts.

**Implementation rule:** choose `hf_` field type, encoding, and granularity from the protocol concept users inspect and filter on. Preserve raw bytes separately only when they carry independently meaningful protocol semantics.

**Confidence:** High. Direct maintainer review on a merged master MR, with the objection specifically framed in display-filter semantics.

## Use string field types that match the bytes actually present on the wire

`FT_STRINGZ` means that the protocol field is represented as a NUL-terminated string in the packet; it should not be used merely because the implementation happens to construct a C NUL-terminated string after extracting a bounded field. If the packet supplies an explicit length, use an ordinary string representation. If the protocol's termination semantics are truncated or padded, use the corresponding `FT_STRINGZTRUNC` or `FT_STRINGZPAD` representation rather than pretending an absent terminator is present.

Merged MR !22834, authored and merged by John Thacker, corrects HTTP form URL-encoded keys and values from `FT_STRINGZ` to `FT_STRING`: those values are length-bounded and are not NUL-terminated on the wire. The MR's rationale explicitly distinguishes real terminated strings from fields whose absence/truncation must be determined by some other protocol mechanism.

**Implementation rule:** select `FT_STRING`, `FT_STRINGZ`, `FT_STRINGZTRUNC`, or `FT_STRINGZPAD` from the field's wire-format contract, not from the representation returned by a helper API. A bounded non-NUL-terminated packet field is not `FT_STRINGZ` even if its extracted host string is terminated for convenience.

**Confidence:** Very high. Merged master semantic cleanup authored and merged by John Thacker with explicit rationale about the field-type contract.

## A returned value from a masked field is the field value, not the untouched storage unit

Do not assume that a `proto_tree_add_*_ret_*()` call using an `hf_` entry with a bitmask returns the original unmasked byte/word suitable for manually extracting other sibling bitfields. The field registration and helper semantics apply the field mask to obtain that field's value.

Merged MR !24251 fixes a Bluetooth HID regression in which the one-byte header contains two 4-bit fields. Code read the header through the masked transaction-type field and then attempted to derive both transaction type and parameter from the returned value. Because the lower bits had already been excluded by the field semantics, every packet could be misclassified. Martin Mathieson reviewed the fix and proposed the accepted minimal approach: obtain each semantic field correctly rather than treating the masked field result as a raw header container.

**Implementation rule:** if later logic needs the original storage unit, fetch it explicitly as raw data or through an unmasked representation. If it needs multiple semantic bitfields, extract each according to its own field definition. Do not reuse the value returned for one masked `hf_` field as source material for another.

**Confidence:** Very high. Merged correctness fix with direct review from Martin Mathieson addressing the exact masking mistake.

## Prefer meaningful visible tree items over hidden fields created only for filtering

When a protocol concept is useful enough to deserve a display filter, first ask whether that concept should also be represented naturally in the protocol tree. Do not create hidden `hf_` fields solely as filter hooks when a real packet concept can be modeled as a visible item or subtree without cluttering the dissection.

Merged MR !22758 initially proposed hidden fields for each NATS operation so users could filter on operations such as `nats.pub` and `nats.msg`. John Thacker explicitly rejected the hidden-field approach and suggested representing each operation as an `FT_NONE` item/subtree beneath one NATS protocol item. The contributor reworked the MR accordingly, attached a sample capture, showed before/after tree presentation, fixed a payload-length issue discovered during the revision, and John approved the resulting implementation. The final diff adds operation-specific `hf_` items at the actual operation PDU ranges and uses them as the operation subtrees rather than invisible filter-only markers.

**Implementation rule:** use hidden fields only when there is a real need for information to be filterable but not visible. If the field names a meaningful structural event, operation, message type, or other protocol concept, model that concept in the visible tree—often with `FT_NONE` when presence itself is the value—and let filtering follow from that representation.

**Confidence:** Very high. Direct design objection and replacement architecture from John Thacker on a merged master MR, followed by an updated sample capture/tree presentation and explicit approval.

## Do not use text-only, unfilterable tree helpers in dissectors

Dissector output should normally be represented by registered, filterable protocol fields, real subtrees, or expert information. `proto_tree_add_text_internal()` is retained for a small number of internal epan uses, but it is not a dissector API: it creates text that has no registered field and therefore cannot participate in display filtering.

Merged master MR !22642, authored by Michael Mann and merged after review by John Thacker and approval by Anders Broman, removes the remaining dissector uses of `proto_tree_add_text_internal()`. The replacements illustrate the intended choices: use `proto_tree_add_subtree()` for structural presentation, registered `hf_` fields for protocol data, and expert fields for diagnostic/status text such as an absent parameter condition.

**Implementation rule:** do not reach for an unfilterable free-form text item when writing a dissector. Model packet concepts using registered fields/subtrees; model noteworthy parser or protocol conditions using expert information. Free-form internal tree helpers are not a shortcut around protocol-field design.

**Confidence:** Very high. Explicit API-boundary statement in a merged master MR from Michael Mann, with direct maintainer review and a multi-dissector cleanup implementing the rule.

## Distinct filter semantics require distinct protocol fields

Do not reuse one `hf_` field merely because two values share a numeric representation or are related internally. A display-filter field is a user-visible semantic identity: reusing it across conceptually different contexts makes a filter for one concept unexpectedly match the other.

Merged master MR !21856, authored by John Thacker and merged by Michael Mann, splits MEGACO Context into separate fields for the packet's actual Context and a generated related-call-leg Context. The MR explicitly explains that users may want to filter frames for one use case without also matching the other, even though both values represent context identifiers.

**Implementation rule:** define separate `hf_` entries when two occurrences have different filtering meaning, provenance, or user intent, even if they share type and values. A generated correlation/helper field should not silently broaden the semantics of the field representing the value actually carried by that protocol location.

**Confidence:** Very high. Merged master semantic-field correction authored by John Thacker and merged by Michael Mann, with the display-filter consequence stated explicitly.

## Keep wire span, extraction semantics, field type, and formatting mutually consistent

A protocol-tree call has several representations of the same packet concept: the number of bytes selected from the tvbuff, the width/signedness used to extract the value, the registered `hf_` type, and any explicit display format. They must describe one compatible semantic value. A mismatch can silently truncate, reinterpret signedness, or make the displayed value disagree with filtering.

Merged master MR !21494 fixes a NOE field for which the existing call claimed a one-byte span, extracted two bytes as unsigned, formatted the result as signed with `%d`, and used an `hf_` field whose type expected four bytes. The accepted cleanup uses `proto_tree_add_item()` so extraction and presentation follow the registered field semantics. During review, Martin Mathieson explained an important nuance in Wireshark's checker: selecting *more* bytes than the field width is suspicious because it can truncate, while selecting fewer bytes can be legitimate when a protocol field was widened in a later version but an older version still carries a narrower encoding.

**Implementation rule:** audit the whole field contract together: tvbuff span, endianness/encoding, extracted C domain, `hf_` width/signedness, mask, and format string. Prefer `proto_tree_add_item()` or another semantic helper when it can perform the wire extraction directly. Do not demand byte-count equality mechanically when a protocol version legitimately encodes the same semantic field more narrowly, but never rely on a wider extraction being silently narrowed by the field definition.

**Confidence:** Very high. Merged master correctness fix plus direct Martin Mathieson explanation of the checker boundary and Anders Broman review of the accepted simpler API.

## Normalize unifying filter fields across equivalent wire representations

A display-filter field deliberately introduced to unify multiple directions, encodings, or protocol variants should expose one normalized semantic value domain. Do not populate such a field with one branch's raw codepoint when another branch expresses the same logical identifier differently; that defeats the purpose of the unified filter.

Merged master MR !16118 adds a single hidden MAC-LTE LCID field intended to let users filter a logical channel regardless of direction. Pascal Quantin caught that the first implementation did not cover extended LCIDs. The review then distinguished the raw extended-LCID codepoint from the logical extended-LCID index and concluded that the unifying field should carry the same semantic index across the ordinary and extended cases. Martin Mathieson revised the implementation accordingly before merge.

**Implementation rule:** when one `hf_` field intentionally provides a common filter over several wire forms, normalize every form to the same protocol-level identifier before adding that field. Keep raw/codepoint-specific fields separately when those representations are independently useful, but do not leak representation-specific numbering into the normalized filter contract.

**Confidence:** Very high. Merged master change with explicit protocol-expert review from Pascal Quantin and an accepted revision by Martin Mathieson addressing the semantic mismatch.

## Preserve numeric filter semantics even when the display is formatted with units

A value that happens to be *displayed* as a string is not thereby a text field. Register measurements using a numeric `FT_*` type so display filters can compare magnitude, and attach unit presentation independently.

In TECMP review !9882, Gilbert Ramirez noticed voltage was registered as `FT_STRING` and explicitly proposed `FT_DOUBLE` so users could filter greater/less-than values. The follow-up **merged master MR !9887** implements that recommendation: a packed two-byte voltage is converted into a double and added with `proto_tree_add_double`, with a unit-formatted field registration. Older stable-branch !9882/!9879 fixes only the zero-padded string display, so they are weaker evidence for the accepted semantic type direction.

**Implementation rule:** model a voltage, rate, temperature, duration, or other measurement with an appropriate numeric field and use display presentation for its suffix/units. Do not make a measurement nonfilterable just to append text to the label.

**Confidence:** Very high. Explicit Gilbert Ramirez review followed by the merged substantive correction.

## State-derived fields must not claim current-packet bytes they did not decode

Merged master MR !9846, authored by Guy Harris, corrects Gryphon IOCTL response presentation. The IOCTL code comes from the earlier *request*, so highlighting a byte in the current response falsely attributes it to response wire data. The accepted call uses `proto_tree_add_uint(..., tvb, 0, 0, saved_ioctl_code)` and registers the field as `FT_UINT32` rather than `FT_UINT8`, matching the 32-bit semantic value. Release backports !9861 and !9862 corroborate the accepted master behavior.

**Implementation rule:** for a computed or saved state value, use a generated/zero-byte current-packet span when it has no genuine backing bytes in that frame; do not manufacture a source range. Keep the registered `FT_*` width equal to the actual semantic-value domain rather than an incidental displayed byte count.

**Confidence:** Extremely high: Guy Harris's merged master correction and two accepted backports.

## Name Boolean filters so both presence and negation have clear semantics

In merged SMPP MR !9732, Gilbert Ramirez questioned `smpp.command_type` as an FT_BOOLEAN: neither `smpp.command_type` nor `!smpp.command_type` makes request/response meaning obvious. He suggested a predicate such as `smpp.is_command` or an enum. The accepted revision instead adds separate, visible, generated `smpp.request` and `smpp.response` presence fields.

**Rule:** Choose meaningful Boolean predicates or category-presence fields; reserve a `*_type` identifier for a multi-valued semantic classification. **Confidence:** Very high; explicit Gilbert review implemented in merged code.

## Frame-number fields should encode the direction of the relationship

In merged !9544, John Thacker adds `FRAMENUM_TYPE(FT_FRAMENUM_RESPONSE)` to GTP/GTPv2 `response_in` fields and `FRAMENUM_TYPE(FT_FRAMENUM_REQUEST)` to `response_to` fields. These retain `FT_FRAMENUM` values but tell Wireshark's presentation layer which kind of related frame is referenced, enabling the appropriate navigation symbols/arrows.

**Implementation rule:** For cross-packet reference fields, register the semantic relationship as well as the frame number; check request and response arrows in the packet tree. **Confidence:** Very high (merged John Thacker change).

## Attach generated-packet diagnostics to the item that actually exists

Merged CIP analysis MR !9542 (Dylan Ulis) builds generated connection-information subtrees and displays matching Forward Close frames or an expert diagnostic when no close was observed. Gilbert Ramirez specifically caught a call-site issue: an expert annotation should be made only in the branch that created the corresponding `proto_item *pi`, not unconditionally outside it; the author fixed this before merge.

**Review rule:** A generated field, stored cross-packet state, and its expert indicator have distinct lifetimes and source ranges. Attach expert information to the precise existing item that represents the condition, and guard optional fields instead of reusing a stale/null item pointer. **Confidence:** High (merged implementation with explicit Gilbert Ramirez correction).

## Two-value enumerations are not automatically Boolean protocol fields

Merged MP4 segment-index support !9423 uses a value-string for an on-wire reference type whose defined values are Movie and Index. Anders Broman asked whether this should be an `FT_BOOLEAN` with a true/false string; the contributor argued these are named protocol variants rather than truth conditions, and the merged diff retained the enum presentation. This is not an authoritative prohibition on Boolean fields, but it shows that the **meaning** of a field matters more than cardinality alone.

**Review rule:** prefer `FT_BOOLEAN` when a protocol actually defines a predicate, and a typed numeric/enum field when a binary-valued discriminator identifies distinct protocol cases. Check the wire specification and filtering semantics before changing representations. **Confidence:** moderate, explicit review question and merged semantic choice.

Merged IEEE 802.11 !9432 supplies a stronger complementary API rule: short-SSID *integer* fields needed `ENC_LITTLE_ENDIAN`, while Alexis La Goutte rejected mistakenly assigning that integer encoding to a separately registered `FT_BOOLEAN` field and requested the appropriate `ENC_ASCII` at that call site. Correct comparable occurrences together, but audit **each field's registered type** before mass-applying an encoding fix. Alexis also requested a commit subject shorter than 80 characters. **Confidence:** very high, explicit maintainer corrections in a merged MR.

Merged BGP-LS fix !9422/!9421 compares an IPv6-address TLV's payload length against the **length constant**, not the similarly named TLV **type ID** (which had yielded an absurd expected size of 262 bytes). **Rule:** distinguish type discriminators from size constants in validation and expert error text. **Confidence:** high, merged stable-branch corrections.

## Ensure declared bitmasks can represent the complete enumerated value domain

Merged DECT-NWK Coverity fix !9190 widens the extended-location type mask from `0xE0` to `0xF0` and adjusts its shift from five to four. Martin Mathieson explicitly pointed out that a three-bit masked value cannot represent the defined enumeration value `0x0F`; matching named `value_string` entries are not enough if the mask makes a branch unreachable. The same fix widens the arithmetic domain before multiplying an IPEI component, avoiding an intermediate overflow.

**Field rule:** reconcile mask width, bit shift, registered field type, every declared enum member, and the arithmetic type used to construct a display value. Code generation and static checking cannot replace validating the entire protocol value domain. **Confidence:** Very high, explicit Martin review resolved by a merged Coverity fix.

## Keep custom-column resolution controls synchronized with field display semantics

Merged John Thacker master !9201 brings `FT_IPv4`, `FT_IPv6` and `FT_FCWWN` custom-field presentation through the shared `address_to_display()` name-resolution path. Its UI changes enable the custom-column resolution toggle for the newly resolvable field types and keep preference helper logic consistent with the runtime formatter.

**Presentation rule:** a field's displayed label, custom-column value, optional name resolution, and UI capability flags must agree. Changing the formatting API alone is insufficient if users cannot select whether they want the resolved or numeric representation. Preserve distinct machine/filter values. **Confidence:** Very high; merged John Thacker cross-layer correction.

## Separate protocol string values from escaped/display-only labels

Merged master **!8990** clarifies `proto_tree_add_string()`'s API documentation: the passed UTF-8 string is the packet-derived **field value**, not a preformatted display label. Embedding `format_text()`-style escaping or markup in the value would change display-filter matching, obscure whitespace semantics and reduce the UI's freedom to render it appropriately. Related merged **!8969** converts an ICMPv6 DNS search-name representation to valid UTF-8 when adding it as a string, recognizing that raw DNS names do not prescribe a general text encoding.

**Rule:** decide wire-byte decoding, value storage, and UI escaping separately. A string-valued field's searchable semantic value must not be overwritten with escaped or emphasized presentation text. Encode or normalize for the field contract, preserve original wire span where feasible, and leave label formatting to the presentation path.

**Confidence:** Very high for the merged API-comment clarification; !8969 is corroborating protocol-specific behavior, not a universal DNS wire-encoding declaration.

## Represent textual identifiers as decoded protocol strings

Merged **!8812** changes an IEEE 802.11 Hotspot 2.0 OSU NAI from `FT_BYTES` with `ENC_NA` to `FT_STRING` with `ENC_ASCII`. The registered field type and encoding affect filtering and machine exports as well as visible tree labels. When a protocol defines text, register a string with the actual encoding rather than retaining opaque bytes simply because packets carry octets. **Confidence:** high, merged narrow corrective diff.
