# Wireshark conventions from !6911–!6960

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

- !6936 (Gerald Combs, merged): conversation lookup wildcards (NO_ADDR_B / NO_PORT_B) and conversation creation omissions (NO_ADDR2 / NO_PORT2) are distinct flag domains. This corroborates the later !7064 hardening.
- !6911 (John Thacker, merged): stream-specific latched state belongs in conversation proto-data, not a capture-global variable. Store the transition frame or equivalent history when redissection must preserve pre-transition interpretation.
- !6922 (John Thacker, merged): pcapng interface metadata can appear after packets begin. One-pass Wiretap consumers must update interface mappings as metadata arrives; if a policy needs future knowledge, document the deterministic one-pass limitation.
- !6923 (John Thacker, merged; Guy Harris and Pascal Quantin discussion): CLI spellings are compatibility surface. Preserve redundant options through a deprecation interval where practical, synchronize documentation/release notes, and avoid unrelated command renames that add script breakage.
- !6937–!6940 (John Thacker, merged): field display modifiers such as BASE_SPECIAL_VALS and BASE_UNIT_STRING must behave consistently across masked fields, bitmask titles, signedness, and integer widths. For BASE_SPECIAL_VALS, unmatched values fall back to numeric display.
- !6957 (merged; Gerald Combs and Alexis La Goutte discussion): fix correctness issues on master first, then cherry-pick or manually adapt them to supported stable branches.
- !6930 (João Valverde, merged): for an exhaustive code-controlled enum, express the impossible state as an invariant instead of hiding it with arbitrary initialization. Packet-controlled malformed input remains ordinary validation.
- !6929 (merged; John Thacker review): keep transient Qt editor keystrokes distinct from model commit when commit-time validation or view refresh can recreate the editor.
- !6953 and !6960 are closed; !6918 is still draft/open. Their implementation choices are not accepted precedent.
