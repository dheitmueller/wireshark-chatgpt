# Review findings 509-558

!539: MP2T reassembly now keys by addresses and ports; independent streams sharing an IP pair must not share reassembly state.

!522: Exablaze and Metamako trailer heuristics were disabled by default because their recognition is too greedy. Weak heuristics may remain available for explicit opt-in.

!509/!510: MC-NMF replaced a signed size return with an error sentinel by a boolean status plus a typed output value. Keep failure status separate from a bounded numeric data domain.

!514: DVB-S2 uses `proto_tree_add_item_ret_uint()` so a displayed field is decoded once and the returned value drives parsing.

!556: Guy Harris added `proto_tree_add_item_ret_ipv4()` and used `ws_in4_addr` in IPv4 APIs. Public field APIs should expose semantic types, and new exports require symbol metadata.

!530: a hidden `3gpp.tmsi` field provides cross-protocol filtering while protocol-specific visible fields remain intact.

!552: use lookup success/failure directly instead of comparing a fallback string such as `Unknown`.

!557: validate packet-associated protocol data before dereferencing it.

!558: avoid a stronger semantic decoder when the packet encoding does not prove its assumptions.

!528 closed without merge; its implementation is not precedent. Reviewer guidance on removing template debris, conservative default-port binding, and providing captures remains useful.

No SMPTE ST 291/VANC packet type was encountered.
