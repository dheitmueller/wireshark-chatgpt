# Wireshark Architecture Notes

This is a curated cache of architectural understanding relevant to future development. It is intentionally incomplete; add material when it becomes useful for actual work.

## Operating rule

When changing a subsystem, trace the real call path in the current source before designing the patch. Prefer existing registration, dispatch, reassembly, conversation, and dissector-table mechanisms over adding parallel infrastructure.

## Areas of particular interest

- `epan/` dissector APIs and protocol-tree construction.
- RTP payload dispatch and media-over-IP dissectors.
- MPEG PES payload dispatch.
- Ancillary-data parsing and code reuse between ST 2110-40 / ST 2038 / related SMPTE transports.
- Test and fuzz target integration.

## Provenance

Architectural claims should ultimately cite current upstream files, commits, or review discussions in prose so stale assumptions are easier to detect and revalidate.
