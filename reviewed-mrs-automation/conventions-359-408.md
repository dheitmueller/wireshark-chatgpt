# Conventions 359-408

This file records durable review lessons from the !359 through !408 batch.

- Reassembly overlap semantics apply even when a duplicate or contained fragment contributes no new bytes; distinguish ordinary overlap from conflicting overlap and cover both in focused tests (!403).
- Persistent multi-pass dissector state should normally be populated on the first pass, at the point each value becomes known, so later passes or continuations cannot erase valid earlier state (Guy Harris !367).
- Extraction and generation tools should work end-to-end from documented inputs rather than depending on unrecorded manual edits; test the actual generated result (!397 corrected by !398).
- When a checked-in generated dissector artifact has a source/template change, regenerate the derived file in the same logical change (!366).
- Put a defined limit around packet-driven decompression expansion and report ordinary bad-input failures through packet diagnostics rather than treating them as internal program failures (!381).
- Resolve dead-store and dead-increment findings against parser semantics; related helpers should use one clear cursor return convention rather than mixing absolute offsets and consumed lengths (!369, !370).
- Packet-tree source and bit geometry is consumed by UI features. The later !959 convention is stronger than !360: prefer truthful field metadata and generic derivation over manual geometry overrides when the normal field contract can express the layout.
- During protocol codepoint transitions, compatibility decoding for observed legacy traffic can be retained while clearly labeling the historical assignment and keeping the current standardized value authoritative (!364).
- For packet-diagram-style views, normalize overlaps, gaps, and collapsed extents before rendering and apply the resulting coordinate transform consistently to later items (!408).
