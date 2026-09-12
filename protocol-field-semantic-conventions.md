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