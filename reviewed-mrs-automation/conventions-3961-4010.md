# Durable conventions from MRs !3961–!4010

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Explicit allocator scope is part of the API contract

Merged !4008 changes TVB helpers that return allocated strings to take an explicit `wmem_allocator_t *` rather than silently using `wmem_packet_scope()`. Helpers needing only scratch storage can allocate explicitly and free after bounds-sensitive operations complete. Guy Harris also required renaming `tvb_bcd_dig_to_wmem_packet_str*`: once packet scope is no longer intrinsic, the historical `packet` name is misleading.

**Rule:** make ownership/lifetime explicit in APIs that return allocated data, use the caller's appropriate scope, and rename/document APIs when a former lifetime assumption stops being true.

## pcapng extension code should share common framing/option machinery

Merged Guy Harris MRs !3994 and !3996 expose common option-section processors and centralize raw integer extraction, alignment-safe copying, and byte-order conversion. The API distinguishes options following section byte order from explicit big- or little-endian extension payloads.

In merged !3990, Guy's BBLog review also establishes two boundaries: Wireshark should point maintainers at a defining document/source for private formats, and standardized pcapng envelope fields such as block/option headers and PENs must follow the pcapng contract even when opaque vendor payload bytes use their own fixed byte order. Guy preferred common custom-block processing and anticipated callback/registration-driven growth rather than vendor-specific core branches.

**Rule:** keep container/framing semantics in common pcapng infrastructure, make byte-order domains explicit, and let extensions own private payload semantics behind generic registration/callback interfaces. For undocumented/private formats, cite the best available authoritative source.

## Preserve semantic filenames until the output/storage boundary

Merged master !3993, with release backports !4000 and !4001, removes SMB-specific ASCII canonicalization of Export Object names. The export subsystem already has the centralized filename-safety function used when an object is actually saved, so early protocol-specific canonicalization only loses valid Unicode and duplicates policy.

**Rule:** keep protocol/display strings semantically faithful while they are data. Apply filesystem-safety transformation once, at the component that actually creates the file.

## Nested dissectors must preserve caller-owned TCP desegmentation state

Merged !3977 restores `pinfo->can_desegment` from `pinfo->saved_can_desegment` before AMQP's nested version dispatch; otherwise a second decrement prevents the surrounding `tcp_dissect_pdus()` from reassembling split PDUs. Later stable !4012/!4013 carry the same fix.

**Rule:** shared `packet_info` reassembly fields are call-chain state. Save/restore the inherited value across nested dispatch instead of consuming it as private state.

## Conversation identity follows the semantic layer that owns state

Merged !3976 adds a distinct iWARP MPA port/endpoint domain because nested RPC dissection otherwise overwrote the outer MPA TCP conversation. RPC still needs connection-oriented semantics, but it must not reuse an identity slot whose ownership belongs to the encapsulating transport-like layer.

**Rule:** when an encapsulation layer and a nested stateful protocol each need conversations, give each the identity domain that models its own state.

## Registered field type determines valid encoding semantics

During merged !4002, Anders Broman required the aggregate Bluetooth LE data-header item to use `ENC_NA`, not `ENC_LITTLE_ENDIAN`; the aggregate item became `FT_NONE`, while scalar subfields retained appropriate encodings.

**Rule:** choose `ENC_*` from the registered field type and bytes interpreted by that exact item. Container/uninterpreted items should not inherit byte-order flags from their children.

## Keep Qt strings in their native domain when comparing

In merged !3986, Roland Knall recommended direct `QString` comparison rather than converting through `std::string`/UTF-8. The conversion happened to be safe for rpcap data, but needlessly narrows the contract.

**Rule:** compare UI strings using Qt string APIs unless an external byte encoding is semantically required.

## Plugin/static initialization must respect dynamic-library data-symbol rules

Guy Harris's merged !3980 documents why a plugin cannot necessarily use `&exported_data_symbol` from libwireshark in a static initializer on MSVC: the shared-library data address is resolved at load time rather than being a compile-time constant suitable for static data initialization.

**Rule:** do not assume imported data-symbol addresses are valid static-initializer constants across toolchains; initialize runtime structures after module load or keep suitable plugin-local constants.
