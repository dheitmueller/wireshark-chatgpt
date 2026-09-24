# Wireshark Command-Line Option Interaction Conventions

This file records durable command-line option-interaction conventions extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Reject incompatible options instead of silently ignoring them

A command-line option that switches a tool into a distinct operating mode should make the compatibility of the remaining options explicit. If another option cannot affect that mode, silently accepting and ignoring it creates an ambiguous user contract and makes correct behavior look like a bug.

Merged master MR !16392, authored by John Thacker, added `editcap --extract-secrets`. The initial description said the mode would take over output and ignore all other options. Guy Harris explicitly challenged that behavior, asking whether inapplicable options should instead warn or fail so users would not report ignored options as bugs. The accepted implementation tracks whether an editing option was supplied, rejects the combination with `WS_EXIT_INVALID_OPTION`, documents that `--extract-secrets` is incompatible with other options except `-V`, and adds tests for the new extraction behavior.

**Implementation rule:** when a mode-selecting option makes other options semantically inapplicable, define the allowed combinations and validate them after option parsing. Prefer a clear diagnostic and an invalid-option result over silently discarding user intent.

**Documentation/testing rule:** document the compatibility set next to the option and test the mode's behavior, including incompatible combinations when practical. The parser, help text, and manual should describe the same contract.

**Confidence:** Extremely high. Merged master behavior with direct Guy Harris review, accepted by the author and reflected in the final implementation and documentation.

## Validate context-dependent combinations only after their derived mode is known, and before output side effects

Some command-line compatibility decisions depend on runtime-derived state rather than option presence alone. In TShark, whether dissection is required depends in part on registered taps, and the safety of dissecting while writing capture output to a pipe differs between live capture and offline file processing. Performing the check before those facts are established can reject valid workflows or, worse, miss an invalid workflow.

Merged master MR !11168, authored and merged by John Thacker, moves TShark's pipe/dissection checks into the live-capture path and after statistics taps have been started and `must_do_dissection()` has computed the actual requirement. The previous ordering checked tap requirements too early, so a tap that was registered later could make TShark dissect while the capture child and another consumer were both using the pipe, leading to nonsensical reads. The accepted change also delays `write_preamble()` until after all invalid option combinations have been rejected, avoiding partial output from a command that then exits with an option error. Merged !11167 independently moves the corresponding debug message until after `do_dissection` has actually been derived, so diagnostics report final state rather than an uninitialized/obsolete phase value.

**Phase-ordering rule:** parse options first, initialize the facilities that contribute to derived execution requirements, compute the final operating mode, and only then validate combinations whose legality depends on that mode. Do not substitute an early approximation of dynamic state for the value the execution path will actually use.

**Side-effect rule:** reject invalid combinations before emitting output preambles, creating externally visible partial results, or otherwise committing side effects that make an invalid invocation look partly successful.

**Diagnostic rule:** log derived mode decisions only after they have been computed from all relevant inputs; debug output should describe the state that governs execution.

**Confidence:** Very high. Both accepted master changes were authored and merged by John Thacker, and !11168 documents concrete misbehavior caused by the earlier phase ordering.