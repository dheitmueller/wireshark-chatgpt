# Conventions from !8511–!8560

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`.

## Explicit packet memory context

Merged !8555 and !8552 replace ambient `wmem_packet_scope()` lookup with explicit context. Use `pinfo->pool` when packet context is already present; pass a `wmem_allocator_t *` to helpers that only need allocation, or `packet_info *` when packet semantics matter. The allocation lifetime should be visible in the helper contract.

## Text representation boundaries

Merged !8546 states directly that `proto_tree_add_string()` values must be UTF-8. !8511 likewise replaces raw TVBuff string pointers with encoding-aware extraction. !8515 and !8520 validate/decode JSON and XML text before protocol-specific parsing. !8543 separates valid semantic UTF-8 from label formatting and control-character escaping.

Merged !8545 (John Thacker) shows that encoded wire length is not a destination capacity after character conversion: UTF-16 source bytes can expand to a longer UTF-8 string. Size/copy from the converted representation.

## Header layering

Merged !8540 (Guy Harris, with João Valverde review) places shared Unicode constants in a lightweight top-level header. João explicitly warns against wmem depending on wsutil and against cyclic dependencies; Guy also avoids platform-heavy transitive includes that caused namespace pollution. !8544 adds the public header to the install/export list.

## Parser safety

Merged !8534 (Gerald Combs) applies both a recursion-depth bound and a no-progress check to packet-controlled loops. These protect different failure modes and should be used together when appropriate. !8522 similarly validates interval endpoint ordering before querying a range data structure.

## Display and lookup hygiene

Merged !8524 escapes valid control/non-printable Unicode and malformed bytes in logs: valid UTF-8 is not automatically safe terminal text. In !8521 Alexis La Goutte catches an out-of-order extended `value_string` entry that forced linear-search fallback; sorted tables should remain sorted.

The historical !17571–!17620 batch remains preserved and counted.
