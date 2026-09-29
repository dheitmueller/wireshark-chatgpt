# Wireshark CLI Option Compatibility Conventions

Merged master MR !5393, authored by João Valverde, handles legacy `-o console.log.level` syntax inside the wslog argument parser, translates the old value to the current logging level, and emits deprecation guidance without changing the general obsolete-preference policy. Closed !5380 is superseded history.

Merged !5367 improved unknown-versus-obsolete preference diagnostics across rawshark, tfshark, and tshark, but accidentally removed tfshark's syntax-error branch; merged !5369 immediately restored it.

**Rules:** localize compatibility translation at the subsystem that owns the replacement interface. When several frontends share a result-code state machine, verify success and every negative/error path in each frontend after mechanical edits.
