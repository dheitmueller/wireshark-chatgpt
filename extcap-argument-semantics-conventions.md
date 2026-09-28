# Wireshark Extcap Argument-Semantics Conventions

## Match extcap argument types to command-line arity

Merged MR !6419 fixes sshdump after a presence-only remote option had been exposed through extcap as a boolean carrying an explicit value. On restart, the unchecked value was still serialized as an option occurrence, and the command-line parser treated presence itself as enabling the option. The accepted extcap declaration uses the flag form instead.

**Rule:** an extcap argument's declared type is part of the command-line contract. Presence-only options must use flag semantics; they are not equivalent to options that consume textual true/false values.

**Testing rule:** exercise first launch and restart/replay paths because extcap may reconstruct saved configuration differently from the initial interactive launch.

**Confidence:** High. Merged master behavioral fix with a concrete restart regression.
