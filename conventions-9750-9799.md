# Wireshark conventions from !9750-!9799

Durable lessons extracted from corpus commit `ddcaa22b51c68f594e425a23388c3a2086813054`. Current upstream source remains authoritative.

- **Parser progress (!9752, John Thacker):** a skip/recovery helper must advance past malformed input or terminate. Resetting the cursor to the same bad CBOR item caused repeated parsing, potential infinite loops, and memory exhaustion.
- **Reassembly boundaries (!9751, !9795):** protocol termination events such as USB STALL are hard transaction boundaries. Finalize only acknowledged/accepted data where required, then clear all state so the next transfer cannot inherit sequence or fragment history.
- **Redissection identity (!9776):** when one frame contains multiple logical PDUs, persist second-pass state per PDU rather than one slot per frame. Persistent conversation values need file-scope lifetime; stateful decompression output needed by later passes must be preserved. The historical CRC key is evidence for per-PDU identity, not a preferred collision-free key design.
- **Layering (!9780):** generic command-line/filter helpers needed by `dumpcap` belong in `wsutil`, not `ui`; shared low-level executables should not gain a UI link dependency for generic utilities.
- **Typed bitmask fields (!9789, Martin Mathieson):** all fields grouped in one bitmask array must agree on the containing width. The checker should validate that group invariant.
- **UTF-8 presentation (!9772, John Thacker):** truncate with UTF-8-aware helpers, not byte-oriented printf precision.
- **Callback context (!9753):** extension callbacks that decode bytes must receive interpretation context already known by the caller, such as pcapng encoding/endianness.
- **Protocol validation (!9758, Guy Harris):** if a packet is structurally recognizable and safe to decode but carries a specification-forbidden value, retain useful dissection and attach expert warning to the offending field rather than rejecting solely for normative invalidity.
- **Length-delimited loops (!9798, John Thacker):** subtract fixed leading fields from the remaining-length budget before iterating variable records.
- **Build/platform modeling (!9794, João Valverde):** normalize supported toolchain/environment detail into a project semantic target identity, then derive downstream architecture properties from that identity; do not broaden the supported build model incidentally.
- **Shared frontend code (!9799, Gilbert Ramirez; open):** substantial duplicated CLI validation/listing logic should move to a common lower layer. Because the MR is open, treat this as provisional design guidance.
