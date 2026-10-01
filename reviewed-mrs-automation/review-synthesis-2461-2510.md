# Review synthesis 2461-2510

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged MRs and direct maintainer guidance are weighted more heavily than closed MRs.

## Build configuration

Guy Harris authored merged master MR 2501 moving Large File Support and fseeko checks before source subdirectories are added, ensuring required compiler definitions apply to all translation units. The preceding Guy-authored MR 2491 replaced a CMake implementation that did not actually enable needed large-file flags on 32-bit Ubuntu with proven libpcap logic; stable backports 2495, 2498, 2502, and 2503 preserve the same approach.

Durable lesson: feature checks that change ABI-visible types, header behavior, or feature-test macros must run before targets that consume them. Validate on the platform that actually requires the feature.

## Display-filter type boundaries

Guy Harris authored merged MRs 2479, 2465, and 2490 removing FT_PCRE as a registered field type and representing regular expressions in the display-filter syntax tree and VM instead. A regex is an expression-language operand, not a protocol-tree field value.

Durable lesson: keep expression-only compiler/VM values out of the protocol field type registry.

## Assertions while dissecting

Guy Harris authored merged MR 2466 replacing ordinary GLib assertions in frame/TCP dissection with Wireshark dissector assertions so a dissector invariant failure is reported through the packet dissection framework rather than terminating the whole application. Related merged MR 2467 uses explicit GLib errors for structurally invalid preference registration so startup/registration programmer errors have meaningful diagnostics.

Durable lesson: use dissector assertions for invariants reached during packet dissection; reserve application-level fatal diagnostics for globally invalid registration or initialization state.

## Subdissector registration intent

Merged MR 2468 initially proposed a preference controlling whether protobuf plugin dissectors could run for all field types. Anders Broman questioned the extra preference: if a subdissector is registered for a concrete field name, that registration already expresses intent. The merged revision simply performs the keyed lookup for every named field.

Durable lesson: do not add a second preference gate when a precise subdissector registration is already the opt-in signal, unless the preference represents independent semantics.

## Avoid catch-all protocol bindings

Merged MR 2482 removes IPPUSB registration for unknown and vendor-specific USB classes because unrelated devices were being decoded as IPPUSB. Only the printer class remains bound by default.

Durable lesson: broad unknown/vendor-specific buckets are not protocol identifiers. Prefer precise table keys, device mappings, conservative reject-capable heuristics, or Decode As.

## Capture subsystem boundaries

Merged MR 2470 consolidates caputils and capchild under a capture directory. Guy Harris's review explains that capture support spans wrapping libpcap/WinPcap/Npcap differences and implementing platform-specific capture behavior those libraries do not provide. He also notes that an interface imported by several unrelated components without one clear owner is a sign that the abstraction should be reconsidered.

Durable lesson: organize capture code around responsibility boundaries, not historical directory names. An API with many consumers but no natural owner deserves architectural scrutiny.

## TCP sequence arithmetic

John Thacker authored merged MR 2461 fixing multisegment PDU handling across TCP sequence-number wraparound. It uses LE_SEQ and GT_SEQ and compares relative distances rather than ordinary integer ordering.

Durable lesson: TCP sequence numbers are modular. Use Wireshark's wrap-aware helpers and distance arithmetic for ranges and lengths.

## Generated code

MRs 2483 through 2489 and MR 2500 continue the established rule that ASN.1/config/template source and generated dissector output move together. Manual filter-name overrides should follow the generator's normalization behavior rather than diverging from it.

## Lower-weight closed-MR evidence

Closed WIP MR 2494 explored a doubly linked reassembly fragment list. Pascal Quantin suggested tracking the next or first hole instead, Tomasz Mon asked that temporary sanity checks become automated reassembly tests, and later discussion reported major gains from the simpler first-gap optimization. The useful review lesson is to benchmark simpler state caching before adding per-fragment structure overhead and to turn temporary correctness instrumentation into regression tests.

Closed MR 2473 proposed a TShark columns switch solely to force column construction for plugins. Guy Harris instead suggested declaring the need for columns at registration time, analogous to tap capability flags, so users do not need to know an internal prerequisite. Treat this as negative design evidence, not accepted implementation.
