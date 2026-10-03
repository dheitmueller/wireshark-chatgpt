# Conventions 459-508

- Per-flow state belongs in conversation/context state rather than mutable globals; globals survive capture changes and collide across simultaneous flows (!505, Pascal Quantin).
- Resolve reusable subdissector handles in proto_reg_handoff_* and validate before use instead of looking them up for every packet (!505).
- Represent meaningful wire fields such as IDs and lengths so users can filter and inspect malformed traffic; do not skip them only for tree aesthetics (!505).
- Platform-specific capture conversion that does not implement the normal Wiretap reader contract belongs in extcap rather than a file-extension exception in Wiretap (!468; Dario Lombardo, Graham Bloice, Guy Harris review; merged extcap result).
- Repeated parsing must advance or terminate. Validate boundary and next-offset arithmetic before it controls another iteration; stop when malformed framing makes later offsets unknowable (!484, !467, !463).
- Display-filter abbreviations are compatibility-facing identifiers. Avoid gratuitous renames, and name a displayed field for the semantic value Wireshark exposes rather than blindly copying a raw RFC bitfield label (!483).
- If callers free an API result, every successful return path must return storage with the same ownership contract (!494).
- An impossible/default path still needs defined state for compilers and static analyzers even after an assertion (!486).
- When protocol variants can coexist or nest, classify at the smallest message scope that can vary rather than relying only on a global preference (!506).
- UI values derived from the application palette must be recomputed or invalidated when the palette changes (!508).
- check_typed_item_calls.py findings are signals to investigate, not authority for uncertain mechanical renames (!475, !482).
