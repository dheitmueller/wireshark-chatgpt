# Review findings: !910-!959

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`.

The strongest merged findings in this batch are:

- !959: John Thacker moved ordinary bit geometry into field metadata so packet-diagram consumers can derive offset and width from the registered type and mask. Gerald Combs requested debug-warning validation; the focused RTP/H.263 test produced substantially fewer warnings.
- !942: Martin Mathieson extended the typed-item checker to detect consecutive fields with the same numeric mask under different labels.
- !938: RTPS removed `g_free` callbacks from a GLib hash table containing wmem-owned packet-scope keys and values, fixing an allocator-ownership mismatch.
- !929, !931, !932: Guy Harris fixed TCP capture setup so `connect()` receives the concrete IPv4/IPv6 socket-address length rather than the capacity of `sockaddr_storage`, and separated socket-creation from connection diagnostics.
- !927: Graham Bloice required the commit message to describe the resulting change rather than the prior merge-request process; the contributor amended it before merge.
- !950: Anders Broman steered the IEEE 1722 H.264 PTV boolean toward Wireshark's shared true/false strings rather than local duplicate wording.
- !955: the Protobuf language parser moved from Bison to Lemon to remove a Bison-specific portability/build requirement; the contributor retested the documented Protobuf/gRPC examples and several earlier bug cases.
- !933: Guy Harris recommended moving a Clang-Analyzer-flagged variable into the block where it is defined and consumed instead of widening its lifetime with defensive initialization.
- !953 closed unmerged. Its Bluetooth pointer initialization is retained as failure evidence but is not weighted as accepted implementation precedent.

No SMPTE ST 291/VANC packet type was encountered.
