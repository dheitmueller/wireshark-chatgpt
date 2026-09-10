# Wireshark Parser Truncation and Dispatch Conventions

This file records durable parser-control conventions extracted from accepted upstream Wireshark merge requests. Current upstream source remains authoritative.

## Use TVB bounds exceptions for capture truncation instead of pre-clipping normal dissection

For ordinary dissector parsing, do not mechanically replace reported-length-based logic with captured-length checks merely to keep offsets inside the captured bytes. Wireshark's TVB access APIs perform bounds checking and intentionally throw the appropriate exception when a capture is truncated. That exception is part of normal dissection control flow: it lets Wireshark annotate the packet as capture-truncated (or handle fragmentation appropriately) instead of silently treating absent captured bytes as if they were simply not part of the protocol object.

Merged MR !24570 originally proposed broadly changing HTTP form decoding from reported lengths to captured lengths. John Thacker explicitly rejected that rationale: TVB APIs such as `tvb_memcpy()` are expected to throw on truncated data, and Wireshark wants the resulting `BoundsError` path because it automatically communicates capture truncation. The accepted MR was narrowed to the actual percent-decoding bug: a `%xx` escape requires three available bytes, so the local structural check needed to require all three.

**Implementation rule:** distinguish protocol-structure validation from capture truncation. Validate structural requirements such as a complete `%xx` escape where the parser needs them, but otherwise let TVB APIs enforce captured-data bounds and raise the canonical truncation exception. Use captured length explicitly when the protocol semantics or an entry-point/heuristic contract requires it; do not use it merely to avoid exceptions that Wireshark intentionally relies on.

**Confidence:** Very high. The MR merged after explicit John Thacker review correcting the broader captured-vs-reported-length approach.

## Reject impossible parser-progress values before entering repeated parsing

When a packet-controlled field determines how many bytes a parser consumes per iteration, validate protocol-forbidden zero (or otherwise non-progressing) values before entering or continuing the loop. A bounds-safe parser can still hang forever if malformed input makes the iteration consume zero bytes.

Merged master MR !24590, authored and merged by John Thacker and backported as !24596 and !24597, rejects zero address/length components in the UDS `addressAndLengthFormatIdentifier`. ISO 14229-1 disallows those zero values, and accepting them could make the parser loop without advancing.

**Implementation rule:** for repeated/iterative parsing, prove that each successful iteration advances the cursor. If the protocol defines zero as invalid for a length/stride component, reject it before it can become the loop increment rather than relying only on buffer bounds checks.

**Confidence:** Very high. Merged master security/robustness fix by John Thacker with two stable-branch backports.

## Decide whether a substructure exists before calling its decoder

If a message variant does not contain a given nested PDU or structure, make that decision in the layer that knows the message variant, before calling the nested decoder. Do not call the decoder and then add special cases inside it for an input that is semantically not that structure.

During review of merged MR !24560, Stig Bjørlykke explicitly directed that PCC-only DECT NR+ Feedback messages be recognized in `dissect_dect_nr()` before calling `dissect_mac_pdu()`, because those messages do not contain a MAC PDU. The accepted direction keeps the nested decoder's input contract clean instead of teaching it to recognize a caller-level absence case.

**Implementation rule:** dispatch based on the outer protocol's discriminator at the boundary where that discriminator is understood. Only invoke a subdissector/parser when its corresponding structure is actually present; keep absence/variant selection out of the lower-level decoder unless that decoder itself owns the discriminator.

**Confidence:** Very high. Explicit maintainer review by Stig Bjørlykke on a merged master dissector fix.
