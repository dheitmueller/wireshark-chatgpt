# Wiretap registration conventions

## Let a file-format module register its own compatibility aliases

Backward-compatible names for a capture file type are part of that format module's registration policy. Avoid a separate central name-remapping table that must be kept synchronized with the module that defines the canonical type.

In merged !2359, Guy Harris replaces the central old-name table with `wtap_register_compatibility_file_subtype_name()` calls made from the libpcap format registration. The shared layer still performs lookup, but the module that owns the format declares which historical names map to its current names.

**Implementation rule:** central infrastructure may provide the registry, but format-specific compatibility entries should be registered by their owner module.

**Confidence:** Extremely high. Merged master architectural cleanup authored by Guy Harris.
