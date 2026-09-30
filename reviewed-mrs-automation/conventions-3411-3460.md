# Convention synthesis: !3411–!3460

Corpus: ddcaa22b51c68f594e425a23388c3a2086813054

- **Capture status/IPC:** Guy Harris !3449 preserves success-with-warning from pcap_activate(). !3448 sends SP_FILE only after the pcapng SHB is written; !3458 shows stderr can be a dumpcap protocol channel. Preserve semantic status, signal readiness only when the consumer can proceed, and keep diagnostics off IPC-owned descriptors.
- **UAT:** !3455, with Pascal Quantin and Guy Harris review, reinforces safe output initialization, exact input validation, diagnostic ownership, and explicit status results.
- **CMake:** Gerald Combs !3434 makes plugin-local include paths PRIVATE; !3450 narrowly invalidates stale dependency discovery. Keep implementation requirements private and invalidate only stale authoritative cache state.
- **Reentrancy:** !3427 warns that WSLua redissect_packets() can recursively re-enter a dissector. Processing-triggering APIs need explicit reentrancy protection or must be called outside that processing callback.
- **Field provenance:** !3432 establishes that masked semantic values read directly from packet bits are wire-backed fields, not generated fields.
- **Protocol evidence:** !3419 updates ASN.1 inputs and generated output together, verifies a spec/wire mismatch with Microsoft dochelp plus a capture, and leaves uncertain structure opaque. Prefer authoritative evidence over speculative decoding.
- **Documentation:** Guy Harris !3423/!3425/!3426 shows worked examples should explain their goal and transformations, not only give commands.
