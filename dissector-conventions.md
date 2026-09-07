# Wireshark Dissector Conventions

This file records conventions that should influence future generated Wireshark code. Treat current upstream source as authoritative and use this file as a cache of verified patterns.

## General implementation rule

Before implementing new functionality, locate several analogous dissectors in the current tree and follow the most modern/common pattern rather than designing APIs or structure from scratch.

## Naming

- Prefix internal header-field (`hf_`) and subtree (`ett_`) identifiers with the protocol name. For example, prefer `hf_op47_sdp_identifier` over `hf_sdp_identifier`, and `ett_op47_wst` over `ett_wst`. This was explicitly requested by Anders Broman in MR !26390 as the expected naming pattern and should be applied throughout new dissector code.

## Bit-field parsing

- Prefer Wireshark's tvbuff/protocol-tree bit APIs over local hand-written bit extraction when parsing data already represented by a `tvbuff_t`. In MR !26390, Anders Broman specifically recommended `tvb_get_bits()` or direct tree addition with `proto_add_bits_item` / `proto_add_bits_item_ret_uint` instead of a custom `get_bits_buf()` helper.
- Before adding a parsing helper, search current APIs and analogous dissectors for an existing operation that expresses the same intent.

## Fetch/convert once and reuse

- When a field must both be added to the protocol tree and consumed by parser/control-flow logic, prefer the appropriate `proto_tree_add_item_ret_*` / `proto_add_bits_item_ret_*` API instead of separately calling `tvb_get_*` and then adding the same field.
- MR !26391 systematically replaces pairs such as `tvb_get_uint8()` + `proto_tree_add_item()` with `proto_tree_add_item_ret_uint8()` across multiple dissectors. MR !26367 independently uses `proto_tree_add_item_ret_uint8()` / `_ret_uint16()` while parsing the SACCH Information IE. Treat this as a strong current idiom and search for a `_ret_` helper before double-fetching a field.
- Reuse values that have already been fetched rather than reading the same bytes again later in the same path. In merged MR !25946, Pascal Quantin explicitly called out re-fetching the NMEA sentence ID and suggested using the already available value.
- Before writing local string/byte conversion helpers, search shared utility headers such as `epan/strutil.h`. The same !25946 review specifically pointed to `convert_string_case()` and `convert_string_to_hex()` as existing functionality that should be considered rather than reinvented locally.

## Source-file organization

- One registered protocol does not imply one C source file. Multiple small, closely related protocols/dissectors may appropriately share a source file. Anders Broman explicitly suggested this for the small ST/VANC dissectors in MR !26390.
- Conversely, splitting a genuinely large and expanding protocol family can be justified when it keeps the framing/core layer thin and lets independent command/type sets evolve without creating one enormous file. In merged MR !25763, Alexis La Goutte initially questioned splitting NVMe-MI into multiple dissector files because Wireshark already has many files; the author explained that several large command sets were coming in follow-up MRs, and the split-by-type design was ultimately merged. Treat this as a scale/cohesion judgment, not a one-protocol-one-file rule.
- Choose source-file boundaries for cohesion and maintainability: consolidate tiny related dissectors, but allow a well-justified split when a protocol family is large enough that separate modules materially improve reviewability and future extension.

## Subdissector architecture and dispatch

- Prefer generic extension mechanisms in a generic dissector over hard-coding behavior for one downstream protocol. MR !26376 is refactoring CoAP's Thread-specific special case toward a generic CoAP heuristic payload subdissector list. Because that MR is still open, treat the exact design as provisional, but the separation-of-concerns direction is worth following when looking for analogous code.
- When a protocol has different entry paths with different meanings/types for the dissector `data` pointer, prefer separate dissector entry points that call shared parsing code rather than trying to infer the runtime meaning of `data`. In !26224 Guy Harris rejected an IEEE 802.15.4 fix that distinguished an integer FCS-type pointer from a pseudo-header pointer by identity. His merged replacement !26229 uses separate dissectors for the two call contracts and common code underneath. Treat this pattern as very high confidence.
- Do not register a protocol directly on an unassigned/dynamic TCP/UDP port merely because an implementation commonly uses it. John Thacker explicitly rejected fixed UDP 61631 registration in MR !26376 and directed the contributor to Decode As. A robust heuristic may be appropriate, but fixed-port registration should correspond to actual protocol assignment/semantics.
- Keep protocol-specific lookup/dependency logic in the consuming protocol when an existing lookup API suffices. In MR !26374 Anders Broman requested that RADIUS use `proto_get_id_by_short_name()` locally rather than modifying generic TLS and DTLS code to expose RADIUS-specific protocol identity.
- It is valid to add a field to the tree and immediately mark it hidden with `proto_item_set_hidden()` when the value should remain display-filterable but another subdissector owns the visible presentation. MR !26393 uses this pattern for eCPRI PC ID/sequence fields when the O-RAN FH dissector claims the PDU.
- Prefer existing dissector tables for protocol-layer dispatch rather than manually selecting a specific downstream dissector when a registered protocol discriminator already exists. Merged MR !26218's GUE dissector hands Variant 0 payloads through the `ip.proto` table, allowing normal nested protocol chains.
- A subdissector should not be considered to have successfully claimed payload merely because an opcode/type suggests it *could* decode it. Merged MR !26223 corrected NVMe-MI code that hid the raw-byte item when no structured decoder actually rendered the data. Base the handled/claimed result on successful structured dissection so undecoded payload remains visible.
- For stateful subdissectors, choose conversation identity from fields that are stable for the lifetime needed by the state machine, not merely the nearest logical protocol identifier. Merged MR !25764 propagates stable SMBus physical source/destination addresses through `pinfo->net_src/net_dst` because MCTP EIDs are unassigned or direction-dependent during discovery; NVMe-MI can then keep devices separate in a multi-device topology. Treat endpoint identity as a protocol-semantics decision, especially before discovery/assignment is complete.

