# Review findings: !9750-!9799

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Strongest evidence: !9758 (Guy Harris) favors continued safe dissection plus expert warning for a recognizable but specification-invalid TLS/DTLS version; !9752 (John Thacker) requires parser recovery to advance; !9772 (John Thacker) uses UTF-8-aware truncation; !9776 preserves per-PDU state for RDP redissection; !9751/!9795 make USB STALL a hard reassembly boundary; !9780 moves generic helpers from UI to wsutil; !9789 extends typed-item checking to mixed-width bitmask sets; !9798 fixes repeated-record length accounting after fixed prefixes; !9753 passes pcapng encoding through extension callbacks.

!9799 is open and therefore provisional. !9778 is closed and superseded by merged !9824. All other MRs in the exact ledger are merged. No SMPTE ST 291/VANC packet type was encountered.
