# Wireshark MR review findings: !3411–!3460

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

Strongest evidence in this batch:

- !3449 (merged, Guy Harris): preserve capture-open success-with-warning as a first-class status and show the warning while capture continues.
- !3448 (merged): emit dumpcap SP_FILE only after the pcapng Section Header Block has been written; closed !3445 used a weaker first-packet boundary and is superseded.
- !3458 (merged): dumpcap can use stderr as a parent/child protocol channel, so logging initialization must not emit arbitrary stderr diagnostics there.
- !3455 (merged): Pascal Quantin and Guy Harris review reinforces safe UAT output initialization, exact validation, error ownership, and explicit status semantics.
- !3451 (merged): copy a va_list before a second consumer traverses it.
- !3450 and !3434 (merged): invalidate stale CMake dependency discovery narrowly, and keep plugin-local include paths PRIVATE.
- !3432 (merged): masked values read directly from packet bits are wire-backed fields, not generated fields.
- !3427 (merged): WSLua redissect_packets() must not be called from a dissector callback because it can recursively re-enter dissection.
- !3421 (merged): parse controls for an early logging subsystem before its first observable use.
- !3419 (merged): change ASN.1 source/templates and regenerated artifacts together; resolve spec/wire conflicts with authoritative evidence and captures rather than guessing.
- !3423/!3425/!3426 (Guy Harris): worked documentation examples should explain the goal and each transformation.

Closed !3445, !3437, !3428 and !3416 were down-weighted. Merged !3422 is superseded by merged revert !3433. All other MRs in the exact ledger were reviewed but added no stronger distinct convention.

No SMPTE 291/VANC packet type was encountered.