## Truncation, verification, and malformed data

- Distinguish **unverifiable** from **invalid** when a capture is missing bytes needed to compute a checksum/MIC or other verdict. Merged MR !26223 changed NVMe-MI MIC handling so a sliced frame reports the MIC as unverified instead of asserting a pass/fail verdict that the capture cannot support.
- Treat protocol-declared lengths and captured lengths according to their actual semantics. In !26223 a short Security Send represents a partial transfer and is deliberately left as bytes, while a Security Receive can legitimately be shorter than its Allocation Length because that value is an upper bound. Do not mechanically equate `captured < advertised` with malformed without checking the protocol contract.
- When a dissector can be invoked directly (for example via Decode As), do not assume a heuristic caller's captured-length guard has already run. Merged MR !25764 adds an explicit `tvb_captured_length()` minimum check in the main MCTP-over-SMBus dissector as well as relying on reported length, because a snaplen-truncated frame can report a large packet while containing too few captured bytes for fixed-header reads. Entry points must independently enforce the preconditions they require.
- Explicitly reject structurally impossible length relationships before subtracting or deriving trailing lengths. !26223 added a minimum-length check for NVMe/TCP Termination Request PDUs because `tcp_dissect_pdus()` only guaranteed the common-header minimum, not that the protocol-specific header fit.

## Reassembly and length arithmetic

- Treat lengths derived from packets, fragment accumulation, and offsets as hostile arithmetic. Check for overflow before allocation/copy and ensure a fragment copy is clipped to remaining capacity.
- MR !26365 uses `ckd_add()` before growing a manually reassembled buffer and emits expert information when the resulting PDU would exceed `INT32_MAX`. MR !26382 independently avoids `cur_off + len` overflow by subtracting first (`avail = tot_len - cur_off`) after proving `tot_len > cur_off`, then clipping `len` to `avail`.
- Merged DICOM MRs !26208/!26215 reinforce this pattern outside packet reassembly: use a size-appropriate unsigned type, `ckd_add()`, and a realistic explicit upper bound that is valid on both 32-bit and 64-bit builds rather than relying on unchecked accumulation or a theoretical maximum.
- Reassembly keys must include every field that participates in the protocol's message identity. !26223 fixed MCTP reassembly collisions by including the tag-owner bit together with the message tag; request and response directions can legally reuse the same numeric tag.
- A `reassembly_table` must have a valid initialization/registration path, not merely a declaration and uses. Merged MR !25984 fixed a Bluetooth BR/EDR table that was used without being registered/initialized; maintainers explicitly discussed adding automated checking for this class of defect. Include reassembly-table registration in code review/pre-submit checks.
- Prefer the standard Wireshark reassembly API when it fits. The existing manual AVCTP reassembly code touched by !26365 explicitly notes that it should eventually be replaced by the standard API with custom key functions.

## Field semantics and compatibility

- Do not infer a field's semantic identity from only part of a compound identifier when the protocol meaning depends on multiple values. Example from ANC/ST 2038 work: a semantic type such as ATC is derived from the DID/SDID combination, not SDID alone.
- Display/filter names should reflect the actual protocol semantics and avoid implying a broader or narrower meaning than the specification supports.
- When correcting an `hf_` type, mask, or filter name, explicitly consider saved display filters and coloring rules as part of compatibility. MR !26223 documented removed fields, changed field types, and mask changes, including cases where a formerly Boolean filter expression would silently stop matching after the field became the correct multi-bit enum. Correct protocol semantics take priority, but user-visible filter breakage should be identified rather than accidental.

## Compiler hygiene

- Wireshark builds commonly treat warnings as errors. Do not leave parsed-but-unused variables such as temporary bitfields unless they are actually consumed.
- When adding parser scaffolding, either expose the parsed value, use it in validation/control flow, or defer parsing until needed.

## Source of truth

Review comments from upstream maintainers should be captured here only when they establish a reusable convention. One-off preferences belong in `review-patterns.md` until corroborated.
