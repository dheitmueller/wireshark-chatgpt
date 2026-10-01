# Durable conventions from MRs !2561–!2610

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Use link/object evidence to enforce module visibility

Merged !2610 (Martin Mathieson) adds `check_static.py`, which inspects built object symbols rather than relying only on textual source searches. It can therefore identify dissector functions/data exported globally even though no other translation unit actually references them. Merged !2580 immediately converts two such functions to `static`.

**Rule:** when a symbol is implementation-local, give it internal linkage. Project tooling may use object/linker evidence to find unnecessarily exported dissector symbols because source-text grep cannot reliably model actual cross-object references.

**Weight:** High — merged checker by Martin Mathieson plus an adjacent merged cleanup produced by the rule.

## Keep useful warnings enabled globally; suppress intentional exceptions narrowly

Merged !2606 is authored and merged by Guy Harris. It removes the global `-Wno-override-init` setting because duplicate designated initializers sometimes reveal real bugs. Known intentional patterns should carry a local suppression rather than disabling the warning for the whole project. Guy also notes that compilers exposing the same diagnostic do not necessarily use the same option name.

**Rule:** do not globally disable a warning merely because a few intentional constructs trigger it. Keep the bug-finding signal active project-wide and use the narrowest source/compiler-specific suppression where the invariant is understood.

**Weight:** Extremely high — direct Guy Harris-authored merged build policy.

## If capture metadata is in the packet bytes, let the decoder own the pseudo-header

Merged !2599 is authored and merged by Guy Harris. NetMon's 802.11 metadata precedes the actual 802.11 frame in packet data. The accepted architecture removes pseudo-header initialization from Wiretap and has the NetMon dissector create/fill a local `ieee_802_11_phdr`, then pass it to the normal radio dissector, just as radiotap-style metadata paths do.

**Rule:** when metadata is itself an encoded header in captured packet bytes, Wiretap should expose the bytes and the metadata dissector should own semantic interpretation and construction of the downstream pseudo-header. Avoid splitting defaults in Wiretap and field decoding in epan unless the capture record format truly supplies out-of-band metadata.

**Weight:** Extremely high — direct Guy Harris-authored merged architecture change.

## Treat derived packet-list columns as cached presentation that must be invalidated

Merged !2604 fixes mark-state changes that did not immediately update a custom `frame.marked` packet-list column. The accepted code redraws visible packets immediately after each state mutation. Guy Harris asks for a local comment at every redraw explaining why it is necessary, and those comments land.

**Rule:** when mutable frame state feeds a derived/custom packet-list value, mutate the model state and explicitly invalidate/redraw the affected presentation. If the refresh call looks redundant from local control flow, document the hidden dependency at the call site.

**Weight:** Very high — merged correctness fix with direct Guy Harris review shaping the final code.

## Save and restore parent packet context around embedded-packet subdissection

Merged !2585 decodes an embedded Ethernet frame inside NetFlow IE 315. Calling the Ethernet dissector can rewrite `packet_info` addresses and columns. Martin Mathieson explicitly asks why the state is saved/restored; the accepted code documents the reason, disables column writes during the child call, saves the parent address tuple, calls the child dissector on a child TVBuff, then restores both column writability and addresses.

**Rule:** a child TVBuff does not isolate shared `packet_info` side effects. When dissecting an embedded packet whose addresses/columns are not supposed to replace the parent protocol's identity, save the relevant parent context, constrain child column writes, and restore the parent state after the call.

**Weight:** Very high — merged implementation shaped by direct Martin Mathieson review.

## Validate fit before narrowing a computed value to a fixed-width format field

During merged !2573, MSVC reports a `size_t` to 16-bit conversion in WAV header construction. Guy Harris explicitly says code should prove that `channels * sizeof(SAMPLE)` fits in 16 bits, return an error if it does not, and only then cast to the 16-bit destination type.

**Rule:** when a computed host-width value is serialized into or stored as a narrower fixed-width field, do not rely on expected real-world ranges. Check the representable bound before narrowing and propagate failure if it does not fit; an explicit cast belongs after the range proof, not instead of it.

**Weight:** Extremely high as review guidance — direct Guy Harris portability/correctness review on a merged MR.

## Heuristic recognition should use cheap structural invariants

Merged !2590 strengthens TFTP request recognition by counting NUL separators and requiring the option/value structure to have an even, nonzero count while rejecting nonprintable bytes. John Thacker explicitly confirms the invariant and the efficiency of the approach. Closed !2586 separately shows the motivation for avoiding exception-throwing accessors in heuristics, but is retained only as lower-weight evidence because it did not merge.

**Rule:** use inexpensive protocol-structural invariants to reject false positives before committing a heuristic dissector. Recognition code should prefer bounded/non-throwing probes where possible and return FALSE for malformed candidates rather than depending on successful deep parsing.

**Weight:** High for the structural invariant from merged !2590; low/moderate for the non-throwing-accessor point from closed !2586.

## Keep generated inputs and checked-in output synchronized

Merged !2592 and !2591 update ASN.1 definitions/config/templates together with generated LTE/NR RRC C. Merged !2575 similarly changes ASN.1 `.cnf` source and the generated dissectors together while improving field semantics.

**Rule:** when generated dissector behavior changes, update the generator/template/configuration source of truth and regenerate the checked-in output in the same change.

**Weight:** High corroboration of an already-established notebook convention.

## Representative captures remain first-class review material

In merged !2583, Alexis La Goutte asks for a pcap exercising the new 802.11 Reduced Neighbor Report support, and the contributor supplies one.

**Rule:** include a small representative capture with protocol/dissector changes when practical; it enables review, regression inspection, and future fuzzing.

**Weight:** High corroboration of the existing sample-capture convention.

## Use semantic field types and named width constants

Merged !2582 (John Thacker) replaces magic MAC/EUI widths with `FT_ETHER_LEN`/`FT_EUI64_LEN`, models Modified EUI64 as `FT_EUI64` rather than raw bytes, and chooses BCD versus ASCII IMEISV decoding from the valid encoded lengths while flagging invalid lengths. Merged !2575 similarly maps fixed-size ASN.1 location identifiers to `FT_UINT8/16/24`.

**Rule:** when Wireshark has a field type or size constant matching the protocol concept, use it instead of raw bytes/magic lengths. Let validation distinguish genuinely supported encodings and report invalid sizes explicitly.

**Weight:** High — merged John Thacker protocol-modeling changes.
