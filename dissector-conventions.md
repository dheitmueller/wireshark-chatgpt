# Wireshark Dissector Conventions

This file records conventions that should influence future generated Wireshark code. Treat current upstream source as authoritative and use this file as a cache of verified patterns.

## General implementation rule

Before implementing new functionality, locate several analogous dissectors in the current tree and follow the most modern/common pattern rather than designing APIs or structure from scratch.

## Field semantics

- Do not infer a field's semantic identity from only part of a compound identifier when the protocol meaning depends on multiple values. Example from ANC/ST 2038 work: a semantic type such as ATC is derived from the DID/SDID combination, not SDID alone.
- Display/filter names should reflect the actual protocol semantics and avoid implying a broader or narrower meaning than the specification supports.

## Compiler hygiene

- Wireshark builds commonly treat warnings as errors. Do not leave parsed-but-unused variables such as temporary bitfields unless they are actually consumed.
- When adding parser scaffolding, either expose the parsed value, use it in validation/control flow, or defer parsing until needed.

## Source of truth

Review comments from upstream maintainers should be captured here only when they establish a reusable convention. One-off preferences belong in `review-patterns.md` until corroborated.
