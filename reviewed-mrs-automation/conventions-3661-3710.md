# Durable conventions extracted from MRs !3661–!3710

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Prefer explicit packet allocator ownership over ambient packet-scope globals

Merged !3673 uses TCP as a proof of concept for replacing `wmem_packet_scope()` with `pinfo->pool`, and merged !3710 applies the same direction broadly. The updated wmem documentation says `pinfo->pool` should be preferred when packet context is available.

**Rule:** pass the allocator/lifetime owner through the API surface when it is available. For packet-lifetime data in dissectors, prefer `pinfo->pool` over silently reaching for the global packet scope. Validate broad lifetime refactors on a high-traffic/stateful dissector and with memory tooling before mechanical expansion.

## Treat block-option getter results as borrowed unless the API transfers ownership

Guy Harris's merged !3699 fixes ERF code that retained a string returned by `wtap_block_get_nth_string_option_value()` and later freed it. The accepted fix duplicates the string before storing it in independently-owned state.

**Rule:** a pointer returned from a Wiretap block option getter is borrowed block storage unless the API explicitly says ownership transfers. If the value must outlive the block, or the receiving object will free it, duplicate it at that ownership boundary.

## Represent optional packet metadata as typed block options when that is its semantic home

Merged !3708 moves drop count, packet ID, and interface queue out of fixed packet-header fields plus parallel `WTAP_HAS_*` bits and into typed `WTAP_BLOCK_PACKET` options.

**Rule:** when metadata is naturally optional block metadata, use the block-option mechanism and let option presence represent whether the value exists.

## Frame TCP protocols as byte streams, not packets

Merged master !3698 and release-3.4 backport !3697 fix DLM3 because multiple application messages can occupy one TCP segment and one message can span segments. The accepted TCP entry point uses `tcp_dissect_pdus()`.

**Rule:** if an application protocol has recoverable PDU lengths over TCP, use Wireshark's TCP PDU helper rather than assuming segment boundaries are message boundaries. Keep the one-PDU decoder separate from stream framing.

## Preserve field-value semantics while hardening parsers

Merged !3693 replaces unsafe JSON unescaping with bounded TVB checks and `wmem_strbuf`. The first rewrite also changed decoded string presentation, which broke existing filters/tests. Gerald Combs preferred restoring the established semantic value instead of changing tests to normalize the accidental output change.

**Rule:** parser hardening should not silently change the semantic value exposed by registered fields. If regression tests fail because a safety refactor changed representation, determine whether the representation change was intended before changing expected output.

## Include bus/interface context when an identifier is only locally unique

Merged !3689 shows that LIN's small frame-ID space is routinely reused across parallel buses. The accepted mapping key combines bus identity with frame ID and reserves bus ID zero as an explicit “any bus” fallback.

**Rule:** key state/dispatch by the full uniqueness domain. If an ID is only unique within a bus, interface, controller, or parent object, include that context.

## Do not force exact 64-bit scripting values through floating-point-only argument forms

Merged !3686 extends WSLua ProtoField masks to 64 bits. Review identified that ordinary Lua numbers cannot exactly encode every 64-bit integer. The accepted binding therefore also supports exact-width `UInt64` userdata and string forms.

**Rule:** when a scripting-language numeric type cannot losslessly represent the native API's full integer domain, provide an exact-width representation path and test the type-conversion boundary across supported platforms.

## Put CLI options in the subsystem that owns their semantics, and ask capabilities rather than hard-coding formats

Guy Harris's merged !3675 removes capture comments from live-capture-only `capture_options`: TShark can add comments while reading and writing files even when live capture is unavailable. Guy's merged follow-up !3676 moves validation outside the libpcap build guard and queries the selected output file type for comment support rather than testing only for pcapng.

**Rule:** CLI option ownership and build gating must follow the operation's semantic capability. Validate output features through the format/capability API. If an option is repeatable and additive, preserve every occurrence and describe it as additive rather than singleton replacement.

## Keep overlapping protocol identifier domains separate while preserving compatibility fallback

Merged !3668 splits standard CAN IDs and extended CAN IDs into separate keyed dissector tables because the numeric spaces overlap. Those more-specific tables are tried before the retained legacy generic table.

**Rule:** if two wire identifier domains can contain the same numeric value but mean different things, represent them as separate dispatch domains. Give specific semantic tables priority while retaining a documented compatibility fallback when needed.
