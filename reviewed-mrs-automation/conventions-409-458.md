# Conventions 409-458

- For a masked integer field, value-table keys must match the value after Wireshark applies the mask/shift, not the raw on-wire bit pattern (!458).
- Do not replace a registered dissector identity with a protocol short name when later machinery redispatches by dissector name; either preserve registration identity or introduce a separate explicit dispatch key (closed !455, Pascal Quantin).
- Configuration profiles and system appearance are orthogonal state domains; do not implement theme changes by automatically switching profiles (closed !453; Stig Bjørlykke, Guy Harris).
- Reuse a display-filter field across dissector contexts only when the semantic domain and value mapping are genuinely identical. Similar labels alone are insufficient (!434 vs closed !448).
- Registered dissector/protocol names are compatibility-facing identifiers for scripting and programmatic lookup. If they must change, retain aliases where supported rather than silently breaking callers (closed !445/!446, Pascal Quantin).
- Prefer `proto_tree_add_item_ret_*` when the same registered wire value is both displayed and consumed by parser/state logic (!449).
- Check optional dependencies for runtime artifact-name collisions as well as source/API compatibility; the Opus plugin was renamed because Windows' external library already produced `opus.dll` (!439).
- Put the complete translatable text, including punctuation such as an ellipsis, inside Qt's `tr()` string rather than relying on macro concatenation that translation extraction may not understand (!438).
- Theme/palette-derived Qt scene state must be rebuilt on palette change, while capture-lifetime state needs a full reset distinct from ordinary per-packet clearing (!436, !417).
- Name shared decoding APIs for representation distinctions that materially affect decoding, such as packed versus unpacked 7-bit strings, and migrate protocol-local decoding onto the common charset layer when possible (Guy Harris !435).
- Checker warnings for duplicate/consecutive filter abbreviations are semantic review signals. Fix proven copy/paste errors, but do not force unlike fields to share an identifier merely to silence the checker (!419, !420, !432, closed !448).
- Present a coherent logical commit for a feature; fold local review-fix commits into the feature change when they merely repair that same change (!422, Ronnie Sahlberg / Gerald Combs).
- Reassembly state from a prior PDU may be reused only while the current sequence still belongs to that unresolved PDU. Once the prior PDU is complete and the cursor is beyond its end, start a new PDU; if the subdissector later requests more data, completion state must be reconsidered (!416, Peter Wu review).
- When repeated inspection of previous packets is standing in for protocol state, introduce an explicit protocol state machine so invalid transitions and later reassembly logic have a stable representation (!411).
