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
