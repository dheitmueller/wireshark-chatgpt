# Wireshark Protocol Field Semantic Conventions

This file records durable conventions for choosing protocol-field representations and using field-return helper APIs. Current upstream source remains authoritative.

## Model filterable protocol fields according to their semantic value

When the protocol defines a value as text, register and expose it as an appropriate string field when users would naturally filter on the textual value. Do not split a string into separately filterable byte fields merely because byte-by-byte extraction is convenient for implementation.

Merged MR !24258 added CIP Identity Object attributes. During review, Michael Mann challenged an implementation that made individual bytes of a language string filterable instead of the language value itself. The merged revision reflects the semantic representation. This is consistent with Wireshark's general goal that display filters describe protocol concepts rather than parser implementation artifacts.

**Implementation rule:** choose `hf_` field type, encoding, and granularity from the protocol concept users inspect and filter on. Preserve raw bytes separately only when they carry independently meaningful protocol semantics.

**Confidence:** High. Direct maintainer review on a merged master MR, with the objection specifically framed in display-filter semantics.

## A returned value from a masked field is the field value, not the untouched storage unit

Do not assume that a `proto_tree_add_*_ret_*()` call using an `hf_` entry with a bitmask returns the original unmasked byte/word suitable for manually extracting other sibling bitfields. The field registration and helper semantics apply the field mask to obtain that field's value.

Merged MR !24251 fixes a Bluetooth HID regression in which the one-byte header contains two 4-bit fields. Code read the header through the masked transaction-type field and then attempted to derive both transaction type and parameter from the returned value. Because the lower bits had already been excluded by the field semantics, every packet could be misclassified. Martin Mathieson reviewed the fix and proposed the accepted minimal approach: obtain each semantic field correctly rather than treating the masked field result as a raw header container.

**Implementation rule:** if later logic needs the original storage unit, fetch it explicitly as raw data or through an unmasked representation. If it needs multiple semantic bitfields, extract each according to its own field definition. Do not reuse the value returned for one masked `hf_` field as source material for another.

**Confidence:** Very high. Merged correctness fix with direct review from Martin Mathieson addressing the exact masking mistake.
