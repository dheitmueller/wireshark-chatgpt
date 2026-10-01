# Review synthesis 2511-2560

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This file restores durable findings from the preceding completed review run whose repository writes were blocked at the time.

- Guy Harris-authored merged 2556 and stable backports 2557/2558 show that a subset TVBuff method delegating a search to its parent must translate successful parent-relative results back into the subset coordinate system exactly once while preserving not-found sentinels.
- Guy Harris-authored 2545 through 2548 distinguish logical range length from captured length when constructing child TVBuffs; ordinary subset APIs should derive the captured extent from the backing TVBuff rather than forcing the logical range length into captured length.
- Closed 2540, with Anders Broman review, is lower-weight negative evidence that Decode As should follow the immediate protocol layer rather than binding gRPC directly on TCP and recursively reconstructing HTTP/2 state.
- Guy Harris-authored merged 2544 favors narrow diagnostic suppression through Wireshark's compiler-diagnostic abstraction when an intentional construct is correct and clearer than a large workaround.
- Merged 2512 and 2518 show that compatibility shims should preserve a newer dependency API's corrected type contract. Wireshark's older-GLib g_memdup2 fallback keeps the gsize signature, then tooling prohibits the unsafe predecessor API.
- Review of 2514 shows that static-analysis dead-store reports in parser code require semantic state review before deleting assignments.
- Merged 2519 shows that value_string_ext lookup tables must retain their numeric ordering contract; tshark -G values can expose a fallback to linear search.
- Guy Harris-authored 2516 distinguishes capture transport mechanism from recorded capture-source identity; extcap FIFO paths should not substitute for meaningful interface name/description metadata.
