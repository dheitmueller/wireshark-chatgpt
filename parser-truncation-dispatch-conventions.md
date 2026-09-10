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

## Localize failure from optional nested decoding without hiding true outer truncation

An optional/speculative nested decoder is a different exception boundary from ordinary parsing of the enclosing protocol. If the outer protocol can validly carry opaque bytes (for example encrypted user data) that happen to match a discovered type, bound the child TVB to the exact payload region and allow a child decode failure to remain local rather than misclassifying the entire outer packet as malformed.

Merged master MR !24361 fixes RTPS user-data dissection where encrypted payload bytes were being sent to a discovered CDR decoder. The accepted implementation passes an explicit payload size, uses `tvb_new_subset_length()` so the child cannot read into following RTPS submessages, and catches nonfatal errors from the optional child to report a warning instead of a top-level malformed RTPS packet. The supplied validation capture showed zero `_ws.malformed` matches after the fix while preserving successful decoding of unencrypted traffic and of the following SRTPS_POSTFIX.

**Implementation rule:** keep normal TVB truncation exceptions for bytes that the enclosing protocol actually requires, but establish a bounded child TVB and a deliberate local failure boundary when dispatching bytes that may validly be opaque or undecodable. Do not use a broad exception catch to suppress genuine parent-protocol truncation.

**Confidence:** High. Merged master fix approved by Anders Broman with focused before/after capture validation; the scope is deliberately narrow so it does not contradict the ordinary-truncation rule above.

## Reinitialize per-record parser mode before each repeated record

Parser state whose meaning is local to one record/parameter must be restored to its default at the start of every iteration. A special record that changes length width, encoding mode, or another local interpretation must not silently alter how the next record is parsed.

Merged master MR !24394 fixes RTPS parameter parsing by resetting `param_length_length` to its normal two-byte width on every parameter iteration. `PID_EXTENDED` temporarily selects a four-byte length field; allowing that value to persist caused a later `PID_SENTINEL` to be missed. The same MR fixes multi-fragment DATA_FRAG reassembly by advancing the source offset for each fragment rather than repeatedly copying the first fragment's bytes, and includes a capture identifying concrete multi-frame reassemblies used to validate the result.

**Implementation rule:** initialize record-local parser mode inside the repeated-record loop, not merely before it. Likewise, when one container carries multiple fragments/elements, derive each element's source position from its index/consumed length rather than reusing the container's initial payload offset.

**Confidence:** High. Merged master correctness fix approved by Jaap Keuter with focused capture validation.