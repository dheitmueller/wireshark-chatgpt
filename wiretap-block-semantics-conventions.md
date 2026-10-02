# Wiretap block semantic conventions

Wiretap block identifiers are an internal semantic namespace, not a mirror of pcapng's on-disk block type numbers. More than one file-format block can map to the same Wiretap semantic block. Name internal block types for the object they represent, such as section, interface description, or decryption secrets.

Opaque block APIs should expose semantic queries such as `wtap_block_get_type()`; callers should not need representation knowledge to discover the object's type.

Prefer explicit semantic block classes over an ad-hoc fixed pool of "custom block" IDs. If a new representation has distinct semantics, model those semantics directly rather than allocating an anonymous slot and leaking format-specific numbering into the common layer.

Evidence: merged Guy Harris-authored !2033, !2036, and !2038.
