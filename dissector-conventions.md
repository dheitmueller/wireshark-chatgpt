# Wireshark Dissector Conventions

This file records conventions that should influence future generated Wireshark code. Treat current upstream source as authoritative and use this file as a cache of verified patterns.

## General implementation rule

Before implementing new functionality, locate several analogous dissectors in the current tree and follow the most modern/common pattern rather than designing APIs or structure from scratch.

## Naming

- Prefix internal header-field (`hf_`) and subtree (`ett_`) identifiers with the protocol name. For example, prefer `hf_op47_sdp_identifier` over `hf_sdp_identifier`, and `ett_op47_wst` over `ett_wst`. This was explicitly requested by Anders Broman in MR !26390 as the expected naming pattern and should be applied throughout new dissector code.

## Bit-field parsing

- Prefer Wireshark's tvbuff/protocol-tree bit APIs over local hand-written bit extraction when parsing data already represented by a `tvbuff_t`. In MR !26390, Anders Broman specifically recommended `tvb_get_bits()` or direct tree addition with `proto_add_bits_item` / `proto_add_bits_item_ret_uint` instead of a custom `get_bits_buf()` helper.
- Before adding a parsing helper, search current APIs and analogous dissectors for an existing operation that expresses the same intent.

## Source-file organization

- One registered protocol does not imply one C source file. Multiple small, closely related protocols/dissectors may appropriately share a source file. Anders Broman explicitly suggested this for the small ST/VANC dissectors in MR !26390.
- Choose source-file boundaries for cohesion and maintainability, not mechanically by protocol-registration count.

## Field semantics

- Do not infer a field's semantic identity from only part of a compound identifier when the protocol meaning depends on multiple values. Example from ANC/ST 2038 work: a semantic type such as ATC is derived from the DID/SDID combination, not SDID alone.
- Display/filter names should reflect the actual protocol semantics and avoid implying a broader or narrower meaning than the specification supports.

## Compiler hygiene

- Wireshark builds commonly treat warnings as errors. Do not leave parsed-but-unused variables such as temporary bitfields unless they are actually consumed.
- When adding parser scaffolding, either expose the parsed value, use it in validation/control flow, or defer parsing until needed.

## Source of truth

Review comments from upstream maintainers should be captured here only when they establish a reusable convention. One-off preferences belong in `review-patterns.md` until corroborated.
