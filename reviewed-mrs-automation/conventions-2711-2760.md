# Durable conventions from !2711–!2760

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Contiguous growth and copy units
Merged !2724, authored by Guy Harris, replaces ptvcursor's manual allocate-and-copy growth with `wmem_realloc()`. Related Guy-authored fixes !2725 and !2726 show the concrete bug in the manual path: a `memcpy()` count was expressed as elements instead of bytes. Prefer realloc when growing one contiguous allocation in the same scope. When copying manually, calculate bytes as element size times the old populated count before changing capacity.

## Named dissector registration phase
In merged !2751, Pascal Quantin requires named SOME/IP handles to be registered in `proto_register_someip()` before handoff. He explains that other consumers can search those handles by name, including exported decrypted-data paths. Register reusable named identities during protocol registration; use handoff to bind those existing handles to transports and tables.

## Tree-independent semantic dissection
Merged master !2730 and stable !2736 move LDAP decrypted-payload dissection outside the optional `sasl_tree` block. Protocol-tree presence controls presentation, not whether recursive semantic dissection occurs. Do not gate state, recursive dissection, columns, taps, or reassembly on tree visibility.

## Platform macro namespace collisions
Merged !2735 hit Windows CI because shared `REG_*` names collided with definitions from WinNT.h reached through a transitive include. Pascal Quantin traced the include path and the accepted header guards matching fallback definitions. Prefer project-prefixed macro names; when platform-provided names must coexist, account for transitive headers and validate on the platform matrix.

## Accumulator-return contracts
Merged !2718 fixes RTP hashing by assigning the return from `add_address_to_hash()` back to the hash value. Do not infer in-place scalar mutation from a helper's name. When a fold/hash helper returns the updated accumulator, propagate that return at every step.

## Preserve loop failure status
Merged !2711, authored by Guy Harris, initializes success before an interface loop, records failure at the failing iteration, breaks, and removes a post-loop assignment that would erase the error. Initialize aggregate status before the loop and never overwrite an early failure with unconditional success during common cleanup.

## Field registration semantics
Merged !2747 corrects field types and widths; !2753 and !2728 correct copied display-filter abbreviations and collisions, with !2738 fixing a follow-up typo. Registered type, width, and filter name are API semantics, not cosmetic metadata.

## Generated-source ownership
Merged !2745 changes ASN.1 templates together with generated C; !2741 changes Lemon source; !2730, !2736, and !2727 keep ASN.1 inputs and generated artifacts synchronized. Fix generated code at its authoritative input and regenerate the checked-in output.

## Numeric presentation continuity
During merged !2717, Anders Broman suggests `BASE_HEX_DEC` when adding decimal presentation to a value historically displayed in hex. A combined base can improve readability without discarding an established representation.

## Closed proposals versus accepted successors
Closed !2714 proposed silently returning when TCP reassembly was unavailable. Later merged !4787, authored by John Thacker, establishes `FragmentBoundsError` for that condition. Down-weight abandoned designs when a later merged change defines the accepted contract.
