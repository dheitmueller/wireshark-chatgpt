# Conventions from MRs 8661-8711

Merged !8677 (João Valverde) establishes that generic ftype strings are encoding-agnostic; UTF-8 validation belongs at APIs that promise protocol/UI text, and packet-derived semantic string values must not be escaped merely for presentation. Merged !8711 is negative/intermediate evidence because João objected after merge; later !8731/!8724 remain authoritative.

Merged !8708, authored by Guy Harris, removes the timezone fixed buffer entirely and passes separator/name components to the final dynamic formatter. Prefer eliminating avoidable intermediate fixed buffers over guessing a larger capacity.

Merged !8689 shows the byte-range rule for bit-level fields: floor the starting bit to its containing byte and ceil the ending bit so every partially consumed final byte is included without an unconditional extra byte.

Merged !8686 shows that configuration operations must respect registry lifecycle. If a startup-time registry is not yet populated, make a narrow domain-backed exception explicit and centralize the underlying filesystem operation and error ownership.

Merged !8698, authored by Guy Harris, shows that compiler range warnings around specification constants should trigger specification/version verification; document later-spec provenance instead of casting away the warning.

Merged !8666 shows that derived classifications with no useful raw numeric domain can be generated FT_STRING fields; keep independent taxonomies separate and avoid expert warnings for legitimate-but-unusual/test values.

Merged !8665 uses proto_tree_add_item_ret_display_string() when a typed field's safe display text is also needed in a surrounding label, avoiding ad-hoc raw-byte string handling.
