# Durable conventions from Wireshark MRs !3461–!3510

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

- !3461 and !3490: reassembly and analysis state must use the protocol's real stream identity. MP2T needs conversation plus direction; mutable packet addresses/ports are not a stable substitute.
- !3463: first-pass fragment lookup and redissection lookup are different operations. Use the reassembly API that matches the pass/state so incomplete end-of-capture fragments do not behave differently on pass two.
- !3498: state defined per capture interface must include interface identity in its key.
- !3477: allocation provenance controls deallocation. Packet-pool objects are reclaimed with packet scope; any explicit early release must use the matching wmem allocator API.
- !3489 and !3472: low-level logging/assertion paths must avoid helpers that can themselves log and should minimize dependency chains that create reentrancy or include-order hazards.
- !3475 and !3464: shared headers should express direct dependencies and should not gain broad higher-layer/configuration includes merely to make one consumer compile.
- !3480: expert info is preferable to plain appended error text for packet problems users should be able to find and filter.
- !3508 and !3484: pcapng mechanics that are common across block types belong in shared format code; block-specific callbacks should handle only block-specific semantics.
- !3488: iteration callbacks that can fail should return failure directly and stop the operation rather than relying on hidden side-channel error state.
- !3492 and !3478: set protocol columns only after positive recognition; nested protocol text should compose with useful existing context rather than erase it.
- !3495: assertions must describe truly invalid programmer states and must not reject legitimate values simply because one caller did not expect them.

No SMPTE 291/VANC packet type was encountered.
