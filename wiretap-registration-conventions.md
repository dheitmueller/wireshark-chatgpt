# Wiretap registration conventions

## Let a file-format module register its own compatibility aliases

Backward-compatible names for a capture file type are part of that format module's registration policy. Avoid a separate central name-remapping table that must be kept synchronized with the module that defines the canonical type.

In merged !2359, Guy Harris replaces the central old-name table with `wtap_register_compatibility_file_subtype_name()` calls made from the libpcap format registration. The shared layer still performs lookup, but the module that owns the format declares which historical names map to its current names.

**Implementation rule:** central infrastructure may provide the registry, but format-specific compatibility entries should be registered by their owner module.

**Confidence:** Extremely high. Merged master architectural cleanup authored by Guy Harris.

## Keep unknown registry values out of the valid index space and validate before reuse

Guy Harris's merged master MR !2224 changes WTAP_FILE_TYPE_SUBTYPE_UNKNOWN from a table entry into the out-of-band sentinel -1 and removes the synthetic unknown file-type record. Guy-authored !2238 then makes a failed UI file-type lookup return that named sentinel and requires callers to diagnose the impossible result. Guy-authored !2217 adds explicit nonnegative and in-range checks before file-type table access and restructures dumper initialization so the subtype is validated once before it is stored and reused by downstream helpers.

**Registry rule:** when unknown is not a real registered entity, represent it outside the valid registry index space. Validate lookup-derived indices at the owning boundary, then structure later helpers around the invariant that stored registry values are valid.

**Confidence:** Extremely high. Three merged master changes authored by Guy Harris that converge on the same sentinel and bounds invariant.

## API cardinality should match its name, and stale plugins should fail visibly

Merged master MR !2215, authored by Guy Harris, renames wtap_register_file_type_subtypes() to the singular wtap_register_file_type_subtype() because each invocation registers exactly one subtype. The source/API break also deliberately forces Wiretap plugins to rebuild, exposing plugins that still use the old file_type_subtype_info layout. Registration gains structural validation and rejects bogus file types without terminating the host.

**Plugin/API rule:** name registration APIs after their actual cardinality and semantics. When a plugin-facing structure has materially changed, an intentional rebuild break can be safer than preserving a compatibility surface that lets stale plugins silently misdescribe their capabilities. Reject invalid plugin registrations with a diagnostic rather than terminating the application when continued operation is safe.

**Confidence:** Extremely high. Direct merged master API and registration cleanup authored by Guy Harris.
## Treat file type/subtype numbers as runtime registry identities

A Wiretap file type/subtype is a registry identity, not a permanent protocol constant that unrelated code should bake in. Format modules should register themselves, retain their assigned runtime identity, and expose a semantic accessor only when callers genuinely need to name a well-known format.

Merged master MR !2164, authored by Guy Harris, converts ERF and systemd-journal subtypes from fixed `WTAP_FILE_TYPE_SUBTYPE_*` constants to runtime registration. Merged Guy-authored !2201 does the same for pcap, nanosecond pcap, and pcapng, replacing direct constants with `wtap_pcap_file_type_subtype()`, `wtap_pcap_nsec_file_type_subtype()`, and `wtap_pcapng_file_type_subtype()`. Diagnostics on an already-open dumper query `wtap_dump_file_type_subtype()` instead of assuming the writer format.

**Registry rule:** treat numeric subtype values as results of the format registry. Keep the identity inside Wiretap and expose the semantic query a caller needs rather than encouraging callers to depend on numeric layout.

**Confidence:** Extremely high. Two merged master architectural refactors authored by Guy Harris.

## Model format support as explicit block/option capabilities

Generic save/export code should ask a file handler which abstract structures it supports rather than infer capabilities from format names or maintain parallel booleans for comments, name-resolution records, interface IDs, and similar features.

Guy Harris's merged master MR !2183 replaces those coarse properties with `supported_block_type` and `supported_option_type` tables, including multiplicity. The Wiretap abstraction is deliberately semantic: a native format need not literally contain a pcapng-style block for Wiretap to expose the corresponding abstract information. In particular, `WTAP_BLOCK_IF_ID_AND_INFO` means packet records can be associated with an interface; a format that merely lists interfaces without packet-to-interface identity must not advertise that capability.

Guy-authored follow-up !2192 immediately fixes the nested capability-table traversal, where the option loop accidentally incremented/indexed the block-loop variable. The accepted code uses distinct `block_idx` and `option_idx` variables.

**Capability rule:** let each format owner declare the exact abstract block/option contract it implements and have generic code query that contract. In nested capability structures, keep index variables tied to their semantic domain so a block index cannot silently be reused as an option index.

**Confidence:** Extremely high. Merged master architecture and immediate correctness follow-up authored by Guy Harris.
